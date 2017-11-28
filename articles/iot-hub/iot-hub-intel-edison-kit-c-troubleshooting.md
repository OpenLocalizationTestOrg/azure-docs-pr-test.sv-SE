---
title: "Ansluta Intel Edison (C) till Azure IoT - felsökning | Microsoft Docs"
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
ms.openlocfilehash: dd6338ad29e0bb858c33e5bb24b8f41d3c22575a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="72f26-104">Felsökning</span><span class="sxs-lookup"><span data-stu-id="72f26-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="72f26-105">Problem med maskinvara</span><span class="sxs-lookup"><span data-stu-id="72f26-105">Hardware issues</span></span>
<span data-ttu-id="72f26-106">Information om hur du löser vanliga problem på Intel modern finns i [officiella felsökning sidan](https://software.intel.com/en-us/node/637974).</span><span class="sxs-lookup"><span data-stu-id="72f26-106">For information about solving common problems on Intel Edison, see the [official troubleshooting page](https://software.intel.com/en-us/node/637974).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="72f26-107">Problem med node.js-paket</span><span class="sxs-lookup"><span data-stu-id="72f26-107">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="72f26-108">Inget svar under gulp uppgifter</span><span class="sxs-lookup"><span data-stu-id="72f26-108">No response during gulp tasks</span></span>
<span data-ttu-id="72f26-109">Om du får problem med att köra gulp uppgifter, kan du lägga till den `--verbose` alternativet för felsökning.</span><span class="sxs-lookup"><span data-stu-id="72f26-109">If you encounter problems running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="72f26-110">Försöker avsluta aktuella gulp uppgifter med hjälp av `Ctrl + C`, och kör sedan följande kommando i din konsolfönster Se felsökningsmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="72f26-110">Try to terminate current gulp tasks by using `Ctrl + C`, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="72f26-111">Du kan visa detaljerade felmeddelanden i konsolens utdata.</span><span class="sxs-lookup"><span data-stu-id="72f26-111">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="npm-issues"></a><span data-ttu-id="72f26-112">NPM-problem</span><span class="sxs-lookup"><span data-stu-id="72f26-112">NPM issues</span></span>
<span data-ttu-id="72f26-113">Försök att uppdatera NPM-paket med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="72f26-113">Try to update your NPM package with the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="72f26-114">Om problemet kvarstår lämna kommentarer i slutet av den här artikeln eller skapa ett GitHub-problem i våra [exempel databasen][sample-repository].</span><span class="sxs-lookup"><span data-stu-id="72f26-114">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository][sample-repository].</span></span>

## <a name="azure-cli-issues"></a><span data-ttu-id="72f26-115">Problem med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="72f26-115">Azure-CLI issues</span></span>
<span data-ttu-id="72f26-116">Azure-kommandoradsgränssnittet (Azure CLI) är en förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="72f26-116">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="72f26-117">Leta efter en lösning i den [Preview installera guiden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) att söka efter lösningar.</span><span class="sxs-lookup"><span data-stu-id="72f26-117">Look for solution in the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) to seek solutions.</span></span> <span data-ttu-id="72f26-118">Försök att uppgradera Azure cli till senaste versionen när kommandon inte fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="72f26-118">Try to upgrade Azure-cli to latest version when commands don’t work as expected.</span></span>

<span data-ttu-id="72f26-119">Om det uppstår några fel med verktyget filen ett [problemet](https://github.com/Azure/azure-cli/issues) i den **problem** avsnitt i GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="72f26-119">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="72f26-120">Hjälp med att felsöka vanliga problem, kontrollera den [viktigt](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="72f26-120">For help troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="72f26-121">Kör följande kommando för att uppgradera pip till senaste versionen om du uppfyller ”det gick inte att hitta en version som uppfyller krav”.</span><span class="sxs-lookup"><span data-stu-id="72f26-121">If you meet "Could not find a version that satisfies the requirement", please run the following command to upgrade pip to lastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="72f26-122">Problem med installation av Python</span><span class="sxs-lookup"><span data-stu-id="72f26-122">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="72f26-123">Äldre installationsproblem (macOS)</span><span class="sxs-lookup"><span data-stu-id="72f26-123">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="72f26-124">När du installerar **pip**, en behörighet felmeddelande när äldre paket som är installerade med **su** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="72f26-124">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="72f26-125">Detta inträffar eftersom en tidigare installation av Python med brew (macOS) inte har avinstallerats helt.</span><span class="sxs-lookup"><span data-stu-id="72f26-125">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="72f26-126">Vissa **pip** paket från en tidigare installation har skapats av rot, som orsakar felet behörighet.</span><span class="sxs-lookup"><span data-stu-id="72f26-126">Some **pip** packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="72f26-127">Lösningen är att ta bort de paket som installerats av roten.</span><span class="sxs-lookup"><span data-stu-id="72f26-127">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="72f26-128">Använd följande steg för att slutföra den här aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="72f26-128">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="72f26-129">Gå till: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="72f26-129">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="72f26-130">Lista paket skapa av roten:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="72f26-130">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="72f26-131">Avinstallera paket från steg 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="72f26-131">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="72f26-132">Installera om Python.</span><span class="sxs-lookup"><span data-stu-id="72f26-132">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="72f26-133">Azure IoT-hubb-problem</span><span class="sxs-lookup"><span data-stu-id="72f26-133">Azure IoT Hub issues</span></span>
<span data-ttu-id="72f26-134">Om du har etablerats dina Azure IoT-hubb med `azure-cli`, och du behöver ett verktyg för att hantera enheter som ansluter till din IoT-hubb kan du prova följande verktyg:</span><span class="sxs-lookup"><span data-stu-id="72f26-134">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="72f26-135">Enheten Explorer</span><span class="sxs-lookup"><span data-stu-id="72f26-135">Device Explorer</span></span>
<span data-ttu-id="72f26-136">[Enheten Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) körs på den lokala Windows-datorn och ansluter till din IoT-hubb i Azure.</span><span class="sxs-lookup"><span data-stu-id="72f26-136">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="72f26-137">Den kommunicerar med följande [IoT-hubbslutpunkter](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="72f26-137">It communicates with the following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

- <span data-ttu-id="72f26-138">_Enheten Identitetshantering_ att etablera och hantera enheter som registrerats med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="72f26-138">_Device identity management_ to provision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="72f26-139">_Ta emot enhet till moln_ så att du kan övervaka meddelanden som skickas från enheten till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="72f26-139">_Receive device-to-cloud_ so you can monitor messages sent from your device to your IoT hub.</span></span>
- <span data-ttu-id="72f26-140">_Skicka moln till enhet_ så att du kan skicka meddelanden till dina enheter från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="72f26-140">_Send cloud-to-device_ so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="72f26-141">Konfigurera din `IoT hub connection string` i det här verktyget och använda dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="72f26-141">Configure your `IoT hub connection string` within this tool to use all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="72f26-142">IoT-hubb Explorer</span><span class="sxs-lookup"><span data-stu-id="72f26-142">IoT hub Explorer</span></span>
<span data-ttu-id="72f26-143">[IoT-hubb Explorer](https://github.com/Azure/iothub-explorer) är ett exempel flera plattformar CLI verktyg för att hantera klienter.</span><span class="sxs-lookup"><span data-stu-id="72f26-143">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage device clients.</span></span> <span data-ttu-id="72f26-144">Du kan använda verktyget för att hantera enheter i identitetsregistret, övervaka meddelanden från enhet till moln och skicka kommandon moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="72f26-144">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="72f26-145">För att installera den senaste (förhandsversion) versionen av verktyget iothub explorer, kör du följande kommando i Kommandotolken miljön:</span><span class="sxs-lookup"><span data-stu-id="72f26-145">To install the latest (prerelease) version of the iothub-explorer tool, run the following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="72f26-146">Du kan använda följande kommando för att få ytterligare hjälp om alla iothub explorer kommandon och deras parametrar:</span><span class="sxs-lookup"><span data-stu-id="72f26-146">You can use the following command to get additional help about all the iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="72f26-147">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="72f26-147">Azure portal</span></span>
<span data-ttu-id="72f26-148">En fullständig CLI-upplevelse hjälper dig att skapa och hantera alla dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="72f26-148">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="72f26-149">Du kanske också vill använda den [Azure-portalen](../azure-portal-overview.md) för att etablera, hantera och felsöka Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="72f26-149">You might also want to use the [Azure portal](../azure-portal-overview.md) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="72f26-150">Problem med Azure-lagring</span><span class="sxs-lookup"><span data-stu-id="72f26-150">Azure storage issues</span></span>
<span data-ttu-id="72f26-151">[Microsoft Azure Lagringsutforskaren (förhandsversion)](http://storageexplorer.com) är en fristående app från Microsoft som du kan använda för att arbeta med [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) data på Windows-, macOS- och Linux.</span><span class="sxs-lookup"><span data-stu-id="72f26-151">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use to work with [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="72f26-152">Med det här verktyget kan du ansluta till din tabell och visa data i den.</span><span class="sxs-lookup"><span data-stu-id="72f26-152">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="72f26-153">Du kan använda det här verktyget för att felsöka problem med ditt Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="72f26-153">You can use this tool to troubleshoot your Azure Storage issues.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72f26-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="72f26-154">Next steps</span></span>
<span data-ttu-id="72f26-155">Den här sidan innehåller bara de vanligaste problemen med Intel modern kit.</span><span class="sxs-lookup"><span data-stu-id="72f26-155">This page only includes the most common problems of Intel Edison kit.</span></span> <span data-ttu-id="72f26-156">Du kan också lämna kommentarer längst ned och rapportera problem felsökningsinformation.</span><span class="sxs-lookup"><span data-stu-id="72f26-156">You can also leave bottom comments to report issues for further troubleshooting.</span></span>

<span data-ttu-id="72f26-157">Gå tillbaka till [Kom igång med Intel Edison (C)](iot-hub-intel-edison-kit-c-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="72f26-157">Go back to [Get started with Intel Edison (C)](iot-hub-intel-edison-kit-c-get-started.md)</span></span>

<!-- Images and links -->

[sample-repository]: https://github.com/Azure-Samples/iot-hub-c-edison-getting-started