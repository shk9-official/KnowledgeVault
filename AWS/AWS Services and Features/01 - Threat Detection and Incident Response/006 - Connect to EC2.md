
### EC2 Key Pairs

- A public-private key pair is generated, for password less authentication.
- Public key is stored in EC2 instance under "authorized_keys".
- Private key is used by the user to authenticate.
- ED25519 and 2048, SHA2RSA is supported.
- If you delete the key from EC2 console, the the public key does not get deleted from the EC2 instance. (Continues to be there under "authorised_keys").
- If launching an EC2 instance from an AMI with a existing public key, the existing public key will reside along with any new public keys configured on the new EC2 instance.

### Remediating exposed EC2 key pairs

- Remove all public keys in each EC2 instance.
- Create a new key pair and add all public key to all EC2 instances.
- Use SSM Run Command to automate add/delete public keys on EC2 instances.
- Run the following command to figure out which EC2 instances (list) were launched with the key pair
	- "aws ec2 describe-instances --filters Name=key-name, values=name_of_key_pair"

### EC2 instance connect

- From browser connect to a EC2 by selecting EC2 instance connect.
- EC2 instance connect API creates a short lived (for 60 secs) public-private key pair.
- The Public key is pushed to instance MetaData.
- EC2 instance connect connects using the temperory private key.
- EC2 instance checks for authorised keys under "authorized_keys" and also from instance Metadata.
- Since the temporary public key is available in instance MetaData, the connection is successful.
- All connections are logged in CloudTrail.
- An inbound security group allowing EC2 instance connect IP is allowed.

### EC2 serial console

- Used for troubleshooting boot, network configurations.
- Connect keyboard, mouse directly into EC2 instance's serial port.
- Used in Nitro based EC2 instances
	- Username and password are used for authentication.
- Disabled by default.

### AWS System Manager Session Manager to connect to EC2 Instances

- Launch VPC Interface Endpoint for the AWS systems manager in the same VPC as EC2 instances.
- Configure a Security Group that allows inbound for VPC's CIDR range on port 443.
	- Attach this Security Group to VPC Interface Endpoint.
- Edit the EC2 instance Security Group to allow outbound to VPC Interface Endpoint's Security Group on port 443.

### Connect to EC2 instance with a lost SSH key pair

#### Using EC2 user data

- EC2 user data runs every time a EC2 boots up.
- Create a new key pair and copy the public key to EC2 instance.
- Stop the instance, update the EC2 user data with
	- Username
	- Public key created in previous step
- Start the instance and connect with Private key.
- Delete the EC2 user data.

#### Using Systems Manager

- SSM agent installed on EC2 instance with IAM role.
- SSM trigger "AWSsupport-ResetAccess" automation
	- Will create new key pair.
	- Private key will be stored in Parameter store.
	- Public key will be stored in EC2 instance.
- Note: SSM sessions manager keeps history of commands used in remote terminal to EC2.

#### Using EC2 instance connect

- EC2 instance connect agent must be installed.
- Connect to EC2 instance via EC2 instance connect.
- Create and configure new key pair.

#### Using Serial Console

- Connect to instance without a network connection.
- Used for Nitro based instances.
- Must be enabled at AWS account level.

#### Using EBS volume swap

- Create a new key pair.
- Stop EC2 instance.
- Detach EBS volume.
- Attach the above EBS volume to another EC2 instance.
- Configure new key pair.
- Reattach the EBS volume to the original EC2 instance.

### Connect to a Windows EC2 instance with a lost password

#### Using EC2Launchv2

- EC2Launchv2 service must be running.
- Detach the EBS volume.
- Attach the volume to another EC2 instance.
- Delete "%pgmData%/Amazon/EC2Launch/state/.run-once"
- Re-attach to original EC2 instance and restart.
	- You will be able to set new password.

#### Using EC2 config

- EC2 config service must be running.
- Works on Windows AMIs before Windows server 2016.
- Detach EBS volumes.
- Attach EBS volume to another EC2 instance.
- Modify "ProgramFiles/Amazon/EC2configservice/settings/config.xml".
	- Set "EC2password to enabled".
- Re-attach to original EC2 instance and restart.
	- You will be able to set new password.

#### Using EC2 Launch

- For Windows server 2016 or later AMIs which doesn't include EC2Launchv2.
- Detach EBS volume.
- Attach volume to another EC2 instance.
- Download and install EC2Rescue tool for Windows server.
- Select offline instance option -> Diagnose and rescue -> Reset Administrator password
- Reattach to original instance and restart.

#### Using Systems Manager

- SSM agent must be installed.
- Method 1 -> Using AWSSupport-RunEC2RescueForWindowsTool
- Method 2 -> Using AWSSupport-ResetAccess
- Method 3 -> Using AWS-Run Powershell script

### EC2 Rescue Tool for Linux

- Install manually or run SSM automation "AWSSupport-TroubleshootSSM".
- Diagnose and troubleshoot common issues.
- Upload results to S3 or AWS support use case
	- Collect system utilization reports
	- Collect log details
	- Automatically remediate system problems.

### EC2 Rescue Tool for Windows (Server 2008R2 or later)

- Diagnose and troubleshoot common issues.
- Install manually or run "AWSSupport-RunEC2RescueForWindowsTool".
- Use "AWSSupport-ExecuteEC2Rescue" automation to troubleshoot connectivity issues.
- Uploads results to S3.
  
### Use cases for using EC2 Rescue tool
- Instance connectivity issues
- OS boot issues
- Gather OS logs and configuration files.
- Common OS issues
- Perform restore.


---



