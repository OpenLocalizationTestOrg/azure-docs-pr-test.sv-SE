---
title: "Ansluta hallon Pi (nod) tooAzure IoT - lektionen 4: ändra app | Microsoft Docs"
description: "Anpassa hello meddelanden toochange hello Indikator är på och av beteende."
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
ms.openlocfilehash: 99b542fcb8639add0f5a0f7a49dd8abd0e224a51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a>Ändra hello och inaktivera beteendet för hello Indikator
## <a name="what-you-will-do"></a>Vad du ska göra
Anpassa hello meddelanden toochange hello Indikator är på och av beteende. Om du har några problem med sökning lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
Använd funktioner toochange av ytterligare Node.js-hello som Indikator är på och av beteende.

## <a name="what-you-need"></a>Vad du behöver
Du måste ha slutfört [kör ett exempelprogram på hallon Pi tooreceive moln till enhet meddelanden](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).

## <a name="add-nodejs-functions"></a>Lägga till funktioner för Node.js
1. Öppna hello exempelprogrammet i Visual Studio-koden genom att köra följande kommandon hello:
   
   ```bash
   cd Lesson4
   code .
   ```
2. Öppna hello `app.js` filen och Lägg sedan till följande funktioner i slutet av hello hello:
   
   ```javascript
   function turnOnLED() {
     wpi.digitalWrite(CONFIG_PIN, 1);
   }
   
   function turnOffLED() {
     wpi.digitalWrite(CONFIG_PIN, 0);
   }
   ```
   
   ![filen App.js med tillagda funktioner](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)
3. Lägg till följande villkor innan hello standardversionen i hello switch-fall block i hello hello `receiveMessageCallback` funktionen:
   
   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
   ```
   
   Du har nu konfigurerat hello exempel programmet toorespond toomore instruktioner via meddelanden. Hej ”on” instruktion aktiverar hello Indikator och hello ”off” instruktion inaktiverar hello Indikator.
4. Öppna hello gulpfile.js filen och Lägg sedan till en ny funktion innan hello funktionen `sendMessage`:
   
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
5. I hello `sendMessage` fungerar genom att ersätta hello rad `var message = buildMessage(sentMessageCount);` med hello ny rad som visas i följande fragment hello:
   
   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Spara alla ändringar som hello.

### <a name="deploy-and-run-hello-sample-application"></a>Distribuera och köra hello exempelprogrammet
Distribuera och köra hello exempelprogrammet på Pi genom att köra följande kommando hello:

```bash
gulp deploy && gulp run
```

Du bör se hello Indikator aktivera i två sekunder och sedan stänga av en annan två sekunder. senaste ”stoppa” hälsningsmeddelande stoppar hello exempelprogrammet från att köras.

![Exempelprogrammet med och inaktivera meddelanden](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Grattis! Du har har anpassat hello-meddelanden som skickas tooPi från IoT-hubb.

### <a name="summary"></a>Sammanfattning
Det här valfria avsnittet visar hur toocustomize meddelanden så att hello exempelprogrammet kan styra hello och inaktivera beteendet för hello Indikator på olika sätt.

