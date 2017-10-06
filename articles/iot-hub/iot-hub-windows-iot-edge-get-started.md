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
# <a name="explore-azure-iot-edge-architecture-on-windows"></a>Utforska Azure IoT kant arkitektur i Windows

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-toorun-hello-sample"></a>Hur toorun hello exempel

Hej **build.cmd** skriptet genererar utdata i hello **skapa** mapp i den lokala kopian av hello **iot-edge** databasen. Här ingår hello två IoT kant moduler som används i det här exemplet.

Hej build skript platser **logger.dll** i hello **skapa\\moduler\\loggaren\\felsöka** mapp och **hello\_world.dll**  i hello **skapa\\moduler\\hello_world\\felsöka** mapp. Använd dessa sökvägar för hello **modulen sökvägen** värden som visas i följande JSON-inställningsfilen hello.

hello hello\_world\_exempel processen tar hello sökvägen tooa JSON-konfigurationsfil som kommandoradsargument. hello följande exempel JSON-fil har angetts i hello SDK lagringsplats på **exempel\\hello\_world\\src\\hello\_world\_win.json**. Den här konfigurationen filen fungerar som om du inte ändrar hello skapa skript tooplace hello IoT kant moduler eller exempel körbara filer i icke-standardplatser.

> [!NOTE]
> hello modulen sökvägar är relativ toohello directory där hello hello\_world\_sample.exe finns. hello exempel JSON configuration file standardvärden toowriting 'log.txt' i den aktuella arbetskatalogen.

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

1. Navigera toohello **skapa** mapp i hello roten för den lokala kopian av hello **iot-edge** databasen.

1. Kör följande kommando hello:

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
