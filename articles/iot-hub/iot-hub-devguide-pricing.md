---
title: aaaUnderstand Azure IoT Hub-priser | Microsoft Docs
description: "Utvecklarhandbok - information om hur fungerade avläsning och prissättning fungerar med IoT-hubben inklusive exempel."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1ac90923-1edf-4134-bbd4-77fee9b68d24
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2016
ms.author: elioda
ms.openlocfilehash: e294c0b7f483e042ca3f63e93c14e0c2d773ae7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-pricing-information"></a>Information om priser för Azure IoT-hubb

[Azure IoT-hubb priser] [ lnk-pricing] ger hello allmän information om olika SKU: er och priser för IoT-hubb. Den här artikeln innehåller ytterligare information om hur olika funktioner i IoT-hubben är avgiftsbelagda som hello meddelanden i IoT Hub.

## <a name="charges-per-operation"></a>Avgifter per åtgärd

| Åtgärd | Faktureringsinformation | 
| --------- | ------------------- |
| Identitetsregisteråtgärder <br/> (skapa, hämta, visa, uppdatera, ta bort) | Inte debiteras. |
| Meddelanden från enheten till molnet | Skickade meddelanden debiteras i 4 KB-block på ingång i IoT Hub, till exempel ett meddelande på 6 KB debiteras 2 meddelanden. |
| Meddelanden moln till enhet | Skickade meddelanden debiteras i 4 KB-block, till exempel ett meddelande på 6 KB debiteras 2 meddelanden. |
| Filöverföringar | Överför tooAzure lagring inte mäts i IoT Hub-filen. Filen överföring stöd för start och slutförande meddelanden debiteras som postmeddelandet förbrukade i steg 4 KB. Till exempel debiteras överföra en fil på 10 MB två meddelanden i tillägg toohello Azure Storage kostnad. |
| Direkta metoder | Lyckad metodbegäranden debiteras i 4 KB-block, debiteras svar med icke-tom organ i 4 KB som ytterligare meddelanden. Begäranden toodisconnected enheter debiteras som meddelanden i 4 KB-block. Till exempel debiteras en metod med 6 KB brödtext som resulterar i ett svar med ingen brödtext från hello enhet, som två meddelanden. en metod med 6 KB brödtext som resulterar i ett 1 KB svar från hello enhet debiteras som två meddelanden för hello begäran plus ett annat meddelande för hello-svaret. |
| Läsoperationer för enhetstvilling | Enheten dubbla läser från hello enheten och hello-lösningen slutet debiteras som meddelanden i 512 byte-segment. Till exempel debiteras läsa en 6 KB enheten dubbla som 12 meddelanden. |
| Dubbla uppdateringar (taggar och egenskaper) | Dubbla uppdateringar från hello enheten och enheten hello debiteras som meddelanden i 512 byte-segment. Till exempel debiteras läsa en 6 KB enheten dubbla som 12 meddelanden. |
| Enheten dubbla frågor | Frågor debiteras som meddelanden beroende på hello storlek i 512 byte-block. |
| Jobbåtgärder <br/> (skapa, uppdatera, visa, ta bort) | Inte debiteras. |
| Åtgärder för jobb per enhet | Jobb-åtgärder (till exempel dubbla uppdateringar och metoder) debiteras som vanligt. Till exempel debiteras ett jobb som ledde till 1000 metodanrop med 1 KB och -svar för tom brödtext 1000 meddelanden. |

> [!NOTE]
> Alla storlekar beräknas överväger hello nyttolastens storlek i byte (protokollet synkroniseringstecken ignoreras). Vid meddelanden (som har egenskaper och brödtext) beräknas hello storlek på ett protokoll-oberoende sätt, enligt beskrivningen i hello [IoT-hubb messaging Utvecklingsguide][lnk-message-size].

## <a name="example-1"></a>Exempel #1

En enhet skickar ett meddelande om 1 KB enhet till moln per minut tooIoT hubben, som läses sedan av Azure Stream Analytics. hello lösningens serverdel anropar en metod (med 512 byte-nyttolast) på hello enheten var tionde minut tootrigger en specifik åtgärd. hello enheten svarar toohello metod med ett resultat av 200 byte.

hello enheten förbrukar 1 meddelande * 60 minuter * 24 timmar = 1440 meddelanden per dag för hälsningsmeddelande för enhet till moln och 2 begäran plus svar * 6 gånger i timmen * 24 timmar = 288 meddelanden för hello metoder för totalt 1728 meddelanden per dag.

## <a name="example-2"></a>Exempel #2

En enhet skickar en 100 KB enhet till moln meddelande varje timme. Uppdaterar den även dess enhet dubbla med 1 KB nyttolaster var fjärde timme. hello-lösningen en gång per dag, avsluta läsningar hello 14 KB enheten dubbla och uppdaterar med 512 byte-nyttolaster toochange konfigurationer.

hello enheten förbrukar 25 (100KB / 4KB) meddelanden * 24 timmar för meddelanden från enhet till moln plus 1 meddelande * 6 gånger per dag för uppdatering av enheter dubbla totalt 156 meddelanden per dag.
hello lösningens serverdel förbrukar 28 meddelanden (14KB/0,5 KB) tooread hello enheten dubbla plus 1 meddelande tooupdate den 29 meddelanden totalt.

Sammanlagt hello enheten och hello lösningens serverdel kan du använda 185 meddelanden per dag.


[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-message-size]: iot-hub-devguide-messages-construct.md
