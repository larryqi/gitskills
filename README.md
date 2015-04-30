# OpenStack Juno - Flat Networking
@(OpenStack)[OpenStack,Juno,flat network]


----------

[toc]
## Preface
I spent much of time trying to figure out the best design for our use case. I've ended up with a **flat networking model**  that minimizes complexity and maximizes performances. The design uses two networks, one for management and control of **OpenStack**, and one for virtual machine traffic. To support two networks, our compute nodes (the servers that run the hypervisor that will host the virtual machines) will require **two** network cards.

![Alt text](./1429107975710.png)

>**Note:**
> The *VMs* will be deployed on one big flat network segment that is accessible by the users, so no *NAT* and no *floating IPs* are required.  Both subnets are fully routed to the larger network and so are accessible by the users. Although this would not be considered secure.

As you can see in the diagram above, we’ve got two types of nodes in our deployment.  The controller node and one or more compute nodes.  The controller node will host the core components of ***OpenStack***, which include:
- **Database Service** ( *MySQL* )
- **Messaging Service** ( *RabbitMQ* )
- **Identity Service** ( *Keystone* )
- **Image Service** ( *Glance* )
- **Block Storage** ( *Cinder* )
- **Web Portal** ( *Horizon* )
- **Compute API** ( *Nova-api* )

The compute nodes will run the rest of the components that we need:
- **Compute Components** ( *Nova-compute* )
- **Networking** ( *Nova-network* )
- **Metadata Service** ( *Nova-api-metadata* )

## My Setup
OK, so I've got two network segments,one for management and one for virtual machines. Each has an  associated *IP* range.  My management network is shared with other existing infrastructure in my data center, however, the virtual machine segment must be dedicated to ***OpenStack***, otherwise we’ll probably create *IP* address conflicts.  My networks are:
```
- Management(192.168.1.0/24 - router address: 192.168.1.1)
- Virtual Machines(192.168.2.0/24 - router address:192.168.2.1)
```
All of my nodes( controller and compute ) will be connected to the management network using their first NIC(`eth0`) and have an *IP* address assigned. The compute nodes will also be connected to the virtual machine network using their second NIC(`eth1`), however no *IP* address will be assigned. Instead, a virtual switch (actually a *linux bridge*) will be linked to `eth1`, and virtual machines will be "plugged" into this virtual switch. The virtual machine will be given `192.168.2.X` *IP* addresses so that they can communicate over the virtual machine network.

ALL of my nodes will run `Ubuntu 14.04.2 LTS (GNU/Linux 3.13.0-48-generic x86_64)`
## Building the Controller Node


----------


## Building the Compute Node


----------


## Testing 


----------


## Feedback && Suggestions
- Author: **Qi, Lin**
- Mail:  <qilinsjtu@gmail.com>


----------


## References
-1. [OpenStack Havana – Flat Networking](http://behindtheracks.com/2013/12/openstack-havana-flat-networking/)
-2. [OpenStack Installation Guide for Ubuntu 14.04](http://docs.openstack.org/juno/install-guide/install/apt/content/)
-3. [OpenStack Networking – FlatManager and FlatDHCPManager](https://www.mirantis.com/blog/openstack-networking-flatmanager-and-flatdhcpmanager/)
