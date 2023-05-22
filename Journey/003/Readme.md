# Web app Private Connectivity to Azure SQL Database

### App Service Plan 

Imagine a multi-tenant infrastructure that has front-end / data roles / file services. Unique per customer, each ASP have a number of workers. These are the things that can auto scale / number of worker such as the VM sizes (sku). A Web App in ASP will be running on these ASP workers.

### Virtual Network

A virtual network allows you to define and control network configuration such as subnet. 
----

The challenge we would like to understand and tackle is the App in the App Service Plan to communicate with the following problem:


- Resources running in the vnet
- Another vnet that is being peered
- On-premises resource that are being connected via site-to-site VPN / Express Route


----

### Solution 1: Service Endpoint


We can turn on service endpoint ([Microsoft.Web](http://microsoft.web/)) from a particular subnet inside the Vnet. The subnet can be classified as subnet 1. This means we specify inbound control into the App Service only allow Subnet 1 access to the firewall.

It is going through the public in address (Microsoft.Web), but it is doing a direct route. Restricting from the rules and its coming from the specific subnet.

**Issue:** Service Endpoint only applies for things inside the subnet. 

### Solution 1.5: App Gateway 


We can deploy app gateway into subnet 1 with the service endpoint from microsoft.web. Other things will go into app gateway and proxy through and go via the path.  
----

### Solution 2: Private Endpoint


Private Endpoint is common and is an ip address that is being consumed that essentially is connecting the traffic to the ASP. You can still lock down the app except the private endpoint.


- Private Endpoint is just an IP in the Vnet for any network that is connected to this Vnet can see the IP and use it. 
- Please ensure that DNS is consistently in place to resolve to the private endpoint.

### Solution 2.5: IP Address Restriction 


IP address can be restricted to a particular address using a load balancer and identifying it within its outbound rules. However, service endpoint is better as its locking it down the subnet.
----

### Other direction? Application talk to things within the Vnet 


### Issue: There are some database in on-premise that application needs access


Hybrid connection manager that we deploy establish an outbound connection over 443 to Azure Relay which then lets the App Service Plan to talk to the Database on-premise . This is **TCP** port and Endpoint. I would need a set of things for things we would want to talk to. 

### Solution 1: Hybrid Connection Manager in Vnet / On-premise via Express Route


There are better solution than this. 

### Solution 2: Gateway Approach

- Vnet gateway support point-to-site VPN to route based dynamic. The App Service Plan will establish a point-to-site connection to the gateway. 
- The Gateway is in its own subnet and it will allow it to live in any region. 

Example: Vnet is in Europe, gateway in other region can talk to the App in US Central. 

### Solution 3: Regional Vnet Integration 


Subnet delegated for the App Service Plan to integrate. The App takes over the subnet and consume the IP Addresses within that subnet. It has to be in the same region as the app. 

**You cannot use this to talk to another vnet in a different region. Only the same region with the application.** 

Each worker will consume a single IP address. If there is 8 workers and we would resize the workers, Azure would spin up 8 new workers with the new size and then delete the other 8 old workers. **We would require a double IP address and this is excluding the 5 IP address mentioned in CIDR Notation.** 

Solution 3 will allow us to talk to things over the Express Route and Private Endpoint to other database but **NOT** peered network.
----

### Summary:


Resources communicating with ⇒ App Service Plan 


- IP restrictions 
- Service Endpoint via Microsoft.Web within the Subnet. 
- Outside of Subnet we can use App Gateway or use Private Endpoint 

App Service Plan communicating with ⇒ Resources


- Hybrid connection manager
- Gateway approach to work across different region
- Vnet integration and has to be in the same region as the app itself.

### Issue: What About Function Apps? 


While there are different apps we can have in the service plan, Functions App can be serverless. 

If I want to use functions and capabiliteis with exception of ip restrions and service endpoints. The Fuctions Apps need to be dedicated or elastic premium close to dediceded but not pure consumption. Functions will not work as its not a dedicated resources.

Azure Functions needs to be either Dedicated / Elastic Premium to use Private Endpoint , Vnet Integration. 
----

### Issue: App Service Environment 


App service environment would be inside the subnet directly that contains front-end, file servers and the workers. Instead in App Service Plan it is dedicated and running in the Subnet inside the Vnet.

Inside the ASE we would have App Service Plan to run applications which have full connectivity. ASE is mroe expensive and is split the cost with other tenant inside the subnet. 
