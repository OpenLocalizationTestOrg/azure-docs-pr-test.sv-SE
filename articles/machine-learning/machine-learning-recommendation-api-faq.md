---
title: "Konfigurera och använda Machine Learning rekommendationer API | Microsoft Docs"
description: "Microsoft rekommendationer API som byggts med Azure Machine Learning vanliga frågor och svar"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 1ffc3c16-e040-4225-9d72-105129938dfa
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 3851589818bb8f4309bf3c65f17b115e0dcd27fa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a><span data-ttu-id="b8679-103">Konfigurera och använda Machine Learning, rekommendationer API, vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="b8679-103">Setting up and using Machine Learning Recommendations API FAQ</span></span>
<span data-ttu-id="b8679-104">**Vad är rekommendationer?**</span><span class="sxs-lookup"><span data-stu-id="b8679-104">**What is RECOMMENDATIONS?**</span></span>

> [!NOTE]
> <span data-ttu-id="b8679-105">Du bör börja använda kognitiva Rekommendationstjänsten API i stället för den här versionen.</span><span class="sxs-lookup"><span data-stu-id="b8679-105">You should start using the Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="b8679-106">Kognitiva Rekommendationstjänsten ersätter den här tjänsten och de nya funktionerna det kommer fram.</span><span class="sxs-lookup"><span data-stu-id="b8679-106">The Recommendations Cognitive Service will be replacing this service, and all the new features will be developed there.</span></span> <span data-ttu-id="b8679-107">Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.</span><span class="sxs-lookup"><span data-stu-id="b8679-107">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="b8679-108">Lär dig mer om [migrera till den nya kognitiva tjänsten](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="b8679-108">Learn more about [Migrating to the new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="b8679-109">För organisationer och företag som förlitar sig på rekommendationer mellan säljer och upp säljer produkter och tjänster till sina kunder rekommendationer i Azure Machine Learning ger en motor för självbetjäning rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="b8679-109">For organizations and businesses that rely on recommendations to cross-sell and up-sell products and services to their customers, RECOMMENDATIONS in Azure Machine Learning provides a self-service recommendations engine.</span></span> <span data-ttu-id="b8679-110">Det är en implementering av samarbetsfunktioner filtrering som använder matrisen factorization som core algoritm.</span><span class="sxs-lookup"><span data-stu-id="b8679-110">It is an implementation of collaborative filtering that uses matrix factorization as its core algorithm.</span></span> <span data-ttu-id="b8679-111">Programutvecklare kan komma åt rekommendationer med hjälp av REST API: er.</span><span class="sxs-lookup"><span data-stu-id="b8679-111">Application developers can access RECOMMENDATIONS by using REST APIs.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="b8679-112">**Vad kan jag göra med rekommendationer?**</span><span class="sxs-lookup"><span data-stu-id="b8679-112">**What can I do with RECOMMENDATIONS?**</span></span>

<span data-ttu-id="b8679-113">REKOMMENDATIONER som indata för ett objekt eller en uppsättning objekt och returnerar en lista över relevanta rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="b8679-113">RECOMMENDATIONS takes as input an item or a set of items and returns a list of relevant recommendations.</span></span> <span data-ttu-id="b8679-114">Till exempel: kund hos en återförsäljare som online klickar du på en produkt.</span><span class="sxs-lookup"><span data-stu-id="b8679-114">For example: A customer of an online retailer clicks a product.</span></span> <span data-ttu-id="b8679-115">Online återförsäljaren skickar produkten som indata till rekommendationer, hämtar en lista över produkter i RETUR och bestämmer vilken av dessa produkter som ska visas för kunden.</span><span class="sxs-lookup"><span data-stu-id="b8679-115">The online retailer sends that product as input to RECOMMENDATIONS, gets a list of products in return, and decides which of these products will be shown to the customer.</span></span> <span data-ttu-id="b8679-116">Du kanske vill använda rekommendationer att optimera din onlinebutiken eller även att meddela din inom försäljning avdelning eller anrop center.</span><span class="sxs-lookup"><span data-stu-id="b8679-116">You may want to use RECOMMENDATIONS to optimize your online store or even to inform your inside sales department or call center.</span></span>

<span data-ttu-id="b8679-117">**Finns det några begränsningar?**</span><span class="sxs-lookup"><span data-stu-id="b8679-117">**Are there any usage limitations?**</span></span>

<span data-ttu-id="b8679-118">Rekommendationerna har följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="b8679-118">Recommendations has the following usage limitations:</span></span>

* <span data-ttu-id="b8679-119">Maximala antalet modeller per prenumeration: 10</span><span class="sxs-lookup"><span data-stu-id="b8679-119">Maximum number of models per subscription: 10</span></span>
* <span data-ttu-id="b8679-120">Maximalt antal objekt som kan innehålla en katalog: 100 000</span><span class="sxs-lookup"><span data-stu-id="b8679-120">Maximum number of items that a catalog can hold: 100,000</span></span>
* <span data-ttu-id="b8679-121">Det maximala antalet punkter för användning som hålls är ~ 5,000,000.</span><span class="sxs-lookup"><span data-stu-id="b8679-121">The maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="b8679-122">Den äldsta tas bort om nya överföras eller rapporteras.</span><span class="sxs-lookup"><span data-stu-id="b8679-122">The oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="b8679-123">Maximal storlek för data som kan skickas via e-post (till exempel importera katalogdata import användningsdata) är 200 MB</span><span class="sxs-lookup"><span data-stu-id="b8679-123">Maximum size of data that can be sent in email (for example, import catalog data, import usage data) is 200 MB</span></span>
* <span data-ttu-id="b8679-124">Antal transaktioner per sekund (Transaktionsprogram) för en modell rekommendationer-version som inte är aktiv är ~ 2 Transaktionsprogram.</span><span class="sxs-lookup"><span data-stu-id="b8679-124">Number of transactions per second (TPS) for a Recommendations model build that is not active is ~2 TPS.</span></span> <span data-ttu-id="b8679-125">En rekommendationer modell-version som är aktiv kan innehålla upp till 20 Transaktionsprogram.</span><span class="sxs-lookup"><span data-stu-id="b8679-125">A Recommendations model build that is active can hold up to 20 TPS.</span></span>

## <a name="purchase-and-billing"></a><span data-ttu-id="b8679-126">Köp och fakturering</span><span class="sxs-lookup"><span data-stu-id="b8679-126">Purchase and Billing</span></span>
<span data-ttu-id="b8679-127">**Hur mycket kostar rekommendationer under starta?**</span><span class="sxs-lookup"><span data-stu-id="b8679-127">**How much does Recommendations cost during the launch period?**</span></span>

<span data-ttu-id="b8679-128">Rekommendationer är en prenumeration-baserad tjänst.</span><span class="sxs-lookup"><span data-stu-id="b8679-128">Recommendations is a subscription-based service.</span></span> <span data-ttu-id="b8679-129">Debitering baserat på mängden transaktioner per månad.</span><span class="sxs-lookup"><span data-stu-id="b8679-129">Charging is based on volume of transactions per month.</span></span> <span data-ttu-id="b8679-130">Du kan kontrollera den [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations) i Microsoft Azure Marketplace för information om priser.</span><span class="sxs-lookup"><span data-stu-id="b8679-130">You can check the [offer page](https://datamarket.azure.com/dataset/amla/recommendations) in Microsoft Azure Marketplace for pricing information.</span></span>

<span data-ttu-id="b8679-131">**Finns det några kostnader som är associerade med med rekommendationer spåra och lagra användaraktivitet för mig?**</span><span class="sxs-lookup"><span data-stu-id="b8679-131">**Are there any costs associated with having Recommendations track and store user activity for me?**</span></span>

<span data-ttu-id="b8679-132">Inte för tillfället.</span><span class="sxs-lookup"><span data-stu-id="b8679-132">Not at the moment.</span></span>

<span data-ttu-id="b8679-133">**Har rekommendationer för en kostnadsfri utvärderingsversion?**</span><span class="sxs-lookup"><span data-stu-id="b8679-133">**Does Recommendations have a free trial?**</span></span>

<span data-ttu-id="b8679-134">Det finns en kostnadsfri testversion som är begränsad till 10 000 transaktioner per månad.</span><span class="sxs-lookup"><span data-stu-id="b8679-134">There is a free trail which is restricted to 10,000 transactions per month.</span></span>

<span data-ttu-id="b8679-135">**När jag debiteras för rekommendationer?**</span><span class="sxs-lookup"><span data-stu-id="b8679-135">**When will I be billed for Recommendations?**</span></span>

<span data-ttu-id="b8679-136">En betald prenumeration är någon prenumeration för vilken det finns en månadsavgift.</span><span class="sxs-lookup"><span data-stu-id="b8679-136">A paid subscription is any subscription for which there is a monthly fee.</span></span> <span data-ttu-id="b8679-137">Du debiteras omedelbart för den första månaden när du köper en betald prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b8679-137">When you purchase a paid subscription, you are immediately charged for the first month's use.</span></span> <span data-ttu-id="b8679-138">Du debiteras belopp som hör till erbjudandet på prenumerationssidan (plus eventuella skatter).</span><span class="sxs-lookup"><span data-stu-id="b8679-138">You are charged the amount that is associated with the offer on the subscription page (plus applicable taxes).</span></span> <span data-ttu-id="b8679-139">Den här månadskostnaden görs varje månad kalender samma dag som köpet ursprungliga tills du avbryter prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="b8679-139">This monthly charge is made each month on the same calendar date as your original purchase until you cancel the subscription.</span></span> 

<span data-ttu-id="b8679-140">**Hur uppgraderar till en högre nivå-tjänst?**</span><span class="sxs-lookup"><span data-stu-id="b8679-140">**How do I upgrade to a higher tier service?**</span></span>

<span data-ttu-id="b8679-141">Du kan köpa eller uppdatera din prenumeration från den [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations) sida på Microsoft Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b8679-141">You can buy or update your subscription from the [offer page](https://datamarket.azure.com/dataset/amla/recommendations) page on Microsoft Azure Marketplace.</span></span>

<span data-ttu-id="b8679-142">När du uppgraderar en prenumeration:</span><span class="sxs-lookup"><span data-stu-id="b8679-142">When you upgrade a subscription:</span></span>

* <span data-ttu-id="b8679-143">Transaktioner som kvar i din gamla prenumeration har inte lagts till din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b8679-143">Transactions that are remaining on your old subscription are not added to your new subscription.</span></span> 
* <span data-ttu-id="b8679-144">Du betalar fullständig pris för den nya prenumerationen, även om du har oanvända transaktioner på din gamla prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b8679-144">You pay full price for the new subscription, even though you have unused transactions on your old subscription.</span></span>

<span data-ttu-id="b8679-145">Processen att uppgradera en prenumeration:</span><span class="sxs-lookup"><span data-stu-id="b8679-145">Process to upgrade a subscription:</span></span>

* <span data-ttu-id="b8679-146">Nevigate till den [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations).</span><span class="sxs-lookup"><span data-stu-id="b8679-146">Nevigate to the [offer page](https://datamarket.azure.com/dataset/amla/recommendations).</span></span>
* <span data-ttu-id="b8679-147">Logga in om du inte redan är inloggad på Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b8679-147">Sign in to the Marketplace if you aren't already Signed in.</span></span>
* <span data-ttu-id="b8679-148">I den högra rutan visas alla tillgängliga planer.</span><span class="sxs-lookup"><span data-stu-id="b8679-148">In the right pane, all the available plans are listed.</span></span> <span data-ttu-id="b8679-149">Klicka på knappen för den plan som du vill uppgradera till.</span><span class="sxs-lookup"><span data-stu-id="b8679-149">Click the radio button for the plan you want to upgrade to.</span></span>
* <span data-ttu-id="b8679-150">Om du vill uppgradera klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b8679-150">If you want to upgrade, click **OK**.</span></span> <span data-ttu-id="b8679-151">Om du inte vill uppgradera klickar du på **Avbryt**.</span><span class="sxs-lookup"><span data-stu-id="b8679-151">If you do not want to upgrade, click **Cancel**.</span></span>

<span data-ttu-id="b8679-152">**Viktiga** dialogrutan Läs noggrant innan du uppgraderar eftersom det finns fakturerings- och konsekvenser.</span><span class="sxs-lookup"><span data-stu-id="b8679-152">**Important** Carefully read the dialog box before you upgrade because there are billing and use implications.</span></span>

<span data-ttu-id="b8679-153">**När avslutas min prenumeration till rekommendationer?**</span><span class="sxs-lookup"><span data-stu-id="b8679-153">**When will my subscription to Recommendations end?**</span></span>

<span data-ttu-id="b8679-154">Din prenumeration avslutas när du avbryter den.</span><span class="sxs-lookup"><span data-stu-id="b8679-154">Your subscription will end when you cancel it.</span></span> <span data-ttu-id="b8679-155">Om du vill avbryta dina prenumerationer finns i följande anvisningar.</span><span class="sxs-lookup"><span data-stu-id="b8679-155">If you would like to cancel your subscriptions, see the following instructions.</span></span>

<span data-ttu-id="b8679-156">**Hur avbryter min rekommendationer prenumeration?**</span><span class="sxs-lookup"><span data-stu-id="b8679-156">**How do I cancel my Recommendations subscription?**</span></span>

<span data-ttu-id="b8679-157">Använd följande steg om du vill avbryta din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b8679-157">To cancel your subscription, use the following steps.</span></span> <span data-ttu-id="b8679-158">Om din aktuella prenumeration är en betald prenumeration, fortsätter prenumerationen gäller fram till slutet av den aktuella faktureringsperioden.</span><span class="sxs-lookup"><span data-stu-id="b8679-158">If your current subscription is a paid subscription, your subscription continues in effect until the end of the current billing period.</span></span> <span data-ttu-id="b8679-159">Om du behöver avbryta gälla omedelbart kan du kontakta oss på [Microsoft-supporten](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span><span class="sxs-lookup"><span data-stu-id="b8679-159">If you need the cancellation to be effective immediately, contact us at [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span></span>

<span data-ttu-id="b8679-160">**Obs** bidrag ges om du avbryter före utgången av en faktureringsperiod eller för transaktioner som inte används i en faktureringsperioden.</span><span class="sxs-lookup"><span data-stu-id="b8679-160">**Note** No refund is given if you cancel before the end of a billing period or for unused transactions in a billing period.</span></span>

* <span data-ttu-id="b8679-161">Navigera till den [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations).</span><span class="sxs-lookup"><span data-stu-id="b8679-161">Navigate to the [offer page](https://datamarket.azure.com/dataset/amla/recommendations).</span></span>
* <span data-ttu-id="b8679-162">Logga in om du inte redan är inloggad på Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b8679-162">Sign in to the Marketplace if you aren't already Signed in.</span></span>
* <span data-ttu-id="b8679-163">Klicka på **Avbryt** till höger om dataset-namnet och status.</span><span class="sxs-lookup"><span data-stu-id="b8679-163">Click **Cancel** to the right of the dataset name and status.</span></span> <span data-ttu-id="b8679-164">Du kan använda den här prenumerationen till slutet av den aktuella faktureringsperioden eller transaktionen gränsen har nåtts (beroende på vilket som inträffar först).</span><span class="sxs-lookup"><span data-stu-id="b8679-164">You can use this subscription until the end of the current billing period or your transaction limit is reached (whichever occurs first).</span></span>

<span data-ttu-id="b8679-165">Om du vill avbryta prenumerationen omedelbart så du kan köpa en prenumeration filen en biljett på [Microsoft-supporten](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span><span class="sxs-lookup"><span data-stu-id="b8679-165">If you would like to cancel your subscription immediately so you can purchase a new subscription, file a ticket at [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span></span>

## <a name="getting-started-with-recommendations"></a><span data-ttu-id="b8679-166">Komma igång med rekommendationer</span><span class="sxs-lookup"><span data-stu-id="b8679-166">Getting started with Recommendations</span></span>
<span data-ttu-id="b8679-167">**Är rekommendationer för mig?**</span><span class="sxs-lookup"><span data-stu-id="b8679-167">**Is Recommendations for me?**</span></span> 

<span data-ttu-id="b8679-168">Rekommendationerna i Machine Learning är för organisationer och företag som förlitar sig på rekommendationer mellan säljer och upp säljer produkter eller tjänster till sina kunder.</span><span class="sxs-lookup"><span data-stu-id="b8679-168">Recommendations in Machine Learning is for organizations and businesses that rely on recommendations to cross-sell and up-sell products or services to their customers.</span></span> <span data-ttu-id="b8679-169">Om du har en kund-riktade webbplats, en säljstyrkan, en inre säljstyrkan eller Callcenter, och om du tillhandahåller en katalog över flera dussin produkter eller tjänster slutresultatet kan dra nytta av rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="b8679-169">If you have a customer-facing website, a sales force, an inside sales force, or a call center, and if you offer a catalog of more than a few dozen products or services, your bottom line may benefit from using Recommendations.</span></span> 

<span data-ttu-id="b8679-170">Experimentera med rekommendationer är avsedda att vara ganska enkel.</span><span class="sxs-lookup"><span data-stu-id="b8679-170">Experimenting with Recommendations is designed to be fairly simple.</span></span> <span data-ttu-id="b8679-171">Den aktuella versionen av API-baserade kräver grundläggande kunskaper i programmering.</span><span class="sxs-lookup"><span data-stu-id="b8679-171">The current API-based version requires basic programming skills.</span></span> <span data-ttu-id="b8679-172">Kontakta leverantören som utvecklat din webbplats om du behöver hjälp.</span><span class="sxs-lookup"><span data-stu-id="b8679-172">If you need assistance, contact the vendor who developed your website.</span></span> <span data-ttu-id="b8679-173">Om du har en intern IT-avdelningen eller intern systemutvecklare ska de kunna få rekommendationer som passar dig.</span><span class="sxs-lookup"><span data-stu-id="b8679-173">If you have an internal IT department or an in-house developer, they should be able to get Recommendations to work for you.</span></span> 

<span data-ttu-id="b8679-174">**Vilka är kraven för att ställa in rekommendationer?**</span><span class="sxs-lookup"><span data-stu-id="b8679-174">**What are the prerequisites for setting up Recommendations?**</span></span>

<span data-ttu-id="b8679-175">Rekommendationer kräver att du har en logg över användarens val i relation till katalogen.</span><span class="sxs-lookup"><span data-stu-id="b8679-175">Recommendations requires that you have a log of user choices as it relates to your catalog.</span></span> <span data-ttu-id="b8679-176">Om du inte har sådan loggen och du har en kund-webbplatser, rekommendationer kan samla in användaraktivitet för dig.</span><span class="sxs-lookup"><span data-stu-id="b8679-176">If you don't have such a log and you do have a customer facing website, Recommendations can collect user activity for you.</span></span> 

<span data-ttu-id="b8679-177">Rekommendationer kräver också en katalog med dina produkter eller tjänster.</span><span class="sxs-lookup"><span data-stu-id="b8679-177">Recommendations also requires a catalog of your products or services.</span></span> <span data-ttu-id="b8679-178">Om du inte har katalogen kan rekommendationer använda faktiska användning kunddata och destillera en katalog.</span><span class="sxs-lookup"><span data-stu-id="b8679-178">If you don't have the catalog, Recommendations can use the actual customer usage data and distill a catalog.</span></span> <span data-ttu-id="b8679-179">En underförstådda katalog kommer inte att innehålla objekt som inte rapporterades som en del av användartransaktioner.</span><span class="sxs-lookup"><span data-stu-id="b8679-179">An implied catalog will not include items that were not reported as part of user transactions.</span></span>

<span data-ttu-id="b8679-180">**Hur ställer jag in rekommendationer för första gången?**</span><span class="sxs-lookup"><span data-stu-id="b8679-180">**How do I set up Recommendations for the first time?**</span></span>

<span data-ttu-id="b8679-181">Efter [prenumerationsapp](https://datamarket.azure.com/dataset/amla/recommendations) rekommendationer, bör du använda API-dokumentationen i den [Azure Machine Learning rekommendationer - snabbstartsguiden](machine-learning-recommendation-api-quick-start-guide.md) att konfigurera tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b8679-181">After [subscribing](https://datamarket.azure.com/dataset/amla/recommendations) to Recommendations, you should use the API documentation in the [Azure Machine Learning Recommendations - Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md) to set up the service.</span></span>

<span data-ttu-id="b8679-182">**Var hittar jag API-dokumentationen?**</span><span class="sxs-lookup"><span data-stu-id="b8679-182">**Where can I find API documentation?**</span></span> 

<span data-ttu-id="b8679-183">API-dokumentationen är [rekommendationer för Azure Machine Learning-snabbstartsguiden](machine-learning-recommendation-api-quick-start-guide.md).</span><span class="sxs-lookup"><span data-stu-id="b8679-183">The API documentation is [Azure Machine Learning Recommendations -Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md).</span></span>

<span data-ttu-id="b8679-184">**Vilka alternativ finns att överföra katalog-och användningsdata till rekommendationer?**</span><span class="sxs-lookup"><span data-stu-id="b8679-184">**What options do I have to upload catalog and usage data to Recommendations?**</span></span>

<span data-ttu-id="b8679-185">Har du två alternativ för uppladdning av data catalog- och användningsdata: du kan exportera data från ditt CRM-system eller andra loggar och överföra den till rekommendationer eller du kan lägga till taggar till din webbplats som ska övervaka användaraktiviteter.</span><span class="sxs-lookup"><span data-stu-id="b8679-185">You have two options for uploading your catalog and usage data: You can export the data from your CRM system or other logs and upload it to Recommendations, or you can add tags to your website that will track user activities.</span></span> <span data-ttu-id="b8679-186">Om du använder den andra metoden lagras data i Azure.</span><span class="sxs-lookup"><span data-stu-id="b8679-186">If you use the latter method, the data will be stored in Azure.</span></span>

## <a name="maintenance-and-support"></a><span data-ttu-id="b8679-187">Underhåll och support</span><span class="sxs-lookup"><span data-stu-id="b8679-187">Maintenance and support</span></span>
<span data-ttu-id="b8679-188">**Hur stor kan min datauppsättning?**</span><span class="sxs-lookup"><span data-stu-id="b8679-188">**How large can my data set be?**</span></span>

<span data-ttu-id="b8679-189">Varje datamängd kan innehålla upp till 100 000 katalogobjekt och upp till 2 048 MB användningsdata.</span><span class="sxs-lookup"><span data-stu-id="b8679-189">Each data set can contain up to 100,000 catalog items and up to 2048 MB of usage data.</span></span>
<span data-ttu-id="b8679-190">En prenumeration kan dessutom innehålla upp till 10 datauppsättningar (modeller).</span><span class="sxs-lookup"><span data-stu-id="b8679-190">In addition, a subscription can contain up to 10 data sets (models).</span></span>

<span data-ttu-id="b8679-191">**Var hittar jag teknisk support för rekommendationer**</span><span class="sxs-lookup"><span data-stu-id="b8679-191">**Where can I get technical support for Recommendations?**</span></span>

<span data-ttu-id="b8679-192">Teknisk support är tillgänglig på den [stöd för Microsoft Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) plats.</span><span class="sxs-lookup"><span data-stu-id="b8679-192">Technical support is available on the [Microsoft Azure Support](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) site.</span></span>

<span data-ttu-id="b8679-193">**Var hittar jag användningsvillkoren**</span><span class="sxs-lookup"><span data-stu-id="b8679-193">**Where can I find the terms of use?**</span></span>

<span data-ttu-id="b8679-194">[Microsoft Azure Machine Learning rekommendationer API användarvillkoren](https://datamarket.azure.com/dataset/amla/recommendations#terms).</span><span class="sxs-lookup"><span data-stu-id="b8679-194">[Microsoft Azure Machine Learning Recommendations API Terms of Service](https://datamarket.azure.com/dataset/amla/recommendations#terms).</span></span>

