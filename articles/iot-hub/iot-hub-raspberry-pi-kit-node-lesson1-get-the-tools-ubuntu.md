---
title: "Ansluta hallon Pi (nod) tooAzure IoT - lektionen 1: Hämta verktyg (Ubuntu) | Microsoft Docs"
description: "Hämta och installera nödvändiga hello-verktyg och programvara för hello första exempelprogram för Pi på Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT-utveckling iot programvara, internet saker programvara, git på ubuntu, gulp kör, installera node js ubuntu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 4d5e45c0-1db9-4662-a039-99ba26333085
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b4f566fa0d1faf8b2321707145f675e3d87f0bef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a>Hämta hello verktyg (Ubuntu 16.04)

> [!div class="op_single_selector"]
> * [Windows 7 eller senare](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)


## <a name="what-you-will-do"></a>Vad du ska göra
Hämta hello utvecklingsverktyg och hello programvara hello första exempelprogram för hallon Pi 3. Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:

* Hur tooinstall Git och Node.js.
  * [Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen. hello-exempelprogram för den här artikeln är lagrade på Git.
  * [Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.
* Hur toouse NPM tooinstall-utvecklingsverktyg med ytterligare Node.js.
  * hello version som krävs av Node.js är 4.5 LTS.
  * [NPM](https://www.npmjs.com) är en av hello paketet chefer för Node.js.

## <a name="what-do-you-need"></a>Vad behöver du
toocomplete den här åtgärden behöver du:

* En Internet-anslutning toodownload hello utvecklingsverktyg och hello programvara.
* En dator som kör Ubuntu 16.04 eller senare.

## <a name="install-git-nodejs-and-npm"></a>Installera Git, Node.js och NPM
Använd hello kortkommandot `Ctrl + Alt + T` tooopen en hello för terminal och kör följande kommandon:

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a>Installera ytterligare utvecklingsverktyg för Node.js
Du använder [gulp.js](http://gulpjs.com) tooautomate hello distribution av hello exempel programmet tooPi. Du också använda hello [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information om din IoT-enheter.

Installera `gulp` och `device-discovery-cli` genom att köra följande kommando i hello terminal hello:

```bash
sudo npm install -g device-discovery-cli gulp
```

Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på Ubuntu finns hello [felsökningsguide för](iot-hub-raspberry-pi-kit-node-troubleshooting.md) för lösningar toocommon problem.

## <a name="install-visual-studio-code"></a>Installera Visual Studio Code
[Hämta](https://code.visualstudio.com/docs/setup/linux) och installera Visual Studio-koden. Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS. Du kan använda den här redigeraren för senare i hello självstudiekursen tooedit hello exempelkod.

## <a name="summary"></a>Sammanfattning
Du har installerat hello krävs utvecklingsverktyg och programvara för hello första exempelprogrammet. hello nästa uppgift är toocreate, distribuera och köra hello exempelprogrammet på Pi.

## <a name="next-steps"></a>Nästa steg
[Skapa och distribuera hello blinka exempelprogrammet](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

