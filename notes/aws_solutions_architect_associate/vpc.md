# Virtual Private Cloud - VPC

- Consists primarily:
  - Internet Gateway
  - Routing tables
  - Network access control lists (NACLS)
  - Security groups
  - Subnets
- 1 Subnet == 1 AZ
- No transitive peering (A --> B --> C, C can talk to B but not to A, it needs to be have it's own peering)
- Security groups are stateful, while NACLS are stateless.
- Bigest network segment is `/16`, the smallest is `/28`