
### Multi-container pod design patterns

![multicontainerpoddesignpattern.png](Attachments/multicontainerpoddesignpattern.png)

- Co-located containers
	- Containers running ins a pod together. No differentiation on which container starts first
	- ![colocatedcontainer.png](Attachments/colocatedcontainer.png)
- Regular Init containers
	- Initialisation steps done by init container before starting the main container. Once init container finishes, the main container starts
	- ![regularinitcontainer.png](Attachments/regularinitcontainer.png)
- Side car containers
	- Setup like an init container, but continues to run throughout the lifecycle of the pod.
	- Main application starts after the sidecar container starts.
	- Sidecar container ends after the main application ends.
	- ![sidecarcontainer.png](Attachments/sidecarcontainer.png)



---
