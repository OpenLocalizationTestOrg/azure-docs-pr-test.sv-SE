---
title: "aaaMonitoring en Linux VM med en VM-tillägg | Microsoft Docs"
description: "Lär dig hur toouse hello Linux diagnostiska tillägget toomonitor hello prestanda och diagnostikdata av Linux VM i Azure."
services: virtual-machines-linux
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f54a11c5-5a0e-40ff-af6c-e60bd464058b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: Ning
ms.openlocfilehash: cf7bfebca8c0367941f7a975417f60fe2e2dab25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-linux-diagnostic-extension-toomonitor-hello-performance-and-diagnostic-data-of-a-linux-vm"></a><span data-ttu-id="be5ae-103">Använd hello Linux diagnostiska tillägget toomonitor hello prestanda och diagnostikdata för en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="be5ae-103">Use hello Linux Diagnostic Extension toomonitor hello performance and diagnostic data of a Linux VM</span></span>

<span data-ttu-id="be5ae-104">Det här dokumentet beskriver version 2.3 av hello Linux diagnostiska tillägg.</span><span class="sxs-lookup"><span data-stu-id="be5ae-104">This document describes version 2.3 of hello Linux Diagnostic Extension.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="be5ae-105">Den här versionen är föråldrad och det kan vara opublicerad helst efter 30 juni 2018.</span><span class="sxs-lookup"><span data-stu-id="be5ae-105">This version is deprecated, and it may be unpublished any time after June 30, 2018.</span></span> <span data-ttu-id="be5ae-106">Den har ersatts av version 3.0.</span><span class="sxs-lookup"><span data-stu-id="be5ae-106">It has been replaced by version 3.0.</span></span> <span data-ttu-id="be5ae-107">Mer information finns i hello [dokumentationen för version 3.0 för hello Linux diagnostiska tillägget](../diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="be5ae-107">For more information, see hello [documentation for version 3.0 of hello Linux Diagnostic Extension](../diagnostic-extension.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="be5ae-108">Introduktion</span><span class="sxs-lookup"><span data-stu-id="be5ae-108">Introduction</span></span>

<span data-ttu-id="be5ae-109">(**Observera**: hello Linux diagnostiska tillägget är öppen källkod på [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) där hello aktuell information om hello tillägget publiceras först.</span><span class="sxs-lookup"><span data-stu-id="be5ae-109">(**Note**: hello Linux Diagnostic Extension is open-sourced on [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) where hello most current information on hello extension is first published.</span></span> <span data-ttu-id="be5ae-110">Du kanske vill toocheck hello [GitHub-sidan](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) första.)</span><span class="sxs-lookup"><span data-stu-id="be5ae-110">You might want toocheck hello [GitHub page](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) first.)</span></span>

<span data-ttu-id="be5ae-111">hello Linux diagnostiska tillägget kan en användare övervakaren hello virtuella Linux-datorer som körs på Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="be5ae-111">hello Linux Diagnostic Extension helps a user monitor hello Linux VMs that are running on Microsoft Azure.</span></span> <span data-ttu-id="be5ae-112">Det har hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="be5ae-112">It has hello following capabilities:</span></span>

* <span data-ttu-id="be5ae-113">Samlar in och överför hello prestandainformation från hello Linux VM toohello användarens lagring tabell, inklusive diagnostik- och syslog.</span><span class="sxs-lookup"><span data-stu-id="be5ae-113">Collects and uploads hello system performance information from hello Linux VM toohello user's storage table, including diagnostic and syslog information.</span></span>
* <span data-ttu-id="be5ae-114">Låter användare toocustomize hello data mått som ska samlas in och överföra.</span><span class="sxs-lookup"><span data-stu-id="be5ae-114">Enables users toocustomize hello data metrics that will be collected and uploaded.</span></span>
* <span data-ttu-id="be5ae-115">Låter användare tooupload angivna filer tooa avsedda lagringstabellen.</span><span class="sxs-lookup"><span data-stu-id="be5ae-115">Enables users tooupload specified log files tooa designated storage table.</span></span>

<span data-ttu-id="be5ae-116">I hello aktuell version 2.3 omfattar hello data:</span><span class="sxs-lookup"><span data-stu-id="be5ae-116">In hello current version 2.3, hello data includes:</span></span>

* <span data-ttu-id="be5ae-117">Alla Linux Rsyslog loggar, inklusive system-, säkerhets- och programloggarna.</span><span class="sxs-lookup"><span data-stu-id="be5ae-117">All Linux Rsyslog logs, including system, security, and application logs.</span></span>
* <span data-ttu-id="be5ae-118">Alla systemdata som har angetts på [hello System Center Cross Platform lösningar plats](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="be5ae-118">All system data that's specified on [hello System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span>
* <span data-ttu-id="be5ae-119">Loggfiler som angetts av användaren.</span><span class="sxs-lookup"><span data-stu-id="be5ae-119">User-specified log files.</span></span>

<span data-ttu-id="be5ae-120">Det här tillägget fungerar med både hello klassiska och Resource Manager distributionsmodellerna.</span><span class="sxs-lookup"><span data-stu-id="be5ae-120">This extension works with both hello classic and Resource Manager deployment models.</span></span>

### <a name="current-version-of-hello-extension-and-deprecation-of-old-versions"></a><span data-ttu-id="be5ae-121">Aktuell version av tillägget hello och utfasningen av äldre versioner</span><span class="sxs-lookup"><span data-stu-id="be5ae-121">Current version of hello extension and deprecation of old versions</span></span>

<span data-ttu-id="be5ae-122">hello senaste versionen av tillägget hello **2.3**, och **eventuella gamla versioner (2.0, 2.1 och 2.2) kommer föråldrad och Opublicerat slutet av det här året (2017)**.</span><span class="sxs-lookup"><span data-stu-id="be5ae-122">hello latest version of hello extension is **2.3**, and **any old versions (2.0, 2.1, and 2.2) will be deprecated and unpublished by end of this year (2017)**.</span></span> <span data-ttu-id="be5ae-123">Om du har installerat hello Linux diagnostik tillägget med automatisk mindre versionsuppgradering inaktiverad rekommenderas starkt att du avinstallerar hello-tillägget och installera om den med automatiska mindre versionsuppgradering aktiverad.</span><span class="sxs-lookup"><span data-stu-id="be5ae-123">If you installed hello Linux Diagnostic extension with automatic minor version upgrade disabled, it's strongly recommended that you uninstall hello extension and reinstall it with automatic minor version upgrade enabled.</span></span> <span data-ttu-id="be5ae-124">Du kan åstadkomma detta genom att ange '2.*' hello versionen om du installerar tillägget hello via Azure XPLAT CLI eller Powershell på klassiska (ASM) virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="be5ae-124">On classic (ASM) VMs, you can achieve this by specifying '2.*' as hello version if you are installing hello extension through Azure XPLAT CLI or Powershell.</span></span> <span data-ttu-id="be5ae-125">På ARM virtuella datorer du kan åstadkomma detta genom att inkludera ”” autoUpgradeMinorVersion ”: true' i hello mall för distribution.</span><span class="sxs-lookup"><span data-stu-id="be5ae-125">On ARM VMs, you can achieve this by including '"autoUpgradeMinorVersion": true' in hello VM deployment template.</span></span> <span data-ttu-id="be5ae-126">En ny installation av hello-tillägget bör också ha hello automatiskt delversion uppgradera alternativet är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="be5ae-126">Also, any new installation of hello extension should have hello auto minor version upgrade option turned on.</span></span>

## <a name="enable-hello-extension"></a><span data-ttu-id="be5ae-127">Aktivera hello-tillägg</span><span class="sxs-lookup"><span data-stu-id="be5ae-127">Enable hello extension</span></span>

<span data-ttu-id="be5ae-128">Du kan aktivera det här tillägget med hjälp av hello [Azure-portalen](https://portal.azure.com/#), Azure PowerShell eller Azure CLI-skript.</span><span class="sxs-lookup"><span data-stu-id="be5ae-128">You can enable this extension by using hello [Azure portal](https://portal.azure.com/#), Azure PowerShell, or Azure CLI scripts.</span></span>

<span data-ttu-id="be5ae-129">tooview hello system och konfigurera prestandadata direkt från hello Azure-portalen, Följ [de här stegen på hello Azure blogg](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).</span><span class="sxs-lookup"><span data-stu-id="be5ae-129">tooview and configure hello system and performance data directly from hello Azure portal, follow [these steps on hello Azure blog](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).</span></span>

<span data-ttu-id="be5ae-130">Den här artikeln fokuserar på hur tooenable och konfigurera hello-tillägget med hjälp av Azure CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="be5ae-130">This article focuses on how tooenable and configure hello extension by using Azure CLI commands.</span></span> <span data-ttu-id="be5ae-131">Detta ger dig tooread och visa hello data direkt från hello lagringstabellen.</span><span class="sxs-lookup"><span data-stu-id="be5ae-131">This allows you tooread and view hello data directly from hello storage table.</span></span>

<span data-ttu-id="be5ae-132">Observera att hello configuration metoder som beskrivs här inte fungerar för hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="be5ae-132">Note that hello configuration methods that are described here won't work for hello Azure portal.</span></span> <span data-ttu-id="be5ae-133">tooview och konfigurerar hello system och prestanda data direkt från hello Azure-portalen, hello tillägget måste vara aktiverat hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="be5ae-133">tooview and configure hello system and performance data directly from hello Azure portal, hello extension must be enabled through hello portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be5ae-134">Krav</span><span class="sxs-lookup"><span data-stu-id="be5ae-134">Prerequisites</span></span>

* <span data-ttu-id="be5ae-135">**Azure Linux-agentens version 2.0.6 eller senare**.</span><span class="sxs-lookup"><span data-stu-id="be5ae-135">**Azure Linux Agent version 2.0.6 or later**.</span></span>

  <span data-ttu-id="be5ae-136">Observera att de flesta Azure VM Linux galleriavbildningar inkluderar version 2.0.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="be5ae-136">Note that most Azure VM Linux gallery images include version 2.0.6 or later.</span></span> <span data-ttu-id="be5ae-137">Du kan köra **WAAgent-version** tooconfirm vilken version som är installerad på hello VM.</span><span class="sxs-lookup"><span data-stu-id="be5ae-137">You can run **WAAgent -version** tooconfirm which version is installed on hello VM.</span></span> <span data-ttu-id="be5ae-138">Om hello VM kör en version som är tidigare än 2.0.6, du kan följa [instruktionerna på GitHub](https://github.com/Azure/WALinuxAgent "instruktioner") tooupdate den.</span><span class="sxs-lookup"><span data-stu-id="be5ae-138">If hello VM is running a version that's earlier than 2.0.6, you can follow [these instructions on GitHub](https://github.com/Azure/WALinuxAgent "instructions") tooupdate it.</span></span>
* <span data-ttu-id="be5ae-139">**Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="be5ae-139">**Azure CLI**.</span></span> <span data-ttu-id="be5ae-140">Följ [vägledningen för att installera CLI](../../../cli-install-nodejs.md) tooset in hello Azure CLI-miljön på datorn.</span><span class="sxs-lookup"><span data-stu-id="be5ae-140">Follow [this guidance for installing CLI](../../../cli-install-nodejs.md) tooset up hello Azure CLI environment on your machine.</span></span> <span data-ttu-id="be5ae-141">När Azure CLI är installerad kan du använda hello **azure** från din kommandoradsgränssnittet (Bash, Terminal eller Kommandotolken) tooaccess hello Azure CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="be5ae-141">After Azure CLI is installed, you can use hello **azure** command from your command-line interface (Bash, Terminal, or command prompt) tooaccess hello Azure CLI commands.</span></span> <span data-ttu-id="be5ae-142">Exempel:</span><span class="sxs-lookup"><span data-stu-id="be5ae-142">For example:</span></span>

  * <span data-ttu-id="be5ae-143">Kör **azure vm-tillägget set--hjälp** för närmare hjälp.</span><span class="sxs-lookup"><span data-stu-id="be5ae-143">Run **azure vm extension set --help** for detailed help information.</span></span>
  * <span data-ttu-id="be5ae-144">Kör **azure-inloggning** toosign i tooAzure.</span><span class="sxs-lookup"><span data-stu-id="be5ae-144">Run **azure login** toosign in tooAzure.</span></span>
  * <span data-ttu-id="be5ae-145">Kör **azure vm listan** toolist alla hello virtuella datorer som du har i Azure.</span><span class="sxs-lookup"><span data-stu-id="be5ae-145">Run **azure vm list** toolist all hello virtual machines that you have on Azure.</span></span>
* <span data-ttu-id="be5ae-146">En storage-konto toostore hello-data.</span><span class="sxs-lookup"><span data-stu-id="be5ae-146">A storage account toostore hello data.</span></span> <span data-ttu-id="be5ae-147">Du måste namnet på ett lagringskonto som skapades tidigare och en access key tooupload hello tooyour datalagring.</span><span class="sxs-lookup"><span data-stu-id="be5ae-147">You will need a storage account name that was created previously and an access key tooupload hello data tooyour storage.</span></span>

## <a name="use-hello-azure-cli-command-tooenable-hello-linux-diagnostic-extension"></a><span data-ttu-id="be5ae-148">Använd hello Azure CLI kommandot tooenable hello Linux diagnostiska tillägg</span><span class="sxs-lookup"><span data-stu-id="be5ae-148">Use hello Azure CLI command tooenable hello Linux Diagnostic Extension</span></span>

### <a name="scenario-1-enable-hello-extension-with-hello-default-data-set"></a><span data-ttu-id="be5ae-149">Scenario 1.</span><span class="sxs-lookup"><span data-stu-id="be5ae-149">Scenario 1.</span></span> <span data-ttu-id="be5ae-150">Aktivera hello-tillägget med hello standarddatauppsättningen</span><span class="sxs-lookup"><span data-stu-id="be5ae-150">Enable hello extension with hello default data set</span></span>

<span data-ttu-id="be5ae-151">I version 2.3 eller senare inkluderar hello standarddata som samlas in</span><span class="sxs-lookup"><span data-stu-id="be5ae-151">In version 2.3 or later, hello default data that will be collected includes:</span></span>

* <span data-ttu-id="be5ae-152">Alla Rsyslog information (inklusive system-, säkerhets- och programloggarna).</span><span class="sxs-lookup"><span data-stu-id="be5ae-152">All Rsyslog information (including system, security, and application logs).</span></span>  
* <span data-ttu-id="be5ae-153">En grundläggande uppsättning bas systemdata.</span><span class="sxs-lookup"><span data-stu-id="be5ae-153">A core set of basis system data.</span></span> <span data-ttu-id="be5ae-154">Observera att hello fullständig datauppsättning beskrivs på hello [System Center Cross Platform lösningar plats](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="be5ae-154">Note that hello full data set is described on hello [System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span>
  <span data-ttu-id="be5ae-155">Om du vill tooenable extra data fortsätta med hello steg i Scenario 2 och 3.</span><span class="sxs-lookup"><span data-stu-id="be5ae-155">If you want tooenable extra data, continue with hello steps in Scenarios 2 and 3.</span></span>

<span data-ttu-id="be5ae-156">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="be5ae-156">Step 1.</span></span> <span data-ttu-id="be5ae-157">Skapa en fil med namnet PrivateConfig.json med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="be5ae-157">Create a file named PrivateConfig.json with hello following content:</span></span>

    {
        "storageAccountName" : "hello storage account tooreceive data",
        "storageAccountKey" : "hello key of hello account"
    }

<span data-ttu-id="be5ae-158">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="be5ae-158">Step 2.</span></span> <span data-ttu-id="be5ae-159">Kör  **azure vm-tillägget anger vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --privat-config-sökvägen PrivateConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="be5ae-159">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --private-config-path PrivateConfig.json**.</span></span>

### <a name="scenario-2-customize-hello-performance-monitor-metrics"></a><span data-ttu-id="be5ae-160">Scenario 2.</span><span class="sxs-lookup"><span data-stu-id="be5ae-160">Scenario 2.</span></span> <span data-ttu-id="be5ae-161">Anpassa hello prestandamått för Övervakare</span><span class="sxs-lookup"><span data-stu-id="be5ae-161">Customize hello performance monitor metrics</span></span>

<span data-ttu-id="be5ae-162">Det här avsnittet beskrivs hur toocustomize hello prestanda och diagnostikdata tabell.</span><span class="sxs-lookup"><span data-stu-id="be5ae-162">This section describes how toocustomize hello performance and diagnostic data table.</span></span>

<span data-ttu-id="be5ae-163">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="be5ae-163">Step 1.</span></span> <span data-ttu-id="be5ae-164">Skapa en fil med namnet PrivateConfig.json med hello innehåll som beskrevs i Scenario 1.</span><span class="sxs-lookup"><span data-stu-id="be5ae-164">Create a file named PrivateConfig.json with hello content that was described in Scenario 1.</span></span> <span data-ttu-id="be5ae-165">Även skapa en fil med namnet PublicConfig.json.</span><span class="sxs-lookup"><span data-stu-id="be5ae-165">Also create a file named PublicConfig.json.</span></span> <span data-ttu-id="be5ae-166">Ange hello vilka data som du vill toocollect.</span><span class="sxs-lookup"><span data-stu-id="be5ae-166">Specify hello particular data you want toocollect.</span></span>

<span data-ttu-id="be5ae-167">För alla stöds providers och variabler, referera till hello [System Center Cross Platform lösningar plats](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="be5ae-167">For all supported providers and variables, reference hello [System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span> <span data-ttu-id="be5ae-168">Du kan ha flera frågor och lagra dem på flera tabeller genom att lägga till fler frågor toohello skript.</span><span class="sxs-lookup"><span data-stu-id="be5ae-168">You can have multiple queries and store them in multiple tables by appending more queries toohello script.</span></span>

<span data-ttu-id="be5ae-169">Hej Rsyslog data samlas alltid som standard.</span><span class="sxs-lookup"><span data-stu-id="be5ae-169">By default, hello Rsyslog data is always collected.</span></span>

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


<span data-ttu-id="be5ae-170">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="be5ae-170">Step 2.</span></span> <span data-ttu-id="be5ae-171">Kör  **azure vm-tillägget anger vm_name LinuxDiagnostic Microsoft.OSTCExtensions ' 2.*'--privat-config-sökvägen PrivateConfig.json--offentliga-config-sökvägen PublicConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="be5ae-171">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.</span></span>

### <a name="scenario-3-upload-your-own-log-files"></a><span data-ttu-id="be5ae-172">Scenario 3.</span><span class="sxs-lookup"><span data-stu-id="be5ae-172">Scenario 3.</span></span> <span data-ttu-id="be5ae-173">Ladda upp en egen loggfiler</span><span class="sxs-lookup"><span data-stu-id="be5ae-173">Upload your own log files</span></span>

<span data-ttu-id="be5ae-174">Det här avsnittet beskrivs hur toocollect och ladda upp specifika loggfiler tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="be5ae-174">This section describes how toocollect and upload specific log files tooyour storage account.</span></span> <span data-ttu-id="be5ae-175">Du måste toospecify båda hello tooyour loggen fil- och hello sökvägsnamn hello tabellen där toostore loggen.</span><span class="sxs-lookup"><span data-stu-id="be5ae-175">You need toospecify both hello path tooyour log file and hello name of hello table where you want toostore your log.</span></span> <span data-ttu-id="be5ae-176">Du kan skapa flera loggfiler genom att lägga till flera tabellen och file poster toohello skript.</span><span class="sxs-lookup"><span data-stu-id="be5ae-176">You can create multiple log files by adding multiple file/table entries toohello script.</span></span>

<span data-ttu-id="be5ae-177">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="be5ae-177">Step 1.</span></span> <span data-ttu-id="be5ae-178">Skapa en fil med namnet PrivateConfig.json med hello innehåll som beskrevs i Scenario 1.</span><span class="sxs-lookup"><span data-stu-id="be5ae-178">Create a file named PrivateConfig.json with hello content that was described in Scenario 1.</span></span> <span data-ttu-id="be5ae-179">Sedan skapa en annan fil som heter PublicConfig.json med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="be5ae-179">Then create another file named PublicConfig.json with hello following content:</span></span>

```json
{
    "fileCfg" :
    [
        {
            "file" : "/var/log/mysql.err",
            "table" : "mysqlerr"
            }
    ]
}
```

<span data-ttu-id="be5ae-180">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="be5ae-180">Step 2.</span></span> <span data-ttu-id="be5ae-181">Kör `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="be5ae-181">Run `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json`.</span></span>

<span data-ttu-id="be5ae-182">Observera att med den här inställningen på hello tillägget versioner tidigare too2.3, alla loggar som skrivs för`/var/log/mysql.err` kan dupliceras för`/var/log/syslog` (eller `/var/log/messages` beroende på hello Linux distro) samt.</span><span class="sxs-lookup"><span data-stu-id="be5ae-182">Note that with this setting on hello extension versions prior too2.3, all logs written too`/var/log/mysql.err` might be duplicated too`/var/log/syslog` (or `/var/log/messages` depending on hello Linux distro) as well.</span></span> <span data-ttu-id="be5ae-183">Om du vill att tooavoid Dubbletten loggning, kan du undanta loggning av `local6` och loggar in rsyslog konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="be5ae-183">If you'd like tooavoid this duplicate logging, you can exclude logging of `local6` facility logs in your rsyslog configuration.</span></span> <span data-ttu-id="be5ae-184">Det beror på hello Linux distro, men på ett Ubuntu 14.04 system hello filen toomodify är `/etc/rsyslog.d/50-default.conf` och du kan ersätta hello rad `*.*;auth,authpriv.none -/var/log/syslog` för`*.*;auth,authpriv,local6.none -/var/log/syslog`.</span><span class="sxs-lookup"><span data-stu-id="be5ae-184">It depends on hello Linux distro, but on an Ubuntu 14.04 system, hello file toomodify is `/etc/rsyslog.d/50-default.conf` and you can replace hello line `*.*;auth,authpriv.none -/var/log/syslog` too`*.*;auth,authpriv,local6.none -/var/log/syslog`.</span></span> <span data-ttu-id="be5ae-185">Det här problemet löses i hello senaste snabbkorrigeringen versionen av 2.3 (2.3.9007), så om du har hello tillägg version 2.3 problemet inte bör sker.</span><span class="sxs-lookup"><span data-stu-id="be5ae-185">This issue is fixed in hello latest hotfix release of 2.3 (2.3.9007), so if you have hello extension version 2.3, this issue should not happen.</span></span> <span data-ttu-id="be5ae-186">Om den fortfarande finns även efter att starta om den virtuella datorn, kontakta oss och hjälper oss att felsöka anledningen hello senaste snabbkorrigeringsversionen inte installeras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="be5ae-186">If it still does even after restarting your VM, please contact us and help us troubleshoot why hello latest hotfix version is not installed automatically.</span></span>

### <a name="scenario-4-stop-hello-extension-from-collecting-any-logs"></a><span data-ttu-id="be5ae-187">Scenario 4.</span><span class="sxs-lookup"><span data-stu-id="be5ae-187">Scenario 4.</span></span> <span data-ttu-id="be5ae-188">Stoppa hello tillägget samlar in loggar</span><span class="sxs-lookup"><span data-stu-id="be5ae-188">Stop hello extension from collecting any logs</span></span>

<span data-ttu-id="be5ae-189">Det här avsnittet beskrivs hur toostop hello tillägget samlar in loggar.</span><span class="sxs-lookup"><span data-stu-id="be5ae-189">This section describes how toostop hello extension from collecting logs.</span></span> <span data-ttu-id="be5ae-190">Observera att hello övervakningsprocessen agent kommer att fortfarande aktiv och körs med den här omkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="be5ae-190">Note that hello monitoring agent process will be still up and running even with this reconfiguration.</span></span> <span data-ttu-id="be5ae-191">Om du vill toostop hello agent processen för övervakning helt göra du det genom att inaktivera hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="be5ae-191">If you'd like toostop hello monitoring agent process completely, you can do so by disabling hello extension.</span></span> <span data-ttu-id="be5ae-192">hello kommandot toodisable hello tillägget är `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.</span><span class="sxs-lookup"><span data-stu-id="be5ae-192">hello command toodisable hello extension is `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.</span></span>

<span data-ttu-id="be5ae-193">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="be5ae-193">Step 1.</span></span> <span data-ttu-id="be5ae-194">Skapa en fil med namnet PrivateConfig.json med hello innehåll som beskrevs i Scenario 1.</span><span class="sxs-lookup"><span data-stu-id="be5ae-194">Create a file named PrivateConfig.json with hello content that was described in Scenario 1.</span></span> <span data-ttu-id="be5ae-195">Skapa en annan fil som heter PublicConfig.json med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="be5ae-195">Create another file named PublicConfig.json with hello following content:</span></span>

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


<span data-ttu-id="be5ae-196">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="be5ae-196">Step 2.</span></span> <span data-ttu-id="be5ae-197">Kör  **azure vm-tillägget anger vm_name LinuxDiagnostic Microsoft.OSTCExtensions ' 2.*'--privat-config-sökvägen PrivateConfig.json--offentliga-config-sökvägen PublicConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="be5ae-197">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.</span></span>

## <a name="review-your-data"></a><span data-ttu-id="be5ae-198">Granska dina data</span><span class="sxs-lookup"><span data-stu-id="be5ae-198">Review your data</span></span>

<span data-ttu-id="be5ae-199">hello prestanda och diagnostiska data lagras i en tabell i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="be5ae-199">hello performance and diagnostic data are stored in an Azure Storage table.</span></span> <span data-ttu-id="be5ae-200">Granska [hur toouse Azure Table Storage från Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) toolearn tooaccess hello data i hello lagring tabell med hjälp av Azure CLI-skript.</span><span class="sxs-lookup"><span data-stu-id="be5ae-200">Review [How toouse Azure Table Storage from Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) toolearn how tooaccess hello data in hello storage table by using Azure CLI scripts.</span></span>

<span data-ttu-id="be5ae-201">Dessutom kan du använda följande UI verktyg tooaccess hello data:</span><span class="sxs-lookup"><span data-stu-id="be5ae-201">In addition, you can use following UI tools tooaccess hello data:</span></span>

1. <span data-ttu-id="be5ae-202">Visual Studio Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="be5ae-202">Visual Studio Server Explorer.</span></span> <span data-ttu-id="be5ae-203">Gå tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="be5ae-203">Go tooyour storage account.</span></span> <span data-ttu-id="be5ae-204">När hello VM har körts under fem minuter ser hello fyra standardtabeller: ”LinuxCpu”, ”LinuxDisk”, ”LinuxMemory” och ”Linuxsyslog”.</span><span class="sxs-lookup"><span data-stu-id="be5ae-204">After hello VM runs for about five minutes, you'll see hello four default tables: “LinuxCpu”, ”LinuxDisk”, ”LinuxMemory”, and ”Linuxsyslog”.</span></span> <span data-ttu-id="be5ae-205">Dubbelklicka på hello namn tooview hello tabelldata.</span><span class="sxs-lookup"><span data-stu-id="be5ae-205">Double-click hello table names tooview hello data.</span></span>
1. <span data-ttu-id="be5ae-206">[Azure Lagringsutforskaren](https://azurestorageexplorer.codeplex.com/ "Azure Lagringsutforskaren").</span><span class="sxs-lookup"><span data-stu-id="be5ae-206">[Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span></span>

![Bild](./media/diagnostic-extension/no1.png)

<span data-ttu-id="be5ae-208">Om du har aktiverat fileCfg eller perfCfg (enligt beskrivningen i Scenario 2 och 3) kan använda du Visual Studio Server Explorer och Azure Lagringsutforskaren tooview icke-förvalt data.</span><span class="sxs-lookup"><span data-stu-id="be5ae-208">If you've enabled fileCfg or perfCfg (as described in Scenarios 2 and 3), you can use Visual Studio Server Explorer and Azure Storage Explorer tooview non-default data.</span></span>

## <a name="known-issues"></a><span data-ttu-id="be5ae-209">Kända problem</span><span class="sxs-lookup"><span data-stu-id="be5ae-209">Known issues</span></span>

* <span data-ttu-id="be5ae-210">Hej Rsyslog information och anges av kunden loggfilen kan endast nås via skript.</span><span class="sxs-lookup"><span data-stu-id="be5ae-210">hello Rsyslog information and customer-specified log file can only be accessed via scripting.</span></span>
