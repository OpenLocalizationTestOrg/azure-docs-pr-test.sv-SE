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
# <a name="explore-azure-iot-edge-architecture-on-windows"></a><span data-ttu-id="73e2d-103">Utforska Azure IoT kant arkitektur i Windows</span><span class="sxs-lookup"><span data-stu-id="73e2d-103">Explore Azure IoT Edge architecture on Windows</span></span>

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-to-run-the-sample"></a><span data-ttu-id="73e2d-104">Köra exemplet</span><span class="sxs-lookup"><span data-stu-id="73e2d-104">How to run the sample</span></span>

<span data-ttu-id="73e2d-105">Den **build.cmd** skriptet genererar utdata i den **skapa** mapp i den lokala kopian av den **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="73e2d-105">The **build.cmd** script generates its output in the **build** folder in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="73e2d-106">Här ingår två IoT kant moduler som används i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="73e2d-106">This output includes the two IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="73e2d-107">Skapa skript platser **logger.dll** i den **skapa\\moduler\\loggaren\\felsöka** mapp och **hello\_world.dll** i den **skapa\\moduler\\hello_world\\felsöka** mapp.</span><span class="sxs-lookup"><span data-stu-id="73e2d-107">The build script places **logger.dll** in the **build\\modules\\logger\\Debug** folder and **hello\_world.dll** in the **build\\modules\\hello_world\\Debug** folder.</span></span> <span data-ttu-id="73e2d-108">Använd dessa sökvägar för den **modulen sökvägen** värden som visas i JSON-filen för följande inställningar.</span><span class="sxs-lookup"><span data-stu-id="73e2d-108">Use these paths for the **module path** values as shown in the following JSON settings file.</span></span>

<span data-ttu-id="73e2d-109">Hello\_world\_exempel processen tar sökvägen till en JSON-konfigurationsfil som kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="73e2d-109">The hello\_world\_sample process takes the path to a JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="73e2d-110">Följande exempel JSON-filen finns i SDK-databasen på **exempel\\hello\_world\\src\\hello\_world\_win.json**.</span><span class="sxs-lookup"><span data-stu-id="73e2d-110">The following example JSON file is provided in the SDK repository at **samples\\hello\_world\\src\\hello\_world\_win.json**.</span></span> <span data-ttu-id="73e2d-111">Den här konfigurationsfilen fungerar som om du inte ändrar build-skript för att placera IoT kant moduler eller exempel körbara filer i icke-standardplatser.</span><span class="sxs-lookup"><span data-stu-id="73e2d-111">This configuration file works as is unless you modify the build script to place the IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="73e2d-112">Modul-sökvägar är i förhållande till katalogen där hello\_world\_sample.exe finns.</span><span class="sxs-lookup"><span data-stu-id="73e2d-112">The module paths are relative to the directory where the hello\_world\_sample.exe is located.</span></span> <span data-ttu-id="73e2d-113">JSON-exempelkonfigurationsfilen skriver som standard ”log.txt” i din aktuella arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="73e2d-113">The sample JSON configuration file defaults to writing 'log.txt' in your current working directory.</span></span>

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

1. <span data-ttu-id="73e2d-114">Navigera till den **skapa** mapp i roten av den lokala kopian av den **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="73e2d-114">Navigate to the **build** folder in the root of your local copy of the **iot-edge** repository.</span></span>

1. <span data-ttu-id="73e2d-115">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="73e2d-115">Run the following command:</span></span>

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
