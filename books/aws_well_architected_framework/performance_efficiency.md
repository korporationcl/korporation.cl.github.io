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

Best practices:
- Understand the available services and resources: Learn about and understand the wide range of services and resources available to you on AWS. Identify which services and
configuration options are relevant to your workload and understand how to use them to achieve optimal performance.

- Define a process for architectural choices: Use existing internal experience and knowledge of AWS, or use external resources, such as published use cases, relevant
documentation, or whitepapers to define a process to choose resources and services. For example, a process that encourages experimentation and benchmarking different services
as they might be used in your workload.

- Factor cost or budget into decisions: Workloads often have budgets that they need to operate within, and the budget is a critical part of efficient operation. Use internal cost
controls and consider budget when selecting resource types and sizes based on predicted resource needs.

- Use policies or reference architectures: Use internal policies or existing reference architectures to make the best architectural choices for your workload. Evaluate which
services and configuration are best for your workload to maximize performance and efficiency.

- Use guidance from AWS or an APN Partner: Use AWS resources, such as Solutions Architects, or an APN Partner to guide your decisions. These resources can help review and
suggest improvements to your architecture to achieve an optimal level of performance.

- Benchmark existing workloads: Benchmark the performance of an existing workload to understand how it performs on AWS. Use the data collected from these benchmarks to
drive architectural decisions.

- Load test your workload: Deploy the latest version of your system on AWS using different resource types and sizes, and use monitoring to capture performance metrics that identify bottlenecks or excess capacity. Use this information when designing or improving your architecture and resource selection based on how it performs.

### Compute

In AWS, compute is available in three forms: instances, containers, and functions:

- Instances are virtualized servers and, therefore, you can change their capabilities with the click of a button or an API call. Because in the cloud resource decisions
are no longer fixed, you can experiment with different server types. At AWS, these virtual server instances come in different families and sizes, and they offer a wide
variety of capabilities, including solid-state drives (SSDs) and graphics processing units (GPUs).

- Containers are a method of operating system virtualization that allow you to run an application and its dependencies in resource-isolated processes.

- Functions abstract the execution environment from the code you want to execute. For example, AWS Lambda allows you to execute code without running an instance.


PERF 2: How do you select your compute solution?
The optimal compute solution for a system varies based on application design, usage patterns, and configuration settings. Architectures may use different compute solutions for various components and enable different features to improve performance. Selecting the wrong compute solution for an architecture can lead to lower performance efficiency.

Best practices:
- Evaluate the available compute options: Look at and understand the performance characteristics of the compute-related options available to you. Know how instances,
containers, and functions work and what advantages, or disadvantages, they bring to your workload.

- Understand the available compute configuration options: Understand how various options complement your workload and which configuration options are best for your
system. Examples of these options include instance family, sizes, features (GPU, I/O), function sizes, container instances, single versus multi-tenancy, and so on.

- Collect compute-related metrics: One of the best ways to understand how your systems are performing is to record and track the true utilization of various resources. This data can then be fed back into making more accurate determinations of resource requirements.

- Determine the required configuration by right-sizing: Analyze the various performance characteristics of your workload and how these characteristics relate to memory, network,
and CPU usage. Use this data when choosing resources that best match your workload's profile. For example, a memory-intensive workload, such as a database, could be served
best by the r-family of instances, while a bursting workload may benefit more from an elastic container system such as Amazon Elastic Container Service.

- Use the available elasticity of resources: AWS provides the flexibility to expand or reduce your resources dynamically through a variety of mechanisms (for example: AWS Auto
Scaling, Amazon Elastic Container Service, and AWS Lambda) to meet changes in demand. Combined with compute-related metrics, a workload can automatically respond to these
changes and utilize the optimal set of resources to achieve its goal.

- Re-evaluate compute needs based on metrics: Use system-level metrics to identify the behavior and requirements of your workload over time. Evaluate your workload's needs
by comparing the available resources with these requirements and make changes to your compute environment to best match your workload's profile. For example, over time a
system may be observed to be more memory-intensive than initially thought, so moving to a different instance family or size may improve both performance and efficiency

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

Best practices:
- Understand storage characteristics and requirements: Understand the different characteristics (for example, shareable, file size, cache size, access patterns, latency,
throughput, and persistence of data) that are required to select the services that best fit your workload, such as Amazon S3, Amazon EBS, Amazon Elastic File System (Amazon
EFS), and Amazon EC2 instance store.

- Evaluate available configuration options: Evaluate the various characteristics and configuration options and how they relate to storage. Understand where and how to use
PIOPS, SSDs, magnetic storage, Amazon S3, Amazon Glacier, or ephemeral storage to optimize storage space and performance for your workload.

- Make decisions based on access patterns and metrics: Choose storage systems and configure them by considering how the workload accesses data. Make performance
improvements, such as choosing caching services or instances that best match your access patterns, utilizing optimal key distributions when storing data in Amazon S3 or
DynamoDB, striping storage volumes, or partitioning data based on system measurements. Increase storage efficiency by choosing object storage, such as Amazon S3, or block
storage, such as Amazon Elastic Block Store. Configure the storage options you choose to match your data access patterns

## Database

Amazon RDS provides a fully managed relational database. With Amazon RDS, you can scale your database's compute and storage resources, often with no downtime.
Amazon DynamoDB is a fully managed NoSQL database that provides single-digit millisecond latency at any scale. Amazon Redshift is a managed petabyte-scale
data warehouse that allows you to change the number or type of nodes as your performance or capacity needs change.

```
PERF 4: How do you select your database solution?
The optimal database solution for a system varies based on requirements for availability, consistency, partition tolerance, latency, durability, scalability, and query capability. Many systems use different database solutions for various sub-systems and enable different features to improve performance. Selecting the wrong database solution and features for a system can lead to lower performance efficiency.
```

Best practices:
- Understand data characteristics: Understand the different characteristics of data in your workload. Determine if the workload requires transactions, how it interacts with data,
what its performance demands are, and so on. Use this data to select the best performing database approach for your workload (for example, relational databases, NoSQL, data
warehouses, or in-memory storage).

- Evaluate the available options: Evaluate the services and storage options that are available as part of the selection process for your workload's storage mechanisms.
Understand how, and when, to use a given service or system for data storage. Learn about available configuration options that can further optimize database performance or
efficiency, such as PIOPs, memory and compute resources, caching, and so on.

- Collect and record database performance metrics: Use tools, libraries, and systems that record performance measurements related to database performance. For example,
measure transactions per second, slow queries, or system latency introduced when accessing the database. Use this data to understand the performance of your database
systems.

- Choose data storage based on access patterns: Use the access patterns of the workload to decide which services and technologies to use. For example, utilize a relational
database for workloads that require transactions, or a key-value store that provides higher throughput but is eventually consistent where applicable.

- Optimize data storage based on access patterns and metrics: Use performance characteristics and access patterns that optimize how data is stored or queried to
achieve the best possible performance. Measure how optimizations such as indexing, key distribution, data warehouse design, or caching strategies affect system performance or
overall efficiency

## Network

In AWS, networking is virtualized and is available in a number of different types and configurations. This makes it easier to match your networking methods more closely
with your needs. AWS offers product features (for example, Enhanced Networking, Amazon EBS-optimized instances, Amazon S3 transfer acceleration, dynamic Amazon
CloudFront) to optimize network traffic. AWS also offers networking features (for example, Amazon Route 53 latency routing, Amazon VPC endpoints, and AWS Direct
Connect) to reduce network distance or jitter.

```
PERF 5: How do you configure your networking solution?
The optimal network solution for a system varies based on latency, throughput requirements, and so on. Physical constraints such as user or on-premises resources drive location options, which can be offset using edge techniques or resource placement.
```

Best practices:
- Understand how networking impacts performance: Analyze and understand how network-related decisions impact workload performance. For example, network latency
often impacts the user experience, and using the wrong protocols can starve network capacity through excessive overhead.

- Understand available product options: Understand the service-level features that are available to optimize network-related performance; for example, EC2 instance network
capability, enhanced networking, Amazon EBS-optimized instances, Amazon S3 Transfer Acceleration, and dynamic content delivery with Amazon CloudFront.

- Evaluate available networking features: Evaluate networking features in AWS that increase performance. Measure the impact of these features through testing, metrics,
and analysis. For example, take advantage of network-level features that are available (including Amazon Route 53 latency-based routing, Amazon VPC endpoints, or AWS Direct
Connect) to reduce latency, network distance, or jitter.

- Use minimal network ACLs: Design your network to minimize the number of ACLs while still meeting requirements. Having too many ACLs can negatively impact network
performance, reducing system performance or efficiency.

- Leverage encryption offloading and load-balancing: Use load balancing for offloading encryption termination (TLS) to improve performance and to manage and route traffic
effectively. Distribute traffic across multiple resources or services to allow your workload to take advantage of the elasticity that AWS provides.

- Choose network protocols to improve performance: When choosing protocols for communication between systems and networks, make decisions based on how those
protocols will impact workload performance.

- Choose location based on network requirements: Use the location options available (for example, AWS Region, Availability Zone, placement groups, and edge locations) to reduce
network latency or improve throughput.

- Optimize network configuration based on metrics: Use data that is collected and analyzed to make informed decisions about optimizing your network configuration.
Measure the impact of those changes and use the impact measurements to make future decisions.

## Review

```
PERF 6: How do you evolve your workload to take advantage of new releases? When architecting workloads, there are finite options that you can choose from. However, over
time, new technologies and approaches become available that could improve the performance of your workload.
```

Best practices:
- Keep up-to date on new resources and services: Evaluate ways to improve performance as new services, design patterns, or product offerings become available. Consider how
these could improve performance or increase the efficiency of the workload through adhoc evaluation, internal discussion, or external analysis.

- Define a process to improve workload performance: Define a process to evaluate new services, design patterns, resource types, and configurations as they become available. For
example, run existing performance tests on new instance offerings to determine which improvements in performance or efficiency would be gained by using them.

- Evolve workload performance over time: As an organization, use the information gathered through the evaluation process to actively drive adoption of new services or
resources when they are available to improve performance or make efficiency gains in your workload.

## Monitoring

After you have implemented your architecture you will need to monitor its performance so that you can remediate any issues before your customers are aware.
Monitoring metrics should be used to raise alarms when thresholds are breached. The alarm can trigger automated action to work around any badly performing
components.

```
PERF 7: How do you monitor your resources to ensure they are performing as expected?
System performance can degrade over time. Monitor system performance to identify this degradation and remediate internal or external factors, such as the operating system or
application load.
```

Best practices:
- Record performance-related metrics: Use Amazon CloudWatch, a third-party service, or self-managed monitoring tools to record performance-related metrics. For example, record
database transactions, slow queries, I/O latency, HTTP request throughput, service latency, or other key data.

- Analyze metrics when events or incidents occur: In response to (or during) an event or incident, use monitoring dashboards or reports to understand and diagnose the impact.
These views provide insight into which portions of the workload are not performing at expected levels.

- Establish KPIs to measure workload performance: Identify the KPIs that indicate whether the system is performing as intended. For example, an API-based workload may use overall
response latency as an indication of overall performance, and an e-commerce site may choose to use the number of purchases being made as its KPI.

- Use monitoring to generate alarm-based notifications: Using the performance-related KPIs that you defined, use a monitoring system that generates alarms automatically when
these measurements are outside expected boundaries.

- Review metrics at regular intervals: As routine maintenance or in response to events or incidents, review which metrics are collected. Use these reviews to identify which metrics were key in addressing issues and which additional metrics, if they were being tracked, would help to identify, address, or prevent issues.

- Monitor and alarm proactively: Use KPIs, combined with monitoring and alerting systems, to proactively address performance-related issues. Use alarms to trigger automated
actions to remediate issues where possible; escalate the alarm to those able to respond if automated response is not possible. For example, a system that can predict expected KPI
values and alarm when they breach certain thresholds, or a tool that can automatically halt or roll back deployments if KPIs are outside of expected values.


## Tradeoffs

When you architect solutions, think about tradeoffs so you can select an optimal approach. Depending on your situation you could trade consistency, durability, and
space versus time or latency to deliver higher performance

```
PERF 8: How do you use tradeoffs to improve performance?
When architecting solutions, actively considering tradeoffs enables you to select an optimal approach. Often you can improve performance by trading consistency, durability, and space for time and latency.
```

Best practices:
- Understand the areas where performance is most critical: Understand and identify areas where increasing the performance of your workload will have a positive impact on
efficiency or the customer experience. For example, a website that has a lot of customer interaction would benefit from using edge services such as Amazon CloudFront to move
content delivery closer to customers.

- Learn about design patterns and services: Research and understand the various design patterns and services that help improve workload performance. As part of the analysis,
identify what you may be trading to achieve higher performance. For example, using Amazon ElastiCache can help to reduce load placed on database systems; however, it
requires some engineering to implement safe caching, or possible introduction of eventual consistency in some areas.

- Identify how tradeoffs impact customers and efficiency: When evaluating performancerelated improvements, consider how those choices will impact customers and workload
efficiency. For example, if using key-value storage such as Amazon DynamoDB would significantly increase system performance, it is also important to evaluate how the
eventually consistent nature of Amazon DynamoDB could affect customers.

- Measure the impact of performance improvements: As changes are made to improve performance, evaluate metrics and data that were collected to determine the impact that
the performance improvement had on the workload, its components, and any customers. This measurement helps you understand the improvements that result from the tradeoff,
and helps you determine if any negative side-effects were introduced.

- Use various performance-related strategies: Where applicable, utilize a number of strategies to improve performance. For example, use strategies like caching data to
prevent excessive network or database calls, using read-replicas for database engines to improve read rates, sharding or compressing data where possible to reduce data volumes,
and buffering and streaming of results as they are available to avoid blocking.

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
- [Amazon S3 Performance Optimization](http://docs.aws.amazon.com/AmazonS3/latest/dev/PerformanceOptimization.html)
- [Amazon EBS Volume Performance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSPerformance.html)

Whitepaper
- [Performance Efficiency Pillar](https://d0.awsstatic.com/whitepapers/architecture/AWS-Performance-Efficiency-Pillar.pdf)

Video
- [AWS re:Invent 2016: Scaling Up to Your First 10 Million Users (ARC201)](https://www.youtube.com/watch?v=n28lDDdlnVg)
- [AWS re:Invent 2017: Deep Dive on Amazon EC2 Instances](https://www.youtube.com/watch?v=mZy6E2I5Rek)