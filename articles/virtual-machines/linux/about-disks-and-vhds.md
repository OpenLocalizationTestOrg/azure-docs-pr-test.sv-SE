---
title: "aaaAbout diskar och virtuella hårddiskar för virtuella datorer i Microsoft Azure Linux | Microsoft Docs"
description: "Läs mer om hello grunderna för diskar och virtuella hårddiskar för Linux virtuella datorer i Azure."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 7be8dd52-98f7-4187-9b78-55197915bc9b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: 862217e4f15ff8fd2e08de71386c4f367d0c39db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="about-disks-and-vhds-for-azure-linux-vms"></a>Om diskar och virtuella hårddiskar för virtuella Azure Linux-datorer
Precis som alla andra datorer använda virtuella datorer i Azure diskar som plats-toostore ett operativsystem, program och data. Alla Azure virtuella datorer har minst två diskar – en disk för Linux-operativsystem och en tillfällig disk. hello operativsystemdisk skapas från en avbildning och både hello operativsystemdisken och hello avbildningen är faktiskt virtuella hårddiskar (VHD) lagras i ett Azure storage-konto. Virtuella datorer kan också ha en eller flera datadiskar som lagras också som virtuella hårddiskar. 

I den här artikeln ska vi pratar om hello olika användningsområden för hello diskar och diskutera hello olika typer av diskar som du kan skapa och använda. Den här artikeln är också tillgängligt för [virtuella Windows-datorer](../windows/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Diskar som används av virtuella datorer

Låt oss ta en titt på hur hello diskarna används av hello virtuella datorer.

## <a name="operating-system-disk"></a>Operativsystemdisk
Varje virtuell dator har en ansluten operativsystemdisk. Den är registrerad som en SATA-enhet och etiketteras /dev/sda som standard. Den här disken har en maximal kapacitet på 2 048 gigabyte (GB). 

## <a name="temporary-disk"></a>Diskutrymme
Varje virtuell dator innehåller en tillfällig disk. hello diskutrymme tillhandahåller kortsiktig lagring för program och processer och är avsedd tooonly lagra data, till exempel page eller växlingen filer. Data på hello diskutrymme kan gå förlorade under en [underhållshändelse](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) eller när du [distribuera en virtuell dator](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Under en standard omstart av hello VM ska hello data på hello temporärkatalog sparas.

På virtuella Linux-datorer hello disken är vanligtvis **/dev/sdb** har formaterats och monterade för**/mnt** av hello Azure Linux-agenten. hello storleken på hello diskutrymme varierar utifrån hello storleken på hello virtuella datorn. Mer information finns i [storlekar för virtuella Linux-datorer](../windows/sizes.md).

Mer information om hur Azure använder hello diskutrymme finns [förstå hello tillfälliga enheten på virtuella datorer i Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Datadisk
En datadisk är en virtuell Hårddisk som är bifogade tooa virtuella toostore programdata eller andra data som du behöver tookeep. Datadiskar registreras som SCSI-enheter och är märkta med en bokstav som du väljer. Varje datadisk har en maximal kapacitet för 4095 GB. hello storleken på hello virtuella avgör hur många datadiskar som du kan koppla tooit och hello typer av lagring som du kan använda toohost hello diskar.

> [!NOTE]
> Mer information om kapacitet för virtuella datorer finns [storlekar för virtuella Linux-datorer](../windows/sizes.md).
> 

Azure skapar en operativsystemdisk när du skapar en virtuell dator från en avbildning. Om du använder en avbildning som innehåller datadiskar skapar Azure även hello datadiskar när hello virtuell dator skapas. Annars kan du lägga till datadiskar när du skapar hello virtuell dator.

Du kan lägga till data diskar tooa virtuell dator när som helst av **kopplar** hello disk toohello virtuella datorn. Du kan använda en virtuell Hårddisk som du har laddat upp eller kopieras tooyour storage-konto eller något som Azure skapar åt dig. Koppla en datadisk associerar hello VHD-filen med hello VM, genom att placera ett lån på hello VHD så den inte kan tas bort från lagring när den är fortfarande kopplad.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="troubleshooting"></a>Felsökning
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Nästa steg
* [Ansluta en disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooadd ytterligare lagringsutrymme för den virtuella datorn.
* [Konfigurera programvarubaserad RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för redundans.
* [Avbilda en virtuell Linux-dator](./classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) så att du kan snabbt distribuera ytterligare virtuella datorer.

