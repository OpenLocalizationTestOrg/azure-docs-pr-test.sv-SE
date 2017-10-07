---
title: "aaaAzure översikt över Mobile Engagement Web SDK | Microsoft Docs"
description: "Hej senaste uppdateringarna och procedurer för hello Web SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 5bbc2fda-0f3f-43d0-a73d-0f2c0f8dc25b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 10/18/2016
ms.author: piyushjo
ms.openlocfilehash: 9e60a232b5eb2c41c405041a88e09d7137563513
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-web-sdk"></a>Azure Mobile Engagement Web SDK
Börja här för alla hello information om hur toointegrate Azure Mobile Engagement i en webbapp. Om du vill att toogive den ett försök innan du kommer igång med din egen webbprogram finns våra [15 minuter kursen](mobile-engagement-web-app-get-started.md).

## <a name="integration-procedures"></a>Integration procedurer
1. Läs [hur toointegrate Mobile Engagement i ditt webbprogram](mobile-engagement-web-integrate-engagement.md).
2. Taggen planera implementeringen Läs [hur toouse hello avancerade Mobile Engagement API-märkning i webbappen](mobile-engagement-web-use-engagement-api.md).

## <a name="release-notes"></a>Viktig information
### <a name="202-10182016"></a>2.0.2 (10/18/2016)
* Fast krascher om privata bläddring (Safari).
* Fast krascher i webbläsare med cookies inaktiverade.

Alla versioner finns hello [slutföra viktig information](mobile-engagement-web-release-notes.md).

## <a name="upgrade-procedures"></a>Uppgraderingsprocesser
### <a name="upgrade-from-121-too200"></a>Uppgradera från 1.2.1 too2.0.0
hello följande avsnitt beskrivs hur toomigrate en Mobile Engagement Web SDK-integration från hello Capptain tjänsten erbjuds av Capptain SAS, tooan Azure Mobile Engagement-app. Om du migrerar från en tidigare version än 1.2.1, kontakta hello Capptain webbplats toomigrate too1.2.1 först och sedan använda hello följande procedurer.

Den här versionen av hello Mobile Engagement Web SDK stöder inte Samsung Smart TV, Opera TV, webOS eller hello Reach-funktionen.

> [!IMPORTANT]
> Capptain och Azure Mobile Engagement hello inte samma tjänst och hello följande procedurer Markera endast hur toomigrate hello-klientappen. Migrera hello Mobile Engagement Web SDK i hello app kommer inte att migrera dina data från en Capptain tooa Mobile Engagement-server.
> 
> 

#### <a name="javascript-files"></a>JavaScript-filer
Ersätt hello filen capptain-sdk.js med hello filen azure engagement.js och uppdatera din skript importerar på samma sätt.

#### <a name="remove-capptain-reach"></a>Ta bort Capptain Reach
Den här versionen av hello Mobile Engagement Web SDK stöder inte hello Reach-funktionen. Om du har integrerat Capptain Reach i ditt program, måste du tooremove den.

Ta bort hello nå CSS import från din sida och ta bort hello relaterade CSS-filen (capptain-reach.css, som standard).

Ta bort hello följer räckvidden resurser: hello hello varumärkesikon (capptain-meddelande-ikonen, som standard) och Stäng bild (capptain-close.png, som standard).

Ta bort hello nå Användargränssnittet för aviseringar via appen. hello standardlayout ser ut så här:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Ta bort hello nå Användargränssnittet för text- och meddelanden och avsökningar. hello standardlayout ser ut så här:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Ta bort hello `reach` objekt från konfigurationen, om den finns. Det ser ut så här:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Ta bort alla andra Reach anpassning, till exempel.

#### <a name="remove-deprecated-apis"></a>Ta bort föråldrade API: er
Vissa API: er från Capptain är föråldrade i hello Mobile Engagement Web SDK.

Ta bort alla anrop toohello följande API: er: `agent.connect`, `agent.disconnect`, `agent.pause`, och `agent.sendMessageToDevice`.

Ta bort någon av följande återanrop från konfigurationen Capptain hello: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, och `onPushMessageReceived`.

#### <a name="configuration"></a>Konfiguration
Mobile Engagement använder en anslutning tooconfigure SDK strängidentifierare, till exempel hello identifierare.

Ersätt hello program-ID med anslutningssträngen. Observera att globala hello-objekt för hello SDK konfiguration ändras från `capptain` för`azureEngagement`.

Före migrering:

    window.capptain = {
      appId: ...,
      [...]
    };

Efter migreringen:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

hello anslutningssträngen för ditt program visas i hello Azure-portalen.

#### <a name="javascript-apis"></a>JavaScript API: er
hello globala JavaScript-objekt `window.capptain` har bytt `window.azureEngagement`, men du kan använda hello `window.engagement` alias för API-anrop. Du kan inte använda den här alias toodefine hello SDK-konfigurationen.

Till exempel `capptain.deviceId` blir `engagement.deviceId`, `capptain.agent.startActivity` blir `engagement.agent.startActivity`och så vidare.

Om en tidigare version av hello Azure Mobile Engagement Web SDK har redan integrerats i programmet, Läs om [uppgradera procedurer](mobile-engagement-web-upgrade-procedure.md).

