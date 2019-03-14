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
