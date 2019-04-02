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

## Basic API to interact with ElasticSearch

- PUT
- GET
- POST
- HEAD
- DELETE

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