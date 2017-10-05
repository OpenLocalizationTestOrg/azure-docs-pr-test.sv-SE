---
title: "Simulerade enhet & Azure IoT Gateway - lektionen 2: Hämta verktyg (Windows) | Microsoft Docs"
description: "Installera verktygen och programvaran på din värddator som kör Windows, skapar en IoT-hubb och registrera enheten i IoT-hubben."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-utveckling, iot-programvara, iot-Molntjänsten, internet av saker programvara, azure cli, installera git för windows, gulp kör, node js windows, installera npm i windows, installera python i windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: c16eee4c-8756-454b-82bf-3eb0dd51aa5f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ecedf5ef89677c73c5d9a3e5250eb9f45f6bad32
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-and-later"></a>Skaffa dig verktyg (Windows 7 och senare)
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
- En Windows-dator.

## <a name="install-git-and-nodejs"></a>Installera Git och Node.js

Klicka på länken nedan om du vill hämta och installera Git och Node.js LTS för Windows.

- [Hämta Git för Windows](https://git-scm.com/download/win/)
- [Hämta Node.js LTS för Windows](https://nodejs.org/en/)

## <a name="install-nodejs-development-tools"></a>Installera Node.js utvecklingsverktyg

Du använder [gulp.js](http://gulpjs.com/) att automatisera distribution och körning av skript.

Tryck på `Windows + R`, typen `cmd` och tryck på `Enter` att öppna en kommandotolk och kör sedan följande kommando:

```cmd
npm install -g gulp
```

Om du får problem med installationen finns i [felsökningsguide för](iot-hub-gateway-kit-c-sim-troubleshooting.md) efter lösningar på vanliga problem.

> [!Note]
> Noden, NPM och Gulp krävs för att köra automatiserade skript som utvecklats i Node.js.

## <a name="install-python"></a>Installera Python

Du kan välja mellan Python 2.7, 3.4 eller 3.5. I den här självstudiekursen kommer använder vi Python 2.7. Om du redan har installerat python, gå till nästa avsnitt.

[Hämta Python för Windows](https://www.python.org/downloads/)

Du måste också lägga till sökvägen till de mappar där Python.exe och pip.exe är installerade i systemet `PATH` miljövariabeln. Som standard installeras python.exe i `C:\Python27` och pip.exe installeras i `C:\Python27\Scripts`.

## <a name="install-the-azure-cli"></a>Installera Azure CLI

Så här installerar du Azure CLI:

1. Öppna ett kommandotolksfönster som administratör.

2. Installera Azure CLI genom att köra följande kommandon:

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```

   Installationen kan ta 5 minuter.

3. Verifiera installationen genom att köra följande kommando:

   ```cmd
   az iot -h
   ```

   Du bör se följande utdata om installationen har slutförts.

   ![Verifiera installation av Azure CLI](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_win.png)

## <a name="install-visual-studio-code"></a>Installera Visual Studio Code

Du kan använda Visual Studio Code senare under kursen för att redigera konfigurationsfiler.

[Hämta](https://code.visualstudio.com/docs/setup/windows) och installera Visual Studio-koden.

## <a name="summary"></a>Sammanfattning

Du har installerat alla verktyg som krävs och programvara på värddatorn. Nästa uppgift är att använda Azure CLI för att skapa en IoT-hubb och registrera enheten i din IoT-hubb.

## <a name="next-steps"></a>Nästa steg
[Skapa en IoT-hubb och registrera din enhet](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
