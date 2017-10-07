---
title: "aaaCreate Azure Analysis Services-servern med hjälp av PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate ett Azure Analysis Services-server med hjälp av PowerShell"
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
ms.openlocfilehash: 269b78983410f773d47c4cea34d6d353b19f9e91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a><span data-ttu-id="8b8d6-103">Skapa en Azure Analysis Services-server med PowerShell</span><span class="sxs-lookup"><span data-stu-id="8b8d6-103">Create an Azure Analysis Services server by using PowerShell</span></span>

<span data-ttu-id="8b8d6-104">Denna Snabbstart beskriver hur du använder PowerShell från hello kommandoraden toocreate en Azure Analysis Services-server i en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-104">This quickstart describes using PowerShell from hello command line toocreate an Azure Analysis Services server in an [Azure resource group](../azure-resource-manager/resource-group-overview.md) in your Azure subscription.</span></span>

<span data-ttu-id="8b8d6-105">Den här uppgiften kräver Azure PowerShell-modul version 4.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-105">This task requires Azure PowerShell module version 4.0 or later.</span></span> <span data-ttu-id="8b8d6-106">toofind hello version, kör ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-106">toofind hello version, run ` Get-Module -ListAvailable AzureRM`.</span></span> <span data-ttu-id="8b8d6-107">tooinstall eller uppgradering, se [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="8b8d6-107">tooinstall or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span> 

> [!NOTE]
> <span data-ttu-id="8b8d6-108">Att skapa en server kan resultera i en ny fakturerbar tjänst.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-108">Creating a server might result in a new billable service.</span></span> <span data-ttu-id="8b8d6-109">Det finns fler toolearn [priser för Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="8b8d6-109">toolearn more, see [Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8b8d6-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8b8d6-110">Prerequisites</span></span>
<span data-ttu-id="8b8d6-111">toocomplete Snabbstart, behöver du:</span><span class="sxs-lookup"><span data-stu-id="8b8d6-111">toocomplete this quickstart, you need:</span></span>

* <span data-ttu-id="8b8d6-112">**Azure-prenumeration**: Besök [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate ett konto.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-112">**Azure subscription**: Visit [Azure Free Trial](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate an account.</span></span>
* <span data-ttu-id="8b8d6-113">**Azure Active Directory**: Prenumerationen måste vara kopplad till en Azure Active Directory-klientorganisation och du måste ha ett konto i den katalogen.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-113">**Azure Active Directory**: Your subscription must be associated with an Azure Active Directory tenant and you must have an account in that directory.</span></span> <span data-ttu-id="8b8d6-114">Det finns fler toolearn [autentisering och användarbehörigheter](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="8b8d6-114">toolearn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>

## <a name="import-azurermanalysisservices-module"></a><span data-ttu-id="8b8d6-115">Importera AzureRm.AnalysisServices modul</span><span class="sxs-lookup"><span data-stu-id="8b8d6-115">Import AzureRm.AnalysisServices module</span></span>
<span data-ttu-id="8b8d6-116">toocreate en server i din prenumeration som du använder hello [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) komponent modulen.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-116">toocreate a server in your subscription, you use hello [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)  component module.</span></span> <span data-ttu-id="8b8d6-117">Läsa hello AzureRm.AnalysisServices modulen i PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-117">Load hello AzureRm.AnalysisServices module into your PowerShell session.</span></span>

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-tooazure"></a><span data-ttu-id="8b8d6-118">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="8b8d6-118">Sign in tooAzure</span></span>

<span data-ttu-id="8b8d6-119">Logga in på tooyour Azure-prenumeration med hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) kommando.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-119">Sign in tooyour Azure subscription by using hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) command.</span></span> <span data-ttu-id="8b8d6-120">Följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-120">Follow hello on-screen directions.</span></span>

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a><span data-ttu-id="8b8d6-121">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="8b8d6-121">Create a resource group</span></span>
 
<span data-ttu-id="8b8d6-122">En [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-122">An [Azure resource group](../azure-resource-manager/resource-group-overview.md) is a logical container where Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="8b8d6-123">När du skapar servern måste du ange en resursgrupp i prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-123">When you create your server, you must specify a resource group in your subscription.</span></span> <span data-ttu-id="8b8d6-124">Om du inte redan har en resursgrupp kan du skapa en ny genom att använda hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) kommando.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-124">If you do not already have a resource group, you can create a new one by using hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> <span data-ttu-id="8b8d6-125">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello västra USA region.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-125">hello following example creates a resource group named `myResourceGroup` in hello West US region.</span></span>

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a><span data-ttu-id="8b8d6-126">Skapa en server</span><span class="sxs-lookup"><span data-stu-id="8b8d6-126">Create a server</span></span>

<span data-ttu-id="8b8d6-127">Skapa en ny server med hjälp av hello [ny AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) kommando.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-127">Create a new server by using hello [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) command.</span></span> <span data-ttu-id="8b8d6-128">hello följande exempel skapas en server med namnet minserver i myResourceGroup i hello västra USA region på hello D1 lager, och anger philipc@adventureworks.com som en serveradministratör.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-128">hello following example creates a server named myServer in myResourceGroup, in hello West US region, at hello D1 tier, and specifies philipc@adventureworks.com as a server administrator.</span></span>

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a><span data-ttu-id="8b8d6-129">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="8b8d6-129">Clean up resources</span></span>

<span data-ttu-id="8b8d6-130">Du kan ta bort hello server från prenumerationen med hello [ta bort AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) kommando.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-130">You can remove hello server from your subscription by using hello [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) command.</span></span> <span data-ttu-id="8b8d6-131">Ta inte bort servern om du fortsätter med andra snabbstartsguider och självstudier i den här samlingen.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-131">If you continue with other quickstarts and tutorials in this collection, do not remove your server.</span></span> <span data-ttu-id="8b8d6-132">hello följande exempel tar bort hello server skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="8b8d6-132">hello following example removes hello server created in hello previous step.</span></span>


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a><span data-ttu-id="8b8d6-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8b8d6-133">Next steps</span></span>
<span data-ttu-id="8b8d6-134">[Hantera Azure Analysis Services med PowerShell](analysis-services-powershell.md) </span><span class="sxs-lookup"><span data-stu-id="8b8d6-134">[Manage Azure Analysis Services with PowerShell](analysis-services-powershell.md) </span></span>  
<span data-ttu-id="8b8d6-135">[Distribuera en modell från SSDT](analysis-services-deploy.md) </span><span class="sxs-lookup"><span data-stu-id="8b8d6-135">[Deploy a model from SSDT](analysis-services-deploy.md) </span></span>  
[<span data-ttu-id="8b8d6-136">Skapa en modell i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8b8d6-136">Create a model in Azure portal</span></span>](analysis-services-create-model-portal.md)