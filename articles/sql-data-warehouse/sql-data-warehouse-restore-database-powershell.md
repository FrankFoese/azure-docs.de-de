---
title: Wiederherstellen einer Azure SQL Data Warehouse-Instanz (PowerShell) | Microsoft Docs
description: PowerShell-Aufgaben zum Wiederherstellen einer Azure SQL Data Warehouse-Instanz.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 9ba1137c3727182fec4e5e90b0d561b055ef756d
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55243622"
---
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Wiederherstellen einer Azure SQL Data Warehouse-Instanz (PowerShell)
> [!div class="op_single_selector"]
> * [Übersicht][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

In diesem Artikel erfahren Sie, wie Sie eine Azure SQL Data Warehouse-Instanz mithilfe von PowerShell wiederherstellen.

## <a name="before-you-begin"></a>Voraussetzungen
**Überprüfen Sie Ihre DTU-Kapazität.** Jedes SQL Data Warehouse wird von einer SQL Server-Instanz gehostet (z.B. myserver.database.windows.net), die über ein Standard-DTU-Kontingent verfügt.  Bevor Sie ein SQL Data Warehouse wiederherstellen können, überprüfen Sie, ob Ihre SQL Server-Instanz über genügend verbleibendes DTU-Kontingent für die Datenbank-Wiederherstellung verfügt. Informationen zum Berechnen des DTU-Bedarfs bzw. zur Anforderung weiterer DTUs finden Sie unter [Anfordern einer DTU-Kontingentänderung][Request a DTU quota change].

### <a name="install-powershell"></a>Installieren von PowerShell
Damit Sie Azure PowerShell mit SQL Data Warehouse verwenden können, müssen Sie Azure PowerShell Version 1.0 oder höher installieren.  Sie können die Version überprüfen, indem Sie **Get-Module -ListAvailable -Name AzureRM**ausführen.  Sie können die neueste Version über den [Microsoft-Webplattform-Installer][Microsoft Web Platform Installer] installieren.  Weitere Informationen zum Installieren der neuesten Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][How to install and configure Azure PowerShell].

## <a name="restore-an-active-or-paused-database"></a>Wiederherstellen einer aktiven oder angehaltenen Datenbank
Verwenden Sie das PowerShell-Cmdlet [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase], um eine Datenbank auf der Grundlage einer Momentaufnahme wiederherzustellen.

1. Öffnen Sie Windows PowerShell.
2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und listen Sie alle Abonnements auf, die Ihrem Konto zugeordnet sind.
3. Wählen Sie das Abonnement aus, in dem die wiederherzustellende Datenbank enthalten ist.
4. Listen Sie die Wiederherstellungspunkte für die Datenbank auf.
5. Wählen Sie den gewünschten Wiederherstellungspunkt mit RestorePointCreationDate aus.
6. Stellen Sie die Datenbank mit dem gewünschten Wiederherstellungspunkt wieder her.
7. Überprüfen Sie, ob die wiederhergestellte Datenbank online ist.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName)).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> Nach Abschluss der Wiederherstellung können Sie Ihre wiederhergestellte Datenbank konfigurieren. Befolgen Sie hierzu die Anleitung [Konfigurieren der Datenbank nach der Wiederherstellung][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Wiederherstellen einer gelöschten Datenbank
Verwenden Sie das Cmdlet [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase], um eine gelöschte Datenbank wiederherzustellen.

1. Öffnen Sie Windows PowerShell.
2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und listen Sie alle Abonnements auf, die Ihrem Konto zugeordnet sind.
3. Wählen Sie das Abonnement aus, das die gelöschte Datenbank enthält, die wiederhergestellt werden soll.
4. Rufen Sie die spezifische gelöschte Datenbank ab.
5. Stellen Sie die gelöschte Datenbank wieder her.
6. Überprüfen Sie, ob die wiederhergestellte Datenbank online ist.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> Nach Abschluss der Wiederherstellung können Sie Ihre wiederhergestellte Datenbank konfigurieren. Befolgen Sie hierzu die Anleitung [Konfigurieren der Datenbank nach der Wiederherstellung][Configure your database after recovery].
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a>Wiederherstellen von einer geografischen Azure-Region aus
Verwenden Sie das Cmdlet [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase], um eine Datenbank wiederherzustellen.

> [!NOTE]
> Sie können eine Geowiederherstellung der Leistungsstufe „Optimiert für Compute“ durchführen! Geben Sie zu diesem Zweck einen „Optimiert für Compute“-ServiceObjectiveName als optionalen Parameter ein. 
>
> 

1. Öffnen Sie Windows PowerShell.
2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und listen Sie alle Abonnements auf, die Ihrem Konto zugeordnet sind.
3. Wählen Sie das Abonnement aus, in dem die wiederherzustellende Datenbank enthalten ist.
4. Rufen Sie die Datenbank ab, die Sie wiederherstellen möchten.
5. Erstellen Sie die Wiederherstellungsanforderung für die Datenbank.
6. Überprüfen Sie den Status der mittels Geowiederherstellung wiederhergestellten Datenbank.

```Powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName "<YourTargetServiceLevel>"

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> Informationen zum Konfigurieren der Datenbank nach Abschluss der Wiederherstellung finden Sie unter [Konfigurieren der Datenbank nach der Wiederherstellung][Configure your database after recovery].
> 
> 

Für die wiederhergestellte Datenbank ist TDE aktiviert, wenn für die Quelldatenbank TDE aktiviert ist.

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den Geschäftskontinuitätsfeatures von Azure SQL-Datenbank-Editionen finden Sie in der [Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Restore-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
