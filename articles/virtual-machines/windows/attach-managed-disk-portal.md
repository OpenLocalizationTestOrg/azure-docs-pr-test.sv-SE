---
title: aaaAttach en hanterad data disk tooa Windows VM - Azure | Microsoft Docs
description: Hur tooattach nya hanterade data disk tooa hello Windows VM i hello Azure portal med Resource Manager-distributionsmodellen.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: bacc0589ad2d93e4d3d055c8f837f8db27291ead
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-managed-data-disk-tooa-windows-vm-in-hello-azure-portal"></a>Hur tooattach hanterade data disk tooa Windows VM i hello Azure-portalen

Den här artikeln visar hur tooattach en nya hanterade data disk tooWindows virtuella datorer via hello Azure-portalen. Innan du gör detta kan du granska dessa tips:

* hello storleken på hello virtuella styr hur många datadiskar som du kan bifoga. Mer information finns i [storlekar för virtuella datorer](sizes.md).
* För en ny disk, behöver du inte toocreate den första eftersom Azure skapar den när du ansluter den.

Du kan också [ansluta en datadisk med hjälp av Powershell](attach-disk-ps.md).



## <a name="add-a-data-disk"></a>Lägg till en datadisk
1. Hello-menyn hello vänster **virtuella datorer**.
2. Välj hello virtuella hello-listan.
3. På hello virtuella bladet, klickar du på **diskar**.
   4. På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.
5. I hello kombinationsrutan hello ny disk, väljer **skapa tom**.
6. I hello **skapa hanterade diskar** bladet, Skriv ett namn för hello disken och justera hello övriga inställningar efter behov. När du är klar klickar du på **skapa**.
7. I hello **diskar** blad, klicka på Spara toosave hello nya diskkonfigurationen för hello VM.
6. När Azure skapar hello disk och bifogas toohello virtuella datorn, hello nya disken visas i hello virtuella datorns Diskinställningar under **Datadiskar**.


## <a name="initialize-a-new-data-disk"></a>Initiera en ny datadisk

1. Ansluta toohello VM.
1. Klicka på hello start-menyn i hello VM och ange **diskmgmt.msc** och träffar **RETUR**. Detta startar hello snapin-modulen Diskhantering.
2. Diskhantering identifierar att du har en ny, ej initierad disk och hello initiera Disk fönster visas.
3. Kontrollera att hello ny disk är markerad och klicka på **OK** tooinitialize den.
4. hello nya disken visas nu som **lediga**. Högerklicka på hello disk och välj **ny enkel volym**. Hej **guiden Ny enkel volym** startar.
5. Gå igenom guiden hello hålla alla standardvärden för hello när du är klar väljer **Slutför**.
6. Stäng Diskhantering.
7. Du får ett popup-fönster som du behöver tooformat hello nya disken innan du kan använda den. Klicka på **Formatera disk**.
8. I hello **Format ny disk** dialogrutan, kontrollera hello inställningar och klickar sedan på **starta**.
9. Du får en varning om att formatering hello diskar raderas alla hello data, klickar du på **OK**.
10. När hello format är klar klickar du på **OK**.

## <a name="use-trim-with-standard-storage"></a>Använd Rensa med standardlagring

Om du använder standardlagring (HDD), bör du aktivera TRIMNING. TRIMNING ignorerar oanvända block på disken hello så att endast debiteras du för lagring som du faktiskt använder. Om du skapar stora filer och ta bort dem kan detta spara på kostnader. 

Du kan köra kommandot toocheck hello TRIM inställningen. Öppna en kommandotolk på din Windows-VM och skriv:

```
fsutil behavior query DisableDeleteNotify
```

Om hello kommandot returnerar 0, är TRIMNING aktiverat på rätt sätt. Om den returnerar 1, kör du följande kommando tooenable TRIMNING hello:
```
fsutil behavior set DisableDeleteNotify 0
```

När data har tagits bort från disken, kan du kontrollera hello TRIM operations tömning korrekt genom att köra defragmenterar med TRIMNING:

```
defrag.exe <volume:> -l
```

Du kan också kontrollera hello hela volymen trimmas genom att formatera hello volym.

## <a name="next-steps"></a>Nästa steg
Om du program behöver toouse hello D: enhet toostore data, kan du [ändra hello enhetsbeteckningen för hello Windows diskutrymme](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
