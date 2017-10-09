---
title: 'Ansluta Intel modern (nod) tooAzure IoT - lektionen 4: blinkar hello Indikator | Microsoft Docs'
description: "Anpassa hello meddelanden toochange hello Indikator är på och av beteende."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: kontrollen ledde med arduino
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 387cd97e-b05e-43c4-b252-f68ad45d524a
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: caeabe311fd1698f298c6d2b4a203ecad80ef7df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a>Ändra hello och inaktivera beteendet för hello Indikator
## <a name="what-you-will-do"></a>Vad du ska göra
Anpassa hello meddelanden toochange hello Indikator är på och av beteende. Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
Använda ytterligare funktioner toochange hello Indikator är på och av beteende.

## <a name="what-you-need"></a>Vad du behöver
Du måste ha slutfört [kör ett exempelprogram på Intel modern tooreceive moln toodevice meddelanden][receive-cloud-to-device-messages].

## <a name="add-functions-tooappjs-and-gulpfilejs"></a>Lägg till funktioner tooapp.js och gulpfile.js
1. Öppna hello exempelprogrammet i Visual Studio-koden genom att köra följande kommandon hello:

   ```bash
   cd Lesson4
   code .
   ```
2. Öppna hello `app.js` filen och Lägg sedan till följande funktioner efter blinkLED() funktionen hello:

   ```javascript
   function turnOnLED() {
     myLed.write(1);
   }

   function turnOffLED() {
     myLed.write(0);
   }
   ```

   ![filen App.js med tillagda funktioner](media/iot-hub-intel-edison-lessons/lesson4/updated_app_node.png)
3. Lägg till hello följande villkor innan hello 'blinkar' skiftläget för hello switch-fall block med hello `receiveMessageCallback` funktionen:

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

   ![Gulpfile.js fil med tillagda funktion][gulpfile]
5. I hello `sendMessage` fungerar genom att ersätta hello rad `var message = buildMessage(sentMessageCount);` med hello ny rad som visas i följande fragment hello:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Spara alla ändringar som hello.

### <a name="deploy-and-run-hello-sample-application"></a>Distribuera och köra hello exempelprogrammet
Distribuera och köra hello exempelprogrammet på modern genom att köra följande kommando hello:

```bash
gulp deploy && gulp run
```

Du bör se hello Indikator aktivera i två sekunder och sedan stänga av en annan två sekunder. senaste ”stoppa” hälsningsmeddelande stoppar hello exempelprogrammet från att köras.

![Aktivera och inaktivera][on-and-off]

Grattis! Du har har anpassat hello-meddelanden som skickas tooEdison från IoT-hubb.

### <a name="summary"></a>Sammanfattning
Det här valfria avsnittet visar hur toocustomize meddelanden så att hello exempelprogrammet kan styra hello och inaktivera beteendet för hello Indikator på olika sätt.

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-node-lesson4-send-cloud-to-device-messages.md
[gulpfile]: media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_node.png
[on-and-off]: media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_node.png
