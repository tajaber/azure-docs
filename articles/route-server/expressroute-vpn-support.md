---
title: Azure Route Server support for ExpressRoute and Azure VPN
description: Learn about how Azure Route Server exchanges routes between network virtual appliances (NVA), Azure ExpressRoute gateways, and Azure VPN gateways.
services: route-server
author: halkazwini
ms.service: route-server
ms.topic: conceptual
ms.date: 06/05/2023
ms.author: halkazwini
ms.custom: template-concept, engagement-fy23
---

# Azure Route Server support for ExpressRoute and Azure VPN

Azure Route Server supports not only third-party network virtual appliances (NVA) running on Azure but also seamlessly integrates with ExpressRoute and Azure VPN gateways. You don’t need to configure or manage the BGP peering between the gateway and Azure Route Server. You can enable route exchange between the gateways and Azure Route Server by enabling [branch-to-branch](quickstart-configure-route-server-portal.md#configure-route-exchange) in Azure portal. If you prefer, you can use [Azure PowerShell](quickstart-configure-route-server-powershell.md#route-exchange) or [Azure CLI](quickstart-configure-route-server-cli.md#configure-route-exchange) to enable the route exchange with the Route Server.

> [!WARNING]
> When you create or delete an Azure Route Server in a virtual network that contains a virtual network gateway (ExpressRoute or VPN), expect downtime until the operation is complete. If you have an ExpressRoute circuit connected to the virtual network where you're creating or deleting the Route Server, the downtime doesn't affect the ExpressRoute circuit or its connections to other virtual networks.

## How does it work?

When you deploy an Azure Route Server along with a virtual network gateway and an NVA in a virtual network, by default Azure Route Server doesn’t propagate the routes it receives from the NVA and virtual network gateway between each other. Once you enable **branch-to-branch** in Route Server, the virtual network gateway and the NVA exchange their routes.

For example, in the following diagram:

* The SDWAN appliance receives from Azure Route Server the route of *On-premises 2*, which is connected to ExpressRoute circuit, along with the route of the virtual network.

* The ExpressRoute gateway receives from Azure Route Server the route of *On-premises 1*, which is connected to the SDWAN appliance, along with the route of the virtual network.

:::image type="content" source="./media/expressroute-vpn-support/expressroute-with-route-server.png" alt-text="Diagram showing ExpressRoute gateway and SDWAN NVA exchanging routes through Azure Route Server.":::

You can also replace the SDWAN appliance with Azure VPN gateway. Since Azure VPN gateway and ExpressRoute are fully managed, you only need to enable the route exchange for the two on-premises networks to talk to each other.

> [!IMPORTANT] 
> Azure VPN gateway must be configured in [**active-active**](../vpn-gateway/vpn-gateway-activeactive-rm-powershell.md) mode and have the ASN set to 65515.
>

:::image type="content" source="./media/expressroute-vpn-support/expressroute-and-vpn-with-route-server.png" alt-text="Diagram showing ExpressRoute gateway and VPN gateways exchanging routes through Azure Route Server.":::

> [!IMPORTANT] 
> When the same route is learned over ExpressRoute, Azure VPN or an SDWAN appliance, the ExpressRoute network will be preferred.
>

## Next steps

- Learn more about [Azure Route Server](route-server-faq.md).
- Learn how to [configure Azure Route Server](quickstart-configure-route-server-powershell.md).
- Learn more about [Azure ExpressRoute and Azure VPN coexistence](../expressroute/how-to-configure-coexisting-gateway-portal.md).
