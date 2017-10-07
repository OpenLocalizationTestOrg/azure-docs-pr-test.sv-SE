---
title: aaaAttach tooa en data-disk Linux VM | Microsoft Docs
description: Hur hello tooattach nya eller befintliga data disk tooa Linux VM i hello Azure portal med Resource Manager-distributionsmodellen.
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5e1c6212-976c-4962-a297-177942f90907
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: cynthn
ms.openlocfilehash: 069c3c6f5e71a8c495342e6d0c6f3d92c4ed5053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-data-disk-tooa-linux-vm-in-hello-azure-portal"></a>Hur tooattach data disk tooa Linux VM i hello Azure-portalen
Den här artikeln visar hur tooattach både nya och befintliga diskar tooa virtuell Linux-dator via hello Azure-portalen. Du kan också [bifoga en data disk tooa Windows VM i hello Azure-portalen](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Du kan välja toouse antingen Azure hanterade diskar eller är ohanterade diskar. Hanterade diskar som hanteras av hello Azure-plattformen och kräver inte någon förberedelse eller plats toostore dem. Ohanterad diskar krävs ett lagringskonto och har några [kvoter och gränser som gäller](../../azure-subscription-service-limits.md#storage-limits). Mer information om Azure Managed Disks finns i [Översikt över Azure Managed Disks](../windows/managed-disks-overview.md).

Granska de här tipsen innan du kopplar diskar tooyour VM:

* hello storleken på hello virtuella styr hur många datadiskar som du kan bifoga. Mer information finns i [storlekar för virtuella datorer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* toouse Premium-lagring, behöver du en virtuell dator DS-serien eller GS-serien. Du kan använda Premium- och Standard diskar med dessa virtuella datorer. Premium-lagring är tillgänglig i vissa regioner. Mer information finns i [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Diskar anslutna toovirtual datorer är faktiskt VHD-filer som är lagrade i Azure. Mer information finns i [om diskar och virtuella hårddiskar för virtuella datorer](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="find-hello-virtual-machine"></a>Hitta hello virtuell dator
1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hej hubbmenyn, klicka på **virtuella datorer**.
3. Välj hello virtuella hello-listan.
4. toohello virtuella datorer bladet i **Essentials**, klickar du på **diskar**.
   
    ![Öppna diskinställningarna för](./media/attach-disk-portal/find-disk-settings.png)

Fortsätt genom att följa anvisningarna för att bifoga antingen en [hanterade diskar](#use-azure-managed-disks) eller [ohanterade disk](#use-unmanaged-disks).

## <a name="use-azure-managed-disks"></a>Använda Azure hanterade diskar

### <a name="attach-a-new-disk"></a>Koppla en ny disk

1. På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.
2. Klicka på hello nedrullningsbara menyn för **namn** och välj **skapa disk**:

    ![Skapa Azure hanterade diskar](./media/attach-disk-portal/create-new-md.png)

3. Ange ett namn för din hanterade disken. Granska hello standardinställningarna, uppdatera efter behov och klicka sedan på **skapa**.
   
   ![Granska diskinställningarna för](./media/attach-disk-portal/create-new-md-settings.png)

4. Klicka på **spara** toocreate hello hanterade disken och uppdatera hello VM-konfiguration:

   ![Spara nya Azure hanterade disken](./media/attach-disk-portal/confirm-create-new-md.png)

5. När Azure skapar hello disk och bifogas toohello virtuella datorn, hello nya disken visas i hello virtuella datorns Diskinställningar under **Datadiskar**. Eftersom hanterade diskar är en översta resurs, visas hello disken i hello rot hello resursgrupp:

   ![Azure-hanterade disken i resursgruppen](./media/attach-disk-portal/view-md-resource-group.png)

### <a name="attach-an-existing-disk"></a>Ansluta en befintlig disk
1. På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.
2. Klicka på hello nedrullningsbara menyn för **namn** tooview en lista över befintliga hanterade diskar tillgänglig tooyour Azure-prenumeration. Välj hello hanterade disken tooattach:

   ![Bifoga den befintliga Azure hanterade disken](./media/attach-disk-portal/select-existing-md.png)

3. Klicka på **spara** tooattach hello befintliga hanterade disken och uppdatera hello VM-konfiguration:
   
   ![Spara Azure hanteras Disk uppdateringar](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. När Azure bifogar hello disk toohello virtuell dator, visas den i hello virtuella datorns Diskinställningar under **Datadiskar**.

## <a name="use-unmanaged-disks"></a>Använda ohanterade diskar

### <a name="attach-a-new-disk"></a>Koppla en ny disk

1. På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.
2. Granska hello standardinställningarna, uppdatera efter behov och klicka sedan på **OK**.
   
   ![Granska diskinställningarna för](./media/attach-disk-portal/attach-new.png)
3. När Azure skapar hello disk och bifogas toohello virtuella datorn, hello nya disken visas i hello virtuella datorns Diskinställningar under **Datadiskar**.

### <a name="attach-an-existing-disk"></a>Ansluta en befintlig disk
1. På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.
2. Under **bifoga den befintliga disken**, klickar du på **VHD-filen**.
   
   ![Bifoga den befintliga disken](./media/attach-disk-portal/attach-existing.png)
3. Under **lagringskonton**, väljer hello-konto och behållare som innehåller hello VHD-filen.
   
   ![Hitta VHD-plats](./media/attach-disk-portal/find-storage-container.png)
4. Välj hello VHD-filen.
5. Under **bifoga den befintliga disken**, hello-filen markerade anges under **VHD-filen**. Klicka på **OK**.
6. När Azure bifogar hello disk toohello virtuell dator, visas den i hello virtuella datorns Diskinställningar under **Datadiskar**.


## <a name="next-steps"></a>Nästa steg
När hello disken läggs tooprepare måste den för användning. Mer information finns i [så här: initiera en ny datadisk i Linux](add-disk.md).
