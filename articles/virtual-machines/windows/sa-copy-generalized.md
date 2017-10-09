---
title: aaaCreate en ohanterad avbildning av en generaliserad virtuell dator i Azure | Microsoft Docs
description: Skapa en unmanged avbildning av en generaliserad virtuell Windows-dator toouse toocreate flera kopior av en virtuell dator i Azure.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: b8a044d20313efa6dd9b9757e61154f922134476
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-unmanaged-vm-image-from-an-azure-vm"></a><span data-ttu-id="53225-103">Hur toocreate en ohanterad VM bild av en Azure VM</span><span class="sxs-lookup"><span data-stu-id="53225-103">How toocreate an unmanaged VM image from an Azure VM</span></span>

<span data-ttu-id="53225-104">Den här artikeln täcker storage-konton.</span><span class="sxs-lookup"><span data-stu-id="53225-104">This article covers using storage accounts.</span></span> <span data-ttu-id="53225-105">Vi rekommenderar att du använder hanterade diskar och hanterade bilder i stället för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="53225-105">We recommend that you use managed disks and managed images instead of a storage account.</span></span> <span data-ttu-id="53225-106">Mer information finns i [samla in en hanterad avbildning av en generaliserad virtuell dator i Azure](capture-image-resource.md).</span><span class="sxs-lookup"><span data-stu-id="53225-106">For more information, see [Capture a managed image of a generalized VM in Azure](capture-image-resource.md).</span></span>

<span data-ttu-id="53225-107">Den här artikeln beskrivs hur du toouse Azure PowerShell toocreate en avbildning av en generaliserad virtuell Azure-dator med hjälp av ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="53225-107">This article shows you how toouse Azure PowerShell toocreate an image of a generalized Azure VM using a storage account.</span></span> <span data-ttu-id="53225-108">Du kan sedan använda hello avbildningen toocreate en annan virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="53225-108">You can then use hello image toocreate another VM.</span></span> <span data-ttu-id="53225-109">hello bilden innehåller hello OS-disken och hello datadiskar som är bifogade toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="53225-109">hello image includes hello OS disk and hello data disks that are attached toohello virtual machine.</span></span> <span data-ttu-id="53225-110">hello bilden innehåller hello virtuella nätverksresurser, så du måste tooset dessa resurser när du skapar hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="53225-110">hello image doesn't include hello virtual network resources, so you need tooset up those resources when you create hello new VM.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="53225-111">Krav</span><span class="sxs-lookup"><span data-stu-id="53225-111">Prerequisites</span></span>
<span data-ttu-id="53225-112">Du behöver toohave Azure PowerShell version 1.0.x eller nyare.</span><span class="sxs-lookup"><span data-stu-id="53225-112">You need toohave Azure PowerShell version 1.0.x or newer installed.</span></span> <span data-ttu-id="53225-113">Om du inte redan har installerat PowerShell, läsa [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för installationssteg.</span><span class="sxs-lookup"><span data-stu-id="53225-113">If you haven't already installed PowerShell, read [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for installation steps.</span></span>

## <a name="generalize-hello-vm"></a><span data-ttu-id="53225-114">Generalisera hello VM</span><span class="sxs-lookup"><span data-stu-id="53225-114">Generalize hello VM</span></span> 
<span data-ttu-id="53225-115">Det här avsnittet beskrivs hur du toogeneralize din Windows-dator för användning som en bild.</span><span class="sxs-lookup"><span data-stu-id="53225-115">This section shows you how toogeneralize your Windows virtual machine for use as an image.</span></span> <span data-ttu-id="53225-116">Att generalisera en virtuell dator tar du bort alla dina personlig information, bland annat och förbereder hello datorn toobe används som en bild.</span><span class="sxs-lookup"><span data-stu-id="53225-116">Generalizing a VM removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="53225-117">Mer information om Sysprep finns [hur tooUse Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="53225-117">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="53225-118">Kontrollera att hello serverroller som körs på datorn hello stöds av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="53225-118">Make sure hello server roles running on hello machine are supported by Sysprep.</span></span> <span data-ttu-id="53225-119">Mer information finns i [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="53225-119">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="53225-120">Om du överför din VHD tooAzure för hello första gången, kontrollera att du har [förberett din virtuella dator](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) innan du kör Sysprep.</span><span class="sxs-lookup"><span data-stu-id="53225-120">If you are uploading your VHD tooAzure for hello first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

<span data-ttu-id="53225-121">Du kan också generalisera en Linux VM som använder `sudo waagent -deprovision+user` och sedan använda PowerShell toocapture hello VM.</span><span class="sxs-lookup"><span data-stu-id="53225-121">You can also generalize a Linux VM using `sudo waagent -deprovision+user` and then use PowerShell toocapture hello VM.</span></span> <span data-ttu-id="53225-122">Information om hur du använder hello CLI toocapture en virtuell dator finns [hur toogeneralize och avbilda en Linux virtuella datorer med hjälp av hello Azure CLI ](../linux/capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="53225-122">For information about using hello CLI toocapture a VM, see [How toogeneralize and capture a Linux virtual machine using hello Azure CLI ](../linux/capture-image.md).</span></span>


1. <span data-ttu-id="53225-123">Logga in toohello Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="53225-123">Sign in toohello Windows virtual machine.</span></span>
2. <span data-ttu-id="53225-124">Öppna hello Kommandotolken som administratör.</span><span class="sxs-lookup"><span data-stu-id="53225-124">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="53225-125">Ändra hello katalogen för**%windir%\system32\sysprep**, och kör sedan `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="53225-125">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="53225-126">I hello **systemförberedelseverktyget** dialogrutan **ange System Out of Box Experience (OOBE)**, och se till att hello **Generalize** är markerad.</span><span class="sxs-lookup"><span data-stu-id="53225-126">In hello **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that hello **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="53225-127">I **avstängningsalternativ**väljer **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="53225-127">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="53225-128">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="53225-128">Click **OK**.</span></span>
   
    ![Starta Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="53225-130">När Sysprep är klar stänger hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="53225-130">When Sysprep completes, it shuts down hello virtual machine.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="53225-131">Starta inte om hello VM tills du är klar uppladdning hello VHD tooAzure eller skapa en avbildning från hello VM.</span><span class="sxs-lookup"><span data-stu-id="53225-131">Do not restart hello VM until you are done uploading hello VHD tooAzure or creating an image from hello VM.</span></span> <span data-ttu-id="53225-132">Kör Sysprep toogeneralize om hello VM av misstag hämtar startas om den igen.</span><span class="sxs-lookup"><span data-stu-id="53225-132">If hello VM accidentally gets restarted, run Sysprep toogeneralize it again.</span></span>
> 
> 

## <a name="log-in-tooazure-powershell"></a><span data-ttu-id="53225-133">Logga in tooAzure PowerShell</span><span class="sxs-lookup"><span data-stu-id="53225-133">Log in tooAzure PowerShell</span></span>
1. <span data-ttu-id="53225-134">Öppna Azure PowerShell och logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="53225-134">Open Azure PowerShell and sign in tooyour Azure account.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
   
    <span data-ttu-id="53225-135">Ett popup-fönster som öppnas för du tooenter dina Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="53225-135">A pop-up window opens for you tooenter your Azure account credentials.</span></span>
2. <span data-ttu-id="53225-136">Hämta hello prenumerations-ID: N för din tillgängliga prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="53225-136">Get hello subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="53225-137">Ange hello korrekt prenumeration med hjälp av hello prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="53225-137">Set hello correct subscription using hello subscription ID.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-hello-vm-and-set-hello-state-toogeneralized"></a><span data-ttu-id="53225-138">Frigöra hello VM och ange hello tillstånd toogeneralized</span><span class="sxs-lookup"><span data-stu-id="53225-138">Deallocate hello VM and set hello state toogeneralized</span></span>
1. <span data-ttu-id="53225-139">Frigöra hello VM resurser.</span><span class="sxs-lookup"><span data-stu-id="53225-139">Deallocate hello VM resources.</span></span>
   
    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    <span data-ttu-id="53225-140">Hej *Status* för hello VM i hello Azure portal ändras från **stoppad** för**Stoppad (frigjord)**.</span><span class="sxs-lookup"><span data-stu-id="53225-140">hello *Status* for hello VM in hello Azure portal changes from **Stopped** too**Stopped (deallocated)**.</span></span>
2. <span data-ttu-id="53225-141">Ange hello hello virtuella datorns status för för**generaliserad**.</span><span class="sxs-lookup"><span data-stu-id="53225-141">Set hello status of hello virtual machine too**Generalized**.</span></span> 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. <span data-ttu-id="53225-142">Kontrollera hello status hello VM.</span><span class="sxs-lookup"><span data-stu-id="53225-142">Check hello status of hello VM.</span></span> <span data-ttu-id="53225-143">Hej **OSState/generaliserad** avsnittet för hello VM bör ha hello **DisplayStatus** ställa in också**VM generaliserad**.</span><span class="sxs-lookup"><span data-stu-id="53225-143">hello **OSState/generalized** section for hello VM should have hello **DisplayStatus** set too**VM generalized**.</span></span>  
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-hello-image"></a><span data-ttu-id="53225-144">Skapa hello avbildning</span><span class="sxs-lookup"><span data-stu-id="53225-144">Create hello image</span></span>

<span data-ttu-id="53225-145">Skapa en avbildning av ohanterade virtuell dator i hello lagring målbehållare med det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="53225-145">Create an unmanaged virtual machine image in hello destination storage container using this command.</span></span> <span data-ttu-id="53225-146">hello avbildningen skapas i hello samma lagringskonto som hello ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="53225-146">hello image is created in hello same storage account as hello original virtual machine.</span></span> <span data-ttu-id="53225-147">Hej `-Path` parametern sparar en kopia av hello JSON-mall för hello VM tooyour lokala källdator.</span><span class="sxs-lookup"><span data-stu-id="53225-147">hello `-Path` parameter saves a copy of hello JSON template for hello source VM tooyour local computer.</span></span> <span data-ttu-id="53225-148">Hej `-DestinationContainerName` parametern är hello namnet på hello-behållare som du vill toohold bilderna.</span><span class="sxs-lookup"><span data-stu-id="53225-148">hello `-DestinationContainerName` parameter is hello name of hello container that you want toohold your images.</span></span> <span data-ttu-id="53225-149">Om hello-behållaren inte finns, skapas den åt dig.</span><span class="sxs-lookup"><span data-stu-id="53225-149">If hello container doesn't exist, it is created for you.</span></span>
   
```powershell
Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
    -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
    -Path <C:\local\Filepath\Filename.json>
```
   
<span data-ttu-id="53225-150">Du kan hämta hello URL för bilden från hello JSON mall.</span><span class="sxs-lookup"><span data-stu-id="53225-150">You can get hello URL of your image from hello JSON file template.</span></span> <span data-ttu-id="53225-151">Gå toohello **resurser** > **storageProfile** > **osDisk** > **bild**  >  **uri** avsnitt för hello fullständiga sökväg i avbildningen.</span><span class="sxs-lookup"><span data-stu-id="53225-151">Go toohello **resources** > **storageProfile** > **osDisk** > **image** > **uri** section for hello complete path of your image.</span></span> <span data-ttu-id="53225-152">hello-URL för hello bild ser ut: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.</span><span class="sxs-lookup"><span data-stu-id="53225-152">hello URL of hello image looks like: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.</span></span>
   
<span data-ttu-id="53225-153">Du kan också kontrollera hello URI i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="53225-153">You can also verify hello URI in hello portal.</span></span> <span data-ttu-id="53225-154">hello bilden är kopierade tooa behållare med namnet **system** i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="53225-154">hello image is copied tooa container named **system** in your storage account.</span></span> 

## <a name="create-a-vm-from-hello-image"></a><span data-ttu-id="53225-155">Skapa en virtuell dator från hello avbildning</span><span class="sxs-lookup"><span data-stu-id="53225-155">Create a VM from hello image</span></span>

<span data-ttu-id="53225-156">Nu kan du skapa en eller flera virtuella datorer från ohanterade hello-avbildning.</span><span class="sxs-lookup"><span data-stu-id="53225-156">Now you can create one or more VMs from hello unmanaged image.</span></span>

### <a name="set-hello-uri-of-hello-vhd"></a><span data-ttu-id="53225-157">Ange hello hello VHD-URI</span><span class="sxs-lookup"><span data-stu-id="53225-157">Set hello URI of hello VHD</span></span>

<span data-ttu-id="53225-158">hello URI för hello VHD toouse har hello format: https://**mittlagringskonto**.blob.core.windows.net/**minbehållare**/**MyVhdName**VHD.</span><span class="sxs-lookup"><span data-stu-id="53225-158">hello URI for hello VHD toouse is in hello format: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span></span> <span data-ttu-id="53225-159">I det här exemplet hello VHD med namnet **myVHD** är i hello lagringskonto **mittlagringskonto** i hello behållaren **minbehållare**.</span><span class="sxs-lookup"><span data-stu-id="53225-159">In this example hello VHD named **myVHD** is in hello storage account **mystorageaccount** in hello container **mycontainer**.</span></span>

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a><span data-ttu-id="53225-160">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="53225-160">Create a virtual network</span></span>
<span data-ttu-id="53225-161">Skapa hello vNet och undernät för hello [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="53225-161">Create hello vNet and subnet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="53225-162">Skapa hello undernät.</span><span class="sxs-lookup"><span data-stu-id="53225-162">Create hello subnet.</span></span> <span data-ttu-id="53225-163">hello följande exempel skapar ett undernät med namnet **mySubnet** i hello resursgruppen **myResourceGroup** med hello adressprefixet **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="53225-163">hello following sample creates a subnet named **mySubnet** in hello resource group **myResourceGroup** with hello address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="53225-164">Skapa hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="53225-164">Create hello virtual network.</span></span> <span data-ttu-id="53225-165">hello följande exempel skapar ett virtuellt nätverk med namnet **myVnet** i hello **västra USA** plats med hello adressprefixet **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="53225-165">hello following sample creates a virtual network named **myVnet** in hello **West US** location with hello address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="53225-166">Skapa en offentlig IP-adress och gränssnitt</span><span class="sxs-lookup"><span data-stu-id="53225-166">Create a public IP address and network interface</span></span>
<span data-ttu-id="53225-167">tooenable kommunikation med hello virtuell dator i hello virtuella nätverk, behöver du en [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="53225-167">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="53225-168">Skapa en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="53225-168">Create a public IP address.</span></span> <span data-ttu-id="53225-169">Det här exemplet skapas en offentlig IP-adress med namnet **myPip**.</span><span class="sxs-lookup"><span data-stu-id="53225-169">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="53225-170">Skapa hello NIC.</span><span class="sxs-lookup"><span data-stu-id="53225-170">Create hello NIC.</span></span> <span data-ttu-id="53225-171">Det här exemplet skapar ett nätverkskort med namnet **myNic**.</span><span class="sxs-lookup"><span data-stu-id="53225-171">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="53225-172">Skapa hello nätverkssäkerhetsgruppen och en RDP-regel</span><span class="sxs-lookup"><span data-stu-id="53225-172">Create hello network security group and an RDP rule</span></span>
<span data-ttu-id="53225-173">toobe kan toolog i tooyour VM som använder RDP måste toohave en säkerhetsregel som tillåter RDP-åtkomst på port 3389.</span><span class="sxs-lookup"><span data-stu-id="53225-173">toobe able toolog in tooyour VM using RDP, you need toohave a security rule that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="53225-174">Det här exemplet skapas en NSG som heter **myNsg** som innehåller en regel med namnet **myRdpRule** som tillåter RDP-trafik via port 3389.</span><span class="sxs-lookup"><span data-stu-id="53225-174">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="53225-175">Mer information om NSG: er finns [öppna portar tooa VM i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="53225-175">For more information about NSGs, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-hello-virtual-network"></a><span data-ttu-id="53225-176">Skapa en variabel för hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="53225-176">Create a variable for hello virtual network</span></span>
<span data-ttu-id="53225-177">Skapa en variabel för hello slutförts virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="53225-177">Create a variable for hello completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-hello-vm"></a><span data-ttu-id="53225-178">Skapa hello VM</span><span class="sxs-lookup"><span data-stu-id="53225-178">Create hello VM</span></span>
<span data-ttu-id="53225-179">hello följande PowerShell Slutför hello konfigurationer av virtuella datorer och använder ohanterade avbildningen som hello källa för hello ny installation.</span><span class="sxs-lookup"><span data-stu-id="53225-179">hello following PowerShell completes hello virtual machine configurations and uses unmanaged image as hello source for hello new installation.</span></span>

</br>

```powershell
    # Enter a new user name and password toouse as hello local administrator account 
    # for remotely accessing hello VM.
    $cred = Get-Credential

    # Name of hello storage account where hello VHD is located. This example sets hello 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of hello virtual machine. This example sets hello VM name as "myVM".
    $vmName = "myVM"

    # Size of hello virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See hello VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for hello VM. This examples sets hello computer name as "myComputer".
    $computerName = "myComputer"

    # Name of hello disk that holds hello OS. This example sets hello 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets hello SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get hello storage account where hello uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set hello VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set hello Windows operating system configuration and add hello NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    # Create hello OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure hello OS disk toobe created from hello existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create hello new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

### <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="53225-180">Kontrollera att hello VM skapades</span><span class="sxs-lookup"><span data-stu-id="53225-180">Verify that hello VM was created</span></span>
<span data-ttu-id="53225-181">När du är klar bör du se hello nyskapad VM i hello [Azure-portalen](https://portal.azure.com) under **Bläddra** > **virtuella datorer**, eller genom att använda följande hello PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="53225-181">When complete, you should see hello newly created VM in hello [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="53225-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53225-182">Next steps</span></span>
<span data-ttu-id="53225-183">din nya virtuella datorn med Azure PowerShell finns i toomanage [hantera virtuella datorer med Azure Resource Manager och PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="53225-183">toomanage your new virtual machine with Azure PowerShell, see [Manage virtual machines using Azure Resource Manager and PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


