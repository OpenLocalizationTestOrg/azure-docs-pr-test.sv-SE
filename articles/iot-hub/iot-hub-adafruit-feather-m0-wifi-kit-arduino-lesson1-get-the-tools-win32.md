---
title: "Ansluta Arduino till Azure IoT - lektionen 1: Hämta verktyg (Windows) | Microsoft Docs"
description: "Hämta och installera verktyg och programvaran för det första exempelprogrammet för Adafruit ludd M0 WiFi för Windows 7 och senare versioner."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino utvecklingsverktyg, iot utveckling, iot-programvara, internet saker programvara, installera git för windows, installera node js windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 9cfb8cd2-eafb-4ba2-b23e-d94e114ff3a6
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5d27c016c4a74e31455e676b3c3070a8e262b21f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-or-later"></a>Skaffa dig verktyg (Windows 7 eller senare)

> [!div class="op_single_selector"]
> * [Windows 7 eller senare][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Vad du ska göra

Hämta utvecklingsverktyg och programvaran för det första exempelprogrammet för Adafruit ludd M0 WiFi Arduino-skiva.

Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].

> [!NOTE]
> Även om programmeringsspråket huvudsakliga logiken Arduino, används Node.js verktyg i erfarenheter att skapa och distribuera exempelprogram.

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:

* Hur du installerar Git och Node.js.
  * [Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen. Exempelprogram för den här artikeln är lagrade på Git.
  * [Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.
* Hur du använder NPM för att installera ytterligare Node.js-utvecklingsverktyg.
  * Kravet på minimiversion av Node.js är 4.5 LTS.
  * [NPM](https://www.npmjs.com) är en av de paket cheferna för Node.js.

## <a name="what-you-need"></a>Vad du behöver

För att slutföra den här åtgärden behöver du:

* En Internet-anslutning att hämta utvecklingsverktyg och programvaran.
* En dator som kör Windows.

## <a name="install-git-and-nodejs"></a>Installera Git och Node.js

Klicka på länkarna nedan för att hämta och installera Git och Node.js LTS för Windows.

* [Hämta Git för Windows](https://git-scm.com/download/win/)
* [Hämta Node.js LTS för Windows](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a>Installera ytterligare utvecklingsverktyg för Node.js

Använd [gulp.js](http://gulpjs.com) att automatisera distributionen av exempelprogrammet Arduino-planen.

Starta en kommandotolk som administratör. Installera `gulp`, `device-discovery-cli` genom att köra följande kommando i terminalen:

```bash
npm install -g gulp device-discovery-cli
```

Om du får problem med installationen Node.js och verktygen för utveckling av ytterligare Node.js på datorn finns på [felsökningsguide för] [ troubleshooting] efter lösningar på vanliga problem.

## <a name="install-visual-studio-code"></a>Installera Visual Studio Code

[Hämta](https://code.visualstudio.com/docs/setup/windows) och installera Visual Studio-koden. Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS. Du kan använda den här redigeraren senare under kursen för att redigera exempelkoden.

## <a name="summary"></a>Sammanfattning

Du har installerat nödvändiga utvecklingsverktyg och programvara för första exempelprogrammet. Nästa uppgift är att skapa, distribuera och kör exempelprogrammet på Arduino-kort.

## <a name="next-steps"></a>Nästa steg

[Skapa och distribuera blinka exempelprogrammet][create-and-deploy-the-blink-sample-application]
<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-mac.md
[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[create-and-deploy-the-blink-sample-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md