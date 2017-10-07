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
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a><span data-ttu-id="2591d-104">Ändra hello och inaktivera beteendet för hello Indikator</span><span class="sxs-lookup"><span data-stu-id="2591d-104">Change hello on and off behavior of hello LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="2591d-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="2591d-105">What you will do</span></span>
<span data-ttu-id="2591d-106">Anpassa hello meddelanden toochange hello Indikator är på och av beteende.</span><span class="sxs-lookup"><span data-stu-id="2591d-106">Customize hello messages toochange hello LED’s on and off behavior.</span></span> <span data-ttu-id="2591d-107">Om du har några problem med sökning lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="2591d-107">If you have any problems, seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="2591d-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="2591d-108">What you will learn</span></span>
<span data-ttu-id="2591d-109">Använd funktioner toochange av ytterligare Node.js-hello som Indikator är på och av beteende.</span><span class="sxs-lookup"><span data-stu-id="2591d-109">Use additional Node.js functions toochange hello LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="2591d-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="2591d-110">What you need</span></span>
<span data-ttu-id="2591d-111">Du måste ha slutfört [kör ett exempelprogram på hallon Pi tooreceive moln till enhet meddelanden](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).</span><span class="sxs-lookup"><span data-stu-id="2591d-111">You must have successfully completed [Run a sample application on Raspberry Pi tooreceive cloud-to-device messages](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).</span></span>

## <a name="add-nodejs-functions"></a><span data-ttu-id="2591d-112">Lägga till funktioner för Node.js</span><span class="sxs-lookup"><span data-stu-id="2591d-112">Add Node.js functions</span></span>
1. <span data-ttu-id="2591d-113">Öppna hello exempelprogrammet i Visual Studio-koden genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="2591d-113">Open hello sample application in Visual Studio code by running hello following commands:</span></span>
   
   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="2591d-114">Öppna hello `app.js` filen och Lägg sedan till följande funktioner i slutet av hello hello:</span><span class="sxs-lookup"><span data-stu-id="2591d-114">Open hello `app.js` file, and then add hello following functions at hello end:</span></span>
   
   ```javascript
   function turnOnLED() {
     wpi.digitalWrite(CONFIG_PIN, 1);
   }
   
   function turnOffLED() {
     wpi.digitalWrite(CONFIG_PIN, 0);
   }
   ```
   
   ![filen App.js med tillagda funktioner](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)
3. <span data-ttu-id="2591d-116">Lägg till följande villkor innan hello standardversionen i hello switch-fall block i hello hello `receiveMessageCallback` funktionen:</span><span class="sxs-lookup"><span data-stu-id="2591d-116">Add hello following conditions before hello default one in hello switch-case block of hello `receiveMessageCallback` function:</span></span>
   
   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
   ```
   
   <span data-ttu-id="2591d-117">Du har nu konfigurerat hello exempel programmet toorespond toomore instruktioner via meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2591d-117">Now you’ve configured hello sample application toorespond toomore instructions through messages.</span></span> <span data-ttu-id="2591d-118">Hej ”on” instruktion aktiverar hello Indikator och hello ”off” instruktion inaktiverar hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="2591d-118">hello "on" instruction turns on hello LED, and hello "off" instruction turns off hello LED.</span></span>
4. <span data-ttu-id="2591d-119">Öppna hello gulpfile.js filen och Lägg sedan till en ny funktion innan hello funktionen `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="2591d-119">Open hello gulpfile.js file, and then add a new function before hello function `sendMessage`:</span></span>
   
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
5. <span data-ttu-id="2591d-121">I hello `sendMessage` fungerar genom att ersätta hello rad `var message = buildMessage(sentMessageCount);` med hello ny rad som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="2591d-121">In hello `sendMessage` function, replace hello line `var message = buildMessage(sentMessageCount);` with hello new line shown in hello following snippet:</span></span>
   
   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="2591d-122">Spara alla ändringar som hello.</span><span class="sxs-lookup"><span data-stu-id="2591d-122">Save all hello changes.</span></span>

### <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="2591d-123">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="2591d-123">Deploy and run hello sample application</span></span>
<span data-ttu-id="2591d-124">Distribuera och köra hello exempelprogrammet på Pi genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="2591d-124">Deploy and run hello sample application on Pi by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="2591d-125">Du bör se hello Indikator aktivera i två sekunder och sedan stänga av en annan två sekunder.</span><span class="sxs-lookup"><span data-stu-id="2591d-125">You should see hello LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="2591d-126">senaste ”stoppa” hälsningsmeddelande stoppar hello exempelprogrammet från att köras.</span><span class="sxs-lookup"><span data-stu-id="2591d-126">hello last "stop" message stops hello sample application from running.</span></span>

![Exempelprogrammet med och inaktivera meddelanden](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

<span data-ttu-id="2591d-128">Grattis!</span><span class="sxs-lookup"><span data-stu-id="2591d-128">Congratulations!</span></span> <span data-ttu-id="2591d-129">Du har har anpassat hello-meddelanden som skickas tooPi från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2591d-129">You’ve successfully customized hello messages that are sent tooPi from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="2591d-130">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="2591d-130">Summary</span></span>
<span data-ttu-id="2591d-131">Det här valfria avsnittet visar hur toocustomize meddelanden så att hello exempelprogrammet kan styra hello och inaktivera beteendet för hello Indikator på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="2591d-131">This optional section demonstrates how toocustomize messages so that hello sample application can control hello on and off behavior of hello LED in a different way.</span></span>

