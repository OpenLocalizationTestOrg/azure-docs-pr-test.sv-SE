---
title: aaaInstall hello Azure CLI 1.0 | Microsoft Docs
description: "Installera hello Azure CLI 1.0 för Mac, Linux och Windows toostart med hjälp av Azure-tjänster"
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
ms.openlocfilehash: a8cd4e38fde6e4b17a768a7caecd280cd91a70f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-azure-cli-10"></a><span data-ttu-id="f66b3-103">Installera hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f66b3-103">Install hello Azure CLI 1.0</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f66b3-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f66b3-104">PowerShell</span></span>](/powershell/azure/overview)
> * [<span data-ttu-id="f66b3-105">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f66b3-105">Azure CLI 1.0</span></span>](cli-install-nodejs.md)
> * [<span data-ttu-id="f66b3-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f66b3-106">Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

> [!IMPORTANT]
> <span data-ttu-id="f66b3-107">Det här avsnittet beskrivs hur tooinstall hello Azure CLI version 1.0, som bygger på nodeJs och stöder alla klassisk distribution API-anrop samt ett stort antal aktiviteter för Resource Manager distribution.</span><span class="sxs-lookup"><span data-stu-id="f66b3-107">This topic describes how tooinstall hello Azure CLI 1.0, which is built on nodeJs and supports all classic deployment API calls as well as a large number of Resource Manager deployment activities.</span></span> <span data-ttu-id="f66b3-108">Du bör använda hello [Azure CLI 2.0](/cli/azure/overview) för nya eller framtida CLI-distribution och hantering.</span><span class="sxs-lookup"><span data-stu-id="f66b3-108">You should use hello [Azure CLI 2.0](/cli/azure/overview) for new or forward-looking CLI deployments and management.</span></span>

<span data-ttu-id="f66b3-109">Snabbt installera hello Azure-kommandoradsgränssnittet (Azure CLI 1.0) toouse öppen källkod shell-baserade kommandon för att skapa och hantera resurser i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f66b3-109">Quickly install hello Azure Command-Line Interface (Azure CLI 1.0) toouse a set of open-source shell-based commands for creating and managing resources in Microsoft Azure.</span></span> <span data-ttu-id="f66b3-110">Har du flera alternativ tooinstall verktygen plattformsoberoende på datorn:</span><span class="sxs-lookup"><span data-stu-id="f66b3-110">You have several options tooinstall these cross-platform tools on your computer:</span></span>

* <span data-ttu-id="f66b3-111">**npm paketet** – kör npm (hello package manager för JavaScript) tooinstall hello senaste Azure CLI 1.0-paket på din distribution av Linux eller OS.</span><span class="sxs-lookup"><span data-stu-id="f66b3-111">**npm package** - Run npm (hello package manager for JavaScript) tooinstall hello latest Azure CLI 1.0 package on your Linux distribution or OS.</span></span> <span data-ttu-id="f66b3-112">Kräver node.js och npm på datorn.</span><span class="sxs-lookup"><span data-stu-id="f66b3-112">Requires node.js and npm on your computer.</span></span>
* <span data-ttu-id="f66b3-113">**Installer** -hämta ett installationsprogram för enkel installation på Mac- eller Windows.</span><span class="sxs-lookup"><span data-stu-id="f66b3-113">**Installer** - Download an installer for easy installation on Mac or Windows.</span></span>
* <span data-ttu-id="f66b3-114">**Dockerbehållare** – börja använda hello senaste CLI i en klar och kör dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="f66b3-114">**Docker container** - Start using hello latest CLI in a ready-to-run Docker container.</span></span> <span data-ttu-id="f66b3-115">Kräver Docker värden på datorn.</span><span class="sxs-lookup"><span data-stu-id="f66b3-115">Requires Docker host on your computer.</span></span>

<span data-ttu-id="f66b3-116">Fler alternativ och bakgrunden finns hello projektet databasen på [GitHub](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="f66b3-116">For more options and background, see hello project repository on [GitHub](https://github.com/azure/azure-xplat-cli).</span></span>

<span data-ttu-id="f66b3-117">När du har installerat hello Azure CLI 1.0 [ansluta till din Azure-prenumeration](xplat-cli-connect.md) och kör hello **azure** kommandon från din kommandoradsgränssnittet (Bash, Terminal, Kommandotolken och så vidare) toowork med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="f66b3-117">Once hello Azure CLI 1.0 is installed, [connect it with your Azure subscription](xplat-cli-connect.md) and run hello **azure** commands from your command-line interface (Bash, Terminal, Command prompt, and so on) toowork with your Azure resources.</span></span>

## <a name="option-1-install-an-npm-package"></a><span data-ttu-id="f66b3-118">Alternativ 1: Installera npm-paket</span><span class="sxs-lookup"><span data-stu-id="f66b3-118">Option 1: Install an npm package</span></span>
<span data-ttu-id="f66b3-119">tooinstall hello CLI från ett npm-paket, kontrollera att du har hämtat och installerat hello [senaste Node.js och npm](https://nodejs.org/en/download/package-manager/).</span><span class="sxs-lookup"><span data-stu-id="f66b3-119">tooinstall hello CLI from an npm package, make sure you have downloaded and installed hello [latest Node.js and npm](https://nodejs.org/en/download/package-manager/).</span></span> <span data-ttu-id="f66b3-120">Kör sedan **installera npm** tooinstall hello azure cli paketet:</span><span class="sxs-lookup"><span data-stu-id="f66b3-120">Then, run **npm install** tooinstall hello azure-cli package:</span></span>

```bash
npm install -g azure-cli
```

<span data-ttu-id="f66b3-121">På Linux-distributioner måste du kanske toouse **sudo** toosuccessfully kör hello **npm** kommandot på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f66b3-121">On Linux distributions, you might need toouse **sudo** toosuccessfully run hello **npm** command, as follows:</span></span>

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> <span data-ttu-id="f66b3-122">Om du behöver tooinstall eller uppdatera Node.js och npm på Linux-distribution eller OS, rekommenderar vi att du installerar hello senaste Node.js LTS versionen (4.x).</span><span class="sxs-lookup"><span data-stu-id="f66b3-122">If you need tooinstall or update Node.js and npm on your Linux distribution or OS, we recommend that you install hello most recent Node.js LTS version (4.x).</span></span> <span data-ttu-id="f66b3-123">Om du använder en äldre version, kan du hämta installationsfel.</span><span class="sxs-lookup"><span data-stu-id="f66b3-123">If you use an older version, you might get installation errors.</span></span>

<span data-ttu-id="f66b3-124">Om du vill hämta hello senaste Linux [tar-filen] [ linux-installer] hello npm paketet lokalt.</span><span class="sxs-lookup"><span data-stu-id="f66b3-124">If you prefer, download hello latest Linux [tar file][linux-installer] for hello npm package locally.</span></span> <span data-ttu-id="f66b3-125">Installera sedan hello hämtade npm-paketet enligt följande (på Linux-distributioner måste du kanske toouse **sudo**):</span><span class="sxs-lookup"><span data-stu-id="f66b3-125">Then, install hello downloaded npm package as follows (on Linux distributions you might need toouse **sudo**):</span></span>

```bash
npm install -g <path toodownloaded tar file>
```

## <a name="option-2-use-an-installer"></a><span data-ttu-id="f66b3-126">Alternativ 2: Använda ett installationsprogram</span><span class="sxs-lookup"><span data-stu-id="f66b3-126">Option 2: Use an installer</span></span>
<span data-ttu-id="f66b3-127">Om du använder en dator med Mac- eller Windows kan hello följande CLI installationsprogram hämtas:</span><span class="sxs-lookup"><span data-stu-id="f66b3-127">If you use a Mac or Windows computer, hello following CLI installers are available for download:</span></span>

* <span data-ttu-id="f66b3-128">[Mac OS X-installationsprogrammet][mac-installer]</span><span class="sxs-lookup"><span data-stu-id="f66b3-128">[Mac OS X installer][mac-installer]</span></span>
* <span data-ttu-id="f66b3-129">[Windows MSI][windows-installer]</span><span class="sxs-lookup"><span data-stu-id="f66b3-129">[Windows MSI][windows-installer]</span></span>

> [!TIP]
> <span data-ttu-id="f66b3-130">I Windows, kan du också hämta hello [installationsprogram för webbplattform](https://go.microsoft.com/?linkid=9828653) tooinstall hello CLI.</span><span class="sxs-lookup"><span data-stu-id="f66b3-130">On Windows, you can also download hello [Web Platform Installer](https://go.microsoft.com/?linkid=9828653) tooinstall hello CLI.</span></span> <span data-ttu-id="f66b3-131">Installationsprogrammet kan du hello alternativet tooinstall ytterligare Azure SDK och kommandoradsverktygen när du har installerat hello CLI.</span><span class="sxs-lookup"><span data-stu-id="f66b3-131">This installer gives you hello option tooinstall additional Azure SDK and command-line tools after installing hello CLI.</span></span>

## <a name="option-3-use-a-docker-container"></a><span data-ttu-id="f66b3-132">Alternativ 3: Använda en dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="f66b3-132">Option 3: Use a Docker container</span></span>
<span data-ttu-id="f66b3-133">Om du har konfigurerat datorn som en [Docker](https://docs.docker.com/engine/understanding-docker/) värden som du kan köra hello senaste Azure CLI 1.0 i en dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="f66b3-133">If you have set up your computer as a [Docker](https://docs.docker.com/engine/understanding-docker/) host, you can run hello latest Azure CLI 1.0 in a Docker container.</span></span> <span data-ttu-id="f66b3-134">Kör hello följande kommando (på Linux-distributioner måste du kanske toouse **sudo**):</span><span class="sxs-lookup"><span data-stu-id="f66b3-134">Run hello following command (on Linux distributions you might need toouse **sudo**):</span></span>

```bash
docker run -it microsoft/azure-cli
```

## <a name="run-azure-cli-10-commands"></a><span data-ttu-id="f66b3-135">Kör kommandon för Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f66b3-135">Run Azure CLI 1.0 commands</span></span>
<span data-ttu-id="f66b3-136">När du har installerat hello Azure CLI 1.0 kör hello **azure** från kommandoraden användargränssnittet (Bash, Terminal, Kommandotolken och så vidare).</span><span class="sxs-lookup"><span data-stu-id="f66b3-136">After hello Azure CLI 1.0 is installed, run hello **azure** command from your command-line user interface (Bash, Terminal, Command prompt, and so on).</span></span> <span data-ttu-id="f66b3-137">Till exempel toorun hello hjälpkommando skriver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="f66b3-137">For example, toorun hello help command, type hello following:</span></span>

```azurecli
azure help
```

> [!NOTE]
> <span data-ttu-id="f66b3-138">På vissa Linux-distributioner kan du få ett felmeddelande liknande för`/usr/bin/env: ‘node’: No such file or directory`.</span><span class="sxs-lookup"><span data-stu-id="f66b3-138">On some Linux distributions, you may receive an error similar too`/usr/bin/env: ‘node’: No such file or directory`.</span></span> <span data-ttu-id="f66b3-139">Det här felet kommer från nya installationer av Node.js som installeras på /usr/bin/nodejs.</span><span class="sxs-lookup"><span data-stu-id="f66b3-139">This error comes from recent installations of Node.js being installed at /usr/bin/nodejs.</span></span> <span data-ttu-id="f66b3-140">toofix, skapa en symbolisk länk för/usr/bin/nod genom att köra det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="f66b3-140">toofix it, create a symbolic link too/usr/bin/node by running this command:</span></span>

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

<span data-ttu-id="f66b3-141">toosee hello version av hello Azure CLI 1.0 du installerat typen hello följande:</span><span class="sxs-lookup"><span data-stu-id="f66b3-141">toosee hello version of hello Azure CLI 1.0 you installed, type hello following:</span></span>

```azurecli
azure --version
```

<span data-ttu-id="f66b3-142">Nu är du klar!</span><span class="sxs-lookup"><span data-stu-id="f66b3-142">Now you are ready!</span></span> <span data-ttu-id="f66b3-143">alla tooaccess hello CLI-kommandon toowork med egna resurser, [ansluta tooyour Azure-prenumeration från hello Azure CLI](xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="f66b3-143">tooaccess all hello CLI commands toowork with your own resources, [connect tooyour Azure subscription from hello Azure CLI](xplat-cli-connect.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f66b3-144">När du börjar använda Azure CLI, visas ett meddelande som frågar om du vill tooallow Microsoft toocollect användningsinformation.</span><span class="sxs-lookup"><span data-stu-id="f66b3-144">When you first use Azure CLI, you see a message asking if you want tooallow Microsoft toocollect usage information.</span></span> <span data-ttu-id="f66b3-145">Det är frivilligt att delta.</span><span class="sxs-lookup"><span data-stu-id="f66b3-145">Participation is voluntary.</span></span> <span data-ttu-id="f66b3-146">Om du väljer tooparticipate måste du stoppa när som helst genom att köra `azure telemetry --disable`.</span><span class="sxs-lookup"><span data-stu-id="f66b3-146">If you choose tooparticipate, you can stop at any time by running `azure telemetry --disable`.</span></span> <span data-ttu-id="f66b3-147">tooenable delta när som helst köra `azure telemetry --enable`.</span><span class="sxs-lookup"><span data-stu-id="f66b3-147">tooenable participation at any time, run `azure telemetry --enable`.</span></span>

## <a name="update-hello-cli"></a><span data-ttu-id="f66b3-148">Uppdatera hello CLI</span><span class="sxs-lookup"><span data-stu-id="f66b3-148">Update hello CLI</span></span>
<span data-ttu-id="f66b3-149">Microsoft släpper ofta uppdaterade versioner av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f66b3-149">Microsoft frequently releases updated versions of hello Azure CLI.</span></span> <span data-ttu-id="f66b3-150">Installera om hello CLI med hello installer för ditt operativsystem eller köra hello senaste dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="f66b3-150">Reinstall hello CLI using hello installer for your operating system, or run hello latest Docker container.</span></span> <span data-ttu-id="f66b3-151">Om du har hello senaste Node.js och npm installerad, uppdatera genom att skriva följande hello (på Linux-distributioner måste du kanske toouse **sudo**).</span><span class="sxs-lookup"><span data-stu-id="f66b3-151">Or, if you have hello latest Node.js and npm installed, update by typing hello following (on Linux distributions you might need toouse **sudo**).</span></span>

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a><span data-ttu-id="f66b3-152">Aktivera flikavslutande</span><span class="sxs-lookup"><span data-stu-id="f66b3-152">Enable tab completion</span></span>
<span data-ttu-id="f66b3-153">Flikslutförande av CLI-kommandon stöds för Mac- och Linux.</span><span class="sxs-lookup"><span data-stu-id="f66b3-153">Tab completion of CLI commands is supported for Mac and Linux.</span></span>

<span data-ttu-id="f66b3-154">tooenable i zsh, kör:</span><span class="sxs-lookup"><span data-stu-id="f66b3-154">tooenable it in zsh, run:</span></span>

```bash
echo '. <(azure --completion)' >> .zshrc
```

<span data-ttu-id="f66b3-155">tooenable i bash, kör:</span><span class="sxs-lookup"><span data-stu-id="f66b3-155">tooenable it in bash, run:</span></span>

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a><span data-ttu-id="f66b3-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f66b3-156">Next steps</span></span>
* <span data-ttu-id="f66b3-157">[Ansluta från hello CLI tooyour Azure-prenumeration](xplat-cli-connect.md) toocreate och hantera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="f66b3-157">[Connect from hello CLI tooyour Azure subscription](xplat-cli-connect.md) toocreate and manage Azure resources.</span></span>
* <span data-ttu-id="f66b3-158">toolearn mer om hello Azure CLI, ladda ned källkoden, rapportera problem, eller bidra toohello projekt, besök hello [GitHub-lagringsplatsen för hello Azure CLI](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="f66b3-158">toolearn more about hello Azure CLI, download source code, report problems, or contribute toohello project, visit hello [GitHub repository for hello Azure CLI](https://github.com/azure/azure-xplat-cli).</span></span>
* <span data-ttu-id="f66b3-159">Om du har frågor om hur du använder hello Azure CLI eller Azure besöka hello [Azure forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span><span class="sxs-lookup"><span data-stu-id="f66b3-159">If you have questions about using hello Azure CLI, or Azure, visit hello [Azure Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span></span>


[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: /cli/azure/get-started-with-az-cli2
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
