---
title: "aaaCreate din första virtuella datorn i ett labb i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur toocreate din första virtuella datorn i ett labb i Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: fbc5a438-6e02-4952-b654-b8fa7322ae5f
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2017
ms.author: tarcher
ms.openlocfilehash: 4c3257efca9be6fdd190eaac1db731464e07fcfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a>Skapa din första virtuella datorn i ett labb i Azure DevTest Labs

När du först komma åt DevTest Labs och vill toocreate din första virtuella datorn, kommer troligen gör du det med hjälp av en förinstallerade [grundläggande marketplace-avbildning](devtest-lab-configure-marketplace-images.md). Vid ett senare tillfälle du även att kunna toochoose från en [anpassad avbildning och en formel](devtest-lab-add-vm.md) när du skapar flera virtuella datorer. 

Den här självstudiekursen vägleder dig genom med hello Azure portal tooadd ditt första VM tooa labb i DevTest Labs.

## <a name="steps-tooadd-your-first-vm-tooa-lab-in-azure-devtest-labs"></a>Steg tooadd ditt första VM tooa labb i Azure DevTest Labs
1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
1. Välj hello labb som du vill toocreate hello VM hello listan övningar.  
1. På hello lab **översikt** bladet väljer **+ Lägg till**.  

    ![VM-knappen Lägg till](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. På hello **väljer du en bas** bladet Välj en marketplace-avbildning för hello VM.
1. På hello **virtuella** bladet, ange ett namn för hello nya virtuella datorn i hello **virtuellt datornamn** textruta.

    ![Blad för labbet VM](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. Ange en **användarnamn** som har beviljats administratörsbehörighet på hello virtuella datorn.  
1. Ange ett lösenord i hello textfält med etiketten **skriver ett värde**.
1. Hej **virtuella disktyp** avgör vilken lagringstyp som disk tillåts för hello virtuella datorer i hello labbet.
1. Välj **storlek för virtuell dator** och välj något av hello fördefinierade objekt som anger hello processorkärnor, ram-storlek och hello hårddiskstorlek av hello VM toocreate.
1. Välj **artefakter** och - hello listan med artefakter - Välj och konfigurera hello artefakter som du vill tooadd toohello grundläggande bild.
    **Obs:** om du nya tooDevTest övningar eller konfigurera artefakter, se toohello [lägga till en befintlig artefakt tooa VM](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) avsnittet och sedan återvända hit när du är klar.
1. Välj **skapa** tooadd hello angetts VM toohello labbet.

   hello blad för labbet visar hello status för skapa hello VM - först som **skapa**, sedan som **kör** efter hello VM har startats.

## <a name="next-steps"></a>Nästa steg
* En gång hello VM har skapats, du kan ansluta toohello VM genom att välja **Anslut** hello VM-bladet.
* Checka ut [lägga till ett labb för VM-tooa](devtest-lab-add-vm.md) fullständig information om hur du lägger till följande virtuella datorer i labbet.
* Utforska hello [DevTest Labs Azure Resource Manager QuickStart mallgalleriet](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).
