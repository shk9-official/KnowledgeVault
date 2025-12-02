
### Helm 2 vs Helm 3

- Helm has a CLI which helps perform Kubernetes operations
- Helm 2 used tiller to talk to cluster
- ![helm2.png](Attachments/helm2.png)
- Helm 3 spoke to cluster directly
- ![helm3.png](Attachments/helm3.png)
- Helm 2 compares the current chart to the previous chart, to determine where to rollback to
	- If something is changed manually, helm 2 will not be able to identify
- ![helm2revisionsrollback-1.png](Attachments/helm2revisionsrollback-1.png)
- ![helm2revisionsrollback-2.png](Attachments/helm2revisionsrollback-2.png)
- Whereas Helm 3 compares the live state to the current chart and the previous chart, to determine where to rollback
	- This is called as 3-way strategic merge patch
- ![helm3revisionsrollback.png](Attachments/helm3revisionsrollback.png)


---

