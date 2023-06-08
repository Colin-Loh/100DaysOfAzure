# Express Route Part 2

Assuming you have already understand the background and some key components and features that Express Route has to offer in Day 4 of our 100DaysOfCloud, we will now focus on the difference between Site-To-Site VPN and ExpressRoute. 

We will also look at how to implement ExpressRoute circuit for the first time and what do you need to know beforehand. 

![Screenshot](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*UKeZhTv7d2t0YkqpwelZkA.png)

## Why would we not use Site-To-Site VPN?

1) Site-to-site VPN is not as secure as it is travelling through the internet
2) Maximum speed of site-to-site VPN is latency with a maximum speed of 1.2Gbps

## Why ExpressRoute? 

ExpressRoute provides direct connection to Azure Services with the help of Partners / Service Providers. This is a private connection that does not pass data through the internet.

Other benefit of Express Route includes:

- Layer 3 connectivity and security standards
	- Establishment of a network connection between an on-premise and Azure cloud network 
- Connect to the Edge router of the on-premise network and Azure infrastructure 
	- This is responsible for routing and forwarding network between local and external networks such as the internet or cloud services.  
- Scalable from 50 Mbps to 10 Gbps 
- 99.9% availability SLA across the entire connection 

![Screenshot](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*w7KTzrzwcVkfgXipOs_NrQ.png)

## How to Set It Up?

### Step 1 - Create a Gateway Subnet 

Create a Gateway Subnet within the Vnet which act as a bridge between Vnet and other networks such as on-premises network or other Vnet.  

### Step 2 - Create an ExpressRoute Circuit

Create an Express Route Circuit, we need to look for a partner / service provider that will extend the connectivity of Azure Data Center to a specific Meet Me / Peering Location close to the Azure Data Center.  

### Step 3 - Find a Service Provider

Network Engineers will have to identify a local vendor / service provider that has a capacity to extend the connectivity to the Meet Me / Peering Location.  
  
### Step 4 - Provide the Service Provider with the Service Key

![Screenshot](https://learn.microsoft.com/en-us/azure/expressroute/media/expressroute-howto-circuit-portal-resource-manager/expressroute-create-configuration.png)

The above diagram, we can decide who will be our partner / service provider under the provider *. Once we have created our express route circuit with our declared provider. We will have a **Service Key** 

The partner does not know that you have picked them. You will need to provide them a Service Key and provide it to the service provider and they will be aware of that express route and start billing.

### Step 5- Connect the Gateway Subnet from step 1 with the Express Route. 

This can only happen when the service provider provision from the service provider.

## Use Cases
  
### ExpressRoute:

- Private and Reliable Connectivity: data is private and not exposed to the public internet. 
- Enhanced Performance: Scalable network speed and lower latency.
- Improved Control and Security: ExpressRoute provide SLA up to 99.99% resiliency with the help of active-active connections. 

### VPN Gateway:

- Small business for prototyping, development, test, labs, and small production workloads.
- Connect on-premises data centres to Azure virtual networks through a *site-to-site* connection.
- Connect individual devices to Azure virtual networks through a *point-to-site* connection.
- Connect Azure virtual networks to other Azure virtual networks through a *network-to-network* connection.
- Suitable when lower-speed bandwidth is within an acceptable tolerance for day-to-day usage.