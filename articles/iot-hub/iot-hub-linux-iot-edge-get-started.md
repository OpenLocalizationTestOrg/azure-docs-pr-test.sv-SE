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
# <a name="explore-azure-iot-edge-architecture-on-linux"></a>Utforska Azure IoT Edge-arkitektur på Linux

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>Köra exemplet

Skriptet **build.sh** genererar sina utdata i mappen **build** i din lokala kopia av databasen **iot-edge**. Här ingår två IoT kant moduler som används i det här exemplet.

Build-skriptet placerar **liblogger.so** i mappen **build/modules/logger/** och **libhello\_world.so** i mappen **build/modules/hello_world/**. Använd dessa sökvägar för den **modulen sökvägen** värden som visas i följande exempel JSON filen.

Hello\_world\_sample-processen använder sökvägen till en JSON-konfigurationsfil som ett argument på kommandoraden. I följande exempel anges JSON-filen i SDK-databasen under **samples/hello\_world/src/hello\_world\_lin.json**. Den här konfigurationsfilen fungerar som om du inte ändrar build-skript för att placera IoT kant moduler eller exempel körbara filer i icke-standardplatser.

> [!NOTE]
> Modulernas sökvägar är relativa till den aktuella arbetskatalog som den körbara hello\_world\_sample-filen startas från, inte katalogen som den körbara filen finns i. JSON-exempelkonfigurationsfilen skriver som standard ”log.txt” i din aktuella arbetskatalog.

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

1. Navigera till den **skapa** mapp i roten av den lokala kopian av den **iot-edge** databasen.

1. Kör följande kommando:

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json`
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

