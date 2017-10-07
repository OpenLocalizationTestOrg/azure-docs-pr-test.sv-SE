---
title: "Connect Arduino (C) tooAzure IoT - lektionen 4: ändra app | Microsoft Docs"
description: "Anpassa hello meddelanden toochange hello Indikator är på och av beteende."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: kontrollen ledde med arduino
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: d7a25430-450e-43c4-a3ed-1eed995b8b7e
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8cc438650f01ae4335d91c94df6a29e0ffbdc508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a>Ändra hello och inaktivera beteendet för hello Indikator
## <a name="what-you-will-do"></a>Vad du ska göra
Anpassa hello meddelanden toochange hello Indikator är på och av beteende. Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) för Adafruit ludd M0 WiFi Arduino-skiva.

## <a name="what-you-will-learn"></a>Vad får du lära dig
Använd funktioner toochange av ytterligare Arduino-hello som Indikator är på och av beteende.

## <a name="what-you-need"></a>Vad du behöver
Du måste ha slutfört [kör ett exempelprogram i ditt Arduino board tooreceive moln toodevice meddelanden][receive-cloud-to-device-messages].

## <a name="add-functions-toomainc-and-gulpfilejs"></a>Lägg till funktioner toomain.c och gulpfile.js
1. Öppna hello exempelprogrammet i Visual Studio-koden genom att köra följande kommandon hello:

   ```bash
   cd Lesson4
   code .
   ```
2. Öppna hello `app.ino` filen och Lägg sedan till följande funktioner efter blinkLED() funktionen hello:

   ```arduino
   static void turnOnLED()
   {
     digitalWrite(LED_PIN, HIGH);
   }

   static void turnOffLED()
   {
     digitalWrite(LED_PIN, LOW);
   }
   ```

   ![App.ino fil med tillagda funktioner][app-ino-file]
3. Lägg till följande villkor innan hello hello `else if` block med hello `receiveMessageCallback` funktionen:

   ```arduino
   else if (strcmp((const char*)value, "\"on\"") == 0)
   {
       turnOnLED();
   }
   else if (0 == strcmp((const char*)value, "\"off\"") == 0)
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
   };
   ```

   ![Gulpfile.js fil med tillagda funktion][gulp-file-js]
5. I hello `sendMessage` fungerar genom att ersätta hello rad `var message = buildMessage(sentMessageCount);` med hello ny rad som visas i följande fragment hello:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Spara alla ändringar som hello.

### <a name="deploy-and-run-hello-sample-application"></a>Distribuera och köra hello exempelprogrammet
Distribuera och köra hello exempelprogrammet på Arduino-kort genom att köra följande kommando hello:

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

Du bör se hello Indikator aktivera i två sekunder och sedan stänga av en annan två sekunder. senaste ”stoppa” hälsningsmeddelande stoppar hello exempelprogrammet från att köras.

![Aktivera och inaktivera][on-and-off]

Grattis! Du har har anpassat hello-meddelanden som skickas tooyour Arduino board från din IoT-hubb.

### <a name="summary"></a>Sammanfattning
Det här valfria avsnittet visar hur toocustomize meddelanden så att hello exempelprogrammet kan styra hello och inaktivera beteendet för hello Indikator på olika sätt.

<!-- Images and links -->

[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[app-ino-file]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_app_ino.png
[gulp-file-js]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_gulpfile_js.png
[on-and-off]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_on_and_off_arduino.png