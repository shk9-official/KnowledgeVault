
AWS sends abuse reports to its customers when their AWS resources are detected to be used for abusive purposes.
- There is a requirement to respond within 24 hours, else account will be blocked.
- We can report any activity to AWS trust and safety team.

AWS abuse complaint - This is a complaint reported by affected AWS customers.

If AWS abuse notice received on a list of EC2 instances
- Remove them from auto-scaling groups(ASG) and elastic load balancer (ELB)
- Capture EC2 metadata before making any changes.