# Reliability
The Reliability pillar includes the ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues.

## Design Principles

- Test recovery procedures: In an on-premises environment, testing is often conducted to prove the system works in a particular scenario. Testing is not typically used to validate recovery strategies. In the cloud, you can test how your system fails, and you can validate your recovery procedures. You can use automation to simulate different failures or to recreate scenarios that led to failures
before. This exposes failure pathways that you can test and rectify before a real failure scenario, reducing the risk of components failing that have not been tested before.

- Automatically recover from failure: By monitoring a system for key performance indicators (KPIs), you can trigger automation when a threshold is breached. This allows for automatic notification and tracking of failures, and for automated recovery processes that work around or repair the failure. With more sophisticated automation, it's possible to anticipate and remediate failures before they occur.

- Scale horizontally to increase aggregate system availability: Replace one large resource with multiple small resources to reduce the impact of a single failure on the overall system. Distribute requests across multiple, smaller resources to ensure that they don't share a common point of failure.

- Stop guessing capacity: A common cause of failure in on-premises systems is resource saturation, when the demands placed on a system exceed the capacity of that system (this is often the objective of denial of service attacks). In the cloud, you can monitor demand and system utilization, and automate the addition or removal of resources to maintain the optimal level to satisfy demand without overor under- provisioning.

- Manage change in automation: Changes to your infrastructure should be done using automation. The changes that need to be managed are changes to the automation.

There are three best practice areas for reliability in the cloud

1. Foundations
2. Change Management
3. Failure Management

## Foundations

The cloud is designed to be essentially limitless, so it is the responsibility of AWS to satisfy the requirement for sufficient networking and
compute capacity, while you are free to change resource size and allocation, such as the size of storage devices, on demand.

REL 1: How do you manage service limits?
Default service limits exist to prevent accidental provisioning of more resources than you need. There are also limits on how often you can call API operations to protect services from abuse. If you are using AWS Direct Connect, you have limits on the amount of data you can transfer on each connection. If you are using AWS Marketplace applications, you need to understand the limitations of the applications. If you are using third-party web services or software as a service, you also need to be aware of the limits of those services.

Best practices:
- Aware of limits but not tracking them: You are aware there are limits, but are not tracking your current limits.

- Monitor and manage limits: Evaluate your potential usage, increase your regional limits appropriately, and allow planned growth in usage.

- Use automated monitoring and management of limits: Implement tools to alert you when thresholds are being approached. As part of AWS Enterprise Support, you can also
automate the limit increase request.

- Accommodate fixed service limits through architecture: Be aware of unchangeable service limits and architect around these.

- Ensure a sufficient gap between the current service limit and the maximum usage to accommodate failover: When a resource fails it may still be counted against limits until
it is successfully terminated. Ensure your limits cover the overlap of all failed resources with replacements before the failed resources are terminated. You should consider an
Availability Zone failure when calculating this gap.

- Manage service limits across all relevant accounts and regions: If you are using multiple AWS accounts or AWS Regions, ensure you request the same limits in all environments in
which you run your production workloads.


REL 2: How do you manage your network topology?
Applications can exist in one or more environments: your existing data center infrastructure, publicly accessible public cloud infrastructure, or private addressed public cloud infrastructure. Network considerations such as intra- and inter-system connectivity, public IP address
management, private address management, and name resolution are fundamental to using resources in the cloud.

Best practices:
- Use highly available connectivity between private addresses in public clouds and onpremises environment: Use multiple AWS Direct Connect (DX) circuits and multiple VPN
tunnels between separately deployed private IP address spaces. Use multiple DX locations for high availability. If you use multiple AWS Regions, you will also need multiple DX
locations in at least 2 regions. You may want to evaluate AWS Marketplace appliances that terminate VPNs. If you use AWS Marketplace appliances, deploy redundant instances for
high availability in different Availability Zones.

- Use highly available network connectivity for the users of the workload: Use a highly available DNS, CloudFront, API Gateway, load balancing, and reverse proxy as the
public facing endpoint of your application. You may want to evaluate AWS Marketplace appliances for load balancing or proxying.

- Enforce non-overlapping private IP address ranges in multiple private address spaces where they are connected: The IP ranges of each of your VPCs must not conflict if they
are peered or connected via VPN. The same is true for private connectivity to your onpremises environments and other cloud providers. You must have a way to allocate private
IP ranges when needed.

- Ensure IP subnet allocation accounts for expansion and availability: Individual Amazon VPC IP address ranges must be large enough to accommodate an application's
requirements, including factoring in future expansion and allocation of IP addresses to subnets across Availability Zones. This includes load balancers, AWS Lambda functions,
EC2 instances, and container-based applications. Additionally, keep some IP addresses available for possible future expansion.

## Change management

REL 3: How does your system adapt to changes in demand?
A scalable system provides elasticity to add and remove resources automatically so that they closely match the current demand at any given point in time.

Best practices:
- Procure resources upon detection of lack of service within a workload: Scaling of resources is performed manually when availability is impacted.

- Procure resources manually upon detection that more resources may be needed soon for a workload: Manually scale compute and storage as need is anticipated.

- Procure resources automatically when scaling a workload up or down: Use services that automatically scale, such as Amazon S3, Amazon CloudFront, Amazon Auto Scaling, and
AWS Lambda. You can also use third-party tools and AWS SDKs to automate scaling.

- Load test the workload: Adopt a load testing methodology to measure if scaling activity will meet workload requirements.

REL 4: How do you monitor your resources?
Logs and metrics are a powerful tool to gain insight into the health of your workloads. You can configure your workload to monitor logs and metrics and send notifications when thresholds are crossed or significant events occur. Ideally, when low-performance thresholds are crossed
or failures occur, the workload has been architected to automatically self-heal or scale in response.

Best practices:
- Monitor the workload in all tiers: Monitor the tiers of the workload with Amazon CloudWatch or third-party tools. Monitor AWS services with Personal Health Dashboard.

- Send notifications based on the monitoring: Organizations that need to know receive notifications when significant events occur.

- Perform automated responses on events: Use automation to take action when an event is detected; for example, to replace failed components.

- Conduct reviews regularly: Frequently review the monitoring of the workload based on significant events and changes to evaluate the architecture and implementation.

REL 5: How do you implement change?
Uncontrolled changes to your environment make it difficult to predict the effect of a change. Controlled changes to provisioned resources and workloads are necessary to ensure that the workloads and the operating environment are running known software and can be patched or
replaced in a predictable manner.

Best practices:
- Deploy changes in a planned manner: Deployments and patching follow a documented process.
- Deploy changes with automation: Deployments and patching are automated.

## Failure management

REL 6: How do you back up data?
Back up data, applications, and operating environments (defined as operating systems configured with applications) to meet requirements for mean time to recovery (MTTR) and recovery point objectives (RPO).

Best practices:
- Identify all data that needs to be backed up and are perform backups or reproduce the data from sources: Back up important data using Amazon S3, Amazon EBS snapshots, or
third-party software. Alternatively, if the data can be reproduced from sources to meet RPO, you may not require a backup.

- Perform data backup automatically or reproduce the data from sources automatically: Automate backups or the reproduction from sources using AWS features (for example,
snapshots of Amazon RDS and Amazon EBS, versions on Amazon S3, etc.), AWS Marketplace solutions, or third-party solutions.

- Perform periodic recovery of the data to verify backup integrity and processes: Validate that your backup process implementation meets Recovery Time Objective and Recovery
Point Objective through a recovery test.

- Secure and encrypt backups or ensure the data is available from a secure source for reproduction: Detect access via authentication and authorization like AWS IAM, and detect
data integrity compromise by using encryption.

REL 7: How does your system withstand component failures?
If your workloads have a requirement, implicit or explicit, for high availability and low mean time to recovery (MTTR), architect your workloads for resilience and distribute your workloads to withstand outages.

Best practices:
- Monitor all layers of the workload to detect failures: Continuously monitor the health of your system and report degradation as well as complete failure.

- Implement loosely coupled dependencies: Dependencies such as queuing systems, streaming systems, workflows, and load balancers are loosely coupled.

- Implement graceful degradation to transform applicable hard dependencies into soft dependencies: When a component's dependencies are unhealthy, the component itself
does not report as unhealthy. It can continue to serve requests in a degraded manner.

- Automating complete recovery because technology constraints exist in parts or all of the workload requiring a single location: Elements of the workload can only run in one
Availability Zone or one data center, requiring you to implement a complete rebuild of the workload with defined recovery objectives.

- Deploy the workload to multiple locations: Distribute workload load across multiple Availability Zones and AWS Regions (for example, DNS, ELB, Application Load Balancer,
and API Gateway). These locations can be as diverse as needed.

- Automate healing on all layers: Use automated capabilities upon detection of failure to perform an action to remediate.

- Send notifications upon availability impacting events: Notifications are sent upon detection of significant events, even if the issue was automatically healed.

REL 8: How do you test resilience?
Test the resilience of your workload to help you find latent bugs that only surface in production. Exercise these tests regularly.

Best practices:
- Use playbooks for unanticipated failures: You have playbooks for failure scenarios that have not been anticipated to identify root causes and assist in strategies for prevention or mitigation.

- Conduct root cause analysis (RCA) and share results: Review system failures based on significant events to evaluate the architecture and identify the root cause. Have a method
to communicate these causes to others as needed.

- Inject failures to test resiliency: Test failures regularly, ensuring coverage of failure pathways.

- Conduct game days regularly: Use game days to regularly exercise your failure procedures with the people who will be involved in actual failure scenarios.

REL 9: How do you plan for disaster recovery?
Disaster recovery (DR) is critical should restoration of data be required from backup methods. Your definition of and execution on the objectives, resources, locations, and functions of this data must align with RTO and RPO objectives.

Best practices:
- Define recovery objectives for downtime and data loss: The workload has a recovery time objective (RTO) and recovery point objective (RPO).

- Use defined recovery strategies to meet the recovery objectives: A disaster recovery (DR) strategy has been defined to meet objectives.

- Test disaster recovery implementation to validate the implementation: Regularly test failover to DR to ensure that RTO and RPO are met.

- Manage configuration drift on all changes: Ensure that AMIs and the system configuration state are up-to-date at the DR site or region, as well as the limits on AWS
services.

- Automate recovery: Use AWS or third-party tools to automate system recovery.

## Key AWS Services

- The AWS service that is essential to Reliability is Amazon CloudWatch
- foundations: IAM, VPC, Trusted advisor, Shield
- Change management: Cloudtrail, Config, Autoscaling, CloudWatch
- Failure management: CloudFormation, S2, Glacier, KMS

## Additional resources

Refer to the following resources to learn more about our best practices for Reliability.

Documentation
- [Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)
- [Service Limits Reports Blog](http://aws.amazon.com/about-aws/whats-new/2014/06/19/amazon-ec2-service-limits-report-now-available/)
- [Amazon Virtual Private Cloud](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html)
- [AWS Shield](http://docs.aws.amazon.com/waf/latest/developerguide/shield-chapter.html)
- [Amazon CloudWatch](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)
- [Amazon S3](http://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html)
- [AWS KMS](http://docs.aws.amazon.com/kms/latest/developerguide/overview.html)

Whitepaper
- [Reliability Pillar](https://d0.awsstatic.com/whitepapers/architecture/AWS-Reliability-Pillar.pdf)
- [Backup Archive and Restore Approach Using AWS](http://d0.awsstatic.com/whitepapers/Backup_Archive_and_Restore_Approaches_Using_AWS.pdf)
- [Managing your AWS Infrastructure at Scale](http://d0.awsstatic.com/whitepapers/managing-your-aws-infrastructure-at-scale.pdf)
- [AWS Disaster Recovery](http://media.amazonwebservices.com/AWS_Disaster_Recovery.pdf)
- [AWS Amazon VPC Connectivity Options](http://media.amazonwebservices.com/AWS_Amazon_VPC_Connectivity_Options.pdf)

Video
- [How do I manage my AWS service limits?](https://aws.amazon.com/premiumsupport/knowledge-center/manage-service-limits/)
- [Embracing Failure: Fault-Injection and Service Reliability](https://www.youtube.com/watch?v=wrY7XoOnysg)

Product
- [AWS Premium Support](https://aws.amazon.com/premiumsupport/)
- [Trusted Advisor](https://aws.amazon.com/premiumsupport/trustedadvisor/)