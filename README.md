# 5gc-kubernetes
5gc-kubernetes is based on free5gc for creating 5G core network environment by kubernetes.

## Environment
- Ubuntu 18.04
- kernel version 5.0.0-23-generic
- docker 18.09.5
- kubernetes 1.19.5
- go1.14.4 linux/amd64
- gcc 7.5.0

## Installation
**Apply Mutil-CNI**

Build multus cni
```
git clone https://github.com/k8snetworkplumbingwg/multus-cni.git
cd multus-cni
cat ./images/multus-daemonset.yml | kubectl apply -f -
```

Build openvswitch
```
apt update
apt install openvswitch-switch -y
git clone https://github.com/kubevirt/ovs-cni.git
kubectl apply -f ovs-cni-master/examples/ovs-cni.yml
```

Download 5gc-kubernetes repository
```
cd ~
git clone https://github.com/Silverbullet9/5gc-kubernetes.git
```


Build and config bridge
```
ovs-vsctl add-br br1
ifconfig br1 up
ifconfig br1 192.168.36.254 netmask 255.255.255.0
kubectl apply -f ~/5gc-kubernetes/ovs-net-crd.yaml
```

**Build Images**

```
git clone https://github.com/free5gc/free5gc-compose.git
cd free5gc-compose
make base
docker-compose build
```

**Build GTP5G module**

```
git clone https://github.com/PrinzOwO/gtp5g.git
cd gtp5g
make
sudo make install
```

**Build database**

```
cd ~/5gc-kubernetes
kubectl apply -f mongo/mongo-pv.yaml
kubectl apply -f mongo/mongo-pvc.yaml
kubectl apply -f unix-daemonset.yaml
kubectl apply -f mongo/5gc-mongo.yaml
kubectl expose deployment db --port=27017
```

**Build network functions**

```
kubectl apply -f 5gc-configmap.yaml
kubectl apply -f ./networkFunctions
```

**Build webui**

```
kubectl apply -f webui/webui.yaml
kubectl apply -f webui/webui-service.yaml
```
