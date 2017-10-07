---
title: aaaAdd Git-lagringsplatsen tooa labb i Azure DevTest Labs | Microsoft Docs
description: "Lägg till en GitHub eller Visual Studio Team Services Git-lagringsplats för anpassade artefakter-källan i Azure DevTest Labs"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 01b459f7-eaf2-45a8-b4b5-2c0a821b33c8
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
ms.openlocfilehash: e590559ffb2d497e39823e35c3f66974f42f13c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-git-repository-toostore-custom-artifacts-and-azure-resource-manager-templates"></a>Lägg till en Git-lagringsplatsen toostore anpassade artefakter och Azure Resource Manager-mallar

Om du vill använda för[skapa anpassade artefakter](devtest-lab-artifact-author.md) för hello virtuella datorer i ditt labb eller [använder Azure Resource Manager mallar toocreate en anpassad testmiljö](devtest-lab-create-environment-from-arm.md), måste du också lägga till ett privat Git-lagringsplatsen tooinclude hello artefakter eller Azure Resource Manager-mallar som skapas av ditt team. hello databasen kan finnas på [GitHub](https://github.com) eller på [Visual Studio Team Services VSTS ()](https://visualstudio.com).

Vi har angett en [Github-lagringsplatsen för artefakter](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) som du kan distribuera som-är eller anpassa för din testlabb. När du anpassa eller skapa en artefakt, du kan inte lagra dem i hello offentliga lagringsplats – måste du skapa dina egna privata lagringsplatsen. 

När du skapar en virtuell dator kan du spara hello Azure Resource Manager-mall, anpassa den om du vill ha och sedan använda den senare tooeasily skapa flera virtuella datorer. Du måste skapa egna privata databasen toostore dina anpassade mallar för Azure Resource Manager.  

* hur toocreate GitHub-lagringsplatsen, se toolearn [GitHub Bootcamp](https://help.github.com/categories/bootcamp/).
* hur toocreate ett Team Services-projekt med en Git-lagringsplats Se toolearn [ansluta tooVisual Studio Team Services](https://www.visualstudio.com/get-started/setup/connect-to-visual-studio-online).

hello visar följande skärmbild ett exempel på hur en databas som innehåller artefakter kan se ut i GitHub:  
![Exempel på GitHub-repo-artefakter](./media/devtest-lab-add-repo/devtestlab-github-artifact-repo-home.png)

## <a name="get-hello-repository-information-and-credentials"></a>Hämta hello databasen information och referenser
tooadd ett databasen tooyour labb måste du först hämta viss information från databasen. hello följande avsnitt får du hjälp att hämta denna information för databaser som finns på GitHub och Visual Studio Team Services.

### <a name="get-hello-github-repository-clone-url-and-personal-access-token"></a>Hämta hello GitHub-lagringsplatsen klon-URL och personlig åtkomst-token
klon-URL för tooget hello GitHub-lagringsplatsen och personliga åtkomsttoken, så här:

1. Bläddra toohello startsida hello GitHub-lagringsplats som innehåller hello artefakt eller Malldefinitioner av Azure Resource Manager.
2. Välj **kloning eller hämta**.
3. Välj hello knappen toocopy hello **HTTPS klona url** toohello Urklipp och spara hello URL för senare användning.
4. Välj hello profil avbildning i hello övre högra hörnet i GitHub och välj **inställningar**.
5. I hello **personliga inställningar** menyn på hello vänster, väljer **personlig åtkomsttoken**.
6. Välj **Skapa ny token**.
7. På hello **ny personliga åtkomsttoken** anger en **Token beskrivning**, acceptera hello Standardobjekt i hello **väljer du**, och välj sedan **Generera Token**.
8. Spara hello genereras token som du behöver den senare.
9. Nu kan du stänga GitHub.   
10. Fortsätta toohello [ansluta lab toohello databasen](#connect-your-lab-to-the-repository) avsnitt.

### <a name="get-hello-visual-studio-team-services-repository-clone-url-and-personal-access-token"></a>Hämta klon-URL för hello Visual Studio Team Services-databasen och personlig åtkomst-token
klon-URL för tooget hello Visual Studio Team Services databasen och personliga åtkomsttoken, så här:

1. Öppna hello startsidan i team-samling (t.ex, `https://contoso-web-team.visualstudio.com`), och välj sedan projektet.
2. På startsidan för hello-projektet, Välj **kod**.
3. tooview hello klon-URL, på hello projektet **kod** väljer **klona**.
4. Spara hello URL när du behöver den senare i den här kursen.
5. toocreate en personlig åtkomst-Token, Välj **min profil** hello användarens konto nedrullningsbara menyn.
6. Markera på hello profil informationssidan **säkerhet**.
7. På hello **säkerhet** väljer **Lägg till**.
8. I hello **skapa en personlig åtkomsttoken** sidan:

   * Ange en **beskrivning** för hello-token.
   * Välj **180 dagar** från hello **upphör att gälla i** lista.
   * Välj **alla tillgängliga konton** från hello **konton** lista.
   * Välj hello **alla scope** alternativet.
   * Välj **skapa Token**.
9. När du är klar visas hello ny token i hello **personlig åtkomsttoken** lista. Välj **kopiera Token**, och spara token hello-värde för senare användning.
10. Fortsätta toohello [ansluta lab toohello databasen](#connect-your-lab-to-the-repository) avsnitt.

## <a name="connect-your-lab-toohello-repository"></a>Ansluta lab toohello databasen
1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
3. Välj önskad hello-labb hello listan övningar.   
4. Välj på hello till vänster **konfiguration och principer**.
5. På hello lab **konfiguration och principer** väljer **databaser**.
6. På hello **databaser** väljer **+ Lägg till**.

    ![Databasen knappen Lägg till](./media/devtest-lab-add-repo/devtestlab-add-repo.png)
7. På hello andra **databaser** anger hello följande information:

   * **Namnet** -ange ett namn för hello-databasen.
   * **Url för Git-klon** -ange hello Git HTTPS klon-URL som du kopierade tidigare från GitHub eller Visual Studio Team Services.
   * **Branch** -ange hello gren tooget definitionerna.
   * **Personlig åtkomst-Token** -ange hello personlig åtkomst-token som du tidigare hämtade från GitHub eller Visual Studio Team Services.
   * **Mappsökvägar** -ange minst en mapp sökväg relativa toohello klon-URL som innehåller din artefakt eller Malldefinitioner av Azure Resource Manager-. När du anger en underkatalog gör att tooinclude hello snedstreck i mappsökvägen hello.

     ![Databaser område](./media/devtest-lab-add-repo/devtestlab-repo-blade.png)
8. Välj **Spara**.

## <a name="next-steps"></a>Nästa steg
När du har skapat ditt privata Git-lagringsplatsen, kan du göra något eller båda av följande hello, beroende på dina behov:
* Lagra dina [anpassade artefakter](devtest-lab-artifact-author.md), som du kan använda senare toocreate nya virtuella datorer.
* [Skapa flera Virtuella miljöer och PaaS-resurser med Azure Resource Manager-mallar](devtest-lab-create-environment-from-arm.md) och sedan lagra hello mallar i ditt privata lagringsplatsen.

När du skapar en virtuell dator kan du kontrollera att hello artefakter eller mallar läggs tooyour Git-lagringsplats. De är tillgängliga direkt i hello lista över artefakter eller mallar med hello namnet på din privata lagringsplatsen som visas i hello-kolumn som anger hello källa. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="related-blog-posts"></a>Relaterade blogginlägg
* [Hur tootroubleshoot misslyckas artefakter i Azure DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md)
* [Ansluta till en VM-tooexisting AD-domän med hjälp av en resource manager-mall i Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)
