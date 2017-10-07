---
title: aaaUnderstand hello Azure IoT Hub identitetsregistret | Microsoft Docs
description: "Utvecklarhandbok - beskrivning av hello IoT-hubb identitetsregistret och hur toouse den toomanage dina enheter. Innehåller information om hello import och export av enheten identiteter gruppvis."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0706eccd-e84c-4ae7-bbd4-2b1a22241147
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c9fe3730a4608e28c47807ecb42e13e73f6a2e80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-hello-identity-registry-in-your-iot-hub"></a>Förstå hello identitetsregistret i din IoT-hubb

Alla IoT-hubben har en identitetsregistret som lagrar information om hello enheter tillåts tooconnect toohello IoT-hubb. Innan en enhet kan ansluta tooan IoT-hubb kan finnas det en post för enheten i hello IoT hub identitetsregistret. En enhet måste också autentisera med hello IoT-hubben utifrån autentiseringsuppgifter som lagras i hello identitetsregistret.

hello enhets-ID som lagras i hello identitetsregistret är skiftlägeskänslig.

På en hög nivå är hello identitetsregistret en REST-kompatibla samling enhetsresurser identitet. När du lägger till en post i hello identitetsregistret skapar IoT-hubb en uppsättning resurser per enhet, till exempel hello kö som innehåller relä moln till enhet meddelanden.

### <a name="when-toouse"></a>När toouse

Använd hello identitetsregistret när du behöver:

* Etablera enheter som ansluter tooyour IoT-hubb.
* Styra per enhet åtkomst tooyour hubbens enheten riktade slutpunkter.

> [!NOTE]
> Hej identitetsregistret innehåller inte några programspecifika metadata.

## <a name="identity-registry-operations"></a>Identitetsregisteråtgärder

Hej IoT-hubb identitetsregistret visar hello följande åtgärder:

* Skapa enhetsidentitet
* Uppdatera enhetsidentitet
* Hämta enhetsidentitet-ID: t
* Ta bort enhetens identitet
* Fram too1000 identiteter
* Exportera alla identiteter tooAzure blob-lagring
* Importera identiteter från Azure blob storage

Dessa åtgärder kan använda Optimistisk samtidighet som anges i [RFC7232][lnk-rfc7232].

> [!IMPORTANT]
> Hej endast sätt tooretrieve alla identiteter i en IoT-hubb identitetsregistret är toouse hello [exportera] [ lnk-export] funktioner.

En IoT-hubb identitetsregistret:

* Innehåller inte några metadata för program.
* Kan användas som en ordlista med hjälp av hello **deviceId** som hello nyckel.
* Stöder inte lättfattliga frågor.

En IoT-lösning har vanligtvis ett separat Arkiv för Lösningsspecifika som innehåller programspecifika metadata. Till exempel skulle hello Lösningsspecifika store i en lösning för smart byggnad registrera hello plats där en temperatursensor distribueras.

> [!IMPORTANT]
> Använd endast hello identitetsregistret för enhetshantering och etablerar operations. Hög genomströmning åtgärder vid körning bör inte beroende utför åtgärder i hello identitetsregistret. Till exempel är kontrollerar hello anslutningsstatus för en enhet innan du skickar ett kommando inte ett mönster som stöds. Se till att toocheck hello [begränsning priser] [ lnk-quotas] för hello identitetsregistret och hello [enheten pulsslag] [ lnk-guidance-heartbeat] mönster.

## <a name="disable-devices"></a>Inaktivera enheter

Du kan inaktivera enheter genom att uppdatera hello **status** -egenskapen för en identitet i hello identitetsregistret. Normalt använder du den här egenskapen i två scenarier:

* Under en allokering orchestration-process. Mer information finns i [Enhetsetableringen][lnk-guidance-provisioning].
* Om du av någon anledning du en enhet har komprometterats eller blivit obehörig.

## <a name="import-and-export-device-identities"></a>Importera och exportera enheten identiteter

Du kan exportera enheten identiteter i grupp från en IoT-hubb identitetsregistret med hjälp av asynkrona åtgärder på hello [IoT-hubb resurs leverantörsslutpunkt][lnk-endpoints]. Export är långvariga jobb som använder en kundens blob-behållaren toosave identitet enhetsdata läsa från hello identitetsregistret.

Du kan importera enheten identiteter i bulk tooan IoT-hubbs identitetsregistret med hjälp av asynkrona åtgärder på hello [IoT-hubb resurs leverantörsslutpunkt][lnk-endpoints]. Import är långvariga jobb som använder data i en kundens blob-behållaren toowrite identitet enhetsdata till hello identitetsregistret.

* Detaljerad information om hello import och export API: er finns [IoT-hubb resursprovidern REST API: er][lnk-resource-provider-apis].
* toolearn mer om körs importera och exportera jobben, se [Massredigera hantering av identiteter för IoT-hubb enheten][lnk-bulk-identity].

## <a name="device-provisioning"></a>Enhetsetableringen

hello enhetsdata som lagras i en viss IoT-lösningen är beroende av hello särskilda krav för lösningen. Men minst en lösning måste lagra enheten identiteter och autentiseringsnycklar. Azure IoT-hubb innehåller en identitetsregistret som kan lagra värdena för varje enhet, t.ex ID, autentiseringsnycklar och statuskoder. En lösning kan använda andra Azure-tjänster, till exempel tabellagring eller blob storage Cosmos DB toostore alla övriga data.

*Enhetsetableringen* är hello process för att lägga till hello inledande toohello datalager i din lösning. tooenable en ny enhet tooconnect tooyour hubb, måste du lägga till enheten ID och nycklar toohello IoT-hubb identitetsregistret. Som en del av hello etableringsprocessen, kanske du måste tooinitialize enhetsspecifika data i andra lösning Arkiv.

## <a name="device-heartbeat"></a>Enheten pulsslag

Hej IoT-hubb identitetsregistret innehåller ett fält med namnet **connectionState**. Använd bara hello **connectionState** fältet under utveckling och felsökning. IoT-lösningar bör inte fråga hello fält vid körning. Till exempel fråga inte hello **connectionState** fältet toocheck om en enhet är ansluten innan du skickar ett moln till enhet eller ett SMS.

Om din IoT-lösningen behöver tooknow om en enhet är ansluten, bör du implementera hello *pulsslag mönster*.

I mönstret pulsslag hello hello enhet skickar meddelanden från enhet till moln minst en gång var fast tidsperiod (t.ex, minst en gång i timmen). Därför även om en enhet inte har några data toosend skickar fortfarande den ett tomt meddelande enhet till moln (vanligtvis med en egenskap som identifierar den som ett pulsslag). På hello tjänstsidan upprätthåller hello en karta med hello senaste pulsslag togs emot för varje enhet. Om hello lösningen inte får ett heartbeat-meddelande inom hello förväntade tiden från hello enhet förutsätter att det finns problem med hello enhet.

En mer komplex implementering kan omfatta hello information från [operations övervakning] [ lnk-devguide-opmon] tooidentify enheter som försöker tooconnect eller kommunicera men misslyckas. När du implementerar hello pulsslag mönstret gör att toocheck [IoT-hubb kvoter och begränsningar][lnk-quotas].

> [!NOTE]
> Om en IoT-lösningen använder hello anslutning tillstånd enbart toodetermine om toosend moln till enhet meddelanden och meddelanden är broadcast toolarge uppsättningar av enheter, bör du använda hello enklare inte *kort förfallotid* mönster. Det här mönstret uppnår hello samma resultat som du underhålla enheten anslutning tillstånd registret med hjälp av mönster för pulsslag hello samtidigt som det är mer effektivt. Om du begär meddelande bekräftelser kan IoT-hubb meddela dig om vilka enheter som kan tooreceive meddelanden och som inte är.

## <a name="device-lifecycle-notifications"></a>Enheten livscykelmeddelanden

IoT-hubb kan meddela din IoT-lösning när en enhetsidentitet skapas eller tas bort genom att skicka meddelanden för enheten livscykel. toodo så IoT-lösningen behöver toocreate en väg och tooset hello datakällan lika för*DeviceLifecycleEvents*. Inga livscykelmeddelanden skickas som standard, som är det inför finns ingen sådan vägar. hello-meddelande innehåller egenskaperna och innehållet.

Egenskaper: Meddelandet Systemegenskaper föregås hello `'$'` symbolen.

| Namn | Värde |
| --- | --- |
$content-typ | application/json |
$iothub-enqueuedtime |  Tid när hello-meddelande skickades |
$iothub-meddelande-källa | deviceLifecycleEvents |
$content-kodning | UTF-8 |
opType | **createDeviceIdentity** eller **deleteDeviceIdentity** |
hubName | Namnet på IoT-hubb |
deviceId | ID för hello-enhet |
operationTimestamp | ISO8601 tidsstämpeln för åtgärden |
iothub-meddelande-schema | deviceLifecycleNotification |

Body: Det här avsnittet är i JSON-format och representerar hello dubbla av hello skapade enhetens identitet. Exempel:

```json
{
    "deviceId":"11576-ailn-test-0-67333793211",
    "etag":"AAAAAAAAAAE=",
    "properties": {
        "desired": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        },
        "reported": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        }
    }
}
```

## <a name="reference-topics"></a>Referensinformation:

hello ger följande Referensinformation dig mer om hello identitetsregistret.

## <a name="device-identity-properties"></a>Egenskaper för enhet identitet

Enheten identiteter representeras som JSON-dokument med hello följande egenskaper:

| Egenskap | Alternativ | Beskrivning |
| --- | --- | --- |
| deviceId |krävs, skrivskyddad på uppdateringar |En skiftlägeskänslig sträng (upp too128 tecken) av alfanumeriska tecken, ASCII-7-bitars + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| ID för virtuella datorer |krävs, skrivskyddad |En IoT hub-genererade, skiftlägeskänsliga sträng in too128 tecken. Det här värdet är används toodistinguish enheter med hello samma **deviceId**, när de har tagits bort och återskapas. |
| ETag |krävs, skrivskyddad |En sträng som representerar en svag ETag för hello enhetsidentitet enligt [RFC7232][lnk-rfc7232]. |
| auth |Valfria |En sammansatt objekt som innehåller information och säkerhet material för autentisering. |
| auth.symkey |Valfria |En sammansatt objekt som innehåller en primär och en sekundär nyckel lagrad i base64-format. |
| status |Krävs |En åtkomst-indikator. Kan vara **aktiverad** eller **inaktiverade**. Om **aktiverad**, hello enhet får tooconnect. Om **inaktiverad**, den här enheten har inte åtkomst till valfri enhet riktade slutpunkt. |
| statusReason |Valfria |128 tecken lång sträng lagrar hello därför för hello Enhetsstatus identitet. Alla UTF-8-tecken tillåts. |
| statusUpdateTime |Skrivskyddad |En temporal indikator som visar hello datum och tid för senaste statusuppdatering-hello. |
| connectionState |Skrivskyddad |Ett fält som anger status för användaranslutning: antingen **ansluten** eller **frånkopplad**. Det här fältet motsvarar hello IoT-hubb vy över hello enhetens anslutningsstatus. **Viktiga**: det här fältet bör användas endast för utveckling/felsökning. hello anslutningens status uppdateras bara för enheter med hjälp av MQTT eller AMQP. Dessutom baseras på protokollnivå pingar (MQTT pingar eller AMQP ping) och den kan ha en maximal fördröjning på endast 5 minuter. Därmed behöver kan det finnas falska positiva identifieringar, t.ex enheter rapporteras som är ansluten men som inte är ansluten. |
| connectionStateUpdatedTime |Skrivskyddad |En temporal indikator som visar hello datum och senaste gången hello anslutningens status uppdaterades. |
| lastActivityTime |Skrivskyddad |En temporal indikator som visar hello datum och senaste gången hello enhet ansluten, tas emot eller skickade ett meddelande. |

> [!NOTE]
> Status för anslutningen kan endast representera hello IoT-hubb vy över hello hello anslutningens status. Vara kan fördröjd uppdateringar toothis tillstånd beroende på nätverkets tillstånd och konfigurationer.

## <a name="additional-reference-material"></a>Ytterligare referensmaterialet

Andra referensavsnitten i hello IoT-hubb Utvecklarhandbok inkluderar:

* [IoT-hubbslutpunkter] [ lnk-endpoints] beskriver hello olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.
* [Begränsning och kvoter] [ lnk-quotas] beskriver hello kvoter och begränsning beteenden som tillämpas toohello IoT-hubb-tjänsten.
* [Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] visar hello olika språk SDK: er som du kan använda när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.
* [IoT-hubb frågespråket] [ lnk-query] beskriver hello frågespråk som du kan använda tooretrieve information från IoT-hubb om enheten twins och jobb.
* [Stöd för IoT-hubb MQTT] [ lnk-devguide-mqtt] ger mer information om stöd för IoT-hubb för hello MQTT protokollet.

## <a name="next-steps"></a>Nästa steg

Nu har du lärt dig hur toouse hello IoT-hubb identitetsregistret du vill ha i följande avsnitt i IoT-hubb developer hello:

* [Kontrollera åtkomst tooIoT Hub][lnk-devguide-security]
* [Använda enhetstillstånd twins toosynchronize och konfigurationer][lnk-devguide-device-twins]
* [Anropa en metod som är direkt på en enhet][lnk-devguide-directmethods]
* [Schema-jobb på flera enheter][lnk-devguide-jobs]

Om du vill tootry titt på hello begrepp som beskrivs i den här artikeln får du är intresserad av hello följande IoT-hubb kursen:

* [Kom igång med Azure IoT-hubb][lnk-getstarted-tutorial]

<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
