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
# <a name="resize-a-linux-vm-with-azure-cli-10"></a>Ändra storlek på en virtuell Linux-dator med Azure CLI 1.0

## <a name="overview"></a>Översikt

När du etablera en virtuell dator (VM) du kan skala hello VM uppåt eller nedåt genom att ändra hello [VM-storlek][vm-sizes]. I vissa fall måste du frigöra hello VM först. Detta kan inträffa om nya hello-storleken inte är tillgänglig på hello maskinvara kluster som är värd för hello VM.

Den här artikeln visar hur tooresize en Linux VM som använder hello [Azure CLI][azure-cli].

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#resize-a-linux-vm) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering


## <a name="resize-a-linux-vm"></a>Ändra storlek på en virtuell Linux-dator
tooresize en VM, utföra hello följande steg.

1. Kör följande kommando CLI hello. Det här kommandot visar hello VM-storlekar som är tillgängliga på hello maskinvara kluster där hello VM finns.
   
    ```azurecli
    azure vm sizes -g myResourceGroup --vm-name myVM
    ```
2. Eventuellt hello storlek visas kör du följande kommando tooresize hello VM hello.
   
    ```azurecli
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM  \
        --enable-boot-diagnostics
        --boot-diagnostics-storage-uri https://mystorageaccount.blob.core.windows.net/ 
    ```
   
    hello VM startas om under den här processen. Efter hello omstart befintlig OS- och datadiskar kommer att mappas om. Allt på hello diskutrymme går förlorade.
   
    Använd hello `--enable-boot-diagnostics` aktiverar [starta diagnostik][boot-diagnostics], toolog eventuella relaterade fel toostartup.
3. Hello önskad storlek inte visas, kör du annars hello följande kommandon toodeallocate hello VM, ändra storlek på den och sedan starta om hello VM.
   
    ```azurecli
    azure vm deallocate -g myResourceGroup myVM
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM \
        --enable-boot-diagnostics --boot-diagnostics-storage-uri \
        https://mystorageaccount.blob.core.windows.net/ 
    azure vm start -g myResourceGroup myVM
    ```
   
   > [!WARNING]
   > Det har frigjorts hello VM Frigör också alla dynamiska IP-adresser som tilldelats toohello VM. hello OS- och datadiskar påverkas inte.
   > 
   > 

## <a name="next-steps"></a>Nästa steg
För ytterligare utbyggbarhet köra flera VM-instanser och skala ut. Mer information finns i [skala automatiskt Linux-datorer i en virtuell dator Skaluppsättning][scale-set]. 

<!-- links -->

[azure-cli]:../../cli-install-nodejs.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
