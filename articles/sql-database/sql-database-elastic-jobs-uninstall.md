---
title: "Så här avinstallerar du Verktyg för elastisk databas jobb"
description: "Så här avinstallerar du verktyget jobb elastisk databas"
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
ms.openlocfilehash: ae7f0bce452a0a86f6f1e4d9b0c93a0fa1727f21
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="uninstall-elastic-database-jobs-components"></a><span data-ttu-id="395b7-103">Avinstallera jobb komponenter för elastisk databas</span><span class="sxs-lookup"><span data-stu-id="395b7-103">Uninstall Elastic Database jobs components</span></span>
<span data-ttu-id="395b7-104">**Den elastiska databasen jobb** komponenter kan avinstalleras med hjälp av portalen eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="395b7-104">**Elastic Database jobs** components can be uninstalled using either the Portal or PowerShell.</span></span>

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a><span data-ttu-id="395b7-105">Avinstallera elastisk databas jobb komponenter med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="395b7-105">Uninstall Elastic Database jobs components using the Azure portal</span></span>
1. <span data-ttu-id="395b7-106">Öppna [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="395b7-106">Open the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="395b7-107">Navigera till den prenumeration som innehåller **elastisk databas jobb** komponenter, nämligen prenumerationen i vilka elastisk databas jobb-komponenter installerades.</span><span class="sxs-lookup"><span data-stu-id="395b7-107">Navigate to the subscription that contains **Elastic Database jobs** components, namely the subscription in which Elastic Database jobs components were installed.</span></span>
3. <span data-ttu-id="395b7-108">Klicka på **Bläddra** och på **resursgrupper**.</span><span class="sxs-lookup"><span data-stu-id="395b7-108">Click **Browse** and click **Resource groups**.</span></span>
4. <span data-ttu-id="395b7-109">Välj den resursgrupp med namnet ”__ElasticDatabaseJob”.</span><span class="sxs-lookup"><span data-stu-id="395b7-109">Select the resource group named "__ElasticDatabaseJob".</span></span>
5. <span data-ttu-id="395b7-110">Ta bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="395b7-110">Delete the resource group.</span></span>

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a><span data-ttu-id="395b7-111">Avinstallera elastisk databas jobb komponenter med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="395b7-111">Uninstall  Elastic Database jobs components using PowerShell</span></span>
1. <span data-ttu-id="395b7-112">Starta Microsoft Azure PowerShell-Kommandotolken och navigera till verktyg underkatalog under mappen Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x: typen **cd verktyg**.</span><span class="sxs-lookup"><span data-stu-id="395b7-112">Launch a Microsoft Azure PowerShell command window and navigate to the tools sub-directory under the Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x folder: Type **cd tools**.</span></span>
   
     <span data-ttu-id="395b7-113">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > CD-verktyg</span><span class="sxs-lookup"><span data-stu-id="395b7-113">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools</span></span>
2. <span data-ttu-id="395b7-114">Köra.\UninstallElasticDatabaseJobs.ps1 PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="395b7-114">Execute the .\UninstallElasticDatabaseJobs.ps1 PowerShell script.</span></span>
   
     <span data-ttu-id="395b7-115">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > avblockera filen.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >.\UninstallElasticDatabaseJobs.ps1</span><span class="sxs-lookup"><span data-stu-id="395b7-115">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1   PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1</span></span>

<span data-ttu-id="395b7-116">Eller helt enkelt, kör du följande skript, förutsatt att standard värden används på installation av komponenter:</span><span class="sxs-lookup"><span data-stu-id="395b7-116">Or simply, execute the following script, assuming default values where used on installation of the components:</span></span>

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager

        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }

        Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a><span data-ttu-id="395b7-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="395b7-117">Next steps</span></span>
<span data-ttu-id="395b7-118">Om du vill installera elastisk databas jobb, se [installera tjänsten jobbet elastisk databas](sql-database-elastic-jobs-service-installation.md)</span><span class="sxs-lookup"><span data-stu-id="395b7-118">To re-install Elastic Database jobs, see [Installing the Elastic Database job service](sql-database-elastic-jobs-service-installation.md)</span></span>

<span data-ttu-id="395b7-119">En översikt över elastisk databas jobb finns [elastisk databas översikt över](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="395b7-119">For an overview of Elastic Database jobs, see [Elastic Database jobs overview](sql-database-elastic-jobs-overview.md).</span></span>

<!--Image references-->


