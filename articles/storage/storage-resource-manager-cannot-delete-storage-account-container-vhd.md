---
title: "Felsöka när du tar bort Azure storage-konton, behållare eller virtuella hårddiskar | Microsoft Docs"
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
ms.openlocfilehash: 318d7146ea53a806baf813ff7de2fe91f18becc8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds"></a>Felsöka när du tar bort Azure storage-konton, behållare eller virtuella hårddiskar

Du kan få fel vid försök att ta bort ett Azure storage-konto, en behållare eller en virtuell hårddisk (VHD) med den [Azure-portalen](https://portal.azure.com). Den här artikeln innehåller felsökningsinformation för att lösa problemet i en Azure Resource Manager distribution.

Om den här artikeln inte åtgärda problemet Azure, gå Azure-forum på [MSDN och Stack Overflow](https://azure.microsoft.com/support/forums/). Du kan publicera ditt problem på dessa forum eller @AzureSupport på Twitter. Du kan också filen en Azure-supportbegäran genom att välja **få support** på den [Azure-supporten](https://azure.microsoft.com/support/options/) plats.

## <a name="symptoms"></a>Symtom
### <a name="scenario-1"></a>Scenario 1
När du försöker ta bort en virtuell Hårddisk i ett lagringskonto i en Resource Manager distribution visas följande felmeddelande:

**Det gick inte att ta bort blobben 'vhds/BlobName.vhd'. Fel: Det finns för närvarande ett lån på blob och inga lease-ID har angetts i begäran.**

Det här problemet kan uppstå eftersom en virtuell dator (VM) har ett lån på den virtuella Hårddisken som du försöker ta bort.

### <a name="scenario-2"></a>Scenario 2
När du försöker ta bort en behållare i ett lagringskonto i en Resource Manager distribution visas följande felmeddelande:

**Det gick inte att ta bort lagring behållaren ”VHD”. Fel: Det finns för närvarande ett lån på behållaren och inga lease-ID har angetts i begäran.**

Det här problemet kan uppstå eftersom behållaren har en virtuell Hårddisk som är låsta i lease-tillstånd.

### <a name="scenario-3"></a>Scenario 3
När du försöker ta bort ett lagringskonto i en Resource Manager distribution visas följande felmeddelande:

**Det gick inte att ta bort lagringskontot 'StorageAccountName'. Fel: Lagringskontot kan inte tas bort eftersom dess artefakter används.**

Det här problemet kan inträffa eftersom lagringskontot innehåller en virtuell Hårddisk som är i tillståndet lån.

## <a name="solution"></a>Lösning 
Du måste identifiera den virtuella Hårddisken som orsakar felet och den associera virtuella datorn för att lösa dessa problem. Sedan koppla från den virtuella Hårddisken från den virtuella datorn (för datadiskar) eller ta bort den virtuella datorn som använder den virtuella Hårddisken (för OS-diskar). Detta tar bort lånet från den virtuella Hårddisken så att den kan tas bort. 

Om du vill göra detta använder du någon av följande metoder:

### <a name="method-1---use-azure-storage-explorer"></a>Metod 1 - Använd Azure Lagringsutforskaren

### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a>Steg 1 identifiera den virtuella Hårddisken som förhindra borttagning av storage-konto

1. När du tar bort lagringskontot visas en dialogruta för meddelande, till exempel följande: 

    ![meddelande när du tar bort lagringskontot](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Kontrollera den **Disk URL** identifiera lagring konto och den virtuella Hårddisken som gör att du ta bort lagringskontot. I följande exempel strängen innan ”. blob.core.windows.net” är lagringskontonamn och ”SCCM2012-2015-08-28.vhd” är virtuella Hårddiskens namn.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

### <a name="step-2-delete-the-vhd-by-using-azure-storage-explorer"></a>Steg 2 ta bort den virtuella Hårddisken med hjälp av Azure Lagringsutforskaren

1. Hämta och installera den senaste versionen av [Azure Lagringsutforskaren](http://storageexplorer.com/). Det här verktyget är en fristående app från Microsoft som hjälper dig att arbeta med Azure Storage-data med Windows, macOS och Linux.
2. Öppna Azure Lagringsutforskaren, Välj ![ikon](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/account.png) Välj Azure-miljön fältet till vänster och sedan logga in.

3. Välj alla prenumerationer eller den prenumeration som innehåller lagringskontot som du vill ta bort.

    ![Lägg till prenumeration](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/addsub.png)

4. Gå till lagringskontot som vi fick från disk URL: en tidigare, Välj den **Blobbbehållare** > **virtuella hårddiskar** och Sök efter den virtuella Hårddisken som gör att du ta bort lagringskontot.
5. Kontrollera om den virtuella Hårddisken finns den **namn på virtuell** kolumnen att hitta den virtuella datorn som använder den här virtuella Hårddisken.

    ![Kontrollera vm](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/check-vm.png)

6. Ta bort lånet från den virtuella Hårddisken med hjälp av Azure-portalen. Mer information finns i [ta bort lånet från den virtuella Hårddisken](#remove-the-lease-from-the-vhd). 

7. Gå till Azure Lagringsutforskaren, högerklicka på den virtuella Hårddisken och väljer Ta bort.

8. Ta bort lagringskontot.

### <a name="method-2---use-azure-portal"></a>Metod 2 – använda Azure-portalen 

#### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a>Steg 1: Identifiera den virtuella Hårddisken som förhindra borttagning av storage-konto

1. När du tar bort lagringskontot visas en dialogruta för meddelande, till exempel följande: 

    ![meddelande när du tar bort lagringskontot](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Kontrollera den **Disk URL** identifiera lagring konto och den virtuella Hårddisken som gör att du ta bort lagringskontot. I följande exempel strängen innan ”. blob.core.windows.net” är lagringskontonamn och ”SCCM2012-2015-08-28.vhd” är virtuella Hårddiskens namn.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

2. Logga in på [Azure Portal](https://portal.azure.com).
3. På navmenyn väljer **alla resurser**. Gå till lagringskontot och välj sedan **Blobbar** > **virtuella hårddiskar**.

    ![Skärmbild av portalen med storage-konto och ”VHD”-behållaren markerat](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

4. Leta upp den virtuella Hårddisken som vi tidigare hämtade från disk-URL. Sedan fastställa vilken virtuell dator med hjälp av den virtuella Hårddisken. Vanligtvis kan du bestämma vilken VM innehåller den virtuella Hårddisken genom att kontrollera att namnet på den virtuella Hårddisken:

VM i modellen för Resource Manager-utveckling

   * OS-diskar generellt följer namnkonventionen: VMName-åååå-MM-DD-HHMMSS.vhd
   * Datadiskar generellt följer namnkonventionen: VMName-åååå-MM-DD-HHMMSS.vhd

VM i development modell

   * OS-diskar generellt följer namnkonventionen: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd
   * Datadiskar generellt följer namnkonventionen: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd

#### <a name="step-2-remove-the-lease-from-the-vhd"></a>Steg 2: Ta bort lånet från den virtuella Hårddisken

[Ta bort lånet från den virtuella Hårddisken](#remove-the-lease-from-the-vhd), och ta bort lagringskontot.

## <a name="what-is-a-lease"></a>Vad är ett lån?
Ett lån är ett lås som kan användas för att styra åtkomsten till en blob (till exempel en virtuell Hårddisk). När en blob hyrs endast ägare av lånet åtkomst till blob. Ett lån är viktigt av följande skäl:

* Det förhindrar att data skadas om flera ägare försök att skriva till samma del av blob samtidigt.
* Det förhindrar att blob tas bort om något aktivt använder den (till exempel en virtuell dator).
* Det förhindrar att lagringskontot tas bort om något aktivt använder den (till exempel en virtuell dator).

### <a name="remove-the-lease-from-the-vhd"></a>Ta bort lånet från den virtuella Hårddisken
Om den virtuella Hårddisken är en OS-disk, måste du ta bort den virtuella datorn för att ta bort lånet:

1. Logga in på [Azure Portal](https://portal.azure.com).
2. På den **hubb** väljer du **virtuella datorer**.
3. Välj den virtuella datorn som innehåller ett lån på den virtuella Hårddisken.
4. Kontrollera att inget aktivt använder den virtuella datorn, och du inte längre behöver den virtuella datorn.
5. Längst upp i den **VM information** bladet väljer **ta bort**, och klicka sedan på **Ja** att bekräfta.
6. Den virtuella datorn ska tas bort, men den virtuella Hårddisken kan behållas. Den virtuella Hårddisken ska dock inte längre ha ett lån på den. Det kan ta några minuter för lånet frigörs. Kontrollera att lånet släpps, gå till **alla resurser** > **Lagringskontonamnet** > **Blobbar**  >   **virtuella hårddiskar**. I den **Blob-egenskaper** rutan i **lån Status** värdet bör vara **olåst**.

Om den virtuella Hårddisken är en datadisk, kan du koppla från den virtuella Hårddisken från den virtuella datorn att ta bort lånet:

1. Logga in på [Azure Portal](https://portal.azure.com).
2. På den **hubb** väljer du **virtuella datorer**.
3. Välj den virtuella datorn som innehåller ett lån på den virtuella Hårddisken.
4. Välj **diskar** på den **VM information** bladet.
5. Välj den datadisk som som innehåller ett lån på den virtuella Hårddisken. Du kan bestämma vilka virtuella Hårddisken är ansluten i disken genom att kontrollera URL: en för den virtuella Hårddisken.
6. Kontrollera med säkerhet att inget aktivt använder datadisken.
7. Klicka på **Detach** på den **Disk information** bladet.
8. Disken nu att koppla från den virtuella datorn och den virtuella Hårddisken inte längre ska ha ett lån på den. Det kan ta några minuter för lånet frigörs. Kontrollera att lånet har släppts, gå till **alla resurser** > **Lagringskontonamnet** > **Blobbar**  >  **virtuella hårddiskar**. I den **Blob-egenskaper** rutan i **lån Status** värdet bör vara **olåst**.

## <a name="next-steps"></a>Nästa steg
* [Ta bort ett lagringskonto](storage-create-storage-account.md#delete-a-storage-account)
* [Hur du delar upp låst lånet för blob-lagring i Microsoft Azure (PowerShell)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
