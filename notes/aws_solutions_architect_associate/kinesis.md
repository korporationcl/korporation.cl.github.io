# AWS Kinesis

- Kinesis is a platform to send your streaming data to AWS. It's easy to load and analyse and you can also build your own custom applications.
- 3 types of Kinesis:
  - Kinesis Streams: Producers stream data to Kinesis
    - Data is contained inside "Shards".
    - 5 transactions per second/reads up to 2MB/second and up to 1000 records/second(1MB/second) per writes.
    - by default data is stored for 24h. It can be stored up to 7 days.
    - Data capacity depends on the amount of Shards you have on the stream. The total capacity of the stream is the sum of the capacity of the      shards.
    - Consumers (EC2) connect to the shard and analyse the data
    - Once your data has been processed can be stored on other services (S3, RedShift, EMR, DynamoDB)
  - Kinesis Firehose:
    - Inside Firehose there is no persistent storage, you need to analyse your data with a lamba function or something.
    - Output from the analysis can be stored on S3, RedShift, ElasticSearch
  - Kinesis Analytics:
    - Can analyse data on the fly
    - Data can be stored on S3, RedShift, ElasticSearch.