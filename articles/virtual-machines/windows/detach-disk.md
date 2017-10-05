---
title: "Koppla från en datadisk från en Windows-VM - Azure | Microsoft Docs"
description: "Lär dig att koppla från en datadisk från en virtuell dator i Azure med hjälp av Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: 97aa69745d200ee76f9f859eb3a8b0ad2f202bad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a><span data-ttu-id="97a11-103">Hur du koppla från en datadisk från en Windows-dator</span><span class="sxs-lookup"><span data-stu-id="97a11-103">How to detach a data disk from a Windows virtual machine</span></span>
<span data-ttu-id="97a11-104">När du inte längre behöver en datadisk som är ansluten till en virtuell dator kan du enkelt koppla bort den.</span><span class="sxs-lookup"><span data-stu-id="97a11-104">When you no longer need a data disk that's attached to a virtual machine, you can easily detach it.</span></span> <span data-ttu-id="97a11-105">Detta tar bort disken från den virtuella datorn, men ta bort inte den från lagring.</span><span class="sxs-lookup"><span data-stu-id="97a11-105">This removes the disk from the virtual machine, but doesn't remove it from storage.</span></span>

> [!WARNING]
> <span data-ttu-id="97a11-106">Om du koppla från en disk som den inte tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="97a11-106">If you detach a disk it is not automatically deleted.</span></span> <span data-ttu-id="97a11-107">Om du har prenumererat på Premium-lagring, fortsätter du lagring avgifter för disken.</span><span class="sxs-lookup"><span data-stu-id="97a11-107">If you have subscribed to Premium storage, you will continue to incur storage charges for the disk.</span></span> <span data-ttu-id="97a11-108">Mer information finns i [priser och fakturering när du använder Premiumlagring](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="97a11-108">For more information refer to [Pricing and Billing when using Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span></span>
>
>

<span data-ttu-id="97a11-109">Om du vill använda befintliga data på disken igen kan du ansluta den igen till samma virtuella dator, eller till en annan.</span><span class="sxs-lookup"><span data-stu-id="97a11-109">If you want to use the existing data on the disk again, you can reattach it to the same virtual machine, or another one.</span></span>

## <a name="detach-a-data-disk-using-the-portal"></a><span data-ttu-id="97a11-110">Koppla ifrån en datadisk med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="97a11-110">Detach a data disk using the portal</span></span>
1. <span data-ttu-id="97a11-111">I portalen hubben, väljer **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="97a11-111">In the portal hub, select **Virtual Machines**.</span></span>
2. <span data-ttu-id="97a11-112">Välj den virtuella dator som har datadisk som du vill koppla från och klickar på **stoppa** att frigöra den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="97a11-112">Select the virtual machine that has the data disk you want to detach and click **Stop** to deallocate the VM.</span></span>
3. <span data-ttu-id="97a11-113">I bladet för virtuella datorer väljer du **diskar**.</span><span class="sxs-lookup"><span data-stu-id="97a11-113">In the virtual machine blade, select **Disks**.</span></span>
4. <span data-ttu-id="97a11-114">Längst upp i den **diskar** bladet väljer **redigera**.</span><span class="sxs-lookup"><span data-stu-id="97a11-114">At the top of the **Disks** blade, select **Edit**.</span></span>
5. <span data-ttu-id="97a11-115">I den **diskar** bladet längst till höger på disken för data som du vill koppla från, klicka på den ![Detach bilden](./media/detach-disk/detach.png) frånkoppling knappen.</span><span class="sxs-lookup"><span data-stu-id="97a11-115">In the **Disks** blade, to the far right of the data disk that you would like to detach, click the ![Detach button image](./media/detach-disk/detach.png) detach button.</span></span>
5. <span data-ttu-id="97a11-116">När disken har tagits bort, klicka på Spara överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="97a11-116">After the disk has been removed, click Save on the top of the blade.</span></span>
6. <span data-ttu-id="97a11-117">I bladet för virtuella datorer, klickar du på **översikt** och klicka sedan på den **starta** längst upp på bladet för att starta om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="97a11-117">In the virtual machine blade, click **Overview** and then click the **Start** button at the top of the blade to restart the VM.</span></span>



<span data-ttu-id="97a11-118">Disken finns kvar i lagringsutrymmet men är inte längre kopplad till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="97a11-118">The disk remains in storage but is no longer attached to a virtual machine.</span></span>

## <a name="detach-a-data-disk-using-powershell"></a><span data-ttu-id="97a11-119">Koppla från en datadisk med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="97a11-119">Detach a data disk using PowerShell</span></span>
<span data-ttu-id="97a11-120">I det här exemplet är det första kommandot hämtar den virtuella datorn med namnet **MyVM07** i den **RG11** resursgrupp med hjälp av cmdleten Get-AzureRmVM.</span><span class="sxs-lookup"><span data-stu-id="97a11-120">In this example, the first command gets the virtual machine named **MyVM07** in the **RG11** resource group using the Get-AzureRmVM cmdlet.</span></span> <span data-ttu-id="97a11-121">Kommandot lagrar den virtuella datorn i den **$VirtualMachine** variabeln.</span><span class="sxs-lookup"><span data-stu-id="97a11-121">The command stores the virtual machine in the **$VirtualMachine** variable.</span></span>

<span data-ttu-id="97a11-122">Det andra kommandot tar bort datadisken med namnet DataDisk3 från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="97a11-122">The second command removes the data disk named DataDisk3 from the virtual machine.</span></span>

<span data-ttu-id="97a11-123">Kommandot uppdaterar tillståndet för den virtuella datorn för att slutföra processen för att ta bort datadisken.</span><span class="sxs-lookup"><span data-stu-id="97a11-123">The final command updates the state of the virtual machine to complete the process of removing the data disk.</span></span>

```powershell
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine
```

<span data-ttu-id="97a11-124">Mer information finns i [ta bort AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).</span><span class="sxs-lookup"><span data-stu-id="97a11-124">For more information, see [Remove-AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).</span></span>

## <a name="next-steps"></a><span data-ttu-id="97a11-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="97a11-125">Next steps</span></span>
<span data-ttu-id="97a11-126">Om du vill återanvända datadisk du just [koppla den till en annan virtuell dator](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="97a11-126">If you want to reuse the data disk, you can just [attach it to another VM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

