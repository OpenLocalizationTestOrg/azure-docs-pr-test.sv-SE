---
title: "Schemaläggaren hög tillgänglighet och tillförlitlighet"
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
ms.openlocfilehash: 7e7fe49de7814b6058468d630f8638720e5864f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scheduler-high-availability-and-reliability"></a><span data-ttu-id="97a60-103">Schemaläggaren hög tillgänglighet och tillförlitlighet</span><span class="sxs-lookup"><span data-stu-id="97a60-103">Scheduler High-Availability and Reliability</span></span>
## <a name="azure-scheduler-high-availability"></a><span data-ttu-id="97a60-104">Azure Scheduler hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="97a60-104">Azure Scheduler High-Availability</span></span>
<span data-ttu-id="97a60-105">Som en kärna Azure-plattformstjänsten Azure Schemaläggaren har hög tillgänglighet och funktioner både geo-redundant service-distributionen och geo regionala jobbet replikering.</span><span class="sxs-lookup"><span data-stu-id="97a60-105">As a core Azure platform service, Azure Scheduler is highly available and features both geo-redundant service deployment and geo-regional job replication.</span></span>

### <a name="geo-redundant-service-deployment"></a><span data-ttu-id="97a60-106">GEO-redundant tjänstdistribution</span><span class="sxs-lookup"><span data-stu-id="97a60-106">Geo-redundant service deployment</span></span>
<span data-ttu-id="97a60-107">Azure Schemaläggaren är tillgänglig via Användargränssnittet i nästan alla geografisk region som är i Azure idag.</span><span class="sxs-lookup"><span data-stu-id="97a60-107">Azure Scheduler is available via the UI in almost every geo region that's in Azure today.</span></span> <span data-ttu-id="97a60-108">Listan över regioner som Azure Schemaläggaren finns i är [som listas här](https://azure.microsoft.com/regions/#services).</span><span class="sxs-lookup"><span data-stu-id="97a60-108">The list of regions that Azure Scheduler is available in is [listed here](https://azure.microsoft.com/regions/#services).</span></span> <span data-ttu-id="97a60-109">Om ett datacenter i en värdbaserad region återges inte tillgänglig, är funktioner för redundans av Azure-schemaläggare så att tjänsten är tillgänglig från en annan datacenter.</span><span class="sxs-lookup"><span data-stu-id="97a60-109">If a data center in a hosted region is rendered unavailable, the failover capabilities of Azure Scheduler are such that the service is available from another data center.</span></span>

### <a name="geo-regional-job-replication"></a><span data-ttu-id="97a60-110">GEO-regionala jobbet replikering</span><span class="sxs-lookup"><span data-stu-id="97a60-110">Geo-regional job replication</span></span>
<span data-ttu-id="97a60-111">Är inte bara Azure Schemaläggaren frontend tillgänglig för av hanteringsbegäranden, men din egen jobbet är också georeplikerad.</span><span class="sxs-lookup"><span data-stu-id="97a60-111">Not only is the Azure Scheduler front-end available for management requests, but your own job is also geo-replicated.</span></span> <span data-ttu-id="97a60-112">När det finns ett avbrott i en region, Azure Scheduler växlas över och ser till att jobbet körs från en annan datacenter i parad geografiska region.</span><span class="sxs-lookup"><span data-stu-id="97a60-112">When there’s an outage in one region, Azure Scheduler fails over and ensures that the job is run from another data center in the paired geographic region.</span></span>

<span data-ttu-id="97a60-113">Till exempel om du har skapat ett jobb i södra centrala USA, replikerar Azure Scheduler automatiskt det jobbet i norra centrala USA.</span><span class="sxs-lookup"><span data-stu-id="97a60-113">For example, if you’ve created a job in South Central US, Azure Scheduler automatically replicates that job in North Central US.</span></span> <span data-ttu-id="97a60-114">När det finns ett fel i södra centrala USA, säkerställer Azure Schemaläggaren att jobbet körs från norra centrala USA.</span><span class="sxs-lookup"><span data-stu-id="97a60-114">When there’s a failure in South Central US, Azure Scheduler ensures that the job is run from North Central US.</span></span> 

![][1]

<span data-ttu-id="97a60-115">Därför garanterar Azure Scheduler att dina data håller sig inom samma bredare geografiska region om ett fel uppstår på Azure.</span><span class="sxs-lookup"><span data-stu-id="97a60-115">As a result, Azure Scheduler ensures that your data stays within the same broader geographic region in case of an Azure failure.</span></span> <span data-ttu-id="97a60-116">Därför kan du inte behöver duplicera jobbet bara att lägga till hög tillgänglighet – Azure Scheduler innehåller automatiskt funktioner för hög tillgänglighet för dina jobb.</span><span class="sxs-lookup"><span data-stu-id="97a60-116">As a result, you need not duplicate your job just to add high availability – Azure Scheduler automatically provides high-availability capabilities for your jobs.</span></span>

## <a name="azure-scheduler-reliability"></a><span data-ttu-id="97a60-117">Azure Scheduler tillförlitlighet</span><span class="sxs-lookup"><span data-stu-id="97a60-117">Azure Scheduler Reliability</span></span>
<span data-ttu-id="97a60-118">Azure Scheduler garanterar sin egen hög tillgänglighet och använder en annan metod till användarskapade jobb.</span><span class="sxs-lookup"><span data-stu-id="97a60-118">Azure Scheduler guarantees its own high-availability and takes a different approach to user-created jobs.</span></span> <span data-ttu-id="97a60-119">Jobbet kan till exempel anropa en HTTP-slutpunkt som inte är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="97a60-119">For example, your job may invoke an HTTP endpoint that’s unavailable.</span></span> <span data-ttu-id="97a60-120">Azure Schemaläggaren försöker dock köras ditt jobb, genom att ge dig alternativ att åtgärda felet.</span><span class="sxs-lookup"><span data-stu-id="97a60-120">Azure Scheduler nonetheless tries to execute your job successfully, by giving you alternative options to deal with failure.</span></span> <span data-ttu-id="97a60-121">Azure Scheduler gör detta på två sätt:</span><span class="sxs-lookup"><span data-stu-id="97a60-121">Azure Scheduler does this in two ways:</span></span>

### <a name="configurable-retry-policy-via-retrypolicy"></a><span data-ttu-id="97a60-122">Konfigurerbar försök princip via ”retryPolicy”</span><span class="sxs-lookup"><span data-stu-id="97a60-122">Configurable Retry Policy via “retryPolicy”</span></span>
<span data-ttu-id="97a60-123">Azure Schemaläggaren kan du konfigurera en princip för återförsök.</span><span class="sxs-lookup"><span data-stu-id="97a60-123">Azure Scheduler allows you to configure a retry policy.</span></span> <span data-ttu-id="97a60-124">Som standard om ett jobb misslyckas försöker Schemaläggaren jobbet igen fyra gånger, med 30 sekunders intervall.</span><span class="sxs-lookup"><span data-stu-id="97a60-124">By default, if a job fails, Scheduler tries the job again four more times, at 30-second intervals.</span></span> <span data-ttu-id="97a60-125">Du kan konfigurera principen så att mer aggressivt (till exempel tio gånger med 30 sekunders mellanrum) försök igen eller glesare (till exempel två gånger med daglig intervall.)</span><span class="sxs-lookup"><span data-stu-id="97a60-125">You may re-configure this retry policy to be more aggressive (for example, ten times, at 30-second intervals) or looser (for example, two times, at daily intervals.)</span></span>

<span data-ttu-id="97a60-126">Som ett exempel på när det kan hjälpa dig, kan du skapa ett jobb som körs en gång i veckan och anropar en HTTP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="97a60-126">As an example of when this may help, you may create a job that runs once a week and invokes an HTTP endpoint.</span></span> <span data-ttu-id="97a60-127">Om HTTP-slutpunkten avser några timmar när jobbet körs, kan du inte vill vänta en mer vecka för jobbet ska köras igen eftersom även standardprincipen för nytt försök kommer att misslyckas.</span><span class="sxs-lookup"><span data-stu-id="97a60-127">If the HTTP endpoint is down for a few hours when your job runs, you may not want to wait one more week for the job to run again since even the default retry policy will fail.</span></span> <span data-ttu-id="97a60-128">I sådana fall kan du konfigurera om principen standard försök att försöka igen var tredje timme (till exempel) i stället för med 30 sekunders mellanrum.</span><span class="sxs-lookup"><span data-stu-id="97a60-128">In such cases, you may reconfigure the standard retry policy to retry every three hours (for example) instead of every 30 seconds.</span></span>

<span data-ttu-id="97a60-129">Om du vill veta hur du konfigurerar en återförsöksprincip avser [retryPolicy](scheduler-concepts-terms.md#retrypolicy).</span><span class="sxs-lookup"><span data-stu-id="97a60-129">To learn how to configure a retry policy, refer to [retryPolicy](scheduler-concepts-terms.md#retrypolicy).</span></span>

### <a name="alternate-endpoint-configurability-via-erroraction"></a><span data-ttu-id="97a60-130">Den alternativa slutpunkten konfigurationsmöjligheter via ”errorAction”</span><span class="sxs-lookup"><span data-stu-id="97a60-130">Alternate Endpoint Configurability via “errorAction”</span></span>
<span data-ttu-id="97a60-131">Mål-slutpunkten för din Azure Scheduler-jobbet är inte kan nås, faller Azure Scheduler tillbaka till den alternativa slutpunkten felhantering när du har följt av dess återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="97a60-131">If the target endpoint for your Azure Scheduler job remains unreachable, Azure Scheduler falls back to the alternate error-handling endpoint after following its retry policy.</span></span> <span data-ttu-id="97a60-132">Om en annan felhantering slutpunkt har konfigurerats, anropar det Azure Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="97a60-132">If an alternate error-handling endpoint is configured, Azure Scheduler invokes it.</span></span> <span data-ttu-id="97a60-133">Med en annan slutpunkt är egna jobb hög tillgänglighet i händelse av fel.</span><span class="sxs-lookup"><span data-stu-id="97a60-133">With an alternate endpoint, your own jobs are highly available in the face of failure.</span></span>

<span data-ttu-id="97a60-134">Exempelvis i diagrammet nedan, följer Azure Scheduler policyn försök igen om du vill nådde en New York-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="97a60-134">As an example, in the diagram below, Azure Scheduler follows its retry policy to hit a New York web service.</span></span> <span data-ttu-id="97a60-135">När de nya försök misslyckas kontrollerar om det finns ett alternativ.</span><span class="sxs-lookup"><span data-stu-id="97a60-135">After the retries fail, it checks if there's an alternate.</span></span> <span data-ttu-id="97a60-136">Sedan går vidare och startar gör begäranden till alternativa med samma återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="97a60-136">It then goes ahead and starts making requests to the alternate with the same retry policy.</span></span>

![][2]

<span data-ttu-id="97a60-137">Observera att samma återförsöksprincip gäller både den ursprungliga åtgärden och den alternativa felåtgärden.</span><span class="sxs-lookup"><span data-stu-id="97a60-137">Note that the same retry policy applies to both the original action and the alternate error action.</span></span> <span data-ttu-id="97a60-138">Du kan också har den alternativa felåtgärd åtgärdstyp skilja sig från den huvudsakliga åtgärden åtgärdstyp.</span><span class="sxs-lookup"><span data-stu-id="97a60-138">It’s also possible to have the alternate error action’s action type be different from the main action’s action type.</span></span> <span data-ttu-id="97a60-139">Till exempel när huvudåtgärden kan anropa en HTTP-slutpunkt, kanske felåtgärden i stället queue storage, service bus-kö och service bus avsnittet åtgärd som har fel-loggning.</span><span class="sxs-lookup"><span data-stu-id="97a60-139">For example, while the main action may be invoking an HTTP endpoint, the error action may instead be a storage queue, service bus queue, or service bus topic action that does error-logging.</span></span>

<span data-ttu-id="97a60-140">Information om hur du konfigurerar en annan slutpunkt, referera till [errorAction](scheduler-concepts-terms.md#action-and-erroraction).</span><span class="sxs-lookup"><span data-stu-id="97a60-140">To learn how to configure an alternate endpoint, refer to [errorAction](scheduler-concepts-terms.md#action-and-erroraction).</span></span>

## <a name="see-also"></a><span data-ttu-id="97a60-141">Se även</span><span class="sxs-lookup"><span data-stu-id="97a60-141">See Also</span></span>
 [<span data-ttu-id="97a60-142">Vad är Scheduler?</span><span class="sxs-lookup"><span data-stu-id="97a60-142">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="97a60-143">Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97a60-143">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="97a60-144">Komma igång med Scheduler på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="97a60-144">Get started using Scheduler in the Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="97a60-145">Prenumerationer och fakturering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97a60-145">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="97a60-146">Skapa komplexa scheman och avancerad upprepning med Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97a60-146">How to build complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="97a60-147">Referens för REST-API:et för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97a60-147">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="97a60-148">Referens för PowerShell-cmdlets för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97a60-148">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="97a60-149">Gränser, standardinställningar och felkoder i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97a60-149">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="97a60-150">Utgående autentisering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="97a60-150">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
