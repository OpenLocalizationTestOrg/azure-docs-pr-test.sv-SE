---
title: aaaPowerShell exempel georeplikering redundans grupp enda Azure SQL Database | Microsoft Docs
description: "Azure PowerShell exempel skriptet tooset in aktiv geo-replikering för en enda Azure SQL-databas"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 0ea7afb1125b95370811ef7f80cb9eb7391b2443
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooconfigure-an-active-geo-replication-failover-group-for-a-single-azure-sql-database"></a>Använd PowerShell tooconfigure redundansväxlingsgrupp en aktiv geo-replikering för en enda Azure SQL-databas

Det här exemplet för PowerShell-skriptet konfigurerar en aktiv geo-replikering redundansväxlingsgrupp för en enda Azure SQL-databas och växlar tooa sekundär replik av hello Azure SQL-databas.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-database/setup-geodr-and-failover-database-failover-group.ps1?highlight=19-22 "Set up failover group for single database")]

## <a name="clean-up-deployment"></a>Rensa distribution

Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myPrimaryResourceGroup"
Remove-AzureRmResourceGroup -ResourceGroupName "mySecondaryResourceGroup"
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Ny AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Ny AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Skapar en logisk server som är värd för en databas eller elastisk pool. |
| [Ny AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | Skapar en elastisk pool i en logisk server. |
| [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) | Uppdaterar Databasegenskaper eller flyttar en databas i, slut på eller mellan elastiska pooler. |
| [Ny AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| Skapar en sekundär databas för en befintlig databas och startar datareplikeringen. |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)| Hämtar en eller flera databaser. |
| [Ange AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| Växlar en sekundär databas toobe primära tooinitiate redundans.|
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | Hämtar hello geo-replikeringslänkar mellan en Azure SQL Database och en resursgrupp eller SQL Server. |
| [Ta bort AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | Avbryter datareplikering mellan en SQL-databas och hello angivna sekundära databasen. |
| [Ta bort AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |
|||

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare exempel för SQL Database PowerShell-skript finns i hello [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).
