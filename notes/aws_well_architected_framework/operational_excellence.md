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

Best practices:
- Evaluate external customer needs: Involve key stakeholders, including business, development, and operations teams, to determine where to focus operations efforts on
external customer needs. This will ensure that you have a thorough understanding of the operations support that is required to achieve business outcomes.

- Evaluate internal customer needs: Involve key stakeholders, including business, development, and operations teams, when determining where to focus operations efforts
on internal customer needs. This will ensure that you have a thorough understanding of the operations support that is required to achieve business outcomes.

- Evaluate compliance requirements: Evaluate external factors, such as regulatory compliance requirements and industry standards, to ensure that you are aware of
guidelines or obligations that may mandate or emphasize specific focus. If no compliance requirements are identified, ensure that you apply due diligence to this determination.

- Evaluate threat landscape: Evaluate threats to the business (for example, competition, business risk and liabilities, operational risks, and information security threats), so that you can include their impact when determining where to focus operations efforts.

- Evaluate tradeoffs: Evaluate the impact of tradeoffs between competing interests, to help make informed decisions when determining where to focus operations efforts. For
example, accelerating speed to market for new features may be emphasized over cost optimization.

- Manage benefits and risks: Manage benefits and risks to make informed decisions when determining where to focus operations efforts. For example, it may be beneficial to deploy
a system with unresolved issues so that significant new features can be made available to customers.

OPS 2: How do you design your workload so that you can understand its state?

Design your workload so that it provides the information necessary for you to understand its internal state (for example, metrics, logs, and traces) 
across all components. This enables you to provide effective responses when appropriate.

Best practices:
- Implement application telemetry: Instrument your application code to emit information about its internal state, status, and achievement of business outcomes. For example, queue
depth, error messages, and response times. Use this information to determine when a response is required.

- Implement and configure workload telemetry: Design and configure your workload to emit information about its internal state and current status. For example, API call volume,
http status codes, and scaling events. Use this information to help determine when a response is required.

- Implement user activity telemetry: Instrument your application code to emit information about user activity. For example, click streams, or started, abandoned, and completed
transactions. Use this information to help understand how the application is used, patterns of usage, and to determine when a response is required.

- Implement dependency telemetry: Design and configure your workload to emit information about the status of resources it depends on. Examples of these are external
databases, DNS, and network connectivity. Use this information to determine when a response is required.

- Implement transaction traceability: Implement your application code and configure your workload components to emit information about the flow of transactions across the
workload. Use this information to determine when a response is required and to assist in identifying the root cause of issues.

OPS 3: How do you reduce defects, ease remediation, and improve flow into production?
Adopt approaches that improve flow of changes into production, that enable refactoring, fast feedback on quality, and bug fixing. 
These accelerate beneficial changes entering production, limit issues deployed, and enable rapid identification and remediation of issues introduced
through deployment activities.


Best practices:
- Use version control: Use version control to enable tracking of changes and releases.

- Test and validate changes: Test and validate changes to help limit and detect errors. Automate testing to reduce errors caused by manual processes, and reduce the level of
effort to test.

- Use configuration management systems: Use configuration management systems to make and track configuration changes. These systems reduce errors caused by manual
processes and reduce the level of effort to deploy changes.

- Use build and deployment management systems: Use build and deployment management systems. These systems reduce errors caused by manual processes and
reduce the level of effort to deploy changes.

- Perform patch management: Perform patch management to gain features, address issues, and remain compliant with governance. Automate patch management to reduce
errors caused by manual processes, and reduce the level of effort to patch.

- Share design standards: Share best practices across teams to increase awareness and maximize the benefits of development efforts.

- Implement practices to improve code quality: Implement practices to improve code quality and minimize defects. For example, test-driven development, code reviews, and
standards adoption.

- Use multiple environments: Use multiple environments to experiment, develop, and test your workload. Use increasing levels of controls to gain confidence your workload will
operate as intended.

- Make frequent, small, reversible changes: Frequent, small, and reversible changes reduce the scope and impact of a change. This eases troubleshooting, enables faster remediation, and provides the option to roll back a change.

- Fully automate integration and deployment: Automate build, deployment, and testing of the workload. This reduces errors caused by manual processes and reduces the effort to
deploy changes.

OPS 4: How do you mitigate deployment risks?
Adopt approaches that provide fast feedback on quality and enable rapid recovery from changes that do not have desired outcomes. 
Using these practices mitigates the impact of issues introduced through the deployment of changes.


Best practices:
- Plan for unsuccessful changes: Plan to revert to a known good state, or remediate in the production environment if a change does not have the desired outcome. This preparation
reduces recovery time through faster responses.

- Test and validate changes: Test changes and validate the results at all lifecycle stages, to confirm new features and minimize the risk and impact of failed deployments.

- Use deployment management systems: Use deployment management systems to track and implement change. This reduces errors cause by manual processes and reduces the
effort to deploy changes.

- Test using limited deployments: Test with limited deployments alongside existing systems to confirm desired outcomes prior to full scale deployment. For example, use
deployment canary testing or one-box deployments.

- Deploy using parallel environments: Implement changes onto parallel environments, and then transition to the new environment. Maintain the prior environment until there
is confirmation of successful deployment. Doing so minimizes recovery time by enabling rollback to the previous environment.

- Deploy frequent, small, reversible changes: Use frequent, small, and reversible changes to reduce the scope of a change. This results in easier troubleshooting and faster
remediation with the option to roll back a change.

- Fully automate integration and deployment: Automate build, deployment, and testing of the workload. This reduces errors cause by manual processes and reduces the effort to
deploy changes.

- Automate testing and rollback: Automate testing of deployed environments to confirm desired outcomes. Automate rollback to previous known good state when outcomes are
not achieved to minimize recovery time and reduce errors caused by manual processes.

OPS 5: How do you know that you are ready to support a workload?
Evaluate the operational readiness of your workload, processes and procedures, and personnel to understand the operational risks related to your workload.

Best practices:
- Ensure personnel capability: Have a mechanism to validate that you have an appropriate number of trained personnel to provide support for operational needs. Train personnel
and adjust personnel capacity as necessary to maintain effective support.

- Ensure consistent review of operational readiness: Ensure you have a consistent review of your readiness to operate a workload. Review must include at a minimum
the operational readiness of the teams and the workload, and security considerations. Implement review activities in code and trigger automated review in response to events
where appropriate, to ensure consistency, speed of execution, and reduce errors caused by manual processes.

- Use runbooks to perform procedures: Runbooks are documented procedures to achieve specific outcomes. Enable consistent and prompt responses to well-understood events
by documenting procedures in runbooks. Implment runbooks as code and trigger the execution of runbooks in response to events where appropriate, to ensure consistency,
speed responses, and reduce errors caused by manual processes.

- Use playbooks to identify issues: Playbooks are documented processes to investigate issues. Enable consistent and prompt responses to failure scenarios by documenting
investigation processes in playbooks. Implement playbooks as code and trigger playbook execution in response to events where appropriate, to ensure consistency, speed
responses, and reduce errors caused by manual processes.

- Make informed decisions to deploy systems and changes: Evaluate the capabilities of the team to support the workload and the workload's compliance with governance. Evaluate
these against the benefits of deployment when determining whether to transition a system or change into production. Understand the benefits and risks to make informed
decisions.

### Operate

- Use runbooks to handle planned and unplanned events. Prioritise responses to events based on the business impact. 
- Ensure if there is an alert raised, there is a proceess associated to be executed.
- Determine the root cause of unplanned events. Use this information to update your procedures to mitigate problems in the future.
- AWS X-Ray, CloudWatch, CloudTrail and VPC Flog logs enable the identification of workload issues in support of root cause analysis and remediation.
- Communicate status of the operations through Dashboards

OPS 6: How do you understand the health of your workload?
Define, capture, and analyze workload metrics to gain visibility to workload events so that you can take appropriate action.

Best practices:
- Identify key performance indicators: Identify key performance indicators (KPIs) based on desired business and customer outcomes. Evaluate KPIs to determine workload success.

- Define workload metrics: Define workload metrics to measure the achievement of KPIs. Define workload metrics to measure the health of the workload. Evaluate metrics to
determine if the workload is achieving desired outcomes, and to understand the health of the workload.

- Collect and analyze workload metrics: Perform regular proactive reviews of metrics to
identify trends and determine where appropriate responses are needed.

- Establish workload metrics baselines: Establish baselines for metrics to provide expected values as the basis for comparison and identification of under and over performing
components.
- Learn expected patterns of activity for workload: Establish patterns of workload activity to determine when behavior is outside of the expected values so that you can respond
appropriately if required.

- Alert when workload outcomes are at risk: Raise an alert when workload outcomes are at risk so that you can respond appropriately if required.

- Alert when workload anomalies are detected: Raise an alert when workload anomalies are detected so that you can respond appropriately if required.

- Validate the achievement of outcomes and the effectiveness of KPIs and metrics: Create a business-level view of your workload operations to help you determine if you
are satisfying needs and to identify areas that need improvement to reach business goals. Validate the effectiveness of KPIs and metrics and revise them if necessary.


OPS 7: How do you understand the health of your operations?
Define, capture, and analyze operations metrics to gain visibility to operations events so that you can take appropriate action.

Best practices:
- Identify key performance indicators: Identify key performance indicators (KPIs) based on desired business and customer outcomes. Evaluate KPIs to determine operations success.

- Define operations metrics: Define operations metrics to measure the achievement of KPIs. Define operations metrics to measure the health of the operations. Evaluate metrics to
determine if the operations are achieving desired outcomes, and to understand operations health.

- Collect and analyze operations metrics: Perform regular proactive reviews of metrics to identify trends and determine where appropriate responses are needed.

- Establish operations metrics baselines: Establish baselines for metrics to provide expected values as the basis for comparison and identification of under and over
performing processes.

- Learn the expected patterns of activity for operations: Establish baselines for metrics to provide expected values as the basis for comparison.

- Alert when operations outcomes are at risk: Raise an alert when operations outcomes are at risk so that you can respond appropriately if required.

- Alert when operations anomalies are detected: Raise an alert when operations anomalies are detected so that you can respond appropriately if required.

- Validate the achievement of outcomes and the effectiveness of KPIs and metrics: Create a business-level view of your operations activities to help you determine if you are
satisfying needs and to identify areas that need improvement to reach business goals. Validate the effectiveness of KPIs and metrics and revise them if necessary.

OPS 8: How do you manage workload and operations events?
Prepare and validate procedures for responding to events to minimize their disruption to your workload.

Best practices:
- Use processes for event, incident, and problem management: Have processes to address observed events, events that require intervention (incidents), and events that require
intervention and either recur or cannot currently be resolved (problems). Use these processes to mitigate the impact of these events on the business and your customers by
ensuring timely and appropriate responses.

- Use a process for root cause analysis: Have a process to identify and document the root cause of an event so that you can develop mitigations to limit or prevent recurrence and
you can develop procedures for prompt and effective responses. Communicate root cause as appropriate, tailored to target audiences.

- Have a process per alert: Have a well-defined response (runbook or playbook), with a specifically identified owner, for any event for which you raise an alert. This ensures
effective and prompt responses to operations events and prevents actionable events from being obscured by less valuable notifications.

- Prioritize operational events based on business impact: Ensure that when multiple events require intervention, those that are most significant to the business are addressed
first. For example, impacts can include loss of life or injury, financial loss, or damage to reputation or trust.

- Define escalation paths: Define escalation paths in your runbooks and playbooks, including what triggers escalation, and procedures for escalation. Specifically identify
owners for each action to ensure effective and prompt responses to operations events.

- Enable push notifications: Communicate directly with your users (for example, with email or SMS) when the services they use are impacted, and when the services return to normal
operating conditions, to enable users to take appropriate action.

- Communicate status through dashboards: Provide dashboards tailored to their target audiences (for example, internal technical teams, leadership, and customers) to
communicate the current operating status of the business and provide metrics of interest.

- Automate responses to events: Automate responses to events to reduce errors caused by manual processes, and to ensure prompt and consistent responses.


### Evolve
With AWS Developer tools you can implement continous delivery build, test and deployment activities. The results of deployments can be used 
to identity opportunities for improvement. You can perform analytics on your metrics data integrating data from operations and deployment
activities to enable analysis of the impact of those activities. This data can be share in cross-team retrospective analysis to identity
opportunities and methods for improvements.

OPS 9: How do you evolve operations?
Dedicate time and resources for continuous incremental improvement to evolve the effectiveness and efficiency of your operations.

Best practices:
- Have a process for continuous improvement: Regularly evaluate and prioritize opportunities for improvement to focus efforts where they can provide the greatest
benefits.

- Implement feedback loops: Include feedback loops in your procedures and workloads to help you identify issues and areas that need improvement.

- Define drivers for improvement: Identify drivers for improvement to help you evaluate and prioritize opportunities.

- Validate insights: Review your analysis results and responses with cross-functional teams and business owners. Use these reviews to establish common understanding, identify
additional impacts, and determine courses of action. Adjust responses as appropriate.

- Perform operations metrics reviews: Regularly perform retrospective analysis of operations metrics with cross-team participants from different areas of the business. Use
these reviews to identify opportunities for improvement, potential courses of action, and to share lessons learned.

- Document and share lessons learned: Document and share lessons learned from the execution of operations activities so that you can use them internally and across teams.

- Allocate time to make improvements: Dedicate time and resources within your processes to make continuous incremental improvements possible.

### Key AWS Services

- Key service for getting operational excelence is CloudFormation, which is use to create templates for your infrastructure. Other services
that support the 3 areas of operational excellence:

- Prepare: AWS Config and AWS Config rules can be used to create standards for workloads.
- Operate: Amazon CloudWatch allows to monitor the health of your workload
- Evolve: Amazon ElasticSearch service (Amazon ES) allows you to analyze your log data to gain actionable insights quickly and securely.

## Addiotional resources

Documentation
- [DevOps and AWS](https://aws.amazon.com/devops/)

Whitepaper
- [Operational Excellence Pillar](https://d0.awsstatic.com/whitepapers/architecture/AWS-Operational-Excellence-Pillar.pdf)

Video
- [DevOps at Amazon](https://www.youtube.com/watch?v=esEFaY0FDKc)