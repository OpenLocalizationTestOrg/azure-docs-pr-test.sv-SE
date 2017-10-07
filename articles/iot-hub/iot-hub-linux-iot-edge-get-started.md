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
# <a name="explore-azure-iot-edge-architecture-on-linux"></a>Utforska Azure IoT Edge-arkitektur på Linux

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-toorun-hello-sample"></a>Hur toorun hello exempel

Hej **build.sh** skriptet genererar utdata i hello **skapa** mapp i den lokala kopian av hello **iot-edge** databasen. Här ingår hello två IoT kant moduler som används i det här exemplet.

Hej build skript platser **liblogger.so** i hello **build-moduler-logg/** mapp och **libhello\_world.so** i hello **skapa / moduler/hello_world/** mapp. Använd dessa sökvägar för hello **modulen sökvägen** värden som visas i hello följande exempelfil JSON-inställningar.

hello hello\_world\_exempel processen tar hello sökvägen tooa JSON-konfigurationsfil ett kommandoradsargument. hello följande exempel JSON-fil har angetts i hello SDK lagringsplats på **exempel/hello\_world/src/hello\_world\_lin.json**. Den här konfigurationen filen fungerar som om du inte ändrar hello skapa skript tooplace hello IoT kant moduler eller exempel körbara filer i icke-standardplatser.

> [!NOTE]
> hello modulen sökvägar är relativ toohello aktuella arbetskatalogen varifrån hello hello\_world\_exempel körbar fil startas, hello katalogen där hello körbara finns inte. hello exempel JSON configuration file standardvärden toowriting 'log.txt' i den aktuella arbetskatalogen.

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

1. Navigera toohello **skapa** mapp i hello roten för den lokala kopian av hello **iot-edge** databasen.

1. Kör följande kommando hello:

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json`
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

