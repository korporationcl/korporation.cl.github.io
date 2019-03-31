# General Performance

## Segments and Merging
Segment merging is computationally expensive, and can eat up a lot of disk I/O. Merges are scheduled to operate in the background because they can take a long time to finish, especially large segments. This is normally fine, because the rate of large segment merges is relatively rare.

But sometimes merging falls behind the ingestion rate. If this happens, Elasticsearch will automatically throttle indexing requests to a single thread. This prevents a segment explosion problem, in which hundreds of segments are generated before they can be merged. Elasticsearch will log INFO-level messages stating now throttling indexing when it detects merging falling behind indexing.

Elasticsearch defaults here are conservative: you don’t want search performance to be impacted by background merging. But sometimes (especially on SSD, or logging scenarios), the throttle limit is too low.

The default is 20 MB/s, which is a good setting for spinning disks. If you have SSDs, you might consider increasing this to 100–200 MB/s. Test to see what works for your system:

```
PUT /_cluster/settings
{
    "persistent" : {
        "indices.store.throttle.max_bytes_per_sec" : "100mb"
    }
}
```

- https://www.elastic.co/guide/en/elasticsearch/guide/current/indexing-performance.html#segments-and-merging

## Indices Recovery (fast)

Peer recovery is the process used to build a new copy of a shard on a node by copying data from the primary. Elasticsearch uses this peer recovery process to rebuild shard copies that were lost if a node has failed, and uses the same process when migrating a shard copy between nodes to rebalance the cluster or to honor any changes to the shard allocation settings.

The following expert setting can be set to manage the resources consumed by peer recoveries:

```
indices.recovery.max_bytes_per_sec

Limits the total inbound and outbound peer recovery traffic on each node. Since this limit applies on each node, but there may be many nodes performing peer recoveries concurrently, the total amount of peer recovery traffic within a cluster may be much higher than this limit. If you set this limit too high then there is a risk that ongoing peer recoveries will consume an excess of bandwidth (or other resources) which could destabilize the cluster. Defaults to 40mb.
```

Set this to a high value, depending of the EC2 instance we use.

- https://www.elastic.co/guide/en/elasticsearch/reference/current/recovery.html


## JAVA HEAP

Never exceed the 32GB RAM limit. Set the HEAP to use less half(or less) of your total RAM.

- https://www.elastic.co/guide/en/elasticsearch/guide/current/heap-sizing.html