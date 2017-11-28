---
title: "Skapa en VM-tillgänglighetsuppsättning i Azure | Microsoft Docs"
description: "Lär dig hur du skapar en tillgänglighetsuppsättning för hanterade eller ohanterade tillgänglighetsuppsättning för dina virtuella datorer med Azure PowerShell eller portalen i Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: e813ade8e105169f21138ed8a8eafda1ff39f42e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="increase-vm-availability-by-creating-an-azure-availability-set"></a><span data-ttu-id="612e4-104">Öka VM tillgänglighet genom att skapa en Azure tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="612e4-104">Increase VM availability by creating an Azure availability set</span></span> 
<span data-ttu-id="612e4-105">Tillgänglighetsuppsättningar ger redundans till ditt program.</span><span class="sxs-lookup"><span data-stu-id="612e4-105">Availability sets provide redundancy to your application.</span></span> <span data-ttu-id="612e4-106">Vi rekommenderar att du grupperar två eller flera virtuella datorer i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="612e4-106">We recommend that you group two or more virtual machines in an availability set.</span></span> <span data-ttu-id="612e4-107">Den här konfigurationen garanterar att under antingen en planerad eller oplanerad underhållshändelse, minst en virtuell dator ska vara tillgänglig och uppfyller 99,95% SLA för Azure.</span><span class="sxs-lookup"><span data-stu-id="612e4-107">This configuration ensures that during either a planned or unplanned maintenance event, at least one virtual machine will be available and meet the 99.95% Azure SLA.</span></span> <span data-ttu-id="612e4-108">Mer information finns i [Serviceavtal för Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="612e4-108">For more information, see the [SLA for Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="612e4-109">Virtuella datorer måste skapas i samma resursgrupp som tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="612e4-109">VMs must be created in the same resource group as the availability set.</span></span>
> 

<span data-ttu-id="612e4-110">Om du vill att den virtuella datorn ska ingå i en tillgänglighetsuppsättning, måste du skapa tillgänglighetsuppsättning först eller när du skapar din första virtuella datorn i uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="612e4-110">If you want your VM to be part of an availability set, you need to create the availability set first or while you are creating your first VM in the set.</span></span> <span data-ttu-id="612e4-111">Om den virtuella datorn kommer att använda hanterade diskar, skapas tillgänglighetsuppsättningen som en hanterad tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="612e4-111">If your VM will be using Managed Disks, the availability set must be created as a managed availability set.</span></span>

<span data-ttu-id="612e4-112">Läs mer om hur du skapar och använder tillgänglighetsuppsättningar [hantera tillgängligheten för virtuella datorer](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="612e4-112">For more information about creating and using availability sets, see [Manage the availability of virtual machines](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a><span data-ttu-id="612e4-113">Använda portalen för att skapa en tillgänglighetsuppsättning innan du skapar dina virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="612e4-113">Use the portal to create an availability set before creating your VMs</span></span>
1. <span data-ttu-id="612e4-114">Klicka på hubbmenyn, **Bläddra** och välj **tillgänglighetsuppsättningar**.</span><span class="sxs-lookup"><span data-stu-id="612e4-114">In the hub menu, click **Browse** and select **Availability sets**.</span></span>
2. <span data-ttu-id="612e4-115">På den **tillgänglighetsuppsättningar bladet**, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="612e4-115">On the **Availability sets blade**, click **Add**.</span></span>
   
    ![Skärmbild som visar knappen Lägg till för att skapa en ny tillgänglighet anges.](./media/create-availability-set/add-availability-set.png)
3. <span data-ttu-id="612e4-117">På den **skapa tillgänglighetsuppsättning** bladet slutföra information för din uppsättning.</span><span class="sxs-lookup"><span data-stu-id="612e4-117">On the **Create availability set** blade, complete the information for your set.</span></span>
   
    ![Skärmbild som visar informationen du behöver ange för att skapa tillgänglighetsuppsättningen.](./media/create-availability-set/create-availability-set.png)
   
   * <span data-ttu-id="612e4-119">**Namnet** -namnet ska vara 1 – 80 tecken som består av siffror, bokstäver, punkter, understreck och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="612e4-119">**Name** - the name should be 1-80 characters made up of numbers, letters, periods, underscores and dashes.</span></span> <span data-ttu-id="612e4-120">Det första tecknet måste vara en bokstav eller siffra.</span><span class="sxs-lookup"><span data-stu-id="612e4-120">The first character must be a letter or number.</span></span> <span data-ttu-id="612e4-121">Det sista tecknet måste vara en bokstav, siffra eller understreck.</span><span class="sxs-lookup"><span data-stu-id="612e4-121">The last character must be a letter, number or underscore.</span></span>
   * <span data-ttu-id="612e4-122">**Fault domäner** -feldomäner definiera grupp med virtuella datorer som delar en gemensam käll- och strömbrytare.</span><span class="sxs-lookup"><span data-stu-id="612e4-122">**Fault domains** - fault domains define the group of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="612e4-123">Som standard de virtuella datorerna separeras över upp till tre feldomäner och kan ändras till mellan 1 och 3.</span><span class="sxs-lookup"><span data-stu-id="612e4-123">By default, the VMs  are separated across up to three fault domains and can be changed to between 1 and 3.</span></span>
   * <span data-ttu-id="612e4-124">**Uppdatera domäner** – fem update domäner tilldelas som standard och det kan anges mellan 1 och 20.</span><span class="sxs-lookup"><span data-stu-id="612e4-124">**Update domains** -  five update domains are assigned by default and this can be set to between 1 and 20.</span></span> <span data-ttu-id="612e4-125">Uppdatera domäner ange grupper av virtuella datorer och underliggande fysiska maskinvara som kan startas på samma gång.</span><span class="sxs-lookup"><span data-stu-id="612e4-125">Update domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at the same time.</span></span> <span data-ttu-id="612e4-126">Till exempel om vi anger fem uppdatera domäner, när fler än fem virtuella datorer är konfigurerade i en enda Tillgänglighetsuppsättning sjätte virtuella datorn placeras i samma uppdateringsdomän som den första virtuella datorn, sjunde i samma UD som den andra virtuella och så vidare.</span><span class="sxs-lookup"><span data-stu-id="612e4-126">For example, if we specify five update domains, when more than five virtual machines are configured within a single Availability Set, the sixth virtual machine will be placed into the same update domain as the first virtual machine, the seventh in the same UD as the second virtual machine, and so on.</span></span> <span data-ttu-id="612e4-127">Ordningen på omstarter kanske inte sekventiell, men kommer att startas om endast en uppdateringsdomän i taget.</span><span class="sxs-lookup"><span data-stu-id="612e4-127">The order of the reboots may not be sequential, but only one update domain will be rebooted at a time.</span></span>
   * <span data-ttu-id="612e4-128">**Prenumerationen** – Välj prenumerationen som ska användas om du har fler än en.</span><span class="sxs-lookup"><span data-stu-id="612e4-128">**Subscription** - select the subscription to use if you have more than one.</span></span>
   * <span data-ttu-id="612e4-129">**Resursgruppen** -Välj en befintlig resursgrupp genom att klicka på pilen och välja en resursgrupp i nedrullningsbara ned.</span><span class="sxs-lookup"><span data-stu-id="612e4-129">**Resource group** - select an existing resource group by clicking the arrow and selecting a resource group from the drop down.</span></span> <span data-ttu-id="612e4-130">Du kan också skapa en ny resursgrupp genom att skriva in ett namn.</span><span class="sxs-lookup"><span data-stu-id="612e4-130">You can also create a new resource group by typing in a name.</span></span> <span data-ttu-id="612e4-131">Namnet kan innehålla något av följande tecken: bokstäver, siffror, punkter, bindestreck, understreck och inledande eller avslutande parentes.</span><span class="sxs-lookup"><span data-stu-id="612e4-131">The name can contain any of the following characters: letters, numbers, periods, dashes, underscores and opening or closing parenthesis.</span></span> <span data-ttu-id="612e4-132">Namnet får inte sluta med en punkt.</span><span class="sxs-lookup"><span data-stu-id="612e4-132">The name cannot end in a period.</span></span> <span data-ttu-id="612e4-133">Alla virtuella datorer i tillgänglighetsgruppen måste skapas i samma resursgrupp som tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="612e4-133">All of the VMs in the availability group need to be created in the same resource group as the availability set.</span></span>
   * <span data-ttu-id="612e4-134">**Plats** -Välj en plats från listrutan.</span><span class="sxs-lookup"><span data-stu-id="612e4-134">**Location** - select a location from the drop-down.</span></span>
   * <span data-ttu-id="612e4-135">**Hanterad** – Välj *Ja* att skapa en hanterad tillgänglighetsuppsättning för att använda med virtuella datorer som använder hanterade diskar för lagring.</span><span class="sxs-lookup"><span data-stu-id="612e4-135">**Managed** - select *Yes* to create a managed availability set to use with VMs that use Managed Disks for storage.</span></span> <span data-ttu-id="612e4-136">Välj **nr** om de virtuella datorerna som ska ingå i uppsättningen använder ohanterade diskar i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="612e4-136">Select **No** if the VMs that will be in the set use unmanaged disks in a storage account.</span></span>
   
4. <span data-ttu-id="612e4-137">När du är klar att lägga till information, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="612e4-137">When you are done entering the information, click **Create**.</span></span> 

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a><span data-ttu-id="612e4-138">Använda portalen för att skapa en virtuell dator och en tillgänglighetsuppsättning på samma gång</span><span class="sxs-lookup"><span data-stu-id="612e4-138">Use the portal to create a virtual machine and an availability set at the same time</span></span>
<span data-ttu-id="612e4-139">Du kan också skapa en ny tillgänglighetsuppsättning för den virtuella datorn när du skapar den första virtuella datorn i uppsättningen om du skapar en ny virtuell dator med hjälp av portalen.</span><span class="sxs-lookup"><span data-stu-id="612e4-139">If you are creating a new VM using the portal, you can also create a new availability set for the VM while you create the first VM in the set.</span></span> <span data-ttu-id="612e4-140">Om du väljer att använda hanterade diskar för den virtuella datorn skapas en hanterad tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="612e4-140">If you choose to use Managed Disks for your VM, a managed availability set will be created.</span></span>

![Skärmbild som visar processen för att skapa en ny tillgänglighetsuppsättning när du skapar den virtuella datorn.](./media/create-availability-set/new-vm-avail-set.png)

## <a name="add-a-new-vm-to-an-existing-availability-set-in-the-portal"></a><span data-ttu-id="612e4-142">Lägg till en ny virtuell dator i en befintlig tillgänglighetsuppsättning i portalen</span><span class="sxs-lookup"><span data-stu-id="612e4-142">Add a new VM to an existing availability set in the portal</span></span>
<span data-ttu-id="612e4-143">Se till att du skapar den i samma för varje ytterligare VM som du skapar som ska tillhöra i uppsättningen **resursgruppen** och välj sedan den befintliga tillgänglighetsuppsättning i steg3.</span><span class="sxs-lookup"><span data-stu-id="612e4-143">For each additional VM that you create that should belong in the set, make sure that you create it in the same **resource group** and then select the existing availability set in Step 3.</span></span> 

![Skärmbild som visar hur du väljer en befintlig tillgänglighetsuppsättning för den virtuella datorn.](./media/create-availability-set/add-vm-to-set.png)

## <a name="use-powershell-to-create-an-availability-set"></a><span data-ttu-id="612e4-145">Använd PowerShell för att skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="612e4-145">Use PowerShell to create an availability set</span></span>
<span data-ttu-id="612e4-146">Det här exemplet skapar en tillgänglighetsuppsättning namngivna **myAvailabilitySet** i den **myResourceGroup** resursgrupp i den **västra USA** plats.</span><span class="sxs-lookup"><span data-stu-id="612e4-146">This example creates an availability set named **myAvailabilitySet** in the **myResourceGroup** resource group in the **West US** location.</span></span> <span data-ttu-id="612e4-147">Detta måste göras innan du skapar den första virtuella dator som ska ingå i uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="612e4-147">This needs to be done before you create the first VM that will be in the set.</span></span>

<span data-ttu-id="612e4-148">Innan du börjar bör du kontrollera att du har den senaste versionen av AzureRM.Compute PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="612e4-148">Before you begin, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="612e4-149">Kör följande kommando för att installera den.</span><span class="sxs-lookup"><span data-stu-id="612e4-149">Run the following command to install it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="612e4-150">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="612e4-150">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


<span data-ttu-id="612e4-151">Om du använder hanterade diskar för dina virtuella datorer, skriver du:</span><span class="sxs-lookup"><span data-stu-id="612e4-151">If you are using managed disks for your VMs, type:</span></span>

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" -managed
```

<span data-ttu-id="612e4-152">Om du använder egna storage-konton för dina virtuella datorer, skriver du:</span><span class="sxs-lookup"><span data-stu-id="612e4-152">If you are using your own storage accounts for your VMs, type:</span></span>

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" 
```

<span data-ttu-id="612e4-153">Mer information finns i [ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="612e4-153">For more information, see [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="612e4-154">Felsökning</span><span class="sxs-lookup"><span data-stu-id="612e4-154">Troubleshooting</span></span>
* <span data-ttu-id="612e4-155">När du skapar en virtuell dator, om det inte finns i den nedrullningsbara listan i portalen tillgänglighetsuppsättningen som du vill kan du skapade den i en annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="612e4-155">When you create a VM, if the availability set you want isn't in the drop-down list in the portal you may have created it in a different resource group.</span></span> <span data-ttu-id="612e4-156">Om du inte vet resursgruppen för din tillgänglighet ange, gå till hubbmenyn och klicka på Bläddra > tillgänglighetsuppsättningar för att se en lista över dina tillgänglighetsuppsättningar, och resursgrupper som de tillhör.</span><span class="sxs-lookup"><span data-stu-id="612e4-156">If you don't know the resource group for your availability set, go to the hub menu and click Browse > Availability sets to see a list of your availability sets and which resource groups they belong to.</span></span>

## <a name="next-steps"></a><span data-ttu-id="612e4-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="612e4-157">Next steps</span></span>
<span data-ttu-id="612e4-158">Lägga till ytterligare lagringsutrymme till den virtuella datorn genom att lägga till ytterligare [datadisk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="612e4-158">Add additional storage to your VM by adding an additional [data disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

