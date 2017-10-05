---
title: "Tillgänglighetsuppsättningar självstudier för virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Läs mer om Tillgänglighetsuppsättningarna för virtuella Windows-datorer i Azure."
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
ms.openlocfilehash: d918362106ef93cf47620e0018d363cd510884b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-availability-sets"></a><span data-ttu-id="ca53d-103">Hur du använder tillgänglighetsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="ca53d-103">How to use availability sets</span></span>

<span data-ttu-id="ca53d-104">I kursen får lära du dig att öka tillgängligheten och tillförlitligheten hos dina virtuella lösningar på Azure med hjälp av en funktion som kallas Tillgänglighetsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="ca53d-104">In this tutorial, you will learn how to increase the availability and reliability of your Virtual Machine solutions on Azure using a capability called Availability Sets.</span></span> <span data-ttu-id="ca53d-105">Tillgänglighetsuppsättningar se till att de virtuella datorerna som du distribuerar i Azure är fördelade på flera isolerade maskinvara kluster.</span><span class="sxs-lookup"><span data-stu-id="ca53d-105">Availability sets ensure that the VMs you deploy on Azure are distributed across multiple isolated hardware clusters.</span></span> <span data-ttu-id="ca53d-106">Detta säkerställer att om ett maskinvaru- eller fel i Azure inträffar, underordnade uppsättning dina virtuella datorer som påverkas och som din lösning ska vara tillgängliga och fungerar för dina kunder som använder den.</span><span class="sxs-lookup"><span data-stu-id="ca53d-106">Doing this ensures that if a hardware or software failure within Azure happens, only a sub-set of your VMs will be impacted and that your overall solution will remain available and operational from the perspective of your customers using it.</span></span> 

<span data-ttu-id="ca53d-107">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="ca53d-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ca53d-108">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="ca53d-108">Create an availability set</span></span>
> * <span data-ttu-id="ca53d-109">Skapa en virtuell dator i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="ca53d-109">Create a VM in an availability set</span></span>
> * <span data-ttu-id="ca53d-110">Kontrollera tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="ca53d-110">Check available VM sizes</span></span>

<span data-ttu-id="ca53d-111">Den här självstudien kräver Azure PowerShell-modul version 3.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ca53d-111">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="ca53d-112">Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="ca53d-112">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="ca53d-113">Om du behöver uppgradera [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="ca53d-113">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="availability-set-overview"></a><span data-ttu-id="ca53d-114">Översikt över tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="ca53d-114">Availability set overview</span></span>

<span data-ttu-id="ca53d-115">En Tillgänglighetsuppsättning är en logisk gruppering funktion som du kan använda i Azure för att säkerställa att VM-resurser som du placerar i den isolerade från varandra när de distribueras i ett Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="ca53d-115">An Availability Set is a logical grouping capability that you can use in Azure to ensure that the VM resources you place within it are isolated from each other when they are deployed within an Azure datacenter.</span></span> <span data-ttu-id="ca53d-116">Azure säkerställer att de virtuella datorerna som du placerar i en Tillgänglighetsuppsättning körs över flera fysiska servrar, beräkna rack, lagringsenheter och nätverksväxlar.</span><span class="sxs-lookup"><span data-stu-id="ca53d-116">Azure ensures that the VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches.</span></span> <span data-ttu-id="ca53d-117">Detta säkerställer att om ett maskinvaru- eller Azure programvarufel påverkas endast en delmängd av dina virtuella datorer, och tillämpningsprogrammet övergripande förblir upp och fortsätter att vara tillgängliga för kunderna.</span><span class="sxs-lookup"><span data-stu-id="ca53d-117">This ensures that in the event of a hardware or Azure software failure, only a subset of your VMs will be impacted, and your overall application will stay up and continue to be available to your customers.</span></span> <span data-ttu-id="ca53d-118">Med hjälp av Tillgänglighetsuppsättningar är en viktig funktion att använda när du vill skapa tillförlitliga molnlösningar.</span><span class="sxs-lookup"><span data-stu-id="ca53d-118">Using Availability Sets is an essential capability to leverage when you want to build reliable cloud solutions.</span></span>

<span data-ttu-id="ca53d-119">Nu ska vi titta en typisk VM-baserad lösning där du kan ha 4 frontend-webbservrar och använda 2 backend-VMs som värd för en databas.</span><span class="sxs-lookup"><span data-stu-id="ca53d-119">Let’s consider a typical VM-based solution where you might have 4 front-end web servers and use 2 back-end VMs that host a database.</span></span> <span data-ttu-id="ca53d-120">Med Azure, skulle du vill definiera två tillgänglighetsuppsättningar innan du distribuerar dina virtuella datorer: en tillgänglighetsuppsättning för skiktet ”web” och en tillgänglighetsuppsättning för skiktet ”databas”.</span><span class="sxs-lookup"><span data-stu-id="ca53d-120">With Azure, you’d want to define two availability sets before you deploy your VMs: one availability set for the “web” tier and one availability set for the “database” tier.</span></span> <span data-ttu-id="ca53d-121">När du skapar en ny virtuell dator kan du ange tillgänglighetsuppsättningen som en parameter till den virtuella datorn az skapar kommando och Azure automatiskt säkerställer att de virtuella datorerna som du skapar i uppsättningen med tillgängliga isoleras över flera fysiska maskinvaruresurser.</span><span class="sxs-lookup"><span data-stu-id="ca53d-121">When you create a new VM you can then specify the availability set as a parameter to the az vm create command, and Azure will automatically ensure that the VMs you create within the available set are isolated across multiple physical hardware resources.</span></span> <span data-ttu-id="ca53d-122">Det innebär att om den fysiska maskinvara som din webbserver eller databasen Server virtuella datorer körs på ett problem har uppstått, vet du att andra instanser av webbservern och databasen virtuella datorer fortsätter att köras bra eftersom de finns på olika typer av maskinvara.</span><span class="sxs-lookup"><span data-stu-id="ca53d-122">This means that if the physical hardware that one of your Web Server or Database Server VMs is running on has a problem, you know that the other instances of your Web Server and Database VMs will remain running fine because they are on different hardware.</span></span>

<span data-ttu-id="ca53d-123">Du bör alltid använda Tillgänglighetsuppsättningar när du vill distribuera tillförlitliga VM baserade lösningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="ca53d-123">You should always use Availability Sets when you want to deploy reliable VM based solutions within Azure.</span></span>

## <a name="create-an-availability-set"></a><span data-ttu-id="ca53d-124">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="ca53d-124">Create an availability set</span></span>

<span data-ttu-id="ca53d-125">Du kan skapa en tillgänglighetsuppsättning med [ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="ca53d-125">You can create an availability set using [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="ca53d-126">I detta exempel vi in både antalet domäner för uppdatering och fel på *2* för tillgänglighetsuppsättning namngivna *myAvailabilitySet* i den *myResourceGroupAvailability* resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ca53d-126">In this example, we set both the number of update and fault domains at *2* for the availability set named *myAvailabilitySet* in the *myResourceGroupAvailability* resource group.</span></span>

<span data-ttu-id="ca53d-127">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ca53d-127">Create a resource group.</span></span>

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

## <a name="create-vms-inside-an-availability-set"></a><span data-ttu-id="ca53d-128">Skapa virtuella datorer i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="ca53d-128">Create VMs inside an availability set</span></span>

<span data-ttu-id="ca53d-129">Virtuella datorer måste skapas inom tillgänglighetsuppsättning för att kontrollera att de distribueras på rätt sätt i maskinvaran.</span><span class="sxs-lookup"><span data-stu-id="ca53d-129">VMs need to be created within the availability set to make sure they are correctly distributed across the hardware.</span></span> <span data-ttu-id="ca53d-130">Du kan inte lägga till en befintlig virtuell dator till en tillgänglighetsuppsättning när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="ca53d-130">You can't add an existing VM to an availability set after it is created.</span></span> 

<span data-ttu-id="ca53d-131">Maskinvaran på en plats är uppdelat i flera domäner för uppdatering och feldomäner.</span><span class="sxs-lookup"><span data-stu-id="ca53d-131">The hardware in a location is divided in to multiple update domains and fault domains.</span></span> <span data-ttu-id="ca53d-132">En **uppdateringsdomän** är en grupp virtuella datorer och underliggande fysiska maskinvara som kan startas på samma gång.</span><span class="sxs-lookup"><span data-stu-id="ca53d-132">An **update domain** is a group of VMs and underlying physical hardware that can be rebooted at the same time.</span></span> <span data-ttu-id="ca53d-133">Virtuella datorer i samma **feldomän** delar vanliga storage samt en gemensam käll- och strömbrytare.</span><span class="sxs-lookup"><span data-stu-id="ca53d-133">VMs in the same **fault domain** share common storage as well as a common power source and network switch.</span></span> 

<span data-ttu-id="ca53d-134">När du skapar en virtuell dator med hjälp av konfiguration av [ny AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) du ange tillgänglighetsuppsättning med hjälp av den `-AvailabilitySetId` parametern för att ange ID för tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="ca53d-134">When you create a VM using configuration using [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) you specify the availability set using the `-AvailabilitySetId` parameter to specify the ID of the availability set.</span></span>

<span data-ttu-id="ca53d-135">Skapa 2 virtuella datorer med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) i tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="ca53d-135">Create 2 VMs with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) in the availability set.</span></span>

```powershell
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

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

   # Here is where we specify the availability set
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

<span data-ttu-id="ca53d-136">Det tar några minuter att skapa och konfigurera både virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ca53d-136">It takes a few minutes to create and configure both VMs.</span></span> <span data-ttu-id="ca53d-137">När du är klar måste 2 virtuella datorer distribuerade i den underliggande maskinvaran.</span><span class="sxs-lookup"><span data-stu-id="ca53d-137">When finished, you will have 2 virtual machines distributed across the underlying hardware.</span></span> 

## <a name="check-for-available-vm-sizes"></a><span data-ttu-id="ca53d-138">Sök efter tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="ca53d-138">Check for available VM sizes</span></span> 

<span data-ttu-id="ca53d-139">Du kan lägga till flera virtuella datorer tillgänglighetsuppsättning senare, men du behöver veta vilka VM-storlekar är tillgängliga på maskinvaran.</span><span class="sxs-lookup"><span data-stu-id="ca53d-139">You can add more VMs to the availability set later, but you need to know what VM sizes are available on the hardware.</span></span> <span data-ttu-id="ca53d-140">Använd [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) att lista alla tillgängliga storlekar på maskinvaran kluster för tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="ca53d-140">Use [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) to list all the available sizes on the hardware cluster for the availability set.</span></span>

```powershell
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="next-steps"></a><span data-ttu-id="ca53d-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ca53d-141">Next steps</span></span>

<span data-ttu-id="ca53d-142">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="ca53d-142">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ca53d-143">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="ca53d-143">Create an availability set</span></span>
> * <span data-ttu-id="ca53d-144">Skapa en virtuell dator i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="ca53d-144">Create a VM in an availability set</span></span>
> * <span data-ttu-id="ca53d-145">Kontrollera tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="ca53d-145">Check available VM sizes</span></span>

<span data-ttu-id="ca53d-146">Gå vidare till nästa kurs vill veta mer om virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ca53d-146">Advance to the next tutorial to learn about virtual machine scale sets.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ca53d-147">Skapa en VM-skalningsuppsättning</span><span class="sxs-lookup"><span data-stu-id="ca53d-147">Create a VM scale set</span></span>](tutorial-create-vmss.md)


