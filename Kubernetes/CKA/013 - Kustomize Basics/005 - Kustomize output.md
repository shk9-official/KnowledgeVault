
### Kustomize output

- `kustomize build` command does not deploy
- ![kustomizebuild.png](Attachments/kustomizebuild.png)
- To build and deploy
	- `kustomize build k8s/ | kubectl apply -f -`
	- `kubectl apply -k k8s/`
- ![kustomizeapply.png](Attachments/kustomizeapply.png)
- Delete with Kustomize
	- `kustomize build k8s/ | kubectl delete -f -`
	- `kubectl delete -k k8s/`
- ![kustomizedelete.png](Attachments/kustomizedelete.png)


---
