---
title: aaaUnderstand Azure IoT Hub-meddelandeformat | Microsoft Docs
description: "Utvecklarhandbok - beskrivs hello format och förväntade innehållet i IoT-hubb meddelanden."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 3b1567e47bc32f70c0c252996648c4035ae81243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-read-iot-hub-messages"></a>Skapa och läsa IoT-hubb

toosupport smidig samverkan mellan protokoll IoT-hubb definierar en gemensam meddelandeformat för alla enheter riktade protokoll. Formatet som används för både [enhet till moln] [ lnk-d2c] och [moln till enhet] [ lnk-c2d] meddelanden. En [IoT-hubb meddelandet] [ lnk-messaging] består av:

* En uppsättning *Systemegenskaper*. Egenskaper som IoT-hubb tolkar eller anger. Den här uppsättningen är förinställda.
* En uppsättning *programegenskaper*. En ordlista med egenskaper för anslutningssträngar som hello program kan definiera och åtkomst utan att behöva toodeserialize hello meddelandetexten. IoT-hubb ändrar aldrig dessa egenskaper.
* En täckande binära brödtext.

Egenskapsnamn och värden kan endast innehålla alfanumeriska ASCII-tecken, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` när du:

* Skicka meddelanden från enhet till moln hello HTTP-protokollet.
* Skicka meddelanden moln till enhet.

Mer information om hur tooencode och avkoda meddelanden som skickas med olika protokoll, se [Azure IoT SDK][lnk-sdks].

hello följande tabell visar hello uppsättning egenskaper i IoT-hubb-meddelanden.

| Egenskap | Beskrivning |
| --- | --- |
| messageId |En användare går identifierare för hello-meddelande som används för request-reply-mönster. Format: En skiftlägeskänslig sträng (upp too128 tecken) av alfanumeriska tecken, ASCII-7-bitars + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Sekvensnummer |En siffra (unika per enhet kö) tilldelas av IoT-hubb tooeach moln till enhet meddelandet. |
| för|Ett mål som anges i [moln till enhet] [ lnk-c2d] meddelanden. |
| ExpiryTimeUtc |Datum och tid för meddelandet upphör att gälla. |
| EnqueuedTime |Datum och tid hello [moln till enhet] [ lnk-c2d] meddelande togs emot av IoT-hubb. |
| CorrelationId |En strängegenskap i ett svarsmeddelande som vanligtvis innehåller hello MessageId hello begäran i request-reply-mönster. |
| Användar-ID |Ett ID som toospecify hello ursprung av meddelanden. När meddelanden som genereras av IoT-hubb anges för`{iot hub name}`. |
| Ack |En feedback generatorn för meddelandet. Den här egenskapen används i moln till enhet meddelanden toorequest IoT-hubb toogenerate feedback meddelanden på grund av hello förbrukningen av hello-meddelande av hello enhet. Möjliga värden: **ingen** (standard): ingen feedback-meddelandet genereras **positivt**: ett meddelande feedback om hello-meddelande slutfördes **negativt**: ta emot en feedback meddelande om hello-meddelande har upphört att gälla (eller leverans av maximalt antal nåddes) utan slutförs efter hello enhet eller **fullständig**: både positiva och negativa. Mer information finns i [meddelande feedback][lnk-feedback]. |
| ConnectionDeviceId |Ett ID som angetts av IoT-hubb på meddelanden från enhet till moln. Den innehåller hello **deviceId** för hello-enhet som skickas hello-meddelande. |
| ConnectionDeviceGenerationId |Ett ID som angetts av IoT-hubb på meddelanden från enhet till moln. Den innehåller hello **generationId** (enligt [identitet enhetsegenskaper][lnk-device-properties]) för hello-enhet som skickas hello-meddelande. |
| ConnectionAuthMethod |En autentiseringsmetod som angetts av IoT-hubb på meddelanden från enhet till moln. Den här egenskapen innehåller information om hello autentisering metod som används för tooauthenticate hello enhet som skickar hello-meddelande. Mer information finns i [enhet toocloud skydd mot förfalskning][lnk-antispoofing]. |

## <a name="message-size"></a>Meddelandestorlek

IoT-hubb mäter meddelandestorlek på ett sätt som protokoll-oberoende överväger endast hello faktiska nyttolast. hello storlek i byte beräknas som hello summan av hello följande:

* hello brödtext storlek i byte.
* hello storlek i byte för alla hello-värden i Systemegenskaper för hello-meddelande.
* hello storlek i byte för alla användare egenskapsnamn och värden.

Egenskapsnamn och värden är begränsad tooASCII tecken, så hello tidslängd hello strängar är lika hello storlek i byte.

## <a name="next-steps"></a>Nästa steg

Information om storleksgränser för meddelanden i IoT-hubb finns [IoT-hubb kvoter och begränsning][lnk-quotas].

toolearn hur toocreate och Läs IoT-hubb meddelanden i olika programmeringsspråk, se hello [Kom igång] [ lnk-get-started] självstudier.

[lnk-messaging]: iot-hub-devguide-messaging.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-feedback]: iot-hub-devguide-messages-c2d.md#message-feedback
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-antispoofing]: iot-hub-devguide-messages-d2c.md#anti-spoofing-properties
