---
title: "aaaProcess Azure IoT Hub-enhet till moln meddelanden med hjälp av vägar (.Net) | Microsoft Docs"
description: "Hur meddelanden tooprocess IoT-hubb meddelanden från enhet till moln med hjälp av regler för Routning och anpassade slutpunkter toodispatch tooother backend-tjänster."
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
ms.openlocfilehash: c1dd5be04ca30c65af2be466ba6c8c1858333154
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-using-routes-net"></a><span data-ttu-id="b628e-103">IoT-hubb enhet till moln-meddelanden med hjälp av vägar (.NET)</span><span class="sxs-lookup"><span data-stu-id="b628e-103">Process IoT Hub device-to-cloud messages using routes (.NET)</span></span>

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

<span data-ttu-id="b628e-104">Den här kursen bygger på hello [Kom igång med IoT-hubb] kursen.</span><span class="sxs-lookup"><span data-stu-id="b628e-104">This tutorial builds on hello [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="b628e-105">hello Självstudier:</span><span class="sxs-lookup"><span data-stu-id="b628e-105">hello tutorial:</span></span>

* <span data-ttu-id="b628e-106">Visar hur toouse routning regler toodispatch meddelanden från enhet till moln i ett enkelt och konfigurationsbaserade sätt.</span><span class="sxs-lookup"><span data-stu-id="b628e-106">Shows you how toouse routing rules toodispatch device-to-cloud messages in an easy, configuration-based way.</span></span>
* <span data-ttu-id="b628e-107">Illustrerar hur tooisolate interaktiva meddelanden som kräver omedelbar åtgärd från hello lösning serverdel för vidare bearbetning.</span><span class="sxs-lookup"><span data-stu-id="b628e-107">Illustrates how tooisolate interactive messages that require immediate action from hello solution back end for further processing.</span></span> <span data-ttu-id="b628e-108">En enhet kan till exempel skicka larmmeddelandet som utlöser lägga till ett ärende i ett CRM-system.</span><span class="sxs-lookup"><span data-stu-id="b628e-108">For example, a device might send an alarm message that triggers inserting a ticket into a CRM system.</span></span> <span data-ttu-id="b628e-109">Däremot-datapunkt meddelanden, till exempel temperatur telemetri, mata in en analytics-motorn.</span><span class="sxs-lookup"><span data-stu-id="b628e-109">In contrast, data-point messages, such as temperature telemetry, feed into an analytics engine.</span></span>

<span data-ttu-id="b628e-110">Hello slutet av den här självstudiekursen, kan du köra tre .NET konsolappar:</span><span class="sxs-lookup"><span data-stu-id="b628e-110">At hello end of this tutorial, you run three .NET console apps:</span></span>

* <span data-ttu-id="b628e-111">**SimulatedDevice**, en modifierad version av hello-app som skapats i hello [Kom igång med IoT-hubb] självstudiekursen skickar meddelanden från enhet till moln datapunkt varje sekund och interaktiva enhet till moln meddelanden var 10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="b628e-111">**SimulatedDevice**, a modified version of hello app created in hello [Get started with IoT Hub] tutorial sends data-point device-to-cloud messages every second, and interactive device-to-cloud messages every 10 seconds.</span></span>
* <span data-ttu-id="b628e-112">**ReadDeviceToCloudMessages** som visar hello icke-kritiska telemetri som skickats av din enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="b628e-112">**ReadDeviceToCloudMessages** that displays hello non-critical telemetry sent by your device app.</span></span>
* <span data-ttu-id="b628e-113">**ReadCriticalQueue** tar bort hello viktiga meddelanden som skickas av din enhetsapp från en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="b628e-113">**ReadCriticalQueue** de-queues hello critical messages sent by your device app from a Service Bus queue.</span></span> <span data-ttu-id="b628e-114">Den här kön är anslutna toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b628e-114">This queue is attached toohello IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="b628e-115">IoT-hubben har SDK stöd för många vilka plattformar och språk, inklusive C, Java och JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b628e-115">IoT Hub has SDK support for many device platforms and languages, including C, Java, and JavaScript.</span></span> <span data-ttu-id="b628e-116">toolearn hur tooreplace hello simulerade enheten med en fysisk enhet i den här kursen finns hello [Azure IoT Developer Center].</span><span class="sxs-lookup"><span data-stu-id="b628e-116">toolearn how tooreplace hello simulated device in this tutorial with a physical device, see hello [Azure IoT Developer Center].</span></span>

<span data-ttu-id="b628e-117">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="b628e-117">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="b628e-118">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="b628e-118">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="b628e-119">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="b628e-119">An active Azure account.</span></span> <br/><span data-ttu-id="b628e-120">Om du inte har ett konto kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/) på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="b628e-120">If you don't have an account, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

<span data-ttu-id="b628e-121">Du bör ha vissa grundläggande kunskaper om [Azure Storage] och [Azure Service Bus].</span><span class="sxs-lookup"><span data-stu-id="b628e-121">You should have some basic knowledge of [Azure Storage] and [Azure Service Bus].</span></span>

## <a name="send-interactive-messages"></a><span data-ttu-id="b628e-122">Skicka interaktiva meddelanden</span><span class="sxs-lookup"><span data-stu-id="b628e-122">Send interactive messages</span></span>

<span data-ttu-id="b628e-123">Ändra hello enhetsapp som du skapade i hello [Kom igång med IoT-hubb] självstudiekursen toooccasionally skicka interaktiva meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b628e-123">Modify hello device app you created in hello [Get started with IoT Hub] tutorial toooccasionally send interactive messages.</span></span>

<span data-ttu-id="b628e-124">I Visual Studio i hello **SimulatedDevice** projekt, Ersätt hello `SendDeviceToCloudMessagesAsync` metod med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="b628e-124">In Visual Studio, in hello **SimulatedDevice** project, replace hello `SendDeviceToCloudMessagesAsync` method with hello following code:</span></span>

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

<span data-ttu-id="b628e-125">Den här metoden lägger slumpmässigt hello egenskapen `"level": "critical"` toomessages som skickas av hello enhet som simulerar ett meddelande som kräver omedelbara åtgärder med hello lösning serverdel.</span><span class="sxs-lookup"><span data-stu-id="b628e-125">This method randomly adds hello property `"level": "critical"` toomessages sent by hello device, which simulates a message that requires immediate action by hello solution back-end.</span></span> <span data-ttu-id="b628e-126">Hej enhetsapp Överför denna information i Egenskaper för hello-meddelande, i stället för i hello meddelandetexten, så att IoT-hubb kan vidarebefordra hello meddelandets toohello rätt meddelande mål.</span><span class="sxs-lookup"><span data-stu-id="b628e-126">hello device app passes this information in hello message properties, instead of in hello message body, so that IoT Hub can route hello message toohello proper message destination.</span></span>

> [!NOTE]
> <span data-ttu-id="b628e-127">Du kan använda egenskaper tooroute meddelanden för olika scenarier, inklusive kall sökväg, bearbetning, dessutom toohello hot sökväg exemplet som visas här.</span><span class="sxs-lookup"><span data-stu-id="b628e-127">You can use message properties tooroute messages for various scenarios including cold-path processing, in addition toohello hot-path example shown here.</span></span>

> [!NOTE]
> <span data-ttu-id="b628e-128">Den här självstudiekursen implementerar inte några återförsöksprincip på hello ut för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="b628e-128">For hello sake of simplicity, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="b628e-129">I produktionskod, bör du implementera en återförsöksprincip som exponentiell backoff enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="b628e-129">In production code, you should implement a retry policy such as exponential backoff, as suggested in hello MSDN article [Transient Fault Handling].</span></span>

## <a name="route-messages-tooa-queue-in-your-iot-hub"></a><span data-ttu-id="b628e-130">Väg tooa kön i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="b628e-130">Route messages tooa queue in your IoT hub</span></span>

<span data-ttu-id="b628e-131">I det här avsnittet får du:</span><span class="sxs-lookup"><span data-stu-id="b628e-131">In this section, you:</span></span>

* <span data-ttu-id="b628e-132">Skapa en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="b628e-132">Create a Service Bus queue.</span></span>
* <span data-ttu-id="b628e-133">Anslut den tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b628e-133">Connect it tooyour IoT hub.</span></span>
* <span data-ttu-id="b628e-134">Konfigurera din IoT-hubb toosend meddelanden toohello kö baserat på hello förekomsten av en egenskap på hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="b628e-134">Configure your IoT hub toosend messages toohello queue based on hello presence of a property on hello message.</span></span>

<span data-ttu-id="b628e-135">Mer information om hur tooprocess meddelanden från Service Bus-köer finns [Kom igång med köer][Service Bus queue].</span><span class="sxs-lookup"><span data-stu-id="b628e-135">For more information about how tooprocess messages from Service Bus queues, see [Get started with queues][Service Bus queue].</span></span>

1. <span data-ttu-id="b628e-136">Skapa en Service Bus-kö som beskrivs i [Kom igång med köer][Service Bus queue].</span><span class="sxs-lookup"><span data-stu-id="b628e-136">Create a Service Bus queue as described in [Get started with queues][Service Bus queue].</span></span> <span data-ttu-id="b628e-137">hello kön måste vara i hello samma prenumeration och region som din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b628e-137">hello queue must be in hello same subscription and region as your IoT hub.</span></span> <span data-ttu-id="b628e-138">Anteckna hello namnområde och kön namn.</span><span class="sxs-lookup"><span data-stu-id="b628e-138">Make a note of hello namespace and queue name.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b628e-139">Service Bus-köer och ämnen som används som IoT-hubbslutpunkter inte får ha **sessioner** eller **dubblettidentifiering** aktiverat.</span><span class="sxs-lookup"><span data-stu-id="b628e-139">Service Bus queues and topics used as IoT Hub endpoints must not have **Sessions** or **Duplicate Detection** enabled.</span></span> <span data-ttu-id="b628e-140">Om något av dessa alternativ är aktiverade hello ändpunkt som **inte åtkomlig** i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b628e-140">If either of those options are enabled, hello endpoint appears as **Unreachable** in hello Azure portal.</span></span>

2. <span data-ttu-id="b628e-141">I hello Azure-portalen, öppna din IoT-hubb och klickar på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="b628e-141">In hello Azure portal, open your IoT hub and click **Endpoints**.</span></span>
    
    ![Slutpunkter i IoT-hubb][30]

3. <span data-ttu-id="b628e-143">I hello **slutpunkter** bladet, klickar du på **Lägg till** på hello översta tooadd kön tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b628e-143">In hello **Endpoints** blade, click **Add** at hello top tooadd your queue tooyour IoT hub.</span></span> <span data-ttu-id="b628e-144">Namnet hello endpoint **CriticalQueue** och använda hello-listrutor tooselect **Service Bus-kö**hello Service Bus-namnrymd som kön finns och hello namnet på din kö.</span><span class="sxs-lookup"><span data-stu-id="b628e-144">Name hello endpoint **CriticalQueue** and use hello drop-downs tooselect **Service Bus queue**, hello Service Bus namespace in which your queue resides, and hello name of your queue.</span></span> <span data-ttu-id="b628e-145">När du är klar klickar du på **spara** längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="b628e-145">When you are done, click **Save** at hello bottom.</span></span>
    
    ![Lägga till en slutpunkt][31]
    
4. <span data-ttu-id="b628e-147">Klicka på **vägar** i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b628e-147">Now click **Routes** in your IoT Hub.</span></span> <span data-ttu-id="b628e-148">Klicka på **Lägg till** hello överst i hello bladet toocreate en routningsregel som skickar meddelanden toohello kö du just lagt till.</span><span class="sxs-lookup"><span data-stu-id="b628e-148">Click **Add** at hello top of hello blade toocreate a routing rule that routes messages toohello queue you just added.</span></span> <span data-ttu-id="b628e-149">Välj **DeviceTelemetry** som hello datakälla.</span><span class="sxs-lookup"><span data-stu-id="b628e-149">Select **DeviceTelemetry** as hello source of data.</span></span> <span data-ttu-id="b628e-150">Ange `level="critical"` som hello villkor och välj hello kö som du just har lagt till som en anpassad slutpunkt som hello routning regeln slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="b628e-150">Enter `level="critical"` as hello condition, and choose hello queue you just added as a custom endpoint as hello routing rule endpoint.</span></span> <span data-ttu-id="b628e-151">När du är klar klickar du på **spara** längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="b628e-151">When you are done, click **Save** at hello bottom.</span></span>
    
    ![Lägga till en väg][32]
    
    <span data-ttu-id="b628e-153">Kontrollera hello återställningsplats vägen anges för**på**.</span><span class="sxs-lookup"><span data-stu-id="b628e-153">Make sure hello fallback route is set too**ON**.</span></span> <span data-ttu-id="b628e-154">Det här värdet är hello standardkonfigurationen för en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b628e-154">This value is hello default configuration for an IoT hub.</span></span>
    
    ![Fallback-väg][33]

## <a name="read-from-hello-queue-endpoint"></a><span data-ttu-id="b628e-156">Läsa från hello kön slutpunkt</span><span class="sxs-lookup"><span data-stu-id="b628e-156">Read from hello queue endpoint</span></span>

<span data-ttu-id="b628e-157">I det här avsnittet läsa hälsningsmeddelande från hello kön slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="b628e-157">In this section, you read hello messages from hello queue endpoint.</span></span>

1. <span data-ttu-id="b628e-158">I Visual Studio att lägga till en Visual C# klassiska skrivbordet projektet toohello aktuella-lösning med hello **Konsolapp (.NET Framework)** projektmall.</span><span class="sxs-lookup"><span data-stu-id="b628e-158">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution, by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="b628e-159">Namnet hello projektet **ReadCriticalQueue**.</span><span class="sxs-lookup"><span data-stu-id="b628e-159">Name hello project **ReadCriticalQueue**.</span></span>

2. <span data-ttu-id="b628e-160">I Solution Explorer högerklickar du på hello **ReadCriticalQueue** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="b628e-160">In Solution Explorer, right-click hello **ReadCriticalQueue** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="b628e-161">Den här åtgärden visar hello **NuGet Package Manager** fönster.</span><span class="sxs-lookup"><span data-stu-id="b628e-161">This operation displays hello **NuGet Package Manager** window.</span></span>

3. <span data-ttu-id="b628e-162">Sök efter **WindowsAzure.ServiceBus**, klickar du på **installera**, och Godkänn hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="b628e-162">Search for **WindowsAzure.ServiceBus**, click **Install**, and accept hello terms of use.</span></span> <span data-ttu-id="b628e-163">Den här åtgärden hämtar, installerar och lägger till en referens toohello Azure Service Bus med dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="b628e-163">This operation downloads, installs, and adds a reference toohello Azure Service Bus, with all its dependencies.</span></span>

4. <span data-ttu-id="b628e-164">Lägg till följande hello **med** instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="b628e-164">Add hello following **using** statements at hello top of hello **Program.cs** file:</span></span>
   
    ```csharp
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. <span data-ttu-id="b628e-165">Slutligen lägger du till följande rader toohello hello **Main** metod.</span><span class="sxs-lookup"><span data-stu-id="b628e-165">Finally, add hello following lines toohello **Main** method.</span></span> <span data-ttu-id="b628e-166">Ersätt hello-anslutningssträngen med **lyssna** behörigheter för hello kö:</span><span class="sxs-lookup"><span data-stu-id="b628e-166">Substitute hello connection string with **Listen** permissions for hello queue:</span></span>
   
    ```csharp
    Console.WriteLine("Receive critical messages. Ctrl-C tooexit.\n");
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

## <a name="run-hello-applications"></a><span data-ttu-id="b628e-167">Köra hello program</span><span class="sxs-lookup"><span data-stu-id="b628e-167">Run hello applications</span></span>
<span data-ttu-id="b628e-168">Nu är du redo toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="b628e-168">Now you are ready toorun hello applications.</span></span>

1. <span data-ttu-id="b628e-169">Högerklicka på lösningen i Visual Studio i Solution Explorer och markera **ange Startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="b628e-169">In Visual Studio, in Solution Explorer, right-click your solution and select **Set StartUp Projects**.</span></span> <span data-ttu-id="b628e-170">Välj **flera Startprojekt**och välj **starta** som hello åtgärd för hello **ReadDeviceToCloudMessages**, **SimulatedDevice**, och **ReadCriticalQueue** projekt.</span><span class="sxs-lookup"><span data-stu-id="b628e-170">Select **Multiple startup projects**, then select **Start** as hello action for hello **ReadDeviceToCloudMessages**, **SimulatedDevice**, and **ReadCriticalQueue** projects.</span></span>
2. <span data-ttu-id="b628e-171">Tryck på **F5** toostart hello tre konsolappar.</span><span class="sxs-lookup"><span data-stu-id="b628e-171">Press **F5** toostart hello three console apps.</span></span> <span data-ttu-id="b628e-172">Hej **ReadDeviceToCloudMessages** appen har endast icke-kritiska meddelanden skickas från hello **SimulatedDevice** program och hello **ReadCriticalQueue** app har bara kritiska meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b628e-172">hello **ReadDeviceToCloudMessages** app has only non-critical messages sent from hello **SimulatedDevice** application, and hello **ReadCriticalQueue** app has only critical messages.</span></span>
   
   ![Tre konsolappar][50]

## <a name="next-steps"></a><span data-ttu-id="b628e-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b628e-174">Next steps</span></span>
<span data-ttu-id="b628e-175">I kursen får du lärt dig hur tooreliably skicka meddelanden från enhet till moln med hjälp av hello meddelandet routningsfunktionen IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b628e-175">In this tutorial, you learned how tooreliably dispatch device-to-cloud messages by using hello message routing functionality of IoT Hub.</span></span>

<span data-ttu-id="b628e-176">Hej [hur toosend moln till enhet meddelanden med IoT-hubb] [ lnk-c2d] visar hur toosend meddelanden tooyour enheter från din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="b628e-176">hello [How toosend cloud-to-device messages with IoT Hub][lnk-c2d] shows you how toosend messages tooyour devices from your solution back end.</span></span>

<span data-ttu-id="b628e-177">toosee exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite][lnk-suite].</span><span class="sxs-lookup"><span data-stu-id="b628e-177">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite][lnk-suite].</span></span>

<span data-ttu-id="b628e-178">toolearn mer information om hur du utvecklar lösningar med IoT-hubb finns hello [IoT-hubb Utvecklarhandbok].</span><span class="sxs-lookup"><span data-stu-id="b628e-178">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

<span data-ttu-id="b628e-179">toolearn mer om meddelanderoutning i IoT Hub, se [skicka och ta emot meddelanden med IoT-hubben][lnk-devguide-messaging].</span><span class="sxs-lookup"><span data-stu-id="b628e-179">toolearn more about message routing in IoT Hub, see [Send and receive messages with IoT Hub][lnk-devguide-messaging].</span></span>

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[30]: ./media/iot-hub-csharp-csharp-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/route-creation.png
[33]: ./media/iot-hub-csharp-csharp-process-d2c/fallback-route.png

<!-- Links -->
[Service Bus queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/
[IoT-hubb Utvecklarhandbok]: iot-hub-devguide.md
[Kom igång med IoT-hubb]: iot-hub-csharp-csharp-getstarted.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
