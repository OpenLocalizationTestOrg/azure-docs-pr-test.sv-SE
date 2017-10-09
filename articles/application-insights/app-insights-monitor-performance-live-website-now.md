---
title: aaaMonitor live ASP.NET web app med Azure Application Insights | Microsoft Docs
description: "Övervaka prestanda för en webbplats utan att distribuera den igen. Fungerar med ASP.NET-webbappar som finns lokalt, i virtuella datorer eller på Azure."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: 0d53f0a59974f40767fae681bafc4f358d1283a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="instrument-web-apps-at-runtime-with-application-insights"></a>Instrumentera webbappar vid körning med Application Insights


Du kan instrumentera en live-webbapp med Azure Application Insights utan toomodify eller distribuera din kod. Om dina appar hanteras av en lokal IIS-server installerar du	Statusövervakare. Om de Azure-webbappar eller köras i en Azure VM, kan du växla på Application Insights-övervakning från hello Azure på Kontrollpanelen. (Det finns även olika artiklar om hur du instrumenterar [J2EE-livewebbappar](app-insights-java-live.md) och [Azure Cloud Services](app-insights-cloudservices.md).) Du behöver en [Microsoft Azure](http://azure.com)-prenumeration.

![exempeldiagram](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

Du kan välja mellan tre vägar tooapply Application Insights tooyour .NET-webbprogram:

* **Byggtid:** [hello Lägg till Application Insights SDK] [ greenbrown] tooyour web app-kod.
* **Körtid:** Instrumentera ditt webbprogram på hello-servern enligt beskrivningen nedan, utan att återskapa och omdistribuera hello kod.
* **Både:** skapa hello SDK i koden web app och gäller även hello körning tillägg. Hämta hello bästa av båda alternativen.

Här är en sammanfattning av vad du får med respektive väg:

|  | Byggtid | Körtid |
| --- | --- | --- |
| Förfrågningar och undantag |Ja |Ja |
| [Mer detaljerade undantag](app-insights-asp-net-exceptions.md) | |Ja |
| [Beroendediagnostik](app-insights-asp-net-dependencies.md) |I .NET 4.6+, men färre detaljer |Ja, fullständiga detaljer: resultatkoder, SQL-kommandotext, HTTP verb|
| [Systemprestandaräknare](app-insights-performance-counters.md) |Ja |Ja |
| [API för anpassad telemetri][api] |Ja |Nej |
| [Spårningsloggsintegrering](app-insights-asp-net-trace-logs.md) |Ja |Nej |
| [Sidvy och användardata](app-insights-javascript.md) |Ja |Nej |
| Behöver toorebuild kod |Ja | Nej |


## <a name="monitor-a-live-azure-web-app"></a>Övervaka en Azure-livewebbapp

Om programmet körs som en Azure web tjänstens, här hur tooswitch om övervakning av:

* Välj Application Insights på Kontrollpanelen för hello app i Azure.

    ![Konfigurera Application Insights för en Azure-webbapp](./media/app-insights-monitor-performance-live-website-now/azure-web-setup.png)
* När hello Application Insights sammanfattningssidan öppnas på hello längst hello nedre tooopen hello fullständig Application Insights-resurs.

    ![Klicka dig igenom tooApplication insikter](./media/app-insights-monitor-performance-live-website-now/azure-web-view-more.png)

[Övervaka appar för moln och virtuella datorer](app-insights-azure.md).

### <a name="enable-client-side-monitoring-in-azure"></a>Aktivera övervakning på klientsidan i Azure

Om du har aktiverat Application Insights i Azure kan du lägga till sidvy och användartelemetri.

1. Välj Inställningar > Programinställningar
2.  Under Appinställningar lägger du till ett nytt nyckel/värde-par: 
   
    Nyckel: `APPINSIGHTS_JAVASCRIPT_ENABLED` 
    
    Värde:`true`

3. **Spara** hello inställningar och **starta om** din app.

hello Application Insights JavaScript SDK är nu injekteras i varje webbsida.

## <a name="monitor-a-live-iis-web-app"></a>Övervaka en IIS-livewebbapp

Om din app finns på en IIS-server aktiverar du Application Insights med hjälp av Statusövervakaren.

1. Logga in med administratörsbehörighet på IIS-webbservern.
2. Om Application Insights Status Monitor inte redan är installerad, hämta och köra hello [statusövervakaren installer](http://go.microsoft.com/fwlink/?LinkId=506648) (eller kör [installationsprogram för webbplattform](https://www.microsoft.com/web/downloads/platform.aspx) och Sök i den för Application Insights Status Övervaka).
3. Välj hello installerade webbprogram eller en webbplats som du vill toomonitor i Status Monitor. Logga in med dina Azure autentiseringsuppgifter.

    Konfigurera hello resurs där du vill att toosee hello resulterar i hello Application Insights-portalen. (Normalt, är det bästa toocreate en ny resurs. Välj en befintlig resurs om du redan har [webbtester][availability] eller [klientövervakning][client] för den här appen.) 

    ![Välj en app och en resurs.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

4. Starta om IIS.

    ![Välj omstart hello överst i dialogrutan hello.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    Webbtjänsten avbryts en liten stund.

## <a name="customize-monitoring-options"></a>Anpassa övervakningsalternativ

Aktivera Application Insights lägger till DLL-filer och ApplicationInsights.config tooyour webbprogram. Du kan [redigera hello .config-filen](app-insights-configuration-with-applicationinsights-config.md) toochange vissa av hello alternativ.

## <a name="when-you-re-publish-your-app-re-enable-application-insights"></a>När du publicerar appen igen ska du återaktivera Application Insights

Innan du publicerar appen igen bör du överväga att [lägga till Application Insights toohello kod i Visual Studio][greenbrown]. Du får mer detaljerad telemetri och anpassad telemetri för hello möjlighet toowrite.

Om du vill toore-publicera utan att lägga till Application Insights toohello kod, Tänk på att hello distributionsprocessen kan ta bort hello DLL: er och ApplicationInsights.config från hello publicerat webbplats. Därför:

1. Om du har redigerat ApplicationInsights.config tar du en kopia av den innan du publicerar appen igen.
2. Publicera om appen.
3. Återaktivera övervakning med Application Insights. (Använd lämplig metod för hello: hello Azure web app på Kontrollpanelen eller hello Status Monitor på en IIS-värd.)
4. Återställa alla ändringar utförs på hello .config-fil.


## <a name="troubleshooting-runtime-configuration-of-application-insights"></a>Felsöka körningskonfigurationen av Application Insights

### <a name="cant-connect-no-telemetry"></a>Går det inte att ansluta? Ser du ingen telemetri?

* Öppna [hello nödvändiga utgående portar](app-insights-ip-addresses.md#outgoing-ports) i serverns brandvägg tooallow statusövervakaren toowork.

* Öppna Status Monitor och välj ditt program i den vänstra rutan. Kontrollera om det finns några diagnostiska meddelanden för det här programmet i hello ”konfigurationen meddelanden” avsnittet:

  ![Öppna bladet toosee begäran om hello prestanda, svarstid, beroende och andra data](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
* Om du ser ett meddelande om ”otillräckliga behörigheter” på servern hello, försök med hello följande:
  * I IIS-hanteraren, Välj programpoolen, öppna **avancerade inställningar**, och under **processmodellen** Observera hello identitet.
  * Lägg till den här identiteten toohello prestandaövervakningsanvändare datorn management på Kontrollpanelen.
* Om du har MMA/SCOM (Systems Center Operations Manager) installerat på servern kan vissa versioner vara i konflikt med varandra. Avinstallerar du både SCOM och statusövervakaren och installera om hello senaste versionerna.
* Mer information finns i [Felsökning][qna].

## <a name="system-requirements"></a>Systemkrav
Operativsystemstöd för Application Insights Status Monitor på servern:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows server 2012 R2
* Windows Server 2016

med senaste SP och .NET Framework 4.5

På klientsidan för hello: Windows 7, 8, 8.1 och 10 igen med .NET Framework 4.5

IIS-stöd: IIS 7, 7.5, 8, 8.5 (IIS krävs)

## <a name="automation-with-powershell"></a>Automatisering med PowerShell
Du kan starta och stoppa övervakningen med hjälp av PowerShell i din IIS-server.

Först importera hello Application Insights-modulen:

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

Ta reda på vilka appar som övervakas:

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* `-Name`(Valfritt) hello namnet på ett webbprogram.
* Visar hello Application Insights övervakningsstatus för varje webbprogram (eller hello med namnet app) i den här IIS-servern.
* Returnerar `ApplicationInsightsApplication` för varje app:

  * `SdkState==EnabledAfterDeployment`: App som övervakas och var instrumenterats vid körning av hello statusövervakaren verktyg eller genom `Start-ApplicationInsightsMonitoring`.
  * `SdkState==Disabled`: hello app inte instrumenterats för Application Insights. Det har aldrig instrumenterats eller körning övervakning har inaktiverats hello statusövervakaren verktyget eller med `Stop-ApplicationInsightsMonitoring`.
  * `SdkState==EnabledByCodeInstrumentation`: hello app har instrumenterats genom att lägga till hello SDK toohello källkoden. Appens SDK kan inte uppdateras eller stoppas.
  * `SdkVersion`Visar hello-version som används för att övervaka den här appen.
  * `LatestAvailableSdkVersion`Visar hello-version som är tillgängliga på hello NuGet-galleriet. tooupgrade hello toothis programversion, Använd `Update-ApplicationInsightsMonitoring`.

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* `-Name`hello namnet på hello app i IIS
* `-InstrumentationKey`Hej ikey av hello Application Insights-resurs där du vill att hello resultat toobe visas.
* Den här cmdleten påverkar endast appar som inte redan har instrumenterats, dvs. SdkState==NotInstrumented.

    hello cmdlet påverkar inte en app som redan instrumenterats. Den inte roll om hello app har instrumenterats vid byggning genom att lägga till hello SDK toohello kod eller vid körningstiden på en tidigare användning av denna cmdlet.

    hello SDK version som används för tooinstrument hello appen är hello-versionen som senast ned toothis server.

    toodownload hello senaste versionen, använda uppdateringen ApplicationInsightsVersion.
* Returnerar `ApplicationInsightsApplication` om åtgärden lyckades. Om det misslyckas, loggar en trace toostderr.

          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* `-Name`hello namnet på en app i IIS
* `-All` Stoppar övervakningen av alla appar på IIS-servern där `SdkState==EnabledAfterDeployment`
* Stoppar övervakningen av hello angivna appar och tar bort instrumentation. Den fungerar bara för appar som har varit instrumenterats på körning med hello övervakning av innehållsstatus verktyg eller starta ApplicationInsightsApplication. (`SdkState==EnabledAfterDeployment`)
* Returnerar ApplicationInsightsApplication.

`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]

* `-Name`: hello namnet på en webbapp i IIS.
* `-InstrumentationKey` (Valfritt.) Använd den här toochange hello resurs toowhich hello appens telemetri skickas.
* Den här cmdleten:
  * Uppgraderingar hello med namnet app toohello version av hello SDK senast hämtats toothis datorn. (Fungerar bara om `SdkState==EnabledAfterDeployment`)
  * Om du anger en instrumentation nyckel är hello med namnet app omkonfigurerats toosend telemetri toohello resursen med nyckeln. (Fungerar om `SdkState != Disabled`)

`Update-ApplicationInsightsVersion`

* Hämtar hello senaste Application Insights SDK toohello server.

## <a name="questions"></a>Frågor om Statusövervakaren

### <a name="what-is-status-monitor"></a>Vad är Statusövervakaren?

Ett program som du installerar på din IIS-webbserver. Det hjälper dig instrumentera och konfigurera webbappar. 

### <a name="when-do-i-use-status-monitor"></a>När ska jag använda Statusövervakaren?

* tooinstrument någon webbapp som körs på IIS-servern - även om det redan körs.
* tooenable ytterligare telemetri för webbprogram som har [byggts med hello Application Insights SDK](app-insights-asp-net.md) vid kompileringen. 

### <a name="can-i-close-it-after-it-runs"></a>Kan jag stänga den när den körs?

Ja. Du kan stänga när den har instrumenterats hello webbplatser som du väljer.

Den samlar inte in telemetri på egen hand. Bara konfigurerar hello webbprogram och anger vissa behörigheter.

### <a name="what-does-status-monitor-do"></a>Vad gör Statusövervakaren?

När du väljer en webbapp för statusövervakaren tooinstrument:

* Hämtar och placerar hello Application Insights sammansättningar och .config-fil i hello webbapp binärfiler mapp.
* Ändrar `web.config` tooadd hello Application Insights http-spårning modul.
* Aktiverar CLR profilering toocollect beroendeanrop.

### <a name="do-i-need-toorun-status-monitor-whenever-i-update-hello-app"></a>Behöver jag toorun statusövervakaren när jag uppdaterar hello app?

Inte om du distribuerar om inkrementellt. 

Om du väljer hello ”ta bort befintliga filer' i hello publiceringsprocess, behöver du toore kör statusövervakaren tooconfigure Application Insights.

### <a name="what-telemetry-is-collected"></a>Vilken telemetri samlas in?

För program som du endast instrumenterar vid körning med Statusövervakaren:

* HTTP-begäranden
* Anropar toodependencies
* Undantag
* Prestandaräknare

För program som redan har instrumenterats vid kompilering:

 * Processräknare.
 * Beroendeanrop (.NET 4.5;), returvärden i beroendeanrop (.NET 4.6).
 * Undantag i stackspårningsvärden.

[Läs mer](http://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next"></a>Nästa steg

Visa telemetrin:

* [Utforska mått](app-insights-metrics-explorer.md) toomonitor prestanda och användning
* [Söka efter händelser och loggar] [ diagnostic] toodiagnose problem
* [Analys](app-insights-analytics.md) för mer avancerade frågor
* [Skapa instrumentpaneler](app-insights-dashboards.md)

Lägg till mer telemetri:

* [Skapa webbtester] [ availability] toomake säker webbplatsen är aktiv.
* [Lägg till webbplats klienten telemetri] [ usage] toosee undantag från webbsidan kod och toolet du infogar spåra anrop.
* [Lägg till Application Insights SDK tooyour kod] [ greenbrown] så att du kan infoga spårning och logga anrop

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-javascript.md
