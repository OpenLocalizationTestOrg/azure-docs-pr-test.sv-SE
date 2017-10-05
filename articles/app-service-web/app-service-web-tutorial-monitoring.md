---
title: "Övervaka ett webbprogram | Microsoft Docs"
description: "Lär dig hur du ställer in övervakning på ditt webbprogram"
services: App-Service
keywords: 
author: btardif
ms.author: byvinyal
ms.date: 04/04/2017
ms.topic: article
ms.service: app-service-web
ms.openlocfilehash: 29df824062d00e01b786533033097948c008588f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-app-service"></a>Övervaka App Service
Den här självstudiekursen vägleder dig genom att övervaka din app och använda de inbyggda plattformsverktyg för att lösa problem när de inträffar.

Varje avsnitt i det här dokumentet går över en specifik funktion. Tillsammans med funktionerna kan du:
- Identifierar ett problem i din app.
- Avgör om problemet beror på din kod eller plattform.
- Begränsa källan till problemet i din kod.
- Felsökning och åtgärda problemet.

## <a name="before-you-begin"></a>Innan du börjar
- Du behöver ett webbprogram för att övervaka och följ stegen nedan.
    - Du kan skapa ett program följer du stegen som beskrivs i den [skapa en ASP.NET-app i Azure med SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) kursen.

- Om du vill testa **fjärrfelsökning** för din app behöver du Visual Studio.
    - Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda den kostnadsfria [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).
    - Se till att du aktiverar **Azure-utveckling** under installationen av Visual Studio.

## <a name="metrics"></a>Steg 1 – Visa mått
**Mått** är användbar för att förstå:
- Programmets hälsotillstånd
- App-prestanda
- Resursförbrukning

När du undersöker ett problem med programmet, är granska mått ett bra ställe att börja. Azure-portalen har ett snabbt sätt att visuellt inspektera mätvärden för din app med hjälp av **Azure-Monitor**.

Mått ger en historiköversikt över flera viktiga aggregeringar för din app. För alla appar som finns i app service, bör du övervaka både Web App och programtjänstplanen.

> [!NOTE]
> En App Service-plan representerar den samling fysiska resurser som dina appar finns i. Alla program som har tilldelats en App Service-plan delar de resurser som definierats av planen. Det innebär att du kan minska kostnaderna när du har flera appar i en plan.
>
> App Service-planer definierar följande:
> * Region: Nordeuropa, östra USA, Sydostasien, osv.
> * Instansstorleken: Liten, Medium, Large och så vidare.
> * Skalningsantalet: en, två eller tre instanser, etc.
> * SKU: Ledigt delade, Basic, Standard, Premium, osv.

Om du vill granska mått för ditt webbprogram, gå till den **översikt** bladet för den app du vill övervaka. Härifrån kan du visa ett diagram för din app mått som en **övervakning panelen**. Klicka på panelen om du vill redigera och konfigurera vilka mått att visa och tidsintervallet för att visa.

Som standard innehåller resursbladet en för programmet begäranden och fel för den senaste timmen.
![Övervaka App](media/app-service-web-tutorial-monitoring/app-service-monitor.png)

Som du ser i exemplet har vi ett program som genererar många **HTTP-fel**. Hög volym av fel är den första uppgift som vi behöver undersöka det här programmet.

> [!TIP]
> Lär dig mer om Azure-Monitor med följande länkar:
> - [Kom igång med Azure-Monitor](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Azure mått](..\monitoring-and-diagnostics\monitoring-overview-metrics.md)
> - [Stöds mått med Azure-Monitor](..\monitoring-and-diagnostics\monitoring-supported-metrics.md)
> - [Azure instrumentpaneler](..\azure-portal\azure-portal-dashboards.md)

## <a name="alerts"></a>Steg 2 – konfigurera aviseringar
**Aviseringar** kan konfigureras att utlösas vid särskilda villkor för din app.

I [steg 1 – Visa mått](#metrics), vi såg att programmet har ett stort antal fel.

Kan konfigurera en avisering för att automatiskt få meddelanden när ett fel uppstår. I det här fallet vill vi att aviseringen ska skicka och e-post varje gång antalet HTTP-50 X fel färdas över ett visst tröskelvärde.

Om du vill skapa en avisering, gå till **övervakning** > **aviseringar** och på **[+] Lägg till avisering**.

![Aviseringar](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts.png)

Ange värden för aviseringskonfigurationen:
- **Resurs:** platsen att övervaka med aviseringen.
- **Namn:** ett namn för aviseringen i det här fallet: *hög HTTP 50 X*.
- **Beskrivning:** oformaterad text förklaring av aviseringen är ute på.
- **Varning vid:** aviseringar kan titta på mått eller händelser, för det här exemplet vi tittar på mått.
- **Mått:** vilka mått som ska övervakas, i det här fallet: *HTTP-fel*.
- **Villkor:** när aviseringen i det här fallet väljer den *större än* alternativet.
- **Tröskelvärde:** vad är värdet att söka efter, i det här fallet: *400*.
- **Period:** aviseringar fungerar över medelvärdet för ett mått. Mindre tidsperioder ge känsligare aviseringar. i det här fallet vi tittar på *5 minuter*.
- **E-ägare och deltagare:** i det här fallet: *aktiverad*.

Nu när aviseringen har skapats skickas ett e-postmeddelande varje gång appen går över den konfigurerade tröskeln. Aktiva aviseringar kan också ses i Azure-portalen.

![Utlösta varningar](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts-triggered.png)


> [!TIP]
> Läs mer om Azure aviseringar med följande länkar:
> - [Vad är aviseringar i Microsoft Azure](..\monitoring-and-diagnostics\monitoring-overview-alerts.md)
> - [Utför en åtgärd på mått](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Skapa mått aviseringar](..\monitoring-and-diagnostics\insights-alerts-portal.md)

## <a name="companion"></a>Steg 3 – Apptjänst tillhörande
**Apptjänst tillhörande** erbjuder ett enkelt sätt att övervaka din app med en inbyggd upplevelse i din mobila enhet (iOS eller Android).

Använd medföljande App Service till:
- Granska program-mått
- Granska och reagera på programaviseringar och rekommendationer
- Grundläggande felsökning (Bläddra, starta, stoppa, starta om appen)
- Hämta push-meddelanden för kritiska händelser.

![Medföljande App Service](media/app-service-web-tutorial-monitoring/app-service-companion.png)

[![App Service medföljande App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![Apptjänst medföljande Google Play](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)

Du kan installera Apptjänst tillhörande från den [Appbutiken](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) eller [Google Play](https://play.google.com/store/apps/details?id=azureApps.AzureApps)

## <a name="diagnose"></a>Steg 4 – diagnostisera och lösa problem
**Diagnostisera och lösa problem** hjälper du separera programproblem formuläret plattform problem. Det kan också föreslå möjliga åtgärder för att få ditt webbprogram till felfritt.

![Diagnostisera och lösa problem](media/app-service-web-tutorial-monitoring/app-service-monitor-diagnosis.png)

Fortsätter med de föregående stegen exempel formuläret se vi att programmet har tagits med availably problem. Tillgängliga plattformar har däremot inte flyttats från 100%.

När appen är med problemet och plattform förblir upp, är det en tydlig indikation på att vi arbetar med ett problem med programmet.

## <a name="logging"></a>Steg 5 - loggning
Nu när vi har minskar antalet fel på ett problem med programmet, titta vi på program- och loggfiler för mer information.

Loggning kan du samla in både **Programdiagnostik** och **Web Serverdiagnostik** loggar för Webbappen.

### <a name="application-diagnostics"></a>Application Diagnostics
Programdiagnostik kan du fånga in spårningar som genereras av programmet vid körning.

Lägga till spårning i tillämpningsprogrammet avsevärt förbättrar möjligheten att felsöka och pekpunkt problem.

I ASP.NET, du kan logga in programspårningar med [System.Diagnostics.Trace klassen](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) att generera händelser som samlas in av logg-infrastruktur. Du kan också ange allvarlighetsgraden spårningen för enklare filtrering.

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
Om du vill aktivera programloggning går du till **övervakning** > **diagnostikloggar** och aktivera programloggning med alternativen.

![Övervaka App](media/app-service-web-tutorial-monitoring/app-service-monitor-applogs.png)

Programloggarna kan lagras i filsystemet för ditt webbprogram eller skickas till blob storage. Scenarier för produktion, har vi rekommenderar för att använda blob storage.

> [!IMPORTANT]
> Om du aktiverar loggning påverkar dina program prestanda- och användning. För produktion scenarier rekommenderas felloggar. Aktivera endast mer utförlig loggning när du undersöker problem.

 ### <a name="web-server-diagnostics"></a>Web Serverdiagnostik
Webbserverloggarna genereras även om din app inte instrumenterats. Apptjänst kan samla in tre olika typer av server-loggar:

- **Web Server-loggning**
    - Information om HTTP-transaktioner med hjälp av den [W3C utökat loggfilsformat](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
    - Användbart när du fastställer övergripande plats mätvärden, till exempel antalet förfrågningar som hanteras eller hur många förfrågningar som kommer från en specifik IP-adress.
- **Detaljerad felloggning**
    - Detaljerad information om fel för HTTP-statuskoder som indikerar att en (statuskod 400 eller högre).
    - [Mer information om detaljerad felloggning](https://www.iis.net/learn/troubleshoot/diagnosing-http-errors/how-to-use-http-detailed-errors-in-iis)
- **Spårning av misslyckade begäranden**
    - Detaljerad information om misslyckade förfrågningar, inklusive en spårning av IIS-komponenter som används för att bearbeta begäran och tidsåtgång i varje komponent.
    - Misslyckade begäranden loggar är användbart när du försöker isolera vad som orsakar ett specifikt HTTP-fel.
    - [Mer information om spårning av misslyckade begäranden](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)

Så här aktiverar du loggning Server:
- Gå till **övervakning** > **diagnostikloggar**.
- Aktivera de olika typerna av Web Serverdiagnostik med alternativen.

![Övervaka App](media/app-service-web-tutorial-monitoring/app-service-monitor-serverlogs.png)

> [!IMPORTANT]
> Om du aktiverar loggning påverkar dina program prestanda- och användning. För produktion scenarier rekommenderas felloggar, aktivera endast mer utförlig loggning när du undersöker problem.

### <a name="accessing-logs"></a>Åtkomst till loggar
Loggfilerna lagras i blob storage kan nås med hjälp av Azure Lagringsutforskaren. Loggar som finns i Webbappens filsystem kan nås via FTP under följande sökvägar:

- **Programloggarna** - `%HOME%/LogFiles/Application/`.
    - Den här mappen innehåller en eller flera textfiler som innehåller information som genereras av programloggning.
- **Det gick inte begäranden** - `%HOME%/LogFiles/W3SVC#########/`.
    - Den här mappen innehåller en XSL-fil och en eller flera XML-filer.
- **Detaljerade felloggar** - `%HOME%/LogFiles/DetailedErrors/`.
    - Den här mappen innehåller en eller flera .htm-filer med omfattande information om HTTP-fel som genereras av din app.
- **Web Server-loggar** - `%HOME%/LogFiles/http/RawLogs`.
    - Den här mappen innehåller en eller flera textfiler som formaterats med W3C utökat loggfilsformat.

## <a name="streaming"></a>Steg 6 - loggen strömning
Direktuppspelningsloggar är praktiskt när du felsöker ett program eftersom det sparar tid jämfört med [åtkomst till loggarna](#Accessing-Logs) via FTP.

Apptjänst kan strömma **programloggarna** och **Webbserverloggar** när de skapas.

> [!TIP]
> Innan du försöker att strömma loggar, se till att du har aktiverat att samla in loggar som beskrivs i den [loggning](#logging) avsnitt.

Gå till för att strömma loggar **övervakning**> **loggström**. Välj **programloggarna** eller **Web server-loggar** beroende på vilken information du söker efter. Härifrån kan du även pausa, starta om och rensa bufferten.

![Direktuppspelningsloggar](media/app-service-web-tutorial-monitoring/app-service-monitor-logstream.png)

> [!TIP]
> Loggar genereras bara när det finns någon trafik i appen, du kan också öka detaljnivå loggar få fler händelser eller information.

## <a name="remote"></a>Steg 7 – fjärrfelsökning
När du har pin-pekar källan för program-problem, använda **fjärrfelsökning** kan du gå igenom koden.

Felsökning kan du bifoga en felsökare till ditt webbprogram som körs i molnet. Du kan ange brytpunkter, ändra minne direkt, stega igenom koden och även ändra koden sökvägen precis som för en app som körs lokalt.

Att koppla felsökaren till din app som körs i molnet:

- Med Visual Studio 2017 kan du öppna lösning för den app du vill felsöka
- Ange vissa bromsar punkter precis som du skulle för lokal utveckling.
- Öppna **cloud explorer** (ctr + / -ctrl + x).
- Logga in med dina autentiseringsuppgifter för azure vid behov.
- Hitta den app som du vill felsöka
- Välj **koppla felsökare** formuläret i **åtgärder** fönstret.

![Fjärrfelsökning](media/app-service-web-tutorial-monitoring/app-service-monitor-vsdebug.png)

Visual Studio konfigurerar ditt program för fjärrfelsökning och öppnar ett webbläsarfönster som navigerar till din app. Bläddra igenom din app att utlösa break punkter och stega igenom koden.

> [!WARNING]
> Du bör inte köra i felsökningsläge i produktion. Om din produktionsprogrammet inte skalas ut att flera serverinstanser felsökning förhindra webbservern svarar på andra begäranden. För felsökning för produktion, bästa resursen är en [konfigurera loggning](#logging) och [Programinsikter](#insights).



## <a name="explorer"></a>Steg 8 – Processutforskaren
När programmet skalas ut till mer än en instans, **processutforskaren** kan hjälpa dig att identifiera instans specifika problem.

Använd **bearbeta Explorer** till:

- Räkna upp alla processer mellan olika instanser av din programtjänstplan.
- Detaljnivån och visa referenser och moduler som är associerade med varje process.
- Visa CPU, aktiv sidmängd och antal på processnivå som hjälper dig att identifiera skenande processer trådar
- Hitta öppna filreferenser och även avsluta en specifik process-instans.

Processutforskaren finns under **övervakning** > **Processutforskaren**.

![Processutforskaren](media/app-service-web-tutorial-monitoring/app-service-monitor-processexplorer.png)


## <a name="insights"></a>Steg 9 – Application Insights
**Application Insights** ger profileringsspårning för program och funktioner för avancerad övervakning för din app.

Använda Application Insights för att identifiera och diagnostisera undantag och prestandaproblem i ditt webbprogram.

Du kan aktivera Application Insights för ditt webbprogram under **övervakning** > **Application Insights**

> [!NOTE]
> Application Insights kan en uppmaning om att installera tillägg för Application Insights-plats för att börja samla in data. Installera tillägget plats gör att en omstart.

![Application Insights](media/app-service-web-tutorial-monitoring/app-service-monitor-appinsights.png)

Application Insights har en kraftfulla funktioner, om du vill veta mer Följ länkarna som ingår i den [nästa steg](#next) avsnitt.

## <a name="next"></a>Nästa steg

 - [Vad är Application Insights](..\application-insights\app-insights-overview.md)
 - [Övervaka Azure web app prestanda med Application Insights](..\application-insights\app-insights-azure-web-apps.md)
 - [Övervaka tillgänglighet och svarstider för alla webbplatser med Application Insights](..\application-insights\app-insights-monitor-web-app-availability.md)
