---
title: "aaaUsage analys för webbprogram med Azure Application Insights | Microsoft docs"
description: "Förstå dina användare och vad de gör för din webbapp."
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: bwren
ms.openlocfilehash: f7f9173cf411fa0d2dfb3b5ba99134a02bbc0e89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="usage-analysis-for-web-applications-with-application-insights"></a>Användningsanalys för webbprogram med Application Insights

Vilka funktioner av ditt webbprogram som är mest populär? Dina användare nå sina mål med din app? De släppa vid särskilda tidpunkter och de kom tillbaka senare?  [Azure Application Insights](app-insights-overview.md) hjälper dig att få kraftfulla insikter om hur personer använder ditt webbprogram. Varje gång du uppdaterar din app kan bedöma du hur bra fungerar för användare. Med denna kunskap kan du datadrivna beslut om din nästa utvecklingscykler.

## <a name="send-telemetry-from-your-app"></a>Skicka telemetri från din app

hello bästa möjliga upplevelse hämtas genom att installera Application Insights både i appen serverkoden och på webbsidor. hello klient- och serverkomponenter för din app skicka telemetri tillbaka toohello Azure-portalen för analys.

1. **Serverkod:** lämpliga Install hello-modulen för din [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), eller [andra](app-insights-platforms.md) app.

    * *Inte vill tooinstall serverkoden? Bara [skapa en Azure Application Insights-resurs](app-insights-create-new-resource.md).*

2. **Koden webbsidan:** öppna hello [Azure-portalen](https://portal.azure.com), öppna hello Application Insights-resurs för din app och öppna sedan **komma igång > övervaka och diagnostisera klientsidan**. 

    ![Kopiera hello skript till hello head av webbsidan master.](./media/app-insights-usage-overview/02-monitor-web-page.png)


3. **Hämta telemetri:** köra projektet i felsökningsläge i några minuter och Sök efter resulterar i hello översikt bladet i Application Insights.

    Publicera din app toomonitor appens prestanda och ta reda på vad användarna gör med din app.

## <a name="include-user-and-session-id-in-your-telemetry"></a>Användar- och session-ID inkluderas i din telemetri
tootrack användare över tid, Programinsikter kräver en sätt tooidentify dem. hello händelser verktyget hello endast användning verktyg som inte kräver ett användar-ID eller ett sessions-ID.

Börja skicka dessa ID: N [här](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).

## <a name="explore-usage-demographics-and-statistics"></a>Utforska användning demografi och statistik
Ta reda om personer använder din app, vilka sidor som de är mest intresserad av, där användarna finns, vilka webbläsare och operativsystem som de använder. 

hello användare och sessioner rapporter filtrera dina data efter sidor eller anpassade händelser och segmentera dem efter egenskaper som plats, miljö och sida. Du kan också lägga till egna filter.

![Användare](./media/app-insights-usage-overview/users.png)  

Om rätt hello peka ut intressanta mönster i hello uppsättning data.  

* Hej **användare** rapporten räknar hello antal unika användare som har åtkomst till dina sidor i din valda tidsperioder. (Användare ska räknas genom att använda cookies. Om någon har åtkomst till din plats med olika webbläsare eller klientdatorer eller rensar sina cookies, och sedan ska räknas mer än en gång.)
* Hej **sessioner** rapporten är hello antalet användarsessioner som har åtkomst till webbplatsen. En session är en period av aktivitet av en användare eller avslutas med en period av inaktivitet av mer än en halvtimme.

[Mer information om verktyg för hello användare, sessioner och händelser](app-insights-usage-segmentation.md)  

## <a name="page-views"></a>Sidvisningar

Från hello användning bladet, klickar du på via hello sidvisningar panelen tooget en detaljerad analys av dina mest populära sidor:

![Hello översikt bladet, klickar du på hello sidan vyerna diagram](./media/app-insights-usage-overview/05-games.png)

hello-exemplet ovan är från en webbplats för spel. Från hello diagram ser omedelbart vi:

* Användning har förbättrats i hello gångna veckan. Kanske fundera över sökmotoroptimering?
* Tennis är hello populäraste spel sida. Nu ska vi fokusera på ytterligare förbättringar toothis sidan.
* I genomsnitt besöka användare hello Tennis cirka tre gånger per vecka. (Det finns ungefär tre gånger så mycket sessioner än användare.)
* De flesta användare besöka hello under hello arbetsvecka för USA och i arbetstid. Vi bör kanske ge knappen ”snabb Dölj” på hello webbsida.
* Hej [anteckningar](app-insights-annotations.md) visar hello diagrammet när nya versioner av hello webbplats har distribuerats. Ingen av hello senaste distributioner hade en märkbar effekt på användning.

Vad händer om du vill tooinvestigate hello trafik tooyour platsen i detalj som delas av en anpassad egenskap som ska skickas på webbplatsen i dess sidan Visa telemetri?

1. Öppna hello **händelser** verktyg i menyn för hello Application Insights-resurs. Det här verktyget kan du analysera hur många sidvisningar och anpassade händelser skickades från din app, baserat på en mängd olika alternativ för filtrering, cohorting och segmentering.
2. I hello ”som används för” listrutan, väljer du ”alla sidvisningen”.
3. Välj en egenskap genom vilka toosplit sidan Visa telemetri i hello ”delning av” listrutan.

## <a name="retention---how-many-users-come-back"></a>Innehållen – hur många användare försöka igen?

Kvarhållning hjälper dig att förstå hur ofta returnera användarna toouse appen, baserat på kohorter av användare som utförde vissa företag åtgärden under en viss tidsperiod. 

- Förstå vilka specifika funktioner orsaka användare toocome tillbaka mer än andra 
- Formuläret hypoteser baserat på verkliga användardata 
- Avgöra om kvarhållning är ett problem i produkten 

![Kvarhållning](./media/app-insights-usage-overview/retention.png) 

hello kvarhållning kontroller överst kan du toodefine specifika händelser och tid intervallet toocalculate lagring. hello diagrammet hello mitten ger en bild av hello övergripande kvarhållning procentandel av hello tidsintervall har angetts. hello diagram på hello nedre representerar enskilda kvarhållning i en viss tidsperiod. Den här detaljnivån kan du toounderstand vad användarna gör och vad kan påverka returnerar användare på en mer detaljerad granularitet.  

[Mer om hello kvarhållning verktyget](app-insights-usage-retention.md)

## <a name="custom-business-events"></a>Anpassade affärshändelser

tooget förstår vad användare göra med ditt webbprogram, är det användbart tooinsert rader med kod toolog anpassade händelser. Dessa händelser kan spåra allt från detaljerad användaråtgärder, till exempel klicka på specifika knappar, toomore betydande affärshändelser, till exempel inköp eller vinna spel. 

Men i vissa fall sidvisningar representerar användbar händelser, det är inte sant i allmänhet. En användare kan öppna en produktsidan utan att köpa hello produkten. 

Med specifika händelser, kan du skapa diagram användarnas pågår via webbplatsen. Du kan ta reda på inställningar för olika alternativ, och där de släppa ut eller har problem. Med denna kunskap kan fatta du välgrundade beslut om hello prioriteter i eftersläpning för utveckling.

Händelser kan loggas in webbsidan hello:

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

Eller i hello på serversidan av hello-webbprogram:

```C#
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

Du kan koppla egenskapen värden toothese händelser så att du kan filtrera eller dela upp hello händelser när du inspektera dem i hello-portalen. Dessutom är en standarduppsättning med egenskaper bifogade tooeach händelse, till exempel anonyma användar-ID, vilket gör att du tootrace hello sekvensen av aktiviteter för en enskild användare.

Lär dig mer om [anpassade händelser](app-insights-api-custom-events-metrics.md#trackevent) och [egenskaper](app-insights-api-custom-events-metrics.md#properties).

### <a name="slice-and-dice-events"></a>Rapportanvändarna händelser

I hello användare, sessioner och händelser verktyg du segmentera och undersök anpassade händelser efter användarnamn, händelse och egenskaper.
![Användare](./media/app-insights-usage-overview/users.png)  
  
## <a name="design-hello-telemetry-with-hello-app"></a>Design hello telemetri med hello app

När du utformar varje funktion i din app kan du överväga hur du kommer toomeasure dess framgång med dina användare. Bestäm vad du behöver toorecord och code hello spåra anrop för dessa händelser i din app från hello affärshändelser start.

## <a name="a--b-testing"></a>EN | B-testning
Om du inte vet vilken variant av en funktion blir bättre, släpp båda, gör varje tillgänglig toodifferent användare. Mäta hello framgångsrik varje och flytta tooa enhetlig version.

För den här tekniken kan du bifoga distinkta egenskapen värden tooall hello telemetri som skickas av varje version av appen. Du kan göra det genom att definiera egenskaperna i hello active TelemetryContext. Dessa standardegenskaperna läggs tooevery telemetri som programmet hello postmeddelandet – inte bara din egna meddelanden, men hello samt standard telemetri.

Filtrera i hello Application Insights-portalen och dela dina data på hello egenskapsvärden som toocompare hello olika versioner.

toodo, [ställa in en telemetri initieraren](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):

```C#


    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }
```

I hello web app initieraren, till exempel Global.asax.cs:

```C#

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

Alla nya TelemetryClients lägger automatiskt till hello-egenskapsvärde som du anger. Enskilda telemetriska händelser kan åsidosätta hello standardvärden.

## <a name="next-steps"></a>Nästa steg
   - [Användare, sessioner, händelser](app-insights-usage-segmentation.md)
   - [Trattar](usage-funnels.md)
   - [Kvarhållning](app-insights-usage-retention.md)
   - [Användarflöden](app-insights-usage-flows.md)
   - [Arbetsböcker](app-insights-usage-workbooks.md)
   - [Lägg till användarkontext](app-insights-usage-send-user-context.md)
