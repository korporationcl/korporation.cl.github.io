# Minikube

The easiest way to try this Kubernetes is installing MiniKube (OSX). The instructions are pretty much well documented https://github.com/kubernetes/minikube
but is summarised as:

```
# Virtualbox is a dependency of minikube
brek cask install virtualbox
brew cask install minikube
```

to start the cluster run `minikube start`:

```
$ minikube start
😄  minikube v1.1.0 on darwin (amd64)
💿  Downloading Minikube ISO ...
 131.28 MB / 131.28 MB [============================================] 100.00% 0s
🔥  Creating virtualbox VM (CPUs=2, Memory=2048MB, Disk=20000MB) ...
🐳  Configuring environment for Kubernetes v1.14.2 on Docker 18.09.6
💾  Downloading kubelet v1.14.2
💾  Downloading kubeadm v1.14.2
🚜  Pulling images ...
🚀  Launching Kubernetes ...
⌛  Verifying: apiserver proxy etcd scheduler controller dns
🏄  Done! kubectl is now configured to use "minikube"
```

then to visualise the status of it, execute `minikube dashboard`:

```
$ minikube dashboard

🔌  Enabling dashboard ...
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:57120/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy/ in your default browser...
```

# DIY

If you want more control and understand what you are doing, do it the hard way and setup every component manually. More documentation to be added.

```
# Install Docker PGP key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Add docker repository
sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

# Install Kubernetes PGP
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -


# Add Kubernetes repository
cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update

# Install Docker, kubelet, kubeadm and kubectl
sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu kubelet=1.13.5-00 kubeadm=1.13.5-00 kubectl=1.13.5-00
sudo apt-mark hold docker-ce kubelet kubeadm kubectl

# Enable this kernel flag for networking | iptables magic
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

# Initialise the cluster (run on the master node only once)
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
# Additional setup needed
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# Apply flannel
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml
```

then from one of the other nodes, join the cluster:

```
kubeadm join INSERT_TOKEN_HERE
```

finally, you should see the results from:

```
kubectl get nodes
```