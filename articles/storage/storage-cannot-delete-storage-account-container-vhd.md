---
title: "tar bort Azure storage-konton, behållare eller virtuella hårddiskar i en klassisk distribution aaaTroubleshoot | Microsoft Docs"
description: "Felsöka tar bort Azure storage-konton, behållare eller virtuella hårddiskar i en klassisk distribution"
services: storage
documentationcenter: 
author: genlin
manager: felixwu
editor: tysonn
tags: storage
ms.assetid: 0f7a8243-d8dc-432a-9d37-1272a0cb3a5c
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 6bbfa032e1968718c623227bb426d553e2951075
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Felsöka tar bort Azure storage-konton, behållare eller virtuella hårddiskar i en klassisk distribution
[!INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Fel kan visas när du försöker toodelete hello Azure storage-konto, behållare eller VHD i hello [Azure-portalen](https://portal.azure.com/) eller hello [klassiska Azure-portalen](https://manage.windowsazure.com/). hello problem kan orsakas av hello följande omständigheter:

* När du tar bort en virtuell dator tas hello disk och VHD inte bort automatiskt. Som kan vara hello orsaken till felet på borttagning av storage-konto. Vi ta inte bort hello disk så att du kan använda hello disk toomount en annan virtuell dator.
* Det finns fortfarande ett lån på en disk eller hello blob som är kopplad till hello disk.
* Det finns fortfarande en VM-avbildning som använder ett konto för blob-behållare eller lagring.

Om din Azure problemet inte åtgärdas i den här artikeln, gå hello Azure-forum på [MSDN och hello Stack Overflow](https://azure.microsoft.com/support/forums/). Du kan publicera problemet på dessa forum eller too@AzureSupport på Twitter. Du kan också filen en Azure-supportbegäran genom att välja **få support** på hello [Azure-supporten](https://azure.microsoft.com/support/options/) plats.

## <a name="symptoms"></a>Symtom
hello efter avsnittet listas vanliga fel som kan visas när du försöker toodelete hello Azure storage-konton, behållare eller virtuella hårddiskar.

### <a name="scenario-1-unable-toodelete-a-storage-account"></a>Scenario 1: Toodelete ett lagringskonto
När du navigerar toohello klassiska storage-konto i hello [Azure-portalen](https://portal.azure.com/) och välj **ta bort**, visas en lista med objekt som hindrar borttagning av hello storage-konto:

  ![Bild av fel när tar bort hello Storage-konto](./media/storage-cannot-delete-storage-account-container-vhd/newerror.png)

När du navigerar toohello storage-konto i hello [klassiska Azure-portalen](https://manage.windowsazure.com/) och välj **ta bort**, något av följande fel hello kan visas:

- *Lagringskontot StorageAccountName innehåller VM-avbildningar. Kontrollera att dessa VM-avbildningar avlägsnas innan lagringskontot.*

- *Det gick inte toodelete storage-konto < vm-storage-konto-name >. Det går inte toodelete storage-konto < vm-storage-konto-name > ”: Storage-konto < vm-storage-konto-name > har vissa aktiva avbildningar och/eller diskar. Kontrollera att dessa bilder och/eller diskar avlägsnas innan lagringskontot.'.*

- *Storage-konto < vm-storage-konto-name > har vissa aktiva avbildningar och/eller diskar, t.ex. xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Se till att dessa avbildningar och/eller diskarna har tagits bort innan du tar bort det här lagringskontot.*

- *Storage-konto < vm-storage-konto-name > har 1 behållare som har en aktiv avbildning och/eller disk artefakter. Kontrollera att dessa artefakter avlägsnas från avbildningslagringsplatsen hello innan du tar bort det här lagringskontot*.

- *Skicka misslyckades Storage-konto < vm-storage-konto-name > har 1 behållare som har en aktiv avbildning och/eller disk artefakter. Se till att dessa artefakter avlägsnas från avbildningslagringsplatsen hello innan du tar bort det här lagringskontot. När du försöker toodelete ett lagringskonto och det finns fortfarande aktiva diskar som är kopplade till den, visas ett meddelande om det finns aktiva diskar som behöver toobe bort*.

### <a name="scenario-2-unable-toodelete-a-container"></a>Scenario 2: Det gick inte toodelete en behållare
När du försöker toodelete hello lagringsbehållaren, kan du se hello följande fel:

*Misslyckade toodelete lagringsbehållaren <container name>. Fel ”: det finns för närvarande ett lån på hello behållare och inget lease-ID har angetts i begäran hello*.

Eller

*hello följande virtuella diskar som använder blobbar i den här behållaren så hello behållaren inte kan tas bort: VirtualMachineDiskName1, VirtualMachineDiskName2,...*

### <a name="scenario-3-unable-toodelete-a-vhd"></a>Scenario 3: Det gick inte toodelete en virtuell Hårddisk
När du tar bort en virtuell dator och sedan försöka toodelete hello BLOB för hello tillhörande virtuella hårddiskar, kan du få hello följande meddelande:

*Misslyckade toodelete blob-sökvägen/XXXXXX-XXXXXX-os-1447379084699.vhd'. Fel ”: det finns för närvarande ett lån på hello blob och inget lease-ID har angetts i hello-begäran.*

Eller

*BLOB BlobName.vhd används som virtuell disk VirtualMachineDiskName, så hello blob inte kan tas bort.*

## <a name="solution"></a>Lösning
tooresolve hello de flesta vanliga problem, försök hello följande metod:

### <a name="step-1-delete-any-disks-that-are-preventing-deletion-of-hello-storage-account-container-or-vhd"></a>Steg 1: Ta bort diskar som hindrar borttagning av hello storage-konto, behållare eller virtuell Hårddisk
1. Växla toohello [klassiska Azure-portalen](https://manage.windowsazure.com/).
2. Välj **VIRTUELLA** > **DISKAR**.

    ![Bild av diskar på virtuella datorer på den klassiska Azure-portalen.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)
3. Leta upp hello diskar som är associerade med hello storage-konto, behållare eller VHD som du vill toodelete. När du markerar hello platsen för hello disk hittar du hello associerade lagringskontot, behållare eller VHD.

    ![Bild som visar information om diskar på den klassiska Azure-portalen](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)
4. Ta bort hello diskar med någon av följande metoder hello:

  - Om det finns ingen VM finns på hello **ansluten till** fält av hello disk kan du ta bort hello disk direkt.

  - Om hello är en datadisk, gör du följande:

    1. Kontrollera hello namn hello VM som hello disk som är kopplad till.
    2. Gå för**virtuella datorer** > **instanser**, och leta sedan upp hello VM.
    3. Kontrollera att inget aktivt använder hello disk.
    4. Välj **koppla från Disk** längst hello hello portal toodetach hello disk.
    5. Gå för**virtuella datorer** > **diskar**, och vänta tills hello **ansluten till** fältet tooturn tomt. Detta anger hello disk har kopplats från hello VM.
    6. Välj **ta bort** längst hello **virtuella datorer** > **diskar** toodelete hello disk.

  - Om hello är en OS-disk (hello **innehåller OS** fältet har ett värde som Windows) och anslutna tooa VM, Följ dessa steg toodelete hello VM. kan inte frånkopplas hello OS-disken så att vi har toodelete hello VM toorelease hello lån.

    1. Kontrollera hello namn hello virtuella hello datadisk är kopplad till.  
    2. Gå för**virtuella datorer** > **instanser**, och sedan väljer hello VM som hello disk är kopplad till.
    3. Kontrollera att inget aktivt använder hello virtuella datorn och att du inte längre behöver hello virtuell dator.
    4. Välj hello VM hello disken är ansluten till, välj sedan **ta bort** > **ta bort hello anslutna diskar**.
    5. Gå för**virtuella datorer** > **diskar**, och vänta tills hello disk toodisappear.  Det kan ta några minuter för den här toooccur och du kan behöva toorefresh hello sidan.
    6. Om hello disk inte försvinner väntar hello **ansluten till** fältet tooturn tomt. Detta anger hello disk har helt oberoende från hello VM.  Sedan, Välj hello disken och markera **ta bort** längst hello hello sidan toodelete hello disk.


   > [!NOTE]
   > Om en disk är anslutna tooa VM kan du inte kan toodelete den. Diskar har kopplats bort från en borttagen virtuell dator asynkront. Det kan ta några minuter efter hello VM har tagits bort för det här fältet tooclear upp.
   >
   >


### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-hello-storage-account-or-container"></a>Steg 2: Ta bort eventuella VM-avbildningar som hindrar borttagning av hello storage-konto eller behållaren
1. Växla toohello [klassiska Azure-portalen](https://manage.windowsazure.com/).
2. Välj **VIRTUELLA** > **bilder**, och ta sedan bort hello-avbildningar som är associerade med hello storage-konto, behållare eller VHD.

    Sedan kan försöka toodelete hello storage-konto, behållare eller VHD.

> [!WARNING]
> Vara säker på att tooback in något du toosave innan du tar bort hello-kontot. När du tar bort en virtuell Hårddisk, blob, tabell, kö eller fil bort den permanent. Se till att resursen hello inte används.
>
>

## <a name="about-hello-stopped-deallocated-status"></a>Om hello Stoppad (frigjord) status
Virtuella datorer som har skapats i hello klassiska distributionsmodellen och som har behållits har hello **Stoppad (frigjord)** status på antingen hello [Azure-portalen](https://portal.azure.com/) eller [klassiska Azure-portalen ](https://manage.windowsazure.com/).

**Klassiska Azure-portalen**:

![Stoppad (Deallocated) status för virtuella datorer på Azure-portalen.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)

**Azure-portalen**:

![Stoppats (frigjorts) status för virtuella datorer på den klassiska Azure-portalen.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

Statusen ”Stoppad (frigjord)” Frigör hello datorresurser, till exempel hello CPU, minne och nätverk. hello diskar, men är fortfarande kvar så att du snabbt återskapa hello VM om det behövs. Diskarna har skapats på virtuella hårddiskar som backas upp av Azure storage. hello storage-konto har dessa virtuella hårddiskar och hello diskar har lån på de virtuella hårddiskarna.

## <a name="next-steps"></a>Nästa steg
* [Ta bort ett lagringskonto](storage-create-storage-account.md#delete-a-storage-account)
