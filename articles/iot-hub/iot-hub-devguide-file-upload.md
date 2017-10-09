---
title: ladda upp aaaUnderstand Azure IoT Hub-filen | Microsoft Docs
description: "Utvecklarhandbok - Använd hello funktionen för överföringen av IoT-hubb toomanage överföra filer från en enhet tooan Azure storage blob-behållare."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a0427925-3e40-4fcd-96c1-2a31d1ddc14b
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: d44f9303ead4fa282dc0baf70887af1b8a03293d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-with-iot-hub"></a><span data-ttu-id="2bf32-103">Överföra filer med IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="2bf32-103">Upload files with IoT Hub</span></span>

<span data-ttu-id="2bf32-104">Detaljerad i hello [IoT-hubbslutpunkter] [ lnk-endpoints] artikel, en enhet kan initiera en filöverföring genom att skicka ett meddelande via en enhet riktade slutpunkt (**/devices/ {deviceId} / filer**).</span><span class="sxs-lookup"><span data-stu-id="2bf32-104">As detailed in hello [IoT Hub endpoints][lnk-endpoints] article, a device can initiate a file upload by sending a notification through a device-facing endpoint (**/devices/{deviceId}/files**).</span></span> <span data-ttu-id="2bf32-105">När en enhet meddelar IoT-hubb som en överföringen är klar, IoT-hubb skickas ett meddelande för filen överför via hello **/messages/servicebound/filenotifications** service-riktade slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="2bf32-105">When a device notifies IoT Hub that an upload is complete, IoT Hub sends a file upload notification message through hello **/messages/servicebound/filenotifications** service-facing endpoint.</span></span>

<span data-ttu-id="2bf32-106">I stället för förtroendeförmedling meddelanden via IoT-hubb själva fungerar IoT-hubb i stället som en dispatcher tooan som är associerade med Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2bf32-106">Instead of brokering messages through IoT Hub itself, IoT Hub instead acts as a dispatcher tooan associated Azure Storage account.</span></span> <span data-ttu-id="2bf32-107">En enhet begär en storage-token från IoT-hubb som är specifika toohello filen hello enhet önskar tooupload.</span><span class="sxs-lookup"><span data-stu-id="2bf32-107">A device requests a storage token from IoT Hub that is specific toohello file hello device wishes tooupload.</span></span> <span data-ttu-id="2bf32-108">hello enhet använder hello SAS URI tooupload hello filen toostorage och när hello överföringen är klar hello enheten skickar ett meddelande om slutförande tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="2bf32-108">hello device uses hello SAS URI tooupload hello file toostorage, and when hello upload is complete hello device sends a notification of completion tooIoT Hub.</span></span> <span data-ttu-id="2bf32-109">IoT-hubb kontrollerar hello filöverföring har slutförts och lägger sedan till en fil överför meddelandet toohello service-riktade filen notification aviseringsslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="2bf32-109">IoT Hub checks hello file upload is complete and then adds a file upload notification message toohello service-facing file notification endpoint.</span></span>

<span data-ttu-id="2bf32-110">Innan du laddar upp en fil tooIoT hubb från en enhet måste du konfigurera din hubb av [associera en Azure Storage] [ lnk-associate-storage] tooit-kontot.</span><span class="sxs-lookup"><span data-stu-id="2bf32-110">Before you upload a file tooIoT Hub from a device, you must configure your hub by [associating an Azure Storage][lnk-associate-storage] account tooit.</span></span>

<span data-ttu-id="2bf32-111">Enheten kan sedan [initiera en överföring] [ lnk-initialize] och sedan [meddela IoT-hubb] [ lnk-notify] när hello överföringen är klar.</span><span class="sxs-lookup"><span data-stu-id="2bf32-111">Your device can then [initialize an upload][lnk-initialize] and then [notify IoT hub][lnk-notify] when hello upload completes.</span></span> <span data-ttu-id="2bf32-112">Du kan också när en enhet meddelar IoT-hubb som hello överföringen är klar, hello-tjänsten kan skapa en [meddelande][lnk-service-notification].</span><span class="sxs-lookup"><span data-stu-id="2bf32-112">Optionally, when a device notifies IoT Hub that hello upload is complete, hello service can generate a [notification message][lnk-service-notification].</span></span>

### <a name="when-toouse"></a><span data-ttu-id="2bf32-113">När toouse</span><span class="sxs-lookup"><span data-stu-id="2bf32-113">When toouse</span></span>

<span data-ttu-id="2bf32-114">Använd överför toosend mediafiler och stora telemetri batchar har laddats upp av periodvis anslutna enheter eller komprimerade toosave bandbredd.</span><span class="sxs-lookup"><span data-stu-id="2bf32-114">Use file upload toosend media files and large telemetry batches uploaded by intermittently connected devices or compressed toosave bandwidth.</span></span>

<span data-ttu-id="2bf32-115">Se för[enhet till moln kommunikation vägledning] [ lnk-d2c-guidance] om osäkra mellan att använda rapporterade egenskaper, meddelanden från enhet till moln eller ladda upp filen.</span><span class="sxs-lookup"><span data-stu-id="2bf32-115">Refer too[Device-to-cloud communication guidance][lnk-d2c-guidance] if in doubt between using reported properties, device-to-cloud messages, or file upload.</span></span>

## <a name="associate-an-azure-storage-account-with-iot-hub"></a><span data-ttu-id="2bf32-116">Associera ett Azure Storage-konto med IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="2bf32-116">Associate an Azure Storage account with IoT Hub</span></span>

<span data-ttu-id="2bf32-117">toouse hello filen överför funktioner, måste du först koppla ett Azure Storage-konto toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2bf32-117">toouse hello file upload functionality, you must first link an Azure Storage account toohello IoT Hub.</span></span> <span data-ttu-id="2bf32-118">Du kan slutföra den här uppgiften via hello [Azure-portalen][lnk-management-portal], eller programmässigt via hello [IoT-hubb resursprovidern REST API: er] [ lnk-resource-provider-apis].</span><span class="sxs-lookup"><span data-stu-id="2bf32-118">You can complete this task either through hello [Azure portal][lnk-management-portal], or programmatically through hello [IoT Hub resource provider REST APIs][lnk-resource-provider-apis].</span></span> <span data-ttu-id="2bf32-119">När du har associerat ett Azure Storage-konto med IoT-hubben, returnerar hello-tjänsten en SAS-URI tooa enhet när hello enheten initierar en begäran om överföring.</span><span class="sxs-lookup"><span data-stu-id="2bf32-119">Once you have associated an Azure Storage account with your IoT Hub, hello service returns a SAS URI tooa device when hello device initiates a file upload request.</span></span>

> [!NOTE]
> <span data-ttu-id="2bf32-120">Hej [Azure IoT SDK] [ lnk-sdks] automatiskt referensen hämtar hello SAS URI, ladda upp hello-fil och för att meddela en överförda IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2bf32-120">hello [Azure IoT SDKs][lnk-sdks] automatically handle retrieving hello SAS URI, uploading hello file, and notifying IoT Hub of a completed upload.</span></span>


## <a name="initialize-a-file-upload"></a><span data-ttu-id="2bf32-121">Initiera en filöverföring</span><span class="sxs-lookup"><span data-stu-id="2bf32-121">Initialize a file upload</span></span>
<span data-ttu-id="2bf32-122">IoT-hubben har en slutpunkt för enheter toorequest SAS-URI för lagring tooupload en fil.</span><span class="sxs-lookup"><span data-stu-id="2bf32-122">IoT Hub has an endpoint specifically for devices toorequest a SAS URI for storage tooupload a file.</span></span> <span data-ttu-id="2bf32-123">uppladdning av tooinitiate hello fil, hello enheten skickar en POST-begäran för`{iot hub}.azure-devices.net/devices/{deviceId}/files` med hello följande JSON-meddelandetext:</span><span class="sxs-lookup"><span data-stu-id="2bf32-123">tooinitiate hello file upload process, hello device sends a POST request too`{iot hub}.azure-devices.net/devices/{deviceId}/files` with hello following JSON body:</span></span>

```json
{
    "blobName": "{name of hello file for which a SAS URI will be generated}"
}
```

<span data-ttu-id="2bf32-124">IoT-hubb returnerar hello följande data, vilken hello-enhet använder tooupload hello-fil:</span><span class="sxs-lookup"><span data-stu-id="2bf32-124">IoT Hub returns hello following data, which hello device uses tooupload hello file:</span></span>

```json
{
    "correlationId": "somecorrelationid",
    "hostName": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a><span data-ttu-id="2bf32-125">Föråldrad: initiera en filöverföring med GET</span><span class="sxs-lookup"><span data-stu-id="2bf32-125">Deprecated: initialize a file upload with a GET</span></span>

> [!NOTE]
> <span data-ttu-id="2bf32-126">Det här avsnittet beskrivs föråldrade funktioner för hur tooreceive SAS-URI från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2bf32-126">This section describes deprecated functionality for how tooreceive a SAS URI from IoT Hub.</span></span> <span data-ttu-id="2bf32-127">Använd hello POST-metoden som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="2bf32-127">Use hello POST method described previously.</span></span>

<span data-ttu-id="2bf32-128">IoT-hubben har två REST-slutpunkter toosupport filen ladda upp en tooget hello SAS-URI för lagring och hello andra toonotify hello IoT-hubb för en överförda.</span><span class="sxs-lookup"><span data-stu-id="2bf32-128">IoT Hub has two REST endpoints toosupport file upload, one tooget hello SAS URI for storage and hello other toonotify hello IoT hub of a completed upload.</span></span> <span data-ttu-id="2bf32-129">hello enheten initierar uppladdning av hello fil genom att skicka en GET toohello IoT-hubb på `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`.</span><span class="sxs-lookup"><span data-stu-id="2bf32-129">hello device initiates hello file upload process by sending a GET toohello IoT hub at `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`.</span></span> <span data-ttu-id="2bf32-130">Hej IoT-hubb returnerar:</span><span class="sxs-lookup"><span data-stu-id="2bf32-130">hello IoT hub returns:</span></span>

* <span data-ttu-id="2bf32-131">En SAS-URI specifika toohello filen toobe upp.</span><span class="sxs-lookup"><span data-stu-id="2bf32-131">A SAS URI specific toohello file toobe uploaded.</span></span>
* <span data-ttu-id="2bf32-132">En Korrelations-ID toobe används när hello överföringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2bf32-132">A correlation ID toobe used once hello upload is completed.</span></span>

## <a name="notify-iot-hub-of-a-completed-file-upload"></a><span data-ttu-id="2bf32-133">Meddela IoT-hubb slutförda filöverföringen</span><span class="sxs-lookup"><span data-stu-id="2bf32-133">Notify IoT Hub of a completed file upload</span></span>

<span data-ttu-id="2bf32-134">hello enheten är ansvarig för uppladdning av hello filen toostorage med hello Azure Storage SDK: er.</span><span class="sxs-lookup"><span data-stu-id="2bf32-134">hello device is responsible for uploading hello file toostorage using hello Azure Storage SDKs.</span></span> <span data-ttu-id="2bf32-135">När hello överföringen är klar, hello enheten skickar en POST-begäran för`{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` med hello följande JSON-meddelandetext:</span><span class="sxs-lookup"><span data-stu-id="2bf32-135">When hello upload is complete, hello device sends a POST request too`{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` with hello following JSON body:</span></span>

```json
{
    "correlationId": "{correlation ID received from hello initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

<span data-ttu-id="2bf32-136">Hej värdet för `isSuccess` är en boolesk som representerar om hello-filen har överförts.</span><span class="sxs-lookup"><span data-stu-id="2bf32-136">hello value of `isSuccess` is a Boolean representing whether hello file was uploaded successfully.</span></span> <span data-ttu-id="2bf32-137">Hej statuskoden för `statusCode` är hello status för hello överföringen av hello filen toostorage och hello `statusDescription` motsvarar toohello `statusCode`.</span><span class="sxs-lookup"><span data-stu-id="2bf32-137">hello status code for `statusCode` is hello status for hello upload of hello file toostorage, and hello `statusDescription` corresponds toohello `statusCode`.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="2bf32-138">Referensinformation:</span><span class="sxs-lookup"><span data-stu-id="2bf32-138">Reference topics:</span></span>

<span data-ttu-id="2bf32-139">hello ger följande Referensinformation dig mer information om hur du överför filer från en enhet.</span><span class="sxs-lookup"><span data-stu-id="2bf32-139">hello following reference topics provide you with more information about uploading files from a device.</span></span>

## <a name="file-upload-notifications"></a><span data-ttu-id="2bf32-140">Filen överför meddelanden</span><span class="sxs-lookup"><span data-stu-id="2bf32-140">File upload notifications</span></span>

<span data-ttu-id="2bf32-141">När en enhet meddelar IoT-hubb som en överföringen är klar, kan du kan också IoT-hubb generera ett meddelande som innehåller hello namn och lagring platsen för hello-filen.</span><span class="sxs-lookup"><span data-stu-id="2bf32-141">Optionally, when a device notifies IoT Hub that an upload is complete, IoT Hub can generate a notification message that contains hello name and storage location of hello file.</span></span>

<span data-ttu-id="2bf32-142">Enligt beskrivningen i [slutpunkter][lnk-endpoints], IoT-hubb levererar filen överför meddelanden via en slutpunkt för service-riktade (**/messages/servicebound/fileuploadnotifications**) som meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2bf32-142">As explained in [Endpoints][lnk-endpoints], IoT Hub delivers file upload notifications through a service-facing endpoint (**/messages/servicebound/fileuploadnotifications**) as messages.</span></span> <span data-ttu-id="2bf32-143">hello får semantik för filen överför meddelanden är hello samma som för meddelanden moln till enhet och har hello samma [meddelandet livscykel][lnk-lifecycle].</span><span class="sxs-lookup"><span data-stu-id="2bf32-143">hello receive semantics for file upload notifications are hello same as for cloud-to-device messages and have hello same [message lifecycle][lnk-lifecycle].</span></span> <span data-ttu-id="2bf32-144">Varje meddelande som hämtas från hello filen överför aviseringsslutpunkten är en JSON-post med hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="2bf32-144">Each message retrieved from hello file upload notification endpoint is a JSON record with hello following properties:</span></span>

| <span data-ttu-id="2bf32-145">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2bf32-145">Property</span></span> | <span data-ttu-id="2bf32-146">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2bf32-146">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2bf32-147">EnqueuedTimeUtc</span><span class="sxs-lookup"><span data-stu-id="2bf32-147">EnqueuedTimeUtc</span></span> |<span data-ttu-id="2bf32-148">Tidsstämpel som anger när hello-meddelande har skapats.</span><span class="sxs-lookup"><span data-stu-id="2bf32-148">Timestamp indicating when hello notification was created.</span></span> |
| <span data-ttu-id="2bf32-149">DeviceId</span><span class="sxs-lookup"><span data-stu-id="2bf32-149">DeviceId</span></span> |<span data-ttu-id="2bf32-150">**DeviceId** av hello-enhet som har överförts hello-filen.</span><span class="sxs-lookup"><span data-stu-id="2bf32-150">**DeviceId** of hello device which uploaded hello file.</span></span> |
| <span data-ttu-id="2bf32-151">BlobUri</span><span class="sxs-lookup"><span data-stu-id="2bf32-151">BlobUri</span></span> |<span data-ttu-id="2bf32-152">URI för hello upp filen.</span><span class="sxs-lookup"><span data-stu-id="2bf32-152">URI of hello uploaded file.</span></span> |
| <span data-ttu-id="2bf32-153">BlobName</span><span class="sxs-lookup"><span data-stu-id="2bf32-153">BlobName</span></span> |<span data-ttu-id="2bf32-154">Namnet på hello upp filen.</span><span class="sxs-lookup"><span data-stu-id="2bf32-154">Name of hello uploaded file.</span></span> |
| <span data-ttu-id="2bf32-155">LastUpdatedTime</span><span class="sxs-lookup"><span data-stu-id="2bf32-155">LastUpdatedTime</span></span> |<span data-ttu-id="2bf32-156">Tidsstämpel som anger när hello filen uppdaterades senast.</span><span class="sxs-lookup"><span data-stu-id="2bf32-156">Timestamp indicating when hello file was last updated.</span></span> |
| <span data-ttu-id="2bf32-157">BlobSizeInBytes</span><span class="sxs-lookup"><span data-stu-id="2bf32-157">BlobSizeInBytes</span></span> |<span data-ttu-id="2bf32-158">Storleken på hello upp filen.</span><span class="sxs-lookup"><span data-stu-id="2bf32-158">Size of hello uploaded file.</span></span> |

<span data-ttu-id="2bf32-159">**Exempel**.</span><span class="sxs-lookup"><span data-stu-id="2bf32-159">**Example**.</span></span> <span data-ttu-id="2bf32-160">Det här exemplet visar hello innehållet i en fil överför meddelandet.</span><span class="sxs-lookup"><span data-stu-id="2bf32-160">This example shows hello body of a file upload notification message.</span></span>

```json
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a><span data-ttu-id="2bf32-161">Konfigurationsalternativ för filen överför meddelande</span><span class="sxs-lookup"><span data-stu-id="2bf32-161">File upload notification configuration options</span></span>

<span data-ttu-id="2bf32-162">Varje IoT-hubb visar hello följande konfigurationsalternativ för filen överför meddelanden:</span><span class="sxs-lookup"><span data-stu-id="2bf32-162">Each IoT hub exposes hello following configuration options for file upload notifications:</span></span>

| <span data-ttu-id="2bf32-163">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2bf32-163">Property</span></span> | <span data-ttu-id="2bf32-164">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2bf32-164">Description</span></span> | <span data-ttu-id="2bf32-165">Intervall och standard</span><span class="sxs-lookup"><span data-stu-id="2bf32-165">Range and default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2bf32-166">**enableFileUploadNotifications**</span><span class="sxs-lookup"><span data-stu-id="2bf32-166">**enableFileUploadNotifications**</span></span> |<span data-ttu-id="2bf32-167">Kontrollerar om filen överför meddelanden skrivs toohello filen meddelanden slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="2bf32-167">Controls whether file upload notifications are written toohello file notifications endpoint.</span></span> |<span data-ttu-id="2bf32-168">Bool.</span><span class="sxs-lookup"><span data-stu-id="2bf32-168">Bool.</span></span> <span data-ttu-id="2bf32-169">Standard: True.</span><span class="sxs-lookup"><span data-stu-id="2bf32-169">Default: True.</span></span> |
| <span data-ttu-id="2bf32-170">**fileNotifications.ttlAsIso8601**</span><span class="sxs-lookup"><span data-stu-id="2bf32-170">**fileNotifications.ttlAsIso8601**</span></span> |<span data-ttu-id="2bf32-171">Standard-TTL för filen överför meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2bf32-171">Default TTL for file upload notifications.</span></span> |<span data-ttu-id="2bf32-172">ISO_8601 intervall in too48H (minst 1 minut).</span><span class="sxs-lookup"><span data-stu-id="2bf32-172">ISO_8601 interval up too48H (minimum 1 minute).</span></span> <span data-ttu-id="2bf32-173">Standard: 1 timme.</span><span class="sxs-lookup"><span data-stu-id="2bf32-173">Default: 1 hour.</span></span> |
| <span data-ttu-id="2bf32-174">**fileNotifications.lockDuration**</span><span class="sxs-lookup"><span data-stu-id="2bf32-174">**fileNotifications.lockDuration**</span></span> |<span data-ttu-id="2bf32-175">Lås varaktighet för hello filen överför meddelanden kö.</span><span class="sxs-lookup"><span data-stu-id="2bf32-175">Lock duration for hello file upload notifications queue.</span></span> |<span data-ttu-id="2bf32-176">5 too300 sekunder (minst 5 sekunder).</span><span class="sxs-lookup"><span data-stu-id="2bf32-176">5 too300 seconds (minimum 5 seconds).</span></span> <span data-ttu-id="2bf32-177">Standard: 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="2bf32-177">Default: 60 seconds.</span></span> |
| <span data-ttu-id="2bf32-178">**fileNotifications.maxDeliveryCount**</span><span class="sxs-lookup"><span data-stu-id="2bf32-178">**fileNotifications.maxDeliveryCount**</span></span> |<span data-ttu-id="2bf32-179">Leverans av maximalt antal för hello filen överför aviseringskön.</span><span class="sxs-lookup"><span data-stu-id="2bf32-179">Maximum delivery count for hello file upload notification queue.</span></span> |<span data-ttu-id="2bf32-180">1 too100.</span><span class="sxs-lookup"><span data-stu-id="2bf32-180">1 too100.</span></span> <span data-ttu-id="2bf32-181">Standard: 100.</span><span class="sxs-lookup"><span data-stu-id="2bf32-181">Default: 100.</span></span> |

## <a name="additional-reference-material"></a><span data-ttu-id="2bf32-182">Ytterligare referensmaterialet</span><span class="sxs-lookup"><span data-stu-id="2bf32-182">Additional reference material</span></span>

<span data-ttu-id="2bf32-183">Andra referensavsnitten i hello IoT-hubb Utvecklarhandbok inkluderar:</span><span class="sxs-lookup"><span data-stu-id="2bf32-183">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="2bf32-184">[IoT-hubbslutpunkter] [ lnk-endpoints] beskriver hello olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="2bf32-184">[IoT Hub endpoints][lnk-endpoints] describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="2bf32-185">[Begränsning och kvoter] [ lnk-quotas] beskriver hello kvoter och begränsning beteenden som tillämpas toohello IoT-hubb-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2bf32-185">[Throttling and quotas][lnk-quotas] describes hello quotas and throttling behaviors that apply toohello IoT Hub service.</span></span>
* <span data-ttu-id="2bf32-186">[Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] visar hello olika språk SDK: er som du kan använda när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="2bf32-186">[Azure IoT device and service SDKs][lnk-sdks] lists hello various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="2bf32-187">[IoT-hubb frågespråket] [ lnk-query] beskriver hello frågespråk som du kan använda tooretrieve information från IoT-hubb om enheten twins och jobb.</span><span class="sxs-lookup"><span data-stu-id="2bf32-187">[IoT Hub query language][lnk-query] describes hello query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="2bf32-188">[Stöd för IoT-hubb MQTT] [ lnk-devguide-mqtt] ger mer information om stöd för IoT-hubb för hello MQTT protokollet.</span><span class="sxs-lookup"><span data-stu-id="2bf32-188">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2bf32-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2bf32-189">Next steps</span></span>

<span data-ttu-id="2bf32-190">Nu har du fått veta hur tooupload filer från enheter med hjälp av IoT-hubb, kanske du är intresserad av i följande avsnitt i IoT-hubb developer hello:</span><span class="sxs-lookup"><span data-stu-id="2bf32-190">Now you have learned how tooupload files from devices using IoT Hub, you may be interested in hello following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="2bf32-191">[Hantera identiteter för enheten i IoT-hubb][lnk-devguide-identities]</span><span class="sxs-lookup"><span data-stu-id="2bf32-191">[Manage device identities in IoT Hub][lnk-devguide-identities]</span></span>
* <span data-ttu-id="2bf32-192">[Kontrollera åtkomst tooIoT Hub][lnk-devguide-security]</span><span class="sxs-lookup"><span data-stu-id="2bf32-192">[Control access tooIoT Hub][lnk-devguide-security]</span></span>
* <span data-ttu-id="2bf32-193">[Använda enhetstillstånd twins toosynchronize och konfigurationer][lnk-devguide-device-twins]</span><span class="sxs-lookup"><span data-stu-id="2bf32-193">[Use device twins toosynchronize state and configurations][lnk-devguide-device-twins]</span></span>
* <span data-ttu-id="2bf32-194">[Anropa en metod som är direkt på en enhet][lnk-devguide-directmethods]</span><span class="sxs-lookup"><span data-stu-id="2bf32-194">[Invoke a direct method on a device][lnk-devguide-directmethods]</span></span>
* <span data-ttu-id="2bf32-195">[Schema-jobb på flera enheter][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="2bf32-195">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="2bf32-196">Om du vill tootry titt på hello begrepp som beskrivs i den här artikeln får du är intresserad av hello följande IoT-hubb kursen:</span><span class="sxs-lookup"><span data-stu-id="2bf32-196">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorial:</span></span>

* <span data-ttu-id="2bf32-197">[Hur tooupload filer från enheter toohello cloud med IoT-hubb][lnk-fileupload-tutorial]</span><span class="sxs-lookup"><span data-stu-id="2bf32-197">[How tooupload files from devices toohello cloud with IoT Hub][lnk-fileupload-tutorial]</span></span>

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messages-c2d.md#the-cloud-to-device-message-lifecycle
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
