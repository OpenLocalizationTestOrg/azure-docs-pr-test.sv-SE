---
title: aaaAzure Application Insights telemetri datamodellen - telemetri kontexten | Microsoft Docs
description: Application Insights telemetri kontexten datamodellen
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/15/2017
ms.author: sergkanz
ms.openlocfilehash: 6cdd6240d1c448f883d104a871ee9d5f6b5af2ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="telemetry-context-application-insights-data-model"></a>Telemetri kontext: Application Insights-datamodell

Varje telemetri-objekt kan ha ett starkt typifierad kontexten fält. Samtliga fält gör det möjligt för ett specifikt scenario för övervakning. Använd hello anpassade egenskaper samling toostore anpassade eller programspecifika kontextuella information.


##<a name="application-version"></a>Programversion

Hello programmet kontexten fälten är alltid om hello-program som skickar hello telemetri. Programversion är används tooanalyze trend ändringar i hello programmets beteende och dess korrelation toohello distributioner.

Maxlängd: 1024


##<a name="client-ip-address"></a>Klientens IP-adress

hello IP-adressen för hello klientenhet. IPv4 och IPv6 stöds. När telemetri som skickas från en tjänst, är hello plats kontext om hello-användaren som initierade hello-åtgärden i hello-tjänsten. Application Insights extrahera hello geografiska plats information från hello klientens IP-Adressen och sedan trunkera den. Så att klientens IP-Adressen ensamt kan inte användas som slutanvändaren identifierbar information. 

Maxlängd: 46


##<a name="device-type"></a>Typ av enhet

Ursprungligen användes i det här fältet tooindicate hello typ av enhet hello hello slutanvändaren av programmet hello används. Idag används främst toodistinguish JavaScript telemetri med hello enhetstyp 'Webbläsare' från serversidan telemetri med hello enhetstyp ”dator”.

Maxlängd: 64


##<a name="operation-id"></a>Åtgärds-id

En unik identifierare för hello rot igen. Den här identifieraren kan toogroup telemetri över flera komponenter. Se [telemetri korrelation](application-insights-correlation.md) mer information. hello åtgärds-id skapas av en begäran eller vyn sida. All telemetri anger det här fältet toohello värde för hello som innehåller begäran eller sidan. 

Maxlängd: 128


##<a name="parent-operation-id"></a>Överordnad åtgärds-ID

Hej Unik identifierare för hello telemetri objektets omedelbart överordnade objekt. Se [telemetri korrelation](application-insights-correlation.md) mer information.

Maxlängd: 128


##<a name="operation-name"></a>Åtgärdsnamn

hello namn (gruppering) av hello-åtgärden. hello åtgärdsnamn skapas av en begäran eller vyn sida. Alla andra objekt i telemetri ange fältet toohello värdet för hello som innehåller begäran eller sidan. Åtgärdsnamnet används för att söka efter alla hello telemetri objekt för en grupp av åtgärder (till exempel ' GET-Home/Index'). Den här kontexten-egenskapen är används tooanswer frågor som ”vad är hello vanliga undantag på den här sidan”.

Maxlängd: 1024


##<a name="synthetic-source-of-hello-operation"></a>Syntetiska källan för hello åtgärden

Namnet på syntetiska källa. Vissa telemetri från hello program kan representera syntetisk trafik. Det kan vara web crawler indexering hello-webbplats, tillgänglighetstester för webbplatsen eller spår från diagnostik bibliotek som Application Insights SDK sig själv.

Maxlängd: 1024


##<a name="session-id"></a>Sessions-id

Sessions-ID - hello hello användarens interaktion med hello app-instansen. Hello session kontexten fälten är alltid om hello slutanvändaren. När telemetri som skickas från en tjänst, är hello sessionskontexten om hello-användaren som initierade hello-åtgärden i hello-tjänsten.

Maxlängd: 64


##<a name="anonymous-user-id"></a>Anonym användar-id

Id för anonyma användare. Representerar hello slutanvändaren av programmet hello. När telemetri som skickas från en tjänst, är hello användarkontext om hello-användaren som initierade hello-åtgärden i hello-tjänsten.

[Provtagning](app-insights-sampling.md) är en av hello tekniker toominimize hello mängden insamlade telemetri. Exempel in eller ut alla hello korrelerad provtagning algoritmen försök tooeither telemetri. Anonym användar-id används för provtagning poäng generation. Så att anonyma användar-id måste vara ett tillräckligt slumpmässiga värde. 

Med hjälp av användarnamn för anonym användare id toostore är ett missbruk av hello-fältet. Använd autentiserad användar-id.

Maxlängd: 128


##<a name="authenticated-user-id"></a>Autentiserat användar-id

Autentiserat användar-id. hello motsatt av anonym användar-id, i det här fältet visar hello användare med ett eget namn. Eftersom dess PII-information den har inte samlats in som standard av de flesta SDK.

Maxlängd: 1024


##<a name="account-id"></a>Konto-id

I program med flera klienter är hello konto-ID eller namn, vilka hello användare fungerar med. Exempel kanske prenumerations-ID för Azure-portalen eller blogg bloggar plattform.

Maxlängd: 1024


##<a name="cloud-role"></a>Molnet roll

Namnet på hello rollen hello programmet är en del av. Mappar direkt toohello rollnamnet i azure. Måste använda toodistinguish micro tjänster som ingår i ett enda program.

Maxlängd: 256


##<a name="cloud-role-instance"></a>Molnet rollinstans

Namn på hello-instans där hello programmet körs. Datornamnet för lokal instansnamn för Azure.

Maxlängd: 256


##<a name="internal-sdk-version"></a>Internt: SDK-version

SDK-version. Se https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification information.

Maxlängd: 64


##<a name="internal-node-name"></a>Internt: Nodnamnet

Det här fältet motsvarar hello nodnamnet används för fakturering. Använd den toooverride hello standard identifiering av noder.

Maxlängd: 256


## <a name="next-steps"></a>Nästa steg

- Lär dig hur för[utöka och filtrera telemetri](app-insights-api-filtering-sampling.md).
- Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.
- Kolla standard kontexten egenskapssamlingen [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).
