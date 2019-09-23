# Load Balancers

- **3 types of LB**
  - Application Load Balancer: Layer 7 LB.
  - Classic Load Balancer (legacy), Layer 4 LB, cheaper.
  - Network load balancer: high network throughput
- To deploy a LoadBalancer you need to at least have 2 public subnets available.
- **504 error means the backend associated with the LB has timeout (instance/application having issues).**
- **X-Forwarded-For function forwards a header with the IPv4 of origin.**
- **Instances are monitored by ELB and reported as `InService` or `OutOfservice`.**
- **Healthchecks check the instance health.**
- **Load Balancers have their unique DNS name**
- **READ ELB FAQ https://aws.amazon.com/elasticloadbalancing/faqs/**
- `sticky sessions` will send the traffic to the same instance. On ALB, traffic is sent to Target group. This is usefull for file uploading.
- `Cross Zone Load Balancing` send traffic to instances in different availability zones.
- `Path patterns`, you can forward requests based on the path URL (path based routing).

# References
- https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html
- https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-backend-instances.html