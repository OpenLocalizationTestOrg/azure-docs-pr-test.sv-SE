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
# <a name="how-toodetach-a-data-disk-from-a-linux-virtual-machine"></a>Hur toodetach en disk från en virtuell Linux-dator

När du inte längre behöver en datadisk som är bifogade tooa virtuell dator kan du enkelt kan koppla från den. Detta tar bort hello disk från hello virtuell dator, men ta bort inte den från lagring. 

> [!WARNING]
> Om du koppla från en disk som den inte tas bort automatiskt. Om du har prenumererat tooPremium lagring, fortsätter tooincur lagringskostnaderna för hello disk. Mer information finns för[priser och fakturering när du använder Premiumlagring](../../storage/common/storage-premium-storage.md#pricing-and-billing). 
> 
> 

Om du vill toouse hello befintliga data på hello disk, du kan återansluta den toohello samma virtuella dator eller en annan.  

## <a name="detach-a-data-disk-using-cli-20"></a>Koppla från en datadisk med hjälp av CLI 2.0

```azurecli
az vm disk detach -g myResourceGroup --vm-name myVm -n myDataDisk
```

hello disk kvar i lagring men är inte längre kopplade tooa virtuell dator.


## <a name="detach-a-data-disk-using-hello-portal"></a>Koppla från en datadisk med hjälp av hello portal
1. I portalen hello-hubb väljer **virtuella datorer**.
2. Välj hello virtuella dator som har hello datadisk toodetach och på **stoppa** toodeallocate hello VM.
3. Hello virtuella bladet välj **diskar**.
4. Hello överst i hello **diskar** bladet väljer **redigera**.
5. I hello **diskar** bladet toohello högerkanten på hello datadisk som du vill att toodetach, klicka på hello ![Detach bilden](./media/detach-disk/detach.png) frånkoppling knappen.
5. Klicka på Spara hello längst upp på bladet hello när hello disken har tagits bort.
6. I hello virtuella bladet, klickar du på **översikt** och klicka sedan på hello **starta** knappen hello överst i hello bladet toorestart hello VM.

hello disk kvar i lagring men är inte längre kopplade tooa virtuell dator.








## <a name="next-steps"></a>Nästa steg
Om du vill tooreuse hello datadisk du just [bifoga den tooanother VM](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

