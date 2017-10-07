---
title: "aaaDevice information metadata i hello remote övervakningslösning | Microsoft Docs"
description: "En beskrivning av hello Azure IoT förkonfigurerade lösningen fjärråtkomst övervakning och dess arkitektur."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1b334769-103b-4eb0-a293-184f3d1ba9a3
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 8387b98b8b2ae4934b0c900bc4df37dc17337c60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="device-information-metadata-in-hello-remote-monitoring-preconfigured-solution"></a>Enhetens information metadata i hello remote förkonfigurerade övervakningslösning

hello Azure IoT Suite remote förkonfigurerade övervakningslösning visar en metod för att hantera enhetsmetadata. Den här artikeln beskrivs hello-metoden den här lösningen tar tooenable du toounderstand:

* Lösning för hello vilka enheter metadata lagras.
* Hur hanterar hello lösning hello enhetens metadata.

## <a name="context"></a>Kontext

Hej fjärrövervaknings förkonfigurerade lösningen använder [Azure IoT Hub] [ lnk-iot-hub] tooenable dina enheter toosend data toohello molnet. hello lösningen lagrar information om enheter på tre olika platser:

| Plats | Information som lagras | Implementering |
| -------- | ------------------ | -------------- |
| Identitetsregistret | Enhets-id, autentiseringsnycklar, aktiverade tillstånd | Inbyggda tooIoT Hub |
| Enheten twins | Metadata: rapporterade egenskaper, önskade egenskaper, taggar | Inbyggda tooIoT Hub |
| Cosmos DB | Kommandot och metoden historik | Anpassad för lösning |

IoT-hubb innehåller en [enhetsidentitetsregistret] [ lnk-identity-registry] toomanage åtkomst till tooan IoT-hubb och använder [enhet twins] [ lnk-device-twin] toomanage enhetens metadata . Det finns också en fjärransluten övervakning Lösningsspecifika *enhetsregistret* som lagrar kommandot och metoden historik. hello fjärråtkomst övervakning lösningen använder en [Cosmos DB] [ lnk-docdb] databasen tooimplement ett eget Arkiv för kommandot och metoden historik.

> [!NOTE]
> hello remote förkonfigurerade övervakningslösning behåller hello enhetsidentitetsregistret synkroniserad med hello information i hello Cosmos-DB-databas. Båda använder samma enhet id toouniquely identifiera hello varje enhet ansluten tooyour IoT-hubb.

## <a name="device-metadata"></a>Enhetens metadata

IoT-hubb upprätthåller en [enheten dubbla] [ lnk-device-twin] anslutna tooa remote övervakningslösning för varje simulerade och fysisk enhet. hello lösningen använder enheten twins toomanage hello metadata som associeras med enheter. En enhet dubbla är en JSON-dokumentet som underhålls av IoT-hubb och hello lösningen använder hello IoT-hubb API toointeract med enheten twins.

En enhet dubbla lagrar tre typer av metadata:

- *Rapporterade egenskaper* tooan IoT-hubb skickas av en enhet. I hello remote övervakningslösning, simulerade enheterna skickar rapporterade egenskaper vid start och svar för**ändra enhetsstatus** kommandon och metoder. Du kan visa rapporterade egenskaper i hello **enhetslistan** och **enhetsinformation** i hello lösning portal. Rapporterat egenskaper är skrivskyddade.
- *Egenskaper för Desired* hämtas från hello IoT-hubb av enheter. Det är hello ansvar hello enheten toomake alla nödvändiga konfigurationsändring på hello enhet. Det är också hello ansvar hello enheten tooreport hello ändra tillbaka toohello hubb som rapporterades egenskap. Du kan ange en önskad egenskapsvärdet hello lösning-portalen.
- *Taggar* finns bara i hello enheten dubbla och aldrig synkroniseras med en enhet. Du kan ange värden i hello lösning portal och använda dem när du filtrerar hello lista över enheter. hello lösningen använder också en tagg tooidentify hello ikonen toodisplay för en enhet i hello lösning portal.

Exempel rapporterade egenskaper från hello simulerade enheter innehåller tillverkare, modellnummer, latitud och longitud. Simulerade enheter returnerar hello listan över metoder som stöds som en rapporterade egenskap.

> [!NOTE]
> hello simulerade enheten koden endast använder hello **Desired.Config.TemperatureMeanValue** och **Desired.Config.TelemetryInterval** egenskaper tooupdate hello rapporterade egenskaper som skickas tillbaka tooIoT hubb. Alla andra ändringsbegäranden för önskad egenskapen ignoreras.

En enhet information metadata JSON-dokumentet lagras i hello enheten registret Cosmos-DB-databasen har hello följande struktur:

```json
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

> [!NOTE]
> Information om en enhet kan även inkludera metadata toodescribe hello telemetri hello enheten skickar tooIoT hubb. hello remote övervakningslösning använder den här telemetri metadata toocustomize hur hello instrumentpanelen visar [dynamiska telemetri][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Livscykel

När du skapar en enhet i hello lösning portal, skapar hello lösningen en post i hello Cosmos DB databasen toostore kommandot och metoden historik. Hello lösning skapar nu även en post för hello enheten i hello enhetsidentitetsregistret, vilket genererar hello nycklar hello enheten använder tooauthenticate med IoT-hubben. Dessutom skapas en delad enhet.

När en enhet ansluter först toohello lösning, skickar den rapporterade egenskaper och informationsmeddelande för enheten. hello rapporterade egenskapsvärden sparas automatiskt i hello enheten dubbla. hello rapporterade egenskaper innehåller hello enhetens tillverkare, modellnummer, serienummer och en lista över metoder som stöds. hälsningsmeddelande enheten information innehåller hello lista över hello kommandon hello enheten stöder inklusive information om några parametrar. När hello lösning får det här meddelandet, uppdaterar hello enhetsinformation i hello Cosmos-DB-databasen.

### <a name="view-and-edit-device-information-in-hello-solution-portal"></a>Visa och redigera enhetsinformation i hello lösning portal

hello listan över enheter i hello lösning portal visar hello följande egenskaper för enhet som kolumner som standard: **Status**, **DeviceId**, **tillverkare**, **Modellnummer**, **serienummer**, **Firmware**, **plattform**, **Processor**, och  **Installerade RAM**. Du kan anpassa hello kolumner genom att klicka på **kolumnen editor**. Hej enhetsegenskaper **latitud** och **longitud** enhet hello plats i hello Bing Map på hello instrumentpanelen.

![Kolumnen redigeraren i listan över enheter][img-device-list]

I hello **enhetsinformation** rutan i hello lösning portalen kan du redigera egenskaper och taggar (rapporterat egenskaper är skrivskyddade).

![Informationspenelen][img-device-edit]

Du kan använda hello lösning portal tooremove en enhet från din lösning. När du tar bort en enhet tar bort hello enhetspost från identitetsregistret hello lösning och tar bort hello enheten dubbla. hello lösning tar också bort information relaterad toohello enheten från hello Cosmos-DB-databas. Innan du tar bort en enhet, måste du inaktivera den.

![Ta bort enheten][img-device-remove]

## <a name="device-information-message-processing"></a>Enheten information meddelandebehandling

Enheten informationsmeddelanden som skickats av en enhet skiljer sig från telemetri meddelanden. Enheten informationsmeddelanden innehåller hello-kommandon som en enhet kan svara på och eventuella kommandohistoriken. IoT-hubb själva känner inte till hello metadata i ett meddelande för enheten information och processer hello meddelandet i hello samma sätt som den bearbetar alla meddelanden enhet till moln. I hello remote övervakningslösning, en [Azure Stream Analytics] [ lnk-stream-analytics] (ASA) jobbet läser hälsningsmeddelande från IoT-hubb. Hej **DeviceInfo** stream analytics-jobbet filter för meddelanden som innehåller **”ObjectType”: ”DeviceInfo”** och vidarebefordrar dem toohello **EventProcessorHost** värden -instans som körs i ett webbprogram. Logiken i hello **EventProcessorHost** instans använder hello id toofind hello Cosmos DB enhetspost för hello särskild enhet och uppdatera hello post.

> [!NOTE]
> En enhet information meddelandet är en standard enhet till moln. hello lösning skiljer enheten informationsmeddelanden och telemetri meddelanden med hjälp av ASA frågor.

## <a name="next-steps"></a>Nästa steg

Nu du är klar med att lära dig hur du kan anpassa hello förkonfigurerade lösningar kan du utforska några av hello andra funktioner och förmågor i hello IoT Suite förkonfigurerade lösningar:

* [Förutsägande Underhåll förkonfigurerade lösning: översikt][lnk-predictive-overview]
* [Vanliga frågor och svar om IoT Suite][lnk-faq]
* [IoT-säkerhet från hello bakgrund][lnk-security-groundup]

<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
