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
# <a name="uninstall-elastic-database-jobs-components"></a><span data-ttu-id="0f38e-103">Avinstallera jobb komponenter för elastisk databas</span><span class="sxs-lookup"><span data-stu-id="0f38e-103">Uninstall Elastic Database jobs components</span></span>
<span data-ttu-id="0f38e-104">**Den elastiska databasen jobb** komponenter kan avinstalleras med hello Portal eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0f38e-104">**Elastic Database jobs** components can be uninstalled using either hello Portal or PowerShell.</span></span>

## <a name="uninstall-elastic-database-jobs-components-using-hello-azure-portal"></a><span data-ttu-id="0f38e-105">Avinstallera elastisk databas jobb komponenter med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0f38e-105">Uninstall Elastic Database jobs components using hello Azure portal</span></span>
1. <span data-ttu-id="0f38e-106">Öppna hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0f38e-106">Open hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="0f38e-107">Navigera toohello prenumeration som innehåller **elastisk databas jobb** komponenter, nämligen hello prenumeration i vilka elastisk databas jobb-komponenter installerades.</span><span class="sxs-lookup"><span data-stu-id="0f38e-107">Navigate toohello subscription that contains **Elastic Database jobs** components, namely hello subscription in which Elastic Database jobs components were installed.</span></span>
3. <span data-ttu-id="0f38e-108">Klicka på **Bläddra** och på **resursgrupper**.</span><span class="sxs-lookup"><span data-stu-id="0f38e-108">Click **Browse** and click **Resource groups**.</span></span>
4. <span data-ttu-id="0f38e-109">Välj hello resursgrupp med namnet ”__ElasticDatabaseJob”.</span><span class="sxs-lookup"><span data-stu-id="0f38e-109">Select hello resource group named "__ElasticDatabaseJob".</span></span>
5. <span data-ttu-id="0f38e-110">Ta bort hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="0f38e-110">Delete hello resource group.</span></span>

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a><span data-ttu-id="0f38e-111">Avinstallera elastisk databas jobb komponenter med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="0f38e-111">Uninstall  Elastic Database jobs components using PowerShell</span></span>
1. <span data-ttu-id="0f38e-112">Starta Microsoft Azure PowerShell-Kommandotolken och navigera toohello verktyg underkatalog under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x mapp: typen **cd verktyg**.</span><span class="sxs-lookup"><span data-stu-id="0f38e-112">Launch a Microsoft Azure PowerShell command window and navigate toohello tools sub-directory under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x folder: Type **cd tools**.</span></span>
   
     <span data-ttu-id="0f38e-113">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > CD-verktyg</span><span class="sxs-lookup"><span data-stu-id="0f38e-113">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools</span></span>
2. <span data-ttu-id="0f38e-114">Köra hello.\UninstallElasticDatabaseJobs.ps1 PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="0f38e-114">Execute hello .\UninstallElasticDatabaseJobs.ps1 PowerShell script.</span></span>
   
     <span data-ttu-id="0f38e-115">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > avblockera filen.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >.\UninstallElasticDatabaseJobs.ps1</span><span class="sxs-lookup"><span data-stu-id="0f38e-115">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1   PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1</span></span>

<span data-ttu-id="0f38e-116">Eller helt enkelt köra hello följande skript, förutsatt att standard värden används på installation av hello komponenter:</span><span class="sxs-lookup"><span data-stu-id="0f38e-116">Or simply, execute hello following script, assuming default values where used on installation of hello components:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="0f38e-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0f38e-117">Next steps</span></span>
<span data-ttu-id="0f38e-118">toore installera elastisk databas jobb, se [installera hello jobbet-tjänsten för elastisk databas](sql-database-elastic-jobs-service-installation.md)</span><span class="sxs-lookup"><span data-stu-id="0f38e-118">toore-install Elastic Database jobs, see [Installing hello Elastic Database job service](sql-database-elastic-jobs-service-installation.md)</span></span>

<span data-ttu-id="0f38e-119">En översikt över elastisk databas jobb finns [elastisk databas översikt över](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0f38e-119">For an overview of Elastic Database jobs, see [Elastic Database jobs overview](sql-database-elastic-jobs-overview.md).</span></span>

<!--Image references-->


