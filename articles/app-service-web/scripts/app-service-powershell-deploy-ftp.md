---
title: "aaaAzure skriptexempel PowerShell - Överför filer tooa webbprogram med hjälp av FTP | Microsoft Docs"
description: "Azure PowerShell skriptexempel - Överför filer tooa webbapp med FTP"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: b7d46d6f-44fd-454c-8008-87dab6eefbc1
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 32a0a529e94c1315cc6730faf23fca2693c50ffb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-tooa-web-app-using-ftp"></a><span data-ttu-id="b3c07-103">Ladda upp filer tooa webbapp med FTP</span><span class="sxs-lookup"><span data-stu-id="b3c07-103">Upload files tooa web app using FTP</span></span>

<span data-ttu-id="b3c07-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och distribuerar sedan koden web app med hjälp av FTP (via [WebClient.UploadFile()](https://msdn.microsoft.com/library/ms144229.aspx)).</span><span class="sxs-lookup"><span data-stu-id="b3c07-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code using FTP (via [WebClient.UploadFile()](https://msdn.microsoft.com/library/ms144229.aspx)).</span></span>

<span data-ttu-id="b3c07-105">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="b3c07-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="b3c07-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="b3c07-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-ftp/deploy-ftp.ps1?highlight=1 "Upload files tooa web app using FTP")]

## <a name="clean-up-deployment"></a><span data-ttu-id="b3c07-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="b3c07-107">Clean up deployment</span></span> 

<span data-ttu-id="b3c07-108">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="b3c07-108">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name $webappname -Force
```

## <a name="script-explanation"></a><span data-ttu-id="b3c07-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="b3c07-109">Script explanation</span></span>

<span data-ttu-id="b3c07-110">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="b3c07-110">This script uses hello following commands.</span></span> <span data-ttu-id="b3c07-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="b3c07-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="b3c07-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="b3c07-112">Command</span></span> | <span data-ttu-id="b3c07-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="b3c07-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b3c07-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="b3c07-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="b3c07-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="b3c07-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b3c07-116">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="b3c07-116">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="b3c07-117">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="b3c07-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="b3c07-118">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="b3c07-118">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="b3c07-119">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b3c07-119">Creates a web app.</span></span> |
| [<span data-ttu-id="b3c07-120">Get-AzureRmWebAppPublishingProfile</span><span class="sxs-lookup"><span data-stu-id="b3c07-120">Get-AzureRmWebAppPublishingProfile</span></span>](/powershell/module/azurerm.websites/get-azurermwebapppublishingprofile) | <span data-ttu-id="b3c07-121">Hämta publiceringsprofilen för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b3c07-121">Get a web app's publishing profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b3c07-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b3c07-122">Next steps</span></span>

<span data-ttu-id="b3c07-123">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b3c07-123">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="b3c07-124">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b3c07-124">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
