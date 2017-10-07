---
title: "IoT-hubb hur aaaAzure för | Microsoft Docs"
description: "Som en utvecklare hur använder jag hello olika funktioner i IoT-hubb?"
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: d9c6e25bb332704dee4327bcdc361a299c064130
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-iot-hub"></a>Hur toouse Azure IoT-hubb

Har du olika alternativ toolearn hur toodevelop för hello IoT-hubb tjänsten:

* Läs hello konceptuella artiklar som beskriver hello funktioner i IoT-hubb i detalj.
* Följ någon hello kurser som täcker hello olika funktioner i IoT-hubb.

## <a name="developer-guide"></a>Utvecklarguide

Som en utvecklare kan du läsa detaljerad konceptuell information om IoT-hubb i hello [Utvecklarhandbok][lnk-devguide]. Den här guiden innehåller:

* Detaljerade beskrivningar av alla IoT-hubb-funktioner som hjälper dig toolearn hur toouse dem.
* Anvisningar om hur toochoose när flera alternativ är tillgängliga.

## <a name="tutorials"></a>Självstudier

Om du föredrar toolearn om specifika IoT-hubb-funktioner genom att utföra praktiska övningarna finns flera självstudier toochoose från. Många av dessa självstudiekurser är tillgängliga i flera programmeringsspråk. Dessa självstudiekurser är:

- [IoT-hubb enhet till moln-meddelanden med hjälp av vägar][lnk-routes-tutorial]. Den här kursen visar hur toouse IoT-hubb routning regler toodispatch meddelanden från enhet till moln i ett enkelt och konfigurationsbaserade sätt.

- [Skicka meddelanden moln till enhet med IoT-hubb][lnk-c2d-tutorial]. Den här kursen visar hur toosend moln till enhet meddelanden via IoT-hubb och ta emot meddelanden moln till enhet på en enhet.

- [Ladda upp filer från enheter toohello moln med IoT-hubben][lnk-upload-tutorial]. Den här kursen visar hur toouse hello filöverföring funktionerna i IoT-hubb.

- [Kom igång med enheten twins][lnk-twin-tutorial]. Den här kursen introducerar toodevice twins, rapporterade egenskaper, önskade egenskaper och taggar. Du kan använda enheten twins toosynchronize värden med dina enheter.

- [Använda direkt metoder][lnk-methods-tutorial]. Den här kursen visar hur toouse direkt metoder. Du lägger till en hanterare för en direkt metod i den simulerade enheten och anropa hello direkta metoden från IoT-hubb.

- [Kom igång med enhetshantering][lnk-dm-tutorial]. Den här kursen visar hur toouse viktiga enhetshantering funktioner, till exempel twins och direkt metoder. Du använder dessa funktioner tooremotely omstart den simulerade enheten.

- [Använda önskade egenskaper tooconfigure enheter][lnk-properties-tutorial]. Den här kursen visar hur toouse hello enheten dubblas önskad och rapporterade egenskaper, tooremotely konfigurera din enhet.

- [Använd enheten jobb tooinitiate en firmware-uppdatering för enheten][lnk-jobs-tutorial]. Den här kursen visar hur toouse viktiga enhetshantering funktioner, till exempel twins och direkt metoder. Du lär dig hur toouse dessa funktioner tooremotely uppdatera enhetens inbyggda programvara.

- [Schema-och broadcast][lnk-schedule-tutorial]. Den här kursen visar hur toouse önskade egenskaper och metoder som direkt toointeract med flera enheter vid ett schemalagt klockslag.

## <a name="next-steps"></a>Nästa steg

toolearn mer om hello IoT-hubb-tjänsten finns hello [Utvecklarhandbok][lnk-devguide].

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-routes-tutorial]: ./iot-hub-csharp-csharp-process-d2c.md
[lnk-c2d-tutorial]: ./iot-hub-csharp-csharp-c2d.md
[lnk-upload-tutorial]: ./iot-hub-csharp-csharp-file-upload.md
[lnk-twin-tutorial]: ./iot-hub-node-node-twin-getstarted.md
[lnk-methods-tutorial]: ./iot-hub-node-node-direct-methods.md
[lnk-dm-tutorial]: ./iot-hub-node-node-device-management-get-started.md
[lnk-properties-tutorial]: ./iot-hub-node-node-twin-how-to-configure.md
[lnk-jobs-tutorial]: ./iot-hub-node-node-firmware-update.md
[lnk-schedule-tutorial]: ./iot-hub-node-node-schedule-jobs.md