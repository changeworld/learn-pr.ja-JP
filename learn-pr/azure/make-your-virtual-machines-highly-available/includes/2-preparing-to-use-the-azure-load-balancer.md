Suppose your company wants to see if Azure Load Balancer will support your Enterprise Resource Planning (ERP) application. Your application has a web interface for users and runs on multiple servers. Each server has a local copy of the ERP database, which is synced across all servers.

Here, you will look at how a load balancer can help provide high availability of services. You will identify the difference between the basic and standard load balancer options and see how to create a load balancer for Azure Virtual Machines.

## What is load balancing?

_Load balancing_ describes various techniques for distributing workloads across multiple devices, such as compute, storage, and networking devices. The goal of load balancing is to optimize the use of multiple resources, to make the most efficient use of these resources as an infrastructure is scaled out, and to ensure services are maintained if some components are unavailable.

Here, we'll look at Azure's load balancing support for virtual machines (VMs).

### What is high availability?

High availability (HA) measures the ability of an application or service to remain accessible despite a failure in any system component. Ideally, there will be not be any noticeable loss of service.

Load balancing is fundamental to the delivery of HA because it allows multiple VMs to act as a pool of servers. The pool can continue to service requests even if some VMs crash or are taken offline for maintenance.

## What is Azure Load Balancer?

**Azure Load Balancer** is an Azure service that distributes incoming requests across multiple VMs in a pool. It distributes incoming network traffic across a set of healthy VMs and avoids any VM that is not able to respond.

 Azure Load Balancer operates at Layer-4 (TCP, UDP) of the OSI 7-layer model. It can be configured to support TCP and UDP application scenarios where the traffic is inbound to Azure VMs, as well as outbound scenarios where other Azure services are passing TCP and UDP traffic out through Azure VMs to external endpoints.

## Public vs. internal load balancers

An Azure Load Balancer can be either _public_ or _internal_ depending on the source of incoming requests.

A **public load balancer** handles client requests from outside of your Azure infrastructure. The public IP address of the load balancer is automatically configured as the load balancer's front end when you create the public IP and the load balancer resource. The following illustration shows a public load balancer.

![An illustration showing a public load balancer distributing client requests from the internet to three VMs on a virtual network.](../media-draft/2-public-load-balancer.png)

An **internal load balancer** processes requests from within a virtual network (or through a VPN). It distributes requests to resources within that virtual network. The load balancer, front-end IP addresses, and virtual networks are not directly accessible from the internet. The following illustration shows an architecture containing both a public and internal load balancer. The public load balancer handles external requests while the internal load balancer forwards the requests to the internal VMs and databases for processing.

![An illustration showing a public load balancer forwarding client requests to an internal load balancer. The internal load balancer then distributes requests to a web tier subnet or database tier subnet based on the type of the request. Both the web tier subnet and the database tier subnet have multiple servers to handle requests.](../media-draft/2-internal-load-balancer.png)

## How does Azure Load Balancer work?

Azure Load Balancer uses information configured in **rules** and **health probes** to determine how new inbound traffic that is received on a load balancer's **front end** is distributed to VM instances in a **back-end pool**.

### Front end

The load balancer front end is an IP configuration, containing one or more public IP addresses, that enables access to the load balancer and its applications over the Internet.

### Back end address pool

Virtual machines connect to a load balancer using their virtual network interface card (vNIC). The back-end address pool contains the IP addresses of the vNICs that are connected to the load balancer. If you place all your VMs in an availability set, you can use this to easily add your VMs to a back-end pool when you're configuring the load balancer.

### Health probe

Load balancers use _health probes_ to determine which virtual machines can service requests. The load balancer will only distribute traffic to VMs that are available and operational. 

A health probe monitors specific ports on each VM. You can define what type of response corresponds to "health"; for example, you might require an `HTTP 200 Available` response from a web application. By default, a VM will be marked as "unavailable" after two consecutive failures at 15-second intervals.

### Load balancer rules

Load balancer _rules_ define how traffic is distributed to backend VMs. The goal is to distribute requests fairly across the healthy VMs in the back-end pool.

Azure Load Balancer uses a hash-based algorithm to rewrite the headers of inbound packets. By default, Load Balancer creates a hash from:

- Source IP addresses
- Source ports
- Destination IP addresses
- Destination ports
- IP protocol numbers

This mechanism ensures that all packets within a packet client flow are sent to the same backend VM instance. A new flow from a client will use a different randomly allocated source port. This mean that the hash will change, and the load balancer may send this flow to a different back-end endpoint.

## Basic vs. Standard Load Balancer SKUs

There are two versions of Azure Load Balancer: **Basic** and **Standard**. They differ in scale, features, and pricing. For example:

- Standard supports HTTPS while Basic does not
- Pool size can be much larger in Standard
- Basic is no-cost while Standard is charged based on rules and throughput.

Standard is a superset of Basic, so any scenario suitable for Basic should also work on Standard. The Basic SKU is generally intended for prototyping and testing while Standard is recommended for production.

## Start the deployment of a basic public load balancer

To create a load-balanced VM system, you need to create the load balancer itself, create a virtual network to contain your virtual machines, and then add VMs to the virtual network.

To create the load balancer using the Azure portal, you define the following:

- Load balancer name
- Type: public or internal
- SKU: Basic or Standard
- Public IP address: dynamic or static
- Resource group and location

Your back-end VMs will all be connected to the same virtual network, so you need to configure this resource next:

- Virtual network name
- Address space to use, such as 172.20.0.0/16
- Resource group
- Name for the subnet to use
- Address space for the subnet (must be within the main space), such as 172.20.0.0/24

You then need to create and deploy your backend VMs and configure them to use your virtual network. You should also place your VMs into the same availability set. Availability sets define the level of fault tolerance across a group of VMs, but for load balancing, they also help you assign your VMs to back-end pools.

You have now seen how to use Azure Load Balancer as part of a high-availability solution. Next, you will use these steps to deploy your own load balancer.