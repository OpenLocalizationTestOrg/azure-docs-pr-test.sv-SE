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
# <a name="lesson-5-create-your-first-azure-iot-gateway-module"></a><span data-ttu-id="dfe90-103">Lektionen 5: Skapa ditt första Azure IoT Gateway-modul</span><span class="sxs-lookup"><span data-stu-id="dfe90-103">Lesson 5: Create your first Azure IoT Gateway module</span></span>
<span data-ttu-id="dfe90-104">Med Azure IoT kant kan toobuild moduler som skrivits i Java, .NET eller Node.js, vägleder den här självstudiekursen dig genom hello stegen för att skapa en modul i C.</span><span class="sxs-lookup"><span data-stu-id="dfe90-104">While Azure IoT Edge allows you toobuild modules written in Java, .NET, or Node.js, this tutorial walks you through hello steps for building a module in C.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="dfe90-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="dfe90-105">What you will do</span></span>

- <span data-ttu-id="dfe90-106">Kompilera och köra hello hello_world sample-appen på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="dfe90-106">Compile and run hello hello_world sample app on Intel NUC.</span></span>
- <span data-ttu-id="dfe90-107">Skapa en modul och kompilera på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="dfe90-107">Create a module and compile it on Intel NUC.</span></span>
- <span data-ttu-id="dfe90-108">Lägg till hello ny modul toohello hello_world sample-appen och sedan köra hello prov på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="dfe90-108">Add hello new module toohello hello_world sample app and then run hello sample on Intel NUC.</span></span> <span data-ttu-id="dfe90-109">hello ny modul skriver ut ”hello_world” meddelanden med en tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="dfe90-109">hello new module prints out "hello_world" messages with a timestamp.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="dfe90-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="dfe90-110">What you will learn</span></span>

- <span data-ttu-id="dfe90-111">Hur toocompile och köra en exempelapp på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="dfe90-111">How toocompile and run a sample app on Intel NUC.</span></span>
- <span data-ttu-id="dfe90-112">Hur toocreate en modul.</span><span class="sxs-lookup"><span data-stu-id="dfe90-112">How toocreate a module.</span></span>
- <span data-ttu-id="dfe90-113">Hur tooadd modulen tooa sample-appen.</span><span class="sxs-lookup"><span data-stu-id="dfe90-113">How tooadd module tooa sample app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="dfe90-114">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="dfe90-114">What you need</span></span>

<span data-ttu-id="dfe90-115">Azure IoT-kant som har installerats på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="dfe90-115">Azure IoT Edge that has been installed on your host computer.</span></span>

## <a name="folder-structure"></a><span data-ttu-id="dfe90-116">Mappstruktur</span><span class="sxs-lookup"><span data-stu-id="dfe90-116">Folder structure</span></span>

<span data-ttu-id="dfe90-117">Hello lektionen 5 undermapp av hello exempelkod som du har klonat i lektionen 1, det finns en `module` mapp och en `sample` mapp.</span><span class="sxs-lookup"><span data-stu-id="dfe90-117">In hello Lesson 5 subfolder of hello sample code which you cloned in lesson 1, there is a `module` folder and a `sample` folder.</span></span>

![my_module](media/iot-hub-gateway-kit-lessons/lesson5/my_module.png)

- <span data-ttu-id="dfe90-119">Hej `module/my_module` mappen innehåller hello kod och skript toobuild hello modul.</span><span class="sxs-lookup"><span data-stu-id="dfe90-119">hello `module/my_module` folder contains hello source code and script toobuild hello module.</span></span>
- <span data-ttu-id="dfe90-120">Hej `sample` mappen innehåller hello källa kod och skript toobuild hello sample-appen.</span><span class="sxs-lookup"><span data-stu-id="dfe90-120">hello `sample` folder contains hello source code and script toobuild hello sample app.</span></span>

## <a name="compile-and-run-hello-helloworld-sample-app-on-intel-nuc"></a><span data-ttu-id="dfe90-121">Kompilera och köra hello hello_world sample-appen på Intel NUC</span><span class="sxs-lookup"><span data-stu-id="dfe90-121">Compile and run hello hello_world sample app on Intel NUC</span></span>

<span data-ttu-id="dfe90-122">Hej `hello_world` exemplet skapar gateway baserat på hello `hello_world.json` -fil som anger hello två fördefinierade moduler som är associerade med hello app.</span><span class="sxs-lookup"><span data-stu-id="dfe90-122">hello `hello_world` sample creates a gateway based on hello `hello_world.json` file which specifies hello two predefined modules associated with hello app.</span></span> <span data-ttu-id="dfe90-123">hello gateway loggar tooa för ”hello world” meddelandefilen 5 sekunder.</span><span class="sxs-lookup"><span data-stu-id="dfe90-123">hello gateway logs a "hello world" message tooa file every 5 seconds.</span></span> <span data-ttu-id="dfe90-124">I det här avsnittet kan du kompilera och köra hello `hello_world` app med den standard-modulen.</span><span class="sxs-lookup"><span data-stu-id="dfe90-124">In this section, you compile and run hello `hello_world` app with its default module.</span></span>

<span data-ttu-id="dfe90-125">toocompile och kör hello `hello_world` appen, Följ dessa steg på värddatorn:</span><span class="sxs-lookup"><span data-stu-id="dfe90-125">toocompile and run hello `hello_world` app, follow these steps on your host computer:</span></span>

1. <span data-ttu-id="dfe90-126">Initiera hello konfigurationsfiler genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="dfe90-126">Initialize hello configuration files by running hello following commands:</span></span>

   ```bash
   cd iot-hub-c-intel-nuc-gateway-getting-started
   cd Lesson5
   npm install
   gulp init
   ```

1. <span data-ttu-id="dfe90-127">Uppdatera konfigurationsfilen för hello gateway med hello Intel NUC MAC-adress.</span><span class="sxs-lookup"><span data-stu-id="dfe90-127">Update hello gateway configuration file with hello MAC address of Intel NUC.</span></span> <span data-ttu-id="dfe90-128">Hoppa över det här steget om du har gått igenom hello lektionen för[konfigurera och köra ett exempelprogram TIVERA][config_ble].</span><span class="sxs-lookup"><span data-stu-id="dfe90-128">Skip this step if you have gone through hello lesson too[configure and run a BLE sample application][config_ble].</span></span>

   1. <span data-ttu-id="dfe90-129">Öppna konfigurationsfilen för hello gateway genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="dfe90-129">Open hello gateway configuration file by running hello following command:</span></span>

      ```bash
      # For Windows command prompt
      code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json

      # For MacOS or Ubuntu
      code ~/.iot-hub-getting-started/config-gateway.json
      ```

   1. <span data-ttu-id="dfe90-130">Uppdateringen hello gateway MAC-adressen när du [konfigurera Intel NUC som en IoT-gateway][setup_nuc], och sedan spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="dfe90-130">Update hello gateway's MAC address when you [set up Intel NUC as a IoT gateway][setup_nuc], and then save hello file.</span></span>

1. <span data-ttu-id="dfe90-131">Kompilera hello exempelkod för källdatorn genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="dfe90-131">Compile hello sample source code by running hello following command:</span></span>

   ```bash
   gulp compile
   ```

   <span data-ttu-id="dfe90-132">hello kommandot överför hello exempel källa kod tooIntel NUC och kör `build.sh` toocompile den.</span><span class="sxs-lookup"><span data-stu-id="dfe90-132">hello command transfers hello sample source code tooIntel NUC and runs `build.sh` toocompile it.</span></span>

1. <span data-ttu-id="dfe90-133">Kör hello `hello_world` appen på Intel NUC genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="dfe90-133">Run hello `hello_world` app on Intel NUC by running hello following command:</span></span>

   ```bash
   gulp run
   ```

   <span data-ttu-id="dfe90-134">Hej kommandot körs `../Tools/run-hello-world.js` som anges i `config.json` toostart hello sample-appen på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="dfe90-134">hello command runs `../Tools/run-hello-world.js` that is specified in `config.json` toostart hello sample app on Intel NUC.</span></span>

   ![run_sample](media/iot-hub-gateway-kit-lessons/lesson5/run_sample.png)

## <a name="create-a-new-module-and-compile-it-on-intel-nuc"></a><span data-ttu-id="dfe90-136">Skapa en ny modul och kompilera på Intel NUC</span><span class="sxs-lookup"><span data-stu-id="dfe90-136">Create a new module and compile it on Intel NUC</span></span>

<span data-ttu-id="dfe90-137">hello stegen nedan hjälper dig att skapa en ny modul och kompilera på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="dfe90-137">hello steps below walk you through creating a new module and compile it on Intel NUC.</span></span> <span data-ttu-id="dfe90-138">hello modulen skriver ut meddelanden med en tidsstämpel vid mottagning av dem.</span><span class="sxs-lookup"><span data-stu-id="dfe90-138">hello module prints out messages with a timestamp upon receiving them.</span></span> <span data-ttu-id="dfe90-139">I det här avsnittet skapar du din första anpassade gateway-modulen.</span><span class="sxs-lookup"><span data-stu-id="dfe90-139">You will create your first customized gateway module in this section.</span></span>

<span data-ttu-id="dfe90-140">Azure IoT kant modul måste implementera hello följande gränssnitt:</span><span class="sxs-lookup"><span data-stu-id="dfe90-140">Any Azure IoT Edge module must implement hello following interfaces:</span></span>

   ```C
   pfModule_ParseConfigurationFromJson Module_ParseConfigurationFromJson
   pfModule_FreeConfiguration Module_FreeConfiguration
   pfModule_Create Module_Create
   pfModule_Destroy Module_Destroy
   pfModule_Receive Module_Receive
   ```

<span data-ttu-id="dfe90-141">Du kan eventuellt implementera hello följande gränssnitt:</span><span class="sxs-lookup"><span data-stu-id="dfe90-141">You can optionally implement hello following interface:</span></span>

   ```C
   pfModule_Start Module_Start
   ```

<span data-ttu-id="dfe90-142">hello visar följande diagram hello viktiga tillstånd sökvägar för en modul.</span><span class="sxs-lookup"><span data-stu-id="dfe90-142">hello following diagram shows hello important state paths of a module.</span></span> <span data-ttu-id="dfe90-143">hello kvadratisk rektanglar representerar metoder som du implementerar tooperform åtgärder när hello modulen flyttar emellan.</span><span class="sxs-lookup"><span data-stu-id="dfe90-143">hello square rectangles represent methods you implement tooperform operations when hello module moves between states.</span></span> <span data-ttu-id="dfe90-144">hello oval är större tillstånd hello modul kan ha.</span><span class="sxs-lookup"><span data-stu-id="dfe90-144">hello ovals are major states hello module can be in.</span></span>

![state_path](media/iot-hub-gateway-kit-lessons/lesson5/state_path.png)

<span data-ttu-id="dfe90-146">Nu ska vi skapa en modul utifrån hello mallen:</span><span class="sxs-lookup"><span data-stu-id="dfe90-146">Now let’s create a module based on hello template:</span></span>

1. <span data-ttu-id="dfe90-147">Öppna hello mallmapp genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="dfe90-147">Open hello template folder by running hello following command:</span></span>

   ```bash
   code module/my_module
   ```

   ![code_module](media/iot-hub-gateway-kit-lessons/lesson5/code_module.png)

   - <span data-ttu-id="dfe90-149">`src/my_module.c`fungerar som en mall som underlättar hello skapandet av en modul.</span><span class="sxs-lookup"><span data-stu-id="dfe90-149">`src/my_module.c` serves as a template that facilitates hello creation of a module.</span></span> <span data-ttu-id="dfe90-150">hello mallen deklarerar hello gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="dfe90-150">hello template declares hello interfaces.</span></span> <span data-ttu-id="dfe90-151">Allt du behöver toodo är tooadd logik toohello `MyModule_Receive` funktion.</span><span class="sxs-lookup"><span data-stu-id="dfe90-151">All you need toodo is tooadd logic toohello `MyModule_Receive` function.</span></span>
   - <span data-ttu-id="dfe90-152">`build.sh`är hello build toocompile hello skriptmodul på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="dfe90-152">`build.sh` is hello build script toocompile hello module on Intel NUC.</span></span>
1. <span data-ttu-id="dfe90-153">Öppna hello `src/my_module.c` filen och innehåller två rubrikfiler:</span><span class="sxs-lookup"><span data-stu-id="dfe90-153">Open hello `src/my_module.c` file and include two header files:</span></span>

   ```C
   #include <stdio.h>
   #include "azure_c_shared_utility/xlogging.h"
   ```

1. <span data-ttu-id="dfe90-154">Lägg till följande kod toohello hello `MyModule_Receive` funktionen:</span><span class="sxs-lookup"><span data-stu-id="dfe90-154">Add hello following code toohello `MyModule_Receive` function:</span></span>

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

1. <span data-ttu-id="dfe90-155">Uppdatera hello `config.json` filen toospecify hello `workspace` mapp på din dator och hello distribution värdsökvägen på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="dfe90-155">Update hello `config.json` file toospecify hello `workspace` folder on your host computer and hello deployment path on Intel NUC.</span></span> <span data-ttu-id="dfe90-156">Under kompilering, hello filer i hello `workspace` mappen kommer att vara överförda toohello sökvägen.</span><span class="sxs-lookup"><span data-stu-id="dfe90-156">During compiling, hello files in hello `workspace` folder will be transferred toohello deployment path.</span></span>

   1. <span data-ttu-id="dfe90-157">Öppna hello `config.json` filen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="dfe90-157">Open hello `config.json` file by running hello following command:</span></span>

      ```bash
      code config.json
      ```

   1. <span data-ttu-id="dfe90-158">Uppdatera `config.json` med hello följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="dfe90-158">Update `config.json` with hello following configuration:</span></span>

      ```json
      "workspace": "./module/my_module",
      "deploy_path": "module/my_module"
      ```

      ![config_json](media/iot-hub-gateway-kit-lessons/lesson5/config_json.png)

1. <span data-ttu-id="dfe90-160">Kompilera hello modulen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="dfe90-160">Compile hello module by running hello following command:</span></span>

   ```bash
   gulp compile
   ```

   <span data-ttu-id="dfe90-161">hello kommandot överför hello källa kod tooIntel NUC och kör `build.sh` toocompile hello modulen.</span><span class="sxs-lookup"><span data-stu-id="dfe90-161">hello command transfers hello source code tooIntel NUC and runs `build.sh` toocompile hello module.</span></span>

## <a name="add-hello-module-toohello-helloworld-sample-app-and-run-hello-app-on-intel-nuc"></a><span data-ttu-id="dfe90-162">Lägg till hello modulen toohello hello_world sample-appen och kör hello app på Intel NUC</span><span class="sxs-lookup"><span data-stu-id="dfe90-162">Add hello module toohello hello_world sample app and run hello app on Intel NUC</span></span>

<span data-ttu-id="dfe90-163">tooperform detta uppgift, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="dfe90-163">tooperform this task, follow these steps:</span></span>

1. <span data-ttu-id="dfe90-164">Lista alla hello tillgängliga modulen binärfiler (.so-filer) på Intel NUC genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="dfe90-164">List all hello available module binaries (.so files) on Intel NUC by running hello following command:</span></span>

   ```bash
   gulp modules --list
   ```

   <span data-ttu-id="dfe90-165">hello binär sökväg för `my_module` som du kompilerat bör nu visas enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="dfe90-165">hello binary path of `my_module` that you compiled should be listed as below:</span></span>

   ```path
   /root/gateway_sample/module/my_module/build/libmy_module.so
   ```

   <span data-ttu-id="dfe90-166">Om du ändrar hello inloggningen Standardanvändarnamnet i `config-gateway.json`, hello binär sökväg startar med `home/<your username>` i stället för `root`.</span><span class="sxs-lookup"><span data-stu-id="dfe90-166">If you change hello default login username in `config-gateway.json`, hello binary path will start with `home/<your username>` instead of `root`.</span></span>

1. <span data-ttu-id="dfe90-167">Lägg till `my_module` toohello `hello_world` exempelapp:</span><span class="sxs-lookup"><span data-stu-id="dfe90-167">Add `my_module` toohello `hello_world` sample app:</span></span>

   1. <span data-ttu-id="dfe90-168">Öppna hello `hello_world.json` filen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="dfe90-168">Open hello `hello_world.json` file by running hello following command:</span></span>

      ```bash
      code sample/hello_world/src/hello_world.json
      ```

   1. <span data-ttu-id="dfe90-169">Lägg till följande kod toohello hello `modules` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="dfe90-169">Add hello following code toohello `modules` section:</span></span>

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

      <span data-ttu-id="dfe90-170">Hej värdet för `module.path` ska vara `/root/gateway_sample/module/my_module/build/libmy_module.so`.</span><span class="sxs-lookup"><span data-stu-id="dfe90-170">hello value of `module.path` should be `/root/gateway_sample/module/my_module/build/libmy_module.so`.</span></span> <span data-ttu-id="dfe90-171">hello koden deklarerar `my_module` toobe som är associerade med hello gateway samt hello plats hello modulen binära anges i `module.path`.</span><span class="sxs-lookup"><span data-stu-id="dfe90-171">hello code declares `my_module` toobe associated with hello gateway as well as hello location of hello module binary specified in `module.path`.</span></span>
   1. <span data-ttu-id="dfe90-172">Lägg till följande kod toohello hello `links` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="dfe90-172">Add hello following code toohello `links` section:</span></span>

      ```json
      {
        "source": "hello_world",
        "sink": "my_module"
      }
      ```

      <span data-ttu-id="dfe90-173">Den här koden anger att meddelanden överförs från hello `hello_world` modul för`my_module`.</span><span class="sxs-lookup"><span data-stu-id="dfe90-173">This code specifies that messages are transferred from hello `hello_world` module too`my_module`.</span></span>

      ![hello_world_json](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_json.png)

1. <span data-ttu-id="dfe90-175">Kör hello `hello_world` sample-appen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="dfe90-175">Run hello `hello_world` sample app by running hello following command:</span></span>

   ```bash
   gulp run --config sample/hello_world/src/hello_world.json
   ```

   <span data-ttu-id="dfe90-176">Hej `--config` parametern tvingar hello `run-hello-world.js` skript toorun med hjälp av hello `hello_world.json` fil.</span><span class="sxs-lookup"><span data-stu-id="dfe90-176">hello `--config` parameter forces hello `run-hello-world.js` script toorun by using hello `hello_world.json` file.</span></span>

   ![hello_world_new](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_new.png)

<span data-ttu-id="dfe90-178">Grattis!</span><span class="sxs-lookup"><span data-stu-id="dfe90-178">Congratulations.</span></span> <span data-ttu-id="dfe90-179">Du kan nu se hello beteendet för den här nya modulen, den bara skrivs ut ”hello world”-meddelanden med en tidsstämpel, ett annat slutresultat hello ursprungliga ”hello_world”-modul.</span><span class="sxs-lookup"><span data-stu-id="dfe90-179">You can now see hello behavior of this new module, it simply prints out "hello world" messages with a timestamp, a different result from hello original "hello_world" module.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dfe90-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dfe90-180">Next Steps</span></span>

<span data-ttu-id="dfe90-181">Du har skapat en ny modul, lade till den toohello hello_world exemplet och get hello exempel app toorun med nya hello-modulen på din gateway.</span><span class="sxs-lookup"><span data-stu-id="dfe90-181">You’ve created a new module, added it toohello hello_world sample, and get hello sample app toorun with hello new module on your gateway.</span></span> <span data-ttu-id="dfe90-182">Om du vill toolearn mer om Azure IoT gateway moduler, hittar du mer modulen exempel här: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).</span><span class="sxs-lookup"><span data-stu-id="dfe90-182">If you want toolearn more about Azure IoT gateway modules, you can find more module samples here: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).</span></span>

<!-- Images and links -->

[config_ble]: iot-hub-gateway-kit-c-lesson3-configure-ble-app.md
[setup_nuc]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
