---
title: "Connect Raspberry PI (C) tooAzure IoT - felsökning | Microsoft Docs"
description: "Felsökning för hello hallon Pi Node.js-upplevelse"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT-problem, internet saker problem
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 22cf50dc-8206-42a2-a1fc-f75fa85135fa
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8f0807104550e8e53a132f7741564b33f1db17ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="17f1d-104">Felsökning</span><span class="sxs-lookup"><span data-stu-id="17f1d-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="17f1d-105">Problem med maskinvara</span><span class="sxs-lookup"><span data-stu-id="17f1d-105">Hardware issues</span></span>
### <a name="hello-application-runs-well-but-hello-led-is-not-blinking"></a><span data-ttu-id="17f1d-106">hello-program körs och men hello Indikator blinkar inte</span><span class="sxs-lookup"><span data-stu-id="17f1d-106">hello application runs well but hello LED is not blinking</span></span>
<span data-ttu-id="17f1d-107">Det här problemet är alltid relaterade toohardware krets anslutningen.</span><span class="sxs-lookup"><span data-stu-id="17f1d-107">This issue is always related toohardware circuit connectivity.</span></span> <span data-ttu-id="17f1d-108">Använd följande steg tooidentify problem hello:</span><span class="sxs-lookup"><span data-stu-id="17f1d-108">Use hello following steps tooidentify problems:</span></span>

1. <span data-ttu-id="17f1d-109">Kontrollera att du har valt rätt hello **GPIO** på din-kort.</span><span class="sxs-lookup"><span data-stu-id="17f1d-109">Check that you chose hello correct **GPIO** on your board.</span></span> <span data-ttu-id="17f1d-110">hello två portar bör vara **GPIO GND (PIN-kod 6)** och **GPIO 04 (PIN-kod 7)**.</span><span class="sxs-lookup"><span data-stu-id="17f1d-110">hello two ports should be **GPIO GND (Pin 6)** and **GPIO 04 (Pin 7)**.</span></span>
2. <span data-ttu-id="17f1d-111">Kontrollera att hello polaritet för din Indikator är korrekt.</span><span class="sxs-lookup"><span data-stu-id="17f1d-111">Check that hello polarity of your LED is correct.</span></span> <span data-ttu-id="17f1d-112">hello längre ben bör ange hello **positiva**, av PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="17f1d-112">hello longer leg should indicate hello **positive**, anode pin.</span></span>
3. <span data-ttu-id="17f1d-113">Använd hello **3, 3v fästa** och **jord Pin** på hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="17f1d-113">Use hello **3.3V Pin** and **GND Pin** on Raspberry Pi 3.</span></span> <span data-ttu-id="17f1d-114">Behandla Pi som hello DC-ström.</span><span class="sxs-lookup"><span data-stu-id="17f1d-114">Treat Pi as hello DC power.</span></span> <span data-ttu-id="17f1d-115">Kontrollera att hello Indikator fungerar bra.</span><span class="sxs-lookup"><span data-stu-id="17f1d-115">Check that hello LED works fine.</span></span>

![Specifikation för Indikator](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a><span data-ttu-id="17f1d-117">Andra problem med maskinvara</span><span class="sxs-lookup"><span data-stu-id="17f1d-117">Other hardware issues</span></span>
<span data-ttu-id="17f1d-118">Information om hur du löser vanliga problem på hallon Pi 3 finns hello [officiella felsökning sidan](http://elinux.org/R-Pi_Troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="17f1d-118">For information about solving common problems on Raspberry Pi 3, see hello [official troubleshooting page](http://elinux.org/R-Pi_Troubleshooting).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="17f1d-119">Problem med node.js-paket</span><span class="sxs-lookup"><span data-stu-id="17f1d-119">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="17f1d-120">Inget svar under gulp uppgifter</span><span class="sxs-lookup"><span data-stu-id="17f1d-120">No response during gulp tasks</span></span>
<span data-ttu-id="17f1d-121">Om du får problem körs gulp uppgifter du kan lägga till hello `--verbose` alternativet för felsökning.</span><span class="sxs-lookup"><span data-stu-id="17f1d-121">If you encounter problems in running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="17f1d-122">Försök tooterminate aktuella gulp uppgifter med hjälp av Ctrl + C och kör sedan följande kommando i konsolen för fönstret toosee felsökningsmeddelanden hello.</span><span class="sxs-lookup"><span data-stu-id="17f1d-122">Try tooterminate current gulp tasks by using Ctrl + C, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="17f1d-123">Du kan visa detaljerade felmeddelanden i konsolens utdata.</span><span class="sxs-lookup"><span data-stu-id="17f1d-123">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="17f1d-124">Problem vid identifiering av enheter</span><span class="sxs-lookup"><span data-stu-id="17f1d-124">Device discovery issues</span></span>
<span data-ttu-id="17f1d-125">Felsökning av vanliga problem med hello `devdisco` kommandot, kontrollera hello [viktigt](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span><span class="sxs-lookup"><span data-stu-id="17f1d-125">For help in troubleshooting common problems with hello `devdisco` command, check hello [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="17f1d-126">npm-problem</span><span class="sxs-lookup"><span data-stu-id="17f1d-126">npm issues</span></span>
<span data-ttu-id="17f1d-127">Försök tooupdate npm-paketet med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="17f1d-127">Try tooupdate your npm package by using hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="17f1d-128">Om hello problemet fortfarande kvarstår lämna kommentarer hello slutet av den här artikeln eller skapa ett GitHub-problem i våra [exempel databasen](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).</span><span class="sxs-lookup"><span data-stu-id="17f1d-128">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="17f1d-129">Fjärrfelsökning</span><span class="sxs-lookup"><span data-stu-id="17f1d-129">Remote debugging</span></span>
### <a name="run-hello-sample-application-in-debug-mode"></a><span data-ttu-id="17f1d-130">Köra hello exempelprogrammet i felsökningsläge</span><span class="sxs-lookup"><span data-stu-id="17f1d-130">Run hello sample application in debug mode</span></span>
```bash
gulp run --debug
```

<span data-ttu-id="17f1d-131">När hello debug-motorn är klar, bör du se ```Debugger listening on port 5858``` i hello konsolens utdata.</span><span class="sxs-lookup"><span data-stu-id="17f1d-131">When hello debug engine is ready, you should see ```Debugger listening on port 5858``` in hello console output.</span></span>

### <a name="configure-visual-studio-code-tooconnect-toohello-remote-device"></a><span data-ttu-id="17f1d-132">Konfigurera Visual Studio Code tooconnect toohello fjärranslutna enheter</span><span class="sxs-lookup"><span data-stu-id="17f1d-132">Configure Visual Studio Code tooconnect toohello remote device</span></span>
1. <span data-ttu-id="17f1d-133">Öppna hello **felsöka** panelen för hello vänster sida.</span><span class="sxs-lookup"><span data-stu-id="17f1d-133">Open hello **Debug** panel on hello left side.</span></span>
2. <span data-ttu-id="17f1d-134">Klicka på hello grön **Start Debugging** (F5)-knappen.</span><span class="sxs-lookup"><span data-stu-id="17f1d-134">Click hello green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="17f1d-135">Visual Studio Code öppnar en launch.json-fil.</span><span class="sxs-lookup"><span data-stu-id="17f1d-135">Visual Studio Code opens a launch.json file.</span></span>
3. <span data-ttu-id="17f1d-136">Uppdatera hello launch.json filen med hello följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="17f1d-136">Update hello launch.json file with hello following content.</span></span> <span data-ttu-id="17f1d-137">Ersätt `[device hostname or IP address]` med hello faktiska IP-adressen eller värdnamnet enhetsnamn.</span><span class="sxs-lookup"><span data-stu-id="17f1d-137">Replace `[device hostname or IP address]` with hello actual device IP address or host name.</span></span>

> [!NOTE]
> <span data-ttu-id="17f1d-138">toolearn mer om hello Visual Studio-felsökning, se för[felsökning i Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).</span><span class="sxs-lookup"><span data-stu-id="17f1d-138">toolearn more about hello Visual Studio Debugging, please refer too[Debugging in Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).</span></span>


```json
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
            "remoteRoot": null
        }
    ]
}
```

![Fjärrkonfiguration felsökning](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-toohello-remote-application"></a><span data-ttu-id="17f1d-140">Koppla toohello fjärrprogram</span><span class="sxs-lookup"><span data-stu-id="17f1d-140">Attach toohello remote application</span></span>
<span data-ttu-id="17f1d-141">Klicka på hello grön **Start Debugging** (F5) knappen toostart felsökning.</span><span class="sxs-lookup"><span data-stu-id="17f1d-141">Click hello green **Start Debugging** (F5) button toostart debugging.</span></span>

<span data-ttu-id="17f1d-142">Läs [JavaScript i VS kod](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn mer om hello felsökare.</span><span class="sxs-lookup"><span data-stu-id="17f1d-142">Read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn more about hello debugger.</span></span>

![Remote felsökning interaktiva](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="17f1d-144">Azure CLI-problem</span><span class="sxs-lookup"><span data-stu-id="17f1d-144">Azure CLI issues</span></span>
<span data-ttu-id="17f1d-145">hello Azure-kommandoradsgränssnittet (Azure CLI) är en förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="17f1d-145">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="17f1d-146">tooseek lösningar, kan du använda hello [Preview installera guiden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span><span class="sxs-lookup"><span data-stu-id="17f1d-146">tooseek solutions, you can use hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span></span>

<span data-ttu-id="17f1d-147">Om det uppstår några buggar med hello verktyget filen ett [problemet](https://github.com/Azure/azure-cli/issues) i hello **problem** avsnitt i GitHub-repo-hello.</span><span class="sxs-lookup"><span data-stu-id="17f1d-147">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="17f1d-148">Hjälp vid felsökning av vanliga problem, kontrollera hello [viktigt](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="17f1d-148">For help in troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

## <a name="python-installation-issues"></a><span data-ttu-id="17f1d-149">Problem med installation av Python</span><span class="sxs-lookup"><span data-stu-id="17f1d-149">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="17f1d-150">Äldre installationsproblem (macOS)</span><span class="sxs-lookup"><span data-stu-id="17f1d-150">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="17f1d-151">När du installerar pip, en behörighet felmeddelande när äldre paket installeras med **su** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="17f1d-151">When you're installing pip, a permission error is thrown when older packages are installed with **su** permissions.</span></span> <span data-ttu-id="17f1d-152">Detta inträffar eftersom en tidigare installation av Python med brew (macOS) inte har avinstallerats helt.</span><span class="sxs-lookup"><span data-stu-id="17f1d-152">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="17f1d-153">Vissa pip-paket från en tidigare installation skapades rot, vilket gör att hello behörighetsfel.</span><span class="sxs-lookup"><span data-stu-id="17f1d-153">Some pip packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="17f1d-154">Hej lösningen är tooremove de paket som installerats av roten.</span><span class="sxs-lookup"><span data-stu-id="17f1d-154">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="17f1d-155">Använd följande steg toocomplete hello den här aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="17f1d-155">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="17f1d-156">Gå till: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="17f1d-156">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="17f1d-157">Lista över paket som skapats av roten:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="17f1d-157">List packages created by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="17f1d-158">Avinstallera paket från steg 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="17f1d-158">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="17f1d-159">Installera om Python.</span><span class="sxs-lookup"><span data-stu-id="17f1d-159">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="17f1d-160">Azure IoT-hubb-problem</span><span class="sxs-lookup"><span data-stu-id="17f1d-160">Azure IoT Hub issues</span></span>
<span data-ttu-id="17f1d-161">Om din Azure IoT-hubb med Azure CLI har etablerats, och du behöver en verktyget toomanage hello enheter som ansluter tooyour IoT-hubb, kan du försöka hello följande verktyg.</span><span class="sxs-lookup"><span data-stu-id="17f1d-161">If you've successfully provisioned your Azure IoT hub with Azure CLI, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools.</span></span>

### <a name="device-explorer"></a><span data-ttu-id="17f1d-162">Enheten explorer</span><span class="sxs-lookup"><span data-stu-id="17f1d-162">Device explorer</span></span>
<span data-ttu-id="17f1d-163">Hej [enheten explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) verktyget körs på den lokala Windows-datorn och ansluter tooyour IoT-hubb i Azure.</span><span class="sxs-lookup"><span data-stu-id="17f1d-163">hello [Device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) tool runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="17f1d-164">Den kommunicerar med hello följande [IoT-hubbslutpunkter](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="17f1d-164">It communicates with hello following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>


* <span data-ttu-id="17f1d-165">*Enheten Identitetshantering* tooprovision och hantera enheter som registrerats med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="17f1d-165">*Device identity management* tooprovision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="17f1d-166">*Ta emot enhet till moln* så att du kan övervaka meddelanden som skickas från din enhet tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="17f1d-166">*Receive device-to-cloud* so you can monitor messages sent from your device tooyour IoT hub.</span></span>
* <span data-ttu-id="17f1d-167">*Skicka moln till enhet* så att du kan skicka meddelanden tooyour enheter från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="17f1d-167">*Send cloud-to-device* so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="17f1d-168">Konfigurera anslutningssträngen IoT-hubb i det här verktyget toouse dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="17f1d-168">Configure your IoT hub connection string within this tool toouse all its capabilities.</span></span>

### <a name="iothub-explorer"></a><span data-ttu-id="17f1d-169">iothub explorer</span><span class="sxs-lookup"><span data-stu-id="17f1d-169">iothub-explorer</span></span>
<span data-ttu-id="17f1d-170">[iothub explorer](https://github.com/Azure/iothub-explorer) är ett exempel flera plattformar CLI toomanage enheter.</span><span class="sxs-lookup"><span data-stu-id="17f1d-170">[iothub-explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage devices.</span></span> <span data-ttu-id="17f1d-171">Du kan använda hello verktyget toomanage hello enheter i hello identitetsregistret, övervaka meddelanden från enhet till moln och skicka meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="17f1d-171">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device messages.</span></span>

<span data-ttu-id="17f1d-172">tooinstall hello senaste () förhandsversion av hello iothub explorer verktyg, kör följande kommando i Kommandotolken miljön hello:</span><span class="sxs-lookup"><span data-stu-id="17f1d-172">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="17f1d-173">Du kan använda följande kommando tooget mer hjälp om alla hello iothub explorer kommandon och deras parametrar hello:</span><span class="sxs-lookup"><span data-stu-id="17f1d-173">You can use hello following command tooget additional help about all hello iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="17f1d-174">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="17f1d-174">Azure portal</span></span>
<span data-ttu-id="17f1d-175">En fullständig CLI-upplevelse hjälper dig att skapa och hantera alla dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="17f1d-175">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="17f1d-176">Du kan också toouse hello [Azure-portalen](../azure-portal-overview.md) toohelp etablera, hantera och felsöka Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="17f1d-176">You might also want toouse hello [Azure portal](../azure-portal-overview.md) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="17f1d-177">Azure Storage-problem</span><span class="sxs-lookup"><span data-stu-id="17f1d-177">Azure Storage issues</span></span>
<span data-ttu-id="17f1d-178">[Microsoft Azure Lagringsutforskaren (förhandsversion)](http://storageexplorer.com) är en fristående app från Microsoft som du kan använda toowork med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="17f1d-178">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use toowork with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="17f1d-179">Med det här verktyget kan du ansluta tooyour tabell och se hello data i den.</span><span class="sxs-lookup"><span data-stu-id="17f1d-179">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="17f1d-180">Du kan använda det här verktyget tootroubleshoot Azure Storage-problem.</span><span class="sxs-lookup"><span data-stu-id="17f1d-180">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>

