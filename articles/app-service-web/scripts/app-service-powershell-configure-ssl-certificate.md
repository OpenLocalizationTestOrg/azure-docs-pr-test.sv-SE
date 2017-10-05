---
title: Azure PowerShell skriptexempel - bindning som ett anpassat SSL-certifikatet till en webbapp | Microsoft Docs
description: "Azure PowerShell skriptexempel - binda ett SSL-certifikat som är anpassade till en webbapp"
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
ms.openlocfilehash: de8deccadcd9571be75447a117888bf2b3f571a0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app"></a><span data-ttu-id="f11e1-103">Koppla en anpassad SSL-certifikatet till en webbapp</span><span class="sxs-lookup"><span data-stu-id="f11e1-103">Bind a custom SSL certificate to a web app</span></span>

<span data-ttu-id="f11e1-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och sedan binda SSL-certifikatet för ett anpassat domännamn till den.</span><span class="sxs-lookup"><span data-stu-id="f11e1-104">This sample script creates a web app in App Service with its related resources, then binds the SSL certificate of a custom domain name to it.</span></span> 

<span data-ttu-id="f11e1-105">Om det behövs installerar du Azure PowerShell med hjälp av anvisningarna i den [Azure PowerShell guiden](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f11e1-105">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="f11e1-106">Se också till att:</span><span class="sxs-lookup"><span data-stu-id="f11e1-106">Also, ensure that:</span></span>

- <span data-ttu-id="f11e1-107">En anslutning med Azure har skapats med hjälp av den `az login` kommando.</span><span class="sxs-lookup"><span data-stu-id="f11e1-107">A connection with Azure has been created using the `az login` command.</span></span>
- <span data-ttu-id="f11e1-108">Du har åtkomst till din domänregistrator DNS-konfigurationssidan.</span><span class="sxs-lookup"><span data-stu-id="f11e1-108">You have access to your domain registrar's DNS configuration page.</span></span>
- <span data-ttu-id="f11e1-109">Du har en giltig. PFX-filen och lösenordet för SSL-certifikatet som du vill ladda upp och binda.</span><span class="sxs-lookup"><span data-stu-id="f11e1-109">You have a valid .PFX file and its password for the SSL certificate you want to upload and bind.</span></span>

## <a name="sample-script"></a><span data-ttu-id="f11e1-110">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="f11e1-110">Sample script</span></span>

<span data-ttu-id="f11e1-111">[!code-powershell[huvudsakliga](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "binda ett anpassat SSL-certifikat till en webbapp")]</span><span class="sxs-lookup"><span data-stu-id="f11e1-111">[!code-powershell[main](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="f11e1-112">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="f11e1-112">Clean up deployment</span></span> 

<span data-ttu-id="f11e1-113">Följande kommando kan användas för att ta bort resursgruppen, webbprogram och alla relaterade resurser efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="f11e1-113">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="f11e1-114">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="f11e1-114">Script explanation</span></span>

<span data-ttu-id="f11e1-115">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="f11e1-115">This script uses the following commands.</span></span> <span data-ttu-id="f11e1-116">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f11e1-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="f11e1-117">Kommando</span><span class="sxs-lookup"><span data-stu-id="f11e1-117">Command</span></span> | <span data-ttu-id="f11e1-118">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="f11e1-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f11e1-119">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f11e1-119">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="f11e1-120">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="f11e1-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f11e1-121">Ny AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="f11e1-121">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="f11e1-122">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="f11e1-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="f11e1-123">Ny AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="f11e1-123">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="f11e1-124">Skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="f11e1-124">Creates a web app.</span></span> |
| [<span data-ttu-id="f11e1-125">Ange AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="f11e1-125">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="f11e1-126">Ändrar en App Service-plan för att ändra dess prisnivå.</span><span class="sxs-lookup"><span data-stu-id="f11e1-126">Modifies an App Service plan to change its pricing tier.</span></span> |
| [<span data-ttu-id="f11e1-127">Ange AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="f11e1-127">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="f11e1-128">Ändrar någon konfiguration för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="f11e1-128">Modifies a web app's configuration.</span></span> |
| [<span data-ttu-id="f11e1-129">Ny AzureRmWebAppSSLBinding</span><span class="sxs-lookup"><span data-stu-id="f11e1-129">New-AzureRmWebAppSSLBinding</span></span>](/powershell/module/azurerm.websites/new-azurermwebappsslbinding) | <span data-ttu-id="f11e1-130">Skapar en SSL-certifikat-bindning för en webbapp.</span><span class="sxs-lookup"><span data-stu-id="f11e1-130">Creates an SSL certificate binding for a web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f11e1-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f11e1-131">Next steps</span></span>

<span data-ttu-id="f11e1-132">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f11e1-132">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="f11e1-133">Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i den [Azure PowerShell-exempel](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f11e1-133">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
