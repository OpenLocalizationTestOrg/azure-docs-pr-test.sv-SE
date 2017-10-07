---
title: aaaAzure virtuella skala anger anslutna Datadiskar | Microsoft Docs
description: "Lär dig hur toouse anslutna datadiskar med virtuella datorer"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: guybo
ms.openlocfilehash: 77b66f80934c0aaf7bb1ad0de00a738052a878ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-vm-scale-sets-and-attached-data-disks"></a><span data-ttu-id="eb78c-103">Skalningsuppsättningar för virtuella Azure-datorer och anslutna datadiskar</span><span class="sxs-lookup"><span data-stu-id="eb78c-103">Azure VM scale sets and attached data disks</span></span>
<span data-ttu-id="eb78c-104">[Skaluppsättningar för virtuella Azure-datorer](/azure/virtual-machine-scale-sets/) stöder nu virtuella datorer med anslutna datadiskar.</span><span class="sxs-lookup"><span data-stu-id="eb78c-104">Azure [virtual machine scale sets](/azure/virtual-machine-scale-sets/) now support virtual machines with attached data disks.</span></span> <span data-ttu-id="eb78c-105">Datadiskar kan definieras i hello lagringsprofilen för skalningsuppsättningar som har skapats med Azure hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="eb78c-105">Data disks can be defined in hello storage profile for scale sets that have been created with Azure Managed Disks.</span></span> <span data-ttu-id="eb78c-106">Tidigare var hello endast direktansluten lagring alternativen med virtuella datorer i skalningsuppsättningar hello OS-enhet och temp enheter.</span><span class="sxs-lookup"><span data-stu-id="eb78c-106">Previously hello only directly attached storage options available with VMs in scale sets were hello OS drive and temp drives.</span></span>

> [!NOTE]
>  <span data-ttu-id="eb78c-107">När du skapar en skala med anslutna datadiskar som har definierats, måste du ändå toomount och format hello diskar från inom en VM-toouse dem (precis som för fristående virtuella Azure-datorer).</span><span class="sxs-lookup"><span data-stu-id="eb78c-107">When you create a scale set with attached data disks defined, you still need toomount and format hello disks from within a VM toouse them (just like for standalone Azure VMs).</span></span> <span data-ttu-id="eb78c-108">Ett enkelt sätt toodo är toouse ett tillägg för anpassat skript som anropar ett standardskript toopartition och formatera hello data diskarna på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="eb78c-108">A convenient way toodo this is toouse a custom script extension which calls a standard script toopartition and format all hello data disks on a VM.</span></span>

## <a name="create-a-scale-set-with-attached-data-disks"></a><span data-ttu-id="eb78c-109">Skapa en skalningsuppsättning med anslutna datadiskar</span><span class="sxs-lookup"><span data-stu-id="eb78c-109">Create a scale set with attached data disks</span></span>
<span data-ttu-id="eb78c-110">Ett enkelt sätt toocreate skaluppsättning med anslutna diskar är toouse hello [Azure CLI](https://github.com/Azure/azure-cli) _vmss skapa_ kommando.</span><span class="sxs-lookup"><span data-stu-id="eb78c-110">A simple way toocreate a scale set with attached disks is toouse hello [Azure CLI](https://github.com/Azure/azure-cli) _vmss create_ command.</span></span> <span data-ttu-id="eb78c-111">hello följande exempel skapar en Azure-resursgrupp och en skalningsuppsättning 10 Ubuntu virtuella datorer, var och en med 2 bifogade datadiskar, 50 GB och 100 GB.</span><span class="sxs-lookup"><span data-stu-id="eb78c-111">hello following example creates an Azure resource group, and a VM scale set of 10 Ubuntu VMs, each with 2 attached data disks, of 50 GB and 100 GB respectively.</span></span>
```bash
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```
<span data-ttu-id="eb78c-112">Observera att hello _vmss skapa_ vissa konfigurationsvärden standardplatsen om du inte anger dem.</span><span class="sxs-lookup"><span data-stu-id="eb78c-112">Note that hello _vmss create_ command defaults certain configuration values if you do not specify them.</span></span> <span data-ttu-id="eb78c-113">toosee hello tillgängliga alternativ att du kan åsidosätta försök:</span><span class="sxs-lookup"><span data-stu-id="eb78c-113">toosee hello available options that you can override try:</span></span>
```bash
az vmss create --help
```
<span data-ttu-id="eb78c-114">Ett annat sätt toocreate en skala med anslutna diskar är toodefine en skala som i en Azure Resource Manager-mall innehåller en _dataDisks_ avsnitt i hello _storageProfile_, och distribuera hello mall.</span><span class="sxs-lookup"><span data-stu-id="eb78c-114">Another way toocreate a scale set with attached data disks is toodefine a scale set in an Azure Resource Manager template, include a _dataDisks_ section in hello _storageProfile_, and deploy hello template.</span></span> <span data-ttu-id="eb78c-115">hello 50 GB och 100 GB disk exemplet ovan skulle definieras så här i hello mallen:</span><span class="sxs-lookup"><span data-stu-id="eb78c-115">hello 50 GB and 100 GB disk example above would be defined like this in hello template:</span></span>
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    }
]
```
<span data-ttu-id="eb78c-116">Du kan se en fullständig, redo toodeploy exempel på en mall för skala med en ansluten disk som anges här: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).</span><span class="sxs-lookup"><span data-stu-id="eb78c-116">You can see a complete, ready toodeploy example of a scale set template with an attached disk defined here: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).</span></span>

## <a name="adding-a-data-disk-tooan-existing-scale-set"></a><span data-ttu-id="eb78c-117">Lägga till en befintlig skala för data disk tooan ange</span><span class="sxs-lookup"><span data-stu-id="eb78c-117">Adding a data disk tooan existing scale set</span></span>
> [!NOTE]
>  <span data-ttu-id="eb78c-118">Du kan bara bifoga diskar tooa skala datamängd som har skapats med [Azure hanterade diskar](./virtual-machine-scale-sets-managed-disks.md).</span><span class="sxs-lookup"><span data-stu-id="eb78c-118">You can only attach data disks tooa scale set which has been created with [Azure Managed Disks](./virtual-machine-scale-sets-managed-disks.md).</span></span>

<span data-ttu-id="eb78c-119">Du kan lägga till en data disk tooa VM-skala med Azure CLI _az vmss disk bifoga_ kommando.</span><span class="sxs-lookup"><span data-stu-id="eb78c-119">You can add a data disk tooa VM scale set using Azure CLI _az vmss disk attach_ command.</span></span> <span data-ttu-id="eb78c-120">Se till att du anger en lun som inte redan används.</span><span class="sxs-lookup"><span data-stu-id="eb78c-120">Make sure you specify a lun which is not already in use.</span></span> <span data-ttu-id="eb78c-121">följande exempel CLI hello lägger till en 50 GB enhet toolun 3:</span><span class="sxs-lookup"><span data-stu-id="eb78c-121">hello following CLI example adds a 50 GB drive toolun 3:</span></span>
```bash
az vmss disk attach -g dsktest -n dskvmss --size-gb 50 --lun 3
```

<span data-ttu-id="eb78c-122">hello följande PowerShell-exempel lägger till en 50 GB enhet toolun 3:</span><span class="sxs-lookup"><span data-stu-id="eb78c-122">hello following PowerShell example adds a 50 GB drive toolun 3:</span></span>
```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myvmssrg -VMScaleSetName myvmss
$vmss = Add-AzureRmVmssDataDisk -VirtualMachineScaleSet $vmss -Lun 3 -Caching 'ReadWrite' -CreateOption Empty -DiskSizeGB 50 -StorageAccountType StandardLRS
Update-AzureRmVmss -ResourceGroupName myvmssrg -Name myvmss -VirtualMachineScaleSet $vmss
```

> [!NOTE]
> <span data-ttu-id="eb78c-123">Olika storlekar på VM har olika begränsningar på hello antal anslutna enheter som de stöder.</span><span class="sxs-lookup"><span data-stu-id="eb78c-123">Different VM sizes have different limits on hello numbers of attached drives they support.</span></span> <span data-ttu-id="eb78c-124">Kontrollera hello [virtuella storlek egenskaper](../virtual-machines/windows/sizes.md) innan du lägger till en ny disk.</span><span class="sxs-lookup"><span data-stu-id="eb78c-124">Check hello [virtual machine size characteristics](../virtual-machines/windows/sizes.md) before adding a new disk.</span></span>

<span data-ttu-id="eb78c-125">Du kan också lägga till en disk genom att lägga till en ny post toohello _dataDisks_ egenskap i hello _storageProfile_ ange definition och tillämpa hello ändring av storlek.</span><span class="sxs-lookup"><span data-stu-id="eb78c-125">You can also add a disk by adding a new entry toohello _dataDisks_ property in hello _storageProfile_ of a scale set definition and applying hello change.</span></span> <span data-ttu-id="eb78c-126">tootest detta, hitta en befintlig scale set-definition i hello [resursutforskaren Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="eb78c-126">tootest this, find an existing scale set definition in hello [Azure Resource Explorer](https://resources.azure.com/).</span></span> <span data-ttu-id="eb78c-127">Välj _redigera_ och lägga till en ny disk toohello lista över datadiskar.</span><span class="sxs-lookup"><span data-stu-id="eb78c-127">Select _Edit_ and add a new disk toohello list of data disks.</span></span> <span data-ttu-id="eb78c-128">T.ex.</span><span class="sxs-lookup"><span data-stu-id="eb78c-128">E.g.</span></span> <span data-ttu-id="eb78c-129">använda hello-exemplet ovan:</span><span class="sxs-lookup"><span data-stu-id="eb78c-129">using hello example above:</span></span>
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    },
    {
    "lun": 3,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 20
    }          
]
```

<span data-ttu-id="eb78c-130">Välj sedan _PLACERA_ tooapply hello ändras tooyour skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="eb78c-130">Then select _PUT_ tooapply hello changes tooyour scale set.</span></span> <span data-ttu-id="eb78c-131">Det här exemplet skulle fungera så länge du använder en VM-storlek som har stöd för fler än två anslutna datadiskar.</span><span class="sxs-lookup"><span data-stu-id="eb78c-131">This example would work as long as you are using a VM size which supports more than two attached data disks.</span></span>

> [!NOTE]
> <span data-ttu-id="eb78c-132">När du gör en ändring tooa skaluppsättning definition, till exempel att lägga till eller ta bort en datadisk, det gäller tooall nyligen skapade virtuella datorer, men endast gäller tooexisting virtuella datorer om hello _upgradePolicy_ -egenskapen anges för ”automatisk”.</span><span class="sxs-lookup"><span data-stu-id="eb78c-132">When you make a change tooa scale set definition such as adding or removing a data disk, it applies tooall newly created VMs, but only applies tooexisting VMs if hello _upgradePolicy_ property is set too"Automatic".</span></span> <span data-ttu-id="eb78c-133">Om det har angetts för ”manuell” är toomanually måste tillämpa hello nya modellen tooexisting virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="eb78c-133">If it is set too"Manual", you need toomanually apply hello new model tooexisting VMs.</span></span> <span data-ttu-id="eb78c-134">Du kan göra detta i hello-portalen med hello _uppdatering AzureRmVmssInstance_ PowerShell, eller använda hello _az vmss update-instanser_ CLI-kommando.</span><span class="sxs-lookup"><span data-stu-id="eb78c-134">You can do this in hello portal, using hello _Update-AzureRmVmssInstance_ PowerShell command, or using hello _az vmss update-instances_ CLI command.</span></span>

## <a name="adding-pre-populated-data-disks-tooan-existent-scale-set"></a><span data-ttu-id="eb78c-135">Att lägga till data i förväg diskar tooan befintliga skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="eb78c-135">Adding pre-populated data disks tooan existent scale set</span></span> 
> <span data-ttu-id="eb78c-136">När du lägger till diskar tooan befintliga skaluppsättning modellen avsiktligt, hello disken skapas alltid tomt.</span><span class="sxs-lookup"><span data-stu-id="eb78c-136">When you add disks tooan existent scale set model, by design, hello disk will always be created empty.</span></span> <span data-ttu-id="eb78c-137">Det här scenariot omfattar också nya instanser som skapats av hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="eb78c-137">This scenario also includes new instances created by hello scale set.</span></span> <span data-ttu-id="eb78c-138">Det här beteendet är eftersom hello scaleset definition har en tom datadisk.</span><span class="sxs-lookup"><span data-stu-id="eb78c-138">This behaviour is because hello scaleset definition has an empty data disk.</span></span> <span data-ttu-id="eb78c-139">I ordning toocreate förifyllda dataenheter för en modell av befintliga scale set, kan du välja något av följande två alternativ:</span><span class="sxs-lookup"><span data-stu-id="eb78c-139">In order toocreate pre-populated data drives for an existent scale set model, you can choose either of next two options:</span></span>

* <span data-ttu-id="eb78c-140">Kopiera data från hello instans 0 VM toohello data diskarna i hello andra virtuella datorer genom att köra ett anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="eb78c-140">Copy data from hello instance 0 VM toohello data disk(s) in hello other VMs by running a custom script.</span></span>
* <span data-ttu-id="eb78c-141">Skapa en hanterad avbildning med hello OS-disk plus datadisk (med hello krävs data) och skapa en ny scaleset med hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="eb78c-141">Create a managed image with hello OS disk plus data disk (with hello required data) and create a new scaleset with hello image.</span></span> <span data-ttu-id="eb78c-142">Det här sättet varje ny virtuell dator skapas har en disk som som har angetts i hello definition av hello scaleset.</span><span class="sxs-lookup"><span data-stu-id="eb78c-142">This way every new VM created will have a data disk that that is provided in hello definition of hello scaleset.</span></span> <span data-ttu-id="eb78c-143">Eftersom den här definitionen kommer referera tooan avbildningen med en datadisk som har anpassade data kommer varje virtuell dator på hello scaleset automatiskt med ändringarna.</span><span class="sxs-lookup"><span data-stu-id="eb78c-143">Since this definition will refer tooan image with a data disk that has customized data, every virtual machine on hello scaleset will automatically come up with these changes.</span></span>

> <span data-ttu-id="eb78c-144">Hej sätt toocreate en anpassad avbildning finns här: [skapa en hanterad avbildning av en generaliserad virtuell dator i Azure](/azure/virtual-machines/windows/capture-image-resource/)</span><span class="sxs-lookup"><span data-stu-id="eb78c-144">hello way toocreate a custom image can be found here: [Create a managed image of a generalized VM in Azure](/azure/virtual-machines/windows/capture-image-resource/)</span></span> 

> <span data-ttu-id="eb78c-145">hello användare behöver toocapture hello instansen 0 VM som har hello nödvändiga data och sedan använda den virtuella hårddisken för hello avbildningen definition.</span><span class="sxs-lookup"><span data-stu-id="eb78c-145">hello user needs toocapture hello instance 0 VM which has hello required data, and then use that vhd for hello image definition.</span></span>

## <a name="removing-a-data-disk-from-a-scale-set"></a><span data-ttu-id="eb78c-146">Ta bort en datadisk från en skalningsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eb78c-146">Removing a data disk from a scale set</span></span>
<span data-ttu-id="eb78c-147">Du kan ta bort en datadisk från en skalningsuppsättning för virtuella datorer med Azure CLI-kommandot _az vmss disk detach_.</span><span class="sxs-lookup"><span data-stu-id="eb78c-147">You can remove a data disk from a VM scale set using Azure CLI _az vmss disk detach_ command.</span></span> <span data-ttu-id="eb78c-148">Till exempel hello följande kommando tar bort hello disk på lun 2:</span><span class="sxs-lookup"><span data-stu-id="eb78c-148">For example hello following command removes hello disk defined at lun 2:</span></span>
```bash
az vmss disk detach -g dsktest -n dskvmss --lun 2
```  
<span data-ttu-id="eb78c-149">På samma sätt du kan också ta bort en disk från en skaluppsättningen genom att ta bort en post från hello _dataDisks_ egenskap i hello _storageProfile_ och tillämpa hello ändringen.</span><span class="sxs-lookup"><span data-stu-id="eb78c-149">Similarly you can also remove a disk from a scale set by removing an entry from hello _dataDisks_ property in hello _storageProfile_ and applying hello change.</span></span> 

## <a name="additional-notes"></a><span data-ttu-id="eb78c-150">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="eb78c-150">Additional notes</span></span>
<span data-ttu-id="eb78c-151">Stöd för Azure Managed diskar och skala bifogade datadiskar är tillgängligt i API-versionen [ _2016-04-30-preview_ ](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) eller senare av hello Microsoft.Compute-API.</span><span class="sxs-lookup"><span data-stu-id="eb78c-151">Support for Azure Managed disks and scale set attached data disks is available in API version [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) or later of hello Microsoft.Compute API.</span></span>

<span data-ttu-id="eb78c-152">I hello första implementeringen av stöd för skalningsuppsättningar anslutna diskar, kan du koppla eller koppla från datadiskar till eller från enskilda virtuella datorer i en skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="eb78c-152">In hello initial implementation of attached disk support for scale sets, you cannot attach or detach data disks to/from individual VMs in a scale set.</span></span>

<span data-ttu-id="eb78c-153">Azure Portal-stöd för anslutna datadiskar i skalningsuppsättningar är begränsat i början.</span><span class="sxs-lookup"><span data-stu-id="eb78c-153">Azure portal support for attached data disks in scale sets is initially limited.</span></span> <span data-ttu-id="eb78c-154">Beroende på dina krav som du kan använda mallar för Azure CLI, PowerShell, SDK: er och REST API toomanage anslutna diskar.</span><span class="sxs-lookup"><span data-stu-id="eb78c-154">Depending on your requirements you can use Azure templates, CLI, PowerShell, SDKs, and REST API toomanage attached disks.</span></span>


