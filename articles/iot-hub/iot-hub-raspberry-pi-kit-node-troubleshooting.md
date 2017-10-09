---
title: "Connect Raspberry PI (C) tooAzure IoT - felsökning | Microsoft Docs"
description: "Felsökning för hello hallon Pi Node.js-upplevelse"
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
ms.openlocfilehash: 8f0807104550e8e53a132f7741564b33f1db17ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a>Felsökning
## <a name="hardware-issues"></a>Problem med maskinvara
### <a name="hello-application-runs-well-but-hello-led-is-not-blinking"></a>hello-program körs och men hello Indikator blinkar inte
Det här problemet är alltid relaterade toohardware krets anslutningen. Använd följande steg tooidentify problem hello:

1. Kontrollera att du har valt rätt hello **GPIO** på din-kort. hello två portar bör vara **GPIO GND (PIN-kod 6)** och **GPIO 04 (PIN-kod 7)**.
2. Kontrollera att hello polaritet för din Indikator är korrekt. hello längre ben bör ange hello **positiva**, av PIN-kod.
3. Använd hello **3, 3v fästa** och **jord Pin** på hallon Pi 3. Behandla Pi som hello DC-ström. Kontrollera att hello Indikator fungerar bra.

![Specifikation för Indikator](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Andra problem med maskinvara
Information om hur du löser vanliga problem på hallon Pi 3 finns hello [officiella felsökning sidan](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Problem med node.js-paket
### <a name="no-response-during-gulp-tasks"></a>Inget svar under gulp uppgifter
Om du får problem körs gulp uppgifter du kan lägga till hello `--verbose` alternativet för felsökning. Försök tooterminate aktuella gulp uppgifter med hjälp av Ctrl + C och kör sedan följande kommando i konsolen för fönstret toosee felsökningsmeddelanden hello. Du kan visa detaljerade felmeddelanden i konsolens utdata.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Problem vid identifiering av enheter
Felsökning av vanliga problem med hello `devdisco` kommandot, kontrollera hello [viktigt](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="npm-issues"></a>npm-problem
Försök tooupdate npm-paketet med hjälp av hello följande kommando:

```bash
npm install -g npm
```

Om hello problemet fortfarande kvarstår lämna kommentarer hello slutet av den här artikeln eller skapa ett GitHub-problem i våra [exempel databasen](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).

## <a name="remote-debugging"></a>Fjärrfelsökning
### <a name="run-hello-sample-application-in-debug-mode"></a>Köra hello exempelprogrammet i felsökningsläge
```bash
gulp run --debug
```

När hello debug-motorn är klar, bör du se ```Debugger listening on port 5858``` i hello konsolens utdata.

### <a name="configure-visual-studio-code-tooconnect-toohello-remote-device"></a>Konfigurera Visual Studio Code tooconnect toohello fjärranslutna enheter
1. Öppna hello **felsöka** panelen för hello vänster sida.
2. Klicka på hello grön **Start Debugging** (F5)-knappen. Visual Studio Code öppnar en launch.json-fil.
3. Uppdatera hello launch.json filen med hello följande innehåll. Ersätt `[device hostname or IP address]` med hello faktiska IP-adressen eller värdnamnet enhetsnamn.

> [!NOTE]
> toolearn mer om hello Visual Studio-felsökning, se för[felsökning i Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).


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

### <a name="attach-toohello-remote-application"></a>Koppla toohello fjärrprogram
Klicka på hello grön **Start Debugging** (F5) knappen toostart felsökning.

Läs [JavaScript i VS kod](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn mer om hello felsökare.

![Remote felsökning interaktiva](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Azure CLI-problem
hello Azure-kommandoradsgränssnittet (Azure CLI) är en förhandsversionen. tooseek lösningar, kan du använda hello [Preview installera guiden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).

Om det uppstår några buggar med hello verktyget filen ett [problemet](https://github.com/Azure/azure-cli/issues) i hello **problem** avsnitt i GitHub-repo-hello.

Hjälp vid felsökning av vanliga problem, kontrollera hello [viktigt](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Problem med installation av Python
### <a name="legacy-installation-issues-macos"></a>Äldre installationsproblem (macOS)
När du installerar pip, en behörighet felmeddelande när äldre paket installeras med **su** behörigheter. Detta inträffar eftersom en tidigare installation av Python med brew (macOS) inte har avinstallerats helt. Vissa pip-paket från en tidigare installation skapades rot, vilket gör att hello behörighetsfel. Hej lösningen är tooremove de paket som installerats av roten. Använd följande steg toocomplete hello den här aktiviteten:

1. Gå till: /usr/local/lib/python2.7/site-packages
2. Lista över paket som skapats av roten:`ls -l | grep root`
3. Avinstallera paket från steg 2:`sudo rm -rf {package name}`
4. Installera om Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT-hubb-problem
Om din Azure IoT-hubb med Azure CLI har etablerats, och du behöver en verktyget toomanage hello enheter som ansluter tooyour IoT-hubb, kan du försöka hello följande verktyg.

### <a name="device-explorer"></a>Enheten explorer
Hej [enheten explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) verktyget körs på den lokala Windows-datorn och ansluter tooyour IoT-hubb i Azure. Den kommunicerar med hello följande [IoT-hubbslutpunkter](iot-hub-devguide.md):


* *Enheten Identitetshantering* tooprovision och hantera enheter som registrerats med IoT-hubben.
* *Ta emot enhet till moln* så att du kan övervaka meddelanden som skickas från din enhet tooyour IoT-hubb.
* *Skicka moln till enhet* så att du kan skicka meddelanden tooyour enheter från din IoT-hubb.

Konfigurera anslutningssträngen IoT-hubb i det här verktyget toouse dess funktioner.

### <a name="iothub-explorer"></a>iothub explorer
[iothub explorer](https://github.com/Azure/iothub-explorer) är ett exempel flera plattformar CLI toomanage enheter. Du kan använda hello verktyget toomanage hello enheter i hello identitetsregistret, övervaka meddelanden från enhet till moln och skicka meddelanden moln till enhet.

tooinstall hello senaste () förhandsversion av hello iothub explorer verktyg, kör följande kommando i Kommandotolken miljön hello:

```bash
npm install -g iothub-explorer@latest
```

Du kan använda följande kommando tooget mer hjälp om alla hello iothub explorer kommandon och deras parametrar hello:

```bash
iothub-explorer help
```

### <a name="azure-portal"></a>Azure Portal
En fullständig CLI-upplevelse hjälper dig att skapa och hantera alla dina Azure-resurser. Du kan också toouse hello [Azure-portalen](../azure-portal-overview.md) toohelp etablera, hantera och felsöka Azure-resurser.

## <a name="azure-storage-issues"></a>Azure Storage-problem
[Microsoft Azure Lagringsutforskaren (förhandsversion)](http://storageexplorer.com) är en fristående app från Microsoft som du kan använda toowork med Azure Storage-data i Windows, macOS och Linux. Med det här verktyget kan du ansluta tooyour tabell och se hello data i den. Du kan använda det här verktyget tootroubleshoot Azure Storage-problem.

