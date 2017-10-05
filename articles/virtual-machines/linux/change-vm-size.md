---
title: "Hur du ändrar storlek på en Linux VM med Azure CLI 2.0 | Microsoft Docs"
description: "Så här skala upp eller ned en Linux-dator genom att ändra VM-storlek."
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: timlt
editor: 
tags: 
ms.assetid: e163f878-b919-45c5-9f5a-75a64f3b14a0
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2017
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 23fc9f7f34732079682857d4ee685fe811751698
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="resize-a-linux-virtual-machine-using-cli-20"></a><span data-ttu-id="42eb3-103">Ändra storlek på en Linux-dator med hjälp av CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="42eb3-103">Resize a Linux virtual machine using CLI 2.0</span></span>

<span data-ttu-id="42eb3-104">När du etablera en virtuell dator (VM) du kan skala VM uppåt eller nedåt genom att ändra den [VM-storlek][vm-sizes].</span><span class="sxs-lookup"><span data-stu-id="42eb3-104">After you provision a virtual machine (VM), you can scale the VM up or down by changing the [VM size][vm-sizes].</span></span> <span data-ttu-id="42eb3-105">I vissa fall måste du frigöra den virtuella datorn först.</span><span class="sxs-lookup"><span data-stu-id="42eb3-105">In some cases, you must deallocate the VM first.</span></span> <span data-ttu-id="42eb3-106">Du måste frigöra den virtuella datorn om önskad storlek inte är tillgänglig på klustret maskinvara som är värd för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="42eb3-106">You need to deallocate the VM if the desired size is not available on the hardware cluster that is hosting the VM.</span></span> <span data-ttu-id="42eb3-107">Den här artikeln beskriver hur du ändrar storlek på en Linux VM med Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="42eb3-107">This article details how to resize a Linux VM with the Azure CLI 2.0.</span></span> <span data-ttu-id="42eb3-108">Du kan också utföra dessa steg med [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="42eb3-108">You can also perform these steps with the [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="resize-a-vm"></a><span data-ttu-id="42eb3-109">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="42eb3-109">Resize a VM</span></span>
<span data-ttu-id="42eb3-110">Om du vill ändra storlek på en virtuell dator, måste senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="42eb3-110">To resize a VM, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

1. <span data-ttu-id="42eb3-111">Visa listan över tillgängliga storlekar på VM maskinvara klustret där den virtuella datorn körs med [az vm-vm-storlek-alternativ för](/cli/azure/vm#list-vm-resize-options).</span><span class="sxs-lookup"><span data-stu-id="42eb3-111">View the list of available VM sizes on the hardware cluster where the VM is hosted with [az vm list-vm-resize-options](/cli/azure/vm#list-vm-resize-options).</span></span> <span data-ttu-id="42eb3-112">I följande exempel visar VM-storlekar för den virtuella datorn med namnet `myVM` i resursgruppen `myResourceGroup` region:</span><span class="sxs-lookup"><span data-stu-id="42eb3-112">The following example lists VM sizes for the VM named `myVM` in the resource group `myResourceGroup` region:</span></span>
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. <span data-ttu-id="42eb3-113">Om den önskade Virtuella datorstorleken visas ändra storlek på den virtuella datorn med [az vm ändra storlek på](/cli/azure/vm#resize).</span><span class="sxs-lookup"><span data-stu-id="42eb3-113">If the desired VM size is listed, resize the VM with [az vm resize](/cli/azure/vm#resize).</span></span> <span data-ttu-id="42eb3-114">I följande exempel ändrar storlek på den virtuella datorn med namnet `myVM` till den `Standard_DS3_v2` storlek:</span><span class="sxs-lookup"><span data-stu-id="42eb3-114">The following example resizes the VM named `myVM` to the `Standard_DS3_v2` size:</span></span>
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    <span data-ttu-id="42eb3-115">Den virtuella datorn startas om under den här processen.</span><span class="sxs-lookup"><span data-stu-id="42eb3-115">The VM restarts during this process.</span></span> <span data-ttu-id="42eb3-116">Efter omstarten mappas befintlig OS- och datadiskar om.</span><span class="sxs-lookup"><span data-stu-id="42eb3-116">After the restart, your existing OS and data disks are remapped.</span></span> <span data-ttu-id="42eb3-117">Allt på den tillfälliga disken går förlorad.</span><span class="sxs-lookup"><span data-stu-id="42eb3-117">Anything on the temporary disk is lost.</span></span>

3. <span data-ttu-id="42eb3-118">Om inte den önskade Virtuella datorstorleken visas måste du först ta bort den virtuella datorn med [az vm frigöra](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="42eb3-118">If the desired VM size is not listed, you need to first deallocate the VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="42eb3-119">Den här processen kan den virtuella datorn ska ändras till valfri storlek som är tillgängliga att regionen stöder och sedan startas.</span><span class="sxs-lookup"><span data-stu-id="42eb3-119">This process allows the VM to then be resized to any size available that the region supports and then started.</span></span> <span data-ttu-id="42eb3-120">Följande steg frigöra, ändra storlek på och sedan starta den virtuella datorn med namnet `myVM` i resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="42eb3-120">The following steps deallocate, resize, and then start the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > <span data-ttu-id="42eb3-121">Det har frigjorts VM också Frigör alla dynamiska IP-adresser som tilldelats den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="42eb3-121">Deallocating the VM also releases any dynamic IP addresses assigned to the VM.</span></span> <span data-ttu-id="42eb3-122">Operativsystemet och datadiskarna påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="42eb3-122">The OS and data disks are not affected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42eb3-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="42eb3-123">Next steps</span></span>
<span data-ttu-id="42eb3-124">För ytterligare utbyggbarhet köra flera VM-instanser och skala ut.</span><span class="sxs-lookup"><span data-stu-id="42eb3-124">For additional scalability, run multiple VM instances and scale out.</span></span> <span data-ttu-id="42eb3-125">Mer information finns i [skala automatiskt Linux-datorer i en virtuell dator Skaluppsättning][scale-set].</span><span class="sxs-lookup"><span data-stu-id="42eb3-125">For more information, see [Automatically scale Linux machines in a Virtual Machine Scale Set][scale-set].</span></span> 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
