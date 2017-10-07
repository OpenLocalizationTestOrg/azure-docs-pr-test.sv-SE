---
title: "Simulerade enhet & Azure IoT Gateway - lektionen 2: Hämta verktyg (Windows) | Microsoft Docs"
description: "Installera hello verktyg och hello programvara på din värddator som kör Windows, skapar en IoT-hubb och registrera enheten i hello IoT-hubb."
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
ms.openlocfilehash: 7ca3c9f11eb232f853fc8fd921b0a49ae37d0184
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-and-later"></a>Hämta hello verktyg (Windows 7 och senare)
> [!div class="op_single_selector"]
> * [Windows 7 eller senare](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Vad du ska göra

- Installera Git, Node.js, Gulp, Python.
- Installera hello Azure-kommandoradsgränssnittet (Azure CLI). 

Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här lektionen får du lära dig:

- Hur tooinstall [Git](https://git-scm.com/) och [Node.js](https://nodejs.org/en/).
  - Git är ett system för öppen källkod distribuerade versionen. hello-exempelprogram för den här lektionen lagras på Git.
  - Node.js är JavaScript-körning med ett omfattande paketet ekosystem.
- Hur toouse [NPM](https://www.npmjs.com/) tooinstall Node.js utvecklingsverktyg.
  - hello version som krävs av Node.js är 4.5 LTS.
  - NPM är en av hello paketet chefer för Node.js.
- Hur tooinstall Visual Studio Code.
  - Visual Studio-koden är plattformsoberoende redigerare för enkel men kraftfull källa för Windows, Linux och macOS. Det har bra stöd för felsökning, inbäddad Git-kontroll, syntaxmarkering, intelligent kod slutförande, kodavsnitt och koden eftersom samt.
- Hur tooinstall Python.
  - Python är en term som används på hög nivå, allmänna, tolkad och dynamiska programmeringsspråk.
- Hur tooinstall hello Azure CLI.
  - hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure. Du arbetar direkt från en kommandorad tooprovision och hantera resurser.
- Hur toouse hello Azure CLI toocreate en IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

- En Internet-anslutning toodownload hello verktyg och program.
- En Windows-dator.

## <a name="install-git-and-nodejs"></a>Installera Git och Node.js

Klicka på följande länkar toodownload hello och installera Git och Node.js LTS för Windows.

- [Hämta Git för Windows](https://git-scm.com/download/win/)
- [Hämta Node.js LTS för Windows](https://nodejs.org/en/)

## <a name="install-nodejs-development-tools"></a>Installera Node.js utvecklingsverktyg

Du använder [gulp.js](http://gulpjs.com/) tooautomate distribution och körning av skript.

Tryck på `Windows + R`, typen `cmd` och tryck på `Enter` tooopen ett kommandotolksfönster och sedan kör hello följande kommando:

```cmd
npm install -g gulp
```

Om du får problem med hello installation finns hello [felsökningsguide för](iot-hub-gateway-kit-c-sim-troubleshooting.md) för lösningar toocommon problem.

> [!Note]
> Noden, NPM och Gulp är obligatoriska toorun automatiseringsskript utvecklats i Node.js.

## <a name="install-python"></a>Installera Python

Du kan välja mellan Python 2.7, 3.4 eller 3.5. I den här självstudiekursen kommer använder vi Python 2.7. Om du redan har installerat python går toohello nästa avsnitt.

[Hämta Python för Windows](https://www.python.org/downloads/)

Du måste också tooadd hello sökvägen hello mappar där Python.exe och pip.exe är installerade toohello system `PATH` miljövariabeln. Som standard installeras python.exe i `C:\Python27` och pip.exe installeras i `C:\Python27\Scripts`.

## <a name="install-hello-azure-cli"></a>Installera hello Azure CLI

tooinstall hello Azure CLI, Följ dessa steg:

1. Öppna ett kommandotolksfönster som administratör.

2. Installera hello Azure CLI genom att köra följande kommandon hello:

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```

   hello-installationen kan ta 5 minuter.

3. Kontrollera hello installationen genom att köra följande kommando hello:

   ```cmd
   az iot -h
   ```

   Du bör se hello följande utdata om hello-installationen har slutförts.

   ![Verifiera installation av Azure CLI](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_win.png)

## <a name="install-visual-studio-code"></a>Installera Visual Studio Code

Du kan använda Visual Studio Code senare i hello självstudiekursen tooedit konfigurationsfiler.

[Hämta](https://code.visualstudio.com/docs/setup/windows) och installera Visual Studio-koden.

## <a name="summary"></a>Sammanfattning

Du har installerat alla hello krävs verktyg och program på värddatorn. Nästa uppgift är toouse hello Azure CLI toocreate en IoT-hubb och registrera enheten i din IoT-hubb.

## <a name="next-steps"></a>Nästa steg
[Skapa en IoT-hubb och registrera din enhet](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
