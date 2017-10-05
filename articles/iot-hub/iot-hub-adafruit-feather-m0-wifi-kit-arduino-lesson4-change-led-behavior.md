---
title: "Ansluta Arduino (C) till Azure IoT - lektionen 4: ändra app | Microsoft Docs"
description: "Anpassa meddelandena som du vill ändra Indikatorns och inaktivera beteende."
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
ms.openlocfilehash: 5009a0466f2c5689b8ab426049f4c4f02272512b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>Ändra på och av beteendet för Indikatorn
## <a name="what-you-will-do"></a>Vad du ska göra
Anpassa meddelandena som du vill ändra Indikatorns och inaktivera beteende. Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) för Adafruit ludd M0 WiFi Arduino-skiva.

## <a name="what-you-will-learn"></a>Vad får du lära dig
Använd ytterligare Arduino funktioner för att ändra Indikatorns och inaktivera beteende.

## <a name="what-you-need"></a>Vad du behöver
Du måste ha slutfört [kör ett exempelprogram på Arduino-kort för att ta emot moln till enhet meddelanden][receive-cloud-to-device-messages].

## <a name="add-functions-to-mainc-and-gulpfilejs"></a>Lägga till funktioner i main.c och gulpfile.js
1. Öppna exempelprogrammet i Visual Studio-koden genom att köra följande kommandon:

   ```bash
   cd Lesson4
   code .
   ```
2. Öppna den `app.ino` filen och Lägg sedan till följande funktioner efter blinkLED() funktionen:

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
3. Lägg till följande villkor innan de `else if` blockera av den `receiveMessageCallback` funktionen:

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
   };
   ```

   ![Gulpfile.js fil med tillagda funktion][gulp-file-js]
5. I den `sendMessage` fungerar genom att ersätta raden `var message = buildMessage(sentMessageCount);` med den nya raden som visas i följande utdrag:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Spara alla ändringar.

### <a name="deploy-and-run-the-sample-application"></a>Distribuera och köra exempelprogrammet
Distribuera och köra exempelprogrammet på Arduino-kort genom att köra följande kommando:

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

Du bör se Indikator aktivera i två sekunder och sedan stänga av en annan två sekunder. Det sista meddelandet ”stoppa” stoppar exempelprogrammet från att köras.

![Aktivera och inaktivera][on-and-off]

Grattis! Meddelanden som skickas till Arduino-kort från IoT-hubben har anpassats.

### <a name="summary"></a>Sammanfattning
Det här valfria avsnittet visar hur du anpassar meddelanden så att exempelprogrammet kan styra den på och av beteendet för Indikatorn på olika sätt.

<!-- Images and links -->

[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[app-ino-file]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_app_ino.png
[gulp-file-js]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_gulpfile_js.png
[on-and-off]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_on_and_off_arduino.png