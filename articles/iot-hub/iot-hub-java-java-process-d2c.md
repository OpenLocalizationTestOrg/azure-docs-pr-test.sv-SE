---
title: "aaaProcess meddelanden för Azure IoT Hub-enhet till moln (Java) | Microsoft Docs"
description: "Hur meddelanden tooprocess IoT-hubb meddelanden från enhet till moln med hjälp av regler för Routning och anpassade slutpunkter toodispatch tooother backend-tjänster."
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
ms.openlocfilehash: 084e84e721ca4297c4d7d6cb06a43b0bed9bce85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-java"></a><span data-ttu-id="65332-103">Bearbeta meddelanden från enhet till moln IoT-hubb (Java)</span><span class="sxs-lookup"><span data-stu-id="65332-103">Process IoT Hub device-to-cloud messages (Java)</span></span>

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

<span data-ttu-id="65332-104">Azure IoT Hub är en helt hanterad tjänst som gör tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning för serverdel.</span><span class="sxs-lookup"><span data-stu-id="65332-104">Azure IoT Hub is a fully managed service that enables reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="65332-105">Andra självstudier ([Kom igång med IoT-hubb] och [meddelanden moln till enhet med IoT-hubben][lnk-c2d]) visar hur toouse hello grundläggande enhet till moln och moln till enhet meddelandefunktioner för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="65332-105">Other tutorials ([Get started with IoT Hub] and [Send cloud-to-device messages with IoT Hub][lnk-c2d]) show you how toouse hello basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span>

<span data-ttu-id="65332-106">Den här kursen bygger på hello koden som visas i hello [Kom igång med IoT-hubb] kursen, och visar hur toouse meddelande routning tooprocess enhet till moln på ett skalbart sätt.</span><span class="sxs-lookup"><span data-stu-id="65332-106">This tutorial builds on hello code shown in hello [Get started with IoT Hub] tutorial, and shows you how toouse message routing tooprocess device-to-cloud messages in a scalable way.</span></span> <span data-ttu-id="65332-107">hello kursen visar hur tooprocess meddelanden som kräver omedelbar åtgärd från hello lösning serverdel.</span><span class="sxs-lookup"><span data-stu-id="65332-107">hello tutorial illustrates how tooprocess messages that require immediate action from hello solution back end.</span></span> <span data-ttu-id="65332-108">En enhet kan till exempel skicka larmmeddelandet som utlöser lägga till ett ärende i ett CRM-system.</span><span class="sxs-lookup"><span data-stu-id="65332-108">For example, a device might send an alarm message that triggers inserting a ticket into a CRM system.</span></span> <span data-ttu-id="65332-109">Däremot feed bara datapunkt meddelanden till en analytics-motor.</span><span class="sxs-lookup"><span data-stu-id="65332-109">By contrast, data-point messages simply feed into an analytics engine.</span></span> <span data-ttu-id="65332-110">Temperatur telemetri från en enhet som är toobe lagras för senare analys är till exempel en datapunkt meddelande.</span><span class="sxs-lookup"><span data-stu-id="65332-110">For example, temperature telemetry from a device that is toobe stored for later analysis is a data-point message.</span></span>

<span data-ttu-id="65332-111">Hello slutet av den här självstudiekursen, kan du köra tre Java konsolappar:</span><span class="sxs-lookup"><span data-stu-id="65332-111">At hello end of this tutorial, you run three Java console apps:</span></span>

* <span data-ttu-id="65332-112">**simulerade enheten**, en modifierad version av hello-app som skapats i hello [Kom igång med IoT-hubb] självstudiekursen skickar meddelanden från enhet till moln datapunkt varje sekund och interaktiva enhet till moln meddelanden var 10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="65332-112">**simulated-device**, a modified version of hello app created in hello [Get started with IoT Hub] tutorial, sends data-point device-to-cloud messages every second, and interactive device-to-cloud messages every 10 seconds.</span></span> <span data-ttu-id="65332-113">Den här appen använder hello AMQP protokollet toocommunicate med IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="65332-113">This app uses hello AMQP protocol toocommunicate with IoT Hub.</span></span>
* <span data-ttu-id="65332-114">**Läs-d2c-meddelanden** visar hello telemetri som skickats av din enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="65332-114">**read-d2c-messages** displays hello telemetry sent by your device app.</span></span>
* <span data-ttu-id="65332-115">**Läs kritisk kö** tar bort hello kritiska meddelanden från hello Service Bus-kö kopplade toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="65332-115">**read-critical-queue** de-queues hello critical messages from hello Service Bus queue attached toohello IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="65332-116">IoT-hubben har SDK stöd för många vilka plattformar och språk, inklusive C, Java och JavaScript.</span><span class="sxs-lookup"><span data-stu-id="65332-116">IoT Hub has SDK support for many device platforms and languages, including C, Java, and JavaScript.</span></span> <span data-ttu-id="65332-117">Anvisningar för hur tooreplace hello enheten i den här självstudiekursen med en fysisk enhet och hur tooconnect enheter tooan IoT Hub, se hello [Azure IoT Developer Center].</span><span class="sxs-lookup"><span data-stu-id="65332-117">For instructions on how tooreplace hello device in this tutorial with a physical device, and how tooconnect devices tooan IoT Hub, see hello [Azure IoT Developer Center].</span></span>

<span data-ttu-id="65332-118">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="65332-118">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="65332-119">En fullständig fungerande version av hello [Kom igång med IoT-hubb] kursen.</span><span class="sxs-lookup"><span data-stu-id="65332-119">A complete working version of hello [Get started with IoT Hub] tutorial.</span></span>
* <span data-ttu-id="65332-120">Hej senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="65332-120">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="65332-121">Maven 3</span><span class="sxs-lookup"><span data-stu-id="65332-121">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="65332-122">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="65332-122">An active Azure account.</span></span> <span data-ttu-id="65332-123">(Om du inte har ett konto kan du skapa ett [kostnadsfritt konto] [lnk-kostnadsfria-utvärderingsversion] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="65332-123">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="65332-124">Du bör ha vissa grundläggande kunskaper om [Azure Storage] och [Azure Service Bus].</span><span class="sxs-lookup"><span data-stu-id="65332-124">You should have some basic knowledge of [Azure Storage] and [Azure Service Bus].</span></span>

## <a name="send-interactive-messages-from-a-device-app"></a><span data-ttu-id="65332-125">Skicka interaktiva meddelanden från en enhetsapp</span><span class="sxs-lookup"><span data-stu-id="65332-125">Send interactive messages from a device app</span></span>
<span data-ttu-id="65332-126">I det här avsnittet kan du ändra hello enhetsapp som du skapade i hello [Kom igång med IoT-hubb] självstudiekursen toooccasionally skicka meddelanden som kräver omedelbar bearbetning.</span><span class="sxs-lookup"><span data-stu-id="65332-126">In this section, you modify hello device app you created in hello [Get started with IoT Hub] tutorial toooccasionally send messages that require immediate processing.</span></span>

1. <span data-ttu-id="65332-127">Använda en redigerare tooopen hello simulated-device\src\main\java\com\mycompany\app\App.java textfil.</span><span class="sxs-lookup"><span data-stu-id="65332-127">Use a text editor tooopen hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span> <span data-ttu-id="65332-128">Den här filen innehåller hello kod för hello **simulerade enheten** app som du skapade i hello [Kom igång med IoT-hubb] kursen.</span><span class="sxs-lookup"><span data-stu-id="65332-128">This file contains hello code for hello **simulated-device** app you created in hello [Get started with IoT Hub] tutorial.</span></span>

2. <span data-ttu-id="65332-129">Ersätt hello **MessageSender** klassen med följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="65332-129">Replace hello **MessageSender** class with hello following code:</span></span>

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
   
    <span data-ttu-id="65332-130">Den här metoden lägger slumpmässigt hello egenskapen `"level": "critical"` toomessages som skickas av hello enhet som simulerar ett meddelande som kräver omedelbara åtgärder med hello programmet serverdel.</span><span class="sxs-lookup"><span data-stu-id="65332-130">This method randomly adds hello property `"level": "critical"` toomessages sent by hello device, which simulates a message that requires immediate action by hello application back-end.</span></span> <span data-ttu-id="65332-131">hello program Överför denna information i Egenskaper för hello-meddelande, i stället för i hello meddelandetexten, så att IoT-hubb kan vidarebefordra hello meddelandets toohello rätt meddelande mål.</span><span class="sxs-lookup"><span data-stu-id="65332-131">hello application passes this information in hello message properties, instead of in hello message body, so that IoT Hub can route hello message toohello proper message destination.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="65332-132">Du kan använda egenskaper tooroute meddelanden för olika scenarier, inklusive kall sökväg, bearbetning, dessutom toohello varm sökväg exemplet som visas här.</span><span class="sxs-lookup"><span data-stu-id="65332-132">You can use message properties tooroute messages for various scenarios including cold-path processing, in addition toohello hot path example shown here.</span></span>

2. <span data-ttu-id="65332-133">Spara och Stäng hello simulated-device\src\main\java\com\mycompany\app\App.java fil.</span><span class="sxs-lookup"><span data-stu-id="65332-133">Save and close hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

    > [!NOTE]
    > <span data-ttu-id="65332-134">Den här självstudiekursen implementerar inte några återförsöksprincip på hello ut för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="65332-134">For hello sake of simplicity, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="65332-135">I produktionskod, bör du implementera en återförsöksprincip som exponentiell backoff enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="65332-135">In production code, you should implement a retry policy such as exponential backoff, as suggested in hello MSDN article [Transient Fault Handling].</span></span>

3. <span data-ttu-id="65332-136">toobuild hello **simulerade enheten** app med Maven, kör följande kommando i Kommandotolken hello i hello simulerade enheten mappen hello:</span><span class="sxs-lookup"><span data-stu-id="65332-136">toobuild hello **simulated-device** app using Maven, execute hello following command at hello command prompt in hello simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="add-a-queue-tooyour-iot-hub-and-route-messages-tooit"></a><span data-ttu-id="65332-137">Lägg till en kö tooyour IoT hub och flöde meddelanden tooit</span><span class="sxs-lookup"><span data-stu-id="65332-137">Add a queue tooyour IoT hub and route messages tooit</span></span>

<span data-ttu-id="65332-138">I det här avsnittet skapar du en Service Bus-kö, Anslut den tooyour IoT-hubb och konfigurera din IoT-hubb toosend meddelanden toohello kö baserat på hello förekomsten av en egenskap på hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="65332-138">In this section, you create a Service Bus queue, connect it tooyour IoT hub, and configure your IoT hub toosend messages toohello queue based on hello presence of a property on hello message.</span></span> <span data-ttu-id="65332-139">Mer information om hur tooprocess meddelanden från Service Bus-köer finns [Kom igång med köer][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="65332-139">For more information about how tooprocess messages from Service Bus queues, see [Get started with queues][lnk-sb-queues-java].</span></span>

1. <span data-ttu-id="65332-140">Skapa en Service Bus-kö som beskrivs i [Kom igång med köer][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="65332-140">Create a Service Bus queue as described in [Get started with queues][lnk-sb-queues-java].</span></span> <span data-ttu-id="65332-141">Anteckna hello namnområde och kön namn.</span><span class="sxs-lookup"><span data-stu-id="65332-141">Make a note of hello namespace and queue name.</span></span>

2. <span data-ttu-id="65332-142">I hello Azure-portalen, öppna din IoT-hubb och klickar på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="65332-142">In hello Azure portal, open your IoT hub and click **Endpoints**.</span></span>

    ![Slutpunkter i IoT-hubb][30]

3. <span data-ttu-id="65332-144">I hello **slutpunkter** bladet, klickar du på **Lägg till** på hello översta tooadd kön tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="65332-144">In hello **Endpoints** blade, click **Add** at hello top tooadd your queue tooyour IoT hub.</span></span> <span data-ttu-id="65332-145">Namnet hello endpoint **CriticalQueue** och använda hello-listrutor tooselect **Service Bus-kö**hello Service Bus-namnrymd som kön finns och hello namnet på din kö.</span><span class="sxs-lookup"><span data-stu-id="65332-145">Name hello endpoint **CriticalQueue** and use hello drop-downs tooselect **Service Bus queue**, hello Service Bus namespace in which your queue resides, and hello name of your queue.</span></span> <span data-ttu-id="65332-146">När du är klar klickar du på **spara** längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="65332-146">When you are done, click **Save** at hello bottom.</span></span>

    ![Lägga till en slutpunkt][31]

4. <span data-ttu-id="65332-148">Klicka på **vägar** i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="65332-148">Now click **Routes** in your IoT Hub.</span></span> <span data-ttu-id="65332-149">Klicka på **Lägg till** hello överst i hello bladet toocreate en routningsregel som skickar meddelanden toohello kö du just lagt till.</span><span class="sxs-lookup"><span data-stu-id="65332-149">Click **Add** at hello top of hello blade toocreate a routing rule that routes messages toohello queue you just added.</span></span> <span data-ttu-id="65332-150">Välj **DeviceTelemetry** som hello datakälla.</span><span class="sxs-lookup"><span data-stu-id="65332-150">Select **DeviceTelemetry** as hello source of data.</span></span> <span data-ttu-id="65332-151">Ange `level="critical"` som hello villkor och välj hello kö som du just har lagt till som en anpassad slutpunkt som hello routning regeln slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="65332-151">Enter `level="critical"` as hello condition, and choose hello queue you just added as a custom endpoint as hello routing rule endpoint.</span></span> <span data-ttu-id="65332-152">När du är klar klickar du på **spara** längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="65332-152">When you are done, click **Save** at hello bottom.</span></span>

    ![Lägga till en väg][32]

    <span data-ttu-id="65332-154">Kontrollera hello återställningsplats vägen anges för**på**.</span><span class="sxs-lookup"><span data-stu-id="65332-154">Make sure hello fallback route is set too**ON**.</span></span> <span data-ttu-id="65332-155">Den här inställningen är hello standardkonfigurationen för en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="65332-155">This setting is hello default configuration of an IoT hub.</span></span>

    ![Fallback-väg][33]

## <a name="optional-read-from-hello-queue-endpoint"></a><span data-ttu-id="65332-157">(Valfritt) Läsa från hello kön slutpunkt</span><span class="sxs-lookup"><span data-stu-id="65332-157">(Optional) Read from hello queue endpoint</span></span>

<span data-ttu-id="65332-158">Du kan om du vill läsa hälsningsmeddelande från hello kön slutpunkten genom att följa instruktionerna hello i [Kom igång med köer][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="65332-158">You can optionally read hello messages from hello queue endpoint by following hello instructions in [Get started with queues][lnk-sb-queues-java].</span></span> <span data-ttu-id="65332-159">Namnet hello app **Läs kritisk kö**.</span><span class="sxs-lookup"><span data-stu-id="65332-159">Name hello app **read-critical-queue**.</span></span>

## <a name="run-hello-applications"></a><span data-ttu-id="65332-160">Köra hello program</span><span class="sxs-lookup"><span data-stu-id="65332-160">Run hello applications</span></span>

<span data-ttu-id="65332-161">Nu är du redo toorun hello tre program.</span><span class="sxs-lookup"><span data-stu-id="65332-161">Now you are ready toorun hello three applications.</span></span>

1. <span data-ttu-id="65332-162">toorun hello **lästa d2c meddelanden** program i en kommandotolk eller shell navigera toohello Läs d2c mappen och kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="65332-162">toorun hello **read-d2c-messages** application, in a command prompt or shell navigate toohello read-d2c folder and execute hello following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![Kör Läs-d2c-meddelanden][readd2c]

2. <span data-ttu-id="65332-164">toorun hello **Läs kritisk kö** program i en kommandotolk eller shell navigera toohello Läs kritisk kö mappen och kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="65332-164">toorun hello **read-critical-queue** application, in a command prompt or shell navigate toohello read-critical-queue folder and execute hello following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Kör Läs kritiska meddelanden][readqueue]

3. <span data-ttu-id="65332-166">toorun hello **simulerade enheten** app i en kommandotolk eller shell navigera toohello simulerade enheten mappen och kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="65332-166">toorun hello **simulated-device** app, in a command prompt or shell navigate toohello simulated-device folder and execute hello following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Kör simulerade enheten][simulateddevice]

## <a name="next-steps"></a><span data-ttu-id="65332-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="65332-168">Next steps</span></span>

<span data-ttu-id="65332-169">I kursen får du lärt dig hur tooreliably skicka meddelanden från enhet till moln med hjälp av hello meddelandet routningsfunktionen IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="65332-169">In this tutorial, you learned how tooreliably dispatch device-to-cloud messages by using hello message routing functionality of IoT Hub.</span></span>

<span data-ttu-id="65332-170">Hej [hur toosend moln till enhet meddelanden med IoT-hubb] [ lnk-c2d] visar hur toosend meddelanden tooyour enheter från din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="65332-170">hello [How toosend cloud-to-device messages with IoT Hub][lnk-c2d] shows you how toosend messages tooyour devices from your solution back end.</span></span>

<span data-ttu-id="65332-171">toosee exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite][lnk-suite].</span><span class="sxs-lookup"><span data-stu-id="65332-171">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite][lnk-suite].</span></span>

<span data-ttu-id="65332-172">toolearn mer information om hur du utvecklar lösningar med IoT-hubb finns hello [IoT-hubb Utvecklarhandbok].</span><span class="sxs-lookup"><span data-stu-id="65332-172">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

<span data-ttu-id="65332-173">toolearn mer om meddelanderoutning i IoT Hub, se [skicka och ta emot meddelanden med IoT-hubben][lnk-devguide-messaging].</span><span class="sxs-lookup"><span data-stu-id="65332-173">toolearn more about message routing in IoT Hub, see [Send and receive messages with IoT Hub][lnk-devguide-messaging].</span></span>

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

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT-hubb Utvecklarhandbok]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Kom igång med IoT-hubb]: iot-hub-java-java-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Hantering av tillfälligt fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-c2d]: iot-hub-java-java-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
