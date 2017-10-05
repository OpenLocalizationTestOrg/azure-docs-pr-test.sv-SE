---
title: 'Connect Intel EDISON (C) till Azure IoT - lektionen 3: skapa funktionsapp | Microsoft Docs'
description: "Azure funktionsapp lyssnar på händelser som Azure IoT hub, bearbetar inkommande meddelanden och skriver dem till Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "lagra data i molnet, data som lagras i molnet, iot Molntjänsten"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 739b82e9-5d4e-4485-8971-f57cbb682faf
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 30018cc2d03fc19c13625ef066989a89f21800e9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>Skapa en Azure funktionsapp och Azure storage-konto
[Azure Functions](../../articles/azure-functions/functions-overview.md) är en lösning för att enkelt köra *funktioner* (små delar av kod) i molnet. Körningen av dina funktioner i Azure är värd för en funktionsapp i Azure.

## <a name="what-will-you-do"></a>Vad ska du göra
Använda en Azure Resource Manager-mall för att skapa en funktionsapp i Azure-och ett Azure storage-konto. Azure funktionsapp lyssnar på händelser som Azure IoT hub, bearbetar inkommande meddelanden och skriver dem till Azure Table storage. Storage-konto används för att läsa de beständiga kopiorna av meddelanden från Azure-tabellen. Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].

## <a name="what-will-you-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Hur du använder [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) att distribuera Azure-resurser.
* Hur du använder en funktionsapp i Azure-att bearbeta meddelanden för IoT-hubb och skriva dem till en tabell i Azure Table storage.

## <a name="what-do-you-need"></a>Vad behöver du
Du måste ha slutfört:
- [Kom igång med Intel-modern][get-started-with-your-intel-edison]
- [Skapa din Azure IoT-hubb][create-your-azure-iot-hub]

## <a name="open-the-sample-app"></a>Öppna sample-appen
Öppna exempelprojektet i Visual Studio Code genom att köra följande kommandon:

```bash
cd Lesson3
code .
```

![Lagringsplatsen struktur][repo-structure]

* Filen i den `app` undermapp är viktiga källfilen. Den här källfilen innehåller koden för att skicka ett meddelande till 20 gånger till din IoT-hubb och blinka Indikator för varje meddelande som skickas.
* Den `arm-template.json` filen är Azure Resource Manager-mallen som innehåller en funktionsapp i Azure-och ett Azure storage-konto.
* Den `arm-template-param.json` filen är den konfigurationsfil som används av Azure Resource Manager-mallen.
* Den `ReceiveDeviceMessages` undermappen innehåller Node.js-kod för Azure-funktionen.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurera Azure Resource Manager-mallar och skapa resurser i Azure
Uppdatera den `arm-template-param.json` filen i Visual Studio-koden.

![Azure Resource Manager-mallparametrar][arm-template-parameters]

* Ersätt **[IoT-hubb namn]** med **{min hubbnamn}** som du angav när du [skapade din IoT-hubb och registrerad Intel modern][created-your-iot-hub-and-registered-intel-edison].
* Ersätt **[prefix sträng för nya resurser]** med valfritt prefix som du vill använda. Prefixet gör att resursnamnet globalt unikt för att undvika konflikter. Använd inte en dash eller en siffra inledande i prefixet.

När du har uppdaterat den `arm-template-param.json` fil, distribuera resurserna till Azure genom att köra följande kommando:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

Det tar cirka fem minuter för att skapa dessa resurser. När resursen skapas pågår, kan du gå vidare till nästa artikel.

## <a name="summary"></a>Sammanfattning
Du har skapat en funktionsapp i Azure-för att bearbeta meddelanden för IoT-hubb och ett Azure storage-konto för att lagra dessa meddelanden. Du kan nu distribuera och köra exemplet för att skicka meddelanden från enhet till moln för modern.

## <a name="next-steps"></a>Nästa steg
[Kör ett exempelprogram för att skicka meddelanden från enhet till moln på Intel modern][send-device-to-cloud-messages].
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-started-with-your-intel-edison]: iot-hub-intel-edison-kit-c-get-started.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-c-get-started.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson3/repo_structure_c.png
[arm-template-parameters]: /media/iot-hub-intel-edison-lessons/lesson3/arm_para_c.png
[created-your-iot-hub-and-registered-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-intel-edison-kit-c-lesson3-run-azure-blink.md