---
title: Select the right tools for migration to Azure Database for MySQL
description: "This article provides a decision table, which helps customers in picking the right tools for migrating into Azure Database for MySQL"
author: aditivgupta
ms.author: adig
ms.reviewer: maghan
ms.date: 09/06/2022
ms.service: mysql
ms.subservice: single-server
ms.topic: how-to
---

# Select the right tools for migration to Azure Database for MySQL

[!INCLUDE[applies-to-mysql-single-flexible-server](../includes/applies-to-mysql-single-flexible-server.md)]

Migrations are multi-step projects that can be tough to complete. Migrating database servers across platforms involves more than data and schema migration. There are also several other components, such as server configuration parameters, networking, access control rules, etc., to move. These are required to ensure that the functionality of the database server in the new target platform mimics the source. 

For detailed information and use cases about migrating databases to Azure Database for MySQL, refer to the [Database Migration Guide](../migrate/mysql-on-premises-azure-db/01-mysql-migration-guide-intro.md). This document provides pointers to help you successfully plan and execute a MySQL migration to Azure. 

In general, migrations can be categorized as either offline or online. 

- With an offline migration, the source server is taken offline, and a dump and restores of the databases are performed on the target server. 

- With an online migration (migration with minimal downtime), the source server allows updates, and the migration solution will take care of replicating the ongoing changes between the source and target server along with the initial dump and restore on the target. 

If your application can afford some downtime, offline migrations are always the preferred choice, as they're simple and easy to execute. However, an online migration is the best choice if your application can only afford minimal downtime. Migrations of most OLTP systems, such as payment processing and e-commerce, fall into this category. 

## Decision table

There are both offline and online migration scenarios to help you select the right tools for migrating to Azure Database for MySQL - Flexible Server.

### Offline

To help you select the right tools for migrating to Azure Database for MySQL, consider the detail in the following table for offline migrations.

| Migration Scenario | Tool(s) | Details | More information |
|--------------------|---------|---------|------------------|
| Single to Flexible Server (Azure portal) | Database Migration Service (classic) and the Azure portal | [Tutorial: DMS (classic) with the Azure portal   (offline)](../../dms/tutorial-mysql-azure-single-to-flex-offline-portal.md) | Recommended |
| Single to Flexible Server (Azure CLI) | [Custom shell script](https://github.com/Azure/azure-mysql/tree/master/azuremysqltomysqlmigrate) | [Migrate from Azure Database for MySQL - Single Server to   Flexible Server in five easy steps!](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/migrate-from-azure-database-for-mysql-single-server-to-flexible/ba-p/2674057) | The [script](https://github.com/Azure/azure-mysql/tree/master/azuremysqltomysqlmigrate) also moves other server components such as security settings and server   parameter configurations. |
| MySQL databases (>= 1 TB) to Azure Database for MySQL | Dump and Restore using **MyDumper/MyLoader** + High Compute VM | [Migrate large databases to Azure Database for MySQL using   mydumper/myloader](concepts-migrate-mydumper-myloader.md) | [Best Practices for migrating large databases to Azure   Database for   MySQL](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/best-practices-for-migrating-large-databases-to-azure-database/ba-p/1362699) |
| MySQL databases (< 1 TB) to Azure Database for MySQL | Database Migration Service (classic) and the Azure portal | [Migrate MySQL databases to Azure Database for MySQL using DMS (classic)](../../dms/tutorial-mysql-azure-mysql-offline-portal.md) | If network bandwidth between source and target is good (e.g: High-speed express route), use Azure DMS (database migration service) |
| Amazon RDS for MySQL databases (< 1 TB) to Azure Database for MySQL | MySQL Workbench | [Migrate Amazon RDS for MySQL databases ( < 1 TB) to Azure   Database for MySQL using MySQL   Workbench](../single-server/how-to-migrate-rds-mysql-workbench.md) | If you have low network bandwidth between source and Azure, use **Mydumper/Myloader + High compute VM** to take advantage of compression settings to efficiently move data over low speed networks |
| Import and export MySQL databases (< 1 TB) in Azure Database for MySQL | mysqldump or MySQL Workbench Import/Export utility | [Import and export - Azure Database for   MySQL](../single-server/concepts-migrate-import-export.md) | Use the **mysqldump** and **MySQL Workbench Export/Import** utility tool to perform offline migrations for smaller databases. |

### Online

To help you select the right tools for migrating to Azure Database for MySQL - Flexible Server, consider the detail in the following table for online migrations.

| Migration Scenario | Tool(s) | Details | More information |
|--------------------|---------|---------|------------------|
| Single to Flexible Server (Azure portal) | Database Migration Service (classic) | [Tutorial: DMS (classic) with the Azure portal (online)](../../dms/tutorial-mysql-Azure-single-to-flex-online-portal.md) | Recommended |
| Single to Flexible Server | Mydumper/Myloader with Data-in replication | [Migrate Azure Database for MySQL – Single Server to Azure Database for MySQL – Flexible Server with open-source   tools](how-to-migrate-single-flexible-minimum-downtime.md) | N/A |
| Azure Database for MySQL Flexible Server Data-in replication | **Mydumper/Myloader with Data-in replication** | [Configure Data-in replication - Azure Database for MySQL Flexible   Server](../flexible-server/how-to-data-in-replication.md) | N/A |

## Next steps
* [Migrate MySQL on-premises to Azure Database for MySQL](../migrate/mysql-on-premises-azure-db/01-mysql-migration-guide-intro.md)