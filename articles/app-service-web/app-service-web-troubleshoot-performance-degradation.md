---
title: aaaSlow web app prestanda i App Service | Microsoft Docs
description: "Den här artikeln hjälper dig att felsöka problem med långsam web app prestanda i Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: top-support-issue
keywords: "Web app prestanda, långsam app app som är långsam"
ms.assetid: b8783c10-3a4a-4dd6-af8c-856baafbdde5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: cephalin
ms.openlocfilehash: 3e56b99b48db0e7baae1fac797a7fcb9eff74c9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Felsöka problem med långsam web app prestanda i Azure App Service
Den här artikeln hjälper dig att felsöka problem med långsam web app prestanda i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och hello Stack Overflow-forum](https://azure.microsoft.com/support/forums/). Alternativt kan du även filen en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och klicka på **Get Support**.

## <a name="symptom"></a>Symtom
När du bläddrar hello webbprogrammet sidor hello belastningen långsamt och ibland timeout.

## <a name="cause"></a>Orsak
Det här problemet beror ofta på nivån programproblem som:

* nätverksbegäranden tar lång tid
* program-kod eller database-frågor som är ineffektiv
* program med hög minne/processor
* programmet kraschar på grund av tooan undantag

## <a name="troubleshooting-steps"></a>Felsökningssteg
Felsökning kan delas in i tre olika uppgifter i sekventiell ordning:

1. [Observera och övervaka programmets beteende](#observe)
2. [Samla in data](#collect)
3. [Minimera hello problemet](#mitigate)

[App Service Web Apps](/services/app-service/web/) ger olika alternativ i varje steg.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Observera och övervaka programmets beteende
#### <a name="track-service-health"></a>Spåra tjänstens hälsa
Microsoft Azure publicizes varje gång en tjänst avbrott eller prestanda försämras. Du kan spåra hello hälsotillståndet för hello-tjänsten på hello [Azure-portalen](https://portal.azure.com/). Mer information finns i [spåra tjänstens hälsa](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Övervaka ditt webbprogram
Det här alternativet kan du toofind ut om programmet har med eventuella problem. I din webbapps blad klickar du på hello **begäranden och fel** panelen. Hej **mått** bladet visar alla hello mått som du kan lägga till.

Några av hello mått som du kanske vill toomonitor för webbappen

* Genomsnittlig minne arbetsminne
* Genomsnittlig svarstid
* CPU-tid
* Arbetsminnet för minne
* Begäranden

![övervaka prestanda för web app](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Mer information finns i:

* [Övervakaren Web Apps i Azure App Service](web-sites-monitor.md)
* [Få varningsmeddelanden](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Övervaka status på slutpunkt
Om du kör ditt webbprogram i hello **Standard** prisnivån, Web Apps kan du övervaka två slutpunkter från tre geografiska platser.

Slutpunktsövervakning konfigurerar webbtester från fördelade platser som testar svarstid och upptid för webbadresser. hello testet utför en HTTP GET-åtgärd på hello web URL toodetermine hello-svarstid och drifttid från varje plats. Varje konfigurerad plats kör ett test var femte minut.

Drifttid kontrolleras med hjälp av HTTP-svarskoder och svarstid mäts i millisekunder. En övervakning testet misslyckas om hello HTTP-svarskoden är större än eller lika med too400 eller om hello svar tar mer än 30 sekunder. En slutpunkt anses vara tillgänglig om övervakning testerna lyckas från alla hello angivna platser.

tooset den, se [övervaka appar i Azure App Service](web-sites-monitor.md).

Se även [hålla Azure-webbplatser in plus Slutpunktsövervakning - med Stefan Schackow](https://channel9.msdn.com/Shows/Azure-Friday/Keeping-Azure-Web-Sites-up-plus-Endpoint-Monitoring-with-Stefan-Schackow) en video om slutpunktsövervakning.

#### <a name="application-performance-monitoring-using-extensions"></a>Övervakning av programprestanda med tillägg
Du kan också övervaka programmets prestanda genom att använda *plats tillägg*.

Varje App Service webbapp tillhandahåller en utökningsbar management-slutpunkt som du kan använda toouse en kraftfull uppsättning av verktyg som distribueras som webbplatstillägg. Tillägg är: 

- Källan kod redigerare som [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx). 
- Hanteringsverktyg för anslutna resurser, till exempel en MySQL-databas som är anslutna tooa webbprogram.

[Azure Application Insights](/services/application-insights/) och [New Relic](/marketplace/partners/newrelic/newrelic/) finns två hello webbplatstillägg som är tillgängliga för prestandaövervakning. toouse New Relic du installerar en agent vid körning. toouse Azure Application Insights du återskapar din kod med en SDK och du kan också installera ett tillägg som ger åtkomst till tooadditional data. hello SDK kan du skriva kod toomonitor hello användnings- och prestanda för din app i detalj.

toouse Application Insights finns [övervaka prestanda i webbprogram](../application-insights/app-insights-web-monitor-performance.md).

toouse New Relic finns [nya Relic prestanda programhantering på Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />

### <a name="2-collect-data"></a>2. Samla in data
hello Web Apps miljö innehåller diagnostikfunktion för att logga information från både hello webbservern och hello webbprogram. hello information är indelade i web serverdiagnostik- och programdiagnostik.

#### <a name="enable-web-server-diagnostics"></a>Aktivera web serverdiagnostik
Du kan aktivera eller inaktivera hello följande typer av loggar:

* **Detaljerad felloggning** -information om felet för HTTP-statuskoder som indikerar att en (statuskod 400 eller högre). Detta kan innehålla information som kan hjälpa dig att avgöra varför hello servern returnerade felkoden hello.
* **Kunde inte begäran spårning** -detaljerad information om misslyckade förfrågningar, inklusive en spårning av hello IIS-komponenter som används tooprocess hello begäran och hello tid i varje komponent. Detta kan vara användbart om du försöker tooimprove web app prestanda eller isolera vad som orsakar ett specifikt HTTP-fel.
* **Web Server-loggning** -Information om HTTP-transaktioner med hello W3C utökat loggfilsformat. Detta är användbart när du fastställer övergripande web app mätvärden, till exempel hello antal begäranden som hanteras eller hur många förfrågningar som kommer från en specifik IP-adress.

#### <a name="enable-application-diagnostics"></a>Aktivera programdiagnostik
Det finns flera alternativ toocollect programprestationsinformation från Web Apps, profil live programmet från Visual Studio eller ändra ditt program kod toolog mer information och spår. Du kan välja hello alternativ baserat på hur mycket du har toohello program och vad du har sett från hello övervakningsverktyg.

##### <a name="use-application-insights-profiler"></a>Använda Application Insights Profiler
Du kan aktivera hello Application Insights Profiler toostart fånga in spårningar detaljerad prestanda. Du kan komma åt spårningar fångas upp toofive dagar sedan när du behöver tooinvestigate problem har inträffat i hello tidigare. Du kan välja det här alternativet så länge som du har åtkomst toohello webbprogram Application Insights-resurs på Azure-portalen.

Application Insights Profiler innehåller statistik om svarstiden för varje webbtjänst-anrop och spår som visar vilka kodrad orsakas hello långsamt svar. Ibland hello App Service-appen är långsam eftersom vissa kod inte skrivs i en performant sätt. Exempel är sekventiella kod som kan köras i parallella och oönskat databasen Lås contentions. Om du tar bort dessa flaskhalsar i hello kod ökar hello appens prestanda, men de är hårda toodetect utan att ställa in avancerade spårningar och loggar. hello spårningssessioner samlas in av Application Insights Profiler kan identifiera hello rader med kod som långsammare hello program och lösa denna utmaning för Apptjänst-appar.

 Mer information finns i [Profiling live Azure-webbappar med Application Insights](../application-insights/app-insights-profiler.md).

##### <a name="use-remote-profiling"></a>Använd Remote profilering
I Azure App Service Web Apps API Apps och WebJobs kan vara från en fjärrdator i listan. Välj det här alternativet om du har åtkomst toohello webbresurs app och du vet hur tooreproduce hello problemet eller om du vet hello exakt tidsintervall hello prestandaproblemet händer.

Fjärråtkomst Profiling är användbart om hello CPU-användningen för processen hello är hög och processen körs långsammare än förväntat eller hello svarstiden för HTTP-begäranden är högre än normal, du kan via fjärranslutning profilen processen och få hello CPU provtagning anropet stackar tooanalyze Hej processaktiviteten och code varm sökvägar.

Mer information finns i [Remote profilering stöd i Azure App Service](https://azure.microsoft.com/blog/remote-profiling-support-in-azure-app-service).

##### <a name="set-up-diagnostic-traces-manually"></a>Ställ in diagnostikspår manuellt
Om du har åtkomst toohello web programmets källkod kan programdiagnostik toocapture information som produceras av ett webbprogram. ASP.NET-program kan använda hello `System.Diagnostics.Trace` klassen toolog information toohello programdiagnostik loggar. Du måste dock toochange hello koden och distribuera ditt program. Den här metoden rekommenderas om din app körs på en testmiljö.

Detaljerade anvisningar om hur tooconfigure ditt program för loggning, se [aktivera diagnostikloggning för web apps i Azure App Service](web-sites-enable-diagnostic-log.md).

#### <a name="use-hello-azure-app-service-support-portal"></a>Använda portalen för hello Azure App Service-stöd
Web Apps ger hello möjlighet tootroubleshoot problem relaterade tooyour webbprogram genom att titta på http-loggar, händelseloggar, process Dumpar och mycket mer. Du kan komma åt den här informationen med hjälp av vår portal Support vid **http://&lt;appens namn >.scm.azurewebsites.net/Support**

hello Azure App Service stöd portal innehåller tre separata flikar toosupport hello tre steg i ett vanligt scenario för felsökning:

1. Se aktuella beteende
2. Analysera genom att samla in diagnostikinformation och köra hello inbyggda analyzers
3. Minimera

Om hello problemet sker just nu, klickar du på **analysera** > **diagnostik** > **diagnostisera nu** toocreate en diagnostiska session, som samlar in http-loggar, loggar i Loggboken, minnesdumpar, PHP felloggarna och PHP-processen rapporten.

När hello data samlas hello stöd portalen körs en analys på hello data och ger dig en HTML-rapport.

Om du vill toodownload hello data, som standard lagras den i hello D:\home\data\DaaS mapp.

Mer information om hello Azure App Service stöd portal finns [nya uppdateringar tooSupport plats tillägget för Azure Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-hello-kudu-debug-console"></a>Använd hello Kudu-Felsökningskonsolen
Web Apps levereras med en Felsökningskonsolen som du kan använda för felsökning, utforska, ladda upp filer, samt JSON-slutpunkter för att hämta information om din miljö. Den här konsolen kallas hello *Kudu konsolen* eller hello *SCM instrumentpanelen* för ditt webbprogram.

Du kan komma åt den här instrumentpanelen genom att gå toohello länk **https://&lt;appens namn >.scm.azurewebsites.net/**.

Några av hello saker som Kudu tillhandahåller är:

* inställningar för ditt program
* loggström
* diagnostiska dump
* Felsöka konsolen där du kan köra Powershell-cmdlets och grundläggande DOS-kommandon.

En annan användbar funktion i Kudu är att, om ditt program är att första chans undantag, kan du använda Kudu och hello SysInternals verktyget Procdump toocreate minnesdumpar. Dessa minnesdumpar är ögonblicksbilder av hello processen och ofta kan hjälpa dig att felsöka mer komplicerade problem med ditt webbprogram.

Mer information om funktionerna i Kudu finns [Azure webbplatser Team Services-verktyg som du bör känna till om](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-hello-issue"></a>3. Minimera hello problemet
#### <a name="scale-hello-web-app"></a>Skala hello-webbprogram
I Azure App Service kan för bättre prestanda och genomflöde, du justera hello skala där du kör programmet. Skala upp ett webbprogram omfattar två relaterade åtgärder: ändra din App Service-plan tooa högre prisnivån och konfigurera vissa inställningar när du har växlat toohello högre prisnivå.

Läs mer om att skala [skala en webbapp i Azure App Service](web-sites-scale.md).

Du kan dessutom välja toorun programmet på mer än en instans. Skala ut inte bara ger mer bearbetningskapacitet, men ger dig även vissa delar av feltolerans. Om hello processen kraschar på en instans fortsätta hello andra instanser tooserve begäranden.

Du kan ange hello skalning toobe manuell eller automatisk.

#### <a name="use-autoheal"></a>Använd säkert att AutoHeal
Säkert att AutoHeal återanvänds hello arbetsprocessen för din app baserat på dina inställningar (t.ex. konfigurationsändringar begäranden, minnesbaserade gränser eller hello tid behövs tooexecute en begäran). För hello mesta är återvinning hello processen hello snabbaste sättet toorecover från ett problem. Även om du kan alltid starta om hello webbprogrammet från direkt i hello Azure-portalen, fungerar säkert att AutoHeal automatiskt åt dig. Allt du behöver toodo är att lägga till vissa utlösare i hello rotens web.config för ditt webbprogram. De här inställningarna fungerar i hello samma sätt även om programmet inte är en .net-app.

Mer information finns i [automatisk återställning Azure webbplatser](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-hello-web-app"></a>Starta om hello-webbprogram
Starta om är ofta hello enklaste sättet toorecover engångsproblem. På hello [Azure-portalen](https://portal.azure.com/), på bladet för ditt webbprogram har hello alternativ toostop eller starta om din app.

 ![Starta om web app toosolve prestandaproblem](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Du kan också hantera ditt webbprogram med hjälp av Azure Powershell. Mer information finns i [Använda Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).
