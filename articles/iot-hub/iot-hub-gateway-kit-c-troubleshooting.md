---
title: "SensorTag enhet & Azure IoT-Gateway - felsökning | Microsoft Docs"
description: "Felsökning för Intel NUC gateway"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT-problem, internet saker problem
ROBOTS: NOINDEX
ms.assetid: 5f742c38-0e33-465a-9a0d-1e41e8d17187
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 7e80951de55ade6b5140608dcff8ebb064f942ca
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="089f0-104">Felsökning</span><span class="sxs-lookup"><span data-stu-id="089f0-104">Troubleshooting</span></span>

## <a name="hardware-issues"></a><span data-ttu-id="089f0-105">Problem med maskinvara</span><span class="sxs-lookup"><span data-stu-id="089f0-105">Hardware issues</span></span>

### <a name="ti-sensortag-cannot-be-connected"></a><span data-ttu-id="089f0-106">Det går inte att ansluta TI SensorTag</span><span class="sxs-lookup"><span data-stu-id="089f0-106">TI SensorTag cannot be connected</span></span>

<span data-ttu-id="089f0-107">Använd felsökning av problem med nätverksanslutningen SensorTag den [SensorTag app](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).</span><span class="sxs-lookup"><span data-stu-id="089f0-107">To troubleshoot SensorTag connectivity issues, use the [SensorTag app](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).</span></span>

### <a name="have-an-issue-with-intel-nuc"></a><span data-ttu-id="089f0-108">Har ett problem med Intel NUC</span><span class="sxs-lookup"><span data-stu-id="089f0-108">Have an issue with Intel NUC</span></span>

<span data-ttu-id="089f0-109">Felsökning av startproblem med avser [Felsökning nr Start på Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).</span><span class="sxs-lookup"><span data-stu-id="089f0-109">To troubleshoot boot issues, refer to [troubleshooting No Boot Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).</span></span>

<span data-ttu-id="089f0-110">Felsökning av problem med operativsystemet avser [felsökning av problem med operativsystemet på Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).</span><span class="sxs-lookup"><span data-stu-id="089f0-110">To troubleshoot operating system issues, refer to [troubleshooting Operating System Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).</span></span>

<span data-ttu-id="089f0-111">Felsökning av problem med andra avser [blinkar och signal koder för Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).</span><span class="sxs-lookup"><span data-stu-id="089f0-111">To troubleshoot other issues, refer to [Blink Codes and Beep Codes for Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="089f0-112">Problem med node.js-paket</span><span class="sxs-lookup"><span data-stu-id="089f0-112">Node.js package issues</span></span>

### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="089f0-113">Inget svar under gulp uppgifter</span><span class="sxs-lookup"><span data-stu-id="089f0-113">No response during gulp tasks</span></span>

<span data-ttu-id="089f0-114">Om du stöter på problem i gulp aktiviteter som körs, kan du lägga till den `--verbose` alternativet för felsökning.</span><span class="sxs-lookup"><span data-stu-id="089f0-114">If you encounter problems in running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="089f0-115">Försöker avsluta aktuella gulp uppgifter med hjälp av `Ctrl + C`, och kör sedan följande kommando i din konsolfönster Se felsökningsmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="089f0-115">Try to terminate current gulp tasks by using `Ctrl + C`, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="089f0-116">Du kan visa detaljerade felmeddelanden i konsolens utdata.</span><span class="sxs-lookup"><span data-stu-id="089f0-116">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="089f0-117">Problem vid identifiering av enheter</span><span class="sxs-lookup"><span data-stu-id="089f0-117">Device discovery issues</span></span>

<span data-ttu-id="089f0-118">Felsökning av vanliga problem med den `discover-sensortag` kommando, kontrollera den [wiki-sida](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).</span><span class="sxs-lookup"><span data-stu-id="089f0-118">For help in troubleshooting common problems with the `discover-sensortag` command, check the [wiki page](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="089f0-119">npm-problem</span><span class="sxs-lookup"><span data-stu-id="089f0-119">npm issues</span></span>

<span data-ttu-id="089f0-120">Försök att uppdatera npm-paketet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="089f0-120">Try to update your npm package by running the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="089f0-121">Om problemet kvarstår lämna kommentarer i slutet av den här artikeln eller skapa ett GitHub-problem i våra [exempel databasen](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).</span><span class="sxs-lookup"><span data-stu-id="089f0-121">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="089f0-122">Fjärrfelsökning</span><span class="sxs-lookup"><span data-stu-id="089f0-122">Remote Debugging</span></span>
> <span data-ttu-id="089f0-123">Nedan instruktioner är avsedda för att felsöka node.js-skript som används i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="089f0-123">Below instructions are meant for debugging node.js scripts used in this tutorial.</span></span>
### <a name="run-the-sample-application-in-debug-mode"></a><span data-ttu-id="089f0-124">Kör exempelprogrammet i felsökningsläge</span><span class="sxs-lookup"><span data-stu-id="089f0-124">Run the sample application in debug mode</span></span>

<span data-ttu-id="089f0-125">Kör exempelprogrammet i felsökningsläge genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="089f0-125">Run the sample application in debug mode by running the following command:</span></span>

```bash
gulp run --debug
```

<span data-ttu-id="089f0-126">När debug-motorn är klar, bör du se `Debugger listening on port 5858` i konsolens utdata.</span><span class="sxs-lookup"><span data-stu-id="089f0-126">When the debug engine is ready, you should see `Debugger listening on port 5858` in the console output.</span></span>

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a><span data-ttu-id="089f0-127">Konfigurera Visual Studio-koden för att ansluta till den fjärranslutna enheten</span><span class="sxs-lookup"><span data-stu-id="089f0-127">Configure Visual Studio Code to connect to the remote device</span></span>

1. <span data-ttu-id="089f0-128">Öppna den **felsöka** panelen till vänster.</span><span class="sxs-lookup"><span data-stu-id="089f0-128">Open the **Debug** panel on the left side.</span></span>
2. <span data-ttu-id="089f0-129">Klicka på gröna **Start Debugging** (F5)-knappen.</span><span class="sxs-lookup"><span data-stu-id="089f0-129">Click the green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="089f0-130">Visual Studio Code öppnar en `launch.json` fil.</span><span class="sxs-lookup"><span data-stu-id="089f0-130">Visual Studio Code opens a `launch.json` file.</span></span>
3. <span data-ttu-id="089f0-131">Uppdatera den `launch.json` filen med följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="089f0-131">Update the `launch.json` file with the following content.</span></span> <span data-ttu-id="089f0-132">Ersätt `[device hostname or IP address]` med det faktiska IP-adress eller värdnamnet enhetsnamnet.</span><span class="sxs-lookup"><span data-stu-id="089f0-132">Replace `[device hostname or IP address]` with the actual device IP address or host name.</span></span>

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

### <a name="attach-to-the-remote-application"></a><span data-ttu-id="089f0-134">Anslut till fjärrprogrammet</span><span class="sxs-lookup"><span data-stu-id="089f0-134">Attach to the remote application</span></span>

<span data-ttu-id="089f0-135">Klicka på gröna **Start Debugging** (F5) för att starta felsökning.</span><span class="sxs-lookup"><span data-stu-id="089f0-135">Click the green **Start Debugging** (F5) button to start debugging.</span></span>

<span data-ttu-id="089f0-136">Läs [JavaScript i VS kod](https://code.visualstudio.com/docs/languages/javascript#_debugging) för mer information om felsökning.</span><span class="sxs-lookup"><span data-stu-id="089f0-136">Read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) to learn more about the debugger.</span></span>

![Felsökning Bell-exempel](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="089f0-138">Azure CLI-problem</span><span class="sxs-lookup"><span data-stu-id="089f0-138">Azure CLI issues</span></span>

<span data-ttu-id="089f0-139">Azure-kommandoradsgränssnittet (Azure CLI) är en förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="089f0-139">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="089f0-140">Om du vill söka efter lösningar du kan använda den [Preview installera guiden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span><span class="sxs-lookup"><span data-stu-id="089f0-140">To seek solutions, you can use the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span></span>

<span data-ttu-id="089f0-141">Om det uppstår några fel med verktyget filen ett [problemet](https://github.com/Azure/azure-cli/issues) i den **problem** avsnitt i GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="089f0-141">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="089f0-142">Hjälp vid felsökning av vanliga problem, kontrollera den [viktigt](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="089f0-142">For help in troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="089f0-143">Kör följande kommando för att uppgradera pip till senaste versionen om du uppfyller ”det gick inte att hitta en version som uppfyller krav”.</span><span class="sxs-lookup"><span data-stu-id="089f0-143">If you meet "Could not find a version that satisfies the requirement", please run the following command to upgrade pip to lastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="089f0-144">Problem med installation av Python</span><span class="sxs-lookup"><span data-stu-id="089f0-144">Python installation issues</span></span>

### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="089f0-145">Äldre installationsproblem (macOS)</span><span class="sxs-lookup"><span data-stu-id="089f0-145">Legacy installation issues (macOS)</span></span>

<span data-ttu-id="089f0-146">När du installerar pip, en behörighet felmeddelande när äldre paket installeras med **su** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="089f0-146">When you're installing pip, a permission error is thrown when older packages are installed with **su** permissions.</span></span> <span data-ttu-id="089f0-147">Detta inträffar eftersom en tidigare installation av Python med brew (macOS) inte har avinstallerats helt.</span><span class="sxs-lookup"><span data-stu-id="089f0-147">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="089f0-148">Vissa pip-paket från en tidigare installation skapades rot, som orsakar felet behörighet.</span><span class="sxs-lookup"><span data-stu-id="089f0-148">Some pip packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="089f0-149">Lösningen är att ta bort de paket som installerats av roten.</span><span class="sxs-lookup"><span data-stu-id="089f0-149">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="089f0-150">Använd följande steg för att slutföra den här aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="089f0-150">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="089f0-151">Gå till `/usr/local/lib/python2.7/site-packages`</span><span class="sxs-lookup"><span data-stu-id="089f0-151">Go to `/usr/local/lib/python2.7/site-packages`</span></span>
2. <span data-ttu-id="089f0-152">Lista över paket som skapats av roten:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="089f0-152">List packages created by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="089f0-153">Avinstallera paket från steg 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="089f0-153">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="089f0-154">Installera om Python.</span><span class="sxs-lookup"><span data-stu-id="089f0-154">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="089f0-155">Azure IoT-hubb-problem</span><span class="sxs-lookup"><span data-stu-id="089f0-155">Azure IoT Hub issues</span></span>

<span data-ttu-id="089f0-156">Om din Azure IoT-hubb med Azure CLI har etablerats och du behöver ett verktyg för att hantera enheter som ansluter till din IoT-hubb, kan du prova följande verktyg.</span><span class="sxs-lookup"><span data-stu-id="089f0-156">If you've successfully provisioned your Azure IoT hub with the Azure CLI, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools.</span></span>

### <a name="device-explorer"></a><span data-ttu-id="089f0-157">Enheten Explorer</span><span class="sxs-lookup"><span data-stu-id="089f0-157">Device Explorer</span></span>

<span data-ttu-id="089f0-158">[Enheten Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) körs på den lokala Windows-datorn och ansluter till din IoT-hubb i Azure.</span><span class="sxs-lookup"><span data-stu-id="089f0-158">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="089f0-159">Den kommunicerar med följande [IoT-hubbslutpunkter](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):</span><span class="sxs-lookup"><span data-stu-id="089f0-159">It communicates with the following [IoT Hub endpoints](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):</span></span>

- <span data-ttu-id="089f0-160">Identitetshantering för enheten för att etablera och hantera enheter som registrerats med din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="089f0-160">Device identity management to provision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="089f0-161">Ta emot enhet till moln så att du kan övervaka meddelanden som skickas från enheten till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="089f0-161">Receive device-to-cloud so you can monitor messages sent from your device to your IoT hub.</span></span>
- <span data-ttu-id="089f0-162">Skicka moln till enhet så att du kan skicka meddelanden till dina enheter från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="089f0-162">Send cloud-to-device so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="089f0-163">Konfigurera anslutningssträngen IoT-hubb i det här verktyget och använda dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="089f0-163">Configure your IoT hub connection string within this tool to use all its capabilities.</span></span>

### <a name="iothub-explorer"></a><span data-ttu-id="089f0-164">iothub explorer</span><span class="sxs-lookup"><span data-stu-id="089f0-164">iothub-explorer</span></span>

<span data-ttu-id="089f0-165">[iothub explorer](https://github.com/Azure/iothub-explorer) är ett exempel flera plattformar CLI verktyg för att hantera klienter.</span><span class="sxs-lookup"><span data-stu-id="089f0-165">[iothub-explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage device clients.</span></span> <span data-ttu-id="089f0-166">Du kan använda verktyget för att hantera enheter i identitetsregistret, övervaka meddelanden från enhet till moln och skicka kommandon moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="089f0-166">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="089f0-167">Installera den senaste (förhandsversion) versionen av verktyget iothub explorer genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="089f0-167">To install the latest (prerelease) version of the iothub-explorer tool, run the following command:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="089f0-168">För ytterligare information om alla iothub explorer kommandon och deras parametrar, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="089f0-168">To get additional help about all the iothub-explorer commands and their parameters, run the following command:</span></span>

```bash
iothub-explorer help
```

### <a name="the-azure-portal"></a><span data-ttu-id="089f0-169">Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="089f0-169">The Azure portal</span></span>

<span data-ttu-id="089f0-170">En fullständig CLI-upplevelse hjälper dig att skapa och hantera alla dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="089f0-170">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="089f0-171">Du kanske också vill använda den [Azure-portalen](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) för att etablera, hantera och felsöka Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="089f0-171">You might also want to use the [Azure portal](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="089f0-172">Azure Storage-problem</span><span class="sxs-lookup"><span data-stu-id="089f0-172">Azure Storage issues</span></span>

<span data-ttu-id="089f0-173">[Microsoft Azure Lagringsutforskaren (förhandsversion)](http://storageexplorer.com/) är en fristående app från Microsoft som du kan använda för att arbeta med Azure Storage data på Windows och macOS Linux.</span><span class="sxs-lookup"><span data-stu-id="089f0-173">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com/) is a standalone app from Microsoft that you can use to work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="089f0-174">Med det här verktyget kan du ansluta till din tabell och visa data i den.</span><span class="sxs-lookup"><span data-stu-id="089f0-174">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="089f0-175">Du kan använda det här verktyget för att felsöka problem med ditt Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="089f0-175">You can use this tool to troubleshoot your Azure Storage issues.</span></span>
