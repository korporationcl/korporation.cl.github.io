# Security
The Security pillar includes the ability to protect information, systems, and assets while delivering business value through risk assessments and mitigation strategies.

There are seven design principles for security in the cloud:

- Implement a strong identity foundation: Implement the principle of least privilege and enforce separation of duties with appropriate authorization for each interaction with your AWS resources. Centralize privilege management and reduce or even eliminate reliance on long-term credentials.

- Enable traceability: Monitor, alert, and audit actions and changes to your environment in real time. Integrate logs and metrics with systems to automatically respond and take action.

- Apply security at all layers: Rather than just focusing on protection of a single outer layer, apply a defense-in-depth approach with other security controls. Apply to all layers (e.g., edge network, VPC, subnet, load balancer, every instance, operating system, and application).

- Automate security best practices: Automated software-based security mechanisms improve your ability to securely scale more rapidly and cost effectively. Create secure architectures, including the implementation of controls that are defined and managed as code in version-controlled templates.

- Protect data in transit and at rest: Classify your data into sensitivity levels and use mechanisms, such as encryption, tokenization, and access control where appropriate.

- Keep people away from data: Create mechanisms and tools to reduce or eliminate the need for direct access or manual processing of data. This reduces the risk of loss or modification and human error when handling sensitive data.

- Prepare for security events: Prepare for an incident by having an incident management process that aligns to your organizational requirements. Run incident

There are five best practice areas for security in the cloud:

1. Identity and Access Management
2. Detective Controls
3. Infrastructure Protection
4. Data Protection
5. Incident Response

## Identity and Access Management

Ensure that only authorized and authenticated users access your resources. AWS IAM is the main service which allows you to create and control users
and resources, set password policies, multi factor authentication and so forth.

SEC 1: How do you manage credentials and authentication?
Credentials and authentication mechanisms include passwords, tokens, and keys that grant access directly or indirectly in your workload. Protect credentials with appropriate mechanisms to help reduce the risk of accidental or malicious use.

Best practices:
- Define identity and access management requirements: Identity and access management configurations need to be defined to meet your organizational, legal, and compliance
requirements.

- Secure AWS root user: Secure the AWS root user with MFA, no access keys, and limit its use to help secure your AWS account.

- Enforce use of multi-factor authentication: Enforce multi-factor authentication (MFA) with software or hardware mechanisms to provide additional access control.

- Automate enforcement of access controls: Enforce access controls through automated tools and by reporting irregularities. This helps you maintain your credential management
requirements.

- Integrate with centralized federation provider: Integrate with a federated identity provider or directory service to authenticate all users in a centralized place. This reduces
the requirement for multiple credentials and reduces management complexity.

- Enforce password requirements: Enforce policies for minimum length, complexity, and reuse of passwords to help protect against brute force and other password attacks.

- Rotate credentials regularly: Rotate credentials regularly to help reduce the risk of old credentials being used by unauthorized systems or users.

- Audit credentials periodically: Audit credentials to ensure the defined controls (for example, MFA) are enforced, rotated regularly, and have appropriate access level.

SEC 2: How do you control human access?
Control human access by implementing controls inline with defined business requirements to reduce risk and lower the impact of unauthorized access. This applies to privileged users and administrators of your AWS account, and also applies to end users of your application

Best practices:
- Define human access requirements: Clearly define access requirements for users based on job function to reduce the risks from unnecessary privileges.

- Grant least privileges: Grant users only the minimum privileges you have defined to reduce the risk of unauthorized access.

- Allocate unique credentials for each individual: Credentials are not shared between any users to help segregation of users and traceability.

- Manage credentials based on user lifecycles: Integrate access management with user lifecycle. For example, decommission a user to revoke unused and unnecessary credentials
when a user leaves or changes roles.

- Automate credential management: Automate credential management to enforce minimum privileges and disable unused credentials. Automate auditing, reporting, and
management of the lifecycle of users.
- Grant access through roles or federation: Use IAM roles instead of IAM users or static access keys to allow for secure cross-account access and federated users.

SEC 3: How do you control programmatic access?
Control programmatic or automated access with appropriately defined, limited, and segregated access to help reduce the risk of unauthorized access. Programmatic access includes access that is internal to your workload, and access to AWS related resources.


Best practices:
- Define programmatic access requirements: Clearly define access requirements for automated or programmatic access to reduce the risks from unnecessary privileges.

- Grant least privileges: Grant automated or programmatic access only the minimum privileges you have defined to reduce the risk of unauthorized access.

- Automate credential management: Automate credential management to enforce minimum privileges and disable unused credentials. Automate auditing, reporting, and
management of dynamic authentication.

- Allocate unique credentials for each component: Credentials are not shared between any component to help segregation and traceability. For example, use different IAM roles for
AWS Lambda functions and EC2 instances.

- Grant access through roles or federation: Use IAM roles or federation instead of IAM users or static access keys to allow for secure programmatic access.

- Implement dynamic authentication: Credentials are dynamically acquired and frequently rotated by a service or system.


- Credentials should not be shared between any user or system!
- User access should be granted using the least privilege approach, password requirements and multi factor authentication enabled.

## Detective Controls

You can use detective controls to identity a potential security threat or incident. There are different type for detective controls, in AWS you can
implement controls by processing logs, events, and monitoring that allows for auditing, automated analysis, alarms.

- AWS CloudTrail logs AWS API calls
- AWS CloudWatch provide monitoring for metrics with alarming
- AWS Config provide configuration history (resources)
- AWS GuardDuty is a managed threat detection that monitors for malicious or unauthorized behaviour.
- Service log level can be stored on S3 (log requests)

SEC 4: How do you detect and investigate security events?
Capture and analyze events from logs and metrics to gain visibility. Take action on security events and potential threats to help secure your workload.

Best practices:
- Define requirements for logs: Define requirements for retention and access control for logs to meet your organizational, legal, and compliance requirements.

- Define requirements for metrics: Collecting metrics and defining baselines allows you to gain insights to potential security threats.

- Define requirements for alerts: Define who should receive alerts and what they should do with the alerts they receive.

- Configure service and application logging: Configure logging throughout the workload, including application logs, AWS services logs, and resource logs.

- Analyze logs centrally: All logs should be collected centrally and automatically analyzed to detect anomalies and indicators of malicious activity or compromise.

- Automate alerting on key indicators: Key indicators, including metrics and events related to security, should be monitored and trigger automated alerts based on thresholds.

- Develop investigation processes: Develop processes to investigate different types of events, including escalation paths for incident response processes.


SEC 5: How do you defend against emerging security threats?
Staying up to date with AWS and industry best practices and threat intelligence helps you be aware of new risks. This enables you to create a threat model to dentify, prioritize, and implement appropriate controls to help protect your workload

Best practices:
- Keep up to date with organizational, legal, and compliance requirements: Stay up to date with organizational, legal, and compliance requirements that enable you to adjust
your security posture to comply.

- Keep up to date with security best practices: Stay up to date with both AWS and industry security best practices to evolve protection of the workload.

- Keep up to date with security threats: Understand attack vectors by staying up to date with the latest security threats. This helps you implement detective and preventive
controls.

- Evaluate new security services and features regularly: Evaluate security services from both AWS and APN Partners, including new features to limit risk of threats.

- Define and prioritize risks using a threat model: Use a threat model to identify and maintain an up to date register of potential threats. Prioritize your threats and adjust your security posture to respond.

- Implement new security services and features: Adopt security services and features to implement controls that help protect your workload.

* Log Management is important for reasons ranging from security or forensic to regulatory or legal requirements.
* AWS allows you to create data retention policies to define a data life cycle (S3, Glaciar, or delete data we don't need)

## Infrastructure protection

Infrastructure protection encompasses control methodologies, such as defense in depth, necessary to meet best practices and organizational or regulatory obligations. Use of these methodologies is critical for successful, ongoing operations in either the cloud or on-premises.

- You can implement stateful and stateless  packet inspection using native AWS technologies or through partnerts on the AWS Marketplace.
- You should use VPC to create private, secured and escalable environments.

SEC 6: How do you protect your networks?
Public and private networks require multiple layers of defense to help protect from external and internal network-based threats.

Best practices:
- Define network protection requirements: Define controls for protection of your networks to meet your organizational, legal, and compliance requirements.

- Limit exposure: Limit the exposure of the workload to the internet and internal networks by only allowing minimum required access.

- Automate configuration management: Enforce and validate secure configurations automatically by using a configuration management service or tool to reduce human error.

- Automate network protection: Automate protection mechanisms to provide a self defending network based on threat intelligence and anomaly detection.

- Implement inspection and protection: Inspect and filter your traffic at the application level; for example, by using a web application firewall, to help protect against threats.

- Control traffic at all layers: Apply controls for controlling both ingress and egress traffic, including data loss prevention. For Amazon Virtual Private Cloud (VPC) this includes security groups, Network ACLs, and subnets. For AWS Lambda, consider running in your private VPC to control traffic.


SEC 7: How do you protect your compute resources?
Compute resources in your workload require multiple layers of defense to help protect from external and internal threats. Compute resources include EC2 
instances, containers, AWS Lambda functions, database services, IoT devices, and more.

Best practices:
- Define compute protection requirements: Define the protection controls for your compute resources to meet your organizational, legal, and compliance requirements.

- Scan for and patch vulnerabilities: Frequently scan for and patch vulnerabilities in your code base and in your infrastructure to help protect against new threats.

- Automate configuration management: Enforce and validate secure configurations automatically by using a configuration management service or tool to reduce human error.

- Automate compute protection: Automate defense with intrusion prevention, including the ability to protect against unknown threats; for example, use virtual patching.

- Reduce attack surface: Reduce the attack surface, including hardening EC2 operating systems and configuring container and serverless resources, to help limit exposure.

- Implement managed services: Implement services that manage resources, such as Amazon RDS, AWS Lambda, and Amazon ECS, to reduce security maintenance tasks.

* Customers can harden the configuration of EC, EC2 Container Service, or ElasticBeanStalk creating an Amazon Machine Instance (AMI)
* Then, you can use Autoscaling or launch manually more of those instances

## Data Protection

- AWS Customers maintain full control of their data
- AWS allows you to encrypt your data and manage keys with AWS KMS.
- Detailed logging contain can contain file access and changes, if you need it.
- AWS has designed storage system with exceptional resilency.
- AWS S3 (standard, Infrequent access, One zone IA, Glacier) provide %99.99999999999 (11x*9) durability of objects over  a given year.
- Versioning to protect overwritten of the data, deletion and so forth (S3)
- Content in a region stays there unless you move the data out of the region.

SEC 8: How do you classify your data?
Classification provides a way to categorize data, based on levels of sensitivity, to help you determine appropriate protective and retention controls.

Best practices:
- Define data classification requirements: Define data classification requirements to meet your organizational, legal, and compliance requirements.

- Define data protection controls: Protect data according to its classification level; for example, secure publicly accessible data by using best practices while protecting sensitive data with additional controls.

- Implement data identification: Classify data with easily identifiable indicators; for example, use tags for Amazon S3 buckets and objects that classify the data in the buckets.

- Automate identification and classification: Automate identification and classification of data to reduce the risk of human error.

- Identify the types of data: Be aware of the types of data in your workload to help you implement controls to meet organizational, legal, and compliance requirements.

SEC 9: How do you protect your data at rest?
Protect your data at rest by defining your requirements and implementing controls, including encryption, to reduce the risk of unauthorized access or loss.

Best practices:
- Define data management and protection at rest requirements: Define data management and protection at rest requirements, such as encryption and data retention, to meet your
organizational, legal, and compliance requirements.

- Implement secure key management: Encryption keys must be stored securely, and rotated with strict access control; for example, by using a key management service such as
AWS Key Management Service. Consider using different keys for segregation of different data classification levels and retention requirements.

- Enforce encryption at rest: Enforce your defined encryption requirements based on the latest standards and best practices to help protect your data at rest.

- Enforce access control: Enforce access control with least privileges and mechanisms, including backups, isolation, and versioning, to help protect your data at rest. Consider
what data you have that is publicly accessible.

- Provide mechanisms to keep people away from data: Keep all users away from directly accessing sensitive data. For example, provide a dashboard instead of direct access to a
data store, and provide tools to indirectly manage the data.

SEC 10: How do you protect your data in transit?
Protecting your data in transit by defining your requirements and implementing controls, including encryption, reduces the risk of unauthorized access or exposure.

Best practices:
- Define data protection in transit requirements: Define data protection in transit requirements, such as encryption standards, based on data classification to meet your
organizational, legal, and compliance requirements. Best practices are to encrypt and authenticate all traffic, and to enforce the latest standards and ciphers.

- Implement secure key and certificate management: Store encryption keys and certificates securely and rotate them with strict access control; for example, by using a
certificate management service such as AWS Certificate Manager.

- Enforce encryption in transit: Enforce your defined encryption requirements based on the latest standards and best practices to help you meet your organizational, legal, and
compliance requirements.

- Automate detection of data leak: Use a tool or detection mechanism to automatically detect attempts to move data outside of defined boundaries; for example, to detect a
database system that is copying data to an unknown host.

- Authenticate network communications: Verify the identity of communications by using protocols, such as Transport Layer Security (TLS) or IPsec, to reduce the risk of data
tampering or loss.

* S3 SSE (server side encryption) helps you to encrypt your data at rest
* SSL termination can be done on Elastic Load balancer (https encryption and decryption)

## Incident Response

In AWS, the following practices facilitate effective incident response:

- Detailed logging (file access, changes, etc)
- Events can trigger automatic actions to response to those events through AWS API
- You can pre-provision "clean" infrastructure using CloudFormation.

SEC 11: How do you respond to an incident?
Preparation is critical to timely investigation and response to security incidents to help minimize potential disruption to your organization.

Best practices:
- Identify key personnel and external resources: Identify internal and external personnel and resources that would help your organization respond to an incident.

- Identify tooling: Identify tooling that would help your organization respond to an incident.

- Develop incident response plans: Create incident response plans, starting with the most likely scenarios for your workload and organization. Include how you would communicate
and escalate both internally and externally.

- Automate containment capability: Automate containment of an incident to reduce response times and organizational impact.

- Identify forensic capabilities: Identify the forensic investigation capabilities that are
available, including external specialists.

- Pre-provision access: Ensure that security personnel have the correct access preprovisioned into AWS so that an appropriate response can be made to an incident.

- Pre-deploy tools: Ensure that security personnel have the right tools pre-deployed into AWS so that an appropriate response can be made to an incident.

- Run game days: Practice incident response game days (simulations) regularly, incorporate lessons learned into plans, and continuously improve responses and plans.

## Key AWS Services

- IAM is the most essential service
- Detective controls can be performed using AWS CloudTrail that records all API calls.
- AWS GuardDuty helps in the detection of unathorized access
- AWS CloudWatch  can trigger automatic actions based on events (metrics)
- Infrastructure protection is led by Amazon VPC, we can launch resources in a private cloud network. CloudFront is a content delivery network (CDN) that delivers data, video, applications and so forth.
- You can use AWS Shield on CloudFront for DDOS mitigation.
- AWS WAF (web application firewall) can be deployed on both CloudFront or Application Load Balancer (ALB) to protect your web app from
common web exploits.
- Data protection, ELB, EBS, S3, RDS include encryption capabilities to encrypt data on transit and at rest. 
- Amazon Macie automatically discovers, classifies and protects sensitive data.
- AWS KMS can be use to create and manage keys for encryption
- Incident Response, using CloudFormation we can automated actions based on CloudWath events, then processed the response on AWS Lambda.

## Additional resources

Documentation
- [AWS Cloud Security](http://aws.amazon.com/security/)
- [AWS Compliance](https://aws.amazon.com/compliance/)
- [AWS Security Blog](http://blogs.aws.amazon.com/security/)

Whitepaper
- [Security Pillar](https://d0.awsstatic.com/whitepapers/architecture/AWS-Security-Pillar.pdf)
- [AWS Security Overview](https://d0.awsstatic.com/whitepapers/Security/AWS%20Security%20Whitepaper.pdf)
- [AWS Security Best Practices](https://aws.amazon.com/whitepapers/aws-security-best-practices/)
- [AWS Risk and Compliance](https://d0.awsstatic.com/whitepapers/compliance/AWS_Risk_and_Compliance_Whitepaper.pdf)

Video
- [AWS Security State of the Union](https://youtu.be/Wvyc-VEUOns)
- [Shared Responsibility Overview](https://www.youtube.com/watch?v=U632-ND7dKQ)