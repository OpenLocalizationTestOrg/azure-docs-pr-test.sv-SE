---
title: "aaaTroubleshooting Docker klientfel i Windows med hjälp av Visual Studio | Microsoft Docs"
description: "Felsöka problem som kan uppstå när du använder Visual Studio toocreate och distribuera web apps tooDocker i Windows med hjälp av Visual Studio."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 7421ae8e044d58fc412d748fb870da4c9b2fdb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-visual-studio-docker-development"></a>Felsökning av Visual Studio Docker-utveckling

När du arbetar med Visual Studio Tools för Docker Preview kan vissa problem uppstå på grund av hello uppbyggnad hello preview.
Följande är några vanliga problem och lösningar.  

## <a name="visual-studio-2017-rc"></a>Visual Studio 2017 RC

### <a name="linux-containers"></a>**Linux-behållare**

####  <a name="build-errors-occur-when-debugging-a-net-core-web-or-console-application"></a>Skapa fel uppstår när du felsöker ett .NET Core webb- eller konsolen program  

Detta kan vara relaterade toonot dela hello enheten där hello projektet finns med Docker för Windows.  Du kan få ett felmeddelande som hello nedan:

```
hello "PrepareForLaunch" task failed unexpectedly.
Microsoft.DotNet.Docker.CommandLineClientException: Creating network "webapplication13628050196_default" with hello default driver
Building webapplication1
Creating webapplication13628050196_webapplication1_1
ERROR: for webapplication1  Cannot create container for service webapplication1: C: drive is not shared. Please share it in Docker for Windows Settings
```
tooresolve problemet:

1. Högerklicka på **Docker för Windows** i hello meddelandefältet och välj sedan **inställningar**.  
2. Välj **delade enheter** och dela hello enhet där hello projektet finns.

### <a name="windows-containers"></a>**Windows-behållare**

hello är följande problem särskilda toodebugging .NET Framework-webb- och konsolen program i Windows-behållare.

#### <a name="prerequisites"></a>Krav

1. Visual Studio 2017 RC (eller senare) med hello .NET Core och Docker Preview arbetsbelastning måste vara installerad.
2. Uppdatering av Windows 10 årsdagar med den senaste Windows Update korrigeringsfiler. Mer specifikt [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) måste vara installerad. 
3. [Docker för Windows](https://docs.docker.com/docker-for-windows/) (skapa 1.13.0 eller senare) måste vara installerad.
4. **Växla tooWindows behållare** måste väljas. I hello meddelandefältet klickar du på **Docker för Windows**, och välj sedan **växla tooWindows behållare**. Se till att den här inställningen sparas när hello datorn har startats om.

#### <a name="console-output-does-not-appear-in-visual-studios-output-window-while-debugging-a-console-application"></a>Konsolens utdata visas inte i Visual Studio utdata när du felsöker ett konsolprogram

Detta är ett känt problem med hello Visual Studio debugger (msvsmon.exe) som för närvarande inte för det här scenariot. Stöd för det här scenariot kan ingå i framtida versioner. toosee utdata från hello konsolprogram i Visual Studio, Använd **Docker: starta projektet**, vilket motsvarar för**starta utan Debugging**.

#### <a name="debugging-web-applications-with-hello-release-configuration-fails-with-403-forbidden-error"></a>Felsökning webbprogram med hello släpper konfigurationen misslyckas med fel (403) förbjuden

toowork runt det här problemet öppnar web.release.config i hello lösning och kommentera ut eller ta bort hello följande rader:

```
<compilation xdt:Transform="RemoveAttributes(debug)" />
```

## <a name="visual-studio-2015"></a>Visual Studio 2015

### <a name="linux-containers"></a>**Linux-behållare**

#### <a name="unable-toovalidate-volume-mapping"></a>Det går inte toovalidate volym mappning
Volymen mappningen är obligatoriska tooshare hello källkoden och binärfilerna för ditt program med hello app mappen i hello behållaren.  Specifika volymen mappningar ingår i docker-compose.dev.debug.yml och docker-compose.dev.release.yml. När filer ändras på värddatorn, de här ändringarna i en liknande mappstruktur hello behållarna.

tooenable volym mappning:

1. Klicka på **Moby** i hello meddelandefältet och välj **inställningar**.
2. Välj **delade enheter**.
3. Välj hello-enhet som är värd för ditt projekt och hello enhet där % USERPROFILE % finns.
4. Klicka på **Använd**.

tootest om volymen mappning fungerar återskapa och väljer F5 från Visual Studio när en eller flera enheter har delat eller kör hello följande kod från en kommandotolk.

> [!NOTE]
> Det här exemplet förutsätter att mappen användare finns på enhet C och att den har delats.
> Ändra vid behov om du har delat en annan enhet.

```
docker run -it -v /c/Users/Public:/wormhole busybox
```

Kör hello följande kod i hello Linux behållaren.

```
/ # ls
```

Du bör se en kataloglista från hello användare/offentlig mapp. Om inga filer visas och mappen /c/Users/Public inte är tom, är volym mappning inte korrekt konfigurerat.

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Ändra toohello destinationsindikeringen directory toosee hello innehåll hello `/c/Users/Public` directory:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

> [!NOTE]
> När du arbetar med virtuella Linux-datorer är hello behållaren filsystemet skiftlägeskänsligt.

## <a name="build-prepareforbuild-task-failed-unexpectedly"></a>Build: ”PrepareForBuild” aktiviteten misslyckades oväntat

Microsoft.DotNet.Docker.CommandLine.ClientException: Ett fel uppstod när tooconnect.

Kontrollera att hello standard Docker-värden körs. Öppna en kommandotolk och kör:

```
docker info
```

Om detta returnerar ett fel och försök sedan toostart hello **Docker för Windows** skrivbordsapp. Om hello skrivbordsapp körs sedan **Moby** ska visas i meddelandefältet hello. Högerklicka på **Moby** och öppna **inställningar**. Klicka på **återställa**, och starta sedan om Docker.

## <a name="an-error-dialog-occurs-when-attempting-tooadd-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>En feldialogruta inträffar när försök tooadd Docker Support eller felsöka ett ASP.NET Core program i en behållare för (F5)

När du avinstallerar och installerar tillägg, kan hello hanteras utökningsbarhet Framework (MEF) cache i Visual Studio skadas. När detta inträffar kan det leda till olika felmeddelanden när du lägga till stöd för Docker och/eller försök toorun eller felsöka (F5) ASP.NET Core programmet. Som en tillfällig lösning kan använda hello följande steg toodelete och generera hello MEF cache.

1. Stäng alla instanser av Visual Studio.
1. Öppna % USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.
1. Ta bort hello följande mappar:
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Öppna Visual Studio.
1. Försök hello scenariot igen.
