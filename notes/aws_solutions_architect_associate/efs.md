# Elastic File System

Is a filestorage  service for EC2. File system grows automatically, same as for shrinking.

# Tips

- Do a lab about setting up EFS and attach the file system to EC2.
- Supports NFSv4 protocol
- You only pay for the storage you use (no pre-provision required)
- Can scale up to Petabytes of data
- Can support thousands of concurrent NFS connections
- Data is stored across multi AZ
- Read after Write consistency (same as S3)