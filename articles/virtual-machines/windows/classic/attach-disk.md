---
title: aaaAttach en disk tooa klassiska Azure-VM | Microsoft Docs
description: Koppla en data disk tooa Windows virtuell dator som skapats med hello klassiska distributionsmodellen och initiera den.
services: virtual-machines-windows, storage
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: be4e3e74-05bc-4527-969f-84f10a1d66a7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: cynthn
ms.openlocfilehash: bfe1b0fa066277d28d3862a18da4b1023cb4452d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="attach-a-data-disk-tooa-windows-virtual-machine-created-with-hello-classic-deployment-model"></a>Koppla en data disk tooa Windows virtuell dator som skapats med hello klassiska distributionsmodellen
<!--
Refernce article:
    If you want toouse hello new portal, see [How tooattach a data disk tooa Windows VM in hello Azure portal](../../virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

Den här artikeln visar hur tooattach nya och befintliga diskar skapas med hello klassisk distribution modellen tooa Windows virtuell dator med hello Azure-portalen.

Du kan också [bifoga en data disk tooa Linux VM i hello Azure-portalen](../../linux/attach-disk-portal.md).

Innan du ansluter en disk, granska de här tipsen:

* hello storleken på hello virtuella styr hur många datadiskar som du kan bifoga. Mer information finns i [storlekar för virtuella datorer](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* toouse Premium-lagring, behöver du en virtuell dator DS-serien eller GS-serien. Du kan använda diskar från Premium- och Standard storage-konton med dessa virtuella datorer. Premium-lagring är tillgänglig i vissa regioner. Mer information finns i [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* För en ny disk, behöver du inte toocreate den första eftersom Azure skapar den när du ansluter den.

Du kan också [ansluta en datadisk med hjälp av Powershell](../../virtual-machines-windows-attach-disk-ps.md).

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).

## <a name="find-hello-virtual-machine"></a>Hitta hello virtuell dator
1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Välj hello virtuell dator från hello resursen som visas på instrumentpanelen för hello.
3. I hello vänstra fönstret under **inställningar**, klickar du på **diskar**.

    ![Öppna diskinställningarna för](./media/attach-disk/virtualmachinedisks.png)

Fortsätt genom att följa anvisningarna för att bifoga antingen en [ny disk](#option-1-attach-a-new-disk) eller en [befintlig disk](#option-2-attach-an-existing-disk).

## <a name="option-1-attach-and-initialize-a-new-disk"></a>Alternativ 1: Anslut och initiera en ny disk

1. På hello **diskar** bladet, klickar du på **bifoga nya**.
2. Granska hello standardinställningarna, uppdatera efter behov och klicka sedan på **OK**.

   ![Granska diskinställningarna för](./media/attach-disk/attach-new.png)

3. När Azure skapar hello disk och bifogas toohello virtuella datorn, hello nya disken visas i hello virtuella datorns Diskinställningar under **Datadiskar**.

### <a name="initialize-a-new-data-disk"></a>Initiera en ny datadisk

1. Ansluta toohello virtuella datorn. Instruktioner finns i [hur tooconnect och logga in tooan Azure virtuella datorn kör Windows](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. När du loggar in toohello virtuella datorn, öppna **Serverhanteraren**. I hello vänster och välj **fil- och lagringstjänster**.

    ![Öppna Serverhanteraren](../media/attach-disk-portal/fileandstorageservices.png)

3. Välj **diskar**.
4. Hej **diskar** avsnitt visar hello diskar. En virtuell dator har mest ofta disk 0, disk 1 och disk 2. 0 är hello operativsystemdisk, 1 är hello tillfälliga disk och disk 2 är hello datadisk nyligen ansluten toohello virtuella datorn. hello data disk visar hello Partition som **okänd**.

 Högerklicka på hello disken och välj **initiera**.

5. Du meddelas att alla data raderas när hello disken initieras. Klicka på **Ja** tooacknowledge hello varning och initiera hello disk. När installationen är klar visas hello partition som **GPT**. Högerklicka på hello disken igen och välj **ny volym**.

6. Slutför guiden för hello med hello standardvärden. När hello guiden är klar hello **volymer** avsnittet visar hello ny volym. hello disken är nu online och klara toostore data.

    ![Volymen har startats](./media/attach-disk/newdiskafterinitialization.png)

## <a name="option-2-attach-an-existing-disk"></a>Alternativ 2: Bifoga en befintlig disk
1. På hello **diskar** bladet, klickar du på **bifoga befintliga**.
2. Under **bifoga den befintliga disken**, klickar du på **plats**.

   ![Bifoga den befintliga disken](./media/attach-disk/attachexistingdisksettings.png)
3. Under **lagringskonton**, väljer hello-konto och behållare som innehåller hello VHD-filen.

   ![Hitta VHD-plats](./media/attach-disk/existdiskstorageaccountandcontainer.png)

4. Välj hello VHD-filen.
5. Under **bifoga den befintliga disken**, hello-filen markerade anges under **VHD-filen**. Klicka på **OK**.
6. När Azure bifogar hello disk toohello virtuell dator, visas den i hello virtuella datorns Diskinställningar under **Datadiskar**.

## <a name="use-trim-with-standard-storage"></a>Använd Rensa med standardlagring

Om du använder standardlagring (HDD), bör du aktivera TRIMNING. TRIMNING ignorerar oanvända block på disken hello så att endast debiteras du för lagring som du faktiskt använder. Med TRIMNING spara kostnader, inklusive oanvända block som uppstår vid borttagning av stora filer.

Du kan köra kommandot toocheck hello TRIM inställningen. Öppna en kommandotolk på din Windows-VM och skriv:

```
fsutil behavior query DisableDeleteNotify
```

Om hello kommandot returnerar 0, är TRIMNING aktiverat på rätt sätt. Om den returnerar 1, kör du följande kommando tooenable TRIMNING hello:
```
fsutil behavior set DisableDeleteNotify 0
```

## <a name="next-steps"></a>Nästa steg
Om ditt program behöver toouse hello D: enhet toostore data, kan du [ändra hello enhetsbeteckningen för hello Windows diskutrymme](../../virtual-machines-windows-change-drive-letter.md).

## <a name="additional-resources"></a>Ytterligare resurser
[Om diskar och virtuella hårddiskar för virtuella datorer](../../virtual-machines-linux-about-disks-vhds.md)
