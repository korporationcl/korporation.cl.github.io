# EC2 (Elastic Compute Cloud)

- You pay for resources on demand (by hour, by second)
- Pricing models are:
  - On demand
  - Reserved instances
  - Spot
  - Dedicated hosts: Physical EC2 Server dedicated.


Reserved instances:
- Standard Reserved Instances: offer up to 75% off. the longer the contract the better the discount.
- Convertible Reserved Instances: offer up to 54% off. You can change instance types during the time.
- Scheduled Reserved Instances: These instances are available during a time frame we set.

Spot instances:
- Usefull for application that are flexible on start and end times (since it can be terminated at any time by AWS)

Dedicated hosts:
- For regulatury requirements(you cannot use multi-tenant virtusalistion or share hypervisor)
- Licensing (license attach to hardware)

# Security Groups

- Security groups are STATEFUL (nacls are stateless)
- All incoming traffic is blocked by default
- all outbound traffic is allowed by default
- Changes are applied immediatly


# EC2 Placement groups

- 3 ways of placing EC2 instances
  - Clustered Placement group
  - Spread Placement group
  - Partitioned

- A cluster placement group: grouping instances within a single AZ. Are recommended for applications that need low latency, high network throughput, or both.
  only some instances can be launched into a Clustered placement group.



# References

- How AWS price works https://d1.awsstatic.com/whitepapers/aws_pricing_overview.pdf (not needed for exam)

# Important for Exam

- If AWS terminates the Spot instance you will not be charged for the partial hour of work. If you terminate the instance, you will be charged (1h).
- Termination protection is OFF by default.
- On EBS backed instances, the default action is to delete the root EBS volume once the instance has been terminated.
- EBS root volume cannot be encrypted (although seems to be a feature recently added). Read more at https://aws.amazon.com/about-aws/whats-new/2019/05/with-a-single-setting-you-can-encrypt-all-new-amazon-ebs-volumes/
- Additional volumes can be encrypted too.
- Attaching IAM roles to EC2 are more security than using access key and secret key (of course)
- IAM Roles are easier to manage
- We change change the Role assigned to an EC2 instance at any time (CLI and web console). IAM is global service.
- metadata gives you information about the instance (curl http://169.254.169.254/latest/meta-data), get the user-data (bootstrap)