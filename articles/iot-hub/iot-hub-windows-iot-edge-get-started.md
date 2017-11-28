---
title: "aaaGet igång med Azure IoT kant (Windows) | Microsoft Docs"
description: "Hur toobuild ett Azure IoT gräns-gatewayen på en Windows datorn och lär dig mer om nyckelbegrepp i Azure IoT kant som JSON-konfigurationsfiler och moduler."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 9aff3724-5e4e-40ec-b95a-d00df4f590c5
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5dd13cbfc02eeb55d9f2dbffca5021f2624acf14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="explore-azure-iot-edge-architecture-on-windows"></a><span data-ttu-id="6ee8c-103">Utforska Azure IoT kant arkitektur i Windows</span><span class="sxs-lookup"><span data-stu-id="6ee8c-103">Explore Azure IoT Edge architecture on Windows</span></span>

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-toorun-hello-sample"></a><span data-ttu-id="6ee8c-104">Hur toorun hello exempel</span><span class="sxs-lookup"><span data-stu-id="6ee8c-104">How toorun hello sample</span></span>

<span data-ttu-id="6ee8c-105">Hej **build.cmd** skriptet genererar utdata i hello **skapa** mapp i den lokala kopian av hello **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="6ee8c-105">hello **build.cmd** script generates its output in hello **build** folder in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="6ee8c-106">Här ingår hello två IoT kant moduler som används i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="6ee8c-106">This output includes hello two IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="6ee8c-107">Hej build skript platser **logger.dll** i hello **skapa\\moduler\\loggaren\\felsöka** mapp och **hello\_world.dll**  i hello **skapa\\moduler\\hello_world\\felsöka** mapp.</span><span class="sxs-lookup"><span data-stu-id="6ee8c-107">hello build script places **logger.dll** in hello **build\\modules\\logger\\Debug** folder and **hello\_world.dll** in hello **build\\modules\\hello_world\\Debug** folder.</span></span> <span data-ttu-id="6ee8c-108">Använd dessa sökvägar för hello **modulen sökvägen** värden som visas i följande JSON-inställningsfilen hello.</span><span class="sxs-lookup"><span data-stu-id="6ee8c-108">Use these paths for hello **module path** values as shown in hello following JSON settings file.</span></span>

<span data-ttu-id="6ee8c-109">hello hello\_world\_exempel processen tar hello sökvägen tooa JSON-konfigurationsfil som kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="6ee8c-109">hello hello\_world\_sample process takes hello path tooa JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="6ee8c-110">hello följande exempel JSON-fil har angetts i hello SDK lagringsplats på **exempel\\hello\_world\\src\\hello\_world\_win.json**.</span><span class="sxs-lookup"><span data-stu-id="6ee8c-110">hello following example JSON file is provided in hello SDK repository at **samples\\hello\_world\\src\\hello\_world\_win.json**.</span></span> <span data-ttu-id="6ee8c-111">Den här konfigurationen filen fungerar som om du inte ändrar hello skapa skript tooplace hello IoT kant moduler eller exempel körbara filer i icke-standardplatser.</span><span class="sxs-lookup"><span data-stu-id="6ee8c-111">This configuration file works as is unless you modify hello build script tooplace hello IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="6ee8c-112">hello modulen sökvägar är relativ toohello directory där hello hello\_world\_sample.exe finns.</span><span class="sxs-lookup"><span data-stu-id="6ee8c-112">hello module paths are relative toohello directory where hello hello\_world\_sample.exe is located.</span></span> <span data-ttu-id="6ee8c-113">hello exempel JSON configuration file standardvärden toowriting 'log.txt' i den aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="6ee8c-113">hello sample JSON configuration file defaults toowriting 'log.txt' in your current working directory.</span></span>

```json
{
  "modules": [
    {
      "name": "logger",
      "loader": {
        "name": "native",
        "entrypoint": {
          "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
        }
      },
      "args": { "filename": "log.txt" }
    },
    {
      "name": "hello_world",
      "loader": {
        "name": "native",
        "entrypoint": {
          "module.path": "..\\..\\..\\modules\\hello_world\\Debug\\hello_world.dll"
        }
      },
      "args": null
      }
  ],
  "links": [
    {
      "source": "hello_world",
      "sink": "logger"
    }
  ]
}
```

1. <span data-ttu-id="6ee8c-114">Navigera toohello **skapa** mapp i hello roten för den lokala kopian av hello **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="6ee8c-114">Navigate toohello **build** folder in hello root of your local copy of hello **iot-edge** repository.</span></span>

1. <span data-ttu-id="6ee8c-115">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="6ee8c-115">Run hello following command:</span></span>

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
