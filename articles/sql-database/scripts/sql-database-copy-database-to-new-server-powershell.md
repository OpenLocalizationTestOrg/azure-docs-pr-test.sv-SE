---
title: aaaPowerShell exempel kopia Azure SQL ny databas server | Microsoft Docs
description: "Azure PowerShell exempel skriptet toocopy en ny server för SQL-databasen tooa"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: c08f993bd75913481b1d534857ac057263e1d02b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocopy-a-sql-database-tooa-new-server"></a>Använd PowerShell toocopy en ny server för SQL-databasen tooa

Exempel på detta PowerShell-skript skapar en kopia av en befintlig databas i en ny server. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="copy-a-database-tooa-new-server"></a>Kopiera en tooa ny databasserver

[!code-powershell[main](../../../powershell_scripts/sql-database/copy-database-to-new-server/copy-database-to-new-server.ps1?highlight=18-21 "Copy database toonew server")]

## <a name="clean-up-deployment"></a>Rensa distribution

Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Ny AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Ny AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Skapar en logisk server som är värd för en databas eller elastisk pool. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Skapar en databas i en logisk server som en enda eller en delad databas. |
| [Ny AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) | Skapar en kopia av en databas som använder hello ögonblicksbilden vid hello aktuell tid. |
| [Ta bort AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |
|||

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare exempel för SQL Database PowerShell-skript finns i hello [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).
