tipe:
![Screenshot that shows the main components of Azure network services.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-networks/media/network-components-66dff480.png)

karakter:
- An **Azure virtual network is a logical isolation** of the Azure cloud resources.
- You can use virtual networks to **provision and manage virtual private networks (VPNs)** in Azure.
- Each virtual network has its own **Classless Inter-Domain Routing (CIDR) block and can be linked** to other virtual networks and on-premises networks.
- You can **link virtual networks with an on-premises** IT infrastructure to create **hybrid or cross-premises solutions**, when the CIDR blocks of the connecting networks don't overlap.
- You control the **DNS server settings for virtual networks**, and segmentation of the virtual network into subnets.

![Diagram of a virtual network with a subnet of two virtual machines. The network connects to an on-premises infrastructure and separate virtual network.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-networks/media/virtual-networks-c016972b.png)

## Create subnets

- Each subnet **contains a range of IP addresses** that fall within the virtual network address space. 
- The **address range for a subnet must be unique** within the address space for the virtual network.
- The range for one **subnet can't overlap** with other subnet IP address ranges **in the same virtual network**.
- The IP address space for **a subnet must be specified by using CIDR** notation.
- You can **segment a virtual network into one or more subnets** in the Azure portal. Characteristics about the IP addresses for the subnets are listed.
  ![Screenshot that shows multiple subnets for a virtual network in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-networks/media/azure-subnets-a5c893d5.png)
  

| Reserved address                  | Reason                                                                |
| --------------------------------- | --------------------------------------------------------------------- |
| `192.168.1.0`                     | This value identifies the virtual network address.                    |
| `192.168.1.1`                     | Azure configures this address as the default gateway.                 |
| `192.168.1.2` _and_ `192.168.1.3` | Azure maps these Azure DNS IP addresses to the virtual network space. |
| `192.168.1.255`                   | This value supplies the virtual network broadcast address.            |


- **Consider service requirements**. Each service directly deployed into a virtual network has specific requirements for routing and the types of traffic that must be allowed into and out of associated subnets.
- **Consider network virtual appliances**. Azure routes network traffic between all subnets in a virtual network, by default. You can override Azure's default routing to prevent Azure routing between subnets. You can also override the default to route traffic between subnets through a network virtual appliance. If you require traffic between resources in the same virtual network to flow through a network virtual appliance, deploy the resources to different subnets.
- **Consider service endpoints**. You can limit access to Azure resources like an Azure storage account or Azure SQL database to specific subnets with a virtual network service endpoint. You can also deny access to the resources from the internet. You might create multiple subnets, and then enable a service endpoint for some subnets, but not others.
- **Consider network security groups**. You can associate zero or one network security group to each subnet in a virtual network. You can associate the same or a different network security group to each subnet. Each network security group contains rules that allow or deny traffic to and from sources and destinations.
- **Consider private links**. Azure Private Link provides private connectivity from a virtual network to Azure platform as a service (PaaS), customer-owned, or Microsoft partner services. Private Link simplifies the network architecture and secures the connection between endpoints in Azure. The service eliminates data exposure to the public internet.

## Plan IP addressing

**Private IP addresses** enable communication within an Azure virtual network and your on-premises network. You create a private IP address for your resource when you use a VPN gateway or Azure ExpressRoute circuit to extend your network to Azure.

**Public IP addresses** allow your resource to communicate with the internet. You can create a public IP address to connect with Azure public-facing services.

![Illustration of a resource with a private IP address and a public IP address.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-networks/media/ip-addressing-54476e47.png)

![Screenshot that shows how to create a public IP address in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-networks/media/create-public-ip-address-f07bd67d.png)

To create a public IP address, configure the following settings:
- **IP Version**: Select to create an **IPv4** or **IPv6** address, or **Both** addresses.
- **SKU**: Select the SKU for the public IP address, including **Basic** or **Standard**. The value must match the SKU of the Azure load balancer with which the address is used.
- **Name**: Enter a name to identify the IP address. The name must be unique within the resource group you select.
- **IP address assignment**: Identify the type of IP address assignment to use.
    - **Dynamic** addresses are assigned after a public IP address is associated to an Azure resource and is started for the first time. **Dynamic addresses can change if a resource such as a virtual machine is stopped (deallocated) and then restarted through Azure**. The address remains the same if a virtual machine is rebooted or stopped from within the guest OS. When a public IP address resource is removed from a resource, the dynamic address is released.
    - **Static** addresses are assigned when a public IP address is created. Static addresses aren't released until a public IP address resource is deleted. If the address isn't associated to a resource, you can change the assignment method after the address is created. If the address is associated to a resource, you might not be able to change the assignment method.

  

|Resource|Public IP address association|Dynamic IP address|Static IP address|
|---|---|---|---|
|Virtual machine|NIC|Yes|Yes|
|Load balancer|Front-end configuration|Yes|Yes|
|VPN gateway|VPN gateway IP configuration|Yes|Yes *****|
|Application gateway|Front-end configuration|Yes|Yes *****|

### Public IP address SKUs

| Feature       | Basic SKU                                                                                  | Standard SKU                                         |
| ------------- | ------------------------------------------------------------------------------------------ | ---------------------------------------------------- |
| IP assignment | Static or Dynamic                                                                          | Static                                               |
| Security      | Open by default                                                                            | Secure by default, closed to inbound traffic         |
| Resources     | Network interfaces, VPN gateways, Application gateways, and internet-facing load balancers | Network interfaces or public standard load balancers |
| Redundancy    | Not zone redundant                                                                         | Zone redundant by default                            |

### private IP addresses


  

| Resource               | Private IP address association | Dynamic IP address | Static IP address |
| ---------------------- | ------------------------------ | ------------------ | ----------------- |
| Virtual machine        | NIC                            | Yes                | Yes               |
| Internal load balancer | Front-end configuration        | Yes                | Yes               |
| Application gateway    | Front-end configuration        | Yes                | Yes               |

There are two options: dynamic and static.

- **Dynamic**: Azure assigns the next available unassigned or unreserved IP address in the subnet's address range. Dynamic assignment is the default allocation method.
    Suppose addresses 10.0.0.4 through 10.0.0.9 are already assigned to other resources. In this case, Azure assigns the address 10.0.0.10 to a new resource.
    
- **Static**: You select and assign any unassigned or unreserved IP address in the subnet's address range.
    Suppose a subnet's address range is 10.0.0.0/16, and addresses 10.0.0.4 through 10.0.0.9 are already assigned to other resources. In this scenario, you can assign any address between 10.0.0.10 and 10.0.255.254.

---

## network security groups

about network security groups

Let's look at the characteristics of network security groups.
- A network security group contains **a list of security rules that allow or deny inbound or outbound network traffic**.
- A network security group can be **associated to a subnet or a network interface**.
- A network security group can be **associated multiple times**.
- You create a network security group and **define security rules in the Azure portal**.
 
### network security group rules

#### inbound traffic rules

![Screenshot that shows default inbound security rules for a network security group in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/inbound-rules-a554314b.png)

#### outbound traffic rules

![Screenshot that shows default outbound security rules for a network security group in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/outbound-rules-ff90d802.png)

effective rules

![Diagram that shows how network security group security rules control traffic to virtual machines.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-security-groups/media/security-groups-7a9d5c84.png)

consideration:

- **Consider allowing all traffic**. If you place your virtual machine within a subnet or utilize a network interface, you don't have to associate the subnet or NIC with a network security group. allow all network traffic
- **Consider importance of allow rules**. When you create a network security group, you must define an **allow** rule for both the subnet and network interface in the group to ensure traffic can get through.
- **Consider intra-subnet traffic**. The security rules for a network security group that's associated to a subnet can affect traffic between all virtual machines in the subnet
- 