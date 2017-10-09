---
title: "aaaUpload VHD-filen med hjälp av Microsoft Azure Lagringsutforskaren för tooAzure DevTest Labs | Microsoft Docs"
description: "Överföra VHD-filen toolab storage-konto med Microsoft Azure Lagringsutforskaren"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 686691e3676cea4b2d7cd8bf045bc43a792c667e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-microsoft-azure-storage-explorer"></a>Överföra VHD-filen toolab storage-konto med Microsoft Azure Lagringsutforskaren

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

VHD-filer kan vara används toocreate anpassade avbildningar, som används tooprovision virtuella datorer i Azure DevTest Labs. Den här artikeln beskrivs hur toouse [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooa labblagringskontot tooupload en VHD-filen. När du har överfört VHD-filen hello [nästa steg avsnittet](#next-steps) innehåller vissa artiklar som illustrerar hur toocreate en anpassad avbildning från hello överföra VHD-filen. Mer information om diskar och virtuella hårddiskar i Azure finns [om diskar och virtuella hårddiskar för virtuella datorer](../virtual-machines/linux/about-disks-and-vhds.md)

## <a name="step-by-step-instructions"></a>Stegvisa instruktioner

Hej följande steg gå dig genom att ladda upp en VHD-filen tooDevTest övningar med [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md).

1. [Hämta och installera hello senaste versionen av hello Microsoft Azure Lagringsutforskaren](http://www.storageexplorer.com).

1. Hämta hello namnet på hello lab storage-konto med hello Azure-portalen:

    1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
    
    1. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
    
    1. Välj önskad hello-labb hello listan övningar.  
    
    1. På bladet hello lab väljer **Configuration**. 
    
    1. På hello lab **Configuration** bladet väljer **anpassade avbildningar (VHD)**.
    
    1. På hello **anpassade avbildningar** bladet välj **+ Lägg till**. 
    
    1. På hello **anpassad bild** bladet väljer **VHD**.
    
    1. På hello **VHD** bladet väljer **överföra en virtuell Hårddisk med hjälp av PowerShell**.
    
        ![Överför VHD med PowerShell][0]
    
    1. Hej **ladda upp en bild med PowerShell** bladet visar ett anrop toohello **Add-AzureVhd** cmdlet. Hej första parametern (*mål*) innehåller hello lagringskontonamnet för hello labbet i hello följande format:
    
        https://<STORAGE-ACCOUNT-Name>.BLOB.Core.Windows.NET/uploads/... 

    1. Anteckna hello lagringskontonamnet eftersom den används i senare steg.
    
1. Ansluta tooan konto för Azure-prenumeration med Lagringsutforskaren.

    > [!TIP] 
    > 
    > Lagringsutforskaren stöder flera anslutningsalternativ. Det här avsnittet visar anslutning tooa storage-konto som är associerade med din Azure-prenumeration. toosee hello andra anslutningsalternativ som stöds av Lagringsutforskaren finns i artikeln toohello [komma igång med Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md).
 
    1. Öppna Lagringsutforskaren.
    
    1. I Lagringsutforskaren, Välj **Azure kontoinställningar**. 
    
        ![Inställningar för Azure-konto][1]
    
    1. hello vänster visar hello Microsoft-konton som du har loggat in till. tooconnect tooanother kontot som väljer **Lägg till ett konto**, och följ hello dialogrutor toosign in med ett Microsoft-konto som är associerad med minst en aktiv Azure-prenumeration.
    
        ![Lägga till ett konto][2]
    
    1. När du loggar in med ett Microsoft-konto, fylls hello vänster med hello Azure-prenumerationer som är associerade med det kontot. Välj hello Azure-prenumerationer som du vill toowork och välj sedan **tillämpa**. (Att välja **alla prenumerationer** växlar hello val av alla eller inga av hello som anges Azure-prenumerationer.)
    
        ![Välja Azure-prenumerationer][3]
    
    1. hello vänster visar hello storage-konton som är associerade med hello valda Azure-prenumerationer.
    
        ![Valda Azure-prenumerationer][4]

1. Leta upp hello labblagringskontot:

    1. Leta upp hello Lagringsutforskaren vänster och expanderar hello noden för hello Azure-prenumeration som äger hello labbet.
    
    1. Expandera under hello prenumeration nod **Lagringskonton**.

    1. Expandera hello lab konto nod tooreveal lagringsnoder för **Blobbbehållare**, **filresurser**, **köer**, och **tabeller**.
    
    1. Expandera hello **Blobbbehållare** nod.
    
    1. Välj hello överföringar blob-behållaren toodisplay innehållet i hello till höger.
        
        ![Ladda upp katalogen][5]

1. Överför hello VHD-fil med Lagringsutforskaren:

    1. I hello Lagringsutforskaren högra rutan, bör du se en lista över hello blobbar i hello **överför** hello labblagringskontot blob-behållare. Välj hello blob editor verktygsfältet **överför** 
        
        ![Överför knappen][6]
    
    1. Från hello **överför** nedrullningsbara menyn och väljer **Överför filer...** .
    
    1. På hello **ladda upp filer** dialogrutan, Välj hello tre punkter.
        
        ![Välj fil][8]  

    1. På hello **Välj filer tooupload** dialogrutan Bläddra toohello önskad VHD-filen markerar du den och välj sedan **öppna**.
    
    1. När returnerade toohello **ladda upp filer** dialogrutan Ändra **Blob typen** för**Sidblob**.
    
    1. Välj **Överför**.

        ![Välj fil][9]  
    
    1. hello Lagringsutforskaren **aktivitetsloggen** visar hello download status (tillsammans med länkar toocancel hello överför). hello processen överför en VHD-fil kan vara tidskrävande beroende på hello storleken på hello VHD-filen och anslutningshastigheten. 

        ![Överför filstatus][10]  

## <a name="next-steps"></a>Nästa steg

- [Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hello Azure-portalen](devtest-lab-create-template.md)
- [Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hjälp av PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

[0]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-image-using-psh.png
[1]: ./media/devtest-lab-upload-vhd-using-storage-explorer/settings-icon.png
[2]: ./media/devtest-lab-upload-vhd-using-storage-explorer/add-account-link.png
[3]: ./media/devtest-lab-upload-vhd-using-storage-explorer/subscriptions-list.png
[4]: ./media/devtest-lab-upload-vhd-using-storage-explorer/storage-accounts-list.png
[5]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-dir.png
[6]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-button.png
[7]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-files.png
[8]: ./media/devtest-lab-upload-vhd-using-storage-explorer/select-file.png
[9]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-file.png
[10]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-status.png
