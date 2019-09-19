# Amazon Route53

- **ELB never have a pre-defined IPV4 address, you resolve them using DNS.**
- Alias record vs CNAME
  - to reference AWS resources use Alias record
  - **Always use Alias record**
- Common types of record:
  - A
  - CNAME
  - MX
  - NS
  - SOA
  - PTR
- **You can buy domain names directly from Amazon (It can take up to 3 days to register a domain name)**
- Alias Records have special functions that are not present in other DNS servers. Their main function is to provide special functionality and      integration into AWS services. Unlike CNAME records, they can also be used at the Zone Apex, where CNAME records cannot. Alias Records can       also point to AWS Resources that are hosted in other accounts by manually entering the ARN Further information.
- There is a default limit of 50 domains names which can be increased contacting AWS support.

# Routing Policies

- Simple routing policy: One record with multiple IP addresses.
  - Route53 will return a different value in RR (TTL)

- Weighted routing policy: Split the traffic based on different weights assigned.
  - Example: You can send 50% traffic to Sydney and 50% to Ireland.
  - You can set healthchecks on individual records. If the healthcheck does not pass the endpoint will be removed until is green again.
  - You can send an email/sms notification with AWS SNS if a healthcheck fails.

- Latency-based routing: allows you to redirect traffic based on network latency.

- Failover routing policy: If you want to have an active-passive setup. If the healthcheck does not pass traffic will be redirected.

- Geolocation routing policy: Traffic will be redirected based on the geo-location of the users.

- Geoproximity routing (Traffic Flow): Route53 route traffic to your resources based on geo-location of users and AWS resources. You can           specificy how much traffic you want to send setting a **bias** value (weight). To use this service you need to setup Route53 Traffic Flow.

- Multivalue routing policy: Multiple records can have healthchecks (same as simple routing but with healthchecks)
  - Use when you want Route 53 to respond to DNS queries with up to eight healthy records selected at random.

# References
- https://www.isc.org/blogs/cname-at-the-apex-of-a-zone/
- https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/ResourceRecordTypes.html
- https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html
- https://tools.ietf.org/html/rfc2181
- https://tools.ietf.org/html/rfc1034