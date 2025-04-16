
### IAM Credentials Report

- Works at AWS account level
- Lists all your AWS account's users and status of their various credentials.


### IAM Access Advisor

- Works at user level
- Shows service permissions granted to a user and when those were last accessed.
	- This can be used to revise and fine tune IAM policies.

### IAM Access Analyzer

- Finds out resources shared externally like,
	- S3
	- IAM roles
	- KMS keys
	- Lambda
	- SQS
	- Secrets Manager
- Define zone of trust -> AWS Account or AWS Organization
- Access outside the zone of trust will generate findings.
	- Ex: If a S3's zone of trust is set to roles, users in a specific AWS account, any access from a client endpoint or a VPC endpoint will generate a finding.

### IAM Access Analyzer Policy Validation

- Validates policy against grammar and best practices.
- Provides Security warnings on the policy.
- Provides actionable recommendations.

### IAM Access Analyzer Policy Generation

- Generates policy based on access activity.
- CloudTrail logs are analysed for this every 90 days.


---
