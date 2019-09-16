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
  - Clustered Placement group (High network throughput / low latency)
  - Spread Placement group (individual critical EC2 instances running in different hardware)
  - Partitioned (Multiple EC2 instances, Cassandra clusters, Hbase, etc)

- A cluster placement group: grouping instances within a single AZ. Are recommended for applications that need low latency, high network throughput, or both.
  only some instances can be launched into a Clustered placement group.

- Spread placement group: Group of instances placed on different underlaying hardware (hosts). Are recommended for applications that have small number of instances
  and should be kept seperate from each other (Individial Instances).

- Partitioned: EC2 divides each group in logical segments called "Partitions". Each partition will run in it's own Rack. Each Rack has it's own network, power        source, etc. Two partitions cannot live on the same Rack, this isolates the impact of having hardware failures.


- Clustered placement group can't span multi AZ
  - Spread and Partitioned only can
- The name of the placement group is **unique** within AWS
- Only some instances are supported to be launched in Placement group (Compute optimized, GPU, Memory Optimized, Storage optimized)
- AWS recommends using the same instance type for launching a placement group.
- You can't merge Placement groups
- You can't move an existent instance to a Placement group. You need to create an AMI and launch a new instance in the placement group.
- Spread Placement group can have a maximum of 7 instances per AZ.

Reference: `https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html`



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