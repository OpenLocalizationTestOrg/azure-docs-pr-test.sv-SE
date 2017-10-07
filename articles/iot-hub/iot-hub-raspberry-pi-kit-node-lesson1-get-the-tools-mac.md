---
title: "Ansluta hallon Pi (nod) tooAzure IoT - lektionen 1: Hämta verktyg (macOS) | Microsoft Docs"
description: "Hämta och installera nödvändiga hello-verktyg och programvara för hello första exempelprogram för Pi på macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT-utveckling iot programvara, internet saker programvara, python mac, installera git på mac, gulp kör, installera node js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 2ea6d211-c0e8-4ade-ac69-d1c2261d78c4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 382b066cb7ece7ffdeb22b162b725727b22e5fac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a>Hämta hello verktyg (macOS 10.10)
> [!div class="op_single_selector"]
> * [Windows 7 eller senare](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Vad du ska göra
Hämta hello utvecklingsverktyg och hello programvara hello första exempelprogram för din hallon Pi 3. Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:

* Hur tooinstall Git och Node.js.
  * [Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen. hello-exempelprogram för den här artikeln är lagrade på Git.
  * [Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.
* Hur toouse NPM tooinstall-utvecklingsverktyg med ytterligare Node.js.
  * hello version som krävs av Node.js är 4.5 LTS.
  * [NPM](https://www.npmjs.com) är en av hello paketet chefer för Node.js.

## <a name="what-you-need"></a>Vad du behöver
toocomplete den här åtgärden behöver du:

* En Internet-anslutning toodownload hello utvecklingsverktyg och hello programvara.
* En Mac som kör macOS Yosemite (10.10) eller senare.

## <a name="install-git-and-nodejs"></a>Installera Git och Node.js
tooinstall Git och Node.js, använda hello [Homebrew](http://brew.sh) paketet hanteringsverktyg genom att följa dessa steg:

1. Installera Homebrew. Om du redan har installerat Homebrew går toostep 2.
   
   1. Tryck på `Cmd + Space` och ange `Terminal` tooopen en terminal.
   2. Kör följande kommando hello:
      
      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. Installera Git och Node.js genom att köra följande kommando hello:
   
   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a>Installera ytterligare utvecklingsverktyg för Node.js
Använd [gulp.js](http://gulpjs.com) tooautomate hello distribution av hello exempel programmet tooPi. Använd hello [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information om din IoT-enheter.

Installera `gulp` och `device-discovery-cli` genom att köra följande kommando i hello terminal hello:

```bash
npm install -g device-discovery-cli gulp
```

Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på macOS finns hello [felsökningsguide för](iot-hub-raspberry-pi-kit-node-troubleshooting.md) för lösningar toocommon problem.

## <a name="install-visual-studio-code"></a>Installera Visual Studio Code
[Hämta](https://code.visualstudio.com/docs/setup/osx) och installera Visual Studio-koden. Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS. Du kan använda den här redigeraren för senare i hello självstudiekursen tooedit hello exempelkod.

## <a name="summary"></a>Sammanfattning
Du har installerat hello krävs utvecklingsverktyg och programvara för hello första exempelprogrammet. hello nästa uppgift är toocreate, distribuera och köra hello exempelprogrammet på Pi.

## <a name="next-steps"></a>Nästa steg
[Skapa och distribuera hello blinka exempelprogrammet](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

