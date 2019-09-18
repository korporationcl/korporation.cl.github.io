# AWS RedShift

- is a fast and powerfull, fully managed, petabyte-scale data warehouse service (for analytics purposes)
- data warehousing databases use different architecture from a database perspective and infrastructure.
- You can setup RedShift as
  - a single node (160GB)
  - Multi node
    - Leader node: manages client connections and receives queries
    - Compute node: store data and perform queries and computation.
    - Maximum amount of compute nodes is 128.
- Advanced compression
  - use less space
- Massively Parallel Processing (MPP)
  - Redshift distributes data and query load across all nodes.
  - You can easily add more nodes to the cluster

- Connections are encrypted on transit using SSL
- Data is encrypted at rest using AES-256 encryption
- By default RedShift manages the encryption key
  - You can use HSM to manage your own key or KMS
- Redshift is only available in **ONE** AZ


# Backups

- Enabled by default (1 day retention), maximum retention is 35 days.
- Redshift will try to keep 3 copies of the data set, the original, the replica on the compute nodes and backup in S3)
- Can asynchronously replicate your snapshots to S3 in another region for DR.

# Price

- Compute node hours; total number of hours you run across all nodes. You are billed 1 unit per node per hour.
- You will not be charged for the leader node hours. Only compute nodes will incur in charges.
- You will be charged for backups and data transfer