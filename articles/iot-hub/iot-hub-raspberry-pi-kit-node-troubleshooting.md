---
title: "Ansluta Raspberry Pi (C) till Azure IoT - felsökning | Microsoft Docs"
description: "Felsökning för hallon Pi Node.js-upplevelse"
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
ms.openlocfilehash: f9058068972ddbb674d3cd159948835dc88c4451
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="fd916-104">Felsökning</span><span class="sxs-lookup"><span data-stu-id="fd916-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="fd916-105">Problem med maskinvara</span><span class="sxs-lookup"><span data-stu-id="fd916-105">Hardware issues</span></span>
### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a><span data-ttu-id="fd916-106">Programmet ska fungera bra men Indikatorn inte blinkar</span><span class="sxs-lookup"><span data-stu-id="fd916-106">The application runs well but the LED is not blinking</span></span>
<span data-ttu-id="fd916-107">Det här problemet är alltid relaterade till maskinvara krets anslutningen.</span><span class="sxs-lookup"><span data-stu-id="fd916-107">This issue is always related to hardware circuit connectivity.</span></span> <span data-ttu-id="fd916-108">Använd följande steg för att identifiera problem:</span><span class="sxs-lookup"><span data-stu-id="fd916-108">Use the following steps to identify problems:</span></span>

1. <span data-ttu-id="fd916-109">Kontrollera att du har valt rätt **GPIO** på din-kort.</span><span class="sxs-lookup"><span data-stu-id="fd916-109">Check that you chose the correct **GPIO** on your board.</span></span> <span data-ttu-id="fd916-110">Två portar bör vara **GPIO GND (PIN-kod 6)** och **GPIO 04 (PIN-kod 7)**.</span><span class="sxs-lookup"><span data-stu-id="fd916-110">The two ports should be **GPIO GND (Pin 6)** and **GPIO 04 (Pin 7)**.</span></span>
2. <span data-ttu-id="fd916-111">Kontrollera att polaritet för din Indikator är korrekt.</span><span class="sxs-lookup"><span data-stu-id="fd916-111">Check that the polarity of your LED is correct.</span></span> <span data-ttu-id="fd916-112">Längre ben bör ange den **positiva**, av PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="fd916-112">The longer leg should indicate the **positive**, anode pin.</span></span>
3. <span data-ttu-id="fd916-113">Använd den **3, 3v fästa** och **jord Pin** på hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="fd916-113">Use the **3.3V Pin** and **GND Pin** on Raspberry Pi 3.</span></span> <span data-ttu-id="fd916-114">Behandla Pi som DC-ström.</span><span class="sxs-lookup"><span data-stu-id="fd916-114">Treat Pi as the DC power.</span></span> <span data-ttu-id="fd916-115">Kontrollera att Indikatorn fungerar bra.</span><span class="sxs-lookup"><span data-stu-id="fd916-115">Check that the LED works fine.</span></span>

![Specifikation för Indikator](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a><span data-ttu-id="fd916-117">Andra problem med maskinvara</span><span class="sxs-lookup"><span data-stu-id="fd916-117">Other hardware issues</span></span>
<span data-ttu-id="fd916-118">Information om hur du löser vanliga problem på hallon Pi 3 finns i [officiella felsökning sidan](http://elinux.org/R-Pi_Troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="fd916-118">For information about solving common problems on Raspberry Pi 3, see the [official troubleshooting page](http://elinux.org/R-Pi_Troubleshooting).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="fd916-119">Problem med node.js-paket</span><span class="sxs-lookup"><span data-stu-id="fd916-119">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="fd916-120">Inget svar under gulp uppgifter</span><span class="sxs-lookup"><span data-stu-id="fd916-120">No response during gulp tasks</span></span>
<span data-ttu-id="fd916-121">Om du stöter på problem i gulp aktiviteter som körs, kan du lägga till den `--verbose` alternativet för felsökning.</span><span class="sxs-lookup"><span data-stu-id="fd916-121">If you encounter problems in running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="fd916-122">Försöker avsluta aktuella gulp uppgifter med hjälp av Ctrl + C och kör sedan följande kommando i konsolfönstret för att se felsökningsmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="fd916-122">Try to terminate current gulp tasks by using Ctrl + C, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="fd916-123">Du kan visa detaljerade felmeddelanden i konsolens utdata.</span><span class="sxs-lookup"><span data-stu-id="fd916-123">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="fd916-124">Problem vid identifiering av enheter</span><span class="sxs-lookup"><span data-stu-id="fd916-124">Device discovery issues</span></span>
<span data-ttu-id="fd916-125">Felsökning av vanliga problem med den `devdisco` kommando, kontrollera den [viktigt](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span><span class="sxs-lookup"><span data-stu-id="fd916-125">For help in troubleshooting common problems with the `devdisco` command, check the [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="fd916-126">npm-problem</span><span class="sxs-lookup"><span data-stu-id="fd916-126">npm issues</span></span>
<span data-ttu-id="fd916-127">Försök att uppdatera npm-paketet med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="fd916-127">Try to update your npm package by using the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="fd916-128">Om problemet kvarstår lämna kommentarer i slutet av den här artikeln eller skapa ett GitHub-problem i våra [exempel databasen](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).</span><span class="sxs-lookup"><span data-stu-id="fd916-128">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="fd916-129">Fjärrfelsökning</span><span class="sxs-lookup"><span data-stu-id="fd916-129">Remote debugging</span></span>
### <a name="run-the-sample-application-in-debug-mode"></a><span data-ttu-id="fd916-130">Kör exempelprogrammet i felsökningsläge</span><span class="sxs-lookup"><span data-stu-id="fd916-130">Run the sample application in debug mode</span></span>
```bash
gulp run --debug
```

<span data-ttu-id="fd916-131">När debug-motorn är klar, bör du se ```Debugger listening on port 5858``` i konsolens utdata.</span><span class="sxs-lookup"><span data-stu-id="fd916-131">When the debug engine is ready, you should see ```Debugger listening on port 5858``` in the console output.</span></span>

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a><span data-ttu-id="fd916-132">Konfigurera Visual Studio-koden för att ansluta till den fjärranslutna enheten</span><span class="sxs-lookup"><span data-stu-id="fd916-132">Configure Visual Studio Code to connect to the remote device</span></span>
1. <span data-ttu-id="fd916-133">Öppna den **felsöka** panelen till vänster.</span><span class="sxs-lookup"><span data-stu-id="fd916-133">Open the **Debug** panel on the left side.</span></span>
2. <span data-ttu-id="fd916-134">Klicka på gröna **Start Debugging** (F5)-knappen.</span><span class="sxs-lookup"><span data-stu-id="fd916-134">Click the green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="fd916-135">Visual Studio Code öppnar en launch.json-fil.</span><span class="sxs-lookup"><span data-stu-id="fd916-135">Visual Studio Code opens a launch.json file.</span></span>
3. <span data-ttu-id="fd916-136">Uppdatera filen launch.json med följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="fd916-136">Update the launch.json file with the following content.</span></span> <span data-ttu-id="fd916-137">Ersätt `[device hostname or IP address]` med det faktiska IP-adress eller värdnamnet enhetsnamnet.</span><span class="sxs-lookup"><span data-stu-id="fd916-137">Replace `[device hostname or IP address]` with the actual device IP address or host name.</span></span>

> [!NOTE]
> <span data-ttu-id="fd916-138">Mer information om felsökning i Visual Studio finns i [felsökning i Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).</span><span class="sxs-lookup"><span data-stu-id="fd916-138">To learn more about the Visual Studio Debugging, please refer to [Debugging in Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).</span></span>


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

### <a name="attach-to-the-remote-application"></a><span data-ttu-id="fd916-140">Anslut till fjärrprogrammet</span><span class="sxs-lookup"><span data-stu-id="fd916-140">Attach to the remote application</span></span>
<span data-ttu-id="fd916-141">Klicka på gröna **Start Debugging** (F5) för att starta felsökning.</span><span class="sxs-lookup"><span data-stu-id="fd916-141">Click the green **Start Debugging** (F5) button to start debugging.</span></span>

<span data-ttu-id="fd916-142">Läs [JavaScript i VS kod](https://code.visualstudio.com/docs/languages/javascript#_debugging) för mer information om felsökning.</span><span class="sxs-lookup"><span data-stu-id="fd916-142">Read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) to learn more about the debugger.</span></span>

![Remote felsökning interaktiva](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="fd916-144">Azure CLI-problem</span><span class="sxs-lookup"><span data-stu-id="fd916-144">Azure CLI issues</span></span>
<span data-ttu-id="fd916-145">Azure-kommandoradsgränssnittet (Azure CLI) är en förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="fd916-145">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="fd916-146">Om du vill söka efter lösningar du kan använda den [Preview installera guiden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span><span class="sxs-lookup"><span data-stu-id="fd916-146">To seek solutions, you can use the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span></span>

<span data-ttu-id="fd916-147">Om det uppstår några fel med verktyget filen ett [problemet](https://github.com/Azure/azure-cli/issues) i den **problem** avsnitt i GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="fd916-147">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="fd916-148">Hjälp vid felsökning av vanliga problem, kontrollera den [viktigt](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="fd916-148">For help in troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

## <a name="python-installation-issues"></a><span data-ttu-id="fd916-149">Problem med installation av Python</span><span class="sxs-lookup"><span data-stu-id="fd916-149">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="fd916-150">Äldre installationsproblem (macOS)</span><span class="sxs-lookup"><span data-stu-id="fd916-150">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="fd916-151">När du installerar pip, en behörighet felmeddelande när äldre paket installeras med **su** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="fd916-151">When you're installing pip, a permission error is thrown when older packages are installed with **su** permissions.</span></span> <span data-ttu-id="fd916-152">Detta inträffar eftersom en tidigare installation av Python med brew (macOS) inte har avinstallerats helt.</span><span class="sxs-lookup"><span data-stu-id="fd916-152">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="fd916-153">Vissa pip-paket från en tidigare installation skapades rot, som orsakar felet behörighet.</span><span class="sxs-lookup"><span data-stu-id="fd916-153">Some pip packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="fd916-154">Lösningen är att ta bort de paket som installerats av roten.</span><span class="sxs-lookup"><span data-stu-id="fd916-154">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="fd916-155">Använd följande steg för att slutföra den här aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="fd916-155">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="fd916-156">Gå till: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="fd916-156">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="fd916-157">Lista över paket som skapats av roten:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="fd916-157">List packages created by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="fd916-158">Avinstallera paket från steg 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="fd916-158">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="fd916-159">Installera om Python.</span><span class="sxs-lookup"><span data-stu-id="fd916-159">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="fd916-160">Azure IoT-hubb-problem</span><span class="sxs-lookup"><span data-stu-id="fd916-160">Azure IoT Hub issues</span></span>
<span data-ttu-id="fd916-161">Om din Azure IoT-hubb med Azure CLI har etablerats och du behöver ett verktyg för att hantera enheter som ansluter till din IoT-hubb, kan du prova följande verktyg.</span><span class="sxs-lookup"><span data-stu-id="fd916-161">If you've successfully provisioned your Azure IoT hub with Azure CLI, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools.</span></span>

### <a name="device-explorer"></a><span data-ttu-id="fd916-162">Enheten explorer</span><span class="sxs-lookup"><span data-stu-id="fd916-162">Device explorer</span></span>
<span data-ttu-id="fd916-163">Den [enheten explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) verktyget körs på den lokala Windows-datorn och ansluter till din IoT-hubb i Azure.</span><span class="sxs-lookup"><span data-stu-id="fd916-163">The [Device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) tool runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="fd916-164">Den kommunicerar med följande [IoT-hubbslutpunkter](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="fd916-164">It communicates with the following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>


* <span data-ttu-id="fd916-165">*Enheten Identitetshantering* att etablera och hantera enheter som registrerats med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="fd916-165">*Device identity management* to provision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="fd916-166">*Ta emot enhet till moln* så att du kan övervaka meddelanden som skickas från enheten till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="fd916-166">*Receive device-to-cloud* so you can monitor messages sent from your device to your IoT hub.</span></span>
* <span data-ttu-id="fd916-167">*Skicka moln till enhet* så att du kan skicka meddelanden till dina enheter från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="fd916-167">*Send cloud-to-device* so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="fd916-168">Konfigurera anslutningssträngen IoT-hubb i det här verktyget och använda dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="fd916-168">Configure your IoT hub connection string within this tool to use all its capabilities.</span></span>

### <a name="iothub-explorer"></a><span data-ttu-id="fd916-169">iothub explorer</span><span class="sxs-lookup"><span data-stu-id="fd916-169">iothub-explorer</span></span>
<span data-ttu-id="fd916-170">[iothub explorer](https://github.com/Azure/iothub-explorer) är ett exempel flera plattformar CLI verktyg för att hantera enheter.</span><span class="sxs-lookup"><span data-stu-id="fd916-170">[iothub-explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage devices.</span></span> <span data-ttu-id="fd916-171">Du kan använda verktyget för att hantera enheter i identitetsregistret, övervaka meddelanden från enhet till moln och skicka meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="fd916-171">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device messages.</span></span>

<span data-ttu-id="fd916-172">För att installera den senaste (förhandsversion) versionen av verktyget iothub explorer, kör du följande kommando i Kommandotolken miljön:</span><span class="sxs-lookup"><span data-stu-id="fd916-172">To install the latest (prerelease) version of the iothub-explorer tool, run the following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="fd916-173">Du kan använda följande kommando för att få ytterligare hjälp om alla iothub explorer kommandon och deras parametrar:</span><span class="sxs-lookup"><span data-stu-id="fd916-173">You can use the following command to get additional help about all the iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="fd916-174">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fd916-174">Azure portal</span></span>
<span data-ttu-id="fd916-175">En fullständig CLI-upplevelse hjälper dig att skapa och hantera alla dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="fd916-175">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="fd916-176">Du kanske också vill använda den [Azure-portalen](../azure-portal-overview.md) för att etablera, hantera och felsöka Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="fd916-176">You might also want to use the [Azure portal](../azure-portal-overview.md) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="fd916-177">Azure Storage-problem</span><span class="sxs-lookup"><span data-stu-id="fd916-177">Azure Storage issues</span></span>
<span data-ttu-id="fd916-178">[Microsoft Azure Lagringsutforskaren (förhandsversion)](http://storageexplorer.com) är en fristående app från Microsoft som du kan använda för att arbeta med Azure Storage data på Windows och macOS Linux.</span><span class="sxs-lookup"><span data-stu-id="fd916-178">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use to work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="fd916-179">Med det här verktyget kan du ansluta till din tabell och visa data i den.</span><span class="sxs-lookup"><span data-stu-id="fd916-179">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="fd916-180">Du kan använda det här verktyget för att felsöka problem med ditt Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="fd916-180">You can use this tool to troubleshoot your Azure Storage issues.</span></span>

