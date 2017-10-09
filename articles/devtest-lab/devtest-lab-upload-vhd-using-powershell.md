---
title: "aaaUpload VHD-filen tooAzure DevTest Labs med hjälp av PowerShell | Microsoft Docs"
description: "Överföra VHD-filen toolab storage-konto med hjälp av PowerShell"
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
ms.openlocfilehash: 9c3ee96e212457b0ef8203714b419350cb97f895
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-powershell"></a>Överföra VHD-filen toolab storage-konto med hjälp av PowerShell

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

VHD-filer kan vara används toocreate anpassade avbildningar, som används tooprovision virtuella datorer i Azure DevTest Labs. hello följande steg beskriver hur du använder PowerShell tooupload en VHD-filen tooa's labblagringskontot. När du har överfört VHD-filen hello [nästa steg avsnittet](#next-steps) innehåller vissa artiklar som illustrerar hur toocreate en anpassad avbildning från hello överföra VHD-filen. Mer information om diskar och virtuella hårddiskar i Azure finns [om diskar och virtuella hårddiskar för virtuella datorer](../virtual-machines/linux/about-disks-and-vhds.md)

## <a name="step-by-step-instructions"></a>Stegvisa instruktioner

hello följande steg gå du hjälp med att ladda upp en VHD-filen tooAzure DevTest Labs med hjälp av PowerShell. 

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.

1. Välj önskad hello-labb hello listan övningar.  

1. På bladet hello lab väljer **Configuration**. 

1. På hello lab **Configuration** bladet väljer **anpassade avbildningar (VHD)**.

1. På hello **anpassade avbildningar** bladet välj **+ Lägg till**. 

1. På hello **anpassad bild** bladet väljer **VHD**.

1. På hello **VHD** bladet väljer **överföra en virtuell Hårddisk med hjälp av PowerShell**.

    ![Överför VHD med PowerShell](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. På hello **ladda upp en bild med PowerShell** bladet, kopiera hello genereras PowerShell-skript tooa textredigerare.

1. Ändra hello **LocalFilePath** parametern för hello **Add-AzureVhd** cmdlet toopoint toohello platsen för hello VHD-filen som du vill tooupload.

1. I PowerShell-Kommandotolken kör hello **Add-AzureVhd** cmdlet (med hello ändrade **LocalFilePath** parametern).

> [!WARNING] 
> 
> hello processen överför en VHD-fil kan vara tidskrävande beroende på hello storleken på hello VHD-filen och anslutningshastigheten.

## <a name="next-steps"></a>Nästa steg

- [Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hello Azure-portalen](devtest-lab-create-template.md)
- [Skapa en anpassad avbildning i Azure DevTest Labs från en VHD-fil med hjälp av PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
