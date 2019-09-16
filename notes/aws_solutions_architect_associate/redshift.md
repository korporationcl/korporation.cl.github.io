# AWS RedShift

# Backups

- Enabled by default (1 day retention), maximum retention is 35 days.
- Redshift will try to keep 3 copies of the data set, the original, the replica on the compute nodes and backup in S3)
- Can asynchronously replicate your snapshots to S3 in another region for DR.
-