---
title: Configure VMware HCX in Azure VMware Solution
description: Configure the on-premises VMware HCX Connector for your Azure VMware Solution private cloud. 
ms.topic: tutorial
ms.service: azure-vmware
ms.date: 12/05/2022
ms.custom: engagement-fy23
---

# Configure on-premises VMware HCX Connector

Once you've [installed the VMware HCX add-on](install-vmware-hcx.md), you're ready to configure the on-premises VMware HCX Connector for your Azure VMware Solution private cloud.  

In this tutorial, you'll learn how to do the following tasks:

* Pair your on-premises VMware HCX Connector with your Azure VMware Solution HCX Cloud Manager
* Configure the network profile, compute profile, and service mesh
* Check the appliance status and validate that migration is possible

After you complete these steps, you'll have a production-ready environment for creating virtual machines (VMs) and migration. 

## Prerequisites

- [VMware HCX Connector](install-vmware-hcx.md) has been installed.

- VMware HCX Enterprise is now available and supported on Azure VMware Solution at no extra cost. HCX Enterprise is automatically installed for all new HCX add-on requests, and existing HCX Advanced customers can upgrade to HCX Enterprise using the Azure portal. 

- If you plan to [enable VMware HCX MON](https://docs.vmware.com/en/VMware-HCX/4.1/hcx-user-guide/GUID-0E254D74-60A9-479C-825D-F373C41F40BC.html), make sure you have:  

   - NSX-T Data Center or vSphere Distributed Switch (vDS) on-premises for HCX Network Extension (vSphere Standard Switch not supported)

   - One or more active stretched network segments

- [VMware software version requirements](https://docs.vmware.com/en/VMware-HCX/4.1/hcx-user-guide/GUID-54E5293B-8707-4D29-BFE8-EE63539CC49B.html) have been met.

- Your on-premises vSphere environment (source environment) meets the [minimum requirements](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-54E5293B-8707-4D29-BFE8-EE63539CC49B.html).

- [Azure ExpressRoute Global Reach](tutorial-expressroute-global-reach-private-cloud.md) is configured between on-premises and Azure VMware Solution private cloud ExpressRoute circuits.

- [All required ports](https://ports.vmware.com/home/VMware-HCX) are open for communication between on-premises components and Azure VMware Solution private.

- [Define VMware HCX network segments](plan-private-cloud-deployment.md#define-vmware-hcx-network-segments).  The primary use cases for VMware HCX are workload migrations and disaster recovery.

- [Review the VMware HCX Documentation](https://docs.vmware.com/en/VMware-HCX/4.1/hcx-user-guide/GUID-BFD7E194-CFE5-4259-B74B-991B26A51758.html) for information on using HCX.

## Add a site pairing

In your data center, you can connect or pair the VMware HCX Cloud Manager in Azure VMware Solution with the VMware HCX Connector.

> [!IMPORTANT]
> As per the [Azure VMware Solution limits](https://learn.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-vmware-solution-limits) the maximum site pairs is 25 and maximum service meshes is 10 in a single HCX manager system, this includes inbound and outbound site pairings.

1. Sign in to your on-premises vCenter Server, and under **Home**, select **HCX**.

1. Under **Infrastructure**, select **Site Pairing** and select the **Connect To Remote Site** option (in the middle of the screen).

1. Enter the Azure VMware Solution HCX Cloud Manager URL or IP address that you noted earlier `https://x.x.x.9` and the credentials for a user that holds the CloudAdmin role in your private cloud. Then select **Connect**.

   > [!NOTE]
   > To successfully establish a site pair:
   > * Your VMware HCX Connector must be able to route to your HCX Cloud Manager IP over port 443.
   >
   > * A service account from your external identity source, such as Active Directory, is recommended for site pairing connections. For more information about setting up separate accounts for connected services, see [Access and Identity Concepts](./concepts-identity.md).

   You'll see a screen showing that your VMware HCX Cloud Manager in Azure VMware Solution and your on-premises VMware HCX Connector are connected (paired).

   :::image type="content" source="media/tutorial-vmware-hcx/site-pairing-complete.png" alt-text="Screenshot showing the site pairing of the HCX Manager in Azure VMware Solution and the VMware HCX Connector.":::

## Create network profiles

VMware HCX Connector deploys a subset of virtual appliances (automated) that requires multiple IP segments. When you create your network profiles, you use the IP segments you identified during the [planning phase](plan-private-cloud-deployment.md#define-vmware-hcx-network-segments).  You'll create four network profiles:

   - Management
   - vMotion
   - Replication
   - Uplink
   
   > [!NOTE]
   > * Azure VMware Solution connected via VPN should set Uplink Network Profile MTU's to 1350 to account for IPSec overhead.
   > * Azure VMWare Solution defaults to 1500 MTU and is sufficient for most ExpressRoute implementations.
   >   * If your ExpressRoute provider does not support jumbo frame, MTU may need to be lowered in ExpressRoute setups as well.
   >   * Changes to MTU should be performed on both HCX Connector (on-premises) and HCX Cloud Manager (Azure VMware Solution) network profiles.

1. Under **Infrastructure**, select **Interconnect** > **Multi-Site Service Mesh** > **Network Profiles** > **Create Network Profile**.

   :::image type="content" source="media/tutorial-vmware-hcx/network-profile-start.png" alt-text="Screenshot showing where to create a network profile in the vSphere Client." lightbox="media/tutorial-vmware-hcx/network-profile-start.png":::

1. For each network profile, select the network and port group, provide a name, and create the segment's IP pool. Then select **Create**.

   :::image type="content" source="media/tutorial-vmware-hcx/example-configurations-network-profile.png" alt-text="Screenshot showing the details for a new network profile." lightbox="media/tutorial-vmware-hcx/example-configurations-network-profile.png":::

For an end-to-end overview of this procedure, view the [Azure VMware Solution: HCX Network Profile](https://www.youtube.com/embed/O0rU4jtXUxc) video.

## Create a compute profile

1. Under **Infrastructure**, select **Interconnect** > **Compute Profiles** > **Create Compute Profile**.

   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-create.png" alt-text="Screenshot that shows the selections for starting to create a compute profile." lightbox="media/tutorial-vmware-hcx/compute-profile-create.png":::

1. Enter a name for the profile and select **Continue**.

   :::image type="content" source="media/tutorial-vmware-hcx/name-compute-profile.png" alt-text="Screenshot that shows the entry of a compute profile name and the Continue button." lightbox="media/tutorial-vmware-hcx/name-compute-profile.png":::

1. Select the services to enable, such as migration, network extension, or disaster recovery, and then select **Continue**.

1. In **Select Service Resources**, select one or more service resources (clusters) to enable the selected VMware HCX services.

1. When you see the clusters in your on-premises datacenter, select **Continue**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-service-resource.png" alt-text="Screenshot that shows selected service resources and the Continue button." lightbox="media/tutorial-vmware-hcx/select-service-resource.png":::

1. From **Select Datastore**, select the datastore storage resource for deploying the VMware HCX Interconnect appliances. Then select **Continue**.

   When multiple resources are selected, VMware HCX uses the first resource selected until its capacity is exhausted.

   :::image type="content" source="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png" alt-text="Screenshot that shows a selected data storage resource and the Continue button." lightbox="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png":::

1. From **Select Management Network Profile**, select the management network profile that you created in previous steps. Then select **Continue**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-management-network-profile.png" alt-text="Screenshot that shows the selection of a management network profile and the Continue button." lightbox="media/tutorial-vmware-hcx/select-management-network-profile.png":::

1. From **Select Uplink Network Profile**, select the uplink network profile you created in the previous procedure. Then select **Continue**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-uplink-network-profile.png" alt-text="Screenshot that shows the selection of an uplink network profile and the Continue button." lightbox="media/tutorial-vmware-hcx/select-uplink-network-profile.png":::

1. From **Select vMotion Network Profile**, select the vMotion network profile that you created in previous steps. Then select **Continue**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-vmotion-network-profile.png" alt-text="Screenshot that shows the selection of a vMotion network profile and the Continue button." lightbox="media/tutorial-vmware-hcx/select-vmotion-network-profile.png":::

1. From **Select vSphere Replication Network Profile**, select the replication network profile that you created in previous steps. Then select **Continue**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-replication-network-profile.png" alt-text="Screenshot that shows the selection of a replication network profile and the Continue button." lightbox="media/tutorial-vmware-hcx/select-replication-network-profile.png":::

1. From **Select Distributed Switches for Network Extensions**, select the switches containing the virtual machines to be migrated to Azure VMware Solution on a layer-2 extended network. Then select **Continue**.

   > [!NOTE]
   > If you are not migrating virtual machines on layer-2 (L2) extended networks, you can skip this step.

   :::image type="content" source="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png" alt-text="Screenshot that shows the selection of distributed virtual switches and the Continue button." lightbox="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png":::

1. Review the connection rules and select **Continue**.

   :::image type="content" source="media/tutorial-vmware-hcx/review-connection-rules.png" alt-text="Screenshot that shows the connection rules and the Continue button." lightbox="media/tutorial-vmware-hcx/review-connection-rules.png":::

1. Select **Finish** to create the compute profile.

   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-done.png" alt-text="Screenshot that shows compute profile information." lightbox="media/tutorial-vmware-hcx/compute-profile-done.png":::

For an end-to-end overview of this procedure, view the [Azure VMware Solution: Compute Profile](https://www.youtube.com/embed/e02hsChI3b8) video.

## Create a service mesh

>[!IMPORTANT]
>Make sure port UDP 4500 is open between your on-premises VMware HCX Connector 'uplink' network profile addresses and the Azure VMware Solution HCX Cloud 'uplink' network profile addresses.  (500 UDP was previously required in legacy versions of HCX.  See https://ports.vmware.com for latest information)


1. Under **Infrastructure**, select **Interconnect** > **Service Mesh** > **Create Service Mesh**.

   :::image type="content" source="media/tutorial-vmware-hcx/create-service-mesh.png" alt-text="Screenshot of selections to start creating a service mesh." lightbox="media/tutorial-vmware-hcx/create-service-mesh.png":::

1. Review the pre-populated sites, and then select **Continue**.

   > [!NOTE]
   > If this is your first service mesh configuration, you won't need to modify this screen.

1. Select the source and remote compute profiles from the drop-down lists, and then select **Continue**.

   The selections define the resources where VMs can consume VMware HCX services.

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-source.png" alt-text="Screenshot that shows selecting the source compute profile." lightbox="media/tutorial-vmware-hcx/select-compute-profile-source.png":::

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-remote.png" alt-text="Screenshot that shows selecting the remote compute profile." lightbox="media/tutorial-vmware-hcx/select-compute-profile-remote.png":::

1. Review services that will be enabled, and then select **Continue**.

1. In **Advanced Configuration - Override Uplink Network profiles**, select **Continue**.

   Uplink network profiles connect to the network through which the remote site's interconnect appliances can be reached.

1. In **Advanced Configuration - Network Extension Appliance Scale Out**, review and select **Continue**.

   You can have up to eight VLANs per appliance, but you can deploy another appliance to add another eight VLANs. You must also have IP space to account for the more appliances, and it's one IP per appliance.  For more information, see [VMware HCX Configuration Limits](https://configmax.vmware.com/guest?vmwareproduct=VMware%20HCX&release=VMware%20HCX&categories=41-0,42-0,43-0,44-0,45-0).

   :::image type="content" source="media/tutorial-vmware-hcx/extend-networks-increase-vlan.png" alt-text="Screenshot that shows where to increase the VLAN count." lightbox="media/tutorial-vmware-hcx/extend-networks-increase-vlan.png":::

1. In **Advanced Configuration - Traffic Engineering**, review and make any modifications that you feel are necessary, and then select **Continue**.

1. Review the topology preview and select **Continue**.

1. Enter a user-friendly name for this service mesh and select **Finish** to complete.

1. Select **View Tasks** to monitor the deployment.

   :::image type="content" source="media/tutorial-vmware-hcx/monitor-service-mesh.png" alt-text="Screenshot that shows the button for viewing tasks.":::

   When the service mesh deployment finishes successfully, you'll see the services as green.

   :::image type="content" source="media/tutorial-vmware-hcx/service-mesh-green.png" alt-text="Screenshot that shows green indicators on services." lightbox="media/tutorial-vmware-hcx/service-mesh-green.png":::

1. Verify the service mesh's health by checking the appliance status.

1. Select **Interconnect** > **Appliances**.

   :::image type="content" source="media/tutorial-vmware-hcx/interconnect-appliance-state.png" alt-text="Screenshot that shows selections for checking the status of the appliance." lightbox="media/tutorial-vmware-hcx/interconnect-appliance-state.png":::

   >[!NOTE]
   >After establishing the service mesh, you may notice a new datastore and a new host in your private cloud. This is perfectly normal behavior after establishing a service mesh.
   >
   >:::image type="content" source="media/tutorial-vmware-hcx/hcx-service-mesh-datastore-host.png" alt-text="Screenshot showing the HCX service mesh datastore and host." lightbox="media/tutorial-vmware-hcx/hcx-service-mesh-datastore-host.png":::

The HCX interconnect tunnel status should indicate **UP** and in green. You're ready to migrate and protect Azure VMware Solution VMs using VMware HCX. Azure VMware Solution supports workload migrations (with or without a network extension). So you can still migrate workloads in your vSphere environment, along with on-premises creation of networks and deployment of VMs onto those networks. For more information, see the [VMware HCX Documentation](https://docs.vmware.com/en/VMware-HCX/index.html). 



For an end-to-end overview of this procedure, view the [Azure VMware Solution: Service Mesh](https://www.youtube.com/embed/COY3oIws108) video.


## Next steps

Now that you've configured the HCX Connector, you can also learn about:

- [Create a HCX network extension](configure-hcx-network-extension.md)

- [VMware HCX Mobility Optimized Networking (MON) guidance](vmware-hcx-mon-guidance.md)

