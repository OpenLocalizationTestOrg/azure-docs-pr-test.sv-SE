---
title: "aaaDesired tillståndskonfigurationen Azure översikt | Microsoft Docs"
description: "Översikt för att använda hello Microsoft Azure-tillägget hanterare för PowerShell Desired State Configuration. Inklusive krav, arkitektur, cmdlets..."
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/09/2017
ms.author: zachal
ms.openlocfilehash: b0337a2f1124f35e5e40c1478bd7530427e59d44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-azure-desired-state-configuration-extension-handler"></a><span data-ttu-id="721ac-104">Introduktion toohello Azure Desired State Configuration-tillägget hanterare</span><span class="sxs-lookup"><span data-stu-id="721ac-104">Introduction toohello Azure Desired State Configuration extension handler</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="721ac-105">hello Azure VM-agenten och dess associerade tillägg är en del av hello Microsoft Azure Infrastructure Services.</span><span class="sxs-lookup"><span data-stu-id="721ac-105">hello Azure VM Agent and associated Extensions are part of hello Microsoft Azure Infrastructure Services.</span></span> <span data-ttu-id="721ac-106">VM-tillägg är programkomponenter som utökar hello VM funktionerna och förenkla olika VM-hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="721ac-106">VM Extensions are software components that extend hello VM functionality and simplify various VM management operations.</span></span> <span data-ttu-id="721ac-107">Till exempel hello VMAccess-tillägget kan vara används tooreset administratörens lösenord eller hello anpassat skript tillägget kan vara används tooexecute ett skript på hello VM.</span><span class="sxs-lookup"><span data-stu-id="721ac-107">For example, hello VMAccess extension can be used tooreset an administrator's password, or hello Custom Script extension can be used tooexecute a script on hello VM.</span></span>

<span data-ttu-id="721ac-108">Den här artikeln introducerar hello PowerShell önskad tillstånd Configuration DSC ()-tillägg för virtuella datorer i Azure som en del av hello Azure PowerShell SDK.</span><span class="sxs-lookup"><span data-stu-id="721ac-108">This article introduces hello PowerShell Desired State Configuration (DSC) Extension for Azure VMs as part of hello Azure PowerShell SDK.</span></span> <span data-ttu-id="721ac-109">Du kan använda nya cmdlet: ar tooupload och tillämpa en PowerShell DSC-konfiguration på en Azure VM aktiverad med hello PowerShell DSC-tillägg.</span><span class="sxs-lookup"><span data-stu-id="721ac-109">You can use new cmdlets tooupload and apply a PowerShell DSC configuration on an Azure VM enabled with hello PowerShell DSC extension.</span></span> <span data-ttu-id="721ac-110">hello PowerShell DSC-tillägg-anrop till PowerShell DSC tooenact hello emot DSC-konfigurationen på hello VM.</span><span class="sxs-lookup"><span data-stu-id="721ac-110">hello PowerShell DSC extension calls into PowerShell DSC tooenact hello received DSC configuration on hello VM.</span></span> <span data-ttu-id="721ac-111">Denna funktion är också tillgängliga via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="721ac-111">This functionality is also available through hello Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="721ac-112">Krav</span><span class="sxs-lookup"><span data-stu-id="721ac-112">Prerequisites</span></span>
<span data-ttu-id="721ac-113">**Lokal dator** toointeract med hello Azure VM-tillägget, du behöver toouse antingen hello Azure-portalen eller hello Azure PowerShell SDK.</span><span class="sxs-lookup"><span data-stu-id="721ac-113">**Local machine** toointeract with hello Azure VM extension, you need toouse either hello Azure portal or hello Azure PowerShell SDK.</span></span> 

<span data-ttu-id="721ac-114">**Gästagenten** hello Azure VM som konfigurerats av hello DSC-konfigurationen måste toobe ett operativsystem som stöder antingen Windows Management Framework (WMF) 4.0 eller 5.0.</span><span class="sxs-lookup"><span data-stu-id="721ac-114">**Guest Agent** hello Azure VM that is configured by hello DSC configuration needs toobe an OS that supports either Windows Management Framework (WMF) 4.0 or 5.0.</span></span> <span data-ttu-id="721ac-115">hello fullständig lista över operativsystemversioner som stöds finns på hello [DSC-tillägg versionshistorik](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).</span><span class="sxs-lookup"><span data-stu-id="721ac-115">hello full list of supported OS versions can be found at hello [DSC Extension Version History](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).</span></span>

## <a name="terms-and-concepts"></a><span data-ttu-id="721ac-116">Termer och begrepp</span><span class="sxs-lookup"><span data-stu-id="721ac-116">Terms and concepts</span></span>
<span data-ttu-id="721ac-117">Den här handboken förutsätter förtrogenhet med hello följande begrepp:</span><span class="sxs-lookup"><span data-stu-id="721ac-117">This guide presumes familiarity with hello following concepts:</span></span>

<span data-ttu-id="721ac-118">Konfiguration – ett dokument för DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="721ac-118">Configuration - A DSC configuration document.</span></span> 

<span data-ttu-id="721ac-119">Nod - mål för DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="721ac-119">Node - A target for a DSC configuration.</span></span> <span data-ttu-id="721ac-120">I det här dokumentet refererar ”nod” alltid tooan Azure VM.</span><span class="sxs-lookup"><span data-stu-id="721ac-120">In this document, "node" always refers tooan Azure VM.</span></span>

<span data-ttu-id="721ac-121">Konfigurationsdata - en .psd1 fil som innehåller miljödata för ett konfigurationsobjekt</span><span class="sxs-lookup"><span data-stu-id="721ac-121">Configuration Data - A .psd1 file containing environmental data for a configuration</span></span>

## <a name="architectural-overview"></a><span data-ttu-id="721ac-122">Översikt över arkitekturen</span><span class="sxs-lookup"><span data-stu-id="721ac-122">Architectural overview</span></span>
<span data-ttu-id="721ac-123">hello Azure DSC-tillägg använder hello Azure VM-agenten framework toodeliver, tillämpar och rapportera om DSC-konfigurationer som körs på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="721ac-123">hello Azure DSC extension uses hello Azure VM Agent framework toodeliver, enact, and report on DSC configurations running on Azure VMs.</span></span> <span data-ttu-id="721ac-124">hello DSC-tillägg förväntar sig en .zip-fil som innehåller minst ett konfigurationsdokument och en uppsättning parametrar som tillhandahålls via hello Azure PowerShell SDK eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="721ac-124">hello DSC extension expects a .zip file containing at least a configuration document, and a set of parameters provided either through hello Azure PowerShell SDK or through hello Azure portal.</span></span>

<span data-ttu-id="721ac-125">När tillägget hello anropas för hello första gången, kör installationen.</span><span class="sxs-lookup"><span data-stu-id="721ac-125">When hello extension is called for hello first time, it runs an installation process.</span></span> <span data-ttu-id="721ac-126">Den här processen installerar en version av hello Windows Management Framework (WMF) med hjälp av följande logik hello:</span><span class="sxs-lookup"><span data-stu-id="721ac-126">This process installs a version of hello Windows Management Framework (WMF) using hello following logic:</span></span>

1. <span data-ttu-id="721ac-127">Om Windows Server 2016 hello Azure VM OS är utförs ingen åtgärd.</span><span class="sxs-lookup"><span data-stu-id="721ac-127">If hello Azure VM OS is Windows Server 2016, no action is taken.</span></span> <span data-ttu-id="721ac-128">Windows Server 2016 har redan hello senaste versionen av PowerShell som är installerad.</span><span class="sxs-lookup"><span data-stu-id="721ac-128">Windows Server 2016 already has hello latest version of PowerShell installed.</span></span>
2. <span data-ttu-id="721ac-129">Om hello `wmfVersion` egenskapen har angetts, den versionen av hello WMF har installerats om det inte är inte kompatibel med hello VM OS.</span><span class="sxs-lookup"><span data-stu-id="721ac-129">If hello `wmfVersion` property is specified, that version of hello WMF is installed unless it is incompatible with hello VM's OS.</span></span>
3. <span data-ttu-id="721ac-130">Om inget `wmfVersion` egenskapen har angetts, hello senaste tillämpliga versionen av hello WMF har installerats.</span><span class="sxs-lookup"><span data-stu-id="721ac-130">If no `wmfVersion` property is specified, hello latest applicable version of hello WMF is installed.</span></span>

<span data-ttu-id="721ac-131">Installation av hello WMF krävs en omstart.</span><span class="sxs-lookup"><span data-stu-id="721ac-131">Installation of hello WMF requires a reboot.</span></span> <span data-ttu-id="721ac-132">Efter omstart hello tillägget hämtar hello ZIP-fil som anges i hello `modulesUrl` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="721ac-132">After reboot, hello extension downloads hello .zip file specified in hello `modulesUrl` property.</span></span> <span data-ttu-id="721ac-133">Om den här platsen är i Azure blob storage, en SAS-token kan anges i hello `sasToken` egenskapen tooaccess hello filen.</span><span class="sxs-lookup"><span data-stu-id="721ac-133">If this location is in Azure blob storage, a SAS token can be specified in hello `sasToken` property tooaccess hello file.</span></span> <span data-ttu-id="721ac-134">När hello .zip hämtas och packat upp, hello configuration-funktion som definierats i `configurationFunction` körs toogenerate hello MOF-filen.</span><span class="sxs-lookup"><span data-stu-id="721ac-134">After hello .zip is downloaded and unpacked, hello configuration function defined in `configurationFunction` is run toogenerate hello MOF file.</span></span> <span data-ttu-id="721ac-135">hello-tillägg körs sedan `Start-DscConfiguration -Force` på hello genereras MOF-filen.</span><span class="sxs-lookup"><span data-stu-id="721ac-135">hello extension then runs `Start-DscConfiguration -Force` on hello generated MOF file.</span></span> <span data-ttu-id="721ac-136">hello tillägget avbildas utdata och skriver ut toohello Azure Status kanal.</span><span class="sxs-lookup"><span data-stu-id="721ac-136">hello extension captures output and writes it back out toohello Azure Status Channel.</span></span> <span data-ttu-id="721ac-137">Från och med nu på hello hanterar DSC MGM övervakning och korrigering som vanligt.</span><span class="sxs-lookup"><span data-stu-id="721ac-137">From this point on, hello DSC LCM handles monitoring and correction as normal.</span></span> 

## <a name="powershell-cmdlets"></a><span data-ttu-id="721ac-138">PowerShell-cmdletar</span><span class="sxs-lookup"><span data-stu-id="721ac-138">PowerShell cmdlets</span></span>
<span data-ttu-id="721ac-139">PowerShell-cmdlets kan användas med Azure Resource Manager hello klassisk distribution modellen toopackage, publicera och övervaka distributioner av DSC-tillägg.</span><span class="sxs-lookup"><span data-stu-id="721ac-139">PowerShell cmdlets can be used with Azure Resource Manager or hello classic deployment model toopackage, publish, and monitor DSC extension deployments.</span></span> <span data-ttu-id="721ac-140">hello följande cmdlet: ar som visas är hello klassisk distribution moduler, men ”Azure” kan ersättas med ”AzureRm” toouse hello Azure Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="721ac-140">hello following cmdlets listed are hello classic deployment modules, but "Azure" can be replaced with "AzureRm" toouse hello Azure Resource Manager model.</span></span> <span data-ttu-id="721ac-141">Till exempel `Publish-AzureVMDscConfiguration` använder hello klassiska distributionsmodellen där `Publish-AzureRmVMDscConfiguration` använder Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="721ac-141">For example,  `Publish-AzureVMDscConfiguration` uses hello classic deployment model, where `Publish-AzureRmVMDscConfiguration` uses Azure Resource Manager.</span></span> 

<span data-ttu-id="721ac-142">`Publish-AzureVMDscConfiguration`använder en konfigurationsfil, söker efter beroende DSC-resurser och skapar en .zip-fil som innehåller hello konfiguration och DSC resurser nödvändiga tooenact hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="721ac-142">`Publish-AzureVMDscConfiguration` takes in a configuration file, scans it for dependent DSC resources, and creates a .zip file containing hello configuration and DSC resources needed tooenact hello configuration.</span></span> <span data-ttu-id="721ac-143">Det kan även skapa hello paketet lokalt med hjälp av hello `-ConfigurationArchivePath` parameter.</span><span class="sxs-lookup"><span data-stu-id="721ac-143">It can also create hello package locally using hello `-ConfigurationArchivePath` parameter.</span></span> <span data-ttu-id="721ac-144">Annars publicerar hello ZIP-filen tooAzure blob-lagring och skyddar den med en SAS-token.</span><span class="sxs-lookup"><span data-stu-id="721ac-144">Otherwise, it publishes hello .zip file tooAzure blob storage and secures it with a SAS token.</span></span>

<span data-ttu-id="721ac-145">hello ZIP-fil som skapats av denna cmdlet har hello .ps1 konfigurationsskript hello roten i hello arkivmapp.</span><span class="sxs-lookup"><span data-stu-id="721ac-145">hello .zip file created by this cmdlet has hello .ps1 configuration script at hello root of hello archive folder.</span></span> <span data-ttu-id="721ac-146">Resurser har hello modulen mappen placeras i hello arkivmapp.</span><span class="sxs-lookup"><span data-stu-id="721ac-146">Resources have hello module folder placed in hello archive folder.</span></span> 

<span data-ttu-id="721ac-147">`Set-AzureVMDscExtension`lägger in hello inställningar som hello PowerShell DSC-tillägg i ett VM-konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="721ac-147">`Set-AzureVMDscExtension` injects hello settings needed by hello PowerShell DSC extension into a VM configuration object.</span></span> <span data-ttu-id="721ac-148">I hello klassiska distributionsmodellen hello VM ändringar måste vara tillämpade tooan virtuella Azure-datorn med `Update-AzureVM`.</span><span class="sxs-lookup"><span data-stu-id="721ac-148">In hello classic deployment model, hello VM changes must be applied tooan Azure VM with `Update-AzureVM`.</span></span> 

<span data-ttu-id="721ac-149">`Get-AzureVMDscExtension`hämtar hello DSC-Tilläggsstatus för en viss virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="721ac-149">`Get-AzureVMDscExtension` retrieves hello DSC extension status of a particular VM.</span></span> 

<span data-ttu-id="721ac-150">`Get-AzureVMDscExtensionStatus`hämtar hello status hello DSC-konfigurationen trätt i kraft av hello DSC-tillägg-hanteraren.</span><span class="sxs-lookup"><span data-stu-id="721ac-150">`Get-AzureVMDscExtensionStatus` retrieves hello status of hello DSC configuration enacted by hello DSC extension handler.</span></span> <span data-ttu-id="721ac-151">Den här åtgärden kan utföras på en enda virtuell dator eller grupp med virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="721ac-151">This action can be performed on a single VM, or group of VMs.</span></span>

<span data-ttu-id="721ac-152">`Remove-AzureVMDscExtension`tar bort hello tillägget hanteraren från en viss virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="721ac-152">`Remove-AzureVMDscExtension` removes hello extension handler from a given virtual machine.</span></span> <span data-ttu-id="721ac-153">Denna cmdlet har **inte** ta bort hello konfigurationen, avinstallera hello WMF eller ändra hello tillämpas inställningarna på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="721ac-153">This cmdlet does **not** remove hello configuration, uninstall hello WMF, or change hello applied settings on hello virtual machine.</span></span> <span data-ttu-id="721ac-154">Det tar bara bort hello tillägget hanterare.</span><span class="sxs-lookup"><span data-stu-id="721ac-154">It only removes hello extension handler.</span></span> 

<span data-ttu-id="721ac-155">**Viktiga skillnader i ASM och Azure Resource Manager-cmdlets**</span><span class="sxs-lookup"><span data-stu-id="721ac-155">**Key differences in ASM and Azure Resource Manager cmdlets**</span></span>

* <span data-ttu-id="721ac-156">Azure Resource Manager-cmdlets är synkrona.</span><span class="sxs-lookup"><span data-stu-id="721ac-156">Azure Resource Manager cmdlets are synchronous.</span></span> <span data-ttu-id="721ac-157">ASM-cmdlets är asynkron.</span><span class="sxs-lookup"><span data-stu-id="721ac-157">ASM cmdlets are asynchronous.</span></span>
* <span data-ttu-id="721ac-158">ResourceGroupName, VMName, ArchiveStorageAccountName, Version och platsen är alla obligatoriska parametrar i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="721ac-158">ResourceGroupName, VMName, ArchiveStorageAccountName, Version, and Location are all required parameters in Azure Resource Manager.</span></span>
* <span data-ttu-id="721ac-159">ArchiveResourceGroupName är en ny valfri parameter för Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="721ac-159">ArchiveResourceGroupName is a new optional parameter for Azure Resource Manager.</span></span> <span data-ttu-id="721ac-160">Du kan ange den här parametern när ditt lagringskonto tillhör tooa annan resursgrupp än hello en där hello virtuell dator skapas.</span><span class="sxs-lookup"><span data-stu-id="721ac-160">You can specify this parameter when your storage account belongs tooa different resource group than hello one where hello virtual machine is created.</span></span>
* <span data-ttu-id="721ac-161">ConfigurationArchive kallas ArchiveBlobName i Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="721ac-161">ConfigurationArchive is called ArchiveBlobName in Azure Resource Manager</span></span>
* <span data-ttu-id="721ac-162">ContainerName kallas ArchiveContainerName i Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="721ac-162">ContainerName is called ArchiveContainerName in Azure Resource Manager</span></span>
* <span data-ttu-id="721ac-163">StorageEndpointSuffix kallas ArchiveStorageEndpointSuffix i Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="721ac-163">StorageEndpointSuffix is called ArchiveStorageEndpointSuffix in Azure Resource Manager</span></span>
* <span data-ttu-id="721ac-164">hello automatisk uppdatering växel har lagts tooAzure Resource Manager tooenable automatisk uppdatering hello tillägget hanteraren toohello senaste version som och när den är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="721ac-164">hello AutoUpdate switch has been added tooAzure Resource Manager tooenable automatic updating of hello extension handler toohello latest version as and when it is available.</span></span> <span data-ttu-id="721ac-165">Observera att den här parametern har hello potentiella toocause omstarter på hello VM när en ny version av hello WMF släpps.</span><span class="sxs-lookup"><span data-stu-id="721ac-165">Note this parameter has hello potential toocause reboots on hello VM when a new version of hello WMF is released.</span></span> 

## <a name="azure-portal-functionality"></a><span data-ttu-id="721ac-166">Azure portal funktioner</span><span class="sxs-lookup"><span data-stu-id="721ac-166">Azure portal functionality</span></span>
<span data-ttu-id="721ac-167">Bläddra tooa VM.</span><span class="sxs-lookup"><span data-stu-id="721ac-167">Browse tooa VM.</span></span> <span data-ttu-id="721ac-168">Under Inställningar -> Allmänt klickar du på ”Extensions”.</span><span class="sxs-lookup"><span data-stu-id="721ac-168">Under Settings -> General click "Extensions."</span></span> <span data-ttu-id="721ac-169">Ett nytt fönster skapas.</span><span class="sxs-lookup"><span data-stu-id="721ac-169">A new pane is created.</span></span> <span data-ttu-id="721ac-170">Klicka på ”Lägg till” och välj PowerShell DSC.</span><span class="sxs-lookup"><span data-stu-id="721ac-170">Click "Add" and select PowerShell DSC.</span></span>

<span data-ttu-id="721ac-171">hello-portalen måste indata.</span><span class="sxs-lookup"><span data-stu-id="721ac-171">hello portal needs input.</span></span>
<span data-ttu-id="721ac-172">**Konfiguration av moduler eller skript**: det här fältet är obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="721ac-172">**Configuration Modules or Script**: This field is mandatory.</span></span> <span data-ttu-id="721ac-173">Kräver en .ps1-fil som innehåller ett konfigurationsskript eller en .zip-fil med en .ps1-konfigurationsskript hello roten och alla beroende resurser i modulen mapparna hello .zip.</span><span class="sxs-lookup"><span data-stu-id="721ac-173">Requires a .ps1 file containing a configuration script, or a .zip file with a .ps1 configuration script at hello root, and all dependent resources in module folders within hello .zip.</span></span> <span data-ttu-id="721ac-174">Det kan skapas med hello `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` cmdlet som ingår i hello Azure PowerShell SDK.</span><span class="sxs-lookup"><span data-stu-id="721ac-174">It can be created with hello `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` cmdlet included in hello Azure PowerShell SDK.</span></span> <span data-ttu-id="721ac-175">hello ZIP-filen har överförts till ditt blob-lagring för användaren som skyddas av en SAS-token.</span><span class="sxs-lookup"><span data-stu-id="721ac-175">hello .zip file is uploaded into your user blob storage secured by a SAS token.</span></span> 

<span data-ttu-id="721ac-176">**Konfigurationsfilen Data PSD1**: det här fältet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="721ac-176">**Configuration Data PSD1 File**: This field is optional.</span></span> <span data-ttu-id="721ac-177">Om din konfiguration kräver en konfigurationsfil för data i .psd1, använder du det här fältet tooselect den och ladda upp den tooyour användaren blob-lagring, där det skyddas av en SAS-token.</span><span class="sxs-lookup"><span data-stu-id="721ac-177">If your configuration requires a configuration data file in .psd1, use this field tooselect it and upload it tooyour user blob storage, where it is secured by a SAS token.</span></span> 

<span data-ttu-id="721ac-178">**Modulkvalificerade namn Configuration**: .ps1-filer kan ha flera configuration-funktioner.</span><span class="sxs-lookup"><span data-stu-id="721ac-178">**Module-Qualified Name of Configuration**: .ps1 files can have multiple configuration functions.</span></span> <span data-ttu-id="721ac-179">Ange hello namnet på hello .ps1 konfigurationsskript följt av en '\' och hello namnet på hello configuration funktion.</span><span class="sxs-lookup"><span data-stu-id="721ac-179">Enter hello name of hello configuration .ps1 script followed by a  '\' and hello name of hello configuration function.</span></span> <span data-ttu-id="721ac-180">Om skriptet .ps1 har hello namnet ”configuration.ps1” och hello konfigurationen är ”IisInstall”, skriver du:`configuration.ps1\IisInstall`</span><span class="sxs-lookup"><span data-stu-id="721ac-180">For example, if your .ps1 script has hello name "configuration.ps1", and hello configuration is "IisInstall", you would enter: `configuration.ps1\IisInstall`</span></span>

<span data-ttu-id="721ac-181">**Konfigurationen argument**: om hello configuration funktionen tar argument, ange dem i här hello format `argumentName1=value1,argumentName2=value2`.</span><span class="sxs-lookup"><span data-stu-id="721ac-181">**Configuration Arguments**: If hello configuration function takes arguments, enter them in here in hello format `argumentName1=value1,argumentName2=value2`.</span></span> <span data-ttu-id="721ac-182">Observera att det här är ett annat format än hur configuration argument accepteras via PowerShell-cmdlets eller Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="721ac-182">Note this format is a different format than how configuration arguments are accepted through PowerShell cmdlets or Resource Manager templates.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="721ac-183">Komma igång</span><span class="sxs-lookup"><span data-stu-id="721ac-183">Getting started</span></span>
<span data-ttu-id="721ac-184">hello Azure DSC-tillägg tar i dokument för DSC-konfigurationen och utfärdar dem på Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="721ac-184">hello Azure DSC extension takes in DSC configuration documents and enacts them on Azure VMs.</span></span> <span data-ttu-id="721ac-185">Ett enkelt exempel på en konfiguration som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="721ac-185">A simple example of a configuration follows.</span></span> <span data-ttu-id="721ac-186">Spara den lokalt som ”IisInstall.ps1”:</span><span class="sxs-lookup"><span data-stu-id="721ac-186">Save it locally as "IisInstall.ps1":</span></span>

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

<span data-ttu-id="721ac-187">hello följa steg plats hello IisInstall.ps1 skript på hello angivna VM, köra hello konfiguration och rapportera om status.</span><span class="sxs-lookup"><span data-stu-id="721ac-187">hello following steps place hello IisInstall.ps1 script on hello specified VM, execute hello configuration, and report back on status.</span></span>
###<a name="classic-model"></a><span data-ttu-id="721ac-188">Klassiska modellen</span><span class="sxs-lookup"><span data-stu-id="721ac-188">Classic model</span></span>
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish hello configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set hello VM toorun hello DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "IisInstall.ps1.zip" -StorageContext $storageContext -ConfigurationName "IisInstall" -Verbose

#Update hello configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```
###<a name="azure-resource-manager-model"></a><span data-ttu-id="721ac-189">Azure Resource Manager-modellen</span><span class="sxs-lookup"><span data-stu-id="721ac-189">Azure Resource Manager model</span></span>

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish hello configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set hello VM toorun hello DSC configuration
Set-AzureRmVmDscExtension -Version 2.21 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"

```

## <a name="logging"></a><span data-ttu-id="721ac-190">Loggning</span><span class="sxs-lookup"><span data-stu-id="721ac-190">Logging</span></span>
<span data-ttu-id="721ac-191">Loggar placeras i:</span><span class="sxs-lookup"><span data-stu-id="721ac-191">Logs are placed in:</span></span>

<span data-ttu-id="721ac-192">C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[versionsnummer]</span><span class="sxs-lookup"><span data-stu-id="721ac-192">C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Version Number]</span></span>

## <a name="next-steps"></a><span data-ttu-id="721ac-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="721ac-193">Next steps</span></span>
<span data-ttu-id="721ac-194">Mer information om PowerShell DSC [finns hello PowerShell Dokumentationscenter](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="721ac-194">For more information about PowerShell DSC, [visit hello PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

<span data-ttu-id="721ac-195">Granska hello [Azure Resource Manager-mall för hello DSC-tillägg](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="721ac-195">Examine hello [Azure Resource Manager template for hello DSC extension](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="721ac-196">toofind ytterligare funktioner som du kan hantera med PowerShell DSC [Bläddra hello PowerShell-galleriet](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) mer DSC-resurser.</span><span class="sxs-lookup"><span data-stu-id="721ac-196">toofind additional functionality you can manage with PowerShell DSC, [browse hello PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) for more DSC resources.</span></span>

<span data-ttu-id="721ac-197">Mer information om skicka känslig parametrar i konfigurationer finns [hantera autentiseringsuppgifter på ett säkert sätt med hello DSC-tillägg hanteraren](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="721ac-197">For details on passing sensitive parameters into configurations, see [Manage credentials securely with hello DSC extension handler](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

