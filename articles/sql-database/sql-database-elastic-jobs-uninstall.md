---
title: "jobb-verktyget för aaaHow toouninstall elastisk databas"
description: "Hur toouninstall hello jobb verktyget för elastisk databas"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: bfc9d820-edbd-4fca-bfbf-1f339cfcc448
ms.service: sql-database
ms.workload: sql-database
ms.custom: scale out apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 3bc1e889d5042bc3eaa9fd9da89816737e0b8df8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="uninstall-elastic-database-jobs-components"></a>Avinstallera jobb komponenter för elastisk databas
**Den elastiska databasen jobb** komponenter kan avinstalleras med hello Portal eller PowerShell.

## <a name="uninstall-elastic-database-jobs-components-using-hello-azure-portal"></a>Avinstallera elastisk databas jobb komponenter med hjälp av hello Azure-portalen
1. Öppna hello [Azure-portalen](https://portal.azure.com/).
2. Navigera toohello prenumeration som innehåller **elastisk databas jobb** komponenter, nämligen hello prenumeration i vilka elastisk databas jobb-komponenter installerades.
3. Klicka på **Bläddra** och på **resursgrupper**.
4. Välj hello resursgrupp med namnet ”__ElasticDatabaseJob”.
5. Ta bort hello resursgruppen.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>Avinstallera elastisk databas jobb komponenter med hjälp av PowerShell
1. Starta Microsoft Azure PowerShell-Kommandotolken och navigera toohello verktyg underkatalog under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x mapp: typen **cd verktyg**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > CD-verktyg
2. Köra hello.\UninstallElasticDatabaseJobs.ps1 PowerShell-skript.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > avblockera filen.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >.\UninstallElasticDatabaseJobs.ps1

Eller helt enkelt köra hello följande skript, förutsatt att standard värden används på installation av hello komponenter:

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager

        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "hello Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }

        Write-Host "Removing hello Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing hello Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a>Nästa steg
toore installera elastisk databas jobb, se [installera hello jobbet-tjänsten för elastisk databas](sql-database-elastic-jobs-service-installation.md)

En översikt över elastisk databas jobb finns [elastisk databas översikt över](sql-database-elastic-jobs-overview.md).

<!--Image references-->


