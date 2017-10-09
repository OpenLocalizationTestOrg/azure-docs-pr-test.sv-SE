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
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a><span data-ttu-id="46181-104">Ändra hello och inaktivera beteendet för hello Indikator</span><span class="sxs-lookup"><span data-stu-id="46181-104">Change hello on and off behavior of hello LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="46181-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="46181-105">What you will do</span></span>
<span data-ttu-id="46181-106">Anpassa hello meddelanden toochange hello Indikator är på och av beteende.</span><span class="sxs-lookup"><span data-stu-id="46181-106">Customize hello messages toochange hello LED’s on and off behavior.</span></span> <span data-ttu-id="46181-107">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="46181-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="46181-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="46181-108">What you will learn</span></span>
<span data-ttu-id="46181-109">Använda ytterligare funktioner toochange hello Indikator är på och av beteende.</span><span class="sxs-lookup"><span data-stu-id="46181-109">Use additional functions toochange hello LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="46181-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="46181-110">What you need</span></span>
<span data-ttu-id="46181-111">Du måste ha slutfört [kör ett exempelprogram på Intel modern tooreceive moln toodevice meddelanden][receive-cloud-to-device-messages].</span><span class="sxs-lookup"><span data-stu-id="46181-111">You must have successfully completed [Run a sample application on Intel Edison tooreceive cloud toodevice messages][receive-cloud-to-device-messages].</span></span>

## <a name="add-functions-tooappjs-and-gulpfilejs"></a><span data-ttu-id="46181-112">Lägg till funktioner tooapp.js och gulpfile.js</span><span class="sxs-lookup"><span data-stu-id="46181-112">Add functions tooapp.js and gulpfile.js</span></span>
1. <span data-ttu-id="46181-113">Öppna hello exempelprogrammet i Visual Studio-koden genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="46181-113">Open hello sample application in Visual Studio code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="46181-114">Öppna hello `app.js` filen och Lägg sedan till följande funktioner efter blinkLED() funktionen hello:</span><span class="sxs-lookup"><span data-stu-id="46181-114">Open hello `app.js` file, and then add hello following functions after blinkLED() function:</span></span>

   ```javascript
   function turnOnLED() {
     myLed.write(1);
   }

   function turnOffLED() {
     myLed.write(0);
   }
   ```

   ![filen App.js med tillagda funktioner](media/iot-hub-intel-edison-lessons/lesson4/updated_app_node.png)
3. <span data-ttu-id="46181-116">Lägg till hello följande villkor innan hello 'blinkar' skiftläget för hello switch-fall block med hello `receiveMessageCallback` funktionen:</span><span class="sxs-lookup"><span data-stu-id="46181-116">Add hello following conditions before hello 'blink' case in hello switch-case block of hello `receiveMessageCallback` function:</span></span>

   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
   ```

   <span data-ttu-id="46181-117">Du har nu konfigurerat hello exempel programmet toorespond toomore instruktioner via meddelanden.</span><span class="sxs-lookup"><span data-stu-id="46181-117">Now you’ve configured hello sample application toorespond toomore instructions through messages.</span></span> <span data-ttu-id="46181-118">Hej ”on” instruktion aktiverar hello Indikator och hello ”off” instruktion inaktiverar hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="46181-118">hello "on" instruction turns on hello LED, and hello "off" instruction turns off hello LED.</span></span>
4. <span data-ttu-id="46181-119">Öppna hello gulpfile.js filen och Lägg sedan till en ny funktion innan hello funktionen `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="46181-119">Open hello gulpfile.js file, and then add a new function before hello function `sendMessage`:</span></span>

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
5. <span data-ttu-id="46181-121">I hello `sendMessage` fungerar genom att ersätta hello rad `var message = buildMessage(sentMessageCount);` med hello ny rad som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="46181-121">In hello `sendMessage` function, replace hello line `var message = buildMessage(sentMessageCount);` with hello new line shown in hello following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="46181-122">Spara alla ändringar som hello.</span><span class="sxs-lookup"><span data-stu-id="46181-122">Save all hello changes.</span></span>

### <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="46181-123">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="46181-123">Deploy and run hello sample application</span></span>
<span data-ttu-id="46181-124">Distribuera och köra hello exempelprogrammet på modern genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="46181-124">Deploy and run hello sample application on Edison by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="46181-125">Du bör se hello Indikator aktivera i två sekunder och sedan stänga av en annan två sekunder.</span><span class="sxs-lookup"><span data-stu-id="46181-125">You should see hello LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="46181-126">senaste ”stoppa” hälsningsmeddelande stoppar hello exempelprogrammet från att köras.</span><span class="sxs-lookup"><span data-stu-id="46181-126">hello last "stop" message stops hello sample application from running.</span></span>

![Aktivera och inaktivera][on-and-off]

<span data-ttu-id="46181-128">Grattis!</span><span class="sxs-lookup"><span data-stu-id="46181-128">Congratulations!</span></span> <span data-ttu-id="46181-129">Du har har anpassat hello-meddelanden som skickas tooEdison från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="46181-129">You’ve successfully customized hello messages that are sent tooEdison from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="46181-130">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="46181-130">Summary</span></span>
<span data-ttu-id="46181-131">Det här valfria avsnittet visar hur toocustomize meddelanden så att hello exempelprogrammet kan styra hello och inaktivera beteendet för hello Indikator på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="46181-131">This optional section demonstrates how toocustomize messages so that hello sample application can control hello on and off behavior of hello LED in a different way.</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-node-lesson4-send-cloud-to-device-messages.md
[gulpfile]: media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_node.png
[on-and-off]: media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_node.png
