---
title: "Skapa en Azure Analysis Services-server med hjälp av PowerShell | Microsoft Docs"
description: "Lär dig hur du skapar en Azure Analysis Services-server med PowerShell"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/01/2017
ms.author: owend
ms.custom: mvc
ms.openlocfilehash: cb42fd3ed51364cf478848cc51ebbb2f175e96d2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a><span data-ttu-id="bfe7d-103">Skapa en Azure Analysis Services-server med PowerShell</span><span class="sxs-lookup"><span data-stu-id="bfe7d-103">Create an Azure Analysis Services server by using PowerShell</span></span>

<span data-ttu-id="bfe7d-104">I den här snabbstarten beskrivs hur du använder PowerShell från kommandoraden för att skapa en Azure Analysis Services-server i en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) i Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-104">This quickstart describes using PowerShell from the command line to create an Azure Analysis Services server in an [Azure resource group](../azure-resource-manager/resource-group-overview.md) in your Azure subscription.</span></span>

<span data-ttu-id="bfe7d-105">Den här uppgiften kräver Azure PowerShell-modul version 4.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-105">This task requires Azure PowerShell module version 4.0 or later.</span></span> <span data-ttu-id="bfe7d-106">Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-106">To find the version, run ` Get-Module -ListAvailable AzureRM`.</span></span> <span data-ttu-id="bfe7d-107">Information om att installera och uppgradera finns i [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps) (Installera Azure PowerShell-modul).</span><span class="sxs-lookup"><span data-stu-id="bfe7d-107">To install or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span> 

> [!NOTE]
> <span data-ttu-id="bfe7d-108">Att skapa en server kan resultera i en ny fakturerbar tjänst.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-108">Creating a server might result in a new billable service.</span></span> <span data-ttu-id="bfe7d-109">Läs mer i [Priser för Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="bfe7d-109">To learn more, see [Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bfe7d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="bfe7d-110">Prerequisites</span></span>
<span data-ttu-id="bfe7d-111">Följande krävs för att slutföra den här snabbstarten:</span><span class="sxs-lookup"><span data-stu-id="bfe7d-111">To complete this quickstart, you need:</span></span>

* <span data-ttu-id="bfe7d-112">**Azure-prenumeration**: Gå till [Kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) för att skapa ett konto.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-112">**Azure subscription**: Visit [Azure Free Trial](https://azure.microsoft.com/offers/ms-azr-0044p/) to create an account.</span></span>
* <span data-ttu-id="bfe7d-113">**Azure Active Directory**: Prenumerationen måste vara kopplad till en Azure Active Directory-klientorganisation och du måste ha ett konto i den katalogen.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-113">**Azure Active Directory**: Your subscription must be associated with an Azure Active Directory tenant and you must have an account in that directory.</span></span> <span data-ttu-id="bfe7d-114">Mer information finns i [Autentisering och användarbehörigheter](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="bfe7d-114">To learn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>

## <a name="import-azurermanalysisservices-module"></a><span data-ttu-id="bfe7d-115">Importera AzureRm.AnalysisServices modul</span><span class="sxs-lookup"><span data-stu-id="bfe7d-115">Import AzureRm.AnalysisServices module</span></span>
<span data-ttu-id="bfe7d-116">Använd komponentmodulen [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) när du skapar en server i prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-116">To create a server in your subscription, you use the [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)  component module.</span></span> <span data-ttu-id="bfe7d-117">Läs in AzureRm.AnalysisServices-modulen i PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-117">Load the AzureRm.AnalysisServices module into your PowerShell session.</span></span>

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-to-azure"></a><span data-ttu-id="bfe7d-118">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="bfe7d-118">Sign in to Azure</span></span>

<span data-ttu-id="bfe7d-119">Logga in på Azure-prenumerationen med kommandot [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount).</span><span class="sxs-lookup"><span data-stu-id="bfe7d-119">Sign in to your Azure subscription by using the [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) command.</span></span> <span data-ttu-id="bfe7d-120">Följ anvisningarna på skärmen.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-120">Follow the on-screen directions.</span></span>

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a><span data-ttu-id="bfe7d-121">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="bfe7d-121">Create a resource group</span></span>
 
<span data-ttu-id="bfe7d-122">En [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-122">An [Azure resource group](../azure-resource-manager/resource-group-overview.md) is a logical container where Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="bfe7d-123">När du skapar servern måste du ange en resursgrupp i prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-123">When you create your server, you must specify a resource group in your subscription.</span></span> <span data-ttu-id="bfe7d-124">Om du inte redan har en resursgrupp, skapa en med kommandot [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="bfe7d-124">If you do not already have a resource group, you can create a new one by using the [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> <span data-ttu-id="bfe7d-125">I följande exempel skapas en resursgrupp med namnet `myResourceGroup` i regionen västra USA.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-125">The following example creates a resource group named `myResourceGroup` in the West US region.</span></span>

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a><span data-ttu-id="bfe7d-126">Skapa en server</span><span class="sxs-lookup"><span data-stu-id="bfe7d-126">Create a server</span></span>

<span data-ttu-id="bfe7d-127">Skapa en ny server med kommandot [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver).</span><span class="sxs-lookup"><span data-stu-id="bfe7d-127">Create a new server by using the [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) command.</span></span> <span data-ttu-id="bfe7d-128">I följande exempel skapas en server med namnet myServer i myResourceGroup, i regionen västra USA på nivå D1 och med philipc@adventureworks.com som serveradministratör.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-128">The following example creates a server named myServer in myResourceGroup, in the West US region, at the D1 tier, and specifies philipc@adventureworks.com as a server administrator.</span></span>

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a><span data-ttu-id="bfe7d-129">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="bfe7d-129">Clean up resources</span></span>

<span data-ttu-id="bfe7d-130">Du kan ta bort servern från prenumerationen med kommandot [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver).</span><span class="sxs-lookup"><span data-stu-id="bfe7d-130">You can remove the server from your subscription by using the [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) command.</span></span> <span data-ttu-id="bfe7d-131">Ta inte bort servern om du fortsätter med andra snabbstartsguider och självstudier i den här samlingen.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-131">If you continue with other quickstarts and tutorials in this collection, do not remove your server.</span></span> <span data-ttu-id="bfe7d-132">I följande exempel tar vi bort servern som skapades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="bfe7d-132">The following example removes the server created in the previous step.</span></span>


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a><span data-ttu-id="bfe7d-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bfe7d-133">Next steps</span></span>
<span data-ttu-id="bfe7d-134">[Hantera Azure Analysis Services med PowerShell](analysis-services-powershell.md) </span><span class="sxs-lookup"><span data-stu-id="bfe7d-134">[Manage Azure Analysis Services with PowerShell](analysis-services-powershell.md) </span></span>  
<span data-ttu-id="bfe7d-135">[Distribuera en modell från SSDT](analysis-services-deploy.md) </span><span class="sxs-lookup"><span data-stu-id="bfe7d-135">[Deploy a model from SSDT](analysis-services-deploy.md) </span></span>  
[<span data-ttu-id="bfe7d-136">Skapa en modell i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bfe7d-136">Create a model in Azure portal</span></span>](analysis-services-create-model-portal.md)