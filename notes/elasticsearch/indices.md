# What's an index?
- An index is where the documents or data is store in ElasticSearch. 
- An index is composed by a shard, which is an instance of Lucene. 
- When you create an index is associated to only one shard. That's why you can't change the size of the shards once the index is created.
- Replica's size can change dynamically at any time (scaling out or in)

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