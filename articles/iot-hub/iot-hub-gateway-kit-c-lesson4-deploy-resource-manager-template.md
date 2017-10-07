---
title: 'SensorTag enhet & Azure IoT-Gateway - Lektion 4: skapa funktionsapp | Microsoft Docs'
description: "Spara meddelanden från Intel NUC tooyour IoT-hubb, skrivs tooAzure Table storage och läses sedan från hello molnet."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "lagra data i molnet hello data som lagras i molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: f84f9a85-e2c4-4a92-8969-f65eb34c194e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: efee3bdc15ced104651f4a500311a5fe614267c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-storage-account"></a>Skapa en Azure-funktionsapp och ett lagringskonto

Azure Functions är en lösning för att enkelt köra _funktioner_ (små delar av kod) i hello molnet. Hello körningen av dina funktioner i Azure är värd för en funktionsapp i Azure. 

## <a name="what-you-will-do"></a>Vad du ska göra

- Använda en Azure Resource Manager-mall toocreate en funktionsapp i Azure-och ett Azure storage-konto. hello Azure funktionsapp lyssnar tooAzure IoT-hubb händelser, bearbetar inkommande meddelanden och skriver dem tooAzure Table storage.

Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).


## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här lektionen får du lära dig:

- Hur toouse Azure Resource Manager toodeploy Azure-resurser.
- Hur fungerar app tooprocess IoT-hubb meddelanden toouse en Azure och skriva dem tooa tabell i Azure Table storage.

## <a name="what-you-need"></a>Vad du behöver

Du måste ha slutfört hello tidigare erfarenheter:

- [Lektionen 1: Konfigurera Intel-NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
- [Lektionen 2: Förbereda din värddatorn och Azure IoT-hubb](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
- [Lektionen 3: Ta emot meddelanden från SensorTag och läsa meddelanden från IoT-hubb](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

## <a name="open-a-sample-app"></a>Öppna en exempelapp

Gå tooyour `iot-hub-c-intel-nuc-gateway-getting-started` lagringsplatsen mappen, initiera hello-konfigurationsfiler och öppna sedan hello exempelprojektet i Visual Studio Code genom att köra följande kommando hello:

```bash
cd Lesson4
npm install
gulp init
code .
```

![Lagringsplatsen struktur](media/iot-hub-gateway-kit-lessons/lesson4/arm_template.png)

- Hej `arm-template.json` filen är hello Azure Resource Manager-mall som innehåller en funktionsapp i Azure-och ett Azure storage-konto.
- Hej `arm-template-param.json` filen är hello konfigurationsfil som hello Azure Resource Manager-mall.
- Hej `ReceiveDeviceMessages` undermappen innehåller hello Node.js-kod för hello Azure-funktion.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurera Azure Resource Manager-mallar och skapa resurser i Azure

Uppdatera hello `arm-template-param.json` filen i Visual Studio-koden.

![arm-mall json](media/iot-hub-gateway-kit-lessons/lesson4/arm_template_param.png)

- Ersätt `[your IoT Hub name]` med `{my hub name}` som du angav i lektionen 2.

När du har uppdaterat hello `arm-template-param.json` fil, distribuera hello resurser tooAzure genom att köra följande kommando hello:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-gateway
```

Använd `iot-gateway` som hello värde för `{resource group name}` om du inte ändrar hello värdet i lektionen 2.

## <a name="summary"></a>Sammanfattning

Du har skapat din Azure funktionen app tooprocess IoT-hubb meddelanden och ett Azure storage-konto toostore dessa meddelanden. Du kan läsa meddelanden som skickas av din gateway tooyour IoT-hubb.

## <a name="next-steps"></a>Nästa steg
[Läs meddelandena kvar i Azure Storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).
