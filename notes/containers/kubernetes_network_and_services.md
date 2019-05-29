# Kubernetes Services and Network

- Each Pod has assigned an unique IP address. There is only one flat network where communication happens between Pods.
- A service object is an object that will be replicated across the whole cluster.

Example of creating a service `myservice.yaml`:

```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  type: NodePort
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30080
```

`selector` here applies | selects everything to the pods that has been tagged as "MyApp". `Protocol` mentions the communication between the Pod in the cluster is happening using TCP port `80`. `NodePort` is the exposed port for external clients that will be listening in all the nodes. `TargetPort` is port that Docker container exposes inside the Pod.

Now we create the service:

```
$ kubectl create -f myservice.yaml
```

List all services:

``
$ kubectl get services
```

# KubeProxy

- Handles all the traffic associated with a service.

# References

[1] https://kubernetes.io/docs/concepts/services-networking/service/
[2] https://kubernetes.io/docs/concepts/services-networking/service/#virtual-ips-and-service-proxies