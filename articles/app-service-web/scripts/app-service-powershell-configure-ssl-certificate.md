---
title: "aaaAzure PowerShell skriptexempel - binda ett anpassat webbprogram för SSL-certifikat tooa | Microsoft Docs"
description: "Azure PowerShell skriptexempel - Bind ett anpassat webbprogram för SSL-certifikat tooa"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 23e83b74-614a-49a0-bc08-7542120eeec5
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6e249ceedb9e2b8872dd0bc8b0aea0d718b28dab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="bind-a-custom-ssl-certificate-tooa-web-app"></a><span data-ttu-id="45315-103">Bind ett anpassat webbprogram för SSL-certifikat tooa</span><span class="sxs-lookup"><span data-stu-id="45315-103">Bind a custom SSL certificate tooa web app</span></span>

<span data-ttu-id="45315-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och Binder hello SSL-certifikatet för en anpassad domän namnet tooit.</span><span class="sxs-lookup"><span data-stu-id="45315-104">This sample script creates a web app in App Service with its related resources, then binds hello SSL certificate of a custom domain name tooit.</span></span> 

<span data-ttu-id="45315-105">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="45315-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="45315-106">Se också till att:</span><span class="sxs-lookup"><span data-stu-id="45315-106">Also, ensure that:</span></span>

- <span data-ttu-id="45315-107">En anslutning med Azure har skapats med hjälp av hello `az login` kommando.</span><span class="sxs-lookup"><span data-stu-id="45315-107">A connection with Azure has been created using hello `az login` command.</span></span>
- <span data-ttu-id="45315-108">Du har åtkomst tooyour domän domänregistrators DNS-konfigurationssidan.</span><span class="sxs-lookup"><span data-stu-id="45315-108">You have access tooyour domain registrar's DNS configuration page.</span></span>
- <span data-ttu-id="45315-109">Du har en giltig. PFX-filen och lösenordet för hello SSL-certifikat som du vill tooupload och bind.</span><span class="sxs-lookup"><span data-stu-id="45315-109">You have a valid .PFX file and its password for hello SSL certificate you want tooupload and bind.</span></span>

## <a name="sample-script"></a><span data-ttu-id="45315-110">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="45315-110">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate tooa web app")]

## <a name="clean-up-deployment"></a><span data-ttu-id="45315-111">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="45315-111">Clean up deployment</span></span> 

<span data-ttu-id="45315-112">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="45315-112">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="45315-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="45315-113">Script explanation</span></span>

<span data-ttu-id="45315-114">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="45315-114">This script uses hello following commands.</span></span> <span data-ttu-id="45315-115">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="45315-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="45315-116">Kommando</span><span class="sxs-lookup"><span data-stu-id="45315-116">Command</span></span> | <span data-ttu-id="45315-117">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="45315-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="45315-118">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="45315-118">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="45315-119">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="45315-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="45315-120">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="45315-120">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="45315-121">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="45315-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="45315-122">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="45315-122">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="45315-123">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="45315-123">Creates a web app.</span></span> |
| [<span data-ttu-id="45315-124">Ange AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="45315-124">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="45315-125">Ändrar en App Service-plan toochange dess prisnivå.</span><span class="sxs-lookup"><span data-stu-id="45315-125">Modifies an App Service plan toochange its pricing tier.</span></span> |
| [<span data-ttu-id="45315-126">Ange AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="45315-126">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="45315-127">Ändrar någon konfiguration för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="45315-127">Modifies a web app's configuration.</span></span> |
| [<span data-ttu-id="45315-128">Ny AzureRmWebAppSSLBinding</span><span class="sxs-lookup"><span data-stu-id="45315-128">New-AzureRmWebAppSSLBinding</span></span>](/powershell/module/azurerm.websites/new-azurermwebappsslbinding) | <span data-ttu-id="45315-129">Skapar en SSL-certifikat-bindning för en webbapp.</span><span class="sxs-lookup"><span data-stu-id="45315-129">Creates an SSL certificate binding for a web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="45315-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="45315-130">Next steps</span></span>

<span data-ttu-id="45315-131">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="45315-131">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="45315-132">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="45315-132">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
