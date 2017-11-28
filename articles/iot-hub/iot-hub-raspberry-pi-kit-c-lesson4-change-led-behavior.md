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
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a><span data-ttu-id="d90f5-104">Ändra hello och inaktivera beteendet för hello Indikator</span><span class="sxs-lookup"><span data-stu-id="d90f5-104">Change hello on and off behavior of hello LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="d90f5-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="d90f5-105">What you will do</span></span>
<span data-ttu-id="d90f5-106">Anpassa hello meddelanden toochange hello Indikator är på och av beteende.</span><span class="sxs-lookup"><span data-stu-id="d90f5-106">Customize hello messages toochange hello LED’s on and off behavior.</span></span> <span data-ttu-id="d90f5-107">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="d90f5-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="d90f5-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="d90f5-108">What you will learn</span></span>
<span data-ttu-id="d90f5-109">Använd funktioner toochange av ytterligare Node.js-hello som Indikator är på och av beteende.</span><span class="sxs-lookup"><span data-stu-id="d90f5-109">Use additional Node.js functions toochange hello LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="d90f5-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="d90f5-110">What you need</span></span>
<span data-ttu-id="d90f5-111">Du måste ha slutfört [kör ett exempelprogram på hallon Pi tooreceive moln toodevice meddelanden](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md).</span><span class="sxs-lookup"><span data-stu-id="d90f5-111">You must have successfully completed [Run a sample application on Raspberry Pi tooreceive cloud toodevice messages](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md).</span></span>

## <a name="add-functions-toomainc-and-gulpfilejs"></a><span data-ttu-id="d90f5-112">Lägg till funktioner toomain.c och gulpfile.js</span><span class="sxs-lookup"><span data-stu-id="d90f5-112">Add functions toomain.c and gulpfile.js</span></span>
1. <span data-ttu-id="d90f5-113">Öppna hello exempelprogrammet i Visual Studio-koden genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="d90f5-113">Open hello sample application in Visual Studio code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="d90f5-114">Öppna hello `main.c` filen och Lägg sedan till följande funktioner efter blinkLED() funktionen hello:</span><span class="sxs-lookup"><span data-stu-id="d90f5-114">Open hello `main.c` file, and then add hello following functions after blinkLED() function:</span></span>

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
3. <span data-ttu-id="d90f5-116">Lägg till följande villkor innan hello standardversionen i hello hello `if` block med hello `receiveMessageCallback` funktionen:</span><span class="sxs-lookup"><span data-stu-id="d90f5-116">Add hello following conditions before hello default one in hello `if` block of hello `receiveMessageCallback` function:</span></span>

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

   <span data-ttu-id="d90f5-117">Du har nu konfigurerat hello exempel programmet toorespond toomore instruktioner via meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d90f5-117">Now you’ve configured hello sample application toorespond toomore instructions through messages.</span></span> <span data-ttu-id="d90f5-118">Hej ”on” instruktion aktiverar hello Indikator och hello ”off” instruktion inaktiverar hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="d90f5-118">hello "on" instruction turns on hello LED, and hello "off" instruction turns off hello LED.</span></span>
4. <span data-ttu-id="d90f5-119">Öppna hello gulpfile.js filen och Lägg sedan till en ny funktion innan hello funktionen `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="d90f5-119">Open hello gulpfile.js file, and then add a new function before hello function `sendMessage`:</span></span>

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
5. <span data-ttu-id="d90f5-121">I hello `sendMessage` fungerar genom att ersätta hello rad `var message = buildMessage(sentMessageCount);` med hello ny rad som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="d90f5-121">In hello `sendMessage` function, replace hello line `var message = buildMessage(sentMessageCount);` with hello new line shown in hello following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="d90f5-122">Spara alla ändringar som hello.</span><span class="sxs-lookup"><span data-stu-id="d90f5-122">Save all hello changes.</span></span>

### <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="d90f5-123">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="d90f5-123">Deploy and run hello sample application</span></span>
<span data-ttu-id="d90f5-124">Distribuera och köra hello exempelprogrammet på Pi genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="d90f5-124">Deploy and run hello sample application on Pi by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="d90f5-125">Du bör se hello Indikator aktivera i två sekunder och sedan stänga av en annan två sekunder.</span><span class="sxs-lookup"><span data-stu-id="d90f5-125">You should see hello LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="d90f5-126">senaste ”stoppa” hälsningsmeddelande stoppar hello exempelprogrammet från att köras.</span><span class="sxs-lookup"><span data-stu-id="d90f5-126">hello last "stop" message stops hello sample application from running.</span></span>

![Exempelprogrammet med och inaktivera meddelanden](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off_c.png)

<span data-ttu-id="d90f5-128">Grattis!</span><span class="sxs-lookup"><span data-stu-id="d90f5-128">Congratulations!</span></span> <span data-ttu-id="d90f5-129">Du har har anpassat hello-meddelanden som skickas tooPi från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d90f5-129">You’ve successfully customized hello messages that are sent tooPi from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="d90f5-130">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d90f5-130">Summary</span></span>
<span data-ttu-id="d90f5-131">Det här valfria avsnittet visar hur toocustomize meddelanden så att hello exempelprogrammet kan styra hello och inaktivera beteendet för hello Indikator på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="d90f5-131">This optional section demonstrates how toocustomize messages so that hello sample application can control hello on and off behavior of hello LED in a different way.</span></span>
