
- Prior approval for penetration test is not required for following services
	- EC2
	- NAT Gateway
	- Elastic load balance (ELB)
	- RDS
	- CloudFront
	- Aurora
	- API Gateway
	- Lambda and Lambda edge functions
	- Lightsail resources
	- Beanstalk environments
	- Elastic Container Service (ECS)
	- Fargate
	- Elasticsearch
	- FSX
	- Transit gateway
	- S3 hosted applications

- The following activities are prohibited
	- DNS zone walking via Route53
	- DoS, DDoS
	- Port flooding
	- Protocol flooding
	- Request flooding
	- Penetration test on DynamoDB

- Simulated DDoS testing to be performed by AWS APN test partner.
	- Target -> Protected AWS resource or edge optimized API Gateway via Shield Advanced.
	- Source -> Not from AWS


---
