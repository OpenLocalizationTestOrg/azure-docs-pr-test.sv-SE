---
title: "aaaTroubleshoot fel när du tar bort Azure storage-konton, behållare eller virtuella hårddiskar | Microsoft Docs"
description: "Felsöka när du tar bort Azure storage-konton, behållare eller virtuella hårddiskar"
services: storage
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: storage
ms.assetid: 17403aa1-fe8d-45ec-bc33-2c0b61126286
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 77361593e2c924d39aba853e0807dc3188f50e60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds"></a>Felsöka när du tar bort Azure storage-konton, behållare eller virtuella hårddiskar

Du kan få fel när du försöker toodelete ett Azure storage-konto, en behållare eller en virtuell hårddisk (VHD) med hello [Azure-portalen](https://portal.azure.com). Den här artikeln innehåller felsökning vägledning toohelp Lös hello problem i en Azure Resource Manager distribution.

Om den här artikeln inte åtgärda problemet Azure, gå hello Azure-forum på [MSDN och Stack Overflow](https://azure.microsoft.com/support/forums/). Du kan publicera ditt problem på dessa forum eller too@AzureSupport på Twitter. Du kan också filen en Azure-supportbegäran genom att välja **få support** på hello [Azure-supporten](https://azure.microsoft.com/support/options/) plats.

## <a name="symptoms"></a>Symtom
### <a name="scenario-1"></a>Scenario 1
När du försöker toodelete en virtuell Hårddisk i ett lagringskonto i en Resource Manager distribution visas hello följande felmeddelande:

**Det gick inte toodelete blob 'vhds/BlobName.vhd'. Fel: Det finns för närvarande ett lån på hello blob och inget lease-ID har angetts i hello-begäran.**

Det här problemet kan uppstå eftersom en virtuell dator (VM) har ett lån på hello VHD som du försöker toodelete.

### <a name="scenario-2"></a>Scenario 2
När du försöker toodelete en behållare i ett lagringskonto i en Resource Manager distribution visas hello följande felmeddelande:

**Det gick inte virtuella hårddiskar' toodelete lagring behållare'. Fel: Det finns för närvarande ett lån på hello behållare och inget lease-ID har angetts i hello-begäran.**

Det här problemet kan inträffa eftersom hello behållaren har en virtuell Hårddisk som är låsta i hello lease-tillstånd.

### <a name="scenario-3"></a>Scenario 3
Hello följande felmeddelande visas när toodelete ett lagringskonto i en Resource Manager distribution:

**Det gick inte toodelete storage-konto 'StorageAccountName'. Fel: hello storage-konto kan inte tas bort på grund av tooits artefakter används.**

Det här problemet kan inträffa eftersom hello storage-konto innehåller en virtuell Hårddisk som är i hello lease-tillstånd.

## <a name="solution"></a>Lösning 
tooresolve dessa problem måste du identifiera hello VHD som orsakar fel hello och hello associerade VM. Sedan koppla hello VHD från hello VM (för datadiskar) eller ta bort hello VM som använder hello VHD (för OS-diskar). Det tar bort hello lån från hello VHD och gör den toobe tas bort. 

toodo denna, Använd en av följande metoder hello:

### <a name="method-1---use-azure-storage-explorer"></a>Metod 1 - Använd Azure Lagringsutforskaren

### <a name="step-1-identify-hello-vhd-that-prevent-deletion-of-hello-storage-account"></a>Steg 1 identifiera hello VHD som förhindra borttagning av hello storage-konto

1. När du tar bort hello storage-konto visas en dialogruta för meddelande, till exempel hello följande: 

    ![meddelande när du tar bort hello storage-konto](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Kontrollera hello **Disk URL** tooidentify hello storage-konto och hello VHD som gör att du tar bort hello storage-konto. I följande exempel hello, hello sträng innan ”. blob.core.windows.net” Hej lagringskontonamn och ”SCCM2012-2015-08-28.vhd” hello VHD namn.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

### <a name="step-2-delete-hello-vhd-by-using-azure-storage-explorer"></a>Steg 2 ta bort hello VHD med hjälp av Azure Lagringsutforskaren

1. Hämta och installera hello senaste versionen av [Azure Lagringsutforskaren](http://storageexplorer.com/). Det här verktyget är en fristående app från Microsoft som möjliggör tooeasily fungerar med Azure Storage-data i Windows, macOS och Linux.
2. Öppna Azure Lagringsutforskaren, Välj ![ikon](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/account.png) Välj Azure-miljön hello vänstra fältet och logga sedan in.

3. Markera alla prenumerationer eller hello-prenumeration som innehåller hello storage-konto som du vill toodelete.

    ![Lägg till prenumeration](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/addsub.png)

4. Gå toohello storage-konto som vi fick från hello disk URL tidigare, Välj hello **Blobbbehållare** > **virtuella hårddiskar** och Sök efter hello VHD som gör att du ta bort hello storage-konto.
5. Om hello VHD hittas, kontrollera hello **namn på virtuell** kolumnen toofind hello VM som använder den här virtuella Hårddisken.

    ![Kontrollera vm](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/check-vm.png)

6. Ta bort hello lån från hello VHD med hjälp av Azure-portalen. Mer information finns i [ta bort hello-lån från hello VHD](#remove-the-lease-from-the-vhd). 

7. Gå toohello Azure Lagringsutforskaren, högerklicka på hello VHD och välj sedan ta bort.

8. Ta bort hello storage-konto.

### <a name="method-2---use-azure-portal"></a>Metod 2 – använda Azure-portalen 

#### <a name="step-1-identify-hello-vhd-that-prevent-deletion-of-hello-storage-account"></a>Steg 1: Identifiera hello VHD som förhindra borttagning av hello storage-konto

1. När du tar bort hello storage-konto visas en dialogruta för meddelande, till exempel hello följande: 

    ![meddelande när du tar bort hello storage-konto](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Kontrollera hello **Disk URL** tooidentify hello storage-konto och hello VHD som gör att du tar bort hello storage-konto. I följande exempel hello, hello sträng innan ”. blob.core.windows.net” Hej lagringskontonamn och ”SCCM2012-2015-08-28.vhd” hello VHD namn.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

2. Logga in toohello [Azure-portalen](https://portal.azure.com).
3. På navmenyn hello väljer **alla resurser**. Gå toohello storage-konto och välj sedan **Blobbar** > **virtuella hårddiskar**.

    ![Skärmbild av hello-portalen med hello storage-konto och hello ”VHD” behållare markerat](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

4. Leta upp hello VHD som vi tidigare hämtade från hello disk URL. Sedan fastställa vilken virtuell dator använder hello VHD. Vanligtvis kan du bestämma vilken VM innehåller hello VHD genom att kontrollera att namnet på hello VHD:

VM i modellen för Resource Manager-utveckling

   * OS-diskar generellt följer namnkonventionen: VMName-åååå-MM-DD-HHMMSS.vhd
   * Datadiskar generellt följer namnkonventionen: VMName-åååå-MM-DD-HHMMSS.vhd

VM i development modell

   * OS-diskar generellt följer namnkonventionen: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd
   * Datadiskar generellt följer namnkonventionen: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd

#### <a name="step-2-remove-hello-lease-from-hello-vhd"></a>Steg 2: Ta bort hello lån från hello VHD

[Ta bort hello lån från hello VHD](#remove-the-lease-from-the-vhd), och ta sedan bort hello storage-konto.

## <a name="what-is-a-lease"></a>Vad är ett lån?
Ett lån är ett lås som kan använda toocontrol åtkomst tooa blob (till exempel en virtuell Hårddisk). När en blob hyrs åtkomst bara hello ägare hello lease hello-blob. Ett lån är viktigt för hello följande orsaker:

* Det förhindrar att data skadas om flera ägare försöker toowrite toohello samma del av hello blob hello samtidigt.
* Det förhindrar att hello blob tas bort om något aktivt använder den (till exempel en virtuell dator).
* Det förhindrar att hello lagringskontot tas bort om något aktivt använder den (till exempel en virtuell dator).

### <a name="remove-hello-lease-from-hello-vhd"></a>Ta bort hello lån från hello VHD
Om hello VHD är en OS-disk, måste du ta bort hello VM tooremove hello lån:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. På hello **hubb** väljer du **virtuella datorer**.
3. Välj hello VM som innehåller ett lån på hello VHD.
4. Kontrollera att inget aktivt använder hello virtuella datorn och att du inte längre behöver hello virtuell dator.
5. Hello överst i hello **VM information** bladet väljer **ta bort**, och klicka sedan på **Ja** tooconfirm.
6. hello VM ska tas bort, men hello VHD kan behållas. Hello VHD bör dock inte längre ha ett lån på den. Det kan ta några minuter för hello lån toobe släpps. tooverify som hello lån släpps finns för**alla resurser** > **Lagringskontonamnet** > **Blobbar**  >  **virtuella hårddiskar**. I hello **Blob-egenskaper** rutan, hello **lån Status** värdet bör vara **olåst**.

Om hello VHD är en datadisk, koppla hello VHD från hello VM tooremove hello lån:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. På hello **hubb** väljer du **virtuella datorer**.
3. Välj hello VM som innehåller ett lån på hello VHD.
4. Välj **diskar** på hello **VM information** bladet.
5. Välj hello datadisk som innehåller ett lån på hello VHD. Du kan bestämma vilka virtuella Hårddisken är ansluten i hello disk genom att kontrollera hello URL för hello VHD.
6. Kontrollera med säkerhet att inget aktivt använder hello datadisk.
7. Klicka på **Detach** på hello **Disk information** bladet.
8. hello disk ska nu frånkopplas från hello VM och hello VHD inte längre ska ha ett lån på den. Det kan ta några minuter för hello lån toobe släpps. tooverify som hello lån har släppts finns för**alla resurser** > **Lagringskontonamnet** > **Blobbar**  >  **virtuella hårddiskar**. I hello **Blob-egenskaper** rutan, hello **lån Status** värdet bör vara **olåst**.

## <a name="next-steps"></a>Nästa steg
* [Ta bort ett lagringskonto](storage-create-storage-account.md#delete-a-storage-account)
* [Hur toobreak hello låst lånet för blob-lagring i Microsoft Azure (PowerShell)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
