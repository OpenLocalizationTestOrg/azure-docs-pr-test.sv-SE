---
title: "Åtgärder i Azure IoT-hubb övervakning | Microsoft Docs"
description: "Hur du använder Azure IoT Hub operations övervakning för att övervaka status för åtgärder för din IoT-hubb i realtid."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a299f3a5-b14d-4586-9c3b-44aea14ed013
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.openlocfilehash: b6de5c5df5f9401a41be152bfa06eb994594e83d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="iot-hub-operations-monitoring"></a><span data-ttu-id="d2d0b-103">IoT-hubb operations övervakning</span><span class="sxs-lookup"><span data-stu-id="d2d0b-103">IoT Hub operations monitoring</span></span>

<span data-ttu-id="d2d0b-104">IoT-hubb operations övervakning kan du övervaka status för åtgärder för din IoT-hubb i realtid.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-104">IoT Hub operations monitoring enables you to monitor the status of operations on your IoT hub in real time.</span></span> <span data-ttu-id="d2d0b-105">IoT-hubb spårar händelser över flera kategorier av åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-105">IoT Hub tracks events across several categories of operations.</span></span> <span data-ttu-id="d2d0b-106">Du kan välja att skicka händelser från en eller flera kategorier för en slutpunkt av din IoT-hubb för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-106">You can opt into sending events from one or more categories to an endpoint of your IoT hub for processing.</span></span> <span data-ttu-id="d2d0b-107">Du kan övervaka data för fel eller ställa in mer komplexa bearbetning baserat på datamönster.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-107">You can monitor the data for errors or set up more complex processing based on data patterns.</span></span>

<span data-ttu-id="d2d0b-108">IoT-hubb övervakar sex kategorier av händelser:</span><span class="sxs-lookup"><span data-stu-id="d2d0b-108">IoT Hub monitors six categories of events:</span></span>

* <span data-ttu-id="d2d0b-109">Enhetens identitet åtgärder</span><span class="sxs-lookup"><span data-stu-id="d2d0b-109">Device identity operations</span></span>
* <span data-ttu-id="d2d0b-110">Enhetstelemetrin</span><span class="sxs-lookup"><span data-stu-id="d2d0b-110">Device telemetry</span></span>
* <span data-ttu-id="d2d0b-111">Meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="d2d0b-111">Cloud-to-device messages</span></span>
* <span data-ttu-id="d2d0b-112">Anslutningar</span><span class="sxs-lookup"><span data-stu-id="d2d0b-112">Connections</span></span>
* <span data-ttu-id="d2d0b-113">Filöverföringar</span><span class="sxs-lookup"><span data-stu-id="d2d0b-113">File uploads</span></span>
* <span data-ttu-id="d2d0b-114">Meddelanderoutning</span><span class="sxs-lookup"><span data-stu-id="d2d0b-114">Message routing</span></span>

## <a name="how-to-enable-operations-monitoring"></a><span data-ttu-id="d2d0b-115">Så här aktiverar du operations övervakning</span><span class="sxs-lookup"><span data-stu-id="d2d0b-115">How to enable operations monitoring</span></span>

1. <span data-ttu-id="d2d0b-116">Skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-116">Create an IoT hub.</span></span> <span data-ttu-id="d2d0b-117">Du hittar anvisningar om hur du skapar en IoT-hubb i den [Kom igång] [ lnk-get-started] guide.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-117">You can find instructions on how to create an IoT hub in the [Get Started][lnk-get-started] guide.</span></span>

1. <span data-ttu-id="d2d0b-118">Öppna bladet för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-118">Open the blade of your IoT hub.</span></span> <span data-ttu-id="d2d0b-119">Därifrån klickar du på **Operations övervakning**.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-119">From there, click **Operations monitoring**.</span></span>

    ![Åtkomståtgärder övervakning konfigurationen för portalen][1]

1. <span data-ttu-id="d2d0b-121">Välj övervakning kategorier som du vill övervaka och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-121">Select the monitoring categories you wish to monitor, and then click **Save**.</span></span> <span data-ttu-id="d2d0b-122">Händelserna som är tillgängliga för att läsa från Event Hub-kompatibel slutpunkten som anges i **övervakningsinställningarna**.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-122">The events are available for reading from the Event Hub-compatible endpoint listed in **Monitoring settings**.</span></span> <span data-ttu-id="d2d0b-123">IoT-hubb-slutpunkt anropas `messages/operationsmonitoringevents`.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-123">The IoT Hub endpoint is called `messages/operationsmonitoringevents`.</span></span>

    ![Konfigurera operations övervakning på din IoT-hubb][2]

> [!NOTE]
> <span data-ttu-id="d2d0b-125">Att välja **utförlig** övervakning för den **anslutningar** kategori orsakar IoT-hubb för att generera diagnostikmeddelanden för ytterligare.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-125">Selecting **Verbose** monitoring for the **Connections** category causes IoT Hub to generate additional diagnostics messages.</span></span> <span data-ttu-id="d2d0b-126">För alla kategorier, den **utförlig** inställningsändringar mängd information IoT-hubb ingår i varje felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-126">For all other categories, the **Verbose** setting changes the quantity of information IoT Hub includes in each error message.</span></span>

## <a name="event-categories-and-how-to-use-them"></a><span data-ttu-id="d2d0b-127">Kategorier och hur du använder dem.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-127">Event categories and how to use them</span></span>

<span data-ttu-id="d2d0b-128">Varje operations övervakning kategorin spårar en annan typ av interaktion med IoT-hubb och varje övervakning kategori har ett schema som definierar hur händelser i den kategorin är strukturerad.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-128">Each operations monitoring category tracks a different type of interaction with IoT Hub, and each monitoring category has a schema that defines how events in that category are structured.</span></span>

### <a name="device-identity-operations"></a><span data-ttu-id="d2d0b-129">Enhetens identitet åtgärder</span><span class="sxs-lookup"><span data-stu-id="d2d0b-129">Device identity operations</span></span>

<span data-ttu-id="d2d0b-130">Enhetskategori identitet operations spårar fel som uppstår vid försök att skapa, uppdatera eller ta bort en post i registret för din IoT-hubb identitet.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-130">The device identity operations category tracks errors that occur when you attempt to create, update, or delete an entry in your IoT hub's identity registry.</span></span> <span data-ttu-id="d2d0b-131">Spårning av den här kategorin är användbart för att etablera scenarier.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-131">Tracking this category is useful for provisioning scenarios.</span></span>

```json
{
    "time": "UTC timestamp",
        "operationName": "create",
        "category": "DeviceIdentityOperations",
        "level": "Error",
        "statusCode": 4XX,
        "statusDescription": "MessageDescription",
        "deviceId": "device-ID",
        "durationMs": 1234,
        "userAgent": "userAgent",
        "sharedAccessPolicy": "accessPolicy"
}
```

### <a name="device-telemetry"></a><span data-ttu-id="d2d0b-132">Enhetstelemetrin</span><span class="sxs-lookup"><span data-stu-id="d2d0b-132">Device telemetry</span></span>

<span data-ttu-id="d2d0b-133">Telemetri enhetskategori spårar fel som uppstår vid IoT-hubb och är relaterade till telemetri pipeline.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-133">The device telemetry category tracks errors that occur at the IoT hub and are related to the telemetry pipeline.</span></span> <span data-ttu-id="d2d0b-134">Den här kategorin innehåller fel som uppstår när du skickar telemetriska händelser (till exempel begränsning) och ta emot telemetriska händelser (till exempel obehöriga läsare).</span><span class="sxs-lookup"><span data-stu-id="d2d0b-134">This category includes errors that occur when sending telemetry events (such as throttling) and receiving telemetry events (such as unauthorized reader).</span></span> <span data-ttu-id="d2d0b-135">Den här kategorin kan inte fånga fel som orsakats av kod som körs på själva enheten.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-135">This category cannot catch errors caused by code running on the device itself.</span></span>

```json
{
        "messageSizeInBytes": 1234,
        "batching": 0,
        "protocol": "Amqp",
        "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
        "time": "UTC timestamp",
        "operationName": "ingress",
        "category": "DeviceTelemetry",
        "level": "Error",
        "statusCode": 4XX,
        "statusType": 4XX001,
        "statusDescription": "MessageDescription",
        "deviceId": "device-ID",
        "EventProcessedUtcTime": "UTC timestamp",
        "PartitionId": 1,
        "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="cloud-to-device-commands"></a><span data-ttu-id="d2d0b-136">Moln till enhet kommandon</span><span class="sxs-lookup"><span data-stu-id="d2d0b-136">Cloud-to-device commands</span></span>

<span data-ttu-id="d2d0b-137">Moln till enhet kommandon kategorin spårar fel som uppstår vid IoT-hubb och är relaterade till moln-till-enhetsmeddelande pipeline.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-137">The cloud-to-device commands category tracks errors that occur at the IoT hub and are related to the cloud-to-device message pipeline.</span></span> <span data-ttu-id="d2d0b-138">Den här kategorin innehåller fel som uppstår när meddelanden moln till enhet (till exempel obehöriga avsändaren), ta emot meddelanden moln till enhet (till exempel leverans för många) och ta emot meddelandet moln till enhet feedback (t ex feedback har upphört att gälla).</span><span class="sxs-lookup"><span data-stu-id="d2d0b-138">This category includes errors that occur when sending cloud-to-device messages (such as unauthorized sender), receiving cloud-to-device messages (such as delivery count exceeded), and receiving cloud-to-device message feedback (such as feedback expired).</span></span> <span data-ttu-id="d2d0b-139">Den här kategorin fånga inte fel från en enhet som hanterar ett moln till enhet meddelande felaktigt om moln till enhet meddelandet levererades har.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-139">This category does not catch errors from a device that improperly handles a cloud-to-device message if the cloud-to-device message was delivered successfully.</span></span>

```json
{
    "messageSizeInBytes": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "deliveryAcknowledgement": 0,
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "C2DCommands",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": “UTC timestamp"
}
```

### <a name="connections"></a><span data-ttu-id="d2d0b-140">Anslutningar</span><span class="sxs-lookup"><span data-stu-id="d2d0b-140">Connections</span></span>

<span data-ttu-id="d2d0b-141">Anslutningar kategorin spårar fel som uppstår när enheter ansluta eller koppla från en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-141">The connections category tracks errors that occur when devices connect or disconnect from an IoT hub.</span></span> <span data-ttu-id="d2d0b-142">Spårning av den här kategorin är användbar för att identifiera obehöriga anslutningsförsök och för att spåra när anslutningen bryts för enheter i områden i dålig anslutning.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-142">Tracking this category is useful for identifying unauthorized connection attempts and for tracking when a connection is lost for devices in areas of poor connectivity.</span></span>

```json
{
    "durationMs": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "deviceConnect",
    "category": "Connections",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID"
}
```

### <a name="file-uploads"></a><span data-ttu-id="d2d0b-143">Filöverföringar</span><span class="sxs-lookup"><span data-stu-id="d2d0b-143">File uploads</span></span>

<span data-ttu-id="d2d0b-144">Filen överför kategorin spårar fel som uppstår vid IoT-hubb och är relaterade till funktioner för överföring av filer.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-144">The file upload category tracks errors that occur at the IoT hub and are related to file upload functionality.</span></span> <span data-ttu-id="d2d0b-145">Den här kategorin omfattar:</span><span class="sxs-lookup"><span data-stu-id="d2d0b-145">This category includes:</span></span>

* <span data-ttu-id="d2d0b-146">Fel som inträffar med SAS-URI, till exempel när det upphör att gälla innan en enhet meddelar hubb för en överförda.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-146">Errors that occur with the SAS URI, such as when it expires before a device notifies the hub of a completed upload.</span></span>
* <span data-ttu-id="d2d0b-147">Det gick inte överföringar som rapporteras av enheten.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-147">Failed uploads reported by the device.</span></span>
* <span data-ttu-id="d2d0b-148">Fel som uppstår när en fil inte hittas i lagringen under skapande av IoT-hubb notification meddelandet.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-148">Errors that occur when a file is not found in storage during IoT Hub notification message creation.</span></span>

<span data-ttu-id="d2d0b-149">Den här kategorin kan inte fånga fel som uppstår direkt medan enheten är Överför en fil till lagring.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-149">This category cannot catch errors that directly occur while the device is uploading a file to storage.</span></span>

```json
{
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "HTTP",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "fileUpload",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "blobUri": "http//bloburi.com",
    "durationMs": 1234
}
```

### <a name="message-routing"></a><span data-ttu-id="d2d0b-150">Meddelanderoutning</span><span class="sxs-lookup"><span data-stu-id="d2d0b-150">Message routing</span></span>

<span data-ttu-id="d2d0b-151">Meddelandet routning kategorin spårar fel som inträffar när meddelandet väg utvärdering och slutpunkten hälsa som uppfattas av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-151">The message routing category tracks errors that occur during message route evaluation and endpoint health as perceived by IoT Hub.</span></span> <span data-ttu-id="d2d0b-152">Den här kategorin innefattar händelser, t.ex. när en regel som utvärderar till ”undefined”, när IoT-hubb markerar en slutpunkt som förlorade och andra fel togs emot från en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-152">This category includes events such as when a rule evaluates to "undefined", when IoT Hub marks an endpoint as dead, and any other errors received from an endpoint.</span></span> <span data-ttu-id="d2d0b-153">Den här kategorin omfattar inte felen om själva meddelandena (till exempel enhet begränsning fel) som har rapporterats under kategorin ”enhetstelemetrin”.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-153">This category does not include specific errors about the messages themselves (such as device throttling errors), which are reported under the "device telemetry" category.</span></span>

```json
{
    "messageSizeInBytes": 1234,
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "routes",
    "level": "Error",
    "deviceId": "device-ID",
    "messageId": "ID of message",
    "routeName": "myroute",
    "endpointName": "myendpoint",
    "details": "ExternalEndpointDisabled"
}
```

## <a name="view-events"></a><span data-ttu-id="d2d0b-154">Visa händelser</span><span class="sxs-lookup"><span data-stu-id="d2d0b-154">View events</span></span>

<span data-ttu-id="d2d0b-155">Du kan använda den *iothub explorer* verktyg för att snabbt testa att din IoT-hubb genererar övervakning händelser.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-155">You can use the *iothub-explorer* tool to quickly test that your IoT hub is generating monitoring events.</span></span> <span data-ttu-id="d2d0b-156">Om du vill installera verktyget, se anvisningarna i den [iothub explorer] [ lnk-iothub-explorer] GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-156">To install the tool, see the instructions in the [iothub-explorer][lnk-iothub-explorer] GitHub repository.</span></span>

1. <span data-ttu-id="d2d0b-157">Kontrollera att den **anslutningar** övervakning kategori är inställd på **utförlig** i portalen.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-157">Make sure the **Connections** monitoring category is set to **Verbose** in the portal.</span></span>

1. <span data-ttu-id="d2d0b-158">Kör följande kommando för att läsa från övervakning slutpunkten i en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="d2d0b-158">At a command-prompt, run the following command to read from the monitoring endpoint:</span></span>

    ```
    iothub-explorer monitor-ops --login {your iothubowner connection string}
    ```

1. <span data-ttu-id="d2d0b-159">Kör följande kommando för att simulera en enhet som skickar meddelanden från enhet till moln i en annan kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="d2d0b-159">In another command-prompt, run the following command to simulate a device sending device-to-cloud messages:</span></span>

    ```
    iothub-explorer simulate-device {your device name} --send "My test message" --login {your iothubowner connection string}
    ```

1. <span data-ttu-id="d2d0b-160">Första Kommandotolken visar händelser som övervakning som den simulerade enheten ansluter till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-160">The first command-prompt shows the monitoring events as the simulated device connects to your IoT hub.</span></span>

## <a name="connect-to-the-monitoring-endpoint"></a><span data-ttu-id="d2d0b-161">Ansluta till övervakning slutpunkt</span><span class="sxs-lookup"><span data-stu-id="d2d0b-161">Connect to the monitoring endpoint</span></span>

<span data-ttu-id="d2d0b-162">Övervakning slutpunkten på din IoT-hubb är Event Hub-kompatibel slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-162">The monitoring endpoint on your IoT hub is an Event Hub-compatible endpoint.</span></span> <span data-ttu-id="d2d0b-163">Du kan använda någon mekanism som fungerar med Händelsehubbar att läsa övervakning meddelanden från den här slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-163">You can use any mechanism that works with Event Hubs to read monitoring messages from this endpoint.</span></span> <span data-ttu-id="d2d0b-164">I följande exempel skapar en grundläggande läsare som inte lämpar sig för en distribution med hög genomströmning.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-164">The following sample creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="d2d0b-165">Mer information om hur du bearbetar meddelanden från Event Hubs finns i självstudiekursen [Komma igång med Event Hubs][lnk-eventhubs-tutorial].</span><span class="sxs-lookup"><span data-stu-id="d2d0b-165">For more information about how to process messages from Event Hubs, see the [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial.</span></span>

<span data-ttu-id="d2d0b-166">För att ansluta till slutpunkten för övervakning, behöver du en anslutningssträng och namnet på slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-166">To connect to the monitoring endpoint, you need a connection string and the endpoint name.</span></span> <span data-ttu-id="d2d0b-167">Följande steg visar hur du hittar de nödvändiga värdena i portalen:</span><span class="sxs-lookup"><span data-stu-id="d2d0b-167">The following steps show you how to find the necessary values in the portal:</span></span>

1. <span data-ttu-id="d2d0b-168">Gå till resursbladet för din IoT-hubb i portalen.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-168">In the portal, navigate to your IoT Hub resource blade.</span></span>

1. <span data-ttu-id="d2d0b-169">Välj **Operations övervakning**, och anteckna den **Event Hub-kompatibelt namn** och **Event Hub-kompatibel endpoint** värden:</span><span class="sxs-lookup"><span data-stu-id="d2d0b-169">Choose **Operations monitoring**, and make a note of the **Event Hub-compatible name** and **Event Hub-compatible endpoint** values:</span></span>

    ![Event Hub-kompatibel endpoint värden][img-endpoints]

1. <span data-ttu-id="d2d0b-171">Välj **principer för delad åtkomst**, Välj **tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-171">Choose **Shared access policies**, then choose **service**.</span></span> <span data-ttu-id="d2d0b-172">Anteckna den **primärnyckel** värde:</span><span class="sxs-lookup"><span data-stu-id="d2d0b-172">Make a note of the **Primary key** value:</span></span>

    ![Primär nyckel för tjänsten delad åtkomst-princip][img-service-key]

<span data-ttu-id="d2d0b-174">Följande C# kodexempel hämtas från Visual Studio **klassiska Windows-skrivbordet** C#-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-174">The following C# code sample is taken from a Visual Studio **Windows Classic Desktop** C# console app.</span></span> <span data-ttu-id="d2d0b-175">Projektet har den **WindowsAzure.ServiceBus** NuGet-paketet installeras.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-175">The project has the **WindowsAzure.ServiceBus** NuGet package installed.</span></span>

* <span data-ttu-id="d2d0b-176">Ersätt platshållaren för anslutningssträngen med en anslutningssträng som använder den **Event Hub-kompatibel endpoint** och tjänsten **primärnyckel** värden som du antecknade tidigare som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="d2d0b-176">Replace the connection string placeholder with a connection string that uses the **Event Hub-compatible endpoint** and service **Primary key** values you noted previously as shown in the following example:</span></span>

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* <span data-ttu-id="d2d0b-177">Ersätt platshållaren namn övervakning slutpunkten för med den **Event Hub-kompatibelt namn** värdet som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="d2d0b-177">Replace the monitoring endpoint name placeholder with the **Event Hub-compatible name** value you noted previously.</span></span>

```cs
class Program
{
    static string connectionString = "{your monitoring endpoint connection string}";
    static string monitoringEndpointName = "{your monitoring endpoint name}";
    static EventHubClient eventHubClient;

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;
        CancellationTokenSource cts = new CancellationTokenSource();
        var tasks = new List<Task>();

        foreach (string partition in d2cPartitions)
        {
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }

        Console.ReadLine();
        Console.WriteLine("Exiting...");
        cts.Cancel();
        Task.WaitAll(tasks.ToArray());
    }

    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested)
            {
                await eventHubReceiver.CloseAsync();
                break;
            }

            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10));

            if (eventData != null)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="d2d0b-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2d0b-178">Next steps</span></span>
<span data-ttu-id="d2d0b-179">Om du vill utforska ytterligare funktionerna i IoT-hubb, se:</span><span class="sxs-lookup"><span data-stu-id="d2d0b-179">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="d2d0b-180">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="d2d0b-180">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="d2d0b-181">[Simulera en enhet med Azure IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="d2d0b-181">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png
[img-endpoints]: media/iot-hub-operations-monitoring/monitoring-endpoint.png
[img-service-key]: media/iot-hub-operations-monitoring/service-key.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md