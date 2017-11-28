---
title: aaaUnderstand Azure externa avgifterna | Microsoft Docs
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
ms.openlocfilehash: 1d2cb28319e2ab4eff753177220993cbf94c96ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-your-azure-billing-for-external-service-charges"></a><span data-ttu-id="8560b-103">Förstå Azure faktureringen för externa serviceavgifter</span><span class="sxs-lookup"><span data-stu-id="8560b-103">Understand your Azure billing for external service charges</span></span>
<span data-ttu-id="8560b-104">Externa tjänster används toobe kallas Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="8560b-104">External services used toobe called Azure Marketplace.</span></span> <span data-ttu-id="8560b-105">I allmänhet de är tjänster som publicerats av tillgängliga för tredje part för Azure men är helt integrerade i Azure.</span><span class="sxs-lookup"><span data-stu-id="8560b-105">Generally, they're services published by third-parties available for Azure but are integrated completely within Azure.</span></span> <span data-ttu-id="8560b-106">Till exempel är ClearDB och SendGrid externa tjänster som du kan köpa i Azure, men inte har publicerats av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8560b-106">For example, ClearDB and SendGrid are external services that you can purchase in Azure, but are not published by Microsoft.</span></span>

<span data-ttu-id="8560b-107">När du etablerar en ny extern tjänst eller en resurs, visas en varning:</span><span class="sxs-lookup"><span data-stu-id="8560b-107">When you provision a new external service or resource, a warning is shown:</span></span>

![Marketplace köpa varning](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

> [!NOTE]
> <span data-ttu-id="8560b-109">Externa tjänster publiceras av företag som inte är Microsoft, men ibland också Microsoft-produkter kategoriseras som externa tjänster.</span><span class="sxs-lookup"><span data-stu-id="8560b-109">External services are published by companies that are not Microsoft, but sometimes Microsoft products are also categorized as external services.</span></span>
> 
> 

## <a name="how-external-services-are-billed"></a><span data-ttu-id="8560b-110">Hur faktureras externa tjänster</span><span class="sxs-lookup"><span data-stu-id="8560b-110">How external services are billed</span></span>
- <span data-ttu-id="8560b-111">Externa tjänster faktureras separat.</span><span class="sxs-lookup"><span data-stu-id="8560b-111">External services are billed separately.</span></span> <span data-ttu-id="8560b-112">De behandlas som enskilda order i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8560b-112">They are treated as individual orders within your Azure subscription.</span></span> <span data-ttu-id="8560b-113">hello faktureringsperioden för varje tjänst anges när du köper hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8560b-113">hello billing period for each service is set when you purchase hello service.</span></span> <span data-ttu-id="8560b-114">Inte toobe förväxlas med hello faktureringsperiod hello prenumeration som du har handlat.</span><span class="sxs-lookup"><span data-stu-id="8560b-114">Not toobe confused with hello billing period of hello subscription under which you purchased it.</span></span> <span data-ttu-id="8560b-115">Du får också separata växlar och ditt kreditkort debiteras separat.</span><span class="sxs-lookup"><span data-stu-id="8560b-115">You also receive separate bills and your credit card is charged separately.</span></span>
- <span data-ttu-id="8560b-116">Varje extern tjänst har en annan fakturering modell.</span><span class="sxs-lookup"><span data-stu-id="8560b-116">Each external service has a different billing model.</span></span> <span data-ttu-id="8560b-117">Vissa tjänster debiteras i en betalning per användning sätt medan andra använder en månatlig baserat betalning modell.</span><span class="sxs-lookup"><span data-stu-id="8560b-117">Some services are billed in a pay-as-you-go fashion while others use a monthly based payment model.</span></span> <span data-ttu-id="8560b-118">Du behöver ett kreditkort för externa Azure-tjänster, du kan inte köpa externa tjänster med faktura lön.</span><span class="sxs-lookup"><span data-stu-id="8560b-118">You need a credit card for Azure external services, you can't buy external services with invoice pay.</span></span>
- <span data-ttu-id="8560b-119">Du kan inte använda ledigt månadskrediter för externa tjänster.</span><span class="sxs-lookup"><span data-stu-id="8560b-119">You can't use monthly free credits for external services.</span></span> <span data-ttu-id="8560b-120">Om du använder en Azure-prenumeration som innehåller [ledigt krediter](https://azure.microsoft.com/pricing/spending-limits/), de kan inte vara tillämpade tooexternal tjänsten växlar.</span><span class="sxs-lookup"><span data-stu-id="8560b-120">If you are using an Azure subscription that includes [free credits](https://azure.microsoft.com/pricing/spending-limits/), they can't be applied tooexternal service bills.</span></span> <span data-ttu-id="8560b-121">Använda ett kreditkort toopurchase externa tjänster.</span><span class="sxs-lookup"><span data-stu-id="8560b-121">Use a credit card toopurchase external services.</span></span>


## <a name="view-external-service-spending-and-history-in-hello-azure-portal"></a><span data-ttu-id="8560b-122">Visa extern tjänsteutgifter och historik i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8560b-122">View external service spending and history in hello Azure portal</span></span>
<span data-ttu-id="8560b-123">Du kan visa en lista över hello externa tjänster som finns på varje prenumeration inom hello [Azure-portalen](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="8560b-123">You can view a list of hello external services that are on each subscription within hello [Azure portal](https://portal.azure.com/):</span></span> 

1. <span data-ttu-id="8560b-124">Logga in toohello [Azure-portalen](https://portal.azure.com/) som hello kontoadministratör.</span><span class="sxs-lookup"><span data-stu-id="8560b-124">Sign in toohello [Azure portal](https://portal.azure.com/) as hello account administrator.</span></span>
2. <span data-ttu-id="8560b-125">I hello navmenyn väljer **prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="8560b-125">In hello Hub menu, select **Subscriptions**.</span></span>
   
    ![Välj prenumerationer i hello hubbmenyn](./media/billing-understand-your-azure-marketplace-charges/sub-button.png) 
3. <span data-ttu-id="8560b-127">I hello **prenumerationer** bladet, Välj hello prenumeration som du vill tooview och välj sedan **externa tjänster**.</span><span class="sxs-lookup"><span data-stu-id="8560b-127">In hello **Subscriptions** blade, select hello subscription that you want tooview, and then select **External services**.</span></span>
   
    ![Välj en prenumeration i hello fakturering bladet](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. <span data-ttu-id="8560b-129">Du bör se alla dina externa tjänsten order, hello utgivarens namn, tjänstnivå som du har köpt, namn du gav hello resurs och aktuell status för hello.</span><span class="sxs-lookup"><span data-stu-id="8560b-129">You should see each of your external service orders, hello publisher name, service tier you bought, name you gave hello resource, and hello current order status.</span></span> <span data-ttu-id="8560b-130">toosee tidigare räkningar, väljer du en extern tjänst.</span><span class="sxs-lookup"><span data-stu-id="8560b-130">toosee past bills, select an external service.</span></span>
   
    ![Välj en extern tjänst](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. <span data-ttu-id="8560b-132">Härifrån kan visa du tidigare faktura belopp som inkluderar hello skatt uppdelning.</span><span class="sxs-lookup"><span data-stu-id="8560b-132">From here, you can view past bill amounts including hello tax breakdown.</span></span>
   
    ![Visa faktureringshistorik externa tjänster](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="view-external-service-spending-for-enterprise-agreement-ea-customers"></a><span data-ttu-id="8560b-134">Visa extern tjänstens utgifter för kunder med Enterprise-avtal (EA)</span><span class="sxs-lookup"><span data-stu-id="8560b-134">View external service spending for Enterprise Agreement (EA) customers</span></span>
<span data-ttu-id="8560b-135">EA kunder kan se externa tjänsteutgifter och hämta rapporter i hello EA-portalen.</span><span class="sxs-lookup"><span data-stu-id="8560b-135">EA customers can see external service spending and download reports in hello EA portal.</span></span> <span data-ttu-id="8560b-136">Se [Azure Marketplace för EA kunder](https://ea.azure.com/helpdocs/azureMarketplace) tooget igång.</span><span class="sxs-lookup"><span data-stu-id="8560b-136">See [Azure Marketplace for EA Customers](https://ea.azure.com/helpdocs/azureMarketplace) tooget started.</span></span>

## <a name="manage-payment-methods-for-external-service-orders"></a><span data-ttu-id="8560b-137">Hantera betalningssätt för extern tjänst order</span><span class="sxs-lookup"><span data-stu-id="8560b-137">Manage payment methods for external service orders</span></span>
<span data-ttu-id="8560b-138">Uppdatera betalningsmetoden för extern tjänst order från hello [Kontocenter](https://account.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="8560b-138">Update your payment methods for external service orders from hello [Account Center](https://account.windowsazure.com/).</span></span>

> [!NOTE]
> <span data-ttu-id="8560b-139">Om du har köpt din prenumeration med ett arbets- eller Skol-konto [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) toomake ändras tooyour betalningsmetod.</span><span class="sxs-lookup"><span data-stu-id="8560b-139">If you purchased your subscription with a Work or School account, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) toomake changes tooyour payment method.</span></span>
> 
> 

1. <span data-ttu-id="8560b-140">Logga in toohello [Kontocenter](https://account.windowsazure.com/) och [navigera toohello **marketplace** fliken](https://account.windowsazure.com/Store)</span><span class="sxs-lookup"><span data-stu-id="8560b-140">Sign in toohello [Account Center](https://account.windowsazure.com/) and [navigate toohello **marketplace** tab](https://account.windowsazure.com/Store)</span></span>
   
    ![Välj marketplace i hello kontocenter](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. <span data-ttu-id="8560b-142">Välj hello externa tjänster du vill toomanage</span><span class="sxs-lookup"><span data-stu-id="8560b-142">Select hello external service you want toomanage</span></span>
   
    ![Välj hello externa tjänster du vill toomanage](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. <span data-ttu-id="8560b-144">Klicka på **Byt betalningsmetod** hello höger på sidan hello.</span><span class="sxs-lookup"><span data-stu-id="8560b-144">Click **Change payment method** on hello right side of hello page.</span></span> <span data-ttu-id="8560b-145">Den här länken öppnar du tooa olika portal toomanage betalningsmetoden.</span><span class="sxs-lookup"><span data-stu-id="8560b-145">This link brings you tooa different portal toomanage your payment method.</span></span>
   
    ![Ordning sammanfattning](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. <span data-ttu-id="8560b-147">Klicka på **redigera info** och följ instruktionerna tooupdate din betalningsinformation.</span><span class="sxs-lookup"><span data-stu-id="8560b-147">Click **Edit info** and follow instructions tooupdate your payment information.</span></span>
   
    ![Välj Redigera info](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)

## <a name="cancel-an-external-service-order"></a><span data-ttu-id="8560b-149">Avbryta en extern tjänst ordning</span><span class="sxs-lookup"><span data-stu-id="8560b-149">Cancel an external service order</span></span>
<span data-ttu-id="8560b-150">Om du vill toocancel beställningen extern tjänst, ta bort hello resurs i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8560b-150">If you want toocancel your external service order, delete hello resource in hello [Azure portal](https://portal.azure.com).</span></span>

![Ta bort resurs](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a><span data-ttu-id="8560b-152">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="8560b-152">Need help?</span></span> <span data-ttu-id="8560b-153">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="8560b-153">Contact support.</span></span>
<span data-ttu-id="8560b-154">Om du fortfarande har frågor, [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.</span><span class="sxs-lookup"><span data-stu-id="8560b-154">If you still have questions, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>

