---
title: "aaaAvailability anger självstudier för virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Läs mer om hello tillgänglighet uppsättningar för virtuella Windows-datorer i Azure."
documentationcenter: 
services: virtual-machines-windows
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 853775c5f126dd815c1933f9d71d2274a75ea661
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-availability-sets"></a><span data-ttu-id="eadde-103">Hur toouse tillgänglighetsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="eadde-103">How toouse availability sets</span></span>

<span data-ttu-id="eadde-104">I kursen får du lära dig hur tooincrease hello tillgänglighet och tillförlitlighet för dina lösningar för virtuell dator på Azure med hjälp av en funktion kallad Tillgänglighetsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="eadde-104">In this tutorial, you will learn how tooincrease hello availability and reliability of your Virtual Machine solutions on Azure using a capability called Availability Sets.</span></span> <span data-ttu-id="eadde-105">Tillgänglighetsuppsättningar se till att hello virtuella datorer som du distribuerar i Azure är fördelade på flera isolerade maskinvara kluster.</span><span class="sxs-lookup"><span data-stu-id="eadde-105">Availability sets ensure that hello VMs you deploy on Azure are distributed across multiple isolated hardware clusters.</span></span> <span data-ttu-id="eadde-106">Detta säkerställer att om ett maskinvaru- eller fel i Azure inträffar, underordnade uppsättning dina virtuella datorer som påverkas och som din lösning ska vara tillgängliga och fungerar ur hello av dina kunder som använder den.</span><span class="sxs-lookup"><span data-stu-id="eadde-106">Doing this ensures that if a hardware or software failure within Azure happens, only a sub-set of your VMs will be impacted and that your overall solution will remain available and operational from hello perspective of your customers using it.</span></span> 

<span data-ttu-id="eadde-107">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="eadde-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eadde-108">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eadde-108">Create an availability set</span></span>
> * <span data-ttu-id="eadde-109">Skapa en virtuell dator i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eadde-109">Create a VM in an availability set</span></span>
> * <span data-ttu-id="eadde-110">Kontrollera tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="eadde-110">Check available VM sizes</span></span>

<span data-ttu-id="eadde-111">Den här kursen kräver hello Azure PowerShell module 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="eadde-111">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="eadde-112">Kör ` Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="eadde-112">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="eadde-113">Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="eadde-113">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="availability-set-overview"></a><span data-ttu-id="eadde-114">Översikt över tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eadde-114">Availability set overview</span></span>

<span data-ttu-id="eadde-115">En Tillgänglighetsuppsättning är en logisk gruppering funktion som du kan använda i Azure tooensure att hello VM resurser som du placerar i den isoleras från varandra när de distribueras i ett Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="eadde-115">An Availability Set is a logical grouping capability that you can use in Azure tooensure that hello VM resources you place within it are isolated from each other when they are deployed within an Azure datacenter.</span></span> <span data-ttu-id="eadde-116">Azure garanterar att hello virtuella datorer du placera inom en Tillgänglighetsuppsättning körs över flera fysiska servrar, beräkna rack, lagringsenheter och nätverksväxlar.</span><span class="sxs-lookup"><span data-stu-id="eadde-116">Azure ensures that hello VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches.</span></span> <span data-ttu-id="eadde-117">Detta säkerställer att hello för händelsen maskinvaru- eller programvarufel Azure, påverkas endast en delmängd av dina virtuella datorer, och tillämpningsprogrammet övergripande förblir upp och fortsätta toobe tillgängliga tooyour kunder.</span><span class="sxs-lookup"><span data-stu-id="eadde-117">This ensures that in hello event of a hardware or Azure software failure, only a subset of your VMs will be impacted, and your overall application will stay up and continue toobe available tooyour customers.</span></span> <span data-ttu-id="eadde-118">Med hjälp av Tillgänglighetsuppsättningar är en viktig funktion tooleverage om du vill toobuild tillförlitliga molnlösningar.</span><span class="sxs-lookup"><span data-stu-id="eadde-118">Using Availability Sets is an essential capability tooleverage when you want toobuild reliable cloud solutions.</span></span>

<span data-ttu-id="eadde-119">Nu ska vi titta en typisk VM-baserad lösning där du kan ha 4 frontend-webbservrar och använda 2 backend-VMs som värd för en databas.</span><span class="sxs-lookup"><span data-stu-id="eadde-119">Let’s consider a typical VM-based solution where you might have 4 front-end web servers and use 2 back-end VMs that host a database.</span></span> <span data-ttu-id="eadde-120">Med Azure, du kan toodefine två tillgänglighetsuppsättningar innan du distribuerar dina virtuella datorer: en tillgänglighetsuppsättning för hello ”web”-nivå och en tillgänglighetsuppsättning för hello ”databas” nivå.</span><span class="sxs-lookup"><span data-stu-id="eadde-120">With Azure, you’d want toodefine two availability sets before you deploy your VMs: one availability set for hello “web” tier and one availability set for hello “database” tier.</span></span> <span data-ttu-id="eadde-121">När du skapar en ny virtuell dator kan du ange hello tillgänglighet som en parameter toohello az virtuell dator skapar kommando och Azure automatiskt garanterar att hello virtuella datorer som du skapar inom hello tillgänglig separat uppsättning över flera fysiska maskinvaruresurser.</span><span class="sxs-lookup"><span data-stu-id="eadde-121">When you create a new VM you can then specify hello availability set as a parameter toohello az vm create command, and Azure will automatically ensure that hello VMs you create within hello available set are isolated across multiple physical hardware resources.</span></span> <span data-ttu-id="eadde-122">Det innebär att du vet att hello om hello fysiska maskinvara som din webbserver eller databasen Server virtuella datorer körs på ett problem har uppstått, andra instanser av webbservern och databasen virtuella datorer fortsätter att köras bra eftersom de finns på olika typer av maskinvara.</span><span class="sxs-lookup"><span data-stu-id="eadde-122">This means that if hello physical hardware that one of your Web Server or Database Server VMs is running on has a problem, you know that hello other instances of your Web Server and Database VMs will remain running fine because they are on different hardware.</span></span>

<span data-ttu-id="eadde-123">Du bör alltid använda Tillgänglighetsuppsättningar när du vill toodeploy tillförlitliga VM baserade lösningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="eadde-123">You should always use Availability Sets when you want toodeploy reliable VM based solutions within Azure.</span></span>

## <a name="create-an-availability-set"></a><span data-ttu-id="eadde-124">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eadde-124">Create an availability set</span></span>

<span data-ttu-id="eadde-125">Du kan skapa en tillgänglighetsuppsättning med [ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="eadde-125">You can create an availability set using [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="eadde-126">I det här exemplet vi ange båda hello antalet domäner för uppdatering och fel på *2* för hello tillgänglighetsuppsättning namngivna *myAvailabilitySet* i hello *myResourceGroupAvailability*resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="eadde-126">In this example, we set both hello number of update and fault domains at *2* for hello availability set named *myAvailabilitySet* in hello *myResourceGroupAvailability* resource group.</span></span>

<span data-ttu-id="eadde-127">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="eadde-127">Create a resource group.</span></span>

```powershell
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```


```powershell
New-AzureRmAvailabilitySet `
   -Location EastUS `
   -Name myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability `
   -Managed `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a><span data-ttu-id="eadde-128">Skapa virtuella datorer i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eadde-128">Create VMs inside an availability set</span></span>

<span data-ttu-id="eadde-129">Virtuella datorer måste toobe skapas i hello tillgänglighet set toomake till korrekt distribueras de över hello maskinvara.</span><span class="sxs-lookup"><span data-stu-id="eadde-129">VMs need toobe created within hello availability set toomake sure they are correctly distributed across hello hardware.</span></span> <span data-ttu-id="eadde-130">Du kan inte lägga till en befintlig virtuell dator tooan tillgänglighetsuppsättning när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="eadde-130">You can't add an existing VM tooan availability set after it is created.</span></span> 

<span data-ttu-id="eadde-131">hello maskinvara på en plats är uppdelat i toomultiple uppdatering domäner och feldomäner.</span><span class="sxs-lookup"><span data-stu-id="eadde-131">hello hardware in a location is divided in toomultiple update domains and fault domains.</span></span> <span data-ttu-id="eadde-132">En **uppdateringsdomän** är en grupp virtuella datorer och underliggande fysiska maskinvara som kan startas på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="eadde-132">An **update domain** is a group of VMs and underlying physical hardware that can be rebooted at hello same time.</span></span> <span data-ttu-id="eadde-133">Virtuella datorer i hello samma **feldomän** delar vanliga storage samt en gemensam käll- och strömbrytare.</span><span class="sxs-lookup"><span data-stu-id="eadde-133">VMs in hello same **fault domain** share common storage as well as a common power source and network switch.</span></span> 

<span data-ttu-id="eadde-134">När du skapar en virtuell dator med hjälp av konfiguration av [ny AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) du ange hello tillgänglighet anges med hello `-AvailabilitySetId` parametern toospecify hello-ID för hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="eadde-134">When you create a VM using configuration using [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) you specify hello availability set using hello `-AvailabilitySetId` parameter toospecify hello ID of hello availability set.</span></span>

<span data-ttu-id="eadde-135">Skapa 2 virtuella datorer med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) i hello tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="eadde-135">Create 2 VMs with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) in hello availability set.</span></span>

```powershell
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for hello virtual machine."

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAvailability `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

for ($i=1; $i -le 2; $i++)
{
   $pip = New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name "mypublicdns$(Get-Random)" `
        -AllocationMethod Static `
        -IdleTimeoutInMinutes 4

   $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
        -Name myNetworkSecurityGroupRuleRDP$i `
        -Protocol Tcp `
        -Direction Inbound `
        -Priority 1000 `
        -SourceAddressPrefix * `
        -SourcePortRange * `
        -DestinationAddressPrefix * `
        -DestinationPortRange 3389 `
        -Access Allow

   $nsg = New-AzureRmNetworkSecurityGroup `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name myNetworkSecurityGroup$i `
        -SecurityRules $nsgRuleRDP

   $nic = New-AzureRmNetworkInterface `
        -Name myNic$i `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -SubnetId $vnet.Subnets[0].Id `
        -PublicIpAddressId $pip.Id `
        -NetworkSecurityGroupId $nsg.Id

   # Here is where we specify hello availability set
   $vm = New-AzureRmVMConfig `
        -VMName myVM$i `
        -VMSize Standard_D1 `
        -AvailabilitySetId $availabilitySet.Id

   $vm = Set-AzureRmVMOperatingSystem `
        -VM $vm `
        -Windows -ComputerName myVM$i `   
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage `
        -VM $vm `
        -PublisherName MicrosoftWindowsServer `
        -Offer WindowsServer `
        -Skus 2016-Datacenter `
        -Version latest
   $vm = Set-AzureRmVMOSDisk `
        -VM $vm `
        -Name myOsDisk$i `
        -DiskSizeInGB 128 `
        -CreateOption FromImage `
        -Caching ReadWrite
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -VM $vm
}

```

<span data-ttu-id="eadde-136">Det tar några minuter toocreate och konfigurera både virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="eadde-136">It takes a few minutes toocreate and configure both VMs.</span></span> <span data-ttu-id="eadde-137">När du är klar måste 2 virtuella datorer distribuerade i hello underliggande maskinvara.</span><span class="sxs-lookup"><span data-stu-id="eadde-137">When finished, you will have 2 virtual machines distributed across hello underlying hardware.</span></span> 

## <a name="check-for-available-vm-sizes"></a><span data-ttu-id="eadde-138">Sök efter tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="eadde-138">Check for available VM sizes</span></span> 

<span data-ttu-id="eadde-139">Du kan lägga till flera virtuella datorer toohello tillgänglighetsuppsättning senare, men du måste tooknow vilka VM-storlekar är tillgängliga på hello maskinvara.</span><span class="sxs-lookup"><span data-stu-id="eadde-139">You can add more VMs toohello availability set later, but you need tooknow what VM sizes are available on hello hardware.</span></span> <span data-ttu-id="eadde-140">Använd [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) toolist alla hello tillgängliga storlekar på hello maskinvara kluster för hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="eadde-140">Use [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) toolist all hello available sizes on hello hardware cluster for hello availability set.</span></span>

```powershell
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="next-steps"></a><span data-ttu-id="eadde-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eadde-141">Next steps</span></span>

<span data-ttu-id="eadde-142">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="eadde-142">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eadde-143">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eadde-143">Create an availability set</span></span>
> * <span data-ttu-id="eadde-144">Skapa en virtuell dator i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eadde-144">Create a VM in an availability set</span></span>
> * <span data-ttu-id="eadde-145">Kontrollera tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="eadde-145">Check available VM sizes</span></span>

<span data-ttu-id="eadde-146">Avancera toohello nästa självstudiekurs toolearn om skalningsuppsättningar i virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="eadde-146">Advance toohello next tutorial toolearn about virtual machine scale sets.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="eadde-147">Skapa en VM-skalningsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eadde-147">Create a VM scale set</span></span>](tutorial-create-vmss.md)


