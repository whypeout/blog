---
draft: true
date: 2024-11-14
slug: Az 104 - Deploy and manage azure compute resources
tags:
---

# Configure virtual machines

# Introduction

Azure Virtual Machines enables you to create on-demand, scalable computing resources. Azure Architects commonly use virtual machines to gain greater control over the computing environment.

Your company is doing consumer research, and your team manages the on-premises servers. The servers you administer run the entire company infrastructure from web servers to databases. However, the hardware is aging and starting to struggle to keep up with some of the new data analysis applications being deployed. Rather than upgrade the hardware, the company decided to deploy Azure virtual machines. You're responsible for deploying the new virtual machines. Your deployment tasks include correctly sizing the machines, selecting storage, and configuring networking.

In this module, you learn how to configure virtual machine names and locations. You examine virtual machine pricing models and discover how to determine the correct virtual machine size. You learn how to configure virtual machine storage, and how to create a virtual machine in the Azure portal. You select a secure virtual machine connection method, and how to configure Windows and Linux virtual machine connections.

The goal of this module is to learn how to configure and manage virtual machines in Azure.

## Learning objectives

In this module, you learn how to:

- Determine the responsibilities of cloud service providers and customers in a cloud computing environment.
- Identify the key considerations and factors involved in planning for virtual machines. Considerations include workload requirements, resource allocation, and secure access.
- Configure virtual machine storage and virtual machine sizing.
- Create a virtual machine in the Azure portal.
- Practice deploying an Azure virtual machine and verify the configuration.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/credentials/certifications/exams/az-104/).

## Prerequisites

- Cloud computing concepts. Familiarity with Infrastructure as a Service (IaaS), virtualization, and resource provisioning in a cloud environment.
    
- Azure fundamentals. Understanding of basic Azure concepts, including Azure subscriptions, resource groups, and storage accounts.
    
- Networking fundamentals. Knowledge of basic networking concepts, including IP addressing, virtual networks, and subnets.
    
- Azure portal. Ability to create and configure resources in the Azure portal.

# Review cloud services responsibilities

The primary advantage of working with virtual machines is to have more control over installed software and configuration settings. Azure Virtual Machines supports more granular control than other Azure services, such as Azure App Service or Azure Cloud Services.

### Things to know about Azure Virtual Machines

Consider the following characteristics of Azure Virtual Machines.

- Azure Virtual Machines is the basis of the Azure infrastructure as a service (IaaS) model. IaaS is an instant computing infrastructure, provisioned and managed over the internet.
    
- A virtual machine provides its own operating system, storage, and networking capabilities, and can run a wide range of applications.
    
- You can implement multiple virtual machines, and configure each machine with different software and settings to support separate operation scenarios, such as development, testing, and deployment.
    
- You can use virtual machines to quickly scale up and down with demand and pay only for what you use.
    
- The responsibilities associated with configuring and maintaining virtual machines is shared between Microsoft and the customer. The following chart shows how the responsibilities are handled across the IaaS (virtual machines), PaaS, SaaS, and on-premises offerings.
    
    ![Diagram of the shared responsibility areas for IaaS, PaaS, SaaS, and on-premises offerings.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machines/media/responsibility-layers-4ffbf946.png)
    

### Things to consider when using IaaS and virtual machines

Let's look at some scenarios for working with IaaS and virtual machines. Think about how you can implement virtual machines in Azure.

- **Consider test and development**. Teams can quickly set up and dismantle test and development environments, bringing new applications to market faster. IaaS and virtual machines make it quick and economical to scale up dev-test environments up and down.
    
- **Consider website hosting**. Running websites by using IaaS and virtual machines can be less expensive than traditional web hosting.
    
- **Consider storage, backup, and recovery**. Virtual machines let organizations avoid the expense for storage and complexity of storage management. Recovery typically requires a skilled staff to manage data and meet legal and compliance requirements. IaaS is useful for handling unpredictable demand and steadily growing storage needs. You can simplify planning and management of backup and recovery systems.
    
- **Consider high-performance computing**. Virtual machines enable high-performance computing (HPC) on supercomputers, computer grids, or computer clusters. HPC helps solve complex problems involving millions of variables or calculations. You can support scenarios such as earthquake and protein folding simulations, climate and weather predictions, financial modeling, and evaluating product designs.
    
- **Consider big data analysis**. Big data is a popular term for massive data sets that contain potentially valuable patterns, trends, and associations. Mining data sets to locate or tease out these hidden patterns requires a huge amount of processing power, which IaaS economically provides.
    
- **Consider extended datacenters**. Add capacity to your datacenter by adding virtual machines in Azure. Avoid the costs of physically adding hardware or space to your physical location. Connect your physical network to the Azure cloud network seamlessly.

# Plan virtual machines

Before you create an Azure virtual machine, it's helpful to make a plan for the machine configuration. You need to consider your preferences for several options, including the machine size and location, storage usage, and associated costs.

### Things to know about configuring virtual machines

Let's walk through a checklist of things you need to consider when configuring a virtual machine.

- Start with the network.
- Choose a name for the virtual machine.
- Decide the location for the virtual machine.
- Determine the size of the virtual machine.
- Review the pricing model and estimate your costs.
- Identify which Azure Storage to use with the virtual machine.
- Select an operating system for the virtual machine.

#### Network configuration

Virtual networks are used in Azure to provide private connectivity between Azure Virtual Machines and other Azure services. Virtual machines and services that are part of the same virtual network can access one another. By default, services outside the virtual network can't connect to services within the virtual network. You can, however, configure the network to allow access to the external service, including your on-premises servers.

Network addresses and subnets aren't trivial to change after configuration. If you plan to connect your private company network to the Azure services, make sure you consider the topology before you put any virtual machines into place.

#### Virtual machine name

The virtual machine name is used as the computer name, which is configured as part of the operating system. You can specify a name with up to 15 characters on a Windows virtual machine and 64 characters on a Linux virtual machine.

The virtual machine name also defines a manageable Azure resource, and it's not trivial to change later. You should choose names that are meaningful and consistent, so you can easily identify what the virtual machine does. A good convention uses several of the following elements in the machine name:

Expand table

|Name element|Examples|Description|
|---|---|---|
|**Environment or purpose**|`dev` (development), `prod` (production), `QA` (testing)|A portion of the name should identify the environment or purpose for the machine.|
|**Location**|`uw` (US West), `je` (Japan East), `ne` (North Europe)|Another portion of the name should specify the region where the machine is deployed.|
|**Instance**|`1`, `02`, `005`|For multiple machines that have similar names, include an instance number in the name to differentiate the machines in the same category.|
|**Product or service**|`Outlook`, `SQL`, `AzureAD`|A portion of the name can specify the product, application, or service that the machine supports.|
|**Role**|`security`, `web`, `messaging`|A portion of the name can specify what role the machine supports within the organization.|

Let's consider how to name the first development web server for your company, hosted in the US South Central location. In this scenario, you might use the machine name `devusc-webvm01`. `dev` stands for development and `usc` identifies the location. `web` indicates the machine as a web server, and the suffix `01` shows the machine is the first in the configuration.

#### Virtual machine location

Azure has datacenters all over the world filled with servers and disks. These datacenters are grouped into geographic regions like West US, North Europe, Southeast Asia, and so on. The datacenters provide redundancy and availability.

Each virtual machine is in a region where you want the resources like CPU and storage to be allocated. The regional location lets you place your virtual machines as close as possible to your users. The location of the machine can improve performance and ensure you meet any legal, compliance, or tax requirements.

There are two other points to consider about the virtual machine location.

- The machine location can limit your available options. Each region has different hardware available, and some configurations aren't available in all regions.
    
- There are price differences between locations. To find the most cost-effective choice, check for your required configuration in different regions.
    

#### Virtual machine size

Azure offers different memory and storage options for different [virtual machine sizes](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes). The best way to determine the appropriate machine size is to consider the type of workload your machine needs to run. Based on the workload, you can choose from a subset of available virtual machine sizes.

#### Azure Storage

[Azure Managed Disks](https://learn.microsoft.com/en-us/azure/virtual-machines/managed-disks-overview) handle Azure storage account creation and management in the background for you. You specify the disk size and the performance tier (Standard or Premium). Azure creates and manages the disk. As you add disks or scale the virtual machine up and down, you don't have to worry about the storage being used.

#### Virtual machine pricing options

A subscription is billed two separate costs for every virtual machine: _compute_ and _storage_. By separating these costs, you can scale them independently and only pay for what you need.

- **Compute expenses** are priced on a per-hour basis but billed on a per-minute basis. If the virtual machine is deployed for 55 minutes, Azure charges you for only 55 minutes of usage. Azure doesn't charge for compute capacity if you stop and deallocate the virtual machine. The [hourly price](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) varies based on the virtual machine size and operating system you select. For the compute costs, you're able to choose from two payment options:
    
    - **Consumption-based**: With the consumption-based option, you pay for compute capacity by the second. You're able to increase or decrease compute capacity on demand and start or stop at any time. Use consumption-based pricing if you run applications with short-term or unpredictable workloads that can't be interrupted. An example scenario is if you're doing a quick test or developing an app in a virtual machine.
        
    - **Reserved Virtual Machine Instances**: The Reserved Virtual Machine Instances (RI) option is an advance purchase of a virtual machine for one or three years in a specified region. The commitment is made up front, and in return, you get up to 72% price savings compared to pay-as-you-go pricing. RIs are flexible and can easily be exchanged or returned for an early termination fee. Use this option if the virtual machine has to run continuously, or you need budget predictability, and you can commit to using the virtual machine for at least a year.
        
- **Storage costs** are charged separately for the Azure Storage used by the virtual machine. The status of the virtual machine has no relation to the Azure Storage charges that are incurred. Azure always charges you for any Azure Storage used by the disks.
    

#### Operating system

Azure provides various operating system images that you can install into the virtual machine, including several versions of Windows and flavors of Linux. Azure bundles the cost of the operating system license into the price.

- If you're looking for more than just base operating system images, you can search [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/compute). There are various install images that include not only the operating system but popular software tools, such as WordPress. The image stack consists of a Linux server, Apache web server, a MySQL database, and PHP. Instead of setting up and configuring each component, you can install an Azure Marketplace image and get the entire stack all at once.
    
- If you don't find a suitable operating system image, you can create your own disk image. Your disk image can be uploaded to Azure Storage and used to create an Azure virtual machine. Keep in mind that Azure only supports 64-bit operating systems.

# Determine virtual machine sizing

Rather than specify processing power, memory, and storage capacity independently, Azure provides different virtual machine sizes that offer variations of these elements in different size configurations. Azure provides a wide range of virtual machine size options that allow you to select the appropriate mix of compute, memory, and storage for your needs.

### Things to know about virtual machine sizes

The best way to determine the appropriate virtual machine size is to consider the type of workload your virtual machine needs to run. Based on the workload, you can choose from a subset of available virtual machine sizes.

The following table shows size classifications for Azure Virtual Machines workloads and recommended usage scenarios.

Expand table

|Classification|Description|Scenarios|
|---|---|---|
|**General purpose**|General-purpose virtual machines are designed to have a balanced CPU-to-memory ratio.|- Testing and development  <br>- Small to medium databases  <br>- Low to medium traffic web servers|
|**Compute optimized**|Compute optimized virtual machines are designed to have a high CPU-to-memory ratio.|- Medium traffic web servers  <br>- Network appliances  <br>- Batch processes  <br>- Application servers|
|**Memory optimized**|Memory optimized virtual machines are designed to have a high memory-to-CPU ratio.|- Relational database servers  <br>- Medium to large caches  <br>- In-memory analytics|
|**Storage optimized**|Storage optimized virtual machines are designed to have high disk throughput and I/O.|- Big Data  <br>- SQL and NoSQL databases  <br>- Data warehousing  <br>- Large transactional databases|
|**GPU**|GPU virtual machines are specialized virtual machines targeted for heavy graphics rendering and video editing. Available with single or multiple GPUs.|- Model training  <br>- Inferencing with deep learning|
|**High performance computes**|High performance compute offers the fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).|- Workloads that require fast performance  <br>- High traffic networks|

#### Resizing virtual machines

Azure allows you to change the virtual machine size when the existing size no longer meets your needs. You can resize a virtual machine if your current hardware configuration is allowed in the new size. This option provides a fully agile and elastic approach to virtual machine management.

When you stop and deallocate the virtual machine, you can select any size available in your region.

 Important

Be cautious when resizing production virtual machines. Resizing a machine might require a restart that can cause a temporary outage or change configuration settings such as the IP address.

# Determine virtual machine storage

Just like any other computer, virtual machines in Azure use disks as a place to store the operating system, applications, and data.

### Things to know about virtual machine storage and disks

All Azure virtual machines have at least two disks: an operating system disk and a temporary disk. Virtual machines can also have one or more data disks. All disks are stored as virtual hard disks (VHDs). A VHD is like a physical disk in an on-premises server but, virtualized.

![Diagram that shows disks used by an Azure virtual machine, including disks for the OS, data, and temporary storage.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machines/media/virtual-machine-disks-ff57089c.png)

#### Operating system disk

Every virtual machine has one attached operating system disk. The OS disk has a preinstalled operating system, which is selected when the virtual machine is created. The OS disk is registered as a SATA drive (Serial Advanced Technology Attachment) and labeled as the `C:` drive by default.

#### Temporary disk

Data on a temporary disk might be lost during a maintenance event or when you redeploy a virtual machine. During a standard reboot of the virtual machine, the data on the temporary drive should persist. However, there are cases where the data might not persist, such as moving to a new host. Therefore, any data on the temporary drive shouldn't be data that's critical to the system.

- On Windows virtual machines, the temporary disk is labeled as the `D:` drive by default. This drive is used for storing the **pagefile.sys** file.
- On Linux virtual machines, the temporary disk is typically `/dev/sdb`. Azure Linux Agent formats this disk and mounts it to `/mnt`.

 Important

Don't store data on the temporary disk. This disk provides temporary storage for applications and processes and is intended to only store data like page or swap files.

#### Data disks

A data disk is a managed disk attached to a virtual machine to store application data or other data you need to keep. Data disks are registered as SCSI drives and are labeled with a letter you choose. The size of a virtual machine determines how many data disks you can attach and the type of storage you can use to host the data disks.

### Things to consider when choosing storage for your virtual machines

Review the following considerations about using Azure Storage and Azure Managed Disks with your virtual machines.

- **Consider Azure Premium Storage**. You can choose Premium Storage to gain high-performance, low-latency disk support for your virtual machines with input/output (I/O)-intensive workloads. Virtual machine disks that use Premium Storage store data on solid-state drives (SSDs). To take advantage of the speed and performance of premium storage disks, you can migrate existing virtual machine disks to Premium Storage.
    
- **Consider multiple Storage disks**. In Azure, you can attach several Premium Storage disks to a virtual machine. Using multiple disks gives your applications up to 256 TB of storage per virtual machine. With Premium Storage, your applications can achieve 80,000 I/O operations per second (IOPS) per virtual machine, and a disk throughput of up to 2,000 megabytes per second (MB/s) per virtual machine. Read operations completed with Premium Storage yield low latencies.
    
- **Consider managed disks**. An Azure-managed disk is a VHD. Azure-managed disks are stored as page blobs, which are a random IO storage object in Azure. The disk is described as _managed_ because it's an abstraction over page blobs, blob containers, and Azure storage accounts. With managed disks, you provision the disk, and Azure takes care of the rest. When you choose to use Azure-managed disks with your workloads, Azure creates and manages the disk for you. The available types of disks are Ultra Solid State Drives (SSD), Premium SSD, Standard SSD, and Standard Hard Disk Drives (HDD).
    
     Note
    
    Managed disks are required for the single instance virtual machine SLA.
    
- **Consider migrating to Premium Storage**. For the best performance for your application, we recommend that you migrate any virtual machine disk that requires high IOPS to Premium Storage. If your disk doesn't require high IOPS, you can help limit costs by keeping it in standard Azure Storage.

# Create virtual machines in the Azure portal

When you create virtual machines in the Azure portal, one of your first decisions is to specify which image to use. Azure supports Windows and Linux operating systems, and there are server and client platforms to choose from. You can also search Azure Marketplace for other supported images:

![Screenshot that shows disk images for virtual machines in Azure Marketplace.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machines/media/server-versions-b93232ca.png)

## Configure virtual machine image

The Azure portal guides you through the configuration process to create your virtual machine image. The process includes configuring basic and advanced options, and specifying details about the disks, virtual networks, and machine management.

![Screenshot that shows the UI for creating a virtual machine in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machines/media/create-virtual-machines-a75605ba.png)

- The **Basics** tab contains the project details, administrator account, and inbound port rules.
    
- On the **Disks** tab, you select the OS disk type and specify your data disks.
    
- The **Networking** tab provides settings to create virtual networks and load balancing.
    
- On the **Management** tab, you can enable automatic shutdown and specify backup details.
    
- On the **Advanced** tab, you can configure agents, scripts, or virtual machine extensions.
    
- Other settings are available on the **Monitoring** and **Tags** tabs.
    

### Learn how to reduce costs when creating your virtual machine

![](https://youtu.be/n9QRCdCNtG0)


# Connect to virtual machines

There are several ways to access your Azure virtual machines. The Azure portal supports options for connecting your Windows and Linux machines, and making connections by using Azure Bastion. The following diagram shows how you can connect Azure virtual machines with the SSH and RDP protocols, Cloud Shell, and Azure Bastion.

![Diagram that shows virtual machine access with the SSH and RDP protocols, Cloud Shell, and Azure Bastion.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machines/media/bastion-connections-29c60c68.png)

### Things to know about connecting with Azure Bastion

The Azure Bastion service is a fully platform-managed PaaS service. Azure Bastion provides secure and seamless RDP/SSH connectivity to your virtual machines directly over SSL. When you connect via Azure Bastion, your virtual machines don't need a public IP address. The following example shows a virtual machine connection with Azure Bastion in the Azure portal.

![](https://youtu.be/epWKTGGa_wY)  

Azure Bastion provides secure RDP and SSH connectivity to all virtual machines in the virtual network. Azure Bastion protects your virtual machines from exposing RDP/SSH ports to the outside world while still providing secure access with RDP/SSH. Azure Bastion lets you connect to the virtual machine directly from the Azure portal. You aren't a client, agent, or another piece of software.

### Things to know about connecting Windows-based virtual machines

To connect to a Windows-based virtual machine hosted on Azure, use the Microsoft Remote Desktop application with the remote desktop protocol (RDP). The Remote Desktop app provides a graphical user interface (GUI) session to an Azure virtual machine that runs any supported version of Windows. The following image shows how to use the RDP protocol to connect to a Windows-based virtual machine in the Azure portal.

![Screenshot that shows how to use the RDP protocol to connect to a Windows-based virtual machine in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machines/media/rdp-connect.png)

To create an RDP connection, you specify the IP address for the virtual machine. As an option, you can select the port to use for the connection. The system provides you with a downloadable RDP file to use for the connection.

### Things to know about connecting Linux-based virtual machines

To connect to a Linux-based virtual machine, you can use a secure shell protocol (SSH) client. SSH is an encrypted connection protocol that allows secure sign-ins over unsecured connections. Depending on your organization's security policies, you can reuse a single public-private key pair to access multiple Azure virtual machines and services. You don't need a separate pair of keys for each virtual machine or service you wish to access. The following image shows how to use the SSH protocol to connect to a Linux-based virtual machine in the Azure portal.

![Screenshot that shows how to use the SSH protocol to connect to a Linux-based virtual machine in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machines/media/connect-ssh.png)

- The **public key** is placed on your Linux virtual machine, or any other service that you wish to use with public-key cryptography.
- The **private key** remains on your local system.

 Important

**Protect your private key**. Don't share your private key. Your public key can be shared with anyone, but only you (or your local security infrastructure) should possess your private key.

# Interactive lab simulation

## Lab scenario

Your organization is planning on using virtual machines in Azure. As the Azure Administrator you need to:

- Be able to use virtual machine Quickstart templates.
- Use templates to create and configure virtual machines.
- Be able to monitor virtual machine activity.

## Objectives

- **Task 1**: Use the Azure Quickstart Template gallery to deploy a virtual machine.
    - Browse to the [Azure Quickstart Template gallery](https://azure.microsoft.com/resources/templates/).
    - Search for a template that deploys a simple Windows Server virtual machine.
    - Edit the template and customize the parameters and variables.
    - Deploy the template to create the virtual machine.
- **Task 2**: Verify and monitor your virtual machine.
    - In the portal, locate your new virtual machine.
    - View monitoring data for CPU, network, and data usage.
    - View activity log information.

 Note

Click on the thumbnail image to start the lab simulation. When you're done, be sure to return to this page so you can continue learning.

[![Screenshot of the simulation page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machines/media/simulation-create-virtual-machine.png)](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209)

---

# Configure virtual machine availability

# Introduction

Managing virtual machines at scale can be challenging, especially when usage patterns vary and demands on applications fluctuate. Azure Administrators need to be able to adjust their virtual machine resources to match changing demands. At the same time, they need to keep their virtual machine configuration consistent to ensure application stability. Achieving these goals means maintaining throughput and responsiveness while minimizing the costs of continually running a large collection of virtual machines.

Your company website uses virtual machines and manages large workloads. The IT department wants to ensure the virtual machines can dynamically adjust to increases and decreases in workloads. They also want to ensure there's a business continuity plan to provide for highly available machines. You're responsible for deploying highly available virtual machines. You decide to use Azure Virtual Machine Scale Sets and the autoscale feature.

In this module, you learn about scaling virtual machines. You learn about availability zones, availability sets, update domains, and fault domains. You also learn about scale sets and autoscale.

The goal of this module is to learn how to successfully respond to changing virtual machine workloads.

## Learning objectives

In this module, you learn how to:

- Implement availability sets and availability zones.
- Implement update and fault domains.
- Implement Azure Virtual Machine Scale Sets.
- Autoscale virtual machines.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104).

## Prerequisites

- Familiarity with creating and managing Azure virtual machines.
- General knowledge of scaling infrastructure resources in fluctuating workloads.

# Plan for maintenance and downtime

Azure Administrators must be prepared for planned and unplanned failures. Let's explore three scenarios that can lead to your Azure virtual machine being impacted.

### Things to know about maintenance planning

An availability plan for Azure virtual machines needs to include strategies for unplanned hardware maintenance, unexpected downtime, and planned maintenance. As you review the following scenarios, think about how these scenarios can affect the example company website.

- An **unplanned hardware maintenance** event occurs when the Azure platform predicts that the hardware or any platform component associated to a physical machine is about to fail. When the platform predicts a failure, it issues an unplanned hardware maintenance event. Azure uses Live Migration technology to migrate your virtual machines from the failing hardware to a healthy physical machine. Live Migration is a virtual machine preserving operation that only pauses the virtual machine for a short time, but performance might be reduced before or after the event.
    
- **Unexpected downtime** occurs when the hardware or the physical infrastructure for your virtual machine fails unexpectedly. Unexpected downtime can include local network failures, local disk failures, or other rack level failures. When detected, the Azure platform automatically migrates (heals) your virtual machine to a healthy physical machine in the same datacenter. During the healing procedure, virtual machines experience downtime (reboot) and in some cases loss of the temporary drive.
    
- **Planned maintenance** events are periodic updates made by Microsoft to the underlying Azure platform to improve overall reliability, performance, and security of the platform infrastructure that your virtual machines run on. Most of these updates are performed without any impact to your virtual machines or Cloud Services.
    

 Note

Microsoft doesn't automatically update your virtual machine operating system or other software. You have complete control and responsibility for those updates. However, the underlying software host and hardware are periodically patched to ensure reliability and high performance.

# Create availability sets

An availability set is a logical feature you can use to ensure a group of related virtual machines are deployed together. The grouping helps to prevent a single point of failure from affecting all of your machines. The grouping ensures that not all of the machines are upgraded at the same time during a host operating system upgrade in the datacenter.

### Things to know about availability sets

Let's review some characteristics of availability sets.

- All virtual machines in an availability set should perform the identical set of functionalities.
    
- All virtual machines in an availability set should have the same software installed.
    
- Azure ensures that virtual machines in an availability set run across multiple physical servers, compute racks, storage units, and network switches.
    
    If a hardware or Azure software failure occurs, only a subset of the virtual machines in the availability set are affected. Your application stays up and continues to be available to your customers.
    
- You can create a virtual machine and an availability set at the same time.
    
    A virtual machine can only be added to an availability set when the virtual machine is created. To change the availability set for a virtual machine, you need to delete and then recreate the virtual machine.
    
- You can build availability sets by using the Azure portal, Azure Resource Manager (ARM) templates, scripting, or API tools.
    
- Microsoft provides robust Service Level Agreements (SLAs) for Azure virtual machines and availability sets. For details, see [SLA for Azure Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_9/).
    

 Note

Adding your virtual machines to an availability set won't protect your applications from operating system or application-specific failures. You'll need to explore other disaster recovery and backup techniques to provide application-level protection.

### Things to consider when using availability sets

Availability sets are an essential capability when you want to build reliable cloud solutions. In your planning for availability sets, keep the following general principles in mind:

- **Consider redundancy**. To achieve redundancy in your configuration, place multiple virtual machines in an availability set.
    
- **Consider separation of application tiers**. Each application tier exercised in your configuration should be located in a separate availability set. The separation helps to mitigate single point of failure on all machines.
    
- **Consider load balancing**. For high availability and network performance, create a load-balanced availability set by using Azure Load Balancer. Load Balancer distributes incoming traffic across working instances of services that are defined in your load-balanced availability set.
    
- **Consider managed disks**. You can use Azure managed disks with your Azure virtual machines in availability sets for block-level storage.

# Review update domains and fault domains

Azure Virtual Machine Availability Sets implements two node concepts to help Azure maintain high availability and fault tolerance when deploying and upgrading applications: _update domains_ and _fault domains_. Each virtual machine in an availability set is placed in one update domain and one fault domain.

### Things to know about update domains

An update domain is a group of nodes that are upgraded together during the process of a service upgrade (or _rollout_). An update domain allows Azure to perform incremental or rolling upgrades across a deployment. Here are some other characteristics of update domains.

- Each update domain contains a set of virtual machines and associated physical hardware that can be updated and rebooted at the same time.
    
- During planned maintenance, only one update domain is rebooted at a time.
    
- By default, there are five (non-user-configurable) update domains.
    
- You can configure up to 20 update domains.
    

### Things to know about fault domains

A fault domain is a group of nodes that represent a physical unit of failure. Think of a fault domain as nodes that belong to the same physical rack.

- A fault domain defines a group of virtual machines that share a common set of hardware (or _switches_) that share a single point of failure. An example is a server rack serviced by a set of power or networking switches.
    
- Two fault domains work together to mitigate against hardware failures, network outages, power interruptions, or software updates.
    

Let's look at a scenario with two fault domains that have two virtual machines each. The virtual machines in each fault domain are contained in different availability sets. The web availability set contains two virtual machines with one machine from each fault domain. The SQL availability set contains two different virtual machines with one from each fault domain.

![Illustration that shows two fault domains with two virtual machines each. The virtual machines in each fault domain are contained in different availability sets.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machine-availability/media/update-fault-domains-c1ceee00.png)

# Review availability zones

Availability zones are a high-availability offering that protects your applications and data from datacenter failures. An availability zone in an Azure region is a combination of a fault domain and an update domain.

Consider a scenario where you create three or more virtual machines across three zones in an Azure region. Your virtual machines are effectively distributed across three fault domains and three update domains. The Azure platform recognizes this distribution across update domains to make sure that virtual machines in different zones aren't updated at the same time.

You can use availability zones to build high-availability into your application architecture by colocating your compute, storage, networking, and data resources within a zone and replicating in other zones.

### Things to know about availability zones

Review the following characteristics of availability zones.

- Availability zones are unique physical locations within an Azure region.
    
- Each zone is made up of one or more datacenters that are equipped with independent power, cooling, and networking.
    
- To ensure resiliency, there's a minimum of three separate zones in all enabled regions.
    
- The physical separation of availability zones within a region protects applications and data from datacenter failures.
    
- Zone-redundant services replicate your applications and data across availability zones to protect against single-points-of-failure.
    

### Things to consider when using availability zones

Azure services that support availability zones are divided into two categories.

Expand table

|Category|Description|Examples|
|---|---|---|
|**Zonal services**|Azure _zonal_ services pin each resource to a specific zone.|- Azure Virtual Machines  <br>- Azure managed disks  <br>- Standard IP addresses|
|**Zone-redundant services**|For Azure services that are zone-redundant, the platform replicates automatically across all zones.|- Azure Storage that's zone-redundant  <br>- Azure SQL Database|

 Tip

To achieve comprehensive business continuity on Azure, build your application architecture by using a combination of availability zones with Azure region pairs.

# Compare vertical and horizontal scaling

A robust virtual machine configuration includes support for scalability. Scalability allows throughput for a virtual machine in proportion to the availability of the associated hardware resources. A scalable virtual machine can handle increases in requests without adversely affecting response time and throughput. For most scaling operations, there are two implementation options: _vertical_ and _horizontal_.

### Things to know about vertical scaling

Vertical scaling, also known as _scale up and scale down_, involves increasing or decreasing the virtual machine **size** in response to a workload. Vertical scaling makes a virtual machine more (scale up) or less (scale down) powerful.

![Illustration that shows vertical scaling where a single virtual machine increases or decreases in size by scaling up or scaling down.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machine-availability/media/vertical-scaling-cdafa792.png)

Here are some scenarios where using vertical scaling can be advantageous:

- If you have a service built on a virtual machine that's under-utilized such as on the weekend, you can use vertical scaling to decrease the virtual machine size and reduce your monthly costs.
    
- You can implement vertical scaling to increase your virtual machine size to support larger demand without having to create extra virtual machines.
    

### Things to know about horizontal scaling

Horizontal scaling is used to adjust the **number** of virtual machines in your configuration to support the changing workload. When you implement horizontal scaling, there's an increase (scale out) or decrease (scale in) in the number of virtual machine instances.

![Illustration that shows horizontal scaling where virtual machines are added to scale out the system to support the workload.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machine-availability/media/horizontal-scaling-3e457e75.png)

### Things to consider when using vertical and horizontal scaling

Review the following considerations regarding vertical and horizontal scaling. Think about which implementation might be required to support your company website.

- **Consider limitations**. Generally speaking, horizontal scaling has fewer limitations than vertical scaling. A vertical scaling implementation depends on the availability of larger hardware, which quickly hits an upper limit and can vary by region. Vertical scaling also usually requires a virtual machine to stop and restart, which can temporarily limit access to applications or data.
    
- **Consider flexibility**. When operating in the cloud, horizontal scaling is more flexible. A horizontal scaling implementation allows you to run potentially thousands of virtual machines to manage changes in workload and throughput.
    
- **Consider reprovisioning**. _Reprovisioning_ is the process of removing an existing virtual machine and replacing it with a new machine. A robust availability plan considers where reprovisioning might be required and plans for interruptions to service. If reprovisioning might be required, determine if any data needs to be maintained and migrated to the new machine.

# Implement Azure Virtual Machine Scale Sets

Azure Virtual Machine Scale Sets are an Azure Compute resource that you can use to deploy and manage a set of **identical** virtual machines. When you implement Virtual Machine Scale Sets and configure all your virtual machines in the same way, you gain true _autoscaling_. Virtual Machine Scale Sets automatically increases the number of your virtual machine instances as application demand increases, and reduces the number of machine instances as demand decreases.

With Virtual Machine Scale Sets, you don't need to pre-provision your virtual machines. It's easier to build large-scale services that target large compute, big data, and containerized workloads. As workloads increase, more virtual machine instances can be added. As workloads decrease, virtual machines instances can be removed. The process of adding and removing machines can be manual or automated, or a combination of both.

### Things to know about Azure Virtual Machine Scale Sets

Review the following characteristics of Azure Virtual Machine Scale Sets.

- All virtual machine instances are created from the same base operating system image and configuration. This approach lets you easily manage hundreds of virtual machines without extra configuration tasks or network management.
    
- Virtual Machine Scale Sets support the use of Azure Load Balancer for basic layer-4 traffic distribution, and Azure Application Gateway for more advanced layer-7 traffic distribution and SSL termination.
    
- You can use Virtual Machine Scale Sets to run multiple instances of your application. If one of the virtual machine instances has a problem, customers continue to access your application through another virtual machine instance with minimal interruption.
    
- Customer demand for your application might change throughout the day or week. To meet customer demand, Virtual Machine Scale Sets implements autoscaling to automatically increase and decrease the number of virtual machines.
    
- Virtual Machine Scale Sets support up to 1,000 virtual machine instances. If you create and upload your own custom virtual machine images, the limit is 600 virtual machine instances.

# Create Virtual Machine Scale Sets

You can implement Azure Virtual Machine Scale Sets in the Azure portal. You specify the number of virtual machines and their sizes, and indicate preferences for using Azure Spot instances, Azure managed disks, and allocation policies.

In the Azure portal, there are several settings to configure to create an Azure Virtual Machine Scale Sets implementation.

![Screenshot that shows how to create Virtual Machine Scale Sets in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machine-availability/media/implement-scale-sets-61516afb.png)

- **Orchestration mode**: Choose how the scale set manages virtual machines. In flexible orchestration mode, you manually create and add a virtual machine of any configuration to the scale set. In uniform orchestration mode, you define a virtual machine model and Azure generates identical instances based on that model.
    
- **Image**: Choose the base operating system or application for the virtual machine (VM).
    
- **VM Architecture**: Azure provides a choice of x64 or Arm64-based virtual machines to run your applications.
    
- **Run with Azure Spot discount**: Azure Spot offers unused Azure capacity at a discounted rate versus pay as you go prices. Workloads should be tolerant to infrastructure loss as Azure may recall capacity.
    
- **Size**: Select a VM size to support the workload that you want to run. The size that you choose then determines factors such as processing power, memory, and storage capacity. Azure offers a wide variety of sizes to support many types of uses. Azure charges an hourly price based on the VM's size and operating system.
    

Under the **Advanced** tab, you can also select the following:

- **Enable scaling beyond 100 instances**: Identify your scaling allocation preference. If you select **No**, your Virtual Machine Scale Sets implementation is limited to one placement group with a maximum capacity of 100. If you select **Yes**, your implementation can span multiple placement groups with capacity up to 1,000. Selecting **Yes** also changes the availability characteristics of your implementation.
    
- **Spreading algorithm**: Microsoft recommends allocating **Max spreading** for your implementation. This approach provides the optimal spreading.

# Implement autoscale

An Azure Virtual Machine Scale Sets implementation can automatically increase or decrease the number of virtual machine instances that run your application. This process is known as _autoscaling_. Autoscaling allows you to dynamically scale your configuration to meet changing workload demands.

![Illustration of a Virtual Machine Scale Sets implementation with a minimum of two virtual machines and a maximum of five machines that autoscale depending on workload demands.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machine-availability/media/autoscale-45b054e0.png)

Autoscaling minimizes the number of unnecessary virtual machine instances that run your application when demand is low. Your customers continue to receive an acceptable level of performance as demand grows and more virtual machine instances are automatically added.

### Things to consider when using autoscaling

Review the following considerations about autoscaling. Think about how this process can benefit your company website implementation.

- **Consider automatic adjusted capacity**. You can create autoscaling rules to define the acceptable performance for a positive customer experience. When the defined thresholds are met, the autoscale rules act to adjust the capacity of your Virtual Machine Scale Sets implementation.
    
- **Consider scale out**. If your application demand increases, the load on the virtual machine instances in your implementation increases. If the increased load is consistent, rather than a brief demand, you can configure autoscale rules to increase the number of virtual machine instances in your implementation.
    
- **Consider scale in**. On an evening or weekend, your application demand might decrease. If the decreased load is consistent over a period of time, you can configure autoscale rules to decrease the number of virtual machine instances in your implementation. The scale-in action reduces the cost to run your Virtual Machine Scale Sets implementation as you only run the number of instances required to meet the current demand.
    
- **Consider scheduled events**. You can implement autoscaling and schedule events to automatically increase or decrease the capacity of your implementation at fixed times.
    
- **Consider overhead**. Using Azure Virtual Machine Scale Sets with autoscaling reduces your management overhead to monitor and optimize the performance of your application.

# Configure autoscale

When you create an Azure Virtual Machine Scale Sets implementation in the Azure portal, you can enable autoscaling. For optimal performance, you should define a minimum, maximum, and default number of virtual machine instances to use during the autoscale process.

In the Azure portal, there are several settings to configure to enable autoscaling with Azure Virtual Machine Scale Sets.

![Screenshot of the settings for configuring virtual machine instances and autoscale in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machine-availability/media/implement-autoscale-74d25345.png)

**Scaling policy**: Manual scale maintains a fixed instance count. Custom autoscale scales the capacity on any schedule, based on any metrics.

- **Minimum number of VMs**: Specify the minimum number of virtual machines that should be available when autoscaling is applied on your Virtual Machine Scale Sets implementation.
    
- **Maximum number of VMs**: Specify the maximum number of virtual machines that can be available when autoscaling is applied on your implementation.
    

**Scale out**

- **CPU threshold**: Specify the CPU usage percentage threshold to trigger the scale-out autoscale rule.
    
- **Duration in minutes**: Duration in minutes is the amount of time that Autoscale engine looks back for metrics. For example, 10 minutes means that every time autoscale runs, it queries metrics for the past 10 minutes. This delay allows your metrics to stabilize and avoids reacting to transient spikes.
    
- **Number of VMs to increase by**: Specify the number of virtual machines to add to your Virtual Machine Scale Sets implementation when the scale-out autoscale rule is triggered.
    

**Scale in**

- **Scale in CPU threshold**: Specify the CPU usage percentage threshold to trigger the scale-in autoscale rule.
    
- **Number of VMs to decrease by**: Specify the number of virtual machines to remove from your implementation when the scale-in autoscale rule is triggered.
    

**Scale in policy**: The [scale-in policy](https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-scale-in-policy) feature provides users a way to configure the order in which virtual machines are scaled-in.

# Interactive lab simulation

## Lab scenario

Your organization is deploying virtual machines in Azure. As the Azure Administrator you need to:

- Determine different virtual machine compute and storage options.
- Implement Virtual Machine Scale Sets, including storage resiliency and scalability options.
- Explore using Azure Virtual Machine Custom Script extensions to automatically configure virtual machines.

## Architecture diagram

![Architecture diagram as explained in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machine-availability/media/lab-08.png)

## Objectives

 Note

The [lab files](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/tree/master/Allfiles/Interactive%20Lab%20Simulation%20Files/08) are available in the GitHub.

- **Task 1**: Deploy zone-resilient Azure virtual machines by using the Azure portal and an Azure Resource Manager template.
    - Create a virtual machine in the Azure portal.
    - Review the template and deploy a second virtual machine.
- **Task 2**: Configure Azure virtual machines by using virtual machine extensions.
    - Create a blob storage container.
    - Upload an Azure PowerShell script. This script installs the Windows Server Web Server role on a virtual machine.
    - Use the custom script extension feature to run the script on a virtual machine. Export the template.
    - Configure the exported template to install the role on a different virtual machine.
- **Task 3**: Scale compute and storage for Azure virtual machines. In this task, you scale compute for Azure virtual machines by changing their size and scale their storage by attaching and configuring their data disks.
    - Resize the virtual machine.
    - Create and attach a new disk to the virtual machine.
    - Use Azure PowerShell to initialize and partition the new disk.
    - Customize the template to resize the virtual machine and change the disk configuration.
- **Task 4**: Register the Microsoft Insights and Microsoft Alerts Management resource providers.
- **Task 5**: Deploy zone-resilient Azure Virtual Machine Scale Sets by using the Azure portal.
    
    - Use the Azure portal to create a Virtual Machine Scale Set.
    - Configure the virtual network to include an inbound rule to allow HTTP.
    - Configure load balancing and manual scaling.
    
    - Deploy the virtual scale set.
- **Task 6**: Configure Azure Virtual Machine Scale Sets by using virtual machine extensions.
    - Upload an Azure PowerShell script to install the install Windows Server Web Server role.
    - Run the script on the virtual machines using the custom script extension feature.
    - Confirm the Internet Information Service (IIS) is now available on the virtual machines.
- **Task 7**: Scale compute and storage for Azure Virtual Machine Scale Sets.
    - Confirm the virtual machines in the scale set are in different regions.
    - Configure autoscale based on a metric.
    - Use Azure PowerShell to start an infinite loop that sends the HTTP requests to the web sites hosted on the instances of Azure Virtual Machine Scale Sets
    - Verify a new resource is provisioned.

 Note

Click on the thumbnail image to start the lab simulation. When you're done, be sure to return to this page so you can continue learning.

[![Screenshot of the simulation page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-virtual-machine-availability/media/simulation-compute-thumbnail.jpg)](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2012)

---

# Configure Azure App Service plans

# Introduction

Azure Administrators need to be able to scale a web application. Scaling enables an application to remain responsive during periods of high demand. Scaling also helps to save money by reducing the resources required when demand drops.

Suppose you work for a large chain of hotels. You're responsible for maintaining the hotel website. Customers visit the website to make new reservations and view details for their current bookings. At certain times of the year, the volume of website traffic grows because customers are browsing hotels for vacations during national/regional holidays. At other times, traffic declines. These website usage patterns are predictable.

In this module, you learn to implementing Azure App Service plans. You learn how different App Service plans provide different pricing and scaling options. You learn how changing the plan affects performance.

The goal of this module is to ensure you can determine the best App Service plan for your application.

## Learning objectives

In this module, you learn how to:

- Identify features and usage cases for Azure App Service.
- Select an appropriate Azure App Service plan pricing tier.
- Scale an Azure App Service plan.
- Scale out an Azure App Service plan.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104).

## Prerequisites

- Basic knowledge of scaling and performance concepts.
- Familiarity with the Azure portal so you can configure the correct App Service plan.

# Implement Azure App Service plans

In Azure App Service, an application runs in an Azure App Service plan. An App Service plan defines a set of compute resources for a web application to run. The compute resources are analogous to a server farm in conventional web hosting. One or more applications can be configured to run on the same computing resources (or in the same App Service plan).

### Things to know about App Service plans

Let's take a closer look at how to implement and use an App Service plan with your virtual machines.

- When you create an App Service plan in a region, a set of compute resources is created for the plan in the specified region. Any applications that you place into the plan run on the compute resources defined by the plan.
    
- Each App Service plan defines three settings:
    
    - **Region**: The region for the App Service plan, such as West US, Central India, North Europe, and so on.
    - **Number of VM instances**: The number of virtual machine instances to allocate for the plan.
    - **Size of VM instances**: The size of the virtual machine instances in the plan, including Small, Medium, or Large.
- You can continue to add new applications to an existing plan as long as the plan has enough resources to handle the increasing load.
    

#### How applications run and scale in App Service plans

The Azure App Service plan is the scale unit of App Service applications. Depending on the pricing tier for your Azure App Service plan, your applications run and scale in a different manner. If your plan is configured to run five virtual machine instances, then all applications in the plan run on all five instances. If your plan is configured for autoscaling, then all applications in the plan are scaled out together based on the autoscale settings.

Here's a summary of how applications run and scale in Azure App Service plan pricing tiers:

- **Free or Shared tier**:
    
    - Applications run by receiving CPU minutes on a shared virtual machine instance.
    - Applications can't scale out.
- **Basic, Standard, Premium, or Isolated tier**:
    
    - Applications run on all virtual machine instances configured in the App Service plan.
    - Multiple applications in the same plan share the same virtual machine instances.
    - If you have multiple deployment slots for an application, all deployment slots run on the same virtual machine instances.
    - If you enable diagnostic logs, perform backups, or run WebJobs, these tasks use CPU cycles and memory on the same virtual machine instances.

### Things to consider when using App Service plans

Review the following considerations about using Azure App Service plans to run and scale your applications. Think about what conditions might apply to running and scaling the hotel website.

- **Consider cost savings**. Because you pay for the computing resources that your App Service plan allocates, you can potentially save money by placing multiple applications into the same App Service plan.
    
- **Consider multiple applications in one plan**. Create a single plan to support multiple applications, to make it easier to configure and maintain shared virtual machine instances. Because the applications share the same virtual machine instances, you need to carefully manage your plan resources and capacity.
    
- **Consider plan capacity**. Before you add a new application to an existing plan, determine the resource requirements for the new application and identify the remaining capacity of your plan.
    
     Important
    
    Overloading an App Service plan can potentially cause downtime for new and existing applications.
    
- **Consider application isolation**. Isolate your application into a new App Service plan when:
    
    - The application is resource-intensive.
    - You want to scale the application independently from the other applications in the existing plan.
    - The application needs resource in a different geographical region.

# Determine Azure App Service plan pricing

The pricing tier of an Azure App Service plan determines what App Service features you get and how much you pay for the plan.

### Things to know about App Service plan pricing tiers

There are six categories of pricing tiers for an Azure App Service plan. Examine the following plan details and think about which plans can support the hotel website requirements.

Expand table

|Feature|Free|Shared|Basic|Standard|Premium|Isolated|
|---|---|---|---|---|---|---|
|Usage|Development, Testing|Development, Testing|Dedicated development, Testing|Production workloads|Enhanced scale, performance|High performance, security, isolation|
|Web, mobile, or API applications|10|100|Unlimited|Unlimited|Unlimited|Unlimited|
|Disk space|1 GB|1 GB|10 GB|50 GB|250 GB|1 TB|
|Auto scale|n/a|n/a|n/a|Supported|Supported|Supported|
|Deployment slots|n/a|n/a|n/a|5|20|20|
|Max instances|n/a|n/a|Up to 3|Up to 10|Up to 30|Up to 100|

#### Free and Shared

The Free and Shared service plans are base tiers that run on the same Azure virtual machines as other applications. Some applications might belong to other customers. These tiers are intended to be used for development and testing purposes only. No SLA is provided for the Free and Shared service plans. Free and Shared plans are metered on a per application basis.

#### Basic

The Basic service plan is designed for applications that have lower traffic requirements, and don't need advanced auto scale and traffic management features. Pricing is based on the size and number of instances you run. Built-in network load-balancing support automatically distributes traffic across instances. The Basic service plan with Linux runtime environments supports Web App for Containers.

#### Standard

The Standard service plan is designed for running production workloads. Pricing is based on the size and number of instances you run. Built-in network load-balancing support automatically distributes traffic across instances. The Standard plan includes auto scale that can automatically adjust the number of virtual machine instances running to match your traffic needs. The Standard service plan with Linux runtime environments supports Web App for Containers.

#### Premium

The Premium service plan is designed to provide enhanced performance for production applications. The upgraded Premium plan, Premium v2, offers Dv2-series virtual machines with faster processors, SSD storage, and double memory-to-core ratio compared to the Standard tier. The new Premium plan also supports higher scale via increased instance count while still providing all the advanced capabilities of the Standard tier. The first generation of Premium plan is still available to support existing customer scaling needs.

#### Isolated

The Isolated service plan is designed to run mission critical workloads that are required to run in a virtual network. The Isolated plan allows customers to run their applications in a private, dedicated environment in an Azure datacenter. The plan offers Dv2-series virtual machines with faster processors, SSD storage, and a double memory-to-core ratio compared to the Standard tier. The private environment used with an Isolated plan is called the App Service Environment. The plan can scale to 100 instances with more available upon request.

# Scale up and scale out Azure App Service

There are two methods for scaling your Azure App Service plan and applications: _scale up_ and _scale out_. You can scale your applications manually or automatically, which is referred to as _autoscale_.

Watch the following video about how to implement automatic scaling for your Azure App Service plan and applications.

### Things to know about Azure App Service scaling

Let's examine the details of scaling for your Azure App Service plan and App Service applications.

- The scale up method increases the amount of CPU, memory, and disk space. Scaling up gives you extra features like dedicated virtual machines, custom domains and certificates, staging slots, autoscaling, and more. You scale up by changing the pricing tier of the Azure App Service plan where your application is placed.
    
- The scale-out method increases the number of virtual machine instances that run your application. You can scale out to as many as 30 instances, depending on your App Service plan pricing tier. Take advantage of App Service Environments in the Isolated tier to further increase your scale-out count to 100 instances. The scale instance count can be configured manually or automatically (autoscale).
    
- With autoscale, you can automatically increase the scale instance count for the scale-out method. Autoscale is based on predefined rules and schedules.
    
- Your App Service plan can be scaled up and down at any time by changing the pricing tier of the plan.
    
![](https://www.youtube.com/watch?v=LS8ZPbQzRpc)

### Things to consider when using Azure App Service scaling

Review the following benefits of implementing scaling for your App Service plan and applications. Think about the scaling advantages for your hotel website.

- **Consider manually adjusting plan tiers**. Start your plan at a lower pricing tier and scale up as needed to acquire more App Service features. Scale down when features are no longer needed, and control your overall costs.
    
    Consider a scenario where you start testing your web app by using the Azure App Service Free tier, where you pay nothing to use the service. After a while, you decide to add a custom DNS name to your web app, so you scale your plan up to the Shared tier. Next, you discover you need to create an SSL binding, so you scale your plan up to the Basic tier. Later, you determine a need for staging environments, so you scale up to the Standard tier. When you need more cores, memory, or storage, you can scale up to a bigger virtual machine size in the same tier.
    
    The same scaling process works in reverse. If you decide you no longer need capabilities or features of a higher tier, scale your plan down to a lower tier and save money.
    
- **Consider autoscale to support users and reduce costs**. Keep serving your users when your application is experiencing high throughput. Implement autoscale to control how many features and support are offered at a given time based on your preference settings and rule conditions. Autoscale helps you save money when the load on your application decreases by automatically reducing your subscribed features.
    
- **Consider no redeployment**. When you change your scale settings, you don't need to change your code or redeploy your applications. Changing your plan scale settings takes only seconds to apply. Your changes affect all applications in your App Service plan.
    
- **Consider scaling for other Azure services**. If your App Service application depends on other Azure services, such as Azure SQL Database or Azure Storage, you can scale these resources separately. These resources aren't managed by your App Service plan.

# Configure Azure App Service autoscale

The autoscale process allows you to have the right amount of resources running to handle the load on your application. You can add resources to support increases in load and save money by removing idle resources.

### Things to know about autoscale

Let's take a closer look at how to use autoscale for your Azure App Service plan and applications.

- To use autoscale, you specify the minimum, and maximum number of instances to run by using a set of rules and conditions.
    
- When your application runs under autoscale conditions, the number of virtual machine instances are automatically adjusted based on your rules. When rule conditions are met, one or more autoscale actions are triggered.
    
- An autoscale setting is read by the autoscale engine to determine whether to scale out or in. Autoscale settings are grouped into profiles.
    
- Autoscale rules include a trigger and a scale action (in or out). The trigger can be metric-based or time-based.
    
    ![Screenshot that shows how to create an autoscale condition in the Azure portal, including settings for the scale mode and instance count.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-app-service-plans/media/web-app-autoscale-94c4da54.png)
    
    - **Metric-based** rules measure application load and add or remove virtual machines based on the load, such as "do this action when CPU usage is above 50%." Example metrics include CPU time, Average response time, and Requests.
        
    - **Time-based** rules (or, schedule-based) allow you to scale when you see time patterns in your load and want to scale before a possible load increase or decrease occurs. An example is "trigger a webhook every 8:00 AM on Saturday in a given time zone."
        
- The autoscale engine uses notification settings.
    
    A notification setting defines what notifications should occur when an autoscale event occurs based on satisfying the criteria of an autoscale setting profile. Autoscale can notify one or more email addresses or make calls to one or more webhooks.
    

### Things to consider when configuring autoscale

There are several considerations to keep in mind when you configure autoscale for your Azure App Service plan and applications.

- **Minimum instance count**. Set a minimum instance count to make sure your application is always running even when there's no load.
    
- **Maximum instance count**. Set a maximum instance count to limit your total possible hourly cost.
    
- **Adequate scale margin**. Make sure your maximum and minimum instance count values are different, and set an adequate margin between the two values. You can automatically scale between the minimum and maximum by using rules you create.
    
- **Scale rule combinations**. Always use a scale-out and scale-in rule combination that performs an increase and decrease. If you don't set a scale-out rule, your application might fail, or performance might degrade under increased loads. If you don't set a scale-in rule, you can experience unnecessary and extensive costs when the load decreases.
    
- **Metric statistics**. Carefully choose the appropriate statistic for your diagnostic metrics, including Average, Minimum, Maximum, and Total.
    
- **Default instance count**. Always select a safe default instance count. The default instance count is important because autoscale scales your service to the count you specify when metrics aren't available.
    
- **Notifications**. Always configure autoscale notifications. It's important to maintain awareness of how your application is performing as the load changes.

---

# Configure Azure App Service

# Introduction

Azure Administrators are interested in solutions that make it easier to deploy and manage their web, mobile, and API applications.

Your company provides consumer research, and your team manages the on-premises servers. The servers you administer run the entire company infrastructure from web servers to databases. The hardware is aging and starting to struggle to keep up with some of the new data analysis applications. Rather than upgrade the hardware, the company decided to deploy Azure App Service.

In this module, you learn how to configure and manage Azure App Service. You learn about configuration settings, deployment slots, and custom domain names. You learn about application backup, recovery, and monitoring.

The goal of this module is to provide you with the knowledge and skills to effectively use Azure App Services.

## Learning objectives

In this module, you learn how to:

- Identify features and usage cases for Azure App Service.
- Create an app with App Service.
- Configure deployment settings, specifically deployment slots.
- Secure your App Service app.
- Configure custom domain names.
- Back up and restore your App Service app.
- Configure Azure Application Insights.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104).

## Prerequisites

- Working knowledge of the Azure portal, so you can configure the service.
    
- Familiarity with cloud-based services, specifically web hosting services.

# Implement Azure App Service

Azure App Service brings together everything you need to create websites, mobile backends, and web APIs for any platform or device. Applications run and scale with ease in both Windows and Linux-based environments.

App Service provides Quickstarts for several products to help you easily create and deploy your Windows and Linux apps:

![Illustration that shows products for which you can use an App Service quickstart to develop and deploy your web, mobile, and API apps.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-app-services/media/web-quickstarts-c154c8e4.png)

### App Service benefits

There are many advantages to using App Service to develop and deploy your web, mobile, and API apps. Review the following table and think about what features can help you host your App Service instances.


| Benefit                                                | Description                                                                                                                                                                                                                                                                                                             |
| ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Multiple languages and frameworks**                  | App Service has first-class support for ASP.NET, Java, Ruby, Node.js, PHP, and Python. You can also run PowerShell and other scripts or executables as background services.                                                                                                                                             |
| **DevOps optimization**                                | App Service supports continuous integration and deployment with Azure DevOps, GitHub, BitBucket, Docker Hub, and Azure Container Registry. You can promote updates through test and staging environments. Manage your apps in App Service by using Azure PowerShell or the cross-platform command-line interface (CLI). |
| **Global scale with high availability**                | App Service helps you scale up or out manually or automatically. You can host your apps anywhere within the Microsoft global datacenter infrastructure, and the App Service SLA offers high availability.                                                                                                               |
| **Connections to SaaS platforms and on-premises data** | App Service lets you choose from more than 50 connectors for enterprise systems (such as SAP), SaaS services (such as Salesforce), and internet services (such as Facebook). You can access on-premises data by using Hybrid Connections and Azure Virtual Networks.                                                    |
| **Security and compliance**                            | App Service is ISO, SOC, and PCI compliant. You can authenticate users with Microsoft Entra ID or with social logins via Google, Facebook, X, or Microsoft. Create IP address restrictions and manage service identities.                                                                                               |
| **Application templates**                              | Choose from an extensive list of application templates in the Azure Marketplace, such as WordPress, Joomla, and Drupal.                                                                                                                                                                                                 |
| **Visual Studio integration**                          | App Service offers dedicated tools in Visual Studio to help streamline the work of creating, deploying, and debugging.                                                                                                                                                                                                  |
| **API and mobile features**                            | App Service provides turn-key CORS support for RESTful API scenarios. You can simplify your mobile app scenarios by enabling authentication, offline data sync, push notifications, and more.                                                                                                                           |
| **Serverless code**                                    | App Service lets you run a code snippet or script on-demand without having to explicitly provision or manage infrastructure. You pay only for the compute time your code actually uses.                                                                                                                                 |

# Create an app with App Service

You can use the Web Apps, Mobile Apps, or API Apps features of Azure App Service, and create your own apps in the Azure portal.

Watch the following video to learn how to create an app with Azure App Service.

### How to create App Services in the Azure portal

![](https://youtu.be/dHTzv-zY17I)

### Things to know about configuration settings

Let's examine some of the basic configuration settings you need to create an app with App Service.

- **Name**: The name for your app must be unique. The name identifies and locates your app in Azure. An example name is `webappces1.azurewebsites.net`. You can map a custom domain name, if you prefer to use that option instead.
    
- **Publish**: App Service hosts (publishes) your app as code or as a Docker Container.
    
- **Runtime stack**: App Service uses a software stack to run your app, including the language and SDK versions. For Linux apps and custom container apps, you can set an optional start-up command or file. Your choices for the stack include .NET Core, .NET Framework, Node.js, PHP, Python, and Ruby. Various versions of each product are available for Linux and Windows.
    
- **Operating system**: The operating system for your app runtime stack can be Linux or Windows.
    
- **Region**: The region location that you choose for your app affects the App Service plans that are available.
    
- **App Service plan**: Your app needs to be associated with an Azure App Service plan to establish available resources, features, and capacity. You can choose from pricing tiers that are available for the region location you selected.
    

#### Post-creation settings

After your app is created, other configuration settings become available in the Azure portal, including app deployment options and path mapping.

![Screenshot that shows other configuration options for an app with the App Service in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-app-services/media/web-app-configuration-27facdc5.png)

Some of the extra configuration settings can be included in the developer's code, while others can be configured in your app. Here are a few of the extra application settings.

- **Always On**: You can keep your app loaded even when there's no traffic. This setting is required for continuous WebJobs or for WebJobs that are triggered by using a CRON expression.
    
- **ARR affinity**: In a multi-instance deployment, you can ensure your app client is routed to the same instance for the life of the session.
    
- **Connection strings**: Connection strings for your app are encrypted at rest and transmitted over an encrypted channel.

# Explore continuous integration and deployment

The Azure portal provides out-of-the-box continuous integration and deployment with Azure DevOps, GitHub, Bitbucket, FTP, or a local Git repository on your development machine. You can connect your web app with any of the above sources and App Service handles the rest for you. App Service auto-synchronizes your code and any future changes to the code into your web app. With Azure DevOps, you can also define your own build and release process. Compile your source code, run tests, and build and deploy the release into your web app every time you commit the code. All of the operations happen implicitly without any need for human administration.

![Illustration that shows two developers sharing a single GitHub source to produce a website built with Azure App Service.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-app-services/media/continuous-development-a0dfd350.png)

### Things to know about continuous deployment

When you create your web app with App Service, you can choose automated or manual deployment. As you review these options, consider which deployment method to implement for your App Service apps.

- **Automated deployment** (continuous integration) is a process used to push out new features and bug fixes in a fast and repetitive pattern with minimal impact on end users. Azure supports automated deployment directly from several sources:
    
    - **Azure DevOps**: Push your code to Azure DevOps (previously known as Visual Studio Team Services), build your code in the cloud, run the tests, generate a release from the code, and finally, push your code to an Azure web app.
        
    - **GitHub**: Azure supports automated deployment directly from GitHub. When you connect your GitHub repository to Azure for automated deployment, any changes you push to your production branch on GitHub are automatically deployed for you.
        
    - **Bitbucket**: With its similarities to GitHub, you can configure an automated deployment with Bitbucket.
        
- **Manual deployment** enables you to manually push your code to Azure. There are several options for manually pushing your code:
    
    - **Git**: The App Service Web Apps feature offers a Git URL that you can add as a remote repository. Pushing to the remote repository deploys your app.
        
    - **CLI**: The `webapp up` command is a feature of the command-line interface that packages your app and deploys it. Deployment can include creating a new App Service web app.
        
    - **Visual Studio**: Visual Studio features an App Service deployment wizard that can walk you through the deployment process.
        
    - **FTP/S**: FTP or FTPS is a traditional way of pushing your code to many hosting environments, including App Service.

# Create deployment slots

When you deploy your web app, web app on Linux, mobile backend, or API app to Azure App Service, you can use a separate deployment slot instead of the default production slot.

### Things to know about deployment slots

Let's take a closer look at the characteristics of deployment slots.

- Deployment slots are live apps that have their own hostnames.
    
- Deployment slots are available in the Standard, Premium, and Isolated App Service pricing tiers. Your app needs to be running in one of these tiers to use deployment slots.
    
- The Standard, Premium, and Isolated tiers offer different numbers of deployment slots.
    
- App content and configuration elements can be swapped between two deployment slots, including the production slot.
    

![Screenshot that shows how to work with deployment slots in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-app-services/media/deployment-slots-5b3660cc.png)

### Things to consider when using deployment slots

There are several advantages to using deployment slots with your App Service app. Review the following benefits and think about how they can support your App Service implementation.

- **Consider validation**. You can validate changes to your app in a staging deployment slot before swapping the app changes with the content in the production slot.
    
- **Consider reductions in downtime**. Deploying an app to a slot first and swapping it into production ensures that all instances of the slot are warmed up before being swapped into production. This option eliminates downtime when you deploy your app. The traffic redirection is seamless, and no requests are dropped because of swap operations. The entire workflow can be automated by configuring **Auto swap** when pre-swap validation isn't needed.
    
- **Consider restoring to last known good site**. After a swap, the slot with the previously staged app now has the previous production app. If the changes swapped into the production slot aren't as you expected, you can perform the same swap immediately to return to your "last known good site."
    
- **Consider Auto swap**. Auto swap streamlines Azure DevOps scenarios where you want to deploy your app continuously with zero cold starts and zero downtime for app customers. When Auto swap is enabled from a slot into production, every time you push your code changes to that slot, App Service automatically swaps the app into production after it's warmed up in the source slot. Auto swap isn't currently supported for Web Apps on Linux.

# Add deployment slots

Completed100 XP

- 5 minutes

Deployment slots are configured in the Azure portal. You can swap your app content and configuration elements between deployment slots, including the production slot.

### How to use deployment slots in Azure App Service

![](https://youtu.be/5rR5C4Z5dU4)

### Things to know about creating deployment slots

Let's review some details about how deployment slots are configured.

- New deployment slots can be empty or cloned.
    
- Deployment slot settings fall into three categories:
    
    - Slot-specific app settings and connection strings (if applicable)
    - Continuous deployment settings (when enabled)
    - Azure App Service authentication settings (when enabled)
- When you clone a configuration from another deployment slot, the cloned configuration is editable. Some configuration elements follow the content across the swap. Other slot-specific configuration elements stay in the source slot after the swap.
    

#### Swapped settings versus slot-specific settings

The following table lists the settings that are swapped between deployment slots, and settings that remain in the source slot (slot-specific). As you review these settings, consider which features are required for your App Service apps.

Expand table

|Swapped settings|Slot-specific settings|
|---|---|
|General settings, such as framework version, 32/64-bit, web sockets  <br>App settings *****  <br>Connection strings *****  <br>Handler mappings  <br>Public certificates  <br>WebJobs content  <br>Hybrid connections ******  <br>Service endpoints ******  <br>Azure Content Delivery Network ******  <br>Path mapping|Custom domain names  <br>Nonpublic certificates and TLS/SSL settings  <br>Scale settings  <br>Always On  <br>IP restrictions  <br>WebJobs schedulers  <br>Diagnostic settings  <br>Cross-origin resource sharing (CORS)  <br>Virtual network integration  <br>Managed identities  <br>Settings that end with the suffix _EXTENSION_VERSION|

***** Setting can be configured to be slot-specific.

****** Feature isn't currently available.

# Secure your App Service app

Azure App Service provides built-in authentication and authorization support. You can sign in users and access data by writing minimal or no code in your web app, API, and mobile backend, and also your Azure Functions apps.

Secure authentication and authorization require deep understanding of security, including federation, encryption, JSON web tokens (JWT) management, grant types, and so on. App Service provides these utilities so you can spend more time and energy on providing business value to your customer.

 Note

You aren't required to use Azure App Service for authentication and authorization. Many web frameworks are bundled with security features, and you can use your preferred service.

### Things to know about app security with App Service

Let's take a closer look at how App Service helps you provide security for your app.

- The authentication and authorization security module in Azure App Service runs in the same environment as your application code, yet separately.
    
- The security module is configured by using app settings. No SDKs, specific languages, or changes to your application code are required.
    
- When you enable the security module, every incoming HTTP request passes through the module before it's handled by your application code.
    
- The security module handles several tasks for your app:
    
    - Authenticate users with the specified provider
    - Validate, store, and refresh tokens
    - Manage the authenticated session
    - Inject identity information into request headers

### Things to consider when using App Service for app security

You configure authentication and authorization security in App Service by selecting features In the Azure portal. Review the following options and think about what security can benefit your App Service apps implementation.

- **Allow Anonymous requests (no action)**. Defer authorization of unauthenticated traffic to your application code. For authenticated requests, App Service also passes along authentication information in the HTTP headers. This feature provides more flexibility for handling anonymous requests. With this feature, you can present multiple sign-in providers to your users.
    
- **Allow only authenticated requests**. Redirect all anonymous requests to `/.auth/login/<provider>` for the provider you choose. The feature is equivalent to **Log in with <provider/>**. If the anonymous request comes from a native mobile app, the returned response is an `HTTP 401 Unauthorized` message. With this feature, you don't need to write any authentication code in your app.
    
     Important
    
    This feature restricts access to **all** calls to your app. Restricting access to all calls might not be desirable if your app requires a public home page, as is the case for many single-page apps.
    
- **Logging and tracing**. View authentication and authorization traces directly in your log files. If you see an authentication error that you didn’t expect, you can conveniently find all the details by looking in your existing application logs. If you enable failed request tracing, you can see exactly how the security module participated in a failed request. In the trace logs, look for references to a module named `EasyAuthModule_32/64`.


# Create custom domain names


When you create a web app, Azure assigns the app to a subdomain of `azurewebsites.net`. Suppose your web app is named `contoso`. Azure creates a URL for your web app as `contoso.azurewebsites.net`. Azure also assigns a virtual IP address for your app. For a production web app, you might want users to see a custom domain name.

### How to add and secure a custom domain on your App Service web app

![](https://youtu.be/bXP6IvNYISw)

### Steps to configure a custom domain name for your app

There are three steps to create a custom domain name. The following steps outline how to create a domain name in the Azure portal.

 Important

To map a custom DNS name to your app, you need a paid tier of an App Service plan for your app.

1. **Reserve your domain name**. The easiest way to set up a custom domain is to buy one directly in the Azure portal. (This name isn't the Azure assigned name of `\*.azurewebsites.net`.) The registration process enables you to manage your web app's domain name directly in the Azure portal instead of going to a third-party site. Configuring the domain name in your web app is also a simple process in the Azure portal.
    
2. **Create DNS records to map the domain to your Azure web app**. The Domain Name System (DNS) uses data records to map domain names to IP addresses. There are several types of DNS records.
    
    - For web apps, you create either an `A` (Address) record or a `CNAME` (Canonical Name) record.
        
        - An `A` record maps a domain name to an IP address.
        - A `CNAME` record maps a domain name to another domain name. DNS uses the second name to look up the address. Users still see the first domain name in their browser. As an example, you could map `contoso.com` to your `webapp.azurewebsites.net` URL.
    - If the IP address changes, a `CNAME` entry is still valid, whereas an `A` record must be updated.
        
    - Some domain registrars don't allow `CNAME` records for the root domain or for wildcard domains. In such cases, you must use an `A` record.
        
3. **Enable the custom domain**. After you have your domain and create your DNS record, use the Azure portal to validate your custom domain and add it to your web app. Be sure to test your domain before publishing.

# Back up and restore your App Service app

The Backup and Restore feature in Azure App Service lets you easily create backups manually or on a schedule. You can configure the backups to be retained for a specific or indefinite amount of time. You can restore your app or site to a snapshot of a previous state by overwriting the existing content or restoring to another app or site.

Watch the following video on how to configure a backup for your App Service instance. This video is based on [Azure Tips and Tricks #28 - Configure a backup for Azure App Service](https://microsoft.github.io/AzureTipsAndTricks/blog/blog/tip28.html).

![](https://youtu.be/uQXDkW1pCzs)
### Things to know about Backup and Restore

Examine the following details about the Backup and Restore feature. Think about how you can implement this feature for your App Service apps.

- To use the Backup and Restore feature, you need the Standard or Premium tier App Service plan for your app or site.
    
- You need an Azure storage account and container in the same subscription as the app to back up.
    
- Azure App Service can back up the following information to the Azure storage account and container you configured for your app:
    
    - App configuration settings
    - File content
    - Any database connected to your app (SQL Database, Azure Database for MySQL, Azure Database for PostgreSQL, MySQL in-app)
- In your storage account, each backup consists of a Zip file and XML file:
    
    - The Zip file contains the back-up data for your app or site.
    - The XML file contains a manifest of the Zip file contents.
- You can configure backups manually or on a schedule.
    
- Full backups are the default.
    
- Partial backups are supported. You can specify files and folders to exclude from a backup.
    
- You restore partial backups of your app or site the same way you restore a regular backup.
    
- Backups can hold up to 10 GB of app and database content.
    
- Backups for your app or site are visible on the **Containers** page of your storage account and app (or site) in the Azure portal.
    

### Things to consider when creating backups and restoring backups

Let's review some considerations about creating a backup for your app or site, and restoring data and content from a backup.

- **Consider full backups**. Do a full backup to easily save all configuration settings, all file content, and all database content connected with your app or site.
    
    When you restore a full backup, all content on the site is replaced with whatever is in the backup. If a file is on the site, but not in the backup, the file is deleted.
    
- **Consider partial backups**. Specify a partial backup so you can choose exactly which files to back up.
    
    When you restore a partial backup, any content located in an excluded folder or file is left as-is.
    
- **Consider browsing back-up files**. Unzip and browse the Zip and XML files associated with your backup to access your backups. This option lets you view the content without actually performing an app or site restore.
    
- **Consider firewall on back-up destination**. If your storage account is enabled with a firewall, you can't use the storage account as the destination for your backups.

# Use Azure Application Insights

Azure Application Insights is a feature of Azure Monitor that lets you monitor your live applications. You can integrate Application Insights with your App Service configure to automatically detect performance anomalies in your apps.

Application Insights is designed to help you continuously improve the performance and usability of your apps. The feature offers powerful analytics tools to help you diagnose issues and understand what users actually do with your apps.

### Things to know about Application Insights

Let's examine some characteristics of Application Insights for Azure Monitor.

- Application Insights works on various platforms including .NET, Node.js and Java EE.
    
- The feature can be used for configurations that are hosted on-premises, in a hybrid environment, or in any public cloud.
    
- Application Insights integrates with your Azure DevOps process, and has connection points to many development tools.
    
- You can monitor and analyze data from mobile apps by integrating with Visual Studio App Center.
    

![Diagram that shows Azure Application Insights receiving information from web pages, client apps, and web services, which is transferred to Alerts, Power BI, and Visual Studio.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-app-services/media/app-insights-16629887.png)

### Things to consider when using Application Insights

Application Insights is ideal for supporting your development team. The feature helps developers understand how your app is performing and how it's being used. Consider monitoring the following items in your App Service configuration scenario.

- **Consider Request rates, response times, and failure rates**. Find out which pages are most popular, at what times of day, and where your users are. See which pages perform best. If your response times and failure rates go high when there are more requests, then perhaps you have a resourcing problem.
    
- **Consider Dependency rates, response times, and failure rates**. Use Application Insights to discover if external services are degrading your app performance.
    
- **Consider Exceptions**. Analyze the aggregated statistics, or pick specific instances and drill into the stack trace and related requests. Both server and browser exceptions are reported.
    
- **Consider Page views and load performance**. Collect the number of page views reported by your users' browsers and analyze the load performance.
    
- **Consider User and session counts**. Application Insights can help you keep track of the number of users and sessions connected to your app.
    
- **Consider Performance counters**. Add Application Insights performance counters from your Windows or Linux server machines. Monitor performance output for the CPU, memory, network usage, and so on.
    
- **Consider Host diagnostics**. Integrate diagnostics from Docker or Azure into your app Application Insights.
    
- **Consider Diagnostic trace logs**. Implement trace logs from your app to help correlate trace events with requests and diagnose issues.
    
- **Consider Custom events and metrics**. Write your own custom events and metric tracking algorithms as client or server code. Track business events such as number of items sold, or number of games won.

# Interactive lab simulation

## Lab scenario

Your organization is migrating on-premises web apps to Azure. As the Azure Administrator you need to:

- Host web sites running on Windows servers by using the PHP runtime stack.
- Implement Azure DevOps practices by using Azure Web Apps deployment slots.

## Architecture diagram

![Architecture diagram as explained in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-app-services/media/lab-09a.png)

## Objectives

- **Task 1**: Create an Azure web app.
    - Create a web app by using the Azure portal.
    - The web app should run on Windows and use the PHP runtime stack.
- **Task 2**: Create a staging deployment slot.
    - Verify there's a production deployment slot.
    - Create a new staging deployment slot.
- **Task 3**: Configure Web App deployment settings.
    - Deploy your web app from a local Git session.
    - Provide the authentication credentials.
- **Task 4**: Deploy code to the staging deployment slot.
    - Use Azure PowerShell to clone the remote repository and set the local path.
    - Add the remote Git session by using the authentication credentials.
    - Display the default web page in a new browser tab.
    - Push the sample web app code from the local repository to the Azure Web App staging deployment slot.
- **Task 5**: Swap the staging slots.
    - Swap the deployment slots.
    - Verify the default web page is replaced with the Hello World page.
- **Task 6**: Configure and test autoscaling of your Azure web app.
    - Configure a custom autoscale rule on the production deployment slot.
    - The scale rule should use the CPU percentage to increase the resource count.
    - Use Azure PowerShell to start an infinite loop that sends the HTTP requests to your web app.
    - Confirm the resource count automatically scales.

 Note

Select the thumbnail image to start the lab simulation. When you're done, be sure to return to this page so you can continue learning.

 Note

You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same.

[![Screenshot of the simulation page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-app-services/media/simulation-web-apps-thumbnail.jpg)](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2013)

---

# Configure Azure Container Instances

# Introduction

Containers and virtual machines are both forms of virtualization, but there are some key differences between them.

To provide context, let's consider a scenario: You're an Azure Administrator responsible for deploying and managing applications in a cloud environment. Your organization is looking for a solution that offers fast startup times, easy management, and the ability to run applications in isolated containers. You want to understand the benefits of using Azure Container Instances and how it compares to virtual machines.

In this module, you learn when to use Azure Container Instances instead of virtual machines. You also get an overview of features and use cases.

The goal of this module is to introduce you to Azure Container Instances.

## Learning objectives

In this module, you learn how to:

- Identify when to use containers versus virtual machines.
- Identify the features and usage cases of Azure Container Instances.
- Implement Azure container groups.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104).

## Prerequisites

- Working knowledge of containerization concepts and terminology.
- Familiarity with cloud computing and experience with the Azure portal.

# Compare containers to virtual machines

Hardware virtualization makes it possible to run multiple isolated instances of operating systems concurrently on the same physical hardware. Containers represent the next stage in the virtualization of computing resources.

Container-based virtualization allows you to virtualize the operating system. This approach lets you run multiple applications within the same instance of an operating system, while maintaining isolation between the applications. The containers within a virtual machine provide functionality similar to that of virtual machines within a physical server.

### Things to know about containers versus virtual machines

To better understand container-based virtualization, let's compare containers and virtual machines.

Expand table

|Compare|Containers|Virtual machines|
|---|---|---|
|**Isolation**|A container typically provides lightweight isolation from the host and other containers, but a container doesn't provide as strong a security boundary as a virtual machine.|A virtual machine provides complete isolation from the host operating system and other virtual machines. This separation is useful when a strong security boundary is critical, such as hosting apps from competing companies on the same server or cluster.|
|**Operating system**|Containers run the user mode portion of an operating system and can be tailored to contain just the needed services for your app. This approach helps you use fewer system resources.|Virtual machines run a complete operating system including the kernel, which requires more system resources (CPU, memory, and storage).|
|**Deployment**|You can deploy individual containers by using Docker via the command line. You can deploy multiple containers by using an orchestrator such as Azure Kubernetes Service.|You can deploy individual virtual machines by using Windows Admin Center or Hyper-V Manager. You can deploy multiple virtual machines by using PowerShell or System Center Virtual Machine Manager.|
|**Persistent storage**|Containers use Azure Disks for local storage for a single node, or Azure Files (SMB shares) for storage shared by multiple nodes or servers.|Virtual machines use a virtual hard disk (VHD) for local storage for a single machine, or an SMB file share for storage shared by multiple servers.|
|**Fault tolerance**|If a cluster node fails, the orchestrator on another cluster node rapidly recreates any containers running on the node.|Virtual machines can fail over to another server in a cluster, where the virtual machine's operating system restarts on the new server.|

### Things to consider when using containers

Containers offer several advantages over physical and virtual machines. Review the following benefits and consider how you can implement containers for the internal apps for your company.

- **Consider flexibility and speed**. Gain increased flexibility and speed when developing and sharing your containerized application code.
    
- **Consider testing**. Choose containers for your configuration to allow for simplified testing of your apps.
    
- **Consider app deployment**. Implement containers to gain streamlined and accelerated deployment of your apps.
    
- **Consider workload density**. Support higher workload density and improve your resource utilization by working with containers.
    

### Understand container images

All containers are created from container images. A container image is a lightweight, standalone, executable package of software that encapsulates everything needed to run an application. It includes the following components:

- **Code**: The application’s source code.
- **Runtime**: The environment required to execute the application.
- **System tools**: Utilities necessary for the application to function.
- **System libraries**: Shared libraries used by the application.
- **Settings**: Configuration parameters specific to the application.

When you create a container image, it becomes a portable unit that can run consistently across different computing environments. These images are the building blocks for containers, which are instances of these images running at runtime.

# Review Azure Container Instances

Containers are becoming the preferred way to package, deploy, and manage cloud applications. There are many options for teams to build and deploy cloud native and containerized applications on Azure. In this unit, we review Azure Container Instances (ACI).

Azure Container Instances offers the fastest and simplest way to run a container in Azure, without having to manage any virtual machines and without having to adopt a higher-level service. Azure Container Instances is a great solution for any scenario that can operate in isolated containers, including simple applications, task automation, and build jobs, because it provides a single pod of Hyper-V isolated containers on demand.

The following illustration shows a web server container built with Azure Container Instances. The container is running on a virtual machine in a virtual network.

![Diagram that shows a web server container running on a virtual machine in a virtual network.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-container-instances/media/container-overview-0e72c2ba.png)

### Things to know about Azure Container Instances

Let's review some of the benefits of using Azure Container Instances. As you review these points, think about how you can implement Container Instances for your internal applications.

- **Fast startup times**. Containers can start in seconds without the need to provision and manage virtual machines.
    
- **Public IP connectivity and DNS names**. Containers can be directly exposed to the internet with an IP address and FQDN (fully qualified domain name).
    
- **Custom sizes**. Container nodes can be scaled dynamically to match actual resource demands for an application.
    
- **Persistent storage**. Containers support direct mounting of Azure Files file shares.
    
- **Linux and Windows containers**. Container Instances can schedule both Windows and Linux containers. Specify the operating system type when you create your container groups.
    
- **Coscheduled groups**. Container Instances supports scheduling of multi-container groups that share host machine resources.
    
- **Virtual network deployment**. Container Instances can be deployed into an Azure virtual network.

# Implement container groups

The top-level resource in Azure Container Instances is the **container group**. A container group is a collection of containers that get scheduled on the same host machine. The containers in a container group share a lifecycle, resources, local network, and storage volumes.

### Things to know about container groups

Let's review some of details about container groups for Azure Container Instances.

- A container group is similar to a pod in Kubernetes. A pod typically has a 1:1 mapping with a container, but a pod can contain multiple containers. The containers in a multi-container pod can share related resources.
    
- Azure Container Instances allocates resources to a multi-container group by adding together the resource requests of all containers in the group. Resources can include items such as CPUs, memory, and GPUs.
    
    Consider a container group that has two containers that each require CPU resources. Each container requests one CPU. Azure Container Instances allocates two CPUs for the container group.
    
- There are two common ways to deploy a multi-container group: Azure Resource Manager (ARM) templates and YAML files.
    
    - **ARM template**. An ARM template is recommended for deploying other Azure service resources when you deploy your container instances, such as an Azure Files file share.
        
    - **YAML file**. Due to the concise nature of the YAML format, a YAML file is recommended when your deployment includes only container instances.
        
- Container groups can share an external-facing IP address, one or more ports on the IP address, and a DNS label with an FQDN.
    
    - **External client access**. You must expose the port on the IP address and from the container to enable external clients to reach a container in your group.
        
    - **Port mapping**. Port mapping isn't supported because containers in a group share a port namespace.
        
    - **Deleted groups**. When a container group is deleted, its IP address and FQDN are released.
        

#### Configuration example

Consider the following example of a multi-container group with two containers.

![Diagram that depicts an Azure Container Instances multi-container group that has two containers.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-container-instances/media/container-groups-ea19ee6b.png)

The multi-container group has the following characteristics and configuration:

- The container group is scheduled on a single host machine, and is assigned a DNS name label.
- The container group exposes a single public IP address with one exposed port.
- One container in the group listens on port 80. The other container listens on port 1433.
- The group includes two Azure Files file shares as volume mounts. Each container in the group mounts one of the file shares locally.

### Things to consider when using container groups

Multi-container groups are useful when you want to divide a single functional task into a few container images. Different teams can deliver the images, and the images can have separate resource requirements.

Consider the following scenarios for working with multi-container groups. Think about what options can support your internal apps for the online retailer.

- **Consider web app updates**. Support updates to your web apps by implementing a multi-container group. One container in the group serves the web app and another container pulls the latest content from source control.
    
- **Consider log data collection**. Use a multi-container group to capture logging and metrics data about your app. Your application container outputs logs and metrics. A logging container collects the output data and writes the data to long-term storage.
    
- **Consider app monitoring**. Enable monitoring for your app with a multi-container group. A monitoring container periodically makes a request to your application container to ensure your app is running and responding correctly. The monitoring container raises an alert if it identifies possible issues with your app.
    
- **Consider front-end and back-end support**. Create a multi-container group to hold your front-end container and back-end container. The front-end container can serve a web app. The back-end container can run a service to retrieve data.

# Review Azure Container Apps

There are many options for teams to build and deploy cloud native and containerized applications on Azure. Let's understand which scenarios and use cases are best suited for Azure Container Apps and how it compares to other container options on Azure.

### Things to know about Azure Container Apps

Azure Container Apps is a serverless platform that allows you to maintain less infrastructure and save costs while running containerized applications. Instead of worrying about server configuration, container orchestration, and deployment details, Container Apps provides all the up-to-date server resources required to keep your applications stable and secure.

Common uses of Azure Container Apps include:

- Deploying API endpoints
- Hosting background processing jobs
- Handling event-driven processing
- Running microservices

Additionally, applications built on Azure Container Apps can dynamically scale based on the following characteristics:

- HTTP traffic
- Event-driven processing
- CPU or memory load
- Any KEDA-supported scaler

### Things to consider when using Azure Container Apps

Azure Container Apps enables you to build serverless microservices and jobs based on containers. Distinctive features of Container Apps include:

- Optimized for running general purpose containers, especially for applications that span many microservices deployed in containers.
- Powered by Kubernetes and open-source technologies like Dapr, KEDA, and envoy.
- Supports Kubernetes-style apps and microservices with features like service discovery and traffic splitting.
- Enables event-driven application architectures by supporting scale based on traffic and pulling from event sources like queues, including scale to zero.
- Supports running on demand, scheduled, and event-driven jobs.

Azure Container Apps doesn't provide direct access to the underlying Kubernetes APIs. If you would like to build Kubernetes-style applications and don't require direct access to all the native Kubernetes APIs and cluster management, Container Apps provides a fully managed experience based on best-practices. For these reasons, many teams may prefer to start building container microservices with Azure Container Apps.

#### Compare container management solutions

Azure Container Instances (ACI) can be managed in several ways. Azure Container Apps (ACA) is one way, and Azure Kubernetes Service (AKS) is another. Here’s a comparison table for when to use ACA and AKS.

Expand table

|Feature|Azure Container Apps (ACA)|Azure Kubernetes Service (AKS)|
|---|---|---|
|Overview|ACA is a serverless container platform that simplifies the deployment and management of microservices-based applications by abstracting away the underlying infrastructure.|AKS simplifies deploying a managed Kubernetes cluster in Azure by offloading the operational overhead to Azure. It’s suitable for complex applications that require orchestration.|
|Deployment|ACA provides a PaaS experience with quick deployment and management capabilities.|AKS offers more control and customization options for Kubernetes environments, making it suitable for complex applications and microservices.|
|Management|ACA builds upon AKS and offers a simplified PaaS experience for running containers, with additional features, like Dapr for microservices.|AKS provides a more granular control over the Kubernetes environment, suitable for teams with Kubernetes expertise.|
|Scalability|ACA supports both HTTP-based autoscaling and event-driven scaling, making it ideal for applications that need to respond quickly to changes in demand.|AKS offers horizontal pod autoscaling and cluster autoscaling, providing robust scalability options for containerized applications.|
|Use Cases|ACA is designed for microservices and serverless applications that benefit from rapid scaling and simplified management.|AKS is best for complex, long-running applications that require full Kubernetes features and tight integration with other Azure services.|
|Integration|ACA integrates with Azure Logic Apps, Functions, and Event Grid for event-driven architectures.|AKS provides features like Azure Policy for Kubernetes, Azure Monitor for containers, and Azure Defender for Kubernetes for comprehensive security and governance.|


# Interactive lab simulation


## Lab scenario

Your organization needs a new platform for its virtualized workloads. As the Azure Administrator you need to:

- Evaluate Azure Container Instances.
- Identify and test your app container by using Docker images.

## Architecture diagram

![Architecture diagram as explained in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-container-instances/media/lab-09b.png)

## Objectives

- **Task 1**: Deploy an Azure Container Instances using a Docker image.
    - Create a new container instance by using a [Linux image](https://github.com/Azure-Samples/aci-helloworld).
    - Configure a valid globally unique DNS host name. This host name is used to test the container.
- **Task 2**: Review the functionality of Azure Container Instances.
    - Confirm the container instance is running.
    - Verify that the Welcome to Azure Container Instances page is displayed.

 Note

Select the thumbnail image to start the lab simulation. When you're done, be sure to return to this page so you can continue learning.

[![Screenshot of the simulation page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-container-instances/media/simulation-container-thumbnail.jpg)](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014)

---

# Manage virtual machines with the Azure CLI

# What is the Azure CLI?

It's quite common for IT departments to manage a large set of Azure resources, ranging from Azure Virtual Machines to managed websites.

While the Azure portal is easy to use for one-off tasks, navigating through the various panes adds time when you have to create, change, or delete multiple things. This is where the command line shines; you can issue commands quickly and efficiently, or even use scripts to run repetitive tasks. With Azure, you have two different command-line tools you can work with: Azure PowerShell and the Azure CLI.

With either of these tools, you can write scripts to check the cloud-server status, deploy new configurations, open ports in the firewall, or connect to a virtual machine to change a setting. Windows admins tend to prefer Azure PowerShell, while developers and Linux admins often use the Azure CLI.

This module focuses on using the Azure CLI to create and manage virtual machines hosted in Azure. If you'd like to get an overview of the Azure CLI, how to install it, and how to work with your Azure subscriptions, make sure to check out the [Control Azure services with the CLI](https://learn.microsoft.com/en-us/training/modules/control-azure-services-with-cli/) training module.

## What is the Azure CLI?

The Azure CLI is Microsoft's cross-platform command-line tool for managing Azure resources. It's available for macOS, Linux, and Windows, or in the browser using [Azure Cloud Shell](https://learn.microsoft.com/en-us/azure/cloud-shell/overview).

The Azure CLI can help you manage Azure resources such as virtual machines and disks from the command line or in scripts. Let's get started and see what it can do with Azure Virtual Machines.

## Learning objectives

In this module, you will:

- Create a virtual machine with the Azure CLI.
- Resize virtual machines with the Azure CLI.
- Perform basic management tasks using the Azure CLI.
- Connect to a running VM with SSH and the Azure CLI.

## Prerequisites

- Basic understanding of the Azure CLI tool from the [Control Azure services with the CLI](https://learn.microsoft.com/en-us/training/modules/control-azure-services-with-cli/) module.

# Exercise - Create a virtual machine


## Logins, subscriptions, and resource groups

You'll be working in the Azure Cloud Shell on the right. Once you activate the sandbox, you'll be logged into Azure with a free subscription that Microsoft Learn manages. You don't have to log in to Azure on your own or select a subscription; this is done for you. You'd also normally create a _resource group_ to hold new resources. In this module, the Azure sandbox creates a resource group for you, which you'll use to execute all the commands.

## Create a Linux VM with the Azure CLI

The Azure CLI includes the `vm` command to work with virtual machines in Azure. We can supply several subcommands to do specific tasks. The most common include:

Expand table

|Sub-command|Description|
|---|---|
|`create`|Create a new virtual machine|
|`deallocate`|Deallocate a virtual machine|
|`delete`|Delete a virtual machine|
|`list`|List the created virtual machines in your subscription|
|`open-port`|Open a specific network port for inbound traffic|
|`restart`|Restart a virtual machine|
|`show`|Get the details for a virtual machine|
|`start`|Start a stopped virtual machine|
|`stop`|Stop a running virtual machine|
|`update`|Update a property of a virtual machine|

 Note

For a complete list of commands, you can check the [Azure CLI reference documentation](https://learn.microsoft.com/en-us/cli/azure/reference-index).

Let's start with the first one: `az vm create`. You can use this command to create a virtual machine in a resource group. There are several parameters you can pass to configure all the aspects of the new VM. The four parameters that you must supply are:

Expand table

|Parameter|Description|
|---|---|
|`--resource-group`|The resource group that will own the virtual machine; use **[sandbox Resource Group]**.|
|`--name`|The name of the virtual machine; must be unique within the resource group.|
|`--image`|The operating system image to use to create the VM.|
|`--location`|The region in which to place the VM. Typically, this would be close to the VM's consumer.|

In addition, it's helpful to add the `--verbose` flag to see progress while the VM is being created.

## Create a Linux virtual machine

Let's create a new Linux virtual machine. Execute the following command in Azure Cloud Shell to create an Ubuntu VM in the _West US_ location.

Azure CLICopy

```
az vm create \
  --resource-group "[sandbox resource group name]" \
  --location westus \
  --name SampleVM \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --generate-ssh-keys \
  --verbose 
```

 Tip

You can use the **Copy** button to copy commands to the clipboard. To paste, right-click on a new line in the Cloud Shell terminal and select **Paste**, or use the Shift+Insert keyboard shortcut (⌘+V on macOS).

This command creates a new **Ubuntu** Linux virtual machine with the name `SampleVM`. Notice that the Azure CLI tool waits while the VM is being created. You can add the `--no-wait` option to tell the Azure CLI tool to return immediately and have Azure continue creating the VM in the background. This is useful if you're executing the command in a script.

We're specifying the administrator account name through the `--admin-username` flag to be `azureuser`. If you omit this, the `az vm create` command will use your _current user name_. Because the rules for account names are different for each OS, it's safer to specify a specific name.

 Note

Common names such as "root" and "admin" aren't allowed for most images.

We're also using the `generate-ssh-keys` flag. Linux distributions use this parameter, and it creates a pair of security keys so we can use the `ssh` tool to access the virtual machine remotely. The two files are placed into the `.ssh` folder on your machine and in the VM. If you already have an SSH key named `id_rsa` in the target folder, then that SSH key will be used rather than generating a new key.

Once Azure CLI finishes creating the VM, you'll get a JSON response which includes the current state of the virtual machine and its public and private IP addresses assigned by Azure:

JSONCopy

```
{
  "fqdns": "",
  "id": "/subscriptions/aaaa0a0a-bb1b-cc2c-dd3d-eeeeee4e4e4e/resourceGroups/Learn-bbbb1b1b-cc2c-dd3d-ee4e-ffffff5f5f5f/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "westus",
  "macAddress": "00-0D-3A-58-F8-45",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
  "resourceGroup": "bbbb1b1b-cc2c-dd3d-ee4e-ffffff5f5f5f",
  "zones": ""
}
```


# Exercise - Test your new virtual machine


When you create a virtual machine, it's assigned a public IP address that's reachable over the internet and a private IP address used within the Azure datacenter. Both of these values appear in the JSON block the `create` command returns, like the following:

JSONCopy

```
{
   ...
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
   ...
}
```

## Connecting to the VM with SSH

We can quickly test that the Linux VM is up and running by using the public IP address in the Secure Shell (`ssh`) tool. Remember that we set our admin name to `azureuser`, so we need specify that. Make sure to use the public IP address from _your_ running instance.

Azure CLICopy

```
ssh azureuser@<public-ip-address>
```

 Note

We don't need a password because we generated an SSH key pair as part of the VM creation. The first time you shell into the VM, you receive a prompt regarding the authenticity of the host.

This is because we are attempting to access an IP address directly instead of through a host name. Answering **yes** saveS the IP address as a valid host for connection and allows the connection to proceed.

OutputCopy

```
The authenticity of host '40.83.165.85 (40.83.165.85)' can't be established.
RSA key fingerprint is SHA256:hlFnTCAzgWVFiMxHK194I2ap6+5hZoj9ex8+/hoM7rE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '40.83.165.85' (RSA) to the list of known hosts.
```

Then, you'll be presented with a remote shell where you can enter Linux commands.

OutputCopy

```
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 5.0.0-1014-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Aug 21 20:32:04 UTC 2019

  System load:  0.0               Processes:           108
  Usage of /:   4.2% of 28.90GB   Users logged in:     0
  Memory usage: 9%                IP address for eth0: 10.0.0.5
  Swap usage:   0%

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@SampleVM:~$
```

Try a few commands, such as `ps` or `ls`, as practice. When you're finished, sign out of the virtual machine by typing `exit` or `logout`.

# Exercise - Explore other VM images

We used UbuntuLTS for the image to create the virtual machine. Azure has several standard VM images you can use to create a virtual machine.

## Listing images

You can get a list of the available VM images using the following command:

Azure CLICopy

```
az vm image list --output table
```

 Note

If you get the error _az: command not found_, type `exit` into the shell and try again.

This outputs the most popular images that are part of an offline list built into the Azure CLI. However, there are _hundreds_ of image options available in the Azure Marketplace.

## Getting all images

You can get a full list by adding the `--all` flag to the command. Because the list of images in the Marketplace is very large, it's helpful to filter the list with the `--publisher`, `--sku` or `–-offer` options.

For example, try the following command to see _all_ Wordpress images available:

Azure CLICopy

```
az vm image list --sku Wordpress --output table --all
```

Or this command to see all images provided by Microsoft:

Azure CLICopy

```
az vm image list --publisher Microsoft --output table --all
```

These commands can take a few moments to complete.

## Location-specific images

Some images are only available in certain locations. Try adding the `--location [location]` flag to the command to scope the results to ones available in the region where you want to create the virtual machine. For example, type the following into Azure Cloud Shell to get a list of images available in the `eastus` region.

Azure CLICopy

```
az vm image list --location eastus --output table
```

Try checking some of the images in the other Azure sandbox available locations:

- westus2
- southcentralus
- centralus
- eastus
- westeurope
- southeastasia
- japaneast
- brazilsouth
- australiasoutheast
- centralindia

 Tip

These are the standard images that are provided by Azure. Keep in mind that you can also [create and upload your own custom images](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-custom-images) to create VMs based on unique configurations or less common versions or distributions of an operating system.

Your command window is probably full now. If you like, you can type `clear` to clear the screen.

# Exercise - Sizing VMs properly

Virtual machines must be sized appropriately for the expected work. A VM without the correct amount of memory or CPU will fail under load or run too slowly to be effective.

## Predefined VM sizes

When you create a virtual machine, you can supply a _VM size_ value that determines the amount of compute resources devoted to the VM, including CPU, GPU, and memory made available to the virtual machine from Azure.

Azure defines a set of predefined VM sizes for Linux and Windows from which to choose based on the expected usage.

Expand table

|Type|Sizes|Description|
|---|---|---|
|General purpose|Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7|Balanced CPU-to-memory. Ideal for dev/test and small to medium applications and data solutions.|
|Compute optimized|Fs, F|High CPU-to-memory. Good for medium-traffic applications, network appliances, and batch processes.|
|Memory optimized|Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D|High memory-to-core. Great for relational databases, medium to large caches, and in-memory analytics.|
|Storage optimized|Ls|High disk throughput and IO. Ideal for big data, SQL, and NoSQL databases.|
|GPU optimized|NV, NC|Specialized VMs targeted for heavy graphic rendering and video editing.|
|High performance|H, A8-11|Our most powerful CPU VMs with optional high-throughput network interfaces (RDMA).|

The available sizes change based on the region in which you're creating the VM. You can get a list of the available sizes using the `vm list-sizes` command. Try typing the following command into Azure Cloud Shell:

Azure CLICopy

```
az vm list-sizes --location eastus --output table
```

Here's an abbreviated response for `eastus`:

OutputCopy

```
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          2048  Standard_B1ms                         1           1047552                    4096
                 2          1024  Standard_B1s                          1           1047552                    2048
                 4          8192  Standard_B2ms                         2           1047552                   16384
                 4          4096  Standard_B2s                          2           1047552                    8192
                 8         16384  Standard_B4ms                         4           1047552                   32768
                16         32768  Standard_B8ms                         8           1047552                   65536
                 4          3584  Standard_DS1_v2                       1           1047552                    7168
                 8          7168  Standard_DS2_v2                       2           1047552                   14336
                16         14336  Standard_DS3_v2                       4           1047552                   28672
                32         28672  Standard_DS4_v2                       8           1047552                   57344
                64         57344  Standard_DS5_v2                      16           1047552                  114688
        ....
                64       3891200  Standard_M128-32ms                  128           1047552                 4096000
                64       3891200  Standard_M128-64ms                  128           1047552                 4096000
                64       3891200  Standard_M128ms                     128           1047552                 4096000
                64       2048000  Standard_M128s                      128           1047552                 4096000
                64       1024000  Standard_M64                         64           1047552                 8192000
                64       1792000  Standard_M64m                        64           1047552                 8192000
                64       2048000  Standard_M128                       128           1047552                16384000
                64       3891200  Standard_M128m                      128           1047552                16384000
```

## Specify a size during VM creation

We didn't specify a size when we created our VM, so Azure selected a default general-purpose size for us. However, we can specify the size as part of the `vm create` command using the `--size` parameter. For example, you could use the following command to create a two-core virtual machine:

Azure CLICopy

```
az vm create \
    --resource-group "[sandbox resource group name]" \
    --name SampleVM2 \
    --image Ubuntu2204 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --verbose \
    --size "Standard_DS2_v2"
```

 Warning

Your subscription tier [enforces limits](https://learn.microsoft.com/en-us/azure/azure-subscription-service-limits) on how many resources you can create, as well as the total size of those resources. Quota limits depend upon your subscription type and region. The Azure CLI lets you know when you exceed this limit with a **Quota Exceeded** error. If you hit this error in your own paid subscription, you can request to raise the limits associated with your paid subscription (up to 10,000 vCPUs) through a [free online request](https://learn.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-quota-errors).

## Resize an existing VM

We can also resize an existing VM if the workload changes or if it was incorrectly sized at creation. Let's use the first VM we created, SampleVM. Before requesting a resize, we must check to see if the desired size is available in the cluster of which our VM is a part. We can use the `vm list-vm-resize-options` command:

Azure CLICopy

```
az vm list-vm-resize-options \
    --resource-group "[sandbox resource group name]" \
    --name SampleVM \
    --output table
```

This command returns a list of all the possible size configurations available in the resource group. If the size we want isn't available in our cluster but _is_ available in the region, we can [deallocate the VM](https://learn.microsoft.com/en-us/cli/azure/vm#az-vm-deallocate). This command stops the running VM and removes it from the current cluster without losing any resources. We can then resize it, which re-creates the VM in a new cluster where the size configuration is available.

 Note

The Microsoft Learn sandbox is limited to a few VM sizes.

To resize a VM, we'll use the `vm resize` command. For example, perhaps we find our VM is underpowered for the task we want it to perform. We could bump it up to a D2s_v3, where it has 2 vCores and 8 GB of memory. Type this command in Cloud Shell:

Azure CLICopy

```
az vm resize \
    --resource-group "[sandbox resource group name]" \
    --name SampleVM \
    --size Standard_D2s_v3
```

This command takes a few minutes to reduce the resources of the VM, and once it's done, it returns a new JSON configuration.


# Exercise - Query system and runtime information about the VM

Now that we've created a virtual machine, we can get information about it through other commands.

Let's start by running `vm list`.

Azure CLICopy

```
az vm list
```

This command will return _all_ virtual machines defined in this subscription. You can filter the output to a specific resource group through the `--resource-group` parameter.

## Output types

Notice that the default response type for all the commands we've done so far is JSON. This is great for scripting, but most people find it harder to read. You can change the output style for any response through the `--output` flag. For example, run the following command in Azure Cloud Shell to see the different output style.

Azure CLICopy

```
az vm list --output table
```

Along with `table`, you can specify `json` (the default), `jsonc` (colorized JSON), or `tsv` (Tab-Separated Values). Try a few variations with the preceding command to see the difference.

## Get the IP address

Another useful command is `vm list-ip-addresses`, which lists the public and private IP addresses for a VM. If they change, or you didn't capture them during creation, you can retrieve them at any time.

Azure CLICopy

```
az vm list-ip-addresses -n SampleVM -o table
```

This returns output like:

OutputCopy

```
VirtualMachine    PublicIPAddresses    PrivateIPAddresses
----------------  -------------------  --------------------
SampleVM          168.61.54.62         10.0.0.4
```

 Tip

Notice that we're using a shorthand syntax for the `--output` flag as `-o`. You can shorten most parameters to Azure CLI commands to a single dash and letter. For example, you can shorten `--name` to `-n` and `--resource-group` to `-g`. This is handy for entering keyboard characters, but we recommend using the full option name in scripts for clarity. Check the documentation for details about each command.

## Get VM details

We can get more detailed information about a specific virtual machine by name or ID running the `vm show` command.

Azure CLICopy

```
az vm show --resource-group "[sandbox resource group name]" --name SampleVM
```

This returns a fairly large JSON block with all sorts of information about the VM, including attached storage devices, network interfaces, and all of the object IDs for resources that the VM is connected to. Again, we could change to a table format, but that omits almost all of the interesting data. Instead, we can turn to a built-in query language for JSON called [JMESPath](http://jmespath.org/).

## Add filters to queries with JMESPath

JMESPath is an industry-standard query language built around JSON objects. The simplest query is to specify an _identifier_ that selects a key in the JSON object.

For example, given the object:

JSONCopy

```
{
  "people": [
    {
      "name": "Fred",
      "age": 28
    },
    {
      "name": "Barney",
      "age": 25
    },
    {
      "name": "Wilma",
      "age": 27
    }
  ]
}
```

We can use the query `people` to return the array of values for the `people` array. If we just want _one_ of the people, we can use an indexer. For example, `people[1]` would return:

JSONCopy

```
{
    "name": "Barney",
    "age": 25
}
```

We can also add specific qualifiers that would return a subset of the objects based on some criteria. For example, adding the qualifier `people[?age > '25']` would return:

JSONCopy

```
[
  {
    "name": "Fred",
    "age": 28
  },
  {
    "name": "Wilma",
    "age": 27
  }
]
```

Finally, we can constrain the results by adding a select: `people[?age > '25'].[name]` that returns just the names:

JSONCopy

```
[
  [
    "Fred"
  ],
  [
    "Wilma"
  ]
]
```

JMESQuery has several other interesting query features. When you have time, check out the [online tutorial](http://jmespath.org/tutorial.html) available on the [JMESPath.org](http://jmespath.org/) site.

## Filter your Azure CLI queries

With a basic understanding of JMES queries, we can add filters to the data returned by queries like the `vm show` command. For example, we can retrieve the admin username:

Azure CLICopy

```
az vm show \
    --resource-group "[sandbox resource group name]" \
    --name SampleVM \
    --query "osProfile.adminUsername"
```

We can get the size assigned to our VM:

Azure CLICopy

```
az vm show \
    --resource-group "[sandbox resource group name]" \
    --name SampleVM \
    --query hardwareProfile.vmSize
```

Or, to retrieve all the IDs for your network interfaces, we can run the query:

Azure CLICopy

```
az vm show \
    --resource-group "[sandbox resource group name]" \
    --name SampleVM \
    --query "networkProfile.networkInterfaces[].id"
```

This query technique works with any Azure CLI command, and you can use it to pull specific bits of data out on the command line. It's useful for scripting, as well. For example, you can pull a value out of your Azure account and store it in an environment or script variable. If you decide to use it this way, it's useful to add the `--output tsv` parameter (which you can shorten to `-o tsv`). This will return results that only include the actual data values with tab separators.

For example:

Azure CLICopy

```
az vm show \
    --resource-group "[sandbox resource group name]" \
    --name SampleVM \
    --query "networkProfile.networkInterfaces[].id" -o tsv
```

returns the text: `/subscriptions/aaaa0a0a-bb1b-cc2c-dd3d-eeeeee4e4e4e/resourceGroups/bbbb1b1b-cc2c-dd3d-ee4e-ffffff5f5f5f/providers/Microsoft.Network/networkInterfaces/SampleVMVMNic`

# Exercise - Start and stop your VM with the Azure CLI

One of the main tasks you'll want to do while running virtual machines is to start and stop them.

## Stop a VM

We can stop a running VM with the `vm stop` command. You must pass the name and resource group or the unique ID for the VM:

Azure CLICopy

```
az vm stop \
    --name SampleVM \
    --resource-group "[sandbox resource group name]"
```

You can verify the VM has stopped by attempting to ping the public IP address, using `ssh`, or through the `vm get-instance-view` command. This final approach returns the same basic data as `vm show`, but includes details about the instance itself. Try entering the following command into Azure Cloud Shell to see the current running state of your VM:

Azure CLICopy

```
az vm get-instance-view \
    --name SampleVM \
    --resource-group "[sandbox resource group name]" \
    --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" -o tsv
```

This command should return `VM stopped` as the result.

## Start a VM

We can do the reverse through the `vm start` command.

Azure CLICopy

```
az vm start \
    --name SampleVM \
    --resource-group "[sandbox resource group name]"
```

This command starts a stopped VM. You can verify it through the `vm get-instance-view` query you used in the last section, which should now return `VM running`.

## Restart a VM

Finally, we can restart a VM if we've made changes that require a reboot by running the `vm restart` command. You can add the `--no-wait` flag if you want the Azure CLI to return immediately without waiting for the VM to reboot.

# Exercise - Install software on your VM

The last thing we want to try on our VM is to install a web server. One of the easiest packages to install is `nginx`.

## Install NGINX web server

1. Locate the public IP address of your _SampleVM_ Linux virtual machine.
    
    Azure CLICopy
    
    ```
    az vm list-ip-addresses --name SampleVM --output table
    ```
    
2. Next, open an `ssh` connection to _SampleVM_ using the Public IP address from the preceding step.
    
    BashCopy
    
    ```
    ssh azureuser@<PublicIPAddress>
    ```
    
3. After you're logged in to the virtual machine, run the following command to install the `nginx` web server. The command takes a few moments to complete.
    
    BashCopy
    
    ```
    sudo apt-get -y update && sudo apt-get -y install nginx
    ```
    
4. Exit the Secure Shell:
    
    BashCopy
    
    ```
    exit
    ```
    

## Retrieve your default page

1. In Azure Cloud Shell, use `curl` to read the default page from your Linux web server by running the following command, replacing `<PublicIPAddress>` with the public IP you found previously. You can also open a new browser tab and try to browse to the public IP address.
    
    BashCopy
    
    ```
    curl -m 80 <PublicIPAddress>
    ```
    
    This command will fail, because the Linux virtual machine doesn't expose port 80 (`http`) through the network security group that secures the network connectivity to the virtual machine. We can fix the failure by running the Azure CLI command `vm open-port`.
    
2. Enter the following command into Cloud Shell to open port 80:
    
    Azure CLICopy
    
    ```
    az vm open-port \
        --port 80 \
        --resource-group "[sandbox resource group name]" \
        --name SampleVM
    ```
    
    It takes a moment to add the network rule and open the port through the firewall.
    
3. Run the `curl` command again.
    
    BashCopy
    
    ```
    curl -m 80 <PublicIPAddress>
    ```
    
    This time, it should return data like the following. You can see the page in a browser as well.
    
    HTMLCopy
    
    ```
    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    <style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
    </style>
    </head>
    <body>
    <h1>Welcome to nginx!</h1>
    <p>If you see this page, the nginx web server is successfully installed and
    working. Further configuration is required.</p>
    
    <p>For online documentation and support, refer to
    <a href="http://nginx.org/">nginx.org</a>.<br/>
    Commercial support is available at
    <a href="http://nginx.com/">nginx.com</a>.</p>
    
    <p><em>Thank you for using nginx.</em></p>
    </body>
    </html>
    ```

---

# Create a Windows virtual machine in Azure


# Introduction

Imagine that you work for a company that does video-data processing and pattern analysis. You're building a new prototype platform to process the video from traffic cameras, analyze trends, and provide actionable data for traffic and road improvements.

To improve your algorithms, you've made arrangements with several new cities to collect their traffic-camera data. However, not all of the video data is in the same format, and many of the formats only have Windows codecs to decode the data. Therefore, you've decided to use Virtual Machines (VMs) to do the initial processing and then push the data onto Azure Functions, which will process a standard format. This approach allows you to bring on new data formats dynamically without stopping the entire system.

Azure provides a robust virtual machine hosting solution that can meet your needs. Let's explore how to create and work with Windows virtual machines in Azure.

## Learning objectives

In this module, you'll:

- Understand the options that are available for virtual machines in Azure.
- Create a Windows virtual machine using the Azure portal.
- Connect to a running Windows virtual machine using Remote Desktop.
- Install software and change the network configuration on a VM using the Azure portal.

## Prerequisites

- Basic understanding of Azure Virtual Machines from [Introduction to Azure Virtual Machines](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-virtual-machines/)
- Remote Desktop client

# Create a Windows virtual machine in Azure

Your company has decided to manage the video data from their traffic cameras in Azure using VMs. In order to run the multiple codecs, we first need to create the VMs. We also need to connect and interact with the VMs. In this unit, you'll learn how to create a VM using the Azure portal. You'll configure the VM for remote access, select a VM image, and choose the proper storage option.

## Introduction to Windows virtual machines in Azure

Azure VMs are an on-demand, scalable cloud-computing resource. They're similar to virtual machines that are hosted in Windows Hyper-V. They include processor, memory, storage, and networking resources. You can start and stop virtual machines at will, just like with Hyper-V, and manage them from the Azure portal or with the Azure CLI. You can also use a Remote Desktop Protocol (RDP) client to connect directly to the Windows desktop user interface (UI) and use the VM as if you were signed in to a local Windows computer.

## Create an Azure VM

You can define and deploy VMs on Azure in several ways: the Azure portal, a script (using the Azure CLI or Azure PowerShell), or through an Azure Resource Manager template. In all cases, you'll need to supply several pieces of information, which we'll cover shortly.

The Azure Marketplace also provides preconfigured images that include both an OS and popular software tools installed for specific scenarios.

![Screenshot showing the Azure Marketplace list of Virtual Machines.](https://learn.microsoft.com/en-us/training/modules/create-windows-virtual-machine-in-azure/media/2-marketplace-vm-choices.png)

## Resources used in a Windows VM

When creating a Windows VM in Azure, you also create resources to host the VM. These resources work together to virtualize a computer and run the Windows operating system. These must either exist (and be selected during VM creation), or they'll be created with the VM.

- A virtual machine that provides CPU and memory resources
- An Azure Storage account to hold the virtual hard disks
- Virtual disks to hold the OS, applications, and data
- A virtual network (VNet) to connect the VM to other Azure services or your own on-premises hardware
- A network interface to communicate with the VNet
- A public IP address so you can access the VM (this is optional)

Like other Azure services, you'll need a **resource group** to contain the VM (and optionally group these resources together for administration). When you create a new VM, you can either use an existing resource group or create a new one.

## Choose the VM image

Selecting an image is one of the first and most important decisions you'll make when creating a VM. An image is a template that's used to create a VM. These templates include an OS and often other software, such as development tools or web-hosting environments.

You can include any application that the computer can support in the VM image. You can create a VM from an image that's preconfigured to exactly match your requirements, such as hosting an ASP.NET Core app.

 Tip

You can also create and upload your own images. [Check the documentation](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-custom-images) for more information.

## Size your VM

Just as a physical machine has a certain amount of memory and CPU power, so does a virtual machine. Azure offers a range of VMs of differing sizes at different price points. The size that you choose will determine the VM's processing power, memory, and max storage capacity.

 Warning

There are quota limits on each subscription that can impact VM creation. In the classic deployment model, you can't have more than 20 virtual _cores_ across all VMs within a region. You can either split up VMs across regions or file an [online request](https://learn.microsoft.com/en-us/azure/azure-supportability/resource-manager-core-quotas-request) to increase your limits.

VM sizes are grouped into categories, starting with the B-series for basic testing, and running up to the H-series for massive computing tasks. You should select the VM's size based on the workload you want to perform. It's possible to change a VM's size after it's been created, but the VM must be stopped first, so it's best to size it appropriately from the start if possible.

### Here are some guidelines based on the scenario you're targeting:

Expand table

|What are you doing?|Consider these sizes|
|---|---|
|**General use computing/web**: Testing and development, small to medium databases, or low to medium traffic web servers|B, Dsv3, Dv3, DSv2, Dv2|
|**Heavy computational tasks**: Medium traffic web servers, network appliances, batch processes, and application servers|Fsv2, Fs, F|
|**Large memory usage**: Relational database servers, medium to large caches, and in-memory analytics.|Esv3, Ev3, M, GS, G, DSv2, Dv2|
|**Data storage and processing**: Big Data, SQL, and NoSQL databases, which need high disk throughput and IO|Ls|
|**Heavy graphics rendering** or video editing, as well as model training and inferencing (ND) with deep learning|NV, NC, NCv2, NCv3, ND|
|**High-performance computing (HPC)**: If you need the fastest and most powerful CPU virtual machines with optional high-throughput network interfaces|H|

## Choose storage options

The next set of decisions revolves around storage. First, you can choose the disk technology. Options include a traditional platter-based hard disk drive (HDD) or a more modern solid-state drive (SSD). Just like the hardware you purchase, SSD storage costs more, but provides better performance.

 Tip

There are two levels of SSD storage available: Standard and Premium. Choose Standard SSD disks if you have normal workloads but want better performance. Choose Premium SSD disks if you have I/O intensive workloads or mission-critical systems that need to process data very quickly.

### Map storage to disks

Azure uses virtual hard disks (VHDs) to represent physical disks for the VM. VHDs replicate the logical format and data of a disk drive, but are stored as page blobs in an Azure Storage account. You can choose on a per-disk basis what type of storage it should use (SSD or HDD). This allows you to control each disk's performance, likely based on the I/O you plan to perform on it.

By default, two virtual hard disks (VHDs) will be created for your Windows VM:

1. **The Operating System disk**: This is your primary or C: drive and has a maximum capacity of 2048 GB.
    
2. **A Temporary disk**: This provides temporary storage for the OS or any apps. It's configured as the D: drive by default and is sized based on the VM size, making it an ideal location for the Windows paging file.
    

 Warning

The temporary disk is not persistent. You should only write data to this disk that you're willing to lose at any time.

#### What about data?

You can store data on the C: drive along with the OS, but a better approach is to create dedicated _data disks_. You can create and attach additional disks to the VM. Each data disk can hold up to 32,767 gibibytes (GiB) of data, with the maximum amount of storage determined by the VM size you select.

 Note

An interesting capability is to create a VHD image from a real disk. This allows you to easily migrate _existing_ information from an on-premises computer to the cloud.

### Unmanaged vs. Managed disks

The final storage choice you'll make is whether to use **unmanaged** or **managed** disks.

With unmanaged disks, you're responsible for the storage accounts that are used to hold the VHDs corresponding to your VM disks. You pay the storage account rates for the amount of space you use. A single storage account has a fixed rate limit of 20,000 I/O operations/sec. This means that a single storage account is capable of supporting 40 standard virtual hard disks at full throttle. If you need to scale out, then you need more than one storage account, which can get complicated.

Managed disks are the newer (and recommended) disk-storage model. They elegantly solve the complexity of unmanaged disks by putting the burden of managing the storage accounts onto Azure. You specify the disk type (Premium or Standard) and the disk size, and Azure creates and manages both the disk _and_ the storage it uses. You don't have to worry about storage account limits, which makes them easier to scale out. They also offer several other benefits:

- **Increased reliability**: Azure ensures that VHDs associated with high-reliability VMs will be placed in different parts of Azure storage to provide similar levels of resilience.
- **Better security**: Managed disks are truly managed resources in the resource group. This means they can use role-based access control (RBAC) to restrict who can work with the VHD data.
- **Snapshot support**: You can use snapshots to create a read-only copy of a VHD. You have to shut down the owning VM, but creating the snapshot only takes a few seconds. Once it's done, you can power on the VM and use the snapshot to create a duplicate VM to troubleshoot a production issue or roll back the VM to the point in time that the snapshot was taken.
- **Backup support**: You can automatically back up managed disks to different regions for disaster recovery with Azure Backup, all without affecting the service of the VM.

## Network communication

Virtual machines communicate with external resources using a virtual network (VNet). The VNet represents a private network in a single region on which your resources communicate. A virtual network is just like the networks you manage on-premises. You can divide them up with subnets to isolate resources, connect them to other networks (including your on-premises networks), and apply traffic rules to govern inbound and outbound connections.

### Plan your network

When you create a new VM, you'll have the option of creating a new virtual network or using an existing VNet in your region.

Having Azure create the network together with the VM is simple, but it's likely not ideal for most scenarios. It's better to plan your network requirements _up front_ for all the components in your architecture and create the VNet structure you'll need separately and then create the VMs and place them into the already-created VNets.

We'll look more at virtual networks a bit later in this module. Let's apply some of this knowledge and create a VM in Azure.


# Exercise - Create a Windows virtual machine

Your company processes video content on Windows VMs. A new city has contracted with your company to process their traffic cameras, but it's a model with which you haven't worked before. You need to create a new Windows VM and install some proprietary codecs in order to process and analyze the new video content.

## Create a new Windows virtual machine

You can create Windows VMs with the Azure portal, Azure CLI, or Azure PowerShell. The best approach is to use the portal, because the **Create a virtual machine** wizard collects all the required information and provides hints and validation messages throughout the process.

1. Sign in to the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com) using the same account you used to activate the sandbox.
    
2. On the Azure portal, under **Azure services**, select **Create a resource**. The **Create a resource** pane appears.
    
3. In _Search services and marketplace_ search box, search for _Windows Server_ and press Enter. Select **Windows Server** by Microsoft. The **Windows Server** pane appears.
    
4. There are several Windows Server options to choose from to create your VM. In the **Plan** dropdown list, scroll down, and select **[smalldisk] Windows Server 2019 Datacenter**.
    
5. Select **Create**. The **Create a virtual machine** pane appears.
    

## Configure the VM settings

Azure presents a _wizard_ as a series of tabs to walk you through all the configuration details for creating the VM. The first tab is **Basics**. You can select **Next** or **Previous** to move from one tab to another, or you can select any tab in the horizontal menu to move to a customizable configuration section.

![Screenshot showing **Basics** tab of the **Create a virtual machine** pane.](https://learn.microsoft.com/en-us/training/modules/create-windows-virtual-machine-in-azure/media/3-azure-portal-create-vm.png)

### Configure basic VM settings

 Note

As you add or change settings in the wizard, Azure validates each value and places a green check mark next to a validated field, or red error indicator below the field. You can hover over an error indicator to get more information about a validation issue.

 Note

It's a best practice to use a standard naming convention for resource names so you can easily identify their purpose. Windows VM names are a bit limited; they must be between 1 and 15 characters, cannot contain non-ASCII or special characters, and must be unique in the current resource group.

1. On the **Basics** tab, enter the following values for each setting.
    

|Setting|Value|
|---|---|
|**Project details**||
|Subscription|Concierge Subscription (the subscription that should be billed for VM hours).|
|Resource Group|Select _**[sandbox resource group name]**_.|
|**Instance details**||
|Virtual machine name|Enter a name for your VM, such as **test-vp-vm2** (for Test Video Processor VM #2).|
|Region|Select a region close to you from the global regions listed in the following table.|
|Availability options|Accept default **No infrastructure redundancy required**. This option is used to ensure the VM is highly available by grouping multiple VMs together to deal with planned or unplanned maintenance events or outages.|
|Security type|Standard|
|Image|Select **[smalldisk] Windows Server 2019 Datacenter - x64 Gen2** from the dropdown list.|
|VM architecture|Accept default (x64)|
|Run with Azure Spot discount|Accept default (unchecked).|
|Size|The **Size** field isn't directly editable. Select or accept the default **Standard DS1 v2**, which will give the VM 1 CPU and 3.5 GB of memory. Optionally, select the field to view recommended or recently chosen sizes; select **See all sizes** to explore filters for sizes based on vCPUs, RAM, Data disks, operations per second, and cost. Select the X in the top right of the pane to close the pane.|
|**Administrator account**||
|Username|Enter a username you'll use to sign in to the VM.|
|Password|Enter a password that's at least 12 characters long and has at least three of the following four characteristics: one lower case character, one uppercase character, one number, and one special character that isn't '\' or '-'. Use something you'll remember or write it down, as you'll need it later.|
|Confirm password|Confirm your password.|
|**Inbound port rules**||
|Public inbound ports|Select **Allow selected ports**. We want to be able to access the desktop for this Windows VM using RDP.|
|Select inbound ports|Select **RDP (3389)** from the dropdown list. As the note in the UI indicates, we can also adjust the network ports after we create the VM.|
|**Licensing**||
|Would you like to use an existing Windows Server License|Leave unchecked|
|||

    The free sandbox allows you to create resources in a subset of the Azure global regions. Select a region from the following list when you create resources:
    
    - West US 2
    - South Central US
    - Central US
    - East US
    - West Europe
    
    - Southeast Asia
    - Japan East
    - Brazil South
    - Australia Southeast
    - Central India
    
2. Select **Next : Disks**.
    
     Tip
    
    You can use the horizonal scroll bar to slide the view to the left to get back to the VM settings, which had opened a new pane to the right.
    

## Configure disks for the VM

1. On the **Disks** tab, enter or select the following values for each setting.
    
|Setting|Value|
|---|---|
|**Disk options**||
|Encryption at host|Accept the default (unchecked)|
|OS disk size|Accept the default **Image default (30 GiB)**.|
|OS disk type|Accept the default **Premium SSD (locally redundant storage)**.|
|Delete with VM|Accept the default (checked)|
|Key management|Accept the default.|
|Enable Ultra Disk compatibility|Accept the default (unchecked)|
|**Data disks**||
|Select **Create and attach a new disk** link. The **Create a new disk** pane appears.|Accept all the default values for the following settings: _Name_; _Source type_; _Size_; _Key management_; and _Enable shared disk_. This is where you could use a snapshot, or Storage Blob, to create a VHD.|

1. Select **OK** to save the settings and close the pane.
    
    ![Screenshot showing the configure disks section for the VM.](https://learn.microsoft.com/en-us/training/modules/create-windows-virtual-machine-in-azure/media/3-configure-disks.png)
    
3. On the **Create a virtual machine** pane **Disks** tab, under **Data disks**, there should now be a new row showing the newly configured disk.
    
    ![Screenshot showing the newly added disk in the VM.](https://learn.microsoft.com/en-us/training/modules/create-windows-virtual-machine-in-azure/media/3-new-disk.png)
    

## Configure the network

1. Select **Next : Networking**.
    
    In a production system where other components are already in use, it would be important to use an _existing_ virtual network so that the VM can communicate with the other cloud services in the production solution. If no virtual network has been defined in this location, create it here and configure the:
    
    - **Subnet**: First subnet to subdivide the address space; it must fit within the defined address space. After the VNet is created, you can add more subnets.
    - **Public IP**: Overall IPV4 space available to this network.
2. On the **Networking** tab, let's change some of the settings. Under the input field for **Virtual network**, select **Create new**. The **Create virtual network** pane appears.
    
3. On the **Create virtual network** pane, enter the following values for each setting.
    

|Setting|Value|
|---|---|
|Name|Accept the default name.|
|**Address space**||
|_Address range_|In the row below the heading, enter `172.16.0.0/16` to give the address space a full range of addresses, then check the box next to the address you just entered. If another address range row exists, select the **Delete** icon to delete it.|
|**Subnets**||
|_Subnet name_|Enter _default_ in the first input field, then select the checkbox next to the name you just entered. If another row exists, select it to delete it.|
|_Address range_|In the empty input field, enter `172.16.1.0/24` to give the subnet 256 IP addresses of space.|

1. Select **OK** to save your settings and return to the **Create a virtual machine** pane.
    

 Note

By default, Azure will create a virtual network, network interface, and public IP for your VM. It's not trivial to change networking options after the VM is created, so always double-check the network assignments for services you create in Azure.

## Finish configuring the VM and create the image

On the **Create a virtual machine** pane, the rest of the tabs have reasonable defaults and there's no need to change any of them. You can explore the other tabs if you like. Each field has an **(i)** icon next to it which, if selected, will show a detailed definition of that configuration setting. Reviewing field descriptions is a great way to learn about the settings you can use to configure the VM.

1. Select **Review + create**. The system will validate your options and display details about the VM being created.
    
2. Select **Create** to deploy the VM. The Azure dashboard will show the name of the VM that's being deployed and details about your deployment. Deployment may take several minutes.
    
3. After deployment completes, select **Go to resource**. Your virtual machine pane appears.
    

Now, let's look at what we can do with this VM.

# Use RDP to connect to Windows Azure virtual machines

Now that you have a Windows VM in Azure, the next thing you’ll do is put your applications and data on those VMs to process our traffic videos.

However, unless you’ve set up a site-to-site VPN to Azure, your Azure VMs won’t be accessible from your local network. If you’re just getting started with Azure, it’s unlikely that you have a working site-to-site VPN, so how can you transfer files to Azure VMs? One easy way is to use Azure’s Remote Desktop Connections feature to share your local drives with your new Azure VMs.

Now that we have a new Windows virtual machine, we need to install our custom software onto it. There are several options to choose from:

- Remote Desktop Protocol (RDP)
- Custom scripts
- Custom VM images (with the software preinstalled)

Let's look at the simplest approach for Windows VMs: Remote Desktop.

## What is the Remote Desktop Protocol?

Remote Desktop Protocol (RDP) provides remote connectivity to the UI of Windows-based computers. RDP lets you sign in to a remote physical or virtual Windows computer and control that computer as if you were seated at the console. An RDP connection allows you to carry out most operations that you can do from a physical computer's console, except for some power and hardware-related functions.

An RDP connection requires an RDP client. Microsoft provides RDP clients for the following operating systems:

- Windows (built-in)
- macOS
- iOS
- Android

The following screenshot displays the Remote Desktop Protocol client in Windows 10.

![Screenshot of the user interface of the Remote Desktop Protocol client.](https://learn.microsoft.com/en-us/training/modules/create-windows-virtual-machine-in-azure/media/4-rdp-client.png)

There are also open-source Linux clients, such as Remmina, that allow you to connect to a Windows PC from an Ubuntu distribution.

## Connecting to an Azure VM

As we learned a moment ago, Azure VMs communicate on a virtual network. They can also have an optional public IP address assigned to them. With a public IP, we can communicate with the VM over the Internet. Alternatively, we can set up a virtual private network (VPN) that connects our on-premises network to Azure, letting us securely connect to the VM without exposing a public IP. This approach is covered in another module, and is fully documented if you're interested in exploring that option.

One thing to be aware of with public IP addresses in Azure is they're often dynamically allocated. That means the IP address can change over time; for VMs, this happens when the VM is restarted. You can pay more to assign static addresses if you want to connect directly to an IP address instead of a name and need to ensure that the IP address won't change.

### How do you connect to a VM in Azure using RDP?

Connecting to a VM in Azure using RDP is a simple process. In the Azure portal, you'll go to your VM's properties, and at the top, select **Connect**. This shows you the IP addresses assigned to the VM and give you the option to download a **preconfigured.rdp** file that Windows then opens in the RDP client. You can choose to connect over the public IP address of the VM in the RDP file. Instead, if you're connecting over VPN or ExpressRoute, you can select the internal IP address. You can also select the port number for the connection.

If you're using a static public IP address for the VM, you can save the **.rdp** file to your desktop. If you're using dynamic IP addressing, the **.rdp** file only remains valid while the VM is running. If you stop and restart the VM, you must download another **.rdp** file.

 Tip

You can also enter the public IP address of the VM into the Windows RDP client and select **Connect**.

When you connect, you'll typically receive two warnings. These are:

- **Publisher warning**: caused by the **.rdp** file not being publicly signed
- **Certificate warning**: caused by the machine certificate not being trusted

In test environments, you can ignore these warnings. In production environments, the **.rdp** file can be signed using **RDPSIGN.EXE** and the machine certificate placed in the client's **Trusted Root Certification Authorities** store.

Let's try using RDP to connect to our VM.

# Exercise - Connect to a Windows virtual machine using RDP

We have our Windows VM deployed and running, but it's not configured to do any work.

Recall that our scenario is a video-processing system. Our platform receives files through FTP. The traffic cameras upload video clips to a known URL, which is mapped to a folder on the server. The custom software on each Windows VM runs as a service and watches the folder and processes each uploaded clip. It then passes the normalized video to our algorithms running on other Azure services.

There are a few things we'd need to configure to support this scenario:

- Install FTP and open the ports it needs to communicate
- Install the proprietary video codec unique to the city's camera system
- Install our transcoding service that processes uploaded videos

Many of these are typical administrative tasks we won't actually cover here, and we don't have software to install. Instead, we'll walk through the steps and show you how you _could_ install custom or third-party software using Remote Desktop. Let's start by getting the connection information.

## Connect to the VM with Remote Desktop Protocol

To connect to an Azure VM with an RDP client, you'll need:

- Public IP address of the VM (or private if the VM is configured to connect to your network)
- Port number

You can enter this information into the RDP client, or download a pre-configured **RDP** file.

 Note

An **RDP** file is a text file that contains a set of name/value pairs that define the connection parameters for an RDP client to connect to a remote computer using the Remote Desktop Protocol.

### Download the RDP file

1. In the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com), ensure the **Overview** pane for the virtual machine that you created earlier is open. You can also find the VM on the Azure **Home** page under **All Resources** if you need to open it. The **Overview** pane has a lot of information about the VM. You can:
    
    - Determine whether the VM is running
    - Stop or restart it
    - Get the public IP address to connect to the VM
    - Get the activity of the CPU, disk, and network
2. In the top menu bar, select **Connect**, then select **Connect** in the drop-down.
    
3. Note the **IP address** and **Port number** settings, then select **Download RDP File** and save it to your computer.
    
4. Before we connect, let's adjust a few settings. On Windows, find the file using Explorer, right-click it, and select **Edit** (you might need to select **Show more options** to find the **Edit** option). On macOS, you'll need to open the file first with the RDP client, then right-click on the item in the displayed list and select **Edit**.
    
5. You can adjust a variety of settings to control the experience in connecting to the Azure VM. The settings you'll want to examine are:
    
    - **Display**: By default, it'll be full screen. You can change this to a lower resolution, or use all your monitors if you have more than one.
    - **Local Resources**: You can share local drives with the VM, allowing you to copy files from your PC to the VM. Select the **More** button under **Local devices and resources** to select what is shared.
    - **Experience**: Adjust the visual experience based on your network quality.
6. Share your Local C: drive so it will be visible to the VM.
    
7. Switch back to the **General** tab and select **Save** to save the changes. You can always come back and edit this file later to try other settings.
    

### Connect to the Windows VM

1. Select **Connect**.
    
2. On the **Remote Desktop Connection** dialog box, note the security warning and the remote computer IP address, then select **Connect** to start the connection to the VM.
    
3. In the **Windows Security** dialog box, enter your username and password that you created in the previous exercise.
    
     Note
    
    If you're using a Windows client to connect to the VM, it will default to known identities on your machine. Select the **More choices** option, and then select **Use a different account** that lets you enter a different username/password combination.
    
4. In the second **Remote Desktop Connection** dialog box, note the certificate errors, then select **Yes**.
    

### Install worker roles

The first time you connect to a Windows server VM, it launches Server Manager. This allows you to assign a worker role for common web or data tasks. You can also launch the Server Manager through the **Start** menu.

This is where we'd add the Web Server role to the server. This installs IIS, and as part of the configuration, you'd turn off HTTP requests and enable the FTP server. We could also ignore IIS and install a third-party FTP server. We'd then configure the FTP server to allow access to a folder on our big data drive we added to the VM.

Because we aren't going to actually configure that here, just close Server Manager.

## Install custom software

We have two approaches we can use to install software. First, this VM is connected to the internet. If the software you need has a downloadable installer, you can open a web browser in the RDP session, download the software, and install it. Second, if your software is custom, like our custom service, you can copy it from your local machine over to the VM to install it. Let's look at this latter approach.

1. Open File Explorer. In the sidebar, select **This PC**. You should see several drives:
    
    - Windows (C:) drive representing the OS
    - Temporary Storage (D:) drive
    - Your local C: drive (it will have a different name than the following screenshot)
    
    ![Screenshot showing the local drive shared with the Azure VM.](https://learn.microsoft.com/en-us/training/modules/create-windows-virtual-machine-in-azure/media/6-drive-list.png)
    

With access to your local drive, you can copy the files for the custom software onto the VM and install the software. We won't actually do that because it's just a simulated scenario, but you can imagine how it would work.

The more interesting thing to observe in the list of drives is what is _missing_. Notice that our **Data** drive is not present. Azure added a VHD, but didn't initialize it.

## Initialize data disks

Any additional drives you create from scratch will need to be initialized and formatted. The process for doing this is identical to a physical drive.

1. Launch the Disk Management tool from the **Start** menu. You might have to go to the **Computer Management** tool first, then **Disk Management**, or try searching for _Disk Management_ in the **Start** Menu.
    
2. The Disk Management tool will display a warning that it has detected an uninitialized disk.
    
    ![Screenshot showing the Disk Management tool warning about an uninitialized data disk in the VM.](https://learn.microsoft.com/en-us/training/modules/create-windows-virtual-machine-in-azure/media/6-disk-management.png)
    
3. Select **OK** to initialize the disk. It'll then appear in the list of volumes, where you can format it and assign a drive letter.
    
4. Open File Explorer, and you should now have your data drive.
    
5. Go ahead and close the RDP client to disconnect from the VM. The server will continue to run.
    

RDP allows you to work with the Azure VM just like a local computer. With Desktop UI access, you can administer this VM as you would any Windows computer; installing software, configuring roles, adjusting features and other common tasks. However, it's a manual process. If you always need to install some software, you might consider automating the process using scripting.

# Configure Azure virtual machine network settings

We've installed our custom software, set up an FTP server, and configured the VM to receive our video files. However, if we try to connect to our public IP address with FTP, we'll find that it's blocked.

Making adjustments to server configuration is commonly performed with equipment in your on-premises environment. In this sense, you can consider Azure VMs to be an extension of that environment. You can make configuration changes, manage networks, open or block traffic, and more through the Azure portal, Azure CLI, or Azure PowerShell tools.

You've already seen some of the basic information and management options in the **Overview** panel for the virtual machine. Let's explore network configuration a bit more.

## Open ports in Azure VMs

By default, new VMs are locked down.

Apps can make outgoing requests, but the only inbound traffic allowed is from the virtual network (for example, other resources on the same local network), and from Azure's Load Balancer (probe checks).

There are two steps to adjusting the configuration to support FTP. When you create a new VM, you have an opportunity to open a few common ports (RDP, HTTP, HTTPS, and SSH). However, if you require other changes to the firewall, you will need to make them yourself.

The process for this involves two steps:

1. Create a Network Security Group.
2. Create an inbound rule allowing traffic on port 20 and 21 for active FTP support.

### What is a Network Security Group?

Virtual networks (VNets) are the foundation of the Azure networking model, and provide isolation and protection. Network Security Groups (NSGs) are the main tool you use to enforce and control network traffic rules at the networking level. NSGs are an optional security layer that provides a software firewall by filtering inbound and outbound traffic on the VNet.

Security groups can be associated to a network interface (for per-host rules), a subnet in the virtual network (to apply to multiple resources), or both levels.

#### Security group rules

NSGs use _rules_ to allow or deny traffic moving through the network. Each rule identifies the source and destination address (or range), protocol, port (or range), direction (inbound or outbound), a numeric priority, and whether to allow or deny the traffic that matches the rule. The following illustration shows NSG rules applied at the subnet and network-interface levels.

![Illustration showing the architecture of network security groups in two different subnets. In one subnet, there are two virtual machines, each with their own network interface rules. The subnet itself has a set of rules that applies to both the virtual machines.](https://learn.microsoft.com/en-us/training/modules/create-windows-virtual-machine-in-azure/media/7-nsg-rules.png)

Each security group has a set of default security rules to apply the default network rules described in the preceding passage. You can't modify these default rules, but you _can_ override them.

#### How Azure uses network rules

For inbound traffic, Azure processes the security group associated to the subnet, then the security group applied to the network interface. Outbound traffic is processed in the opposite order (the network interface first, followed by the subnet).

 Warning

Keep in mind that security groups are optional at both levels. If no security group is applied, then **all traffic is allowed** by Azure. If the VM has a public IP, this could be a serious risk, particularly if the OS doesn't provide some sort of firewall.

The rules are evaluated in _priority order_, starting with the **lowest priority** rule. Deny rules always **stop** the evaluation. For example, if an outbound request is blocked by a network interface rule, any rules applied to the subnet will not be checked. In order for traffic to be allowed through the security group, it must pass through _all_ applied groups.

The last rule is always a **Deny All** rule. This is a default rule added to every security group for both inbound and outbound traffic with a priority of 65500. That means to have traffic pass through the security group, _you must have an allow rule_ or the default final rule will block it. [Learn more about security rules](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview#security-rules).

 Note

SMTP (port 25) is a special case. Depending on your subscription level and when your account was created, outbound SMTP traffic might be blocked. You can make a request to remove this restriction with business justification.

---

# Host a web application with Azure App Service

# Introduction

Imagine you're building a website for a new business, or you're running an existing web app on an aging on-premises server. Setting up a new server can be challenging. You need the appropriate hardware, likely a server-level operating system, and a web-hosting stack.

Once it's running, you need to maintain the server. And what happens if your website traffic increases? You might need to invest in more hardware.

Hosting your web application using Azure App Service makes deploying and managing a web app easier when compared to managing a physical server. In this module, we implement and deploy a web app to App Service.

## Learning objectives

In this module, you'll:

- Use the Azure portal to create an Azure App Service web app.
- Use developer tools to create the code for a starter web application.
- Deploy your code to App Service.

## Prerequisites

- Ability to navigate the Azure portal
- Ability to use a command-line interface

# Create a web app in the Azure portal

In this unit, you learn how to create an Azure App Service web app using the Azure portal.

## Why use the Azure portal?

The first step in hosting your web application is to create a web app (an Azure App Service app) inside your Azure subscription.

There are several ways you can create a web app. You can use the Azure portal, the Azure Command Line Interface (CLI), a script, or an integrated development environment (IDE) like Visual Studio.

The information in this unit discusses how to use the Azure portal to create a web app, and you use this information to create a web app in the next exercise. For this module, we demonstrate using the Azure portal because it's a graphical experience, which makes it a great learning tool. The portal helps you discover available features, add other resources, and customize existing resources.

## What is Azure App Service?

Azure App Service is a fully managed web application hosting platform. This platform as a service (PaaS) offered by Azure allows you to focus on designing and building your app while Azure takes care of the infrastructure to run and scale your applications.

### Deployment slots

Using the Azure portal, you can easily add **deployment slots** to an App Service web app. For instance, you can create a **staging** deployment slot where you can push your code to test on Azure. Once you're happy with your code, you can easily **swap** the staging deployment slot with the production slot. All it takes is a few mouse clicks in the Azure portal.

![Screenshot of the staging deployment slot to test the deployments.](https://learn.microsoft.com/en-us/training/modules/host-a-web-app-with-azure-app-service/media/2-deployment-slots.png)

### Continuous integration/deployment support

The Azure portal provides out-of-the-box continuous integration and deployment with Azure Repos, GitHub, Bitbucket, FTP, or a local Git repository on your development machine. You can connect your web app with any of the preceding sources, and App Service does the rest for you. It automatically syncs your code and any future changes on the code into the web app. Furthermore, with Azure Repos, you can define your own build and release process. A full process that compiles your source code, runs the tests, builds a release, and finally deploys the release into your web app every time you commit the code. All that happens implicitly, without any need for you to intervene.

![Screenshot of setting up deployment options and choosing source for the deployment source code.](https://learn.microsoft.com/en-us/training/modules/host-a-web-app-with-azure-app-service/media/2-continuous-integration.png)

### Integrated Visual Studio publishing and FTP publishing

In addition to being able to set up continuous integration/deployment for your web app, you can always benefit from the tight integration with Visual Studio to publish your web app to Azure via Web Deploy technology. App Service also supports FTP-based publishing for more traditional workflows.

### Built-in autoscale support (automatic scale-out based on real-world load)

The ability to scale up/down or scale out is baked into the web app. Depending on the web app's usage, you can scale your app up/down by increasing/decreasing the resources of the underlying machine that's hosting your web app. Resources can be the number of cores or the amount of RAM available. On the other hand, you can scale out your app by increasing the number of machine instances that are running your web app.

## Creating a web app

When you're ready to run a web app on Azure, you can visit the Azure portal and create a **Web App** resource. Creating a web app allocates a set of hosting resources in App Service. You can use these resources to host any web-based application Azure supports, whether it's ASP.NET Core, Node.js, Java, Python, and so on.

The Azure portal provides a wizard to create a web app. This wizard requires the following fields:

|_Field_|_Description_|
|---|---|
|**Subscription**|A valid and active Azure subscription.|
|**Resource group**|A valid resource group.|
|**Name**|The name of the web app. This name becomes part of the app's URL, so it must be unique among all Azure App Service web apps.|
|**Publish**|You can deploy your application to App Service as **code** or as a ready-to-run Docker **Container**. Selecting **Container** activates the wizard's Container tab, where you provide information about the Docker registry from which App Service retrieves your image.|
|**Runtime stack**|If you choose to deploy your application as code, App Service needs to know what runtime your application uses (examples include Node.js, Python, Java, and .NET). If you deploy your application as a container, you don't need to choose a runtime stack, because your image includes it.|
|**Operating system**|App Service can host applications on **Windows** or **Linux** servers. For more information, see the _Operating systems_ section in this unit.|
|**Region**|The Azure region from which your application is served.|
|**Pricing Plans**|See the _Pricing Plans_ section in this unit for information about App Service plans.|

### Operating systems

If you're deploying your app as code, many of the available runtime stacks are limited to one operating system or the other. After you choose a runtime stack, the toggle will indicate whether or not you have a choice of operating system. If your target runtime stack is available on both operating systems, select the one that you use to develop and test your application.

If your application is packaged as a container, specify the operating system in your container.

### App Service plans

An **App Service** plan is a set of virtual server resources that run App Service apps. A plan's **size** (sometimes referred to as its **sku** or **pricing tier**) determines the performance characteristics of the virtual servers that run the apps assigned to the plan, and the App Service features to which those apps have access. Every App Service web app you create must be assigned to a single App Service plan that runs it.

A single App Service plan can host multiple App Service web apps. In most cases, the number of apps you can run on a single plan is limited by the apps' performance characteristics and the plan's resource limitations.

App Service plans determine the App Service's unit of billing. The size of each App Service plan in your subscription, in addition to the bandwidth resources the apps deployed to those plans use, determines the price you pay. The number of web apps deployed to your App Service plans has no effect on your bill.

You can use any of the available Azure management tools to create an App Service plan. When you create a web app via the Azure portal, the wizard helps you to create a new plan at the same time if you don't already have one.

# Exercise - Create a web app in the Azure portal

Choose your development language

C#JavaNode.jsPython

In this unit, you use the Azure portal to create a web app.

## Create a web app

Sign in to the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com) using the same account you used to activate the sandbox.

1. On the Azure portal menu or from the **Home** page, select **Create a resource**. Everything you create on Azure is a resource. The **Create a resource** pane appears.
    
    Here, you can search for the resource you want to create, or select one of the popular resources that people create in the Azure portal.
    
2. In the **Create a resource** menu, select **Web**.
    
3. Select **Web App**. If you don't see it, in the search box, search for and select **Web App**. The **Create Web App** pane appears.
    
4. On the **Basics** tab, enter the following values for each setting.
    

|Setting|Value|Details|
|---|---|---|
|**Project Details**|||
|Subscription|Concierge Subscription|The web app you're creating must belong to a resource group. Here, you select the Azure subscription to which the resource group belongs (or is going to belong, if you're creating it within the wizard).|
|Resource Group|Select [Sandbox resource group]|The resource group to which the web app belongs. All Azure resources must belong to a resource group.|
|**Instance Details**|||
|Name|_Enter a unique name_|The name of your web app. This name becomes part of the app's URL: _appname_.azurewebsites.net. The name you choose must be unique among all Azure web apps.|
|Publish|Code|The method you want to use to publish your application. When publishing an application as code, you must also configure **Runtime stack** to prepare App Service resources to run your app.|
|Runtime stack|.NET 8 (LTS)|The platform on which your application is going to run. Your choice might affect whether you have a choice of operating system - for some runtime stacks, App Service supports only one operating system.|
|Operating System|Linux|The operating system used on the virtual servers to run your app.|
|Region|Central US|The geographical region from which your app is hosted.|
|**Pricing plans**|||
|Linux Plan|Accept default|The name of the App Service plan that powers your app. By default, the wizard creates a new plan in the same region as the web app.|
|Pricing plan|Free F1|The pricing tier of the service plan being created. The pricing plan determines the performance characteristics of the virtual servers that power your app and the features to which it has access. Select **Free F1** in the drop-down.|

![Screenshot showing web app creation details.](https://learn.microsoft.com/en-us/training/modules/host-a-web-app-with-azure-app-service/media/3-create-web-app-dotnet.png)


5. Leave any other settings as default. Select **Review + Create** to go to the review pane, and then select **Create**. The portal shows the deployment pane, where you can view the status of your deployment.
    
     Note
    
    It can take a moment for deployment to complete.
    

## Preview your web app

1. When deployment is complete, select **Go to resource**. The portal shows the App Service Overview pane for your web app.
    
    ![Screenshot showing the App Service pane with the URL link of the overview section highlighted.](https://learn.microsoft.com/en-us/training/modules/host-a-web-app-with-azure-app-service/media/3-web-app-home.png)
    
2. To preview your web app's default content, select the URL under **Default domain** at the top right. The placeholder page that loads indicates that your web app is up and running and is ready to receive deployment of your app's code.
    

![Screenshot showing your App Service in a browser.](https://learn.microsoft.com/en-us/training/modules/host-a-web-app-with-azure-app-service/media/3-web-app-online-dotnet.png)

Leave the browser tab with the new app's placeholder page open. You'll come back to it after your app is deployed.

# Prepare the web application code

Choose your development language

C#JavaNode.jsPython

In this unit, you learn how to create the code for your web application and integrate it into a source-control repository.

## Bootstrap a web application

Now that you created the resources for deploying your web application, you have to prepare the code you want to deploy. There are many ways to bootstrap a new web application, so what we learn here might be different to what you're used to. The goal is to quickly provide you a starting point to complete a full cycle up to the deployment.

 Note

All the code and commands shown on this page are only for explanation purposes; **you do not need to execute any of them**. We use them in a subsequent exercise.

The `dotnet` command-line tool that's part of the .NET SDK allows you to directly create the code for a new web application. In particular, you can use the `dotnet new` command to generate a new application from a template:

BashCopy

```
dotnet new mvc --name <YourAppName>
```

This command creates a new ASP.NET Core Model-View Cotroller (MVC) application in a new folder with the specified name.

## Adding your code to source control

After your web application code is ready, the next step is usually to put the code into a source-control repository such as Git. If you have Git installed on your machine, running these commands in your source-code folder initializes the repository.

BashCopy

```
git init
git add .
git commit -m "Initial commit"
```

These commands allow you to initialize a local Git repository and create a first commit with your code. You immediately gain the benefit of keeping a history of your changes with commits. Later on, you're able to synchronize your local repository with a remote repository; for example, hosted on GitHub. This synchronization allows you to set up continuous integration and continuous deployment (CI/CD). While we recommend using a source-control repository for production applications, it's not a requirement to be able to deploy an application to Azure App Service.

 Note

Using CI/CD enables more frequent code deployment in a reliable manner by automating builds, tests, and deployments for every code change. It enables delivering new features and bug fixes for your application faster and more effectively.

# Exercise - Write code to implement a web application

Choose your development language

C#JavaNode.jsPython

In this unit, you use developer tools to create the code for a starter web application.

## Create a new web project

The heart of the .NET CLI tools is the `dotnet` command-line tool. Using this command, you create a new ASP.NET Core web project.

Run the following command to create a new ASP.NET Core Model-View Cotroller (MVC) application named "BestBikeApp":

BashCopy

```
dotnet new mvc --name BestBikeApp
```

The command creates a new folder named "BestBikeApp" to hold your project.

### Optionally test your web app

You can test your application locally on Azure. To do so, use the following steps:

1. Run the following commands to build and run your web application in the background:
    
    BashCopy
    
    ```
    cd BestBikeApp
    dotnet run &
    ```
    
    You should get output like the following example:
    
    ConsoleCopy
    
    ```
    [1] <process-number>
    <username> [ ~/BestBikeApp ]$ Building...
    warn: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[35]
          No XML encryptor configured. Key {b4a2970c-215c-4eb2-92b4-c28d021158c6} may be persisted to storage in unencrypted form.
    info: Microsoft.Hosting.Lifetime[14]
          Now listening on: http://localhost:<port>
    info: Microsoft.Hosting.Lifetime[0]
          Application started. Press Ctrl+C to shut down.
    info: Microsoft.Hosting.Lifetime[0]
          Hosting environment: Development
    info: Microsoft.Hosting.Lifetime[0]
          Content root path: /home/cephas_lin/BestBikeApp
    ```
    
    In the output, take note of the values of _<process-number/>_ and _<port/>_.
    
2. Run the following command to browse to your web application, replacing _<port/>_ with the port you noted in the last step.
    
    BashCopy
    
    ```
    curl -kL http://localhost:<port>/
    ```
    
    You should see some HTML appear, ending in the following lines:
    
    HTMLCopy
    
    ```
    <html>
    ...
    <div class="text-center">
        <h1 class="display-4">Welcome</h1>
        <p>Learn about <a href="https://learn.microsoft.com/aspnet/core">building Web apps with ASP.NET Core</a>.</p>
    </div>
    
            </main>
        </div>
    
        <footer b-b5g3qljvtd class="border-top footer text-muted">
            <div b-b5g3qljvtd class="container">
                &copy; 2024 - BestBikeApp - <a href="/Home/Privacy">Privacy</a>
            </div>
        </footer>
        <script src="/lib/jquery/dist/jquery.min.js"></script>
        <script src="/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
        <script src="/js/site.js?v=hRQyftXiu1lLX2P9Ly9xa4gHJgLeR1uGN5qegUobtGo"></script>
    
    </body>
    </html>
    ```
    
3. Using _<process-number/>_ that you noted earlier, stop dotnet:
    
    Azure CLICopy
    
    Open Cloud Shell
    
    ```
    kill <process-number/>
    ```

# Deploy code to App Service


Now, let's see how we can deploy our application to App Service.

## Automated deployment

Automated deployment, or continuous integration, is a process used to push out new features and bug fixes in a fast and repetitive pattern with minimal impact on end users.

Azure supports automated deployment directly from several sources. The following options are available:

- **Azure Repos**: You can push your code to Azure Repos, build your code in the cloud, run the tests, generate a release from the code, and finally push your code to an Azure Web App.
- **GitHub**: Azure supports automated deployment directly from GitHub. When you connect your GitHub repository to Azure for automated deployment, any changes you push to your production branch on GitHub are automatically deployed for you.
- **Bitbucket**: Due to its similarities to GitHub, you can configure an automated deployment with Bitbucket.

## Manual deployment

There are a few options that you can use to manually push your code to Azure:

- **Git**: App Service web apps feature a Git URL that you can add as a remote repository. Pushing to the remote repository deploys your app.
- _**az webapp up**_: `webapp up` is a feature of the `az` command-line interface that packages your app and deploys it. Unlike other deployment methods, `az webapp up` can create a new App Service web app for you if one isn't created.
- **Deploy application packages**: You can use `az webapp deploy` to deploy a ZIP, WAR, EAR, or JAR to App Service. You can also deploy scripts and static files with the same method.
- **Visual Studio**: Visual Studio features an App Service deployment wizard that walks you through the deployment process.
- **FTP/S**: FTP or FTPS is a traditional way of pushing your code to many hosting environments, including App Service.

# Exercise - Deploy your code to App Service


Choose your development language

C#JavaNode.jsPython

In this unit, you deploy your web application to App Service.

## Deploy with `az webapp deploy`

Let's deploy the .NET application with ZIP deploy.

First, use `dotnet publish` to build the final app files and `zip` to package them into a zip file:

BashCopy

```
cd ~/BestBikeApp
dotnet publish -o pub
cd pub
zip -r site.zip *
```

Finally, perform the deployment with `az webapp deploy`. Replace `<your-app-name>` in the following command with the name of your Azure web app and run it:

BashCopy

```
az webapp deploy \
    --src-path site.zip \
    --resource-group [sandbox resource group name] \
    --name <your-app-name>
```

The deployment takes a few minutes, during which time you get status output. When the command finishes running, you see an output message like this:

OutputCopy

```
Deployment has completed successfully
You can visit your app at: http://<app-name>-<hash>.<region>.azurewebsites.net
```

## Verify the deployment

In a new tab, navigate to the URL shown in the output. You get the splash page for a new ASP.NET Core web app.

![Screenshot of welcome page.](https://learn.microsoft.com/en-us/training/modules/host-a-web-app-with-azure-app-service/media/7-web-app-in-browser.png)

Congratulations, you successfully hosted your new ASP.NET Core application, on App Service!

