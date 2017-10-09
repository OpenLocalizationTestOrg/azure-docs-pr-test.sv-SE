---
title: "aaaCall REST-slutpunkter med HTTP + Swagger-koppling för Logikappar i Azure | Microsoft Docs"
description: "Ansluta tooREST slutpunkter från logikappar via Swagger med hello HTTP + Swagger-koppling"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: eccfd87c-c5fe-4cf7-b564-9752775fd667
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: baaa57689ff41fcd052f9d86086e36619ddec46e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-http--swagger-action"></a><span data-ttu-id="8a49c-103">Kom igång med hello HTTP + Swagger-åtgärd</span><span class="sxs-lookup"><span data-stu-id="8a49c-103">Get started with hello HTTP + Swagger action</span></span>

<span data-ttu-id="8a49c-104">Du kan skapa en förstklassig connector tooany REST-slutpunkt via en [Swagger-dokument](https://swagger.io) när du använder hello HTTP + Swagger-åtgärd i logik app arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="8a49c-104">You can create a first-class connector tooany REST endpoint through a [Swagger document](https://swagger.io) when you use hello HTTP + Swagger action in your logic app workflow.</span></span> <span data-ttu-id="8a49c-105">Du kan också utöka logic apps toocall valfri REST-slutpunkt förstklassigt logik App Designer upplevelse.</span><span class="sxs-lookup"><span data-stu-id="8a49c-105">You can also extend logic apps toocall any REST endpoint with a first-class Logic App Designer experience.</span></span>

<span data-ttu-id="8a49c-106">hur toocreate logikappar med kopplingar, se toolearn [skapa en ny logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="8a49c-106">toolearn how toocreate logic apps with connectors, see [Create a new logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a><span data-ttu-id="8a49c-107">Använd HTTP + Swagger som en utlösare eller en åtgärd</span><span class="sxs-lookup"><span data-stu-id="8a49c-107">Use HTTP + Swagger as a trigger or an action</span></span>

<span data-ttu-id="8a49c-108">hello HTTP + Swagger-trigger och action arbete hello samma som hello [HTTP-åtgärd](connectors-native-http.md) men ge en bättre upplevelse i logik App Designer genom att exponera hello API struktur och utdata från hello [Swagger-metadata](https://swagger.io) .</span><span class="sxs-lookup"><span data-stu-id="8a49c-108">hello HTTP + Swagger trigger and action work hello same as hello [HTTP action](connectors-native-http.md) but provide a better experience in Logic App Designer by exposing hello API structure and outputs from hello [Swagger metadata](https://swagger.io).</span></span> <span data-ttu-id="8a49c-109">Du kan också använda hello HTTP + Swagger-koppling som en utlösare.</span><span class="sxs-lookup"><span data-stu-id="8a49c-109">You can also use hello HTTP + Swagger connector as a trigger.</span></span> <span data-ttu-id="8a49c-110">Om du vill tooimplement en avsökning utlösare följer hello avsökning mönster som beskrivs i [skapa anpassade API: er toocall andra API: er, tjänster och system från logikappar](../logic-apps/logic-apps-create-api-app.md#polling-triggers).</span><span class="sxs-lookup"><span data-stu-id="8a49c-110">If you want tooimplement a polling trigger, follow hello polling pattern that's described in [Create custom APIs toocall other APIs, services, and systems from logic apps](../logic-apps/logic-apps-create-api-app.md#polling-triggers).</span></span>

<span data-ttu-id="8a49c-111">Lär dig mer om [logik app utlösare och åtgärder](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8a49c-111">Learn more about [logic app triggers and actions](connectors-overview.md).</span></span>

<span data-ttu-id="8a49c-112">Här är ett exempel på hur toouse hello HTTP + Swagger igen som en åtgärd i ett arbetsflöde i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="8a49c-112">Here's an example of how toouse hello HTTP + Swagger operation as an action in a workflow in a logic app.</span></span>

1. <span data-ttu-id="8a49c-113">Välj hello **nytt steg** knappen.</span><span class="sxs-lookup"><span data-stu-id="8a49c-113">Select hello **New Step** button.</span></span>
2. <span data-ttu-id="8a49c-114">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="8a49c-114">Select **Add an action**.</span></span>
3. <span data-ttu-id="8a49c-115">Skriv i sökrutan för hello åtgärd **swagger** toolist hello HTTP + Swagger-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="8a49c-115">In hello action search box, type **swagger** toolist hello HTTP + Swagger action.</span></span>
   
    ![Välj HTTP + Swagger åtgärd](./media/connectors-native-http-swagger/using-action-1.png)
4. <span data-ttu-id="8a49c-117">Ange hello URL för ett Swagger-dokument:</span><span class="sxs-lookup"><span data-stu-id="8a49c-117">Type hello URL for a Swagger document:</span></span>
   
   * <span data-ttu-id="8a49c-118">toowork från hello logik App Designer hello URL måste vara en HTTPS-slutpunkt och har CORS aktiverat.</span><span class="sxs-lookup"><span data-stu-id="8a49c-118">toowork from hello Logic App Designer, hello URL must be an HTTPS endpoint and have CORS enabled.</span></span>
   * <span data-ttu-id="8a49c-119">Om hello Swagger-dokument inte uppfylla detta krav kan du använda [Azure Storage med CORS aktiverat](#hosting-swagger-from-storage) toostore hello dokumentet.</span><span class="sxs-lookup"><span data-stu-id="8a49c-119">If hello Swagger document doesn't meet this requirement, you can use [Azure Storage with CORS enabled](#hosting-swagger-from-storage) toostore hello document.</span></span>
5. <span data-ttu-id="8a49c-120">Klicka på **nästa** tooread och visa från hello Swagger-dokument.</span><span class="sxs-lookup"><span data-stu-id="8a49c-120">Click **Next** tooread and render from hello Swagger document.</span></span>
6. <span data-ttu-id="8a49c-121">Lägga till i alla parametrar som krävs för hello HTTP-anrop.</span><span class="sxs-lookup"><span data-stu-id="8a49c-121">Add in any parameters that are required for hello HTTP call.</span></span>
   
    ![Slutföra HTTP-åtgärden](./media/connectors-native-http-swagger/using-action-2.png)
7. <span data-ttu-id="8a49c-123">toosave och publicera logikappen klickar du på **spara** designer i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="8a49c-123">toosave and publish your logic app, click **Save** on designer toolbar.</span></span>

### <a name="host-swagger-from-azure-storage"></a><span data-ttu-id="8a49c-124">Värden Swagger från Azure Storage</span><span class="sxs-lookup"><span data-stu-id="8a49c-124">Host Swagger from Azure Storage</span></span>
<span data-ttu-id="8a49c-125">Du kanske vill tooreference Swagger-dokument som inte finns eller som inte uppfyller kraven för hello säkerhet och cross-origin för hello designer.</span><span class="sxs-lookup"><span data-stu-id="8a49c-125">You might want tooreference a Swagger document that's not hosted, or that doesn't meet hello security and cross-origin requirements for hello designer.</span></span> <span data-ttu-id="8a49c-126">tooresolve det här problemet kan du lagra hello Swagger-dokument i Azure Storage och aktivera CORS tooreference hello dokumentet.</span><span class="sxs-lookup"><span data-stu-id="8a49c-126">tooresolve this issue, you can store hello Swagger document in Azure Storage and enable CORS tooreference hello document.</span></span>  

<span data-ttu-id="8a49c-127">Här följer hello steg toocreate, konfigurera och lagra Swagger-dokument i Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="8a49c-127">Here are hello steps toocreate, configure, and store Swagger documents in Azure Storage:</span></span>

1. <span data-ttu-id="8a49c-128">[Skapa ett Azure storage-konto med Azure Blob storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="8a49c-128">[Create an Azure storage account with Azure Blob storage](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="8a49c-129">tooperform detta steg kan ange behörigheter för**offentlig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="8a49c-129">tooperform this step, set permissions too**Public Access**.</span></span>

2. <span data-ttu-id="8a49c-130">Aktivera CORS för hello-blob.</span><span class="sxs-lookup"><span data-stu-id="8a49c-130">Enable CORS on hello blob.</span></span> 

   <span data-ttu-id="8a49c-131">tooautomatically konfigurerar den här inställningen kan du använda [detta PowerShell-skript](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).</span><span class="sxs-lookup"><span data-stu-id="8a49c-131">tooautomatically configure this setting, you can use [this PowerShell script](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).</span></span>

3. <span data-ttu-id="8a49c-132">Överför hello Swagger-filen toohello blob.</span><span class="sxs-lookup"><span data-stu-id="8a49c-132">Upload hello Swagger file toohello blob.</span></span> 

   <span data-ttu-id="8a49c-133">Du kan utföra det här steget från hello [Azure-portalen](https://portal.azure.com) eller från ett verktyg som [Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="8a49c-133">You can perform this step from hello [Azure portal](https://portal.azure.com) or from a tool like [Azure Storage Explorer](http://storageexplorer.com/).</span></span>

4. <span data-ttu-id="8a49c-134">Referera till ett HTTPS länken toohello dokument i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="8a49c-134">Reference an HTTPS link toohello document in Azure Blob storage.</span></span> 

   <span data-ttu-id="8a49c-135">hello länk använder det här formatet:</span><span class="sxs-lookup"><span data-stu-id="8a49c-135">hello link uses this format:</span></span>

   `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`

## <a name="technical-details"></a><span data-ttu-id="8a49c-136">Teknisk information</span><span class="sxs-lookup"><span data-stu-id="8a49c-136">Technical details</span></span>
<span data-ttu-id="8a49c-137">Följande är hello information för hello utlösare och åtgärder som den här HTTP + Swagger anslutningen stöder.</span><span class="sxs-lookup"><span data-stu-id="8a49c-137">Following are hello details for hello triggers and actions that this HTTP + Swagger connector supports.</span></span>

## <a name="http--swagger-triggers"></a><span data-ttu-id="8a49c-138">HTTP- + Swagger-utlösare</span><span class="sxs-lookup"><span data-stu-id="8a49c-138">HTTP + Swagger triggers</span></span>
<span data-ttu-id="8a49c-139">En utlösare är en händelse som kan använda toostart hello arbetsflöde som har definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="8a49c-139">A trigger is an event that can be used toostart hello workflow that's defined in a logic app.</span></span> [<span data-ttu-id="8a49c-140">Mer information om utlösare.</span><span class="sxs-lookup"><span data-stu-id="8a49c-140">Learn more about triggers.</span></span>](connectors-overview.md) <span data-ttu-id="8a49c-141">Hej HTTP + Swagger koppling har en utlösare.</span><span class="sxs-lookup"><span data-stu-id="8a49c-141">hello HTTP + Swagger connector has one trigger.</span></span>

| <span data-ttu-id="8a49c-142">Utlösare</span><span class="sxs-lookup"><span data-stu-id="8a49c-142">Trigger</span></span> | <span data-ttu-id="8a49c-143">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8a49c-143">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8a49c-144">HTTP- + Swagger</span><span class="sxs-lookup"><span data-stu-id="8a49c-144">HTTP + Swagger</span></span> |<span data-ttu-id="8a49c-145">Gör ett HTTP-anrop och returnerar hello svar innehåll</span><span class="sxs-lookup"><span data-stu-id="8a49c-145">Make an HTTP call and return hello response content</span></span> |

## <a name="http--swagger-actions"></a><span data-ttu-id="8a49c-146">HTTP- + Swagger-åtgärder</span><span class="sxs-lookup"><span data-stu-id="8a49c-146">HTTP + Swagger actions</span></span>
<span data-ttu-id="8a49c-147">En åtgärd är en åtgärd som utförs av hello arbetsflöde som har definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="8a49c-147">An action is an operation that's carried out by hello workflow that's defined in a logic app.</span></span> [<span data-ttu-id="8a49c-148">Mer information om åtgärder.</span><span class="sxs-lookup"><span data-stu-id="8a49c-148">Learn more about actions.</span></span>](connectors-overview.md) <span data-ttu-id="8a49c-149">Hej HTTP + Swagger koppling har en möjlig åtgärd.</span><span class="sxs-lookup"><span data-stu-id="8a49c-149">hello HTTP + Swagger connector has one possible action.</span></span>

| <span data-ttu-id="8a49c-150">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="8a49c-150">Action</span></span> | <span data-ttu-id="8a49c-151">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8a49c-151">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8a49c-152">HTTP- + Swagger</span><span class="sxs-lookup"><span data-stu-id="8a49c-152">HTTP + Swagger</span></span> |<span data-ttu-id="8a49c-153">Gör ett HTTP-anrop och returnerar hello svar innehåll</span><span class="sxs-lookup"><span data-stu-id="8a49c-153">Make an HTTP call and return hello response content</span></span> |

### <a name="action-details"></a><span data-ttu-id="8a49c-154">Åtgärdsinformation</span><span class="sxs-lookup"><span data-stu-id="8a49c-154">Action details</span></span>
<span data-ttu-id="8a49c-155">Hej HTTP + Swagger connector medföljer en möjlig åtgärd.</span><span class="sxs-lookup"><span data-stu-id="8a49c-155">hello HTTP + Swagger connector comes with one possible action.</span></span> <span data-ttu-id="8a49c-156">Följande är information om varje hello åtgärder, obligatoriska och valfria inmatningsfält och hello motsvarande utdata information som är associerade med deras användning.</span><span class="sxs-lookup"><span data-stu-id="8a49c-156">Following is information about each of hello actions, their required and optional input fields, and hello corresponding output details that are associated with their usage.</span></span>

#### <a name="http--swagger"></a><span data-ttu-id="8a49c-157">HTTP- + Swagger</span><span class="sxs-lookup"><span data-stu-id="8a49c-157">HTTP + Swagger</span></span>
<span data-ttu-id="8a49c-158">Göra en utgående HTTP-begäran med hjälp av Swagger-metadata.</span><span class="sxs-lookup"><span data-stu-id="8a49c-158">Make an HTTP outbound request with assistance of Swagger metadata.</span></span>
<span data-ttu-id="8a49c-159">En asterisk (*) innebär ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="8a49c-159">An asterisk (*) means a required field.</span></span>

| <span data-ttu-id="8a49c-160">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="8a49c-160">Display name</span></span> | <span data-ttu-id="8a49c-161">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="8a49c-161">Property name</span></span> | <span data-ttu-id="8a49c-162">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8a49c-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8a49c-163">Metoden *</span><span class="sxs-lookup"><span data-stu-id="8a49c-163">Method*</span></span> |<span data-ttu-id="8a49c-164">Metoden</span><span class="sxs-lookup"><span data-stu-id="8a49c-164">method</span></span> |<span data-ttu-id="8a49c-165">HTTP-verb toouse.</span><span class="sxs-lookup"><span data-stu-id="8a49c-165">HTTP verb toouse.</span></span> |
| <span data-ttu-id="8a49c-166">URI *</span><span class="sxs-lookup"><span data-stu-id="8a49c-166">URI*</span></span> |<span data-ttu-id="8a49c-167">URI: N</span><span class="sxs-lookup"><span data-stu-id="8a49c-167">uri</span></span> |<span data-ttu-id="8a49c-168">URI för hello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="8a49c-168">URI for hello HTTP request.</span></span> |
| <span data-ttu-id="8a49c-169">Rubriker</span><span class="sxs-lookup"><span data-stu-id="8a49c-169">Headers</span></span> |<span data-ttu-id="8a49c-170">Rubriker</span><span class="sxs-lookup"><span data-stu-id="8a49c-170">headers</span></span> |<span data-ttu-id="8a49c-171">Ett JSON-objekt i HTTP-huvuden tooinclude.</span><span class="sxs-lookup"><span data-stu-id="8a49c-171">A JSON object of HTTP headers tooinclude.</span></span> |
| <span data-ttu-id="8a49c-172">Innehåll</span><span class="sxs-lookup"><span data-stu-id="8a49c-172">Body</span></span> |<span data-ttu-id="8a49c-173">Brödtext</span><span class="sxs-lookup"><span data-stu-id="8a49c-173">body</span></span> |<span data-ttu-id="8a49c-174">hello text för HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="8a49c-174">hello HTTP request body.</span></span> |
| <span data-ttu-id="8a49c-175">Autentisering</span><span class="sxs-lookup"><span data-stu-id="8a49c-175">Authentication</span></span> |<span data-ttu-id="8a49c-176">Autentisering</span><span class="sxs-lookup"><span data-stu-id="8a49c-176">authentication</span></span> |<span data-ttu-id="8a49c-177">Autentisering toouse för begäran.</span><span class="sxs-lookup"><span data-stu-id="8a49c-177">Authentication toouse for request.</span></span> <span data-ttu-id="8a49c-178">Mer information finns i hello [HTTP-anslutningen](connectors-native-http.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="8a49c-178">For more information, see hello [HTTP connector](connectors-native-http.md#authentication).</span></span> |

<span data-ttu-id="8a49c-179">**Information för utdata**</span><span class="sxs-lookup"><span data-stu-id="8a49c-179">**Output details**</span></span>

<span data-ttu-id="8a49c-180">HTTP-svar</span><span class="sxs-lookup"><span data-stu-id="8a49c-180">HTTP response</span></span>

| <span data-ttu-id="8a49c-181">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="8a49c-181">Property Name</span></span> | <span data-ttu-id="8a49c-182">Datatyp</span><span class="sxs-lookup"><span data-stu-id="8a49c-182">Data type</span></span> | <span data-ttu-id="8a49c-183">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8a49c-183">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8a49c-184">Rubriker</span><span class="sxs-lookup"><span data-stu-id="8a49c-184">Headers</span></span> |<span data-ttu-id="8a49c-185">Objektet</span><span class="sxs-lookup"><span data-stu-id="8a49c-185">object</span></span> |<span data-ttu-id="8a49c-186">Svarsrubriker</span><span class="sxs-lookup"><span data-stu-id="8a49c-186">Response headers</span></span> |
| <span data-ttu-id="8a49c-187">Innehåll</span><span class="sxs-lookup"><span data-stu-id="8a49c-187">Body</span></span> |<span data-ttu-id="8a49c-188">Objektet</span><span class="sxs-lookup"><span data-stu-id="8a49c-188">object</span></span> |<span data-ttu-id="8a49c-189">Objektet Response</span><span class="sxs-lookup"><span data-stu-id="8a49c-189">Response object</span></span> |
| <span data-ttu-id="8a49c-190">Statuskod</span><span class="sxs-lookup"><span data-stu-id="8a49c-190">Status Code</span></span> |<span data-ttu-id="8a49c-191">int</span><span class="sxs-lookup"><span data-stu-id="8a49c-191">int</span></span> |<span data-ttu-id="8a49c-192">HTTP-statuskod</span><span class="sxs-lookup"><span data-stu-id="8a49c-192">HTTP status code</span></span> |

### <a name="http-responses"></a><span data-ttu-id="8a49c-193">HTTP-svar</span><span class="sxs-lookup"><span data-stu-id="8a49c-193">HTTP responses</span></span>
<span data-ttu-id="8a49c-194">När du gör anrop toovarious åtgärder kan få du vissa svar.</span><span class="sxs-lookup"><span data-stu-id="8a49c-194">When making calls toovarious actions, you might get certain responses.</span></span> <span data-ttu-id="8a49c-195">Följande är en tabell som visar motsvarande svar och beskrivningar.</span><span class="sxs-lookup"><span data-stu-id="8a49c-195">Following is a table that outlines corresponding responses and descriptions.</span></span>

| <span data-ttu-id="8a49c-196">Namn</span><span class="sxs-lookup"><span data-stu-id="8a49c-196">Name</span></span> | <span data-ttu-id="8a49c-197">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8a49c-197">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8a49c-198">200</span><span class="sxs-lookup"><span data-stu-id="8a49c-198">200</span></span> |<span data-ttu-id="8a49c-199">OKEJ</span><span class="sxs-lookup"><span data-stu-id="8a49c-199">OK</span></span> |
| <span data-ttu-id="8a49c-200">202</span><span class="sxs-lookup"><span data-stu-id="8a49c-200">202</span></span> |<span data-ttu-id="8a49c-201">Godkänt</span><span class="sxs-lookup"><span data-stu-id="8a49c-201">Accepted</span></span> |
| <span data-ttu-id="8a49c-202">400</span><span class="sxs-lookup"><span data-stu-id="8a49c-202">400</span></span> |<span data-ttu-id="8a49c-203">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="8a49c-203">Bad request</span></span> |
| <span data-ttu-id="8a49c-204">401</span><span class="sxs-lookup"><span data-stu-id="8a49c-204">401</span></span> |<span data-ttu-id="8a49c-205">Behörighet saknas</span><span class="sxs-lookup"><span data-stu-id="8a49c-205">Unauthorized</span></span> |
| <span data-ttu-id="8a49c-206">403</span><span class="sxs-lookup"><span data-stu-id="8a49c-206">403</span></span> |<span data-ttu-id="8a49c-207">Tillåts inte</span><span class="sxs-lookup"><span data-stu-id="8a49c-207">Forbidden</span></span> |
| <span data-ttu-id="8a49c-208">404</span><span class="sxs-lookup"><span data-stu-id="8a49c-208">404</span></span> |<span data-ttu-id="8a49c-209">Det gick inte att hitta</span><span class="sxs-lookup"><span data-stu-id="8a49c-209">Not Found</span></span> |
| <span data-ttu-id="8a49c-210">500</span><span class="sxs-lookup"><span data-stu-id="8a49c-210">500</span></span> |<span data-ttu-id="8a49c-211">Internt serverfel.</span><span class="sxs-lookup"><span data-stu-id="8a49c-211">Internal server error.</span></span> <span data-ttu-id="8a49c-212">Okänt fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="8a49c-212">Unknown error occurred.</span></span> |

- - -
## <a name="next-steps"></a><span data-ttu-id="8a49c-213">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8a49c-213">Next steps</span></span>

* [<span data-ttu-id="8a49c-214">Skapa en logikapp</span><span class="sxs-lookup"><span data-stu-id="8a49c-214">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)
* [<span data-ttu-id="8a49c-215">Sök efter andra kopplingar</span><span class="sxs-lookup"><span data-stu-id="8a49c-215">Find other connectors</span></span>](apis-list.md)