---
title: aaaAdd ett VM tooa labb i Azure DevTest Labs | Microsoft Docs
description: "Lär dig hur tooadd ett virtuella tooa labb i Azure DevTest Labs"
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
ms.date: 02/24/2017
ms.author: tarcher
ms.openlocfilehash: 82838e4349550db56de311264c188140b9556b24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-vm-tooa-lab-in-azure-devtest-labs"></a>Lägga till ett VM tooa labb i Azure DevTest Labs
Om du redan har [skapat din första virtuella dator](devtest-lab-create-first-vm.md), du förmodligen gjorde det från en förinstallerade [marketplace-avbildning](devtest-lab-configure-marketplace-images.md). Nu om du vill tooadd efterföljande VMs tooyour labbet, du kan också välja en *grundläggande* som är antingen en [anpassad avbildning](devtest-lab-create-template.md) eller en [formeln](devtest-lab-manage-formulas.md). Den här självstudiekursen vägleder dig genom att använda hello Azure portal tooadd ett VM tooa labb i DevTest Labs.

Den här artikeln visar också hur toomanage hello artefakter för en virtuell dator i labbet.

## <a name="steps-tooadd-a-vm-tooa-lab-in-azure-devtest-labs"></a>Steg tooadd ett VM tooa labb i Azure DevTest Labs
1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
1. Välj hello labb som du vill toocreate hello VM hello listan övningar.  
1. På hello lab **översikt** bladet väljer **+ Lägg till**.  

    ![VM-knappen Lägg till](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. På hello **väljer du en bas** bladet Välj en bas för hello VM.
1. På hello **virtuella** bladet, ange ett namn för hello nya virtuella datorn i hello **virtuellt datornamn** textruta.

    ![Blad för labbet VM](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Ange en **användarnamn** som har beviljats administratörsbehörighet på hello virtuella datorn.  
1. Om du vill toouse ett lösenord som lagras i din [hemliga store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store)väljer **använda en sparad hemlighet**, och ange ett nyckelvärde som motsvarar tooyour hemlighet (lösenord). Annars kan ange ett lösenord i hello textfält med etiketten **skriver ett värde**.
1. Hej **virtuella disktyp** avgör vilken lagringstyp som disk tillåts för hello virtuella datorer i hello labbet.
1. Välj **storlek för virtuell dator** och välj något av hello fördefinierade objekt som anger hello processorkärnor, ram-storlek och hello hårddiskstorlek av hello VM toocreate.
1. Välj **artefakter** och - hello listan med artefakter - Välj och konfigurera hello artefakter som du vill tooadd toohello grundläggande bild.
    **Obs:** om du nya tooDevTest övningar eller konfigurera artefakter, se toohello [lägga till en befintlig artefakt tooa VM](#add-an-existing-artifact-to-a-vm) avsnittet och sedan återvända hit när du är klar.
1. Välj **avancerade inställningar** tooconfigure hello VM Nätverksalternativ alternativ och upphör att gälla. 

   tooset upphör att gälla kan välja hello Kalender ikonen toospecify ett datum då hello VM tas bort automatiskt.  Som standard upphör aldrig hello VM. 
1. Om du vill tooview eller kopiera hello Azure Resource Manager-mall, se toohello [spara Azure Resource Manager-mall](#save-azure-resource-manager-template) avsnittet och återvända hit när du är klar.
1. Välj **skapa** tooadd hello angetts VM toohello labbet.
1. hello blad för labbet visar hello status för skapa hello VM - först som **skapa**, sedan som **kör** efter hello VM har startats.

> [!NOTE]
> [Lägg till en claimable VM](devtest-lab-add-claimable-vm.md) visar hur toomake hello VM claimable så att den är tillgänglig för användning av alla användare i hello labbet.
>
>

## <a name="add-an-existing-artifact-tooa-vm"></a>Lägg till en befintlig artefakt tooa VM
När du skapar en virtuell dator måste du lägga till befintliga artefakter. Varje labb ingår artefakter från hello offentliga DevTest Labs Artefaktcentrallagret samt artefakter att du har skapat och tillagda tooyour äger Artefaktcentrallagret.

* Azure DevTest Labs *artefakter* kan du ange *åtgärder* som utförs när hello VM etableras, som kör Windows PowerShell-skript, köra Bash-kommandon och installera programvara.
* Artefakt *parametrar* kan du anpassa hello artefakt för ditt specifika scenario

toodiscover hur toocreate artefakter, se hello artikeln [Lär dig hur tooauthor egna artefakter för hjälp med DevTest Labs](devtest-lab-artifact-author.md).

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
1. Välj hello labb som innehåller hello VM som du vill använda toowork hello listan övningar.  
1. Välj **Mina virtuella datorer**.
1. Välj hello önskade VM.
1. Välj **artefakter**. 
1. Välj **gäller artefakter**.
1. På hello **gäller artefakter** bladet, Välj hello artefakt som du vill tooadd toohello VM.
1. På hello **Lägg till artefakt** bladet ange parametervärden för hello som krävs och valfria parametrar som du behöver.  
1. Välj **Lägg till** tooadd hello artefakt och gå sedan tillbaka toohello **gäller artefakter** bladet.
1. Fortsätt lägga till artefakter som behövs för den virtuella datorn.
1. När du har lagt till din artefakter, kan du [ändra hello ordning i vilken hello artefakter körs](#change-the-order-in-which-artifacts-are-run). Du kan även gå tillbaka för[visa eller ändra en artefakt](#view-or-modify-an-artifact).
1. När du är klar att lägga till artefakter, Välj **tillämpa**

## <a name="change-hello-order-in-which-artifacts-are-run"></a>Ändra hello ordning där artefakter körs
Som standard utförs hello åtgärder av hello artefakter i hello ordning som de har lagts till toohello VM. hello visar följande steg hur toochange hello ordning i vilken hello artefakter körs.

1. Hello överst i hello **gäller artefakter** , Välj hello länk som indikerar hello antal artefakter som lagts till toohello VM-bladet.
   
    ![Antalet artefakter som lagts till tooVM](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. På hello **valt artefakter** bladet drar och släpper hello artefakter i hello önskad ordning. **Obs:** om du har svårt att dra hello artefakt, se till att du drar från vänster sida av hello artefakt hello. 
1. Välj **OK** när du är klar.  

## <a name="view-or-modify-an-artifact"></a>Visa eller ändra en artefakt
hello följande steg visar hur tooview eller ändra hello parametrar för en artefakt:

1. Hello överst i hello **gäller artefakter** , Välj hello länk som indikerar hello antal artefakter som lagts till toohello VM-bladet.
   
    ![Antalet artefakter som lagts till tooVM](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. På hello **valt artefakter** bladet, Välj hello artefakt som du vill tooview eller redigera.  
1. På hello **Lägg till artefakt** bladet, se något behov ändringar och välj **OK** tooclose hello **Lägg till artefakt** bladet.
1. Välj **OK** tooclose hello **valt artefakter** bladet.

## <a name="save-azure-resource-manager-template"></a>Spara Azure Resource Manager-mall
En mall för Azure Resource Manager tillhandahåller en deklarativ metod toodefine repeterbara distribution. hello följande steg förklarar hur toosave hello Azure Resource Manager-mall för hello VM håller på att skapas.
När Spara, kan du använda hello Azure Resource Manager-mall för[distribuera nya virtuella datorer med Azure PowerShell](../azure-resource-manager/resource-group-overview.md#template-deployment).

1. På hello **virtuella** bladet väljer **visa ARM-mallen**.
2. På hello **visa Azure Resource Manager-mall** bladet, Välj hello mallens text.
3. Kopiera hello markerad text toohello Urklipp.
4. Välj **OK** tooclose hello **visa Azure Resource Manager-mall bladet**.
5. Öppna en textredigerare.
6. Klistra in i hello mallens text från Urklipp hello.
7. Spara hello-fil för senare användning.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a>Nästa steg
* En gång hello VM har skapats, du kan ansluta toohello VM genom att välja **Anslut** hello VM-bladet.
* Lär dig hur för[skapa anpassade artefakter för den virtuella datorn med DevTest Labs](devtest-lab-artifact-author.md).
* Utforska hello [DevTest Labs Azure Resource Manager QuickStart mallgalleriet](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
