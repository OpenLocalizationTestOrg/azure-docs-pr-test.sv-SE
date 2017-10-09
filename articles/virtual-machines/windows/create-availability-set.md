---
title: "aaaCreate en VM-tillgänglighet som i Azure | Microsoft Docs"
description: "Lär dig hur toocreate en hanterad tillgänglighet ange eller ohanterad tillgänglighet för dina virtuella datorer med Azure PowerShell eller hello portalen i hello Resource Manager-distributionsmodellen."
keywords: "Tillgänglighetsuppsättning"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a3db8659-ace8-4e78-8b8c-1e75c04c042c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eadcdfcd28bb2fa21a4647f207b390c33e022ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="increase-vm-availability-by-creating-an-azure-availability-set"></a><span data-ttu-id="aea10-104">Öka VM tillgänglighet genom att skapa en Azure tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="aea10-104">Increase VM availability by creating an Azure availability set</span></span> 
<span data-ttu-id="aea10-105">Tillgänglighetsuppsättningar ger redundans tooyour program.</span><span class="sxs-lookup"><span data-stu-id="aea10-105">Availability sets provide redundancy tooyour application.</span></span> <span data-ttu-id="aea10-106">Vi rekommenderar att du grupperar två eller flera virtuella datorer i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="aea10-106">We recommend that you group two or more virtual machines in an availability set.</span></span> <span data-ttu-id="aea10-107">Den här konfigurationen garanterar att under antingen en planerad eller oplanerad underhållshändelse, minst en virtuell dator blir tillgänglig och uppfyller hello 99,95% SLA för Azure.</span><span class="sxs-lookup"><span data-stu-id="aea10-107">This configuration ensures that during either a planned or unplanned maintenance event, at least one virtual machine will be available and meet hello 99.95% Azure SLA.</span></span> <span data-ttu-id="aea10-108">Mer information finns i hello [SLA för virtuella datorer](https://azure.microsoft.com/support/legal/sla/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="aea10-108">For more information, see hello [SLA for Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aea10-109">Virtuella datorer måste skapas i hello samma resursgrupp som hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="aea10-109">VMs must be created in hello same resource group as hello availability set.</span></span>
> 

<span data-ttu-id="aea10-110">Om du vill VM toobe-tillhör en tillgänglighetsuppsättning, du behöver toocreate hello tillgänglighet Ange första eller när du skapar din första virtuella datorn i hello uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="aea10-110">If you want your VM toobe part of an availability set, you need toocreate hello availability set first or while you are creating your first VM in hello set.</span></span> <span data-ttu-id="aea10-111">Om den virtuella datorn kommer att använda hanterade diskar, skapas hello tillgänglighetsuppsättning som en hanterad tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="aea10-111">If your VM will be using Managed Disks, hello availability set must be created as a managed availability set.</span></span>

<span data-ttu-id="aea10-112">Läs mer om hur du skapar och använder tillgänglighetsuppsättningar [hantera hello tillgängligheten för virtuella datorer](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="aea10-112">For more information about creating and using availability sets, see [Manage hello availability of virtual machines](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="use-hello-portal-toocreate-an-availability-set-before-creating-your-vms"></a><span data-ttu-id="aea10-113">Använd hello portal toocreate en tillgänglighetsuppsättning innan du skapar dina virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="aea10-113">Use hello portal toocreate an availability set before creating your VMs</span></span>
1. <span data-ttu-id="aea10-114">Hej hubbmenyn, klicka på **Bläddra** och välj **tillgänglighetsuppsättningar**.</span><span class="sxs-lookup"><span data-stu-id="aea10-114">In hello hub menu, click **Browse** and select **Availability sets**.</span></span>
2. <span data-ttu-id="aea10-115">På hello **tillgänglighetsuppsättningar bladet**, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="aea10-115">On hello **Availability sets blade**, click **Add**.</span></span>
   
    ![Skärmbild som visar hello lägga till knappen för att skapa en ny tillgänglighetsuppsättning.](./media/create-availability-set/add-availability-set.png)
3. <span data-ttu-id="aea10-117">På hello **skapa tillgänglighetsuppsättning** bladet, fullständig hello information för din uppsättning.</span><span class="sxs-lookup"><span data-stu-id="aea10-117">On hello **Create availability set** blade, complete hello information for your set.</span></span>
   
    ![Skärmbild som visar hello information du behöver tooenter toocreate hello tillgänglighet anges.](./media/create-availability-set/create-availability-set.png)
   
   * <span data-ttu-id="aea10-119">**Namnet** -hello namnet ska vara 1 – 80 tecken som består av siffror, bokstäver, punkter, understreck och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="aea10-119">**Name** - hello name should be 1-80 characters made up of numbers, letters, periods, underscores and dashes.</span></span> <span data-ttu-id="aea10-120">hello första tecknet måste vara en bokstav eller siffra.</span><span class="sxs-lookup"><span data-stu-id="aea10-120">hello first character must be a letter or number.</span></span> <span data-ttu-id="aea10-121">hello sista tecknet måste vara en bokstav, siffra eller understreck.</span><span class="sxs-lookup"><span data-stu-id="aea10-121">hello last character must be a letter, number or underscore.</span></span>
   * <span data-ttu-id="aea10-122">**Fault domäner** -feldomäner definiera hello grupp virtuella datorer som delar en gemensam käll- och strömbrytare.</span><span class="sxs-lookup"><span data-stu-id="aea10-122">**Fault domains** - fault domains define hello group of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="aea10-123">Som standard hello VMs separeras över in toothree feldomäner och kan vara ändrade toobetween 1 och 3.</span><span class="sxs-lookup"><span data-stu-id="aea10-123">By default, hello VMs  are separated across up toothree fault domains and can be changed toobetween 1 and 3.</span></span>
   * <span data-ttu-id="aea10-124">**Uppdatera domäner** – fem update domäner tilldelas som standard och du kan ange toobetween 1 och 20.</span><span class="sxs-lookup"><span data-stu-id="aea10-124">**Update domains** -  five update domains are assigned by default and this can be set toobetween 1 and 20.</span></span> <span data-ttu-id="aea10-125">Uppdatera domäner ange grupper av virtuella datorer och underliggande fysiska maskinvara som kan startas på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="aea10-125">Update domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at hello same time.</span></span> <span data-ttu-id="aea10-126">Till exempel om vi anger fem update domäner, när fler än fem virtuella datorer är konfigurerade i en enda Tillgänglighetsuppsättning, hello sjätte virtuella datorn placeras i hello samma uppdateringsdomän som hello första virtuella datorn hello sjunde i hello samma UD som hello andra virtuella datorer och så vidare.</span><span class="sxs-lookup"><span data-stu-id="aea10-126">For example, if we specify five update domains, when more than five virtual machines are configured within a single Availability Set, hello sixth virtual machine will be placed into hello same update domain as hello first virtual machine, hello seventh in hello same UD as hello second virtual machine, and so on.</span></span> <span data-ttu-id="aea10-127">hello ordning hello omstarter kanske inte sekventiell, men kommer att startas om endast en uppdateringsdomän i taget.</span><span class="sxs-lookup"><span data-stu-id="aea10-127">hello order of hello reboots may not be sequential, but only one update domain will be rebooted at a time.</span></span>
   * <span data-ttu-id="aea10-128">**Prenumerationen** -Välj hello prenumeration toouse om du har mer än ett.</span><span class="sxs-lookup"><span data-stu-id="aea10-128">**Subscription** - select hello subscription toouse if you have more than one.</span></span>
   * <span data-ttu-id="aea10-129">**Resursgruppen** -Välj en befintlig resursgrupp genom att klicka på pilen hello och välja en resursgrupp hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="aea10-129">**Resource group** - select an existing resource group by clicking hello arrow and selecting a resource group from hello drop down.</span></span> <span data-ttu-id="aea10-130">Du kan också skapa en ny resursgrupp genom att skriva in ett namn.</span><span class="sxs-lookup"><span data-stu-id="aea10-130">You can also create a new resource group by typing in a name.</span></span> <span data-ttu-id="aea10-131">hello kan innehålla något av följande tecken hello: bokstäver, siffror, punkter, bindestreck, understreck och inledande eller avslutande parentes.</span><span class="sxs-lookup"><span data-stu-id="aea10-131">hello name can contain any of hello following characters: letters, numbers, periods, dashes, underscores and opening or closing parenthesis.</span></span> <span data-ttu-id="aea10-132">hello namn får inte sluta med en punkt.</span><span class="sxs-lookup"><span data-stu-id="aea10-132">hello name cannot end in a period.</span></span> <span data-ttu-id="aea10-133">Alla hello virtuella datorer i hello tillgänglighetsgrupp behöver toobe som skapats i hello samma resursgrupp som hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="aea10-133">All of hello VMs in hello availability group need toobe created in hello same resource group as hello availability set.</span></span>
   * <span data-ttu-id="aea10-134">**Plats** -Välj en plats hello i listrutan.</span><span class="sxs-lookup"><span data-stu-id="aea10-134">**Location** - select a location from hello drop-down.</span></span>
   * <span data-ttu-id="aea10-135">**Hanterad** – Välj *Ja* toocreate en hanterad tillgänglighet ange toouse med virtuella datorer som använder hanterade diskar för lagring.</span><span class="sxs-lookup"><span data-stu-id="aea10-135">**Managed** - select *Yes* toocreate a managed availability set toouse with VMs that use Managed Disks for storage.</span></span> <span data-ttu-id="aea10-136">Välj **nr** om hello virtuella datorer som ska finnas i uppsättning hello använda ohanterade diskar i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="aea10-136">Select **No** if hello VMs that will be in hello set use unmanaged disks in a storage account.</span></span>
   
4. <span data-ttu-id="aea10-137">När du är klar att mata in hello information klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="aea10-137">When you are done entering hello information, click **Create**.</span></span> 

## <a name="use-hello-portal-toocreate-a-virtual-machine-and-an-availability-set-at-hello-same-time"></a><span data-ttu-id="aea10-138">Använd hello portal toocreate en virtuell dator och en tillgänglighet ange på hello samma tid</span><span class="sxs-lookup"><span data-stu-id="aea10-138">Use hello portal toocreate a virtual machine and an availability set at hello same time</span></span>
<span data-ttu-id="aea10-139">Om du skapar en ny virtuell dator med hjälp av hello portal, du kan också skapa en ny tillgänglighetsuppsättning för hello VM, när du skapar hello första virtuella datorn i hello uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="aea10-139">If you are creating a new VM using hello portal, you can also create a new availability set for hello VM while you create hello first VM in hello set.</span></span> <span data-ttu-id="aea10-140">Om du väljer toouse hanteras diskarna för den virtuella datorn skapas en hanterad tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="aea10-140">If you choose toouse Managed Disks for your VM, a managed availability set will be created.</span></span>

![Skärmbild som visar hello processen för att skapa en ny tillgänglighetsuppsättning när du skapar hello VM.](./media/create-availability-set/new-vm-avail-set.png)

## <a name="add-a-new-vm-tooan-existing-availability-set-in-hello-portal"></a><span data-ttu-id="aea10-142">Lägg till en ny VM tooan befintliga tillgänglighetsuppsättning i hello-portalen</span><span class="sxs-lookup"><span data-stu-id="aea10-142">Add a new VM tooan existing availability set in hello portal</span></span>
<span data-ttu-id="aea10-143">För varje ytterligare VM som du skapar som bör tillhör hello set, bör du skapa den i hello samma **resursgruppen** och sedan väljer hello befintliga tillgänglighetsuppsättning i steg3.</span><span class="sxs-lookup"><span data-stu-id="aea10-143">For each additional VM that you create that should belong in hello set, make sure that you create it in hello same **resource group** and then select hello existing availability set in Step 3.</span></span> 

![Skärmbild som visar hur tooselect en befintlig tillgänglighetsuppsättning ange toouse för den virtuella datorn.](./media/create-availability-set/add-vm-to-set.png)

## <a name="use-powershell-toocreate-an-availability-set"></a><span data-ttu-id="aea10-145">Använd PowerShell toocreate tillgänglighet ange</span><span class="sxs-lookup"><span data-stu-id="aea10-145">Use PowerShell toocreate an availability set</span></span>
<span data-ttu-id="aea10-146">Det här exemplet skapar en tillgänglighetsuppsättning namngivna **myAvailabilitySet** i hello **myResourceGroup** resursgrupp i hello **västra USA** plats.</span><span class="sxs-lookup"><span data-stu-id="aea10-146">This example creates an availability set named **myAvailabilitySet** in hello **myResourceGroup** resource group in hello **West US** location.</span></span> <span data-ttu-id="aea10-147">Detta måste toobe klar innan du skapar hello första virtuella dator som ska ingå i hello uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="aea10-147">This needs toobe done before you create hello first VM that will be in hello set.</span></span>

<span data-ttu-id="aea10-148">Innan du börjar bör du kontrollera att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="aea10-148">Before you begin, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="aea10-149">Kör följande kommando tooinstall hello den.</span><span class="sxs-lookup"><span data-stu-id="aea10-149">Run hello following command tooinstall it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="aea10-150">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="aea10-150">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


<span data-ttu-id="aea10-151">Om du använder hanterade diskar för dina virtuella datorer, skriver du:</span><span class="sxs-lookup"><span data-stu-id="aea10-151">If you are using managed disks for your VMs, type:</span></span>

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" -managed
```

<span data-ttu-id="aea10-152">Om du använder egna storage-konton för dina virtuella datorer, skriver du:</span><span class="sxs-lookup"><span data-stu-id="aea10-152">If you are using your own storage accounts for your VMs, type:</span></span>

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" 
```

<span data-ttu-id="aea10-153">Mer information finns i [ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="aea10-153">For more information, see [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="aea10-154">Felsökning</span><span class="sxs-lookup"><span data-stu-id="aea10-154">Troubleshooting</span></span>
* <span data-ttu-id="aea10-155">När du skapar kanske en virtuell dator, om hello tillgänglighetsuppsättningen som du vill använda inte finns i hello listrutan i hello portal du har skapat den i en annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="aea10-155">When you create a VM, if hello availability set you want isn't in hello drop-down list in hello portal you may have created it in a different resource group.</span></span> <span data-ttu-id="aea10-156">Om du inte vet hello resursgrupp för ditt tillgänglighet ange, gå toohello hubbmenyn och klicka på Bläddra > tillgänglighetsuppsättningar toosee anger en lista över din tillgänglighet och vilka resursgrupper som de tillhör.</span><span class="sxs-lookup"><span data-stu-id="aea10-156">If you don't know hello resource group for your availability set, go toohello hub menu and click Browse > Availability sets toosee a list of your availability sets and which resource groups they belong to.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aea10-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aea10-157">Next steps</span></span>
<span data-ttu-id="aea10-158">Lägga till ytterligare lagringsutrymme tooyour VM genom att lägga till ytterligare [datadisk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="aea10-158">Add additional storage tooyour VM by adding an additional [data disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

