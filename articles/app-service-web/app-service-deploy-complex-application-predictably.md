---
title: "aaaProvision och distribuera mikrotjänster förutsägbart i Azure"
description: "Lär dig hur toodeploy ett program består av mikrotjänster i Azure App Service som en enhet och förutsägbart med hjälp av JSON resurs mallar och PowerShell-skript."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: bb51e565-e462-4c60-929a-2ff90121f41d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2016
ms.author: cephalin
ms.openlocfilehash: d32c2fc82a8b09e89224ec437e5819b65b2e9e78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Etablera och distribuera mikrotjänster förutsägbart i Azure
Den här kursen visar hur tooprovision och distribuera ett program som består av [mikrotjänster](https://en.wikipedia.org/wiki/Microservices) i [Azure App Service](/services/app-service/) som en enhet och förutsägbart JSON resurs mallar och PowerShell-skript. 

Är avgörande toosuccess när etablering och distribuera hög skala program som består av hög frikopplad mikrotjänster, repeterbarhet och förutsägbarhet. [Azure Apptjänst](/services/app-service/) kan du toocreate mikrotjänster med webbappar, mobilappar, API apps och logikappar. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) aktiverar du toomanage alla hello mikrotjänster som en enhet, tillsammans med resursberoenden, till exempel inställningar för databas- och åtkomstkontroll. Du kan också distribuera sådant program med hjälp av JSON-mallarna och enkla PowerShell-skript. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Vad du ska göra
I hello kursen distribuerar du ett program som innehåller:

* Två webbappar (d.v.s. två mikrotjänster)
* En serverdel SQL-databas
* App-inställningar och anslutningssträngar källkontroll
* Programinsikter, aviseringar, autoskalning inställningar

## <a name="tools-you-will-use"></a>Verktyg som du vill använda
I den här kursen använder hello följande verktyg. Eftersom den inte är omfattande diskussion om verktyg I villig toostick toohello slutpunkt till slutpunkt scenario och bara ge dig en kort introduktion tooeach och var du hittar mer information om den. 

### <a name="azure-resource-manager-templates-json"></a>Azure Resource Manager-mallar (JSON)
Varje gång du skapar en webbapp i Azure App Service, till exempel använder Azure Resource Manager JSON mallen toocreate hello hela gruppen med hello komponenten resurser. En komplex mall från hello [Azure Marketplace](/marketplace) som hello [skalbar WordPress](/marketplace/partners/wordpress/scalablewordpress/) app kan innehålla hello MySQL-databas, storage-konton, hello App Service-plan, hello webbprogrammet själva, Varningsregler, app inställningar, Autoskala inställningar och mer och alla dessa mallar är tillgängliga tooyou via PowerShell. Mer information om hur toodownload och använda mallarna, se [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).

Mer information om hello Azure Resource Manager-mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Azure SDK 2.6 för Visual Studio
hello innehåller senaste SDK förbättringar toohello mallstöd för Resource Manager-i hello JSON-redigerare. Du kan använda den här tooquickly Skapa mall för en resurs från grunden eller öppna en befintlig JSON-mall (t.ex en mall för hämtade galleriet) för redigering, fylla hello parameterfilen och även distribuera hello resursgruppen direkt från en Azure Resursgruppen lösning.

Mer information finns i [2.6 av Azure SDK för Visual Studio](https://azure.microsoft.com/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Azure PowerShell 0.8.0 eller senare
Från och med version 0.8.0 innehåller hello Azure PowerShell-installationen hello Azure Resource Manager-modulen lägga toohello Azure-modulen. Den här nya modulen kan du tooscript hello distributionen av resursgrupper.

Mer information finns i [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure Resource Explorer
Detta [preview verktyget](https://resources.azure.com) kan du tooexplore hello JSON definitioner av alla hello resursgrupper i din prenumeration och hello enskilda resurser. Du kan redigera hello JSON definitioner av en resurs, ta bort en hel hierarki av resurser och skapa nya resurser i hello-verktyget.  hello information tillgänglig i det här verktyget är mycket användbart vid redigering av mallen eftersom den visar vilka egenskaper som du behöver tooset för en viss typ av resurs, hello korrigera värden, osv. Du kan även skapa resursgruppen i hello [Azure Portal](https://portal.azure.com/), granska definitionerna JSON i hello explorer verktyget toohelp du templatize hello resursgruppen.

### <a name="deploy-tooazure-button"></a>TooAzure knappen distribuera
Om du använder GitHub för källkontroll kan du placera en [distribuera tooAzure knappen](https://azure.microsoft.com/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) i din viktigt. MD, vilket gör att en nyckelfärdig distribution UI tooAzure. Medan du kan göra detta för ett enkelt webbprogram, kan du utöka den här tooenable distribuera en hela resursgruppen genom att placera en fil azuredeploy.json i hello Lagringsplatsens rot. Den här JSON-fil som innehåller hello resource group mallen ska användas av hello distribuera tooAzure knappen toocreate hello resursgruppen. Ett exempel finns hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) exempel som ska användas i den här kursen.

## <a name="get-hello-sample-resource-group-template"></a>Hämta hello exempelmall resurs grupp
Nu ska vi gå rätt tooit.

1. Navigera toohello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) Apptjänst exempel.
2. Klicka på readme.md, **distribuera tooAzure**.
3. Du har vidtagit toohello [distribuera till azure](https://deploy.azure.com) plats och vanliga tooinput distribution parametrar. Observera att de flesta av hello fält fylls med hello databasnamn och vissa slumpmässiga strängar för dig. Du kan ändra alla hello fält om du vill, men endast hello saker som du har tooenter administrativa hello SQL Server-inloggning och hello lösenord och klicka sedan på **nästa**.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)
4. Klicka sedan på **distribuera** toostart hello distributionsprocessen. När hello processen körs toocompletion, klickar du på hello http://todoapp*XXXX*. azurewebsites.net länk toobrowse hello distribuerat program. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)
   
   hello Användargränssnittet är lite långsamt när du bläddrar först tooit eftersom hello appar bara startar, men övertyga dig om att det är ett fullt fungerande program.
5. Tillbaka på hello distribuera klickar du på hello **hantera** länka toosee hello nytt program i hello Azure-portalen.
6. I hello **Essentials** listrutan, klickar du på hello resurslänken grupp. Observera också att hello webbprogrammet är redan ansluten toohello GitHub-lagringsplatsen under **externt projekt**. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
7. Observera att det finns redan två webbprogram och en SQL-databasen i resursgruppen hello i hello resursgruppsbladet.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)

Allt som du just har sett i en kort stund ett fullständigt distribuerad två mikrotjänster program med alla hello komponenter, beroenden, inställningar, databas och kontinuerlig publicering ställs in med en automatisk orchestration i Azure Resource Manager. Detta görs genom att två saker:

* hello distribuera tooAzure knappen
* azuredeploy.JSON i hello lagringsplatsen rot

Du kan distribuera den här samma program flera, hundratals eller tusentals gånger och har hello exakt samma konfiguration varje gång. hello repeterbarhet och hello förutsägbarheten för den här metoden kan du toodeploy hög skala program med enkel och förtroende.

## <a name="examine-or-edit-azuredeployjson"></a>Granska (eller redigera) AZUREDEPLOY. JSON
Nu ska vi titta på hur hello GitHub-lagringsplatsen har ställts in. Du kommer att använda hello JSON-redigerare i hello Azure .NET SDK, så om du inte redan har installerat [Azure .NET SDK 2.6](/downloads/), göra det nu.

1. Klona hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) databas med hjälp av din favorit git-verktyget. I hello skärmbilden nedan göra jag detta i hello Team Explorer i Visual Studio 2013.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)
2. Öppna azuredeploy.json i Visual Studio hello Lagringsplatsens rot. Om du inte ser hello JSON-disposition fönstret måste tooinstall Azure .NET SDK.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Jag vill inte pågående toodescribe av hello JSON-format, men hello [mer resurser](#resources) avsnitt finns länkar för hello resurs grupp mallspråk. Här kan bara vi tooshow du hello intressanta funktioner som kan hjälpa dig att komma igång med att göra en egen anpassad mall för distribution av appen.

### <a name="parameters"></a>Parametrar
Ta en titt på hello parametrar avsnittet toosee att de flesta av dessa parametrar är vilka hello **distribuera tooAzure** efterfrågar tooinput knappen. hello plats bakom hello **distribuera tooAzure** knappen fyller hello indata användargränssnitt med hello parametrar som definierats i azuredeploy.json. Dessa parametrar används i hela hello resursdefinitionerna resursnamn, egenskapsvärden, t.ex.

### <a name="resources"></a>Resurser
Du kan se att 4 översta resurser definieras, inklusive en SQL Server-instans, en apptjänstplan och två webbprogram i hello resurser noden. 

#### <a name="app-service-plan"></a>App Service-plan
Låt oss börja med en enkel rotnivå resurs i hello JSON. I hello JSON-disposition, klickar du på hello App Service-plan med namnet **[hostingPlanName]** toohighlight hello motsvarande JSON-kod. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Observera att hello `type` elementet anger hello sträng för en App Service-plan (den anropades en servergrupp med en lång, lång tid sedan) och andra element och egenskaper fylls i med hjälp av hello parametrar som definierats i hello JSON-fil och har inte den här resursen alla kapslade resurser.

> [!NOTE]
> Observera också att hello-värdet för `apiVersion` anger vilken version av hello REST API toouse hello JSON resursdefinitionen med, och det kan påverka hur hello resursen ska formateras i hello Azure `{}`. 
> 
> 

#### <a name="sql-server"></a>SQL Server
Klicka på hello SQL Server-resurs med namnet **SQLServer** i hello JSON-disposition.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)

Obs hello följande om hello markerat JSON-kod:

* hello användning av parametrar säkerställer att hello skapade resurser är namnet och konfigurerats på ett sätt som gör dem överensstämmer med varandra.
* hello SQLServer resurs har två kapslade resurser, var och en har ett annat värde för `type`.
* hello kapslade resurser inuti `“resources”: […]`, där hello databasen och hello brandväggsregler definieras, har en `dependsOn` element som anger hello resurs-ID för hello rotnivå SQLServer resurs. Detta talar om Azure Resource Manager ”innan du skapar den här resursen som andra resurser måste redan finnas; och om den andra resursen har definierats i mallen för hello sedan skapa den första ”.
  
  > [!NOTE]
  > Detaljerad information om hur toouse hello `resourceId()` finns [Azure Resource Manager mallen Functions](../azure-resource-manager/resource-group-template-functions-resource.md#resourceid).
  > 
  > 
* Hej effekten av hello `dependsOn` elementet är att Azure Resource Manager vet vilka resurser kan skapas parallellt och vilka resurser måste vara sekventiellt. 

#### <a name="web-app"></a>Webbapp
Nu ska vi gå vidare toohello faktiska webbprogram själva, vilket är mer komplicerad. Klicka på hello [variables('apiSiteName')] webbapp i hello JSON-disposition toohighlight dess JSON-kod. Lägg märke till att saker får mycket mer intressant. För detta ändamål ska jag berätta om hello funktioner i taget:

##### <a name="root-resource"></a>Rot-resurs
hello webbprogrammet beror på två olika resurser. Det innebär att Azure Resource Manager skapar hello webbprogrammet förrän båda hello App Service-plan och hello SQL Server-instans skapas.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>App-inställningar
hello app-inställningar också definieras som en kapslad resurs.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

I hello `properties` element för `config/appsettings`, du har två appinställningar hello format `“<name>” : “<value>”`.

* `PROJECT`är en [KUDU inställningen](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) som talar om Azure-distribution som projektet toouse i en Visual Studio-lösning med flera projekt. Jag får du lära dig hur källkontrollen har konfigurerats, men eftersom hello ToDoApp kod i ett Visual Studio-lösning med flera projekt kan vi behöver den här inställningen.
* `clientUrl`är helt enkelt en app som anger att hello programkod använder.

##### <a name="connection-strings"></a>Anslutningssträngar
hello anslutningssträngar också definieras som en kapslad resurs.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

I hello `properties` element för `config/connectionstrings`, varje anslutningssträngen har definierats som ett namn: värde-par med hello specifika format för `“<name>” : {“value”: “…”, “type”: “…”}`. För hello `type` elementet möjliga värden är `MySql`, `SQLServer`, `SQLAzure`, och `Custom`.

> [!TIP]
> För en fullständig förteckning över hello anslutningstyper sträng kör följande kommando i Azure PowerShell hello: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
> 
> 

##### <a name="source-control"></a>Källkontrollen
inställningar för hello källkontrollen också definieras som en kapslad resurs. Azure Resource Manager använder den här tooconfigure kontinuerlig resurspublicering (se begränsning på `IsManualIntegration` senare) och även tookick av hello distribution av programkoden automatiskt under hello bearbetningen av hello JSON-fil.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`och `branch` bör vara ganska intuitiva och ska peka toohello Git-lagringsplatsen och hello namnet på hello gren toopublish från. Dessa definieras igen av indataparametrar. 

Notera i hello `dependsOn` element som i tillägg toohello webbresursen app, `sourcecontrols/web` beror också på `config/appsettings` och `config/connectionstrings`. Detta beror på att när `sourcecontrols/web` är konfigurerad, hello Azure distributionsprocessen försöker automatiskt toodeploy, skapa och starta hello programkod. Lägga till den här beroende kan du se till att måste programmet hello därför åtkomstinställningar toohello krävs app och anslutningssträngar innan hello programkoden körs. 

> [!NOTE]
> Observera också att `IsManualIntegration` har angetts för`true`. Den här egenskapen är nödvändiga i den här självstudiekursen eftersom du inte verkligen äger hello GitHub-lagringsplatsen och därmed faktiskt går inte att bevilja behörighet tooAzure tooconfigure kontinuerlig publicering från [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) (d.v.s. push automatisk databas uppdateras tooAzure). Du kan använda standardvärdet för hello `false` för hello angivna lagringsplats endast om du har konfigurerat hello ägarens GitHub autentiseringsuppgifter i hello [Azure-portalen](https://portal.azure.com/) innan. Med andra ord, om du har ställt in källa kontrollen tooGitHub eller BitBucket för en app i hello [Azure Portal](https://portal.azure.com/) tidigare med ditt användar-autentiseringsuppgifter, och sedan Azure kommer ihåg hello autentiseringsuppgifter och använda dem när du distribuerar en app från GitHub eller BitBucket i hello framtida. Men om du inte redan har gjort det, att distribution av hello JSON-mall misslyckas när Azure Resource Manager försöker tooconfigure hello webbprogram inställningar för källkontrollen eftersom den inte kan logga in GitHub eller BitBucket med hello databasens ägare autentiseringsuppgifter.
> 
> 

## <a name="compare-hello-json-template-with-deployed-resource-group"></a>Jämför hello JSON-mall med distribuerade resursgruppen.
Här kan du kan gå igenom alla hello webbprogram blad i hello [Azure Portal](https://portal.azure.com/), men det finns ett annat verktyg som inte är precis som användbart, om mer. Gå toohello [resursutforskaren Azure](https://resources.azure.com) preview verktyg som ger dig en JSON-representation av alla hello resursgrupper i dina prenumerationer som de finns i hello Azure-serverdel. Du kan också se hur hello resursgruppens JSON-hierarki i Azure motsvarar hello hierarki i hello mallfilen som har använt toocreate den.

Till exempel när jag go toohello [resursutforskaren Azure](https://resources.azure.com) verktyg och expandera hello noder i hello explorer, syns hello resursgrupp och hello rotnivå resurser som samlas in under deras respektive resurstyper.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Om du detaljnivån tooa webbprogrammet ska kunna toosee web app configuration information liknande toohello nedan skärmbild:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Återigen hello kapslade resurser bör ha en mycket lik toothose hierarki i JSON-mallfilen och du bör se hello app-inställningar, anslutningssträngar m.m. avspeglas korrekt i hello JSON-fönstret. hello avsaknaden av inställningar här kan tyda på ett problem med JSON-filen och kan hjälpa dig att felsöka en JSON-fil i mallen.

## <a name="deploy-hello-resource-group-template-yourself"></a>Distribuera hello resurs grupp mallen själv
Hej **distribuera tooAzure** knappen är bra, men det gör du toodeploy hello resource group mallen i azuredeploy.json endast om du redan har pushas azuredeploy.json tooGitHub. hello Azure .NET SDK innehåller också hello verktyg för du toodeploy alla JSON-mallfil direkt från din lokala dator. toodo detta, följ hello stegen nedan:

1. I Visual Studio klickar du på **filen** > **ny** > **projekt**.
2. Klicka på **Visual C#** > **moln** > **Azure-resursgrupp**, klicka på **OK**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)
3. I **Välj Azure-mall**väljer **tom mall** och på **OK**.
4. Dra azuredeploy.json till hello **mallen** mapp med det nya projektet.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)
5. Öppna hello kopieras azuredeploy.json i Solution Explorer.
6. För hello dig ut av hello demonstrationen ska vi lägga till vissa standard Application Insights resurser tooour JSON-fil, genom att klicka på **Lägg till resurs**. Om du är intresserad av distribuera hello JSON-fil kan du hoppa över toohello distribuera steg.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)
7. Välj **Programinsikter för Web Apps**, kontrollera att en befintlig App Service-plan och webb-app är markerad och klicka sedan på **Lägg till**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)
   
   Du kommer nu att kan toosee flera nya resurser, beroende på hello resurs så att den inte kan ha beroenden i antingen hello App Service-plan eller hello webbprogrammet. Dessa resurser är inte aktiverade som deras befintliga definitionen och du kommer toochange som.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
8. Klicka på hello JSON-disposition, **appInsights Autoskala** toohighlight dess JSON-kod. Detta är hello skalning inställning för din programtjänstplan.
9. I hello markerade JSON-kod, leta upp hello `location` och `enabled` egenskaper och ange dem som visas nedan.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)
10. Klicka på hello JSON-disposition, **CPUHigh appInsights** toohighlight dess JSON-kod. Detta är en varning.
11. Leta upp hello `location` och `isEnabled` egenskaper och ange dem som visas nedan. Hello samma för hello andra tre aviseringar (lila lökar).
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)
12. Nu är du redo toodeploy. Högerklicka på hello-projektet och välj **distribuera** > **ny distribution**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)
13. Logga in på ditt Azure-konto om du inte redan gjort det..
14. Välj en befintlig resursgrupp i din prenumeration eller skapa ett nytt en, Välj **azuredeploy.json**, och klicka sedan på **redigera parametrar**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)
    
    Du kommer nu att kan tooedit alla hello parametrar som definierats i hello mallfilen i en bra tabell. Parametrar som definierar standardvärden har redan sina standardvärden och parametrar som definierar en lista över tillåtna värden som ska visas som nedrullningsbara listorna.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)
15. Fyll i alla hello tom parametrar och använda hello [GitHub-repo-adress för ToDoApp](https://github.com/azure-appservice-samples/ToDoApp.git) i **repoUrl**. Klicka på **spara**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)
    
    > [!NOTE]
    > Autoskalning är en funktion som erbjuds i **Standard** nivå eller högre och planera nivå aviseringar finns funktioner som erbjuds i **grundläggande** tjänstnivån eller högre, behöver du tooset hello **sku** Parametern för**Standard** eller **Premium** ordning toosee alla din nya App Insights-resurser lysa.
    > 
    > 
16. Klicka på **distribuera**. Om du har valt **spara lösenord**, hello lösenordet sparas i parameterfilen hello **i klartext**. Annars blir du ombedd tooinput hello lösenord under hello distribueringen.

Klart! Nu behöver du bara toogo toohello [Azure Portal](https://portal.azure.com/) och hello [resursutforskaren Azure](https://resources.azure.com) verktyget toosee hello nya aviseringar och Autoskala inställningar läggs tooyour JSON distribuerat program.

Steg i det här avsnittet åstadkommas huvudsakligen hello följande:

1. Förberedda hello mallfil
2. Skapa en parameter filen toogo med hello mallfilen
3. Distribuerade hello mallfilen med hello parameterfilen

hello sista steget görs enkelt genom en PowerShell-cmdlet. toosee vad Visual Studio gjorde när den distribuerats ditt program, öppna Scripts\Deploy-AzureResourceGroup.ps1. Det är mycket kod det, men bara vi toohighlight alla relevanta hello kod som du måste toodeploy hello mallfilen med hello parameterfil.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

Hej senaste cmdlet `New-AzureResourceGroup`, är hello som faktiskt utför hello-åtgärd. Allt detta ska visa att med hjälp av hello av verktygsuppsättning, det är relativt enkelt toodeploy tooyou ditt moln programmet förutsägbart. Varje gång som du kör cmdlet hello på hello samma mall med hello samma parameterfil, ska tooget hello samma resultat.

## <a name="summary"></a>Sammanfattning
DevOps är repeterbarhet och förutsägbarhet nycklar tooany lyckad distribution av en hög skala program består av mikrotjänster. Du har distribuerat en två-mikrotjänster programmet tooAzure som en enskild resursgrupp med hello Azure Resource Manager-mall i den här självstudiekursen. Förhoppningsvis det du har fått hello kunskap du behöver i ordning toostart konvertera ditt program i Azure till en mall och etablera och distribuera den förutsägbart. 

## <a name="next-steps"></a>Nästa steg
Ta reda på hur för[gäller flexibel metoder och kontinuerligt publicera programmet mikrotjänster med enkel](app-service-agile-software-development.md) och avancerade tekniker för programdistribution som [förhandsversionstestning distribution](app-service-web-test-in-production-controlled-test-flight.md) enkelt.

<a name="resources"></a>

## <a name="more-resources"></a>Fler resurser
* [Språk för Azure Resource Manager-mallen](../azure-resource-manager/resource-group-authoring-templates.md)
* [Redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager Mallfunktioner](../azure-resource-manager/resource-group-template-functions.md)
* [Distribuera ett program med Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md)
* [Använda Azure PowerShell med Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)
* [Felsökning av resursgruppen distributioner i Azure](../azure-resource-manager/resource-manager-common-deployment-errors.md)

