---
title: "Azure IoT Hub-enhet till moln meddelanden med hjälp av vägar (.Net) | Microsoft Docs"
description: "Så här bearbeta meddelanden från IoT-hubb enhet till moln genom att använda regler för Routning och anpassade slutpunkter för att skicka meddelanden till andra backend-tjänster."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 5177bac9-722f-47ef-8a14-b201142ba4bc
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 1d2b52ea005ab520bf294efa603512c00a92ee63
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-using-routes-net"></a><span data-ttu-id="217a8-103">IoT-hubb enhet till moln-meddelanden med hjälp av vägar (.NET)</span><span class="sxs-lookup"><span data-stu-id="217a8-103">Process IoT Hub device-to-cloud messages using routes (.NET)</span></span>

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

<span data-ttu-id="217a8-104">Den här kursen bygger på den [Kom igång med IoT-hubb] kursen.</span><span class="sxs-lookup"><span data-stu-id="217a8-104">This tutorial builds on the [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="217a8-105">Självstudier:</span><span class="sxs-lookup"><span data-stu-id="217a8-105">The tutorial:</span></span>

* <span data-ttu-id="217a8-106">Visar hur du använder regler för routning för att skicka meddelanden från enhet till moln i ett enkelt och konfigurationsbaserade sätt.</span><span class="sxs-lookup"><span data-stu-id="217a8-106">Shows you how to use routing rules to dispatch device-to-cloud messages in an easy, configuration-based way.</span></span>
* <span data-ttu-id="217a8-107">Visar hur du isolera interaktiva meddelanden som kräver omedelbar åtgärd från lösningens serverdel för vidare bearbetning.</span><span class="sxs-lookup"><span data-stu-id="217a8-107">Illustrates how to isolate interactive messages that require immediate action from the solution back end for further processing.</span></span> <span data-ttu-id="217a8-108">En enhet kan till exempel skicka larmmeddelandet som utlöser lägga till ett ärende i ett CRM-system.</span><span class="sxs-lookup"><span data-stu-id="217a8-108">For example, a device might send an alarm message that triggers inserting a ticket into a CRM system.</span></span> <span data-ttu-id="217a8-109">Däremot-datapunkt meddelanden, till exempel temperatur telemetri, mata in en analytics-motorn.</span><span class="sxs-lookup"><span data-stu-id="217a8-109">In contrast, data-point messages, such as temperature telemetry, feed into an analytics engine.</span></span>

<span data-ttu-id="217a8-110">I slutet av den här kursen kan köra du tre .NET konsolappar:</span><span class="sxs-lookup"><span data-stu-id="217a8-110">At the end of this tutorial, you run three .NET console apps:</span></span>

* <span data-ttu-id="217a8-111">**SimulatedDevice**, en modifierad version av app som skapats i den [Kom igång med IoT-hubb] självstudiekursen skickar meddelanden från enhet till moln datapunkt varje sekund och interaktiva enhet till moln meddelanden var 10: e sekund.</span><span class="sxs-lookup"><span data-stu-id="217a8-111">**SimulatedDevice**, a modified version of the app created in the [Get started with IoT Hub] tutorial sends data-point device-to-cloud messages every second, and interactive device-to-cloud messages every 10 seconds.</span></span>
* <span data-ttu-id="217a8-112">**ReadDeviceToCloudMessages** som visar den icke-kritiska telemetri som skickats av din enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="217a8-112">**ReadDeviceToCloudMessages** that displays the non-critical telemetry sent by your device app.</span></span>
* <span data-ttu-id="217a8-113">**ReadCriticalQueue** tar bort viktiga meddelanden som skickas av din enhetsapp från en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="217a8-113">**ReadCriticalQueue** de-queues the critical messages sent by your device app from a Service Bus queue.</span></span> <span data-ttu-id="217a8-114">Den här kön är kopplad till IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="217a8-114">This queue is attached to the IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="217a8-115">IoT-hubben har SDK stöd för många vilka plattformar och språk, inklusive C, Java och JavaScript.</span><span class="sxs-lookup"><span data-stu-id="217a8-115">IoT Hub has SDK support for many device platforms and languages, including C, Java, and JavaScript.</span></span> <span data-ttu-id="217a8-116">Mer information om hur du ersätter den simulerade enheten i den här självstudiekursen med en fysisk enhet, finns det [Azure IoT Developer Center].</span><span class="sxs-lookup"><span data-stu-id="217a8-116">To learn how to replace the simulated device in this tutorial with a physical device, see the [Azure IoT Developer Center].</span></span>

<span data-ttu-id="217a8-117">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="217a8-117">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="217a8-118">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="217a8-118">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="217a8-119">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="217a8-119">An active Azure account.</span></span> <br/><span data-ttu-id="217a8-120">Om du inte har ett konto kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/) på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="217a8-120">If you don't have an account, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

<span data-ttu-id="217a8-121">Du bör ha vissa grundläggande kunskaper om [Azure Storage] och [Azure Service Bus].</span><span class="sxs-lookup"><span data-stu-id="217a8-121">You should have some basic knowledge of [Azure Storage] and [Azure Service Bus].</span></span>

## <a name="send-interactive-messages"></a><span data-ttu-id="217a8-122">Skicka interaktiva meddelanden</span><span class="sxs-lookup"><span data-stu-id="217a8-122">Send interactive messages</span></span>

<span data-ttu-id="217a8-123">Ändra appen för enheter som du skapade i den [Kom igång med IoT-hubb] kursen att ibland skicka interaktiva meddelanden.</span><span class="sxs-lookup"><span data-stu-id="217a8-123">Modify the device app you created in the [Get started with IoT Hub] tutorial to occasionally send interactive messages.</span></span>

<span data-ttu-id="217a8-124">I Visual Studio, i den **SimulatedDevice** projektet genom att ersätta den `SendDeviceToCloudMessagesAsync` metoden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="217a8-124">In Visual Studio, in the **SimulatedDevice** project, replace the `SendDeviceToCloudMessagesAsync` method with the following code:</span></span>

```csharp
private static async void SendDeviceToCloudMessagesAsync()
{
    double minTemperature = 20;
    double minHumidity = 60;
    Random rand = new Random();

    while (true)
    {
        double currentTemperature = minTemperature + rand.NextDouble() * 15;
        double currentHumidity = minHumidity + rand.NextDouble() * 20;

        var telemetryDataPoint = new
        {
            deviceId = "myFirstDevice",
            temperature = currentTemperature,
            humidity = currentHumidity
        };
        var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
        string levelValue;

        if (rand.NextDouble() > 0.7)
        {
            messageString = "This is a critical message";
            levelValue = "critical";
        }
        else
        {
            levelValue = "normal";
        }
        
        var message = new Message(Encoding.ASCII.GetBytes(messageString));
        message.Properties.Add("level", levelValue);
        
        await deviceClient.SendEventAsync(message);
        Console.WriteLine("{0} > Sent message: {1}", DateTime.Now, messageString);

        await Task.Delay(1000);
    }
}
```

<span data-ttu-id="217a8-125">Den här metoden lägger slumpmässigt till egenskapen `"level": "critical"` på meddelanden som skickas av enheten, som simulerar ett meddelande som kräver omedelbara åtgärder med lösningen serverdel.</span><span class="sxs-lookup"><span data-stu-id="217a8-125">This method randomly adds the property `"level": "critical"` to messages sent by the device, which simulates a message that requires immediate action by the solution back-end.</span></span> <span data-ttu-id="217a8-126">Appen enheten överför denna information i egenskaperna för meddelandet i stället för i meddelandetexten, så att IoT-hubb kan vidarebefordra meddelandet till rätt meddelandets mål.</span><span class="sxs-lookup"><span data-stu-id="217a8-126">The device app passes this information in the message properties, instead of in the message body, so that IoT Hub can route the message to the proper message destination.</span></span>

> [!NOTE]
> <span data-ttu-id="217a8-127">Du kan använda meddelandeegenskaper skicka meddelanden för olika scenarier, inklusive kall sökväg bearbetning, förutom hot sökväg exemplet som visas här.</span><span class="sxs-lookup"><span data-stu-id="217a8-127">You can use message properties to route messages for various scenarios including cold-path processing, in addition to the hot-path example shown here.</span></span>

> [!NOTE]
> <span data-ttu-id="217a8-128">För enkelhetens skull implementerar inte den här självstudiekursen princip försök igen.</span><span class="sxs-lookup"><span data-stu-id="217a8-128">For the sake of simplicity, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="217a8-129">I produktionskod, bör du implementera en återförsöksprincip som exponentiell backoff enligt förslaget i MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="217a8-129">In production code, you should implement a retry policy such as exponential backoff, as suggested in the MSDN article [Transient Fault Handling].</span></span>

## <a name="route-messages-to-a-queue-in-your-iot-hub"></a><span data-ttu-id="217a8-130">Skicka meddelanden till en kö i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="217a8-130">Route messages to a queue in your IoT hub</span></span>

<span data-ttu-id="217a8-131">I det här avsnittet får du:</span><span class="sxs-lookup"><span data-stu-id="217a8-131">In this section, you:</span></span>

* <span data-ttu-id="217a8-132">Skapa en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="217a8-132">Create a Service Bus queue.</span></span>
* <span data-ttu-id="217a8-133">Anslut den till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="217a8-133">Connect it to your IoT hub.</span></span>
* <span data-ttu-id="217a8-134">Konfigurera din IoT-hubb för att skicka meddelanden till kön baserat på förekomsten av en egenskap i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="217a8-134">Configure your IoT hub to send messages to the queue based on the presence of a property on the message.</span></span>

<span data-ttu-id="217a8-135">Mer information om hur du bearbeta meddelanden från Service Bus-köer finns [Kom igång med köer][Service Bus queue].</span><span class="sxs-lookup"><span data-stu-id="217a8-135">For more information about how to process messages from Service Bus queues, see [Get started with queues][Service Bus queue].</span></span>

1. <span data-ttu-id="217a8-136">Skapa en Service Bus-kö som beskrivs i [Kom igång med köer][Service Bus queue].</span><span class="sxs-lookup"><span data-stu-id="217a8-136">Create a Service Bus queue as described in [Get started with queues][Service Bus queue].</span></span> <span data-ttu-id="217a8-137">Kön måste vara i samma prenumeration och region som din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="217a8-137">The queue must be in the same subscription and region as your IoT hub.</span></span> <span data-ttu-id="217a8-138">Anteckna namnet på namnområdet och kön.</span><span class="sxs-lookup"><span data-stu-id="217a8-138">Make a note of the namespace and queue name.</span></span>

    > [!NOTE]
    > <span data-ttu-id="217a8-139">Service Bus-köer och ämnen som används som IoT-hubbslutpunkter inte får ha **sessioner** eller **dubblettidentifiering** aktiverat.</span><span class="sxs-lookup"><span data-stu-id="217a8-139">Service Bus queues and topics used as IoT Hub endpoints must not have **Sessions** or **Duplicate Detection** enabled.</span></span> <span data-ttu-id="217a8-140">Om något av dessa alternativ är aktiverade ändpunkt som **inte åtkomlig** i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="217a8-140">If either of those options are enabled, the endpoint appears as **Unreachable** in the Azure portal.</span></span>

2. <span data-ttu-id="217a8-141">Öppna din IoT-hubb i Azure-portalen och klicka på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="217a8-141">In the Azure portal, open your IoT hub and click **Endpoints**.</span></span>
    
    ![Slutpunkter i IoT-hubb][30]

3. <span data-ttu-id="217a8-143">I den **slutpunkter** bladet, klickar du på **Lägg till** längst upp för att lägga till kön till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="217a8-143">In the **Endpoints** blade, click **Add** at the top to add your queue to your IoT hub.</span></span> <span data-ttu-id="217a8-144">Namnet på slutpunkten **CriticalQueue** och markera med hjälp av listrutorna **Service Bus-kö**, Service Bus-namnrymd som kön finns och namnet på din kö.</span><span class="sxs-lookup"><span data-stu-id="217a8-144">Name the endpoint **CriticalQueue** and use the drop-downs to select **Service Bus queue**, the Service Bus namespace in which your queue resides, and the name of your queue.</span></span> <span data-ttu-id="217a8-145">När du är klar klickar du på **spara** längst ned.</span><span class="sxs-lookup"><span data-stu-id="217a8-145">When you are done, click **Save** at the bottom.</span></span>
    
    ![Lägga till en slutpunkt][31]
    
4. <span data-ttu-id="217a8-147">Klicka på **vägar** i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="217a8-147">Now click **Routes** in your IoT Hub.</span></span> <span data-ttu-id="217a8-148">Klicka på **Lägg till** längst upp på bladet för att skapa en regel för vidarebefordran som skickar meddelanden till kön du just lagt till.</span><span class="sxs-lookup"><span data-stu-id="217a8-148">Click **Add** at the top of the blade to create a routing rule that routes messages to the queue you just added.</span></span> <span data-ttu-id="217a8-149">Välj **DeviceTelemetry** som datakällan.</span><span class="sxs-lookup"><span data-stu-id="217a8-149">Select **DeviceTelemetry** as the source of data.</span></span> <span data-ttu-id="217a8-150">Ange `level="critical"` som villkor och välj den kö som du just har lagt till som en anpassad slutpunkt som routning regeln slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="217a8-150">Enter `level="critical"` as the condition, and choose the queue you just added as a custom endpoint as the routing rule endpoint.</span></span> <span data-ttu-id="217a8-151">När du är klar klickar du på **spara** längst ned.</span><span class="sxs-lookup"><span data-stu-id="217a8-151">When you are done, click **Save** at the bottom.</span></span>
    
    ![Lägga till en väg][32]
    
    <span data-ttu-id="217a8-153">Kontrollera återställningsplats vägen är inställd på **på**.</span><span class="sxs-lookup"><span data-stu-id="217a8-153">Make sure the fallback route is set to **ON**.</span></span> <span data-ttu-id="217a8-154">Det här värdet är standardkonfigurationen för en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="217a8-154">This value is the default configuration for an IoT hub.</span></span>
    
    ![Fallback-väg][33]

## <a name="read-from-the-queue-endpoint"></a><span data-ttu-id="217a8-156">Läsa från kön slutpunkten</span><span class="sxs-lookup"><span data-stu-id="217a8-156">Read from the queue endpoint</span></span>

<span data-ttu-id="217a8-157">I det här avsnittet läsa meddelanden från kön slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="217a8-157">In this section, you read the messages from the queue endpoint.</span></span>

1. <span data-ttu-id="217a8-158">I Visual Studio lägger du till ett Visual C# Classic Desktop-projekt i den aktuella lösningen med hjälp av projektmallen **Console App (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="217a8-158">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution, by using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="217a8-159">Namnge projektet **ReadCriticalQueue**.</span><span class="sxs-lookup"><span data-stu-id="217a8-159">Name the project **ReadCriticalQueue**.</span></span>

2. <span data-ttu-id="217a8-160">I Solution Explorer högerklickar du på den **ReadCriticalQueue** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="217a8-160">In Solution Explorer, right-click the **ReadCriticalQueue** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="217a8-161">Den här åtgärden visar den **NuGet Package Manager** fönster.</span><span class="sxs-lookup"><span data-stu-id="217a8-161">This operation displays the **NuGet Package Manager** window.</span></span>

3. <span data-ttu-id="217a8-162">Sök efter **WindowsAzure.ServiceBus**, klickar du på **installera**, och Godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="217a8-162">Search for **WindowsAzure.ServiceBus**, click **Install**, and accept the terms of use.</span></span> <span data-ttu-id="217a8-163">Den här åtgärden hämtar, installerar och lägger till en referens till Azure Service Bus med dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="217a8-163">This operation downloads, installs, and adds a reference to the Azure Service Bus, with all its dependencies.</span></span>

4. <span data-ttu-id="217a8-164">Lägg till följande **med** instruktioner överst i den **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="217a8-164">Add the following **using** statements at the top of the **Program.cs** file:</span></span>
   
    ```csharp
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. <span data-ttu-id="217a8-165">Slutligen lägger du till följande rader till den **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="217a8-165">Finally, add the following lines to the **Main** method.</span></span> <span data-ttu-id="217a8-166">Ersätt anslutningssträngen med **lyssna** behörigheter för kön:</span><span class="sxs-lookup"><span data-stu-id="217a8-166">Substitute the connection string with **Listen** permissions for the queue:</span></span>
   
    ```csharp
    Console.WriteLine("Receive critical messages. Ctrl-C to exit.\n");
    var connectionString = "{service bus listen string}";
    var queueName = "{queue name}";
    
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);

    client.OnMessage(message =>
        {
            Stream stream = message.GetBody<Stream>();
            StreamReader reader = new StreamReader(stream, Encoding.ASCII);
            string s = reader.ReadToEnd();
            Console.WriteLine(String.Format("Message body: {0}", s));
        });
        
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a><span data-ttu-id="217a8-167">Köra programmen</span><span class="sxs-lookup"><span data-stu-id="217a8-167">Run the applications</span></span>
<span data-ttu-id="217a8-168">Du är nu redo att köra programmen.</span><span class="sxs-lookup"><span data-stu-id="217a8-168">Now you are ready to run the applications.</span></span>

1. <span data-ttu-id="217a8-169">Högerklicka på lösningen i Visual Studio i Solution Explorer och markera **ange Startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="217a8-169">In Visual Studio, in Solution Explorer, right-click your solution and select **Set StartUp Projects**.</span></span> <span data-ttu-id="217a8-170">Välj **flera Startprojekt**och välj **starta** åtgärden för den **ReadDeviceToCloudMessages**, **SimulatedDevice**, och **ReadCriticalQueue** projekt.</span><span class="sxs-lookup"><span data-stu-id="217a8-170">Select **Multiple startup projects**, then select **Start** as the action for the **ReadDeviceToCloudMessages**, **SimulatedDevice**, and **ReadCriticalQueue** projects.</span></span>
2. <span data-ttu-id="217a8-171">Tryck på **F5** för att starta konsolen de tre appar.</span><span class="sxs-lookup"><span data-stu-id="217a8-171">Press **F5** to start the three console apps.</span></span> <span data-ttu-id="217a8-172">Den **ReadDeviceToCloudMessages** appen har endast icke-kritiska meddelanden som skickas från den **SimulatedDevice** program, och **ReadCriticalQueue** appen har endast kritiska meddelanden.</span><span class="sxs-lookup"><span data-stu-id="217a8-172">The **ReadDeviceToCloudMessages** app has only non-critical messages sent from the **SimulatedDevice** application, and the **ReadCriticalQueue** app has only critical messages.</span></span>
   
   ![Tre konsolappar][50]

## <a name="next-steps"></a><span data-ttu-id="217a8-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="217a8-174">Next steps</span></span>
<span data-ttu-id="217a8-175">I den här självstudiekursen beskrivs hur du ska skicka meddelanden från enhet till moln med hjälp av meddelandet routningsfunktionen för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="217a8-175">In this tutorial, you learned how to reliably dispatch device-to-cloud messages by using the message routing functionality of IoT Hub.</span></span>

<span data-ttu-id="217a8-176">Den [hur du skickar meddelanden moln till enhet med IoT-hubben] [ lnk-c2d] visar hur du kan skicka meddelanden till dina enheter från din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="217a8-176">The [How to send cloud-to-device messages with IoT Hub][lnk-c2d] shows you how to send messages to your devices from your solution back end.</span></span>

<span data-ttu-id="217a8-177">Exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite][lnk-suite].</span><span class="sxs-lookup"><span data-stu-id="217a8-177">To see examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite][lnk-suite].</span></span>

<span data-ttu-id="217a8-178">Mer information om hur du utvecklar lösningar med IoT-hubb finns i [IoT-hubb Utvecklarhandbok].</span><span class="sxs-lookup"><span data-stu-id="217a8-178">To learn more about developing solutions with IoT Hub, see the [IoT Hub developer guide].</span></span>

<span data-ttu-id="217a8-179">Läs mer om meddelanderoutning i IoT-hubb i [skicka och ta emot meddelanden med IoT-hubben][lnk-devguide-messaging].</span><span class="sxs-lookup"><span data-stu-id="217a8-179">To learn more about message routing in IoT Hub, see [Send and receive messages with IoT Hub][lnk-devguide-messaging].</span></span>

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[30]: ./media/iot-hub-csharp-csharp-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/route-creation.png
[33]: ./media/iot-hub-csharp-csharp-process-d2c/fallback-route.png

<!-- Links -->
[Service Bus queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
<span data-ttu-id="217a8-180">[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/</span><span class="sxs-lookup"><span data-stu-id="217a8-180">[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/</span></span>
<span data-ttu-id="217a8-181">[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/</span><span class="sxs-lookup"><span data-stu-id="217a8-181">[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/</span></span>
<span data-ttu-id="217a8-182">[IoT-hubb Utvecklarhandbok]: iot-hub-devguide.md</span><span class="sxs-lookup"><span data-stu-id="217a8-182">[IoT Hub developer guide]: iot-hub-devguide.md</span></span>
<span data-ttu-id="217a8-183">[Kom igång med IoT-hubb]: iot-hub-csharp-csharp-getstarted.md</span><span class="sxs-lookup"><span data-stu-id="217a8-183">[Get started with IoT Hub]: iot-hub-csharp-csharp-getstarted.md</span></span>
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
<span data-ttu-id="217a8-184">[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot</span><span class="sxs-lookup"><span data-stu-id="217a8-184">[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot</span></span>
<span data-ttu-id="217a8-185">[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span><span class="sxs-lookup"><span data-stu-id="217a8-185">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span></span>
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
