# AMI Types

- Based on Region
- Operating System
- Architecture
- Storage for device (EBS backed vs Instance store aka ephemeral storage)
- Instances EBS backed: root device is created from an EBS snapshot
- Instance store: root device is created from 'template' stored in S3.
- you can reboot both types without loosing data.
- On termination the root volume will be deleted, although we can keep the root device on EBS backed instances (not by default)

