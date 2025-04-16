### Overview

Incident - Unauthorized access, use, modification or disclosure of information

Incident response - Handle situation to limit damage, reduce recovery time and cost.

Event driven security - Once an event is detected, response mechanism is triggered to remediate event.

Preventive controls examples -> WAF, IAM, Network firewalls

Detective controls examples -> CloudTrail, CloudWatch, Inspector, Detective

### Incident response plan in cloud

- Preparation phase
	- Security controls are in place
	- Logging enabled via CloudTrail, CloudWatch, VPC Flowlogs etc.
	- Use AWS organization to separate accounts to reduce blast radius.
- Detection phase
	- Behaviour based rules set up to identify/detect breaches
		- Ex: login at 3 a.m, increased number of sign-in failures.
- Containment phase
	- Setup automation to contain resources
	- Use AWS CLI or SDK for quick containment using pre-defined security groups which are highly restrictive.
- Investigation phase
	- Use CloudWatch logs to investigate.
	- Use AWS Config to see any configuration changes in infrastructure.
- Recovery phase
	- Setup automation to deploy application using pre-defined AMIs.
- Lessons learnt and RCA
	- What was lacking?
	- What worked?
	- What can be improved?

### Incident response plan when AWS access key / secret key is leaked

- AWS attaches "AWSCompromisedKeyQuarantinev2" policy to reduce actions that can be performed with this key.
	- AWS continuously scans public repositories for access and secret keys.
	- AWS sends e-mail and raises a support ticket, notifying the user of the key leak and remediation steps.
- Incident response plan
	- Determine the access associated with the key.
		- Is it for root user or IAM user? What are the policies attached with the key?
	- Invalidate credentials -> Deactivate key -> Review breakages -> Delete key -> Add Deny all policy to user.
	- Invalidate any temporary credentials issued with the exposed key.
		- STS tokens can be created from access/secret keys, with validity ranging from 15 minutes to 36 hours.
	- Restore access with new credentials
		- New user with access/secret keys.
		- Using IAM roles instead of access/secret keys is a preferred security practice.
	- Review AWS account for all actions which look suspicious.

### Incident response for compromised EC2 instance

- Capture instance metadata.
- Enable termination protection.
- Lock/Isolate the instance
	- Should not be able to communicate outside
	- Accessible only for forensic purposes
	- Automation is important to apply Security Groups with heavy restrictions immediately. ("SG-NoOutbound")
- Detach the instance from any auto scaling groups(ASG).
- De-register instance from any elastic load balancer (ELB).
- Take EBS snapshot and memory dump for forensic purposes
	- EBS snapshot takes a snapshot of all files in the file system.
	- Automate memory capture using SSM run command.
- Perform forensics
	- Create new EC2 instance and attach the EBS snapshot for analysis.
	- Terminate instance and perform offline investigation.
	- To delete all authorized public ssh keys, login as root, search for "authorized_keys" on disk and delete them.
	- Perform online investigation by using the memory dump and analysing the network traffic.
- Tag the EC2 instance.
- Automate all process using Lambda.
- RCA is released

### Incident response for compromised S3 bucket

- Identify from Guard Duty on what is compromised.
- Identify source from Amazon Detective, CloudTrail.
	- Verify if source is authorized.
- Secure S3 bucket
	- Block public access
	- Update S3 bucket policies and user policies to restrict access
	- Configure VPC endpoints for S3
	- Configure S3 access via pre-signed urls.
	- Configure S3 access points.
	- Configure S3 ACLs.

### Incident response for compromised ECS cluster

- Identify from Guard Duty on what is compromised.
- Identify source of malicious activity -> container, image, tasks, etc.
- Isolate the impacted tasks
	- Deny all ingress/egress traffic.
- Evaluate presence of malware.

### Incident response for compromised standalone container

- Identify malicious activity from Guard Duty
- Identify the malicious container
	- Deny all ingress/egress traffic
- Suspend all process in container (pause)
- Stop the container and analyse EBS snapshots from Guard Duty (snapshot retention feature)
- Evaluate presence of malware.

### Incident response for compromised RDS instances

- Identify affected DB instance and DB user using Guard Duty.
- If not legitimate behaviour
	- Restrict network access via Security Groups and NACLs.
	- Restrict DB access for suspected DB user.
- Rotate suspected DB user credentials.
- Review DB audit logs to identify leaked data.
- Secure RDS DB instance
	- Use Secrets Manager to rotate DB password.
	- Use "IAM DB authentication" to manage DB user's access without passwords.

### Incident response for compromised AWS credentials

- Identify IAM user using Guard Duty.
- Rotate exposed AWS credentials.
- Invalidate temporary credentials by attaching an "explicit deny" policy to the affected user, with a STS date condition. (STS date condition is set to a date post which if a STS is created, it will have explicit deny to all actions)
- Deactivate and delete credentials.
- Check CloudTrail log for unauthorised activity.
- Review AWS resources to detect unauthorised activity, such as delete, create etc.
- Verify AWS account information.

### Incident response for compromised IAM roles

- Identify IAM role using Guard Duty.
- Invalidate temporary credentials by attaching an "explicit deny" policy to the affected user, with a STS date condition. (STS date condition is set to a date post which if a STS is created, it will have explicit deny to all actions)
- Revoke access for the identity to the linked AD.
- Check CloudTrail log for unauthorised activity.
- Review AWS resources to detect unauthorised activity, such as delete, create etc.
- Verify AWS account information.

### Incident response for compromised account

- Rotate and delete exposed AWS keys.
- Rotate and delete any unauthorised IAM user credentials.
	- Rotate existing IAM user's passwords.
- Rotate and delete all EC2 key pairs.
- Check CloudTrail log for unauthorised activity.
- Review AWS resources to detect unauthorised activity, such as delete, create etc.
- Verify AWS account information.


---
