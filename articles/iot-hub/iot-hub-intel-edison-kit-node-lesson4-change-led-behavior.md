---
title: "Ansluta Intel modern (nod) till Azure IoT - lektionen 4: blinkar på Indikator | Microsoft Docs"
description: "Anpassa meddelandena som du vill ändra Indikatorns och inaktivera beteende."
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
ms.openlocfilehash: fa99050dad62534e2825e93f1170d2f3ecf5a3ba
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-on-and-off-behavior-of-the-led"></a><span data-ttu-id="016be-104">Ändra på och av beteendet för Indikatorn</span><span class="sxs-lookup"><span data-stu-id="016be-104">Change the on and off behavior of the LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="016be-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="016be-105">What you will do</span></span>
<span data-ttu-id="016be-106">Anpassa meddelandena som du vill ändra Indikatorns och inaktivera beteende.</span><span class="sxs-lookup"><span data-stu-id="016be-106">Customize the messages to change the LED’s on and off behavior.</span></span> <span data-ttu-id="016be-107">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="016be-107">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="016be-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="016be-108">What you will learn</span></span>
<span data-ttu-id="016be-109">Använd ytterligare funktioner för att ändra Indikatorns och inaktivera beteende.</span><span class="sxs-lookup"><span data-stu-id="016be-109">Use additional functions to change the LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="016be-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="016be-110">What you need</span></span>
<span data-ttu-id="016be-111">Du måste ha slutfört [kör ett exempelprogram på Intel modern att ta emot moln till enhet meddelanden][receive-cloud-to-device-messages].</span><span class="sxs-lookup"><span data-stu-id="016be-111">You must have successfully completed [Run a sample application on Intel Edison to receive cloud to device messages][receive-cloud-to-device-messages].</span></span>

## <a name="add-functions-to-appjs-and-gulpfilejs"></a><span data-ttu-id="016be-112">Lägga till funktioner i app.js och gulpfile.js</span><span class="sxs-lookup"><span data-stu-id="016be-112">Add functions to app.js and gulpfile.js</span></span>
1. <span data-ttu-id="016be-113">Öppna exempelprogrammet i Visual Studio-koden genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="016be-113">Open the sample application in Visual Studio code by running the following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="016be-114">Öppna den `app.js` filen och Lägg sedan till följande funktioner efter blinkLED() funktionen:</span><span class="sxs-lookup"><span data-stu-id="016be-114">Open the `app.js` file, and then add the following functions after blinkLED() function:</span></span>

   ```javascript
   function turnOnLED() {
     myLed.write(1);
   }

   function turnOffLED() {
     myLed.write(0);
   }
   ```

   ![filen App.js med tillagda funktioner](media/iot-hub-intel-edison-lessons/lesson4/updated_app_node.png)
3. <span data-ttu-id="016be-116">Lägg till följande villkor innan 'blinka' fallet i switch-fall block i den `receiveMessageCallback` funktionen:</span><span class="sxs-lookup"><span data-stu-id="016be-116">Add the following conditions before the 'blink' case in the switch-case block of the `receiveMessageCallback` function:</span></span>

   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
   ```

   <span data-ttu-id="016be-117">Du har nu konfigurerat exempelprogrammet att svara på flera instruktioner via meddelanden.</span><span class="sxs-lookup"><span data-stu-id="016be-117">Now you’ve configured the sample application to respond to more instructions through messages.</span></span> <span data-ttu-id="016be-118">”On”-instruktion, aktiveras Indikatorn och instruktionen ”off” stänger av Indikatorn.</span><span class="sxs-lookup"><span data-stu-id="016be-118">The "on" instruction turns on the LED, and the "off" instruction turns off the LED.</span></span>
4. <span data-ttu-id="016be-119">Öppna filen gulpfile.js och Lägg sedan till en ny funktion innan funktionen `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="016be-119">Open the gulpfile.js file, and then add a new function before the function `sendMessage`:</span></span>

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
5. <span data-ttu-id="016be-121">I den `sendMessage` fungerar genom att ersätta raden `var message = buildMessage(sentMessageCount);` med den nya raden som visas i följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="016be-121">In the `sendMessage` function, replace the line `var message = buildMessage(sentMessageCount);` with the new line shown in the following snippet:</span></span>

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="016be-122">Spara alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="016be-122">Save all the changes.</span></span>

### <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="016be-123">Distribuera och köra exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="016be-123">Deploy and run the sample application</span></span>
<span data-ttu-id="016be-124">Distribuera och köra exempelprogrammet på modern genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="016be-124">Deploy and run the sample application on Edison by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="016be-125">Du bör se Indikator aktivera i två sekunder och sedan stänga av en annan två sekunder.</span><span class="sxs-lookup"><span data-stu-id="016be-125">You should see the LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="016be-126">Det sista meddelandet ”stoppa” stoppar exempelprogrammet från att köras.</span><span class="sxs-lookup"><span data-stu-id="016be-126">The last "stop" message stops the sample application from running.</span></span>

![Aktivera och inaktivera][on-and-off]

<span data-ttu-id="016be-128">Grattis!</span><span class="sxs-lookup"><span data-stu-id="016be-128">Congratulations!</span></span> <span data-ttu-id="016be-129">Meddelanden som skickas till modern från IoT-hubben har anpassats.</span><span class="sxs-lookup"><span data-stu-id="016be-129">You’ve successfully customized the messages that are sent to Edison from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="016be-130">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="016be-130">Summary</span></span>
<span data-ttu-id="016be-131">Det här valfria avsnittet visar hur du anpassar meddelanden så att exempelprogrammet kan styra den på och av beteendet för Indikatorn på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="016be-131">This optional section demonstrates how to customize messages so that the sample application can control the on and off behavior of the LED in a different way.</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-node-lesson4-send-cloud-to-device-messages.md
[gulpfile]: media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_node.png
[on-and-off]: media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_node.png
