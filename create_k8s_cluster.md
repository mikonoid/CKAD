## Prerequisites

3 VMs 

1 - Master node
2 - worker node
3 - worker node

## Install Kubernetes 

### On all 3 nodes

Add repositories

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

```

Install Docker and K8S

```
sudo apt-get update

sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu kubelet=1.14.5-00 kubeadm=1.14.5-00 kubectl=1.14.5-00

sudo apt-mark hold docker-ce kubelet kubeadm kubectl

```

Enable iptables bridge

```
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf

sudo modprobe br_netfilter

sudo sysctl -p

```

### On Master node only

Initialize the cluster

Change from 0 to 1

```
sudo nano /proc/sys/net/ipv4/ip_forward 

```
Change it also in /etc/sysctl.conf and run 

```
sudo sysctl -p

```


Initialize the cluster

```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```


Setup kubeconfig

```
mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Install Flannel networking 

```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml
```

### On each worker node

Copy from initializing master output following items:
- IP 
- token 
- hash

Run command for node joining to master

```
sudo kubeadm join $master_ip:6443 --token $token --discovery-token-ca-cert-hash $hash
```

### Check k8s cluster

On master node

```
kubectl get nodes
```

You should see all nodes with a status of Ready

```
NAME                      STATUS   ROLES    AGE   VERSION
mkivanov1.vagrant.local   Ready    master   54m   v1.13.4
mkivanov2.vagrant.local   Ready    <none>   49m   v1.13.4
mkivanov3.vagrant.local   Ready    <none>   49m   v1.13.4
```






