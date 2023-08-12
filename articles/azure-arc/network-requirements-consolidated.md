---
title: Azure Arc network requirements
description: A consolidated list of network requirements for Azure Arc features and Azure Arc-enabled services. Lists endpoints, ports, and protocols.
ms.date: 02/01/2023
ms.topic: reference
---

# Azure Arc network requirements

This article lists the endpoints, ports, and protocols required for Azure Arc-enabled services and features.

[!INCLUDE [network-requirement-principles](includes/network-requirement-principles.md)]

## Azure Arc-enabled Kubernetes endpoints

Connectivity to the Arc Kubernetes-based endpoints is required for all Kubernetes-based Arc offerings, including:

- Azure Arc-enabled Kubernetes
- Azure Arc-enabled App services
- Azure Arc-enabled Machine Learning
- Azure Arc-enabled data services (direct connectivity mode only)

[!INCLUDE [network-requirements](kubernetes/includes/network-requirements.md)]

For an example, see [Quickstart: Connect an existing Kubernetes cluster to Azure Arc](kubernetes/quickstart-connect-cluster.md).

## Azure Arc-enabled data services

This section describes requirements specific to Azure Arc-enabled data services, in addition to the Arc-enabled Kubernetes endpoints listed above.

[!INCLUDE [network-requirements](data/includes/network-requirements.md)]

For more information, see [Connectivity modes and requirements](data/connectivity.md).

## Azure Arc-enabled servers

Connectivity to Arc-enabled server endpoints is required for:

- Azure Arc-enabled SQL Server
- Azure Arc-enabled VMware vSphere (preview) <sup>*</sup>
- Azure Arc-enabled System Center Virtual Machine Manager (preview) <sup>*</sup>
- Azure Arc-enabled Azure Stack (HCI) (preview) <sup>*</sup>

   <sup>*</sup>Only required for guest management enabled.

[!INCLUDE [network-requirements](servers/includes/network-requirements.md)]

For examples, see [Connected Machine agent network requirements](servers/network-requirements.md)].

## Azure Arc resource bridge (preview)

This section describes additional networking requirements specific to deploying Azure Arc resource bridge (preview) in your enterprise. These requirements also apply to Azure Arc-enabled VMware vSphere (preview) and Azure Arc-enabled System Center Virtual Machine Manager (preview).

[!INCLUDE [network-requirements](resource-bridge/includes/network-requirements.md)]

## Azure Arc-enabled System Center Virtual Machine Manager (preview)

Azure Arc-enabled System Center Virtual Machine Manager (SCVMM) also requires:

| **Service** | **Port** | **URL** | **Direction** | **Notes**|
| --- | --- | --- | --- | --- |
| SCVMM management Server | 443 | URL of the SCVMM management server | Appliance VM IP and control plane endpoint need outbound connection. | Used by the SCVMM server to communicate with the Appliance VM and the control plane. |

For more information, see [Overview of Arc-enabled System Center Virtual Machine Manager (preview)](system-center-virtual-machine-manager/overview.md).

## Azure Arc-enabled VMware vSphere (preview)

Azure Arc-enabled VMware vSphere also requires:

| **Service** | **Port** | **URL** | **Direction** | **Notes**|
| --- | --- | --- | --- | --- |
| vCenter Server | 443 | URL of the vCenter server  | Appliance VM IP and control plane endpoint need outbound connection. | Used to by the vCenter server to communicate with the Appliance VM and the control plane.|

For more information, see [Support matrix for Azure Arc-enabled VMware vSphere (preview)](vmware-vsphere/support-matrix-for-arc-enabled-vmware-vsphere.md).

## Additional endpoints

Depending on your scenario, you may need connectivity to other URLs, such as those used by the Azure portal, management tools, or other Azure services. In particular, review these lists to ensure that you allow connectivity to any necessary endpoints:

- [Azure portal URLs](../azure-portal/azure-portal-safelist-urls.md)
- [Azure CLI endpoints for proxy bypass](/cli/azure/azure-cli-endpoints)
