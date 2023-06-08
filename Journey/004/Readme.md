# Express Route - Part 1

## Introduction

In every large organisation, the most common networking compontent that they do use is ExpressRoute. Its Day 4 and I want to explore ExpressRoute indepth and identify what is it and how do we actually use it! 

ExpressRoute allows organisation to extend its on-premises network into the Microsoft Cloud over private connection with the help of a service provider. 

## Background

Lets understand a few things before we dive deep into ExpressRoute:

**The MS Backbone**
- Private network that interconnects Microsoft's data centers worldwide and provides the underlying connectivity for various Microsoft services, including Azure, Microsoft 365, and other cloud-based offerings.

**Edge Nodes and Edge Site**
- Edge nodes are like Azure FrontDoor and the point of these edge nodes they bring computing resources closer to the users or device. 
- Each Edge Site is a physical location where multiple edge nodes / servers distributed across different geographic locations.  
- These sites are strategically positioned closer to the end-users or devices they serve, enabling faster data processing and reduced latency. 
  
**Meet Me / Peering Location**
- Organizations may choose to deploy edge sites at or near peering locations for direct network connectivity and optimize data transfer between networks.
- It is a interconnection point where different Internet service providers (ISPs), content providers, or networks exchange traffic with each other.
- It is a private network that does not send data or communicate through the internet.

**ExpressRoute Direct**
- Ports in Microsoft Edge, different service provider have ports and split each other to customers.
- Express Route Direct allow customer to purchase a port to allow us to communicate with MS Backbone without the need of going through a service provider.

**Active-Active Connection**
- Resiliency, we get two connections. If there is a 1gbps circuit, we actually get 2 x 1 gbps circuits.
- There is 2 gbps due to the active-active nature of the express route.
- This will provide us resiliency / SLA. Basically we have 2 ethernet physical connection over the circuit. 

## Connectivity Options

Now that you understand some background information, lets go through the 3 types of connectivity options:

1) **Point-To-Point Ethernet Network**
- Service provider is going to drop a line to my on-premise door. It is a single line that support two virtual ethernet over it and have one physical connection. This will connect our network using a physical level from on-premises to the MS Backbone

2) **Any-To-Any VPN Network**
- Multiple data centres connecting to MPLS. Microsoft Enterprise Edge becomes a node of that MPLS and connect to any-to-any nodes. 

3) **Virtual Cross-Connection via Service Provider at a Colocation Facility (Meet Me/ Peering Location)**

## Setting Up The Express Route Circuit

![Screenshot](https://learn.microsoft.com/en-us/azure/expressroute/media/expressroute-howto-circuit-portal-resource-manager/expressroute-create-configuration.png)

**SKU**
- Standard SKU - any region specific boundary
- Premium SKU - access to anything outside of the boundary
- Local SKU - Only communicate to the region that is **LOCAL** to the Meet Me. 

**LOCAL SKU**
- Microsoft would have a meet me close to any corresponding regions data centre. If there is a local express route - establishing that connection. The boundary of that LOCAL can access is the region that is align to the Meet Me location.

**Billing**
- Ingres and egress is part of the bill that will happen at the end of the month. 

![Screenshot](https://learn.microsoft.com/en-us/azure/expressroute/media/expressroute-howto-circuit-portal-resource-manager/expressroute-circuit-overview.png)

**Service Key**
- A Express Route circuit is defined by a service key and it gives us a service key which will be shared to our service provider. 

### Express Route Peering Types

![Screenshot](https://learn.microsoft.com/en-us/azure/expressroute/media/expressroute-introduction/expressroute-connection-overview.png)

The screenshot above is from the MS Doc [here](https://learn.microsoft.com/en-us/azure/expressroute/expressroute-introduction)

There are 2 types of peering:

- **Microsoft Peering:**

All those PaaS and SaaS service that don’t live in a virtual network and are in a public IP space. They aren’t living within a virtual network.  

You cannot turn off O365 even with Microsoft Peering as it is designed to work over the internet. You will need to work with the Office team to get authorisation to work off the internet.  
  
- **Private Peering:**

Everything that can live in a virtual network and in a private IP space. 

We would need to set those peering up ourselves by establishing an **Express Route Circuit.** We have a Service Key which has a one-to-one mapping between the service key → the circuit.

## Microsoft Peering - Setting It Up
To offer services through our Express Route circuit: We need a / 29 or 2 x /30s that we own.  
  
A /30s is 4 IPs but only 2 of them are usable to due the 1st being a Network and 4th being a Broadcast. We need 2 x /30s because we have 2 active-active paths.  
  
The first usable IP Address will go to the Customer and second one going to Microsoft.   

We have to provide those for our **Public IP Space**. When we establish those dual BGP sessions to share the routes. 
  
We also need to have an ASN that we own - Autonomous System Number. We will have to publicly register it which will give 32-bit number. ASN will allow us to pass it on to our usable IP address spaces.  
  
We need to give the following:
- ASN
- BGP Peering
- Prefixes that we want to advertise via the connection - depends on how many NAT device

## Private Peering - Setting It Up

Dealing with private IP space, we need to do exactly the same thing as Microsoft peering.  
We need to have **2 x /30s in Private IP space.**  
  
We provide the 2 IP scope and our ASN to establish connectivity of our Vnet to our Express Route Circuit. We can connect the gateway to the express route circuit with the authorization key. As long as we have the Resouce ID and the Auhtorization key in the circuit, we can consume the private peering from anywhere - different subscription, tenant, region.   
  
10x Authorization key = 10x Virtual Network.  

## Hub and Spoke

We can also establish a Spoke and Hub method:  
- Hub which has connectivity to the Express Route Circuit  
- Spoke around the HUB that will use the connectivity to go and talk to on-premises  

Outbound flow, it can actually bypass and go out other IP addresses. The inbound goes through the gateway and the outbound doesn’t go through the gateway. 

## Route Filter 

We can create a route filter. A route filter allow us to tick which services that can go through our express route circuit. This includes filtering services from specific regions to control which route can have access to the internet.

Control services what we can see over our express route. When we create an express route circuit we are creating in a subscription specifically for billing purposes. note: that Express Route Circuit in not limited to that subscription. 

## Express Route - Fast Path

This allows us to bypass the gateway for the traffic. A gateway supports a certain size / flow, if we have an express route direct we have 100gpbs but we won’t have the full 100gbps to that virtual network.

A Fast Path will allow us to give us the full extend of that bandwidth. 

Are you still with me? I will slowly update this Day 4 journey with more screenshot which will hopefully make more sense to everyone including myself. I will be doing a part 2 which goes indepth how we can set it up step by step if you were to create a new ExpressRoute from scratch in any organisation.