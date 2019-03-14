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

REL 2: How do you manage your network topology?
Applications can exist in one or more environments: your existing data center infrastructure, publicly accessible public cloud infrastructure, or private addressed public cloud infrastructure. Network considerations such as intra- and inter-system connectivity, public IP address
management, private address management, and name resolution are fundamental to using resources in the cloud.

## Change management

REL 3: How does your system adapt to changes in demand?
A scalable system provides elasticity to add and remove resources automatically so that they closely match the current demand at any given point in time.

REL 4: How do you monitor your resources?
Logs and metrics are a powerful tool to gain insight into the health of your workloads. You can configure your workload to monitor logs and metrics and send notifications when thresholds are crossed or significant events occur. Ideally, when low-performance thresholds are crossed
or failures occur, the workload has been architected to automatically self-heal or scale in response.

REL 5: How do you implement change?
Uncontrolled changes to your environment make it difficult to predict the effect of a change. Controlled changes to provisioned resources and workloads are necessary to ensure that the workloads and the operating environment are running known software and can be patched or
replaced in a predictable manner.

## Failure management

REL 6: How do you back up data?
Back up data, applications, and operating environments (defined as operating systems configured with applications) to meet requirements for mean time to recovery (MTTR) and recovery point objectives (RPO).

REL 7: How does your system withstand component failures?
If your workloads have a requirement, implicit or explicit, for high availability and low mean time to recovery (MTTR), architect your workloads for resilience and distribute your workloads to withstand outages.

REL 8: How do you test resilience?
Test the resilience of your workload to help you find latent bugs that only surface in production. Exercise these tests regularly.

REL 9: How do you plan for disaster recovery?
Disaster recovery (DR) is critical should restoration of data be required from backup methods. Your definition of and execution on the objectives, resources, locations, and functions of this data must align with RTO and RPO objectives.

## Key AWS Services

- The AWS service that is essential to Reliability is Amazon CloudWatch
- foundations: IAM, VPC, Trusted advisor, Shield
- Change management: Cloudtrail, Config, Autoscaling, CloudWatch
- Failure management: CloudFormation, S2, Glacier, KMS
