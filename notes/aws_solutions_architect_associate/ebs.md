# Elastic Block Storage (EBS)

- General Purpose SSD (gp2). Designed for most of the workloads.
- Provision IOPS (io1). Designed mostly for databases.
  - Max IOPS is 32,000
- Throughput optimised HDD (st1). Designed for data warehouse and big data.
- Cold Hard drive (sc1). File Servers (NFS/Samba)
- Magnetic (standard). Infrequently access data.
- EBS Snapshots are stored on S3
- Once an instance is terminated so will the root volume, but not additional volumes.
- Snapshots are incremental.
- To create an snapshot of the root volume you need to stop the instance to guarantee data consistency (you can do it anyway.)
- Volumes are always in the same AZ as the instance.
- To migrate a volume to a different AZ, take snapshot, create an AMI of it and then launch the instance on the new AZ.
- To move onen volume to a different region,, take snapshot, create an AMI and copy that AMI to that region.

# References
- EBS Performance: `https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSPerformance.html`

# Encrypted root device volumes/snapshots

You can encrypt root device volumes on creation or follow the procedure:

1. Create snapshot from root device
2. Create a copy of the snapshot and select encrypt snapshot
3. Create AMI from the encrypted version of the snapshot
4. Launch new instance using the encrypted snapshot


# Tips

- snapshots from volumes that are encrypted cannot be launch without encryption
- so volumes restored from encrypted snapshots are encrypted by default
- you can share snapshots only if they are **unencrypted**
- you can encrypt root device volume on launch
