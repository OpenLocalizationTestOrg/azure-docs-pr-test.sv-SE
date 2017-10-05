---
title: "Ansluta Intel modern (nod) till Azure IoT - lektionen 4: felsökning | Microsoft Docs"
description: "Felsökning för Intel modern Node.js-upplevelse"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Felsökning av arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: f11c5521-8a36-44c0-baad-f5ec26ab4618
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d54dd4a13ed87152fb6c039f38a5ffe8b4470df9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a>Felsökning
## <a name="hardware-issues"></a>Problem med maskinvara
Information om hur du löser vanliga problem på Intel modern finns i [officiella felsökning sidan](https://software.intel.com/en-us/node/637974).

## <a name="nodejs-package-issues"></a>Problem med node.js-paket
### <a name="no-response-during-gulp-tasks"></a>Inget svar under gulp uppgifter
Om du får problem med att köra gulp uppgifter, kan du lägga till den `--verbose` alternativet för felsökning. Försöker avsluta aktuella gulp uppgifter med hjälp av `Ctrl + C`, och kör sedan följande kommando i din konsolfönster Se felsökningsmeddelanden. Du kan visa detaljerade felmeddelanden i konsolens utdata. 

```bash
gulp --verbose
```

### <a name="npm-issues"></a>NPM-problem
Försök att uppdatera NPM-paket med följande kommando:

```bash
npm install -g npm
```

Om problemet kvarstår lämna kommentarer i slutet av den här artikeln eller skapa ett GitHub-problem i våra [exempel databasen][sample-repository].

## <a name="remote-debugging"></a>Fjärrfelsökning

### <a name="run-the-sample-application-in-debug-mode"></a>Kör exempelprogrammet i felsökningsläge

```bash
gulp run --debug
```

När debug-motorn är klar kan du ska kunna se ```Debugger listening on port 5858``` från konsolen utdata.

### <a name="configure-vs-code-to-connect-to-the-remote-device"></a>Konfigurera VS-kod för att ansluta till den fjärranslutna enheten

Öppna den **felsöka** panelen från den vänstra sidan.

Klicka på gröna **Start Debugging** (F5)-knappen. VS-kod öppnas en **launch.json** fil som du behöver uppdatera.

Uppdatering av **launch.json** filen med följande innehåll genom att ersätta `[device hostname or IP address]` med faktiska enhetens IP-adress eller värdnamn.  

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Fjärrkonfiguration felsökning](media/iot-hub-intel-edison-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Anslut till fjärrprogrammet

Klicka på gröna **Start Debugging** (F5) knappen och njut av felsökning.

Du kan läsa [JavaScript i VS kod](https://code.visualstudio.com/docs/languages/javascript#_debugging) för mer information om felsökning.

![Remote felsökning interaktiva](media/iot-hub-intel-edison-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Problem med Azure CLI
Azure-kommandoradsgränssnittet (Azure CLI) är en förhandsversionen. Leta efter en lösning i den [Preview installera guiden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) att söka efter lösningar. Försök att uppgradera Azure cli till senaste versionen när kommandon inte fungerar som förväntat.

Om det uppstår några fel med verktyget filen ett [problemet](https://github.com/Azure/azure-cli/issues) i den **problem** avsnitt i GitHub-lagringsplatsen.

Hjälp med att felsöka vanliga problem, kontrollera den [viktigt](https://github.com/Azure/azure-cli/blob/master/README.rst).

Kör följande kommando för att uppgradera pip till senaste versionen om du uppfyller ”det gick inte att hitta en version som uppfyller krav”.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Problem med installation av Python
### <a name="legacy-installation-issues-macos"></a>Äldre installationsproblem (macOS)
När du installerar **pip**, en behörighet felmeddelande när äldre paket som är installerade med **su** behörigheter. Detta inträffar eftersom en tidigare installation av Python med brew (macOS) inte har avinstallerats helt. Vissa **pip** paket från en tidigare installation har skapats av rot, som orsakar felet behörighet. Lösningen är att ta bort de paket som installerats av roten. Använd följande steg för att slutföra den här aktiviteten:

1. Gå till: /usr/local/lib/python2.7/site-packages
2. Lista paket skapa av roten:`ls -l | grep root`
3. Avinstallera paket från steg 2:`sudo rm -rf {package name}`
4. Installera om Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT-hubb-problem
Om du har etablerats dina Azure IoT-hubb med `azure-cli`, och du behöver ett verktyg för att hantera enheter som ansluter till din IoT-hubb kan du prova följande verktyg:

### <a name="device-explorer"></a>Enheten Explorer
[Enheten Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) körs på den lokala Windows-datorn och ansluter till din IoT-hubb i Azure. Den kommunicerar med följande [IoT-hubbslutpunkter](iot-hub-devguide.md):

- _Enheten Identitetshantering_ att etablera och hantera enheter som registrerats med IoT-hubben.
- _Ta emot enhet till moln_ så att du kan övervaka meddelanden som skickas från enheten till din IoT-hubb.
- _Skicka moln till enhet_ så att du kan skicka meddelanden till dina enheter från din IoT-hubb.

Konfigurera din `IoT hub connection string` i det här verktyget och använda dess funktioner.

### <a name="iot-hub-explorer"></a>IoT-hubb Explorer
[IoT-hubb Explorer](https://github.com/Azure/iothub-explorer) är ett exempel flera plattformar CLI verktyg för att hantera klienter. Du kan använda verktyget för att hantera enheter i identitetsregistret, övervaka meddelanden från enhet till moln och skicka kommandon moln till enhet.

För att installera den senaste (förhandsversion) versionen av verktyget iothub explorer, kör du följande kommando i Kommandotolken miljön:

```bash
npm install -g iothub-explorer@latest
```

Du kan använda följande kommando för att få ytterligare hjälp om alla iothub explorer kommandon och deras parametrar:

```bash
iothub-explorer help
```

### <a name="azure-portal"></a>Azure Portal
En fullständig CLI-upplevelse hjälper dig att skapa och hantera alla dina Azure-resurser. Du kanske också vill använda den [Azure-portalen](../azure-portal-overview.md) för att etablera, hantera och felsöka Azure-resurser.

## <a name="azure-storage-issues"></a>Problem med Azure-lagring
[Microsoft Azure Lagringsutforskaren (förhandsversion)](http://storageexplorer.com) är en fristående app från Microsoft som du kan använda för att arbeta med [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) data på Windows-, macOS- och Linux. Med det här verktyget kan du ansluta till din tabell och visa data i den. Du kan använda det här verktyget för att felsöka problem med ditt Azure Storage.

## <a name="next-steps"></a>Nästa steg
Den här sidan innehåller bara de vanligaste problemen med Intel modern kit. Du kan också lämna kommentarer längst ned och rapportera problem felsökningsinformation.

Gå tillbaka till [komma igång med Intel modern (Node.js)](iot-hub-intel-edison-kit-node-get-started.md)

<!-- Images and links -->

[sample-repository]: https://github.com/Azure-Samples/iot-hub-node-edison-getting-started