---
title: Skapa anpassade VM-avbildningar med Azure PowerShell | Microsoft Docs
description: "Självstudiekurs – skapa en anpassad VM-avbildning med hjälp av Azure PowerShell."
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
ms.openlocfilehash: 96be2872a902a7d7063bf1dff7b4ca209a5b67c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a><span data-ttu-id="d6e8f-103">Skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="d6e8f-103">Create a custom image of an Azure VM using PowerShell</span></span>

<span data-ttu-id="d6e8f-104">Anpassade avbildningar liknar marketplace-bilder, men du skapa dem själv.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-104">Custom images are like marketplace images, but you create them yourself.</span></span> <span data-ttu-id="d6e8f-105">Anpassade avbildningar kan användas för bootstrap konfigurationer, till exempel förinstalleras program, Programinställningar och andra OS-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-105">Custom images can be used to bootstrap configurations such as preloading applications, application configurations, and other OS configurations.</span></span> <span data-ttu-id="d6e8f-106">I den här självstudiekursen skapar du en egen anpassad avbildning av en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-106">In this tutorial, you create your own custom image of an Azure virtual machine.</span></span> <span data-ttu-id="d6e8f-107">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="d6e8f-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d6e8f-108">Sysprep och generalisera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="d6e8f-108">Sysprep and generalize VMs</span></span>
> * <span data-ttu-id="d6e8f-109">Skapa en egen avbildning</span><span class="sxs-lookup"><span data-stu-id="d6e8f-109">Create a custom image</span></span>
> * <span data-ttu-id="d6e8f-110">Skapa en virtuell dator från en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="d6e8f-110">Create a VM from a custom image</span></span>
> * <span data-ttu-id="d6e8f-111">Alla avbildningar i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="d6e8f-111">List all the images in your subscription</span></span>
> * <span data-ttu-id="d6e8f-112">Ta bort en bild</span><span class="sxs-lookup"><span data-stu-id="d6e8f-112">Delete an image</span></span>

<span data-ttu-id="d6e8f-113">Den här självstudien kräver Azure PowerShell-modul version 3.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-113">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="d6e8f-114">Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-114">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="d6e8f-115">Om du behöver uppgradera [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="d6e8f-115">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="d6e8f-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="d6e8f-116">Before you begin</span></span>

<span data-ttu-id="d6e8f-117">Stegen nedan innehåller information om hur du tar en befintlig virtuell dator och omvandla det till en återanvändbara anpassad avbildning som du kan använda för att skapa nya VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-117">The steps below detail how to take an existing VM and turn it into a re-usable custom image that you can use to create new VM instances.</span></span>

<span data-ttu-id="d6e8f-118">Du måste ha en befintlig virtuell dator för att slutföra exemplet i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-118">To complete the example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="d6e8f-119">Om det behövs, detta [skriptexempel](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) kan skapa en åt dig.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-119">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="d6e8f-120">När gå igenom kursen, ersätter namn resursgrupp och VM där det behövs.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-120">When working through the tutorial, replace the resource group and VM names where needed.</span></span>

## <a name="prepare-vm"></a><span data-ttu-id="d6e8f-121">Förbereda VM</span><span class="sxs-lookup"><span data-stu-id="d6e8f-121">Prepare VM</span></span>

<span data-ttu-id="d6e8f-122">Om du vill skapa en avbildning av en virtuell dator måste du förbereda den virtuella datorn genom att generalisera den virtuella datorn, det har frigjorts och markera källan VM som generaliserad i Azure.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-122">To create an image of a virtual machine, you need to prepare the VM by generalizing the VM, deallocating, and then marking the source VM as generalized in Azure.</span></span>

### <a name="generalize-the-windows-vm-using-sysprep"></a><span data-ttu-id="d6e8f-123">Generalisera Windows VM med hjälp av Sysprep</span><span class="sxs-lookup"><span data-stu-id="d6e8f-123">Generalize the Windows VM using Sysprep</span></span>

<span data-ttu-id="d6e8f-124">Sysprep tar bort alla dina personlig information, bland annat och förbereder datorn som ska användas som en bild.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-124">Sysprep removes all your personal account information, among other things, and prepares the machine to be used as an image.</span></span> <span data-ttu-id="d6e8f-125">Mer information om Sysprep finns [så att använda Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="d6e8f-125">For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>


1. <span data-ttu-id="d6e8f-126">Ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-126">Connect to the virtual machine.</span></span>
2. <span data-ttu-id="d6e8f-127">Öppna Kommandotolken som administratör.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-127">Open the Command Prompt window as an administrator.</span></span> <span data-ttu-id="d6e8f-128">Ändra katalogen till *%windir%\system32\sysprep*, och kör sedan *sysprep.exe*.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-128">Change the directory to *%windir%\system32\sysprep*, and then run *sysprep.exe*.</span></span>
3. <span data-ttu-id="d6e8f-129">I den **systemförberedelseverktyget** dialogrutan *ange System Out of Box Experience (OOBE)*, och se till att den *Generalize* är markerad.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-129">In the **System Preparation Tool** dialog box, select *Enter System Out-of-Box Experience (OOBE)*, and make sure that the *Generalize* check box is selected.</span></span>
4. <span data-ttu-id="d6e8f-130">I **avstängningsalternativ**väljer *avstängning* och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-130">In **Shutdown Options**, select *Shutdown* and then click **OK**.</span></span>
5. <span data-ttu-id="d6e8f-131">När Sysprep har slutförts stängs den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-131">When Sysprep completes, it shuts down the virtual machine.</span></span> <span data-ttu-id="d6e8f-132">**Starta inte om den virtuella datorn**.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-132">**Do not restart the VM**.</span></span>

### <a name="deallocate-and-mark-the-vm-as-generalized"></a><span data-ttu-id="d6e8f-133">Frigöra och markera den virtuella datorn som generaliserad</span><span class="sxs-lookup"><span data-stu-id="d6e8f-133">Deallocate and mark the VM as generalized</span></span>

<span data-ttu-id="d6e8f-134">Om du vill skapa en avbildning måste den virtuella datorn frigjorts och har markerats som generaliserad i Azure.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-134">To create an image, the VM needs to be deallocated and marked as generalized in Azure.</span></span>

<span data-ttu-id="d6e8f-135">Frigöra den virtuella datorn med hjälp av [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="d6e8f-135">Deallocated the VM using [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Force
```

<span data-ttu-id="d6e8f-136">Ange status för den virtuella datorn till `-Generalized` med [Set AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="d6e8f-136">Set the status of the virtual machine to `-Generalized` using [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm).</span></span> 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Generalized
```


## <a name="create-the-image"></a><span data-ttu-id="d6e8f-137">Skapa avbildningen</span><span class="sxs-lookup"><span data-stu-id="d6e8f-137">Create the image</span></span>

<span data-ttu-id="d6e8f-138">Nu kan du skapa en avbildning av den virtuella datorn med hjälp av [ny AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) och [ny AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage).</span><span class="sxs-lookup"><span data-stu-id="d6e8f-138">Now you can create an image of the VM by using [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) and [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage).</span></span> <span data-ttu-id="d6e8f-139">I följande exempel skapas en bild med namnet *myImage* från en virtuell dator med namnet *myVM*.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-139">The following example creates an image named *myImage* from a VM named *myVM*.</span></span>

<span data-ttu-id="d6e8f-140">Hämta den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-140">Get the virtual machine.</span></span> 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroupImages
```

<span data-ttu-id="d6e8f-141">Skapa image-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-141">Create the image configuration.</span></span>

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

<span data-ttu-id="d6e8f-142">Skapa avbildningen.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-142">Create the image.</span></span>

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroupImages
``` 

 
## <a name="create-vms-from-the-image"></a><span data-ttu-id="d6e8f-143">Skapa virtuella datorer från avbildningen</span><span class="sxs-lookup"><span data-stu-id="d6e8f-143">Create VMs from the image</span></span>

<span data-ttu-id="d6e8f-144">Nu när du har skapat en avbildning kan du skapa en eller flera nya virtuella datorer från avbildningen.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-144">Now that you have an image, you can create one or more new VMs from the image.</span></span> <span data-ttu-id="d6e8f-145">Skapa en virtuell dator från en anpassad avbildning påminner mycket om att skapa en virtuell dator med hjälp av en Marketplace-avbildning.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-145">Creating a VM from a custom image is very similar to creating a VM using a Marketplace image.</span></span> <span data-ttu-id="d6e8f-146">När du använder en Marketplace-avbildning, behöver du information om avbildning, image-providern, erbjudande, SKU och version.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-146">When you use a Marketplace image, you have to information about the image, image provider, offer, SKU and version.</span></span> <span data-ttu-id="d6e8f-147">Med en anpassad avbildning behöver du bara ange ID för den anpassade bild resursen.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-147">With a custom image, you just need to provide the ID of the custom image resource.</span></span> 

<span data-ttu-id="d6e8f-148">I skriptet nedan skapar vi en variabel *$image* att lagra information om en anpassad avbildning med hjälp av [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) och sedan använder vi [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) och ange ID med hjälp av den *$image* variabeln vi just skapade.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-148">In the following script, we create a variable *$image* to store information about the custom image using [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) and then we use [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) and specify the ID using the *$image* variable we just created.</span></span> 

<span data-ttu-id="d6e8f-149">Skriptet skapar en virtuell dator med namnet *myVMfromImage* från våra anpassad avbildning i en ny resursgrupp med namnet *myResourceGroupFromImage* i den *västra USA* plats.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-149">The script creates a VM named *myVMfromImage* from our custom image in a new resource group named *myResourceGroupFromImage* in the *West US* location.</span></span>


```powershell
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

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

# Here is where we create a variable to store information about the image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroupImages

# Here is where we specify that we want to create the VM from and image and provide the image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a><span data-ttu-id="d6e8f-150">Avbildningshantering</span><span class="sxs-lookup"><span data-stu-id="d6e8f-150">Image management</span></span> 

<span data-ttu-id="d6e8f-151">Här följer några exempel på vanliga hanteringsuppgifter för avbildning och hur du utför dem med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-151">Here are some examples of common management image tasks and how to complete them using PowerShell.</span></span>

<span data-ttu-id="d6e8f-152">Visa alla avbildningar efter namn.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-152">List all images by name.</span></span>

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

<span data-ttu-id="d6e8f-153">Ta bort en bild.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-153">Delete an image.</span></span> <span data-ttu-id="d6e8f-154">Det här exemplet tar bort det bild med namnet *myOldImage* från den *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-154">This example deletes the image named *myOldImage* from the *myResourceGroup*.</span></span>

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="d6e8f-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6e8f-155">Next steps</span></span>

<span data-ttu-id="d6e8f-156">I kursen får skapat du en anpassad VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-156">In this tutorial, you created a custom VM image.</span></span> <span data-ttu-id="d6e8f-157">Du har lärt dig hur till:</span><span class="sxs-lookup"><span data-stu-id="d6e8f-157">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d6e8f-158">Sysprep och generalisera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="d6e8f-158">Sysprep and generalize VMs</span></span>
> * <span data-ttu-id="d6e8f-159">Skapa en egen avbildning</span><span class="sxs-lookup"><span data-stu-id="d6e8f-159">Create a custom image</span></span>
> * <span data-ttu-id="d6e8f-160">Skapa en virtuell dator från en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="d6e8f-160">Create a VM from a custom image</span></span>
> * <span data-ttu-id="d6e8f-161">Alla avbildningar i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="d6e8f-161">List all the images in your subscription</span></span>
> * <span data-ttu-id="d6e8f-162">Ta bort en bild</span><span class="sxs-lookup"><span data-stu-id="d6e8f-162">Delete an image</span></span>

<span data-ttu-id="d6e8f-163">Gå vidare till nästa kurs att lära dig hur högtillgängliga virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d6e8f-163">Advance to the next tutorial to learn about how highly available virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d6e8f-164">Skapa virtuella datorer med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="d6e8f-164">Create highly available VMs</span></span>](tutorial-availability-sets.md)



