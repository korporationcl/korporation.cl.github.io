# What's an index?
- An index is where the documents or data is store in ElasticSearch. 
- An index is composed by a shard, which is an instance of Lucene. 
- When you create an index is associated to only one shard. That's why you can't change the size of the shards once the index is created.
- Replica's size can change dynamically at any time (scaling out or in)

# What happens when you create an index on a cluster?

![example with a 3-node cluster](/assets/elas_0402.png)

Here is the sequence of steps necessary to successfully create, index, or delete a document on both the primary and any replica shards:

- The client sends a create, index, or delete request to Node 1.

- The node uses the document’s _id to determine that the document belongs to shard 0. It forwards the request to Node 3, where the primary copy of shard 0 is currently allocated.

- Node 3 executes the request on the primary shard. If it is successful, it forwards the request in parallel to the replica shards on Node 1 and Node 2. Once all of the replica shards report success, Node 3 reports success to the coordinating node, which reports success to the client.

By the time the client receives a successful response, the document change has been executed on the primary shard and on all replica shards. Your change is safe.

# What happens when you retrieve a document?

![example retrieving a document](/assets/elas_0403.png)