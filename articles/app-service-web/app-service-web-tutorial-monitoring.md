---
title: aaaMonitor en Webbapp | Microsoft Docs
description: "Lär dig hur tooset in övervakning på ditt webbprogram"
services: App-Service
keywords: 
author: btardif
ms.author: byvinyal
ms.date: 04/04/2017
ms.topic: article
ms.service: app-service-web
ms.openlocfilehash: c2f5e9842c732a804f1caee5d67e53dad24e190a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-app-service"></a>Övervaka App Service
Den här självstudiekursen vägleder dig genom att övervaka din app och använda hello inbyggda plattform verktyg toosolve problem när de inträffar.

Varje avsnitt i det här dokumentet går över en specifik funktion. Använda hello-funktioner tillsammans kan du:
- Identifierar ett problem i din app.
- Avgöra när hello problemet orsakas av din kod eller hello plattform.
- Begränsa hello källan hello problemet i koden.
- Felsökning och åtgärda hello problem.

## <a name="before-you-begin"></a>Innan du börjar
- Du behöver en Web App toomonitor och följ hello beskrivs stegen.
    - Du kan skapa ett program efter hello stegen som beskrivs i hello [skapa en ASP.NET-app i Azure med SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) kursen.

- Om du vill tootry **fjärrfelsökning** för din app behöver du Visual Studio.
    - Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda hello ledigt [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).
    - Kontrollera att du aktiverar **Azure-utveckling** under installationen av hello Visual Studio.

## <a name="metrics"></a>Steg 1 – Visa mått
**Mått** är användbara toounderstand:
- Programmets hälsotillstånd
- App-prestanda
- Resursförbrukning

När du undersöker ett problem med programmet, är granska mått ett bra toostart. Azure-portalen har ett snabbt sätt toovisually inspektera hello mätvärden för din app med hjälp av **Azure-Monitor**.

Mått ger en historiköversikt över flera viktiga aggregeringar för din app. För alla appar som finns i app service, bör du övervaka både hello Web App och hello App Service-plan.

> [!NOTE]
> En apptjänstplan representerar hello mängd fysiska resurser som används toohost dina appar. Alla program som tilldelats tooan App Service-plan hello resurser definieras av den så att du toosave kostnad när värd för flera appar.
>
> App Service-planer definierar följande:
> * Region: Nordeuropa, östra USA, Sydostasien, osv.
> * Instansstorleken: Liten, Medium, Large och så vidare.
> * Skalningsantalet: en, två eller tre instanser, etc.
> * SKU: Ledigt delade, Basic, Standard, Premium, osv.

tooreview mätvärden för ditt webbprogram, gå toohello **översikt** bladet hello app som du vill toomonitor. Härifrån kan du visa ett diagram för din app mått som en **övervakning panelen**. Klicka på hello panelen tooedit och konfigurera vilka mått tooview och hello tid intervallet toodisplay.

Ger en vy för hello programförfrågningar och fel för hello senaste timmen som standard hello-resursbladet.
![Övervaka App](media/app-service-web-tutorial-monitoring/app-service-monitor.png)

Som du ser i exemplet hello vi har ett program som genererar många **HTTP-fel**. hello hög volym av fel är hello första indikation tooinvestigate måste det här programmet.

> [!TIP]
> Lär dig mer om Azure-Monitor med hello följande länkar:
> - [Kom igång med Azure-Monitor](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Azure mått](..\monitoring-and-diagnostics\monitoring-overview-metrics.md)
> - [Stöds mått med Azure-Monitor](..\monitoring-and-diagnostics\monitoring-supported-metrics.md)
> - [Azure instrumentpaneler](..\azure-portal\azure-portal-dashboards.md)

## <a name="alerts"></a>Steg 2 – konfigurera aviseringar
**Aviseringar** kan vara konfigurerade tootrigger på särskilda villkor för din app.

I [steg 1 – Visa mått](#metrics), vi såg att programmet hello hade ett stort antal fel.

Kan du konfigurera en varning tooautomatically få meddelanden när ett fel uppstår. I det här fallet vi vill hello avisering toosend och e-post varje gång hello antal HTTP-50 X fel färdas över ett visst tröskelvärde.

toocreate en avisering, navigera för**övervakning** > **aviseringar** och på **[+] Lägg till avisering**.

![Aviseringar](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts.png)

Ange värden för hello varningskonfigurationen:
- **Resurs:** hello plats toomonitor med hello avisering.
- **Namn:** ett namn för aviseringen i det här fallet: *hög HTTP 50 X*.
- **Beskrivning:** oformaterad text förklaring av aviseringen är ute på.
- **Varning vid:** aviseringar kan titta på mått eller händelser, för det här exemplet vi tittar på mått.
- **Mått:** vilka mått toomonitor i det här fallet: *HTTP-fel*.
- **Villkor:** när tooalert, i det här fallet väljer hello *större än* alternativet.
- **Tröskelvärde:** vad är värdet toolook för, i det här fallet: *400*.
- **Period:** aviseringar fungerar över hello medelvärdet för ett mått. Mindre tidsperioder ge känsligare aviseringar. i det här fallet vi tittar på *5 minuter*.
- **E-ägare och deltagare:** i det här fallet: *aktiverad*.

Nu när hello avisering skapas ett e-postmeddelande skickas varje gång hello appen går över hello konfigurerade tröskelvärdet. Aktiva aviseringar kan också ses i hello Azure-portalen.

![Utlösta varningar](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts-triggered.png)


> [!TIP]
> Läs mer om Azure aviseringar med hello följande länkar:
> - [Vad är aviseringar i Microsoft Azure](..\monitoring-and-diagnostics\monitoring-overview-alerts.md)
> - [Utför en åtgärd på mått](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Skapa mått aviseringar](..\monitoring-and-diagnostics\insights-alerts-portal.md)

## <a name="companion"></a>Steg 3 – Apptjänst tillhörande
**Apptjänst tillhörande** erbjuder ett enkelt sätt toomonitor din app med en inbyggd upplevelse i din mobila enhet (iOS eller Android).

Använd medföljande App Service till:
- Granska program-mått
- Granska och reagera på programaviseringar och rekommendationer
- Grundläggande felsökning (Bläddra, starta, stoppa, omstart hello app)
- Hämta push-meddelanden för kritiska händelser.

![Medföljande App Service](media/app-service-web-tutorial-monitoring/app-service-companion.png)

[![App Service medföljande App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![Apptjänst medföljande Google Play](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)

Du kan installera Apptjänst tillhörande från hello [Appbutiken](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) eller [Google Play](https://play.google.com/store/apps/details?id=azureApps.AzureApps)

## <a name="diagnose"></a>Steg 4 – diagnostisera och lösa problem
**Diagnostisera och lösa problem** hjälper du separera programproblem formuläret plattform problem. Det kan också föreslå möjliga åtgärder tooget din tillbaka toohealthy Web App.

![Diagnostisera och lösa problem](media/app-service-web-tutorial-monitoring/app-service-monitor-diagnosis.png)

Fortsätter med hello exempel formuläret föregående steg, se vi att programmet hello har med availably problem. Hello plattform tillgänglighet har däremot inte flyttats från 100%.

När hello app har problemet och hello plattform förblir upp, är det en tydlig indikation på att vi arbetar med ett problem med programmet.

## <a name="logging"></a>Steg 5 - loggning
Nu när vi har minskar antalet problem med programmet hello fel tooan, kan vi titta på hello program- och loggar tooget mer information.

Loggning kan du toocollect båda **Programdiagnostik** och **Web Serverdiagnostik** loggar för Webbappen.

### <a name="application-diagnostics"></a>Application Diagnostics
Programdiagnostik kan du toocapture spårningar produceras av programmet hello vid körning.

Lägga till spårning tooyour förbättrar programmet avsevärt din möjlighet toodebug och pekpunkt problem.

I ASP.NET, du kan logga in programspårningar med [System.Diagnostics.Trace klassen](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) toogenerate händelser som samlas in av hello loggen infrastruktur. Du kan också ange hello allvarlighetsgraden hello spårning för enklare filtrering.

```csharp
public ActionResult Delete(Guid? id)
{
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id);
    if (id == null)
    {
        System.Diagnostics.Trace.TraceError("/Todos/Delete/ failed, ID is null");
        return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
    }
    Todo todo = db.Todoes.Find(id);
    if (todo == null)
    {
        System.Diagnostics.Trace.TraceWarning("/Todos/Delete/ failed, ID: " + id + " could not be found");
        return HttpNotFound();
    }
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id + "completed successfully");
    return View(todo);
}
```
tooenable programloggning gå för**övervakning** > **diagnostikloggar** och aktivera programloggning med hello växlar.

![Övervaka App](media/app-service-web-tutorial-monitoring/app-service-monitor-applogs.png)

Programloggarna kan vara lagrade tooyour Web App filsystem eller push-installeras tooblob lagring. Scenarier för produktion är det rekommenderade toouse blob-lagring.

> [!IMPORTANT]
> Om du aktiverar loggning påverkar dina program prestanda- och användning. För produktion scenarier rekommenderas felloggar. Aktivera endast mer utförlig loggning när du undersöker problem.

 ### <a name="web-server-diagnostics"></a>Web Serverdiagnostik
Webbserverloggarna genereras även om din app inte instrumenterats. Apptjänst kan samla in tre olika typer av server-loggar:

- **Web Server-loggning**
    - Information om HTTP-transaktioner med hello [W3C utökat loggfilsformat](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
    - Användbart när du fastställer övergripande plats mätvärden, till exempel hello antal begäranden som hanteras eller hur många förfrågningar som kommer från en specifik IP-adress.
- **Detaljerad felloggning**
    - Detaljerad information om fel för HTTP-statuskoder som indikerar att en (statuskod 400 eller högre).
    - [Mer information om detaljerad felloggning](https://www.iis.net/learn/troubleshoot/diagnosing-http-errors/how-to-use-http-detailed-errors-in-iis)
- **Spårning av misslyckade begäranden**
    - Detaljerad information om misslyckade förfrågningar, inklusive en spårning av hello IIS-komponenter används tooprocess hello begäran och hello tid i varje komponent.
    - Loggar för misslyckade begäranden är användbara vid tooisolate vad som orsakar ett specifikt HTTP-fel.
    - [Mer information om spårning av misslyckade begäranden](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)

tooenable Server-loggning:
- Gå för**övervakning** > **diagnostikloggar**.
- Aktivera hello olika typer av Web Serverdiagnostik med hello växlar.

![Övervaka App](media/app-service-web-tutorial-monitoring/app-service-monitor-serverlogs.png)

> [!IMPORTANT]
> Om du aktiverar loggning påverkar dina program prestanda- och användning. För produktion scenarier rekommenderas felloggar, aktivera endast mer utförlig loggning när du undersöker problem.

### <a name="accessing-logs"></a>Åtkomst till loggar
Loggfilerna lagras i blob storage kan nås med hjälp av Azure Lagringsutforskaren. Loggar som finns i hello Web App filsystem kan nås via FTP under hello följande sökvägar:

- **Programloggarna** - `%HOME%/LogFiles/Application/`.
    - Den här mappen innehåller en eller flera textfiler som innehåller information som genereras av programloggning.
- **Det gick inte begäranden** - `%HOME%/LogFiles/W3SVC#########/`.
    - Den här mappen innehåller en XSL-fil och en eller flera XML-filer.
- **Detaljerade felloggar** - `%HOME%/LogFiles/DetailedErrors/`.
    - Den här mappen innehåller en eller flera .htm-filer med omfattande information om HTTP-fel som genereras av din app.
- **Web Server-loggar** - `%HOME%/LogFiles/http/RawLogs`.
    - Den här mappen innehåller en eller flera textfiler som formaterats med hello W3C utökat loggfilsformat.

## <a name="streaming"></a>Steg 6 - loggen strömning
Direktuppspelningsloggar är praktiskt när du felsöker ett program eftersom det sparar tid jämfört med för[åtkomst till hello loggar](#Accessing-Logs) via FTP.

Apptjänst kan strömma **programloggarna** och **Webbserverloggar** när de skapas.

> [!TIP]
> Innan du försöker toostream loggar, kontrollera att du har aktiverat att samla in loggar enligt beskrivningen i hello [loggning](#logging) avsnitt.

toostream loggar gå för**övervakning**> **loggström**. Välj **programloggarna** eller **Web server-loggar** beroende på vilken information du söker efter. Här kan du även pausa, starta om och rensa hello buffert.

![Direktuppspelningsloggar](media/app-service-web-tutorial-monitoring/app-service-monitor-logstream.png)

> [!TIP]
> Loggar genereras bara när det trafik på hello app, öka hello detaljnivå av loggar tooget fler händelser eller information.

## <a name="remote"></a>Steg 7 – fjärrfelsökning
När du har PIN-kod pekar hello källan för problem med hello program använder **fjärrfelsökning** toowalk via hello kod.

Felsökning kan du bifoga en felsökare tooyour Web App körs i hello moln. Du kan ange brytpunkter, ändra minne direkt, stega igenom koden och även ändra hello kodsökvägen precis som för en app som körs lokalt.

tooattach hello felsökare tooyour appen körs i hello moln:

- Med hjälp av Visual Studio 2017, öppna hello lösning för hello app vill du toodebug
- Ange vissa bromsar punkter precis som du skulle för lokal utveckling.
- Öppna **cloud explorer** (ctr + / -ctrl + x).
- Logga in med dina autentiseringsuppgifter för azure vid behov.
- Hitta hello app som du vill toodebug
- Välj **koppla felsökare** formuläret hello **åtgärder** fönstret.

![Fjärrfelsökning](media/app-service-web-tutorial-monitoring/app-service-monitor-vsdebug.png)

Visual Studio konfigurerar ditt program för fjärrfelsökning och öppnar ett webbläsarfönster som navigerar tooyour app. Bläddra igenom din app tootrigger break punkter och gå igenom hello kod.

> [!WARNING]
> Du bör inte köra i felsökningsläge i produktion. Om din produktionsprogrammet inte skalas ut toomultiple serverinstanser felsökning hindra hello-webbservern från svarar tooother begäranden. För att felsöka problem i produktionen bästa resursen är för[konfigurera loggning](#logging) och [Programinsikter](#insights).



## <a name="explorer"></a>Steg 8 – Processutforskaren
När programmet skalas ut toomore än en instans **processutforskaren** kan hjälpa dig att identifiera instans specifika problem.

Använd **bearbeta Explorer** till:

- Räkna upp alla hello processer mellan olika instanser av din programtjänstplan.
- Detaljnivån och visa hello referenser och moduler som är associerade med varje process.
- Visa CPU, aktiv sidmängd och tråd antal vid hello bearbeta nivå toohelp du identifiera skenande processer
- Hitta öppna filreferenser och även avsluta en specifik process-instans.

Processutforskaren finns under **övervakning** > **Processutforskaren**.

![Processutforskaren](media/app-service-web-tutorial-monitoring/app-service-monitor-processexplorer.png)


## <a name="insights"></a>Steg 9 – Application Insights
**Application Insights** ger profileringsspårning för program och funktioner för avancerad övervakning för din app.

Använd Application Insights toodetect och diagnostisera undantag och prestandaproblem i ditt webbprogram.

Du kan aktivera Application Insights för ditt webbprogram under **övervakning** > **Application Insights**

> [!NOTE]
> Application Insights kan medföra att du tooinstall hello Application Insights plats tillägget toostart insamling av data. Installation hello webbplatstillägg gör en omstart.

![Application Insights](media/app-service-web-tutorial-monitoring/app-service-monitor-appinsights.png)

Application Insights har kraftfulla funktioner kan toolearn fler följa hello länkar som ingår i hello [nästa steg](#next) avsnitt.

## <a name="next"></a>Nästa steg

 - [Vad är Application Insights](..\application-insights\app-insights-overview.md)
 - [Övervaka Azure web app prestanda med Application Insights](..\application-insights\app-insights-azure-web-apps.md)
 - [Övervaka tillgänglighet och svarstider för alla webbplatser med Application Insights](..\application-insights\app-insights-monitor-web-app-availability.md)
