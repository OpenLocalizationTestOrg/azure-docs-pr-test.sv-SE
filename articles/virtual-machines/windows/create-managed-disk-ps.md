---
title: "aaaCreate hanterade diskar från en virtuell Hårddisk i Azure | Microsoft Docs"
description: "Skapa en hanterad disk från en VHD som för närvarande i ett Azure storage-konto med hello Resource Manager-modellen."
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
ms.date: 02/05/2017
ms.author: cynthn
ms.openlocfilehash: 77adaac5419186ff85039fe2c4752f021aa5e448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a><span data-ttu-id="89b52-103">Skapa hanterade diskar från ohanterade diskar i ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="89b52-103">Create managed disks from unmanaged disks in a storage account</span></span>

<span data-ttu-id="89b52-104">Hanterade diskar kan skapas från en befintlig datadisk eller en OS-disk som används för närvarande i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="89b52-104">A managed disk can be created from an existing data disk or an OS disk that is currently in an Azure storage account.</span></span> <span data-ttu-id="89b52-105">Du kan också skapa en tom disk som kan användas som en ny datadisk för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="89b52-105">You can also create an empty disk that can be used as a new data disk for a VM.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="89b52-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="89b52-106">Before you begin</span></span>
<span data-ttu-id="89b52-107">Om du använder PowerShell kan du se till att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="89b52-107">If you use PowerShell, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="89b52-108">Kör följande kommando tooinstall hello den.</span><span class="sxs-lookup"><span data-stu-id="89b52-108">Run hello following command tooinstall it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="89b52-109">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="89b52-109">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a><span data-ttu-id="89b52-110">Skapa en hanterad disk från en virtuell Hårddisk i ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="89b52-110">Create a managed disk from a VHD in an Azure storage account</span></span>

<span data-ttu-id="89b52-111">I hello exempel vi skapar en disk från en virtuell Hårddisk som hanterade diskar och tilldelar den toohello parametern **disk 1 $** toouse senare.</span><span class="sxs-lookup"><span data-stu-id="89b52-111">In hello example we create a disk from a VHD as managed disk and assign it toohello parameter **$disk1** toouse later.</span></span> 

<span data-ttu-id="89b52-112">hello hanterade diskar kommer att skapas i hello **västra USA** plats i en resursgrupp med namnet **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="89b52-112">hello managed disk will be created in hello **West-US** location, in a resource group named **myResourceGroup**.</span></span> <span data-ttu-id="89b52-113">hello disk namnges **myDisk** och kommer att skapas från en VHD-fil med namnet **myDisk.vhd** vi överfört tidigare tooa lagringskontonamnet **mittlagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="89b52-113">hello disk will be named **myDisk** and it will be created from a VHD file named **myDisk.vhd** we previously uploaded tooa storage account named **mystorageaccount**.</span></span> <span data-ttu-id="89b52-114">hello hanterade diskar kommer att skapas i premium lokalt redundant lagring (LRS).</span><span class="sxs-lookup"><span data-stu-id="89b52-114">hello managed disk will be created in premium locally-redundant storage (LRS).</span></span> <span data-ttu-id="89b52-115">StandardLRS och PremiumLRS är endast hello **- AccountType** alternativ som är tillgängliga för hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="89b52-115">StandardLRS and PremiumLRS are hello only **-AccountType** options available for managed disks.</span></span> 

1.  <span data-ttu-id="89b52-116">Ange vissa parametrar</span><span class="sxs-lookup"><span data-stu-id="89b52-116">Set some parameters</span></span>

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.windows.net/vhds/myDisk.vhd"
    ```

2. <span data-ttu-id="89b52-117">Skapa hello datadisk.</span><span class="sxs-lookup"><span data-stu-id="89b52-117">Create hello data disk.</span></span> 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```
    
    

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a><span data-ttu-id="89b52-118">Skapa en tom datadisk som en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="89b52-118">Create an empty data disk as a managed disk</span></span>

<span data-ttu-id="89b52-119">I hello exempel vi skapa en tom datadisk som hanterade diskar och tilldela den toohello parametern **$dataDisk2** toouse senare.</span><span class="sxs-lookup"><span data-stu-id="89b52-119">In hello example we create an empty data disk as managed disk and assign it toohello parameter **$dataDisk2** toouse later.</span></span> <span data-ttu-id="89b52-120">En tom datadisk måste toobe initiera loggning i toohello VM och använder diskmgmt.msc eller [med WinRM och ett skript](attach-disk-ps.md#initialize-the-disk), när den är ansluten tooa använder VM.</span><span class="sxs-lookup"><span data-stu-id="89b52-120">An empty data disk will need toobe initialized logging in toohello VM and using diskmgmt.msc or [remotely using WinRM and a script](attach-disk-ps.md#initialize-the-disk), once it is attached tooa running VM.</span></span>

<span data-ttu-id="89b52-121">hello tom datadisk kommer att skapas i hello **Väst centrala oss** plats i en resursgrupp med namnet **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="89b52-121">hello empty data disk will be created in hello **West Central US** location, in a resource group named **myResourceGroup**.</span></span> <span data-ttu-id="89b52-122">hello disk namnges **myEmptyDataDisk**.</span><span class="sxs-lookup"><span data-stu-id="89b52-122">hello disk will be named **myEmptyDataDisk**.</span></span> <span data-ttu-id="89b52-123">hello tom disk kommer att skapas i premium lokalt redundant lagring (LRS).</span><span class="sxs-lookup"><span data-stu-id="89b52-123">hello empty disk will be created in premium locally-redundant storage (LRS).</span></span> <span data-ttu-id="89b52-124">StandardLRS och PremiumLRS är endast hello **- AccountType** alternativ som är tillgängliga för hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="89b52-124">StandardLRS and PremiumLRS are hello only **-AccountType** options available for managed disks.</span></span>

<span data-ttu-id="89b52-125">hello diskens storlek i det här exemplet är 128GB, men du bör välja en storlek som uppfyller hello behoven för alla program som körs på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="89b52-125">hello disk size in this example is 128GB, but you should choose a size that meets hello needs of any applications running on your VM.</span></span>

1.  <span data-ttu-id="89b52-126">Ange vissa parametrar</span><span class="sxs-lookup"><span data-stu-id="89b52-126">Set some parameters</span></span>

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. <span data-ttu-id="89b52-127">Skapa hello datadisk.</span><span class="sxs-lookup"><span data-stu-id="89b52-127">Create hello data disk.</span></span>
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```
    
## <a name="next-steps"></a><span data-ttu-id="89b52-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89b52-128">Next Steps</span></span>   
- <span data-ttu-id="89b52-129">Om du redan har en virtuell dator kan du [ansluta en datadisk](attach-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="89b52-129">If you already have a VM, you can [attach a data disk](attach-disk-portal.md).</span></span>
