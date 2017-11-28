---
title: "Ansluta Raspberry Pi (C) till Azure IoT - felsökning | Microsoft Docs"
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
ms.openlocfilehash: 828669db23fa8d608029134fbe364033456d935a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="9b458-104">Felsökning</span><span class="sxs-lookup"><span data-stu-id="9b458-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="9b458-105">Problem med maskinvara</span><span class="sxs-lookup"><span data-stu-id="9b458-105">Hardware issues</span></span>
### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a><span data-ttu-id="9b458-106">Programmet ska fungera bra men Indikatorn inte blinkar</span><span class="sxs-lookup"><span data-stu-id="9b458-106">The application runs well but the LED is not blinking</span></span>
<span data-ttu-id="9b458-107">Det här problemet är alltid relaterade till maskinvara krets anslutningen.</span><span class="sxs-lookup"><span data-stu-id="9b458-107">This issue is always related to the hardware circuit connectivity.</span></span> <span data-ttu-id="9b458-108">Använd följande steg för att identifiera problem.</span><span class="sxs-lookup"><span data-stu-id="9b458-108">Use the following steps to identify problems.</span></span>

1. <span data-ttu-id="9b458-109">Kontrollera att du har valt rätt **GPIO** på din-kort.</span><span class="sxs-lookup"><span data-stu-id="9b458-109">Check that you chose the correct **GPIO** on your board.</span></span> <span data-ttu-id="9b458-110">Två portar bör vara **GPIO GND (PIN-kod 6)** och **GPIO 04 (PIN-kod 7)**.</span><span class="sxs-lookup"><span data-stu-id="9b458-110">The two ports should be **GPIO GND (Pin 6)** and **GPIO 04 (Pin 7)**.</span></span>
2. <span data-ttu-id="9b458-111">Kontrollera att polaritet för din Indikator är korrekt.</span><span class="sxs-lookup"><span data-stu-id="9b458-111">Check that the polarity of your LED is correct.</span></span> <span data-ttu-id="9b458-112">Längre ben bör ange den **positiva**, av PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="9b458-112">The longer leg should indicate the **positive**, anode pin.</span></span>
3. <span data-ttu-id="9b458-113">Använd den **3, 3v fästa** och **jord Pin** på hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="9b458-113">Use the **3.3V Pin** and **GND Pin** on Raspberry Pi 3.</span></span> <span data-ttu-id="9b458-114">Behandla Pi som DC-ström.</span><span class="sxs-lookup"><span data-stu-id="9b458-114">Treat Pi as the DC power.</span></span> <span data-ttu-id="9b458-115">Kontrollera att Indikatorn fungerar bra.</span><span class="sxs-lookup"><span data-stu-id="9b458-115">Check that the LED works fine.</span></span>

![Specifikation för Indikator](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a><span data-ttu-id="9b458-117">Andra problem med maskinvara</span><span class="sxs-lookup"><span data-stu-id="9b458-117">Other hardware issues</span></span>
<span data-ttu-id="9b458-118">Information om hur du löser vanliga problem på hallon Pi 3 finns i [officiella felsökning sidan](http://elinux.org/R-Pi_Troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="9b458-118">For information about solving common problems on Raspberry Pi 3, see the [official troubleshooting page](http://elinux.org/R-Pi_Troubleshooting).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="9b458-119">Problem med node.js-paket</span><span class="sxs-lookup"><span data-stu-id="9b458-119">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="9b458-120">Inget svar under gulp uppgifter</span><span class="sxs-lookup"><span data-stu-id="9b458-120">No response during gulp tasks</span></span>
<span data-ttu-id="9b458-121">Om du får problem med att köra gulp uppgifter, kan du lägga till den `--verbose` alternativet för felsökning.</span><span class="sxs-lookup"><span data-stu-id="9b458-121">If you encounter problems running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="9b458-122">Försöker avsluta aktuella gulp uppgifter med hjälp av `Ctrl + C`, och kör sedan följande kommando i din konsolfönster Se felsökningsmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="9b458-122">Try to terminate current gulp tasks by using `Ctrl + C`, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="9b458-123">Du kan visa detaljerade felmeddelanden i konsolens utdata.</span><span class="sxs-lookup"><span data-stu-id="9b458-123">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="9b458-124">Problem vid identifiering av enheter</span><span class="sxs-lookup"><span data-stu-id="9b458-124">Device discovery issues</span></span>
<span data-ttu-id="9b458-125">Felsökning av vanliga problem med den `devdisco` kommando, kontrollera den [viktigt](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span><span class="sxs-lookup"><span data-stu-id="9b458-125">For help in troubleshooting common problems with the `devdisco` command, check the [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="9b458-126">NPM-problem</span><span class="sxs-lookup"><span data-stu-id="9b458-126">NPM issues</span></span>
<span data-ttu-id="9b458-127">Försök att uppdatera NPM-paket med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9b458-127">Try to update your NPM package with the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="9b458-128">Om problemet kvarstår lämna kommentarer i slutet av den här artikeln eller skapa ett GitHub-problem i våra [exempel databasen](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)</span><span class="sxs-lookup"><span data-stu-id="9b458-128">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="9b458-129">Fjärrfelsökning</span><span class="sxs-lookup"><span data-stu-id="9b458-129">Remote debugging</span></span>

<span data-ttu-id="9b458-130">Remote felsökning support blir snart tillgänglig i Visual Studio Code C/C++-tillägget.</span><span class="sxs-lookup"><span data-stu-id="9b458-130">Remote debugging support will be available soon in Visual Studio Code C/C++ Extension.</span></span>

<span data-ttu-id="9b458-131">Du kan använda GDB via favorit SSH terminalen i en användarsystem:</span><span class="sxs-lookup"><span data-stu-id="9b458-131">In a meanwhile you can use GDB via your favourite SSH terminal:</span></span>

```bash
cd c-pi-lesson-x
sudo gdb app
```

## <a name="azure-cli-issues"></a><span data-ttu-id="9b458-132">Problem med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9b458-132">Azure-CLI issues</span></span>
<span data-ttu-id="9b458-133">Azure-kommandoradsgränssnittet (Azure CLI) är en förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="9b458-133">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="9b458-134">Leta efter en lösning i den [Preview installera guiden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) att söka efter lösningar.</span><span class="sxs-lookup"><span data-stu-id="9b458-134">Look for solution in the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) to seek solutions.</span></span> <span data-ttu-id="9b458-135">Försök att uppgradera Azure cli till senaste versionen när kommandon inte fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="9b458-135">Try to upgrade Azure-cli to latest version when commands don’t work as expected.</span></span>

<span data-ttu-id="9b458-136">Om det uppstår några fel med verktyget filen ett [problemet](https://github.com/Azure/azure-cli/issues) i den **problem** avsnitt i GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="9b458-136">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="9b458-137">Hjälp med att felsöka vanliga problem, kontrollera den [viktigt](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="9b458-137">For help troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="9b458-138">Kör följande kommando för att uppgradera pip till senaste versionen om du uppfyller ”det gick inte att hitta en version som uppfyller krav”.</span><span class="sxs-lookup"><span data-stu-id="9b458-138">If you meet "Could not find a version that satisfies the requirement", please run the following command to upgrade pip to lastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="9b458-139">Problem med installation av Python</span><span class="sxs-lookup"><span data-stu-id="9b458-139">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="9b458-140">Äldre installationsproblem (macOS)</span><span class="sxs-lookup"><span data-stu-id="9b458-140">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="9b458-141">När du installerar **pip**, en behörighet felmeddelande när äldre paket som är installerade med **su** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="9b458-141">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="9b458-142">Detta inträffar eftersom en tidigare installation av Python med brew (macOS) inte har avinstallerats helt.</span><span class="sxs-lookup"><span data-stu-id="9b458-142">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="9b458-143">Vissa **pip** paket från en tidigare installation har skapats av rot, som orsakar felet behörighet.</span><span class="sxs-lookup"><span data-stu-id="9b458-143">Some **pip** packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="9b458-144">Lösningen är att ta bort de paket som installerats av roten.</span><span class="sxs-lookup"><span data-stu-id="9b458-144">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="9b458-145">Använd följande steg för att slutföra den här aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="9b458-145">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="9b458-146">Gå till: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="9b458-146">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="9b458-147">Lista paket skapa av roten:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="9b458-147">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="9b458-148">Avinstallera paket från steg 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="9b458-148">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="9b458-149">Installera om Python.</span><span class="sxs-lookup"><span data-stu-id="9b458-149">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="9b458-150">Azure IoT-hubb-problem</span><span class="sxs-lookup"><span data-stu-id="9b458-150">Azure IoT Hub issues</span></span>
<span data-ttu-id="9b458-151">Om du har etablerats dina Azure IoT-hubb med `azure-cli`, och du behöver ett verktyg för att hantera enheter som ansluter till din IoT-hubb kan du prova följande verktyg:</span><span class="sxs-lookup"><span data-stu-id="9b458-151">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="9b458-152">Enheten Explorer</span><span class="sxs-lookup"><span data-stu-id="9b458-152">Device Explorer</span></span>
<span data-ttu-id="9b458-153">[Enheten Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) körs på den lokala Windows-datorn och ansluter till din IoT-hubb i Azure.</span><span class="sxs-lookup"><span data-stu-id="9b458-153">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="9b458-154">Den kommunicerar med följande [IoT-hubbslutpunkter](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="9b458-154">It communicates with the following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

* <span data-ttu-id="9b458-155">*Enheten Identitetshantering* att etablera och hantera enheter som registrerats med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="9b458-155">*Device identity management* to provision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="9b458-156">*Ta emot enhet till moln* så att du kan övervaka meddelanden som skickas från enheten till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9b458-156">*Receive device-to-cloud* so you can monitor messages sent from your device to your IoT hub.</span></span>
* <span data-ttu-id="9b458-157">*Skicka moln till enhet* så att du kan skicka meddelanden till dina enheter från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9b458-157">*Send cloud-to-device* so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="9b458-158">Konfigurera din `IoT hub connection string` i det här verktyget och använda dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="9b458-158">Configure your `IoT hub connection string` within this tool to use all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="9b458-159">IoT-hubb Explorer</span><span class="sxs-lookup"><span data-stu-id="9b458-159">IoT hub Explorer</span></span>
<span data-ttu-id="9b458-160">[IoT-hubb Explorer](https://github.com/Azure/iothub-explorer) är ett exempel flera plattformar CLI verktyg för att hantera klienter.</span><span class="sxs-lookup"><span data-stu-id="9b458-160">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage device clients.</span></span> <span data-ttu-id="9b458-161">Du kan använda verktyget för att hantera enheter i identitetsregistret, övervaka meddelanden från enhet till moln och skicka kommandon moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="9b458-161">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="9b458-162">För att installera den senaste (förhandsversion) versionen av verktyget iothub explorer, kör du följande kommando i Kommandotolken miljön:</span><span class="sxs-lookup"><span data-stu-id="9b458-162">To install the latest (prerelease) version of the iothub-explorer tool, run the following command in your command-line environment:</span></span>

```
npm install -g iothub-explorer@latest
```

<span data-ttu-id="9b458-163">Du kan använda följande kommando för att få ytterligare hjälp om alla iothub explorer kommandon och deras parametrar:</span><span class="sxs-lookup"><span data-stu-id="9b458-163">You can use the following command to get additional help about all the iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="9b458-164">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9b458-164">Azure portal</span></span>
<span data-ttu-id="9b458-165">En fullständig CLI-upplevelse hjälper dig att skapa och hantera alla dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="9b458-165">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="9b458-166">Du kanske också vill använda den [Azure-portalen](../azure-portal-overview.md) för att etablera, hantera och felsöka Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="9b458-166">You might also want to use the [Azure portal](../azure-portal-overview.md) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="9b458-167">Problem med Azure-lagring</span><span class="sxs-lookup"><span data-stu-id="9b458-167">Azure storage issues</span></span>
<span data-ttu-id="9b458-168">[Microsoft Azure Lagringsutforskaren (förhandsversion)](http://storageexplorer.com) är en fristående app från Microsoft som du kan använda för att arbeta med Azure Storage data på Windows och macOS Linux.</span><span class="sxs-lookup"><span data-stu-id="9b458-168">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use to work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="9b458-169">Med det här verktyget kan du ansluta till din tabell och visa data i den.</span><span class="sxs-lookup"><span data-stu-id="9b458-169">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="9b458-170">Du kan använda det här verktyget för att felsöka problem med ditt Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9b458-170">You can use this tool to troubleshoot your Azure Storage issues.</span></span>
