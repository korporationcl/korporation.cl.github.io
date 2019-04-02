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

## Default ports

- HTTP API `TCP/9200` and `TCP/9300`.
- Internal node communication happens between `TCP/9300` and `TCP/9400`

More information about ports can be [find here](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/modules-transport.html).

# Node Discovery

- For AWS we use the [EC2 plugin](https://www.elastic.co/guide/en/elasticsearch/plugins/6.7/discovery-ec2.html)

# Slow queries

- Similar to the slow queries in MySQL we can register queries that are not performing well.


```
PUT /my_index/_settings
{
index.search.slowlog.threshold.query.warn: 10s
index.search.slowlog.threshold.query.info: 5s
index.search.slowlog.threshold.query.debug: 2s
index.search.slowlog.threshold.query.trace: 500ms

index.search.slowlog.threshold.fetch.warn: 1s
index.search.slowlog.threshold.fetch.info: 800ms
index.search.slowlog.threshold.fetch.debug: 500ms
index.search.slowlog.threshold.fetch.trace: 200ms

index.search.slowlog.level: info
}
```

## Plugins

We can install plugins using the following command (6.7):

```
sudo bin/elasticsearch-plugin install [plugin_name]
```

older releases (2.x, 1.x):

```
bin/plugin install [plugin_name]
```

Further reference in the [official documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules-slowlog.html).

## References
  - [CMS vs G1 Collector](https://medium.com/naukri-engineering/garbage-collection-in-elasticsearch-and-the-g1gc-16b79a447181)