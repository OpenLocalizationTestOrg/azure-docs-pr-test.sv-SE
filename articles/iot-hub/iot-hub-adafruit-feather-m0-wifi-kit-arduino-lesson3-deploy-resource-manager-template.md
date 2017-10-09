---
title: 'Connect Arduino (C) tooAzure IoT - lektionen 3: malldistribution | Microsoft Docs'
description: "hello Azure funktionsapp lyssnar tooAzure IoT-hubb händelser, bearbetar inkommande meddelanden och skriver dem tooAzure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "lagra data i molnet hello data som lagras i molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 9c8f4cd1-9511-4601-ad7e-51761a986753
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 6a84a6d3c5263a85c8997cf69fe446d73ab7a5fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>Skapa en Azure funktionsapp och Azure storage-konto
[Azure Functions](../../articles/azure-functions/functions-overview.md) är en lösning för att enkelt köra *funktioner* (små delar av kod) i hello molnet. Hello körningen av dina funktioner i Azure är värd för en funktionsapp i Azure.

## <a name="what-will-you-do"></a>Vad ska du göra
Använda en Azure Resource Manager-mall toocreate en funktionsapp i Azure-och ett Azure storage-konto. hello Azure funktionsapp lyssnar tooAzure IoT-hubb händelser, bearbetar inkommande meddelanden och skriver dem tooAzure Table storage.

Om du har några problem med söka efter lösningar på hello [felsökning för Adafruit ludd M0 WiFi Arduino-kort](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).

## <a name="what-will-you-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Hur toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure resurser.
* Hur fungerar app tooprocess IoT-hubb meddelanden toouse en Azure och skriva dem tooa tabell i Azure Table storage.

## <a name="what-do-you-need"></a>Vad behöver du
Du måste ha slutfört:
- [Kom igång med Arduino-kort][get-started]
- [Skapa din Azure IoT-hubb][create-iot-hub]

## <a name="open-hello-sample-app"></a>Öppna hello sample-appen
Öppna hello exempelprojektet i Visual Studio Code genom att köra följande kommandon hello:

```bash
cd Lesson3
code .
```

![Lagringsplatsen struktur][repo-structure]

* Hej `app.ino` filen i hello `app` undermapp är hello viktiga källfilen. Den här källfilen innehåller hello kod toosend ett meddelande 20 gånger tooyour IoT-hubb och blinka hello Indikator för varje meddelande som skickas.
* Hej `config.json` innehåller konfigurationsinställningar som krävs.
* Hej `arm-template.json` filen är hello Azure Resource Manager-mall som innehåller en funktionsapp i Azure-och ett Azure storage-konto.
* Hej `arm-template-param.json` filen är hello konfigurationsfil som hello Azure Resource Manager-mall.
* Hej `ReceiveDeviceMessages` undermappen innehåller hello Node.js-kod för hello Azure-funktion.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurera Azure Resource Manager-mallar och skapa resurser i Azure
Uppdatera hello `arm-template-param.json` filen i Visual Studio-koden.

![Azure Resource Manager-mallparametrar][arm-template-params]

* Ersätt **[IoT-hubb namn]** med **{min hubbnamn}** som du angav när du [skapade din IoT-hubb och registrerat din Arduino board] [ created-iot-hub-and-registered-arduino-board].
* Ersätt **[prefix sträng för nya resurser]** med valfritt prefix som du vill använda. hello prefix garanterar hello resursnamnet är globalt unik tooavoid konflikt. Använd inte en dash eller en siffra inledande i hello prefix.

När du har uppdaterat hello `arm-template-param.json` fil, distribuera hello resurser tooAzure genom att köra följande kommando hello:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

Det tar cirka fem minuter toocreate dessa resurser. Medan hello resursen skapas, kan du gå vidare toohello nästa artikel.

## <a name="summary"></a>Sammanfattning
Du har skapat din Azure funktionen app tooprocess IoT-hubb meddelanden och ett Azure storage-konto toostore dessa meddelanden. Du kan nu distribuera och köra exemplet toosend enhet till moln hälsningsmeddelande på Arduino-kort.

## <a name="next-steps"></a>Nästa steg
[Kör ett exempel programmet toosend meddelanden från enhet till moln på Arduino-kort][send-device-to-cloud-messages]

<!-- Images and links -->

[get-started]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[create-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/repo_structure_c.png
[arm-template-params]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/arm_para_arduino.png
[created-iot-hub-and-registered-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md