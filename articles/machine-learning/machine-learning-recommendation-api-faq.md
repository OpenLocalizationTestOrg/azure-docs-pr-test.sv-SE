---
title: "aaaSet in och använda hello Machine Learning rekommendationer API | Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 980bf1a36f3291275d9ef0fee9b4446f7e0cbecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a><span data-ttu-id="87bf1-103">Konfigurera och använda Machine Learning, rekommendationer API, vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="87bf1-103">Setting up and using Machine Learning Recommendations API FAQ</span></span>
<span data-ttu-id="87bf1-104">**Vad är rekommendationer?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-104">**What is RECOMMENDATIONS?**</span></span>

> [!NOTE]
> <span data-ttu-id="87bf1-105">Du bör börja använda hello kognitiva rekommendationer API-tjänsten i stället för den här versionen.</span><span class="sxs-lookup"><span data-stu-id="87bf1-105">You should start using hello Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="87bf1-106">hello kognitiva Rekommendationstjänsten ersätter den här tjänsten och alla nya funktioner för hello det kommer fram.</span><span class="sxs-lookup"><span data-stu-id="87bf1-106">hello Recommendations Cognitive Service will be replacing this service, and all hello new features will be developed there.</span></span> <span data-ttu-id="87bf1-107">Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.</span><span class="sxs-lookup"><span data-stu-id="87bf1-107">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="87bf1-108">Lär dig mer om [migrera toohello ny kognitiva tjänst](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="87bf1-108">Learn more about [Migrating toohello new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="87bf1-109">REKOMMENDATIONERNA i Azure Machine Learning ger en motor för självbetjäning rekommendationer för organisationer och företag som förlitar sig på rekommendationer toocross-säljer och upp säljer produkter och tjänster tootheir kunder.</span><span class="sxs-lookup"><span data-stu-id="87bf1-109">For organizations and businesses that rely on recommendations toocross-sell and up-sell products and services tootheir customers, RECOMMENDATIONS in Azure Machine Learning provides a self-service recommendations engine.</span></span> <span data-ttu-id="87bf1-110">Det är en implementering av samarbetsfunktioner filtrering som använder matrisen factorization som core algoritm.</span><span class="sxs-lookup"><span data-stu-id="87bf1-110">It is an implementation of collaborative filtering that uses matrix factorization as its core algorithm.</span></span> <span data-ttu-id="87bf1-111">Programutvecklare kan komma åt rekommendationer med hjälp av REST API: er.</span><span class="sxs-lookup"><span data-stu-id="87bf1-111">Application developers can access RECOMMENDATIONS by using REST APIs.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="87bf1-112">**Vad kan jag göra med rekommendationer?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-112">**What can I do with RECOMMENDATIONS?**</span></span>

<span data-ttu-id="87bf1-113">REKOMMENDATIONER som indata för ett objekt eller en uppsättning objekt och returnerar en lista över relevanta rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="87bf1-113">RECOMMENDATIONS takes as input an item or a set of items and returns a list of relevant recommendations.</span></span> <span data-ttu-id="87bf1-114">Till exempel: kund hos en återförsäljare som online klickar du på en produkt.</span><span class="sxs-lookup"><span data-stu-id="87bf1-114">For example: A customer of an online retailer clicks a product.</span></span> <span data-ttu-id="87bf1-115">hello online återförsäljare skickar produkten som inkommande tooRECOMMENDATIONS hämtar en lista över produkter i RETUR och bestämmer vilken av dessa produkter visas toohello kunden.</span><span class="sxs-lookup"><span data-stu-id="87bf1-115">hello online retailer sends that product as input tooRECOMMENDATIONS, gets a list of products in return, and decides which of these products will be shown toohello customer.</span></span> <span data-ttu-id="87bf1-116">Du kanske vill toouse rekommendationer toooptimize onlinebutiken eller ens tooinform din inom försäljning avdelning eller anrop center.</span><span class="sxs-lookup"><span data-stu-id="87bf1-116">You may want toouse RECOMMENDATIONS toooptimize your online store or even tooinform your inside sales department or call center.</span></span>

<span data-ttu-id="87bf1-117">**Finns det några begränsningar?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-117">**Are there any usage limitations?**</span></span>

<span data-ttu-id="87bf1-118">Rekommendationerna har hello följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="87bf1-118">Recommendations has hello following usage limitations:</span></span>

* <span data-ttu-id="87bf1-119">Maximala antalet modeller per prenumeration: 10</span><span class="sxs-lookup"><span data-stu-id="87bf1-119">Maximum number of models per subscription: 10</span></span>
* <span data-ttu-id="87bf1-120">Maximalt antal objekt som kan innehålla en katalog: 100 000</span><span class="sxs-lookup"><span data-stu-id="87bf1-120">Maximum number of items that a catalog can hold: 100,000</span></span>
* <span data-ttu-id="87bf1-121">hello maxantalet återställningspunkter för användning som hålls är ~ 5,000,000.</span><span class="sxs-lookup"><span data-stu-id="87bf1-121">hello maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="87bf1-122">hello äldsta tas bort om nya överföras eller rapporteras.</span><span class="sxs-lookup"><span data-stu-id="87bf1-122">hello oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="87bf1-123">Maximal storlek för data som kan skickas via e-post (till exempel importera katalogdata import användningsdata) är 200 MB</span><span class="sxs-lookup"><span data-stu-id="87bf1-123">Maximum size of data that can be sent in email (for example, import catalog data, import usage data) is 200 MB</span></span>
* <span data-ttu-id="87bf1-124">Antal transaktioner per sekund (Transaktionsprogram) för en modell rekommendationer-version som inte är aktiv är ~ 2 Transaktionsprogram.</span><span class="sxs-lookup"><span data-stu-id="87bf1-124">Number of transactions per second (TPS) for a Recommendations model build that is not active is ~2 TPS.</span></span> <span data-ttu-id="87bf1-125">En rekommendationer modell-version som är aktiv kan innehålla upp too20 Transaktionsprogram.</span><span class="sxs-lookup"><span data-stu-id="87bf1-125">A Recommendations model build that is active can hold up too20 TPS.</span></span>

## <a name="purchase-and-billing"></a><span data-ttu-id="87bf1-126">Köp och fakturering</span><span class="sxs-lookup"><span data-stu-id="87bf1-126">Purchase and Billing</span></span>
<span data-ttu-id="87bf1-127">**Hur mycket kostar rekommendationer under hello starta?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-127">**How much does Recommendations cost during hello launch period?**</span></span>

<span data-ttu-id="87bf1-128">Rekommendationer är en prenumeration-baserad tjänst.</span><span class="sxs-lookup"><span data-stu-id="87bf1-128">Recommendations is a subscription-based service.</span></span> <span data-ttu-id="87bf1-129">Debitering baserat på mängden transaktioner per månad.</span><span class="sxs-lookup"><span data-stu-id="87bf1-129">Charging is based on volume of transactions per month.</span></span> <span data-ttu-id="87bf1-130">Du kan kontrollera hello [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations) i Microsoft Azure Marketplace för information om priser.</span><span class="sxs-lookup"><span data-stu-id="87bf1-130">You can check hello [offer page](https://datamarket.azure.com/dataset/amla/recommendations) in Microsoft Azure Marketplace for pricing information.</span></span>

<span data-ttu-id="87bf1-131">**Finns det några kostnader som är associerade med med rekommendationer spåra och lagra användaraktivitet för mig?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-131">**Are there any costs associated with having Recommendations track and store user activity for me?**</span></span>

<span data-ttu-id="87bf1-132">Inte tillfället hello.</span><span class="sxs-lookup"><span data-stu-id="87bf1-132">Not at hello moment.</span></span>

<span data-ttu-id="87bf1-133">**Har rekommendationer för en kostnadsfri utvärderingsversion?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-133">**Does Recommendations have a free trial?**</span></span>

<span data-ttu-id="87bf1-134">Det finns en kostnadsfri testversion som är begränsad too10, 000 transaktioner per månad.</span><span class="sxs-lookup"><span data-stu-id="87bf1-134">There is a free trail which is restricted too10,000 transactions per month.</span></span>

<span data-ttu-id="87bf1-135">**När jag debiteras för rekommendationer?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-135">**When will I be billed for Recommendations?**</span></span>

<span data-ttu-id="87bf1-136">En betald prenumeration är någon prenumeration för vilken det finns en månadsavgift.</span><span class="sxs-lookup"><span data-stu-id="87bf1-136">A paid subscription is any subscription for which there is a monthly fee.</span></span> <span data-ttu-id="87bf1-137">När du köper en betald prenumeration omedelbart debiteras du för hello först månadens användning.</span><span class="sxs-lookup"><span data-stu-id="87bf1-137">When you purchase a paid subscription, you are immediately charged for hello first month's use.</span></span> <span data-ttu-id="87bf1-138">Du debiteras hello belopp som är associerad med hello erbjudandet på hello prenumerationssidan (plus eventuella skatter).</span><span class="sxs-lookup"><span data-stu-id="87bf1-138">You are charged hello amount that is associated with hello offer on hello subscription page (plus applicable taxes).</span></span> <span data-ttu-id="87bf1-139">Den här månadskostnaden görs varje månad hello samma kalender datum som köpet ursprungliga tills du avbryter hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="87bf1-139">This monthly charge is made each month on hello same calendar date as your original purchase until you cancel hello subscription.</span></span> 

<span data-ttu-id="87bf1-140">**Hur uppgraderar jag tooa tjänst på högre nivå**</span><span class="sxs-lookup"><span data-stu-id="87bf1-140">**How do I upgrade tooa higher tier service?**</span></span>

<span data-ttu-id="87bf1-141">Du kan köpa eller uppdatera din prenumeration från hello [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations) sida på Microsoft Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="87bf1-141">You can buy or update your subscription from hello [offer page](https://datamarket.azure.com/dataset/amla/recommendations) page on Microsoft Azure Marketplace.</span></span>

<span data-ttu-id="87bf1-142">När du uppgraderar en prenumeration:</span><span class="sxs-lookup"><span data-stu-id="87bf1-142">When you upgrade a subscription:</span></span>

* <span data-ttu-id="87bf1-143">Transaktioner som kvar på prenumerationen gamla läggs inte tooyour ny prenumeration.</span><span class="sxs-lookup"><span data-stu-id="87bf1-143">Transactions that are remaining on your old subscription are not added tooyour new subscription.</span></span> 
* <span data-ttu-id="87bf1-144">Du betalar hela priset för hello ny prenumeration, även om du har oanvända transaktioner på din gamla prenumeration.</span><span class="sxs-lookup"><span data-stu-id="87bf1-144">You pay full price for hello new subscription, even though you have unused transactions on your old subscription.</span></span>

<span data-ttu-id="87bf1-145">Processen tooupgrade en prenumeration:</span><span class="sxs-lookup"><span data-stu-id="87bf1-145">Process tooupgrade a subscription:</span></span>

* <span data-ttu-id="87bf1-146">Nevigate toohello [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations).</span><span class="sxs-lookup"><span data-stu-id="87bf1-146">Nevigate toohello [offer page](https://datamarket.azure.com/dataset/amla/recommendations).</span></span>
* <span data-ttu-id="87bf1-147">Logga in toohello Marketplace om du inte redan är inloggad.</span><span class="sxs-lookup"><span data-stu-id="87bf1-147">Sign in toohello Marketplace if you aren't already Signed in.</span></span>
* <span data-ttu-id="87bf1-148">I högra fönstret hello visas alla tillgängliga planer för hello.</span><span class="sxs-lookup"><span data-stu-id="87bf1-148">In hello right pane, all hello available plans are listed.</span></span> <span data-ttu-id="87bf1-149">Klicka på hello alternativknapp för hello-plan som du vill tooupgrade till.</span><span class="sxs-lookup"><span data-stu-id="87bf1-149">Click hello radio button for hello plan you want tooupgrade to.</span></span>
* <span data-ttu-id="87bf1-150">Om du vill tooupgrade **OK**.</span><span class="sxs-lookup"><span data-stu-id="87bf1-150">If you want tooupgrade, click **OK**.</span></span> <span data-ttu-id="87bf1-151">Om du inte vill att tooupgrade **Avbryt**.</span><span class="sxs-lookup"><span data-stu-id="87bf1-151">If you do not want tooupgrade, click **Cancel**.</span></span>

<span data-ttu-id="87bf1-152">**Viktiga** noggrant skrivskyddade hello dialogrutan innan du uppgraderar eftersom det finns fakturerings- och konsekvenser.</span><span class="sxs-lookup"><span data-stu-id="87bf1-152">**Important** Carefully read hello dialog box before you upgrade because there are billing and use implications.</span></span>

<span data-ttu-id="87bf1-153">**Då avslutas tooRecommendations min prenumeration?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-153">**When will my subscription tooRecommendations end?**</span></span>

<span data-ttu-id="87bf1-154">Din prenumeration avslutas när du avbryter den.</span><span class="sxs-lookup"><span data-stu-id="87bf1-154">Your subscription will end when you cancel it.</span></span> <span data-ttu-id="87bf1-155">Om du vill att toocancel hello följa anvisningar finns i dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="87bf1-155">If you would like toocancel your subscriptions, see hello following instructions.</span></span>

<span data-ttu-id="87bf1-156">**Hur avbryter min rekommendationer prenumeration?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-156">**How do I cancel my Recommendations subscription?**</span></span>

<span data-ttu-id="87bf1-157">toocancel din prenumeration, Använd hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="87bf1-157">toocancel your subscription, use hello following steps.</span></span> <span data-ttu-id="87bf1-158">Om din aktuella prenumeration är en betald prenumeration, fortsätter prenumerationen i kraft förrän hello slutet av hello aktuella fakturering period.</span><span class="sxs-lookup"><span data-stu-id="87bf1-158">If your current subscription is a paid subscription, your subscription continues in effect until hello end of hello current billing period.</span></span> <span data-ttu-id="87bf1-159">Om du behöver hello annullering toobe gälla omedelbart, kontaktar du oss på [Microsoft-supporten](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span><span class="sxs-lookup"><span data-stu-id="87bf1-159">If you need hello cancellation toobe effective immediately, contact us at [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span></span>

<span data-ttu-id="87bf1-160">**Obs** bidrag ges om du avbryter innan hello slutet av en faktureringsperiod eller för transaktioner som inte används i en faktureringsperioden.</span><span class="sxs-lookup"><span data-stu-id="87bf1-160">**Note** No refund is given if you cancel before hello end of a billing period or for unused transactions in a billing period.</span></span>

* <span data-ttu-id="87bf1-161">Navigera toohello [erbjuder sidan](https://datamarket.azure.com/dataset/amla/recommendations).</span><span class="sxs-lookup"><span data-stu-id="87bf1-161">Navigate toohello [offer page](https://datamarket.azure.com/dataset/amla/recommendations).</span></span>
* <span data-ttu-id="87bf1-162">Logga in toohello Marketplace om du inte redan är inloggad.</span><span class="sxs-lookup"><span data-stu-id="87bf1-162">Sign in toohello Marketplace if you aren't already Signed in.</span></span>
* <span data-ttu-id="87bf1-163">Klicka på **Avbryt** toohello höger i hello dataset-namnet och status.</span><span class="sxs-lookup"><span data-stu-id="87bf1-163">Click **Cancel** toohello right of hello dataset name and status.</span></span> <span data-ttu-id="87bf1-164">Du kan använda den här prenumerationen förrän hello hello aktuella fakturering period eller transaktionen gränsen slut (beroende på vilket som inträffar först).</span><span class="sxs-lookup"><span data-stu-id="87bf1-164">You can use this subscription until hello end of hello current billing period or your transaction limit is reached (whichever occurs first).</span></span>

<span data-ttu-id="87bf1-165">Om du vill att toocancel prenumerationen omedelbart så du kan köpa en prenumeration filen en biljett på [Microsoft-supporten](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span><span class="sxs-lookup"><span data-stu-id="87bf1-165">If you would like toocancel your subscription immediately so you can purchase a new subscription, file a ticket at [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).</span></span>

## <a name="getting-started-with-recommendations"></a><span data-ttu-id="87bf1-166">Komma igång med rekommendationer</span><span class="sxs-lookup"><span data-stu-id="87bf1-166">Getting started with Recommendations</span></span>
<span data-ttu-id="87bf1-167">**Är rekommendationer för mig?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-167">**Is Recommendations for me?**</span></span> 

<span data-ttu-id="87bf1-168">Rekommendationerna i Machine Learning är för organisationer och företag som förlitar sig på rekommendationer toocross säljer och upp säljer produkter eller tjänster tootheir kunder.</span><span class="sxs-lookup"><span data-stu-id="87bf1-168">Recommendations in Machine Learning is for organizations and businesses that rely on recommendations toocross-sell and up-sell products or services tootheir customers.</span></span> <span data-ttu-id="87bf1-169">Om du har en kund-riktade webbplats, en säljstyrkan, en inre säljstyrkan eller Callcenter, och om du tillhandahåller en katalog över flera dussin produkter eller tjänster slutresultatet kan dra nytta av rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="87bf1-169">If you have a customer-facing website, a sales force, an inside sales force, or a call center, and if you offer a catalog of more than a few dozen products or services, your bottom line may benefit from using Recommendations.</span></span> 

<span data-ttu-id="87bf1-170">Experimentera med rekommendationer är utformad toobe ganska enkel.</span><span class="sxs-lookup"><span data-stu-id="87bf1-170">Experimenting with Recommendations is designed toobe fairly simple.</span></span> <span data-ttu-id="87bf1-171">hello aktuella API-baserade versionen kräver grundläggande kunskaper i programmering.</span><span class="sxs-lookup"><span data-stu-id="87bf1-171">hello current API-based version requires basic programming skills.</span></span> <span data-ttu-id="87bf1-172">Kontakta hello-leverantör som utvecklat din webbplats om du behöver hjälp.</span><span class="sxs-lookup"><span data-stu-id="87bf1-172">If you need assistance, contact hello vendor who developed your website.</span></span> <span data-ttu-id="87bf1-173">Om du har en intern IT-avdelningen eller intern systemutvecklare ska de kunna tooget rekommendationer toowork för dig.</span><span class="sxs-lookup"><span data-stu-id="87bf1-173">If you have an internal IT department or an in-house developer, they should be able tooget Recommendations toowork for you.</span></span> 

<span data-ttu-id="87bf1-174">**Vad är hello förutsättningar för att skapa rekommendationer?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-174">**What are hello prerequisites for setting up Recommendations?**</span></span>

<span data-ttu-id="87bf1-175">Rekommendationer kräver att du har en logg över användarens val när det gäller tooyour katalog.</span><span class="sxs-lookup"><span data-stu-id="87bf1-175">Recommendations requires that you have a log of user choices as it relates tooyour catalog.</span></span> <span data-ttu-id="87bf1-176">Om du inte har sådan loggen och du har en kund-webbplatser, rekommendationer kan samla in användaraktivitet för dig.</span><span class="sxs-lookup"><span data-stu-id="87bf1-176">If you don't have such a log and you do have a customer facing website, Recommendations can collect user activity for you.</span></span> 

<span data-ttu-id="87bf1-177">Rekommendationer kräver också en katalog med dina produkter eller tjänster.</span><span class="sxs-lookup"><span data-stu-id="87bf1-177">Recommendations also requires a catalog of your products or services.</span></span> <span data-ttu-id="87bf1-178">Om du inte har hello katalog rekommendationer använder hello faktiska användning kundinformation och destillera en katalog.</span><span class="sxs-lookup"><span data-stu-id="87bf1-178">If you don't have hello catalog, Recommendations can use hello actual customer usage data and distill a catalog.</span></span> <span data-ttu-id="87bf1-179">En underförstådda katalog kommer inte att innehålla objekt som inte rapporterades som en del av användartransaktioner.</span><span class="sxs-lookup"><span data-stu-id="87bf1-179">An implied catalog will not include items that were not reported as part of user transactions.</span></span>

<span data-ttu-id="87bf1-180">**Hur ställer jag in rekommendationer för hello första gången?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-180">**How do I set up Recommendations for hello first time?**</span></span>

<span data-ttu-id="87bf1-181">Efter [prenumerationsapp](https://datamarket.azure.com/dataset/amla/recommendations) tooRecommendations, bör du använda hello API-dokumentationen i hello [Azure Machine Learning rekommendationer - snabbstartsguiden](machine-learning-recommendation-api-quick-start-guide.md) tooset hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="87bf1-181">After [subscribing](https://datamarket.azure.com/dataset/amla/recommendations) tooRecommendations, you should use hello API documentation in hello [Azure Machine Learning Recommendations - Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md) tooset up hello service.</span></span>

<span data-ttu-id="87bf1-182">**Var hittar jag API-dokumentationen?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-182">**Where can I find API documentation?**</span></span> 

<span data-ttu-id="87bf1-183">hello API-dokumentationen är [rekommendationer för Azure Machine Learning-snabbstartsguiden](machine-learning-recommendation-api-quick-start-guide.md).</span><span class="sxs-lookup"><span data-stu-id="87bf1-183">hello API documentation is [Azure Machine Learning Recommendations -Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md).</span></span>

<span data-ttu-id="87bf1-184">**Alternativ gör jag har tooRecommendations av tooupload katalog- och användningsdata?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-184">**What options do I have tooupload catalog and usage data tooRecommendations?**</span></span>

<span data-ttu-id="87bf1-185">Har du två alternativ för uppladdning av data catalog- och användningsdata: du kan exportera hello data från ditt CRM-system eller andra loggar och överföra den tooRecommendations eller du kan lägga till taggar tooyour webbplats som ska övervaka användaraktiviteter.</span><span class="sxs-lookup"><span data-stu-id="87bf1-185">You have two options for uploading your catalog and usage data: You can export hello data from your CRM system or other logs and upload it tooRecommendations, or you can add tags tooyour website that will track user activities.</span></span> <span data-ttu-id="87bf1-186">Om du använder hello senare metoden lagras hello data i Azure.</span><span class="sxs-lookup"><span data-stu-id="87bf1-186">If you use hello latter method, hello data will be stored in Azure.</span></span>

## <a name="maintenance-and-support"></a><span data-ttu-id="87bf1-187">Underhåll och support</span><span class="sxs-lookup"><span data-stu-id="87bf1-187">Maintenance and support</span></span>
<span data-ttu-id="87bf1-188">**Hur stor kan min datauppsättning?**</span><span class="sxs-lookup"><span data-stu-id="87bf1-188">**How large can my data set be?**</span></span>

<span data-ttu-id="87bf1-189">Varje datamängd kan innehålla upp too100 000 katalogobjekt och in användningsdata too2048 MB.</span><span class="sxs-lookup"><span data-stu-id="87bf1-189">Each data set can contain up too100,000 catalog items and up too2048 MB of usage data.</span></span>
<span data-ttu-id="87bf1-190">En prenumeration kan dessutom innehålla too10 data uppsättningar (modeller).</span><span class="sxs-lookup"><span data-stu-id="87bf1-190">In addition, a subscription can contain up too10 data sets (models).</span></span>

<span data-ttu-id="87bf1-191">**Var hittar jag teknisk support för rekommendationer**</span><span class="sxs-lookup"><span data-stu-id="87bf1-191">**Where can I get technical support for Recommendations?**</span></span>

<span data-ttu-id="87bf1-192">Teknisk support är tillgänglig på hello [stöd för Microsoft Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) plats.</span><span class="sxs-lookup"><span data-stu-id="87bf1-192">Technical support is available on hello [Microsoft Azure Support](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) site.</span></span>

<span data-ttu-id="87bf1-193">**Var hittar jag hello användningsvillkor**</span><span class="sxs-lookup"><span data-stu-id="87bf1-193">**Where can I find hello terms of use?**</span></span>

<span data-ttu-id="87bf1-194">[Microsoft Azure Machine Learning rekommendationer API användarvillkoren](https://datamarket.azure.com/dataset/amla/recommendations#terms).</span><span class="sxs-lookup"><span data-stu-id="87bf1-194">[Microsoft Azure Machine Learning Recommendations API Terms of Service](https://datamarket.azure.com/dataset/amla/recommendations#terms).</span></span>

