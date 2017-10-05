---
title: "Simulerade enhet & Azure IoT Gateway - lektionen 2: Hämta verktyg (macOS) | Microsoft Docs"
description: "Installera verktyg på Mac-dator, skapa en IoT-hubb och registrera enheten i IoT-hubben."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-utveckling, iot program, iot-Molntjänsten, internet saker programvara, azure cli, installera python mac, installera git på mac gulp kör, installera node js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 42f9d186-e20c-4ef9-98cc-71d39e058b06
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d86332816130de7a6951a74ceb215c8ce476d5f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos"></a>Skaffa dig verktyg (macOS)
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

- Så här installerar du [Git](https://git-scm.com/) och [Node.js](https://nodejs.org/en/).
  - Git är ett system för öppen källkod distribuerade versionen. Exempelprogram för den här lektionen lagras på Git.
  - Node.js är JavaScript-körning med ett omfattande paketet ekosystem.
- Hur du använder [NPM](https://www.npmjs.com/) installera Node.js utvecklingsverktyg.
  - Versionen som krävs för Node.js är 4.5 LTS.
  - NPM är en av de paket cheferna för Node.js.
- Så här installerar Visual Studio-koden.
  - Visual Studio-koden är plattformsoberoende redigerare för enkel men kraftfull källa för Windows, Linux och macOS. Det har bra stöd för felsökning, inbäddad Git-kontroll, syntaxmarkering, intelligent kod slutförande, kodavsnitt och koden eftersom samt.
- Hur du installerar Python.
  - Python är en term som används på hög nivå, allmänna, tolkad och dynamiska programmeringsspråk.
- Så här installerar du Azure CLI.
  - Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure. Du arbetar direkt från en kommandorad för att etablera och hantera resurser.
- Hur du använder Azure CLI för att skapa en IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

- En Internet-anslutning att hämta verktyg och program.
- En Mac-dator som kör OS X Yosemite (10.10) eller senare.

## <a name="install-git-and-nodejs"></a>Installera Git och Node.js

Installera Git och Node.js med verktyget Homebrew paketet för hantering genom att följa dessa steg:

1. [Hämta](http://brew.sh/) och installera Homebrew. Om du redan har installerat Homebrew går du till steg 2.
   1. Tryck på `Cmd + Space` och ange `Terminal` att öppna en terminal.
   2. Kör följande kommando:

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```

2. Installera Git och Node.js genom att köra följande kommando:

    ```bash
    brew install node git
    ```

## <a name="install-nodejs-development-tools"></a>Installera Node.js utvecklingsverktyg

Du använder [gulp.js](http://gulpjs.com/) att automatisera distribution och körning av skript.

Om du vill installera gulp, kör du följande kommando i en terminal:

```bash
npm install -g gulp
```

Om du får problem med installationen finns i [felsökningsguide för](iot-hub-gateway-kit-c-sim-troubleshooting.md) efter lösningar på vanliga problem.

> [!Note]
> Noden, NPM och Gulp krävs för att köra automatiserade skript som utvecklats i Node.js.

## <a name="install-python"></a>Installera Python

Även om Mac OS X har Python 2.7, rekommenderar vi att du installerar Python via Homebrew. Se [installerar Python på Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).

Installera Python och pip genom att köra följande kommando:

```bash
brew install python
```

## <a name="install-the-azure-cli"></a>Installera Azure CLI

Så här installerar du Azure CLI:

1. Kör följande kommandon i terminalen:
   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
   Installationen kan ta 5 minuter.

2. Verifiera installationen genom att köra följande kommando:
   ```bash
   az iot -h
   ```
   Du bör se följande utdata om installationen har slutförts.

   ![Verifiera installation av Azure CLI](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_osx.png)

## <a name="install-visual-studio-code"></a>Installera Visual Studio Code

Du kan använda Visual Studio Code senare under kursen för att redigera konfigurationsfiler.

[Hämta](https://code.visualstudio.com/docs/setup/osx) och installera Visual Studio-koden.

## <a name="summary"></a>Sammanfattning

Du har installerat alla verktyg som krävs och programvara på en Mac-dator. Nästa uppgift är att använda Azure CLI för att skapa en IoT-hubb och registrera enheten i din IoT-hubb.

## <a name="next-steps"></a>Nästa steg
[Skapa en IoT-hubb och registrera enheten](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
