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



# Basic API to interact with ElasticSearch

- PUT
- GET
- POST
- HEAD
- DELETE

## Refresh - Near Realtime Search

With the development of per-segment search, the delay between indexing a document and making it visible to search dropped dramatically. New documents could be made searchable within minutes, but that still isn’t fast enough.

The bottleneck is the disk. Commiting a new segment to disk requires an fsync to ensure that the segment is physically written to disk and that data will not be lost if there is a power failure. But an fsync is costly; it cannot be performed every time a document is indexed without a big performance hit.

What was needed was a more lightweight way to make new documents visible to search, which meant removing fsync from the equation.

Sitting between Elasticsearch and the disk is the filesystem cache. As before, documents in the in-memory indexing buffer, Figure “A Lucene index with new documents in the in-memory buffer”) are written to a new segment:

![A Lucene index with new documents in buffer](/assets/elas_1104.png)

Figure “The buffer contents have been written to a segment, which is searchable, but is not yet commited”). But the new segment is **written to the filesystem cache first—which** is cheap—and only later is it flushed to disk—which is expensive. But once a file is in the cache, it can be opened and read, just like any other file.

![The buffer have been written to segment](/assets/elas_1105.png)

Lucene allows new segments to be written and opened—making the documents they contain **visible to search—without performing a full commit**. This is a much lighter process than a commit, and can be done frequently without ruining performance.

## Refresh API

In Elasticsearch, this lightweight process of writing and opening a new segment is called a refresh. By default, every shard is refreshed automatically once every second. This is why we say that Elasticsearch has near real-time search: document changes are not visible to search immediately, but will become visible within 1 second.

You can adjust it executing:

```
$ curl -XPOST 'localhost:9200/myindex/_settings?index.refresh_interval=30s'
```

This would set the interval to 30 seconds.


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