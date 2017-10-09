---
title: "aaaCreate en anpassad avbildning Azure DevTest Labs från en VHD-fil | Microsoft Docs"
description: "Lär dig hur toocreate en anpassad avbildning i Azure DevTest Labs från en VHD-filen med hello Azure-portalen"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 80af8ea1cb72380f868df0a76c4a0dcd92e63cf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Skapa en anpassad avbildning från en VHD-fil

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Stegvisa instruktioner

hello följande steg beskriver hur du skapar en anpassad avbildning från en VHD-fil med hello Azure-portalen:

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.

1. Välj önskad hello-labb hello listan övningar.  

1. På bladet hello lab väljer **Configuration**. 

1. På hello lab **Configuration** bladet väljer **anpassade avbildningar (VHD)**.

1. På hello **anpassade avbildningar** bladet väljer **+ Lägg till**.

    ![Lägg till anpassad bild](./media/devtest-lab-create-template/add-custom-image.png)

1. Ange hello namn på hello anpassad avbildning. Det här namnet visas i hello lista över grundläggande bilder när du skapar en virtuell dator.

1. Ange hello beskrivning av hello anpassad avbildning. Den här beskrivningen visas i hello lista över grundläggande bilder när du skapar en virtuell dator.

1. Välj **VHD**.

1. Från hello **VHD** bladet, Välj hello önskad VHD-filen.

1. Välj **OK** tooclose hello **VHD** bladet.

1. Välj **Operativsystemskonfiguration**.

1. På hello **Operativsystemskonfiguration** väljer du antingen **Windows** eller **Linux**.

1. Om **Windows** har valts anger via hello kryssrutan om *Sysprep* har körts på hello-datorn. 

1. Välj **OK** tooclose hello **Operativsystemets konfiguration** bladet.

1. Välj **OK** toocreate hello anpassad avbildning.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Relaterade blogginlägg

- [Anpassade avbildningar eller formler?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiera anpassade avbildningar mellan Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Nästa steg

- [Lägga till ett VM tooyour labb](./devtest-lab-add-vm-with-artifacts.md)
