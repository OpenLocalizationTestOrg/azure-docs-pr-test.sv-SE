---
title: 'Simulerade enhet & Azure IoT Gateway - lektionen 2: registrera enhet | Microsoft Docs'
description: 
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Azure iot-hubb internet av saker moln, azure iot-hubb skapar enhet, ti sensortag, ti tivera
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 23cfbe21-22c6-4fe1-ae41-63714a897f12
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d1052ed2fa9e022966e6e71fa2c7d4f18c333b2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-azure-iot-hub-and-register-your-device"></a>Skapa din Azure IoT-hubb och registrera din enhet

## <a name="what-you-will-do"></a>Vad du ska göra

- Skapa en resursgrupp
- Skapa din första IoT-hubb
- Registrera din enhet i din IoT-hubb med hjälp av hello Azure CLI. 

När du registrerar din enhet i din IoT-hubb genererar hello Azure IoT Hub service en nyckel för din enhet toouse tooauthenticate med hello-tjänsten. 

Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här lektionen får du lära dig:

- Hur toouse hello Azure CLI toocreate en IoT-hubb.
- Hur tooregister en enhet i en IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

- En aktiv Azure-prenumeration. Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.
- Du bör ha hello Azure CLI installerad.

## <a name="create-an-iot-hub"></a>Skapa en IoT Hub

toocreate en IoT-hubb så här:

1. Logga in tooyour Azure-konto genom att köra följande kommando hello:

   ```bash
   az login
   ```

   Alla tillgängliga prenumerationer visas efter en lyckad inloggning.

2. Ange standard hello Azure-prenumeration som du vill toouse genom att köra följande kommando hello:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name`hittar du i hello utdata från hello `az login` eller hello `az account list` kommando.

3. Registrera hello providern genom att köra följande kommando hello. Resursproviders är tjänster som tillhandahåller resurser för ditt program. Du måste registrera hello providern innan du kan distribuera hello Azure-resurs som hello tjänstleverantören erbjuder.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```

4. Skapa en resursgrupp med namnet `iot-gateway` i hello västra USA region genom att köra följande kommando hello:

   ```bash
   az group create --name iot-gateway --location westus
   ```
   
   `westus`är hello-plats som du skapar din resursgrupp. Om du vill toouse till en annan plats, kan du köra `az account list-locations -o table` toosee alla hello platser har stöd för Azure.

5. Skapa en IoT-hubb i hello `iot-gateway` resursgruppen genom att köra följande kommando hello:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-gateway
   ```

Som standard skapar hello verktyget en IoT-hubb i hello kostnadsfria prisnivån. Mer information finns i [Azure IoT Hub-priser](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE]
> hello namnet på din IoT-hubb måste vara globalt unika. Du kan skapa en enda F1-versionen av Azure Iot Hub under din Azure-prenumeration.

## <a name="register-your-device-in-your-iot-hub"></a>Registrera din enhet i din IoT-hubb

Varje enhet som skickar meddelanden tooyour IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID.
Registrera din enhet i din IoT-hubb genom att köra följande kommando:

```bash
az iot device create --device-id mydevice --hub-name {my hub name} --resource-group iot-gateway
```

## <a name="summary"></a>Sammanfattning

Du har skapat en IoT-hubb och registrerat din logiska enhet med en enhetsidentitet i din IoT-hubb. Du är klar toolearn hur tooconfigure och kör en gateway exempel toosend programdata från fysisk enhet tooyour IoT-hubb i hello cloud.

## <a name="next-steps"></a>Nästa steg
[Konfigurera och köra en simulerad enhet molnet överför exempelprogrammet](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)