---
title: aaaAzure Mobile Engagement Web SDK uppgraderingsprocesser | Microsoft Docs
description: "Hej senaste uppdateringarna och procedurer för hello Web SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a20529b4-ec8d-4503-8ae9-09b5f0846d5b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: a2df65904c6b56584ce6588ed26a9b79f3aa27ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Azure Mobile Engagement Web SDK uppgraderingsprocesser
Om du redan har integrerat en tidigare version av hello Azure Mobile Engagement Web SDK i ditt webbprogram måste tooconsider hello följande punkter när du uppgraderar hello SDK.

Om du hoppade över flera versioner av hello Mobile Engagement Web SDK, kanske du måste toocomplete flera procedurer hello uppgraderingsprocessen. Till exempel om du migrerar från 1.4.0 too1.6.0 första Följ hello procedurer tooupgrade från 1.4.0 too1.5.0. Följ sedan hello procedurer tooupgrade 1.5.0 too1.6.0.

Oavsett vilken version som du uppgraderar från, ersätta en tidigare version av hello filen azure-engagement.js med hello senaste versionen av hello-fil.

## <a name="upgrade-from-121-too200"></a>Uppgradera från 1.2.1 too2.0.0
Det här avsnittet beskrivs hur toomigrate en Mobile Engagement Web SDK-integration från hello Capptain tjänsten erbjuds av Capptain SAS, tooan Azure Mobile Engagement-app. Om du migrerar från en tidigare version du finns hello Capptain webbplats toofirst migrera too1.2.1 och tillämpa sedan hello följande procedurer.

Den här versionen av hello Mobile Engagement Web SDK stöder inte Samsung Smart TV, Opera TV, webOS eller hello Reach-funktionen.

> [!IMPORTANT]
> Capptain och Azure Mobile Engagement är inte hello samma tjänst. hello följande procedur visar hur endast toomigrate hello-klientappen. Migrera hello Mobile Engagement Web SDK i hello app kommer inte att migrera dina data från en Capptain tooa Mobile Engagement-server.
> 
> 

### <a name="javascript-files"></a>JavaScript-filer
Ersätt hello filen capptain-sdk.js med hello filen azure engagement.js och uppdatera din skript importerar på samma sätt.

### <a name="remove-capptain-reach"></a>Ta bort Capptain Reach
Den här versionen av hello Mobile Engagement Web SDK stöder inte hello Reach-funktionen. Om du har integrerat Capptain Reach i ditt program måste tooremove den.

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

### <a name="remove-deprecated-apis"></a>Ta bort föråldrade API: er
Vissa API: er från Capptain är föråldrade i hello Mobile Engagement Web SDK.

Ta bort alla anrop toohello följande API: er: `agent.connect`, `agent.disconnect`, `agent.pause`, och `agent.sendMessageToDevice`.

Ta bort alla instanser av följande återanrop från konfigurationen Capptain hello: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, och `onPushMessageReceived`.

### <a name="configuration"></a>Konfiguration
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

### <a name="javascript-apis"></a>JavaScript API: er
hello globala JavaScript-objekt `window.capptain` har bytt `window.azureEngagement` men du kan använda hello `window.engagement` alias för API-anrop. Du kan inte använda hello-alias toodefine hello SDK konfiguration.

Till exempel `capptain.deviceId` blir `engagement.deviceId`, `capptain.agent.startActivity` blir `engagement.agent.startActivity`och så vidare.

