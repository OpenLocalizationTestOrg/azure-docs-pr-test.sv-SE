---
title: "Connect Intel EDISON (C) till Azure IoT - lektionen 1: Hämta verktyg (macOS) | Microsoft Docs"
description: "Hämta och installera verktyg och programvaran för det första exempelprogrammet för modern på macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino utvecklingsverktyg, iot utveckling, iot-programvara, internet saker programvara, installera git på mac installera node js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 3f764f2e-25fa-4dde-9e8d-d278453fccfd
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 27939f731121522f688e606052492bda8ae045fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos-1010"></a>Hämta verktygen (macOS 10.10)
> [!div class="op_single_selector"]
> * [Windows 7 eller senare][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Vad du ska göra
Hämta utvecklingsverktyg och programvaran för det första exempelprogrammet för Intel-modern. Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].

> [!NOTE]
> Även om programmeringsspråket huvudsakliga logiken C, används Node.js verktyg i erfarenheter att skapa och distribuera exempelprogram.

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:

* Hur du installerar Git och Node.js.
  * [Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen. Exempelprogram för den här artikeln är lagrade på Git.
  * [Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.
* Hur du använder NPM för att installera ytterligare Node.js-utvecklingsverktyg.
  * Versionen som krävs för Node.js är 4.5 LTS.
  * [NPM](https://www.npmjs.com) är en av de paket cheferna för Node.js.

## <a name="what-you-need"></a>Vad du behöver
För att slutföra den här åtgärden behöver du:
* En Internet-anslutning att hämta utvecklingsverktyg och programvaran.
* En Mac som kör macOS Yosemite (10.10) eller senare.

## <a name="install-git-and-nodejs"></a>Installera Git och Node.js
Installera Git och Node.js med den [Homebrew](http://brew.sh) paketet hanteringsverktyg genom att följa dessa steg:

1. Installera Homebrew. Om du redan har installerat Homebrew går du till steg 2.

   1. Tryck på `Cmd + Space` och ange `Terminal` att öppna en terminal.
   2. Kör följande kommando:

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. Installera Git och Node.js genom att köra följande kommando:

   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a>Installera ytterligare utvecklingsverktyg för Node.js
Använd [gulp.js](http://gulpjs.com) att automatisera distributionen av exempelprogrammet till din modern.

Installera `gulp` genom att köra följande kommando i terminalen:

```bash
sudo npm install -g gulp
```

Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på macOS finns i [felsökningsguide för] [ troubleshooting] efter lösningar på vanliga problem.

## <a name="install-visual-studio-code"></a>Installera Visual Studio Code
[Hämta](https://code.visualstudio.com/docs/setup/osx) och installera Visual Studio-koden. Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS. Du kan använda den här redigeraren senare under kursen för att redigera exempelkoden.

## <a name="summary"></a>Sammanfattning
Du har installerat nödvändiga utvecklingsverktyg och programvara för första exempelprogrammet. Nästa uppgift är att skapa, distribuera och köra exempelprogrammet på modern.

## <a name="next-steps"></a>Nästa steg
[Skapa och distribuera programmet blinka][create-and-deploy-the-blink-application]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md
