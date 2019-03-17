# Performance Efficiency 
The Performance Efficiency pillar includes the ability to use computing resources efficiently to meet system requirements, and to maintain that efficiency as demand
changes and technologies evolve.

5 design principles are:

- Democratize advanced technologies: Technologies that are difficult to implement can become easier to consume by pushing that knowledge and complexity into
the cloud vendor's domain. Rather than having your IT team learn how to host and run a new technology, they can simply consume it as a service. For example, NoSQL databases, media transcoding,and machine learning are all technologies that require expertise that is not evenly dispersed across the technical community.
In the cloud, these technologies become services that your team can consume while focusing on product development rather than resource provisioning and management.

- Go global in minutes: Easily deploy your system in multiple Regions around the world with just a few clicks. This allows you to provide lower latency and a better
experience for your customers at minimal cost.

- Use serverless architectures: In the cloud, serverless architectures remove the need for you to run and maintain servers to carry out traditional compute activities. For
example, storage services can act as static websites, removing the need for web servers, and event services can host your code for you. This not only removes the operational burden of managing these servers, but also can lower transactional costs because these managed services operate at cloud scale.

- Experiment more often: With virtual and automatable resources, you can quickly carry out comparative testing using different types of instances, storage, or configurations.

- Mechanical sympathy: Use the technology approach that aligns best to what you are trying to achieve. For example, consider data access patterns when selecting database or storage approaches.

4 best practices:

1. Selection
2. Review
3. Monitoring
4. Tradeoffs

## Selection

PERF 1: How do you select the best performing architecture?
Often, multiple approaches are required to get optimal performance across a workload. Well-architected systems use multiple solutions and enable different features to improve
performance.

Your architecture will likely combine a number of different architectural approaches (for example, event-driven, ETL, or pipeline). The implementation of your architecture
will use the AWS services that are specific to the optimization of your architecture’s performance. In the following sections we look at the four main resource types that
you should consider (compute, storage, database, and network).

### Compute

In AWS, compute is available in three forms: instances, containers, and functions:

- Instances are virtualized servers and, therefore, you can change their capabilities with the click of a button or an API call. Because in the cloud resource decisions
are no longer fixed, you can experiment with different server types. At AWS, these virtual server instances come in different families and sizes, and they offer a wide
variety of capabilities, including solid-state drives (SSDs) and graphics processing units (GPUs).

- Containers are a method of operating system virtualization that allow you to run an application and its dependencies in resource-isolated processes.

- Functions abstract the execution environment from the code you want to execute. For example, AWS Lambda allows you to execute code without running an instance.


PERF 2: How do you select your compute solution?
The optimal compute solution for a system varies based on application design, usage patterns, and configuration settings. Architectures may use different compute solutions for various
components and enable different features to improve performance. Selecting the wrong compute solution for an architecture can lead to lower performance efficiency.

## Storage

The optimal storage solution for a particular system will vary based on the kind of access method (block, file, or object), patterns of access (random or sequential), throughput required, frequency of access (online, offline, archival), frequency of update (WORM, dynamic), and availability and durability constraints. Well-architected
systems use multiple storage solutions and enable different features to improve performance.

In AWS, storage is virtualized and is available in a number of different types. This makes it easier to match your storage methods more closely with your needs, and also
offers storage options that are not easily achievable with on- premises infrastructure. For example, Amazon S3 is designed for 11 nines of durability. You can also change
from using magnetic hard disk drives (HDDs) to SSDs, and easily move virtual drives from one instance to another in seconds.

```
PERF 3: How do you select your storage solution?
The optimal storage solution for a system varies based on the kind of access method (block, file, or object), patterns of access (random or sequential), required throughput, frequency of access (online, offline, archival), frequency of update (WORM, dynamic), and availability and durability constraints. Well-architected systems use multiple storage solutions and enable different features to improve performance and use resources efficiently.
```

## Database

Amazon RDS provides a fully managed relational database. With Amazon RDS, you can scale your database's compute and storage resources, often with no downtime.
Amazon DynamoDB is a fully managed NoSQL database that provides single-digit millisecond latency at any scale. Amazon Redshift is a managed petabyte-scale
data warehouse that allows you to change the number or type of nodes as your performance or capacity needs change.

```
PERF 4: How do you select your database solution?
The optimal database solution for a system varies based on requirements for availability, consistency, partition tolerance, latency, durability, scalability, and query capability. Many systems use different database solutions for various sub-systems and enable different features to improve performance. Selecting the wrong database solution and features for a system can lead to lower performance efficiency.
```

## Network

In AWS, networking is virtualized and is available in a number of different types and configurations. This makes it easier to match your networking methods more closely
with your needs. AWS offers product features (for example, Enhanced Networking, Amazon EBS-optimized instances, Amazon S3 transfer acceleration, dynamic Amazon
CloudFront) to optimize network traffic. AWS also offers networking features (for example, Amazon Route 53 latency routing, Amazon VPC endpoints, and AWS Direct
Connect) to reduce network distance or jitter.

```
PERF 5: How do you configure your networking solution?
The optimal network solution for a system varies based on latency, throughput requirements, and so on. Physical constraints such as user or on-premises resources drive location options, which can be offset using edge techniques or resource placement.
```

## Review

```
PERF 6: How do you evolve your workload to take advantage of new releases? When architecting workloads, there are finite options that you can choose from. However, over
time, new technologies and approaches become available that could improve the performance of your workload.
```

## Monitoring

After you have implemented your architecture you will need to monitor its performance so that you can remediate any issues before your customers are aware.
Monitoring metrics should be used to raise alarms when thresholds are breached. The alarm can trigger automated action to work around any badly performing
components.

```
PERF 7: How do you monitor your resources to ensure they are performing as expected?
System performance can degrade over time. Monitor system performance to identify this degradation and remediate internal or external factors, such as the operating system or
application load.
```

## Tradeoffs

When you architect solutions, think about tradeoffs so you can select an optimal approach. Depending on your situation you could trade consistency, durability, and
space versus time or latency to deliver higher performance

```
PERF 8: How do you use tradeoffs to improve performance?
When architecting solutions, actively considering tradeoffs enables you to select an optimal approach. Often you can improve performance by trading consistency, durability, and space for time and latency.
```

## Key AWS Services

- AWS CloudWatch is the essential to performance efficiency
- Selection:
  - *Compute*: Autoscaling
  - *Storage*: EBS (SSD, PIOS), S3 (S3 transfer accelaration)
  - *Database*: RDS (replicas, PIOS), DynamoDB (1ms latency)
  - *Network*: Route53, VPC (direct connect, endpoints)
- Review: AWS blog and whats new section on website to follow latest news and releases
- Monitoring: Cloudwatch (alarms, notifications) and integration with Lambda
- Tradeoffs: AWS ElastiCache, AWS Cloudfront, AWS Snowball. RDS replicas can help to scale heavy read-only workloads. 

## Additional resources

Documentation
- Amazon S3 Performance Optimization
- Amazon EBS Volume Performance

Whitepaper
- Performance Efficiency Pillar

Video
- AWS re:Invent 2016: Scaling Up to Your First 10 Million Users (ARC201)
- AWS re:Invent 2017: Deep Dive on Amazon EC2 Instances