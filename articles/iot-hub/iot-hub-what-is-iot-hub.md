---
title: "aaaAzure IoT-hubb översikt | Microsoft Docs"
description: "Översikt över hello Azure IoT Hub-tjänsten: Vad är IoT-hubb, enhetsanslutning, internet saker kommunikationsmönster, gateways och hello service-stödd kommunikation mönster"
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d0b46868a1ec9e13c8f107b90269c5307f4ba27c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-hello-azure-iot-hub-service"></a>Översikt över hello Azure IoT Hub-tjänsten

Välkommen tooAzure IoT-hubb. Den här artikeln innehåller en översikt över Azure IoT Hub och beskriver varför du bör använda den här tjänsten tooimplement en lösning för Sakernas Internet (IoT). Azure IoT Hub är en helt hanterad tjänst som möjliggör tillförlitlig och säker dubbelriktad kommunikation mellan flera miljoner IoT-enheter och som tillhandahåller serverdelen för lösningar av den här typen. Azure IoT Hub:

* Tillhandahåller flera alternativ för kommunikation från enhet till moln och från moln till enhet, inklusive enkelriktade meddelanden, filöverföringar och begäran/svar-metoder.
* Innehåller inbyggda deklarativ meddelandet routning tooother Azure-tjänster.
* Tillhandahåller frågbart lager för enhetsmetadata och synkroniserad tillståndsinformation.
* Kan skydda kommunikation och åtkomstkontroll med säkerhetsnycklar eller X.509-certifikat för varje enhet.
* Tillhandahåller omfattande övervakning för händelser relaterade till enhetsanslutningar och enhetsidentiteter.
* Innehåller enhetsbibliotek för hello mest populära språk och plattformar.

hello artikel [jämförelse av IoT-hubb och Händelsehubbar] [ lnk-compare] beskriver hello viktigaste skillnaderna mellan dessa två tjänster och visar hello fördelarna med IoT-hubb i IoT-lösningar.

Mer information om hur Azure- och IoT-hubb skydda IoT-lösningen finns [Sakernas Internet security från hello bakgrund][lnk-security-ground-up].

![Azure IoT Hub som moln-gateway i IoT-lösningar][img-architecture]

> [!NOTE]
> En detaljerad beskrivning av IoT-arkitekturen finns i hello [Referensarkitektur för Microsoft Azure IoT][lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>Utmaningar med IoT-enhetsanslutningar

IoT-hubb och hello enhetsbibliotek hjälper dig toomeet hello utmaningarna med hur tooreliably och på ett säkert sätt ansluta enheter toohello lösningens serverdel. IoT-enheter:

* Är ofta inbyggda system utan mänsklig operatör.
* Kan finnas på avlägsna platser där fysisk åtkomst är dyr.
* Kan endast nås via hello lösningens serverdel.
* Kan ha begränsade ström- och bearbetningsresurser.
* Kan ha oregelbunden, långsam eller dyr nätverksanslutning.
* Kanske måste toouse egna, anpassade eller branschspecifika programprotokoll.
* Kan skapas med en stor mängd populära maskinvaru- och programvaruplattformar.

Dessutom toohello kraven ovan behöver alla IoT-lösningar också erbjuda skalbarhet, säkerhet och tillförlitlighet. hello resulterande uppsättningen anslutningskrav är svår och tidskrävande tooimplement när du använder traditionell tekniker, till exempel webbehållare och asynkrona mäklare.

## <a name="why-use-azure-iot-hub"></a>Varför ska du använda Azure IoT Hub?

Dessutom tooa omfattande uppsättning [enhet till moln] [ lnk-d2c-guidance] och [moln till enhet] [ lnk-c2d-guidance] kommunikationsalternativ, inklusive meddelanden, fil överföringar och request-reply-metoder, Azure IoT Hub adresser hello-enhetsanslutning utmaningar i hello följande sätt:

* **Enhetstvillingar**. Med hjälp av [enhetstvillingar][lnk-twins] kan du lagra, synkronisera och fråga efter enhetsmetadata och tillståndsinformation. Enhetstvillingar är JSON-dokument som lagrar information om enhetstillstånd (metadata, konfigurationer och villkor). IoT-hubb kvarstår en enhet dubbla för varje enhet som du ansluter tooIoT hubb.

* **Autentisering per enhet och säkra anslutningar**. Du kan etablera varje enhet har sin egen [säkerhetsnyckeln] [ lnk-devguide-security] tooenable den tooconnect tooIoT hubb. Hej [IoT-hubb identitetsregistret] [ lnk-devguide-identityregistry] enheten identiteter och nycklar lagras i en lösning. En lösningens serverdel kan lägga till enskilda enheter tooallow eller neka listor tooenable fullständig kontroll över enheten.

* **Väg enhet till moln meddelanden tooAzure tjänster baserat på regler för deklarativ**. IoT-hubb kan du toodefine meddelande vägar baserat på routning regler toocontrol där din hubb skickar meddelanden från enhet till moln. Regler för routning behöver inte du toowrite någon kod och kan ske hello av anpassade efter införandet meddelandet avsändare.

* **Övervaka enhetsanslutningsåtgärder**. Du kan få detaljerade åtgärdsloggar om enhetsidentitetshanteringsåtgärder och enhetsanslutningshändelser. Den här övervakningsfunktionen gör att din IoT-lösningen tooidentify anslutningsproblem, så enheter som försöker tooconnect med fel autentiseringsuppgifter för ofta skicka meddelanden eller avvisa alla moln till enhet meddelanden.

* **En omfattande uppsättning enhetsbibliotek**. [SDK:er för Azure IoT-enheter][lnk-device-sdks] är tillgängliga och stöds för olika språk och plattformar – C för många Linux-distributioner, Windows och realtidsoperativsystem. SDK:erna för Azure IoT-enheter stöder även hanterade språk som C#, Java och JavaScript.

* **IoT-protokoll och utökningsbarhet**. Om din lösning inte kan använda hello enhetsbibliotek, exponerar IoT-hubb en offentlig protokoll som möjliggör enheter toonatively Använd hello MQTT v3.1.1, HTTP 1.1 eller AMQP 1.0-protokoll. Du kan också utöka IoT-hubb toosupport för anpassade protokoll av:

  * Skapa en gateway för fältet med [Azure IoT kant] [ lnk-iot-edge] som konverterar din anpassade protokollet tooone av hello tre protokoll tolkas av IoT-hubb.
  * Anpassa hello [Azure IoT-protokollgatewayen][protocol-gateway], en komponent med öppen källkod som körs i molnet hello.

* **Skalning**. Azure IoT-hubb skalas toomillions samtidigt anslutna enheter och miljontals händelser per sekund.

## <a name="gateways"></a>Gateways

En gateway i en IoT-lösning är vanligtvis antingen en [protokollgatewayen] [ lnk-iotedge] som har distribuerats i hello moln eller en [fältet gateway] [ lnk-field-gateway] som är distribuera lokalt med dina enheter. En gateway protocol utför översättning av protokollet, till exempel MQTT tooAMQP. Fältet gateway kan köra analytics på hello kant fatta viktiga beslut tooreduce svarstid, anger enhetshanteringstjänster, framtvinga säkerhet och sekretess begränsningar och också utföra protokollöversättning. Båda typerna av gateways fungerar som en mellanhand mellan dina enheter och IoT Hub.

En fält-gateway skiljer sig från en enkel trafikroutningsenhet (till exempel en enhet för översättning av nätverksadresser eller en brandvägg) eftersom den vanligtvis har en aktiv roll i hanteringen av åtkomst- och informationsflödet i din lösning.

En lösning kan omfatta både protokoll- och fält-gateways.

## <a name="how-does-iot-hub-work"></a>Hur fungerar IoT Hub?

Azure IoT-hubb implementerar hello [service-stödd kommunikation] [ lnk-service-assisted-pattern] mönster toomediate hello samverkan mellan dina enheter och din lösning serverdel. hello syftet med service-stödd kommunikation är tooestablish säker, dubbelriktad Kommunikationssökvägar mellan ett system, till exempel IoT-hubb och särskilda enheter som har distribuerats i ej betrodda fysiskt utrymme. hello mönster upprättar hello följande principer:

* Säkerhet prioriteras högre än alla andra funktioner.

* Enheter accepterar inte oönskad nätverksinformation. En enhet upprättar alla anslutningar och vägar som ”endast utgående”. Hello enheten måste regelbundet initiera en anslutning toocheck för alla väntande kommandon tooprocess för enheten-tooreceive ett kommando från hello lösningens serverdel.

* Enheter ansluter endast tooor upprätta vägar toowell kända tjänster som de är peerkopplat med, till exempel IoT-hubb.

* hello kommunikation sökvägen mellan enheten och tjänsten eller mellan enheter och gateway skyddas på protokollnivå för hello program.

* Auktoriseringen och autentiseringen på systemnivå baseras på enhetsspecifika identiteter. De gör att autentiseringsuppgifter och behörigheter för åtkomst kan återkallas nästan omedelbart.

* Dubbelriktad kommunikation för enheter som ansluter bara ibland på grund av toopower eller anslutningen problem underlättas genom att hålla kommandon och enhetsmeddelanden förrän en enhet ansluter tooreceive dem. IoT-hubb underhåller enhetsspecifika köer för hello-kommandon som skickas.

* Nyttolasten programdata skyddas separat för skyddade överföringen via gateways tooa viss tjänst.

hello mobila branschen har använt hello service-stödd kommunikation mönster vid enorma skala tooimplement push notification services som [Windows Push Notification Services][lnk-wns], [Google Cloud Messaging][lnk-google-messaging], och [Apple Push Notification Service][lnk-apple-push].

IoT Hub stöds över ExpressRoutes offentliga peering-sökväg.

## <a name="next-steps"></a>Nästa steg

toolearn hur toosend meddelanden från en enhet och ta emot dem från IoT-hubb, samt hur tooconfigure meddelandet dirigerar finns [skicka och ta emot meddelanden med IoT-hubben][lnk-send-messages].

toolearn hur IoT-hubb aktiverar standardbaserad enhetshantering för du tooremotely hantera, konfigurera och uppdatera dina enheter finns i [översikt över hantering av enheter med IoT-hubben][lnk-device-management].

Du kan använda hello Azure IoT-enhet SDK tooimplement klientprogram på flera olika plattformar för enheter maskinvara och operativsystem. hello enhet SDK innehåller bibliotek som underlättar skicka telemetri tooan IoT-hubb och ta emot moln till enhet meddelanden. När du använder hello enheten SDK: er, du kan välja mellan olika nätverk protokoll toocommunicate med IoT-hubben. toolearn finns fler hello [information om enhet SDK][lnk-device-sdks].

tooget startade skriva kod och köra några exempel finns hello [Kom igång med IoT-hubb] [ lnk-get-started] kursen.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Service Assisted Communication (Tjänstassisterad kommunikation) – blogginlägg av Clemens Vasters"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-iotedge]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-send-messages]: iot-hub-devguide-messaging.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-security-ground-up]: iot-hub-security-ground-up.md
