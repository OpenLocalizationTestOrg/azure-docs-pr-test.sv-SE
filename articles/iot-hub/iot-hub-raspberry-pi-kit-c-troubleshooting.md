---
title: "Connect Raspberry PI (C) tooAzure IoT - felsökning | Microsoft Docs"
description: "Felsökning för hallon Pi Node.js-upplevelse"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: IOT-problem, internet saker problem
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 3653c79b-8a0c-41d4-b0bf-f6b4edb4d233
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4f1ea81dd25d10a39c2939f5ee5f19f6d2ba2b2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="d33e3-104">Felsökning</span><span class="sxs-lookup"><span data-stu-id="d33e3-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="d33e3-105">Problem med maskinvara</span><span class="sxs-lookup"><span data-stu-id="d33e3-105">Hardware issues</span></span>
### <a name="hello-application-runs-well-but-hello-led-is-not-blinking"></a><span data-ttu-id="d33e3-106">hello-program körs och men hello Indikator blinkar inte</span><span class="sxs-lookup"><span data-stu-id="d33e3-106">hello application runs well but hello LED is not blinking</span></span>
<span data-ttu-id="d33e3-107">Det här problemet är alltid relaterade toohello maskinvara krets anslutningen.</span><span class="sxs-lookup"><span data-stu-id="d33e3-107">This issue is always related toohello hardware circuit connectivity.</span></span> <span data-ttu-id="d33e3-108">Använd hello följande steg tooidentify problem.</span><span class="sxs-lookup"><span data-stu-id="d33e3-108">Use hello following steps tooidentify problems.</span></span>

1. <span data-ttu-id="d33e3-109">Kontrollera att du har valt rätt hello **GPIO** på din-kort.</span><span class="sxs-lookup"><span data-stu-id="d33e3-109">Check that you chose hello correct **GPIO** on your board.</span></span> <span data-ttu-id="d33e3-110">hello två portar bör vara **GPIO GND (PIN-kod 6)** och **GPIO 04 (PIN-kod 7)**.</span><span class="sxs-lookup"><span data-stu-id="d33e3-110">hello two ports should be **GPIO GND (Pin 6)** and **GPIO 04 (Pin 7)**.</span></span>
2. <span data-ttu-id="d33e3-111">Kontrollera att hello polaritet för din Indikator är korrekt.</span><span class="sxs-lookup"><span data-stu-id="d33e3-111">Check that hello polarity of your LED is correct.</span></span> <span data-ttu-id="d33e3-112">hello längre ben bör ange hello **positiva**, av PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="d33e3-112">hello longer leg should indicate hello **positive**, anode pin.</span></span>
3. <span data-ttu-id="d33e3-113">Använd hello **3, 3v fästa** och **jord Pin** på hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="d33e3-113">Use hello **3.3V Pin** and **GND Pin** on Raspberry Pi 3.</span></span> <span data-ttu-id="d33e3-114">Behandla Pi som hello DC-ström.</span><span class="sxs-lookup"><span data-stu-id="d33e3-114">Treat Pi as hello DC power.</span></span> <span data-ttu-id="d33e3-115">Kontrollera att hello Indikator fungerar bra.</span><span class="sxs-lookup"><span data-stu-id="d33e3-115">Check that hello LED works fine.</span></span>

![Specifikation för Indikator](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a><span data-ttu-id="d33e3-117">Andra problem med maskinvara</span><span class="sxs-lookup"><span data-stu-id="d33e3-117">Other hardware issues</span></span>
<span data-ttu-id="d33e3-118">Information om hur du löser vanliga problem på hallon Pi 3 finns hello [officiella felsökning sidan](http://elinux.org/R-Pi_Troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="d33e3-118">For information about solving common problems on Raspberry Pi 3, see hello [official troubleshooting page](http://elinux.org/R-Pi_Troubleshooting).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="d33e3-119">Problem med node.js-paket</span><span class="sxs-lookup"><span data-stu-id="d33e3-119">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="d33e3-120">Inget svar under gulp uppgifter</span><span class="sxs-lookup"><span data-stu-id="d33e3-120">No response during gulp tasks</span></span>
<span data-ttu-id="d33e3-121">Om du stöter på problem med att köra gulp uppgifter kan du lägga till hello `--verbose` alternativet för felsökning.</span><span class="sxs-lookup"><span data-stu-id="d33e3-121">If you encounter problems running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="d33e3-122">Försök tooterminate aktuella gulp uppgifter med hjälp av `Ctrl + C`, och sedan kör hello följande kommando i konsolen för fönstret toosee felsökningsmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="d33e3-122">Try tooterminate current gulp tasks by using `Ctrl + C`, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="d33e3-123">Du kan visa detaljerade felmeddelanden i konsolens utdata.</span><span class="sxs-lookup"><span data-stu-id="d33e3-123">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="d33e3-124">Problem vid identifiering av enheter</span><span class="sxs-lookup"><span data-stu-id="d33e3-124">Device discovery issues</span></span>
<span data-ttu-id="d33e3-125">Felsökning av vanliga problem med hello `devdisco` kommandot, kontrollera hello [viktigt](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span><span class="sxs-lookup"><span data-stu-id="d33e3-125">For help in troubleshooting common problems with hello `devdisco` command, check hello [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="d33e3-126">NPM-problem</span><span class="sxs-lookup"><span data-stu-id="d33e3-126">NPM issues</span></span>
<span data-ttu-id="d33e3-127">Försök tooupdate NPM-paketet med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d33e3-127">Try tooupdate your NPM package with hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="d33e3-128">Om hello problemet fortfarande kvarstår lämna kommentarer hello slutet av den här artikeln eller skapa ett GitHub-problem i våra [exempel databasen](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)</span><span class="sxs-lookup"><span data-stu-id="d33e3-128">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="d33e3-129">Fjärrfelsökning</span><span class="sxs-lookup"><span data-stu-id="d33e3-129">Remote debugging</span></span>

<span data-ttu-id="d33e3-130">Remote felsökning support blir snart tillgänglig i Visual Studio Code C/C++-tillägget.</span><span class="sxs-lookup"><span data-stu-id="d33e3-130">Remote debugging support will be available soon in Visual Studio Code C/C++ Extension.</span></span>

<span data-ttu-id="d33e3-131">Du kan använda GDB via favorit SSH terminalen i en användarsystem:</span><span class="sxs-lookup"><span data-stu-id="d33e3-131">In a meanwhile you can use GDB via your favourite SSH terminal:</span></span>

```bash
cd c-pi-lesson-x
sudo gdb app
```

## <a name="azure-cli-issues"></a><span data-ttu-id="d33e3-132">Problem med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d33e3-132">Azure-CLI issues</span></span>
<span data-ttu-id="d33e3-133">hello Azure-kommandoradsgränssnittet (Azure CLI) är en förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="d33e3-133">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="d33e3-134">Leta efter en lösning i hello [Preview installera guiden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek lösningar.</span><span class="sxs-lookup"><span data-stu-id="d33e3-134">Look for solution in hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek solutions.</span></span> <span data-ttu-id="d33e3-135">Försök tooupgrade Azure cli toolatest version när kommandon inte fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="d33e3-135">Try tooupgrade Azure-cli toolatest version when commands don’t work as expected.</span></span>

<span data-ttu-id="d33e3-136">Om det uppstår några buggar med hello verktyget filen ett [problemet](https://github.com/Azure/azure-cli/issues) i hello **problem** avsnitt i GitHub-repo-hello.</span><span class="sxs-lookup"><span data-stu-id="d33e3-136">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="d33e3-137">För hjälp med att felsöka vanliga problem, kontrollera hello [viktigt](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="d33e3-137">For help troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="d33e3-138">Om du uppfyller ”det gick inte att hitta en version som uppfyller krav på hello”, ange kommando hello kör följande tooupgrade pip toolastest version.</span><span class="sxs-lookup"><span data-stu-id="d33e3-138">If you meet "Could not find a version that satisfies hello requirement", please run hello following command tooupgrade pip toolastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="d33e3-139">Problem med installation av Python</span><span class="sxs-lookup"><span data-stu-id="d33e3-139">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="d33e3-140">Äldre installationsproblem (macOS)</span><span class="sxs-lookup"><span data-stu-id="d33e3-140">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="d33e3-141">När du installerar **pip**, en behörighet felmeddelande när äldre paket som är installerade med **su** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="d33e3-141">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="d33e3-142">Detta inträffar eftersom en tidigare installation av Python med brew (macOS) inte har avinstallerats helt.</span><span class="sxs-lookup"><span data-stu-id="d33e3-142">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="d33e3-143">Vissa **pip** paket från en tidigare installation har skapats av roten, vilket gör att hello behörighetsfel.</span><span class="sxs-lookup"><span data-stu-id="d33e3-143">Some **pip** packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="d33e3-144">Hej lösningen är tooremove de paket som installerats av roten.</span><span class="sxs-lookup"><span data-stu-id="d33e3-144">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="d33e3-145">Använd följande steg toocomplete hello den här aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="d33e3-145">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="d33e3-146">Gå till: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="d33e3-146">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="d33e3-147">Lista paket skapa av roten:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="d33e3-147">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="d33e3-148">Avinstallera paket från steg 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="d33e3-148">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="d33e3-149">Installera om Python.</span><span class="sxs-lookup"><span data-stu-id="d33e3-149">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="d33e3-150">Azure IoT-hubb-problem</span><span class="sxs-lookup"><span data-stu-id="d33e3-150">Azure IoT Hub issues</span></span>
<span data-ttu-id="d33e3-151">Om du har etablerats dina Azure IoT-hubb med `azure-cli`, och du behöver en verktyget toomanage hello enheter som ansluter tooyour IoT hub, försök hello följande verktyg:</span><span class="sxs-lookup"><span data-stu-id="d33e3-151">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="d33e3-152">Enheten Explorer</span><span class="sxs-lookup"><span data-stu-id="d33e3-152">Device Explorer</span></span>
<span data-ttu-id="d33e3-153">[Enheten Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) körs på den lokala Windows-datorn och ansluter tooyour IoT-hubb i Azure.</span><span class="sxs-lookup"><span data-stu-id="d33e3-153">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="d33e3-154">Den kommunicerar med hello följande [IoT-hubbslutpunkter](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="d33e3-154">It communicates with hello following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

* <span data-ttu-id="d33e3-155">*Enheten Identitetshantering* tooprovision och hantera enheter som registrerats med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="d33e3-155">*Device identity management* tooprovision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="d33e3-156">*Ta emot enhet till moln* så att du kan övervaka meddelanden som skickas från din enhet tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d33e3-156">*Receive device-to-cloud* so you can monitor messages sent from your device tooyour IoT hub.</span></span>
* <span data-ttu-id="d33e3-157">*Skicka moln till enhet* så att du kan skicka meddelanden tooyour enheter från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d33e3-157">*Send cloud-to-device* so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="d33e3-158">Konfigurera din `IoT hub connection string` i det här verktyget toouse dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="d33e3-158">Configure your `IoT hub connection string` within this tool toouse all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="d33e3-159">IoT-hubb Explorer</span><span class="sxs-lookup"><span data-stu-id="d33e3-159">IoT hub Explorer</span></span>
<span data-ttu-id="d33e3-160">[IoT-hubb Explorer](https://github.com/Azure/iothub-explorer) är ett exempel flera plattformar CLI toomanage enhetsklienter.</span><span class="sxs-lookup"><span data-stu-id="d33e3-160">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage device clients.</span></span> <span data-ttu-id="d33e3-161">Du kan använda hello verktyget toomanage hello enheter i hello identitetsregistret, övervaka meddelanden från enhet till moln och skicka kommandon moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="d33e3-161">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="d33e3-162">tooinstall hello senaste () förhandsversion av hello iothub explorer verktyg, kör följande kommando i Kommandotolken miljön hello:</span><span class="sxs-lookup"><span data-stu-id="d33e3-162">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command in your command-line environment:</span></span>

```
npm install -g iothub-explorer@latest
```

<span data-ttu-id="d33e3-163">Du kan använda följande kommando tooget mer hjälp om alla hello iothub explorer kommandon och deras parametrar hello:</span><span class="sxs-lookup"><span data-stu-id="d33e3-163">You can use hello following command tooget additional help about all hello iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="d33e3-164">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d33e3-164">Azure portal</span></span>
<span data-ttu-id="d33e3-165">En fullständig CLI-upplevelse hjälper dig att skapa och hantera alla dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="d33e3-165">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="d33e3-166">Du kan också toouse hello [Azure-portalen](../azure-portal-overview.md) toohelp etablera, hantera och felsöka Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="d33e3-166">You might also want toouse hello [Azure portal](../azure-portal-overview.md) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="d33e3-167">Problem med Azure-lagring</span><span class="sxs-lookup"><span data-stu-id="d33e3-167">Azure storage issues</span></span>
<span data-ttu-id="d33e3-168">[Microsoft Azure Lagringsutforskaren (förhandsversion)](http://storageexplorer.com) är en fristående app från Microsoft som du kan använda toowork med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="d33e3-168">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use toowork with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="d33e3-169">Med det här verktyget kan du ansluta tooyour tabell och se hello data i den.</span><span class="sxs-lookup"><span data-stu-id="d33e3-169">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="d33e3-170">Du kan använda det här verktyget tootroubleshoot Azure Storage-problem.</span><span class="sxs-lookup"><span data-stu-id="d33e3-170">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>
