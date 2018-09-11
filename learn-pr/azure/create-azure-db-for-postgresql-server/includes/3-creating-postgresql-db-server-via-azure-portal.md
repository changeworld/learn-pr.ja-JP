Let's assume that you're currently using an on-premise PostgreSQL relational database using flexible data types and geospatial support. Your company is looking at expanding, requiring the database to scale. As an alternative to investing in additional hardware, you are tasked with finding an optimal cloud-hosted database offering and you have decided to use an Azure Database for PostgreSQL server.

## What is an Azure Database for PostgreSQL server?

The PostgreSQL server is a central administration point for one or more databases. The PostgreSQL service in Azure is a managed resource that provides performance guarantees, and provides access and features at the server level.

An **Azure Database for PostgreSQL** server is the parent _resource_ for a database. A _resource_ is a manageable item that is available through Azure. Creating this resource allows you to configure your server instance.

### What is an Azure Database for PostgreSQL server resource?

An Azure Database for PostgreSQL server resource is a container with strong lifetime implications for your server and databases. If the server resource is deleted, all databases are also deleted. Keep in mind that all resources belonging to the parent are hosted in the same region.

The server resource name is used to define the server endpoint name. For example, if the resource name is **mypgsqlserver** then the server name becomes **mypgsqlserver.postgres.database.azure.com**

The server resource also provides the __connection scope__ for management policies that apply to its database. For example, login, firewall, users, roles, and configuration.

Just like the open-source version of PostgreSQL, the server is available in several versions and allows for the installation of extensions. You'll choose which server version to install.

> [!NOTE]
> Extensions allow for bundling multiple SQL objects together in a single package that can be loaded or removed with a single command. An example of >an extension is ```chkpass```, which provides a data type for auto-encrypted passwords.

## Pricing tiers

The Azure Database for PostgreSQL provides you with the option to choose from three pricing tiers based on parameters such as compute power and storage.

### Basic

The **Basic** tier is ideal for workloads that requires light compute and I/O performance. In this tier, you have access to the following hardware:

- Compute Gen 4 CPUs base on Intel E5-2673 v3 (Haswell) 2.4-GHz processors available as either a 1 or 2 vCore configuration
- Compute Gen 5 CPUs base on Intel E5-2673 v4 (Broadwell) 2.3-GHz processors available as either a 1 or 2 vCore configuration
- Storage up to 1 TB
- Locally redundant backup

You'll use a specific pricing tier setting to illustrate support of a specific scenario when you create an example server later. Keep in mind that for production servers, you'll choose a pricing tier to match your environment.

### General Purpose

The **General Purpose** tier is ideal for most business workloads requiring balanced compute and memory with scalable I/O throughput. In this tier, you have access to the following hardware:

- Compute Gen 4 CPUs base on Intel E5-2673 v3 (Haswell) 2.4-GHz processors available in 2, 4, 8, 18, 32 vCore configurations
- Compute Gen 5 CPUs base on Intel E5-2673 v4 (Broadwell) 2.3-GHz processors available in 2, 4, 8, 18, 32 vCore configurations
- Storage up to 4 TB
- Locally redundant backup
- Geographically redundant backup

### Memory Optimized

The **Memory Optimized** tier is ideal for high-performance database workloads requiring in-memory performance for faster transaction processing and higher concurrency. In this tier, you have access to the following hardware:

- Compute Gen 5 CPUs base on Intel E5-2673 v4 (Broadwell) 2.3-GHz processors available in 2, 4, 8, 16 vCore configurations
- Storage up to 4 TB
- Locally redundant backup
- Geographically redundant backup

## Steps to create an Azure Database for PostgreSQL server

You'll typically create an Azure Database for PostgreSQL server using the Azure portal. Let's look at the steps you'll take.

First you'll sign into the Azure portal, and then you'll click **Create a resource**.

You'll select **Databases** and **Azure Database for PostgreSQL**. You can also use the Search functionality to find this category.

The portal will display a PostgreSQL server configuration screen, also called a blade, you make your selection.

![Screenshot of the Azure portal showing the creation blade for a new PostgreSQL database.](../media-draft/4-create-blade.png)

You'll need to give a value to all the items in the blade, so let's have a look at each.

### Server name

Recall, earlier we mentioned that you'll create a server resource. The server name is the item that specifies this resource. As a result, you'll have to choose a unique name for the server. The server name must be all lowercase and can have numbers and hyphens.

Let's say you want to name the server _Adventure Works Tracking_. You would then set up the name as ```adventure-works-tracking```. If you try to create a server with the same name that already exists, you'll get an error to that effect.

### Subscription

The subscription field is used for billing. You'll pick a specific subscription in case you have more than one subscription available.

### Resource group

You'll use a resource group to manage all the resources related to your server. You can create a new resource group or reuse an existing resource group.

### Source

You can create a server either from scratch by choosing the _Blank_ default option or from an existing backup. The Backup option will give the opportunity restore a geo-backup of an existing Azure Database for PostgreSQL server.

### Server admin login name

You create the server admin user. Choose a login name to use as an administrator login for the new server. The admin login name can't be azure_superuser, azure_pg_admin, admin, administrator, root, guest, or public. It can't start with pg_. Remember or write down this name for future use.

### Password

You choose a password to use with the above administrator login name. Your password must include characters from three of the following categories: English uppercase letters, English lowercase letters, numbers (0 through 9), and non-alphanumeric characters (!, $, #, %, etc.). Remember or write down this password for future use.

### Confirm password

Retype the password as confirmation.

### Location

The location option allows you to specify where the server is created physically. Choose the geographical location closest to you. In a real-world scenario, the location should be the location closest to the majority of your users.

### Version

You can specify the version of PostgreSQL to use. Microsoft aims to support the current version and two previous versions of PostgreSQL. You'll choose a version that matches your production environment.

> [!NOTE]
> For more information, see the following sources:
> - [Supported PostgreSQL Database Versions](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions)
> - [Versioning Policy](https://www.postgresql.org/support/versioning/)

### Pricing tier

You'll choose a pricing tier that will support your server's workload. Recall, you have three tiers to choose from. As we saw earlier, each of these tiers allows you to configure the compute and storage options. For example, here is an example with the Basic price tier selected, a Gen 5 compute and a 25-day backup retention period.

![Screenshot of the Azure portal showing the database pricing tier for a new PostgreSQL database.](../media-draft/4-azure-db-pricing-tier.png)

All that is left now is to review the values you entered, make any notes you may need for later, and click the create button to create the server.

Deploying the server takes a couple of minutes. You'll receive a notification when the deployment is complete. From the notification, you can navigate to the newly created server.

You've now seen the steps you take to create an Azure Database for PostgreSQL. In the next unit, you'll have to opportunity to create your own Azure Database for PostgreSQL server.
