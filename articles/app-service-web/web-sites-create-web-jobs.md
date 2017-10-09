---
title: aaaRun bakgrundsaktiviteter med WebJobs
description: "Lär dig hur toorun bakgrundsaktiviteter i Azure-webbappar."
services: app-service
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2016
ms.author: glenga
ms.openlocfilehash: 96a3d977a806e7192207f0f4da79dfd248694336
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-background-tasks-with-webjobs"></a><span data-ttu-id="70db5-103">Kör bakgrundsuppgifter med WebJobs</span><span class="sxs-lookup"><span data-stu-id="70db5-103">Run Background tasks with WebJobs</span></span>
## <a name="overview"></a><span data-ttu-id="70db5-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="70db5-104">Overview</span></span>
<span data-ttu-id="70db5-105">Du kan köra program eller skript i WebJobs i din [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webbprogrammet på tre sätt: på begäran, kontinuerligt, eller enligt ett schema.</span><span class="sxs-lookup"><span data-stu-id="70db5-105">You can run programs or scripts in WebJobs in your [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) web app in three ways: on demand, continuously, or on a schedule.</span></span> <span data-ttu-id="70db5-106">Det finns inga extra kostnad toouse WebJobs.</span><span class="sxs-lookup"><span data-stu-id="70db5-106">There is no additional cost toouse WebJobs.</span></span>

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

<span data-ttu-id="70db5-107">Den här artikeln visar hur toodeploy WebJobs med hjälp av hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="70db5-107">This article shows how toodeploy WebJobs by using hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="70db5-108">Information om hur toodeploy med hjälp av Visual Studio eller en kontinuerlig distributionsprocessen finns [hur tooDeploy Azure WebJobs tooWeb appar](websites-dotnet-deploy-webjobs.md).</span><span class="sxs-lookup"><span data-stu-id="70db5-108">For information about how toodeploy by using Visual Studio or a continuous delivery process, see [How tooDeploy Azure WebJobs tooWeb Apps](websites-dotnet-deploy-webjobs.md).</span></span>

<span data-ttu-id="70db5-109">hello Azure WebJobs SDK förenklar många WebJobs-programmeringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="70db5-109">hello Azure WebJobs SDK simplifies many WebJobs programming tasks.</span></span> <span data-ttu-id="70db5-110">Mer information finns i [vad är hello WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="70db5-110">For more information, see [What is hello WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span></span>

 <span data-ttu-id="70db5-111">Azure Functions erbjuder ett annat sätt toorun program och skript från en serverlösa miljö eller från en Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="70db5-111">Azure Functions provides another way toorun programs and scripts from either a serverless environment or from an App Service app.</span></span> <span data-ttu-id="70db5-112">Mer information finns i [översikt över Azure Functions](../azure-functions/functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="70db5-112">For more information, see [Azure Functions overview](../azure-functions/functions-overview.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="70db5-113"><a name="acceptablefiles"></a>Filtyper för skript eller program</span><span class="sxs-lookup"><span data-stu-id="70db5-113"><a name="acceptablefiles"></a>Acceptable file types for scripts or programs</span></span>
<span data-ttu-id="70db5-114">följande filtyper hello accepteras:</span><span class="sxs-lookup"><span data-stu-id="70db5-114">hello following file types are accepted:</span></span>

* <span data-ttu-id="70db5-115">.cmd, .bat, .exe (med windows cmd)</span><span class="sxs-lookup"><span data-stu-id="70db5-115">.cmd, .bat, .exe (using windows cmd)</span></span>
* <span data-ttu-id="70db5-116">.ps1 (med powershell)</span><span class="sxs-lookup"><span data-stu-id="70db5-116">.ps1 (using powershell)</span></span>
* <span data-ttu-id="70db5-117">.SH (med bash)</span><span class="sxs-lookup"><span data-stu-id="70db5-117">.sh (using bash)</span></span>
* <span data-ttu-id="70db5-118">.php (med php)</span><span class="sxs-lookup"><span data-stu-id="70db5-118">.php (using php)</span></span>
* <span data-ttu-id="70db5-119">.PY (med python)</span><span class="sxs-lookup"><span data-stu-id="70db5-119">.py (using python)</span></span>
* <span data-ttu-id="70db5-120">.js (med nod)</span><span class="sxs-lookup"><span data-stu-id="70db5-120">.js (using node)</span></span>
* <span data-ttu-id="70db5-121">.JAR (med java)</span><span class="sxs-lookup"><span data-stu-id="70db5-121">.jar (using java)</span></span>

## <span data-ttu-id="70db5-122"><a name="CreateOnDemand"></a>Skapa en på begäran Webbjobb hello-portalen</span><span class="sxs-lookup"><span data-stu-id="70db5-122"><a name="CreateOnDemand"></a>Create an on demand WebJob in hello portal</span></span>
1. <span data-ttu-id="70db5-123">I hello **Webbapp** bladet för hello [Azure Portal](https://portal.azure.com), klickar du på **alla inställningar > WebJobs** tooshow hello **WebJobs** bladet.</span><span class="sxs-lookup"><span data-stu-id="70db5-123">In hello **Web App** blade of hello [Azure Portal](https://portal.azure.com), click **All settings > WebJobs** tooshow hello **WebJobs** blade.</span></span>
   
    ![Webbjobbet bladet](./media/web-sites-create-web-jobs/wjblade.png)
2. <span data-ttu-id="70db5-125">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="70db5-125">Click **Add**.</span></span> <span data-ttu-id="70db5-126">Hej **lägger till Webbjobb** visas.</span><span class="sxs-lookup"><span data-stu-id="70db5-126">hello **Add WebJob** dialog appears.</span></span>
   
    ![Webbjobbet bladet Lägg till](./media/web-sites-create-web-jobs/addwjblade.png)
3. <span data-ttu-id="70db5-128">Under **namn**, ange ett namn för hello Webbjobb.</span><span class="sxs-lookup"><span data-stu-id="70db5-128">Under **Name**, provide a name for hello WebJob.</span></span> <span data-ttu-id="70db5-129">hello-namnet måste börja med en bokstav eller en siffra och får inte innehålla specialtecken än ”-” och ”_”.</span><span class="sxs-lookup"><span data-stu-id="70db5-129">hello name must start with a letter or a number and cannot contain any special characters other than "-" and "_".</span></span>
4. <span data-ttu-id="70db5-130">I hello **hur tooRun** väljer **körs på begäran**.</span><span class="sxs-lookup"><span data-stu-id="70db5-130">In hello **How tooRun** box, choose **Run on Demand**.</span></span>
5. <span data-ttu-id="70db5-131">I hello **filöverföring** rutan och klicka på ikonen för hello mapp Bläddra toohello zip-fil som innehåller skriptet.</span><span class="sxs-lookup"><span data-stu-id="70db5-131">In hello **File Upload** box, click hello folder icon and browse toohello zip file that contains your script.</span></span> <span data-ttu-id="70db5-132">hello zip-filen ska innehålla den körbara filen (.exe .cmd .bat .sh .php .py .js) samt alla stödfiler behövs toorun hello program eller skript.</span><span class="sxs-lookup"><span data-stu-id="70db5-132">hello zip file should contain your executable (.exe .cmd .bat .sh .php .py .js) as well as any supporting files needed toorun hello program or script.</span></span>
6. <span data-ttu-id="70db5-133">Kontrollera **skapa** tooupload hello skriptet tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="70db5-133">Check **Create** tooupload hello script tooyour web app.</span></span> 
   
    <span data-ttu-id="70db5-134">hello namn du angav för hello Webbjobbet visas i listan hello på hello **WebJobs** bladet.</span><span class="sxs-lookup"><span data-stu-id="70db5-134">hello name you specified for hello WebJob appears in hello list on hello **WebJobs** blade.</span></span>
7. <span data-ttu-id="70db5-135">toorun hello Webbjobb, högerklickar du på namnet i hello listan och klicka på **kör**.</span><span class="sxs-lookup"><span data-stu-id="70db5-135">toorun hello WebJob, right-click its name in hello list and click **Run**.</span></span>
   
    ![Köra Webbjobbet](./media/web-sites-create-web-jobs/runondemand.png)

## <span data-ttu-id="70db5-137"><a name="CreateContinuous"></a>Skapa ett körs kontinuerligt Webbjobb</span><span class="sxs-lookup"><span data-stu-id="70db5-137"><a name="CreateContinuous"></a>Create a continuously running WebJob</span></span>
1. <span data-ttu-id="70db5-138">toocreate ett Webbjobb som körs kontinuerligt följa samma steg för att skapa ett Webbjobb som körs en gång, men i hello hello **hur tooRun** väljer **kontinuerlig**.</span><span class="sxs-lookup"><span data-stu-id="70db5-138">toocreate a continuously executing WebJob, follow hello same steps for creating a WebJob that runs once, but in hello **How tooRun** box, choose **Continuous**.</span></span>
2. <span data-ttu-id="70db5-139">Högerklicka på hello Webbjobb i hello listan och klicka på toostart eller stoppa ett kontinuerligt Webbjobb **starta** eller **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="70db5-139">toostart or stop a continuous WebJob, right-click hello WebJob in hello list and click **Start** or **Stop**.</span></span>

> [!NOTE]
> <span data-ttu-id="70db5-140">Om ditt webbprogram som körs på mer än en instans, ska ett körs kontinuerligt Webbjobb köras på alla dina instanser.</span><span class="sxs-lookup"><span data-stu-id="70db5-140">If your web app runs on more than one instance, a continuously running WebJob will run on all of your instances.</span></span> <span data-ttu-id="70db5-141">Vid behov och schemalagda Webbjobb körs på en enda instans som valts för belastningsutjämning med Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="70db5-141">On-demand and scheduled WebJobs run on a single instance selected for load balancing by Microsoft Azure.</span></span>
> 
> <span data-ttu-id="70db5-142">För kontinuerliga Webbjobb toorun tillförlitligt och på alla instanser aktivera hello alltid på * konfigurationsinställning för hello webbprogrammet annars de kan stoppas när hello SCM värdplatsen har varit inaktiv för länge.</span><span class="sxs-lookup"><span data-stu-id="70db5-142">For Continuous WebJobs toorun reliably and on all instances, enable hello Always On* configuration setting for hello web app otherwise they can stop running when hello SCM host site has been idle for too long.</span></span>
> 
> 

## <span data-ttu-id="70db5-143"><a name="CreateScheduledCRON"></a>Skapa ett schemalagda Webbjobb med ett CRON-uttryck</span><span class="sxs-lookup"><span data-stu-id="70db5-143"><a name="CreateScheduledCRON"></a>Create a scheduled WebJob using a CRON expression</span></span>
<span data-ttu-id="70db5-144">Den här tekniken är tillgängliga tooWeb appar som körs i Basic, Standard och Premium-läge och kräver hello **alltid på** inställningen toobe aktiverad på hello app.</span><span class="sxs-lookup"><span data-stu-id="70db5-144">This technique is available tooWeb Apps running in Basic, Standard or Premium mode, and requires hello **Always On** setting toobe enabled on hello app.</span></span>

<span data-ttu-id="70db5-145">tooturn en på begäran Webbjobb till ett schemalagda Webbjobb bara innehålla en `settings.job` filen hello roten i din Webbjobb zip-filen.</span><span class="sxs-lookup"><span data-stu-id="70db5-145">tooturn an On Demand WebJob into a scheduled WebJob, simply include a `settings.job` file at hello root of your WebJob zip file.</span></span> <span data-ttu-id="70db5-146">Den här JSON-filen ska innehålla en `schedule` egenskap med ett [CRON-uttryck](https://en.wikipedia.org/wiki/Cron), per exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="70db5-146">This JSON file should include a `schedule` property with a [CRON expression](https://en.wikipedia.org/wiki/Cron), per example below.</span></span>

<span data-ttu-id="70db5-147">hello CRON-uttryck består av 6 fält: `{second} {minute} {hour} {day} {month} {day of hello week}`.</span><span class="sxs-lookup"><span data-stu-id="70db5-147">hello CRON expression is composed of 6 fields: `{second} {minute} {hour} {day} {month} {day of hello week}`.</span></span>

<span data-ttu-id="70db5-148">Till exempel tootrigger din Webbjobb var 15: e minut din `settings.job` skulle ha:</span><span class="sxs-lookup"><span data-stu-id="70db5-148">For example, tootrigger your WebJob every 15 minutes, your `settings.job` would have:</span></span>

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

<span data-ttu-id="70db5-149">Andra CRON schema-exempel:</span><span class="sxs-lookup"><span data-stu-id="70db5-149">Other CRON schedule examples:</span></span>

* <span data-ttu-id="70db5-150">Varje timme (d.v.s. när hello antal minuter är 0):`0 0 * * * *`</span><span class="sxs-lookup"><span data-stu-id="70db5-150">Every hour (i.e. whenever hello count of minutes is 0): `0 0 * * * *`</span></span> 
* <span data-ttu-id="70db5-151">Varje timme från too5 9: 00 PM:`0 0 9-17 * * *`</span><span class="sxs-lookup"><span data-stu-id="70db5-151">Every hour from 9 AM too5 PM: `0 0 9-17 * * *`</span></span> 
* <span data-ttu-id="70db5-152">På 9:30:00 varje dag:`0 30 9 * * *`</span><span class="sxs-lookup"><span data-stu-id="70db5-152">At 9:30 AM every day: `0 30 9 * * *`</span></span>
* <span data-ttu-id="70db5-153">På 9:30:00 varje veckodag:`0 30 9 * * 1-5`</span><span class="sxs-lookup"><span data-stu-id="70db5-153">At 9:30 AM every week day: `0 30 9 * * 1-5`</span></span>

<span data-ttu-id="70db5-154">**Obs**: när du distribuerar ett Webbjobb från Visual Studio, se till att toomark din `settings.job` filegenskaper som Kopiera om nyare.</span><span class="sxs-lookup"><span data-stu-id="70db5-154">**Note**: when deploying a WebJob from Visual Studio, make sure toomark your `settings.job` file properties as 'Copy if newer'.</span></span>

## <span data-ttu-id="70db5-155"><a name="CreateScheduled"></a>Skapa ett schemalagda Webbjobb med hello Azure Schemaläggaren</span><span class="sxs-lookup"><span data-stu-id="70db5-155"><a name="CreateScheduled"></a>Create a scheduled WebJob using hello Azure Scheduler</span></span>
<span data-ttu-id="70db5-156">Hej följande alternativa metoden använder hello Azure Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="70db5-156">hello following alternate technique makes use of hello Azure Scheduler.</span></span> <span data-ttu-id="70db5-157">I det här fallet saknar din Webbjobb kännedom direkt av hello schema.</span><span class="sxs-lookup"><span data-stu-id="70db5-157">In this case, your WebJob does not have any direct knowledge of hello schedule.</span></span> <span data-ttu-id="70db5-158">Hello Azure Scheduler hämtar i stället konfigurerade tootrigger din Webbjobb enligt ett schema.</span><span class="sxs-lookup"><span data-stu-id="70db5-158">Instead, hello Azure Scheduler gets configured tootrigger your WebJob on a schedule.</span></span> 

<span data-ttu-id="70db5-159">hello Azure Portal ännu inte har hello möjlighet toocreate schemalagda Webbjobb men tills funktionen läggs du kan göra det med hjälp av hello [klassiska portalen](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="70db5-159">hello Azure Portal doesn't yet have hello ability toocreate a scheduled WebJob, but until that feature is added you can do it by using hello [classic portal](http://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="70db5-160">I hello [klassiska portalen](http://manage.windowsazure.com) gå toohello Webbjobb sidan och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="70db5-160">In hello [classic portal](http://manage.windowsazure.com) go toohello WebJob page and click **Add**.</span></span>
2. <span data-ttu-id="70db5-161">I hello **hur tooRun** väljer **körs enligt ett schema**.</span><span class="sxs-lookup"><span data-stu-id="70db5-161">In hello **How tooRun** box, choose **Run on a schedule**.</span></span>
   
    ![Nytt schemalagt jobb][NewScheduledJob]
3. <span data-ttu-id="70db5-163">Välj hello **Scheduler Region** för jobbet, och klicka sedan på pilen hello hello nedre högra hello dialogrutan tooproceed toohello nästa skärm.</span><span class="sxs-lookup"><span data-stu-id="70db5-163">Choose hello **Scheduler Region** for your job, and then click hello arrow on hello bottom right of hello dialog tooproceed toohello next screen.</span></span>
4. <span data-ttu-id="70db5-164">I hello **skapa jobbet** dialogrutan Välj hello typ av **återkommande** önskade: **gång jobbet** eller **återkommande jobb**.</span><span class="sxs-lookup"><span data-stu-id="70db5-164">In hello **Create Job** dialog, choose hello type of **Recurrence** you want: **One-time job** or **Recurring job**.</span></span>
   
    ![Schemalägg återkommande][SchdRecurrence]
5. <span data-ttu-id="70db5-166">Också välja en **Start** tid: **nu** eller **vid en viss tid**.</span><span class="sxs-lookup"><span data-stu-id="70db5-166">Also choose a **Starting** time: **Now** or **At a specific time**.</span></span>
   
    ![Starttiden för schemat][SchdStart]
6. <span data-ttu-id="70db5-168">Om du vill toostart vid en viss tid, väljer du din första tidsvärden under **startar på**.</span><span class="sxs-lookup"><span data-stu-id="70db5-168">If you want toostart at a specific time, choose your starting time values under **Starting On**.</span></span>
   
    ![Schemat Start vid en viss tid][SchdStartOn]
7. <span data-ttu-id="70db5-170">Om du väljer ett återkommande projekt måste du ha hello **utför varje** alternativet toospecify hello ofta förekomst och hello **slutar på** alternativet toospecify en sluttid.</span><span class="sxs-lookup"><span data-stu-id="70db5-170">If you chose a recurring job, you have hello **Recur Every** option toospecify hello frequency of occurrence and hello **Ending On** option toospecify an ending time.</span></span>
   
    ![Schemalägg återkommande][SchdRecurEvery]
8. <span data-ttu-id="70db5-172">Om du väljer **veckor**, kan du välja hello **på ett visst schema** och ange hello veckodagar hello som du vill hello jobbet toorun.</span><span class="sxs-lookup"><span data-stu-id="70db5-172">If you choose **Weeks**, you can select hello **On a Particular Schedule** box and specify hello days of hello week that you want hello job toorun.</span></span>
   
    ![Schemat dagarnas hello vecka][SchdWeeksOnParticular]
9. <span data-ttu-id="70db5-174">Om du väljer **månader** och välj hello **på ett visst schema** kan du ange hello jobbet toorun på visst numrerat **dagar** hello månad.</span><span class="sxs-lookup"><span data-stu-id="70db5-174">If you choose **Months** and select hello **On a Particular Schedule** box, you can set hello job toorun on particular numbered **Days** in hello month.</span></span> 
   
    ![Schemalägga särskilda datum i hello månad][SchdMonthsOnPartDays]
10. <span data-ttu-id="70db5-176">Om du väljer **veckodagar**, du kan välja vilken dag eller hello veckodagar i hello månad hello jobbet toorun på.</span><span class="sxs-lookup"><span data-stu-id="70db5-176">If you choose **Week Days**, you can select which day or days of hello week in hello month you want hello job toorun on.</span></span>
    
     ![Schemalägga viss vecka dagar i månaden][SchdMonthsOnPartWeekDays]
11. <span data-ttu-id="70db5-178">Slutligen kan du också använda hello **förekomster** alternativet toochoose vilken vecka i månaden hello (första, andra, tredje o.s.v.) du vill hello jobbet toorun på hello veckodagar som du angav.</span><span class="sxs-lookup"><span data-stu-id="70db5-178">Finally, you can also use hello **Occurrences** option toochoose which week in hello month (first, second, third etc.) you want hello job toorun on hello week days you specified.</span></span>
    
    ![Schemalägga viss veckodagar på viss veckor i månaden][SchdMonthsOnPartWeekDaysOccurences]
12. <span data-ttu-id="70db5-180">När du har skapat ett eller flera jobb visas namnen på hello WebJobs flik med deras status, typ och annan information.</span><span class="sxs-lookup"><span data-stu-id="70db5-180">After you have created one or more jobs, their names will appear on hello WebJobs tab with their status, schedule type, and other information.</span></span> <span data-ttu-id="70db5-181">Historisk information bevaras för hello senaste 30 WebJobs.</span><span class="sxs-lookup"><span data-stu-id="70db5-181">Historical information for hello last 30  WebJobs is maintained.</span></span>
    
    ![Lista över jobb][WebJobsListWithSeveralJobs]

### <span data-ttu-id="70db5-183"><a name="Scheduler"></a>Schemalagda jobb och Azure Schemaläggaren</span><span class="sxs-lookup"><span data-stu-id="70db5-183"><a name="Scheduler"></a>Scheduled jobs and Azure Scheduler</span></span>
<span data-ttu-id="70db5-184">Schemalagda jobb kan ytterligare konfigureras i hello Azure Scheduler sidor i hello [klassiska portalen](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="70db5-184">Scheduled jobs can be further configured in hello Azure Scheduler pages of hello [classic portal](http://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="70db5-185">Hej WebJobs-sidan, klicka på hello jobbet **schema** länk toonavigate toohello Azure Scheduler Portalsida.</span><span class="sxs-lookup"><span data-stu-id="70db5-185">On hello WebJobs page, click hello job's **schedule** link toonavigate toohello Azure Scheduler portal page.</span></span> 
   
   ![Länka tooAzure Schemaläggaren][LinkToScheduler]
2. <span data-ttu-id="70db5-187">Klicka på hello jobb hello Schemaläggaren på sidan.</span><span class="sxs-lookup"><span data-stu-id="70db5-187">On hello Scheduler page, click hello job.</span></span>
   
    ![Jobb på hello Portalsida för Schemaläggaren][SchedulerPortal]
3. <span data-ttu-id="70db5-189">Hej **Jobbåtgärd** öppnas, där du kan konfigurera hello jobbet ytterligare.</span><span class="sxs-lookup"><span data-stu-id="70db5-189">hello **Job Action** page opens, where you can further configure hello job.</span></span> 
   
    ![Jobbåtgärden PageInScheduler][JobActionPageInScheduler]

## <span data-ttu-id="70db5-191"><a name="ViewJobHistory"></a>Visa hello jobbhistorik</span><span class="sxs-lookup"><span data-stu-id="70db5-191"><a name="ViewJobHistory"></a>View hello job history</span></span>
1. <span data-ttu-id="70db5-192">tooview hello körningstiden för ett jobb, inklusive jobb som skapats med hello WebJobs SDK, klickar du på motsvarande länk under hello **loggar** kolumn av hello WebJobs-bladet.</span><span class="sxs-lookup"><span data-stu-id="70db5-192">tooview hello execution history of a job, including jobs created with hello WebJobs SDK, click  its corresponding link under hello **Logs** column of hello WebJobs blade.</span></span> <span data-ttu-id="70db5-193">(Du kan använda hello Urklipp ikonen toocopy hello URL för hello log file sidan toohello Urklipp om du vill.)</span><span class="sxs-lookup"><span data-stu-id="70db5-193">(You can use hello clipboard icon toocopy hello URL of hello log file page toohello clipboard if you wish.)</span></span>
   
    ![Loggar länk](./media/web-sites-create-web-jobs/wjbladelogslink.png)
2. <span data-ttu-id="70db5-195">Klicka på hello länken öppnar hello informationssidan för hello Webbjobb.</span><span class="sxs-lookup"><span data-stu-id="70db5-195">Clicking hello link opens hello details page for hello WebJob.</span></span> <span data-ttu-id="70db5-196">Den här sidan visar hello av namnet på hello kommandot Kör hello senaste tidpunkten för den kördes och dess lyckats eller misslyckats.</span><span class="sxs-lookup"><span data-stu-id="70db5-196">This page shows you hello name of hello command run, hello last times it ran, and its success or failure.</span></span> <span data-ttu-id="70db5-197">Under **senaste jobbet körs**, klicka på en gång toosee mer information.</span><span class="sxs-lookup"><span data-stu-id="70db5-197">Under **Recent job runs**, click a time toosee further details.</span></span>
   
    ![WebJobDetails][WebJobDetails]
3. <span data-ttu-id="70db5-199">Hej **Webbjobb kör information** visas.</span><span class="sxs-lookup"><span data-stu-id="70db5-199">hello **WebJob Run Details** page appears.</span></span> <span data-ttu-id="70db5-200">Klicka på **växla utdata** toosee hello text hello logga innehåll.</span><span class="sxs-lookup"><span data-stu-id="70db5-200">Click **Toggle Output** toosee hello text of hello log contents.</span></span> <span data-ttu-id="70db5-201">hello utdataloggen är i textformat.</span><span class="sxs-lookup"><span data-stu-id="70db5-201">hello output log is in text format.</span></span> 
   
    ![Web-jobbet körs information][WebJobRunDetails]
4. <span data-ttu-id="70db5-203">toosee hello utdatatext i ett nytt webbläsarfönster klickar du på hello **hämta** länk.</span><span class="sxs-lookup"><span data-stu-id="70db5-203">toosee hello output text in a separate browser window, click hello **download** link.</span></span> <span data-ttu-id="70db5-204">toodownload hello själva texten, högerklicka på hello länk och använda din webbläsare alternativ toosave hello-filens innehåll.</span><span class="sxs-lookup"><span data-stu-id="70db5-204">toodownload hello text itself, right-click hello link and use your browser options toosave hello file contents.</span></span>
   
    ![Hämta utdata][DownloadLogOutput]
5. <span data-ttu-id="70db5-206">Hej **WebJobs** länk hello överst på hello sidan innehåller en praktiskt sätt tooget tooa lista över WebJobs på hello historik instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="70db5-206">hello **WebJobs** link at hello top of hello page provides a convenient way tooget tooa list of WebJobs on hello history dashboard.</span></span>
   
    ![Länken tooWebJobs lista][WebJobsLinkToDashboardList]
   
    ![Lista över WebJobs i instrumentpanelen för historik][WebJobsListInJobsDashboard]
   
    <span data-ttu-id="70db5-209">Klicka på någon av dessa länkar tar dig toohello Webbjobb informationssidan för hello jobb som du har valt.</span><span class="sxs-lookup"><span data-stu-id="70db5-209">Clicking one of these links takes you toohello WebJob Details page for hello job you selected.</span></span>

## <span data-ttu-id="70db5-210"><a name="WHPNotes"></a>Anteckningar</span><span class="sxs-lookup"><span data-stu-id="70db5-210"><a name="WHPNotes"></a>Notes</span></span>
* <span data-ttu-id="70db5-211">Webbprogram i ledigt läge kan timeout efter 20 minuter om det finns ingen webbplats med begäranden toohello scm (distribution) och portalen hello webbapp inte är öppen i Azure.</span><span class="sxs-lookup"><span data-stu-id="70db5-211">Web apps in Free mode can time out after 20 minutes if there are no requests toohello scm (deployment) site and hello web app's portal is not open in Azure.</span></span> <span data-ttu-id="70db5-212">Begäranden toohello faktiska webbplatsen kommer inte att återställa det här.</span><span class="sxs-lookup"><span data-stu-id="70db5-212">Requests toohello actual site will not reset this.</span></span>
* <span data-ttu-id="70db5-213">Koden för en kontinuerlig jobbet måste toobe skrivs toorun i en oändlig loop.</span><span class="sxs-lookup"><span data-stu-id="70db5-213">Code for a continuous job needs toobe written toorun in an endless loop.</span></span>
* <span data-ttu-id="70db5-214">Kontinuerlig jobb körs alltid endast när hello webbprogram som är igång.</span><span class="sxs-lookup"><span data-stu-id="70db5-214">Continuous jobs run continuously only when hello web app is up.</span></span>
* <span data-ttu-id="70db5-215">Basic och Standard lägen erbjudande hello alltid på funktion som, när aktiverad förhindrar web apps får viloläge.</span><span class="sxs-lookup"><span data-stu-id="70db5-215">Basic and Standard modes offer hello Always On feature which, when enabled, prevents web apps from becoming idle.</span></span>
* <span data-ttu-id="70db5-216">Du kan bara felsöka Webbjobb som körs kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="70db5-216">You can only debug continuously running WebJobs.</span></span> <span data-ttu-id="70db5-217">Schemalagd eller på begäran WebJobs-Felsökning stöds inte.</span><span class="sxs-lookup"><span data-stu-id="70db5-217">Debugging scheduled or on-demand WebJobs is not supported.</span></span>

## <span data-ttu-id="70db5-218"><a name="NextSteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="70db5-218"><a name="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="70db5-219">Mer information finns i [Azure WebJobs rekommenderas resurser][WebJobsRecommendedResources].</span><span class="sxs-lookup"><span data-stu-id="70db5-219">For more information, see [Azure WebJobs Recommended Resources][WebJobsRecommendedResources].</span></span>

[PSonWebJobs]:http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx
[WebJobsRecommendedResources]:http://go.microsoft.com/fwlink/?LinkId=390226

[OnDemandWebJob]: ./media/web-sites-create-web-jobs/01aOnDemandWebJob.png
[WebJobsList]: ./media/web-sites-create-web-jobs/02aWebJobsList.png
[NewContinuousJob]: ./media/web-sites-create-web-jobs/03aNewContinuousJob.png
[NewScheduledJob]: ./media/web-sites-create-web-jobs/04aNewScheduledJob.png
[SchdRecurrence]: ./media/web-sites-create-web-jobs/05SchdRecurrence.png
[SchdStart]: ./media/web-sites-create-web-jobs/06SchdStart.png
[SchdStartOn]: ./media/web-sites-create-web-jobs/07SchdStartOn.png
[SchdRecurEvery]: ./media/web-sites-create-web-jobs/08SchdRecurEvery.png
[SchdWeeksOnParticular]: ./media/web-sites-create-web-jobs/09SchdWeeksOnParticular.png
[SchdMonthsOnPartDays]: ./media/web-sites-create-web-jobs/10SchdMonthsOnPartDays.png
[SchdMonthsOnPartWeekDays]: ./media/web-sites-create-web-jobs/11SchdMonthsOnPartWeekDays.png
[SchdMonthsOnPartWeekDaysOccurences]: ./media/web-sites-create-web-jobs/12SchdMonthsOnPartWeekDaysOccurences.png
[RunOnce]: ./media/web-sites-create-web-jobs/13RunOnce.png
[WebJobsListWithSeveralJobs]: ./media/web-sites-create-web-jobs/13WebJobsListWithSeveralJobs.png
[WebJobLogs]: ./media/web-sites-create-web-jobs/14WebJobLogs.png
[WebJobDetails]: ./media/web-sites-create-web-jobs/15WebJobDetails.png
[WebJobRunDetails]: ./media/web-sites-create-web-jobs/16WebJobRunDetails.png
[DownloadLogOutput]: ./media/web-sites-create-web-jobs/17DownloadLogOutput.png
[WebJobsLinkToDashboardList]: ./media/web-sites-create-web-jobs/18WebJobsLinkToDashboardList.png
[WebJobsListInJobsDashboard]: ./media/web-sites-create-web-jobs/19WebJobsListInJobsDashboard.png
[LinkToScheduler]: ./media/web-sites-create-web-jobs/31LinkToScheduler.png
[SchedulerPortal]: ./media/web-sites-create-web-jobs/32SchedulerPortal.png
[JobActionPageInScheduler]: ./media/web-sites-create-web-jobs/33JobActionPageInScheduler.png

