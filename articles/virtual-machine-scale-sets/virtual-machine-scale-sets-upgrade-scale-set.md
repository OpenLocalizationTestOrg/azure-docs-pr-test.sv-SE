---
title: "aaaUpgrade en virtuell dator i Azure skaluppsättning | Microsoft Docs"
description: "Uppgradera en skaluppsättning för virtuell Azure-dator"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: guybo
ms.openlocfilehash: 068e98503f8d37ea71e45b8673a01da2e814f521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-virtual-machine-scale-set"></a><span data-ttu-id="fe0c2-103">Uppgradera en virtuella datorns skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="fe0c2-103">Upgrade a virtual machine scale set</span></span>
<span data-ttu-id="fe0c2-104">Den här artikeln beskriver hur du kan distribuera en OS uppdatering tooan virtuella Azure-datorn skaluppsättningen utan driftavbrott.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-104">This article describes how you can roll out an OS update tooan Azure virtual machine scale set without any downtime.</span></span> <span data-ttu-id="fe0c2-105">I den här kontexten innebär en OS-uppdatering ändra hello version eller SKU hello OS eller ändra hello URI för en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-105">In this context, an OS update involves changing hello version or SKU of hello OS or changing hello URI of a custom image.</span></span> <span data-ttu-id="fe0c2-106">Uppdaterar utan driftavbrott uppdatera virtuella datorer en i taget eller grupper (till exempel en feldomän i taget) i stället för på en gång.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-106">Updating without downtime means updating virtual machines one at a time or in groups (such as one fault domain at a time) rather than all at once.</span></span> <span data-ttu-id="fe0c2-107">På så sätt, kan alla virtuella datorer som inte uppgraderas att köra.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-107">By doing so, any virtual machines that are not being upgraded can keep running.</span></span>

<span data-ttu-id="fe0c2-108">tooavoid tvetydighet skilja mellan fyra typer av OS-uppdateringar som du kanske vill vi tooperform:</span><span class="sxs-lookup"><span data-stu-id="fe0c2-108">tooavoid ambiguity, let’s distinguish four types of OS update you might want tooperform:</span></span>

* <span data-ttu-id="fe0c2-109">Ändra hello version eller SKU av en plattformsavbildning.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-109">Changing hello version or SKU of a platform image.</span></span> <span data-ttu-id="fe0c2-110">Till exempel ändra Ubuntu 14.04.2-LTS version från 14.04.201506100 too14.04.201507060 eller ändra hello Ubuntu 15.10/senaste SKU too16.04.0-LTS/latest.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-110">For example, changing Ubuntu 14.04.2-LTS version from 14.04.201506100 too14.04.201507060, or changing hello Ubuntu 15.10/latest SKU too16.04.0-LTS/latest.</span></span> <span data-ttu-id="fe0c2-111">Det här scenariot beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-111">This scenario is covered in this article.</span></span>
* <span data-ttu-id="fe0c2-112">Ändra hello URI som pekar tooa ny version av en anpassad avbildning som du skapat (**egenskaper** > **virtualMachineProfile** > **storageProfile**  >  **osDisk** > **bild** > **uri**).</span><span class="sxs-lookup"><span data-stu-id="fe0c2-112">Changing hello URI that points tooa new version of a custom image you built (**properties** > **virtualMachineProfile** > **storageProfile** > **osDisk** > **image** > **uri**).</span></span> <span data-ttu-id="fe0c2-113">Det här scenariot beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-113">This scenario is covered in this article.</span></span>
* <span data-ttu-id="fe0c2-114">Ändra hello bildreferens för skaluppsättning som har skapats med hjälp av Azure hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-114">Changing hello image reference of a scale set that was created using Azure Managed Disks.</span></span>
* <span data-ttu-id="fe0c2-115">Korrigering hello operativsystem från en virtuell dator (exempel på detta är hur du installerar en säkerhetskorrigering och kör Windows Update).</span><span class="sxs-lookup"><span data-stu-id="fe0c2-115">Patching hello OS from within a virtual machine (examples of this include installing a security patch and running Windows Update).</span></span> <span data-ttu-id="fe0c2-116">Det här scenariot stöds, men som inte omfattas i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-116">This scenario is supported but not covered in this article.</span></span>

<span data-ttu-id="fe0c2-117">Skalningsuppsättningar i virtuella datorer som distribueras som en del av en [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) kluster beskrivs inte här.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-117">Virtual machine scale sets that are deployed as part of an [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) cluster are not covered here.</span></span> <span data-ttu-id="fe0c2-118">Se [korrigering Windows OS i Service Fabric-kluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) mer information om Service Fabric-korrigering.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-118">See [Patch Windows OS in your Service Fabric cluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) for more information about patching Service Fabric.</span></span>

<span data-ttu-id="fe0c2-119">hello grundläggande aktivitetssekvensen för att ändra hello OS-version/SKU av en plattformsavbildning eller hello URI för en anpassad avbildning ser ut som följer:</span><span class="sxs-lookup"><span data-stu-id="fe0c2-119">hello basic sequence for changing hello OS version/SKU of a platform image or hello URI of a custom image looks as follows:</span></span>

1. <span data-ttu-id="fe0c2-120">Hämta hello virtuella skala modellen.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-120">Get hello virtual machine scale set model.</span></span>
2. <span data-ttu-id="fe0c2-121">Ändra hello version SKU, bildreferens eller URI-värdet i hello modellen.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-121">Change hello version, SKU, image reference, or URI value in hello model.</span></span>
3. <span data-ttu-id="fe0c2-122">Uppdatera hello modellen.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-122">Update hello model.</span></span>
4. <span data-ttu-id="fe0c2-123">Gör en *manualUpgrade* anropa på hello virtuella datorer i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-123">Do a *manualUpgrade* call on hello virtual machines in hello scale set.</span></span> <span data-ttu-id="fe0c2-124">Det här steget gäller endast om *upgradePolicy* har angetts för**manuell** i din skaluppsättningen.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-124">This step is only relevant if *upgradePolicy* is set too**Manual** in your scale set.</span></span> <span data-ttu-id="fe0c2-125">Om det har angetts för**automatisk**, alla hello virtuella datorer uppgraderas samtidigt, vilket orsakar driftstopp.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-125">If it is set too**Automatic**, all hello virtual machines are upgraded at once, thus causing downtime.</span></span>

<span data-ttu-id="fe0c2-126">Med denna information i åtanke kan du nu ska vi se hur du kan uppdatera hello version av en skala som angetts i PowerShell och genom att använda hello REST API.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-126">With this information in mind, let’s see how you could update hello version of a scale set in PowerShell, and by using hello REST API.</span></span> <span data-ttu-id="fe0c2-127">De här exemplen täcker hello fall av en plattformsavbildning, men den här artikeln innehåller tillräckligt med information för tooadapt du den här processen tooa anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-127">These examples cover hello case of a platform image, but this article provides enough information for you tooadapt this process tooa custom image.</span></span>

## <a name="powershell"></a><span data-ttu-id="fe0c2-128">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fe0c2-128">PowerShell</span></span>
<span data-ttu-id="fe0c2-129">Det här exemplet uppdaterar en skaluppsättning för Windows virtuell dator (skapa toohello ny version 4.0.20160229.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-129">This example updates a Windows virtual machine scale set (creating toohello new version 4.0.20160229.</span></span> <span data-ttu-id="fe0c2-130">När du har uppdaterat hello modellen har en uppdatering för en virtuell datorinstans i taget.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-130">After updating hello model, it does an update one virtual machine instance at a time.</span></span>

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get hello VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update hello virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

<span data-ttu-id="fe0c2-131">Om du uppdaterar hello URI för en anpassad avbildning istället för att ändra en plattform Avbildningsversion, ersätter hello ”ange hello nya version” rad med ett kommando som kommer att uppdatera hello Källavbildningen URI.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-131">If you are updating hello URI for a custom image instead of changing a platform image version, replace hello “set hello new version” line with a command that will update hello source image URI.</span></span> <span data-ttu-id="fe0c2-132">Till exempel om hello skaluppsättning skapades utan att använda Azure hanterade diskar, hello update skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="fe0c2-132">For example, if hello scale set was created without using Azure Managed Disks, hello update would look like this:</span></span>

```powershell
# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```

<span data-ttu-id="fe0c2-133">Om en anpassad avbildning baserad skaluppsättning har skapats med hjälp av Azure hanterade diskar och sedan hello bildreferens skulle uppdateras.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-133">If a custom image based scale set was created using Azure Managed Disks, then hello image reference would be updated.</span></span> <span data-ttu-id="fe0c2-134">Exempel:</span><span class="sxs-lookup"><span data-stu-id="fe0c2-134">For example:</span></span>

```powershell
# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.imageReference.id = $newImageReference
```

## <a name="hello-rest-api"></a><span data-ttu-id="fe0c2-135">hello REST API</span><span class="sxs-lookup"><span data-stu-id="fe0c2-135">hello REST API</span></span>
<span data-ttu-id="fe0c2-136">Här är några av Python-exempel som använder hello Azure REST API tooroll ut en uppdatering för OS-version.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-136">Here are a couple of Python examples that use hello Azure REST API tooroll out an OS version update.</span></span> <span data-ttu-id="fe0c2-137">Använder båda hello lightweight [azurerm](https://pypi.python.org/pypi/azurerm) bibliotek med Azure REST API-omslutning funktioner toodo GET på hello skala ange modell, följt av en PUT med en uppdaterad modell.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-137">Both use hello lightweight [azurerm](https://pypi.python.org/pypi/azurerm) library of Azure REST API wrapper functions toodo a GET on hello scale set model, followed by a PUT with an updated model.</span></span> <span data-ttu-id="fe0c2-138">De också titta på virtuella instanser vyer tooidentify hello virtuella datorer av uppdateringsdomän.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-138">They also look at virtual machine instances views tooidentify hello virtual machines by update domain.</span></span>

### <a name="vmssupgrade"></a><span data-ttu-id="fe0c2-139">Vmssupgrade</span><span class="sxs-lookup"><span data-stu-id="fe0c2-139">Vmssupgrade</span></span>
 <span data-ttu-id="fe0c2-140">[Vmssupgrade](https://github.com/gbowerman/vmsstools) Python-skriptet som tooroll ut en uppgradering OS-tooa som kör virtuella datorn har ställts in en uppdateringsdomän i taget.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-140">[Vmssupgrade](https://github.com/gbowerman/vmsstools) is a Python script that's used tooroll out an OS upgrade tooa running virtual machine scale set one update domain at a time.</span></span>

![Vmssupgrade skript för att välja virtuella datorer eller en uppdateringsdomän](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

<span data-ttu-id="fe0c2-142">Det här skriptet kan du välja specifika virtuella datorer tooupdate eller ange en uppdateringsdomän.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-142">This script lets you choose specific virtual machines tooupdate or specify an update domain.</span></span> <span data-ttu-id="fe0c2-143">Det stöder ändra en plattform Avbildningsversion eller ändra hello URI för en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-143">It supports changing a platform image version or changing hello URI of a custom image.</span></span>

### <a name="vmsseditor"></a><span data-ttu-id="fe0c2-144">Vmsseditor</span><span class="sxs-lookup"><span data-stu-id="fe0c2-144">Vmsseditor</span></span>
<span data-ttu-id="fe0c2-145">[Vmsseditor](https://github.com/gbowerman/vmssdashboard) är en generell redigerare för skalningsuppsättningar i virtuella datorer som visar virtuella datorn status som ett heatmap där en rad representerar en uppdateringsdomän.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-145">[Vmsseditor](https://github.com/gbowerman/vmssdashboard) is a general-purpose editor for virtual machine scale sets that shows virtual machine status as a heatmap where one row represents one update domain.</span></span> <span data-ttu-id="fe0c2-146">Bland annat kan du uppdatera hello modell för en skalningsuppsättning med en ny version, en SKU eller en anpassad avbildning URI och välj sedan fel domäner tooupgrade.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-146">Among other things, you can update hello model for a scale set with a new version, SKU, or custom image URI, and then pick fault domains tooupgrade.</span></span> <span data-ttu-id="fe0c2-147">När du gör det, är alla hello virtuella datorer i domänen update uppgraderade toohello ny modell.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-147">When you do so, all hello virtual machines in that update domain are upgraded toohello new model.</span></span> <span data-ttu-id="fe0c2-148">Alternativt kan du göra löpande uppgradering baserat på hello batchstorlek önskat.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-148">Alternatively, you can do a rolling upgrade based on hello batch size of your choice.</span></span>  

<span data-ttu-id="fe0c2-149">hello visar följande skärmbild en modell av en skala som angetts för Ubuntu 14.04-2LTS version 14.04.201507060.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-149">hello following screenshot shows a model of a scale set for Ubuntu 14.04-2LTS version 14.04.201507060.</span></span> <span data-ttu-id="fe0c2-150">Många fler alternativ har lagts till toothis verktyget eftersom den här skärmbilden har tagits.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-150">Many more options have been added toothis tool since this screenshot was taken.</span></span>

![Vmsseditor modell för en skala som angetts för Ubuntu 14.04-2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

<span data-ttu-id="fe0c2-152">När du klickar på **uppgradera** och sedan **Hämta detaljer**, virtuella datorer i UD 0 Starta tooupdate.</span><span class="sxs-lookup"><span data-stu-id="fe0c2-152">After you click **Upgrade** and then **Get Details**, virtual machines in UD 0 start tooupdate.</span></span>

![Vmsseditor visar uppdatering pågår](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

