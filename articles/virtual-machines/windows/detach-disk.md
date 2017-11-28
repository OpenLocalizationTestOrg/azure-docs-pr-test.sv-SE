---
title: "aaaDetach en datadisk från en Windows-VM - Azure | Microsoft Docs"
description: "Lär dig toodetach en datadisk från en virtuell dator i Azure med hjälp av hello Resource Manager-modellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: f3f581d3f33329db2ecb7d25a68bc59af7361aad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetach-a-data-disk-from-a-windows-virtual-machine"></a><span data-ttu-id="85743-103">Hur toodetach en disk från en Windows-dator</span><span class="sxs-lookup"><span data-stu-id="85743-103">How toodetach a data disk from a Windows virtual machine</span></span>
<span data-ttu-id="85743-104">När du inte längre behöver en datadisk som är bifogade tooa virtuell dator kan du enkelt kan koppla från den.</span><span class="sxs-lookup"><span data-stu-id="85743-104">When you no longer need a data disk that's attached tooa virtual machine, you can easily detach it.</span></span> <span data-ttu-id="85743-105">Detta tar bort hello disk från hello virtuell dator, men ta bort inte den från lagring.</span><span class="sxs-lookup"><span data-stu-id="85743-105">This removes hello disk from hello virtual machine, but doesn't remove it from storage.</span></span>

> [!WARNING]
> <span data-ttu-id="85743-106">Om du koppla från en disk som den inte tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="85743-106">If you detach a disk it is not automatically deleted.</span></span> <span data-ttu-id="85743-107">Om du har prenumererat tooPremium lagring, fortsätter tooincur lagringskostnaderna för hello disk.</span><span class="sxs-lookup"><span data-stu-id="85743-107">If you have subscribed tooPremium storage, you will continue tooincur storage charges for hello disk.</span></span> <span data-ttu-id="85743-108">Mer information finns för[priser och fakturering när du använder Premiumlagring](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="85743-108">For more information refer too[Pricing and Billing when using Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span></span>
>
>

<span data-ttu-id="85743-109">Om du vill toouse hello befintliga data på hello disk, du kan återansluta den toohello samma virtuella dator eller en annan.</span><span class="sxs-lookup"><span data-stu-id="85743-109">If you want toouse hello existing data on hello disk again, you can reattach it toohello same virtual machine, or another one.</span></span>

## <a name="detach-a-data-disk-using-hello-portal"></a><span data-ttu-id="85743-110">Koppla från en datadisk med hjälp av hello portal</span><span class="sxs-lookup"><span data-stu-id="85743-110">Detach a data disk using hello portal</span></span>
1. <span data-ttu-id="85743-111">I portalen hello-hubb väljer **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="85743-111">In hello portal hub, select **Virtual Machines**.</span></span>
2. <span data-ttu-id="85743-112">Välj hello virtuella dator som har hello datadisk toodetach och på **stoppa** toodeallocate hello VM.</span><span class="sxs-lookup"><span data-stu-id="85743-112">Select hello virtual machine that has hello data disk you want toodetach and click **Stop** toodeallocate hello VM.</span></span>
3. <span data-ttu-id="85743-113">Hello virtuella bladet välj **diskar**.</span><span class="sxs-lookup"><span data-stu-id="85743-113">In hello virtual machine blade, select **Disks**.</span></span>
4. <span data-ttu-id="85743-114">Hello överst i hello **diskar** bladet väljer **redigera**.</span><span class="sxs-lookup"><span data-stu-id="85743-114">At hello top of hello **Disks** blade, select **Edit**.</span></span>
5. <span data-ttu-id="85743-115">I hello **diskar** bladet toohello högerkanten på hello datadisk som du vill att toodetach, klicka på hello ![Detach bilden](./media/detach-disk/detach.png) frånkoppling knappen.</span><span class="sxs-lookup"><span data-stu-id="85743-115">In hello **Disks** blade, toohello far right of hello data disk that you would like toodetach, click hello ![Detach button image](./media/detach-disk/detach.png) detach button.</span></span>
5. <span data-ttu-id="85743-116">Klicka på Spara hello längst upp på bladet hello när hello disken har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="85743-116">After hello disk has been removed, click Save on hello top of hello blade.</span></span>
6. <span data-ttu-id="85743-117">I hello virtuella bladet, klickar du på **översikt** och klicka sedan på hello **starta** knappen hello överst i hello bladet toorestart hello VM.</span><span class="sxs-lookup"><span data-stu-id="85743-117">In hello virtual machine blade, click **Overview** and then click hello **Start** button at hello top of hello blade toorestart hello VM.</span></span>



<span data-ttu-id="85743-118">hello disk kvar i lagring men är inte längre kopplade tooa virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="85743-118">hello disk remains in storage but is no longer attached tooa virtual machine.</span></span>

## <a name="detach-a-data-disk-using-powershell"></a><span data-ttu-id="85743-119">Koppla från en datadisk med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="85743-119">Detach a data disk using PowerShell</span></span>
<span data-ttu-id="85743-120">I det här exemplet hello första kommandot hämtar hello virtuell dator med namnet **MyVM07** i hello **RG11** resursgrupp med hello Get-AzureRmVM cmdlet.</span><span class="sxs-lookup"><span data-stu-id="85743-120">In this example, hello first command gets hello virtual machine named **MyVM07** in hello **RG11** resource group using hello Get-AzureRmVM cmdlet.</span></span> <span data-ttu-id="85743-121">Hej kommandot lagrar hello virtuell dator i hello **$VirtualMachine** variabeln.</span><span class="sxs-lookup"><span data-stu-id="85743-121">hello command stores hello virtual machine in hello **$VirtualMachine** variable.</span></span>

<span data-ttu-id="85743-122">hello andra kommandot tar bort hello datadisk med namnet DataDisk3 från hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="85743-122">hello second command removes hello data disk named DataDisk3 from hello virtual machine.</span></span>

<span data-ttu-id="85743-123">hello kommandot uppdaterar hello virtuella toocomplete hello processen för att ta bort datadisk hello hello tillstånd.</span><span class="sxs-lookup"><span data-stu-id="85743-123">hello final command updates hello state of hello virtual machine toocomplete hello process of removing hello data disk.</span></span>

```powershell
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine
```

<span data-ttu-id="85743-124">Mer information finns i [ta bort AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).</span><span class="sxs-lookup"><span data-stu-id="85743-124">For more information, see [Remove-AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).</span></span>

## <a name="next-steps"></a><span data-ttu-id="85743-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="85743-125">Next steps</span></span>
<span data-ttu-id="85743-126">Om du vill tooreuse hello datadisk du just [bifoga den tooanother VM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="85743-126">If you want tooreuse hello data disk, you can just [attach it tooanother VM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

