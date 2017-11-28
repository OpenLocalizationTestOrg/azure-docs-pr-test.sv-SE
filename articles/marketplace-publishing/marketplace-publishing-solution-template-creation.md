---
title: "aaaGuide toocreating en lösningsmall för hello Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar hur toocreate, certifiera och distribuera en Multi-VM avbildningen Lösningsmall för inköp på hello Azure Marketplace."
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
ms.openlocfilehash: b0e7067176337dd0d3f6f6ec04c963f80f706ab0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guide-toocreate-a-solution-template-for-azure-marketplace"></a><span data-ttu-id="6e2bd-103">Guiden toocreate en för lösningsmall för Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="6e2bd-103">Guide toocreate a solution template for Azure Marketplace</span></span>
<span data-ttu-id="6e2bd-104">När du har slutfört steg 1, [skapande av konton och registrering][link-acct-creation], vi Interaktiv du på hello skapande av en Azure-kompatibel lösningsmall på [tekniska krav för att skapa en lösningsmall](marketplace-publishing-solution-template-creation-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="6e2bd-104">After completing step 1, [Account creation and registration][link-acct-creation], we guided you on hello creation of an Azure-compatible solution template at [Technical prerequisites for creating a solution template](marketplace-publishing-solution-template-creation-prerequisites.md).</span></span> <span data-ttu-id="6e2bd-105">Nu går du igenom hello steg för att skapa en lösningsmall för för flera virtuella datorer på hello [Publiceringsportal] [ link-pubportal] för hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6e2bd-105">Now we will walk you through hello steps for creating a solution template for multiple VMs on hello [Publishing Portal][link-pubportal] for hello Azure Marketplace.</span></span>

## <a name="create-your-solution-template-offer-in-hello-publishing-portal"></a><span data-ttu-id="6e2bd-106">Skapa lösningen mallen erbjudandet i hello Publishing Portal</span><span class="sxs-lookup"><span data-stu-id="6e2bd-106">Create your solution template offer in hello Publishing Portal</span></span>
<span data-ttu-id="6e2bd-107">Gå för [https://publish.windowsazure.com](http://publish.windowsazure.com). När du loggar in för första gången toohello för hello [Publiceringsportal](https://publish.windowsazure.com/), Använd hello samma konto med företagets säljare profil har registrerats.</span><span class="sxs-lookup"><span data-stu-id="6e2bd-107">Go too [https://publish.windowsazure.com](http://publish.windowsazure.com). When signing in for hello first time toohello [Publishing Portal](https://publish.windowsazure.com/), use hello same account with which your company’s seller profile was registered.</span></span> <span data-ttu-id="6e2bd-108">Senare kan du lägga till anställda på företaget som medadministratör i hello Publishing Portal.</span><span class="sxs-lookup"><span data-stu-id="6e2bd-108">Later, you can add any employee of your company as a co-admin in hello Publishing Portal.</span></span>

### <a name="1-select-solution-templates"></a><span data-ttu-id="6e2bd-109">1. Välj ”lösningsmallar”</span><span class="sxs-lookup"><span data-stu-id="6e2bd-109">1. Select "Solution templates"</span></span>
  ![Rita][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a><span data-ttu-id="6e2bd-111">2. Skapa en ny lösningsmall</span><span class="sxs-lookup"><span data-stu-id="6e2bd-111">2. Create a new solution template</span></span>
  ![Rita][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a><span data-ttu-id="6e2bd-113">3. Börja med topologier</span><span class="sxs-lookup"><span data-stu-id="6e2bd-113">3. Start with topologies</span></span>
<span data-ttu-id="6e2bd-114">En lösningsmall är en ”överordnad” tooall dess topologier.</span><span class="sxs-lookup"><span data-stu-id="6e2bd-114">A solution template is a "parent" tooall of its topologies.</span></span> <span data-ttu-id="6e2bd-115">Du kan definiera flera topologier i en erbjudande-/lösningsmall.</span><span class="sxs-lookup"><span data-stu-id="6e2bd-115">You can define multiple topologies in one offer/solution template.</span></span> <span data-ttu-id="6e2bd-116">När ett erbjudande pushas toostaging pushas den med alla sina topologier.</span><span class="sxs-lookup"><span data-stu-id="6e2bd-116">When an offer is pushed toostaging, it is pushed with all of its topologies.</span></span> <span data-ttu-id="6e2bd-117">Gör hello nedan toodefine erbjudandet:</span><span class="sxs-lookup"><span data-stu-id="6e2bd-117">Follow hello steps below toodefine your offer:</span></span>     

* <span data-ttu-id="6e2bd-118">Skapa en topologi: ”ID” är vanligtvis hello namnet på hello topologi för hello lösningsmall.</span><span class="sxs-lookup"><span data-stu-id="6e2bd-118">Create a Topology: “Topology Identifier” is typically hello name of hello topology for hello solution template.</span></span> <span data-ttu-id="6e2bd-119">hello-ID används i hello URL som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="6e2bd-119">hello topology identifier is used in hello URL as shown below:</span></span>

  <span data-ttu-id="6e2bd-120">Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}</span><span class="sxs-lookup"><span data-stu-id="6e2bd-120">Azure Marketplace: http://azure.microsoft.com/marketplace/partners/{PublisherNamespace}/{OfferIdentifier}{TopologyIdentifier}</span></span>

  <span data-ttu-id="6e2bd-121">Azure-portalen: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}</span><span class="sxs-lookup"><span data-stu-id="6e2bd-121">Azure Portal: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{TopologyIdentifier}</span></span>
* <span data-ttu-id="6e2bd-122">Lägg till en ny version.</span><span class="sxs-lookup"><span data-stu-id="6e2bd-122">Add a new version.</span></span>

### <a name="4-get-your-topology-versions-certified"></a><span data-ttu-id="6e2bd-123">4. Hämta din topologi-versioner som är certifierade</span><span class="sxs-lookup"><span data-stu-id="6e2bd-123">4. Get your topology versions certified</span></span>
<span data-ttu-id="6e2bd-124">Överför en zip-fil som innehåller alla nödvändiga filer tooprovision att viss version av hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="6e2bd-124">Upload a zip file that contains all required files tooprovision that particular version of hello topology.</span></span> <span data-ttu-id="6e2bd-125">Den här zipfilen måste innehålla hello följande:</span><span class="sxs-lookup"><span data-stu-id="6e2bd-125">This zip file must contain hello following:</span></span>

* <span data-ttu-id="6e2bd-126">*mainTemplate.json* och *createUiDefinition.json* fil på dess rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="6e2bd-126">*mainTemplate.json* and *createUiDefinition.json* file at its root directory.</span></span>
* <span data-ttu-id="6e2bd-127">Alla länkade mallar och alla nödvändiga skript.</span><span class="sxs-lookup"><span data-stu-id="6e2bd-127">Any linked templates and all required scripts.</span></span>

  > [!TIP]
  > <span data-ttu-id="6e2bd-128">När utvecklarna arbetar på att skapa hello lösning mallen topologier och få dem certifierad, hello business kan marknadsföring och/eller juridiska avdelningar inom företaget arbeta med hello marknadsföring och juridiska innehåll.</span><span class="sxs-lookup"><span data-stu-id="6e2bd-128">While your developers work on creating hello solution template topologies and getting them certified, hello business, marketing, and/or legal departments of your company can work on hello marketing and legal content.</span></span>
  >
  >

## <a name="next-steps"></a><span data-ttu-id="6e2bd-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6e2bd-129">Next steps</span></span>
<span data-ttu-id="6e2bd-130">Nu när du har skapat din lösning och överföra hello zip-filen följer du anvisningarna hello i hello [Marketplace marketing content guiden](marketplace-publishing-push-to-staging.md) innan du skickar hello erbjudande toostaging.</span><span class="sxs-lookup"><span data-stu-id="6e2bd-130">Now that you created your solution template and uploaded hello zip file, please follow hello instructions in hello [Marketplace marketing content guide](marketplace-publishing-push-to-staging.md) before pushing hello offer toostaging.</span></span> <span data-ttu-id="6e2bd-131">toosee hello fullständig uppsättning marketplace publicera artiklar, besök [komma igång: hur toopublish ett erbjudande toohello Azure Marketplace](marketplace-publishing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="6e2bd-131">toosee hello full set of marketplace publishing articles, visit [Getting started: How toopublish an offer toohello Azure Marketplace](marketplace-publishing-getting-started.md).</span></span>

<span data-ttu-id="6e2bd-132">Du kan också vara intresserad av dessa relaterade artiklar:</span><span class="sxs-lookup"><span data-stu-id="6e2bd-132">You might also be interested in these related articles:</span></span>

* <span data-ttu-id="6e2bd-133">VM-avbildningar: [om avbildningar av virtuella datorer i Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)</span><span class="sxs-lookup"><span data-stu-id="6e2bd-133">VM images: [About Virtual Machine Images in Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)</span></span>
* <span data-ttu-id="6e2bd-134">VM-tillägg: [VM-agenten och VM-tillägg översikt](https://msdn.microsoft.com/library/azure/dn832621.aspx) och [Azure VM-tillägg och funktioner](https://msdn.microsoft.com/library/azure/dn606311.aspx)</span><span class="sxs-lookup"><span data-stu-id="6e2bd-134">VM extensions: [VM Agent and VM Extensions Overview](https://msdn.microsoft.com/library/azure/dn832621.aspx) and [Azure VM Extensions and Features](https://msdn.microsoft.com/library/azure/dn606311.aspx)</span></span>
* <span data-ttu-id="6e2bd-135">Azure Resource Manager: [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) och [enkel mall-exempel](https://github.com/rjmax/ArmExamples)</span><span class="sxs-lookup"><span data-stu-id="6e2bd-135">Azure Resource Manager: [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) and [Simple Template Examples](https://github.com/rjmax/ArmExamples)</span></span>
* <span data-ttu-id="6e2bd-136">Storage-konto begränsar: [hur tooMonitor för Storage-konto begränsning](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) och [Premium-lagring](../storage/common/storage-premium-storage.md#scalability-and-performance-targets)</span><span class="sxs-lookup"><span data-stu-id="6e2bd-136">Storage account throttles: [How tooMonitor for Storage Account Throttling](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) and [Premium storage](../storage/common/storage-premium-storage.md#scalability-and-performance-targets)</span></span>

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
