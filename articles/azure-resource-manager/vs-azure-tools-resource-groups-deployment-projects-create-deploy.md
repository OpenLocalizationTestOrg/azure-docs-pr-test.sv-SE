---
title: aaaVisual Studio Azure resource grupprojekt | Microsoft Docs
description: "Använda Visual Studio toocreate en Azure resursgruppsprojektet och distribuera hello resurser tooAzure."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 4bd084c8-0842-4a10-8460-080c6a085bec
ms.service: azure-resource-manager
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: tomfitz
ms.openlocfilehash: 672c1e71fb809b3b547f0fad30240d45de1ba923
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Skapa och distribuera Azure-resursgrupper via Visual Studio
Med Visual Studio och hello [Azure SDK](https://azure.microsoft.com/downloads/), du kan skapa ett projekt som distribuerar din infrastruktur och kod tooAzure. Du kan till exempel definiera hello Webbvärden, webbplatsen och databasen för din app och distribuera den infrastrukturen tillsammans med hello kod. Eller så kan du definiera en virtuell dator, ett virtuellt nätverk och ett lagringskonto och distribuera den infrastrukturen tillsammans med ett skript som körs på den virtuella datorn. Hej **Azure-resursgrupp** projekt för distribution aktiverar du toodeploy alla hello behövs resurser i en enda, repeterbara åtgärd. Mer information om hur du distribuerar och hanterar dina resurser finns i [Översikt över Azure Resource Manager](resource-group-overview.md).

Azure-resursgruppsprojekt innehåller Azure Resource Manager JSON-mallar, som definierar hello resurser som du distribuerar tooAzure. toolearn om hello elementen i Resource Manager-mall för hello finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md). Visual Studio kan du tooedit dessa mallar och innehåller verktyg som gör det lättare att arbeta med mallar.

I den här artikeln ska du distribuera en webbapp och SQL Database. Dock är hello steg nästan hello samma för alla typer av resurser. Det är lika lätt att distribuera en virtuell dator och dess relaterade resurser. Visual Studio har många olika startmallar som du kan använda för att distribuera vanliga scenarier.

Den här artikeln visar Visual Studio 2017. Om du använder Visual Studio 2015 Update 2 och Microsoft Azure SDK för .NET 2.9 eller Visual Studio 2013 med Azure SDK 2.9 din upplevelse i stort sett hello samma. Du kan använda versioner av hello Azure SDK från 2.6 eller senare. men skilja din upplevelse av hello användargränssnittet sig från hello gränssnitt som visas i den här artikeln. Vi rekommenderar starkt att du installerar hello senaste versionen av hello [Azure SDK](https://azure.microsoft.com/downloads/) innan du börjar hello stegen. 

## <a name="create-azure-resource-group-project"></a>Skapa ett projekt för en Azure-resursgrupp
I den här proceduren ska du skapa ett projekt för en Azure-resursgrupp med en mall av typen **Webbapp + SQL**.

1. I Visual Studio väljer du **Arkiv**, **Nytt projekt** och väljer sedan **C#** eller **Visual Basic**. Välj sedan **Moln** och projektet **Azure-resursgrupp**.
   
    ![Projekt för molndistribution](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)
2. Välj hello mall som du vill toodeploy tooAzure Resource Manager. Observera att det finns många olika alternativ beroende på hello typ av projekt som du vill toodeploy. För den här artikeln väljer hello **Webbapp + SQL** mall.
   
    ![Välja en mall](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)
   
    hello-mallen som du väljer är bara en startpunkt; Du kan lägga till och ta bort resurser toofulfill ditt scenario.
   
   > [!NOTE]
   > Visual Studio hämtar en lista över tillgängliga mallar online. hello listan kan ändras.
   > 
   > 
   
    Visual Studio skapar ett distributionsprojekt för resursgrupper för hello webbapp och SQL-databas.
3. toosee vad du har skapat, titta på hello nod i hello projekt för distribution.
   
    ![visa noder](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)
   
    Eftersom vi valde hello webbapp + SQL-mall för det här exemplet finns hello följande filer: 
   
   | Filnamn | Beskrivning |
   | --- | --- |
   | Deploy-AzureResourceGroup.ps1 |Ett PowerShell-skript som anropar PowerShell-kommandon toodeploy tooAzure Resource Manager.<br />**Obs** Visual Studio använder den här PowerShell-skriptet toodeploy mallen. Alla ändringar du gör toothis skriptet påverkar distributionen i Visual Studio, så var försiktig. |
   | WebSiteSQLDatabase.json |hello Resource Manager-mall som definierar hello infrastruktur som du vill distribuera tooAzure och hello parametrar som du kan ange under distributionen. Den definierar även hello beroenden mellan hello resurser så Resource Manager distribuerar hello resurser i hello rätt ordning. |
   | WebSiteSQLDatabase.parameters.json |En parameterfil som innehåller värden som krävs av hello mallen. Du anger parametern värden toocustomize varje distribution. |
   
    Alla distributionsprojekt för resursgrupper innehåller dessa grundläggande filer. Andra projekt kan innehålla ytterligare filer toosupport andra funktioner.

## <a name="customize-hello-resource-manager-template"></a>Anpassa hello Resource Manager-mall
Du kan anpassa ett distributionsprojekt genom att ändra hello JSON-mallarna som beskriver hello-resurser som du vill toodeploy. JSON står för JavaScript Object Notation och är ett format för serialiserade data som är lätt toowork med. hello JSON-filerna använder ett schema som du refererar till hello överst i varje fil. Om du vill toounderstand hello schema, kan du hämta och analysera den. hello schemat definierar vilka element är giltiga, hello typer och format för fält, hello möjliga värdena för uppräknade värden och så vidare. toolearn om hello elementen i Resource Manager-mall för hello finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).

Öppna toowork på din mall **WebSiteSQLDatabase.json**.

hello Visual Studio redigeraren innehåller verktyg tooassist du redigera hello Resource Manager-mall. Hej **JSON-disposition** gör det enkelt toosee hello-element har definierats i mallen.

![visa JSON-disposition](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Om du väljer hello element i hello disposition tar toothat tillhör hello mall och visar hello motsvarande JSON.

![navigera i JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Du kan lägga till en resurs genom att antingen välja hello **Lägg till resurs** knappen överst hello hello JSON-disposition eller genom att högerklicka på **resurser** och välja **Lägg till ny resurs**.

![lägga till en resurs](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

I den här självstudiekursen väljer du **Lagringskonto** och ger det ett namn. Ange ett namn som innehåller fler än 11 tecken och endast siffror och små bokstäver.

![lägga till lagringsutrymme](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Observera att inte bara lades hello resurs men även en parameter för hello skriver storage-konto och en variabel för hello namnet på hello storage-konto.

![visa disposition](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

Hej **storageType** är fördefinierad med tillåtna typer och en standardtyp. Du kan lämna dessa värden eller redigera dem för ditt scenario. Om du inte vill att någon toodeploy en **Premium_LRS** storage-konto med den här mallen, ta bort den från hello tillåtna typer. 

```json
"storageType": {
  "type": "string",
  "defaultValue": "Standard_LRS",
  "allowedValues": [
    "Standard_LRS",
    "Standard_ZRS",
    "Standard_GRS",
    "Standard_RAGRS"
  ]
}
```

Visual Studio tillhandahåller även intellisense toohelp du förstår vilka egenskaper som är tillgängliga när du redigerar hello mallen. Till exempel tooedit hello egenskaperna för din App Service-plan navigerar toohello **HostingPlan** resurs, och Lägg till ett värde för hello **egenskaper**. Observera att intellisense visar hello tillgängliga värden och ger en beskrivning av det aktuella värdet.

![visa IntelliSense](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

Du kan ange **numberOfWorkers** too1.

```json
"properties": {
  "name": "[parameters('hostingPlanName')]",
  "numberOfWorkers": 1
}
```

## <a name="deploy-hello-resource-group-project-tooazure"></a>Distribuera hello resursgruppen projekt tooAzure
Du är nu redo toodeploy projektet. När du distribuerar ett projekt för Azure-resursgrupp kan distribuera du den tooan Azure-resursgrupp. hello resursgrupp är en logisk gruppering av resurser som delar en gemensam livscykel.

1. Välj på snabbmenyn för distributionsprojektets nod hello hello **distribuera** > **ny**.
   
    ![Menyalternativet Distribuera, Ny distribution](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)
   
    Hej **distribuera tooResource grupp** dialogrutan visas.
   
    ![Distribuera tooResource Gruppdialogrutan](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)
2. I hello **resursgruppen** listrutan, Välj en befintlig resursgrupp eller skapa en ny. toocreate en resursgrupp, öppna hello **resursgruppen** listrutan och väljer **Skapa nytt**.
   
    ![Distribuera tooResource Gruppdialogrutan](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)
   
    Hej **skapa resursgrupp** dialogrutan visas. Ge gruppen ett namn och en plats och välj hello **skapa** knappen.
   
    ![Dialogrutan Skapa resursgrupp](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
3. Redigera hello parametrar för hello distributionen genom att välja hello **redigera parametrar** knappen.
   
    ![Redigera parametrar](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)
4. Ange värden för hello tom parametrar och välj hello **spara** knappen. hello tom parametrar är **hostingPlanName**, **administratorLogin**, **administratorLoginPassword**, och **databaseName**.
   
    **hostingPlanName** anger ett namn för hello [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) toocreate. 
   
    **administratorLogin** anger hello användarnamn för hello SQL Server-administratören. Använd inte vanliga admin-namn som **sa** eller **admin**. 
   
    Hej **administratorLoginPassword** anger ett lösenord för SQL Server-administratören. Hej **spara lösenord i klartext i parameterfilen hello** alternativet är inte säkert; därför inte väljer det här alternativet. Eftersom hello lösenordet inte sparas som oformaterad text, behöver tooprovide lösenordet igen under distributionen. 
   
    **databaseName** anger ett namn för hello databasen toocreate. 
   
    ![Dialogrutan Redigera parametrar](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
5. Välj hello **distribuera** knappen toodeploy hello projekt tooAzure. En PowerShell-konsolen öppnas utanför hello Visual Studio-instans. Ange hello administratörslösenord för SQL Server i hello PowerShell-konsolen när du tillfrågas. **PowerShell-konsolen kan döljs bakom andra objekt eller minimerat i hello Aktivitetsfältet.** Leta efter den här konsolen och välj den tooprovide hello lösenord.
   
   > [!NOTE]
   > Visual Studio begära tooinstall hello Azure PowerShell-cmdlets. Du behöver hello Azure PowerShell cmdlets toosuccessfully distribuera resursgrupper. Installera dem om du uppmanas att göra det.
   > 
   > 
6. hello distributionen kan ta några minuter. I hello **utdata** windows kan du se hello status hello-distribution. När hello distributionen är klar visar hello sista meddelandet en lyckad distribution med något som liknar:
   
        ... 
        18:00:58 - Successfully deployed template 'websitesqldatabase.json' tooresource group 'DemoSiteGroup'.
7. I en webbläsare, öppna hello [Azure-portalen](https://portal.azure.com/) och logga in tooyour konto. toosee hello resursgrupp, Välj **resursgrupper** och hello resursgruppen som du har distribuerat till.
   
    ![välja grupp](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)
8. Du kan se alla hello distribuerade resurser. Observera att hello namnet på hello lagringskontot är inte exakt vilka du angav när du lägger till den här resursen. hello storage-konto måste vara unika. hello mallen automatiskt lägger till en sträng med tecken toohello namn du angav tooprovide ett unikt namn. 
   
    ![visa resurser](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)
9. Om du gör ändringar och vill tooredeploy projektet, Välj hello befintlig resursgrupp hello snabbmenyn för Azure resursgruppsprojektet. På snabbmenyn hello väljer **distribuera**, och välj sedan hello resursgrupp som du har distribuerat.
   
    ![Azure-resursgrupp som har distribuerats](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Distribuera kod med din infrastruktur
Nu är du har distribuerat hello infrastrukturen för din app, men det finns ingen direkt kod som distribueras med hello-projekt. Den här artikeln visar hur toodeploy en webbapp och SQL Database tabeller under distributionen. Om du distribuerar en virtuell dator i stället för en webbapp vill toorun kod på hello datorn som en del av distributionen. Hej processen för att distribuera koden för en webbapp eller för att konfigurera en virtuell dator är nästan hello samma.

1. Lägg till ett projekt tooyour Visual Studio-lösning. Högerklicka på hello lösning och välj **Lägg till** > **nytt projekt**.
   
    ![lägga till projekt](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)
2. Lägga till en **ASP.NET webbapp**. 
   
    ![lägga till webbapp](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
3. Välj **MVC**.
   
    ![välja MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
4. När Visual Studio skapar webbappen, finns båda projekten i hello-lösning.
   
    ![visa projekt](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)
5. Du måste nu toomake till resursgruppsprojektet är medveten om hello nytt projekt. Gå tillbaka tooyour resursgruppsprojektet (AzureResourceGroup1). Högerklicka på **Referenser** och välj **Lägg till referens**.
   
    ![lägga till referens](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)
6. Välj hello webbappsprojektet som du skapade.
   
    ![lägga till referens](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
   
    Genom att lägga till en referens, länka resursgruppsprojektet för hello web app-projekt toohello och anges automatiskt tre nyckelegenskaper. Du ser dessa egenskaper i hello **egenskaper** fönster för hello referens.
   
      ![se referens](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
   
    hello egenskaper är:
   
   * Hej **ytterligare egenskaper** innehåller hello distributionspaketets mellanlagringsplats som skickas toohello Azure Storage. Observera hello-mappen (ExampleApp) och (package.zip). Du behöver tooknow dessa värden, eftersom du ger dem som parametrar när du distribuerar hello app. 
   * Hej **ta med filsökväg** innehåller hello sökväg där hello paketet skapas. Hej **ta med mål** innehåller hello-kommando som distributionen körs. 
   * Hej standardvärdet **skapa; Paketet** aktiverar hello distribution toobuild och skapa ett webbdistributionspaket (package.zip).  
     
     Du behöver inte en publiceringsprofil som hello distributionen hämtar nödvändig information för hello från hello egenskaper toocreate hello paketet.
7. Gå tillbaka tooWebSiteSQLDatabase.json och lägga till en resurs toohello mall.
   
    ![lägga till en resurs](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)
8. Denna gång väljer du **Webbdistribution för Web Apps**. 
   
    ![lägga till webbdistribution](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
9. Distribuera om din grupp projekt toohello resurs resursgrupp. Den här gången finns det några nya parametrar. Du behöver inte tooprovide värden för **_artifactsLocation** eller **_artifactsLocationSasToken** eftersom Visual Studio genererar automatiskt dessa värden. Du måste dock tooset hello mapp och filnamn toohello sökväg som innehåller distributionspaketet hello (visas som **ExampleAppPackageFolder** och **ExampleAppPackageFileName** i följande bild hello ). Ange hello-värden som du såg tidigare i hello referensegenskaper (**ExampleApp** och **package.zip**).
   
    ![lägga till webbdistribution](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
   
    För hello **artefaktlagringskonto**, Välj hello en distribueras med den här resursgruppen.
10. Efter hello distributionen är klar, Välj ditt webbprogram i hello-portalen. Välj hello URL toobrowse toohello plats.
    
     ![bläddra på webbplats](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)
11. Observera att hello ASP.NET-standardappen har distribuerats korrekt.
    
     ![visa distribuerad app](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Nästa steg
* toolearn om hur du hanterar dina resurser via hello portal finns [Using hello Azure portal toomanage resurserna i Azure](resource-group-portal.md).
* toolearn mer information om mallar finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).

