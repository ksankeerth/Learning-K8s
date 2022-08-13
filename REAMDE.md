# Learning-K8s

I use this repository to take about notes K8s and internal components.

## CNI

CNI stands for Container Network Interface. The CNI is just a specification and different CNI plugins can be used in K8s clusters. It's also possible to use multiple CNI plugins in the same cluster. The CNI has to be installed on all the nodes of the K8s cluster.CNI stands for Container Network Interface. The CNI is a specification but differnt CNI plugins can be used in K8s clusters. It's also possible to use multiple CNI plugins in the same cluster. The CNI has to be installed on all the nodes of K8s cluster


### The Job of a CNI Plugin

The K8s will create containers when it asked or scheduler will create it self. After creating Containers, the container needs network interface in order to talk other containeir,pods, node and internet.  

The job of a CNI plugin is to attach a network interface and do the necessary configuration to allow communications between pods, nodes, containers,s and the internet.

The CNI specification mentions 3 requirements for network communication

1. All the containers can communicate each others withou NAT (Network Address Translation)
2. All the nodes can communicate with all containers (and vice versa) without NAT
3. The IP address the container see itself should be same as what others see.

Any executable satisfy the above requirements and implements the API/Spec of CNI can be considered as CNI plugins. There are many different CNI plugins for different use cases. How the CNI plugin facilitates the above-mentioned communication is not important to the K8s cluster. Different plugins use different approaches to solve the problem. For example, some use an Overlay network, some use eBPF.

## Configuration of CNI Plugin

The CNI Plugin configuration can be found under `/etc/cni/net.d/` in all the nodes of K8s cluster. A sample CNI plugin configuration is shown below.

```apache
{
  "cniVersion": "0.3.1",
  "name": "cilium",
  "type": "cilium-cni",
  "enable-debug": false,
  "log-file": "/var/run/cilium/cilium-cni.log"
}
```

The `cniVerion`, `name` and `type` are mandatory fields of the configuration. Other fields are CNI provider specific configuration. 

## Installing a CNI Plugin

The most of CNI plugin can be installed using scripts. After the installation, the cni plugin can be found in `/opt/cni/bin`. The CNI plugin is executable.
