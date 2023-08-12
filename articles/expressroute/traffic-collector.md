---
title: Enable flow logging using Azure ExpressRoute Traffic Collector (Preview)
description: Learn about ExpressRoute Traffic Collector and the different use cases where this feature will be helpful.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/02/2022
ms.author: duau
ms.custom: references_regions
---

# Enable flow logging using ExpressRoute Traffic Collector (Preview)

ExpressRoute Traffic Collector enables sampling of network flows sent over your ExpressRoute Direct circuits. Flow logs get sent to a [Log Analytics workspace](../azure-monitor/logs/log-analytics-overview.md) where you can create your own log queries for further analysis, export the data to any visualization tool or SIEM (Security Information and Event Management) of your choice. Flow logging can be enabled for both private peering and Microsoft peering with ExpressRoute Traffic Collector.

> [!IMPORTANT]
> ExpressRoute Traffic Collector is currently in PREVIEW.
> See the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) for legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

:::image type="content" source="./media/traffic-collector/main-diagram.png" alt-text="Diagram of ExpressRoute traffic collector in an Azure environment.":::

## Use cases

Flow logs can help you derive various traffic insights. Most common use cases are:

### Network monitoring

- Monitor Azure private peering and Microsoft peering traffic
- Near real-time visibility into network throughput and performance
- Perform network diagnosis
- Capacity forecasting

### Monitor network usage and cost optimization

- Analyze traffic trends by filtering sampled flows by IP, port or by applications
- Top talkers for a source IP, destination IP or applications
- Optimize network traffic expenses by analyzing traffic trends

### Network forensics analysis

- Identify compromised IPs by analyzing all the associated network flows
- Export flow logs to a SIEM (Security Information and Event Management) tool to monitor, correlate events, generate security alerts

## Flow log collection and sampling

ExpressRoute Traffic Collector enables flow collection for Azure private peering and Microsoft peering. Flow logs are collected every minute. All packets collected for a given flow gets aggregated and imported into a Log Analytics workspace for further analysis. During flow collection, not every packet is captured into its own flow record. ExpressRoute Traffic Collector uses a sampling rate of 1:4096, meaning 1 out of every 4096 packets gets captured. Therefore, sampling rate short flows (in total bytes) may not get collected. This sampling size doesn't affect network traffic analysis when sampled data is aggregated over a longer period of time. Flow collection time and sampling rate are fixed and can't be changed.

## Flow log schema

| Column                 | Type     | Description                                                                          |
| ---------------------- | -------- | ------------------------------------------------------------------------------------ |
| ATCRegion              | string   | ExpressRoute Traffic Collector (ATC) deployment region.                              |
| ATCResourceId          | string   | Azure resource ID of ExpressRoute Traffic Collector (ATC).                           |
| BgpNextHop             | string   | Border Gateway Protocol (BGP) next hop as defined in the routing table.              |
| DestinationIp          | string   | Destination IP address.                                                              |
| DestinationPort        | int      | TCP destination port.                                                                |
| Dot1qCustomerVlanId    | int      | Dot1q Customer VlanId.                                                               |
| Dot1qVlanId            | int      | Dot1q VlanId.                                                                        |
| DstAsn                 | int      | Destination Autonomous System Number (ASN).                                          |
| DstMask                | int      | Mask of destination subnet.                                                          |
| DstSubnet              | string   | Destination subnet of destination IP.                                                |
| ExRCircuitDirectPortId | string   | Azure resource ID of Express Route Circuit's direct port.                            |
| ExRCircuitId           | string   | Azure resource ID of Express Route Circuit.                                          |
| ExRCircuitServiceKey   | string   | Service key of Express Route Circuit.                                                |
| FlowRecordTime         | datetime | Timestamp (UTC) when Express Route Circuit emitted this flow record.                 |
| Flowsequence           | long     | Flow sequence of this flow.                                                          |
| IcmpType               | int      | Protocol type as specified in IP header.                                             |
| IpClassOfService       | int      | IP Class of service as specified in IP header.                                       |
| IpProtocolIdentifier   | int      | Protocol type as specified in IP header.                                             |
| IpVerCode              | int      | IP version as defined in the IP header.                                              |
| MaxTtl                 | int      | Maximum time to live (TTL) as defined in the IP header.                              |
| MinTtl                 | int      | Minimum time to live (TTL) as defined in the IP header.                              |
| NextHop                | string   | Next hop as per forwarding table.                                                    |
| NumberOfBytes          | long     | Total number of bytes of packets captured in this flow.                              |
| NumberOfPackets        | long     | Total number of packets captured in this flow.                                       |
| OperationName          | string   | The specific ExpressRoute Traffic Collector operation that emitted this flow record. |
| PeeringType            | string   | Express Route Circuit peering type.                                                  |
| Protocol               | int      | Protocol type as specified in IP header.                                             |
| \_ResourceId           | string   | A unique identifier for the resource that the record is associated with              |
| SchemaVersion          | string   | Flow record schema version.                                                          |
| SourceIp               | string   | Source IP address.                                                                   |
| SourcePort             | int      | TCP source port.                                                                     |
| SourceSystem           | string   |                                                                                      |
| SrcAsn                 | int      | Source Autonomous System Number (ASN).                                               |
| SrcMask                | int      | Mask of source subnet.                                                               |
| SrcSubnet              | string   | Source subnet of source IP.                                                          |
| \_SubscriptionId       | string   | A unique identifier for the subscription that the record is associated with          |
| TcpFlag                | int      | TCP flag as defined in the TCP header.                                               |
| TenantId               | string   |                                                                                      |
| TimeGenerated          | datetime | Timestamp (UTC) when the ExpressRoute Traffic Collector emitted this flow record.    |
| Type                   | string   | The name of the table                                                                |

## Region availability

ExpressRoute Traffic Collector is supported in the following regions:

- Central US
- East US
- East US 2
- North Central US 
- South Central US 
- West Central US 
- West US 
- West US 2 
- West US 3

## Next steps

- Learn how to [set up ExpressRoute Traffic Collector](how-to-configure-traffic-collector.md).
- [ExpressRoute Traffic Collector FAQ](../expressroute/expressroute-faqs.md#expressroute-traffic-collector).
