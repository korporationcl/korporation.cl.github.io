The pillars of the AWS Well-Architected Framework (5 items)

## Operational Excelence
The ability to monitor and run systems to deliver business value and to continually improve supporting processes and procedures.

## Security
The ability to protect information, systems and assets while delivering business value through risk assessments and mitigations
strategies.

## Reliability
The ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet
demand, and mitigate disruptions such as misconfigurations or transient network issues.

## Performance Eficiency
The ability to use computing resources efficiently to meet system requirements, and to maintain that effiency as demand changes
and technologies evolve.

## Cost Optimisation
The ability to run systems to deliver business value at the lowest price point.


# General design Principles

- Stop guessing capacity needs: We can scale up and down at any time on AWS.
- Test systems at production scale: you can create production-scale test environment on demand, complete your testing and then decommission the infrastructure.
- Automate to make architectural experimentation easier: Automation allows to you replicate system at any time and low cost (manual effort is expensive)
- Allow for evolutionary architectures: Capability on the cloud is dynamic, not a static resource. 
- Drive architectures using data: Architecture as code, use that data to inform your choices.
- Improve through game days: Test your environments in production, introduce game days or Chaos monkey tasks. 


## Operational Excellence 

The Operational Excellence pillar includes the ability to run and monitor systems to deliver business value and to continually improve supporting 
processes and procedures.


The 6 design principles for operational excellence in the cloud are:

- Perform operations as code: you can define your whole workload (application, infrastructure) as code. By performing automating operations you limit
the human error and enable consistent responses to events.

- Annotate documentation: In an on-premises system documentation is manually made. Automate the creation of documentation  since can be used by
persons and systems.

- Make frequent, small, reversible changes: Design workloads that allow components to be updated frequently. Making small changes are easy to rollback in
case they fail.

- Refine operations procedures frequently: Validate and review your procedures during game days or chaos monkey activities. 

- Anticipate failure: Perform "pre-mortem" exercises to identity potential sources of failures. Set game days to perform this activity.

- Learn from all operational failures: Share what is learned from all operational events and failures ("post-mortems").


## Definition

There are three best practice areas for operational excellence in the cloud:

1. Prepare
2. Operate
3. Evolve


### Prepare

Effective preparation is required to drive operational excellence. Design workloads with mecanism to be monitored. Procedures should be captured
in playbooks and runbooks. Practice responses in supported environments through game days. Using CloudFormation you can automate the infrastructure to have
consistent, templates of your environment (sandbox for example).

CloudWatch can be used to monitor system resources, use CloudTrail to monitor the API calls and VPC FlowLogs to get network metrics. 

The following questions focus on these considerations for operational excellence.
(For a list of operational excellence questions, answers, and best practices, see the
Appendix.)

OPS 1: How do you determine what your priorities are?
Everyone needs to understand their part in enabling business success. Have shared goals in order to set priorities for resources. 
This will maximize the benefits of your efforts.

OPS 2: How do you design your workload so that you can understand its state?

Design your workload so that it provides the information necessary for you to understand its internal state (for example, metrics, logs, and traces) 
across all components. This enables you to provide effective responses when appropriate.

OPS 3: How do you reduce defects, ease remediation, and improve flow into production?
Adopt approaches that improve flow of changes into production, that enable refactoring, fast feedback on quality, and bug fixing. 
These accelerate beneficial changes entering production, limit issues deployed, and enable rapid identification and remediation of issues introduced
through deployment activities.

OPS 4: How do you mitigate deployment risks?
Adopt approaches that provide fast feedback on quality and enable rapid recovery from changes that do not have desired outcomes. 
Using these practices mitigates the impact of issues introduced through the deployment of changes.

OPS 5: How do you know that you are ready to support a workload?
Evaluate the operational readiness of your workload, processes and procedures, and personnel to understand the operational risks related to your workload.


### Operate

- Use runbooks to handle planned and unplanned events. Prioritise responses to events based on the business impact. 
- Ensure if there is an alert raised, there is a proceess associated to be executed.
- Determine the root cause of unplanned events. Use this information to update your procedures to mitigate problems in the future.
- AWS X-Ray, CloudWatch, CloudTrail and VPC Flog logs enable the identification of workload issues in support of root cause analysis and remediation.
- Communicate status of the operations through Dashboards

OPS 6: How do you understand the health of your workload?
Define, capture, and analyze workload metrics to gain visibility to workload events so that you can take appropriate action.

OPS 7: How do you understand the health of your operations?
Define, capture, and analyze operations metrics to gain visibility to operations events so that you can take appropriate action.

OPS 8: How do you manage workload and operations events?
Prepare and validate procedures for responding to events to minimize their disruption to your workload.

- Manual processes for deployment, release management, changes and rollback should be voided (all the things should be automated).

### Evolve
With AWS Developer tools you can implement continous delivery build, test and deployment activities. The results of deployments can be used 
to identity opportunities for improvement. You can perform analytics on your metrics data integrating data from operations and deployment
activities to enable analysis of the impact of those activities. This data can be share in cross-team retrospective analysis to identity
opportunities and methods for improvements.

OPS 9: How do you evolve operations?
Dedicate time and resources for continuous incremental improvement to evolve the effectiveness and efficiency of your operations.

### Key AWS Services

- Key service for getting operational excelence is CloudFormation, which is use to create templates for your infrastructure. Other services
that support the 3 areas of operational excellence:

- Prepare: AWS Config and AWS Config rules can be used to create standards for workloads.
- Operate: Amazon CloudWatch allows to monitor the health of your workload
- Evolve: Amazon ElasticSearch service (Amazon ES) allows you to analyze your log data to gain actionable insights quickly and securely.