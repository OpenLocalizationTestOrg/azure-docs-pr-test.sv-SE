---
title: aaaManage formler i Azure DevTest Labs toocreate VMs | Microsoft Docs
description: "Lär dig hur tooupdate och ta bort Azure DevTest Labs formler"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 855debe46f3b70cc45ea5d55869663b64e225124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-devtest-labs-formulas"></a>Hantera Azure DevTest Labs formler

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

Den här artikeln beskrivs hur toocreate en formel från en bas (anpassad avbildning, Marketplace-avbildning eller en annan metod) eller en befintlig virtuell dator. Den här artikeln hjälper dig att hantera befintliga formler också.

## <a name="create-a-formula"></a>Skapa en formel
Alla med DevTest Labs *användare* behörigheter är kan toocreate virtuella datorer med en formel som bas. Det finns två sätt toocreate formler: 

* Använd när du vill toodefine alla hello av hello formeln egenskaper från en bas.
* Använd när du vill toocreate en formel baserat på hello inställningarna för en befintlig virtuell dator från en befintlig lab VM.

Mer information om att lägga till användare och behörigheter finns [lägga till ägare och användare i Azure DevTest Labs](./devtest-lab-add-devtest-user.md).

### <a name="create-a-formula-from-a-base"></a>Skapa en formel från en bas
hello följande steg leder dig igenom hello processen att skapa en formel från en anpassad avbildning, Marketplace-avbildning eller en annan formel.

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).

2. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.

3. Välj önskad hello-labb hello listan övningar.  

4. På bladet hello lab väljer **formler (återanvändbara baser)**.
   
    ![Formeln menyn](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. På hello **formler** bladet väljer **+ Lägg till**.
   
    ![Lägga till en formel](./media/devtest-lab-create-formulas/add-formula.png)

6. På hello **väljer du en bas** bladet välj hello base (anpassad avbildning, Marketplace-avbildning eller metod) som du vill toocreate hello formel.
   
    ![Grundläggande lista](./media/devtest-lab-create-formulas/base-list.png)

7. På hello **skapa formeln** bladet ange hello följande värden:
   
    * **Formelnamnet** -ange ett namn för din formel. Det här värdet visas i hello lista över grundläggande bilder när du skapar en virtuell dator. verifiera hello namnet som du skriver du det och om det är inte giltig, ett meddelande visar hello kraven för ett giltigt namn.
    * **Beskrivning** -ange en beskrivning för din formel. Det här värdet är tillgängligt hello formeln snabbmenyn när du skapar en virtuell dator.
    * **Användarnamnet** -ange ett användarnamn som har beviljats administratörsbehörighet.
    * **Lösenordet** – ange - eller välj hello listrutan - ett-värde som är associerad med hello hemlighet (lösenord) som du vill toouse för hello angiven användare. Läs mer om hello hemligheter [Azure DevTest Labs: hemliga datorarkivet](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).
    * **Typ av virtuell dator disk** – ange antingen Hårddisk (hårddiskenheten) eller SSD (SSD) tooindicate vilka lagring disktyp tillåts för hello virtuella datorer som etablerats med hjälp av den här grundläggande bild.
    * ** Virtuella storlek ** – Välj något av hello fördefinierade objekt som anger hello processorkärnor, ram-storlek och hello hårddiskstorlek av hello VM toocreate. 
    * **Artefakter** -väljer tooopen hello **lägga till artefakter** bladet, där du kan välja och konfigurera hello artefakter som du vill tooadd toohello basavbildning. Läs mer om artefakter [hantera VM artefakter i Azure DevTest Labs](./devtest-lab-add-vm-with-artifacts.md).
    * **Avancerade inställningar** -väljer tooopen hello **Avancerat** bladet där du konfigurerar hello följande inställningar:
        * **Virtuellt nätverk** -ange hello önskas virtuellt nätverk.
        * **Undernät** -ange hello önskad undernät.    
        * **IP-adresskonfiguration** -ange om du vill hello Public, Private eller delade IP-adresser. Mer information om delade IP-adresser finns [Förstå delade IP-adresser i Azure DevTest Labs](./devtest-lab-shared-ip.md).
        * **Se den här datorn claimable** -gör en dator ”claimable” innebär att den inte tilldelas ägarskap för närvarande hello skapas. I stället aktiveras lab kan tootake ägarskap (”anspråk”) hello datorn hello labb-bladet.     
    * **Bild** -fältet visar namnet på hello basavbildning som du har valt på hello föregående blad. 
     
       ![Skapa formeln](./media/devtest-lab-create-formulas/create-formula.png)

8. Välj **skapa** toocreate hello formel.

9. När hello formeln har skapats visas i listan hello på hello **formler** bladet.

### <a name="create-a-formula-from-a-vm"></a>Skapa en formel från en virtuell dator
hello leder följande steg dig igenom hello processen att skapa en formel som baseras på en befintlig virtuell dator. 

> [!NOTE]
> toocreate en formel från en virtuell dator, hello VM måste ha skapats efter den 30 mars 2016. 
> 
> 

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
3. Välj önskad hello-labb hello listan övningar.  
4. På hello lab **översikt** bladet välj hello VM som du vill toocreate hello formel.
   
    ![Labs virtuella datorer](./media/devtest-lab-create-formulas/my-vms.png)
5. På bladet hello VM väljer **skapa formeln (återanvändbara base)**.
   
    ![Skapa formeln](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. På hello **skapa formeln** bladet anger du en **namn** och **beskrivning** för din nya formel.
   
    ![Skapa formeln bladet](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. Välj **OK** toocreate hello formel.

## <a name="modify-a-formula"></a>Ändra en formel
toomodify en formel, Följ dessa steg:

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
3. Välj önskad hello-labb hello listan övningar.  
4. På bladet hello lab väljer **formler (återanvändbara baser)**.
   
    ![Formeln menyn](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. På hello **Lab formler** bladet, Välj hello formeln som du vill toomodify.
6. På hello **uppdatera formel** bladet hello vill redigera och välj **uppdatering**.

## <a name="delete-a-formula"></a>Ta bort en formel
toodelete en formel, Följ dessa steg:

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
3. Välj önskad hello-labb hello listan övningar.  
4. På hello lab **inställningar** bladet väljer **formler**.
   
    ![Formeln menyn](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. På hello **Lab formler** bladet, Välj hello ellips toohello höger i hello formeln gärna toodelete.
   
    ![Formeln menyn](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. På snabbmenyn för hello formel, väljer **ta bort**.
   
    ![Formeln snabbmenyn](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. Välj **Ja** toohello bekräftelsedialogruta för borttagning.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Relaterade blogginlägg
* [Anpassade avbildningar eller formler?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Nästa steg
När du har skapat en formel för användning när du skapar en virtuell dator, hello nästa steg är för[lägga till ett labb för VM-tooyour](devtest-lab-add-vm-with-artifacts.md).

