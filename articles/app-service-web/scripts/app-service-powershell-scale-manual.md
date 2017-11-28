---
title: aaaAzure PowerShell skriptexempel - skala ett webbprogram manuellt | Microsoft Docs
description: "Azure PowerShell-skript exempel – skala ett webbprogram manuellt"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: de5d4285-9c7d-4735-a695-288264047375
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: c749031fbe6c6bcbb25395387b4f32b2ba75cef4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-manually"></a><span data-ttu-id="cc0fc-103">Skala ett webbprogram manuellt</span><span class="sxs-lookup"><span data-stu-id="cc0fc-103">Scale a web app manually</span></span>

<span data-ttu-id="cc0fc-104">I det här scenariot får du lära dig toocreate en resursgrupp, apptjänst planerings- och webb-app.</span><span class="sxs-lookup"><span data-stu-id="cc0fc-104">In this scenario you will learn toocreate a resource group, app service plan and web app.</span></span> <span data-ttu-id="cc0fc-105">Du kommer sedan att skala hello App Service-Plan från en enda instans toomultiple instanser.</span><span class="sxs-lookup"><span data-stu-id="cc0fc-105">You will then scale hello App Service Plan from a single instance toomultiple instances.</span></span>

<span data-ttu-id="cc0fc-106">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="cc0fc-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="cc0fc-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="cc0fc-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/scale-manual/scale-manual.ps1 "Scale a web app manually")]

## <a name="clean-up-deployment"></a><span data-ttu-id="cc0fc-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="cc0fc-108">Clean up deployment</span></span> 

<span data-ttu-id="cc0fc-109">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="cc0fc-109">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="cc0fc-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="cc0fc-110">Script explanation</span></span>

<span data-ttu-id="cc0fc-111">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="cc0fc-111">This script uses hello following commands.</span></span> <span data-ttu-id="cc0fc-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="cc0fc-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="cc0fc-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="cc0fc-113">Command</span></span> | <span data-ttu-id="cc0fc-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="cc0fc-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cc0fc-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="cc0fc-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="cc0fc-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="cc0fc-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="cc0fc-117">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="cc0fc-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="cc0fc-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="cc0fc-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="cc0fc-119">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="cc0fc-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="cc0fc-120">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="cc0fc-120">Creates a web app.</span></span> |
| [<span data-ttu-id="cc0fc-121">Ange AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="cc0fc-121">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="cc0fc-122">Ändrar någon konfiguration för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="cc0fc-122">Modifies a web app's configuration.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cc0fc-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cc0fc-123">Next steps</span></span>

<span data-ttu-id="cc0fc-124">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cc0fc-124">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="cc0fc-125">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="cc0fc-125">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
