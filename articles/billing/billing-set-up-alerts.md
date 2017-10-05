---
title: "Ställ in fakturerings- eller varningar för Azure-prenumerationer | Microsoft Docs"
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
ms.openlocfilehash: 7a1b579fdde831fdc3afa0a2aee4c24890216ed1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a><span data-ttu-id="fc463-104">Ställ in fakturerings- eller varningar för Microsoft Azure-prenumerationer</span><span class="sxs-lookup"><span data-stu-id="fc463-104">Set up billing or credit alerts for your Microsoft Azure subscriptions</span></span>
<span data-ttu-id="fc463-105">Om du är kontoadministratören för en Azure-prenumeration kan du använda fakturering aviseringstjänsten Azure för att skapa anpassade fakturaaviseringar som hjälper dig övervaka och hantera fakturerings-aktivitet för Azure-konton.</span><span class="sxs-lookup"><span data-stu-id="fc463-105">If you’re the Account Admin for an Azure subscription, you can use the Azure Billing Alert Service to create customized billing alerts that help you monitor and manage billing activity for your Azure accounts.</span></span>

<span data-ttu-id="fc463-106">Tjänsten är i förhandsvisning, så du måste aktivera det på sidan funktioner för förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="fc463-106">This service is in preview, so you need to enable it in the Preview Features page first.</span></span>

## <a name="set-the-alert-threshold-and-email-recipients"></a><span data-ttu-id="fc463-107">Ange signalmottagare tröskelvärde och e-post</span><span class="sxs-lookup"><span data-stu-id="fc463-107">Set the alert threshold and email recipients</span></span>
1. <span data-ttu-id="fc463-108">Besök [sidan förhandsgranskningsfunktioner](https://account.windowsazure.com/PreviewFeatures) och aktivera **fakturering aviseringstjänsten**.</span><span class="sxs-lookup"><span data-stu-id="fc463-108">Visit [the Preview Features page](https://account.windowsazure.com/PreviewFeatures) and enable **Billing Alert Service**.</span></span>

1. <span data-ttu-id="fc463-109">När du har fått e-postbekräftelse som faktureringen är aktiverad för din prenumeration besöker [prenumerationssidan](https://account.windowsazure.com/Subscriptions) i kontoportalen.</span><span class="sxs-lookup"><span data-stu-id="fc463-109">After you receive the email confirmation that the billing service is turned on for your subscription, visit [the Subscriptions page](https://account.windowsazure.com/Subscriptions) in the account portal.</span></span> <span data-ttu-id="fc463-110">Klicka på den prenumeration som du vill övervaka och klicka sedan på **aviseringar**.</span><span class="sxs-lookup"><span data-stu-id="fc463-110">Click the subscription you want to monitor, and then click **Alerts**.</span></span>

    ![Skärmbild av prenumerationsvyn i Azure kontocenter med aviseringar markerat][Image1]

2. <span data-ttu-id="fc463-112">Klicka sedan på **Lägg till avisering** att skapa din första.</span><span class="sxs-lookup"><span data-stu-id="fc463-112">Next, click **Add Alert** to create your first one.</span></span> <span data-ttu-id="fc463-113">Du kan ställa in totalt fem fakturaaviseringar per prenumeration, med ett annat tröskelvärde och upp till två e-postmottagare för varje avisering.</span><span class="sxs-lookup"><span data-stu-id="fc463-113">You can set up a total of five billing alerts per subscription, with a different threshold and up to two email recipients for each alert.</span></span>

    ![Skärmbild av vyn aviseringar, där du kan lägga till avisering][Image2]

3. <span data-ttu-id="fc463-115">När du lägger till en avisering du ge det ett unikt namn väljer ett tröskelvärde för utgiftsgräns och välj e-postadresser som aviseringar skickas.</span><span class="sxs-lookup"><span data-stu-id="fc463-115">When you add an alert, you give it a unique name, choose a spending threshold, and choose the email addresses where alerts are sent.</span></span> <span data-ttu-id="fc463-116">När du ställer in tröskelvärdet, kan du välja antingen en **Billing Total** eller en **Penningkredit** från den **avisering för** lista.</span><span class="sxs-lookup"><span data-stu-id="fc463-116">When setting up the threshold, you can choose either a **Billing Total** or a **Monetary Credit** from the **Alert For** list.</span></span> <span data-ttu-id="fc463-117">Fakturering Totalt skickas en avisering när prenumerationen utgifter överstiger tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="fc463-117">For a billing total, an alert is sent when subscription spending exceeds the threshold.</span></span> <span data-ttu-id="fc463-118">För en penningkredit skickas en avisering när penningkrediten sjunker under gränsen.</span><span class="sxs-lookup"><span data-stu-id="fc463-118">For a monetary credit, an alert is sent when monetary credits drop below the limit.</span></span> <span data-ttu-id="fc463-119">Penningkrediten gäller vanligtvis prenumerationer kostnadsfri utvärderingsversion och Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc463-119">Monetary credits usually apply to Free Trial and Visual Studio subscriptions.</span></span>

    ![Skärmbild av vyn avisering tillägg, där du kan konfigurera mottagare][Image3]

<span data-ttu-id="fc463-121">Azure stöder en e-postadress men Kontrollera inte att e-postadressen fungerar, så dubbelkolla för skrivfel.</span><span class="sxs-lookup"><span data-stu-id="fc463-121">Azure supports any email address but doesn't verify that the email address works, so double-check for typos.</span></span>

## <a name="check-on-your-alerts"></a><span data-ttu-id="fc463-122">Kontrollera på aviseringar</span><span class="sxs-lookup"><span data-stu-id="fc463-122">Check on your alerts</span></span>
<span data-ttu-id="fc463-123">När du har skapat aviseringar Account Center visas dem och hur många fler kan du ställa in.</span><span class="sxs-lookup"><span data-stu-id="fc463-123">After you set up alerts, the Account Center lists them and shows how many more you can set up.</span></span> <span data-ttu-id="fc463-124">För varje avisering måste se du datum och tid som det har skickats, oavsett om det är en avisering om Billing Total eller kredit och den gräns som du konfigurerar.</span><span class="sxs-lookup"><span data-stu-id="fc463-124">For each alert, you see the date and time it was sent, whether it’s an alert for Billing Total or Monetary Credit, and the limit you set up.</span></span> <span data-ttu-id="fc463-125">Formatet för datum och tid är 24-timmarsformat universaltid samordna (UTC) och datum är åååå-mm-dd-formatet.</span><span class="sxs-lookup"><span data-stu-id="fc463-125">The date and time format is 24-hour Universal Time Coordinate (UTC) and the date is yyyy-mm-dd format.</span></span> <span data-ttu-id="fc463-126">Klicka på plustecknet för en avisering i listan om du vill redigera den eller klicka på Papperskorgen för att ta bort den.</span><span class="sxs-lookup"><span data-stu-id="fc463-126">Click the plus sign for an alert in the list to edit it, or click the trash-can to delete it.</span></span>

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a><span data-ttu-id="fc463-127">Fakturering aviseringar för kunder med Enterprise-avtal (EA)</span><span class="sxs-lookup"><span data-stu-id="fc463-127">Billing alerts for Enterprise Agreement (EA) customers</span></span>
<span data-ttu-id="fc463-128">EA kunder kan få aviseringar för varje avdelning under en registrering med inställningen utgifter kvoter.</span><span class="sxs-lookup"><span data-stu-id="fc463-128">EA customers can get alerts for each department under an enrollment by setting spending quotas.</span></span> <span data-ttu-id="fc463-129">Se [avdelning utgifter kvoter](https://ea.azure.com/helpdocs/departmentSpendingQuotas) i EA-portalen för att komma igång.</span><span class="sxs-lookup"><span data-stu-id="fc463-129">See [Department Spending Quotas](https://ea.azure.com/helpdocs/departmentSpendingQuotas) in the EA portal to get started.</span></span>

## <a name="learn-more-about-azure-cost-management"></a><span data-ttu-id="fc463-130">Lär dig mer om hantering av Azure kostnad</span><span class="sxs-lookup"><span data-stu-id="fc463-130">Learn more about Azure cost management</span></span>
- <span data-ttu-id="fc463-131">Beräkna kostnader med hjälp av den [prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/calculator/), [totalkostnad för ägarskap Kalkylatorn](https://aka.ms/azure-tco-calculator), och när du lägger till en tjänst.</span><span class="sxs-lookup"><span data-stu-id="fc463-131">Estimate costs using the [pricing calculator](https://azure.microsoft.com/pricing/calculator/), [total cost of ownership calculator](https://aka.ms/azure-tco-calculator), and when you add a service.</span></span>
- <span data-ttu-id="fc463-132">[Granska din användning och kostnader regelbundet i Azure-portalen](billing-getting-started.md#costs).</span><span class="sxs-lookup"><span data-stu-id="fc463-132">[Review your usage and costs regularly in Azure portal](billing-getting-started.md#costs).</span></span>
- <span data-ttu-id="fc463-133">Aktivera [Azure Advisor kostnad rekommendationer](../advisor/advisor-cost-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="fc463-133">Turn on [Azure Advisor cost recommendations](../advisor/advisor-cost-recommendations.md).</span></span>

<span data-ttu-id="fc463-134">Läs mer i [Azure kostnad management vägledning](billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="fc463-134">To learn more, see [Azure cost management guidance](billing-getting-started.md).</span></span>

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
