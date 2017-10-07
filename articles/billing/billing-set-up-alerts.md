---
title: "aaaSet in fakturerings- eller varningar för Azure-prenumerationer | Microsoft Docs"
description: "Beskriver hur du kan ställa in aviseringar på fakturan Azure så att du kan undvika fakturering överraskningar."
keywords: "avisering för kredit, fakturering avisering"
services: 
documentationcenter: 
author: vikdesai
manager: tonguyen
editor: 
tags: billing
ms.assetid: 9b7b3eeb-cd9d-4690-86a3-51b1e2a8974f
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: vikdesai
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 711b9c72c59874792b0e229cdc5ec0fa517c24c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a><span data-ttu-id="b33f7-104">Ställ in fakturerings- eller varningar för Microsoft Azure-prenumerationer</span><span class="sxs-lookup"><span data-stu-id="b33f7-104">Set up billing or credit alerts for your Microsoft Azure subscriptions</span></span>
<span data-ttu-id="b33f7-105">Om du är hello kontoadministratören för en Azure-prenumeration kan använda du hello Azure Billing-aviseringstjänsten toocreate anpassade fakturering aviseringar som hjälper dig att övervaka och hantera fakturerings-aktivitet för Azure-konton.</span><span class="sxs-lookup"><span data-stu-id="b33f7-105">If you’re hello Account Admin for an Azure subscription, you can use hello Azure Billing Alert Service toocreate customized billing alerts that help you monitor and manage billing activity for your Azure accounts.</span></span>

<span data-ttu-id="b33f7-106">Den här tjänsten är i förhandsgranskningen, så du måste tooenable den hello förhandsgranskningsfunktioner sidan först.</span><span class="sxs-lookup"><span data-stu-id="b33f7-106">This service is in preview, so you need tooenable it in hello Preview Features page first.</span></span>

## <a name="set-hello-alert-threshold-and-email-recipients"></a><span data-ttu-id="b33f7-107">Ange hello tröskelvärde och e-post signalmottagare</span><span class="sxs-lookup"><span data-stu-id="b33f7-107">Set hello alert threshold and email recipients</span></span>
1. <span data-ttu-id="b33f7-108">Besök [hello förhandsgranskningsfunktioner sidan](https://account.windowsazure.com/PreviewFeatures) och aktivera **fakturering aviseringstjänsten**.</span><span class="sxs-lookup"><span data-stu-id="b33f7-108">Visit [hello Preview Features page](https://account.windowsazure.com/PreviewFeatures) and enable **Billing Alert Service**.</span></span>

1. <span data-ttu-id="b33f7-109">När du har fått hello e-postbekräftelse som hello faktureringen är aktiverad för din prenumeration besöker [hello prenumerationssidan](https://account.windowsazure.com/Subscriptions) hello-kontoportalen.</span><span class="sxs-lookup"><span data-stu-id="b33f7-109">After you receive hello email confirmation that hello billing service is turned on for your subscription, visit [hello Subscriptions page](https://account.windowsazure.com/Subscriptions) in hello account portal.</span></span> <span data-ttu-id="b33f7-110">Klicka på hello prenumeration du vill toomonitor och klicka sedan på **aviseringar**.</span><span class="sxs-lookup"><span data-stu-id="b33f7-110">Click hello subscription you want toomonitor, and then click **Alerts**.</span></span>

    ![Skärmbild av hello Prenumerationsvy över Azure kontocenter med aviseringar markerat][Image1]

2. <span data-ttu-id="b33f7-112">Klicka sedan på **Lägg till avisering** toocreate din första en.</span><span class="sxs-lookup"><span data-stu-id="b33f7-112">Next, click **Add Alert** toocreate your first one.</span></span> <span data-ttu-id="b33f7-113">Du kan ange en summa av fem fakturaaviseringar per prenumeration med ett annat tröskelvärde och uppåt tootwo e-postmottagare för varje avisering.</span><span class="sxs-lookup"><span data-stu-id="b33f7-113">You can set up a total of five billing alerts per subscription, with a different threshold and up tootwo email recipients for each alert.</span></span>

    ![Skärmbild av hello aviseringar visas där du kan lägga till avisering][Image2]

3. <span data-ttu-id="b33f7-115">När du lägger till en avisering du ge det ett unikt namn väljer ett tröskelvärde för utgiftsgräns och välj hello e-postadresser som aviseringar skickas.</span><span class="sxs-lookup"><span data-stu-id="b33f7-115">When you add an alert, you give it a unique name, choose a spending threshold, and choose hello email addresses where alerts are sent.</span></span> <span data-ttu-id="b33f7-116">När du ställer in hello tröskelvärdet, kan du välja antingen en **Billing Total** eller en **Penningkredit** från hello **avisering för** lista.</span><span class="sxs-lookup"><span data-stu-id="b33f7-116">When setting up hello threshold, you can choose either a **Billing Total** or a **Monetary Credit** from hello **Alert For** list.</span></span> <span data-ttu-id="b33f7-117">Sammanlagt fakturering skickas en avisering när prenumerationen utgifter överskrider tröskelvärdet för hello.</span><span class="sxs-lookup"><span data-stu-id="b33f7-117">For a billing total, an alert is sent when subscription spending exceeds hello threshold.</span></span> <span data-ttu-id="b33f7-118">För en penningkredit skickas en avisering när penningkrediten sjunker under hello gränsen.</span><span class="sxs-lookup"><span data-stu-id="b33f7-118">For a monetary credit, an alert is sent when monetary credits drop below hello limit.</span></span> <span data-ttu-id="b33f7-119">Penningkrediten gäller vanligtvis tooFree utvärderingsversionen och Visual Studio-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="b33f7-119">Monetary credits usually apply tooFree Trial and Visual Studio subscriptions.</span></span>

    ![Skärmbild av hello avisering tillägg view, där du kan konfigurera mottagare][Image3]

<span data-ttu-id="b33f7-121">Azure har stöd för en e-postadress men inte kontrollera att hello e-postadressen fungerar, så dubbelkolla för skrivfel.</span><span class="sxs-lookup"><span data-stu-id="b33f7-121">Azure supports any email address but doesn't verify that hello email address works, so double-check for typos.</span></span>

## <a name="check-on-your-alerts"></a><span data-ttu-id="b33f7-122">Kontrollera på aviseringar</span><span class="sxs-lookup"><span data-stu-id="b33f7-122">Check on your alerts</span></span>
<span data-ttu-id="b33f7-123">När du har skapat aviseringar hello Account Center visas dem och hur många fler kan du ställa in.</span><span class="sxs-lookup"><span data-stu-id="b33f7-123">After you set up alerts, hello Account Center lists them and shows how many more you can set up.</span></span> <span data-ttu-id="b33f7-124">För varje avisering finns hello datum och tid som det har skickats, oavsett om det är en avisering om Billing Total eller kredit och hello gräns som du konfigurerar.</span><span class="sxs-lookup"><span data-stu-id="b33f7-124">For each alert, you see hello date and time it was sent, whether it’s an alert for Billing Total or Monetary Credit, and hello limit you set up.</span></span> <span data-ttu-id="b33f7-125">hello format för datum och tid är 24 timmar universaltid samordna (UTC) och hello datum är åååå-mm-dd-formatet.</span><span class="sxs-lookup"><span data-stu-id="b33f7-125">hello date and time format is 24-hour Universal Time Coordinate (UTC) and hello date is yyyy-mm-dd format.</span></span> <span data-ttu-id="b33f7-126">Klicka på hello plus registrera för en avisering i hello listan tooedit den eller hello-papperskorg toodelete den.</span><span class="sxs-lookup"><span data-stu-id="b33f7-126">Click hello plus sign for an alert in hello list tooedit it, or click hello trash-can toodelete it.</span></span>

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a><span data-ttu-id="b33f7-127">Fakturering aviseringar för kunder med Enterprise-avtal (EA)</span><span class="sxs-lookup"><span data-stu-id="b33f7-127">Billing alerts for Enterprise Agreement (EA) customers</span></span>
<span data-ttu-id="b33f7-128">EA kunder kan få aviseringar för varje avdelning under en registrering med inställningen utgifter kvoter.</span><span class="sxs-lookup"><span data-stu-id="b33f7-128">EA customers can get alerts for each department under an enrollment by setting spending quotas.</span></span> <span data-ttu-id="b33f7-129">Se [avdelning utgifter kvoter](https://ea.azure.com/helpdocs/departmentSpendingQuotas) i hello EA portal tooget igång.</span><span class="sxs-lookup"><span data-stu-id="b33f7-129">See [Department Spending Quotas](https://ea.azure.com/helpdocs/departmentSpendingQuotas) in hello EA portal tooget started.</span></span>

## <a name="learn-more-about-azure-cost-management"></a><span data-ttu-id="b33f7-130">Lär dig mer om hantering av Azure kostnad</span><span class="sxs-lookup"><span data-stu-id="b33f7-130">Learn more about Azure cost management</span></span>
- <span data-ttu-id="b33f7-131">Uppskatta kostnader med hello [prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/calculator/), [totalkostnad för ägarskap Kalkylatorn](https://aka.ms/azure-tco-calculator), och när du lägger till en tjänst.</span><span class="sxs-lookup"><span data-stu-id="b33f7-131">Estimate costs using hello [pricing calculator](https://azure.microsoft.com/pricing/calculator/), [total cost of ownership calculator](https://aka.ms/azure-tco-calculator), and when you add a service.</span></span>
- <span data-ttu-id="b33f7-132">[Granska din användning och kostnader regelbundet i Azure-portalen](billing-getting-started.md#costs).</span><span class="sxs-lookup"><span data-stu-id="b33f7-132">[Review your usage and costs regularly in Azure portal](billing-getting-started.md#costs).</span></span>
- <span data-ttu-id="b33f7-133">Aktivera [Azure Advisor kostnad rekommendationer](../advisor/advisor-cost-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="b33f7-133">Turn on [Azure Advisor cost recommendations](../advisor/advisor-cost-recommendations.md).</span></span>

<span data-ttu-id="b33f7-134">Det finns fler toolearn [Azure kostnad management vägledning](billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="b33f7-134">toolearn more, see [Azure cost management guidance](billing-getting-started.md).</span></span>

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
