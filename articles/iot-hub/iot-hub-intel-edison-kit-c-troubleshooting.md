---
title: "Connect Intel EDISON (C) tooAzure IoT - felsökning | Microsoft Docs"
description: "Felsökning för Intel modern C-upplevelse"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Felsökning av arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: fe20f2fe-490c-4910-82e1-578ed504ae86
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c4a71983e3906cfc5ce7c832cf858852b9bd744a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="93237-104">Felsökning</span><span class="sxs-lookup"><span data-stu-id="93237-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="93237-105">Problem med maskinvara</span><span class="sxs-lookup"><span data-stu-id="93237-105">Hardware issues</span></span>
<span data-ttu-id="93237-106">Information om hur du löser vanliga problem på Intel modern finns hello [officiella felsökning sidan](https://software.intel.com/en-us/node/637974).</span><span class="sxs-lookup"><span data-stu-id="93237-106">For information about solving common problems on Intel Edison, see hello [official troubleshooting page](https://software.intel.com/en-us/node/637974).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="93237-107">Problem med node.js-paket</span><span class="sxs-lookup"><span data-stu-id="93237-107">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="93237-108">Inget svar under gulp uppgifter</span><span class="sxs-lookup"><span data-stu-id="93237-108">No response during gulp tasks</span></span>
<span data-ttu-id="93237-109">Om du stöter på problem med att köra gulp uppgifter kan du lägga till hello `--verbose` alternativet för felsökning.</span><span class="sxs-lookup"><span data-stu-id="93237-109">If you encounter problems running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="93237-110">Försök tooterminate aktuella gulp uppgifter med hjälp av `Ctrl + C`, och sedan kör hello följande kommando i konsolen för fönstret toosee felsökningsmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="93237-110">Try tooterminate current gulp tasks by using `Ctrl + C`, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="93237-111">Du kan visa detaljerade felmeddelanden i konsolens utdata.</span><span class="sxs-lookup"><span data-stu-id="93237-111">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="npm-issues"></a><span data-ttu-id="93237-112">NPM-problem</span><span class="sxs-lookup"><span data-stu-id="93237-112">NPM issues</span></span>
<span data-ttu-id="93237-113">Försök tooupdate NPM-paketet med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="93237-113">Try tooupdate your NPM package with hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="93237-114">Om hello problemet fortfarande kvarstår lämna kommentarer hello slutet av den här artikeln eller skapa ett GitHub-problem i våra [exempel databasen][sample-repository].</span><span class="sxs-lookup"><span data-stu-id="93237-114">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository][sample-repository].</span></span>

## <a name="azure-cli-issues"></a><span data-ttu-id="93237-115">Problem med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="93237-115">Azure-CLI issues</span></span>
<span data-ttu-id="93237-116">hello Azure-kommandoradsgränssnittet (Azure CLI) är en förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="93237-116">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="93237-117">Leta efter en lösning i hello [Preview installera guiden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek lösningar.</span><span class="sxs-lookup"><span data-stu-id="93237-117">Look for solution in hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek solutions.</span></span> <span data-ttu-id="93237-118">Försök tooupgrade Azure cli toolatest version när kommandon inte fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="93237-118">Try tooupgrade Azure-cli toolatest version when commands don’t work as expected.</span></span>

<span data-ttu-id="93237-119">Om det uppstår några buggar med hello verktyget filen ett [problemet](https://github.com/Azure/azure-cli/issues) i hello **problem** avsnitt i GitHub-repo-hello.</span><span class="sxs-lookup"><span data-stu-id="93237-119">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="93237-120">För hjälp med att felsöka vanliga problem, kontrollera hello [viktigt](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="93237-120">For help troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="93237-121">Om du uppfyller ”det gick inte att hitta en version som uppfyller krav på hello”, ange kommando hello kör följande tooupgrade pip toolastest version.</span><span class="sxs-lookup"><span data-stu-id="93237-121">If you meet "Could not find a version that satisfies hello requirement", please run hello following command tooupgrade pip toolastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="93237-122">Problem med installation av Python</span><span class="sxs-lookup"><span data-stu-id="93237-122">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="93237-123">Äldre installationsproblem (macOS)</span><span class="sxs-lookup"><span data-stu-id="93237-123">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="93237-124">När du installerar **pip**, en behörighet felmeddelande när äldre paket som är installerade med **su** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="93237-124">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="93237-125">Detta inträffar eftersom en tidigare installation av Python med brew (macOS) inte har avinstallerats helt.</span><span class="sxs-lookup"><span data-stu-id="93237-125">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="93237-126">Vissa **pip** paket från en tidigare installation har skapats av roten, vilket gör att hello behörighetsfel.</span><span class="sxs-lookup"><span data-stu-id="93237-126">Some **pip** packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="93237-127">Hej lösningen är tooremove de paket som installerats av roten.</span><span class="sxs-lookup"><span data-stu-id="93237-127">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="93237-128">Använd följande steg toocomplete hello den här aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="93237-128">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="93237-129">Gå till: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="93237-129">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="93237-130">Lista paket skapa av roten:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="93237-130">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="93237-131">Avinstallera paket från steg 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="93237-131">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="93237-132">Installera om Python.</span><span class="sxs-lookup"><span data-stu-id="93237-132">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="93237-133">Azure IoT-hubb-problem</span><span class="sxs-lookup"><span data-stu-id="93237-133">Azure IoT Hub issues</span></span>
<span data-ttu-id="93237-134">Om du har etablerats dina Azure IoT-hubb med `azure-cli`, och du behöver en verktyget toomanage hello enheter som ansluter tooyour IoT hub, försök hello följande verktyg:</span><span class="sxs-lookup"><span data-stu-id="93237-134">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="93237-135">Enheten Explorer</span><span class="sxs-lookup"><span data-stu-id="93237-135">Device Explorer</span></span>
<span data-ttu-id="93237-136">[Enheten Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) körs på den lokala Windows-datorn och ansluter tooyour IoT-hubb i Azure.</span><span class="sxs-lookup"><span data-stu-id="93237-136">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="93237-137">Den kommunicerar med hello följande [IoT-hubbslutpunkter](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="93237-137">It communicates with hello following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

- <span data-ttu-id="93237-138">_Enheten Identitetshantering_ tooprovision och hantera enheter som registrerats med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="93237-138">_Device identity management_ tooprovision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="93237-139">_Ta emot enhet till moln_ så att du kan övervaka meddelanden som skickas från din enhet tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="93237-139">_Receive device-to-cloud_ so you can monitor messages sent from your device tooyour IoT hub.</span></span>
- <span data-ttu-id="93237-140">_Skicka moln till enhet_ så att du kan skicka meddelanden tooyour enheter från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="93237-140">_Send cloud-to-device_ so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="93237-141">Konfigurera din `IoT hub connection string` i det här verktyget toouse dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="93237-141">Configure your `IoT hub connection string` within this tool toouse all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="93237-142">IoT-hubb Explorer</span><span class="sxs-lookup"><span data-stu-id="93237-142">IoT hub Explorer</span></span>
<span data-ttu-id="93237-143">[IoT-hubb Explorer](https://github.com/Azure/iothub-explorer) är ett exempel flera plattformar CLI toomanage enhetsklienter.</span><span class="sxs-lookup"><span data-stu-id="93237-143">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage device clients.</span></span> <span data-ttu-id="93237-144">Du kan använda hello verktyget toomanage hello enheter i hello identitetsregistret, övervaka meddelanden från enhet till moln och skicka kommandon moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="93237-144">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="93237-145">tooinstall hello senaste () förhandsversion av hello iothub explorer verktyg, kör följande kommando i Kommandotolken miljön hello:</span><span class="sxs-lookup"><span data-stu-id="93237-145">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="93237-146">Du kan använda följande kommando tooget mer hjälp om alla hello iothub explorer kommandon och deras parametrar hello:</span><span class="sxs-lookup"><span data-stu-id="93237-146">You can use hello following command tooget additional help about all hello iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="93237-147">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="93237-147">Azure portal</span></span>
<span data-ttu-id="93237-148">En fullständig CLI-upplevelse hjälper dig att skapa och hantera alla dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="93237-148">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="93237-149">Du kan också toouse hello [Azure-portalen](../azure-portal-overview.md) toohelp etablera, hantera och felsöka Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="93237-149">You might also want toouse hello [Azure portal](../azure-portal-overview.md) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="93237-150">Problem med Azure-lagring</span><span class="sxs-lookup"><span data-stu-id="93237-150">Azure storage issues</span></span>
<span data-ttu-id="93237-151">[Microsoft Azure Lagringsutforskaren (förhandsversion)](http://storageexplorer.com) är en fristående app från Microsoft som du kan använda toowork med [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) data på Windows-, macOS- och Linux.</span><span class="sxs-lookup"><span data-stu-id="93237-151">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use toowork with [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="93237-152">Med det här verktyget kan du ansluta tooyour tabell och se hello data i den.</span><span class="sxs-lookup"><span data-stu-id="93237-152">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="93237-153">Du kan använda det här verktyget tootroubleshoot Azure Storage-problem.</span><span class="sxs-lookup"><span data-stu-id="93237-153">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>

## <a name="next-steps"></a><span data-ttu-id="93237-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93237-154">Next steps</span></span>
<span data-ttu-id="93237-155">Den här sidan innehåller endast hello de vanligaste problemen med Intel modern kit.</span><span class="sxs-lookup"><span data-stu-id="93237-155">This page only includes hello most common problems of Intel Edison kit.</span></span> <span data-ttu-id="93237-156">Du kan också lämna kommentarer längst ned tooreport problem för felsökningsinformation.</span><span class="sxs-lookup"><span data-stu-id="93237-156">You can also leave bottom comments tooreport issues for further troubleshooting.</span></span>

<span data-ttu-id="93237-157">Gå tillbaka för[Kom igång med Intel Edison (C)](iot-hub-intel-edison-kit-c-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="93237-157">Go back too[Get started with Intel Edison (C)](iot-hub-intel-edison-kit-c-get-started.md)</span></span>

<!-- Images and links -->

[sample-repository]: https://github.com/Azure-Samples/iot-hub-c-edison-getting-started