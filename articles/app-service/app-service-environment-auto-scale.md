---
title: "V1 autoskalning och Apptjänst-miljö"
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
ms.openlocfilehash: f32affd285f3918feb0e893543f2a28f678b7b10
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="autoscaling-and-app-service-environment-v1"></a><span data-ttu-id="061e1-103">V1 autoskalning och Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="061e1-103">Autoscaling and App Service Environment v1</span></span>

> [!NOTE]
> <span data-ttu-id="061e1-104">Den här artikeln handlar om Apptjänstmiljö v1.</span><span class="sxs-lookup"><span data-stu-id="061e1-104">This article is about the App Service Environment v1.</span></span>  <span data-ttu-id="061e1-105">Det finns en nyare version av Apptjänst-miljön som är enklare att använda och körs på mer kraftfulla infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="061e1-105">There is a newer version of the App Service Environment that is easier  to use and runs on more powerful infrastructure.</span></span> <span data-ttu-id="061e1-106">Mer information om den nya versionen start med den [introduktion till Apptjänst-miljön](../app-service/app-service-environment/intro.md).</span><span class="sxs-lookup"><span data-stu-id="061e1-106">To learn more about the new version start with the [Introduction to the App  Service Environment](../app-service/app-service-environment/intro.md).</span></span>
> 

<span data-ttu-id="061e1-107">Stöd för Azure Apptjänst-miljöer *autoskalning*.</span><span class="sxs-lookup"><span data-stu-id="061e1-107">Azure App Service environments support *autoscaling*.</span></span> <span data-ttu-id="061e1-108">Du kan Autoskala enskilda arbetarpooler utifrån mått eller schema.</span><span class="sxs-lookup"><span data-stu-id="061e1-108">You can autoscale individual worker pools based on metrics or schedule.</span></span>

![Autoskala alternativ för en arbetspool.][intro]

<span data-ttu-id="061e1-110">Autoskalning optimerar användningen av dina resurser genom att automatiskt växande och minska storleken på en Apptjänst-miljö för att anpassa din budget och eller läsa in profilen.</span><span class="sxs-lookup"><span data-stu-id="061e1-110">Autoscaling optimizes your resource utilization by automatically growing and shrinking an App Service environment to fit your budget and or load profile.</span></span>

## <a name="configure-worker-pool-autoscale"></a><span data-ttu-id="061e1-111">Konfigurera worker poolen Autoskala</span><span class="sxs-lookup"><span data-stu-id="061e1-111">Configure worker pool autoscale</span></span>
<span data-ttu-id="061e1-112">Du kan komma åt Autoskala funktioner från den **inställningar** fliken för worker-pool.</span><span class="sxs-lookup"><span data-stu-id="061e1-112">You can access the autoscale functionality from the **Settings** tab of the worker pool.</span></span>

![Fliken Inställningar för worker-pool.][settings-scale]

<span data-ttu-id="061e1-114">Därifrån är gränssnittet ska vara ganska bekant eftersom den samma upplevelse som visas när du skalar en programtjänstplan.</span><span class="sxs-lookup"><span data-stu-id="061e1-114">From there, the interface should be fairly familiar since it is the same experience that you see when you scale an App Service plan.</span></span> 

![Inställningar för manuell skala.][scale-manual]

<span data-ttu-id="061e1-116">Du kan också konfigurera en profil för Autoskala.</span><span class="sxs-lookup"><span data-stu-id="061e1-116">You can also configure an autoscale profile.</span></span>

![Autoskala inställningar.][scale-profile]

<span data-ttu-id="061e1-118">Autoskalningsprofiler är användbara för att ange begränsningar för nivå.</span><span class="sxs-lookup"><span data-stu-id="061e1-118">Autoscale profiles are useful to set limits on your scale.</span></span> <span data-ttu-id="061e1-119">På så sätt kan du ha en konsekvent prestanda som uppstår genom att ange ett skalvärde som nedre gräns (1) och en förutsägbar utgifter fjärrskrivbordsanslutning genom att ange en övre gräns (2).</span><span class="sxs-lookup"><span data-stu-id="061e1-119">This way, you can have a consistent performance experience by setting a lower bound scale value (1) and a predictable spend cap by setting an upper bound (2).</span></span>

![Skala inställningarna i profilen.][scale-profile2]

<span data-ttu-id="061e1-121">När du definierar en profil kan du lägga till automatiska regler för att skala upp eller ned antalet instanser i arbetspool innanför kontrollens gränser som definierats av profilen.</span><span class="sxs-lookup"><span data-stu-id="061e1-121">After you define a profile, you can add autoscale rules to scale up or down the number of instances in the worker pool within the bounds defined by the profile.</span></span> <span data-ttu-id="061e1-122">Autoskala regler baserat på mått.</span><span class="sxs-lookup"><span data-stu-id="061e1-122">Autoscale rules are based on metrics.</span></span>

![Skalningsregeln.][scale-rule]

 <span data-ttu-id="061e1-124">Alla arbetspool eller frontend mått kan användas för att definiera automatiska regler.</span><span class="sxs-lookup"><span data-stu-id="061e1-124">Any worker pool or front-end metrics can be used to define autoscale rules.</span></span> <span data-ttu-id="061e1-125">De här måtten är samma mått som du kan övervaka i resursen bladet diagrammen eller Ställ in aviseringar för.</span><span class="sxs-lookup"><span data-stu-id="061e1-125">These metrics are the same metrics you can monitor in the resource blade graphs or set alerts for.</span></span>

## <a name="autoscale-example"></a><span data-ttu-id="061e1-126">Autoskala exempel</span><span class="sxs-lookup"><span data-stu-id="061e1-126">Autoscale example</span></span>
<span data-ttu-id="061e1-127">Autoskala för en Apptjänst-miljö kan bäst illustreras genom att gå via ett scenario.</span><span class="sxs-lookup"><span data-stu-id="061e1-127">Autoscale of an App Service environment can best be illustrated by walking through a scenario.</span></span>

<span data-ttu-id="061e1-128">Den här artikeln beskrivs alla nödvändiga överväganden när du ställer in autoskalning.</span><span class="sxs-lookup"><span data-stu-id="061e1-128">This article explains all the necessary considerations when you set up autoscale.</span></span> <span data-ttu-id="061e1-129">Artikeln vägleder dig genom de samspel som när du räkna in autoskalning apptjänstmiljöer som finns i Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="061e1-129">The article walks you through the interactions that come into play when you factor in autoscaling App Service environments that are hosted in App Service Environment.</span></span>

### <a name="scenario-introduction"></a><span data-ttu-id="061e1-130">Scenariot introduktion</span><span class="sxs-lookup"><span data-stu-id="061e1-130">Scenario introduction</span></span>
<span data-ttu-id="061e1-131">Frank är en sysadmin för ett företag som en del av arbetsbelastningarna som hanterar han har migrerats till en Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="061e1-131">Frank is a sysadmin for an enterprise who has migrated a portion of the workloads that he manages to an App Service environment.</span></span>

<span data-ttu-id="061e1-132">Apptjänst-miljö konfigureras för manuell skala på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="061e1-132">The App Service environment is configured to manual scale as follows:</span></span>

* <span data-ttu-id="061e1-133">**Främre slutar:** 3</span><span class="sxs-lookup"><span data-stu-id="061e1-133">**Front ends:** 3</span></span>
* <span data-ttu-id="061e1-134">**Arbetspool 1**: 10</span><span class="sxs-lookup"><span data-stu-id="061e1-134">**Worker pool 1**: 10</span></span>
* <span data-ttu-id="061e1-135">**Arbetspool 2**: 5</span><span class="sxs-lookup"><span data-stu-id="061e1-135">**Worker pool 2**: 5</span></span>
* <span data-ttu-id="061e1-136">**Arbetspool 3**: 5</span><span class="sxs-lookup"><span data-stu-id="061e1-136">**Worker pool 3**: 5</span></span>

<span data-ttu-id="061e1-137">Arbetspool 1 används för produktionsarbetsbelastningar medan arbetspool 2 och arbetspool 3 används för quality assurance (QA) och arbetsbelastningar för utveckling.</span><span class="sxs-lookup"><span data-stu-id="061e1-137">Worker pool 1 is used for production workloads, while worker pool 2 and worker pool 3 are used for quality assurance (QA) and development workloads.</span></span>

<span data-ttu-id="061e1-138">App Service-planer för QA och dev konfigureras för manuell skala.</span><span class="sxs-lookup"><span data-stu-id="061e1-138">The App Service plans for QA and dev are configured to manual scale.</span></span> <span data-ttu-id="061e1-139">Produktion App Service-plan anges Autoskala hantera variationer i belastningen och trafik.</span><span class="sxs-lookup"><span data-stu-id="061e1-139">The production App Service plan is set to autoscale to deal with variations in load and traffic.</span></span>

<span data-ttu-id="061e1-140">Frank är insatt i programmet.</span><span class="sxs-lookup"><span data-stu-id="061e1-140">Frank is very familiar with the application.</span></span> <span data-ttu-id="061e1-141">Han känner att användningsnivå för är mellan 9:00:00 och 18:00 eftersom detta är ett line-of-business (LOB)-program som de anställda använder medan de är på kontoret.</span><span class="sxs-lookup"><span data-stu-id="061e1-141">He knows that the peak hours for load are between 9:00 AM and 6:00 PM because this is a line-of-business (LOB) application that employees use while they are in the office.</span></span> <span data-ttu-id="061e1-142">Användning utelämnar efter när användare är klar för den dagen.</span><span class="sxs-lookup"><span data-stu-id="061e1-142">Usage drops after that when users are done for that day.</span></span> <span data-ttu-id="061e1-143">Utanför användningsnivå finns fortfarande vissa eftersom användare kan komma åt appen från en fjärrdator med hjälp av sina mobila enheter eller hemdatorer.</span><span class="sxs-lookup"><span data-stu-id="061e1-143">Outside peak hours, there is still some load because users can access the app remotely by using their mobile devices or home PCs.</span></span> <span data-ttu-id="061e1-144">Produktion App Service-plan har redan konfigurerats för att kunna Autoskala baserat på CPU-användning med följande regler:</span><span class="sxs-lookup"><span data-stu-id="061e1-144">The production App Service plan is already configured to autoscale based on CPU usage with the following rules:</span></span>

![Inställningar för LOB-app.][asp-scale]

| <span data-ttu-id="061e1-146">**Autoskala profil – veckodagar – App Service-plan**</span><span class="sxs-lookup"><span data-stu-id="061e1-146">**Autoscale profile – Weekdays – App Service plan**</span></span> | <span data-ttu-id="061e1-147">**Autoskala profil – helger – App Service-plan**</span><span class="sxs-lookup"><span data-stu-id="061e1-147">**Autoscale profile – Weekends – App Service plan**</span></span> |
| --- | --- |
| <span data-ttu-id="061e1-148">**Namn:** veckodag profil</span><span class="sxs-lookup"><span data-stu-id="061e1-148">**Name:** Weekday profile</span></span> |<span data-ttu-id="061e1-149">**Namn:** helg profil</span><span class="sxs-lookup"><span data-stu-id="061e1-149">**Name:** Weekend profile</span></span> |
| <span data-ttu-id="061e1-150">**Skala av:** schema-och prestanda</span><span class="sxs-lookup"><span data-stu-id="061e1-150">**Scale by:** Schedule and performance rules</span></span> |<span data-ttu-id="061e1-151">**Skala av:** schema-och prestanda</span><span class="sxs-lookup"><span data-stu-id="061e1-151">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="061e1-152">**Profil:** veckodagar</span><span class="sxs-lookup"><span data-stu-id="061e1-152">**Profile:** Weekdays</span></span> |<span data-ttu-id="061e1-153">**Profil:** lördag</span><span class="sxs-lookup"><span data-stu-id="061e1-153">**Profile:** Weekend</span></span> |
| <span data-ttu-id="061e1-154">**Typ:** upprepning</span><span class="sxs-lookup"><span data-stu-id="061e1-154">**Type:** Recurrence</span></span> |<span data-ttu-id="061e1-155">**Typ:** upprepning</span><span class="sxs-lookup"><span data-stu-id="061e1-155">**Type:** Recurrence</span></span> |
| <span data-ttu-id="061e1-156">**Målområde:** 5 till 20 instanser</span><span class="sxs-lookup"><span data-stu-id="061e1-156">**Target range:** 5 to 20 instances</span></span> |<span data-ttu-id="061e1-157">**Målområde:** 3 till 10 instanser</span><span class="sxs-lookup"><span data-stu-id="061e1-157">**Target range:** 3 to 10 instances</span></span> |
| <span data-ttu-id="061e1-158">**Dagar:** måndag, tisdag, onsdag, torsdag, fredag</span><span class="sxs-lookup"><span data-stu-id="061e1-158">**Days:** Monday, Tuesday, Wednesday, Thursday, Friday</span></span> |<span data-ttu-id="061e1-159">**Dagar:** lördag, söndag</span><span class="sxs-lookup"><span data-stu-id="061e1-159">**Days:** Saturday, Sunday</span></span> |
| <span data-ttu-id="061e1-160">**Starttid:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="061e1-160">**Start time:** 9:00 AM</span></span> |<span data-ttu-id="061e1-161">**Starttid:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="061e1-161">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="061e1-162">**Tidszon:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="061e1-162">**Time zone:** UTC-08</span></span> |<span data-ttu-id="061e1-163">**Tidszon:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="061e1-163">**Time zone:** UTC-08</span></span> |
|  | |
| <span data-ttu-id="061e1-164">**Autoskala regeln (skala upp)**</span><span class="sxs-lookup"><span data-stu-id="061e1-164">**Autoscale rule (Scale Up)**</span></span> |<span data-ttu-id="061e1-165">**Autoskala regeln (skala upp)**</span><span class="sxs-lookup"><span data-stu-id="061e1-165">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="061e1-166">**Resurs:** produktion (Apptjänst-miljö)</span><span class="sxs-lookup"><span data-stu-id="061e1-166">**Resource:** Production (App Service Environment)</span></span> |<span data-ttu-id="061e1-167">**Resurs:** produktion (Apptjänst-miljö)</span><span class="sxs-lookup"><span data-stu-id="061e1-167">**Resource:** Production (App Service Environment)</span></span> |
| <span data-ttu-id="061e1-168">**Mått:** CPU i %</span><span class="sxs-lookup"><span data-stu-id="061e1-168">**Metric:** CPU %</span></span> |<span data-ttu-id="061e1-169">**Mått:** CPU i %</span><span class="sxs-lookup"><span data-stu-id="061e1-169">**Metric:** CPU %</span></span> |
| <span data-ttu-id="061e1-170">**Åtgärden:** större än 60%</span><span class="sxs-lookup"><span data-stu-id="061e1-170">**Operation:** Greater than 60%</span></span> |<span data-ttu-id="061e1-171">**Åtgärden:** större än 80%</span><span class="sxs-lookup"><span data-stu-id="061e1-171">**Operation:** Greater than 80%</span></span> |
| <span data-ttu-id="061e1-172">**Varaktighet:** 5 minuter</span><span class="sxs-lookup"><span data-stu-id="061e1-172">**Duration:** 5 Minutes</span></span> |<span data-ttu-id="061e1-173">**Varaktighet:** 10 minuter</span><span class="sxs-lookup"><span data-stu-id="061e1-173">**Duration:** 10 Minutes</span></span> |
| <span data-ttu-id="061e1-174">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="061e1-174">**Time aggregation:** Average</span></span> |<span data-ttu-id="061e1-175">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="061e1-175">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="061e1-176">**Åtgärd:** öka antalet av 2</span><span class="sxs-lookup"><span data-stu-id="061e1-176">**Action:** Increase count by 2</span></span> |<span data-ttu-id="061e1-177">**Åtgärd:** öka antalet med 1</span><span class="sxs-lookup"><span data-stu-id="061e1-177">**Action:** Increase count by 1</span></span> |
| <span data-ttu-id="061e1-178">**Kall ned (minuter):** 15</span><span class="sxs-lookup"><span data-stu-id="061e1-178">**Cool down (minutes):** 15</span></span> |<span data-ttu-id="061e1-179">**Kall ned (minuter):** 20</span><span class="sxs-lookup"><span data-stu-id="061e1-179">**Cool down (minutes):** 20</span></span> |
|  | |
| <span data-ttu-id="061e1-180">**Autoskala regeln (skala ned)**</span><span class="sxs-lookup"><span data-stu-id="061e1-180">**Autoscale rule (Scale Down)**</span></span> |<span data-ttu-id="061e1-181">**Autoskala regeln (skala ned)**</span><span class="sxs-lookup"><span data-stu-id="061e1-181">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="061e1-182">**Resurs:** produktion (Apptjänst-miljö)</span><span class="sxs-lookup"><span data-stu-id="061e1-182">**Resource:** Production (App Service Environment)</span></span> |<span data-ttu-id="061e1-183">**Resurs:** produktion (Apptjänst-miljö)</span><span class="sxs-lookup"><span data-stu-id="061e1-183">**Resource:** Production (App Service Environment)</span></span> |
| <span data-ttu-id="061e1-184">**Mått:** CPU i %</span><span class="sxs-lookup"><span data-stu-id="061e1-184">**Metric:** CPU %</span></span> |<span data-ttu-id="061e1-185">**Mått:** CPU i %</span><span class="sxs-lookup"><span data-stu-id="061e1-185">**Metric:** CPU %</span></span> |
| <span data-ttu-id="061e1-186">**Åtgärden:** mindre än 30%</span><span class="sxs-lookup"><span data-stu-id="061e1-186">**Operation:** Less than 30%</span></span> |<span data-ttu-id="061e1-187">**Åtgärden:** mindre än 20%</span><span class="sxs-lookup"><span data-stu-id="061e1-187">**Operation:** Less than 20%</span></span> |
| <span data-ttu-id="061e1-188">**Varaktighet:** 10 minuter</span><span class="sxs-lookup"><span data-stu-id="061e1-188">**Duration:** 10 minutes</span></span> |<span data-ttu-id="061e1-189">**Varaktighet:** 15 minuter</span><span class="sxs-lookup"><span data-stu-id="061e1-189">**Duration:** 15 minutes</span></span> |
| <span data-ttu-id="061e1-190">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="061e1-190">**Time aggregation:** Average</span></span> |<span data-ttu-id="061e1-191">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="061e1-191">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="061e1-192">**Åtgärd:** minska antalet med 1</span><span class="sxs-lookup"><span data-stu-id="061e1-192">**Action:** Decrease count by 1</span></span> |<span data-ttu-id="061e1-193">**Åtgärd:** minska antalet med 1</span><span class="sxs-lookup"><span data-stu-id="061e1-193">**Action:** Decrease count by 1</span></span> |
| <span data-ttu-id="061e1-194">**Kall ned (minuter):** 20</span><span class="sxs-lookup"><span data-stu-id="061e1-194">**Cool down (minutes):** 20</span></span> |<span data-ttu-id="061e1-195">**Kall ned (minuter):** 10</span><span class="sxs-lookup"><span data-stu-id="061e1-195">**Cool down (minutes):** 10</span></span> |

### <a name="app-service-plan-inflation-rate"></a><span data-ttu-id="061e1-196">App Service-plan inflationen hastighet</span><span class="sxs-lookup"><span data-stu-id="061e1-196">App Service plan inflation rate</span></span>
<span data-ttu-id="061e1-197">Programtjänstplaner som har konfigurerats för att kunna Autoskala gör det på en högsta överföringshastighet per timme.</span><span class="sxs-lookup"><span data-stu-id="061e1-197">App Service plans that are configured to autoscale do so at a maximum rate per hour.</span></span> <span data-ttu-id="061e1-198">Detta värde kan beräknas baserat på de värden som anges på Autoskala regeln.</span><span class="sxs-lookup"><span data-stu-id="061e1-198">This rate can be calculated based on the values provided on the autoscale rule.</span></span>

<span data-ttu-id="061e1-199">Förstå och beräkna den *App Service-plan inflationen hastighet* är viktigt för Apptjänst-miljö Autoskala eftersom skala ändringar i en arbetspool inte omedelbar.</span><span class="sxs-lookup"><span data-stu-id="061e1-199">Understanding and calculating the *App Service plan inflation rate* is important for App Service environment autoscale because scale changes to a worker pool are not instantaneous.</span></span>

<span data-ttu-id="061e1-200">App Service-plan inflationen satsen beräknas enligt följande:</span><span class="sxs-lookup"><span data-stu-id="061e1-200">The App Service plan inflation rate is calculated as follows:</span></span>

![App Service-plan inflationen beräkning.][ASP-Inflation]

<span data-ttu-id="061e1-202">Baserat på Autoskala – skala upp regel för produktion programtjänstplanen veckodag profil:</span><span class="sxs-lookup"><span data-stu-id="061e1-202">Based on the Autoscale – Scale Up rule for the Weekday profile of the production App Service plan:</span></span>

![App Service-plan inflationen hastighet för veckodagar baserat på Autoskala – skala upp regeln.][Equation1]

<span data-ttu-id="061e1-204">När det gäller Autoskala – skala upp regel för lördag profilen för produktion App Service-plan löser formeln till:</span><span class="sxs-lookup"><span data-stu-id="061e1-204">In the case of the Autoscale – Scale Up rule for the Weekend profile of the production App Service plan, the formula would resolve to:</span></span>

![App Service-plan inflationen hastighet för helger baserat på Autoskala – skala upp regeln.][Equation2]

<span data-ttu-id="061e1-206">Det här värdet kan också beräknas för skala ned åtgärder.</span><span class="sxs-lookup"><span data-stu-id="061e1-206">This value can also be calculated for scale-down operations.</span></span>

<span data-ttu-id="061e1-207">Baserat på Autoskala – skala ned regel för profilen veckodag för produktion App Service-plan detta skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="061e1-207">Based on the Autoscale – Scale Down rule for the Weekday profile of the production App Service plan, this would look as follows:</span></span>

![App Service-plan inflationen hastighet för veckodagar baserat på Autoskala – skala ned regeln.][Equation3]

<span data-ttu-id="061e1-209">När det gäller Autoskala – skala ned regel för lördag profilen för produktion App Service-plan löser formeln till:</span><span class="sxs-lookup"><span data-stu-id="061e1-209">In the case of the Autoscale – Scale Down rule for the Weekend profile of the production App Service plan, the formula would resolve to:</span></span>  

![App Service-plan inflationen hastighet för helger baserat på Autoskala – skala ned regeln.][Equation4]

<span data-ttu-id="061e1-211">Produktion App Service-plan kan växa på högst åtta instanser per timme under vecka och fyra instanser per timme när det är helgen.</span><span class="sxs-lookup"><span data-stu-id="061e1-211">The production App Service plan can grow at a maximum rate of eight instances/hour during the week and four instances/hour during the weekend.</span></span> <span data-ttu-id="061e1-212">Det kan släppa instanser på högst fyra instanser per timme under vecka och sex instanser per timme under helger.</span><span class="sxs-lookup"><span data-stu-id="061e1-212">It can release instances at a maximum rate of four instances/hour during the week and six instances/hour during weekends.</span></span>

<span data-ttu-id="061e1-213">Om flera App Service-planer finns i en arbetspool, måste du beräkna den *totala inflationen hastigheten* som summan av inflationen hastighet för alla programtjänstplaner som värd i den poolen, worker.</span><span class="sxs-lookup"><span data-stu-id="061e1-213">If multiple App Service plans are being hosted in a worker pool, you have to calculate the *total inflation rate* as the sum of the inflation rate for all the App Service plans that are being hosting in that worker pool.</span></span>

![Beräkning av totala inflationen hastigheten för flera App Service-planer finns i en arbetspool.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a><span data-ttu-id="061e1-215">Använda App Service-plan inflationen hastighet för att definiera worker poolen Autoskala regler</span><span class="sxs-lookup"><span data-stu-id="061e1-215">Use the App Service plan inflation rate to define worker pool autoscale rules</span></span>
<span data-ttu-id="061e1-216">Worker pooler som är värdar för App Service-planer som har konfigurerats för att kunna Autoskala behöver att allokera en buffert av kapacitet.</span><span class="sxs-lookup"><span data-stu-id="061e1-216">Worker pools that host App Service plans that are configured to autoscale need to be allocated a buffer of capacity.</span></span> <span data-ttu-id="061e1-217">Bufferten kan åtgärderna Autoskala att växa eller krympa programtjänstplanen efter behov.</span><span class="sxs-lookup"><span data-stu-id="061e1-217">The buffer allows for the autoscale operations to grow and shrink the App Service plan as needed.</span></span> <span data-ttu-id="061e1-218">Minsta bufferten är beräknade App Service planera inflationen summan.</span><span class="sxs-lookup"><span data-stu-id="061e1-218">The minimum buffer would be the calculated Total App Service Plan Inflation Rate.</span></span>

<span data-ttu-id="061e1-219">Eftersom skalningsåtgärder för Apptjänst-miljö ta lite tid att tillämpa, bör ändringar konto för ytterligare begäran ändringar som kan inträffa när en skalningsåtgärd pågår.</span><span class="sxs-lookup"><span data-stu-id="061e1-219">Because App Service environment scale operations take some time to apply, any change should account for further demand changes that could happen while a scale operation is in progress.</span></span> <span data-ttu-id="061e1-220">För att tillgodose det här svarstid, rekommenderar vi att du använder den beräknade App Service planera inflationen frekvens som det minsta antalet instanser som har lagts till för varje Autoskala-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="061e1-220">To accommodate this latency, we recommend that you use the calculated Total App Service Plan Inflation Rate as the minimum number of instances that are added for each autoscale operation.</span></span>

<span data-ttu-id="061e1-221">Med den här informationen kan du definiera följande Autoskala profil och regler Frank:</span><span class="sxs-lookup"><span data-stu-id="061e1-221">With this information, Frank can define the following autoscale profile and rules:</span></span>

![Autoskala profil regler för LOB-exempel.][Worker-Pool-Scale]

| <span data-ttu-id="061e1-223">**Autoskala profil – veckodagar**</span><span class="sxs-lookup"><span data-stu-id="061e1-223">**Autoscale profile – Weekdays**</span></span> | <span data-ttu-id="061e1-224">**Autoskala profil – söndag**</span><span class="sxs-lookup"><span data-stu-id="061e1-224">**Autoscale profile – Weekends**</span></span> |
| --- | --- |
| <span data-ttu-id="061e1-225">**Namn:** veckodag profil</span><span class="sxs-lookup"><span data-stu-id="061e1-225">**Name:** Weekday profile</span></span> |<span data-ttu-id="061e1-226">**Namn:** helg profil</span><span class="sxs-lookup"><span data-stu-id="061e1-226">**Name:** Weekend profile</span></span> |
| <span data-ttu-id="061e1-227">**Skala av:** schema-och prestanda</span><span class="sxs-lookup"><span data-stu-id="061e1-227">**Scale by:** Schedule and performance rules</span></span> |<span data-ttu-id="061e1-228">**Skala av:** schema-och prestanda</span><span class="sxs-lookup"><span data-stu-id="061e1-228">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="061e1-229">**Profil:** veckodagar</span><span class="sxs-lookup"><span data-stu-id="061e1-229">**Profile:** Weekdays</span></span> |<span data-ttu-id="061e1-230">**Profil:** lördag</span><span class="sxs-lookup"><span data-stu-id="061e1-230">**Profile:** Weekend</span></span> |
| <span data-ttu-id="061e1-231">**Typ:** upprepning</span><span class="sxs-lookup"><span data-stu-id="061e1-231">**Type:** Recurrence</span></span> |<span data-ttu-id="061e1-232">**Typ:** upprepning</span><span class="sxs-lookup"><span data-stu-id="061e1-232">**Type:** Recurrence</span></span> |
| <span data-ttu-id="061e1-233">**Målområde:** 13 25 instanser</span><span class="sxs-lookup"><span data-stu-id="061e1-233">**Target range:** 13 to 25 instances</span></span> |<span data-ttu-id="061e1-234">**Målområde:** 6-15 instanser</span><span class="sxs-lookup"><span data-stu-id="061e1-234">**Target range:** 6 to 15 instances</span></span> |
| <span data-ttu-id="061e1-235">**Dagar:** måndag, tisdag, onsdag, torsdag, fredag</span><span class="sxs-lookup"><span data-stu-id="061e1-235">**Days:** Monday, Tuesday, Wednesday, Thursday, Friday</span></span> |<span data-ttu-id="061e1-236">**Dagar:** lördag, söndag</span><span class="sxs-lookup"><span data-stu-id="061e1-236">**Days:** Saturday, Sunday</span></span> |
| <span data-ttu-id="061e1-237">**Starttid:** 7:00:00</span><span class="sxs-lookup"><span data-stu-id="061e1-237">**Start time:** 7:00 AM</span></span> |<span data-ttu-id="061e1-238">**Starttid:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="061e1-238">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="061e1-239">**Tidszon:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="061e1-239">**Time zone:** UTC-08</span></span> |<span data-ttu-id="061e1-240">**Tidszon:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="061e1-240">**Time zone:** UTC-08</span></span> |
|  | |
| <span data-ttu-id="061e1-241">**Autoskala regeln (skala upp)**</span><span class="sxs-lookup"><span data-stu-id="061e1-241">**Autoscale rule (Scale Up)**</span></span> |<span data-ttu-id="061e1-242">**Autoskala regeln (skala upp)**</span><span class="sxs-lookup"><span data-stu-id="061e1-242">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="061e1-243">**Resurs:** arbetspool 1</span><span class="sxs-lookup"><span data-stu-id="061e1-243">**Resource:** Worker pool 1</span></span> |<span data-ttu-id="061e1-244">**Resurs:** arbetspool 1</span><span class="sxs-lookup"><span data-stu-id="061e1-244">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="061e1-245">**Mått:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="061e1-245">**Metric:** WorkersAvailable</span></span> |<span data-ttu-id="061e1-246">**Mått:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="061e1-246">**Metric:** WorkersAvailable</span></span> |
| <span data-ttu-id="061e1-247">**Åtgärden:** mindre än 8</span><span class="sxs-lookup"><span data-stu-id="061e1-247">**Operation:** Less than 8</span></span> |<span data-ttu-id="061e1-248">**Åtgärden:** färre än 3</span><span class="sxs-lookup"><span data-stu-id="061e1-248">**Operation:** Less than 3</span></span> |
| <span data-ttu-id="061e1-249">**Varaktighet:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="061e1-249">**Duration:** 20 minutes</span></span> |<span data-ttu-id="061e1-250">**Varaktighet:** 30 minuter</span><span class="sxs-lookup"><span data-stu-id="061e1-250">**Duration:** 30 minutes</span></span> |
| <span data-ttu-id="061e1-251">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="061e1-251">**Time aggregation:** Average</span></span> |<span data-ttu-id="061e1-252">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="061e1-252">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="061e1-253">**Åtgärd:** öka antalet med 8</span><span class="sxs-lookup"><span data-stu-id="061e1-253">**Action:** Increase count by 8</span></span> |<span data-ttu-id="061e1-254">**Åtgärd:** öka antalet med 3</span><span class="sxs-lookup"><span data-stu-id="061e1-254">**Action:** Increase count by 3</span></span> |
| <span data-ttu-id="061e1-255">**Kall ned (minuter):** 180</span><span class="sxs-lookup"><span data-stu-id="061e1-255">**Cool down (minutes):** 180</span></span> |<span data-ttu-id="061e1-256">**Kall ned (minuter):** 180</span><span class="sxs-lookup"><span data-stu-id="061e1-256">**Cool down (minutes):** 180</span></span> |
|  | |
| <span data-ttu-id="061e1-257">**Autoskala regeln (skala ned)**</span><span class="sxs-lookup"><span data-stu-id="061e1-257">**Autoscale rule (Scale Down)**</span></span> |<span data-ttu-id="061e1-258">**Autoskala regeln (skala ned)**</span><span class="sxs-lookup"><span data-stu-id="061e1-258">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="061e1-259">**Resurs:** arbetspool 1</span><span class="sxs-lookup"><span data-stu-id="061e1-259">**Resource:** Worker pool 1</span></span> |<span data-ttu-id="061e1-260">**Resurs:** arbetspool 1</span><span class="sxs-lookup"><span data-stu-id="061e1-260">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="061e1-261">**Mått:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="061e1-261">**Metric:** WorkersAvailable</span></span> |<span data-ttu-id="061e1-262">**Mått:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="061e1-262">**Metric:** WorkersAvailable</span></span> |
| <span data-ttu-id="061e1-263">**Åtgärden:** större än 8</span><span class="sxs-lookup"><span data-stu-id="061e1-263">**Operation:** Greater than 8</span></span> |<span data-ttu-id="061e1-264">**Åtgärden:** större än 3</span><span class="sxs-lookup"><span data-stu-id="061e1-264">**Operation:** Greater than 3</span></span> |
| <span data-ttu-id="061e1-265">**Varaktighet:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="061e1-265">**Duration:** 20 minutes</span></span> |<span data-ttu-id="061e1-266">**Varaktighet:** 15 minuter</span><span class="sxs-lookup"><span data-stu-id="061e1-266">**Duration:** 15 minutes</span></span> |
| <span data-ttu-id="061e1-267">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="061e1-267">**Time aggregation:** Average</span></span> |<span data-ttu-id="061e1-268">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="061e1-268">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="061e1-269">**Åtgärd:** minska antalet med 2</span><span class="sxs-lookup"><span data-stu-id="061e1-269">**Action:** Decrease count by 2</span></span> |<span data-ttu-id="061e1-270">**Åtgärd:** minska antalet med 3</span><span class="sxs-lookup"><span data-stu-id="061e1-270">**Action:** Decrease count by 3</span></span> |
| <span data-ttu-id="061e1-271">**Kall ned (minuter):** 120</span><span class="sxs-lookup"><span data-stu-id="061e1-271">**Cool down (minutes):** 120</span></span> |<span data-ttu-id="061e1-272">**Kall ned (minuter):** 120</span><span class="sxs-lookup"><span data-stu-id="061e1-272">**Cool down (minutes):** 120</span></span> |

<span data-ttu-id="061e1-273">Mål-intervallet som definierats i profilen beräknas genom att de minsta instanser som anges i profilen för App Service-plan + buffert.</span><span class="sxs-lookup"><span data-stu-id="061e1-273">The Target range defined in the profile is calculated by the minimum instances defined in the profile for the App Service plan + buffer.</span></span>

<span data-ttu-id="061e1-274">Maximalt intervall är summan av alla maximalt intervall för alla App Service-planer finns i poolen worker.</span><span class="sxs-lookup"><span data-stu-id="061e1-274">The Maximum range would be the sum of all the maximum ranges for all App Service plans hosted in the worker pool.</span></span>

<span data-ttu-id="061e1-275">Öka antalet för skalan regler ska konfigureras med minst 1 X-App Service-Plan inflationen hastighet för skalan.</span><span class="sxs-lookup"><span data-stu-id="061e1-275">The Increase count for the scale up rules should be set to at least 1X the App Service Plan Inflation Rate for scale up.</span></span>

<span data-ttu-id="061e1-276">Minska antalet kan justeras till något mellan 1/2 X eller 1 X-App Service Plan inflationen hastighet för att skala ned.</span><span class="sxs-lookup"><span data-stu-id="061e1-276">Decrease count can be adjusted to something between 1/2X or 1X the App Service Plan Inflation Rate for scale down.</span></span>

### <a name="autoscale-for-front-end-pool"></a><span data-ttu-id="061e1-277">Autoskala för frontend-pool</span><span class="sxs-lookup"><span data-stu-id="061e1-277">Autoscale for front-end pool</span></span>
<span data-ttu-id="061e1-278">Regler för frontend Autoskala är enklare än för arbetarpooler.</span><span class="sxs-lookup"><span data-stu-id="061e1-278">Rules for front-end autoscale are simpler than for worker pools.</span></span> <span data-ttu-id="061e1-279">Främst bör du</span><span class="sxs-lookup"><span data-stu-id="061e1-279">Primarily, you should</span></span>  
<span data-ttu-id="061e1-280">Kontrollera att beakta varaktighet för mätning och cooldown timers skalningsåtgärder på en apptjänstplan inte är omedelbar.</span><span class="sxs-lookup"><span data-stu-id="061e1-280">make sure that duration of the measurement and the cooldown timers consider that scale operations on an App Service plan are not instantaneous.</span></span>

<span data-ttu-id="061e1-281">I det här scenariot vet Frank att Felfrekvensen ökar när frontwebbservrarna når 80% CPU-användning och anger Autoskala regeln för att öka instanser på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="061e1-281">For this scenario, Frank knows that the error rate increases after front ends reach 80% CPU utilization and sets the autoscale rule to increase instances as follows:</span></span>

![Autoskala inställningar för frontend-pool.][Front-End-Scale]

| <span data-ttu-id="061e1-283">**Autoskala profil – framför slutar**</span><span class="sxs-lookup"><span data-stu-id="061e1-283">**Autoscale profile – Front ends**</span></span> |
| --- |
| <span data-ttu-id="061e1-284">**Namn:** Autoskala – framför slutar</span><span class="sxs-lookup"><span data-stu-id="061e1-284">**Name:** Autoscale – Front ends</span></span> |
| <span data-ttu-id="061e1-285">**Skala av:** schema-och prestanda</span><span class="sxs-lookup"><span data-stu-id="061e1-285">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="061e1-286">**Profil:** varje dag</span><span class="sxs-lookup"><span data-stu-id="061e1-286">**Profile:** Everyday</span></span> |
| <span data-ttu-id="061e1-287">**Typ:** upprepning</span><span class="sxs-lookup"><span data-stu-id="061e1-287">**Type:** Recurrence</span></span> |
| <span data-ttu-id="061e1-288">**Målområde:** 3 till 10 instanser</span><span class="sxs-lookup"><span data-stu-id="061e1-288">**Target range:** 3 to 10 instances</span></span> |
| <span data-ttu-id="061e1-289">**Dagar:** varje dag</span><span class="sxs-lookup"><span data-stu-id="061e1-289">**Days:** Everyday</span></span> |
| <span data-ttu-id="061e1-290">**Starttid:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="061e1-290">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="061e1-291">**Tidszon:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="061e1-291">**Time zone:** UTC-08</span></span> |
|  |
| <span data-ttu-id="061e1-292">**Autoskala regeln (skala upp)**</span><span class="sxs-lookup"><span data-stu-id="061e1-292">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="061e1-293">**Resurs:** frontend-pool</span><span class="sxs-lookup"><span data-stu-id="061e1-293">**Resource:** Front-end pool</span></span> |
| <span data-ttu-id="061e1-294">**Mått:** CPU i %</span><span class="sxs-lookup"><span data-stu-id="061e1-294">**Metric:** CPU %</span></span> |
| <span data-ttu-id="061e1-295">**Åtgärden:** större än 60%</span><span class="sxs-lookup"><span data-stu-id="061e1-295">**Operation:** Greater than 60%</span></span> |
| <span data-ttu-id="061e1-296">**Varaktighet:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="061e1-296">**Duration:** 20 minutes</span></span> |
| <span data-ttu-id="061e1-297">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="061e1-297">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="061e1-298">**Åtgärd:** öka antalet med 3</span><span class="sxs-lookup"><span data-stu-id="061e1-298">**Action:** Increase count by 3</span></span> |
| <span data-ttu-id="061e1-299">**Kall ned (minuter):** 120</span><span class="sxs-lookup"><span data-stu-id="061e1-299">**Cool down (minutes):** 120</span></span> |
|  |
| <span data-ttu-id="061e1-300">**Autoskala regeln (skala ned)**</span><span class="sxs-lookup"><span data-stu-id="061e1-300">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="061e1-301">**Resurs:** arbetspool 1</span><span class="sxs-lookup"><span data-stu-id="061e1-301">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="061e1-302">**Mått:** CPU i %</span><span class="sxs-lookup"><span data-stu-id="061e1-302">**Metric:** CPU %</span></span> |
| <span data-ttu-id="061e1-303">**Åtgärden:** mindre än 30%</span><span class="sxs-lookup"><span data-stu-id="061e1-303">**Operation:** Less than 30%</span></span> |
| <span data-ttu-id="061e1-304">**Varaktighet:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="061e1-304">**Duration:** 20 Minutes</span></span> |
| <span data-ttu-id="061e1-305">**Tid aggregering:** Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="061e1-305">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="061e1-306">**Åtgärd:** minska antalet med 3</span><span class="sxs-lookup"><span data-stu-id="061e1-306">**Action:** Decrease count by 3</span></span> |
| <span data-ttu-id="061e1-307">**Kall ned (minuter):** 120</span><span class="sxs-lookup"><span data-stu-id="061e1-307">**Cool down (minutes):** 120</span></span> |

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
