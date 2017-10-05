---
title: 'Connect Raspberry PI (C) till Azure IoT - lektionen 2: registrera enhet | Microsoft Docs'
description: "Skapa en resursgrupp, skapa en Azure IoT-hubb och registrera Pi i Azure IoT-hubb med hjälp av Azure CLI."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: ansluta till Raspberry pi moln, pi-moln
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 4bcfb071-b3ae-48cc-8ea5-7e7434732287
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d7bfd8f6ae8d15dfe09f06a40a4ab415ff2e0a7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a>Skapa din IoT-hubb och registrera hallon Pi 3
## <a name="what-you-will-do"></a>Vad du ska göra
* Skapa en resursgrupp.
* Skapa din Azure IoT-hubb i resursgruppen.
* Lägg till hallon Pi 3 Azure IoT-hubben med hjälp av Azure-kommandoradsgränssnittet (Azure CLI).

När du använder Azure CLI för att lägga till Pi din IoT-hubb kan genererar tjänsten en nyckel för Pi att autentisera med tjänsten. Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Hur du använder Azure CLI för att skapa en IoT-hubb.
* Så här skapar du en enhetsidentitet för Pi i din IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver
* Ett Azure-konto
* En Mac- eller en Windows-dator med Azure CLI installerad

## <a name="create-your-iot-hub"></a>Skapa din IoT-hubb
Azure IoT-hubb hjälper dig att ansluta, övervaka och hantera miljontals IoT tillgångar. Följ dessa steg för att skapa din IoT-hubb:

1. Logga in på ditt Azure-konto genom att köra följande kommando:

   ```bash
   az login
   ```

   Alla tillgängliga prenumerationer visas efter en lyckad inloggning.

2. Ange den standard-prenumeration som du vill använda genom att köra följande kommando:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name`hittar du i utdata från den `az login` eller `az account list` kommando.

3. Registrera providern genom att köra följande kommando. Resursproviders är tjänster som tillhandahåller resurser för ditt program. Du måste registrera providern innan du kan distribuera Azure-resurs som providern erbjuder.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. Skapa en resursgrupp med namnet iot-sample i USA, västra region genom att köra följande kommando:

   ```bash
   az group create --name iot-sample --location westus
   ```

   `westus`är den plats som du skapar din resursgrupp. Om du vill använda en annan plats, kan du köra `az account list-locations -o table` att se alla platser har stöd för Azure.
 
5. Skapa en IoT-hubb i iot-sample resursgruppen genom att köra följande kommando:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

   Som standard skapar verktyget en IoT-hubb i den kostnadsfria prisnivån. Mer information finns i [Azure IoT Hub-priser](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE]
> Namnet på din IoT-hubb måste vara globalt unika. Du kan skapa en enda F1-versionen av Azure IoT Hub under din Azure-prenumeration.

## <a name="register-pi-in-your-iot-hub"></a>Registrera Pi i din IoT-hubb
Varje enhet som skickar meddelanden till din IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID.

Registrera Pi i din hubb genom att köra följande kommando:

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="summary"></a>Sammanfattning
Du har skapat en IoT-hubb och registrerad Pi med en enhetsidentitet i din IoT-hubb. Du är redo att lära dig hur du skickar meddelanden från Pi till din IoT-hubb.

## <a name="next-steps"></a>Nästa steg
[Skapa en funktionsapp i Azure-och ett Azure Storage-konto för att bearbeta och lagra IoT-hubb meddelanden](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).

