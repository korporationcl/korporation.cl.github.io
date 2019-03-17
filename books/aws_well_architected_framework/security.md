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

SEC 2: How do you control human access?
Control human access by implementing controls inline with defined business requirements to reduce risk and lower the impact of unauthorized access. This applies to privileged users and administrators of your AWS account, and also applies to end users of your application

SEC 3: How do you control programmatic access?
Control programmatic or automated access with appropriately defined, limited, and segregated access to help reduce the risk of unauthorized access. Programmatic access includes access that is internal to your workload, and access to AWS related resources.

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

SEC 5: How do you defend against emerging security threats?
Staying up to date with AWS and industry best practices and threat intelligence helps you be aware of new risks. This enables you to create a threat model to dentify, prioritize, and implement appropriate controls to help protect your workload

- Log Management is important for reasons ranging from security or forensic to regulatory or legal requirements.
- AWS allows you to create data retention policies to define a data life cycle (S3, Glaciar, or delete data we don't need)

## Infrastructure protection

Infrastructure protection encompasses control methodologies, such as defense in depth, necessary to meet best practices and organizational or regulatory obligations. Use of these methodologies is critical for successful, ongoing operations in either the cloud or on-premises.

- You can implement stateful and stateless  packet inspection using native AWS technologies or through partnerts on the AWS Marketplace.
- You should use VPC to create private, secured and escalable environments.

SEC 6: How do you protect your networks?
Public and private networks require multiple layers of defense to help protect from external and internal network-based threats.

SEC 7: How do you protect your compute resources?
Compute resources in your workload require multiple layers of defense to help protect from external and internal threats. Compute resources include EC2 
instances, containers, AWS Lambda functions, database services, IoT devices, and more.

- Customers can harden the configuration of EC, EC2 Container Service, or ElasticBeanStalk creating an Amazon Machine Instance (AMI)
- Then, you can use Autoscaling or launch manually more of those instances

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

SEC 9: How do you protect your data at rest?
Protect your data at rest by defining your requirements and implementing controls, including encryption, to reduce the risk of unauthorized access or loss.

SEC 10: How do you protect your data in transit?
Protecting your data in transit by defining your requirements and implementing controls, including encryption, reduces the risk of unauthorized access or exposure.

- S3 SSE (server side encryption) helps you to encrypt your data at rest
- SSL termination can be done on Elastic Load balancer (https encryption and decryption)

## Incident Response

In AWS, the following practices facilitate effective incident response:

- Detailed logging (file access, changes, etc)
- Events can trigger automatic actions to response to those events through AWS API
- You can pre-provision "clean" infrastructure using CloudFormation.

SEC 11: How do you respond to an incident?
Preparation is critical to timely investigation and response to security incidents to help minimize potential disruption to your organization.

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