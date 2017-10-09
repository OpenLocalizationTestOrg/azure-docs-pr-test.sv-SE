---
title: aaaView Azure Application Insights AppData | Microsoft Docs
description: "Du kan använda hello Application Insights Connector lösning toodiagnose prestandaproblem och förstå vad användarna göra med din app när övervakas med Application Insights."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 49280cad-3526-43e1-a365-c6a3bf66db52
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: banders
ms.openlocfilehash: 38109337ebbc8970dccb65365ba8284d9cee19a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-connector-solution-preview-in-operations-management-suite-oms"></a>Application Insights Connector lösning (förhandsgranskning) i Operations Management Suite (OMS)

![Application Insights symbol](./media/log-analytics-app-insights-connector/app-insights-connector-symbol.png)

hello program Insights Connector lösningen hjälper dig att diagnostisera prestandaproblem och förstå vad användare göra med din app när den är övervakad med [Programinsikter](../application-insights/app-insights-overview.md). Vyer för hello samma programtelemetri som utvecklare finns i Application Insights är tillgängliga i OMS. När du integrerar dina Application Insights-appar med OMS ökas visningen av dina program genom att låta igen och programdata på ett ställe. Med hello samma vyer som hjälper dig att toocollaborate med apputvecklare. hello vanliga vyer kan du minska hello tid toodetect och lösa både program- och plattform problem.

När du använder hello-lösning kan du:

- Visa alla dina appar i Application Insights på en plats, även om de finns i olika Azure-prenumerationer
- Korrelera infrastrukturdata med programdata
- Visualisera programdata med perspektiv i loggen Sök
- Pivotera från logganalys data tooyour Application Insights-app i hello OMS och Azure portaler

## <a name="connected-sources"></a>Anslutna källor

Till skillnad från de flesta andra logganalys-lösningar kan inte samlas in för hello Application Insights Connector av agenter. Alla data som används av hello lösningen kommer direkt från Azure.

| Ansluten källa | Stöds | Beskrivning |
| --- | --- | --- |
| [Windows-agenter](log-analytics-windows-agents.md) | Nej | hello lösningen samlar inte in information från Windows-agenter. |
| [Linux-agenter](log-analytics-linux-agents.md) | Nej | hello lösningen samlar inte in information från Linux-agenter. |
| [SCOM-hanteringsgrupp](log-analytics-om-agents.md) | Nej | hello lösningen samlar inte in information från agenter i en ansluten SCOM-hanteringsgrupp. |
| [Azure Storage-konto](log-analytics-azure-storage.md) | Nej | hello-lösningen stöder inte samlingsinformation från Azure storage. |

## <a name="prerequisites"></a>Krav

- tooaccess Application Insights Connector information som du måste ha en Azure-prenumeration
- Du måste ha minst en konfigurerade Application Insights-resurs.
- Du måste vara hello ägare eller deltagare i hello Application Insights-resurs.

## <a name="configuration"></a>Konfiguration

1. Aktivera hello Azure Web Apps Analytics lösningar från hello [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ApplicationInsights?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).
2. I hello OMS-portalen klickar du på **inställningar** &gt; **Data** &gt; **Programinsikter**.
3. Under **välja en prenumeration**, välja en prenumeration med Application Insights-resurser och sedan under **programnamn**, Välj en eller flera program.
4. Klicka på **Spara**.

Data blir tillgängliga i cirka 30 minuter och hello Application Insights panelen uppdateras med data, till exempel hello följande bild:

![Application Insights panelen](./media/log-analytics-app-insights-connector/app-insights-tile.png)

Andra pekar tookeep i åtanke:

- Du kan bara koppla Application Insights appar tooone OMS-arbetsyta.
- Du kan bara koppla [Standard eller Premium Application Insights-resurser](https://azure.microsoft.com/pricing/details/application-insights) tooOMS logganalys. Du kan dock använda hello kostnadsfria nivån av logganalys.

## <a name="management-packs"></a>Hanteringspaket

Den här lösningen installerar inte några management packs i anslutna hanteringsgrupper.

## <a name="use-hello-solution"></a>Använd hello lösning

hello följande avsnitt beskrivs hur du kan använda hello-blad visas i hello Application Insights dashboard tooview och interagera med data från dina appar.

### <a name="view-application-insights-connector-information"></a>Visa information om Application Insights Connector

Klicka på hello **Programinsikter** panelen tooopen hello **Programinsikter** instrumentpanelen toosee hello följande blad.

![Instrumentpanel för program insikter](./media/log-analytics-app-insights-connector/app-insights-dash01.png)

![Instrumentpanel för program insikter](./media/log-analytics-app-insights-connector/app-insights-dash02.png)

hello instrumentpanelen innehåller hello blad hello tabell. Varje bladet visar upp too10 objekt som matchar att bladets kriterier för hello angetts scope och ett tidsintervall. Du kan köra en sökning i loggen som returnerar alla poster när du klickar på **se alla** längst ned hello hello bladet eller när du klickar på hello bladet sidhuvud.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

| **Kolumnen** | **Beskrivning** |
| --- | --- |
| Program – antal program | Visar hello antalet program i programresurser. Även listor programmet namn och för varje hello antal poster i programmet. Klicka på hello nummer toorun en logg sökning efter<code>Type=ApplicationInsights &#124; measure sum(SampledCount) by ApplicationName</code> <br><br>  Klicka på ett program namnet toorun en logg-sökning för hello-program som visar programmet poster per värd, poster av typen telemetri och alla data av typen (baserat på hello sista dagen). |
| Datavolymen – värdar som skickar data | Visar hello antalet värdar för datorn som data skickas. Visar också datorn är värd och antal poster för varje värd. Klicka på hello nummer toorun en logg sökning efter<code>Type=ApplicationInsights &#124; measure sum(SampledCount) by Host</code> <br><br> Klicka på en dator namn toorun en logg sökning efter hello värden som visar programmet poster per värd, poster av typen telemetri och alla data av typen (baserat på hello sista dagen). |
| Tillgänglighet – Webtest resultat | Visar ett ringdiagram för webbtestresultat, som anger pass eller misslyckas. Klicka på hello diagram toorun en logg sökning efter<code>Type=ApplicationInsights TelemetryType=Availability &#124; measure sum(SampledCount) by AvailabilityResult</code> <br><br> Resultatet visar hello antalet överför och fel för alla test. Den visar alla webbprogram med trafik för hello senaste minuten. Klicka på ett program namnet tooview en logg sökning visar information om misslyckade webbtest. |
| Serverbegäranden – förfrågningar per timme | Visar ett linjediagram för hello serverbegäranden per timme för olika program. Hovra över en rad i hello diagram toosee hello upp 3 program ta emot begäranden för en punkt i tiden. Visar också en lista över hello program tar emot förfrågningar och hello antal begäranden för hello valda perioden. <br><br>Klicka på hello diagram toorun en logg sökning efter <code>Type=ApplicationInsights TelemetryType=Request &#124; measure sum(SampledCount) by ApplicationName interval 1hour</code> som visar mer detaljerad linjediagram av hello serverbegäranden per timme för olika program. <br><br> Klicka på ett program i hello listan toorun en logg sökning efter <code>Type=ApplicationInsights  ApplicationName=yourapplicationname  TelemetryType=Request</code> som visar en lista med begäranden, diagram för begäranden över tid och begäran varaktighet och en lista över begäran svarskoder.   |
| Fel – misslyckade begäranden per timme | Visar ett linjediagram för misslyckade programförfrågningar per timme. Hovra över hello diagram toosee hello översta 3 program med misslyckade begäranden för en punkt i tiden. Visar en lista över program med hello antalet misslyckade begäranden för varje också. Klicka på hello diagram toorun en logg sökning efter <code>Type=ApplicationInsights TelemetryType=Request  RequestSuccess = false &#124; measure sum(SampledCount) by ApplicationName interval 1hour</code> som visar mer detaljerad linjediagram av begäranden om misslyckade program. <br><br>Klicka på ett objekt i hello listan toorun en logg sökning efter <code>Type=ApplicationInsights ApplicationName=yourapplicationname TelemetryType=Request  RequestSuccess=false</code> som visar misslyckade begäranden, diagram för misslyckade begäranden över tid och begäran varaktighet och en lista över svarskoder för misslyckade begäranden. |
| Undantag – undantag per timme | Visar ett linjediagram av undantag per timme. Hovra över hello diagram toosee hello översta 3-program med undantag för en punkt i tiden. Visar en lista över program med hello antalet undantag för varje också. Klicka på hello diagram toorun en logg sökning efter <code>Type=ApplicationInsights TelemetryType=Exception &#124; measure sum(SampledCount) by ApplicationName interval 1hour</code> som visar mer detaljerad länken i diagrammet över undantag. <br><br>Klicka på ett objekt i hello listan toorun en logg sökning efter <code>Type=ApplicationInsights  ApplicationName=yourapplicationname TelemetryType=Exception</code> som visar en lista över undantag, diagram för undantag över tid och misslyckade begäranden och en lista över typer av undantag.  |

### <a name="view-hello-application-insights-perspective-with-log-search"></a>Visa hello Application Insights perspektiv med Sök i loggfilen

När du klickar på ett objekt i hello instrumentpanelen kan du se ett perspektiv för Application Insights som visas i sökningen. hello perspektivet innehåller en utökad visualiseringen baserat på hello telemetri som valts. I så fall visualiseringen innehållsändringar för olika telemetri typer.

När du klickar någonstans i hello program bladet visas hello standard **program** perspektiv.

![Application Insights program perspektiv](./media/log-analytics-app-insights-connector/applications-blade-drill-search.png)

hello perspektiv visar en översikt över hello-program som du har valt.

Hej **tillgänglighet** bladet visar perspektivvyn olika där du kan se webbtestresultat och relaterade misslyckade begäranden.

![Application Insights tillgänglighet perspektiv](./media/log-analytics-app-insights-connector/availability-blade-drill-search.png)

När du klickar någonstans i hello **serverbegäranden** eller **fel** blad, hello perspektiv komponenter ändra toogive du en visualisering som relaterade toorequests.

![Application Insights fel bladet](./media/log-analytics-app-insights-connector/server-requests-failures-drill-search.png)

När du klickar någonstans i hello **undantag** bladet finns en visualisering som är anpassad tooexceptions.

![Application Insights undantag bladet](./media/log-analytics-app-insights-connector/exceptions-blade-drill-search.png)

Oavsett om du klickar på något en hello **Application Insights Connector** instrumentpanelen i hello **Sök** sidan, alla frågor som returnerar Application Insights-data visar hello Application Insights perspektiv. Om du visar Application Insights-data, till exempel en **&#42;** frågan visar också hello perspektiv fliken som hello följande bild:

![Application Insights ](./media/log-analytics-app-insights-connector/app-insights-search.png)

Perspektiv komponenter uppdateras beroende på hello sökfråga. Detta innebär att du kan filtrera hello resultat med hjälp av alla sökfältet ger du hello möjlighet toosee hello data från:

- Alla program
- Ett enda valt program
- En grupp av program

### <a name="pivot-tooan-app-in-hello-azure-portal"></a>Pivot tooan app i hello Azure-portalen

Application Insights Connector bladen är utformad tooenable toopivot toohello valt appen Application Insights *när du använder hello OMS-portalen*. Du kan använda hello lösning som en övergripande övervakning plattform som hjälper dig att felsöka en app. När du ser ett potentiellt problem i någon av dina anslutna program, kan du antingen gå till den i OMS sökning eller du kan pivotera direkt toohello Application Insights app.

toopivot, klicka på hello ellipserna (**...** ) som visas vid hello slutet av varje rad, och välj **öppna i Application Insights**.

>[!NOTE]
>**Öppna i Application Insights** är inte tillgänglig i hello Azure-portalen.

![Öppna i Application Insights](./media/log-analytics-app-insights-connector/open-in-app-insights.png)

### <a name="sample-corrected-data"></a>Korrigerade exempel data

Application Insights ger  *[provtagning korrigering](../application-insights/app-insights-sampling.md)*  toohelp minska telemetri trafiken. När du aktiverar provtagning på appen Application Insights hämta minskat antal poster som lagras i Application Insights och i OMS. Medan datakonsekvens bevaras i hello **Application Insights Connector** sida och perspektiv, bör du manuellt korrigera samplade data för dina egna frågor.

Här är ett exempel på provtagning korrigering i en sökning i loggen:

```
Type=ApplicationInsights | measure sum(SampledCount) by TelemetryType
```

Hej **provtagning antal** fältet finns i alla poster och visar hello antalet datapunkter som representerar hello-post. Om du aktiverar samplingsfrekvensen för appen Application Insights **provtagning antal** är större än 1. toocount hello faktiska antalet poster som programmet skapar summan hello **provtagning antal** fält.

Sampling påverkar endast hello Totalt antal poster som programmet skapar. Du behöver inte toocorrect provtagning för mått fält som **RequestDuration** eller **AvailabilityDuration** eftersom dessa fält visar hello genomsnittet för representeras poster.

## <a name="input-data"></a>Indata

hello lösning tar emot hello följande telemetri typer av data från dina anslutna appar Application Insights:

- Tillgänglighet
- Undantag
- Begäranden
- Sidvisningar – för din arbetsyta tooreceive sidvisningar måste du konfigurera dina appar toocollect informationen. Mer information finns i [PageViews](../application-insights/app-insights-api-custom-events-metrics.md#page-views).
- Anpassade händelser – för din arbetsyta tooreceive anpassade händelser, måste du konfigurera dina appar toocollect informationen. Mer information finns i [TrackEvent](../application-insights/app-insights-api-custom-events-metrics.md#trackevent).

Data tas emot av OMS från Application Insights vartefter det blir tillgängligt.

## <a name="output-data"></a>utdata

En post med en *typen* av *ApplicationInsights* skapas för varje typ av indata. ApplicationInsights poster har egenskaper som visas i följande avsnitt hello:

### <a name="generic-fields"></a>Allmän fält

| Egenskap | Beskrivning |
| --- | --- |
| Typ | ApplicationInsights |
| ClientIP |   |
| TimeGenerated | Tiden hello posten |
| ApplicationId | Instrumentation nyckeln hello Application Insights-appen |
| ApplicationName | Namnet på hello Application Insights app |
| RoleInstance | ID för server-värd |
| DeviceType | Klientenheten |
| ScreenResolution |   |
| Kontinent | Kontinent där hello begäran har sitt ursprung |
| Land/region | Land där hello begäran har sitt ursprung |
| Provins | Region, status eller locale där hello begäran har sitt ursprung |
| Ort | Stad där hello begäran har sitt ursprung |
| isSynthetic | Anger om hello begäran har skapats av en användare eller automatiserad metod. = Användargenererat TRUE eller false = automatiserad metod |
| SamplingRate | Procentandelen telemetri som genererats av hello SDK som skickas tooportal. Intervallet 0,0 100,0. |
| SampledCount | 100/(SamplingRate). Till exempel 4 =&gt; 25% |
| IsAuthenticated | Sant eller falskt |
| Åtgärds-ID | Objekt som har samma åtgärds-ID visas som relaterade objekt i hello portal hello. Vanligtvis hello begärande-ID |
| ParentOperationID | ID för hello överordnade åtgärden |
| OperationName |   |
| Sessions-ID | GUID toouniquely identifiera hello session där hello begäran har skapats |
| SourceSystem | ApplicationInsights |

### <a name="availability-specific-fields"></a>Tillgänglighet-specifika fält

| Egenskap | Beskrivning |
| --- | --- |
| TelemetryType | Tillgänglighet |
| AvailabilityTestName | Namnet på hello webbtestet |
| AvailabilityRunLocation | Geografisk källan för http-begäran |
| AvailabilityResult | Anger hello lyckade resultat av hello webbtestet |
| AvailabilityMessage | hello-meddelande kopplade toohello webbtestet |
| AvailabilityCount | 100 /(Sampling Rate). Till exempel 4 =&gt; 25% |
| DataSizeMetricValue | 1.0 eller 0,0 |
| DataSizeMetricCount | 100 /(Sampling Rate). Till exempel 4 =&gt; 25% |
| AvailabilityDuration | Tid i millisekunder för testperioden för hello web |
| AvailabilityDurationCount | 100 /(Sampling Rate). Till exempel 4 =&gt; 25% |
| AvailabilityValue |   |
| AvailabilityMetricCount |   |
| AvailabilityTestId | Unikt GUID för hello webbtestet |
| AvailabilityTimestamp | Exakt tidsstämpeln för hello tillgänglighetstest |
| AvailabilityDurationMin | För provade poster visar i det här fältet hello minsta web testperioden (millisekunder) för datapunkter hello representeras |
| AvailabilityDurationMax | För provade poster visar i det här fältet hello maximala web testperioden (millisekunder) för datapunkter hello representeras |
| AvailabilityDurationStdDev | Det här fältet visar hello standardavvikelsen mellan alla web test varaktighet (i millisekunder) för datapunkter hello representeras för provade poster |
| AvailabilityMin |   |
| AvailabilityMax |   |
| AvailabilityStdDev | &nbsp;  |

### <a name="exception-specific-fields"></a>Undantag-specifika fält

| Typ | ApplicationInsights |
| --- | --- |
| TelemetryType | Undantag |
| ExceptionType | Typ av hello undantag |
| ExceptionMethod | hello-metod som skapar hello-undantag |
| ExceptionAssembly | Sammansättningen som innehåller hello framework och version samt hello-token för offentlig nyckel |
| ExceptionGroup | Typ av hello undantag |
| ExceptionHandledAt | Anger hello som hanteras hello undantag |
| ExceptionCount | 100 /(Sampling Rate). Till exempel 4 =&gt; 25% |
| ExceptionMessage | Meddelande av hello undantag |
| ExceptionStack | Fullständig stack för hello undantag |
| ExceptionHasStack | TRUE om undantag har en stack |



### <a name="request-specific-fields"></a>Begäran-specifika fält

| Egenskap | Beskrivning |
| --- | --- |
| Typ | ApplicationInsights |
| TelemetryType | Förfrågan |
| ResponseCode | HTTP-svaret tooclient |
| RequestSuccess | Anger lyckad eller misslyckad. SANT eller FALSKT. |
| Begärande-ID | ID toouniquely identifiera hello begäran |
| RequestName | GET/POST + URL-bas |
| RequestDuration | Tid i sekunder för hello begäran varaktighet |
| URL: EN | URL för hello begäran inte inklusive värden |
| Värd | Web server-värd |
| URLBase | Fullständig Webbadress för hello-begäran |
| ApplicationProtocol | Typ av protokoll som används av programmet hello |
| RequestCount | 100 /(Sampling Rate). Till exempel 4 =&gt; 25% |
| RequestDurationCount | 100 /(Sampling Rate). Till exempel 4 =&gt; 25% |
| RequestDurationMin | Fältet visar hello minsta begäran varaktighet (i millisekunder) för datapunkter hello representeras för provade poster. |
| RequestDurationMax | Det här fältet visar hello maximal varaktighet för begäran (millisekunder) för datapunkter hello representeras för provade poster |
| RequestDurationStdDev | Det här fältet visar hello standardavvikelsen mellan alla begäran varaktighet (i millisekunder) för datapunkter hello som representeras för provade poster |

## <a name="sample-log-searches"></a>Exempel på loggsökningar

Den här lösningen har inte en uppsättning exempel loggen sökningar visas på hello instrumentpanelen. Dock visas exempelfrågor loggen sökning med beskrivningar i hello [visa Application Insights Connector information](#view-application-insights-connector-information) avsnitt.

## <a name="next-steps"></a>Nästa steg

- Använd [loggen Sök](log-analytics-log-searches.md) tooview detaljerad information om Application Insights-appar.
