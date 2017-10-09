---
title: "aaaLearn om begränsning i BizTalk-tjänst | Microsoft Docs"
description: "Läs mer om begränsning tröskelvärden och resulterande runtime beteenden för BizTalk-tjänst. Begränsning är baserad på minnesanvändning och antalet meddelanden. MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: f6663cf2-cda4-4bac-855e-27d2ad5c4fa4
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 46c8806c3a1f4eeb793f721f849771e0ec155197
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-throttling"></a><span data-ttu-id="9c24c-105">BizTalk Services: Begränsning</span><span class="sxs-lookup"><span data-stu-id="9c24c-105">BizTalk Services: Throttling</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="9c24c-106">Azure BizTalk-tjänst implementerar tjänsten begränsning baserat på två villkor: minne användnings- och hello antalet samtidiga meddelanden bearbetning.</span><span class="sxs-lookup"><span data-stu-id="9c24c-106">Azure BizTalk Services implements service throttling based on two conditions: memory usage and hello number of simultaneous messages processing.</span></span> <span data-ttu-id="9c24c-107">Det här avsnittet visar hello begränsning tröskelvärden och beskriver hello Runtime beteende när ett bandbreddsbegränsning villkor uppfylls.</span><span class="sxs-lookup"><span data-stu-id="9c24c-107">This topic lists hello throttling thresholds and describes hello Runtime behavior when a throttling condition occurs.</span></span>

## <a name="throttling-thresholds"></a><span data-ttu-id="9c24c-108">Begränsning av tröskelvärden</span><span class="sxs-lookup"><span data-stu-id="9c24c-108">Throttling Thresholds</span></span>
<span data-ttu-id="9c24c-109">följande tabell visar hello hello bandbreddsbegränsning käll- och tröskelvärden:</span><span class="sxs-lookup"><span data-stu-id="9c24c-109">hello following table lists hello throttling source and thresholds:</span></span>

|  | <span data-ttu-id="9c24c-110">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9c24c-110">Description</span></span> | <span data-ttu-id="9c24c-111">Lågtröskelövervakare</span><span class="sxs-lookup"><span data-stu-id="9c24c-111">Low Threshold</span></span> | <span data-ttu-id="9c24c-112">Tröskelvärde för hög</span><span class="sxs-lookup"><span data-stu-id="9c24c-112">High Threshold</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9c24c-113">Minne</span><span class="sxs-lookup"><span data-stu-id="9c24c-113">Memory</span></span> |<span data-ttu-id="9c24c-114">% av total system minne tillgängligt/PageFileBytes.</span><span class="sxs-lookup"><span data-stu-id="9c24c-114">% of total system memory available/PageFileBytes.</span></span> <p><p><span data-ttu-id="9c24c-115">Totalt antal tillgängliga PageFileBytes är cirka 2 gånger hello RAM hello system.</span><span class="sxs-lookup"><span data-stu-id="9c24c-115">Total available PageFileBytes is approximately 2 times hello RAM of hello system.</span></span> |<span data-ttu-id="9c24c-116">60%</span><span class="sxs-lookup"><span data-stu-id="9c24c-116">60%</span></span> |<span data-ttu-id="9c24c-117">70%</span><span class="sxs-lookup"><span data-stu-id="9c24c-117">70%</span></span> |
| <span data-ttu-id="9c24c-118">Meddelandebehandling</span><span class="sxs-lookup"><span data-stu-id="9c24c-118">Message Processing</span></span> |<span data-ttu-id="9c24c-119">Antal meddelanden som bearbetar samtidigt</span><span class="sxs-lookup"><span data-stu-id="9c24c-119">Number of messages processing simultaneously</span></span> |<span data-ttu-id="9c24c-120">40 * antal kärnor</span><span class="sxs-lookup"><span data-stu-id="9c24c-120">40 * number of cores</span></span> |<span data-ttu-id="9c24c-121">100 * antal kärnor</span><span class="sxs-lookup"><span data-stu-id="9c24c-121">100 * number of cores</span></span> |

<span data-ttu-id="9c24c-122">När en högre tröskelvärdet har uppnåtts startar toothrottle Azure BizTalk-tjänst.</span><span class="sxs-lookup"><span data-stu-id="9c24c-122">When a high threshold is reached, Azure BizTalk Services starts toothrottle.</span></span> <span data-ttu-id="9c24c-123">Begränsning slutar när hello låg tröskelvärde har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="9c24c-123">Throttling stops when hello low threshold is reached.</span></span> <span data-ttu-id="9c24c-124">Till exempel använder din tjänst 65% av systemminnet.</span><span class="sxs-lookup"><span data-stu-id="9c24c-124">For example, your service is using 65% system memory.</span></span> <span data-ttu-id="9c24c-125">I den här situationen begränsning inte hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c24c-125">In this situation, hello service does not throttle.</span></span> <span data-ttu-id="9c24c-126">Tjänsten startar med 70% av systemminnet.</span><span class="sxs-lookup"><span data-stu-id="9c24c-126">Your service starts using 70% system memory.</span></span> <span data-ttu-id="9c24c-127">I den här situationen hello tjänsten begränsar och fortsätter toothrottle tills hello tjänsten använder 60% (låg tröskel) minne.</span><span class="sxs-lookup"><span data-stu-id="9c24c-127">In this situation, hello service throttles and continues toothrottle until hello service uses 60% (low threshold) system memory.</span></span>

<span data-ttu-id="9c24c-128">Azure BizTalk-tjänster spårar hello begränsning status (normala tillstånd jämfört med begränsad tillstånd) och hello begränsning varaktighet.</span><span class="sxs-lookup"><span data-stu-id="9c24c-128">Azure BizTalk Services tracks hello throttling status (normal state vs. throttled state) and hello throttling duration.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="9c24c-129">Runtime-beteende</span><span class="sxs-lookup"><span data-stu-id="9c24c-129">Runtime Behavior</span></span>
<span data-ttu-id="9c24c-130">När Azure BizTalk-tjänst anger tillståndet bandbreddsbegränsning, inträffar hello följande:</span><span class="sxs-lookup"><span data-stu-id="9c24c-130">When Azure BizTalk Services enters a throttling state, hello following occurs:</span></span>

* <span data-ttu-id="9c24c-131">Begränsning är per rollinstans.</span><span class="sxs-lookup"><span data-stu-id="9c24c-131">Throttling is per role instance.</span></span> <span data-ttu-id="9c24c-132">Exempel:</span><span class="sxs-lookup"><span data-stu-id="9c24c-132">For example:</span></span><br/>
  <span data-ttu-id="9c24c-133">RoleInstanceA begränsning.</span><span class="sxs-lookup"><span data-stu-id="9c24c-133">RoleInstanceA is throttling.</span></span> <span data-ttu-id="9c24c-134">RoleInstanceB begränsning är inte.</span><span class="sxs-lookup"><span data-stu-id="9c24c-134">RoleInstanceB is not throttling.</span></span> <span data-ttu-id="9c24c-135">I den här situationen behandlas meddelanden i RoleInstanceB som förväntat.</span><span class="sxs-lookup"><span data-stu-id="9c24c-135">In this situation, messages in RoleInstanceB are processed as expected.</span></span> <span data-ttu-id="9c24c-136">Meddelanden i RoleInstanceA ignoreras och misslyckas med hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="9c24c-136">Messages in RoleInstanceA are discarded and fail with hello following error:</span></span><br/><br/><span data-ttu-id="9c24c-137">
  **Servern är upptagen. Försök igen.**</span><span class="sxs-lookup"><span data-stu-id="9c24c-137">
**Server is busy. Please try again.**</span></span><br/><br/>
* <span data-ttu-id="9c24c-138">Alla pull-källor inte söka eller hämta ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="9c24c-138">Any pull sources do not poll or download a message.</span></span> <span data-ttu-id="9c24c-139">Exempel:</span><span class="sxs-lookup"><span data-stu-id="9c24c-139">For example:</span></span><br/>
  <span data-ttu-id="9c24c-140">En pipeline tar emot meddelanden från en extern källa för FTP.</span><span class="sxs-lookup"><span data-stu-id="9c24c-140">A pipeline pulls messages from an external FTP source.</span></span> <span data-ttu-id="9c24c-141">Hej rollinstansen gör hello pull hämtar i bandbreddsbegränsning läge.</span><span class="sxs-lookup"><span data-stu-id="9c24c-141">hello role instance doing hello pull gets into a throttling state.</span></span> <span data-ttu-id="9c24c-142">I den här situationen stoppar hello pipeline hämtar ytterligare meddelanden tills hälsningspaket rollinstansen slutar begränsning.</span><span class="sxs-lookup"><span data-stu-id="9c24c-142">In this situation, hello pipeline stops downloading additional messages until hello role instance stops throttling.</span></span>
* <span data-ttu-id="9c24c-143">Ett svar skickas toohello klienten så hello-klienten kan skicka hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="9c24c-143">A response is sent toohello client so hello client can resubmit hello message.</span></span>
* <span data-ttu-id="9c24c-144">Du måste vänta tills hello begränsning har lösts.</span><span class="sxs-lookup"><span data-stu-id="9c24c-144">You must wait until hello throttling is resolved.</span></span> <span data-ttu-id="9c24c-145">Mer specifikt måste du vänta tills hello låg tröskelvärdet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="9c24c-145">Specifically, you must wait until hello low threshold is reached.</span></span>

## <a name="important-notes"></a><span data-ttu-id="9c24c-146">Obs!</span><span class="sxs-lookup"><span data-stu-id="9c24c-146">Important notes</span></span>
* <span data-ttu-id="9c24c-147">Begränsning kan inte inaktiveras.</span><span class="sxs-lookup"><span data-stu-id="9c24c-147">Throttling cannot be disabled.</span></span>
* <span data-ttu-id="9c24c-148">Begränsning av tröskelvärden kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="9c24c-148">Throttling thresholds cannot be modified.</span></span>
* <span data-ttu-id="9c24c-149">Begränsning har implementerats hela systemet.</span><span class="sxs-lookup"><span data-stu-id="9c24c-149">Throttling is implemented system-wide.</span></span>
* <span data-ttu-id="9c24c-150">hello Azure SQL Database-Server har också inbyggd begränsning.</span><span class="sxs-lookup"><span data-stu-id="9c24c-150">hello Azure SQL Database Server also has built-in throttling.</span></span>

## <a name="additional-azure-biztalk-services-topics"></a><span data-ttu-id="9c24c-151">Avsnitt om ytterligare Azure BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="9c24c-151">Additional Azure BizTalk Services topics</span></span>
* [<span data-ttu-id="9c24c-152">Installera hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="9c24c-152">Installing hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [<span data-ttu-id="9c24c-153">Självstudier: Azure BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="9c24c-153">Tutorials: Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [<span data-ttu-id="9c24c-154">Hur jag börja använda hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="9c24c-154">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="9c24c-155">Azure BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="9c24c-155">Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a><span data-ttu-id="9c24c-156">Se även</span><span class="sxs-lookup"><span data-stu-id="9c24c-156">See Also</span></span>
* [<span data-ttu-id="9c24c-157">BizTalk-tjänst: Utvecklare, Basic, Standard och Premium-utgåvor diagram</span><span class="sxs-lookup"><span data-stu-id="9c24c-157">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [<span data-ttu-id="9c24c-158">BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal</span><span class="sxs-lookup"><span data-stu-id="9c24c-158">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [<span data-ttu-id="9c24c-159">BizTalk Services: Etablering av statusdiagram</span><span class="sxs-lookup"><span data-stu-id="9c24c-159">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [<span data-ttu-id="9c24c-160">BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning</span><span class="sxs-lookup"><span data-stu-id="9c24c-160">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [<span data-ttu-id="9c24c-161">BizTalk Services: Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="9c24c-161">BizTalk Services: Backup and Restore</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [<span data-ttu-id="9c24c-162">BizTalk Services: Utfärdarens namn och nyckel</span><span class="sxs-lookup"><span data-stu-id="9c24c-162">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

