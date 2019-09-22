# Load Balancers

- **3 types of LB**
  - Application Load Balancer: Layer 7 LB.
  - Classic Load Balancer (legacy), Layer 4 LB, cheaper.
  - Network load balancer: high network throughput
- To deploy a LoadBalancer you need to at least have 2 public subnets available.
- **504 error means the backend associated with the LB has timeout (instance/application having issues).**
- **X-Forwarded-For function forwards a header with the IPv4 of origin.**

