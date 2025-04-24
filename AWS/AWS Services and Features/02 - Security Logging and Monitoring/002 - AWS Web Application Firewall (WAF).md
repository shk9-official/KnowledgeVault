
### AWS Web Application Firewall (WAF)

Works on layer 7
- HTTP, HTTPS, gRPC protocols
- Can check UserAgent etc.

- Rule statements
	- What to check for inside a web request?
	- Ex: HTTP headers, HTTP methods, Query strings, URI path, Body etc.
- Rules
	- Multiple Rule statements (from above) combined with AND, OR, NOT and with a Action like Allow, Block make up a Rule.
	- Request based rule
		- Ex: Source IP of a request, SQL like code in request
		- WAF IP set match rule statements can block requests from specific IP ranges.
	- Rate based rule
		- Ex: # of requests exceeds 100 requests in 10 minutes
		- Rate based rules can automatically block IP addresses that exceed a request threshold over a given time.
- Association
	- Entity to which WAF is associated with.
	- Ex: ALB, CloudFront, API Gateway, App Sync. - WAF can front and protect these assets.
		- WAF cannot front EC2.
- Web ACL
	- Contains Rules, Rules statements and Associations.
	- Applies rules on incoming packets and decides to allow or block.
	- Web ACL logging can be enabled at WAF console.
		- These logs can be sent to S3 via Kinesis Data Firehose.
- Rule groups
	- Contains multiple rules
	- Part of multiple Web ACLs.
- Rule types
	- Custom own rules - Written and managed by AWS consumer
	- AWS managed rules - Written and managed by AWS
	- Rules from market place - WAF Rules written by 3rd party
- Rule priority
	- Every rule is assigned a priority.
	- If a request matches a "P0" rule, then none of the other rules will inspect the request.


---
