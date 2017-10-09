---
title: aaaCreate anpassade VM-avbildningar med hello Azure PowerShell | Microsoft Docs
description: "Självstudiekurs – skapa en anpassad VM-avbildning med hjälp av hello Azure PowerShell."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 3a759fe1b7e7b72f531399b0f4a99e341713c6a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a><span data-ttu-id="5e647-103">Skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="5e647-103">Create a custom image of an Azure VM using PowerShell</span></span>

<span data-ttu-id="5e647-104">Anpassade avbildningar liknar marketplace-bilder, men du skapa dem själv.</span><span class="sxs-lookup"><span data-stu-id="5e647-104">Custom images are like marketplace images, but you create them yourself.</span></span> <span data-ttu-id="5e647-105">Anpassade avbildningar kan vara används toobootstrap konfigurationer, till exempel förinstalleras program, Programinställningar och andra OS-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="5e647-105">Custom images can be used toobootstrap configurations such as preloading applications, application configurations, and other OS configurations.</span></span> <span data-ttu-id="5e647-106">I den här självstudiekursen skapar du en egen anpassad avbildning av en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="5e647-106">In this tutorial, you create your own custom image of an Azure virtual machine.</span></span> <span data-ttu-id="5e647-107">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="5e647-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5e647-108">Sysprep och generalisera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="5e647-108">Sysprep and generalize VMs</span></span>
> * <span data-ttu-id="5e647-109">Skapa en egen avbildning</span><span class="sxs-lookup"><span data-stu-id="5e647-109">Create a custom image</span></span>
> * <span data-ttu-id="5e647-110">Skapa en virtuell dator från en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="5e647-110">Create a VM from a custom image</span></span>
> * <span data-ttu-id="5e647-111">Visa en lista med alla hello bilder i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="5e647-111">List all hello images in your subscription</span></span>
> * <span data-ttu-id="5e647-112">Ta bort en bild</span><span class="sxs-lookup"><span data-stu-id="5e647-112">Delete an image</span></span>

<span data-ttu-id="5e647-113">Den här kursen kräver hello Azure PowerShell module 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="5e647-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="5e647-114">Kör ` Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="5e647-114">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="5e647-115">Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="5e647-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5e647-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="5e647-116">Before you begin</span></span>

<span data-ttu-id="5e647-117">hello stegen nedan i detalj hur tootake en befintlig virtuell dator och aktivera den till en återanvändbara anpassad avbildning som du kan använda toocreate nya VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="5e647-117">hello steps below detail how tootake an existing VM and turn it into a re-usable custom image that you can use toocreate new VM instances.</span></span>

<span data-ttu-id="5e647-118">toocomplete hello exempel i den här självstudiekursen, måste du ha en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5e647-118">toocomplete hello example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="5e647-119">Om det behövs, detta [skriptexempel](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) kan skapa en åt dig.</span><span class="sxs-lookup"><span data-stu-id="5e647-119">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="5e647-120">När gå igenom självstudiekursen hello ersätter namn hello resursgrupp och VM där det behövs.</span><span class="sxs-lookup"><span data-stu-id="5e647-120">When working through hello tutorial, replace hello resource group and VM names where needed.</span></span>

## <a name="prepare-vm"></a><span data-ttu-id="5e647-121">Förbereda VM</span><span class="sxs-lookup"><span data-stu-id="5e647-121">Prepare VM</span></span>

<span data-ttu-id="5e647-122">toocreate en avbildning av en virtuell dator måste tooprepare hello VM genom att generalisera hello VM, det har frigjorts, och sedan göra hello källa VM som generaliserad i Azure.</span><span class="sxs-lookup"><span data-stu-id="5e647-122">toocreate an image of a virtual machine, you need tooprepare hello VM by generalizing hello VM, deallocating, and then marking hello source VM as generalized in Azure.</span></span>

### <a name="generalize-hello-windows-vm-using-sysprep"></a><span data-ttu-id="5e647-123">Generalisera hello virtuell Windows-dator med hjälp av Sysprep</span><span class="sxs-lookup"><span data-stu-id="5e647-123">Generalize hello Windows VM using Sysprep</span></span>

<span data-ttu-id="5e647-124">Sysprep tar bort alla dina personlig information, bland annat och förbereder hello datorn toobe används som en bild.</span><span class="sxs-lookup"><span data-stu-id="5e647-124">Sysprep removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="5e647-125">Mer information om Sysprep finns [hur tooUse Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="5e647-125">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>


1. <span data-ttu-id="5e647-126">Ansluta toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5e647-126">Connect toohello virtual machine.</span></span>
2. <span data-ttu-id="5e647-127">Öppna hello Kommandotolken som administratör.</span><span class="sxs-lookup"><span data-stu-id="5e647-127">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="5e647-128">Ändra hello katalogen för*%windir%\system32\sysprep*, och kör sedan *sysprep.exe*.</span><span class="sxs-lookup"><span data-stu-id="5e647-128">Change hello directory too*%windir%\system32\sysprep*, and then run *sysprep.exe*.</span></span>
3. <span data-ttu-id="5e647-129">I hello **systemförberedelseverktyget** dialogrutan *ange System Out of Box Experience (OOBE)*, och se till att hello *Generalize* är markerad.</span><span class="sxs-lookup"><span data-stu-id="5e647-129">In hello **System Preparation Tool** dialog box, select *Enter System Out-of-Box Experience (OOBE)*, and make sure that hello *Generalize* check box is selected.</span></span>
4. <span data-ttu-id="5e647-130">I **avstängningsalternativ**väljer *avstängning* och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5e647-130">In **Shutdown Options**, select *Shutdown* and then click **OK**.</span></span>
5. <span data-ttu-id="5e647-131">När Sysprep är klar stänger hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5e647-131">When Sysprep completes, it shuts down hello virtual machine.</span></span> <span data-ttu-id="5e647-132">**Starta inte om hello VM**.</span><span class="sxs-lookup"><span data-stu-id="5e647-132">**Do not restart hello VM**.</span></span>

### <a name="deallocate-and-mark-hello-vm-as-generalized"></a><span data-ttu-id="5e647-133">Frigöra och markera hello VM som generaliserad</span><span class="sxs-lookup"><span data-stu-id="5e647-133">Deallocate and mark hello VM as generalized</span></span>

<span data-ttu-id="5e647-134">toocreate en avbildning måste hello VM toobe frigjorts och har markerats som generaliserad i Azure.</span><span class="sxs-lookup"><span data-stu-id="5e647-134">toocreate an image, hello VM needs toobe deallocated and marked as generalized in Azure.</span></span>

<span data-ttu-id="5e647-135">Frigjord hello VM med hjälp av [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="5e647-135">Deallocated hello VM using [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Force
```

<span data-ttu-id="5e647-136">Ange hello hello virtuella datorns status för för`-Generalized` med [Set AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="5e647-136">Set hello status of hello virtual machine too`-Generalized` using [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm).</span></span> 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Generalized
```


## <a name="create-hello-image"></a><span data-ttu-id="5e647-137">Skapa hello avbildning</span><span class="sxs-lookup"><span data-stu-id="5e647-137">Create hello image</span></span>

<span data-ttu-id="5e647-138">Nu kan du skapa en avbildning av hello VM med hjälp av [ny AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) och [ny AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage).</span><span class="sxs-lookup"><span data-stu-id="5e647-138">Now you can create an image of hello VM by using [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) and [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage).</span></span> <span data-ttu-id="5e647-139">hello följande exempel skapas en bild med namnet *myImage* från en virtuell dator med namnet *myVM*.</span><span class="sxs-lookup"><span data-stu-id="5e647-139">hello following example creates an image named *myImage* from a VM named *myVM*.</span></span>

<span data-ttu-id="5e647-140">Hämta hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5e647-140">Get hello virtual machine.</span></span> 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroupImages
```

<span data-ttu-id="5e647-141">Skapa hello image-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="5e647-141">Create hello image configuration.</span></span>

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

<span data-ttu-id="5e647-142">Skapa hello avbildning.</span><span class="sxs-lookup"><span data-stu-id="5e647-142">Create hello image.</span></span>

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroupImages
``` 

 
## <a name="create-vms-from-hello-image"></a><span data-ttu-id="5e647-143">Skapa virtuella datorer från hello-bild</span><span class="sxs-lookup"><span data-stu-id="5e647-143">Create VMs from hello image</span></span>

<span data-ttu-id="5e647-144">Nu när du har skapat en avbildning kan du kan skapa en eller flera nya virtuella datorer från hello avbildning.</span><span class="sxs-lookup"><span data-stu-id="5e647-144">Now that you have an image, you can create one or more new VMs from hello image.</span></span> <span data-ttu-id="5e647-145">Skapa en virtuell dator från en anpassad avbildning är mycket lik toocreating en virtuell dator med hjälp av en Marketplace-avbildning.</span><span class="sxs-lookup"><span data-stu-id="5e647-145">Creating a VM from a custom image is very similar toocreating a VM using a Marketplace image.</span></span> <span data-ttu-id="5e647-146">När du använder en Marketplace-avbildning har tooinformation om hello-avbildning, image-providern, erbjudande, SKU och version.</span><span class="sxs-lookup"><span data-stu-id="5e647-146">When you use a Marketplace image, you have tooinformation about hello image, image provider, offer, SKU and version.</span></span> <span data-ttu-id="5e647-147">Med en anpassad avbildning måste du bara tooprovide hello-ID för hello egen bildresurs.</span><span class="sxs-lookup"><span data-stu-id="5e647-147">With a custom image, you just need tooprovide hello ID of hello custom image resource.</span></span> 

<span data-ttu-id="5e647-148">I följande skript hello, skapar vi en variabel *$image* toostore information om hello anpassad avbildning med hjälp av [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) och sedan använder vi [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage)och ange hello-ID med hjälp av hello *$image* variabeln vi just skapade.</span><span class="sxs-lookup"><span data-stu-id="5e647-148">In hello following script, we create a variable *$image* toostore information about hello custom image using [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) and then we use [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) and specify hello ID using hello *$image* variable we just created.</span></span> 

<span data-ttu-id="5e647-149">hello skriptet skapar en virtuell dator med namnet *myVMfromImage* från våra anpassad avbildning i en ny resursgrupp med namnet *myResourceGroupFromImage* i hello *västra USA* plats.</span><span class="sxs-lookup"><span data-stu-id="5e647-149">hello script creates a VM named *myVMfromImage* from our custom image in a new resource group named *myResourceGroupFromImage* in hello *West US* location.</span></span>


```powershell
$cred = Get-Credential -Message "Enter a username and password for hello virtual machine."

New-AzureRmResourceGroup -Name myResourceGroupFromImage -Location EastUS

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name "mypublicdns$(Get-Random)" `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4

  $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

  $nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$vmConfig = New-AzureRmVMConfig `
    -VMName myVMfromImage `
    -VMSize Standard_D1 | Set-AzureRmVMOperatingSystem -Windows `
        -ComputerName myComputer `
        -Credential $cred 

# Here is where we create a variable toostore information about hello image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroupImages

# Here is where we specify that we want toocreate hello VM from and image and provide hello image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a><span data-ttu-id="5e647-150">Avbildningshantering</span><span class="sxs-lookup"><span data-stu-id="5e647-150">Image management</span></span> 

<span data-ttu-id="5e647-151">Här följer några exempel på vanliga hanteringsuppgifter för avbildningen och hur toocomplete dem med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5e647-151">Here are some examples of common management image tasks and how toocomplete them using PowerShell.</span></span>

<span data-ttu-id="5e647-152">Visa alla avbildningar efter namn.</span><span class="sxs-lookup"><span data-stu-id="5e647-152">List all images by name.</span></span>

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

<span data-ttu-id="5e647-153">Ta bort en bild.</span><span class="sxs-lookup"><span data-stu-id="5e647-153">Delete an image.</span></span> <span data-ttu-id="5e647-154">Det här exemplet tar bort hello bild med namnet *myOldImage* från hello *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="5e647-154">This example deletes hello image named *myOldImage* from hello *myResourceGroup*.</span></span>

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="5e647-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5e647-155">Next steps</span></span>

<span data-ttu-id="5e647-156">I kursen får skapat du en anpassad VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="5e647-156">In this tutorial, you created a custom VM image.</span></span> <span data-ttu-id="5e647-157">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="5e647-157">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5e647-158">Sysprep och generalisera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="5e647-158">Sysprep and generalize VMs</span></span>
> * <span data-ttu-id="5e647-159">Skapa en egen avbildning</span><span class="sxs-lookup"><span data-stu-id="5e647-159">Create a custom image</span></span>
> * <span data-ttu-id="5e647-160">Skapa en virtuell dator från en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="5e647-160">Create a VM from a custom image</span></span>
> * <span data-ttu-id="5e647-161">Visa en lista med alla hello bilder i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="5e647-161">List all hello images in your subscription</span></span>
> * <span data-ttu-id="5e647-162">Ta bort en bild</span><span class="sxs-lookup"><span data-stu-id="5e647-162">Delete an image</span></span>

<span data-ttu-id="5e647-163">Avancera toohello nästa självstudiekurs toolearn om hur högtillgängliga virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5e647-163">Advance toohello next tutorial toolearn about how highly available virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5e647-164">Skapa virtuella datorer med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="5e647-164">Create highly available VMs</span></span>](tutorial-availability-sets.md)



