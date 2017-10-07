---
title: "aaaConfigure Azure Marketplace inställningarna i Azure DevTest Labs | Microsoft Docs"
description: "Konfigurera vilka Azure Marketplace-bilder som kan användas när du skapar en virtuell dator i Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 804c6af2-17e9-4320-af3a-f454bd398379
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: bb4b7f1c0cbe967bee724f7ee20f64f8c4ea58ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Konfigurera inställningar för Azure Marketplace-avbildning i Azure DevTest Labs
DevTest Labs har stöd för att skapa virtuella datorer baserat på Azure Marketplace-bilder beroende på hur du har konfigurerat Azure Marketplace-avbildningar toobe används i labbet. Den här artikeln beskrivs hur du toospecify som eventuellt Azure Marketplace-bilder kan vara används när du skapar virtuella datorer i ett labb. Detta säkerställer att din grupp endast har åtkomst toohello Marketplace-bilder som de behöver. 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>Välj vilka Azure Marketplace-bilder tillåts när du skapar en virtuell dator
1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
3. Välj önskad hello-labb hello listan övningar. 
4. På bladet hello lab väljer **konfiguration och principer**.
5. På lab's **konfiguration och principer** bladet under **virtuella Datorbaser**väljer **Marketplace-bilder**.
6. Ange om du vill att alla hello kvalificerade Azure Marketplace-avbildningar toobe användas som bas för en ny virtuell dator. Om du väljer **Ja**, sedan alla hello Azure Marketplace-bilder som uppfyller alla hello följande villkor är tillåtna i hello testlabb:
   
   * hello bild skapas en enda virtuell dator **och**
   * hello avbildningen använder Azure Resource Manager tooprovision VMs **och**
   * hello bilden behöver inte köpa ett extra licensieringsplan
     
    Om du vill att inga bilder toobe tillåts eller toospecify vilka bilder kan användas, Välj önskade **nr**.
     
     ![Alternativet tooallow alla Marketplace-avbildningar toobe används som bas avbildningar för virtuella datorer](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. Om du väljer **nr** toohello föregående steg, hello **tillåtna bilder/Välj alla** kryssrutan aktiverad. 
   Du kan använda det här alternativet tillsammans med hello Sök tooquickly Markera eller avmarkera alla hello objekt visas i hello lista.
   * Välj önskade tooallow för att skapa Virtuella individuellt genom att markera kryssrutan för varje avbildning motsvarande hello Azure Marketplace-bilder.
   * Välj ingenting hello-listan om du inte vill tooallow alla Azure Marketplace-avbildningar toobe används i hello labbet.
   
    ![Du kan ange vilken Azure Marketplace-avbildningar kan användas som bas avbildningar för virtuella datorer](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nästa steg
När du har konfigurerat hur Azure Marketplace-bilder tillåts när du skapar en virtuell dator, hello nästa steg är för[lägga till ett labb för VM-tooyour](devtest-lab-add-vm-with-artifacts.md).

