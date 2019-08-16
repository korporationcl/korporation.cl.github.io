# Storage Gateway

- Virtual machine appliance can be use or physical hardware installed in datacenter (VMWare or Microsoft Hyper-V)
- 3 types of storage are:
  - File Gateway (NFS/Samba): Objects are stored in S3 and you mount them through NFS
  - Volume Gateway (iSCI): This presents volumes to the application (like a EBS volume). Data can be stored in AWS as EBS snapshots.
    - Stored volumes (primary data stored locally then async backing up to AWS as EBS Snapshots)
    - Cached volumes (S3 is the primary data storage and the local disk are for caching the frequently access data)
  - TapeGateway (VTL) - replicate your tape data to AWS as backup

# Exam tips
- File gateway for storing files on S3
- Volume gateways
  - Store volume: data is accessed locally and back up to AWS async.
  - Cache volume: data is stored in S3 and most frequently access objects cached locally.
  - TapeGateway: to move tape data to AWS.


