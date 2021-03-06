## Fully Automatic deployment of NexentaEdge Container-Converged environment
This procedure describes Enterprise grade mechanism to deploy Containers for production usage. Deployment is pre-checking environment and ensures that destination target meets requirements. It also sets most optimal configuration and provides a way to select profile whie deploying.

### Step 1: Install NEDEPLOY management tool
NexentaEdge defines set of well described Chef Cookbooks which provides easy way to deploy complex cluster infrastructure excluding possible human error.

To enable NEDEPLOY tool set the following or similar alias:
```
alias nedeploy="docker run --network host -it nexenta/nedge-nedeploy /opt/nedeploy/nedeploy"
```

### Step 2: Designing cluster network

From a physical perspective, a NexentaEdge cluster is a collection of server devices connected via a high-performance 10,25,40,etc Gigabit switch. From a logical perspective, a NexentaEdge cluster consists of data nodes and gateway nodes running as containers that communicate over a Replicast network. The cluster provides storage services over an external network using the protocols that NexentaEdge supports. A NexentaEdge deployment consists of a single physical cluster and one or more logical cluster namespaces. Each logical cluster namespace may have multiple different tenants.

Figure below shows the components of a NexentaEdge cluster:

![fig1: deplyoment](https://raw.githubusercontent.com/nexenta/nedge-dev/master/images/deployment.png)

A NexentaEdge cluster consists of the following components. A given device may have multiple roles assigned to it; for example, a server may be configured as a data node, gateway node and management controller.

* Data node containers - The data nodes collectively provide the storage for the NexentaEdge cluster. Objects are broken into chunks and distributed across the data nodes using the Replicast protocol. The set of data nodes where the chunks are stored or retrieved is determined based on server load and capacity information. Data nodes may be configured with interfaces to an IPv6 Replicast network for data distribution and storage and to an IPv4 network (either an external network or dedicated management network) for initial configuration with the NEDEPLOY tool and subsequent administration with the NEADM tool. After initial configuration, data nodes require only a connection to the Replicast network, since administration of the data nodes is done by the deployment workstation via the management controller node, which has connectivity to both the management network and the Replicast network.

* Gateway node containers - Gateway nodes provide the connection between external clients and the data stored in the NexentaEdge cluster. Gateway nodes accept and respond to client requests, translating them into actions performed in the NexentaEdge cluster. Gateway nodes are provisioned with interfaces to the external network, Replicast network, and management network (if different from the external network). When you configure a NexentaEdge cluster, you indicate which storage service(s) you want to provide for a given tenant, then specify which should be the gateway nodes for that cluster/tenant/service combination.

* Replicast network - The Replicast network is an isolated IPv6 VLAN used for communication and data transfer among the data nodes and gateway nodes in the NexentaEdge cluster. The Replicast protocol provides the means for efficient storage and retrieval of data in the cluster.

* Deployment workstation container - The deployment workstation is the system from which you deploy and configure the NexentaEdge software to the other nodes. NexentaEdge uses the Chef environment for installation. You deploy NexentaEdge using a Chef Solo instance packaged with the NexentaEdge software.  To deploy NexentaEdge to the nodes in the cluster using the NEDEPLOY tool, the deployment workstation must have IPv4 network connectivity to the nodes, either through a management network or an external network.

* Management controller node - A management controller (typically co-resident with Data or Gateway container) is a node that translates external cluster-wide behavior into internal component-specific configuration, and may provide the connection between the deployment workstation and the data nodes in the Replicast network. At least one of the nodes in the cluster must be a management controller. Management controllers need to have network connectivity to both the deployment workstation and to the other nodes in the cluster.

* External network - External clients store and retrieve data in the NexentaEdge cluster by communicating with gateway nodes on the external network. The external network may use IPv4 or IPv6 and is likely to carry traffic unrelated to NexentaEdge.

* Management network - To aid in deploying and administering the NexentaEdge cluster, you may elect to place the deployment workstation and data nodes in a dedicated IPv4 management network. The NEDEPLOY and NEADM tools, running on the deployment workstation, send configuration information to and receive status information from the data nodes over this network.

* External clients - External clients are end-user application containers that access data stored in the NexentaEdge cluster via gateway nodes. External clients access data in the cluster using APIs of the storage services NexentaEdge supports: Docker volumes (NBD, NFS), Direct NFSv3,v4,v4.1, Block iSCSI/HA, Swift/S3/S3S Object.

### Step 3: Selecting most optimal operational profile

TODO

### Step 4: Deploying nodes across cluster

TODO
