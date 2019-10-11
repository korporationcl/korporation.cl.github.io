# Amazon Route53

- Route53 is an Authoritative DNS service
- TLD; Top Level domains
  - .com
  - .edu
  - .uk
- SOA (Start of authority record)
  - Administrator of the zone
  - default nunber of seconds/TTL for the records
  - Name of the server
  - Current version of the zone
  - It is mandatory in all zone files
- TLD domain servers will redirect traffic to the DNS Server which contains the Authoritive DNS records (Route53)
- You can get a domain with any Domain registrars. Domain registrars manage part of the Internet domain names.
- Domain registrars must be acredited by a gTLD(Generic TLD) and/or country code TLD (ccTLD) registry.
- There are 13 root servers available across the whole Internet

# How DNS resolution works

1. You type `www.amazon.com` in your browser,  it will check your  `/etc/hosts` file.
2. If no entry is there, then will ask the local cache.
3. If no local cache is available, it will ask the DNS resolver for the results.
4. If no answer there, DNS resolver it will ask the ROOT servers.
5. A root server will receive the request and will try to resolve the domain name but will fail.
6. root server will find the the entity responsible for the TLD (.com) and will get the name server.
7. the request is send to the root server in charge of the domain `www.amazon.com`. This will get you IP of the name servers for the domain.
8. route53/name-server checks for an entry looking for the `www.amazon.com`. Inside the `amazon.com` zone, there is a record for `www`.


- **ELB never have a pre-defined IPV4 address, you resolve them using DNS.**
- Alias record vs CNAME
  - to reference AWS resources use Alias record
  - **Always use Alias record**
- Common types of record:
  - A: Converts a name to IP. (AAAA for IPv6 address)
  - CNAME (canonical name): Can resolve a domain name to another. `pepe.example.com` CNAME `mara.example.com`.
  - MX (Mail Exchange): Define your mail servers
  - NS (Name servers): Name servers are used by TLD servers to direct traffic to the DNS server that contains the authoritative records.
  - PTR(Pointer): Reverses an IP to domain name.
  - SPF(Sender Policy Framework): Tell you which IP's are allowed to send email on behalf of the company name.
  - TXT: Text information.
  - SRV (Service): Defines the name and port number for a particular service. For example `example.com` has a HTTP service running on `TCP/80`.

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

- Geolocation routing policy: Traffic will be redirected based on the geo-location of the users. It has 3 levels of granularity: `continent`, `country` and `state`.

- Geoproximity routing (Traffic Flow): Route53 route traffic to your resources based on geo-location of users and AWS resources. You can           specificy how much traffic you want to send setting a **bias** value (weight). To use this service you need to setup Route53 Traffic Flow.

- Multivalue routing policy: Multiple records can have healthchecks (same as simple routing but with healthchecks)
  - Use when you want Route 53 to respond to DNS queries with up to eight healthy records selected at random.

# References
- https://www.isc.org/blogs/cname-at-the-apex-of-a-zone/
- https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/ResourceRecordTypes.html
- https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html
- https://tools.ietf.org/html/rfc2181
- https://tools.ietf.org/html/rfc1034