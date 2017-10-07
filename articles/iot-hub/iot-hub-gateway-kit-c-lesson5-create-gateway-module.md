---
title: "aaaCreate din första Azure IoT Gateway-modulen | Microsoft Docs"
description: "Skapa en modul och Lägg den tooa exempel app toocustomize modulen beteenden."
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
ms.openlocfilehash: 48996fc026c8b708e328b5629801465810e5b6a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="lesson-5-create-your-first-azure-iot-gateway-module"></a>Lektionen 5: Skapa ditt första Azure IoT Gateway-modul
Med Azure IoT kant kan toobuild moduler som skrivits i Java, .NET eller Node.js, vägleder den här självstudiekursen dig genom hello stegen för att skapa en modul i C.

## <a name="what-you-will-do"></a>Vad du ska göra

- Kompilera och köra hello hello_world sample-appen på Intel NUC.
- Skapa en modul och kompilera på Intel NUC.
- Lägg till hello ny modul toohello hello_world sample-appen och sedan köra hello prov på Intel NUC. hello ny modul skriver ut ”hello_world” meddelanden med en tidsstämpel.

## <a name="what-you-will-learn"></a>Vad får du lära dig

- Hur toocompile och köra en exempelapp på Intel NUC.
- Hur toocreate en modul.
- Hur tooadd modulen tooa sample-appen.

## <a name="what-you-need"></a>Vad du behöver

Azure IoT-kant som har installerats på värddatorn.

## <a name="folder-structure"></a>Mappstruktur

Hello lektionen 5 undermapp av hello exempelkod som du har klonat i lektionen 1, det finns en `module` mapp och en `sample` mapp.

![my_module](media/iot-hub-gateway-kit-lessons/lesson5/my_module.png)

- Hej `module/my_module` mappen innehåller hello kod och skript toobuild hello modul.
- Hej `sample` mappen innehåller hello källa kod och skript toobuild hello sample-appen.

## <a name="compile-and-run-hello-helloworld-sample-app-on-intel-nuc"></a>Kompilera och köra hello hello_world sample-appen på Intel NUC

Hej `hello_world` exemplet skapar gateway baserat på hello `hello_world.json` -fil som anger hello två fördefinierade moduler som är associerade med hello app. hello gateway loggar tooa för ”hello world” meddelandefilen 5 sekunder. I det här avsnittet kan du kompilera och köra hello `hello_world` app med den standard-modulen.

toocompile och kör hello `hello_world` appen, Följ dessa steg på värddatorn:

1. Initiera hello konfigurationsfiler genom att köra följande kommandon hello:

   ```bash
   cd iot-hub-c-intel-nuc-gateway-getting-started
   cd Lesson5
   npm install
   gulp init
   ```

1. Uppdatera konfigurationsfilen för hello gateway med hello Intel NUC MAC-adress. Hoppa över det här steget om du har gått igenom hello lektionen för[konfigurera och köra ett exempelprogram TIVERA][config_ble].

   1. Öppna konfigurationsfilen för hello gateway genom att köra följande kommando hello:

      ```bash
      # For Windows command prompt
      code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json

      # For MacOS or Ubuntu
      code ~/.iot-hub-getting-started/config-gateway.json
      ```

   1. Uppdateringen hello gateway MAC-adressen när du [konfigurera Intel NUC som en IoT-gateway][setup_nuc], och sedan spara hello-filen.

1. Kompilera hello exempelkod för källdatorn genom att köra följande kommando hello:

   ```bash
   gulp compile
   ```

   hello kommandot överför hello exempel källa kod tooIntel NUC och kör `build.sh` toocompile den.

1. Kör hello `hello_world` appen på Intel NUC genom att köra följande kommando hello:

   ```bash
   gulp run
   ```

   Hej kommandot körs `../Tools/run-hello-world.js` som anges i `config.json` toostart hello sample-appen på Intel NUC.

   ![run_sample](media/iot-hub-gateway-kit-lessons/lesson5/run_sample.png)

## <a name="create-a-new-module-and-compile-it-on-intel-nuc"></a>Skapa en ny modul och kompilera på Intel NUC

hello stegen nedan hjälper dig att skapa en ny modul och kompilera på Intel NUC. hello modulen skriver ut meddelanden med en tidsstämpel vid mottagning av dem. I det här avsnittet skapar du din första anpassade gateway-modulen.

Azure IoT kant modul måste implementera hello följande gränssnitt:

   ```C
   pfModule_ParseConfigurationFromJson Module_ParseConfigurationFromJson
   pfModule_FreeConfiguration Module_FreeConfiguration
   pfModule_Create Module_Create
   pfModule_Destroy Module_Destroy
   pfModule_Receive Module_Receive
   ```

Du kan eventuellt implementera hello följande gränssnitt:

   ```C
   pfModule_Start Module_Start
   ```

hello visar följande diagram hello viktiga tillstånd sökvägar för en modul. hello kvadratisk rektanglar representerar metoder som du implementerar tooperform åtgärder när hello modulen flyttar emellan. hello oval är större tillstånd hello modul kan ha.

![state_path](media/iot-hub-gateway-kit-lessons/lesson5/state_path.png)

Nu ska vi skapa en modul utifrån hello mallen:

1. Öppna hello mallmapp genom att köra följande kommando hello:

   ```bash
   code module/my_module
   ```

   ![code_module](media/iot-hub-gateway-kit-lessons/lesson5/code_module.png)

   - `src/my_module.c`fungerar som en mall som underlättar hello skapandet av en modul. hello mallen deklarerar hello gränssnitt. Allt du behöver toodo är tooadd logik toohello `MyModule_Receive` funktion.
   - `build.sh`är hello build toocompile hello skriptmodul på Intel NUC.
1. Öppna hello `src/my_module.c` filen och innehåller två rubrikfiler:

   ```C
   #include <stdio.h>
   #include "azure_c_shared_utility/xlogging.h"
   ```

1. Lägg till följande kod toohello hello `MyModule_Receive` funktionen:

   ```C
   if (message == NULL)
   {
      LogError("invalid arg message");
   }
   else
   {
      // get hello message content
      const CONSTBUFFER * content = Message_GetContent(message);
      // get hello local time and format it
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
                  LogError("unable toostrftime");
              }
              else
              {
                  printf("[%s] %.*s\r\n", timetemp, (int)content->size, content->buffer);
              }
          }
      }
   }
   ```

1. Uppdatera hello `config.json` filen toospecify hello `workspace` mapp på din dator och hello distribution värdsökvägen på Intel NUC. Under kompilering, hello filer i hello `workspace` mappen kommer att vara överförda toohello sökvägen.

   1. Öppna hello `config.json` filen genom att köra följande kommando hello:

      ```bash
      code config.json
      ```

   1. Uppdatera `config.json` med hello följande konfiguration:

      ```json
      "workspace": "./module/my_module",
      "deploy_path": "module/my_module"
      ```

      ![config_json](media/iot-hub-gateway-kit-lessons/lesson5/config_json.png)

1. Kompilera hello modulen genom att köra följande kommando hello:

   ```bash
   gulp compile
   ```

   hello kommandot överför hello källa kod tooIntel NUC och kör `build.sh` toocompile hello modulen.

## <a name="add-hello-module-toohello-helloworld-sample-app-and-run-hello-app-on-intel-nuc"></a>Lägg till hello modulen toohello hello_world sample-appen och kör hello app på Intel NUC

tooperform detta uppgift, gör du följande:

1. Lista alla hello tillgängliga modulen binärfiler (.so-filer) på Intel NUC genom att köra följande kommando hello:

   ```bash
   gulp modules --list
   ```

   hello binär sökväg för `my_module` som du kompilerat bör nu visas enligt nedan:

   ```path
   /root/gateway_sample/module/my_module/build/libmy_module.so
   ```

   Om du ändrar hello inloggningen Standardanvändarnamnet i `config-gateway.json`, hello binär sökväg startar med `home/<your username>` i stället för `root`.

1. Lägg till `my_module` toohello `hello_world` exempelapp:

   1. Öppna hello `hello_world.json` filen genom att köra följande kommando hello:

      ```bash
      code sample/hello_world/src/hello_world.json
      ```

   1. Lägg till följande kod toohello hello `modules` avsnitt:

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

      Hej värdet för `module.path` ska vara `/root/gateway_sample/module/my_module/build/libmy_module.so`. hello koden deklarerar `my_module` toobe som är associerade med hello gateway samt hello plats hello modulen binära anges i `module.path`.
   1. Lägg till följande kod toohello hello `links` avsnitt:

      ```json
      {
        "source": "hello_world",
        "sink": "my_module"
      }
      ```

      Den här koden anger att meddelanden överförs från hello `hello_world` modul för`my_module`.

      ![hello_world_json](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_json.png)

1. Kör hello `hello_world` sample-appen genom att köra följande kommando hello:

   ```bash
   gulp run --config sample/hello_world/src/hello_world.json
   ```

   Hej `--config` parametern tvingar hello `run-hello-world.js` skript toorun med hjälp av hello `hello_world.json` fil.

   ![hello_world_new](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_new.png)

Grattis! Du kan nu se hello beteendet för den här nya modulen, den bara skrivs ut ”hello world”-meddelanden med en tidsstämpel, ett annat slutresultat hello ursprungliga ”hello_world”-modul.

## <a name="next-steps"></a>Nästa steg

Du har skapat en ny modul, lade till den toohello hello_world exemplet och get hello exempel app toorun med nya hello-modulen på din gateway. Om du vill toolearn mer om Azure IoT gateway moduler, hittar du mer modulen exempel här: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).

<!-- Images and links -->

[config_ble]: iot-hub-gateway-kit-c-lesson3-configure-ble-app.md
[setup_nuc]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
