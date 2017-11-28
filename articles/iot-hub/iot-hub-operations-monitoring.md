---
title: "aaaAzure IoT-hubb operations övervakning | Microsoft Docs"
description: "Hur toouse Azure IoT Hub operations övervakning toomonitor hello status för åtgärder för din IoT-hubb i realtid."
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
ms.openlocfilehash: a0b233ef2d9bd0827e19fa30fdbdd49b2b61b813
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="iot-hub-operations-monitoring"></a><span data-ttu-id="4bbd9-103">IoT-hubb operations övervakning</span><span class="sxs-lookup"><span data-stu-id="4bbd9-103">IoT Hub operations monitoring</span></span>

<span data-ttu-id="4bbd9-104">IoT-hubb operations övervakning kan du toomonitor hello status för åtgärder för din IoT-hubb i realtid.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-104">IoT Hub operations monitoring enables you toomonitor hello status of operations on your IoT hub in real time.</span></span> <span data-ttu-id="4bbd9-105">IoT-hubb spårar händelser över flera kategorier av åtgärder.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-105">IoT Hub tracks events across several categories of operations.</span></span> <span data-ttu-id="4bbd9-106">Du kan välja att skicka händelser från en eller flera kategorier tooan slutpunkten för din IoT-hubb för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-106">You can opt into sending events from one or more categories tooan endpoint of your IoT hub for processing.</span></span> <span data-ttu-id="4bbd9-107">Du kan övervaka hello data för fel eller ställa in mer komplexa bearbetning baserat på datamönster.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-107">You can monitor hello data for errors or set up more complex processing based on data patterns.</span></span>

<span data-ttu-id="4bbd9-108">IoT-hubb övervakar sex kategorier av händelser:</span><span class="sxs-lookup"><span data-stu-id="4bbd9-108">IoT Hub monitors six categories of events:</span></span>

* <span data-ttu-id="4bbd9-109">Enhetens identitet åtgärder</span><span class="sxs-lookup"><span data-stu-id="4bbd9-109">Device identity operations</span></span>
* <span data-ttu-id="4bbd9-110">Enhetstelemetrin</span><span class="sxs-lookup"><span data-stu-id="4bbd9-110">Device telemetry</span></span>
* <span data-ttu-id="4bbd9-111">Meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="4bbd9-111">Cloud-to-device messages</span></span>
* <span data-ttu-id="4bbd9-112">Anslutningar</span><span class="sxs-lookup"><span data-stu-id="4bbd9-112">Connections</span></span>
* <span data-ttu-id="4bbd9-113">Filöverföringar</span><span class="sxs-lookup"><span data-stu-id="4bbd9-113">File uploads</span></span>
* <span data-ttu-id="4bbd9-114">Meddelanderoutning</span><span class="sxs-lookup"><span data-stu-id="4bbd9-114">Message routing</span></span>

## <a name="how-tooenable-operations-monitoring"></a><span data-ttu-id="4bbd9-115">Hur tooenable operations övervakning</span><span class="sxs-lookup"><span data-stu-id="4bbd9-115">How tooenable operations monitoring</span></span>

1. <span data-ttu-id="4bbd9-116">Skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-116">Create an IoT hub.</span></span> <span data-ttu-id="4bbd9-117">Du hittar anvisningar om hur toocreate en IoT-hubb i hello [Kom igång] [ lnk-get-started] guide.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-117">You can find instructions on how toocreate an IoT hub in hello [Get Started][lnk-get-started] guide.</span></span>

1. <span data-ttu-id="4bbd9-118">Öppna hello bladet för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-118">Open hello blade of your IoT hub.</span></span> <span data-ttu-id="4bbd9-119">Därifrån klickar du på **Operations övervakning**.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-119">From there, click **Operations monitoring**.</span></span>

    ![Åtkomståtgärder övervakning konfigurationen hello-portalen][1]

1. <span data-ttu-id="4bbd9-121">Välj hello övervakning av du vill toomonitor och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-121">Select hello monitoring categories you wish toomonitor, and then click **Save**.</span></span> <span data-ttu-id="4bbd9-122">hello händelser som är tillgängliga för att läsa från hello Event Hub-kompatibel slutpunkt som anges i **övervakningsinställningarna**.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-122">hello events are available for reading from hello Event Hub-compatible endpoint listed in **Monitoring settings**.</span></span> <span data-ttu-id="4bbd9-123">Hej IoT-hubb kallas `messages/operationsmonitoringevents`.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-123">hello IoT Hub endpoint is called `messages/operationsmonitoringevents`.</span></span>

    ![Konfigurera operations övervakning på din IoT-hubb][2]

> [!NOTE]
> <span data-ttu-id="4bbd9-125">Att välja **utförlig** övervakning för hello **anslutningar** kategori orsakar IoT-hubb toogenerate ytterligare diagnostiska meddelanden.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-125">Selecting **Verbose** monitoring for hello **Connections** category causes IoT Hub toogenerate additional diagnostics messages.</span></span> <span data-ttu-id="4bbd9-126">Alla kategorier, hello **utförlig** inställningsändringar hello mängd information IoT-hubb ingår i varje felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-126">For all other categories, hello **Verbose** setting changes hello quantity of information IoT Hub includes in each error message.</span></span>

## <a name="event-categories-and-how-toouse-them"></a><span data-ttu-id="4bbd9-127">Kategorier och hur toouse dem.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-127">Event categories and how toouse them</span></span>

<span data-ttu-id="4bbd9-128">Varje operations övervakning kategorin spårar en annan typ av interaktion med IoT-hubb och varje övervakning kategori har ett schema som definierar hur händelser i den kategorin är strukturerad.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-128">Each operations monitoring category tracks a different type of interaction with IoT Hub, and each monitoring category has a schema that defines how events in that category are structured.</span></span>

### <a name="device-identity-operations"></a><span data-ttu-id="4bbd9-129">Enhetens identitet åtgärder</span><span class="sxs-lookup"><span data-stu-id="4bbd9-129">Device identity operations</span></span>

<span data-ttu-id="4bbd9-130">hello enhetskategori identitet operations spårar fel som uppstår när du försöker toocreate, uppdatera eller ta bort en post i registret för din IoT-hubb identitet.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-130">hello device identity operations category tracks errors that occur when you attempt toocreate, update, or delete an entry in your IoT hub's identity registry.</span></span> <span data-ttu-id="4bbd9-131">Spårning av den här kategorin är användbart för att etablera scenarier.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-131">Tracking this category is useful for provisioning scenarios.</span></span>

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

### <a name="device-telemetry"></a><span data-ttu-id="4bbd9-132">Enhetstelemetrin</span><span class="sxs-lookup"><span data-stu-id="4bbd9-132">Device telemetry</span></span>

<span data-ttu-id="4bbd9-133">hello telemetri enhetskategori spårar fel som uppstår vid hello IoT-hubb och är relaterade toohello telemetri pipeline.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-133">hello device telemetry category tracks errors that occur at hello IoT hub and are related toohello telemetry pipeline.</span></span> <span data-ttu-id="4bbd9-134">Den här kategorin innehåller fel som uppstår när du skickar telemetriska händelser (till exempel begränsning) och ta emot telemetriska händelser (till exempel obehöriga läsare).</span><span class="sxs-lookup"><span data-stu-id="4bbd9-134">This category includes errors that occur when sending telemetry events (such as throttling) and receiving telemetry events (such as unauthorized reader).</span></span> <span data-ttu-id="4bbd9-135">Den här kategorin kan inte fånga fel som orsakats av kod som körs på själva hello-enheten.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-135">This category cannot catch errors caused by code running on hello device itself.</span></span>

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

### <a name="cloud-to-device-commands"></a><span data-ttu-id="4bbd9-136">Moln till enhet kommandon</span><span class="sxs-lookup"><span data-stu-id="4bbd9-136">Cloud-to-device commands</span></span>

<span data-ttu-id="4bbd9-137">hello moln till enhet kommandon kategorin spårar fel som uppstår vid hello IoT-hubb och är relaterade toohello moln-till-enhetsmeddelande pipeline.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-137">hello cloud-to-device commands category tracks errors that occur at hello IoT hub and are related toohello cloud-to-device message pipeline.</span></span> <span data-ttu-id="4bbd9-138">Den här kategorin innehåller fel som uppstår när meddelanden moln till enhet (till exempel obehöriga avsändaren), ta emot meddelanden moln till enhet (till exempel leverans för många) och ta emot meddelandet moln till enhet feedback (t ex feedback har upphört att gälla).</span><span class="sxs-lookup"><span data-stu-id="4bbd9-138">This category includes errors that occur when sending cloud-to-device messages (such as unauthorized sender), receiving cloud-to-device messages (such as delivery count exceeded), and receiving cloud-to-device message feedback (such as feedback expired).</span></span> <span data-ttu-id="4bbd9-139">Den här kategorin fånga inte fel från en enhet som hanterar ett moln till enhet meddelande felaktigt om moln till enhet hälsningsmeddelande levereras.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-139">This category does not catch errors from a device that improperly handles a cloud-to-device message if hello cloud-to-device message was delivered successfully.</span></span>

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

### <a name="connections"></a><span data-ttu-id="4bbd9-140">Anslutningar</span><span class="sxs-lookup"><span data-stu-id="4bbd9-140">Connections</span></span>

<span data-ttu-id="4bbd9-141">hello anslutningar kategorin spårar fel som uppstår när enheter ansluta eller koppla från en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-141">hello connections category tracks errors that occur when devices connect or disconnect from an IoT hub.</span></span> <span data-ttu-id="4bbd9-142">Spårning av den här kategorin är användbar för att identifiera obehöriga anslutningsförsök och för att spåra när anslutningen bryts för enheter i områden i dålig anslutning.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-142">Tracking this category is useful for identifying unauthorized connection attempts and for tracking when a connection is lost for devices in areas of poor connectivity.</span></span>

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

### <a name="file-uploads"></a><span data-ttu-id="4bbd9-143">Filöverföringar</span><span class="sxs-lookup"><span data-stu-id="4bbd9-143">File uploads</span></span>

<span data-ttu-id="4bbd9-144">hello filen överför kategorin spårar fel som uppstår vid hello IoT-hubb och är relaterade toofile överför funktioner.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-144">hello file upload category tracks errors that occur at hello IoT hub and are related toofile upload functionality.</span></span> <span data-ttu-id="4bbd9-145">Den här kategorin omfattar:</span><span class="sxs-lookup"><span data-stu-id="4bbd9-145">This category includes:</span></span>

* <span data-ttu-id="4bbd9-146">Fel som inträffar med hello SAS URI, till exempel när det upphör att gälla innan en enhet meddelar hello hubb för en överförda.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-146">Errors that occur with hello SAS URI, such as when it expires before a device notifies hello hub of a completed upload.</span></span>
* <span data-ttu-id="4bbd9-147">Det gick inte överföringar som rapporterats av hello enhet.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-147">Failed uploads reported by hello device.</span></span>
* <span data-ttu-id="4bbd9-148">Fel som uppstår när en fil inte hittas i lagringen under skapande av IoT-hubb notification meddelandet.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-148">Errors that occur when a file is not found in storage during IoT Hub notification message creation.</span></span>

<span data-ttu-id="4bbd9-149">Den här kategorin kan inte fånga fel som uppstår direkt medan hello enheten laddar upp en fil toostorage.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-149">This category cannot catch errors that directly occur while hello device is uploading a file toostorage.</span></span>

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

### <a name="message-routing"></a><span data-ttu-id="4bbd9-150">Meddelanderoutning</span><span class="sxs-lookup"><span data-stu-id="4bbd9-150">Message routing</span></span>

<span data-ttu-id="4bbd9-151">hello-meddelande routning kategorin spårar fel som inträffar när meddelandet väg utvärdering och slutpunkten hälsa som uppfattas av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-151">hello message routing category tracks errors that occur during message route evaluation and endpoint health as perceived by IoT Hub.</span></span> <span data-ttu-id="4bbd9-152">Den här kategorin innefattar händelser, t.ex. när en regel utvärderar för ”Odefinierad” när IoT-hubb markerar en slutpunkt som förlorade och andra fel togs emot från en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-152">This category includes events such as when a rule evaluates too"undefined", when IoT Hub marks an endpoint as dead, and any other errors received from an endpoint.</span></span> <span data-ttu-id="4bbd9-153">Den här kategorin omfattar inte felen om hälsningsmeddelande för sig (t.ex enheten begränsning fel) som har rapporterats under hello ”enhetstelemetrin” kategori.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-153">This category does not include specific errors about hello messages themselves (such as device throttling errors), which are reported under hello "device telemetry" category.</span></span>

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

## <a name="view-events"></a><span data-ttu-id="4bbd9-154">Visa händelser</span><span class="sxs-lookup"><span data-stu-id="4bbd9-154">View events</span></span>

<span data-ttu-id="4bbd9-155">Du kan använda hello *iothub explorer* verktyget tooquickly testa att din IoT-hubb genererar övervakning händelser.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-155">You can use hello *iothub-explorer* tool tooquickly test that your IoT hub is generating monitoring events.</span></span> <span data-ttu-id="4bbd9-156">tooinstall hello verktyget, se hello instruktionerna i hello [iothub explorer] [ lnk-iothub-explorer] GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-156">tooinstall hello tool, see hello instructions in hello [iothub-explorer][lnk-iothub-explorer] GitHub repository.</span></span>

1. <span data-ttu-id="4bbd9-157">Se till att hello **anslutningar** övervakning kategori har angetts för**utförlig** hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-157">Make sure hello **Connections** monitoring category is set too**Verbose** in hello portal.</span></span>

1. <span data-ttu-id="4bbd9-158">Kör hello efter kommandot tooread från hello övervakning slutpunkt i en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="4bbd9-158">At a command-prompt, run hello following command tooread from hello monitoring endpoint:</span></span>

    ```
    iothub-explorer monitor-ops --login {your iothubowner connection string}
    ```

1. <span data-ttu-id="4bbd9-159">Kör följande kommando toosimulate hello en enhet som skickar meddelanden från enhet till moln i en annan kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="4bbd9-159">In another command-prompt, run hello following command toosimulate a device sending device-to-cloud messages:</span></span>

    ```
    iothub-explorer simulate-device {your device name} --send "My test message" --login {your iothubowner connection string}
    ```

1. <span data-ttu-id="4bbd9-160">hello första kommandotolk visar hello övervakning händelser som hello simulerade enheten ansluter tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-160">hello first command-prompt shows hello monitoring events as hello simulated device connects tooyour IoT hub.</span></span>

## <a name="connect-toohello-monitoring-endpoint"></a><span data-ttu-id="4bbd9-161">Ansluta toohello övervakning slutpunkt</span><span class="sxs-lookup"><span data-stu-id="4bbd9-161">Connect toohello monitoring endpoint</span></span>

<span data-ttu-id="4bbd9-162">hello övervakning slutpunkt på din IoT-hubb är Event Hub-kompatibel slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-162">hello monitoring endpoint on your IoT hub is an Event Hub-compatible endpoint.</span></span> <span data-ttu-id="4bbd9-163">Du kan använda någon mekanism som fungerar med Händelsehubbar tooread övervakning meddelanden från den här slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-163">You can use any mechanism that works with Event Hubs tooread monitoring messages from this endpoint.</span></span> <span data-ttu-id="4bbd9-164">hello skapar följande exempel en grundläggande läsare som inte lämpar sig för en distribution med hög genomströmning.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-164">hello following sample creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="4bbd9-165">Mer information om hur tooprocess meddelanden från Händelsehubbar finns hello [Kom igång med Händelsehubbar] [ lnk-eventhubs-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-165">For more information about how tooprocess messages from Event Hubs, see hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial.</span></span>

<span data-ttu-id="4bbd9-166">tooconnect toohello övervakning slutpunkt, behöver du en sträng och hello endpoint anslutningsnamn.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-166">tooconnect toohello monitoring endpoint, you need a connection string and hello endpoint name.</span></span> <span data-ttu-id="4bbd9-167">hello följande steg visar hur toofind hello nödvändiga värden i hello portal:</span><span class="sxs-lookup"><span data-stu-id="4bbd9-167">hello following steps show you how toofind hello necessary values in hello portal:</span></span>

1. <span data-ttu-id="4bbd9-168">Navigera tooyour resursbladet för IoT-hubb i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-168">In hello portal, navigate tooyour IoT Hub resource blade.</span></span>

1. <span data-ttu-id="4bbd9-169">Välj **Operations övervakning**, och anteckna hello **Event Hub-kompatibelt namn** och **Event Hub-kompatibel endpoint** värden:</span><span class="sxs-lookup"><span data-stu-id="4bbd9-169">Choose **Operations monitoring**, and make a note of hello **Event Hub-compatible name** and **Event Hub-compatible endpoint** values:</span></span>

    ![Event Hub-kompatibel endpoint värden][img-endpoints]

1. <span data-ttu-id="4bbd9-171">Välj **principer för delad åtkomst**, Välj **tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-171">Choose **Shared access policies**, then choose **service**.</span></span> <span data-ttu-id="4bbd9-172">Anteckna hello **primärnyckel** värde:</span><span class="sxs-lookup"><span data-stu-id="4bbd9-172">Make a note of hello **Primary key** value:</span></span>

    ![Primär nyckel för tjänsten delad åtkomst-princip][img-service-key]

<span data-ttu-id="4bbd9-174">hello följande kodexempel för C# hämtas från Visual Studio **klassiska Windows-skrivbordet** C#-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-174">hello following C# code sample is taken from a Visual Studio **Windows Classic Desktop** C# console app.</span></span> <span data-ttu-id="4bbd9-175">hello-projekt har hello **WindowsAzure.ServiceBus** NuGet-paketet installeras.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-175">hello project has hello **WindowsAzure.ServiceBus** NuGet package installed.</span></span>

* <span data-ttu-id="4bbd9-176">Ersätt hello anslutning sträng platshållaren med en anslutningssträng som använder hello **Event Hub-kompatibel endpoint** och tjänsten **primärnyckel** värden som du antecknade tidigare enligt följande hello Exempel:</span><span class="sxs-lookup"><span data-stu-id="4bbd9-176">Replace hello connection string placeholder with a connection string that uses hello **Event Hub-compatible endpoint** and service **Primary key** values you noted previously as shown in hello following example:</span></span>

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* <span data-ttu-id="4bbd9-177">Ersätt hello övervakning endpoint platshållare med hello **Event Hub-kompatibelt namn** värdet som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="4bbd9-177">Replace hello monitoring endpoint name placeholder with hello **Event Hub-compatible name** value you noted previously.</span></span>

```cs
class Program
{
    static string connectionString = "{your monitoring endpoint connection string}";
    static string monitoringEndpointName = "{your monitoring endpoint name}";
    static EventHubClient eventHubClient;

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key tooexit.\n");

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

## <a name="next-steps"></a><span data-ttu-id="4bbd9-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4bbd9-178">Next steps</span></span>
<span data-ttu-id="4bbd9-179">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="4bbd9-179">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="4bbd9-180">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="4bbd9-180">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="4bbd9-181">[Simulera en enhet med Azure IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="4bbd9-181">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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