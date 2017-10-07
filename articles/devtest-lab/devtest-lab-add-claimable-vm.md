---
title: aaaAdd ett claimable VM tooa labb i Azure DevTest Labs | Microsoft Docs
description: "Lär dig hur tooadd ett claimable virtuella tooa labb i Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: tarcher
ms.openlocfilehash: fe6385ae2e59b9636b82aec250dc3a1f8a40ba5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-claimable-vm-tooa-lab-in-azure-devtest-labs"></a>Lägga till ett claimable VM tooa labb i Azure DevTest Labs
Du lägger till ett claimable VM tooa labb i ett liknande sätt toohow du [lägga till en standard VM](devtest-lab-add-vm.md) – från en *grundläggande* som är antingen en [anpassad avbildning](devtest-lab-create-template.md), [formeln](devtest-lab-manage-formulas.md), eller [Marketplace-avbildning](devtest-lab-configure-marketplace-images.md). Den här självstudiekursen vägleder dig genom att använda hello Azure portal tooadd ett claimable VM tooa labb i DevTest Labs och visar hello-processen en användare följer tooclaim hello VM.

> [!NOTE]
> Om du distribuerar lab virtuella datorer via [Azure Resource Manager-mallar](devtest-lab-create-environment-from-arm.md), du kan skapa claimable virtuella datorer genom att ange hello **allowClaim** egenskapen tootrue i hello properties-avsnittet.
>
>

## <a name="steps-tooadd-a-claimable-vm-tooa-lab-in-azure-devtest-labs"></a>Steg tooadd ett claimable VM tooa labb i Azure DevTest Labs
1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
1. Välj hello lab hello listan övningar som du vill toocreate hello claimable VM.  
1. På hello lab **översikt** bladet väljer **+ Lägg till**.  

    ![VM-knappen Lägg till](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. På hello **väljer du en bas** bladet Välj en bas för hello VM.
1. På hello **virtuella** bladet, ange ett namn för hello nya virtuella datorn i hello **virtuellt datornamn** textruta.

    ![Blad för labbet VM](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Ange en **användarnamn** som har beviljats administratörsbehörighet på hello virtuella datorn.  
1. Om du vill toouse ett lösenord som lagras i din [hemliga store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store)väljer **använda en sparad hemlighet**, och ange ett nyckelvärde som motsvarar tooyour hemlighet (lösenord). Annars kan ange ett lösenord i hello textfält med etiketten **skriver ett värde**.
1. Hej **virtuella disktyp** avgör vilken lagringstyp som disk tillåts för hello virtuella datorer i hello labbet.
1. Välj **storlek för virtuell dator** och välj något av hello fördefinierade objekt som anger hello processorkärnor, ram-storlek och hello hårddiskstorlek av hello VM toocreate.
1. Välj **artefakter** och hello listan över artefakter och välj och konfigurera hello artefakter som du vill tooadd toohello grundläggande bild. Om du nya tooDevTest övningar eller konfigurera artefakter, se toohello [lägga till en befintlig artefakt tooa VM](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) avsnittet och sedan återvända hit när du är klar.
1. Välj **avancerade inställningar** tooconfigure hello VM Nätverksalternativ alternativ och upphör att gälla. Under **anspråk alternativ**, Välj **Ja** toomake hello datorn claimable.

  ![Välj toomake hello VM claimable.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. Om du vill tooview eller kopiera hello Azure Resource Manager-mall, se toohello [spara Azure Resource Manager-mall](devtest-lab-add-vm.md#save-azure-resource-manager-template) avsnittet och återvända hit när du är klar.
1. Välj **skapa** tooadd hello angetts VM toohello labbet.
1. hello blad för labbet visar hello status för skapa hello VM - först som **skapa**, sedan som **kör** efter hello VM har startats.


## <a name="using-a-claimable-vm"></a>Med hjälp av en claimable VM

En användare kan göra anspråk på alla VM hello listan över ”Claimable virtuella datorer” genom att göra något av följande:

* Högerklicka på någon av hello virtuella datorer i hello listan hello listan över ”Claimable virtuella datorer” längst ned hello hello lab översikt bladet, och välj **anspråk datorn**.

 ![Begäran om en specifik claimable VM.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* Hello överst i hello **översikt** bladet välj **anspråk någon**. En slumpmässig virtuell dator har tilldelats hello listan över claimable virtuella datorer.

 ![Begära alla claimable VM.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


När en användare anspråk som en virtuell dator, flyttas till sin lista över ”mina virtuella datorer” och är inte längre claimable av någon annan användare.

## <a name="next-steps"></a>Nästa steg
* En gång hello VM har skapats, du kan ansluta toohello VM genom att välja **Anslut** hello VM-bladet.
* Utforska hello [DevTest Labs Azure Resource Manager QuickStart mallgalleriet](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)
