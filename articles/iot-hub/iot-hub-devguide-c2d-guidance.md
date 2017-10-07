---
title: "alternativ för aaaAzure IoT-hubb moln till enhet | Microsoft Docs"
description: "Utvecklarhandbok - riktlinjer för när toouse direkt metoder, enheten dubbla önskade egenskaper eller moln till enhet meddelanden för moln till enhet kommunikation."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: 1ac90923-1edf-4134-bbd4-77fee9b68d24
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: elioda
ms.openlocfilehash: bb95445054fa2711e34fc1d928c3665e0246c81c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-to-device-communications-guidance"></a>Vägledning för kommunikation moln till enhet
IoT-hubb finns tre alternativ för enheten appar tooexpose funktioner tooa backend-app:

* [Dirigera metoder] [ lnk-methods] för kommunikation som kräver omedelbar bekräftelse av hello resultat. Direkt-metoder används ofta för interaktiv kontroll av enheter, till exempel aktivera fläktar.
* [Dubbla har önskade egenskaper] [ lnk-twins] för tidskrävande kommandon avsedd tooput hello enhet till en viss önskad tillstånd. Till exempel hello telemetri skicka too30 minuter.
* [Moln till enhet meddelanden] [ lnk-c2d] för enkelriktade meddelanden toohello enhetsapp.

Här följer en detaljerad jämförelse av hello olika moln till enhet kommunikationsalternativ.

|  | Direkta metoder | Dubblas egenskaper | Meddelanden moln till enhet |
| ---- | ------- | ---------- | ---- |
| Scenario | Kommandon som kräver omedelbar bekräftelse, till exempel aktivera fläktar. | Långvariga kommandon avsedd tooput hello enheten till ett visst tillstånd. Till exempel hello telemetri skicka too30 minuter. | Enkelriktade meddelanden toohello enhetsapp. |
| Dataflöde | Dubbelriktad. hello enhetsapp svara toohello metoden direkt. hello lösningens serverdel tar emot hello resultatet sammanhang toohello begäran. | Enkelriktad. hello enheten appen tar emot ett meddelande med hello-egenskapen ändras. | Enkelriktad. hello enheten appen tar emot hello-meddelande
| Hållbarhet | Frånkopplade enheter kontaktas inte. hello lösningens serverdel meddelas hello enheten inte är ansluten. | Egenskapsvärden bevaras i hello enheten dubbla. Enheten läses den vid nästa återanslutning. Egenskapsvärden är strängfält med hello [IoT-hubb frågespråket][lnk-query]. | Meddelanden kan behållas av IoT-hubb för in too48 timmar. |
| Mål | Enhet med hjälp av **deviceId**, eller flera enheter med hjälp av [jobb][lnk-jobs]. | Enhet med hjälp av **deviceId**, eller flera enheter med hjälp av [jobb][lnk-jobs]. | Enskild enhet av **deviceId**. |
| Storlek | Konfigurera too8KB begäran och svar om 8KB. | Maximalt önskade egenskaper storleken är 8KB. | Konfigurera too64KB meddelanden. |
| frekvens | Hög. Mer information finns i [IoT-hubb begränsar][lnk-quotas]. | Medel. Mer information finns i [IoT-hubb begränsar][lnk-quotas]. | Låg. Mer information finns i [IoT-hubb begränsar][lnk-quotas]. |
| Protokoll | För närvarande endast tillgängligt när med MQTT. | För närvarande endast tillgängligt när med MQTT. | Tillgängligt på alla protokoll. Enheten måste avsöka när du använder HTTP. |

Lär dig hur toouse direkt metoder, egenskaper och moln till enhet meddelanden i hello följande kurser:

* [Använda direkt metoder][lnk-methods-tutorial], för direkta metoder.
* [Använda önskade egenskaper tooconfigure enheter][lnk-twin-properties]för enheten dubbla har önskade egenskaper. 
* [Skicka meddelanden moln till enhet][lnk-c2d-tutorial], för moln till enhet meddelanden.

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-tutorial]: iot-hub-node-node-c2d.md
