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
# <a name="run-background-tasks-with-webjobs"></a>Kör bakgrundsuppgifter med WebJobs
## <a name="overview"></a>Översikt
Du kan köra program eller skript i WebJobs i din [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webbprogrammet på tre sätt: på begäran, kontinuerligt, eller enligt ett schema. Det finns utan extra kostnad för att använda WebJobs.

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

Den här artikeln visar hur du distribuerar WebJobs med hjälp av den [Azure Portal](https://portal.azure.com). Information om hur du distribuerar med hjälp av Visual Studio eller en kontinuerlig leveransprocess finns [hur du distribuerar Azure WebJobs webbappar](websites-dotnet-deploy-webjobs.md).

Azure WebJobs SDK förenklar många WebJobs programmeringsuppgifter. Mer information finns i [vad är WebJobs SDK](websites-dotnet-webjobs-sdk.md).

 Azure Functions erbjuder ett annat sätt att köra program och skript från en serverlösa miljö eller från en Apptjänst-app. Mer information finns i [översikt över Azure Functions](../azure-functions/functions-overview.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="acceptablefiles"></a>Filtyper för skript eller program
Följande filtyper accepteras:

* .cmd, .bat, .exe (med windows cmd)
* .ps1 (med powershell)
* .SH (med bash)
* .php (med php)
* .PY (med python)
* .js (med nod)
* .JAR (med java)

## <a name="CreateOnDemand"></a>Skapa en på begäran Webbjobb i portalen
1. I den **Webbapp** bladet för den [Azure-portalen](https://portal.azure.com), klickar du på **alla inställningar > WebJobs** att visa den **WebJobs** bladet.
   
    ![Webbjobbet bladet](./media/web-sites-create-web-jobs/wjblade.png)
2. Klicka på **Lägg till**. Den **lägger till Webbjobb** visas.
   
    ![Webbjobbet bladet Lägg till](./media/web-sites-create-web-jobs/addwjblade.png)
3. Under **namn**, ange ett namn för Webbjobbet. Namnet måste börja med en bokstav eller en siffra och får inte innehålla specialtecken än ”-” och ”_”.
4. I den **hur du kör** väljer **körs på begäran**.
5. I den **filöverföring** rutan, klicka på mappikonen och bläddra till zip-filen som innehåller skriptet. Zip-filen ska innehålla den körbara filen (.exe .cmd .bat .sh .php .py .js) samt alla stödfiler som behövs för att köra program eller skript.
6. Kontrollera **skapa** att överföra skriptet till ditt webbprogram. 
   
    Namnet du angav för WebJob som visas i listan på den **WebJobs** bladet.
7. Om du vill köra Webbjobbet högerklickar du på namnet i listan och klickar på **kör**.
   
    ![Köra Webbjobbet](./media/web-sites-create-web-jobs/runondemand.png)

## <a name="CreateContinuous"></a>Skapa ett körs kontinuerligt Webbjobb
1. Om du vill skapa ett Webbjobb som körs kontinuerligt, följer du samma steg för att skapa ett Webbjobb som körs en gång, men i den **hur du kör** väljer **kontinuerlig**.
2. För att starta eller stoppa ett kontinuerligt Webbjobb, högerklicka på Webbjobb i listan och klicka på **starta** eller **stoppa**.

> [!NOTE]
> Om ditt webbprogram som körs på mer än en instans, ska ett körs kontinuerligt Webbjobb köras på alla dina instanser. Vid behov och schemalagda Webbjobb körs på en enda instans som valts för belastningsutjämning med Microsoft Azure.
> 
> Aktivera för kontinuerliga Webbjobb ska köras på ett tillförlitligt sätt och på alla instanser av Always On * konfigurationsinställning för webbprogrammet annars de kan stoppas när SCM värdplatsen har varit inaktiv för länge.
> 
> 

## <a name="CreateScheduledCRON"></a>Skapa ett schemalagda Webbjobb med ett CRON-uttryck
Den här tekniken är tillgängliga för webbprogram som körs i Basic, Standard och Premium-läge och kräver den **alltid på** inställningen är aktiverad i appen.

Om du vill aktivera en på begäran Webbjobb till ett schemalagda Webbjobb bara innehålla en `settings.job` filen i roten på din Webbjobb zip-filen. Den här JSON-filen ska innehålla en `schedule` egenskap med ett [CRON-uttryck](https://en.wikipedia.org/wiki/Cron), per exemplet nedan.

CRON-uttryck består av 6 fält: `{second} {minute} {hour} {day} {month} {day of the week}`.

Till exempel för att utlösa din Webbjobb var 15: e minut din `settings.job` skulle ha:

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

Andra CRON schema-exempel:

* Varje timme (dvs. när antalet minuter är 0):`0 0 * * * *` 
* Varje timme från 9: 00 och 17: 00:`0 0 9-17 * * *` 
* På 9:30:00 varje dag:`0 30 9 * * *`
* På 9:30:00 varje veckodag:`0 30 9 * * 1-5`

**Obs**: när du distribuerar ett Webbjobb från Visual Studio kan du se till att markera din `settings.job` filegenskaper som Kopiera om nyare.

## <a name="CreateScheduled"></a>Skapa ett schemalagda Webbjobb med hjälp av Schemaläggaren i Azure
Följande alternativ metod är användning av Schemaläggaren i Azure. I det här fallet saknar din Webbjobb kännedom direkt för schemat. I stället hämtar Azure Schemaläggaren har konfigurerats för att utlösa din Webbjobb enligt ett schema. 

Azure Portal ännu inte har möjlighet att skapa ett schemalagda Webbjobb men tills funktionen läggs du kan göra det med hjälp av den [klassiska portalen](http://manage.windowsazure.com).

1. I den [klassiska portalen](http://manage.windowsazure.com) går du till Webbjobbet och klickar på **Lägg till**.
2. I den **hur du kör** väljer **körs enligt ett schema**.
   
    ![Nytt schemalagt jobb][NewScheduledJob]
3. Välj den **Scheduler Region** för jobbet, och sedan klicka på pilen längst ned till höger i dialogrutan för att gå vidare till nästa sida.
4. I den **skapa jobbet** dialogrutan Välj typ av **återkommande** du vill: **gång jobbet** eller **återkommande jobb**.
   
    ![Schemalägg återkommande][SchdRecurrence]
5. Också välja en **Start** tid: **nu** eller **vid en viss tid**.
   
    ![Starttiden för schemat][SchdStart]
6. Om du vill starta vid en viss tid väljer du din första tidsvärden under **startar på**.
   
    ![Schemat Start vid en viss tid][SchdStartOn]
7. Om du väljer ett återkommande jobb, som du har den **utför varje** alternativet för att ange hur ofta förekomst och **slutar på** alternativet för att ange en sluttid.
   
    ![Schemalägg återkommande][SchdRecurEvery]
8. Om du väljer **veckor**, kan du välja den **på ett visst schema** och anger veckodagarna som du vill att jobbet ska köras.
   
    ![Schemat dagar i veckan][SchdWeeksOnParticular]
9. Om du väljer **månader** och välj den **på ett visst schema** kan du ange jobbet ska köras på visst numrerat **dagar** i månaden. 
   
    ![Schemat för särskilda datum i månaden][SchdMonthsOnPartDays]
10. Om du väljer **veckodagar**, du kan välja eller vilka dagar i veckan i månaden som du vill att jobbet ska köras på.
    
     ![Schemalägga viss vecka dagar i månaden][SchdMonthsOnPartWeekDays]
11. Slutligen kan du kan också använda den **förekomster** möjlighet att välja vilken vecka i månaden (första, andra, tredje o.s.v.) du vill att jobbet ska köras på veckodagar som du angav.
    
    ![Schemalägga viss veckodagar på viss veckor i månaden][SchdMonthsOnPartWeekDaysOccurences]
12. När du har skapat ett eller flera jobb visas namnen på fliken WebJobs med deras status, typ och annan information. Historisk information för de senaste 30 WebJobs bevaras.
    
    ![Lista över jobb][WebJobsListWithSeveralJobs]

### <a name="Scheduler"></a>Schemalagda jobb och Azure Schemaläggaren
Schemalagda jobb kan ytterligare konfigureras i Azure Scheduler-sidor i den [klassiska portalen](http://manage.windowsazure.com).

1. Jobbets på sidan WebJobs **schema** länken för att gå till sidan Schemaläggaren i Azure portal. 
   
   ![Länk till Azure Schemaläggaren][LinkToScheduler]
2. På sidan Schemaläggaren, klickar du på jobbet.
   
    ![Jobb på portalsidan Schemaläggaren][SchedulerPortal]
3. Den **Jobbåtgärd** öppnas, där du kan konfigurera jobbet ytterligare. 
   
    ![Jobbåtgärden PageInScheduler][JobActionPageInScheduler]

## <a name="ViewJobHistory"></a>Visa jobbets historik
1. Om du vill visa historiken för körning av ett jobb, inklusive jobb som skapats med WebJobs-SDK klickar du på motsvarande länk under den **loggar** kolumn i bladet WebJobs. (Du kan använda ikonen Urklipp Kopiera Webbadressen till sidan log file till Urklipp om du vill.)
   
    ![Loggar länk](./media/web-sites-create-web-jobs/wjbladelogslink.png)
2. Klicka på länken öppnar informationssidan för Webbjobbet. Den här sidan visar namnet på kommandot Kör den senaste tidpunkten för den kördes och dess lyckats eller misslyckats. Under **senaste jobbet körs**, klicka på en gång för att se ytterligare information.
   
    ![WebJobDetails][WebJobDetails]
3. Den **Webbjobb kör information** visas. Klicka på **växla utdata** att se texten i logginnehållet. Logg för utdata är i textformat. 
   
    ![Web-jobbet körs information][WebJobRunDetails]
4. Klicka för att visa den resulterande texten i ett nytt webbläsarfönster i **hämta** länk. Högerklicka på länken för att hämta själva texten, och Använd webbläsaren för att spara filinnehållet.
   
    ![Hämta utdata][DownloadLogOutput]
5. Den **WebJobs** längst upp på sidan är ett bekvämt sätt att komma till en lista över WebJobs på instrumentpanelen historik.
   
    ![Länka till WebJobs-lista][WebJobsLinkToDashboardList]
   
    ![Lista över WebJobs i instrumentpanelen för historik][WebJobsListInJobsDashboard]
   
    Klicka på någon av dessa länkar går du till sidan Webbjobb information för jobbet som du har valt.

## <a name="WHPNotes"></a>Anteckningar
* Webbprogram i ledigt läge kan timeout efter 20 minuter om det finns inga förfrågningar till webbplatsen scm (distribution) och webbappens portalen inte är öppen i Azure. Begäranden till den faktiska platsen kommer inte att återställa det här.
* Koden för en kontinuerlig jobbet måste skrivas ska köras i en oändlig loop.
* Kontinuerlig jobben körs alltid endast när webbappen är igång.
* Basic och Standard lägen erbjudande den alltid på Funktion, som när aktiverad förhindrar web apps får viloläge.
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

