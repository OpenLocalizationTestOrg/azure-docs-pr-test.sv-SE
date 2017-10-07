---
title: "aaaUpload VHD-filen med hjälp av AzCopy för tooAzure DevTest Labs | Microsoft Docs"
description: "Överföra VHD-filen toolab lagringskonto med hjälp av AzCopy"
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
ms.openlocfilehash: 14f9e933b0bd27451f6bcb94841ecc381213e578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-azcopy"></a>Överföra VHD-filen toolab lagringskonto med hjälp av AzCopy

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

VHD-filer kan vara används toocreate anpassade avbildningar, som används tooprovision virtuella datorer i Azure DevTest Labs. hello följande steg får du hjälp med hello AzCopy kommandoradsverktyget tooupload en VHD-filen tooa's labblagringskontot. När du har överfört VHD-filen hello [nästa steg avsnittet](#next-steps) innehåller vissa artiklar som illustrerar hur toocreate en anpassad avbildning från hello överföra VHD-filen. Mer information om diskar och virtuella hårddiskar i Azure finns [om diskar och virtuella hårddiskar för virtuella datorer](../virtual-machines/linux/about-disks-and-vhds.md)

> [!NOTE] 
>  
> AzCopy är ett kommandoradsverktyg som endast för Windows.

## <a name="step-by-step-instructions"></a>Stegvisa instruktioner

Hej följande steg gå du hjälp med att ladda upp en VHD-filen tooAzure DevTest Labs med [AzCopy](http://aka.ms/downloadazcopy). 

1. Hämta hello namnet på hello lab storage-konto med hello Azure-portalen:

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.

1. Välj önskad hello-labb hello listan övningar.  

1. På bladet hello lab väljer **Configuration**. 

1. På hello lab **Configuration** bladet väljer **anpassade avbildningar (VHD)**.

1. På hello **anpassade avbildningar** bladet välj **+ Lägg till**. 

1. På hello **anpassad bild** bladet väljer **VHD**.

1. På hello **VHD** bladet väljer **överföra en virtuell Hårddisk med hjälp av PowerShell**.

    ![Överför VHD med PowerShell](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. Hej **ladda upp en bild med PowerShell** bladet visar ett anrop toohello **Add-AzureVhd** cmdlet. Hej första parametern (*mål*) innehåller hello URI för en blob-behållare (*överför*) i hello följande format:

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. Anteckna hello fullständiga URI eftersom den används i senare steg.

1. Överför hello VHD-fil med AzCopy:
 
1. [Hämta och installera hello senaste versionen av AzCopy](http://aka.ms/downloadazcopy).

1. Öppna ett kommandofönster och gå toohello installationskatalogen för AzCopy. Du kan också kan du lägga till hello AzCopy tooyour systemsökvägen installationsplatsen. AzCopy är som standard installerade toohello följande katalog:

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. Med hjälp av hello konto nyckel och blob lagringsbehållaren URI, kör följande kommando vid kommandotolken hello hello. Hej *vhdFileName* värdet måste toobe inom citattecken. hello processen överför en VHD-fil kan vara tidskrävande beroende på hello storleken på hello VHD-filen och anslutningshastigheten.   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a>Nästa steg

- [Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hello Azure-portalen](devtest-lab-create-template.md)
- [Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hjälp av PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)