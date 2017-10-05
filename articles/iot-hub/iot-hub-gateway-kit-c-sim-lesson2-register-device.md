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
ms.openlocfilehash: 5557989453eb47e4c3a287b26603eff040eb1d96
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-azure-iot-hub-and-register-your-device"></a>Skapa din Azure IoT-hubb och registrera din enhet

## <a name="what-you-will-do"></a>Vad du ska göra

- Skapa en resursgrupp
- Skapa din första IoT-hubb
- Registrera din enhet i din IoT-hubb med hjälp av Azure CLI. 

När du registrerar din enhet i din IoT-hubb skapar en nyckel för att enheten ska använda för att autentisera med tjänsten Azure IoT Hub-tjänsten. 

Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här lektionen får du lära dig:

- Hur du använder Azure CLI för att skapa en IoT-hubb.
- Hur du registrerar en enhet i en IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

- En aktiv Azure-prenumeration. Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.
- Du bör ha Azure CLI installerad.

## <a name="create-an-iot-hub"></a>Skapa en IoT Hub

Följ dessa steg om du vill skapa en IoT-hubb:

1. Logga in på ditt Azure-konto genom att köra följande kommando:

   ```bash
   az login
   ```

   Alla tillgängliga prenumerationer visas efter en lyckad inloggning.

2. Ange Azure-prenumeration som du vill använda genom att köra följande kommando:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name`hittar du i utdata från den `az login` eller `az account list` kommando.

3. Registrera providern genom att köra följande kommando. Resursproviders är tjänster som tillhandahåller resurser för ditt program. Du måste registrera providern innan du kan distribuera Azure-resurs som providern erbjuder.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```

4. Skapa en resursgrupp med namnet `iot-gateway` i USA, västra region genom att köra följande kommando:

   ```bash
   az group create --name iot-gateway --location westus
   ```
   
   `westus`är den plats som du skapar din resursgrupp. Om du vill använda en annan plats, kan du köra `az account list-locations -o table` att se alla platser har stöd för Azure.

5. Skapa en IoT-hubb i den `iot-gateway` resursgruppen genom att köra följande kommando:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-gateway
   ```

Som standard skapar verktyget en IoT-hubb i den kostnadsfria prisnivån. Mer information finns i [Azure IoT Hub-priser](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE]
> Namnet på din IoT-hubb måste vara globalt unika. Du kan skapa en enda F1-versionen av Azure Iot Hub under din Azure-prenumeration.

## <a name="register-your-device-in-your-iot-hub"></a>Registrera din enhet i din IoT-hubb

Varje enhet som skickar meddelanden till din IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID.
Registrera din enhet i din IoT-hubb genom att köra följande kommando:

```bash
az iot device create --device-id mydevice --hub-name {my hub name} --resource-group iot-gateway
```

## <a name="summary"></a>Sammanfattning

Du har skapat en IoT-hubb och registrerat din logiska enhet med en enhetsidentitet i din IoT-hubb. Du är redo att lära dig hur du konfigurerar och kör ett exempelprogram för gateway för att skicka data från den fysiska enheten till din IoT-hubb i molnet.

## <a name="next-steps"></a>Nästa steg
[Konfigurera och köra en simulerad enhet molnet överför exempelprogrammet](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)