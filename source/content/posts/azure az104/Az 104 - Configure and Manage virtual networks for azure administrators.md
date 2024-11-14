---
draft: true
date: 2024-11-14
slug: Az 104 - Configure and Manage virtual networks for azure administrators
tags:
---

# Configure virtual networks

# Introduction

Azure virtual networks are an essential component for creating private networks in Azure. They allow different Azure resources to securely communicate with each other, the internet, and on-premises networks.

Suppose you work for a company in the healthcare industry. Your company is looking to migrate their on-premises infrastructure to Azure. They want to ensure secure communication between their resources in Azure and their on-premises network. The company is also concerned about scalability and availability. By using Azure virtual networks, they can create a private network in Azure and securely connect their resources.

The topics covered in this module include subnetting, creating virtual networks, and using private and public IP addresses. You also learn about the different scenarios where virtual networks can be used, such as creating a dedicated private cloud-only network or extending a data center securely. The module provides a detailed explanation of subnets and their benefits, and how to plan IP addressing for Azure resources.

By the end of this module, you have a clear understanding of how to create and configure virtual networks in Azure. You're able to effectively use subnets, assign IP addresses, and ensure secure communication between your Azure resources and on-premises network.

## Learning objectives

In this module, you learn how to:

- Describe Azure virtual network features and components.
- Identify features and usage cases for subnets and subnetting.
- Identify usage cases for private and public IP addresses.
- Create a virtual network and assign an IP address.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104).

## Prerequisites

- Basic knowledge of virtual networking in cloud environments.
- Familiarity with IP address formats and subnetting.

# Plan virtual networks

A major incentive for adopting cloud solutions like Azure is to enable information technology departments to transition server resources to the cloud. Moving resources to the cloud can save money and simplify administrative operations. Relocating resources removes the need to maintain expensive datacenters with uninterruptible power supplies, generators, multiple fail-safes, or clustered database servers. For small and medium-sized companies, which might not have the expertise to maintain their own robust infrastructure, moving to the cloud is particularly appealing.

After resources are moved to Azure, they require the same networking functionality as an on-premises deployment. In specific scenarios, the resources require some level of network isolation. Azure network services offer a range of components with functionalities and capabilities, as shown in the following image:

![Screenshot that shows the main components of Azure network services.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-networks/media/network-components-66dff480.png)

### Things to know about Azure virtual networks

You can implement Azure Virtual Network to create a virtual representation of your network in the cloud. Let's examine some characteristics of virtual networks in Azure.

- An Azure virtual network is a logical isolation of the Azure cloud resources.
    
- You can use virtual networks to provision and manage virtual private networks (VPNs) in Azure.
    
- Each virtual network has its own Classless Inter-Domain Routing (CIDR) block and can be linked to other virtual networks and on-premises networks.
    
- You can link virtual networks with an on-premises IT infrastructure to create hybrid or cross-premises solutions, when the CIDR blocks of the connecting networks don't overlap.
    
- You control the DNS server settings for virtual networks, and segmentation of the virtual network into subnets.
    

The following illustration depicts a virtual network that has a subnet containing two virtual machines. The virtual network has connections to an on-premises infrastructure and a separate virtual network.

![Diagram of a virtual network with a subnet of two virtual machines. The network connects to an on-premises infrastructure and separate virtual network.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-networks/media/virtual-networks-c016972b.png)

### Things to consider when using virtual networks

Virtual networks can be used in many ways. As you think about the configuration plan for your virtual networks and subnets, consider the following scenarios.

Expand table

|Scenario|Description|
|---|---|
|_Create a dedicated private cloud-only virtual network_|Sometimes you don't require a cross-premises configuration for your solution. When you create a virtual network, your services and virtual machines within your virtual network can communicate directly and securely with each other in the cloud. You can still configure endpoint connections for the virtual machines and services that require internet communication, as part of your solution.|
|_Securely extend your data center with virtual networks_|You can build traditional site-to-site VPNs to securely scale your datacenter capacity. Site-to-site VPNs use IPSEC to provide a secure connection between your corporate VPN gateway and Azure.|
|_Enable hybrid cloud scenarios_|Virtual networks give you the flexibility to support a range of hybrid cloud scenarios. You can securely connect cloud-based applications to any type of on-premises system, such as mainframes and Unix systems.|

# Create subnets


Subnets provide a way for you to implement logical divisions within your virtual network. Your network can be segmented into subnets to help improve security, increase performance, and make it easier to manage.

### Things to know about subnets

There are certain conditions for the IP addresses in a virtual network when you apply segmentation with subnets.

- Each subnet contains a range of IP addresses that fall within the virtual network address space.
    
- The address range for a subnet must be unique within the address space for the virtual network.
    
- The range for one subnet can't overlap with other subnet IP address ranges in the same virtual network.
    
- The IP address space for a subnet must be specified by using CIDR notation.
    
- You can segment a virtual network into one or more subnets in the Azure portal. Characteristics about the IP addresses for the subnets are listed.
    
    ![Screenshot that shows multiple subnets for a virtual network in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-networks/media/azure-subnets-a5c893d5.png)
    

#### Reserved addresses

For each subnet, Azure reserves five IP addresses. The first four addresses and the last address are reserved.

Let's examine the reserved addresses in an IP address range of `192.168.1.0/24`.

Expand table

|Reserved address|Reason|
|---|---|
|`192.168.1.0`|This value identifies the virtual network address.|
|`192.168.1.1`|Azure configures this address as the default gateway.|
|`192.168.1.2` _and_ `192.168.1.3`|Azure maps these Azure DNS IP addresses to the virtual network space.|
|`192.168.1.255`|This value supplies the virtual network broadcast address.|

### Things to consider when using subnets

When you plan for adding subnet segments within your virtual network, there are several factors to consider. Review the following scenarios.

- **Consider service requirements**. Each service directly deployed into a virtual network has specific requirements for routing and the types of traffic that must be allowed into and out of associated subnets. A service might require or create their own subnet. There must be enough unallocated space to meet the service requirements. Suppose you connect a virtual network to an on-premises network by using Azure VPN Gateway. The virtual network must have a dedicated subnet for the gateway.
    
- **Consider network virtual appliances**. Azure routes network traffic between all subnets in a virtual network, by default. You can override Azure's default routing to prevent Azure routing between subnets. You can also override the default to route traffic between subnets through a network virtual appliance. If you require traffic between resources in the same virtual network to flow through a network virtual appliance, deploy the resources to different subnets.
    
- **Consider service endpoints**. You can limit access to Azure resources like an Azure storage account or Azure SQL database to specific subnets with a virtual network service endpoint. You can also deny access to the resources from the internet. You might create multiple subnets, and then enable a service endpoint for some subnets, but not others.
    
- **Consider network security groups**. You can associate zero or one network security group to each subnet in a virtual network. You can associate the same or a different network security group to each subnet. Each network security group contains rules that allow or deny traffic to and from sources and destinations.
    
- **Consider private links**. Azure Private Link provides private connectivity from a virtual network to Azure platform as a service (PaaS), customer-owned, or Microsoft partner services. Private Link simplifies the network architecture and secures the connection between endpoints in Azure. The service eliminates data exposure to the public internet.

# Create virtual networks

You can create new virtual networks at any time. You can also add virtual networks when you create a virtual machine.

### Things to know about creating virtual networks

Review the following requirements for creating a virtual network.

- When you create a virtual network, you need to define the IP address space for the network.
    
- Plan to use an IP address space that's not already in use in your organization.
    
    - The address space for the network can be either on-premises or in the cloud, but not both.
        
    - Once you create the IP address space, it can't be changed. If you plan your address space for cloud-only virtual networks, you might later decide to connect an on-premises site.
        
- To create a virtual network, you need to define at least one subnet.
    
    - Each subnet contains a range of IP addresses that fall within the virtual network address space.
        
    - The address range for each subnet must be unique within the address space for the virtual network.
        
    - The range for one subnet can't overlap with other subnet IP address ranges in the same virtual network.
        
- You can create a virtual network in the Azure portal. Provide the Azure subscription, resource group, virtual network name, and service region for the network.
    
    ![Screenshot that shows how to create a virtual network in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-networks/media/create-virtual-networks-b4f1fd40.png)
    

 Note

Default limits on Azure networking resources can change periodically. Be sure to consult the [Azure networking documentation](https://learn.microsoft.com/en-us/azure/networking/) for the latest information.

# Plan IP addressing

You can assign IP addresses to Azure resources to communicate with other Azure resources, your on-premises network, and the internet. There are two types of Azure IP addresses: _private_ and _public_.

**Private IP addresses** enable communication within an Azure virtual network and your on-premises network. You create a private IP address for your resource when you use a VPN gateway or Azure ExpressRoute circuit to extend your network to Azure.

**Public IP addresses** allow your resource to communicate with the internet. You can create a public IP address to connect with Azure public-facing services.

The following illustration shows a virtual machine resource that has a private IP address and a public IP address.

![Illustration of a resource with a private IP address and a public IP address.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-networks/media/ip-addressing-54476e47.png)

### Things to know about IP addresses

Let's take a closer look at the characteristics of IP addresses.

- IP addresses can be statically assigned or dynamically assigned.
    
- You can separate dynamically and statically assigned IP resources into different subnets.
    
- Static IP addresses don't change and are best for certain situations, such as:
    
    - DNS name resolution, where a change in the IP address requires updating host records.
    - IP address-based security models that require apps or services to have a static IP address.
    - TLS/SSL certificates linked to an IP address.
    - Firewall rules that allow or deny traffic by using IP address ranges.
    - Role-based virtual machines such as Domain Controllers and DNS servers.


# Create public IP addressing

You can create a public IP address for your resource in the Azure portal.

![Screenshot that shows how to create a public IP address in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-networks/media/create-public-ip-address-f07bd67d.png)

### Things to consider when creating a public IP address

To create a public IP address, configure the following settings:

- **IP Version**: Select to create an **IPv4** or **IPv6** address, or **Both** addresses.
    
- **SKU**: Select the SKU for the public IP address, including **Basic** or **Standard**. The value must match the SKU of the Azure load balancer with which the address is used.
    
- **Name**: Enter a name to identify the IP address. The name must be unique within the resource group you select.
    
- **IP address assignment**: Identify the type of IP address assignment to use.
    
    - **Dynamic** addresses are assigned after a public IP address is associated to an Azure resource and is started for the first time. Dynamic addresses can change if a resource such as a virtual machine is stopped (deallocated) and then restarted through Azure. The address remains the same if a virtual machine is rebooted or stopped from within the guest OS. When a public IP address resource is removed from a resource, the dynamic address is released.
        
    - **Static** addresses are assigned when a public IP address is created. Static addresses aren't released until a public IP address resource is deleted. If the address isn't associated to a resource, you can change the assignment method after the address is created. If the address is associated to a resource, you might not be able to change the assignment method.
        

 Note

If you select **IPv6** for the IP version, the assignment method must be **Dynamic** for the Basic SKU. Standard SKU addresses are **Static** for both IPv4 and IPv6 addresses.

# Associate public IP addresses

A public IP address resource can be associated with virtual machine network interfaces, internet-facing load balancers, VPN gateways, and application gateways. You can associate your resource with both dynamic and static public IP addresses.

### Things to consider when associating public IP addresses

The following table summarizes how you can associate public IP addresses for different types of resources.

Expand table

|Resource|Public IP address association|Dynamic IP address|Static IP address|
|---|---|---|---|
|Virtual machine|NIC|Yes|Yes|
|Load balancer|Front-end configuration|Yes|Yes|
|VPN gateway|VPN gateway IP configuration|Yes|Yes *****|
|Application gateway|Front-end configuration|Yes|Yes *****|

***** Static IP addresses are available on certain SKUs only.

#### Public IP address SKUs

When you create a public IP address, you select the Basic or Standard SKU. Your SKU choice affects the IP assignment method, security, available resources, and redundancy options.

The following table summarizes the differences between the SKU types for public IP addresses.

Expand table

|Feature|Basic SKU|Standard SKU|
|---|---|---|
|IP assignment|Static or Dynamic|Static|
|Security|Open by default|Secure by default, closed to inbound traffic|
|Resources|Network interfaces, VPN gateways, Application gateways, and internet-facing load balancers|Network interfaces or public standard load balancers|
|Redundancy|Not zone redundant|Zone redundant by default|

# Allocate or assign private IP addresses


A private IP address resource can be associated with virtual machine network interfaces, internal load balancers, and application gateways. Azure can provide an IP address (dynamic assignment) or you can assign the IP address (static assignment).

### Things to consider when associating private IP addresses

The following table summarizes how you can associate private IP addresses for different types of resources.

Expand table

|Resource|Private IP address association|Dynamic IP address|Static IP address|
|---|---|---|---|
|Virtual machine|NIC|Yes|Yes|
|Internal load balancer|Front-end configuration|Yes|Yes|
|Application gateway|Front-end configuration|Yes|Yes|

#### Private IP address assignment

A private IP address is allocated from the address range of the virtual network subnet that a resource is deployed in. There are two options: dynamic and static.

- **Dynamic**: Azure assigns the next available unassigned or unreserved IP address in the subnet's address range. Dynamic assignment is the default allocation method.
    
    Suppose addresses 10.0.0.4 through 10.0.0.9 are already assigned to other resources. In this case, Azure assigns the address 10.0.0.10 to a new resource.
    
- **Static**: You select and assign any unassigned or unreserved IP address in the subnet's address range.
    
    Suppose a subnet's address range is 10.0.0.0/16, and addresses 10.0.0.4 through 10.0.0.9 are already assigned to other resources. In this scenario, you can assign any address between 10.0.0.10 and 10.0.255.254.

# Interactive lab simulation

## Lab scenario

Your organization is migrating network infrastructure and virtual machines to Azure. As the Azure Administrator you need to:

- Configure Azure virtual networks and subnets.
- Connect remotely to Azure virtual machines by using RDP.
- Verify virtual machines in the same virtual network can communicate.

## Architecture diagram

![Diagram of the architecture as explained in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-networks/media/create-network-architecture.png)

## Objectives

- **Task 1**: Create a virtual network.
    - Create a virtual network, **vnet1**, with an IP address space of 10.1.0.0/16.
    - Create a subnet, **default**, with an IP address space of 10.1.0.0/24.
- **Task 2**: Create two virtual machines.
    - Create a virtual machine, **vm1**, in **vnet1** and allow inbound RDP.
    - Create a second virtual machine, **vm2**, in **vnet1** and allow inbound RDP.
    - Ensure both virtual machines are deployed and running before continuing.
- **Task 3**: Test the virtual machine connections.
    - Connect to **vm1** with RDP.
    - Connect to **vm2** with RDP.
    - Disable the public and private Windows Firewall on both virtual machines.
    - Use Azure PowerShell to confirm **vm1** can ping **vm2**.

 Note

Select the thumbnail image to start the lab simulation. When you're done, be sure to return to this page so you can continue learning.

[![Screenshot of the simulation page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-networks/media/simulation-create-networks.png)](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%204)

# Summary and resources

In this module, you learned about Azure virtual networks and their importance in creating private networks in Azure. You explored the benefits of using virtual networks, such as scalability, availability, and isolation. You learned how to create virtual networks with subnetting and how to determine which resources require public or private IP addresses.

The main takeaways from this module are:

- Azure virtual networks allow different Azure resources to securely communicate with each other, the internet, and on-premises networks.
    
- Subnets within virtual networks provide logical divisions, improving security, performance, and management.
    
- When creating virtual networks, ensure that the IP address space is unique and doesn't overlap with other subnets.
    
- IP addresses can provide public or private access to resources.
    

## Learn more with documentation

- [What is Azure Virtual Network?](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview). This article is your starting point to learn about virtual networks.
    
- [Public IP addresses](https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-addresses). This article reviews the basics of when to use public IP addresses.
    
- [Private IP addresses](https://learn.microsoft.com/en-us/azure/virtual-network/private-ip-addresses). This article reviews the basics of when to use private IP addresses.
    

## Learn more with self-paced training

- [Introduction to Azure Virtual Networks](https://learn.microsoft.com/en-us/training/modules/introduction-to-azure-virtual-networks/). Learn how to design and implement core Azure Networking infrastructure.
    
- [Design an IP addressing schema for your Azure deployment (sandbox)](https://learn.microsoft.com/en-us/training/modules/design-ip-addressing-for-azure/). Learn about network IP addressing and integration.
    
- [Implement Windows Server IaaS virtual machine IP addressing and routing](https://learn.microsoft.com/en-us/training/modules/implement-windows-server-iaas-virtual-machine-ip-addressing-routing/). Learn about IP addressing and virtual networks for virtual machines.

---

# Configure network security groups

# Introduction

Network security groups are a way to limit network traffic to resources in your virtual network. Network security groups contain a list of security rules that allow or deny inbound or outbound network traffic.

Suppose your company has several locations and wants to migrate to a cloud based solution. The company only considers moving key systems onto the cloud platform if stringent security requirements can be met. These requirements include tight control over which computers have network access to the app servers. You need to secure both virtual machine networking and Azure services networking. Your goal is to prevent unwanted or unsecured network traffic from being able to reach key systems.

In this module, you learn how to create a network security group, configure inbound and outbound port rules, and verify secure connectivity.

The goal of this module is to teach you how to control network traffic with network security groups.

# Implement network security groups


You can limit network traffic to resources in your virtual network by using a network security group. You can assign a network security group to a subnet or a network interface, and define security rules in the group to control network traffic.

### Things to know about network security groups

Let's look at the characteristics of network security groups.

- A network security group contains a list of security rules that allow or deny inbound or outbound network traffic.
    
- A network security group can be associated to a subnet or a network interface.
    
- A network security group can be associated multiple times.
    
- You create a network security group and define security rules in the Azure portal.
    

Network security groups are defined for your virtual machines in the Azure portal. The **Overview** page for a virtual machine provides information about the associated network security groups. You can see details such as the assigned subnets, assigned network interfaces, and the defined security rules.

![Screenshot that shows details for a network security group for a virtual machine in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/network-security-groups-1ebf7bed.png)

#### Network security groups and subnets

You can assign network security groups to a subnet and create a protected screened subnet (also referred to as a demilitarized zone or _DMZ_). A DMZ acts as a buffer between resources within your virtual network and the internet.

- Use the network security group to restrict traffic flow to all machines that reside within the subnet.
    
- Each subnet can have a maximum of one associated network security group.
    

#### Network security groups and network interfaces

You can assign network security groups to a network interface card (NIC).

- Define network security group rules to control all traffic that flows through a NIC.
    
- Each network interface that exists in a subnet can have zero, or one, associated network security groups.

# Determine network security group rules

Security rules in network security groups enable you to filter network traffic. You can define rules to control the traffic flow in and out of virtual network subnets and network interfaces.

### Things to know about security rules

Let's review the characteristics of security rules in network security groups.

- Azure creates several default security rules within each network security group, including inbound traffic and outbound traffic. Examples of default rules include `DenyAllInbound` traffic and `AllowInternetOutbound` traffic.
    
- Azure creates the default security rules in each network security group that you create.
    
- You can add more security rules to a network security group by specifying conditions for any of the following settings:
    
    - **Name**
    - **Priority**
    - **Port**
    - **Protocol** (Any, TCP, UDP)
    - **Source** (Any, IP addresses, Service tag)
    - **Destination** (Any, IP addresses, Virtual network)
    - **Action** (Allow or Deny)
- Each security rule is assigned a Priority value. All security rules for a network security group are processed in priority order. When a rule has a low Priority value, the rule has a higher priority or precedence in terms of order processing.
    
- You can't remove the default security rules.
    
- You can override a default security rule by creating another security rule that has a higher Priority setting for your network security group.
    

#### Inbound traffic rules

Azure defines three default inbound security rules for your network security group. These rules **deny all inbound traffic** except traffic from your virtual network and Azure load balancers. The following image shows the default inbound security rules for a network security group in the Azure portal.

![Screenshot that shows default inbound security rules for a network security group in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/inbound-rules-a554314b.png)

#### Outbound traffic rules

Azure defines three default outbound security rules for your network security group. These rules **only allow outbound traffic** to the internet and your virtual network. The following image shows the default outbound security rules for a network security group in the Azure portal.

![Screenshot that shows default outbound security rules for a network security group in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/outbound-rules-ff90d802.png)

# Determine network security group effective rules

Each network security group and its defined security rules are evaluated independently. Azure processes the conditions in each rule defined for each virtual machine in your configuration.

- For inbound traffic, Azure first processes network security group security rules for any associated subnets and then any associated network interfaces.
- For outbound traffic, the process is reversed. Azure first evaluates network security group security rules for any associated network interfaces followed by any associated subnets.
- For both the inbound and outbound evaluation process, Azure also checks how to apply the rules for intra-subnet traffic.

How Azure ends up applying your defined security rules for a virtual machine determines the overall _effectiveness_ of your rules.

### Things to know about effective security rules

Let's explore how network security group rules are defined and processed within a virtual network to yield the effective rules.

Consider the following virtual network configuration that shows network security groups (NSGs) controlling traffic to virtual machines (VMs). The configuration requires security rules to manage network traffic to and from the internet over TCP port 80 via the network interface.

![Diagram that shows how network security group security rules control traffic to virtual machines.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/security-groups-7a9d5c84.png)

In this virtual network configuration, there are three subnets. Subnet 1 contains two virtual machines: VM 1 and VM 2. Subnet 2 and Subnet 3 each contain one virtual machine: VM 3 and VM 4, respectively. Each VM has a network interface card (NIC).

Azure evaluates each NSG configuration to determine the effective security rules:

Expand table

|Evaluation|Subnet _NSG_|NIC _NSG_|Inbound rules|Outbound rules|
|---|---|---|---|---|
|**VM 1**|Subnet 1  <br>_NSG 1_|NIC  <br>_NSG 2_|_NSG 1_ subnet rules have precedence over _NSG 2_ NIC rules|_NSG 2_ NIC rules have precedence over _NSG 1_ subnet rules|
|**VM 2**|Subnet 1  <br>_NSG 1_|NIC  <br>_none_|_NSG 1_ subnet rules apply to both subnet and NIC|Azure default rules apply to NIC  <br>and _NSG 1_ subnet rules apply to subnet only|
|**VM 3**|Subnet 2  <br>_none_|NIC  <br>_NSG 2_|Azure default rules apply to subnet  <br>and _NSG 2_ rules apply to NIC|_NSG 2_ NIC rules apply to NIC and subnet|
|**VM 4**|Subnet 3  <br>_none_|NIC  <br>_none_|Azure default rules apply to both subnet and NIC  <br>and all inbound traffic is allowed|Azure default rules apply to both subnet and NIC  <br>and all outbound traffic is allowed|

#### Inbound traffic effective rules

Azure processes rules for inbound traffic for all VMs in the configuration. Azure identifies if the VMs are members of an NSG, and if they have an associated subnet or NIC.

- When an NSG is created, Azure creates the default security rule `DenyAllInbound` for the group. The default behavior is to deny all inbound traffic from the internet. If an NSG has a subnet or NIC, the rules for the subnet or NIC can override the default Azure security rules.
    
- NSG inbound rules for a subnet in a VM take precedence over NSG inbound rules for a NIC in the same VM.
    

#### Outbound traffic effective rules

Azure processes rules for outbound traffic by first examining NSG associations for NICs in all VMs.

- When an NSG is created, Azure creates the default security rule `AllowInternetOutbound` for the group. The default behavior is to allow all outbound traffic to the internet. If an NSG has a subnet or NIC, the rules for the subnet or NIC can override the default Azure security rules.
    
- NSG outbound rules for a NIC in a VM take precedence over NSG outbound rules for a subnet in the same VM.
    

### Things to consider when creating effective rules

Review the following considerations regarding creating effective security rules for machines in your virtual network.

- **Consider allowing all traffic**. If you place your virtual machine within a subnet or utilize a network interface, you don't have to associate the subnet or NIC with a network security group. This approach allows all network traffic through the subnet or NIC according to the default Azure security rules. If you're not concerned about controlling traffic to your resource at a specific level, then don't associate your resource at that level to a network security group.
    
- **Consider importance of allow rules**. When you create a network security group, you must define an **allow** rule for both the subnet and network interface in the group to ensure traffic can get through. If you have a subnet or NIC in your network security group, you must define an allow rule at each level. Otherwise, the traffic is denied for any level that doesn't provide an allow rule definition.
    
- **Consider intra-subnet traffic**. The security rules for a network security group that's associated to a subnet can affect traffic between all virtual machines in the subnet. By default, Azure allows virtual machines in the same subnet to send traffic to each other (referred to as _intra-subnet traffic_). You can prohibit intra-subnet traffic by defining a rule in the network security group to deny all inbound and outbound traffic. This rule prevents all virtual machines in your subnet from communicating with each other.
    
- **Consider rule priority**. The security rules for a network security group are processed in priority order. To ensure a particular security rule is always processed, assign the lowest possible priority value to the rule. It's a good practice to leave gaps in your priority numbering, such as 100, 200, 300, and so. The gaps in the numbering allow you to add new rules without having to edit existing rules.
    

### View effective security rules

If you have several network security groups and aren't sure which security rules are being applied, you can use the **Effective security rules** link in the Azure portal. You can use the link to verify which security rules are applied to your machines, subnets, and network interfaces.

![Screenshot of the Networking page in the Azure portal showing the Effective security rules link highlighted.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/effective-security-rules-d93ab464.png)

# Create network security group rules

It's easy to add security rules to control inbound and outbound traffic in the Azure portal. You can configure your virtual network security group rule settings, and select from a large variety of communication services, including HTTPS, RDP, FTP, and DNS.

### Things to know about configuring security rules

Let's look at some of the properties you need to specify to create your security rules. As you review these settings, think about the traffic rules you need to create and what services can fulfill your network requirements.

![Screenshot that shows how to configure source and destination settings to create a security rule in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/add-network-security-rule-2f306d23.png)

- **Source**: Identifies how the security rule controls **inbound** traffic. The value specifies a specific source IP address range that's allowed or denied. The source filter can be any resource, an IP address range, an application security group, or a default tag.
    
- **Destination**: Identifies how the security rule controls **outbound** traffic. The value specifies a specific destination IP address range that's allowed or denied. The destination filter value is similar to the source filter. The value can be any resource, an IP address range, an application security group, or a default tag.
    
- **Service**: Specifies the destination protocol and port range for the security rule. You can choose a predefined service like RDP or SSH or provide a custom port range. There are a large number of services to select from.
    
    ![Screenshot that shows service rule options for a security rule in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/security-services.png)
    
- **Priority**: Assigns the priority order value for the security rule. Rules are processed according to the priority order of all rules for a network security group, including a subnet and network interface. The lower the priority value, the higher priority for the rule.
    
    ![Screenshot that shows how to set the priority value for a security rule in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/security-priority.png)

# Implement application security groups

You can implement [application security groups](https://learn.microsoft.com/en-us/azure/virtual-network/application-security-groups) in your Azure virtual network to logically group your virtual machines by workload. You can then define your network security group rules based on your application security groups.

### Things to know about using application security groups

Application security groups work in the same way as network security groups, but they provide an application-centric way of looking at your infrastructure. You join your virtual machines to an application security group. Then you use the application security group as a source or destination in the network security group rules.

Let's examine how to implement application security groups by creating a configuration for an online retailer. In our example scenario, we need to control network traffic to virtual machines in application security groups.

![Diagram that shows how application security groups combine with network security groups to protect applications.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/application-security-groups.png)

#### Scenario requirements

Here are the scenario requirements for our example configuration:

- We have six virtual machines in our configuration with two web servers and two database servers.
- Customers access the online catalog hosted on our web servers.
- The web servers must be accessible from the internet over HTTP port 80 and HTTPS port 443.
- Inventory information is stored on our database servers.
- The database servers must be accessible over HTTPS port 1433.
- Only our web servers should have access to our database servers.

#### Solution

For our scenario, we need to build the following configuration:

1. Create application security groups for the virtual machines.
    
    1. Create an application security group named `WebASG` to group our web server machines.
        
    2. Create an application security group named `DBASG` to group our database server machines.
        
2. Assign the network interfaces for the virtual machines.
    
    - For each virtual machine server, assign its NIC to the appropriate application security group.
3. Create the network security group and security rules.
    
    - **Rule 1**: Set **Priority** to 100. Allow access from the internet to machines in the `WebASG` group from HTTP port 80 and HTTPS port 443.
        
        Rule 1 has the lowest priority value, so it has precedence over the other rules in the group. Customer access to our online catalog is paramount in our design.
        
    - **Rule 2**: Set **Priority** to 110. Allow access from machines in the `WebASG` group to machines in the `DBASG` group over HTTPS port 1433.
        
    - **Rule 3**: Set **Priority** to 120. **Deny** (X) access from anywhere to machines in the `DBASG` group over HTTPS port 1433.
        
        The combination of Rule 2 and Rule 3 ensures that only our web servers can access our database servers. This security configuration protects our inventory databases from outside attack.
        

### Things to consider when using application security groups

There are several advantages to implementing application security groups in your virtual networks.

- **Consider IP address maintenance**. When you control network traffic by using application security groups, you don't need to configure inbound and outbound traffic for specific IP addresses. If you have many virtual machines in your configuration, it can be difficult to specify all of the affected IP addresses. As you maintain your configuration, the number of your servers can change. These changes can require you to modify how you support different IP addresses in your security rules.
    
- **Consider no subnets**. By organizing your virtual machines into application security groups, you don't need to also distribute your servers across specific subnets. You can arrange your servers by application and purpose to achieve logical groupings.
    
- **Consider simplified rules**. Application security groups help to eliminate the need for multiple rule sets. You don't need to create a separate rule for each virtual machine. You can dynamically apply new rules to designated application security groups. New security rules are automatically applied to all the virtual machines in the specified application security group.
    
- **Consider workload support**. A configuration that implements application security groups is easy to maintain and understand because the organization is based on workload usage. Application security groups provide logical arrangements for your applications, services, data storage, and workloads.

# Interactive lab simulation

## Lab scenario

Your organization wants to ensure that access to virtual machines is restricted. As the Azure Administrator, you need to:

- Create and configure network security groups.
- Associate network security groups to virtual machines.
- Deny and allow access to the virtual machines by using network security groups.

## Architecture diagram

![Diagram showing the architecture as explained in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/architecture-create-networks.png)

## Objectives

- **Task 1**: Create a virtual machine to test network security.
    - Create a Windows Server virtual machine.
    - Don't configure any inbound port rules or NIC network security groups.
    - Verify the virtual machine is created.
    - Review the **Inbound port rules** tab, and note there are no network security groups associated with the virtual machine.
- **Task 2**: Create a network security group, and associate the group with the virtual machine.
    - Create a network security group.
    - Associate the network security group with the virtual machine network interface (NIC).
- **Task 3**: Configure an inbound security port rule to allow RDP.
    - Verify you can't connect to the virtual machine by using RDP.
    - Add an **inbound port rule** to allow RDP to the virtual machine on port 3389.
    - Verify you can now connect to the virtual machine with RDP.
- **Task 4**: Configure an outbound security port rule to deny internet access
    - Verify you can access the internet from the virtual machine.
    - Add an **outbound port rule** to deny internet access from the virtual machine.
    - Verify you can no longer access the internet from the virtual machine.

 Note

Select the thumbnail image to start the lab simulation. When you're done, be sure to return to this page so you can continue learning.

[![Screenshot of the simulation page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/simulation-create-networks.png)](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2013)

---

# Configure Azure Virtual Network peering

# Introduction

Azure Virtual Network peering lets you connect virtual networks in the same or different regions. Azure Virtual Network peering provides secure communication between resources in the peered networks.

Suppose your engineering company is migrating services to Azure. The company is deploying services into separate Azure virtual networks. Private connectivity between the virtual networks isn't yet configured. Several business units identified services in the virtual networks that need to communicate with each other.

You're responsible for implementing an Azure Virtual Network peering solution and enabling connectivity between the virtual networks. Two of your strategy goals include preventing exposure of the services to the internet, and keeping the integration as simple as possible. Your solution should address transit and connectivity concerns.

The goal of this module is to successfully implement Azure Virtual Network peering.

## Learning objectives

In this module, you learn how to:

- Identify usage cases and product features of Azure Virtual Network peering.
- Configure your network to implement Azure VPN Gateway for transit connectivity.
- Extend peering by using a hub and spoke network with user-defined routes and service chaining.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104).

## Prerequisites

- Basic understanding of cloud networking including virtual networks and virtual machines.
    
- Familiarity with the command line connectivity testing tools.

# Determine Azure Virtual Network peering uses

Perhaps the simplest and quickest way to connect your virtual networks is to use Azure Virtual Network peering. Virtual Network peering enables you to seamlessly connect two Azure virtual networks. After the networks are peered, the two virtual networks operate as a single network, for connectivity purposes.

### Things to know about Azure Virtual Network peering

Let's examine some prominent characteristics of Azure Virtual Network peering.

- There are two types of Azure Virtual Network peering: _regional_ and _global_.
    
    ![Diagram that demonstrates the two types of Azure Virtual Network peering: global and regional.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-vnet-peering/media/network-peering-5beae28a.png)
    
- **Regional virtual network peering** connects Azure virtual networks that exist in the same region.
    
- **Global virtual network peering** connects Azure virtual networks that exist in different regions.
    
- You can create a regional peering of virtual networks in the same Azure public cloud region, or in the same China cloud region, or in the same Microsoft Azure Government cloud region.
    
- You can create a global peering of virtual networks in any Azure public cloud region, or in any China cloud region.
    
- Global peering of virtual networks in different Azure Government cloud regions isn't permitted.
    
- After you create a peering between virtual networks, the individual virtual networks are still managed as separate resources.
    

### Things to consider when using Azure Virtual Network peering

Consider the following benefits of using Azure Virtual Network peering.

Expand table

|Benefit|Description|
|---|---|
|**Private network connections**|When you implement Azure Virtual Network peering, network traffic between peered virtual networks is private. Traffic between the virtual networks is kept on the Microsoft Azure backbone network. No public internet, gateways, or encryption is required in the communication between the virtual networks.|
|**Strong performance**|Because Azure Virtual Network peering utilizes the Azure infrastructure, you gain a low-latency, high-bandwidth connection between resources in different virtual networks.|
|**Simplified communication**|Azure Virtual Network peering lets resources in one virtual network communicate with resources in a different virtual network, after the virtual networks are peered.|
|**Seamless data transfer**|You can create an Azure Virtual Network peering configuration to transfer data across Azure subscriptions, deployment models, and across Azure regions.|
|**No resource disruptions**|Azure Virtual Network peering doesn't require downtime for resources in either virtual network when creating the peering, or after the peering is created.|

# Determine gateway transit and connectivity

When virtual networks are peered, you can configure Azure VPN Gateway in the peered virtual network as a _transit point_. In this scenario, a peered virtual network uses the remote VPN gateway to gain access to other resources.

Consider a scenario where three virtual networks in the same region are connected by virtual network peering. Virtual network A and virtual network B are each peered with a hub virtual network. The hub virtual network contains several resources, including a gateway subnet and an Azure VPN gateway. The VPN gateway is configured to allow VPN gateway transit. Virtual network B accesses resources in the hub, including the gateway subnet, by using a remote VPN gateway.

![Diagram of a regional virtual network peering. One network allows VPN gateway transit and uses a remote VPN gateway to access resources in a hub virtual network.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-vnet-peering/media/gateway-transit-173a51a0.png)

### Things to know about Azure VPN Gateway

Let's take a closer look at how Azure VPN Gateway is implemented with Azure Virtual Network peering.

- A virtual network can have only one VPN gateway.
    
- Gateway transit is supported for both regional and global virtual network peering.
    
- When you allow VPN gateway transit, the virtual network can communicate to resources outside the peering. In our sample illustration, the gateway subnet gateway within the hub virtual network can complete tasks such as:
    
    - Use a site-to-site VPN to connect to an on-premises network.
    - Use a vnet-to-vnet connection to another virtual network.
    - Use a point-to-site VPN to connect to a client.
- Gateway transit allows peered virtual networks to share the gateway and get access to resources. With this implementation, you don't need to deploy a VPN gateway in the peer virtual network.
    
- You can apply network security groups in a virtual network to block or allow access to other virtual networks or subnets. When you configure virtual network peering, you can choose to open or close the network security group rules between the virtual networks.

# Create virtual network peering

Azure Virtual Network peering can be configured for virtual networks by using PowerShell, the Azure CLI, and in the Azure portal. In this module, we review the steps to create the peering in the Azure portal for virtual networks deployed through Azure Resource Manager.

### Things to know about creating virtual network peering

There are a few points to review before we look at how to create the peering in the Azure portal.

- To implement virtual network peering, your Azure account must be assigned to the `Network Contributor` or `Classic Network Contributor` role. Alternatively, your Azure account can be assigned to a custom role that can complete the necessary peering actions. For details, see [Permissions](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-peering?tabs=peering-portal#permissions).
    
- To create a peering, you need two virtual networks.
    
- The second virtual network in the peering is referred to as the _remote network_.
    
- Initially, the virtual machines in your virtual networks can't communicate with each other. After the peering is established, the machines can communicate within the peered network based on your configuration settings.
    
![](https://youtu.be/pSqDlQlcsLo)

## How to connect virtual networks across Azure regions with Azure Global VNet peering

## How to check your peering status

In the Azure portal, you can check the connectivity status of the virtual networks in your virtual network peering. The status conditions depend on how your virtual networks are deployed.

 Important

Your peering isn't successfully established until both virtual networks in the peering have a status of **Connected**.

- For deployment with the Azure Resource Manager, the two primary status conditions are **Initiated** and **Connected**. For the classic deployment model, the **Updating** status condition is also used.
    
- When you create the initial peering _to_ the second (remote) virtual network from the first virtual network, the peering status for the first virtual network is **Initiated**.
    
- When you create the subsequent peering _from_ the second virtual network to the first virtual network, the peering status for both the first and remote virtual networks is **Connected**. In the Azure portal, you can see the status for the first virtual network change from **Initiated** to **Connected**.


# Extend peering with user-defined routes and service chaining

Virtual network peering is nontransitive. The communication capabilities in a peering are available to only the virtual networks and resources in the peering. Other mechanisms have to be used to enable traffic to and from resources and networks outside the private peering network.

Suppose you have three virtual networks: A, B, and C. You establish virtual network peering between networks A and B, and also between networks B and C. You don't set up peering between networks A and C. The virtual network peering capabilities that you set up between networks B and C don't automatically enable peering communication capabilities between networks A and C.

### Things to know about extending peering

There are a few ways to extend the capabilities of your peering for resources and virtual networks outside your peering network:

- Hub and spoke networks
- User-defined routes
- Service chaining

You can implement these mechanisms and create a multi-level hub and spoke architecture. These options can help overcome the limit on the number of virtual network peerings per virtual network.

The following diagram shows a hub and spoke virtual network with an NVA and VPN gateway. The hub and spoke network is accessible to other virtual networks via user-defined routes and service chaining.

![Diagram that shows a hub virtual network with an NVA and VPN gateway that are accessible to other virtual networks.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-vnet-peering/media/service-chains-5c9286d1.png)

Expand table

|Mechanism|Description|
|---|---|
|**Hub and spoke network**|When you deploy a hub-and-spoke network, the hub virtual network can host infrastructure components like a network virtual appliance (NVA) or Azure VPN gateway. All the spoke virtual networks can then peer with the hub virtual network. Traffic can flow through NVAs or VPN gateways in the hub virtual network.|
|**User-defined route (UDR)**|Virtual network peering enables the next hop in a user-defined route to be the IP address of a virtual machine in the peered virtual network, or a VPN gateway.|
|**Service chaining**|Service chaining is used to direct traffic from one virtual network to a virtual appliance or gateway. A user-defined route defines the peered networks.|

# Interactive lab simulation

## Lab scenario

Your organization has three datacenters connected with a mesh wide-area network. As the Azure Administrator, you need to implement the on-premises infrastructure in Azure.

- There are two offices, New York and Boston, in one region.
- There's one office, Seattle, in another region.
- All the offices need to be networked together so they can share information.
- This simulation focuses on the connectivity of the offices, and not creating the individual Azure resources.

## Architecture diagram

![Architecture diagram as explained in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-vnet-peering/media/lab-05.png)

## Objectives

 Note

You may find slight differences between the interactive simulation and the Azure environment, but the core concepts and ideas being demonstrated are the same.

- **Task 1**: Create the infrastructure environment. In this task, you deploy three virtual machines. Virtual machines are deployed in different regions and virtual networks.
    - Use a template to create the virtual networks and virtual machines in the different regions. You can review the [lab templates](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/tree/master/Allfiles/Interactive%20Lab%20Simulation%20Files/05).
    - Use Azure PowerShell to deploy the template.
- **Task 2**: Configure local and global virtual network peering.
    - Create a local virtual network peering between the two virtual networks in the same region.
    - Create a global virtual network peering between virtual networks in different regions.
- **Task 3**: Test intersite connectivity between virtual machines on the three virtual networks.
    - Test the virtual machine connections in the same region.
    - Test the virtual machine connections in different regions.

 Note

select the thumbnail image to start the lab simulation. When you're done, be sure to return to this page so you can continue learning.

[![Screenshot of the simulation page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-vnet-peering/media/simulation-intersite-thumbnail.jpg)](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%209)

# Summary and resources

In this module, you learned Azure Virtual Network peering lets you connect virtual networks in a hub and spoke topology. You learned how to configure your virtual networks with Azure VPN Gateway for transit connectivity. You explored how to extend peering with user-defined routes and service chaining.

The main takeaways from this module are:

- Azure Virtual Network peering allows for the connection of virtual networks in a hub and spoke topology.
    
- There are two types of peering: regional and global. Regional peering connects virtual networks in the same region. Global peering connects virtual networks in different regions.
    
- Network traffic between peered virtual networks is private and kept on the Azure backbone network.
    
- You can configure Azure VPN Gateway in the peered virtual network as a transit point to access resources in another network.
    
- Network security groups can be applied to block or allow access between virtual networks when configuring virtual network peering.

---

# Configure network routing and endpoints

# Introduction

Administrators use network routes to control the flow of traffic through a network. Azure virtual networking provides capabilities to help you customize your network routes, establish service endpoints, and access private links.

In this module, suppose your company recently suffered a security incident that exposed customer personal information. This security incident has resulted in the loss of customers' confidential data and confidence. To address this scenario, the IT team has recommended implementing network virtual appliances (NVAs). You need to ensure traffic is properly routed through the virtual appliances. You're exploring other security options like service endpoints and private links.

## Learning objectives

In this module, you learn how to:

- Implement system routes and user-defined routes.
- Configure a custom route.
- Implement service endpoints.
- Identify features and usage cases for Azure Private Link and endpoint services.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104). The module concepts are covered in:

Configure and manage virtual networking (25–30%)

- Implement and manage virtual networking.
    - Configure user-defined network routes.
    - Configure endpoints on subnets.
    - Configure private endpoints.

## Prerequisites

- Familiarity with network routing.

# Review system routes

Azure uses _system routes_ to direct network traffic between virtual machines, on-premises networks, and the internet. Information about the system routes is recorded in a _route table_.

### Things to know about system routes

Let's take a closer look at how Azure implements system routes.

- Azure uses system routes to control traffic for virtual machines in several scenarios:
    
    - Traffic between virtual machines in the same subnet
    - Traffic between virtual machines in different subnets in the same virtual network
    - Traffic from virtual machines to the internet
- A route table contains a set of rules (called _routes_) that specifies how packets should be routed in a virtual network.
    
- Route tables record information about the system routes, where the tables are associated to subnets.
    
- Each packet leaving a subnet is handled based on the associated route table.
    
- Packets are matched to routes by using the destination. The destination can be an IP address, a virtual network gateway, a virtual appliance, or the internet.
    
- When a matching route can't be found, the packet is dropped.
    

#### Business scenario

Suppose you have a virtual network with two subnets. In this configuration, you can use Azure system routes to control communication between the subnets and between subnets and the internet. A front-end subnet can use a system route to access the internet. A back-end subnet can use a system route to access the front-end subnet. Both subnets access a route table. The following illustration highlights this scenario:

![Diagram that shows two subnets that use system routes as described in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-routing-endpoints/media/system-routes-08992506.png)

# Identify user-defined routes

Azure automatically handles all network traffic routing, but in some cases, a custom configuration is preferable. In these situations, you can configure _user-defined routes_ (UDRs) and _next hop_ targets.

### Things to know about user-defined routes

Let's examine the characteristics of user-defined routes.

- UDRs control network traffic by defining routes that specify the _next hop_ of the traffic flow.
    
- The next hop can be one of the following targets:
    
    - Virtual network gateway
    - Virtual network
    - Internet
    - Network virtual appliance (NVA)
- Similar to system routes, UDRs also access route tables.
    
- Each route table can be associated to multiple subnets.
    
- Each subnet can be associated to one route table only.
    
- There are no charges for creating route tables in Microsoft Azure.
    

#### Business scenario

Suppose you have a virtual machine that performs a network function like routing, firewalling, or WAN optimization. You want to direct certain subnet traffic to the NVA. To accomplish this configuration, you can place an NVA between subnets or between one subnet and the internet. The subnet can use a UDR to access the NVA and then the internet. The subnet can use another UDR and NVA to access the back-end subnet. The following illustration highlights this scenario:

![Diagram that shows two subnets that use a UDR to access an NVA as described in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-routing-endpoints/media/user-defined-routes-2417e693.png)

# Determine service endpoint uses

A virtual network _service endpoint_ provides the identity of your virtual network to the Azure service. After service endpoints are enabled in your virtual network, you can secure Azure service resources to your virtual network by adding a _virtual network rule_ to the resources.

Today, Azure service traffic from a virtual network uses public IP addresses as source IP addresses. With service endpoints, service traffic switches to use virtual network private addresses as the source IP addresses when accessing the Azure service from a virtual network. This switch allows you to access the services without the need for reserved public IP addresses that are typically used in IP firewalls.

### Things to know about service endpoints

Review the following characteristics of service endpoints.

- Service endpoints can extend your virtual network identity to your Azure services to secure your service resources.
    
- You secure your Azure service resources to your virtual network by using virtual network rules.
    
- Virtual network rules can remove public internet access to resources, and allow traffic only from your virtual network.
    
- Service endpoints always take service traffic directly from your virtual network to the service on the Microsoft Azure backbone network.
    
- Service endpoints are configured through the subnet. No extra overhead is required to maintain the endpoints.
    

The following illustration shows a virtual machine connecting to the Azure service through a service endpoint. A virtual machine in a subnet accesses an Azure Storage account through a service endpoint. Virtual network rules allow the virtual machine to access the Azure service resource, but not communicate with the internet.

![Diagram of a virtual machine in a subnet connecting to an Azure service through a service endpoint.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-routing-endpoints/media/service-endpoint-addresses-a027197f.png)

### Things to consider when using service endpoints

There are several scenarios where using service endpoints can be advantageous. Review the following points and think about how you can implement service endpoints in your configuration.

- **Consider improved security for resources**. Implement service endpoints to improve the security of your Azure service resources. When service endpoints are enabled in your virtual network, you secure Azure service resources to your virtual network with virtual network rules. The rule improves security by fully removing public internet access to resources, and allowing traffic only from your virtual network.
    
- **Consider optimal routing for service traffic**. Routes in your virtual network that force internet traffic to your on-premises or network virtual appliances also typically force Azure service traffic to take the same route as the internet traffic. This traffic control process is known as _forced-tunneling_. Service endpoints provide optimal routing for Azure service traffic to allow you to circumvent forced tunneling.
    
- **Consider direct traffic to the Microsoft network**. Use service endpoints to keep traffic on the Azure backbone network. This approach allows you to continue auditing and monitoring outbound internet traffic from your virtual networks, through forced-tunneling, without impacting service traffic. Learn more about [user-defined routes and forced-tunneling](https://learn.microsoft.com/en-us/azure/firewall/forced-tunneling).
    
- **Consider easy configuration and maintenance**. Configure service endpoints in your subnets for simple setup and low maintenance. You no longer need reserved public IP addresses in your virtual networks to secure Azure resources through an IP firewall. There are no NAT or gateway devices required to set up the service endpoints.
    

> Note
>
>With service endpoints, the virtual machine IP addresses switch from public to private IPv4 addresses. Existing Azure service firewall rules that use Azure public IP addresses stop working after the switch. Ensure Azure service firewall rules allow for this switch before you set up service endpoints. You might also experience temporary interruption to service traffic from this subnet while configuring service endpoints.

# Determine service endpoint services

It's easy to add a service endpoint to the virtual network. In the Azure portal, you select the Azure service for which to create the endpoint. In this unit, we examine several services, including Azure Cosmos DB, Event Hubs, Key Vault, and SQL Database.

![Screenshot of the Service endpoints page in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-routing-endpoints/media/add-service-endpoints-5df9ecfc.png)

 Note

Adding service endpoints can take up to 15 minutes to complete. Each service endpoint integration has its own Azure documentation page.

Expand table

|Service|Availability|Description|
|---|---|---|
|**Azure Storage**|Generally available in all Azure regions|This endpoint gives traffic an optimal route to the Azure Storage service. Each Storage account supports up to 100 virtual network rules.|
|**Azure SQL Database and Azure SQL Data Warehouse**|Generally available in all Azure regions|A firewall security feature controls whether your database accepts communication from particular subnets in virtual networks. This feature applies to the database server for your single databases and elastic pool in SQL Database or your databases in SQL Data Warehouse.|
|**Azure Database for PostgreSQL and Azure Database for MySQL**|Generally available in Azure regions where database service is available|Virtual network service endpoints and rules extend the private address space of a virtual network to your Azure Database for PostgreSQL server and Azure Database for MySQL server.|
|**Azure Cosmos DB**|Generally available in all Azure regions|You can configure the Azure Cosmos DB account to allow access only from a specific subnet of virtual network. Enable service endpoints to access Azure Cosmos DB on the subnet within a virtual network. Traffic from the subnet is sent to Azure Cosmos DB with the identity of the subnet and virtual network. After the Azure Cosmos DB service endpoint is enabled, you can limit access to the subnet by adding it to your Azure Cosmos DB account.|
|**Azure Key Vault**|Generally available in all Azure regions|The virtual network service endpoints for Key Vault allow you to restrict access to a specified virtual network. The endpoints also allow you to restrict access to a list of IPv4 (internet protocol version 4) address ranges. Any user connecting to your key vault from outside those sources is denied access.|
|**Azure Service Bus and Azure Event Hubs**|Generally available in all Azure regions|The integration of Service Bus with virtual network service endpoints enables secure access to messaging capabilities from workloads like virtual machines that are bound to virtual networks. The network traffic path is secured on both ends.|

# Identify private link uses

Azure Private Link provides private connectivity from a virtual network to Azure platform as a service (PaaS), customer-owned, or Microsoft partner services. It simplifies the network architecture and secures the connection between endpoints in Azure by eliminating data exposure to the public internet.

### Things to know about Azure Private Link

Let's examine the characteristics of Azure Private Link and network routing configurations.

- Azure Private Link keeps all traffic on the Microsoft global network. There's no public internet access.
    
- Private Link is global and there are no regional restrictions. You can connect privately to services running in other Azure regions.
    
- Services delivered on Azure can be brought into your private virtual network by mapping your network to a private endpoint.
    
- Private Link can privately deliver your own services in your customer's virtual networks.
    
- All traffic to the service can be routed through the private endpoint. No gateways, NAT devices, Azure ExpressRoute or VPN connections, or public IP addresses are required.
    

The following Illustration demonstrates a network routing configuration with Azure Private Link. The service connects to a network security group (NSG) private endpoint by using Azure SQL Database. This configuration prevents a direct connection.

![Diagram that shows a network routing configuration with Azure Private Link as described in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-routing-endpoints/media/private-links-602b4a62.png)

### Things to consider when using Azure Private Link

There are many benefits to working with Azure Private Link. Review the following points and consider how you can implement the service for your scenarios.

- **Consider private connectivity to services on Azure**. Connect privately to services running in other Azure regions. Traffic remains on the Microsoft network with no public internet access.
    
- **Consider integration with on-premises and peered networks**. Access private endpoints over private peering or VPN tunnels from on-premises or peered virtual networks. Microsoft hosts the traffic, so you don't need to set up public peering or use the internet to migrate your workloads to the cloud.
    
- **Consider protection against data exfiltration for Azure resources**. Map private endpoints to Azure PaaS resources. When there's a security incident within your network, only the mapped resources are accessible. This implementation eliminates the threat of data exfiltration.
    
- **Consider services delivered directly to customer virtual networks**. Privately consume Azure PaaS, Microsoft partner, and your own services in your virtual networks on Azure. Private Link works across tenants to help unify your experience across services. Send, approve, or reject requests directly without permissions or role-based access controls.

# Interactive lab simulation

## Lab scenario

Your organization is exploring Azure virtual networking capabilities. As the Azure Administrator you've been tasked to implement the following requirements:

- Create and configure a virtual network in Azure.
- Deploy two virtual machines into different subnets of the virtual network.
- Ensure the virtual machines have public IP addresses that won't change over time.
- Protect the virtual machine public endpoints from being accessible from the internet.
- Ensure internal Azure virtual machines names and IP addresses can be resolved.
- Ensure a publicly available domain name can be resolved by external queries.

## Architecture diagram

![Architecture diagram as explained in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-routing-endpoints/media/lab-04.png)

 Note

Tasks 1 - 4 focus on IP addresses and access.

## Objectives

 Note

You may find slight differences between the interactive simulation and the Azure environment, but the core concepts and ideas being demonstrated are the same.

- **Task 1**: Create and configure a virtual network in Azure.
    - Create a virtual network, **az104-04-vnet1**.
    - Add two subnets, **Subnet0** and **Subnet1**, to the virtual network.
- **Task 2**: Deploy virtual machines into different subnets of the virtual network.
    - Review a JSON template that will deploy two virtual machines, **VM0** and **VM1**.
    - Use Azure PowerShell to deploy the template.
- **Task 3**: Configure private and public IP addresses of Azure virtual machines. Ensure the IP addresses don't change over time.
    - Associate the VM0 NIC with a static public IP address, **az104-04-pip0**.
    - Associate the VM1 NIC with a static public IP address, **az104-04-pip1**.
- **Task 4**: Configure network security groups. Protect the virtual machine public endpoints from being accessible from the internet.
    - Verify you can't use RDP to connect to a virtual machine.
    - Create a network security group.
    - Configure inbound security rules to allow RDP.
    - Associate the network security group with the virtual machine NICs.
    - Confirm that you can now use RDP to connect to a virtual machine.
- **Task 5**: Configure Azure DNS for internal name resolution. Ensure internal Azure virtual machines names and IP addresses can be resolved.
    - Create a private DNS zone for your organization.
    - Add a virtual network link to the virtual network.
    - Verify the virtual machines DNS records are registered.
    - Verify internal DNS name resolution is working.
- **Task 6**: Configure Azure DNS for external name resolution. Ensure a publicly available domain name can be resolved by external queries.
    - Create a DNS zone for a publicly available domain name.
    - Add a DNS record for each virtual machine.
    - Verify external DNS name resolution is working.

 Note

Select the thumbnail image to start the lab simulation. When you're done, be sure to return to this page so you can continue learning.

[![Screenshot of the simulation page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-routing-endpoints/media/simulation-networks-thumbnail.jpg)](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208)
# Summary and resources

Network routes control the flow of traffic through your network. You can customize these routes, implement service endpoints, and work with private links.

In this module, you learned how to implement system routes and user-defined routes. You identified features and usage cases for Azure Private Link and endpoint services. You explored how to configure a custom route, and discovered how to work with service endpoints.

## Learn more

- Peruse [virtual network traffic routing documentation](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview).
    
- Read about [Virtual network traffic routing](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview).
    
- Route [network traffic with a route table by using the Azure portal](https://learn.microsoft.com/en-us/azure/virtual-network/tutorial-create-route-table-portal).
    
- Configure [BGP for Azure VPN Gateway by using PowerShell](https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).
    
- Create [custom routes](https://learn.microsoft.com/en-us/azure/virtual-network/manage-route-table#create-a-route).
    
- View [all routes for a subnet and diagnose virtual machine routing problems](https://learn.microsoft.com/en-us/azure/virtual-network/diagnose-network-routing-problem).
    
- Determine [the next hop type between a virtual machine and a destination IP address](https://learn.microsoft.com/en-us/azure/network-watcher/diagnose-vm-network-routing-problem#use-next-hop).
    
- Explore [user-defined routes and forced-tunneling in Azure Firewall](https://learn.microsoft.com/en-us/azure/firewall/forced-tunneling).
    

## Learn more with self-paced training

- Complete an [introduction to Azure Private Link](https://learn.microsoft.com/en-us/training/modules/introduction-azure-private-link/).

## Learn more with optional hands-on exercises

- Manage and control [traffic flow in your Azure deployment with routes (sandbox)](https://learn.microsoft.com/en-us/training/modules/control-network-traffic-flow-with-routes/).

---

# Configure Azure Load Balancer

# Introduction

Many applications need to be resilient to failure and scale easily when demand increases. Administrators can address these requirements by using Azure Load Balancer.

Suppose your healthcare organization is launching a new portal application for patients to schedule appointments. The application has a patient portal, web application frontend, and business tier database. The database is used by the frontend to retrieve and save patient information.

The new portal needs to be available around the clock to handle failures. The portal must adjust to fluctuations in load by adding and removing resources to match the load. You need a solution to distribute work to virtual machines across the system as virtual machines are added. The solution should detect failures and reroute jobs to virtual machines as needed. Improved resiliency and scalability are required to help ensure patients can schedule appointments from any location.

You're responsible for configuring the load balancers to distribute incoming network traffic across a group of back-end servers. You need to scale your applications while maintaining throughput and keeping response times low.

The goal of this module is to equip you to implement an Azure load balancer.

## Learning objectives

In this module, you learn how to:

- Identify features and usage cases for Azure Load Balancer.
- Implement public and internal Azure load balancers.
- Compare features of load balancer SKUs and configuration differences.
- Configure back-end pools, load-balancing rules, session persistence, and health probes.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104).

## Prerequisites

- Basic knowledge of virtual networks and routing.
- Familiarity with the Azure portal so you can configure the load balancer.

# Determine Azure Load Balancer uses

Azure Load Balancer delivers high availability and network performance to your applications. Administrators use load balancing to efficiently distribute incoming network traffic across back-end servers and resources. A load balancer is implemented by using load-balancing rules and health probes.

The following diagram shows how Azure Load Balancer works. The frontend exchanges information with a load balancer. The load balancer uses rules and health probes to communicate with the backend.

![Diagram that shows how a load balancer works as described in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-load-balancer/media/load-balancer-4caf947b.png)

### Things to know about Azure Load Balancer

Let's take a closer look at how Azure Load Balancer operates.

- Azure Load Balancer can be used for inbound and outbound scenarios.
    
- You can implement a **public** or **internal** load balancer, or use both types in a combination configuration.
    
- To implement a load balancer, you configure four components:
    
    - Front-end IP configuration
    - Back-end pools
    - Health probes
    - Load-balancing rules
- The front-end configuration specifies the public IP or internal IP that your load balancer responds to.
    
- The back-end pools are your services and resources, including Azure Virtual Machines or instances in Azure Virtual Machine Scale Sets.
    
- Load-balancing rules determine how traffic is distributed to back-end resources.
    
- Health probes ensure the resources in the backend are healthy.
    
- Load Balancer scales up to millions of TCP and UDP application flows.

# Implement a public load balancer

Administrators use public load balancers to map the public IP addresses and port numbers of incoming traffic to the private IP addresses and port numbers of virtual machines. The mapping can also be configured for response traffic from the virtual machines.

Load-balancing rules are used to specify how to distribute specific types of traffic across multiple virtual machines or services. You can use this approach to share the load of incoming web request traffic across multiple web servers.

#### Business scenario

Consider a scenario where internet traffic attempts to reach virtual machines in a web tier subnet that implements a public load balancer. Internet clients send webpage requests to the public IP address of a web app on TCP port 80. Azure Load Balancer intercepts the traffic and distributes the requests across the virtual machines in the load-balanced set according to the defined load-balancing rules. The following illustration highlights this scenario:

![Diagram showing how a public load balancer works as described in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-load-balancer/media/public-load-balancer-46d5d9fe.png)

# Implement an internal load balancer

Administrators use internal load balancers to direct traffic to resources that reside in a virtual network, or to resources that use a VPN to access Azure infrastructure. In this configuration, front-end IP addresses and virtual networks are never directly exposed to an internet endpoint. Internal line-of-business applications run in Azure and are accessed from within Azure or from on-premises resources.

#### Business scenario

Suppose you have an Azure SQL Database tier subnet with several virtual machines, and you implement an internal load balancer. Database requests need to be distributed to the backend. The internal load balancer receives the database requests and uses the load-balancing rules to determine how to distribute the requests to the back-end SQL servers. The SQL servers respond on port 1433. The following illustration highlights this scenario:

![Diagram showing how an internal load balancer works as described in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-load-balancer/media/internal-load-balancer-5ae85589.png)

### Things to consider when using an internal load balancer

You can implement an internal load balancer to achieve several types of load balancing.

- **Within virtual network**: Establish load balancing from your virtual machines in the virtual network to a set of virtual machines that reside within the same virtual network.
    
- **For cross-premises virtual network**: Apply load balancing from your on-premises computers to a set of virtual machines that reside within the same virtual network.
    
- **For multi-tier applications**: Implement load balancing for your internet-facing multi-tier applications when the back-end tiers aren't internet-facing. The back-end tiers require traffic load-balancing from the internet-facing tier.
    
- **For line-of-business applications**: Add load balancing for your line-of-business applications hosted in Azure without having to add other load balancer hardware or software. This scenario includes on-premises servers that are in the set of computers whose traffic is load-balanced.
    
- **With public load balancer**: Configure a public load balancer in front of your internal load balancer to create a multi-tier application.

# Determine load balancer SKUs

When you create an Azure load balancer in the Azure portal, you select the type of load balancer to create (internal or public) and the Stock Keeping Unit (SKU). Azure Load Balancer supports three SKU options: Basic, Standard, and Gateway. Each SKU provides different features, scenario scaling, and pricing.

![Screenshot that shows how to create an Azure load balancer in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-load-balancer/media/load-balancer-types-a4c0eceb.png)

### Things to know about Azure Load Balancer SKUs

Let's review some points to consider when choosing the SKU type for your load balancer.

- Standard Load Balancer is the newest product. It's essentially a superset of Basic Load Balancer.
    
- The Standard SKU offers an expanded and more granular feature set than the Basic SKU.
    
- The Basic SKU can be upgraded to the Standard SKU. But, new designs and architectures should use the Standard SKU.
    
- The Gateway SKU supports high performance and high availability scenarios with third-party network virtual appliances (NVAs).
    

## Compare Basic and Standard SKU features

The following table provides a brief comparison of how features are implemented in the Standard and Basic SKUs.

Expand table

|Feature|Basic SKU|Standard SKU|
|---|---|---|
|**Health probes**|HTTP, TCP|HTTPS, HTTP, TCP|
|**Availability zones**|Not available|Zone-redundant and zonal frontends for inbound and outbound traffic|
|**Multiple frontends**|Inbound only|Inbound and outbound|
|**Security**|- Open by default  <br>- (Optional) Control through network security groups (NSGs)|- Closed to inbound flows unless allowed by an NSG  <br>- Internal traffic from the virtual network to the internal load balancer is allowed|

# Create back-end pools

Each load balancer has one or more back-end pools that are used for distributing traffic. The back-end pools contain the IP addresses of the virtual NICs that are connected to your load balancer. You configure these pool settings in the Azure portal.

![Screenshot that shows how to configure back-end pools in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-load-balancer/media/backend-pools-1984adb4.png)

### Things to know about back-end pools

The SKU type that you select determines which endpoint configurations are supported for the pool along with the number of pool instances allowed.

- The Basic SKU allows up to 300 pools, and the Standard SKU allows up to 1,000 pools.
    
- When you configure the back-end pools, you can connect to availability sets, virtual machines, or Azure Virtual Machine Scale Sets.
    
- For the Basic SKU, you can select virtual machines in a single availability set or virtual machines in an instance of Azure Virtual Machine Scale Sets.
    
- For the Standard SKU, you can select virtual machines or Virtual Machine Scale Sets in a single virtual network. Your configuration can include a combination of virtual machines, availability sets, and Virtual Machine Scale Sets.

# Create health probes

A health probe allows your load balancer to monitor the status of your application. The probe dynamically adds or removes virtual machines from your load balancer rotation based on the machine response to health checks. When a probe fails to respond, the load balancer stops sending new connections to the unhealthy instance.

The following image shows how to create a health probe in the Azure portal. A custom HTTP health probe is configured to run on TCP port 80. The probe is defined to check the health of the virtual machine instances at 5-second intervals.

![Screenshot that shows how to create a health probe in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-load-balancer/media/add-health-probe-1d86fb2b.png)

### Things to know about health probes

There are two main ways to configure a custom health probe: **HTTP** and **TCP**.

- In an **HTTP probe**, the load balancer probes your back-end pool endpoints every 15 seconds. A virtual machine instance is considered _healthy_ if it responds with an HTTP 200 message within the specified timeout period (default is 31 seconds). If any status other than HTTP 200 is returned, the instance is considered _unhealthy_, and the probe fails.
    
- A **TCP probe** relies on establishing a successful TCP session to a defined probe port. If the specified listener on the virtual machine exists, the probe succeeds. If the connection is refused, the probe fails.
    
- To configure a probe, you specify values for the following settings:
    
    - **Port**: Back-end port
    - **URI**: URI for requesting the health status from the backend
    - **Interval**: Amount of time between probe attempts (default is 15 seconds)
    - **Unhealthy threshold**: Number of failures that must occur for the instance to be considered unhealthy
- A **Guest agent probe** is a third option that uses the guest agent inside the virtual machine. This option isn't recommended when an HTTP or TCP custom probe configuration is possible.

# Create load balancer rules

You can define load-balancing rules to specify how traffic is distributed to your back-end pools. Each rule maps a front-end IP address and port combination to a set of back-end IP address and port combinations.

![Screenshot that shows how to create load-balancing rules in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-load-balancer/media/add-load-balancer-rules-f4d9b188.png)

### Things to know about load-balancing rules

Let's take a closer look at how to configure load-balancing rules for your back-end pools.

- To configure a load-balancing rule, you need to have a frontend, backend, and health probe for your load balancer.
    
- To define a rule in the Azure portal, you configure several settings:
    
    - **IP version** (IPv4 or IPv6)
    - **Front-end IP address**, *_Port_, and **Protocol** (TCP or UDP)
    - **Back-end pool** and **Back-end port**
    - **Health probe**
    - **Session persistence**
- By default, Azure Load Balancer distributes network traffic equally among multiple virtual machines.
    
    Azure Load Balancer uses a five-tuple hash to map traffic to available servers. The tuple consists of the source IP address, source port, destination IP address, destination port, and protocol type. The load balancer provides stickiness only within a transport session.
    
- **Session persistence** specifies how to handle traffic from a client. By default, successive requests from a client go to any virtual machine in your pool.
    
    You can modify the session persistence behavior as follows:
    
    - **None (default)**: Any virtual machine can handle the request.
    - **Client IP**: Successive requests from the same client IP address go to the same virtual machine.
    - **Client IP and protocol**: Successive requests from the same client IP address and protocol combination go to the same virtual machine.
    
     Note
    
    Maintaining session persistence information is important for applications that implement a shopping cart. Can you think of other applications that might benefit from session persistence?
    
- Load-balancing rules can be used in combination with NAT rules.
    
    Consider a scenario where you use NAT from a load balancer's public address to TCP port 3389 on a specific virtual machine. By combining your NAT rule with load-balancing rules, you can enable remote desktop access from outside of Azure.

# Interactive lab simulation

## Lab scenario

Your organization is migrating hub and spoke network topologies to Azure. As the Azure Administrator you need to:

- Replicate the on-premises functionality in Azure.
- Configure virtual network peering and traffic routing.
- Implement load balancer and application gateway functionality.
- Test to ensure traffic management is flowing as intended.

## Architecture diagram

![Architecture diagram as explained in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-load-balancer/media/lab-06.png)

## Objectives

- **Task 1: Provision the lab environment.** In this task, you deploy four virtual machines into the same Azure region. The first two reside in a hub virtual network, while the remaining two reside in a separate spoke virtual network.
    - Review an [Azure Resource Manager template](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/tree/master/Allfiles/Interactive%20Lab%20Simulation%20Files/06).
    - This template includes the virtual machines and virtual networks in the underlying architecture.
    - Use Azure PowerShell to install the Network Watcher extension on the Azure virtual machines.
- **Task 2: Configure the hub and spoke network topology.** In this task, you configure local peering between the virtual networks you deployed in the previous tasks in order to create a hub and spoke network topology.
    - Configure virtual network peering between the virtual networks.
    - Ensure forwarded traffic is allowed to facilitate routing between spoke virtual networks.
- **Task 3: Test transitivity of virtual network peering.** In this task, you test transitivity of virtual network peering by using Network Watcher.
    - Use Network Watcher to verify peered networks are reachable.
    - Use Network Watcher to verify unpeered networks are unreachable.
- **Task 4: Configure routing in the hub and spoke topology**. In this task, you configure and test routing between the two spoke virtual networks.
    - Enable IP forwarding on a virtual machine.
    - Install the remote access Windows feature with associated tools.
    - Create routing tables and associate them with the appropriate subnets.
    - Use Network Watcher to verify traffic routed through the virtual machine.
- **Task 5: Implement Azure Load Balancer**. In this task, you implement an Azure load balancer in front of the two Azure virtual machines in the hub virtual network.
    - Create a load balancer with a public IP address.
    - Create a back-end pool that includes the virtual machines.
    - Add a load balancing rule to alternate between virtual machines in the back-end pool.
    - Test to confirm that the load balancer is working correctly.
- **Task 6: Implement Azure Application Gateway**. In this task, you implement an Azure application gateway in front of the two Azure virtual machines in the spoke virtual networks.
    - Create a dedicated subnet for the application gateway.
    - Create an application gateway with a public IP address.
    - Configure the application gateway back-end pool to include the virtual machines.
    - Test to ensure traffic is balanced across the back-end virtual machines.

 Note

Select the thumbnail image to start the lab simulation. When you're done, be sure to return to this page so you can continue learning.

[![Screenshot of the simulation page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-load-balancer/media/simulation-traffic-thumbnail.jpg)](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2010)

# Summary and resources

In this module, you learned about Azure Load Balancer and its features. Azure Load Balancer distributed workloads and network traffic across virtual machines, making applications more resilient and scalable. You learned about load balancer SKUs, back-end pools, load-balancing rules, session persistence, and health probes.

The main takeaways from this module are:

- Azure Load Balancer helps distribute network traffic across servers and resources.
    
- Load balancing can be used for inbound and outbound scenarios.
    
- There are public and internal load balancers.
    
- Load-balancing rules specify how traffic is distributed to your back-end pools.
    
- Back-end pools contain the IP addresses of the virtual NICs that are connected to your load balancer.
    
- Health probes dynamically add or remove virtual machines based on virtual machine health checks.
    

## Learn more with documentation

- [Azure Load Balancer documentation](https://learn.microsoft.com/en-us/azure/load-balancer/). This collection of articles is your starting point for all things load balancer.
    
- [Create a public load balancer for virtual machines in the Azure portal](https://learn.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal). This article reviews creating a public load balancer for a backend pool with two virtual machines.
    

## Learn more with self-paced training

- [Introduction to Azure Load Balancer](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-load-balancer/). Learn what Azure Load Balancer does, how it works, and when you should choose to use Load Balancer as a solution
    
- [Improve application scalability and resiliency by using Azure Load Balancer (sandbox)](https://learn.microsoft.com/en-us/training/modules/improve-app-scalability-resiliency-with-load-balancer/). Learn about the different load balancers in Azure and how to choose the right Azure load balancer solution.
    
- [Load balance non-HTTP(S) traffic in Azure](https://learn.microsoft.com/en-us/training/modules/load-balancing-non-https-traffic-azure/). Learn the different load balancer options in Azure and how to choose and implement the right Azure solution for non-HTTP(S) traffic.

---

# Configure Azure Application Gateway


# Introduction

Azure Application Gateway is a load balancer for web traffic. Administrators implement an application gateway to manage traffic to their web apps.

Suppose you work for the motor vehicle department of a governmental organization. The department runs several public websites that enable drivers to register their vehicles, and renew their licenses online. The vehicle registration website has been running on a single server, and has suffered multiple outages because of server failures. The outages have resulted in frustrated drivers trying to register their vehicles before their registrations expire.

You're responsible for improving the resiliency of the site by adding multiple web servers to distribute the load. You want to centralize the site on a single load-balancing service, and simplify the URLs for site visitors. You're researching how to implement Azure Application Gateway.

## Learning objectives

In this module, you learn how to:

- Identify features and usage cases for Azure Application Gateway.
- Implement an Azure application gateway, including selecting a routing method.
- Configure gateway components, such as listeners, health probes, and routing rules.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104). The module concepts are covered in:

Configure and manage virtual networking (25–30%)

- Configure load balancing.
    - Configure Azure Application Gateway.

# Implement Azure Application Gateway

Administrators use Azure Application Gateway to manage requests from client applications to their web apps. An application gateway listens for incoming traffic to web apps and checks for messages sent via protocols like HTTP. Gateway rules direct the traffic to resources in a back-end pool.

#### Business scenario

Consider a scenario where internet client applications request access to resources in a load-balanced back-end pool. The requests can be managed by implementing Azure Application Gateway to listen for HTTP(S) messages. Messages can be handled by load-balancing rules to direct client request traffic to the appropriate resources in the pool. The following diagram illustrates this scenario:

![Diagram that illustrates how Azure Application Gateway manages requests from client applications to resources in a back-end pool, as described in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-application-gateway/media/application-gateway-cb3392f4.png)

### Things to know about Azure Application Gateway

Let's examine some of the benefits of using Azure Application Gateway to manage internet traffic to your web applications.

| Benefit                        | Description                                                                                                                                                                                                                                                                        |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Application layer routing**  | Use application layer routing to direct traffic to a back-end pool of web servers based on the URL of a request. The back-end pool can include Azure virtual machines, Azure Virtual Machine Scale Sets, Azure App Service, and even on-premises servers.                          |
| **Round-robin load balancing** | Employ round-robin load balancing to distribute incoming traffic across multiple servers. Send load-balance requests to the servers in each back-end pool. Client requests are forwarded in a cycle through a group of servers to create an effective balance for the server load. |
| **Session stickiness**         | Apply session stickiness to your application gateway to ensure client requests in the same session are routed to the same back-end server.                                                                                                                                         |
| **Supported protocols**        | Build an application gateway to support the HTTP, HTTPS, HTTP/2, or WebSocket protocols.                                                                                                                                                                                           |
| **Firewall protection**        | Implement a web application firewall to protect against web application vulnerabilities.                                                                                                                                                                                           |
| **Encryption**                 | Support end-to-end request encryption for your web applications.                                                                                                                                                                                                                   |
| **Load autoscaling**           | Dynamically adjust capacity as your web traffic load changes.                                                                                                                                                                                                                      |


# Determine Azure Application Gateway routing

Clients send requests to your web apps by specifying the IP address or DNS name of your application gateway. Your gateway directs the requests to a selected web server in your back-end pool according to a set of rules. You define the rules for your gateway to identify the allowable routes for the request traffic.

### Things to know about traffic routing

Let's take a closer look at your routing options for Azure Application Gateway.

- Azure Application Gateway offers two primary methods for routing traffic:
    
    - **Path-based routing** sends requests with different URL paths to different pools of back-end servers.
        
    - **Multi-site routing** configures more than one web application on the same application gateway instance.
        
- You can configure your application gateway to **redirect** traffic.
    
    Application Gateway can redirect traffic received at one listener to another listener, or to an external site. This approach is commonly used by web apps to automatically redirect HTTP requests to communicate via HTTPS. The redirection ensures all communication between your web app and clients occurs over an encrypted path.
    
- You can implement Application Gateway to **rewrite HTTP headers**.
    
    HTTP headers allow the client and server to pass parameter information with the request or the response. In this scenario, you can translate URLs or query string parameters, and modify request and response headers. Add conditions to ensure URLs or headers are rewritten only for certain conditions.
    
- Application Gateway allows you to create custom error pages instead of displaying default error pages. You can use your own branding and layout by using a custom error page.
    

#### Path-based routing

You can implement path-based routing to direct requests for specific URL paths to the appropriate back-end pool. Consider a scenario where your web app receives requests for videos or images. You can use path-based routing to direct requests for the `/video/\*` path to a back-end pool of servers that are optimized to handle video streaming. Image requests for the `/images/\*` path can be directed to a pool of servers that handle image retrieval. The following illustration demonstrates this routing method:

![Diagram that shows a path-based routing approach.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-application-gateway/media/path-based-routing-15bcef5f.png)

#### Multi-site routing

When you need to support multiple web apps on the same application gateway instance, multi-site routing is the best option. Multi-site configurations are useful for supporting multi-tenant applications, where each tenant has its own set of virtual machines or other resources hosting a web application.

In this configuration, you register multiple DNS names (CNAMEs) for the IP address of your application gateway and specify the name of each site. Application Gateway uses separate listeners to wait for requests for each site. Each listener passes the request to a different rule, which can route the requests to servers in a different back-end pool.

Consider a scenario where you need to support traffic to two sites on the same gateway. You can direct all requests for the `http://contoso.com` site to servers in one back-end pool, and requests for the `http://fabrikam.com` site to another back-end pool. The following illustration demonstrates this routing method.

![Diagram that shows a multiple site routing approach.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-application-gateway/media/site-based-routing-e686b605.png)

# Configure Azure Application Gateway components

Azure Application Gateway has a series of components that combine to route requests to a pool of web servers and to check the health of these web servers. These components include the frontend IP address, back-end pools, routing rules, health probes, and listeners. As an option, the gateway can also implement a firewall.

### Things to know about Application Gateway components

Let's explore how the components of an application gateway work together.

- The **front-end IP address** receives the client requests.
    
- An optional **Web Application Firewall** checks incoming traffic for common threats before the requests reach the listeners.
    
- One or more **listeners** receive the traffic and route the requests to the back-end pool.
    
- **Routing rules** define how to analyze the request to direct the request to the appropriate back-end pool.
    
- A **back-end pool** contains web servers for resources like virtual machines or Virtual Machine Scale Sets. Each pool has a load balancer to distribute the workload across the resources.
    
- **Health probes** determine which back-end pool servers are available for load-balancing.
    

The following flowchart demonstrates how the Application Gateway components work together to direct traffic requests between the frontend and back-end pools in your configuration.

![Flowchart that demonstrates how Application Gateway components direct traffic requests between the frontend and back-end pools.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-application-gateway/media/configure-app-gateway-0193dbd6.png)

#### Front-end IP address

Client requests are received through your front-end IP address. Your application gateway can have a public or private IP address, or both. You can have only one public IP address and only one private IP address.

#### Web Application Firewall (optional)

You can enable Azure Web Application Firewall for Azure Application Gateway to handle incoming requests before they reach your listener. The firewall checks each request for threats based on the Open Web Application Security Project (OWASP). Common threats include SQL-injection, cross-site scripting, command injection, HTTP request smuggling and response splitting, and remote file inclusion. Other threats can come from bots, crawlers, scanners, and HTTP protocol violations and anomalies.

OWASP defines a set of generic rules for detecting attacks. These rules are referred to as the Core Rule Set (CRS). The rule sets are under continuous review as attacks evolve in sophistication. Azure Web Application Firewall supports two rule sets: CRS 2.2.9 and CRS 3.0. CRS 3.0 is the default and more recent of these rule sets. If necessary, you can opt to select only specific rules in a rule set to target certain threats. Additionally, you can customize the firewall to specify which elements in a request to examine, and limit the size of messages to prevent massive uploads from overwhelming your servers.

#### Listeners

Listeners accept traffic arriving on a specified combination of protocol, port, host, and IP address. Each listener routes requests to a back-end pool of servers according to your routing rules. A listener can be _Basic_ or _Multi-site_. A Basic listener only routes a request based on the path in the URL. A Multi-site listener can also route requests by using the hostname element of the URL. Listeners also handle TLS/SSL certificates for securing your application between the user and Application Gateway.

#### Routing rules

A routing rule binds your listeners to the back-end pools. A rule specifies how to interpret the hostname and path elements in the URL of a request, and then direct the request to the appropriate back-end pool. A routing rule also has an associated set of HTTP settings. These HTTP settings indicate whether (and how) traffic is encrypted between Application Gateway and the back-end servers. Other configuration information includes protocol, session stickiness, connection draining, request timeout period, and health probes.

#### Back-end pools

A back-end pool references a collection of web servers. You provide the IP address of each web server and the port on which it listens for requests when configuring the pool. Each pool can specify a fixed set of virtual machines, Virtual Machine Scale Sets, an app hosted by Azure App Services, or a collection of on-premises servers. Each back-end pool has an associated load balancer that distributes work across the pool.

#### Health probes

Health probes determine which servers in your back-end pool are available for load-balancing. Application Gateway uses a health probe to send a request to a server. When the server returns an HTTP response with a status code between 200 and 399, the server is considered healthy. If you don't configure a health probe, Application Gateway creates a default probe that waits for 30 seconds before identifying a server as unavailable (unhealthy).

# Summary and resources

Azure Application Gateway provides load balancing and application routing capabilities across multiple web sites. Several routing methods are available, including multi-site and path-based. Application Gateway also provides Azure Web Application Firewall to supply built-in security features.

In this module, you identified features and usage cases for Azure Application Gateway. You explored Application Gateway components, including listeners, firewalls, health probes, and routing rules. You learned how to implement an application gateway, including selecting the appropriate routing method.

## Learn more

- Read about [Azure Application Gateway](https://learn.microsoft.com/en-us/azure/application-gateway/overview).
    
- Examine [Application Gateway components](https://learn.microsoft.com/en-us/azure/application-gateway/application-gateway-components).
    
- Discover [Application Gateway features](https://learn.microsoft.com/en-us/azure/application-gateway/features).
    
- Read about [Azure Web Application Firewall on Application Gateway](https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/ag-overview).
    
- Explore [Application Gateway redirection routing](https://learn.microsoft.com/en-us/azure/application-gateway/redirect-overview).
    
- Configure an [application gateway to host multiple web sites](https://learn.microsoft.com/en-us/azure/application-gateway/create-multiple-sites-portal).
    
- Rewrite [HTTP headers and URL with Application Gateway](https://learn.microsoft.com/en-us/azure/application-gateway/rewrite-http-headers-url).
    

## Learn more with self-paced training

- Complete an [introduction to Azure Application Gateway](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-application-gateway/).

## Learn more with optional hands-on exercises

- Load balance [HTTP(S) traffic in Azure](https://learn.microsoft.com/en-us/training/modules/load-balancing-https-traffic-azure/). _Azure subscription required_.
    
- Load balance your [web service traffic with Azure Application Gateway](https://learn.microsoft.com/en-us/training/modules/load-balance-web-traffic-with-application-gateway/). _Azure subscription required_.
    
- Encrypt [network traffic end-to-end with Azure Application Gateway](https://learn.microsoft.com/en-us/training/modules/end-to-end-encryption-with-app-gateway/). _Azure subscription required_.

---

# Design an IP addressing schema for your Azure deployment

# Introduction

Imagine you're the solution architect for a manufacturing company. Your company is beginning a project to move many services out of its existing datacenter and into the Azure cloud. The company wants to integrate the existing network with Azure. You need to plan the public and private IP addresses for the network carefully so you don’t run out of addresses and have capacity for future growth. A good IP addressing scheme provides flexibility, room for growth, and integration with on-premises networks.

In this module, you learn about the public and private IP addressing capabilities of Azure virtual networks. Also, you learn how to gather the necessary requirements for planning an IP address scheme. This module covers the on-premises integration methods of point-to-site and site-to-site, and also virtual network-to-virtual network peering. You also design and implement virtual networks and configure and verify virtual network peering. By the end of this module, you understand how to plan IP addressing for an Azure network and how to integrate Azure with an on-premises network.

## Learning objectives

In this module, you'll:

- Identify the private IP addressing capabilities of Azure virtual networks.
- Identify the public IP addressing capabilities of Azure.
- Identify the requirements for IP addressing when integrating with on-premises networks.

## Prerequisites

- Knowledge of basic networking concepts, network subnets, and IP addressing
- Familiarity with Azure virtual networking

# Network IP addressing and integration


To integrate resources in an Azure virtual network with resources in your on-premises network, you must understand how you can connect those resources and how to configure IP addresses.

Your manufacturing company wants to migrate a business-critical database to Azure. Client applications on desktop computers, laptops, and mobile devices need constant access to the database as if the database remained in the on-premises network. You want to move the database server without affecting users.

In this unit, you look at a typical on-premises network design and compare it to a typical Azure network design. You learn about requirements for IP addressing when integrating an Azure network with on-premises networks.

## On-premises IP addressing

A typical on-premises network design includes these components:

- Routers
- Firewalls
- Switches
- Network segmentation

![Diagram of a typical on-premises network design.](https://learn.microsoft.com/en-us/training/modules/design-ip-addressing-for-azure/media/2-on-premises-network.png)

The preceding diagram shows a simplified version of a typical on-premises network. On the routers facing the Internet Service Provider (ISP), you have public IP addresses that your outbound internet traffic uses as their source. These addresses also are used for inbound traffic across the internet. The internet service provider might issue you a block of IP addresses to assign to your devices. Or, you might have your own block of public IP addresses that your organization owns and controls. You can assign these addresses to systems that you would like to make accessible from the internet, such as web servers.

The perimeter network and internal zone have private IP addresses. In the perimeter network and internal zone, the IP addresses that are assigned to these devices aren't accessible over the internet. The administrator has full control over the IP address assignment, name resolution, security settings, and security rules. There are three ranges of nonroutable IP addresses that are designed for internal networks that won't be sent over internet routers:

- 10.0.0.0 to 10.255.255.255
- 172.16.0.0 to 172.31.255.255
- 192.168.0.0 to 192.168.255.255

The administrator can add or remove on-premises subnets to accommodate network devices and services. The number of subnets and IP addresses you can have in your on-premises network depends on the Classless Inter-Domain Routing (CIDR) for the IP address block.

## CIDR

Classless Inter-Domain Routing (CIDR) is a method for allocating IP addresses and routing Internet Protocol packets. CIDR allows for more efficient use of IP address space by enabling the creation of variable-length subnet masks (VLSMs), which can allocate IP addresses in a more granular and flexible manner. This method helps to reduce the wastage of IP addresses and improves the scalability of the network. CIDR notation represents an IP address followed by a slash and a number; 192.168.0.0/24. The number indicates the length of the subnet mask.

## Azure IP addressing

Azure virtual networks use private IP addresses. The ranges of private IP addresses are the same as for on-premises IP addressing. Like on-premises networks, the administrator has full control over the IP address assignment, name resolution, security settings, and security rules in an Azure virtual network. The administrator can add or remove subnets depending on the CIDR for the IP address block.

A typical Azure network design usually has these components:

- Virtual networks
- Subnets
- Network security groups
- Firewalls
- Load balancers

![Diagram of a typical Azure network design.](https://learn.microsoft.com/en-us/training/modules/design-ip-addressing-for-azure/media/2-azure-network.png)

In Azure, the network design has features and functions that are similar to an on-premises network, but the network's structure is different. The Azure network doesn't follow the typical on-premises hierarchical network design. The Azure network allows you to scale up and scale down infrastructure based on demand. Provisioning in the Azure network happens in a matter of seconds. There are no hardware devices like routers or switches. The entire infrastructure is virtual, and you can slice it into chunks that suit your requirements.

In Azure, you typically implement a network security group and a firewall. You use subnets to isolate front-end services, including web servers and Domain Name Systems (DNS), and back-end services like databases and storage systems. Network security groups filter internal and external traffic at the network layer. A firewall has more extensive capabilities for network-layer filtering and application-layer filtering. By deploying both network security groups and a firewall, you get improved isolation of resources for a secure network architecture.

## Basic properties of Azure virtual networks

A virtual network is your network in the cloud. You can divide your virtual network into multiple subnets. Each subnet contains a portion of the IP-address space assigned to your virtual network. You can add, remove, expand, or shrink a subnet if there are no VMs or services deployed in it.

By default, all subnets in an Azure virtual network can communicate with each other. However, you can use a network security group to deny communication between subnets. Regarding sizing, the smallest supported subnet uses a /29 subnet mask, and the largest supported subnet uses a /2 subnet mask. The smallest subnet has eight IP addresses, and the largest subnet has 1,073,741,824 IP addresses.

## Integrate Azure with on-premises networks

Before you start integrating Azure with on-premises networks, it's important to identify the current private IP address scheme the on-premises network uses. There can be no IP address overlap for interconnected networks.

For example, you can't use 192.168.0.0/16 on your on-premises network and use 192.168.10.0/24 on your Azure virtual network. These ranges both contain the same IP addresses so traffic can't be routed between them.

You can, however, have the same class range for multiple networks. For example, you can use the 10.10.0.0/16 address space for your on-premises network and the 10.20.0.0/16 address space for your Azure network because they don't overlap.

It's vital to check for overlaps when you're planning an IP address scheme. If there's an overlap of IP addresses, you can't integrate your on-premises network with your Azure network.

# Public and private IP addressing in Azure

You work for a manufacturing company, and you're moving resources into Azure. The database server must be accessible for clients in your on-premises network. Public resources—like web servers—must be accessible from the internet. You want to ensure that you plan IP addresses that support both these requirements.

In this unit, you explore the constraints and limitations for public and private IP addresses in Azure. Also, you look at the capabilities that are available in Azure to reassign IP addresses in your network.

## IP address types

In Azure, you can use two types of IP addresses:

- **Public IP addresses**
- **Private IP addresses**

You can allocate both types of IP addresses in one of two ways:

- **Dynamic**
- **Static**

Let's take a closer look at how the IP address types work together.

## Public IP addresses

Use a public IP address for public-facing services. A public address can be either static or dynamic. A public IP address can be assigned to a virtual machine (VM), an internet-facing load balancer, a VPN gateway, or an application gateway.

- **Dynamic public IP addresses** are assigned addresses that can change over the lifespan of the Azure resource. The dynamic IP address is allocated when you create or start a VM. The IP address is released when you stop or delete the VM. In each Azure region, public IP addresses are assigned from a unique pool of addresses. The default allocation method is dynamic.
    
- **Static public IP addresses** are assigned addresses that don't change over the lifespan of the Azure resource. To ensure that the IP address for the resource remains the same, you can set the allocation method to static. In this case, an IP address is assigned immediately, and is released only when you delete the resource or change the IP allocation method to dynamic.
    

### SKUs for public IP addresses

For public IP addresses, there are two SKUs from which to choose: **Basic** and **Standard**. All public IP addresses created before the introduction of SKUs are Basic SKU public IP addresses. With the introduction of SKUs, you can choose the scale, features, and pricing for load balancing internet traffic.

Both Basic and Standard SKUs have by default:

- An inbound originated flow idle timeout of four minutes, which is adjustable to up to 30 minutes.
- A fixed outbound originated flow idle timeout of four minutes.

#### Basic SKU

You can assign Basic public IPs by using static or dynamic allocation methods. You can assign Basic public IPs to any Azure resource that can be assigned a public IP address. Including network interfaces, VPN gateways, application gateways, and internet-facing load balancers.

By default, Basic SKU IP addresses:

- Are open. Network security groups are recommended, but optional, for restricting inbound or outbound traffic.
- Are available for inbound only traffic.
- Are available when using instance meta data service (IMDS).
- Don't support Availability Zones.
- Don't support routing preferences.

#### Standard SKU

By default, Standard SKU IP addresses:

- Always use static allocation.
- Are secure, and thus closed to inbound traffic. You must enable inbound traffic by using a network security group.
- Are zone-redundant and optionally zonal (they can be created as zonal and guaranteed in a specific availability zone).
- Can be assigned to network interfaces, Standard public load balancers, application gateways, or VPN gateways.
- Can be utilized with the routing preference to enable more granular control of how traffic is routed between Azure and the Internet.
- Can be used as anycast frontend IPs for cross-region load balancers.

For more information, see [SKU comparison](https://learn.microsoft.com/en-us/azure/load-balancer/skus), Load Balancer [overview](https://learn.microsoft.com/en-us/azure/load-balancer/load-balancer-overview), and [components](https://learn.microsoft.com/en-us/azure/load-balancer/components).

### Public IP address prefix

In Azure, a _public IP address prefix_ is a reserved, static range of public IP addresses. Azure assigns an IP address from a pool of available addresses that's unique to each region in each Azure cloud. When you define a Public IP address prefix, associated public IP addresses are assigned from a pool for an Azure region.

In a region with Availability Zones, Public IP address prefixes can be created as zone-redundant or associated with a specific availability zone.

The benefit of a public IP address prefix is that you can specify firewall rules for a known range of IP addresses. If your business needs to have datacenters in different regions, you need a different public IP address range for each region. You can assign the addresses from a public IP address prefix to any Azure resource that supports public IP addresses.

You can create a public IP address prefix by specifying a name and prefix size. The prefix size is the number of reserved addresses available for use.

- Public IP address prefixes consist of IPv4 or IPv6 addresses.
- You can use technology like Azure Traffic Manager to balance region-specific instances.
- You can only bring your own public IP addresses from on-premises networks into Azure by using a [Custom IP address prefix](https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/custom-ip-address-prefix).
- You can't specify addresses when you create a prefix; Azure assigns them. After a prefix is created, the IP addresses are fixed in a contiguous range.
- Public IP addresses can't be moved between regions; all IP addresses are region-specific.

## Private IP addresses

Private IP addresses are used for communication within an Azure Virtual Network, including virtual networks and your on-premises networks. You can set private IP addresses to dynamic (DHCP lease) or static (DHCP reservation).

**Dynamic private IP addresses** are assigned through a DHCP lease and can change over the lifespan of the Azure resource.

**Static private IP addresses** are assigned through a DHCP reservation and don't change throughout the lifespan of the Azure resource. Static private IP addresses persist if a resource is stopped or deallocated.

## IP addressing for Azure virtual networks

In Azure, a virtual network is a fundamental component that acts as an organization's network. The administrator has full control over IP address assignment, security settings, and security rules. When you create a virtual network, you define a scope of IP addresses. Private IP addressing works the same way as it does in an on-premises network. You choose the private IP addresses that the Internet Assigned Numbers Authority (IANA) reserves based on your network requirements:

- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

A subnet is a range of IP address within the virtual network. You can divide a virtual network into multiple subnets. Each subnet must have a unique address range, which is specified in classless interdomain routing (CIDR) format. CIDR is a way to represent a block of network IP addresses. An IPv4 CIDR, specified as part of the IP address, shows the length of the network prefix.

Consider, for example, CIDR 192.168.10.0/24. "192.168.10.0" is the network address, and "24" indicates that the first 24 bits are part of the network address, leaving the last 8 bits for specific host addresses. A subnet's address range can't overlap with other subnets in the virtual network or with the on-premises network.

For all subnets in Azure, the first three IP addresses are reserved by default. For protocol conformance, the first and last IP addresses of all subnets also are reserved. In Azure, an internal DHCP service assigns and maintains the lease of IP addresses. The `.1`, `.2`, `.3`, and last IP addresses aren't visible or configurable by the Azure customer. These addresses are reserved and used by internal Azure services.

In Azure virtual networks, IP addresses can be allocated to the following types of resources:

- Virtual machine network interfaces
- Load balancers
- Application gateways

# Plan IP addressing for your networks


In your manufacturing company, you asked the operations and engineering teams about their requirements for the number of virtual machines in Azure. Also, you asked them about their plans for expansion. Based on the results of this survey, you want to plan an IP addressing scheme that you won't have to change in the foreseeable future.

In this unit, you explore the requirements for a network IP address scheme. You learn about classless inter-domain routing (CIDR) and how you can use it to slice an IP block to meet your addressing needs. In the next unit, there's an exercise that shows how to plan IP addressing for Azure virtual networks.

## Gather your requirements

Before planning your network IP address scheme, you must gather the requirements for your infrastructure. These requirements also help you prepare for future growth by reserving extra IP addresses and subnets.

Here are two of the questions you might ask to discover the requirements:

- How many devices do you have on the network?
- How many devices are you planning to add to the network in the future?

When your network expands, you don't want to redesign the IP address scheme. Here are some other questions you could ask:

- Based on the services running on the infrastructure, what devices do you need to separate?
- How many subnets do you need?
- How many devices per subnet do you have?
- How many devices are you planning to add to the subnets in future?
- Are all subnets going to be the same size?
- How many subnets do you want or plan to add in future?

You need to isolate some services. Isolating services provides another layer of security, but also requires good planning. For example, public devices can access your front-end servers, but the back-end servers need to be isolated. Subnets help isolate the network in Azure. However, all subnets within a virtual network can communicate with each other in Azure by default. To provide further isolation, you can use a network security group. You might isolate services depending on the data and its security requirements. For example, you might choose to isolate HR data and the company's financial data from customer databases.

When you know the requirements, you have a greater understanding of the total number of devices on the network per subnet and how many subnets you need. CIDR allows more flexible allocation of IP addresses than was possible with the original system of IP address classes. Depending on your requirements, you determine the required subnets and hosts out of the block of IP Addresses.

Remember that Azure uses the first three addresses on each subnet. The subnets' first and last IP addresses also are reserved for protocol conformance. Therefore, the number of possible addresses on an Azure subnet is **(2^n)-5**, where **n** represents the number of host bits.

# Summary


In this module, you have:

- Identified the private and public IP addressing capabilities of Azure virtual networks.
- Identified how to integrate on-premises networks with Azure.
- Planned an IP address scheme for your Azure infrastructure and created virtual networks.

Now that you understand how to plan IP addressing for Azure networks, you understand the private and public IP addressing capabilities of Azure virtual networks. You can use this information to plan out the IP addressing for your own Azure infrastructure.

## Learn more

For more information about IP addressing in Azure, see the following articles:

- [Public IP addresses](https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-addresses)
- [Public IP address prefix](https://learn.microsoft.com/en-us/azure/virtual-network/public-ip-address-prefix)

---

# Distribute your services across Azure virtual networks and integrate them by using virtual network peering

# Introduction

Imagine you're the solution architect for an engineering company that has been migrating services into Azure. The company deployed services into separate virtual networks, but has yet to configure private connectivity between them.

Several business units identified services in these virtual networks that need to communicate with each other. You need to enable this connectivity, but you don't want to expose these services to the internet. You also want to keep the integration as simple as possible.

In this module, you learn about virtual network connection options and why virtual network peering is suited for this scenario. You create three virtual networks and configure virtual network peering between them. You then test your configuration to make sure it meets your connectivity goals.

## Learning objectives

In this module, you'll:

- Identify use cases for virtual network peering.
- Identify the features and limitations of virtual network peering.
- Configure peering connections between virtual networks.

# Connect services by using virtual network peering


You can use virtual network peering to directly connect Azure virtual networks together. When you use peering to connect virtual networks, virtual machines (VMs) in these networks can communicate with each other as if they're in the same network.

With peered virtual networks, traffic between virtual machines is routed through the Azure network. The traffic uses only private IP addresses. It doesn't rely on internet connectivity, gateways, or encrypted connections. The traffic is always private, and it takes advantage of the high bandwidth and low latency of the Azure backbone network.

![A basic diagram of two virtual networks that are connected by virtual network peering.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/2-vnet-peering.svg)

The two types of peering connections are created in the same way:

- **Virtual network peering** connects virtual networks in the same Azure region, such as two virtual networks in North Europe.
- **Global virtual network peering** connects virtual networks that are in different Azure regions, such as a virtual network in North Europe and a virtual network in West Europe.

Virtual network peering doesn't affect or disrupt any resources that are already deployed to your virtual networks. When you use virtual network peering, consider the key features defined in the following sections.

## Reciprocal connections

When you create a virtual network peering connection with Azure PowerShell or Azure CLI, only one side of the peering gets created. To complete the virtual network peering configuration, you need to configure the peering in the reverse direction to establish connectivity. When you create the virtual network peering connection through the Azure portal, the configuration for both sides is completed at the same time.

Think of how you connect two network switches together. You might connect a cable to each switch and maybe configure some settings so that the switches can communicate. Virtual network peering requires similar connections in each virtual network. Reciprocal connections provide this functionality.

## Cross-subscription virtual network peering

You can use virtual network peering even when both virtual networks are in different subscriptions. This setup might be necessary for mergers and acquisitions, or to connect virtual networks in subscriptions that different departments manage. Virtual networks can be in different subscriptions, and the subscriptions can use the same or different Microsoft Entra tenants.

When you use virtual network peering across subscriptions, you might find that an administrator of one subscription doesn't administer the peer network's subscription. The administrator might not be able to configure both ends of the connection. To peer the virtual networks when both subscriptions are in different Microsoft Entra tenants, the administrators of each subscription must grant the peer subscription's administrator the `Network Contributor` role on their virtual network.

## Transitivity

Virtual network peering is nontransitive. Only virtual networks that are directly peered can communicate with each other. Virtual networks can't communicate with peers of their peers.

Suppose, for example, that your three virtual networks (A, B, C) are peered like this: A <-> B <-> C. Resources in A can't communicate with resources in C because that traffic can't transit through virtual network B. If you need communication between virtual network A and virtual network C, you must explicitly peer these two virtual networks.

## Gateway transit

You can connect to your on-premises network from a peered virtual network if you enable gateway transit from a virtual network that has a VPN gateway. Using gateway transit, you can enable on-premises connectivity without deploying virtual network gateways to all your virtual networks. This method can reduce the overall cost and complexity of your network. By using virtual network peering with gateway transit, you can configure a single virtual network as a hub network. Connect this hub network to your on-premises datacenter and share its virtual network gateway with peers.

To enable gateway transit, configure the **Allow gateway transit** option in the hub virtual network where the gateway connection to your on-premises network is deployed. Also configure the **Use remote gateways** option in any spoke virtual networks.

 Note

If you want to enable the **Use remote gateways** option in a spoke network peering, you can't deploy a virtual network gateway in the spoke virtual network.

## Overlapping address spaces

IP address spaces of connected networks within Azure and between Azure and your on-premises network, can't overlap. This rule is also true for peered virtual networks. Keep this rule in mind when you're planning your network design. In any networks you connect through virtual network peering, VPN, or ExpressRoute, assign different address spaces that don't overlap.

![Diagram of a comparison of overlapping and nonoverlapping network addressing.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/2-non-overlapping-networks.svg)

## Alternative connectivity methods

Virtual network peering is the least complex way to connect virtual networks together. Other methods focus primarily on connectivity between on-premises and Azure networks rather than connections between virtual networks.

You can also connect virtual networks together through an ExpressRoute circuit. ExpressRoute is a dedicated, private connection between an on-premises datacenter and the Azure backbone network. The virtual networks that connect to an ExpressRoute circuit are part of the same routing domain and can communicate with each other. ExpressRoute connections don't go over the public internet, so your communications with Azure services are as secure as possible.

VPNs use the internet to connect your on-premises datacenter to the Azure backbone through an encrypted tunnel. You can use a site-to-site configuration to connect virtual networks together through VPN gateways. VPN gateways have higher latency than virtual network peering setups. They're more complex and can cost more to manage.

When virtual networks are connected through both a gateway and virtual network peering, traffic flows through the peering configuration.

## When to choose virtual network peering

Virtual network peering can be a great way to enable network connectivity between services that are in different virtual networks. Virtual network peering should be your first choice when you need to integrate Azure virtual networks. It's easy to implement and deploy, and it works well across regions and subscriptions.

Peering might not be your best option if you have [existing VPN or ExpressRoute](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview#service-chaining) connections or services behind Azure Basic Load Balancers that would be accessed from a peered virtual network. In these cases, you should research alternatives.


# Exercise - Prepare virtual networks for peering by using Azure CLI commands

Let's say your company is now ready to implement virtual network peering. You want to connect systems that are deployed in different virtual networks. To test this plan, you start by creating virtual networks to support the services your company is already running in Azure. You need three virtual networks:

- The **Sales** virtual network is deployed in **North Europe**. Sales systems use this virtual network to process the data added after a customer is engaged. The Sales team wants access to Marketing data.
- The **Marketing** virtual network is deployed in **North Europe**. Marketing systems use this virtual network. Members of the Marketing team regularly chat with the Sales team. To share their data with the Sales team, they must download it because the Sales and Marketing systems aren't connected.
- The **Research** virtual network is deployed in **West Europe**. Research systems use this virtual network. Members of the Research team have a logical working relationship with Marketing, but they don't want the Sales team to have direct access to their data.

![A diagram of virtual networks you need to create.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/3-prepare-vnets.svg)

You're going to create the following resources:

Expand table

|Virtual network|Region|Virtual network address space|Subnet|Subnet address space|
|---|---|---|---|---|
|SalesVNet|North Europe|10.1.0.0/16|Apps|10.1.1.0/24|
|MarketingVNet|North Europe|10.2.0.0/16|Apps|10.2.1.0/24|
|ResearchVNet|West Europe|10.3.0.0/16|Data|10.3.1.0/24|

## Create the virtual networks

1. In Cloud Shell, run the following command to create the virtual network and subnet for the **Sales** systems:
    
    Azure CLICopy
    
    ```
    az network vnet create \
        --resource-group "[sandbox resource group name]" \
        --name SalesVNet \
        --address-prefixes 10.1.0.0/16 \
        --subnet-name Apps \
        --subnet-prefixes 10.1.1.0/24 \
        --location northeurope
    ```
    
2. Run the following command to create the virtual network and subnet for the **Marketing** systems:
    
    Azure CLICopy
    
    ```
    az network vnet create \
        --resource-group "[sandbox resource group name]" \
        --name MarketingVNet \
        --address-prefixes 10.2.0.0/16 \
        --subnet-name Apps \
        --subnet-prefixes 10.2.1.0/24 \
        --location northeurope
    ```
    
3. Run the following command to create the virtual network and subnet for the **Research** systems:
    
    Azure CLICopy
    
    ```
    az network vnet create \
        --resource-group "[sandbox resource group name]" \
        --name ResearchVNet \
        --address-prefixes 10.3.0.0/16 \
        --subnet-name Data \
        --subnet-prefixes 10.3.1.0/24 \
        --location westeurope
    ```
    

## Confirm the virtual network configuration

Let's take a quick look at what you created.

1. View the virtual networks you've created by running the following command in Cloud Shell:
    
    Azure CLICopy
    
    ```
    az network vnet list --query "[?contains(provisioningState, 'Succeeded')]" --output table
    ```
    
2. Your output should look like this example:
    
    OutputCopy
    
    ```
    Location     Name           EnableDdosProtection    ProvisioningState    ResourceGuid                          ResourceGroup
    -----------  -------------  ----------------------  -------------------  ------------------------------------  ------------------------------------------
    westeurope   ResearchVNet   False                   Succeeded            9fe09fe0-d6cd-4043-aba8-b5e850a91251  learn-cb081b92-bc67-49cf-a965-1aeb40a2e25c
    northeurope  SalesVNet      False                   Succeeded            8f030706-cce4-4a7b-8da2-a9f738887ffd  learn-cb081b92-bc67-49cf-a965-1aeb40a2e25c
    northeurope  MarketingVNet  False                   Succeeded            ffbf8430-b0eb-4c3d-aa94-3b3156b90bed  learn-cb081b92-bc67-49cf-a965-1aeb40a2e25c
    ```
    

## Create virtual machines in each virtual network

Now, you deploy some Ubuntu virtual machines (VMs) in each of the virtual networks. These VMs simulate the services in each virtual network. In the final unit of this module, you use these VMs to test connectivity between the virtual networks.

1. In Cloud Shell, run the following command, replacing `<password>` with a password that meets the [requirements for Linux VMs](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/faq#what-are-the-password-requirements-when-creating-a-vm), to create an Ubuntu virtual machine (VM) in the **Apps** subnet of **SalesVNet**. Note this password for later use.
    
    Azure CLICopy
    
    ```
    az vm create \
        --resource-group "[sandbox resource group name]" \
        --no-wait \
        --name SalesVM \
        --location northeurope \
        --vnet-name SalesVNet \
        --subnet Apps \
        --image Ubuntu2204 \
        --admin-username azureuser \
        --admin-password <password>
    ```
    
     Note
    
    The `--no-wait` parameter in this command lets you continue working in Cloud Shell while the VM is building.
    
2. Run the following command, replacing `<password>` with a password that meets the [requirements for Linux VMs](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/faq#what-are-the-password-requirements-when-creating-a-vm), to create another Ubuntu VM in the **Apps** subnet of **MarketingVNet**. Note this password for later use. The VM might take a minute or two to be created.
    
    Azure CLICopy
    
    ```
    az vm create \
        --resource-group "[sandbox resource group name]" \
        --no-wait \
        --name MarketingVM \
        --location northeurope \
        --vnet-name MarketingVNet \
        --subnet Apps \
        --image Ubuntu2204 \
        --admin-username azureuser \
        --admin-password <password>
    ```
    
3. Run the following command, replacing `<password>` with a password that meets the [requirements for Linux VMs](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/faq#what-are-the-password-requirements-when-creating-a-vm), to create an Ubuntu VM in the **Data** subnet of **ResearchVNet**. Note this password for later use.
    
    Azure CLICopy
    
    ```
    az vm create \
        --resource-group "[sandbox resource group name]" \
        --no-wait \
        --name ResearchVM \
        --location westeurope \
        --vnet-name ResearchVNet \
        --subnet Data \
        --image Ubuntu2204 \
        --admin-username azureuser \
        --admin-password <password>
    ```
    
    The VMs might take several minutes to reach a running state.
    
4. To confirm that the VMs are running, run the following command. The Linux `watch` command is configured to refresh every five seconds.
    
    BashCopy
    
    ```
    watch -d -n 5 "az vm list \
        --resource-group "[sandbox resource group name]" \
        --show-details \
        --query '[*].{Name:name, ProvisioningState:provisioningState, PowerState:powerState}' \
        --output table"
    ```
    
    A **ProvisioningState** of **Succeeded** and a **PowerState** of **VM running** indicates a successful deployment for the VM.
    
5. When your VMs are running, you're ready to move on. Press `Ctrl-c` to stop the command and continue on with the exercise.
# Exercise - Configure virtual network peering connections by using Azure CLI commands

You created virtual networks and ran virtual machines (VMs) within them. However, the virtual networks have no connectivity, and none of these systems can communicate with each other.

To enable communication, you need to create peering connections for the virtual networks. To satisfy your company's requirements, you configure a hub and spoke topology and permit virtual network access when you create the peering connections.

## Create virtual network peering connections

Follow these steps to create connections between the virtual networks and to configure the behavior of each connection.

1. In Cloud Shell, run the following command to create the peering connection between the **SalesVNet** and **MarketingVNet** virtual networks. This command also permits virtual network access across this peering connection.
    
    Azure CLICopy
    
    ```
    az network vnet peering create \
        --name SalesVNet-To-MarketingVNet \
        --remote-vnet MarketingVNet \
        --resource-group "[sandbox resource group name]" \
        --vnet-name SalesVNet \
        --allow-vnet-access
    ```
    
2. Run the following command to create a reciprocal connection from **MarketingVNet** to **SalesVNet**. This step completes the connection between these virtual networks.
    
    Azure CLICopy
    
    ```
    az network vnet peering create \
        --name MarketingVNet-To-SalesVNet \
        --remote-vnet SalesVNet \
        --resource-group "[sandbox resource group name]" \
        --vnet-name MarketingVNet \
        --allow-vnet-access
    ```
    

Now that you have connections between Sales and Marketing, create connections between Marketing and Research.

1. In Cloud Shell, run the following command to create the peering connection between the **MarketingVNet** and **ResearchVNet** virtual networks:
    
    Azure CLICopy
    
    ```
    az network vnet peering create \
        --name MarketingVNet-To-ResearchVNet \
        --remote-vnet ResearchVNet \
        --resource-group "[sandbox resource group name]" \
        --vnet-name MarketingVNet \
        --allow-vnet-access
    ```
    
2. Run the following command to create the reciprocal connection between **ResearchVNet** and **MarketingVNet**:
    
    Azure CLICopy
    
    ```
    az network vnet peering create \
        --name ResearchVNet-To-MarketingVNet \
        --remote-vnet MarketingVNet \
        --resource-group "[sandbox resource group name]" \
        --vnet-name ResearchVNet \
        --allow-vnet-access
    ```
    

## Check the virtual network peering connections

Now that the peering connections between the virtual networks are created, make sure the connections work.

1. In Cloud Shell, run the following command to check the connection between **SalesVNet** and **MarketingVNet**:
    
    Azure CLICopy
    
    ```
    az network vnet peering list \
        --resource-group "[sandbox resource group name]" \
        --vnet-name SalesVNet \
        --query "[].{Name:name, Resource:resourceGroup, PeeringState:peeringState, AllowVnetAccess:allowVirtualNetworkAccess}"\
        --output table
    ```
    
2. You created only one connection from **SalesVNet**, so you get only one result. In the **PeeringState** column, make sure the status is **Connected**.
    
3. Run the following command to check the peering connection between the **ResearchVNet** and **MarketingVNet** virtual networks:
    
    Azure CLICopy
    
    ```
    az network vnet peering list \
        --resource-group "[sandbox resource group name]" \
        --vnet-name ResearchVNet \
        --query "[].{Name:name, Resource:resourceGroup, PeeringState:peeringState, AllowVnetAccess:allowVirtualNetworkAccess}"\
        --output table
    ```
    
4. Again, you created only one connection from **ResearchVNet**, so you get only one result. In the **PeeringState** column, make sure the status is **Connected**.
    
5. Run the following command to check the peering connections for the **MarketingVNet** virtual network.
    
    Azure CLICopy
    
    ```
    az network vnet peering list \
        --resource-group "[sandbox resource group name]" \
        --vnet-name MarketingVNet \
        --query "[].{Name:name, Resource:resourceGroup, PeeringState:peeringState, AllowVnetAccess:allowVirtualNetworkAccess}"\
        --output table
    ```
    
    Remember that you created connections from Marketing to Sales and from Marketing to Research, so you should get two connections. In the **PeeringState** column, make sure the status of both connections is **Connected**.
    

Your peering connections between the virtual networks should now look like this diagram:

![Diagram of the resulting virtual network peering connections.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/4-vnet-peering-configure-connections-result.svg)

## Check effective routes

You can further check the peering connection by looking at the routes that apply to the network interfaces of the VMs.

1. Run the following command to look at the routes that apply to the **SalesVM** network interface:
    
    Azure CLICopy
    
    ```
    az network nic show-effective-route-table \
        --resource-group "[sandbox resource group name]" \
        --name SalesVMVMNic \
        --output table
    ```
    
    The output table shows the effective routes for the virtual machine's network interface. For **SalesVMVMNic**, you should have a route to **10.2.0.0/16** with _Next Hop Type_ of **VNetPeering**. This network route is for the peering connection from **SalesVNet** to **MarketingVNet**.
    
    OutputCopy
    
    ```
    Source    State    Address Prefix    Next Hop Type    Next Hop IP
    --------  -------  ----------------  ---------------  -------------
    Default   Active   10.1.0.0/16       VnetLocal
    Default   Active   10.2.0.0/16       VNetPeering
    Default   Active   0.0.0.0/0         Internet
    Default   Active   10.0.0.0/8        None
    Default   Active   100.64.0.0/10     None
    Default   Active   192.168.0.0/16    None
    ```
    
2. Run the following command to look at the routes for **MarketingVM**:
    
    Azure CLICopy
    
    ```
    az network nic show-effective-route-table \
        --resource-group "[sandbox resource group name]" \
        --name MarketingVMVMNic \
        --output table
    ```
    
    The output table shows the effective routes for the virtual machine's network interface. For **MarketingVMVMNic**, you should have a route to **10.1.0.0/16** with a next hop type of **VNetPeering** and a route to **10.3.0.0/16** with a next hop type of **VNetGlobalPeering**. These network routes are for the peering connection from **MarketingVNet** to **SalesVNet** and from **MarketingVNet** to **ResearchVNet**.
    
    OutputCopy
    
    ```
    Source    State    Address Prefix    Next Hop Type      Next Hop IP
    --------  -------  ----------------  -----------------  -------------
    Default   Active   10.2.0.0/16       VnetLocal
    Default   Active   10.1.0.0/16       VNetPeering
    Default   Active   0.0.0.0/0         Internet
    Default   Active   10.0.0.0/8        None
    Default   Active   100.64.0.0/10     None
    Default   Active   192.168.0.0/16    None
    Default   Active   10.3.0.0/16       VNetGlobalPeering
    ```
    
3. Run the following command to look at the routes for **ResearchVM**:
    
    Azure CLICopy
    
    ```
    az network nic show-effective-route-table \
        --resource-group "[sandbox resource group name]" \
        --name ResearchVMVMNic \
        --output table
    ```
    
    The output table shows the effective routes for the virtual machine's network interface. For **ResearchVMVMNic**, you should have a route to **10.2.0.0/16** with a next hop type of **VNetGlobalPeering**. This network route is for the peering connection from **ResearchVNet** to **MarketingVNet**.
    
    OutputCopy
    
    ```
    Source    State    Address Prefix    Next Hop Type      Next Hop IP
    --------  -------  ----------------  -----------------  -------------
    Default   Active   10.3.0.0/16       VnetLocal
    Default   Active   0.0.0.0/0         Internet
    Default   Active   10.0.0.0/8        None
    Default   Active   100.64.0.0/10     None
    Default   Active   192.168.0.0/16    None
    Default   Active   10.2.0.0/16       VNetGlobalPeering
    ```
    

Now that your peering connections are configured, let's take a look at how these connections affect the communication between VMs.

# Exercise - Verify virtual network peering by using SSH between Azure virtual machines

In the previous unit, you configured peering connections between the virtual networks to enable resources to communicate with each other. Your configuration used a hub and spoke topology. **MarketingVNet** was the hub, and **SalesVNet** and **ResearchVNet** were spokes.

![Diagram of a hub and spoke topology for virtual networks.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/5-hub-spoke-network.svg)

Remember, peering connections are nontransitive. Intermediate virtual networks don't allow connectivity to flow through them to connected virtual networks. **SalesVNet** can communicate with **MarketingVNet**. **ResearchVNet** can communicate with **MarketingVNet**. **MarketingVNet** can communicate with both **SalesVNet** and **ResearchVNet**. The only communication that isn't permitted is between **SalesVNet** and **ResearchVNet**. Even though **SalesVNet** and **ResearchVNet** are both connected to **MarketingVNet**, they can't communicate with each other because they're not directly peered to each other.

Let's confirm the connectivity across the peering connections. First, you create a connection from Azure Cloud Shell to the _public_ IP address of a target virtual machine (VM). Then you connect from the target VM to the destination VM by using the destination VM's _private_ IP address.

 Important

To test the virtual network peering connection, connect to the private IP address assigned to each VM.

1. Connect to your VMs by using SSH (Secure Shell) directly from Cloud Shell. When using SSH, you first find the public IP addresses that are assigned to your test VMs.
    
2. Run the following command in Cloud Shell to list the IP addresses you use to connect to the VMs:
    
    Azure CLICopy
    
    ```
    az vm list \
        --resource-group "[sandbox resource group name]" \
        --query "[*].{Name:name, PrivateIP:privateIps, PublicIP:publicIps}" \
        --show-details \
        --output table
    ```
    
3. Record the output. You need the IP addresses for the exercises in this unit.
    

Before you start the tests, think about what you learned in this module. What results do you expect? Which VMs are able to communicate with each other, and which ones aren't?

## Test connections from SalesVM

In the first test, you use SSH in Cloud Shell to connect to the public IP address of **SalesVM**. You then attempt to connect from **SalesVM** to **MarketingVM** and **ResearchVM**.

1. In Cloud Shell, run the following command, using SSH to connect to the public IP address of **SalesVM**. In the command, replace `<SalesVM public IP>` with the VM's _public_ IP address.
    
    BashCopy
    
    ```
    ssh -o StrictHostKeyChecking=no azureuser@<SalesVM public IP>
    ```
    
    ![A diagram showing connection to the public IP address of SalesVM.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/5-sales-step-1.svg)
    
2. Sign in with the password that you used to create the VM. The prompt now shows that you're signed in to **SalesVM**.
    
3. In Cloud Shell, run the following command, using SSH to connect to the private IP address of **MarketingVM**. In the command, replace `<MarketingVM private IP>` with this VM's _private_ IP address.
    
    BashCopy
    
    ```
    ssh -o StrictHostKeyChecking=no azureuser@<MarketingVM private IP>
    ```
    
    ![Diagram showing connection from SalesVM to the private IP address of MarketingVM.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/5-sales-step-5.svg)
    
    The connection attempt should succeed because of the peering connection between the **SalesVNet** and **MarketingVNet** virtual networks.
    
4. Sign in by using the password you used to create the VM.
    
5. Enter `exit` to close this SSH session and return to the **SalesVM** prompt.
    
6. In Cloud Shell, run the following command, using SSH to connect to the private IP address of **ResearchVM**. In the command, replace `<ResearchVM private IP>` with this VM's _private_ IP address.
    
    BashCopy
    
    ```
    ssh -o StrictHostKeyChecking=no azureuser@<ResearchVM private IP>
    ```
    
7. The connection attempt should fail because there's no peering connection between the **SalesVNet** and **ResearchVNet** virtual networks. Up to 60 seconds might pass before the connection attempt times out. To force the attempt to stop, use Ctrl+C.
    
    ![Diagram showing the attempt failing to connect from SalesVM to the private IP address of ResearchVM.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/5-sales-step-9.svg)
    
8. Enter `exit` to close the SSH session and return to Cloud Shell.
    

## Test connections from ResearchVM

In the second test, you use SSH in Cloud Shell to connect to the public IP address of **ResearchVM**. Then, you attempt to connect from **ResearchVM** to **MarketingVM** and **SalesVM**.

1. In Cloud Shell, run the following command, using SSH to connect to the public IP address of **ResearchVM**. In the command, replace `<ResearchVM public IP>` with this VM's _public_ IP address.
    
    BashCopy
    
    ```
    ssh -o StrictHostKeyChecking=no azureuser@<ResearchVM public IP>
    ```
    
    ![Diagram showing connection to the public IP address of ResearchVM.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/5-research-step-1.svg)
    
2. Sign in by using the password that you used to create the VM. The prompt now shows that you're signed in to **ResearchVM**.
    
3. In Cloud Shell, run the following command, using SSH to connect to the private IP address of **MarketingVM**. In the command, replace `<MarketingVM private IP>` with this VM's _private_ IP address.
    
    BashCopy
    
    ```
    ssh -o StrictHostKeyChecking=no azureuser@<MarketingVM private IP>
    ```
    
    ![Diagram showing connection to the private IP address of MarketingVM.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/5-research-step-5.svg)
    
    The connection attempt should succeed because of the peering connection between the **ResearchVNet** and **MarketingVNet** virtual networks.
    
4. Sign in by using the password you used to create the VM.
    
5. Enter `exit` to close this SSH session and return to the **ResearchVM** prompt.
    
6. In Cloud Shell, run the following command, using SSH to connect to the private IP address of **SalesVM**. In the command, replace `<SalesVM private IP>` with this VM's _private_ IP address.
    
    BashCopy
    
    ```
    ssh -o StrictHostKeyChecking=no azureuser@<SalesVM private IP>
    ```
    
7. The connection attempt should fail because there's no peering connection between the **ResearchVNet** and **SalesVNet** virtual networks. Up to 60 seconds might pass before the connection attempt times out. To force the attempt to stop, use Ctrl+C.
    
    ![Diagram showing the attempt failing to connect ResearchVM to the private IP address of SalesVM.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/5-research-step-9.svg)
    
8. Enter `exit` to close the SSH session and return to Cloud Shell.
    

## Test connections from Marketing VM

In the final test, you use SSH in Cloud Shell to connect to the public IP address of **MarketingVM**. You then attempt to connect from **MarketingVM** to **ResearchVM** and **SalesVM**.

1. In Cloud Shell, run the following command, using SSH to connect to the public IP address of **MarketingVM**. In the command, replace `<MarketingVM public IP>` with this VM's _public_ IP address.
    
    BashCopy
    
    ```
    ssh -o StrictHostKeyChecking=no azureuser@<MarketingVM public IP>
    ```
    
    ![Diagram that shows connection to the public IP address of MarketingVM.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/5-marketing-step-1.svg)
    
2. Sign in by using the password that you used to create the VM. The prompt shows that you're signed in to **MarketingVM**.
    
3. In Cloud Shell, run the following command, using SSH to connect to the private IP address of **ResearchVM**. In the command, replace `<ResearchVM private IP>` with this VM's _private_ IP address.
    
    BashCopy
    
    ```
    ssh -o StrictHostKeyChecking=no azureuser@<ResearchVM private IP>
    ```
    
    ![Diagram that shows Azure Cloud Shell connecting to the MarketingVNet and the ResearchVNet virtual networks, using a peering connection.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/5-marketing-step-5.svg)
    
    The connection attempt should succeed because of the peering connection between the **MarketingVNet** and **ResearchVNet** virtual networks.
    
4. Sign in by using the password you used to create the VM.
    
5. Enter `exit` to close this SSH session, and return to the **MarketingVM** prompt.
    
6. In Cloud Shell, run the following command, using SSH to connect to the private IP address of **SalesVM**. In the command, replace `<SalesVM private IP>` with this VM's _private_ IP address.
    
    BashCopy
    
    ```
    ssh -o StrictHostKeyChecking=no azureuser@<SalesVM private IP>
    ```
    
    This connection attempt should also succeed because of the peering connection between the **MarketingVNet** and **SalesVNet** virtual networks.
    
    ![Diagram that shows Azure Cloud Shell connecting to the MarketingVNet and the SalesVNet virtual machines, using a peering connection.](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/media/5-marketing-step-9.svg)
    
7. Sign in by using the password you used to create the VM.
    
8. Enter `exit` to close this SSH session, and return to the **MarketingVM** prompt.
    
9. Enter `exit` to close the SSH session, and return to Cloud Shell.
    

This simple test using SSH demonstrates network connectivity between peered virtual networks. It also demonstrates lack of network connectivity for transitive connections.

If these servers were running application services, the server connectivity would allow communication between the services running on the VMs. The connectivity would allow the business to share data across departments as required.

# Summary

In this module, you learned how to use peering to connect virtual networks in a hub and spoke topology. You used VMs and SSH to verify connectivity between virtual networks. The peering connections enable communication for services that run on the VMs.

Now that you understand how to peer virtual networks together, you can use this secure and cost-effective method in your Azure network infrastructure. The method enables low-latency communication between resources in virtual networks. It supports scenarios where resources are in different regions or subscriptions. Virtual network peering should be your first choice when you need to connect virtual networks.

---
# Host your domain on Azure DNS

# Introduction

Azure DNS lets you host your Domain Name System (DNS) records for your domains on Azure infrastructure. With Azure DNS, you can use the same credentials, APIs, tools, and billing as your other Azure services.

Let's say that your company recently bought the custom domain name wideworldimporters.com from a third-party domain-name registrar. The domain name is for a new website that your organization plans to launch. You need a hosting service for DNS domains. This hosting service would resolve the wideworldimporters.com domain to your web server's IP address.

You're already using Azure to build your website, so you decide to use Azure DNS to manage your domain.

This module shows you how to configure Azure DNS to host your domain. You also see how to add an alias and other DNS records to resolve your domain name to a website.

## Learning objectives

In this module, you will:

- Configure Azure DNS to host your domain.

## Prerequisites

- Knowledge of networking concepts like name resolution and IP addresses.

# What is Azure DNS?

Azure DNS is a hosting service for Domain Name System (DNS) domains that provides name resolution by using Microsoft Azure infrastructure.

In this unit, you learn what DNS is and how it works. You also learn about Azure DNS and why you'd use it.

## What is DNS?

DNS, or the Domain Name System, is a protocol within the TCP/IP standard. DNS serves an essential role of translating the human-readable domain names—for example: `www.wideworldimports.com`—into a known IP address. IP addresses enable computers and network devices to identify and route requests among themselves.

DNS uses a global directory hosted on servers around the world. Microsoft is part of the network that provides a DNS service through Azure DNS.

A DNS server is also known as a DNS name server, or just a name server.

## How does DNS work?

A DNS server carries out one of two primary functions:

- Maintains a local cache of recently accessed or used domain names and their IP addresses. This cache provides a faster response to a local domain lookup request. If the DNS server can't find the requested domain, it passes the request to another DNS server. This process repeats at each DNS server until either a match is made or the search times out.
- Maintains the key-value pair database of IP addresses and any host or subdomain over which the DNS server has authority. This function is often associated with mail, web, and other internet domain services.

### DNS server assignment

In order for a computer, server, or other network-enabled device to access web-based resources, it must reference a DNS server.

When you connect by using your on-premises network, the DNS settings come from your server. When you connect by using an external location like a hotel, the DNS settings come from the internet service provider (ISP).

### Domain lookup requests

Here's a simplified overview of the process a DNS server uses when it resolves a domain-name lookup request:

- If the domain name is stored in the short-term cache, the DNS server resolves the domain request.
- If the domain isn't in the cache, it contacts one or more DNS servers on the web to see if they have a match. When a match is found, the DNS server updates the local cache and resolves the request.
- If the domain isn't found after a reasonable number of DNS checks, the DNS server responds with a _domain cannot be found_ error.

### IPv4 and IPv6

Every computer, server, or network-enabled device on your network has an IP address. An IP address is unique within your domain. There are two standards of IP address: IPv4 and IPv6.

- **IPv4** is composed of four sets of numbers, in the range 0 to 255, each separated by a dot; for example: 127.0.0.1. Today, IPv4 is the most commonly used standard. Yet, with the increase in IoT devices, the IPv4 standard will eventually be unable to keep up.
    
- **IPv6** is a relatively new standard and is intended to eventually replace IPv4. It consists of eight groups of hexadecimal numbers, each separated by a colon; for example: fe80:11a1:ac15:e9gf:e884:edb0:ddee:fea3.
    

Many network devices are now provisioned with both an IPv4 and an IPv6 address. The DNS name server can resolve domain names to both IPv4 and IPv6 addresses.

### DNS settings for your domain

Whether a third-party host your DNS server or you manage it in-house, you need to configure it for each host type you're using. Host types include web, email, or other services you're using.

As the administrator for your company, you want to set up a DNS server by using Azure DNS. In this instance, the DNS server acts as a start of authority (SOA) for your domain.

### DNS record types

Configuration information for your DNS server is stored as a file within a zone on your DNS server. Each file is called a record. The following record types are the most commonly created and used:

- **A** is the host record, and is the most common type of DNS record. It maps the domain or host name to the IP address.
- **CNAME** is a Canonical Name record that's used to create an alias from one domain name to another domain name. If you had different domain names that all accessed the same website, you'd use CNAME.
- **MX** is the mail exchange record. It maps mail requests to your mail server, whether hosted on-premises or in the cloud.
- **TXT** is the text record. It's used to associate text strings with a domain name. Azure and Microsoft 365 use TXT records to verify domain ownership.

Additionally, there are the following record types:

- Wildcards
- CAA (certificate authority)
- NS (name server)
- SOA (start of authority)
- SPF (sender policy framework)
- SRV (server locations)

The SOA and NS records are created automatically when you create a DNS zone by using Azure DNS.

### Record sets

Some record types support the concept of record sets, or resource record sets. A record set allows for multiple resources to be defined in a single record. For example, here's an A record that has one domain with two IP addresses:

Copy

```
www.wideworldimports.com.     3600    IN    A    127.0.0.1
www.wideworldimports.com.     3600    IN    A    127.0.0.2
```

SOA and CNAME records can't contain record sets.

## What is Azure DNS?

Azure DNS allows you to host and manage your domains by using a globally distributed name-server infrastructure. It allows you to manage all of your domains by using your existing Azure credentials.

Azure DNS acts as the SOA for the domain.

You can't use Azure DNS to register a domain name; you need to register it by using a third-party domain registrar.

## Why use Azure DNS to host your domain?

Azure DNS is built on the Azure Resource Manager service, which offers the following benefits:

- Improved security
- Ease of use
- Private DNS domains
- Alias record sets

At this time, Azure DNS doesn't support Domain Name System Security Extensions. If you require this security extension, you should host those portions of your domain with a third-party provider.

### Security features

Azure DNS provides the following security features:

- **Role-based access control**, which gives you fine-grained control over users' access to Azure resources. You can monitor their usage and control the resources and services to which they have access.
- **Activity logs**, which let you track changes to a resource and pinpoint where faults occurred.
- **Resource locking**, which gives you a greater level of control to restrict or remove access to resource groups, subscriptions, or any Azure resources.

### Ease of use

Azure DNS can manage DNS records for your Azure services and provide DNS for your external resources. Azure DNS uses the same Azure credentials, support contract, and billing as your other Azure services.

You can manage your domains and records by using the Azure portal, Azure PowerShell cmdlets, or the Azure CLI. Applications that require automated DNS management can integrate with the service by using the REST API and software development kit (SDKs).

### Private domains

Azure DNS handles translating external domain names to IP addresses. Azure DNS lets you create private zones. These zones provide name resolution for virtual machines (VMs) within a virtual network and between virtual networks without having to create a custom DNS solution. Private zones allow you to use your own custom domain names rather than the Azure-provided names.

To publish a private DNS zone to your virtual network, you specify the list of virtual networks that are allowed to resolve records within the zone.

Private DNS zones have the following benefits:

- DNS zones are supported as part of the Azure infrastructure, so there's no need to invest in a DNS solution.
- All DNS record types are supported: A, CNAME, TXT, MX, SOA, AAAA, PTR, and SRV.
- Host names for VMs in your virtual network are automatically maintained.
- Split-horizon DNS support allows the same domain name to exist in both private and public zones. It resolves to the correct one based on the originating request location.

### Alias record sets

Alias records sets can point to an Azure resource. For example, you can set up an alias record to direct traffic to an Azure public IP address, an Azure Traffic Manager profile, or an Azure Content Delivery Network endpoint.

The alias record set is supported in the following DNS record types:

- A
- AAAA
- CNAME

# Configure Azure DNS to host your domain

The new company website is in final testing. You're working on the plan to deploy the wideworldimports.com domain by using Azure DNS. You need to understand what steps are involved.

In this unit, you learn how to:

- Create and configure a DNS zone for your domain by using Azure DNS.
- Understand how to link your domain to an Azure DNS zone.
- Create and configure a private DNS zone.

## Configure a public DNS zone

You use a DNS zone to host the DNS records for a domain, such as wideworldimports.com.

### Step 1: Create a DNS zone in Azure

You used a third-party domain-name registrar to register the wideworldimports.com domain. The domain doesn't point to your organization's website yet.

To host the domain name with Azure DNS, you first need to create a DNS zone for that domain. A DNS zone holds all the DNS entries for your domain.

When creating a DNS zone, you need to supply the following details:

- **Subscription**: The subscription to be used.
    
- **Resource group**: The name of the resource group to hold your domains. If one doesn't exist, create one to allow for better control and management.
    
- **Name**: Your domain name, which in this case is wideworldimports.com.
    
- **Resource group location**: The location defaults to the location of the resource group.
    
    ![Screenshot of Create DNS zone page.](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/3-create-dns-zone.png)
    

### Step 2: Get your Azure DNS name servers

After you create a DNS zone for the domain, you need to get the name server details from the name servers (NS) record. You use these details to update your domain registrar's information and point to the Azure DNS zone.

![Screenshot of the name server details on the DNS zone page.](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/3-name-server.png)

### Step 3: Update the domain registrar setting

As the domain owner, you need to sign in to the domain-management application provided by your domain registrar. In the management application, edit the NS record and change the NS details to match your Azure DNS name server details.

Changing the NS details is called _domain delegation_. When you delegate the domain, you must use all four name servers provided by Azure DNS.

### Step 4: Verify delegation of domain name services

The next step is to verify that the delegated domain now points to the Azure DNS zone you created for the domain. This process can take as few as 10 minutes, but might take longer.

To verify the success of the domain delegation, query the start of authority (SOA) record. The SOA record is automatically created when the Azure DNS zone is set up. You can verify the SOA record using a tool like nslookup.

The SOA record represents your domain and becomes the reference point when other DNS servers are searching for your domain on the internet.

To verify the delegation, use nslookup like this:

dosCopy

```
nslookup -type=SOA wideworldimports.com
```

### Step 5: Configure your custom DNS settings

The domain name is wideworldimports.com. When it's used in a browser, the domain resolves to your website. But what if you want to add in web servers or load balancers? These resources need to have their own custom settings in the DNS zone, either as an A record or a CNAME.

#### A record

Each A record requires the following details:

- **Name**: The name of the custom domain, for example _webserver1_.
- **Type**: In this instance, it's A.
- **TTL**: Represents the "time-to-live" as a whole unit, where 1 is one second. This value indicates how long the A record lives in a DNS cache before it expires.
- **IP address**: The IP address of the server to which this A record should resolve.

#### CNAME record

The CNAME is the canonical name, or the alias for an A record. Use CNAME when you have different domain names that all access the same website. For example, you might need a CNAME in the _wideworldimports_ zone if you want both www.wideworldimports.com and wideworldimports.com to resolve to the same IP address.

You'd create the CNAME record in the _wideworldimports_ zone with the following information:

- NAME: www
- TTL: 600 seconds
- Record type: CNAME

If you exposed a web function, you'd create a CNAME record that resolves to the Azure function.

## Configure a private DNS zone

Another type of DNS zone that you can configure and host in Azure is a private DNS zone. Private DNS zones aren't visible on the Internet, and don't require that you use a domain registrar. You can use private DNS zones to assign DNS names to virtual machines (VMs) in your Azure virtual networks.

### Step 1: Create a private DNS zone

In the Azure portal, search for _private DNS zones_. To create the private zone, you need enter a resource group and the name of the zone. For example, the name might be something like private.wideworldimports.com.

![Screenshot of the Create Private DNS zone page.](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/3-create-private-dns-zone.png)

### Step 2: Identify virtual networks

Let's assume that your organization already created your VMs and virtual networks in a production environment. Identify the virtual networks associated with VMs that need name-resolution support. To link the virtual networks to the private zone, you need the virtual network names.

### Step 3: Link your virtual network to a private DNS zone

To link the private DNS zone to a virtual network, you create a virtual network link. In the Azure portal, go to the private zone, and select **Virtual network links**.

![Screenshot of the Virtual Network Links page in a private DNS zone.](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/3-virtual-network-link-option.png)

Select **Add** to pick the virtual network you want to link to the private zone.

![Screenshot of Add virtual network link page.](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/3-add-virtual-network-link.png)

You add a virtual network link record for each virtual network that needs private name-resolution support.

In the next unit, you learn how to create a public DNS zone.


# Exercise - Create a DNS zone and an A record by using Azure DNS

In the previous unit, we described setting up and configuring the wideworldimports.com domain to point to your Azure hosting on Azure DNS.

In this unit, you'll:

- Set up an Azure DNS and create a public DNS zone.
- Create an A record.
- Verify that the A record resolves to an IP address.

## Create a DNS zone in Azure DNS

Before you can host the wideworldimports.com domain on your servers, you need to create a DNS zone. The DNS zone holds all the configuration records associated with your domain.

To create your DNS zone:

1. Sign in to the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com) with the account you used to activate the sandbox.
    
2. On the Azure **home** page, under **Azure services**, select **Create a resource**. The **Create a resource** pane appears.
    
3. In the _Search services and marketplace_ search box, search for and select **DNS zone** by Microsoft. The **DNS zone** pane appears.
    
4. Select **Create** > **DNS zone**.
    
    ![Screenshot of DNS zone, with Create highlighted.](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/4-dnszonecreate.png)
    
    The **Create DNS zone** pane appears.
    
5. On the **Basics** tab, enter the following values for each setting.
    

    
|Setting|Value|
|---|---|
|**Project details**||
|Subscription|Concierge subscription|
|Resource group|From the dropdown list, select [sandbox resource group]|
|**Instance details**||
|Name|The name needs to be unique in the sandbox. Use `wideworldimportsXXXX.com`, and replace the "Xs" with letters or numbers.|
    
    ![Screenshot of Create DNS zone page.](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/4-creatednszone.png)
    
6. Select **Review + create**.
    
7. After validation passes, select **Create**. It takes a few minutes to create the DNS zone.
    
8. When deployment is complete, select **Go to resource**. The **Overview** pane for your **DNS zone** appears.
    
9. Select **Record Sets** from the top menu bar.
    
    By default, the NS, and SOA record sets are automatically created and automatically deleted whenever a DNS zone is created or deleted. The NS record set defines the Azure DNS namespaces and contains the four Azure DNS records. You use all four records when you update the registrar.
    
    The SOA record represents your domain, and is used when other DNS servers are searching for your domain.
    
10. Make a note of the NS record values. You need them in the next section.
    

## Create a DNS record

Now that the DNS zone exists, you need to create the necessary records to support the domain.

The primary record set to create is the A record. The A record set is used to point traffic from a logical domain name to the hosting server's IP address. An A record set can have multiple records. In a record set, the domain name remains constant, while the IP addresses differ.

1. If you're not already on the **Record Sets** screen, then open the **DNS zone** pane for _wideworldimportsXXXX.com_. In the top menu bar, select **Record sets**.
    
2. On the **Record Sets** pane, select **+ Add** in the top menu bar.
    
3. Select **Add** at the top of the Record sets page.
    
    [![Screenshot of adding a record set.](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/4-add-a-record.png)](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/4-arecord.png#lightbox)
    
    The **Add record set** pane appears.
    
4. Enter the following values for each setting.
    
  
|Setting|Value|Description|
|---|---|---|
|Name|www|The host name that you want to resolve to an IP address.|
|Type|A|The **A** record is the most commonly used. If you're using IPv6, select the **AAAA** type.|
|Alias record set|No|This setting can only be applied to A, AAAA, and CNAME record types.|
|TTL|1|The time to live, which specifies the period of time each DNS server caches the resolution before being purged.|
|TTL unit|Hours|This value can be seconds, minutes, hours, days, or weeks. Here, you're selecting hours.|
|IP Address|10.10.10.10|The IP address the record name resolves to. In a real-world scenario, you'd enter the public IP address for your web server.|
    
5. Select **Add** to add the record to your zone.
    
    [![Screenshot of A record set.](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/4-arecord.png)](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/4-arecord.png#lightbox)
    

It's possible to have more than one IP address set up for your web server. In that case, you add all the associated IP addresses as records in the A record set. After the record set is created, you can update it with more IP addresses.

## Verify your global Azure DNS

In a real-world scenario, after you create the public DNS zone, you update the NS records of the domain-name registrar to delegate the domain to Azure.

Even though we don't have a registered domain, it's still possible to verify that the DNS zone works as expected by using the `nslookup` tool.

### Use nslookup to verify the configuration

Here's how to use `nslookup` to verify the DNS zone configuration.

1. Use Cloud Shell to run the following command. Replace the DNS zone name with the zone you created, and replace `<name server address>` with one of the NS values you copied after you created the DNS zone.
    
    BashCopy
    
    ```
    nslookup www.wideworldimportsXXXX.com <name server address>
    ```
    
    The command should look something like the following example:
    
    BashCopy
    
    ```
    nslookup www.wideworldimportsXXXX.com ns1-04.azure-dns.com
    ```
    
2. You should see that your host name `www.wideworldimportsXXXX.com` resolves to 10.10.10.10.
    
    ![Screenshot of Cloud Shell, showing the nslookup results.](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/4-nslookup.png)
    

Congratulations! You successfully set up a DNS zone and created an A record.

# Dynamically resolve resource name by using alias record

In the previous exercise, you successfully delegated the domain from the domain registrar to your Azure DNS and configured an A record to link the domain to your web server.

The next phase of the deployment is to improve resiliency by using a load balancer. Load balancers distribute inbound data requests and traffic across one or more servers. They reduce the load on any one server and improve performance. This technology is well established. You can use it throughout your on-premises network.

You know that the A record and CNAME record don't support direct connection to Azure resources like your load balancers. You're tasked with finding out how to link the apex domain with a load balancer.

## What is an apex domain?

The apex domain is your domain's highest level. In our case, that's wideworldimports.com. The apex domain is also sometimes referred to as the _zone apex_ or _root apex_. The **@** symbol often represents the apex domain in your DNS zone records.

If you check the DNS zone for wideworldimports.com, you see that there are two apex domain records: NS and SOA. The NS and SOA records are automatically created when you created the DNS zone.

CNAME records that you might need for an Azure Traffic Manager profile or Azure Content Delivery Network endpoints aren't supported at the zone apex level. However, other _alias records_ are supported at the zone apex level.

## What are alias records?

Azure alias records enable a zone apex domain to reference other Azure resources from the DNS zone. You don't need to create complex redirection policies. You can also use an Azure alias to route all traffic through Traffic Manager.

The Azure alias record can point to the following Azure resources:

- A Traffic Manager profile
- Azure Content Delivery Network endpoints
- A public IP resource
- A front-door profile

Alias records provide lifecycle tracking of target resources, ensuring that changes to any target resource are automatically applied to the DNS zone. Alias records also provide support for load-balanced applications in the zone apex.

The alias record set supports the following DNS zone record types:

- **A**: The IPv4 domain name-mapping record.
- **AAAA**: The IPv6 domain name-mapping record.
- **CNAME**: The alias for your domain, which links to the A record.

## Uses for alias records

The following are some of the advantages of using alias records:

- **Prevents dangling DNS records**: A dangling DNS record occurs when the DNS zone records aren't up to date with changes to IP addresses. Alias records prevent dangling references by tightly coupling the lifecycle of a DNS record with an Azure resource.
- **Updates DNS record set automatically when IP addresses change**: When the underlying IP address of a resource, service, or application is changed, the alias record ensures that any associated DNS records are automatically refreshed.
- **Hosts load-balanced applications at the zone apex**: Alias records allow for zone apex resource routing to Traffic Manager.
- **Points zone apex to Azure Content Delivery Network endpoints**: With alias records, you can now directly reference your Azure Content Delivery Network instance.

An alias record allows you to link the zone apex (wideworldimports.com) to a load balancer. It creates a link to the Azure resource rather than a direct IP-based connection. So, if the IP address of your load balancer changes, the zone apex record continues to work.

# Exercise - Create alias records for Azure DNS

Your new website's deployment was a huge success. Usage volumes are higher than anticipated. The single web server on which the website runs is showing signs of strain. Your organization wants to increase the number of servers and distribute the load using a load balancer.

You now know you can use an Azure alias record to provide a dynamic, automatically refreshing link between the zone apex and the load balancer.

In this unit, you'll:

- Set up a virtual network with two VMs and a load balancer.
- Learn how to configure an Azure alias at the zone apex to direct to the load balancer.
- Verify that the domain name resolves to one or either of the VMs on your virtual network.

## Set up a virtual network, load balancer, and VMs in Azure

When you manually create a virtual network, load balancer, and two VMs, it takes some time. To reduce this time, you can use a Bash setup script that's available on GitHub. Follow these instructions to create a test environment for your alias record.

1. In Azure Cloud Shell, run the following setup script:
    
    BashCopy
    
    ```
    git clone https://github.com/MicrosoftDocs/mslearn-host-domain-azure-dns.git
    ```
    
2. To run the setup script, run the following commands:
    
    BashCopy
    
    ```
    cd mslearn-host-domain-azure-dns
    chmod +x setup.sh
    ./setup.sh
    ```
    
    The setup script takes a few minutes to run. The script:
    
    - Creates a network security group.
    - Creates two network interface controllers (NICs) and two VMs.
    - Creates a virtual network and assigns the VMs.
    - Creates a public IP address and updates the configuration of the VMs.
    - Creates a load balancer that references the VMs, including rules for the load balancer.
    - Links the NICs to the load balancer.
    
    After the script completes, it shows you the public IP address for the load balancer. Copy the IP address to use it later.
    

## Create an alias record in your zone apex

Now that you created a test environment, you're ready to set up the Azure alias record in your zone apex.

1. In the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com), select **Resource groups**. The **Resource groups** pane appears.
    
2. Select the resource group: [sandbox resource group]. The **Resource group** pane appears.
    
3. In the list of resources, select the DNS zone you created in the previous exercise, wideworldimportsXXXX.com. The **wideworldimportsXXXX.com DNS zone** pane appears.
    
4. In the menu bar, select **+ Record set**. The **Add record set** pane appears.
    
5. Enter the following values for each setting to create an alias record.
   
|Setting|Value|
|---|---|
|Name|Leave the name blank. Blank indicates the DNS zone for wideworldimportsXXXX.com.|
|Type|A. Even though we're creating an alias, the base record type must still be either A, AAAA, or CNAME.|
|Alias record set|Yes|
|Alias type|Azure resource|
|Azure resource|From the list of resources, select **myPublicIP**. It can take up to 15 minutes for the deployments to propagate. If this resource isn't listed, wait several minutes, refresh the portal, and try again.|

    ![Screenshot of Add record set.](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/6-aliasrecord-azurelb.png)
    
6. Select **OK** to add the record to your zone.
    

When the new alias record is created, it should look something like this:

![Screenshot of the DNS zone, with an alias record created.](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/6-aliasrecord04.png)

## Verify that the alias resolves to the load balancer

Now, you need to verify that the alias record is set up correctly. In a real-world scenario, you'd have an actual domain, and would complete the domain delegation to Azure DNS. You'd use the registered domain name for this exercise. Because this unit assumes there's no registered domain, you use the public IP address.

1. In the Azure portal, go to the resource group, select **myPublicIP**, then select the **Copy** icon next to the IP address.
    
    ![Screenshot of the DNS zone with an alias record created.](https://learn.microsoft.com/en-us/training/modules/host-domain-azure-dns/media/6-publicipaddress.png)
    
2. In a web browser, paste the Public IP address as the URL.
    
3. You see a basic web page that shows the name of the virtual machine (VM) to which the load balancer sent the request.

---

# Manage and control traffic flow in your Azure deployment with routes

# Introduction

A virtual network lets you implement a security perimeter around your resources in the cloud. You can control the information that flows in and out of a virtual network. You can also restrict access to allow only the traffic that originates from trusted sources.

Suppose that you're the solution architect for a retail organization. Also suppose that your organization recently suffered a security incident that exposed customer information such as names, addresses, and credit card numbers. Malicious actors infiltrated vulnerabilities in your retailer's network infrastructure, which resulted in the loss of customers' confidential information.

As part of a remediation plan, the security team recommends adding network protections in the form of network virtual appliances. The cloud infrastructure team must ensure traffic gets properly routed through the virtual appliances and gets inspected for malicious activity.

In this module, you'll learn about Azure routing, and you'll create custom routes to control the traffic flow. You'll also learn to redirect the traffic through the network virtual appliance so you can inspect the traffic before it's allowed through.

## Learning objectives

In this module, you'll:

- Identify the routing capabilities of an Azure virtual network.
- Configure routing within a virtual network.
- Deploy a basic network virtual appliance.
- Configure routing to send traffic through a network virtual appliance.

## Prerequisites

- Knowledge of basic networking concepts, including subnets and IP addressing
- Familiarity with Azure virtual networking

# Identify routing capabilities of an Azure virtual network

To control traffic flow within your virtual network, you must learn the purpose and benefits of custom routes. You must also learn how to configure the routes to direct traffic flow through a network virtual appliance (NVA).

## Azure routing

Network traffic in Azure is automatically routed across Azure subnets, virtual networks, and on-premises networks. System routes control this routing. They're assigned by default to each subnet in a virtual network. With these system routes, any Azure virtual machine that is deployed into a virtual network can communicate with any other in the network. These virtual machines are also potentially accessible from on-premises through a hybrid network or the internet.

You can't create or delete system routes, but you can override the system routes by adding custom routes to control traffic flow to the next hop.

Every subnet has the following default system routes:

| Address prefix                | Next hop type   |
| ----------------------------- | --------------- |
| Unique to the virtual network | Virtual network |
| 0.0.0.0/0                     | Internet        |
| 10.0.0.0/8                    | None            |
| 172.16.0.0/12                 | None            |
| 192.168.0.0/16                | None            |
| 100.64.0.0/10                 | None            |

The **Next hop type** column shows the network path taken by traffic sent to each address prefix. The path can be one of the following hop types:

- **Virtual network**: A route is created in the address prefix. The prefix represents each address range created at the virtual-network level. If multiple address ranges are specified, multiple routes are created for each address range.
- **Internet**: The default system route 0.0.0.0/0 routes any address range to the internet, unless you override Azure's default route with a custom route.
- **None**: Any traffic routed to this hop type is dropped and doesn't get routed outside the subnet. By default, the following IPv4 private-address prefixes are created: 10.0.0.0/8, 172.16.0.0/12, and 192.168.0.0/16. The prefix 100.64.0.0/10 for a shared address space is also added. None of these address ranges are globally routable.

The following diagram shows an overview of system routes and shows how traffic flows among subnets and the internet by default. You can see from the diagram that traffic flows freely among the two subnets and the internet.

![Diagram of traffic flowing among subnets and the internet.](https://learn.microsoft.com/en-us/training/modules/control-network-traffic-flow-with-routes/media/2-system-routes-subnets-internet.svg)

Within Azure, there are other system routes. Azure creates these routes if the following capabilities are enabled:

- Virtual network peering
- Service chaining
- Virtual network gateway
- Virtual network service endpoint

### Virtual network peering and service chaining

Virtual network peering and service chaining let virtual networks within Azure connect to one another. With this connection, virtual machines can communicate with each other within the same region or across regions. This communication in turn creates more routes within the default route table. Service chaining lets you override these routes by creating user-defined routes between peered networks.

The following diagram shows two virtual networks with peering configured. The user-defined routes are configured to route traffic through an NVA or an Azure VPN gateway.

![Diagram of virtual network peering with user-defined routes.](https://learn.microsoft.com/en-us/training/modules/control-network-traffic-flow-with-routes/media/2-virtual-network-peering-udrs.svg)

### Virtual network gateway

Use a virtual network gateway to send encrypted traffic between Azure and on-premises over the internet and to send encrypted traffic between Azure networks. A virtual network gateway contains routing tables and gateway services.

![Diagram of the structure of a virtual network gateway.](https://learn.microsoft.com/en-us/training/modules/control-network-traffic-flow-with-routes/media/2-virtual-network-gateway.svg)

### Virtual network service endpoint

Virtual network endpoints extend your private address space in Azure by providing a direct connection to your Azure resources. This connection restricts the flow of traffic: your Azure virtual machines can access your storage account directly from the private address space and deny access from a public virtual machine. As you enable service endpoints, Azure creates routes in the route table to direct this traffic.

## Custom routes

System routes might make it easy for you to quickly get your environment up and running. However, there are many scenarios in which you want to more closely control the traffic flow within your network. For example, you might want to route traffic through an NVA or through a firewall. This control is possible with custom routes.

You have two options for implementing custom routes: create a user-defined route, or use Border Gateway Protocol (BGP) to exchange routes between Azure and on-premises networks.

### User-defined routes

You can use a user-defined route to override the default system routes so traffic can be routed through firewalls or NVAs.

For example, you might have a network with two subnets and want to add a virtual machine in the perimeter network to be used as a firewall. You can create a user-defined route so that traffic passes through the firewall and doesn't go directly between the subnets.

When creating user-defined routes, you can specify these next hop types:

- **Virtual appliance**: A virtual appliance is typically a firewall device used to analyze or filter traffic that is entering or leaving your network. You can specify the private IP address of a Network Interface Card (NIC) attached to a virtual machine so that IP forwarding can be enabled. Or you can provide the private IP address of an internal load balancer.
- **Virtual network gateway**: Use to indicate when you want routes for a specific address to be routed to a virtual network gateway. The virtual network gateway is specified as a VPN for the next hop type.
- **Virtual network**: Use to override the default system route within a virtual network.
- **Internet**: Use to route traffic to a specified address prefix that is routed to the internet.
- **None**: Use to drop traffic sent to a specified address prefix.

With user-defined routes, you can't specify the next hop type **VirtualNetworkServiceEndpoint**, which indicates virtual network peering.

### Service tags for user-defined routes

You can specify a service tag as the address prefix for a user-defined route instead of an explicit IP range. A service tag represents a group of IP address prefixes from a given Azure service. Microsoft manages the address prefixes encompassed by the service tag and automatically updates the service tag as addresses change, thus minimizing the complexity of frequent updates to user-defined routes and reducing the number of routes you need to create.

### Border gateway protocol

A network gateway in your on-premises network can exchange routes with a virtual network gateway in Azure by using BGP. BGP is the standard routing protocol that's normally used to exchange routing information among two or more networks. BGP is used to transfer data and information between autonomous systems on the internet, such as different host gateways.

Typically, you use BGP to advertise on-premises routes to Azure when you're connected to an Azure datacenter through Azure ExpressRoute. You can also configure BGP if you connect to an Azure virtual network by using a VPN site-to-site connection.

The following diagram shows a topology with paths that can pass data between Azure VPN Gateway and on-premises networks:

![Diagram showing an example of using the Border Gateway Protocol.](https://learn.microsoft.com/en-us/training/modules/control-network-traffic-flow-with-routes/media/2-bgp.svg)

BGP offers network stability, because routers can quickly change connections to send packets if a connection path goes down.

## Route selection and priority

If multiple routes are available in a route table, Azure uses the route with the longest prefix match. For example, a message is sent to the IP address 10.0.0.2, but two routes are available with the 10.0.0.0/16 and 10.0.0.0/24 prefixes. Azure selects the route with the 10.0.0.0/24 prefix because it's more specific.

The longer the route prefix, the shorter the list of IP addresses available through that prefix. When you use longer prefixes, the routing algorithm can select the intended address more quickly.

You can't configure multiple user-defined routes with the same address prefix.

If there are multiple routes with the same address prefix, Azure selects the route based on the type in the following order of priority:

1. User-defined routes
2. BGP routes
3. System routes

# Exercise - Create custom routes

As you implement your security strategy, you want to control how network traffic is routed across your Azure infrastructure.

In the following exercise, you use a network virtual appliance (NVA) to help secure and monitor traffic. You want to ensure communication between front-end public servers and internal private servers is always routed through the appliance.

You configure the network so that all traffic flowing from a public subnet to a private subnet will be routed through the NVA. To make this flow happen, you create a custom route for the public subnet to route this traffic to a perimeter-network subnet. Later, you deploy an NVA to the perimeter-network subnet.

![Diagram of virtual network, subnets, and route table.](https://learn.microsoft.com/en-us/training/modules/control-network-traffic-flow-with-routes/media/3-virtual-network-subnets-route-table.svg)

In this exercise, you create the route table, custom route, and subnets. You'll then associate the route table with a subnet.

## Create a route table and custom route

The first task is to create a new routing table and then add a custom route for all traffic intended for the private subnet.

 Note

You might get an error that reads: _This command is implicitly deprecated_. Please ignore this error for this learning module. We're working on it!

1. In the Cloud Shell window on the right side of the screen, select the **More** icon (**...**), then select **Settings** > **Go to Classic version**.
    
2. In Azure Cloud Shell, run the following command to create a route table:
    
    Azure CLICopy
    
    ```
        az network route-table create \
            --name publictable \
            --resource-group "[sandbox resource group name]" \
            --disable-bgp-route-propagation false
    ```
    
3. Run the following command in Cloud Shell to create a custom route:
    
    Azure CLICopy
    
    ```
        az network route-table route create \
            --route-table-name publictable \
            --resource-group "[sandbox resource group name]" \
            --name productionsubnet \
            --address-prefix 10.0.1.0/24 \
            --next-hop-type VirtualAppliance \
            --next-hop-ip-address 10.0.2.4
    ```
    

## Create a virtual network and subnets

The next task is to create the **vnet** virtual network and the three subnets you need: **publicsubnet**, **privatesubnet**, and **dmzsubnet**.

1. Run the following command to create the **vnet** virtual network and the **publicsubnet** subnet:
    
    Azure CLICopy
    
    ```
        az network vnet create \
            --name vnet \
            --resource-group "[sandbox resource group name]" \
            --address-prefixes 10.0.0.0/16 \
            --subnet-name publicsubnet \
            --subnet-prefixes 10.0.0.0/24
    ```
    
2. Run the following command in Cloud Shell to create the **privatesubnet** subnet:
    
    Azure CLICopy
    
    ```
        az network vnet subnet create \
            --name privatesubnet \
            --vnet-name vnet \
            --resource-group "[sandbox resource group name]" \
            --address-prefixes 10.0.1.0/24
    ```
    
3. Run the following command to create the **dmzsubnet** subnet:
    
    Azure CLICopy
    
    ```
        az network vnet subnet create \
            --name dmzsubnet \
            --vnet-name vnet \
            --resource-group "[sandbox resource group name]" \
            --address-prefixes 10.0.2.0/24
    ```
    
4. You should now have three subnets. Run the following command to show all of the subnets in the **vnet** virtual network:
    
    Azure CLICopy
    
    ```
        az network vnet subnet list \
            --resource-group "[sandbox resource group name]" \
            --vnet-name vnet \
            --output table
    ```
    

## Associate the route table with the public subnet

The final task in this exercise is to associate the route table with the **publicsubnet** subnet.

Run the following command to associate the route table with the public subnet.

Azure CLICopy

```
    az network vnet subnet update \
        --name publicsubnet \
        --vnet-name vnet \
        --resource-group "[sandbox resource group name]" \
        --route-table publictable
```


# What is an NVA?

A network virtual appliance (NVA) is a virtual appliance that consists of various layers like:

- A firewall
- A WAN optimizer
- Application-delivery controllers
- Routers
- Load balancers
- IDS/IPS
- Proxies

You can deploy NVAs that you choose from providers in Azure Marketplace. Such providers include Cisco, Check Point, Barracuda, Sophos, WatchGuard, and SonicWall. You can use an NVA to filter traffic inbound to a virtual network, to block malicious requests, and to block requests made from unexpected resources.

In the retail-organization example scenario, you must work with the security and network teams. You want to implement a secure environment that scrutinizes all incoming traffic and blocks unauthorized traffic from passing on to the internal network. You also want to secure both virtual-machine networking and Azure-services networking as part of your company's network-security strategy.

Your goal is to prevent unwanted or unsecured network traffic from reaching key systems.

As part of the network-security strategy, you must control the flow of traffic within your virtual network. You also must learn the role of an NVA and the benefit of using an NVA to control traffic flow through an Azure network.

## Network virtual appliance

**Network virtual appliances (NVAs) are virtual machines that control the flow of network traffic by controlling routing**. You'll typically use them to manage traffic flowing from a perimeter-network environment to other networks or subnets.

![Diagram of a network architecture with a network virtual appliance.](https://learn.microsoft.com/en-us/training/modules/control-network-traffic-flow-with-routes/media/4-nva.svg)

You can deploy firewall appliances into a virtual network in different configurations. **You can put a firewall appliance in a perimeter-network subnet in the virtual network**, or if you want more control of security, implement a microsegmentation approach.

With the microsegmentation approach, you can create dedicated subnets for the firewall and then deploy web applications and other services in other subnets. All traffic is routed through the firewall and inspected by the NVAs. You'll enable forwarding on the virtual-appliance network interfaces to pass traffic that is accepted by the appropriate subnet.

Microsegmentation lets the firewall inspect all packets at OSI Layer 4 and, for application-aware appliances, Layer 7. When you deploy an NVA to Azure, it acts as a router that forwards requests between subnets on the virtual network.

Some NVAs require multiple network interfaces. One network interface is dedicated to the management network for the appliance. Additional network interfaces manage and control the traffic processing. After you’ve deployed the NVA, you can then configure the appliance to route the traffic through the proper interface.

### User-defined routes

For most environments, the default system routes already defined by Azure are enough to get the environments up and running. In certain cases, you should create a routing table and add custom routes. Examples include:

- Access to the internet via on-premises network using forced tunneling
- Using virtual appliances to control traffic flow

You can create multiple route tables in Azure. Each route table can be associated with one or more subnets. A subnet can only be associated with one route table.

## Network virtual appliances in a highly available architecture

If traffic is routed through an NVA, the NVA becomes a critical piece of your infrastructure. Any NVA failures directly affect the ability of your services to communicate. It's important to include a highly available architecture in your NVA deployment.

There are several methods of achieving high availability when using NVAs. At the end of this module, you can find more information about using NVAs in highly available scenarios.

# Exercise - Create an NVA and virtual machines



In the next stage of your security implementation, you'll deploy a network virtual appliance (NVA) to secure and monitor traffic between your front-end public servers and internal private servers.

First, configure the appliance to forward IP traffic. If IP forwarding isn't enabled, traffic that is routed through your appliance will never be received by its intended destination servers.

In this exercise, you deploy the **nva** network appliance to the **dmzsubnet** subnet. Then you enable IP forwarding so that traffic from **`*`** and traffic that uses the custom route is sent to the **privatesubnet** subnet.

![Diagram of a Network virtual appliance with IP forwarding enabled.](https://learn.microsoft.com/en-us/training/modules/control-network-traffic-flow-with-routes/media/5-nva-ip-forwarding.svg)

In the following steps, you'll deploy an NVA. You'll then update the Azure virtual NIC and the network settings within the appliance to enable IP forwarding.

## Deploy the network virtual appliance

To build the NVA, deploy an Ubuntu LTS instance.

1. In Cloud Shell, run the following command to deploy the appliance. Replace `<password>` with a suitable password of your choice for the **azureuser** admin account.
    
    Azure CLICopy
    
    ```
    az vm create \
        --resource-group "[sandbox resource group name]" \
        --name nva \
        --vnet-name vnet \
        --subnet dmzsubnet \
        --image Ubuntu2204 \
        --admin-username azureuser \
        --admin-password <password>
    ```
    

## Enable IP forwarding for the Azure network interface

In the next steps, you enable IP forwarding for the **nva** network appliance. When traffic flows to the NVA but is meant for another target, the NVA will route that traffic to its correct destination.

1. Run the following command to get the NVA network interface's ID:
    
    Azure CLICopy
    
    ```
    NICID=$(az vm nic list \
        --resource-group "[sandbox resource group name]" \
        --vm-name nva \
        --query "[].{id:id}" --output tsv)
    
    echo $NICID
    ```
    
2. Run the following command to get the NVA network interface's name:
    
    Azure CLICopy
    
    ```
    NICNAME=$(az vm nic show \
        --resource-group "[sandbox resource group name]" \
        --vm-name nva \
        --nic $NICID \
        --query "{name:name}" --output tsv)
    
    echo $NICNAME
    ```
    
3. Run the following command to enable IP forwarding for the network interface:
    
    Azure CLICopy
    
    ```
    az network nic update --name $NICNAME \
        --resource-group "[sandbox resource group name]" \
        --ip-forwarding true
    ```
    

## Enable IP forwarding in the appliance

1. Run the following command to save the NVA virtual machine's public IP address to the variable `NVAIP`:
    
    Azure CLICopy
    
    ```
    NVAIP="$(az vm list-ip-addresses \
        --resource-group "[sandbox resource group name]" \
        --name nva \
        --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
        --output tsv)"
    
    echo $NVAIP
    ```
    
2. Run the following command to enable IP forwarding within the NVA:
    
    BashCopy
    
    ```
    ssh -t -o StrictHostKeyChecking=no azureuser@$NVAIP 'sudo sysctl -w net.ipv4.ip_forward=1; exit;'
    ```
    
    When prompted, enter the password you used when you created the virtual machine.

# Exercise - Route traffic through the NVA


Now that you've created the network virtual appliance (NVA) and virtual machines (VMs), you'll route the traffic through the NVA.

![Visualization of virtual machines and IP addresses.](https://learn.microsoft.com/en-us/training/modules/control-network-traffic-flow-with-routes/media/6-vms-ip-addresses.svg)

## Create public and private virtual machines

The next steps deploy a VM into the public and private subnets.

1. Open the Cloud Shell editor and create a file named cloud-init.txt.
    
    BashCopy
    
    ```
    code cloud-init.txt
    ```
    
2. Add the following configuration information to the file. With this configuration, the `inetutils-traceroute` package is installed when you create a new VM. This package contains the `traceroute` utility that you'll use later in this exercise.
    
    TextCopy
    
    ```
    #cloud-config
    package_upgrade: true
    packages:
       - inetutils-traceroute
    ```
    
3. Press Ctrl+S to save the file, and then press Ctrl+Q to close the editor.
    
4. In Cloud Shell, run the following command to create the **public** VM. Replace `<password>` with a suitable password for the **azureuser** account.
    
    Azure CLICopy
    
    ```
    az vm create \
        --resource-group "[sandbox resource group name]" \
        --name public \
        --vnet-name vnet \
        --subnet publicsubnet \
        --image Ubuntu2204 \
        --admin-username azureuser \
        --no-wait \
        --custom-data cloud-init.txt \
        --admin-password <password>
    ```
    
5. Run the following command to create the **private** VM. Replace `<password>` with a suitable password.
    
    Azure CLICopy
    
    ```
    az vm create \
        --resource-group "[sandbox resource group name]" \
        --name private \
        --vnet-name vnet \
        --subnet privatesubnet \
        --image Ubuntu2204 \
        --admin-username azureuser \
        --no-wait \
        --custom-data cloud-init.txt \
        --admin-password <password>
    ```
    
6. Run the following Linux `watch` command to check that the VMs are running. The `watch` command periodically runs the `az vm list` command so that you can monitor the progress of the VMs.
    
    BashCopy
    
    ```
    watch -d -n 5 "az vm list \
        --resource-group "[sandbox resource group name]" \
        --show-details \
        --query '[*].{Name:name, ProvisioningState:provisioningState, PowerState:powerState}' \
        --output table"
    ```
    
    A **ProvisioningState** value of "Succeeded" and a **PowerState** value of "VM running" indicate a successful deployment. When all three VMs are running, you're ready to move on. Press Ctrl-C to stop the command and continue with the exercise.
    
7. Run the following command to save the public IP address of the **public** VM to a variable named `PUBLICIP`:
    
    Azure CLICopy
    
    ```
    PUBLICIP="$(az vm list-ip-addresses \
        --resource-group "[sandbox resource group name]" \
        --name public \
        --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
        --output tsv)"
    
    echo $PUBLICIP
    ```
    
8. Run the following command to save the public IP address of the **private** VM to a variable named `PRIVATEIP`:
    
    Azure CLICopy
    
    ```
    PRIVATEIP="$(az vm list-ip-addresses \
        --resource-group "[sandbox resource group name]" \
        --name private \
        --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
        --output tsv)"
    
    echo $PRIVATEIP
    ```
    

## Test traffic routing through the network virtual appliance

The final steps use the Linux `traceroute` utility to show how traffic is routed. You'll use the `ssh` command to run `traceroute` on each VM. The first test shows the route taken by ICMP packets sent from the **public** VM to the **private** VM. The second test shows the route taken by ICMP packets sent from the **private** VM to the **public** VM.

1. Run the following command to trace the route from **public** to **private**. When prompted, enter the password for the **azureuser** account that you specified earlier.
    
    BashCopy
    
    ```
    ssh -t -o StrictHostKeyChecking=no azureuser@$PUBLICIP 'traceroute private --type=icmp; exit'
    ```
    
    If you receive the error message `bash: traceroute: command not found`, wait a minute and retry the command. The automated installation of `traceroute` can take a minute or two after VM deployment. After the command succeeds, the output should look similar to the following example:
    
    TextCopy
    
    ```
    traceroute to private.kzffavtrkpeulburui2lgywxwg.gx.internal.cloudapp.net (10.0.1.4), 64 hops max
    1   10.0.2.4  0.710ms  0.410ms  0.536ms
    2   10.0.1.4  0.966ms  0.981ms  1.268ms
    Connection to 52.165.151.216 closed.
    ```
    
    Notice that the first hop is to 10.0.2.4. This address is the private IP address of **nva**. The second hop is to 10.0.1.4, the address of **private**. In the first exercise, you added this route to the route table and linked the table to the **publicsubnet** subnet. So now all traffic from **public** to **private** is routed through the NVA.
    
    ![Diagram of route from public to private.](https://learn.microsoft.com/en-us/training/modules/control-network-traffic-flow-with-routes/media/6-public-private-route.svg)
    
2. Run the following command to trace the route from **private** to **public**. When prompted, enter the password for the **azureuser** account.
    
    BashCopy
    
    ```
    ssh -t -o StrictHostKeyChecking=no azureuser@$PRIVATEIP 'traceroute public --type=icmp; exit'
    ```
    
    You should see the traffic go directly to **public** (10.0.0.4) and not through the NVA, as shown in the following command output.
    
    TextCopy
    
    ```
    traceroute to public.kzffavtrkpeulburui2lgywxwg.gx.internal.cloudapp.net (10.0.0.4), 64 hops max
    1   10.0.0.4  1.095ms  1.610ms  0.812ms
    Connection to 52.173.21.188 closed.
    ```
    
    The **private** VM is using default routes, and traffic is routed directly between the subnets.
    
    ![Diagram of route from private to public.](https://learn.microsoft.com/en-us/training/modules/control-network-traffic-flow-with-routes/media/6-private-public-route.svg)
    

You've now configured routing between subnets to direct traffic from the public internet through the **dmzsubnet** subnet before it reaches the private subnet. In the **dmzsubnet** subnet, you added a VM that acts as an NVA. You can configure this NVA to detect potentially malicious requests and block them before they reach their intended targets.

---

# Improve application scalability and resiliency by using Azure Load Balancer

# Introduction

Many apps need to be resilient to failure and scale easily when demand increases. You can address those needs by using Azure Load Balancer.

Suppose you work for a healthcare organization that's launching a new portal application with which patients can schedule appointments. The application has a patient portal, a web-application front end, and a business-tier database. The front end uses the database to retrieve and save patient information.

The new portal needs to be available around the clock to handle failures. The portal must adjust to load fluctuations by adding and removing resources to match the load. The organization needs a solution that distributes work to virtual machines across the system as virtual machines are added. The solution should detect failures and reroute jobs to virtual machines as needed. Improved resiliency and scalability help ensure that patients can schedule appointments from any location.

By the end of this module, you're able to use Azure Load Balancer to build a resilient and scalable app architecture.

## Learning objectives

In this module, you'll:

- Identify the features and capabilities of Azure Load Balancer.
- Deploy and configure an instance of Azure Load Balancer.

## Prerequisites

- Basic knowledge of networking concepts.
- Basic knowledge of Azure virtual machines.
- Familiarity with the Azure portal.

# Azure Load Balancer features and capabilities

With Azure Load Balancer, you can spread user requests across multiple virtual machines or other services. It allows you to scale the app to larger sizes than a single virtual machine can support, and ensures that users get service even when a virtual machine fails.

In your healthcare organization, you can expect large user demand. It's vitally important that each user can book an appointment, even during peak demand or when one or more virtual machines fail. By using multiple virtual servers for your front end with a load balancer to distribute traffic among them, you achieve a high capacity because all the virtual servers collaborate to satisfy requests. You also improve resilience because the load balancer can automatically reroute traffic when a virtual server fails.

Here, you learn how Load Balancer's features can help you create robust app architectures.

## Distribute traffic with Azure Load Balancer

Azure Load Balancer is a service you can use to distribute traffic across multiple virtual machines. Use Load Balancer to scale applications and create high availability for your virtual machines and services. Load balancers use a hash-based distribution algorithm. **By default, a five-tuple hash is used to map traffic to available servers.** The hash is made from the following elements:

- **Source IP**: The IP address of the requesting client.
- **Source port**: The port of the requesting client.
- **Destination IP**: The destination IP of the request.
- **Destination port**: The destination port of the request.
- **Protocol type**: The specified protocol type. Transmission Control Protocol (TCP) or User Datagram Protocol (UDP).

![Diagram showing an overview of Azure Load Balancer.](https://learn.microsoft.com/en-us/training/modules/improve-app-scalability-resiliency-with-load-balancer/media/2-load-balancer-distribution.svg)

Load Balancer supports inbound and outbound scenarios, provides low latency and high throughput, and scales up to millions of flows for TCP and UDP applications.

Load balancers aren't physical instances. Load-balancer objects are used to express how Azure configures its infrastructure to meet your requirements.

With Load Balancer, you can use availability sets and availability zones to ensure that virtual machines are always available:

Expand table

|Configuration|Service level agreement (SLA)|Information|
|---|---|---|
|**Availability set**|99.95%|Protection from hardware failures within datacenters|
|**Availability zone**|99.99%|Protection from entire datacenter failure|

### Availability sets

An availability set is a logical grouping used to isolate virtual machine resources from each other when they're deployed. Azure ensures that the virtual machines you put in an availability set run across multiple physical servers, compute racks, storage units, and network switches. If there's a hardware or software failure, only a subset of your virtual machines is affected. Your overall solution stays operational. Availability sets are essential for building reliable cloud solutions.

![Diagram showing an overview of availability sets in Azure.](https://learn.microsoft.com/en-us/training/modules/improve-app-scalability-resiliency-with-load-balancer/media/2-availability-sets.svg)

### Availability zones

An availability zone offers groups of one or more datacenters that have independent power, cooling, and networking. The virtual machines in an availability zone are placed in different physical locations within the same region. Use this architecture when you want to ensure that you can continue to serve users when an entire datacenter fails.

![Diagram showing an overview of availability zones in Azure.](https://learn.microsoft.com/en-us/training/modules/improve-app-scalability-resiliency-with-load-balancer/media/2-az-graphic-two.svg)

Availability zones don't support all virtual machine sizes and aren't available in all Azure regions. Check that they're supported in your region before you use them in your architecture.

## Select the right Load Balancer product

Two products are available when you create a load balancer in Azure: _basic_ load balancers and _standard_ load balancers.

Basic load balancers allow:

- Port forwarding
- Automatic reconfiguration
- Health probes
- Outbound connections through source network address translation (SNAT)
- Diagnostics through Azure Log Analytics for public-facing load balancers

You can only use basic load balancers with a single availability set or scale set.

Standard load balancers support all of the basic load balancer features. They also allow:

- HTTPS health probes
- Availability zones
- Diagnostics through Azure Monitor, for multidimensional metrics
- High availability (HA) ports
- Outbound rules
- A guaranteed SLA (99.99% for two or more virtual machines)

## Internal and external load balancers

An external load balancer operates by distributing client traffic across multiple virtual machines. An external load balancer permits traffic from the internet. The traffic might come from browsers, mobile apps, or other sources. In a healthcare organization, the balancer distributes the load of all the browsers that run the client healthcare application.

An internal load balancer distributes a load from internal Azure resources to other Azure resources. For example, if you have front-end web servers that need to call the business logic hosted on multiple middle-tier servers, you can distribute that load evenly by using an internal load balancer. No traffic is allowed from internet sources. In a healthcare organization, a load balancer distributes a load across the internal application tier.


# Configure a public load balancer

As the solution architect for the healthcare portal, you need to distribute the load from the client browsers over the virtual machines in your web farm. You need to set up a load balancer and configure the virtual machines to be balanced.

A public load balancer maps the public IP address and port number of incoming traffic to the private IP address and port number of a virtual machine in the back-end pool. The responses are then returned to the client. By applying load-balancing rules, you can distribute specific types of traffic across multiple virtual machines or services.

## Distribution modes

By default, Azure Load Balancer distributes network traffic equally among virtual machine instances. The following distribution modes are also possible if a different behavior is required:

- **Five-tuple hash**: The default distribution mode for Load Balancer is a five-tuple hash. The tuple is composed of source IP, source port, destination IP, destination port, and protocol type. Because the source port is included in the hash and the source port changes for each session, clients might be directed to a different virtual machine for each session.
    
    ![Diagram showing how hash-based distribution works.](https://learn.microsoft.com/en-us/training/modules/improve-app-scalability-resiliency-with-load-balancer/media/3-load-balancer-distribution.svg)
    
- **Source IP affinity**: This distribution mode is also known as _session affinity_ or _client IP affinity_. To map traffic to the available servers, the source IP affinity mode uses a two-tuple hash (from the source IP address and destination IP address) or a three-tuple hash (from the source IP address, destination IP address, and protocol type). The hash ensures that requests from a specific client are always sent to the same virtual machine behind the load balancer.
    
    ![Diagram showing how session affinity works.](https://learn.microsoft.com/en-us/training/modules/improve-app-scalability-resiliency-with-load-balancer/media/3-load-balancer-session-affinity.svg)
    

## Choose a distribution mode

In the healthcare-portal example, imagine that a developer requirement of the presentation tier is to use in-memory sessions to store the signed-in user's profile as the user interacts with the portal.

In this scenario, the load balancer must provide source IP affinity to maintain a user's session. The profile is stored only on the virtual machine to which the client first connects, because that IP address is directed to the same server. When you create the load-balancer endpoint, you must specify the distribution mode by using the following PowerShell example:

PowerShellCopy

```
$lb = Get-AzLoadBalancer -Name MyLb -ResourceGroupName MyResourceGroup
$lb.LoadBalancingRules[0].LoadDistribution = 'sourceIp'
Set-AzLoadBalancer -LoadBalancer $lb
```

To add session persistence through the Azure portal:

1. In the Azure portal, select your Load Balancer resource.
    
2. In the **Load balancing rules** page under the _Settings_ pane, select the relevant load balancing rule.
    
    ![Screenshot showing how to select a load balancing rule in the Azure portal.](https://learn.microsoft.com/en-us/training/modules/improve-app-scalability-resiliency-with-load-balancer/media/3-load-balancer-rules.png)
    
3. In the _load balancing rule settings_ page, change the value for **Session persistence** from **None** to **Client IP**.
    

![Screenshot showing how to set IP affinity in the Azure portal.](https://learn.microsoft.com/en-us/training/modules/improve-app-scalability-resiliency-with-load-balancer/media/3-screenshot-session-persistence.png)

## Load Balancer and Remote Desktop Gateway

Remote Desktop Gateway is a Windows service that enables clients on the internet to make Remote Desktop Protocol (RDP) connections through firewalls to Remote Desktop servers on your private network. The default five-tuple hash in Load Balancer is incompatible with this service. If you want to use Load Balancer with your Remote Desktop servers, use source IP affinity.

## Load Balancer and media upload

Another use case for source IP affinity is media upload. In many implementations, a client initiates a session through a Transmission Control Protocol (TCP) protocol and connects to a destination IP address. This connection remains open throughout the upload to monitor progress, but the file is uploaded through a separate User Datagram Protocol (UDP) protocol.

With the five-tuple hash, the load balancer likely sends the TCP and UDP connections to different destination IP addresses and the upload doesn't finish successfully. Use source IP affinity to resolve this issue.

# Exercise - Configure a public load balancer


You can configure Azure Load Balancer by using the Azure portal, PowerShell, or the Azure CLI.

In your healthcare organization, you want to load-balance client traffic to provide a consistent response based on the patient portal web servers' health. You have two virtual machines (VMs) in an availability set to act as your healthcare-portal web application.

Here, you create a load balancer resource and use it to distribute a load across the virtual machines.

## Deploy the patient portal web application

First, deploy your patient-portal application across two virtual machines in a single availability set. To save time, let's start by running a script to create this application. The script:

- Creates a virtual network and network infrastructure for the virtual machines.
- Creates two virtual machines in this virtual network.

To deploy the patient portal web application:

1. Run the following `git clone` command in Azure Cloud Shell. The command clones the repo that contains the source for the app and runs the setup script from GitHub. Then changes to the directory of the cloned repo.
    
    BashCopy
    
    ```
    git clone https://github.com/MicrosoftDocs/mslearn-improve-app-scalability-resiliency-with-load-balancer.git
    cd mslearn-improve-app-scalability-resiliency-with-load-balancer
    ```
    
2. As its name suggests, the script generates two virtual machines in a single availability set. It takes about two minutes to run.
    
    BashCopy
    
    ```
    bash create-high-availability-vm-with-sets.sh [sandbox resource group name]
    ```
    
3. When the script finishes, on the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com) menu or from the **Home** page, select **Resource groups**, then select the **[sandbox resource group name]** resource group. Review the resources created by the script.
    

## Create a load balancer

Let's use the Azure CLI to create the load balancer and its associated resources.

1. Create a new public IP address.
    
    Azure CLICopy
    
    ```
    az network public-ip create \
      --resource-group [sandbox resource group name] \
      --allocation-method Static \
      --name myPublicIP
    ```
    
2. Create the load balancer.
    
    Azure CLICopy
    
    ```
    az network lb create \
      --resource-group [sandbox resource group name] \
      --name myLoadBalancer \
      --public-ip-address myPublicIP \
      --frontend-ip-name myFrontEndPool \
      --backend-pool-name myBackEndPool
    ```
    
3. Create a health probe that allows the load balancer to monitor the healthcare portal's status. The health probe dynamically adds or removes virtual machines from the load-balancer rotation based on their response to health checks.
    
    Azure CLICopy
    
    ```
    az network lb probe create \
      --resource-group [sandbox resource group name] \
      --lb-name myLoadBalancer \
      --name myHealthProbe \
      --protocol tcp \
      --port 80  
    ```
    
4. Now, you need a load balancer rule to define how traffic is distributed to the virtual machines. You define the front-end IP configuration for the incoming traffic and the back-end IP pool to receive the traffic, along with the required source and destination port. To make sure only healthy virtual machines receive traffic, you also define the health probe to use.
    
    Azure CLICopy
    
    ```
    az network lb rule create \
      --resource-group [sandbox resource group name] \
      --lb-name myLoadBalancer \
      --name myHTTPRule \
      --protocol tcp \
      --frontend-port 80 \
      --backend-port 80 \
      --frontend-ip-name myFrontEndPool \
      --backend-pool-name myBackEndPool \
      --probe-name myHealthProbe
    ```
    
5. Connect the virtual machines to the back-end pool by updating the network interfaces that the script created to use the back-end pool information.
    
    Azure CLICopy
    
    ```
    az network nic ip-config update \
      --resource-group [sandbox resource group name] \
      --nic-name webNic1 \
      --name ipconfig1 \
      --lb-name myLoadBalancer \
      --lb-address-pools myBackEndPool
    
    az network nic ip-config update \
      --resource-group [sandbox resource group name] \
      --nic-name webNic2 \
      --name ipconfig1 \
      --lb-name myLoadBalancer \
      --lb-address-pools myBackEndPool
    ```
    
6. Run the following command to get the load balancer's public IP address and your website's URL:
    
    Azure CLICopy
    
    ```
    echo http://$(az network public-ip show \
                    --resource-group [sandbox resource group name] \
                    --name myPublicIP \
                    --query ipAddress \
                    --output tsv)
    ```
    

## Test the load balancer configuration

Let's test the load balancer setup, by showing how it can handle availability and health issues dynamically.

1. In a new browser tab, go to the public IP address that you noted. A response from one of the virtual machines is displayed in the browser.
    
2. Try a "force refresh" by pressing Ctrl+F5 a few times to see that the response is returned randomly from both virtual machines.
    
3. On the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com) menu or from the **Home** page, select **All resources**. Then select **webVM1**, and select **Stop**.
    
4. Return to the tab that shows the website and force a refresh of the webpage. All requests are returned from **webVM2**.

# Internal load balancer

In addition to balancing requests from users to front-end servers, you can use Azure Load Balancer to distribute traffic from front-end servers evenly among back-end servers.

In your healthcare organization, front-end servers call business logic services hosted on a middle tier. You want to ensure that the middle tier is as scalable and resilient as the front end. You want to use a load balancer to distribute requests from the front-end servers evenly among the middle-tier servers. This way, you can scale out the middle-tier servers to achieve the highest capacity possible. You also ensure that the middle tier is resilient to failure. When a server fails, the load balancer automatically reroutes traffic to another server.

Here, you learn how to use load balancers to distribute internal traffic.

## Configure an internal load balancer

In the healthcare-portal scenario, a web tier handles requests from users. The web tier connects to databases to retrieve data for users. The database tier is also deployed on two virtual machines. To allow the front-end web portal to continue to serve client requests if a database server fails, you can set up an internal load balancer to distribute traffic to the database servers.

You can configure an internal load balancer in almost the same way as an external load balancer, but with these differences:

- When you create the load balancer, select **Internal** for the **Type** value. When you select this setting, the load balancer's front-end IP address isn't exposed to the internet.
- Assign a private IP address instead of a public IP address for the load balancer's front end.
- Place the load balancer in the protected virtual network that contains the virtual machines you want to handle the requests.

The internal load balancer should be visible only to the web tier. All the virtual machines that host the databases are in one subnet. You can use an internal load balancer to distribute traffic to those virtual machines.

![Diagram showing internal load balancer.](https://learn.microsoft.com/en-us/training/modules/improve-app-scalability-resiliency-with-load-balancer/media/5-internal-load-balancer.svg)

## Choose the distribution mode

In the healthcare portal, the application tier is stateless, so you don't need to use source IP affinity. You can use the default distribution mode of a five-tuple hash. This mode offers the greatest scalability and resilience. The load balancer routes traffic to any healthy server.

