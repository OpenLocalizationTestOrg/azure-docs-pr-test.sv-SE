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
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a><span data-ttu-id="51465-104">Ändra hello och inaktivera beteendet för hello Indikator</span><span class="sxs-lookup"><span data-stu-id="51465-104">Change hello on and off behavior of hello LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="51465-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="51465-105">What you will do</span></span>
<span data-ttu-id="51465-106">Anpassa hello meddelanden toochange hello Indikator är på och av beteende.</span><span class="sxs-lookup"><span data-stu-id="51465-106">Customize hello messages toochange hello LED’s on and off behavior.</span></span> <span data-ttu-id="51465-107">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) för Adafruit ludd M0 WiFi Arduino-skiva.</span><span class="sxs-lookup"><span data-stu-id="51465-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="51465-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="51465-108">What you will learn</span></span>
<span data-ttu-id="51465-109">Använd funktioner toochange av ytterligare Arduino-hello som Indikator är på och av beteende.</span><span class="sxs-lookup"><span data-stu-id="51465-109">Use additional Arduino functions toochange hello LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="51465-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="51465-110">What you need</span></span>
<span data-ttu-id="51465-111">Du måste ha slutfört [kör ett exempelprogram i ditt Arduino board tooreceive moln toodevice meddelanden][receive-cloud-to-device-messages].</span><span class="sxs-lookup"><span data-stu-id="51465-111">You must have successfully completed [Run a sample application on your Arduino board tooreceive cloud toodevice messages][receive-cloud-to-device-messages].</span></span>

## <a name="add-functions-toomainc-and-gulpfilejs"></a><span data-ttu-id="51465-112">Lägg till funktioner toomain.c och gulpfile.js</span><span class="sxs-lookup"><span data-stu-id="51465-112">Add functions toomain.c and gulpfile.js</span></span>
1. <span data-ttu-id="51465-113">Öppna hello exempelprogrammet i Visual Studio-koden genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="51465-113">Open hello sample application in Visual Studio code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="51465-114">Öppna hello `app.ino` filen och Lägg sedan till följande funktioner efter blinkLED() funktionen hello:</span><span class="sxs-lookup"><span data-stu-id="51465-114">Open hello `app.ino` file, and then add hello following functions after blinkLED() function:</span></span>

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
3. <span data-ttu-id="51465-116">Lägg till följande villkor innan hello hello `else if` block med hello `receiveMessageCallback` funktionen:</span><span class="sxs-lookup"><span data-stu-id="51465-116">Add hello following conditions before hello `else if` block of hello `receiveMessageCallback` function:</span></span>

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

   <span data-ttu-id="51465-117">Du har nu konfigurerat hello exempel programmet toorespond toomore instruktioner via meddelanden.</span><span class="sxs-lookup"><span data-stu-id="51465-117">Now you’ve configured hello sample application toorespond toomore instructions through messages.</span></span> <span data-ttu-id="51465-118">Hej ”on” instruktion aktiverar hello Indikator och hello ”off” instruktion inaktiverar hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="51465-118">hello "on" instruction turns on hello LED, and hello "off" instruction turns off hello LED.</span></span>
4. <span data-ttu-id="51465-119">Öppna hello gulpfile.js filen och Lägg sedan till en ny funktion innan hello funktionen `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="51465-119">Open hello gulpfile.js file, and then add a new function before hello function `sendMessage`:</span></span>

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
5. <span data-ttu-id="51465-121">I hello `sendMessage` fungerar genom att ersätta hello rad `var message = buildMessage(sentMessageCount);` med hello ny rad som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="51465-121">In hello `sendMessage` function, replace hello line `var message = buildMessage(sentMessageCount);` with hello new line shown in hello following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="51465-122">Spara alla ändringar som hello.</span><span class="sxs-lookup"><span data-stu-id="51465-122">Save all hello changes.</span></span>

### <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="51465-123">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="51465-123">Deploy and run hello sample application</span></span>
<span data-ttu-id="51465-124">Distribuera och köra hello exempelprogrammet på Arduino-kort genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="51465-124">Deploy and run hello sample application on your Arduino board by running hello following command:</span></span>

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

<span data-ttu-id="51465-125">Du bör se hello Indikator aktivera i två sekunder och sedan stänga av en annan två sekunder.</span><span class="sxs-lookup"><span data-stu-id="51465-125">You should see hello LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="51465-126">senaste ”stoppa” hälsningsmeddelande stoppar hello exempelprogrammet från att köras.</span><span class="sxs-lookup"><span data-stu-id="51465-126">hello last "stop" message stops hello sample application from running.</span></span>

![Aktivera och inaktivera][on-and-off]

<span data-ttu-id="51465-128">Grattis!</span><span class="sxs-lookup"><span data-stu-id="51465-128">Congratulations!</span></span> <span data-ttu-id="51465-129">Du har har anpassat hello-meddelanden som skickas tooyour Arduino board från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="51465-129">You’ve successfully customized hello messages that are sent tooyour Arduino board from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="51465-130">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="51465-130">Summary</span></span>
<span data-ttu-id="51465-131">Det här valfria avsnittet visar hur toocustomize meddelanden så att hello exempelprogrammet kan styra hello och inaktivera beteendet för hello Indikator på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="51465-131">This optional section demonstrates how toocustomize messages so that hello sample application can control hello on and off behavior of hello LED in a different way.</span></span>

<!-- Images and links -->

[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[app-ino-file]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_app_ino.png
[gulp-file-js]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_gulpfile_js.png
[on-and-off]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_on_and_off_arduino.png