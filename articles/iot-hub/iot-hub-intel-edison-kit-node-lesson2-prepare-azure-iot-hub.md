---
title: 'Ansluta Intel modern (nod) till Azure IoT - lektionen 2: registrera enhet | Microsoft Docs'
description: "Skapa en resursgrupp, skapa en Azure IoT-hubb och registrera modern i Azure IoT-hubb med hjälp av Azure CLI."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: c1465cc2-d0d9-4326-967c-64d95b61dd54
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 81584c1b7314c4331ac059b8e3ed782970588ae2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-intel-edison"></a>Skapa din IoT-hubb och registrera Intel modern
## <a name="what-you-will-do"></a>Vad du ska göra
* Skapa en resursgrupp.
* Skapa din Azure IoT-hubb i resursgruppen.
* Lägg till Intel modern Azure IoT-hubben med hjälp av Azure-kommandoradsgränssnittet (Azure CLI).

När du använder Azure CLI för att lägga till modern din IoT-hubb kan genererar tjänsten en nyckel för modern att autentisera med tjänsten. Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Hur du använder Azure CLI för att skapa en IoT-hubb.
* Så här skapar du en enhetsidentitet för modern i din IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver
* Ett Azure-konto. Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.
* Du bör ha Azure CLI installerad.

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
> Namnet på din IoT-hubb måste vara globalt unika.
> Du kan skapa en enda F1-versionen av Azure IoT Hub under din Azure-prenumeration.


## <a name="register-edison-in-your-iot-hub"></a>Registrera modern i din IoT-hubb
Varje enhet som skickar meddelanden till din IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID.

Registrera modern i din IoT-hubb genom att köra följande kommando:

```bash
az iot device create --device-id myinteledison --hub-name {my hub name}
```

## <a name="summary"></a>Sammanfattning
Du har skapat en IoT-hubb och registrerad modern med en enhetsidentitet i din IoT-hubb. Du är redo att lära dig hur du skickar meddelanden från modern till din IoT-hubb.

## <a name="next-steps"></a>Nästa steg
[Skapa en funktionsapp i Azure-och ett Azure Storage-konto för att bearbeta och lagra IoT-hubb meddelanden][process-and-store-iot-hub-messages].


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md