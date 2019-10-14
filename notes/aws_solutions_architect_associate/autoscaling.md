# EC2 Autoscaling

- Launch Configuration: Your group uses a launch configuration as a template for its EC2 instances. When you create a launch configuration, you can specify information such as the AMI ID, instance type, key pair, security groups, and block device mapping for your instances.
- The cooldown period is a configurable setting that helps ensure to not launch or terminate additional instances before previous scaling activities take effect.
- You can add a lifecycle hook to your Auto Scaling group to perform custom actions when instances launch or terminate.
