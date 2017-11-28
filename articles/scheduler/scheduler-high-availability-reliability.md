---
title: "aaaScheduler hög tillgänglighet och tillförlitlighet"
description: "Schemaläggaren hög tillgänglighet och tillförlitlighet"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5ec78e60-a9b9-405a-91a8-f010f3872d50
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2016
ms.author: deli
ms.openlocfilehash: 5c9efb333eb42b393adc5deea657ca99206d425e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-high-availability-and-reliability"></a><span data-ttu-id="8698c-103">Schemaläggaren hög tillgänglighet och tillförlitlighet</span><span class="sxs-lookup"><span data-stu-id="8698c-103">Scheduler High-Availability and Reliability</span></span>
## <a name="azure-scheduler-high-availability"></a><span data-ttu-id="8698c-104">Azure Scheduler hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="8698c-104">Azure Scheduler High-Availability</span></span>
<span data-ttu-id="8698c-105">Som en kärna Azure-plattformstjänsten Azure Schemaläggaren har hög tillgänglighet och funktioner både geo-redundant service-distributionen och geo regionala jobbet replikering.</span><span class="sxs-lookup"><span data-stu-id="8698c-105">As a core Azure platform service, Azure Scheduler is highly available and features both geo-redundant service deployment and geo-regional job replication.</span></span>

### <a name="geo-redundant-service-deployment"></a><span data-ttu-id="8698c-106">GEO-redundant tjänstdistribution</span><span class="sxs-lookup"><span data-stu-id="8698c-106">Geo-redundant service deployment</span></span>
<span data-ttu-id="8698c-107">Azure Schemaläggaren är tillgänglig via hello Användargränssnittet i nästan alla geografisk region som är i Azure idag.</span><span class="sxs-lookup"><span data-stu-id="8698c-107">Azure Scheduler is available via hello UI in almost every geo region that's in Azure today.</span></span> <span data-ttu-id="8698c-108">hello listan över regioner som Azure Schemaläggaren finns i är [som listas här](https://azure.microsoft.com/regions/#services).</span><span class="sxs-lookup"><span data-stu-id="8698c-108">hello list of regions that Azure Scheduler is available in is [listed here](https://azure.microsoft.com/regions/#services).</span></span> <span data-ttu-id="8698c-109">Om ett datacenter i en värdbaserad region återges inte tillgänglig, är hello redundans funktionerna i Azure Schemaläggaren så att hello-tjänsten är tillgänglig från en annan datacenter.</span><span class="sxs-lookup"><span data-stu-id="8698c-109">If a data center in a hosted region is rendered unavailable, hello failover capabilities of Azure Scheduler are such that hello service is available from another data center.</span></span>

### <a name="geo-regional-job-replication"></a><span data-ttu-id="8698c-110">GEO-regionala jobbet replikering</span><span class="sxs-lookup"><span data-stu-id="8698c-110">Geo-regional job replication</span></span>
<span data-ttu-id="8698c-111">Inte bara är hello Azure Scheduler frontend tillgänglig för av hanteringsbegäranden, men din egen jobbet är också georeplikerad.</span><span class="sxs-lookup"><span data-stu-id="8698c-111">Not only is hello Azure Scheduler front-end available for management requests, but your own job is also geo-replicated.</span></span> <span data-ttu-id="8698c-112">När det finns ett avbrott i en region, Azure Scheduler växlas över och garanterar hello jobbet körs från en annan datacenter i hello parad geografiskt område.</span><span class="sxs-lookup"><span data-stu-id="8698c-112">When there’s an outage in one region, Azure Scheduler fails over and ensures that hello job is run from another data center in hello paired geographic region.</span></span>

<span data-ttu-id="8698c-113">Till exempel om du har skapat ett jobb i södra centrala USA, replikerar Azure Scheduler automatiskt det jobbet i norra centrala USA.</span><span class="sxs-lookup"><span data-stu-id="8698c-113">For example, if you’ve created a job in South Central US, Azure Scheduler automatically replicates that job in North Central US.</span></span> <span data-ttu-id="8698c-114">När det finns ett fel i södra centrala USA, säkerställer Azure Scheduler hello jobbet körs från norra centrala USA.</span><span class="sxs-lookup"><span data-stu-id="8698c-114">When there’s a failure in South Central US, Azure Scheduler ensures that hello job is run from North Central US.</span></span> 

![][1]

<span data-ttu-id="8698c-115">Därför garanterar Azure Scheduler att dina data förblir inom hello samma bredare geografiska region om ett fel uppstår på Azure.</span><span class="sxs-lookup"><span data-stu-id="8698c-115">As a result, Azure Scheduler ensures that your data stays within hello same broader geographic region in case of an Azure failure.</span></span> <span data-ttu-id="8698c-116">Därför kan du inte behöver duplicera din jobbet bara tooadd hög tillgänglighet – Azure Scheduler innehåller automatiskt funktioner för hög tillgänglighet för dina jobb.</span><span class="sxs-lookup"><span data-stu-id="8698c-116">As a result, you need not duplicate your job just tooadd high availability – Azure Scheduler automatically provides high-availability capabilities for your jobs.</span></span>

## <a name="azure-scheduler-reliability"></a><span data-ttu-id="8698c-117">Azure Scheduler tillförlitlighet</span><span class="sxs-lookup"><span data-stu-id="8698c-117">Azure Scheduler Reliability</span></span>
<span data-ttu-id="8698c-118">Azure Scheduler garanterar sin egen hög tillgänglighet och använder en annan metod toouser skapade jobb.</span><span class="sxs-lookup"><span data-stu-id="8698c-118">Azure Scheduler guarantees its own high-availability and takes a different approach toouser-created jobs.</span></span> <span data-ttu-id="8698c-119">Jobbet kan till exempel anropa en HTTP-slutpunkt som inte är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="8698c-119">For example, your job may invoke an HTTP endpoint that’s unavailable.</span></span> <span data-ttu-id="8698c-120">Azure Scheduler ändå försöker tooexecute ditt jobb, genom att ge dig alternativ toodeal med fel.</span><span class="sxs-lookup"><span data-stu-id="8698c-120">Azure Scheduler nonetheless tries tooexecute your job successfully, by giving you alternative options toodeal with failure.</span></span> <span data-ttu-id="8698c-121">Azure Scheduler gör detta på två sätt:</span><span class="sxs-lookup"><span data-stu-id="8698c-121">Azure Scheduler does this in two ways:</span></span>

### <a name="configurable-retry-policy-via-retrypolicy"></a><span data-ttu-id="8698c-122">Konfigurerbar försök princip via ”retryPolicy”</span><span class="sxs-lookup"><span data-stu-id="8698c-122">Configurable Retry Policy via “retryPolicy”</span></span>
<span data-ttu-id="8698c-123">Azure Schemaläggaren kan du tooconfigure en återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="8698c-123">Azure Scheduler allows you tooconfigure a retry policy.</span></span> <span data-ttu-id="8698c-124">Som standard om ett jobb misslyckas försöker Schemaläggaren hello jobbet igen fyra gånger, med 30 sekunders intervall.</span><span class="sxs-lookup"><span data-stu-id="8698c-124">By default, if a job fails, Scheduler tries hello job again four more times, at 30-second intervals.</span></span> <span data-ttu-id="8698c-125">Du kan konfigurera den här försök princip toobe mer aggressivt (till exempel tio gånger, med 30 sekunders mellanrum) eller glesare (till exempel två gånger med daglig intervall.)</span><span class="sxs-lookup"><span data-stu-id="8698c-125">You may re-configure this retry policy toobe more aggressive (for example, ten times, at 30-second intervals) or looser (for example, two times, at daily intervals.)</span></span>

<span data-ttu-id="8698c-126">Som ett exempel på när det kan hjälpa dig, kan du skapa ett jobb som körs en gång i veckan och anropar en HTTP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="8698c-126">As an example of when this may help, you may create a job that runs once a week and invokes an HTTP endpoint.</span></span> <span data-ttu-id="8698c-127">Om hello HTTP-slutpunkten är igång på några timmar när jobbet körs kan kanske du inte vill toowait en mer vecka för hello jobbet toorun igen eftersom även hello försök standardprincipen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="8698c-127">If hello HTTP endpoint is down for a few hours when your job runs, you may not want toowait one more week for hello job toorun again since even hello default retry policy will fail.</span></span> <span data-ttu-id="8698c-128">I sådana fall måste du kanske konfigurera om hello standard försök princip tooretry alla tre timmar (till exempel) i stället för med 30 sekunders mellanrum.</span><span class="sxs-lookup"><span data-stu-id="8698c-128">In such cases, you may reconfigure hello standard retry policy tooretry every three hours (for example) instead of every 30 seconds.</span></span>

<span data-ttu-id="8698c-129">hur tooconfigure en återförsöksprincip finns för toolearn[retryPolicy](scheduler-concepts-terms.md#retrypolicy).</span><span class="sxs-lookup"><span data-stu-id="8698c-129">toolearn how tooconfigure a retry policy, refer too[retryPolicy](scheduler-concepts-terms.md#retrypolicy).</span></span>

### <a name="alternate-endpoint-configurability-via-erroraction"></a><span data-ttu-id="8698c-130">Den alternativa slutpunkten konfigurationsmöjligheter via ”errorAction”</span><span class="sxs-lookup"><span data-stu-id="8698c-130">Alternate Endpoint Configurability via “errorAction”</span></span>
<span data-ttu-id="8698c-131">Hello mål slutpunkten för din Azure Scheduler-jobbet är inte kan nås, faller Azure Scheduler tillbaka toohello alternativa felhantering slutpunkten när du har följt av dess återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="8698c-131">If hello target endpoint for your Azure Scheduler job remains unreachable, Azure Scheduler falls back toohello alternate error-handling endpoint after following its retry policy.</span></span> <span data-ttu-id="8698c-132">Om en annan felhantering slutpunkt har konfigurerats, anropar det Azure Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="8698c-132">If an alternate error-handling endpoint is configured, Azure Scheduler invokes it.</span></span> <span data-ttu-id="8698c-133">Med en annan slutpunkt är egna jobb hög tillgänglighet i hello framsidan för felet.</span><span class="sxs-lookup"><span data-stu-id="8698c-133">With an alternate endpoint, your own jobs are highly available in hello face of failure.</span></span>

<span data-ttu-id="8698c-134">Exempelvis i diagrammet nedan som hello följer Azure Scheduler dess försök princip toohit en New York-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="8698c-134">As an example, in hello diagram below, Azure Scheduler follows its retry policy toohit a New York web service.</span></span> <span data-ttu-id="8698c-135">När hello återförsök misslyckas, kontrollerar om det finns ett alternativ.</span><span class="sxs-lookup"><span data-stu-id="8698c-135">After hello retries fail, it checks if there's an alternate.</span></span> <span data-ttu-id="8698c-136">Sedan går vidare och startar begäranden toohello alternativ med hello samma försök princip.</span><span class="sxs-lookup"><span data-stu-id="8698c-136">It then goes ahead and starts making requests toohello alternate with hello same retry policy.</span></span>

![][2]

<span data-ttu-id="8698c-137">Observera att hello samma återförsöksprincip gäller tooboth hello ursprungliga åtgärd och hello alternativa felåtgärd.</span><span class="sxs-lookup"><span data-stu-id="8698c-137">Note that hello same retry policy applies tooboth hello original action and hello alternate error action.</span></span> <span data-ttu-id="8698c-138">Det är också möjligt toohave hello alternativa fel åtgärdens åtgärdstyp skilja sig från hello huvudåtgärden åtgärdstyp.</span><span class="sxs-lookup"><span data-stu-id="8698c-138">It’s also possible toohave hello alternate error action’s action type be different from hello main action’s action type.</span></span> <span data-ttu-id="8698c-139">Till exempel när hello huvudåtgärden kan anropa en HTTP-slutpunkt, kanske hello felåtgärd i stället queue storage, service bus-kö och service bus avsnittet åtgärd som har fel-loggning.</span><span class="sxs-lookup"><span data-stu-id="8698c-139">For example, while hello main action may be invoking an HTTP endpoint, hello error action may instead be a storage queue, service bus queue, or service bus topic action that does error-logging.</span></span>

<span data-ttu-id="8698c-140">hur tooconfigure en annan slutpunkt finns för toolearn[errorAction](scheduler-concepts-terms.md#action-and-erroraction).</span><span class="sxs-lookup"><span data-stu-id="8698c-140">toolearn how tooconfigure an alternate endpoint, refer too[errorAction](scheduler-concepts-terms.md#action-and-erroraction).</span></span>

## <a name="see-also"></a><span data-ttu-id="8698c-141">Se även</span><span class="sxs-lookup"><span data-stu-id="8698c-141">See Also</span></span>
 [<span data-ttu-id="8698c-142">Vad är Scheduler?</span><span class="sxs-lookup"><span data-stu-id="8698c-142">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="8698c-143">Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="8698c-143">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="8698c-144">Komma igång med Schemaläggaren i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8698c-144">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="8698c-145">Prenumerationer och fakturering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="8698c-145">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="8698c-146">Hur toobuild komplex schemaläggs och avancerade upprepning med Azure Schemaläggaren</span><span class="sxs-lookup"><span data-stu-id="8698c-146">How toobuild complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="8698c-147">Referens för REST-API:et för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="8698c-147">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="8698c-148">Referens för PowerShell-cmdlets för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="8698c-148">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="8698c-149">Gränser, standardinställningar och felkoder i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="8698c-149">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="8698c-150">Utgående autentisering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="8698c-150">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
