# Cluster Architecture

Kubernetes is composed of a master node (Control Plane) and workers (where containers are running). Some key aspects from each role are:

## Control Plane (master node):

- API Server: communication hub for all the components, exposes Kubernetes API to all the other components.
- Scheduler: is in charge of assigning applications to a node/worker
- Controller Manager: maintains the cluster, handle node failures, number of pods, etc.
- etcd: is data store for cluster configuration.

The master node will never run applications on it, only worker nodes, however master node can be replicated for high availability purposes.


## Worker node

Is responsible for running the application and services you need:

- kubelet: runs and manage the containers on the node. It talks to the API server. It also talks to the container runtime (Docker)
- kubeproxy: load balance traffic between application components.
- container runtime: the program that runs the containers such as Docker (there are others rkt, containerd).


## References

[1] https://kubernetes.io/docs/concepts/overview/components/
