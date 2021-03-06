<properties
   pageTitle="Add or remove nodes to a standalone Service Fabric cluster | Microsoft Azure"
   description="Learn how to add or remove nodes to an Azure Service Fabric cluster on a physical or virtual machine running Windows Server, which could be on-premises or in any cloud."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="dkshir;chackdan"/>


# Add or remove nodes to a standalone Service Fabric cluster running on Windows Server

After you have [created your standalone Service Fabric cluster on Windows Server machines](service-fabric-cluster-creation-for-windows-server.md), your business needs may change so that you might need to add or remove multiple nodes to your cluster. This article provides detailed steps to achieve this goal.


## Add nodes to your cluster

1. Prepare the VM/machine you want to add to your cluster by following the steps mentioned in the [Prepare the machines to meet the prerequisites for cluster deployment](service-fabric-cluster-creation-for-windows-server.md#preparemachines) section.
2. Plan which fault domain and upgrade domain you are going to add this VM/machine to.
3. Remote desktop (RDP) into the VM/machine that you want to add to the cluster.
4. Copy or [download the standalone package for Service Fabric for Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) to this VM/machine and unzip the package.
5. Run Powershell as an administrator, and navigate to the location of the unzipped package.
6. Run *AddNode.ps1* Powershell with the parameters describing the new node to add. The example below adds a new node called VM5, with type NodeType0, IP address 182.17.34.52 into UD1 and FD1. The *ExistingClusterConnectionEndPoint* is a connection endpoint for a node already in the existing cluster. For this endpoint, you can choose the IP address of *any* node in the cluster.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1 -AcceptEULA

```

## Remove nodes from your cluster

1. Depending on the Reliablity level chosen for the cluster, you cannot remove the first n (3/5/7/9) nodes of the primary node type
2. Running RemoveNode command on a dev cluster is not supported.
2. Remote desktop (RDP) into the VM/machine that you want to remove from the cluster.
2. Copy or [download the standalone package for Service Fabric for Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) and unzip the package to this VM/machine.
3. Run Powershell as an administrator, and navigate to the location of the unzipped package.
4. Run *RemoveNode.ps1* in PowerShell. The example below removes the current node from the cluster. The *ExistingClientConnectionEndpoint* is a client connection endpoint for any node that will remain in the cluster. Choose the IP address and the endpoint port of *any* **other node** in the cluster. This **other node** will in turn update the cluster configuration for the removed node. 

```
.\RemoveNode.ps1 -ExistingClientConnectionEndpoint 182.17.34.50:19000
```

Please note that even after removing a node, it may show up as being down in queries and SFX. This is a known defect and will be fixed in an upcoming release. 


## Next steps
- [Configuration settings for standalone Windows cluster](service-fabric-cluster-manifest.md)
- [Secure a standalone cluster on Windows using Windows security](service-fabric-windows-cluster-windows-security.md)
- [Secure a standalone cluster on Windows using X509 certificates](service-fabric-windows-cluster-x509-security.md)
- [Create a standalone Service Fabric cluster with Azure VMs running Windows](service-fabric-cluster-creation-with-windows-azure-vms.md)
