---
title: aaaCreate, skapa och distribuera logikappar i Visual Studio - Azure Logic Apps | Microsoft Docs
description: "Skapa Visual Studio-projekt så att du kan utforma, skapa och distribuera Azure Logic Apps."
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e484e5ce-77e9-4fa9-bcbe-f851b4eb42a6
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 5154cb05f9a48e9f0f2381a6953947217f7bb114
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a>Utforma, skapa och distribuera Azure Logic Apps i Visual Studio

Även om hello [Azure-portalen](https://portal.azure.com/) erbjuder ett bra sätt du toocreate och hantera Azure Logikappar kan du använda Visual Studio för att utforma, skapa och distribuera dina logic apps. Visual Studio innehåller omfattande verktyg som hello logik App Designer du toocreate logikappar, konfigurera mallar för distribution och automatisering och distribuera tooany miljö. 

Läs tooget igång med Azure Logikappar [hur toocreate logikappen första i hello Azure-portalen](logic-apps-create-a-logic-app.md).

## <a name="installation-steps"></a>Installationssteg

tooinstall och konfigurera Visual Studio tools för Logikappar i Azure, Följ dessa steg.

### <a name="prerequisites"></a>Krav

* [Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) eller Visual Studio 2015
* [Den senaste Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 eller senare)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* Åtkomst toohello web när du använder hello inbäddade designer

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a>Installera Visual Studio tools för Azure Logic Apps

När du har installerat hello krav:

1. Öppna Visual Studio. På hello **verktyg** väljer du **tillägg och uppdateringar**.
2. Expandera hello **Online** kategori så att du kan söka online.
3. Bläddra eller Sök efter **Logikappar** tills du hittar **Azure Logic Apps Tools för Visual Studio**.
4. toodownload och installera hello-tillägg, klickar du på **hämta**.
5. Starta om Visual Studio efter installationen.

> [!NOTE]
> Du kan också hämta [Azure Logic Apps Tools för Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) och hello [Azure Logic Apps verktyg för Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) direkt från hello Visual Studio Marketplace.

När du har slutfört installationen kan du använda hello Azure-resursgruppsprojekt med logik App Designer.

## <a name="create-your-project"></a>Skapa projektet

1. På hello **filen** menyn Gå för**ny**, och välj **projekt**. Eller tooadd ditt projekt tooan befintlig lösning, gå för**Lägg till**, och välj **nytt projekt**.

    ![Arkivmenyn](./media/logic-apps-deploy-from-vs/filemenu.png)

2. I hello **nytt projekt** fönstret hitta **moln**, och välj **Azure-resursgrupp**. Namnge projektet och klicka på **OK**.

    ![Lägg till nytt projekt](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. Välj hello **Logikapp** mall som skapar en tom logik app Distributionsmall för du toouse. När du har valt en mall klickar du på **OK**.

    ![Välj logiska appmallen](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    Du har nu lagt till din logik app-projekt tooyour lösning. 
    Distributionsfilen bör visas i hello Solution Explorer.

    ![Distribution av fil](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a>Skapa din logikapp med logik App Designer

När du har en Azure-resursgrupp projekt som innehåller en logikapp, kan du öppna hello logik App Designer i Visual Studio toocreate arbetsflödet. 

> [!NOTE]
> hello designer kräver en Internetanslutning för fråga kopplingar för tillgängliga egenskaper och data. Till exempel om du använder hello Dynamics CRM Online connector hello designer frågar dina CRM-instansen tooshow tillgängliga anpassade och standardegenskaperna.

1. Högerklicka på din `<template>.json` fil och markera **öppna med logik App Designer**. (`Ctrl+L`)

2. Välj Azure-prenumeration, resursgrupp och plats för mallen för distribution.

    > [!NOTE]
    > Designa en logikapp skapar API-anslutningen resurser frågan för egenskaper under design. Visual Studio använder den valda resursen grupp toocreate anslutningarna vid designtillfället. tooview eller ändra alla API-anslutningar, gå toohello Azure-portalen och bläddra efter **API anslutningar**.

    ![Väljaren för prenumeration](./media/logic-apps-deploy-from-vs/designer_picker.png)

    hello-designer använder hello definition i hello `<template>.json` filen för återgivning.

4. Skapa och utforma din logikapp. Mallen för distribution har uppdaterats med dina ändringar.

    ![Logik App Designer i Visual Studio](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

Visual Studio lägger till `Microsoft.Web/connections` resurser för din resursfilen för alla anslutningar logikappen måste toofunction. Dessa anslutningsegenskaper kan anges när du distribuerar och hanteras när du distribuerar i **API anslutningar** i hello Azure-portalen.

### <a name="switch-toojson-code-view"></a>Växla tooJSON kodvy

tooshow hello JSON-representation för din logikapp, Välj hello **kodvy** fliken längst hello hello designer.

tooswitch tillbaka toohello fullständiga resurs JSON, högerklicka på hello `<template>.json` fil och markera **öppna**.

### <a name="add-references-for-dependent-resources-toovisual-studio-deployment-templates"></a>Lägg till referenser för mallar för distribution av beroende resurser tooVisual Studio

När du vill att dina logic app tooreference beroende resurser, kan du använda [Azure Resource Manager Mallfunktioner](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) i mallen för logik app-distribution. Till exempel kanske ditt logik app tooreference ett Azure-funktion eller integreringspaket konto som du vill toodeploy tillsammans med din logikapp. Följ dessa riktlinjer om hur toouse parametrar i mallen för distribution så som hello logik App Designer återges korrekt. 

Du kan använda logik app parametrar i dessa typer av utlösare och åtgärder:

*   Underordnat arbetsflöde
*   Funktionsapp
*   APIM anrop
*   API-anslutning runtime-URL
*   Sökväg för API-anslutning

Och du kan använda Mallfunktioner, till exempel parametrar, variabler, resourceId, concat osv. Här är exempelvis hur du kan ersätta hello Azure-funktion resurs-ID:

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

Och där du ska använda parametrarna:

```
"MyFunction": {
    "type": "Function",
    "inputs": {
        "body":{},
        "function":{
            "id":"[resourceid('Microsoft.Web/sites/functions','functionApp',parameters('functionName'))]"
        }
    },
    "runAfter":{}
}
```
Ett annat exempel parameterstyra du hello Service Bus skickar meddelandet igen:

```
"Send_message": {
    "type": "ApiConnection",
        "inputs": {
            "host": {
                "connection": {
                    "name": "@parameters('$connections')['servicebus']['connectionId']"
                }
            },
            "method": "post",
            "path": "[concat('/@{encodeURIComponent(''', parameters('queueuname'), ''')}/messages')]",
            "body": {
                "ContentData": "@{base64(triggerBody())}"
            },
            "queries": {
                "systemProperties": "None"
            }
        },
        "runAfter": {}
    }
```
> [!NOTE] 
> host.runtimeUrl är valfritt och kan tas bort från din mall om det finns.
> 


> [!NOTE] 
> För hello logik App Designer toowork när du använder parametrar, måste du ange standardvärden, till exempel:
> 
> ```
> "parameters": {
>     "IntegrationAccount": {
>     "type":"string",
>     "minLength":1,
>     "defaultValue":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Logic/integrationAccounts/<integrationAccountName>"
>     }
> },
> ```

### <a name="save-your-logic-app"></a>Spara din logikapp

toosave logikappen när som helst, gå för**filen** > **spara**. (`Ctrl+S`) 

Om din logikapp har fel när du sparar din app, visas de i hello Visual Studio **utdata** fönster.

## <a name="deploy-your-logic-app-from-visual-studio"></a>Distribuera din logikapp från Visual Studio

Du kan distribuera direkt från Visual Studio i ett par steg när du har konfigurerat din app. 

1. Högerklicka på projektet i Solution Explorer och gå för**distribuera** > **ny distribution...**

    ![Ny distribution](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. När du uppmanas att logga in tooyour Azure-prenumeration. 

3. Nu måste du välja hello information för hello resursgruppen där du vill att toodeploy logikappen. När du är klar väljer du **distribuera**.

    > [!NOTE]
    > Kontrollera att du väljer hello rätt mall och fil med parametrar för hello resursgrupp. Till exempel om du vill toodeploy tooa produktionsmiljön, välja hello parameterfilen för produktion.

    ![Distribuera tooresource grupp](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    hello distributionens status visas i hello **utdata** fönster. 
    Du måste kanske tooselect **Azure etablering** i hello **visa utdata från** lista.

    ![Statusresultat för distribution](./media/logic-apps-deploy-from-vs/output.png)

I framtida hello, kan du redigera din logikapp i källkontrollen och använder Visual Studio toodeploy nya versioner.

> [!NOTE]
> Om du ändrar hello definition i hello Azure-portalen direkt, skrivs dessa ändringar över när du distribuerar från Visual Studio nästa gång. 

## <a name="add-your-logic-app-tooan-existing-resource-group-project"></a>Lägg till logik app tooan befintlig resursgrupp projektet

Om du har en befintlig resursgrupp-projekt kan du lägga till projektet logik app toothat i hello JSON-disposition. Du kan också lägga till en annan logikapp tillsammans med hello-app som du skapade tidigare.

1. Öppna hello `<template>.json` fil.

2. tooopen hello JSON-disposition gå för**visa** > **andra fönster** > **JSON-disposition**.

3. tooadd en resurs toohello mallfil, klickar du på **Lägg till resurs** hello överst i hello JSON-disposition. Eller högerklicka i fönstret JSON-disposition hello **resurser**, och välj **Lägg till ny resurs**.

    ![JSON-disposition](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. I hello **Lägg till resurs** dialogrutan, lokaliserar och markerar **Logikapp**. Namnge din logikapp och välj **Lägg till**.

    ![Lägga till en resurs](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a>Nästa steg

* [Hantera logikappar med Visual Studio Cloud Explorer](logic-apps-manage-from-vs.md)
* [Visa vanliga exempel och scenarier](logic-apps-examples-and-scenarios.md)
* [Lär dig hur tooautomate företag bearbetar med Azure Logikappar](http://channel9.msdn.com/Events/Build/2016/T694)
* [Lär dig hur toointegrate dina system med Azure Logikappar](http://channel9.msdn.com/Events/Build/2016/P462)
