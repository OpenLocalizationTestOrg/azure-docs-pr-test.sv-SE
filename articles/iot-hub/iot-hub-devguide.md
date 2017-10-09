---
title: "aaaDeveloper guide för Azure IoT-hubb | Microsoft Docs"
description: "Utvecklarhandbok för hello Azure IoT Hub innehåller diskussioner slutpunkter, säkerhet, hello identitetsregistret, hantering av enheter, direkt metoder, enheten twins, filöverföringar, jobb, hello frågespråk för IoT-hubb och meddelanden."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: d534ff9d-2de5-4995-bb2d-84a02693cb2e
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 2d3f18399e4cef6f9c4850a5caeb170a2d091a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-developer-guide"></a>Utvecklarhandbok för Azure IoT-hubb

Azure IoT Hub är en helt hanterad tjänst som hjälper till att aktivera tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning tillbaka avslutas.

Azure IoT-hubb ger dig:

* Säker kommunikation med hjälp av autentiseringsuppgifterna per enhet och åtkomstkontroll.
* Flera enhet till moln och moln till enhet storskaliga kommunikationsalternativ.
* Frågbar lagring av information om tillstånd per enhet och metadata.
* Enkelt enhetsanslutning med enhetsbibliotek för hello mest populära språk och plattformar.

Den här IoT-hubb Utvecklarhandbok innehåller hello följande artiklar:

* [Enhet till moln kommunikation vägledning] [ lnk-d2c-guidance] kan du välja mellan meddelanden från enhet till moln, enheten dubbla rapporterade egenskaper och ladda upp filen.
* [Moln till enhet kommunikation vägledning] [ lnk-c2d-guidance] kan du välja mellan direkt metoder, enheten dubbla egenskaper och moln till enhet meddelanden.
* [Enhet till moln och moln till enhet meddelanden med IoT-hubben] [ devguide-messaging] beskriver hello meddelandefunktioner (enhet till moln och moln till enhet) som visar IoT-hubb.
  * [Skicka meddelanden från enhet till moln tooIoT hubb][devguide-messages-d2c].
  * [Läsa meddelanden från enhet till moln från hello inbyggd slutpunkt][devguide-builtin].
  * [Använd anpassade slutpunkter och routningsregler för meddelanden från enhet till moln][devguide-custom].
  * [Skicka meddelanden moln till enhet från IoT-hubb][devguide-messages-c2d].
  * [Skapa och läsa IoT-hubb][devguide-format].
* [Överföra filer från en enhet] [ devguide-upload] beskriver hur du kan ladda upp filer från en enhet. hello artikeln innehåller också information om exempelvis hello kan skicka meddelanden hello överföringen.
* [Hantera identiteter för enheten i IoT-hubb] [ devguide-identities] beskriver vilken information varje IoT-hubb registret identitetslagringar och hur du kan komma åt och ändra den.
* [Kontrollera åtkomst tooIoT hubb] [ devguide-security] beskrivs hello säkerhet används toogrant åtkomst tooIoT hubb funktioner för både enheter och komponenter i molnet. hello artikeln innehåller information om hur du använder token och X.509-certifikat och information om hello behörigheter som du kan ge.
* [Använda enhetstillstånd twins toosynchronize och konfigurationer] [ devguide-device-twins] beskriver hello *enheten dubbla* begrepp och hello funktioner som det visar till exempel synkronisera en enhet med sin enhet dubbla. hello artikeln innehåller information om hello data som lagras i en delad enhet.
* [Anropa en metod som är direkt på en enhet] [ devguide-directmethods] beskriver hello livscykeln för en direkt metod, information om hur tooinvoke metoder på en enhet från din backend-app och referensen hello direkt metod på enheten.
* [Schemalägga jobb på flera enheter] [ devguide-jobs] beskriver hur du schemalägger jobb på flera enheter. hello artikeln beskriver hur toosubmit jobb som utför uppgifter som att köra en direkt metod, uppdaterar en enhet med en delad enhet. Här beskrivs också hur tooquery hello status för ett jobb.
* [Referera - Välj ett kommunikationsprotokoll] [ devguide-protocol] beskriver hello kommunikationsprotokoll att IoT-hubb som har stöd för kommunikation mellan och visar hello portar som ska vara öppen.
* [Referens - IoT-hubbslutpunkter] [ devguide-endpoints] beskriver hello olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder. hello beskrivs också hur du kan skapa ytterligare slutpunkter i din IoT-hubb och hur toouse en fältet gateway tooenable enheter anslutningen tooyour IoT-hubb slutpunkter i scenarier som inte är standard.
* [Referens - frågespråket för IoT-hubb för enheten twins, jobb och meddelanderoutning] [ devguide-query] beskriver den IoT-hubb frågespråk som möjliggör tooretrieve information från din hubb om enheten twins och jobb.
* [Referens - kvoter och begränsning] [ devguide-quotas] sammanfattar hello kvoter som anges i hello IoT-hubb-tjänsten och hello begränsning beteende som du kan förvänta dig toosee när en kvot.
* [Referera - priser] [ devguide-pricing] ger allmän information om olika SKU: er och priser för IoT-hubb och information om hur olika funktioner i IoT-hubben är avgiftsbelagda som hello meddelanden i IoT Hub.
* [Referens - enheten och tjänsten SDK] [ devguide-sdks] visar hello Azure IoT-SDK: er kan du använda toodevelop både enheten och tjänsten appar som samverkar med din IoT-hubb. hello artikeln innehåller länkar tooonline API-dokumentationen.
* [Referens - stöd för IoT-hubb MQTT] [ devguide-mqtt] innehåller detaljerad information om hur IoT-hubb stöder hello MQTT-protokollet. hello artikeln beskriver hello stöd för hello MQTT protokollet inbyggda toohello Azure IoT-SDK: er och innehåller information om hur du använder hello MQTT protokollet direkt.
* [Ordlista] [ devguide-glossary] en lista över vanliga IoT-hubb-relaterade termer.

[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md
[devguide-pricing]: iot-hub-devguide-pricing.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[devguide-messages-d2c]: iot-hub-devguide-messages-d2c.md
[devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[devguide-custom]: iot-hub-devguide-messages-read-custom.md
[devguide-messages-c2d]: iot-hub-devguide-messages-c2d.md
[devguide-format]: iot-hub-devguide-messages-construct.md
[devguide-protocol]: iot-hub-devguide-protocols.md
