---
title: "Connect Arduino (C) tooAzure IoT - felsökning | Microsoft Docs"
description: "Sida för Adafruit ludd M0 WiFi Arduino upplevelse för felsökning"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Felsökning av arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: fdcc56ff-4420-463c-8a0e-5a1d215a874f
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ed793041ffa1887dbe73067f7c48d2417e982866
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a>Felsökning
## <a name="hardware-issues"></a>Problem med maskinvara
Information om hur du löser vanliga problem på Adafruit ludd M0 WiFi Arduino-kort finns hello [officiella felsökning sidan](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500?view=all#faq).

## <a name="nodejs-package-issues"></a>Problem med node.js-paket
### <a name="no-response-during-gulp-tasks"></a>Inget svar under gulp uppgifter
Om du stöter på problem med att köra gulp uppgifter kan du lägga till hello `--verbose` alternativet för felsökning. Försök tooterminate aktuella gulp uppgifter med hjälp av `Ctrl + C`, och sedan kör hello följande kommando i konsolen för fönstret toosee felsökningsmeddelanden. Du kan visa detaljerade felmeddelanden i konsolens utdata.

```bash
gulp --verbose
```

Du kan också lägga till `--listen` tooopen logginformation för seriell port toooutput enhet.

```bash
gulp --listen
``` 

### <a name="npm-issues"></a>NPM-problem
Försök tooupdate NPM-paketet med hello följande kommando:

```bash
npm install -g npm
```

Om hello problemet fortfarande kvarstår lämna kommentarer hello slutet av den här artikeln eller skapa ett GitHub-problem i våra [exempel databasen][sample-repository].

## <a name="azure-cli-issues"></a>Problem med Azure CLI
hello Azure-kommandoradsgränssnittet (Azure CLI) är en förhandsversionen. Leta efter en lösning i hello [Preview installera guiden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek lösningar. Försök tooupgrade Azure cli toolatest version när kommandon inte fungerar som förväntat.

Om det uppstår några buggar med hello verktyget filen ett [problemet](https://github.com/Azure/azure-cli/issues) i hello **problem** avsnitt i GitHub-repo-hello.

För hjälp med att felsöka vanliga problem, kontrollera hello [viktigt](https://github.com/Azure/azure-cli/blob/master/README.rst).

Om du uppfyller ”det gick inte att hitta en version som uppfyller krav på hello”, ange kommando hello kör följande tooupgrade pip toolastest version.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Problem med installation av Python
### <a name="legacy-installation-issues-macos"></a>Äldre installationsproblem (macOS)
När du installerar **pip**, en behörighet felmeddelande när äldre paket som är installerade med **su** behörigheter. Detta inträffar eftersom en tidigare installation av Python med brew (macOS) inte har avinstallerats helt. Vissa **pip** paket från en tidigare installation har skapats av roten, vilket gör att hello behörighetsfel. Hej lösningen är tooremove de paket som installerats av roten. Använd följande steg toocomplete hello den här aktiviteten:

1. Gå till: /usr/local/lib/python2.7/site-packages
2. Lista paket skapa av roten:`ls -l | grep root`
3. Avinstallera paket från steg 2:`sudo rm -rf {package name}`
4. Installera om Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT-hubb-problem
Om du har etablerats dina Azure IoT-hubb med `azure-cli`, och du behöver en verktyget toomanage hello enheter som ansluter tooyour IoT hub, försök hello följande verktyg:

### <a name="device-explorer"></a>Enheten Explorer
[Enheten Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) körs på den lokala Windows-datorn och ansluter tooyour IoT-hubb i Azure. Den kommunicerar med hello följande [IoT-hubbslutpunkter](iot-hub-devguide.md):

* *Enheten Identitetshantering* tooprovision och hantera enheter som registrerats med IoT-hubben.
* *Ta emot enhet till moln* så att du kan övervaka meddelanden som skickas från din enhet tooyour IoT-hubb.
* *Skicka moln till enhet* så att du kan skicka meddelanden tooyour enheter från din IoT-hubb.

Konfigurera din `IoT hub connection string` i det här verktyget toouse dess funktioner.

### <a name="iot-hub-explorer"></a>IoT-hubb Explorer
[IoT-hubb Explorer](https://github.com/Azure/iothub-explorer) är ett exempel flera plattformar CLI toomanage enhetsklienter. Du kan använda hello verktyget toomanage hello enheter i hello identitetsregistret, övervaka meddelanden från enhet till moln och skicka kommandon moln till enhet.


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

## <a name="azure-storage-issues"></a>Problem med Azure-lagring
[Microsoft Azure Lagringsutforskaren (förhandsversion)](http://storageexplorer.com) är en fristående app från Microsoft som du kan använda toowork med Azure Storage-data i Windows, macOS och Linux. Med det här verktyget kan du ansluta tooyour tabell och se hello data i den. Du kan använda det här verktyget tootroubleshoot Azure Storage-problem.

<!-- Images and links -->

[sample-repository]: https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md