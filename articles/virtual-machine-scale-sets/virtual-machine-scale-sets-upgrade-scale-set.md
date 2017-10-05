---
title: "Uppgradera en skaluppsättning för virtuell dator i Azure | Microsoft Docs"
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
ms.openlocfilehash: c7093e221ff8fe69ded1cfbce4f3ddeb1a195666
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-a-virtual-machine-scale-set"></a><span data-ttu-id="a2fe2-103">Uppgradera en virtuella datorns skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="a2fe2-103">Upgrade a virtual machine scale set</span></span>
<span data-ttu-id="a2fe2-104">Den här artikeln beskriver hur du lanserar en OS-uppdatering till en virtuell dator i Azure-skala utan driftavbrott.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-104">This article describes how you can roll out an OS update to an Azure virtual machine scale set without any downtime.</span></span> <span data-ttu-id="a2fe2-105">I den här kontexten innebär en OS-uppdatering ändra version eller SKU av Operativsystemet eller ändra URI för en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-105">In this context, an OS update involves changing the version or SKU of the OS or changing the URI of a custom image.</span></span> <span data-ttu-id="a2fe2-106">Uppdaterar utan driftavbrott uppdatera virtuella datorer en i taget eller grupper (till exempel en feldomän i taget) i stället för på en gång.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-106">Updating without downtime means updating virtual machines one at a time or in groups (such as one fault domain at a time) rather than all at once.</span></span> <span data-ttu-id="a2fe2-107">På så sätt, kan alla virtuella datorer som inte uppgraderas att köra.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-107">By doing so, any virtual machines that are not being upgraded can keep running.</span></span>

<span data-ttu-id="a2fe2-108">För att undvika tvetydighet vi skilja fyra typer av OS-uppdateringar som du kanske vill utföra:</span><span class="sxs-lookup"><span data-stu-id="a2fe2-108">To avoid ambiguity, let’s distinguish four types of OS update you might want to perform:</span></span>

* <span data-ttu-id="a2fe2-109">Ändra version eller SKU av en plattformsavbildning.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-109">Changing the version or SKU of a platform image.</span></span> <span data-ttu-id="a2fe2-110">Till exempel ändra Ubuntu 14.04.2-LTS version från 14.04.201506100 till 14.04.201507060, eller ändra Ubuntu 15.10/senaste SKU till 16.04.0-LTS/latest.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-110">For example, changing Ubuntu 14.04.2-LTS version from 14.04.201506100 to 14.04.201507060, or changing the Ubuntu 15.10/latest SKU to 16.04.0-LTS/latest.</span></span> <span data-ttu-id="a2fe2-111">Det här scenariot beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-111">This scenario is covered in this article.</span></span>
* <span data-ttu-id="a2fe2-112">Ändra den URI som pekar på en ny version av en anpassad avbildning som du skapat (**egenskaper** > **virtualMachineProfile** > **storageProfile**  >  **osDisk** > **bild** > **uri**).</span><span class="sxs-lookup"><span data-stu-id="a2fe2-112">Changing the URI that points to a new version of a custom image you built (**properties** > **virtualMachineProfile** > **storageProfile** > **osDisk** > **image** > **uri**).</span></span> <span data-ttu-id="a2fe2-113">Det här scenariot beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-113">This scenario is covered in this article.</span></span>
* <span data-ttu-id="a2fe2-114">Ändra bildreferens för skaluppsättning som har skapats med hjälp av Azure hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-114">Changing the image reference of a scale set that was created using Azure Managed Disks.</span></span>
* <span data-ttu-id="a2fe2-115">Korrigering av operativsystem från en virtuell dator (exempel på detta är hur du installerar en säkerhetskorrigering och kör Windows Update).</span><span class="sxs-lookup"><span data-stu-id="a2fe2-115">Patching the OS from within a virtual machine (examples of this include installing a security patch and running Windows Update).</span></span> <span data-ttu-id="a2fe2-116">Det här scenariot stöds, men som inte omfattas i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-116">This scenario is supported but not covered in this article.</span></span>

<span data-ttu-id="a2fe2-117">Skalningsuppsättningar i virtuella datorer som distribueras som en del av en [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) kluster beskrivs inte här.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-117">Virtual machine scale sets that are deployed as part of an [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) cluster are not covered here.</span></span> <span data-ttu-id="a2fe2-118">Se [korrigering Windows OS i Service Fabric-kluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) mer information om Service Fabric-korrigering.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-118">See [Patch Windows OS in your Service Fabric cluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) for more information about patching Service Fabric.</span></span>

<span data-ttu-id="a2fe2-119">Grundläggande sekvensen för att ändra OS-version/SKU av en plattformsavbildning eller URI för en anpassad avbildning ser ut som följer:</span><span class="sxs-lookup"><span data-stu-id="a2fe2-119">The basic sequence for changing the OS version/SKU of a platform image or the URI of a custom image looks as follows:</span></span>

1. <span data-ttu-id="a2fe2-120">Hämta den virtuella datorn skala modellen.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-120">Get the virtual machine scale set model.</span></span>
2. <span data-ttu-id="a2fe2-121">Ändra version SKU, bildreferens eller URI-värdet i modellen.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-121">Change the version, SKU, image reference, or URI value in the model.</span></span>
3. <span data-ttu-id="a2fe2-122">Uppdatera modellen.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-122">Update the model.</span></span>
4. <span data-ttu-id="a2fe2-123">Gör en *manualUpgrade* anropa för de virtuella datorerna i skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-123">Do a *manualUpgrade* call on the virtual machines in the scale set.</span></span> <span data-ttu-id="a2fe2-124">Det här steget gäller endast om *upgradePolicy* är inställd på **manuell** i din skaluppsättningen.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-124">This step is only relevant if *upgradePolicy* is set to **Manual** in your scale set.</span></span> <span data-ttu-id="a2fe2-125">Om den är inställd på **automatisk**, alla virtuella datorer uppgraderas samtidigt, vilket orsakar driftstopp.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-125">If it is set to **Automatic**, all the virtual machines are upgraded at once, thus causing downtime.</span></span>

<span data-ttu-id="a2fe2-126">Med den här informationen i åtanke nu ska vi se hur du kan uppdatera versionen av en skala som angetts i PowerShell och med hjälp av REST API.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-126">With this information in mind, let’s see how you could update the version of a scale set in PowerShell, and by using the REST API.</span></span> <span data-ttu-id="a2fe2-127">De här exemplen täcker i fallet med en plattformsavbildning, men den här artikeln innehåller tillräckligt med information att anpassa den här processen för att en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-127">These examples cover the case of a platform image, but this article provides enough information for you to adapt this process to a custom image.</span></span>

## <a name="powershell"></a><span data-ttu-id="a2fe2-128">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a2fe2-128">PowerShell</span></span>
<span data-ttu-id="a2fe2-129">Det här exemplet uppdaterar en Windows virtuella skaluppsättningen (skapar till den nya versionen 4.0.20160229.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-129">This example updates a Windows virtual machine scale set (creating to the new version 4.0.20160229.</span></span> <span data-ttu-id="a2fe2-130">När du har uppdaterat modellen har en uppdatering för en virtuell datorinstans i taget.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-130">After updating the model, it does an update one virtual machine instance at a time.</span></span>

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

<span data-ttu-id="a2fe2-131">Om du uppdaterar URI för en anpassad avbildning istället för att ändra en plattform Avbildningsversion Ersätt ”ange den nya versionen” rad med ett kommando som kommer att uppdatera Källavbildningen URI.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-131">If you are updating the URI for a custom image instead of changing a platform image version, replace the “set the new version” line with a command that will update the source image URI.</span></span> <span data-ttu-id="a2fe2-132">Till exempel om skaluppsättning skapades utan att använda Azure hanterade diskar, uppdateringen skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="a2fe2-132">For example, if the scale set was created without using Azure Managed Disks, the update would look like this:</span></span>

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```

<span data-ttu-id="a2fe2-133">Om en anpassad avbildning baserad skaluppsättning har skapats med hjälp av Azure hanterade diskar och sedan bildreferensen skulle uppdateras.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-133">If a custom image based scale set was created using Azure Managed Disks, then the image reference would be updated.</span></span> <span data-ttu-id="a2fe2-134">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a2fe2-134">For example:</span></span>

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.id = $newImageReference
```

## <a name="the-rest-api"></a><span data-ttu-id="a2fe2-135">REST API</span><span class="sxs-lookup"><span data-stu-id="a2fe2-135">The REST API</span></span>
<span data-ttu-id="a2fe2-136">Här är några av Python-exempel som använder Azure REST API för att distribuera en uppdatering för OS-version.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-136">Here are a couple of Python examples that use the Azure REST API to roll out an OS version update.</span></span> <span data-ttu-id="a2fe2-137">Både använda förenklade [azurerm](https://pypi.python.org/pypi/azurerm) bibliotek med Azure REST API-omslutning funktionerna för att utföra GET på skalan ange modell, följt av en PUT med en uppdaterad modell.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-137">Both use the lightweight [azurerm](https://pypi.python.org/pypi/azurerm) library of Azure REST API wrapper functions to do a GET on the scale set model, followed by a PUT with an updated model.</span></span> <span data-ttu-id="a2fe2-138">De också titta på virtuella instanser vyer för att identifiera de virtuella datorerna genom att uppdatera domänen.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-138">They also look at virtual machine instances views to identify the virtual machines by update domain.</span></span>

### <a name="vmssupgrade"></a><span data-ttu-id="a2fe2-139">Vmssupgrade</span><span class="sxs-lookup"><span data-stu-id="a2fe2-139">Vmssupgrade</span></span>
 <span data-ttu-id="a2fe2-140">[Vmssupgrade](https://github.com/gbowerman/vmsstools) är Python-skriptet som används för att distribuera en OS-uppgradering till en aktiv virtuell dator skala ange en uppdateringsdomän i taget.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-140">[Vmssupgrade](https://github.com/gbowerman/vmsstools) is a Python script that's used to roll out an OS upgrade to a running virtual machine scale set one update domain at a time.</span></span>

![Vmssupgrade skript för att välja virtuella datorer eller en uppdateringsdomän](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

<span data-ttu-id="a2fe2-142">Det här skriptet kan du välja specifika virtuella datorer för att uppdatera eller ange en uppdateringsdomän.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-142">This script lets you choose specific virtual machines to update or specify an update domain.</span></span> <span data-ttu-id="a2fe2-143">Det stöder ändra en plattform Avbildningsversion eller ändra URI för en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-143">It supports changing a platform image version or changing the URI of a custom image.</span></span>

### <a name="vmsseditor"></a><span data-ttu-id="a2fe2-144">Vmsseditor</span><span class="sxs-lookup"><span data-stu-id="a2fe2-144">Vmsseditor</span></span>
<span data-ttu-id="a2fe2-145">[Vmsseditor](https://github.com/gbowerman/vmssdashboard) är en generell redigerare för skalningsuppsättningar i virtuella datorer som visar virtuella datorn status som ett heatmap där en rad representerar en uppdateringsdomän.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-145">[Vmsseditor](https://github.com/gbowerman/vmssdashboard) is a general-purpose editor for virtual machine scale sets that shows virtual machine status as a heatmap where one row represents one update domain.</span></span> <span data-ttu-id="a2fe2-146">Bland annat kan du uppdatera modellen för en skalningsuppsättning med en ny version, en SKU eller en anpassad avbildning URI och välj sedan feldomäner att uppgradera.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-146">Among other things, you can update the model for a scale set with a new version, SKU, or custom image URI, and then pick fault domains to upgrade.</span></span> <span data-ttu-id="a2fe2-147">När du gör det uppgraderas alla virtuella datorer i domänen uppdateringen till den nya modellen.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-147">When you do so, all the virtual machines in that update domain are upgraded to the new model.</span></span> <span data-ttu-id="a2fe2-148">Alternativt kan du göra en löpande uppgradering baserat på batchstorlek önskat.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-148">Alternatively, you can do a rolling upgrade based on the batch size of your choice.</span></span>  

<span data-ttu-id="a2fe2-149">Följande skärmbild visar en modell av en skala som angetts för Ubuntu 14.04-2LTS version 14.04.201507060.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-149">The following screenshot shows a model of a scale set for Ubuntu 14.04-2LTS version 14.04.201507060.</span></span> <span data-ttu-id="a2fe2-150">Många fler alternativ har lagts till det här verktyget eftersom den här skärmbilden har tagits.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-150">Many more options have been added to this tool since this screenshot was taken.</span></span>

![Vmsseditor modell för en skala som angetts för Ubuntu 14.04-2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

<span data-ttu-id="a2fe2-152">När du klickar på **uppgradera** och sedan **Hämta detaljer**, virtuella datorer i UD 0 börja uppdatera.</span><span class="sxs-lookup"><span data-stu-id="a2fe2-152">After you click **Upgrade** and then **Get Details**, virtual machines in UD 0 start to update.</span></span>

![Vmsseditor visar uppdatering pågår](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

