---
title: "Förstå Azure externa avgifterna | Microsoft Docs"
description: "Läs mer om fakturering för externa tjänster, tidigare känt som Marketplace, avgifter i Azure."
services: 
documentationcenter: 
author: adpick
manager: tonguyen
editor: 
tags: billing
ms.assetid: 5e0e2a3c-d111-4054-8508-0c111c1b749b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: adpick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 11701ce0162113ef6c8e056d3a30fe1d8f702f92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="understand-your-azure-billing-for-external-service-charges"></a><span data-ttu-id="d6a66-103">Förstå Azure faktureringen för externa serviceavgifter</span><span class="sxs-lookup"><span data-stu-id="d6a66-103">Understand your Azure billing for external service charges</span></span>
<span data-ttu-id="d6a66-104">Externa tjänster för att anropa Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="d6a66-104">External services used to be called Azure Marketplace.</span></span> <span data-ttu-id="d6a66-105">I allmänhet de är tjänster som publicerats av tillgängliga för tredje part för Azure men är helt integrerade i Azure.</span><span class="sxs-lookup"><span data-stu-id="d6a66-105">Generally, they're services published by third-parties available for Azure but are integrated completely within Azure.</span></span> <span data-ttu-id="d6a66-106">Till exempel är ClearDB och SendGrid externa tjänster som du kan köpa i Azure, men inte har publicerats av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d6a66-106">For example, ClearDB and SendGrid are external services that you can purchase in Azure, but are not published by Microsoft.</span></span>

<span data-ttu-id="d6a66-107">När du etablerar en ny extern tjänst eller en resurs, visas en varning:</span><span class="sxs-lookup"><span data-stu-id="d6a66-107">When you provision a new external service or resource, a warning is shown:</span></span>

![Marketplace köpa varning](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

> [!NOTE]
> <span data-ttu-id="d6a66-109">Externa tjänster publiceras av företag som inte är Microsoft, men ibland också Microsoft-produkter kategoriseras som externa tjänster.</span><span class="sxs-lookup"><span data-stu-id="d6a66-109">External services are published by companies that are not Microsoft, but sometimes Microsoft products are also categorized as external services.</span></span>
> 
> 

## <a name="how-external-services-are-billed"></a><span data-ttu-id="d6a66-110">Hur faktureras externa tjänster</span><span class="sxs-lookup"><span data-stu-id="d6a66-110">How external services are billed</span></span>
- <span data-ttu-id="d6a66-111">Externa tjänster faktureras separat.</span><span class="sxs-lookup"><span data-stu-id="d6a66-111">External services are billed separately.</span></span> <span data-ttu-id="d6a66-112">De behandlas som enskilda order i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d6a66-112">They are treated as individual orders within your Azure subscription.</span></span> <span data-ttu-id="d6a66-113">Faktureringsperioden för varje tjänst anges när du köper tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d6a66-113">The billing period for each service is set when you purchase the service.</span></span> <span data-ttu-id="d6a66-114">Ska inte förväxlas med faktureringsperiod för prenumerationen som du har handlat.</span><span class="sxs-lookup"><span data-stu-id="d6a66-114">Not to be confused with the billing period of the subscription under which you purchased it.</span></span> <span data-ttu-id="d6a66-115">Du får också separata växlar och ditt kreditkort debiteras separat.</span><span class="sxs-lookup"><span data-stu-id="d6a66-115">You also receive separate bills and your credit card is charged separately.</span></span>
- <span data-ttu-id="d6a66-116">Varje extern tjänst har en annan fakturering modell.</span><span class="sxs-lookup"><span data-stu-id="d6a66-116">Each external service has a different billing model.</span></span> <span data-ttu-id="d6a66-117">Vissa tjänster debiteras i en betalning per användning sätt medan andra använder en månatlig baserat betalning modell.</span><span class="sxs-lookup"><span data-stu-id="d6a66-117">Some services are billed in a pay-as-you-go fashion while others use a monthly based payment model.</span></span> <span data-ttu-id="d6a66-118">Du behöver ett kreditkort för externa Azure-tjänster, du kan inte köpa externa tjänster med faktura lön.</span><span class="sxs-lookup"><span data-stu-id="d6a66-118">You need a credit card for Azure external services, you can't buy external services with invoice pay.</span></span>
- <span data-ttu-id="d6a66-119">Du kan inte använda ledigt månadskrediter för externa tjänster.</span><span class="sxs-lookup"><span data-stu-id="d6a66-119">You can't use monthly free credits for external services.</span></span> <span data-ttu-id="d6a66-120">Om du använder en Azure-prenumeration som innehåller [ledigt krediter](https://azure.microsoft.com/pricing/spending-limits/), de kan inte tillämpas på externa-tjänsten växlar.</span><span class="sxs-lookup"><span data-stu-id="d6a66-120">If you are using an Azure subscription that includes [free credits](https://azure.microsoft.com/pricing/spending-limits/), they can't be applied to external service bills.</span></span> <span data-ttu-id="d6a66-121">Använda ett kreditkort till externa tjänster.</span><span class="sxs-lookup"><span data-stu-id="d6a66-121">Use a credit card to purchase external services.</span></span>


## <a name="view-external-service-spending-and-history-in-the-azure-portal"></a><span data-ttu-id="d6a66-122">Visa extern tjänsteutgifter och historik i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d6a66-122">View external service spending and history in the Azure portal</span></span>
<span data-ttu-id="d6a66-123">Du kan visa en lista över de externa tjänster som finns på varje prenumeration inom den [Azure-portalen](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="d6a66-123">You can view a list of the external services that are on each subscription within the [Azure portal](https://portal.azure.com/):</span></span> 

1. <span data-ttu-id="d6a66-124">Logga in på den [Azure-portalen](https://portal.azure.com/) som kontoadministratör.</span><span class="sxs-lookup"><span data-stu-id="d6a66-124">Sign in to the [Azure portal](https://portal.azure.com/) as the account administrator.</span></span>
2. <span data-ttu-id="d6a66-125">På navmenyn väljer **prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="d6a66-125">In the Hub menu, select **Subscriptions**.</span></span>
   
    ![Välj prenumerationer på navmenyn](./media/billing-understand-your-azure-marketplace-charges/sub-button.png) 
3. <span data-ttu-id="d6a66-127">I den **prenumerationer** bladet, Välj den prenumeration som du vill visa och välj sedan **externa tjänster**.</span><span class="sxs-lookup"><span data-stu-id="d6a66-127">In the **Subscriptions** blade, select the subscription that you want to view, and then select **External services**.</span></span>
   
    ![Välj en prenumeration i bladet fakturering](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. <span data-ttu-id="d6a66-129">Du bör se alla dina externa tjänsten order, Utgivarnamn, tjänstnivå som du har köpt, namn du gav för resursen och aktuella statusen för.</span><span class="sxs-lookup"><span data-stu-id="d6a66-129">You should see each of your external service orders, the publisher name, service tier you bought, name you gave the resource, and the current order status.</span></span> <span data-ttu-id="d6a66-130">Välj en extern tjänst om du vill se tidigare växlar.</span><span class="sxs-lookup"><span data-stu-id="d6a66-130">To see past bills, select an external service.</span></span>
   
    ![Välj en extern tjänst](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. <span data-ttu-id="d6a66-132">Härifrån kan visa du tidigare faktura belopp inklusive moms fördelningen.</span><span class="sxs-lookup"><span data-stu-id="d6a66-132">From here, you can view past bill amounts including the tax breakdown.</span></span>
   
    ![Visa faktureringshistorik externa tjänster](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="view-external-service-spending-for-enterprise-agreement-ea-customers"></a><span data-ttu-id="d6a66-134">Visa extern tjänstens utgifter för kunder med Enterprise-avtal (EA)</span><span class="sxs-lookup"><span data-stu-id="d6a66-134">View external service spending for Enterprise Agreement (EA) customers</span></span>
<span data-ttu-id="d6a66-135">EA kunder kan se externa tjänsteutgifter och hämta rapporter i EA-portalen.</span><span class="sxs-lookup"><span data-stu-id="d6a66-135">EA customers can see external service spending and download reports in the EA portal.</span></span> <span data-ttu-id="d6a66-136">Se [Azure Marketplace för EA kunder](https://ea.azure.com/helpdocs/azureMarketplace) att komma igång.</span><span class="sxs-lookup"><span data-stu-id="d6a66-136">See [Azure Marketplace for EA Customers](https://ea.azure.com/helpdocs/azureMarketplace) to get started.</span></span>

## <a name="manage-payment-methods-for-external-service-orders"></a><span data-ttu-id="d6a66-137">Hantera betalningssätt för extern tjänst order</span><span class="sxs-lookup"><span data-stu-id="d6a66-137">Manage payment methods for external service orders</span></span>
<span data-ttu-id="d6a66-138">Uppdatera betalningsmetoden för extern tjänst order från den [Kontocenter](https://account.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="d6a66-138">Update your payment methods for external service orders from the [Account Center](https://account.windowsazure.com/).</span></span>

> [!NOTE]
> <span data-ttu-id="d6a66-139">Om du har köpt din prenumeration med ett arbets- eller Skol-konto [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) att göra ändringar i din betalningsmetod.</span><span class="sxs-lookup"><span data-stu-id="d6a66-139">If you purchased your subscription with a Work or School account, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to make changes to your payment method.</span></span>
> 
> 

1. <span data-ttu-id="d6a66-140">Logga in på den [Kontocenter](https://account.windowsazure.com/) och [navigera till den **marketplace** fliken](https://account.windowsazure.com/Store)</span><span class="sxs-lookup"><span data-stu-id="d6a66-140">Sign in to the [Account Center](https://account.windowsazure.com/) and [navigate to the **marketplace** tab](https://account.windowsazure.com/Store)</span></span>
   
    ![Välj marketplace mitt konto](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. <span data-ttu-id="d6a66-142">Välj extern tjänsten som du vill hantera</span><span class="sxs-lookup"><span data-stu-id="d6a66-142">Select the external service you want to manage</span></span>
   
    ![Välj extern tjänsten som du vill hantera](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. <span data-ttu-id="d6a66-144">Klicka på **Byt betalningsmetod** till höger på sidan.</span><span class="sxs-lookup"><span data-stu-id="d6a66-144">Click **Change payment method** on the right side of the page.</span></span> <span data-ttu-id="d6a66-145">Den här länken kan du till en annan portal och hantera din betalningsmetod.</span><span class="sxs-lookup"><span data-stu-id="d6a66-145">This link brings you to a different portal to manage your payment method.</span></span>
   
    ![Ordning sammanfattning](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. <span data-ttu-id="d6a66-147">Klicka på **redigera info** och följ instruktionerna för att uppdatera din betalningsinformation.</span><span class="sxs-lookup"><span data-stu-id="d6a66-147">Click **Edit info** and follow instructions to update your payment information.</span></span>
   
    ![Välj Redigera info](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)

## <a name="cancel-an-external-service-order"></a><span data-ttu-id="d6a66-149">Avbryta en extern tjänst ordning</span><span class="sxs-lookup"><span data-stu-id="d6a66-149">Cancel an external service order</span></span>
<span data-ttu-id="d6a66-150">Om du vill avbryta din order extern tjänst kan du ta bort resursen i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d6a66-150">If you want to cancel your external service order, delete the resource in the [Azure portal](https://portal.azure.com).</span></span>

![Ta bort resurs](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a><span data-ttu-id="d6a66-152">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="d6a66-152">Need help?</span></span> <span data-ttu-id="d6a66-153">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="d6a66-153">Contact support.</span></span>
<span data-ttu-id="d6a66-154">Om du fortfarande har frågor, [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) få snabbt lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="d6a66-154">If you still have questions, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>

