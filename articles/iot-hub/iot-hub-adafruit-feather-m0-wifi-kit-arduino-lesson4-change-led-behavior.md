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
# <a name="change-the-on-and-off-behavior-of-the-led"></a><span data-ttu-id="7e3e7-104">Ändra på och av beteendet för Indikatorn</span><span class="sxs-lookup"><span data-stu-id="7e3e7-104">Change the on and off behavior of the LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="7e3e7-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="7e3e7-105">What you will do</span></span>
<span data-ttu-id="7e3e7-106">Anpassa meddelandena som du vill ändra Indikatorns och inaktivera beteende.</span><span class="sxs-lookup"><span data-stu-id="7e3e7-106">Customize the messages to change the LED’s on and off behavior.</span></span> <span data-ttu-id="7e3e7-107">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) för Adafruit ludd M0 WiFi Arduino-skiva.</span><span class="sxs-lookup"><span data-stu-id="7e3e7-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="7e3e7-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="7e3e7-108">What you will learn</span></span>
<span data-ttu-id="7e3e7-109">Använd ytterligare Arduino funktioner för att ändra Indikatorns och inaktivera beteende.</span><span class="sxs-lookup"><span data-stu-id="7e3e7-109">Use additional Arduino functions to change the LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="7e3e7-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="7e3e7-110">What you need</span></span>
<span data-ttu-id="7e3e7-111">Du måste ha slutfört [kör ett exempelprogram på Arduino-kort för att ta emot moln till enhet meddelanden][receive-cloud-to-device-messages].</span><span class="sxs-lookup"><span data-stu-id="7e3e7-111">You must have successfully completed [Run a sample application on your Arduino board to receive cloud to device messages][receive-cloud-to-device-messages].</span></span>

## <a name="add-functions-to-mainc-and-gulpfilejs"></a><span data-ttu-id="7e3e7-112">Lägga till funktioner i main.c och gulpfile.js</span><span class="sxs-lookup"><span data-stu-id="7e3e7-112">Add functions to main.c and gulpfile.js</span></span>
1. <span data-ttu-id="7e3e7-113">Öppna exempelprogrammet i Visual Studio-koden genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="7e3e7-113">Open the sample application in Visual Studio code by running the following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="7e3e7-114">Öppna den `app.ino` filen och Lägg sedan till följande funktioner efter blinkLED() funktionen:</span><span class="sxs-lookup"><span data-stu-id="7e3e7-114">Open the `app.ino` file, and then add the following functions after blinkLED() function:</span></span>

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
3. <span data-ttu-id="7e3e7-116">Lägg till följande villkor innan de `else if` blockera av den `receiveMessageCallback` funktionen:</span><span class="sxs-lookup"><span data-stu-id="7e3e7-116">Add the following conditions before the `else if` block of the `receiveMessageCallback` function:</span></span>

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

   <span data-ttu-id="7e3e7-117">Du har nu konfigurerat exempelprogrammet att svara på flera instruktioner via meddelanden.</span><span class="sxs-lookup"><span data-stu-id="7e3e7-117">Now you’ve configured the sample application to respond to more instructions through messages.</span></span> <span data-ttu-id="7e3e7-118">”On”-instruktion, aktiveras Indikatorn och instruktionen ”off” stänger av Indikatorn.</span><span class="sxs-lookup"><span data-stu-id="7e3e7-118">The "on" instruction turns on the LED, and the "off" instruction turns off the LED.</span></span>
4. <span data-ttu-id="7e3e7-119">Öppna filen gulpfile.js och Lägg sedan till en ny funktion innan funktionen `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="7e3e7-119">Open the gulpfile.js file, and then add a new function before the function `sendMessage`:</span></span>

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
5. <span data-ttu-id="7e3e7-121">I den `sendMessage` fungerar genom att ersätta raden `var message = buildMessage(sentMessageCount);` med den nya raden som visas i följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="7e3e7-121">In the `sendMessage` function, replace the line `var message = buildMessage(sentMessageCount);` with the new line shown in the following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="7e3e7-122">Spara alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="7e3e7-122">Save all the changes.</span></span>

### <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="7e3e7-123">Distribuera och köra exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="7e3e7-123">Deploy and run the sample application</span></span>
<span data-ttu-id="7e3e7-124">Distribuera och köra exempelprogrammet på Arduino-kort genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7e3e7-124">Deploy and run the sample application on your Arduino board by running the following command:</span></span>

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

<span data-ttu-id="7e3e7-125">Du bör se Indikator aktivera i två sekunder och sedan stänga av en annan två sekunder.</span><span class="sxs-lookup"><span data-stu-id="7e3e7-125">You should see the LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="7e3e7-126">Det sista meddelandet ”stoppa” stoppar exempelprogrammet från att köras.</span><span class="sxs-lookup"><span data-stu-id="7e3e7-126">The last "stop" message stops the sample application from running.</span></span>

![Aktivera och inaktivera][on-and-off]

<span data-ttu-id="7e3e7-128">Grattis!</span><span class="sxs-lookup"><span data-stu-id="7e3e7-128">Congratulations!</span></span> <span data-ttu-id="7e3e7-129">Meddelanden som skickas till Arduino-kort från IoT-hubben har anpassats.</span><span class="sxs-lookup"><span data-stu-id="7e3e7-129">You’ve successfully customized the messages that are sent to your Arduino board from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="7e3e7-130">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7e3e7-130">Summary</span></span>
<span data-ttu-id="7e3e7-131">Det här valfria avsnittet visar hur du anpassar meddelanden så att exempelprogrammet kan styra den på och av beteendet för Indikatorn på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="7e3e7-131">This optional section demonstrates how to customize messages so that the sample application can control the on and off behavior of the LED in a different way.</span></span>

<!-- Images and links -->

[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[app-ino-file]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_app_ino.png
[gulp-file-js]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_gulpfile_js.png
[on-and-off]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_on_and_off_arduino.png