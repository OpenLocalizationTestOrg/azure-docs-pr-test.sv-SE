---
title: "aaaAutoscaling och Apptjänstmiljö v1"
description: "Autoskalning och Apptjänst-miljö"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: c23af2d8-d370-4b1f-9b3e-8782321ddccb
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: 1a03cf494309e80596b64471d1a067b2f64a9fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="autoscaling-and-app-service-environment-v1"></a><span data-ttu-id="4c4db-103">V1 autoskalning och Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="4c4db-103">Autoscaling and App Service Environment v1</span></span>

> [!NOTE]
> <span data-ttu-id="4c4db-104">Den här artikeln handlar om hello Apptjänstmiljö v1.</span><span class="sxs-lookup"><span data-stu-id="4c4db-104">This article is about hello App Service Environment v1.</span></span>  <span data-ttu-id="4c4db-105">Det finns en nyare version av hello Apptjänst-miljö som är enklare toouse och körs på mer kraftfulla infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="4c4db-105">There is a newer version of hello App Service Environment that is easier  toouse and runs on more powerful infrastructure.</span></span> <span data-ttu-id="4c4db-106">Mer om hello nya versionen börjar med hello toolearn [introduktion toohello Apptjänstmiljö](../app-service/app-service-environment/intro.md).</span><span class="sxs-lookup"><span data-stu-id="4c4db-106">toolearn more about hello new version start with hello [Introduction toohello App  Service Environment](../app-service/app-service-environment/intro.md).</span></span>
> 

<span data-ttu-id="4c4db-107">Stöd för Azure Apptjänst-miljöer *autoskalning*.</span><span class="sxs-lookup"><span data-stu-id="4c4db-107">Azure App Service environments support *autoscaling*.</span></span> <span data-ttu-id="4c4db-108">Du kan Autoskala enskilda arbetarpooler utifrån mått eller schema.</span><span class="sxs-lookup"><span data-stu-id="4c4db-108">You can autoscale individual worker pools based on metrics or schedule.</span></span>

![Autoskala alternativ för en arbetspool.][intro]

<span data-ttu-id="4c4db-110">Autoskalning optimerar användningen av resurser genom att automatiskt växande och minska storleken på en Apptjänst-miljö toofit din budget och/eller belastningen profil.</span><span class="sxs-lookup"><span data-stu-id="4c4db-110">Autoscaling optimizes your resource utilization by automatically growing and shrinking an App Service environment toofit your budget and or load profile.</span></span>

## <a name="configure-worker-pool-autoscale"></a><span data-ttu-id="4c4db-111">Konfigurera worker poolen Autoskala</span><span class="sxs-lookup"><span data-stu-id="4c4db-111">Configure worker pool autoscale</span></span>
<span data-ttu-id="4c4db-112">Du kan komma åt hello Autoskala funktioner från hello **inställningar** fliken hello arbetspool.</span><span class="sxs-lookup"><span data-stu-id="4c4db-112">You can access hello autoscale functionality from hello **Settings** tab of hello worker pool.</span></span>

![Fliken Inställningar i hello arbetspool.][settings-scale]

<span data-ttu-id="4c4db-114">Därifrån är hello-gränssnittet ska vara ganska bekant eftersom den hello planera samma upplevelse som visas när du skalar en Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="4c4db-114">From there, hello interface should be fairly familiar since it is hello same experience that you see when you scale an App Service plan.</span></span> 

![Inställningar för manuell skala.][scale-manual]

<span data-ttu-id="4c4db-116">Du kan också konfigurera en profil för Autoskala.</span><span class="sxs-lookup"><span data-stu-id="4c4db-116">You can also configure an autoscale profile.</span></span>

![Autoskala inställningar.][scale-profile]

<span data-ttu-id="4c4db-118">Autoskalningsprofiler är användbara tooset gränsen på din skala.</span><span class="sxs-lookup"><span data-stu-id="4c4db-118">Autoscale profiles are useful tooset limits on your scale.</span></span> <span data-ttu-id="4c4db-119">På så sätt kan du ha en konsekvent prestanda som uppstår genom att ange ett skalvärde som nedre gräns (1) och en förutsägbar utgifter fjärrskrivbordsanslutning genom att ange en övre gräns (2).</span><span class="sxs-lookup"><span data-stu-id="4c4db-119">This way, you can have a consistent performance experience by setting a lower bound scale value (1) and a predictable spend cap by setting an upper bound (2).</span></span>

![Skala inställningarna i profilen.][scale-profile2]

<span data-ttu-id="4c4db-121">När du definierar en profil kan du lägga till Autoskala regler tooscale uppåt eller nedåt hello antalet instanser i hello arbetspool inom hello gränser som definierats av hello-profilen.</span><span class="sxs-lookup"><span data-stu-id="4c4db-121">After you define a profile, you can add autoscale rules tooscale up or down hello number of instances in hello worker pool within hello bounds defined by hello profile.</span></span> <span data-ttu-id="4c4db-122">Autoskala regler baserat på mått.</span><span class="sxs-lookup"><span data-stu-id="4c4db-122">Autoscale rules are based on metrics.</span></span>

![Skalningsregeln.][scale-rule]

 <span data-ttu-id="4c4db-124">Alla arbetspool eller frontend mått kan vara används toodefine Autoskala regler.</span><span class="sxs-lookup"><span data-stu-id="4c4db-124">Any worker pool or front-end metrics can be used toodefine autoscale rules.</span></span> <span data-ttu-id="4c4db-125">De här måtten är hello samma mått som du kan övervaka i hello resurs bladet diagram eller ställa in aviseringar för.</span><span class="sxs-lookup"><span data-stu-id="4c4db-125">These metrics are hello same metrics you can monitor in hello resource blade graphs or set alerts for.</span></span>

## <a name="autoscale-example"></a><span data-ttu-id="4c4db-126">Autoskala exempel</span><span class="sxs-lookup"><span data-stu-id="4c4db-126">Autoscale example</span></span>
<span data-ttu-id="4c4db-127">Autoskala för en Apptjänst-miljö kan bäst illustreras genom att gå via ett scenario.</span><span class="sxs-lookup"><span data-stu-id="4c4db-127">Autoscale of an App Service environment can best be illustrated by walking through a scenario.</span></span>

<span data-ttu-id="4c4db-128">Den här artikeln beskrivs alla hello nödvändiga överväganden när du ställer in autoskalning.</span><span class="sxs-lookup"><span data-stu-id="4c4db-128">This article explains all hello necessary considerations when you set up autoscale.</span></span> <span data-ttu-id="4c4db-129">hello artikeln guidar dig igenom hello interaktioner som spelas upp när du räkna in autoskalning apptjänstmiljöer som finns i Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="4c4db-129">hello article walks you through hello interactions that come into play when you factor in autoscaling App Service environments that are hosted in App Service Environment.</span></span>

### <a name="scenario-introduction"></a><span data-ttu-id="4c4db-130">Scenariot introduktion</span><span class="sxs-lookup"><span data-stu-id="4c4db-130">Scenario introduction</span></span>
<span data-ttu-id="4c4db-131">Frank är en sysadmin för ett företag som har migrerats till en del av hello arbetsbelastningar han hanterar tooan Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="4c4db-131">Frank is a sysadmin for an enterprise who has migrated a portion of hello workloads that he manages tooan App Service environment.</span></span>

<span data-ttu-id="4c4db-132">hello konfigurerad Apptjänst-miljön är toomanual skala på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4c4db-132">hello App Service environment is configured toomanual scale as follows:</span></span>

* <span data-ttu-id="4c4db-133">**Främre slutar:** 3</span><span class="sxs-lookup"><span data-stu-id="4c4db-133">**Front ends:** 3</span></span>
* <span data-ttu-id="4c4db-134">**Arbetspool 1**: 10</span><span class="sxs-lookup"><span data-stu-id="4c4db-134">**Worker pool 1**: 10</span></span>
* <span data-ttu-id="4c4db-135">**Arbetspool 2**: 5</span><span class="sxs-lookup"><span data-stu-id="4c4db-135">**Worker pool 2**: 5</span></span>
* <span data-ttu-id="4c4db-136">**Arbetspool 3**: 5</span><span class="sxs-lookup"><span data-stu-id="4c4db-136">**Worker pool 3**: 5</span></span>

<span data-ttu-id="4c4db-137">Arbetspool 1 används för produktionsarbetsbelastningar medan arbetspool 2 och arbetspool 3 används för quality assurance (QA) och arbetsbelastningar för utveckling.</span><span class="sxs-lookup"><span data-stu-id="4c4db-137">Worker pool 1 is used for production workloads, while worker pool 2 and worker pool 3 are used for quality assurance (QA) and development workloads.</span></span>

<span data-ttu-id="4c4db-138">hello App Service-planer för QA och dev är konfigurerad toomanual skala.</span><span class="sxs-lookup"><span data-stu-id="4c4db-138">hello App Service plans for QA and dev are configured toomanual scale.</span></span> <span data-ttu-id="4c4db-139">hello produktion App Service-plan anges tooautoscale toodeal med variationer i nätverksbelastningen och.</span><span class="sxs-lookup"><span data-stu-id="4c4db-139">hello production App Service plan is set tooautoscale toodeal with variations in load and traffic.</span></span>

<span data-ttu-id="4c4db-140">Frank är insatt i hello program.</span><span class="sxs-lookup"><span data-stu-id="4c4db-140">Frank is very familiar with hello application.</span></span> <span data-ttu-id="4c4db-141">Han känner att hello användningsnivå för är mellan 9:00:00 och 18:00 eftersom detta är ett line-of-business (LOB)-program som medarbetarna använder när de arbetar med hello office.</span><span class="sxs-lookup"><span data-stu-id="4c4db-141">He knows that hello peak hours for load are between 9:00 AM and 6:00 PM because this is a line-of-business (LOB) application that employees use while they are in hello office.</span></span> <span data-ttu-id="4c4db-142">Användning utelämnar efter när användare är klar för den dagen.</span><span class="sxs-lookup"><span data-stu-id="4c4db-142">Usage drops after that when users are done for that day.</span></span> <span data-ttu-id="4c4db-143">Utanför användningsnivå finns fortfarande vissa eftersom användare kan komma åt hello app från en fjärrdator med hjälp av sina mobila enheter eller hemdatorer.</span><span class="sxs-lookup"><span data-stu-id="4c4db-143">Outside peak hours, there is still some load because users can access hello app remotely by using their mobile devices or home PCs.</span></span> <span data-ttu-id="4c4db-144">hello produktion programtjänstplanen är redan konfigurerat tooautoscale baserat på CPU-användning med hello följande regler:</span><span class="sxs-lookup"><span data-stu-id="4c4db-144">hello production App Service plan is already configured tooautoscale based on CPU usage with hello following rules:</span></span>

![Inställningar för LOB-app.][asp-scale]

| <span data-ttu-id="4c4db-146">**Autoskala profil – veckodagar – App Service-plan**</span><span class="sxs-lookup"><span data-stu-id="4c4db-146">**Autoscale profile – Weekdays – App Service plan**</span></span> | <span data-ttu-id="4c4db-147">**Autoskala profil – helger – App Service-plan**</span><span class="sxs-lookup"><span data-stu-id="4c4db-147">**Autoscale profile – Weekends – App Service plan**</span></span> |
| --- | --- |
| <span data-ttu-id="4c4db-148">**Namn:** veckodag profil</span><span class="sxs-lookup"><span data-stu-id="4c4db-148">**Name:** Weekday profile</span></span> |<span data-ttu-id="4c4db-149">**Namn:** helg profil</span><span class="sxs-lookup"><span data-stu-id="4c4db-149">**Name:** Weekend profile</span></span> |
| <span data-ttu-id="4c4db-150">**Skala av:** schema-och prestanda</span><span class="sxs-lookup"><span data-stu-id="4c4db-150">**Scale by:** Schedule and performance rules</span></span> |<span data-ttu-id="4c4db-151">**Skala av:** schema-och prestanda</span><span class="sxs-lookup"><span data-stu-id="4c4db-151">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="4c4db-152">**Profil:** veckodagar</span><span class="sxs-lookup"><span data-stu-id="4c4db-152">**Profile:** Weekdays</span></span> |<span data-ttu-id="4c4db-153">**Profil:** lördag</span><span class="sxs-lookup"><span data-stu-id="4c4db-153">**Profile:** Weekend</span></span> |
| <span data-ttu-id="4c4db-154">**Typ:** upprepning</span><span class="sxs-lookup"><span data-stu-id="4c4db-154">**Type:** Recurrence</span></span> |<span data-ttu-id="4c4db-155">**Typ:** upprepning</span><span class="sxs-lookup"><span data-stu-id="4c4db-155">**Type:** Recurrence</span></span> |
| <span data-ttu-id="4c4db-156">**Målområde:** 5 too20 instanser</span><span class="sxs-lookup"><span data-stu-id="4c4db-156">**Target range:** 5 too20 instances</span></span> |<span data-ttu-id="4c4db-157">**Målområde:** 3 too10 instanser</span><span class="sxs-lookup"><span data-stu-id="4c4db-157">**Target range:** 3 too10 instances</span></span> |
| <span data-ttu-id="4c4db-158">**Dagar:** måndag, tisdag, onsdag, torsdag, fredag</span><span class="sxs-lookup"><span data-stu-id="4c4db-158">**Days:** Monday, Tuesday, Wednesday, Thursday, Friday</span></span> |<span data-ttu-id="4c4db-159">**Dagar:** lördag, söndag</span><span class="sxs-lookup"><span data-stu-id="4c4db-159">**Days:** Saturday, Sunday</span></span> |
| <span data-ttu-id="4c4db-160">**Starttid:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="4c4db-160">**Start time:** 9:00 AM</span></span> |<span data-ttu-id="4c4db-161">**Starttid:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="4c4db-161">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="4c4db-162">**Tidszon:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="4c4db-162">**Time zone:** UTC-08</span></span> |<span data-ttu-id="4c4db-163">**Tidszon:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="4c4db-163">**Time zone:** UTC-08</span></span> |
|  | |
| <span data-ttu-id="4c4db-164">**Autoskala regeln (skala upp)**</span><span class="sxs-lookup"><span data-stu-id="4c4db-164">**Autoscale rule (Scale Up)**</span></span> |<span data-ttu-id="4c4db-165">**Autoskala regeln (skala upp)**</span><span class="sxs-lookup"><span data-stu-id="4c4db-165">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="4c4db-166">**Resurs:** produktion (Apptjänst-miljö)</span><span class="sxs-lookup"><span data-stu-id="4c4db-166">**Resource:** Production (App Service Environment)</span></span> |<span data-ttu-id="4c4db-167">**Resurs:** produktion (Apptjänst-miljö)</span><span class="sxs-lookup"><span data-stu-id="4c4db-167">**Resource:** Production (App Service Environment)</span></span> |
| <span data-ttu-id="4c4db-168">**Mått:** CPU i %</span><span class="sxs-lookup"><span data-stu-id="4c4db-168">**Metric:** CPU %</span></span> |<span data-ttu-id="4c4db-169">**Mått:** CPU i %</span><span class="sxs-lookup"><span data-stu-id="4c4db-169">**Metric:** CPU %</span></span> |
| <span data-ttu-id="4c4db-170">**Åtgärden:** större än 60%</span><span class="sxs-lookup"><span data-stu-id="4c4db-170">**Operation:** Greater than 60%</span></span> |<span data-ttu-id="4c4db-171">**Åtgärden:** större än 80%</span><span class="sxs-lookup"><span data-stu-id="4c4db-171">**Operation:** Greater than 80%</span></span> |
| <span data-ttu-id="4c4db-172">**Varaktighet:** 5 minuter</span><span class="sxs-lookup"><span data-stu-id="4c4db-172">**Duration:** 5 Minutes</span></span> |<span data-ttu-id="4c4db-173">**Varaktighet:** 10 minuter</span><span class="sxs-lookup"><span data-stu-id="4c4db-173">**Duration:** 10 Minutes</span></span> |
| <span data-ttu-id="4c4db-174">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="4c4db-174">**Time aggregation:** Average</span></span> |<span data-ttu-id="4c4db-175">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="4c4db-175">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="4c4db-176">**Åtgärd:** öka antalet av 2</span><span class="sxs-lookup"><span data-stu-id="4c4db-176">**Action:** Increase count by 2</span></span> |<span data-ttu-id="4c4db-177">**Åtgärd:** öka antalet med 1</span><span class="sxs-lookup"><span data-stu-id="4c4db-177">**Action:** Increase count by 1</span></span> |
| <span data-ttu-id="4c4db-178">**Kall ned (minuter):** 15</span><span class="sxs-lookup"><span data-stu-id="4c4db-178">**Cool down (minutes):** 15</span></span> |<span data-ttu-id="4c4db-179">**Kall ned (minuter):** 20</span><span class="sxs-lookup"><span data-stu-id="4c4db-179">**Cool down (minutes):** 20</span></span> |
|  | |
| <span data-ttu-id="4c4db-180">**Autoskala regeln (skala ned)**</span><span class="sxs-lookup"><span data-stu-id="4c4db-180">**Autoscale rule (Scale Down)**</span></span> |<span data-ttu-id="4c4db-181">**Autoskala regeln (skala ned)**</span><span class="sxs-lookup"><span data-stu-id="4c4db-181">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="4c4db-182">**Resurs:** produktion (Apptjänst-miljö)</span><span class="sxs-lookup"><span data-stu-id="4c4db-182">**Resource:** Production (App Service Environment)</span></span> |<span data-ttu-id="4c4db-183">**Resurs:** produktion (Apptjänst-miljö)</span><span class="sxs-lookup"><span data-stu-id="4c4db-183">**Resource:** Production (App Service Environment)</span></span> |
| <span data-ttu-id="4c4db-184">**Mått:** CPU i %</span><span class="sxs-lookup"><span data-stu-id="4c4db-184">**Metric:** CPU %</span></span> |<span data-ttu-id="4c4db-185">**Mått:** CPU i %</span><span class="sxs-lookup"><span data-stu-id="4c4db-185">**Metric:** CPU %</span></span> |
| <span data-ttu-id="4c4db-186">**Åtgärden:** mindre än 30%</span><span class="sxs-lookup"><span data-stu-id="4c4db-186">**Operation:** Less than 30%</span></span> |<span data-ttu-id="4c4db-187">**Åtgärden:** mindre än 20%</span><span class="sxs-lookup"><span data-stu-id="4c4db-187">**Operation:** Less than 20%</span></span> |
| <span data-ttu-id="4c4db-188">**Varaktighet:** 10 minuter</span><span class="sxs-lookup"><span data-stu-id="4c4db-188">**Duration:** 10 minutes</span></span> |<span data-ttu-id="4c4db-189">**Varaktighet:** 15 minuter</span><span class="sxs-lookup"><span data-stu-id="4c4db-189">**Duration:** 15 minutes</span></span> |
| <span data-ttu-id="4c4db-190">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="4c4db-190">**Time aggregation:** Average</span></span> |<span data-ttu-id="4c4db-191">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="4c4db-191">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="4c4db-192">**Åtgärd:** minska antalet med 1</span><span class="sxs-lookup"><span data-stu-id="4c4db-192">**Action:** Decrease count by 1</span></span> |<span data-ttu-id="4c4db-193">**Åtgärd:** minska antalet med 1</span><span class="sxs-lookup"><span data-stu-id="4c4db-193">**Action:** Decrease count by 1</span></span> |
| <span data-ttu-id="4c4db-194">**Kall ned (minuter):** 20</span><span class="sxs-lookup"><span data-stu-id="4c4db-194">**Cool down (minutes):** 20</span></span> |<span data-ttu-id="4c4db-195">**Kall ned (minuter):** 10</span><span class="sxs-lookup"><span data-stu-id="4c4db-195">**Cool down (minutes):** 10</span></span> |

### <a name="app-service-plan-inflation-rate"></a><span data-ttu-id="4c4db-196">App Service-plan inflationen hastighet</span><span class="sxs-lookup"><span data-stu-id="4c4db-196">App Service plan inflation rate</span></span>
<span data-ttu-id="4c4db-197">Programtjänstplaner som är konfigurerade tooautoscale gör det på en högsta överföringshastighet per timme.</span><span class="sxs-lookup"><span data-stu-id="4c4db-197">App Service plans that are configured tooautoscale do so at a maximum rate per hour.</span></span> <span data-ttu-id="4c4db-198">Detta värde kan beräknas baserat på hello-värden som anges på hello Autoskala regeln.</span><span class="sxs-lookup"><span data-stu-id="4c4db-198">This rate can be calculated based on hello values provided on hello autoscale rule.</span></span>

<span data-ttu-id="4c4db-199">Förstå och beräkna hello *App Service-plan inflationen hastighet* är viktigt för Apptjänst-miljö Autoskala eftersom skala ändringar tooa arbetspool inte omedelbar.</span><span class="sxs-lookup"><span data-stu-id="4c4db-199">Understanding and calculating hello *App Service plan inflation rate* is important for App Service environment autoscale because scale changes tooa worker pool are not instantaneous.</span></span>

<span data-ttu-id="4c4db-200">hello App Service-plan inflationen hastighet beräknas enligt följande:</span><span class="sxs-lookup"><span data-stu-id="4c4db-200">hello App Service plan inflation rate is calculated as follows:</span></span>

![App Service-plan inflationen beräkning.][ASP-Inflation]

<span data-ttu-id="4c4db-202">Baserat på hello Autoskala – skala upp regel för hello veckodag profil av hello App Service-plan:</span><span class="sxs-lookup"><span data-stu-id="4c4db-202">Based on hello Autoscale – Scale Up rule for hello Weekday profile of hello production App Service plan:</span></span>

![App Service-plan inflationen hastighet för veckodagar baserat på Autoskala – skala upp regeln.][Equation1]

<span data-ttu-id="4c4db-204">Hello formeln skulle motsvara hello Autoskala – skala upp regel för hello helgens profil av hello App Service-plan hello gäller:</span><span class="sxs-lookup"><span data-stu-id="4c4db-204">In hello case of hello Autoscale – Scale Up rule for hello Weekend profile of hello production App Service plan, hello formula would resolve to:</span></span>

![App Service-plan inflationen hastighet för helger baserat på Autoskala – skala upp regeln.][Equation2]

<span data-ttu-id="4c4db-206">Det här värdet kan också beräknas för skala ned åtgärder.</span><span class="sxs-lookup"><span data-stu-id="4c4db-206">This value can also be calculated for scale-down operations.</span></span>

<span data-ttu-id="4c4db-207">Baserat på hello Autoskala – skala ned regel för hello veckodag profil av hello App Service-plan detta skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="4c4db-207">Based on hello Autoscale – Scale Down rule for hello Weekday profile of hello production App Service plan, this would look as follows:</span></span>

![App Service-plan inflationen hastighet för veckodagar baserat på Autoskala – skala ned regeln.][Equation3]

<span data-ttu-id="4c4db-209">Hello formeln skulle motsvara hello Autoskala – skala ned regel för hello helgens profil av hello App Service-plan hello gäller:</span><span class="sxs-lookup"><span data-stu-id="4c4db-209">In hello case of hello Autoscale – Scale Down rule for hello Weekend profile of hello production App Service plan, hello formula would resolve to:</span></span>  

![App Service-plan inflationen hastighet för helger baserat på Autoskala – skala ned regeln.][Equation4]

<span data-ttu-id="4c4db-211">hello produktion App Service-plan kan växa på högst åtta instanser per timme under hello vecka och fyra instanser per timme när hello helg.</span><span class="sxs-lookup"><span data-stu-id="4c4db-211">hello production App Service plan can grow at a maximum rate of eight instances/hour during hello week and four instances/hour during hello weekend.</span></span> <span data-ttu-id="4c4db-212">Det kan släppa instanser på högst fyra instanser per timme under hello vecka och sex instanser per timme under helger.</span><span class="sxs-lookup"><span data-stu-id="4c4db-212">It can release instances at a maximum rate of four instances/hour during hello week and six instances/hour during weekends.</span></span>

<span data-ttu-id="4c4db-213">Om flera App Service-planer finns i en arbetspool, har du toocalculate hello *totala inflationen hastigheten* som hello summan av hello inflationen hastighet för alla hello App Service-planer som värd i den poolen, worker.</span><span class="sxs-lookup"><span data-stu-id="4c4db-213">If multiple App Service plans are being hosted in a worker pool, you have toocalculate hello *total inflation rate* as hello sum of hello inflation rate for all hello App Service plans that are being hosting in that worker pool.</span></span>

![Beräkning av totala inflationen hastigheten för flera App Service-planer finns i en arbetspool.][ASP-Total-Inflation]

### <a name="use-hello-app-service-plan-inflation-rate-toodefine-worker-pool-autoscale-rules"></a><span data-ttu-id="4c4db-215">Använd hello Apptjänst planera inflationen hastighet toodefine worker poolen Autoskala regler</span><span class="sxs-lookup"><span data-stu-id="4c4db-215">Use hello App Service plan inflation rate toodefine worker pool autoscale rules</span></span>
<span data-ttu-id="4c4db-216">Worker pooler som är värdar för App Service-planer som är konfigurerade tooautoscale behöver att allokera en buffert av kapacitet.</span><span class="sxs-lookup"><span data-stu-id="4c4db-216">Worker pools that host App Service plans that are configured tooautoscale need to be allocated a buffer of capacity.</span></span> <span data-ttu-id="4c4db-217">hello buffert tillåter hello Autoskala operations toogrow och minska programtjänstplanen efter behov.</span><span class="sxs-lookup"><span data-stu-id="4c4db-217">hello buffer allows for hello autoscale operations toogrow and shrink the App Service plan as needed.</span></span> <span data-ttu-id="4c4db-218">hello minsta buffertstorlek skulle beräknas hello App Service planera inflationen frekvens.</span><span class="sxs-lookup"><span data-stu-id="4c4db-218">hello minimum buffer would be hello calculated Total App Service Plan Inflation Rate.</span></span>

<span data-ttu-id="4c4db-219">Eftersom skalningsåtgärder för Apptjänst-miljö ta viss tid tooapply, bör ändringar konto för ytterligare begäran ändringar som kan inträffa när en skalningsåtgärd pågår.</span><span class="sxs-lookup"><span data-stu-id="4c4db-219">Because App Service environment scale operations take some time tooapply, any change should account for further demand changes that could happen while a scale operation is in progress.</span></span> <span data-ttu-id="4c4db-220">tooaccommodate latens, rekommenderar vi att du använder hello beräknas App Service planera inflationen frekvens som hello minsta antal instanser som har lagts till för varje Autoskala-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="4c4db-220">tooaccommodate this latency, we recommend that you use hello calculated Total App Service Plan Inflation Rate as hello minimum number of instances that are added for each autoscale operation.</span></span>

<span data-ttu-id="4c4db-221">Med den här informationen Frank definiera hello följande Autoskala profil och regler:</span><span class="sxs-lookup"><span data-stu-id="4c4db-221">With this information, Frank can define hello following autoscale profile and rules:</span></span>

![Autoskala profil regler för LOB-exempel.][Worker-Pool-Scale]

| <span data-ttu-id="4c4db-223">**Autoskala profil – veckodagar**</span><span class="sxs-lookup"><span data-stu-id="4c4db-223">**Autoscale profile – Weekdays**</span></span> | <span data-ttu-id="4c4db-224">**Autoskala profil – söndag**</span><span class="sxs-lookup"><span data-stu-id="4c4db-224">**Autoscale profile – Weekends**</span></span> |
| --- | --- |
| <span data-ttu-id="4c4db-225">**Namn:** veckodag profil</span><span class="sxs-lookup"><span data-stu-id="4c4db-225">**Name:** Weekday profile</span></span> |<span data-ttu-id="4c4db-226">**Namn:** helg profil</span><span class="sxs-lookup"><span data-stu-id="4c4db-226">**Name:** Weekend profile</span></span> |
| <span data-ttu-id="4c4db-227">**Skala av:** schema-och prestanda</span><span class="sxs-lookup"><span data-stu-id="4c4db-227">**Scale by:** Schedule and performance rules</span></span> |<span data-ttu-id="4c4db-228">**Skala av:** schema-och prestanda</span><span class="sxs-lookup"><span data-stu-id="4c4db-228">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="4c4db-229">**Profil:** veckodagar</span><span class="sxs-lookup"><span data-stu-id="4c4db-229">**Profile:** Weekdays</span></span> |<span data-ttu-id="4c4db-230">**Profil:** lördag</span><span class="sxs-lookup"><span data-stu-id="4c4db-230">**Profile:** Weekend</span></span> |
| <span data-ttu-id="4c4db-231">**Typ:** upprepning</span><span class="sxs-lookup"><span data-stu-id="4c4db-231">**Type:** Recurrence</span></span> |<span data-ttu-id="4c4db-232">**Typ:** upprepning</span><span class="sxs-lookup"><span data-stu-id="4c4db-232">**Type:** Recurrence</span></span> |
| <span data-ttu-id="4c4db-233">**Målområde:** 13 too25 instanser</span><span class="sxs-lookup"><span data-stu-id="4c4db-233">**Target range:** 13 too25 instances</span></span> |<span data-ttu-id="4c4db-234">**Målområde:** 6 too15 instanser</span><span class="sxs-lookup"><span data-stu-id="4c4db-234">**Target range:** 6 too15 instances</span></span> |
| <span data-ttu-id="4c4db-235">**Dagar:** måndag, tisdag, onsdag, torsdag, fredag</span><span class="sxs-lookup"><span data-stu-id="4c4db-235">**Days:** Monday, Tuesday, Wednesday, Thursday, Friday</span></span> |<span data-ttu-id="4c4db-236">**Dagar:** lördag, söndag</span><span class="sxs-lookup"><span data-stu-id="4c4db-236">**Days:** Saturday, Sunday</span></span> |
| <span data-ttu-id="4c4db-237">**Starttid:** 7:00:00</span><span class="sxs-lookup"><span data-stu-id="4c4db-237">**Start time:** 7:00 AM</span></span> |<span data-ttu-id="4c4db-238">**Starttid:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="4c4db-238">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="4c4db-239">**Tidszon:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="4c4db-239">**Time zone:** UTC-08</span></span> |<span data-ttu-id="4c4db-240">**Tidszon:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="4c4db-240">**Time zone:** UTC-08</span></span> |
|  | |
| <span data-ttu-id="4c4db-241">**Autoskala regeln (skala upp)**</span><span class="sxs-lookup"><span data-stu-id="4c4db-241">**Autoscale rule (Scale Up)**</span></span> |<span data-ttu-id="4c4db-242">**Autoskala regeln (skala upp)**</span><span class="sxs-lookup"><span data-stu-id="4c4db-242">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="4c4db-243">**Resurs:** arbetspool 1</span><span class="sxs-lookup"><span data-stu-id="4c4db-243">**Resource:** Worker pool 1</span></span> |<span data-ttu-id="4c4db-244">**Resurs:** arbetspool 1</span><span class="sxs-lookup"><span data-stu-id="4c4db-244">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="4c4db-245">**Mått:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="4c4db-245">**Metric:** WorkersAvailable</span></span> |<span data-ttu-id="4c4db-246">**Mått:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="4c4db-246">**Metric:** WorkersAvailable</span></span> |
| <span data-ttu-id="4c4db-247">**Åtgärden:** mindre än 8</span><span class="sxs-lookup"><span data-stu-id="4c4db-247">**Operation:** Less than 8</span></span> |<span data-ttu-id="4c4db-248">**Åtgärden:** färre än 3</span><span class="sxs-lookup"><span data-stu-id="4c4db-248">**Operation:** Less than 3</span></span> |
| <span data-ttu-id="4c4db-249">**Varaktighet:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="4c4db-249">**Duration:** 20 minutes</span></span> |<span data-ttu-id="4c4db-250">**Varaktighet:** 30 minuter</span><span class="sxs-lookup"><span data-stu-id="4c4db-250">**Duration:** 30 minutes</span></span> |
| <span data-ttu-id="4c4db-251">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="4c4db-251">**Time aggregation:** Average</span></span> |<span data-ttu-id="4c4db-252">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="4c4db-252">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="4c4db-253">**Åtgärd:** öka antalet med 8</span><span class="sxs-lookup"><span data-stu-id="4c4db-253">**Action:** Increase count by 8</span></span> |<span data-ttu-id="4c4db-254">**Åtgärd:** öka antalet med 3</span><span class="sxs-lookup"><span data-stu-id="4c4db-254">**Action:** Increase count by 3</span></span> |
| <span data-ttu-id="4c4db-255">**Kall ned (minuter):** 180</span><span class="sxs-lookup"><span data-stu-id="4c4db-255">**Cool down (minutes):** 180</span></span> |<span data-ttu-id="4c4db-256">**Kall ned (minuter):** 180</span><span class="sxs-lookup"><span data-stu-id="4c4db-256">**Cool down (minutes):** 180</span></span> |
|  | |
| <span data-ttu-id="4c4db-257">**Autoskala regeln (skala ned)**</span><span class="sxs-lookup"><span data-stu-id="4c4db-257">**Autoscale rule (Scale Down)**</span></span> |<span data-ttu-id="4c4db-258">**Autoskala regeln (skala ned)**</span><span class="sxs-lookup"><span data-stu-id="4c4db-258">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="4c4db-259">**Resurs:** arbetspool 1</span><span class="sxs-lookup"><span data-stu-id="4c4db-259">**Resource:** Worker pool 1</span></span> |<span data-ttu-id="4c4db-260">**Resurs:** arbetspool 1</span><span class="sxs-lookup"><span data-stu-id="4c4db-260">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="4c4db-261">**Mått:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="4c4db-261">**Metric:** WorkersAvailable</span></span> |<span data-ttu-id="4c4db-262">**Mått:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="4c4db-262">**Metric:** WorkersAvailable</span></span> |
| <span data-ttu-id="4c4db-263">**Åtgärden:** större än 8</span><span class="sxs-lookup"><span data-stu-id="4c4db-263">**Operation:** Greater than 8</span></span> |<span data-ttu-id="4c4db-264">**Åtgärden:** större än 3</span><span class="sxs-lookup"><span data-stu-id="4c4db-264">**Operation:** Greater than 3</span></span> |
| <span data-ttu-id="4c4db-265">**Varaktighet:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="4c4db-265">**Duration:** 20 minutes</span></span> |<span data-ttu-id="4c4db-266">**Varaktighet:** 15 minuter</span><span class="sxs-lookup"><span data-stu-id="4c4db-266">**Duration:** 15 minutes</span></span> |
| <span data-ttu-id="4c4db-267">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="4c4db-267">**Time aggregation:** Average</span></span> |<span data-ttu-id="4c4db-268">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="4c4db-268">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="4c4db-269">**Åtgärd:** minska antalet med 2</span><span class="sxs-lookup"><span data-stu-id="4c4db-269">**Action:** Decrease count by 2</span></span> |<span data-ttu-id="4c4db-270">**Åtgärd:** minska antalet med 3</span><span class="sxs-lookup"><span data-stu-id="4c4db-270">**Action:** Decrease count by 3</span></span> |
| <span data-ttu-id="4c4db-271">**Kall ned (minuter):** 120</span><span class="sxs-lookup"><span data-stu-id="4c4db-271">**Cool down (minutes):** 120</span></span> |<span data-ttu-id="4c4db-272">**Kall ned (minuter):** 120</span><span class="sxs-lookup"><span data-stu-id="4c4db-272">**Cool down (minutes):** 120</span></span> |

<span data-ttu-id="4c4db-273">hello mål-intervall som anges i profilen för hello beräknas genom hello minsta instanser som definierats i profilen för hello programtjänstplanen + buffert.</span><span class="sxs-lookup"><span data-stu-id="4c4db-273">hello Target range defined in hello profile is calculated by hello minimum instances defined in the profile for hello App Service plan + buffer.</span></span>

<span data-ttu-id="4c4db-274">hello maximalt intervall skulle vara hello summan av alla hello maximalt intervall för alla App Service-planer finns i hello arbetspool.</span><span class="sxs-lookup"><span data-stu-id="4c4db-274">hello Maximum range would be hello sum of all hello maximum ranges for all App Service plans hosted in hello worker pool.</span></span>

<span data-ttu-id="4c4db-275">hello öka antalet för hello skalas upp regler ska vara set tooat minst 1 X-App Service Plan inflationen hastighet för att skala upp.</span><span class="sxs-lookup"><span data-stu-id="4c4db-275">hello Increase count for hello scale up rules should be set tooat least 1X the App Service Plan Inflation Rate for scale up.</span></span>

<span data-ttu-id="4c4db-276">Minska antalet kan vara justerade toosomething mellan 1/2 X eller 1 X hello App Service planera inflationen hastighet för att skala ned.</span><span class="sxs-lookup"><span data-stu-id="4c4db-276">Decrease count can be adjusted toosomething between 1/2X or 1X hello App Service Plan Inflation Rate for scale down.</span></span>

### <a name="autoscale-for-front-end-pool"></a><span data-ttu-id="4c4db-277">Autoskala för frontend-pool</span><span class="sxs-lookup"><span data-stu-id="4c4db-277">Autoscale for front-end pool</span></span>
<span data-ttu-id="4c4db-278">Regler för frontend Autoskala är enklare än för arbetarpooler.</span><span class="sxs-lookup"><span data-stu-id="4c4db-278">Rules for front-end autoscale are simpler than for worker pools.</span></span> <span data-ttu-id="4c4db-279">Främst bör du</span><span class="sxs-lookup"><span data-stu-id="4c4db-279">Primarily, you should</span></span>  
<span data-ttu-id="4c4db-280">Kontrollera att beakta varaktighet hello mätning och hello cooldown timers skalningsåtgärder på en apptjänstplan inte är omedelbar.</span><span class="sxs-lookup"><span data-stu-id="4c4db-280">make sure that duration of hello measurement and hello cooldown timers consider that scale operations on an App Service plan are not instantaneous.</span></span>

<span data-ttu-id="4c4db-281">I det här scenariot vet Frank att hello Felfrekvens ökar när frontwebbservrarna når 80% CPU-användning och anger hello Autoskala regeln tooincrease instanser på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4c4db-281">For this scenario, Frank knows that hello error rate increases after front ends reach 80% CPU utilization and sets hello autoscale rule tooincrease instances as follows:</span></span>

![Autoskala inställningar för frontend-pool.][Front-End-Scale]

| <span data-ttu-id="4c4db-283">**Autoskala profil – framför slutar**</span><span class="sxs-lookup"><span data-stu-id="4c4db-283">**Autoscale profile – Front ends**</span></span> |
| --- |
| <span data-ttu-id="4c4db-284">**Namn:** Autoskala – framför slutar</span><span class="sxs-lookup"><span data-stu-id="4c4db-284">**Name:** Autoscale – Front ends</span></span> |
| <span data-ttu-id="4c4db-285">**Skala av:** schema-och prestanda</span><span class="sxs-lookup"><span data-stu-id="4c4db-285">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="4c4db-286">**Profil:** varje dag</span><span class="sxs-lookup"><span data-stu-id="4c4db-286">**Profile:** Everyday</span></span> |
| <span data-ttu-id="4c4db-287">**Typ:** upprepning</span><span class="sxs-lookup"><span data-stu-id="4c4db-287">**Type:** Recurrence</span></span> |
| <span data-ttu-id="4c4db-288">**Målområde:** 3 too10 instanser</span><span class="sxs-lookup"><span data-stu-id="4c4db-288">**Target range:** 3 too10 instances</span></span> |
| <span data-ttu-id="4c4db-289">**Dagar:** varje dag</span><span class="sxs-lookup"><span data-stu-id="4c4db-289">**Days:** Everyday</span></span> |
| <span data-ttu-id="4c4db-290">**Starttid:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="4c4db-290">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="4c4db-291">**Tidszon:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="4c4db-291">**Time zone:** UTC-08</span></span> |
|  |
| <span data-ttu-id="4c4db-292">**Autoskala regeln (skala upp)**</span><span class="sxs-lookup"><span data-stu-id="4c4db-292">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="4c4db-293">**Resurs:** frontend-pool</span><span class="sxs-lookup"><span data-stu-id="4c4db-293">**Resource:** Front-end pool</span></span> |
| <span data-ttu-id="4c4db-294">**Mått:** CPU i %</span><span class="sxs-lookup"><span data-stu-id="4c4db-294">**Metric:** CPU %</span></span> |
| <span data-ttu-id="4c4db-295">**Åtgärden:** större än 60%</span><span class="sxs-lookup"><span data-stu-id="4c4db-295">**Operation:** Greater than 60%</span></span> |
| <span data-ttu-id="4c4db-296">**Varaktighet:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="4c4db-296">**Duration:** 20 minutes</span></span> |
| <span data-ttu-id="4c4db-297">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="4c4db-297">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="4c4db-298">**Åtgärd:** öka antalet med 3</span><span class="sxs-lookup"><span data-stu-id="4c4db-298">**Action:** Increase count by 3</span></span> |
| <span data-ttu-id="4c4db-299">**Kall ned (minuter):** 120</span><span class="sxs-lookup"><span data-stu-id="4c4db-299">**Cool down (minutes):** 120</span></span> |
|  |
| <span data-ttu-id="4c4db-300">**Autoskala regeln (skala ned)**</span><span class="sxs-lookup"><span data-stu-id="4c4db-300">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="4c4db-301">**Resurs:** arbetspool 1</span><span class="sxs-lookup"><span data-stu-id="4c4db-301">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="4c4db-302">**Mått:** CPU i %</span><span class="sxs-lookup"><span data-stu-id="4c4db-302">**Metric:** CPU %</span></span> |
| <span data-ttu-id="4c4db-303">**Åtgärden:** mindre än 30%</span><span class="sxs-lookup"><span data-stu-id="4c4db-303">**Operation:** Less than 30%</span></span> |
| <span data-ttu-id="4c4db-304">**Varaktighet:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="4c4db-304">**Duration:** 20 Minutes</span></span> |
| <span data-ttu-id="4c4db-305">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="4c4db-305">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="4c4db-306">**Åtgärd:** minska antalet med 3</span><span class="sxs-lookup"><span data-stu-id="4c4db-306">**Action:** Decrease count by 3</span></span> |
| <span data-ttu-id="4c4db-307">**Kall ned (minuter):** 120</span><span class="sxs-lookup"><span data-stu-id="4c4db-307">**Cool down (minutes):** 120</span></span> |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
