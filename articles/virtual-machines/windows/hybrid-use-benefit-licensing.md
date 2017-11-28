---
title: "aaaAzure Hybrid Använd förmån för Windows Server och Windows-klient | Microsoft Docs"
description: "Lär dig hur toomaximize din Windows Software Assurance fördelar toobring lokalt licenser tooAzure"
services: virtual-machines-windows
documentationcenter: 
author: kmouss
manager: timlt
editor: 
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 5/26/2017
ms.author: xujing
ms.openlocfilehash: f24487320a60132aaf766a31f3e6f3726d4a3bd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-hybrid-use-benefit-for-windows-server-and-windows-client"></a><span data-ttu-id="b2c3b-103">Hybridrapportering i Azure används förmån för Windows Server och Windows-klient</span><span class="sxs-lookup"><span data-stu-id="b2c3b-103">Azure Hybrid Use Benefit for Windows Server and Windows Client</span></span>
<span data-ttu-id="b2c3b-104">För kunder med Software Assurance Azure Hybrid Använd förmånen kan du toouse dina lokala licenser för Windows Server och Windows-klienter och kör virtuella Windows-datorer i Azure till en lägre kostnad.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-104">For customers with Software Assurance, Azure Hybrid Use Benefit allows you toouse your on-premises Windows Server and Windows Client licenses and run Windows virtual machines in Azure at a reduced cost.</span></span> <span data-ttu-id="b2c3b-105">Azure Hybrid Använd förmån för Windows Server innehåller Windows Server 2008R2, Windows Server 2012, Windows Server 2012 R2 och Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-105">Azure Hybrid Use Benefit for Windows Server includes Windows Server 2008R2, Windows Server 2012, Windows Server 2012R2, and Windows Server 2016.</span></span> <span data-ttu-id="b2c3b-106">Azure Hybrid Använd förmån för Windows-klienten innehåller Windows 10.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-106">Azure Hybrid Use Benefit for Windows Client includes Windows 10.</span></span> <span data-ttu-id="b2c3b-107">Mer information finns i hello [Azure Hybrid Använd förmånen licensiering sidan](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span><span class="sxs-lookup"><span data-stu-id="b2c3b-107">For more information, please see hello [Azure Hybrid Use Benefit licensing page](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span></span>

>[!IMPORTANT]
><span data-ttu-id="b2c3b-108">Azure Hybrid Använd fördelar för Windows-klienten är för närvarande under förhandsgranskning med hjälp av hello Windows 10-avbildning i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-108">Azure Hybrid Use Benefits for Windows Client is currently in Preview using hello Windows 10 image in hello Azure Marketplace.</span></span> <span data-ttu-id="b2c3b-109">Bara Enterprise-kunder med Windows 10 Enterprise E3/E5 per användare eller Windows VDA per användare (användare prenumerationslicenser eller tillägg användarlicenser prenumeration) (”kvalificerande licenser”) är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-109">Only Enterprise customers with Windows 10 Enterprise E3/E5 per user or Windows VDA per user (User Subscription Licenses or Add-on User Subscription Licenses) (“Qualifying Licenses”) are eligible.</span></span>
>
>

## <a name="ways-toouse-azure-hybrid-use-benefit"></a><span data-ttu-id="b2c3b-110">Sätt toouse Azure Hybrid Använd förmån</span><span class="sxs-lookup"><span data-stu-id="b2c3b-110">Ways toouse Azure Hybrid Use Benefit</span></span>
<span data-ttu-id="b2c3b-111">Det finns ett par olika sätt toodeploy virtuella Windows-datorer med hello Azure Hybrid Använd förmånen:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-111">There are a couple of different ways toodeploy Windows VMs with hello Azure Hybrid Use Benefit:</span></span>

1. <span data-ttu-id="b2c3b-112">Du kan distribuera virtuella datorer från [specifika Marketplace-bilder](#deploy-a-vm-using-the-azure-marketplace) som är förkonfigurerad med Azure Hybrid Använd förmån - Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 och Windows Server 2008SP1.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-112">You can deploy VMs from [specific Marketplace images](#deploy-a-vm-using-the-azure-marketplace) that are pre-configured with Azure Hybrid Use Benefit - Windows Server 2016, Windows Server 2012R2, Windows Server 2012 and Windows Server 2008SP1.</span></span>
2. <span data-ttu-id="b2c3b-113">Du kan [ladda upp en anpassad VM](#upload-a-windows-vhd) och [distribueras med hjälp av en Resource Manager-mall](#deploy-a-vm-via-resource-manager) eller [Azure PowerShell](#detailed-powershell-deployment-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="b2c3b-113">You can [upload a custom VM](#upload-a-windows-vhd) and [deploy using a Resource Manager template](#deploy-a-vm-via-resource-manager) or [Azure PowerShell](#detailed-powershell-deployment-walkthrough).</span></span>

## <a name="deploy-a-vm-using-hello-azure-marketplace"></a><span data-ttu-id="b2c3b-114">Distribuera en virtuell dator med hjälp av hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="b2c3b-114">Deploy a VM using hello Azure Marketplace</span></span>
<span data-ttu-id="b2c3b-115">Följande bilder är tillgängliga i hello Marketplace förkonfigurerade med Azure Hybrid Använd förmån: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 och Windows Server 2008SP1.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-115">Following images are available in hello Marketplace pre-configured with Azure Hybrid Use Benefit: Windows Server 2016, Windows Server 2012R2, Windows Server 2012 and Windows Server 2008SP1.</span></span> <span data-ttu-id="b2c3b-116">Dessa avbildningar kan distribueras direkt från hello Azure-portalen, Resource Manager-mallar eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-116">These images can be deployed directly from hello Azure portal, Resource Manager templates, or Azure PowerShell.</span></span>

<span data-ttu-id="b2c3b-117">Du kan distribuera dessa avbildningar direkt från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-117">You can deploy these images directly from hello Azure portal.</span></span> <span data-ttu-id="b2c3b-118">För användning i Resource Manager-mallar med Azure PowerShell, visa hello listan över bilder på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-118">For use in Resource Manager templates and with Azure PowerShell, view hello list of images as follows:</span></span>

<span data-ttu-id="b2c3b-119">För Windows Server:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-119">For Windows Server:</span></span>
```powershell
Get-AzureRmVMImagesku -Location westus -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```
- <span data-ttu-id="b2c3b-120">2016 Datacenter version 2016.127.20170406 eller senare</span><span class="sxs-lookup"><span data-stu-id="b2c3b-120">2016-Datacenter version 2016.127.20170406 or above</span></span>

- <span data-ttu-id="b2c3b-121">2012 R2 Datacenter version 4.127.20170406 eller senare</span><span class="sxs-lookup"><span data-stu-id="b2c3b-121">2012-R2-Datacenter version 4.127.20170406 or above</span></span>

- <span data-ttu-id="b2c3b-122">2012 Datacenter version 3.127.20170406 eller senare</span><span class="sxs-lookup"><span data-stu-id="b2c3b-122">2012-Datacenter version 3.127.20170406 or above</span></span>

- <span data-ttu-id="b2c3b-123">2008 R2 SP1-version 2.127.20170406 eller senare</span><span class="sxs-lookup"><span data-stu-id="b2c3b-123">2008-R2-SP1 version 2.127.20170406 or above</span></span>

<span data-ttu-id="b2c3b-124">För Windows-klient:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-124">For Windows Client:</span></span>
```powershell
Get-AzureRMVMImageSku -Location "West US" -Publisher "MicrosoftWindowsServer" `
    -Offer "Windows-HUB"
```

## <a name="upload-a-windows-server-vhd"></a><span data-ttu-id="b2c3b-125">Överför en Windows Server VHD</span><span class="sxs-lookup"><span data-stu-id="b2c3b-125">Upload a Windows Server VHD</span></span>
<span data-ttu-id="b2c3b-126">toodeploy en Windows Server-VM i Azure måste du först toocreate en virtuell Hårddisk som innehåller din grundläggande Windows-version.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-126">toodeploy a Windows Server VM in Azure, you first need toocreate a VHD that contains your base Windows build.</span></span> <span data-ttu-id="b2c3b-127">Den här virtuella Hårddisken måste förberedas på rätt sätt via Sysprep innan du laddar upp det tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-127">This VHD must be appropriately prepared via Sysprep before you upload it tooAzure.</span></span> <span data-ttu-id="b2c3b-128">Du kan [Läs mer om hello VHD krav och processer](upload-generalized-managed.md) och [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="b2c3b-128">You can [read more about hello VHD requirements and Sysprep process](upload-generalized-managed.md) and [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span> <span data-ttu-id="b2c3b-129">Säkerhetskopiera hello VM innan du kör Sysprep.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-129">Back up hello VM before running Sysprep.</span></span> 

<span data-ttu-id="b2c3b-130">Kontrollera att du har [installerat och konfigurerat hello senaste Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b2c3b-130">Make sure you have [installed and configured hello latest Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="b2c3b-131">När du har skapat den virtuella Hårddisken kan överföra hello VHD tooyour Azure Storage-konto med hello `Add-AzureRmVhd` cmdlet enligt följande:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-131">Once you have prepared your VHD, upload hello VHD tooyour Azure Storage account using hello `Add-AzureRmVhd` cmdlet as follows:</span></span>

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> <span data-ttu-id="b2c3b-132">Microsoft SQL Server, SharePoint Server och Dynamics kan också använda din Software Assurance-licensiering.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-132">Microsoft SQL Server, SharePoint Server, and Dynamics can also utilize your Software Assurance licensing.</span></span> <span data-ttu-id="b2c3b-133">Du måste fortfarande tooprepare hello Windows Server-avbildning genom att installera programkomponenterna och tillhandahåller licensnycklar i enlighet med detta och sedan överföra hello disk image tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-133">You still need tooprepare hello Windows Server image by installing your application components and providing license keys accordingly, then uploading hello disk image tooAzure.</span></span> <span data-ttu-id="b2c3b-134">Granska hello lämplig dokumentation för att köra Sysprep med ditt program som [överväganden för installation av SQL Server med hjälp av Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) eller [skapa SharePoint Server 2016 referensbild (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).</span><span class="sxs-lookup"><span data-stu-id="b2c3b-134">Review hello appropriate documentation for running Sysprep with your application, such as [Considerations for Installing SQL Server using Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) or [Build a SharePoint Server 2016 Reference Image (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).</span></span>
>
>

<span data-ttu-id="b2c3b-135">Du kan också läsa mer om [överför hello VHD tooAzure process](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)</span><span class="sxs-lookup"><span data-stu-id="b2c3b-135">You can also read more about [uploading hello VHD tooAzure process](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)</span></span>


## <a name="deploy-a-vm-via-resource-manager-template"></a><span data-ttu-id="b2c3b-136">Distribuera en virtuell dator via Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="b2c3b-136">Deploy a VM via Resource Manager Template</span></span>
<span data-ttu-id="b2c3b-137">I Resource Manager-mallar, en ytterligare parameter för `licenseType` kan anges.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-137">Within your Resource Manager templates, an additional parameter for `licenseType` can be specified.</span></span> <span data-ttu-id="b2c3b-138">Du kan läsa mer om [skapa mallar för Azure Resource Manager](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3b-138">You can read more about [authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span> <span data-ttu-id="b2c3b-139">När du har överförts VHD-tooAzure redigerar du Resource Manager tooinclude hello licens malltypen som en del av hello compute-providern och distribuera mallen som vanligt:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-139">Once you have your VHD uploaded tooAzure, edit you Resource Manager template tooinclude hello license type as part of hello compute provider and deploy your template as normal:</span></span>

<span data-ttu-id="b2c3b-140">För Windows Server:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-140">For Windows Server:</span></span>
```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

<span data-ttu-id="b2c3b-141">För Windows-klient toouse med Azure Marketplace-avbildning endast:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-141">For Windows Client toouse with Azure Marketplace Image only:</span></span>
```json
"properties": {  
   "licenseType": "Windows_Client",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="deploy-a-vm-via-powershell-quickstart"></a><span data-ttu-id="b2c3b-142">Distribuera en virtuell dator via PowerShell-Snabbstart</span><span class="sxs-lookup"><span data-stu-id="b2c3b-142">Deploy a VM via PowerShell quickstart</span></span>
<span data-ttu-id="b2c3b-143">När du distribuerar Windows Server-VM via PowerShell kan du ha en ytterligare parameter för `-LicenseType`.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-143">When deploying your Windows Server VM via PowerShell, you have an additional parameter for `-LicenseType`.</span></span> <span data-ttu-id="b2c3b-144">När du har överförts VHD-tooAzure kan du skapa en virtuell dator med hjälp av `New-AzureRmVM` och ange hello licensiering på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-144">Once you have your VHD uploaded tooAzure, you create a VM using `New-AzureRmVM` and specify hello licensing type as follows:</span></span>

<span data-ttu-id="b2c3b-145">För Windows Server:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-145">For Windows Server:</span></span>
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

<span data-ttu-id="b2c3b-146">För Windows-klient toouse med Azure Marketplace-avbildning endast:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-146">For Windows Client toouse with Azure Marketplace Image only:</span></span>
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

<span data-ttu-id="b2c3b-147">Du kan [läsa en mer detaljerad genomgång om hur du distribuerar en virtuell dator i Azure via PowerShell](hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) nedan eller läsa ett mer beskrivande vägledning om hello olika steg för[skapa en virtuell Windows-dator med Resource Manager och PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b2c3b-147">You can [read a more detailed walkthrough on deploying a VM in Azure via PowerShell](hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) below, or read a more descriptive guide on hello different steps too[create a Windows VM using Resource Manager and PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


## <a name="verify-your-vm-is-utilizing-hello-licensing-benefit"></a><span data-ttu-id="b2c3b-148">Kontrollera den virtuella datorn använder hello licensiering förmån</span><span class="sxs-lookup"><span data-stu-id="b2c3b-148">Verify your VM is utilizing hello licensing benefit</span></span>
<span data-ttu-id="b2c3b-149">När du har distribuerat den virtuella datorn via hello PowerShell eller Resource Manager distributionsmetoden Kontrollera hello licenstypen med `Get-AzureRmVM` på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-149">Once you have deployed your VM through either hello PowerShell or Resource Manager deployment method, verify hello license type with `Get-AzureRmVM` as follows:</span></span>

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

<span data-ttu-id="b2c3b-150">hello utdata är liknande toohello följande exempel för Windows Server:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-150">hello output is similar toohello following example for Windows Server:</span></span>

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

<span data-ttu-id="b2c3b-151">Den här utdatan står i kontrast mot hello följande VM distribueras utan Azure Hybrid Använd förmånen licensiering, till exempel en virtuell dator distribueras direkt från hello Azure-galleriet:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-151">This output contrasts with hello following VM deployed without Azure Hybrid Use Benefit licensing, such as a VM deployed straight from hello Azure Gallery:</span></span>

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="detailed-powershell-deployment-walkthrough"></a><span data-ttu-id="b2c3b-152">Detaljerad genomgång för distribution av PowerShell</span><span class="sxs-lookup"><span data-stu-id="b2c3b-152">Detailed PowerShell deployment walkthrough</span></span>
<span data-ttu-id="b2c3b-153">hello följande detaljerad PowerShell steg visa en fullständig distribution av en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-153">hello following detailed PowerShell steps show a full deployment of a VM.</span></span> <span data-ttu-id="b2c3b-154">Du kan läsa mer kontext som toohello faktiska cmdlets och olika komponenter som skapas i [skapa en virtuell Windows-dator med Resource Manager och PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b2c3b-154">You can read more context as toohello actual cmdlets and different components being created in [Create a Windows VM using Resource Manager and PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="b2c3b-155">Du steg för steg att skapa din resursgrupp, storage-konto och virtuella nätverk, och sedan definiera den virtuella datorn och slutligen skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-155">You step through creating your resource group, storage account, and virtual networking, then define your VM and finally create your VM.</span></span>

<span data-ttu-id="b2c3b-156">Först på ett säkert sätt hämta autentiseringsuppgifter genom att ange en plats och resursgruppens namn:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-156">First, securely obtain credentials, set a location, and resource group name:</span></span>

```powershell
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "myResourceGroup"
```

<span data-ttu-id="b2c3b-157">Skapa en offentlig IP-adress:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-157">Create a public IP:</span></span>

```powershell
$publicIPName = "myPublicIP"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName `
    -Location $location -AllocationMethod "Dynamic"
```

<span data-ttu-id="b2c3b-158">Definiera din undernät, nätverkskort och virtuella nätverk:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-158">Define your subnet, NIC, and VNET:</span></span>

```powershell
$subnetName = "mySubnet"
$nicName = "myNIC"
$vnetName = "myVnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location `
    -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

<span data-ttu-id="b2c3b-159">Namnge den virtuella datorn och skapa en VM-konfiguration:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-159">Name your VM and create a VM config:</span></span>

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

<span data-ttu-id="b2c3b-160">Definiera ditt operativsystem:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-160">Define your OS:</span></span>

```powershell
$computerName = "myVM"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

<span data-ttu-id="b2c3b-161">Lägg till din NIC toohello VM:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-161">Add your NIC toohello VM:</span></span>

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

<span data-ttu-id="b2c3b-162">Definiera hello storage-konto toouse:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-162">Define hello storage account toouse:</span></span>

```powershell
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName mystorageaccount
```

<span data-ttu-id="b2c3b-163">Överför den virtuella Hårddisken på lämpligt sätt har förberetts, och koppla tooyour VM för användning:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-163">Upload your VHD, suitably prepared, and attach tooyour VM for use:</span></span>

```powershell
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/vhd/myvhd.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage `
    -SourceImageUri $urlOfUploadedImageVhd -Windows
```

<span data-ttu-id="b2c3b-164">Slutligen skapar den virtuella datorn och definiera hello licensiering typen tooutilize förmån för Azure Hybrid användning:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-164">Finally, create your VM and define hello licensing type tooutilize Azure Hybrid Use Benefit:</span></span>

<span data-ttu-id="b2c3b-165">För Windows Server:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-165">For Windows Server:</span></span>
```powershell
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType "Windows_Server"
```

## <a name="deploy-a-virtual-machine-scale-set-via-resource-manager-template"></a><span data-ttu-id="b2c3b-166">Distribuera en virtuell dator skala in via Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="b2c3b-166">Deploy a virtual machine scale set via Resource Manager template</span></span>
<span data-ttu-id="b2c3b-167">Inom en ytterligare parameter för dina VMSS Resource Manager-mallar `licenseType` måste anges.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-167">Within your VMSS Resource Manager templates, an additional parameter for `licenseType` must be specified.</span></span> <span data-ttu-id="b2c3b-168">Du kan läsa mer om [skapa mallar för Azure Resource Manager](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3b-168">You can read more about [authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span> <span data-ttu-id="b2c3b-169">Redigera din Resource Manager tooinclude hello licenseType mallegenskapen som en del av hello scale set virtualMachineProfile och distribuera mallen som vanligt - Se exemplet nedan med hjälp av Windows Server 2016-avbildning:</span><span class="sxs-lookup"><span data-stu-id="b2c3b-169">Edit your Resource Manager template tooinclude hello licenseType property as part of hello scale set’s virtualMachineProfile and deploy your template as normal - see example below using 2016 Windows Server image:</span></span>


```json
"virtualMachineProfile": {
    "storageProfile": {
        "osDisk": {
            "createOption": "FromImage"
        },
        "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2016-Datacenter",
            "version": "latest"
        }
    },
    "licenseType": "Windows_Server",
    "osProfile": {
            "computerNamePrefix": "[parameters('vmssName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
    }
```

> [!NOTE]
> <span data-ttu-id="b2c3b-170">Stöd för att distribuera en virtuell dator-skala med AHUB fördelarna via PowerShell och andra SDK-verktyg kommer snart.</span><span class="sxs-lookup"><span data-stu-id="b2c3b-170">Support for deploying a virtual machine scale set with AHUB benefits through PowerShell and other SDK tools is coming soon.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="b2c3b-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b2c3b-171">Next steps</span></span>
<span data-ttu-id="b2c3b-172">Läs mer om [Azure Hybrid Använd förmånen licensiering](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span><span class="sxs-lookup"><span data-stu-id="b2c3b-172">Read more about [Azure Hybrid Use Benefit licensing](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span></span>

<span data-ttu-id="b2c3b-173">Lär dig mer om [med hjälp av Resource Manager-mallar](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3b-173">Learn more about [using Resource Manager templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="b2c3b-174">Lär dig mer om [Azure Hybrid Använd nytta och Azure Site Recovery göra migrera program tooAzure ännu mer kostnadseffektiv](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/).</span><span class="sxs-lookup"><span data-stu-id="b2c3b-174">Learn more about [Azure Hybrid Use Benefit and Azure Site Recovery make migrating applications tooAzure even more cost-effective](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/).</span></span>
