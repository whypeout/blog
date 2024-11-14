---
draft: true
date: 2024-11-14
slug: Az 104 - Managed Identities Governance
tags:
---

# Microsoft Entra ID

# Examine Microsoft Entra ID

Students should be familiar with Active Directory Domain Services (AD DS or traditionally called just "Active Directory"). AD DS is a directory service that provides the methods for storing directory data, such as user accounts and passwords, and makes this data available to network users, administrators, and other devices and services. It runs as a service on Windows Server, referred to as a domain controller.

Microsoft Entra ID is part of the platform as a service (PaaS) offering and operates as a Microsoft-managed directory service in the cloud. It’s not a part of the core infrastructure that customers own and manage, nor is it an Infrastructure as a service offering. While this implies that you have less control over its implementation, it also means that you don’t have to dedicate resources to its deployment or maintenance.

With Microsoft Entra ID, you also have access to a set of features that aren’t natively available in AD DS, such as support for multi-factor authentication, identity protection, and self-service password reset.

You can use Microsoft Entra ID to provide more secure access to cloud-based resources for organizations and individuals by:

- Configuring access to applications
- Configuring single sign-on (SSO) to cloud-based SaaS applications
- Managing users and groups
- Provisioning users
- Enabling federation between organizations
- Providing an identity management solution
- Identifying irregular sign-in activity
- Configuring multi-factor authentication
- Extending existing on-premises Active Directory implementations to Microsoft Entra ID
- Configuring Application Proxy for cloud and local applications
- Configuring Conditional Access for users and devices

![Diagram that shows the Microsoft Entra Connect Stack.](https://learn.microsoft.com/en-us/training/wwl-azure/understand-azure-active-directory/media/azure-active-directory-connect-stack-f1aae359.png)

Microsoft Entra constitutes a separate Azure service. Its most elementary form, which any new Azure subscription includes automatically, doesn't incur any extra cost and is referred to as the Free tier. If you subscribe to any Microsoft Online business services (for example, Microsoft 365 or Microsoft Intune), you automatically get Microsoft Entra ID with access to all the Free features.

 Note

By default, when you create a new Azure subscription by using a Microsoft account, the subscription automatically includes a new Microsoft Entra tenant named Default Directory.

Some of the more advanced identity management features require paid versions of Microsoft Entra ID, offered in the form of Basic and Premium tiers. Some of these features are also automatically included in Microsoft Entra instances generated as part of Microsoft 365 subscriptions. Differences between Microsoft Entra versions are discussed later in this module.

Implementing Microsoft Entra ID isn't the same as deploying virtual machines in Azure, adding AD DS, and then deploying some domain controllers for a new forest and domain. Microsoft Entra ID is a different service, much more focused on providing identity management services to web-based apps, unlike AD DS, which is more focused on on-premises apps.

### Microsoft Entra tenants

Unlike AD DS, Microsoft Entra ID is multi-tenant by design and is implemented specifically to ensure isolation between its individual directory instances. It’s the world’s largest multi-tenant directory, hosting over a million directory services instances, with billions of authentication requests per week. The term tenant in this context typically represents a company or organization that signed up for a subscription to a Microsoft cloud-based service such as Microsoft 365, Intune, or Azure, each of which uses Microsoft Entra ID. However, from a technical standpoint, the term tenant represents an individual Microsoft Entra instance. Within an Azure subscription, you can create multiple Microsoft Entra tenants. Having multiple Microsoft Entra tenants might be convenient if you want to test Microsoft Entra functionality in one tenant without affecting the others.

At any given time, an Azure subscription must be associated with one, and only one, Microsoft Entra tenant. This association allows you to grant permissions to resources in the Azure subscription (via RBAC) to users, groups, and applications that exist in that particular Microsoft Entra tenant.

 Note

You can associate the same Microsoft Entra tenant with multiple Azure subscriptions. This allows you to use the same users, groups, and applications to manage resources across multiple Azure subscriptions.

Each Microsoft Entra tenant is assigned the default Domain Name System (DNS) domain name, consisting of a unique prefix. The prefix, derived from the name of the Microsoft account you use to create an Azure subscription or provided explicitly when creating a Microsoft Entra tenant, is followed by the **onmicrosoft.com** suffix. Adding at least one custom domain name to the same Microsoft Entra tenant is possible and common. This name utilizes the DNS domain namespace that the corresponding company or organization owns. The Microsoft Entra tenant serves as the security boundary and a container for Microsoft Entra objects such as users, groups, and applications. A single Microsoft Entra tenant can support multiple Azure subscriptions.

### Microsoft Entra schema

The Microsoft Entra schema contains fewer object types than that of AD DS. Most notably, it doesn't include a definition of the computer class, although it does include the device class. The process of joining devices to Microsoft Entra differs considerably from the process of joining computers to AD DS. The Microsoft Entra schema is also easily extensible, and its extensions are fully reversible.

The lack of support for the traditional computer domain membership means that you can't use Microsoft Entra ID to manage computers or user settings by using traditional management techniques, such as Group Policy Objects (GPOs). Instead, Microsoft Entra ID and its services define a concept of modern management. Microsoft Entra ID’s primary strength lies in providing directory services; storing and publishing user, device, and application data; and handling the authentication and authorization of the users, devices, and applications. The effectiveness and efficiency of these features are apparent based on existing deployments of cloud services such as Microsoft 365, which rely on Microsoft Entra ID as their identity provider and support millions of users.

Microsoft Entra ID doesn't include the organizational unit (OU) class, which means that you can't arrange its objects into a hierarchy of custom containers, which is frequently used in on-premises AD DS deployments. However, this isn't a significant shortcoming, because OUs in AD DS are used primarily for Group Policy scoping and delegation. You can accomplish equivalent arrangements by organizing objects based on their group membership.

Objects of the Application and servicePrincipal classes represent applications in Microsoft Entra ID. An object in the Application class contains an application definition and an object in the servicePrincipal class constitutes its instance in the current Microsoft Entra tenant. Separating these two sets of characteristics allows you to define an application in one tenant and use it across multiple tenants by creating a service principal object for this application in each tenant. Microsoft Entra ID creates the service principal object when you register the corresponding application in that Microsoft Entra tenant.

---

# Compare Microsoft Entra ID and Active Directory Domain Services

You could view Microsoft Entra ID simply as the cloud-based counterpart of AD DS; however, while Microsoft Entra ID and AD DS share some common characteristics, there are several significant differences between them.

### Characteristics of AD DS

AD DS is the traditional deployment of Windows Server-based Active Directory on a physical or virtual server. Although AD DS is commonly considered being primarily a directory service, it’s only one component of the Windows Active Directory suite of technologies, which also includes Active Directory Certificate Services (AD CS), Active Directory Lightweight Directory Services (AD LDS), Active Directory Federation Services (AD FS), and Active Directory Rights Management Services (AD RMS).

When comparing AD DS with Microsoft Entra ID, it’s important to note the following characteristics of AD DS:

- AD DS is a true directory service, with a hierarchical X.500-based structure.
- AD DS uses Domain Name System (DNS) for locating resources such as domain controllers.
- You can query and manage AD DS by using Lightweight Directory Access Protocol (LDAP) calls.
- AD DS primarily uses the Kerberos protocol for authentication.
- AD DS uses OUs and GPOs for management.
- AD DS includes computer objects, representing computers that join an Active Directory domain.
- AD DS uses trusts between domains for delegated management.

You can deploy AD DS on an Azure virtual machine to enable scalability and availability for an on-premises AD DS. However, deploying AD DS on an Azure virtual machine doesn't make any use of Microsoft Entra ID.

 Note

Deploying AD DS on an Azure virtual machine requires one or more extra Azure data disks because you shouldn't use drive C for AD DS storage. These disks are needed to store the AD DS database, logs, and the sysvol folder. The Host Cache Preference setting for these disks must be set to None.

### Characteristics of Microsoft Entra ID

Although Microsoft Entra ID has many similarities to AD DS, there are also many differences. It’s important to realize that using Microsoft Entra isn’t the same as deploying an Active Directory domain controller on an Azure virtual machine and adding it to your on-premises domain.

When comparing Microsoft Entra ID with AD DS, it’s important to note the following characteristics of Microsoft Entra ID:

- Microsoft Entra ID is primarily an identity solution, and it’s designed for internet-based applications by using HTTP (port 80) and HTTPS (port 443) communications.
- Microsoft Entra ID is a multi-tenant directory service.
- Microsoft Entra users and groups are created in a flat structure, and there are no OUs or GPOs.
- You can't query Microsoft Entra ID by using LDAP; instead, Microsoft Entra ID uses the REST API over HTTP and HTTPS.
- Microsoft Entra ID doesn't use Kerberos authentication; instead, it uses HTTP and HTTPS protocols such as SAML, WS-Federation, and OpenID Connect for authentication, and uses OAuth for authorization.
- Microsoft Entra ID includes federation services, and many third-party services such as Facebook are federated with and trust Microsoft Entra ID.

---

# Examine Microsoft Entra ID as a directory service for cloud apps

When you deploy cloud services such as Microsoft 365 or Intune, you also need to have directory services in the cloud to provide authentication and authorization for these services. Because of this, each cloud service that needs authentication will create its own Microsoft Entra tenant. When a single organization uses more than one cloud service, it’s much more convenient for these cloud services to use a single cloud directory instead of having separate directories for each service.

It’s now possible to have one identity service that covers all Microsoft cloud-based services, such as Microsoft 365, Azure, Microsoft Dynamics 365, and Intune. Microsoft Entra ID provides developers with centralized authentication and authorization for applications in Azure by using other identity providers or on-premises AD DS. Microsoft Entra ID can provide users with an SSO experience when using applications such as Facebook, Google services, Yahoo, or Microsoft cloud services.

The process of implementing Microsoft Entra ID support for custom applications is rather complex and beyond the scope of this course. However, the Azure portal and Microsoft Visual Studio 2013 and later make the process of configuring such support more straightforward.

In particular, you can enable Microsoft Entra authentication for the Web Apps feature of Azure App Service directly from the Authentication/Authorization blade in the Azure portal. By designating the Microsoft Entra tenant, you can ensure that only users with accounts in that directory can access the website. It’s possible to apply different authentication settings to individual deployment slots.

For more information, see [Configure your App Service app to use Microsoft Entra login](https://learn.microsoft.com/en-us/azure/app-service/configure-authentication-provider-aad).

---

# Compare Microsoft Entra ID P1 and P2 plans

The Microsoft Entra ID P1 or P2 tier provides extra functionality as compared to the Free and Office 365 editions. However, premium versions require additional cost per user provisioning. Microsoft Entra ID P1 or P2 comes in two versions P1 and P2. You can procure it as an extra license or as a part of the Microsoft Enterprise Mobility + Security, which also includes the license for Azure Information Protection and Intune.

Microsoft provides a free trial period that can be used to experience the full functionality of the Microsoft Entra ID P2 edition. The following features are available with the Microsoft Entra ID P1 edition:

- **Self-service group management**. It simplifies the administration of groups where users are given the rights to create and manage the groups. End users can create requests to join other groups, and group owners can approve requests and maintain their groups’ memberships.
- **Advanced security reports and alerts**. You can monitor and protect access to your cloud applications by viewing detailed logs that show advanced anomalies and inconsistent access pattern reports. Advanced reports are machine learning based and can help you gain new insights to improve access security and respond to potential threats.
- **Multi-factor authentication**. Full multi-factor authentication (MFA) works with on-premises applications (using virtual private network [VPN], RADIUS, and others), Azure, Microsoft 365, Dynamics 365, and third-party Microsoft Entra gallery applications. It doesn't work with non-browser off-the-shelf apps, such as Microsoft Outlook. Full multi-factor authentication is covered in more detail in the following units in this lesson.
- **Microsoft Identity Manager (MIM) licensing**. MIM integrates with Microsoft Entra ID P1 or P2 to provide hybrid identity solutions. MIM can bridge multiple on-premises authentication stores such as AD DS, LDAP, Oracle, and other applications with Microsoft Entra ID. This provides consistent experiences to on-premises line-of-business (LOB) applications and SaaS solutions.
- **Enterprise SLA of 99.9%**. You're guaranteed at least 99.9% availability of the Microsoft Entra ID P1 or P2 service. The same SLA applies to Microsoft Entra Basic.
- **Password reset with writeback**. Self-service password reset follows the Active Directory on-premises password policy.
- **Cloud App Discovery feature of Microsoft Entra ID**. This feature discovers the most frequently used cloud-based applications.
- **Conditional Access based on device, group, or location**. This lets you configure conditional access for critical resources, based on several criteria.
- **Microsoft Entra Connect Health**. You can use this tool to gain operational insight into Microsoft Entra ID. It works with alerts, performance counters, usage patterns, and configuration settings, and presents the collected information in the Microsoft Entra Connect Health portal.

In addition to these features, the Microsoft Entra ID P2 license provides extra functionalities:

- **Microsoft Entra ID Protection**. This feature provides enhanced functionalities for monitoring and protecting user accounts. You can define user risk policies and sign-in policies. In addition, you can review users’ behavior and flag users for risk.
- **Microsoft Entra Privileged Identity Management**. This functionality lets you configure additional security levels for privileged users such as administrators. With Privileged Identity Management, you define permanent and temporary administrators. You also define a policy workflow that activates whenever someone wants to use administrative privileges to perform some task.

 Note

Plans change frequently. Check Microsoft's website for the current plans and capabilities.

---

# Examine Microsoft Entra Domain Services


In most organizations today, line-of-business (LOB) applications are deployed on computers and devices that are domain members. These organizations use AD DS–based credentials for authentication, and Group Policy manages them. When you consider moving these apps to run in Azure, one key issue is how to provide authentication services to these apps. To satisfy this need, you can choose to implement a site-to-site virtual private network (VPN) between your local infrastructure and the Azure IaaS, or you can deploy replica domain controllers from your local AD DS as virtual machines (VMs) in Azure. These approaches can entail additional costs and administrative effort. Additionally, the difference between these two approaches is that with the first option, authentication traffic will cross the VPN, while in the second option, replication traffic will cross the VPN and authentication traffic stays in the cloud.

Microsoft provides Microsoft Entra Domain Services as an alternative to these approaches. This service, which runs as part of the Microsoft Entra ID P1 or P2 tier, provides domain services such as Group Policy management, domain joining, and Kerberos authentication to your Microsoft Entra tenant. These services are fully compatible with locally deployed AD DS, so you can use them without deploying and managing additional domain controllers in the cloud.

![Diagram that shows the Microsoft Entra Domain Services Overview.](https://learn.microsoft.com/en-us/training/wwl-azure/understand-azure-active-directory/media/azure-active-directory-virtual-network-340081c4.png)

Because Microsoft Entra ID can integrate with your local AD DS, when you implement Microsoft Entra Connect, users can utilize organizational credentials in both on-premises AD DS and in Microsoft Entra Domain Services. Even if you don’t have AD DS deployed locally, you can choose to use Microsoft Entra Domain Services as a cloud-only service. This enables you to have similar functionality of locally deployed AD DS without having to deploy a single domain controller on-premises or in the cloud. For example, an organization can choose to create a Microsoft Entra tenant and enable Microsoft Entra Domain Services, and then deploy a virtual network between its on-premises resources and the Microsoft Entra tenant. You can enable Microsoft Entra Domain Services for this virtual network so that all on-premises users and services can use domain services from Microsoft Entra ID.

Microsoft Entra Domain Services provides several benefits for organizations, such as:

- Administrators don't need to manage, update, and monitor domain controllers.
- Administrators don't need to deploy and manage Active Directory replication.
- There’s no need to have Domain Admins or Enterprise Admins groups for domains that Microsoft Entra ID manages.

If you choose to implement Microsoft Entra Domain Services, you need to be aware of the service's current limitations. These include:

- Only the base computer Active Directory object is supported.
- It’s not possible to extend the schema for the Microsoft Entra Domain Services domain.
- The organizational unit (OU) structure is flat and nested OUs aren't currently supported.
- There’s a built-in Group Policy Object (GPO), and it exists for computer and user accounts.
- It’s not possible to target OUs with built-in GPOs. Additionally, you can't use Windows Management Instrumentation filters or security-group filtering.

By using Microsoft Entra Domain Services, you can freely migrate applications that use LDAP, NTLM, or the Kerberos protocols from your on-premises infrastructure to the cloud. You can also use applications such as Microsoft SQL Server or Microsoft SharePoint Server on VMs or deploy them in the Azure IaaS, without needing domain controllers in the cloud or a VPN to local infrastructure.

You can enable Microsoft Entra Domain Services by using the Azure portal. This service charges per hour based on the size of your directory.

---

---
# Configure User and Group accounts

# Introduction

Access to Azure resources is controlled through user accounts and identities that are defined in Microsoft Entra ID. Microsoft Entra ID supports group accounts to help you organize user accounts for easier administration.

In this module, your company wants to take advantage of the user and group account features in Microsoft Entra ID. You need to understand the concepts of user accounts and group accounts. You're looking for information about how to create, configure, and manage these accounts. Your organization needs support for bulk configuration of settings, group account organization, and managing accounts across multiple directories.

In this module, you learn about user accounts and group accounts. You learn about an administrator shortcut for bulk creation of user accounts. You learn about controlling administrator access through administrative units. You also simulate user and group creation in the Azure portal.

The goal of this module is to successfully create and manage user and group accounts.

# Create user accounts

Every user who wants access to Azure resources needs an Azure user account. A user account has all the information required to authenticate the user during the sign-in process. Microsoft Entra ID supports three types of user accounts. The types indicate where the user is defined (in the cloud or on-premises), and whether the user is internal or external to your Microsoft Entra organization.

### Things to know about user accounts

The following table describes the user accounts supported in Microsoft Entra ID. As you review these options, consider what types of user accounts suit your organization.

Expand table

|User account|Description|
|---|---|
|**Cloud identity**|A user account with a _cloud identity_ is defined only in Microsoft Entra ID. This type of user account includes administrator accounts and users who are managed as part of your organization. A cloud identity can be for user accounts defined in your Microsoft Entra organization, and also for user accounts defined in an external Microsoft Entra instance. When a cloud identity is removed from the primary directory, the user account is deleted.|
|**Directory-synchronized identity**|User accounts that have a _directory-synchronized identity_ are defined in an on-premises Active Directory. A synchronization activity occurs via Microsoft Entra Connect to bring these user accounts in to Azure. The source for these accounts is Windows Server Active Directory.|
|**Guest user**|_Guest user_ accounts are defined outside Azure. Examples include user accounts from other cloud providers, and Microsoft accounts like an Xbox LIVE account. The source for guest user accounts is Invited user. Guest user accounts are useful when external vendors or contractors need access to your Azure resources.|

### Things to consider when choosing user accounts

- **Consider where users are defined**. Determine where your users are defined. Are all your users defined within your Microsoft Entra organization, or are some users defined in external Microsoft Entra instances? Do you have users who are external to your organization? It's common for businesses to support two or more account types in their infrastructure.
    
- **Consider support for external contributors**. Allow external contributors to access Azure resources in your organization by supporting the **Guest user** account type. When the external contributor no longer requires access, you can remove the user account and their access privileges.
    
- **Consider a combination of user accounts**. Implement the user account types that enable your organization to satisfy their business requirements. Support directory-synchronized identity user accounts for users defined in Windows Server Active Directory. Support cloud identities for users defined in your internal Microsoft Entra structure or for user defined in an external Microsoft Entra instance.

# Manage user accounts

There are several ways to add cloud identity user accounts in Microsoft Entra ID. A common approach is by using the Azure portal. User accounts can also be added to Microsoft Entra ID through Microsoft 365 Admin Center, Microsoft Intune admin console, and the Azure CLI.

### Things to know about cloud identity accounts

Let's review how cloud identity user accounts are defined in Microsoft Entra ID. Here's an example of the new **User** page in the Azure portal. The administrator can **Create** a user within the organization or **Invite** a guest user to provide access to organization resources:

![Screenshot of the User page in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-user-group-accounts/media/add-user-accounts-133b7dbf.png)

- A new user account must have a display name and an associated user account name. An example display name is `Aran Sawyer-Miller` and the associated user account name could be `asawmill@contoso.com`.
    
- Information and settings that describe a user are stored in the user account profile.
    
- The profile can have other settings like a user's job title, and their contact email address.
    
- A user with Global administrator or User administrator privileges can preset profile data in user accounts, such as the main phone number for the company.
    
- Non-admin users can set some of their own profile data, but they can't change their display name or account name.
    

### Things to consider when managing cloud identity accounts

There are several points to consider about managing user accounts. As you review this list, consider how you can add cloud identity user accounts for your organization.

- **Consider user profile data**. Allow users to set their profile information for their accounts, as needed. User profile data, including the user's picture, job, and contact information is optional. You can also supply certain profile settings for each user based on your organization's requirements.
    
- **Consider restore options for deleted accounts**. Include restore scenarios in your account management plan. Restore operations for a deleted account are available up to 30 days after an account is removed. After 30 days, a deleted user account can't be restored.
    
- **Consider gathered account data**. Collect sign-in and audit log information for user accounts. Microsoft Entra ID lets you gather this data to help you analyze and improve your infrastructure.

# Create bulk user accounts

Microsoft Entra ID supports several bulk operations, including bulk create and delete for user accounts. The most common approach for these operations is to use the Azure portal. Azure PowerShell can be used for bulk upload of user accounts.

### Things to know about bulk account operations

Let's examine some characteristics of bulk operations in the Azure portal. Here's an example that shows the **Bulk create user** option for new user accounts in Microsoft Entra ID:

![Screenshot that shows the Bulk create user option for new user accounts in Azure AD.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-user-group-accounts/media/bulk-user-accounts-224ad1b7.png)

- Only Global administrators or User administrators have privileges to create and delete user accounts in the Azure portal.
    
- To complete bulk create or delete operations, the admin fills out a comma-separated values (CSV) template of the data for the user accounts.
    
- Bulk operation templates can be downloaded from the Microsoft Entra admin center.
    
- Bulk lists of user accounts can be downloaded.
    

### Things to consider when creating user accounts

Here are some design considerations for creating and deleting user accounts. Think about what user account conventions and processes might be required by your organization.

- **Consider naming conventions**. Establish or implement a naming convention for your user accounts. Apply conventions to user account names, display names, and user aliases for consistency across the organization. Conventions for names and aliases can simplify the bulk create process by reducing areas of uniqueness in the CSV file. A convention for user names could begin with the user's last name followed by a period, and end with the user's first name, as in `Sawyer-Miller.Aran@contoso.com`.
    
- **Consider using initial passwords**. Implement a convention for the initial password of a newly created user. Design a system to notify new users about their passwords in a secure way. You might generate a random password and email it to the new user or their manager.
    
- **Consider strategies for minimizing errors**. View and address any errors, by downloading the results file on the **Bulk operation results** page in the Azure portal. The results file contains the reason for each error. An error might be a user account that's already been created or an account that's duplicated. Generally, it's easier to upload and troubleshoot smaller groups of user accounts.

# Create group accounts

Microsoft Entra ID allows your organization to define two different types of group accounts. **Security groups** are used to manage member and computer access to shared resources for a group of users. You can create a security group for a specific security policy and apply the same permissions to all members of a group. **Microsoft 365 groups** provide collaboration opportunities. Group members have access to a shared mailbox, calendar, files, SharePoint site, and more.

### Things to know about creating group accounts

Review the following characteristics of group accounts in Microsoft Entra ID. The following screenshot shows a list of groups in the Azure portal:

![Screenshot that shows a list of groups in the Azure portal, and their group and membership types.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-user-group-accounts/media/group-accounts-378b3e22.png)

- Use security groups to set permissions for all group members at the same time, rather than adding permissions to each member individually.
    
- Add Microsoft 365 groups to enable group access for guest users outside your Microsoft Entra organization.
    
- Security groups can be implemented only by a Microsoft Entra administrator.
    
- Normal users and Microsoft Entra admins can both use Microsoft 365 groups.
    

### Things to consider when adding group members

When you add members to a group, there are different ways you can assign member access rights. As you read through these options, consider which groups are needed to support your organization, and what access rights should be applied to group members.

Expand table

|Access rights|Description|
|---|---|
|**Assigned**|Add specific users as members of a group, where each user can have unique permissions.|
|**Dynamic user**|Use dynamic membership rules to automatically add and remove group members. When member attributes change, Azure reviews the dynamic group rules for the directory. If the member attributes meet the rule requirements, the member is added to the group. If the member attributes no longer meet the rule requirements, the member is removed.|
|**Dynamic device**|(_Security groups only_) Apply dynamic group rules to automatically add and remove devices in security groups. When device attributes change, Azure reviews the dynamic group rules for the directory. If the device attributes meet the rule requirements, the device is added to the security group. If the device attributes no longer meet the rule requirements, the device is removed.|

# Create administrative units

As you design your strategy for managing identities and governance in Azure, planning for comprehensive management of your Microsoft Entra infrastructure is critical. It can be useful to restrict administrative scope by using administrative units for your organization. The division of roles and responsibilities is especially helpful for organizations that have many independent divisions.

Consider the management tasks for a large university that's composed of several different schools like Business, Engineering, and Medicine. The university has administrative offices, academic buildings, social buildings, and student dormitories. For security purposes, each business office has its own internal network for resources like servers, printers, and fax machines. Each academic building is connected to the university network, so both instructors and students can access their accounts. The network is also available to students and deans in the dormitories and social buildings. Across the university, guest users require access to the internet via the university network.

The university has a team of IT admins who work together to control resource access, manage users, and set policies for the school. Some admins have greater privileges than others depending on the scope of their responsibilities. A central authority is needed to plan, manage, and oversee the complete structure. In this scenario, you can assign administrative units to make it easier to manage the organization.

![Diagram of administrative units for each university department.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-user-group-accounts/media/admin-unit-overview.png)

### Things to think about administrative units

Consider how a central admin role can use administrative units to support the Engineering department in our scenario:

- Create a role that has administrative permissions for only Microsoft Entra users in the Engineering department administrative unit.
    
- Create an administrative unit for the Engineering department.
    
- Populate the administrative unit with only the Engineering department students, staff, and resources.
    
- Add the Engineering department IT team to the role, along with its scope.
    

### Things to consider when working with administrative units

Think about how you can implement administrative units in your organization. Here are some considerations:

- **Consider management tools**. Review your options for managing AUs. You can use the Azure portal, PowerShell cmdlets and scripts, or Microsoft Graph.
    
- **Consider role requirements in the Azure portal**. Plan your strategy for administrative units according to role privileges. In the Azure portal, only the Global Administrator or Privileged Role Administrator users can manage AUs.
    
- **Consider scope of administrative units**. Recognize that the scope of an administrative unit applies only to _management_ permissions. Members and admins of an administrative unit can exercise their default _user_ permissions to browse other users, groups, or resources outside of their administrative unit.

# Summary and resources

Azure Administrators must be familiar with configuring user and group accounts in Microsoft Entra ID.

In this module, you learned that every user who wants access to Azure resources needs an Azure user account. Microsoft Entra ID supports access to your organization's resources by assigning access rights to users and groups. You discovered how user and group accounts are created in Microsoft Entra ID. You explored how to configure and manage user and group accounts, including bulk configuration. You reviewed how your organization can support group account organization, and manage accounts across multiple directories.

The main takeaways for this module are:

- Microsoft Entra ID supports three types of user accounts: cloud identities, directory-synchronized identities, and guest user identities.
    
- Cloud identities have profile information such as job title and office location. This information can be customized for your organization's needs.
    
- You can bulk create user and group accounts. The process uses a template file managed through the portal.
    
- There are two types of group accounts: Security and Microsoft 365.
    
- Administrative units help you control administrator access to resources.
    

## Learn more with Azure documentation

- [Enterprise user management documentation](https://learn.microsoft.com/en-us/entra/identity/users/). This collection of articles that covers various topics related to user authentication in Microsoft Entra. You learn how to use groups, domain names, and licenses to manage user access to apps and resources3.
    
- [Manage Microsoft Entra groups and group membership](https://learn.microsoft.com/en-us/entra/fundamentals/how-to-manage-groups). This article explains how to create, edit, and delete groups in Microsoft Entra. You also learn how to add or remove members and owners, assign roles, and use dynamic rules for group membership2.
    

## Learn more with self-paced training

- [Manage users and groups in Microsoft Entra ID](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/). This training module covers the basic principles of user and groups.

---

# Configure Subscriptions

# Introduction

This module provides an overview of Azure subscriptions and their importance in managing costs for organizations.

You work for a multinational company that has recently decided to migrate its infrastructure to the cloud. As part of this migration, you have been tasked with managing the costs associated with the company's Azure resources. You need to understand how Azure subscriptions work and how they can help you effectively manage and optimize costs.

The goal of this module is to equip you with the knowledge and skills to successfully manage Azure subscriptions and control costs for your organization. You learn how Azure subscriptions work and use cost management tools and techniques. You optimize resource usage, prevent overspending, and make informed decisions to achieve cost savings.

## Learning objectives

In this module, you learn how to:

- Determine the correct region to locate Azure services.
    
- Review features and use cases for Azure subscriptions.
    
- Obtain an Azure subscription.
    
- Understand billing and features for different Azure subscriptions.
    
- Use Microsoft Cost Management and Billing for cost analysis.
    
- Discover when to use Azure resource tagging.
    
- Identify ways to reduce costs.
    

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/credentials/certifications/exams/az-104/).

## Prerequisites

- Familiarity with cloud computing and Azure cloud services.
- Familiarity with cloud service billing models and subscription methods.
- Familiarity with cost management techniques such as reports and budgets.

# Identify Azure regions


Microsoft Azure is made up of datacenters located around the globe. These datacenters are organized and made available to end users by region. A [region](https://azure.microsoft.com/global-infrastructure/regions/) is a geographical area on the planet containing at least one, but potentially multiple datacenters. The datacenters are in close proximity and networked together with a low-latency network. A few examples of regions are West US, Canada Central, West Europe, Australia East, and Japan West.

### Things to know about regions

Here are some points to consider about regions:

- Azure is generally available in more than 60 regions in 140 countries.
    
- Azure has more global regions than any other cloud provider.
    
- Regions provide you with the flexibility and scale needed to bring applications closer to your users.
    
- Regions preserve data residency and offer comprehensive compliance and resiliency options for customers.
    

### Things to know about regional pairs

Most Azure regions are paired with another region within the same geography to make a _regional pair_ (or _paired regions_). Regional pairs help to support always-on availability of Azure resources used by your infrastructure. The following table describes some prominent characteristics of paired regions:

Expand table

|Characteristic|Description|
|---|---|
|**Physical isolation**|Azure prefers at least 300 miles of separation between datacenters in a regional pair. This principle isn't practical or possible in all geographies. Physical datacenter separation reduces the likelihood of natural disasters, civil unrest, power outages, or physical network outages affecting both regions at once.|
|**Platform-provided replication**|Some services like Geo-Redundant Storage provide automatic replication to the paired region.|
|**Region recovery order**|During a broad outage, recovery of one region is prioritized out of every pair. Applications that are deployed across paired regions are guaranteed to have one of the regions recovered with priority.|
|**Sequential updates**|Planned Azure system updates are rolled out to paired regions sequentially (not at the same time). Rolling updates minimizes downtime, reduces bugs, and logical failures in the rare event of a bad update.|
|**Data residency**|Regions reside within the same geography as their enabled set (except for the Brazil South and Singapore regions).|

### Things to consider when using regions and regional pairs

You've reviewed the important considerations about regions and regional pairs. Now think about how you might implement regions in your organization.

- **Consider resource and region deployment**. Plan the regions where you want to deploy your resources. For most Azure services, when you deploy a resource in Azure, you choose the region where you want your resource to be deployed.
    
- **Consider service support by region**. Research region and service availability. Some services or Azure Virtual Machines features are available only in certain regions, such as specific Virtual Machines sizes or storage types.
    
- **Consider services that don't require regions**. Identify services that don't need region support. Some global Azure services that don't require you to select a region. These services include Microsoft Entra ID, Microsoft Azure Traffic Manager, and Azure DNS.
    
- **Consider exceptions to region pairing**. Check the Azure website for current region availability and exceptions. If you plan to support the Brazil South region, note this region is paired with a region outside its geography. The Singapore region also has an exception to standard regional pairing.
    
- **Consider benefits of data residency**. Take advantage of the benefits of data residency offered by regional pairs. This feature can help you meet requirements for tax and law enforcement jurisdiction purposes.
    

## Find regions for your business geography

Visit the Azure global infrastructure website to find supported regions for your business geography. You can search by country/region name or by Microsoft product. A list of supported region pairs and exceptions is also available.

![Screenshot of the Azure global infrastructure and Azure geographies website.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/azure-geographies.png)

Expand table

|By geography|By product|Paired regions|
|---|---|---|
|Search [Azure regions](https://azure.microsoft.com/global-infrastructure/geographies/#geographies) by geography.|Search [Azure products](https://azure.microsoft.com/global-infrastructure/services/) by region or geography.|Search for [paired regions](https://learn.microsoft.com/en-us/azure/best-practices-availability-paired-regions#azure-cross-region-replication-pairings-for-all-geographies) and exceptions.|
|[![Screenshot that shows how to search for available regions by geographic location.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/regions-select-by-geography.png)](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/regions-select-by-geography-expanded.png#lightbox)|[![Screenshot that shows how to find products available according to region or geographic location.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/regions-select-by-product.png)](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/regions-select-by-product-expanded.png#lightbox)|[![Screenshot that shows how to search for regional pairs.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/search-region-pairs.png)](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/search-region-pairs-expanded.png#lightbox)|

# Implement Azure subscriptions


An Azure subscription is a logical unit of Azure services that's linked to an Azure account. An Azure account is an identity in Microsoft Entra ID or a directory that's trusted by Microsoft Entra ID, such as a work or school account. Subscriptions help you organize access to Azure cloud service resources, and help you control how resource usage is reported, billed, and paid.

![Diagram that shows the relationship between an Azure subscription and an Azure account, which is an identity in Microsoft Entra ID.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/azure-subscriptions-e855533e.png)

### Things to know about subscriptions

As you think about the subscriptions to implement for your company, consider the following points:

- Every Azure cloud service belongs to a subscription.
    
- Each subscription can have a different billing and payment configuration.
    
- Multiple subscriptions can be linked to the same Azure account.
    
- More than one Azure account can be linked to the same subscription.
    
- Billing for Azure services is done on a per-subscription basis.
    
- If your Azure account is the only account associated with a subscription, you're responsible for the billing requirements.
    
- Programmatic operations for a cloud service might require a subscription ID.
    

### Things to consider when using subscriptions

Consider how many subscriptions your organization needs to support the business scenarios. As you plan, think about how you can organize your resources into resource groups.

- **Consider the types of Azure accounts required**. Determine the types of Azure accounts your users will link with Azure subscriptions. You can use a Microsoft Entra account or a directory that's trusted by Microsoft Entra ID like a work or school account. If you don't belong to one of these organizations, you can sign up for an Azure account by using your Microsoft Account, which is also trusted by Microsoft Entra ID.
    
- **Consider multiple subscriptions**. Set up different subscriptions and payment options according to your company's departments, projects, regional offices, and so on. A user can have more than one subscription linked to their Azure account, where each subscription pertains to resources, access privileges, limits, and billing for a specific project.
    
- **Consider a dedicated shared services subscription**. Plan for how users can share resources allocated in a single subscription. Use a shared services subscription to ensure all common network resources are billed together and isolated from other workloads. Examples of shared services subscriptions include Azure ExpressRoute and Virtual WAN.
    
- **Consider access to resources**. Every Azure subscription can be associated with a Microsoft Entra ID. Users and services authenticate with Microsoft Entra ID before they access resources.

# Obtain an Azure subscription


To use Azure, you must have an Azure subscription. There are several ways to procure an Azure subscription. You can obtain an Azure subscription as part of an Enterprise agreement, or through a Microsoft reseller or Microsoft partner. Users can also open a personal free account for a trial subscription.

### Things to know about obtaining an Azure subscription

Review the following ways to obtain an Azure subscription and consider which options would work for your organization.

Expand table

|Procurement option|Description|
|---|---|
|![](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/enterprise-icon.png)|**Enterprise agreement**  <br>  <br>Any [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/) customer can add Azure to their agreement by making an upfront monetary commitment to Azure. The commitment is consumed throughout the year by using any combination of the wide variety of cloud services Azure offers.|
|![](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/resellers-icon.png)|**Microsoft reseller**  <br>  <br>Buy Azure through the [Open Licensing program](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx), which provides a simple, flexible way to purchase cloud services from your Microsoft reseller. If you already purchased an Azure in Open license key, [activate a new subscription or add more credits now](https://azure.microsoft.com/offers/ms-azr-0111p/).|
|![](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/partners-icon.png)|**Microsoft partner**  <br>  <br>Find a [Microsoft partner](https://azure.microsoft.com/partners/directory/) who can design and implement your Azure cloud solution. These partners have the business and technology expertise to recommend solutions that meet the unique needs of your business.|
|![](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/personal-icon.png)|**Personal free account**  <br>  <br>Any user can sign up for a [free trial account](https://azure.microsoft.com/free/). You can get started using Azure right away, and you won't be charged until you choose to upgrade.|

# Identify Azure subscription usage

We reviewed the ways you can obtain an Azure subscription. Now let's look at the types of Azure subscriptions that are available.

Azure offers free and paid subscription options to meet different needs and requirements. The most common subscriptions are **Free**, **Pay-As-You-Go**, **Enterprise Agreement**, and **Student**. For your organization, you can choose a combination of procurement options and subscription choices to meet your business scenarios.

### Things to consider when choosing Azure subscriptions

As you think about which types of Azure subscriptions would work for your organization, consider these scenarios:

- **Consider trying Azure for free**. An Azure free subscription includes a monetary credit to spend on any service for the first 30 days. You get free access to the most popular Azure products for 12 months, and access to more than 25 products that are always free. An Azure free subscription is an excellent way for new users to get started.
    
    - To set up a free subscription, you need a phone number, a credit card, and a Microsoft account.
    - The credit card information is used for identity verification only. You aren't charged for any services until you upgrade to a paid subscription.
- **Consider paying monthly for used services**. A Pay-As-You-Go (PAYG) subscription charges you monthly for the services you used in that billing period. This subscription type is appropriate for a wide range of users, from individuals to small businesses, and many large organizations as well.
    
- **Consider using an Azure Enterprise Agreement**. An Enterprise Agreement provides flexibility to buy cloud services and software licenses under one agreement. The agreement comes with discounts for new licenses and Software Assurance. This type of subscription targets enterprise-scale organizations.
    
- **Consider supporting Azure for students**. An Azure for Students subscription includes a monetary credit that can be used within the first 12 months.
    
    - Students can select free services without providing a credit card during the sign-up process.
    - You must verify your student status through your organizational email address.

 Note

For a complete list of Azure subscription options, see the current [Microsoft Azure offers](https://azure.microsoft.com/support/legal/offer-details/).

# Implement Microsoft Cost Management


With Azure products and services, you pay only for what you use. As you create and use Azure resources, you're charged for the resources.

Microsoft Cost Management provides support for administrative billing tasks and helps you manage billing access to costs. You can use the product to monitor and control Azure spending, and optimize your Azure resource usage.

![Screenshot of the Microsoft Cost Management dashboard showing service name and location costs, and billing forecasts.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/cost-management-67750831.png)

### Things to know about Microsoft Cost Management

Your organization is interested in the benefits of using Microsoft Cost Management to monitor their subscription billing and resource usage. As you plan for your implementation, review the following product characteristics and features:

- Microsoft Cost Management shows organizational cost and usage patterns with advanced analytics. Costs are based on negotiated prices and factor in reservation and Azure Hybrid Benefit discounts. Predictive analytics are also available.
    
- Reports in Microsoft Cost Management show the usage-based costs consumed by Azure services and third-party Marketplace offerings. Collectively, the reports show your internal and external costs for usage and Azure Marketplace charges. The reports help you understand your spending and resource use, and can help find spending anomalies. Charges, such as reservation purchases, support, and taxes might not be visible in reports.
    
- The product uses Azure management groups, budgets, and recommendations to show clearly how your expenses are organized and how you might reduce costs.
    
- You can use the Azure portal or various APIs for export automation to integrate cost data with external systems and processes. Automated billing data export and scheduled reports are also available.
    

### Things to consider when using Microsoft Cost Management

Microsoft Cost Management can help you plan for and control your organization costs. Consider how the product features can be implemented to support your business scenarios:

- **Consider cost analysis**. Take advantage of Microsoft Cost Management cost analysis features to explore and analyze your organizational costs. You can view aggregated costs by organization to understand where costs are accrued, and to identify spending trends. Monitor accumulated costs over time to estimate monthly, quarterly, or even yearly cost trends against a budget.
    
- **Consider budget options**. Use Microsoft Cost Management features to establish and maintain budgets. The product helps you plan for and meet financial accountability in your organization. Budgets help prevent cost thresholds or limits from being surpassed. You can utilize analysis data to inform others about their spending to proactively manage costs. The budget features help you see how company spending progresses over time.
    
- **Consider recommendations**. Review the Microsoft Cost Management recommendations to learn how you can optimize and improve efficiency by identifying idle and underutilized resources. Recommendations can reveal less expensive resource options. When you act on the recommendations, you change the way you use your resources to save money. Using recommendations is an easy process:
    
    1. View cost optimization recommendations to see potential usage inefficiencies.
    2. Act on a recommendation to modify your Azure resource use and implement a more cost-effective option.
    3. Verify the new action to make sure the change has the desired effect.
- **Consider exporting cost management data**. Microsoft Cost Management helps you work with your billing information. If you use external systems to access or review cost management data, you can easily export the data from Azure.
    
    - Set a daily scheduled export in comma-separated-value (CSV) format and store the data files in Azure storage.
    - Access your exported data from your external system.

# Apply resource tagging

You can apply tags to your Azure resources to logically organize them by categories. Tags are useful for sorting, searching, managing, and doing analysis on your resources.

Each resource tag consists of a name and a value. You could have the tag name `Server` and the value `Production` or `Development`, and then apply the tag/value pair to your Engineering computer resources.

Here's an example that shows how to add tags for a resource group in the Azure portal:

![Screenshot that shows how to add tags for a resource group in the Azure portal.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/resource-tags-357b5e1a.png)

### Things to know about resource tags

As you plan your Azure subscriptions, resources, and services, review these characteristics of Azure resource tags:

- Each resource tag has a name and a value.
    
- The tag name remains constant for all resources that have the tag applied.
    
- The tag value can be selected from a defined set of values, or unique for a specific resource instance.
    
- A resource or resource group can have a maximum of 50 tag name/value pairs.
    
- Tags applied to a resource group aren't inherited by the resources in the resource group.
    

### Things to consider when using resource tags

Here are a few things you can do with resource tags:

- **Consider searching on tag data**. Search for resources in your subscription by querying on the tag name and value.
    
- **Consider finding related resources**. Retrieve related resources from other resource groups by searching on the tag name or value.
    
- **Consider grouping billing data**. Group resources like virtual machines by cost center and production environment. When you download the resource usage comma-separated values (CSV) file for your services, the tags appear in the `Tags` column.
    
- **Consider creating tags with PowerShell or the Azure CLI**. Create many resource tags programmatically by using Azure PowerShell or the Azure CLI.
    

### How to keep your Azure subscription clean

![https://youtu.be/EigjR891PIM](https://youtu.be/EigjR891PIM)


# Apply cost savings


Azure has several options that can help you gain significant cost savings for your organization. As you prepare your implementation plan for Azure subscriptions, services, and resources, consider the following cost saving advantages.

Expand table

|Cost saving|Description|
|---|---|
|**Reservations**|Save money by paying ahead. You can pay for one year or three years of virtual machine, SQL Database compute capacity, Azure Cosmos DB throughput, or other Azure resources. Pre-paying allows you to get a discount on the resources you use. Reservations can significantly reduce your virtual machine, SQL database compute, Azure Cosmos DB, or other resource costs up to 72% on pay-as-you-go prices. Reservations provide a billing discount and don't affect the runtime state of your resources.|
|**Azure Hybrid Benefits**|Access pricing benefits if you have a license that includes _Software Assurance_. Azure Hybrid Benefits helps maximize the value of existing on-premises Windows Server or SQL Server license investments when migrating to Azure. There's an Azure Hybrid Benefit Savings Calculator to help you determine your savings.|
|**Azure Credits**|Use the monthly credit benefit to develop, test, and experiment with new solutions on Azure. As a Visual Studio subscriber, you could use Microsoft Azure at no extra charge. With your monthly Azure credit, Azure is your personal sandbox for development and testing.|
|**Azure regions**|Compare pricing across regions. Pricing can vary from one region to another, even in the US. Double check the pricing in various regions to see if you can save by selecting a different region for your subscription.|
|**Budgets**|Apply the budgeting features in Microsoft Cost Management to help plan and drive organizational accountability. With budgets, you can account for the Azure services you consume or subscribe to during a specific period. Monitor spending over time and inform others about their spending to proactively manage costs. Use budgets to compare and track spending as you analyze costs.|
|**Pricing Calculator**|The [Pricing Calculator](https://azure.microsoft.com/pricing/calculator/) provides estimates in all areas of Azure, including compute, networking, storage, web, and databases.|

The following image shows a scenario for using the Pricing Calculator. The customer has an instance of a D1 series VM on Windows. The instance is running in the West US region at the standard tier level.

 ![Screenshot of the Pricing Calculator with estimates for an instance of a D1 series VM on Windows. The instance is running in the West US region at the standard tier level.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-subscriptions/media/pricing-calculator-c1cb80d5.png)


# Summary and resources


Azure Administrators commonly obtain and manage Azure subscriptions. Azure subscriptions help you effectively identify and manage costs for your organization, so you can provide services and resources for specific scenarios.

In this module, you learned about supported Azure regions, and how to locate Azure services. You reviewed the features and use cases for Azure subscriptions, and how to obtain subscriptions. You explored features and billing for different types of Azure subscriptions, and how to apply resource tagging. You discovered how Microsoft Cost Management can be used for cost analysis. You learned how to identify ways you can reduce your billing costs.

The main takeaways from this module are:

- Azure regions provide flexibility, data residency, compliance, and resiliency options.
    
- Azure subscriptions are essential for managing access to Azure resources and billing.
    
- Azure offers various subscription options such as Free, Pay-As-You-Go, Enterprise Agreement, and Student.
    
- Azure offers cost-saving options such as reservations, Azure Hybrid Benefits and Azure credits.
    
- Resource tagging allows for organizing and analyzing resources in Azure.
    
- Microsoft Cost Management helps monitor and control Azure spending.
    
- The Pricing Calculator provides billing estimates for different usage cases.
    

## Learn more with Azure documentation

- [Cost management](https://learn.microsoft.com/en-us/azure/cost-management-billing/#cost-management). This collection of articles covers pricing, reporting, analytics, monitoring and optimizing costs.
    
- [Billing and subscriptions management](https://learn.microsoft.com/en-us/azure/cost-management-billing/#billing---subscriptions). This collection of articles covers managing your account, subscriptions, bills, and payments.
    
- [Pricing calculator](https://azure.microsoft.com/pricing/calculator/). This tool estimates the hourly or monthly costs for using Azure.
    

## Learn more with self-paced training

- [Control Azure spending and manage bills with Microsoft Cost Management + Billing](https://learn.microsoft.com/en-us/training/paths/control-spending-manage-bills/). Learn how to monitor and control your Azure spending and optimize the use of Azure resources.
    
- [Introduction to analyzing costs and creating budgets with Microsoft Cost Management (exercise, subscription required)](https://learn.microsoft.com/en-us/training/modules/analyze-costs-create-budgets-azure-cost-management/). Learn how to use cost analysis to understand how your costs accrue each month. Use this understanding to create an Azure budget to monitor and alert on your costs.
    
- [Describe cost management in Azure (exercise, subscription required)](https://learn.microsoft.com/en-us/training/modules/describe-cost-management-azure/). Explore methods to estimate, track, and manage costs in Azure.

---

# Configure Azure Policy


Learn how to configure Azure Policy to implement compliance requirements.

## Learning objectives

In this module, you learn how to:

- Create management groups to target policies and spending budgets.
- Implement Azure Policy with policy and initiative definitions.
- Scope Azure policies and determine compliance.

# Introduction


[Azure Policy](https://azure.microsoft.com/services/azure-policy/) is a service in Azure that enables you to create, assign, and manage policies to control or audit your resources. These policies enforce different rules over your resource configurations so the configurations stay compliant with corporate standards.

Your company is subject to many regulations and compliance rules. Your company wants to ensure each department implements and deploys resources correctly. You're responsible for investigating how to use Azure Policy and management groups to implement compliance measures.

In this module, you learn how to implement Azure policies. The content includes examples of policy definitions and initiative definitions. You learn how to scope your policies and identify noncompliant resources. You also learn the benefits and usage of management groups.

The goal to this module is to ensure Administrators can use Azure policy to ensure resource compliance.

## Learning objectives

In this module, you learn how to:

- Implement Azure Policy with policy and initiative definitions.
    
- Scope Azure policies and determine compliance.
    
- Create management groups to target policies and spending budgets.
    

## Skills measured

The content in the module helps you prepare for [Exam AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/certifications/exams/az-104).

## Prerequisites

- Basic understanding of Azure concepts, such as subscriptions, resource groups, and resources.
- Working knowledge of governance concepts, such as compliance and reporting.
- Familiarity with Azure Resource Manager and how to use it to deploy and manage resources.
- Knowledge of JSON syntax so you can create policy definitions and policy assignments.


# Create management groups

Organizations that use multiple subscriptions need a way to efficiently manage access, policies, and compliance. Azure management groups provide a level of scope and control above your subscriptions. You can use management groups as containers to manage access, policy, and compliance across your subscriptions.

Things to know about management groups
Consider the following characteristics of Azure management groups:

By default, all new subscriptions are placed under the top-level management group, or root group.

All subscriptions within a management group automatically inherit the conditions applied to that management group.

A management group tree can support up to six levels of depth.

Azure role-based access control authorization for management group operations isn't enabled by default.

The following diagram shows how Azure management groups can be used to organize subscriptions in a hierarchy of unified policy and access management. In this scenario, the organization has a single top-level management group. Every directory under the root group is folded into the top-level group.

![](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-policy/media/management-groups-aa92c04a.png)

Things to consider when using management groups
Review the following ways you can use management groups in Azure Policy to manage your subscriptions:

- **Consider custom hierarchies and groups**. Align your Azure subscriptions by using custom hierarchies and grouping that meet your company's organizational structure and business scenarios. You can use management groups to target policies and spending budgets across subscriptions.

- **Consider policy inheritance**. Control the hierarchical inheritance of access and privileges in policy definitions. All subscriptions within a management group inherit the conditions applied to the management group. You can apply policies to a management group to limit the regions available for creating virtual machines (VMs). The policy can be applied to all management groups, subscriptions, and resources under the initial management group, to ensure VMs are created only in the specified regions.

- **Consider compliance rules**. Organize your subscriptions into management groups to help meet compliance rules for individual departments and teams.

- **Consider cost reporting**. Use management groups to do cost reporting by department or for specific business scenarios. You can use management groups to report on budget details across subscriptions.

# Create management groups
You can create a management group with Azure Policy by using the Azure portal, PowerShell, or the Azure CLI. Here's an example of what you see in the Azure portal:

![](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-policy/media/create-management-groups-66ff9f21.png)

A management group has a directory unique identifier (ID) and a display name. The ID is used to submit commands on the management group. The ID value can't be changed after it's created because it's used throughout the Azure system to identify the management group. The display name for the management group is optional and can be changed at any time.

# Implement Azure policies


Azure Policy is a service in Azure that you can use to create, assign, and manage policies. You can use policies to enforce rules on your resources to meet corporate compliance standards and service level agreements. Azure Policy runs evaluations and scans on your resources to make sure they're compliant.

### Things to know about Azure Policy

The main advantages of Azure Policy are in the areas of enforcement and compliance, scaling, and remediation. Azure Policy is also important for teams that run an environment that requires different forms of governance.

Expand table

|Advantage|Description|
|---|---|
|**Enforce rules and compliance**|Enable built-in policies, or build custom policies for all resource types. Support real-time policy evaluation and enforcement, and periodic or on-demand compliance evaluation.|
|**Apply policies at scale**|Apply policies to a management group with control across your entire organization. Apply multiple policies and aggregate policy states with policy initiative. Define an exclusion scope.|
|**Perform remediation**|Conduct real-time remediation, and remediation on your existing resources.|
|**Exercise governance**|Implement governance tasks for your environment:  <br>- Support multiple engineering teams (deploying to and operating in the environment)  <br>- Manage multiple subscriptions  <br>- Standardize and enforce how cloud resources are configured  <br>- Manage regulatory compliance, cost control, security, and design consistency|

### Things to consider when using Azure Policy

Review the following scenarios for using Azure Policy. Consider how you can implement the service in your organization.

- **Consider deployable resources**. Specify the resource types that your organization can deploy by using Azure Policy. You can specify the set of virtual machine SKUs that your organization can deploy.
    
- **Consider location restrictions**. Restrict the locations your users can specify when deploying resources. You can choose the geographic locations or regions that are available to your organization.
    
- **Consider rules enforcement**. Enforce compliance rules and configuration options to help manage your resources and user options. You can enforce a required tag on resources and define the allowed values.
    
- **Consider inventory audits**. Use Azure Policy with Azure Backup service on your VMs and run inventory audits.

# Create Azure policies

Azure Administrators use Azure Policy to create policies that define conventions for resources. A _policy definition_ describes the compliance conditions for a resource, and the actions to complete when the conditions are met. One or more policy definitions are grouped into an _initiative definition_, to control the scope of your policies and evaluate the compliance of your resources.

![Diagram that shows an initiative definition for a group of policy definitions that are applied to resources.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-policy/media/implement-azure-policy-b4a4a47c.png)

There are four basic steps to create and work with policy definitions in Azure Policy.

### Step 1: Create policy definitions

A policy definition expresses a condition to evaluate and the actions to perform when the condition is met. You can create your own policy definitions, or choose from built-in definitions in Azure Policy. You can create a policy definition to prevent VMs in your organization from being deployed, if they're exposed to a public IP address.

### Step 2: Create an initiative definition

An initiative definition is a set of policy definitions that help you track your resource compliance state to meet a larger goal. You can create your own initiative definitions, or use built-in definitions in Azure Policy. You can use an initiative definition to ensure resources are compliant with security regulations.

### Step 3: Scope the initiative definition

Azure Policy lets you control how your initiative definitions are applied to resources in your organization. You can limit the scope of an initiative definition to specific management groups, subscriptions, or resource groups.

### Step 4: Determine compliance

After you assign an initiative definition, you can evaluate the state of compliance for all your resources. Individual resources, resource groups, and subscriptions within a scope can be exempted from having the policy rules affect it. Exclusions are handled individually for each assignment.

# Create policy definitions


Azure Policy offers built-in policy definitions to help you quickly configure control conditions for your resources. In addition to the built-in policies, you can also create your own definitions, or import definitions from other sources.

## Access built-in policy definitions

You can sort the [list of built-in definitions](https://learn.microsoft.com/en-us/azure/governance/policy/samples/built-in-policies) by category to search for policies that meet your business needs.

![Screenshot that shows a list of built-in policy definitions in Azure Policy.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-policy/media/policy-definition-3ce7a058.png)

Here are some examples of built-in policy definitions:

- **Allowed virtual machine size SKUs**: Specify a set of VM size SKUs that your organization can deploy. This policy is located under the Compute category.
    
- **Allowed locations**: Restrict the locations users can specify when deploying resources. Use this policy to enforce your geo-compliance requirements. This policy is located under the General category.
    
- **Configure Azure Device Update for IoT Hub accounts to disable public network access**: Disable public network access for your Device Update for IoT Hub resources. This policy is located under the Internet of Things category.
    

## Add new policy definitions

If you don't find a built-in policy to meet your business needs, you can add or create a new definition. Policy definitions can also be imported into Azure Policy from [GitHub](https://github.com/Azure/azure-policy/tree/master/samples). New policy definitions are added to the samples repository almost daily.

![Screenshot that shows how to add a new policy definition, and the option to import a sample policy definition from GitHub.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-policy/media/new-policy-definition-46cb3ecb.png)

 Note

When you add or create a policy definition, be sure the definition uses the specific JSON format required by Azure. For more information, see [Azure Policy definition structure](https://learn.microsoft.com/en-us/azure/governance/policy/concepts/definition-structure).

# Create an initiative definition


After you determine your policy definitions, the next step is to create an initiative definition for your policies. An initiative definition has one or more policy definitions. One example for using initiative definitions is to ensure your resources are compliant with security regulations.

> Tip
>
> Even if you have only a few policy definitions in your organization, we recommend creating and applying an initiative definition.

## Add a new initiative definition

When you create an initiative definition, be sure the definition uses the specific JSON format required by Azure. For more information, see [Azure Policy initiative definition structure](https://learn.microsoft.com/en-us/azure/governance/policy/concepts/initiative-definition-structure).

Here's an example of how to create a new initiative definition in the Azure portal:

![Screenshot that shows how to create a new initiative definition.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-policy/media/create-initiative-definition-e1198a51.png)

## Use a built-in initiative definition

You can create your own initiative definitions, or use built-in definitions in Azure Policy. You can sort the [list of built-in initiatives](https://learn.microsoft.com/en-us/azure/governance/policy/samples/built-in-initiatives) by category to search for definitions for your organization.

Here are some examples of built-in initiative definitions:

- **Audit machines with insecure password security settings**: Use this initiative to deploy an audit policy to specified resources in your organization. The definition evaluates the resources to check for insecure password security settings. This initiative is located in the Guest Configuration category.
    
- **Configure Windows machines to run Azure Monitor Agent and associate them to a Data Collection Rule**: Use this initiative to monitor and secure your Windows VMs, Virtual Machine Scale Sets, and Arc machines. The definition deploys the Azure Monitor Agent extension and associates the resources with a specified Data Collection Rule. This initiative is located in the Monitoring category.
    
- **Configure Azure Defender to be enabled on SQL servers**: Enable Azure Defender on your Azure SQL Servers to detect anomalous activities indicating unusual and potentially harmful attempts to access or exploit databases. This initiative is located in the SQL category.

# Scope the initiative definition


After you create your initiative definition, the next step is to assign the initiative to establish the scope for the policies. The scope determines what resources or grouping of resources are affected by the conditions of the policies.

Here's an example that shows how to configure the scope assignment:

![Screenshot that shows how to assign an initiative definition to resources or groups or resources to establish the scope.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-policy/media/assign-definition-87bc203c.png)

To establish the scope, you select the affected subscriptions. As an option, you can also choose the affected resource groups.

The following example shows how to apply the scope:

![Screenshot that shows how a scope is applied to a subscription, and optionally applied to a resource group.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-policy/media/scope-initiative-9fbf59d5.png)

# Determine compliance


You have your policies defined, your initiative definition created, and your policies assigned to affected resources. The last step is to evaluate the state of compliance for your scoped resources.

The following example shows how you can use the **Compliance** feature to look for non-compliant initiatives, policies, and resources.

![Screenshot that shows how to use the compliance feature to look for non-compliant initiatives, policies, and resources.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-policy/media/determine-compliance-c198f4ba.png)

Your policy conditions are evaluated against your existing scoped resources. Although the Azure portal doesn't show the evaluation logic, the compliance state results are shown. The compliance state result is either compliant or non-compliant.

# Interactive lab simulation

## Lab scenario

Your organization is piloting a new infrastructure project. The CTO wants to know which Azure resources are being used on the new project. Your specific tasks are:

- Create a way to tag the project resources.
- Don't allow resources to be created without the resource tag.
- If a resource is created without the tag, automatically add the tag.

## Architecture diagram

![Architecture diagram as explained in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-policy/media/lab-02b.png)

## Objectives

- **Task 1**: Create and assign tags via the Azure portal.
    - For testing purposes, identify the Cloud Shell resource group.
    - Add a tag to the resource group. Assign the value of the tag.
    - Verify the storage account in the resource group doesn't have the tag.
- **Task 2**: Enforce tagging by using an Azure policy.
    - Locate the **Require a tag and its value on resources** built-in policy and review the definition.
    - Assign the policy to the resource group.
    - Configure the required tag: **Role** with a value of **Infra**.
    - Create a new storage account in the resource group and verify without the tag you can't create the resource.
- **Task 3**: Automatically apply tagging by using an Azure policy.
    - Assign the **Inherit a tag from the resource group if missing** built-in policy to the resource group.
    - Configure remediation to automatically add the **Role** tag if it is missing from a new resource.
    - Create a new storage account and verify the tag and value are added.

 Note

Click on the thumbnail image to start the lab simulation. When you're done, be sure to return to this page so you can continue learning.

[![Screenshot of the simulation page.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-azure-policy/media/simulation-policy-thumbnail.jpg)](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%203)

# Summary and resources


Azure Policy is a service in Azure that enables you to create, assign, and manage policies. Azure Policy helps you define and implement your governance strategy by using policies to control and audit your resources.

In this module, you have learned about Azure Policy and how it allows you to control and audit your resources. You explored how to implement Azure policy definitions and initiatives for your corporate departments. You learned how to create management groups, scope policies, and manage spending budgets. You reviewed how Azure policies can be scoped to meet compliance regulations.

The main takeaways from this module are:

- Azure Policy is a powerful service in Azure that enables you to enforce rules and ensure compliance with corporate standards and service level agreements.
- Management groups provide a way to efficiently manage access, policies, and compliance across multiple subscriptions, allowing for unified policy and access management.
- Creating policy definitions and initiative definitions allows you to define conventions for resources and control the scope of policies, ensuring resource compliance.
- The Compliance feature in Azure Policy helps you determine the compliance state of your resources and evaluate whether they're compliant or non-compliant.

## Learn more with Azure documentation

- [Azure Policy documentation](https://learn.microsoft.com/en-us/azure/governance/policy/). This collection of articles is your starting point for all things Azure policy.
    
- [Azure Policy built-in policy definitions](https://learn.microsoft.com/en-us/azure/governance/policy/samples/built-in-policies). This page is an index of Azure Policy built-in policy definitions.
    
- [Azure built-in policy initiatives](https://learn.microsoft.com/en-us/azure/governance/policy/samples/built-in-initiatives). This page is an index of Azure Policy built-in initiative definitions.
    
- [Quickstart: Create a policy assignment to identify noncompliant resources](https://learn.microsoft.com/en-us/azure/governance/policy/assign-policy-portal). This quickstart steps you through the process of creating a policy assignment to identify virtual machines that aren't using managed disks.
    

## Learn more with self-paced training

- [Introduction to Azure Policy](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-policy/). This module introduces you to Azure Policy and describes its characteristics, capabilities, and use cases.

---
# Manage users and groups in Microsoft Entra ID


In this module, you'll learn to manage users and groups in Microsoft Entra ID.

## Learning objectives

In this module, you will:

- Learn the difference between Microsoft Entra ID and Windows Server Active Directory.
- Understand tenants, subscriptions, and users.
- Create a new Microsoft Entra ID.
- Add users and groups to a Microsoft Entra ID.
- Manage roles in a Microsoft Entra ID.
- Learn how to create a hybrid identity solution with Microsoft Entra Connect.

# Introduction



Transitioning workloads to the cloud involves more than just moving servers, websites, and data. Companies need to think about how to secure those resources, identify authorized users, and ensure that the data accessed, services created, and operations performed by those users is permitted. Security is a complex area, and it's easy to get wrong, as shown by the multitude of successful attacks on big companies in the news.

We need to control user access to company data centrally, provide a definitive identity for each user that a company uses for each service, and ensure employees and vendors have _just enough access_ to do their job. When an employee leaves or a vendor's contract is over, ensuring that their access is removed is even more important.

Azure tries to make these sorts of problems easier to solve with Microsoft Entra ID. Microsoft Entra ID is Microsoft's cloud-based identity and access-management service, which provides single sign-on and multifactor authentication to help protect your users from 99.9 percent of cybersecurity attacks.

## Learning objectives

In this module, you'll:

- Learn the difference between Microsoft Entra ID and Windows Server Active Directory.
- Understand tenants, subscriptions, and users.
- Create a new Microsoft Entra ID.
- Add users and groups to a Microsoft Entra ID.
- Manage roles in a Microsoft Entra ID.
- Learn how to create a hybrid identity solution with Microsoft Entra Connect.

## Prerequisites

- Basic understanding of identity and role-based access control
- Experience using the Azure portal

# What is Microsoft Entra ID?


Though they once shared a similar name, Microsoft Entra ID is _not_ a cloud version of Windows Server Active Directory. It's also not intended as a complete replacement for an on-premises Active Directory. Instead, if you're already using a Windows AD server, you can connect it to Microsoft Entra ID to extend your directory into Azure. This approach allows users to use the same credentials to access local and cloud-based resources.

![Conceptual art showing Windows AD and Microsoft Entra ID controlling resources.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/2-azure-vs-windows-ad.png)

A user can also use Microsoft Entra ID independently of Windows AD. Smaller companies can use Microsoft Entra ID as their only directory service to control access to their applications and SaaS products, such as Microsoft 365, Salesforce, and Dropbox.

 Note

Keep in mind that this approach doesn't provide a completely centralized administrative model; for example, local Windows machines would authenticate using local credentials. Users can write applications to use Microsoft Entra ID and provide authentication and authorization to be administered by a user in a single place.

## Directories, subscriptions, and users

Microsoft offers several cloud-based offerings today, all of which can use Microsoft Entra ID to identify users and control access:

- Microsoft Azure
- Microsoft 365
- Microsoft Intune
- Microsoft Dynamics 365

When a company or organization signs up to use one of these offerings, they're assigned a default _directory_, an instance of Microsoft Entra ID. This directory holds the users and groups that will have access to each of the services the company has purchased. You can refer to this default directory as a _tenant_. A tenant represents the organization and the default directory assigned to it.

**A _subscription_ in Azure is both a billing entity and a security boundary**. Resources such as virtual machines, websites, and databases are associated with a single subscription. Each subscription also has a single account _owner_ responsible for any charges incurred by resources in that subscription. If your organization wants a subscription billed to another account, you can transfer the subscription. `A subscription is associated with a **single Microsoft Entra directory**`. Multiple subscriptions can trust the same directory, but a subscription can only trust one directory.

You can add users and groups to multiple subscriptions. This allows the user to create, control, and access resources in the subscription. When you add a user to a subscription, the user must be known to the associated directory, as shown in the following image:

![Conceptual art showing users, directories, and subscriptions in Azure.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/2-users-subs-and-directories.png)

If you belong to multiple directories, you can switch the current directory in which you're working through the **Directory + subscription** button in the Azure portal header.

![Screenshot showing the Directory selection dialog in Azure portal.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/2-directory-and-subscription.png)

You can also decide how the default directory is selected: last visited or a specific directory. You can also set the default filter for displayed subscriptions. Default filters are useful if you have access to several subscriptions, but typically only work in a few of them.

## Create a new directory

An organization (tenant) has one associated default Microsoft Entra directory. However, owners can create additional directories to support development or testing purposes, or because they want to have separate directories to synchronize with their local Windows Server AD forests.

 Important

The steps to create a new directory follow; however, unless you're an owner of your Azure account, this option won't be available to you. The Azure Sandbox doesn't allow you to create new Microsoft Entra directories.

1. Sign in to the [Azure portal](https://portal.azure.com/).
    
2. On the Azure home page, under **Azure services**, select **Create a resource**.
    
3. In the left menu pane, select **Identity**, and then search for and select **Microsoft Entra ID**.
    
4. Select **Create**.
    
5. Select **Microsoft Entra ID** for the tenant type, then select **Next : Configuration**.
    
6. Enter the following values for each setting.
    
    - **Organization name**: Enter a name for the directory to help distinguish it from your other directories. The directory to be created will be used in production; provide a name that your users will recognize as your organization's name. You can change the name later if you want.
        
    - **Initial domain name**: Enter a domain name associated with your organization. Azure will give a validation error unless the domain isn't known. The default domain name will always have the suffix `.onmicrosoft.com`. You can't change the default domain. If you choose to, you can add a custom domain owned by your organization so defined users can use a traditional company email, such as `john@contoso.com`.
        
    - **Country or region**: Select the country/region in which the directory should reside. The country/region will identify the region and data center where the Microsoft Entra instance will live; you can't change it later.
        
    
    ![Screenshot showing the AD creation process.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/2-create-directory.png)
    
7. Select **Create** to create the new directory. A free tier directory will be created where you can add users, create roles, register apps and devices, and control licenses.
    

After you've created the directory, select **Click here to manage your new tenant** to go to the Overview dashboard that lets you control all directory aspects.

![Screenshot of the Microsoft Entra dashboard.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/2-aad-dashboard.png)

Let's explore one of the primary elements you'll work with in Microsoft Entra ID: **users**.


# Create and manage users


Every user who needs access to Azure resources needs an Azure user account. Your user account contains all the information needed to authenticate you during the sign-in process. Once authenticated, Microsoft Entra ID builds an access token to authorize you, determine what resources you can access, and determine what you can do with those resources.

You can use the **Microsoft Entra ID** dashboard in the Azure portal to work with user objects. Keep in mind that you can only work with a single directory at a time, but you can use the **Directory + Subscription** pane to switch directories. The dashboard also has a **Manage tenants** button in the toolbar, which makes it easy to view all your directories and switch to another available directory.

## View users

To view the Microsoft Entra users, in the left menu pane, under **Manage**, select **Users**. The **All Users** pane appears. Notice the **User type** and **Identities** columns, as shown in the following screenshot:

![Screenshot that depicts the All users pane, with the User type and Identities columns noted.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/m1-aad-users.png)

Typically, Microsoft Entra ID defines users in three ways:

- **Cloud identities**: These users exist only in Microsoft Entra ID. Examples are administrator accounts and users that you manage yourself. Their source is **Microsoft Entra ID** or **External Microsoft Entra ID** if the user is defined in another Microsoft Entra instance, but needs access to subscription resources controlled by this directory. When these accounts are removed from the primary directory, they are deleted.
    
- **Directory-synchronized identities**: These users exist in an on-premises Active Directory. A synchronization activity that occurs via **Microsoft Entra Connect** brings these users in to Azure. Their source is **Windows Server AD**.
    
- **Guest users**: These users exist outside Azure. Examples are accounts from other cloud providers and Microsoft accounts, such as an Xbox LIVE account. Their source is **Invited user**. This type of account is useful when external vendors or contractors need access to your Azure resources. Once their help is no longer necessary, you can remove the account and all of their access.
    

## Add users

You can add cloud identities to Microsoft Entra ID in multiple ways:

- Syncing an on-premises Windows Server Active Directory
- Using the Azure portal
- Using the command line
- Other options

### Sync an on-premises Windows Server Active Directory

Microsoft Entra Connect is a separate service that allows you to synchronize a traditional Active Directory with your Microsoft Entra instance. This is how most enterprise customers add users to the directory. The advantage to this approach is users can use single sign-on (SSO) to access local and cloud-based resources.

### Use the Azure portal

You can manually add new users through the Azure portal. This is the easiest way to add a small set of users. You need to be in the **User Administrator** role to perform this function.

1. To add a new user with the Azure portal, in the top menu bar, select **New user**, then select **Create new user**.
    
    ![Screenshot showing the New User button highlighted in the Microsoft Entra admin center.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/2-new-user-all-users-pane.png)
    
2. In addition to **Name** and **User name**, you can add profile information, like **Job Title** and **Department**, on the **Properties** tab.
    
    ![Screenshot showing the New user dialog.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/2-new-user-user-pane.png)
    
    The default behavior is to create a new user in the organization. The user will have a username with the default domain name assigned to the directory such as alice@staracoustics.onmicrosoft.com.
    
3. You can also _invite_ a user into the directory. In this case, an email is sent to a known email address, and an account is created and associated with that email address if the user accepts the invitation.
    
    ![Screenshot showing the invite screen.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/3-new-user-invite.png)
    
    The invited user will need to create an associated Microsoft account (MSA) if that specific email address isn't associated with one, and the account will be added to the Microsoft Entra ID as a guest user.
    

### Use the command line

If you have a lot of users to add, a better option is to use a command-line tool. You can run the [New-MgUser](https://learn.microsoft.com/en-us/powershell/module/microsoft.graph.users/new-mguser) PowerShell command to add cloud-based users.

PowerShellCopy

```
# Create a password profile value
$PasswordProfile = @{ Password = "<Password>" }

# Create the new user
New-MgUser -DisplayName "Abby Brown" -PasswordProfile $PasswordProfile -MailNickName "AbbyB" -UserPrincipalName "AbbyB@contoso.com" -AccountEnabled 
```

The command will return the new user object you created.

OutputCopy

```
DisplayName Id                                    UserPrincipalName
----------- --                                    -----------------
Abby Brown  f36634c8-8a93-4909-9248-0845548bc515  AbbyB@contoso.com
```

If you prefer a more standard command-line interface, you can use the Azure CLI:

Azure CLICopy

```
az ad user create --display-name "Abby Brown" \
                  --password "<password>" \
                  --user-principal-name "AbbyB@contoso.com" \
                  --force-change-password-next-login true \
                  --mail-nickname "AbbyB"
```

Command-line tools allow you to add users in bulk through scripting. The most common approach for this is to use a comma-separated values (CSV) file. You can either manually create this file or export the file from an existing data source.

If you're planning to use a CSV, here are some things to think about:

- **Naming conventions**: Establish or implement a naming convention for usernames, display names, and aliases. For example, a username might consist of the last name, followed by a period (.), followed by the first name; for example, Smith.John@contoso.com.
    
- **Passwords**: Implement a convention for the initial password of a newly created user. Determine how new users will receive their passwords in a security-enhanced way. A commonly used method is generating a random password and then emailing it to the new user or their manager.
    

To use a CSV with Azure PowerShell:

1. Run the [Connect-MgGraph](https://learn.microsoft.com/en-us/powershell/microsoftgraph/authentication-commands#using-connect-mggraph) command to create a PowerShell connection to your directory. Connect with an admin account that has privileges on your directory.
    
2. Create new password profiles for the new users. The passwords for the new users need to conform to the password-complexity rules you have set for your directory.
    
3. Use `Import-CSV` to import the CSV. You need to specify the path and file name of the CSV.
    
4. Loop through the users in the file, constructing the user parameters needed for each user. Example parameters are User Principal Name, Display Name, Given Name, Department, and Job Title.
    
5. Run the `New-MgUser` command to create each user. Be sure to enable each account.
    

### Other options

You can also add users to Microsoft Entra ID programmatically using the Microsoft Graph API, or through the Microsoft 365 Admin Center and the Microsoft Intune Admin console if you're sharing the same directory.


# Create and manage groups


A Microsoft Entra group helps organize users, which makes it easier to manage permissions. Using groups lets the resource owner (or Microsoft Entra directory owner), assign a set of access permissions to all the group members instead of having to provide the rights one by one. Groups allow you to define a security boundary, then add and remove specific users to grant or deny access with a minimum amount of effort. Even better, Microsoft Entra ID supports the ability to define membership based on rules, such as what department a user works in or the job title they have.

Microsoft Entra ID allows you to define two different types of groups.

1. **Security groups**: These are the most common, and are used to manage member and computer access to shared resources for a group of users. For example, you can create a security group for a specific security policy. By doing it this way, you can give a set of permissions to all the members at once instead of having to add permissions to each member individually. This option requires a Microsoft Entra administrator.
    
2. **Microsoft 365 groups**: These groups provide collaboration opportunities by giving members access to a shared mailbox, calendar, files, SharePoint site, and more. This option also lets you give people outside of your organization access to the group. This option is available to users as well as admins.
    

## View available groups

You can view all groups by selecting **Groups** under the **Manage** section from the Microsoft Entra dashboard. A new Microsoft Entra ID install won't have any groups defined.

![Screenshot that depicts all the existing groups.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/m1-groups1.png)

## Add groups to Microsoft Entra ID

The same options we saw with users are available to create groups in Microsoft Entra ID. The Azure portal is the easiest way to create groups. You must select the group type (Security or Microsoft 365), assign a unique group name, description and a _membership type_.

![Screenshot of the Create Group feature in the Azure portal.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/4-add-group-portal.png)

The membership type field can be one of three values:

1. **Assigned (static)**. The group will contain specific users or groups that you select.
    
2. **Dynamic user**. You can create rules based on characteristics to enable attribute-based dynamic memberships for groups. For example, if a user’s department is Sales, that user will be dynamically assigned to the Sales group.
    
    You can use security groups for either devices or users, but you can only use Microsoft 365 Groups for user groups. If the user's department changes in the future, they're automatically removed from the group. This feature requires a Microsoft Entra ID P1 license.
    
3. **Dynamic device**. You can create rules based on characteristics to enable attribute-based dynamic memberships for groups. For example, if a user’s device is associated with the Service department, that device will be dynamically assigned to the Service group.
    
    You can use security groups for either devices or users, but you can only use Microsoft 365 Groups for user groups. If the device's association with a particular department changes in the future, it's automatically removed from the group. This feature requires a Microsoft Entra ID P1 license.
    

Finally, you can select group owner(s) that can administer the group and member(s) that will belong to the group. Both of these can contain other groups as well as individual users.

### Script group creation

You can also use Microsoft Graph PowerShell to add a group using the [New-MgGroup](https://learn.microsoft.com/en-us/powershell/module/microsoft.graph.groups/new-mggroup) command as shown here:

PowerShellCopy

```
New-MgGroup -Description "Marketing" -DisplayName "Marketing" -MailNickName "Marketing" -SecurityEnabled -MailEnabled:$False
```

## Change membership for a group

Once a group is created, you can add or remove users (or groups) from it by editing the group membership. Select the group and use the options in the **Manage** section.

![Screenshot showing the group-management options.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/4-edit-group-membership.png)


# Use roles to control resource access

## Built-in roles for Azure Resources (uses PowerShell)

Azure provides several _built-in roles_ to cover the most common security scenarios. To understand how the roles work, let's examine three roles that apply to all resource types:

- **Owner**: Has full access to all resources, including the right to delegate access to others.
- **Contributor**: Can create and manage all types of Azure resources, but can’t grant access to others.
- **Reader**: Can view existing Azure resources.

## Role definitions

Each role is a set of properties defined in a JavaScript Object Notation (JSON) file. This role definition includes a **Name**, **ID**, and **Description**. It also includes the allowable permissions (**Actions**), denied permissions (**NotActions**), and scope (for example, read access) for the role.

For the Owner role, that means all actions, indicated by an asterisk (*); no denied actions; and all scopes, indicated by a forward slash (/).

You can get this information using the PowerShell `Get-AzRoleDefinition Owner` cmdlet.

PowerShellCopy

```
Get-AzRoleDefinition Owner
```

This code should produce the following output:

OutputCopy

```
Name             : Owner
Id               : 8e3af657-a8ff-443c-a75c-2fe8c4bcb635
IsCustom         : False
Description      : Lets you manage everything, including access to resources.
Actions          : {*}
NotActions       : {}
DataActions      : {}
NotDataActions   : {}
AssignableScopes : {/}
```

Try the same for the **Contributor** and **Reader** roles to see the actions allowed and denied.

## Examine the built-in roles

Next, let's explore some of the other built-in roles.

1. Open the [Azure portal](https://portal.azure.com/).
    
2. On the Azure home page, select **Resource groups** in the left-hand menu.
    
3. Select a resource group. Your _Resource group_ pane appears.
    
4. In the left menu pane, select the **Access control (IAM)**. The **Access control (IAM)** pane appears for your resource group.
    
5. On the interior menu bar, select the **Roles** tab as follows to see the list of available roles.
    
    ![Screenshot showing the roles in the Azure portal.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/5-list-roles.png)
    

## What's a role definition?

A role definition is a collection of permissions. A role definition lists the operations the role can perform, such as **read, write, and delete**. It can also list the operations that can't be performed or operations related to underlying data.

As previously described, a role definition has the following structure:

Expand table

|Name|Description|
|---|---|
|`Id`|Unique identifier for the role, assigned by Azure|
|`IsCustom`|True if a custom role, False if a built-in role|
|`Description`|A readable description of the role|
|`Actions []`|Allowed permissions; `*` indicates all|
|`NotActions []`|Denied permissions|
|`DataActions []`|Specific allowed permissions as applied to data, for example `Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read`|
|`NotDataActions []`|Specific denied permissions as applied to data.|
|`AssignableScopes []`|Scopes where this role applies; `/` indicates global, but can reach into a hierarchical tree|

This structure is represented as JSON when used in role-based access control (RBAC) or from the underlying API. For example, here's the **Contributor** role definition in JSON format.

JSONCopy

```
{
  "Name": "Contributor",
  "Id": "b24988ac-6180-42a0-ab88-20f7382dd24c",
  "IsCustom": false,
  "Description": "Lets you manage everything except access to resources.",
  "Actions": [
    "*"
  ],
  "NotActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/"
  ]
}
```

### Actions and NotActions

You can tailor the `Actions` and `NotActions` properties to grant and deny the exact permissions you need. These properties are always in the format: `{Company}.{ProviderName}/{resourceType}/{action}`.

As an example, here are the actions for the three roles we looked at previously:

Expand table

|Built-in Role|Actions|NotActions|
|---|---|---|
|Owner (allow all actions)|`*`|-|
|Contributor (allow all actions except writing or deleting role assignments)|`*`|`Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write, Microsoft.Authorization/elevateAccess/Action`|
|Reader (allow all read actions)|`*/read`|-|

The wildcard (`*`) operation under `Actions` indicates that the principal assigned to this role can perform all actions; or in other words, this role can manage everything, including actions defined in the future, as Azure adds new resource types. With the **Reader** role, only the `read` action is allowed.

The operations under `NotActions` are subtracted from `Actions`. With the **Contributor** role, `NotActions` removes this role's ability to manage access to resources, and also removes assigning access to resources.

### DataActions and NotDataActions

Data operations are specified in the `DataActions` and `NotDataActions` properties. You can specify data operations separately from the management operations. This prevents current role assignments with wildcards (`*`) from suddenly having access to data. Here are some data operations that you can specify in `DataActions` and `NotDataActions`:

- Read a list of blobs in a container
- Write a storage blob in a container
- Delete a message in a queue

You can only add data operations to the `DataActions` and `NotDataActions` properties. Resource providers identify which operations are data operations by setting the `isDataAction` property to `true`. Roles that don't have data operations can omit these properties from the role definition.

These actions work exactly like their management cousins. You can specify the actions you want to allow (or `*` for all), then provide a list of specific actions to remove in the `NotDataActions` collection. Here are some examples; you can find the [full list of actions and data actions in the resource provider documentation](https://learn.microsoft.com/en-us/azure/role-based-access-control/resource-provider-operations):

Expand table

|Data operation|Description|
|---|---|
|`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete`|Delete blob data|
|`Microsoft.Compute/virtualMachines/login/action`|Log in to a VM as a regular user|
|`Microsoft.EventHub/namespaces/messages/send/action`|Send messages on an event hub|
|`Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read`|Return a file/folder or list of files/folders|
|`Microsoft.Storage/storageAccounts/queueServices/queues/messages/read`|Read a message from a queue|

### Assignable Scopes

Defining the **Actions** and **NotActions** properties is not enough to fully implement a role. You also need to properly scope your role.

The **AssignableScopes** property of the role specifies the scopes (subscriptions, resource groups, or resources) within which the role is available for assignment. You can make the custom role available for assignment just in the subscriptions or resource groups that need it, thus avoiding cluttering the user experience for the rest of the subscriptions or resource groups.

Here are some examples:

Expand table

|To|Use Scope|
|---|---|
|Restrict to a subscription|`"/subscriptions/{sub-id}"`|
|Restrict to a specific resource group on a specific subscription|`"/subscriptions/{sub-id}/resourceGroups/{rg-name}"`|
|Restrict to a specific resource|`"/subscriptions/{sub-id}/resourceGroups/{rg-name}/{resource-name}"`|
|Make a role available for assignment in two subscriptions|`"/subscriptions/{sub-id}", "/subscriptions/{sub-id}"`|

## Create roles

Microsoft Entra ID comes with built-in roles that are likely to cover 99% of what you'll ever want to do. It's preferable to use a built-in role if possible. However, you can create custom roles if you find it necessary.

 Note

Custom role creation requires Microsoft Entra ID P1 or P2; you can't create custom roles in the free tier.

You can create a new role through several mechanisms:

- **Azure portal**: You can use the Azure portal to create a custom role by selecting **Microsoft Entra ID** > **Roles and administrators** > **New custom role**.
    
- **Azure PowerShell**: You can use the `New-AzRoleDefinition` cmdlet to define a new role.
    
- **Azure Graph API**: You can use a REST call to the Graph API to programmatically create a new role.
    

This module's Summary section includes a link to the documentation for all three approaches.


# Connect Active Directory to Microsoft Entra ID with Microsoft Entra Connect


Companies that use an on-premises Windows Server Active Directory solution can integrate their existing users and groups with Microsoft Entra ID with **Microsoft Entra Connect**. This is a free tool you can download and install to synchronize your local AD with your Azure directory.

With Microsoft Entra Connect, you can provide your users with a common identity for Microsoft 365, Azure, and SaaS applications integrated with Microsoft Entra ID in a hybrid identity environment.

![A diagram of Microsoft Entra Connect synchronizing an on-premises Active Directory with Microsoft Entra ID.](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/media/m1-aad-connect1.png)

## What's included in Microsoft Entra Connect?

Microsoft Entra Connect provides several components that you can install to create a hybrid identity system.

- **Sync services**: This component is responsible for creating users, groups, and other objects. It also makes sure that identity information for your on-premises users and groups matches that in the cloud.
- **Health monitoring**: Microsoft Entra Connect Health supplies robust monitoring and a central location in the Azure portal for viewing this activity.
- **AD FS**: Federation is an optional part of Microsoft Entra Connect that you can use to configure a hybrid environment via an on-premises AD FS infrastructure. Organizations can use this to address complex deployments such as domain join SSO, enforcement of the Active Directory sign-in policy, and smart card or third-party multifactor authentication.
- **Password hash synchronization**: This feature is a sign-in method that synchronizes a hash of a user’s on-premises Active Directory password with Microsoft Entra ID.
- **Pass-through authentication**: This allows users to sign in to both on-premises and cloud-based applications using the same passwords. This reduces IT helpdesk costs, because users are less likely to forget how to sign in. This feature provides an alternative to **Password hash synchronization** that allows organizations to enforce their security and password complexity policies.

## Benefits

Integrating your on-premises directories with Microsoft Entra ID makes your users more productive by supplying a common identity for accessing both cloud and on-premises resources. Users and organizations get the following advantages:

- Users can use a single identity to access both on-premises applications and cloud services such as Microsoft 365.
- A single tool provides an easy deployment experience for synchronization and sign-in.
- Integration provides the newest capabilities for your scenarios. Microsoft Entra Connect replaces older versions of identity integration tools such as DirSync and Azure AD Sync.

## Installing

Installing and configuring Microsoft Entra Connect is not a trivial task, and requires some initial planning and decisions before you begin. Microsoft has a full [Installation guide](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-install-roadmap) to help you prepare, install, configure and test your AD Connect setup.

# Summary


Microsoft Entra ID is the hub for user management in the cloud. It provides a single-sign on capability that makes it easier to onboard new employees and ensure you remove access from previous employees. It integrates tightly with your on-premises Windows Server Active Directory so you have a seamless login experience across all your local and cloud-based resources, and it integrates with other cloud environments like Salesforce, Microsoft 365, Dropbox, or Google. Finally, it can reduce your IT burden through its centralized management portal, support for self-service password resets, multifactor authentication for increased security, high reliability, and support for managing users and groups with roles.

## Further reading

To learn more about some of the topics explored in this module, check out the following references.

- [Azure built-in roles](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles)
- [Secure your Azure resources with Conditional Access](https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview)
- [Create custom roles for Azure resources](https://learn.microsoft.com/en-us/azure/role-based-access-control/custom-roles)
- [Hybrid identity with Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/hybrid/)
- [Microsoft Entra Connect](https://www.microsoft.com/download/details.aspx?id=47594)
- [Transfer billing ownership of an Azure subscription to another account](https://learn.microsoft.com/en-us/azure/billing/billing-subscription-transfer)

---
# Secure your Azure resources with Azure role-based access control (Azure RBAC)

# Introduction

For any organization using the cloud, securing your Azure resources such as virtual machines, websites, networks, and storage is a critical function. You want to ensure that your data and assets are protected, but still grant your employees and partners the access they need to perform their jobs. Azure RBAC is an authorization system in Azure that helps you manage who has access to Azure resources, what they can do with those resources, and where they have access.

Suppose you work for First Up Consultants, which is an engineering firm that specializes in circuit and electrical design. They've moved their workloads and assets to Azure to make collaboration easier across several offices and other companies. You work in the IT department at First Up Consultants. You're responsible for keeping the company's assets secure, but still allowing users to access the resources they need. You've heard that Azure RBAC can help you manage resources in Azure.

In this module, you'll learn how to use Azure RBAC to manage access to resources in Azure.

## Learning objectives

In this module, you'll:

- Verify access to resources for yourself and others.
- Grant access to resources.
- View activity logs of Azure RBAC changes.

# What is Azure RBAC?

When it comes to identity and access, most organizations that are considering using the public cloud are concerned about two things:

1. Ensuring that when people leave the organization, they lose access to resources in the cloud.
2. Striking the right balance between autonomy and central governance. For example, giving project teams the ability to create and manage virtual machines in the cloud, while centrally controlling the networks those VMs use to communicate with other resources.

Microsoft Entra ID and Azure RBAC work together to make it simple to carry out these goals.

## Azure subscriptions

First, remember that each Azure subscription is associated with a single Microsoft Entra directory. Users, groups, and applications in that directory can manage resources in the Azure subscription. The subscriptions use Microsoft Entra ID for single sign-on (SSO) and access management. You can extend your on-premises Active Directory to the cloud by using **Microsoft Entra Connect**. This feature allows your employees to manage their Azure subscriptions by using their existing work identities. When you disable an on-premises Active Directory account, it automatically loses access to all Azure subscriptions connected with Microsoft Entra ID.

## What's Azure RBAC?

Azure RBAC is an authorization system built on Azure Resource Manager that provides fine-grained access management for resources in Azure. With Azure RBAC, you can grant the exact access that users need to do their jobs. For example, you can use Azure RBAC to let one employee manage virtual machines in a subscription, while another manages SQL databases within the same subscription.

The following video describes Azure RBAC in detail:

![](https://www.microsoft.com/en-us/videoplayer/embed/RE2yEvk?postJsllMsg=true)

You can grant access by assigning the appropriate Azure role to users, groups, and applications at a certain scope. The scope of a role assignment can be a management group, subscription, a resource group, or a single resource. A role assigned at a parent scope also grants access to the child scopes contained within it. For example, a user with access to a resource group can manage all the resources it contains like websites, virtual machines, and subnets. The Azure role that you assign dictates what resources the user, group, or application can manage within that scope.

The following diagram depicts how the classic subscription administrator roles, Azure roles, and Microsoft Entra roles are related at a high level. Child scopes, such as service instances, inherit roles assigned at a higher scope, like an entire subscription.

![Diagram that depicts how the classic subscription administrator roles, Azure roles, and Microsoft Entra roles are related at a high level.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/2-azuread-and-azure-roles.png)

In the preceding diagram, a subscription is associated with only one Microsoft Entra tenant. Also note that a resource group can have multiple resources, but it's associated with only one subscription. Although it's not obvious from the diagram, a resource can be bound to only one resource group.

## What can I do with Azure RBAC?

Azure RBAC allows you to grant access to Azure resources that you control. Suppose you need to manage access to resources in Azure for the development, engineering, and marketing teams. You’ve started to receive access requests, and you need to quickly learn how access management works for Azure resources.

Here are some scenarios you can implement with Azure RBAC:

- Allow one user to manage virtual machines in a subscription and another user to manage virtual networks.
- Allow a database administrator group to manage SQL databases in a subscription.
- Allow a user to manage all resources in a resource group, such as virtual machines, websites, and subnets.
- Allow an application to access all resources in a resource group.

## Azure RBAC in the Azure portal

In several areas in the Azure portal, you'll see a pane named **Access control (IAM)**, also known as _identity and access management_. On this pane, you can see who has access to that area and their role. Using this same pane, you can grant or remove access.

The following shows an example of the Access control (IAM) pane for a resource group. In this example, Alain has been assigned the Backup Operator role for this resource group.

![Screenshot of the Azure portal showing the Access control Role assignment pane with the Backup operator section highlighted.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/2-resource-group-access-control.png)

## How does Azure RBAC work?

You can control access to resources using Azure RBAC by creating role assignments, which control how permissions are enforced. To create a role assignment, you need three elements: a security principal, a role definition, and a scope. You can think of these elements as _who_, _what_, and _where_.

### 1. Security principal (who)

A _security principal_ is just a fancy name for a user, group, or application to which you want to grant access.

![An illustration showing security principal including user, group, and service principal.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/2-rbac-security-principal.png)

### 2. Role definition (what)

A _role definition_ is a collection of permissions. It's sometimes just called a role. A role definition lists the permissions the role can perform such as read, write, and delete. Roles can be high-level, like Owner, or specific, like Virtual Machine Contributor.

![An illustration listing different built-in and custom roles with zoom-in on the definition for the contributor role.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/2-rbac-role-definition.png)

Azure includes several built-in roles that you can use. The following lists four fundamental built-in roles:

- **Owner**: Has full access to all resources, including the right to delegate access to others.
- **Contributor**: Can create and manage all types of Azure resources, but can’t grant access to others.
- **Reader**: Can view existing Azure resources.
- **User Access Administrator**: Lets you manage user access to Azure resources.

If the built-in roles don't meet the specific needs of your organization, you can create your own custom roles.

### 3. Scope (where)

_Scope_ is the level where the access applies. This is helpful if you want to make someone a Website Contributor but only for one resource group.

In Azure, you can specify a scope at multiple levels: management group, subscription, resource group, or resource. Scopes are structured in a parent-child relationship. When you grant access at a parent scope, the child scopes automatically inherit those permissions. For example, if a group is assigned the Contributor role at the subscription scope, it will inherit the role for all resource groups and resources within the subscription.

![An illustration showing a hierarchical representation of different Azure levels to apply scope. The hierarchy, starting with the highest level, is in this order: Management group, subscription, resource group, and resource.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/2-rbac-scope.png)

### Role assignment

Once you have determined the who, what, and where, you can combine those elements to grant access. A _role assignment_ is the process of binding a role to a security principal at a particular scope for the purpose of granting access. To grant access, you'll create a role assignment. To revoke access, you'll remove a role assignment.

The following example shows how the Marketing group has been assigned the Contributor role at the sales resource group scope.

![An illustration showing a sample role assignment process for Marketing group, which is a combination of security principal, role definition, and scope. The Marketing group falls under the Group security principal and has a Contributor role assigned for the Resource group scope.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/2-rbac-overview.png)

## Azure RBAC is an allow model

Azure RBAC is an _allow_ model. This means that when you're assigned a role, Azure RBAC allows you to perform certain actions such as read, write, or delete. If one role assignment grants you read permissions to a resource group, and a different role assignment grants you write permissions to the same resource group, then you'll have read and write permissions on that resource group.

Azure RBAC has something called `NotActions` permissions. You can use `NotActions` to create a set of not allowed permissions. The access a role grants—the _effective permissions_—is computed by subtracting the `NotActions` operations from the `Actions` operations. For example, the [Contributor](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#contributor) role has both `Actions` and `NotActions`. The wildcard (*) in `Actions` indicates that it can perform all operations on the control plane. You'd then subtract the following operations in `NotActions` to compute the effective permissions:

- Delete roles and role assignments
- Create roles and role assignments
- Grant the caller User Access Administrator access at the tenant scope
- Create or update any blueprint artifacts
- Delete any blueprint artifacts

# Exercise - List access using Azure RBAC and the Azure portal

At First Up Consultants, you've been granted access to a resource group for the marketing team. You want to familiarize yourself with the Azure portal and see what roles are currently assigned.

You need an Azure subscription to complete the exercises. If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) and add a subscription before you begin. If you're a student, you can take advantage of the [Azure for students](https://azure.microsoft.com/free/students/) offer.

## List role assignments for yourself

Follow these steps to see what roles are currently assigned to you.

1. Sign in to the [Azure portal](https://portal.azure.com/).
    
2. On the **Profile** menu, select the ellipsis (**...**) to see more links.
    
    ![Screenshot of user menu with My permissions highlighted.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/4-my-permissions-menu.png)
    
3. Select **My permissions** to open the **My permissions** pane.
    
    ![Screenshot of the My permissions pane.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/4-my-permissions-pane.png)
    
    You'll find the roles that you've been assigned and the scope. Your list will look different.
    

## List role assignments for a resource group

Follow these steps to see what roles are assigned at the resource group scope.

1. In the Search box at the top, search for and select **Resource groups**.
    
    ![Screenshot of the Azure portal that shows how to search for resource groups.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/4-resource-groups.png)
    
2. In the list of resource groups, select a resource group.
    
    These steps use a resource group named **example-group**, but your resource group's name will be different.
    
3. On the left menu pane, select **Access control (IAM)**.
    
    ![Screenshot showing Access control (IAM) option on the resource group pane.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/4-resource-group-access-control.png)
    
4. Select the **Role assignments** tab.
    
    This tab shows who has access to the resource group. Notice that some roles are scoped to **This resource**, while others are **(Inherited)** from a parent scope.
    
    ![Screenshot showing Role assignments tab for the selected resource group.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/4-resource-group-role-assignment.png)
    

## List roles

As you learned in the previous unit, a role is a collection of permissions. Azure has more than 70 built-in roles that you can use in your role assignments. To list the roles:

- In the menu bar at the top of the pane, select the **Roles** tab to list of all the built-in and custom roles.
    
    Select a role's **View** link in the **Details** column, then select the **Assignments** tab to display the number of users and groups assigned to that role.
    
    ![Screenshot showing a list of Roles and users and groups assigned to each role.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/4-roles-list.png)
    

In this unit, you learned how to list the role assignments for yourself in the Azure portal. You also learned how to list the role assignments for a resource group.
# Exercise - Grant access using Azure RBAC and the Azure portal


A coworker named Alain at First Up Consultants needs permission to create and manage virtual machines for a project on which they're working. Your manager has asked that you handle this request. Using the best practice to grant users the least privileges to get their work done, you decide to assign Alain the Virtual Machine Contributor role for a resource group.

## Grant access

Follow this procedure to assign the Virtual Machine Contributor role to a user at the resource group scope.

1. Sign in to the [Azure portal](https://portal.azure.com/) as an administrator that has permissions to assign roles, such as [User Access Administrator](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#user-access-administrator) or [Owner](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#owner).
    
2. In the Search box at the top, search for **Resource groups**.
    
    ![Screenshot of the Azure portal that shows how to search for resource groups.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/5-resource-groups.png)
    
3. In the list of resource groups, select a resource group.
    
    These steps use a resource group named **example-group**, but your resource group's name will be different.
    
4. On the left menu pane, select **Access control (IAM)**.
    
5. Select the **Role assignments** tab to display the current list of role assignments at this scope.
    
    ![Screenshot showing Role assignments tab for the selected resource group.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/5-resource-group-role-assignment.png)
    
6. Select **Add** > **Add role assignment**.
    
    If you don't have permissions to assign roles, the **Add role assignment** option will be disabled.
    
    ![Screenshot that shows Add role assignment menu.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/5-resource-group-add-role-assignment.png)
    
    The **Add role assignment** page opens.
    
7. On the **Role** tab, search for and select **Virtual Machine Contributor**.
    
    ![Screenshot that shows Add role assignment and list of roles.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/5-select-role.png)
    
8. Select **Next**.
    
9. On the **Members** tab, select **Select members**.
    
10. Search for and select a user.
    
    ![Screenshot of the add role assignment page that shows the select members option.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/5-select-members-option.png)
    
11. Select **Select** to add the user to the Members list.
    
12. Select **Next**.
    
13. On the **Review + assign** tab, review the role assignment settings.
    
14. Select **Review + assign** to assign the role.
    
    After a few moments, the user is assigned the Virtual Machine Contributor role at the resource group scope. The user can now create and manage virtual machines just within this resource group.
    
    ![Screenshot that shows the Virtual Machine Contributor role assigned to a user.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/5-vm-contributor-assignment.png)
    

## Remove access

In Azure RBAC, you can remove a role assignment to remove access.

1. In the list of role assignments, select **View Assignments**.
    
2. Search for and check the box for the user with the Virtual Machine Contributor role.
    
3. Select **Remove**.
    
    ![Screenshot that shows the Remove role assignment message.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/5-remove-role-assignment.png)
    
4. In the **Remove role assignments** message that appears, select **Yes**.
    

In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using the Azure portal.

# Exercise - View activity logs for Azure RBAC changes

First Up Consultants reviews Azure RBAC changes quarterly for auditing and troubleshooting purposes. You know that changes get logged in the [Azure Activity Log](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/activity-log). Your manager has asked if you can generate a report of the role assignment and custom role changes for the last month.

## View activity logs

The easiest way to get started is to view the activity logs with the Azure portal.

1. Select **All services**, then search for **Activity log**.
    
    ![Screenshot of the Azure portal showing the location of Activity logs option.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/6-all-services-activity-log.png)
    
2. Select **Activity log** to open the activity log.
    
    ![Screenshot of the Azure portal showing the Activity logs.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/6-activity-log-portal.png)
    
3. Set the **Timespan** filter to **Last month**.
    
4. Add an **Operation** filter and type **role** to filter the list.
    
5. Select the following Azure RBAC operations:
    
    - Create role assignment (roleAssignments)
    - Delete role assignment (roleAssignments)
    - Create or update custom role definition (roleDefinitions)
    - Delete custom role definition (roleDefinitions)
    
    ![Screenshot showing a list of Operation filter with the four filters selected.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/6-operation-filter.png)
    
    After a moment, you'll get a list of all the role assignment and role definition operations for the last month. There's also a button at the top of the screen to download the activity log as a CSV file.
    
6. Select one of the operations to get the activity log details.
    
    ![Screenshot showing the details for an activity log.](https://learn.microsoft.com/en-us/training/modules/secure-azure-resources-with-rbac/media/6-activity-log-details.png)
    

In this unit, you learned how to use Azure Activity Log to list Azure RBAC changes in the portal and generate a simple report.

---

# Allow users to reset their password with Microsoft Entra self-service password reset


# Introduction

Suppose you're an IT administrator for a large retail organization. Your organization has started using Microsoft Entra ID to allow employees to securely sign in and use software as a service (SaaS) apps and access the organization's resources in Microsoft 365. You're overwhelmed with password-reset requests because you currently reset employees' passwords manually. To get these employees back to being productive quickly and reduce your workload, you decide to evaluate and set up self-service password reset in Microsoft Entra ID.

In this module, you'll learn how Azure supports this feature and how to set it up. Note that only paid subscriptions can leverage this, while free and pay-as-you-go can't.

By the end of this module, you'll be able to configure self-service password reset in Microsoft Entra ID.

## Learning objectives

In this module, you'll:

- Decide whether to implement self-service password reset.
- Implement self-service password reset to meet your requirements.
- Configure self-service password reset to customize the experience.

## Prerequisites

- Basic understanding of Microsoft Entra ID

# What is self-service password reset in Microsoft Entra ID?

You've been asked to assess ways to reduce help-desk costs in your retail organization. You've noticed that the support staff spends a lot of their time resetting passwords for users. Users often complain about delays with this process, and these delays impact their productivity. You want to understand how you can configure Azure to allow users to manage their own passwords.

In this unit, you'll learn how self-service password reset (SSPR) works in Microsoft Entra ID.

## Why use SSPR?

In Microsoft Entra ID, any user can change their password if they're already signed in. But if they're not signed in, forgot their password, or it's expired, they'll need to reset their password. With SSPR, users can reset their passwords in a web browser or from a Windows sign-in screen to regain access to Azure, Microsoft 365, and any other application that uses Microsoft Entra ID for authentication.

SSPR reduces the load on administrators because users can fix password problems themselves without having to call the help desk. Also, it minimizes the productivity impact of a forgotten or expired password. Users don't have to wait until an administrator is available to reset their password.

## How SSPR works

The user initiates a password reset either by going directly to the password-reset portal, or by selecting the **Can't access your account** link on a sign-in page. The reset portal takes these steps:

1. **Localization**: The portal checks the browser's locale setting and renders the SSPR page in the appropriate language.
2. **Verification**: The user enters their username and passes a CAPTCHA to ensure that it's a user and not a bot.
3. **Authentication**: The user enters the required data to authenticate their identity. They might enter a code or answer security questions.
4. **Password reset**: If the user passes the authentication tests, they can enter a new password and confirm it.
5. **Notification**: A message is sent to the user to confirm the reset.

There are several ways you can customize the SSPR user experience. For example, you can add your company logo to the sign-in page so users know they're in the right place to reset their password.

## Authenticate a password reset

It's critical to verify a user's identity before you allow a password reset. Malicious users might exploit any weakness in the system to impersonate that user. Azure supports six different ways to authenticate reset requests.

As an administrator, you can choose the methods to use when you configure SSPR. Enable two or more of these methods so that users can choose the ones they can easily use. The methods are:

Expand table

|Authentication method|How to register|How to authenticate for a password reset|
|---|---|---|
|Mobile app notification|Install the Microsoft Authenticator app on your mobile device, then register it on the multifactor authentication setup page.|Azure sends a notification to the app, which you can either verify or deny.|
|Mobile app code|This method also uses the Authenticator app, and you install and register it in the same way.|Enter the code from the app.|
|Email|Provide an email address that's external to Azure and Microsoft 365.|Azure sends a code to the address, which you enter in the reset wizard.|
|Mobile phone|Provide a mobile phone number.|Azure sends a code to the phone in an SMS message, which you enter in the reset wizard. You can also choose to get an automated call.|
|Office phone|Provide a nonmobile phone number.|You receive an automated call to this number and press #.|
|Security questions|Select questions such as "In what city was your mother born?" and save their responses.|Answer the questions.|

In free and trial Microsoft Entra organizations, phone call options aren't supported.

### Require the minimum number of authentication methods

You can specify the minimum number of methods that the user must set up, either one or two. For example, you might enable the mobile app code, email, office phone, and security questions methods and specify a minimum of two methods. Users can then choose the two methods they prefer, like mobile app code and email.

For the security-question method, you can specify a minimum number of questions the user must set up to register for this method. You also can specify a minimum number of questions they must answer correctly to reset their password.

After your users register the required information for the minimum number of methods you've specified, they're considered registered for SSPR.

### Recommendations

- Enable two or more of the authentication reset request methods.
- Use the mobile app notification or code as the primary method. But also enable the email or office phone methods to support users without mobile devices.
- The mobile phone method isn't a recommended method, because it's possible to send fraudulent SMS messages.
- The security-question option is the least recommended method, because the answers to the security questions might be known to other people. Only use the security-question method in combination with at least one other method.

### Accounts associated with administrator roles

- A strong, two-method authentication policy is always applied to accounts with an administrator role, regardless of your configuration for other users.
- The security-question method isn't available to accounts associated with an administrator role.

## Configure notifications

Administrators can choose how users are notified of password changes. There are two options you can enable:

- **Notify users on password resets**: The user who resets their own password is notified to their primary and secondary email addresses. If the reset was done by a malicious user, this notification alerts the user, who can take mitigation steps.
- **Notify all admins when other admins reset their password**: All administrators are notified when another administrator resets their password.

## License requirements

There are three editions of Microsoft Entra ID: free, Premium P1, and Premium P2. The password-reset functionality you can use depends on your edition.

Any user who is signed in can change their password, regardless of the edition of Microsoft Entra ID.

What if you're not signed in, and you've forgotten your password or your password has expired? In this case, you can use SSPR in Microsoft Entra ID P1 or P2. It's also available with Microsoft 365 Apps for business or Microsoft 365.

In a hybrid situation, where you have Active Directory on-premises and Microsoft Entra ID in the cloud, any password change in the cloud must be written back to the on-premises directory. This writeback support is available in Microsoft Entra ID P1 or P2. It's also available with Microsoft 365 Apps for business.

## SSPR deployment options

You can deploy SSPR with password writeback by using [Microsoft Entra Connect](https://learn.microsoft.com/en-us/azure/active-directory/authentication/tutorial-enable-sspr-writeback) or [cloud sync](https://learn.microsoft.com/en-us/azure/active-directory/authentication/tutorial-enable-cloud-sync-sspr-writeback), depending on user needs. You can deploy each option side-by-side in different domains to target different sets of users. This helps existing users on-premises to write back password changes, while adding an option for users in disconnected domains because of a company merger or split. Users from an existing on-premises domain can use Microsoft Entra Connect, while new users from a merger can use cloud sync in another domain.

Cloud sync can also provide higher availability, because it doesn't rely on a single instance of Microsoft Entra Connect. For a feature comparison between the two deployment options, see [Comparison between Microsoft Entra Connect and cloud sync](https://learn.microsoft.com/en-us/azure/active-directory/cloud-sync/what-is-cloud-sync#how-is-azure-ad-connect-cloud-sync-different-from-azure-ad-connect-sync).


# Implement Microsoft Entra self-service password reset

You've decided to implement self-service password reset (SSPR) in Microsoft Entra ID for your organization. You want to start using SSPR for a group of 20 users in the marketing department as a trial deployment. If everything works well, you'll enable SSPR for your whole organization.

In this unit, you'll learn how to enable SSPR in Microsoft Entra ID.

## Prerequisites

Before you start to configure SSPR, you need a:

- **Microsoft Entra organization**: This organization must have at least a trial license enabled.
- **Microsoft Entra account with Global Administrator privileges**: You'll use this account to set up SSPR.
- **Non-administrative user account**: You'll use this account to test SSPR. It's important that this account isn't an administrator, because Microsoft Entra imposes extra requirements on administrative accounts for SSPR. This user, and all user accounts, must have a valid license to use SSPR.
- **Security group with which to test the configuration**: The non-administrative user account must be a member of this group. You'll use this security group to limit who you roll SSPR out to.

If you don't already have a Microsoft Entra organization that you can use for this module, we'll set one up in the next unit.

## Scope of SSPR rollout

There are three settings for the **Self-service password reset enabled** property:

- **None**: No users in the Microsoft Entra organization can use SSPR. This value is the default.
- **Selected**: Only the members of the specified security group can use SSPR. You can use this option to enable SSPR for a targeted group of users who can test it and verify that it works as expected. When you're ready to roll it out broadly, set the property to **Enabled** so that all users have access to SSPR.
- **All**: All users in the Microsoft Entra organization can use SSPR.

## Configure SSPR

** Here are the high-level steps to configure SSPR:

1. Go to the [Azure portal](https://portal.azure.com/), then to **Microsoft Entra ID** > **Manage** > **Password reset**.
    
2. **Properties**:
    
    - Enable SSPR.
    - You can enable it for all users in the Microsoft Entra organization or for selected users.
    - To enable for selected users, you must specify the security group. Members of this group can use SSPR.
    
    ![Screenshot of the Password Reset configuration panel. Properties option is selected allowing user to enable self service password resets.](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/media/3-enable-sspr.png)
    
3. **Authentication methods**:
    
    - Choose whether to require one or two authentication methods.
    - Choose the authentication methods that the users can use.
    
    ![Screenshot of the Password Reset panel's Authentication methods option selected displaying panel with authentication options.](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/media/3-auth-methods.png)
    
4. **Registration**:
    
    - Specify whether users are required to register for SSPR when they next sign in.
    - Specify how often users are asked to reconfirm their authentication information.
    
    ![Screenshot of the Password Reset panel's Registration option selected displaying panel with registration options.](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/media/3-registration-options.png)
    
5. **Notifications**: Choose whether to notify users and administrators of password resets.
    
    ![Screenshot of the Password Reset panel's Notification option selected displaying panel with notification options.](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/media/3-notification-settings.png)
    
6. **Customization**: Provide an email address or web page URL where your users can get help.
    
    ![Screenshot of the Password Reset panel's Customization option selected displaying panel with helpdesk options.](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/media/3-customization-settings.png)

# Exercise - Set up self-service password reset

In this unit, you'll configure and test self-service password reset (SSPR) by using your mobile phone. You'll need to use your mobile phone to complete the password-reset process in this exercise.

## Create a Microsoft Entra organization

For this step, you'll want to create a new directory and sign up for trial Premium subscription for Microsoft Entra ID.

1. Sign in to the [Azure portal](https://portal.azure.com/).
    
2. Select **Create a resource** > **Identity** > **Microsoft Entra ID**.
    
    ![Screenshot that shows Microsoft Entra ID in the Azure Marketplace.](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/media/4-create-active-directory.png)
    
3. Select **Microsoft Entra ID**, then select **Next : Configuration**.
    
4. On the **Create tenant** page, use the following values. Then select **Review + Create**, followed by **Create**.
    
    Expand table
	    
| Property            | Value                                                                                                 |
| ------------------- | ----------------------------------------------------------------------------------------------------- |
| Organization name   | Choose any organization name.                                                                         |
| Initial domain name | Choose a domain name that's unique within **.onmicrosoft.com**. Make a note of the domain you choose. |
| Country or region   | United States.                                                                                        |
    
5. Complete the CAPTCHA, then select **Submit**.
    
6. After you create the organization, select the F5 key to refresh the page. Select your user account and then select **Switch directory**.
    
7. Select the organization you just created.
    

## Create a Microsoft Entra ID P2 trial subscription

Now activate a trial Premium subscription for the organization so that you can test SSPR.

1. Go to **Microsoft Entra ID** > **Manage** > **Password reset**.
2. Select **Get a free Premium trial to use this feature**.
3. Under **Microsoft Entra ID P2**, expand **Free trial**, and select **Activate**.
4. Refresh the browser to see the **Password reset - Properties** page. You might need to refresh a few times.

## Create a group

You want to roll out SSPR to a limited set of users first to make sure your SSPR configuration works as expected. Let's begin by creating a security group for the limited rollout.

1. In the Microsoft Entra organization you created, under **Manage**, select **Groups**.
    
2. Select **New Group**.
    
3. Enter the following values:
    
| Setting           | Value                                   |
| ----------------- | --------------------------------------- |
| Group type        | Security                                |
| Group name        | SSPRTesters                             |
| Group description | Members are testing the rollout of SSPR |
| Membership type   | Assigned                                |
|                   |                                         |
    
4. Select **Create**.
    
    ![Screenshot that shows new group form filled out and the create button highlighted.](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/media/4-create-group.png)
    

## Create a user account

To test your configuration, create an account that's not associated with an administrator role. You'll also assign the account to the group you created.

1. In your Microsoft Entra organization, under **Manage**, select **Users**.
    
2. Select **+ New user**, select **Create new user** in the drop-down, and use the following values:
    
| Setting   | Value                                                                                                               |
| --------- | ------------------------------------------------------------------------------------------------------------------- |
| User name | balas                                                                                                               |
| Name      | Bala Sandhu                                                                                                         |
| Password  | Select the **Copy** icon next to the autogenerated password, then paste the password to a text editor like Notepad. |
    
3. Select the **Assignments** tab.
    
4. Select **Add group**, check the box for the **SSPRTesters** group, and then the **Select** button.
    
5. Select **Review + create** and then select **Create**.
    

## Enable SSPR

Now, you're ready to enable SSPR for the group.

1. In your Microsoft Entra organization, under **Manage**, select **Password reset**.
    
2. If the **Password reset** page still displays the message **Get a free Premium trial to use this feature**, wait for a few minutes and then refresh the page.
    
3. On the **Properties** page, select **Selected**. Select the **No groups selected** link, select the box next to the **SSPRTesters** group, and then the **Select** button.
    
4. Select **Save**.
    
    ![Screenshot of the Password Reset properties panel wwith SSPR enabled and selected group set to SSPRTesters.](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/media/4-choose-sspr-group.png)
    
5. Under **Manage**, select the **Authentication methods**, **Registration**, and **Notifications** pages to review the default values.
    
6. Select **Customization**.
    
7. Select **Yes**, and then in the **Custom helpdesk email or URL** text box, enter **admin@organization-domain-name.onmicrosoft.com**. Replace "organization-domain-name" with the domain name of the Microsoft Entra organization you created. If you've forgotten the domain name, hover over your profile in the Azure portal.
    
8. Select **Save**.
    

## Register for SSPR

Now that the SSPR configuration is complete, register a mobile phone number for the user you created.

 Note

If you get a message that says "The administrator has not enabled this feature," use private/incognito mode in your web browser.

1. In a new browser window, go to [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup).
    
2. Sign in with the user name **balas@organization-domain-name.onmicrosoft.com** and the password that you noted earlier. Remember to replace "organization-domain-name" with the domain name of the Microsoft Entra organization you created.
    
3. If you're asked to update your password, enter a new password of your choice. Make sure you note the new password.
    
4. Select the **Security info** tab, and then select **+ Add sign-in method**.
    
5. In the **Add a method** box, select **Phone**.
    
6. Enter your mobile phone details.
    
    ![Screenshot that shows mobile phone registration form for SSPR.](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/media/4-register-mobile-phone.png)
    
7. Select the **Text me a code** radio button, and then select **Next**.
    
8. When you receive the code on your mobile phone, enter the code in the text box and select **Next**.
    
9. Select **Done**.
    

## Test SSPR

Now, let's test whether the user can reset their password.

1. In a new browser window, go to [https://aka.ms/sspr](https://aka.ms/sspr).
    
2. For **User ID**, type **balas@organization-domain-name.onmicrosoft.com**. Replace "organization-domain-name" with the domain you used for your Microsoft Entra organization.
    
    ![Screenshot that shows the password reset dialog.](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/media/4-start-password-reset.png)
    
3. Complete the CAPTCHA and select **Next**.
    
4. Enter your mobile phone number, then select **Text**.
    
5. When the text arrives, in the **Enter your verification code** text box, enter the code you were sent. Select **Next**.
    
6. Enter a new password, and then select **Finish**. Make sure you note the new password.
    
7. Close the browser window.

# Exercise - Customize directory branding

Suppose you've been asked to display your retail organization's branding on the Azure sign-in page to reassure users that they're passing credentials to a legitimate system. Here, you'll learn how to configure this custom branding.

To complete this exercise, you must have two image files:

- A page background image. This must be a PNG or JPG file, 1920 x 1080 pixels, and smaller than 300 KB.
- A company logo image. This must be a PNG or JPG file, 32 x 32 pixels, and smaller than 5 KB.

## Customize Microsoft Entra organization branding

Let's use Microsoft Entra ID to set up the custom branding.

1. Sign in to the [Azure portal](https://portal.azure.com/).
    
2. Go to your Microsoft Entra organization by selecting **Microsoft Entra ID**. If you're not in the right Microsoft Entra organization, go to your Azure profile, and select **Switch directory** to find your organization.
    
3. Under **Manage**, select **Company branding**. Then select the **Customize** button in the center of the screen.
    
4. Next to **Favicon**, select **Browse**. Select your logo image.
    
5. Next to **Background image**, select **Browse**. Select your page background image.
    
6. Select a **Page background color** or accept the default.
    
    ![Screenshot that shows the configure company branding form.](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/media/5-customize-ui.png)
    
7. Select **Review + Create**, and then select **Create**.
    

## Test the organization's branding

Now, let's use the account that we created in the last exercise to test the branding.

1. In a new browser window, go to [https://login.microsoft.com](https://login.microsoft.com/).
    
2. Select the account for **Bala Sandhu**. Your custom branding is displayed.
    
    ![Screenshot that shows the customized sign-in page.](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/media/5-custom-login-page.png)
    
3. Select **Forgot my password**.
    
    ![Screenshot that shows organization logo on password reset page.](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/media/5-forgot-password-branding.png)

# Summary

In this module, you've learned how you can use SSPR in Microsoft Entra ID to allow users to reset their forgotten or expired passwords. An administrator doesn't have to do the password reset. SSPR is secured by authentication methods of your choice. These methods can include a mobile authentication app, a code sent to you by an SMS text message, or security questions.

SSPR helps reduce the amount of work required from administrators. It also minimizes the productivity impact for users when they forget their password.

## Clean up

Remember to clean up after you've finished.

- **Delete the user you created in Microsoft Entra ID**: Go to **Microsoft Entra ID** > **Manage** > **Users**. Check the box next to the user and select **Delete**. Select **OK**.
- **Delete the group you created in Microsoft Entra ID**: Go to **Microsoft Entra ID** > **Manage** > **Groups**. Check the box next to the group and select **Delete**. Select **OK**.
- **Turn off self-service password reset**: Go to **Microsoft Entra ID** > **Manage** > **Password reset**. Under **Self service password reset enabled**, select **None**. Select **Save**.

If you created a Premium trial Microsoft Entra tenant for this module, you can delete the tenant 30 days after the trial has expired.

## Learn more

- [Tutorial: Enable users to unlock their account or reset passwords using Microsoft Entra self-service password reset](https://learn.microsoft.com/en-us/entra/identity/authentication/tutorial-enable-sspr)
- [How it works: Microsoft Entra self-service password reset](https://learn.microsoft.com/en-us/entra/identity/authentication/concept-sspr-howitworks)
- [Enable self-service password reset](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-enable-password-reset-customers)

---

