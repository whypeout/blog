---
draft: true
date: 2024-11-14
slug: Az 104 - Implement and Manage storage in azure
tags:
---

# Configure storage accounts

# Introduction

Azure Storage is Microsoft's cloud storage solution for modern data storage scenarios.

Suppose you work for a large e-commerce company that needs to store and serve a vast number of product images to its customers. The company wants a scalable and reliable solution that can handle high traffic and ensure data durability. They want to quickly restore data if there's an outage.

In this module, you learn how to configure storage accounts and select appropriate storage types in Azure. The module covers topics such as implementing replication strategies, and configuring secure access to storage.

The goal of this module is to provide Azure Administrators with the knowledge and skills to effectively configure and manage Azure storage accounts.

## Learning objectives

In this module, you learn how to:

- Identify features and usage cases for Azure storage accounts.
- Select between different types of Azure Storage and create storage accounts.
- Select a storage replication strategy.
- Configure secure network access to storage endpoints.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104).

## Prerequisites

- Experience with the Azure portal.
- Familiarity with managing different types of data storage.

# Implement Azure Storage

Azure Storage is Microsoft's cloud storage solution for modern data storage scenarios. Azure Storage offers a massively scalable object store for data objects. It provides a file system service for the cloud, a messaging store for reliable messaging, and a NoSQL store.

Azure Storage is a service that you can use to store files, messages, tables, and other types of information. You use Azure Storage for applications like file shares. Developers use Azure Storage for working data. Working data includes websites, mobile apps, and desktop applications. Azure Storage is also used by IaaS virtual machines, and PaaS cloud services.

### Things to know about Azure Storage

You can think of Azure Storage as supporting three categories of data: structured data, unstructured data, and virtual machine data. Review the following categories and think about which types of storage are used in your organization.

Expand table

|Category|Description|Storage examples|
|---|---|---|
|**Virtual machine data**|Virtual machine data storage includes disks and files. Disks are persistent block storage for Azure IaaS virtual machines. Files are fully managed file shares in the cloud.|Storage for virtual machine data is provided through Azure managed disks. Data disks are used by virtual machines to store data like database files, website static content, or custom application code. The number of data disks you can add depends on the virtual machine size. Each data disk has a maximum capacity of 32,767 GB.|
|**Unstructured data**|Unstructured data is the least organized. Unstructured data may not have a clear relationship. The format of unstructured data is referred to as _nonrelational_.|Unstructured data can be stored by using Azure Blob Storage and Azure Data Lake Storage. Blob Storage is a highly scalable, REST-based cloud object store. Azure Data Lake Storage is the Hadoop Distributed File System (HDFS) as a service.|
|**Structured data**|Structured data is stored in a relational format that has a shared schema. Structured data is often contained in a database table with rows, columns, and keys. Tables are an autoscaling NoSQL store.|Structured data can be stored by using Azure Table Storage, Azure Cosmos DB, and Azure SQL Database. Azure Cosmos DB is a globally distributed database service. Azure SQL Database is a fully managed database-as-a-service built on SQL.|

### How to create a storage account

#### Storage account types

General purpose Azure storage accounts have two types: Standard and Premium.

- **Standard** storage accounts are backed by magnetic hard disk drives (HDD). A standard storage account provides the lowest cost per GB. You can use Standard storage for applications that require bulk storage or where data is infrequently accessed.
    
- **Premium** storage accounts are backed by solid-state drives (SSD) and offer consistent low-latency performance. You can use Premium storage for Azure virtual machine disks with I/O-intensive applications like databases.
    

 Note

You can't convert a Standard storage account to a Premium storage account or vice versa. You must create a new storage account with the desired type and copy data, if applicable, to a new storage account.

### Things to consider when using Azure Storage

As you think about your configuration plan for Azure Storage, consider these prominent features.

- **Consider durability and availability**. Azure Storage is durable and highly available. Redundancy ensures your data is safe during transient hardware failures. You replicate data across datacenters or geographical regions for protection from local catastrophe or natural disaster. Replicated data remains highly available during an unexpected outage.
    
- **Consider secure access**. Azure Storage encrypts all data. Azure Storage provides you with fine-grained control over who has access to your data.
    
- **Consider scalability**. Azure Storage is designed to be massively scalable to meet the data storage and performance needs of modern applications.
    
- **Consider manageability**. Microsoft Azure handles hardware maintenance, updates, and critical issues for you.
    
- **Consider data accessibility**. Data in Azure Storage is accessible from anywhere in the world over HTTP or HTTPS. Microsoft provides SDKs for Azure Storage in various languages. You can use .NET, Java, Node.js, Python, PHP, Ruby, Go, and the REST API. Azure Storage supports scripting in Azure PowerShell or the Azure CLI. The Azure portal and Azure Storage Explorer offer easy visual solutions for working with your data.

# Explore Azure Storage services

Azure Storage offers four data services that can be accessed by using an Azure storage account:

- **Azure Blob Storage (containers)**: A massively scalable object store for text and binary data.
    
- **Azure Files**: Managed file shares for cloud or on-premises deployments.
    
- **Azure Queue Storage**: A messaging store for reliable messaging between application components.
    
- **Azure Table Storage**: A service that stores nonrelational structured data (also known as structured NoSQL data).
    

Let's examine the details of these services.

### Azure Blob Storage (containers)

Azure Blob Storage is Microsoft's object storage solution for the cloud. Blob Storage is optimized for storing massive amounts of unstructured or _nonrelational_ data, such as text or binary data. Blob Storage is ideal for:

- Serving images or documents directly to a browser.
- Storing files for distributed access.
- Streaming video and audio.
- Storing data for backup and restore, disaster recovery, and archiving.
- Storing data for analysis by an on-premises or Azure-hosted service.

Objects in Blob Storage can be accessed from anywhere in the world via HTTP or HTTPS. Users or client applications can access blobs via URLs, the Azure Storage REST API, Azure PowerShell, the Azure CLI, or an Azure Storage client library. The storage client libraries are available for multiple languages, including .NET, Java, Node.js, Python, PHP, and Ruby.

 Note

You can access data from Azure Blob Storage [by using the NFS protocol](https://learn.microsoft.com/en-us/training/modules/access-data-azure-blob-storage-multiple-protocols/4-access-data-azure-blob-storage-nfs-protocol).

### Azure Files

Azure Files enables you to set up highly available network file shares. Shares can be accessed by using the Server Message Block (SMB) protocol and the Network File System (NFS) protocol. Multiple virtual machines can share the same files with both read and write access. You can also read the files by using the REST interface or the storage client libraries.

File shares can be used for many common scenarios:

- Many on-premises applications use file shares. This feature makes it easier to migrate those applications that share data to Azure. If you mount the file share to the same drive letter that the on-premises application uses, the part of your application that accesses the file share should work with minimal, if any, changes.
- Configuration files can be stored on a file share and accessed from multiple virtual machines. Tools and utilities used by multiple developers in a group can be stored on a file share, ensuring that everybody can find them, and that they use the same version.
- Diagnostic logs, metrics, and crash dumps are just three examples of data that can be written to a file share and processed or analyzed later.

The storage account credentials are used to provide authentication for access to the file share. All users who have the share mounted should have full read/write access to the share.

### Azure Queue Storage

Azure Queue Storage is used to store and retrieve messages. Queue messages can be up to 64 KB in size, and a queue can contain millions of messages. Queues are used to store lists of messages to be processed asynchronously.

Consider a scenario where you want your customers to be able to upload pictures, and you want to create thumbnails for each picture. You could have your customer wait for you to create the thumbnails while uploading the pictures. An alternative is to use a queue. When the customer finishes the upload, you can write a message to the queue. Then you can use an Azure Function to retrieve the message from the queue and create the thumbnails. Each of the processing parts can be scaled separately, which gives you more control when tuning the configuration.

### Azure Table Storage

Azure Table storage is a service that stores non-relational structured data (also known as structured NoSQL data) in the cloud, providing a key/attribute store with a schemaless design. Because Table storage is schemaless, it's easy to adapt your data as the needs of your application evolve. Access to Table storage data is fast and cost-effective for many types of applications, and is typically lower in cost than traditional SQL for similar volumes of data. In addition to the existing Azure Table Storage service, there's a new Azure Cosmos DB Table API offering that provides throughput-optimized tables, global distribution, and automatic secondary indexes.

### Things to consider when choosing Azure Storage services

As you think about your configuration plan for Azure Storage, consider the prominent features of the types of Azure Storage and which options support your application needs.

- **Consider storage optimization for massive data**. Azure Blob Storage is optimized for storing massive amounts of unstructured data. Objects in Blob Storage can be accessed from anywhere in the world via HTTP or HTTPS. Blob Storage is ideal for serving data directly to a browser, streaming data, and storing data for backup and restore.
    
- **Consider storage with high availability**. Azure Files supports highly available network file shares. On-premises apps use file shares for easy migration. By using Azure Files, all users can access shared data and tools. Storage account credentials provide file share authentication to ensure all users who have the file share mounted have the correct read/write access.
    
- **Consider storage for messages**. Use Azure Queue Storage to store large numbers of messages. Queue Storage is commonly used to create a backlog of work to process asynchronously.
    
- **Consider storage for structured data**. Azure Table Storage is ideal for storing structured, nonrelational data. It provides throughput-optimized tables, global distribution, and automatic secondary indexes. Because Azure Table Storage is part of Azure Cosmos DB, you have access to a fully managed NoSQL database service for modern app development.

# Determine storage account types

Azure Storage offers several storage account options. Each [storage account](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-overview) supports different features and has its own pricing model.

### Things to know about storage account types

Review the following options and think about what storage accounts are required to support your applications.


|Storage account|Supported services|Recommended usage|
|---|---|---|
|[**Standard** **general-purpose v2**](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-upgrade)|Blob Storage (including Data Lake Storage), Queue Storage, Table Storage, and Azure Files|Standard storage account for most scenarios, including blobs, file shares, queues, tables, and disks (page blobs).|
|[**Premium** **block blobs**](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-block-blob-premium)|Blob Storage (including Data Lake Storage)|Premium storage account for block blobs and append blobs. Recommended for applications with high transaction rates. Use Premium block blobs if you work with smaller objects or require consistently low storage latency. This storage is designed to scale with your applications.|
|[**Premium** **file shares**](https://learn.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share)|Azure Files|Premium storage account for file shares only. Recommended for enterprise or high-performance scale applications. Use Premium file shares if you require support for both Server Message Block (SMB) and NFS file shares.|
|[**Premium** **page blobs**](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-pageblob-overview)|Page blobs only|Premium high-performance storage account for page blobs only. Page blobs are ideal for storing index-based and sparse data structures, such as operating systems, data disks for virtual machines, and databases.|

 Note

All storage account types are encrypted by using Storage Service Encryption (SSE) for data at rest.

### How to manage your storage account

Learn more about [how to manage your storage account](https://www.youtube.com/embed/-j_8clCWYn8).

# Determine replication strategies

The data in your Azure storage account is always replicated to ensure durability and high availability. Azure Storage replication copies your data to protect from planned and unplanned events. These events range from transient hardware failures, network or power outages, massive natural disasters, and so on. You can choose to replicate your data within the same data center, across zonal data centers within the same region, and even across regions. Replication ensures your storage account meets the Service-Level Agreement (SLA) for Azure Storage even if there are failures.

We explore four replication strategies:

- Locally redundant storage (LRS)
- Zone redundant storage (ZRS)
- Geo-redundant storage (GRS)
- Geo-zone-redundant storage (GZRS)

### Locally redundant storage

Locally redundant storage is the lowest-cost replication option and offers the least durability compared to other strategies. If a data center-level disaster occurs, such as fire or flooding, all replicas might be lost or unrecoverable. Despite its limitations, LRS can be appropriate in several scenarios:

- Your application stores data that can be easily reconstructed if data loss occurs.
- Your data is constantly changing like in a live feed, and storing the data isn't essential.
- Your application is restricted to replicating data only within a country/region due to data governance requirements.

### Zone redundant storage

Zone redundant storage synchronously replicates your data across three storage clusters in a single region. Each storage cluster is physically separated from the others and resides in its own availability zone. Each availability zone, and the ZRS cluster within it, is autonomous, and has separate utilities and networking capabilities. Storing your data in a ZRS account ensures you can access and manage your data if a zone becomes unavailable. ZRS provides excellent performance and low latency.

- ZRS isn't currently available in all regions.
- Changing to ZRS from another data replication option requires the physical data movement from a single storage stamp to multiple stamps within a region.

### Geo-redundant storage

Geo-redundant storage replicates your data to a secondary region (hundreds of miles away from the primary location of the source data). GRS provides a higher level of durability even during a regional outage. GRS is designed to provide at least 99.99999999999999% **(16 9's) durability**. When your storage account has GRS enabled, your data is durable even when there's a complete regional outage or a disaster where the primary region isn't recoverable.

If you implement GRS, you have two related options to choose from:

- **GRS** replicates your data to another data center in a secondary region. The data is available to be read only if Microsoft initiates a failover from the primary to secondary region.
    
- **Read-access geo-redundant storage** (RA-GRS) is based on GRS. RA-GRS replicates your data to another data center in a secondary region, and also provides you with the option to read from the secondary region. With RA-GRS, you can read from the secondary region regardless of whether Microsoft initiates a failover from the primary to the secondary.
    

For a storage account with GRS or RA-GRS enabled, all data is first replicated with locally redundant storage. An update is first committed to the primary location and replicated by using LRS. The update is then replicated asynchronously to the secondary region by using GRS. Data in the secondary region uses LRS. Both the primary and secondary regions manage replicas across separate fault domains and upgrade domains within a storage scale unit. The storage scale unit is the basic replication unit within the datacenter. Replication at this level is provided by LRS.

### Geo-zone redundant storage

Geo-zone-redundant storage combines the high availability of zone-redundant storage with protection from regional outages as provided by geo-redundant storage. Data in a GZRS storage account is replicated across three Azure availability zones in the primary region, and also replicated to a secondary geographic region for protection from regional disasters. Each Azure region is paired with another region within the same geography, together making a regional pair.

With a GZRS storage account, you can continue to read and write data if an availability zone becomes unavailable or is unrecoverable. Additionally, your data is also durable during a complete regional outage or during a disaster in which the primary region isn't recoverable. GZRS is designed to provide at least 99.99999999999999% (16 9's) durability of objects over a given year. GZRS also offers the same scalability targets as LRS, ZRS, GRS, or RA-GRS. You can optionally enable read access to data in the secondary region with read-access geo-zone-redundant storage (RA-GZRS).

 Tip

Microsoft recommends using GZRS for applications that require consistency, durability, high availability, excellent performance, and resilience for disaster recovery. Enable RA-GZRS for read access to a secondary region when there's a regional disaster.

### Things to consider when choosing replication strategies

Let's examine the scope of durability and availability for the different replication strategies. The following table describes several key factors during the replication process, including node unavailability within a data center, and whether the entire data center (zonal or nonzonal) becomes unavailable. The table identifies read access to data in a remote, geo-replicated region during region-wide unavailability, and the supported Azure storage account types.

|Node in data center unavailable|Entire data center unavailable|Region-wide outage|Read access during region-wide outage|
|---|---|---|---|
|- **LRS**  <br>- **ZRS**  <br>- **GRS**  <br>- **RA-GRS**  <br>- **GZRS**  <br>- **RA-GZRS**|- **ZRS**  <br>- **GRS**  <br>- **RA-GRS**  <br>- **GZRS**  <br>- **RA-GZRS**|- **GRS**  <br>- **RA-GRS**  <br>- **GZRS**  <br>- **RA-GZRS**|- **RA-GRS**  <br>- **RA-GZRS**|

# Access storage

Every object you store in Azure Storage has a unique URL address. Your storage account name forms the _subdomain_ portion of the URL address. The combination of the subdomain and the domain name, which is specific to each service, forms an endpoint for your storage account.

Let's look at an example. If your storage account name is _mystorageaccount_, default endpoints for your storage account are formed for the Azure services as shown in the following table:

|Service|Default endpoint|
|---|---|
|**Container service**|`//`**`mystorageaccount`**`.blob.core.windows.net`|
|**Table service**|`//`**`mystorageaccount`**`.table.core.windows.net`|
|**Queue service**|`//`**`mystorageaccount`**`.queue.core.windows.net`|
|**File service**|`//`**`mystorageaccount`**`.file.core.windows.net`|

We create the URL to access an object in your storage account by appending the object's location in the storage account to the endpoint.

To access the _myblob_ data in the _mycontainer_ location in your storage account, we use the following URL address:

`//`**`mystorageaccount`**`.blob.core.windows.net/`**`mycontainer`**`/`**`myblob`**.

## Configure custom domains

You can configure a custom domain to access blob data in your Azure storage account. As we reviewed, the default endpoint for Azure Blob Storage is `\<storage-account-name>.blob.core.windows.net`. If you map a custom domain and subdomain, such as `www.contoso.com`, to the blob or web endpoint for your storage account, your users can use that domain to access blob data in your storage account.

 Note

Azure Storage doesn't currently provide native support for HTTPS with custom domains. You can implement an Azure Content Delivery Network (CDN) to access blobs by using custom domains over HTTPS.

There are two ways to configure a custom domain: direct mapping and intermediary domain mapping.

- **Direct mapping** lets you enable a custom domain for a subdomain to an Azure storage account. For this approach, you create a `CNAME` record that points from the subdomain to the Azure storage account.
    
    The following example shows how a subdomain is mapped to an Azure storage account to create a `CNAME` record in the domain name system (DNS):
    
    - Subdomain: `blobs.contoso.com`
    - Azure storage account: `\<storage account>\.blob.core.windows.net`
    - Direct `CNAME` record: `contosoblobs.blob.core.windows.net`
- **Intermediary domain mapping** is applied to a domain that's already in use within Azure. This approach might result in minor downtime while the domain is being mapped. To avoid downtime, you can use the `asverify` intermediary domain to validate the domain. By prepending the `asverify` keyword to your own subdomain, you permit Azure to recognize your custom domain without modifying the DNS record for the domain. After you modify the DNS record for the domain, your domain is mapped to the blob endpoint with no downtime.
    
    The following example shows how a domain in use is mapped to an Azure storage account in the DNS with the `asverify` intermediary domain:
    
    - `CNAME` record: **`asverify`**`.blobs.contoso.com`
    - Intermediate `CNAME` record: **`asverify`**`.contosoblobs.blob.core.windows.net`
    
    [Learn more about intermediary domain mapping](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-custom-domain-name?tabs=azure-portal)

# Secure storage endpoints

In the Azure portal, each Azure service requires certain steps to configure the service endpoints and restrict network access.

To access these settings for your storage account, you use the **Firewalls and virtual networks** settings. You add the virtual networks that should have access to the service for the account.

![Screenshot of the Storage Account Firewalls and virtual networks settings in the Azure portal. One virtual network is selected and the firewall has an IP address range.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-storage-accounts/media/secure-storage-access-d32868ef.png)

### Things to know about configuring service endpoints

Here are some points to consider about configuring service access settings:

- The **Firewalls and virtual networks** settings restrict access to your storage account from specific subnets on virtual networks or public IPs.
    
- You can configure the service to allow access to one or more public IP ranges.
    
- Subnets and virtual networks must exist in the same Azure region or region pair as your storage account.
    

 Important

Be sure to test the service endpoint and verify the endpoint limits access as expected.

---

# Configure Azure Blob Storage

# Introduction

Azure Blob Storage is a service for storing large amounts of unstructured object data. Unstructured data is data that doesn't adhere to a particular data model or definition, such as text or binary data.

In this module, your media company has an extensive library of video clips that are accessed thousands of times a day. The company relies on you to configure Blob Storage for the video data. You plan to use access tiers to reduce cost and improve performance. You're developing a lifecycle management strategy for the older videos. Your plan also includes configuring object replication for failover.

## Learning objectives

In this module, you will:

- Understand the purpose and benefits of Azure Blob Storage.
- Create and configure Azure Blob Storage accounts.
- Manage containers and blobs within Azure Blob Storage.
- Optimize blob storage performance and scalability.
- Implement lifecycle management policies to automate data movement and deletion.
- Determine the best pricing plans for your Azure Blob Storage.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104).

## Prerequisites

Here are some common prerequisites that can be beneficial for understanding and successfully completing this module.

- Basic understanding of cloud computing: Familiarity with cloud computing concepts, such as virtualization, scalability, and pay-as-you-go pricing models, can provide a foundation for understanding how Azure Blob Storage fits into the broader cloud ecosystem.
    
- Knowledge of Azure fundamentals: Having a basic understanding of Microsoft Azure services and concepts, such as Azure Resource Manager, Azure Storage Accounts, and Azure Virtual Networks, can help you navigate and configure blob storage effectively.
    
- Familiarity with storage concepts: Understanding fundamental storage concepts like file systems, directories, files, and data replication can be beneficial when working with blob storage.
    
- Experience with Azure Portal or Azure CLI: Familiarity with the Azure Portal (web-based management interface) or Azure CLI (command-line interface) can help you navigate and configure blob storage resources efficiently.
    
- Basic programming or scripting skills: While not always required, having some knowledge of programming or scripting languages like PowerShell or Python can be advantageous when automating blob storage configuration tasks.

# Implement Azure Blob Storage

Azure Blob Storage is a service that stores unstructured data in the cloud as objects or blobs. Blob stands for Binary Large Object. Blob Storage is also referred to as _object storage_ or _container storage_.

### Things to know about Azure Blob Storage

Let's examine some configuration characteristics of Blob Storage.

- Blob Storage can store any type of text or binary data. Some examples are text documents, images, video files, and application installers.
    
- Blob Storage uses three resources to store and manage your data:
    
    - An Azure storage account
    - Containers in an Azure storage account
    - Blobs in a container
- To implement Blob Storage, you configure several settings:
    
    - Blob container options
    - Blob types and upload options
    - Blob Storage access tiers
    - Blob lifecycle rules
    - Blob object replication options

The following diagram shows the relationship between the Blob Storage resources.

![Diagram that shows the Azure Blob Storage architecture.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-blob-storage/media/blob-storage-94fb52b8.png)

### Things to consider when implementing Azure Blob Storage

There are many common uses for Blob Storage. Consider the following scenarios and think about your own data needs:

- **Consider browser uploads**. Use Blob Storage to serve images or documents directly to a browser.
    
- **Consider distributed access**. Blob Storage can store files for distributed access, such as during an installation process.
    
- **Consider streaming data**. Stream video and audio by using Blob Storage.
    
- **Consider archiving and recovery**. Blob Storage is a great solution for storing data for backup and restore, disaster recovery, and archiving.
    
- **Consider application access**. You can store data in Blob Storage for analysis by an on-premises or Azure-hosted service.

# Create blob containers

Azure Blob Storage uses a container resource to group a set of blobs. A blob can't exist by itself in Blob Storage. A blob must be stored in a container resource.

### Things to know about containers and blobs

Let's look at the configuration characteristics of containers and blobs.

- All blobs must be in a container.
    
- A container can store an unlimited number of blobs.
    
- An Azure storage account can contain an unlimited number of containers.
    
- You can create the container in the Azure portal.
    
- You upload blobs into a container.
    

### How to move content between containers

### Configure a container

In the Azure portal, you configure two settings to create a container for an Azure storage account. As you review these details, consider how you might organize containers in your storage account.

![Screenshot that shows the container creation page and the public access level choices in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-blob-storage/media/blob-containers-a243a2b9.png)

- **Name**: Enter a name for your container. The name must be unique within the Azure storage account.
    
    - The name can contain only lowercase letters, numbers, and hyphens.
    - The name must begin with a letter or a number.
    - The minimum length for the name is three characters.
    - The maximum length for the name is 63 characters.
- **Public access level**: The access level specifies whether the container and its blobs can be accessed publicly. By default, container data is private and visible only to the account owner. There are three access level choices:
    
    - **Private**: (Default) Prohibit anonymous access to the container and blobs.
    - **Blob**: Allow anonymous public read access for the blobs only.
    - **Container**: Allow anonymous public read and list access to the entire container, including the blobs.

 Note

You can also create a blob container with PowerShell by using the `New-AzStorageContainer` command.

# Assign blob access tiers

Azure Storage supports several access tiers for blob data, including Hot, Cool, and Archive. Each access tier is optimized to support a particular pattern of data usage.

### Things to know about blob access tiers

Let's examine characteristics of the blob access tiers.

#### Hot tier

The Hot tier is optimized for frequent reads and writes of objects in the Azure storage account. A good usage case is data that is actively being processed. By default, new storage accounts are created in the Hot tier. This tier has the lowest access costs, but higher storage costs than the Cool and Archive tiers.

#### Cool tier

The Cool tier is optimized for storing large amounts of data that's infrequently accessed. This tier is intended for data that remains in the Cool tier for at least 30 days. A usage case for the Cool tier is short-term backup and disaster recovery datasets and older media content. This content shouldn't be viewed frequently, but it needs to be immediately available. Storing data in the Cool tier is more cost-effective. Accessing data in the Cool tier can be more expensive than accessing data in the Hot tier.

#### Cold tier

The Cold tier is also optimized for storing large amounts of data that's infrequently accessed. This tier is intended for data that can remain in the tier for at least 90 days.

#### Archive tier

The Archive tier is an offline tier that's optimized for data that can tolerate several hours of retrieval latency. Data must remain in the Archive tier for at least 180 days or be subject to an early deletion charge. Data for the Archive tier includes secondary backups, original raw data, and legally required compliance information. This tier is the most cost-effective option for storing data. Accessing data is more expensive in the Archive tier than accessing data in the other tiers.

### Compare access tiers

The access options for Azure Blob Storage offer a range of features and support levels to help you optimize your storage costs. As you compare the features and support, think about which access options can best support your application needs.

|Comparison|Hot access tier|Cool access tier|Cold access tier|Archive access tier|
|---|---|---|---|---|
|**Availability**|99.9%|99%|99%|99%|
|**Availability (RA-GRS reads)**|99.99%|99.9%|99.9%|99.9%|
|**Latency (time to first byte)**|milliseconds|milliseconds|milliseconds|hours|
|**Minimum storage duration**|N/A|30 days|90 days|180 days|

### Configure the blob access tier

In the Azure portal, you can select the blob access tier for your Azure storage account. You can also change the blob access tier for your account at any time. By selecting the correct access tier for your needs, you can store your blob data in the most cost-effective manner.

# Add blob lifecycle management rules

Every data set has a unique lifecycle. Early in the lifecycle, users tend to access some of the data in the set, but not all of the data. As the data set ages, access to all of the data in the set tends to dramatically reduce. Some data set stays idle in the cloud and is rarely accessed after it's stored. Some data expires within a few days or months after it's created. Other data is actively read and modified throughout the data set lifetime.

Azure Blob Storage supports lifecycle management for data sets. It offers a rich rule-based policy for GPv2 and Blob Storage accounts. You can use lifecycle policy rules to transition your data to the appropriate access tiers, and set expiration times for the end of a data set's lifecycle.

### How to automatically manage Azure Blobs lifecycles | Azure Tips and Tricks

### Things to know about lifecycle management

You can use Azure Blob Storage lifecycle management policy rules to accomplish several tasks.

- Transition blobs to a cooler storage tier (Hot to Cool, Hot to Archive, Cool to Archive) to optimize for performance and cost.
    
- Delete blobs at the end of their lifecycles.
    
- Define rule-based conditions to run once per day at the Azure storage account level.
    
- Apply rule-based conditions to containers or a subset of blobs.
    

#### Business scenario

Consider a scenario where data is frequently accessed in the early stages of the lifecycle, but only occasionally after two weeks. After the first month, the data set is rarely accessed. In this scenario, the Hot tier of Blob Storage is best during the early stages. Cool tier storage is most appropriate for occasional access. Archive tier storage is the best option after the data ages over a month. To achieve this transition, lifecycle management policy rules are available to move aging data to cooler tiers.

### Configure lifecycle management policy rules

In the Azure portal, you create lifecycle management policy rules for your Azure storage account by specifying several settings. For each rule, you create **If - Then** block conditions to transition or expire data based on your specifications. As you review these details, consider how you can set up lifecycle management policy rules for your data sets.

![Screenshot that shows how to add a lifecycle management policy rule for blob data in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-blob-storage/media/blob-lifecycle-2854d812.png)

- **If**: The **If** clause sets the evaluation clause for the policy rule. When the **If** clause evaluates to true, the **Then** clause is executed. Use the **If** clause to set the time period to apply to the blob data. The lifecycle management feature checks if the data is accessed or modified according to the specified time.
    
    - **More than (days ago)**: The number of days to use in the evaluation condition.
- **Then**: The **Then** clause sets the action clause for the policy rule. When the **If** clause evaluates to true, the **Then** clause is executed. Use the **Then** clause to set the transition action for the blob data. The lifecycle management feature transitions the data based on the setting.
    
    - **Move to cool storage**: The blob data is transitioned to Cool tier storage.
    - **Move to archive storage**: The blob data is transitioned to Archive tier storage.
    - **Delete the blob**: The blob data is deleted.

By designing policy rules to adjust storage tiers in respect to the age of data, you can design the least expensive storage options for your needs.

# Determine blob object replication

Object replication copies blobs in a container asynchronously according to policy rules that you configure. During the replication process, the following contents are copied from the source container to the destination container:

- The blob contents
- The blob metadata and properties
- Any versions of data associated with the blob

The following illustration shows an example of asynchronous replication of blob containers between regions.

![Diagram that shows asynchronous replication of blob containers between regions.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-blob-storage/media/blob-object-replication-21fd3c07.png)

### Things to know about blob object replication

There are several considerations to keep in mind when planning your configuration for blob object replication.

- Object replication requires that blob versioning is enabled on both the source and destination accounts.
    
- Object replication doesn't support blob snapshots. Any snapshots on a blob in the source account aren't replicated to the destination account.
    
- Object replication is supported when the source and destination accounts are in the Hot, Cool, or Cold tier. The source and destination accounts can be in different tiers.
    
- When you configure object replication, you create a replication policy that specifies the source Azure storage account and the destination storage account.
    
- A replication policy includes one or more rules that specify a source container and a destination container. The policy identifies the blobs in the source container to replicate.
    

### Things to consider when configuring blob object replication

There are many benefits to using blob object replication. Consider the following scenarios and think about how replication can be a part of your Blob Storage strategy.

- **Consider latency reductions**. Minimize latency with blob object replication. You can reduce latency for read requests by enabling clients to consume data from a region that's in closer physical proximity.
    
- **Consider efficiency for compute workloads**. Improve efficiency for compute workloads by using blob object replication. With object replication, compute workloads can process the same sets of blobs in different regions.
    
- **Consider data distribution**. Optimize your configuration for data distribution. You can process or analyze data in a single location and then replicate only the results to other regions.
    
- **Consider costs benefits**. Manage your configuration and optimize your storage policies to achieve cost benefits. After your data is replicated, you can reduce costs by moving the data to the Archive tier by using lifecycle management policies.

# Upload blobs

A blob can be any type of data and any size file. Azure Storage offers three types of blobs: _block blob_, _page blob_, and _append blob_.

### Things to know about blob types

Let's take a closer look at the characteristics of blob types.

- **Block blobs**. A block blob consists of blocks of data that are assembled to make a blob. Most Blob Storage scenarios use block blobs. Block blobs are ideal for storing text and binary data in the cloud, like files, images, and videos.
    
- **Append blobs**. An append blob is similar to a block blob because the append blob also consists of blocks of data. The blocks of data in an append blob are optimized for _append_ operations. Append blobs are useful for logging scenarios, where the amount of data can increase as the logging operation continues.
    
- **Page blobs**. A page blob can be up to 8 TB in size. Page blobs are more efficient for frequent read/write operations. Azure Virtual Machines uses page blobs for operating system disks and data disks.
    
- The block blob type is the default type for a new blob. When you're creating a new blob, if you don't choose a specific type, the new blob is created as a block blob.
    
- After you create a blob, you can't change its type.
    

### Things to consider when using blob upload tools

A common approach for uploading blobs to your Azure storage account is to use Azure Storage Explorer. Many other tools are also available. Review the following options and consider which tools would suit your configuration needs.

Expand table

|Upload tool|Description|
|---|---|
|**AzCopy**|An easy-to-use command-line tool for Windows and Linux. You can copy data to and from Blob Storage, across containers, and across storage accounts.|
|**Azure Data Box Disk**|A service for transferring on-premises data to Blob Storage when large datasets or network constraints make uploading data over the wire unrealistic. You can use Azure Data Box Disk to request solid-state disks (SSDs) from Microsoft. You can copy your data to those disks and ship them back to Microsoft to be uploaded into Blob Storage.|
|**Azure Import/Export**|A service that helps you export large amounts of data from your storage account to hard drives that you provide and that Microsoft then ships back to you with your data.|

#### Business scenario

The following example shows how to upload blob data in Azure Storage Explorer. After you identify the files to upload, you choose the blob type and block size, and the container folder. You also set the authentication method and encryption scope.

![Screenshot of the Upload Blob page that shows the Authentication type, blob types, and block size.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-blob-storage/media/upload-blobs-7ad73d30.png)

### How to use Blob versioning

You can enable Blob storage versioning to automatically maintain previous versions of an object. When blob versioning is enabled, you can access earlier versions of a blob to recover your data if it's modified or deleted.

# Determine Blob Storage pricing

All Azure storage accounts use a pricing model for Azure Blob Storage that's based on the tier of each blob.

### Things to know about pricing for Blob Storage

Review the following billing considerations for an Azure storage account and Blob Storage.

- **Performance tiers**. The Blob Storage tier determines the amount of data stored and the cost for storing that data. As the performance tier gets cooler, the per-gigabyte cost decreases.
    
- **Data access costs**. Data access charges increase as the tier gets cooler. For data in the Cool and Archive tiers, you're billed a per-gigabyte data access charge for reads.
    
- **Transaction costs**. There's a per-transaction charge for all tiers. The charge increases as the tier gets cooler.
    
- **Geo-replication data transfer costs**. This charge only applies to accounts that have geo-replication configured, including GRS and RA-GRS. Geo-replication data transfer incurs a per-gigabyte charge.
    
- **Outbound data transfer costs**. Outbound data transfers (data that's transferred out of an Azure region) incur billing for bandwidth usage on a per-gigabyte basis. This billing is consistent with general-purpose Azure storage accounts.
    
- **Changes to the storage tier**. If you change the account storage tier from Cool to Hot, you incur a charge equal to reading all the data existing in the storage account. Changing the account storage tier from Hot to Cool incurs a charge equal to writing all the data into the Cool tier (GPv2 accounts only).

# Interactive lab simulation

## Lab scenario

Your organization is migrating storage to Azure. As the Azure Administrator you need to:

- Organize content into storage accounts.
- Upload and manage images.
- Monitor and troubleshoot storage accounts.

## Objectives

- **Task 1**: Create a storage account.
    - Create a storage account in your region with locally redundant storage.
    - Verify the storage account was created.
- **Task 2**: Work with blob storage.
    - Create a private blob container.
    - Upload a file to the container.
- **Task 3**: Monitor the storage container.
    - Review common storage problems and troubleshooting guides.
    - Review insights for performance, availability, and capacity.

 Note

Click on the thumbnail image to start the lab simulation. When you're done, be sure to return to this page so you can continue learning.

[![Screenshot of the simulation page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-blob-storage/media/simulation-blob-storage.jpg)](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%205)

---

# Configure Azure Storage security

# Introduction

Azure Storage provides a comprehensive set of security capabilities that work together to enable developers to build secure applications.

In this module, your company is storing sensitive data in Azure Storage, including personal information. The data is used internally and by external application developers. You're responsible for ensuring the data is secure for all users. You're tasked with providing configuration solutions to grant secure access to the information.

## Learning objectives

In this module, you learn how to:

- Configure a shared access signature, including the uniform resource identifier (URI) and SAS parameters.
- Configure Azure Storage encryption.
- Implement customer-managed keys.
- Recommend opportunities to improve Azure Storage security.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104). The module concepts are covered in:

Implement and manage storage (15–20%)

- Secure storage
    - Generate shared access signature (SAS) tokens
    - Manage access keys
    - Configure Microsoft Entra authentication for an Azure storage account

# Review Azure Storage security strategies

Administrators use different strategies to ensure their data is secure. Common approaches include encryption, authentication, authorization, and user access control with credentials, file permissions, and private signatures. Azure Storage offers a suite of security capabilities based on common strategies to help you secure your data.

### Things to know about Azure Storage security strategies

Let's look at some characteristics of Azure Storage security.

- **Encryption**. All data written to Azure Storage is automatically encrypted by using Azure Storage encryption.
    
- **Authentication**. Microsoft Entra ID and role-based access control (RBAC) are supported for Azure Storage for both resource management operations and data operations.
    
    - Assign RBAC roles scoped to an Azure storage account to security principals, and use Microsoft Entra ID to authorize resource management operations like key management.
    - Microsoft Entra integration is supported for data operations on Azure Blob Storage and Azure Queue Storage.
- **Data in transit**. Data can be secured in transit between an application and Azure by using Client-Side Encryption, HTTPS, or SMB 3.0.
    
- **Disk encryption**. Operating system disks and data disks used by Azure Virtual Machines can be encrypted by using Azure Disk Encryption.
    
- **Shared access signatures**. Delegated access to the data objects in Azure Storage can be granted by using a shared access signature (SAS).
    
- **Authorization**. Every request made against a secured resource in Blob Storage, Azure Files, Queue Storage, or Azure Cosmos DB (Azure Table Storage) must be authorized. Authorization ensures that resources in your storage account are accessible only when you want them to be, and to only those users or applications whom you grant access.
    

### Things to consider when using authorization security

Review the following strategies for authorizing requests to Azure Storage. Think about what security strategies would work for your Azure Storage.

Expand table

|Authorization strategy|Description|
|---|---|
|**Microsoft Entra ID**|Microsoft Entra ID is Microsoft's cloud-based identity and access management service. With Microsoft Entra ID, you can assign fine-grained access to users, groups, or applications by using role-based access control.|
|**Shared Key**|Shared Key authorization relies on your Azure storage account access keys and other parameters to produce an encrypted signature string. The string is passed on the request in the Authorization header.|
|**Shared access signatures**|A SAS delegates access to a particular resource in your Azure storage account with specified permissions and for a specified time interval.|
|**Anonymous access to containers and blobs**|You can optionally make blob resources public at the container or blob level. A public container or blob is accessible to any user for anonymous read access. Read requests to public containers and blobs don't require authorization.|

# Create shared access signatures

A shared access signature (SAS) is a uniform resource identifier (URI) that grants restricted access rights to Azure Storage resources. SAS is a secure way to share your storage resources without compromising your account keys.

You can provide a SAS to clients who shouldn't have access to your storage account key. By distributing a SAS URI to these clients, you grant them access to a resource for a specified period of time.

### Things to know about shared access signatures

Let's review some characteristics of a SAS.

- A SAS gives you granular control over the type of access you grant to clients who have the SAS.
    
- An account-level SAS can delegate access to multiple Azure Storage services, such as blobs, files, queues, and tables.
    
- You can specify the time interval for which a SAS is valid, including the start time and the expiration time.
    
- You specify the permissions granted by the SAS. A SAS for a blob might grant read and write permissions to that blob, but not delete permissions.
    
- SAS provides account-level and service-level control.
    
    - **Account-level** SAS delegates access to resources in one or more Azure Storage services.
        
    - **Service-level** SAS delegates access to a resource in only one Azure Storage service.
        
    
     Note
    
    A **stored access policy** can provide another level of control when you use a service-level SAS on the server side. You can group SASs and provide other restrictions by using a stored access policy.
    
- There are optional SAS configuration settings:
    
    - **IP addresses**. You can identify an IP address or range of IP addresses from which Azure Storage accepts the SAS. Configure this option to specify a range of IP addresses that belong to your organization.
        
    - **Protocols**. You can specify the protocol over which Azure Storage accepts the SAS. Configure this option to restrict access to clients by using HTTPS.
        

## Configure a shared access signature

In the Azure portal, you configure several settings to create a SAS. As you review these details, consider how you might implement shared access signatures in your storage security solution.

![Screenshot of the Create a shared access signature key page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-storage-security/media/configure-secure-signatures-be02fa89.png)

- **Signing method**: Choose the signing method: Account key or User delegation key.
- **Signing key**: Select the signing key from your list of keys.
- **Permissions**: Select the permissions granted by the SAS, such as read or write.
- **Start and Expiry date/time**: Specify the time interval for which the SAS is valid. Set the start time and the expiry time.
- **Allowed IP addresses**: (Optional) Identify an IP address or range of IP addresses from which Azure Storage accepts the SAS.
- **Allowed protocols**: (Optional) Select the protocol over which Azure Storage accepts the SAS.

# Identify URI and SAS parameters

When you create your shared access signature (SAS), a uniform resource identifier (URI) is created by using parameters and tokens. The URI consists of your Azure Storage resource URI and the SAS token.

![Storage Resource and the S A S Token combine to form the U R I.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-storage-security/media/secure-parameters-76db5bda.png)

### Things to know about URI definitions

Let's look at a sample URI definition and examine the parameters. This sample creates a service-level SAS that grants read and write permissions to a blob. Consider how you might configure the parameters to support your Azure Storage resources.

URICopy

```
https://myaccount.blob.core.windows.net/?restype=service&comp=properties&sv=2015-04-05&ss=bf&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B
```

|Parameter|Example|Description|
|---|---|---|
|**Resource URI**|`https://myaccount.`**`blob`**`.core.windows.net/` `?restype=`**`service`** `&amp;comp=properties`|Defines the Azure Storage endpoint and other parameters. This example defines an endpoint for Blob Storage and indicates that the SAS applies to service-level operations. When the URI is used with `GET`, the Storage properties are retrieved. When the URI is used with `SET`, the Storage properties are configured.|
|**Storage version**|**`sv`**`=2015-04-05`|For Azure Storage version 2012-02-12 and later, this parameter indicates the version to use. This example indicates that version 2015-04-05 (April 5, 2015) should be used.|
|**Storage service**|**`ss`**`=bf`|Specifies the Azure Storage to which the SAS applies. This example indicates that the SAS applies to Blob Storage and Azure Files.|
|**Start time**|**`st`**`=2015-04-29T22%3A18%3A26Z`|(Optional) Specifies the start time for the SAS in UTC time. This example sets the start time as April 29, 2015 22:18:26 UTC. If you want the SAS to be valid immediately, omit the start time.|
|**Expiry time**|**`se`**`=2015-04-30T02%3A23%3A26Z`|Specifies the expiration time for the SAS in UTC time. This example sets the expiry time as April 30, 2015 02:23:26 UTC.|
|**Resource**|**`sr`**`=b`|Specifies which resources are accessible via the SAS. This example specifies that the accessible resource is in Blob Storage.|
|**Permissions**|**`sp`**`=rw`|Lists the permissions to grant. This example grants access to read and write operations.|
|**IP range**|**`sip`**`=168.1.5.60-168.1.5.70`|Specifies a range of IP addresses from which a request is accepted. This example defines the IP address range 168.1.5.60 through 168.1.5.70.|
|**Protocol**|**`spr`**`=https`|Specifies the protocols from which Azure Storage accepts the SAS. This example indicates that only requests by using HTTPS are accepted.|
|**Signature**|**`sig`**`=F%6GRVAZ5Cdj2Pw4tgU7Il` `STkWgn7bUkkAg8P6HESXwmf%4B`|Specifies that access to the resource is authenticated by using an HMAC signature. The signature is computed over a string-to-sign with a key by using the SHA256 algorithm, and encoded by using Base64 encoding.|

# Determine Azure Storage encryption

Azure Storage encryption for data at rest protects your data by ensuring your organizational security and compliance commitments are met. The encryption and decryption processes happen automatically. Because your data is secured by default, you don't need to modify your code or applications.

### Things to know about Azure Storage encryption

Examine the following characteristics of Azure Storage encryption.

- Data is encrypted automatically before it's persisted to Azure Managed Disks, Azure Blob Storage, Azure Queue Storage, Azure Cosmos DB, Azure Table Storage, or Azure Files.
    
- Data is automatically decrypted before it's retrieved.
    
- Azure Storage encryption, encryption at rest, decryption, and key management are transparent to users.
    
- All data written to Azure Storage is encrypted through 256-bit advanced encryption standard (AES) encryption. AES is one of the strongest block ciphers available.
    
- Azure Storage encryption is enabled for all new and existing storage accounts and can't be disabled.
    

## Configure Azure Storage encryption

In the Azure portal, you configure Azure Storage encryption by specifying the encryption type. You can manage the keys yourself, or you can have the keys managed by Microsoft. Consider how you might implement Azure Storage encryption for your storage security.

![Screenshot that shows Azure Storage encryption, including keys managed by Microsoft and customer-managed keys.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-storage-security/media/secure-encryption-e3b68445.png)
# Create customer-managed keys

For your Azure Storage security solution, you can use Azure Key Vault to manage your encryption keys. The Azure Key Vault APIs can be used to generate encryption keys. You can also create your own encryption keys and store them in a key vault.

### Things to know about customer-managed keys

Consider the following characteristics of customer-managed keys.

- By creating your own keys (referred to as _customer-managed_ keys), you have more flexibility and greater control.
    
- You can create, disable, audit, rotate, and define access controls for your encryption keys.
    
- Customer-managed keys can be used with Azure Storage encryption. You can use a new key or an existing key vault and key. The Azure storage account and the key vault must be in the same region, but they can be in different subscriptions.
    

## Configure customer-managed keys

In the Azure portal, you can configure customer-managed encryption keys. You can create your own keys, or you can have the keys managed by Microsoft. Consider how you might use Azure Key Vault to create your own customer-managed encryption keys.

![Screenshot that shows how to create a customer-managed key.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-storage-security/media/customer-keys-b24acc48.png)

- **Encryption type**: Choose how the encryption key is managed: by Microsoft or by yourself (customer).
- **Encryption key**: Specify an encryption key by entering a URI, or select a key from an existing key vault.

# Apply Azure Storage security best practices

We reviewed how to create and work with a shared access signature (SAS) and the benefits it can provide to your storage security solution.

It's important to understand that when you use a SAS in your application, there can be potential risks.

- If a SAS is compromised, it can be used by anyone who obtains it, including a malicious user.
    
- If a SAS provided to a client application expires and the application is unable to retrieve a new SAS from your service, the application functionality might be hindered.
    

Watch this video for more ideas on how to secure your storage. This video is based on [Azure Tips and Tricks #272 Azure Security Best Practices](https://microsoft.github.io/AzureTipsAndTricks/blog/blog/tip272.html).

### Recommendations for managing risks

Let's look at some recommendations that can help mitigate risks when working with a SAS.

Expand table

|Recommendation|Description|
|---|---|
|**Always use HTTPS for creation and distribution**|If a SAS is passed over HTTP and intercepted, an attacker can intercept and use the SAS. These _man-in-the-middle_ attacks can compromise sensitive data or allow for data corruption by the malicious user.|
|**Reference stored access policies where possible**|Stored access policies give you the option to revoke permissions without having to regenerate the Azure storage account keys. Set the storage account key expiration date far in the future.|
|**Set near-term expiry times for an unplanned SAS**|If a SAS is compromised, you can mitigate attacks by limiting the SAS validity to a short time. This practice is important if you can't reference a stored access policy. Near-term expiration times also limit the amount of data that can be written to a blob by limiting the time available to upload to it.|
|**Require clients automatically renew the SAS**|Require your clients to renew the SAS well before the expiration date. By renewing early, you allow time for retries if the service providing the SAS is unavailable.|
|**Plan carefully for the SAS start time**|If you set the start time for a SAS to now, then due to clock skew (differences in current time according to different machines), failures might be observed intermittently for the first few minutes. In general, set the start time to at least 15 minutes in the past. Or, don't set a specific start time, which causes the SAS to be valid immediately in all cases. The same conditions generally apply to the expiry time. You might observe up to 15 minutes of clock skew in either direction on any request. For clients that use a REST API version earlier than 2012-02-12, the maximum duration for a SAS that doesn't reference a stored access policy is 1 hour. Any policies that specify a longer term will fail.|
|**Define minimum access permissions for resources**|A security best practice is to provide a user with the minimum required privileges. If a user only needs read access to a single entity, then grant them read access to that single entity, and not read/write/delete access to all entities. This practice also helps lessen the damage if a SAS is compromised because the SAS has less power in the hands of an attacker.|
|**Understand account billing for usage, including a SAS**|If you provide write access to a blob, a user might choose to upload a 200-GB blob. If you've given them read access as well, they might choose to download the blob 10 times, which incurs 2 TB in egress costs for you. Again, provide limited permissions to help mitigate the potential actions of malicious users. Use a short-lived SAS to reduce this threat, but be mindful of clock skew on the end time.|
|**Validate data written by using a SAS**|When a client application writes data to your Azure storage account, keep in mind there can be problems with the data. If your application requires validated or authorized data, validate the data after it's written, but before it's used. This practice also protects against corrupt or malicious data being written to your account, either by a user who properly acquired the SAS, or by a user exploiting a leaked SAS.|
|**Don't assume a SAS is always the correct choice**|In some scenarios, the risks associated with a particular operation against your Azure storage account outweigh the benefits of using a SAS. For such operations, create a middle-tier service that writes to your storage account after performing business rule validation, authentication, and auditing. Also, sometimes it's easier to manage access in other ways. If you want to make all blobs in a container publicly readable, you can make the container Public, rather than providing a SAS to every client for access.|
|**Monitor your applications with Azure Storage Analytics**|You can use logging and metrics to observe any spike in authentication failures. You might see spikes from an outage in your SAS provider service or to the inadvertent removal of a stored access policy.|

# Interactive lab simulation

## Lab scenario

Your organization is migrating storage to Azure. As the Azure Administrator you need to:

- Evaluate the use of Azure storage for storing files. These files are currently residing in on-premises data stores.
- Minimize the cost of storage by placing less frequently accessed files in lower-priced storage tiers.
- Explore different protection mechanisms that Azure Storage offers, including network access, authentication, authorization, and replication.
- Determine to what extent Azure Files service might be suitable for hosting your on-premises file shares.

## Architecture diagram

![Architecture diagram as explained in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-storage-security/media/lab-07.png)

## Objectives

- **Task 1**: Create the infrastructure environment.
    - Use a template to create the virtual networks and virtual machines. You can review the [lab template](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/tree/master/Allfiles/Interactive%20Lab%20Simulation%20Files/07).
    - Use Azure PowerShell to deploy the template.
- **Task 2**: Create and configure Azure Storage accounts.
    - Create a storage account.
    - Configure the storage account to include redundancy and access tiers.
- **Task 3**: Manage blob storage.
    - Create a private Blob container.
    - Upload a file into the container.
- **Task 4**: Manage authentication and authorization for Azure Storage.
    - Generate a shared access signature (SAS) with limited time access.
    - Verify the SAS is working correctly.
- **Task 5**: Create and configure an Azure Files share.
    - Create a file and connect to it.
    - Use Azure PowerShell to add items to the file share.
- **Task 6**: Manage network access for Azure Storage.
    - Limit access to the Azure storage account from only specific IP addresses.
    - Confirm access is denied from the Cloud Shell.

 Note

Click on the thumbnail image to start the lab simulation. When you're done, be sure to return to this page so you can continue learning.

[![Screenshot of the simulation page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-storage-security/media/simulation-storage-thumbnail.jpg)](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2011)

---

# Configure Azure Files and Azure File Sync

# Introduction

Azure Files offers fully managed file shares in the cloud that are accessible via industry standard protocols. Azure File Sync is a service that allows you to cache several Azure Files shares on an on-premises Windows Server or cloud virtual machine.

In this module, your company has a large repository of organizational documents. Offices are located in different geographical regions, and users need the most current versions of the documents. You're researching how to implement Azure Files shares to provide a central location for the documents. You'd like to configure Azure File Sync to keep the documents up to date across the dispersed offices.

## Learning objectives

In this module, you learn how to:

- Identify storage for file shares.
- Compare file shares to blob and disk storage.
- Configure Azure file shares, file share snapshots, and soft delete.
- Use Azure Storage Explorer to access your file share.
- Identify use cases and features of Azure File Sync.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104). The module concepts are covered in:

Implement and manage storage (15–20%).

- Configure Azure Files and Azure Blob Storage
    - Create and configure a files share in Azure storage.
    - Configure snapshots and soft delete for Azure Files.

# Compare storage for file shares and blob data

[Azure Files](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-introduction) offers shared storage for applications by using the industry standard [Server Message Block](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) and Network File System (NFS) protocols. Azure virtual machines (VMs) and cloud services can share file data across application components by using mounted shares. On-premises applications can also access file data in the share.

### Things to know about Azure Files

Let's examine some characteristics of Azure Files.

- Azure Files stores data as true directory objects in file shares.
    
- Azure Files provides shared access to files across multiple VMs. Any number of Azure virtual machines or roles can mount and access an Azure file share simultaneously.
    
- Applications that run in Azure VMs or cloud services can mount an Azure file share to access file data. This process is similar to how a desktop application mounts a typical SMB share.
    
- Azure Files offers fully managed file shares in the cloud. Azure file shares can be mounted concurrently by cloud or on-premises deployments of Windows, Linux, and macOS.
    

### Things to consider when using Azure Files

There are many common scenarios for using Azure Files. As you review the following suggestions, think about how Azure Files can provide solutions for your organization.

- **Consider replacement and supplement options**. Replace or supplement traditional on-premises file servers or NAS devices by using Azure Files.
    
- **Consider global access**. Directly access Azure file shares by using most operating systems, such as Windows, macOS, and Linux, from anywhere in the world.
    
- **Consider lift and shift support**. _Lift and shift_ applications to the cloud with Azure Files for apps that expect a file share to store file application or user data.
    
- **Consider using Azure File Sync**. Replicate Azure file shares to Windows Servers by using Azure File Sync. You can replicate on-premises or in the cloud for performance and distributed caching of the data where it's being used. We'll take a closer look at Azure File Sync in a later unit.
    
- **Consider shared applications**. Store shared application settings such as configuration files in Azure Files.
    
- **Consider diagnostic data**. Use Azure Files to store diagnostic data such as logs, metrics, and crash dumps in a shared location.
    
- **Consider tools and utilities**. Azure Files is a good option for storing tools and utilities that are needed for developing or administering Azure VMs or cloud services.
    

## Compare Azure Files to Azure Blob Storage

It's important to understand when to use Azure Files to store data in file shares rather than using Azure Blob Storage to store data as blobs. The following table compares different features of these services and common implementation scenarios.

|Azure Files (file shares)|Azure Blob Storage (blobs)|
|---|---|
|Azure Files provides the SMB and NFS protocols, client libraries, and a REST interface that allows access from anywhere to stored files.|Azure Blob Storage provides client libraries and a REST interface that allows unstructured data to be stored and accessed at a massive scale in block blobs.|
|- Files in an Azure Files share are true directory objects.  <br>- Data in Azure Files is accessed through file shares across multiple virtual machines.|- Blobs in Azure Blob Storage are a flat namespace.  <br>- Blob data in Azure Blob Storage is accessed through a container.|
|_**Azure Files** is ideal to lift and shift an application to the cloud that already uses the native file system APIs. Share data between the app and other applications running in Azure._  <br>  <br>_Azure Files is a good option when you want to store development and debugging tools that need to be accessed from many virtual machines._|_**Azure Blob Storage** is ideal for applications that need to support streaming and random-access scenarios._  <br>  <br>_Azure Blob Storage is a good option when you want to be able to access application data from anywhere._|

# Manage Azure file shares

Azure Files offers two industry-standard file system protocols for mounting Azure file shares: the Server Message Block (SMB) protocol and the Network File System (NFS) protocol. Azure file shares don't support both the SMB and NFS protocols on the same file share, although you can create SMB and NFS Azure file shares within the same storage account.

## Types of Azure file shares

Azure also offers two types of file shares: standard and premium. There are key differences between premium and standard file shares:

- The premium tier stores data on modern solid-state drives (SSDs), while the standard tier uses hard disk drives (HDDs).
    
- Standard file shares can be used with SMB and REST protocols only, while premium file shares can be used with SMB, NFS, and REST protocols.
    
- You can easily switch between hot, cool, and transaction optimized tiers of standard file shares, but you can't switch from premium file shares to any of the standard tiers.
    

## Creating SMB Azure file shares

There are two important settings that you need to be aware of when creating and configuring SMB Azure file shares.

- **Open port 445**. SMB communicates over TCP port 445. Make sure port 445 is open. Also, make sure your firewall isn't blocking TCP port 445 from the client machine. If you can't unblock port 445, then a VPN or ExpressRoute connection from on-premises to your Azure network is required, with Azure Files exposed on your internal network using private endpoints.
    
- **Enable secure transfer**. The `Secure transfer required` setting enhances the security of your storage account by limiting requests to your storage account from secure connections only. Consider the scenario where you use REST APIs to access your storage account. If you attempt to connect, and secure transfer required is enabled, you must connect by using HTTPS. If you try to connect to your account by using HTTP, and secure transfer required is enabled, the connection is rejected.
    

## Mount an SMB Azure file share on Windows

You can use Azure file shares seamlessly in Windows and Windows Server. You can connect to your Azure file share with Windows or Windows Server in the Azure portal. Specify the **Drive** where you want to mount the share, and choose the **Authentication method**. The Azure portal supplies you with PowerShell commands to run when you're ready to work with the file share. This video shows you how to mount the SMB file share on Windows.

![Manage Azure file shares - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/configure-azure-files-file-sync/3-manage-file-shares)
## Mount SMB Azure file share on Linux

You can also connect to Azure file shares from Linux machines. From your virtual machine page, select **Connect**. SMB Azure file shares can be mounted in Linux distributions by using the CIFS kernel client. File mounting can be done on-demand with the `mount` command or on-boot (persistent) by creating an entry in /etc/fstab.


# Create file share snapshots

Azure Files provides the capability to take share [snapshots of file shares](https://learn.microsoft.com/en-us/azure/storage/files/storage-snapshots-files). File share snapshots capture a point-in-time, read-only copy of your data.

![Screenshot of a file share snapshot that shows the snapshot name and date it was created.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-files-file-sync/media/file-share-snapshot-cbda2136.png)

### Things to know about file share snapshots

Let's review some characteristics of file share snapshots.

- The Azure Files share snapshot capability is provided at the file share level.
    
- Share snapshots are incremental in nature. Only data changed since the most recent share snapshot is saved.
    
- Incremental snapshots minimize the time required to create share snapshots and saves on storage costs.
    
- Even though share snapshots are saved incrementally, you only need to retain the most recent share snapshot to restore the share.
    
- You can retrieve a share snapshot for an individual file. This level of support helps with restoring individual files rather than having to restore to the entire file share.
    
- Share snapshots provide only file-level protection. They don't prevent fat-finger deletions on a file share or storage account. If you delete a file share that has share snapshots, all of its snapshots will be deleted along with the share.
    

### Things to consider when using file share snapshots

There are several benefits to using file share snapshots and having access to incremental point-in-time data storage. As you review the following suggestions, think about how you can implement file share snapshots in your Azure Files storage solution.

Expand table

|Benefit|Description|
|---|---|
|**_Protect against application error and data corruption_**|Applications that use file shares perform operations like writing, reading, storage, transmission, and processing. When an application is misconfigured or an unintentional bug is introduced, accidental overwrite or damage can happen to a few data blocks. To help protect against these scenarios, you can take a share snapshot before you deploy new application code. When a bug or application error is introduced with the new deployment, you can go back to a previous version of your data on that file share.|
|**_Protect against accidental deletions or unintended changes_**|Imagine you're working on a text file in a file share. After the text file is closed, you lose the ability to undo your changes. In this scenario, you need to recover a previous version of your file. You can use share snapshots to recover previous versions of the file if it's accidentally renamed or deleted.|
|**_Support backup and recovery_**|After you create a file share, you can periodically create a snapshot of the file share to use it for data backup. A share snapshot, when taken periodically, helps maintain previous versions of data that can be used for future audit requirements or disaster recovery.|

# Implement soft delete for Azure Files

Azure Files offers [soft delete for file shares](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-prevent-file-share-deletion?toc=%2Fazure%2Fstorage%2Ffile-sync). Soft delete lets you recover deleted files and file shares.

![Illustration that depicts how to enable soft delete on an Azure file share.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-files-file-sync/media/files-enable-soft-delete-new-ui.png)

### Things to know about soft delete for Azure Files

Let's take a look at the characteristics of soft delete for Azure Files.

- Soft delete for file shares is enabled at the file share level.
    
- Soft delete transitions content to a soft deleted state instead of being permanently erased.
    
- Soft delete lets you configure the retention period. The retention period is the amount of time that soft deleted file shares are stored and available for recovery.
    
- Soft delete provides a retention period between 1 and 365 days.
    
- Soft delete can be enabled on either new or existing file shares.
    

### Things to consider when using soft delete for Azure Files

There are many advantages to using soft delete for Azure Files. Consider the following scenarios, and think about how you can use soft delete.

- **Recover from accidental data loss**. Use soft delete to recover data that is deleted or corrupted.
    
- **Upgrade scenarios**. Use soft delete to restore to a known good state after a failed upgrade attempt.
    
- **Ransomware protection**. Use soft delete to recover data without paying ransom to cybercriminals.
    
- **Long-term retention**. Use soft delete to comply with data retention requirements.
    
- **Business continuity**. Use soft delete to prepare your infrastructure to be highly available for critical workloads.

# Use Azure Storage Explorer

[Azure Storage Explorer](https://learn.microsoft.com/en-us/azure/storage/storage-explorer/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) is a standalone application that makes it easy to work with Azure Storage data on Windows, macOS, and Linux. With Azure Storage Explorer, you can access multiple accounts and subscriptions, and manage all your Storage content.

![Screenshot of Azure Storage Explorer that shows the Emulator storage account open, which has a folder and several documents. The access tier information is visible.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-files-file-sync/media/storage-explorer.png)

### Things to know about Azure Storage Explorer

Azure Storage Explorer has the following characteristics.

- Azure Storage Explorer requires both management (Azure Resource Manager) and data layer permissions to allow full access to your resources. You need Microsoft Entra ID (formerly Azure Active Directory) permissions to access your storage account, the containers in your account, and the data in the containers.
    
- Azure Storage Explorer lets you connect to different storage accounts.
    
    - Connect to storage accounts associated with your Azure subscriptions.
    - Connect to storage accounts and services that are shared from other Azure subscriptions.
    - Connect to and manage local storage by using the Azure Storage Emulator.
    
    ![Screenshot of the Azure Explorer Manage Accounts page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-files-file-sync/media/connection-options-1df9c8f7.png)
    

### Things to consider when using Azure Storage Explorer

Azure Storage Explorer supports many scenarios for working with storage accounts in global and national Azure. As you review these options, think about which scenarios apply to your Azure Storage implementation.

Expand table

|Scenario|Description|
|---|---|
|**Connect to an Azure subscription**|Manage storage resources that belong to your Azure subscription.|
|**Work with local development storage**|Manage local storage by using the Azure Storage Emulator.|
|**Attach to external storage**|Manage storage resources that belong to another Azure subscription or that are under national Azure clouds by using the storage account name, key, and endpoints. This scenario is described in more detail in the next section.|
|**Attach a storage account with a SAS**|Manage storage resources that belong to another Azure subscription by using a shared access signature (SAS).|
|**Attach a service with a SAS**|Manage a specific Azure Storage service (blob container, queue, or table) that belongs to another Azure subscription by using a SAS.|

## Attach to external storage account

Azure Storage Explorer lets you attach to external storage accounts so storage accounts can be easily shared.

To create the connection, you need the external storage **Account name** and **Account key**. In the Azure portal, the account key is called **key1**.

![Screenshot of the Azure Storage Explorer wizard to connect to an external storage account.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-files-file-sync/media/attach-name-key-13fe3ba3.png)

To use a storage account name and key from a national Azure cloud, use the **Storage endpoints domain** drop-down menu to select **Other**, and then enter the custom storage account endpoint domain.

### Access keys

Access keys provide access to the entire storage account. You're provided two access keys so you can maintain connections by using one key while regenerating the other.

 Important

Store your access keys securely. We recommend regenerating your access keys regularly.

When you regenerate your access keys, you must update any Azure resources and applications that access this storage account to use the new keys. This action doesn't interrupt access to disks from your virtual machines.

# Deploy Azure File Sync

[Azure File Sync](https://learn.microsoft.com/en-us/azure/storage/file-sync/file-sync-introduction) enables you to cache several Azure Files shares on an on-premises Windows Server or cloud virtual machine. You can use Azure File Sync to centralize your organization's file shares in Azure Files, while keeping the flexibility, performance, and compatibility of an on-premises file server.

![Illustration that depicts how Azure File Sync can be used to cache an organization's file shares in Azure Files.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-files-file-sync/media/file-sync-1d3fd2e7.png)

### Things to know about Azure File Sync

Let's take a look at the characteristics of Azure File Sync.

- Azure File Sync transforms Windows Server into a quick cache of your Azure Files shares.
    
- You can use any protocol that's available on Windows Server to access your data locally with Azure File Sync, including SMB, NFS, and FTPS.
    
- Azure File Sync supports as many caches as you need around the world.
    

#### Cloud tiering

Cloud tiering is an optional feature of Azure File Sync. Frequently accessed files are cached locally on the server while all other files are tiered to Azure Files based on policy settings.

- When a file is tiered, Azure File Sync replaces the file locally with a pointer. A pointer is commonly referred to as a _reparse point_. The reparse point represents a URL to the file in Azure Files.
    
- When a user opens a tiered file, Azure File Sync seamlessly recalls the file data from Azure Files without the user needing to know that the file is stored in Azure.
    
- Cloud tiering files have greyed icons with an offline `O` file attribute to let the user know when the file is only in Azure.
    

### Things to consider when using Azure File Sync

There are many advantages to using Azure File Sync. Consider the following scenarios, and think about how you can use Azure File Sync with your Azure Files shares.

- **Consider application lift and shift**. Use Azure File Sync to move applications that require access between Azure and on-premises systems. Provide write access to the same data across Windows Servers and Azure Files.
    
- **Consider support for branch offices**. Support your branch offices that need to back up files by using Azure File Sync. Use the service to set up a new server that connects to Azure storage.
    
- **Consider backup and disaster recovery**. After you implement Azure File Sync, Azure Backup backs up your on-premises data. Restore file metadata immediately and recall data as needed for rapid disaster recovery.
    
- **Consider file archiving with cloud tiering**. Azure File Sync stores only recently accessed data on local servers. Implement cloud tiering so non-used data moves to Azure Files.

---

# Create an Azure Storage account

# Introduction

Most organizations have diverse requirements for their cloud-hosted data. For example, storing data in a specific region, or needing separate billing for different data categories. Azure storage accounts let you formalize these types of policies and apply them to your Azure data.

Suppose you work at a chocolate manufacturer that produces baking ingredients such as cocoa powder and chocolate chips. You market your products to grocery stores who then sell them to consumers.

Your formulations and manufacturing processes are trade secrets. The spreadsheets, documents, and instructional videos that capture this information are critical to your business and require geographically redundant storage. This data is primarily accessed from your main factory, so you would like to store it in a nearby datacenter. The expense for this storage needs to be billed to the manufacturing department.

You also have a sales group that creates cookie recipes and baking videos to promote your products to consumers. Your priority for this data is low cost, rather than redundancy or location. This storage must be billed to the sales team.

By creating multiple Azure storage accounts, with each one having the appropriate settings for the data it holds, you can handle these types of business requirements.

## Learning objectives

In this module, you will:

- Decide how many storage accounts you need for your project.
- Determine the appropriate settings for each storage account.
- Create a storage account using the Azure portal.

# Decide how many storage accounts you need

Organizations often have multiple storage accounts to enable them to implement different sets of requirements. In the chocolate-manufacturer example, there's one storage account for private business data and one storage account for consumer-facing files. In this unit, you learn the policy factors that each type of storage account controls, which helps you decide how many accounts you need.

## What is Azure Storage?

Azure provides many ways to store your data, including multiple database options like Azure SQL Database, Azure Cosmos DB, and Azure Table Storage. Azure offers multiple ways to store and send messages, such as Azure Queues and Event Hubs. You can even store loose files using services like Azure Files and Azure Blobs.

Azure groups four of these data services together under the name _Azure Storage_. The four services are:

- Azure Blobs
- Azure Files
- Azure Queues
- Azure Tables

The following illustration shows the elements of Azure Storage.

![Illustration identifying the Azure data services that are part of Azure Storage.](https://learn.microsoft.com/en-us/training/modules/create-azure-storage-account/media/2-azure-storage.png)

These four data services are all primitive, cloud-based storage services, and are often used together in the same application.

## What is a storage account?

A _storage account_ is a container that groups a set of Azure Storage services together. Only data services from Azure Storage can be included in a storage account (Azure Blobs, Azure Files, Azure Queues, and Azure Tables). The following illustration shows a storage account containing several data services.

![Illustration of an Azure storage account containing a mixed collection of data services.](https://learn.microsoft.com/en-us/training/modules/create-azure-storage-account/media/2-what-is-a-storage-account.png)

Combining data services into a single storage account enables you to manage them as a group. The settings you specify when you create the account, or any changes that you make after creation, apply to all services in the storage account. Deleting a storage account deletes all of the data stored inside it.

A storage account is an Azure resource and is part of a resource group. The following illustration shows an Azure subscription containing multiple resource groups, where each group contains one or more storage accounts.

![Illustration of an Azure subscription containing multiple resource groups, each with one or more storage accounts.](https://learn.microsoft.com/en-us/training/modules/create-azure-storage-account/media/2-resource-groups-and-storage-accounts.png)

Other Azure data services, such as Azure SQL and Azure Cosmos DB, are managed as independent Azure resources and can't be included in a storage account. The following illustration shows a typical arrangement: Blobs, Files, Queues, and Tables are contained within storage accounts, while other services aren't.

![Illustration of an Azure subscription showing some data services that cannot be placed in a storage account.](https://learn.microsoft.com/en-us/training/modules/create-azure-storage-account/media/2-typical-subscription-organization.png)

## Storage account settings

A storage account defines a policy that applies to all the storage services in the account. For example, you could specify that all the contained services will be stored in the West US datacenter, accessible only over https, and billed to the sales department's subscription.

A storage account defines the following settings:

- **Subscription**: The Azure subscription that the storage account services are billed to.
    
- **Location**: The datacenter that stores the services in the account.
    
- **Performance**: Determines the data services you can have in your storage account and the type of hardware disks used to store the data.
    
    - **Standard** allows you to have any data service (Blob, File, Queue, Table) and uses magnetic disk drives.
    - **Premium** provides more services for storing data. For example, storing unstructured object data as block blobs or append blobs, and specialized file storage used to store and create premium file shares. These storage accounts use solid-state drives (SSD) for storage.
- **Replication**: Determines the strategy used to make copies of your data to protect against hardware failure or natural disaster. At a minimum, Azure automatically maintains three copies of your data within the datacenter associated with the storage account. The minimum replication is called locally redundant storage (LRS), and guards against hardware failure but doesn't protect you from an event that incapacitates the entire datacenter. You can upgrade to one of the other options such as geo-redundant storage (GRS) to get replication at different datacenters across the world.
    
- **Access tier**: Controls how quickly you're able to access the blobs in a storage account. The Hot access tier is optimized for storing frequently accessed or modified data and provides quicker access than Cool, but at increased storage cost. The Cool access tier is optimized for storing infrequently accessed or modified data, and has a lower storage cost. Hot access tier applies only to blobs, and serves as the default value for new blobs.
    
- **Secure transfer required**: A security feature that determines the supported protocols for access. Enabled requires HTTPS, while disabled allows HTTP.
    
- **Virtual networks**: A security feature that allows inbound access requests from only the virtual networks that you specify.
    

## How many storage accounts do you need?

A storage account represents a collection of settings like location, replication strategy, and subscription owner. You need one storage account for each group of settings that you want to apply to your data. The following illustration shows two storage accounts that differ in one setting; that one difference is enough to require separate storage accounts.

![Illustration showing two storage accounts with different settings.](https://learn.microsoft.com/en-us/training/modules/create-azure-storage-account/media/2-multiple-storage-accounts.png)

Typically, your data diversity, cost sensitivity, and tolerance for management overhead determine the number of storage accounts you need.

### Data diversity

Organizations often generate data that differs along several vectors. For example, where the data is consumed, how sensitive it is, which group pays the bills for it, etc. Diversity along any of these vectors can lead to multiple storage accounts. Let's consider two examples:

1. Do you have data that is specific to a country/region? If so, you might want to store the data in a datacenter in that country/region for performance or compliance reasons. You need one storage account for each geographical region.
    
2. Do you have some data that is proprietary and some for public consumption? If so, you could enable virtual networks for the proprietary data and not for the public data. Separating proprietary data and public data requires separate storage accounts.
    

In general, increased diversity means an increased number of storage accounts.

### Cost sensitivity

A storage account by itself has no financial cost; however, the settings you choose for the account do influence the cost of services in the account. Geo-redundant storage costs more than locally redundant storage. Premium performance and the Hot access tier increase the cost of blobs.

You can use multiple storage accounts to reduce costs. For example, you could partition your data into critical and noncritical categories. You could place your critical data into a storage account with geo-redundant storage and put your noncritical data in a different storage account with locally redundant storage.

### Tolerance for management overhead

Each storage account requires some time and attention from an administrator to create and maintain. It also increases complexity for anyone who adds data to your cloud storage. Everyone in an administrator role needs to understand the purpose of each storage account so they add new data to the correct account.

Storage accounts are powerful tools to help you obtain the performance and security you need while minimizing costs. A typical strategy is to start with an analysis of your data. Create partitions that share characteristics like location, billing, and replication strategy. Then, create one storage account for each partition.

# Choose your account settings

The storage account settings that we covered in the previous unit apply to the data services in the account. Here, we discuss the three settings that apply to the account itself, rather than to the data stored in the account:

- Name
- Deployment model
- Account kind

These settings affect how you manage your account and the cost of the services within it.

## Name

Each storage account has a name. The name must be globally unique within Azure, use only lowercase letters and digits, and be between 3 and 24 characters.

## Deployment model

A _deployment model_ is the system Azure uses to organize your resources. The model defines the API that you use to create, configure, and manage those resources. Azure provides two deployment models, _Resource Manager_ and _Classic_. Resource Manager is the current model that uses the Azure Resource Manager API. The Classic model, [which is currently being retired](https://azure.microsoft.com/updates/cloud-services-classic-retirement-announcement-feb2024), was a legacy offering that used the classic deployment model.

Most Azure resources only work with Resource Manager, which makes it easy to decide which model to choose. However, storage accounts, virtual machines, and virtual networks support both, so you must choose one or the other when you create your storage account.

The key feature difference between the two models is their support for grouping. The Resource Manager model adds the concept of a _resource group_, which isn't available in the classic model. A resource group lets you deploy and manage a collection of resources as a single unit.

Microsoft recommends that you use the **Resource Manager** deployment model for all new resources.

## Account kind

Storage account _kind_ is a set of policies that determine which data services you can include in the account and the pricing of those services. There are four kinds of storage accounts:

- **Standard - StorageV2 (general purpose v2)**: the current offering that supports all storage types and all of the latest features.
- **Premium - Page blobs**: Premium storage account type for page blobs only.
- **Premium - Block blobs**: Premium storage account type for block blobs and append blobs.
- **Premium - File shares**: Premium storage account type for file shares only.

Microsoft recommends that you use the **Standard - StorageV2 (general purpose v2)** option for new storage accounts.

The core advice is to choose the **Resource Manager** deployment model and the **Standard - StorageV2 (general purpose v2)** account kind for all your storage accounts. For new resources, there are few reasons to consider the other choices.

# Choose an account creation tool

There are several tools that create a storage account. Your choice is typically based on if you want a GUI and whether you need automation.

## Available tools

The available tools are:

- Azure portal
- Azure CLI (Command-line interface)
- Azure PowerShell
- Management client libraries

The Azure portal provides a GUI with explanations for each setting, which makes it easy to use and helpful for learning about the options.

The other tools in this list all support automation. The Azure CLI and Azure PowerShell let you write scripts, while the management libraries allow you to incorporate the creation into a client app.

## How to choose a tool

Storage accounts are typically based on an analysis of your data, so they tend to be relatively stable. As a result, storage-account creation is usually a one-time operation done at the start of a project. For one-time activities, the portal is the most common choice.

In the rare cases where you need automation, the decision is between a programmatic API or a scripting solution. Scripts are typically faster to create and less work to maintain because there's no need for an IDE, NuGet packages, or build steps. If you have an existing client application, the management libraries might be an attractive choice; otherwise, scripts are a better option.

# Exercise - Create a storage account using the Azure portal

## Verify your account

In this unit, you use the Azure portal to create a storage account for a fictitious southern California surf report web app. The surf report site lets users upload photos and videos of local beach conditions. Viewers of the site use the content to help them choose the beach with the best surfing conditions.

Your list of design and feature goals is:

- Video content must load quickly.
- The site must handle unexpected spikes in upload volume.
- Outdated content must be removed as surf conditions change so the site always shows current conditions.

You decide to buffer uploaded content in an Azure Queue for processing and then transfer it to an Azure Blob for persistent storage. You need a storage account that can hold both queues and blobs while delivering low-latency access to your content.

## Create a storage account using Azure portal

1. Sign in to the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com) using the same account you used to activate the sandbox.
    
2. On the resource menu, or from the **Home** page, select **Storage accounts**. The **Storage accounts** pane appears.
    
3. On the command bar, select **Create.** The **Create a storage account** pane appears.
    
4. On the **Basics** tab, enter the following values for each setting.

| Setting              | Value                                                                                                                                                                                                                                                                                                                                                         |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Project details**  |                                                                                                                                                                                                                                                                                                                                                               |
| Subscription         | Concierge Subscription                                                                                                                                                                                                                                                                                                                                        |
| Resource group       | [sandbox resource group name] from the dropdown list.                                                                                                                                                                                                                                                                                                         |
| **Instance details** |                                                                                                                                                                                                                                                                                                                                                               |
| Storage account name | Enter a unique name. This name is used to generate the public URL to access the data in the account. The name must be unique across all existing storage account names in Azure. Names must have 3 to 24 characters and can contain only lowercase letters and numbers.                                                                                       |
| Region               | Select a location near to you from the dropdown list.                                                                                                                                                                                                                                                                                                         |
| Performance          | _Standard_. This option decides the type of disk storage used to hold the data in the Storage account. Standard uses traditional hard disks, and Premium uses solid-state drives (SSD) for faster access.                                                                                                                                                     |
| Redundancy           | Select _Locally redundant storage (LRS)_ from the dropdown list. In our case, the images and videos quickly become out-of-date and are removed from the site. As a result, there's little value to paying extra for _Geo-redundant storage (GRS)_. If a catastrophic event results in data loss, you can restart the site with fresh content from your users. |

1. Select **Next**. On the **Advanced** tab, enter the following values for each setting.
    
| Setting                                                      | Value                                                                                                                                                                                                                                                                                                                                                                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Security**                                                 |                                                                                                                                                                                                                                                                                                                                                                                                        |
| Require secure transfer for REST API operations              | _Check_. This setting controls whether **HTTP** can be used for the REST APIs that access data in the storage account. Setting this option to _enable_ forces all clients to use **HTTPS**. Most of the time, you want to set secure transfer to _enable_; using HTTPS over the network is considered a best practice.                                                                                 |
| Allow enabling anonymous access on individual containers     | _Check_. Blob containers, by default, don't permit anonymous access to their content. This setting allows authorized users to selectively enable anonymous access on specific containers.                                                                                                                                                                                                              |
| Enable storage account key access                            | _Check_. We want to allow clients to access data via SAS.                                                                                                                                                                                                                                                                                                                                              |
| Default to Microsoft Entra authorization in the Azure portal | _Uncheck_. Clients are public, not part of an Active Directory.                                                                                                                                                                                                                                                                                                                                        |
| Minimum TLS version                                          | Select _Version 1.2_ from dropdown list. TLS 1.2 is a secure version of TLS, and Azure Storage uses it on public HTTPS endpoints. TLS 1.1 and 1.0 are supported for backwards compatibility. See _Warning_ at end of table.                                                                                                                                                                            |
| Permitted scope for copy operations                          | Accept default                                                                                                                                                                                                                                                                                                                                                                                         |
| **Hierarchical Namespace**                                   |                                                                                                                                                                                                                                                                                                                                                                                                        |
| Enable hierarchical namespace                                | _Uncheck_. Data Lake hierarchical namespace is for big-data applications that aren't relevant to this module.                                                                                                                                                                                                                                                                                          |
| **Access protocols**                                         |                                                                                                                                                                                                                                                                                                                                                                                                        |
| Enable SFTP                                                  | Accept default.                                                                                                                                                                                                                                                                                                                                                                                        |
| Enable network file system v3                                | Accept default.                                                                                                                                                                                                                                                                                                                                                                                        |
| **Blob storage**                                             |                                                                                                                                                                                                                                                                                                                                                                                                        |
| Allow cross-tenant replication                               | _Uncheck_. Active Directory isn't being used for this exercise.                                                                                                                                                                                                                                                                                                                                        |
| Access tier                                                  | _Hot_. This setting is only used for Blob storage. The _Hot_ access tier is ideal for frequently accessed data; the _Cool_ access tier is better for infrequently accessed data. This setting only sets the _default_ value. When you create a Blob, you can set a different value for the data. In our case, we want the videos to load quickly, so we use the high-performance option for our blobs. |
| **Azure Files**                                              |                                                                                                                                                                                                                                                                                                                                                                                                        |
| Enable large file shares                                     | Accept default. Large file shares provide support up to a 100 TiB, however this type of storage account can't convert to a Geo-redundant storage offering, and upgrades are permanent.                                                                                                                                                                                                                 |
    
     Warning
    
    If _Enable large file shares_ is selected, it will enforce additional restrictions, and Azure files service connections without encryption will fail, including scenarios using SMB 2.1 or 3.0 on Linux. Because Azure storage doesn't support SSL for custom domain names, this option cannot be used with a custom domain name.
    
6. Select **Next**. On the **Networking** tab, enter the following values for each setting.
    
    Expand table
    
    |Setting|Value|
    |---|---|
    |**Network connectivity**||
    |Network access|_Enable public access from all networks_. We want to allow public Internet access. Our content is public facing, and we need to allow access from public clients.|
    |**Network routing**||
    |Routing preference|_Microsoft network routing_. We want to make use of the Microsoft global network that is optimized for low-latency path selection.|
    
7. Select **Next**. On the **Data protection** tab, enter the following values for each setting.
    
    Expand table
    
    |Setting|Value|
    |---|---|
    |**Recovery**||
    |Enable point-in-time restore for containers|_Uncheck_. Not necessary for this implementation.|
    |Enable soft delete for blobs|_Uncheck_. Soft delete lets you recover blob data in cases where blobs or blob snapshots are deleted accidentally or overwritten.|
    |Enable soft delete for containers|_Uncheck_. Soft delete lets you recover your containers that are deleted accidentally.|
    |Enable soft delete for file shares|_Uncheck_. File share soft delete lets you recover your accidentally deleted file share data more easily.|
    |**Tracking**||
    |Enable versioning for blobs|_Uncheck_. Not necessary for this implementation.|
    |Enable blob change feed|_Uncheck_. Not necessary for this implementation.|
    |**Access control**||
    |Enable version-level immutability support|_Uncheck_. Not necessary for this implementation.|
    
8. Select **Next**. Accept the defaults on the **Encryption** tab.
    
9. Select **Next**. Here on the **Tags** tab, you can associate key/value pairs with the account for your categorization to determine if a feature is available to selected Azure resources.
    
10. Select **Next** to validate your options and to ensure all the required fields are selected. If there are issues, this tab identifies them so you can correct them.
    
11. When validation passes successfully, select **Create** to deploy the storage account.
    
12. When deployment is complete, which can take up to two minutes, select **Go to resource**. View the details about your new storage account in the **Essentials** section.
    

You created a storage account with settings driven by your business requirements. For example, you might select a West US datacenter because your customers are primarily located in southern California. The typical flow for creating a storage account is: first analyze your data and goals, and then configure the storage account options to match.

---

# Upload, download, and manage data with Azure Storage Explorer

# Introduction

Azure Storage Explorer makes it easy to access and edit data in Azure.

Suppose you work for an enterprise that has developed a customer relationship management (CRM) application. The application writes data to Azure Data Lake Storage. Your software engineers occasionally need to view stored data, upload new data, and administer these storage services. They'd like to have a user-friendly tool for these activities.

In this module, you'll learn about the features of Azure Storage Explorer, how to install it, and what it can connect to. Finally, you'll use Storage Explorer to connect to Data Lake Storage to create a database and upload data.

By the end of this module, you'll know how to use Storage Explorer to manipulate data in multiple Azure services.

## Learning objectives

- Identify the features of Azure Storage Explorer
- Install Storage Explorer
- Use Storage Explorer to connect to Azure Storage services and manipulate stored data

## Prerequisites

- Basic knowledge of Azure Storage and Azure Data Lake Storage
- Ability to install software locally

# Connect Azure Storage Explorer to a storage account

Storage accounts provide a flexible solution that keeps data as files, tables, and messages. With Azure Storage Explorer, it's easy to read and manipulate this data.

You want to enable your engineers to manage the data stored in Azure Storage so they can maintain the data that your CRM application uses. You want to assess whether they can use Storage Explorer for this purpose.

Here, you'll learn about Storage Explorer and how you can use it to manage data from multiple storage accounts and subscriptions. You'll learn different ways of using Storage Explorer to connect to your data, Azure Stack, and data held in Azure Data Lake Storage.

## What is Storage Explorer?

Storage Explorer is a GUI application that Microsoft developed to simplify accessing and managing data stored in Azure Storage accounts. Storage Explorer is available on Windows, macOS, and Linux.

Some of the benefits of using Storage Explorer are:

- It's easy to connect to and manage multiple storage accounts.
- The interface lets you connect to Data Lake Storage.
- You can also use the interface to update and view entities in your storage accounts.
- Storage Explorer is free to download and use.

With Storage Explorer, you can use a range of storage and data-operation tasks on any of your Azure Storage accounts. These tasks include edit, download, copy, and delete.

### Supported software versions

The latest version of the Azure Storage Explorer application runs on the following versions of these platforms:

Expand table

|Operating system|Versions|
|---|---|
|Windows|Windows 11 and Windows 10|
|macOS|macOS 10.15 Catalina and later|
|Linux|Ubuntu 20.04 x64, Ubuntu 18.04 x64, or Ubuntu 16.04 x64|

### Azure Storage types

Azure Storage Explorer can access many different data types from services like these:

- **Azure Blob Storage**: Blob storage is used to store unstructured data as a binary large object (blob).
- **Azure Table Storage**: Table storage is used to store NoSQL, semi-structured data.
- **Azure Queue Storage**: Queue storage is used to store messages in a queue, which applications can then access and process through HTTP(S) calls.
- **Azure Files**: Azure Files is a file-sharing service that enables access through the Server Message Block protocol, similar to traditional file servers.
- **Azure Data Lake Storage**: Azure Data Lake, based on Apache Hadoop, is designed for large data volumes and can store unstructured and structured data. Azure Data Lake Storage Gen2 is Azure Blob Storage with the hierarchical namespace feature enabled on the account.

### Manage multiple storage accounts in multiple subscriptions

If you have multiple storage accounts across multiple subscriptions in your Azure tenant, managing them through the Azure portal can be time consuming. Storage Explorer lets you manage the data stored in multiple Azure Storage accounts and across Azure subscriptions.

### Use local emulators

During the development phase of your project, you might not want developers to incur additional costs by using Azure Storage accounts. In those cases, you can use a locally based emulator. Storage Explorer supports two emulators: Azure Storage Emulator and Azurite.

- Azure Storage Emulator uses a local instance of Microsoft SQL Server 2012 Express LocalDB. It emulates Azure Table, Queue, and Blob storage.
- Azurite, which is based on Node.js, is an open-source emulator that supports most Azure Storage commands through an API.

Storage Explorer requires the emulator to be running before you open it. Connecting to your emulator is no different than connecting to Azure Storage accounts, except that you'll choose the **Attach to a local emulator** connection type.

All locally emulated storage connection types appear in **Local & Attached** > **Storage accounts**.

## Connecting Storage Explorer to Azure

There are several ways to connect your Storage Explorer application to your Azure Storage accounts.

You need two permissions to access your Azure Storage account: management and data. However, you can use Storage Explorer with only the data-layer permission. The data layer requires the user to be granted, at a minimum, a read data role. The nature of the read/write role should be specific to the type of data stored in the storage account. The data layer is used to access blobs, containers, and other data resources.

The management role grants access to view lists of your various storage accounts, containers, and service endpoints.

### Connection types

There are many ways to connect an Azure Storage Explorer instance to your Azure resources. For example:

- Add resources by using Microsoft Entra ID
- Use a connection string
- Use a shared access signature URI
- Use a name and key
- Attach to a local emulator
- Attach to Azure Data Lake Storage by using a URI

We'll explore a few of these connection types and provide an overview of the required steps to set up the connection.

### Add an Azure account by using Microsoft Entra ID

Use this connection type when the user can access the data layer. You can use it only to create a container. Connecting to Azure Storage through Microsoft Entra ID requires more configuration than the other methods. The account that you use to connect to Azure must have the correct permissions and authorization to access the target resources.

To add a resource by using Microsoft Entra ID:

1. Open Storage Explorer.
2. Select the **Sign in with Azure** option and sign in to Azure.
3. Connect to your Azure Storage account.
4. Select **Add a resource via Microsoft Entra ID**, and then choose the Azure tenant and the associated account.
5. When you're prompted, provide the type of resource that you're connecting to.
6. Review and verify the connection details, and then select **Connect**.

It's crucial to select the correct resource type because it changes the information that you need to enter.

Any connections that you create through this approach will appear in the resource tree, in this branch: **Local & attached** > **Storage Accounts** > **Attached Containers** > **Blob**.

### Connect by using a shared access signature URI

A shared access signature (SAS) URI is an unambiguous identifier that's used to access your Azure Storage resources.

With this connection method, you'll use a SAS URI for the required storage account. You'll need a SAS URI whether you want to use a file share, table, queue, or blob container. You can get a SAS URI either from the Azure portal or from Storage Explorer. For more information, see [Create an account SAS](https://learn.microsoft.com/en-us/rest/api/storageservices/create-account-sas?redirectedfrom=MSDN).

To add a SAS connection:

1. Open Storage Explorer.
2. Connect to your Azure Storage account.
3. Select the connection type: **Shared access signature URI (SAS)**.
4. Provide a meaningful name for the connection.
5. Provide the Service URL.
6. Review and verify the connection details, and then select **Connect**.

When you've added a connection, it appears in the resource tree as a new node. You'll find the connection node in this branch: **Local & attached** > **Storage Accounts** > **Attached Container** > **Service**.

### Connect by using a storage account name and key

To connect to a storage account on Azure quickly, you use the account key that's associated with the storage. To find the storage access keys from the Azure portal, go to the correct storage account page and select **access keys**.

To add a connection:

1. Open Storage Explorer.
2. Connect to your Azure Storage account.
3. Select the connection type: **Storage account name and key**.
4. Provide a meaningful name for the connection.
5. When you're prompted, provide the name of the storage account and either of the account keys needed to access it.
6. From the provided list, select the storage domain that you want to use.
7. Review and verify the connection details, and then select **Connect**.

When the connection is added, it appears in the resource tree as a connection node. The connection node is in this branch: **Local & attached** > **Storage Accounts**.

# Exercise - Connect Azure Storage Explorer to a storage account

It's easy to browse through the contents of an Azure Storage account by using Azure Storage Explorer.

Now that you have a better understanding of the features and capabilities of Storage Explorer, you can try it for yourself. Use Storage Explorer to explore some of the files that your CRM system stores in Azure Storage.

Here, you'll try Storage Explorer by downloading, installing, and connecting to an Azure Storage account. You'll create a blob and a queue in your storage account.

## Download and install Azure Storage Explorer

First, you need to download and install Storage Explorer.

1. Browse to the [Azure Storage Explorer website](https://azure.microsoft.com/products/storage/storage-explorer/?azure-sandbox=true).
    
2. Select **Download now**, then select your preferred operating system. The following steps will go through the Windows version of the application. Your steps will be different if you're using a different OS.
    
3. Locate the downloaded file and run it. For the Windows version, use the **StorageExplorer.exe** file.
    
4. Accept the license agreement and select **Install**.
    
5. Browse to the location where you want to install Storage Explorer or accept the default location. Select **Next**.
    
6. For Windows installations, select the **Start menu** folder. Accept the default and select **Next**.
    
7. When the installation is complete, select **Finish**.
    

Storage Explorer automatically opens after installation.

## Connect to an Azure account

When you first open Storage Explorer, it displays the **Connect to Azure Storage** wizard.

1. First, select **Connect to Azure resources**, and then choose **Subscription**.
    
    ![Screenshot that shows the Select resource screen in the Azure Storage wizard.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/3-connect-resource.png)
    
2. There are several **Azure environment** options to select from. Select **Azure**, then select **Next**.
    
    ![Screenshot that shows the Select Azure environment screen in the Connect to Azure Storage wizard.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/3-storage-explorer-connect.png)
    
3. Your browser opens and an Azure sign-in page appears. Use your Azure credentials to sign in.
    
    ![Screenshot that shows the Azure sign-in page.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/3-storage-explorer-azure-sign-in.png)
    
4. When you've signed in to your Azure instance, the associated Azure account and Azure subscription appear in the **Account Management** section.
    
    ![Screenshot that shows the account management panel after signing in to an Azure account.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/3-account-panel-subscriptions-apply.png)
    
    Confirm that the **Concierge Subscription** subscription is selected and account details are correct, and then select **Open Explorer**.
    

You've now connected Storage Explorer to your Azure subscription. Leave Storage Explorer open while you work through the next steps.

## Create a storage account and add a blob

1. In Azure Cloud Shell, run the following command to create a storage account.
    
    Azure CLICopy
    
    ```
    az storage account create \
    --name mslearn$RANDOM \
    --resource-group "[sandbox resource group name]" \
    --sku Standard_GRS \
    --kind StorageV2
    ```
    
    In the output, note the name of the storage account. After the storage account is created, switch back to Storage Explorer.
    
2. If it isn't currently visible, toggle the **EXPLORER** view so that the pane is shown.
    
3. In the **EXPLORER** pane, select **Refresh All**, then locate and expand **Concierge Subscription**.
    
    ![Screenshot that shows the expansion of Concierge Subscription.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/3-storage-explorer-create-blob-1.png)
    
4. Locate and expand the storage account that you created earlier. It should be named something similar to **mslearn12345**, ending with a different set of numbers. It has four virtual folders: **Blob Containers**, **File Shares**, **Queues**, and **Tables**.
    
     Note
    
    If the storage account you created earlier isn't listed, wait a few moments and select **Refresh All**; it can take a couple of minutes for the account to appear.
    
5. Right-click the **Blob Containers** virtual folder to access the shortcut menu, then select **Create Blob Container**.
    
    ![Screenshot that shows the shortcut menu options for the Blob Containers folder.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/3-storage-explorer-create-blob-context-menu.png)
    
6. Name the container **myblobcontainer** and press Enter.
    
    Each created container appears in a tab to the right of the resource tree.
    
    ![Screenshot that shows the content and details of the new myblobcontainer blob container.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/3-storage-explorer-create-blob-view.png)
    
7. Upload a blob to the container. In the **myblobcontainer** pane, select **Upload**, then select **Upload Files**. The **Upload Files** dialog box appears.
    
8. For **Selected files**, select the ellipsis (**...**). Browse to a small file on your device and select **Open**. Select **Upload** to upload the file.
    
    ![Screenshot that shows the Upload Files dialog box.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/3-upload-blob.png)
    
    You should now see your file stored in your storage account.
    
    ![Screenshot that shows the file in the storage account.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/3-upload-blob-complete.png)
    

From here, you can upload additional files, download files, make copies, and do other administrative tasks.

## Create a queue in your Azure Storage account

To create a queue in your storage account:

1. In the resource tree, find **Concierge Subscription** and expand the options.
    
2. Expand the **cloudshell** storage account.
    
3. Right-click the **Queues** virtual folder to access the shortcut menu, then select **Create Queue**.
    
4. An empty and unnamed queue is created inside the **Queues** folder. The queue won't be created until you give it a name.
    
     Note
    
    Containers have specific rules that govern how they can be named. They must begin and end with either a letter or a number, must be all lowercase, and can have numbers and hyphens. The name can't contain a double hyphen.
    
    Name this new queue **myqueue** and press Enter to create the queue. Each created queue appears on a tab to the right of the resource tree.
    
    ![Screenshot that shows the content and details of the new myblob blob container.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/3-storage-explorer-create-queue-view.png)
    
    From this view, you can manage the queue's content. If our application used this queue and experienced an issue with processing a message, you could connect to the queue and view the message contents to determine the issue.

# Connect Azure Storage Explorer to Azure Data Lake Storage

Azure Storage Explorer doesn't just access Azure Storage. It can also access data in Azure Data Lake Storage.

## Use Storage Explorer to manage Data Lake Storage

You've worked through the basics of connecting Storage Explorer to your Azure account. In the CRM system, your developers use Data Lake Storage for big-data storage. You want to use Storage Explorer to connect to it.

Azure Data Lake Storage is used for storing and analyzing large data sets. It supports large data workloads. It's well suited to capture data of any type or size, and at any speed. Data Lake Storage features all the expected enterprise-grade capabilities like security, scalability, reliability, manageability, and availability.

There are two types of Azure Data Lake Storage: Gen1 and Gen2. Azure Data Lake Storage Gen1 has been retired; follow the [Azure Data Lake Storage Gen1 migration guidance](https://azure.microsoft.com/updates/action-required-switch-to-azure-data-lake-storage-gen2-by-29-february-2024/) for existing Gen1 accounts.

You can use Storage Explorer to connect to Data Lake accounts. Just like with storage accounts, you can use Storage Explorer to:

- Create, delete, and manage containers.
- Upload, manage, and administer blobs.

Let's create Azure Data Lake Storage, then use Storage Explorer to connect to it.


# Exercise - Connect Azure Storage Explorer to Azure Data Lake Storage

Azure Storage Explorer isn't just about storage accounts. You can also use it to investigate and download data from Azure Data Lake Storage.

You've learned how simple creating and managing blob and queue resources in your Azure Storage account is. Now you want to push your understanding further and learn how the storage account connects to your developers' data lake, which they use to store infrastructure data for the CRM system.

Azure Data Lake Storage Gen2 isn't a dedicated service or account type. It's a set of capabilities that you unlock by enabling the hierarchical namespace feature of an Azure Storage account.

Here, you'll learn how to use Storage Explorer to connect to Azure Data Lake Storage Gen2, create a container, and upload data into it.

## Create a storage account with Azure Data Lake Storage Gen2 capabilities

Let's look at connecting to a Data Lake Storage Gen2-enabled account. Before you can use Storage Explorer to manage your Data Lake Storage Gen2-enabled account, you need to create the storage account in Azure.

To create the storage account, use the **az storage account create** command:

Azure CLICopy

```
az storage account create \
    --name dlstoragetest$RANDOM \
    --resource-group [Sandbox resource group] \
    --location westus2 \
    --sku Standard_LRS \
    --kind StorageV2 \
    --hns
```

 Note

Please give the storage account several minutes to complete.

## Connect to your Data Lake Gen2 enabled storage account

Now that you've created a Gen2-enabled storage account, Storage Explorer should automatically connect to it.

1. In Storage Explorer, in the **EXPLORER** pane, locate **Concierge Subscription** and expand it to show all the storage accounts.
    
     Note
    
    It might take several minutes for the storage account to display in Storage Explorer. If you don't see the storage account, wait a few moments and select **Refresh all**.
    
2. You'll see the **dlstoragetest001 (ADLS Gen2)** storage account displayed under the storage accounts. Your account will have a different number suffix.
    
    ![Screenshot that shows the Azure Data Lake Storage Gen2 account.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/5-azure-data-lake-gen2-storage-account.png)
    

### Create a container

All containers in a Data Lake Gen2-enabled storage account are blobs. To create a new container:

1. Expand the **dlstoragetest001** storage account, right-click **Blob Containers**, and select **Create Blob Container** from the shortcut menu.
    
    ![Screenshot that shows the shortcut menu for adding a container.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/5-data-lake-create-blob-container.png)
    
2. Name the new container **myfilesystem**.
    
3. When the container is created, the pane for the container appears. There, you can manage the container contents.
    
    ![Screenshot that shows the myfilesystem control ribbon and view.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/5-data-lake-create-blob-container-view.png)
    

### Upload and view blob data

With the new **myfilesystem** container created, you can now upload files or folders to it.

1. To upload a file, select the **Upload** option, then select **Upload Files**.
    
    ![Screenshot that shows the upload options.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/5-data-lake-container-upload-options.png)
    
2. In the dialog box, use the ellipsis (**...**) to select the file that you want to upload.
    
3. Select the file you want to upload, then select **Open**.
    
4. Select the **Upload** button.
    
5. The file is available to the **myfilesystem** container.
    
    ![Screenshot that shows the uploaded file.](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/media/5-data-lake-file-uploaded.png)
    

You can upload as many files as you want to this folder. Also, you can create an unlimited number of folders. You can then organize and manage the content in your folders as you do with your file system.

---

in addition but unknown

# Choose a data storage approach in Azure

# Introduction

Data comes in different shapes and sizes. No single storage solution fits all data. For example, an online retail website has multiple, distinct datasets that are all used to run the business. Datasets include product catalog data, media files like photos and videos, and financial business data. Each dataset has different requirements. It's your job to figure out which storage solution is best. The key factors to consider in deciding on the optimal storage solution are how to classify your data, how your data will be used, and how you can get the best performance for your application.

## Learning objectives

In this module, you will:

- Classify your data as structured, semi-structured, or unstructured.
- Determine how your data will be used.
- Determine whether your data requires transactions.

# Classify your data

An online retail business has different types of data. Each type of data might benefit from a different storage solution.

Application data can be classified in one of three ways: structured, semi-structured, and unstructured. Here, you'll learn how to classify your data so that you can choose the appropriate storage solution for the type of data.

## Approaches to storing data in the cloud

The following video introduces your options for storing data in the cloud:  
  

### Structured data

In structured data, sometimes called _relational data_, all data has the same fields or properties. All the data has the same organization and shape, or _schema_. The shared schema allows this type of data to be easily searched by using query languages like Structured Query Language (SQL). This capability makes this data style perfect for applications like CRM systems, reservations, and inventory management.

Structured data often is stored in database tables with rows and columns. In the table, a key column indicates how one row in a table relates to data in another row of another table. In the following image, a table that has data about grades gets data from a table of student names and a table of class data by using key columns.

![Diagram that shows two structured data tables and a relationship table that has data that ties them together.](https://learn.microsoft.com/en-us/training/modules/choose-storage-approach-in-azure/media/relational-database.png)

Structured data is straightforward in that it's easy to enter, query, and analyze. All the data is in the same format. However, forcing a consistent structure also means that the evolution of the data is more difficult. If you add or remove data fields, you must update each record to conform to the new structure.

### Semi-structured data

Semi-structured data is less organized than structured data. Semi-structured data isn't stored in a relational format because the fields don't fit neatly into tables, rows, and columns. Semi-structured data contains tags that make the organization and hierarchy of the data apparent. One example is key/value pairs. Semi-structured data is also referred to as non-relational or _not only SQL (NoSQL)_ data.

A data serialization language defines semi-structured data. In data classification, _serialization_ is the process of converting data into a format that can be transmitted or stored.

Software developers use data serialization languages to write data stored in memory to a file, which can then be sent to another system, parsed, and read. The sender and receiver don't need to know details about the other system. Both systems can understand the data if using the same serialization language.  
  

### Common serialization languages

Three common serialization languages are XML, JSON, and YAML.

#### XML

_Extensible Markup Language (XML)_ was one of the first data languages to be widely used. XML is text-based, which makes it easily human-readable and machine-readable. XML parsers are available for almost all popular development platforms.

You can use XML to express relationships. XML has standards for schema, transformation, and even displaying on the web.

Here's an example of a person's name, age, and hobbies expressed in XML:

XMLCopy

```
<Person Age="23">
    <FirstName>Quinn</FirstName>
    <LastName>Anderson</LastName>
    <Hobbies>
        <Hobby Type="Sports">Golf</Hobby>
        <Hobby Type="Leisure">Reading</Hobby>
        <Hobby Type="Leisure">Guitar</Hobby>
   </Hobbies>
</Person>
```

XML expresses the shape of the data by using _tags_ that are defined inside angle braces. The tags come in two forms: _elements_ such as `<FirstName>` and _attributes_ that can be expressed in text like `Age="23"`. Elements can have child elements to express relationships. For example, the `<Hobbies>` tag expresses a collection of `Hobby` elements.

XML is flexible and can express complex data easily. However, it tends to be more verbose, which makes it larger to store, process, and pass over a network. As a result, other formats have become more popular.

#### JSON

_JavaScript Object Notation (JSON)_ has a lightweight specification and uses curly braces to indicate data structure. Compared to XML, JSON is less verbose, and it's easier for humans to read. JSON frequently is used by web services to return data.

Here's the same person's name, age, and hobbies expressed in JSON:

JSONCopy

```
{
    "firstName": "Quinn",
    "lastName": "Anderson",
    "age": "23",
    "hobbies": [
        { "type": "Sports", "value": "Golf" },
        { "type": "Leisure", "value": "Reading" },
        { "type": "Leisure", "value": "Guitar" }
    ]
}
```

The JSON format isn't as formal as XML. It's closer to a key/value pair model than to a formal data expression. As you might guess from the name, the JavaScript programming language has built-in support for this format, so it's popular for web development. Like XML, other languages have parsers you can use to work with this data format. The downside of JSON is that it tends to be more programmer-oriented, so it's harder for non-technical people to read and modify.

#### YAML

_YAML Ain't Markup Language (YAML)_ is a more recently developed data serialization language. One of the benefits of using YAML is that it's easier for humans to read than some other languages. Line separation and indentation define the data structure. The YAML format reduces the dependency on structural characters like parentheses, commas, and brackets.

Here's the same data expressed in YAML:

YAMLCopy

```
firstName: Quinn
lastName: Anderson
age: 23
hobbies:
    - type: Sports
      value: Golf
    - type: Leisure
      value: Reading
    - type: Leisure
      value: Guitar
```

This format is more readable than JSON. Configuration files that people write but programs parse is a common use for it. YAML is the newest of these data formats.

It's often used for configuration files written by people but parsed by programs.

### What is semi-structured or NoSQL data?

The following video describes semi-structured data and NoSQL data storage options:  
  

### Unstructured data

The organization of unstructured data is undefined. Unstructured data is often delivered in file format, such as in photo or video files. The video file itself might have an overall structure and come with semi-structured metadata, but the data that forms the video itself is unstructured. Therefore, photos, videos, and other similar files are classified as unstructured data.

Examples of unstructured data include:

- Media files, like photos, videos, and audio files.
- Microsoft 365 files, like Word documents.
- Text files.
- Log files.

## Data classification: Evaluate your data types

You can classify data in one of three ways: structured, semi-structured, and unstructured. Understanding the differences so that you can classify your data helps you choose the correct storage solution.

Structured data is organized data that neatly fits into tables or columns of data. Semi-structured data is still organized and has clear properties and values, but there's variety to the data. Unstructured data doesn't fit neatly into tables or columns, and it doesn't have a uniform schema.

Let's look at the datasets used in an online retail business and classify them.

### Product catalog data

Product catalog data for an online retail business is semi-structured in nature. Each product has a product SKU, a description, a quantity, a price, size options, color options, a photo, and possibly a video. This data appears relational to begin with because it all has the same structure. However, as you introduce new products or different kinds of products, you might want to add data fields. For example, new tennis shoes you carry are Bluetooth-enabled to relay sensor data from the shoe to a fitness app on the user's phone. This feature appears to be a growing trend, and you want to give customers the option to filter on "Bluetooth-enabled" shoes. You don't want to update all your existing shoe data with a Bluetooth-enabled property. You want to add this new property only to new shoes.

With the addition of the Bluetooth-enabled property, your shoe data is no longer homogenous. You've introduced differences in the schema. If this change is the only exception you expect to encounter, you can normalize the existing data so that all products included a "Bluetooth-enabled" field to maintain a structured, relational organization. However, if it's just one of many specialty fields that you envision supporting in the future, the classification of the data is semi-structured. Tags organize the data, but each product in the catalog can contain unique fields.

The classification for product catalog data is _semi-structured_.

### Photos and videos

The photos and videos displayed on product pages are unstructured data. Although the media file might contain metadata, the body of the media file is unstructured.

The data classification for photos and videos is _unstructured_.

### Business data

Business analysts want to implement business intelligence to perform inventory pipeline evaluations and sales data reviews. To perform these operations, data from multiple months needs to be aggregated and then queried. Because of the need to aggregate similar data, this data must be structured, so that one month can be compared to the next.

The classification for business data is _structured_.

# Determine operational needs

After you've identified the kind of data you want to store (structured, semi-structured, or unstructured), the next step is to determine how you'll use the data. For example, as an online retailer, you know that customers need quick access to product data, and business users need to run complex analytical queries. As you work through these requirements, taking your data classification into account, you can begin to plan your data storage solution.

Here, you'll answer some questions to help you determine what to do with your data.

## Operations and latency

What are the main operations you'll be completing on each data type, and what are the performance requirements for the data?

Ask these questions about your data:

- Will you be doing simple lookups by using an ID field?
- Do you need to query the database for one or more fields?
- How many create, update, and delete operations do you expect to run?
- Do you need to run complex analytical queries?
- How quickly do these operations need to process?

The answers to these questions will help you decide on the best storage solution for your data.

## Operations and latency: Evaluate your data types

Let's walk through each of the datasets with these questions in mind and discuss the requirements.

### Product catalog data

For product catalog data in an online retail scenario, customer needs are the highest priority. Customers will want to query the product catalog to find an item or category they have in mind. For example, a customer might query all tennis shoes, then tennis shoes on sale, and then tennis shoes on sale in a particular size. Customer needs might require many read operations, and they must be able to query on specific fields.

When a customer places an order, the application must update product quantities. The update operations need to happen as quickly as the read operations so that users don't put an item in their shopping carts when that item just sold out. The application must support not only a large number of read operations, but it also requires increased write operations for product catalog data. Be sure to determine the priorities for all the users of the database, not only the primary users.

### Photos and videos

Photos and videos that are displayed on product pages have different requirements. They need fast retrieval times so that they're displayed on the site at the same time as product catalog data, but they don't need to be queried independently. Instead, you can rely on the results of the product query, and include the video ID or URL as a property on the product data. You need to retrieve photos and videos using their IDs only.

Customers won't make updates to existing photos or videos, but they can add new photos for product reviews. For example, a customer might upload an image of them wearing their new shoes.

As an employee, you also upload and delete product photos that are provided by your product vendor. But those updates don't need to happen as quickly as your other product data updates.

In summary, you can query photos and videos by ID to return the entire file. However, create and update operations are less frequent and have lower priority.

### Business data

For data analysis, the company uses only historical data. No original data is updated based on the analysis, so business data is read-only. Users don't expect their complex analytics to run instantly, so it's acceptable to have some latency in the results.

Business data is stored in multiple datasets. Not all business analysts need write access to all datasets, but all business analysts can read from all datasets.

# Group multiple operations in a transaction

If a change to one piece of data must result in a change to another piece of data, an application needs to group together a series of data updates. You can use _transactions_ to group these updates. In a transaction, if one event in a series of updates fails, the entire series can be rolled back or undone.

An example is an online retailer that uses a transaction to place an order, verify payment, and update the product inventory. Grouping the related events ensures that you don't reduce your inventory levels until you receive an approved form of payment.

Next, learn about transactions and whether they're required for your data.

## What is a transaction?

A transaction is a logical group of database operations that run together.

Here's the question to ask yourself regarding whether you need to use transactions in your application: Will a change to one piece of data in your dataset affect another piece of data? If the answer is yes, you'll need support for transactions in your database service.

_ACID guarantees_ define transactions by ensuring a set of four requirements are met. ACID is an acronym for _atomicity_, _consistency_, _isolation_, and _durability_.

- _Atomicity_ means a transaction must run exactly once, and it must be atomic. Either all of the work is done or none of it is. Operations within a transaction usually share a common intent and are interdependent.
- _Consistency_ ensures that the data is consistent both before and after the transaction.
- _Isolation_ ensures that each transaction is unaffected by other transactions.
- _Durability_ means that changes made as a result of a transaction are permanently saved in the system. The system saves data that's committed so even in the event of a failure and system restart, the data is available in its correct state.

When a database offers ACID guarantees, these principles are consistently applied to each transaction.

### OLTP vs. OLAP

Transactional databases are often called _online transaction processing (OLTP)_ systems. OLTP systems commonly support many users, have quick response times, and handle large volumes of data. They also are highly available, which means they have minimal downtime. OLTP systems typically handle small transactions or relatively simple transactions.

An example of an Azure service that supports OLTP is Azure SQL Database.

_Online analytical processing (OLAP)_ systems commonly support fewer users, have longer response times, can be less available, and typically handle large transactions or complex transactions.

An example of an Azure service that supports OLAP is Azure Analysis Services.

The terms OLTP and OLAP aren't used as frequently as they used to be, but understanding them makes it easier to categorize the needs of your application.

## Transactions: Evaluate your data types

Ensuring that your data is in the correct state isn't always an easy task. Transactions can help by enforcing data integrity requirements on your data. If your data benefits from ACID principles, choose a storage solution that supports transactions.

Let's walk through each of the datasets in the online retail scenario and determine the need for transactions.

### Product catalog data

Product catalog data should be stored in a transactional database. When a user places an order and the payment is verified, the item inventory should be updated. Likewise, if the customer's credit card is declined, the order should be rolled back and the inventory shouldn't be updated. These relationships require transactions.

### Photos and videos

Photos and videos in a product catalog don't require transactional support. These files are changed only when an update is made or new files are added. Even though there's a relationship between the image and the actual product data, it's not transactional in nature.

### Business data

Because business data is historical and unchanging, transactional support isn't required. Business analysts who work with the data also have unique query needs. They often work with aggregates in their queries, so they can work with the totals of other smaller data points.

# Choose a storage solution on Azure

Choosing the correct storage solution can lead to better performance, cost savings, and improved manageability. Each type of data has different storage requirements. It's your job to determine which storage solution is best for the types of data your company uses. Always consider the type of data, the operations required, expected latency, and the need for transactional support.

Here, you apply what you've learned about the data in your online retail scenario and find the best Azure service for each dataset.

## Product catalog data

**Data classification**: Semi-structured because of the need to extend or modify the schema for new products.

**Operations**: Customers require a high number of read operations, with the ability to query many fields within the database. Also, the business requires a high number of write operations to track its constantly changing inventory.

**Latency and throughput**: High throughput and low latency.

**Transactional support**: Because product data is tied to payment and inventory, transactional support is required.

### Recommended service: Azure Cosmos DB

Azure Cosmos DB supports semi-structured or NoSQL data by design. So, supporting new fields like the "Bluetooth-enabled" field or any new fields you need in the future is something you can do with Azure Cosmos DB.

Azure Cosmos DB supports SQL for queries, and every property is indexed by default. You can create queries so that your customers can filter on any property in the catalog.

Azure Cosmos DB is also ACID-compliant, so you can be assured that your transactions are completed according to those strict requirements. An OLTP connector is available for Azure Cosmos DB.

As an added plus, you also can use Azure Cosmos DB to easily replicate your data anywhere in the world. If your e-commerce site has users concentrated in the US, France, and England, you can replicate your data to datacenters in those regions. Latency is reduced because you've physically moved the data closer to your users.

Even with data that's replicated around the world, you can choose from one of five consistency levels. By choosing the right consistency level, you can determine the tradeoffs to make between consistency, availability, latency, and throughput. You can scale up to handle higher customer demand during peak shopping times or scale down during slower times to conserve on cost.

#### Why not other Azure services?

Azure SQL Database would be an excellent choice for this dataset if you could identify the subset of properties that are common for most of the products and the variable properties that might not exist in some products. You can use Azure SQL Database to combine structured data in the columns and semi-structured data stored as JSON columns that can be easily extended. Azure SQL Database can provide many of the same benefits of Azure Cosmos DB. However, it provides little benefit if the structure of your data is changing in different entities, and you can't predefine a set of common properties that are repeated in most of the entities. Unlike Azure Cosmos DB, which indexes every property in the documents, Azure SQL Database needs to explicitly define what properties in semi-structured documents should be indexed. Azure Cosmos DB is a better choice for highly unstructured and variable data in which you can't predict what properties should be indexed. Azure SQL Database supports OLTP.

Other Azure services, like Azure Table storage, Apache HBase in Azure HDInsight, and Azure Cache for Redis, can also store NoSQL data. In this scenario, users will want to query on multiple fields, so Azure Cosmos DB is a better fit. Azure Cosmos DB indexes every field by default, whereas other Azure services are limited in the data they index. Querying on non-indexed fields results in reduced performance.

## Photos and videos

**Data classification**: Unstructured.

**Operations**: Retrieve only by ID, customers require a high number of read operations with low latency, and create operations and update operations will be less frequent and can have higher latency than read operations.

**Latency and throughput**: Retrievals by ID need to support low latency and high throughput. Create operations and update operations can have higher latency than read operations.

**Transactional support**: Not required.

### Recommended service: Azure Blob Storage

Azure Blob Storage supports storing files like photos and videos. It also works with Azure Content Delivery Network by caching the most frequently used content, and then storing it on edge servers. Azure Content Delivery Network reduces latency when serving those images to your users.

In Azure Blob Storage, you can also move images from the hot storage tier to the cool storage tier or archive storage tier. This helps reduce costs and focus throughput on the most frequently viewed images and videos.

#### Why not other Azure services?

You could upload your images to Azure App Service, so that the same server running your app serves up your images. This solution would work if you didn't have many files. But if you have lots of files and a global audience, you'll get better performance by using Azure Blob Storage with Azure Content Delivery Network.

## Business data

**Data classification**: Structured.

**Operations**: Read-only, complex analytical queries across multiple databases.

**Latency and throughput**: Some latency in the results is expected based on the complex nature of the queries.

**Transactional support**: Not required.

### Recommended service: Azure SQL Database

Business analysts most likely will query business data using SQL, as they are more knowledgeable in this query language than any other. You can use Azure SQL Database as a solution by itself. However, if you pair it with Azure Analysis Services, data analysts can create a semantic model over the data in Azure SQL Database. Data analysts can then share the model with business users, who need only to connect to the model from any business intelligence (BI) tool to immediately explore the data and gain insights. Azure Analysis Services supports OLAP.

#### Why not other Azure services?

Azure Synapse Analytics supports OLAP solutions and SQL queries, but your business analysts will need to perform cross-database queries, which Azure Synapse Analytics doesn't support.

Azure Stream Analytics is a great way to analyze data and transform it into actionable insights, but its focus is on real-time data that is streaming in. In this scenario, the business analysts are looking at historical data only.

