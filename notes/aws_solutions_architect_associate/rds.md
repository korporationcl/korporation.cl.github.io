# Relational Database Service - RDS

- Online Transactional processing (OLTP)
  - SQL
  - MySQL
  - AuroraDB
  - PostgreSQL
  - Oracle
- NO SQL
  - DynamoDB
- RedShift (Analytics) - Online Analytics process (OLAP)
- ElastiCache (Memcache and Redis). Speed up performance of existent databases (caching in memory)

# Features to remember
- Patching AWS instance is Amazon's responsibility, of course.
- RDS is not serverless (Aurora can though, the rest is not)
- You cannot login into the instance but you can access via `mysql-client`, as usual.

# Read Replicas, Multi AZ and Backups

## Backups

- Automated backups
  - Automated backups allow you to recover in any point in time within the **retention period**. The retention period can be between `1` and `35` days.
  - Generates a daily Full backup and will save the transaction logs throughout the day.
  - When performing a recovery, AWS choose the most recent daily backup and apply all the transactions pending from logs.
  - Automated backups are enabled by default
  - Backups are stored on S3 and you get free storage equal to the size of the database. If RDS instance is 10GB you get 10GB for backups.
  - **Backups are generated within the defined window. During this time storage/IO may be suspended or you may experience latency.**


- DB Snapshots
  - Those are generated manually. They are stored even after deleting the instance, unlike automated backups.

- Restoring a snapshot/backup will always create a new instance and DNS endpoint.

## Multi AZ

- Allows you to have a copy of the running instance in another AZ. Replication is done by AWS. In the event of a maintenance event, DB failure, AZ Failure, etc,
  AWS will switch the traffic over to the standby replica without intervention.
- Multi AZ is for disaster recovery - high availability purposes.
- Multi AZ is available for:
  - MySQL
  - SQL Server
  - Oracle
  - MariaDB
  - PostgreSQL


# Read Replicas

- Read replicas are read-only
- Read replicas use async replication from primary database
- You use read replicas for heavy read-only workloads
- Replicas are used for scaling, not DR.
- In order to deploy a replica, you need to enable backups on main database
- You can have up to 5 read replica copies
- You can have read replicas of another replica.
- Each read replica have it's own DNS endpoint
- Read replicas can have Multi AZ enabled
- Read replicas can be promoted to be master. This breaks the replication
- You can have a read replica in another region.

## Encryption at rest

- Encryption is done using KMS
- Encryption is supported for:
  - MySQL
  - Oracle
  - SQL Server
  - PostgreSQL
  - MariaDB
  - Aurora
- Once the instance is using encryption, the data stored on the volume, snapshots, read replicas and backups are encrypted (everything is encrypted).

# References

- https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Limits.html
- https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateSnapshot.html
