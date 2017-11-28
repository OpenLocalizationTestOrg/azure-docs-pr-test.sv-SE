---
title: "aaaAzure Compute - Linux diagnostiska tillägget | Microsoft Docs"
description: "Hur tooconfigure hello Azure Linux diagnostiska tillägg (LAD) toocollect mått och loggar händelser från virtuella Linux-datorer körs i Azure."
services: virtual-machines-linux
author: jasonzio
manager: anandram
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/09/2017
ms.author: jasonzio
ms.openlocfilehash: 2b27ec00a876ded359a75170b407e28c40d8445d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-linux-diagnostic-extension-toomonitor-metrics-and-logs"></a><span data-ttu-id="59bb8-103">Använd Linux diagnostiska tillägget toomonitor mått och loggar</span><span class="sxs-lookup"><span data-stu-id="59bb8-103">Use Linux Diagnostic Extension toomonitor metrics and logs</span></span>

<span data-ttu-id="59bb8-104">Det här dokumentet beskriver version 3.0 och senare av hello Linux diagnostiska tillägg.</span><span class="sxs-lookup"><span data-stu-id="59bb8-104">This document describes version 3.0 and newer of hello Linux Diagnostic Extension.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="59bb8-105">Information om version 2.3 och äldre finns [dokumentet](./classic/diagnostic-extension-v2.md).</span><span class="sxs-lookup"><span data-stu-id="59bb8-105">For information about version 2.3 and older, see [this document](./classic/diagnostic-extension-v2.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="59bb8-106">Introduktion</span><span class="sxs-lookup"><span data-stu-id="59bb8-106">Introduction</span></span>

<span data-ttu-id="59bb8-107">hello Linux diagnostiska tillägget hjälper användare Övervakare hello hälsa för en Linux-VM som körs på Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="59bb8-107">hello Linux Diagnostic Extension helps a user monitor hello health of a Linux VM running on Microsoft Azure.</span></span> <span data-ttu-id="59bb8-108">Det har hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="59bb8-108">It has hello following capabilities:</span></span>

* <span data-ttu-id="59bb8-109">Samlar in system prestandamått från hello VM och lagrar dem i en viss tabell i ett avsedda storage-konto.</span><span class="sxs-lookup"><span data-stu-id="59bb8-109">Collects system performance metrics from hello VM and stores them in a specific table in a designated storage account.</span></span>
* <span data-ttu-id="59bb8-110">Hämtar händelser från syslog och lagrar dem i en viss tabell i hello avses storage-konto.</span><span class="sxs-lookup"><span data-stu-id="59bb8-110">Retrieves log events from syslog and stores them in a specific table in hello designated storage account.</span></span>
* <span data-ttu-id="59bb8-111">Låter användare toocustomize hello data mått som samlas in och överföra.</span><span class="sxs-lookup"><span data-stu-id="59bb8-111">Enables users toocustomize hello data metrics that are collected and uploaded.</span></span>
* <span data-ttu-id="59bb8-112">Kan användare toocustomize hello syslog verksamhet och allvarlighetsgrader för händelser som samlas in och överföra.</span><span class="sxs-lookup"><span data-stu-id="59bb8-112">Enables users toocustomize hello syslog facilities and severity levels of events that are collected and uploaded.</span></span>
* <span data-ttu-id="59bb8-113">Låter användare tooupload angivna filer tooa avsedda lagringstabellen.</span><span class="sxs-lookup"><span data-stu-id="59bb8-113">Enables users tooupload specified log files tooa designated storage table.</span></span>
* <span data-ttu-id="59bb8-114">Stöder skicka mått och logga händelser tooarbitrary EventHub slutpunkter och JSON-formaterad blobbar i hello utsedda storage-konto.</span><span class="sxs-lookup"><span data-stu-id="59bb8-114">Supports sending metrics and log events tooarbitrary EventHub endpoints and JSON-formatted blobs in hello designated storage account.</span></span>

<span data-ttu-id="59bb8-115">Det här tillägget fungerar med båda modellerna i Azure-distribution.</span><span class="sxs-lookup"><span data-stu-id="59bb8-115">This extension works with both Azure deployment models.</span></span>

## <a name="installing-hello-extension-in-your-vm"></a><span data-ttu-id="59bb8-116">Installera hello-tillägget på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="59bb8-116">Installing hello extension in your VM</span></span>

<span data-ttu-id="59bb8-117">Du kan aktivera det här tillägget med hjälp av hello Azure PowerShell-cmdlets, Azure CLI-skript eller mallar för Azure-distribution.</span><span class="sxs-lookup"><span data-stu-id="59bb8-117">You can enable this extension by using hello Azure PowerShell cmdlets, Azure CLI scripts, or Azure deployment templates.</span></span> <span data-ttu-id="59bb8-118">Mer information finns i [tillägg funktioner](./extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="59bb8-118">For more information, see [Extensions Features](./extensions-features.md).</span></span>

<span data-ttu-id="59bb8-119">hello Azure-portalen kan inte använda tooenable eller konfigurera LAD 3.0.</span><span class="sxs-lookup"><span data-stu-id="59bb8-119">hello Azure portal cannot be used tooenable or configure LAD 3.0.</span></span> <span data-ttu-id="59bb8-120">Istället installerar och konfigurerar version 2.3.</span><span class="sxs-lookup"><span data-stu-id="59bb8-120">Instead, it installs and configures version 2.3.</span></span> <span data-ttu-id="59bb8-121">Azure portal diagram och aviseringar kan du arbeta med data från båda versionerna av hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="59bb8-121">Azure portal graphs and alerts work with data from both versions of hello extension.</span></span>

<span data-ttu-id="59bb8-122">Dessa instruktioner för installation och en [nedladdningsbara exempelkonfiguration](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) konfigurera LAD 3.0 till:</span><span class="sxs-lookup"><span data-stu-id="59bb8-122">These installation instructions and a [downloadable sample configuration](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) configure LAD 3.0 to:</span></span>

* <span data-ttu-id="59bb8-123">avbilda och lagra hello samma mått som tillhandahålls av LAD 2.3;</span><span class="sxs-lookup"><span data-stu-id="59bb8-123">capture and store hello same metrics as were provided by LAD 2.3;</span></span>
* <span data-ttu-id="59bb8-124">avbilda en användbar uppsättning filen system statistik, nya tooLAD 3.0;</span><span class="sxs-lookup"><span data-stu-id="59bb8-124">capture a useful set of file system metrics, new tooLAD 3.0;</span></span>
* <span data-ttu-id="59bb8-125">Avbilda hello syslog standardsamling aktiveras med LAD 2.3;</span><span class="sxs-lookup"><span data-stu-id="59bb8-125">capture hello default syslog collection enabled by LAD 2.3;</span></span>
* <span data-ttu-id="59bb8-126">Aktivera hello Azure portal upplevelsen för diagram och varningar på VM-mått.</span><span class="sxs-lookup"><span data-stu-id="59bb8-126">enable hello Azure portal experience for charting and alerting on VM metrics.</span></span>

<span data-ttu-id="59bb8-127">hello nedladdningsbara konfigurationen är bara ett exempel. ändra den toosuit dina egna behov.</span><span class="sxs-lookup"><span data-stu-id="59bb8-127">hello downloadable configuration is just an example; modify it toosuit your own needs.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="59bb8-128">Krav</span><span class="sxs-lookup"><span data-stu-id="59bb8-128">Prerequisites</span></span>

* <span data-ttu-id="59bb8-129">**Azure Linux-agentens version 2.2.0 eller senare**.</span><span class="sxs-lookup"><span data-stu-id="59bb8-129">**Azure Linux Agent version 2.2.0 or later**.</span></span> <span data-ttu-id="59bb8-130">De flesta Azure VM Linux galleriavbildningar inkluderar version 2.2.7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="59bb8-130">Most Azure VM Linux gallery images include version 2.2.7 or later.</span></span> <span data-ttu-id="59bb8-131">Kör `/usr/sbin/waagent -version` tooconfirm hello version är installerad på hello VM.</span><span class="sxs-lookup"><span data-stu-id="59bb8-131">Run `/usr/sbin/waagent -version` tooconfirm hello version installed on hello VM.</span></span> <span data-ttu-id="59bb8-132">Så om hello VM använder en äldre version av gästagenten för hello [instruktionerna](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) tooupdate den.</span><span class="sxs-lookup"><span data-stu-id="59bb8-132">If hello VM is running an older version of hello guest agent, follow [these instructions](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) tooupdate it.</span></span>
* <span data-ttu-id="59bb8-133">**Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="59bb8-133">**Azure CLI**.</span></span> <span data-ttu-id="59bb8-134">[Ställ in hello Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) miljö på din dator.</span><span class="sxs-lookup"><span data-stu-id="59bb8-134">[Set up hello Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) environment on your machine.</span></span>
* <span data-ttu-id="59bb8-135">Hej wget-kommando om du inte redan har det: kör `sudo apt-get install wget`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-135">hello wget command, if you don't already have it: Run `sudo apt-get install wget`.</span></span>
* <span data-ttu-id="59bb8-136">En befintlig Azure-prenumeration och befintligt lagringsutrymme för ett konto i den toostore hello data.</span><span class="sxs-lookup"><span data-stu-id="59bb8-136">An existing Azure subscription and an existing storage account within it toostore hello data.</span></span>

### <a name="sample-installation"></a><span data-ttu-id="59bb8-137">Exempel installation</span><span class="sxs-lookup"><span data-stu-id="59bb8-137">Sample installation</span></span>

<span data-ttu-id="59bb8-138">Fyll i hello rätt parametrar på hello först tre rader och sedan köra skriptet som rot:</span><span class="sxs-lookup"><span data-stu-id="59bb8-138">Fill in hello correct parameters on hello first three lines, then execute this script as root:</span></span>

```bash
# Set your Azure VM diagnostic parameters correctly below
my_resource_group=<your_azure_resource_group_name_containing_your_azure_linux_vm>
my_linux_vm=<your_azure_linux_vm_name>
my_diagnostic_storage_account=<your_azure_storage_account_for_storing_vm_diagnostic_data>

# Should login tooAzure first before anything else
az login

# Select hello subscription containing hello storage account
az account set --subscription <your_azure_subscription_id>

# Download hello sample Public settings. (You could also use curl or any web browser)
wget https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json -O portal_public_settings.json

# Build hello VM resource ID. Replace storage account name and resource ID in hello public settings.
my_vm_resource_id=$(az vm show -g $my_resource_group -n $my_linux_vm --query "id" -o tsv)
sed -i "s#__DIAGNOSTIC_STORAGE_ACCOUNT__#$my_diagnostic_storage_account#g" portal_public_settings.json
sed -i "s#__VM_RESOURCE_ID__#$my_vm_resource_id#g" portal_public_settings.json

# Build hello protected settings (storage account SAS token)
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 9999-12-31T23:59Z --permissions wlacu --resource-types co --services bt -o tsv)
my_lad_protected_settings="{'storageAccountName': '$my_diagnostic_storage_account', 'storageAccountSasToken': '$my_diagnostic_storage_account_sastoken'}"

# Finallly tell Azure tooinstall and enable hello extension
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 3.0 --resource-group $my_resource_group --vm-name $my_linux_vm --protected-settings "${my_lad_protected_settings}" --settings portal_public_settings.json
```

<span data-ttu-id="59bb8-139">hello-URL för hello exempelkonfiguration och dess innehåll är ämne toochange.</span><span class="sxs-lookup"><span data-stu-id="59bb8-139">hello URL for hello sample configuration, and its contents, are subject toochange.</span></span> <span data-ttu-id="59bb8-140">Hämta en kopia av hello portalinställningar JSON-fil och anpassa den efter dina behov.</span><span class="sxs-lookup"><span data-stu-id="59bb8-140">Download a copy of hello portal settings JSON file and customize it for your needs.</span></span> <span data-ttu-id="59bb8-141">Alla mallar eller automatisering som du bör använda din egen kopia, i stället för att hämta URL: en varje gång.</span><span class="sxs-lookup"><span data-stu-id="59bb8-141">Any templates or automation you construct should use your own copy, rather than downloading that URL each time.</span></span>

### <a name="updating-hello-extension-settings"></a><span data-ttu-id="59bb8-142">Uppdaterar inställningar för hello-tillägg</span><span class="sxs-lookup"><span data-stu-id="59bb8-142">Updating hello extension settings</span></span>

<span data-ttu-id="59bb8-143">När du har ändrat din skyddade eller inställningar för offentliga kan distribuera dem toohello VM genom att köra hello samma kommando.</span><span class="sxs-lookup"><span data-stu-id="59bb8-143">After you've changed your Protected or Public settings, deploy them toohello VM by running hello same command.</span></span> <span data-ttu-id="59bb8-144">Om något ändras i inställningarna för hello skickas hello uppdaterade inställningar toohello tillägg.</span><span class="sxs-lookup"><span data-stu-id="59bb8-144">If anything changed in hello settings, hello updated settings are sent toohello extension.</span></span> <span data-ttu-id="59bb8-145">LAD läser hello-konfigurationen och startar om sig själv.</span><span class="sxs-lookup"><span data-stu-id="59bb8-145">LAD reloads hello configuration and restarts itself.</span></span>

### <a name="migration-from-previous-versions-of-hello-extension"></a><span data-ttu-id="59bb8-146">Migrering från tidigare versioner av hello-tillägg</span><span class="sxs-lookup"><span data-stu-id="59bb8-146">Migration from previous versions of hello extension</span></span>

<span data-ttu-id="59bb8-147">hello senaste versionen av tillägget hello **3.0**.</span><span class="sxs-lookup"><span data-stu-id="59bb8-147">hello latest version of hello extension is **3.0**.</span></span> <span data-ttu-id="59bb8-148">**Eventuella gamla versioner (2.x) är föråldrade och kan vara opublicerad på eller efter den 31 juli 2018**.</span><span class="sxs-lookup"><span data-stu-id="59bb8-148">**Any old versions (2.x) are deprecated and may be unpublished on or after July 31, 2018**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="59bb8-149">Det här tillägget introducerar senaste ändringar toohello konfigurationen av hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="59bb8-149">This extension introduces breaking changes toohello configuration of hello extension.</span></span> <span data-ttu-id="59bb8-150">En ändring har gjorts tooimprove hello säkerheten för hello tillägget; Därför kan bakåtkompatibilitet kompatibilitet med 2.x inte upprätthållas.</span><span class="sxs-lookup"><span data-stu-id="59bb8-150">One such change was made tooimprove hello security of hello extension; as a result, backwards compatibility with 2.x could not be maintained.</span></span> <span data-ttu-id="59bb8-151">Dessutom skiljer hello tillägget Publisher för detta tillägg sig hello utgivaren för hello 2.x-versionerna.</span><span class="sxs-lookup"><span data-stu-id="59bb8-151">Also, hello Extension Publisher for this extension is different than hello publisher for hello 2.x versions.</span></span>
>
> <span data-ttu-id="59bb8-152">toomigrate från 2.x toothis ny version av hello tillägget måste du avinstallera hello gamla tillägget (under hello gamla utgivarens namn) och sedan installera version 3 för hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="59bb8-152">toomigrate from 2.x toothis new version of hello extension, you must uninstall hello old extension (under hello old publisher name), then install version 3 of hello extension.</span></span>

<span data-ttu-id="59bb8-153">Rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="59bb8-153">Recommendations:</span></span>

* <span data-ttu-id="59bb8-154">Installera hello-tillägget med automatisk mindre versionsuppgradering aktiverad.</span><span class="sxs-lookup"><span data-stu-id="59bb8-154">Install hello extension with automatic minor version upgrade enabled.</span></span>
  * <span data-ttu-id="59bb8-155">På klassiska distributionsmodellen VMs, anger du '3.*' hello versionen om du installerar tillägget hello via Azure XPLAT CLI eller Powershell.</span><span class="sxs-lookup"><span data-stu-id="59bb8-155">On classic deployment model VMs, specify '3.*' as hello version if you are installing hello extension through Azure XPLAT CLI or Powershell.</span></span>
  * <span data-ttu-id="59bb8-156">Modellen virtuella datorer på Azure Resource Manager-distribution kan du ange ' ”autoUpgradeMinorVersion”: true' i hello mall för distribution.</span><span class="sxs-lookup"><span data-stu-id="59bb8-156">On Azure Resource Manager deployment model VMs, include '"autoUpgradeMinorVersion": true' in hello VM deployment template.</span></span>
* <span data-ttu-id="59bb8-157">Använd en ny/annat lagringskonto för LAD 3.0.</span><span class="sxs-lookup"><span data-stu-id="59bb8-157">Use a new/different storage account for LAD 3.0.</span></span> <span data-ttu-id="59bb8-158">Det finns flera små inkonsekvenser mellan LAD 2.3 och LAD 3.0 som gör att dela ett program som krånglar konto:</span><span class="sxs-lookup"><span data-stu-id="59bb8-158">There are several small incompatibilities between LAD 2.3 and LAD 3.0 that make sharing an account troublesome:</span></span>
  * <span data-ttu-id="59bb8-159">LAD 3.0 lagrar syslog-händelser i en tabell med ett annat namn.</span><span class="sxs-lookup"><span data-stu-id="59bb8-159">LAD 3.0 stores syslog events in a table with a different name.</span></span>
  * <span data-ttu-id="59bb8-160">Hej counterSpecifier strängarna för `builtin` mått skiljer sig åt i LAD 3.0.</span><span class="sxs-lookup"><span data-stu-id="59bb8-160">hello counterSpecifier strings for `builtin` metrics differ in LAD 3.0.</span></span>

## <a name="protected-settings"></a><span data-ttu-id="59bb8-161">Skyddade inställningar</span><span class="sxs-lookup"><span data-stu-id="59bb8-161">Protected settings</span></span>

<span data-ttu-id="59bb8-162">Denna uppsättning konfigurationsinformation innehåller känslig information som borde vara skyddad från den gemensamma vyn, till exempel lagring autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="59bb8-162">This set of configuration information contains sensitive information that should be protected from public view, for example, storage credentials.</span></span> <span data-ttu-id="59bb8-163">Dessa inställningar är överförda tooand lagras av hello tillägg i krypterad form.</span><span class="sxs-lookup"><span data-stu-id="59bb8-163">These settings are transmitted tooand stored by hello extension in encrypted form.</span></span>

```json
{
    "storageAccountName" : "hello storage account tooreceive data",
    "storageAccountEndPoint": "hello hostname suffix for hello cloud for this account",
    "storageAccountSasToken": "SAS access token",
    "mdsdHttpProxy": "HTTP proxy settings",
    "sinksConfig": { ... }
}
```

<span data-ttu-id="59bb8-164">Namn</span><span class="sxs-lookup"><span data-stu-id="59bb8-164">Name</span></span> | <span data-ttu-id="59bb8-165">Värde</span><span class="sxs-lookup"><span data-stu-id="59bb8-165">Value</span></span>
---- | -----
<span data-ttu-id="59bb8-166">storageAccountName</span><span class="sxs-lookup"><span data-stu-id="59bb8-166">storageAccountName</span></span> | <span data-ttu-id="59bb8-167">hello namnet på hello lagringskonto där data skrivs av hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="59bb8-167">hello name of hello storage account in which data is written by hello extension.</span></span>
<span data-ttu-id="59bb8-168">storageAccountEndPoint</span><span class="sxs-lookup"><span data-stu-id="59bb8-168">storageAccountEndPoint</span></span> | <span data-ttu-id="59bb8-169">(valfritt) hello-slutpunkt som identifierar hello moln i vilka hello lagringskontot finns.</span><span class="sxs-lookup"><span data-stu-id="59bb8-169">(optional) hello endpoint identifying hello cloud in which hello storage account exists.</span></span> <span data-ttu-id="59bb8-170">Om den här inställningen saknas LAD standard toohello offentliga Azure-molnet, `https://core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-170">If this setting is absent, LAD defaults toohello Azure public cloud, `https://core.windows.net`.</span></span> <span data-ttu-id="59bb8-171">toouse ett lagringskonto i Azure Tyskland eller Azure Government Azure Kina, ange värdet i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="59bb8-171">toouse a storage account in Azure Germany, Azure Government, or Azure China, set this value accordingly.</span></span>
<span data-ttu-id="59bb8-172">storageAccountSasToken</span><span class="sxs-lookup"><span data-stu-id="59bb8-172">storageAccountSasToken</span></span> | <span data-ttu-id="59bb8-173">En [konto-SAS-token](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) för Blob- och tjänster (`ss='bt'`), tillämpliga toocontainers och objekt (`srt='co'`), vilket ger lägga till, skapa, visa, uppdatera och skrivbehörighet (`sp='acluw'`).</span><span class="sxs-lookup"><span data-stu-id="59bb8-173">An [Account SAS token](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) for Blob and Table services (`ss='bt'`), applicable toocontainers and objects (`srt='co'`), which grants add, create, list, update, and write permissions (`sp='acluw'`).</span></span> <span data-ttu-id="59bb8-174">Gör *inte* inkluderar hello inledande frågetecken (?).</span><span class="sxs-lookup"><span data-stu-id="59bb8-174">Do *not* include hello leading question-mark (?).</span></span>
<span data-ttu-id="59bb8-175">mdsdHttpProxy</span><span class="sxs-lookup"><span data-stu-id="59bb8-175">mdsdHttpProxy</span></span> | <span data-ttu-id="59bb8-176">(valfritt) HTTP-proxy information som behövs för tooenable hello tillägget tooconnect toohello angetts storage-konto och slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="59bb8-176">(optional) HTTP proxy information needed tooenable hello extension tooconnect toohello specified storage account and endpoint.</span></span>
<span data-ttu-id="59bb8-177">sinksConfig</span><span class="sxs-lookup"><span data-stu-id="59bb8-177">sinksConfig</span></span> | <span data-ttu-id="59bb8-178">(valfritt) Information om alternativa mål toowhich mått och händelser levereras.</span><span class="sxs-lookup"><span data-stu-id="59bb8-178">(optional) Details of alternative destinations toowhich metrics and events can be delivered.</span></span> <span data-ttu-id="59bb8-179">hello avsnitten som följer beskrivs hello närmare för varje mottagare för data som stöds av hello extension.</span><span class="sxs-lookup"><span data-stu-id="59bb8-179">hello specific details of each data sink supported by hello extension are covered in hello sections that follow.</span></span>

<span data-ttu-id="59bb8-180">Du kan enkelt skapa hello krävs SAS-token via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="59bb8-180">You can easily construct hello required SAS token through hello Azure portal.</span></span>

1. <span data-ttu-id="59bb8-181">Välj önskade hello tillägget toowrite hello Allmänt lagringskonto konto toowhich</span><span class="sxs-lookup"><span data-stu-id="59bb8-181">Select hello general-purpose storage account toowhich you want hello extension toowrite</span></span>
1. <span data-ttu-id="59bb8-182">Välj ”delad åtkomstsignatur” från hello inställningar tillhör hello vänstra menyn</span><span class="sxs-lookup"><span data-stu-id="59bb8-182">Select "Shared access signature" from hello Settings part of hello left menu</span></span>
1. <span data-ttu-id="59bb8-183">Se hello relevanta avsnitt enligt beskrivningen ovan</span><span class="sxs-lookup"><span data-stu-id="59bb8-183">Make hello appropriate sections as previously described</span></span>
1. <span data-ttu-id="59bb8-184">Klicka hello ”generera SAS”.</span><span class="sxs-lookup"><span data-stu-id="59bb8-184">Click hello "Generate SAS" button.</span></span>

![Bild](./media/diagnostic-extension/make_sas.png)

<span data-ttu-id="59bb8-186">Kopiera hello genereras SAS i hello storageAccountSasToken område. ta bort hello inledande frågetecken (””?).</span><span class="sxs-lookup"><span data-stu-id="59bb8-186">Copy hello generated SAS into hello storageAccountSasToken field; remove hello leading question-mark ("?").</span></span>

### <a name="sinksconfig"></a><span data-ttu-id="59bb8-187">sinksConfig</span><span class="sxs-lookup"><span data-stu-id="59bb8-187">sinksConfig</span></span>

```json
"sinksConfig": {
    "sink": [
        {
            "name": "sinkname",
            "type": "sinktype",
            ...
        },
        ...
    ]
},
```

<span data-ttu-id="59bb8-188">Det här valfria avsnittet definierar ytterligare mål toowhich hello tillägget skickar hello information som samlas in.</span><span class="sxs-lookup"><span data-stu-id="59bb8-188">This optional section defines additional destinations toowhich hello extension sends hello information it collects.</span></span> <span data-ttu-id="59bb8-189">Hej ”sink” matris innehåller ett objekt för varje ytterligare data sink.</span><span class="sxs-lookup"><span data-stu-id="59bb8-189">hello "sink" array contains an object for each additional data sink.</span></span> <span data-ttu-id="59bb8-190">attributet ”typ” avgör hello hello andra attribut i hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="59bb8-190">hello "type" attribute determines hello other attributes in hello object.</span></span>

<span data-ttu-id="59bb8-191">Element</span><span class="sxs-lookup"><span data-stu-id="59bb8-191">Element</span></span> | <span data-ttu-id="59bb8-192">Värde</span><span class="sxs-lookup"><span data-stu-id="59bb8-192">Value</span></span>
------- | -----
<span data-ttu-id="59bb8-193">namn</span><span class="sxs-lookup"><span data-stu-id="59bb8-193">name</span></span> | <span data-ttu-id="59bb8-194">En sträng används toorefer toothis sink någon annanstans i hello tilläggets konfiguration.</span><span class="sxs-lookup"><span data-stu-id="59bb8-194">A string used toorefer toothis sink elsewhere in hello extension configuration.</span></span>
<span data-ttu-id="59bb8-195">typ</span><span class="sxs-lookup"><span data-stu-id="59bb8-195">type</span></span> | <span data-ttu-id="59bb8-196">hello typ av mottagare som definieras.</span><span class="sxs-lookup"><span data-stu-id="59bb8-196">hello type of sink being defined.</span></span> <span data-ttu-id="59bb8-197">Anger hello andra värden (eventuella) i instanser av den här typen.</span><span class="sxs-lookup"><span data-stu-id="59bb8-197">Determines hello other values (if any) in instances of this type.</span></span>

<span data-ttu-id="59bb8-198">Hello Linux diagnostiska tillägg version 3.0 stöder två typer av mottagare: EventHub och JsonBlob.</span><span class="sxs-lookup"><span data-stu-id="59bb8-198">Version 3.0 of hello Linux Diagnostic Extension supports two sink types: EventHub, and JsonBlob.</span></span>

#### <a name="hello-eventhub-sink"></a><span data-ttu-id="59bb8-199">Hej EventHub mottagare</span><span class="sxs-lookup"><span data-stu-id="59bb8-199">hello EventHub sink</span></span>

```json
"sink": [
    {
        "name": "sinkname",
        "type": "EventHub",
        "sasURL": "https SAS URL"
    },
    ...
]
```

<span data-ttu-id="59bb8-200">Hej ”sasURL” post innehåller hello fullständig Webbadress som innehåller SAS-token för hello Event Hub toowhich data måste publiceras.</span><span class="sxs-lookup"><span data-stu-id="59bb8-200">hello "sasURL" entry contains hello full URL, including SAS token, for hello Event Hub toowhich data should be published.</span></span> <span data-ttu-id="59bb8-201">LAD kräver en SAS namngivning av en princip som aktiverar hello skicka anspråk.</span><span class="sxs-lookup"><span data-stu-id="59bb8-201">LAD requires a SAS naming a policy that enables hello Send claim.</span></span> <span data-ttu-id="59bb8-202">Ett exempel:</span><span class="sxs-lookup"><span data-stu-id="59bb8-202">An example:</span></span>

* <span data-ttu-id="59bb8-203">Skapa ett namnområde för Händelsehubbar kallas`contosohub`</span><span class="sxs-lookup"><span data-stu-id="59bb8-203">Create an Event Hubs namespace called `contosohub`</span></span>
* <span data-ttu-id="59bb8-204">Skapa en Händelsehubb i hello-namnområde som kallas`syslogmsgs`</span><span class="sxs-lookup"><span data-stu-id="59bb8-204">Create an Event Hub in hello namespace called `syslogmsgs`</span></span>
* <span data-ttu-id="59bb8-205">Skapa en princip för delad åtkomst på hello Event Hub med namnet `writer` som aktiverar hello skicka anspråk</span><span class="sxs-lookup"><span data-stu-id="59bb8-205">Create a Shared access policy on hello Event Hub named `writer` that enables hello Send claim</span></span>

<span data-ttu-id="59bb8-206">Om du har skapat en SAS bra till midnatt UTC på den 1 januari 2018 kan hello sasURL värdet vara:</span><span class="sxs-lookup"><span data-stu-id="59bb8-206">If you created a SAS good until midnight UTC on January 1, 2018, hello sasURL value might be:</span></span>

```url
https://contosohub.servicebus.windows.net/syslogmsgs?sr=contosohub.servicebus.windows.net%2fsyslogmsgs&sig=xxxxxxxxxxxxxxxxxxxxxxxxx&se=1514764800&skn=writer
```

<span data-ttu-id="59bb8-207">Mer information om hur du genererar SAS-token för Händelsehubbar finns [webbsidan](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).</span><span class="sxs-lookup"><span data-stu-id="59bb8-207">For more information about generating SAS tokens for Event Hubs, see [this web page](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).</span></span>

#### <a name="hello-jsonblob-sink"></a><span data-ttu-id="59bb8-208">Hej JsonBlob mottagare</span><span class="sxs-lookup"><span data-stu-id="59bb8-208">hello JsonBlob sink</span></span>

```json
"sink": [
    {
        "name": "sinkname",
        "type": "JsonBlob"
    },
    ...
]
```

<span data-ttu-id="59bb8-209">Data dirigeras tooa JsonBlob sink är lagrade i blobar i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="59bb8-209">Data directed tooa JsonBlob sink is stored in blobs in Azure storage.</span></span> <span data-ttu-id="59bb8-210">Varje instans av LAD skapar en blob varje timme för varje mottagare namn.</span><span class="sxs-lookup"><span data-stu-id="59bb8-210">Each instance of LAD creates a blob every hour for each sink name.</span></span> <span data-ttu-id="59bb8-211">Varje blobb innehåller alltid en syntaktiskt giltig JSON-matris med objektet.</span><span class="sxs-lookup"><span data-stu-id="59bb8-211">Each blob always contains a syntactically valid JSON array of object.</span></span> <span data-ttu-id="59bb8-212">Nya poster läggs automatiskt till toohello matris.</span><span class="sxs-lookup"><span data-stu-id="59bb8-212">New entries are atomically added toohello array.</span></span> <span data-ttu-id="59bb8-213">Blobbar som lagras i en behållare med samma namn som hello sink hello.</span><span class="sxs-lookup"><span data-stu-id="59bb8-213">Blobs are stored in a container with hello same name as hello sink.</span></span> <span data-ttu-id="59bb8-214">hello Azure storage-regler för blob-behållarnamn tillämpas toohello namnen på JsonBlob sänkor: mellan 3 och 63 gemena alfanumeriska ASCII-tecken eller bindestreck.</span><span class="sxs-lookup"><span data-stu-id="59bb8-214">hello Azure storage rules for blob container names apply toohello names of JsonBlob sinks: between 3 and 63 lower-case alphanumeric ASCII characters or dashes.</span></span>

## <a name="public-settings"></a><span data-ttu-id="59bb8-215">Inställningar för offentliga</span><span class="sxs-lookup"><span data-stu-id="59bb8-215">Public settings</span></span>

<span data-ttu-id="59bb8-216">Den här strukturen innehåller olika block med inställningar som styr hello information som samlas in av hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="59bb8-216">This structure contains various blocks of settings that control hello information collected by hello extension.</span></span> <span data-ttu-id="59bb8-217">Varje inställningen är valfri.</span><span class="sxs-lookup"><span data-stu-id="59bb8-217">Each setting is optional.</span></span> <span data-ttu-id="59bb8-218">Om du anger `ladCfg`, måste du också ange `StorageAccount`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-218">If you specify `ladCfg`, you must also specify `StorageAccount`.</span></span>

```json
{
    "ladCfg":  { ... },
    "perfCfg": { ... },
    "fileLogs": { ... },
    "StorageAccount": "hello storage account tooreceive data",
    "mdsdHttpProxy" : ""
}
```

<span data-ttu-id="59bb8-219">Element</span><span class="sxs-lookup"><span data-stu-id="59bb8-219">Element</span></span> | <span data-ttu-id="59bb8-220">Värde</span><span class="sxs-lookup"><span data-stu-id="59bb8-220">Value</span></span>
------- | -----
<span data-ttu-id="59bb8-221">StorageAccount</span><span class="sxs-lookup"><span data-stu-id="59bb8-221">StorageAccount</span></span> | <span data-ttu-id="59bb8-222">hello namnet på hello lagringskonto där data skrivs av hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="59bb8-222">hello name of hello storage account in which data is written by hello extension.</span></span> <span data-ttu-id="59bb8-223">Måste vara samma namn som har angetts i hello hello [skyddade inställningarna](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="59bb8-223">Must be hello same name as is specified in hello [Protected settings](#protected-settings).</span></span>
<span data-ttu-id="59bb8-224">mdsdHttpProxy</span><span class="sxs-lookup"><span data-stu-id="59bb8-224">mdsdHttpProxy</span></span> | <span data-ttu-id="59bb8-225">(valfritt) Samma som i hello [skyddade inställningarna](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="59bb8-225">(optional) Same as in hello [Protected settings](#protected-settings).</span></span> <span data-ttu-id="59bb8-226">hello offentliga värde åsidosätts av hello privata värde, om ange.</span><span class="sxs-lookup"><span data-stu-id="59bb8-226">hello public value is overridden by hello private value, if set.</span></span> <span data-ttu-id="59bb8-227">Placera proxyinställningar som innehåller en hemlighet, till exempel ett lösenord i hello [skyddade inställningarna](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="59bb8-227">Place proxy settings that contain a secret, such as a password, in hello [Protected settings](#protected-settings).</span></span>

<span data-ttu-id="59bb8-228">hello återstående elementen beskrivs detaljerat i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="59bb8-228">hello remaining elements are described in detail in hello following sections.</span></span>

### <a name="ladcfg"></a><span data-ttu-id="59bb8-229">ladCfg</span><span class="sxs-lookup"><span data-stu-id="59bb8-229">ladCfg</span></span>

```json
"ladCfg": {
    "diagnosticMonitorConfiguration": {
        "eventVolume": "Medium",
        "metrics": { ... },
        "performanceCounters": { ... },
        "syslogEvents": { ... }
    },
    "sampleRateInSeconds": 15
}
```

<span data-ttu-id="59bb8-230">Den här valfria struktur kontroller hello insamlingen av mätvärden och loggfiler för leverans toohello Azure-tjänsten och tooother mätvärdesdata egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="59bb8-230">This optional structure controls hello gathering of metrics and logs for delivery toohello Azure Metrics service and tooother data sinks.</span></span> <span data-ttu-id="59bb8-231">Du måste ange antingen `performanceCounters` eller `syslogEvents` eller båda.</span><span class="sxs-lookup"><span data-stu-id="59bb8-231">You must specify either `performanceCounters` or `syslogEvents` or both.</span></span> <span data-ttu-id="59bb8-232">Du måste ange hello `metrics` struktur.</span><span class="sxs-lookup"><span data-stu-id="59bb8-232">You must specify hello `metrics` structure.</span></span>

<span data-ttu-id="59bb8-233">Element</span><span class="sxs-lookup"><span data-stu-id="59bb8-233">Element</span></span> | <span data-ttu-id="59bb8-234">Värde</span><span class="sxs-lookup"><span data-stu-id="59bb8-234">Value</span></span>
------- | -----
<span data-ttu-id="59bb8-235">eventVolume</span><span class="sxs-lookup"><span data-stu-id="59bb8-235">eventVolume</span></span> | <span data-ttu-id="59bb8-236">(valfritt) Kontroller hello antalet partitioner som skapas i hello lagring tabell.</span><span class="sxs-lookup"><span data-stu-id="59bb8-236">(optional) Controls hello number of partitions created within hello storage table.</span></span> <span data-ttu-id="59bb8-237">Måste vara något av `"Large"`, `"Medium"`, eller `"Small"`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-237">Must be one of `"Large"`, `"Medium"`, or `"Small"`.</span></span> <span data-ttu-id="59bb8-238">Om inte anges är standardvärdet för hello `"Medium"`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-238">If not specified, hello default value is `"Medium"`.</span></span>
<span data-ttu-id="59bb8-239">sampleRateInSeconds</span><span class="sxs-lookup"><span data-stu-id="59bb8-239">sampleRateInSeconds</span></span> | <span data-ttu-id="59bb8-240">(valfritt) hello standardintervallet mellan samling raw (unaggregated) mått.</span><span class="sxs-lookup"><span data-stu-id="59bb8-240">(optional) hello default interval between collection of raw (unaggregated) metrics.</span></span> <span data-ttu-id="59bb8-241">hello minsta stöds samplingsfrekvens är 15 sekunder.</span><span class="sxs-lookup"><span data-stu-id="59bb8-241">hello smallest supported sample rate is 15 seconds.</span></span> <span data-ttu-id="59bb8-242">Om inte anges är standardvärdet för hello `15`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-242">If not specified, hello default value is `15`.</span></span>

#### <a name="metrics"></a><span data-ttu-id="59bb8-243">metrics</span><span class="sxs-lookup"><span data-stu-id="59bb8-243">metrics</span></span>

```json
"metrics": {
    "resourceId": "/subscriptions/...",
    "metricAggregation" : [
        { "scheduledTransferPeriod" : "PT1H" },
        { "scheduledTransferPeriod" : "PT5M" }
    ]
}
```

<span data-ttu-id="59bb8-244">Element</span><span class="sxs-lookup"><span data-stu-id="59bb8-244">Element</span></span> | <span data-ttu-id="59bb8-245">Värde</span><span class="sxs-lookup"><span data-stu-id="59bb8-245">Value</span></span>
------- | -----
<span data-ttu-id="59bb8-246">resourceId</span><span class="sxs-lookup"><span data-stu-id="59bb8-246">resourceId</span></span> | <span data-ttu-id="59bb8-247">hello Azure Resource Manager resurs-ID för hello VM eller hello virtuella skala ange toowhich hello VM tillhör.</span><span class="sxs-lookup"><span data-stu-id="59bb8-247">hello Azure Resource Manager resource ID of hello VM or of hello virtual machine scale set toowhich hello VM belongs.</span></span> <span data-ttu-id="59bb8-248">Den här inställningen måste vara anges också om alla JsonBlob sink används i hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="59bb8-248">This setting must be also specified if any JsonBlob sink is used in hello configuration.</span></span>
<span data-ttu-id="59bb8-249">scheduledTransferPeriod</span><span class="sxs-lookup"><span data-stu-id="59bb8-249">scheduledTransferPeriod</span></span> | <span data-ttu-id="59bb8-250">hello frekvens vid vilken sammanställd är toobe beräknad och överförs tooAzure statistik, uttryckt som en är 8601 tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="59bb8-250">hello frequency at which aggregate metrics are toobe computed and transferred tooAzure Metrics, expressed as an IS 8601 time interval.</span></span> <span data-ttu-id="59bb8-251">hello minsta överföringsperioden är 60 sekunder, det vill säga PT1M.</span><span class="sxs-lookup"><span data-stu-id="59bb8-251">hello smallest transfer period is 60 seconds, that is, PT1M.</span></span> <span data-ttu-id="59bb8-252">Du måste ange minst en scheduledTransferPeriod.</span><span class="sxs-lookup"><span data-stu-id="59bb8-252">You must specify at least one scheduledTransferPeriod.</span></span>

<span data-ttu-id="59bb8-253">Exempel för hello mått som anges i avsnittet för hello prestandaräknarna samlas var 15: e sekund eller i hello samplingsfrekvens definieras explicit för hello räknare.</span><span class="sxs-lookup"><span data-stu-id="59bb8-253">Samples of hello metrics specified in hello performanceCounters section are collected every 15 seconds or at hello sample rate explicitly defined for hello counter.</span></span> <span data-ttu-id="59bb8-254">Om flera scheduledTransferPeriod frekvenser visas (som hello exempel), beräknas varje aggregerings oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="59bb8-254">If multiple scheduledTransferPeriod frequencies appear (as in hello example), each aggregation is computed independently.</span></span>

#### <a name="performancecounters"></a><span data-ttu-id="59bb8-255">performanceCounters</span><span class="sxs-lookup"><span data-stu-id="59bb8-255">performanceCounters</span></span>

```json
"performanceCounters": {
    "sinks": "",
    "performanceCounterConfiguration": [
        {
            "type": "builtin",
            "class": "Processor",
            "counter": "PercentIdleTime",
            "counterSpecifier": "/builtin/Processor/PercentIdleTime",
            "condition": "IsAggregate=TRUE",
            "sampleRate": "PT15S",
            "unit": "Percent",
            "annotation": [
                {
                    "displayName" : "Aggregate CPU %idle time",
                    "locale" : "en-us"
                }
            ]
        }
    ]
}
```

<span data-ttu-id="59bb8-256">Det här valfria avsnittet styr hello samling mått.</span><span class="sxs-lookup"><span data-stu-id="59bb8-256">This optional section controls hello collection of metrics.</span></span> <span data-ttu-id="59bb8-257">Rådata prover aggregeras för varje [scheduledTransferPeriod](#metrics) tooproduce dessa värden:</span><span class="sxs-lookup"><span data-stu-id="59bb8-257">Raw samples are aggregated for each [scheduledTransferPeriod](#metrics) tooproduce these values:</span></span>

* <span data-ttu-id="59bb8-258">medelvärde</span><span class="sxs-lookup"><span data-stu-id="59bb8-258">mean</span></span>
* <span data-ttu-id="59bb8-259">minsta</span><span class="sxs-lookup"><span data-stu-id="59bb8-259">minimum</span></span>
* <span data-ttu-id="59bb8-260">Maximalt</span><span class="sxs-lookup"><span data-stu-id="59bb8-260">maximum</span></span>
* <span data-ttu-id="59bb8-261">senaste samlas in värdet</span><span class="sxs-lookup"><span data-stu-id="59bb8-261">last-collected value</span></span>
* <span data-ttu-id="59bb8-262">antal prover raw används toocompute hello aggregering</span><span class="sxs-lookup"><span data-stu-id="59bb8-262">count of raw samples used toocompute hello aggregate</span></span>

<span data-ttu-id="59bb8-263">Element</span><span class="sxs-lookup"><span data-stu-id="59bb8-263">Element</span></span> | <span data-ttu-id="59bb8-264">Värde</span><span class="sxs-lookup"><span data-stu-id="59bb8-264">Value</span></span>
------- | -----
<span data-ttu-id="59bb8-265">egenskaperna</span><span class="sxs-lookup"><span data-stu-id="59bb8-265">sinks</span></span> | <span data-ttu-id="59bb8-266">(valfritt) En kommaavgränsad lista med namnen på egenskaperna toowhich LAD skickar sammanlagda mått resultat.</span><span class="sxs-lookup"><span data-stu-id="59bb8-266">(optional) A comma-separated list of names of sinks toowhich LAD sends aggregated metric results.</span></span> <span data-ttu-id="59bb8-267">Alla är sammanställda publicerade tooeach visas mottagare.</span><span class="sxs-lookup"><span data-stu-id="59bb8-267">All aggregated metrics are published tooeach listed sink.</span></span> <span data-ttu-id="59bb8-268">Se [sinksConfig](#sinksconfig).</span><span class="sxs-lookup"><span data-stu-id="59bb8-268">See [sinksConfig](#sinksconfig).</span></span> <span data-ttu-id="59bb8-269">Exempel: `"EHsink1, myjsonsink"`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-269">Example: `"EHsink1, myjsonsink"`.</span></span>
<span data-ttu-id="59bb8-270">typ</span><span class="sxs-lookup"><span data-stu-id="59bb8-270">type</span></span> | <span data-ttu-id="59bb8-271">Identifierar hello faktiska providern av hello mått.</span><span class="sxs-lookup"><span data-stu-id="59bb8-271">Identifies hello actual provider of hello metric.</span></span>
<span data-ttu-id="59bb8-272">Klass</span><span class="sxs-lookup"><span data-stu-id="59bb8-272">class</span></span> | <span data-ttu-id="59bb8-273">Identifierar hello specifikt mått i hello leverantörens namnrymd tillsammans med ”räknaren”.</span><span class="sxs-lookup"><span data-stu-id="59bb8-273">Together with "counter", identifies hello specific metric within hello provider's namespace.</span></span>
<span data-ttu-id="59bb8-274">Räknaren</span><span class="sxs-lookup"><span data-stu-id="59bb8-274">counter</span></span> | <span data-ttu-id="59bb8-275">Identifierar hello specifikt mått i hello leverantörens namnrymd tillsammans med ”class”.</span><span class="sxs-lookup"><span data-stu-id="59bb8-275">Together with "class", identifies hello specific metric within hello provider's namespace.</span></span>
<span data-ttu-id="59bb8-276">counterSpecifier</span><span class="sxs-lookup"><span data-stu-id="59bb8-276">counterSpecifier</span></span> | <span data-ttu-id="59bb8-277">Identifierar hello specifikt mått i hello Azure mått namnområde.</span><span class="sxs-lookup"><span data-stu-id="59bb8-277">Identifies hello specific metric within hello Azure Metrics namespace.</span></span>
<span data-ttu-id="59bb8-278">Villkor</span><span class="sxs-lookup"><span data-stu-id="59bb8-278">condition</span></span> | <span data-ttu-id="59bb8-279">(valfritt) Väljer en specifik instans av hello objektet toowhich hello mått gäller eller väljer hello sammanställning över alla instanser av objektet.</span><span class="sxs-lookup"><span data-stu-id="59bb8-279">(optional) Selects a specific instance of hello object toowhich hello metric applies or selects hello aggregation across all instances of that object.</span></span> <span data-ttu-id="59bb8-280">Mer information finns i hello [ `builtin` måttdefinitioner](#metrics-supported-by-builtin).</span><span class="sxs-lookup"><span data-stu-id="59bb8-280">For more information, see hello [`builtin` metric definitions](#metrics-supported-by-builtin).</span></span>
<span data-ttu-id="59bb8-281">sampleRate</span><span class="sxs-lookup"><span data-stu-id="59bb8-281">sampleRate</span></span> | <span data-ttu-id="59bb8-282">ÄR 8601 intervallet som anger Hej då rådata prover för det här måttet har samlats in.</span><span class="sxs-lookup"><span data-stu-id="59bb8-282">IS 8601 interval that sets hello rate at which raw samples for this metric are collected.</span></span> <span data-ttu-id="59bb8-283">Om inte ange intervall för insamling av hello har angetts av hello värdet för [sampleRateInSeconds](#ladcfg).</span><span class="sxs-lookup"><span data-stu-id="59bb8-283">If not set, hello collection interval is set by hello value of [sampleRateInSeconds](#ladcfg).</span></span> <span data-ttu-id="59bb8-284">hello kortaste stöds samplingsfrekvens är 15 sekunder (PT15S).</span><span class="sxs-lookup"><span data-stu-id="59bb8-284">hello shortest supported sample rate is 15 seconds (PT15S).</span></span>
<span data-ttu-id="59bb8-285">enhet</span><span class="sxs-lookup"><span data-stu-id="59bb8-285">unit</span></span> | <span data-ttu-id="59bb8-286">Måste vara ett av de här strängarna: ”antal”, ”byte”, ”sekunder”, ”procent”, ”CountPerSecond”, ”BytesPerSecond”, ”millisekunder”.</span><span class="sxs-lookup"><span data-stu-id="59bb8-286">Should be one of these strings: "Count", "Bytes", "Seconds", "Percent", "CountPerSecond", "BytesPerSecond", "Millisecond".</span></span> <span data-ttu-id="59bb8-287">Definierar hello enhet för hello mått.</span><span class="sxs-lookup"><span data-stu-id="59bb8-287">Defines hello unit for hello metric.</span></span> <span data-ttu-id="59bb8-288">Konsumenter av hello insamlade data förväntas hello insamlade data värden toomatch denna enhet.</span><span class="sxs-lookup"><span data-stu-id="59bb8-288">Consumers of hello collected data expect hello collected data values toomatch this unit.</span></span> <span data-ttu-id="59bb8-289">LAD ignorerar det här fältet.</span><span class="sxs-lookup"><span data-stu-id="59bb8-289">LAD ignores this field.</span></span>
<span data-ttu-id="59bb8-290">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="59bb8-290">displayName</span></span> | <span data-ttu-id="59bb8-291">hello etikett (på hello språk som anges av hello associerade nationella inställningarna) toobe kopplade toothis data i Azure mått.</span><span class="sxs-lookup"><span data-stu-id="59bb8-291">hello label (in hello language specified by hello associated locale setting) toobe attached toothis data in Azure Metrics.</span></span> <span data-ttu-id="59bb8-292">LAD ignorerar det här fältet.</span><span class="sxs-lookup"><span data-stu-id="59bb8-292">LAD ignores this field.</span></span>

<span data-ttu-id="59bb8-293">Hej counterSpecifier är ett valfritt ID.</span><span class="sxs-lookup"><span data-stu-id="59bb8-293">hello counterSpecifier is an arbitrary identifier.</span></span> <span data-ttu-id="59bb8-294">Konsumenter av statistik, som hello Azure portal diagram och avisering funktion, Använd counterSpecifier som hello ”nyckel” som identifierar ett mått eller en instans av ett mått.</span><span class="sxs-lookup"><span data-stu-id="59bb8-294">Consumers of metrics, like hello Azure portal charting and alerting feature, use counterSpecifier as hello "key" that identifies a metric or an instance of a metric.</span></span> <span data-ttu-id="59bb8-295">För `builtin` statistik, rekommenderar vi du använder counterSpecifier värden som börjar med `/builtin/`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-295">For `builtin` metrics, we recommend you use counterSpecifier values that begin with `/builtin/`.</span></span> <span data-ttu-id="59bb8-296">Om du samlar in en specifik instans av ett mått, rekommenderar vi du bifoga hello identifierare för hello instans toohello counterSpecifier värde.</span><span class="sxs-lookup"><span data-stu-id="59bb8-296">If you are collecting a specific instance of a metric, we recommend you attach hello identifier of hello instance toohello counterSpecifier value.</span></span> <span data-ttu-id="59bb8-297">Några exempel:</span><span class="sxs-lookup"><span data-stu-id="59bb8-297">Some examples:</span></span>

* <span data-ttu-id="59bb8-298">`/builtin/Processor/PercentIdleTime`-Inaktivitetstid var i genomsnitt över alla kärnor</span><span class="sxs-lookup"><span data-stu-id="59bb8-298">`/builtin/Processor/PercentIdleTime` - Idle time averaged across all cores</span></span>
* <span data-ttu-id="59bb8-299">`/builtin/Disk/FreeSpace(/mnt)`-Ledigt utrymme för hello /mnt filsystem</span><span class="sxs-lookup"><span data-stu-id="59bb8-299">`/builtin/Disk/FreeSpace(/mnt)` - Free space for hello /mnt filesystem</span></span>
* <span data-ttu-id="59bb8-300">`/builtin/Disk/FreeSpace`-Ledigt utrymme som var i genomsnitt över alla monterade filsystem</span><span class="sxs-lookup"><span data-stu-id="59bb8-300">`/builtin/Disk/FreeSpace` - Free space averaged across all mounted filesystems</span></span>

<span data-ttu-id="59bb8-301">Varken LAD eller hello Azure-portalen förväntar hello counterSpecifier värdet toomatch alla mönster.</span><span class="sxs-lookup"><span data-stu-id="59bb8-301">Neither LAD nor hello Azure portal expects hello counterSpecifier value toomatch any pattern.</span></span> <span data-ttu-id="59bb8-302">Vara konsekvent i hur du skapar counterSpecifier värden.</span><span class="sxs-lookup"><span data-stu-id="59bb8-302">Be consistent in how you construct counterSpecifier values.</span></span>

<span data-ttu-id="59bb8-303">När du anger `performanceCounters`, LAD skrivs alltid tooa tabell i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="59bb8-303">When you specify `performanceCounters`, LAD always writes data tooa table in Azure storage.</span></span> <span data-ttu-id="59bb8-304">Du kan ha hello samma data som skrivs tooJSON blobbarna och/eller Händelsehubbar, men du inaktivera inte lagra data tooa tabell.</span><span class="sxs-lookup"><span data-stu-id="59bb8-304">You can have hello same data written tooJSON blobs and/or Event Hubs, but you cannot disable storing data tooa table.</span></span> <span data-ttu-id="59bb8-305">Alla instanser av hello diagnostiska tillägg konfigurerat toouse hello samma lagringskonto namn och en slutpunkt kan du lägga till sina mått och loggar toohello samma tabell.</span><span class="sxs-lookup"><span data-stu-id="59bb8-305">All instances of hello diagnostic extension configured toouse hello same storage account name and endpoint add their metrics and logs toohello same table.</span></span> <span data-ttu-id="59bb8-306">Om för många virtuella datorer skriver skriver toohello samma tabell, partition i Azure kan begränsa toothat partition.</span><span class="sxs-lookup"><span data-stu-id="59bb8-306">If too many VMs are writing toohello same table partition, Azure can throttle writes toothat partition.</span></span> <span data-ttu-id="59bb8-307">Hej eventVolume inställningen orsaker poster toobe fördelade på 1 (liten), 10 (medel) eller 100 (stor) olika partitioner.</span><span class="sxs-lookup"><span data-stu-id="59bb8-307">hello eventVolume setting causes entries toobe spread across 1 (Small), 10 (Medium), or 100 (Large) different partitions.</span></span> <span data-ttu-id="59bb8-308">Vanligtvis är ”medel” räcker tooensure trafik begränsas inte.</span><span class="sxs-lookup"><span data-stu-id="59bb8-308">Usually, "Medium" is sufficient tooensure traffic is not throttled.</span></span> <span data-ttu-id="59bb8-309">hello använder Azure mått av hello Azure-portalen hello data i den här tabellen tooproduce diagram eller tootrigger aviseringar.</span><span class="sxs-lookup"><span data-stu-id="59bb8-309">hello Azure Metrics feature of hello Azure portal uses hello data in this table tooproduce graphs or tootrigger alerts.</span></span> <span data-ttu-id="59bb8-310">hello tabellnamnet är hello sammanfogning av dessa strängar:</span><span class="sxs-lookup"><span data-stu-id="59bb8-310">hello table name is hello concatenation of these strings:</span></span>

* `WADMetrics`
* <span data-ttu-id="59bb8-311">Hej ”scheduledTransferPeriod” för hello samman värden som lagras i hello tabell</span><span class="sxs-lookup"><span data-stu-id="59bb8-311">hello "scheduledTransferPeriod" for hello aggregated values stored in hello table</span></span>
* `P10DV2S`
* <span data-ttu-id="59bb8-312">Ett datum i form av Hej ”ÅÅÅÅMMDD” som ändrar var 10: e dag</span><span class="sxs-lookup"><span data-stu-id="59bb8-312">A date, in hello form "YYYYMMDD", which changes every 10 days</span></span>

<span data-ttu-id="59bb8-313">Exempel inkluderar `WADMetricsPT1HP10DV2S20170410` och `WADMetricsPT1MP10DV2S20170609`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-313">Examples include `WADMetricsPT1HP10DV2S20170410` and `WADMetricsPT1MP10DV2S20170609`.</span></span>

#### <a name="syslogevents"></a><span data-ttu-id="59bb8-314">syslogEvents</span><span class="sxs-lookup"><span data-stu-id="59bb8-314">syslogEvents</span></span>

```json
"syslogEvents": {
    "sinks": "",
    "syslogEventConfiguration": {
        "facilityName1": "minSeverity",
        "facilityName2": "minSeverity",
        ...
    }
}
```

<span data-ttu-id="59bb8-315">Det här valfria avsnittet styr hello insamling av händelser från syslog.</span><span class="sxs-lookup"><span data-stu-id="59bb8-315">This optional section controls hello collection of log events from syslog.</span></span> <span data-ttu-id="59bb8-316">Syslog-händelser fångas inte alls om hello avsnittet utelämnas.</span><span class="sxs-lookup"><span data-stu-id="59bb8-316">If hello section is omitted, syslog events are not captured at all.</span></span>

<span data-ttu-id="59bb8-317">Hej syslogEventConfiguration samlingen innehåller en post för varje syslog-funktion av intresse.</span><span class="sxs-lookup"><span data-stu-id="59bb8-317">hello syslogEventConfiguration collection has one entry for each syslog facility of interest.</span></span> <span data-ttu-id="59bb8-318">Om minSeverity är ”NONE” för en viss funktion, eller om den och inte visas i hello element på alla, fångas inga händelser från den anläggningen.</span><span class="sxs-lookup"><span data-stu-id="59bb8-318">If minSeverity is "NONE" for a particular facility, or if that facility does not appear in hello element at all, no events from that facility are captured.</span></span>

<span data-ttu-id="59bb8-319">Element</span><span class="sxs-lookup"><span data-stu-id="59bb8-319">Element</span></span> | <span data-ttu-id="59bb8-320">Värde</span><span class="sxs-lookup"><span data-stu-id="59bb8-320">Value</span></span>
------- | -----
<span data-ttu-id="59bb8-321">egenskaperna</span><span class="sxs-lookup"><span data-stu-id="59bb8-321">sinks</span></span> | <span data-ttu-id="59bb8-322">En kommaavgränsad lista med namnen på egenskaperna toowhich enskild logg händelserna publiceras.</span><span class="sxs-lookup"><span data-stu-id="59bb8-322">A comma-separated list of names of sinks toowhich individual log events are published.</span></span> <span data-ttu-id="59bb8-323">Alla händelser matchar hello begränsningar i syslogEventConfiguration är publicerade tooeach visas mottagare.</span><span class="sxs-lookup"><span data-stu-id="59bb8-323">All log events matching hello restrictions in syslogEventConfiguration are published tooeach listed sink.</span></span> <span data-ttu-id="59bb8-324">Exempel: ”EHforsyslog”</span><span class="sxs-lookup"><span data-stu-id="59bb8-324">Example: "EHforsyslog"</span></span>
<span data-ttu-id="59bb8-325">Funktionen</span><span class="sxs-lookup"><span data-stu-id="59bb8-325">facilityName</span></span> | <span data-ttu-id="59bb8-326">En lokal syslog-namn (som ”LOGGA\_användare” eller ”LOGGA\_LOCAL0”).</span><span class="sxs-lookup"><span data-stu-id="59bb8-326">A syslog facility name (such as "LOG\_USER" or "LOG\_LOCAL0").</span></span> <span data-ttu-id="59bb8-327">Avsnittet Hej ”och” i hello [syslog man sidan](http://man7.org/linux/man-pages/man3/syslog.3.html) hello fullständig lista.</span><span class="sxs-lookup"><span data-stu-id="59bb8-327">See hello "facility" section of hello [syslog man page](http://man7.org/linux/man-pages/man3/syslog.3.html) for hello full list.</span></span>
<span data-ttu-id="59bb8-328">minSeverity</span><span class="sxs-lookup"><span data-stu-id="59bb8-328">minSeverity</span></span> | <span data-ttu-id="59bb8-329">En syslog-allvarlighetsgrad (som ”LOGGA\_fel” eller ”LOGGA\_INFO”).</span><span class="sxs-lookup"><span data-stu-id="59bb8-329">A syslog severity level (such as "LOG\_ERR" or "LOG\_INFO").</span></span> <span data-ttu-id="59bb8-330">Avsnittet hello ”nivå” i hello [syslog man sidan](http://man7.org/linux/man-pages/man3/syslog.3.html) hello fullständig lista.</span><span class="sxs-lookup"><span data-stu-id="59bb8-330">See hello "level" section of hello [syslog man page](http://man7.org/linux/man-pages/man3/syslog.3.html) for hello full list.</span></span> <span data-ttu-id="59bb8-331">hello tillägget samlar in händelser skickas toohello anläggning i eller ovanför hello angetts nivå.</span><span class="sxs-lookup"><span data-stu-id="59bb8-331">hello extension captures events sent toohello facility at or above hello specified level.</span></span>

<span data-ttu-id="59bb8-332">När du anger `syslogEvents`, LAD skrivs alltid tooa tabell i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="59bb8-332">When you specify `syslogEvents`, LAD always writes data tooa table in Azure storage.</span></span> <span data-ttu-id="59bb8-333">Du kan ha hello samma data som skrivs tooJSON blobbarna och/eller Händelsehubbar, men du inaktivera inte lagra data tooa tabell.</span><span class="sxs-lookup"><span data-stu-id="59bb8-333">You can have hello same data written tooJSON blobs and/or Event Hubs, but you cannot disable storing data tooa table.</span></span> <span data-ttu-id="59bb8-334">hello partitionering beteendet för den här tabellen är hello detsamma som beskrevs för `performanceCounters`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-334">hello partitioning behavior for this table is hello same as described for `performanceCounters`.</span></span> <span data-ttu-id="59bb8-335">hello tabellnamnet är hello sammanfogning av dessa strängar:</span><span class="sxs-lookup"><span data-stu-id="59bb8-335">hello table name is hello concatenation of these strings:</span></span>

* `LinuxSyslog`
* <span data-ttu-id="59bb8-336">Ett datum i form av Hej ”ÅÅÅÅMMDD” som ändrar var 10: e dag</span><span class="sxs-lookup"><span data-stu-id="59bb8-336">A date, in hello form "YYYYMMDD", which changes every 10 days</span></span>

<span data-ttu-id="59bb8-337">Exempel inkluderar `LinuxSyslog20170410` och `LinuxSyslog20170609`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-337">Examples include `LinuxSyslog20170410` and `LinuxSyslog20170609`.</span></span>

### <a name="perfcfg"></a><span data-ttu-id="59bb8-338">perfCfg</span><span class="sxs-lookup"><span data-stu-id="59bb8-338">perfCfg</span></span>

<span data-ttu-id="59bb8-339">Det här valfria avsnittet styr körning av godtycklig [OMI](https://github.com/Microsoft/omi) frågor.</span><span class="sxs-lookup"><span data-stu-id="59bb8-339">This optional section controls execution of arbitrary [OMI](https://github.com/Microsoft/omi) queries.</span></span>

```json
"perfCfg": [
    {
        "namespace": "root/scx",
        "query": "SELECT PercentAvailableMemory, PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
        "table": "LinuxOldMemory",
        "frequency": 300,
        "sinks": ""
    }
]
```

<span data-ttu-id="59bb8-340">Element</span><span class="sxs-lookup"><span data-stu-id="59bb8-340">Element</span></span> | <span data-ttu-id="59bb8-341">Värde</span><span class="sxs-lookup"><span data-stu-id="59bb8-341">Value</span></span>
------- | -----
<span data-ttu-id="59bb8-342">namnområde</span><span class="sxs-lookup"><span data-stu-id="59bb8-342">namespace</span></span> | <span data-ttu-id="59bb8-343">(valfritt) hello OMI namnområde inom vilken hello frågan ska köras.</span><span class="sxs-lookup"><span data-stu-id="59bb8-343">(optional) hello OMI namespace within which hello query should be executed.</span></span> <span data-ttu-id="59bb8-344">Om inget anges hello standardvärdet är ”root/scx”, implementeras av hello [System Center plattformsoberoende Providers](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).</span><span class="sxs-lookup"><span data-stu-id="59bb8-344">If unspecified, hello default value is "root/scx", implemented by hello [System Center Cross-platform Providers](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).</span></span>
<span data-ttu-id="59bb8-345">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="59bb8-345">query</span></span> | <span data-ttu-id="59bb8-346">hello OMI frågan toobe utförs.</span><span class="sxs-lookup"><span data-stu-id="59bb8-346">hello OMI query toobe executed.</span></span>
<span data-ttu-id="59bb8-347">Tabell</span><span class="sxs-lookup"><span data-stu-id="59bb8-347">table</span></span> | <span data-ttu-id="59bb8-348">(valfritt) hello Azure storage tabell i hello avses storage-konto (se [skyddade inställningarna](#protected-settings)).</span><span class="sxs-lookup"><span data-stu-id="59bb8-348">(optional) hello Azure storage table, in hello designated storage account (see [Protected settings](#protected-settings)).</span></span>
<span data-ttu-id="59bb8-349">frequency</span><span class="sxs-lookup"><span data-stu-id="59bb8-349">frequency</span></span> | <span data-ttu-id="59bb8-350">(valfritt) hello antalet sekunder mellan körning av hello frågan.</span><span class="sxs-lookup"><span data-stu-id="59bb8-350">(optional) hello number of seconds between execution of hello query.</span></span> <span data-ttu-id="59bb8-351">Standardvärdet är 300 (5 minuter). lägsta värdet är 15 sekunder.</span><span class="sxs-lookup"><span data-stu-id="59bb8-351">Default value is 300 (5 minutes); minimum value is 15 seconds.</span></span>
<span data-ttu-id="59bb8-352">egenskaperna</span><span class="sxs-lookup"><span data-stu-id="59bb8-352">sinks</span></span> | <span data-ttu-id="59bb8-353">(valfritt) En kommaavgränsad lista över ytterligare sänkor toowhich raw mått resultat ska publiceras.</span><span class="sxs-lookup"><span data-stu-id="59bb8-353">(optional) A comma-separated list of names of additional sinks toowhich raw sample metric results should be published.</span></span> <span data-ttu-id="59bb8-354">Ingen aggregering av exemplen rådata beräknas genom hello tillägg eller Azure mått.</span><span class="sxs-lookup"><span data-stu-id="59bb8-354">No aggregation of these raw samples is computed by hello extension or by Azure Metrics.</span></span>

<span data-ttu-id="59bb8-355">Antingen ”table” eller ”egenskaperna”, eller både och måste anges.</span><span class="sxs-lookup"><span data-stu-id="59bb8-355">Either "table" or "sinks", or both, must be specified.</span></span>

### <a name="filelogs"></a><span data-ttu-id="59bb8-356">fileLogs</span><span class="sxs-lookup"><span data-stu-id="59bb8-356">fileLogs</span></span>

<span data-ttu-id="59bb8-357">Kontroller hello avbilda loggfiler.</span><span class="sxs-lookup"><span data-stu-id="59bb8-357">Controls hello capture of log files.</span></span> <span data-ttu-id="59bb8-358">LAD in nya textrader som de är skrivna toohello filen och skriver dem tootable rader och/eller alla angivna sänkor (JsonBlob eller EventHub).</span><span class="sxs-lookup"><span data-stu-id="59bb8-358">LAD captures new text lines as they are written toohello file and writes them tootable rows and/or any specified sinks (JsonBlob or EventHub).</span></span>

```json
"fileLogs": [
    {
        "file": "/var/log/mydaemonlog",
        "table": "MyDaemonEvents",
        "sinks": ""
    }
]
```

<span data-ttu-id="59bb8-359">Element</span><span class="sxs-lookup"><span data-stu-id="59bb8-359">Element</span></span> | <span data-ttu-id="59bb8-360">Värde</span><span class="sxs-lookup"><span data-stu-id="59bb8-360">Value</span></span>
------- | -----
<span data-ttu-id="59bb8-361">Filen</span><span class="sxs-lookup"><span data-stu-id="59bb8-361">file</span></span> | <span data-ttu-id="59bb8-362">hello fullständig sökväg till hello log file toobe bevakade och avbildas.</span><span class="sxs-lookup"><span data-stu-id="59bb8-362">hello full pathname of hello log file toobe watched and captured.</span></span> <span data-ttu-id="59bb8-363">hello pathname måste namnet på en enstaka fil. Det går inte att namnge en katalog eller innehåller jokertecken.</span><span class="sxs-lookup"><span data-stu-id="59bb8-363">hello pathname must name a single file; it cannot name a directory or contain wildcards.</span></span>
<span data-ttu-id="59bb8-364">Tabell</span><span class="sxs-lookup"><span data-stu-id="59bb8-364">table</span></span> | <span data-ttu-id="59bb8-365">(valfritt) hello Azure storage tabell i hello avses lagring konto (som anges i hello skyddade konfiguration) till vilka nya rader från hello ”stjärt” hello filen skrivs.</span><span class="sxs-lookup"><span data-stu-id="59bb8-365">(optional) hello Azure storage table, in hello designated storage account (as specified in hello protected configuration), into which new lines from hello "tail" of hello file are written.</span></span>
<span data-ttu-id="59bb8-366">egenskaperna</span><span class="sxs-lookup"><span data-stu-id="59bb8-366">sinks</span></span> | <span data-ttu-id="59bb8-367">(valfritt) En kommaavgränsad lista över ytterligare sänkor toowhich loggen rader skickas.</span><span class="sxs-lookup"><span data-stu-id="59bb8-367">(optional) A comma-separated list of names of additional sinks toowhich log lines sent.</span></span>

<span data-ttu-id="59bb8-368">Antingen ”table” eller ”egenskaperna”, eller både och måste anges.</span><span class="sxs-lookup"><span data-stu-id="59bb8-368">Either "table" or "sinks", or both, must be specified.</span></span>

## <a name="metrics-supported-by-hello-builtin-provider"></a><span data-ttu-id="59bb8-369">Mått som stöds av hello builtin-providern</span><span class="sxs-lookup"><span data-stu-id="59bb8-369">Metrics supported by hello builtin provider</span></span>

<span data-ttu-id="59bb8-370">hello builtin mått providern är en källa för mått mest intressanta tooa bred uppsättning användare.</span><span class="sxs-lookup"><span data-stu-id="59bb8-370">hello builtin metric provider is a source of metrics most interesting tooa broad set of users.</span></span> <span data-ttu-id="59bb8-371">De här måtten faller inom fem bred klasser:</span><span class="sxs-lookup"><span data-stu-id="59bb8-371">These metrics fall into five broad classes:</span></span>

* <span data-ttu-id="59bb8-372">Processor</span><span class="sxs-lookup"><span data-stu-id="59bb8-372">Processor</span></span>
* <span data-ttu-id="59bb8-373">Minne</span><span class="sxs-lookup"><span data-stu-id="59bb8-373">Memory</span></span>
* <span data-ttu-id="59bb8-374">Nätverk</span><span class="sxs-lookup"><span data-stu-id="59bb8-374">Network</span></span>
* <span data-ttu-id="59bb8-375">Filsystem</span><span class="sxs-lookup"><span data-stu-id="59bb8-375">Filesystem</span></span>
* <span data-ttu-id="59bb8-376">Disk</span><span class="sxs-lookup"><span data-stu-id="59bb8-376">Disk</span></span>

### <a name="builtin-metrics-for-hello-processor-class"></a><span data-ttu-id="59bb8-377">inbyggd mätvärden för hello Processor-klass</span><span class="sxs-lookup"><span data-stu-id="59bb8-377">builtin metrics for hello Processor class</span></span>

<span data-ttu-id="59bb8-378">hello Processor klass av mätvärden innehåller information om processoranvändning i hello VM.</span><span class="sxs-lookup"><span data-stu-id="59bb8-378">hello Processor class of metrics provides information about processor usage in hello VM.</span></span> <span data-ttu-id="59bb8-379">Vid sammanställning procenttal är hello resultatet hello genomsnitt mellan alla processorer.</span><span class="sxs-lookup"><span data-stu-id="59bb8-379">When aggregating percentages, hello result is hello average across all CPUs.</span></span> <span data-ttu-id="59bb8-380">I två grundläggande VM om en kärna var 100% upptagen och hello andra 100% inaktiv rapporteras hello PercentIdleTime skulle värdet vara 50.</span><span class="sxs-lookup"><span data-stu-id="59bb8-380">In a two core VM, if one core was 100% busy and hello other was 100% idle, hello reported PercentIdleTime would be 50.</span></span> <span data-ttu-id="59bb8-381">Om varje kärna 50% upptagen för hello samma period hello rapporterade resultat kan även vara 50.</span><span class="sxs-lookup"><span data-stu-id="59bb8-381">If each core was 50% busy for hello same period, hello reported result would also be 50.</span></span> <span data-ttu-id="59bb8-382">I en fyra kärnor virtuell dator med en kärna med 100% upptagen och hello andra inaktiv hello rapporterade PercentIdleTime är 75.</span><span class="sxs-lookup"><span data-stu-id="59bb8-382">In a four core VM, with one core 100% busy and hello others idle, hello reported PercentIdleTime would be 75.</span></span>

<span data-ttu-id="59bb8-383">Räknaren</span><span class="sxs-lookup"><span data-stu-id="59bb8-383">counter</span></span> | <span data-ttu-id="59bb8-384">Betydelse</span><span class="sxs-lookup"><span data-stu-id="59bb8-384">Meaning</span></span>
------- | -------
<span data-ttu-id="59bb8-385">PercentIdleTime</span><span class="sxs-lookup"><span data-stu-id="59bb8-385">PercentIdleTime</span></span> | <span data-ttu-id="59bb8-386">Procentandel av tiden under hello aggregering fönster som processorer köra hello kernel inaktiv loop</span><span class="sxs-lookup"><span data-stu-id="59bb8-386">Percentage of time during hello aggregation window that processors were executing hello kernel idle loop</span></span>
<span data-ttu-id="59bb8-387">PercentProcessorTime</span><span class="sxs-lookup"><span data-stu-id="59bb8-387">PercentProcessorTime</span></span> | <span data-ttu-id="59bb8-388">Procentandelen av tid att köra en icke-inaktiv tråd</span><span class="sxs-lookup"><span data-stu-id="59bb8-388">Percentage of time executing a non-idle thread</span></span>
<span data-ttu-id="59bb8-389">PercentIOWaitTime</span><span class="sxs-lookup"><span data-stu-id="59bb8-389">PercentIOWaitTime</span></span> | <span data-ttu-id="59bb8-390">Procentandelen tid som väntar på att toocomplete för i/o-åtgärder</span><span class="sxs-lookup"><span data-stu-id="59bb8-390">Percentage of time waiting for IO operations toocomplete</span></span>
<span data-ttu-id="59bb8-391">PercentInterruptTime</span><span class="sxs-lookup"><span data-stu-id="59bb8-391">PercentInterruptTime</span></span> | <span data-ttu-id="59bb8-392">Procentandelen tid som kör maskin-och programvara avbrott och DPC: er (uppskjutna proceduranrop)</span><span class="sxs-lookup"><span data-stu-id="59bb8-392">Percentage of time executing hardware/software interrupts and DPCs (deferred procedure calls)</span></span>
<span data-ttu-id="59bb8-393">PercentUserTime</span><span class="sxs-lookup"><span data-stu-id="59bb8-393">PercentUserTime</span></span> | <span data-ttu-id="59bb8-394">Icke-inaktiv tid under hello aggregering hello procentandelen tid som ägnats åt användaren fler med normal prioritet</span><span class="sxs-lookup"><span data-stu-id="59bb8-394">Of non-idle time during hello aggregation window, hello percentage of time spent in user more at normal priority</span></span>
<span data-ttu-id="59bb8-395">PercentNiceTime</span><span class="sxs-lookup"><span data-stu-id="59bb8-395">PercentNiceTime</span></span> | <span data-ttu-id="59bb8-396">Icke-inaktiv tid hello procent av nedsänkt (bra) prioritet</span><span class="sxs-lookup"><span data-stu-id="59bb8-396">Of non-idle time, hello percentage spent at lowered (nice) priority</span></span>
<span data-ttu-id="59bb8-397">PercentPrivilegedTime</span><span class="sxs-lookup"><span data-stu-id="59bb8-397">PercentPrivilegedTime</span></span> | <span data-ttu-id="59bb8-398">Icke-inaktiv tid hello procent av privilegierad (kernel)-läge</span><span class="sxs-lookup"><span data-stu-id="59bb8-398">Of non-idle time, hello percentage spent in privileged (kernel) mode</span></span>

<span data-ttu-id="59bb8-399">hello bör först fyra räknare sum too100%.</span><span class="sxs-lookup"><span data-stu-id="59bb8-399">hello first four counters should sum too100%.</span></span> <span data-ttu-id="59bb8-400">hello senast räknare tre också summan too100%. de dela upp hello summan av PercentProcessorTime, PercentIOWaitTime och PercentInterruptTime.</span><span class="sxs-lookup"><span data-stu-id="59bb8-400">hello last three counters also sum too100%; they subdivide hello sum of PercentProcessorTime, PercentIOWaitTime, and PercentInterruptTime.</span></span>

<span data-ttu-id="59bb8-401">Ange tooobtain ett mått samman mellan alla processorer `"condition": "IsAggregate=TRUE"`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-401">tooobtain a single metric aggregated across all processors, set `"condition": "IsAggregate=TRUE"`.</span></span> <span data-ttu-id="59bb8-402">tooobtain ett mått för en viss processor så som hello andra logisk processor för en fyra grundläggande VM, ange `"condition": "Name=\\"1\\""`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-402">tooobtain a metric for a specific processor, such as hello second logical processor of a four core VM, set `"condition": "Name=\\"1\\""`.</span></span> <span data-ttu-id="59bb8-403">Logisk processor nummer är inom räckhåll hello `[0..n-1]`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-403">Logical processor numbers are in hello range `[0..n-1]`.</span></span>

### <a name="builtin-metrics-for-hello-memory-class"></a><span data-ttu-id="59bb8-404">inbyggd mätvärden för hello minne klass</span><span class="sxs-lookup"><span data-stu-id="59bb8-404">builtin metrics for hello Memory class</span></span>

<span data-ttu-id="59bb8-405">hello minne klass av mätvärden innehåller information om växling och växla minnesanvändning.</span><span class="sxs-lookup"><span data-stu-id="59bb8-405">hello Memory class of metrics provides information about memory utilization, paging, and swapping.</span></span>

<span data-ttu-id="59bb8-406">Räknaren</span><span class="sxs-lookup"><span data-stu-id="59bb8-406">counter</span></span> | <span data-ttu-id="59bb8-407">Betydelse</span><span class="sxs-lookup"><span data-stu-id="59bb8-407">Meaning</span></span>
------- | -------
<span data-ttu-id="59bb8-408">AvailableMemory</span><span class="sxs-lookup"><span data-stu-id="59bb8-408">AvailableMemory</span></span> | <span data-ttu-id="59bb8-409">Tillgängligt fysiskt minne i MiB</span><span class="sxs-lookup"><span data-stu-id="59bb8-409">Available physical memory in MiB</span></span>
<span data-ttu-id="59bb8-410">PercentAvailableMemory</span><span class="sxs-lookup"><span data-stu-id="59bb8-410">PercentAvailableMemory</span></span> | <span data-ttu-id="59bb8-411">Tillgängligt fysiskt minne i procent av det totala minnet</span><span class="sxs-lookup"><span data-stu-id="59bb8-411">Available physical memory as a percent of total memory</span></span>
<span data-ttu-id="59bb8-412">UsedMemory</span><span class="sxs-lookup"><span data-stu-id="59bb8-412">UsedMemory</span></span> | <span data-ttu-id="59bb8-413">Använd fysiskt minne (MiB)</span><span class="sxs-lookup"><span data-stu-id="59bb8-413">In-use physical memory (MiB)</span></span>
<span data-ttu-id="59bb8-414">PercentUsedMemory</span><span class="sxs-lookup"><span data-stu-id="59bb8-414">PercentUsedMemory</span></span> | <span data-ttu-id="59bb8-415">Används fysiskt minne i procent av det totala minnet</span><span class="sxs-lookup"><span data-stu-id="59bb8-415">In-use physical memory as a percent of total memory</span></span>
<span data-ttu-id="59bb8-416">PagesPerSec</span><span class="sxs-lookup"><span data-stu-id="59bb8-416">PagesPerSec</span></span> | <span data-ttu-id="59bb8-417">Total växling (läsa/skriva)</span><span class="sxs-lookup"><span data-stu-id="59bb8-417">Total paging (read/write)</span></span>
<span data-ttu-id="59bb8-418">PagesReadPerSec</span><span class="sxs-lookup"><span data-stu-id="59bb8-418">PagesReadPerSec</span></span> | <span data-ttu-id="59bb8-419">Sidorna läses säkerhetskopiering store (växlingsfil, fil, mappade filen, etc.)</span><span class="sxs-lookup"><span data-stu-id="59bb8-419">Pages read from backing store (swap file, program file, mapped file, etc.)</span></span>
<span data-ttu-id="59bb8-420">PagesWrittenPerSec</span><span class="sxs-lookup"><span data-stu-id="59bb8-420">PagesWrittenPerSec</span></span> | <span data-ttu-id="59bb8-421">Sidor som skrivs toobacking lagra (växlingsfil, mappade filen osv.)</span><span class="sxs-lookup"><span data-stu-id="59bb8-421">Pages written toobacking store (swap file, mapped file, etc.)</span></span>
<span data-ttu-id="59bb8-422">AvailableSwap</span><span class="sxs-lookup"><span data-stu-id="59bb8-422">AvailableSwap</span></span> | <span data-ttu-id="59bb8-423">Oanvända växlingsutrymme (MiB)</span><span class="sxs-lookup"><span data-stu-id="59bb8-423">Unused swap space (MiB)</span></span>
<span data-ttu-id="59bb8-424">PercentAvailableSwap</span><span class="sxs-lookup"><span data-stu-id="59bb8-424">PercentAvailableSwap</span></span> | <span data-ttu-id="59bb8-425">Oanvända växlingsutrymme som en procentandel av totalt växlingsutrymme</span><span class="sxs-lookup"><span data-stu-id="59bb8-425">Unused swap space as a percentage of total swap</span></span>
<span data-ttu-id="59bb8-426">UsedSwap</span><span class="sxs-lookup"><span data-stu-id="59bb8-426">UsedSwap</span></span> | <span data-ttu-id="59bb8-427">Använd växlingsutrymme (MiB)</span><span class="sxs-lookup"><span data-stu-id="59bb8-427">In-use swap space (MiB)</span></span>
<span data-ttu-id="59bb8-428">PercentUsedSwap</span><span class="sxs-lookup"><span data-stu-id="59bb8-428">PercentUsedSwap</span></span> | <span data-ttu-id="59bb8-429">Används växlingsutrymme som en procentandel av totalt växlingsutrymme</span><span class="sxs-lookup"><span data-stu-id="59bb8-429">In-use swap space as a percentage of total swap</span></span>

<span data-ttu-id="59bb8-430">Mått i den här klassen har en enda instans.</span><span class="sxs-lookup"><span data-stu-id="59bb8-430">This class of metrics has only a single instance.</span></span> <span data-ttu-id="59bb8-431">Hej ”villkor” attribut har inga användbara inställningar och ska uteslutas.</span><span class="sxs-lookup"><span data-stu-id="59bb8-431">hello "condition" attribute has no useful settings and should be omitted.</span></span>

### <a name="builtin-metrics-for-hello-network-class"></a><span data-ttu-id="59bb8-432">inbyggd mätvärden för hello nätverket klass</span><span class="sxs-lookup"><span data-stu-id="59bb8-432">builtin metrics for hello Network class</span></span>

<span data-ttu-id="59bb8-433">hello nätverket klass av mätvärden innehåller information om nätverksaktivitet på en enskild nätverksgränssnitt sedan start.</span><span class="sxs-lookup"><span data-stu-id="59bb8-433">hello Network class of metrics provides information about network activity on an individual network interfaces since boot.</span></span> <span data-ttu-id="59bb8-434">LAD visar inte gränssnittshändelsen bandbredd statistik, som kan hämtas från värden mått.</span><span class="sxs-lookup"><span data-stu-id="59bb8-434">LAD does not expose bandwidth metrics, which can be retrieved from host metrics.</span></span>

<span data-ttu-id="59bb8-435">Räknaren</span><span class="sxs-lookup"><span data-stu-id="59bb8-435">counter</span></span> | <span data-ttu-id="59bb8-436">Betydelse</span><span class="sxs-lookup"><span data-stu-id="59bb8-436">Meaning</span></span>
------- | -------
<span data-ttu-id="59bb8-437">BytesTransmitted</span><span class="sxs-lookup"><span data-stu-id="59bb8-437">BytesTransmitted</span></span> | <span data-ttu-id="59bb8-438">Totalt antal byte som skickats sedan start</span><span class="sxs-lookup"><span data-stu-id="59bb8-438">Total bytes sent since boot</span></span>
<span data-ttu-id="59bb8-439">BytesReceived</span><span class="sxs-lookup"><span data-stu-id="59bb8-439">BytesReceived</span></span> | <span data-ttu-id="59bb8-440">Totalt antal byte som tagits emot sedan start</span><span class="sxs-lookup"><span data-stu-id="59bb8-440">Total bytes received since boot</span></span>
<span data-ttu-id="59bb8-441">BytesTotal</span><span class="sxs-lookup"><span data-stu-id="59bb8-441">BytesTotal</span></span> | <span data-ttu-id="59bb8-442">Totalt antal byte som skickats eller tagits emot sedan start</span><span class="sxs-lookup"><span data-stu-id="59bb8-442">Total bytes sent or received since boot</span></span>
<span data-ttu-id="59bb8-443">PacketsTransmitted</span><span class="sxs-lookup"><span data-stu-id="59bb8-443">PacketsTransmitted</span></span> | <span data-ttu-id="59bb8-444">Totalt antal paket som skickats sedan start</span><span class="sxs-lookup"><span data-stu-id="59bb8-444">Total packets sent since boot</span></span>
<span data-ttu-id="59bb8-445">PacketsReceived</span><span class="sxs-lookup"><span data-stu-id="59bb8-445">PacketsReceived</span></span> | <span data-ttu-id="59bb8-446">Totalt antal paket som tagits emot sedan start</span><span class="sxs-lookup"><span data-stu-id="59bb8-446">Total packets received since boot</span></span>
<span data-ttu-id="59bb8-447">TotalRxErrors</span><span class="sxs-lookup"><span data-stu-id="59bb8-447">TotalRxErrors</span></span> | <span data-ttu-id="59bb8-448">Antal mottagna fel sedan start</span><span class="sxs-lookup"><span data-stu-id="59bb8-448">Number of receive errors since boot</span></span>
<span data-ttu-id="59bb8-449">TotalTxErrors</span><span class="sxs-lookup"><span data-stu-id="59bb8-449">TotalTxErrors</span></span> | <span data-ttu-id="59bb8-450">Antal överföringsfel sedan start</span><span class="sxs-lookup"><span data-stu-id="59bb8-450">Number of transmit errors since boot</span></span>
<span data-ttu-id="59bb8-451">TotalCollisions</span><span class="sxs-lookup"><span data-stu-id="59bb8-451">TotalCollisions</span></span> | <span data-ttu-id="59bb8-452">Antal kollisioner som rapporterats av hello nätverksportar sedan start</span><span class="sxs-lookup"><span data-stu-id="59bb8-452">Number of collisions reported by hello network ports since boot</span></span>

 <span data-ttu-id="59bb8-453">Även om den här klassen är instanced stöder inte LAD sparandet nätverket mått samman över alla nätverksenheter.</span><span class="sxs-lookup"><span data-stu-id="59bb8-453">Although this class is instanced, LAD does not support capturing Network metrics aggregated across all network devices.</span></span> <span data-ttu-id="59bb8-454">tooobtain hello mått för ett visst gränssnitt, till exempel eth0, ange `"condition": "InstanceID=\\"eth0\\""`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-454">tooobtain hello metrics for a specific interface, such as eth0, set `"condition": "InstanceID=\\"eth0\\""`.</span></span>

### <a name="builtin-metrics-for-hello-filesystem-class"></a><span data-ttu-id="59bb8-455">inbyggd mätvärden för hello filsystem-klass</span><span class="sxs-lookup"><span data-stu-id="59bb8-455">builtin metrics for hello Filesystem class</span></span>

<span data-ttu-id="59bb8-456">hello Filesystem klass av mätvärden innehåller information om användning av filsystem.</span><span class="sxs-lookup"><span data-stu-id="59bb8-456">hello Filesystem class of metrics provides information about filesystem usage.</span></span> <span data-ttu-id="59bb8-457">Absoluta och procentuella värden rapporteras som de skulle vara visas tooan vanlig användare (inte rot).</span><span class="sxs-lookup"><span data-stu-id="59bb8-457">Absolute and percentage values are reported as they'd be displayed tooan ordinary user (not root).</span></span>

<span data-ttu-id="59bb8-458">Räknaren</span><span class="sxs-lookup"><span data-stu-id="59bb8-458">counter</span></span> | <span data-ttu-id="59bb8-459">Betydelse</span><span class="sxs-lookup"><span data-stu-id="59bb8-459">Meaning</span></span>
------- | -------
<span data-ttu-id="59bb8-460">FreeSpace</span><span class="sxs-lookup"><span data-stu-id="59bb8-460">FreeSpace</span></span> | <span data-ttu-id="59bb8-461">Tillgängligt diskutrymme i byte</span><span class="sxs-lookup"><span data-stu-id="59bb8-461">Available disk space in bytes</span></span>
<span data-ttu-id="59bb8-462">UsedSpace</span><span class="sxs-lookup"><span data-stu-id="59bb8-462">UsedSpace</span></span> | <span data-ttu-id="59bb8-463">Använda diskutrymmet i byte</span><span class="sxs-lookup"><span data-stu-id="59bb8-463">Used disk space in bytes</span></span>
<span data-ttu-id="59bb8-464">PercentFreeSpace</span><span class="sxs-lookup"><span data-stu-id="59bb8-464">PercentFreeSpace</span></span> | <span data-ttu-id="59bb8-465">Procent ledigt utrymme</span><span class="sxs-lookup"><span data-stu-id="59bb8-465">Percentage free space</span></span>
<span data-ttu-id="59bb8-466">PercentUsedSpace</span><span class="sxs-lookup"><span data-stu-id="59bb8-466">PercentUsedSpace</span></span> | <span data-ttu-id="59bb8-467">Använt utrymme i procent</span><span class="sxs-lookup"><span data-stu-id="59bb8-467">Percentage used space</span></span>
<span data-ttu-id="59bb8-468">PercentFreeInodes</span><span class="sxs-lookup"><span data-stu-id="59bb8-468">PercentFreeInodes</span></span> | <span data-ttu-id="59bb8-469">Procentandelen oanvända noder</span><span class="sxs-lookup"><span data-stu-id="59bb8-469">Percentage of unused inodes</span></span>
<span data-ttu-id="59bb8-470">PercentUsedInodes</span><span class="sxs-lookup"><span data-stu-id="59bb8-470">PercentUsedInodes</span></span> | <span data-ttu-id="59bb8-471">Procentandelen allokerat (i används) noder summeras över alla filsystem</span><span class="sxs-lookup"><span data-stu-id="59bb8-471">Percentage of allocated (in use) inodes summed across all filesystems</span></span>
<span data-ttu-id="59bb8-472">BytesReadPerSecond</span><span class="sxs-lookup"><span data-stu-id="59bb8-472">BytesReadPerSecond</span></span> | <span data-ttu-id="59bb8-473">Läsa antalet byte per sekund</span><span class="sxs-lookup"><span data-stu-id="59bb8-473">Bytes read per second</span></span>
<span data-ttu-id="59bb8-474">BytesWrittenPerSecond</span><span class="sxs-lookup"><span data-stu-id="59bb8-474">BytesWrittenPerSecond</span></span> | <span data-ttu-id="59bb8-475">Byte som skrivs per sekund</span><span class="sxs-lookup"><span data-stu-id="59bb8-475">Bytes written per second</span></span>
<span data-ttu-id="59bb8-476">BytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="59bb8-476">BytesPerSecond</span></span> | <span data-ttu-id="59bb8-477">Byte läsas eller skrivas per sekund</span><span class="sxs-lookup"><span data-stu-id="59bb8-477">Bytes read or written per second</span></span>
<span data-ttu-id="59bb8-478">ReadsPerSecond</span><span class="sxs-lookup"><span data-stu-id="59bb8-478">ReadsPerSecond</span></span> | <span data-ttu-id="59bb8-479">Läsåtgärder per sekund</span><span class="sxs-lookup"><span data-stu-id="59bb8-479">Read operations per second</span></span>
<span data-ttu-id="59bb8-480">WritesPerSecond</span><span class="sxs-lookup"><span data-stu-id="59bb8-480">WritesPerSecond</span></span> | <span data-ttu-id="59bb8-481">Skrivåtgärder per sekund</span><span class="sxs-lookup"><span data-stu-id="59bb8-481">Write operations per second</span></span>
<span data-ttu-id="59bb8-482">TransfersPerSecond</span><span class="sxs-lookup"><span data-stu-id="59bb8-482">TransfersPerSecond</span></span> | <span data-ttu-id="59bb8-483">Läsa eller skriva åtgärder per sekund</span><span class="sxs-lookup"><span data-stu-id="59bb8-483">Read or write operations per second</span></span>

<span data-ttu-id="59bb8-484">Sammanställda värden över alla filsystem kan erhållas genom att ange `"condition": "IsAggregate=True"`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-484">Aggregated values across all file systems can be obtained by setting `"condition": "IsAggregate=True"`.</span></span> <span data-ttu-id="59bb8-485">Värden för en specifik monterade filsystem, som ”/ mnt”, kan erhållas genom att ange `"condition": 'Name="/mnt"'`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-485">Values for a specific mounted file system, such as "/mnt", can be obtained by setting `"condition": 'Name="/mnt"'`.</span></span>

### <a name="builtin-metrics-for-hello-disk-class"></a><span data-ttu-id="59bb8-486">inbyggd mätvärden för hello Disk-klass</span><span class="sxs-lookup"><span data-stu-id="59bb8-486">builtin metrics for hello Disk class</span></span>

<span data-ttu-id="59bb8-487">hello Disk klass av mätvärden innehåller information om diskanvändning för enheten.</span><span class="sxs-lookup"><span data-stu-id="59bb8-487">hello Disk class of metrics provides information about disk device usage.</span></span> <span data-ttu-id="59bb8-488">Statistiken gäller toohello hela enheten.</span><span class="sxs-lookup"><span data-stu-id="59bb8-488">These statistics apply toohello entire drive.</span></span> <span data-ttu-id="59bb8-489">Om det finns flera filsystem på en enhet, är hello räknare för enheten ett effektivt sätt, samman över alla.</span><span class="sxs-lookup"><span data-stu-id="59bb8-489">If there are multiple file systems on a device, hello counters for that device are, effectively, aggregated across all of them.</span></span>

<span data-ttu-id="59bb8-490">Räknaren</span><span class="sxs-lookup"><span data-stu-id="59bb8-490">counter</span></span> | <span data-ttu-id="59bb8-491">Betydelse</span><span class="sxs-lookup"><span data-stu-id="59bb8-491">Meaning</span></span>
------- | -------
<span data-ttu-id="59bb8-492">ReadsPerSecond</span><span class="sxs-lookup"><span data-stu-id="59bb8-492">ReadsPerSecond</span></span> | <span data-ttu-id="59bb8-493">Läsåtgärder per sekund</span><span class="sxs-lookup"><span data-stu-id="59bb8-493">Read operations per second</span></span>
<span data-ttu-id="59bb8-494">WritesPerSecond</span><span class="sxs-lookup"><span data-stu-id="59bb8-494">WritesPerSecond</span></span> | <span data-ttu-id="59bb8-495">Skrivåtgärder per sekund</span><span class="sxs-lookup"><span data-stu-id="59bb8-495">Write operations per second</span></span>
<span data-ttu-id="59bb8-496">TransfersPerSecond</span><span class="sxs-lookup"><span data-stu-id="59bb8-496">TransfersPerSecond</span></span> | <span data-ttu-id="59bb8-497">Totalt antal åtgärder per sekund</span><span class="sxs-lookup"><span data-stu-id="59bb8-497">Total operations per second</span></span>
<span data-ttu-id="59bb8-498">AverageReadTime</span><span class="sxs-lookup"><span data-stu-id="59bb8-498">AverageReadTime</span></span> | <span data-ttu-id="59bb8-499">Det genomsnittliga antalet sekunder per läsning</span><span class="sxs-lookup"><span data-stu-id="59bb8-499">Average seconds per read operation</span></span>
<span data-ttu-id="59bb8-500">AverageWriteTime</span><span class="sxs-lookup"><span data-stu-id="59bb8-500">AverageWriteTime</span></span> | <span data-ttu-id="59bb8-501">Det genomsnittliga antalet sekunder per skrivning</span><span class="sxs-lookup"><span data-stu-id="59bb8-501">Average seconds per write operation</span></span>
<span data-ttu-id="59bb8-502">AverageTransferTime</span><span class="sxs-lookup"><span data-stu-id="59bb8-502">AverageTransferTime</span></span> | <span data-ttu-id="59bb8-503">Det genomsnittliga antalet sekunder per åtgärd</span><span class="sxs-lookup"><span data-stu-id="59bb8-503">Average seconds per operation</span></span>
<span data-ttu-id="59bb8-504">AverageDiskQueueLength</span><span class="sxs-lookup"><span data-stu-id="59bb8-504">AverageDiskQueueLength</span></span> | <span data-ttu-id="59bb8-505">Genomsnittligt antal köade diskåtgärder</span><span class="sxs-lookup"><span data-stu-id="59bb8-505">Average number of queued disk operations</span></span>
<span data-ttu-id="59bb8-506">ReadBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="59bb8-506">ReadBytesPerSecond</span></span> | <span data-ttu-id="59bb8-507">Antalet lästa byte per sekund</span><span class="sxs-lookup"><span data-stu-id="59bb8-507">Number of bytes read per second</span></span>
<span data-ttu-id="59bb8-508">WriteBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="59bb8-508">WriteBytesPerSecond</span></span> | <span data-ttu-id="59bb8-509">Antalet byte som skrivs per sekund</span><span class="sxs-lookup"><span data-stu-id="59bb8-509">Number of bytes written per second</span></span>
<span data-ttu-id="59bb8-510">BytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="59bb8-510">BytesPerSecond</span></span> | <span data-ttu-id="59bb8-511">Antalet byte som har lästs eller skrivits per sekund</span><span class="sxs-lookup"><span data-stu-id="59bb8-511">Number of bytes read or written per second</span></span>

<span data-ttu-id="59bb8-512">Sammanställda värden över alla diskar kan erhållas genom att ange `"condition": "IsAggregate=True"`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-512">Aggregated values across all disks can be obtained by setting `"condition": "IsAggregate=True"`.</span></span> <span data-ttu-id="59bb8-513">Ange tooget information för en specifik enhet (till exempel/dev/sdf1), `"condition": "Name=\\"/dev/sdf1\\""`.</span><span class="sxs-lookup"><span data-stu-id="59bb8-513">tooget information for a specific device (for example, /dev/sdf1), set `"condition": "Name=\\"/dev/sdf1\\""`.</span></span>

## <a name="installing-and-configuring-lad-30-via-cli"></a><span data-ttu-id="59bb8-514">Installera och konfigurera LAD 3.0 via CLI</span><span class="sxs-lookup"><span data-stu-id="59bb8-514">Installing and configuring LAD 3.0 via CLI</span></span>

<span data-ttu-id="59bb8-515">Under förutsättning att de skydda inställningarna är i hello filen PrivateConfig.json och din offentliga konfigurationsinformation finns i PublicConfig.json, kör kommandot:</span><span class="sxs-lookup"><span data-stu-id="59bb8-515">Assuming your protected settings are in hello file PrivateConfig.json and your public configuration information is in PublicConfig.json, run this command:</span></span>

```azurecli
az vm extension set *resource_group_name* *vm_name* LinuxDiagnostic Microsoft.Azure.Diagnostics '3.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json
```

<span data-ttu-id="59bb8-516">hello kommandot förutsätter att du använder hello Azure Resource Manager-läge (arm) av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="59bb8-516">hello command assumes you are using hello Azure Resource Management mode (arm) of hello Azure CLI.</span></span> <span data-ttu-id="59bb8-517">tooconfigure LAD för klassisk distribution modellen (ASM) virtuella datorer, växla för ”asm”-läge (`azure config mode asm`) och utelämna hello resursgruppens namn i hello-kommandot.</span><span class="sxs-lookup"><span data-stu-id="59bb8-517">tooconfigure LAD for classic deployment model (ASM) VMs, switch too"asm" mode (`azure config mode asm`) and omit hello resource group name in hello command.</span></span> <span data-ttu-id="59bb8-518">Mer information finns i hello [plattformsoberoende CLI dokumentationen](https://docs.microsoft.com/azure/xplat-cli-connect).</span><span class="sxs-lookup"><span data-stu-id="59bb8-518">For more information, see hello [cross-platform CLI documentation](https://docs.microsoft.com/azure/xplat-cli-connect).</span></span>

## <a name="an-example-lad-30-configuration"></a><span data-ttu-id="59bb8-519">En exempelkonfiguration LAD 3.0</span><span class="sxs-lookup"><span data-stu-id="59bb8-519">An example LAD 3.0 configuration</span></span>

<span data-ttu-id="59bb8-520">Baserat på hello föregående definitioner, härs exempel LAD 3.0 tilläggets konfiguration med förklaringar.</span><span class="sxs-lookup"><span data-stu-id="59bb8-520">Based on hello preceding definitions, here's a sample LAD 3.0 extension configuration with some explanation.</span></span> <span data-ttu-id="59bb8-521">tooapply det här exemplet tooyour, bör du använda namnet på ditt eget lagringskonto, SAS-token-konto och EventHubs SAS-token.</span><span class="sxs-lookup"><span data-stu-id="59bb8-521">tooapply this sample tooyour case, you should use your own storage account name, account SAS token, and EventHubs SAS tokens.</span></span>

### <a name="privateconfigjson"></a><span data-ttu-id="59bb8-522">PrivateConfig.json</span><span class="sxs-lookup"><span data-stu-id="59bb8-522">PrivateConfig.json</span></span>

<span data-ttu-id="59bb8-523">Konfigurera inställningarna för privat:</span><span class="sxs-lookup"><span data-stu-id="59bb8-523">These private settings configure:</span></span>

* <span data-ttu-id="59bb8-524">ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="59bb8-524">a storage account</span></span>
* <span data-ttu-id="59bb8-525">en matchande konto-SAS-token</span><span class="sxs-lookup"><span data-stu-id="59bb8-525">a matching account SAS token</span></span>
* <span data-ttu-id="59bb8-526">flera egenskaperna (JsonBlob eller EventHubs med SAS-token)</span><span class="sxs-lookup"><span data-stu-id="59bb8-526">several sinks (JsonBlob or EventHubs with SAS tokens)</span></span>

```json
{
  "storageAccountName": "yourdiagstgacct",
  "storageAccountSasToken": "sv=xxxx-xx-xx&ss=bt&srt=co&sp=wlacu&st=yyyy-yy-yyT21%3A22%3A00Z&se=zzzz-zz-zzT21%3A22%3A00Z&sig=fake_signature",
  "sinksConfig": {
    "sink": [
      {
        "name": "SyslogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "FilelogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "MyJsonMetricsBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=fake_signature&se=1808096361&skn=yourehpolicy"
      },
      {
        "name": "MyMetricEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&skn=yourehpolicy"
      },
      {
        "name": "LoggingEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&se=1808096361&skn=yourehpolicy"
      }
    ]
  }
}
```

### <a name="publicconfigjson"></a><span data-ttu-id="59bb8-527">PublicConfig.json</span><span class="sxs-lookup"><span data-stu-id="59bb8-527">PublicConfig.json</span></span>

<span data-ttu-id="59bb8-528">Dessa inställningar för offentliga orsaka LAD till:</span><span class="sxs-lookup"><span data-stu-id="59bb8-528">These public settings cause LAD to:</span></span>

* <span data-ttu-id="59bb8-529">Överför procent processortid och använda diskutrymme mått toohello `WADMetrics*` tabell</span><span class="sxs-lookup"><span data-stu-id="59bb8-529">Upload percent-processor-time and used-disk-space metrics toohello `WADMetrics*` table</span></span>
* <span data-ttu-id="59bb8-530">Överföra meddelanden från syslog anläggning ”användare” och allvarlighetsgraden ”information” toohello `LinuxSyslog*` tabell</span><span class="sxs-lookup"><span data-stu-id="59bb8-530">Upload messages from syslog facility "user" and severity "info" toohello `LinuxSyslog*` table</span></span>
* <span data-ttu-id="59bb8-531">Överför rådata OMI frågan resultat (PercentProcessorTime och PercentIdleTime) toohello med namnet `LinuxCPU` tabell</span><span class="sxs-lookup"><span data-stu-id="59bb8-531">Upload raw OMI query results (PercentProcessorTime and PercentIdleTime) toohello named `LinuxCPU` table</span></span>
* <span data-ttu-id="59bb8-532">Överför tillagda rader i filen `/var/log/myladtestlog` toohello `MyLadTestLog` tabell</span><span class="sxs-lookup"><span data-stu-id="59bb8-532">Upload appended lines in file `/var/log/myladtestlog` toohello `MyLadTestLog` table</span></span>

<span data-ttu-id="59bb8-533">Data överförs också till i varje fall:</span><span class="sxs-lookup"><span data-stu-id="59bb8-533">In each case, data is also uploaded to:</span></span>

* <span data-ttu-id="59bb8-534">Azure Blob storage (behållarens namn är enligt definitionen i hello JsonBlob sink)</span><span class="sxs-lookup"><span data-stu-id="59bb8-534">Azure Blob storage (container name is as defined in hello JsonBlob sink)</span></span>
* <span data-ttu-id="59bb8-535">EventHubs slutpunkt (som anges i hello EventHubs sink)</span><span class="sxs-lookup"><span data-stu-id="59bb8-535">EventHubs endpoint (as specified in hello EventHubs sink)</span></span>

```json
{
  "StorageAccount": "yourdiagstgacct",
  "sampleRateInSeconds": 15,
  "ladCfg": {
    "diagnosticMonitorConfiguration": {
      "performanceCounters": {
        "sinks": "MyMetricEventHub,MyJsonMetricsBlob",
        "performanceCounterConfiguration": [
          {
            "unit": "Percent",
            "type": "builtin",
            "counter": "PercentProcessorTime",
            "counterSpecifier": "/builtin/Processor/PercentProcessorTime",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Aggregate CPU %utilization"
              }
            ],
            "condition": "IsAggregate=TRUE",
            "class": "Processor"
          },
          {
            "unit": "Bytes",
            "type": "builtin",
            "counter": "UsedSpace",
            "counterSpecifier": "/builtin/FileSystem/UsedSpace",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Used disk space on /"
              }
            ],
            "condition": "Name=\"/\"",
            "class": "Filesystem"
          }
        ]
      },
      "metrics": {
        "metricAggregation": [
          {
            "scheduledTransferPeriod": "PT1H"
          },
          {
            "scheduledTransferPeriod": "PT1M"
          }
        ],
        "resourceId": "/subscriptions/your_azure_subscription_id/resourceGroups/your_resource_group_name/providers/Microsoft.Compute/virtualMachines/your_vm_name"
      },
      "eventVolume": "Large",
      "syslogEvents": {
        "sinks": "SyslogJsonBlob,LoggingEventHub",
        "syslogEventConfiguration": {
          "LOG_USER": "LOG_INFO"
        }
      }
    }
  },
  "perfCfg": [
    {
      "query": "SELECT PercentProcessorTime, PercentIdleTime FROM SCX_ProcessorStatisticalInformation WHERE Name='_TOTAL'",
      "table": "LinuxCpu",
      "frequency": 60,
      "sinks": "LinuxCpuJsonBlob,LinuxCpuEventHub"
    }
  ],
  "fileLogs": [
    {
      "file": "/var/log/myladtestlog",
      "table": "MyLadTestLog",
      "sinks": "FilelogJsonBlob,LoggingEventHub"
    }
  ]
}
```

<span data-ttu-id="59bb8-536">Hej `resourceId` hello konfiguration måste matcha att ange hello VM eller hello virtuella skalan.</span><span class="sxs-lookup"><span data-stu-id="59bb8-536">hello `resourceId` in hello configuration must match that of hello VM or hello virtual machine scale set.</span></span>

* <span data-ttu-id="59bb8-537">Azure-plattformen mått diagram och avisering vet hello resourceId av hello VM som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="59bb8-537">Azure platform metrics charting and alerting knows hello resourceId of hello VM you're working on.</span></span> <span data-ttu-id="59bb8-538">Det förväntas toofind hello data för den virtuella datorn med hjälp av hello resourceId hello sökning nyckel.</span><span class="sxs-lookup"><span data-stu-id="59bb8-538">It expects toofind hello data for your VM using hello resourceId hello lookup key.</span></span>
* <span data-ttu-id="59bb8-539">Om du använder Azure Autoskala måste hello resourceId i hello Autoskala konfigurationen matcha hello resourceId används av LAD.</span><span class="sxs-lookup"><span data-stu-id="59bb8-539">If you use Azure autoscale, hello resourceId in hello autoscale configuration must match hello resourceId used by LAD.</span></span>
* <span data-ttu-id="59bb8-540">hello resourceId är inbyggd i hello namnen på JsonBlobs som skrivits av LAD.</span><span class="sxs-lookup"><span data-stu-id="59bb8-540">hello resourceId is built into hello names of JsonBlobs written by LAD.</span></span>

## <a name="view-your-data"></a><span data-ttu-id="59bb8-541">Granska dina data</span><span class="sxs-lookup"><span data-stu-id="59bb8-541">View your data</span></span>

<span data-ttu-id="59bb8-542">Använd hello Azure portal tooview prestandadata eller Ställ in aviseringar:</span><span class="sxs-lookup"><span data-stu-id="59bb8-542">Use hello Azure portal tooview performance data or set alerts:</span></span>

![Bild](./media/diagnostic-extension/graph_metrics.png)

<span data-ttu-id="59bb8-544">Hej `performanceCounters` data lagras alltid i en tabell i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="59bb8-544">hello `performanceCounters` data are always stored in an Azure Storage table.</span></span> <span data-ttu-id="59bb8-545">Azure Storage-API: er är tillgängliga för många språk och plattformar.</span><span class="sxs-lookup"><span data-stu-id="59bb8-545">Azure Storage APIs are available for many languages and platforms.</span></span>

<span data-ttu-id="59bb8-546">Data som skickas tooJsonBlob sänkor lagras i BLOB i hello lagringskontonamnet i hello [skyddade inställningarna](#protected-settings).</span><span class="sxs-lookup"><span data-stu-id="59bb8-546">Data sent tooJsonBlob sinks is stored in blobs in hello storage account named in hello [Protected settings](#protected-settings).</span></span> <span data-ttu-id="59bb8-547">Du kan använda hello blob-data med hjälp av alla Azure Blob Storage-API: er.</span><span class="sxs-lookup"><span data-stu-id="59bb8-547">You can consume hello blob data using any Azure Blob Storage APIs.</span></span>

<span data-ttu-id="59bb8-548">Dessutom kan du använda dessa UI verktyg tooaccess hello data i Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="59bb8-548">In addition, you can use these UI tools tooaccess hello data in Azure Storage:</span></span>

* <span data-ttu-id="59bb8-549">Visual Studio Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="59bb8-549">Visual Studio Server Explorer.</span></span>
* <span data-ttu-id="59bb8-550">[Microsoft Azure Lagringsutforskaren](https://azurestorageexplorer.codeplex.com/ "Azure Lagringsutforskaren").</span><span class="sxs-lookup"><span data-stu-id="59bb8-550">[Microsoft Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span></span>

<span data-ttu-id="59bb8-551">Den här ögonblicksbild av en Microsoft Azure Lagringsutforskaren session visar hello genererats Azure Storage-tabeller och behållare från en korrekt konfigurerad LAD 3.0-tillägg på ett test VM.</span><span class="sxs-lookup"><span data-stu-id="59bb8-551">This snapshot of a Microsoft Azure Storage Explorer session shows hello generated Azure Storage tables and containers from a correctly configured LAD 3.0 extension on a test VM.</span></span> <span data-ttu-id="59bb8-552">hello avbildningen matchar inte exakt hello [LAD 3.0 exempelkonfiguration](#an-example-lad-30-configuration).</span><span class="sxs-lookup"><span data-stu-id="59bb8-552">hello image doesn't match exactly with hello [sample LAD 3.0 configuration](#an-example-lad-30-configuration).</span></span>

![Bild](./media/diagnostic-extension/stg_explorer.png)

<span data-ttu-id="59bb8-554">Se hello relevanta [EventHubs dokumentationen](../../event-hubs/event-hubs-what-is-event-hubs.md) toolearn hur tooconsume meddelanden publicerade tooan EventHubs slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="59bb8-554">See hello relevant [EventHubs documentation](../../event-hubs/event-hubs-what-is-event-hubs.md) toolearn how tooconsume messages published tooan EventHubs endpoint.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59bb8-555">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59bb8-555">Next steps</span></span>

* <span data-ttu-id="59bb8-556">Skapa mått aviseringar i [Azure-Monitor](../../monitoring-and-diagnostics/insights-alerts-portal.md) för hello mått som du samlar in.</span><span class="sxs-lookup"><span data-stu-id="59bb8-556">Create metric alerts in [Azure Monitor](../../monitoring-and-diagnostics/insights-alerts-portal.md) for hello metrics you collect.</span></span>
* <span data-ttu-id="59bb8-557">Skapa [övervakning diagram](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) för din mått.</span><span class="sxs-lookup"><span data-stu-id="59bb8-557">Create [monitoring charts](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) for your metrics.</span></span>
* <span data-ttu-id="59bb8-558">Lär dig hur för[skapa en virtuella datorns skaluppsättning](/azure/virtual-machines/linux/tutorial-create-vmss) med din mått toocontrol autoskalning.</span><span class="sxs-lookup"><span data-stu-id="59bb8-558">Learn how too[create a virtual machine scale set](/azure/virtual-machines/linux/tutorial-create-vmss) using your metrics toocontrol autoscaling.</span></span>
