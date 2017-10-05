---
title: "Kom igång med Azure IoT kant (Windows) | Microsoft Docs"
description: "Hur du skapar ett Azure IoT gräns-gatewayen på en Windows-dator och lär dig mer om nyckelbegrepp i Azure IoT kant som JSON-konfigurationsfiler och moduler."
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
ms.openlocfilehash: 5db39bab8e31a8e7026b34e72b4614b0f6f57772
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-windows"></a>Utforska Azure IoT kant arkitektur i Windows

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-to-run-the-sample"></a>Köra exemplet

Den **build.cmd** skriptet genererar utdata i den **skapa** mapp i den lokala kopian av den **iot-edge** databasen. Här ingår två IoT kant moduler som används i det här exemplet.

Skapa skript platser **logger.dll** i den **skapa\\moduler\\loggaren\\felsöka** mapp och **hello\_world.dll** i den **skapa\\moduler\\hello_world\\felsöka** mapp. Använd dessa sökvägar för den **modulen sökvägen** värden som visas i JSON-filen för följande inställningar.

Hello\_world\_exempel processen tar sökvägen till en JSON-konfigurationsfil som kommandoradsargument. Följande exempel JSON-filen finns i SDK-databasen på **exempel\\hello\_world\\src\\hello\_world\_win.json**. Den här konfigurationsfilen fungerar som om du inte ändrar build-skript för att placera IoT kant moduler eller exempel körbara filer i icke-standardplatser.

> [!NOTE]
> Modul-sökvägar är i förhållande till katalogen där hello\_world\_sample.exe finns. JSON-exempelkonfigurationsfilen skriver som standard ”log.txt” i din aktuella arbetskatalog.

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

1. Navigera till den **skapa** mapp i roten av den lokala kopian av den **iot-edge** databasen.

1. Kör följande kommando:

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
