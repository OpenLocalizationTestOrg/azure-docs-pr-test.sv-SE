---
title: Installera Azure CLI 1.0 | Microsoft Docs
description: "Installera Azure CLI 1.0 för Mac, Linux och Windows att börja använda Azure-tjänster"
editor: 
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: bdb776c8-7a76-4f3a-887c-236b4fffee10
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: command-line-interface
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: rasquill
ms.openlocfilehash: 63b35ed25b809a16b61b685fd35aa67474b0a369
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-the-azure-cli-10"></a><span data-ttu-id="d1a5f-103">Installera Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d1a5f-103">Install the Azure CLI 1.0</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d1a5f-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d1a5f-104">PowerShell</span></span>](/powershell/azure/overview)
> * [<span data-ttu-id="d1a5f-105">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d1a5f-105">Azure CLI 1.0</span></span>](cli-install-nodejs.md)
> * [<span data-ttu-id="d1a5f-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d1a5f-106">Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

> [!IMPORTANT]
> <span data-ttu-id="d1a5f-107">Det här avsnittet beskriver hur du installerar Azure CLI 1.0, som bygger på nodeJs och stöder alla klassisk distribution API-anrop, samt ett stort antal aktiviteter för Resource Manager distribution.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-107">This topic describes how to install the Azure CLI 1.0, which is built on nodeJs and supports all classic deployment API calls as well as a large number of Resource Manager deployment activities.</span></span> <span data-ttu-id="d1a5f-108">Du bör använda den [Azure CLI 2.0](/cli/azure/overview) för nya eller framtida CLI-distribution och hantering.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-108">You should use the [Azure CLI 2.0](/cli/azure/overview) for new or forward-looking CLI deployments and management.</span></span>

<span data-ttu-id="d1a5f-109">Snabbt installera Azure-kommandoradsgränssnittet (Azure CLI 1.0) om du vill använda en uppsättning med öppen källkod shell-baserade kommandon för att skapa och hantera resurser i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-109">Quickly install the Azure Command-Line Interface (Azure CLI 1.0) to use a set of open-source shell-based commands for creating and managing resources in Microsoft Azure.</span></span> <span data-ttu-id="d1a5f-110">Du har flera alternativ för att installera verktygen plattformsoberoende på datorn:</span><span class="sxs-lookup"><span data-stu-id="d1a5f-110">You have several options to install these cross-platform tools on your computer:</span></span>

* <span data-ttu-id="d1a5f-111">**npm paketet** -kör npm (package manager för JavaScript) för att installera det senaste Azure CLI 1.0-paketet på din distribution av Linux eller OS.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-111">**npm package** - Run npm (the package manager for JavaScript) to install the latest Azure CLI 1.0 package on your Linux distribution or OS.</span></span> <span data-ttu-id="d1a5f-112">Kräver node.js och npm på datorn.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-112">Requires node.js and npm on your computer.</span></span>
* <span data-ttu-id="d1a5f-113">**Installer** -hämta ett installationsprogram för enkel installation på Mac- eller Windows.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-113">**Installer** - Download an installer for easy installation on Mac or Windows.</span></span>
* <span data-ttu-id="d1a5f-114">**Dockerbehållare** - Start med hjälp av senaste CLI i en klar och kör dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-114">**Docker container** - Start using the latest CLI in a ready-to-run Docker container.</span></span> <span data-ttu-id="d1a5f-115">Kräver Docker värden på datorn.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-115">Requires Docker host on your computer.</span></span>

<span data-ttu-id="d1a5f-116">Fler alternativ och bakgrunden finns i databasen projektet på [GitHub](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="d1a5f-116">For more options and background, see the project repository on [GitHub](https://github.com/azure/azure-xplat-cli).</span></span>

<span data-ttu-id="d1a5f-117">När du har installerat Azure CLI 1.0 [ansluta till din Azure-prenumeration](xplat-cli-connect.md) och kör den **azure** kommandon från din kommandoradsgränssnittet (Bash, Terminal, Kommandotolken och så vidare) att arbeta med din Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-117">Once the Azure CLI 1.0 is installed, [connect it with your Azure subscription](xplat-cli-connect.md) and run the **azure** commands from your command-line interface (Bash, Terminal, Command prompt, and so on) to work with your Azure resources.</span></span>

## <a name="option-1-install-an-npm-package"></a><span data-ttu-id="d1a5f-118">Alternativ 1: Installera npm-paket</span><span class="sxs-lookup"><span data-stu-id="d1a5f-118">Option 1: Install an npm package</span></span>
<span data-ttu-id="d1a5f-119">Om du vill installera CLI från ett npm-paket, kontrollera att du har hämtat och installerat den [senaste Node.js och npm](https://nodejs.org/en/download/package-manager/).</span><span class="sxs-lookup"><span data-stu-id="d1a5f-119">To install the CLI from an npm package, make sure you have downloaded and installed the [latest Node.js and npm](https://nodejs.org/en/download/package-manager/).</span></span> <span data-ttu-id="d1a5f-120">Kör sedan **installera npm** att installera azure cli-paketet:</span><span class="sxs-lookup"><span data-stu-id="d1a5f-120">Then, run **npm install** to install the azure-cli package:</span></span>

```bash
npm install -g azure-cli
```

<span data-ttu-id="d1a5f-121">På Linux-distributioner, kan du behöva använda **sudo** för att köra den **npm** kommandot på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d1a5f-121">On Linux distributions, you might need to use **sudo** to successfully run the **npm** command, as follows:</span></span>

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> <span data-ttu-id="d1a5f-122">Om du behöver installera eller uppdatera Node.js och npm på Linux-distribution eller OS, rekommenderar vi att du installerar den senaste versionen för Node.js LTS (4.x).</span><span class="sxs-lookup"><span data-stu-id="d1a5f-122">If you need to install or update Node.js and npm on your Linux distribution or OS, we recommend that you install the most recent Node.js LTS version (4.x).</span></span> <span data-ttu-id="d1a5f-123">Om du använder en äldre version, kan du hämta installationsfel.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-123">If you use an older version, you might get installation errors.</span></span>

<span data-ttu-id="d1a5f-124">Om du vill hämta den senaste Linux [tar-filen] [ linux-installer] för npm paketet lokalt.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-124">If you prefer, download the latest Linux [tar file][linux-installer] for the npm package locally.</span></span> <span data-ttu-id="d1a5f-125">Installera sedan hämtade npm-paketet enligt följande (på Linux-distributioner som du kan behöva använda **sudo**):</span><span class="sxs-lookup"><span data-stu-id="d1a5f-125">Then, install the downloaded npm package as follows (on Linux distributions you might need to use **sudo**):</span></span>

```bash
npm install -g <path to downloaded tar file>
```

## <a name="option-2-use-an-installer"></a><span data-ttu-id="d1a5f-126">Alternativ 2: Använda ett installationsprogram</span><span class="sxs-lookup"><span data-stu-id="d1a5f-126">Option 2: Use an installer</span></span>
<span data-ttu-id="d1a5f-127">Om du använder en Mac- eller Windows-dator är följande CLI installationsprogram hämtas:</span><span class="sxs-lookup"><span data-stu-id="d1a5f-127">If you use a Mac or Windows computer, the following CLI installers are available for download:</span></span>

* <span data-ttu-id="d1a5f-128">[Mac OS X-installationsprogrammet][mac-installer]</span><span class="sxs-lookup"><span data-stu-id="d1a5f-128">[Mac OS X installer][mac-installer]</span></span>
* <span data-ttu-id="d1a5f-129">[Windows MSI][windows-installer]</span><span class="sxs-lookup"><span data-stu-id="d1a5f-129">[Windows MSI][windows-installer]</span></span>

> [!TIP]
> <span data-ttu-id="d1a5f-130">I Windows, kan du också hämta de [installationsprogram för webbplattform](https://go.microsoft.com/?linkid=9828653) installera CLI.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-130">On Windows, you can also download the [Web Platform Installer](https://go.microsoft.com/?linkid=9828653) to install the CLI.</span></span> <span data-ttu-id="d1a5f-131">Det här installationsprogrammet kan du välja att installera ytterligare Azure SDK och kommandoradsverktygen när du har installerat CLI.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-131">This installer gives you the option to install additional Azure SDK and command-line tools after installing the CLI.</span></span>

## <a name="option-3-use-a-docker-container"></a><span data-ttu-id="d1a5f-132">Alternativ 3: Använda en dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="d1a5f-132">Option 3: Use a Docker container</span></span>
<span data-ttu-id="d1a5f-133">Om du har konfigurerat datorn som en [Docker](https://docs.docker.com/engine/understanding-docker/) värd, kan du köra den senaste Azure CLI 1.0 i en dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-133">If you have set up your computer as a [Docker](https://docs.docker.com/engine/understanding-docker/) host, you can run the latest Azure CLI 1.0 in a Docker container.</span></span> <span data-ttu-id="d1a5f-134">Kör följande kommando (på Linux-distributioner som du kan behöva använda **sudo**):</span><span class="sxs-lookup"><span data-stu-id="d1a5f-134">Run the following command (on Linux distributions you might need to use **sudo**):</span></span>

```bash
docker run -it microsoft/azure-cli
```

## <a name="run-azure-cli-10-commands"></a><span data-ttu-id="d1a5f-135">Kör kommandon för Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d1a5f-135">Run Azure CLI 1.0 commands</span></span>
<span data-ttu-id="d1a5f-136">När Azure CLI 1.0 installeras, kör du den **azure** från kommandoraden användargränssnittet (Bash, Terminal, Kommandotolken och så vidare).</span><span class="sxs-lookup"><span data-stu-id="d1a5f-136">After the Azure CLI 1.0 is installed, run the **azure** command from your command-line user interface (Bash, Terminal, Command prompt, and so on).</span></span> <span data-ttu-id="d1a5f-137">Till exempel för att köra kommandot help, skriver du följande:</span><span class="sxs-lookup"><span data-stu-id="d1a5f-137">For example, to run the help command, type the following:</span></span>

```azurecli
azure help
```

> [!NOTE]
> <span data-ttu-id="d1a5f-138">På vissa Linux-distributioner kan du få ett felmeddelande liknande `/usr/bin/env: ‘node’: No such file or directory`.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-138">On some Linux distributions, you may receive an error similar to `/usr/bin/env: ‘node’: No such file or directory`.</span></span> <span data-ttu-id="d1a5f-139">Det här felet kommer från nya installationer av Node.js som installeras på /usr/bin/nodejs.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-139">This error comes from recent installations of Node.js being installed at /usr/bin/nodejs.</span></span> <span data-ttu-id="d1a5f-140">Åtgärda det genom att skapa en symbolisk länk till /usr/bin/node genom att köra det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="d1a5f-140">To fix it, create a symbolic link to /usr/bin/node by running this command:</span></span>

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

<span data-ttu-id="d1a5f-141">Om du vill se versionen av Azure CLI 1.0 som du har installerat, skriver du följande:</span><span class="sxs-lookup"><span data-stu-id="d1a5f-141">To see the version of the Azure CLI 1.0 you installed, type the following:</span></span>

```azurecli
azure --version
```

<span data-ttu-id="d1a5f-142">Nu är du klar!</span><span class="sxs-lookup"><span data-stu-id="d1a5f-142">Now you are ready!</span></span> <span data-ttu-id="d1a5f-143">Åtkomst till alla CLI-kommandona ska fungera med egna resurser, [ansluta till din Azure-prenumeration från Azure CLI](xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="d1a5f-143">To access all the CLI commands to work with your own resources, [connect to your Azure subscription from the Azure CLI](xplat-cli-connect.md).</span></span>

> [!NOTE]
> <span data-ttu-id="d1a5f-144">När du börjar använda Azure CLI, visas ett meddelande som frågar om du vill att Microsoft ska kunna samla in information om användning.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-144">When you first use Azure CLI, you see a message asking if you want to allow Microsoft to collect usage information.</span></span> <span data-ttu-id="d1a5f-145">Det är frivilligt att delta.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-145">Participation is voluntary.</span></span> <span data-ttu-id="d1a5f-146">Om du väljer att delta, du kan stoppa när som helst genom att köra `azure telemetry --disable`.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-146">If you choose to participate, you can stop at any time by running `azure telemetry --disable`.</span></span> <span data-ttu-id="d1a5f-147">Om du vill aktivera delta när som helst köra `azure telemetry --enable`.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-147">To enable participation at any time, run `azure telemetry --enable`.</span></span>

## <a name="update-the-cli"></a><span data-ttu-id="d1a5f-148">Uppdatera CLI</span><span class="sxs-lookup"><span data-stu-id="d1a5f-148">Update the CLI</span></span>
<span data-ttu-id="d1a5f-149">Microsoft släpper ofta uppdaterade versioner av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-149">Microsoft frequently releases updated versions of the Azure CLI.</span></span> <span data-ttu-id="d1a5f-150">Installera om CLI med hjälp av installationsprogrammet för operativsystemet eller kör den senaste dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-150">Reinstall the CLI using the installer for your operating system, or run the latest Docker container.</span></span> <span data-ttu-id="d1a5f-151">Om du har de senaste Node.js och npm installerad, uppdatera genom att skriva följande (på Linux-distributioner som du kan behöva använda **sudo**).</span><span class="sxs-lookup"><span data-stu-id="d1a5f-151">Or, if you have the latest Node.js and npm installed, update by typing the following (on Linux distributions you might need to use **sudo**).</span></span>

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a><span data-ttu-id="d1a5f-152">Aktivera flikavslutande</span><span class="sxs-lookup"><span data-stu-id="d1a5f-152">Enable tab completion</span></span>
<span data-ttu-id="d1a5f-153">Flikslutförande av CLI-kommandon stöds för Mac- och Linux.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-153">Tab completion of CLI commands is supported for Mac and Linux.</span></span>

<span data-ttu-id="d1a5f-154">Om du vill aktivera den i zsh, kör du:</span><span class="sxs-lookup"><span data-stu-id="d1a5f-154">To enable it in zsh, run:</span></span>

```bash
echo '. <(azure --completion)' >> .zshrc
```

<span data-ttu-id="d1a5f-155">Om du vill aktivera den i bash, kör du:</span><span class="sxs-lookup"><span data-stu-id="d1a5f-155">To enable it in bash, run:</span></span>

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a><span data-ttu-id="d1a5f-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d1a5f-156">Next steps</span></span>
* <span data-ttu-id="d1a5f-157">[Ansluta från CLI på Azure-prenumerationen](xplat-cli-connect.md) att skapa och hantera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="d1a5f-157">[Connect from the CLI to your Azure subscription](xplat-cli-connect.md) to create and manage Azure resources.</span></span>
* <span data-ttu-id="d1a5f-158">Om du vill veta mer om Azure CLI, ladda ned källkoden rapportera problem eller bidra till projektet, finns det [GitHub-lagringsplatsen för Azure CLI](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="d1a5f-158">To learn more about the Azure CLI, download source code, report problems, or contribute to the project, visit the [GitHub repository for the Azure CLI](https://github.com/azure/azure-xplat-cli).</span></span>
* <span data-ttu-id="d1a5f-159">Om du har frågor om hur du använder Azure CLI eller Azure finns i [Azure forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span><span class="sxs-lookup"><span data-stu-id="d1a5f-159">If you have questions about using the Azure CLI, or Azure, visit the [Azure Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span></span>


[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: /cli/azure/get-started-with-az-cli2
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
