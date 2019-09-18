# Amazon Route53

- ELB never have a pre-defined IPV4 address, you resolve them using DNS.
- Alias record vs CNAME
  - to reference AWS resources use Alias record
- Always use Alias record
- Common types of record:
  - A
  - CNAME
  - MX
  - NS
  - SOA
  - PTR
- **You can buy domain names directly from Amazon (It can take up to 3 days to register a domain name)**

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


# References
- https://www.isc.org/blogs/cname-at-the-apex-of-a-zone/