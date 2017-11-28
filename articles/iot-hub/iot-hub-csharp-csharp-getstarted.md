---
title: "aaaGet igång med Azure IoT Hub (.NET) | Microsoft Docs"
description: "Lär dig hur toosend enhet till moln meddelanden tooAzure IoT-hubb med IoT-SDK för .NET. Skapa simulerade enheten och tjänsten appar tooregister enheten, skicka meddelanden och läsa meddelanden från IoT-hubb."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: f40604ff-8fd6-4969-9e99-8574fbcf036c
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 56cf14687411898ea0fa4ebb1782e18b3930809c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-tooyour-iot-hub-using-net"></a><span data-ttu-id="cab71-104">Ansluta din enhet tooyour IoT-hubb med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="cab71-104">Connect your device tooyour IoT hub using .NET</span></span>

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="cab71-105">Hello slutet av den här självstudiekursen har du tre .NET konsolappar:</span><span class="sxs-lookup"><span data-stu-id="cab71-105">At hello end of this tutorial, you have three .NET console apps:</span></span>

* <span data-ttu-id="cab71-106">**CreateDeviceIdentity**, vilket skapar en enhetsidentitet och tillhörande nyckeln tooconnect din enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="cab71-106">**CreateDeviceIdentity**, which creates a device identity and associated security key tooconnect your device app.</span></span>
* <span data-ttu-id="cab71-107">**ReadDeviceToCloudMessages**, som visar hello telemetri som skickats av din enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="cab71-107">**ReadDeviceToCloudMessages**, which displays hello telemetry sent by your device app.</span></span>
* <span data-ttu-id="cab71-108">**SimulatedDevice**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och skickar meddelandet telemetri varje sekund med hello MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="cab71-108">**SimulatedDevice**, which connects tooyour IoT hub with hello device identity created earlier, and sends a telemetry message every second by using hello MQTT protocol.</span></span>

<span data-ttu-id="cab71-109">Du kan hämta eller klona hello Visual Studio-lösning som innehåller hello tre appar från Github.</span><span class="sxs-lookup"><span data-stu-id="cab71-109">You can download or clone hello Visual Studio solution, which contains hello three apps from Github.</span></span>

```bash
git clone https://github.com/Azure-Samples/iot-hub-dotnet-simulated-device-client-app.git
```

> [!NOTE]
> <span data-ttu-id="cab71-110">Information om hello Azure IoT-SDK: er som du kan använda toobuild både program toorun på enheter och din lösningens serverdel finns [Azure IoT SDK][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="cab71-110">For information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices, and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="cab71-111">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="cab71-111">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="cab71-112">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="cab71-112">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="cab71-113">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="cab71-113">An active Azure account.</span></span> <span data-ttu-id="cab71-114">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="cab71-114">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="cab71-115">Nu har du skapat din IoT-hubb och du har hello värdnamn och IoT-hubb anslutningssträngen som du behöver toocomplete hello resten av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="cab71-115">You have now created your IoT hub, and you have hello host name and IoT Hub connection string that you need toocomplete hello rest of this tutorial.</span></span>

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-csharp](../../includes/iot-hub-get-started-create-device-identity-csharp.md)]

<a id="D2C_csharp"></a>
## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="cab71-116">Ta emot meddelanden från enheten till molnet</span><span class="sxs-lookup"><span data-stu-id="cab71-116">Receive device-to-cloud messages</span></span>
<span data-ttu-id="cab71-117">I det här avsnittet ska du skapa en .NET-konsolapp som läser ”enhet till molnet”-meddelanden från IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="cab71-117">In this section, you create a .NET console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="cab71-118">En IoT-hubb Exponerar en [Azure Event Hubs][lnk-event-hubs-overview]-kompatibel endpoint tooenable du tooread meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="cab71-118">An IoT hub exposes an [Azure Event Hubs][lnk-event-hubs-overview]-compatible endpoint tooenable you tooread device-to-cloud messages.</span></span> <span data-ttu-id="cab71-119">enkel tookeep saker, den här guiden skapar en grundläggande läsare som inte lämpar sig för en distribution med hög genomströmning.</span><span class="sxs-lookup"><span data-stu-id="cab71-119">tookeep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="cab71-120">toolearn hur tooprocess enhet till moln meddelanden i skala, se hello [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="cab71-120">toolearn how tooprocess device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span> <span data-ttu-id="cab71-121">Mer information om hur tooprocess meddelanden från Händelsehubbar finns hello [Kom igång med Händelsehubbar] [ lnk-eventhubs-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="cab71-121">For more information about how tooprocess messages from Event Hubs, see hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial.</span></span> <span data-ttu-id="cab71-122">(Den här kursen är tillämpliga toohello IoT-hubb Event Hub-kompatibel slutpunkter.)</span><span class="sxs-lookup"><span data-stu-id="cab71-122">(This tutorial is applicable toohello IoT Hub Event Hub-compatible endpoints.)</span></span>

> [!NOTE]
> <span data-ttu-id="cab71-123">hello använder Event Hub-kompatibel slutpunkt för att läsa meddelanden från enhet till moln alltid hello AMQP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="cab71-123">hello Event Hub-compatible endpoint for reading device-to-cloud messages always uses hello AMQP protocol.</span></span>

1. <span data-ttu-id="cab71-124">I Visual Studio att lägga till en Visual C# klassiska skrivbordet projektet toohello aktuella-lösning med hello **Konsolapp (.NET Framework)** projektmall.</span><span class="sxs-lookup"><span data-stu-id="cab71-124">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution, by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="cab71-125">Se till att hello av .NET Framework 4.5.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="cab71-125">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="cab71-126">Namnet hello projektet **ReadDeviceToCloudMessages**.</span><span class="sxs-lookup"><span data-stu-id="cab71-126">Name hello project **ReadDeviceToCloudMessages**.</span></span>

    ![Nytt Visual C# Windows Classic Desktop-projekt][10a]

2. <span data-ttu-id="cab71-128">I Solution Explorer högerklickar du på hello **ReadDeviceToCloudMessages** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="cab71-128">In Solution Explorer, right-click hello **ReadDeviceToCloudMessages** project, and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="cab71-129">I hello **NuGet Package Manager** fönster, söka efter **WindowsAzure.ServiceBus**väljer **installera**, och Godkänn hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="cab71-129">In hello **NuGet Package Manager** window, search for **WindowsAzure.ServiceBus**, select **Install**, and accept hello terms of use.</span></span> <span data-ttu-id="cab71-130">Den här proceduren hämtar, installerar och lägger till en referens för[Azure Service Bus][lnk-servicebus-nuget], med dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="cab71-130">This procedure downloads, installs, and adds a reference too[Azure Service Bus][lnk-servicebus-nuget], with all its dependencies.</span></span> <span data-ttu-id="cab71-131">Det här paketet kan hello programmet tooconnect toohello Event Hub-kompatibel slutpunkt på din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cab71-131">This package enables hello application tooconnect toohello Event Hub-compatible endpoint on your IoT hub.</span></span>

4. <span data-ttu-id="cab71-132">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="cab71-132">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>

    ```csharp
    using Microsoft.ServiceBus.Messaging;
    using System.Threading;
    ```

5. <span data-ttu-id="cab71-133">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="cab71-133">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="cab71-134">Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello hub du skapat i hello avsnittet ”Skapa en IoT-hubb”.</span><span class="sxs-lookup"><span data-stu-id="cab71-134">Replace hello placeholder value with hello IoT Hub connection string for hello hub you created in hello "Create an IoT hub" section.</span></span>

    ```csharp
    static string connectionString = "{iothub connection string}";
    static string iotHubD2cEndpoint = "messages/events";
    static EventHubClient eventHubClient;
    ```

6. <span data-ttu-id="cab71-135">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="cab71-135">Add hello following method toohello **Program** class:</span></span>

    ```csharp
    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested) break;
            EventData eventData = await eventHubReceiver.ReceiveAsync();
            if (eventData == null) continue;

            string data = Encoding.UTF8.GetString(eventData.GetBytes());
            Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
        }
    }
    ```

    <span data-ttu-id="cab71-136">Den här metoden använder en **EventHubReceiver** instans tooreceive meddelanden från alla hello IoT hub-enhet till moln får partitioner.</span><span class="sxs-lookup"><span data-stu-id="cab71-136">This method uses an **EventHubReceiver** instance tooreceive messages from all hello IoT hub device-to-cloud receive partitions.</span></span> <span data-ttu-id="cab71-137">Observera hur du skickar en `DateTime.Now` parameter när du skapar hello **EventHubReceiver** objektet, så att den bara tar emot meddelanden som skickas när den har startat.</span><span class="sxs-lookup"><span data-stu-id="cab71-137">Notice how you pass a `DateTime.Now` parameter when you create hello **EventHubReceiver** object, so that it only receives messages sent after it starts.</span></span> <span data-ttu-id="cab71-138">Det här filtret är användbart i en testmiljö så att du kan se hello aktuella uppsättning av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="cab71-138">This filter is useful in a test environment so you can see hello current set of messages.</span></span> <span data-ttu-id="cab71-139">Koden bör se till att den bearbetar alla hälsningsmeddelande i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cab71-139">In a production environment, your code should make sure that it processes all hello messages.</span></span> <span data-ttu-id="cab71-140">Mer information finns i självstudiekursen hello [hur tooprocess IoT-hubb meddelanden från enhet till moln][lnk-process-d2c-tutorial].</span><span class="sxs-lookup"><span data-stu-id="cab71-140">For more information, see hello tutorial [How tooprocess IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial].</span></span>

7. <span data-ttu-id="cab71-141">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="cab71-141">Finally, add hello following lines toohello **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Receive messages. Ctrl-C tooexit.\n");
    eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

    var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

    CancellationTokenSource cts = new CancellationTokenSource();

    System.Console.CancelKeyPress += (s, e) =>
    {
        e.Cancel = true;
        cts.Cancel();
        Console.WriteLine("Exiting...");
    };

    var tasks = new List<Task>();
    foreach (string partition in d2cPartitions)
    {
        tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
    }  
    Task.WaitAll(tasks.ToArray());
    ```

## <a name="create-a-device-app"></a><span data-ttu-id="cab71-142">Skapa en enhetsapp</span><span class="sxs-lookup"><span data-stu-id="cab71-142">Create a device app</span></span>

<span data-ttu-id="cab71-143">I det här avsnittet skapar du en .NET-konsolapp som simulerar en enhet som skickar meddelanden från enhet till moln tooan IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cab71-143">In this section, you create a .NET console app that simulates a device that sends device-to-cloud messages tooan IoT hub.</span></span>

1. <span data-ttu-id="cab71-144">I Visual Studio att lägga till en Visual C# klassiska skrivbordet projektet toohello aktuella-lösning med hello **Konsolapp (.NET Framework)** projektmall.</span><span class="sxs-lookup"><span data-stu-id="cab71-144">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution, by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="cab71-145">Se till att hello av .NET Framework 4.5.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="cab71-145">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="cab71-146">Namnet hello projektet **SimulatedDevice**.</span><span class="sxs-lookup"><span data-stu-id="cab71-146">Name hello project **SimulatedDevice**.</span></span>

    ![Nytt Visual C# Windows Classic Desktop-projekt][10b]

2. <span data-ttu-id="cab71-148">I Solution Explorer högerklickar du på hello **SimulatedDevice** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="cab71-148">In Solution Explorer, right-click hello **SimulatedDevice** project, and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="cab71-149">I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **Microsoft.Azure.Devices.Client**väljer **installera** tooinstall hello **Microsoft.Azure.Devices.Client** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="cab71-149">In hello **NuGet Package Manager** window, select **Browse**, search for **Microsoft.Azure.Devices.Client**, select **Install** tooinstall hello **Microsoft.Azure.Devices.Client** package, and accept hello terms of use.</span></span> <span data-ttu-id="cab71-150">Den här proceduren hämtar, installerar och lägger till en referens toohello [SDK NuGet-paketet för Azure IoT-enhet] [ lnk-device-nuget] och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="cab71-150">This procedure downloads, installs, and adds a reference toohello [Azure IoT device SDK NuGet package][lnk-device-nuget] and its dependencies.</span></span>

4. <span data-ttu-id="cab71-151">Lägg till följande hello `using` instruktionen hello överst i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="cab71-151">Add hello following `using` statement at hello top of hello **Program.cs** file:</span></span>

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Newtonsoft.Json;
    ```

5. <span data-ttu-id="cab71-152">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="cab71-152">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="cab71-153">Ersätt `{iot hub hostname}` med hello IoT hub-värdnamnet du hämtade i hello ”skapa en IoT-hubb”.</span><span class="sxs-lookup"><span data-stu-id="cab71-153">Substitute `{iot hub hostname}` with hello IoT hub host name you retrieved in hello "Create an IoT hub" section.</span></span> <span data-ttu-id="cab71-154">Ersätt `{device key}` med hello enhetsnyckel som du hämtade i hello ”skapa en enhetsidentitet”.</span><span class="sxs-lookup"><span data-stu-id="cab71-154">Substitute `{device key}` with hello device key you retrieved in hello "Create a device identity" section.</span></span>

    ```csharp
    static DeviceClient deviceClient;
    static string iotHubUri = "{iot hub hostname}";
    static string deviceKey = "{device key}";
    ```

6. <span data-ttu-id="cab71-155">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="cab71-155">Add hello following method toohello **Program** class:</span></span>

    ```csharp
    private static async void SendDeviceToCloudMessagesAsync()
    {
        double minTemperature = 20;
        double minHumidity = 60;
        int messageId = 1;
        Random rand = new Random();

        while (true)
        {
            double currentTemperature = minTemperature + rand.NextDouble() * 15;
            double currentHumidity = minHumidity + rand.NextDouble() * 20;

            var telemetryDataPoint = new
            {
                messageId = messageId++,
                deviceId = "myFirstDevice",
                temperature = currentTemperature,
                humidity = currentHumidity
            };
            var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
            var message = new Message(Encoding.ASCII.GetBytes(messageString));
            message.Properties.Add("temperatureAlert", (currentTemperature > 30) ? "true" : "false");

            await deviceClient.SendEventAsync(message);
            Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

            await Task.Delay(1000);
        }
    }
    ```

    <span data-ttu-id="cab71-156">Den här metoden skickar ett nytt ”enhet till molnet”-meddelande varje sekund.</span><span class="sxs-lookup"><span data-stu-id="cab71-156">This method sends a new device-to-cloud message every second.</span></span> <span data-ttu-id="cab71-157">hello-meddelande innehåller en JSON-serialiserade objekt med hello enhets-ID och slumpmässigt genererade siffror toosimulate en temperatursensor och en fuktighet sensor.</span><span class="sxs-lookup"><span data-stu-id="cab71-157">hello message contains a JSON-serialized object, with hello device ID and randomly generated numbers toosimulate a temperature sensor, and a humidity sensor.</span></span>

7. <span data-ttu-id="cab71-158">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="cab71-158">Finally, add hello following lines toohello **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Simulated device\n");
    deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey ("myFirstDevice", deviceKey), TransportType.Mqtt);

    SendDeviceToCloudMessagesAsync();
    Console.ReadLine();
    ```

    <span data-ttu-id="cab71-159">Som standard hello **skapa** metod i en app för .NET Framework skapar en **DeviceClient** -instans som använder hello AMQP protokollet toocommunicate med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="cab71-159">By default, hello **Create** method in a .NET Framework app creates a **DeviceClient** instance that uses hello AMQP protocol toocommunicate with IoT Hub.</span></span> <span data-ttu-id="cab71-160">toouse hello MQTT eller HTTP-protokollet använder hello åsidosättning av hello **skapa** metod som du kan använda toospecify hello-protokollet.</span><span class="sxs-lookup"><span data-stu-id="cab71-160">toouse hello MQTT or HTTP protocol, use hello override of hello **Create** method that enables you toospecify hello protocol.</span></span> <span data-ttu-id="cab71-161">UWP och PCL klienter använder hello HTTP-protokollet som standard.</span><span class="sxs-lookup"><span data-stu-id="cab71-161">UWP and PCL clients use hello HTTP protocol by default.</span></span> <span data-ttu-id="cab71-162">Om du använder hello HTTP-protokollet bör du också lägga till hello **Microsoft.AspNet.WebApi.Client** NuGet-paketet tooyour projekt tooinclude hello **System.Net.Http.Formatting** namnområde.</span><span class="sxs-lookup"><span data-stu-id="cab71-162">If you use hello HTTP protocol, you should also add hello **Microsoft.AspNet.WebApi.Client** NuGet package tooyour project tooinclude hello **System.Net.Http.Formatting** namespace.</span></span>

<span data-ttu-id="cab71-163">Den här kursen tar dig igenom hello steg toocreate en IoT-hubb enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="cab71-163">This tutorial takes you through hello steps toocreate an IoT Hub device app.</span></span> <span data-ttu-id="cab71-164">Du kan också använda hello [ansluten tjänst för Azure IoT Hub] [ lnk-connected-service] Visual Studio-tillägget tooadd hello nödvändig kod tooyour enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="cab71-164">You can also use hello [Connected Service for Azure IoT Hub][lnk-connected-service] Visual Studio extension tooadd hello necessary code tooyour device app.</span></span>

> [!NOTE]
> <span data-ttu-id="cab71-165">enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="cab71-165">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="cab71-166">I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="cab71-166">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="cab71-167">Köra hello appar</span><span class="sxs-lookup"><span data-stu-id="cab71-167">Run hello apps</span></span>

<span data-ttu-id="cab71-168">Du är nu redo toorun hello appar.</span><span class="sxs-lookup"><span data-stu-id="cab71-168">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="cab71-169">Högerklicka på din lösning i Solution Explorer i Visual Studio och klicka sedan på **Ange startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="cab71-169">In Visual Studio, in Solution Explorer, right-click your solution, and then click **Set StartUp projects**.</span></span> <span data-ttu-id="cab71-170">Välj **flera Startprojekt**, och välj sedan **starta** som hello åtgärd för båda hello **ReadDeviceToCloudMessages** och **SimulatedDevice** projekt.</span><span class="sxs-lookup"><span data-stu-id="cab71-170">Select **Multiple startup projects**, and then select **Start** as hello action for both hello **ReadDeviceToCloudMessages** and **SimulatedDevice** projects.</span></span>

    ![Egenskaper för startprojekt][41]

2. <span data-ttu-id="cab71-172">Tryck på **F5** toostart både appar som körs.</span><span class="sxs-lookup"><span data-stu-id="cab71-172">Press **F5** toostart both apps running.</span></span> <span data-ttu-id="cab71-173">Hej konsolens utdata från hello **SimulatedDevice** visar hello meddelanden appen enheten skickar tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cab71-173">hello console output from hello **SimulatedDevice** app shows hello messages your device app sends tooyour IoT hub.</span></span> <span data-ttu-id="cab71-174">Hej konsolens utdata från hello **ReadDeviceToCloudMessages** app visar hälsningsmeddelande som tar emot din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cab71-174">hello console output from hello **ReadDeviceToCloudMessages** app shows hello messages that your IoT hub receives.</span></span>

    ![Konsolens utdata från appar][42]

3. <span data-ttu-id="cab71-176">Hej **användning** panelen i hello [Azure-portalen] [ lnk-portal] visar hello antalet meddelanden som skickas toohello IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="cab71-176">hello **Usage** tile in hello [Azure portal][lnk-portal] shows hello number of messages sent toohello IoT hub:</span></span>

    ![Panelen Användning på Azure Portal][43]

## <a name="next-steps"></a><span data-ttu-id="cab71-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cab71-178">Next steps</span></span>

<span data-ttu-id="cab71-179">I den här självstudiekursen konfigurerade en IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="cab71-179">In this tutorial, you configured an IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="cab71-180">Du har använt den här enhetens identitet tooenable hello enheten app toosend meddelanden från enhet till moln toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cab71-180">You used this device identity tooenable hello device app toosend device-to-cloud messages toohello IoT hub.</span></span> <span data-ttu-id="cab71-181">Du kan också skapat en app som visar hello som tagits emot av hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cab71-181">You also created an app that displays hello messages received by hello IoT hub.</span></span>

<span data-ttu-id="cab71-182">toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:</span><span class="sxs-lookup"><span data-stu-id="cab71-182">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="cab71-183">[Connecting your device][lnk-connect-device] (Ansluta din enhet)</span><span class="sxs-lookup"><span data-stu-id="cab71-183">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="cab71-184">[Connecting your device][lnk-device-management] (Komma igång med enhetshantering)</span><span class="sxs-lookup"><span data-stu-id="cab71-184">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="cab71-185">[Komma igång med IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="cab71-185">[Getting started with IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="cab71-186">toolearn hur tooextend IoT-lösningen och processen enhet till moln meddelandena i skala, se hello [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="cab71-186">toolearn how tooextend your IoT solution and process device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10a]: ./media/iot-hub-csharp-csharp-getstarted/create-receive-csharp1.png
[10b]: ./media/iot-hub-csharp-csharp-getstarted/create-device-csharp1.png


<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
