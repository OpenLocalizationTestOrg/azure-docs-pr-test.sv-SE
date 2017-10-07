---
title: aaaExport Azure Resource Manager-mall | Microsoft Docs
description: "Använd Azure Resource Manager tooexport en mall från en befintlig resursgrupp."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 5f5ca940-eef8-4125-b6a0-f44ba04ab5ab
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/06/2017
ms.author: tomfitz
ms.openlocfilehash: 94daa4812da2fec705044ca31c8e74e6d59bd53f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Exportera en Azure Resource Manager-mall från befintliga resurser
I den här artikeln får du lära dig hur tooexport Resource Manager-mall från befintliga resurser i din prenumeration. Du kan använda den genererade mallen toogain en bättre förståelse av mallens syntax.

Det finns två sätt tooexport en mall:

* Du kan exportera hello **faktiska mall som används för distribution av**. hello exporterade mallen innehåller alla hello parametrar och variabler exakt som de visas i hello ursprungliga mallen. Den här metoden är användbar när du distribuerade resurser via hello portal och vill toosee hello mallen toocreate resurserna. Mallen är enkel att använda. 
* Du kan exportera en **genereras mall som representerar hello aktuell status för hello resursgruppen**. exporterad mall för hello baseras inte på en mall som du använde för distribution. I stället skapar en mall som är en ögonblicksbild av hello resursgruppen. hello exporterad mall har många hårdkodade värden och förmodligen inte så många parametrar som du vanligtvis definierar. Den här metoden är användbar när du har ändrat hello resursgruppen efter distributionen. Du måste vanligtvis göra ändringar i mallen innan du kan använda den.

Det här avsnittet visar båda metoderna hello-portalen.

## <a name="deploy-resources"></a>Distribuera resurser
Låt oss börja med att distribuera resurser tooAzure som du kan använda för att exportera som en mall. Om du redan har en resursgrupp i din prenumeration som du vill tooexport tooa mall kan du hoppa över det här avsnittet. hello resten av den här artikeln förutsätter att du har distribuerat hello webbapp och SQL database-lösning som visas i det här avsnittet. Om du använder en annan lösning din upplevelse kan vara lite annorlunda, men hello steg tooexport en mall är hello samma. 

1. I hello [Azure-portalen](https://portal.azure.com)väljer **ny**.
   
      ![Välj ny](./media/resource-manager-export-template/new.png)
2. Sök efter **webbapp + SQL** och välj den hello tillgängliga alternativ.
   
      ![Sök efter webbapp och SQL](./media/resource-manager-export-template/webapp-sql.png)

3. Välj **Skapa**.

      ![Välj Skapa](./media/resource-manager-export-template/create.png)

4. Ange hello krävs värden för hello webbapp och SQL-databas. Välj **Skapa**.

      ![Ange webb- och SQL-värde](./media/resource-manager-export-template/provide-web-values.png)

hello distributionen kan ta någon minut. När hello distributionen är klar innehåller din prenumeration hello lösning.

## <a name="view-template-from-deployment-history"></a>Visa en mall från distributionshistoriken
1. Gå toohello resursgruppsbladet för din nya resursgrupp. Observera att hello-bladet visar hello resultatet av hello senaste distributionen. Välj den här länken.
   
      ![blad för resursgrupp](./media/resource-manager-export-template/select-deployment.png)
2. Du ser distributionshistoriken för hello grupp. I ditt fall visar hello bladet förmodligen bara en distribution. Välj den här distributionen.
   
     ![den senaste distributionen](./media/resource-manager-export-template/select-history.png)
3. hello bladet visar en sammanfattning av hello-distribution. Översikt över hello innehåller hello status för hello-distributionen och dess åtgärder och hello-värden som du angav för parametrarna. toosee hello mallen som du använde för hello-distribution, väljer **visa mall**.
   
     ![visa distributionssammanfattning](./media/resource-manager-export-template/view-template.png)
4. Resource Manager hämtar hello följande sju filer åt dig:
   
   1. **Mallen** -hello-mallen som definierar hello infrastrukturen för lösningen. När du skapade hello storage-konto via hello portal Resource Manager används en mall toodeploy det och sparade mallen för framtida bruk.
   2. **Parametrarna** -en parameterfil som du kan använda toopass i värden under distributionen. Den innehåller hello-värden som du angav under hello första distributionen. Du kan ändra dessa värden när du distribuerar hello mallen.
   3. **CLI** -Azure Command-Line-interface (CLI) skriptfilen som du kan använda toodeploy hello mallen.
   3. **CLI 2.0** -Azure Command-Line-interface (CLI) skriptfilen som du kan använda toodeploy hello mallen.
   4. **PowerShell** -en Azure PowerShell-skriptfil som du kan använda toodeploy hello mallen.
   5. **.NET** -A .NET-klass som du kan använda toodeploy hello mallen.
   6. **Ruby** -A Ruby-klass som du kan använda toodeploy hello mallen.
      
      hello filer är tillgängliga via länkar mellan hello-bladet. Som standard visar hello bladet hello mallen.
      
       ![Visa mall](./media/resource-manager-export-template/see-template.png)
      
Den här mallen är hello faktiska mall används toocreate din webbapp och SQL-databas. Observera att den innehåller parametrar som gör att du tooprovide olika värden under distributionen. toolearn mer om hello strukturen i en mall finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).

## <a name="export-hello-template-from-resource-group"></a>Exportera hello-mall från resursgruppen
Om du har manuellt dina resurser har ändrats eller lagts till resurser i flera distributioner, avspeglar hämta en mall från hello distributionshistoriken inte hello hello resursgruppen aktuella tillstånd. Det här avsnittet visar hur tooexport en mall som visar hello hello resursgruppen aktuella tillstånd. 

> [!NOTE]
> Du kan inte exportera en mall för en resursgrupp som har fler än 200 resurser.
> 
> 

1. tooview hello mall för en resursgrupp väljer **automatiseringsskriptet**.
   
      ![Exportera resursgrupp](./media/resource-manager-export-template/select-automation.png)
   
     Resource Manager utvärderar hello resurser i hello resursgrupp och genererar en mall för dessa resurser. Inte alla resurstyper stöder hello mallfunktionen för export. Du kan se ett felmeddelande om att det finns ett problem med hello export. Du lär dig hur toohandle de problem i hello [korrigering export problem](#fix-export-issues) avsnitt.
2. Du ser igen hello sex filer som du kan använda tooredeploy hello lösning. Den här gången hello mallen är dock något annorlunda. Observera att hello genererade mallen innehåller färre parametrar än hello mallen i föregående avsnitt. Många av hello värden (t.ex. platsen och SKU-värden) är också hårdkodat i den här mallen i stället för att acceptera ett parametervärde. Innan den här mallen, kanske du vill tooedit hello mallen toomake bättre användning av parametrar. 
   
3. Du har två olika alternativ för fortsatt toowork med den här mallen. Du kan hämta hello mall och arbeta med den lokalt med en JSON-redigerare. Eller, du kan spara hello mallbibliotek tooyour och arbeta med den hello-portalen.
   
     Om du föredrar att använda en JSON-redigerare som [VS kod](https://code.visualstudio.com/) eller [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), kanske du föredrar hämta hello mall lokalt och använda den editor. toowork lokalt, Välj **hämta**.
   
      ![Ladda ned mall](./media/resource-manager-export-template/download-template.png)
   
     Om du inte har ställt in med en JSON-redigerare kanske du föredrar att redigera mallen hello hello-portalen. hello resten av det här avsnittet förutsätter att du har sparat hello tooyour mallbibliotek i hello-portalen. Dock se hello samma syntax ändrar toohello mallen om du arbetar lokalt med en JSON-redigerare eller hello-portalen. toowork hello-portalen, Välj **lägga till toolibrary**.
   
      ![Lägg till toolibrary](./media/resource-manager-export-template/add-to-library.png)
   
     När du lägger till ett bibliotek av mallar toohello ge hello mallen ett namn och beskrivning. Välj sedan **Spara**.
   
     ![ange mallvärden](./media/resource-manager-export-template/save-library-template.png)
4. tooview en mall som sparats i biblioteket, Välj **fler tjänster**, typen **mallar** toofilter resultat, väljer **mallar**.
   
      ![hitta mallar](./media/resource-manager-export-template/find-templates.png)
5. Välj hello mall med hello-namnet som du har sparat.
   
      ![välj mall](./media/resource-manager-export-template/select-saved-template.png)

## <a name="customize-hello-template"></a>Anpassa hello-mall
hello exporterade mallen fungerar bra om du vill toocreate hello samma web app och SQL-databasen för varje distribution. Resource Manager innehåller dock alternativ som gör att du kan distribuera mallar med mycket bättre flexibilitet. Den här artikeln visar hur tooadd parametrar för hello databasen administratörens namn och lösenord. Du kan använda den här samma metod tooadd mer flexibilitet för andra värden i hello mallen.

1. toocustomize hello mall och välj **redigera**.
   
     ![visa mall](./media/resource-manager-export-template/select-edit.png)
2. Välj hello mall.
   
     ![redigera mall](./media/resource-manager-export-template/select-added-template.png)
3. toobe kan toopass hello värden som du kanske vill toospecify under distributionen som lägger till följande två parametrar toohello hello **parametrar** avsnitt i hello mallen:

   ```json
   "administratorLogin": {
       "type": "String"
   },
   "administratorLoginPassword": {
       "type": "SecureString"
   },
   ```

4. toouse hello nya parametrar, Ersätt hello SQL server-definition i hello **resurser** avsnitt. Observera att parametervärden nu används för **administratorLogin** och **administratorLoginPassword**.

   ```json
   {
       "comments": "Generalized from resource: '/subscriptions/{subscription-id}/resourceGroups/exportsite/providers/Microsoft.Sql/servers/tfserverexport'.",
       "type": "Microsoft.Sql/servers",
       "kind": "v12.0",
       "name": "[parameters('servers_tfserverexport_name')]",
       "apiVersion": "2014-04-01-preview",
       "location": "South Central US",
       "scale": null,
       "properties": {
           "administratorLogin": "[parameters('administratorLogin')]",
           "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
           "version": "12.0"
       },
       "dependsOn": []
   },
   ```

6. Välj **OK** när du är klar redigera hello-mallen.
7. Välj **spara** toosave hello ändringar toohello mall.
   
     ![spara mall](./media/resource-manager-export-template/save-template.png)
8. tooredeploy hello uppdateras mall och välj **distribuera**.
   
     ![distribuera mallen](./media/resource-manager-export-template/redeploy-template.png)
9. Ange parametervärden och välj en resurs toodeploy hello gruppresurser till.


## <a name="fix-export-issues"></a>Åtgärda exportproblem
Inte alla resurstyper stöder hello mallfunktionen för export. tooresolve problemet kan manuellt lägga till hello saknas resurser i mallen igen. hello felmeddelande innehåller hello resurstyper som inte kan exporteras. Leta reda på resurstypen i [mallreferensen](/azure/templates/). Till exempel toomanually lägga till en virtuell nätverksgateway, se [Microsoft.Network/virtualNetworkGateways mallreferensen](/azure/templates/microsoft.network/virtualnetworkgateways).

> [!NOTE]
> Exportproblem uppstår bara när du exporterar från en resursgrupp i stället för från distributionshistoriken. Om distributionen senaste motsvarar hello aktuell status för hello resursgrupp, ska du exportera hello mallen från hello distributionshistoriken i stället för från hello resursgrupp. Exportera endast från en resursgrupp när du har gjort ändringar toohello resursgrupp som inte har definierats i en mall.
> 
> 

## <a name="next-steps"></a>Nästa steg
Du har lärt dig hur tooexport en mall från resurser som du skapade i hello-portalen.

* Du kan distribuera en mall genom [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md) eller [REST API](resource-group-template-deploy-rest.md).
* hur tooexport en mall med PowerShell, se toosee [med hjälp av Azure PowerShell med Azure Resource Manager](powershell-azure-resource-manager.md).
* hur tooexport en mall med Azure CLI, se toosee [Använd hello Azure CLI för Mac, Linux och Windows med Azure Resource Manager](xplat-cli-azure-resource-manager.md).

