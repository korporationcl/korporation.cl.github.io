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

COST 2: How do you monitor usage and cost?
Establish policies and procedures to monitor and appropriately allocate your costs. This allows you to measure and improve the cost efficiency of this workload.

COST 3: How do you decommission resources?
Implement change control and resource management from project inception to end-of-life. This ensures you shut down or terminate unused resources to reduce waste.
```

## Cost-Effective Resources

```
COST 4: How do you evaluate cost when you select services?
Amazon EC2, Amazon EBS, and Amazon S3 are building-block AWS services. Managed services, such as Amazon RDS and Amazon DynamoDB, are higher level, or application level, AWS
services. By selecting the appropriate building blocks and managed services, you can optimize this workload for cost. For example, using managed services, you can reduce or remove much of your administrative and operational overhead, freeing you to work on applications and business-related activities.

COST 5: How do you meet cost targets when you select resource type and size?
Ensure that you choose the appropriate resource size for the task at hand. By selecting the most cost effective type and size, you minimize waste.

COST 6: How do you use pricing models to reduce cost?
Use the pricing model that is most appropriate for your resources to minimize expense.

COST 7: How do you plan for data transfer charges?
Ensure that you plan and monitor data transfer charges so that you can make architectural decisions to minimize costs. A small yet effective architectural change can drastically reduce your operational costs over time.
```

## Matching supply and demand

```
COST 8: How do you match supply of resources with demand?
For a workload that has balanced spend and performance, ensure that everything you pay for is used and avoid significantly underutilizing instances. A skewed utilization metric in either direction has an adverse impact on your organization, in either operational costs (degraded performance due to over-utilization), or wasted AWS expenditures (due to over-provisioning).
```

## Optimizing Over Time

```
COST 9: How do you evaluate new services?
As AWS releases new services and features, it is a best practice to review your existing architectural decisions to ensure they continue to be the most cost effective.
```

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