---
title: "aaaAzure Application Insights telemetri datamodellen - begäran telemetri | Microsoft Docs"
description: "Application Insights datamodellen för begärandetelemetri"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: bwren
ms.openlocfilehash: 6042975a35f5e672e5adb5390feecc63d0b284b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="request-telemetry-application-insights-data-model"></a>Begära telemetri: Application Insights-datamodell

En begäran telemetri objekt (i [Programinsikter](app-insights-overview.md)) representerar hello logisk följd av körningen som utlöses av en extern begäran tooyour program. Varje begäran körs identifieras med unikt `ID` och `url` som innehåller alla hello körning av parametrar. Du kan gruppera begäranden efter logiska `name` och definiera hello `source` för den här begäran. Kodkörning kan resultera i `success` eller `fail` och har en viss `duration`. Både lyckade och misslyckade körningar kan grupperas ytterligare med `resultCode`. Starttid för hello begärandetelemetri som definierats på hello kuvert nivå.

Begäran telemetri stöder hello standard utökningsbarhetsmodell med hjälp av anpassade `properties` och `measurements`.

## <a name="name"></a>Namn

Namnet på hello begäran representerar sökvägen tooprocess hello begäran. Låg kardinalitet värdet tooallow bättre gruppering av begäranden. För HTTP-begäran som den representerar hello HTTP-metoden och mallen för URL-sökväg som `GET /values/{id}` utan hello faktiska `id` värde.

Application Insights webb-SDK skickar begäran namn ”i befintligt skick” med angående tooletter fallet. Gruppering på Användargränssnittet är skiftlägeskänsligt så `GET /Home/Index` räknas separat från `GET /home/INDEX` även om ofta de resulterar i hello samma kontrollanten och åtgärden körning. Hej för som beror på att URL: er i allmänhet är [skiftlägeskänsliga](http://www.w3.org/TR/WD-html40-970708/htmlweb.html). Du kanske vill toosee om alla `404` inträffade för hello URL: er har skrivit in versaler. Du kan läsa mer på begäran namnsamling av ASP.Net Web SDK i hello [blogginlägget](http://apmtips.com/blog/2015/02/23/request-name-and-url/).

Maxlängd: 1024 tecken

## <a name="id"></a>ID

Identifierare för en instans för anrop av begäran. Används för korrelation mellan begäran och andra telemetri-objekt. ID ska vara globalt unika. Mer information finns i [korrelation](application-insights-correlation.md) sidan.

Maxlängd: 128 tecken

## <a name="url"></a>URL

URL-begäran med alla frågan string-parametrar.

Maxlängd: 2048 tecken

## <a name="source"></a>Källa

Källan för hello-begäran. Exempel är hello instrumentation nyckeln för hello anroparen eller hello ip-adressen för hello anroparen. Mer information finns i [korrelation](application-insights-correlation.md) sidan.

Maxlängd: 1024 tecken

## <a name="duration"></a>Varaktighet

Begär tid i formatet: `DD.HH:MM:SS.MMMMMM`. Måste vara positiv och mindre än `1000` dagar. Det här fältet är obligatoriskt eftersom begärandetelemetri motsvarar hello igen med hello början och slutet av hello.

## <a name="response-code"></a>Svarskod

Resultatet av en begäran körs. HTTP-statuskoden för HTTP-begäranden. Det kan vara `HRESULT` värde eller ett undantag av typen för andra typer av begäranden.

Maxlängd: 1024 tecken

## <a name="success"></a>Lyckades

Uppgift om lyckade och misslyckade anrop. Det här fältet är obligatoriskt. När inte anges explicit för`false` -begäran som toobe genomförd. Ange ett värde för`false` om åtgärden avbröts av undantagsfel eller returnerade felkoden resultat.

Hello-webbprogram, Programinsikter definiera för begäran som misslyckades när hello svarskoden är mindre hello `400` än eller lika med för`401`. Det finns dock fall när standardmappningen inte matchar hello semantiska av programmet hello. Svarskoden `404` kan tyda på ”inga poster”, som kan vara en del av regelbundet flöde. Det kan också indikera en bruten länk. Hello brutna länkar, kan du även implementera mer avancerad logik. Du kan markera brutna länkar som fel endast när dessa länkar befinner sig på samma plats genom att analysera url referent hello. Eller markera dem som fel vid åtkomst från hello företagets mobila program. På liknande sätt `301` och `302` anger ett fel vid åtkomst från hello-klient som stöder omdirigering.

Delvis accepteras innehåll `206` kan tyda på ett fel på en övergripande begäran. Application Insights slutpunkten får exempelvis en batch med telemetri artiklar som en enskild begäran. Den returnerar `206` när vissa objekt i hello batch har inte bearbetats. Öka mängden `206` tyder på problem som behöver toobe undersökas. Liknande logik som gäller för`207` flera Status där hello lyckas kanske hello sämsta av separata svarskoder.

Du kan läsa mer på begäran resultatet kod och statuskoden hello [blogginlägget](http://apmtips.com/blog/2016/12/03/request-success-and-response-code/).

## <a name="custom-properties"></a>Anpassade egenskaper

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Anpassade mått

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Nästa steg

- [Skriva anpassade begärandetelemetri](app-insights-api-custom-events-metrics.md#trackrequest)
- Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.
- Lär dig hur för[konfigurerar ASP.NET Core](app-insights-asp-net.md) program med Application Insights.
- Checka ut [plattformar](app-insights-platforms.md) stöds av Application Insights.
