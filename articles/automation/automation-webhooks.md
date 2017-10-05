---
title: Starta en Azure Automation-runbook med en webhook | Microsoft Docs
description: "En webhook som gör att en klient att starta en runbook i Azure Automation från ett HTTP-anrop.  Den här artikeln beskriver hur du skapar en webhook och hur du anropar en om du vill starta en runbook."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9b20237c-a593-4299-bbdc-35c47ee9e55d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: magoedte;bwren;sngun
ms.openlocfilehash: 6c65427fcd18e41a90dfb872aa9525f758b17b87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a><span data-ttu-id="ea0bf-104">Starta en Azure Automation-runbook med en webhook</span><span class="sxs-lookup"><span data-stu-id="ea0bf-104">Starting an Azure Automation runbook with a webhook</span></span>
<span data-ttu-id="ea0bf-105">En *webhook* kan du starta en viss runbook i Azure Automation via en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-105">A *webhook* allows you to start a particular runbook in Azure Automation through a single HTTP request.</span></span> <span data-ttu-id="ea0bf-106">Detta gör att externa tjänster, till exempel Visual Studio Team Services, GitHub, logganalys för Microsoft Operations Management Suite eller anpassade program att starta runbooks utan att implementera en fullständig lösning med hjälp av Azure Automation-API.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-106">This allows external services such as Visual Studio Team Services, GitHub, Microsoft Operations Management Suite Log Analytics, or custom applications to start runbooks without implementing a full solution using the Azure Automation API.</span></span>  
<span data-ttu-id="ea0bf-107">![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)</span><span class="sxs-lookup"><span data-stu-id="ea0bf-107">![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)</span></span>

<span data-ttu-id="ea0bf-108">Du kan jämföra webhooks andra metoder för att starta en runbook [starta en runbook i Azure Automation](automation-starting-a-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="ea0bf-108">You can compare webhooks to other methods of starting a runbook in [Starting a runbook in Azure Automation](automation-starting-a-runbook.md)</span></span>

## <a name="details-of-a-webhook"></a><span data-ttu-id="ea0bf-109">Information om en webhook</span><span class="sxs-lookup"><span data-stu-id="ea0bf-109">Details of a webhook</span></span>
<span data-ttu-id="ea0bf-110">I följande tabell beskrivs de egenskaper som du måste konfigurera för en webhook.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-110">The following table describes the properties that you must configure for a webhook.</span></span>

| <span data-ttu-id="ea0bf-111">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ea0bf-111">Property</span></span> | <span data-ttu-id="ea0bf-112">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ea0bf-112">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ea0bf-113">Namn</span><span class="sxs-lookup"><span data-stu-id="ea0bf-113">Name</span></span> |<span data-ttu-id="ea0bf-114">Du kan ange vilket namn som helst för en webhook eftersom detta inte är exponerad för klienten.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-114">You can provide any name you want for a webhook since this is not exposed to the client.</span></span>  <span data-ttu-id="ea0bf-115">Den används endast för dig för att identifiera runbook i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-115">It is only used for you to identify the runbook in Azure Automation.</span></span> <br>  <span data-ttu-id="ea0bf-116">Som bästa praxis, bör du ge webhooken ett namn som är relaterade till klienter som använder den.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-116">As a best practice, you should give the webhook a name related to the client that will use it.</span></span> |
| <span data-ttu-id="ea0bf-117">URL: EN</span><span class="sxs-lookup"><span data-stu-id="ea0bf-117">URL</span></span> |<span data-ttu-id="ea0bf-118">URL till webhooken är unika adressen som en klient anropar med en HTTP POST till att starta runbook kopplad till webhooken.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-118">The URL of the webhook is the unique address that a client calls with an HTTP POST to start the runbook linked to the webhook.</span></span>  <span data-ttu-id="ea0bf-119">Det genereras automatiskt när du har skapat webhooken.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-119">It is automatically generated when you create the webhook.</span></span>  <span data-ttu-id="ea0bf-120">Du kan inte ange en anpassad URL.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-120">You cannot specify a custom URL.</span></span> <br> <br>  <span data-ttu-id="ea0bf-121">URL: en innehåller en säkerhetstoken som gör att runbook anropas av en tredjeparts-system utan ytterligare autentisering.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-121">The URL contains a security token that allows the runbook to be invoked by a third party system with no further authentication.</span></span> <span data-ttu-id="ea0bf-122">Det bör därför behandlas som ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-122">For this reason, it should be treated like a password.</span></span>  <span data-ttu-id="ea0bf-123">Av säkerhetsskäl bör visa du bara URL: en i Azure-portalen när webhook har skapats.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-123">For security reasons, you can only view the URL in the Azure portal at the time the webhook is created.</span></span> <span data-ttu-id="ea0bf-124">Du bör anteckna URL-Adressen i en säker plats för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-124">You should note the URL in a secure location for future use.</span></span> |
| <span data-ttu-id="ea0bf-125">Förfallodatum</span><span class="sxs-lookup"><span data-stu-id="ea0bf-125">Expiration date</span></span> |<span data-ttu-id="ea0bf-126">Varje webhook har ett sista giltighetsdatum som den kan inte längre användas som ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-126">Like a certificate, each webhook has an expiration date at which time it can no longer be used.</span></span>  <span data-ttu-id="ea0bf-127">Den här upphör att gälla kan ändras när du har skapat webhooken.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-127">This expiration date can be modified after the webhook is created.</span></span> |
| <span data-ttu-id="ea0bf-128">Enabled</span><span class="sxs-lookup"><span data-stu-id="ea0bf-128">Enabled</span></span> |<span data-ttu-id="ea0bf-129">En webhook är aktiverad som standard när den skapas.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-129">A webhook is enabled by default when it is created.</span></span>  <span data-ttu-id="ea0bf-130">Om du den inaktiverad, kommer ingen klient att kunna använda den.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-130">If you set it to Disabled, then no client will be able to use it.</span></span>  <span data-ttu-id="ea0bf-131">Du kan ange den **aktiverad** egenskapen när du skapar webhooken eller när som helst när den skapas.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-131">You can set the **Enabled** property when you create the webhook or anytime once it is created.</span></span> |

### <a name="parameters"></a><span data-ttu-id="ea0bf-132">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ea0bf-132">Parameters</span></span>
<span data-ttu-id="ea0bf-133">En webhook kan definiera värden för runbookparametrar som används när runbook startas genom att webhooken.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-133">A webhook can define values for runbook parameters that are used when the runbook is started by that webhook.</span></span> <span data-ttu-id="ea0bf-134">Webhooken måste innehålla värden för alla obligatoriska parametrar runbook och kan innehålla värden för valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-134">The webhook must include values for any mandatory parameters of the runbook and may include values for optional parameters.</span></span> <span data-ttu-id="ea0bf-135">Ett parametervärde som konfigurerats för att en webhook kan ändras när webhoook.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-135">A parameter value configured to a webhook can be modified even after creating the webhoook.</span></span> <span data-ttu-id="ea0bf-136">Flera webhooks som är kopplad till en enda runbook kan använda olika parametervärden.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-136">Multiple webhooks linked to a single runbook can each use different parameter values.</span></span>

<span data-ttu-id="ea0bf-137">När en klient startar en runbook med en webhook, kan inte det åsidosätta de parametervärden som definierats i webhooken.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-137">When a client starts a runbook using a webhook, it cannot override the parameter values defined in the webhook.</span></span>  <span data-ttu-id="ea0bf-138">Om du vill ta emot data från klienten, runbook kan acceptera en enda parameter med namnet **$WebhookData** av typen [objekt] som innehåller data som klienten innehåller i POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-138">To receive data from the client, the runbook can accept a single parameter called **$WebhookData** of type [object] that will contain data that the client includes in the POST request.</span></span>

![Webhookdata egenskaper](media/automation-webhooks/webhook-data-properties.png)

<span data-ttu-id="ea0bf-140">Den **$WebhookData** objekt har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="ea0bf-140">The **$WebhookData** object will have the following properties:</span></span>

| <span data-ttu-id="ea0bf-141">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ea0bf-141">Property</span></span> | <span data-ttu-id="ea0bf-142">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ea0bf-142">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ea0bf-143">WebhookName</span><span class="sxs-lookup"><span data-stu-id="ea0bf-143">WebhookName</span></span> |<span data-ttu-id="ea0bf-144">Namnet på webhook.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-144">The name of the webhook.</span></span> |
| <span data-ttu-id="ea0bf-145">RequestHeader</span><span class="sxs-lookup"><span data-stu-id="ea0bf-145">RequestHeader</span></span> |<span data-ttu-id="ea0bf-146">Hash-tabell som innehåller huvuden inkommande POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-146">Hash table containing the headers of the incoming POST request.</span></span> |
| <span data-ttu-id="ea0bf-147">requestBody</span><span class="sxs-lookup"><span data-stu-id="ea0bf-147">RequestBody</span></span> |<span data-ttu-id="ea0bf-148">Innehållet i den inkommande POST-begäranden.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-148">The body of the incoming POST request.</span></span>  <span data-ttu-id="ea0bf-149">Detta behåller all formatering, till exempel sträng, JSON, XML, eller formulär-kodade data.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-149">This will retain any formatting such as string, JSON, XML, or form encoded data.</span></span> <span data-ttu-id="ea0bf-150">Runbook måste vara skriven för att arbeta med dataformatet som förväntas.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-150">The runbook must be written to work with the data format that is expected.</span></span> |

<span data-ttu-id="ea0bf-151">Det finns ingen konfiguration av webhooken krävs för att stödja den **$WebhookData** parametern och runbook inte krävs för att godkänna.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-151">There is no configuration of the webhook required to support the **$WebhookData** parameter, and the runbook is not required to accept it.</span></span>  <span data-ttu-id="ea0bf-152">Om runbook inte definierar parametern ignoreras någon information om den begäran som skickades från klienten.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-152">If the runbook does not define the parameter, then any details of the request sent from the client is ignored.</span></span>

<span data-ttu-id="ea0bf-153">Om du anger ett värde för $WebhookData när du skapar webhooken, att värdet ska åsidosättas när webhooken startar runbook med data från klienten POST-begäran, även om klienten inte innehåller några data i begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-153">If you specify a value for $WebhookData when you create the webhook, that value will be overriden when the webhook starts the runbook with the data from the client POST request, even if the client does not include any data in the request body.</span></span>  <span data-ttu-id="ea0bf-154">Om du startar en runbook med hjälp av en annan metod än en webhook $WebhookData, kan du ange ett värde för $Webhookdata som identifieras av runbook.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-154">If you start a runbook that has $WebhookData using a method other than a webhook, you can provide a value for $Webhookdata that will be recognized by the runbook.</span></span>  <span data-ttu-id="ea0bf-155">Det här värdet ska vara ett objekt med samma [egenskaper](#details-of-a-webhook) som $Webhookdata så att runbook fungerar korrekt med det som om den arbetade med faktiska WebhookData skickades av en webhook.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-155">This value should be an object with the same [properties](#details-of-a-webhook) as $Webhookdata so that the runbook can properly work with it as if it was working with actual WebhookData passed by a webhook.</span></span>

<span data-ttu-id="ea0bf-156">Till exempel om du startar följande runbook från Azure Portal och vill skicka vissa exempel WebhookData för att testa, eftersom WebhookData är ett objekt som ska den skickas som JSON i Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-156">For example, if you are starting the following runbook from the Azure Portal and want to pass some sample WebhookData for testing, since WebhookData is an object, it should be passed as JSON in the UI.</span></span>

![WebhookData-parameter från Gränssnittet](media/automation-webhooks/WebhookData-parameter-from-UI.png)

<span data-ttu-id="ea0bf-158">För ovan runbook, om du har följande egenskaper för parametern WebhookData:</span><span class="sxs-lookup"><span data-stu-id="ea0bf-158">For the above runbook, if you have the following properties for the WebhookData parameter:</span></span>

1. <span data-ttu-id="ea0bf-159">WebhookName: *MyWebhook*</span><span class="sxs-lookup"><span data-stu-id="ea0bf-159">WebhookName: *MyWebhook*</span></span>
2. <span data-ttu-id="ea0bf-160">RequestHeader: *från = testanvändare*</span><span class="sxs-lookup"><span data-stu-id="ea0bf-160">RequestHeader: *From=Test User*</span></span>
3. <span data-ttu-id="ea0bf-161">RequestBody: *[”VM1”, ”VM2”]*</span><span class="sxs-lookup"><span data-stu-id="ea0bf-161">RequestBody: *[“VM1”, “VM2”]*</span></span>

<span data-ttu-id="ea0bf-162">Du skulle sedan överföra följande JSON-värde i Användargränssnittet för WebhookData-parametern:</span><span class="sxs-lookup"><span data-stu-id="ea0bf-162">Then you would pass the following JSON value in the UI for the WebhookData parameter:</span></span>  

* <span data-ttu-id="ea0bf-163">{”WebhookName”: ”MyWebhook”, ”RequestHeader”: {”från”: ”testanvändare”}, ”RequestBody” ”: [\"VM1\",\"VM2\"]”}</span><span class="sxs-lookup"><span data-stu-id="ea0bf-163">{"WebhookName":"MyWebhook", "RequestHeader":{"From":"Test User"}, "RequestBody":"[\"VM1\",\"VM2\"]"}</span></span>

![Starta WebhookData parametern från Gränssnittet](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> <span data-ttu-id="ea0bf-165">Värdena för alla indataparametrar loggas med runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-165">The values of all input parameters are logged with the runbook job.</span></span>  <span data-ttu-id="ea0bf-166">Detta innebär att alla indata som angetts av klienten i webhook-begäran är inloggade och tillgänglig för alla med åtkomst till automation-jobb.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-166">This means that any input provided by the client in the webhook request will be logged and available to anyone with access to the automation job.</span></span>  <span data-ttu-id="ea0bf-167">Därför bör du vara försiktig om känslig information, inklusive i webhook-anrop.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-167">For this reason, you should be cautious about including sensitive information in webhook calls.</span></span>
>

## <a name="security"></a><span data-ttu-id="ea0bf-168">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="ea0bf-168">Security</span></span>
<span data-ttu-id="ea0bf-169">Säkerheten för en webhook är beroende av säkerheten för URL: en som innehåller en säkerhetstoken som gör att de kan anropas.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-169">The security of a webhook relies on the privacy of its URL which contains a security token that allows it to be invoked.</span></span> <span data-ttu-id="ea0bf-170">Azure Automation utför inte någon autentisering på begäran så länge det görs till rätt URL.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-170">Azure Automation does not perform any authentication on the request as long as it is made to the correct URL.</span></span> <span data-ttu-id="ea0bf-171">Därför bör du inte använda webhooks för runbooks som utför mycket känsliga funktioner utan att använda ett annat sätt att verifiera begäran.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-171">For this reason, webhooks should not be used for runbooks that perform highly sensitive functions without using an alternate means of validating the request.</span></span>

<span data-ttu-id="ea0bf-172">Du kan inkludera logik i runbook för att fastställa att den anropades av en webhook genom att kontrollera den **WebhookName** -egenskapen för parametern $WebhookData.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-172">You can include logic within the runbook to determine that it was called by a webhook by checking the **WebhookName** property of the $WebhookData parameter.</span></span> <span data-ttu-id="ea0bf-173">Runbook kan utföra ytterligare verifiering genom att söka efter specifika informationen i den **RequestHeader** eller **RequestBody** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-173">The runbook could perform further validation by looking for particular information in the **RequestHeader** or **RequestBody** properties.</span></span>

<span data-ttu-id="ea0bf-174">En annan strategi är att runbooken som utför vissa verifiering av ett externt villkor när det tog emot en webhook-begäran.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-174">Another strategy is to have the runbook perform some validation of an external condition when it received a webhook request.</span></span>  <span data-ttu-id="ea0bf-175">Anta till exempel att en runbook som anropas av GitHub när det finns ett nytt genomförande till GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-175">For example, consider a runbook that is called by GitHub whenever there is a new commit to a GitHub repository.</span></span>  <span data-ttu-id="ea0bf-176">Runbook kan ansluta till GitHub för att verifiera ett nytt genomförande inträffat faktiskt precis innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-176">The runbook might connect to GitHub to validate that a new commit had actually just occurred before continuing.</span></span>

## <a name="creating-a-webhook"></a><span data-ttu-id="ea0bf-177">Skapat en webhook</span><span class="sxs-lookup"><span data-stu-id="ea0bf-177">Creating a webhook</span></span>
<span data-ttu-id="ea0bf-178">Använd följande procedur för att skapa en ny webhook kopplad till en runbook i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-178">Use the following procedure to create a new webhook linked to a runbook in the Azure portal.</span></span>

1. <span data-ttu-id="ea0bf-179">Från den **Runbooks blad** i Azure-portalen klickar du på runbook som webhooken startar att visa dess detaljer-bladet.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-179">From the **Runbooks blade** in the Azure portal, click the runbook that the webhook will start to view its detail blade.</span></span>
2. <span data-ttu-id="ea0bf-180">Klicka på **Webhook** längst upp på bladet för att öppna den **lägga till Webhook** bladet.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-180">Click **Webhook** at the top of the blade to open the **Add Webhook** blade.</span></span> <br><span data-ttu-id="ea0bf-181">
   ![Knappen Webhooks](media/automation-webhooks/webhooks-button.png)</span><span class="sxs-lookup"><span data-stu-id="ea0bf-181">
![Webhooks button](media/automation-webhooks/webhooks-button.png)</span></span>
3. <span data-ttu-id="ea0bf-182">Klicka på **skapa nya webhook** att öppna den **skapa webhook-bladet**.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-182">Click **Create new webhook** to open the **Create webhook blade**.</span></span>
4. <span data-ttu-id="ea0bf-183">Ange en **namn**, **förfallodatum** för webhooken och om den ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-183">Specify a **Name**, **Expiration Date** for the webhook and whether it should be enabled.</span></span> <span data-ttu-id="ea0bf-184">Se [information om en webhook](#details-of-a-webhook) mer dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-184">See [Details of a webhook](#details-of-a-webhook) for more information these properties.</span></span>
5. <span data-ttu-id="ea0bf-185">Klicka på Kopiera-ikonen och tryck på Ctrl + C för att kopiera Webbadressen till webhooken.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-185">Click the copy icon and press Ctrl+C to copy the URL of the webhook.</span></span>  <span data-ttu-id="ea0bf-186">Registrera den på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-186">Then record it in a safe place.</span></span>  <span data-ttu-id="ea0bf-187">**När du har skapat webhooken kan du hämta URL: en igen.**</span><span class="sxs-lookup"><span data-stu-id="ea0bf-187">**Once you create the webhook, you cannot retrieve the URL again.**</span></span> <br><span data-ttu-id="ea0bf-188">
   ![Webhooksadressen](media/automation-webhooks/copy-webhook-url.png)</span><span class="sxs-lookup"><span data-stu-id="ea0bf-188">
![Webhook URL](media/automation-webhooks/copy-webhook-url.png)</span></span>
6. <span data-ttu-id="ea0bf-189">Klicka på **parametrar** att ange värden för runbook-parametrar.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-189">Click **Parameters** to provide values for the runbook parameters.</span></span>  <span data-ttu-id="ea0bf-190">Om runbooken har obligatoriska parametrar, kommer sedan du inte att kunna skapa webhooken om värden har angetts.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-190">If the runbook has mandatory parameters, then you will not be able to create the webhook unless values are provided.</span></span>
7. <span data-ttu-id="ea0bf-191">Klicka på **skapa** att skapa webhooken.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-191">Click **Create** to create the webhook.</span></span>

## <a name="using-a-webhook"></a><span data-ttu-id="ea0bf-192">Med hjälp av en webhook</span><span class="sxs-lookup"><span data-stu-id="ea0bf-192">Using a webhook</span></span>
<span data-ttu-id="ea0bf-193">Om du vill använda en webhook när den har skapats, måste klientprogrammet utfärda en HTTP POST med URL-Adressen för webhooken.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-193">To use a webhook after it has been created, your client application must issue an HTTP POST with the URL for the webhook.</span></span>  <span data-ttu-id="ea0bf-194">Syntaxen för webhooken har följande format.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-194">The syntax of the webhook will be in the following format.</span></span>

    http://<Webhook Server>/token?=<Token Value>

<span data-ttu-id="ea0bf-195">Klienten får ett av följande returkoder från POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-195">The client will receive one of the following return codes from the POST request.</span></span>  

| <span data-ttu-id="ea0bf-196">Kod</span><span class="sxs-lookup"><span data-stu-id="ea0bf-196">Code</span></span> | <span data-ttu-id="ea0bf-197">Text</span><span class="sxs-lookup"><span data-stu-id="ea0bf-197">Text</span></span> | <span data-ttu-id="ea0bf-198">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ea0bf-198">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="ea0bf-199">202</span><span class="sxs-lookup"><span data-stu-id="ea0bf-199">202</span></span> |<span data-ttu-id="ea0bf-200">Godkänt</span><span class="sxs-lookup"><span data-stu-id="ea0bf-200">Accepted</span></span> |<span data-ttu-id="ea0bf-201">Begäran accepterades och runbook placerades i kö.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-201">The request was accepted, and the runbook was successfully queued.</span></span> |
| <span data-ttu-id="ea0bf-202">400</span><span class="sxs-lookup"><span data-stu-id="ea0bf-202">400</span></span> |<span data-ttu-id="ea0bf-203">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="ea0bf-203">Bad Request</span></span> |<span data-ttu-id="ea0bf-204">Begäran accepterades inte av något av följande skäl.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-204">The request was not accepted for one of the following reasons.</span></span> <ul> <li><span data-ttu-id="ea0bf-205">Webhook har gått ut.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-205">The webhook has expired.</span></span></li> <li><span data-ttu-id="ea0bf-206">Webhook har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-206">The webhook is disabled.</span></span></li> <li><span data-ttu-id="ea0bf-207">Token i URL-Adressen är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-207">The token in the URL is invalid.</span></span></li>  </ul> |
| <span data-ttu-id="ea0bf-208">404</span><span class="sxs-lookup"><span data-stu-id="ea0bf-208">404</span></span> |<span data-ttu-id="ea0bf-209">Det gick inte att hitta</span><span class="sxs-lookup"><span data-stu-id="ea0bf-209">Not Found</span></span> |<span data-ttu-id="ea0bf-210">Begäran accepterades inte av något av följande skäl.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-210">The request was not accepted for one of the following reasons.</span></span> <ul> <li><span data-ttu-id="ea0bf-211">Webhooken hittades inte.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-211">The webhook was not found.</span></span></li> <li><span data-ttu-id="ea0bf-212">Runbook hittades inte.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-212">The runbook was not found.</span></span></li> <li><span data-ttu-id="ea0bf-213">Det gick inte att hitta kontot.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-213">The account was not found.</span></span></li>  </ul> |
| <span data-ttu-id="ea0bf-214">500</span><span class="sxs-lookup"><span data-stu-id="ea0bf-214">500</span></span> |<span data-ttu-id="ea0bf-215">Internt serverfel</span><span class="sxs-lookup"><span data-stu-id="ea0bf-215">Internal Server Error</span></span> |<span data-ttu-id="ea0bf-216">URL: en är giltig, men ett fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-216">The URL was valid, but an error occurred.</span></span>  <span data-ttu-id="ea0bf-217">Skicka begäran igen.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-217">Please resubmit the request.</span></span> |

<span data-ttu-id="ea0bf-218">Under förutsättning att begäran lyckas, webhook svaret innehåller jobb-id i JSON-format på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-218">Assuming the request is successful, the webhook response contains the job id in JSON format as follows.</span></span> <span data-ttu-id="ea0bf-219">Den innehåller en enda jobb-id, men JSON-format kan för eventuella framtida förbättringar.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-219">It will contain a single job id, but the JSON format allows for potential future enhancements.</span></span>

    {"JobIds":["<JobId>"]}  

<span data-ttu-id="ea0bf-220">Klienten kan inte fastställa när runbook-jobbet har slutförts eller dess status för slutförande från webhooken.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-220">The client cannot determine when the runbook job completes or its completion status from the webhook.</span></span>  <span data-ttu-id="ea0bf-221">Det kan avgöra den här informationen med jobb-id med en annan metod, till exempel [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) eller [Azure Automation API](https://msdn.microsoft.com/library/azure/mt163826.aspx).</span><span class="sxs-lookup"><span data-stu-id="ea0bf-221">It can determine this information using the job id with another method such as [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) or the [Azure Automation API](https://msdn.microsoft.com/library/azure/mt163826.aspx).</span></span>

### <a name="example"></a><span data-ttu-id="ea0bf-222">Exempel</span><span class="sxs-lookup"><span data-stu-id="ea0bf-222">Example</span></span>
<span data-ttu-id="ea0bf-223">I följande exempel använder Windows PowerShell för att starta en runbook med en webhook.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-223">The following example uses Windows PowerShell to start a runbook with a webhook.</span></span>  <span data-ttu-id="ea0bf-224">Observera att alla språk som kan göra en HTTP-begäran kan använda en webhook; Windows PowerShell används bara exempel.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-224">Note that any language that can make an HTTP request can use a webhook; Windows PowerShell is just used here as an example.</span></span>

<span data-ttu-id="ea0bf-225">Runbook förväntar sig en lista över virtuella datorer som har formaterats i JSON i brödtexten i begäran.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-225">The runbook is expecting a list of virtual machines formatted in JSON in the body of the request.</span></span> <span data-ttu-id="ea0bf-226">Vi också inklusive information om som startar runbook och datum och tid som startas i huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-226">We also are including information about who is starting the runbook and the date and time it is being started in the header of the request.</span></span>      

    $uri = "https://s1events.azure-automation.net/webhooks?token=8ud0dSrSo%2fvHWpYbklW%3c8s0GrOKJZ9Nr7zqcS%2bIQr4c%3d"
    $headers = @{"From"="user@contoso.com";"Date"="05/28/2015 15:47:00"}

    $vms  = @(
                @{ Name="vm01";ServiceName="vm01"},
                @{ Name="vm02";ServiceName="vm02"}
            )
    $body = ConvertTo-Json -InputObject $vms

    $response = Invoke-RestMethod -Method Post -Uri $uri -Headers $headers -Body $body
    $jobid = ConvertFrom-Json $response


<span data-ttu-id="ea0bf-227">Följande bild visar informationen i huvudet (med hjälp av en [Fiddler](http://www.telerik.com/fiddler) trace) från den här begäran.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-227">The following image shows the header information (using a [Fiddler](http://www.telerik.com/fiddler) trace) from this request.</span></span> <span data-ttu-id="ea0bf-228">Detta inkluderar standard sidhuvuden för HTTP-begäran förutom anpassat datum och från-huvuden som vi har lagt till.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-228">This includes standard headers of an HTTP request in addition to the custom Date and From headers that we added.</span></span>  <span data-ttu-id="ea0bf-229">Var och en av dessa värden är tillgänglig för runbook i den **RequestHeaders** -egenskapen för **WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-229">Each of these values is available to the runbook in the **RequestHeaders** property of **WebhookData**.</span></span>

![Knappen Webhooks](media/automation-webhooks/webhook-request-headers.png)

<span data-ttu-id="ea0bf-231">Följande bild visar brödtexten i begäran (med hjälp av en [Fiddler](http://www.telerik.com/fiddler) trace) som är tillgängligt för runbook i den **RequestBody** -egenskapen för **WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-231">The following image shows the body of the request (using a [Fiddler](http://www.telerik.com/fiddler) trace)  that is available to the runbook in the **RequestBody** property of **WebhookData**.</span></span> <span data-ttu-id="ea0bf-232">Detta formateras som JSON eftersom som har det format som ingick i brödtexten i begäran.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-232">This is formatted as JSON because that was the format that was included in the body of the request.</span></span>     

![Knappen Webhooks](media/automation-webhooks/webhook-request-body.png)

<span data-ttu-id="ea0bf-234">Följande bild visar den begäran som skickas från Windows PowerShell och resulterande svar.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-234">The following image shows the request being sent from Windows PowerShell and the resulting response.</span></span>  <span data-ttu-id="ea0bf-235">Jobb-id extraheras från svaret och konverteras till en sträng.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-235">The job id is extracted from the response and converted to a string.</span></span>

![Knappen Webhooks](media/automation-webhooks/webhook-request-response.png)

<span data-ttu-id="ea0bf-237">Följande exempel-runbook som accepterar föregående exempelbegäran och startar de virtuella datorerna som anges i begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-237">The following sample runbook accepts the previous example request and starts the virtual machines specified in the request body.</span></span>

    workflow Test-StartVirtualMachinesFromWebhook
    {
        param (
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {

            # Collect properties of WebhookData
            $WebhookName     =     $WebhookData.WebhookName
            $WebhookHeaders =     $WebhookData.RequestHeader
            $WebhookBody     =     $WebhookData.RequestBody

            # Collect individual headers. VMList converted from JSON.
            $From = $WebhookHeaders.From
            $VMList = ConvertFrom-Json -InputObject $WebhookBody
            Write-Output "Runbook started from webhook $WebhookName by $From."

            # Authenticate to Azure resources
            $Cred = Get-AutomationPSCredential -Name 'MyAzureCredential'
            Add-AzureAccount -Credential $Cred

            # Start each virtual machine
            foreach ($VM in $VMList)
            {
                $VMName = $VM.Name
                Write-Output "Starting $VMName"
                Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName
            }
        }
        else {
            Write-Error "Runbook mean to be started only from webhook."
        }
    }


## <a name="starting-runbooks-in-response-to-azure-alerts"></a><span data-ttu-id="ea0bf-238">Starta runbooks som svar på Azure-aviseringar</span><span class="sxs-lookup"><span data-stu-id="ea0bf-238">Starting runbooks in response to Azure alerts</span></span>
<span data-ttu-id="ea0bf-239">Webhook-aktiverade runbooks som kan användas för att ta hänsyn till [Azure aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="ea0bf-239">Webhook-enabled runbooks can be used to react to [Azure alerts](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span> <span data-ttu-id="ea0bf-240">Resurser i Azure kan övervakas genom att samla in statistik som prestanda, tillgänglighet och användningsinformation med hjälp av Azure-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-240">Resources in Azure can be monitored by collecting the statistics like performance, availability and usage with the help of Azure alerts.</span></span> <span data-ttu-id="ea0bf-241">Du kan ta emot en avisering baserat på övervakning mått eller händelser för din Azure-resurser, för närvarande Automation-konton stöder endast mått.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-241">You can receive an alert based on monitoring metrics or events for your Azure resources, currently Automation Accounts support only metrics.</span></span> <span data-ttu-id="ea0bf-242">När värdet för ett visst mått överstiger tröskelvärdet tilldelade eller om konfigurerade händelsen utlöses sedan ett meddelande skickas till tjänstadministratör eller medadministratörer för att lösa aviseringen, mer information om mått och händelser hittar du [ Azure-aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="ea0bf-242">When the value of a specified metric exceeds the threshold assigned or if the configured event is triggered then a notification is sent to the service admin or co-admins to resolve the alert, for more information on metrics and events please refer to [Azure alerts](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="ea0bf-243">Förutom att använda Azure-aviseringar som ett system, kan du också startar runbooks som svar på aviseringar.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-243">Besides using Azure alerts as a notification system, you can also kick off runbooks in response to alerts.</span></span> <span data-ttu-id="ea0bf-244">Azure Automation ger möjlighet att köra webhook-aktiverade runbooks med Azure-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-244">Azure Automation provides the capability to run webhook-enabled runbooks with Azure alerts.</span></span> <span data-ttu-id="ea0bf-245">När ett mått överskrider det konfigurerade tröskelvärdet sedan varningsregeln blir aktiv och utlöser automation-webhook som i sin tur kör runbook.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-245">When a metric exceeds the configured threshold value then the alert rule becomes active and triggers the automation webhook which in turn executes the runbook.</span></span>

![Webhooks](media/automation-webhooks/webhook-alert.jpg)

### <a name="alert-context"></a><span data-ttu-id="ea0bf-247">Varningskontext</span><span class="sxs-lookup"><span data-stu-id="ea0bf-247">Alert context</span></span>
<span data-ttu-id="ea0bf-248">Överväg att en Azure-resurs, till exempel en virtuell dator, CPU-användning för den här datorn är ett mått för KPI.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-248">Consider an Azure resource such as a virtual machine, CPU utilization of this machine is one of the key performance metric.</span></span> <span data-ttu-id="ea0bf-249">Om CPU-användning är 100% eller mer än en viss mängd under lång tid, kanske du vill starta om den virtuella datorn för att åtgärda problemet.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-249">If the CPU utilization is 100% or more than a certain amount for long period of time, you might want to restart the virtual machine to fix the problem.</span></span> <span data-ttu-id="ea0bf-250">Detta kan lösas genom att konfigurera en aviseringsregel till den virtuella datorn och den här regeln tar CPU-procent som dess mått.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-250">This can be solved by configuring an alert rule to the virtual machine and this rule takes CPU percentage as its metric.</span></span> <span data-ttu-id="ea0bf-251">CPU-procent här tas bara som ett exempel men det finns många andra mått som du kan konfigurera att Azure-resurser och starta om den virtuella datorn är en åtgärd som vidtagits för att åtgärda det här problemet kan du konfigurera runbook för att vidta andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-251">CPU percentage here is just taken as an example but there are many other metrics that you can configure to your Azure resources and restarting the virtual machine is an action that is taken to fix this issue, you can configure the runbook to take other actions.</span></span>

<span data-ttu-id="ea0bf-252">När detta varningsregeln blir aktiv och utlöser webhook-aktiverade-runbook skickas aviseringssammanhanget till runbook.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-252">When this the alert rule becomes active and triggers the webhook-enabled runbook, it sends the alert context to the runbook.</span></span> <span data-ttu-id="ea0bf-253">[Aviseringskontext](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) innehåller information, inklusive **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** och **tidsstämpel** som krävs att identifiera den resurs där den kommer att åtgärda runbook.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-253">[Alert context](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) contains details including **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** and **Timestamp** which are required for the runbook to identify the resource on which it will be taking action.</span></span> <span data-ttu-id="ea0bf-254">Varna kontext är inbäddad i body-delen av den **WebhookData** objekt som skickas till runbook och den kan nås med **Webhook.RequestBody** egenskapen</span><span class="sxs-lookup"><span data-stu-id="ea0bf-254">Alert context is embedded in the body part of the **WebhookData** object sent to the runbook and it can be accessed with **Webhook.RequestBody** property</span></span>

### <a name="example"></a><span data-ttu-id="ea0bf-255">Exempel</span><span class="sxs-lookup"><span data-stu-id="ea0bf-255">Example</span></span>
<span data-ttu-id="ea0bf-256">Skapa en virtuell Azure-dator i din prenumeration och koppla en [aviseringen för att övervaka mått för CPU-procent](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="ea0bf-256">Create an Azure virtual machine in your subscription and associate an [alert to monitor CPU percentage metric](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span> <span data-ttu-id="ea0bf-257">Kontrollera att du fylla i fältet webhook med URL-Adressen till webhooken som skapades när webhooken medan varningen.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-257">While creating the alert make sure you populate the webhook field with the URL of the webhook which was generated while creating the webhook.</span></span>

<span data-ttu-id="ea0bf-258">Följande exempel-runbook utlöses när varningsregeln aktiveras och samlar det in aviseringskontext-parametrar som krävs att identifiera den resurs där den kommer att åtgärda runbook.</span><span class="sxs-lookup"><span data-stu-id="ea0bf-258">The following sample runbook is triggered when the alert rule becomes active and it collects the Alert context parameters which are required for the runbook to identify the resource on which it will be taking action.</span></span>

    workflow Invoke-RunbookUsingAlerts
    {
        param (      
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {   
            # Collect properties of WebhookData.
            $WebhookName    =   $WebhookData.WebhookName
            $WebhookBody    =   $WebhookData.RequestBody
            $WebhookHeaders =   $WebhookData.RequestHeader

            # Outputs information on the webhook name that called This
            Write-Output "This runbook was started from webhook $WebhookName."


            # Obtain the WebhookBody containing the AlertContext
            $WebhookBody = (ConvertFrom-Json -InputObject $WebhookBody)
            Write-Output "`nWEBHOOK BODY"
            Write-Output "============="
            Write-Output $WebhookBody

            # Obtain the AlertContext     
            $AlertContext = [object]$WebhookBody.context

            # Some selected AlertContext information
            Write-Output "`nALERT CONTEXT DATA"
            Write-Output "==================="
            Write-Output $AlertContext.name
            Write-Output $AlertContext.subscriptionId
            Write-Output $AlertContext.resourceGroupName
            Write-Output $AlertContext.resourceName
            Write-Output $AlertContext.resourceType
            Write-Output $AlertContext.resourceId
            Write-Output $AlertContext.timestamp

            # Act on the AlertContext data, in our case restarting the VM.
            # Authenticate to your Azure subscription using Organization ID to be able to restart that Virtual Machine.
            $cred = Get-AutomationPSCredential -Name "MyAzureCredential"
            Add-AzureAccount -Credential $cred
            Select-AzureSubscription -subscriptionName "Visual Studio Ultimate with MSDN"

            #Check the status property of the VM
            Write-Output "Status of VM before taking action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
            Write-Output "Restarting VM"

            # Restart the VM by passing VM name and Service name which are same in this case
            Restart-AzureVM -ServiceName $AlertContext.resourceName -Name $AlertContext.resourceName
            Write-Output "Status of VM after alert is active and takes action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
        }
        else  
        {
            Write-Error "This runbook is meant to only be started from a webhook."  
        }  
    }



## <a name="next-steps"></a><span data-ttu-id="ea0bf-259">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ea0bf-259">Next steps</span></span>
* <span data-ttu-id="ea0bf-260">Mer information om olika sätt att starta en runbook finns [starta en Runbook](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="ea0bf-260">For details on different ways to start a runbook, see [Starting a Runbook](automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="ea0bf-261">Information om hur du visar Status för en Runbook-jobb finns [Runbook som körs i Azure Automation](automation-runbook-execution.md).</span><span class="sxs-lookup"><span data-stu-id="ea0bf-261">For information on viewing the Status of a Runbook Job, refer to [Runbook execution in Azure Automation](automation-runbook-execution.md).</span></span>
* <span data-ttu-id="ea0bf-262">Information om hur du använder Azure Automation för att vidta åtgärder för Azure aviseringar finns [åtgärda Azure VM aviseringar med Automation-Runbooks](automation-azure-vm-alert-integration.md).</span><span class="sxs-lookup"><span data-stu-id="ea0bf-262">To learn how to use Azure Automation to take action on Azure Alerts, see [Remediate Azure VM Alerts with Automation Runbooks](automation-azure-vm-alert-integration.md).</span></span>
