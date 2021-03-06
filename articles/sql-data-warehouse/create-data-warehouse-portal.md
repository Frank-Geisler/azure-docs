---
title: 'Quickstart: Create an Azure SQL data warehouse - Azure Portal | Microsoft Docs'
description: For Azure SQL Data Warehouse, create a SQL server, server-level firewall rule, and a data warehouse in the Azure portal. Then query it. 
keywords: sql data warehouse tutorial, create a SQL data warehouse
services: sql-database
documentationcenter: ''
author: barbkess
manager: jhubbard
editor: ''

ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: "Active"
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: quickstart
ms.date: 11/20/2017
ms.author: barbkess

---
# Quickstart: Create and query an Azure SQL data warehouse in the Azure portal

Quickly create and query an Azure SQL data warehouse using the Azure portal.

If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.

## Before you begin

Download and install the newest version of [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS).

## Sign in to the Azure portal

Sign in to the [Azure portal](https://portal.azure.com/).

## Create a data warehouse

An Azure SQL data warehouse is created with a defined set of [compute resources](performance-tiers.md). The database is created within an [Azure resource group](../azure-resource-manager/resource-group-overview.md) and in an [Azure SQL logical server](../sql-database/sql-database-features.md). 

Follow these steps to create a SQL data warehouse that contains the AdventureWorksDW sample data. 

1. Click the **New** button in the upper left-hand corner of the Azure portal.

2. Select **Databases** from the **New** page, and select **SQL Data Warehouse** under **Featured** on the **New** page.

    ![create empty data warehouse](media/load-data-from-azure-blob-storage-using-polybase/create-empty-data-warehouse.png)

3. Fill out the SQL Data Warehouse form with the following information:   

    | Setting | Suggested value | Description | 
    | ------- | --------------- | ----------- | 
    | **Database name** | mySampleDataWarehouse | For valid database names, see [Database Identifiers](/sql/relational-databases/databases/database-identifiers). Note, a data warehouse is a type of database.| 
    | **Subscription** | Your subscription  | For details about your subscriptions, see [Subscriptions](https://account.windowsazure.com/Subscriptions). |
    | **Resource group** | myResourceGroup | For valid resource group names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
    | **Select source** | Sample | Specifies to load a sample database. Note, a data warehouse is one type of database. |
    | **Select sample** | AdventureWorksDW | Specifies to load the AdventureWorksDW sample database.  |

    ![create data warehouse](media/create-data-warehouse-portal/select-sample.png)

4. Click **Server** to create and configure a new server for your new database. Fill out the **New server form** with the following information: 

    | Setting | Suggested value | Description | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | **Server name** | Any globally unique name | For valid server names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
    | **Server admin login** | Any valid name | For valid login names, see [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).|
    | **Password** | Any valid password | Your password must have at least eight characters and must contain characters from three of the following categories: upper case characters, lower case characters, numbers, and non-alphanumeric characters. |
    | **Location** | Any valid location | For information about regions, see [Azure Regions](https://azure.microsoft.com/regions/). |

    ![create database server](media/load-data-from-azure-blob-storage-using-polybase/create-database-server.png)

5. Click **Select**.

6. Click **Performance tier** to specify the performance configuration for the data warehouse.

7. For this tutorial, select the **Optimized for Elasticity** performance tier. The slider, by default, is set to **DW400**.  Try moving it up and down to see how it works. 

    ![configure performance](media/load-data-from-azure-blob-storage-using-polybase/configure-performance.png)

8. Click **Apply**.

9. Now that you have completed the SQL Database form, click **Create** to provision the database. Provisioning takes a few minutes. 

    ![click create](media/load-data-from-azure-blob-storage-using-polybase/click-create.png)

10. On the toolbar, click **Notifications** to monitor the deployment process.
    
     ![notification](media/load-data-from-azure-blob-storage-using-polybase/notification.png)

## Create a server-level firewall rule

The SQL Data Warehouse service creates a firewall at the server-level that prevents external applications and tools from connecting to the server or any databases on the server. To enable connectivity, you can add firewall rules that enable connectivity for specific IP addresses.  Follow these steps to create a [server-level firewall rule](../sql-database/sql-database-firewall-configure.md) for your client's IP address. 

> [!NOTE]
> SQL Data Warehouse communicates over port 1433. If you are trying to connect from within a corporate network, outbound traffic over port 1433 might not be allowed by your network's firewall. If so, you cannot connect to your Azure SQL Database server unless your IT department opens port 1433.
>

1. After the deployment completes, click **SQL databases** from the left-hand menu and then click **mySampleDatabase** on the **SQL databases** page. The overview page for your database opens, showing you the fully qualified server name (such as **mynewserver-20171113.database.windows.net**) and provides options for further configuration. 

2. Copy this fully qualified server name for use to connect to your server and its databases in subsequent quick starts. To open server settings, click the server name.

   ![find server name](media/load-data-from-azure-blob-storage-using-polybase/find-server-name.png) 

3. To open server settings, 
4. click the server name.

   ![server settings](media/load-data-from-azure-blob-storage-using-polybase/server-settings.png) 

5. Click **Show firewall settings**. The **Firewall settings** page for the SQL Database server opens. 

   ![server firewall rule](media/load-data-from-azure-blob-storage-using-polybase/server-firewall-rule.png) 

4. To add your current IP address to a new firewall rule, click **Add client IP** on the toolbar. A firewall rule can open port 1433 for a single IP address or a range of IP addresses.

5. Click **Save**. A server-level firewall rule is created for your current IP address opening port 1433 on the logical server.

6. Click **OK** and then close the **Firewall settings** page.

You can now connect to the SQL server and its data warehouses using this IP address. The connection works from SQL Server Management Studio or another tool of your choice. When you connect, use the ServerAdmin account you created previously.  

> [!IMPORTANT]
> By default, access through the SQL Database firewall is enabled for all Azure services. Click **OFF** on this page and then click **Save** to disable the firewall for all Azure services.

## Get the fully qualified server name

Get the fully qualified server name for your SQL server in the Azure portal. Later you use the fully qualified name when connecting to the server.

1. Sign in to the [Azure portal](https://portal.azure.com/).
2. Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page. 
3. In the **Essentials** pane in the Azure portal page for your database, locate and then copy the **Server name**. In this example, the fully qualified name is mynewserver-20171113.database.windows.net. 

    ![connection information](media/load-data-from-azure-blob-storage-using-polybase/find-server-name.png)  

## Connect to the server as server admin

This section uses [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS) to establish a connection to your Azure SQL server.

1. Open SQL Server Management Studio.

2. In the **Connect to Server** dialog box, enter the following information:

   | Setting       | Suggested value | Description | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Server type | Database engine | This value is required |
   | Server name | The fully qualified server name | Here's an example: **mynewserver-20171113.database.windows.net**. |
   | Authentication | SQL Server Authentication | SQL Authentication is the only authentication type that is configured in this tutorial. |
   | Login | The server admin account | This is the account that you specified when you created the server. |
   | Password | The password for your server admin account | This is the password that you specified when you created the server. |

    ![connect to server](media/load-data-from-azure-blob-storage-using-polybase/connect-to-server.png)

4. Click **Connect**. The Object Explorer window opens in SSMS. 

5. In Object Explorer, expand **Databases**. Then expand **mySampleDatabase** to view the objects in your new database.

    ![database objects](media/create-data-warehouse-portal/connected.png) 

## Run some queries

SQL Data Warehouse uses T-SQL as the query language. To open a query window and run some T-SQL queries, use the following steps:

1. Right-click **mySampleDataWarehouse** and select **New Query**.  A new query window opens.
2. In the query window, enter the following command to see a list of databases.

    ```sql
    SELECT * FROM sys.databases
    ```

3. Click **Execute**.  The query results show two databases: **master** and **mySampleDataWarehouse**.

    ![Query databases](media/create-data-warehouse-portal/query-databases.png)

4. To look at some data, use the following command to see the number of customers with last name of Adams that have three children at home. The results list six customers. 

    ```sql
    SELECT LastName, FirstName FROM dbo.dimCustomer
    WHERE LastName = 'Adams' AND NumberChildrenAtHome = 3;
    ```

    ![Query dbo.dimCustomer](media/create-data-warehouse-portal/query-customer.png)

## Clean up resources

You are being charged for data warehouse units and data stored your data warehouse. These compute and storage resources are billed separately. 

- If you want to keep the data in storage, you can pause compute when you aren't using the data warehouse. By pausing compute, you are only charged for data storage. You can resume compute whenever you are ready to work with the data.
- If you want to remove future charges, you can delete the data warehouse. 

Follow these steps to clean up resources as you desire.

1. Sign in to the [Azure portal](https://portal.azure.com), click on your data warehouse.

    ![Clean up resources](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. To pause compute, click the **Pause** button. When the data warehouse is paused, you see a **Start** button.  To resume compute, click **Start**.

2. To remove the data warehouse so you are not charged for compute or storage, click **Delete**.

3. To remove the SQL server you created, click **mynewserver-20171113.database.windows.net** in the previous image, and then click **Delete**.  Be careful with this deletion, since deleting the server also deletes all databases assigned to the server.

4. To remove the resource group, click **myResourceGroup**, and then click **Delete resource group**.


## Next steps
You have now created a data warehouse, created a firewall rule, connected to your data warehouse, and run a few queries. To learn more about Azure SQL Data Warehouse, continue to the tutorial for loading data.
> [!div class="nextstepaction"]
>[Load data into a SQL data warehouse](load-data-from-azure-blob-storage-using-polybase.md)