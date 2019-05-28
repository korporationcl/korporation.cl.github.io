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
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376
```

Now we execute:

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