---
title: aaaAzure hanterade program i hello Marketplace | Microsoft Docs
description: "Beskriver Azure hanterade program som är tillgängliga via hello Marketplace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: b3cdf3f1fccdd47db699e4892ae8bce35118bfd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-managed-applications-in-hello-marketplace"></a><span data-ttu-id="c81d1-103">Azure hanterade program i hello Marketplace</span><span class="sxs-lookup"><span data-stu-id="c81d1-103">Azure managed applications in hello Marketplace</span></span>

 <span data-ttu-id="c81d1-104">MSPs ISV: er och systemintegrerare (SIs) kan använda Azure hanterade program toooffer kunderna lösningar tooall Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c81d1-104">MSPs, ISVs, and system integrators (SIs) can use Azure managed applications toooffer their solutions tooall Azure Marketplace customers.</span></span> <span data-ttu-id="c81d1-105">Sådana lösningar minska hello Service och underhåll kostnader för kunder.</span><span class="sxs-lookup"><span data-stu-id="c81d1-105">Such solutions reduce hello maintenance and servicing overhead for customers.</span></span> <span data-ttu-id="c81d1-106">Utgivare kan sälja infrastruktur- och programvara via hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c81d1-106">Publishers can sell infrastructure and software through hello Marketplace.</span></span> <span data-ttu-id="c81d1-107">De kan koppla tjänster och operativa stöd toomanaged program.</span><span class="sxs-lookup"><span data-stu-id="c81d1-107">They can attach services and operational support toomanaged applications.</span></span> <span data-ttu-id="c81d1-108">Mer information finns i [hanteras Programöversikt](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c81d1-108">For more information, see [Managed application overview](managed-application-overview.md).</span></span>

<span data-ttu-id="c81d1-109">Den här artikeln beskrivs hur en MSP, ISV eller SI kan publicera ett program toohello Marketplace och gör den tillgänglig toocustomers.</span><span class="sxs-lookup"><span data-stu-id="c81d1-109">This article explains how an MSP, ISV, or SI can publish an application toohello Marketplace and make it broadly available toocustomers.</span></span>

## <a name="prerequisites-for-publishing-a-managed-application"></a><span data-ttu-id="c81d1-110">Krav för att publicera ett hanterat program</span><span class="sxs-lookup"><span data-stu-id="c81d1-110">Prerequisites for publishing a managed application</span></span>

<span data-ttu-id="c81d1-111">Krav för toolisting i hello Marketplace:</span><span class="sxs-lookup"><span data-stu-id="c81d1-111">Prerequisites toolisting in hello Marketplace:</span></span>

* <span data-ttu-id="c81d1-112">Tekniska</span><span class="sxs-lookup"><span data-stu-id="c81d1-112">Technical</span></span>

    *  <span data-ttu-id="c81d1-113">Information om hello grundläggande struktur och syntaxen för Azure Resource Manager-mallar finns [Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="c81d1-113">For information about hello basic structure and syntax of Azure Resource Manager templates, see [Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
    *  <span data-ttu-id="c81d1-114">tooview fullständig mallen lösningar finns [Azure-snabbstartsmallar](https://azure.microsoft.com/en-us/documentation/templates/) eller hello [Quickstart mallen databasen](https://github.com/azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="c81d1-114">tooview complete template solutions, see [Azure Quickstart templates](https://azure.microsoft.com/en-us/documentation/templates/) or hello [Quickstart template repository](https://github.com/azure/azure-quickstart-templates).</span></span>
    *  <span data-ttu-id="c81d1-115">Information om hur toocreate hello gränssnitt för kunder som distribuerar ditt program via hello Marketplace finns [skapa en fil för användaren gränssnittsdefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c81d1-115">For information about how toocreate hello interface for customers who deploy your application through hello Marketplace, see [Create a user interface definition file](managed-application-createuidefinition-overview.md).</span></span>

* <span data-ttu-id="c81d1-116">Sökassistent (affärsbehov)</span><span class="sxs-lookup"><span data-stu-id="c81d1-116">Nontechnical (business requirements)</span></span>

    *   <span data-ttu-id="c81d1-117">Ditt företag eller dess dotterbolag måste finnas i ett land där försäljning stöds av hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c81d1-117">Your company or its subsidiary must be located in a country where sales are supported by hello Marketplace.</span></span>
    *   <span data-ttu-id="c81d1-118">Produkten måste ha licens på ett sätt som är kompatibel med fakturering modeller som stöds av hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c81d1-118">Your product must be licensed in a way that is compatible with billing models supported by hello Marketplace.</span></span>
    *   <span data-ttu-id="c81d1-119">Du är ansvarig för att teknisk support tillgängliga toocustomers på ett kommersiellt rimliga sätt.</span><span class="sxs-lookup"><span data-stu-id="c81d1-119">You're responsible for making technical support available toocustomers in a commercially reasonable manner.</span></span> <span data-ttu-id="c81d1-120">hello stöd kan vara fria, betald, eller via community stöder.</span><span class="sxs-lookup"><span data-stu-id="c81d1-120">hello support can be free, paid, or through community support.</span></span>
    *   <span data-ttu-id="c81d1-121">Du är ansvarig för att licensiera programmet och eventuella beroenden för programvara från tredje part.</span><span class="sxs-lookup"><span data-stu-id="c81d1-121">You're responsible for licensing your software and any third-party software dependencies.</span></span>
    *   <span data-ttu-id="c81d1-122">Du måste ange innehåll som uppfyller villkoren för ditt erbjudande toobe som visas i hello Marketplace och hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c81d1-122">You must provide content that meets criteria for your offering toobe listed in hello Marketplace and in hello Azure portal.</span></span>
    *   <span data-ttu-id="c81d1-123">Du måste acceptera toohello villkoren i hello Azure Marketplace deltagande principer eller utgivare avtal.</span><span class="sxs-lookup"><span data-stu-id="c81d1-123">You must agree toohello terms of hello Azure Marketplace Participation Policies and Publisher Agreement.</span></span>
    *   <span data-ttu-id="c81d1-124">Du måste acceptera toocomply med hello användningsvillkoren och sekretesspolicyn för Microsoft certifierad programmet avtalet för Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c81d1-124">You must agree toocomply with hello Terms of Use, Microsoft Privacy Statement, and Microsoft Azure Certified Program Agreement.</span></span>

## <a name="create-a-new-azure-application-offer"></a><span data-ttu-id="c81d1-125">Skapa ett nytt erbjudande för Azure-program</span><span class="sxs-lookup"><span data-stu-id="c81d1-125">Create a new Azure application offer</span></span>

<span data-ttu-id="c81d1-126">Efter att du uppfyller hello förutsättningar du är klar toocreate erbjudandet hanterade program.</span><span class="sxs-lookup"><span data-stu-id="c81d1-126">After you meet hello prerequisites, you're ready toocreate your managed application offer.</span></span> <span data-ttu-id="c81d1-127">Låt oss ta en snabb överblick över ett erbjudande och en SKU.</span><span class="sxs-lookup"><span data-stu-id="c81d1-127">Let's take a quick overview of an offer and a SKU.</span></span>

### <a name="offer"></a><span data-ttu-id="c81d1-128">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="c81d1-128">Offer</span></span>

<span data-ttu-id="c81d1-129">hello-erbjudande för ett hanterat program motsvarar tooa klass av produkten erbjudande från en utgivare.</span><span class="sxs-lookup"><span data-stu-id="c81d1-129">hello offer for a managed application corresponds tooa class of product offering from a publisher.</span></span> <span data-ttu-id="c81d1-130">Om du har en ny typ av lösningen-program som du vill toomake som är tillgängliga i hello Marketplace kan konfigurera du den som ett nytt erbjudande.</span><span class="sxs-lookup"><span data-stu-id="c81d1-130">If you have a new type of solution/application that you want toomake available in hello Marketplace, you can set it up as a new offer.</span></span> <span data-ttu-id="c81d1-131">Ett erbjudande är en samling av SKU: er.</span><span class="sxs-lookup"><span data-stu-id="c81d1-131">An offer is a collection of SKUs.</span></span> <span data-ttu-id="c81d1-132">Varje erbjudande visas som sin egen enhet i hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c81d1-132">Every offer appears as its own entity in hello Marketplace.</span></span>

### <a name="sku"></a><span data-ttu-id="c81d1-133">SKU</span><span class="sxs-lookup"><span data-stu-id="c81d1-133">SKU</span></span>

<span data-ttu-id="c81d1-134">En SKU är hello minsta köpbara ett erbjudande.</span><span class="sxs-lookup"><span data-stu-id="c81d1-134">A SKU is hello smallest purchasable unit of an offer.</span></span> <span data-ttu-id="c81d1-135">Du kan använda en SKU inom hello samma produkten klass (erbjudandet) toodifferentiate mellan:</span><span class="sxs-lookup"><span data-stu-id="c81d1-135">You can use a SKU within hello same product class (offer) toodifferentiate between:</span></span>

* <span data-ttu-id="c81d1-136">Olika funktioner som stöds.</span><span class="sxs-lookup"><span data-stu-id="c81d1-136">Different features that are supported.</span></span>
* <span data-ttu-id="c81d1-137">Om hello erbjudandet är hanterad eller ohanterad.</span><span class="sxs-lookup"><span data-stu-id="c81d1-137">Whether hello offer is managed or unmanaged.</span></span>
* <span data-ttu-id="c81d1-138">Fakturering modeller som stöds.</span><span class="sxs-lookup"><span data-stu-id="c81d1-138">Billing models that are supported.</span></span>

<span data-ttu-id="c81d1-139">En SKU visas under hello överordnade erbjudandet i hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c81d1-139">A SKU appears under hello parent offer in hello Marketplace.</span></span> <span data-ttu-id="c81d1-140">Det verkar som sin egen köpbara entitet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c81d1-140">It appears as its own purchasable entity in hello Azure portal.</span></span>

### <a name="set-up-an-offer"></a><span data-ttu-id="c81d1-141">Konfigurera ett erbjudande</span><span class="sxs-lookup"><span data-stu-id="c81d1-141">Set up an offer</span></span>

1. <span data-ttu-id="c81d1-142">Logga in toohello [molnpartnerportalen](https://cloudpartner.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c81d1-142">Sign in toohello [Cloud Partner portal](https://cloudpartner.azure.com/).</span></span>

2. <span data-ttu-id="c81d1-143">Hello navigeringsfönstret hello vänster och välj **+ nytt erbjudande** > **Azure program**.</span><span class="sxs-lookup"><span data-stu-id="c81d1-143">In hello navigation pane on hello left, select **+ New offer** > **Azure Applications**.</span></span>

    ![Nytt erbjudande](./media/managed-application-author-marketplace/newOffer.png)

3. <span data-ttu-id="c81d1-145">Fylla i hello formulär som visas på hello kvar i hello **Editor** vyn.</span><span class="sxs-lookup"><span data-stu-id="c81d1-145">Fill out hello forms that appear on hello left in hello **Editor** view.</span></span> <span data-ttu-id="c81d1-146">Obligatoriska fält är markerade med en röd asterisk (*).</span><span class="sxs-lookup"><span data-stu-id="c81d1-146">Required fields are marked with a red asterisk (*).</span></span>

    ![Inställningar för erbjudande](./media/managed-application-author-marketplace/newOffer_OfferSettings.png)

    <span data-ttu-id="c81d1-148">Fyra huvudsakliga formulär är används toocreate ett hanterat program:</span><span class="sxs-lookup"><span data-stu-id="c81d1-148">Four main forms are used toocreate a managed application:</span></span>

    <span data-ttu-id="c81d1-149">a.</span><span class="sxs-lookup"><span data-stu-id="c81d1-149">a.</span></span> <span data-ttu-id="c81d1-150">Inställningar för erbjudande</span><span class="sxs-lookup"><span data-stu-id="c81d1-150">Offer Settings</span></span>

    <span data-ttu-id="c81d1-151">b.</span><span class="sxs-lookup"><span data-stu-id="c81d1-151">b.</span></span> <span data-ttu-id="c81d1-152">SKU: er</span><span class="sxs-lookup"><span data-stu-id="c81d1-152">SKUs</span></span>

    <span data-ttu-id="c81d1-153">c.</span><span class="sxs-lookup"><span data-stu-id="c81d1-153">c.</span></span> <span data-ttu-id="c81d1-154">Marketplace</span><span class="sxs-lookup"><span data-stu-id="c81d1-154">Marketplace</span></span>

    <span data-ttu-id="c81d1-155">d.</span><span class="sxs-lookup"><span data-stu-id="c81d1-155">d.</span></span> <span data-ttu-id="c81d1-156">Support</span><span class="sxs-lookup"><span data-stu-id="c81d1-156">Support</span></span>

<span data-ttu-id="c81d1-157">Formulären beskrivs mer detaljerat i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="c81d1-157">These forms are described in greater detail in hello following sections.</span></span>

## <a name="offer-settings-form"></a><span data-ttu-id="c81d1-158">Erbjudande inställningar för formulär</span><span class="sxs-lookup"><span data-stu-id="c81d1-158">Offer Settings form</span></span>
<span data-ttu-id="c81d1-159">Använd den här grundformat toospecify hello erbjudande inställningarna.</span><span class="sxs-lookup"><span data-stu-id="c81d1-159">Use this basic form toospecify hello offer settings.</span></span>

1. <span data-ttu-id="c81d1-160">Fyll i hello **erbjuder inställningar** formuläret.</span><span class="sxs-lookup"><span data-stu-id="c81d1-160">Fill in hello **Offer Settings** form.</span></span> <span data-ttu-id="c81d1-161">hello olika fält är:</span><span class="sxs-lookup"><span data-stu-id="c81d1-161">hello different fields are:</span></span>

    <span data-ttu-id="c81d1-162">a.</span><span class="sxs-lookup"><span data-stu-id="c81d1-162">a.</span></span> <span data-ttu-id="c81d1-163">**Erbjudande-ID**: den unika identifieraren identifierar hello erbjudandet i en profil för utgivaren.</span><span class="sxs-lookup"><span data-stu-id="c81d1-163">**Offer ID**: This unique identifier identifies hello offer within a publisher profile.</span></span> <span data-ttu-id="c81d1-164">Detta ID visas i produkten URL: er, Resource Manager-mallar och fakturering rapporter.</span><span class="sxs-lookup"><span data-stu-id="c81d1-164">This ID is visible in product URLs, Resource Manager templates, and billing reports.</span></span> <span data-ttu-id="c81d1-165">Det kan bara består av gemena alfanumeriska tecken och bindestreck (-).</span><span class="sxs-lookup"><span data-stu-id="c81d1-165">It can only be composed of lowercase alphanumeric characters or dashes (-).</span></span> <span data-ttu-id="c81d1-166">hello-ID får inte sluta med ett streck.</span><span class="sxs-lookup"><span data-stu-id="c81d1-166">hello ID can't end in a dash.</span></span> <span data-ttu-id="c81d1-167">Det är begränsad tooa högst 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="c81d1-167">It's limited tooa maximum of 50 characters.</span></span> <span data-ttu-id="c81d1-168">När ett erbjudande lanseras är i det här fältet låst.</span><span class="sxs-lookup"><span data-stu-id="c81d1-168">After an offer goes live, this field is locked.</span></span>

    <span data-ttu-id="c81d1-169">b.</span><span class="sxs-lookup"><span data-stu-id="c81d1-169">b.</span></span> <span data-ttu-id="c81d1-170">**Publicerings-ID**: Använd den här listrutan toochoose hello publisher profilen du vill toopublish erbjudandet under.</span><span class="sxs-lookup"><span data-stu-id="c81d1-170">**Publisher ID**: Use this drop-down list toochoose hello publisher profile you want toopublish this offer under.</span></span> <span data-ttu-id="c81d1-171">När ett erbjudande lanseras är i det här fältet låst.</span><span class="sxs-lookup"><span data-stu-id="c81d1-171">After an offer goes live, this field is locked.</span></span>

    <span data-ttu-id="c81d1-172">c.</span><span class="sxs-lookup"><span data-stu-id="c81d1-172">c.</span></span> <span data-ttu-id="c81d1-173">**Namnet**: det här visningsnamnet för ditt erbjudande visas i hello Marketplace och hello portal.</span><span class="sxs-lookup"><span data-stu-id="c81d1-173">**Name**: This display name for your offer appears in hello Marketplace and in hello portal.</span></span> <span data-ttu-id="c81d1-174">Det kan ha högst 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="c81d1-174">It can have a maximum of 50 characters.</span></span> <span data-ttu-id="c81d1-175">Innehåller ett okänt varumärke för produkten.</span><span class="sxs-lookup"><span data-stu-id="c81d1-175">Include a recognizable brand name for your product.</span></span> <span data-ttu-id="c81d1-176">Inkludera inte ditt företagsnamn, om det inte är hur släpps.</span><span class="sxs-lookup"><span data-stu-id="c81d1-176">Don't include your company name here unless that's how it's marketed.</span></span> <span data-ttu-id="c81d1-177">Om du marknadsföring erbjudandet på din egen webbplats, kan du kontrollera att hello-namn är exakt hur den visas på webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="c81d1-177">If you're marketing this offer on your own website, ensure that hello name is exactly how it appears on your website.</span></span>

2. <span data-ttu-id="c81d1-178">Välj **spara** toosave ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="c81d1-178">Select **Save** toosave your progress.</span></span> 

## <a name="skus-form"></a><span data-ttu-id="c81d1-179">SKU: er formulär</span><span class="sxs-lookup"><span data-stu-id="c81d1-179">SKUs form</span></span>
<span data-ttu-id="c81d1-180">hello nästa steg är tooadd SKU: er för ditt erbjudande.</span><span class="sxs-lookup"><span data-stu-id="c81d1-180">hello next step is tooadd SKUs for your offer.</span></span>

1. <span data-ttu-id="c81d1-181">Välj **SKU: er** > **nya SKU**.</span><span class="sxs-lookup"><span data-stu-id="c81d1-181">Select **SKUs** > **New SKU**.</span></span> 

    ![Välj ny SKU](./media/managed-application-author-marketplace/newOffer_skus.png)

2. <span data-ttu-id="c81d1-183">Ange en **SKU ID**.</span><span class="sxs-lookup"><span data-stu-id="c81d1-183">Enter a **SKU ID**.</span></span> <span data-ttu-id="c81d1-184">En SKU-ID är en unik identifierare för hello SKU inom ett erbjudande.</span><span class="sxs-lookup"><span data-stu-id="c81d1-184">A SKU ID is a unique identifier for hello SKU within an offer.</span></span> <span data-ttu-id="c81d1-185">Detta ID visas i produkten URL: er, Resource Manager-mallar och fakturering rapporter.</span><span class="sxs-lookup"><span data-stu-id="c81d1-185">This ID is visible in product URLs, Resource Manager templates, and billing reports.</span></span> <span data-ttu-id="c81d1-186">Det kan bara består av gemena alfanumeriska tecken och bindestreck (-).</span><span class="sxs-lookup"><span data-stu-id="c81d1-186">It can only be composed of lowercase alphanumeric characters or dashes (-).</span></span> <span data-ttu-id="c81d1-187">hello-ID får inte sluta med ett streck och är begränsat tooa högst 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="c81d1-187">hello ID can't end in a dash, and it's limited tooa maximum of 50 characters.</span></span> <span data-ttu-id="c81d1-188">När ett erbjudande lanseras är i det här fältet låst.</span><span class="sxs-lookup"><span data-stu-id="c81d1-188">After an offer goes live, this field is locked.</span></span> <span data-ttu-id="c81d1-189">Du kan ha flera SKU: er i ett erbjudande.</span><span class="sxs-lookup"><span data-stu-id="c81d1-189">You can have multiple SKUs within an offer.</span></span> <span data-ttu-id="c81d1-190">En SKU måste du planera toopublish för varje avbildning.</span><span class="sxs-lookup"><span data-stu-id="c81d1-190">You need a SKU for each image you plan toopublish.</span></span>

3. <span data-ttu-id="c81d1-191">Fylla hello **SKU information** avsnittet hello följande format:</span><span class="sxs-lookup"><span data-stu-id="c81d1-191">Fill out hello **SKU Details** section on hello following form:</span></span>

    ![Ange nya SKU](./media/managed-application-author-marketplace/newOffer_newsku.png)

    <span data-ttu-id="c81d1-193">Fyll ut hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="c81d1-193">Fill out hello following fields:</span></span>
    
    <span data-ttu-id="c81d1-194">a.</span><span class="sxs-lookup"><span data-stu-id="c81d1-194">a.</span></span> <span data-ttu-id="c81d1-195">**Rubrik**: Ange en rubrik för denna SKU.</span><span class="sxs-lookup"><span data-stu-id="c81d1-195">**Title**: Enter a title for this SKU.</span></span> <span data-ttu-id="c81d1-196">Den här rubriken visas i hello-galleriet för det här objektet.</span><span class="sxs-lookup"><span data-stu-id="c81d1-196">This title appears in hello gallery for this item.</span></span>

    <span data-ttu-id="c81d1-197">b.</span><span class="sxs-lookup"><span data-stu-id="c81d1-197">b.</span></span> <span data-ttu-id="c81d1-198">**Översikt över**: Ange en kort sammanfattning för denna SKU.</span><span class="sxs-lookup"><span data-stu-id="c81d1-198">**Summary**: Enter a short summary for this SKU.</span></span> <span data-ttu-id="c81d1-199">Den här texten visas under hello rubrik.</span><span class="sxs-lookup"><span data-stu-id="c81d1-199">This text appears underneath hello title.</span></span>

    <span data-ttu-id="c81d1-200">c.</span><span class="sxs-lookup"><span data-stu-id="c81d1-200">c.</span></span> <span data-ttu-id="c81d1-201">**Beskrivning**: Ange en detaljerad beskrivning om hello SKU.</span><span class="sxs-lookup"><span data-stu-id="c81d1-201">**Description**: Enter a detailed description about hello SKU.</span></span>

    <span data-ttu-id="c81d1-202">d.</span><span class="sxs-lookup"><span data-stu-id="c81d1-202">d.</span></span> <span data-ttu-id="c81d1-203">**SKU-typen**: hello tillåtna värden är **hanterat program** och **Lösningsmallar**.</span><span class="sxs-lookup"><span data-stu-id="c81d1-203">**SKU Type**: hello allowed values are **Managed Application** and **Solution Templates**.</span></span> <span data-ttu-id="c81d1-204">Det här fallet markerar **hanterat program**.</span><span class="sxs-lookup"><span data-stu-id="c81d1-204">For this case, select **Managed Application**.</span></span>

4. <span data-ttu-id="c81d1-205">Fylla hello **Paketinformation** avsnittet hello följande format:</span><span class="sxs-lookup"><span data-stu-id="c81d1-205">Fill out hello **Package Details** section on hello following form:</span></span>

    ![Paket](./media/managed-application-author-marketplace/newOffer_newsku_package.png)

    <span data-ttu-id="c81d1-207">Fyll ut hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="c81d1-207">Fill out hello following fields:</span></span>

    <span data-ttu-id="c81d1-208">a.</span><span class="sxs-lookup"><span data-stu-id="c81d1-208">a.</span></span> <span data-ttu-id="c81d1-209">**Aktuell Version**: Ange en version för hello-paketet som du överför.</span><span class="sxs-lookup"><span data-stu-id="c81d1-209">**Current Version**: Enter a version for hello package you upload.</span></span> <span data-ttu-id="c81d1-210">Det måste vara i formatet för hello `{number}.{number}.{number}{number}`.</span><span class="sxs-lookup"><span data-stu-id="c81d1-210">It should be in hello format `{number}.{number}.{number}{number}`.</span></span>

    <span data-ttu-id="c81d1-211">b.</span><span class="sxs-lookup"><span data-stu-id="c81d1-211">b.</span></span> <span data-ttu-id="c81d1-212">**Välj en paketfil**: det här paketet innehåller hello följande filer som är komprimerade till en ZIP-fil:</span><span class="sxs-lookup"><span data-stu-id="c81d1-212">**Select a package file**: This package contains hello following files that are compressed into a .zip file:</span></span>
    * <span data-ttu-id="c81d1-213">**applianceMainTemplate.json**: hello distribution mallfilen som har använt toodeploy hello lösning/application.</span><span class="sxs-lookup"><span data-stu-id="c81d1-213">**applianceMainTemplate.json**: hello deployment template file that's used toodeploy hello solution/application.</span></span> <span data-ttu-id="c81d1-214">Information om hur toocreate mallfilerna för distribution, se [skapa din första Azure Resource Manager-mallen](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="c81d1-214">For information about how toocreate deployment template files, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>
    * <span data-ttu-id="c81d1-215">**appliancecreateUIDefinition.json**: den här filen används av hello Azure portal toogenerate hello-användargränssnittet som har använt tooprovision lösning/programmet.</span><span class="sxs-lookup"><span data-stu-id="c81d1-215">**appliancecreateUIDefinition.json**: This file is used by hello Azure portal toogenerate hello user interface that's used tooprovision this solution/application.</span></span> <span data-ttu-id="c81d1-216">Mer information finns i [Kom igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c81d1-216">For more information, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
    * <span data-ttu-id="c81d1-217">**mainTemplate.json**: den här mallen innehåller endast hello Microsoft.Solution/appliances resurs.</span><span class="sxs-lookup"><span data-stu-id="c81d1-217">**mainTemplate.json**: This template file contains only hello Microsoft.Solution/appliances resource.</span></span> <span data-ttu-id="c81d1-218">Hej mainTemplate filen innehåller hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="c81d1-218">hello mainTemplate file includes hello following properties:</span></span>

        *  <span data-ttu-id="c81d1-219">**typ**: Använd **Marketplace** för hanterade program i hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c81d1-219">**kind**: Use **Marketplace** for managed applications in hello Marketplace.</span></span>
        *  <span data-ttu-id="c81d1-220">**ManagedResourceGroupId**: den här resursgruppen i hello kundens prenumeration är där alla hello-resurser som definierats i applianceMainTemplate.json har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="c81d1-220">**ManagedResourceGroupId**: This resource group in hello customer's subscription is where all hello resources defined in applianceMainTemplate.json are deployed.</span></span>
        *  <span data-ttu-id="c81d1-221">**PublisherPackageId**: denna sträng som unikt identifierar hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="c81d1-221">**PublisherPackageId**: This string uniquely identifies hello package.</span></span> <span data-ttu-id="c81d1-222">Ange hello värde i hello-format för `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.</span><span class="sxs-lookup"><span data-stu-id="c81d1-222">Provide hello value in hello format of `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.</span></span>

<span data-ttu-id="c81d1-223">Hämta hello **erbjuder ID** och **Publicerings-ID** från hello publicering portal, enligt följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="c81d1-223">Obtain hello **Offer ID** and **Publisher ID** from hello publishing portal, as shown in hello following image:</span></span>

![Erbjudande-ID](./media/managed-application-author-marketplace/UniqueString_pubid_offerid.png)
        
<span data-ttu-id="c81d1-225">Hämta hello **SKU ID**som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="c81d1-225">Obtain hello **SKU ID**, as shown in hello following image:</span></span>

![SKU-ID](./media/managed-application-author-marketplace/UniqueString_skuid.png)
        
<span data-ttu-id="c81d1-227">Hämtar hello paketet **Version**som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="c81d1-227">Obtain hello package **Version**, as shown in hello following image:</span></span>

![Paketversion](./media/managed-application-author-marketplace/UniqueString_packageversion.png)
    
  <span data-ttu-id="c81d1-229">Baserat på hello föregående exempel hello värdet för **PublisherPackageId** är `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="c81d1-229">Based on hello preceding examples, hello value of **PublisherPackageId** is `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.</span></span>

  <span data-ttu-id="c81d1-230">Exempel på mainTemplate.json:</span><span class="sxs-lookup"><span data-stu-id="c81d1-230">Sample mainTemplate.json:</span></span>

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "storageAccountNamePrefix": {
        "type": "string",
        "metadata": {
          "description": "Specify hello name of hello storage account"
        }
      },
      "storageAccountType": {
        "type": "string"
      }
    },
    "variables": {
      "managedResourceGroup": "[concat(resourceGroup().id,uniquestring(resourceGroup().id))]"
    },
    "resources": [{
      "type": "Microsoft.Solutions/appliances",
      "apiVersion": "2016-09-01-preview",
      "name": "[concat(parameters('storageAccountNamePrefix'), '-', 'managed')]",
      "location": "[resourceGroup().location]",
      "kind": "marketplace",
      "properties": {
        "managedResourceGroupId": "[variables('managedResourceGroup')]",
        "PublisherPackageId":"azureappliancetest.ravmanagedapptest.ravpreviewmanagedsku.1.0.0",
        "parameters": {
          "storageAccountName": {
            "value": "[parameters('storageAccountNamePrefix')]"
          },
          "storageAccountType": {
            "value": "[parameters('storageAccountType')]"
          }
        }
      }
    }],
    "outputs": {

    }
  }
  ```

<span data-ttu-id="c81d1-231">Det här paketet innehåller kapslade mallar eller skript som är nödvändiga toosuccessfully etablera det här programmet.</span><span class="sxs-lookup"><span data-stu-id="c81d1-231">This package should contain any other nested templates or scripts that are required toosuccessfully provision this application.</span></span> <span data-ttu-id="c81d1-232">Hej måste mainTemplate.json, applianceMainTemplate.json och applianceCreateUIDefinition.json filer finnas hello rotmappen.</span><span class="sxs-lookup"><span data-stu-id="c81d1-232">hello mainTemplate.json, applianceMainTemplate.json, and applianceCreateUIDefinition.json files must be present at hello root folder.</span></span>

* <span data-ttu-id="c81d1-233">**Tillstånd**: den här egenskapen definierar vilka som får åtkomst och hello åtkomstnivå toohello resurser i kundernas prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="c81d1-233">**Authorizations**: This property defines who gets access and hello level of access toohello resources in customers' subscriptions.</span></span> <span data-ttu-id="c81d1-234">hello publisher kan använda den toomanage hello program för hello kunds räkning.</span><span class="sxs-lookup"><span data-stu-id="c81d1-234">hello publisher can use it toomanage hello application on behalf of hello customer.</span></span>
* <span data-ttu-id="c81d1-235">**PrincipalId**: den här egenskapen är hello Azure Active Directory (AD Azure) identifierare för en användare, grupp eller ett program som har beviljats vissa behörigheter för hello resurser i hello kundens prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c81d1-235">**PrincipalId**: This property is hello Azure Active Directory (Azure AD) identifier of a user, user group, or application that's granted certain permissions on hello resources in hello customer's subscription.</span></span> <span data-ttu-id="c81d1-236">hello rolldefinitionen beskriver hello behörigheter.</span><span class="sxs-lookup"><span data-stu-id="c81d1-236">hello Role Definition describes hello permissions.</span></span> 
* <span data-ttu-id="c81d1-237">**Rolldefinitionen**: den här egenskapen är en lista över alla hello inbyggda rollbaserad åtkomstkontroll (RBAC) roller tillhandahålls av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c81d1-237">**Role Definition**: This property is a list of all hello built-in Role-Based Access Control (RBAC) roles provided by Azure AD.</span></span> <span data-ttu-id="c81d1-238">Du kan välja hello roll som är mest lämpliga toouse toomanage hello resurser för hello kunds räkning.</span><span class="sxs-lookup"><span data-stu-id="c81d1-238">You can select hello role that's most appropriate toouse toomanage hello resources on behalf of hello customer.</span></span>

<span data-ttu-id="c81d1-239">Du kan lägga till flera tillstånd.</span><span class="sxs-lookup"><span data-stu-id="c81d1-239">You can add multiple authorizations.</span></span> <span data-ttu-id="c81d1-240">Vi rekommenderar att du skapar en grupp i AD-användare och ange dess ID i **PrincipalId**.</span><span class="sxs-lookup"><span data-stu-id="c81d1-240">We recommend that you create an AD user group and specify its ID in **PrincipalId**.</span></span> <span data-ttu-id="c81d1-241">På så sätt kan du kan lägga till fler användare toohello användargrupp utan hello måste tooupdate hello SKU.</span><span class="sxs-lookup"><span data-stu-id="c81d1-241">This way, you can add more users toohello user group without hello need tooupdate hello SKU.</span></span>

<span data-ttu-id="c81d1-242">Läs mer om hur RBAC [komma igång med RBAC på hello Azure-portalen](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="c81d1-242">For more information about RBAC, see [Get started with RBAC in hello Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>

## <a name="marketplace-form"></a><span data-ttu-id="c81d1-243">Marketplace-formulär</span><span class="sxs-lookup"><span data-stu-id="c81d1-243">Marketplace form</span></span>

<span data-ttu-id="c81d1-244">hello Marketplace formuläret begär fält som visas på hello [Azure Marketplace](https://azuremarketplace.microsoft.com) och på hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c81d1-244">hello Marketplace form asks for fields that show up on hello [Azure Marketplace](https://azuremarketplace.microsoft.com) and on hello [Azure portal](https://portal.azure.com/).</span></span>

### <a name="preview-subscription-ids"></a><span data-ttu-id="c81d1-245">Prenumerations-ID: N för förhandsgranskning</span><span class="sxs-lookup"><span data-stu-id="c81d1-245">Preview subscription IDs</span></span>

<span data-ttu-id="c81d1-246">Ange en lista över Azure-prenumeration ID: N som kan komma åt hello erbjudande när den har publicerats.</span><span class="sxs-lookup"><span data-stu-id="c81d1-246">Enter a list of Azure subscription IDs that can access hello offer after it's published.</span></span> <span data-ttu-id="c81d1-247">Du kan använda dessa vitt visas prenumerationer tootest hello förhandsgranskas erbjudande innan du gör den live.</span><span class="sxs-lookup"><span data-stu-id="c81d1-247">You can use these white-listed subscriptions tootest hello previewed offer before you make it live.</span></span> <span data-ttu-id="c81d1-248">Du kan sammanställa en lista för tillåten för in too100 prenumerationer i hello partnerportalen.</span><span class="sxs-lookup"><span data-stu-id="c81d1-248">You can compile a white list of up too100 subscriptions in hello partner portal.</span></span>

### <a name="suggested-categories"></a><span data-ttu-id="c81d1-249">Föreslagna kategorier</span><span class="sxs-lookup"><span data-stu-id="c81d1-249">Suggested categories</span></span>

<span data-ttu-id="c81d1-250">Välj toofive kategorier hello listan som erbjudandet bäst kan associeras med.</span><span class="sxs-lookup"><span data-stu-id="c81d1-250">Select up toofive categories from hello list that your offer can be best associated with.</span></span> <span data-ttu-id="c81d1-251">Dessa kategorier är används toomap ditt erbjudande toohello produktkategorier som är tillgängliga i hello [Azure Marketplace](https://azuremarketplace.microsoft.com) och hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c81d1-251">These categories are used toomap your offer toohello product categories that are available in hello [Azure Marketplace](https://azuremarketplace.microsoft.com) and hello [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="azure-marketplace"></a><span data-ttu-id="c81d1-252">Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="c81d1-252">Azure Marketplace</span></span>

<span data-ttu-id="c81d1-253">hello sammanfattning av det hanterade programmet visar hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="c81d1-253">hello summary of your managed application displays hello following fields:</span></span>

![Marketplace-sammanfattning](./media/managed-application-author-marketplace/publishvm10.png)

<span data-ttu-id="c81d1-255">Hej **översikt över** för det hanterade programmet visar hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="c81d1-255">hello **Overview** tab for your managed application displays hello following fields:</span></span>

![Marketplace-översikt](./media/managed-application-author-marketplace/publishvm11.png)

<span data-ttu-id="c81d1-257">Hej **planer + priser** för det hanterade programmet visar hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="c81d1-257">hello **Plans + Pricing** tab for your managed application displays hello following fields:</span></span>

![Marketplace-planer](./media/managed-application-author-marketplace/publishvm15.png)

#### <a name="azure-portal"></a><span data-ttu-id="c81d1-259">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c81d1-259">Azure portal</span></span>

<span data-ttu-id="c81d1-260">hello sammanfattning av det hanterade programmet visar hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="c81d1-260">hello summary of your managed application displays hello following fields:</span></span>

![Översikt över Portal](./media/managed-application-author-marketplace/publishvm12.png)

<span data-ttu-id="c81d1-262">hello översikt för det hanterade programmet visar hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="c81d1-262">hello overview for your managed application displays hello following fields:</span></span>

![Portalen översikt](./media/managed-application-author-marketplace/publishvm13.png)

#### <a name="logo-guidelines"></a><span data-ttu-id="c81d1-264">Logotypen riktlinjer</span><span class="sxs-lookup"><span data-stu-id="c81d1-264">Logo guidelines</span></span>

<span data-ttu-id="c81d1-265">Följ dessa riktlinjer för en logotyp som du överför i hello molnpartnerportalen:</span><span class="sxs-lookup"><span data-stu-id="c81d1-265">Follow these guidelines for any logo that you upload in hello Cloud Partner portal:</span></span>

*   <span data-ttu-id="c81d1-266">hello Azure design har en enkel färgpalett.</span><span class="sxs-lookup"><span data-stu-id="c81d1-266">hello Azure design has a simple color palette.</span></span> <span data-ttu-id="c81d1-267">Begränsa hello antalet primära och sekundära färger i din logotyp.</span><span class="sxs-lookup"><span data-stu-id="c81d1-267">Limit hello number of primary and secondary colors on your logo.</span></span>
*   <span data-ttu-id="c81d1-268">hello temafärger hello portalen är vit och svart.</span><span class="sxs-lookup"><span data-stu-id="c81d1-268">hello theme colors of hello portal are white and black.</span></span> <span data-ttu-id="c81d1-269">Använd inte färgerna som hello bakgrundsfärg för din logotyp.</span><span class="sxs-lookup"><span data-stu-id="c81d1-269">Don't use these colors as hello background color for your logo.</span></span> <span data-ttu-id="c81d1-270">Använd en färg som gör din logotyp framträdande hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="c81d1-270">Use a color that makes your logo prominent in hello portal.</span></span> <span data-ttu-id="c81d1-271">Vi rekommenderar enkla primära färger.</span><span class="sxs-lookup"><span data-stu-id="c81d1-271">We recommend simple primary colors.</span></span> <span data-ttu-id="c81d1-272">*Om du använder en genomskinlig bakgrund, kontrollera att hello logotyp och texten inte vitt, svart eller blå.*</span><span class="sxs-lookup"><span data-stu-id="c81d1-272">*If you use a transparent background, make sure that hello logo and text aren't white, black, or blue.*</span></span>
*   <span data-ttu-id="c81d1-273">Använd inte en toning bakgrund på hello-logotypen.</span><span class="sxs-lookup"><span data-stu-id="c81d1-273">Don't use a gradient background on hello logo.</span></span>
*   <span data-ttu-id="c81d1-274">Placera inte text på hello logotyp, inte ens företaget eller varumärke.</span><span class="sxs-lookup"><span data-stu-id="c81d1-274">Don't place text on hello logo, not even your company or brand name.</span></span> <span data-ttu-id="c81d1-275">hello utseende och känslan av din logotyp ska vara platt och undvika toningar.</span><span class="sxs-lookup"><span data-stu-id="c81d1-275">hello look and feel of your logo should be flat and avoid gradients.</span></span>
*   <span data-ttu-id="c81d1-276">Kontrollera att hello logotypen inte har sträckts ut.</span><span class="sxs-lookup"><span data-stu-id="c81d1-276">Make sure hello logo isn't stretched.</span></span>

#### <a name="hero-logo"></a><span data-ttu-id="c81d1-277">Hjälte-logotyp</span><span class="sxs-lookup"><span data-stu-id="c81d1-277">Hero logo</span></span>

<span data-ttu-id="c81d1-278">hello hjälte logotypen är valfritt.</span><span class="sxs-lookup"><span data-stu-id="c81d1-278">hello hero logo is optional.</span></span> <span data-ttu-id="c81d1-279">hello publisher kan välja inte tooupload en hjälte logotyp.</span><span class="sxs-lookup"><span data-stu-id="c81d1-279">hello publisher can choose not tooupload a hero logo.</span></span> <span data-ttu-id="c81d1-280">När hello hjälte ikonen har överförts kan inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="c81d1-280">After hello hero icon is uploaded, it can't be deleted.</span></span> <span data-ttu-id="c81d1-281">Hello partner måste följa hello Marketplace riktlinjer för hjälte ikoner som helst.</span><span class="sxs-lookup"><span data-stu-id="c81d1-281">At that time, hello partner must follow hello Marketplace guidelines for hero icons.</span></span>

<span data-ttu-id="c81d1-282">Följ dessa riktlinjer för hello hjälte logotypen ikon:</span><span class="sxs-lookup"><span data-stu-id="c81d1-282">Follow these guidelines for hello hero logo icon:</span></span>

*   <span data-ttu-id="c81d1-283">hello utgivarens namn, hello plan rubrik och hello erbjudande lång sammanfattning visas i vitt.</span><span class="sxs-lookup"><span data-stu-id="c81d1-283">hello publisher display name, hello plan title, and hello offer long summary are displayed in white.</span></span> <span data-ttu-id="c81d1-284">Därför inte använda en ljusare hello bakgrunden hello hjälte ikon.</span><span class="sxs-lookup"><span data-stu-id="c81d1-284">Therefore, don't use a light color for hello background of hello hero icon.</span></span> <span data-ttu-id="c81d1-285">En svart, vit eller genomskinlig bakgrund är inte tillåten för hjälte ikoner.</span><span class="sxs-lookup"><span data-stu-id="c81d1-285">A black, white, or transparent background isn't allowed for hero icons.</span></span>
*   <span data-ttu-id="c81d1-286">När hello erbjudande visas hello publisher visningsnamn, hello plan rubrik, hello erbjudande lång sammanfattning och hello **skapa** knappen inbäddad programmässigt i hello hjälte logotyp.</span><span class="sxs-lookup"><span data-stu-id="c81d1-286">After hello offer is listed, hello publisher display name, hello plan title, hello offer long summary, and hello **Create** button are embedded programmatically inside hello hero logo.</span></span> <span data-ttu-id="c81d1-287">Därför inte ange valfri text när du utformar hello hjälte logotyp.</span><span class="sxs-lookup"><span data-stu-id="c81d1-287">Consequently, don't enter any text while you design hello hero logo.</span></span> <span data-ttu-id="c81d1-288">Lämna tomt utrymme på hello rätt eftersom hello text programmässigt ingår i detta utrymme.</span><span class="sxs-lookup"><span data-stu-id="c81d1-288">Leave empty space on hello right because hello text is included programmatically in that space.</span></span> <span data-ttu-id="c81d1-289">hello tomt utrymme för hello text ska 415 x 100 bildpunkter på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="c81d1-289">hello empty space for hello text should be 415 x 100 pixels on hello right.</span></span> <span data-ttu-id="c81d1-290">Det är förskjutning med 370 bildpunkter från hello vänster.</span><span class="sxs-lookup"><span data-stu-id="c81d1-290">It's offset by 370 pixels from hello left.</span></span>

    ![Hjälte logotypen exempel](./media/managed-application-author-marketplace/publishvm14.png)

## <a name="support-form"></a><span data-ttu-id="c81d1-292">Stöd för formulär</span><span class="sxs-lookup"><span data-stu-id="c81d1-292">Support form</span></span>

<span data-ttu-id="c81d1-293">Fyll i hello **stöder** formuläret med stöd för kontakter från ditt företag.</span><span class="sxs-lookup"><span data-stu-id="c81d1-293">Fill out hello **Support** form with support contacts from your company.</span></span> <span data-ttu-id="c81d1-294">Den här informationen kan vara engineering customer support kontakter.</span><span class="sxs-lookup"><span data-stu-id="c81d1-294">This information might be engineering contacts and customer support contacts.</span></span>

## <a name="publish-an-offer"></a><span data-ttu-id="c81d1-295">Publicera ett erbjudande</span><span class="sxs-lookup"><span data-stu-id="c81d1-295">Publish an offer</span></span>

<span data-ttu-id="c81d1-296">När du fyller i alla hello avsnitt markerar **publicera** toostart hello process som gör att din tillgängliga toocustomers erbjudandet.</span><span class="sxs-lookup"><span data-stu-id="c81d1-296">After you fill out all hello sections, select **Publish** toostart hello process that makes your offer available toocustomers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c81d1-297">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c81d1-297">Next steps</span></span>

* <span data-ttu-id="c81d1-298">En introduktion toomanaged program, se [hanteras Programöversikt](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c81d1-298">For an introduction toomanaged applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="c81d1-299">Information om hur du använder ett hanterat program från hello Marketplace finns [använda Azure hanterade program i hello Marketplace](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="c81d1-299">For information about consuming a managed application from hello Marketplace, see [Consume Azure managed applications in hello Marketplace](managed-application-consume-marketplace.md).</span></span>
* <span data-ttu-id="c81d1-300">Information om hur du publicerar ett program för Tjänstkatalog hanteras finns [skapa och publicera en applikation för Tjänstkatalog hanteras](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="c81d1-300">For information about publishing a Service Catalog managed application, see [Create and publish a Service Catalog managed application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="c81d1-301">Information om att använda ett Tjänstkatalogen hanterade program, se [använder ett Tjänstkatalogen hanterade program](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="c81d1-301">For information about consuming a Service Catalog managed application, see [Consume a Service Catalog managed application](managed-application-consumption.md).</span></span>
