
### Application failure

- https://kubernetes.io/docs/tasks/debug/debug-application/
- The db service name mentioned in the pod should be exact match to the db service's name.
- The db service's endpoint should point to the ip of db pod and to the port (target port) number on which the db pod is listening on
- The web services endpoint should point to the ip of web pod and to the port (target port) number on which the web pod is listening on
	- The selector used in service should match the label specified in the corresponding pod
- The db credential mentioned in the pod/deployment should be correct
- The password set for db on the db pod should match the db credential with which web pod is connecting the db service with
- The node port should be correct on the web service


---
