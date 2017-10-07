---
title: aaaAzure Mobile Engagement Web SDK integration | Microsoft Docs
description: "Hej senaste uppdateringarna och procedurer för hello Azure Mobile Engagement Web SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: b5daa2a2-942b-489d-aa1d-568c3b25e56f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 02/29/2016
ms.author: piyushjo
ms.openlocfilehash: 99613b68b615bec4ddcfcc8e4e0133ce9d887bad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Integrera Azure Mobile Engagement i ett webbprogram
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

hello beskrivs i den här artikeln hello enklaste sättet tooactivate hello analyser och övervakningsfunktionerna i Azure Mobile Engagement i ditt webbprogram.

Följ hello steg tooactivate hello loggen rapporter som är nödvändiga toocompute all statistik om användare, sessioner, aktiviteter, krascher och technicals. För program-beroende statistik, till exempel händelser, fel och jobb, måste du aktivera loggen rapporter manuellt med hjälp av hello Azure Mobile Engagement-API. Mer information lär du dig [hur toouse hello avancerade Mobile Engagement API-märkning i ett webbprogram](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>Introduktion
[Hämta hello Azure Mobile Engagement Web SDK](http://aka.ms/P7b453).
hello Mobile Engagement Web SDK levereras som en JavaScript-fil, azure-engagement.js som du har tooinclude på varje sida i din webbplats eller programmet.

> [!IMPORTANT]
> Innan du kör det här skriptet måste du köra ett skript eller kod fragment som du skriver tooconfigure Mobile Engagement för ditt program.
> 
> 

## <a name="browser-compatibility"></a>Webbläsarkompatibilitet
hello Mobile Engagement Web SDK använder inbyggd JSON kodning och avkodning dessutom toocross domän AJAX-begäranden (förlita dig på hello W3C CORS specification). Den är kompatibel med hello följande webbläsare:

* Microsoft Edge 12 +
* Internet Explorer 10 +
* Firefox 3.5 +
* Chrome 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Konfigurera Mobile Engagement
Skriva ett skript som skapar en global `azureEngagement` JavaScript-objekt enligt följande exempel hello. Eftersom din plats kan ha flera sidor, förutsätter det här exemplet att det här skriptet ingår i varje sida. I det här exemplet hello JavaScript-objekt med namnet `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

Hej `connectionString` värdet för ditt program visas i hello Azure-portalen.

> [!NOTE]
> `appVersionName`och `appVersionCode` är valfria. Vi rekommenderar dock att du konfigurerar dem så att analytics kan bearbeta versionsinformation.
> 
> 

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Inkludera Mobile Engagement skript i sidorna
Lägga till Mobile Engagement skript tooyour sidor i något av följande sätt hello:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Eller:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias
Efter hello Mobile Engagement Web SDK skript har lästs in, skapar den hello **engagement** alias tooaccess hello SDK-API: er. Du kan inte använda den här alias toodefine hello SDK-konfigurationen. Det här aliaset används som en referens i den här dokumentationen.

Observera att om hello Standardalias i konflikt med en annan global variabel från din sida, du kan ändra den i hello konfiguration enligt följande innan du läser in hello Mobile Engagement Web SDK:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Grundläggande reporting
Grundläggande rapportering i Mobile Engagement innehåller statistik för session-nivå, till exempel statistik om användare, sessioner, aktiviteter och krascher.

### <a name="session-tracking"></a>Sessionsspårning
En Mobile Engagement-session är uppdelad i en serie aktiviteter, alla identifierade med ett namn.

I klassiska webbplats rekommenderar vi att du deklarera en annan aktivitet på varje sida i din webbplats. För en webbplatsen eller programmet i vilka hello sidan aldrig ändras, kanske du vill tootrack hello aktiviteter i mindre skala, såsom hello-sidan.

Antingen sätt, toostart eller ändra hello aktuella användaraktivitet, anrop hello `engagement.agent.startActivity` funktion. Exempel:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

hello Mobile Engagement-servern avslutar automatiskt en öppen session inom tre minuter efter hello appen på sidan är stängd.

Alternativt kan du avsluta en session manuellt genom att anropa `engagement.agent.endActivity`. Anger hello aktuella användare aktiviteten too'Idle ”.  hello-sessionen avslutas 10 sekunder senare om ett nytt anrop för`engagement.agent.startActivity` återupptar hello-sessionen.

Du kan konfigurera hello 10 sekunder fördröjning i hello globala engagement objekt på följande sätt:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end hello session as soon as endActivity is called

> [!NOTE]
> Du kan inte använda `engagement.agent.endActivity` i hello `onunload` motringning eftersom du inte göra AJAX-anrop i det här skedet.
> 
> 

## <a name="advanced-reporting"></a>Avancerad rapportering
Om du vill tooreport programspecifika händelser, fel och jobb, måste du eventuellt toouse hello Mobile Engagement-API. Du kommer åt hello Mobile Engagement API via hello `engagement.agent` objekt.

Du kan komma åt alla hello avancerade funktioner i Mobile Engagement i hello Mobile Engagement-API. hello API beskrivs i artikel hello [hur toouse hello avancerade Mobile Engagement API-märkning i ett webbprogram](mobile-engagement-web-use-engagement-api.md).

## <a name="customize-hello-urls-used-for-ajax-calls"></a>Anpassa hello URL: er används för AJAX-anrop
Du kan anpassa URL: er som hello Mobile Engagement Web SDK använder. Till exempel tooredefine hello loggnings-URL (hello SDK slutpunkten för loggning), kan du åsidosätta hello konfiguration som detta:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Om URL-funktioner returnerar en sträng som börjar med `/`, `//`, `http://`, eller `https://`, hello standardschema används inte. Som standard hello `https://` schema används för dessa URL: er. Om du vill toocustomize hello standardschema åsidosätta hello konfiguration, så här:

    window.azureEngagement = {
      ...
      urls: {
        ...         
        scheme: '//'
      }
    };
