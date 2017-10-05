---
title: Azure hanterade program i Marketplace | Microsoft Docs
description: "Beskriver Azure hanterade program som är tillgängliga via Marketplace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 58ac7665abf7e75a43bb0b92bdf6f41005c3efe8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-managed-applications-in-the-marketplace"></a><span data-ttu-id="804b7-103">Azure hanterade program i Marketplace</span><span class="sxs-lookup"><span data-stu-id="804b7-103">Azure managed applications in the Marketplace</span></span>

 <span data-ttu-id="804b7-104">MSPs ISV: er och systemintegrerare (SIs) kan använda Azure hanterade program att erbjuda sina lösningar för alla Azure Marketplace-kunder.</span><span class="sxs-lookup"><span data-stu-id="804b7-104">MSPs, ISVs, and system integrators (SIs) can use Azure managed applications to offer their solutions to all Azure Marketplace customers.</span></span> <span data-ttu-id="804b7-105">Sådana lösningar minska underhållet och underhåll kostnader för kunder.</span><span class="sxs-lookup"><span data-stu-id="804b7-105">Such solutions reduce the maintenance and servicing overhead for customers.</span></span> <span data-ttu-id="804b7-106">Utgivare kan sälja infrastruktur- och programvara på marknadsplatsen.</span><span class="sxs-lookup"><span data-stu-id="804b7-106">Publishers can sell infrastructure and software through the Marketplace.</span></span> <span data-ttu-id="804b7-107">De kan koppla tjänster och operativa stöd till hanterade program.</span><span class="sxs-lookup"><span data-stu-id="804b7-107">They can attach services and operational support to managed applications.</span></span> <span data-ttu-id="804b7-108">Mer information finns i [hanteras Programöversikt](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="804b7-108">For more information, see [Managed application overview](managed-application-overview.md).</span></span>

<span data-ttu-id="804b7-109">Den här artikeln beskrivs hur en MSP, ISV eller SI kan publicera ett program till Marketplace och göra den tillgänglig för kunder.</span><span class="sxs-lookup"><span data-stu-id="804b7-109">This article explains how an MSP, ISV, or SI can publish an application to the Marketplace and make it broadly available to customers.</span></span>

## <a name="prerequisites-for-publishing-a-managed-application"></a><span data-ttu-id="804b7-110">Krav för att publicera ett hanterat program</span><span class="sxs-lookup"><span data-stu-id="804b7-110">Prerequisites for publishing a managed application</span></span>

<span data-ttu-id="804b7-111">Förutsättningar för att lista i Marketplace:</span><span class="sxs-lookup"><span data-stu-id="804b7-111">Prerequisites to listing in the Marketplace:</span></span>

* <span data-ttu-id="804b7-112">Tekniska</span><span class="sxs-lookup"><span data-stu-id="804b7-112">Technical</span></span>

    *  <span data-ttu-id="804b7-113">Information om grundläggande struktur och syntaxen för Azure Resource Manager-mallar finns [Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="804b7-113">For information about the basic structure and syntax of Azure Resource Manager templates, see [Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
    *  <span data-ttu-id="804b7-114">Om du vill visa fullständig mallen lösningar finns [Azure Quickstart mallar](https://azure.microsoft.com/en-us/documentation/templates/) eller [Quickstart mallen databasen](https://github.com/azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="804b7-114">To view complete template solutions, see [Azure Quickstart templates](https://azure.microsoft.com/en-us/documentation/templates/) or the [Quickstart template repository](https://github.com/azure/azure-quickstart-templates).</span></span>
    *  <span data-ttu-id="804b7-115">Information om hur du skapar gränssnittet för kunder som distribuerar ditt program via Marketplace finns [skapa en fil för användaren gränssnittsdefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="804b7-115">For information about how to create the interface for customers who deploy your application through the Marketplace, see [Create a user interface definition file](managed-application-createuidefinition-overview.md).</span></span>

* <span data-ttu-id="804b7-116">Sökassistent (affärsbehov)</span><span class="sxs-lookup"><span data-stu-id="804b7-116">Nontechnical (business requirements)</span></span>

    *   <span data-ttu-id="804b7-117">Ditt företag eller dess dotterbolag måste finnas i ett land där försäljning stöds av Marketplace.</span><span class="sxs-lookup"><span data-stu-id="804b7-117">Your company or its subsidiary must be located in a country where sales are supported by the Marketplace.</span></span>
    *   <span data-ttu-id="804b7-118">Produkten måste ha licens på ett sätt som är kompatibel med fakturering modeller som stöds av Marketplace.</span><span class="sxs-lookup"><span data-stu-id="804b7-118">Your product must be licensed in a way that is compatible with billing models supported by the Marketplace.</span></span>
    *   <span data-ttu-id="804b7-119">Du är ansvarig för teknisk support för kunder på ett kommersiellt rimliga sätt.</span><span class="sxs-lookup"><span data-stu-id="804b7-119">You're responsible for making technical support available to customers in a commercially reasonable manner.</span></span> <span data-ttu-id="804b7-120">Stöd kan vara fria, betald, eller via community stöd.</span><span class="sxs-lookup"><span data-stu-id="804b7-120">The support can be free, paid, or through community support.</span></span>
    *   <span data-ttu-id="804b7-121">Du är ansvarig för att licensiera programmet och eventuella beroenden för programvara från tredje part.</span><span class="sxs-lookup"><span data-stu-id="804b7-121">You're responsible for licensing your software and any third-party software dependencies.</span></span>
    *   <span data-ttu-id="804b7-122">Du måste ange innehåll som uppfyller villkoren för ditt erbjudande ska visas i Marketplace och i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="804b7-122">You must provide content that meets criteria for your offering to be listed in the Marketplace and in the Azure portal.</span></span>
    *   <span data-ttu-id="804b7-123">Du måste acceptera villkoren i avtalet för utgivaren och Azure Marketplace deltagande principer.</span><span class="sxs-lookup"><span data-stu-id="804b7-123">You must agree to the terms of the Azure Marketplace Participation Policies and Publisher Agreement.</span></span>
    *   <span data-ttu-id="804b7-124">Du måste acceptera att följa den användningsvillkoren och sekretesspolicyn för Microsoft certifierade programavtalet för Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="804b7-124">You must agree to comply with the Terms of Use, Microsoft Privacy Statement, and Microsoft Azure Certified Program Agreement.</span></span>

## <a name="create-a-new-azure-application-offer"></a><span data-ttu-id="804b7-125">Skapa ett nytt erbjudande för Azure-program</span><span class="sxs-lookup"><span data-stu-id="804b7-125">Create a new Azure application offer</span></span>

<span data-ttu-id="804b7-126">Efter att du uppfyller kraven som är du redo att skapa erbjudandet hanterade program.</span><span class="sxs-lookup"><span data-stu-id="804b7-126">After you meet the prerequisites, you're ready to create your managed application offer.</span></span> <span data-ttu-id="804b7-127">Låt oss ta en snabb överblick över ett erbjudande och en SKU.</span><span class="sxs-lookup"><span data-stu-id="804b7-127">Let's take a quick overview of an offer and a SKU.</span></span>

### <a name="offer"></a><span data-ttu-id="804b7-128">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="804b7-128">Offer</span></span>

<span data-ttu-id="804b7-129">Erbjudande för ett hanterat program motsvarar en klass av produkten erbjudande från en utgivare.</span><span class="sxs-lookup"><span data-stu-id="804b7-129">The offer for a managed application corresponds to a class of product offering from a publisher.</span></span> <span data-ttu-id="804b7-130">Om du har en ny typ av lösning/program som du vill ska vara tillgängliga i Marketplace kan konfigurera du den som ett nytt erbjudande.</span><span class="sxs-lookup"><span data-stu-id="804b7-130">If you have a new type of solution/application that you want to make available in the Marketplace, you can set it up as a new offer.</span></span> <span data-ttu-id="804b7-131">Ett erbjudande är en samling av SKU: er.</span><span class="sxs-lookup"><span data-stu-id="804b7-131">An offer is a collection of SKUs.</span></span> <span data-ttu-id="804b7-132">Varje erbjudande visas som sin egen enhet i Marketplace.</span><span class="sxs-lookup"><span data-stu-id="804b7-132">Every offer appears as its own entity in the Marketplace.</span></span>

### <a name="sku"></a><span data-ttu-id="804b7-133">SKU</span><span class="sxs-lookup"><span data-stu-id="804b7-133">SKU</span></span>

<span data-ttu-id="804b7-134">En SKU är den minsta köpbara enheten ett erbjudande.</span><span class="sxs-lookup"><span data-stu-id="804b7-134">A SKU is the smallest purchasable unit of an offer.</span></span> <span data-ttu-id="804b7-135">Du kan använda en SKU inom samma produkten klass (erbjudandet) för att skilja mellan:</span><span class="sxs-lookup"><span data-stu-id="804b7-135">You can use a SKU within the same product class (offer) to differentiate between:</span></span>

* <span data-ttu-id="804b7-136">Olika funktioner som stöds.</span><span class="sxs-lookup"><span data-stu-id="804b7-136">Different features that are supported.</span></span>
* <span data-ttu-id="804b7-137">Om erbjudandet är hanterad eller ohanterad.</span><span class="sxs-lookup"><span data-stu-id="804b7-137">Whether the offer is managed or unmanaged.</span></span>
* <span data-ttu-id="804b7-138">Fakturering modeller som stöds.</span><span class="sxs-lookup"><span data-stu-id="804b7-138">Billing models that are supported.</span></span>

<span data-ttu-id="804b7-139">En SKU visas under överordnade erbjudandet på Marketplace.</span><span class="sxs-lookup"><span data-stu-id="804b7-139">A SKU appears under the parent offer in the Marketplace.</span></span> <span data-ttu-id="804b7-140">Det verkar som sin egen köpbara enhet i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="804b7-140">It appears as its own purchasable entity in the Azure portal.</span></span>

### <a name="set-up-an-offer"></a><span data-ttu-id="804b7-141">Konfigurera ett erbjudande</span><span class="sxs-lookup"><span data-stu-id="804b7-141">Set up an offer</span></span>

1. <span data-ttu-id="804b7-142">Logga in på den [molnpartnerportalen](https://cloudpartner.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="804b7-142">Sign in to the [Cloud Partner portal](https://cloudpartner.azure.com/).</span></span>

2. <span data-ttu-id="804b7-143">I navigeringsfönstret till vänster, Välj **+ nytt erbjudande** > **Azure program**.</span><span class="sxs-lookup"><span data-stu-id="804b7-143">In the navigation pane on the left, select **+ New offer** > **Azure Applications**.</span></span>

    ![Nytt erbjudande](./media/managed-application-author-marketplace/newOffer.png)

3. <span data-ttu-id="804b7-145">Fylla i formulär som visas till vänster i den **Editor** vyn.</span><span class="sxs-lookup"><span data-stu-id="804b7-145">Fill out the forms that appear on the left in the **Editor** view.</span></span> <span data-ttu-id="804b7-146">Obligatoriska fält är markerade med en röd asterisk (*).</span><span class="sxs-lookup"><span data-stu-id="804b7-146">Required fields are marked with a red asterisk (*).</span></span>

    ![Inställningar för erbjudande](./media/managed-application-author-marketplace/newOffer_OfferSettings.png)

    <span data-ttu-id="804b7-148">Fyra huvudsakliga formulär används för att skapa ett hanterat program:</span><span class="sxs-lookup"><span data-stu-id="804b7-148">Four main forms are used to create a managed application:</span></span>

    <span data-ttu-id="804b7-149">a.</span><span class="sxs-lookup"><span data-stu-id="804b7-149">a.</span></span> <span data-ttu-id="804b7-150">Inställningar för erbjudande</span><span class="sxs-lookup"><span data-stu-id="804b7-150">Offer Settings</span></span>

    <span data-ttu-id="804b7-151">b.</span><span class="sxs-lookup"><span data-stu-id="804b7-151">b.</span></span> <span data-ttu-id="804b7-152">SKU: er</span><span class="sxs-lookup"><span data-stu-id="804b7-152">SKUs</span></span>

    <span data-ttu-id="804b7-153">c.</span><span class="sxs-lookup"><span data-stu-id="804b7-153">c.</span></span> <span data-ttu-id="804b7-154">Marketplace</span><span class="sxs-lookup"><span data-stu-id="804b7-154">Marketplace</span></span>

    <span data-ttu-id="804b7-155">d.</span><span class="sxs-lookup"><span data-stu-id="804b7-155">d.</span></span> <span data-ttu-id="804b7-156">Support</span><span class="sxs-lookup"><span data-stu-id="804b7-156">Support</span></span>

<span data-ttu-id="804b7-157">Formulären beskrivs mer detaljerat i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="804b7-157">These forms are described in greater detail in the following sections.</span></span>

## <a name="offer-settings-form"></a><span data-ttu-id="804b7-158">Erbjudande inställningar för formulär</span><span class="sxs-lookup"><span data-stu-id="804b7-158">Offer Settings form</span></span>
<span data-ttu-id="804b7-159">Använd den här grundläggande formuläret för att ange inställningar för erbjudande.</span><span class="sxs-lookup"><span data-stu-id="804b7-159">Use this basic form to specify the offer settings.</span></span>

1. <span data-ttu-id="804b7-160">Fyll i den **erbjuder inställningar** formuläret.</span><span class="sxs-lookup"><span data-stu-id="804b7-160">Fill in the **Offer Settings** form.</span></span> <span data-ttu-id="804b7-161">Olika områden är:</span><span class="sxs-lookup"><span data-stu-id="804b7-161">The different fields are:</span></span>

    <span data-ttu-id="804b7-162">a.</span><span class="sxs-lookup"><span data-stu-id="804b7-162">a.</span></span> <span data-ttu-id="804b7-163">**Erbjudande-ID**: den unika identifieraren identifierar erbjudandet i en profil för utgivaren.</span><span class="sxs-lookup"><span data-stu-id="804b7-163">**Offer ID**: This unique identifier identifies the offer within a publisher profile.</span></span> <span data-ttu-id="804b7-164">Detta ID visas i produkten URL: er, Resource Manager-mallar och fakturering rapporter.</span><span class="sxs-lookup"><span data-stu-id="804b7-164">This ID is visible in product URLs, Resource Manager templates, and billing reports.</span></span> <span data-ttu-id="804b7-165">Det kan bara består av gemena alfanumeriska tecken och bindestreck (-).</span><span class="sxs-lookup"><span data-stu-id="804b7-165">It can only be composed of lowercase alphanumeric characters or dashes (-).</span></span> <span data-ttu-id="804b7-166">ID: T får inte sluta med ett streck.</span><span class="sxs-lookup"><span data-stu-id="804b7-166">The ID can't end in a dash.</span></span> <span data-ttu-id="804b7-167">Det är begränsat till högst 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="804b7-167">It's limited to a maximum of 50 characters.</span></span> <span data-ttu-id="804b7-168">När ett erbjudande lanseras är i det här fältet låst.</span><span class="sxs-lookup"><span data-stu-id="804b7-168">After an offer goes live, this field is locked.</span></span>

    <span data-ttu-id="804b7-169">b.</span><span class="sxs-lookup"><span data-stu-id="804b7-169">b.</span></span> <span data-ttu-id="804b7-170">**Publicerings-ID**: Använd den här listrutan för att välja publisher-profil som du vill publicera det här erbjudandet under.</span><span class="sxs-lookup"><span data-stu-id="804b7-170">**Publisher ID**: Use this drop-down list to choose the publisher profile you want to publish this offer under.</span></span> <span data-ttu-id="804b7-171">När ett erbjudande lanseras är i det här fältet låst.</span><span class="sxs-lookup"><span data-stu-id="804b7-171">After an offer goes live, this field is locked.</span></span>

    <span data-ttu-id="804b7-172">c.</span><span class="sxs-lookup"><span data-stu-id="804b7-172">c.</span></span> <span data-ttu-id="804b7-173">**Namnet**: det här visningsnamnet för ditt erbjudande visas i Marketplace och i portalen.</span><span class="sxs-lookup"><span data-stu-id="804b7-173">**Name**: This display name for your offer appears in the Marketplace and in the portal.</span></span> <span data-ttu-id="804b7-174">Det kan ha högst 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="804b7-174">It can have a maximum of 50 characters.</span></span> <span data-ttu-id="804b7-175">Innehåller ett okänt varumärke för produkten.</span><span class="sxs-lookup"><span data-stu-id="804b7-175">Include a recognizable brand name for your product.</span></span> <span data-ttu-id="804b7-176">Inkludera inte ditt företagsnamn, om det inte är hur släpps.</span><span class="sxs-lookup"><span data-stu-id="804b7-176">Don't include your company name here unless that's how it's marketed.</span></span> <span data-ttu-id="804b7-177">Om du marknadsföring erbjudandet på din egen webbplats, måste du kontrollera att namnet är exakt hur den visas på webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="804b7-177">If you're marketing this offer on your own website, ensure that the name is exactly how it appears on your website.</span></span>

2. <span data-ttu-id="804b7-178">Välj **spara** att spara ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="804b7-178">Select **Save** to save your progress.</span></span> 

## <a name="skus-form"></a><span data-ttu-id="804b7-179">SKU: er formulär</span><span class="sxs-lookup"><span data-stu-id="804b7-179">SKUs form</span></span>
<span data-ttu-id="804b7-180">Nästa steg är att lägga till SKU: er för ditt erbjudande.</span><span class="sxs-lookup"><span data-stu-id="804b7-180">The next step is to add SKUs for your offer.</span></span>

1. <span data-ttu-id="804b7-181">Välj **SKU: er** > **nya SKU**.</span><span class="sxs-lookup"><span data-stu-id="804b7-181">Select **SKUs** > **New SKU**.</span></span> 

    ![Välj ny SKU](./media/managed-application-author-marketplace/newOffer_skus.png)

2. <span data-ttu-id="804b7-183">Ange en **SKU ID**.</span><span class="sxs-lookup"><span data-stu-id="804b7-183">Enter a **SKU ID**.</span></span> <span data-ttu-id="804b7-184">En SKU-ID är en unik identifierare för SKU: N i ett erbjudande.</span><span class="sxs-lookup"><span data-stu-id="804b7-184">A SKU ID is a unique identifier for the SKU within an offer.</span></span> <span data-ttu-id="804b7-185">Detta ID visas i produkten URL: er, Resource Manager-mallar och fakturering rapporter.</span><span class="sxs-lookup"><span data-stu-id="804b7-185">This ID is visible in product URLs, Resource Manager templates, and billing reports.</span></span> <span data-ttu-id="804b7-186">Det kan bara består av gemena alfanumeriska tecken och bindestreck (-).</span><span class="sxs-lookup"><span data-stu-id="804b7-186">It can only be composed of lowercase alphanumeric characters or dashes (-).</span></span> <span data-ttu-id="804b7-187">ID: T får inte sluta med ett streck och är begränsat till högst 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="804b7-187">The ID can't end in a dash, and it's limited to a maximum of 50 characters.</span></span> <span data-ttu-id="804b7-188">När ett erbjudande lanseras är i det här fältet låst.</span><span class="sxs-lookup"><span data-stu-id="804b7-188">After an offer goes live, this field is locked.</span></span> <span data-ttu-id="804b7-189">Du kan ha flera SKU: er i ett erbjudande.</span><span class="sxs-lookup"><span data-stu-id="804b7-189">You can have multiple SKUs within an offer.</span></span> <span data-ttu-id="804b7-190">Du behöver en SKU för varje avbildning som du planerar att publicera.</span><span class="sxs-lookup"><span data-stu-id="804b7-190">You need a SKU for each image you plan to publish.</span></span>

3. <span data-ttu-id="804b7-191">Fyll i den **SKU information** avsnitt i följande format:</span><span class="sxs-lookup"><span data-stu-id="804b7-191">Fill out the **SKU Details** section on the following form:</span></span>

    ![Ange nya SKU](./media/managed-application-author-marketplace/newOffer_newsku.png)

    <span data-ttu-id="804b7-193">Fylla i följande fält:</span><span class="sxs-lookup"><span data-stu-id="804b7-193">Fill out the following fields:</span></span>
    
    <span data-ttu-id="804b7-194">a.</span><span class="sxs-lookup"><span data-stu-id="804b7-194">a.</span></span> <span data-ttu-id="804b7-195">**Rubrik**: Ange en rubrik för denna SKU.</span><span class="sxs-lookup"><span data-stu-id="804b7-195">**Title**: Enter a title for this SKU.</span></span> <span data-ttu-id="804b7-196">Den här rubriken visas i galleriet för det här objektet.</span><span class="sxs-lookup"><span data-stu-id="804b7-196">This title appears in the gallery for this item.</span></span>

    <span data-ttu-id="804b7-197">b.</span><span class="sxs-lookup"><span data-stu-id="804b7-197">b.</span></span> <span data-ttu-id="804b7-198">**Översikt över**: Ange en kort sammanfattning för denna SKU.</span><span class="sxs-lookup"><span data-stu-id="804b7-198">**Summary**: Enter a short summary for this SKU.</span></span> <span data-ttu-id="804b7-199">Den här texten visas under rubriken.</span><span class="sxs-lookup"><span data-stu-id="804b7-199">This text appears underneath the title.</span></span>

    <span data-ttu-id="804b7-200">c.</span><span class="sxs-lookup"><span data-stu-id="804b7-200">c.</span></span> <span data-ttu-id="804b7-201">**Beskrivning**: Ange en detaljerad beskrivning av SKU: N.</span><span class="sxs-lookup"><span data-stu-id="804b7-201">**Description**: Enter a detailed description about the SKU.</span></span>

    <span data-ttu-id="804b7-202">d.</span><span class="sxs-lookup"><span data-stu-id="804b7-202">d.</span></span> <span data-ttu-id="804b7-203">**SKU-typen**: de tillåtna värdena är **hanterat program** och **Lösningsmallar**.</span><span class="sxs-lookup"><span data-stu-id="804b7-203">**SKU Type**: The allowed values are **Managed Application** and **Solution Templates**.</span></span> <span data-ttu-id="804b7-204">Det här fallet markerar **hanterat program**.</span><span class="sxs-lookup"><span data-stu-id="804b7-204">For this case, select **Managed Application**.</span></span>

4. <span data-ttu-id="804b7-205">Fyll i den **Paketinformation** avsnitt i följande format:</span><span class="sxs-lookup"><span data-stu-id="804b7-205">Fill out the **Package Details** section on the following form:</span></span>

    ![Paket](./media/managed-application-author-marketplace/newOffer_newsku_package.png)

    <span data-ttu-id="804b7-207">Fylla i följande fält:</span><span class="sxs-lookup"><span data-stu-id="804b7-207">Fill out the following fields:</span></span>

    <span data-ttu-id="804b7-208">a.</span><span class="sxs-lookup"><span data-stu-id="804b7-208">a.</span></span> <span data-ttu-id="804b7-209">**Aktuell Version**: Ange en version för det paket som du överför.</span><span class="sxs-lookup"><span data-stu-id="804b7-209">**Current Version**: Enter a version for the package you upload.</span></span> <span data-ttu-id="804b7-210">Det bör vara i formatet `{number}.{number}.{number}{number}`.</span><span class="sxs-lookup"><span data-stu-id="804b7-210">It should be in the format `{number}.{number}.{number}{number}`.</span></span>

    <span data-ttu-id="804b7-211">b.</span><span class="sxs-lookup"><span data-stu-id="804b7-211">b.</span></span> <span data-ttu-id="804b7-212">**Välj en paketfil**: det här paketet innehåller följande filer som är komprimerade till en ZIP-fil:</span><span class="sxs-lookup"><span data-stu-id="804b7-212">**Select a package file**: This package contains the following files that are compressed into a .zip file:</span></span>
    * <span data-ttu-id="804b7-213">**applianceMainTemplate.json**: distribution mallfilen som används för att distribuera lösningen/application.</span><span class="sxs-lookup"><span data-stu-id="804b7-213">**applianceMainTemplate.json**: The deployment template file that's used to deploy the solution/application.</span></span> <span data-ttu-id="804b7-214">Information om hur du skapar distributionen mallfilerna finns [skapa din första Azure Resource Manager-mallen](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="804b7-214">For information about how to create deployment template files, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>
    * <span data-ttu-id="804b7-215">**appliancecreateUIDefinition.json**: den här filen används av Azure-portalen för att generera användargränssnittet som används för att etablera den här lösningen/application.</span><span class="sxs-lookup"><span data-stu-id="804b7-215">**appliancecreateUIDefinition.json**: This file is used by the Azure portal to generate the user interface that's used to provision this solution/application.</span></span> <span data-ttu-id="804b7-216">Mer information finns i [Kom igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="804b7-216">For more information, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
    * <span data-ttu-id="804b7-217">**mainTemplate.json**: den här mallen innehåller Microsoft.Solution/appliances resursen.</span><span class="sxs-lookup"><span data-stu-id="804b7-217">**mainTemplate.json**: This template file contains only the Microsoft.Solution/appliances resource.</span></span> <span data-ttu-id="804b7-218">Filen mainTemplate innehåller följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="804b7-218">The mainTemplate file includes the following properties:</span></span>

        *  <span data-ttu-id="804b7-219">**typ**: Använd **Marketplace** för hanterade program på Marketplace.</span><span class="sxs-lookup"><span data-stu-id="804b7-219">**kind**: Use **Marketplace** for managed applications in the Marketplace.</span></span>
        *  <span data-ttu-id="804b7-220">**ManagedResourceGroupId**: den här resursgruppen i kundens prenumeration är där alla resurser som definierats i applianceMainTemplate.json har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="804b7-220">**ManagedResourceGroupId**: This resource group in the customer's subscription is where all the resources defined in applianceMainTemplate.json are deployed.</span></span>
        *  <span data-ttu-id="804b7-221">**PublisherPackageId**: denna sträng som unikt identifierar paketet.</span><span class="sxs-lookup"><span data-stu-id="804b7-221">**PublisherPackageId**: This string uniquely identifies the package.</span></span> <span data-ttu-id="804b7-222">Ange ett värde i formatet `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.</span><span class="sxs-lookup"><span data-stu-id="804b7-222">Provide the value in the format of `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.</span></span>

<span data-ttu-id="804b7-223">Hämta den **erbjuder ID** och **Publicerings-ID** från publishing portal som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="804b7-223">Obtain the **Offer ID** and **Publisher ID** from the publishing portal, as shown in the following image:</span></span>

![Erbjudande-ID](./media/managed-application-author-marketplace/UniqueString_pubid_offerid.png)
        
<span data-ttu-id="804b7-225">Hämta den **SKU ID**, enligt följande bild:</span><span class="sxs-lookup"><span data-stu-id="804b7-225">Obtain the **SKU ID**, as shown in the following image:</span></span>

![SKU-ID](./media/managed-application-author-marketplace/UniqueString_skuid.png)
        
<span data-ttu-id="804b7-227">Hämta paketet **Version**, enligt följande bild:</span><span class="sxs-lookup"><span data-stu-id="804b7-227">Obtain the package **Version**, as shown in the following image:</span></span>

![Paketversion](./media/managed-application-author-marketplace/UniqueString_packageversion.png)
    
  <span data-ttu-id="804b7-229">Baserat på i föregående exempel kan värdet för **PublisherPackageId** är `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="804b7-229">Based on the preceding examples, the value of **PublisherPackageId** is `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.</span></span>

  <span data-ttu-id="804b7-230">Exempel på mainTemplate.json:</span><span class="sxs-lookup"><span data-stu-id="804b7-230">Sample mainTemplate.json:</span></span>

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "storageAccountNamePrefix": {
        "type": "string",
        "metadata": {
          "description": "Specify the name of the storage account"
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

<span data-ttu-id="804b7-231">Det här paketet innehåller alla andra kapslade mallar eller skript som krävs för att kunna etablera det här programmet.</span><span class="sxs-lookup"><span data-stu-id="804b7-231">This package should contain any other nested templates or scripts that are required to successfully provision this application.</span></span> <span data-ttu-id="804b7-232">MainTemplate.json, applianceMainTemplate.json och applianceCreateUIDefinition.json filer måste finnas i rotmappen.</span><span class="sxs-lookup"><span data-stu-id="804b7-232">The mainTemplate.json, applianceMainTemplate.json, and applianceCreateUIDefinition.json files must be present at the root folder.</span></span>

* <span data-ttu-id="804b7-233">**Tillstånd**: den här egenskapen definierar vilka som får åtkomst och vilken åtkomstnivå till resurserna i kundernas prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="804b7-233">**Authorizations**: This property defines who gets access and the level of access to the resources in customers' subscriptions.</span></span> <span data-ttu-id="804b7-234">Utgivaren kan använda den för att hantera program för kundens räkning.</span><span class="sxs-lookup"><span data-stu-id="804b7-234">The publisher can use it to manage the application on behalf of the customer.</span></span>
* <span data-ttu-id="804b7-235">**PrincipalId**: den här egenskapen är Azure Active Directory (Azure AD)-ID för en användare, grupp eller ett program som har beviljats vissa behörigheter för resurser i kundens prenumeration.</span><span class="sxs-lookup"><span data-stu-id="804b7-235">**PrincipalId**: This property is the Azure Active Directory (Azure AD) identifier of a user, user group, or application that's granted certain permissions on the resources in the customer's subscription.</span></span> <span data-ttu-id="804b7-236">Rolldefinitionen beskriver behörigheten.</span><span class="sxs-lookup"><span data-stu-id="804b7-236">The Role Definition describes the permissions.</span></span> 
* <span data-ttu-id="804b7-237">**Rolldefinitionen**: den här egenskapen är en lista med alla inbyggda rollbaserad åtkomstkontroll (RBAC) roller tillhandahålls av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="804b7-237">**Role Definition**: This property is a list of all the built-in Role-Based Access Control (RBAC) roles provided by Azure AD.</span></span> <span data-ttu-id="804b7-238">Du kan välja den roll som är mest lämpligt att använda för att hantera resurserna för kundens räkning.</span><span class="sxs-lookup"><span data-stu-id="804b7-238">You can select the role that's most appropriate to use to manage the resources on behalf of the customer.</span></span>

<span data-ttu-id="804b7-239">Du kan lägga till flera tillstånd.</span><span class="sxs-lookup"><span data-stu-id="804b7-239">You can add multiple authorizations.</span></span> <span data-ttu-id="804b7-240">Vi rekommenderar att du skapar en grupp i AD-användare och ange dess ID i **PrincipalId**.</span><span class="sxs-lookup"><span data-stu-id="804b7-240">We recommend that you create an AD user group and specify its ID in **PrincipalId**.</span></span> <span data-ttu-id="804b7-241">På så sätt kan du lägga till fler användare i användargruppen utan att behöva uppdatera SKU: N.</span><span class="sxs-lookup"><span data-stu-id="804b7-241">This way, you can add more users to the user group without the need to update the SKU.</span></span>

<span data-ttu-id="804b7-242">Läs mer om hur RBAC [komma igång med RBAC på Azure portal](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="804b7-242">For more information about RBAC, see [Get started with RBAC in the Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>

## <a name="marketplace-form"></a><span data-ttu-id="804b7-243">Marketplace-formulär</span><span class="sxs-lookup"><span data-stu-id="804b7-243">Marketplace form</span></span>

<span data-ttu-id="804b7-244">Marketplace-formulär uppmanar för fält som visas på den [Azure Marketplace](https://azuremarketplace.microsoft.com) och på den [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="804b7-244">The Marketplace form asks for fields that show up on the [Azure Marketplace](https://azuremarketplace.microsoft.com) and on the [Azure portal](https://portal.azure.com/).</span></span>

### <a name="preview-subscription-ids"></a><span data-ttu-id="804b7-245">Prenumerations-ID: N för förhandsgranskning</span><span class="sxs-lookup"><span data-stu-id="804b7-245">Preview subscription IDs</span></span>

<span data-ttu-id="804b7-246">Ange en lista över Azure-prenumeration ID: N som kan komma åt erbjudandet när den har publicerats.</span><span class="sxs-lookup"><span data-stu-id="804b7-246">Enter a list of Azure subscription IDs that can access the offer after it's published.</span></span> <span data-ttu-id="804b7-247">Du kan använda dessa vitt visas prenumerationer för att testa förhandsgranskade erbjudandet innan du gör den live.</span><span class="sxs-lookup"><span data-stu-id="804b7-247">You can use these white-listed subscriptions to test the previewed offer before you make it live.</span></span> <span data-ttu-id="804b7-248">Du kan sammanställa en lista för tillåten av upp till 100 prenumerationer i partnerportalen.</span><span class="sxs-lookup"><span data-stu-id="804b7-248">You can compile a white list of up to 100 subscriptions in the partner portal.</span></span>

### <a name="suggested-categories"></a><span data-ttu-id="804b7-249">Föreslagna kategorier</span><span class="sxs-lookup"><span data-stu-id="804b7-249">Suggested categories</span></span>

<span data-ttu-id="804b7-250">Välj upp till fem kategorier i listan som erbjudandet bäst kan associeras med.</span><span class="sxs-lookup"><span data-stu-id="804b7-250">Select up to five categories from the list that your offer can be best associated with.</span></span> <span data-ttu-id="804b7-251">Dessa kategorier som används för att mappa erbjudandet till produktkategorier som är tillgängliga i den [Azure Marketplace](https://azuremarketplace.microsoft.com) och [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="804b7-251">These categories are used to map your offer to the product categories that are available in the [Azure Marketplace](https://azuremarketplace.microsoft.com) and the [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="azure-marketplace"></a><span data-ttu-id="804b7-252">Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="804b7-252">Azure Marketplace</span></span>

<span data-ttu-id="804b7-253">Sammanfattning av det hanterade programmet visar följande fält:</span><span class="sxs-lookup"><span data-stu-id="804b7-253">The summary of your managed application displays the following fields:</span></span>

![Marketplace-sammanfattning](./media/managed-application-author-marketplace/publishvm10.png)

<span data-ttu-id="804b7-255">Den **översikt över** för det hanterade programmet visar följande fält:</span><span class="sxs-lookup"><span data-stu-id="804b7-255">The **Overview** tab for your managed application displays the following fields:</span></span>

![Marketplace-översikt](./media/managed-application-author-marketplace/publishvm11.png)

<span data-ttu-id="804b7-257">Den **planer + priser** för det hanterade programmet visar följande fält:</span><span class="sxs-lookup"><span data-stu-id="804b7-257">The **Plans + Pricing** tab for your managed application displays the following fields:</span></span>

![Marketplace-planer](./media/managed-application-author-marketplace/publishvm15.png)

#### <a name="azure-portal"></a><span data-ttu-id="804b7-259">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="804b7-259">Azure portal</span></span>

<span data-ttu-id="804b7-260">Sammanfattning av det hanterade programmet visar följande fält:</span><span class="sxs-lookup"><span data-stu-id="804b7-260">The summary of your managed application displays the following fields:</span></span>

![Översikt över Portal](./media/managed-application-author-marketplace/publishvm12.png)

<span data-ttu-id="804b7-262">Översikt för det hanterade programmet visar följande fält:</span><span class="sxs-lookup"><span data-stu-id="804b7-262">The overview for your managed application displays the following fields:</span></span>

![Portalen översikt](./media/managed-application-author-marketplace/publishvm13.png)

#### <a name="logo-guidelines"></a><span data-ttu-id="804b7-264">Logotypen riktlinjer</span><span class="sxs-lookup"><span data-stu-id="804b7-264">Logo guidelines</span></span>

<span data-ttu-id="804b7-265">Följ dessa riktlinjer för en logotyp som du överför i molnpartnerportalen:</span><span class="sxs-lookup"><span data-stu-id="804b7-265">Follow these guidelines for any logo that you upload in the Cloud Partner portal:</span></span>

*   <span data-ttu-id="804b7-266">Azure designen har en enkel färgpalett.</span><span class="sxs-lookup"><span data-stu-id="804b7-266">The Azure design has a simple color palette.</span></span> <span data-ttu-id="804b7-267">Begränsa antalet primära och sekundära färger i din logotyp.</span><span class="sxs-lookup"><span data-stu-id="804b7-267">Limit the number of primary and secondary colors on your logo.</span></span>
*   <span data-ttu-id="804b7-268">Färger med portalen är vit och svart.</span><span class="sxs-lookup"><span data-stu-id="804b7-268">The theme colors of the portal are white and black.</span></span> <span data-ttu-id="804b7-269">Använd inte färgerna som bakgrundsfärg för din logotyp.</span><span class="sxs-lookup"><span data-stu-id="804b7-269">Don't use these colors as the background color for your logo.</span></span> <span data-ttu-id="804b7-270">Använd en färg som gör din logotyp framträdande i portalen.</span><span class="sxs-lookup"><span data-stu-id="804b7-270">Use a color that makes your logo prominent in the portal.</span></span> <span data-ttu-id="804b7-271">Vi rekommenderar enkla primära färger.</span><span class="sxs-lookup"><span data-stu-id="804b7-271">We recommend simple primary colors.</span></span> <span data-ttu-id="804b7-272">*Om du använder en genomskinlig bakgrund, se till att logotyp och texten inte vitt, svart eller blå.*</span><span class="sxs-lookup"><span data-stu-id="804b7-272">*If you use a transparent background, make sure that the logo and text aren't white, black, or blue.*</span></span>
*   <span data-ttu-id="804b7-273">Använd inte en toning bakgrund på logotypen.</span><span class="sxs-lookup"><span data-stu-id="804b7-273">Don't use a gradient background on the logo.</span></span>
*   <span data-ttu-id="804b7-274">Placera inte text på logotypen, inte ens företaget eller varumärke.</span><span class="sxs-lookup"><span data-stu-id="804b7-274">Don't place text on the logo, not even your company or brand name.</span></span> <span data-ttu-id="804b7-275">Utseendet och känslan logotypens bör platt och undvika toningar.</span><span class="sxs-lookup"><span data-stu-id="804b7-275">The look and feel of your logo should be flat and avoid gradients.</span></span>
*   <span data-ttu-id="804b7-276">Kontrollera att logotypen inte har sträckts ut.</span><span class="sxs-lookup"><span data-stu-id="804b7-276">Make sure the logo isn't stretched.</span></span>

#### <a name="hero-logo"></a><span data-ttu-id="804b7-277">Hjälte-logotyp</span><span class="sxs-lookup"><span data-stu-id="804b7-277">Hero logo</span></span>

<span data-ttu-id="804b7-278">Logotypen hjälte är valfritt.</span><span class="sxs-lookup"><span data-stu-id="804b7-278">The hero logo is optional.</span></span> <span data-ttu-id="804b7-279">Utgivaren kan du inte överföra en hjälte logotyp.</span><span class="sxs-lookup"><span data-stu-id="804b7-279">The publisher can choose not to upload a hero logo.</span></span> <span data-ttu-id="804b7-280">När ikonen hjälte har överförts kan inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="804b7-280">After the hero icon is uploaded, it can't be deleted.</span></span> <span data-ttu-id="804b7-281">Partnern måste följa Marketplace riktlinjer för hjälte ikoner som helst.</span><span class="sxs-lookup"><span data-stu-id="804b7-281">At that time, the partner must follow the Marketplace guidelines for hero icons.</span></span>

<span data-ttu-id="804b7-282">Följ dessa riktlinjer för ikonen hjälte logotyp:</span><span class="sxs-lookup"><span data-stu-id="804b7-282">Follow these guidelines for the hero logo icon:</span></span>

*   <span data-ttu-id="804b7-283">Visningsnamn för utgivaren, plan rubrik och erbjudandet lång sammanfattning visas i vitt.</span><span class="sxs-lookup"><span data-stu-id="804b7-283">The publisher display name, the plan title, and the offer long summary are displayed in white.</span></span> <span data-ttu-id="804b7-284">Därför inte använda en ljusare ikonen hjälte bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="804b7-284">Therefore, don't use a light color for the background of the hero icon.</span></span> <span data-ttu-id="804b7-285">En svart, vit eller genomskinlig bakgrund är inte tillåten för hjälte ikoner.</span><span class="sxs-lookup"><span data-stu-id="804b7-285">A black, white, or transparent background isn't allowed for hero icons.</span></span>
*   <span data-ttu-id="804b7-286">När erbjudandet visas utgivaren visar namn, planera rubrik, erbjudandet lång sammanfattning och **skapa** knappen inbäddad programmässigt i hjälte logotypen.</span><span class="sxs-lookup"><span data-stu-id="804b7-286">After the offer is listed, the publisher display name, the plan title, the offer long summary, and the **Create** button are embedded programmatically inside the hero logo.</span></span> <span data-ttu-id="804b7-287">Därför inte ange valfri text när du utformar hjälte logotypen.</span><span class="sxs-lookup"><span data-stu-id="804b7-287">Consequently, don't enter any text while you design the hero logo.</span></span> <span data-ttu-id="804b7-288">Lämna tomt utrymme till höger eftersom texten programmässigt ingår i detta utrymme.</span><span class="sxs-lookup"><span data-stu-id="804b7-288">Leave empty space on the right because the text is included programmatically in that space.</span></span> <span data-ttu-id="804b7-289">Det tomma utrymmet för texten som ska vara 415 x 100 bildpunkter till höger.</span><span class="sxs-lookup"><span data-stu-id="804b7-289">The empty space for the text should be 415 x 100 pixels on the right.</span></span> <span data-ttu-id="804b7-290">Det är förskjutning med 370 bildpunkter från vänster.</span><span class="sxs-lookup"><span data-stu-id="804b7-290">It's offset by 370 pixels from the left.</span></span>

    ![Hjälte logotypen exempel](./media/managed-application-author-marketplace/publishvm14.png)

## <a name="support-form"></a><span data-ttu-id="804b7-292">Stöd för formulär</span><span class="sxs-lookup"><span data-stu-id="804b7-292">Support form</span></span>

<span data-ttu-id="804b7-293">Fyll i den **stöder** formuläret med stöd för kontakter från ditt företag.</span><span class="sxs-lookup"><span data-stu-id="804b7-293">Fill out the **Support** form with support contacts from your company.</span></span> <span data-ttu-id="804b7-294">Den här informationen kan vara engineering customer support kontakter.</span><span class="sxs-lookup"><span data-stu-id="804b7-294">This information might be engineering contacts and customer support contacts.</span></span>

## <a name="publish-an-offer"></a><span data-ttu-id="804b7-295">Publicera ett erbjudande</span><span class="sxs-lookup"><span data-stu-id="804b7-295">Publish an offer</span></span>

<span data-ttu-id="804b7-296">När du fyller i alla avsnitt markerar **publicera** att starta processen som gör erbjudandet tillgängliga för kunder.</span><span class="sxs-lookup"><span data-stu-id="804b7-296">After you fill out all the sections, select **Publish** to start the process that makes your offer available to customers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="804b7-297">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="804b7-297">Next steps</span></span>

* <span data-ttu-id="804b7-298">En introduktion till hanterade program, se [hanteras Programöversikt](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="804b7-298">For an introduction to managed applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="804b7-299">Information om hur du använder ett hanterat program från Marketplace finns [använda Azure hanterade program i Marketplace](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="804b7-299">For information about consuming a managed application from the Marketplace, see [Consume Azure managed applications in the Marketplace](managed-application-consume-marketplace.md).</span></span>
* <span data-ttu-id="804b7-300">Information om hur du publicerar ett program för Tjänstkatalog hanteras finns [skapa och publicera en applikation för Tjänstkatalog hanteras](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="804b7-300">For information about publishing a Service Catalog managed application, see [Create and publish a Service Catalog managed application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="804b7-301">Information om att använda ett Tjänstkatalogen hanterade program, se [använder ett Tjänstkatalogen hanterade program](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="804b7-301">For information about consuming a Service Catalog managed application, see [Consume a Service Catalog managed application](managed-application-consumption.md).</span></span>
