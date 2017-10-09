---
title: "aaaAzure PowerShell skriptexempel - ansluta ett lagringskonto för web app tooa | Microsoft Docs"
description: Azure PowerShell-skript Sample - Anslut en web app tooa storage-konto
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: e4831bdc-2068-4883-9474-0b34c2e3e255
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 07693366d32fbaefe92c18df67ded81661e1a2df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-storage-account"></a><span data-ttu-id="2d76b-103">Ansluta en web app tooa storage-konto</span><span class="sxs-lookup"><span data-stu-id="2d76b-103">Connect a web app tooa storage account</span></span>

<span data-ttu-id="2d76b-104">I det här scenariot du lära dig hur toocreate ett Azure storage-konto och ett Azure web app.</span><span class="sxs-lookup"><span data-stu-id="2d76b-104">In this scenario you will learn how toocreate an Azure storage account and an Azure web app.</span></span> <span data-ttu-id="2d76b-105">Du kan länka hello storage-konto toohello webbapp med app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="2d76b-105">Then you will link hello storage account toohello web app using app settings.</span></span>

<span data-ttu-id="2d76b-106">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="2d76b-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="2d76b-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="2d76b-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-storage/connect-to-storage.ps1 "Connect a web app tooa storage account")]

## <a name="clean-up-deployment"></a><span data-ttu-id="2d76b-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="2d76b-108">Clean up deployment</span></span> 

<span data-ttu-id="2d76b-109">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="2d76b-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="2d76b-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="2d76b-110">Script explanation</span></span>

<span data-ttu-id="2d76b-111">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="2d76b-111">This script uses hello following commands.</span></span> <span data-ttu-id="2d76b-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="2d76b-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="2d76b-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="2d76b-113">Command</span></span> | <span data-ttu-id="2d76b-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="2d76b-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2d76b-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="2d76b-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="2d76b-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="2d76b-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2d76b-117">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="2d76b-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="2d76b-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="2d76b-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="2d76b-119">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="2d76b-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="2d76b-120">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="2d76b-120">Creates a web app.</span></span> |
| [<span data-ttu-id="2d76b-121">Ny AzureRMStorageAccount</span><span class="sxs-lookup"><span data-stu-id="2d76b-121">New-AzureRMStorageAccount</span></span>](/powershell/module/azurerm.storage/new-azurermstorageaccount) | <span data-ttu-id="2d76b-122">Skapar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="2d76b-122">Creates a Storage account.</span></span> |
| [<span data-ttu-id="2d76b-123">Get-AzureRMStorageAccountKey</span><span class="sxs-lookup"><span data-stu-id="2d76b-123">Get-AzureRMStorageAccountKey</span></span>](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | <span data-ttu-id="2d76b-124">Hämtar hello snabbtangenter för ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2d76b-124">Gets hello access keys for an Azure Storage account.</span></span> |
| [<span data-ttu-id="2d76b-125">Ange AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="2d76b-125">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="2d76b-126">Ändrar någon konfiguration för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="2d76b-126">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2d76b-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2d76b-127">Next steps</span></span>

<span data-ttu-id="2d76b-128">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2d76b-128">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="2d76b-129">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="2d76b-129">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
