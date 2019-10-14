# MySQL Aurora

# Features
- Start with 10GB, scales in 10GB increments to 64TB (Storage autoscaling)
- Compute resources can scale up to 32vCPU and 244GB of memory
- **2 copies of you data is contained in each availability zone. Minimum of **3** AZ, 6 copies of the data.**
- **Aurora is designed to handle the loss of `2` copies of the data for writes, and up to `3` for reading.**
- Aurora storage is self-healing. Data blocks and disks are continously scanned for errors and repair them.
- **Two types of Aurora**:
  - Aurora replicas (currently 15)
  - MySQL Read replicas (currently 5)
- **Automated failover is only available with Aurora replicas**

  # Backups

- **Backups are enabled by default.**
- Backups do not impact database performance (sureeee)
- **You can take snapshots in Aurora without database performance (sureee)**
- **You can share snapshots with other AWS accounts**

# References

- https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Replication.html