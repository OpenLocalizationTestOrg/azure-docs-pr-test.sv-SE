---
title: "Simulerade enhet & Azure IoT-Gateway - felsökning | Microsoft Docs"
description: "Felsökning för Intel NUC gateway"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT-problem, internet saker problem
ROBOTS: NOINDEX
ms.assetid: 3ee8f4b0-5799-40a3-8cf0-8d5aa44dbc2b
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: cc7add6273e66aadeca9a4915a44f5edf61a0a59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a>Felsökning

## <a name="hardware-issues"></a>Problem med maskinvara

### <a name="ti-sensortag-cannot-be-connected"></a>Det går inte att ansluta TI SensorTag

tootroubleshoot SensorTag-anslutningsproblem, använda hello [SensorTag app](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).

### <a name="have-an-issue-with-intel-nuc"></a>Har ett problem med Intel NUC

tootroubleshoot startproblem finns för[Felsökning nr Start på Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).

problem med tootroubleshoot operativsystem, se för[felsökning av problem med operativsystemet på Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).

tootroubleshoot andra frågor refererar för[blinkar och signal koder för Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).

## <a name="nodejs-package-issues"></a>Problem med node.js-paket

### <a name="no-response-during-gulp-tasks"></a>Inget svar under gulp uppgifter

Om du får problem körs gulp uppgifter du kan lägga till hello `--verbose` alternativet för felsökning. Försök tooterminate aktuella gulp uppgifter med hjälp av `Ctrl + C`, och sedan kör hello följande kommando i konsolen för fönstret toosee felsökningsmeddelanden. Du kan visa detaljerade felmeddelanden i konsolens utdata.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Problem vid identifiering av enheter

Felsökning av vanliga problem med hello `discover-sensortag` kommandot, kontrollera hello [wiki-sida](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).

### <a name="npm-issues"></a>npm-problem

Försök tooupdate npm-paketet genom att köra följande kommando hello:

```bash
npm install -g npm
```

Om hello problemet fortfarande kvarstår lämna kommentarer hello slutet av den här artikeln eller skapa ett GitHub-problem i våra [exempel databasen](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).

## <a name="remote-debugging"></a>Fjärrfelsökning
> Nedan instruktioner är avsedda för att felsöka node.js-skript som används i den här kursen.
### <a name="run-hello-sample-application-in-debug-mode"></a>Köra hello exempelprogrammet i felsökningsläge

Kör exempelprogrammet hello i felsökningsläge genom att köra följande kommando hello:

```bash
gulp run --debug
```

När hello debug-motorn är klar, bör du se `Debugger listening on port 5858` i hello konsolens utdata.

### <a name="configure-visual-studio-code-tooconnect-toohello-remote-device"></a>Konfigurera Visual Studio Code tooconnect toohello fjärranslutna enheter

1. Öppna hello **felsöka** panelen för hello vänster sida.
2. Klicka på hello grön **Start Debugging** (F5)-knappen. Visual Studio Code öppnar en `launch.json` fil.
3. Uppdatera hello `launch.json` fil med hello följande innehåll. Ersätt `[device hostname or IP address]` med hello faktiska IP-adressen eller värdnamnet enhetsnamn.

   ``` json
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
            "remoteRoot": "~/ble_sample"
        }
     ]
   }
   ```

![Fjärrkonfiguration felsökning](./media/iot-hub-gateway-kit-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-toohello-remote-application"></a>Koppla toohello fjärrprogram

Klicka på hello grön **Start Debugging** (F5) knappen toostart felsökning.

Läs [JavaScript i VS kod](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn mer om hello felsökare.

![Felsökning Bell-exempel](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a>Azure CLI-problem

hello Azure-kommandoradsgränssnittet (Azure CLI) är en förhandsversionen. tooseek lösningar, kan du använda hello [Preview installera guiden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).

Om det uppstår några buggar med hello verktyget filen ett [problemet](https://github.com/Azure/azure-cli/issues) i hello **problem** avsnitt i GitHub-repo-hello.

Hjälp vid felsökning av vanliga problem, kontrollera hello [viktigt](https://github.com/Azure/azure-cli/blob/master/README.rst).

Om du uppfyller ”det gick inte att hitta en version som uppfyller krav på hello”, ange kommando hello kör följande tooupgrade pip toolastest version.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Problem med installation av Python

### <a name="legacy-installation-issues-macos"></a>Äldre installationsproblem (macOS)

När du installerar pip, en behörighet felmeddelande när äldre paket installeras med **su** behörigheter. Detta inträffar eftersom en tidigare installation av Python med brew (macOS) inte har avinstallerats helt. Vissa pip-paket från en tidigare installation skapades rot, vilket gör att hello behörighetsfel. Hej lösningen är tooremove de paket som installerats av roten. Använd följande steg toocomplete hello den här aktiviteten:

1. Gå för`/usr/local/lib/python2.7/site-packages`
2. Lista över paket som skapats av roten:`ls -l | grep root`
3. Avinstallera paket från steg 2:`sudo rm -rf {package name}`
4. Installera om Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT-hubb-problem

Om din Azure IoT-hubb med hello Azure CLI har etablerats, och du behöver en verktyget toomanage hello enheter som ansluter tooyour IoT-hubb, kan du försöka hello följande verktyg.

### <a name="device-explorer"></a>Enheten Explorer

[Enheten Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) körs på den lokala Windows-datorn och ansluter tooyour IoT-hubb i Azure. Den kommunicerar med hello följande [IoT-hubbslutpunkter](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):

- Enheten identity management tooprovision och hantera enheter som registrerats med IoT-hubben.
- Ta emot enhet till moln så att du kan övervaka meddelanden som skickas från din enhet tooyour IoT-hubb.
- Skicka moln till enhet så att du kan skicka meddelanden tooyour enheter från din IoT-hubb.

Konfigurera anslutningssträngen IoT-hubb i det här verktyget toouse dess funktioner.

### <a name="iothub-explorer"></a>iothub explorer

[iothub explorer](https://github.com/Azure/iothub-explorer) är ett exempel flera plattformar CLI toomanage enhetsklienter. Du kan använda hello verktyget toomanage hello enheter i hello identitetsregistret, övervaka meddelanden från enhet till moln och skicka kommandon moln till enhet.

tooinstall hello senaste () förhandsversion av hello iothub explorer verktyget kör hello följande kommando:

```bash
npm install -g iothub-explorer@latest
```

tooget mer hjälp om alla Hej iothub explorer kommandon och deras parametrar, kör hello följande kommando:

```bash
iothub-explorer help
```

### <a name="hello-azure-portal"></a>hello Azure-portalen

En fullständig CLI-upplevelse hjälper dig att skapa och hantera alla dina Azure-resurser. Du kan också toouse hello [Azure-portalen](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) toohelp etablera, hantera och felsöka Azure-resurser.

## <a name="azure-storage-issues"></a>Azure Storage-problem

[Microsoft Azure Lagringsutforskaren (förhandsversion)](http://storageexplorer.com/) är en fristående app från Microsoft som du kan använda toowork med Azure Storage-data i Windows, macOS och Linux. Med det här verktyget kan du ansluta tooyour tabell och se hello data i den. Du kan använda det här verktyget tootroubleshoot Azure Storage-problem.
