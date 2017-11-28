---
title: "Kör bakgrundsuppgifter med WebJobs"
description: "Lär dig mer om att köra bakgrundsaktiviteter i Azure web apps."
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
ms.openlocfilehash: 3f8ae748e3d9c6b4e342536926a52b4e8f37ee51
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-background-tasks-with-webjobs"></a><span data-ttu-id="1f214-103">Kör bakgrundsuppgifter med WebJobs</span><span class="sxs-lookup"><span data-stu-id="1f214-103">Run Background tasks with WebJobs</span></span>
## <a name="overview"></a><span data-ttu-id="1f214-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1f214-104">Overview</span></span>
<span data-ttu-id="1f214-105">Du kan köra program eller skript i WebJobs i din [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webbprogrammet på tre sätt: på begäran, kontinuerligt, eller enligt ett schema.</span><span class="sxs-lookup"><span data-stu-id="1f214-105">You can run programs or scripts in WebJobs in your [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) web app in three ways: on demand, continuously, or on a schedule.</span></span> <span data-ttu-id="1f214-106">Det finns utan extra kostnad för att använda WebJobs.</span><span class="sxs-lookup"><span data-stu-id="1f214-106">There is no additional cost to use WebJobs.</span></span>

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

<span data-ttu-id="1f214-107">Den här artikeln visar hur du distribuerar WebJobs med hjälp av den [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1f214-107">This article shows how to deploy WebJobs by using the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="1f214-108">Information om hur du distribuerar med hjälp av Visual Studio eller en kontinuerlig leveransprocess finns [hur du distribuerar Azure WebJobs webbappar](websites-dotnet-deploy-webjobs.md).</span><span class="sxs-lookup"><span data-stu-id="1f214-108">For information about how to deploy by using Visual Studio or a continuous delivery process, see [How to Deploy Azure WebJobs to Web Apps](websites-dotnet-deploy-webjobs.md).</span></span>

<span data-ttu-id="1f214-109">Azure WebJobs SDK förenklar många WebJobs programmeringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1f214-109">The Azure WebJobs SDK simplifies many WebJobs programming tasks.</span></span> <span data-ttu-id="1f214-110">Mer information finns i [vad är WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="1f214-110">For more information, see [What is the WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span></span>

 <span data-ttu-id="1f214-111">Azure Functions erbjuder ett annat sätt att köra program och skript från en serverlösa miljö eller från en Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="1f214-111">Azure Functions provides another way to run programs and scripts from either a serverless environment or from an App Service app.</span></span> <span data-ttu-id="1f214-112">Mer information finns i [översikt över Azure Functions](../azure-functions/functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1f214-112">For more information, see [Azure Functions overview](../azure-functions/functions-overview.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="1f214-113"><a name="acceptablefiles"></a>Filtyper för skript eller program</span><span class="sxs-lookup"><span data-stu-id="1f214-113"><a name="acceptablefiles"></a>Acceptable file types for scripts or programs</span></span>
<span data-ttu-id="1f214-114">Följande filtyper accepteras:</span><span class="sxs-lookup"><span data-stu-id="1f214-114">The following file types are accepted:</span></span>

* <span data-ttu-id="1f214-115">.cmd, .bat, .exe (med windows cmd)</span><span class="sxs-lookup"><span data-stu-id="1f214-115">.cmd, .bat, .exe (using windows cmd)</span></span>
* <span data-ttu-id="1f214-116">.ps1 (med powershell)</span><span class="sxs-lookup"><span data-stu-id="1f214-116">.ps1 (using powershell)</span></span>
* <span data-ttu-id="1f214-117">.SH (med bash)</span><span class="sxs-lookup"><span data-stu-id="1f214-117">.sh (using bash)</span></span>
* <span data-ttu-id="1f214-118">.php (med php)</span><span class="sxs-lookup"><span data-stu-id="1f214-118">.php (using php)</span></span>
* <span data-ttu-id="1f214-119">.PY (med python)</span><span class="sxs-lookup"><span data-stu-id="1f214-119">.py (using python)</span></span>
* <span data-ttu-id="1f214-120">.js (med nod)</span><span class="sxs-lookup"><span data-stu-id="1f214-120">.js (using node)</span></span>
* <span data-ttu-id="1f214-121">.JAR (med java)</span><span class="sxs-lookup"><span data-stu-id="1f214-121">.jar (using java)</span></span>

## <span data-ttu-id="1f214-122"><a name="CreateOnDemand"></a>Skapa en på begäran Webbjobb i portalen</span><span class="sxs-lookup"><span data-stu-id="1f214-122"><a name="CreateOnDemand"></a>Create an on demand WebJob in the portal</span></span>
1. <span data-ttu-id="1f214-123">I den **Webbapp** bladet för den [Azure-portalen](https://portal.azure.com), klickar du på **alla inställningar > WebJobs** att visa den **WebJobs** bladet.</span><span class="sxs-lookup"><span data-stu-id="1f214-123">In the **Web App** blade of the [Azure Portal](https://portal.azure.com), click **All settings > WebJobs** to show the **WebJobs** blade.</span></span>
   
    ![Webbjobbet bladet](./media/web-sites-create-web-jobs/wjblade.png)
2. <span data-ttu-id="1f214-125">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1f214-125">Click **Add**.</span></span> <span data-ttu-id="1f214-126">Den **lägger till Webbjobb** visas.</span><span class="sxs-lookup"><span data-stu-id="1f214-126">The **Add WebJob** dialog appears.</span></span>
   
    ![Webbjobbet bladet Lägg till](./media/web-sites-create-web-jobs/addwjblade.png)
3. <span data-ttu-id="1f214-128">Under **namn**, ange ett namn för Webbjobbet.</span><span class="sxs-lookup"><span data-stu-id="1f214-128">Under **Name**, provide a name for the WebJob.</span></span> <span data-ttu-id="1f214-129">Namnet måste börja med en bokstav eller en siffra och får inte innehålla specialtecken än ”-” och ”_”.</span><span class="sxs-lookup"><span data-stu-id="1f214-129">The name must start with a letter or a number and cannot contain any special characters other than "-" and "_".</span></span>
4. <span data-ttu-id="1f214-130">I den **hur du kör** väljer **körs på begäran**.</span><span class="sxs-lookup"><span data-stu-id="1f214-130">In the **How to Run** box, choose **Run on Demand**.</span></span>
5. <span data-ttu-id="1f214-131">I den **filöverföring** rutan, klicka på mappikonen och bläddra till zip-filen som innehåller skriptet.</span><span class="sxs-lookup"><span data-stu-id="1f214-131">In the **File Upload** box, click the folder icon and browse to the zip file that contains your script.</span></span> <span data-ttu-id="1f214-132">Zip-filen ska innehålla den körbara filen (.exe .cmd .bat .sh .php .py .js) samt alla stödfiler som behövs för att köra program eller skript.</span><span class="sxs-lookup"><span data-stu-id="1f214-132">The zip file should contain your executable (.exe .cmd .bat .sh .php .py .js) as well as any supporting files needed to run the program or script.</span></span>
6. <span data-ttu-id="1f214-133">Kontrollera **skapa** att överföra skriptet till ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1f214-133">Check **Create** to upload the script to your web app.</span></span> 
   
    <span data-ttu-id="1f214-134">Namnet du angav för WebJob som visas i listan på den **WebJobs** bladet.</span><span class="sxs-lookup"><span data-stu-id="1f214-134">The name you specified for the WebJob appears in the list on the **WebJobs** blade.</span></span>
7. <span data-ttu-id="1f214-135">Om du vill köra Webbjobbet högerklickar du på namnet i listan och klickar på **kör**.</span><span class="sxs-lookup"><span data-stu-id="1f214-135">To run the WebJob, right-click its name in the list and click **Run**.</span></span>
   
    ![Köra Webbjobbet](./media/web-sites-create-web-jobs/runondemand.png)

## <span data-ttu-id="1f214-137"><a name="CreateContinuous"></a>Skapa ett körs kontinuerligt Webbjobb</span><span class="sxs-lookup"><span data-stu-id="1f214-137"><a name="CreateContinuous"></a>Create a continuously running WebJob</span></span>
1. <span data-ttu-id="1f214-138">Om du vill skapa ett Webbjobb som körs kontinuerligt, följer du samma steg för att skapa ett Webbjobb som körs en gång, men i den **hur du kör** väljer **kontinuerlig**.</span><span class="sxs-lookup"><span data-stu-id="1f214-138">To create a continuously executing WebJob, follow the same steps for creating a WebJob that runs once, but in the **How to Run** box, choose **Continuous**.</span></span>
2. <span data-ttu-id="1f214-139">För att starta eller stoppa ett kontinuerligt Webbjobb, högerklicka på Webbjobb i listan och klicka på **starta** eller **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="1f214-139">To start or stop a continuous WebJob, right-click the WebJob in the list and click **Start** or **Stop**.</span></span>

> [!NOTE]
> <span data-ttu-id="1f214-140">Om ditt webbprogram som körs på mer än en instans, ska ett körs kontinuerligt Webbjobb köras på alla dina instanser.</span><span class="sxs-lookup"><span data-stu-id="1f214-140">If your web app runs on more than one instance, a continuously running WebJob will run on all of your instances.</span></span> <span data-ttu-id="1f214-141">Vid behov och schemalagda Webbjobb körs på en enda instans som valts för belastningsutjämning med Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="1f214-141">On-demand and scheduled WebJobs run on a single instance selected for load balancing by Microsoft Azure.</span></span>
> 
> <span data-ttu-id="1f214-142">Aktivera för kontinuerliga Webbjobb ska köras på ett tillförlitligt sätt och på alla instanser av Always On * konfigurationsinställning för webbprogrammet annars de kan stoppas när SCM värdplatsen har varit inaktiv för länge.</span><span class="sxs-lookup"><span data-stu-id="1f214-142">For Continuous WebJobs to run reliably and on all instances, enable the Always On* configuration setting for the web app otherwise they can stop running when the SCM host site has been idle for too long.</span></span>
> 
> 

## <span data-ttu-id="1f214-143"><a name="CreateScheduledCRON"></a>Skapa ett schemalagda Webbjobb med ett CRON-uttryck</span><span class="sxs-lookup"><span data-stu-id="1f214-143"><a name="CreateScheduledCRON"></a>Create a scheduled WebJob using a CRON expression</span></span>
<span data-ttu-id="1f214-144">Den här tekniken är tillgängliga för webbprogram som körs i Basic, Standard och Premium-läge och kräver den **alltid på** inställningen är aktiverad i appen.</span><span class="sxs-lookup"><span data-stu-id="1f214-144">This technique is available to Web Apps running in Basic, Standard or Premium mode, and requires the **Always On** setting to be enabled on the app.</span></span>

<span data-ttu-id="1f214-145">Om du vill aktivera en på begäran Webbjobb till ett schemalagda Webbjobb bara innehålla en `settings.job` filen i roten på din Webbjobb zip-filen.</span><span class="sxs-lookup"><span data-stu-id="1f214-145">To turn an On Demand WebJob into a scheduled WebJob, simply include a `settings.job` file at the root of your WebJob zip file.</span></span> <span data-ttu-id="1f214-146">Den här JSON-filen ska innehålla en `schedule` egenskap med ett [CRON-uttryck](https://en.wikipedia.org/wiki/Cron), per exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="1f214-146">This JSON file should include a `schedule` property with a [CRON expression](https://en.wikipedia.org/wiki/Cron), per example below.</span></span>

<span data-ttu-id="1f214-147">CRON-uttryck består av 6 fält: `{second} {minute} {hour} {day} {month} {day of the week}`.</span><span class="sxs-lookup"><span data-stu-id="1f214-147">The CRON expression is composed of 6 fields: `{second} {minute} {hour} {day} {month} {day of the week}`.</span></span>

<span data-ttu-id="1f214-148">Till exempel för att utlösa din Webbjobb var 15: e minut din `settings.job` skulle ha:</span><span class="sxs-lookup"><span data-stu-id="1f214-148">For example, to trigger your WebJob every 15 minutes, your `settings.job` would have:</span></span>

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

<span data-ttu-id="1f214-149">Andra CRON schema-exempel:</span><span class="sxs-lookup"><span data-stu-id="1f214-149">Other CRON schedule examples:</span></span>

* <span data-ttu-id="1f214-150">Varje timme (dvs. när antalet minuter är 0):`0 0 * * * *`</span><span class="sxs-lookup"><span data-stu-id="1f214-150">Every hour (i.e. whenever the count of minutes is 0): `0 0 * * * *`</span></span> 
* <span data-ttu-id="1f214-151">Varje timme från 9: 00 och 17: 00:`0 0 9-17 * * *`</span><span class="sxs-lookup"><span data-stu-id="1f214-151">Every hour from 9 AM to 5 PM: `0 0 9-17 * * *`</span></span> 
* <span data-ttu-id="1f214-152">På 9:30:00 varje dag:`0 30 9 * * *`</span><span class="sxs-lookup"><span data-stu-id="1f214-152">At 9:30 AM every day: `0 30 9 * * *`</span></span>
* <span data-ttu-id="1f214-153">På 9:30:00 varje veckodag:`0 30 9 * * 1-5`</span><span class="sxs-lookup"><span data-stu-id="1f214-153">At 9:30 AM every week day: `0 30 9 * * 1-5`</span></span>

<span data-ttu-id="1f214-154">**Obs**: när du distribuerar ett Webbjobb från Visual Studio kan du se till att markera din `settings.job` filegenskaper som Kopiera om nyare.</span><span class="sxs-lookup"><span data-stu-id="1f214-154">**Note**: when deploying a WebJob from Visual Studio, make sure to mark your `settings.job` file properties as 'Copy if newer'.</span></span>

## <span data-ttu-id="1f214-155"><a name="CreateScheduled"></a>Skapa ett schemalagda Webbjobb med hjälp av Schemaläggaren i Azure</span><span class="sxs-lookup"><span data-stu-id="1f214-155"><a name="CreateScheduled"></a>Create a scheduled WebJob using the Azure Scheduler</span></span>
<span data-ttu-id="1f214-156">Följande alternativ metod är användning av Schemaläggaren i Azure.</span><span class="sxs-lookup"><span data-stu-id="1f214-156">The following alternate technique makes use of the Azure Scheduler.</span></span> <span data-ttu-id="1f214-157">I det här fallet saknar din Webbjobb kännedom direkt för schemat.</span><span class="sxs-lookup"><span data-stu-id="1f214-157">In this case, your WebJob does not have any direct knowledge of the schedule.</span></span> <span data-ttu-id="1f214-158">I stället hämtar Azure Schemaläggaren har konfigurerats för att utlösa din Webbjobb enligt ett schema.</span><span class="sxs-lookup"><span data-stu-id="1f214-158">Instead, the Azure Scheduler gets configured to trigger your WebJob on a schedule.</span></span> 

<span data-ttu-id="1f214-159">Azure Portal ännu inte har möjlighet att skapa ett schemalagda Webbjobb men tills funktionen läggs du kan göra det med hjälp av den [klassiska portalen](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="1f214-159">The Azure Portal doesn't yet have the ability to create a scheduled WebJob, but until that feature is added you can do it by using the [classic portal](http://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="1f214-160">I den [klassiska portalen](http://manage.windowsazure.com) går du till Webbjobbet och klickar på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1f214-160">In the [classic portal](http://manage.windowsazure.com) go to the WebJob page and click **Add**.</span></span>
2. <span data-ttu-id="1f214-161">I den **hur du kör** väljer **körs enligt ett schema**.</span><span class="sxs-lookup"><span data-stu-id="1f214-161">In the **How to Run** box, choose **Run on a schedule**.</span></span>
   
    ![Nytt schemalagt jobb][NewScheduledJob]
3. <span data-ttu-id="1f214-163">Välj den **Scheduler Region** för jobbet, och sedan klicka på pilen längst ned till höger i dialogrutan för att gå vidare till nästa sida.</span><span class="sxs-lookup"><span data-stu-id="1f214-163">Choose the **Scheduler Region** for your job, and then click the arrow on the bottom right of the dialog to proceed to the next screen.</span></span>
4. <span data-ttu-id="1f214-164">I den **skapa jobbet** dialogrutan Välj typ av **återkommande** du vill: **gång jobbet** eller **återkommande jobb**.</span><span class="sxs-lookup"><span data-stu-id="1f214-164">In the **Create Job** dialog, choose the type of **Recurrence** you want: **One-time job** or **Recurring job**.</span></span>
   
    ![Schemalägg återkommande][SchdRecurrence]
5. <span data-ttu-id="1f214-166">Också välja en **Start** tid: **nu** eller **vid en viss tid**.</span><span class="sxs-lookup"><span data-stu-id="1f214-166">Also choose a **Starting** time: **Now** or **At a specific time**.</span></span>
   
    ![Starttiden för schemat][SchdStart]
6. <span data-ttu-id="1f214-168">Om du vill starta vid en viss tid väljer du din första tidsvärden under **startar på**.</span><span class="sxs-lookup"><span data-stu-id="1f214-168">If you want to start at a specific time, choose your starting time values under **Starting On**.</span></span>
   
    ![Schemat Start vid en viss tid][SchdStartOn]
7. <span data-ttu-id="1f214-170">Om du väljer ett återkommande jobb, som du har den **utför varje** alternativet för att ange hur ofta förekomst och **slutar på** alternativet för att ange en sluttid.</span><span class="sxs-lookup"><span data-stu-id="1f214-170">If you chose a recurring job, you have the **Recur Every** option to specify the frequency of occurrence and the **Ending On** option to specify an ending time.</span></span>
   
    ![Schemalägg återkommande][SchdRecurEvery]
8. <span data-ttu-id="1f214-172">Om du väljer **veckor**, kan du välja den **på ett visst schema** och anger veckodagarna som du vill att jobbet ska köras.</span><span class="sxs-lookup"><span data-stu-id="1f214-172">If you choose **Weeks**, you can select the **On a Particular Schedule** box and specify the days of the week that you want the job to run.</span></span>
   
    ![Schemat dagar i veckan][SchdWeeksOnParticular]
9. <span data-ttu-id="1f214-174">Om du väljer **månader** och välj den **på ett visst schema** kan du ange jobbet ska köras på visst numrerat **dagar** i månaden.</span><span class="sxs-lookup"><span data-stu-id="1f214-174">If you choose **Months** and select the **On a Particular Schedule** box, you can set the job to run on particular numbered **Days** in the month.</span></span> 
   
    ![Schemat för särskilda datum i månaden][SchdMonthsOnPartDays]
10. <span data-ttu-id="1f214-176">Om du väljer **veckodagar**, du kan välja eller vilka dagar i veckan i månaden som du vill att jobbet ska köras på.</span><span class="sxs-lookup"><span data-stu-id="1f214-176">If you choose **Week Days**, you can select which day or days of the week in the month you want the job to run on.</span></span>
    
     ![Schemalägga viss vecka dagar i månaden][SchdMonthsOnPartWeekDays]
11. <span data-ttu-id="1f214-178">Slutligen kan du kan också använda den **förekomster** möjlighet att välja vilken vecka i månaden (första, andra, tredje o.s.v.) du vill att jobbet ska köras på veckodagar som du angav.</span><span class="sxs-lookup"><span data-stu-id="1f214-178">Finally, you can also use the **Occurrences** option to choose which week in the month (first, second, third etc.) you want the job to run on the week days you specified.</span></span>
    
    ![Schemalägga viss veckodagar på viss veckor i månaden][SchdMonthsOnPartWeekDaysOccurences]
12. <span data-ttu-id="1f214-180">När du har skapat ett eller flera jobb visas namnen på fliken WebJobs med deras status, typ och annan information.</span><span class="sxs-lookup"><span data-stu-id="1f214-180">After you have created one or more jobs, their names will appear on the WebJobs tab with their status, schedule type, and other information.</span></span> <span data-ttu-id="1f214-181">Historisk information för de senaste 30 WebJobs bevaras.</span><span class="sxs-lookup"><span data-stu-id="1f214-181">Historical information for the last 30  WebJobs is maintained.</span></span>
    
    ![Lista över jobb][WebJobsListWithSeveralJobs]

### <span data-ttu-id="1f214-183"><a name="Scheduler"></a>Schemalagda jobb och Azure Schemaläggaren</span><span class="sxs-lookup"><span data-stu-id="1f214-183"><a name="Scheduler"></a>Scheduled jobs and Azure Scheduler</span></span>
<span data-ttu-id="1f214-184">Schemalagda jobb kan ytterligare konfigureras i Azure Scheduler-sidor i den [klassiska portalen](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="1f214-184">Scheduled jobs can be further configured in the Azure Scheduler pages of the [classic portal](http://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="1f214-185">Jobbets på sidan WebJobs **schema** länken för att gå till sidan Schemaläggaren i Azure portal.</span><span class="sxs-lookup"><span data-stu-id="1f214-185">On the WebJobs page, click the job's **schedule** link to navigate to the Azure Scheduler portal page.</span></span> 
   
   ![Länk till Azure Schemaläggaren][LinkToScheduler]
2. <span data-ttu-id="1f214-187">På sidan Schemaläggaren, klickar du på jobbet.</span><span class="sxs-lookup"><span data-stu-id="1f214-187">On the Scheduler page, click the job.</span></span>
   
    ![Jobb på portalsidan Schemaläggaren][SchedulerPortal]
3. <span data-ttu-id="1f214-189">Den **Jobbåtgärd** öppnas, där du kan konfigurera jobbet ytterligare.</span><span class="sxs-lookup"><span data-stu-id="1f214-189">The **Job Action** page opens, where you can further configure the job.</span></span> 
   
    ![Jobbåtgärden PageInScheduler][JobActionPageInScheduler]

## <span data-ttu-id="1f214-191"><a name="ViewJobHistory"></a>Visa jobbets historik</span><span class="sxs-lookup"><span data-stu-id="1f214-191"><a name="ViewJobHistory"></a>View the job history</span></span>
1. <span data-ttu-id="1f214-192">Om du vill visa historiken för körning av ett jobb, inklusive jobb som skapats med WebJobs-SDK klickar du på motsvarande länk under den **loggar** kolumn i bladet WebJobs.</span><span class="sxs-lookup"><span data-stu-id="1f214-192">To view the execution history of a job, including jobs created with the WebJobs SDK, click  its corresponding link under the **Logs** column of the WebJobs blade.</span></span> <span data-ttu-id="1f214-193">(Du kan använda ikonen Urklipp Kopiera Webbadressen till sidan log file till Urklipp om du vill.)</span><span class="sxs-lookup"><span data-stu-id="1f214-193">(You can use the clipboard icon to copy the URL of the log file page to the clipboard if you wish.)</span></span>
   
    ![Loggar länk](./media/web-sites-create-web-jobs/wjbladelogslink.png)
2. <span data-ttu-id="1f214-195">Klicka på länken öppnar informationssidan för Webbjobbet.</span><span class="sxs-lookup"><span data-stu-id="1f214-195">Clicking the link opens the details page for the WebJob.</span></span> <span data-ttu-id="1f214-196">Den här sidan visar namnet på kommandot Kör den senaste tidpunkten för den kördes och dess lyckats eller misslyckats.</span><span class="sxs-lookup"><span data-stu-id="1f214-196">This page shows you the name of the command run, the last times it ran, and its success or failure.</span></span> <span data-ttu-id="1f214-197">Under **senaste jobbet körs**, klicka på en gång för att se ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="1f214-197">Under **Recent job runs**, click a time to see further details.</span></span>
   
    ![WebJobDetails][WebJobDetails]
3. <span data-ttu-id="1f214-199">Den **Webbjobb kör information** visas.</span><span class="sxs-lookup"><span data-stu-id="1f214-199">The **WebJob Run Details** page appears.</span></span> <span data-ttu-id="1f214-200">Klicka på **växla utdata** att se texten i logginnehållet.</span><span class="sxs-lookup"><span data-stu-id="1f214-200">Click **Toggle Output** to see the text of the log contents.</span></span> <span data-ttu-id="1f214-201">Logg för utdata är i textformat.</span><span class="sxs-lookup"><span data-stu-id="1f214-201">The output log is in text format.</span></span> 
   
    ![Web-jobbet körs information][WebJobRunDetails]
4. <span data-ttu-id="1f214-203">Klicka för att visa den resulterande texten i ett nytt webbläsarfönster i **hämta** länk.</span><span class="sxs-lookup"><span data-stu-id="1f214-203">To see the output text in a separate browser window, click the **download** link.</span></span> <span data-ttu-id="1f214-204">Högerklicka på länken för att hämta själva texten, och Använd webbläsaren för att spara filinnehållet.</span><span class="sxs-lookup"><span data-stu-id="1f214-204">To download the text itself, right-click the link and use your browser options to save the file contents.</span></span>
   
    ![Hämta utdata][DownloadLogOutput]
5. <span data-ttu-id="1f214-206">Den **WebJobs** längst upp på sidan är ett bekvämt sätt att komma till en lista över WebJobs på instrumentpanelen historik.</span><span class="sxs-lookup"><span data-stu-id="1f214-206">The **WebJobs** link at the top of the page provides a convenient way to get to a list of WebJobs on the history dashboard.</span></span>
   
    ![Länka till WebJobs-lista][WebJobsLinkToDashboardList]
   
    ![Lista över WebJobs i instrumentpanelen för historik][WebJobsListInJobsDashboard]
   
    <span data-ttu-id="1f214-209">Klicka på någon av dessa länkar går du till sidan Webbjobb information för jobbet som du har valt.</span><span class="sxs-lookup"><span data-stu-id="1f214-209">Clicking one of these links takes you to the WebJob Details page for the job you selected.</span></span>

## <span data-ttu-id="1f214-210"><a name="WHPNotes"></a>Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1f214-210"><a name="WHPNotes"></a>Notes</span></span>
* <span data-ttu-id="1f214-211">Webbprogram i ledigt läge kan timeout efter 20 minuter om det finns inga förfrågningar till webbplatsen scm (distribution) och webbappens portalen inte är öppen i Azure.</span><span class="sxs-lookup"><span data-stu-id="1f214-211">Web apps in Free mode can time out after 20 minutes if there are no requests to the scm (deployment) site and the web app's portal is not open in Azure.</span></span> <span data-ttu-id="1f214-212">Begäranden till den faktiska platsen kommer inte att återställa det här.</span><span class="sxs-lookup"><span data-stu-id="1f214-212">Requests to the actual site will not reset this.</span></span>
* <span data-ttu-id="1f214-213">Koden för en kontinuerlig jobbet måste skrivas ska köras i en oändlig loop.</span><span class="sxs-lookup"><span data-stu-id="1f214-213">Code for a continuous job needs to be written to run in an endless loop.</span></span>
* <span data-ttu-id="1f214-214">Kontinuerlig jobben körs alltid endast när webbappen är igång.</span><span class="sxs-lookup"><span data-stu-id="1f214-214">Continuous jobs run continuously only when the web app is up.</span></span>
* <span data-ttu-id="1f214-215">Basic och Standard lägen erbjudande den alltid på Funktion, som när aktiverad förhindrar web apps får viloläge.</span><span class="sxs-lookup"><span data-stu-id="1f214-215">Basic and Standard modes offer the Always On feature which, when enabled, prevents web apps from becoming idle.</span></span>
* <span data-ttu-id="1f214-216">Du kan bara felsöka Webbjobb som körs kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="1f214-216">You can only debug continuously running WebJobs.</span></span> <span data-ttu-id="1f214-217">Schemalagd eller på begäran WebJobs-Felsökning stöds inte.</span><span class="sxs-lookup"><span data-stu-id="1f214-217">Debugging scheduled or on-demand WebJobs is not supported.</span></span>

## <span data-ttu-id="1f214-218"><a name="NextSteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f214-218"><a name="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="1f214-219">Mer information finns i [Azure WebJobs rekommenderas resurser][WebJobsRecommendedResources].</span><span class="sxs-lookup"><span data-stu-id="1f214-219">For more information, see [Azure WebJobs Recommended Resources][WebJobsRecommendedResources].</span></span>

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

