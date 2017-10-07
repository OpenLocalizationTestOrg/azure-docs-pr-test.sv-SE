---
title: "aaaGet igång med Azure IoT kant (Linux) | Microsoft Docs"
description: "Hur toobuild en gateway på en Linux datorn och lär dig mer om nyckelbegrepp i Azure IoT kant som JSON-konfigurationsfiler och moduler."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40aa9c8ddca6a974c361cbb0b453c7d0ddc71b8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a><span data-ttu-id="67b2f-103">Utforska Azure IoT Edge-arkitektur på Linux</span><span class="sxs-lookup"><span data-stu-id="67b2f-103">Explore Azure IoT Edge architecture on Linux</span></span>

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-toorun-hello-sample"></a><span data-ttu-id="67b2f-104">Hur toorun hello exempel</span><span class="sxs-lookup"><span data-stu-id="67b2f-104">How toorun hello sample</span></span>

<span data-ttu-id="67b2f-105">Hej **build.sh** skriptet genererar utdata i hello **skapa** mapp i den lokala kopian av hello **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="67b2f-105">hello **build.sh** script generates its output in hello **build** folder in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="67b2f-106">Här ingår hello två IoT kant moduler som används i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="67b2f-106">This output includes hello two IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="67b2f-107">Hej build skript platser **liblogger.so** i hello **build-moduler-logg/** mapp och **libhello\_world.so** i hello **skapa / moduler/hello_world/** mapp.</span><span class="sxs-lookup"><span data-stu-id="67b2f-107">hello build script places **liblogger.so** in hello **build/modules/logger/** folder and **libhello\_world.so** in hello **build/modules/hello_world/** folder.</span></span> <span data-ttu-id="67b2f-108">Använd dessa sökvägar för hello **modulen sökvägen** värden som visas i hello följande exempelfil JSON-inställningar.</span><span class="sxs-lookup"><span data-stu-id="67b2f-108">Use these paths for hello **module path** values as shown in hello following example JSON settings file.</span></span>

<span data-ttu-id="67b2f-109">hello hello\_world\_exempel processen tar hello sökvägen tooa JSON-konfigurationsfil ett kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="67b2f-109">hello hello\_world\_sample process takes hello path tooa JSON configuration file a command-line argument.</span></span> <span data-ttu-id="67b2f-110">hello följande exempel JSON-fil har angetts i hello SDK lagringsplats på **exempel/hello\_world/src/hello\_world\_lin.json**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-110">hello following example JSON file is provided in hello SDK repository at **samples/hello\_world/src/hello\_world\_lin.json**.</span></span> <span data-ttu-id="67b2f-111">Den här konfigurationen filen fungerar som om du inte ändrar hello skapa skript tooplace hello IoT kant moduler eller exempel körbara filer i icke-standardplatser.</span><span class="sxs-lookup"><span data-stu-id="67b2f-111">This configuration file works as is unless you modify hello build script tooplace hello IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="67b2f-112">hello modulen sökvägar är relativ toohello aktuella arbetskatalogen varifrån hello hello\_world\_exempel körbar fil startas, hello katalogen där hello körbara finns inte.</span><span class="sxs-lookup"><span data-stu-id="67b2f-112">hello module paths are relative toohello current working directory from where hello hello\_world\_sample executable is launched, not hello directory where hello executable is located.</span></span> <span data-ttu-id="67b2f-113">hello exempel JSON configuration file standardvärden toowriting 'log.txt' i den aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="67b2f-113">hello sample JSON configuration file defaults toowriting 'log.txt' in your current working directory.</span></span>

```json
{
    "modules" :
    [
        {
            "name" : "logger",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args" : {"filename":"log.txt"}
        },
        {
            "name" : "hello_world",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/hello_world/libhello_world.so"
            }
            },
            "args" : null
        }
    ],
    "links":
    [
        {
            "source": "hello_world",
            "sink": "logger"
        }
    ]
}
```

1. <span data-ttu-id="67b2f-114">Navigera toohello **skapa** mapp i hello roten för den lokala kopian av hello **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="67b2f-114">Navigate toohello **build** folder in hello root of your local copy of hello **iot-edge** repository.</span></span>

1. <span data-ttu-id="67b2f-115">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="67b2f-115">Run hello following command:</span></span>

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json`
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

