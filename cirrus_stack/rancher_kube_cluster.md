recommended firewall rules

REF: https://rancher.com/docs/rancher/v2.x/en/installation/requirements/
Full breakdown for each role; etcd, control plane, worker REF: https://rancher.com/docs/rke/latest/en/os/#ports
For Master Node with Rancher is managing the clusters

# Ports for Communication with Downstream Clusters
To communicate with downstream clusters, Rancher requires different ports to be open depending on the infrastructure you are using.

For example, if you are deploying Rancher on nodes hosted by an infrastructure provider, port 22 must be open for SSH.

The following diagram depicts the ports that are opened for each cluster type.

![port-communications.svg](../../_resources/port-communications.svg)


## Port Requirements for the Rancher Management Plane


Basic Port Requirements

The following tables break down the port requirements for inbound and outbound traffic:

### Inbound Rules for Rancher Nodes
| PROTOCOL | PORT | SOURCE | DESCRIPTION |
|:-------- |:----:|:------ |:----------- |   
|TCP	|80	 |Load balancer/proxy that does external SSL termination	|Rancher UI/API when external SSL termination is used
|TCP	|443 | etcd nodes, controlplane nodes, worker nodes, hosted, imported Kubernetes, any source that needs to be able to use the Rancher UI or API |Rancher agent, Rancher UI/API, kubectl
### Outbound Rules for Rancher Nodes
| PROTOCOL | PORT | DESTINATION	| DESCRIPTION |
|:-------- |:----:|:----------- |:----------- |   
|TCP	|22	|Any node IP from a node created using Node Driver	|SSH provisioning of nodes using Node Driver
|TCP	|443	|35.160.43.145/32, 35.167.242.46/32, 52.33.59.17/32	|git.rancher.io (catalogs)
|TCP	|2376	|Any node IP from a node created using Node driver	|Docker daemon TLS port used by Docker Machine
|TCP	|6443	|Hosted/Imported Kubernetes API	|Kubernetes API server
**Note:** Rancher nodes may also require additional outbound access for any external authentication provider which is configured (LDAP for example).

## Additional Port Requirements for Nodes in an RKE Kubernetes Cluster
You will need to open additional ports to launch the Kubernetes cluster that is required for a high-availability installation of Rancher.

If you follow the Rancher installation documentation for setting up a Kubernetes cluster using RKE, you will set up a cluster in which all three nodes have all three roles: etcd, controlplane, and worker. In that case, you can refer to this list of requirements for each node with all three roles.

If you installed Rancher on a Kubernetes cluster that doesn’t have all three roles on each node, refer to the port requirements for the Rancher Kubernetes Engine (RKE). The RKE docs show a breakdown of the port requirements for each role.

### Inbound Rules for Nodes with All Three Roles: etcd, Controlplane, and Worker
| PROTOCOL | PORT | SOURCE | DESCRIPTION |
|:-------- |:----:|:------ |:----------- | 
|TCP	|22	|Linux worker nodes only, and any network that you want to be able to remotely access this node from.	|Remote access over SSH
|TCP	|80	|Any source that consumes Ingress services	|Ingress controller (HTTP)
|TCP	|443	|Any source that consumes Ingress services	|Ingress controller (HTTPS)
|TCP	|2376	|Rancher nodes	|Docker daemon TLS port used by Docker Machine (only needed when using Node Driver/Templates)
|TCP	|2379	|etcd nodes and controlplane nodes	|etcd client requests
|TCP	|2380	|etcd nodes and controlplane nodes	|etcd peer communication
|TCP	|3389	|Windows worker nodes only, and any network that you want to be able to remotely access this node from.	|Remote access over RDP
|TCP	|6443	|etcd nodes, controlplane nodes, and worker nodes	|Kubernetes apiserver
|UDP	|8472	|etcd nodes, controlplane nodes, and worker nodes	Canal/Flannel |VXLAN overlay networking
|TCP	|9099	|the node itself (local traffic, not across nodes)	|Canal/Flannel livenessProbe/readinessProbe
|TCP	|10250	|controlplane nodes	|kubelet
|TCP	|10254	|the node itself (local traffic, not across nodes)	|Ingress controller livenessProbe/readinessProbe
|TCP/UDP	|30000-32767	|Any source that consumes NodePort services	|NodePort port range
### Outbound Rules for Nodes with All Three Roles: etcd, Controlplane, and Worker
| PROTOCOL | PORT | SOURCE | DESTINATION | DESCRIPTION |
|:-------- |:----:|:------ |:----------- |:----------- |  
|TCP	|22	|RKE node	|Any node configured in Cluster Configuration File	|SSH provisioning of node by RKE
|TCP	|443	|Rancher nodes	|Rancher agent	|
|TCP	|2379	|etcd nodes	|etcd client requests	|
|TCP	|2380	|etcd nodes	|etcd peer communication	|
|TCP	|6443	|RKE node	|controlplane nodes	|Kubernetes API server |
|TCP	|6443	|controlplane nodes	|Kubernetes API server	
|UDP	|8472	|etcd nodes, controlplane nodes, and worker nodes	|Canal/Flannel VXLAN overlay networking	 |
|TCP	|9099	|the node itself (local traffic, not across nodes)	|Canal/Flannel livenessProbe/readinessProbe	|
|TCP	|10250	|etcd nodes, controlplane nodes, and worker nodes	|kubelet	|
|TCP	|10254	|the node itself (local traffic, not across nodes)	|Ingress controller livenessProbe/readinessProbe	|




