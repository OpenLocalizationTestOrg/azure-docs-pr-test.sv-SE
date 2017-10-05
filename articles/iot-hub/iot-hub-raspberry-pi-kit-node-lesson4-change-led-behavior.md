---
title: "Ansluta Raspberry Pi (nod) till Azure IoT - lektionen 4: ändra app | Microsoft Docs"
description: "Anpassa meddelandena som du vill ändra Indikatorns och inaktivera beteende."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: kontrollen ledde raspberry pi, raspberry pi ledde kontroll, raspberry pi kontroll ledde
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 3b42a4ad-0197-42f6-8ca9-04c883e879e8
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b2ae23ac9cc1723936c4b4e1900b95cdcde744df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>Ändra på och av beteendet för Indikatorn
## <a name="what-you-will-do"></a>Vad du ska göra
Anpassa meddelandena som du vill ändra Indikatorns och inaktivera beteende. Om du har några problem, söka efter lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
Använd ytterligare Node.js-funktioner för att ändra Indikatorns och inaktivera beteende.

## <a name="what-you-need"></a>Vad du behöver
Du måste ha slutfört [kör ett exempelprogram på hallon Pi ta emot meddelanden moln till enhet](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).

## <a name="add-nodejs-functions"></a>Lägga till funktioner för Node.js
1. Öppna exempelprogrammet i Visual Studio-koden genom att köra följande kommandon:
   
   ```bash
   cd Lesson4
   code .
   ```
2. Öppna den `app.js` filen och Lägg sedan till följande funktioner i slutet:
   
   ```javascript
   function turnOnLED() {
     wpi.digitalWrite(CONFIG_PIN, 1);
   }
   
   function turnOffLED() {
     wpi.digitalWrite(CONFIG_PIN, 0);
   }
   ```
   
   ![filen App.js med tillagda funktioner](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)
3. Lägg till följande villkor innan standardversionen i blocket switch-fall av den `receiveMessageCallback` funktionen:
   
   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
   ```
   
   Du har nu konfigurerat exempelprogrammet att svara på flera instruktioner via meddelanden. ”On”-instruktion, aktiveras Indikatorn och instruktionen ”off” stänger av Indikatorn.
4. Öppna filen gulpfile.js och Lägg sedan till en ny funktion innan funktionen `sendMessage`:
   
   ```javascript
   var buildCustomMessage = function (messageId) {
     if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
       return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
     } else if (messageId < MAX_MESSAGE_COUNT) {
       return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
     } else {
       return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
     }
   }
   ```
   
   ![Gulpfile.js fil med tillagda funktion](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)
5. I den `sendMessage` fungerar genom att ersätta raden `var message = buildMessage(sentMessageCount);` med den nya raden som visas i följande utdrag:
   
   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Spara alla ändringar.

### <a name="deploy-and-run-the-sample-application"></a>Distribuera och köra exempelprogrammet
Distribuera och köra exempelprogrammet på Pi genom att köra följande kommando:

```bash
gulp deploy && gulp run
```

Du bör se Indikator aktivera i två sekunder och sedan stänga av en annan två sekunder. Det sista meddelandet ”stoppa” stoppar exempelprogrammet från att köras.

![Exempelprogrammet med och inaktivera meddelanden](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Grattis! Meddelanden som skickas till Pi från IoT-hubben har anpassats.

### <a name="summary"></a>Sammanfattning
Det här valfria avsnittet visar hur du anpassar meddelanden så att exempelprogrammet kan styra den på och av beteendet för Indikatorn på olika sätt.

