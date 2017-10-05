---
title: "Ansluta Raspberry Pi (nod) till Azure IoT - lektionen 4: ändra app | Microsoft Docs"
description: "Anpassa meddelandena som du vill ändra Indikatorns och inaktivera beteende."
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
ms.openlocfilehash: b2ae23ac9cc1723936c4b4e1900b95cdcde744df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-on-and-off-behavior-of-the-led"></a><span data-ttu-id="f0bf8-104">Ändra på och av beteendet för Indikatorn</span><span class="sxs-lookup"><span data-stu-id="f0bf8-104">Change the on and off behavior of the LED</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="f0bf8-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="f0bf8-105">What you will do</span></span>
<span data-ttu-id="f0bf8-106">Anpassa meddelandena som du vill ändra Indikatorns och inaktivera beteende.</span><span class="sxs-lookup"><span data-stu-id="f0bf8-106">Customize the messages to change the LED’s on and off behavior.</span></span> <span data-ttu-id="f0bf8-107">Om du har några problem, söka efter lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f0bf8-107">If you have any problems, seek solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f0bf8-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="f0bf8-108">What you will learn</span></span>
<span data-ttu-id="f0bf8-109">Använd ytterligare Node.js-funktioner för att ändra Indikatorns och inaktivera beteende.</span><span class="sxs-lookup"><span data-stu-id="f0bf8-109">Use additional Node.js functions to change the LED’s on and off behavior.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f0bf8-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="f0bf8-110">What you need</span></span>
<span data-ttu-id="f0bf8-111">Du måste ha slutfört [kör ett exempelprogram på hallon Pi ta emot meddelanden moln till enhet](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).</span><span class="sxs-lookup"><span data-stu-id="f0bf8-111">You must have successfully completed [Run a sample application on Raspberry Pi to receive cloud-to-device messages](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).</span></span>

## <a name="add-nodejs-functions"></a><span data-ttu-id="f0bf8-112">Lägga till funktioner för Node.js</span><span class="sxs-lookup"><span data-stu-id="f0bf8-112">Add Node.js functions</span></span>
1. <span data-ttu-id="f0bf8-113">Öppna exempelprogrammet i Visual Studio-koden genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="f0bf8-113">Open the sample application in Visual Studio code by running the following commands:</span></span>
   
   ```bash
   cd Lesson4
   code .
   ```
2. <span data-ttu-id="f0bf8-114">Öppna den `app.js` filen och Lägg sedan till följande funktioner i slutet:</span><span class="sxs-lookup"><span data-stu-id="f0bf8-114">Open the `app.js` file, and then add the following functions at the end:</span></span>
   
   ```javascript
   function turnOnLED() {
     wpi.digitalWrite(CONFIG_PIN, 1);
   }
   
   function turnOffLED() {
     wpi.digitalWrite(CONFIG_PIN, 0);
   }
   ```
   
   ![filen App.js med tillagda funktioner](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)
3. <span data-ttu-id="f0bf8-116">Lägg till följande villkor innan standardversionen i blocket switch-fall av den `receiveMessageCallback` funktionen:</span><span class="sxs-lookup"><span data-stu-id="f0bf8-116">Add the following conditions before the default one in the switch-case block of the `receiveMessageCallback` function:</span></span>
   
   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
   ```
   
   <span data-ttu-id="f0bf8-117">Du har nu konfigurerat exempelprogrammet att svara på flera instruktioner via meddelanden.</span><span class="sxs-lookup"><span data-stu-id="f0bf8-117">Now you’ve configured the sample application to respond to more instructions through messages.</span></span> <span data-ttu-id="f0bf8-118">”On”-instruktion, aktiveras Indikatorn och instruktionen ”off” stänger av Indikatorn.</span><span class="sxs-lookup"><span data-stu-id="f0bf8-118">The "on" instruction turns on the LED, and the "off" instruction turns off the LED.</span></span>
4. <span data-ttu-id="f0bf8-119">Öppna filen gulpfile.js och Lägg sedan till en ny funktion innan funktionen `sendMessage`:</span><span class="sxs-lookup"><span data-stu-id="f0bf8-119">Open the gulpfile.js file, and then add a new function before the function `sendMessage`:</span></span>
   
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
5. <span data-ttu-id="f0bf8-121">I den `sendMessage` fungerar genom att ersätta raden `var message = buildMessage(sentMessageCount);` med den nya raden som visas i följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="f0bf8-121">In the `sendMessage` function, replace the line `var message = buildMessage(sentMessageCount);` with the new line shown in the following snippet:</span></span>
   
   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. <span data-ttu-id="f0bf8-122">Spara alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="f0bf8-122">Save all the changes.</span></span>

### <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="f0bf8-123">Distribuera och köra exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="f0bf8-123">Deploy and run the sample application</span></span>
<span data-ttu-id="f0bf8-124">Distribuera och köra exempelprogrammet på Pi genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f0bf8-124">Deploy and run the sample application on Pi by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="f0bf8-125">Du bör se Indikator aktivera i två sekunder och sedan stänga av en annan två sekunder.</span><span class="sxs-lookup"><span data-stu-id="f0bf8-125">You should see the LED turn on for two seconds, and then turn off for another two seconds.</span></span> <span data-ttu-id="f0bf8-126">Det sista meddelandet ”stoppa” stoppar exempelprogrammet från att köras.</span><span class="sxs-lookup"><span data-stu-id="f0bf8-126">The last "stop" message stops the sample application from running.</span></span>

![Exempelprogrammet med och inaktivera meddelanden](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

<span data-ttu-id="f0bf8-128">Grattis!</span><span class="sxs-lookup"><span data-stu-id="f0bf8-128">Congratulations!</span></span> <span data-ttu-id="f0bf8-129">Meddelanden som skickas till Pi från IoT-hubben har anpassats.</span><span class="sxs-lookup"><span data-stu-id="f0bf8-129">You’ve successfully customized the messages that are sent to Pi from your IoT hub.</span></span>

### <a name="summary"></a><span data-ttu-id="f0bf8-130">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f0bf8-130">Summary</span></span>
<span data-ttu-id="f0bf8-131">Det här valfria avsnittet visar hur du anpassar meddelanden så att exempelprogrammet kan styra den på och av beteendet för Indikatorn på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="f0bf8-131">This optional section demonstrates how to customize messages so that the sample application can control the on and off behavior of the LED in a different way.</span></span>

