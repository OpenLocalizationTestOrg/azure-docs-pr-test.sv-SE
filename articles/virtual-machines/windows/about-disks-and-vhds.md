---
title: "Om ohanterad (sidblobbar) och hanterade diskar lagring för virtuella datorer i Microsoft Azure Windows | Microsoft Docs"
description: "Lär dig grunderna om ohanterad (sidblobbar) och hanterade diskar lagring för virtuella Windows-datorer i Azure."
services: virtual-machines
author: iainfoulds
manager: jeconnoc
ms.service: virtual-machines
ms.workload: storage
ms.tgt_pltfrm: windows
ms.topic: article
ms.date: 11/15/2017
ms.author: iainfou
ms.openlocfilehash: bf5c5cc0637b9a515bf567ff8933170d7fc1a8ba
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/10/2018
---
# <a name="about-disks-storage-for-azure-windows-vms"></a>Om diskar lagring för virtuella Azure Windows-datorer
Precis som andra dator använder virtuella datorer i Azure diskar som en plats för att lagra ett operativsystem, program och data. Alla Azure virtuella datorer har minst två diskar – en disk i Windows-operativsystem och en tillfällig disk. Operativsystemdisken har skapats från en avbildning och både operativsystemdisken och image är virtuella hårddiskar (VHD) lagras i ett Azure storage-konto. Virtuella datorer kan också ha en eller flera datadiskar som lagras också som virtuella hårddiskar. 

I den här artikeln ska vi pratar om olika användningsområden för diskar och beskrivs de olika typerna av diskar som du kan skapa och använda. Den här artikeln är också tillgängligt för [virtuella Linux-datorer](../linux/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Diskar som används av virtuella datorer

Låt oss ta en titt på hur diskarna som används av de virtuella datorerna.

### <a name="operating-system-disk"></a>Operativsystemdisk
Varje virtuell dator har en ansluten operativsystemdisk. Den har registrerats som en SATA-enhet och märkta som enhet C: som standard. Den här disken har en maximal kapacitet på 2 048 gigabyte (GB). 

### <a name="temporary-disk"></a>Diskutrymme
Varje virtuell dator innehåller en tillfällig disk. Den tillfälliga disken tillhandahåller kortsiktig lagring för program och processer och är avsedd att bara lagra data, till exempel sida eller växla filer. Data på den tillfälliga disken kan gå förlorade under en [underhållshändelse](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) eller när du [distribuera en virtuell dator](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Data på den tillfälliga enheten under en standard omstart av den virtuella datorn ska sparas.

Den tillfälliga disken är märkt som D: enhet som standard och det används för att lagra pagefile.sys. Om du vill mappa om den här disken till en annan enhetsbeteckning finns [ändra enhetsbeteckningen för den tillfälliga disken i Windows](change-drive-letter.md). Storleken på den tillfälliga disken beror på storleken på den virtuella datorn. Mer information finns i [storlekar för Windows-datorer](sizes.md).

Mer information om hur Azure använder temporär disk finns [förstå den tillfälliga enheten på virtuella datorer i Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)


### <a name="data-disk"></a>Datadisk
En datadisk är en virtuell Hårddisk som är kopplad till en virtuell dator kan lagra programdata och andra data som du behöver. Datadiskar registreras som SCSI-enheter och är märkta med en bokstav som du väljer. Varje datadisk har en maximal kapacitet för 4095 GB. Storleken på den virtuella datorn avgör hur många datadiskar som du kan ansluta till den och typen av lagring som du kan använda som värd för diskarna.

> [!NOTE]
> Mer information om kapacitet för virtuella datorer finns [storlekar för Windows-datorer](sizes.md).
> 

Azure skapar en operativsystemdisk när du skapar en virtuell dator från en avbildning. Om du använder en avbildning med datadiskar skapar Azure även datadiskar när den virtuella datorn skapas. Annars kan du lägga till datadiskar när du har skapat den virtuella datorn.

Du kan lägga till diskar till en virtuell dator när som helst av **kopplar** disken till den virtuella datorn. Du kan använda en virtuell Hårddisk som du har laddat upp eller kopieras till ditt lagringskonto eller använda en tom virtuell Hårddisk som Azure skapar åt dig. Koppla en datadisk associerar VHD-filen med den virtuella datorn genom att placera ett lån på den virtuella Hårddisken, så den inte kan tas bort från lagring när den är kopplad.


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a>En sista rekommendation: Använd TRIMNING med ohanterad standarddiskar 

Om du använder ohanterade standarddiskar (HDD), bör du aktivera TRIMNING. TRIMNING ignorerar oanvända block på disken så att endast debiteras du för lagring som du faktiskt använder. Om du skapar stora filer och ta bort dem kan detta spara på kostnader. 

Du kan köra det här kommandot för att kontrollera TRIM inställningen. Öppna en kommandotolk på din Windows-VM och skriv:


```
fsutil behavior query DisableDeleteNotify
```

Om kommandot returnerar 0, är TRIMNING aktiverat på rätt sätt. Om den returnerar 1, kör du följande kommando för att aktivera TRIMNING:

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> Obs: Stöd för Trim börjar med Windows Server 2012 / Windows 8 och senare finns i avsnittet [nya API: et kan appar skicka ”rensa och ta bort mappning” tips till lagringsmedia](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).
> 

<!-- Might want to match next-steps from overview of managed disks -->
## <a name="next-steps"></a>Nästa steg
* [Ansluta en disk](attach-disk-portal.md) att lägga till ytterligare lagringsutrymme för den virtuella datorn.
* [Skapa en ögonblicksbild](snapshot-copy-managed-disk.md).
* [Konvertera till hanterade diskar](convert-unmanaged-to-managed-disks.md).


