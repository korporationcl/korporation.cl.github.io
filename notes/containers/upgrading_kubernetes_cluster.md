# Upgrading procedure

1. Check the version you are running (client) and server:

```
sudo kubectl version --short
Client Version: v1.12.7
Server Version: v1.12.8
```

Look for the release you want to ugprade (I believe we can't go directly to 1.14.2 so upgrading to 1.13.6)
```
apt-cache policy kubeadm
```


unlock kubeadm and upgrade it to `1.13.6`:

```
apt-mark unhold kubeadm && \
apt-get update && apt-get install -y kubeadm=1.13.6-00 && \
apt-mark hold kubeadm
```

check the release you are now:

```
kubeadm version
kubeadm version: &version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.6", GitCommit:"abdda3f9fefa29172298a2e42f5102e777a8ec25", GitTreeState:"clean", BuildDate:"2019-05-08T13:51:38Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
```

Now you can run upgrade plan and check if you can upgrade your cluster:

```
kubeadm upgrade plan
[preflight] Running pre-flight checks.
[upgrade] Making sure the cluster is healthy:
[upgrade/config] Making sure the configuration is correct:
[upgrade/config] Reading configuration from the cluster...
[upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
[upgrade] Fetching available versions to upgrade to
[upgrade/versions] Cluster version: v1.12.8
[upgrade/versions] kubeadm version: v1.13.6
I0605 05:51:12.742339   11326 version.go:237] remote version is much newer: v1.14.2; falling back to: stable-1.13
[upgrade/versions] Latest stable version: v1.13.6
[upgrade/versions] Latest version in the v1.12 series: v1.12.9

Components that must be upgraded manually after you have upgraded the control plane with 'kubeadm upgrade apply':
COMPONENT   CURRENT       AVAILABLE
Kubelet     3 x v1.12.7   v1.12.9

Upgrade to the latest version in the v1.12 series:

COMPONENT            CURRENT   AVAILABLE
API Server           v1.12.8   v1.12.9
Controller Manager   v1.12.8   v1.12.9
Scheduler            v1.12.8   v1.12.9
Kube Proxy           v1.12.8   v1.12.9
CoreDNS              1.2.2     1.2.6
Etcd                 3.2.24    3.2.24

You can now apply the upgrade by executing the following command:

	kubeadm upgrade apply v1.12.9

_____________________________________________________________________

Components that must be upgraded manually after you have upgraded the control plane with 'kubeadm upgrade apply':
COMPONENT   CURRENT       AVAILABLE
Kubelet     3 x v1.12.7   v1.13.6

Upgrade to the latest stable version:

COMPONENT            CURRENT   AVAILABLE
API Server           v1.12.8   v1.13.6
Controller Manager   v1.12.8   v1.13.6
Scheduler            v1.12.8   v1.13.6
Kube Proxy           v1.12.8   v1.13.6
CoreDNS              1.2.2     1.2.6
Etcd                 3.2.24    3.2.24

You can now apply the upgrade by executing the following command:

	kubeadm upgrade apply v1.13.6
```

so at this stage we execute the upgrade:

```
kubeadm upgrade apply v1.13.6
[preflight] Running pre-flight checks.
[upgrade] Making sure the cluster is healthy:
[upgrade/config] Making sure the configuration is correct:
[upgrade/config] Reading configuration from the cluster...
[upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
[upgrade/apply] Respecting the --cri-socket flag that is set with higher priority than the config file.
[upgrade/version] You have chosen to change the cluster version to "v1.13.6"
[upgrade/versions] Cluster version: v1.12.8
[upgrade/versions] kubeadm version: v1.13.6
[upgrade/confirm] Are you sure you want to proceed with the upgrade? [y/N]: y
```

now upgrade the kubelet on the master node:

```
apt-mark unhold kubelet && \
apt-get update && apt-get install -y kubelet=1.13.6-00 && \
apt-mark hold kubelet
```

at the end the control plane (master node) should be upgraded at this point:

```
kubectl get nodes

NAME                           STATUS   ROLES    AGE   VERSION
1c.mylabserver.com   Ready    master   15d   v1.13.6
2c.mylabserver.com   Ready    <none>   15d   v1.12.7
3c.mylabserver.com   Ready    <none>   15d   v1.12.7
```

2. Upgrade kubectl in all the worker nodes


```
apt-mark unhold kubectl && \
apt-get update && apt-get install -y kubectl=1.13.6-00 && \
apt-mark hold kubectl
```

3. Upgrade kubeadm and kubelet in all the worker nodes

```
apt-mark unhold kubelet kubeadm
apt-get update
apt-get install -y kubelet=1.13.6-00 kubeadm=1.13.6-00
apt-mark hold kubelet kubeadm
```

4. At the end the output should be similar to this:

```
kubectl get nodes
NAME                           STATUS   ROLES    AGE   VERSION
1c.mylabserver.com   Ready    master   15d   v1.13.6
2c.mylabserver.com   Ready    <none>   15d   v1.13.6
3c.mylabserver.com   Ready    <none>   15d   v1.13.6
```

Full documentation for this procedure is at https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade-1-13/