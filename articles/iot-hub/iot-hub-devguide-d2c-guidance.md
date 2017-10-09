---
title: "alternativ för aaaAzure IoT-hubb enhet till moln | Microsoft Docs"
description: "Utvecklarhandbok - riktlinjer för när meddelanden från enhet till moln toouse, rapporterade egenskaper eller fil för att överföra till moln till enhet kommunikation."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 979136db-c92d-4288-870c-f305e8777bdd
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: elioda
ms.openlocfilehash: 2caefc4f59e16ad28b0d8cf4b3bb627b4cba803b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="device-to-cloud-communications-guidance"></a>Vägledning för enhet till moln kommunikation
När du skickar information från hello enheten app toohello lösningens serverdel visar IoT-hubb tre alternativ:

* [Meddelanden från enhet till moln] [ lnk-d2c] för tid serien telemetri och aviseringar.
* [Rapporterade egenskaper] [ lnk-twins] för rapportering enhetsinformation tillstånd som tillgängliga funktioner, villkor eller hello tillståndet för tidskrävande arbetsflöden. Konfigurationen och programuppdateringar.
* [Filen överföringar] [ lnk-fileupload] filer och stora telemetri batchar har laddats upp av periodvis anslutna enheter för media eller komprimerad toosave bandbredd.

Här följer en detaljerad jämförelse av hello olika kommunikationsalternativ enhet till moln.

|  | Meddelanden från enheten till molnet | Rapporterat egenskaper | Filöverföringar |
| ---- | ------- | ---------- | ---- |
| Scenario | Telemetri tidsserier och aviseringar. Till exempel 256 KB sensor databatchar skickas var femte minut. | Tillgängliga funktioner och -villkor. Till exempel hello aktuell enhet anslutningsläget, till exempel mobilnät eller Wi-Fi. Synkroniserar tidskrävande arbetsflöden, till exempel konfiguration och programvaruuppdateringar. | Mediefiler. Stora (vanligtvis komprimerade) telemetri batchar. |
| Lagring och hämtning | Lagras tillfälligt i IoT Hub in too7 dagar. Sekventiell läsning. | Lagras av IoT-hubb i hello enheten dubbla. Strängfält med hello [IoT-hubb frågespråket][lnk-query]. | Lagras i Azure Storage-konto för användaren. |
| Storlek | Konfigurera too256 KB meddelanden. | Maximal rapporterade egenskaper storleken är 8 KB. | Maximal filstorlek som stöds av Azure Blob Storage. |
| frekvens | Hög. Mer information finns i [IoT-hubb begränsar][lnk-quotas]. | Medel. Mer information finns i [IoT-hubb begränsar][lnk-quotas]. | Låg. Mer information finns i [IoT-hubb begränsar][lnk-quotas]. |
| Protokoll | Tillgängligt på alla protokoll. | För närvarande endast tillgängligt när med MQTT. | Tillgängliga när du använder alla protokoll, men kräver HTTP på hello enhet. |

Det är möjligt att ett program kräver tooboth skicka information som en telemetri tidsserier eller avisering och även toomake dem tillgängliga i hello enheten dubbla. I det här scenariot kan du välja något av följande alternativ för hello:

* Hej enhetsapp skickar meddelandet enhet till moln och rapporterar en Egenskapsändring.
* hello lösningens serverdel kan lagra hello information i hello enheten dubbla taggar när den tar emot hello-meddelande.

Eftersom meddelanden från enhet till moln aktiverar en mycket högre genomströmning än enhet dubbla uppdateringar, är det ibland värdefulla tooavoid uppdatering hello enheten dubbla för varje enhet till moln-meddelande.


[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-fileupload]: iot-hub-devguide-file-upload.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
