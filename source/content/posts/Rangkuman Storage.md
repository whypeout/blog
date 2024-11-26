## Storage Account

Storage Data

|Category|Description|Storage examples|
|---|---|---|
|**Virtual machine data**|Virtual machine data storage includes disks and files. Disks are persistent block storage for Azure IaaS virtual machines. Files are fully managed file shares in the cloud.|Storage for virtual machine data is provided through Azure managed disks. Data disks are used by virtual machines to store data like database files, website static content, or custom application code. The number of data disks you can add depends on the virtual machine size. Each data disk has a maximum capacity of 32,767 GB.|
|**Unstructured data**|Unstructured data is the least organized. Unstructured data may not have a clear relationship. The format of unstructured data is referred to as _nonrelational_.|Unstructured data can be stored by using Azure Blob Storage and Azure Data Lake Storage. Blob Storage is a highly scalable, REST-based cloud object store. Azure Data Lake Storage is the Hadoop Distributed File System (HDFS) as a service.|
|**Structured data**|Structured data is stored in a relational format that has a shared schema. Structured data is often contained in a database table with rows, columns, and keys. Tables are an autoscaling NoSQL store.|Structured data can be stored by using Azure Table Storage, Azure Cosmos DB, and Azure SQL Database. Azure Cosmos DB is a globally distributed database service. Azure SQL Database is a fully managed database-as-a-service built on SQL.|

Tipe:
- **Standard** : Slow HDD, Cheap cost per GB, infrequently accessed
- **Premium** : Fast SSD low latency . az vm disk i/o intensive like db

Services:
- **Azure Blob Storage (containers)**: A massively scalable object store for text and binary data.
- **Azure Files**: Managed file shares for cloud or on-premises deployments.
- **Azure Queue Storage**: A messaging store for reliable messaging between application components.
- **Azure Table Storage**: stores nonrelational structured data (structured NoSQL data).

AZ Blob storage:
- Serving images or documents directly to a browser.
- Storing files for distributed access.
- Streaming video and audio.
- Storing data for backup and restore, disaster recovery, and archiving.
- Storing data for analysis by an on-premises or Azure-hosted service.

AZ Files: akses via Shares (SMB) or NFS. or via REST

AZ Queue Storage : is used to store and retrieve messages. Queue messages can be up to 64 KB in size, and a queue can contain millions of messages. Queues are used to store lists of messages to be processed asynchronously.

AZ Table Storage: is a service that stores non-relational structured data (also known as structured NoSQL data) in the cloud, providing a key/attribute store with a schemaless design

Storage Account Types


| Storage account                                                                                                       | Supported services                                                                        | Recommended usage                                                                                                                                                                                  |
| --------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [**Standard** **general-purpose v2**](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-upgrade) | Blob Storage (including Data Lake Storage), Queue Storage, Table Storage, and Azure Files | Standard storage account : blobs, file shares, queues, tables, and disks (page blobs).                                                                                                             |
| [**Premium** **block blobs**](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-block-blob-premium)  | Blob Storage (including Data Lake Storage)                                                | Premium storage account for block blobs and append blobs.                                                                                                                                          |
| [**Premium** **file shares**](https://learn.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share) | Azure Files                                                                               | Premium storage account for file shares only. Server Message Block (SMB) and NFS file shares.                                                                                                      |
| [**Premium** **page blobs**](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-pageblob-overview)    | Page blobs only                                                                           | Premium high-performance storage account for page blobs only. ideal for storing index-based and sparse data structures, such as operating systems, data disks for virtual machines, and databases. |

URL Access Storage

|Service|Default endpoint|
|---|---|
|**Container service**|`//`**`mystorageaccount`**`.blob.core.windows.net`|
|**Table service**|`//`**`mystorageaccount`**`.table.core.windows.net`|
|**Queue service**|`//`**`mystorageaccount`**`.queue.core.windows.net`|
|**File service**|`//`**`mystorageaccount`**`.file.core.windows.net`|
To access the _myblob_ data in the _mycontainer_ location in your storage account, we use the following URL address:

`//`**`mystorageaccount`**`.blob.core.windows.net/`**`mycontainer`**`/`**`myblob`**.


AZ Blob Storage

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

![Diagram that shows the Azure Blob Storage architecture.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-blob-storage/media/blob-storage-94fb52b8.png)

Configure Containers:

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

Blob Access Tiers:

- Hot tier: sering dibaca tulis/akses. default. akses murah, storage mahal.
- Cool tier: data besar jarang diakses. 30days. short term backup. cost effective storage, tapi mahal di akses
- Cold tier: data besar jarang diakses. 90days
- Archive tier: offline tier, hours latency. 180days. storage paling murah, tapi paling mahal utk aksesnya

Storage Lifecycle
- cooler storage tier: hot to cool, hot to archive, cool to archive to optimize performance and cost
- delete blob at end lifecycles
- rule based conditions : if (last modified: 30days) move to cool; or (90days) move to archive; or delete blob

Upload blobs
- block blobs: text & binary (files video images audio)
- append blobs: optimized append like logging
- page blobs: upto 8tb. frequent read/write ops. az vm system disks & data disks
- block blobs: default if you not choose.
- blob types cannot be change after its creation

tools to upload blobs:
- azCopy: windows/linux cli to copy to/from blob storage, containers & storage accounts
- azure data box disk: transfer on premise data to blob storage
- az import/export: export data to disk then microsoft ship to you.

![Screenshot of the Upload Blob page that shows the Authentication type, blob types, and block size.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-blob-storage/media/upload-blobs-7ad73d30.png)

blob data pricing:
- performance tiers: cooler = cheaper per gigabytes
- data access cost: hotter = cheaper access cost

## Azure storage security

strategy:
- encryption: encrypted az storage encryption
- authentication: entra id RBAC
- data in transit: secured in transit between app & azure by client-side encrypt, https or smb 3.0
- disk encryption: os & data disk in az vm encrypted w az disk encryption
- shared access signature SAS: delegate access to data in az storage
- authorization: akses harus authorized. 

authorized strategy:
- entra id RBAC
- shared key
- shared access signatures: specified time interval and permission
- anonymous access to containers and blobs (public read)

create SAS
- specified delegate to multiple az storage services, blobs files queues tables
- specified delegate time intervals which valid start time until expired time
- delegate permissions like read and write but not delete
- account level & service level control:
	- account level: delegate access to resources one/more storage services
	- service level: delegate access to resources only one az storage services
optional config:
- ip address: /32 or ranges to accept access.
- protocols: restricts to only HTTPS
![Screenshot of the Create a shared access signature key page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-storage-security/media/configure-secure-signatures-be02fa89.png)

must:
- signing method: account key/user delegation key
- signing key: select key
- permissions: read, write, read-write
- start & expire date

```
https://myaccount.blob.core.windows.net/?restype=service&comp=properties&sv=2015-04-05&ss=bf&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B

# storage resource
https://myaccount.blob.core.windows.net/?restype=service&comp=properties

# sas token
# storage version
&sv=2015-04-05 
# storage service
&ss=bf
# start time
&st=2015-04-29T22%3A18%3A26Z
# expire time
&se=2015-04-30T02%3A23%3A26Z
# resource
&sr=b
# permission
&sp=rw
# ip ranges
&sip=168.1.5.60-168.1.5.70
# protocols
&spr=https
# signature
&sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B
```

encryption
- customer-managed keys: use az key vault to store
  ![Screenshot that shows how to create a customer-managed key.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-storage-security/media/customer-keys-b24acc48.png)
- microsoft-managed keys


## Az File Sync


## Azure Storage account

![Illustration identifying the Azure data services that are part of Azure Storage.](https://learn.microsoft.com/en-us/training/modules/create-azure-storage-account/media/2-azure-storage.png)

A _storage account_ is a container that groups a set of Azure Storage services together. Only data services from Azure Storage can be included in a storage account (Azure Blobs, Azure Files, Azure Queues, and Azure Tables)

![Illustration of an Azure storage account containing a mixed collection of data services.](https://learn.microsoft.com/en-us/training/modules/create-azure-storage-account/media/2-what-is-a-storage-account.png)

![Illustration of an Azure subscription containing multiple resource groups, each with one or more storage accounts.](https://learn.microsoft.com/en-us/training/modules/create-azure-storage-account/media/2-resource-groups-and-storage-accounts.png)

![Illustration of an Azure subscription showing some data services that cannot be placed in a storage account.](https://learn.microsoft.com/en-us/training/modules/create-azure-storage-account/media/2-typical-subscription-organization.png)

SA settings:
-  **Subscription**: storage account services are billed to.
- **Location**: The datacenter that stores the services in the account.
- **Performance**: Determines the type of hardware disks used
    - **Standard** allows you to have any data service (Blob, File, Queue, Table) and uses magnetic disk drives.
    - **Premium** provides more services for storing data. storing unstructured object data as block blobs or append blobs, and specialized file storage used to store and create premium file shares. These storage accounts use solid-state drives (SSD)
- **Replication**: Determines the strategy used to make copies of your data to protect against hardware failure or natural disaster.
- **Access tier**: Controls how quickly you're able to access the blobs in a storage account.
- **Secure transfer required**: A security feature that determines the supported protocols for access. Enabled requires HTTPS, while disabled allows HTTP.
- **Virtual networks**: A security feature that allows inbound access requests from only the virtual networks that you specify.

## Account kind

Storage account _kind_ is a set of policies that determine which data services you can include in the account and the pricing of those services. There are four kinds of storage accounts:

- **Standard - StorageV2 (general purpose v2)**: the current offering that supports all storage types and all of the latest features.
- **Premium - Page blobs**: for page blobs only.
- **Premium - Block blobs**: for block blobs and append blobs.
- **Premium - File shares**: for file shares only.

Microsoft recommends that you use the **Standard - StorageV2 (general purpose v2)** option for new storage accounts.

---

