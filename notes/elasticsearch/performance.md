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


# JVM

## Garbage Collection Primer

Before we describe the stats, it is useful to give a crash course in garbage collection and its impact on Elasticsearch. If you are familar with garbage collection in the JVM, feel free to skip down.

Java is a garbage-collected language, which means that the programmer does not manually manage memory allocation and deallocation. The programmer simply writes code, and the Java Virtual Machine (JVM) manages the process of allocating memory as needed, and then later cleaning up that memory when no longer needed.

When memory is allocated to a JVM process, it is allocated in a big chunk called the heap. The JVM then breaks the heap into two groups, referred to as generations:

Young (or Eden)
The space where newly instantiated objects are allocated. The young generation space is often quite small, usually 100 MB–500 MB. The young-gen also contains two survivor spaces.
Old
The space where older objects are stored. These objects are expected to be long-lived and persist for a long time. The old-gen is often much larger than the young-gen, and Elasticsearch nodes can see old-gens as large as 30 GB.
When an object is instantiated, it is placed into young-gen. When the young generation space is full, a young-gen garbage collection (GC) is started. Objects that are still "alive" are moved into one of the survivor spaces, and "dead" objects are removed. If an object has survived several young-gen GCs, it will be "tenured" into the old generation.

A similar process happens in the old generation: when the space becomes full, a garbage collection is started and dead objects are removed.

Nothing comes for free, however. Both the young- and old-generation garbage collectors have phases that "stop the world." During this time, the JVM literally halts execution of the program so it can trace the object graph and collect dead objects. During this stop-the-world phase, nothing happens. Requests are not serviced, pings are not responded to, shards are not relocated. The world quite literally stops.

This isn’t a big deal for the young generation; its small size means GCs execute quickly. But the old-gen is quite a bit larger, and a slow GC here could mean 1s or even 15s of pausing—which is unacceptable for server software.

The garbage collectors in the JVM are very sophisticated algorithms and do a great job minimizing pauses. And Elasticsearch tries very hard to be garbage-collection friendly, by intelligently reusing objects internally, reusing network buffers, and enabling Doc Values by default. But ultimately, GC frequency and duration is a metric that needs to be watched by you, since it is the number one culprit for cluster instability.

A cluster that is frequently experiencing long GC will be a cluster that is under heavy load with not enough memory. These long GCs will make nodes drop off the cluster for brief periods. This instability causes shards to relocate frequently as Elasticsearch tries to keep the cluster balanced and enough replicas available. This in turn increases network traffic and disk I/O, all while your cluster is attempting to service the normal indexing and query load.

In short, long GCs are bad and need to be minimized as much as possible.

- https://www.elastic.co/guide/en/elasticsearch/guide/current/_monitoring_individual_nodes.html#garbage_collector_primer

---
Never exceed the 32GB RAM limit. Set the HEAP to use less half(or less) of your total RAM.

- https://www.elastic.co/guide/en/elasticsearch/guide/current/heap-sizing.html