---
title: "Anger aaaDeploy en app på den virtuella datorn"
description: "Använd tillägg toodepoy en app på Azure Virtual Machine-Skalningsuppsättningar."
services: virtual-machine-scale-sets
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: adegeo
ms.openlocfilehash: 5f3988b9511d80370a8be1fc042c21fee212506e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a><span data-ttu-id="63156-103">Distribuera programmet på virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="63156-103">Deploy your application on virtual machine scale sets</span></span>

<span data-ttu-id="63156-104">Den här artikeln beskrivs olika sätt att hur tooinstall software hello hello skala in etableras.</span><span class="sxs-lookup"><span data-stu-id="63156-104">This article describes different ways of how tooinstall software at hello time hello scale set is provisioned.</span></span>

<span data-ttu-id="63156-105">Du kanske vill tooreview hello [skala ange översikt över Design](virtual-machine-scale-sets-design-overview.md) artikel som beskriver vissa av hello begränsningar har införts av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="63156-105">You may want tooreview hello [Scale Set Design Overview](virtual-machine-scale-sets-design-overview.md) article, which describes some of hello limits imposed by virtual machine scale sets.</span></span>

## <a name="capture-and-reuse-an-image"></a><span data-ttu-id="63156-106">Fånga och återanvända en avbildning</span><span class="sxs-lookup"><span data-stu-id="63156-106">Capture and reuse an image</span></span>

<span data-ttu-id="63156-107">Du kan använda en virtuell dator som du har i Azure tooprepare en bas-avbildning för din skala angett.</span><span class="sxs-lookup"><span data-stu-id="63156-107">You can use a virtual machine you have in Azure tooprepare a base-image for your scale set.</span></span> <span data-ttu-id="63156-108">Den här processen skapar hanterade diskar i ditt lagringskonto som du kan referera till som hello basavbildning för din skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="63156-108">This process creates a managed disk in your storage account, which you can reference as hello base image for your scale set.</span></span> 

<span data-ttu-id="63156-109">Hej gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="63156-109">Do hello following steps:</span></span>

1. <span data-ttu-id="63156-110">Skapa en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="63156-110">Create an Azure Virtual Machine</span></span>
   * <span data-ttu-id="63156-111">[Linux][linux-vm-create]</span><span class="sxs-lookup"><span data-stu-id="63156-111">[Linux][linux-vm-create]</span></span>
   * <span data-ttu-id="63156-112">[Windows][windows-vm-create]</span><span class="sxs-lookup"><span data-stu-id="63156-112">[Windows][windows-vm-create]</span></span>

2. <span data-ttu-id="63156-113">Fjärråtkomst till hello virtuell dator och anpassa hello system tooyour önskemål.</span><span class="sxs-lookup"><span data-stu-id="63156-113">Remote into hello virtual machine and customize hello system tooyour liking.</span></span>

   <span data-ttu-id="63156-114">Om du vill kan installera du programmet nu.</span><span class="sxs-lookup"><span data-stu-id="63156-114">If you want, you can install your application now.</span></span> <span data-ttu-id="63156-115">Dock vet att genom att installera programmet nu, kan du uppgradera ditt program som är mer komplicerad eftersom du kan behöva tooremove den första.</span><span class="sxs-lookup"><span data-stu-id="63156-115">However, know that by installing your application now, you may make upgrading your application more complicated because you may need tooremove it first.</span></span> <span data-ttu-id="63156-116">I stället kan du använda det här steget tooinstall allt ditt program kan behöva, som en specifik funktion för körning eller operativsystem.</span><span class="sxs-lookup"><span data-stu-id="63156-116">Instead, you can use this step tooinstall any prerequisites your application may need, like a specific runtime or operating system feature.</span></span>

3. <span data-ttu-id="63156-117">Följ hello ”skapar du en dator” självstudier för antingen [Linux] [ linux-vm-capture] eller [Windows][windows-vm-capture].</span><span class="sxs-lookup"><span data-stu-id="63156-117">Follow hello "capture a machine" tutorial for either [Linux][linux-vm-capture] or [Windows][windows-vm-capture].</span></span>

4. <span data-ttu-id="63156-118">Skapa en [Skaluppsättning för virtuell dator] [ vmss-create] med hello bild-URI som du har hämtat i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="63156-118">Create a [Virtual Machine Scale Set][vmss-create] with hello image URI you captured in hello previous step.</span></span>

<span data-ttu-id="63156-119">Mer information om diskar finns [översikt för hanterade diskar](../virtual-machines/windows/managed-disks-overview.md) och [Använd kopplade Datadiskar](virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="63156-119">For more information about disks, see [Managed Disks Overview](../virtual-machines/windows/managed-disks-overview.md) and [Use Attached Data Disks](virtual-machine-scale-sets-attached-disks.md).</span></span>

## <a name="install-when-hello-scale-set-is-provisioned"></a><span data-ttu-id="63156-120">Installera när hello skaluppsättning har etablerats</span><span class="sxs-lookup"><span data-stu-id="63156-120">Install when hello scale set is provisioned</span></span>

<span data-ttu-id="63156-121">Tillägg för virtuell dator kan vara tillämpas tooa virtuella datorns skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="63156-121">Virtual machine extensions can be applied tooa virtual machine scale set.</span></span> <span data-ttu-id="63156-122">Du kan anpassa hello virtuella datorer i en skala som angetts som en hel grupp med ett tillägg för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="63156-122">With a virtual machine extension, you can customize hello virtual machines in a scale set as a whole group.</span></span> <span data-ttu-id="63156-123">Mer information om tillägg finns [virtuella Datortillägg](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="63156-123">For more information about extensions, see [Virtual Machine Extensions](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="63156-124">Det finns tre huvudsakliga tillägg som du kan använda, beroende på om ditt operativsystem är Linux- eller Windows-baserade.</span><span class="sxs-lookup"><span data-stu-id="63156-124">There are three main extensions you can use, depending on if your operating system is Linux-based or Windows-based.</span></span>

### <a name="windows"></a><span data-ttu-id="63156-125">Windows</span><span class="sxs-lookup"><span data-stu-id="63156-125">Windows</span></span>

<span data-ttu-id="63156-126">För en Windows-baserade operativsystem, använder du antingen hello **anpassat skript v1.8** tillägg eller hello **PowerShell DSC** tillägg.</span><span class="sxs-lookup"><span data-stu-id="63156-126">For a Windows-based operating system, use either hello **Custom Script v1.8** extension, or hello **PowerShell DSC** extension.</span></span>

#### <a name="custom-script"></a><span data-ttu-id="63156-127">Anpassat skript</span><span class="sxs-lookup"><span data-stu-id="63156-127">Custom Script</span></span>

<span data-ttu-id="63156-128">hello tillägget för anpassat skript körs ett skript på varje virtuell datorinstans i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="63156-128">hello Custom Script extension runs a script on each virtual machine instance in hello scale set.</span></span> <span data-ttu-id="63156-129">Anger vilka filer som är hämtade toohello virtuella datorn och sedan vilken kommandot Kör en .config-fil eller en variabel.</span><span class="sxs-lookup"><span data-stu-id="63156-129">A config file or variable indicates which files are downloaded toohello virtual machine, and then what command runs.</span></span> <span data-ttu-id="63156-130">Du kan använda den här toorun ett installationsprogram, ett skript, en kommandofil, körbara filer till exempel.</span><span class="sxs-lookup"><span data-stu-id="63156-130">You could use this toorun an installer, a script, a batch file, any executable for example.</span></span>

<span data-ttu-id="63156-131">PowerShell använder en hash-tabell för hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="63156-131">PowerShell uses a hashtable for hello settings.</span></span> <span data-ttu-id="63156-132">Det här exemplet konfigureras hello anpassat skript tillägget toorun ett PowerShell.skript som installerar IIS.</span><span class="sxs-lookup"><span data-stu-id="63156-132">This example configures hello custom script extension toorun a PowerShell script that installs IIS.</span></span>

```powershell
# Setup extension configuration hashtable variable
$customConfig = @{
  "fileUris" = @("https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1");
  "commandToExecute" = "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> `"%TEMP%\StartupLog.txt`" 2>&1";
};

# Add hello extension toohello config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Compute -Type CustomScriptExtension -TypeHandlerVersion 1.8 -Name "customscript1" -Setting $customConfig

# Send hello new config tooAzure
Update-AzureRmVmss -ResourceGroupName $rg -Name "MyVmssTest143"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
><span data-ttu-id="63156-133">Använd hello `-ProtectedSetting` växla för alla inställningar som kan innehålla känslig information.</span><span class="sxs-lookup"><span data-stu-id="63156-133">Use hello `-ProtectedSetting` switch for any settings that may contain sensitive information.</span></span>

---------


<span data-ttu-id="63156-134">Azure CLI använder en json-fil för hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="63156-134">Azure CLI uses a json file for hello settings.</span></span> <span data-ttu-id="63156-135">Det här exemplet konfigureras hello anpassat skript tillägget toorun ett PowerShell.skript som installerar IIS.</span><span class="sxs-lookup"><span data-stu-id="63156-135">This example configures hello custom script extension toorun a PowerShell script that installs IIS.</span></span> <span data-ttu-id="63156-136">Spara hello följande json-fil som _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="63156-136">Save hello following json file as _settings.json_.</span></span>

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1"
  ],
  "commandToExecute": "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> \"%TEMP%\StartupLog.txt\" 2>&1"
}
```

<span data-ttu-id="63156-137">Kör sedan kommandot Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="63156-137">Then, run this Azure CLI command.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Compute --version 1.8 --name CustomScriptExtension --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="63156-138">Använd hello `--protected-settings` växla för alla inställningar som kan innehålla känslig information.</span><span class="sxs-lookup"><span data-stu-id="63156-138">Use hello `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

### <a name="powershell-dsc"></a><span data-ttu-id="63156-139">PowerShell DSC</span><span class="sxs-lookup"><span data-stu-id="63156-139">PowerShell DSC</span></span>

<span data-ttu-id="63156-140">Du kan använda PowerShell DSC toocustomize hello scale set vm-instanser.</span><span class="sxs-lookup"><span data-stu-id="63156-140">You can use PowerShell DSC toocustomize hello scale set vm instances.</span></span> <span data-ttu-id="63156-141">Hej **DSC** tillägg som publicerats av **Microsoft.Powershell** distribuerar och kör hello tillhandahålls DSC-konfigurationen på varje virtuell dator-instans.</span><span class="sxs-lookup"><span data-stu-id="63156-141">hello **DSC** extension published by **Microsoft.Powershell** deploys and runs hello provided DSC configuration on each virtual machine instance.</span></span> <span data-ttu-id="63156-142">En .config-fil eller en variabel visar hello tillägget där *.zip* paketet är och vilka _skript-funktionen_ kombination toorun.</span><span class="sxs-lookup"><span data-stu-id="63156-142">A config file or variable tells hello extension where *.zip* package is, and which _script-function_ combination toorun.</span></span>

<span data-ttu-id="63156-143">PowerShell använder en hash-tabell för hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="63156-143">PowerShell uses a hashtable for hello settings.</span></span> <span data-ttu-id="63156-144">Det här exemplet används ett DSC-paket som installerar IIS.</span><span class="sxs-lookup"><span data-stu-id="63156-144">This example deploys a DSC package that installs IIS.</span></span>

```powershell
# Setup extension configuration hashtable variable
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Add hello extension toohello config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Powershell -Type DSC -TypeHandlerVersion 2.24 -Name "dsc1" -Setting $dscConfig

# Send hello new config tooAzure
Update-AzureRmVmss -ResourceGroupName $rg -Name "myscaleset1"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
><span data-ttu-id="63156-145">Använd hello `-ProtectedSetting` växla för alla inställningar som kan innehålla känslig information.</span><span class="sxs-lookup"><span data-stu-id="63156-145">Use hello `-ProtectedSetting` switch for any settings that may contain sensitive information.</span></span>

-----------

<span data-ttu-id="63156-146">Azure CLI använder en json för hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="63156-146">Azure CLI uses a json for hello settings.</span></span> <span data-ttu-id="63156-147">Det här exemplet används ett DSC-paket som installerar IIS.</span><span class="sxs-lookup"><span data-stu-id="63156-147">This example deploys a DSC package that installs IIS.</span></span> <span data-ttu-id="63156-148">Spara hello följande json-fil som _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="63156-148">Save hello following json file as _settings.json_.</span></span>

```json
{
  "wmfVersion": "latest",
  "configuration": {
    "url": "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip",
    "script": "configure-http.ps1",
    "function": "WebsiteTest"
  }
}
```

<span data-ttu-id="63156-149">Kör sedan kommandot Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="63156-149">Then, run this Azure CLI command.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Powershell --version 2.24 --name DSC --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="63156-150">Använd hello `--protected-settings` växla för alla inställningar som kan innehålla känslig information.</span><span class="sxs-lookup"><span data-stu-id="63156-150">Use hello `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

### <a name="linux"></a><span data-ttu-id="63156-151">Linux</span><span class="sxs-lookup"><span data-stu-id="63156-151">Linux</span></span>

<span data-ttu-id="63156-152">Linux kan använda antingen hello **anpassat skript v2.0** tillägg eller Använd **moln init** under skapandet.</span><span class="sxs-lookup"><span data-stu-id="63156-152">Linux can use either hello **Custom Script v2.0** extension or use **cloud-init** during creation.</span></span>

<span data-ttu-id="63156-153">Anpassat skript är en enkel tillägg som laddar ned filer toohello virtuella instanser och kör ett kommando.</span><span class="sxs-lookup"><span data-stu-id="63156-153">Custom script is a simple extension that downloads files toohello virtual machine instances, and runs a command.</span></span>

#### <a name="custom-script"></a><span data-ttu-id="63156-154">Anpassat skript</span><span class="sxs-lookup"><span data-stu-id="63156-154">Custom Script</span></span>

<span data-ttu-id="63156-155">Spara hello följande json-fil som _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="63156-155">Save hello following json file as _settings.json_.</span></span>

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
  ],
  "commandToExecute": "bash installserver.sh"
}
```

<span data-ttu-id="63156-156">Använd hello Azure CLI tooadd det här tillägget tooan befintliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="63156-156">Use hello Azure CLI tooadd this extension tooan existing virtual machine scale set.</span></span> <span data-ttu-id="63156-157">Varje virtuell dator i hello skala in automatiskt körs hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="63156-157">Each virtual machine in hello scale set automatically runs hello extension.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="63156-158">Använd hello `--protected-settings` växla för alla inställningar som kan innehålla känslig information.</span><span class="sxs-lookup"><span data-stu-id="63156-158">Use hello `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

#### <a name="cloud-init"></a><span data-ttu-id="63156-159">Molnet initiering</span><span class="sxs-lookup"><span data-stu-id="63156-159">Cloud-Init</span></span>

<span data-ttu-id="63156-160">Molnet Init används när hello scale set skapas.</span><span class="sxs-lookup"><span data-stu-id="63156-160">Cloud-Init is used when hello scale set is created.</span></span> <span data-ttu-id="63156-161">Skapa först en lokal fil med namnet _moln init.txt_ och Lägg till din konfiguration tooit.</span><span class="sxs-lookup"><span data-stu-id="63156-161">First, create a local file named _cloud-init.txt_ and add your configuration tooit.</span></span> <span data-ttu-id="63156-162">Se exempelvis [denna gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)</span><span class="sxs-lookup"><span data-stu-id="63156-162">For example, see [this gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)</span></span>

<span data-ttu-id="63156-163">Använd hello Azure CLI toocreate en skala har angetts.</span><span class="sxs-lookup"><span data-stu-id="63156-163">Use hello Azure CLI toocreate a scale set.</span></span> <span data-ttu-id="63156-164">Hej `--custom-data` fältet accepterar hello filnamnet för ett moln init-skript.</span><span class="sxs-lookup"><span data-stu-id="63156-164">hello `--custom-data` field accepts hello file name of a cloud-init script.</span></span>

```azurecli
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image Canonical:UbuntuServer:14.04.4-LTS:latest \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

## <a name="how-do-i-manage-application-updates"></a><span data-ttu-id="63156-165">Hur hanterar programuppdateringar?</span><span class="sxs-lookup"><span data-stu-id="63156-165">How do I manage application updates?</span></span>

<span data-ttu-id="63156-166">Om du har distribuerat ditt program via ett tillägg alter hello resurstilläggsdefinitionen på något sätt.</span><span class="sxs-lookup"><span data-stu-id="63156-166">If you deployed your application through an extension, alter hello extension definition in some way.</span></span> <span data-ttu-id="63156-167">Den här ändringen gör hello tillägget toobe omdistribueras tooall virtuella instanser.</span><span class="sxs-lookup"><span data-stu-id="63156-167">This change causes hello extension toobe redeployed tooall virtual machine instances.</span></span> <span data-ttu-id="63156-168">Något **måste** ändras om hello filnamnstillägg, till exempel byta namn på en refererade filen, annars Azure har inte se som hello tillägg har ändrats.</span><span class="sxs-lookup"><span data-stu-id="63156-168">Something **must** be changed about hello extension, such as renaming a referenced file, otherwise, Azure does not see that hello extension has changed.</span></span>

<span data-ttu-id="63156-169">Om du inbyggd hello program i din egen avbildning av operativsystemet kan du använda en pipeline för automatisk distribution för programuppdateringar.</span><span class="sxs-lookup"><span data-stu-id="63156-169">If you baked hello application into your own operating system image, use an automated deployment pipeline for application updates.</span></span> <span data-ttu-id="63156-170">Utforma din arkitektur toofacilitate snabb växling av en stegvis skala i produktion.</span><span class="sxs-lookup"><span data-stu-id="63156-170">Design your architecture toofacilitate rapid swapping of a staged scale set into production.</span></span> <span data-ttu-id="63156-171">Ett bra exempel på den här metoden är hello [Azure Spinnaker drivrutinen arbete](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).</span><span class="sxs-lookup"><span data-stu-id="63156-171">A good example of this approach is hello [Azure Spinnaker driver work](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).</span></span>

<span data-ttu-id="63156-172">[Packare](https://www.packer.io/) och [Terraform](https://www.terraform.io/) stöd för Azure Resource Manager, så att du kan definiera dina bilder ”som kod” och skapar dem i Azure, sedan använda hello VHD i en skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="63156-172">[Packer](https://www.packer.io/) and [Terraform](https://www.terraform.io/) support Azure Resource Manager, so you can also define your images "as code" and build them in Azure, then use hello VHD in your scale set.</span></span> <span data-ttu-id="63156-173">Dock skulle göra så bli problematisk för marketplace-bilder, där tillägg/anpassade skript bli viktigare eftersom du inte direkt ändra bits från marketplace.</span><span class="sxs-lookup"><span data-stu-id="63156-173">However, doing so would become problematic for marketplace images, where extensions/custom scripts become more important since you don’t directly manipulate bits from marketplace.</span></span>

## <a name="what-happens-when-a-scale-set-scales-out"></a><span data-ttu-id="63156-174">Vad händer när en skaluppsättning för skalor ut?</span><span class="sxs-lookup"><span data-stu-id="63156-174">What happens when a scale set scales out?</span></span>
<span data-ttu-id="63156-175">Hello programmet installeras automatiskt när du lägger till en eller flera virtuella datorer tooa skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="63156-175">When you add one or more virtual machines tooa scale set, hello application is automatically installed.</span></span> <span data-ttu-id="63156-176">För exempel om hello skalan har tillägg som definierats kan köras de på en ny virtuell dator när den skapas.</span><span class="sxs-lookup"><span data-stu-id="63156-176">For example if hello scale set has extensions defined, they run on a new virtual machine each time it is created.</span></span> <span data-ttu-id="63156-177">Om hello skaluppsättning baseras på en anpassad avbildning är en kopia av anpassade hello Källavbildningen eventuella nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="63156-177">If hello scale set is based on a custom image, any new virtual machine is a copy of hello source custom image.</span></span> <span data-ttu-id="63156-178">Om hello skala uppsättning virtuella datorer är värdar för behållaren, kanske du har Start kod tooload hello behållare i tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="63156-178">If hello scale set virtual machines are container hosts, then you might have startup code tooload hello containers in a Custom Script Extension.</span></span> <span data-ttu-id="63156-179">Eller ett tillägg kan installera en agent som registreras med en orchestrator för klustret, till exempel Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="63156-179">Or, an extension might install an agent that registers with a cluster orchestrator, such as Azure Container Service.</span></span>


## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a><span data-ttu-id="63156-180">Hur du lanserar en OS-uppdatering update domäner?</span><span class="sxs-lookup"><span data-stu-id="63156-180">How do you roll out an OS update across update domains?</span></span>
<span data-ttu-id="63156-181">Anta att du vill tooupdate OS-avbildningen samtidigt som hello virtuella skaluppsättningen körs.</span><span class="sxs-lookup"><span data-stu-id="63156-181">Suppose you want tooupdate your OS image while keeping hello virtual machine scale set running.</span></span> <span data-ttu-id="63156-182">PowerShell och hello Azure CLI kan uppdatera hello avbildningar av virtuella datorer, en virtuell dator i taget.</span><span class="sxs-lookup"><span data-stu-id="63156-182">PowerShell and hello Azure CLI can update hello virtual machine images, one virtual machine at a time.</span></span> <span data-ttu-id="63156-183">Hej [uppgradera en Virtual Machine Scale Set](./virtual-machine-scale-sets-upgrade-scale-set.md) artikeln innehåller även ytterligare information om vilka alternativ som är tillgängliga tooperform uppgraderar du ett operativsystem i en skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="63156-183">hello [Upgrade a Virtual Machine Scale Set](./virtual-machine-scale-sets-upgrade-scale-set.md) article also provides further information on what options are available tooperform an operating system upgrade across a virtual machine scale set.</span></span>

## <a name="next-steps"></a><span data-ttu-id="63156-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="63156-184">Next steps</span></span>

* [<span data-ttu-id="63156-185">Använd PowerShell toomanage din skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="63156-185">Use PowerShell toomanage your scale set.</span></span>](virtual-machine-scale-sets-windows-manage.md)
* [<span data-ttu-id="63156-186">Skapa en mall för skalan.</span><span class="sxs-lookup"><span data-stu-id="63156-186">Create a scale set template.</span></span>](virtual-machine-scale-sets-mvss-start.md)


[linux-vm-create]: ../virtual-machines/linux/tutorial-manage-vm.md
[windows-vm-create]: ../virtual-machines/windows/tutorial-manage-vm.md
[linux-vm-capture]: ../virtual-machines/linux/capture-image.md
[windows-vm-capture]: ../virtual-machines/windows/capture-image.md 
[vmss-create]: virtual-machine-scale-sets-create.md

