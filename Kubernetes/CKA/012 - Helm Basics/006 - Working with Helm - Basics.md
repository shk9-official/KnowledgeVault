
### Helm Basics

- `helm --help`
- `helm repo --help`
- `helm repo update --help`
- `helm rollback --help`
- `helm search hub wordpress`
	- Search the repository for Wordpress application
	- ![helmsearch.png](Attachments/helmsearch.png)
- Deploy application
	- Add bitnami repository
		- `helm repo add bitnami https://charts.bitnami.com/bitnami`
	- Deploy application
		- `helm install my-release bitnami/wordpress`
		- Application is deployed as a release
	- ![helmdeploy.png](Attachments/helmdeploy.png)
	- List all releases
		- `helm list`
	- To remove the deployed application
		- `helm uninstall my-release`
	- ![helmlistuninstall.png](Attachments/helmlistuninstall.png)
	- To list existing repositories
		- `helm repo list`
	- To update repository
		- `helm repo update`
	- ![helmrepocmds.png](Attachments/helmrepocmds.png)


---
