---
title: "Ansluta Raspberry Pi (C) till Azure IoT - felsökning | Microsoft Docs"
description: "Felsökning för hallon Pi Node.js-upplevelse"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT-problem, internet saker problem
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 22cf50dc-8206-42a2-a1fc-f75fa85135fa
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: f9058068972ddbb674d3cd159948835dc88c4451
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a>Felsökning
## <a name="hardware-issues"></a>Problem med maskinvara
### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Programmet ska fungera bra men Indikatorn inte blinkar
Det här problemet är alltid relaterade till maskinvara krets anslutningen. Använd följande steg för att identifiera problem:

1. Kontrollera att du har valt rätt **GPIO** på din-kort. Två portar bör vara **GPIO GND (PIN-kod 6)** och **GPIO 04 (PIN-kod 7)**.
2. Kontrollera att polaritet för din Indikator är korrekt. Längre ben bör ange den **positiva**, av PIN-kod.
3. Använd den **3, 3v fästa** och **jord Pin** på hallon Pi 3. Behandla Pi som DC-ström. Kontrollera att Indikatorn fungerar bra.

![Specifikation för Indikator](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Andra problem med maskinvara
Information om hur du löser vanliga problem på hallon Pi 3 finns i [officiella felsökning sidan](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Problem med node.js-paket
### <a name="no-response-during-gulp-tasks"></a>Inget svar under gulp uppgifter
Om du stöter på problem i gulp aktiviteter som körs, kan du lägga till den `--verbose` alternativet för felsökning. Försöker avsluta aktuella gulp uppgifter med hjälp av Ctrl + C och kör sedan följande kommando i konsolfönstret för att se felsökningsmeddelanden. Du kan visa detaljerade felmeddelanden i konsolens utdata.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Problem vid identifiering av enheter
Felsökning av vanliga problem med den `devdisco` kommando, kontrollera den [viktigt](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="npm-issues"></a>npm-problem
Försök att uppdatera npm-paketet med hjälp av följande kommando:

```bash
npm install -g npm
```

Om problemet kvarstår lämna kommentarer i slutet av den här artikeln eller skapa ett GitHub-problem i våra [exempel databasen](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).

## <a name="remote-debugging"></a>Fjärrfelsökning
### <a name="run-the-sample-application-in-debug-mode"></a>Kör exempelprogrammet i felsökningsläge
```bash
gulp run --debug
```

När debug-motorn är klar, bör du se ```Debugger listening on port 5858``` i konsolens utdata.

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a>Konfigurera Visual Studio-koden för att ansluta till den fjärranslutna enheten
1. Öppna den **felsöka** panelen till vänster.
2. Klicka på gröna **Start Debugging** (F5)-knappen. Visual Studio Code öppnar en launch.json-fil.
3. Uppdatera filen launch.json med följande innehåll. Ersätt `[device hostname or IP address]` med det faktiska IP-adress eller värdnamnet enhetsnamnet.

> [!NOTE]
> Mer information om felsökning i Visual Studio finns i [felsökning i Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).


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

![Fjärrkonfiguration felsökning](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Anslut till fjärrprogrammet
Klicka på gröna **Start Debugging** (F5) för att starta felsökning.

Läs [JavaScript i VS kod](https://code.visualstudio.com/docs/languages/javascript#_debugging) för mer information om felsökning.

![Remote felsökning interaktiva](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Azure CLI-problem
Azure-kommandoradsgränssnittet (Azure CLI) är en förhandsversionen. Om du vill söka efter lösningar du kan använda den [Preview installera guiden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).

Om det uppstår några fel med verktyget filen ett [problemet](https://github.com/Azure/azure-cli/issues) i den **problem** avsnitt i GitHub-lagringsplatsen.

Hjälp vid felsökning av vanliga problem, kontrollera den [viktigt](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Problem med installation av Python
### <a name="legacy-installation-issues-macos"></a>Äldre installationsproblem (macOS)
När du installerar pip, en behörighet felmeddelande när äldre paket installeras med **su** behörigheter. Detta inträffar eftersom en tidigare installation av Python med brew (macOS) inte har avinstallerats helt. Vissa pip-paket från en tidigare installation skapades rot, som orsakar felet behörighet. Lösningen är att ta bort de paket som installerats av roten. Använd följande steg för att slutföra den här aktiviteten:

1. Gå till: /usr/local/lib/python2.7/site-packages
2. Lista över paket som skapats av roten:`ls -l | grep root`
3. Avinstallera paket från steg 2:`sudo rm -rf {package name}`
4. Installera om Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT-hubb-problem
Om din Azure IoT-hubb med Azure CLI har etablerats och du behöver ett verktyg för att hantera enheter som ansluter till din IoT-hubb, kan du prova följande verktyg.

### <a name="device-explorer"></a>Enheten explorer
Den [enheten explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) verktyget körs på den lokala Windows-datorn och ansluter till din IoT-hubb i Azure. Den kommunicerar med följande [IoT-hubbslutpunkter](iot-hub-devguide.md):


* *Enheten Identitetshantering* att etablera och hantera enheter som registrerats med IoT-hubben.
* *Ta emot enhet till moln* så att du kan övervaka meddelanden som skickas från enheten till din IoT-hubb.
* *Skicka moln till enhet* så att du kan skicka meddelanden till dina enheter från din IoT-hubb.

Konfigurera anslutningssträngen IoT-hubb i det här verktyget och använda dess funktioner.

### <a name="iothub-explorer"></a>iothub explorer
[iothub explorer](https://github.com/Azure/iothub-explorer) är ett exempel flera plattformar CLI verktyg för att hantera enheter. Du kan använda verktyget för att hantera enheter i identitetsregistret, övervaka meddelanden från enhet till moln och skicka meddelanden moln till enhet.

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

## <a name="azure-storage-issues"></a>Azure Storage-problem
[Microsoft Azure Lagringsutforskaren (förhandsversion)](http://storageexplorer.com) är en fristående app från Microsoft som du kan använda för att arbeta med Azure Storage data på Windows och macOS Linux. Med det här verktyget kan du ansluta till din tabell och visa data i den. Du kan använda det här verktyget för att felsöka problem med ditt Azure Storage.

