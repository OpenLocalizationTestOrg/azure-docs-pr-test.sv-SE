---
title: "Skapa en hanterad disk från en virtuell Hårddisk i Azure | Microsoft Docs"
description: "Skapa en hanterad disk från en virtuell Hårddisk som för närvarande i ett Azure storage-konto med hjälp av Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: c03ebf73f1090b595149daf2eb3e274b05822f4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a><span data-ttu-id="3ec64-103">Skapa hanterade diskar från ohanterade diskar i ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="3ec64-103">Create managed disks from unmanaged disks in a storage account</span></span>

<span data-ttu-id="3ec64-104">Hanterade diskar kan skapas från en befintlig datadisk eller en OS-disk som används för närvarande i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3ec64-104">A managed disk can be created from an existing data disk or an OS disk that is currently in an Azure storage account.</span></span> <span data-ttu-id="3ec64-105">Du kan också skapa en tom disk som kan användas som en ny datadisk för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3ec64-105">You can also create an empty disk that can be used as a new data disk for a VM.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="3ec64-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="3ec64-106">Before you begin</span></span>
<span data-ttu-id="3ec64-107">Om du använder PowerShell kan du kontrollera att du har den senaste versionen av AzureRM.Compute PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="3ec64-107">If you use PowerShell, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="3ec64-108">Kör följande kommando för att installera den.</span><span class="sxs-lookup"><span data-stu-id="3ec64-108">Run the following command to install it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="3ec64-109">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3ec64-109">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a><span data-ttu-id="3ec64-110">Skapa en hanterad disk från en virtuell Hårddisk i ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="3ec64-110">Create a managed disk from a VHD in an Azure storage account</span></span>

<span data-ttu-id="3ec64-111">I exemplet med vi skapar en disk från en virtuell Hårddisk som hanterade diskar och kopplar den till parametern **disk 1 $** för senare användning.</span><span class="sxs-lookup"><span data-stu-id="3ec64-111">In the example we create a disk from a VHD as managed disk and assign it to the parameter **$disk1** to use later.</span></span> 

<span data-ttu-id="3ec64-112">Den hantera disken kommer att skapas i den **västra USA** plats i en resursgrupp med namnet **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="3ec64-112">The managed disk will be created in the **West-US** location, in a resource group named **myResourceGroup**.</span></span> <span data-ttu-id="3ec64-113">Disken får namnet **myDisk** och kommer att skapas från en VHD-fil med namnet **myDisk.vhd** vi tidigare har överförts till ett lagringskonto med namnet **mittlagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="3ec64-113">The disk will be named **myDisk** and it will be created from a VHD file named **myDisk.vhd** we previously uploaded to a storage account named **mystorageaccount**.</span></span> <span data-ttu-id="3ec64-114">Den hantera disken ska skapas i premium lokalt redundant lagring (LRS).</span><span class="sxs-lookup"><span data-stu-id="3ec64-114">The managed disk will be created in premium locally-redundant storage (LRS).</span></span> <span data-ttu-id="3ec64-115">StandardLRS och PremiumLRS är den enda **- AccountType** alternativ som är tillgängliga för hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="3ec64-115">StandardLRS and PremiumLRS are the only **-AccountType** options available for managed disks.</span></span> 

1.  <span data-ttu-id="3ec64-116">Ange vissa parametrar</span><span class="sxs-lookup"><span data-stu-id="3ec64-116">Set some parameters</span></span>

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.windows.net/vhds/myDisk.vhd"
    ```

2. <span data-ttu-id="3ec64-117">Skapa datadisk för.</span><span class="sxs-lookup"><span data-stu-id="3ec64-117">Create the data disk.</span></span> 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```
    
    

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a><span data-ttu-id="3ec64-118">Skapa en tom datadisk som en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="3ec64-118">Create an empty data disk as a managed disk</span></span>

<span data-ttu-id="3ec64-119">I exemplet med vi skapar en tom datadisk som hanterade diskar och kopplar den till parametern **$dataDisk2** för senare användning.</span><span class="sxs-lookup"><span data-stu-id="3ec64-119">In the example we create an empty data disk as managed disk and assign it to the parameter **$dataDisk2** to use later.</span></span> <span data-ttu-id="3ec64-120">En tom datadisk måste vara initierad logga in på den virtuella datorn och använder diskmgmt.msc eller [med WinRM och ett skript](attach-disk-ps.md#initialize-the-disk), när den är kopplad till en aktiv virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3ec64-120">An empty data disk will need to be initialized logging in to the VM and using diskmgmt.msc or [remotely using WinRM and a script](attach-disk-ps.md#initialize-the-disk), once it is attached to a running VM.</span></span>

<span data-ttu-id="3ec64-121">Tomma datadisken kommer att skapas i den **Väst centrala oss** plats i en resursgrupp med namnet **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="3ec64-121">The empty data disk will be created in the **West Central US** location, in a resource group named **myResourceGroup**.</span></span> <span data-ttu-id="3ec64-122">Disken får namnet **myEmptyDataDisk**.</span><span class="sxs-lookup"><span data-stu-id="3ec64-122">The disk will be named **myEmptyDataDisk**.</span></span> <span data-ttu-id="3ec64-123">Tom disk kommer att skapas i premium lokalt redundant lagring (LRS).</span><span class="sxs-lookup"><span data-stu-id="3ec64-123">The empty disk will be created in premium locally-redundant storage (LRS).</span></span> <span data-ttu-id="3ec64-124">StandardLRS och PremiumLRS är den enda **- AccountType** alternativ som är tillgängliga för hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="3ec64-124">StandardLRS and PremiumLRS are the only **-AccountType** options available for managed disks.</span></span>

<span data-ttu-id="3ec64-125">Diskens storlek i det här exemplet är 128GB, men du bör välja en storlek som uppfyller behoven för alla program som körs på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3ec64-125">The disk size in this example is 128GB, but you should choose a size that meets the needs of any applications running on your VM.</span></span>

1.  <span data-ttu-id="3ec64-126">Ange vissa parametrar</span><span class="sxs-lookup"><span data-stu-id="3ec64-126">Set some parameters</span></span>

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. <span data-ttu-id="3ec64-127">Skapa datadisk för.</span><span class="sxs-lookup"><span data-stu-id="3ec64-127">Create the data disk.</span></span>
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```
    
## <a name="next-steps"></a><span data-ttu-id="3ec64-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3ec64-128">Next Steps</span></span>   
- <span data-ttu-id="3ec64-129">Om du redan har en virtuell dator kan du [ansluta en datadisk](attach-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3ec64-129">If you already have a VM, you can [attach a data disk](attach-disk-portal.md).</span></span>
