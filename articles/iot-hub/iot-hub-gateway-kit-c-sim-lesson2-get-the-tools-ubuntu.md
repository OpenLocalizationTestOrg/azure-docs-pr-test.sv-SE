---
title: "Simulerade enhet & Azure IoT Gateway - lektionen 2: Hämta verktyg (Ubuntu) | Microsoft Docs"
description: "Installera verktygen och programvaran på din värddator som kör Ubuntu, skapar en IoT-hubb och registrera enheten i IoT-hubben."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-utveckling, iot-programvara, iot-Molntjänsten, internet av saker programvara, azure cli git på ubuntu, gulp kör, installera node js ubuntu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: cf673154-ce67-4ed7-a9f7-2440301c6270
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 349daf5c3206f7fc20662beebd16928624142a56
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a>Hämta verktygen (Ubuntu 16.04)
> [!div class="op_single_selector"]
> * [Windows 7 eller senare](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Vad du ska göra

- Installera Git, Node.js, Gulp, Python.
- Installera Azure-kommandoradsgränssnittet (Azure CLI). 

Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).
## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här lektionen får du lära dig:

- Hur du installerar Git och Node.js.
  - Git är ett system för öppen källkod distribuerade versionen. Exempelprogram för den här lektionen lagras på Git.
  - Node.js är JavaScript-körning med ett omfattande paketet ekosystem.
- Hur du använder NPM installera Node.js utvecklingsverktyg.
  - Versionen som krävs för Node.js är 4.5 LTS.
  - NPM är en av de paket cheferna för Node.js.
- Så här installerar Visual Studio-koden.
  - Visual Studio-koden är plattformsoberoende redigerare för enkel men kraftfull källa för Windows, Linux och macOS. Det har bra stöd för felsökning, inbäddad Git-kontroll, syntaxmarkering, intelligent kod slutförande, kodavsnitt och koden eftersom samt.
- Så här installerar du Azure CLI
  - Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure. Du arbetar direkt från en kommandorad för att etablera och hantera resurser.
- Hur du använder Azure CLI för att skapa en IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

- En Internet-anslutning att hämta verktyg och program.
- En dator som kör Ubuntu 16.04 eller senare.

## <a name="install-git-and-nodejs"></a>Installera Git och Node.js

Följ dessa steg om du vill installera Git och Node.js:

1. Tryck på `Ctrl + Alt + T` att öppna en terminal.
2. Kör följande kommandon:

   ```bash
   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install git
   ```

## <a name="install-nodejs-development-tools"></a>Installera Node.js utvecklingsverktyg

Du använder [gulp.js](http://gulpjs.com/) att automatisera distribution och körning av skript.

Om du vill installera gulp, kör du följande kommando i en terminal:

```bash
sudo npm install -g gulp
```

Om du får problem med installationen finns i [felsökningsguide för](iot-hub-gateway-kit-c-sim-troubleshooting.md) efter lösningar på vanliga problem.

> [!Note]
> Noden, NPM och Gulp krävs för att köra automatiserade skript som utvecklats i Node.js.

## <a name="install-the-azure-cli"></a>Installera Azure CLI

Så här installerar du Azure CLI:

1. Kör följande kommandon i terminalen:

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```

   Installationen kan ta 5 minuter.

2. Verifiera installationen genom att köra följande kommando:

   ```bash
   az iot -h
   ```
Du bör se följande utdata om installationen har slutförts.
![Verifiera installation av Azure CLI](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)

### <a name="install-visual-studio-code"></a>Installera Visual Studio Code

Du kan använda Visual Studio Code senare under kursen för att redigera konfigurationsfiler.

[Hämta](https://code.visualstudio.com/docs/setup/linux) och installera Visual Studio-koden.

## <a name="summary"></a>Sammanfattning

Du har installerat alla verktyg som krävs och programvara på värddatorn. Nästa uppgift är att använda Azure CLI för att skapa en IoT-hubb och registrera enheten i din IoT-hubb.

## <a name="next-steps"></a>Nästa steg
[Skapa en IoT-hubb och registrera din enhet](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
