---
title: "aaaBuild en serverlösa app i Visual Studio | Microsoft Docs"
description: "Kom igång med din första serverlösa app med den här guiden för att skapa, distribuera och hantera hello app i Visual Studio."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 74530eea6060ffe2139f7c9d6daab8a46f808162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-serverless-app-in-visual-studio-with-logic-apps-and-functions"></a>Skapa en serverlösa app i Visual Studio med Logic Apps och funktioner

Tillåt för snabb utveckling och distribution av molnprogram serverlösa verktyg och funktioner i Azure.  Det här dokumentet fokuserar på hur tooget startas i Visual Studio skapar ett program.  En översikt över serverlösa i Azure [kan hittas i den här artikeln](logic-apps-serverless-overview.md).

## <a name="getting-everything-ready"></a>Förbereder allt

Här följer hello kraven toobuild ett program från Visual Studio:

* [Visual Studio 2017](https://www.visualstudio.com/vs/) eller Visual Studio 2015 - Community, Professional och Enterprise
* [Logic Apps Tools för Visual Studio](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551)
* [Den senaste Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 eller senare)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* [Azure Functions grundläggande verktyg](https://www.npmjs.com/package/azure-functions-core-tools) toodebug fungerar lokalt
* Åtkomst toohello web när du använder hello inbäddade Logikapp designer

## <a name="getting-started-with-a-deployment-template"></a>Komma igång med en Distributionsmall

Hantera resurser i Azure är klar inom en resursgrupp.  En resursgrupp är en logisk gruppering av resurser.  Resursgrupper kan distribution och hantering av en samling resurser.  För ett program i Azure våra resursgruppen som innehåller både Azure Logic Apps och Azure Functions.  Vi kan kan toodevelop med hjälp av hello resursgruppen projekt i Visual Studio, hantera och distribuera hello hela programmet som ett enskilt objekt.

### <a name="create-a-resource-group-project-in-visual-studio"></a>Skapa en resursgrupp-projekt i Visual Studio

1. I Visual Studio klickar du på tooadd en **nytt projekt**
1. I hello **moln** kategori, Välj toocreate en Azure **resursgruppen** projekt  
 * Om du inte ser hello kategori eller projekt i listan, bör du se till hello Azure SDK för Visual Studio
1. Ge projektet hello namn och plats och välj **Ok** toocreate Visual Studio uppmanar tooselect en mall.  Du kan välja toostart från tom, starta med en logik App eller andra resurser.  Men använder i det här fallet vi en mall för Azure Quickstart tooget oss igång med en serverlösa app.
1. Välj tooshow mallar från **Azure Quickstart** ![att välja Azure Quickstart mallar][1]
1. Välj hello serverlösa quickstart mall: **101-logic-app-and-function-app** och på **Ok**

hello skapas quickstart en Distributionsmall i resursgruppsprojektet.  hello mallen innehåller en enkel Logikapp som anropar en Azure-funktioner och returnerar hello resultat.  Om du öppnar hello `azuredeploy.json` filen i Hej Solution Explorer, du kan se hello resurser för hello serverlösa app.

## <a name="deploying-hello-serverless-application"></a>Distribuera program utan hello

Innan du kan öppna hello Logikapp visuell designer i Visual Studio, måste det toobe en redan distribuerad Azure-resursgrupp.  Detta tillåter hello designer toocreate och Använd anslutningar tooresources och tjänster i hello logikapp.  tooget igång, vi behöver toodeploy hello lösning som har skapats.

1. Högerklicka på hello-projekt i Visual Studio, markera **distribuera**, och skapa en **ny** distribution ![välja ny resurs-distribution][2]
1. Välj en giltig Azure-prenumeration och resursgrupp
1. Välj för**distribuera** hello lösning
1. Ange i hello namn för hello Logikapp och hello Azure Funktionsapp.  hello Azure funktionsnamn behöver toobe globalt unik

Serverlösa hello-lösningen distribueras till hello angivna resursgruppen.  Om du tittar på hello **utdata** i Visual Studio kan du se hello status hello-distribution.

## <a name="editing-hello-logic-app-in-visual-studio"></a>Redigera hello logikapp i Visual Studio

När hello lösningen har distribuerats i valfri resursgrupp, kan hello visuella designern använda tooedit och göra ändringar toohello logikapp.

1. Högerklicka på hello `azuredeploy.json` filen i hello Solution Explorer och markera **öppna med Logic Apps Designer**
1. Välj hello **resursgruppen** och **plats** hello lösningen har distribuerats väljer du tooand **OK**

Hej Logikapp visuella designern ska nu visas med Visual Studio.  Du kan fortsätta tooadd steg, ändra hello arbetsflöde och spara ändringarna.  Du kan också skapa logikappar från Visual Studio.  Om du högerklickar på hello **resurser** i hello mallen navigator, kan du välja tooadd en **Logikapp** toohello projekt.  Tom logikappar läsa in hello visuella designern utan en fördistribuera i en resursgrupp.

### <a name="managing-and-viewing-run-history-for-a-deployed-logic-app"></a>Hantera och visa körningshistorik för en distribuerad logikapp

Du kan också hantera och visa hello kör historiken för logic apps i Azure.  Om du öppnar hello **Cloud Explorer** verktyg i Visual Studio kan du högerklicka på alla Logikapp och väljer tooedit, inaktivera, visa egenskaper, eller visa kör historik.  Klicka på Redigera kan du också toodownload en publicerade logikapp till en resursgrupp för Visual Studio-projekt.  Detta innebär att även om du startade skapa din logikapp i hello Azure-portalen, du kan fortfarande importera och hantera den från Visual Studio.

## <a name="developing-an-azure-function-in-visual-studio"></a>Utveckla en Azure-funktion i Visual Studio

hello Distributionsmall distribuerar de Azure-funktioner som ingår i hello lösning för hello git-lagringsplats som angetts i hello `azuredeploy.json` variabler.  Om du skapar ett projekt för funktionen i hello lösning checkar in i källkontrollen (GitHub, Visual Studio Team Services osv.) och uppdatera hello `repo` variabel, hello mallen distribuerar hello Azure-funktion.

### <a name="creating-an-azure-function-project"></a>Skapa ett projekt för Azure-funktion

Om du använder JavaScript, Python, F #, Bash, Batch eller PowerShell, följ hello [stegen i hello funktioner CLI](../azure-functions/functions-run-local.md) toocreate ett projekt.  Om du utvecklar en funktion i C#, kan du använda en [C#-klassbiblioteket](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/) i hello aktuella lösning för hello Azure-funktion.

## <a name="next-steps"></a>Nästa steg

* [Lär dig hur toobuild en serverlösa sociala instrumentpanel](logic-apps-scenario-social-serverless.md)
* [Hantera en logikapp från Visual Studio Cloud Explorer](logic-apps-manage-from-vs.md)
* [Språk i Arbetsflödesdefinitionen för logik App](logic-apps-workflow-definition-language.md)

<!-- Image references -->
[1]: ./media/logic-apps-serverless-get-started-vs/select-template.png
[2]: ./media/logic-apps-serverless-get-started-vs/deploy.png
