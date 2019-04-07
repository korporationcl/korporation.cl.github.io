# What's an index?
- An index is where the documents or data is store in ElasticSearch (you search for documents and documents can be indexed and the results go in the "index")
- A document is searchable after is "indexed". It usually happens after 1 second.
- An index is composed by a shard, which is an instance of Lucene.
- Each node can hold one or more indices or shards.
- When you create an index is associated to only one shard. That's why you can't change the size of the shards once the index is created.
- Replica's size can change dynamically at any time (scaling out or in)

# What's a shard?

- A shard is where your index are.
- Indices are partitioned into shards so that they can be distributed among other nodes.
- Every shard is a separate instance of Apache Lucene.
- Every index can be sharded
  - Default number of shards is `5`
  - You can only specify the number of shards for an index during the creation of the index. Once set there is no way to change it.
- Shards can be `Primary` or `Replica`
  - Primaries are the ones where the `write` operations are performed
  - Replicas are where the `read` operations happen. When a document gets updated you get a new copy of the document, instead of updating the document on the replica.
  - An index must have at least `1` Primary shard. Replicas can be set to `0`, although if you have an outage your data might be gone.

# What's Lucene?

## Key points
- Lucene is an open source project and is where your data rest.
- Lucene creates an inverted index of your data and it's data structure is called a "segment".
- The invertex index once is written to disk is "immutable", which means it doesn't change once is created, ever.
- Lucene creates and merge segments at will (small ones are merged into a bigger one)
- A commit point is a file which lists all the known segments (ready for search)


A segment is an inverted index in its own right, but now the word index in Lucene came to mean a collection of segments plus a commit point—a file that lists all known segments

![Lucene index with commit point of 3 segments](/assets/elas_1101.png)


A per-segment search works as follows:

1. New documents are collected in an in-memory indexing buffer. See “A Lucene index with new documents in the in-memory buffer, ready to commit”.
2. Every so often, the buffer is commited:
  - A new segment—a supplementary inverted index—is written to disk.
  - A new commit point is written to disk, which includes the name of the new segment.
  - The disk is fsync’ed—all writes waiting in the filesystem cache are flushed to disk, to ensure that they have been physically written.
3. The new segment is opened, making the documents it contains visible to search.
4. The in-memory buffer is cleared, and is ready to accept new documents.

New documents are added first to **in-memory buffer indexing buffer**:

![A Lucene index with new documents in memory, ready to commit](/assets/elas_1102.png)

After a commit, a new segment is added to the commit point and the buffer is cleared:

![After a commit, a new segment is added to the commit point and the buffer is cleared](/assets/elas_1103.png)

When a query is issued, all known segments are queried in turn. Term statistics are aggregated across all segments to ensure that the relevance of each term and each document is calculated accurately. In this way, new documents can be added to the index relatively cheaply.

## Deletes and Updates (documents)
Segments are immutable, so documents cannot be removed from older segments, nor can older segments be updated to reflect a newer version of a document. Instead, every commit point includes a .del file that lists which documents in which segments have been deleted.

When a document is “deleted,” it is actually just marked as deleted in the .del file. A document that has been marked as deleted can still match a query, but it is removed from the results list before the final query results are returned.

Document updates work in a similar way: when a document is updated, the old version of the document is marked as deleted, and the new version of the document is indexed in a new segment. Perhaps both versions of the document will match a query, but the older deleted version is removed before the query results are returned.

# Near Realtime Search

With the development of per-segment search, the delay between indexing a document and making it visible to search dropped dramatically. New documents could be made searchable within minutes, but that still isn’t fast enough.

The bottleneck is the disk. Commiting a new segment to disk requires an fsync to ensure that the segment is physically written to disk and that data will not be lost if there is a power failure. But an fsync is costly; it cannot be performed every time a document is indexed without a big performance hit.

What was needed was a more lightweight way to make new documents visible to search, which meant removing fsync from the equation.

Sitting between Elasticsearch and the disk is the filesystem cache. As before, documents in the in-memory indexing buffer, Figure “A Lucene index with new documents in the in-memory buffer”) are written to a new segment:

![A Lucene index with new documents in buffer](/assets/elas_1104.png)

Figure “The buffer contents have been written to a segment, which is searchable, but is not yet commited”). But the new segment is **written to the filesystem cache first—which** is cheap—and only later is it flushed to disk—which is expensive. But once a file is in the cache, it can be opened and read, just like any other file.

![The buffer have been written to segment](/assets/elas_1105.png)

Lucene allows new segments to be written and opened—making the documents they contain **visible to search—without performing a full commit**. This is a much lighter process than a commit, and can be done frequently without ruining performance.

- https://www.elastic.co/guide/en/elasticsearch/guide/current/near-real-time.html#near-real-time

# Refresh API

In Elasticsearch, this lightweight process of writing and opening a new segment is called a refresh. By default, every shard is refreshed automatically once every second. This is why we say that Elasticsearch has near real-time search: document changes are not visible to search immediately, but will become visible within 1 second.

You can adjust it executing:

```
$ curl -XPOST 'localhost:9200/myindex/_settings?index.refresh_interval=30s'
```

This would set the interval to 30 seconds. This can be confusing for new users: they index a document and try to search for it, and it just isn’t there. The way around this is to perform a manual refresh, with the refresh API:

```
POST /_refresh (1)
POST /blogs/_refresh (2)
```


1. Refresh all indices.
2. Refresh just the blogs index.

# Tranlog (Transaction logs)

Without an fsync to flush data in the filesystem cache to disk, we cannot be sure that the data will still be there after a power failure, or even after exiting the application normally. For Elasticsearch to be reliable, it needs to ensure that changes are persisted to disk.

In Dynamically Updatable Indices, we said that a full commit flushes segments to disk and writes a commit point, which lists all known segments. Elasticsearch uses this commit point during startup or when reopening an index to decide which segments belong to the current shard.

While we refresh once every second to achieve near real-time search, we still need to do full commits regularly to make sure that we can recover from failure. But what about the document changes that happen between commits? We don’t want to lose those either.

Elasticsearch added a translog, or transaction log, which records every operation in Elasticsearch as it happens. With the translog, the process now looks like this:

1) When a document is indexed, it is added to the in-memory buffer and appended to the translog, as shown in Figure “New documents are added to the in-memory buffer and appended to the transaction log”:

![New documents added to in-memory and appended to translog](/assets/elas_1106.png)

2) The refresh leaves the shard in the state depicted in Figure “After a refresh, the buffer is cleared but the transaction log is not”. Once every second, the shard is refreshed:

The docs in the in-memory buffer are written to a new segment, without an fsync.
The segment is opened to make it visible to search.
The in-memory buffer is cleared.

![After a refresh, the buffer is cleared but transaction log is not](/assets/elas_1107.png)


3) This process continues with more documents being added to the in-memory buffer and appended to the transaction log (see Figure “The transaction log keeps accumulating documents”):

![The transaction log keeps accumulating documents](/assets/elas_1108.png)

4) Every so often—such as when the translog is getting too big, **the index is flushed; a new translog is created, and a full commit is performed** (see Figure “After a flush, the segments are fully commited and the transaction log is cleared”):

- Any docs in the in-memory buffer are written to a new segment.
- The buffer is cleared.
- A commit point is written to disk.
- The filesystem cache is flushed with an fsync.
- The old translog is deleted.

The translog provides a persistent record of all operations that have not yet been flushed to disk. When starting up, Elasticsearch will use the last commit point to recover known segments from disk, and will then replay all operations in the translog to add the changes that happened after the last commit.

The translog is also used to provide real-time CRUD. When you try to retrieve, update, or delete a document by ID, it first checks the translog for any recent changes before trying to retrieve the document from the relevant segment. This means that it always has access to the latest known version of the document, in real-time.

![After a flush, the segments are fully commited and the transaction log is cleared](/assets/elas_1109.png)

## Flush API

The action of performing a commit and truncating the translog is known in Elasticsearch as a flush. Shards are flushed automatically every 30 minutes, or when the translog becomes too big. See the translog documentation for settings that can be used to control these thresholds:

The flush API can be used to perform a manual flush:

```
POST /blogs/_flush (1)
POST /_flush?wait_for_ongoing (2)
```

[1] Flush the blogs index.

[2] Flush all indices and wait until all flushes have completed before returning.

- https://www.elastic.co/guide/en/elasticsearch/guide/current/translog.html

## Segment Merging

With the automatic refresh process creating a new segment every second, it doesn’t take long for the number of segments to explode. Having too many segments is a problem. Each segment consumes file handles, memory, and CPU cycles. More important, **every search request has to check every segment in turn**; the more segments there are, the slower the search will be.

Elasticsearch solves this problem by merging segments in the background. Small segments are merged into bigger segments, which, in turn, are merged into even bigger segments.

**This is the moment when those old deleted documents are purged from the filesystem. Deleted documents (or old versions of updated documents) are not copied over to the new bigger segment.**

There is nothing you need to do to enable merging. It happens automatically while you are indexing and searching. The process works like as depicted in Figure “Two commited segments and one uncommited segment in the process of being merged into a bigger segment”:

1) While indexing, the refresh process creates new segments and opens them for search.

2) The merge process selects a few segments of similar size and merges them into a new bigger segment in the background. This does not interrupt indexing and searching.

![Two commited segments and one uncommited segment in the process of being merged into a bigger segment](/assets/elas_1110.png)

3) Figure “Once merging has finished, the old segments are deleted” illustrates activity as the merge completes:

- The new segment is flushed to disk.
- A new commit point is written that includes the new segment and excludes the old, smaller segments.
- The new segment is opened for search.
- The old segments are deleted.

![Once merging has finished, the old segments are deleted](/assets/elas_1111.png)

The merging of big segments can use a lot of I/O and CPU, which can hurt search performance if left unchecked. By default, Elasticsearch throttles the merge process so that search still has enough resources available to perform well.

- https://www.elastic.co/guide/en/elasticsearch/guide/current/merge-process.html

## Optimize API

The optimize API is best described as the forced merge API. It forces a shard to be merged down to the number of segments specified in the max_num_segments parameter. The intention is to reduce the number of segments (usually to one) in order to speed up search performance.

In certain specific circumstances, the optimize API can be beneficial. The typical use case is for logging, where logs are stored in an index per day, week, or month. Older indices are essentially read-only; they are unlikely to change.

In this case, it can be useful to optimize the shards of an old index down to a single segment each; it will use fewer resources and searches will be quicker:

```
POST /logstash-2014-10/_optimize?max_num_segments=1
```

---

# What happens when you create an index on a cluster?

![example with a 3-node cluster](/assets/elas_0402.png)

Here is the sequence of steps necessary to successfully create, index, or delete a document on both the primary and any replica shards:

1. The client sends a create, index, or delete request to Node 1.

2. The node uses the document’s _id to determine that the document belongs to shard 0. It forwards the request to Node 3, where the primary copy of shard 0 is currently allocated.

3. Node 3 executes the request on the primary shard. If it is successful, it forwards the request in parallel to the replica shards on Node 1 and Node 2. Once all of the replica shards report success, Node 3 reports success to the coordinating node, which reports success to the client.

By the time the client receives a successful response, the document change has been executed on the primary shard and on all replica shards. Your change is safe.

# What happens when you retrieve a document?

![example retrieving a document](/assets/elas_0403.png)

Here is the sequence of steps to retrieve a document from either a primary or replica shard:

1. The client sends a get request to Node 1.

2. The node uses the document’s _id to determine that the document belongs to shard 0. Copies of shard 0 exist on all three nodes. On this occasion, it forwards the request to Node 2.

3. Node 2 returns the document to Node 1, which returns the document to the client.

For read requests, the coordinating node will choose **a different shard copy on every request** in order to balance the load; it **round-robins** through all shard copies.

It is possible that, while a document **is being indexed**, the document will already be present on the primary shard but **not yet copied to the replica shards**. In this case, a replica might report that the document doesn’t exist, while the primary would have returned the document successfully. Once the indexing request has returned success to the user,the document will be available on the primary and all replica shards.

# Partial updates to a document

![update document](/assets/elas_0404.png)

Here is the sequence of steps used to perform a partial update on a document:

1. The client sends an update request to Node 1.

2. It forwards the request to Node 3, where the primary shard is allocated.

3. Node 3 retrieves the document from the primary shard, changes the JSON in the _source field, and tries to reindex the document on the primary shard. If the document has already been changed by another process, it retries step 3 up to retry_on_conflict times, before giving up.

4. If Node 3 has managed to update the document successfully, it forwards the new version of the document in parallel to the replica shards on Node 1 and Node 2 to be reindexed. Once all replica shards report success, Node 3 reports success to the coordinating node, which reports success to the client.
The update API also accepts the routing, consistency, and timeout parameters that are explained in Creating, Indexing, and Deleting a Document.


>> Document-Based Replication
>>
>> When a primary shard forwards changes to its replica shards, it doesn’t forward the update request. Instead it forwards the new version of the full document. Remember that
>> these changes are forwarded to the replica shards **asynchronously**, and there is no guarantee that they will arrive in the same order that they were sent. If Elasticsearch
>> forwarded just the change, it is possible that changes would be applied in the wrong order, resulting in a corrupt document.

# Multidocument patterns (get)

The patterns for the mget and bulk APIs are similar to those for individual documents. The difference is that the coordinating node knows in which shard each document lives. It breaks up the multidocument request into a multidocument request per shard, and forwards these in parallel to each participating node.

Once it receives answers from each node, it collates their responses into a single response, which it returns to the client, as shown in Figure 12, “Retrieving multiple documents with mget”.

![Retrieving multiple documents with mget](/assets/elas_0405.png)

Here is the sequence of steps necessary to retrieve multiple documents with a single mget request:

1. The client sends an mget request to Node 1.
2. Node 1 builds a multi-get request per shard, and forwards these requests in parallel to the nodes hosting each required primary or replica shard. Once all replies have been received, Node 1 builds the response and returns it to the client.

A routing parameter can be set for each document in the docs array.

The bulk API, as depicted in Figure 13, “Multiple document changes with bulk”, allows the execution of multiple create, index, delete, and update requests within a single bulk request.

# Multiple document changes with bulk

![Multipe update with bulk](/assets/elas_0406.png)

The sequence of steps followed by the bulk API are as follows:

1. The client sends a bulk request to Node 1.

2. Node 1 builds a bulk request per shard, and forwards these requests in parallel to the nodes hosting each involved primary shard.

3. The primary shard executes each action serially, one after another. As each action succeeds, the primary forwards the new document (or deletion) to its replica shards in
parallel, and then moves on to the next action. Once all replica shards report success for all actions, the node reports success to the coordinating node, which collates the responses and returns them to the client.

The bulk API also accepts the consistency parameter at the top level for the whole bulk request, and the routing parameter in the metadata for each request.