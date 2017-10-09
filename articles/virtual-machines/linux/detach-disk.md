---
title: "aaaDetach en datadisk från en Linux VM - Azure | Microsoft Docs"
description: "Lär dig toodetach en datadisk från en virtuell dator i Azure med hjälp av CLI 2.0 eller hello Azure-portalen."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: 1c6145fc97f13179457225e93e0fb7adc261a65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetach-a-data-disk-from-a-linux-virtual-machine"></a><span data-ttu-id="e0ee8-103">Hur toodetach en disk från en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="e0ee8-103">How toodetach a data disk from a Linux virtual machine</span></span>

<span data-ttu-id="e0ee8-104">När du inte längre behöver en datadisk som är bifogade tooa virtuell dator kan du enkelt kan koppla från den.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-104">When you no longer need a data disk that's attached tooa virtual machine, you can easily detach it.</span></span> <span data-ttu-id="e0ee8-105">Detta tar bort hello disk från hello virtuell dator, men ta bort inte den från lagring.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-105">This removes hello disk from hello virtual machine, but doesn't remove it from storage.</span></span> 

> [!WARNING]
> <span data-ttu-id="e0ee8-106">Om du koppla från en disk som den inte tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-106">If you detach a disk it is not automatically deleted.</span></span> <span data-ttu-id="e0ee8-107">Om du har prenumererat tooPremium lagring, fortsätter tooincur lagringskostnaderna för hello disk.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-107">If you have subscribed tooPremium storage, you will continue tooincur storage charges for hello disk.</span></span> <span data-ttu-id="e0ee8-108">Mer information finns för[priser och fakturering när du använder Premiumlagring](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="e0ee8-108">For more information refer too[Pricing and Billing when using Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span></span> 
> 
> 

<span data-ttu-id="e0ee8-109">Om du vill toouse hello befintliga data på hello disk, du kan återansluta den toohello samma virtuella dator eller en annan.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-109">If you want toouse hello existing data on hello disk again, you can reattach it toohello same virtual machine, or another one.</span></span>  

## <a name="detach-a-data-disk-using-cli-20"></a><span data-ttu-id="e0ee8-110">Koppla från en datadisk med hjälp av CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e0ee8-110">Detach a data disk using CLI 2.0</span></span>

```azurecli
az vm disk detach -g myResourceGroup --vm-name myVm -n myDataDisk
```

<span data-ttu-id="e0ee8-111">hello disk kvar i lagring men är inte längre kopplade tooa virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-111">hello disk remains in storage but is no longer attached tooa virtual machine.</span></span>


## <a name="detach-a-data-disk-using-hello-portal"></a><span data-ttu-id="e0ee8-112">Koppla från en datadisk med hjälp av hello portal</span><span class="sxs-lookup"><span data-stu-id="e0ee8-112">Detach a data disk using hello portal</span></span>
1. <span data-ttu-id="e0ee8-113">I portalen hello-hubb väljer **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-113">In hello portal hub, select **Virtual Machines**.</span></span>
2. <span data-ttu-id="e0ee8-114">Välj hello virtuella dator som har hello datadisk toodetach och på **stoppa** toodeallocate hello VM.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-114">Select hello virtual machine that has hello data disk you want toodetach and click **Stop** toodeallocate hello VM.</span></span>
3. <span data-ttu-id="e0ee8-115">Hello virtuella bladet välj **diskar**.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-115">In hello virtual machine blade, select **Disks**.</span></span>
4. <span data-ttu-id="e0ee8-116">Hello överst i hello **diskar** bladet väljer **redigera**.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-116">At hello top of hello **Disks** blade, select **Edit**.</span></span>
5. <span data-ttu-id="e0ee8-117">I hello **diskar** bladet toohello högerkanten på hello datadisk som du vill att toodetach, klicka på hello ![Detach bilden](./media/detach-disk/detach.png) frånkoppling knappen.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-117">In hello **Disks** blade, toohello far right of hello data disk that you would like toodetach, click hello ![Detach button image](./media/detach-disk/detach.png) detach button.</span></span>
5. <span data-ttu-id="e0ee8-118">Klicka på Spara hello längst upp på bladet hello när hello disken har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-118">After hello disk has been removed, click Save on hello top of hello blade.</span></span>
6. <span data-ttu-id="e0ee8-119">I hello virtuella bladet, klickar du på **översikt** och klicka sedan på hello **starta** knappen hello överst i hello bladet toorestart hello VM.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-119">In hello virtual machine blade, click **Overview** and then click hello **Start** button at hello top of hello blade toorestart hello VM.</span></span>

<span data-ttu-id="e0ee8-120">hello disk kvar i lagring men är inte längre kopplade tooa virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e0ee8-120">hello disk remains in storage but is no longer attached tooa virtual machine.</span></span>








## <a name="next-steps"></a><span data-ttu-id="e0ee8-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0ee8-121">Next steps</span></span>
<span data-ttu-id="e0ee8-122">Om du vill tooreuse hello datadisk du just [bifoga den tooanother VM](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e0ee8-122">If you want tooreuse hello data disk, you can just [attach it tooanother VM](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

