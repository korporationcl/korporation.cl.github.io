# DynamoDB

- DynamoDB is a NOSQL database engine managed by AWS.
- Use cases:
  - Storing JSON Data
  - Storing web sessions
  - Storing metadata of blob data stored in S3
- When creating a table you have to specify a **table name and **primary key**
- A primary key cannot be changed once the table has been created. A primary key uniquely identifies each item in the table.
- DynamoDB supports two types of keys:
  - Partition key: Primary key is made of one attribute. It builds an unordered hash index on this primary key.
  - Partition and sort key: Primary key is made of two attributes. First attribute is the `partition key` and the second is the `sort key`.

Creating a table called `Music` with a primary key `artist`:

| Artist | Song
| Toto | Viva la vida
| ColdPlay | For you
| ColdPlay | Viva la vida
| BobMarley | 3 little birds

- Basic components of DynamoDB are:
  - Tables
  - Items
  - Attributes

- Backups are disabled by default
- Attributes item's limit is 400KB
- **Data is spread across 3 different data centers (AZ)**
- The consistency models they have are:
- **Eventual consistent reads (default)**
  - Consistency across all copies of data is usually reached within a second. Repeating a read after a short time should return the updated data (best read performance).
  - **Strongly consistent reads**
    - Read returns a result that reflects all writes that received a successful response prior to the read (less than 1 second)
- **When using a single region you are charged by:**
 - Storage
 - Read and write capacity
- By default a `table` has `provisioned` read-throughout and write-throughout associated and you are billed by the hour of that throughput if you exceeded the free tier.
- **DynamoDB automatically spreads the data and traffic for the table over a sufficient number of partitions.**
- **DynamoDB Accelerator (DAX) delivers fast response times for accessing eventually consistent data (microseconds).**