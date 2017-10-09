---
title: "aaaUsing Sök i Azure Application Insights | Microsoft Docs"
description: "Sök och filtrera rådata telemetri som skickas av ditt webbprogram."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 2a437555-8043-45ec-937a-225c9bf0066b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: df2b0eb017ad48bcdc6ef57d8dff207d120143b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-search-in-application-insights"></a>Använda sökning i Application Insights
Sökningen är en funktion i [Programinsikter](app-insights-overview.md) som du använder toofind och utforska enskilda telemetri objekt, till exempel sidvyer undantag, eller webbförfrågningar. Och du kan visa loggspårningar och händelser som du har kodats.

(Mer komplexa frågor över dina data, Använd [Analytics](app-insights-analytics-tour.md).)

## <a name="where-do-you-see-search"></a>Där kan du se sökningen?
### <a name="in-hello-azure-portal"></a>I hello Azure-portalen
Du kan öppna diagnostiska sökning uttryckligen från hello insikter Programöversikt bladet för ditt program:

![Öppna diagnostiska sökning](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)

Det kan öppnas när du klickar på via några diagram och rutnät artiklar. Dess filter konfigureras i det här fallet före toofocus för hello typ av objekt du valt. 

Till exempel på hello översikt bladet finns ett stapeldiagram begäranden som klassificerats av svarstid. Klickar du på via en prestanda intervallet toosee en lista med enskilda begäranden i den svarstid intervall:

![Klicka dig igenom begäran prestanda](./media/app-insights-diagnostic-search/07-open-from-filters.png)

hello huvuddelen av diagnostiska Search är en lista över telemetri objekt - serverbegäranden, sidan vyer, anpassade händelser som du har ett kodat och så vidare. Hello är överst i listan hello en sammanfattande diagram som visar antalet händelser över tid.

Klicka på Uppdatera tooget nya händelser.

### <a name="in-visual-studio"></a>I Visual Studio 2013

I Visual Studio finns också en Application Insights sökfönstret. Det är mest användbara för att visa telemetriska händelser som genererats av hello-program som du felsöker. Men det kan också visa hello händelser som samlas in från din publicerade app på hello Azure-portalen.

Öppna fönstret för hello Sök i Visual Studio:

![Visual Studio öppna Application Insights-sökning](./media/app-insights-diagnostic-search/32.png)

hello sökfönstret har funktioner liknande toohello web-portalen:

![Visual Studio Application Insights sökfönstret](./media/app-insights-diagnostic-search/34.png)

hello spåra åtgärden fliken är tillgänglig när du öppnar en begäran eller vyn sida. Ett ' åtgärden ' är en sekvens av händelser som är associerad med tooa begäran eller sidan vy. Till exempel kan beroendeanrop, undantag, loggar och anpassade händelser ingå i en enda åtgärd. hello spåra åtgärden fliken visar hello grafiskt tidsinställning och varaktighet för dessa händelser i en relation toohello begäran eller sidan. 

## <a name="inspect-individual-items"></a>Inspektera enskilda objekt
Markera alla objekt telemetri toosee nyckelfält och relaterade objekt. Klicka på ”...” Om du vill toosee hello fullständig uppsättning fält. 

![Klicka på nytt arbetsobjekt och redigera hello fält.](./media/app-insights-diagnostic-search/10-detail.png)

## <a name="filter-event-types"></a>Filtrera händelsetyper
Öppna hello filterbladet och välj hello händelse skriver du vill toosee. (Om du vill senare toorestore hello filter som du har öppnat hello-bladet, klickar du på Återställ.)

![Välj Filter och välj telemetri typer](./media/app-insights-diagnostic-search/02-filter-req.png)

hello händelsetyper är:

* **Spåra** - [diagnostikloggar](app-insights-asp-net-trace-logs.md) inklusive TrackTrace, log4Net, NLog och System.Diagnostic.Trace anrop.
* **Begära** -HTTP-begäranden tas emot av serverprogrammet, inklusive sidor, skript, bilder, filer och data. Dessa händelser finns används toocreate hello förfrågan och svar översikt över diagram.
* **Vyn sida** - [telemetri som skickas av hello webbklient](app-insights-javascript.md), används toocreate sidan Visa rapporter. 
* **Anpassad händelse** - om du har infogat anrop tooTrackEvent() i ordning för[övervaka användningen](app-insights-api-custom-events-metrics.md), kan du söka dem här.
* **Undantag** – utan felhantering [undantag i hello server](app-insights-asp-net-exceptions.md), och de som du loggar in med hjälp av TrackException().
* **Beroende** - [anrop från serverprogrammet](app-insights-asp-net-dependencies.md) tooother tjänster, till exempel REST API: er eller databaser och AJAX-anrop från din [klientkod](app-insights-javascript.md).
* **Tillgänglighet** -resultaten av [tillgänglighetstester](app-insights-monitor-web-app-availability.md).

## <a name="filter-on-property-values"></a>Filtrera på egenskapsvärden
Du kan filtrera händelser på hello värden för deras egenskaper. hello tillgängliga egenskaper är beroende av hello händelsetyper som du har valt. 

Till exempel välja ut begäranden med en specifik svarskod. 

![Expandera en egenskap och väljer ett värde](./media/app-insights-diagnostic-search/03-response500.png)

Om du väljer några värden för en viss egenskap har hello samma effekt som att välja alla värden. Den växlar av filtreringen för den egenskapen.

### <a name="narrow-your-search"></a>Begränsa sökningen
Observera att hello räknar toohello höger i hello filtervärden visar hur många förekomster det i hello aktuella filtreringen. 

Det är tydligt hello rpthuvud/medarbetare begäran resulterar i de flesta hello ”500” fel i det här exemplet:

![Expandera en egenskap och väljer ett värde](./media/app-insights-diagnostic-search/04-failingReq.png)




## <a name="find-events-with-hello-same-property"></a>Sök efter händelser med hello samma egenskap
Hitta alla hello objekt hello samma värde för egenskap:

![Högerklicka på en egenskap](./media/app-insights-diagnostic-search/12-samevalue.png)


## <a name="search-hello-data"></a>Sök hello data

> [!NOTE]
> toowrite mer komplexa frågor, öppna [ **Analytics** ](app-insights-analytics-tour.md) från hello överkant hello Sök bladet.
> 

Du kan söka efter villkoren i någon av hello egenskapsvärden. Detta är särskilt användbart om du har skrivit [anpassade händelser](app-insights-api-custom-events-metrics.md) med egenskapsvärden. 

Du kanske vill tooset ett tidsintervall som söker över ett kortare intervall är snabbare. 

![Öppna diagnostiska sökning](./media/app-insights-diagnostic-search/appinsights-311search.png)

Sök efter fullständiga ord, inte delsträngar. Använd citattecken tooenclose specialtecken.

| Sträng | är *inte* genom | men dessa hittar den |
| --- | --- | --- |
| HomeController.About |hem<br/>Domänkontrollant<br/>Ut | homecontroller<br/>Om<br/>”homecontroller.about”|
|USA|UNI<br/>Tomas|Förenade<br/>tillstånd<br/>Förenade och tillstånd<br/>”USA”

Här följer hello sökuttryck som du kan använda:

| Exempelfråga | Verkan |
| --- | --- |
| `apple` |Sök efter alla händelser i hello tidsintervall vars fält innehåller hello ordet ”apple” |
| `apple AND banana` |Sök efter händelser som innehåller båda orden. Använd kapital ”och”, inte ”och”. |
| `apple OR banana`<br/>`apple banana` |Sök efter händelser som innehåller antingen word. Använda ”eller”, inte ”eller”.<br/>Kort form. |
| `apple NOT banana` |Sök efter händelser som innehåller ett ord men inte hello andra. |



## <a name="sampling"></a>Samling
Om din app genererar mycket telemetri (och du använder hello SDK för ASP.NET version 2.0.0-beta3 eller senare), hello anpassningsbar provtagning modulen automatiskt minskar hello volymen som skickas toohello portal genom att skicka en representativ del av händelser. Dock händelser som är relaterade toohello samma begäran markeras eller avmarkeras som en grupp, så att du kan navigera mellan relaterade händelser. 

[Lär dig mer om insamling](app-insights-sampling.md).



## <a name="create-work-item"></a>Skapa arbetsobjekt
Du kan skapa ett programfel i GitHub eller Visual Studio Team Services med hello information från ett telemetri-objekt. 

![Klicka på nytt arbetsobjekt och redigera hello fält.](./media/app-insights-diagnostic-search/42.png)

hello första gången som du gör det uppmanas du tooconfigure en länk tooyour Team Services-konto och projektet.

![Fyll hello URL på Team Services-servern och hello projektnamn och klicka på auktorisera](./media/app-insights-diagnostic-search/41.png)

(Du kan också konfigurera hello länk på hello arbetsobjekt blad.)

## <a name="save-your-search"></a>Spara sökningen
När du har angett alla hello filter som du vill kan spara du hello sökning som en favorit. Om du arbetar i ett organisationskonto kan du välja om tooshare den med andra medlemmar.

![Klicka på favoriten hello namn och klicka på Spara](./media/app-insights-diagnostic-search/08-favorite-save.png)

toosee hello Sök igen, **gå toohello översikt bladet** och öppna Favoriter:

![Panelen Favoriter](./media/app-insights-diagnostic-search/09-favorite-get.png)

Om du har sparat med relativa tidsintervall har hello öppnas på nytt bladet hello senaste data. Om du har sparat med absolut tidsintervall du ser hello samma data varje gång. (Om 'Relativa' är inte tillgänglig när du vill toosave favorit, klicka på tidsintervall i hello-huvudet ange ett tidsintervall som inte är ett anpassat intervall)

## <a name="send-more-telemetry-tooapplication-insights"></a>Skicka telemetri om flera tooApplication insikter
I tillägg toohello out box telemetri skickas av Application Insights SDK kan du:

* Avbilda loggspårningar från dina Favoriter loggningsramverk i [.NET](app-insights-asp-net-trace-logs.md) eller [Java](app-insights-java-trace-logs.md). Detta innebär att du kan söka igenom din loggspårningar och korrelera dem med sidvisningar, undantag och andra händelser. 
* [Skriva kod](app-insights-api-custom-events-metrics.md) toosend anpassade händelser, sidvisningar och undantag. 

[Lär dig hur toosend loggar och anpassad telemetri tooApplication insikter](app-insights-asp-net-trace-logs.md).

## <a name="questions"></a>FRÅGOR OCH SVAR
### <a name="limits"></a>Hur mycket data finns kvar?

Se hello [gränser sammanfattning](app-insights-pricing.md#limits-summary).

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Hur kan jag se postdata i min serverbegäranden?
Vi inte logga hello postdata automatiskt, men du kan använda [TrackTrace eller loggfil anrop](app-insights-asp-net-trace-logs.md). Placera hello postdata i hello message-parameter. Du kan filtrera efter hello-meddelande i hello samma sätt kan du filtrera efter egenskaper, men hello storleksgränsen är längre.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="add"></a>Nästa steg
* [Skriva komplexa frågor i Analytics](app-insights-analytics-tour.md)
* [Skicka loggar och anpassad telemetri tooApplication insikter](app-insights-asp-net-trace-logs.md)
* [Ställ in tillgänglighet och svarstider tester](app-insights-monitor-web-app-availability.md)
* [Felsökning](app-insights-troubleshoot-faq.md)
