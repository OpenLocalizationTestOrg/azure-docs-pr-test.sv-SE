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
# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a><span data-ttu-id="fef76-103">Utforma, skapa och distribuera Azure Logic Apps i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fef76-103">Design, build, and deploy Azure Logic Apps in Visual Studio</span></span>

<span data-ttu-id="fef76-104">Även om hello [Azure-portalen](https://portal.azure.com/) erbjuder ett bra sätt du toocreate och hantera Azure Logikappar kan du använda Visual Studio för att utforma, skapa och distribuera dina logic apps.</span><span class="sxs-lookup"><span data-stu-id="fef76-104">Although hello [Azure portal](https://portal.azure.com/) offers a great way for you toocreate and manage Azure Logic Apps, you can use Visual Studio for designing, building, and deploying your logic apps.</span></span> <span data-ttu-id="fef76-105">Visual Studio innehåller omfattande verktyg som hello logik App Designer du toocreate logikappar, konfigurera mallar för distribution och automatisering och distribuera tooany miljö.</span><span class="sxs-lookup"><span data-stu-id="fef76-105">Visual Studio provides rich tools like hello Logic App Designer for you toocreate logic apps, configure deployment and automation templates, and deploy tooany environment.</span></span> 

<span data-ttu-id="fef76-106">Läs tooget igång med Azure Logikappar [hur toocreate logikappen första i hello Azure-portalen](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="fef76-106">tooget started with Azure Logic Apps, learn [how toocreate your first logic app in hello Azure portal](logic-apps-create-a-logic-app.md).</span></span>

## <a name="installation-steps"></a><span data-ttu-id="fef76-107">Installationssteg</span><span class="sxs-lookup"><span data-stu-id="fef76-107">Installation steps</span></span>

<span data-ttu-id="fef76-108">tooinstall och konfigurera Visual Studio tools för Logikappar i Azure, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="fef76-108">tooinstall and configure Visual Studio tools for Azure Logic Apps, follow these steps.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="fef76-109">Krav</span><span class="sxs-lookup"><span data-stu-id="fef76-109">Prerequisites</span></span>

* <span data-ttu-id="fef76-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) eller Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="fef76-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) or Visual Studio 2015</span></span>
* <span data-ttu-id="fef76-111">[Den senaste Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 eller senare)</span><span class="sxs-lookup"><span data-stu-id="fef76-111">[Latest Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 or greater)</span></span>
* [<span data-ttu-id="fef76-112">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="fef76-112">Azure PowerShell</span></span>](https://github.com/Azure/azure-powershell#installation)
* <span data-ttu-id="fef76-113">Åtkomst toohello web när du använder hello inbäddade designer</span><span class="sxs-lookup"><span data-stu-id="fef76-113">Access toohello web when using hello embedded designer</span></span>

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a><span data-ttu-id="fef76-114">Installera Visual Studio tools för Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="fef76-114">Install Visual Studio tools for Azure Logic Apps</span></span>

<span data-ttu-id="fef76-115">När du har installerat hello krav:</span><span class="sxs-lookup"><span data-stu-id="fef76-115">After you install hello prerequisites:</span></span>

1. <span data-ttu-id="fef76-116">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fef76-116">Open Visual Studio.</span></span> <span data-ttu-id="fef76-117">På hello **verktyg** väljer du **tillägg och uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="fef76-117">On hello **Tools** menu, select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="fef76-118">Expandera hello **Online** kategori så att du kan söka online.</span><span class="sxs-lookup"><span data-stu-id="fef76-118">Expand hello **Online** category so you can search online.</span></span>
3. <span data-ttu-id="fef76-119">Bläddra eller Sök efter **Logikappar** tills du hittar **Azure Logic Apps Tools för Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="fef76-119">Browse or search for **Logic Apps** until you find **Azure Logic Apps Tools for Visual Studio**.</span></span>
4. <span data-ttu-id="fef76-120">toodownload och installera hello-tillägg, klickar du på **hämta**.</span><span class="sxs-lookup"><span data-stu-id="fef76-120">toodownload and install hello extension, click **Download**.</span></span>
5. <span data-ttu-id="fef76-121">Starta om Visual Studio efter installationen.</span><span class="sxs-lookup"><span data-stu-id="fef76-121">Restart Visual Studio after installation.</span></span>

> [!NOTE]
> <span data-ttu-id="fef76-122">Du kan också hämta [Azure Logic Apps Tools för Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) och hello [Azure Logic Apps verktyg för Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) direkt från hello Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="fef76-122">You can also download [Azure Logic Apps Tools for Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) and hello [Azure Logic Apps Tools for Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) directly from hello Visual Studio Marketplace.</span></span>

<span data-ttu-id="fef76-123">När du har slutfört installationen kan du använda hello Azure-resursgruppsprojekt med logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="fef76-123">After you finish installation, you can use hello Azure Resource Group project with Logic App Designer.</span></span>

## <a name="create-your-project"></a><span data-ttu-id="fef76-124">Skapa projektet</span><span class="sxs-lookup"><span data-stu-id="fef76-124">Create your project</span></span>

1. <span data-ttu-id="fef76-125">På hello **filen** menyn Gå för**ny**, och välj **projekt**.</span><span class="sxs-lookup"><span data-stu-id="fef76-125">On hello **File** menu, go too**New**, and select **Project**.</span></span> <span data-ttu-id="fef76-126">Eller tooadd ditt projekt tooan befintlig lösning, gå för**Lägg till**, och välj **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="fef76-126">Or tooadd your project tooan existing solution, go too**Add**, and select **New Project**.</span></span>

    ![Arkivmenyn](./media/logic-apps-deploy-from-vs/filemenu.png)

2. <span data-ttu-id="fef76-128">I hello **nytt projekt** fönstret hitta **moln**, och välj **Azure-resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="fef76-128">In hello **New Project** window, find **Cloud**, and select **Azure Resource Group**.</span></span> <span data-ttu-id="fef76-129">Namnge projektet och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fef76-129">Name your project, and click **OK**.</span></span>

    ![Lägg till nytt projekt](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. <span data-ttu-id="fef76-131">Välj hello **Logikapp** mall som skapar en tom logik app Distributionsmall för du toouse.</span><span class="sxs-lookup"><span data-stu-id="fef76-131">Select hello **Logic App** template, which creates a blank logic app deployment template for you toouse.</span></span> <span data-ttu-id="fef76-132">När du har valt en mall klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fef76-132">After you select your template, click **OK**.</span></span>

    ![Välj logiska appmallen](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    <span data-ttu-id="fef76-134">Du har nu lagt till din logik app-projekt tooyour lösning.</span><span class="sxs-lookup"><span data-stu-id="fef76-134">You've now added your logic app project tooyour solution.</span></span> 
    <span data-ttu-id="fef76-135">Distributionsfilen bör visas i hello Solution Explorer.</span><span class="sxs-lookup"><span data-stu-id="fef76-135">In hello Solution Explorer, your deployment file should appear.</span></span>

    ![Distribution av fil](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a><span data-ttu-id="fef76-137">Skapa din logikapp med logik App Designer</span><span class="sxs-lookup"><span data-stu-id="fef76-137">Create your logic app with Logic App Designer</span></span>

<span data-ttu-id="fef76-138">När du har en Azure-resursgrupp projekt som innehåller en logikapp, kan du öppna hello logik App Designer i Visual Studio toocreate arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="fef76-138">When you have an Azure Resource Group project that contains a logic app, you can open hello Logic App Designer in Visual Studio toocreate your workflow.</span></span> 

> [!NOTE]
> <span data-ttu-id="fef76-139">hello designer kräver en Internetanslutning för fråga kopplingar för tillgängliga egenskaper och data.</span><span class="sxs-lookup"><span data-stu-id="fef76-139">hello designer requires an internet connection too query connectors for available properties and data.</span></span> <span data-ttu-id="fef76-140">Till exempel om du använder hello Dynamics CRM Online connector hello designer frågar dina CRM-instansen tooshow tillgängliga anpassade och standardegenskaperna.</span><span class="sxs-lookup"><span data-stu-id="fef76-140">For example, if you use hello Dynamics CRM Online connector, hello designer queries your CRM instance tooshow available custom and default properties.</span></span>

1. <span data-ttu-id="fef76-141">Högerklicka på din `<template>.json` fil och markera **öppna med logik App Designer**.</span><span class="sxs-lookup"><span data-stu-id="fef76-141">Right-click your `<template>.json` file, and select **Open with Logic App Designer**.</span></span> <span data-ttu-id="fef76-142">(`Ctrl+L`)</span><span class="sxs-lookup"><span data-stu-id="fef76-142">(`Ctrl+L`)</span></span>

2. <span data-ttu-id="fef76-143">Välj Azure-prenumeration, resursgrupp och plats för mallen för distribution.</span><span class="sxs-lookup"><span data-stu-id="fef76-143">Choose your Azure subscription, resource group, and location for your deployment template.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fef76-144">Designa en logikapp skapar API-anslutningen resurser frågan för egenskaper under design.</span><span class="sxs-lookup"><span data-stu-id="fef76-144">Designing a logic app creates API Connection resources that query for properties during design.</span></span> <span data-ttu-id="fef76-145">Visual Studio använder den valda resursen grupp toocreate anslutningarna vid designtillfället.</span><span class="sxs-lookup"><span data-stu-id="fef76-145">Visual Studio uses your selected resource group toocreate those connections during design time.</span></span> <span data-ttu-id="fef76-146">tooview eller ändra alla API-anslutningar, gå toohello Azure-portalen och bläddra efter **API anslutningar**.</span><span class="sxs-lookup"><span data-stu-id="fef76-146">tooview or change any API Connections, go toohello Azure portal, and browse for **API Connections**.</span></span>

    ![Väljaren för prenumeration](./media/logic-apps-deploy-from-vs/designer_picker.png)

    <span data-ttu-id="fef76-148">hello-designer använder hello definition i hello `<template>.json` filen för återgivning.</span><span class="sxs-lookup"><span data-stu-id="fef76-148">hello designer uses hello definition in hello `<template>.json` file for rendering.</span></span>

4. <span data-ttu-id="fef76-149">Skapa och utforma din logikapp.</span><span class="sxs-lookup"><span data-stu-id="fef76-149">Create and design your logic app.</span></span> <span data-ttu-id="fef76-150">Mallen för distribution har uppdaterats med dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="fef76-150">Your deployment template is updated with your changes.</span></span>

    ![Logik App Designer i Visual Studio](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

<span data-ttu-id="fef76-152">Visual Studio lägger till `Microsoft.Web/connections` resurser för din resursfilen för alla anslutningar logikappen måste toofunction.</span><span class="sxs-lookup"><span data-stu-id="fef76-152">Visual Studio adds `Microsoft.Web/connections` resources too your resource file for any connections your logic app needs toofunction.</span></span> <span data-ttu-id="fef76-153">Dessa anslutningsegenskaper kan anges när du distribuerar och hanteras när du distribuerar i **API anslutningar** i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fef76-153">These connection properties can be set when you deploy, and managed after you deploy in **API Connections** in hello Azure portal.</span></span>

### <a name="switch-toojson-code-view"></a><span data-ttu-id="fef76-154">Växla tooJSON kodvy</span><span class="sxs-lookup"><span data-stu-id="fef76-154">Switch tooJSON code view</span></span>

<span data-ttu-id="fef76-155">tooshow hello JSON-representation för din logikapp, Välj hello **kodvy** fliken längst hello hello designer.</span><span class="sxs-lookup"><span data-stu-id="fef76-155">tooshow hello JSON representation for your logic app, select hello **Code View** tab at hello bottom of hello designer.</span></span>

<span data-ttu-id="fef76-156">tooswitch tillbaka toohello fullständiga resurs JSON, högerklicka på hello `<template>.json` fil och markera **öppna**.</span><span class="sxs-lookup"><span data-stu-id="fef76-156">tooswitch back toohello full resource JSON, right-click hello `<template>.json` file, and select **Open**.</span></span>

### <a name="add-references-for-dependent-resources-toovisual-studio-deployment-templates"></a><span data-ttu-id="fef76-157">Lägg till referenser för mallar för distribution av beroende resurser tooVisual Studio</span><span class="sxs-lookup"><span data-stu-id="fef76-157">Add references for dependent resources tooVisual Studio deployment templates</span></span>

<span data-ttu-id="fef76-158">När du vill att dina logic app tooreference beroende resurser, kan du använda [Azure Resource Manager Mallfunktioner](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) i mallen för logik app-distribution.</span><span class="sxs-lookup"><span data-stu-id="fef76-158">When you want your logic app tooreference dependent resources, you can use [Azure Resource Manager template functions](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) in your logic app deployment template.</span></span> <span data-ttu-id="fef76-159">Till exempel kanske ditt logik app tooreference ett Azure-funktion eller integreringspaket konto som du vill toodeploy tillsammans med din logikapp.</span><span class="sxs-lookup"><span data-stu-id="fef76-159">For example, you might want your logic app tooreference an Azure Function or integration account that you want toodeploy alongside your logic app.</span></span> <span data-ttu-id="fef76-160">Följ dessa riktlinjer om hur toouse parametrar i mallen för distribution så som hello logik App Designer återges korrekt.</span><span class="sxs-lookup"><span data-stu-id="fef76-160">Follow these guidelines about how toouse parameters in your deployment template so that hello Logic App Designer renders correctly.</span></span> 

<span data-ttu-id="fef76-161">Du kan använda logik app parametrar i dessa typer av utlösare och åtgärder:</span><span class="sxs-lookup"><span data-stu-id="fef76-161">You can use logic app parameters in these kinds of triggers and actions:</span></span>

*   <span data-ttu-id="fef76-162">Underordnat arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="fef76-162">Child workflow</span></span>
*   <span data-ttu-id="fef76-163">Funktionsapp</span><span class="sxs-lookup"><span data-stu-id="fef76-163">Function app</span></span>
*   <span data-ttu-id="fef76-164">APIM anrop</span><span class="sxs-lookup"><span data-stu-id="fef76-164">APIM call</span></span>
*   <span data-ttu-id="fef76-165">API-anslutning runtime-URL</span><span class="sxs-lookup"><span data-stu-id="fef76-165">API connection runtime URL</span></span>
*   <span data-ttu-id="fef76-166">Sökväg för API-anslutning</span><span class="sxs-lookup"><span data-stu-id="fef76-166">API connection path</span></span>

<span data-ttu-id="fef76-167">Och du kan använda Mallfunktioner, till exempel parametrar, variabler, resourceId, concat osv. Här är exempelvis hur du kan ersätta hello Azure-funktion resurs-ID:</span><span class="sxs-lookup"><span data-stu-id="fef76-167">And you can use template functions such as parameters, variables, resourceId, concat, etc. For example, here's how you can replace hello Azure Function resource ID:</span></span>

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

<span data-ttu-id="fef76-168">Och där du ska använda parametrarna:</span><span class="sxs-lookup"><span data-stu-id="fef76-168">And where you would use parameters:</span></span>

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
<span data-ttu-id="fef76-169">Ett annat exempel parameterstyra du hello Service Bus skickar meddelandet igen:</span><span class="sxs-lookup"><span data-stu-id="fef76-169">As another example you can parameterize hello Service Bus send message operation:</span></span>

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
> <span data-ttu-id="fef76-170">host.runtimeUrl är valfritt och kan tas bort från din mall om det finns.</span><span class="sxs-lookup"><span data-stu-id="fef76-170">host.runtimeUrl is optional and can be removed from your template if present.</span></span>
> 


> [!NOTE] 
> <span data-ttu-id="fef76-171">För hello logik App Designer toowork när du använder parametrar, måste du ange standardvärden, till exempel:</span><span class="sxs-lookup"><span data-stu-id="fef76-171">For hello Logic App Designer toowork when you use parameters, you must provide default values, for example:</span></span>
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

### <a name="save-your-logic-app"></a><span data-ttu-id="fef76-172">Spara din logikapp</span><span class="sxs-lookup"><span data-stu-id="fef76-172">Save your logic app</span></span>

<span data-ttu-id="fef76-173">toosave logikappen när som helst, gå för**filen** > **spara**.</span><span class="sxs-lookup"><span data-stu-id="fef76-173">toosave your logic app at anytime, go too**File** > **Save**.</span></span> <span data-ttu-id="fef76-174">(`Ctrl+S`)</span><span class="sxs-lookup"><span data-stu-id="fef76-174">(`Ctrl+S`)</span></span> 

<span data-ttu-id="fef76-175">Om din logikapp har fel när du sparar din app, visas de i hello Visual Studio **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="fef76-175">If your logic app has any errors when you save your app, they appear in hello Visual Studio **Outputs** window.</span></span>

## <a name="deploy-your-logic-app-from-visual-studio"></a><span data-ttu-id="fef76-176">Distribuera din logikapp från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fef76-176">Deploy your logic app from Visual Studio</span></span>

<span data-ttu-id="fef76-177">Du kan distribuera direkt från Visual Studio i ett par steg när du har konfigurerat din app.</span><span class="sxs-lookup"><span data-stu-id="fef76-177">After configuring your app, you can deploy directly from Visual Studio in just a couple steps.</span></span> 

1. <span data-ttu-id="fef76-178">Högerklicka på projektet i Solution Explorer och gå för**distribuera** > **ny distribution...**</span><span class="sxs-lookup"><span data-stu-id="fef76-178">In Solution Explorer, right-click your project, and go too**Deploy** > **New Deployment...**</span></span>

    ![Ny distribution](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. <span data-ttu-id="fef76-180">När du uppmanas att logga in tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fef76-180">When you're prompted, sign in tooyour Azure subscription.</span></span> 

3. <span data-ttu-id="fef76-181">Nu måste du välja hello information för hello resursgruppen där du vill att toodeploy logikappen.</span><span class="sxs-lookup"><span data-stu-id="fef76-181">Now you must select hello details for hello resource group where you want toodeploy your logic app.</span></span> <span data-ttu-id="fef76-182">När du är klar väljer du **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="fef76-182">When you're done, select **Deploy**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fef76-183">Kontrollera att du väljer hello rätt mall och fil med parametrar för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="fef76-183">Make sure that you select hello correct template and parameters file for hello resource group.</span></span> <span data-ttu-id="fef76-184">Till exempel om du vill toodeploy tooa produktionsmiljön, välja hello parameterfilen för produktion.</span><span class="sxs-lookup"><span data-stu-id="fef76-184">For example, if you want toodeploy tooa production environment, choose hello production parameters file.</span></span>

    ![Distribuera tooresource grupp](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    <span data-ttu-id="fef76-186">hello distributionens status visas i hello **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="fef76-186">hello deployment status appears in hello **Output** window.</span></span> 
    <span data-ttu-id="fef76-187">Du måste kanske tooselect **Azure etablering** i hello **visa utdata från** lista.</span><span class="sxs-lookup"><span data-stu-id="fef76-187">You might have tooselect **Azure Provisioning** in hello **Show output from** list.</span></span>

    ![Statusresultat för distribution](./media/logic-apps-deploy-from-vs/output.png)

<span data-ttu-id="fef76-189">I framtida hello, kan du redigera din logikapp i källkontrollen och använder Visual Studio toodeploy nya versioner.</span><span class="sxs-lookup"><span data-stu-id="fef76-189">In hello future, you can edit your logic app in source control, and use Visual Studio toodeploy new versions.</span></span>

> [!NOTE]
> <span data-ttu-id="fef76-190">Om du ändrar hello definition i hello Azure-portalen direkt, skrivs dessa ändringar över när du distribuerar från Visual Studio nästa gång.</span><span class="sxs-lookup"><span data-stu-id="fef76-190">If you change hello definition in hello Azure portal directly, those changes are overwritten when you deploy from Visual Studio next time.</span></span> 

## <a name="add-your-logic-app-tooan-existing-resource-group-project"></a><span data-ttu-id="fef76-191">Lägg till logik app tooan befintlig resursgrupp projektet</span><span class="sxs-lookup"><span data-stu-id="fef76-191">Add your logic app tooan existing Resource Group project</span></span>

<span data-ttu-id="fef76-192">Om du har en befintlig resursgrupp-projekt kan du lägga till projektet logik app toothat i hello JSON-disposition.</span><span class="sxs-lookup"><span data-stu-id="fef76-192">If you have an existing Resource Group project, you can add your logic app toothat project in hello JSON Outline window.</span></span> <span data-ttu-id="fef76-193">Du kan också lägga till en annan logikapp tillsammans med hello-app som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="fef76-193">You can also add another logic app alongside hello app you previously created.</span></span>

1. <span data-ttu-id="fef76-194">Öppna hello `<template>.json` fil.</span><span class="sxs-lookup"><span data-stu-id="fef76-194">Open hello `<template>.json` file.</span></span>

2. <span data-ttu-id="fef76-195">tooopen hello JSON-disposition gå för**visa** > **andra fönster** > **JSON-disposition**.</span><span class="sxs-lookup"><span data-stu-id="fef76-195">tooopen hello JSON Outline window, go too**View** > **Other Windows** > **JSON Outline**.</span></span>

3. <span data-ttu-id="fef76-196">tooadd en resurs toohello mallfil, klickar du på **Lägg till resurs** hello överst i hello JSON-disposition.</span><span class="sxs-lookup"><span data-stu-id="fef76-196">tooadd a resource toohello template file, click **Add Resource** at hello top of hello JSON Outline window.</span></span> <span data-ttu-id="fef76-197">Eller högerklicka i fönstret JSON-disposition hello **resurser**, och välj **Lägg till ny resurs**.</span><span class="sxs-lookup"><span data-stu-id="fef76-197">Or in hello JSON Outline window, right-click **resources**, and select **Add New Resource**.</span></span>

    ![JSON-disposition](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. <span data-ttu-id="fef76-199">I hello **Lägg till resurs** dialogrutan, lokaliserar och markerar **Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="fef76-199">In hello **Add Resource** dialog box, find and select **Logic App**.</span></span> <span data-ttu-id="fef76-200">Namnge din logikapp och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="fef76-200">Name your logic app, and choose **Add**.</span></span>

    ![Lägga till en resurs](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a><span data-ttu-id="fef76-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fef76-202">Next Steps</span></span>

* [<span data-ttu-id="fef76-203">Hantera logikappar med Visual Studio Cloud Explorer</span><span class="sxs-lookup"><span data-stu-id="fef76-203">Manage logic apps with Visual Studio Cloud Explorer</span></span>](logic-apps-manage-from-vs.md)
* [<span data-ttu-id="fef76-204">Visa vanliga exempel och scenarier</span><span class="sxs-lookup"><span data-stu-id="fef76-204">View common examples and scenarios</span></span>](logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="fef76-205">Lär dig hur tooautomate företag bearbetar med Azure Logikappar</span><span class="sxs-lookup"><span data-stu-id="fef76-205">Learn how tooautomate business processes with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/T694)
* [<span data-ttu-id="fef76-206">Lär dig hur toointegrate dina system med Azure Logikappar</span><span class="sxs-lookup"><span data-stu-id="fef76-206">Learn how toointegrate your systems with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/P462)
