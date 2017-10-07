---
title: "Connect Raspberry PI (C) tooAzure IoT - lektionen 4: ändra app | Microsoft Docs"
description: "Anpassa hello meddelanden toochange hello Indikator är på och av beteende."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: kontrollen ledde raspberry pi, raspberry pi ledde kontroll, raspberry pi kontroll ledde
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 0201b8ed-d5e6-4445-9a4d-1305003d1eff
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: f4739c4e9a58b4b0fe964b5c3c81e5918982099f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a>Ändra hello och inaktivera beteendet för hello Indikator
## <a name="what-you-will-do"></a>Vad du ska göra
Anpassa hello meddelanden toochange hello Indikator är på och av beteende. Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
Använd funktioner toochange av ytterligare Node.js-hello som Indikator är på och av beteende.

## <a name="what-you-need"></a>Vad du behöver
Du måste ha slutfört [kör ett exempelprogram på hallon Pi tooreceive moln toodevice meddelanden](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md).

## <a name="add-functions-toomainc-and-gulpfilejs"></a>Lägg till funktioner toomain.c och gulpfile.js
1. Öppna hello exempelprogrammet i Visual Studio-koden genom att köra följande kommandon hello:

   ```bash
   cd Lesson4
   code .
   ```
2. Öppna hello `main.c` filen och Lägg sedan till följande funktioner efter blinkLED() funktionen hello:

   ```c
   static void turnOnLED()
   {
     digitalWrite(LED_PIN, HIGH);
   }

   static void turnOffLED()
   {
     digitalWrite(LED_PIN, LOW);
   }
   ```

   ![main.c fil med tillagda funktioner](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_c.png)
3. Lägg till följande villkor innan hello standardversionen i hello hello `if` block med hello `receiveMessageCallback` funktionen:

   ```c
   else if (0 == strcmp((const char*)value, "\"on\""))
   {
       turnOnLED();
   }
   else if (0 == strcmp((const char*)value, "\"off\""))
   {
       turnOffLED();
   }
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

   ![Gulpfile.js fil med tillagda funktion](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile_c.png)
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

![Exempelprogrammet med och inaktivera meddelanden](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off_c.png)

Grattis! Du har har anpassat hello-meddelanden som skickas tooPi från IoT-hubb.

### <a name="summary"></a>Sammanfattning
Det här valfria avsnittet visar hur toocustomize meddelanden så att hello exempelprogrammet kan styra hello och inaktivera beteendet för hello Indikator på olika sätt.
