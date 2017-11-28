---
title: "Lär dig mer om hur du begränsar i BizTalk-tjänst | Microsoft Docs"
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
ms.openlocfilehash: 145e7470bbc01c676a1fb5856c0f9a8726e667fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-throttling"></a><span data-ttu-id="534a3-105">BizTalk Services: Begränsning</span><span class="sxs-lookup"><span data-stu-id="534a3-105">BizTalk Services: Throttling</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="534a3-106">Azure BizTalk-tjänst implementerar tjänsten begränsning baserat på två villkor: minnesanvändning och antalet samtidiga meddelanden bearbetning.</span><span class="sxs-lookup"><span data-stu-id="534a3-106">Azure BizTalk Services implements service throttling based on two conditions: memory usage and the number of simultaneous messages processing.</span></span> <span data-ttu-id="534a3-107">Det här avsnittet visar bandbreddsbegränsning tröskelvärden och beskriver beteende under körning när ett bandbreddsbegränsning villkor uppfylls.</span><span class="sxs-lookup"><span data-stu-id="534a3-107">This topic lists the throttling thresholds and describes the Runtime behavior when a throttling condition occurs.</span></span>

## <a name="throttling-thresholds"></a><span data-ttu-id="534a3-108">Begränsning av tröskelvärden</span><span class="sxs-lookup"><span data-stu-id="534a3-108">Throttling Thresholds</span></span>
<span data-ttu-id="534a3-109">I följande tabell visas bandbreddsbegränsning käll- och tröskelvärden:</span><span class="sxs-lookup"><span data-stu-id="534a3-109">The following table lists the throttling source and thresholds:</span></span>

|  | <span data-ttu-id="534a3-110">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="534a3-110">Description</span></span> | <span data-ttu-id="534a3-111">Lågtröskelövervakare</span><span class="sxs-lookup"><span data-stu-id="534a3-111">Low Threshold</span></span> | <span data-ttu-id="534a3-112">Tröskelvärde för hög</span><span class="sxs-lookup"><span data-stu-id="534a3-112">High Threshold</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="534a3-113">Minne</span><span class="sxs-lookup"><span data-stu-id="534a3-113">Memory</span></span> |<span data-ttu-id="534a3-114">% av total system minne tillgängligt/PageFileBytes.</span><span class="sxs-lookup"><span data-stu-id="534a3-114">% of total system memory available/PageFileBytes.</span></span> <p><p><span data-ttu-id="534a3-115">Totalt antal tillgängliga PageFileBytes är cirka 2 gånger mängden RAM-minne i systemet.</span><span class="sxs-lookup"><span data-stu-id="534a3-115">Total available PageFileBytes is approximately 2 times the RAM of the system.</span></span> |<span data-ttu-id="534a3-116">60%</span><span class="sxs-lookup"><span data-stu-id="534a3-116">60%</span></span> |<span data-ttu-id="534a3-117">70%</span><span class="sxs-lookup"><span data-stu-id="534a3-117">70%</span></span> |
| <span data-ttu-id="534a3-118">Meddelandebehandling</span><span class="sxs-lookup"><span data-stu-id="534a3-118">Message Processing</span></span> |<span data-ttu-id="534a3-119">Antal meddelanden som bearbetar samtidigt</span><span class="sxs-lookup"><span data-stu-id="534a3-119">Number of messages processing simultaneously</span></span> |<span data-ttu-id="534a3-120">40 * antal kärnor</span><span class="sxs-lookup"><span data-stu-id="534a3-120">40 * number of cores</span></span> |<span data-ttu-id="534a3-121">100 * antal kärnor</span><span class="sxs-lookup"><span data-stu-id="534a3-121">100 * number of cores</span></span> |

<span data-ttu-id="534a3-122">När en hög tröskelvärde uppnås börjar Azure BizTalk-tjänst begränsning.</span><span class="sxs-lookup"><span data-stu-id="534a3-122">When a high threshold is reached, Azure BizTalk Services starts to throttle.</span></span> <span data-ttu-id="534a3-123">Begränsning stoppas när det lägsta tröskelvärdet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="534a3-123">Throttling stops when the low threshold is reached.</span></span> <span data-ttu-id="534a3-124">Till exempel använder din tjänst 65% av systemminnet.</span><span class="sxs-lookup"><span data-stu-id="534a3-124">For example, your service is using 65% system memory.</span></span> <span data-ttu-id="534a3-125">I så fall kan begränsning tjänsten inte.</span><span class="sxs-lookup"><span data-stu-id="534a3-125">In this situation, the service does not throttle.</span></span> <span data-ttu-id="534a3-126">Tjänsten startar med 70% av systemminnet.</span><span class="sxs-lookup"><span data-stu-id="534a3-126">Your service starts using 70% system memory.</span></span> <span data-ttu-id="534a3-127">I så fall kan tjänsten begränsar och fortsätter att begränsa förrän tjänsten använder 60% (låg tröskel) minne.</span><span class="sxs-lookup"><span data-stu-id="534a3-127">In this situation, the service throttles and continues to throttle until the service uses 60% (low threshold) system memory.</span></span>

<span data-ttu-id="534a3-128">Azure BizTalk-tjänster spårar bandbreddsbegränsning status (normala tillstånd jämfört med begränsad tillstånd) och bandbreddsbegränsning varaktighet.</span><span class="sxs-lookup"><span data-stu-id="534a3-128">Azure BizTalk Services tracks the throttling status (normal state vs. throttled state) and the throttling duration.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="534a3-129">Runtime-beteende</span><span class="sxs-lookup"><span data-stu-id="534a3-129">Runtime Behavior</span></span>
<span data-ttu-id="534a3-130">När Azure BizTalk-tjänst anger tillståndet bandbreddsbegränsning, inträffar följande:</span><span class="sxs-lookup"><span data-stu-id="534a3-130">When Azure BizTalk Services enters a throttling state, the following occurs:</span></span>

* <span data-ttu-id="534a3-131">Begränsning är per rollinstans.</span><span class="sxs-lookup"><span data-stu-id="534a3-131">Throttling is per role instance.</span></span> <span data-ttu-id="534a3-132">Exempel:</span><span class="sxs-lookup"><span data-stu-id="534a3-132">For example:</span></span><br/>
  <span data-ttu-id="534a3-133">RoleInstanceA begränsning.</span><span class="sxs-lookup"><span data-stu-id="534a3-133">RoleInstanceA is throttling.</span></span> <span data-ttu-id="534a3-134">RoleInstanceB begränsning är inte.</span><span class="sxs-lookup"><span data-stu-id="534a3-134">RoleInstanceB is not throttling.</span></span> <span data-ttu-id="534a3-135">I den här situationen behandlas meddelanden i RoleInstanceB som förväntat.</span><span class="sxs-lookup"><span data-stu-id="534a3-135">In this situation, messages in RoleInstanceB are processed as expected.</span></span> <span data-ttu-id="534a3-136">Meddelanden i RoleInstanceA ignoreras och misslyckas med följande fel:</span><span class="sxs-lookup"><span data-stu-id="534a3-136">Messages in RoleInstanceA are discarded and fail with the following error:</span></span><br/><br/><span data-ttu-id="534a3-137">
  **Servern är upptagen. Försök igen.**</span><span class="sxs-lookup"><span data-stu-id="534a3-137">
**Server is busy. Please try again.**</span></span><br/><br/>
* <span data-ttu-id="534a3-138">Alla pull-källor inte söka eller hämta ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="534a3-138">Any pull sources do not poll or download a message.</span></span> <span data-ttu-id="534a3-139">Exempel:</span><span class="sxs-lookup"><span data-stu-id="534a3-139">For example:</span></span><br/>
  <span data-ttu-id="534a3-140">En pipeline tar emot meddelanden från en extern källa för FTP.</span><span class="sxs-lookup"><span data-stu-id="534a3-140">A pipeline pulls messages from an external FTP source.</span></span> <span data-ttu-id="534a3-141">Instansen gör pull hämtar i bandbreddsbegränsning läge.</span><span class="sxs-lookup"><span data-stu-id="534a3-141">The role instance doing the pull gets into a throttling state.</span></span> <span data-ttu-id="534a3-142">I den här situationen stoppar pipeline hämtar ytterligare meddelanden tills rollinstansen slutar begränsning.</span><span class="sxs-lookup"><span data-stu-id="534a3-142">In this situation, the pipeline stops downloading additional messages until the role instance stops throttling.</span></span>
* <span data-ttu-id="534a3-143">Ett svar skickas till klienten så att klienten kan skicka meddelandet.</span><span class="sxs-lookup"><span data-stu-id="534a3-143">A response is sent to the client so the client can resubmit the message.</span></span>
* <span data-ttu-id="534a3-144">Du måste vänta tills den begränsning har lösts.</span><span class="sxs-lookup"><span data-stu-id="534a3-144">You must wait until the throttling is resolved.</span></span> <span data-ttu-id="534a3-145">Mer specifikt måste du vänta tills det lägsta tröskelvärdet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="534a3-145">Specifically, you must wait until the low threshold is reached.</span></span>

## <a name="important-notes"></a><span data-ttu-id="534a3-146">Obs!</span><span class="sxs-lookup"><span data-stu-id="534a3-146">Important notes</span></span>
* <span data-ttu-id="534a3-147">Begränsning kan inte inaktiveras.</span><span class="sxs-lookup"><span data-stu-id="534a3-147">Throttling cannot be disabled.</span></span>
* <span data-ttu-id="534a3-148">Begränsning av tröskelvärden kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="534a3-148">Throttling thresholds cannot be modified.</span></span>
* <span data-ttu-id="534a3-149">Begränsning har implementerats hela systemet.</span><span class="sxs-lookup"><span data-stu-id="534a3-149">Throttling is implemented system-wide.</span></span>
* <span data-ttu-id="534a3-150">Azure SQL Database-Server har också inbyggd begränsning.</span><span class="sxs-lookup"><span data-stu-id="534a3-150">The Azure SQL Database Server also has built-in throttling.</span></span>

## <a name="additional-azure-biztalk-services-topics"></a><span data-ttu-id="534a3-151">Avsnitt om ytterligare Azure BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="534a3-151">Additional Azure BizTalk Services topics</span></span>
* [<span data-ttu-id="534a3-152">Installera Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="534a3-152">Installing the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [<span data-ttu-id="534a3-153">Självstudier: Azure BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="534a3-153">Tutorials: Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [<span data-ttu-id="534a3-154">Hur gör jag för att börja använda Azure BizTalk Services SDK?</span><span class="sxs-lookup"><span data-stu-id="534a3-154">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="534a3-155">Azure BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="534a3-155">Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a><span data-ttu-id="534a3-156">Se även</span><span class="sxs-lookup"><span data-stu-id="534a3-156">See Also</span></span>
* [<span data-ttu-id="534a3-157">BizTalk-tjänst: Utvecklare, Basic, Standard och Premium-utgåvor diagram</span><span class="sxs-lookup"><span data-stu-id="534a3-157">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [<span data-ttu-id="534a3-158">BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal</span><span class="sxs-lookup"><span data-stu-id="534a3-158">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [<span data-ttu-id="534a3-159">BizTalk Services: Etablering av statusdiagram</span><span class="sxs-lookup"><span data-stu-id="534a3-159">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [<span data-ttu-id="534a3-160">BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning</span><span class="sxs-lookup"><span data-stu-id="534a3-160">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [<span data-ttu-id="534a3-161">BizTalk Services: Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="534a3-161">BizTalk Services: Backup and Restore</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [<span data-ttu-id="534a3-162">BizTalk Services: Utfärdarens namn och nyckel</span><span class="sxs-lookup"><span data-stu-id="534a3-162">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

