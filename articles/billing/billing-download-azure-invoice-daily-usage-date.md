---
title: "Hämta Azure billing faktura och dagligen användningsdata | Microsoft Docs"
description: "Beskriver hur du laddar ned eller visa din Azure faktura och daglig användning faktureringsinformation."
keywords: "fakturering faktura, ladda ned faktura, azure faktura, azure användning"
services: 
documentationcenter: 
author: genlin
manager: tonguyen
editor: 
tags: billing
ms.assetid: 6d568d1d-3bd6-4348-97d0-1098b5fe0661
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c63d9523555f47b4e5c0f3766f3ba080f76ac19e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a><span data-ttu-id="4e789-104">Hämta eller visa din Azure fakturering faktura och dagliga användningsdata</span><span class="sxs-lookup"><span data-stu-id="4e789-104">Download or view your Azure billing invoice and daily usage data</span></span>
<span data-ttu-id="4e789-105">Du kan hämta fakturan från den [Azure-portalen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) eller låta skickas via e-post.</span><span class="sxs-lookup"><span data-stu-id="4e789-105">You can download your invoice from the [Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) or have it sent in email.</span></span> <span data-ttu-id="4e789-106">För att hämta det dagliga arbetet, gå till den [Azure Kontocenter](https://account.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="4e789-106">To download your daily usage, go to the [Azure Account Center](https://account.windowsazure.com).</span></span> <span data-ttu-id="4e789-107">Endast vissa roller som har behörighet att hämta faktureringsinformation faktura och information om användning, som kontoadministratör.</span><span class="sxs-lookup"><span data-stu-id="4e789-107">Only certain roles have permission to get billing invoice and usage information, like the Account Administrator.</span></span> <span data-ttu-id="4e789-108">Läs mer om att få tillgång till faktureringsinformationen i [hantera åtkomst till Azure fakturering med roller](billing-manage-access.md).</span><span class="sxs-lookup"><span data-stu-id="4e789-108">To learn more about getting access to billing information, see [Manage access to Azure billing using roles](billing-manage-access.md).</span></span>

## <a name="get-your-invoice-in-email-pdf"></a><span data-ttu-id="4e789-109">Hämta fakturan i e-post (PDF)</span><span class="sxs-lookup"><span data-stu-id="4e789-109">Get your invoice in email (.pdf)</span></span>
<span data-ttu-id="4e789-110">Du kan välja och konfigurera ytterligare mottagare för att ta emot din Azure faktura i ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="4e789-110">You can opt in and configure additional recipients to receive your Azure invoice in an email.</span></span> <span data-ttu-id="4e789-111">Den här funktionen är kanske inte tillgängliga för vissa prenumerationer som stöd för erbjudanden, Enterprise-avtal eller Azure i Open.</span><span class="sxs-lookup"><span data-stu-id="4e789-111">This feature may not be available for certain subscriptions such as support offers, Enterprise Agreements, or Azure in Open.</span></span>

1. <span data-ttu-id="4e789-112">Välj din prenumeration från den [prenumerationer bladet](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).</span><span class="sxs-lookup"><span data-stu-id="4e789-112">Select your subscription from the [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).</span></span> <span data-ttu-id="4e789-113">Anmäl för varje prenumeration som du äger.</span><span class="sxs-lookup"><span data-stu-id="4e789-113">Opt-in for each subscription you own.</span></span> <span data-ttu-id="4e789-114">Klicka på **fakturor** sedan **e-min faktura**.</span><span class="sxs-lookup"><span data-stu-id="4e789-114">Click **Invoices** then **Email my invoice**.</span></span> 

    ![Skärmbild som visar opt-in-flöde](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. <span data-ttu-id="4e789-116">Klicka på **välja** och acceptera villkoren.</span><span class="sxs-lookup"><span data-stu-id="4e789-116">Click **Opt in** and accept the terms.</span></span>

    ![Skärmbild som visar opt-in-flöde](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. <span data-ttu-id="4e789-118">När du har accepterat avtalet, kan du konfigurera ytterligare mottagare.</span><span class="sxs-lookup"><span data-stu-id="4e789-118">Once you've accepted the agreement, you can configure additional recipients.</span></span>

    ![Skärmbild som visar opt-in-flöde](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
<span data-ttu-id="4e789-120">Om du inte får ett e-postmeddelande när du har följt stegen, kontrollera din e-postadress är korrekt i den [kommunikationsinställningar i din profil](https://account.windowsazure.com/profile).</span><span class="sxs-lookup"><span data-stu-id="4e789-120">If you don't get an email after following the steps, make sure your email address is correct in the [communication preferences on your profile](https://account.windowsazure.com/profile).</span></span>

## <a name="download-invoice-from-azure-portal-pdf"></a><span data-ttu-id="4e789-121">Ladda ned faktura från Azure-portalen (PDF)</span><span class="sxs-lookup"><span data-stu-id="4e789-121">Download invoice from Azure portal (.pdf)</span></span>

1. <span data-ttu-id="4e789-122">Välj din prenumeration från den [prenumerationer bladet](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) i Azure-portalen som [en användare med tillgång till fakturor](billing-manage-access.md).</span><span class="sxs-lookup"><span data-stu-id="4e789-122">Select your subscription from the [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal as [an user with access to invoices](billing-manage-access.md).</span></span>

2. <span data-ttu-id="4e789-123">Välj **fakturor**.</span><span class="sxs-lookup"><span data-stu-id="4e789-123">Select **Invoices**.</span></span> 

    ![Skärmbild som visar alternativet fakturerings- och användningsdata](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. <span data-ttu-id="4e789-125">Klicka på **ladda ned faktura** att visa en kopia av fakturan PDF.</span><span class="sxs-lookup"><span data-stu-id="4e789-125">Click **Download Invoice** to view a copy of your PDF invoice.</span></span> <span data-ttu-id="4e789-126">Om det står **inte tillgänglig**, se [Varför ser jag en faktura för den senaste faktureringsperioden?](#noinvoice)</span><span class="sxs-lookup"><span data-stu-id="4e789-126">If it says **Not available**, see [Why don't I see an invoice for the last billing period?](#noinvoice)</span></span>

    ![Skärmbild som visar fakturering punkter, hämtningsalternativ och totala debiteringen för varje faktureringsperioden](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. <span data-ttu-id="4e789-128">Du kan också visa det dagliga arbetet genom att klicka på faktureringsperioden.</span><span class="sxs-lookup"><span data-stu-id="4e789-128">You can also view your daily usage by clicking the billing period.</span></span> 

<span data-ttu-id="4e789-129">Mer information om fakturan finns [förstå fakturan för Microsoft Azure](billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="4e789-129">For more information about your invoice, see [Understand your bill for Microsoft Azure](billing-understand-your-bill.md).</span></span> <span data-ttu-id="4e789-130">Hjälp med att hantera kostnader finns [förhindrar oväntade kostnader med Azure fakturerings- och kostnaden management](billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="4e789-130">For help managing costs, see [Prevent unexpected costs with Azure billing and cost management](billing-getting-started.md).</span></span>

## <a name="download-usage-from-the-account-center-csv"></a><span data-ttu-id="4e789-131">Ladda ned användning från Kontocenter (.csv)</span><span class="sxs-lookup"><span data-stu-id="4e789-131">Download usage from the Account Center (.csv)</span></span>

1. <span data-ttu-id="4e789-132">Logga in på den [Azure Kontocenter](https://account.windowsazure.com/subscriptions) som kontoadministratör.</span><span class="sxs-lookup"><span data-stu-id="4e789-132">Sign into the [Azure Account Center](https://account.windowsazure.com/subscriptions) as the Account Administrator.</span></span>

2. <span data-ttu-id="4e789-133">Välj den prenumeration som du vill ha information om faktura och användning.</span><span class="sxs-lookup"><span data-stu-id="4e789-133">Select the subscription for which you want the invoice and usage information.</span></span>

3. <span data-ttu-id="4e789-134">Välj **FAKTURERING HISTORIK**.</span><span class="sxs-lookup"><span data-stu-id="4e789-134">Select **BILLING HISTORY**.</span></span> 

    ![Skärmbild som visar historik betalningsalternativ](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. <span data-ttu-id="4e789-136">Du kan se din instruktioner för de senaste sex fakturering perioderna och aktuell ofakturerad period.</span><span class="sxs-lookup"><span data-stu-id="4e789-136">You can see your statements for the last six billing periods and the current unbilled period.</span></span> 

    ![Skärmbild som visar fakturering perioder, alternativ för att ladda ned faktura och daglig användning och totala debiteringen för varje faktureringsperioden](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. <span data-ttu-id="4e789-138">Välj **Visa aktuella instruktionen** att se en uppskattning av dina debiteringar när uppskattningen har genererats.</span><span class="sxs-lookup"><span data-stu-id="4e789-138">Select **View Current Statement** to see an estimate of your charges at the time the estimate was generated.</span></span> <span data-ttu-id="4e789-139">Den här informationen uppdateras bara dagligen och får inte innehålla din användning.</span><span class="sxs-lookup"><span data-stu-id="4e789-139">This information is only updated daily and may not include all your usage.</span></span> <span data-ttu-id="4e789-140">Fakturan månatliga kan skilja sig från den här beräkningen.</span><span class="sxs-lookup"><span data-stu-id="4e789-140">Your monthly invoice may differ from this estimate.</span></span>

    ![Skärmbild som visar alternativet Visa aktuella instruktionen](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Skärmbild som visar uppskattning av aktuella avgifter](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. <span data-ttu-id="4e789-143">Välj **ladda ned användning** att hämta dagliga användningsdata som en CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="4e789-143">Select **Download Usage** to download the daily usage data as a CSV file.</span></span> <span data-ttu-id="4e789-144">Om du ser två versioner som är tillgängliga, hämta version 2.</span><span class="sxs-lookup"><span data-stu-id="4e789-144">If you see two versions available, download version 2.</span></span>

    ![Skärmbild som visar alternativet ladda ned användning](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

<span data-ttu-id="4e789-146">Enbart administratörskontot kan komma åt Azure Kontocenter.</span><span class="sxs-lookup"><span data-stu-id="4e789-146">Only the Account Administrator can access the Azure Account Center.</span></span> <span data-ttu-id="4e789-147">Andra fakturering administratörer, till exempel en ägare får användning informationen med hjälp av den [fakturerings-API: er](billing-usage-rate-card-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4e789-147">Other billing admins, such as an Owner, can get usage information using the [Billing APIs](billing-usage-rate-card-overview.md).</span></span>

<span data-ttu-id="4e789-148">Mer information om det dagliga arbetet finns [förstå fakturan för Microsoft Azure](billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="4e789-148">For more information about your daily usage, see [Understand your bill for Microsoft Azure](billing-understand-your-bill.md).</span></span> <span data-ttu-id="4e789-149">Hjälp med att hantera kostnader finns [förhindrar oväntade kostnader med Azure fakturerings- och kostnaden management](billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="4e789-149">For help managing costs, see [Prevent unexpected costs with Azure billing and cost management](billing-getting-started.md).</span></span>

## <span data-ttu-id="4e789-150"><a name="noinvoice"></a>Varför ser jag en faktura för den senaste faktureringsperioden</span><span class="sxs-lookup"><span data-stu-id="4e789-150"><a name="noinvoice"></a> Why don't I see an invoice for the last billing period?</span></span>

<span data-ttu-id="4e789-151">Det kan finnas flera skäl till att du inte ser en faktura:</span><span class="sxs-lookup"><span data-stu-id="4e789-151">There could be several reasons that you don't see an invoice:</span></span>

- <span data-ttu-id="4e789-152">Du har ett månatliga kreditbelopp med din prenumeration som du inte överskrider eller du har en kostnadsfri utvärderingsversion.</span><span class="sxs-lookup"><span data-stu-id="4e789-152">You have a monthly credit amount with your subscription that you didn't exceed or you have a Free Trial.</span></span> <span data-ttu-id="4e789-153">En faktura skapas bara när du är skyldig pengar.</span><span class="sxs-lookup"><span data-stu-id="4e789-153">An invoice is only generated when you owe money.</span></span>

- <span data-ttu-id="4e789-154">Det är mindre än 30 dagar från den dag som du prenumererar på Azure.</span><span class="sxs-lookup"><span data-stu-id="4e789-154">It's less than 30 days from the day you subscribed to Azure.</span></span>

- <span data-ttu-id="4e789-155">Fakturan är inte genererats ännu.</span><span class="sxs-lookup"><span data-stu-id="4e789-155">The invoice isn't generated yet.</span></span> <span data-ttu-id="4e789-156">Vänta till slutet av faktureringsperioden.</span><span class="sxs-lookup"><span data-stu-id="4e789-156">Wait until the end of the billing period.</span></span>

- <span data-ttu-id="4e789-157">Om du inte är kontoadministratören kanske inte äldre fakturor avaialbe till dig.</span><span class="sxs-lookup"><span data-stu-id="4e789-157">If you're not the Account Administrator, older invoices may not be avaialbe to you.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="4e789-158">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="4e789-158">Need help?</span></span> <span data-ttu-id="4e789-159">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="4e789-159">Contact support.</span></span>
<span data-ttu-id="4e789-160">Om du fortfarande har fler frågor, [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) få snabbt lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="4e789-160">If you still have further questions, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>

