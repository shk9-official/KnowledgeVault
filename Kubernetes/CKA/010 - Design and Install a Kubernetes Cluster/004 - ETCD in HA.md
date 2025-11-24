### ETCD in HA

- ETCD is a distributed reliable key-value store that is simple, secure and fast
- ETCD is distributed
	- Read happens from all instances
	- For write, a leader is elected and write happens only through it.
	- Once the leader writes, it sends a copy to all the other ETCD nodes
	- If a write request comes to the leader, it writes it.
	- If a write request comes to a follower, it forwards it to the leader for writing.
- ETCD implements distributed consensus, leader election using RAFT protocol
- A write is considered to be complete if there is quorum
	- Quorum is (N/2 + 1)
	- It is recommended to choose odd number of nodes.


---
### Important update: Kubernetes the hard way
Installing Kubernetes the hard way can help you gain a better understanding of putting together the different components manually.

An optional series on this is available on our YouTube channel here:

[https://www.youtube.com/watch?v=uUupRagM7m0&list=PL2We04F3Y_41jYdadX55fdJplDvgNGENo](https://www.youtube.com/watch?v=uUupRagM7m0&list=PL2We04F3Y_41jYdadX55fdJplDvgNGENo)

The GIT Repo for this tutorial can be found here: [https://github.com/mmumshad/kubernetes-the-hard-way](https://github.com/mmumshad/kubernetes-the-hard-way)


---

![Kubernetes-CKA-0900-Install.pdf](Attachments/Kubernetes-CKA-0900-Install.pdf)


---
