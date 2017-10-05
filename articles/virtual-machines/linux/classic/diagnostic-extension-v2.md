---
title: "Övervaka en Linux VM med en VM-tillägg | Microsoft Docs"
description: "Lär dig hur du använder tillägget Linux diagnostiska för att övervaka prestanda och diagnostikdata av Linux VM i Azure."
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
ms.openlocfilehash: b8c6e2e22d8478b6e92e7b7942f15d37a840fed3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-linux-diagnostic-extension-to-monitor-the-performance-and-diagnostic-data-of-a-linux-vm"></a><span data-ttu-id="a1469-103">Använd diagnostiktillägget för Linux för att övervaka prestanda och diagnostikdata för en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="a1469-103">Use the Linux Diagnostic Extension to monitor the performance and diagnostic data of a Linux VM</span></span>

<span data-ttu-id="a1469-104">Det här dokumentet beskriver versionen 2.3 av diagnostiska tillägget Linux.</span><span class="sxs-lookup"><span data-stu-id="a1469-104">This document describes version 2.3 of the Linux Diagnostic Extension.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a1469-105">Den här versionen är föråldrad och det kan vara opublicerad helst efter 30 juni 2018.</span><span class="sxs-lookup"><span data-stu-id="a1469-105">This version is deprecated, and it may be unpublished any time after June 30, 2018.</span></span> <span data-ttu-id="a1469-106">Den har ersatts av version 3.0.</span><span class="sxs-lookup"><span data-stu-id="a1469-106">It has been replaced by version 3.0.</span></span> <span data-ttu-id="a1469-107">Mer information finns i [dokumentationen för version 3.0 för Linux diagnostiska tillägget](../diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="a1469-107">For more information, see the [documentation for version 3.0 of the Linux Diagnostic Extension](../diagnostic-extension.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="a1469-108">Introduktion</span><span class="sxs-lookup"><span data-stu-id="a1469-108">Introduction</span></span>

<span data-ttu-id="a1469-109">(**Observera**: utvidgning diagnostiska Linux är öppen källkod på [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) där den mest aktuella informationen på tillägget publiceras först.</span><span class="sxs-lookup"><span data-stu-id="a1469-109">(**Note**: The Linux Diagnostic Extension is open-sourced on [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) where the most current information on the extension is first published.</span></span> <span data-ttu-id="a1469-110">Du kanske vill kontrollera den [GitHub-sidan](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) första.)</span><span class="sxs-lookup"><span data-stu-id="a1469-110">You might want to check the [GitHub page](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) first.)</span></span>

<span data-ttu-id="a1469-111">Tillägget Linux diagnostiska hjälper användare-övervakaren Linux virtuella datorer som körs på Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="a1469-111">The Linux Diagnostic Extension helps a user monitor the Linux VMs that are running on Microsoft Azure.</span></span> <span data-ttu-id="a1469-112">Det har följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="a1469-112">It has the following capabilities:</span></span>

* <span data-ttu-id="a1469-113">Samlar in och överför prestandainformation från Linux VM till användarens lagringstabellen, samt information om diagnostik och syslog.</span><span class="sxs-lookup"><span data-stu-id="a1469-113">Collects and uploads the system performance information from the Linux VM to the user's storage table, including diagnostic and syslog information.</span></span>
* <span data-ttu-id="a1469-114">Kan du anpassa de mätvärden för data som samlas in och överföra.</span><span class="sxs-lookup"><span data-stu-id="a1469-114">Enables users to customize the data metrics that will be collected and uploaded.</span></span>
* <span data-ttu-id="a1469-115">Gör att användarna kan ladda upp angivna loggfiler till en tabell med avsedda lagring.</span><span class="sxs-lookup"><span data-stu-id="a1469-115">Enables users to upload specified log files to a designated storage table.</span></span>

<span data-ttu-id="a1469-116">I den aktuella versionen 2.3 innehåller följande information:</span><span class="sxs-lookup"><span data-stu-id="a1469-116">In the current version 2.3, the data includes:</span></span>

* <span data-ttu-id="a1469-117">Alla Linux Rsyslog loggar, inklusive system-, säkerhets- och programloggarna.</span><span class="sxs-lookup"><span data-stu-id="a1469-117">All Linux Rsyslog logs, including system, security, and application logs.</span></span>
* <span data-ttu-id="a1469-118">Alla systemdata som har angetts på [webbplatsen för System Center Cross Platform lösningar](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="a1469-118">All system data that's specified on [the System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span>
* <span data-ttu-id="a1469-119">Loggfiler som angetts av användaren.</span><span class="sxs-lookup"><span data-stu-id="a1469-119">User-specified log files.</span></span>

<span data-ttu-id="a1469-120">Det här tillägget fungerar med både klassiskt och Resource Manager distributionsmodellerna.</span><span class="sxs-lookup"><span data-stu-id="a1469-120">This extension works with both the classic and Resource Manager deployment models.</span></span>

### <a name="current-version-of-the-extension-and-deprecation-of-old-versions"></a><span data-ttu-id="a1469-121">Aktuell version av tillägget och utfasningen av äldre versioner</span><span class="sxs-lookup"><span data-stu-id="a1469-121">Current version of the extension and deprecation of old versions</span></span>

<span data-ttu-id="a1469-122">Den senaste versionen av tillägget är **2.3**, och **eventuella gamla versioner (2.0, 2.1 och 2.2) kommer föråldrad och Opublicerat slutet av det här året (2017)**.</span><span class="sxs-lookup"><span data-stu-id="a1469-122">The latest version of the extension is **2.3**, and **any old versions (2.0, 2.1, and 2.2) will be deprecated and unpublished by end of this year (2017)**.</span></span> <span data-ttu-id="a1469-123">Om du har installerat tillägget Linux diagnostik med automatisk mindre versionsuppgradering inaktiverad rekommenderas starkt att du avinstallerar tillägget och installera om den med automatiska mindre versionsuppgradering aktiverad.</span><span class="sxs-lookup"><span data-stu-id="a1469-123">If you installed the Linux Diagnostic extension with automatic minor version upgrade disabled, it's strongly recommended that you uninstall the extension and reinstall it with automatic minor version upgrade enabled.</span></span> <span data-ttu-id="a1469-124">Du kan åstadkomma detta genom att ange '2.*' som versionen om du installerar tillägget via Azure XPLAT CLI eller Powershell på klassiska (ASM) virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a1469-124">On classic (ASM) VMs, you can achieve this by specifying '2.*' as the version if you are installing the extension through Azure XPLAT CLI or Powershell.</span></span> <span data-ttu-id="a1469-125">På ARM virtuella datorer du kan åstadkomma detta genom att inkludera ”” autoUpgradeMinorVersion ”: true' i mallen för distribution av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a1469-125">On ARM VMs, you can achieve this by including '"autoUpgradeMinorVersion": true' in the VM deployment template.</span></span> <span data-ttu-id="a1469-126">En ny installation av tillägget bör också ha den lägre versionen av automatiskt uppgradera alternativet är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="a1469-126">Also, any new installation of the extension should have the auto minor version upgrade option turned on.</span></span>

## <a name="enable-the-extension"></a><span data-ttu-id="a1469-127">Aktivera tillägget</span><span class="sxs-lookup"><span data-stu-id="a1469-127">Enable the extension</span></span>

<span data-ttu-id="a1469-128">Du kan aktivera det här tillägget med hjälp av den [Azure-portalen](https://portal.azure.com/#), Azure PowerShell eller Azure CLI-skript.</span><span class="sxs-lookup"><span data-stu-id="a1469-128">You can enable this extension by using the [Azure portal](https://portal.azure.com/#), Azure PowerShell, or Azure CLI scripts.</span></span>

<span data-ttu-id="a1469-129">Om du vill visa och konfigurera systemet och prestanda data direkt från Azure-portalen, Följ [de här stegen på Azure-bloggen](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).</span><span class="sxs-lookup"><span data-stu-id="a1469-129">To view and configure the system and performance data directly from the Azure portal, follow [these steps on the Azure blog](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).</span></span>

<span data-ttu-id="a1469-130">Den här artikeln fokuserar på hur du aktiverar och konfigurerar tillägget med hjälp av Azure CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="a1469-130">This article focuses on how to enable and configure the extension by using Azure CLI commands.</span></span> <span data-ttu-id="a1469-131">På så sätt kan du läsa och visa data direkt från lagringstabellen.</span><span class="sxs-lookup"><span data-stu-id="a1469-131">This allows you to read and view the data directly from the storage table.</span></span>

<span data-ttu-id="a1469-132">Observera att konfigurationen metoderna som beskrivs här inte fungerar för Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a1469-132">Note that the configuration methods that are described here won't work for the Azure portal.</span></span> <span data-ttu-id="a1469-133">Om du vill visa och konfigurera systemet och prestanda data direkt från Azure-portalen, måste tillägget aktiveras via portalen.</span><span class="sxs-lookup"><span data-stu-id="a1469-133">To view and configure the system and performance data directly from the Azure portal, the extension must be enabled through the portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a1469-134">Krav</span><span class="sxs-lookup"><span data-stu-id="a1469-134">Prerequisites</span></span>

* <span data-ttu-id="a1469-135">**Azure Linux-agentens version 2.0.6 eller senare**.</span><span class="sxs-lookup"><span data-stu-id="a1469-135">**Azure Linux Agent version 2.0.6 or later**.</span></span>

  <span data-ttu-id="a1469-136">Observera att de flesta Azure VM Linux galleriavbildningar inkluderar version 2.0.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a1469-136">Note that most Azure VM Linux gallery images include version 2.0.6 or later.</span></span> <span data-ttu-id="a1469-137">Du kan köra **WAAgent-version** bekräfta vilken version som är installerad på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a1469-137">You can run **WAAgent -version** to confirm which version is installed on the VM.</span></span> <span data-ttu-id="a1469-138">Om den virtuella datorn kör en version som är tidigare än 2.0.6, du kan följa [instruktionerna på GitHub](https://github.com/Azure/WALinuxAgent "instruktioner") att uppdatera den.</span><span class="sxs-lookup"><span data-stu-id="a1469-138">If the VM is running a version that's earlier than 2.0.6, you can follow [these instructions on GitHub](https://github.com/Azure/WALinuxAgent "instructions") to update it.</span></span>
* <span data-ttu-id="a1469-139">**Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="a1469-139">**Azure CLI**.</span></span> <span data-ttu-id="a1469-140">Följ [vägledningen för att installera CLI](../../../cli-install-nodejs.md) att konfigurera Azure CLI-miljö på din dator.</span><span class="sxs-lookup"><span data-stu-id="a1469-140">Follow [this guidance for installing CLI](../../../cli-install-nodejs.md) to set up the Azure CLI environment on your machine.</span></span> <span data-ttu-id="a1469-141">När Azure CLI är installerad kan du använda den **azure** från kommandoradsgränssnittet (Bash, Terminal eller Kommandotolken) att komma åt Azure CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="a1469-141">After Azure CLI is installed, you can use the **azure** command from your command-line interface (Bash, Terminal, or command prompt) to access the Azure CLI commands.</span></span> <span data-ttu-id="a1469-142">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a1469-142">For example:</span></span>

  * <span data-ttu-id="a1469-143">Kör **azure vm-tillägget set--hjälp** för närmare hjälp.</span><span class="sxs-lookup"><span data-stu-id="a1469-143">Run **azure vm extension set --help** for detailed help information.</span></span>
  * <span data-ttu-id="a1469-144">Kör **azure-inloggning** att logga in på Azure.</span><span class="sxs-lookup"><span data-stu-id="a1469-144">Run **azure login** to sign in to Azure.</span></span>
  * <span data-ttu-id="a1469-145">Kör **azure vm listan** att lista alla virtuella datorer som finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="a1469-145">Run **azure vm list** to list all the virtual machines that you have on Azure.</span></span>
* <span data-ttu-id="a1469-146">Ett lagringskonto för att lagra data.</span><span class="sxs-lookup"><span data-stu-id="a1469-146">A storage account to store the data.</span></span> <span data-ttu-id="a1469-147">Du måste namnet på ett lagringskonto som skapades tidigare och snabbtangent att överföra data till din lagring.</span><span class="sxs-lookup"><span data-stu-id="a1469-147">You will need a storage account name that was created previously and an access key to upload the data to your storage.</span></span>

## <a name="use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension"></a><span data-ttu-id="a1469-148">Använda Azure CLI-kommando för att aktivera tillägget Linux diagnostik</span><span class="sxs-lookup"><span data-stu-id="a1469-148">Use the Azure CLI command to enable the Linux Diagnostic Extension</span></span>

### <a name="scenario-1-enable-the-extension-with-the-default-data-set"></a><span data-ttu-id="a1469-149">Scenario 1.</span><span class="sxs-lookup"><span data-stu-id="a1469-149">Scenario 1.</span></span> <span data-ttu-id="a1469-150">Aktivera tillägget med standarddatauppsättningen</span><span class="sxs-lookup"><span data-stu-id="a1469-150">Enable the extension with the default data set</span></span>

<span data-ttu-id="a1469-151">I version 2.3 eller senare innehåller standarddata som samlas in:</span><span class="sxs-lookup"><span data-stu-id="a1469-151">In version 2.3 or later, the default data that will be collected includes:</span></span>

* <span data-ttu-id="a1469-152">Alla Rsyslog information (inklusive system-, säkerhets- och programloggarna).</span><span class="sxs-lookup"><span data-stu-id="a1469-152">All Rsyslog information (including system, security, and application logs).</span></span>  
* <span data-ttu-id="a1469-153">En grundläggande uppsättning bas systemdata.</span><span class="sxs-lookup"><span data-stu-id="a1469-153">A core set of basis system data.</span></span> <span data-ttu-id="a1469-154">Observera att alla data är beskrivs på den [System Center Cross Platform lösningar plats](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="a1469-154">Note that the full data set is described on the [System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span>
  <span data-ttu-id="a1469-155">Om du vill aktivera extra data fortsätter du med stegen i Scenario 2 och 3.</span><span class="sxs-lookup"><span data-stu-id="a1469-155">If you want to enable extra data, continue with the steps in Scenarios 2 and 3.</span></span>

<span data-ttu-id="a1469-156">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="a1469-156">Step 1.</span></span> <span data-ttu-id="a1469-157">Skapa en fil med namnet PrivateConfig.json med följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="a1469-157">Create a file named PrivateConfig.json with the following content:</span></span>

    {
        "storageAccountName" : "the storage account to receive data",
        "storageAccountKey" : "the key of the account"
    }

<span data-ttu-id="a1469-158">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="a1469-158">Step 2.</span></span> <span data-ttu-id="a1469-159">Kör  **azure vm-tillägget anger vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --privat-config-sökvägen PrivateConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="a1469-159">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --private-config-path PrivateConfig.json**.</span></span>

### <a name="scenario-2-customize-the-performance-monitor-metrics"></a><span data-ttu-id="a1469-160">Scenario 2.</span><span class="sxs-lookup"><span data-stu-id="a1469-160">Scenario 2.</span></span> <span data-ttu-id="a1469-161">Anpassa övervakaren prestandamått</span><span class="sxs-lookup"><span data-stu-id="a1469-161">Customize the performance monitor metrics</span></span>

<span data-ttu-id="a1469-162">Det här avsnittet beskriver hur du anpassar prestanda och diagnostikdata tabell.</span><span class="sxs-lookup"><span data-stu-id="a1469-162">This section describes how to customize the performance and diagnostic data table.</span></span>

<span data-ttu-id="a1469-163">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="a1469-163">Step 1.</span></span> <span data-ttu-id="a1469-164">Skapa en fil som heter PrivateConfig.json med det innehåll som beskrevs i Scenario 1.</span><span class="sxs-lookup"><span data-stu-id="a1469-164">Create a file named PrivateConfig.json with the content that was described in Scenario 1.</span></span> <span data-ttu-id="a1469-165">Även skapa en fil med namnet PublicConfig.json.</span><span class="sxs-lookup"><span data-stu-id="a1469-165">Also create a file named PublicConfig.json.</span></span> <span data-ttu-id="a1469-166">Ange de specifika data som du vill samla in.</span><span class="sxs-lookup"><span data-stu-id="a1469-166">Specify the particular data you want to collect.</span></span>

<span data-ttu-id="a1469-167">För alla stöds providers och variabler, referera till den [System Center Cross Platform lösningar plats](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="a1469-167">For all supported providers and variables, reference the [System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span> <span data-ttu-id="a1469-168">Du kan ha flera frågor och lagra dem på flera tabeller genom att lägga till flera frågor till skriptet.</span><span class="sxs-lookup"><span data-stu-id="a1469-168">You can have multiple queries and store them in multiple tables by appending more queries to the script.</span></span>

<span data-ttu-id="a1469-169">Rsyslog data samlas alltid som standard.</span><span class="sxs-lookup"><span data-stu-id="a1469-169">By default, the Rsyslog data is always collected.</span></span>

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


<span data-ttu-id="a1469-170">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="a1469-170">Step 2.</span></span> <span data-ttu-id="a1469-171">Kör  **azure vm-tillägget anger vm_name LinuxDiagnostic Microsoft.OSTCExtensions ' 2.*'--privat-config-sökvägen PrivateConfig.json--offentliga-config-sökvägen PublicConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="a1469-171">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.</span></span>

### <a name="scenario-3-upload-your-own-log-files"></a><span data-ttu-id="a1469-172">Scenario 3.</span><span class="sxs-lookup"><span data-stu-id="a1469-172">Scenario 3.</span></span> <span data-ttu-id="a1469-173">Ladda upp en egen loggfiler</span><span class="sxs-lookup"><span data-stu-id="a1469-173">Upload your own log files</span></span>

<span data-ttu-id="a1469-174">Det här avsnittet beskriver hur du samlar in och överföra specifika loggfiler till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a1469-174">This section describes how to collect and upload specific log files to your storage account.</span></span> <span data-ttu-id="a1469-175">Du måste ange både sökvägen till loggfilen och namnet på tabellen där du vill lagra loggen.</span><span class="sxs-lookup"><span data-stu-id="a1469-175">You need to specify both the path to your log file and the name of the table where you want to store your log.</span></span> <span data-ttu-id="a1469-176">Du kan skapa flera loggfiler genom att lägga till flera poster i filtabellen/skriptet.</span><span class="sxs-lookup"><span data-stu-id="a1469-176">You can create multiple log files by adding multiple file/table entries to the script.</span></span>

<span data-ttu-id="a1469-177">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="a1469-177">Step 1.</span></span> <span data-ttu-id="a1469-178">Skapa en fil som heter PrivateConfig.json med det innehåll som beskrevs i Scenario 1.</span><span class="sxs-lookup"><span data-stu-id="a1469-178">Create a file named PrivateConfig.json with the content that was described in Scenario 1.</span></span> <span data-ttu-id="a1469-179">Skapa sedan en annan fil som heter PublicConfig.json med följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="a1469-179">Then create another file named PublicConfig.json with the following content:</span></span>

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

<span data-ttu-id="a1469-180">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="a1469-180">Step 2.</span></span> <span data-ttu-id="a1469-181">Kör `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="a1469-181">Run `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json`.</span></span>

<span data-ttu-id="a1469-182">Observera att den här inställningen i tillägget versioner före 2.3 alla loggar skrivs till `/var/log/mysql.err` kan kopieras till `/var/log/syslog` (eller `/var/log/messages` beroende på Linux-distro) samt.</span><span class="sxs-lookup"><span data-stu-id="a1469-182">Note that with this setting on the extension versions prior to 2.3, all logs written to `/var/log/mysql.err` might be duplicated to `/var/log/syslog` (or `/var/log/messages` depending on the Linux distro) as well.</span></span> <span data-ttu-id="a1469-183">Om du vill undvika dubbla loggningen kan du undanta loggning av `local6` och loggar in rsyslog konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a1469-183">If you'd like to avoid this duplicate logging, you can exclude logging of `local6` facility logs in your rsyslog configuration.</span></span> <span data-ttu-id="a1469-184">Det beror på Linux-distro, men på ett Ubuntu 14.04 system är filen för att ändra `/etc/rsyslog.d/50-default.conf` och du kan ersätta raden `*.*;auth,authpriv.none -/var/log/syslog` till `*.*;auth,authpriv,local6.none -/var/log/syslog`.</span><span class="sxs-lookup"><span data-stu-id="a1469-184">It depends on the Linux distro, but on an Ubuntu 14.04 system, the file to modify is `/etc/rsyslog.d/50-default.conf` and you can replace the line `*.*;auth,authpriv.none -/var/log/syslog` to `*.*;auth,authpriv,local6.none -/var/log/syslog`.</span></span> <span data-ttu-id="a1469-185">Det här problemet löses i den senaste versionen snabbkorrigering för 2.3 (2.3.9007), så om du har version 2.3 för tillägg för det här problemet inte bör sker.</span><span class="sxs-lookup"><span data-stu-id="a1469-185">This issue is fixed in the latest hotfix release of 2.3 (2.3.9007), so if you have the extension version 2.3, this issue should not happen.</span></span> <span data-ttu-id="a1469-186">Om den fortfarande finns även efter att starta om den virtuella datorn, kontakta oss och hjälper oss att felsöka anledningen till att den senaste snabbkorrigeringsversionen inte installeras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="a1469-186">If it still does even after restarting your VM, please contact us and help us troubleshoot why the latest hotfix version is not installed automatically.</span></span>

### <a name="scenario-4-stop-the-extension-from-collecting-any-logs"></a><span data-ttu-id="a1469-187">Scenario 4.</span><span class="sxs-lookup"><span data-stu-id="a1469-187">Scenario 4.</span></span> <span data-ttu-id="a1469-188">Stoppa filtillägget samlar in loggar</span><span class="sxs-lookup"><span data-stu-id="a1469-188">Stop the extension from collecting any logs</span></span>

<span data-ttu-id="a1469-189">Det här avsnittet beskrivs hur du stoppar tillägget från insamling av loggar.</span><span class="sxs-lookup"><span data-stu-id="a1469-189">This section describes how to stop the extension from collecting logs.</span></span> <span data-ttu-id="a1469-190">Observera att agenten övervakningsprocessen är fortfarande aktiv och körs med den här omkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="a1469-190">Note that the monitoring agent process will be still up and running even with this reconfiguration.</span></span> <span data-ttu-id="a1469-191">Om du vill stoppa agent övervakningsprocessen helt, kan du göra det genom att inaktivera tillägget.</span><span class="sxs-lookup"><span data-stu-id="a1469-191">If you'd like to stop the monitoring agent process completely, you can do so by disabling the extension.</span></span> <span data-ttu-id="a1469-192">Kommandot för att inaktivera tillägget är `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.</span><span class="sxs-lookup"><span data-stu-id="a1469-192">The command to disable the extension is `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.</span></span>

<span data-ttu-id="a1469-193">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="a1469-193">Step 1.</span></span> <span data-ttu-id="a1469-194">Skapa en fil som heter PrivateConfig.json med det innehåll som beskrevs i Scenario 1.</span><span class="sxs-lookup"><span data-stu-id="a1469-194">Create a file named PrivateConfig.json with the content that was described in Scenario 1.</span></span> <span data-ttu-id="a1469-195">Skapa en annan fil som heter PublicConfig.json med följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="a1469-195">Create another file named PublicConfig.json with the following content:</span></span>

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


<span data-ttu-id="a1469-196">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="a1469-196">Step 2.</span></span> <span data-ttu-id="a1469-197">Kör  **azure vm-tillägget anger vm_name LinuxDiagnostic Microsoft.OSTCExtensions ' 2.*'--privat-config-sökvägen PrivateConfig.json--offentliga-config-sökvägen PublicConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="a1469-197">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.</span></span>

## <a name="review-your-data"></a><span data-ttu-id="a1469-198">Granska dina data</span><span class="sxs-lookup"><span data-stu-id="a1469-198">Review your data</span></span>

<span data-ttu-id="a1469-199">Prestanda och diagnostiska data lagras i en tabell i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a1469-199">The performance and diagnostic data are stored in an Azure Storage table.</span></span> <span data-ttu-id="a1469-200">Granska [hur du använder Azure Table Storage från Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) att lära dig hur du kommer åt data i lagringstabellen med hjälp av Azure CLI-skript.</span><span class="sxs-lookup"><span data-stu-id="a1469-200">Review [How to use Azure Table Storage from Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) to learn how to access the data in the storage table by using Azure CLI scripts.</span></span>

<span data-ttu-id="a1469-201">Dessutom kan du kan du använda följande UI-verktyg för att komma åt data:</span><span class="sxs-lookup"><span data-stu-id="a1469-201">In addition, you can use following UI tools to access the data:</span></span>

1. <span data-ttu-id="a1469-202">Visual Studio Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="a1469-202">Visual Studio Server Explorer.</span></span> <span data-ttu-id="a1469-203">Gå till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a1469-203">Go to your storage account.</span></span> <span data-ttu-id="a1469-204">När den virtuella datorn körs under fem minuter, visas de fyra standardtabeller: ”LinuxCpu”, ”LinuxDisk”, ”LinuxMemory” och ”Linuxsyslog”.</span><span class="sxs-lookup"><span data-stu-id="a1469-204">After the VM runs for about five minutes, you'll see the four default tables: “LinuxCpu”, ”LinuxDisk”, ”LinuxMemory”, and ”Linuxsyslog”.</span></span> <span data-ttu-id="a1469-205">Dubbelklicka på tabellnamn för att visa data.</span><span class="sxs-lookup"><span data-stu-id="a1469-205">Double-click the table names to view the data.</span></span>
1. <span data-ttu-id="a1469-206">[Azure Lagringsutforskaren](https://azurestorageexplorer.codeplex.com/ "Azure Lagringsutforskaren").</span><span class="sxs-lookup"><span data-stu-id="a1469-206">[Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span></span>

![Bild](./media/diagnostic-extension/no1.png)

<span data-ttu-id="a1469-208">Om du har aktiverat fileCfg eller perfCfg (enligt beskrivningen i Scenario 2 och 3), kan du använda Visual Studio Server Explorer och Azure Lagringsutforskaren för att visa data som inte är standard.</span><span class="sxs-lookup"><span data-stu-id="a1469-208">If you've enabled fileCfg or perfCfg (as described in Scenarios 2 and 3), you can use Visual Studio Server Explorer and Azure Storage Explorer to view non-default data.</span></span>

## <a name="known-issues"></a><span data-ttu-id="a1469-209">Kända problem</span><span class="sxs-lookup"><span data-stu-id="a1469-209">Known issues</span></span>

* <span data-ttu-id="a1469-210">Rsyslog information och anges av kunden loggfilen kan endast nås via skript.</span><span class="sxs-lookup"><span data-stu-id="a1469-210">The Rsyslog information and customer-specified log file can only be accessed via scripting.</span></span>
