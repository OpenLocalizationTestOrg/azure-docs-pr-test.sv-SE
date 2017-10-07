---
title: "SensorTag enhet & Azure IoT-Gateway - lektion 2: Hämta verktyg (Ubuntu) | Microsoft Docs"
description: "Installera hello verktyg och hello programvara på din värddator som kör Ubuntu, skapar en IoT-hubb och registrera enheten i hello IoT-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-utveckling, iot-programvara, iot-Molntjänsten, internet av saker programvara, azure cli git på ubuntu, gulp kör, installera node js ubuntu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 0bac1412-385b-4255-a33f-9d44c35feb3e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c9edca91e791ef914b1920178b66eadd12ae0281
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a>Hämta hello verktyg (Ubuntu 16.04)
> [!div class="op_single_selector"]
> * [Windows 7 eller senare](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Vad du ska göra

- Installera Git, Node.js, Gulp, Python.
- Installera hello Azure-kommandoradsgränssnittet (Azure CLI). 

Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).
## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här lektionen får du lära dig:

- Hur tooinstall Git och Node.js.
  - Git är ett system för öppen källkod distribuerade versionen. hello-exempelprogram för den här lektionen lagras på Git.
  - Node.js är JavaScript-körning med ett omfattande paketet ekosystem.
- Hur toouse NPM tooinstall Node.js utvecklingsverktyg.
  - hello version som krävs av Node.js är 4.5 LTS.
  - NPM är en av hello paketet chefer för Node.js.
- Hur tooinstall Visual Studio Code.
  - Visual Studio-koden är plattformsoberoende redigerare för enkel men kraftfull källa för Windows, Linux och macOS. Det har bra stöd för felsökning, inbäddad Git-kontroll, syntaxmarkering, intelligent kod slutförande, kodavsnitt och koden eftersom samt.
- Hur tooinstall hello Azure CLI
  - hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure. Du arbetar direkt från en kommandorad tooprovision och hantera resurser.
- Hur toouse hello Azure CLI toocreate en IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

- En Internet-anslutning toodownload hello verktyg och program.
- En dator som kör Ubuntu 16.04 eller senare.

## <a name="install-git-and-nodejs"></a>Installera Git och Node.js

tooinstall Git och Node.js, gör du följande:

1. Tryck på `Ctrl + Alt + T` tooopen en terminal.
2. Kör följande kommandon hello:

   ```bash
   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install git
   ```

## <a name="install-nodejs-development-tools"></a>Installera Node.js utvecklingsverktyg

Du använder [gulp.js](http://gulpjs.com/) tooautomate distribution och körning av skript.

tooinstall gulp kör följande kommando i hello terminal hello:

```bash
sudo npm install -g gulp
```

Om du får problem med hello installation finns hello [felsökningsguide för](iot-hub-gateway-kit-c-troubleshooting.md) för lösningar toocommon problem.

> [!Note]
> Noden, NPM och Gulp är obligatoriska toorun automatiseringsskript utvecklats i Node.js.

## <a name="install-hello-azure-cli"></a>Installera hello Azure CLI

tooinstall hello Azure CLI, Följ dessa steg:

1. Kör följande kommandon i hello terminal hello:

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```

   hello-installationen kan ta 5 minuter.

2. Kontrollera hello installationen genom att köra följande kommando hello:

   ```bash
   az iot -h
   ```
Du bör se hello följande utdata om hello-installationen har slutförts.
![Verifiera installation av Azure CLI](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)

### <a name="install-visual-studio-code"></a>Installera Visual Studio Code

Du kan använda Visual Studio Code senare i hello självstudiekursen tooedit konfigurationsfiler.

[Hämta](https://code.visualstudio.com/docs/setup/linux) och installera Visual Studio-koden.

## <a name="summary"></a>Sammanfattning

Du har installerat alla hello krävs verktyg och program på värddatorn. Nästa uppgift är toouse hello Azure CLI toocreate en IoT-hubb och registrera enheten i din IoT-hubb.

## <a name="next-steps"></a>Nästa steg
[Skapa en IoT-hubb och registrera din enhet](iot-hub-gateway-kit-c-lesson2-register-device.md)
