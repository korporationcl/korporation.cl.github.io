# DynamoDB

- Data is stored in SSD
- Data is spread across 3 different data centers (AZ)
- Eventual consistent reads (default)
  - Consistency across all copies of data is usually reached within a second (read takes 1 or more second). Repeating a read after a short time should return the
    updated data (best read performance).
- Strongly consistent reads
  - Read returns a result that reflects all writes that received a successful response prior to the read (less than 1 second)