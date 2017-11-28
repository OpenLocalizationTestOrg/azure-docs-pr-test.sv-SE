---
title: aaaHow tooresize en Linux VM med hello Azure CLI 2.0 | Microsoft Docs
description: "Hur tooscale upp eller skala ned en virtuell Linux-dator, genom att ändra hello VM-storlek."
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
ms.openlocfilehash: e8fba485b5bcc7824f546de5cf3df77624a28008
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-linux-virtual-machine-using-cli-20"></a><span data-ttu-id="5e2fe-103">Ändra storlek på en Linux-dator med hjälp av CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5e2fe-103">Resize a Linux virtual machine using CLI 2.0</span></span>

<span data-ttu-id="5e2fe-104">När du etablera en virtuell dator (VM) du kan skala hello VM uppåt eller nedåt genom att ändra hello [VM-storlek][vm-sizes].</span><span class="sxs-lookup"><span data-stu-id="5e2fe-104">After you provision a virtual machine (VM), you can scale hello VM up or down by changing hello [VM size][vm-sizes].</span></span> <span data-ttu-id="5e2fe-105">I vissa fall måste du frigöra hello VM först.</span><span class="sxs-lookup"><span data-stu-id="5e2fe-105">In some cases, you must deallocate hello VM first.</span></span> <span data-ttu-id="5e2fe-106">Du måste toodeallocate hello VM eventuellt hello storleken inte är tillgänglig på hello maskinvara kluster som är värd för hello VM.</span><span class="sxs-lookup"><span data-stu-id="5e2fe-106">You need toodeallocate hello VM if hello desired size is not available on hello hardware cluster that is hosting hello VM.</span></span> <span data-ttu-id="5e2fe-107">Den här artikeln beskrivs hur tooresize en Linux VM med hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="5e2fe-107">This article details how tooresize a Linux VM with hello Azure CLI 2.0.</span></span> <span data-ttu-id="5e2fe-108">Du kan också utföra dessa steg med hello [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5e2fe-108">You can also perform these steps with hello [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="resize-a-vm"></a><span data-ttu-id="5e2fe-109">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5e2fe-109">Resize a VM</span></span>
<span data-ttu-id="5e2fe-110">tooresize en virtuell dator måste hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="5e2fe-110">tooresize a VM, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

1. <span data-ttu-id="5e2fe-111">Visa hello lista med tillgängliga Virtuella storlekar på hello maskinvara kluster där hello VM finns med [az vm-vm-storlek-alternativ för](/cli/azure/vm#list-vm-resize-options).</span><span class="sxs-lookup"><span data-stu-id="5e2fe-111">View hello list of available VM sizes on hello hardware cluster where hello VM is hosted with [az vm list-vm-resize-options](/cli/azure/vm#list-vm-resize-options).</span></span> <span data-ttu-id="5e2fe-112">hello följande exempel visar en lista över VM-storlekar för hello virtuella datorn med namnet `myVM` i hello resursgruppen `myResourceGroup` region:</span><span class="sxs-lookup"><span data-stu-id="5e2fe-112">hello following example lists VM sizes for hello VM named `myVM` in hello resource group `myResourceGroup` region:</span></span>
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. <span data-ttu-id="5e2fe-113">Ändra storlek på eventuellt hello VM-storlek visas hello virtuell dator med [az vm ändra storlek på](/cli/azure/vm#resize).</span><span class="sxs-lookup"><span data-stu-id="5e2fe-113">If hello desired VM size is listed, resize hello VM with [az vm resize](/cli/azure/vm#resize).</span></span> <span data-ttu-id="5e2fe-114">hello följande exempel ändrar storleken hello virtuella datorn med namnet `myVM` toohello `Standard_DS3_v2` storlek:</span><span class="sxs-lookup"><span data-stu-id="5e2fe-114">hello following example resizes hello VM named `myVM` toohello `Standard_DS3_v2` size:</span></span>
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    <span data-ttu-id="5e2fe-115">hello VM startas om under den här processen.</span><span class="sxs-lookup"><span data-stu-id="5e2fe-115">hello VM restarts during this process.</span></span> <span data-ttu-id="5e2fe-116">Efter omstart hello mappas befintlig OS- och datadiskar om.</span><span class="sxs-lookup"><span data-stu-id="5e2fe-116">After hello restart, your existing OS and data disks are remapped.</span></span> <span data-ttu-id="5e2fe-117">Allt på hello diskutrymme går förlorad.</span><span class="sxs-lookup"><span data-stu-id="5e2fe-117">Anything on hello temporary disk is lost.</span></span>

3. <span data-ttu-id="5e2fe-118">Eventuellt hello VM-storlek inte visas toofirst måste frigöra hello virtuell dator med [az vm frigöra](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="5e2fe-118">If hello desired VM size is not listed, you need toofirst deallocate hello VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="5e2fe-119">Den här processen kan hello VM toothen att ändrade tooany storlek som hello stöder region och sedan startas.</span><span class="sxs-lookup"><span data-stu-id="5e2fe-119">This process allows hello VM toothen be resized tooany size available that hello region supports and then started.</span></span> <span data-ttu-id="5e2fe-120">hello följande steg frigöra, ändra storlek på och sedan starta hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="5e2fe-120">hello following steps deallocate, resize, and then start hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > <span data-ttu-id="5e2fe-121">Det har frigjorts hello VM Frigör också alla dynamiska IP-adresser som tilldelats toohello VM.</span><span class="sxs-lookup"><span data-stu-id="5e2fe-121">Deallocating hello VM also releases any dynamic IP addresses assigned toohello VM.</span></span> <span data-ttu-id="5e2fe-122">hello OS- och datadiskar påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="5e2fe-122">hello OS and data disks are not affected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e2fe-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5e2fe-123">Next steps</span></span>
<span data-ttu-id="5e2fe-124">För ytterligare utbyggbarhet köra flera VM-instanser och skala ut. Mer information finns i [skala automatiskt Linux-datorer i en virtuell dator Skaluppsättning][scale-set].</span><span class="sxs-lookup"><span data-stu-id="5e2fe-124">For additional scalability, run multiple VM instances and scale out. For more information, see [Automatically scale Linux machines in a Virtual Machine Scale Set][scale-set].</span></span> 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
