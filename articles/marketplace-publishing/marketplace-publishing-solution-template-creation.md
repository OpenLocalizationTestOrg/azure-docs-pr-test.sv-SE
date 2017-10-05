---
title: "Guide för att skapa en lösningsmall för Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar för hur du skapar, certifiera och distribuera en lösning för flera Virtuella bilden mall för inköp på Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e14e05f2-2385-4ce0-b351-0747cb74ba19
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: b753bfb4bd69bd9aacf4eebd8999397394c28bc4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a><span data-ttu-id="b5082-103">Guide för att skapa en för lösningsmall för Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="b5082-103">Guide to create a solution template for Azure Marketplace</span></span>
<span data-ttu-id="b5082-104">När du har slutfört steg 1, [skapande av konton och registrering][link-acct-creation], vi Interaktiv du vid skapandet av en Azure-kompatibel lösningsmall på [tekniska krav för att skapa en lösningsmall](marketplace-publishing-solution-template-creation-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="b5082-104">After completing step 1, [Account creation and registration][link-acct-creation], we guided you on the creation of an Azure-compatible solution template at [Technical prerequisites for creating a solution template](marketplace-publishing-solution-template-creation-prerequisites.md).</span></span> <span data-ttu-id="b5082-105">Nu går du igenom stegen för att skapa en lösningsmall för för flera virtuella datorer på den [Publiceringsportal] [ link-pubportal] för Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b5082-105">Now we will walk you through the steps for creating a solution template for multiple VMs on the [Publishing Portal][link-pubportal] for the Azure Marketplace.</span></span>

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a><span data-ttu-id="b5082-106">Skapa lösningen mallen erbjudandet i Publishing Portal</span><span class="sxs-lookup"><span data-stu-id="b5082-106">Create your solution template offer in the Publishing Portal</span></span>
<span data-ttu-id="b5082-107">Gå till [https://publish.windowsazure.com](http://publish.windowsazure.com). När du loggar in för första gången och det [Publiceringsportal](https://publish.windowsazure.com/), Använd samma konto med företagets säljare profil har registrerats.</span><span class="sxs-lookup"><span data-stu-id="b5082-107">Go to  [https://publish.windowsazure.com](http://publish.windowsazure.com). When signing in for the first time to the [Publishing Portal](https://publish.windowsazure.com/), use the same account with which your company’s seller profile was registered.</span></span> <span data-ttu-id="b5082-108">Du kan senare lägga till anställda på företaget som medadministratör i Publishing Portal.</span><span class="sxs-lookup"><span data-stu-id="b5082-108">Later, you can add any employee of your company as a co-admin in the Publishing Portal.</span></span>

### <a name="1-select-solution-templates"></a><span data-ttu-id="b5082-109">1. Välj ”lösningsmallar”</span><span class="sxs-lookup"><span data-stu-id="b5082-109">1. Select "Solution templates"</span></span>
  ![Rita][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a><span data-ttu-id="b5082-111">2. Skapa en ny lösningsmall</span><span class="sxs-lookup"><span data-stu-id="b5082-111">2. Create a new solution template</span></span>
  ![Rita][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a><span data-ttu-id="b5082-113">3. Börja med topologier</span><span class="sxs-lookup"><span data-stu-id="b5082-113">3. Start with topologies</span></span>
<span data-ttu-id="b5082-114">En lösningsmall är "överordnad" alla dess topologier.</span><span class="sxs-lookup"><span data-stu-id="b5082-114">A solution template is a "parent" to all of its topologies.</span></span> <span data-ttu-id="b5082-115">Du kan definiera flera topologier i en erbjudande-/lösningsmall.</span><span class="sxs-lookup"><span data-stu-id="b5082-115">You can define multiple topologies in one offer/solution template.</span></span> <span data-ttu-id="b5082-116">När ett erbjudande pushas till mellanlagring pushas den med alla sina topologier.</span><span class="sxs-lookup"><span data-stu-id="b5082-116">When an offer is pushed to staging, it is pushed with all of its topologies.</span></span> <span data-ttu-id="b5082-117">Följ stegen nedan för att definiera erbjudandet:</span><span class="sxs-lookup"><span data-stu-id="b5082-117">Follow the steps below to define your offer:</span></span>     

* <span data-ttu-id="b5082-118">Skapa en topologi: ”ID” är vanligtvis namnet på topologin för lösningsmall.</span><span class="sxs-lookup"><span data-stu-id="b5082-118">Create a Topology: “Topology Identifier” is typically the name of the topology for the solution template.</span></span> <span data-ttu-id="b5082-119">Topologi-identifieraren används i URL: en som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="b5082-119">The topology identifier is used in the URL as shown below:</span></span>

  <span data-ttu-id="b5082-120">Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}</span><span class="sxs-lookup"><span data-stu-id="b5082-120">Azure Marketplace: http://azure.microsoft.com/marketplace/partners/{PublisherNamespace}/{OfferIdentifier}{TopologyIdentifier}</span></span>

  <span data-ttu-id="b5082-121">Azure-portalen: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}</span><span class="sxs-lookup"><span data-stu-id="b5082-121">Azure Portal: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{TopologyIdentifier}</span></span>
* <span data-ttu-id="b5082-122">Lägg till en ny version.</span><span class="sxs-lookup"><span data-stu-id="b5082-122">Add a new version.</span></span>

### <a name="4-get-your-topology-versions-certified"></a><span data-ttu-id="b5082-123">4. Hämta din topologi-versioner som är certifierade</span><span class="sxs-lookup"><span data-stu-id="b5082-123">4. Get your topology versions certified</span></span>
<span data-ttu-id="b5082-124">Överför en zip-fil som innehåller alla filer som krävs för att etablera den viss versionen av topologin.</span><span class="sxs-lookup"><span data-stu-id="b5082-124">Upload a zip file that contains all required files to provision that particular version of the topology.</span></span> <span data-ttu-id="b5082-125">Den här zipfilen måste innehålla följande:</span><span class="sxs-lookup"><span data-stu-id="b5082-125">This zip file must contain the following:</span></span>

* <span data-ttu-id="b5082-126">*mainTemplate.json* och *createUiDefinition.json* fil på dess rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="b5082-126">*mainTemplate.json* and *createUiDefinition.json* file at its root directory.</span></span>
* <span data-ttu-id="b5082-127">Alla länkade mallar och alla nödvändiga skript.</span><span class="sxs-lookup"><span data-stu-id="b5082-127">Any linked templates and all required scripts.</span></span>

  > [!TIP]
  > <span data-ttu-id="b5082-128">När utvecklarna arbetar på att skapa lösningen mallen topologier och få dem certifierad, företag, kan marknadsföring och/eller juridiska avdelningar inom företaget arbeta med marknadsföring och juridiska innehållet.</span><span class="sxs-lookup"><span data-stu-id="b5082-128">While your developers work on creating the solution template topologies and getting them certified, the business, marketing, and/or legal departments of your company can work on the marketing and legal content.</span></span>
  >
  >

## <a name="next-steps"></a><span data-ttu-id="b5082-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5082-129">Next steps</span></span>
<span data-ttu-id="b5082-130">Nu när du har skapat din lösning och upp zip-filen, följ instruktionerna i den [Marketplace marketing content guiden](marketplace-publishing-push-to-staging.md) innan du skickar erbjudandet till Förproduktion.</span><span class="sxs-lookup"><span data-stu-id="b5082-130">Now that you created your solution template and uploaded the zip file, please follow the instructions in the [Marketplace marketing content guide](marketplace-publishing-push-to-staging.md) before pushing the offer to staging.</span></span> <span data-ttu-id="b5082-131">En fullständig uppsättning marketplace publicera artiklar finns [komma igång: hur du publicerar ett erbjudande på Azure Marketplace](marketplace-publishing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="b5082-131">To see the full set of marketplace publishing articles, visit [Getting started: How to publish an offer to the Azure Marketplace](marketplace-publishing-getting-started.md).</span></span>

<span data-ttu-id="b5082-132">Du kan också vara intresserad av dessa relaterade artiklar:</span><span class="sxs-lookup"><span data-stu-id="b5082-132">You might also be interested in these related articles:</span></span>

* <span data-ttu-id="b5082-133">VM-avbildningar: [om avbildningar av virtuella datorer i Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)</span><span class="sxs-lookup"><span data-stu-id="b5082-133">VM images: [About Virtual Machine Images in Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)</span></span>
* <span data-ttu-id="b5082-134">VM-tillägg: [VM-agenten och VM-tillägg översikt](https://msdn.microsoft.com/library/azure/dn832621.aspx) och [Azure VM-tillägg och funktioner](https://msdn.microsoft.com/library/azure/dn606311.aspx)</span><span class="sxs-lookup"><span data-stu-id="b5082-134">VM extensions: [VM Agent and VM Extensions Overview](https://msdn.microsoft.com/library/azure/dn832621.aspx) and [Azure VM Extensions and Features](https://msdn.microsoft.com/library/azure/dn606311.aspx)</span></span>
* <span data-ttu-id="b5082-135">Azure Resource Manager: [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) och [enkel mall-exempel](https://github.com/rjmax/ArmExamples)</span><span class="sxs-lookup"><span data-stu-id="b5082-135">Azure Resource Manager: [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) and [Simple Template Examples](https://github.com/rjmax/ArmExamples)</span></span>
* <span data-ttu-id="b5082-136">Storage-konto begränsar: [övervaka för Storage-konto begränsning](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) och [Premium-lagring](../storage/common/storage-premium-storage.md#scalability-and-performance-targets)</span><span class="sxs-lookup"><span data-stu-id="b5082-136">Storage account throttles: [How to Monitor for Storage Account Throttling](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) and [Premium storage](../storage/common/storage-premium-storage.md#scalability-and-performance-targets)</span></span>

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
