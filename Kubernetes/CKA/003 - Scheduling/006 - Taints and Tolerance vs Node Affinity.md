
### Taints and Tolerance vs Node Affinity

- Taints and Tolerations along with Node Affinity can be used together to completely dedicate nodes for specific pods
	- First use Taints and Tolerations to prevent other pods from landing in the nodes
	- Next use Node Affinity to prevent pods in question to be placed on other nodes


---
