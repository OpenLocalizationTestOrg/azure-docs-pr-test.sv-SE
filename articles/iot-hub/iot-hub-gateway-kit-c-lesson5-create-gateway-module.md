---
title: "Skapa din första Azure IoT Gateway-modulen | Microsoft Docs"
description: "Skapa en modul och lägga till den i en exempelapp för att anpassa modulen beteenden."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: cd7660f4-7b8b-4091-8d71-bb8723165b0b
ms.service: iot-hub
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5e28422158684c3aaf0ac3fdf5b19c80fbccfb02
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-5-create-your-first-azure-iot-gateway-module"></a>Lektionen 5: Skapa ditt första Azure IoT Gateway-modul
Med Azure IoT gräns kan du skapa moduler som skrivits i Java, .NET eller Node.js, vägleder den här självstudiekursen dig genom stegen för att skapa en modul i C.

## <a name="what-you-will-do"></a>Vad du ska göra

- Kompilera och köra hello_world sample-appen på Intel NUC.
- Skapa en modul och kompilera på Intel NUC.
- Lägg till ny modul hello_world sample-appen och sedan köra exemplet på Intel NUC. Ny modul skriver ut ”hello_world” meddelanden med en tidsstämpel.

## <a name="what-you-will-learn"></a>Vad får du lära dig

- Så att kompilera och köra en exempelapp på Intel NUC.
- Så här skapar du en modul.
- Hur du lägger till modulen sample-appen.

## <a name="what-you-need"></a>Vad du behöver

Azure IoT-kant som har installerats på värddatorn.

## <a name="folder-structure"></a>Mappstruktur

Det finns i undermappen lektionen 5 i exempelkoden som du har klonat i lektionen 1 en `module` mapp och en `sample` mapp.

![my_module](media/iot-hub-gateway-kit-lessons/lesson5/my_module.png)

- Den `module/my_module` mappen innehåller källkoden och skript för att skapa modulen.
- Den `sample` mappen innehåller källkoden och skript för att skapa sample-appen.

## <a name="compile-and-run-the-helloworld-sample-app-on-intel-nuc"></a>Kompilera och köra hello_world sample-appen på Intel NUC

Den `hello_world` exemplet skapar gateway baserat på den `hello_world.json` -fil som anger de två fördefinierade moduler som är associerat med appen. Gatewayen loggas ett meddelande om ”hello world” i en fil var femte sekund. I det här avsnittet kan du kompilera och köra den `hello_world` app med den standard-modulen.

Att kompilera och köra den `hello_world` appen, Följ dessa steg på värddatorn:

1. Initiera konfigurationsfilerna genom att köra följande kommandon:

   ```bash
   cd iot-hub-c-intel-nuc-gateway-getting-started
   cd Lesson5
   npm install
   gulp init
   ```

1. Uppdatera konfigurationsfilen gateway med Intel NUC MAC-adress. Hoppa över det här steget om du har gått igenom lektionen till [konfigurera och köra ett exempelprogram TIVERA][config_ble].

   1. Öppna konfigurationsfilen gateway genom att köra följande kommando:

      ```bash
      # For Windows command prompt
      code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json

      # For MacOS or Ubuntu
      code ~/.iot-hub-getting-started/config-gateway.json
      ```

   1. Uppdatering av gateway-MAC-adressen när du [konfigurera Intel NUC som en gateway för IoT][setup_nuc], och sedan spara filen.

1. Kompilera källkoden exemplet genom att köra följande kommando:

   ```bash
   gulp compile
   ```

   Kommandot överför exempelkod för källan till Intel NUC och kör `build.sh` att kompilera den.

1. Kör den `hello_world` appen på Intel NUC genom att köra följande kommando:

   ```bash
   gulp run
   ```

   Kommandot körs `../Tools/run-hello-world.js` som anges i `config.json` starta sample-appen på Intel NUC.

   ![run_sample](media/iot-hub-gateway-kit-lessons/lesson5/run_sample.png)

## <a name="create-a-new-module-and-compile-it-on-intel-nuc"></a>Skapa en ny modul och kompilera på Intel NUC

Stegen nedan hjälper dig att skapa en ny modul och kompilera på Intel NUC. Modulen skriver ut meddelanden med en tidsstämpel vid mottagning av dem. I det här avsnittet skapar du din första anpassade gateway-modulen.

Azure IoT kant modul måste implementera följande gränssnitt:

   ```C
   pfModule_ParseConfigurationFromJson Module_ParseConfigurationFromJson
   pfModule_FreeConfiguration Module_FreeConfiguration
   pfModule_Create Module_Create
   pfModule_Destroy Module_Destroy
   pfModule_Receive Module_Receive
   ```

Alternativt kan du implementera följande gränssnitt:

   ```C
   pfModule_Start Module_Start
   ```

Följande diagram visar viktiga tillstånd sökvägar för en modul. Kvadratisk rektanglar representerar metoder som du implementerar för att utföra åtgärder när modulen flyttar emellan. Ovalerna är större tillstånd modulen kan vara i.

![state_path](media/iot-hub-gateway-kit-lessons/lesson5/state_path.png)

Nu ska vi skapa en modul som baseras på mallen:

1. Öppna mallmappen genom att köra följande kommando:

   ```bash
   code module/my_module
   ```

   ![code_module](media/iot-hub-gateway-kit-lessons/lesson5/code_module.png)

   - `src/my_module.c`fungerar som en mall som underlättar skapandet av en modul. Mallen deklarerar gränssnitten. Allt du behöver göra är att lägga till kod till den `MyModule_Receive` funktion.
   - `build.sh`är build-skript för att kompilera modulen på Intel NUC.
1. Öppna den `src/my_module.c` filen och innehåller två rubrikfiler:

   ```C
   #include <stdio.h>
   #include "azure_c_shared_utility/xlogging.h"
   ```

1. Lägg till följande kod i den `MyModule_Receive` funktionen:

   ```C
   if (message == NULL)
   {
      LogError("invalid arg message");
   }
   else
   {
      // get the message content
      const CONSTBUFFER * content = Message_GetContent(message);
      // get the local time and format it
      time_t temp = time(NULL);
      if (temp == (time_t)-1)
      {
          LogError("time function failed");
      }
      else
      {
          struct tm* t = localtime(&temp);
          if (t == NULL)
          {
              LogError("localtime failed");
          }
          else
          {
              char timetemp[80] = { 0 };
              if (strftime(timetemp, sizeof(timetemp) / sizeof(timetemp[0]), "%c", t) == 0)
              {
                  LogError("unable to strftime");
              }
              else
              {
                  printf("[%s] %.*s\r\n", timetemp, (int)content->size, content->buffer);
              }
          }
      }
   }
   ```

1. Uppdatering av `config.json` att ange den `workspace` mapp på värddatorn och sökvägen för distribution på Intel NUC. Under kompilering av filerna i den `workspace` mappen kommer att överföras till sökvägen för distribution.

   1. Öppna den `config.json` filen genom att köra följande kommando:

      ```bash
      code config.json
      ```

   1. Uppdatera `config.json` med följande konfiguration:

      ```json
      "workspace": "./module/my_module",
      "deploy_path": "module/my_module"
      ```

      ![config_json](media/iot-hub-gateway-kit-lessons/lesson5/config_json.png)

1. Kompilera modulen genom att köra följande kommando:

   ```bash
   gulp compile
   ```

   Kommandot överför källkoden till Intel NUC och kör `build.sh` att kompilera modulen.

## <a name="add-the-module-to-the-helloworld-sample-app-and-run-the-app-on-intel-nuc"></a>Lägga till modulen hello_world sample-appen och kör appen på Intel NUC

Följ dessa steg om du vill utföra den här uppgiften:

1. Lista alla tillgängliga modulen binärfilerna (.so-filer) på Intel NUC genom att köra följande kommando:

   ```bash
   gulp modules --list
   ```

   Binär sökväg för `my_module` som du kompilerat bör nu visas enligt nedan:

   ```path
   /root/gateway_sample/module/my_module/build/libmy_module.so
   ```

   Om du ändrar inloggningen Standardanvändarnamnet i `config-gateway.json`, binär sökväg startar med `home/<your username>` i stället för `root`.

1. Lägg till `my_module` till den `hello_world` sample-appen:

   1. Öppna den `hello_world.json` filen genom att köra följande kommando:

      ```bash
      code sample/hello_world/src/hello_world.json
      ```

   1. Lägg till följande kod i den `modules` avsnitt:

      ```json
      {
       "name": "my_module",
       "loader": {
       "name": "native",
       "entrypoint": {
       "module.path": "/root/gateway_sample/module/my_module/build/libmy_module.so"
         }
        },
       "args": null
      }
      ```

      Värdet för `module.path` ska vara `/root/gateway_sample/module/my_module/build/libmy_module.so`. Koden deklarerar `my_module` som ska associeras med gatewayen som platsen för den binära modul som anges i `module.path`.
   1. Lägg till följande kod i den `links` avsnitt:

      ```json
      {
        "source": "hello_world",
        "sink": "my_module"
      }
      ```

      Den här koden anger att meddelanden överförs från den `hello_world` modulen `my_module`.

      ![hello_world_json](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_json.png)

1. Kör den `hello_world` sample-appen genom att köra följande kommando:

   ```bash
   gulp run --config sample/hello_world/src/hello_world.json
   ```

   Den `--config` parametern tvingar den `run-hello-world.js` skriptet ska köras med hjälp av den `hello_world.json` filen.

   ![hello_world_new](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_new.png)

Grattis. Du kan nu se beteendet för den här nya modulen, den bara skrivs ut ”hello world”-meddelanden med en tidsstämpel, ett annat resultat från den ursprungliga modulen ”hello_world”.

## <a name="next-steps"></a>Nästa steg

Du har skapat en ny modul, lägga till exempel hello_world och get sample-appen ska köras med den nya modulen på din gateway. Om du vill veta mer om Azure IoT gateway moduler hittar du mer modulen exempel här: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).

<!-- Images and links -->

[config_ble]: iot-hub-gateway-kit-c-lesson3-configure-ble-app.md
[setup_nuc]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
