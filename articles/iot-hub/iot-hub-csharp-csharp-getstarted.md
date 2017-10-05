---
title: "Komma igång med Azure IoT Hub (.NET) | Microsoft Docs"
description: "Lär dig hur du skickar meddelanden från enheten till molnet i Azure IoT Hub med IoT SDK:er för .NET. Skapa appar för simulerade enheter och tjänster för att registrera din enhet, skicka meddelanden och läsa meddelanden från IoT Hub."
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
ms.openlocfilehash: 69296eb9ac2a74a97b632d27733a6a06500b4abd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="connect-your-device-to-your-iot-hub-using-net"></a><span data-ttu-id="787ea-104">Anslut din enhet till IoT Hub med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="787ea-104">Connect your device to your IoT hub using .NET</span></span>

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="787ea-105">I slutet av den här självstudiekursen har du tre .NET-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="787ea-105">At the end of this tutorial, you have three .NET console apps:</span></span>

* <span data-ttu-id="787ea-106">**CreateDeviceIdentity**, som skapar en enhetsidentitet och en associerad säkerhetsnyckel för att ansluta din app för enheter.</span><span class="sxs-lookup"><span data-stu-id="787ea-106">**CreateDeviceIdentity**, which creates a device identity and associated security key to connect your device app.</span></span>
* <span data-ttu-id="787ea-107">**ReadDeviceToCloudMessages**, som visar telemetri som skickas av appen för enheter.</span><span class="sxs-lookup"><span data-stu-id="787ea-107">**ReadDeviceToCloudMessages**, which displays the telemetry sent by your device app.</span></span>
* <span data-ttu-id="787ea-108">**SimulatedDevice**, som ansluter till din IoT Hub med enhetsidentiteten som skapades tidigare och som skickar ett telemetrimeddelande varje sekund med hjälp av MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="787ea-108">**SimulatedDevice**, which connects to your IoT hub with the device identity created earlier, and sends a telemetry message every second by using the MQTT protocol.</span></span>

<span data-ttu-id="787ea-109">Du kan hämta eller klona Visual Studio-lösningen som innehåller de tre apparna från Github.</span><span class="sxs-lookup"><span data-stu-id="787ea-109">You can download or clone the Visual Studio solution, which contains the three apps from Github.</span></span>

```bash
git clone https://github.com/Azure-Samples/iot-hub-dotnet-simulated-device-client-app.git
```

> [!NOTE]
> <span data-ttu-id="787ea-110">Artikeln om [Azure IoT SDK:er][lnk-hub-sdks] innehåller information om Azure IoT SDK:er som du kan använda för att skapa båda programmen så att de kan köras på enheter och på lösningens backend-server.</span><span class="sxs-lookup"><span data-stu-id="787ea-110">For information about the Azure IoT SDKs that you can use to build both applications to run on devices, and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="787ea-111">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="787ea-111">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="787ea-112">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="787ea-112">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="787ea-113">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="787ea-113">An active Azure account.</span></span> <span data-ttu-id="787ea-114">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="787ea-114">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="787ea-115">Nu har du skapat din IoT Hub och du har värdnamnet och IoT Hub-anslutningssträngen som du behöver för att slutföra resten av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="787ea-115">You have now created your IoT hub, and you have the host name and IoT Hub connection string that you need to complete the rest of this tutorial.</span></span>

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-csharp](../../includes/iot-hub-get-started-create-device-identity-csharp.md)]

<a id="D2C_csharp"></a>
## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="787ea-116">Ta emot meddelanden från enheten till molnet</span><span class="sxs-lookup"><span data-stu-id="787ea-116">Receive device-to-cloud messages</span></span>
<span data-ttu-id="787ea-117">I det här avsnittet ska du skapa en .NET-konsolapp som läser ”enhet till molnet”-meddelanden från IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="787ea-117">In this section, you create a .NET console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="787ea-118">En IoT Hub exponerar en [Azure Event Hubs][lnk-event-hubs-overview]-kompatibel slutpunkt så att du kan läsa meddelanden från enheter till molnet.</span><span class="sxs-lookup"><span data-stu-id="787ea-118">An IoT hub exposes an [Azure Event Hubs][lnk-event-hubs-overview]-compatible endpoint to enable you to read device-to-cloud messages.</span></span> <span data-ttu-id="787ea-119">För att göra det så enkelt som möjligt skapar vi en grundläggande läsare i den härs självstudiekursen som inte passar för distributioner med hög genomströmning.</span><span class="sxs-lookup"><span data-stu-id="787ea-119">To keep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="787ea-120">I självstudiekursen [Bearbeta meddelanden från enhet till moln][lnk-process-d2c-tutorial] står det hur du bearbetar ”enhet till molnet”-meddelanden i hög skala.</span><span class="sxs-lookup"><span data-stu-id="787ea-120">To learn how to process device-to-cloud messages at scale, see the [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span> <span data-ttu-id="787ea-121">Mer information om hur du bearbetar meddelanden från Event Hubs finns i självstudiekursen [Komma igång med Event Hubs][lnk-eventhubs-tutorial].</span><span class="sxs-lookup"><span data-stu-id="787ea-121">For more information about how to process messages from Event Hubs, see the [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial.</span></span> <span data-ttu-id="787ea-122">(Den här självstudiekursen gäller Event Hub-kompatibla slutpunkter i IoT Hub.)</span><span class="sxs-lookup"><span data-stu-id="787ea-122">(This tutorial is applicable to the IoT Hub Event Hub-compatible endpoints.)</span></span>

> [!NOTE]
> <span data-ttu-id="787ea-123">Event Hub-kompatibla slutpunkter för läsning av meddelanden från enheter till molnet använder alltid AMQP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="787ea-123">The Event Hub-compatible endpoint for reading device-to-cloud messages always uses the AMQP protocol.</span></span>

1. <span data-ttu-id="787ea-124">I Visual Studio lägger du till ett Visual C# Classic Desktop-projekt i den aktuella lösningen med hjälp av projektmallen **Console App (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="787ea-124">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution, by using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="787ea-125">Kontrollera att .NET Framework-versionen är 4.5.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="787ea-125">Make sure the .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="787ea-126">Ge projektet namnet **ReadDeviceToCloudMessages**.</span><span class="sxs-lookup"><span data-stu-id="787ea-126">Name the project **ReadDeviceToCloudMessages**.</span></span>

    ![Nytt Visual C# Windows Classic Desktop-projekt][10a]

2. <span data-ttu-id="787ea-128">Högerklicka i Solution Explorer på projektet **ReadDeviceToCloudMessages** och klicka sedan på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="787ea-128">In Solution Explorer, right-click the **ReadDeviceToCloudMessages** project, and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="787ea-129">I fönstret för **NuGet-pakethanteraren** letar du upp **WindowsAzure.ServiceBus**, väljer **Installera** och godkänner användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="787ea-129">In the **NuGet Package Manager** window, search for **WindowsAzure.ServiceBus**, select **Install**, and accept the terms of use.</span></span> <span data-ttu-id="787ea-130">Denna procedur hämtar, installerar och lägger till en referens för [Azure Service Bus][lnk-servicebus-nuget], med alla dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="787ea-130">This procedure downloads, installs, and adds a reference to [Azure Service Bus][lnk-servicebus-nuget], with all its dependencies.</span></span> <span data-ttu-id="787ea-131">Det här paketet gör att programmet kan ansluta till Event Hub-kompatibla slutpunkter i din IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="787ea-131">This package enables the application to connect to the Event Hub-compatible endpoint on your IoT hub.</span></span>

4. <span data-ttu-id="787ea-132">Lägg till följande `using`-uttryck överst i **Program.cs**-filen:</span><span class="sxs-lookup"><span data-stu-id="787ea-132">Add the following `using` statements at the top of the **Program.cs** file:</span></span>

    ```csharp
    using Microsoft.ServiceBus.Messaging;
    using System.Threading;
    ```

5. <span data-ttu-id="787ea-133">Lägg till följande fält i klassen **Program**.</span><span class="sxs-lookup"><span data-stu-id="787ea-133">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="787ea-134">Ersätt platshållarvärdet med IoT Hub-anslutningssträngen som du skapade i avsnittet ”Skapa en IoT Hub”.</span><span class="sxs-lookup"><span data-stu-id="787ea-134">Replace the placeholder value with the IoT Hub connection string for the hub you created in the "Create an IoT hub" section.</span></span>

    ```csharp
    static string connectionString = "{iothub connection string}";
    static string iotHubD2cEndpoint = "messages/events";
    static EventHubClient eventHubClient;
    ```

6. <span data-ttu-id="787ea-135">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="787ea-135">Add the following method to the **Program** class:</span></span>

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

    <span data-ttu-id="787ea-136">Den här metoden använder en **EventHubReceiver**-instans för att ta emot meddelanden från alla IoT-hubbens ”enhet till molnet”-mottagarpartitioner.</span><span class="sxs-lookup"><span data-stu-id="787ea-136">This method uses an **EventHubReceiver** instance to receive messages from all the IoT hub device-to-cloud receive partitions.</span></span> <span data-ttu-id="787ea-137">Observera att du skickar en `DateTime.Now`-parameter när du skapar **EventHubReceiver**-objektet, så att det bara tar emot meddelanden när det har startat.</span><span class="sxs-lookup"><span data-stu-id="787ea-137">Notice how you pass a `DateTime.Now` parameter when you create the **EventHubReceiver** object, so that it only receives messages sent after it starts.</span></span> <span data-ttu-id="787ea-138">Detta filter är användbart i en testmiljö så att du kan se den aktuella uppsättningen meddelanden.</span><span class="sxs-lookup"><span data-stu-id="787ea-138">This filter is useful in a test environment so you can see the current set of messages.</span></span> <span data-ttu-id="787ea-139">I en produktionsmiljö ska din kod kontrollera att den bearbetar alla meddelanden.</span><span class="sxs-lookup"><span data-stu-id="787ea-139">In a production environment, your code should make sure that it processes all the messages.</span></span> <span data-ttu-id="787ea-140">Mer information finns i självstudiekursen [Behandla meddelanden från enheten till molnet i IoT Hub][lnk-process-d2c-tutorial].</span><span class="sxs-lookup"><span data-stu-id="787ea-140">For more information, see the tutorial [How to process IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial].</span></span>

7. <span data-ttu-id="787ea-141">Slutligen lägger du till följande rader till **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="787ea-141">Finally, add the following lines to the **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
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

## <a name="create-a-device-app"></a><span data-ttu-id="787ea-142">Skapa en enhetsapp</span><span class="sxs-lookup"><span data-stu-id="787ea-142">Create a device app</span></span>

<span data-ttu-id="787ea-143">I det här avsnittet ska du skapa en .NET-konsolapp som simulerar en enhet som skickar ”enhet till molnet”-meddelanden till en IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="787ea-143">In this section, you create a .NET console app that simulates a device that sends device-to-cloud messages to an IoT hub.</span></span>

1. <span data-ttu-id="787ea-144">I Visual Studio lägger du till ett Visual C# Classic Desktop-projekt i den aktuella lösningen med hjälp av projektmallen **Console App (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="787ea-144">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution, by using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="787ea-145">Kontrollera att .NET Framework-versionen är 4.5.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="787ea-145">Make sure the .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="787ea-146">Ge projektet namnet **SimulatedDevice**.</span><span class="sxs-lookup"><span data-stu-id="787ea-146">Name the project **SimulatedDevice**.</span></span>

    ![Nytt Visual C# Windows Classic Desktop-projekt][10b]

2. <span data-ttu-id="787ea-148">Högerklicka på projektet **SimulatedDevice** i Solution Explorer och klicka sedan på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="787ea-148">In Solution Explorer, right-click the **SimulatedDevice** project, and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="787ea-149">Välj **Bläddra** i fönstret för **NuGet-pakethanteraren**, leta upp **Microsoft.Azure.Devices.Client**, välj **Installera** för att installera **Microsoft.Azure.Devices.Client**-paketet och godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="787ea-149">In the **NuGet Package Manager** window, select **Browse**, search for **Microsoft.Azure.Devices.Client**, select **Install** to install the **Microsoft.Azure.Devices.Client** package, and accept the terms of use.</span></span> <span data-ttu-id="787ea-150">Denna procedur hämtar, installerar och lägger till en referens för [NuGet-paketet SDK för Azure IoT-enheter][lnk-device-nuget] och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="787ea-150">This procedure downloads, installs, and adds a reference to the [Azure IoT device SDK NuGet package][lnk-device-nuget] and its dependencies.</span></span>

4. <span data-ttu-id="787ea-151">Lägg till följande `using`-instruktion högst upp i filen **Program.cs**:</span><span class="sxs-lookup"><span data-stu-id="787ea-151">Add the following `using` statement at the top of the **Program.cs** file:</span></span>

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Newtonsoft.Json;
    ```

5. <span data-ttu-id="787ea-152">Lägg till följande fält i klassen **Program**.</span><span class="sxs-lookup"><span data-stu-id="787ea-152">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="787ea-153">Ersätt `{iot hub hostname}` med det IoT Hub-värdnamn som du hämtade i avsnittet Skapa en IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="787ea-153">Substitute `{iot hub hostname}` with the IoT hub host name you retrieved in the "Create an IoT hub" section.</span></span> <span data-ttu-id="787ea-154">Ersätt `{device key}` med den enhetsnyckel som du hämtade i avsnittet Skapa en enhetsidentitet.</span><span class="sxs-lookup"><span data-stu-id="787ea-154">Substitute `{device key}` with the device key you retrieved in the "Create a device identity" section.</span></span>

    ```csharp
    static DeviceClient deviceClient;
    static string iotHubUri = "{iot hub hostname}";
    static string deviceKey = "{device key}";
    ```

6. <span data-ttu-id="787ea-155">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="787ea-155">Add the following method to the **Program** class:</span></span>

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

    <span data-ttu-id="787ea-156">Den här metoden skickar ett nytt ”enhet till molnet”-meddelande varje sekund.</span><span class="sxs-lookup"><span data-stu-id="787ea-156">This method sends a new device-to-cloud message every second.</span></span> <span data-ttu-id="787ea-157">Meddelandet innehåller ett JSON-serialiserat objekt med enhets-ID:t och ett slumpmässigt genererat nummer för att simulera en temperatursensor och en fuktighetssensor.</span><span class="sxs-lookup"><span data-stu-id="787ea-157">The message contains a JSON-serialized object, with the device ID and randomly generated numbers to simulate a temperature sensor, and a humidity sensor.</span></span>

7. <span data-ttu-id="787ea-158">Slutligen lägger du till följande rader till **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="787ea-158">Finally, add the following lines to the **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Simulated device\n");
    deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey ("myFirstDevice", deviceKey), TransportType.Mqtt);

    SendDeviceToCloudMessagesAsync();
    Console.ReadLine();
    ```

    <span data-ttu-id="787ea-159">Som standard skapar metoden **Create** i en .NET Framework-app en **DeviceClient**-instans som använder AMQP-protokollet för att kommunicera med IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="787ea-159">By default, the **Create** method in a .NET Framework app creates a **DeviceClient** instance that uses the AMQP protocol to communicate with IoT Hub.</span></span> <span data-ttu-id="787ea-160">Om du vill använda MQTT eller HTTP-protokollet använder du åsidosättandet av **Create**-metoden som gör att du kan ange protokollet.</span><span class="sxs-lookup"><span data-stu-id="787ea-160">To use the MQTT or HTTP protocol, use the override of the **Create** method that enables you to specify the protocol.</span></span> <span data-ttu-id="787ea-161">UWP- och PCL-klienter använder HTTP-protokoll som standard.</span><span class="sxs-lookup"><span data-stu-id="787ea-161">UWP and PCL clients use the HTTP protocol by default.</span></span> <span data-ttu-id="787ea-162">Om du använder HTTP-protokollet bör du också ta med namnområdet **System.Net.Http.Formatting** genom att lägga till NuGet-paketet **Microsoft.AspNet.WebApi.Client** till ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="787ea-162">If you use the HTTP protocol, you should also add the **Microsoft.AspNet.WebApi.Client** NuGet package to your project to include the **System.Net.Http.Formatting** namespace.</span></span>

<span data-ttu-id="787ea-163">Den här självstudiekursen beskriver steg för steg hur du skapar en IoT Hub-app.</span><span class="sxs-lookup"><span data-stu-id="787ea-163">This tutorial takes you through the steps to create an IoT Hub device app.</span></span> <span data-ttu-id="787ea-164">Du kan också lägga till nödvändig kod i enhetsprogrammet med hjälp av Visual Studio-tillägget [Ansluten tjänst för Azure IoT Hub][lnk-connected-service].</span><span class="sxs-lookup"><span data-stu-id="787ea-164">You can also use the [Connected Service for Azure IoT Hub][lnk-connected-service] Visual Studio extension to add the necessary code to your device app.</span></span>

> [!NOTE]
> <span data-ttu-id="787ea-165">För att göra det så enkelt som möjligt implementerar vi ingen princip för omförsök i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="787ea-165">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="787ea-166">I produktionskoden bör du implementera principer för omförsök (till exempel en exponentiell backoff), vilket rekommenderas i MSDN-artikeln om [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="787ea-166">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-the-apps"></a><span data-ttu-id="787ea-167">Kör apparna</span><span class="sxs-lookup"><span data-stu-id="787ea-167">Run the apps</span></span>

<span data-ttu-id="787ea-168">Nu är det dags att köra apparna.</span><span class="sxs-lookup"><span data-stu-id="787ea-168">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="787ea-169">Högerklicka på din lösning i Solution Explorer i Visual Studio och klicka sedan på **Ange startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="787ea-169">In Visual Studio, in Solution Explorer, right-click your solution, and then click **Set StartUp projects**.</span></span> <span data-ttu-id="787ea-170">Välj **Flera startprojekt** och välj sedan **Start** som åtgärden för både **ReadDeviceToCloudMessages**- och **SimulatedDevice**-projektet.</span><span class="sxs-lookup"><span data-stu-id="787ea-170">Select **Multiple startup projects**, and then select **Start** as the action for both the **ReadDeviceToCloudMessages** and **SimulatedDevice** projects.</span></span>

    ![Egenskaper för startprojekt][41]

2. <span data-ttu-id="787ea-172">Starta båda apparna genom att trycka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="787ea-172">Press **F5** to start both apps running.</span></span> <span data-ttu-id="787ea-173">Konsolens utdata från appen **SimulatedDevice** visar meddelandena som din enhetsapp skickar till IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="787ea-173">The console output from the **SimulatedDevice** app shows the messages your device app sends to your IoT hub.</span></span> <span data-ttu-id="787ea-174">Konsolens utdata från appen **ReadDeviceToCloudMessages** visar meddelandena som din IoT Hub tar emot.</span><span class="sxs-lookup"><span data-stu-id="787ea-174">The console output from the **ReadDeviceToCloudMessages** app shows the messages that your IoT hub receives.</span></span>

    ![Konsolens utdata från appar][42]

3. <span data-ttu-id="787ea-176">På panelen **Användning** på [Azure Portal][lnk-portal] kan du se hur många meddelanden som har skickats till IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="787ea-176">The **Usage** tile in the [Azure portal][lnk-portal] shows the number of messages sent to the IoT hub:</span></span>

    ![Panelen Användning på Azure Portal][43]

## <a name="next-steps"></a><span data-ttu-id="787ea-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="787ea-178">Next steps</span></span>

<span data-ttu-id="787ea-179">I de här självstudierna konfigurerade du en IoT Hub på Azure Portal och skapade sedan en enhetsidentitet i IoT-hubbens identitetsregister.</span><span class="sxs-lookup"><span data-stu-id="787ea-179">In this tutorial, you configured an IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="787ea-180">Du använde den här enhetsidentiteten så att enhetsappen kunde skicka ”enhet till molnet”-meddelanden till IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="787ea-180">You used this device identity to enable the device app to send device-to-cloud messages to the IoT hub.</span></span> <span data-ttu-id="787ea-181">Du skapade också en app som visar meddelandena som tagits emot av IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="787ea-181">You also created an app that displays the messages received by the IoT hub.</span></span>

<span data-ttu-id="787ea-182">Mer information om hur du kan komma igång med IoT Hub och utforska andra IoT-scenarier finns här:</span><span class="sxs-lookup"><span data-stu-id="787ea-182">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

* <span data-ttu-id="787ea-183">[Connecting your device][lnk-connect-device] (Ansluta din enhet)</span><span class="sxs-lookup"><span data-stu-id="787ea-183">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="787ea-184">[Connecting your device][lnk-device-management] (Komma igång med enhetshantering)</span><span class="sxs-lookup"><span data-stu-id="787ea-184">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="787ea-185">[Komma igång med IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="787ea-185">[Getting started with IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="787ea-186">Självstudiekursen [Bearbeta meddelanden från enhet till moln][lnk-process-d2c-tutorial] beskriver hur du utökar din IoT-lösning och bearbetar ”enhet till molnet”-meddelanden i hög skala.</span><span class="sxs-lookup"><span data-stu-id="787ea-186">To learn how to extend your IoT solution and process device-to-cloud messages at scale, see the [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>

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
