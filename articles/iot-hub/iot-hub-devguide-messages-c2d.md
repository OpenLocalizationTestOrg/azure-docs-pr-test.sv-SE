---
title: aaaUnderstand Azure IoT Hub moln till enhet messaging | Microsoft Docs
description: "Utvecklarhandbok - hur toouse moln till enhet meddelanden med IoT-hubben. Innehåller information om livscykeln för hello-meddelande och konfigurationsalternativ."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 5c747b50163873d823556a8baa769c4b8f7f8c44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-to-device-messages-from-iot-hub"></a>Skicka meddelanden moln till enhet från IoT-hubb

toosend enkelriktade meddelanden toohello enhetsapp från din lösningens serverdel meddelanden moln-enheter från IoT-hubb tooyour enheten. En beskrivning av andra alternativ för molnet till enheter som stöds av IoT-hubb finns [moln till enhet kommunikation vägledning][lnk-c2d-guidance].

Du skickar meddelanden moln till enhet via en slutpunkt för service-riktade (**/meddelanden/devicebound**). En enhet får sedan hello meddelanden via en enhetsspecifik slutpunkt (**/devices/ {deviceId} / meddelanden/devicebound**).

Varje moln till enhet-meddelande är inriktad på en enskild enhet genom att ange hello **till** egenskapen för**/devices/ {deviceId} / meddelanden/devicebound**.

Varje enhet kön innehåller högst 50 moln till enhet meddelanden. Försök toosend flera meddelanden toohello samma enhet som resulterar i ett fel.

## <a name="hello-cloud-to-device-message-lifecycle"></a>hello moln-till-enhetsmeddelande livscykel

tooguarantee på-minst en gång meddelandeleverans, IoT-hubb kvarstår moln till enhet meddelanden i köer per enhet. Enheter måste uttryckligen Bekräfta *slutförande* för IoT-hubb tooremove hello dem från kön. Den här metoden garanterar återhämtning mot anslutning och enhetsfel.

hello följande diagram visar hello livscykel tillstånd diagram för ett moln till enhet meddelande i IoT-hubb.

![Moln-till-enhetsmeddelande livscykel][img-lifecycle]

Anger när hello IoT-hubb tjänsten skickar ett meddelande tooa enhet, hello service hello meddelandet tillstånd för**köas**. När en enhet vill för*får* meddelandet IoT-hubb *Lås* hello-meddelande (genom att ange hello tillstånd för**osynliga**), vilket gör att andra trådar på hello enheten toostart ta emot andra meddelanden. När en enhet tråd är klar hello bearbetning av ett meddelande skickas ett meddelande till IoT-hubb av *Slutför* hello-meddelande. IoT-hubb ställer in hello tillstånd för**slutförd**.

En enhet kan också välja att:

* *Avvisa* hello-meddelande, vilket gör att IoT-hubb tooset den toohello **Deadlettered** tillstånd. Enheter som ansluter via hello MQTT-protokollet kan inte avvisa meddelanden moln till enhet.
* *Avbryt* hello-meddelande, vilket gör IoT-hubb tooput hello-meddelande tillbaka i hello kön med hello-statusen inställd för**köas**.

En tråd kan misslyckas tooprocess ett meddelande utan att meddela IoT-hubb. I det här fallet meddelanden automatiskt övergången från hello **osynliga** tillstånd tillbaka toohello **köas** tillstånd efter en *synlighet (eller lås) timeout*. hello standardvärdet för det här är en minut.

Ett meddelande kan övergå mellan hello **köas** och **osynliga** tillstånd, mest hello antalet gånger som anges i hello **max antal leverans** -egenskapen i IoT-hubb. Efter att antalet övergångar, IoT-hubb anger hello tillståndet för hello-meddelande för**Deadlettered**. På liknande sätt IoT-hubb anger hello tillståndet för ett meddelande för**Deadlettered** efter dess förfallotid (se [tid toolive][lnk-ttl]).

Hej [hur toosend moln till enhet meddelanden med IoT-hubb] [ lnk-c2d-tutorial] visar hur toosend moln till enhet meddelanden från hello molnet och ta emot dem på en enhet.

En enhet fyller normalt ett moln till enhet meddelande när hello förlust av hello-meddelande inte påverkar hello programlogik. Till exempel när hello enheten har sparat hello meddelandeinnehåll lokalt eller har har utfört en åtgärd. hello-meddelande kan också innehålla tillfälliga uppgifter, vars förlust inte påverkar hello programmet hello funktioner. Ibland är för tidskrävande uppgifter du kan slutföra hälsningsmeddelande moln till enhet efter beständighet hello Aktivitetsbeskrivningen i lokal lagring. Du kan sedan meddela hello lösningens serverdel med en eller flera meddelanden från enhet till moln i olika faser av förloppet för hello aktiviteten.

## <a name="message-expiration-time-toolive"></a>Utgångna (tid toolive)

Alla moln till enhet meddelanden har en förfallotid. Nu anges antingen av hello-tjänsten (i hello **ExpiryTimeUtc** egenskapen), eller genom IoT-hubb med hello standard *tid toolive* angetts som en IoT-hubb-egenskap. Se [moln till enhet konfigurationsalternativ][lnk-c2d-configuration].

Ett vanligt sätt tootake nytta av meddelande upphör att gälla och undvika att skicka meddelanden toodisconnected enheter är tooset toolive värden för kort tid. Den här metoden ger samma resultat som underhålla hello enhetens anslutning tillstånd, samtidigt som det är effektivare hello. När du begär meddelande bekräftelser meddelar IoT-hubb vilka enheter som är kan tooreceive meddelanden och vilka enheter som inte är online eller har misslyckats.

## <a name="message-feedback"></a>Meddelandet feedback

När du skickar ett meddelande om moln till enhet kan hello tjänstbegäran hello leverans av varje meddelande feedback om hello sluttillstånd i meddelandet.

| Ack-egenskap | Beteende |
| ------------ | -------- |
| **positivt** | IoT-hubb genererar ett meddelande om feedback om, och om hello moln till enhet meddelandet nådde hello **slutförd** tillstånd. |
| **negativt** | IoT-hubb genererar ett meddelande som feedback om och bara om moln till enhet hälsningsmeddelande når hello **Deadlettered** tillstånd. |
| **fullständig**     | IoT-hubb genererar ett meddelande om feedback i båda fallen. |

Om **Ack** är **fullständig**, och inte ett meddelande feedback, innebär det att feedback hello-meddelande har upphört att gälla. hello-tjänsten kan inte vet vad hände toohello ursprungliga meddelandet. En tjänst bör i praktiken, se till att den kan bearbeta hello feedback innan den upphör. hello maximala förfallotiden är två dagar som ger tillräckligt med tid tooget hello tjänsten körs igen om ett fel inträffar.

Enligt beskrivningen i [slutpunkter][lnk-endpoints], IoT-hubb ger feedback via en slutpunkt för service-riktade (**/messages/servicebound/feedback**) som meddelanden. hello semantik för att ta emot feedback hello är samma som för meddelanden moln till enhet och har hello samma [meddelandet livscykel][lnk-lifecycle]. När det är möjligt batchar meddelandet feedback i ett enda meddelande med hello följande format:

| Egenskap     | Beskrivning |
| ------------ | ----------- |
| EnqueuedTime | Tidsstämpel som anger när hello-meddelande har skapats. |
| Användar-ID       | `{iot hub name}` |
| ContentType  | `application/vnd.microsoft.iothub.feedback.json` |

hello-meddelandetexten är en JSON-serialiserad matris med poster, var och en med hello följande egenskaper:

| Egenskap           | Beskrivning |
| ------------------ | ----------- |
| EnqueuedTimeUtc    | Tidsstämpel som anger när det hände hello resultatet av hello-meddelande. Till exempel hello enheten har slutförts eller hello-meddelande har upphört att gälla. |
| originalMessageId  | **MessageId** för hello moln-till-enhetsmeddelande toowhich informationen feedback avser. |
| statusCode         | Strängen som krävs. Används i feedback-meddelanden som genereras av IoT-hubb. <br/> ”Lyckades” <br/> 'Har upphört att gälla: <br/> 'DeliveryCountExceeded' <br/> 'Avvisade' <br/> 'Rensas' |
| Beskrivning        | Sträng som värden för **StatusCode**. |
| DeviceId           | **DeviceId** för hello målenhet av hello moln-till-enhetsmeddelande toowhich denna typ av feedback avser. |
| DeviceGenerationId | **DeviceGenerationId** för hello målenhet av hello moln-till-enhetsmeddelande toowhich denna typ av feedback avser. |

hello-tjänsten måste ange en **MessageId** för hello moln till enhet message toobe kan toocorrelate dess feedback med ursprungliga hello-meddelande.

hello visar följande exempel hello meddelandetext feedback.

```json
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0,
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

## <a name="cloud-to-device-configuration-options"></a>Konfigurationsalternativ för moln till enhet

Varje IoT-hubb visar hello följande konfigurationsalternativ för meddelanden moln till enhet:

| Egenskap                  | Beskrivning | Intervall och standard |
| ------------------------- | ----------- | ----------------- |
| defaultTtlAsIso8601       | Standard-TTL för moln till enhet meddelanden. | ISO_8601 intervall in too2D (minst 1 minut). Standard: 1 timme. |
| MaxDeliveryCount          | Leverans av maximalt antal för köer moln till enhet per enhet. | 1 too100. Standard: 10. |
| feedback.ttlAsIso8601     | Kvarhållning för service-bunden feedback meddelanden. | ISO_8601 intervall in too2D (minst 1 minut). Standard: 1 timme. |
| feedback.maxDeliveryCount |Leverans av maximalt antal för feedbackkö. | 1 too100. Standard: 100. |

Mer information om hur tooset dessa konfigurationsalternativ finns [skapa IoT-hubbar][lnk-portal].

## <a name="next-steps"></a>Nästa steg

Information om hello SDK kan du använda tooreceive moln till enhet meddelanden, finns [Azure IoT SDK][lnk-sdks].

tootry ut ta emot meddelanden moln till enhet, finns hello [skicka moln till enhet] [ lnk-c2d-tutorial] kursen.

[img-lifecycle]: ./media/iot-hub-devguide-messages-c2d/lifecycle.png

[lnk-portal]: iot-hub-create-through-portal.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-ttl]: #message-expiration-time-to-live
[lnk-c2d-configuration]: #cloud-to-device-configuration-options
[lnk-lifecycle]: #message-lifecycle
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
