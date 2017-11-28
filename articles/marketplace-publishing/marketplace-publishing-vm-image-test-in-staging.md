---
title: "den virtuella datorn erbjuder för hello Marketplace aaaTest | Microsoft Docs"
description: "Förstå hur tootest din virtuella dator bild för hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 7a41c3c6-625c-4478-b804-e124dee89040
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: hascipio
ms.openlocfilehash: ab166d2c3c536810a3a8f48330f0482b9b4e58d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="test-your-vm-offer-for-hello-azure-marketplace-in-staging"></a><span data-ttu-id="f02d4-103">Testa din VM-erbjudande för hello Azure Marketplace under mellanlagring</span><span class="sxs-lookup"><span data-stu-id="f02d4-103">Test your VM offer for hello Azure Marketplace in staging</span></span>
<span data-ttu-id="f02d4-104">Mellanlagring betyder att distribuera din SKU i ett privat ”sandbox” där du kan testa och validera dess funktioner innan du distribuerar den toohello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f02d4-104">Staging means deploying your SKU in a private “sandbox” where you can test and validate its functionality before deploying it toohello Marketplace.</span></span> <span data-ttu-id="f02d4-105">hello SKU visas i Förproduktion precis som tooa kund som har distribuerat den.</span><span class="sxs-lookup"><span data-stu-id="f02d4-105">hello SKU appears in staging just as it would tooa customer who has deployed it.</span></span> <span data-ttu-id="f02d4-106">VM-avbildning måste vara certifierad toobe pushas toostaging.</span><span class="sxs-lookup"><span data-stu-id="f02d4-106">Your VM image must be certified toobe pushed toostaging.</span></span>

## <a name="step-1-push-your-offer-toostaging"></a><span data-ttu-id="f02d4-107">Steg 1: Push erbjudande-toostaging</span><span class="sxs-lookup"><span data-stu-id="f02d4-107">Step 1: Push your offer toostaging</span></span>
1. <span data-ttu-id="f02d4-108">På hello **publicera** klickar du på **Push tooStaging**.</span><span class="sxs-lookup"><span data-stu-id="f02d4-108">On hello **Publish** tab, click **Push tooStaging**.</span></span>
   
    ![Rita](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)
2. <span data-ttu-id="f02d4-110">Om hello Publishing Portal meddelar dig om eventuella fel, rätta dem.</span><span class="sxs-lookup"><span data-stu-id="f02d4-110">If hello Publishing Portal notifies you of any errors, correct them.</span></span>
3. <span data-ttu-id="f02d4-111">I hello **vem som kan komma åt erbjudandet mellanlagrade?** dialogrutan Ange hello lista över Azure-prenumerationer som du ska använda toopreview erbjudandet i hello [Azure preview portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f02d4-111">In hello **Who can access your staged offer?** dialog box, enter hello list of Azure subscriptions that you will use toopreview your offer in hello [Azure preview portal](https://portal.azure.com).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="f02d4-112">Om virtuella datorer och lösningsmallar du **inte** godkända prenumerationer av typen CSP DreamSpark eller Azure i Open.</span><span class="sxs-lookup"><span data-stu-id="f02d4-112">In case of Virtual Machines and Solution templates, please **do not** whitelist subscriptions of type CSP, DreamSpark or Azure in Open.</span></span>
   > 
   > 

    > <span data-ttu-id="f02d4-113">Om virtuella datorer, när du klickar på knappen hello **PUSH tooSTAGING**, hello följande steg utförs bakom hello scen.</span><span class="sxs-lookup"><span data-stu-id="f02d4-113">In case of Virtual Machines, when you click on hello button **PUSH tooSTAGING**, hello following steps are performed behind hello scene.</span></span> <span data-ttu-id="f02d4-114">Du kommer att kunna tooview hello förloppet för varje steg i hello hello publicera fliken publicering av portalen.</span><span class="sxs-lookup"><span data-stu-id="f02d4-114">You will be able tooview hello progress of each step under hello PUBLISH tab in hello Publishing portal.</span></span> <span data-ttu-id="f02d4-115">Du måste kontrollera den här sidan med regelbundna intervall (tills hello status visar MELLANLAGRAD) alla felinformation som behöver korrigering från din slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f02d4-115">You must check this page at regular interval (until hello status shows STAGED) for any failure information which need correction from your end.</span></span>

    > - <span data-ttu-id="f02d4-116">Din fristående begäran går toohello certifikatutfärdare team som Validera hello vhd först.</span><span class="sxs-lookup"><span data-stu-id="f02d4-116">At first your staging request goes toohello certification team who validate hello vhd.</span></span> <span data-ttu-id="f02d4-117">Men om din begäran har gjort endast marknadsföring ändring, hoppas hello certifikatutfärdare steget över.</span><span class="sxs-lookup"><span data-stu-id="f02d4-117">However, if your request has got only marketing change, then hello certification step is skipped.</span></span>
    > - <span data-ttu-id="f02d4-118">När hello certifikatutfärdare är klar hello replikering av hello erbjudande start för alla Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="f02d4-118">Once hello certification is complete, replication of hello offer start across all hello Azure datacenters.</span></span> <span data-ttu-id="f02d4-119">Normalt tar 24 48hours för hello replikering toocomplete men kan ta upp tooa vecka beroende på hello storleken på hello vhd.</span><span class="sxs-lookup"><span data-stu-id="f02d4-119">It generally takes 24-48hours for hello replication toocomplete but may take up tooa week depending on hello size of hello vhd.</span></span> <span data-ttu-id="f02d4-120">Om din begäran har stött på marknadsföring bara ändra, sedan är hello replikering dock snabbare.</span><span class="sxs-lookup"><span data-stu-id="f02d4-120">However, if your request has got only marketing change, then hello replication is faster.</span></span>
    > - <span data-ttu-id="f02d4-121">När hello replikeringen är klar, sedan hello erbjudandet ska finnas i hello [Azure-portalen](http:/portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f02d4-121">When hello replication is complete, then hello offer will be available in hello [Azure portal](http:/portal.azure.com).</span></span> <span data-ttu-id="f02d4-122">Vid den tiden hello status blir MELLANLAGRAD i hello publicering av portalen.</span><span class="sxs-lookup"><span data-stu-id="f02d4-122">At that time hello status become STAGED in hello Publishing portal.</span></span> <span data-ttu-id="f02d4-123">En mellanlagrad erbjudandet är synliga i hello [Azure-portalen](http:/portal.azure.com) med enbart hello e-ID som associeras med hello med vilka hello erbjudande mellanlagras.</span><span class="sxs-lookup"><span data-stu-id="f02d4-123">A staged offer is visible in hello [Azure portal](http:/portal.azure.com) only using hello email id(s) associated with hello subscription with which hello offer is staged.</span></span>

1. <span data-ttu-id="f02d4-124">Logga in toohello [Azure preview portal](https://portal.azure.com) genom att använda en hello Azure-prenumerationer som anges i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="f02d4-124">Sign in toohello [Azure preview portal](https://portal.azure.com) by using one of hello Azure subscriptions listed in hello previous step.</span></span>
2. <span data-ttu-id="f02d4-125">Hitta erbjudandet och verifiera din VM avbildningen punkter:</span><span class="sxs-lookup"><span data-stu-id="f02d4-125">Find your offer and validate your VM image points:</span></span>
   
   * <span data-ttu-id="f02d4-126">Kontrollera att marknadsföring innehåll visas korrekt i hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f02d4-126">Make sure that marketing content shows up correctly in hello Marketplace.</span></span>
   * <span data-ttu-id="f02d4-127">Slutpunkt till slutpunkt för distributionen av hello VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="f02d4-127">End-to-end deployment of hello VM image.</span></span>
     
      ![img kartan portal](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [!IMPORTANT]
> <span data-ttu-id="f02d4-129">Erbjudandet finns kvar i Förproduktion tills du meddela Microsoft via hello Publishing Portal [**publicera** fliken > Klicka på hello **”begär godkännande tooPush tooProduction”**] att du är klar toopush tooproduction.</span><span class="sxs-lookup"><span data-stu-id="f02d4-129">Your offer will remain in staging until you notify Microsoft via hello Publishing Portal [**Publish** tab > click on hello button **"Request Approval tooPush tooProduction"**] that you are ready toopush tooproduction.</span></span> <span data-ttu-id="f02d4-130">Det här är en perfekt tid toohave alla medlemmar i gruppen kontrollera över allt inför erbjudandet ska visas.</span><span class="sxs-lookup"><span data-stu-id="f02d4-130">This is an ideal time toohave all members of your team check over everything in preparation for your offer going listed.</span></span>
> 
> <span data-ttu-id="f02d4-131">hello mellanlagring plattform är avsedd för testning hello erbjudandet i ett förhandsgranskningsläge för av hello utgivare.</span><span class="sxs-lookup"><span data-stu-id="f02d4-131">hello staging platform is designed for testing hello offer in a preview mode by hello publisher.</span></span> <span data-ttu-id="f02d4-132">Vi avråder med hjälp av den här platofrm för mobilkommunikationssystemet ändamål.</span><span class="sxs-lookup"><span data-stu-id="f02d4-132">We strongly discourage using this platofrm for commerical purposes.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="f02d4-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f02d4-133">Next steps</span></span>
<span data-ttu-id="f02d4-134">Nu när erbjudandet ”mellanlagras” och du har testat dess funktioner och marknadsföring innehåll, kan du fortsätta toohello slutliga publicering fasen **steg 4**: [distribuera ditt erbjudande toohello Marketplace](marketplace-publishing-push-to-production.md).</span><span class="sxs-lookup"><span data-stu-id="f02d4-134">Now that your offer is "staged" and you have tested its functionality and marketing content, you can proceed toohello final publishing phase, **Step 4**: [Deploying your offer toohello Marketplace](marketplace-publishing-push-to-production.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="f02d4-135">Se även</span><span class="sxs-lookup"><span data-stu-id="f02d4-135">See also</span></span>
* [<span data-ttu-id="f02d4-136">Komma igång: hur toopublish ett erbjudande toohello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="f02d4-136">Getting started: How toopublish an offer toohello Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

