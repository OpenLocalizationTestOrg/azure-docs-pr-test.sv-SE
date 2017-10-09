---
title: aaaAgile programvaruutveckling med Azure App Service
description: "Lär dig hur toocreate hög skalning komplexa program med Azure App Service på ett sätt som har stöd för flexibel programutveckling."
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
ms.openlocfilehash: a1c1c78cfff711774943b0235ed762f03f48fc6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="agile-software-development-with-azure-app-service"></a>Flexibel programvaruutveckling med Azure App Service
I den här kursen får du lära dig hur toocreate hög skalning komplexa program med [Azure App Service](/azure/app-service/) på ett sätt som stöder [flexibel programvaruutveckling](https://en.wikipedia.org/wiki/Agile_software_development). Det förutsätts att du redan vet hur för[distribuera komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md).

Begränsningar i tekniska processer kan ofta vara hello sätt genomföras flexibel metoder. Azure Apptjänst med funktioner som [kontinuerlig publicering](app-service-continuous-deployment.md), [mellanlagringsmiljöer](web-sites-staged-publishing.md) (platser) och [övervakning](web-sites-monitor.md)när klokt tillsammans med hello orchestration och hantering av distribution i [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md), kan vara en del av en bra lösning för utvecklare som omfatta flexibel programutveckling.

hello följande tabell är en kort lista över kraven som är associerade med en smidig utveckling och hur Azure-tjänster aktivera dem..

| Krav | Hur kan användas för Azure |
| --- | --- |
| -Skapa vid varje incheckning<br>-Skapa automatiskt och snabba |När konfigurerad med kontinuerlig distribution, kan Azure App Service fungera som live körs versioner baserat på en utvecklings-gren. Varje gång koden skickas toohello gren, är det automatiskt inbyggda och körs live i Azure. |
| – Kontrollera skapar själv testa |Läser in tester, webbtester osv, kan distribueras med hello Azure Resource Manager-mall. |
| -Om du utför testerna i en klon av produktionsmiljö |Azure Resource Manager-mallar kan vara används toocreate kloner av hello Azure produktionsmiljö (inklusive app-inställningar, anslutning sträng mallar, skalning, etc.) för att testa snabbt och förutsägbart. |
| – Visa resultatet av den senaste versionen enkelt |Kontinuerlig distribution tooAzure från en databas innebär att du kan testa ny kod i en levande program omedelbart efter att du genomför ändringarna. |
| -Genomför toohello huvudsakliga gren varje dag<br>– Automatisera distributionen |Kontinuerlig integrering av ett produktionsprogram med en databas huvudsakliga gren distribuerar automatiskt varje commit/merge toohello huvudsakliga gren tooproduction. |

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Vad du ska göra
Du får via en typisk dev-test-steg-produktion arbetsflöde i ordning toopublish nya ändringar toohello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) exempelprogrammet, som består av två [webbappar](/services/app-service/web/), en är en klientdel (FE) och Hej andra är en webb-API-serverdel (BE), och en [SQL-databas](/services/sql-database/). Du arbetar med hello följande arkitektur för distribution:

![](./media/app-service-agile-software-development/what-1-architecture.png)

tooput hello bild till ord:

* arkitektur för hello-distribution är uppdelad i tre olika miljöer (eller [resursgrupper](../azure-resource-manager/resource-group-overview.md) i Azure), var och en med sin egen [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), [skalning](web-sites-scale.md) inställningar och SQL-databas. 
* Varje miljö kan hanteras separat. De kan även finnas i olika prenumerationer.
* Mellanlagring och produktion implementeras som två platser för hello samma App Service-appen. Hej mastergrenen har konfigurerats för kontinuerlig integration med hello mellanlagringsplatsen.
* När en commit toomaster gren verifieras på hello mellanlagringsplatsen (med produktionsdata) Hej kontrolleras fristående app växlas till hello produktionsplatsen [utan avbrott](web-sites-staged-publishing.md).

hello produktions- och mellanlagring miljö definieras av hello mall när [  *&lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

Hej dev och testmiljöer definieras av hello mall när [  *&lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

Du kommer också använda hello vanliga grenstruktur strategi, med kod som flyttas från hello dev gren in toohello test gren och toohello mastergrenen (flytta i kvalitet, så toospeak).

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-need"></a>Vad du behöver
* Ett Azure-konto
* En [GitHub](https://github.com/) konto
* Git Shell (installeras med [GitHub för Windows](https://windows.github.com/))-aktiverar du toorun både hello Git och PowerShell-kommandon i hello samma session 
* Senaste [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps) bits
* Grundläggande förståelse för hello följande verktyg:
  * [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) malldistribution (Se även [distribuerar ett komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md))
  * [Git](http://git-scm.com/documentation)
  * [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> Du behöver ett Azure-konto toocomplete den här kursen:
> 
> * Du kan [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/) – du får kredit du kan använda tootry ut betald Azure-tjänster och även när de används du kan behålla hello kontot och använda kostnadsfria Azure-tjänster, till exempel Web Apps.
> * Du kan [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -din Visual Studio-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.
> 
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="set-up-your-production-environment"></a>Ställ in din produktionsmiljö
> [!NOTE]
> hello-skriptet som används i den här självstudiekursen automatiskt konfigurerar kontinuerlig publicering från GitHub-lagringsplatsen. Detta kräver att dina GitHub-autentiseringsuppgifter lagras redan i Azure, annars hello skripta distribution misslyckas vid försök tooconfigure inställningar för källkontrollen för hello web apps. 
> 
> toostore din GitHub autentiseringsuppgifter i Azure, skapa en webbapp i hello [Azure-portalen](https://portal.azure.com/) och [konfigurera GitHub distribution](app-service-continuous-deployment.md). Du behöver bara toodo detta en gång. 
> 
> 

Du har ett program som körs live i Azure i ett typiskt scenario för DevOps, och du vill toomake ändringar tooit genom kontinuerlig publicering. I det här scenariot har du en mall som du utvecklade, testas och används toodeploy hello produktionsmiljön. Du kan ange den i det här avsnittet.

1. Skapa din egen förgrening av hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) databasen. Information om hur du skapar din förgrening finns [duplicera en Repo](https://help.github.com/articles/fork-a-repo/). När din förgrening har skapats kan se du den i webbläsaren.
   
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. Öppna en session för Git-gränssnittet. Om du inte har Git Shell ännu, installera [GitHub för Windows](https://windows.github.com/) nu.
3. Skapa en lokal kloning av din förgrening genom att köra följande kommando hello:

        git clone https://github.com/<your_fork>/ToDoApp.git 
4. När du har din lokala klona navigera för*&lt;repository_root >*\ARMTemplates och kör hello deploy.ps1 skript på följande sätt:
   
        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git
5. När du uppmanas önskat ange hello användarnamn och lösenord för åtkomst till databasen.
   
   Du bör se hello etablering förloppet för olika Azure-resurser. När distributionen är klar startar hello program i webbläsaren hello hello skriptet och ger dig ett eget signal.
   
    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
   
   > [!TIP]
   > Ta en titt på  *&lt;repository_root >*\ARMTemplates\Deploy.ps1, toosee hur den genererar resurser med unika ID: N. Du kan använda samma metod toocreate klonar av hello hello samma distribution utan att oroa motstridiga resursnamn.
   > 
   > 
6. Tillbaka i sessionen Git Shell, kör du:
   
        .\swap –Name ToDoApp<unique_string>master
   
    ![](./media/app-service-agile-software-development/production-4-swap.png)
7. När hello skriptet är klar går du tillbaka toobrowse toohello frontend-adress (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) toosee hello program som körs i produktion.
8. Logga in toohello [Azure-portalen](https://portal.azure.com/) och ta en titt på vad som har skapats.
   
   Du bör vara kan toosee två web apps i hello samma resursgrupp, en med hello `Api` suffix i hello namn. Om du tittar på hello resursgruppvy se du också hello SQL-databas och server, hello App Service-plan och hello fristående fack för hello web apps. Bläddra igenom hello olika resurser och jämför dem med  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json toosee hur de är konfigurerade i hello mallen.
   
    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

Du har nu konfigurerat hello-produktionsmiljö. Därefter kommer startar du en ny uppdatering toohello ansökan.

## <a name="create-dev-and-test-branches"></a>Skapa dev och testa filialer
Nu när du har en komplexa program som körs i produktion i Azure, blir en uppdateringsprogrammet tooyour i enlighet med flexibel metod. I det här avsnittet skapar du hello utveckling och testning filialer som du behöver toomake hello nödvändiga uppdateringar.

1. Skapa hello testmiljö. I sessionen Git Shell hello kör följande kommandon för toocreate hello miljö för en ny gren kallas **NewUpdate**. 
   
        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate
2. När du uppmanas önskat ange hello användarnamn och lösenord för åtkomst till databasen. 
   
   När distributionen är klar startar hello program i webbläsaren hello hello skriptet och ger dig ett eget signal. Nu har du en ny gren med sin egen testmiljö. Ta en stund tooreview några saker om den här testmiljön:
   
   * Du kan skapa den i Azure-prenumeration. Det innebär att hello produktionsmiljön kan hanteras separat från din testmiljö.
   * Din testmiljö körs live i Azure.
   * Testmiljön är identiska toohello produktionsmiljön, förutom hello mellanlagring platser och hello skalning. Du vet om det. eftersom de är hello bara skillnaderna mellan ProdandStage.json och Dev.json.
   * Du kan hantera din testmiljö i sin egen programtjänstplanen till ett annat pris-nivån (exempelvis **lediga**).
   * Ta bort den här testmiljö är lika enkelt som hello resursgruppen. Du lär dig hur toodo detta [senare](#delete).
3. Gå på toocreate en dev gren genom att köra hello följande kommandon:
   
        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev
4. När du uppmanas önskat ange hello användarnamn och lösenord för åtkomst till databasen. 
   
   Ta en stund tooreview några saker om den här utvecklingsmiljö: 
   
   * Din utvecklingsmiljö har en testmiljö för konfiguration identiska toohello eftersom den distribueras med hjälp av hello samma mall.
   * Varje utvecklingsmiljö kan skapas i hello-utvecklare egen Azure-prenumeration, lämnar hello test miljö toobe hanteras separat.
   * Din utvecklingsmiljö körs live i Azure.
   * Ta bort hello dev är miljö så enkelt som hello resursgruppen. Du lär dig hur toodo detta [senare](#delete).

> [!NOTE]
> När du har flera utvecklare som arbetar på hello ny uppdatering skapa var och en av dem enkelt en gren och dedikerad utvecklingsmiljö med hello följande steg:
> 
> 1. Skapa egna förgrening av hello lagringsplatsen i GitHub (se [duplicera en Repo](https://help.github.com/articles/fork-a-repo/)).
> 2. Klona hello förgrening på användarens lokala dator
> 3. Kör hello kommandon samma toocreate sina egna dev gren och miljö.
> 
> 

När du är klar bör din GitHub-förgrening ha tre grenar:

![](./media/app-service-agile-software-development/test-1-github-view.png)

Och du bör ha sex webbappar (tre uppsättningar av två) i tre separata resursgrupper:

![](./media/app-service-agile-software-development/test-2-all-webapps.png)

> [!NOTE]
> ProdandStage.json anger hello produktion miljö toouse hello **Standard** prisnivån som är lämplig för skalbarhet hello produktionsprogram.
> 
> 

## <a name="build-and-test-every-commit"></a>Skapa och testa varje incheckning
Hej mallfilerna ProdAndStage.json och Dev.json ange redan hello parametrar för datakällan, vilket som standard ställer in kontinuerlig publicering för hello webbprogrammet. Därför utlöser varje genomförande toohello GitHub gren ett tooAzure för automatisk distribution av grenen. Nu ska vi se hur inställningarna fungerar nu.

1. Kontrollera att du är i hello Dev grenen av hello lokal databas. toodo detta, kör hello följande kommando i Git-gränssnittet:
   
        git checkout Dev
2. Göra en ändring toohello app UI layer genom att ändra hello kod toouse [Bootstrap](http://getbootstrap.com/components/) visas. Öppna  *&lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml och gör hello följande markerade ändringen:
   
    ![](./media/app-service-agile-software-development/commit-1-changes.png)
   
    > [!NOTE]
    > Om du inte kan läsa hello föregående bild: 
    > 
    > * I rad 18 ändrar `check-list` för`list-group`.
    > * I rad 19 ändrar `class="check-list-item"` för`class="list-group-item"`.
    > 
    > 
3. Spara hello ändringen. Tillbaka Git Shell, kör hello följande kommandon:
   
        cd <repository_root>
        git add .
        git commit -m "changed toobootstrap style"
        git push origin Dev
   
   Dessa git-kommandon är liknande för ”kontrollerar i kod” i en annan källkontrollsystem som TFS. När du kör `git push`, hello nya commit utlöser en automatisk kod push tooAzure, vilken sedan bygger hello programmet tooreflect hello ändring i hello-utvecklingsmiljö.
4. tooverify som den här koden push tooyour utvecklingsmiljö har inträffat, gå tooyour dev miljö webbsida app och titta på hello **distribution** del. Du bör vara kan toosee din senaste genomförande det meddelandet.
   
    ![](./media/app-service-agile-software-development/commit-2-deployed.png)
5. Därifrån klickar du på **Bläddra** toosee hello nya ändringar i hello live program i Azure.
   
    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)
   
   Det är en mindre ändring toohello program. Men många gånger nya ändringar tooa komplexa webbprogram har oönskade och oönskade sidoeffekter. Som kan tooeasily testa kan varje incheckning i live versioner du toocatch problemen innan kunderna ser dem.

Nu bör du vara nöjd med hello utförandet som utvecklare på hello **NewUpdate** -projekt kan du skapa en utvecklingsmiljö för dig själv, och sedan skapa varje incheckning och testa alla build.

## <a name="merge-code-into-test-environment"></a>Sammanfoga koden i testmiljö
När du är klar toopush koden från Dev Förgrena in tooNewUpdate gren är hello standard git processen:

1. Slå samman alla nya incheckningar tooNewUpdate till hello Dev gren i GitHub, till exempel incheckningar som skapats av andra utvecklare. Alla nya commit på GitHub utlöser en kod push- och versionsinformation i hello-utvecklingsmiljö. Du kan sedan kontrollera att din kod i Dev grenen fortfarande fungerar med hello senaste bits från NewUpdate grenen.
2. Slå samman alla nya incheckningar från Dev gren till NewUpdate gren på GitHub. Den här åtgärden utlöser en kod push- och versionsinformation i hello testmiljö. 

Observera igen att eftersom kontinuerlig distribution har redan konfigurerats med dessa git filialer, inte behöver du tootake andra åtgärder som att köra integration versioner. Du behöver tooperform standard källa kontrollen metoder med git och Azure utför alla hello build processer för dig.

Nu ska vi skicka koden för**NewUpdate** grenen. Git Shell, kör hello följande kommandon:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

Klart! 

Gå toohello app webbsida för din miljö test-toosee din nya commit (sammanfogas NewUpdate gren) nu pushas toohello testmiljö. Klicka på **Bläddra** toosee som hello formatändring nu körs live i Azure.

## <a name="deploy-update-tooproduction"></a>Distribuera uppdateringen tooproduction
Skicka koden toohello bör mellanlagring och produktion miljö du inte annorlunda än vad du redan har gjort när du pushas kod toohello testmiljö. Det är mycket enkelt. 

Git Shell, kör hello följande kommandon:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Kom ihåg att baserat på hello sätt hello mellanlagring och produktion miljö har ställts in i ProdandStage.json, din nya kod är nedtryckt toohello **mellanlagring** fack och kör det. Så om du navigerar toohello mellanlagringsplatsen URL finns hello ny kod körs det. toodo detta, kör hello följande cmdlet i Git-gränssnittet.

    Start-Process -FilePath "http://ToDoApp<unique_string>master-Staging.azurewebsites.net"

Och nu när du har kontrollerat hello uppdatering i hello mellanlagringsplatsen hello endast återstår toodo är tooswap den i produktion. Git Shell, kör du bara hello följande kommandon:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Grattis! Du har publicerat ett nytt webbprogram för uppdatering tooyour produktion. Dessutom är som gjorde det enkelt att skapa utvecklings- och testmiljöer och skapa och testa varje incheckning. Detta är avgörande byggblock för flexibel programutveckling.

<a name="delete"></a>

## <a name="delete-dev-and-test-environments"></a>Ta bort utvecklings- och testmiljöer
Eftersom du har ändamålsenligt konstruerad din dev och testa miljöer toobe självständiga resursgrupper är det enkelt toodelete dem. toodelete hello som du skapade i den här självstudiekursen, både hello GitHub filialer och Azure artefakter, kör du bara följande kommandon i Git Shell hello:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Sammanfattning
Flexibel programutveckling är en måste ha för många företag som vill tooadopt Azure som deras plattform. I den här kursen har du lärt dig hur toocreate och tear exakt repliker eller nära repliker av hello produktionsmiljö utan problem, även för komplexa program. Du har också fått lära dig hur tooleverage denna möjlighet toocreate en utveckling bearbeta som kan skapa och testa varje enskild incheckning i Azure. Den här självstudiekursen förhoppningsvis visar du hur du bäst använder Azure App Service och Azure Resource Manager tillsammans toocreate en DevOps-lösning som caters tooagile metoder. Därefter du kan bygga vidare på det här scenariot genom att utföra avancerade DevOps-metoder som [test i produktion](app-service-web-test-in-production-get-start.md). Ett vanligt scenario för testning i produktionsmiljö Se [Flighting distribution (betatestning) i Azure App Service](app-service-web-test-in-production-controlled-test-flight.md).

## <a name="more-resources"></a>Fler resurser
* [Distribuera ett komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md)
* [Flexibel utveckling i praktiken: Tips och trick för moderniserade utvecklingscykeln](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
* [Avancerad distribution för webbprogram i Azure med hjälp av Resource Manager-mallar](http://channel9.msdn.com/Events/Build/2015/2-620)
* [Redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md)
* [JSONLint - hello JSON-verifieraren](http://jsonlint.com/)
* [ARMClient – Ställ in GitHub publishing toosite](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
* [Git förgrening – grundläggande förgrening och sammanfogning](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [David Ebbo blogg](http://blog.davidebbo.com/)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure plattformsoberoende kommandoradsverktyg](../cli-install-nodejs.md)
* [Skapa eller redigera användare i Azure AD](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
* [Projektet Kudu-Wiki](https://github.com/projectkudu/kudu/wiki)

