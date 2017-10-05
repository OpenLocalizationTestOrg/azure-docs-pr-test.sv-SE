---
title: 'SensorTag enhet & Azure IoT-Gateway - Lektion 4: skapa funktionsapp | Microsoft Docs'
description: "Spara meddelanden från Intel NUC till din IoT-hubb, skriva dem till Azure Table storage och läses sedan från molnet."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "lagra data i molnet, data som lagras i molnet, iot Molntjänsten"
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
ms.openlocfilehash: 717c91e8332660f19d596c05a8a23afd8df1d51c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-storage-account"></a>Skapa en Azure-funktionsapp och ett lagringskonto

Azure Functions är en lösning för att enkelt köra _funktioner_ (små delar av kod) i molnet. Körningen av dina funktioner i Azure är värd för en funktionsapp i Azure. 

## <a name="what-you-will-do"></a>Vad du ska göra

- Använda en Azure Resource Manager-mall för att skapa en funktionsapp i Azure-och ett Azure storage-konto. Azure funktionsapp lyssnar på händelser som Azure IoT hub, bearbetar inkommande meddelanden och skriver dem till Azure Table storage.

Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).


## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här lektionen får du lära dig:

- Hur du använder Azure Resource Manager för att distribuera Azure-resurser.
- Hur du använder en funktionsapp i Azure-för att bearbeta meddelanden för IoT-hubb och skriva dem till en tabell i Azure Table storage.

## <a name="what-you-need"></a>Vad du behöver

Du måste ha slutfört tidigare erfarenheter:

- [Lektionen 1: Konfigurera Intel-NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
- [Lektionen 2: Förbereda din värddatorn och Azure IoT-hubb](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
- [Lektionen 3: Ta emot meddelanden från SensorTag och läsa meddelanden från IoT-hubb](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

## <a name="open-a-sample-app"></a>Öppna en exempelapp

Gå till din `iot-hub-c-intel-nuc-gateway-getting-started` lagringsplatsen, initiera konfigurationsfiler och öppna exempelprojektet i Visual Studio Code genom att köra följande kommando:

```bash
cd Lesson4
npm install
gulp init
code .
```

![Lagringsplatsen struktur](media/iot-hub-gateway-kit-lessons/lesson4/arm_template.png)

- Den `arm-template.json` filen är Azure Resource Manager-mallen som innehåller en funktionsapp i Azure-och ett Azure storage-konto.
- Den `arm-template-param.json` filen är den konfigurationsfil som används av Azure Resource Manager-mallen.
- Den `ReceiveDeviceMessages` undermappen innehåller Node.js-kod för Azure-funktionen.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurera Azure Resource Manager-mallar och skapa resurser i Azure

Uppdatera den `arm-template-param.json` filen i Visual Studio-koden.

![arm-mall json](media/iot-hub-gateway-kit-lessons/lesson4/arm_template_param.png)

- Ersätt `[your IoT Hub name]` med `{my hub name}` som du angav i lektionen 2.

När du har uppdaterat den `arm-template-param.json` fil, distribuera resurserna till Azure genom att köra följande kommando:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-gateway
```

Använd `iot-gateway` som värde för `{resource group name}` om du inte ändra värdet i lektionen 2.

## <a name="summary"></a>Sammanfattning

Du har skapat en funktionsapp i Azure-för att bearbeta meddelanden för IoT-hubb och ett Azure storage-konto för att lagra dessa meddelanden. Du kan läsa meddelanden som skickas av din gateway till din IoT-hubb.

## <a name="next-steps"></a>Nästa steg
[Läs meddelandena kvar i Azure Storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).
