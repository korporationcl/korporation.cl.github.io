# Cost Optimization

The Cost Optimization pillar includes the ability to run systems to deliver business value at the lowest price point.

Design Principles

- Adopt a consumption model: Pay only for the computing resources that you require and increase or decrease usage depending on business requirements, not by
using elaborate forecasting. For example, development and test environments are typically only used for eight hours a day during the work week. You can stop these
resources when they are not in use for a potential cost savings of 75% (40 hours versus 168 hours).

- Measure overall efficiency: Measure the business output of the workload and the costs associated with delivering it. Use this measure to know the gains you make
from increasing output and reducing costs.

- Stop spending money on data center operations: AWS does the heavy lifting of racking, stacking, and powering servers, so you can focus on your customers and
organization projects rather than on IT infrastructure.

- Analyze and attribute expenditure: The cloud makes it easier to accurately identify the usage and cost of systems, which then allows transparent attribution of IT costs
to individual workload owners. This helps measure return on investment (ROI) and gives workload owners an opportunity to optimize their resources and reduce costs.

- Use managed and application level services to reduce cost of ownership: In the cloud, managed and application level services remove the operational burden of
maintaining servers for tasks such as sending email or managing databases. As managed services operate at cloud scale, they can offer a lower cost per transaction
or service.

There are four best practice areas for cost optimization in the cloud:

1. Expenditure Awareness
2. Cost-Effective Resources
3. Matching supply and demand
4. Optimizing Over Time

## Expenditure Awareness


```
COST 1: How do you govern usage?
Establish policies and mechanisms to ensure that appropriate costs are incurred while objectives are achieved. By employing a checks-and-balances approach, you can innovate
without overspending.
```

Best practices:
- Develop policies based on your organization requirements: Develop policies that define how resources are managed by your organization. Policies should cover cost aspects of
resources and workloads, including creation, modification and decommission over the resource lifetime. Also develop cost targets and goals for workloads.

- Implement an account structure: Implement a structure of accounts that maps to your organization. This assists in allocating and managing costs throughout your organization.

• Implement groups and roles: Implement groups and roles that align to your policies and control who can create, modify, or decommission instances and resources in each group; for example, development, test, and production groups. This applies to AWS services and third-party solutions.

- Implement cost controls: Implement controls based on organization policies and defined groups and roles. These ensure that costs are only incurred as defined by organization
requirements; for example, control access to regions or resource types with IAM policies.

- Track project lifecycle: Track, measure, and audit the lifecycle of projects, teams, and environments to avoid using and paying for unnecessary resources.



```
COST 2: How do you monitor usage and cost?
Establish policies and procedures to monitor and appropriately allocate your costs. This allows you to measure and improve the cost efficiency of this workload.
```

Best practices:
- Configure AWS Cost and Usage Report: Configure the AWS Cost and Usage Report to capture detailed usage and billing information.

- Identify cost attribution categories: Identify organization categories that could be used to allocate cost within your organization.

- Establish organization metrics: Establish the organization metrics that are required for this workload. Example metrics of a workload are customer reports produced or web pages
served to customers.

- Define and implement tagging: Define a tagging schema based on organization, and workload attributes, and cost allocation categories. Implement tagging across all
resources.

- Configure billing and cost management tools: Configure AWS Cost Explorer and AWS Budgets inline with your organization policies.

- Report and notify on cost optimization: Configure AWS Budgets to provide notifications on cost and usage against targets. Have regular meetings to analyze this workload's cost
efficiency and to promote cost aware culture.

- Monitor cost proactively: Implement tooling and dashboards to monitor cost proactively for this workload; do not just look at costs and categories when you receive notifications.

- This helps to identify positive trends and promote them throughout your organization.

- Allocate costs based on workload metrics: Allocate this workload's costs by metrics or business outcomes to measure workload cost efficiency. Implement a process to analyze
the AWS Cost and Usage Report with Amazon Athena, which can provide insight and charge back capability.


```
COST 3: How do you decommission resources?
Implement change control and resource management from project inception to end-of-life. This ensures you shut down or terminate unused resources to reduce waste.
```

Best practices:
- Track resources over their life time: Define and implement a method to track resources and their associations with systems, over their life time. You can use tagging to identify the workload or function of the resource.

- Implement a decommissioning process: Implement a process to identify and decommission orphaned resources.

- Decommission resources in an unplanned manner: Decommission resources on an unplanned basis. This is typically triggered by events such as periodic audits and is usually
performed manually.

- Decommission resources automatically: Design your workload to gracefully handle resource termination as you identify and decommission non-critical resources, resources
that are not required, or resources with low utilization.

## Cost-Effective Resources

```
COST 4: How do you evaluate cost when you select services?
Amazon EC2, Amazon EBS, and Amazon S3 are building-block AWS services. Managed services, such as Amazon RDS and Amazon DynamoDB, are higher level, or application level, AWS
services. By selecting the appropriate building blocks and managed services, you can optimize this workload for cost. For example, using managed services, you can reduce or remove much of your administrative and operational overhead, freeing you to work on applications and business-related activities.
```

Best practices:
- Identify organization requirements for cost: Work with team members to define the balance between cost optimization and other pillars, such as performance and reliability,
for this workload.

- Analyze all components of this workload: Ensure every workload component is analyzed, regardless of current size or current costs. Review effort should reflect potential benefit, such as current and projected costs.

- Perform a thorough analysis of each component: Look at overall cost to the organization of each component. Look at total cost of ownership by factoring in cost of operations
and management, especially when using managed services. Review effort should reflect potential benefit; for example, time spent analyzing is proportional to component cost.

- Select components of this workload to optimize cost inline with organization priorities: Factor in cost when selecting all components. This includes using application level and
managed services such as Amazon RDS, Amazon DynamoDB, Amazon SNS, and Amazon SES to reduce overall organization cost. Use serverless and containers for compute, such
as AWS Lambda, Amazon S3 for static websites, and Amazon ECS. Minimize license costs by using open source software, or software that does not have license fees. For example
Amazon Linux for compute workloads, or migrate databases to Amazon Aurora.

- Perform cost analysis for different usage over time: Workloads can change over time, and some services or features are more cost effective at different usage levels. By
performing the analysis on each component over time and at projected usage, you ensure this workload remains cost effective over its lifetime.


```
COST 5: How do you meet cost targets when you select resource type and size?
Ensure that you choose the appropriate resource size for the task at hand. By selecting the most cost effective type and size, you minimize waste.
```

Best practices:
- Perform cost modeling: Identify organization requirements and perform cost modeling of the workload and each of its components. Perform benchmark activities for the workload
under different predicted loads and compare the costs. The modeling effort should reflect potential benefit; for example, time spent is proportional to component cost.

- Select resource type and size based on estimates: Estimate resource size or type based on workload and resource characteristics; for example, compute, memory, throughput, or
write intensive. This estimate is typically made using a previous version of the workload (such as an on-premises version), using documentation, or using other sources of
information about the workload.

- Select resource type and size based on metrics: Use metrics from the currently running workload to select the right size and type to optimize for cost. Appropriately provision
throughput, sizing, and storage for services such as Amazon EC2, Amazon DynamoDB, Amazon EBS (PIOPS), Amazon RDS, Amazon EMR, and networking. This can be done with a
feedback loop such as automatic scaling, or by a manual process of re-sizing.

```
COST 6: How do you use pricing models to reduce cost?
Use the pricing model that is most appropriate for your resources to minimize expense.
```

Best practices:
- Perform pricing model analysis: Perform an analysis on the workload using the Reserved Instance Recommendations feature in AWS Cost Explorer.

- Implement different pricing models, with low coverage: Implement reserved capacity, Spot Instances, Spot Blocks or Spot Fleet, in the workload but with low coverage, at less
than 80 percent of overall recommendations.

- Implement pricing models for all components of this workload: Permanently running resources have high coverage with reserved capacity, with at least 80 percent of
recommendations implemented. Short term capacity is configured to use Spot Instances, Spot Blocks or Spot Fleet. On demand is only used for short-term workloads that cannot
be interrupted, and do not run long enough for reserved capacity: typically 25 to 75 percent of the year, depending on the resource type.

- Implement regions based on cost: Resource pricing can be different in each region. Factoring in region cost ensures you pay the lowest overall price for this workload.


```
COST 7: How do you plan for data transfer charges?
Ensure that you plan and monitor data transfer charges so that you can make architectural decisions to minimize costs. A small yet effective architectural change can drastically reduce your operational costs over time.
```

Best practices:
- Perform data transfer modeling: Gather organization requirements and perform data transfer modeling of the workload and each of its components. This identifies the lowest
cost point for its current data transfer requirements.

- Select components to optimize data transfer cost: All components are selected, and architecture is designed to reduce data transfer costs. This includes using components such
as WAN optimization and Multi-AZ configurations.

- Implement services to reduce data transfer costs: Implement services to reduce data transfer; for example, using a CDN such as Amazon CloudFront to deliver content to end
users, caching layers using Amazon ElastiCache, or using AWS Direct Connect instead of VPN for connectivity to AWS.

## Matching supply and demand

```
COST 8: How do you match supply of resources with demand?
For a workload that has balanced spend and performance, ensure that everything you pay for is used and avoid significantly underutilizing instances. A skewed utilization metric in either direction has an adverse impact on your organization, in either operational costs (degraded performance due to over-utilization), or wasted AWS expenditures (due to over-provisioning).
```

Best practices:
- Perform an analysis on the workload demand: Analyze the demand of the workload over time. Ensure the analysis covers seasonal trends and accurately represents operating
conditions over the full workload lifetime. Analysis effort should reflect potential benefit; for example, time spent is proportional to the workload cost.

- Provision resources reactively or unplanned: Resource levels change due to demand, however provisioning is in an unplanned manner, typically manually, and triggered by
adverse events or changes in the workload. Resourcing is slow to change, and typically results in over or under provisioning.

- Provision resources dynamically: Resources are provisioned in a planned manner. This can be demand-based, such as through automatic scaling; buffer-based, where demand
is spread over time with lower overall resourcing used; or time-based, where demand is predictable and resources are provided based on time. These methods result in the least
amount of over or under provisioning.


## Optimizing Over Time

```
COST 9: How do you evaluate new services?
As AWS releases new services and features, it is a best practice to review your existing architectural decisions to ensure they continue to be the most cost effective.
```

Best practices:
- Establish a cost optimization function: Create a team that regularly reviews cost and usage across the organization.

- Develop a workload review process: Develop a process that defines the criteria and process for workload review. The review effort should reflect potential benefit; for
example, core workloads or workloads with a value of over 10% of the bill are reviewed quarterly, while workloads below 10% are reviewed annually.

- Review and implement services in an unplanned way: Adopt new services in an unplanned way.

- Review and analyze this workload regularly: Existing workloads are regularly reviewed as per defined processes.

- Keep up to date with new service releases: Consult regularly with experts or APN Partners to consider which services and features provide lower cost. Review AWS blogs and
other information sources.


# Key AWS Services

The tool that is essential to Cost Optimization is **Cost Explorer**, which helps you gain visibility and insights into your usage, across your workloads and throughout
your organization. The following services and features support the four areas in cost optimization:

- Expenditure Awareness: AWS Cost Explorer allows you to view and track your usage in detail. AWS Budgets notify you if your usage or spend exceeds actual or
forecast budgeted amounts.

- Cost-Effective Resources: You can use Cost Explorer for Reserved Instance recommendations, and see patterns in how much you spend on AWS resources over time. Use Amazon CloudWatch and Trusted Advisor to help right size your resources. You can use Amazon Aurora on RDS to remove database licensing costs. AWS Direct Connect and Amazon CloudFront can be used to optimize data transfer.

- Matching supply and demand: Auto Scaling allows you to add or remove resources to match demand without overspending.

- Optimizing Over Time: The AWS News Blog and the What's New section on the AWS website are resources for learning about newly launched features and services.
AWS Trusted Advisor inspects your AWS environment and finds opportunities to save you money by eliminating unused or idle resources or committing to Reserved
Instance capacity.


## Additional Resources

Documentation
- [Analyzing Your Costs with Cost Explorer](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-explorer-what-is.html)
- [AWS Cloud Economics Center](https://aws.amazon.com/economics/)
- [AWS Detailed Billing Reports](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-reports-costusage.html)

Whitepaper
- [Cost Optimization Pillar](https://d0.awsstatic.com/whitepapers/architecture/AWS-Cost-Optimization-Pillar.pdf)

Video
- [Cost Optimization on AWS](https://www.youtube.com/watch?v=XQFweGjK_-o)

Tool
- [AWS Total Cost of Ownership (TCO) Calculators](http://aws.amazon.com/tco-calculator)
- [AWS Simple Monthly Calculator](http://calculator.s3.amazonaws.com/index.html)