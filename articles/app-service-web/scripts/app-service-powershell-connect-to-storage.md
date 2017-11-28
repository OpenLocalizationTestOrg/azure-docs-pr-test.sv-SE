---
title: "Azure PowerShell-skript exempel – ansluta en webbapp till ett lagringskonto | Microsoft Docs"
description: "Azure PowerShell-skript exempel – ansluta en webbapp till ett lagringskonto"
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
ms.openlocfilehash: 481f3efdb1cbbeba328183da7e320c7e5b819b3a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="connect-a-web-app-to-a-storage-account"></a><span data-ttu-id="7be87-103">Ansluta en webbapp till ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="7be87-103">Connect a web app to a storage account</span></span>

<span data-ttu-id="7be87-104">I det här scenariot du lära dig hur du skapar ett Azure storage-konto och ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="7be87-104">In this scenario you will learn how to create an Azure storage account and an Azure web app.</span></span> <span data-ttu-id="7be87-105">Sedan kommer du länka storage-konto till det webbprogram som använder app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="7be87-105">Then you will link the storage account to the web app using app settings.</span></span>

<span data-ttu-id="7be87-106">Om det behövs installerar du Azure PowerShell med hjälp av anvisningarna i den [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` att skapa en anslutning med Azure.</span><span class="sxs-lookup"><span data-stu-id="7be87-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="7be87-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="7be87-107">Sample script</span></span>

<span data-ttu-id="7be87-108">[!code-powershell[huvudsakliga](../../../powershell_scripts/app-service/connect-to-storage/connect-to-storage.ps1 "ansluta en webbapp till ett lagringskonto")]</span><span class="sxs-lookup"><span data-stu-id="7be87-108">[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-storage/connect-to-storage.ps1 "Connect a web app to a storage account")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="7be87-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="7be87-109">Clean up deployment</span></span> 

<span data-ttu-id="7be87-110">Följande kommando kan användas för att ta bort resursgruppen, webbprogram och alla relaterade resurser efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="7be87-110">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="7be87-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="7be87-111">Script explanation</span></span>

<span data-ttu-id="7be87-112">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="7be87-112">This script uses the following commands.</span></span> <span data-ttu-id="7be87-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="7be87-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="7be87-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="7be87-114">Command</span></span> | <span data-ttu-id="7be87-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="7be87-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7be87-116">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="7be87-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="7be87-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="7be87-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7be87-118">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="7be87-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="7be87-119">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="7be87-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="7be87-120">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="7be87-120">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="7be87-121">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="7be87-121">Creates a web app.</span></span> |
| [<span data-ttu-id="7be87-122">Ny AzureRMStorageAccount</span><span class="sxs-lookup"><span data-stu-id="7be87-122">New-AzureRMStorageAccount</span></span>](/powershell/module/azurerm.storage/new-azurermstorageaccount) | <span data-ttu-id="7be87-123">Skapar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7be87-123">Creates a Storage account.</span></span> |
| [<span data-ttu-id="7be87-124">Get-AzureRMStorageAccountKey</span><span class="sxs-lookup"><span data-stu-id="7be87-124">Get-AzureRMStorageAccountKey</span></span>](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | <span data-ttu-id="7be87-125">Hämtar åtkomstnycklarna för ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7be87-125">Gets the access keys for an Azure Storage account.</span></span> |
| [<span data-ttu-id="7be87-126">Ange AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="7be87-126">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="7be87-127">Ändrar någon konfiguration för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="7be87-127">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7be87-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7be87-128">Next steps</span></span>

<span data-ttu-id="7be87-129">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7be87-129">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="7be87-130">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i den [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="7be87-130">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
