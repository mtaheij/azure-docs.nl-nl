---
title: Naslag informatie over beheer-API'S voor Azure SQL Managed instance
description: Meer informatie over het maken en configureren van beheerde exemplaren van Azure SQL Managed instance.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: devx-track-azurecli
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/12/2019
ms.openlocfilehash: 21239d868213df5d3208b8c790c67d675928d98a
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91283765"
---
# <a name="managed-api-reference-for-azure-sql-managed-instance"></a>Beheerde API-verwijzing voor Azure SQL Managed instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

U kunt beheerde exemplaren van een Azure SQL Managed instance maken en configureren met behulp van de Azure Portal, Power shell, Azure CLI, REST API en Transact-SQL. In dit artikel vindt u een overzicht van de functies en de API die u kunt gebruiken om beheerde exemplaren te maken en te configureren.

## <a name="azure-portal-create-a-managed-instance"></a>Azure Portal: Een beheerd exemplaar maken

Voor een Snelstartgids waarin wordt getoond hoe u een beheerd exemplaar maakt, Zie [Quick Start: een beheerd exemplaar maken](instance-create-quickstart.md).

## <a name="powershell-create-and-configure-managed-instances"></a>Power shell: beheerde instanties maken en configureren

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De Power shell-Azure Resource Manager module wordt nog steeds ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module AZ. SQL. Zie [AzureRM. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)voor deze cmdlets. De argumenten voor de opdrachten in de module AZ en in de AzureRM-modules zijn aanzienlijk identiek.

Als u beheerde exemplaren met Azure PowerShell wilt maken en beheren, gebruikt u de volgende Power shell-cmdlets. Als u Power shell wilt installeren of upgraden, raadpleegt u [de Azure PowerShell-module installeren](/powershell/azure/install-az-ps).

> [!TIP]
> Zie voor beelden van Power shell-scripts [Quick start script: een beheerd exemplaar maken met een Power shell-bibliotheek](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../quick-start-script-create-azure-sql-managed-instance-using-powershell/).

| Cmdlet | Beschrijving |
| --- | --- |
|[New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)|Hiermee maakt u een beheerd exemplaar. |
|[Get-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstance)|Retourneert informatie over een beheerd exemplaar.|
|[Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance)|Hiermee stelt u eigenschappen in voor een beheerd exemplaar.|
|[Remove-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstance)|Hiermee verwijdert u een beheerd exemplaar.|
|[Get-AzSqlInstanceOperation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstanceoperation)|Hiermee wordt een lijst opgehaald met beheer bewerkingen die worden uitgevoerd op het beheerde exemplaar of een specifieke bewerking.|
|[Stoppen-AzSqlInstanceOperation](https://docs.microsoft.com/powershell/module/az.sql/stop-azsqlinstanceoperation)|Hiermee wordt de specifieke beheer bewerking die op het beheerde exemplaar is uitgevoerd, geannuleerd.|
|[New-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstancedatabase)|Hiermee maakt u een SQL Managed instance-data base.|
|[Get-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabase)|Retourneert informatie over een SQL Managed instance-data base.|
|[Remove-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabase)|Hiermee verwijdert u een SQL Managed instance-data base.|
|[Restore-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase)|Hiermee herstelt u een SQL Managed instance-data base.|

## <a name="azure-cli-create-and-configure-managed-instances"></a>Azure CLI: beheerde instanties maken en configureren

Als u beheerde exemplaren wilt maken en configureren met [Azure cli](/cli/azure), gebruikt u de volgende [Azure cli-opdrachten voor SQL Managed instance](/cli/azure/sql/mi). Gebruik [Azure Cloud shell](/azure/cloud-shell/overview) om de cli in uw browser uit te voeren of [Installeer](/cli/azure/install-azure-cli) het op MacOS, Linux of Windows.

> [!TIP]
> Zie [werken met SQL Managed instance met Azure cli](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44)voor een Snelstartgids van Azure cli.

| Cmdlet | Beschrijving |
| --- | --- |
|[AZ SQL mi Create](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) |Hiermee maakt u een beheerd exemplaar.|
|[AZ SQL mi List](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-list)|Een lijst met beschik bare beheerde exemplaren.|
|[AZ SQL mi show](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-show)|Hiermee haalt u de Details voor een beheerd exemplaar op.|
|[AZ SQL mi update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)|Hiermee wordt een beheerd exemplaar bijgewerkt.|
|[AZ SQL mi Delete](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-delete)|Hiermee verwijdert u een beheerd exemplaar.|
|[AZ SQL mi op lijst](https://docs.microsoft.com/cli/azure/sql/mi/op#az_sql_mi_op_list)|Hiermee wordt een lijst opgehaald met beheer bewerkingen die worden uitgevoerd op het beheerde exemplaar.|
|[AZ SQL mi op show](https://docs.microsoft.com/cli/azure/sql/mi/op#az_sql_mi_op_show)|Hiermee wordt de specifieke beheer bewerking opgehaald die wordt uitgevoerd op het beheerde exemplaar.|
|[AZ SQL mi op Cancel](https://docs.microsoft.com/cli/azure/sql/mi/op#az_sql_mi_op_cancel)|Hiermee wordt de specifieke beheer bewerking die op het beheerde exemplaar is uitgevoerd, geannuleerd.|
|[AZ SQL DEELB Create](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-create) |Hiermee maakt u een beheerde data base.|
|[AZ SQL DEELB List](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-list)|Een lijst met beschik bare beheerde data bases.|
|[AZ SQL DEELB Restore](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-restore)|Hiermee herstelt u een beheerde data base.|
|[AZ SQL DEELB Delete](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-delete)|Hiermee verwijdert u een beheerde data base.|

## <a name="transact-sql-create-and-configure-instance-databases"></a>Transact-SQL: exemplaar databases maken en configureren

Als u exemplaar databases wilt maken en configureren nadat het beheerde exemplaar is gemaakt, gebruikt u de volgende T-SQL-opdrachten. U kunt deze opdrachten geven met behulp van de Azure Portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is), [Visual Studio code](https://code.visualstudio.com/docs)of een ander programma dat verbinding kan maken met een server en Transact-SQL-opdrachten kan passeren.

> [!TIP]
> Voor Quick starts die laten zien hoe u een beheerd exemplaar met behulp van SQL Server Management Studio in micro soft Windows configureert en er verbinding mee maakt, raadpleegt u [Quick Start: Azure VM configureren om verbinding te maken met Azure SQL Managed instance](connect-vm-instance-configure.md) en [Quick Start: Configureer een punt-naar-site-verbinding met Azure SQL Managed instance van on-premises](point-to-site-p2s-configure.md).

> [!IMPORTANT]
> U kunt een beheerd exemplaar niet maken of verwijderen met behulp van Transact-SQL.

| Opdracht | Beschrijving |
| --- | --- |
|[DATA BASE MAKEN](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current&preserve-view=true)|Hiermee maakt u een nieuwe instantie database in een SQL-beheerd exemplaar. U moet zijn verbonden met de hoofd database om een nieuwe Data Base te kunnen maken.|
| [ALTER DATA BASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current&preserve-view=true) |Hiermee wijzigt u een exemplaar database in een SQL Managed instance.|

## <a name="rest-api-create-and-configure-managed-instances"></a>REST API: beheerde instanties maken en configureren

Gebruik deze REST API aanvragen om beheerde exemplaren te maken en te configureren.

| Opdracht | Beschrijving |
| --- | --- |
|[Beheerde instanties: maken of bijwerken](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)|Hiermee wordt een beheerd exemplaar gemaakt of bijgewerkt.|
|[Beheerde instanties-verwijderen](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)|Hiermee verwijdert u een beheerd exemplaar.|
|[Beheerde instanties-ophalen](https://docs.microsoft.com/rest/api/sql/managedinstances/get)|Hiermee haalt u een beheerd exemplaar op.|
|[Beheerde instanties-lijst](https://docs.microsoft.com/rest/api/sql/managedinstances/list)|Retourneert een lijst met beheerde exemplaren in een abonnement.|
|[Beheerde instanties-lijst op resource groep](https://docs.microsoft.com/rest/api/sql/managedinstances/listbyresourcegroup)|Retourneert een lijst met beheerde exemplaren in een resource groep.|
|[Beheerde instanties-bijwerken](https://docs.microsoft.com/rest/api/sql/managedinstances/update)|Hiermee wordt een beheerd exemplaar bijgewerkt.|
|[Beheerde exemplaar bewerkingen-lijst op beheerde instantie](https://docs.microsoft.com/rest/api/sql/managedinstanceoperations/listbymanagedinstance)|Hiermee wordt een lijst opgehaald met beheer bewerkingen die worden uitgevoerd op het beheerde exemplaar.|
|[Bewerkingen voor beheerd exemplaar-ophalen](https://docs.microsoft.com/rest/api/sql/managedinstanceoperations/get)|Hiermee wordt de specifieke beheer bewerking opgehaald die wordt uitgevoerd op het beheerde exemplaar.|
|[Bewerkingen voor beheerd exemplaar-annuleren](https://docs.microsoft.com/rest/api/sql/managedinstanceoperations/cancel)|Hiermee wordt de specifieke beheer bewerking die op het beheerde exemplaar is uitgevoerd, geannuleerd.|

## <a name="next-steps"></a>Volgende stappen

- Zie [migreren naar Azure SQL database](../database/migrate-to-database-from-sql-server.md)voor meer informatie over het migreren van een SQL Server Data Base naar Azure.
- Zie [functies](../database/features-comparison.md)voor meer informatie over ondersteunde functies.
