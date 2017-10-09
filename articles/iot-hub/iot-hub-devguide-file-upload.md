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
# <a name="upload-files-with-iot-hub"></a>Överföra filer med IoT-hubb

Detaljerad i hello [IoT-hubbslutpunkter] [ lnk-endpoints] artikel, en enhet kan initiera en filöverföring genom att skicka ett meddelande via en enhet riktade slutpunkt (**/devices/ {deviceId} / filer**). När en enhet meddelar IoT-hubb som en överföringen är klar, IoT-hubb skickas ett meddelande för filen överför via hello **/messages/servicebound/filenotifications** service-riktade slutpunkt.

I stället för förtroendeförmedling meddelanden via IoT-hubb själva fungerar IoT-hubb i stället som en dispatcher tooan som är associerade med Azure Storage-konto. En enhet begär en storage-token från IoT-hubb som är specifika toohello filen hello enhet önskar tooupload. hello enhet använder hello SAS URI tooupload hello filen toostorage och när hello överföringen är klar hello enheten skickar ett meddelande om slutförande tooIoT hubb. IoT-hubb kontrollerar hello filöverföring har slutförts och lägger sedan till en fil överför meddelandet toohello service-riktade filen notification aviseringsslutpunkten.

Innan du laddar upp en fil tooIoT hubb från en enhet måste du konfigurera din hubb av [associera en Azure Storage] [ lnk-associate-storage] tooit-kontot.

Enheten kan sedan [initiera en överföring] [ lnk-initialize] och sedan [meddela IoT-hubb] [ lnk-notify] när hello överföringen är klar. Du kan också när en enhet meddelar IoT-hubb som hello överföringen är klar, hello-tjänsten kan skapa en [meddelande][lnk-service-notification].

### <a name="when-toouse"></a>När toouse

Använd överför toosend mediafiler och stora telemetri batchar har laddats upp av periodvis anslutna enheter eller komprimerade toosave bandbredd.

Se för[enhet till moln kommunikation vägledning] [ lnk-d2c-guidance] om osäkra mellan att använda rapporterade egenskaper, meddelanden från enhet till moln eller ladda upp filen.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Associera ett Azure Storage-konto med IoT-hubb

toouse hello filen överför funktioner, måste du först koppla ett Azure Storage-konto toohello IoT-hubb. Du kan slutföra den här uppgiften via hello [Azure-portalen][lnk-management-portal], eller programmässigt via hello [IoT-hubb resursprovidern REST API: er] [ lnk-resource-provider-apis]. När du har associerat ett Azure Storage-konto med IoT-hubben, returnerar hello-tjänsten en SAS-URI tooa enhet när hello enheten initierar en begäran om överföring.

> [!NOTE]
> Hej [Azure IoT SDK] [ lnk-sdks] automatiskt referensen hämtar hello SAS URI, ladda upp hello-fil och för att meddela en överförda IoT-hubb.


## <a name="initialize-a-file-upload"></a>Initiera en filöverföring
IoT-hubben har en slutpunkt för enheter toorequest SAS-URI för lagring tooupload en fil. uppladdning av tooinitiate hello fil, hello enheten skickar en POST-begäran för`{iot hub}.azure-devices.net/devices/{deviceId}/files` med hello följande JSON-meddelandetext:

```json
{
    "blobName": "{name of hello file for which a SAS URI will be generated}"
}
```

IoT-hubb returnerar hello följande data, vilken hello-enhet använder tooupload hello-fil:

```json
{
    "correlationId": "somecorrelationid",
    "hostName": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Föråldrad: initiera en filöverföring med GET

> [!NOTE]
> Det här avsnittet beskrivs föråldrade funktioner för hur tooreceive SAS-URI från IoT-hubb. Använd hello POST-metoden som beskrivs ovan.

IoT-hubben har två REST-slutpunkter toosupport filen ladda upp en tooget hello SAS-URI för lagring och hello andra toonotify hello IoT-hubb för en överförda. hello enheten initierar uppladdning av hello fil genom att skicka en GET toohello IoT-hubb på `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. Hej IoT-hubb returnerar:

* En SAS-URI specifika toohello filen toobe upp.
* En Korrelations-ID toobe används när hello överföringen har slutförts.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Meddela IoT-hubb slutförda filöverföringen

hello enheten är ansvarig för uppladdning av hello filen toostorage med hello Azure Storage SDK: er. När hello överföringen är klar, hello enheten skickar en POST-begäran för`{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` med hello följande JSON-meddelandetext:

```json
{
    "correlationId": "{correlation ID received from hello initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Hej värdet för `isSuccess` är en boolesk som representerar om hello-filen har överförts. Hej statuskoden för `statusCode` är hello status för hello överföringen av hello filen toostorage och hello `statusDescription` motsvarar toohello `statusCode`.

## <a name="reference-topics"></a>Referensinformation:

hello ger följande Referensinformation dig mer information om hur du överför filer från en enhet.

## <a name="file-upload-notifications"></a>Filen överför meddelanden

När en enhet meddelar IoT-hubb som en överföringen är klar, kan du kan också IoT-hubb generera ett meddelande som innehåller hello namn och lagring platsen för hello-filen.

Enligt beskrivningen i [slutpunkter][lnk-endpoints], IoT-hubb levererar filen överför meddelanden via en slutpunkt för service-riktade (**/messages/servicebound/fileuploadnotifications**) som meddelanden. hello får semantik för filen överför meddelanden är hello samma som för meddelanden moln till enhet och har hello samma [meddelandet livscykel][lnk-lifecycle]. Varje meddelande som hämtas från hello filen överför aviseringsslutpunkten är en JSON-post med hello följande egenskaper:

| Egenskap | Beskrivning |
| --- | --- |
| EnqueuedTimeUtc |Tidsstämpel som anger när hello-meddelande har skapats. |
| DeviceId |**DeviceId** av hello-enhet som har överförts hello-filen. |
| BlobUri |URI för hello upp filen. |
| BlobName |Namnet på hello upp filen. |
| LastUpdatedTime |Tidsstämpel som anger när hello filen uppdaterades senast. |
| BlobSizeInBytes |Storleken på hello upp filen. |

**Exempel**. Det här exemplet visar hello innehållet i en fil överför meddelandet.

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

## <a name="file-upload-notification-configuration-options"></a>Konfigurationsalternativ för filen överför meddelande

Varje IoT-hubb visar hello följande konfigurationsalternativ för filen överför meddelanden:

| Egenskap | Beskrivning | Intervall och standard |
| --- | --- | --- |
| **enableFileUploadNotifications** |Kontrollerar om filen överför meddelanden skrivs toohello filen meddelanden slutpunkt. |Bool. Standard: True. |
| **fileNotifications.ttlAsIso8601** |Standard-TTL för filen överför meddelanden. |ISO_8601 intervall in too48H (minst 1 minut). Standard: 1 timme. |
| **fileNotifications.lockDuration** |Lås varaktighet för hello filen överför meddelanden kö. |5 too300 sekunder (minst 5 sekunder). Standard: 60 sekunder. |
| **fileNotifications.maxDeliveryCount** |Leverans av maximalt antal för hello filen överför aviseringskön. |1 too100. Standard: 100. |

## <a name="additional-reference-material"></a>Ytterligare referensmaterialet

Andra referensavsnitten i hello IoT-hubb Utvecklarhandbok inkluderar:

* [IoT-hubbslutpunkter] [ lnk-endpoints] beskriver hello olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.
* [Begränsning och kvoter] [ lnk-quotas] beskriver hello kvoter och begränsning beteenden som tillämpas toohello IoT-hubb-tjänsten.
* [Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] visar hello olika språk SDK: er som du kan använda när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.
* [IoT-hubb frågespråket] [ lnk-query] beskriver hello frågespråk som du kan använda tooretrieve information från IoT-hubb om enheten twins och jobb.
* [Stöd för IoT-hubb MQTT] [ lnk-devguide-mqtt] ger mer information om stöd för IoT-hubb för hello MQTT protokollet.

## <a name="next-steps"></a>Nästa steg

Nu har du fått veta hur tooupload filer från enheter med hjälp av IoT-hubb, kanske du är intresserad av i följande avsnitt i IoT-hubb developer hello:

* [Hantera identiteter för enheten i IoT-hubb][lnk-devguide-identities]
* [Kontrollera åtkomst tooIoT Hub][lnk-devguide-security]
* [Använda enhetstillstånd twins toosynchronize och konfigurationer][lnk-devguide-device-twins]
* [Anropa en metod som är direkt på en enhet][lnk-devguide-directmethods]
* [Schema-jobb på flera enheter][lnk-devguide-jobs]

Om du vill tootry titt på hello begrepp som beskrivs i den här artikeln får du är intresserad av hello följande IoT-hubb kursen:

* [Hur tooupload filer från enheter toohello cloud med IoT-hubb][lnk-fileupload-tutorial]

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
