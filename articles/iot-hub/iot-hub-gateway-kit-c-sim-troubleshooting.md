---
title: "Simulerade enhet & Azure IoT-Gateway - felsökning | Microsoft Docs"
description: "Felsökning för Intel NUC gateway"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT-problem, internet saker problem
ROBOTS: NOINDEX
ms.assetid: 3ee8f4b0-5799-40a3-8cf0-8d5aa44dbc2b
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: cc7add6273e66aadeca9a4915a44f5edf61a0a59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="217f4-104">Felsökning</span><span class="sxs-lookup"><span data-stu-id="217f4-104">Troubleshooting</span></span>

## <a name="hardware-issues"></a><span data-ttu-id="217f4-105">Problem med maskinvara</span><span class="sxs-lookup"><span data-stu-id="217f4-105">Hardware issues</span></span>

### <a name="ti-sensortag-cannot-be-connected"></a><span data-ttu-id="217f4-106">Det går inte att ansluta TI SensorTag</span><span class="sxs-lookup"><span data-stu-id="217f4-106">TI SensorTag cannot be connected</span></span>

<span data-ttu-id="217f4-107">tootroubleshoot SensorTag-anslutningsproblem, använda hello [SensorTag app](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).</span><span class="sxs-lookup"><span data-stu-id="217f4-107">tootroubleshoot SensorTag connectivity issues, use hello [SensorTag app](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).</span></span>

### <a name="have-an-issue-with-intel-nuc"></a><span data-ttu-id="217f4-108">Har ett problem med Intel NUC</span><span class="sxs-lookup"><span data-stu-id="217f4-108">Have an issue with Intel NUC</span></span>

<span data-ttu-id="217f4-109">tootroubleshoot startproblem finns för[Felsökning nr Start på Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).</span><span class="sxs-lookup"><span data-stu-id="217f4-109">tootroubleshoot boot issues, refer too[troubleshooting No Boot Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).</span></span>

<span data-ttu-id="217f4-110">problem med tootroubleshoot operativsystem, se för[felsökning av problem med operativsystemet på Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).</span><span class="sxs-lookup"><span data-stu-id="217f4-110">tootroubleshoot operating system issues, refer too[troubleshooting Operating System Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).</span></span>

<span data-ttu-id="217f4-111">tootroubleshoot andra frågor refererar för[blinkar och signal koder för Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).</span><span class="sxs-lookup"><span data-stu-id="217f4-111">tootroubleshoot other issues, refer too[Blink Codes and Beep Codes for Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="217f4-112">Problem med node.js-paket</span><span class="sxs-lookup"><span data-stu-id="217f4-112">Node.js package issues</span></span>

### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="217f4-113">Inget svar under gulp uppgifter</span><span class="sxs-lookup"><span data-stu-id="217f4-113">No response during gulp tasks</span></span>

<span data-ttu-id="217f4-114">Om du får problem körs gulp uppgifter du kan lägga till hello `--verbose` alternativet för felsökning.</span><span class="sxs-lookup"><span data-stu-id="217f4-114">If you encounter problems in running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="217f4-115">Försök tooterminate aktuella gulp uppgifter med hjälp av `Ctrl + C`, och sedan kör hello följande kommando i konsolen för fönstret toosee felsökningsmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="217f4-115">Try tooterminate current gulp tasks by using `Ctrl + C`, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="217f4-116">Du kan visa detaljerade felmeddelanden i konsolens utdata.</span><span class="sxs-lookup"><span data-stu-id="217f4-116">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="217f4-117">Problem vid identifiering av enheter</span><span class="sxs-lookup"><span data-stu-id="217f4-117">Device discovery issues</span></span>

<span data-ttu-id="217f4-118">Felsökning av vanliga problem med hello `discover-sensortag` kommandot, kontrollera hello [wiki-sida](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).</span><span class="sxs-lookup"><span data-stu-id="217f4-118">For help in troubleshooting common problems with hello `discover-sensortag` command, check hello [wiki page](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="217f4-119">npm-problem</span><span class="sxs-lookup"><span data-stu-id="217f4-119">npm issues</span></span>

<span data-ttu-id="217f4-120">Försök tooupdate npm-paketet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="217f4-120">Try tooupdate your npm package by running hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="217f4-121">Om hello problemet fortfarande kvarstår lämna kommentarer hello slutet av den här artikeln eller skapa ett GitHub-problem i våra [exempel databasen](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).</span><span class="sxs-lookup"><span data-stu-id="217f4-121">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="217f4-122">Fjärrfelsökning</span><span class="sxs-lookup"><span data-stu-id="217f4-122">Remote Debugging</span></span>
> <span data-ttu-id="217f4-123">Nedan instruktioner är avsedda för att felsöka node.js-skript som används i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="217f4-123">Below instructions are meant for debugging node.js scripts used in this tutorial.</span></span>
### <a name="run-hello-sample-application-in-debug-mode"></a><span data-ttu-id="217f4-124">Köra hello exempelprogrammet i felsökningsläge</span><span class="sxs-lookup"><span data-stu-id="217f4-124">Run hello sample application in debug mode</span></span>

<span data-ttu-id="217f4-125">Kör exempelprogrammet hello i felsökningsläge genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="217f4-125">Run hello sample application in debug mode by running hello following command:</span></span>

```bash
gulp run --debug
```

<span data-ttu-id="217f4-126">När hello debug-motorn är klar, bör du se `Debugger listening on port 5858` i hello konsolens utdata.</span><span class="sxs-lookup"><span data-stu-id="217f4-126">When hello debug engine is ready, you should see `Debugger listening on port 5858` in hello console output.</span></span>

### <a name="configure-visual-studio-code-tooconnect-toohello-remote-device"></a><span data-ttu-id="217f4-127">Konfigurera Visual Studio Code tooconnect toohello fjärranslutna enheter</span><span class="sxs-lookup"><span data-stu-id="217f4-127">Configure Visual Studio Code tooconnect toohello remote device</span></span>

1. <span data-ttu-id="217f4-128">Öppna hello **felsöka** panelen för hello vänster sida.</span><span class="sxs-lookup"><span data-stu-id="217f4-128">Open hello **Debug** panel on hello left side.</span></span>
2. <span data-ttu-id="217f4-129">Klicka på hello grön **Start Debugging** (F5)-knappen.</span><span class="sxs-lookup"><span data-stu-id="217f4-129">Click hello green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="217f4-130">Visual Studio Code öppnar en `launch.json` fil.</span><span class="sxs-lookup"><span data-stu-id="217f4-130">Visual Studio Code opens a `launch.json` file.</span></span>
3. <span data-ttu-id="217f4-131">Uppdatera hello `launch.json` fil med hello följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="217f4-131">Update hello `launch.json` file with hello following content.</span></span> <span data-ttu-id="217f4-132">Ersätt `[device hostname or IP address]` med hello faktiska IP-adressen eller värdnamnet enhetsnamn.</span><span class="sxs-lookup"><span data-stu-id="217f4-132">Replace `[device hostname or IP address]` with hello actual device IP address or host name.</span></span>

   ``` json
   {
     "version": "0.2.0",
     "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": "~/ble_sample"
        }
     ]
   }
   ```

![Fjärrkonfiguration felsökning](./media/iot-hub-gateway-kit-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-toohello-remote-application"></a><span data-ttu-id="217f4-134">Koppla toohello fjärrprogram</span><span class="sxs-lookup"><span data-stu-id="217f4-134">Attach toohello remote application</span></span>

<span data-ttu-id="217f4-135">Klicka på hello grön **Start Debugging** (F5) knappen toostart felsökning.</span><span class="sxs-lookup"><span data-stu-id="217f4-135">Click hello green **Start Debugging** (F5) button toostart debugging.</span></span>

<span data-ttu-id="217f4-136">Läs [JavaScript i VS kod](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn mer om hello felsökare.</span><span class="sxs-lookup"><span data-stu-id="217f4-136">Read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn more about hello debugger.</span></span>

![Felsökning Bell-exempel](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="217f4-138">Azure CLI-problem</span><span class="sxs-lookup"><span data-stu-id="217f4-138">Azure CLI issues</span></span>

<span data-ttu-id="217f4-139">hello Azure-kommandoradsgränssnittet (Azure CLI) är en förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="217f4-139">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="217f4-140">tooseek lösningar, kan du använda hello [Preview installera guiden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span><span class="sxs-lookup"><span data-stu-id="217f4-140">tooseek solutions, you can use hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span></span>

<span data-ttu-id="217f4-141">Om det uppstår några buggar med hello verktyget filen ett [problemet](https://github.com/Azure/azure-cli/issues) i hello **problem** avsnitt i GitHub-repo-hello.</span><span class="sxs-lookup"><span data-stu-id="217f4-141">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="217f4-142">Hjälp vid felsökning av vanliga problem, kontrollera hello [viktigt](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="217f4-142">For help in troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="217f4-143">Om du uppfyller ”det gick inte att hitta en version som uppfyller krav på hello”, ange kommando hello kör följande tooupgrade pip toolastest version.</span><span class="sxs-lookup"><span data-stu-id="217f4-143">If you meet "Could not find a version that satisfies hello requirement", please run hello following command tooupgrade pip toolastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="217f4-144">Problem med installation av Python</span><span class="sxs-lookup"><span data-stu-id="217f4-144">Python installation issues</span></span>

### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="217f4-145">Äldre installationsproblem (macOS)</span><span class="sxs-lookup"><span data-stu-id="217f4-145">Legacy installation issues (macOS)</span></span>

<span data-ttu-id="217f4-146">När du installerar pip, en behörighet felmeddelande när äldre paket installeras med **su** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="217f4-146">When you're installing pip, a permission error is thrown when older packages are installed with **su** permissions.</span></span> <span data-ttu-id="217f4-147">Detta inträffar eftersom en tidigare installation av Python med brew (macOS) inte har avinstallerats helt.</span><span class="sxs-lookup"><span data-stu-id="217f4-147">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="217f4-148">Vissa pip-paket från en tidigare installation skapades rot, vilket gör att hello behörighetsfel.</span><span class="sxs-lookup"><span data-stu-id="217f4-148">Some pip packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="217f4-149">Hej lösningen är tooremove de paket som installerats av roten.</span><span class="sxs-lookup"><span data-stu-id="217f4-149">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="217f4-150">Använd följande steg toocomplete hello den här aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="217f4-150">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="217f4-151">Gå för`/usr/local/lib/python2.7/site-packages`</span><span class="sxs-lookup"><span data-stu-id="217f4-151">Go too`/usr/local/lib/python2.7/site-packages`</span></span>
2. <span data-ttu-id="217f4-152">Lista över paket som skapats av roten:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="217f4-152">List packages created by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="217f4-153">Avinstallera paket från steg 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="217f4-153">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="217f4-154">Installera om Python.</span><span class="sxs-lookup"><span data-stu-id="217f4-154">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="217f4-155">Azure IoT-hubb-problem</span><span class="sxs-lookup"><span data-stu-id="217f4-155">Azure IoT Hub issues</span></span>

<span data-ttu-id="217f4-156">Om din Azure IoT-hubb med hello Azure CLI har etablerats, och du behöver en verktyget toomanage hello enheter som ansluter tooyour IoT-hubb, kan du försöka hello följande verktyg.</span><span class="sxs-lookup"><span data-stu-id="217f4-156">If you've successfully provisioned your Azure IoT hub with hello Azure CLI, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools.</span></span>

### <a name="device-explorer"></a><span data-ttu-id="217f4-157">Enheten Explorer</span><span class="sxs-lookup"><span data-stu-id="217f4-157">Device Explorer</span></span>

<span data-ttu-id="217f4-158">[Enheten Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) körs på den lokala Windows-datorn och ansluter tooyour IoT-hubb i Azure.</span><span class="sxs-lookup"><span data-stu-id="217f4-158">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="217f4-159">Den kommunicerar med hello följande [IoT-hubbslutpunkter](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):</span><span class="sxs-lookup"><span data-stu-id="217f4-159">It communicates with hello following [IoT Hub endpoints](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):</span></span>

- <span data-ttu-id="217f4-160">Enheten identity management tooprovision och hantera enheter som registrerats med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="217f4-160">Device identity management tooprovision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="217f4-161">Ta emot enhet till moln så att du kan övervaka meddelanden som skickas från din enhet tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="217f4-161">Receive device-to-cloud so you can monitor messages sent from your device tooyour IoT hub.</span></span>
- <span data-ttu-id="217f4-162">Skicka moln till enhet så att du kan skicka meddelanden tooyour enheter från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="217f4-162">Send cloud-to-device so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="217f4-163">Konfigurera anslutningssträngen IoT-hubb i det här verktyget toouse dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="217f4-163">Configure your IoT hub connection string within this tool toouse all its capabilities.</span></span>

### <a name="iothub-explorer"></a><span data-ttu-id="217f4-164">iothub explorer</span><span class="sxs-lookup"><span data-stu-id="217f4-164">iothub-explorer</span></span>

<span data-ttu-id="217f4-165">[iothub explorer](https://github.com/Azure/iothub-explorer) är ett exempel flera plattformar CLI toomanage enhetsklienter.</span><span class="sxs-lookup"><span data-stu-id="217f4-165">[iothub-explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage device clients.</span></span> <span data-ttu-id="217f4-166">Du kan använda hello verktyget toomanage hello enheter i hello identitetsregistret, övervaka meddelanden från enhet till moln och skicka kommandon moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="217f4-166">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="217f4-167">tooinstall hello senaste () förhandsversion av hello iothub explorer verktyget kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="217f4-167">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="217f4-168">tooget mer hjälp om alla Hej iothub explorer kommandon och deras parametrar, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="217f4-168">tooget additional help about all hello iothub-explorer commands and their parameters, run hello following command:</span></span>

```bash
iothub-explorer help
```

### <a name="hello-azure-portal"></a><span data-ttu-id="217f4-169">hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="217f4-169">hello Azure portal</span></span>

<span data-ttu-id="217f4-170">En fullständig CLI-upplevelse hjälper dig att skapa och hantera alla dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="217f4-170">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="217f4-171">Du kan också toouse hello [Azure-portalen](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) toohelp etablera, hantera och felsöka Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="217f4-171">You might also want toouse hello [Azure portal](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="217f4-172">Azure Storage-problem</span><span class="sxs-lookup"><span data-stu-id="217f4-172">Azure Storage issues</span></span>

<span data-ttu-id="217f4-173">[Microsoft Azure Lagringsutforskaren (förhandsversion)](http://storageexplorer.com/) är en fristående app från Microsoft som du kan använda toowork med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="217f4-173">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com/) is a standalone app from Microsoft that you can use toowork with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="217f4-174">Med det här verktyget kan du ansluta tooyour tabell och se hello data i den.</span><span class="sxs-lookup"><span data-stu-id="217f4-174">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="217f4-175">Du kan använda det här verktyget tootroubleshoot Azure Storage-problem.</span><span class="sxs-lookup"><span data-stu-id="217f4-175">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>
