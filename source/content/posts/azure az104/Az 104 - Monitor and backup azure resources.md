---
draft: false
date: 2024-11-14
slug: Az 104 - Monitor and backup azure resources
tags:
  - azure
  - az104
---

# Introduction to Azure Backup

# Introduction

Information technology workers understand the importance of data to the organization. The need to protect that data drives decisions around storage, backups, and security. Many companies implement policies that dictate backup specifications for frequency, duration of backup storage, and restore policies.

For on-premises scenarios, backup solutions might have included local redundant storage solutions or off-site storage. Scenarios using backup to tape drives and storing offsite come with the resulting delay in restoring the data (because of the need to transport the tapes back to the server rooms and from performing the restore operation). This can result in significant downtime.

These backup solutions might not always address some of the most important considerations, such as security of the backups, the potential for the company to be impacted by a ransomware attack, or human error in the backup and restore operations. An ideal solution would be cost-effective, simple to use, and secure. This is where Azure Backup comes in.

![Diagram of a backup scenario with a company's servers and workstations on the left, with files and folders, using the Backup Agent to back up the data to Microsoft Azure storage.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-backup/media/architecture-on-premises-mars.png)

Azure Backup can also address scenarios for your Azure environments, with support for:

- Azure VMs
- Azure Managed Disks
- Azure Files
- SQL Server in Azure VMs
- SAP HANA databases in Azure VMs
- Azure Database for PostgreSQL servers
- Azure Blobs
- Azure Database for PostgreSQL - Flexible servers
- Azure Database for MySQL - Flexible servers
- Azure Kubernetes cluster

## Example scenario

You're running an application powered by SQL Server. The database is running in an always-on availability group across three Azure VMs. You want to back up the databases using an Azure native backup service. You're looking to store the backup for 10 years in cheaper storage for your audit and compliance needs. You'd like to monitor the backup jobs daily for all such databases.

![Diagram of an application using a SQL Server backend database and Azure Backup for data backup scenarios.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-backup/media/scenario.png)

## What will we be doing?

We'll evaluate the features and capabilities of Azure Backup to help decide if:

- Azure Backup can offer a solution for your backup needs.
- You can back up and restore the data you need for your organization.
- Azure Backup offers secure storage of your data.

## What is the main goal?

By the end of this session, you'll be able to decide if Azure Backup is the right solution to consider for your data-protection needs.

# What is Azure Backup?

Let's start by defining Azure Backup and taking a quick tour of the core features. This overview should help you decide whether Azure Backup might be a good fit for your data-protection needs.

## What is Azure Backup?

The Azure Backup service provides simple, secure, and cost-effective solutions to back up your data and recover it from the Microsoft Azure cloud.

![Diagram of the Azure Backup service implementing backup agents in the on-premises environment to the cloud. Middle section displays the components of Azure Backup for security and scalability with an underlying bar indicating central management.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-backup/media/azure-backup-overview.png)

## Azure Backup definition

Azure Backup is an Azure service that provides cost effective, secure, and zero-infrastructure backup solutions for all Azure-managed data assets.

The centralized management interface makes it easy to define backup policies and protect a wide range of enterprise workloads, including Azure Virtual Machines, Azure Disks, SQL and SAP databases, Azure file shares, and blobs.

![Diagram of Azure Backup architecture displaying workloads at the bottom, feeding upwards into the data plane, and tying into the management plane. Management contains backup policies, Azure policies, Azure Monitor, and Azure Lighthouse services.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-backup/media/azure-backup-architecture.png)

## When to use Azure Backup

As the IT admin of your organization, you're responsible for meeting the compliance needs for all the data assets of the firm, and backup is a critical aspect. There are also various application admins in your company who need to do self-service backup and restore to take care of issues like data corruption or rogue-admin scenarios. You're looking for an enterprise-class backup solution to protect all your workloads and manage them from a central place.

Azure Backup can provide backup services for the following data assets:

- On-premises files, folders, and system state
- Azure Virtual Machines (VMs)
- Azure Managed Disks
- Azure Files Shares
- SQL Server in Azure VMs
- SAP HANA (High-performance Analytic Appliance) databases in Azure VMs
- Azure Database for PostgreSQL servers
- Azure Blobs
- Azure Database for PostgreSQL - Flexible servers
- Azure Database for MySQL - Flexible servers
- Azure Kubernetes cluster

![Screenshot of Azure Backup center displaying a list of backup jobs. The list displays the backup instance, data source, operation type, and status.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-backup/media/backup-center-jobs.png)

## Key features

Let's look at some key features of Azure Backup.

| Feature                             | Description                                                                                                                                                                                      | Usage                                                                                                                                                                                                                                    |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Zero-infrastructure backup solution | Unlike conventional backup solutions, no backup server or infrastructure is needed. Similarly, no backup storage needs to be deployed, because Azure Backup automatically manages and scales it. | Zero-infrastructure solution eliminates capital expenses and reduces operational expenses. It increases ease of use by automating storage management.                                                                                    |
| At-scale management                 | Natively manage your entire backup estate from a central console called Backup Center. Use APIs, PowerShell, and Azure CLI to automate Backup policy and security configurations.                | Backup center simplifies data protection management at-scale by enabling you to discover, govern, monitor, operate, and optimize backup management from one unified console, which helps you to drive operational efficiency with Azure. |
| Security                            | Azure Backup provides built-in security to your backup environment, both when your data is in transit and at rest by using capabilities encryption, private endpoints, alerts, and so on.        | Your backups are automatically secured against ransomware, malicious admins, and accidental deletions.                                                                                                                                   |

## How do Recovery Time Objective and Recovery Point Objective work?

Recovery Time Objective (RTO) is the target time within which a business process must be restored after a disaster occurs to avoid unacceptable consequences. For instance, if a critical application goes down due to a server failure and the business can only tolerate a maximum of four hours of downtime, then the RTO is four hours.

Recovery Point Objective (RPO) is the maximum amount of data loss, measured in time, that your organization can sustain during an event.

The following example scenario describes both the RPO and RTO concepts:

Your organization has an RPO of one hour for your customer database, which means you perform backups every hour. If a data-loss incident occurs, you lose not more than one hour of data. When you set RTO to three hours, then if a system failure occurs, you aim to restore access to the database within three hours to minimize the impact on operations.

# How Azure Backup works

Let's take a look at how Azure Backup works to provide the data protection you need. Particularly, let's look at how the different aspects of the backup service make it easy to back up various types of data, and how it offers security for your backups as well. In this unit, we cover the following aspects of the Azure Backup Service:

- **Workload integration layer - Backup Extension**: Integration with the actual workload, such as Azure virtual machines (VMs) or Azure Blobs, happens at this layer.
- **Data Plane - Access Tiers**: There are three access tiers where the backups could be stored:
    - Snapshot tier
    - Standard tier
    - Archive tier
- **Data Plane - Availability and Security**: The backup data is replicated across zones or regions, based on the redundancy the user specifies.
- **Management Plane – Recovery Services vault/Backup vault and Backup center**: The vault provides an interface for the user to interact with the backup service.

## What data is backed up and how?

The simplest explanation of Azure Backup is that it backs up data, machine state, and workloads running on on-premises machines and VM instances to the Azure cloud. Azure Backup stores the backed-up data in Recovery Services vaults and Backup vaults.

For **on-premises** Windows machines, you can back up directly to Azure with the **Azure Backup Microsoft Azure Recovery Services (MARS) agent**. Alternatively, you can back up these Windows machines to a backup server, perhaps a **System Center Data Protection Manager (DPM) or Microsoft Azure Backup Server (MABS).** You can then back that server up to a Recovery Services vault in Azure.

If you're using **Azure VMs**, you can back them up **directly**. **Azure Backup** installs a backup extension to the Azure VM agent that's running on the VM, which allows you to back up the entire VM. If you only want to back up the files and folders on the VM, you can do so by running the **MARS agent.**

Azure Backup stores backed-up data in vaults: Recovery Services vaults and Backup vaults. A vault is an online-storage entity in Azure that's used to hold data such as backup copies, recovery points, and backup policies.

### Supported backup types

Azure Backup supports full backups and incremental backups. Your initial backup is a full backup. The incremental backup is used by DPM/MABS use the incremental backup for disk backups, and used in all backups to Azure. As the name suggests, incremental backups only focus on the blocks of data that changed since the previous backup.

Azure Backup also supports SQL Server backup types. The following table outlines the support for SQL Server type backups:

Expand table

|Type|Description|Usage|
|---|---|---|
|Full|A full database backup backs up the entire database. It contains all the data in a specific database or in a set of filegroups or files. A full backup also contains enough logs to recover that data.|At most, you can trigger one full backup per day. You can choose to make a full backup on a daily or weekly interval.|
|Differential|A differential backup is based on the most recent full-data backup. It captures only the data that changed since the full backup.|At most, you can trigger one differential backup per day. You can't configure a full backup and a differential backup on the same day.|
|Multiple backups per day|Back up Azure VMs hourly with a minimum recovery point objective (RPO) of 4 hours and a maximum of 24 hours.|You can use Enhanced backup policy to set the backup schedule to 4, 6, 8, 12, and 24 hours (respectively) for new Azure offerings, such as Trusted Launch VM.|
|Selective disk backup|Selectively back up a subset of the data disks that are attached to your VM, then restore a subset of the disks that are available in a recovery point, both from instant restore and vault tier. Selective disk backup helps you manage critical data in a subset of the VM disks and use database backup solutions when you want to back up only their OS disk to reduce cost.|Azure Backup provides Selective Disk backup and restore capability using Enhanced backup policy.|
|Transaction Log|A log backup enables point-in-time restoration up to a specific second.|At most, you can configure transactional log backups every 15 minutes.|

## Workload integration layer - Backup Extension

A backup extension specific to each workload is installed on the source VM or a worker VM. At the time of backup (as defined by the user in the Backup Policy) the backup extension generates the backup, which could be:

- **Storage**: Snapshots when using an Azure VM or Azure Files.
    
- **Stream backup**: For databases like SQL or High-performance Analytic Appliance (HANA) running in VMs.
    

The backup data is eventually transferred to Azure Backup managed storage in the data plane by using secure Azure networks Network Security Groups (NSG), Firewalls, or more sophisticated private endpoints.

## Data Plane - Access Tiers

There are three access tiers where the backups can be stored:

- **Snapshot tier**: (Workload-specific term) In the first phase of a virtual machine backup, the snapshot is taken and stored along with the disk. This form of storage is referred to as a snapshot tier. Restoring a snapshot tier is faster than restoring from a vault, because it eliminates the wait time for snapshots to be copied from the vault before triggering the restore operation. The snapshots of the VM/Azure Files/Azure Blobs/and so on are retained in the customer's subscription in a specified resource group. This container ensures that restores are quick, because the backup/snapshot is available locally to the customer.
    
- **Vault-standard tier**: Backup data for all workloads supported by Azure Backup is stored in vaults, which hold backup storage, an autoscaling set of storage accounts managed by Azure Backup. The Vault-standard tier is an online storage tier that allows you to store an isolated copy of backup data in a Microsoft-managed tenant, thus creating an extra layer of protection. For workloads where snapshot tier is supported, there's a copy of the backup data in both the snapshot tier and the Vault-standard tier. The Vault-standard tier ensures that backup data is available even if the data source being backed up is deleted or compromised.
    
- **Archive tier**: Customers rely on Azure Backup for storing backup data, including their Long-Term Retention (LTR) backup data, with retention needs defined in the organization's compliance rules. In most cases, the older backup data is rarely accessed and is only stored for compliance needs.
    
    Azure Backup supports backup of long-term retention points in the archive tier.
    

All tiers offer different recovery time objectives (RTO) and are priced differently.

![Diagram of the various workloads such as on-premises server, Azure VMs, Azure files, etc. feeding into the data plane where the access tiers are located.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-backup/media/data-plane.png)

## Data Plane - Availability and Security

The backup data is replicated across zones or regions, based on the redundancy you specify. You can choose from locally redundant storage (LRS), Geo-redundant storage (GRS), or zone-redundant storage (ZRS). These options provide you with highly available data storage capabilities.

The data is kept safe by encrypting it and implementing Azure role-based access control (RBAC). You choose who can perform backup and restore operations. Azure Backup also provides protection against malicious deletion of your backup by using soft-delete operations. A deleted backup is stored for 14 days, free of charge, which allows you to recover the backup if needed.

Azure Backup also supports a backup data lifecycle-management scenario that allows you to comply with retention policies.

![Graphic displaying the three security options of Azure RBAC, encryption, and soft delete as icons.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-backup/media/built-in-security.png)

## Management Plane – Recovery Services vault/Backup vault and Backup center

Azure Backup uses Recovery Services vaults and Backup vaults to orchestrate and manage backups. It also uses vaults to store backed-up data. The vault provides an interface for the user to interact with the backup service. Azure Backup Policies within each vault define when the backups should get triggered and how long they need to be retained.

You can use a single vault or multiple vaults to organize and manage your backup. If you manage your workloads with a single subscription and single resource, you can use a single vault to monitor and manage your backup estate. If your workloads are spread across multiple subscriptions, you can create multiple vaults with one or more vaults per subscription.

![Diagram of the management plane. The recovery services vault shows the options for backup policies and management with the portal, SDK, or the Command-line interface (CLI).](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-backup/media/backup-vaults.png)

Backup center allows you to have a single pane of glass to manage all tasks related to backups. Backup center is designed to function well across a large and distributed Azure environment. You can use Backup center to efficiently manage backups spanning multiple workload types, vaults, subscriptions, regions, and Azure Lighthouse tenants.

![Screenshot of the Backup center user interface in the Azure portal displaying backup information for Azure Virtual machines related to jobs and backup instances.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-backup/media/backup-center.png)

# When to use Azure Backup

Here, we'll discuss how you can decide if Azure Backup is the right choice for your data-protection needs. We'll highlight common backup scenarios where Azure Backup provides benefits, such as:

- Ensuring availability of your data.
- Protecting your Azure workloads.
- Securing your data.

## Decision criteria

Azure Backup is an Azure service that provides secure and zero-infrastructure backup solutions for all Azure-managed data assets. It protects a wide range of enterprise workloads, including Azure Virtual Machines, Azure Disks, SQL and SAP databases, and Azure file shares and blobs.

The main criteria that we're evaluating are outlined in the following table. The table contains some key areas where Azure Backup can provide services to you for data protection.

Expand table

|Criteria|Consideration|
|---|---|
|Azure workloads|Azure VMs, Azure Disks, SQL Server or SAP HANA databases running in Azure VMs, Azure Blobs, Azure Disks, Azure Database for PostgreSQL.|
|Compliance|Customer-defined backup policy with long-term retention across multiple zones or regions.|
|Operational recoveries|With self-service backup and restores, the application administrator can take care of issues that might arise such as accidental deletion or data corruption.|

## Apply the criteria

In the introduction, we presented a scenario where your organization might have an application that relies on data from a back-end SQL Server installation. SQL Server is running on three Azure VMs. The data in the backup must be retained for up to 10 years to meet compliance requirements. You also want to be able to monitor the backups.

Before we dive into how Azure Backup can help meet these needs, it's important to understand what's not currently supported. If your three Azure VMs are deployed across multiple subscriptions or regions, you should be aware that Azure Backup doesn’t support cross-region backup for most workloads. However, it does support cross-region restore in a paired secondary region.

### Can Azure Backup protect the Azure VMs hosting the SQL Server instances?

Azure Backup is able to back up entire Windows and Linux VMs using backup extensions. As a result, you can back up the entire VM that hosts SQL Server. If you only want to back up the files, folders, and system state on the Azure VMs, you can use the Microsoft Azure Recovery Services (MARS) agent.

If your main concern is to only back up the SQL Server data, Azure Backup provides support for that as well. Azure Backup offers a stream-based, specialized solution to back up SQL Servers running in Azure VMs. This solution aligns with Azure Backup's benefits of zero-infrastructure backup, long-term retention, and central management.

Additionally, Azure Backup provides the following advantages specifically for SQL Server:

- Workload aware backups that support all backup types: full, differential, and log
- 15-minute recovery point objective (RPO) with frequent log backups
- Point-in-time recovery up to a second
- Individual database-level backup and restore

![Diagram of SQL Server hosted on an Azure VM backed up to a Recovery Services Vaults in Azure Backup. Displayed are also a data path and controls arrow depicting two-way flow for the data path and control path flow from Azure Backup to the backup extension on the VM.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-backup/media/azure-backup-sql-overview.png)

### Does Azure Backup help with compliance?

You can implement required access-control mechanisms for your backups. Vaults (Recovery Services and Backup vaults) provide the management capabilities and are accessible via the Azure portal, Backup Center, Vault dashboards, SDK, CLI, and even REST APIs. It's also an Azure role-based access control (Azure RBAC) boundary, providing you with the option to restrict access to backups only to authorized Backup Admins.

Short-term retention can be _minutes_ or _daily_. Retention for _weekly_, _monthly_, or _yearly_ backup points is referred to as _Long-term retention_.

Long-term retention can be:

- **Planned (compliance requirements)**: If you know in advance that data is required years from the current time, use Long-term retention.
- **Unplanned (on-demand requirement)**: If you don't know in advance, then you can use on-demand backup with specific custom retention settings (these custom retention settings aren't impacted by policy settings).
- **On-demand backup with custom retention**: If you need to take a backup not scheduled via backup policy, then you can use an on-demand backup. It can be useful for taking backups that don’t fit your scheduled backup or for taking granular backup (for example, multiple IaaS VM backups per day since scheduled backup permits only one backup per day). It's important to note that the retention policy defined in scheduled policy doesn't apply to on-demand backups.

You can also implement policy management to help with compliance. Azure Backup Policies within each vault define when the backups should be triggered and how long they need to be retained. You can also manage these policies and apply them across multiple items.

### Does Azure Backup simplify monitoring and administration?

For monitoring and reporting, Azure Backup integrates with Log Analytics and provides the ability to see reports via Workbooks.

Azure Backup provides in-built job monitoring for operations such as configuring backup, backing up, restoring, deleting backups, and so on. It's scoped to the vault and ideal for monitoring a single vault.

If you need to monitor operational activities at scale, Backup Explorer provides an aggregated view of your entire backup estate, enabling detailed drill-down analysis and troubleshooting. It's a built-in Azure Monitor workbook that provides a single, central location to help you monitor operational activities across the entire backup estate on Azure, spanning tenants, locations, subscriptions, resource groups, and vaults.

---

# Configure Azure Monitor

# Introduction

Azure Monitor is a comprehensive solution that collects, analyzes, and responds to telemetry data from both on-premises and cloud environments.

Suppose you work for a large e-commerce company that relies heavily on its online platform to generate revenue. During a major sales event, your website experiences a sudden increase in traffic, causing performance issues and impacting customer experience. As a result, customers are unable to complete their purchases. Poor customer experience led to lost sales and had a negative impact on the company's reputation. To prevent this from happening again, you need a tool that can monitor the availability and performance of your applications and services in real-time. With this tool you can quickly identify and resolve issues. Azure Monitor is the solution that can help you achieve this.

In this module, you will learn about the features and usage cases of Azure Monitor. You learn how to configure and interpret metrics and logs. You review the different components and data types in Azure Monitor. You also learn how to access and query the activity log.

The goal of this module is to equip you with the knowledge and skills to effectively use Azure Monitor.

## Learning objectives

In this module, you learn how to:

- Identify the features and usage cases for Azure Monitor.
- Configure and interpret metrics and logs.
- Identify the Azure Monitor components and data types.
- Configure the Azure Monitor activity Log.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104).

## Prerequisites

- Familiarity with basic monitoring, evaluation, and reporting concepts.
- Knowledge of Azure resources and services that benefit from monitoring activities.
- Knowledge of the Azure portal so you can implement monitoring techniques.

# Describe Azure Monitor key capabilities

Azure Monitor provides you with a comprehensive solution for collecting, analyzing, and responding to telemetry data from your on-premises and cloud environments. The service features help you understand how your applications are performing. You can use Azure Monitor to proactively identify issues that affect your apps and resources, and take action to maximize their availability and performance.

### Things to know about Azure Monitor

Azure Monitor provides features and capabilities in three areas:

- **Monitor and visualize metrics**: Azure Monitor gathers numerical metric values from your Azure resources according to your preferences. Azure Monitor offers different methods for viewing your metric data to help you understand the health, operation, and performance of your system.
    
- **Query and analyze logs**: Azure Monitor Logs (Log Analytics) generates activity logs, diagnostic logs, and telemetry information from your monitoring solutions. The service provides analytics queries that you can use to help with troubleshooting and visualizations of your log data.
    
- **Set up alerts and actions**: Azure Monitor lets you set up alerts for your gathered data to notify you when critical conditions arise. You can configure actions based on the alert conditions, and take automated corrective steps based on triggers from your metrics or logs.

# Describe Azure Monitor components

Monitoring is the act of collecting and analyzing data. The data can be used to determine the performance, health, and availability of your business applications and the resources they depend on.

An effective monitoring strategy helps you understand the detailed operation of the components of your applications. Monitoring also helps you increase your uptime by proactively notifying you of critical issues. You can then resolve the issues before they become severe.

Azure includes multiple services that individually perform a specific role or task in the monitoring space. Together, these services deliver a comprehensive solution for collecting, analyzing, and acting on data from your applications and the Azure resources that support them. The services also work to monitor critical on-premises resources to provide a hybrid monitoring environment. Understanding the tools and data that are available is the first step in developing a complete monitoring strategy for your application.

### Things to know about monitoring with Azure

Let's take a look at the various Azure components that support Azure Monitor capabilities. The following diagram provides a high-level view of how Azure and Azure Monitor work together to provide you with a robust monitoring and diagnostics solution.

![Diagram that shows the different monitoring and diagnostic services available in Azure as described in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-monitor/media/monitor-service-d0bdfd6d.png)

- The monitoring and diagnostic services offered in Azure are divided into broad **categories** such as Core, Application, Infrastructure, and Shared Capabilities.
    
- **Data stores** in Azure Monitor hold your metrics and logs. [Azure Monitor Metrics](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/data-platform-metrics) and [Azure Monitor Logs](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/data-platform-logs) are the two base types of data used by the service.
    
- Various **monitoring sources** provide Azure Monitor with the metrics and logs data to analyze. These sources can include your Azure subscription and tenant, your Azure service instances, your Azure resources, data from your applications, and more.
    
- [Azure Monitor Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/insights/insights-overview) performs different functions with the collected data, including analysis, alerting, and streaming to external systems.
    
    - **Get insights**: Access the Azure Application Insights extension to Azure Monitor to use the Application Performance Monitoring (APM) features. You can use APM tools to monitor your application performance and gather trace logging data. Application Insights are available for many Azure services, such as Azure Virtual Machines and Azure Virtual Machine Scale Sets, Azure Container Instances, Azure Cosmos DB, and Azure IoT Edge.
        
    - **Visualize**: Utilize the many options in Azure Monitor for viewing and interpreting your gathered metrics and logs. You can use Power BI with the Azure Workbooks feature of Azure Monitor and access configurable dashboards and views.
        
    - **Analyze**: Work with Azure Monitor Logs (Log Analytics) in the Azure portal to write log queries for your data. You can interactively analyze your log data by using Azure Monitor Metrics and the powerful analysis engine.
        
    - **Respond**: Set up log alert rules in Azure Monitor to receive notifications about your application performance. You can configure the service to take automated action when the results of your queries and alerts match certain conditions or results.
        
    - **Integrate**: Ingest and export log query results from the Azure CLI, Azure PowerShell cmdlets, and various APIs. Set up automated export of your log data to your Azure Storage account or Azure Event Hubs. Build workflows to retrieve your log data and copy to external locations with Azure Logic Apps.

# Define metrics and logs

All data collected by Azure Monitor fits into one of two fundamental types, [metrics and logs](https://learn.microsoft.com/en-us/azure/azure-monitor/platform/data-collection):

**Metrics** are numerical values that describe some aspect of a system at a particular point in time. Metrics are lightweight and capable of supporting near real-time scenarios.

**Logs** contain different kinds of data organized into records with different sets of properties for each type. Data like events and traces are stored as logs along with performance data so all the data can be combined for analysis.

### Things to know about Azure Monitor metrics

Let's examine how to work with Azure Monitor metrics in the Azure portal.

- For many Azure resources, the metrics data collected by Azure Monitor is displayed on the **Overview** page for the resource in the Azure portal. Consider the overview for an Azure virtual machine that has several charts that show performance metrics.
    
- You can use Azure Monitor **metrics explorer** to view the metrics for your Azure services and resources.
    
- In the Azure portal, select any graph for a resource to open the associated metrics data in metrics explorer. The tool lets you chart the values of multiple metrics over time. You can work with the charts interactively or pin them to a dashboard to view them with other visualizations.
    

![Illustration that depicts Azure Monitor metrics data graphs providing information to Metric Analytics in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-monitor/media/monitor-data-platform-7dbebda9.png)

### Things to know about Azure Monitor Logs

You can also work with Azure Monitor Logs (Log Analytics) in the Azure portal. Let's review the details.

- In the Azure portal, log data collected by Azure Monitor is stored in Log Analytics.
    
- Log Analytics includes a [rich query language](https://learn.microsoft.com/en-us/azure/azure-monitor/log-query/log-query-overview) to help you quickly retrieve, consolidate, and analyze your collected data.
    
- You can work with Log Analytics to create and test queries. Use the query results to directly analyze the data, save your queries, visualize the data, and create alert rules.
    
- Azure Monitor uses a version of the [Data Explorer](https://learn.microsoft.com/en-us/azure/kusto/query/) query language. The language is suitable for simple log queries, but also includes advanced functionality like aggregations, joins, and smart analytics. You can quickly learn the query language by completing several available lessons. Particular guidance is provided for users familiar with SQL and Splunk.
    

![Illustration that depicts an Azure Monitor Logs database providing information to Log Analytics in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-monitor/media/log-data-32e50eae.png)

# Identify monitoring data and tiers

Azure Monitor can collect data from various sources. You can think of the collected data as being categorized by tier. Tiers can include data collected from many sources, such as:

- Your application
- The operating system
- Services and resources used by your application
- The platform that supports your application

### Things to know about data collection

Review the following details about how Azure Monitor collects different categories of data.

- Azure Monitor begins collecting data as soon as you create your Azure subscription and add resources.
    
- When you create or modify resources, this data is stored in Azure Monitor activity logs.
    
- Performance data about resources, along with the amount of resources consumed, is stored as Azure Monitor metrics.
    
- Extend the data you're collecting by enabling diagnostics and adding Azure Monitor Agent to compute resources. By extending your data sources, you can collect data for the internal operation of the resources.
    
- Azure Monitor Agent also lets you configure different data sources to collect logs and metrics from Windows and Linux guest operating systems.
    
- Azure Monitor can collect log data from any REST client by using the Data Collector API. The Data Collector API lets you create custom monitoring scenarios and extend monitoring to resources that don't expose data through other sources.
    

#### Monitoring data tiers

The following table summarizes the tiers of monitoring data that are collected by Azure Monitor.

Expand table

|Data tier|Description|
|---|---|
|**Application**|The Application tier contains monitoring data about the performance and functionality of your application code. This data is collected regardless of your platform.|
|**Guest OS**|Monitoring data about the operating system on which your application is running is organized into the Guest OS tier. Your application can run in Azure, another cloud, or on-premises.|
|**Azure resource**|The Azure resource tier holds monitoring data about the operation of any Azure resource you utilize, including consumption details for the resource.|
|**Azure subscription**|The Azure subscription tier contains monitoring data about the operation and management of your Azure subscription. The tier also contains data about the health and operation of Azure itself.|
|**Azure tenant**|Data about the operation of your tenant-level Azure services, such as Microsoft Entra ID, is organized into the Azure tenant tier.|

# Describe activity log events

The Azure Monitor activity log is a subscription log that provides insight into subscription-level events that occur in Azure. Events can include a range of data from Azure Resource Manager operational data to updates on Azure service health events.

### How to use the Azure Activity Log

![](https://youtu.be/ACVpH6C_NL8)

### Things to know about activity logs

Let's examine some details about working with activity logs in Azure Monitor.

- You can use the information in activity logs to understand the status of resource operations and other relevant properties.
    
- Activity logs can help you determine the "what, who, and when" for any write operation (PUT, POST, DELETE) performed on resources in your subscription.
    
- Activity logs are kept for 90 days.
    
- You can query for any range of dates in an activity log, as long as the starting date isn't more than 90 days in the past.
    
- You can retrieve events from your activity logs by using the Azure portal, the Azure CLI, PowerShell cmdlets, and the Azure Monitor REST API.
    

![Diagram that shows how Azure Monitor activity logs gather information from compute and non-compute resources in Azure.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-monitor/media/activity-log-7d1913ad.png)

#### Business scenarios

Activity logs can help you monitor your configuration and get details for many scenarios, such as:

_What operations happened on resources in my subscription?_

_Who initiated the operations?_

_When did the operations occur?_

_What's the current status of the operations?_

_What are the values of other properties that can help with my analysis of the resources and operations?_

# Query the activity log

In the Azure portal, you can filter your Azure Monitor activity logs so you can view specific information. The filters enable you to review only the activity log data that meets your criteria. You might set filters to review monitoring data about critical events for your primary subscription and production virtual machine during peak business hours.

![Screenshot that shows filter options for activity logs in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-monitor/media/query-activity-log-a92271d9.png)

### Things to know about activity log filters

Let's review some of the filters you can set to control what data to review in your activity log:

- **Subscription**: Show the data for one or more specified Azure subscription names.
    
- **Timespan**: Show data for a specified time by choosing the start and end time for events, such as a six-hour period.
    
- **Event Severity**: Show events at the selected severity levels, including _Informational_, _Warning_, _Error_, or _Critical_.
    
- **Resource group**: Show data for one or more specified resource groups within your specified subscriptions.
    
- **Resource (name)**: Show data for the specified resources.
    
- **Resource type**: Show data for resources of a specified type, such as `Microsoft.Compute/virtualmachines`.
    
- **Operation name**: Show data for a selected Azure Resource Manager operation, such as `Microsoft.SQL/servers/Write`.
    
- **Event initiated by**: Show operation data for a specified user who performed the operation, referred to as the "caller."
    

After you define a set of filters, you can pin the filter set to the Azure Monitor dashboard. You can also download your activity log search results as a CSV file.

In addition to the filters, you can enter a text string in the **Search** box. Azure Monitor tries to match your search string against data returned for all fields in all events that corresponds to your filter settings.

### Things to know about event categories

The following table summarizes the categories of events that you can review in your activity logs. The information displayed for events is based on your other filter settings.

Expand table

  

| Event category      | Event data                                                                                                                                                                                                       | Examples                                                                                                                 |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **Administrative**  | All create, update, delete, and action operations performed through Azure Resource Manager, and any changes to role-based access control (RBAC) in your filtered subscriptions                                   | `create virtual machine`  <br>  <br>`delete network security group`                                                      |
| **Service Health**  | All service health events for Azure services and resources connected with your filtered subscriptions, including _Action Required_, _Assisted Recovery_, _Incident_, _Maintenance_, _Information_, or _Security_ | `SQL Azure in East US is experiencing downtime`  <br>  <br>`Azure SQL Data Warehouse Scheduled Maintence Complete`       |
| **Resource Health** | All resource health events for your filtered Azure resources, including _Available_, _Unavailable_, _Degraded_, or _Unknown_, and identified as _Platform Initiated_ or _User Initiate_                          | `Virtual Machine health status changed to unavailable`  <br>  <br>`Web App health status changed to available`           |
| **Alert**           | All activations of Azure alerts for your filtered subscriptions and resources                                                                                                                                    | `CPU % on devVM001 has been over 80 for the past 5 minutes`  <br>  <br>`Disk read LessThan 100000 in the last 5 minutes` |
| **Autoscale**       | All events related to the operation of the autoscale engine based on any autoscale settings defined for your filtered subscriptions                                                                              | `Autoscale scale up action failed`                                                                                       |
| **Recommendation**  | Recommendation events for certain Azure resource types, such as web sites and SQL servers, based on your filtered subscriptions and resources                                                                    | _Recommendations for how to better utilize your resources_                                                               |
| **Security**        | All alerts generated by Microsoft Defender for Cloud affecting your filtered subscriptions and resources                                                                                                         | `Suspicious double extension file executed`                                                                              |
| **Policy**          | All effect action operations performed by Azure Policy for your filtered subscriptions and resources, where every action taken by Azure Policy is modeled as an operation on a resource                          | `Audit` and `Deny`                                                                                                       |

# Interactive lab simulation

## Lab scenario

Your organization wants insight into the performance and configuration of Azure resources. As the Azure Administrator you need to:

- Explore Azure virtual machine monitoring capabilities, including available metrics.
- Explore alerts and notification features.
- Review logs by using Azure Monitor Logs (Log Analytics) queries.

## Architecture diagram

![Architecture diagram as explained in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-monitor/media/lab-11.png)

## Objectives

- **Task 1**: Provision the lab environment.
    - Review an [Azure Resource Manager (ARM) template](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/blob/master/Allfiles/Labs/11/az104-11-vm-template.json).
    - Use the ARM template to deploy a virtual machine to use to test monitoring scenarios.
- **Task 2**: Register the Microsoft Insights and Microsoft Alerts Management resource providers.
    - Create a Log Analytics workspace in the same region as the virtual machines.
    - Create an Azure Automation Account and associate it with the Azure Monitor Logs (Log Analytics) workspace.
    - Enable update management.
- **Task 3**: Create and configure an Azure Monitor Logs (Log Analytics) workspace and Azure Automation-based solutions.
    - Review Azure virtual machine monitoring options.
    - Review the list of available metrics.
- **Task 4**: Review default monitoring settings of Azure virtual machines.
- **Task 5**: Configure Azure virtual machine diagnostic settings.
    - Review the Azure virtual machine monitoring settings and enable guest-level monitoring.
    - Enable Azure Monitor Agent and available metrics.
- **Task 6**: Review Azure Monitor functionality.
    - Configure Azure Monitor metrics.
    - Create an alert rule based on average percentage CPU.
    - Configure notifications for an action group.
    - Trigger increased CPU utilization and review alert notifications.
- **Task 7**: Review Azure Monitor Logs (Log Analytics) functionality.
    - Create a log query to chart the virtual machine's available memory over the last hour.
    - Run the log query and preview the data.

 Note

Select the thumbnail image to start the lab simulation. When you're done, be sure to return to this page so you can continue learning.

[![Screenshot of the simulation page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-monitor/media/simulation-monitor-thumbnail.jpg)](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2017)

---

# Configure Log Analytics

# Introduction

Log Analytics is a tool in Azure Monitor that allows you to edit and run log queries for data collected in Azure Monitor Logs. It offers query features and tools, supports the Kusto Query Language (KQL), and allows for detailed analysis and problem-solving.

Imagine you're an Azure Administrator working for a large e-commerce company. Your company recently experienced a major security breach, and you need to investigate the root cause and prevent future incidents. You have access to logs from various Azure services, but manually analyzing them would be time-consuming and inefficient.

By using Log Analytics, you can easily query and analyze the logs to identify any suspicious activities, track changes, and ensure compliance with security standards. With Log Analytics, you can quickly assess update requirements and time-to-complete, track changes, and identify access issues in your systems. It helps meet strict SLAs for businesses and provides a single interface for analyzing data from multiple sources.

The goal of this module is to provide you with the knowledge and skills to effectively use Log Analytics in Azure Monitor.

## Learning objectives

In this module, you learn how to:

- Identify the features and usage cases for Log Analytics in Azure Monitor.
- Structure and create a Log Analytics workspace in the Azure portal.
- Use KQL to query a Log Analytics workspace and review results.

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104).

## Prerequisites

- Working knowledge of Azure Monitor including data sources and collected data.
- Experience with the Azure portal including navigating and locating resources.
- Familiarity with structuring and executing data queries.

# Determine Log Analytics uses

Log Analytics is a tool for Azure Monitor that's available in the Azure portal. You can use Log Analytics to edit and run log queries for the data collected in Azure Monitor Logs. Log queries help you to search for patterns and identify issues.

![Screenshot that shows an example of Azure Monitor Logs in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-log-analytics/media/log-analytics-38112dc9.png)

### Things to know about Log Analytics

Let's examine some characteristics of Log Analytics in Azure Monitor.

- Log Analytics in Azure Monitor offers query features and tools that help you answer virtually any question about your monitored configuration.
    
- Log Analytics supports the Kusto Query Language (KQL). You can create simple or complex queries with KQL, including:
    
    - Search and sort by value, time, property state, and more
    - Join data from multiple tables
    - Aggregate large sets of data
    - Perform intricate operations with minimal code
- When your Azure Monitor Logs contain sufficient collected data, and you understand how to construct the appropriate query, you can use Log Analytics to complete detailed analysis and problem solving.
    

### Things to consider when using Log Analytics

Some features in Azure Monitor, such as insights and solutions, process log data without exposing you to the underlying queries. To use other Azure Monitor features, you need to understand how to construct queries and apply them to interactively analyze data in Azure Monitor Logs. The following business scenarios showcase the advantages of querying Azure Monitor Logs with Log Analytics.

#### Business scenario: Assess update requirements and time-to-complete

An important daily task for IT admins is to assess system update requirements and plan for configuration patches. Accurate scheduling is critical because the patching process relates to SLAs to the business and can negatively affect business functions.

In the past, administrators had to schedule a patch update with only limited knowledge of how long it might take to complete the process. With an Azure subscription, admins can access benefits of the Microsoft Azure platform. Azure collects data from all customers performing patches. Azure uses the gathered data to provide an average patching time for specific updates.

This use of "crowd-sourced" data is unique to cloud systems. It's a great example of how Log Analytics in Azure Monitor can you help meet strict SLAs for your business.

#### Business scenario: Track changes and identify access issues

Troubleshooting an operational incident is a complex process that requires access to multiple data streams. By monitoring your systems from the Azure platform, you can easily perform analysis from multiple angles. You have access to data from a wide variety of sources through a single interface for correlation of information.

By tracking changes across the Azure environment, Log Analytics in Azure Monitor can help you easily identify common issues, such as:

- Abnormal behavior from a specific account
- Users installing unapproved software
- Unexpected system reboots or shutdowns
- Evidence of security breaches
- Specific problems in loosely coupled applications

# Create a Log Analytics workspace

When you capture logs and data in Azure Monitor, Azure stores the collected information in a Log Analytics workspace. Your Log Analytics workspace is the basic management environment for Azure Monitor Logs.

### How to define your Log Analytics scope

![](https://youtu.be/34IrPO1Xh2o)

### Things to know about the Log Analytics workspace

To get started with Log Analytics in Azure Monitor, you need to create your workspace. Each workspace has a unique workspace ID and resource ID. After you create your workspace, you configure your data sources and solutions to store their data in your workspace.

![Screenshot that shows how to create a Log Analytics workspace in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-log-analytics/media/create-workspace-f37a5b11.png)

To create your Log Analytics workspace, configure the following parameters:

- **Name**: Provide a name for your new Log Analytics workspace. The name for your workspace must be unique within your resource group.
    
- **Subscription**: Specify the Azure Subscription to associate with your workspace.
    
- **Resource Group**: Specify the resource group to associate with your workspace. You can choose an existing resource group or create a new one. The resource group must contain at least one Azure Virtual Machines instance.
    
- **Region**: Select the region where you deploy your virtual machines.
    
     Note
    
    The region must support Log Analytics. You can review the [regions that support Log Analytics](https://azure.microsoft.com/explore/global-infrastructure/products-by-region/). In the **Search for a product** box, enter "Azure Monitor."
    
- **Pricing**: The default pricing tier for a new workspace is _pay-as-you-go_. Charges incur only after you start collecting data.
    
    Each Log Analytics workspace in Azure Monitor can have a different pricing tier. You can change the pricing tier for a workspace and also track the changes.

# Create Kusto queries

Log Analytics in Azure Monitor supports the Kusto Query Language (KQL). The KQL syntax helps you quickly and easily create simple or complex queries to retrieve and consolidate your monitoring data in the repository.

## Write Kusto Query Language log queries for Azure Monitor

Watch the following video to learn how to write log queries with Log Analytics in Azure Monitor. The video covers the following concepts:

- View table data in the Azure Monitor Logs repository
- Create simple and complex queries
- Filter and summarize search results
- Add visualizations for search results

In the next unit, we take a closer look at how to structure a log query.

### Things to consider when using log queries

Here are some of the many things you can accomplish with log queries in Log Analytics:

- Create and save searches of your data stored in the Azure Monitor Logs repository.
    
- Use your saved log searches to directly analyze your data in the Azure portal.
    
- Configure your saved log searches to run automatically.
    
- Configure your saved log searches to produce notification alerts.
    
- Add visualizations for your saved log searches to see graphical views of your environment health.
    
- Export your data from the repository into analytical tools like Power BI or Excel.

# Structure Log Analytics queries

Administrators build Log Analytics queries from data stored in dedicated tables in a Log Analytics workspace. Some common dedicated tables include Event, Syslog, Heartbeat, and Alert. When you build a Kusto Query Language (KQL) query, you begin by determining which tables in the Azure Monitor Logs repository have the data you're looking for.

The following illustration highlights how KQL queries use the dedicated table data for your monitored services and resources.

![Illustration that shows how to build Log Analytics queries from data in dedicated tables in a Log Analytics workspace.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-log-analytics/media/query-tables-f3872e3a.png)

### Things to know about KQL query structure

Let's take a closer look at dedicated table data and how to structure a KQL log query.

- Each of your selected data sources and solution stores its data in dedicated tables in your Log Analytics workspace.
    
- Documentation for each data source and solution includes the name of the data type that it creates and a description of each of its properties.
    
- The basic structure of a query is a source table followed by a series of commands (referred to as _operators_).
    
- A query can have a chain of multiple operators to refine your data and perform advanced functions.
    
- Each operator in a query chain begins with a pipe character `|`.
    
- Many queries require data from a single table only, but other queries can use various options and include data from multiple tables.
    

### KQL log query examples

Let's review some common KQL log query operators and example syntax.

We can build queries to search for data in the `StormEvent` table that has five entries:

Expand table

|type|event|severity|start|duration|region|
|---|---|---|---|---|---|
|`Water`|`Freezing rain`|`1`|`6:00 AM 01-27-2023`|`3 hours`|`1, 2`|
|`Wind`|`High winds`|`1`|`8:00 AM 01-27-2023`|`12 hours`|`1, 2, 4, 5`|
|`Temperature`|`Below freezing`|`2`|`11:00 PM 01-26-2023`|`10 hours`|`1, 2, 4, 5`|
|`Water`|`Snow`|`3`|`4:00 PM 01-26-2023`|`10 hours`|`1, 2, 4, 5`|
|`Water`|`Flood warning`|`2`|`9:00 AM 01-26-2023`|`10 hours`|`3`|

To find other operators and examples, review: [Analyze monitoring data with Kusto Query Language - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/paths/analyze-monitoring-data-with-kql/).

#### Count number of items

Use the `count` operator to discover the number of records in an input record set.

The following example returns the number of records in the `StormEvent` table. The query results reveal the `StormEvent` table has five entries.

KustoCopy

```
StormEvent | count
```

Query results:

Expand table

|count|
|---|
|`5`|

#### Return first number of items

Use the `top` operator to see the first N records of your input record set, sorted by your specified columns. The columns correspond to data properties defined in the dedicated table.

The following example returns the first three data records for `StormEvent`. The results table shows the storm `event` name, the severity, and the forecasted duration.

KustoCopy

```
StormEvent | top 3 by event severity duration
```

Query results:

Expand table

|event|severity|duration|
|---|---|---|
|`Freezing rain`|`1`|`3 hours`|
|`High winds`|`1`|`12 hours`|
|`Below freezing`|`2`|`10 hours`|

#### Find matching items

Use the `where` operator to filter your table to the subset of rows that match the supplied predicate value. The predicate value indicates what to search for in the table, as in `where=="find-this"`.

The following example filters the data records for `StormEvent` to use only records that match "snow."

KustoCopy

```
StormEvent | where event=="snow"
```

Your query filters to one row in the `StormEvent` table:

Expand table

|type|event|severity|start|duration|region|
|---|---|---|---|---|---|
|`Water`|`Snow`|`3`|`4:00 PM 01-26-2023`|`10 hours`|`1, 2, 4, 5`|

---

# Configure Network Watcher

# Introduction

Azure Network Watcher is a powerful tool that allows you to monitor, diagnose, and manage resources in an Azure virtual network.

Imagine you're an IT administrator for a large e-commerce company. Your company relies heavily on its Azure virtual network to host its website and handle customer transactions. One day, you receive reports from customers that they're unable to access the website or complete their purchases. You need to quickly identify the cause of the connectivity issues and resolve them to minimize the impact on your business.

Your organization plans to use Network Watcher. Network Watcher's monitoring and diagnostic capabilities let you easily pinpoint the root cause of the problem and take appropriate actions. By using features such as **IP flow verification, next hop analysis, and connection troubleshooting**, you can ensure that your virtual network is functioning optimally.

In this module, you learn about the various features and use cases of Azure Network Watcher. The topics covered include IP flow verification, next hop analysis, and the topology tool. The module guides you on how to diagnose network configuration issues, such as broken security rules.

The goal of this module is to provide you with a comprehensive understanding of Azure Network Watcher and its capabilities. By the end of this module, you effectively monitor, diagnose, and manage your Azure virtual network using Network Watcher.

# Describe Azure Network Watcher features

Azure Network Watcher provides tools to monitor, diagnose, view metrics, and enable or disable logs for resources in an Azure virtual network. Network Watcher is a regional service that enables you to monitor and diagnose conditions at a network scenario level.

![Screenshot of the Network Watcher Get Started page in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-watcher/media/network-watcher-861659e0.png)

### Things to know about Network Watcher

Let's review some of the prominent features of Network Watcher.

Expand table

|Feature|Description|Scenarios|
|---|---|---|
|**IP flow verify**|Quickly diagnose connectivity issues from or to the internet, and from or to your on-premises environment.|_Identify if a security rule blocks ingress or egress traffic to or from a virtual machine_  <br>  <br>_Troubleshoot issues to determine if other exploration is required_|
|**Next hop**|View the next connection point (or _next hop_) in your network route, and analyze your network routing configuration.|_Determine if there's a next hop, and view the next hop target, type, and route table_  <br>  <br>_Confirm traffic reaches an intended target destination_|
|**VPN troubleshoot**|Diagnose and troubleshoot the health of your virtual network gateway or connection with gathered data. View connection statistics, CPU and memory information, IKE security errors, packet drops, and buffers and events.|_View summary diagnostics in the Azure portal_  <br>  <br>_Review detailed diagnostics in generated log files stored in your Azure storage account_  <br>  <br>_Simultaneously troubleshoot multiple gateways or connections_|
|**NSG diagnostics**|Use flow logs to map IP traffic through a network security group (NSG). A common implementation for NSG flow logs is to meet security compliance regulations and auditing requirements.|_Define prescriptive NSG rules for your organization, and conduct periodic compliance audits_  <br>  <br>_Compare your prescriptive NSG rules against the effective rules for each virtual machine in your network_|
|**Connection troubleshoot**|Azure Network Watcher Connection Troubleshoot is a more recent addition to the Network Watcher suite of networking tools and capabilities. Check a direct TCP or ICMP connection from a virtual machine, application gateway, or Azure Bastion host to a virtual machine.|_Troubleshoot your network performance and connectivity issues in Azure_  <br>  <br>_Troubleshoot connection issues for a virtual machine, application gateway, or Azure Bastion host_|

 Note

To use Network Watcher, you must be an Owner, Contributor, or Network Contributor. If you create a custom role, the role must be able to read, write, and delete the Network Watcher service.

### Things to consider when using Network Watcher

Azure Network Watcher supports many Azure monitoring tasks and scenarios. As you review these features, think about how Network Watcher can support your Azure monitoring requirements.

- **Consider remote monitoring**. Automate remote network monitoring with packet capture. You can monitor and diagnose networking issues without logging in to your virtual machines.
    
- **Consider alert notifications**. Set alerts to trigger packet capture, and access real-time performance information at the packet level. When you observe an issue, you can investigate in detail for better diagnoses.
    
- **Consider NSG flow log diagnosis**. Use NSG flow logs to gain insight into your network traffic. Build a deeper understanding of your network traffic pattern by using NSG flow logs. Information provided by flow logs helps you gather data for compliance, auditing, and monitoring your network security profile.
    
- **Consider log analysis**. Diagnose your most common Azure VPN Gateway and connections issues. You can identify issues and use the generated detailed logs to assist your analysis.

# Review IP flow verify diagnostics

The **IP flow verify** feature in Azure Network Watcher checks connectivity from or to the internet, and from or to your on-premises environment. This feature helps you identify if a security rule is blocking traffic to or from your virtual machine or the internet.

![Screenshot of the IP flow verify feature in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-watcher/media/flow-verify-d136d78d.png)

### Things to know about IP flow verify

Let's examine the configuration details and functionality of the IP flow verify feature in Azure Network Watcher.

- You configure the IP flow verify feature with the following properties in the Azure portal:
    
    - Virtual machine and network interface
    - Local (source) port number
    - Remote (destination) IP address, and remote port number
    - Communication protocol (TCP or UDP)
    - Traffic direction (Inbound or Outbound)
- The feature tests communication for a target virtual machine with associated network security group (NSG) rules by running inbound and outbound packets to and from the machine.
    
- After the test runs complete, the feature informs you whether communication with the machine succeeds (allows access) or fails (denies access).
    
- If the target machine denies the packet because of an NSG, the feature returns the name of the controlling security rule.
    

### Things to consider when using IP flow verify

The IP flow verify feature is ideal for helping to ensure correct application of your security rules.

When you deploy a virtual machine, Azure applies several default security rules to your machine. The security rules allow or deny traffic to or from your virtual machine. You can override Azure's default rules or create other rules.

At some point, your virtual machine might be unable to communicate with other resources because of a security rule. You can use the IP flow verify feature to troubleshoot your NSG rules.

If test runs fail, but the IP flow verify feature doesn't indicate the issue is related to your NSG rules, you need to explore other areas, such as firewall restrictions.

# Review next hop diagnostics

The **next hop** feature in Azure Network Watcher checks if traffic is being directed to the intended destination. This feature lets you view the next connection point (or _next hop_) in your network route, and helps you verify a correct network configuration.

![Screenshot of the next hop feature in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-watcher/media/next-hop-d7dde5f6.png)

### Things to know about next hop

Let's review the configuration properties and summary of the next hop feature in Azure Network Watcher.

- You configure the next hop feature with the following properties in the Azure portal:
    
    - Your subscription and resource group
    - Virtual machine and network interface
    - Source IP address
    - Destination IP address (If you want to confirm a specified target is reachable)
- The feature tests the next connection point in your network route configuration.
    
- The next hop test returns three items:
    
    - Next hop type
    - IP address of the next hop (If available)
    - Route table for the next hop (If available)
- Examples of a next hop are _Internet_, _Virtual Network_, and _Virtual Network Service Endpoint_.
    
- If the next hop is a user-defined route (UDR), the process returns the UDR route. Otherwise, next hop returns the system route.
    
- If the next hop is of type _None_, there might be a valid system route to the destination IP address, but no next hop exists to route the traffic to the target.
    

### Things to consider when using next hop

The next hop feature is ideal for identifying unresponsive virtual machines or broken routes in your network.

When you create a virtual network, Azure creates several default outbound routes for network traffic. Outbound traffic from all resources (such as virtual machines) deployed in the virtual network is routed based on Azure's default routes. You can override Azure's default routes or create other routes.

You might find that a virtual machine can no longer communicate with other resources connected by a specific route. You can use the next hop feature to examine a specific source and destination IP address in your configuration.

Next hop tests the communication between the source and destination, and reports the type of next hop in the traffic route. You can then remove, change, or add a route, to resolve routing issues.

# Visualize the network topology

Administrators sometimes need to troubleshoot virtual networks that they didn't help to create. They might not be fully aware of all the aspects of the infrastructure and configuration.

Azure Network Watcher provides a network monitoring **topology** tool to help administrators visualize and understand infrastructure. The following image shows an example topology diagram for a virtual network in Network Watcher.

![Screenshot of the Network Watcher Topology page in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-watcher/media/monitor-visualization-1fb7bd5c.png)

### Things to know about the topology tool

Review the following characteristics of the network topology capability in Azure Network Watcher.

- The Network Watcher Topology tool generates a visual diagram of the resources in a virtual network.
    
- The graphical display shows the resources in the network, their interconnections, and their relationships with each other.
    
- You can view subnets, virtual machines, network interfaces, public IP addresses, network security groups, route tables, and more.
    
- To generate a topology, you need an Azure Network Watcher instance in the same region as the virtual network.

---

# Improve incident response with Azure Monitor alerts

# Introduction

Microsoft Azure provides a robust alerting and monitoring solution called Azure Monitor. You can use Azure Monitor to configure notifications and alerts for your key systems and applications. These alerts ensure that the correct team knows when a problem arises.

You work for a large shipping company that recently deployed several web applications to the Azure platform. Due to a configuration error, the customer-facing order tracker was offline. The issue wasn't identified until customers started complaining that they couldn't track their orders. As a consequence, customer satisfaction with your service dropped.

As your company's Azure solution architect, you need to find a solution that detects problems in your environments in real time. The solution should notify the correct team so it can resolve any problems before your customers notice.

## Learning objectives

In this module, you'll:

- Explore alerts by using Azure Monitor.
- Understand when to use metric, log, and activity log events.
- Create and use metric alerts, log search alerts, and activity log alerts.
- Use action groups to determine what kind of notifications are sent and to whom.
- Learn how to use alert-processing rules to override the normal behavior of action groups when needed.

## Prerequisites

- Knowledge of Azure Monitor

# Explore the different alert types that Azure Monitor supports

Azure Monitor is a powerful reporting and analytics tool. You can use it for insights into the behavior and running of your environment and applications. You can then respond proactively to faults in your system.

After the downtime that your customers faced, you set up monitoring on your key resources in Azure. With the monitoring in place, you want to make sure the right people are being alerted at the right level.

In this unit, you learn how Azure Monitor receives resource data, what makes up an alert, and how and when to use an alert. Finally, you learn how to create and manage your own alerts.

## Data types in Azure Monitor

Azure Monitor receives data from target resources like applications, operating systems, Azure resources, Azure subscriptions, and Azure tenants. The nature of the resource defines which data types are available. A data type can be a _metric_, a _log_, or both a metric and a log:

- The focus for _metric_-based data types is the numerical time-sensitive values that represent some aspect of the target resource.
- The focus for _log_-based data types is the querying of content data held in structured, record-based log files that are relevant to the target resource.

![Diagram that represents the target resources feeding into Azure Monitor and the two principal signal types: metrics and logs.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/2-azure-resource-signal-types.svg)

There are three signal types that you can use to monitor your environment:

- **Metric** alerts provide an alert trigger when a specified threshold is exceeded. For example, a metric alert can notify you when CPU usage is greater than 95 percent.
- **Activity log** alerts notify you when Azure resources change state. For example, an activity log alert can notify you when a resource is deleted.
- **Log** alerts are based on things written to log files. For example, a log search alert can notify you when a web server returns a particular number of 404 or 500 responses.

## Composition of an alert rule

Every alert or notification available in Azure Monitor is the product of a rule. Some of these rules are built into the Azure platform. You can use alert rules to create custom alerts and notifications. No matter which target resource or data source you use, the composition of an alert rule remains the same.

- **RESOURCE**
    - The _target resource_ for the alert rule. You can assign multiple target resources to a single alert rule. The type of resource defines the available signal types.
- **CONDITION**
    - The _signal type_ used to assess the rule. The signal type can be a metric, an activity log, or logs. There are others, but this module doesn't cover them.
    - The _alert logic_ applied to the data that's being supplied via the signal type. The structure of the alert logic changes depending on the signal type.
- **ACTIONS**
    - The _action_, like sending an email, sending a Short Message Service (SMS) message, or using a webhook.
    - An _action group_, which typically contains a unique set of recipients for the action.
- **ALERT DETAILS**
    - An _alert name_ and an _alert description_ that specify the alert's purpose.
    - The _severity_ of the alert if the criteria or logic test evaluates `true`. The five severity levels are:
        - **0**: Critical
        - **1**: Error
        - **2**: Warning
        - **3**: Informational
        - **4**: Verbose

![Screenshot of the Create rule page in the Azure Monitor portal.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/2-creating-an-alert.png)

## Scope of alert rules

You can get monitoring data from across most of the Azure services and report on it by using the Azure Monitor pipeline. In the Azure Monitor pipeline, you can create alert rules for these items and more:

- Metric values
- Log search queries
- Activity log events
- Health of the underlying Azure platform
- Tests for website availability

## Manage alert rules

Not every alert rule that you create needs to run forever. With Azure Monitor, you can specify one or more alert rules and enable or disable them, as needed.

As an Azure solution architect, you want to use Azure Monitor to enable tightly focused and specific alerts before any application change. Then, disable the alerts after a successful deployment.

## Alert summary view

The alert page shows a summary of all alerts. You can apply filters to the view by using one or more of the following categories: subscriptions, alert condition, severity, or time ranges. The view includes only alerts that match these criteria.

![Screenshot of Azure Monitor alerts page in the Azure Monitor portal.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/2-alerts-page.png)

### Alert condition

The system sets the alert condition.

- When an alert fires, the alert's monitor condition is set to **Fired**.
- After the underlying condition that caused the alert to fire clears, the monitor condition is set to **Resolved**.

# Use metric alerts for alerts about performance issues in your Azure environment

Azure Monitor can use thresholds to monitor specific resources. In an organization, it's far more useful to be notified when the free disk space on a server is less than five percent, instead of being alerted every time a file is saved.

As a solution architect, you want to implement regular threshold monitoring for many of your target resources and instances. Monitoring helps to head off potential issues before they can affect your customers.

In this unit, you investigate the different kinds of metric alerts that Azure Monitor supports.

## When would you use metric alerts?

In Azure Monitor, you can use metric alerts to achieve regular threshold monitoring of Azure resources. Azure Monitor runs metric alert trigger conditions at regular intervals. When the evaluation is true, Azure Monitor sends a notification. Metric alerts are stateful, and Azure Monitor sends a notification only when the prerequisite conditions are met.

Metric alerts can be useful if, for instance, you need to know when your server CPU utilization is reaching a critical threshold of 90 percent. You can receive alerts when your database storage is getting too low, or when network latency is about to reach unacceptable levels.

## Composition of a metric alert

As you learned in the previous unit, all alerts are the product of the rules that govern them. For metric alerts, there's another factor to define: the condition type. It can be static or dynamic.

You must define the type of statistical analysis to be used with either static or dynamic metric alerts. Example types are minimum, maximum, average, and total. In this example, you define the period of data to be assessed: the last 10 minutes. Finally, you set the frequency by which the alert conditions are checked: every two minutes.

### Use static threshold metric alerts

Static metric alerts are based on simple static conditions and thresholds that you define. With static metrics, you specify the threshold used to trigger the alert or notification.

In the previously defined scenario, a static alert with a threshold of 85 percent CPU utilization checks the rule every two minutes. It evaluates the last 10 minutes of CPU utilization data to assess if it rises above the threshold. If the evaluation is true, the alert triggers the actions associated with the action group.

### Use dynamic threshold metric alerts

Dynamic metric alerts use machine-learning tools that Azure provides to automatically improve the accuracy of the thresholds defined by the initial rule.

There's no hard threshold in dynamic metrics. However, you need to define two more parameters:

- The _look-back period_ defines how many previous periods need to be evaluated. For example, if you set the look-back period to three, then in the example used here, the assessed data range would be 30 minutes (three sets of 10 minutes).
    
- The _number of violations_ expresses how many times the logic condition has to deviate from the expected behavior before the alert rule fires a notification. In this example, if you set the number of violations to two, the alert would be triggered after two deviations from the calculated threshold.
    

## Understand dimensions

Until now, the assessed metric alerts we discussed, focused on a single target instance. Azure Monitor supports dimensions, which enable monitoring data to be supplied from multiple target instances.

You can use dimensions to define one metric alert rule and apply it to multiple related instances. For example, you can monitor CPU utilization across all the servers running your app. You can then receive an individual notification for each server instance when the rule conditions are triggered.

You can define the dimensions by naming each target instance specifically, or you can define the dimensions by using the asterisk (*) wildcard, which uses all available instances.

## Scale metric alerts

Azure Monitor supports creating metric alerts that, like dimensions, monitor multiple resources. Scaling is currently limited to Azure virtual machines. However, a single metric alert can monitor resources in one Azure region.

Creating scaling metric alert rules to monitor multiple resources is no different than creating any other metric alert rule; you just select all the resources that you want to monitor.

Like dimensions, a scaling metric alert is individual to the resource that triggered it.

# Exercise - Use metric alerts to alert on performance issues in your Azure environment


The shipping company you work for wants to avoid any future issues with updates to its applications on the Azure platform. To improve the alert capabilities in Azure, you made the choice to use Azure metric alerts.

In this exercise, you create a Linux virtual machine (VM). This VM runs an app that runs the CPU at 100 percent utilization. You create monitoring rules in the Azure portal and in the Azure CLI to alert you about high CPU usage.

## Create the VM

This VM runs a specific configuration that stresses the CPU and generates the metric monitoring data needed to trigger an alert.

1. Start by creating the configuration script. To create the `cloud-init.txt` file with the configuration for the VM, run the following command in Azure Cloud Shell:
    
    BashCopy
    
    ```
    cat <<EOF > cloud-init.txt
    #cloud-config
    package_upgrade: true
    packages:
    - stress
    runcmd:
    - sudo stress --cpu 1
    EOF
    ```
    
2. To set up an Ubuntu Linux VM, run the following `az vm create` command. This command uses the `cloud-init.txt` file that you created in the previous step to configure the newly created Ubuntu Linux VM.
    
    Azure CLICopy
    
    ```
    az vm create \
        --resource-group "[sandbox resource group name]" \
        --name vm1 \
        --location eastUS \
        --image Ubuntu2204 \
        --custom-data cloud-init.txt \
        --generate-ssh-keys
    ```
    

## Create the metric alert using the Azure portal

 Note

Wait until the VM is successfully created before proceeding with the exercise. The VM creation process is complete when you get the completed JSON output in the Azure Cloud Shell window.

You can use either the Azure portal or the CLI to create a metric alert. In this exercise, we cover both, starting with the Azure portal.

1. Sign in to the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com) using the same account that you used to activate the sandbox.
    
2. On the Azure portal menu, search for and select **Monitor**. On the **Monitor Overview** page, select **Alerts**.
    
3. Open the **+ Create** menu, and select **Alert rule**
    
4. On the **Select a resource** pane, set the scope for your alert rule. You can filter by subscription, resource type, or resource location.
    
5. In the **Resource types** drop-down, start to type "virtual machines", and select **Virtual machines**.
    
6. Check the box next to **vm1**, then select **Apply** at the bottom of the pane.
    
    ![Screenshot that shows the 'Select a resource' pane, with `vm1` selected.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/4-select-resource-scope.png)
    
7. Select **Next:Condition** at the bottom of the page.
    
8. In the **Signal name** drop-down, select **Percentage CPU**.
    
9. In the **Alert logic** section, enter (or confirm) the following values for each setting.
    
    
|Setting|Value|
|---|---|
|**Alert logic**||
|Threshold|Static|
|Aggregation type|Maximum|
|Operator|Greater than|
|Threshold value|90|
|**When to evaluate**||
|Check every|1 minute|
|Lookback period|1 minute|
    
    ![Screenshot that shows the settings for metric condition logic.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/4-metric-alert-logic.png)
    
10. Select the **Details** tab at the top of the page. In the **Alert rule details** section, enter the following values for each setting.
    
    
|Setting|Value|
|---|---|
|Severity|2 - Warning|
|Alert rule name|Cpu90PercentAlert|
|Description|Virtual machine is running at or greater than 90% CPU utilization|
    
11. Expand the **Advanced options** section and confirm the following values for each setting.
    
    
|Setting|Value|
|---|---|
|Enable upon creation|Yes (checked)|
|Automatically resolve alerts|Yes (checked)|
    
    [![Screenshot that shows the completed settings for the Alert rule details section.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/4-metric-alert-details.png)](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/4-metric-alert-details.png#lightbox)
    
12. Select **Review + create** to validate your input, and then select **Create**.
    

You successfully created a metric alert rule that triggers an alert when the CPU percentage on the VM exceeds 90 percent. The rule checks every minute and reviews one minute of data. It can take up to 10 minutes for a metric alert rule to become active.

## Create the metric alert through the CLI

You can also set up metric alerts by using the CLI. This process can be quicker than using the portal, especially if you're planning to set up more than one alert.

Let's create a new metric alert similar to the one you set up in the Azure portal.

1. Run the following command in Cloud Shell to obtain the resource ID of the virtual machine you previously created:
    
    BashCopy
    
    ```
    VMID=$(az vm show \
            --resource-group "[sandbox resource group name]" \
            --name vm1 \
            --query id \
            --output tsv)
    ```
    
2. Run the following command to create a new metric alert. The alert is triggered when the VM CPU is greater than 80 percent.
    
    Azure CLICopy
    
    ```
    az monitor metrics alert create \
        -n "Cpu80PercentAlert" \
        --resource-group "[sandbox resource group name]" \
        --scopes $VMID \
        --condition "max percentage CPU > 80" \
        --description "Virtual machine is running at or greater than 80% CPU utilization" \
        --evaluation-frequency 1m \
        --window-size 1m \
        --severity 3
    ```
    

## View your metric alerts in Azure Monitor

In this exercise, you set up an Ubuntu VM and configured it to stress test the CPU. You also created a metric rule to detect when the maximum CPU percentage exceeds 80 percent and 90 percent.

 Note

It might take 10 minutes before you see the alerts show up in the Azure portal.

1. Return to the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com).
    
2. On the Azure portal menu, select **Monitor**, and then select **Alerts** in the left menu pane.
    
    This step presents the Alert summary pane, where you can see the count of the number of alerts. If you don't see your alerts listed, wait a few minutes and select **Refresh**.
    
    ![Screenshot that shows the alert summary pane.' pane.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/4-alert-summary-pane.png)
    
3. You configured your metric alerts with severities of 2 and 3. Select one of the alerts to view the severity level.
    
4. Select one of the alerts to show the alert details.

# Use log search alerts to alert on events in your application


You can use Azure Monitor to capture important information from log files. Applications, operating systems, other hardware, or Azure services can create these log files.

As a solution architect, you want to explore ways that monitoring log data can detect issues before they become issues for your customers. You know that Azure Monitor supports the use of log data.

In this unit, you understand how using log data can improve resilience in your system.

## When to use log search alerts

Log search alerts use log data to assess the rule logic and, if necessary, trigger an alert. This data can come from any Azure resource: server logs, application server logs, or application logs.

By its nature, log data is historical, so usage is focused on analytics and trends.

You can use these types of logs to assess if any of your servers exceeded their CPU utilization by a given threshold during the last 30 minutes. Or, you could evaluate response codes issued on your web application server in the last hour.

## How log search alerts work

Log search alerts behave in a slightly different way than other alert mechanisms. The first part of a log search alert defines the log search rule. The rule defines how often it should run, the time period under evaluation, and the query to be run.

When a log search evaluates as positive, it creates an alert record and triggers any associated actions.

## Composition of log search rules

Every log search alert has an associated search rule with the following composition:

- **Log query**: Query that runs every time the alert rule fires.
- **Time period**: Time range for the query.
- **Frequency**: How often the query should run.
- **Threshold**: Trigger point for an alert to be created.

Log search results are one of two types: _number of records_ or _metric measurement_.

### Number of records

Consider using the number-of-records type of log search when you're working with an event or event-driven data. Examples are syslog and web-app responses.

This type of log search returns a single alert when the number of records in a search result reaches or exceeds the value for the number of records (threshold). For example, when the threshold for the search rule is greater or equal to five, the query results have to return five or more rows of data before the alert is triggered.

### Metric measurement

Metric measurement logs offer the same basic functionality as metric alert logs.

Unlike number-of-records search logs, metric measurement logs require further criteria to be set:

- **Aggregate function**: The calculation to be made against the result data. An example is count or average. The result of the function is called **AggregatedValue**.
- **Group field**: Indicates how the result should be grouped. This criterion is used with the aggregated value. For example, you might specify that you want the average grouped by computer.
- **Interval**: The time interval by which data is aggregated. For example, if you specify 10 minutes, an alert record is created for each aggregated block of 10 minutes.
- **Threshold**: A point defined by an aggregated value and the total number of breaches.

Consider using this type of alert when you need to add a level of tolerance to the results found. One use for this type of alert is to respond if a particular trend or pattern is found. For example, if the number of breaches is five, and any server in your group exceeds 85 percent CPU utilization more than five times within the given time period, an alert fires.

As you can see, metric measurements greatly reduce the volume of alerts that are produced. Still, give careful consideration when you're setting the threshold parameters to avoid missing critical alerts.

## Stateless nature of log search alerts

One of the primary considerations when you're evaluating the use of log search alerts is that they're stateless (stateful log search alerts are [currently in preview](https://learn.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-types#log-alerts)). A stateless log search alert generates new alerts every time the rule criteria are triggered, regardless of whether the alert was previously recorded.

# Use activity log alerts to alert on events within your Azure infrastructure


Activity log alerts allow you to be notified when a specific event happens on some Azure resource. For example, you can be notified when someone creates a new virtual machine (VM) in a subscription.

An activity log can also include alerts for Azure service health. A company can get notifications when service issues or planned maintenance happens on the Azure platform.

As an Azure solution architect, you want to explore the capability to monitor selected Azure resources within your subscription. You want to understand how you can use the resources to improve your team's responsiveness and the stability of your systems.

In this unit, you explore the two different kinds of activity log alerts. Now that you're aware of the different kinds of alerts you can use in Azure Monitor, you want to know how you can trigger actions for your alerts. Actions might include sending an email or creating an IT Service Management (ITSM) support ticket.

## When to use activity log alerts

So far, you learned about two different types of alerts supported in Azure Monitor. _Metric alerts_ are ideally suited to monitoring for threshold breaches or spotting trends; _Log search alerts_ allow for greater analytical monitoring of historical data.

_Activity log alerts_ are designed to work with Azure resources. Typically, you create this type of log to receive notifications when specific changes occur on a resource within your Azure subscription.

There are two types of activity log alerts:

- **Specific operations**: Applies to resources within your Azure subscription, and often has a scope with specific resources or a resource group. You use this type when you need to receive an alert that reports a change to an aspect of your subscription. For example, you can receive an alert if a VM is deleted or new roles are assigned to a user.
- **Service health events**: Include notice of incidents and maintenance of target resources.

## Composition of an activity log alert

It's important to note that activity log alerts monitor events only in the subscription where the alert was created.

Activity log alerts are based on events. The best approach for defining them is to use Azure Monitor to filter all the events in your subscription until you find the one that you want. To begin the creation process, you then select **Add activity log alert**.

Like the previous alerts, activity log alerts have their own attributes:

- **Category**: Administrative, service health, autoscale, policy, or recommendation
- **Scope**: Resource level, resource group level, or subscription level
- **Resource group**: Where the alert rule is saved
- **Resource type**: Namespace for the target of the alert
- **Operation name**: Operation name
- **Level**: Verbose, informational, warning, error, or critical
- **Status**: Started, failed, or succeeded
- **Event initiated by**: Email address or Microsoft Entra identifier (known as the "caller") for the user

### Create a resource-specific log alert

When you create your activity log alert, you select **Activity Log** for the signal type. You then see all the available alerts for the resource you select. The following image shows all the administrative alerts for Azure VMs. In this example, an alert is triggered when a VM is powered off.

Changing the monitor service enables you to reduce the list of options. Selecting **Administrative** filters all the signals to show only admin-related signals.

![Screenshot of the signal logic for activity log alerts related to VMs.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/6-example-activity-log-alert.png)

### Create a service health alert

Service health alerts aren't like all the other alert types you learned about in this module so far. To create a new alert, search for and select **Service Health** in the Azure portal. Next, select **Health alerts**. After you select **Create service health alert**, the steps to create the alert are similar to the steps used to create other alerts.

![Screenshot that shows how to create a new service health alert.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/6-service-health-alerts.png)

The only difference is that you no longer need to select a resource, because the alert is for a whole region in Azure. What you can select is the kind of health event on which you want to be alerted. You can select service issues, planned maintenance, health advisories, or choose all of the events. The remaining steps of performing actions and naming the alerts are the same.

# Use action groups and alert processing rules to send notifications when an alert is fired


Azure Monitor, Azure Service Health, and Azure Advisor use _action groups_ to notify users about the alert, and to take an action when an alert is fired. An action group is a collection of notification preferences and actions that are executed when the alert is fired. You can run one or more actions for each triggered alert.

Azure Monitor can perform any of the following actions:

- Send an email
- Send a Short Message Service (SMS) message
- Create an Azure app push notification
- Make a voice call to a number
- Call an Azure function
- Trigger a logic app
- Send a notification to a webhook
- Create an IT Service Management (ITSM) ticket
- Use a runbook (to restart a virtual machine (VM) or scale a VM up or down)

Once you create an action group, you can reuse that action group as often as you want. For example, after you create an action to email your company's operations team, you can add that action group to all service-health events.

While you're creating the alert rule, you can either create a new action group or add an existing action group to the alert rule. You can also edit an existing alert to add an action group.

## Alert processing rules

Use alert processing rules to override the normal behavior of a fired alert by adding or suppressing an action group. You can use alert processing rules to add action groups or remove (suppress) action groups from your fired alerts. Alert processing rules are different from alert rules. Alert rules trigger alerts when a condition is met in your monitored resources. Alert processing rules modify the alerts as they're being fired.

You can use alert processing rules to:

- Suppress notifications during planned maintenance windows.
- Implement management at scale, by specifying commonly used logic in a single rule, instead of having to set it consistently in all your alert rules.
- Add an action group to all alert types.

You can apply alert processing rules to different resource scopes, from a single resource, or to an entire subscription. You can also use them to apply various filters or have the rule work on a predefined schedule.

You can control when the alert processing rule applies. By default the rule is always active. But, you can select a one-time window for this rule to apply, or you can set a recurrence such as a weekly recurrence.

# Exercise -Use an activity log alert and an action group to notify users about events in your Azure infrastructure


The shipping company for which you work wants to avoid any future issues with updates to its applications on the Azure platform. To improve the alerting capabilities within Azure, you can create activity log alerts.

Your goal is to set up a Linux virtual machine (VM) and create an activity log monitoring rule to detect when a VM is deleted. Then, delete the VM to trigger this alert.

## Create the Azure activity log monitor

1. Sign in to the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com) with the same account you used to activate the sandbox.
    
2. On the Azure portal resource menu or under **Azure services**, select **Monitor**. The **Overview** pane for Monitor appears.
    
3. In the Monitor menu, select **Alerts**. The **Monitor | Alerts** pane appears.
    
4. On the command bar, select **Create +** and select **Alert rule**. The **Create an alert rule** pane appears with the **Scope** section open and the **Select a resource** pane open on the right-hand side of your screen.
    
5. In the **Resource type** dropdown list, search for and select **Virtual machines**.
    
6. You want an alert when any virtual machine in your resource group is deleted. Select the box for the **[sandbox resource group name]** resource group, then select **Apply**.
    
    ![Screenshot that shows the Select a scope pane with the sandbox resource group selected.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/7-alert-select-resource.png)
    
7. The **Create an alert rule** pane reappears with the Scope target resource showing **All Virtual machines**. Select the **Condition** tab. The **Select a signal** pane appears.
    
8. Select the **See all signals** link, then search for and select **Delete Virtual Machine (Virtual Machines)**. Select **Apply**
    
9. The **Create an alert rule** pane reappears. You want to receive alerts of all types, so leave **Alert logic** settings at their default of **All selected**. Leave the **Create an alert rule** pane open for the next section.
    

## Add an email alert action

For the previous Azure Monitor alert, you didn't add any actions. You just viewed triggered alerts in the Azure portal. Actions let you send an email for notifications, to trigger an Azure function, or to call a webhook. In this exercise, we're adding an email alert when VMs are deleted.

1. On the **Create an alert rule** pane, select the **Next: Actions** button, and select **Use action groups**.
    
2. Select **Create action group** in the **Select action group** pane. The **Create an action group** pane appears.
    
3. On the **Basics** tab, enter the following values for each setting.
    
| Setting              | Value                                                      |
| -------------------- | ---------------------------------------------------------- |
| **Project details**  |                                                            |
| Subscription         | **Concierge Subscription**                                 |
| Resource group       | From the dropdown list, select your sandbox resource group |
| Region               | **Global** (default)                                       |
| **Instance details** |                                                            |
| Action group name    | **Alert the operations team**                              |
| Display name         | **AlertOps**                                               |
    
4. Select **Next: Notifications**, and enter the following values for each setting.
    
|Setting|Value|
|---|---|
|Notification type|Select **Email/SMS message/Push/Voice**|
|Name|**VM was deleted**|
    
5. The **Email/SMS message/Push/Voice** pane appears automatically. If it didn't, select the **Edit** pencil icon.
    
6. Select **Email**, and in the **Email** box, enter your email address, and then select **OK**.
    
7. Select **Review + create** to validate your input.
    
8. Select **Create**.
    
9. The **Create an alert rule** pane reappears. Select **Next: Details** and enter the following values for each setting.
    
| Setting         | Value                                       |
| --------------- | ------------------------------------------- |
| Alert rule name | **VM was deleted**                          |
| Description     | **A VM in your resource group was deleted** |
    
10. Expand the **Advanced options** section and confirm that **Enable alert rule upon creation** is selected.
    
    ![Screenshot that shows a completed alert details section.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/7-all-vm-alert-details.png)
    
11. Select **Review + create** to validate your input, then select **Create**.
    

Recipients added to the configured action group (operations team) receive a notification:

- When they're added to the action group
- When the alert is activated
- When the alert is triggered

It can take up to five minutes for an activity log alert rule to become active. In this exercise, if you delete the virtual machine before the rule deploys, the alert rule might not be triggered. Because of this delay, you might not see the same results in the following steps after you delete the VM.

## Delete your virtual machine

To trigger an alert, you need to delete the Linux VM that you created in the previous exercise.

1. On the Azure portal menu or from the **Home** page, select **Virtual machines**.
    
2. Check the box for the **vm1** virtual machine.
    
3. Select **Delete** from the menu bar.
    
4. Enter _delete_ to confirm deletion and select **Delete**.
    
5. In the title bar, select the **Notifications** icon and wait until **vm1** is successfully deleted.
    

## View your activity log alerts in Azure Monitor

In the exercise, you set up an Ubuntu VM and created an activity log rule to detect when the VM was deleted. You then deleted a VM from your resource group. Let's check whether an alert was triggered.

1. You should receive a notification email that reads, **Important notice: Azure Monitor alert VM was deleted was activated...** If not, open your email program and look for an email from azure-noreply@microsoft.com.
    
    ![Screenshot of alert email.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/7-alert-email.png)
    
2. On the Azure portal resource menu, select **Monitor**, and then select **Alerts** in the menu on the left.
    
3. You should have three verbose alerts that were generated by deleting **vm1**.
    
    ![Screenshot that shows all alerts with Name, Severity, Alert condition, User response and Fired time.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/7-vm-rg-deleted-alert.png)
    
4. Select the name of one of the alerts (For example, **VM was deleted**). An **Alert details** pane appears that shows more details about the event.
    

## Add an alert processing rule to the alert

We're going to schedule a one-time, overnight, planned maintenance. It starts in the evening and continues until the next morning.

1. In the Azure portal resource menu, select **Monitor**, select **Alerts** in the menu on the left, and select **Alert processing rules** in the menu bar.
    
2. Select **+ Create**.
    
3. Check the box for your sandbox resource group as the scope of the alert processing rule, then select **Apply**.
    
4. Select **Next: Rule settings**, then select **Suppress notifications**.
    
5. Select **Next: Scheduling**.
    
6. By default, the rule works all the time, unless you disable it. We're going to define the rule to suppress notifications for a one-time overnight planned maintenance. Enter these settings for the scheduling of the alert processing rule:
    
| Setting        | Value                         |
| -------------- | ----------------------------- |
| Apply the rule | At a specific time            |
| Start          | Enter today's date at 10pm.   |
| End            | Enter tomorrow's date at 7am. |
| Time zone      | Select the local timezone.    |
    
![Screenshot of the scheduling section of an alert processing rule.](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/8-alert-processing-rule-schedule.png)[https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/8-alert-processing-rule-schedule.png#lightbox](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/media/8-alert-processing-rule-schedule.png#lightbox)
    
7. Select **Next: Details** and enter these settings:
    
| Setting        | Value                                                  |
| -------------- | ------------------------------------------------------ |
| Resource group | Select your sandbox resource group.                    |
| Rule name      | **Planned Maintenance**                                |
| Description    | **Suppress notifications during planned maintenance.** |
    
8. Select **Review + create** to validate your input, then select **Create**.

---

# Analyze your Azure infrastructure by using Azure Monitor logs

# Introduction

Logging and monitoring the health of your services is a vital component of production applications. You need to be able to determine the causes of failures. You also need to identify any problems before they occur.

Azure Monitor is an important tool to help you in this process. It allows you to gather monitoring and diagnostic information about the health of your services. You can use this information to visualize and analyze the causes of problems that might occur in your app.

Suppose that you work for a large organization's operations team. The organization is running large-scale production apps in the cloud. The team wants to consolidate its log data in a single service to improve visibility across services and simplify its logging strategy.

The team began implementing Azure Monitor logs. It wants to fully understand how the logs work. It also wants to know the service's capabilities to query and evaluate the log data that's fed into it.

## Learning objectives

In this module, you'll:

- Identify the features and capabilities of Azure Monitor logs.
- Create basic Azure Monitor log queries to extract information from log data.

# Features of Azure Monitor logs

Azure Monitor is a service for collecting and analyzing telemetry. It helps you get maximum performance and availability for your cloud applications and for your on-premises resources and applications. It shows how your applications are performing and identifies any issues with them.

## Data collection in Azure Monitor

Azure Monitor collects two fundamental types of data: metrics and logs. Metrics tell you how a resource is performing and the other resources that it's consuming. Logs contain records that show when resources are created or modified.

The following diagram gives a high-level view of Azure Monitor. On the left are the data-monitoring sources: Azure, operating systems, and custom sources. At the center of the diagram are the data stores for metrics and logs. On the right are the functions that Azure Monitor performs with this collected data, such as analysis, alerting, and streaming to external systems.

![Diagram of Azure Monitor's architecture, displaying the sources of monitoring data, the data stores, and functions performed on the data.](https://learn.microsoft.com/en-us/training/modules/analyze-infrastructure-with-azure-monitor-logs/media/2-azure-monitor.svg)

Azure Monitor collects data automatically from a range of components. For example:

- **Application data**: Data that relates to your custom application code.
- **Operating-system data**: Data from the Windows or Linux virtual machines that host your application.
- **Azure resource data**: Data that relates to the operations of an Azure resource, such as a web app or a load balancer.
- **Azure subscription data**: Data that relates to your subscription, including data about Azure health and availability.
- **Azure tenant data**: Data about your Azure organization-level services, such as Microsoft Entra ID.

Because Azure Monitor is an automatic system, it begins to collect data from these sources as soon as you create Azure resources like virtual machines and web apps. You can extend the data that Azure Monitor collects by:

- **Enabling diagnostics**: For some resources, such as Azure SQL Databases, you'll receive full information about a resource only after you've enabled diagnostic logging for it. You can use the Azure portal, the Azure CLI, or PowerShell to enable diagnostics.
- **Adding an agent**: For virtual machines, you can install the Log Analytics agent and configure it to send data to a Log Analytics workspace. This agent increases the amount of information that's sent to Azure Monitor.

Your developers might also want to send data to Azure Monitor from custom code such as a web app, an Azure function, or a mobile app. They send data by calling the Data Collector API. You can communicate with this REST interface through HTTP. This interface is compatible with various development frameworks such as .NET Framework, Node.js, and Python. Developers can choose their favorite language and framework to log data in Azure Monitor.

### Logs

Logs contain time-stamped information about resource changes. The type of information recorded varies by log source. The log data is organized into records, with different sets of properties for each type of record. The logs can include numeric values like Azure Monitor metrics, but most include text data rather than numeric values.

The most common type of log entry records an event. Events can occur sporadically rather than at fixed intervals or according to a schedule. Applications and services create events and provide the event context. You can store metric data in logs to combine them with other monitoring data for analysis.

You can log data from Azure Monitor in a Log Analytics workspace. Azure provides an analysis engine and a rich query language. The logs show the context of any problems, and they're useful for identifying root causes.

![Screenshot of an example query against Azure logs with the query text on top and a graph displaying the results below.](https://learn.microsoft.com/en-us/training/modules/analyze-infrastructure-with-azure-monitor-logs/media/2-azure-logs-query-example.png)

### Metrics

Metrics are numerical values that describe some aspect of a system at a point in time. Azure Monitor can capture metrics in near-real time. The metrics are collected at regular intervals, and they're useful for alerting because of their frequent sampling. You can use various algorithms to compare a metric to other metrics and observe trends over time.

Metrics are stored in a time-series database. This data store is most effective for analyzing time-stamped data. Metrics are suited for alerting and fast detection of issues. They can tell you about system performance. If needed, you can combine them with logs to identify the root cause of issues.

![Screenshot of an example chart in Azure Metrics displaying average CPU percentage.](https://learn.microsoft.com/en-us/training/modules/analyze-infrastructure-with-azure-monitor-logs/media/2-azure-monitor-metrics.png)

## Analyzing logs by using Kusto

To retrieve, consolidate, and analyze data, you can specify a query to run in Azure Monitor logs. You can write a log query with the Kusto query language, which Azure Data Explorer also uses.

You can test log queries in the Azure portal so you can work with them interactively. You'll typically start with basic queries, then progress to more advanced functions as your requirements become more complex.

In the Azure portal, you can create custom dashboards, which are targeted displays of resources and data. You can build each dashboard from a set of tiles. Each tile might show a set of resources, a chart, a data table, or some custom text. Azure Monitor provides tiles that you can add to dashboards; for example, you might use a tile to display a Kusto query's results in a dashboard.

In the example scenario, the operations team can consolidate its monitoring data by visualizing it in charts and tables. These tools are effective for summarizing data and presenting it to different audiences.

By using Azure dashboards, you can combine various kinds of data—including both logs and metrics—into a single pane in the Azure portal. For example, you might want to create a dashboard that combines tiles that show a graph of metrics, a table of activity logs, charts from Azure Monitor, and a log query's output.

# Create basic Azure Monitor log queries to extract information from log data

You can use Azure Monitor log queries to extract information from log data. Querying is an important part of examining the log data that Azure Monitor captures.

In the example scenario, the operations team uses Azure Monitor log queries to examine the health of its system.

## Write Azure Monitor log queries by using Log Analytics

You can find the Log Analytics tool in the Azure portal and use it to run sample queries or to create your own queries:

1. In the Azure portal, in the left menu pane, select **Monitor**.
    
    The Azure Monitor page appears along with more options, including **Activity Log**, **Alerts**, **Metrics**, and **Logs**.
    
2. Select **Logs**.
    
    Here, you can enter your query and see the output.
    
    ![Screenshot of Azure Monitor with a new query tab opened.](https://learn.microsoft.com/en-us/training/modules/analyze-infrastructure-with-azure-monitor-logs/media/3-azure-monitor-portal-query-pane.png)
    

## Write queries by using the Kusto language

You can use the Kusto Query Language to query log information for your services running in Azure. A Kusto query is a read-only request to process data and return results. You'll state the query in plain text by using a data-flow model that's designed to make the syntax easy to read, write, and automate. The query uses schema entities that are organized in a hierarchy similar to that of Azure SQL Database: databases, tables, and columns.

A Kusto query consists of a sequence of query statements delimited by a semicolon (`;`). At least one statement is a tabular expression statement. A tabular expression statement formats the data arranged as a table of columns and rows.

A tabular expression statement's syntax has a tabular data flow from one tabular query operator to another, starting with a data source. A data source might be a table in a database or an operator that produces data. The data then flows through a set of data-transformation operators that are bound together with the pipe (`|`) delimiter.

For example, the following Kusto query has a single tabular expression statement. The statement starts with a reference to a table called `Events`. The database that hosts this table is implicit here, and is part of the connection information. The data for that table, stored in rows, is filtered by the value of the `StartTime` column. The data is filtered further by the value of the `State` column. The query then returns the count of the resulting rows.

KustoCopy

```
Events
| where StartTime >= datetime(2018-11-01) and StartTime < datetime(2018-12-01)
| where State == "FLORIDA"  
| count
```

 Note

The Kusto query language that Azure Monitor uses is case-sensitive. Language keywords are typically written in lowercase. When you're using names of tables or columns in a query, make sure to use the correct case.

Events captured from the event logs of monitored computers are just one type of data source. Azure Monitor provides many other types of data sources. For example, the `Heartbeat` data source reports the health of all computers that report to your Log Analytics workspace. You can also capture data from performance counters and update management records.

The following example retrieves the most recent heartbeat record for each computer. The computer is identified by its IP address. In this example, the `summarize` aggregation with the `arg_max` function returns the record with the most recent value for each IP address.

KustoCopy

```
Heartbeat
| summarize arg_max(TimeGenerated, *) by ComputerIP
```

# Exercise - Create basic Azure Monitor log queries to extract information from log data

The operations team doesn't currently have enough information about its system behavior to diagnose and resolve problems effectively. To address this issue, the team has configured an Azure Monitor workspace with the company's Azure services. It runs Kusto queries to get the system's status and attempts to identify the causes of any problems that might occur.

In particular, the team is interested in monitoring security events to check for possible attempts to break into the system. An attacker might try to manipulate the applications running on the system, so the team also wants to gather application data for further analysis. An attacker might also try to halt the computers that compose the system, so the team wants to examine how and when machines are stopped and restarted.

In this exercise, you'll practice performing Azure log queries against a demo project that contains sample data in tables, logs, and queries.

## Create basic Azure Monitor log queries to extract information from log data

Let's use the **Azure Demo Logs pane** to practice writing queries. The demo project workspace is prepopulated with sample data. Azure offers an optimized SQL-like query with visualization options of its data in a language called KQL (Kusto Query Language.)

1. Open the [Logs demo environment](https://portal.azure.com/learn.docs.microsoft.com/#blade/Microsoft_Azure_Monitoring_Logs/DemoLogsBlade?azure-portal=true). In the top-left corner, under **New Query 1**, you'll find **Demo**, which identifies the query's workspace (or scope). The left side of this pane contains several tabs: **Tables**, **Queries**, and **Functions**. The right side has a scratchpad for creating or editing queries.
    
2. On the **New Query 1** tab, enter a basic query on the first line of the scratchpad. This query retrieves the details of the 10 most recent security events.
    
    KustoCopy
    
    ```
    SecurityEvent
        | take 10
    ```
    
3. In the command bar, select **Run** to execute the query and view the results. You can expand each row in the results pane for more information.
    
4. Sort the data by time by adding a filter to your query:
    
    KustoCopy
    
    ```
    SecurityEvent
        | top 10 by TimeGenerated
    ```
    
5. Add a filter clause and a time range. Run this query to fetch records that are more than 30 minutes old and that have a level of 10 or more:
    
    KustoCopy
    
    ```
    SecurityEvent
        | where TimeGenerated < ago(30m)
        | where toint(Level) >= 10
    ```
    
6. Run the following query to search the `AppEvents` table for records of the `Clicked Schedule Button` event being invoked in the last 24 hours:
    
    KustoCopy
    
    ```
    AppEvents 
        | where TimeGenerated > ago(24h)
        | where Name == "Clicked Schedule Button"
    ```
    
7. Run the following query to display the number of different computers that generated heartbeat events each week for the last three weeks. The results appear as a bar chart:
    
    KustoCopy
    
    ```
    Heartbeat
        | where TimeGenerated >= startofweek(ago(21d))
        | summarize dcount(Computer) by endofweek(TimeGenerated) | render barchart kind=default
    ```
    

## Use predefined Azure log queries to extract information from log data

In addition to writing queries from scratch, the operations team can also take advantage of predefined queries in Azure Logs that answer common questions related to their resources' health, availability, usage, and performance.

1. Use the **Time Range** parameter in the command bar to set a custom range. Select the month, year, and day to a range from January to today. You can set and apply a custom time to any query.
    
2. On the left toolbar, select **Queries**. In the drop-down list in the left menu, you can view a list of the sample queries grouped by _Category_, _Query type_, _Resource type_, _Solution_, or _Topic_.
    
    ![Screenshot of Group By dropdown options under Queries tab in side pane.](https://learn.microsoft.com/en-us/training/modules/analyze-infrastructure-with-azure-monitor-logs/media/4-azure-queries-category-groupby.png)
    
3. Select **Category** in the drop-down list, then select **IT & Management Tools**.
    
4. In the search box, enter _Distinct missing updates cross computers_. Double-click the query in the left pane, then select **Run**. The **Logs** pane reappears, with the query returning a list of Windows updates missing from virtual machines that are sending logs to the workspace.
    
     Note
    
    You can also run this same query from the **Logs** pane. In the left pane, select the **Queries** tab, then select **Category** in the **Group by** dropdown list. Now scroll down the list, expand **IT & Management Tools**, and double-click **Distinct missing updates cross computers**. Select **Run** to run the query. When you select a predefined query in the left pane, the query code is appended to whatever query exists in the scratchpad. Remember to clear the scratchpad before opening or adding a new query to run.
    
5. In the left pane, clear the search box. Select **Queries**, then select **Category** in the **Group by** dropdown list. Expand **Azure Monitor** and double-click **Computers availability today**. Select **Run**. This query creates a time series chart with the number of unique IP addresses sending logs into the workspace each hour for the last day.
    
6. Select **Topic** in the **Group by** dropdown list, scroll down to expand **Function App**, and then double-click **Show application logs from Function Apps**. Select **Run**. This query returns a list of application logs, sorted by time with the latest logs shown first.
    

You'll notice that from the Kusto queries you used here, it's easy to target a query to a specific time window, event level, or event log type. The security team can easily examine heartbeats to identify when servers are unavailable, which might indicate a denial-of-service attack. If the team spots the time when a server was unavailable, it can query for events in the security log around that time to diagnose whether an attack caused the interruption. Additionally, predefined queries can also evaluate VM availability, identify missing Windows updates, and review firewall logs to view denied network flows intended for the VMs of interest.

---

# Monitor your Azure virtual machines with Azure Monitor

# Introduction

Suppose you're the IT administrator for a musical group's website hosted on Azure virtual machines (VMs). The website runs mission-critical services for the group, including ticket booking, venue information, and tour updates. The website must respond quickly and remain accessible during frequent updates and spikes in traffic.

You need to maintain sufficient VM size and memory to effectively host the website without incurring unnecessary costs. You also need to proactively prevent and quickly respond to any access, security, and performance issues. To help achieve these objectives, you want to quickly and easily monitor your VMs' traffic, health, performance, and events.

Azure Monitor provides built-in and customizable monitoring abilities. You can use these to track the health, performance, and behavior of the VM host and the operating system, workloads, and applications running on your VM. This learning module shows you how to view VM host monitoring data, set up recommended alert rules, and use VM insights and custom data collection rules (DCRs) to collect and analyze monitoring data from inside your VMs.

## Prerequisites

To complete this module, you need the following prerequisites:

- Familiarity with virtualization, Azure portal navigation, and Azure VMs.
- Access to an Azure subscription with at least **Contributor** role. If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) and add a subscription before you begin. If you're a student, you can take advantage of the [Azure for students](https://azure.microsoft.com/free/students/) offer.

## Learning objectives

- Understand which monitoring data you need to collect from your VM.
- Enable and view recommended alerts and diagnostics.
- Use Azure Monitor to collect and analyze VM host data.
- Use Azure Monitor Agent to collect VM client performance metrics and event logs.

# Introduction

Suppose you're the IT administrator for a musical group's website hosted on Azure virtual machines (VMs). The website runs mission-critical services for the group, including ticket booking, venue information, and tour updates. The website must respond quickly and remain accessible during frequent updates and spikes in traffic.

You need to maintain sufficient VM size and memory to effectively host the website without incurring unnecessary costs. You also need to proactively prevent and quickly respond to any access, security, and performance issues. To help achieve these objectives, you want to quickly and easily monitor your VMs' traffic, health, performance, and events.

Azure Monitor provides built-in and customizable monitoring abilities. You can use these to track the health, performance, and behavior of the VM host and the operating system, workloads, and applications running on your VM. This learning module shows you how to view VM host monitoring data, set up recommended alert rules, and use VM insights and custom data collection rules (DCRs) to collect and analyze monitoring data from inside your VMs.

## Prerequisites

To complete this module, you need the following prerequisites:

- Familiarity with virtualization, Azure portal navigation, and Azure VMs.
- Access to an Azure subscription with at least **Contributor** role. If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) and add a subscription before you begin. If you're a student, you can take advantage of the [Azure for students](https://azure.microsoft.com/free/students/) offer.

## Learning objectives

- Understand which monitoring data you need to collect from your VM.
- Enable and view recommended alerts and diagnostics.
- Use Azure Monitor to collect and analyze VM host data.
- Use Azure Monitor Agent to collect VM client performance metrics and event logs.

# Monitoring for Azure VMs

In this unit, you explore Azure monitoring capabilities for VMs, and the types of monitoring data you can collect and analyze with Azure Monitor. Azure Monitor is a comprehensive monitoring solution for collecting, analyzing, and responding to monitoring data from Azure and non-Azure resources, including VMs. Azure Monitor has two main monitoring features: Azure Monitor Metrics and Azure Monitor Logs.

Metrics are numerical values collected at predetermined intervals to describe some aspect of a system. Metrics can measure VM performance, resource utilization, error counts, user responses, or any other aspect of the system that you can quantify. Azure Monitor Metrics automatically monitors a predefined set of metrics for every Azure VM, and retains the data for 93 days with some exceptions.

Logs are recorded system events that contain a timestamp and different types of structured or free-form data. Azure automatically records activity logs for all Azure resources. This data is available at the resource level. Azure Monitor doesn't collect logs by default, but you can configure Azure Monitor Logs to collect from any Azure resource. Azure Monitor Logs stores log data in a Log Analytics workspace for querying and analysis.

## VM monitoring layers

Azure VMs have several layers that require monitoring. Each of the following layers has a distinct set of telemetry and monitoring requirements.

- Host VM
- Guest operating system (OS)
- Client workloads
- Applications that run on the VM

![Diagram that shows fundamental VM architecture.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/monitoring-layers.png)

## Host VM monitoring

The VM host represents the compute, storage, and network resources that Azure allocates to the VM.

### VM host metrics

VM host metrics measure technical aspects of the VM such as processor utilization and whether the machine is running. You can use VM host metrics to:

- Trigger an alert when your VM is reaching its disk or CPU limits.
- Identify trends or patterns.
- Control your operational costs by sizing VMs according to usage and demand.

Azure automatically collects basic metrics for VM hosts. On the VM's **Overview** page in the Azure portal, you can see built-in graphs for the following important VM host metrics.

- VM availability
- CPU usage percentage (average)
- OS disk usage (total)
- Network operations (total)
- Disk operations per second (average)

You can use Azure Monitor Metrics Explorer to plot more metrics graphs, investigate changes, and visually correlate metrics trends for your VMs. With Metrics Explorer you can:

- Plot multiple metrics on a graph to see how much traffic hits your VM and how the VM performs.
- Track the same metric over multiple VMs in a resource group or other scope, and use splitting to show each VM on the graph.
- Select flexible time ranges and granularity.
- Specify many other settings such as chart type and value ranges.
- Send graphs to workbooks or pin them to dashboards for quickly viewing health and performance.
- Group metrics by time intervals, geographic regions, server clusters, or application components.

[![Screenshot showing CPU percentage usage and inbound flow chart.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/2-vm-metrics-screenshot.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/2-vm-metrics-screenshot.png#lightbox)

### Recommended alert rules

Alerts proactively notify you of specified occurrences and patterns in your VM host metrics. _Recommended alert rules_ are a predefined set of alert rules based on commonly monitored host metrics. These rules define recommended CPU, memory, disk, and network usage levels to alert on. The rules also include VM availability, which alerts you when the VM stops running.

You can quickly enable and configure recommended alert rules when you create an Azure VM, or afterwards from the VM's portal page. You can also view, configure, and create custom alerts by using Azure Monitor Alerts.

### Activity logs

Azure Monitor automatically records and displays activity logs for Azure VMs. Activity logs include information like VM startup or modifications. You can create diagnostic settings to send activity logs to the following destinations:

- **Azure Monitor Logs:** For more complex querying and alerting, and for longer retention up to two years.
- **Azure Storage:** For cheaper, long-term archiving.
- **Azure Event Hubs:** To forward outside of Azure.

### Boot diagnostics

Boot diagnostics are host logs you can use to help troubleshoot boot issues with your VMs. You can enable boot diagnostics by default when you create a VM, or afterwards for existing VMs.

Once you enable boot diagnostics, you can see screenshots from the VM's hypervisor for both Windows and Linux machines, and view the serial console log output of the VM boot sequence for Linux machines. Boot diagnostics stores data in a managed storage account.

## Guest OS, client workload, and application monitoring

VM client monitoring can include monitoring the operating system (OS), workloads, and applications that run on the VM. To collect metrics and logs from guest OS and client workloads and applications, you need to install Azure Monitor Agent and set up a DCR.

DCRs define what data to collect and where to send that data. You can use a DCR to send Azure Monitor metrics data, or _performance counters_, to Azure Monitor Logs or Azure Monitor Metrics. You can also send event log data to Azure Monitor Logs. In other words, Azure Monitor Metrics can store only metrics data, but Azure Monitor Logs can store both metrics and event logs.

### VM insights

VM insights is an Azure Monitor feature that helps get you started monitoring your VM clients. VM insights is especially useful for exploring overall VM usage and performance when you don't yet know the metric of primary interest. VM insights provides:

- Simplified Azure Monitor Agent onboarding to enable monitoring a VM's guest OS and workloads.
- A preconfigured DCR that monitors and collects the most common performance counters for Windows and Linux.
- Predefined trending performance metrics charts and workbooks from the VM's guest OS.
- A set of predefined workbooks that show collected VM client metrics over time.
- Optionally, a collection of processes running on the VM, dependencies with other services, and a dependency map that displays interconnected components with other VMs and external sources.

Predefined VM insights workbooks show performance, connections, active ports, traffic, and other collected data from one or several VMs. You can view VM insights data directly from a single VM, or see a combined view of multiple VMs to view and assess trends and patterns across VMs. You can edit the prebuilt workbook configurations, or create your own custom workbooks.

### Client event log data

VM insights creates a DCR that collects a specific set of performance counters. To collect other data, such as event logs, you can create a separate DCR that specifies the data you want to collect from the VM and where to send it. Azure Monitor stores collected log data in a Log Analytics workspace. From there, you can access and analyze the data by using log queries written in Kusto Query Language (KQL).

# Monitor VM host data

You want to monitor the VMs that host your website, so you decide to quickly create a VM in the Azure portal and evaluate its built-in monitoring capabilities. In this unit, you use the Azure portal to create a Linux VM with recommended alerts and boot diagnostics enabled. As soon as the VM starts up, Azure automatically begins collecting basic metrics and activity logs. You can then view built-in metrics graphs, activity logs, and boot diagnostics.

## Create a VM and enable recommended alerts

1. Sign in to the [Azure portal](https://portal.azure.com/), and in the Search field, enter _virtual machines_.
    
2. On the **Virtual machines** page, select **Create**, and then select **Azure virtual machine**.
    
3. On the **Basics** tab of the **Create a virtual machine** page:
    
    - In the **Subscription** field, select the correct subscription if not already selected.
    - Under **Resource group**:
        1. Select **Create new**.
        2. Under **Name**, enter _learn-monitor-vm-rg_.
        3. Select **OK**.
    - For **Virtual machine name**, enter _monitored-linux-vm_.
    - For **Image**, select **Ubuntu Server 20.04 LTS - x64 Gen2**.
4. Leave the other settings at their current values, and select the **Monitoring** tab.
    
    [![Screenshot that shows the Basics tab of the Create a virtual machine page.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/create-vm-basic.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/create-vm-basic.png#lightbox)
    
5. On the **Monitoring** tab, select the checkbox next to **Enable recommended alert rules**.
    
6. On the **Set up recommended alert rules** screen:
    
    1. Select all the listed alert rules if not already selected, and adjust the values if desired.
    2. Under **Notify me by**, select the checkbox next to **Email**, and enter an email address to receive alert notifications.
    3. Select **Save**.
7. Under **Diagnostics**, for **Boot diagnostics**, ensure that **Enable with managed storage account (recommended)** is selected.
    
     Note
    
    Don't select **Enable OS guest diagnostics**. The Linux Diagnostics Agent (LAD) is deprecated, and you can enable OS guest and client monitoring later.
    
8. Select **Review + create** at the bottom of the page, and when validation passes, select **Create**.
    
    [![Screenshot that shows the Monitoring tab and alert rule configuration screen of the Create a virtual machine page.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/create-vm-monitoring.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/create-vm-monitoring.png#lightbox)
    
9. On the **Generate new key pair** popup dialog box, select **Download private key and create resource**.
    

It can take a few minutes to create the VM. When you get the notification that the VM is created, select **Go to resource** to see basic metrics data.

## View built-in metrics graphs

Once your VM is created, Azure starts collecting basic metrics data automatically. Built-in metrics graphs, along with the recommended alerts you enabled, can help you monitor whether and when your VM encounters health or performance issues. You can then use more advanced monitoring and analytics capabilities to investigate issue causes and remediation.

1. To view basic metrics graphs, on the VMs **Overview** page, select the **Monitoring** tab.
    
    [![Screenshot that shows Monitoring tab on a VMs Overview screen.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/select-monitoring.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/select-monitoring.png#lightbox)
    
2. Under **Performance and utilization** > **Platform metrics**, review the following metrics graphs related to the VMs performance and utilization. Select **Show more metrics** if all the graphs don't appear immediately.
    
    - **VM Availability**
    - **CPU (average)**
    - **Disk bytes (total)**
    - **Network (total)**
    - **Disk operations/sec (average)**
    
    [![Screenshot that shows the platform metrics graphics on the VM Overview page.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/platform-metrics.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/platform-metrics.png#lightbox)
    
3. Under **Guest OS metrics**, notice that guest OS metrics aren't being collected yet. In the next units, you configure VM insights and data collection rules to collect guest OS metrics.
    

## View the activity log

You can view the VMs activity log by selecting **Activity log** from the VMs left navigation menu. You can also retrieve entries by using PowerShell or the Azure CLI.

[![Screenshot of the activity log for a VM.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/activity-log.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/activity-log.png#lightbox)

## View boot diagnostics

You enabled boot diagnostics when you created the VM. You can view boot diagnostics to view boot data and troubleshoot startup issues.

1. In the left navigation menu for the VM, select **Boot diagnostics** under **Help**.
    
2. On the **Boot diagnostics** page, select **Screenshot** to see a startup screenshot from the VMs hypervisor. Select **Serial log** to view log messages created when the VM started.
    
    [![Screenshot that shows the boot diagnostic image captured.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/3-boot-diagnostics.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/3-boot-diagnostics.png#lightbox)

# Use Metrics Explorer to view detailed host metrics

You want to investigate how traffic flowing into your VM affects its CPU capability. If the built-in metrics charts for a VM don't already show the data you need, you can use Metrics Explorer to create customized metrics charts. In this unit, you plot a graph that displays your VM's maximum percentage CPU and average inbound flow data together.

Azure Monitor Metrics Explorer offers a UI for exploring and analyzing VM metrics. You can use Metrics Explorer to view and create custom charts for many VM host metrics in addition to the metrics shown on the built-in graphs.

## Understand Metrics Explorer

To open Metrics Explorer, you can:

- Select **Metrics** from the VM's left navigation menu under **Monitoring**.
- Select the **See all Metrics** link next to **Platform metrics** on the **Monitoring** tab of the VM's **Overview** page.
- Select **Metrics** from the left navigation menu on the Azure Monitor **Overview** page.

[![Screenshot that shows Metrics Explorer.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/metrics-explorer.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/metrics-explorer.png#lightbox)

In Metrics Explorer, you can select the following values from the dropdown fields:

- **Scope:** If you open Metrics Explorer from a VM, this field is prepopulated with the VM name. You can add more items with the same resource type (VMs) and location.
- **Metric Namespace**: Most resource types have only one namespace, but for some types, you must pick a namespace. For example, storage accounts have separate namespaces for files, tables, blobs, and queues.
- **Metric**: Each metrics namespace has many metrics available to choose from.
- **Aggregation**: For each metric, Metrics Explorer applies a default aggregation. You can use a different aggregation to get different information about the metric.

You can apply the following aggregation functions to metrics:

- **Count**: Counts the number of data points.
- **Average (Avg)**: Calculates the arithmetic mean of values.
- **Maximum (Max)**: Identifies the highest value.
- **Minimum (Min)**: Identifies the lowest value.
- **Sum**: Adds up all the values.

You can select flexible time ranges for graphs from the past 30 minutes to the last 30 days, or custom ranges. You can specify time interval granularity from one minute to one month.

## Create a metrics graph

To create a Metrics Explorer graph that shows host VM maximum percentage CPU and inbound flows together for the past 30 minutes:

1. Open **Metrics Explorer** by selecting **See all Metrics** on the VM's **Monitoring** tab or selecting **Metrics** from the VM's left navigation menu.
    
2. **Scope** and **Metric Namespace** are already populated for the host VM. Select **Percentage CPU** from the **Metrics** dropdown list.
    
3. **Aggregation** is automatically populated with **Avg**, but change it to **Max**.
    
    [![Screenshot of the Percentage CPU metrics graph for a VM.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/3-view-host-level-metrics.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/3-view-host-level-metrics.png#lightbox)
    
4. Select **Add metric** at upper left.
    
5. Under **Metric**, select **Inbound Flows**. Leave **Aggregation** at **Avg**.
    
6. At upper right, select **Local Time: Last 24 hours (Automatic - 15 minutes)**, change it to **Last 30 minutes**, and select **Apply**.
    

Your graph should look similar to the following screenshot:

[![Screenshot that shows a graph of CPU usage and inbound traffic.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/3-metric-graph.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/3-metric-graph.png#lightbox)
# Collect client performance counters by using VM insights

Besides monitoring your VM host's health, utilization, and performance, you need to monitor the software and processes running on your VM. These are called the VM guest or client. In this unit, you enable the Azure Monitor VM insights feature, which offers a quick way to start monitoring the VM client.

The VM client includes the operating system and other workloads and applications. To monitor the software running on your VM, you install the Azure Monitor Agent, which collects data from inside the VM. VM insights:

- Installs Azure Monitor Agent on your VM.
- Creates a DCR that collects and sends a predefined set of client performance data to a Log Analytics workspace.
- Presents the data in curated workbooks.

Although you don't need to use VM insights to install Azure Monitor Agent, create DCRs, or set up workbooks, VM insights makes setting up VM client monitoring easy. VM insights provides you with a basis for monitoring the performance of your VM client and mapping the processes running on your machine.

## Enable VM insights

1. In the Azure portal, on your VM's **Overview** page, select **Insights** from the left navigation menu under **Monitoring**.
    
2. On the **Insights** page, select **Enable**.
    
3. Under **Data collection rule**, note the properties of the DCR that VM insights creates. In the DCR description, **Processes and dependencies (Map)** is set to **Disabled**, which you can change to **Enabled** or review [this article on how-to](https://learn.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-maps) if greyed out. Also a default **Log Analytics workspace** is created or assigned.
    
4. Select **Configure**.
    
    [![Screenshot that shows enabling and configuring VM insights.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/enable-insights.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/enable-insights.png#lightbox)
    
    Configuration of the workspace and the agent installation typically takes 5 to 10 minutes. It can take another 5 to 10 minutes for data to become available to view in the portal.
    
5. When the deployment finishes, confirm that the Azure Monitor Agent is installed by looking on the **Properties** tab of the VM's **Overview** page under **Extensions + applications**.
    
    On the **Monitoring** tab of the **Overview** page, under **Performance and utilization**, you can see that **Guest OS metrics** are now being collected.
    
    [![Screenshot that shows Guest OS metrics on the VM's Monitoring tab.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/guest-os-metrics.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/guest-os-metrics.png#lightbox)
    

## View VM insights

VM insights creates a DCR that sends client VM performance counters to Azure Monitor Logs. Because the DCR sends its metrics to Azure Monitor Logs, you don't use Metrics Explorer to view the metrics data that VM insights collects.

To view the VM insights performance graphs and maps:

1. Select **Insights** from the VM's left navigation menu under **Monitoring**.
    
2. Near the top of the **Insights** page, select the **Performance** tab. The prebuilt VM insights Performance workbook shows charts and graphs with performance-related data for the current VM.
    
    [![Screenshot that shows the prebuilt VM insights Performance workbook.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/vm-insights-performance.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/vm-insights-performance.png#lightbox)
    
    - You can customize the view by specifying a different **Time range** at the top of the page and different aggregations at the top of each graph.
        
    - Select **View Workbooks** to select from other available prebuilt VM insights workbooks. Select **Go To Gallery** to select from a gallery of other VM insights workbooks and workbook templates, or to edit and create your own workbooks.
        
3. If enabled earlier, select the **Map** tab on the **Insights** page to see the workbook for the Map feature. The map visualizes the VM's dependencies by discovering running process groups and processes that have active network connections over a specified time range.
    
    [![Screenshot that shows a dependency map on the Map tab of VM insights.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/dependency-map.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/dependency-map.png#lightbox)

# Collect VM client event logs

Azure Monitor Metrics and VM insights performance counters help you identify performance anomalies, and alert when thresholds are reached. But to analyze the root causes of issues you detect, you need to analyze log data to see which system events caused or contributed to the issues. In this unit, you set up a DCR to collect Linux VM Syslog data, and view the log data in Azure Monitor Log Analytics by using a simple Kusto Query Language (KQL) query.

VM insights installs the Azure Monitor Agent, and creates a DCR that collects predefined performance counters, maps process dependencies, and presents the data in prebuilt workbooks. You can create your own DCRs to collect VM performance counters that the VM insights DCR doesn't collect, or to collect log data.

When you create DCRs in the Azure portal, you can select from a range of performance counters and sampling rates or add custom performance counters. Alternatively, you can select from a predefined set of log types and severity levels or define custom log schemas. You can associate a single DCR to any or all VMs in your subscription, but you might need multiple DCRs to collect different types of data from different VMs.

## Create a DCR to collect log data

In the Azure portal, search for and select _monitor_ to go to the Azure Monitor **Overview** page.

[![Screenshot that shows the Azure Monitor Overview page.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/monitor-overview.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/monitor-overview.png#lightbox)

### Create a Data Collection Endpoint

You must have a data collection endpoint to send log data to. To create an endpoint:

1. In the Azure Monitor left navigation menu under **Settings**, select **Data Collection Endpoints**.
2. On the **Data Collection Endpoints** page, select **Create**.
3. On the **Create data collection endpoint** page, for **Name**, enter _linux-logs-endpoint_.
4. Select the same **Subscription**, **Resource group**, and **Region** as your VM uses.
5. Select **Review + create**, and when validation passes, select **Create**.

### Create the Data Collection Rule

To create the DCR to collect the event logs:

1. In the Monitor left navigation menu under **Settings**, select **Data Collection Rules**.
    
2. On the **Data Collection Rules** page, you can see the DCR that VM insights created. Select **Create** to create a new data collection rule.
    
    [![Screenshot of the Data Collection Rules screen with Create highlighted.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/dcr-create.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/dcr-create.png#lightbox)
    
3. On the **Basics** tab of the **Create Data Collection Rule** screen, provide the following information:
    
    - **Rule name**: Enter _collect-events-linux_.
    - **Subscription**, **Resource Group**, and **Region**: Select the same as for your VM.
    - **Platform Type**: Select **Linux**.
4. Select **Next: Resources** or the **Resources** tab.
    
    [![Screenshot of the Basics tab of the Create Data Collection Rule screen.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/create-dcr-basics.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/create-dcr-basics.png#lightbox)
    
5. On the **Resources** screen, select **Add resources**.
    
6. On the **Select a scope** screen, select the **monitored-linux-vm** VM, and then select **Apply**.
    
7. On the **Resources** screen, select **Enable Data Collection Endpoints**.
    
8. Under **Data collection endpoint** for the **monitored-linux-vm**, select the **linux-logs-endpoint** you created.
    
9. Select **Next: Collect and deliver**, or the **Collect and deliver** tab.
    
    [![Screenshot of the Resources tab of the Create Data Collection Rule screen.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/create-dcr-resources.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/create-dcr-resources.png#lightbox)
    
10. On the **Collect and deliver** tab, select **Add data source**.
    
11. On the **Add data source** screen, under **Data source type**, select **Linux Syslog**.
    
12. On the **Add data source** screen, select **Next: Destination** or the **Destination** tab, and make sure the **Account or namespace** matches the Log Analytics workspace that you want to use. You can use the default Log Analytics workspace that VM insights set up, or create or use another Log Analytics workspace.
    
13. On the **Add data source** screen, select **Add data source**.
    
14. On the **Create Data Collection Rule** screen, select **Review + create**, and when validation passes, select **Create**.
    
    [![Screenshot of Review + create highlighted on the Create Data Collection Rule screen.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/create-dcr-finish.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/create-dcr-finish.png#lightbox)
    

## View log data

You can view and analyze the log data collected by your DCR by using KQL log queries. A set of sample KQL queries is available for VMs, but you can write a query to look at the events your DCR is collecting.

1. On your VM's **Overview** page, select **Logs** from the left navigation menu under **Monitoring**. Log Analytics opens an empty query window with the scope set to your VM.
    
    You can also access log data by selecting **Logs** from the left navigation of the Azure Monitor **Overview** page. If necessary, select **Select scope** at the top of the query window to scope the query to the desired Log Analytics workspace and VM.
    
     Note
    
    The **Queries** window with sample queries might open when you open Log Analytics. For now, close this window, because you're going to manually create a simple query.
    
2. In the empty query window, type _Syslog_, and then select **Run**. All the system log events the DCR collected within the **Time range** are displayed.
    
3. You can refine your query to identify events of interest. For example, you can display only the events that had a **SeverityLevel** of **warning**.
    
    [![Screenshot that shows the events returned from the Syslog by the DCR.](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/dcr-log.png)](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/media/dcr-log.png#lightbox)

# Summary

Azure Monitor helps you collect, analyze, and alert on various types of host and client monitoring data from your Azure VMs.

- Azure Monitor provides a set of VM host logs and performance and usage metrics for all Azure VMs.
- You can enable recommended alert rules when you create VMs or afterwards to alert on important VM host metrics.
- Azure Monitor Metrics Explorer lets you graph and analyze metrics for Azure VMs and other resources.
- VM insights provides a simple way to monitor important VM client performance counters and processes running on your VM.
- You can create data collection rules to collect other metrics and logs from your VM client.
- You can use Log Analytics to query and analyze log data.

Now that you understand these tools, you're confident that Azure Monitor can effectively monitor your Azure VMs and help you keep your website running effectively.


---

