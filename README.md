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
###### Apply Mutil-CNI
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

```


Build and config bridge
```
ovs-vsctl add-br br1
```
