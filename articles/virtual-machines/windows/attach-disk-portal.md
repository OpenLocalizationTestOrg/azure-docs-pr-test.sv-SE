---
title: Ansluta en ohanterad datadisk till en Windows-VM - Azure | Microsoft Docs
description: "Så här kopplar du ny eller befintlig ohanterade datadisk till en virtuell Windows-dator i Azure-portalen med hjälp av Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: c0886302c82641f8cc1a00d3972870d58ba8afb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-attach-an-unmanaged-data-disk-to-a-windows-vm-in-the-azure-portal"></a>Hur du kopplar en ohanterad datadisk till en virtuell Windows-dator i Azure-portalen

Den här artikeln visar hur du kopplar både nya och befintliga ohanterade diskar till Windows-datorer via Azure-portalen. Du kan också [ansluta en datadisk med hjälp av PowerShell](./attach-disk-ps.md). Innan du gör detta kan du granska dessa tips:

* Storleken på den virtuella datorn styr hur många datadiskar som du kan bifoga. Mer information finns i [storlekar för virtuella datorer](sizes.md).
* Om du vill använda Premium-lagring, behöver du en virtuell dator DS-serien eller GS-serien. Du kan använda diskar från Premium- och Standard storage-konton med dessa virtuella datorer. Premium-lagring är tillgänglig i vissa regioner. Mer information finns i [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* För en ny disk behöver du inte skapa först eftersom Azure skapar den när du ansluter den.


Du kan också [ansluta en datadisk med hjälp av Powershell](attach-disk-ps.md).


## <a name="find-the-virtual-machine"></a>Hitta den virtuella datorn
1. Logga in på [Azure Portal](https://portal.azure.com/).
2. Klicka på menyn till vänster **virtuella datorer**.
3. Välj den virtuella datorn i listan.
4. I bladet för virtuella datorer, klickar du på **diskar**.
   
Fortsätt genom att följa anvisningarna för att bifoga antingen en [ny disk](#option-1-attach-a-new-disk) eller en [befintlig disk](#option-2-attach-an-existing-disk).

## <a name="option-1-attach-and-initialize-a-new-disk"></a>Alternativ 1: Anslut och initiera en ny disk
1. På den **diskar** bladet, klickar du på **+ Lägg till datadisk**.
2. I den **bifoga hanterade diskar** bladet, ange ett namn för disken i **namn** och välj sedan **ny (tom disk)** i **typ av datakälla**.
3. Under **lagringsbehållaren**, klicka på den **Bläddra** knappen och bläddra till lagringskontot och behållare där du vill att den nya virtuella Hårddisken lagras och klicka sedan på **Välj**. 
  
   ![Granska diskinställningarna för](./media/attach-disk-portal/attach-empty-unmanaged.png)
   
3. När du är klar med inställningarna för datadisken klickar du på **OK**.
4. I den **diskar** bladet, klickar du på **spara** att lägga till disken i VM-konfiguration.


### <a name="initialize-a-new-data-disk"></a>Initiera en ny datadisk

1. Ansluta till den virtuella datorn. Instruktioner finns i [ansluta och logga in på en virtuell Azure-dator kör Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
1. Klicka på den **starta** menyn inuti den virtuella datorn och skriv **diskmgmt.msc** och träffar **RETUR**. Detta startar snapin-modulen Diskhantering.
2. Diskhantering märker att du har en ny, oinitierad disk och initiera Disk-fönstret visas.
3. Kontrollera att den nya disken är markerad och klicka på **OK** att initiera den.
4. Den nya disken visas nu som **lediga**. Högerklicka på disken och välj **ny enkel volym**. Den **guiden Ny enkel volym** startar.
5. Gå igenom guiden och hålla alla standardinställningar, när du är klar väljer **Slutför**.
6. Stäng Diskhantering.
7. Du får ett popup-fönster som du vill formatera den nya disken innan du kan använda den. Klicka på **Formatera disk**.
8. I den **Format ny disk** dialogrutan Kontrollera inställningarna och klicka sedan på **starta**.
9. Du får en varning om att diskarna formateras tas alla data, klickar på **OK**.
10. När formatet är klar klickar du på **OK**.


## <a name="option-2-attach-an-existing-disk"></a>Alternativ 2: Bifoga en befintlig disk
1. På den **diskar** bladet, klickar du på **+ Lägg till datadisk**.
2. På den **bifoga ohanterade disk** bladet i **typ av datakälla** Välj **befintlig blob**.

    ![Granska diskinställningarna för](./media/attach-disk-portal/attach-existing-unmanaged.png)

    3. Klicka på **Bläddra** att navigera till lagringskontot och där det finns befintliga VHD-behållare. Klicka på och VHD- och klicka sedan på **Välj**.
4. Klicka på **OK** i den **bifoga ohanterade disk** bladet.
5. I den **diskar** bladet, klickar du på **spara** att lägga till disken i konfigurationen för den virtuella datorn.
   


## <a name="use-trim-with-standard-storage"></a>Använd Rensa med standardlagring

Om du använder standardlagring (HDD), bör du aktivera TRIMNING. TRIMNING ignorerar oanvända block på disken så att endast debiteras du för lagring som du faktiskt använder. Om du skapar stora filer och ta bort dem kan detta spara på kostnader. 

Du kan köra det här kommandot för att kontrollera TRIM inställningen. Öppna en kommandotolk på din Windows-VM och skriv:

```
fsutil behavior query DisableDeleteNotify
```

Om kommandot returnerar 0, är TRIMNING aktiverat på rätt sätt. Om den returnerar 1, kör du följande kommando för att aktivera TRIMNING:
```
fsutil behavior set DisableDeleteNotify 0
```

När du tar bort data från disken, kan du se till åtgärderna TRIM tömning korrekt genom att köra defragmenterar med TRIMNING:

```
defrag.exe <volume:> -l
```

Du kan också kontrollera hela volymen trimmas genom att formatera volymen.


## <a name="next-steps"></a>Nästa steg
Om du program behöver använda D: enhet för att lagra data, kan du [ändra enhetsbeteckningen för den tillfälliga disken i Windows](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

