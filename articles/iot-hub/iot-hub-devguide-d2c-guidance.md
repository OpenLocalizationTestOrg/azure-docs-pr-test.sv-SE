---
title: Azure IoT Hub-enhet till moln alternativ | Microsoft Docs
description: "Utvecklarhandbok - hjälp när du ska använda meddelanden från enhet till moln, rapporterade egenskaper eller ladda upp filen för moln till enhet kommunikation."
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
ms.date: 10/09/2017
ms.author: elioda
ms.openlocfilehash: 335928776e1e62caf2855cd5a5684ccaf37f73cd
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="device-to-cloud-communications-guidance"></a>Vägledning för enhet till moln kommunikation
Skicka information från appen enhet till lösningens serverdel, IoT-hubb visar när tre alternativ:

* [Meddelanden från enhet till moln] [ lnk-d2c] för tid serien telemetri och aviseringar.
* [Dubbla enheten har rapporterat egenskaper] [ lnk-twins] för rapportering enhetsinformation tillstånd som tillgängliga funktioner, villkor och tillståndet för tidskrävande arbetsflöden. Konfigurationen och programuppdateringar.
* [Filen överföringar] [ lnk-fileupload] filer och stora telemetri batchar har laddats upp av periodvis anslutna enheter för media eller komprimerad för att spara bandbredd.

Här följer en detaljerad jämförelse av olika kommunikationsalternativen enhet till moln.

|  | Meddelanden från enheten till molnet | Enheten dubbla rapporterade egenskaper | Filöverföringar |
| ---- | ------- | ---------- | ---- |
| Scenario | Telemetri tidsserier och aviseringar. Till exempel 256 KB sensor databatchar skickas var femte minut. | Tillgängliga funktioner och -villkor. Till exempel aktuell enhet anslutningsläget, till exempel mobilnät eller Wi-Fi. Synkroniserar tidskrävande arbetsflöden, till exempel konfiguration och programvaruuppdateringar. | Mediefiler. Stora (vanligtvis komprimerade) telemetri batchar. |
| Lagring och hämtning | Lagras tillfälligt i IoT Hub, upp till 7 dagar. Sekventiell läsning. | Lagras av IoT-hubb i dubbla för enheten. Hämta med hjälp av den [IoT-hubb frågespråket][lnk-query]. | Lagras i Azure Storage-konto för användaren. |
| Storlek | Upp till 256 KB-meddelanden. | Maximal rapporterade egenskaper storleken är 8 KB. | Maximal filstorlek som stöds av Azure Blob Storage. |
| frekvens | Hög. Mer information finns i [IoT-hubb begränsar][lnk-quotas]. | Medel. Mer information finns i [IoT-hubb begränsar][lnk-quotas]. | Låg. Mer information finns i [IoT-hubb begränsar][lnk-quotas]. |
| Protokoll | Tillgängligt på alla protokoll. | Tillgängligt med MQTT eller AMQP. | Tillgängliga när du använder alla protokoll, men kräver HTTPS på enheten. |

Det är möjligt att ett program kräver att både skicka information som en telemetri tidsserie eller meddela och göra den tillgänglig i enheten dubbla. I det här scenariot kan du välja något av följande alternativ:

* Appen enheten skickar meddelandet enhet till moln och rapporterar en Egenskapsändring.
* Lösningens serverdel kan lagra informationen på den enheten dubbla taggar när den tar emot meddelandet.

Eftersom meddelanden från enhet till moln aktiverar en mycket högre genomströmning än enhet dubbla uppdateringar, är det ibland önskvärt att undvika att uppdatera enheten dubbla för varje enhet till moln-meddelande.


[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-fileupload]: iot-hub-devguide-file-upload.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
