# CloudFront

- Content Delivery Network to deliver content based on geolocation (to reduce latency)
- Edge location is where the content is going to be cached (thoese are separate from AZ's)
- Origin is the source of the files that will be distributed. Can be an S3 bucket, EC2 instance, LB or Route53.
- Distribution is the name of the CDN, which consists of a list of Edge locations.

Web Distributions: Used for websites
RTMP: Used for media streaming

# Exam tips

- Edge locations are both READ and WRITE
- Objects are cached for a default Time to Live (TTL)
- You can clear cached objects but you will be charged (invalidation)

