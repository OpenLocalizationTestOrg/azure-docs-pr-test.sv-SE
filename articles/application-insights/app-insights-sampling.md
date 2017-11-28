---
title: aaaTelemetry provtagning i Azure Application Insights | Microsoft Docs
description: Hur tookeep hello telemetrivolym under kontroll.
services: application-insights
documentationcenter: windows
author: vgorbenko
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bwren
ms.openlocfilehash: e19c350d0a5f16736c100322a9922fcfbf5d7b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sampling-in-application-insights"></a><span data-ttu-id="3059a-103">Sampling i Application Insights</span><span class="sxs-lookup"><span data-stu-id="3059a-103">Sampling in Application Insights</span></span>


<span data-ttu-id="3059a-104">Sampling är en funktion i [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3059a-104">Sampling is a feature in [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="3059a-105">Det är hello rekommenderade sätt tooreduce telemetri trafik och lagring, samtidigt som en statistiskt korrekt analys av programdata.</span><span class="sxs-lookup"><span data-stu-id="3059a-105">It is hello recommended way tooreduce telemetry traffic and storage, while preserving  a statistically correct analysis of application data.</span></span> <span data-ttu-id="3059a-106">hello filter markerar objekt som är relaterade så att du kan bläddra mellan objekten när du gör diagnostiska undersökningar.</span><span class="sxs-lookup"><span data-stu-id="3059a-106">hello filter selects items that are related, so that you can navigate between items when you are doing diagnostic investigations.</span></span>
<span data-ttu-id="3059a-107">När mått Antal presenteras tooyou hello-portalen, de renormalized tootake hänsyn hello provtagning, toominimize någon effekten på hello statistik.</span><span class="sxs-lookup"><span data-stu-id="3059a-107">When metric counts are presented tooyou in hello portal, they are renormalized tootake account of hello sampling, toominimize any effect on hello statistics.</span></span>

<span data-ttu-id="3059a-108">Samplingsfrekvensen minskar kostnaderna för trafik och data och hjälper dig att undvika begränsning.</span><span class="sxs-lookup"><span data-stu-id="3059a-108">Sampling reduces traffic and data costs, and helps you avoid throttling.</span></span>

## <a name="in-brief"></a><span data-ttu-id="3059a-109">Kort sagt:</span><span class="sxs-lookup"><span data-stu-id="3059a-109">In brief:</span></span>
* <span data-ttu-id="3059a-110">Sampling behåller 1 i * n * registrerar och ignorerar hello rest.</span><span class="sxs-lookup"><span data-stu-id="3059a-110">Sampling retains 1 in *n* records and discards hello rest.</span></span> <span data-ttu-id="3059a-111">Det kan till exempel lagra 1: 5 händelser, en samplingsfrekvensen 20%.</span><span class="sxs-lookup"><span data-stu-id="3059a-111">For example, it might retain 1 in 5 events, a sampling rate of 20%.</span></span> 
* <span data-ttu-id="3059a-112">Sampling sker automatiskt om programmet skickar mycket telemetri i ASP.NET web server apps.</span><span class="sxs-lookup"><span data-stu-id="3059a-112">Sampling happens automatically if your application sends a lot of telemetry, in ASP.NET web server apps.</span></span>
* <span data-ttu-id="3059a-113">Du kan också ange provtagning manuellt, antingen i hello Företagsportalen på hello priser för sidan. eller i hello SDK för ASP.NET i hello .config-fil, minska tooalso hello nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="3059a-113">You can also set sampling manually, either in hello portal on hello pricing page; or in hello ASP.NET SDK in hello .config file, tooalso reduce hello network traffic.</span></span>
* <span data-ttu-id="3059a-114">Om du loggar anpassade händelser och du vill toomake till att en uppsättning händelser är antingen bevaras eller tas bort tillsammans, se till att de har hello samma åtgärds-ID-värde.</span><span class="sxs-lookup"><span data-stu-id="3059a-114">If you log custom events and you want toomake sure that a set of events is either retained or discarded together, make sure that they have hello same OperationId value.</span></span>
* <span data-ttu-id="3059a-115">hello provtagning divisor * n * rapporteras i varje post i hello egenskapen `itemCount`, som i sökningen visas under hello eget namn ”begäran antal” eller ”händelseantal”.</span><span class="sxs-lookup"><span data-stu-id="3059a-115">hello sampling divisor *n* is reported in each record in hello property `itemCount`, which in Search appears under hello friendly name "request count" or "event count".</span></span> <span data-ttu-id="3059a-116">När provtagning inte är i drift, `itemCount==1`.</span><span class="sxs-lookup"><span data-stu-id="3059a-116">When sampling is not in operation, `itemCount==1`.</span></span>
* <span data-ttu-id="3059a-117">Om du skriver Analytics-frågor, bör du [ta hänsyn till provtagning](app-insights-analytics-tour.md#counting-sampled-data).</span><span class="sxs-lookup"><span data-stu-id="3059a-117">If you write Analytics queries, you should [take account of sampling](app-insights-analytics-tour.md#counting-sampled-data).</span></span> <span data-ttu-id="3059a-118">I synnerhet i stället för bara räknar poster, du bör använda `summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="3059a-118">In particular, instead of simply counting records, you should use `summarize sum(itemCount)`.</span></span>

## <a name="types-of-sampling"></a><span data-ttu-id="3059a-119">Typer av provtagning</span><span class="sxs-lookup"><span data-stu-id="3059a-119">Types of sampling</span></span>
<span data-ttu-id="3059a-120">Det finns tre alternativ provtagningsmetoder:</span><span class="sxs-lookup"><span data-stu-id="3059a-120">There are three alternative sampling methods:</span></span>

* <span data-ttu-id="3059a-121">**Anpassningsbar provtagning** justeras automatiskt hello telemetrivolym skickas från hello SDK i din ASP.NET-app.</span><span class="sxs-lookup"><span data-stu-id="3059a-121">**Adaptive sampling** automatically adjusts hello volume of telemetry sent from hello SDK in your ASP.NET app.</span></span> <span data-ttu-id="3059a-122">Som standard från SDK v 2.0.0-beta3.</span><span class="sxs-lookup"><span data-stu-id="3059a-122">Default from SDK v 2.0.0-beta3.</span></span> <span data-ttu-id="3059a-123">För närvarande tillgänglig för ASP.NET serversidan telemetri.</span><span class="sxs-lookup"><span data-stu-id="3059a-123">Currently available for ASP.NET server-side telemetry only.</span></span> 
* <span data-ttu-id="3059a-124">**Fast räntesats provtagning** minskar hello telemetrivolym skickas från både din ASP.NET-server och från användarnas webbläsare.</span><span class="sxs-lookup"><span data-stu-id="3059a-124">**Fixed-rate sampling** reduces hello volume of telemetry sent from both your ASP.NET server and from your users' browsers.</span></span> <span data-ttu-id="3059a-125">Du kan ange hello hastighet.</span><span class="sxs-lookup"><span data-stu-id="3059a-125">You set hello rate.</span></span> <span data-ttu-id="3059a-126">hello-klienten och servern ska synkronisera sina provtagning så att, i sökningen som du kan navigera mellan relaterade sidvisningar och förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="3059a-126">hello client and server will synchronize their sampling so that, in Search, you can navigate between related page views and requests.</span></span>
* <span data-ttu-id="3059a-127">**Införandet provtagning** fungerar i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3059a-127">**Ingestion sampling** works in hello Azure portal.</span></span> <span data-ttu-id="3059a-128">Den tar bort vissa av hello telemetri som tas emot från din app med en hastighet som du anger.</span><span class="sxs-lookup"><span data-stu-id="3059a-128">It discards some of hello telemetry that arrives from your app, at a rate that you set.</span></span> <span data-ttu-id="3059a-129">Det minskar inte telemetri trafik, men hjälper dig att hålla inom den månatliga kvoten.</span><span class="sxs-lookup"><span data-stu-id="3059a-129">It doesn't reduce telemetry traffic, but helps you keep within your monthly quota.</span></span> <span data-ttu-id="3059a-130">hello stor nytta av införandet provtagning är att du kan ange den utan att omdistribuera din app och det fungerar enhetligt för alla servrar och klienter.</span><span class="sxs-lookup"><span data-stu-id="3059a-130">hello big advantage of ingestion sampling is that you can set it without redeploying your app, and it works uniformly for all servers and clients.</span></span> 

<span data-ttu-id="3059a-131">Om Adaptiv eller fast hastighet provtagning är i drift, har införandet provtagning inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="3059a-131">If Adaptive or Fixed rate sampling are in operation, Ingestion sampling is disabled.</span></span>

## <a name="ingestion-sampling"></a><span data-ttu-id="3059a-132">Införandet provtagning</span><span class="sxs-lookup"><span data-stu-id="3059a-132">Ingestion sampling</span></span>
<span data-ttu-id="3059a-133">Det här formuläret för provtagning fungerar på hello punkt där hello telemetri från webbservern, webbläsare och enheter når hello Application Insights tjänstslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="3059a-133">This form of sampling operates at hello point where hello telemetry from your web server, browsers, and devices reaches hello Application Insights service endpoint.</span></span> <span data-ttu-id="3059a-134">Även om det inte minska hello telemetri trafik som skickas från din app, det minskar hello bearbetas och lagras (och debiteras för) av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="3059a-134">Although it doesn't reduce hello telemetry traffic sent from your app, it does reduce hello amount processed and retained (and charged for) by Application Insights.</span></span>

<span data-ttu-id="3059a-135">Använd den här typen av samplingsfrekvensen om din app ofta färdas över månatliga kvoten och du inte har hello möjlighet att använda antingen hello SDK-baserade typer av provtagning.</span><span class="sxs-lookup"><span data-stu-id="3059a-135">Use this type of sampling if your app often goes over its monthly quota and you don't have hello option of using either of hello SDK-based types of sampling.</span></span> 

<span data-ttu-id="3059a-136">Ange hello samplingsfrekvensen i hello kvoter och priser bladet:</span><span class="sxs-lookup"><span data-stu-id="3059a-136">Set hello sampling rate in hello Quotas and Pricing blade:</span></span>

![Klicka på inställningar, kvot, prover, hello program Översikt bladet och sedan välja en samplingsfrekvensen och klicka på Uppdatera.](./media/app-insights-sampling/04.png)

<span data-ttu-id="3059a-138">Precis som andra typer av provtagning behåller hello algoritmen relaterad telemetri objekt.</span><span class="sxs-lookup"><span data-stu-id="3059a-138">Like other types of sampling, hello algorithm retains related telemetry items.</span></span> <span data-ttu-id="3059a-139">Till exempel, när du undersöks hello telemetri i sökning, du ska kunna toofind hello begäran relaterade tooa särskilda undantag.</span><span class="sxs-lookup"><span data-stu-id="3059a-139">For example, when you're inspecting hello telemetry in Search, you'll be able toofind hello request related tooa particular exception.</span></span> <span data-ttu-id="3059a-140">Måttet räknar, till exempel förfrågningar och undantag hastighet behålls på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="3059a-140">Metric counts such as request rate and exception rate are correctly retained.</span></span>

<span data-ttu-id="3059a-141">Datapunkter som ignoreras av provtagning är inte tillgängliga i alla Application Insights-funktioner som [löpande Export](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="3059a-141">Data points that are discarded by sampling are not available in any Application Insights feature such as [Continuous Export](app-insights-export-telemetry.md).</span></span>

<span data-ttu-id="3059a-142">Införandet provtagning fungerar inte när SDK-baserade anpassningsbara eller fasta hastighet provtagning är i drift.</span><span class="sxs-lookup"><span data-stu-id="3059a-142">Ingestion sampling doesn't operate while SDK-based adaptive or fixed-rate sampling is in operation.</span></span> <span data-ttu-id="3059a-143">Om hello samplingsfrekvensen på hello SDK är mindre än 100%, ignoreras hello införandet samplingsfrekvensen som du ställer in.</span><span class="sxs-lookup"><span data-stu-id="3059a-143">If hello sampling rate at hello SDK is less than 100%, then hello ingestion sampling rate that you set is ignored.</span></span>

> [!WARNING]
> <span data-ttu-id="3059a-144">hello-värde som visas på panelen hello anger hello-värdet som du anger för införandet provtagning.</span><span class="sxs-lookup"><span data-stu-id="3059a-144">hello value shown on hello tile indicates hello value that you set for ingestion sampling.</span></span> <span data-ttu-id="3059a-145">Den representera inte hello faktiska samplingsfrekvensen om SDK provtagning är i drift.</span><span class="sxs-lookup"><span data-stu-id="3059a-145">It doesn't represent hello actual sampling rate if SDK sampling is in operation.</span></span>
> 
> 

## <a name="adaptive-sampling-at-your-web-server"></a><span data-ttu-id="3059a-146">Anpassningsbar provtagning på din webbserver</span><span class="sxs-lookup"><span data-stu-id="3059a-146">Adaptive sampling at your web server</span></span>
<span data-ttu-id="3059a-147">Anpassningsbar provtagning är tillgänglig för hello Application Insights SDK för ASP.NET v 2.0.0-beta3 och senare och är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="3059a-147">Adaptive sampling is available for hello Application Insights SDK for ASP.NET v 2.0.0-beta3 and later, and is enabled by default.</span></span> 

<span data-ttu-id="3059a-148">Anpassningsbar provtagning påverkar hello telemetrivolym skickas från din app toohello för web server Application Insights-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3059a-148">Adaptive sampling affects hello volume of telemetry sent from your web server app toohello Application Insights service.</span></span> <span data-ttu-id="3059a-149">hello volym justeras automatiskt tookeep inom en angiven maximala mängden trafik.</span><span class="sxs-lookup"><span data-stu-id="3059a-149">hello volume is adjusted automatically tookeep within a specified maximum rate of traffic.</span></span>

<span data-ttu-id="3059a-150">Det fungerar inte på låg mängder telemetri, så en app i felsökning eller en webbplats med låg belastning påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="3059a-150">It doesn't operate at low volumes of telemetry, so an app in debugging or a website with low usage won't be affected.</span></span>

<span data-ttu-id="3059a-151">tooachieve hello målvolymen, vissa hello telemetri genereras ignoreras.</span><span class="sxs-lookup"><span data-stu-id="3059a-151">tooachieve hello target volume, some of hello telemetry generated is discarded.</span></span> <span data-ttu-id="3059a-152">Men precis som andra typer av provtagning hello algoritmen behåller relaterad telemetri objekt.</span><span class="sxs-lookup"><span data-stu-id="3059a-152">But like other types of sampling, hello algorithm retains related telemetry items.</span></span> <span data-ttu-id="3059a-153">Till exempel, när du undersöks hello telemetri i sökning, du ska kunna toofind hello begäran relaterade tooa särskilda undantag.</span><span class="sxs-lookup"><span data-stu-id="3059a-153">For example, when you're inspecting hello telemetry in Search, you'll be able toofind hello request related tooa particular exception.</span></span> 

<span data-ttu-id="3059a-154">Måttet räknas som förfrågningar och undantag hastighet är justerade toocompensate för hello samplingsfrekvens, så att de visar ungefär rätt värden i måttet Explorer.</span><span class="sxs-lookup"><span data-stu-id="3059a-154">Metric counts such as request rate and exception rate are adjusted toocompensate for hello sampling rate, so that they show approximately correct values in Metric Explorer.</span></span>

<span data-ttu-id="3059a-155">**Uppdatera ditt projekt NuGet** paket toohello senaste *förhandsversionen* version av Application Insights: Högerklicka på hello projektet i Solution Explorer, välja hantera NuGet-paket, kontrollera **inkludera förhandsversionen** och Sök efter Microsoft.ApplicationInsights.Web.</span><span class="sxs-lookup"><span data-stu-id="3059a-155">**Update your project's NuGet** packages toohello latest *pre-release* version of Application Insights: Right-click hello project in Solution Explorer, choose Manage NuGet Packages, check **Include prerelease** and search for Microsoft.ApplicationInsights.Web.</span></span> 

<span data-ttu-id="3059a-156">I [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), kan du justera flera parametrar i hello `AdaptiveSamplingTelemetryProcessor` nod.</span><span class="sxs-lookup"><span data-stu-id="3059a-156">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), you can adjust several parameters in hello `AdaptiveSamplingTelemetryProcessor` node.</span></span> <span data-ttu-id="3059a-157">hello siffror som visas är hello standardvärden:</span><span class="sxs-lookup"><span data-stu-id="3059a-157">hello figures shown are hello default values:</span></span>

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    <span data-ttu-id="3059a-158">hello mål hastigheten som hello anpassningsbar algoritmen syftar för **på varje server-värd**.</span><span class="sxs-lookup"><span data-stu-id="3059a-158">hello target rate that hello adaptive algorithm aims for **on each server host**.</span></span> <span data-ttu-id="3059a-159">Om ditt webbprogram kör på flera värdar, minska värdet så tooremain inom dina mål mängden trafik på hello Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="3059a-159">If your web app runs on many hosts, reduce this value so as tooremain within your target rate of traffic at hello Application Insights portal.</span></span>
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    <span data-ttu-id="3059a-160">hello intervall på vilka hello aktuellt antal telemetri görs en ny utvärdering.</span><span class="sxs-lookup"><span data-stu-id="3059a-160">hello interval at which hello current rate of telemetry is re-evaluated.</span></span> <span data-ttu-id="3059a-161">Utvärderingen utförs som en glidande medelvärde.</span><span class="sxs-lookup"><span data-stu-id="3059a-161">Evaluation is performed as a moving average.</span></span> <span data-ttu-id="3059a-162">Du kanske vill tooshorten intervallet om din telemetri är ansvariga toosudden belastning.</span><span class="sxs-lookup"><span data-stu-id="3059a-162">You might want tooshorten this interval if your telemetry is liable toosudden bursts.</span></span>
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    <span data-ttu-id="3059a-163">När provtagning procentandel ändras, hur snart efter är tillåtna vi toolower provtagning procentandel igen toocapture mindre data.</span><span class="sxs-lookup"><span data-stu-id="3059a-163">When sampling percentage value changes, how soon after are we allowed toolower sampling percentage again toocapture less data.</span></span>
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    <span data-ttu-id="3059a-164">När provtagning procentandel ändras, hur snart när vi får tooincrease provtagning procentandel igen toocapture mer data.</span><span class="sxs-lookup"><span data-stu-id="3059a-164">When sampling percentage value changes, how soon after are we allowed tooincrease sampling percentage again toocapture more data.</span></span>
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    <span data-ttu-id="3059a-165">Eftersom provtagning procentandel varierar, vad är hello minimivärdet vi är tillåtna tooset.</span><span class="sxs-lookup"><span data-stu-id="3059a-165">As sampling percentage varies, what is hello minimum value we're allowed tooset.</span></span>
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    <span data-ttu-id="3059a-166">Eftersom provtagning procentandel varierar, vad är hello maxvärdet vi är tillåtna tooset.</span><span class="sxs-lookup"><span data-stu-id="3059a-166">As sampling percentage varies, what is hello maximum value we're allowed tooset.</span></span>
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    <span data-ttu-id="3059a-167">Hello beräkningen av hello glidande medelvärde, tilldelas hello vikt toohello senaste värde.</span><span class="sxs-lookup"><span data-stu-id="3059a-167">In hello calculation of hello moving average, hello weight assigned toohello most recent value.</span></span> <span data-ttu-id="3059a-168">Använd ett värde lika tooor som är mindre än 1.</span><span class="sxs-lookup"><span data-stu-id="3059a-168">Use a value equal tooor less than 1.</span></span> <span data-ttu-id="3059a-169">Lägre värden ändrar hello algoritmen reagera mindre toosudden.</span><span class="sxs-lookup"><span data-stu-id="3059a-169">Smaller values make hello algorithm less reactive toosudden changes.</span></span>
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    <span data-ttu-id="3059a-170">hello-värdet som tilldelas när hello app har startats.</span><span class="sxs-lookup"><span data-stu-id="3059a-170">hello value assigned when hello app has just started.</span></span> <span data-ttu-id="3059a-171">Inte minska detta när du felsöker.</span><span class="sxs-lookup"><span data-stu-id="3059a-171">Don't reduce this while you're debugging.</span></span> 

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    <span data-ttu-id="3059a-172">En semikolonavgränsad lista över typer som du inte vill att sampla toobe.</span><span class="sxs-lookup"><span data-stu-id="3059a-172">A semi-colon delimited list of types that you do not want toobe sampled.</span></span> <span data-ttu-id="3059a-173">Identifiera typer är: beroende, händelse, undantag, PageView, begäran, spårning.</span><span class="sxs-lookup"><span data-stu-id="3059a-173">Recognized types are: Dependency, Event, Exception, PageView, Request, Trace.</span></span> <span data-ttu-id="3059a-174">Alla instanser av hello angetts typer överförs. hello-typer som inte har angetts samplas.</span><span class="sxs-lookup"><span data-stu-id="3059a-174">All instances of hello specified types are transmitted; hello types that are not specified are sampled.</span></span>

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    <span data-ttu-id="3059a-175">En semikolonavgränsad lista över typer som du vill toobe provtagning.</span><span class="sxs-lookup"><span data-stu-id="3059a-175">A semi-colon delimited list of types that you want toobe sampled.</span></span> <span data-ttu-id="3059a-176">Identifiera typer är: beroende, händelse, undantag, PageView, begäran, spårning.</span><span class="sxs-lookup"><span data-stu-id="3059a-176">Recognized types are: Dependency, Event, Exception, PageView, Request, Trace.</span></span> <span data-ttu-id="3059a-177">hello angetts typer samplas; alla instanser av hello andra typer kommer alltid att överföras.</span><span class="sxs-lookup"><span data-stu-id="3059a-177">hello specified types are sampled; all instances of hello other types will always be transmitted.</span></span>


<span data-ttu-id="3059a-178">**tooswitch av** anpassningsbar provtagning, ta bort hello AdaptiveSamplingTelemetryProcessor nod från applicationinsights-config.</span><span class="sxs-lookup"><span data-stu-id="3059a-178">**tooswitch off** adaptive sampling, remove hello AdaptiveSamplingTelemetryProcessor node from applicationinsights-config.</span></span>

### <a name="alternative-configure-adaptive-sampling-in-code"></a><span data-ttu-id="3059a-179">Alternativ: Konfigurera anpassningsbar provtagning i koden</span><span class="sxs-lookup"><span data-stu-id="3059a-179">Alternative: configure adaptive sampling in code</span></span>
<span data-ttu-id="3059a-180">Du kan använda koden i stället för att justera samplingsfrekvensen i hello .config-fil.</span><span class="sxs-lookup"><span data-stu-id="3059a-180">Instead of adjusting sampling in hello .config file, you can use code.</span></span> <span data-ttu-id="3059a-181">Detta ger dig toospecify Återanropsfunktionen som anropas när hello samplingsfrekvensen är ny utvärdering.</span><span class="sxs-lookup"><span data-stu-id="3059a-181">This allows you toospecify a callback function that is invoked whenever hello sampling rate is re-evaluated.</span></span> <span data-ttu-id="3059a-182">Du kan använda detta, exempelvis toofind reda på vilka samplingsfrekvensen används.</span><span class="sxs-lookup"><span data-stu-id="3059a-182">You could use this, for example, toofind out what sampling rate is being used.</span></span>

<span data-ttu-id="3059a-183">Ta bort hello `AdaptiveSamplingTelemetryProcessor` noden från hello .config-fil.</span><span class="sxs-lookup"><span data-stu-id="3059a-183">Remove hello `AdaptiveSamplingTelemetryProcessor` node from hello .config file.</span></span>

<span data-ttu-id="3059a-184">*C#*</span><span class="sxs-lookup"><span data-stu-id="3059a-184">*C#*</span></span>

```C#

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var adaptiveSamplingSettings = new SamplingPercentageEstimatorSettings();

    // Optional: here you can adjust hello settings from their defaults.

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;

    builder.UseAdaptiveSampling(
         adaptiveSamplingSettings,

        // Callback on rate re-evaluation:
        (double afterSamplingTelemetryItemRatePerSecond,
         double currentSamplingPercentage,
         double newSamplingPercentage,
         bool isSamplingPercentageChanged,
         SamplingPercentageEstimatorSettings s
        ) =>
        {
          if (isSamplingPercentageChanged)
          {
             // Report hello sampling rate.
             telemetryClient.TrackMetric("samplingPercentage", newSamplingPercentage);
          }
      });

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

<span data-ttu-id="3059a-185">([Lär dig mer om telemetri processorer](app-insights-api-filtering-sampling.md#filtering).)</span><span class="sxs-lookup"><span data-stu-id="3059a-185">([Learn about telemetry processors](app-insights-api-filtering-sampling.md#filtering).)</span></span>

<a name="other-web-pages"></a>

## <a name="sampling-for-web-pages-with-javascript"></a><span data-ttu-id="3059a-186">Samplingsfrekvensen för webbsidor med JavaScript</span><span class="sxs-lookup"><span data-stu-id="3059a-186">Sampling for web pages with JavaScript</span></span>
<span data-ttu-id="3059a-187">Du kan konfigurera webbsidor för fast räntesats provtagning från alla servrar.</span><span class="sxs-lookup"><span data-stu-id="3059a-187">You can configure web pages for fixed-rate sampling from any server.</span></span> 

<span data-ttu-id="3059a-188">När du [konfigurera hello webbsidor för Application Insights](app-insights-javascript.md), ändra hello fragment som du får från hello Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="3059a-188">When you [configure hello web pages for Application Insights](app-insights-javascript.md), modify hello snippet that you get from hello Application Insights portal.</span></span> <span data-ttu-id="3059a-189">(I ASP.NET appar hello fragment vanligtvis först i _Layout.cshtml.)  Infoga en rad som `samplingPercentage: 10,` innan hello instrumentation nyckel:</span><span class="sxs-lookup"><span data-stu-id="3059a-189">(In ASP.NET apps, hello snippet typically goes in _Layout.cshtml.)  Insert a line like `samplingPercentage: 10,` before hello instrumentation key:</span></span>

    <script>
    var appInsights= ... 
    }({ 


    // Value must be 100/N where N is an integer.
    // Valid examples: 50, 25, 20, 10, 5, 1, 0.1, ...
    samplingPercentage: 10, 

    instrumentationKey:...
    }); 

    window.appInsights=appInsights; 
    appInsights.trackPageView(); 
    </script> 

<span data-ttu-id="3059a-190">Välj ett procentvärde som är nära too100/N, där N är ett heltal för hello samplingsprocenten.</span><span class="sxs-lookup"><span data-stu-id="3059a-190">For hello sampling percentage, choose a percentage that is close too100/N where N is an integer.</span></span>  <span data-ttu-id="3059a-191">Samlar för närvarande stöd inte för andra värden.</span><span class="sxs-lookup"><span data-stu-id="3059a-191">Currently sampling doesn't support other values.</span></span>

<span data-ttu-id="3059a-192">Om du även aktivera fast räntesats provtagning i hello server synkroniserar hello klienter och servrar så att, i sökningen som du kan navigera mellan relaterade sidvisningar och förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="3059a-192">If you also enable fixed-rate sampling at hello server, hello clients and server will synchronize so that, in Search, you can  navigate between related page views and requests.</span></span>

## <a name="fixed-rate-sampling-for-aspnet-web-sites"></a><span data-ttu-id="3059a-193">Fast räntesats samplingsfrekvensen för ASP.NET-webbplatser</span><span class="sxs-lookup"><span data-stu-id="3059a-193">Fixed-rate sampling for ASP.NET web sites</span></span>
<span data-ttu-id="3059a-194">Fasta samplingsfrekvensen minskar hello trafik som skickas från webbservern och webbläsare.</span><span class="sxs-lookup"><span data-stu-id="3059a-194">Fixed rate sampling reduces hello traffic sent from your web server and web browsers.</span></span> <span data-ttu-id="3059a-195">Till skillnad från anpassningsbar samplingsfrekvensen minskar den telemetri till en fast kostnad som du valt.</span><span class="sxs-lookup"><span data-stu-id="3059a-195">Unlike adaptive sampling, it reduces telemetry at a fixed rate decided by you.</span></span> <span data-ttu-id="3059a-196">Det synkroniserar även hello klienten och servern provtagning så att relaterade objekt bevaras – till exempel så att om du tittar på vyn sida i sökningen kan du hitta relaterade begäran.</span><span class="sxs-lookup"><span data-stu-id="3059a-196">It also synchronizes hello client and server sampling so that related items are retained - for example, so that if you look at a page view in Search, you can find its related request.</span></span>

<span data-ttu-id="3059a-197">hello provtagning algoritmen behåller relaterade objekt.</span><span class="sxs-lookup"><span data-stu-id="3059a-197">hello sampling algorithm retains related items.</span></span> <span data-ttu-id="3059a-198">För varje HTTP-begäran är händelsen som den och dess relaterade händelser antingen ignoreras eller skickas.</span><span class="sxs-lookup"><span data-stu-id="3059a-198">For each HTTP request event, it and its related events are either discarded or transmitted.</span></span> 

<span data-ttu-id="3059a-199">I Metrics Explorer multipliceras priser som begäran och undantag räknas med en faktor toocompensate för hello samplingsfrekvensen, så att de är ungefär korrekt.</span><span class="sxs-lookup"><span data-stu-id="3059a-199">In Metrics Explorer, rates such as request and exception counts are multiplied by a factor toocompensate for hello sampling rate, so that they are approximately correct.</span></span>

1. <span data-ttu-id="3059a-200">**Uppdatera ditt projekt NuGet-paket** toohello senaste *förhandsversionen* version av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="3059a-200">**Update your project's NuGet packages** toohello latest *pre-release* version of Application Insights.</span></span> <span data-ttu-id="3059a-201">Högerklicka på hello projektet i Solution Explorer, välja hantera NuGet-paket, kontrollera **inkludera förhandsversion** och Sök efter Microsoft.ApplicationInsights.Web.</span><span class="sxs-lookup"><span data-stu-id="3059a-201">Right-click hello project in Solution Explorer, choose Manage NuGet Packages, check **Include prerelease** and search for Microsoft.ApplicationInsights.Web.</span></span> 
2. <span data-ttu-id="3059a-202">**Inaktivera anpassningsbar provtagning**: I [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), ta bort eller kommentera ut hello `AdaptiveSamplingTelemetryProcessor` nod.</span><span class="sxs-lookup"><span data-stu-id="3059a-202">**Disable adaptive sampling**: In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), remove or comment out hello `AdaptiveSamplingTelemetryProcessor` node.</span></span>
   
    ```xml
   
    <TelemetryProcessors>
   
    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->

    ```

1. <span data-ttu-id="3059a-203">**Aktivera hello fast räntesats provtagning modulen.**</span><span class="sxs-lookup"><span data-stu-id="3059a-203">**Enable hello fixed-rate sampling module.**</span></span> <span data-ttu-id="3059a-204">Lägg till den här fragment för[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="3059a-204">Add this snippet too[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span>
   
    ```XML
   
    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
   
      <!-- Set a percentage close too100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>
   
    ```

> [!NOTE]
> <span data-ttu-id="3059a-205">Välj ett procentvärde som är nära too100/N, där N är ett heltal för hello samplingsprocenten.</span><span class="sxs-lookup"><span data-stu-id="3059a-205">For hello sampling percentage, choose a percentage that is close too100/N where N is an integer.</span></span>  <span data-ttu-id="3059a-206">Samlar för närvarande stöd inte för andra värden.</span><span class="sxs-lookup"><span data-stu-id="3059a-206">Currently sampling doesn't support other values.</span></span>
> 
> 

### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a><span data-ttu-id="3059a-207">Alternativ: aktivera fast räntesats provtagning i serverkoden</span><span class="sxs-lookup"><span data-stu-id="3059a-207">Alternative: enable fixed-rate sampling in your server code</span></span>
<span data-ttu-id="3059a-208">I stället för att ange hello provtagning parameter i hello .config-fil kan använda du koden.</span><span class="sxs-lookup"><span data-stu-id="3059a-208">Instead of setting hello sampling parameter in hello .config file, you can use code.</span></span> 

<span data-ttu-id="3059a-209">*C#*</span><span class="sxs-lookup"><span data-stu-id="3059a-209">*C#*</span></span>

```C#

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.GetTelemetryProcessorChainBuilder();
    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

<span data-ttu-id="3059a-210">([Lär dig mer om telemetri processorer](app-insights-api-filtering-sampling.md#filtering).)</span><span class="sxs-lookup"><span data-stu-id="3059a-210">([Learn about telemetry processors](app-insights-api-filtering-sampling.md#filtering).)</span></span>

## <a name="when-toouse-sampling"></a><span data-ttu-id="3059a-211">När toouse provtagning?</span><span class="sxs-lookup"><span data-stu-id="3059a-211">When toouse sampling?</span></span>
<span data-ttu-id="3059a-212">Anpassningsbar provtagning aktiveras automatiskt om du använder hello SDK för ASP.NET version 2.0.0-beta3 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3059a-212">Adaptive sampling is automatically enabled if you use hello ASP.NET SDK version 2.0.0-beta3 or later.</span></span> <span data-ttu-id="3059a-213">Du kan använda införandet provtagning (på vår server) oavsett vilken SDK-version som du använder.</span><span class="sxs-lookup"><span data-stu-id="3059a-213">No matter what SDK version you use, you can use ingestion sampling (at our server).</span></span>

<span data-ttu-id="3059a-214">Du behöver inte samplingsfrekvensen för de flesta program för små och medelstora storlek.</span><span class="sxs-lookup"><span data-stu-id="3059a-214">You don’t need sampling for most small and medium size applications.</span></span> <span data-ttu-id="3059a-215">hello mest användbara diagnostisk information och mest korrekta statistik erhålls genom att samla in data på alla användaraktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3059a-215">hello most useful diagnostic information and most accurate statistics are obtained by collecting data on all your user activities.</span></span> 

<span data-ttu-id="3059a-216">hello huvudsakliga fördelarna med samplingsfrekvensen är:</span><span class="sxs-lookup"><span data-stu-id="3059a-216">hello main advantages of sampling are:</span></span>

* <span data-ttu-id="3059a-217">Application Insights service således (”begränsas”) datapunkter när appen skickar en mycket hög andel telemetri kort sagt tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="3059a-217">Application Insights service drops ("throttles") data points when your app sends a very high rate of telemetry in short time interval.</span></span> 
* <span data-ttu-id="3059a-218">tookeep inom hello [kvot](app-insights-pricing.md) datapunkter för din prisnivå.</span><span class="sxs-lookup"><span data-stu-id="3059a-218">tookeep within hello [quota](app-insights-pricing.md) of data points for your pricing tier.</span></span> 
* <span data-ttu-id="3059a-219">tooreduce nätverkstrafik från hello samling telemetri.</span><span class="sxs-lookup"><span data-stu-id="3059a-219">tooreduce network traffic from hello collection of telemetry.</span></span> 

### <a name="which-type-of-sampling-should-i-use"></a><span data-ttu-id="3059a-220">Vilken typ av provtagning ska jag använda?</span><span class="sxs-lookup"><span data-stu-id="3059a-220">Which type of sampling should I use?</span></span>
<span data-ttu-id="3059a-221">**Använd införandet provtagning om:**</span><span class="sxs-lookup"><span data-stu-id="3059a-221">**Use ingestion sampling if:**</span></span>

* <span data-ttu-id="3059a-222">Ofta gå igenom den månatliga kvoten av telemetri.</span><span class="sxs-lookup"><span data-stu-id="3059a-222">You often go through your monthly quota of telemetry.</span></span>
* <span data-ttu-id="3059a-223">Du använder en version av hello SDK som inte stöder provtagning – till exempel, hello Java SDK eller ASP.NET-versioner tidigare än 2.</span><span class="sxs-lookup"><span data-stu-id="3059a-223">You're using a version of hello SDK that doesn't support sampling - for example, hello Java SDK or ASP.NET versions earlier than 2.</span></span>
* <span data-ttu-id="3059a-224">Du får mycket telemetri från användarnas webbläsare.</span><span class="sxs-lookup"><span data-stu-id="3059a-224">You're getting a lot of telemetry from your users' web browsers.</span></span>

<span data-ttu-id="3059a-225">**Använd fast räntesats provtagning om:**</span><span class="sxs-lookup"><span data-stu-id="3059a-225">**Use fixed-rate sampling if:**</span></span>

* <span data-ttu-id="3059a-226">Du använder hello Application Insights SDK för ASP.NET web services version 2.0.0 eller senare, och</span><span class="sxs-lookup"><span data-stu-id="3059a-226">You're using hello Application Insights SDK for ASP.NET web services version 2.0.0 or later, and</span></span>
* <span data-ttu-id="3059a-227">Du vill synkroniserade provtagning mellan klienten och servern, så att när du vill veta händelser i [Sök](app-insights-diagnostic-search.md), du kan navigera mellan relaterade händelser på hello-klienten och servern, till exempel sidvisningar och HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="3059a-227">You want synchronized sampling between client and server, so that, when you're investigating events in [Search](app-insights-diagnostic-search.md), you can navigate between related events on hello client and server, such as page views and http requests.</span></span>
* <span data-ttu-id="3059a-228">Du är säker på av hello lämpliga samplingsprocenten för din app.</span><span class="sxs-lookup"><span data-stu-id="3059a-228">You are confident of hello appropriate sampling percentage for your app.</span></span> <span data-ttu-id="3059a-229">Det bör vara tillräckligt högt tooget exakta mått, men nedan hello hastigheten som överskrider din prisnivå kvot och hello throttling-begränsning.</span><span class="sxs-lookup"><span data-stu-id="3059a-229">It should be high enough tooget accurate metrics, but below hello rate that exceeds your pricing quota and hello throttling limits.</span></span> 

<span data-ttu-id="3059a-230">**Använda anpassningsbar provtagning:**</span><span class="sxs-lookup"><span data-stu-id="3059a-230">**Use adaptive sampling:**</span></span>

<span data-ttu-id="3059a-231">I annat fall rekommenderar vi anpassningsbar provtagning.</span><span class="sxs-lookup"><span data-stu-id="3059a-231">Otherwise, we recommend adaptive sampling.</span></span> <span data-ttu-id="3059a-232">Detta är aktiverat som standard i hello ASP.NET server SDK version 2.0.0-beta3 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3059a-232">This is enabled by default in hello ASP.NET server SDK, version 2.0.0-beta3 or later.</span></span> <span data-ttu-id="3059a-233">Det minska inte trafik till en viss minsta hastighet, så att den inte påverkar en plats med låg användning.</span><span class="sxs-lookup"><span data-stu-id="3059a-233">It doesn't reduce traffic until a certain minimum rate, so it won't affect a low-use site.</span></span>

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a><span data-ttu-id="3059a-234">Hur vet jag om sampling är i drift?</span><span class="sxs-lookup"><span data-stu-id="3059a-234">How do I know whether sampling is in operation?</span></span>
<span data-ttu-id="3059a-235">toodiscover hello faktiska samplingsfrekvens oavsett om den har tillämpats, använda en [Analytics query](app-insights-analytics.md) sådana här:</span><span class="sxs-lookup"><span data-stu-id="3059a-235">toodiscover hello actual sampling rate no matter where it has been applied, use an [Analytics query](app-insights-analytics.md) such as this:</span></span>

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

<span data-ttu-id="3059a-236">Bevaras i varje post, `itemCount` anger hello antal ursprungliga poster som den representerar lika too1 + hello antal tidigare borttagna poster.</span><span class="sxs-lookup"><span data-stu-id="3059a-236">In each retained record, `itemCount` indicates hello number of original records that it represents, equal too1 + hello number of previous discarded records.</span></span> 

## <a name="how-does-sampling-work"></a><span data-ttu-id="3059a-237">Hur fungerar provtagning?</span><span class="sxs-lookup"><span data-stu-id="3059a-237">How does sampling work?</span></span>
<span data-ttu-id="3059a-238">Fast räntesats och anpassningsbar provtagning är en funktion i hello SDK i ASP.NET-versioner från 2.0.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="3059a-238">Fixed-rate and adaptive sampling are a feature of hello SDK in ASP.NET versions from 2.0.0 onwards.</span></span> <span data-ttu-id="3059a-239">Införandet provtagning är en funktion i hello Application Insights-tjänsten och kan vara i drift om hello SDK inte utför provtagning.</span><span class="sxs-lookup"><span data-stu-id="3059a-239">Ingestion sampling is a feature of hello Application Insights service, and can be in operation if hello SDK is not performing sampling.</span></span> 

<span data-ttu-id="3059a-240">hello provtagning algoritmen bestämmer vilka telemetri objekt toodrop och vilka tookeep som omfattas av viktiga och som (om det är i hello SDK eller hello Application Insights-tjänsten).</span><span class="sxs-lookup"><span data-stu-id="3059a-240">hello sampling algorithm decides which telemetry items toodrop, and which ones tookeep (whether it's in hello SDK or in hello Application Insights service).</span></span> <span data-ttu-id="3059a-241">hello provtagning beslut baseras på flera regler som syftar toopreserve alla relaterade-datapunkter är intakta, underhålla Avbrottsfritt diagnostik i Application Insights som är tillämplig och tillförlitlig även med en minskad datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="3059a-241">hello sampling decision is based on several rules that aim toopreserve all interrelated data points intact, maintaining a diagnostic experience in Application Insights that is actionable and reliable even with a reduced data set.</span></span> <span data-ttu-id="3059a-242">Till exempel om din app skickar ytterligare telemetri objekt (till exempel undantag och spårningar som loggats från denna begäran) för en misslyckad begäran, provtagning inte delar upp denna begäran och andra telemetri.</span><span class="sxs-lookup"><span data-stu-id="3059a-242">For example, if for a failed request your app sends additional telemetry items (such as exception and traces logged from this request), sampling will not split this request and other telemetry.</span></span> <span data-ttu-id="3059a-243">Den fortsätter eller utelämnar dem samtidigt.</span><span class="sxs-lookup"><span data-stu-id="3059a-243">It either keeps or drops them all together.</span></span> <span data-ttu-id="3059a-244">När du tittar på hello Frågedetaljer i Application Insights kan du därför alltid se hello begäran tillsammans med dess associerade telemetri-objekt.</span><span class="sxs-lookup"><span data-stu-id="3059a-244">As a result, when you look at hello request details in Application Insights, you can always see hello request along with its associated telemetry items.</span></span> 

<span data-ttu-id="3059a-245">För program som definierar ”användare” (det vill säga de vanligaste webbprogram), hello provtagning beslut baserat på hello hash för hello användar-id, vilket innebär att all telemetri för en viss användare är antingen bevaras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="3059a-245">For applications that define "user" (that is, most typical web applications), hello sampling decision is based on hello hash of hello user id, which means that all telemetry for any particular user is either preserved or dropped.</span></span> <span data-ttu-id="3059a-246">För hello typer av program som inte definierar användare (till exempel webbtjänster) baserat hello provtagning beslutet på hello åtgärds-id för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="3059a-246">For hello types of applications that don't define users (such as web services) hello sampling decision is based on hello operation id of hello request.</span></span> <span data-ttu-id="3059a-247">Slutligen för hello telemetri artiklar som varken har användaren eller åtgärden-id som angetts (till exempel telemetri objekt som rapporterats från asynkron trådar med ingen kontext för http) provtagning bara samlar in en del av telemetri varje typ av objekt.</span><span class="sxs-lookup"><span data-stu-id="3059a-247">Finally, for hello telemetry items that neither have user nor operation id set (for example telemetry items reported from asynchronous threads with no http context) sampling simply captures a percentage of telemetry items of each type.</span></span> 

<span data-ttu-id="3059a-248">När presenterar telemetri tillbaka tooyou hello hello Application Insights-tjänsten justerar hello mått med samma samplingsprocenten som används för närvarande hello mängden toocompensate för hello saknar datapunkter.</span><span class="sxs-lookup"><span data-stu-id="3059a-248">When presenting telemetry back tooyou, hello Application Insights service adjusts hello metrics by hello same sampling percentage that was used at hello time of collection, toocompensate for hello missing data points.</span></span> <span data-ttu-id="3059a-249">När du tittar på hello telemetri i Application Insights därför ser hello användare statistiskt korrekt approximeringar som är mycket nära toohello reellt tal.</span><span class="sxs-lookup"><span data-stu-id="3059a-249">Hence, when looking at hello telemetry in Application Insights, hello users are seeing statistically correct approximations that are very close toohello real numbers.</span></span>

<span data-ttu-id="3059a-250">hello korrekt hello uppskattning är i stort sett beror på hello konfigurerade samplingsprocenten.</span><span class="sxs-lookup"><span data-stu-id="3059a-250">hello accuracy of hello approximation largely depends on hello configured sampling percentage.</span></span> <span data-ttu-id="3059a-251">Dessutom ökar hello noggrannhet för program som hanterar stora mängder vanligtvis liknande begäranden från många olika användare.</span><span class="sxs-lookup"><span data-stu-id="3059a-251">Also, hello accuracy increases for applications that handle a large volume of generally similar requests from lots of users.</span></span> <span data-ttu-id="3059a-252">Hej däremot för program som inte fungerar med en stor belastning på, provtagning behövs inte dessa program kan skicka all telemetri vanligtvis när du befinner dig inom hello kvoten utan att orsaka förlust av data från begränsning.</span><span class="sxs-lookup"><span data-stu-id="3059a-252">On hello other hand, for applications that don't work with a significant load, sampling is not needed as these applications can usually send all their telemetry while staying within hello quota, without causing data loss from throttling.</span></span> 

<span data-ttu-id="3059a-253">Observera att Application Insights exempel inte mått och sessioner telemetri typer eftersom för dessa typer, minskad hello precisionen kan vara hög oönskade.</span><span class="sxs-lookup"><span data-stu-id="3059a-253">Note that Application Insights does not sample Metrics and Sessions telemetry types, since for these types, reduction in hello precision can be highly undesirable.</span></span> 

### <a name="adaptive-sampling"></a><span data-ttu-id="3059a-254">Anpassningsbar provtagning</span><span class="sxs-lookup"><span data-stu-id="3059a-254">Adaptive sampling</span></span>
<span data-ttu-id="3059a-255">Anpassningsbar provtagning lägger till en komponent som övervakar hello aktuella överföringshastigheten från hello SDK och justerar hello provtagning procentandel tootry toostay inom hello mål högsta överföringshastighet.</span><span class="sxs-lookup"><span data-stu-id="3059a-255">Adaptive sampling adds a component that monitors hello current rate of transmission from hello SDK, and adjusts hello sampling percentage tootry toostay within hello target maximum rate.</span></span> <span data-ttu-id="3059a-256">hello justeringen beräknas med jämna mellanrum och baseras på ett glidande medelvärde för hello utgående överföringshastigheten.</span><span class="sxs-lookup"><span data-stu-id="3059a-256">hello adjustment is recalculated at regular intervals, and is based on a moving average of hello outgoing transmission rate.</span></span>

## <a name="sampling-and-hello-javascript-sdk"></a><span data-ttu-id="3059a-257">Provtagning och hello JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="3059a-257">Sampling and hello JavaScript SDK</span></span>
<span data-ttu-id="3059a-258">hello på klientsidan (JavaScript) SDK deltar i fast räntesats provtagning tillsammans med hello serversidan SDK.</span><span class="sxs-lookup"><span data-stu-id="3059a-258">hello client-side (JavaScript) SDK participates in fixed-rate sampling in conjunction with hello server-side SDK.</span></span> <span data-ttu-id="3059a-259">Hej instrumenterats sidor bara skicka telemetri för klientsidan från hello samma användare för vilka hello serversidan gjorts beslutet för ”sample i”.</span><span class="sxs-lookup"><span data-stu-id="3059a-259">hello instrumented pages will only send client-side telemetry from hello same users for which hello server-side made its decision too"sample in."</span></span> <span data-ttu-id="3059a-260">Den här logiken är utformad toomaintain integriteten hos användarsession över - och server-klienten.</span><span class="sxs-lookup"><span data-stu-id="3059a-260">This logic is designed toomaintain integrity of user session across client- and server-sides.</span></span> <span data-ttu-id="3059a-261">Därför från ett visst telemetri objekt i Application Insights hittar du alla andra telemetri objekt för den här användaren eller sessionen.</span><span class="sxs-lookup"><span data-stu-id="3059a-261">As a result, from any particular telemetry item in Application Insights you can find all other telemetry items for this user or session.</span></span> 

<span data-ttu-id="3059a-262">*Klient- och serversidan telemetri Visa inte samordnas exempel som beskrivs ovan.*</span><span class="sxs-lookup"><span data-stu-id="3059a-262">*My client and server-side telemetry don't show coordinated samples as you describe above.*</span></span>

* <span data-ttu-id="3059a-263">Kontrollera att du har aktiverat fast räntesats provtagning både servern och klienten.</span><span class="sxs-lookup"><span data-stu-id="3059a-263">Verify that you enabled fixed-rate sampling both on server and client.</span></span>
* <span data-ttu-id="3059a-264">Kontrollera att hello SDK version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3059a-264">Make sure that hello SDK version is 2.0 or above.</span></span>
* <span data-ttu-id="3059a-265">Kontrollera att du ställer in hello samma provtagning procent i både hello klient och server.</span><span class="sxs-lookup"><span data-stu-id="3059a-265">Check that you set hello same sampling percentage in both hello client and server.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="3059a-266">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="3059a-266">Frequently Asked Questions</span></span>
<span data-ttu-id="3059a-267">*Varför inte provtagning en enkel ”samla in X procent av varje typ av telemetri”?*</span><span class="sxs-lookup"><span data-stu-id="3059a-267">*Why isn't sampling a simple "collect X percent of each telemetry type"?*</span></span>

* <span data-ttu-id="3059a-268">När den här metoden för provtagning skulle ge en mycket hög exakta mått approximeringar, skulle brytas möjlighet toocorrelate diagnostikdata per användare, session och begäran som är viktiga för diagnostik.</span><span class="sxs-lookup"><span data-stu-id="3059a-268">While this sampling approach would provide with a very high precision in metric approximations, it would break ability toocorrelate diagnostic data per user, session, and request, which is critical for diagnostics.</span></span> <span data-ttu-id="3059a-269">Därför provtagning fungerar bättre med ”samla in alla telemetri artiklar för X procent av app-användare” eller ”samla in all telemetri för X procent av begäranden som app” logik.</span><span class="sxs-lookup"><span data-stu-id="3059a-269">Therefore, sampling works better with "collect all telemetry items for X percent of app users", or "collect all telemetry for X percent of app requests" logic.</span></span> <span data-ttu-id="3059a-270">Hello telemetri objekt som inte är associerade med hello-begäranden (till exempel asynkron behandling i bakgrunden) är hello faller tillbaka för ”samla in X procent av alla objekt för varje typ av telemetri”.</span><span class="sxs-lookup"><span data-stu-id="3059a-270">For hello telemetry items not associated with hello requests (such as background asynchronous processing), hello fall back is too"collect X percent of all items for each telemetry type."</span></span> 

<span data-ttu-id="3059a-271">*Hej samplingsprocenten kan förändras över tid?*</span><span class="sxs-lookup"><span data-stu-id="3059a-271">*Can hello sampling percentage change over time?*</span></span>

* <span data-ttu-id="3059a-272">Ja, anpassningsbar provtagning gradvis ändrar hello samplingsprocenten baserat på hello som för närvarande observerats hello telemetrivolym.</span><span class="sxs-lookup"><span data-stu-id="3059a-272">Yes, adaptive sampling gradually changes hello sampling percentage, based on hello currently observed volume of hello telemetry.</span></span>

<span data-ttu-id="3059a-273">*Om jag använder fast räntesats provtagning hur vet jag vilka provtagning procentandel fungerar hello bäst för min app?*</span><span class="sxs-lookup"><span data-stu-id="3059a-273">*If I use fixed-rate sampling, how do I know which sampling percentage will work hello best for my app?*</span></span>

* <span data-ttu-id="3059a-274">Ett sätt är toostart med anpassningsbar provtagning, ta reda på vad ge ett omdöme reglerar på (se hello ovan fråga), och sedan växla toofixed-hastighet provtagning med denna kurs.</span><span class="sxs-lookup"><span data-stu-id="3059a-274">One way is toostart with adaptive sampling, find out what rate it settles on (see hello above question), and then switch toofixed-rate sampling using that rate.</span></span> 
  
    <span data-ttu-id="3059a-275">Annars kan få du tooguess.</span><span class="sxs-lookup"><span data-stu-id="3059a-275">Otherwise, you have tooguess.</span></span> <span data-ttu-id="3059a-276">Analysera ditt nuvarande bruk som telemetri i AI, se någon begränsning som inträffar och uppskattning hello telemetrivolym hello samlas in.</span><span class="sxs-lookup"><span data-stu-id="3059a-276">Analyze your current telemetry usage in AI, observe any throttling that is occurring, and estimate hello volume of hello collected telemetry.</span></span> <span data-ttu-id="3059a-277">Dessa tre indata, tillsammans med din valda prisnivå förslag på hur mycket du kanske vill tooreduce hello telemetrivolym hello samlas in.</span><span class="sxs-lookup"><span data-stu-id="3059a-277">These three inputs, together with your selected pricing tier, suggest how much you might want tooreduce hello volume of hello collected telemetry.</span></span> <span data-ttu-id="3059a-278">Dock kan en ökning av hello antal användare eller vissa SKIFT i hello telemetrivolym ogiltig uppskattningen.</span><span class="sxs-lookup"><span data-stu-id="3059a-278">However, an increase in hello number of your users or some other shift in hello volume of telemetry might invalidate your estimate.</span></span>

<span data-ttu-id="3059a-279">*Vad händer om jag konfigurera samplingsprocenten för lågt?*</span><span class="sxs-lookup"><span data-stu-id="3059a-279">*What happens if I configure sampling percentage too low?*</span></span>

* <span data-ttu-id="3059a-280">Låg samplingsprocenten (over-aggressive provtagning) minskar hello riktighet hello approximeringar när Application Insights försöker toocompensate hello visualisering av hello data för hello data volym minska.</span><span class="sxs-lookup"><span data-stu-id="3059a-280">Excessively low sampling percentage (over-aggressive sampling) reduces hello accuracy of hello approximations, when Application Insights attempts toocompensate hello visualization of hello data for hello data volume reduction.</span></span> <span data-ttu-id="3059a-281">Dessutom diagnostiska upplevelse kan påverkas negativt, med hello sällan misslyckas eller långsam begäranden prov ut.</span><span class="sxs-lookup"><span data-stu-id="3059a-281">Also, diagnostic experience might be negatively impacted, as some of hello infrequently failing or slow requests may be sampled out.</span></span>

<span data-ttu-id="3059a-282">*Vad händer om jag konfigurera samplingsprocenten för hög?*</span><span class="sxs-lookup"><span data-stu-id="3059a-282">*What happens if I configure sampling percentage too high?*</span></span>

* <span data-ttu-id="3059a-283">Konfiguration för hög provtagning procentandel (inte aggressivt tillräckligt) resulterar i en otillräcklig minskning i hello mängd hello samlas in telemetri.</span><span class="sxs-lookup"><span data-stu-id="3059a-283">Configuring too high sampling percentage (not aggressive enough) results in an insufficient reduction in hello volume of hello collected telemetry.</span></span> <span data-ttu-id="3059a-284">Du kan fortfarande se telemetri dataförlust relaterade toothrottling och hello kostnaden för att använda Application Insights kan vara högre än du planerat på grund av toooverage avgifter.</span><span class="sxs-lookup"><span data-stu-id="3059a-284">You may still experience telemetry data loss related toothrottling, and hello cost of using Application Insights might be higher than you planned due toooverage charges.</span></span>

<span data-ttu-id="3059a-285">*På vilka plattformar kan du använda provtagning?*</span><span class="sxs-lookup"><span data-stu-id="3059a-285">*On what platforms can I use sampling?*</span></span>

* <span data-ttu-id="3059a-286">Införandet sampling kan ske automatiskt för alla telemetri över en viss volym om hello SDK inte utför provtagning.</span><span class="sxs-lookup"><span data-stu-id="3059a-286">Ingestion sampling can occur automatically for any telemetry above a certain volume, if hello SDK is not performing sampling.</span></span> <span data-ttu-id="3059a-287">Detta fungerar, till exempel om din app använder en Java-server, eller om du använder en äldre version av hello SDK för ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3059a-287">This would work, for example, if your app uses a Java server, or if you are using an older version of hello ASP.NET SDK.</span></span>
* <span data-ttu-id="3059a-288">Om du använder SDK för ASP.NET-versioner 2.0.0 och senare (finns i Azure eller på din server), får du anpassningsbar provtagning som standard men du kan växla toofixed hastighet som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="3059a-288">If you're using ASP.NET SDK versions 2.0.0 and above (hosted either in Azure or on your own server), you get adaptive sampling by default, but you can switch toofixed-rate as described above.</span></span> <span data-ttu-id="3059a-289">Med fast räntesats provtagning synkroniserar hello webbläsare SDK automatiskt toosample relaterade händelser.</span><span class="sxs-lookup"><span data-stu-id="3059a-289">With fixed-rate sampling, hello browser SDK automatically synchronizes toosample related events.</span></span> 

<span data-ttu-id="3059a-290">*Det finns vissa ovanliga händelser som jag vill alltid toosee. Hur kan jag få dem tidigare hello provtagning modulen?*</span><span class="sxs-lookup"><span data-stu-id="3059a-290">*There are certain rare events I always want toosee. How can I get them past hello sampling module?*</span></span>

* <span data-ttu-id="3059a-291">Initiera en separat instans av TelemetryClient med en ny TelemetryConfiguration (inte hello standard aktiva).</span><span class="sxs-lookup"><span data-stu-id="3059a-291">Initialize a separate instance of TelemetryClient with a new TelemetryConfiguration (not hello default Active one).</span></span> <span data-ttu-id="3059a-292">Använd den toosend sällsynta händelserna.</span><span class="sxs-lookup"><span data-stu-id="3059a-292">Use that toosend your rare events.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3059a-293">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3059a-293">Next steps</span></span>
* <span data-ttu-id="3059a-294">[Filtrering](app-insights-api-filtering-sampling.md) kan ge mer strikt kontroll över din SDK skickar.</span><span class="sxs-lookup"><span data-stu-id="3059a-294">[Filtering](app-insights-api-filtering-sampling.md) can provide more strict control of what your SDK sends.</span></span>

