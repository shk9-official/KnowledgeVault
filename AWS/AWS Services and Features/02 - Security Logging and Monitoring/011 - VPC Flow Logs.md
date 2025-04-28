
### VPC Flow Logs

- Captures IP traffic to and from from VPC.
- Scope
	- Traffic information that is flowing into a resource, such as EC2.
	- Traffic information of resource connecting to an outbound endpoint
- Captures at interface level of EC2 as well
- Flow logs captured is not real time
- Flow logs are stored in 
	- S3
	- CloudWatch
		- Permissions for VPC Flow logs to send logs to CloudWatch log group
			- logs:CreateLogGroup
			- logs:CreateLogStream
			- logs:PutLogEvents
			- logs:DescribeLogGroups
			- logs:DescribeLogStreams
- Analysis on Flow logs can be done via
	- Athena
	- CloudWatch insights (log insights and contributor insights)
- Some of the IP traffic which are not captured in Flow logs
	- Traffic to and fro AWS DNS server (169.254.169.253)
	- Traffic to and fro for Amazon Windows license activation
	- Traffic to and fro for 169.254.169.254 for AWS instance metadata
	- DHCP traffic
- VPC Flow Log format
	- Version -> 2
	- Account ID -> 7742829482
	- InterfaceID -> eni-4d788e3d
	- SrcAddr -> 115.73.149.218
	- DestAddr -> 10.0.5.157
	- SrcPort -> 12053
	- DestPort -> 23
	- Protocol # -> 6 (TCP->6, UDP ->17, ICMP->1,...)
	- PacketsTransferred -> 2
	- BytesTransferred -> 88
	- StartTimeUnixSec -> 1485439809
	- EndTimeUnixSec -> 14854440090
	- Action -> Accept/Reject
	- LogStatus -> Ok


### VPC traffic mirroring

![vpctrafficmirroring.png](Attachments/vpctrafficmirroring.png)

---
