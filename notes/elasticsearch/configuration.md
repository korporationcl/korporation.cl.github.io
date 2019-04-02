# ElasticSearch Configuration

## Node
- file paths, labels, network interfaces, etc.

## Index
- Number of shards, Replicas, Refresh rates, read only, etc

## Cluster
- Logging Levels, index templates, shard allocation and so forth.
- Lifetime options
  - **persistent**: they will remain even after a full cluster restart
  - **transient**: they will stay until the a full cluster restart

```
# This will not survive after a restart
curl -XPUT 'http://localhost:9200/_cluster/settings' -d '
{
 "transient" : {
 "logger.discovery" : "DEBUG"
 }
}'
```

## Paths

- Where to find the configuration(`path.config`)
- Where to find the logs (`path.logs`)
- Where to find the plugins (`path.plugins`)
- where the data is stored (`path.data`)

## Java

- ES_HEAP_SIZE
  - Never use more than 1/2 of your total RAM
  - Never allocate over 32GB RAM (do not use a EC2 instance that supports it)
  - The rest of the memory available will be use to persist the data on disk by ES calling `fsync`
  - Do not allow JVM to swap
    - turn off your swap or set `bootstrap.mlockall`
  - Evaluate your GarbageCollector (`CMS` is the default one but this might have changed to `G1`)