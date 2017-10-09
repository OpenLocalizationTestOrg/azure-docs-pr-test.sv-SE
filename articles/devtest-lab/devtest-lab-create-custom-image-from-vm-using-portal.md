---
title: "aaaCreate en anpassad avbildning i Azure DevTest Labs från en virtuell dator | Microsoft Docs"
description: "Lär dig hur toocreate en anpassad avbildning i Azure DevTest Labs från ett etablerade VM använder hello Azure-portalen"
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
ms.openlocfilehash: 7dccb79d3db4aae676c7bd2f6b800301210491e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-from-a-vm"></a>Skapa en anpassad avbildning från en virtuell dator

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Stegvisa instruktioner

Du kan skapa en anpassad avbildning från en allokerad virtuell dator och därefter använda den anpassade bilden toocreate identiska virtuella datorer. hello följande steg visar hur toocreate en anpassad avbildning från en virtuell dator:

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.

1. Välj önskad hello-labb hello listan övningar.  

1. På bladet hello lab väljer **Mina virtuella datorer**.
 
1. På hello **Mina virtuella datorer** bladet välj hello VM som du vill toocreate hello anpassad avbildning.

1. På bladet hello VM väljer **Skapa anpassad avbildning (VHD)**.

    ![Skapa anpassad avbildning menyobjekt](./media/devtest-lab-create-template/create-custom-image.png)

1. På hello **skapa avbildningen** bladet, ange ett namn och beskrivning för den anpassade avbildningen. Den här informationen visas i hello lista över databaser när du skapar en virtuell dator.

    ![Skapa anpassad avbildning bladet](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Välj om sysprep kördes på hello VM. Om hello sysprep inte kördes på hello VM, kan du ange om du vill att sysprep körs när en virtuell dator skapas från den här anpassade avbildningen.

1. Välj **OK** när klar toocreate hello anpassad avbildning.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Relaterade blogginlägg

- [Anpassade avbildningar eller formler?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiera anpassade avbildningar mellan Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Nästa steg

- [Lägga till ett VM tooyour labb](./devtest-lab-add-vm-with-artifacts.md)
