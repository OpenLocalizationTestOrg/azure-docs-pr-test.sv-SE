---
title: "mallar för distribution av aaaCreate för Logikappar i Azure | Microsoft Docs"
description: "Skapa Azure Resource Manager-mallar för logic apps för hantering av distribution och version"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 85928ec6-d7cb-488e-926e-2e5db89508ee
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 2f09445f10a376a745d6acbba94ca29d5f79fc09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-templates-for-logic-apps-deployment-and-release-management"></a>Skapa mallar för logikappar för hantering av distribution och version

När en logikapp har skapats kan du kanske vill toocreate den som en Azure Resource Manager-mall.
På så sätt kan du enkelt distribuera hello logik app tooany miljö eller resursgrupp som du kanske behöver.
Mer information om Resource Manager-mallar finns i [skapa mallar för Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) och [distribuera resurser med hjälp av Azure Resource Manager-mallar](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Mall för distribution av logik-app

En logikapp har tre huvudkomponenter:

* **Logik app resurs**: innehåller information om till exempel priser plan, plats och hello arbetsflödesdefinitionen.
* **Arbetsflödesdefinitionen**: beskriver din logikapp arbetsflödessteg och hur hello Logic Apps motorn ska köra hello arbetsflöde.
Du kan visa den här definitionen i din logikapp **kodvy** fönster.
I hello logik appresursen du hittar den här definitionen i hello `definition` egenskapen.
* **Anslutningar**: refererar tooseparate resurser som på ett säkert sätt lagra metadata om någon koppling anslutningar, till exempel en anslutningssträng och ett åtkomst-token.
I hello logik appresursen logikappen som refererar till dessa resurser i hello `parameters` avsnitt.

Du kan visa dessa delar av befintliga logikappar med hjälp av ett verktyg som [resursutforskaren Azure](http://resources.azure.com).

toomake en mall för logik app toouse med distributionen av resursgrupper måste du definiera hello resurser och parameterstyra efter behov.
Till exempel om du distribuerar tooa utveckling och test produktionsmiljön, vill du förmodligen toouse annan anslutning strängar tooa SQL-databasen i varje miljö.
Eller så kanske du vill toodeploy inom olika prenumerationer eller resursgrupper.  

## <a name="create-a-logic-app-deployment-template"></a>Skapa en logik app Distributionsmall

hello enklaste sättet toohave en giltig logik app Distributionsmall är toouse den [Visual Studio Tools för Logic Apps](logic-apps-deploy-from-vs.md).
hello Visual Studio tools Generera en giltig Distributionsmall som kan användas på alla prenumerationen eller platsen.

Några andra verktyg kan hjälpa dig när du skapar en logik app Distributionsmall.
Du kan skriva manuellt, det vill säga med hjälp av hello resurser beskrivs redan här toocreate parametrar efter behov.
Ett annat alternativ är toouse en [logik som skapar appen mallen](https://github.com/jeffhollan/LogicAppTemplateCreator) PowerShell-modulen. Den här modulen för öppen källkod utvärderas först hello logikapp och alla anslutningar som använder, och genererar Mallresurser med hello nödvändiga parametrar för distribution.
Till exempel om du har en logikapp som tar emot ett meddelande från en Azure Service Bus-kö och läggs data tooan Azure SQL database hello verktyget bevarar alla hello orchestration logik och parameterizes hello SQL och Service Bus-anslutningssträngar så att de kan ställas in vid distribution.

> [!NOTE]
> Anslutningar måste vara inom hello samma resursgrupp som hello logikapp.
>
>

### <a name="install-hello-logic-app-template-powershell-module"></a>Installera PowerShell-modulen för hello logik app mall
hello enklaste sättet tooinstall hello-modulen är via hello [PowerShell-galleriet](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1), med kommandot hello `Install-Module -Name LogicAppTemplate`.  

Du kan också installera hello PowerShell-modulen manuellt:

1. Hämta hello senaste versionen av hello [logik som skapar appen mallen](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
2. Extrahera hello-mappen för din PowerShell-modul (vanligtvis `%UserProfile%\Documents\WindowsPowerShell\Modules`).

För hello modulen toowork med klient- och prenumeration åtkomst-token, rekommenderar vi att du använder det med hello [ARMClient](https://github.com/projectkudu/ARMClient) kommandoradsverktyget.  Detta [blogginlägget](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) ARMClient beskrivs i detalj.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Generera en logik app mall med hjälp av PowerShell
När PowerShell har installerats kan generera du en mall med hjälp av hello följande kommando:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`är hello Azure prenumerations-ID. Den här raden först hämtar en åtkomst-token via ARMClient, och sedan kommer via toohello PowerShell-skript och skapar sedan hello mallen i en JSON-fil.

## <a name="add-parameters-tooa-logic-app-template"></a>Lägg till parametrar tooa logik app mall
Du kan fortsätta tooadd eller ändra parametrar som du kan behöva när du har skapat mallen logik app. Om din definition innehåller en resurs-ID tooan Azure funktion eller inkapslat arbetsflöde som du planerar toodeploy i en enkel distribution, kan du lägga till fler resurser tooyour mall och parameterstyra ID: N efter behov. hello gäller samma tooany referenser toocustom API: er eller Swagger-slutpunkter du förväntar dig toodeploy med varje resursgrupp.

## <a name="deploy-a-logic-app-template"></a>Distribuera en app logik-mall

Du kan distribuera mallen med hjälp av verktyg som PowerShell, REST-API [Visual Studio Team Services versionshantering](#team-services), och malldistribution via hello Azure-portalen.
Dessutom toostore hello värden för parametrar, rekommenderar vi att du skapar en [parameterfilen](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).
Lär dig hur för[distribuera resurser med Azure Resource Manager-mallar och PowerShell](../azure-resource-manager/resource-group-template-deploy.md) eller [distribuera resurser med Azure Resource Manager-mallar och hello Azure-portalen](../azure-resource-manager/resource-group-template-deploy-portal.md).

### <a name="authorize-oauth-connections"></a>Godkänna OAuth-anslutningar

Efter distributionen fungerar hello logikapp slutpunkt till slutpunkt med giltiga parametrar.
Du måste dock fortfarande auktorisera OAuth anslutningar toogenerate en åtkomst-token.
tooauthorize OAuth-anslutningar, öppna hello logikapp i hello Logic Apps Designer och godkänna dessa anslutningar. Eller för automatisk distribution, kan du använda ett skript tooconsent tooeach OAuth-anslutning.
Det finns ett exempelskript på GitHub under den [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) projekt.

<a name="team-services"></a>
## <a name="visual-studio-team-services-release-management"></a>Visual Studio Team Services versionshantering

Ett vanligt scenario för att distribuera och hantera en miljö är toouse ett verktyg som versionshantering i Visual Studio Team Services med en logik app Distributionsmall. Visual Studio Team Services innehåller en [distribuera Azure-resursgrupp](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) uppgiften att lägga till tooany build eller släppa pipeline. Du behöver toohave en [tjänstens huvudnamn](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) för auktorisering toodeploy, och du kan generera hello versionen definition.

1. Välj i versionshantering, **tom** så att du skapar en tom definition.

    ![Skapa en tom definition][1]

2. Välj alla resurser som du behöver för den här, troligen inklusive hello logik appmallen som skapas manuellt eller som en del av hello Byggprocessen.
3. Lägg till en **Azure Resource distribution** aktivitet.
4. Konfigurera med en [tjänstens huvudnamn](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/), och refererar till hello mallen och mallparametrar filer.
5. Fortsätt toobuild stegen nedan i hello release-processen för alla andra miljö, automatiserat test och godkännare efter behov.

<!-- Image References -->
[1]: ./media/logic-apps-create-deploy-template/emptyreleasedefinition.png
