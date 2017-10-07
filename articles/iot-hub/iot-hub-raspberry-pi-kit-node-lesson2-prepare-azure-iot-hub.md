---
featureFlags: usabilla
title: 'Ansluta hallon Pi (nod) tooAzure IoT - lektionen 2: registrera enhet | Microsoft Docs'
description: "Skapa en resursgrupp, skapa en Azure IoT-hubb och registrera Pi i hello IoT-hubb identitetsregistret med hjälp av Azure CLI."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: ansluta till Raspberry pi moln, pi-moln
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 736215b6-e7e4-46f9-af30-0ded9ffa5204
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 97533298d52d1187c49a4c35ddda922d6e45c87d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a>Skapa din IoT-hubb och registrera hallon Pi 3
## <a name="what-you-will-do"></a>Vad du ska göra
* Skapa en resursgrupp.
* Skapa din Azure IoT-hubb i hello resursgruppen.
* Lägga till hallon Pi 3 toohello Azure IoT-hubb med hello Azure-kommandoradsgränssnittet (Azure CLI).

När du använder Azure CLI tooadd Pi tooyour IoT-hubb genererar hello tjänsten en nyckel för Pi tooauthenticate med hello-tjänsten. Om du har några problem med sökning lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Hur toouse Azure CLI toocreate en IoT-hubb
* Hur toocreate enhetsidentitet för Pi i din IoT-hubb

## <a name="what-you-need"></a>Vad du behöver
* Ett Azure-konto
* En Mac- eller en Windows-dator med hello Azure CLI installerat

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
> hello namnet på din IoT-hubb måste vara globalt unika. Du kan skapa en enda F1-versionen av Azure IoT Hub under din Azure-prenumeration.

## <a name="register-pi-in-your-iot-hub"></a>Registrera Pi i din IoT-hubb
Varje enhet som skickar meddelanden tooyour IoT-hubb och tar emot meddelanden från din IoT-hubb måste registreras med ett unikt ID. Du kommer att använda Azure CLI tooregister din Pi och skapa ett självsignerat X.509-certifikat för autentisering.

Kör följande kommando hello:

```bash
# For Windows command prompt
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir %USERPROFILE%\.iot-hub-getting-started
 
# For macOS or Ubuntu
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir ~/.iot-hub-getting-started
```

## <a name="summary"></a>Sammanfattning
Du har skapat en IoT-hubb och registrerad Pi med en enhetsidentitet i din IoT-hubb. Du är klar toolearn hur toosend meddelanden från Pi tooyour IoT-hubb.

## <a name="next-steps"></a>Nästa steg
[Skapa en funktionsapp i Azure-och ett Azure storage-konto tooprocess och lagra IoT-hubb meddelanden](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

