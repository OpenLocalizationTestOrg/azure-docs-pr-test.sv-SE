---
title: Skapa, skapa och distribuera logikappar i Visual Studio - Azure Logic Apps | Microsoft Docs
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
ms.openlocfilehash: e7f5cf483d22e4c60dedbe5176ceb0bc8b2b6e66
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a><span data-ttu-id="10414-103">Utforma, skapa och distribuera Azure Logic Apps i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="10414-103">Design, build, and deploy Azure Logic Apps in Visual Studio</span></span>

<span data-ttu-id="10414-104">Även om den [Azure-portalen](https://portal.azure.com/) erbjuder ett bra sätt att skapa och hantera Azure Logic Apps kan du använda Visual Studio för att utforma, skapa och distribuera dina logic apps.</span><span class="sxs-lookup"><span data-stu-id="10414-104">Although the [Azure portal](https://portal.azure.com/) offers a great way for you to create and manage Azure Logic Apps, you can use Visual Studio for designing, building, and deploying your logic apps.</span></span> <span data-ttu-id="10414-105">Visual Studio innehåller omfattande verktyg som logik App Designer att skapa logikappar, konfigurera mallar för distribution och automatisering och distribuera till vilken miljö.</span><span class="sxs-lookup"><span data-stu-id="10414-105">Visual Studio provides rich tools like the Logic App Designer for you to create logic apps, configure deployment and automation templates, and deploy to any environment.</span></span> 

<span data-ttu-id="10414-106">Lär dig att komma igång med Azure Logikappar [hur du skapar din första logikapp i Azure portal](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="10414-106">To get started with Azure Logic Apps, learn [how to create your first logic app in the Azure portal](logic-apps-create-a-logic-app.md).</span></span>

## <a name="installation-steps"></a><span data-ttu-id="10414-107">Installationssteg</span><span class="sxs-lookup"><span data-stu-id="10414-107">Installation steps</span></span>

<span data-ttu-id="10414-108">Följ dessa steg för att installera och konfigurera Visual Studio tools för Logikappar i Azure.</span><span class="sxs-lookup"><span data-stu-id="10414-108">To install and configure Visual Studio tools for Azure Logic Apps, follow these steps.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="10414-109">Krav</span><span class="sxs-lookup"><span data-stu-id="10414-109">Prerequisites</span></span>

* <span data-ttu-id="10414-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) eller Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="10414-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) or Visual Studio 2015</span></span>
* <span data-ttu-id="10414-111">[Den senaste Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 eller senare)</span><span class="sxs-lookup"><span data-stu-id="10414-111">[Latest Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 or greater)</span></span>
* [<span data-ttu-id="10414-112">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="10414-112">Azure PowerShell</span></span>](https://github.com/Azure/azure-powershell#installation)
* <span data-ttu-id="10414-113">Tillgång till Internet när du använder embedded designer</span><span class="sxs-lookup"><span data-stu-id="10414-113">Access to the web when using the embedded designer</span></span>

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a><span data-ttu-id="10414-114">Installera Visual Studio tools för Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="10414-114">Install Visual Studio tools for Azure Logic Apps</span></span>

<span data-ttu-id="10414-115">När du har installerat krav:</span><span class="sxs-lookup"><span data-stu-id="10414-115">After you install the prerequisites:</span></span>

1. <span data-ttu-id="10414-116">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="10414-116">Open Visual Studio.</span></span> <span data-ttu-id="10414-117">På den **verktyg** väljer du **tillägg och uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="10414-117">On the **Tools** menu, select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="10414-118">Expandera den **Online** kategori så att du kan söka online.</span><span class="sxs-lookup"><span data-stu-id="10414-118">Expand the **Online** category so you can search online.</span></span>
3. <span data-ttu-id="10414-119">Bläddra eller Sök efter **Logikappar** tills du hittar **Azure Logic Apps Tools för Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="10414-119">Browse or search for **Logic Apps** until you find **Azure Logic Apps Tools for Visual Studio**.</span></span>
4. <span data-ttu-id="10414-120">Om du vill hämta och installera tillägget genom att klicka på **hämta**.</span><span class="sxs-lookup"><span data-stu-id="10414-120">To download and install the extension, click **Download**.</span></span>
5. <span data-ttu-id="10414-121">Starta om Visual Studio efter installationen.</span><span class="sxs-lookup"><span data-stu-id="10414-121">Restart Visual Studio after installation.</span></span>

> [!NOTE]
> <span data-ttu-id="10414-122">Du kan också hämta [Azure Logic Apps Tools för Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) och [Azure Logic Apps verktyg för Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) direkt från Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="10414-122">You can also download [Azure Logic Apps Tools for Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) and the [Azure Logic Apps Tools for Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) directly from the Visual Studio Marketplace.</span></span>

<span data-ttu-id="10414-123">När du har slutfört installationen kan du kan använda Azure-resursgrupp projektet med logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="10414-123">After you finish installation, you can use the Azure Resource Group project with Logic App Designer.</span></span>

## <a name="create-your-project"></a><span data-ttu-id="10414-124">Skapa projektet</span><span class="sxs-lookup"><span data-stu-id="10414-124">Create your project</span></span>

1. <span data-ttu-id="10414-125">På den **filen** gå till menyn **ny**, och välj **projekt**.</span><span class="sxs-lookup"><span data-stu-id="10414-125">On the **File** menu, go to **New**, and select **Project**.</span></span> <span data-ttu-id="10414-126">Eller Lägg till ditt projekt i en befintlig lösning, gå till **Lägg till**, och välj **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="10414-126">Or to add your project to an existing solution, go to **Add**, and select **New Project**.</span></span>

    ![Arkivmenyn](./media/logic-apps-deploy-from-vs/filemenu.png)

2. <span data-ttu-id="10414-128">I den **nytt projekt** fönstret hitta **moln**, och välj **Azure-resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="10414-128">In the **New Project** window, find **Cloud**, and select **Azure Resource Group**.</span></span> <span data-ttu-id="10414-129">Namnge projektet och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="10414-129">Name your project, and click **OK**.</span></span>

    ![Lägg till nytt projekt](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. <span data-ttu-id="10414-131">Välj den **Logikapp** mall som skapar en tom logik app Distributionsmall som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="10414-131">Select the **Logic App** template, which creates a blank logic app deployment template for you to use.</span></span> <span data-ttu-id="10414-132">När du har valt en mall klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="10414-132">After you select your template, click **OK**.</span></span>

    ![Välj logiska appmallen](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    <span data-ttu-id="10414-134">Du har nu lagt till ditt logik app-projekt i lösningen.</span><span class="sxs-lookup"><span data-stu-id="10414-134">You've now added your logic app project to your solution.</span></span> 
    <span data-ttu-id="10414-135">I Solution Explorer ska distributionsfilen visas.</span><span class="sxs-lookup"><span data-stu-id="10414-135">In the Solution Explorer, your deployment file should appear.</span></span>

    ![Distribution av fil](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a><span data-ttu-id="10414-137">Skapa din logikapp med logik App Designer</span><span class="sxs-lookup"><span data-stu-id="10414-137">Create your logic app with Logic App Designer</span></span>

<span data-ttu-id="10414-138">När du har en Azure-resursgrupp projekt som innehåller en logikapp kan öppna du logik App Designer i Visual Studio för att skapa arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="10414-138">When you have an Azure Resource Group project that contains a logic app, you can open the Logic App Designer in Visual Studio to create your workflow.</span></span> 

> [!NOTE]
> <span data-ttu-id="10414-139">Designern kräver en Internetanslutning till frågan kopplingar för tillgängliga egenskaper och data.</span><span class="sxs-lookup"><span data-stu-id="10414-139">The designer requires an internet connection to query connectors for available properties and data.</span></span> <span data-ttu-id="10414-140">Om du använder Dynamics CRM Online-anslutningen frågar designern din CRM-instans om du vill visa tillgängliga anpassade och standardegenskaperna.</span><span class="sxs-lookup"><span data-stu-id="10414-140">For example, if you use the Dynamics CRM Online connector, the designer queries your CRM instance to show available custom and default properties.</span></span>

1. <span data-ttu-id="10414-141">Högerklicka på din `<template>.json` fil och markera **öppna med logik App Designer**.</span><span class="sxs-lookup"><span data-stu-id="10414-141">Right-click your `<template>.json` file, and select **Open with Logic App Designer**.</span></span> <span data-ttu-id="10414-142">(`Ctrl+L`)</span><span class="sxs-lookup"><span data-stu-id="10414-142">(`Ctrl+L`)</span></span>

2. <span data-ttu-id="10414-143">Välj Azure-prenumeration, resursgrupp och plats för mallen för distribution.</span><span class="sxs-lookup"><span data-stu-id="10414-143">Choose your Azure subscription, resource group, and location for your deployment template.</span></span>

    > [!NOTE]
    > <span data-ttu-id="10414-144">Designa en logikapp skapar API-anslutningen resurser frågan för egenskaper under design.</span><span class="sxs-lookup"><span data-stu-id="10414-144">Designing a logic app creates API Connection resources that query for properties during design.</span></span> <span data-ttu-id="10414-145">Visual Studio använder den valda resursgruppen för att skapa anslutningarna vid designtillfället.</span><span class="sxs-lookup"><span data-stu-id="10414-145">Visual Studio uses your selected resource group to create those connections during design time.</span></span> <span data-ttu-id="10414-146">Om du vill visa eller ändra alla API-anslutningar, gå till Azure-portalen och bläddra efter **API anslutningar**.</span><span class="sxs-lookup"><span data-stu-id="10414-146">To view or change any API Connections, go to the Azure portal, and browse for **API Connections**.</span></span>

    ![Väljaren för prenumeration](./media/logic-apps-deploy-from-vs/designer_picker.png)

    <span data-ttu-id="10414-148">Designern använder definitionen i den `<template>.json` filen för återgivning.</span><span class="sxs-lookup"><span data-stu-id="10414-148">The designer uses the definition in the `<template>.json` file for rendering.</span></span>

4. <span data-ttu-id="10414-149">Skapa och utforma din logikapp.</span><span class="sxs-lookup"><span data-stu-id="10414-149">Create and design your logic app.</span></span> <span data-ttu-id="10414-150">Mallen för distribution har uppdaterats med dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="10414-150">Your deployment template is updated with your changes.</span></span>

    ![Logik App Designer i Visual Studio](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

<span data-ttu-id="10414-152">Visual Studio lägger till `Microsoft.Web/connections` resurser till dina resursfilen för alla anslutningar logikappen behöver för att fungera.</span><span class="sxs-lookup"><span data-stu-id="10414-152">Visual Studio adds `Microsoft.Web/connections` resources to your resource file for any connections your logic app needs to function.</span></span> <span data-ttu-id="10414-153">Dessa anslutningsegenskaper kan anges när du distribuerar och hanteras när du distribuerar i **API anslutningar** i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="10414-153">These connection properties can be set when you deploy, and managed after you deploy in **API Connections** in the Azure portal.</span></span>

### <a name="switch-to-json-code-view"></a><span data-ttu-id="10414-154">Växla till kodvy JSON</span><span class="sxs-lookup"><span data-stu-id="10414-154">Switch to JSON code view</span></span>

<span data-ttu-id="10414-155">Om du vill visa JSON-representation för din logikapp, Välj den **kodvy** fliken längst ned i designern.</span><span class="sxs-lookup"><span data-stu-id="10414-155">To show the JSON representation for your logic app, select the **Code View** tab at the bottom of the designer.</span></span>

<span data-ttu-id="10414-156">Om du vill växla tillbaka till den fullständiga resursen JSON, högerklicka på den `<template>.json` fil och markera **öppna**.</span><span class="sxs-lookup"><span data-stu-id="10414-156">To switch back to the full resource JSON, right-click the `<template>.json` file, and select **Open**.</span></span>

### <a name="add-references-for-dependent-resources-to-visual-studio-deployment-templates"></a><span data-ttu-id="10414-157">Lägg till referenser för beroende resurser till mallar för distribution av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="10414-157">Add references for dependent resources to Visual Studio deployment templates</span></span>

<span data-ttu-id="10414-158">När du vill att din logikapp för att referera till beroende resurser, kan du använda [Azure Resource Manager Mallfunktioner](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) i mallen för logik app-distribution.</span><span class="sxs-lookup"><span data-stu-id="10414-158">When you want your logic app to reference dependent resources, you can use [Azure Resource Manager template functions](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) in your logic app deployment template.</span></span> <span data-ttu-id="10414-159">Exempelvis kanske du vill logikappen att referera till ett Azure-funktion eller integreringspaket konto som du vill distribuera tillsammans med din logikapp.</span><span class="sxs-lookup"><span data-stu-id="10414-159">For example, you might want your logic app to reference an Azure Function or integration account that you want to deploy alongside your logic app.</span></span> <span data-ttu-id="10414-160">Följ dessa riktlinjer om hur du använder parametrar i mallen för distribution så att logik App Designern återger korrekt.</span><span class="sxs-lookup"><span data-stu-id="10414-160">Follow these guidelines about how to use parameters in your deployment template so that the Logic App Designer renders correctly.</span></span> 

<span data-ttu-id="10414-161">Du kan använda logik app parametrar i dessa typer av utlösare och åtgärder:</span><span class="sxs-lookup"><span data-stu-id="10414-161">You can use logic app parameters in these kinds of triggers and actions:</span></span>

*   <span data-ttu-id="10414-162">Underordnat arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="10414-162">Child workflow</span></span>
*   <span data-ttu-id="10414-163">Funktionsapp</span><span class="sxs-lookup"><span data-stu-id="10414-163">Function app</span></span>
*   <span data-ttu-id="10414-164">APIM anrop</span><span class="sxs-lookup"><span data-stu-id="10414-164">APIM call</span></span>
*   <span data-ttu-id="10414-165">API-anslutning runtime-URL</span><span class="sxs-lookup"><span data-stu-id="10414-165">API connection runtime URL</span></span>
*   <span data-ttu-id="10414-166">Sökväg för API-anslutning</span><span class="sxs-lookup"><span data-stu-id="10414-166">API connection path</span></span>

<span data-ttu-id="10414-167">Och du kan använda Mallfunktioner, till exempel parametrar, variabler, resourceId, concat osv. Här är exempelvis hur du kan ersätta resurs-ID för Azure-funktion:</span><span class="sxs-lookup"><span data-stu-id="10414-167">And you can use template functions such as parameters, variables, resourceId, concat, etc. For example, here's how you can replace the Azure Function resource ID:</span></span>

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

<span data-ttu-id="10414-168">Och där du ska använda parametrarna:</span><span class="sxs-lookup"><span data-stu-id="10414-168">And where you would use parameters:</span></span>

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
<span data-ttu-id="10414-169">Ett annat exempel parameterstyra du Service Bus skicka meddelandet igen:</span><span class="sxs-lookup"><span data-stu-id="10414-169">As another example you can parameterize the Service Bus send message operation:</span></span>

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
> <span data-ttu-id="10414-170">host.runtimeUrl är valfritt och kan tas bort från din mall om det finns.</span><span class="sxs-lookup"><span data-stu-id="10414-170">host.runtimeUrl is optional and can be removed from your template if present.</span></span>
> 


> [!NOTE] 
> <span data-ttu-id="10414-171">För logik App Designer ska fungera när du använder parametrar måste du ange standardvärden, till exempel:</span><span class="sxs-lookup"><span data-stu-id="10414-171">For the Logic App Designer to work when you use parameters, you must provide default values, for example:</span></span>
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

### <a name="save-your-logic-app"></a><span data-ttu-id="10414-172">Spara din logikapp</span><span class="sxs-lookup"><span data-stu-id="10414-172">Save your logic app</span></span>

<span data-ttu-id="10414-173">Om du vill spara din logikapp när som helst gå till **filen** > **spara**.</span><span class="sxs-lookup"><span data-stu-id="10414-173">To save your logic app at anytime, go to **File** > **Save**.</span></span> <span data-ttu-id="10414-174">(`Ctrl+S`)</span><span class="sxs-lookup"><span data-stu-id="10414-174">(`Ctrl+S`)</span></span> 

<span data-ttu-id="10414-175">Om din logikapp har fel när du sparar din app, visas de i Visual Studio **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="10414-175">If your logic app has any errors when you save your app, they appear in the Visual Studio **Outputs** window.</span></span>

## <a name="deploy-your-logic-app-from-visual-studio"></a><span data-ttu-id="10414-176">Distribuera din logikapp från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="10414-176">Deploy your logic app from Visual Studio</span></span>

<span data-ttu-id="10414-177">Du kan distribuera direkt från Visual Studio i ett par steg när du har konfigurerat din app.</span><span class="sxs-lookup"><span data-stu-id="10414-177">After configuring your app, you can deploy directly from Visual Studio in just a couple steps.</span></span> 

1. <span data-ttu-id="10414-178">Högerklicka på projektet i Solution Explorer och gå till **distribuera** > **ny distribution...**</span><span class="sxs-lookup"><span data-stu-id="10414-178">In Solution Explorer, right-click your project, and go to **Deploy** > **New Deployment...**</span></span>

    ![Ny distribution](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. <span data-ttu-id="10414-180">När du uppmanas logga in på Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="10414-180">When you're prompted, sign in to your Azure subscription.</span></span> 

3. <span data-ttu-id="10414-181">Nu måste du välja information för resursgruppen där du vill distribuera din logikapp.</span><span class="sxs-lookup"><span data-stu-id="10414-181">Now you must select the details for the resource group where you want to deploy your logic app.</span></span> <span data-ttu-id="10414-182">När du är klar väljer du **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="10414-182">When you're done, select **Deploy**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="10414-183">Kontrollera att du väljer rätt mall och parametrar fil för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="10414-183">Make sure that you select the correct template and parameters file for the resource group.</span></span> <span data-ttu-id="10414-184">Om du vill distribuera till en produktionsmiljö kan du till exempel välja parameterfilen produktion.</span><span class="sxs-lookup"><span data-stu-id="10414-184">For example, if you want to deploy to a production environment, choose the production parameters file.</span></span>

    ![Distribuera till resursgrupp](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    <span data-ttu-id="10414-186">Distributionens status visas i den **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="10414-186">The deployment status appears in the **Output** window.</span></span> 
    <span data-ttu-id="10414-187">Du kan behöva välja **Azure etablering** i den **visa utdata från** lista.</span><span class="sxs-lookup"><span data-stu-id="10414-187">You might have to select **Azure Provisioning** in the **Show output from** list.</span></span>

    ![Statusresultat för distribution](./media/logic-apps-deploy-from-vs/output.png)

<span data-ttu-id="10414-189">I framtiden, kan du redigera din logikapp i källkontrollen och använda Visual Studio för att distribuera nya versioner.</span><span class="sxs-lookup"><span data-stu-id="10414-189">In the future, you can edit your logic app in source control, and use Visual Studio to deploy new versions.</span></span>

> [!NOTE]
> <span data-ttu-id="10414-190">Om du ändrar definitionen i Azure portal direkt, skrivs dessa ändringar över när du distribuerar från Visual Studio nästa gång.</span><span class="sxs-lookup"><span data-stu-id="10414-190">If you change the definition in the Azure portal directly, those changes are overwritten when you deploy from Visual Studio next time.</span></span> 

## <a name="add-your-logic-app-to-an-existing-resource-group-project"></a><span data-ttu-id="10414-191">Lägg till din logikapp till ett befintligt projekt i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="10414-191">Add your logic app to an existing Resource Group project</span></span>

<span data-ttu-id="10414-192">Om du har en befintlig resursgrupp-projekt kan du lägga till din logikapp projektet i fönstret JSON-disposition.</span><span class="sxs-lookup"><span data-stu-id="10414-192">If you have an existing Resource Group project, you can add your logic app to that project in the JSON Outline window.</span></span> <span data-ttu-id="10414-193">Du kan också lägga till en annan logikapp tillsammans med den app som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="10414-193">You can also add another logic app alongside the app you previously created.</span></span>

1. <span data-ttu-id="10414-194">Öppna filen `<template>.json`.</span><span class="sxs-lookup"><span data-stu-id="10414-194">Open the `<template>.json` file.</span></span>

2. <span data-ttu-id="10414-195">Om du vill öppna fönstret JSON-disposition, gå till **visa** > **andra fönster** > **JSON-disposition**.</span><span class="sxs-lookup"><span data-stu-id="10414-195">To open the JSON Outline window, go to **View** > **Other Windows** > **JSON Outline**.</span></span>

3. <span data-ttu-id="10414-196">Klicka på Lägg till en resurs i mallen, **Lägg till resurs** längst upp i fönstret JSON-disposition.</span><span class="sxs-lookup"><span data-stu-id="10414-196">To add a resource to the template file, click **Add Resource** at the top of the JSON Outline window.</span></span> <span data-ttu-id="10414-197">Eller högerklicka i fönstret JSON-disposition **resurser**, och välj **Lägg till ny resurs**.</span><span class="sxs-lookup"><span data-stu-id="10414-197">Or in the JSON Outline window, right-click **resources**, and select **Add New Resource**.</span></span>

    ![JSON-disposition](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. <span data-ttu-id="10414-199">I den **Lägg till resurs** dialogrutan, lokaliserar och markerar **Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="10414-199">In the **Add Resource** dialog box, find and select **Logic App**.</span></span> <span data-ttu-id="10414-200">Namnge din logikapp och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="10414-200">Name your logic app, and choose **Add**.</span></span>

    ![Lägga till en resurs](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a><span data-ttu-id="10414-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="10414-202">Next Steps</span></span>

* [<span data-ttu-id="10414-203">Hantera logikappar med Visual Studio Cloud Explorer</span><span class="sxs-lookup"><span data-stu-id="10414-203">Manage logic apps with Visual Studio Cloud Explorer</span></span>](logic-apps-manage-from-vs.md)
* [<span data-ttu-id="10414-204">Visa vanliga exempel och scenarier</span><span class="sxs-lookup"><span data-stu-id="10414-204">View common examples and scenarios</span></span>](logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="10414-205">Lär dig att automatisera affärsprocesser med Azure Logikappar</span><span class="sxs-lookup"><span data-stu-id="10414-205">Learn how to automate business processes with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/T694)
* [<span data-ttu-id="10414-206">Lär dig hur du integrerar dina system med Azure Logikappar</span><span class="sxs-lookup"><span data-stu-id="10414-206">Learn how to integrate your systems with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/P462)
