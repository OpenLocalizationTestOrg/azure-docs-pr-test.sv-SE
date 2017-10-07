---
title: "aaaApplication prestanda vanliga frågor och svar för Azure-webbappar | Microsoft Docs"
description: "Få svar toofrequently frågor och svar om tillgänglighet, prestanda och problem i hello Web Apps-funktionen i Azure App Service."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 7f2383743079e4c630fd548b0efd9993029afe11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-performance-faqs-for-web-apps-in-azure"></a>Programprestanda vanliga frågor och svar för Web Apps i Azure

Den här artikeln innehåller svar toofrequently frågor (FAQ) och om problem med prestanda för hello [funktionen Web Apps i Azure App Service](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-is-my-app-slow"></a>Varför är min långsam appen?

Flera faktorer kan bidra tooslow prestanda. För detaljerad felsökning, se [Felsök långsam web app prestanda](app-service-web-troubleshoot-performance-degradation.md).

## <a name="how-do-i-troubleshoot-a-high-cpu-consumption-scenario"></a>Hur felsöker jag en hög CPU-förbrukning scenario?

I vissa hög CPU-förbrukning, kan din app verkligen kräva mer datorresurser. I så fall bör du skalning tooa högre tjänstnivå så hello programmet hämtar alla hello resurser som behövs. Andra tider hög CPU-användning kan bero på en felaktig loop eller kodning praxis. Få inblick i vad utlösa ökade CPU-användning är en process i två steg. Först skapar en processdump och analysera hello processdump. Mer information finns i [samla in och analysera en dumpfil för hög CPU-användning för Web Apps](https://blogs.msdn.microsoft.com/asiatech/2016/01/20/how-to-capture-dump-when-intermittent-high-cpu-happens-on-azure-web-app/).

## <a name="how-do-i-troubleshoot-a-high-memory-consumption-scenario"></a>Hur felsöker ett scenario med hög minnesförbrukning?

I vissa scenarier med hög minnesförbrukning kräva appen verkligen mer datorresurser. I så fall bör du skalning tooa högre tjänstnivå så hello programmet hämtar alla hello resurser som behövs. Andra gånger kan ett programfel i hello kod orsaka en minnesläcka. Kodning praxis kan också öka minnesanvändningen. Få inblick i vad utlösa höga minnesområdet förbrukningen är en process i två steg. Först skapar en processdump och analysera hello processdump. Krasch Diagnoser från hello Azure Galleri för tillägget kan effektivt utföra båda dessa steg. Mer information finns i [samla in och analysera en dumpfil för återkommande höga minnesområdet för Web Apps](https://blogs.msdn.microsoft.com/asiatech/2016/02/02/how-to-capture-and-analyze-dump-for-intermittent-high-memory-on-azure-web-app/).

## <a name="how-do-i-automate-app-service-web-apps-by-using-powershell"></a>Hur jag för att automatisera App Service web apps med hjälp av PowerShell?

Du kan använda PowerShell-cmdlets toomanage och underhålla App Service web apps. I vår blogginlägget [automatisera webbprogram finns i Azure App Service med hjälp av PowerShell](https://blogs.msdn.microsoft.com/puneetgupta/2016/03/21/automating-webapps-hosted-in-azure-app-service-through-powershell-arm-way/), beskrivs hur vanliga uppgifter för tooautomate toouse Azure Resource Manager-baserade PowerShell-cmdlets. hello blogginlägget har också exempelkod för olika hanteringsuppgifter för web apps. Beskrivningar och syntaxen för alla App Service web apps-cmdletar finns [AzureRM.Websites](https://docs.microsoft.com/powershell/module/azurerm.websites/?view=azurermps-4.0.0).

## <a name="how-do-i-view-my-web-apps-event-logs"></a>Hur visar jag min webbprogrammet händelseloggar

tooview ditt webbprogram händelseloggarna:

1. Logga in tooyour [Kudu webbplats](https://*yourwebsitename*.scm.azurewebsites.net).
2. Välj menyn hello **Felsökningskonsolen** > **CMD**.
3. Välj hello **loggfiler** mapp.
4. tooview händelseloggar, Välj för hello pennikonen bredvid**eventlog.xml**.
5. toodownload hello loggar, Kör PowerShell-cmdlet för hello `Save-AzureWebSiteLog -Name webappname`.

## <a name="how-do-i-capture-a-user-mode-memory-dump-of-my-web-app"></a>Hur jag för att avbilda en användarläge minnesdump för webbappen?

toocapture en användarläge minnesdump av ditt webbprogram:

1. Logga in tooyour [Kudu webbplats](https://*yourwebsitename*.scm.azurewebsites.net).
2. Välj hello **Processutforskaren** menyn.
3. Högerklicka på hello **w3wp.exe** eller Webbjobb-processen.
4. Välj **hämta minnesdump** > **fullständig Dump**.

## <a name="how-do-i-view-process-level-info-for-my-web-app"></a>Hur visar jag processnivå info för webbappen

Har du två alternativ för att visa processnivå information för ditt webbprogram:

*   I hello Azure-portalen:
    1. Öppna hello **Processutforskaren** för hello webbprogrammet.
    2. toosee hello information, Välj hello **w3wp.exe** process.
*   I hello Kudu-konsolen:
    1. Logga in tooyour [Kudu webbplats](https://*yourwebsitename*.scm.azurewebsites.net).
    2. Välj hello **Processutforskaren** menyn.
    3. För hello **w3wp.exe** process, Välj **egenskaper**.

## <a name="when-i-browse-toomy-app-i-see-error-403---this-web-app-is-stopped-how-do-i-resolve-this"></a>När jag söker toomy appen finns ”felet 403 – det här webbprogrammet har stoppats”. Hur lösa problemet?

Tre villkoren kan orsaka detta fel:

* hello webbprogrammet har nått gränsen för fakturering och webbplatsen har inaktiverats.
* hello webbprogrammet har stoppats på hello-portalen.
* hello webbprogrammet har nått en resursgräns kvot som kan uppstå tooa kostnadsfri eller delad skala service-plan.

toosee vad som orsakar fel hello och tooresolve hello problemet, följ hello stegen i [Web Apps: ”fel 403 – det här webbprogrammet har stoppats”](https://blogs.msdn.microsoft.com/waws/2016/01/05/azure-web-apps-error-403-this-web-app-is-stopped/).

## <a name="where-can-i-learn-more-about-quotas-and-limits-for-various-app-service-plans"></a>Var kan jag läsa mer om kvoter och gränser för olika programtjänstplaner?

Information om kvoter och begränsningar finns [gränser för Apptjänst](../azure-subscription-service-limits.md#app-service-limits). 

## <a name="how-do-i-decrease-hello-response-time-for-hello-first-request-after-idle-time"></a>Hur jag för att minska hello svarstiden för hello första förfrågan efter inaktivitet?

Som standard inaktiveras webbappar om de är inaktiv under en angiven tidsperiod. Det här sättet hello system kan spara resurser. hello Nackdelen är att hello svar toohello första begäran när hello webbprogrammet tas bort är längre, tooallow hello web app tooload och starta som man betjänar svar. I Basic och Standard serviceplaner kan du aktivera hello **alltid på** inställningen tookeep hello app alltid läses in. Detta eliminerar längre inläsningstiden när hello app är inaktiv. toochange hello **alltid på** inställningen:

1. Gå tooyour webbprogram i hello Azure-portalen.
2. Välj **programinställningar**.
3. För **alltid på**väljer **på**.

## <a name="how-do-i-turned-on-failed-request-tracing"></a>Hur jag aktiverade spårning av misslyckade begäranden?

tooturn spårning för misslyckade begäranden:

1. Gå tooyour webbprogram i hello Azure-portalen.
3. Välj **alla inställningar** > **diagnostik loggar**.
4. För **spårning av misslyckade begäranden**väljer **på**.
5. Välj **Spara**.
6. Hello web app bladet välj **verktyg**.
7. Välj **Visual Studio Online**.
8. Om hello inställningen inte är **på**väljer **på**.
9. Välj **Gå**.
10. Välj **Web.config**.
11. System.webServer, Lägg till den här konfigurationen (toocapture en specifik URL):

    ```
    <system.webServer>
    <tracing> <traceFailedRequests>
    <remove path="*api*" />
    <add path="*api*">
    <traceAreas>
    <add provider="ASP" verbosity="Verbose" />
    <add provider="ASPNET" areas="Infrastructure,Module,Page,AppServices" verbosity="Verbose" />
    <add provider="ISAPI Extension" verbosity="Verbose" />
    <add provider="WWW Server" areas="Authentication,Security,Filter,StaticFile,CGI,Compression, Cache,RequestNotifications,Module,FastCGI" verbosity="Verbose" />
    </traceAreas>
    <failureDefinitions statusCodes="200-999" />
    </add> </traceFailedRequests>
    </tracing>
    ```
12. problem med tootroubleshoot långsamma prestanda, Lägg till den här konfigurationen (om hello avbildning av begäran tar mer än 30 sekunder):
    ```
    <system.webServer>
    <tracing> <traceFailedRequests>
    <remove path="*" />
    <add path="*">
    <traceAreas> <add provider="ASP" verbosity="Verbose" />
    <add provider="ASPNET" areas="Infrastructure,Module,Page,AppServices" verbosity="Verbose" />
    <add provider="ISAPI Extension" verbosity="Verbose" />
    <add provider="WWW Server" areas="Authentication,Security,Filter,StaticFile,CGI,Compression, Cache,RequestNotifications,Module,FastCGI" verbosity="Verbose" />
    </traceAreas>
    <failureDefinitions timeTaken="00:00:30" statusCodes="200-999" />
    </add> </traceFailedRequests>
    </tracing>
    ```
13. toodownload hello misslyckades begäranden i hello [portal](https://portal.azure.com)går tooyour webbplats.
15. Välj **verktyg** > **Kudu** > **Gå**.
18. Välj menyn hello **Felsökningskonsolen** > **CMD**.
19. Välj hello **loggfiler** mapp och välj sedan hello mapp med ett namn som börjar med **W3SVC**.
20. toosee hello XML-fil, Välj hello pennikonen.

## <a name="i-see-hello-message-worker-process-requested-recycle-due-toopercent-memory-limit-how-do-i-address-this-issue"></a>Hello-meddelande ”arbetsprocess begärt återvinning förfaller too'Percent minne ' gränsen”. Hur åtgärdar problemet?

hello är maximalt tillgängligt minne för en 32-bitarsprocess (även på ett 64-bitars operativsystem) 2 GB. Hello arbetsprocessen är som standard too32-bitars i App Service (för kompatibilitet med äldre webbprogram).

Överväg att byta too64-bitars processer, så du kan dra nytta av hello extra minne tillgängligt i Web Worker-rollen. Detta utlöser en web app omstart, så schemalägga därefter.

Observera också att en 64-bitars miljö kräver en serviceplan Basic eller Standard. Frigör och delade planer köras alltid i en 32-bitars-miljö.

Mer information finns i [konfigurera webbappar i App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-configure).

## <a name="why-does-my-request-time-out-after-240-seconds"></a>Varför har min begäran-timeout efter 240 sekunder?

Azure belastningsutjämnare har en standardinställning för timeout för inaktivitet fyra minuter. Detta är vanligtvis en rimlig svar tidsgräns för en webbegäran. Om ditt webbprogram kräver behandling i bakgrunden, bör du använda Azure WebJobs. hello Azure webbapp kan anropa WebJobs och aviseras när behandling i bakgrunden är klar. Du kan välja mellan flera metoder för att använda WebJobs, inklusive köer och utlösare.

WebJobs är avsedd för behandling i bakgrunden. Du kan göra så mycket bakgrund bearbetning av som du vill använda i ett Webbjobb. Läs mer om WebJobs [kör bakgrundsaktiviteter med WebJobs](https://docs.microsoft.com/azure/app-service-web/web-sites-create-web-jobs).

## <a name="aspnet-core-applications-that-are-hosted-in-app-service-sometimes-stop-responding-how-do-i-fix-this-issue"></a>ASP.NET Core program som finns i App Service ibland slutar svara. Hur kan jag åtgärda det här problemet?

Ett känt problem med en tidigare [Kestrel version](https://github.com/aspnet/KestrelHttpServer/issues/1182) kan orsaka en ASP.NET Core 1.0-app som finns i Apptjänst toointermittently slutar svara. Du kan också se meddelandet: ”hello angetts CGI-programmet påträffade ett fel och hello avslutas hello serverprocessen”.

Det här problemet löses i Kestrel version 1.0.2. Den här versionen ingår i hello ASP.NET Core 1.0.3 uppdateringen. tooresolve problemet kan kontrollera att du uppdaterar din app beroenden toouse Kestrel 1.0.2. Du kan också använda en av två lösningar som beskrivs i hello blogginlägget [ASP.NET Core 1.0 långsam prestanda problem i App Service web apps](https://blogs.msdn.microsoft.com/waws/2016/12/11/asp-net-core-slow-perf-issues-on-azure-websites).


## <a name="i-cant-find-my-log-files-in-hello-file-structure-of-my-web-app-how-can-i-find-them"></a>Jag kan inte hitta min loggfiler i hello filstruktur för webbappen. Hur hittar jag dem?

Om du använder hello lokala cachefunktionen i App Service påverkas hello mappstruktur för hello loggfiler och mappar för din App Service-instans. När lokal Cache används skapas undermappar i hello lagring loggfiler och mappar. hello undermappar Använd hello namngivning mönstret ”UID” + tidsstämpel. Varje undermapp motsvarar tooa VM-instans i vilka hello webbprogram som körs eller har körts.

toodetermine om du använder lokala cachen, kontrollera din Apptjänst **programinställningar** fliken. Om det lokala cacheminnet används hello appinställningen `WEBSITE_LOCAL_CACHE_OPTION` har angetts för`Always`. Mer information om lokala cachen finns hello [App lokala cacheminne översikt](https://docs.microsoft.com/azure/app-service/app-service-local-cache).

Om du inte använder lokal Cache och har det här problemet kan du skicka en supportbegäran.

## <a name="i-see-hello-message-an-attempt-was-made-tooaccess-a-socket-in-a-way-forbidden-by-its-access-permissions-how-do-i-resolve-this"></a>Hello-meddelande ”ett försök har gjorts tooaccess en socket på ett sätt som tillåts inte av åtkomstbehörigheterna”. Hur lösa problemet?

Det här felet uppstår vanligen om hello utgående TCP-anslutningar på hello VM-instans är slut. I App Service tillämpas begränsningar för hello maximalt antal utgående anslutningar som kan göras för varje VM-instans. Mer information finns i [mellan VM numeriska gränser](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#cross-vm-numerical-limits).

Det här felet kan också uppstå om du försöker tooaccess en lokal adress från ditt program. Mer information finns i [lokal adressbegäranden](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#local-address-requests).

Mer information om utgående anslutningar i ditt webbprogram finns hello blogginlägget om [utgående anslutningar tooAzure webbplatser](http://www.freekpaans.nl/2015/08/starving-outgoing-connections-on-windows-azure-web-sites/).

## <a name="how-do-i-use-visual-studio-tooremote-debug-my-app-service-web-app"></a>Hur använder jag felsökning i Visual Studio tooremote min App Service webbapp?

En detaljerad genomgång som visar hur toodebug ditt webbprogram med hjälp av Visual Studio, se [Remote felsöka din Apptjänst-webbapp](https://blogs.msdn.microsoft.com/benjaminperkins/2016/09/22/remote-debug-your-azure-app-service-web-app/).
