---
title: aaaHow tooresize en Linux VM med hello Azure CLI 1.0 | Microsoft Docs
description: "Hur tooscale upp eller skala ned en virtuell Linux-dator, genom att ändra hello VM-storlek."
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2016
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 43dd955dc2f2dd9d1b2da07ecbfbf2459bcaa4d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-linux-vm-with-azure-cli-10"></a><span data-ttu-id="59bb9-103">Ändra storlek på en virtuell Linux-dator med Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="59bb9-103">Resize a Linux VM with Azure CLI 1.0</span></span>

## <a name="overview"></a><span data-ttu-id="59bb9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="59bb9-104">Overview</span></span>

<span data-ttu-id="59bb9-105">När du etablera en virtuell dator (VM) du kan skala hello VM uppåt eller nedåt genom att ändra hello [VM-storlek][vm-sizes].</span><span class="sxs-lookup"><span data-stu-id="59bb9-105">After you provision a virtual machine (VM), you can scale hello VM up or down by changing hello [VM size][vm-sizes].</span></span> <span data-ttu-id="59bb9-106">I vissa fall måste du frigöra hello VM först.</span><span class="sxs-lookup"><span data-stu-id="59bb9-106">In some cases, you must deallocate hello VM first.</span></span> <span data-ttu-id="59bb9-107">Detta kan inträffa om nya hello-storleken inte är tillgänglig på hello maskinvara kluster som är värd för hello VM.</span><span class="sxs-lookup"><span data-stu-id="59bb9-107">This can happen if hello new size is not available on hello hardware cluster that is hosting hello VM.</span></span>

<span data-ttu-id="59bb9-108">Den här artikeln visar hur tooresize en Linux VM som använder hello [Azure CLI][azure-cli].</span><span class="sxs-lookup"><span data-stu-id="59bb9-108">This article shows how tooresize a Linux VM using hello [Azure CLI][azure-cli].</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="59bb9-109">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="59bb9-109">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="59bb9-110">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="59bb9-110">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="59bb9-111">[Azure CLI 1.0](#resize-a-linux-vm) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="59bb9-111">[Azure CLI 1.0](#resize-a-linux-vm) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="59bb9-112">[Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="59bb9-112">[Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="resize-a-linux-vm"></a><span data-ttu-id="59bb9-113">Ändra storlek på en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="59bb9-113">Resize a Linux VM</span></span>
<span data-ttu-id="59bb9-114">tooresize en VM, utföra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="59bb9-114">tooresize a VM, perform hello following steps.</span></span>

1. <span data-ttu-id="59bb9-115">Kör följande kommando CLI hello.</span><span class="sxs-lookup"><span data-stu-id="59bb9-115">Run hello following CLI command.</span></span> <span data-ttu-id="59bb9-116">Det här kommandot visar hello VM-storlekar som är tillgängliga på hello maskinvara kluster där hello VM finns.</span><span class="sxs-lookup"><span data-stu-id="59bb9-116">This command lists hello VM sizes that are available on hello hardware cluster where hello VM is hosted.</span></span>
   
    ```azurecli
    azure vm sizes -g myResourceGroup --vm-name myVM
    ```
2. <span data-ttu-id="59bb9-117">Eventuellt hello storlek visas kör du följande kommando tooresize hello VM hello.</span><span class="sxs-lookup"><span data-stu-id="59bb9-117">If hello desired size is listed, run hello following command tooresize hello VM.</span></span>
   
    ```azurecli
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM  \
        --enable-boot-diagnostics
        --boot-diagnostics-storage-uri https://mystorageaccount.blob.core.windows.net/ 
    ```
   
    <span data-ttu-id="59bb9-118">hello VM startas om under den här processen.</span><span class="sxs-lookup"><span data-stu-id="59bb9-118">hello VM will restart during this process.</span></span> <span data-ttu-id="59bb9-119">Efter hello omstart befintlig OS- och datadiskar kommer att mappas om.</span><span class="sxs-lookup"><span data-stu-id="59bb9-119">After hello restart, your existing OS and data disks will be remapped.</span></span> <span data-ttu-id="59bb9-120">Allt på hello diskutrymme går förlorade.</span><span class="sxs-lookup"><span data-stu-id="59bb9-120">Anything on hello temporary disk will be lost.</span></span>
   
    <span data-ttu-id="59bb9-121">Använd hello `--enable-boot-diagnostics` aktiverar [starta diagnostik][boot-diagnostics], toolog eventuella relaterade fel toostartup.</span><span class="sxs-lookup"><span data-stu-id="59bb9-121">Use hello `--enable-boot-diagnostics` option enables [boot diagnostics][boot-diagnostics], toolog any errors related toostartup.</span></span>
3. <span data-ttu-id="59bb9-122">Hello önskad storlek inte visas, kör du annars hello följande kommandon toodeallocate hello VM, ändra storlek på den och sedan starta om hello VM.</span><span class="sxs-lookup"><span data-stu-id="59bb9-122">Otherwise, if hello desired size is not listed, run hello following commands toodeallocate hello VM, resize it, and then restart hello VM.</span></span>
   
    ```azurecli
    azure vm deallocate -g myResourceGroup myVM
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM \
        --enable-boot-diagnostics --boot-diagnostics-storage-uri \
        https://mystorageaccount.blob.core.windows.net/ 
    azure vm start -g myResourceGroup myVM
    ```
   
   > [!WARNING]
   > <span data-ttu-id="59bb9-123">Det har frigjorts hello VM Frigör också alla dynamiska IP-adresser som tilldelats toohello VM.</span><span class="sxs-lookup"><span data-stu-id="59bb9-123">Deallocating hello VM also releases any dynamic IP addresses assigned toohello VM.</span></span> <span data-ttu-id="59bb9-124">hello OS- och datadiskar påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="59bb9-124">hello OS and data disks are not affected.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="59bb9-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59bb9-125">Next steps</span></span>
<span data-ttu-id="59bb9-126">För ytterligare utbyggbarhet köra flera VM-instanser och skala ut. Mer information finns i [skala automatiskt Linux-datorer i en virtuell dator Skaluppsättning][scale-set].</span><span class="sxs-lookup"><span data-stu-id="59bb9-126">For additional scalability, run multiple VM instances and scale out. For more information, see [Automatically scale Linux machines in a Virtual Machine Scale Set][scale-set].</span></span> 

<!-- links -->

[azure-cli]:../../cli-install-nodejs.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
