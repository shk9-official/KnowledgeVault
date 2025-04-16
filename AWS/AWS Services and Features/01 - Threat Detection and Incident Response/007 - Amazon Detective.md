
### Amazon Detective

- Amazon Detective is used to investigate and identify root cause of security issues or suspicious activity.
	- It uses machine learning and graphs in backend.
- Input for Detective
	- VPC flow logs
	- CloudTrail
	- Guard Duty
- It can analyse and visualise logs
- Deep investigation based on user, resource IP, success/failure of access, IAM roles, etc.
- It is a Threat Detection service.
- It can detect if CloudTrail is disabled and who disabled it.
- Flow
	- All findings from Guard Duty are sent to Detective
	- Triage the findings in Detective, and determine if they are False +ves or -ves.
	- Scope all affected resources
	- Response and remediate the finding.

---
