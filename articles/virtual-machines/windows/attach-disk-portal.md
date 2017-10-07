---
title: aaaAttach en ohanterad data disk tooa Windows VM - Azure | Microsoft Docs
description: Hur hello tooattach ny eller befintlig ohanterade data disk tooa Windows VM i hello Azure portal med Resource Manager-distributionsmodellen.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3790fc59-7264-41df-b7a3-8d1226799885
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: 923a6663974143bf359970f9b0eb0d36cabcba9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-an-unmanaged-data-disk-tooa-windows-vm-in-hello-azure-portal"></a>Hur tooattach en ohanterad data disk tooa Windows VM i hello Azure-portalen

Den här artikeln lär du dig hur tooattach både nya och befintliga ohanterad diskar tooWindows virtuella datorer via hello Azure-portalen. Du kan också [ansluta en datadisk med hjälp av PowerShell](./attach-disk-ps.md). Innan du gör detta kan du granska dessa tips:

* hello storleken på hello virtuella styr hur många datadiskar som du kan bifoga. Mer information finns i [storlekar för virtuella datorer](sizes.md).
* toouse Premium-lagring, behöver du en virtuell dator DS-serien eller GS-serien. Du kan använda diskar från Premium- och Standard storage-konton med dessa virtuella datorer. Premium-lagring är tillgänglig i vissa regioner. Mer information finns i [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* För en ny disk, behöver du inte toocreate den första eftersom Azure skapar den när du ansluter den.


Du kan också [ansluta en datadisk med hjälp av Powershell](attach-disk-ps.md).


## <a name="find-hello-virtual-machine"></a>Hitta hello virtuell dator
1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello-menyn hello vänster **virtuella datorer**.
3. Välj hello virtuella hello-listan.
4. I bladet för virtuella datorer av hello klickar du på **diskar**.
   
Fortsätt genom att följa anvisningarna för att bifoga antingen en [ny disk](#option-1-attach-a-new-disk) eller en [befintlig disk](#option-2-attach-an-existing-disk).

## <a name="option-1-attach-and-initialize-a-new-disk"></a>Alternativ 1: Anslut och initiera en ny disk
1. På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.
2. I hello **bifoga hanterade diskar** bladet, ange ett namn för hello disk i **namn** och välj sedan **ny (tom disk)** i **typ av datakälla**.
3. Under **lagringsbehållaren**, klicka på hello **Bläddra** knappen och bläddra toohello storage-konto och en behållare där du som hello nya VHD-toobe lagras och klicka sedan på **Välj**. 
  
   ![Granska diskinställningarna för](./media/attach-disk-portal/attach-empty-unmanaged.png)
   
3. När du är klar med hello inställningar för hello datadisk klickar du på **OK**.
4. Tillbaka i hello **diskar** bladet, klickar du på **spara** tooadd hello disk toohello VM-konfiguration.


### <a name="initialize-a-new-data-disk"></a>Initiera en ny datadisk

1. Ansluta toohello virtuella datorn. Instruktioner finns i [hur tooconnect och logga in tooan Azure virtuella datorn kör Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
1. Klicka på hello **starta** menyn i hello VM och skriva **diskmgmt.msc** och träffar **RETUR**. Detta startar hello snapin-modulen Diskhantering.
2. Diskhantering märker att du har en ny, oinitierad disk och hello initiera Disk fönster visas.
3. Kontrollera att hello ny disk är markerad och klicka på **OK** tooinitialize den.
4. hello nya disken visas nu som **lediga**. Högerklicka på hello disk och välj **ny enkel volym**. Hej **guiden Ny enkel volym** startar.
5. Gå igenom guiden hello hålla alla standardvärden för hello när du är klar väljer **Slutför**.
6. Stäng Diskhantering.
7. Du får ett popup-fönster som du behöver tooformat hello nya disken innan du kan använda den. Klicka på **Formatera disk**.
8. I hello **Format ny disk** dialogrutan, kontrollera hello inställningar och klickar sedan på **starta**.
9. Du får en varning om att hello diskar formateras alla hello data, klickar på **OK**.
10. När hello format är klar klickar du på **OK**.


## <a name="option-2-attach-an-existing-disk"></a>Alternativ 2: Bifoga en befintlig disk
1. På hello **diskar** bladet, klickar du på **+ Lägg till datadisk**.
2. På hello **bifoga ohanterade disk** bladet i **typ av datakälla** Välj **befintlig blob**.

    ![Granska diskinställningarna för](./media/attach-disk-portal/attach-existing-unmanaged.png)

    3. Klicka på **Bläddra** toonavigate toohello storage-konto och behållare där hello befintlig virtuell Hårddisk finns. Klicka på hello VHD och klicka sedan på **Välj**.
4. Klicka på **OK** i hello **bifoga ohanterade disk** bladet.
5. I hello **diskar** bladet, klickar du på **spara** tooadd hello diskkonfigurationen toohello för hello VM.
   


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

