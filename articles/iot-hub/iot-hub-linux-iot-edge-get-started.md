---
title: "Kom igång med Azure IoT kant (Linux) | Microsoft Docs"
description: "Så här bygger du en gateway på en Linux-dator och lär dig om viktiga begrepp i Azure IoT Edge, som moduler och JSON-konfigurationsfiler."
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
ms.openlocfilehash: b02d79fcd9cd2a2ef0041aac4e85528263c8d58a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a><span data-ttu-id="683b9-103">Utforska Azure IoT Edge-arkitektur på Linux</span><span class="sxs-lookup"><span data-stu-id="683b9-103">Explore Azure IoT Edge architecture on Linux</span></span>

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a><span data-ttu-id="683b9-104">Köra exemplet</span><span class="sxs-lookup"><span data-stu-id="683b9-104">How to run the sample</span></span>

<span data-ttu-id="683b9-105">Skriptet **build.sh** genererar sina utdata i mappen **build** i din lokala kopia av databasen **iot-edge**.</span><span class="sxs-lookup"><span data-stu-id="683b9-105">The **build.sh** script generates its output in the **build** folder in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="683b9-106">Här ingår två IoT kant moduler som används i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="683b9-106">This output includes the two IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="683b9-107">Build-skriptet placerar **liblogger.so** i mappen **build/modules/logger/** och **libhello\_world.so** i mappen **build/modules/hello_world/**.</span><span class="sxs-lookup"><span data-stu-id="683b9-107">The build script places **liblogger.so** in the **build/modules/logger/** folder and **libhello\_world.so** in the **build/modules/hello_world/** folder.</span></span> <span data-ttu-id="683b9-108">Använd dessa sökvägar för den **modulen sökvägen** värden som visas i följande exempel JSON filen.</span><span class="sxs-lookup"><span data-stu-id="683b9-108">Use these paths for the **module path** values as shown in the following example JSON settings file.</span></span>

<span data-ttu-id="683b9-109">Hello\_world\_sample-processen använder sökvägen till en JSON-konfigurationsfil som ett argument på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="683b9-109">The hello\_world\_sample process takes the path to a JSON configuration file a command-line argument.</span></span> <span data-ttu-id="683b9-110">I följande exempel anges JSON-filen i SDK-databasen under **samples/hello\_world/src/hello\_world\_lin.json**.</span><span class="sxs-lookup"><span data-stu-id="683b9-110">The following example JSON file is provided in the SDK repository at **samples/hello\_world/src/hello\_world\_lin.json**.</span></span> <span data-ttu-id="683b9-111">Den här konfigurationsfilen fungerar som om du inte ändrar build-skript för att placera IoT kant moduler eller exempel körbara filer i icke-standardplatser.</span><span class="sxs-lookup"><span data-stu-id="683b9-111">This configuration file works as is unless you modify the build script to place the IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="683b9-112">Modulernas sökvägar är relativa till den aktuella arbetskatalog som den körbara hello\_world\_sample-filen startas från, inte katalogen som den körbara filen finns i.</span><span class="sxs-lookup"><span data-stu-id="683b9-112">The module paths are relative to the current working directory from where the hello\_world\_sample executable is launched, not the directory where the executable is located.</span></span> <span data-ttu-id="683b9-113">JSON-exempelkonfigurationsfilen skriver som standard ”log.txt” i din aktuella arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="683b9-113">The sample JSON configuration file defaults to writing 'log.txt' in your current working directory.</span></span>

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

1. <span data-ttu-id="683b9-114">Navigera till den **skapa** mapp i roten av den lokala kopian av den **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="683b9-114">Navigate to the **build** folder in the root of your local copy of the **iot-edge** repository.</span></span>

1. <span data-ttu-id="683b9-115">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="683b9-115">Run the following command:</span></span>

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json`
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

