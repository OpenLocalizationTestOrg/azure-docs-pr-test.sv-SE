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
# <a name="run-background-tasks-with-webjobs"></a>Kör bakgrundsuppgifter med WebJobs
## <a name="overview"></a>Översikt
Du kan köra program eller skript i WebJobs i din [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webbprogrammet på tre sätt: på begäran, kontinuerligt, eller enligt ett schema. Det finns inga extra kostnad toouse WebJobs.

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

Den här artikeln visar hur toodeploy WebJobs med hjälp av hello [Azure Portal](https://portal.azure.com). Information om hur toodeploy med hjälp av Visual Studio eller en kontinuerlig distributionsprocessen finns [hur tooDeploy Azure WebJobs tooWeb appar](websites-dotnet-deploy-webjobs.md).

hello Azure WebJobs SDK förenklar många WebJobs-programmeringsuppgifter. Mer information finns i [vad är hello WebJobs SDK](websites-dotnet-webjobs-sdk.md).

 Azure Functions erbjuder ett annat sätt toorun program och skript från en serverlösa miljö eller från en Apptjänst-app. Mer information finns i [översikt över Azure Functions](../azure-functions/functions-overview.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="acceptablefiles"></a>Filtyper för skript eller program
följande filtyper hello accepteras:

* .cmd, .bat, .exe (med windows cmd)
* .ps1 (med powershell)
* .SH (med bash)
* .php (med php)
* .PY (med python)
* .js (med nod)
* .JAR (med java)

## <a name="CreateOnDemand"></a>Skapa en på begäran Webbjobb hello-portalen
1. I hello **Webbapp** bladet för hello [Azure Portal](https://portal.azure.com), klickar du på **alla inställningar > WebJobs** tooshow hello **WebJobs** bladet.
   
    ![Webbjobbet bladet](./media/web-sites-create-web-jobs/wjblade.png)
2. Klicka på **Lägg till**. Hej **lägger till Webbjobb** visas.
   
    ![Webbjobbet bladet Lägg till](./media/web-sites-create-web-jobs/addwjblade.png)
3. Under **namn**, ange ett namn för hello Webbjobb. hello-namnet måste börja med en bokstav eller en siffra och får inte innehålla specialtecken än ”-” och ”_”.
4. I hello **hur tooRun** väljer **körs på begäran**.
5. I hello **filöverföring** rutan och klicka på ikonen för hello mapp Bläddra toohello zip-fil som innehåller skriptet. hello zip-filen ska innehålla den körbara filen (.exe .cmd .bat .sh .php .py .js) samt alla stödfiler behövs toorun hello program eller skript.
6. Kontrollera **skapa** tooupload hello skriptet tooyour webbprogram. 
   
    hello namn du angav för hello Webbjobbet visas i listan hello på hello **WebJobs** bladet.
7. toorun hello Webbjobb, högerklickar du på namnet i hello listan och klicka på **kör**.
   
    ![Köra Webbjobbet](./media/web-sites-create-web-jobs/runondemand.png)

## <a name="CreateContinuous"></a>Skapa ett körs kontinuerligt Webbjobb
1. toocreate ett Webbjobb som körs kontinuerligt följa samma steg för att skapa ett Webbjobb som körs en gång, men i hello hello **hur tooRun** väljer **kontinuerlig**.
2. Högerklicka på hello Webbjobb i hello listan och klicka på toostart eller stoppa ett kontinuerligt Webbjobb **starta** eller **stoppa**.

> [!NOTE]
> Om ditt webbprogram som körs på mer än en instans, ska ett körs kontinuerligt Webbjobb köras på alla dina instanser. Vid behov och schemalagda Webbjobb körs på en enda instans som valts för belastningsutjämning med Microsoft Azure.
> 
> För kontinuerliga Webbjobb toorun tillförlitligt och på alla instanser aktivera hello alltid på * konfigurationsinställning för hello webbprogrammet annars de kan stoppas när hello SCM värdplatsen har varit inaktiv för länge.
> 
> 

## <a name="CreateScheduledCRON"></a>Skapa ett schemalagda Webbjobb med ett CRON-uttryck
Den här tekniken är tillgängliga tooWeb appar som körs i Basic, Standard och Premium-läge och kräver hello **alltid på** inställningen toobe aktiverad på hello app.

tooturn en på begäran Webbjobb till ett schemalagda Webbjobb bara innehålla en `settings.job` filen hello roten i din Webbjobb zip-filen. Den här JSON-filen ska innehålla en `schedule` egenskap med ett [CRON-uttryck](https://en.wikipedia.org/wiki/Cron), per exemplet nedan.

hello CRON-uttryck består av 6 fält: `{second} {minute} {hour} {day} {month} {day of hello week}`.

Till exempel tootrigger din Webbjobb var 15: e minut din `settings.job` skulle ha:

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

Andra CRON schema-exempel:

* Varje timme (d.v.s. när hello antal minuter är 0):`0 0 * * * *` 
* Varje timme från too5 9: 00 PM:`0 0 9-17 * * *` 
* På 9:30:00 varje dag:`0 30 9 * * *`
* På 9:30:00 varje veckodag:`0 30 9 * * 1-5`

**Obs**: när du distribuerar ett Webbjobb från Visual Studio, se till att toomark din `settings.job` filegenskaper som Kopiera om nyare.

## <a name="CreateScheduled"></a>Skapa ett schemalagda Webbjobb med hello Azure Schemaläggaren
Hej följande alternativa metoden använder hello Azure Schemaläggaren. I det här fallet saknar din Webbjobb kännedom direkt av hello schema. Hello Azure Scheduler hämtar i stället konfigurerade tootrigger din Webbjobb enligt ett schema. 

hello Azure Portal ännu inte har hello möjlighet toocreate schemalagda Webbjobb men tills funktionen läggs du kan göra det med hjälp av hello [klassiska portalen](http://manage.windowsazure.com).

1. I hello [klassiska portalen](http://manage.windowsazure.com) gå toohello Webbjobb sidan och klicka på **Lägg till**.
2. I hello **hur tooRun** väljer **körs enligt ett schema**.
   
    ![Nytt schemalagt jobb][NewScheduledJob]
3. Välj hello **Scheduler Region** för jobbet, och klicka sedan på pilen hello hello nedre högra hello dialogrutan tooproceed toohello nästa skärm.
4. I hello **skapa jobbet** dialogrutan Välj hello typ av **återkommande** önskade: **gång jobbet** eller **återkommande jobb**.
   
    ![Schemalägg återkommande][SchdRecurrence]
5. Också välja en **Start** tid: **nu** eller **vid en viss tid**.
   
    ![Starttiden för schemat][SchdStart]
6. Om du vill toostart vid en viss tid, väljer du din första tidsvärden under **startar på**.
   
    ![Schemat Start vid en viss tid][SchdStartOn]
7. Om du väljer ett återkommande projekt måste du ha hello **utför varje** alternativet toospecify hello ofta förekomst och hello **slutar på** alternativet toospecify en sluttid.
   
    ![Schemalägg återkommande][SchdRecurEvery]
8. Om du väljer **veckor**, kan du välja hello **på ett visst schema** och ange hello veckodagar hello som du vill hello jobbet toorun.
   
    ![Schemat dagarnas hello vecka][SchdWeeksOnParticular]
9. Om du väljer **månader** och välj hello **på ett visst schema** kan du ange hello jobbet toorun på visst numrerat **dagar** hello månad. 
   
    ![Schemalägga särskilda datum i hello månad][SchdMonthsOnPartDays]
10. Om du väljer **veckodagar**, du kan välja vilken dag eller hello veckodagar i hello månad hello jobbet toorun på.
    
     ![Schemalägga viss vecka dagar i månaden][SchdMonthsOnPartWeekDays]
11. Slutligen kan du också använda hello **förekomster** alternativet toochoose vilken vecka i månaden hello (första, andra, tredje o.s.v.) du vill hello jobbet toorun på hello veckodagar som du angav.
    
    ![Schemalägga viss veckodagar på viss veckor i månaden][SchdMonthsOnPartWeekDaysOccurences]
12. När du har skapat ett eller flera jobb visas namnen på hello WebJobs flik med deras status, typ och annan information. Historisk information bevaras för hello senaste 30 WebJobs.
    
    ![Lista över jobb][WebJobsListWithSeveralJobs]

### <a name="Scheduler"></a>Schemalagda jobb och Azure Schemaläggaren
Schemalagda jobb kan ytterligare konfigureras i hello Azure Scheduler sidor i hello [klassiska portalen](http://manage.windowsazure.com).

1. Hej WebJobs-sidan, klicka på hello jobbet **schema** länk toonavigate toohello Azure Scheduler Portalsida. 
   
   ![Länka tooAzure Schemaläggaren][LinkToScheduler]
2. Klicka på hello jobb hello Schemaläggaren på sidan.
   
    ![Jobb på hello Portalsida för Schemaläggaren][SchedulerPortal]
3. Hej **Jobbåtgärd** öppnas, där du kan konfigurera hello jobbet ytterligare. 
   
    ![Jobbåtgärden PageInScheduler][JobActionPageInScheduler]

## <a name="ViewJobHistory"></a>Visa hello jobbhistorik
1. tooview hello körningstiden för ett jobb, inklusive jobb som skapats med hello WebJobs SDK, klickar du på motsvarande länk under hello **loggar** kolumn av hello WebJobs-bladet. (Du kan använda hello Urklipp ikonen toocopy hello URL för hello log file sidan toohello Urklipp om du vill.)
   
    ![Loggar länk](./media/web-sites-create-web-jobs/wjbladelogslink.png)
2. Klicka på hello länken öppnar hello informationssidan för hello Webbjobb. Den här sidan visar hello av namnet på hello kommandot Kör hello senaste tidpunkten för den kördes och dess lyckats eller misslyckats. Under **senaste jobbet körs**, klicka på en gång toosee mer information.
   
    ![WebJobDetails][WebJobDetails]
3. Hej **Webbjobb kör information** visas. Klicka på **växla utdata** toosee hello text hello logga innehåll. hello utdataloggen är i textformat. 
   
    ![Web-jobbet körs information][WebJobRunDetails]
4. toosee hello utdatatext i ett nytt webbläsarfönster klickar du på hello **hämta** länk. toodownload hello själva texten, högerklicka på hello länk och använda din webbläsare alternativ toosave hello-filens innehåll.
   
    ![Hämta utdata][DownloadLogOutput]
5. Hej **WebJobs** länk hello överst på hello sidan innehåller en praktiskt sätt tooget tooa lista över WebJobs på hello historik instrumentpanel.
   
    ![Länken tooWebJobs lista][WebJobsLinkToDashboardList]
   
    ![Lista över WebJobs i instrumentpanelen för historik][WebJobsListInJobsDashboard]
   
    Klicka på någon av dessa länkar tar dig toohello Webbjobb informationssidan för hello jobb som du har valt.

## <a name="WHPNotes"></a>Anteckningar
* Webbprogram i ledigt läge kan timeout efter 20 minuter om det finns ingen webbplats med begäranden toohello scm (distribution) och portalen hello webbapp inte är öppen i Azure. Begäranden toohello faktiska webbplatsen kommer inte att återställa det här.
* Koden för en kontinuerlig jobbet måste toobe skrivs toorun i en oändlig loop.
* Kontinuerlig jobb körs alltid endast när hello webbprogram som är igång.
* Basic och Standard lägen erbjudande hello alltid på funktion som, när aktiverad förhindrar web apps får viloläge.
* Du kan bara felsöka Webbjobb som körs kontinuerligt. Schemalagd eller på begäran WebJobs-Felsökning stöds inte.

## <a name="NextSteps"></a>Nästa steg
Mer information finns i [Azure WebJobs rekommenderas resurser][WebJobsRecommendedResources].

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

