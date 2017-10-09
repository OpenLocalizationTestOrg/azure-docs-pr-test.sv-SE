---
title: "aaaCreate flera Virtuella miljöer och PaaS-resurser med Azure Resource Manager-mallar | Microsoft Docs"
description: "Lär dig hur toocreate flera Virtuella miljöer och PaaS-resurser i Azure DevTest Labs från en Azure Resource Manager-mall"
services: devtest-lab,virtual-machines,visual-studio-online
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
ms.date: 01/31/2017
ms.author: tarcher
ms.openlocfilehash: ab8628f6cb5a666435258efb93921ec69ad3a13a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>Skapa flera Virtuella miljöer och PaaS-resurser med Azure Resource Manager-mallar

Hej [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040) kan du tooeasily [skapa och Lägg till ett labb för VM-tooa](https://docs.microsoft.com/en-us/azure/devtest-lab/devtest-lab-add-vm). Detta fungerar bra för att skapa en virtuell dator i taget. Men har hello miljön innehåller flera virtuella datorer, toobe skapas individuellt på varje virtuell dator. För scenarier, till exempel en webbapp i flera nivåer eller en SharePoint-grupp är en mekanism nödvändiga tooallow för hello skapa flera virtuella datorer i ett enda steg. Med hjälp av Azure Resource Manager-mallar kan du nu ange hello-infrastrukturen och konfigurationen av din lösning för Azure och upprepade gånger distribuera flera virtuella datorer i ett konsekvent tillstånd. Den här funktionen ger hello följande fördelar:

- Azure Resource Manager-mallar har lästs in direkt från din källkontroll (GitHub eller Team Services Git).
- När du konfigurerat dina användare kan skapa en miljö genom att välja en Azure Resource Manager-mall från hello Azure-portalen som de kan göra med andra typer av [VM baser](./devtest-lab-comparing-vm-base-image-types.md).
- Azure PaaS-resurser kan etableras i en miljö med en Azure Resource Manager-mall i tillägg tooIaaS virtuella datorer.
- hello kostnaden för miljöer kan spåras i hello labb i tillägg tooindividual virtuella datorer som skapats av andra typer av databaser.
- PaaS-resurser skapas och visas i Kostnadsuppföljning; dock gäller inte automatisk avstängning på VM tooPaaS resurser.
- Användare har hello samma VM-principkontroll för miljöer som de har för enskild lab virtuella datorer.

Mer information om hello många [fördelarna med att använda Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#the-benefits-of-using-resource-manager) toodeploy, uppdatera eller ta bort alla dina lab resurser i en enda åtgärd.

> [!NOTE]
> När du använder en Resource Manager-mall som en bas-toocreate mer lab VMs, finns det vissa skillnader tookeep i åtanke om du skapar flera virtuella datorer eller enskild för virtuella datorer. Använd en virtuell dators Azure Resource Manager-mall beskrivs skillnaderna i detalj.
>
>

## <a name="configure-azure-resource-manager-template-repositories"></a>Konfigurera databaser för Azure Resource Manager-mall

Som en hello ska metodtips med infrastruktur som koden och konfigurationen som koden, miljö mallar hanteras i källkontroll. Azure DevTest Labs följer den här övningen och läser in alla mallar för Azure Resource Manager direkt från GitHub eller VSTS Git-databaser. Resource Manager-mallar användas över hello hela versionen livscykel, från hello test toohello produktionsmiljön.

Det finns ett par regler toofollow tooorganize Azure Resource Manager-mallar i en databas:

- hello Huvudmall filen måste ha namnet `azuredeploy.json`. 

    ![Nyckeln Azure Resource Manager mallfilerna](./media/devtest-lab-create-environment-from-arm/master-template.png)

- Om du vill toouse parametervärden som definierats i en parameterfil hello parameterfil måste ha namnet `azuredeploy.parameters.json`.
- Du kan använda hello parametrar `_artifactsLocation` och `_artifactsLocationSasToken` tooconstruct hello parametersLink URI-värde, vilket gör att DevTest Labs tooautomatically hantera kapslade mallar. Se [hur Azure DevTest Labs underlättar kapslade Resource Manager mall-distributioner för testmiljöer](https://blogs.msdn.microsoft.com/devtestlab/2017/05/23/how-azure-devtest-labs-makes-nested-arm-template-deployments-easier-for-testing-environments/) för mer information.
- Metadata kan vara definierade toospecify hello Mallens visningsnamn och en beskrivning. Dessa metadata måste vara i en fil med namnet `metadata.json`. hello visar metadatafil i följande exempel hur toospecify hello visas namn och beskrivning: 

```json
{
 
"itemDisplayName": "<your template name>",
 
"description": "<description of hello template>"
 
}
```

hello leder följande steg dig genom att lägga till ett labb för tooyour av databasen med hjälp av hello Azure-portalen. 

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
1. Välj önskad hello-labb hello listan övningar.   
1. På bladet hello lab väljer **konfiguration och principer**.

    ![Konfiguration och principer](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. Från hello **konfiguration och principer** inställningslistan markerar **databaser**. Hej **databaser** bladet visar hello-databaser som har lagts till toohello labbet. En databas med namnet `Public Repo` genereras automatiskt för alla testlabb och ansluter toohello [DevTest Labs GitHub-repo-](https://github.com/Azure/azure-devtestlab) som innehåller flera Virtuella artefakter för din användning.

    ![Offentliga lagringsplatsen](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. Välj **Lägg till +** tooadd din mall Azure Resource Manager-databasen.
1. När hello andra **databaser** öppnas bladet anger hello nödvändig information på följande sätt:
    - **Namnet** -ange hello databasnamn som används i hello labbet.
    - **URL för Git-klon** -ange hello klon-URL för GIT HTTPS från GitHub eller Visual Studio Team Services.  
    - **Branch** -ange hello gren namn tooaccess Azure Resource Manager mallen definitionerna. 
    - **Personlig åtkomsttoken** -hello personliga åtkomsttoken används toosecurely åtkomst till databasen. tooget din token från Visual Studio Team Services, Välj  **&lt;dittnamn >> min profil > Säkerhet > offentlig åtkomst-token**. tooget din token från GitHub, Välj din avatar följt genom att välja **Inställningar > offentlig åtkomst-token**. 
    - **Mappsökvägar** – med någon av hello två inmatningsfält, ange hello mappsökväg som börjar med ett snedstreck - / - och är relativ tooyour Git klona URI tooeither dina artefaktdefinitioner (första inmatningsfältet) eller Azure Resource Manager-mall definitioner.   
    
        ![Offentliga lagringsplatsen](./media/devtest-lab-create-environment-from-arm/repo-values.png)

1. När alla hello krävs fält har angetts och valideras hello, välja **spara**.

hello nästa avsnitt får du hjälp med att skapa miljöer från en Azure Resource Manager-mall.

## <a name="create-an-environment-from-an-azure-resource-manager-template"></a>Skapa en miljö med en Azure Resource Manager-mall

När en mall för Azure Resource Manager-databasen har konfigurerats i hello labbet, kan lab-användare skapa en miljö med hjälp av Azure-portalen med hello följande steg:

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
1. Välj önskad hello-labb hello listan övningar.   
1. På bladet hello lab väljer **Lägg till +**.
1. Hej **väljer du en bas** bladet visar grundläggande hello bilder som du kan använda med hello Azure Resource Manager-mallar som visas först. Välj hello önskade Azure Resource Manager-mall.

    ![Välj en bas](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. På hello **Lägg till** bladet ange hello **miljönamn** värde. hello-miljönamn är vad är visas tooyour användare i hello labbet. hello återstående indatafält har definierats i hello Azure Resource Manager-mall. Om standardvärden definieras i mallen hello eller hello `azuredeploy.parameter.json` -fil, standardvärden visas i dessa indatafälten. För parametrar av typen *säker sträng*, du kan använda hello hemligheter som lagras i hello lab [hemliga datorarkivet](https://azure.microsoft.com/en-us/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store).

    ![Bladet Lägg till](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > Det finns flera parametervärden som – även om angett - visas som tomma värden. Därför om användare tilldelar dessa värden tooparameters i en Azure Resource Manager-mall, visar DevTest Labs inte hello värden. i stället visar tomt inmatningsfält där hello lab användare behöver tooenter värdet när du skapar hello-miljö.
    > 
    > - GEN UNIKT
    > - GEN - UNIKT-[N]
    > - GEN SSH-PUB-NYCKEL
    > - GEN-LÖSENORD 
 
1. Välj **Lägg till** toocreate hello miljö. hello miljö startar etablering omedelbart med hello status visas i hello **Mina virtuella datorer** lista. En ny resursgrupp skapas automatiskt av hello lab tooprovision alla hello-resurser som definierats i mallen för hello Azure Resource Manager.
1. När du har skapat hello miljö väljer hello miljö i hello **Mina virtuella datorer** listan tooopen hello resursgruppsbladet och bläddra igenom alla hello resurser som etablerades i hello-miljö.
    
    ![Min virtuella datorer.](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)
   
   Du kan också expandera hello miljö tooview bara hello lista över virtuella datorer som tillhandahålls i hello-miljö.
    
    ![Min virtuella datorer.](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Klicka på någon av hello miljöer tooview hello tillgängliga åtgärder – till exempel använda artefakter, bifoga datadiskar, ändra automatisk avstängning tid och mycket mer.

    ![Åtgärder för miljö](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="next-steps"></a>Nästa steg
* När en virtuell dator har skapats kan du ansluta toohello VM genom att välja **Anslut** hello VM-bladet.
* Visa och hantera resurser i en miljö genom att välja hello miljö i hello **Mina virtuella datorer** lista i labbet. 
* Utforska hello [Azure Resource Manager-mallar från Azure Quickstart mallgalleriet](https://github.com/Azure/azure-quickstart-templates)
