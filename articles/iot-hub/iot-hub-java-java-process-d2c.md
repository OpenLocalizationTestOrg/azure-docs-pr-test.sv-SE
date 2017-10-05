---
title: Azure IoT Hub-enhet till moln meddelanden (Java) | Microsoft Docs
description: "Så här bearbeta meddelanden från IoT-hubb enhet till moln genom att använda regler för Routning och anpassade slutpunkter för att skicka meddelanden till andra backend-tjänster."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.openlocfilehash: d1aca8f39e305105d4ec9f63fbe7bee95487e294
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-java"></a><span data-ttu-id="c037c-103">Bearbeta meddelanden från enhet till moln IoT-hubb (Java)</span><span class="sxs-lookup"><span data-stu-id="c037c-103">Process IoT Hub device-to-cloud messages (Java)</span></span>

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

<span data-ttu-id="c037c-104">Azure IoT Hub är en helt hanterad tjänst som gör tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning för serverdel.</span><span class="sxs-lookup"><span data-stu-id="c037c-104">Azure IoT Hub is a fully managed service that enables reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="c037c-105">Andra självstudier ([Kom igång med IoT-hubb] och [meddelanden moln till enhet med IoT-hubben][lnk-c2d]) visar hur du använder grundläggande enheten till molnet och moln till enhet meddelandefunktioner för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c037c-105">Other tutorials ([Get started with IoT Hub] and [Send cloud-to-device messages with IoT Hub][lnk-c2d]) show you how to use the basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span>

<span data-ttu-id="c037c-106">Den här kursen bygger på koden som visas i den [Kom igång med IoT-hubb] kursen, och visar hur du använder meddelanderoutning vill bearbeta meddelanden från enhet till moln i ett skalbart sätt.</span><span class="sxs-lookup"><span data-stu-id="c037c-106">This tutorial builds on the code shown in the [Get started with IoT Hub] tutorial, and shows you how to use message routing to process device-to-cloud messages in a scalable way.</span></span> <span data-ttu-id="c037c-107">Kursen visar hur du kan bearbeta meddelanden som kräver omedelbara åtgärder från lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="c037c-107">The tutorial illustrates how to process messages that require immediate action from the solution back end.</span></span> <span data-ttu-id="c037c-108">En enhet kan till exempel skicka larmmeddelandet som utlöser lägga till ett ärende i ett CRM-system.</span><span class="sxs-lookup"><span data-stu-id="c037c-108">For example, a device might send an alarm message that triggers inserting a ticket into a CRM system.</span></span> <span data-ttu-id="c037c-109">Däremot feed bara datapunkt meddelanden till en analytics-motor.</span><span class="sxs-lookup"><span data-stu-id="c037c-109">By contrast, data-point messages simply feed into an analytics engine.</span></span> <span data-ttu-id="c037c-110">Temperatur telemetri från en enhet som ska lagras för senare analys är till exempel en datapunkt meddelande.</span><span class="sxs-lookup"><span data-stu-id="c037c-110">For example, temperature telemetry from a device that is to be stored for later analysis is a data-point message.</span></span>

<span data-ttu-id="c037c-111">I slutet av den här kursen kan köra du tre Java-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="c037c-111">At the end of this tutorial, you run three Java console apps:</span></span>

* <span data-ttu-id="c037c-112">**simulerade enheten**, en modifierad version av app som skapats i den [Kom igång med IoT-hubb] självstudiekursen skickar meddelanden från enhet till moln datapunkt varje sekund och interaktiva enhet till moln meddelanden var 10: e sekund .</span><span class="sxs-lookup"><span data-stu-id="c037c-112">**simulated-device**, a modified version of the app created in the [Get started with IoT Hub] tutorial, sends data-point device-to-cloud messages every second, and interactive device-to-cloud messages every 10 seconds.</span></span> <span data-ttu-id="c037c-113">Den här appen använder AMQP-protokollet för att kommunicera med IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c037c-113">This app uses the AMQP protocol to communicate with IoT Hub.</span></span>
* <span data-ttu-id="c037c-114">**Läs-d2c-meddelanden** visar telemetri som skickats av din enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="c037c-114">**read-d2c-messages** displays the telemetry sent by your device app.</span></span>
* <span data-ttu-id="c037c-115">**Läs kritisk kö** tar bort viktiga meddelanden från Service Bus-kö som är kopplade till IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="c037c-115">**read-critical-queue** de-queues the critical messages from the Service Bus queue attached to the IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="c037c-116">IoT-hubben har SDK stöd för många vilka plattformar och språk, inklusive C, Java och JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c037c-116">IoT Hub has SDK support for many device platforms and languages, including C, Java, and JavaScript.</span></span> <span data-ttu-id="c037c-117">Instruktioner om hur du ersätter enheten i den här självstudiekursen med en fysisk enhet och ansluta enheter till en IoT-hubb finns i [Azure IoT Developer Center].</span><span class="sxs-lookup"><span data-stu-id="c037c-117">For instructions on how to replace the device in this tutorial with a physical device, and how to connect devices to an IoT Hub, see the [Azure IoT Developer Center].</span></span>

<span data-ttu-id="c037c-118">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c037c-118">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="c037c-119">En fullständig fungerande version av den [Kom igång med IoT-hubb] kursen.</span><span class="sxs-lookup"><span data-stu-id="c037c-119">A complete working version of the [Get started with IoT Hub] tutorial.</span></span>
* <span data-ttu-id="c037c-120">Senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="c037c-120">The latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="c037c-121">Maven 3</span><span class="sxs-lookup"><span data-stu-id="c037c-121">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="c037c-122">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c037c-122">An active Azure account.</span></span> <span data-ttu-id="c037c-123">(Om du inte har ett konto kan du skapa ett [kostnadsfritt konto] [lnk-kostnadsfria-utvärderingsversion] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="c037c-123">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="c037c-124">Du bör ha vissa grundläggande kunskaper om [Azure Storage] och [Azure Service Bus].</span><span class="sxs-lookup"><span data-stu-id="c037c-124">You should have some basic knowledge of [Azure Storage] and [Azure Service Bus].</span></span>

## <a name="send-interactive-messages-from-a-device-app"></a><span data-ttu-id="c037c-125">Skicka interaktiva meddelanden från en enhetsapp</span><span class="sxs-lookup"><span data-stu-id="c037c-125">Send interactive messages from a device app</span></span>
<span data-ttu-id="c037c-126">I det här avsnittet kan du ändra appen för enheter som du skapade i den [Kom igång med IoT-hubb] kursen att ibland skicka meddelanden som kräver omedelbar bearbetning.</span><span class="sxs-lookup"><span data-stu-id="c037c-126">In this section, you modify the device app you created in the [Get started with IoT Hub] tutorial to occasionally send messages that require immediate processing.</span></span>

1. <span data-ttu-id="c037c-127">Använd en textredigerare för att öppna filen simulated-device\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="c037c-127">Use a text editor to open the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span> <span data-ttu-id="c037c-128">Den här filen innehåller koden för den **simulerade enheten** app som du skapade i den [Kom igång med IoT-hubb] kursen.</span><span class="sxs-lookup"><span data-stu-id="c037c-128">This file contains the code for the **simulated-device** app you created in the [Get started with IoT Hub] tutorial.</span></span>

2. <span data-ttu-id="c037c-129">Ersätt den **MessageSender** klassen med följande kod:</span><span class="sxs-lookup"><span data-stu-id="c037c-129">Replace the **MessageSender** class with the following code:</span></span>

    ```java
    private static class MessageSender implements Runnable {

        public void run()  {
            try {
                double minTemperature = 20;
                double minHumidity = 60;
                Random rand = new Random();

                while (true) {
                    String msgStr;
                    Message msg;
                    if (new Random().nextDouble() > 0.7) {
                        msgStr = "This is a critical message.";
                        msg = new Message(msgStr);
                        msg.setProperty("level", "critical");
                    } else {
                        double currentTemperature = minTemperature + rand.nextDouble() * 15;
                        double currentHumidity = minHumidity + rand.nextDouble() * 20; 
                        TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
                        telemetryDataPoint.deviceId = deviceId;
                        telemetryDataPoint.temperature = currentTemperature;
                        telemetryDataPoint.humidity = currentHumidity;

                        msgStr = telemetryDataPoint.serialize();
                        msg = new Message(msgStr);
                    }
                    
                    System.out.println("Sending: " + msgStr);

                    Object lockobj = new Object();
                    EventCallback callback = new EventCallback();
                    client.sendEventAsync(msg, callback, lockobj);

                    synchronized (lockobj) {
                        lockobj.wait();
                    }
                    Thread.sleep(1000);
                }
            } catch (InterruptedException e) {
                System.out.println("Finished.");
            }
        }
    }
    ```
   
    <span data-ttu-id="c037c-130">Den här metoden lägger slumpmässigt till egenskapen `"level": "critical"` på meddelanden som skickas av enheten, som simulerar ett meddelande som kräver omedelbara åtgärder med programmet serverdel.</span><span class="sxs-lookup"><span data-stu-id="c037c-130">This method randomly adds the property `"level": "critical"` to messages sent by the device, which simulates a message that requires immediate action by the application back-end.</span></span> <span data-ttu-id="c037c-131">Programmet Överför denna information i egenskaperna för meddelandet i stället för i meddelandetexten, så att IoT-hubb kan vidarebefordra meddelandet till rätt meddelandets mål.</span><span class="sxs-lookup"><span data-stu-id="c037c-131">The application passes this information in the message properties, instead of in the message body, so that IoT Hub can route the message to the proper message destination.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c037c-132">Du kan använda meddelandeegenskaper skicka meddelanden för olika scenarier, inklusive kall sökväg bearbetning, förutom varm sökväg exemplet som visas här.</span><span class="sxs-lookup"><span data-stu-id="c037c-132">You can use message properties to route messages for various scenarios including cold-path processing, in addition to the hot path example shown here.</span></span>

2. <span data-ttu-id="c037c-133">Spara och stäng filen simulated-device\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="c037c-133">Save and close the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c037c-134">För enkelhetens skull implementerar inte den här självstudiekursen princip försök igen.</span><span class="sxs-lookup"><span data-stu-id="c037c-134">For the sake of simplicity, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="c037c-135">I produktionskod, bör du implementera en återförsöksprincip som exponentiell backoff enligt förslaget i MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="c037c-135">In production code, you should implement a retry policy such as exponential backoff, as suggested in the MSDN article [Transient Fault Handling].</span></span>

3. <span data-ttu-id="c037c-136">Skapa appen **simulated-device** med hjälp av Maven genom att köra följande kommando i Kommandotolken i mappen simulated-device:</span><span class="sxs-lookup"><span data-stu-id="c037c-136">To build the **simulated-device** app using Maven, execute the following command at the command prompt in the simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="add-a-queue-to-your-iot-hub-and-route-messages-to-it"></a><span data-ttu-id="c037c-137">Lägg till en kö i IoT-hubb och flöde meddelanden till den</span><span class="sxs-lookup"><span data-stu-id="c037c-137">Add a queue to your IoT hub and route messages to it</span></span>

<span data-ttu-id="c037c-138">I det här avsnittet skapar du en Service Bus-kö, ansluta till din IoT-hubb och konfigurera din IoT-hubb för att skicka meddelanden till kön baserat på förekomsten av en egenskap i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="c037c-138">In this section, you create a Service Bus queue, connect it to your IoT hub, and configure your IoT hub to send messages to the queue based on the presence of a property on the message.</span></span> <span data-ttu-id="c037c-139">Mer information om hur du bearbeta meddelanden från Service Bus-köer finns [Kom igång med köer][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="c037c-139">For more information about how to process messages from Service Bus queues, see [Get started with queues][lnk-sb-queues-java].</span></span>

1. <span data-ttu-id="c037c-140">Skapa en Service Bus-kö som beskrivs i [Kom igång med köer][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="c037c-140">Create a Service Bus queue as described in [Get started with queues][lnk-sb-queues-java].</span></span> <span data-ttu-id="c037c-141">Anteckna namnet på namnområdet och kön.</span><span class="sxs-lookup"><span data-stu-id="c037c-141">Make a note of the namespace and queue name.</span></span>

2. <span data-ttu-id="c037c-142">Öppna din IoT-hubb i Azure-portalen och klicka på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="c037c-142">In the Azure portal, open your IoT hub and click **Endpoints**.</span></span>

    ![Slutpunkter i IoT-hubb][30]

3. <span data-ttu-id="c037c-144">I den **slutpunkter** bladet, klickar du på **Lägg till** längst upp för att lägga till kön till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c037c-144">In the **Endpoints** blade, click **Add** at the top to add your queue to your IoT hub.</span></span> <span data-ttu-id="c037c-145">Namnet på slutpunkten **CriticalQueue** och markera med hjälp av listrutorna **Service Bus-kö**, Service Bus-namnrymd som kön finns och namnet på din kö.</span><span class="sxs-lookup"><span data-stu-id="c037c-145">Name the endpoint **CriticalQueue** and use the drop-downs to select **Service Bus queue**, the Service Bus namespace in which your queue resides, and the name of your queue.</span></span> <span data-ttu-id="c037c-146">När du är klar klickar du på **spara** längst ned.</span><span class="sxs-lookup"><span data-stu-id="c037c-146">When you are done, click **Save** at the bottom.</span></span>

    ![Lägga till en slutpunkt][31]

4. <span data-ttu-id="c037c-148">Klicka på **vägar** i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c037c-148">Now click **Routes** in your IoT Hub.</span></span> <span data-ttu-id="c037c-149">Klicka på **Lägg till** längst upp på bladet för att skapa en regel för vidarebefordran som skickar meddelanden till kön du just lagt till.</span><span class="sxs-lookup"><span data-stu-id="c037c-149">Click **Add** at the top of the blade to create a routing rule that routes messages to the queue you just added.</span></span> <span data-ttu-id="c037c-150">Välj **DeviceTelemetry** som datakällan.</span><span class="sxs-lookup"><span data-stu-id="c037c-150">Select **DeviceTelemetry** as the source of data.</span></span> <span data-ttu-id="c037c-151">Ange `level="critical"` som villkor och välj den kö som du just har lagt till som en anpassad slutpunkt som routning regeln slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="c037c-151">Enter `level="critical"` as the condition, and choose the queue you just added as a custom endpoint as the routing rule endpoint.</span></span> <span data-ttu-id="c037c-152">När du är klar klickar du på **spara** längst ned.</span><span class="sxs-lookup"><span data-stu-id="c037c-152">When you are done, click **Save** at the bottom.</span></span>

    ![Lägga till en väg][32]

    <span data-ttu-id="c037c-154">Kontrollera återställningsplats vägen är inställd på **på**.</span><span class="sxs-lookup"><span data-stu-id="c037c-154">Make sure the fallback route is set to **ON**.</span></span> <span data-ttu-id="c037c-155">Den här inställningen är standardkonfigurationen för en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c037c-155">This setting is the default configuration of an IoT hub.</span></span>

    ![Fallback-väg][33]

## <a name="optional-read-from-the-queue-endpoint"></a><span data-ttu-id="c037c-157">(Valfritt) Läsa från kön slutpunkten</span><span class="sxs-lookup"><span data-stu-id="c037c-157">(Optional) Read from the queue endpoint</span></span>

<span data-ttu-id="c037c-158">Du kan du läsa meddelanden från kön slutpunkten genom att följa instruktionerna i [Kom igång med köer][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="c037c-158">You can optionally read the messages from the queue endpoint by following the instructions in [Get started with queues][lnk-sb-queues-java].</span></span> <span data-ttu-id="c037c-159">Namnge appen **Läs kritisk kö**.</span><span class="sxs-lookup"><span data-stu-id="c037c-159">Name the app **read-critical-queue**.</span></span>

## <a name="run-the-applications"></a><span data-ttu-id="c037c-160">Köra programmen</span><span class="sxs-lookup"><span data-stu-id="c037c-160">Run the applications</span></span>

<span data-ttu-id="c037c-161">Nu är du redo att köra tre program.</span><span class="sxs-lookup"><span data-stu-id="c037c-161">Now you are ready to run the three applications.</span></span>

1. <span data-ttu-id="c037c-162">Att köra den **lästa d2c meddelanden** program i en kommandotolk eller shell navigera till mappen Läs d2c och kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c037c-162">To run the **read-d2c-messages** application, in a command prompt or shell navigate to the read-d2c folder and execute the following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![Kör Läs-d2c-meddelanden][readd2c]

2. <span data-ttu-id="c037c-164">Att köra den **Läs kritisk kö** program i en kommandotolk eller shell navigera till mappen Läs kritiska kön och kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c037c-164">To run the **read-critical-queue** application, in a command prompt or shell navigate to the read-critical-queue folder and execute the following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Kör Läs kritiska meddelanden][readqueue]

3. <span data-ttu-id="c037c-166">Att köra den **simulerade enheten** i en kommandotolk eller shell appen, navigera till mappen simulerade enheten och kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c037c-166">To run the **simulated-device** app, in a command prompt or shell navigate to the simulated-device folder and execute the following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Kör simulerade enheten][simulateddevice]

## <a name="next-steps"></a><span data-ttu-id="c037c-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c037c-168">Next steps</span></span>

<span data-ttu-id="c037c-169">I den här självstudiekursen beskrivs hur du ska skicka meddelanden från enhet till moln med hjälp av meddelandet routningsfunktionen för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c037c-169">In this tutorial, you learned how to reliably dispatch device-to-cloud messages by using the message routing functionality of IoT Hub.</span></span>

<span data-ttu-id="c037c-170">Den [hur du skickar meddelanden moln till enhet med IoT-hubben] [ lnk-c2d] visar hur du kan skicka meddelanden till dina enheter från din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="c037c-170">The [How to send cloud-to-device messages with IoT Hub][lnk-c2d] shows you how to send messages to your devices from your solution back end.</span></span>

<span data-ttu-id="c037c-171">Exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite][lnk-suite].</span><span class="sxs-lookup"><span data-stu-id="c037c-171">To see examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite][lnk-suite].</span></span>

<span data-ttu-id="c037c-172">Mer information om hur du utvecklar lösningar med IoT-hubb finns i [IoT-hubb Utvecklarhandbok].</span><span class="sxs-lookup"><span data-stu-id="c037c-172">To learn more about developing solutions with IoT Hub, see the [IoT Hub developer guide].</span></span>

<span data-ttu-id="c037c-173">Läs mer om meddelanderoutning i IoT-hubb i [skicka och ta emot meddelanden med IoT-hubben][lnk-devguide-messaging].</span><span class="sxs-lookup"><span data-stu-id="c037c-173">To learn more about message routing in IoT Hub, see [Send and receive messages with IoT Hub][lnk-devguide-messaging].</span></span>

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-java-java-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-java-java-process-d2c/route-creation.png
[33]: ./media/iot-hub-java-java-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-java]: ../service-bus-messaging/service-bus-java-how-to-use-queues.md

<span data-ttu-id="c037c-174">[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/</span><span class="sxs-lookup"><span data-stu-id="c037c-174">[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/</span></span>
<span data-ttu-id="c037c-175">[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/</span><span class="sxs-lookup"><span data-stu-id="c037c-175">[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/</span></span>

<span data-ttu-id="c037c-176">[IoT-hubb Utvecklarhandbok]: iot-hub-devguide.md</span><span class="sxs-lookup"><span data-stu-id="c037c-176">[IoT Hub developer guide]: iot-hub-devguide.md</span></span>
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
<span data-ttu-id="c037c-177">[Kom igång med IoT-hubb]: iot-hub-java-java-getstarted.md</span><span class="sxs-lookup"><span data-stu-id="c037c-177">[Get started with IoT Hub]: iot-hub-java-java-getstarted.md</span></span>
<span data-ttu-id="c037c-178">[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot</span><span class="sxs-lookup"><span data-stu-id="c037c-178">[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot</span></span>
<span data-ttu-id="c037c-179">[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh675232.aspx</span><span class="sxs-lookup"><span data-stu-id="c037c-179">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh675232.aspx</span></span>

<!-- Links -->
<span data-ttu-id="c037c-180">[Hantering av tillfälligt fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span><span class="sxs-lookup"><span data-stu-id="c037c-180">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span></span>

[lnk-c2d]: iot-hub-java-java-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
