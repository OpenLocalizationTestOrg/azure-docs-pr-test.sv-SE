---
title: "Ansluta hallon Pi (nod) tooAzure IoT - lektionen 1: Hämta verktyg (Windows) | Microsoft Docs"
description: "Hämta och installera nödvändiga hello-verktyg och programvara för hello första exempelprogram för Pi för Windows 7 och senare versioner."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT-utveckling iot programvara, internet saker programvara, installera git för windows, gulp kör, node js windows, installera npm i windows, installera python i windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: b3d88e17-97cc-4f23-85fd-a688fc228eb8
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ea7f77cc79c70c8c7952b63462b926471fcf71cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-or-later"></a>Hämta hello verktyg (Windows 7 eller senare)

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
  * hello kravet på minimiversion av Node.js är 4.5 LTS.
  * [NPM](https://www.npmjs.com) är en av hello paketet chefer för Node.js.

## <a name="what-you-need"></a>Vad du behöver
toocomplete den här åtgärden behöver du:

* En Internet-anslutning toodownload hello utvecklingsverktyg och hello programvara.
* En dator som kör Windows.

## <a name="install-git-and-nodejs"></a>Installera Git och Node.js
Klicka på följande länkar toodownload hello och installera Git och Node.js LTS för Windows.

* [Hämta Git för Windows](https://git-scm.com/download/win/)
* [Hämta Node.js LTS för Windows](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a>Installera ytterligare utvecklingsverktyg för Node.js
Använd [gulp.js](http://gulpjs.com) tooautomate hello distribution av hello exempel programmet tooPi. Du också använda hello [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information om din IoT-enheter.

Starta en kommandotolk som administratör. Installera `gulp` och `device-discovery-cli` genom att köra följande kommando hello:

```bash
npm install -g device-discovery-cli gulp
```

Om du får problem med installationen Node.js och dessa ytterligare Node.js-utvecklingsverktyg på datorn läser hello [felsökningsguide för](iot-hub-raspberry-pi-kit-node-troubleshooting.md) för lösningar toocommon problem.

## <a name="install-visual-studio-code"></a>Installera Visual Studio Code
[Hämta](https://code.visualstudio.com/docs/setup/windows) och installera Visual Studio-koden. Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS. Du kan använda den här redigeraren för senare i hello självstudiekursen tooedit hello exempelkod.

## <a name="summary"></a>Sammanfattning
Du har installerat hello krävs utvecklingsverktyg och programvara för hello första exempelprogrammet. hello nästa uppgift är toocreate, distribuera och köra hello exempelprogrammet på Pi.

## <a name="next-steps"></a>Nästa steg
[Skapa och distribuera hello blinka exempelprogrammet](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

