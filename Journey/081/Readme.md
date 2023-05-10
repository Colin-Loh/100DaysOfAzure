# Azure Landing Zone (ALZ)

## Introduction

Starting out my career as a Platform Engineer, I was put onto a project that required me to develop an Azure Landing Zone.
This made me realize how important it is for any organization to know what an Azure Landing Zone is and how it can help them.

A quick explanation of Azure Landing Zone:
* Landing Zone is an environment to host your workload that is pre-provisioned through Infrastructure as Code (Terraform).
* Uses a pre-defined cloud services to add capabilities such as Networking / Identity Management / Governance / Security / Management (see 5 key principles below).
* Provide a stable place for application and workflow to run and scale.

5 Key Principles for ALZ: 
* Networking services > Azure Express Route
* Set identity management rule > Azure Active Directory
* Governance environment > Azure Policy
* Security control > Azure Sentinel
* Manage environment > Azure Monitor 

## Customer Profile

The customer requested a hybrid connectivity to on-premises location. Azure Landing Zone can support enterprise application portloios and add hybrid connecitivty with ExpressRoute or VPN when required.

A hub and spoke network topology allows you to create a central Hub Vnet that contains shared networking components. 
This includes: 
* ExpressRoute
* VPN Gateway
* Firewall 

The Hub Vnet can later be used by the spoke Vnet via Vnet Peering to centralize connectivity in you environment. 

* Gateway transit in Vnet peering allows spokes to have connecivity to/from on-premises via ExpressRoute or VPN
* Transitive connectivity across spokes can be implemented by deploying User Defined Routes (UDR) on the spokes and using Azure Firewall or an NVA in the hub.

## Use Case

If the customer decide to migrate on-premise application to Azure that require hybrid connectivity, you would simply need to create
the Connectivity Subscription and place it into the Platform > Connectivity Management Group as shown below.

<p align="center">
<img src="Example01.png">
</p>

Other additional things that will be deployed are the following:
* A scalable Management Group hierarchy aligned to core platform capabilities using managed Azure RBAC and Azure Policy.
* Azure Policies that enable automomy for the platform and landing zone such as enforcing resource types, naming, properties for security and compliance.
* Integrate Azure Environment with Azure DevOps by providing PA Token to create new repository
* Azure Subscription specifically dedicated for connectivity. 
* Landing Zone subscription for connected application and resources including a virtual network that will be connected to the hub via Vnet Peering.

## Cloud Research

- ✍️ Document your trial and errors. Share what you tried to learn and understand about the cloud topic or while completing micro-project.
- 🖼️ Show as many screenshot as possible so others can experience in your cloud research.

## Try yourself

✍️ Add a mini tutorial to encourage the reader to get started learning something new about the cloud.

### Step 1 — Summary of Step

![Screenshot](https://via.placeholder.com/500x300)

### Step 1 — Summary of Step

![Screenshot](https://via.placeholder.com/500x300)

### Step 3 — Summary of Step

![Screenshot](https://via.placeholder.com/500x300)

## ☁️ Cloud Outcome

✍️ (Result) Describe your personal outcome, and lessons learned.

## Next Steps

✍️ Describe what you think you think you want to do next.

## Social Proof

✍️ Show that you shared your process on Twitter or LinkedIn

[link](link)
