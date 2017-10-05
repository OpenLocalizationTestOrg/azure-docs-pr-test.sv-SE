---
title: Flexibel programvaruutveckling med Azure App Service
description: "Lär dig hur du skapar hög skalning komplexa program med Azure App Service på ett sätt som har stöd för flexibel programutveckling."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: c0fdb676-36a6-4738-925f-65b4835d187f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.openlocfilehash: 5ed888cbb422766cf2094f5980dfd1c599bd431c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="agile-software-development-with-azure-app-service"></a>Flexibel programvaruutveckling med Azure App Service
I kursen får du lära dig hur du skapar hög skalning komplexa program med [Azure App Service](/azure/app-service/) på ett sätt som stöder [flexibel programvaruutveckling](https://en.wikipedia.org/wiki/Agile_software_development). Det förutsätts att du redan vet hur du [distribuera komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md).

Begränsningar i tekniska processer kan ofta hindra lyckad implementering av flexibel metoder. Azure Apptjänst med funktioner som [kontinuerlig publicering](app-service-continuous-deployment.md), [mellanlagringsmiljöer](web-sites-staged-publishing.md) (platser) och [övervakning](web-sites-monitor.md)när klokt tillsammans med orchestration och hantering av distribution i [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md), kan vara en del av en bra lösning för utvecklare som omfatta flexibel programutveckling.

Följande tabell är en kort lista över kraven som är associerade med en smidig utveckling och hur Azure-tjänster aktivera dem..

| Krav | Hur kan användas för Azure |
| --- | --- |
| -Skapa vid varje incheckning<br>-Skapa automatiskt och snabba |När konfigurerad med kontinuerlig distribution, kan Azure App Service fungera som live körs versioner baserat på en utvecklings-gren. Varje gång koden skickas till grenen är automatiskt inbyggda och körs live i Azure. |
| – Kontrollera skapar själv testa |Läser in tester, webbtester osv, kan distribueras med hjälp av Azure Resource Manager-mallen. |
| -Om du utför testerna i en klon av produktionsmiljö |Azure Resource Manager-mallar kan användas för att skapa kloner i Azure produktionsmiljön (inklusive app-inställningar, anslutning sträng mallar, skalning, etc.) för att testa snabbt och förutsägbart. |
| – Visa resultatet av den senaste versionen enkelt |Kontinuerlig distribution till Azure från en databas innebär att du kan testa ny kod i en levande program omedelbart efter att du genomför ändringarna. |
| -Genomför huvudsakliga grenen varje dag<br>– Automatisera distributionen |Kontinuerlig integrering av ett produktionsprogram med en databas huvudsakliga gren distribuerar automatiskt varje commit/merge huvudsakliga grenen till produktionen. |

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Vad du ska göra
Du får via en typisk dev-test-steg-produktion arbetsflöde för att publicera nya ändringar i den [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) exempelprogrammet, som består av två [webbappar](/services/app-service/web/), en är en klientdel (FE) och andra är en webb-API-serverdel (BE), och en [SQL-databas](/services/sql-database/). Du arbetar med distributionsarkitekturen för följande:

![](./media/app-service-agile-software-development/what-1-architecture.png)

Placera bilden till ord:

* Arkitektur för distribution är uppdelad i tre olika miljöer (eller [resursgrupper](../azure-resource-manager/resource-group-overview.md) i Azure), var och en med sin egen [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), [skalning](web-sites-scale.md) inställningar och SQL-databas. 
* Varje miljö kan hanteras separat. De kan även finnas i olika prenumerationer.
* Mellanlagring och produktion implementeras som två platser i samma App Service-appen. Mastergrenen har konfigurerats för kontinuerlig integrering med mellanlagringsplatsen.
* När ett genomförande till mastergrenen verifieras på mellanlagringsplatsen (med produktionsdata) verifierade fristående app växlas till produktionsplatsen [utan avbrott](web-sites-staged-publishing.md).

Produktions- och mellanlagring miljön definieras av mallen på [  *&lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

Utvecklings- och testmiljöer definieras av mallen på [  *&lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

Du kommer också använda den vanliga grenstruktur strategin med kod som flyttas från grenen dev upp till grenen test, sedan till mastergrenen (flytta i kvalitet, så till speak).

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-need"></a>Vad du behöver
* Ett Azure-konto
* En [GitHub](https://github.com/) konto
* Git Shell (installeras med [GitHub för Windows](https://windows.github.com/)) – kan du köra Git- och PowerShell-kommandon i samma session 
* Senaste [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps) bits
* Grundläggande förståelse av följande verktyg:
  * [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) malldistribution (Se även [distribuerar ett komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md))
  * [Git](http://git-scm.com/documentation)
  * [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> Du behöver ett Azure-konto för att kunna slutföra den här guiden:
> 
> * Du kan [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/) – du får kredit du kan använda för att testa Azure-betaltjänster och även när de används du kan behålla kontot och använda kostnadsfria Azure-tjänster, till exempel Web Apps.
> * Du kan [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -din Visual Studio-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.
> 
> Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="set-up-your-production-environment"></a>Ställ in din produktionsmiljö
> [!NOTE]
> Skriptet som används i den här självstudiekursen automatiskt konfigurerar kontinuerlig publicering från GitHub-lagringsplatsen. Detta kräver att dina GitHub-autentiseringsuppgifter lagras redan i Azure, annars skriptbaserade distributionen misslyckas vid försök att konfigurera inställningar för kontroll av datakälla för web apps. 
> 
> För att lagra dina GitHub-autentiseringsuppgifter i Azure, skapa en webbapp i den [Azure-portalen](https://portal.azure.com/) och [konfigurera GitHub distribution](app-service-continuous-deployment.md). Du behöver bara göra detta en gång. 
> 
> 

Du har ett program som körs live i Azure i ett typiskt scenario för DevOps, och du vill göra ändringar genom kontinuerlig publicering. I det här scenariot har du en mall som utvecklas, testas och används för att distribuera produktionsmiljön. Du kan ange den i det här avsnittet.

1. Skapa din egen förgrening av den [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) databasen. Information om hur du skapar din förgrening finns [duplicera en Repo](https://help.github.com/articles/fork-a-repo/). När din förgrening har skapats kan se du den i webbläsaren.
   
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. Öppna en session för Git-gränssnittet. Om du inte har Git Shell ännu, installera [GitHub för Windows](https://windows.github.com/) nu.
3. Skapa en lokal kloning av din förgrening genom att köra följande kommando:

        git clone https://github.com/<your_fork>/ToDoApp.git 
4. När du har din lokala kloning, gå till  *&lt;repository_root >*\ARMTemplates och köra deploy.ps1 skript på följande sätt:
   
        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git
5. När du uppmanas ange önskat användarnamn och lösenord för åtkomst till databasen.
   
   Du bör se etablering förloppet för olika Azure-resurser. När distributionen är klar startar programmet i webbläsaren skriptet och ger dig ett eget signal.
   
    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
   
   > [!TIP]
   > Ta en titt på  *&lt;repository_root >*\ARMTemplates\Deploy.ps1 att se hur den genererar resurser med unika ID: N. Du kan använda samma metod för att skapa klonerna för samma distribution utan att oroa motstridiga resursnamn.
   > 
   > 
6. Tillbaka i sessionen Git Shell, kör du:
   
        .\swap –Name ToDoApp<unique_string>master
   
    ![](./media/app-service-agile-software-development/production-4-swap.png)
7. Gå tillbaka till Bläddra till den frontend-adress när skriptet har slutförts (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) att se det program som körs i produktion.
8. Logga in på den [Azure-portalen](https://portal.azure.com/) och ta en titt på vad som har skapats.
   
   Du ska kunna se två web apps i samma resursgrupp, en med den `Api` suffix i namnet. Om du tittar på resursgruppvyn se du också SQL-databasen och server App Service-plan och fristående fack för web apps. Bläddra igenom de olika resurserna och jämför dem med  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json att se hur de är konfigurerade i mallen.
   
    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

Du har nu konfigurerat produktionsmiljön. Därefter kommer startar du en ny uppdatering till programmet.

## <a name="create-dev-and-test-branches"></a>Skapa dev och testa filialer
Nu när du har en komplexa program som körs i produktion i Azure, blir en uppdatering till ditt program i enlighet med flexibel metod. I det här avsnittet ska du skapa dev och testa filialer som du behöver du göra alla nödvändiga uppdateringar.

1. Skapa testmiljön. Kör följande kommandon för att skapa miljön för en ny gren kallas i sessionen Git Shell **NewUpdate**. 
   
        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate
2. När du uppmanas ange önskat användarnamn och lösenord för åtkomst till databasen. 
   
   När distributionen är klar startar programmet i webbläsaren skriptet och ger dig ett eget signal. Nu har du en ny gren med sin egen testmiljö. Ta en stund att gå igenom några saker om den här testmiljön:
   
   * Du kan skapa den i Azure-prenumeration. Det innebär att produktionsmiljön kan hanteras separat från din testmiljö.
   * Din testmiljö körs live i Azure.
   * Testmiljön är identisk med produktionsmiljön, förutom för mellanlagring platserna och inställningarna för skalning. Du vet om det. eftersom de är de enda skillnaderna mellan ProdandStage.json och Dev.json.
   * Du kan hantera din testmiljö i sin egen programtjänstplanen till ett annat pris-nivån (exempelvis **lediga**).
   * Ta bort den här testmiljö är lika enkelt som att ta bort resursgruppen. Du lär dig hur du gör detta [senare](#delete).
3. Gå vidare till att skapa en gren dev genom att köra följande kommandon:
   
        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev
4. När du uppmanas ange önskat användarnamn och lösenord för åtkomst till databasen. 
   
   Ta en stund att gå igenom några saker om den här utvecklingsmiljö: 
   
   * Din utvecklingsmiljö har en konfiguration som är identisk med testmiljön eftersom den distribueras med hjälp av samma mall.
   * Varje utvecklingsmiljö kan skapas i utvecklarens egen Azure-prenumerationen, lämnar testmiljön hanteras separat.
   * Din utvecklingsmiljö körs live i Azure.
   * Ta bort utvecklingsmiljö är så enkelt som att ta bort resursgruppen. Du lär dig hur du gör detta [senare](#delete).

> [!NOTE]
> När du har flera utvecklare som arbetar på den nya uppdateringen skapa dem enkelt en gren och dedikerad utvecklingsmiljö med följande steg:
> 
> 1. Skapa egna förgrening av lagringsplatsen i GitHub (se [duplicera en Repo](https://help.github.com/articles/fork-a-repo/)).
> 2. Klona förgrening på användarens lokala dator
> 3. Kör samma kommandon för att skapa egna dev gren och miljö.
> 
> 

När du är klar bör din GitHub-förgrening ha tre grenar:

![](./media/app-service-agile-software-development/test-1-github-view.png)

Och du bör ha sex webbappar (tre uppsättningar av två) i tre separata resursgrupper:

![](./media/app-service-agile-software-development/test-2-all-webapps.png)

> [!NOTE]
> ProdandStage.json anger produktionsmiljön för att använda den **Standard** prisnivån som är lämplig för skalbarhet av programmet för produktion.
> 
> 

## <a name="build-and-test-every-commit"></a>Skapa och testa varje incheckning
Mallfilerna ProdAndStage.json och Dev.json ange redan kontrollparametrar källa som standard konfigurerar kontinuerlig publicering för webbprogrammet. Varje incheckning GitHub-grenen utlöser därför en automatisk distribution till Azure av grenen. Nu ska vi se hur inställningarna fungerar nu.

1. Kontrollera att du är i grenen utveckling av lokal databas. Gör detta genom att köra följande kommando i Git-gränssnittet:
   
        git checkout Dev
2. Gör en ändring i appens användargränssnitt layer genom att ändra koden för att använda [Bootstrap](http://getbootstrap.com/components/) visas. Öppna  *&lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml och gör följande markerade ändra:
   
    ![](./media/app-service-agile-software-development/commit-1-changes.png)
   
    > [!NOTE]
    > Om du inte kan läsa den föregående bilden: 
    > 
    > * I rad 18 ändrar `check-list` till `list-group`.
    > * I rad 19 ändrar `class="check-list-item"` till `class="list-group-item"`.
    > 
    > 
3. Spara ändringen. Tillbaka i Git Shell, kör du följande kommandon:
   
        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev
   
   Kommandona git liknar ”kontrollerar i kod” i en annan källkontrollsystem som TFS. När du kör `git push`, nya genomförandet utlöser en automatisk kod push till Azure, vilket återskapas sedan programmet som ska ändras i dev-miljö.
4. För att verifiera att den här koden push till din utvecklingsmiljö har inträffat, gå till din utvecklingsmiljö webbsida app och titta på den **distribution** del. Du ska kunna se senaste genomförande meddelandet det.
   
    ![](./media/app-service-agile-software-development/commit-2-deployed.png)
5. Därifrån klickar du på **Bläddra** vill se nya ändringar i programmet live i Azure.
   
    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)
   
   Det är en mindre ändring till programmet. Men många gånger nya ändringar till ett komplexa webbprogram har oönskade och oönskade sidoeffekter. Att kunna testa enkelt varje incheckning i live versioner kan du fånga problemen innan kunderna ser dem.

Nu bör du vara nöjd med genomförandet som utvecklare på den **NewUpdate** -projekt kan du skapa en utvecklingsmiljö för dig själv, och sedan skapa varje incheckning och testa alla build.

## <a name="merge-code-into-test-environment"></a>Sammanfoga koden i testmiljö
När du är redo att skicka koden från Dev gren upp till NewUpdate gren är standard git-processen:

1. Sammanfoga alla nya incheckningar till NewUpdate i grenen Dev i GitHub, till exempel incheckningar som skapats av andra utvecklare. Alla nya genomför en kod push i GitHub-utlösare och bygga i dev-miljö. Du kan kontrollera din kod i Dev grenen fungerar fortfarande med de senaste bitarna NewUpdate utgår ifrån.
2. Slå samman alla nya incheckningar från Dev gren till NewUpdate gren på GitHub. Den här åtgärden utlöser en kod push- och versionsinformation i testmiljön. 

Observera igen att eftersom kontinuerlig distribution har redan konfigurerats med dessa git filialer, inte behöver du vidta några åtgärder som att köra integration skapar. Du behöver utföra standard källa kontrollen metoder med git och Azure utför alla build-processer för dig.

Nu ska vi skicka koden till **NewUpdate** grenen. Git Shell, kör följande kommandon:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

Klart! 

Gå till sidan web app för testmiljön att se din nya commit (sammanfogas NewUpdate gren) nu pushas till testmiljön. Klicka på **Bläddra** att format ändra nu körs live i Azure.

## <a name="deploy-update-to-production"></a>Distribuera till produktion
Skicka koden till mellanlagring och produktion miljö bör du inte annorlunda än vad du redan har gjort när du pushas kod till testmiljön. Det är mycket enkelt. 

Git Shell, kör följande kommandon:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Kom ihåg att baserat på miljön mellanlagring och produktion är inställt i ProdandStage.json, din nya kod skickas till den **mellanlagring** fack och kör det. Så om du navigerar till den mellanlagringsplatsen URL kan du se den nya koden körs det. Om du vill göra det, kör du följande cmdlet i Git-gränssnittet.

    Start-Process -FilePath "http://ToDoApp<unique_string>master-Staging.azurewebsites.net"

Och när du har kontrollerat uppdateringen i mellanlagringsplatsen återstår för bara om du vill göra är nu att växla till produktion. Git Shell, kör du bara följande kommandon:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Grattis! Du har publicerat en ny uppdatering i ditt webbprogram för produktion. Dessutom är som gjorde det enkelt att skapa utvecklings- och testmiljöer och skapa och testa varje incheckning. Detta är avgörande byggblock för flexibel programutveckling.

<a name="delete"></a>

## <a name="delete-dev-and-test-environments"></a>Ta bort utvecklings- och testmiljöer
Eftersom du har ändamålsenligt konstruerad din dev- och testmiljöer för att självständigt resursgrupper, är det enkelt att ta bort dem. För att ta bort som skapade du i den här självstudiekursen, både GitHub filialer och Azure artefakter som bara kör följande kommandon i Git-gränssnittet:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Sammanfattning
Flexibel programutveckling är en måste ha för många företag som vill använda Azure som deras plattform. I den här kursen har du lärt dig hur du skapar och går sönder exakt repliker eller nära repliker i produktionsmiljön med enkel även för komplexa program. Du har lärt dig hur man utnyttjar denna möjlighet att skapa en utvecklingsprocessen som kan skapa och testa varje enskild incheckning i Azure. Den här självstudiekursen förhoppningsvis visar du hur du bäst använder Azure App Service och Azure Resource Manager tillsammans för att skapa en DevOps-lösning som caters till flexibel metoder. Därefter du kan bygga vidare på det här scenariot genom att utföra avancerade DevOps-metoder som [test i produktion](app-service-web-test-in-production-get-start.md). Ett vanligt scenario för testning i produktionsmiljö Se [Flighting distribution (betatestning) i Azure App Service](app-service-web-test-in-production-controlled-test-flight.md).

## <a name="more-resources"></a>Fler resurser
* [Distribuera ett komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md)
* [Flexibel utveckling i praktiken: Tips och trick för moderniserade utvecklingscykeln](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
* [Avancerad distribution för webbprogram i Azure med hjälp av Resource Manager-mallar](http://channel9.msdn.com/Events/Build/2015/2-620)
* [Redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md)
* [JSONLint - JSON-verifieraren](http://jsonlint.com/)
* [ARMClient – konfigurera GitHub publicering till plats](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
* [Git förgrening – grundläggande förgrening och sammanfogning](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [David Ebbo blogg](http://blog.davidebbo.com/)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure plattformsoberoende kommandoradsverktyg](../cli-install-nodejs.md)
* [Skapa eller redigera användare i Azure AD](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
* [Projektet Kudu-Wiki](https://github.com/projectkudu/kudu/wiki)

