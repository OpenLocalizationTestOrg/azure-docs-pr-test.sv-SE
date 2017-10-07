---
title: 'Ansluta Intel modern (nod) tooAzure IoT - lektionen 2: registrera enhet | Microsoft Docs'
description: "Skapa en resursgrupp, skapa en Azure IoT-hubb och registrera modern i hello Azure IoT-hubb med hjälp av hello Azure CLI."
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
ms.openlocfilehash: a70cd8d6a6d620a2cf6226397e061af6cf81e0af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-intel-edison"></a>Skapa din IoT-hubb och registrera Intel modern
## <a name="what-you-will-do"></a>Vad du ska göra
* Skapa en resursgrupp.
* Skapa din Azure IoT-hubb i hello resursgruppen.
* Lägga till Intel modern toohello Azure IoT-hubb med hello Azure-kommandoradsgränssnittet (Azure CLI).

När du använder hello Azure CLI tooadd modern tooyour IoT-hubb genererar hello tjänsten en nyckel för modern tooauthenticate med hello-tjänsten. Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Hur toouse hello Azure CLI toocreate en IoT-hubb.
* Hur toocreate enhetsidentitet för modern i din IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver
* Ett Azure-konto. Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.
* Du bör ha hello Azure CLI installerad.

## <a name="create-your-iot-hub"></a>Skapa din IoT-hubb
Azure IoT-hubb hjälper dig att ansluta, övervaka och hantera miljontals IoT tillgångar. toocreate din IoT-hubb gör du följande:

1. Logga in tooyour Azure-konto genom att köra följande kommando hello:

   ```bash
   az login
   ```

   Alla tillgängliga prenumerationer visas efter en lyckad inloggning.

2. Ange hello standard prenumeration som du vill toouse genom att köra följande kommando hello:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name`hittar du i hello utdata från hello `az login` eller hello `az account list` kommando.

3. Registrera hello providern genom att köra följande kommando hello. Resursproviders är tjänster som tillhandahåller resurser för ditt program. Du måste registrera hello providern innan du kan distribuera hello Azure-resurs som hello tjänstleverantören erbjuder.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. Skapa en resursgrupp med namnet iot-sample i hello västra USA region genom att köra följande kommando hello:

   ```bash
   az group create --name iot-sample --location westus
   ```

   `westus`är hello-plats som du skapar din resursgrupp. Om du vill toouse till en annan plats, kan du köra `az account list-locations -o table` toosee alla hello platser har stöd för Azure.

5. Skapa en IoT-hubb i hello iot-sample resursgruppen genom att köra följande kommando hello:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

Som standard skapar hello verktyget en IoT-hubb i hello kostnadsfria prisnivån. Mer information finns i [Azure IoT Hub-priser](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE] 
> hello namnet på din IoT-hubb måste vara globalt unika.
> Du kan skapa en enda F1-versionen av Azure IoT Hub under din Azure-prenumeration.


## <a name="register-edison-in-your-iot-hub"></a>Registrera modern i din IoT-hubb
Varje enhet som skickar meddelanden tooyour IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID.

Registrera modern i din IoT-hubb genom att köra följande kommando:

```bash
az iot device create --device-id myinteledison --hub-name {my hub name}
```

## <a name="summary"></a>Sammanfattning
Du har skapat en IoT-hubb och registrerad modern med en enhetsidentitet i din IoT-hubb. Du är klar toolearn hur toosend meddelanden från modern tooyour IoT-hubb.

## <a name="next-steps"></a>Nästa steg
[Skapa en funktionsapp i Azure-och en Azure Storage-konto tooprocess och lagra IoT-hubb meddelanden][process-and-store-iot-hub-messages].


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md