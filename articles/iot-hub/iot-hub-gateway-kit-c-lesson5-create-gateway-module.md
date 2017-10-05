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
# <a name="lesson-5-create-your-first-azure-iot-gateway-module"></a><span data-ttu-id="54c35-103">Lektionen 5: Skapa ditt första Azure IoT Gateway-modul</span><span class="sxs-lookup"><span data-stu-id="54c35-103">Lesson 5: Create your first Azure IoT Gateway module</span></span>
<span data-ttu-id="54c35-104">Med Azure IoT gräns kan du skapa moduler som skrivits i Java, .NET eller Node.js, vägleder den här självstudiekursen dig genom stegen för att skapa en modul i C.</span><span class="sxs-lookup"><span data-stu-id="54c35-104">While Azure IoT Edge allows you to build modules written in Java, .NET, or Node.js, this tutorial walks you through the steps for building a module in C.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="54c35-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="54c35-105">What you will do</span></span>

- <span data-ttu-id="54c35-106">Kompilera och köra hello_world sample-appen på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="54c35-106">Compile and run the hello_world sample app on Intel NUC.</span></span>
- <span data-ttu-id="54c35-107">Skapa en modul och kompilera på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="54c35-107">Create a module and compile it on Intel NUC.</span></span>
- <span data-ttu-id="54c35-108">Lägg till ny modul hello_world sample-appen och sedan köra exemplet på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="54c35-108">Add the new module to the hello_world sample app and then run the sample on Intel NUC.</span></span> <span data-ttu-id="54c35-109">Ny modul skriver ut ”hello_world” meddelanden med en tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="54c35-109">The new module prints out "hello_world" messages with a timestamp.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="54c35-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="54c35-110">What you will learn</span></span>

- <span data-ttu-id="54c35-111">Så att kompilera och köra en exempelapp på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="54c35-111">How to compile and run a sample app on Intel NUC.</span></span>
- <span data-ttu-id="54c35-112">Så här skapar du en modul.</span><span class="sxs-lookup"><span data-stu-id="54c35-112">How to create a module.</span></span>
- <span data-ttu-id="54c35-113">Hur du lägger till modulen sample-appen.</span><span class="sxs-lookup"><span data-stu-id="54c35-113">How to add module to a sample app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="54c35-114">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="54c35-114">What you need</span></span>

<span data-ttu-id="54c35-115">Azure IoT-kant som har installerats på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="54c35-115">Azure IoT Edge that has been installed on your host computer.</span></span>

## <a name="folder-structure"></a><span data-ttu-id="54c35-116">Mappstruktur</span><span class="sxs-lookup"><span data-stu-id="54c35-116">Folder structure</span></span>

<span data-ttu-id="54c35-117">Det finns i undermappen lektionen 5 i exempelkoden som du har klonat i lektionen 1 en `module` mapp och en `sample` mapp.</span><span class="sxs-lookup"><span data-stu-id="54c35-117">In the Lesson 5 subfolder of the sample code which you cloned in lesson 1, there is a `module` folder and a `sample` folder.</span></span>

![my_module](media/iot-hub-gateway-kit-lessons/lesson5/my_module.png)

- <span data-ttu-id="54c35-119">Den `module/my_module` mappen innehåller källkoden och skript för att skapa modulen.</span><span class="sxs-lookup"><span data-stu-id="54c35-119">The `module/my_module` folder contains the source code and script to build the module.</span></span>
- <span data-ttu-id="54c35-120">Den `sample` mappen innehåller källkoden och skript för att skapa sample-appen.</span><span class="sxs-lookup"><span data-stu-id="54c35-120">The `sample` folder contains the source code and script to build the sample app.</span></span>

## <a name="compile-and-run-the-helloworld-sample-app-on-intel-nuc"></a><span data-ttu-id="54c35-121">Kompilera och köra hello_world sample-appen på Intel NUC</span><span class="sxs-lookup"><span data-stu-id="54c35-121">Compile and run the hello_world sample app on Intel NUC</span></span>

<span data-ttu-id="54c35-122">Den `hello_world` exemplet skapar gateway baserat på den `hello_world.json` -fil som anger de två fördefinierade moduler som är associerat med appen.</span><span class="sxs-lookup"><span data-stu-id="54c35-122">The `hello_world` sample creates a gateway based on the `hello_world.json` file which specifies the two predefined modules associated with the app.</span></span> <span data-ttu-id="54c35-123">Gatewayen loggas ett meddelande om ”hello world” i en fil var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="54c35-123">The gateway logs a "hello world" message to a file every 5 seconds.</span></span> <span data-ttu-id="54c35-124">I det här avsnittet kan du kompilera och köra den `hello_world` app med den standard-modulen.</span><span class="sxs-lookup"><span data-stu-id="54c35-124">In this section, you compile and run the `hello_world` app with its default module.</span></span>

<span data-ttu-id="54c35-125">Att kompilera och köra den `hello_world` appen, Följ dessa steg på värddatorn:</span><span class="sxs-lookup"><span data-stu-id="54c35-125">To compile and run the `hello_world` app, follow these steps on your host computer:</span></span>

1. <span data-ttu-id="54c35-126">Initiera konfigurationsfilerna genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="54c35-126">Initialize the configuration files by running the following commands:</span></span>

   ```bash
   cd iot-hub-c-intel-nuc-gateway-getting-started
   cd Lesson5
   npm install
   gulp init
   ```

1. <span data-ttu-id="54c35-127">Uppdatera konfigurationsfilen gateway med Intel NUC MAC-adress.</span><span class="sxs-lookup"><span data-stu-id="54c35-127">Update the gateway configuration file with the MAC address of Intel NUC.</span></span> <span data-ttu-id="54c35-128">Hoppa över det här steget om du har gått igenom lektionen till [konfigurera och köra ett exempelprogram TIVERA][config_ble].</span><span class="sxs-lookup"><span data-stu-id="54c35-128">Skip this step if you have gone through the lesson to [configure and run a BLE sample application][config_ble].</span></span>

   1. <span data-ttu-id="54c35-129">Öppna konfigurationsfilen gateway genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="54c35-129">Open the gateway configuration file by running the following command:</span></span>

      ```bash
      # For Windows command prompt
      code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json

      # For MacOS or Ubuntu
      code ~/.iot-hub-getting-started/config-gateway.json
      ```

   1. <span data-ttu-id="54c35-130">Uppdatering av gateway-MAC-adressen när du [konfigurera Intel NUC som en gateway för IoT][setup_nuc], och sedan spara filen.</span><span class="sxs-lookup"><span data-stu-id="54c35-130">Update the gateway's MAC address when you [set up Intel NUC as a IoT gateway][setup_nuc], and then save the file.</span></span>

1. <span data-ttu-id="54c35-131">Kompilera källkoden exemplet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="54c35-131">Compile the sample source code by running the following command:</span></span>

   ```bash
   gulp compile
   ```

   <span data-ttu-id="54c35-132">Kommandot överför exempelkod för källan till Intel NUC och kör `build.sh` att kompilera den.</span><span class="sxs-lookup"><span data-stu-id="54c35-132">The command transfers the sample source code to Intel NUC and runs `build.sh` to compile it.</span></span>

1. <span data-ttu-id="54c35-133">Kör den `hello_world` appen på Intel NUC genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="54c35-133">Run the `hello_world` app on Intel NUC by running the following command:</span></span>

   ```bash
   gulp run
   ```

   <span data-ttu-id="54c35-134">Kommandot körs `../Tools/run-hello-world.js` som anges i `config.json` starta sample-appen på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="54c35-134">The command runs `../Tools/run-hello-world.js` that is specified in `config.json` to start the sample app on Intel NUC.</span></span>

   ![run_sample](media/iot-hub-gateway-kit-lessons/lesson5/run_sample.png)

## <a name="create-a-new-module-and-compile-it-on-intel-nuc"></a><span data-ttu-id="54c35-136">Skapa en ny modul och kompilera på Intel NUC</span><span class="sxs-lookup"><span data-stu-id="54c35-136">Create a new module and compile it on Intel NUC</span></span>

<span data-ttu-id="54c35-137">Stegen nedan hjälper dig att skapa en ny modul och kompilera på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="54c35-137">The steps below walk you through creating a new module and compile it on Intel NUC.</span></span> <span data-ttu-id="54c35-138">Modulen skriver ut meddelanden med en tidsstämpel vid mottagning av dem.</span><span class="sxs-lookup"><span data-stu-id="54c35-138">The module prints out messages with a timestamp upon receiving them.</span></span> <span data-ttu-id="54c35-139">I det här avsnittet skapar du din första anpassade gateway-modulen.</span><span class="sxs-lookup"><span data-stu-id="54c35-139">You will create your first customized gateway module in this section.</span></span>

<span data-ttu-id="54c35-140">Azure IoT kant modul måste implementera följande gränssnitt:</span><span class="sxs-lookup"><span data-stu-id="54c35-140">Any Azure IoT Edge module must implement the following interfaces:</span></span>

   ```C
   pfModule_ParseConfigurationFromJson Module_ParseConfigurationFromJson
   pfModule_FreeConfiguration Module_FreeConfiguration
   pfModule_Create Module_Create
   pfModule_Destroy Module_Destroy
   pfModule_Receive Module_Receive
   ```

<span data-ttu-id="54c35-141">Alternativt kan du implementera följande gränssnitt:</span><span class="sxs-lookup"><span data-stu-id="54c35-141">You can optionally implement the following interface:</span></span>

   ```C
   pfModule_Start Module_Start
   ```

<span data-ttu-id="54c35-142">Följande diagram visar viktiga tillstånd sökvägar för en modul.</span><span class="sxs-lookup"><span data-stu-id="54c35-142">The following diagram shows the important state paths of a module.</span></span> <span data-ttu-id="54c35-143">Kvadratisk rektanglar representerar metoder som du implementerar för att utföra åtgärder när modulen flyttar emellan.</span><span class="sxs-lookup"><span data-stu-id="54c35-143">The square rectangles represent methods you implement to perform operations when the module moves between states.</span></span> <span data-ttu-id="54c35-144">Ovalerna är större tillstånd modulen kan vara i.</span><span class="sxs-lookup"><span data-stu-id="54c35-144">The ovals are major states the module can be in.</span></span>

![state_path](media/iot-hub-gateway-kit-lessons/lesson5/state_path.png)

<span data-ttu-id="54c35-146">Nu ska vi skapa en modul som baseras på mallen:</span><span class="sxs-lookup"><span data-stu-id="54c35-146">Now let’s create a module based on the template:</span></span>

1. <span data-ttu-id="54c35-147">Öppna mallmappen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="54c35-147">Open the template folder by running the following command:</span></span>

   ```bash
   code module/my_module
   ```

   ![code_module](media/iot-hub-gateway-kit-lessons/lesson5/code_module.png)

   - <span data-ttu-id="54c35-149">`src/my_module.c`fungerar som en mall som underlättar skapandet av en modul.</span><span class="sxs-lookup"><span data-stu-id="54c35-149">`src/my_module.c` serves as a template that facilitates the creation of a module.</span></span> <span data-ttu-id="54c35-150">Mallen deklarerar gränssnitten.</span><span class="sxs-lookup"><span data-stu-id="54c35-150">The template declares the interfaces.</span></span> <span data-ttu-id="54c35-151">Allt du behöver göra är att lägga till kod till den `MyModule_Receive` funktion.</span><span class="sxs-lookup"><span data-stu-id="54c35-151">All you need to do is to add logic to the `MyModule_Receive` function.</span></span>
   - <span data-ttu-id="54c35-152">`build.sh`är build-skript för att kompilera modulen på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="54c35-152">`build.sh` is the build script to compile the module on Intel NUC.</span></span>
1. <span data-ttu-id="54c35-153">Öppna den `src/my_module.c` filen och innehåller två rubrikfiler:</span><span class="sxs-lookup"><span data-stu-id="54c35-153">Open the `src/my_module.c` file and include two header files:</span></span>

   ```C
   #include <stdio.h>
   #include "azure_c_shared_utility/xlogging.h"
   ```

1. <span data-ttu-id="54c35-154">Lägg till följande kod i den `MyModule_Receive` funktionen:</span><span class="sxs-lookup"><span data-stu-id="54c35-154">Add the following code to the `MyModule_Receive` function:</span></span>

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

1. <span data-ttu-id="54c35-155">Uppdatering av `config.json` att ange den `workspace` mapp på värddatorn och sökvägen för distribution på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="54c35-155">Update the `config.json` file to specify the `workspace` folder on your host computer and the deployment path on Intel NUC.</span></span> <span data-ttu-id="54c35-156">Under kompilering av filerna i den `workspace` mappen kommer att överföras till sökvägen för distribution.</span><span class="sxs-lookup"><span data-stu-id="54c35-156">During compiling, the files in the `workspace` folder will be transferred to the deployment path.</span></span>

   1. <span data-ttu-id="54c35-157">Öppna den `config.json` filen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="54c35-157">Open the `config.json` file by running the following command:</span></span>

      ```bash
      code config.json
      ```

   1. <span data-ttu-id="54c35-158">Uppdatera `config.json` med följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="54c35-158">Update `config.json` with the following configuration:</span></span>

      ```json
      "workspace": "./module/my_module",
      "deploy_path": "module/my_module"
      ```

      ![config_json](media/iot-hub-gateway-kit-lessons/lesson5/config_json.png)

1. <span data-ttu-id="54c35-160">Kompilera modulen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="54c35-160">Compile the module by running the following command:</span></span>

   ```bash
   gulp compile
   ```

   <span data-ttu-id="54c35-161">Kommandot överför källkoden till Intel NUC och kör `build.sh` att kompilera modulen.</span><span class="sxs-lookup"><span data-stu-id="54c35-161">The command transfers the source code to Intel NUC and runs `build.sh` to compile the module.</span></span>

## <a name="add-the-module-to-the-helloworld-sample-app-and-run-the-app-on-intel-nuc"></a><span data-ttu-id="54c35-162">Lägga till modulen hello_world sample-appen och kör appen på Intel NUC</span><span class="sxs-lookup"><span data-stu-id="54c35-162">Add the module to the hello_world sample app and run the app on Intel NUC</span></span>

<span data-ttu-id="54c35-163">Följ dessa steg om du vill utföra den här uppgiften:</span><span class="sxs-lookup"><span data-stu-id="54c35-163">To perform this task, follow these steps:</span></span>

1. <span data-ttu-id="54c35-164">Lista alla tillgängliga modulen binärfilerna (.so-filer) på Intel NUC genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="54c35-164">List all the available module binaries (.so files) on Intel NUC by running the following command:</span></span>

   ```bash
   gulp modules --list
   ```

   <span data-ttu-id="54c35-165">Binär sökväg för `my_module` som du kompilerat bör nu visas enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="54c35-165">The binary path of `my_module` that you compiled should be listed as below:</span></span>

   ```path
   /root/gateway_sample/module/my_module/build/libmy_module.so
   ```

   <span data-ttu-id="54c35-166">Om du ändrar inloggningen Standardanvändarnamnet i `config-gateway.json`, binär sökväg startar med `home/<your username>` i stället för `root`.</span><span class="sxs-lookup"><span data-stu-id="54c35-166">If you change the default login username in `config-gateway.json`, the binary path will start with `home/<your username>` instead of `root`.</span></span>

1. <span data-ttu-id="54c35-167">Lägg till `my_module` till den `hello_world` sample-appen:</span><span class="sxs-lookup"><span data-stu-id="54c35-167">Add `my_module` to the `hello_world` sample app:</span></span>

   1. <span data-ttu-id="54c35-168">Öppna den `hello_world.json` filen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="54c35-168">Open the `hello_world.json` file by running the following command:</span></span>

      ```bash
      code sample/hello_world/src/hello_world.json
      ```

   1. <span data-ttu-id="54c35-169">Lägg till följande kod i den `modules` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="54c35-169">Add the following code to the `modules` section:</span></span>

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

      <span data-ttu-id="54c35-170">Värdet för `module.path` ska vara `/root/gateway_sample/module/my_module/build/libmy_module.so`.</span><span class="sxs-lookup"><span data-stu-id="54c35-170">The value of `module.path` should be `/root/gateway_sample/module/my_module/build/libmy_module.so`.</span></span> <span data-ttu-id="54c35-171">Koden deklarerar `my_module` som ska associeras med gatewayen som platsen för den binära modul som anges i `module.path`.</span><span class="sxs-lookup"><span data-stu-id="54c35-171">The code declares `my_module` to be associated with the gateway as well as the location of the module binary specified in `module.path`.</span></span>
   1. <span data-ttu-id="54c35-172">Lägg till följande kod i den `links` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="54c35-172">Add the following code to the `links` section:</span></span>

      ```json
      {
        "source": "hello_world",
        "sink": "my_module"
      }
      ```

      <span data-ttu-id="54c35-173">Den här koden anger att meddelanden överförs från den `hello_world` modulen `my_module`.</span><span class="sxs-lookup"><span data-stu-id="54c35-173">This code specifies that messages are transferred from the `hello_world` module to `my_module`.</span></span>

      ![hello_world_json](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_json.png)

1. <span data-ttu-id="54c35-175">Kör den `hello_world` sample-appen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="54c35-175">Run the `hello_world` sample app by running the following command:</span></span>

   ```bash
   gulp run --config sample/hello_world/src/hello_world.json
   ```

   <span data-ttu-id="54c35-176">Den `--config` parametern tvingar den `run-hello-world.js` skriptet ska köras med hjälp av den `hello_world.json` filen.</span><span class="sxs-lookup"><span data-stu-id="54c35-176">The `--config` parameter forces the `run-hello-world.js` script to run by using the `hello_world.json` file.</span></span>

   ![hello_world_new](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_new.png)

<span data-ttu-id="54c35-178">Grattis.</span><span class="sxs-lookup"><span data-stu-id="54c35-178">Congratulations.</span></span> <span data-ttu-id="54c35-179">Du kan nu se beteendet för den här nya modulen, den bara skrivs ut ”hello world”-meddelanden med en tidsstämpel, ett annat resultat från den ursprungliga modulen ”hello_world”.</span><span class="sxs-lookup"><span data-stu-id="54c35-179">You can now see the behavior of this new module, it simply prints out "hello world" messages with a timestamp, a different result from the original "hello_world" module.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54c35-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="54c35-180">Next Steps</span></span>

<span data-ttu-id="54c35-181">Du har skapat en ny modul, lägga till exempel hello_world och get sample-appen ska köras med den nya modulen på din gateway.</span><span class="sxs-lookup"><span data-stu-id="54c35-181">You’ve created a new module, added it to the hello_world sample, and get the sample app to run with the new module on your gateway.</span></span> <span data-ttu-id="54c35-182">Om du vill veta mer om Azure IoT gateway moduler hittar du mer modulen exempel här: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).</span><span class="sxs-lookup"><span data-stu-id="54c35-182">If you want to learn more about Azure IoT gateway modules, you can find more module samples here: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).</span></span>

<!-- Images and links -->

[config_ble]: iot-hub-gateway-kit-c-lesson3-configure-ble-app.md
[setup_nuc]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
