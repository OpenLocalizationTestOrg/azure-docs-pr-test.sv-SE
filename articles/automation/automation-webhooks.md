---
title: aaaStarting en Azure Automation-runbook med en webhook | Microsoft Docs
description: "En webhook som tillåter en klient toostart en runbook i Azure Automation från ett HTTP-anrop.  Den här artikeln beskriver hur toocreate en webhook och hur toocall en toostart en runbook."
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
ms.openlocfilehash: ca6cde66b3784ceb5d0bc5921cee87aea74cb150
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a><span data-ttu-id="8f92c-104">Starta en Azure Automation-runbook med en webhook</span><span class="sxs-lookup"><span data-stu-id="8f92c-104">Starting an Azure Automation runbook with a webhook</span></span>
<span data-ttu-id="8f92c-105">En *webhook* kan du toostart en viss runbook i Azure Automation via en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="8f92c-105">A *webhook* allows you toostart a particular runbook in Azure Automation through a single HTTP request.</span></span> <span data-ttu-id="8f92c-106">Detta gör externa tjänster, till exempel Visual Studio Team Services, GitHub, logganalys för Microsoft Operations Management Suite eller anpassade program toostart runbooks utan att implementera en fullständig lösning med hjälp av hello Azure Automation-API.</span><span class="sxs-lookup"><span data-stu-id="8f92c-106">This allows external services such as Visual Studio Team Services, GitHub, Microsoft Operations Management Suite Log Analytics, or custom applications toostart runbooks without implementing a full solution using hello Azure Automation API.</span></span>  
<span data-ttu-id="8f92c-107">![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)</span><span class="sxs-lookup"><span data-stu-id="8f92c-107">![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)</span></span>

<span data-ttu-id="8f92c-108">Du kan jämföra webhooks tooother metoder för att starta en runbook [starta en runbook i Azure Automation](automation-starting-a-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="8f92c-108">You can compare webhooks tooother methods of starting a runbook in [Starting a runbook in Azure Automation](automation-starting-a-runbook.md)</span></span>

## <a name="details-of-a-webhook"></a><span data-ttu-id="8f92c-109">Information om en webhook</span><span class="sxs-lookup"><span data-stu-id="8f92c-109">Details of a webhook</span></span>
<span data-ttu-id="8f92c-110">hello beskrivs följande tabell hello egenskaper som du måste konfigurera för en webhook.</span><span class="sxs-lookup"><span data-stu-id="8f92c-110">hello following table describes hello properties that you must configure for a webhook.</span></span>

| <span data-ttu-id="8f92c-111">Egenskap</span><span class="sxs-lookup"><span data-stu-id="8f92c-111">Property</span></span> | <span data-ttu-id="8f92c-112">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8f92c-112">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="8f92c-113">Namn</span><span class="sxs-lookup"><span data-stu-id="8f92c-113">Name</span></span> |<span data-ttu-id="8f92c-114">Du kan ange ett namn som du vill använda för en webhook eftersom det är inte visas toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="8f92c-114">You can provide any name you want for a webhook since this is not exposed toohello client.</span></span>  <span data-ttu-id="8f92c-115">Den används endast för du tooidentify hello runbook i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="8f92c-115">It is only used for you tooidentify hello runbook in Azure Automation.</span></span> <br>  <span data-ttu-id="8f92c-116">Som bästa praxis bör ge du hello webhook en namnet relaterade toohello klient som använder den.</span><span class="sxs-lookup"><span data-stu-id="8f92c-116">As a best practice, you should give hello webhook a name related toohello client that will use it.</span></span> |
| <span data-ttu-id="8f92c-117">URL: EN</span><span class="sxs-lookup"><span data-stu-id="8f92c-117">URL</span></span> |<span data-ttu-id="8f92c-118">hello-URL för hello webhook är hello unik adress att en klient anropar med en HTTP POST toostart hello runbook länkade toohello webhooken.</span><span class="sxs-lookup"><span data-stu-id="8f92c-118">hello URL of hello webhook is hello unique address that a client calls with an HTTP POST toostart hello runbook linked toohello webhook.</span></span>  <span data-ttu-id="8f92c-119">Den genereras automatiskt när du skapar hello webhooken.</span><span class="sxs-lookup"><span data-stu-id="8f92c-119">It is automatically generated when you create hello webhook.</span></span>  <span data-ttu-id="8f92c-120">Du kan inte ange en anpassad URL.</span><span class="sxs-lookup"><span data-stu-id="8f92c-120">You cannot specify a custom URL.</span></span> <br> <br>  <span data-ttu-id="8f92c-121">hello Webbadressen innehåller en säkerhetstoken som gör hello runbook toobe anropas av en tredjeparts-system utan ytterligare autentisering.</span><span class="sxs-lookup"><span data-stu-id="8f92c-121">hello URL contains a security token that allows hello runbook toobe invoked by a third party system with no further authentication.</span></span> <span data-ttu-id="8f92c-122">Det bör därför behandlas som ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="8f92c-122">For this reason, it should be treated like a password.</span></span>  <span data-ttu-id="8f92c-123">Av säkerhetsskäl kan du endast visa hello URL: en i hello Azure-portalen på hello tid hello webhook har skapats.</span><span class="sxs-lookup"><span data-stu-id="8f92c-123">For security reasons, you can only view hello URL in hello Azure portal at hello time hello webhook is created.</span></span> <span data-ttu-id="8f92c-124">Du bör anteckna hello URL i en säker plats för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="8f92c-124">You should note hello URL in a secure location for future use.</span></span> |
| <span data-ttu-id="8f92c-125">Förfallodatum</span><span class="sxs-lookup"><span data-stu-id="8f92c-125">Expiration date</span></span> |<span data-ttu-id="8f92c-126">Varje webhook har ett sista giltighetsdatum som den kan inte längre användas som ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="8f92c-126">Like a certificate, each webhook has an expiration date at which time it can no longer be used.</span></span>  <span data-ttu-id="8f92c-127">Den här upphör att gälla kan ändras när hello webhook har skapats.</span><span class="sxs-lookup"><span data-stu-id="8f92c-127">This expiration date can be modified after hello webhook is created.</span></span> |
| <span data-ttu-id="8f92c-128">Enabled</span><span class="sxs-lookup"><span data-stu-id="8f92c-128">Enabled</span></span> |<span data-ttu-id="8f92c-129">En webhook är aktiverad som standard när den skapas.</span><span class="sxs-lookup"><span data-stu-id="8f92c-129">A webhook is enabled by default when it is created.</span></span>  <span data-ttu-id="8f92c-130">Om du anger det tooDisabled kommer ingen klient kommer att kunna toouse den.</span><span class="sxs-lookup"><span data-stu-id="8f92c-130">If you set it tooDisabled, then no client will be able toouse it.</span></span>  <span data-ttu-id="8f92c-131">Du kan ange hello **aktiverad** egenskapen när du skapar hello webhook eller när som helst när den skapas.</span><span class="sxs-lookup"><span data-stu-id="8f92c-131">You can set hello **Enabled** property when you create hello webhook or anytime once it is created.</span></span> |

### <a name="parameters"></a><span data-ttu-id="8f92c-132">Parametrar</span><span class="sxs-lookup"><span data-stu-id="8f92c-132">Parameters</span></span>
<span data-ttu-id="8f92c-133">En webhook kan definiera värden för runbookparametrar som används när hello runbook startas genom att webhooken.</span><span class="sxs-lookup"><span data-stu-id="8f92c-133">A webhook can define values for runbook parameters that are used when hello runbook is started by that webhook.</span></span> <span data-ttu-id="8f92c-134">Hej webhook måste innehålla värden för alla obligatoriska parametrar för hello runbook och kan innehålla värden för valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="8f92c-134">hello webhook must include values for any mandatory parameters of hello runbook and may include values for optional parameters.</span></span> <span data-ttu-id="8f92c-135">En parameter värdet som konfigurerats tooa webhook kan ändras när hello webhoook.</span><span class="sxs-lookup"><span data-stu-id="8f92c-135">A parameter value configured tooa webhook can be modified even after creating hello webhoook.</span></span> <span data-ttu-id="8f92c-136">Flera webhooks länkade tooa enstaka runbook kan använda olika parametervärden.</span><span class="sxs-lookup"><span data-stu-id="8f92c-136">Multiple webhooks linked tooa single runbook can each use different parameter values.</span></span>

<span data-ttu-id="8f92c-137">Det kan inte åsidosätta hello parametervärden som definierats i hello webhook när en klient startar en runbook med en webhook.</span><span class="sxs-lookup"><span data-stu-id="8f92c-137">When a client starts a runbook using a webhook, it cannot override hello parameter values defined in hello webhook.</span></span>  <span data-ttu-id="8f92c-138">tooreceive data från hello klient, hello runbook kan acceptera en enda parameter med namnet **$WebhookData** av typen [objekt] som innehåller data som innehåller den hello-klienten i hello POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="8f92c-138">tooreceive data from hello client, hello runbook can accept a single parameter called **$WebhookData** of type [object] that will contain data that hello client includes in hello POST request.</span></span>

![Webhookdata egenskaper](media/automation-webhooks/webhook-data-properties.png)

<span data-ttu-id="8f92c-140">Hej **$WebhookData** objekt har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="8f92c-140">hello **$WebhookData** object will have hello following properties:</span></span>

| <span data-ttu-id="8f92c-141">Egenskap</span><span class="sxs-lookup"><span data-stu-id="8f92c-141">Property</span></span> | <span data-ttu-id="8f92c-142">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8f92c-142">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="8f92c-143">WebhookName</span><span class="sxs-lookup"><span data-stu-id="8f92c-143">WebhookName</span></span> |<span data-ttu-id="8f92c-144">hello namnet på webhook hello.</span><span class="sxs-lookup"><span data-stu-id="8f92c-144">hello name of hello webhook.</span></span> |
| <span data-ttu-id="8f92c-145">RequestHeader</span><span class="sxs-lookup"><span data-stu-id="8f92c-145">RequestHeader</span></span> |<span data-ttu-id="8f92c-146">Hash-tabell som innehåller hello-huvuden hello inkommande POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="8f92c-146">Hash table containing hello headers of hello incoming POST request.</span></span> |
| <span data-ttu-id="8f92c-147">requestBody</span><span class="sxs-lookup"><span data-stu-id="8f92c-147">RequestBody</span></span> |<span data-ttu-id="8f92c-148">hello brödtext hello inkommande POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="8f92c-148">hello body of hello incoming POST request.</span></span>  <span data-ttu-id="8f92c-149">Detta behåller all formatering, till exempel sträng, JSON, XML, eller formulär-kodade data.</span><span class="sxs-lookup"><span data-stu-id="8f92c-149">This will retain any formatting such as string, JSON, XML, or form encoded data.</span></span> <span data-ttu-id="8f92c-150">Hej runbook måste skrivas toowork med hello dataformat som förväntas.</span><span class="sxs-lookup"><span data-stu-id="8f92c-150">hello runbook must be written toowork with hello data format that is expected.</span></span> |

<span data-ttu-id="8f92c-151">Det finns ingen konfiguration av hello webhook krävs toosupport hello **$WebhookData** parameter, och hello runbook är inte obligatoriska tooaccept den.</span><span class="sxs-lookup"><span data-stu-id="8f92c-151">There is no configuration of hello webhook required toosupport hello **$WebhookData** parameter, and hello runbook is not required tooaccept it.</span></span>  <span data-ttu-id="8f92c-152">Om hello runbook inte definierar hello parametern ignoreras någon information om hello-begäran som skickades från hello-klient.</span><span class="sxs-lookup"><span data-stu-id="8f92c-152">If hello runbook does not define hello parameter, then any details of hello request sent from hello client is ignored.</span></span>

<span data-ttu-id="8f92c-153">Om du anger ett värde för $WebhookData när du skapar hello webhook ska värdet åsidosättas när hello webhook startar hello runbook med hello data från hello klienten POST-begäran, även om hello klienten inte innehåller några data i begärandetexten för hello.</span><span class="sxs-lookup"><span data-stu-id="8f92c-153">If you specify a value for $WebhookData when you create hello webhook, that value will be overriden when hello webhook starts hello runbook with hello data from hello client POST request, even if hello client does not include any data in hello request body.</span></span>  <span data-ttu-id="8f92c-154">Om du startar en runbook med hjälp av en annan metod än en webhook $WebhookData, kan du ange ett värde för $Webhookdata som identifieras av hello runbook.</span><span class="sxs-lookup"><span data-stu-id="8f92c-154">If you start a runbook that has $WebhookData using a method other than a webhook, you can provide a value for $Webhookdata that will be recognized by hello runbook.</span></span>  <span data-ttu-id="8f92c-155">Det här värdet ska vara ett objekt med hello samma [egenskaper](#details-of-a-webhook) som $Webhookdata så att hello runbook korrekt kan arbeta med det som om den arbetade med faktiska WebhookData skickades av en webhook.</span><span class="sxs-lookup"><span data-stu-id="8f92c-155">This value should be an object with hello same [properties](#details-of-a-webhook) as $Webhookdata so that hello runbook can properly work with it as if it was working with actual WebhookData passed by a webhook.</span></span>

<span data-ttu-id="8f92c-156">Till exempel om du startar hello följande runbook från hello Azure-portalen och vill toopass några exempel på WebhookData för att testa, eftersom WebhookData är ett objekt som ska den skickas som JSON i hello Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="8f92c-156">For example, if you are starting hello following runbook from hello Azure Portal and want toopass some sample WebhookData for testing, since WebhookData is an object, it should be passed as JSON in hello UI.</span></span>

![WebhookData-parameter från Gränssnittet](media/automation-webhooks/WebhookData-parameter-from-UI.png)

<span data-ttu-id="8f92c-158">För hello ovan runbook, om du har följande egenskaper för hello WebhookData parametern hello:</span><span class="sxs-lookup"><span data-stu-id="8f92c-158">For hello above runbook, if you have hello following properties for hello WebhookData parameter:</span></span>

1. <span data-ttu-id="8f92c-159">WebhookName: *MyWebhook*</span><span class="sxs-lookup"><span data-stu-id="8f92c-159">WebhookName: *MyWebhook*</span></span>
2. <span data-ttu-id="8f92c-160">RequestHeader: *från = testanvändare*</span><span class="sxs-lookup"><span data-stu-id="8f92c-160">RequestHeader: *From=Test User*</span></span>
3. <span data-ttu-id="8f92c-161">RequestBody: *[”VM1”, ”VM2”]*</span><span class="sxs-lookup"><span data-stu-id="8f92c-161">RequestBody: *[“VM1”, “VM2”]*</span></span>

<span data-ttu-id="8f92c-162">Du skulle sedan överföra hello följande JSON-värde i hello Användargränssnittet för hello WebhookData parameter:</span><span class="sxs-lookup"><span data-stu-id="8f92c-162">Then you would pass hello following JSON value in hello UI for hello WebhookData parameter:</span></span>  

* <span data-ttu-id="8f92c-163">{”WebhookName”: ”MyWebhook”, ”RequestHeader”: {”från”: ”testanvändare”}, ”RequestBody” ”: [\"VM1\",\"VM2\"]”}</span><span class="sxs-lookup"><span data-stu-id="8f92c-163">{"WebhookName":"MyWebhook", "RequestHeader":{"From":"Test User"}, "RequestBody":"[\"VM1\",\"VM2\"]"}</span></span>

![Starta WebhookData parametern från Gränssnittet](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> <span data-ttu-id="8f92c-165">hello värdena för alla indataparametrar loggas med hello runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="8f92c-165">hello values of all input parameters are logged with hello runbook job.</span></span>  <span data-ttu-id="8f92c-166">Detta innebär att alla indata som angetts av hello-klienten i hello webhook begäran kommer att loggas och tillgängliga tooanyone med åtkomst toohello automation-jobb.</span><span class="sxs-lookup"><span data-stu-id="8f92c-166">This means that any input provided by hello client in hello webhook request will be logged and available tooanyone with access toohello automation job.</span></span>  <span data-ttu-id="8f92c-167">Därför bör du vara försiktig om känslig information, inklusive i webhook-anrop.</span><span class="sxs-lookup"><span data-stu-id="8f92c-167">For this reason, you should be cautious about including sensitive information in webhook calls.</span></span>
>

## <a name="security"></a><span data-ttu-id="8f92c-168">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="8f92c-168">Security</span></span>
<span data-ttu-id="8f92c-169">hello säkerheten för en webhook är beroende av hello sekretessen för URL: en som innehåller en säkerhetstoken som gör det toobe anropas.</span><span class="sxs-lookup"><span data-stu-id="8f92c-169">hello security of a webhook relies on hello privacy of its URL which contains a security token that allows it toobe invoked.</span></span> <span data-ttu-id="8f92c-170">Azure Automation utför inte någon autentisering på hello begäran så länge det görs toohello rätt URL.</span><span class="sxs-lookup"><span data-stu-id="8f92c-170">Azure Automation does not perform any authentication on hello request as long as it is made toohello correct URL.</span></span> <span data-ttu-id="8f92c-171">Därför användas webhooks inte för runbooks som utför mycket känsliga funktioner utan att använda ett annat sätt att verifiera hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="8f92c-171">For this reason, webhooks should not be used for runbooks that perform highly sensitive functions without using an alternate means of validating hello request.</span></span>

<span data-ttu-id="8f92c-172">Du kan inkludera logik i hello runbook toodetermine att den anropades av en webhook genom att kontrollera hello **WebhookName** -egenskapen för hello $WebhookData-parametern.</span><span class="sxs-lookup"><span data-stu-id="8f92c-172">You can include logic within hello runbook toodetermine that it was called by a webhook by checking hello **WebhookName** property of hello $WebhookData parameter.</span></span> <span data-ttu-id="8f92c-173">Hej runbook kan utföra ytterligare verifiering genom att söka efter en viss information i hello **RequestHeader** eller **RequestBody** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8f92c-173">hello runbook could perform further validation by looking for particular information in hello **RequestHeader** or **RequestBody** properties.</span></span>

<span data-ttu-id="8f92c-174">En annan strategi är toohave hello runbook utför vissa verifiering av ett externt villkor när det tog emot en webhook-begäran.</span><span class="sxs-lookup"><span data-stu-id="8f92c-174">Another strategy is toohave hello runbook perform some validation of an external condition when it received a webhook request.</span></span>  <span data-ttu-id="8f92c-175">Anta till exempel att en runbook som anropas av GitHub när det finns en ny commit tooa GitHub-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="8f92c-175">For example, consider a runbook that is called by GitHub whenever there is a new commit tooa GitHub repository.</span></span>  <span data-ttu-id="8f92c-176">Hej runbook kan ansluta tooGitHub toovalidate som ett nytt genomförande inträffat faktiskt precis innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="8f92c-176">hello runbook might connect tooGitHub toovalidate that a new commit had actually just occurred before continuing.</span></span>

## <a name="creating-a-webhook"></a><span data-ttu-id="8f92c-177">Skapat en webhook</span><span class="sxs-lookup"><span data-stu-id="8f92c-177">Creating a webhook</span></span>
<span data-ttu-id="8f92c-178">Använd hello följa proceduren toocreate en ny webhook länkade tooa runbook i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8f92c-178">Use hello following procedure toocreate a new webhook linked tooa runbook in hello Azure portal.</span></span>

1. <span data-ttu-id="8f92c-179">Från hello **Runbooks blad** i hello Azure-portalen klickar du på hello-runbook som hello webhook startar tooview dess detaljer-bladet.</span><span class="sxs-lookup"><span data-stu-id="8f92c-179">From hello **Runbooks blade** in hello Azure portal, click hello runbook that hello webhook will start tooview its detail blade.</span></span>
2. <span data-ttu-id="8f92c-180">Klicka på **Webhook** hello överst i hello bladet tooopen hello **lägga till Webhook** bladet.</span><span class="sxs-lookup"><span data-stu-id="8f92c-180">Click **Webhook** at hello top of hello blade tooopen hello **Add Webhook** blade.</span></span> <br><span data-ttu-id="8f92c-181">
   ![Knappen Webhooks](media/automation-webhooks/webhooks-button.png)</span><span class="sxs-lookup"><span data-stu-id="8f92c-181">
![Webhooks button](media/automation-webhooks/webhooks-button.png)</span></span>
3. <span data-ttu-id="8f92c-182">Klicka på **skapa nya webhook** tooopen hello **skapa webhook-bladet**.</span><span class="sxs-lookup"><span data-stu-id="8f92c-182">Click **Create new webhook** tooopen hello **Create webhook blade**.</span></span>
4. <span data-ttu-id="8f92c-183">Ange en **namn**, **förfallodatum** för hello webhook och om den ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="8f92c-183">Specify a **Name**, **Expiration Date** for hello webhook and whether it should be enabled.</span></span> <span data-ttu-id="8f92c-184">Se [information om en webhook](#details-of-a-webhook) mer dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8f92c-184">See [Details of a webhook](#details-of-a-webhook) for more information these properties.</span></span>
5. <span data-ttu-id="8f92c-185">Klicka på Kopiera-ikonen hello och tryck på Ctrl + C toocopy hello URL för hello webhook.</span><span class="sxs-lookup"><span data-stu-id="8f92c-185">Click hello copy icon and press Ctrl+C toocopy hello URL of hello webhook.</span></span>  <span data-ttu-id="8f92c-186">Registrera den på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="8f92c-186">Then record it in a safe place.</span></span>  <span data-ttu-id="8f92c-187">**När du skapar hello webhook kan hämta du inte hello URL igen.**</span><span class="sxs-lookup"><span data-stu-id="8f92c-187">**Once you create hello webhook, you cannot retrieve hello URL again.**</span></span> <br><span data-ttu-id="8f92c-188">
   ![Webhooksadressen](media/automation-webhooks/copy-webhook-url.png)</span><span class="sxs-lookup"><span data-stu-id="8f92c-188">
![Webhook URL](media/automation-webhooks/copy-webhook-url.png)</span></span>
6. <span data-ttu-id="8f92c-189">Klicka på **parametrar** tooprovide värden för hello runbook-parametrar.</span><span class="sxs-lookup"><span data-stu-id="8f92c-189">Click **Parameters** tooprovide values for hello runbook parameters.</span></span>  <span data-ttu-id="8f92c-190">Om hello runbook har obligatoriska parametrar kan blir du inte kan toocreate hello webhook om värden har angetts.</span><span class="sxs-lookup"><span data-stu-id="8f92c-190">If hello runbook has mandatory parameters, then you will not be able toocreate hello webhook unless values are provided.</span></span>
7. <span data-ttu-id="8f92c-191">Klicka på **skapa** toocreate hello webhooken.</span><span class="sxs-lookup"><span data-stu-id="8f92c-191">Click **Create** toocreate hello webhook.</span></span>

## <a name="using-a-webhook"></a><span data-ttu-id="8f92c-192">Med hjälp av en webhook</span><span class="sxs-lookup"><span data-stu-id="8f92c-192">Using a webhook</span></span>
<span data-ttu-id="8f92c-193">toouse en webhook när den har skapats, ditt klientprogram utfärda en HTTP POST med hello URL för hello webhooken.</span><span class="sxs-lookup"><span data-stu-id="8f92c-193">toouse a webhook after it has been created, your client application must issue an HTTP POST with hello URL for hello webhook.</span></span>  <span data-ttu-id="8f92c-194">hello syntaxen för hello webhook har hello följande format.</span><span class="sxs-lookup"><span data-stu-id="8f92c-194">hello syntax of hello webhook will be in hello following format.</span></span>

    http://<Webhook Server>/token?=<Token Value>

<span data-ttu-id="8f92c-195">hello klienten får en hello följande returkoder från hello POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="8f92c-195">hello client will receive one of hello following return codes from hello POST request.</span></span>  

| <span data-ttu-id="8f92c-196">Kod</span><span class="sxs-lookup"><span data-stu-id="8f92c-196">Code</span></span> | <span data-ttu-id="8f92c-197">Text</span><span class="sxs-lookup"><span data-stu-id="8f92c-197">Text</span></span> | <span data-ttu-id="8f92c-198">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8f92c-198">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="8f92c-199">202</span><span class="sxs-lookup"><span data-stu-id="8f92c-199">202</span></span> |<span data-ttu-id="8f92c-200">Godkänt</span><span class="sxs-lookup"><span data-stu-id="8f92c-200">Accepted</span></span> |<span data-ttu-id="8f92c-201">hello-begäran har godkänts och hello runbook placerades i kö.</span><span class="sxs-lookup"><span data-stu-id="8f92c-201">hello request was accepted, and hello runbook was successfully queued.</span></span> |
| <span data-ttu-id="8f92c-202">400</span><span class="sxs-lookup"><span data-stu-id="8f92c-202">400</span></span> |<span data-ttu-id="8f92c-203">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="8f92c-203">Bad Request</span></span> |<span data-ttu-id="8f92c-204">hello begäran accepterades inte av något av följande orsaker hello.</span><span class="sxs-lookup"><span data-stu-id="8f92c-204">hello request was not accepted for one of hello following reasons.</span></span> <ul> <li><span data-ttu-id="8f92c-205">Hej webhook har gått ut.</span><span class="sxs-lookup"><span data-stu-id="8f92c-205">hello webhook has expired.</span></span></li> <li><span data-ttu-id="8f92c-206">Hej webhook har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="8f92c-206">hello webhook is disabled.</span></span></li> <li><span data-ttu-id="8f92c-207">hello-token i hello URL: en är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="8f92c-207">hello token in hello URL is invalid.</span></span></li>  </ul> |
| <span data-ttu-id="8f92c-208">404</span><span class="sxs-lookup"><span data-stu-id="8f92c-208">404</span></span> |<span data-ttu-id="8f92c-209">Det gick inte att hitta</span><span class="sxs-lookup"><span data-stu-id="8f92c-209">Not Found</span></span> |<span data-ttu-id="8f92c-210">hello begäran accepterades inte av något av följande orsaker hello.</span><span class="sxs-lookup"><span data-stu-id="8f92c-210">hello request was not accepted for one of hello following reasons.</span></span> <ul> <li><span data-ttu-id="8f92c-211">Hej webhook hittades inte.</span><span class="sxs-lookup"><span data-stu-id="8f92c-211">hello webhook was not found.</span></span></li> <li><span data-ttu-id="8f92c-212">Hej runbook hittades inte.</span><span class="sxs-lookup"><span data-stu-id="8f92c-212">hello runbook was not found.</span></span></li> <li><span data-ttu-id="8f92c-213">hello-konto hittades inte.</span><span class="sxs-lookup"><span data-stu-id="8f92c-213">hello account was not found.</span></span></li>  </ul> |
| <span data-ttu-id="8f92c-214">500</span><span class="sxs-lookup"><span data-stu-id="8f92c-214">500</span></span> |<span data-ttu-id="8f92c-215">Internt serverfel</span><span class="sxs-lookup"><span data-stu-id="8f92c-215">Internal Server Error</span></span> |<span data-ttu-id="8f92c-216">hello URL är giltig, men ett fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="8f92c-216">hello URL was valid, but an error occurred.</span></span>  <span data-ttu-id="8f92c-217">Skicka hello begäran igen.</span><span class="sxs-lookup"><span data-stu-id="8f92c-217">Please resubmit hello request.</span></span> |

<span data-ttu-id="8f92c-218">Under förutsättning att hello begäran lyckas, hello webhook svaret innehåller hello jobb-id i JSON-format på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="8f92c-218">Assuming hello request is successful, hello webhook response contains hello job id in JSON format as follows.</span></span> <span data-ttu-id="8f92c-219">Den innehåller en enda jobb-id, men hello JSON-format kan för eventuella framtida förbättringar.</span><span class="sxs-lookup"><span data-stu-id="8f92c-219">It will contain a single job id, but hello JSON format allows for potential future enhancements.</span></span>

    {"JobIds":["<JobId>"]}  

<span data-ttu-id="8f92c-220">hello-klienten kan inte fastställa när hello runbook-jobbet har slutförts eller dess status för slutförande från hello webhooken.</span><span class="sxs-lookup"><span data-stu-id="8f92c-220">hello client cannot determine when hello runbook job completes or its completion status from hello webhook.</span></span>  <span data-ttu-id="8f92c-221">Det kan avgöra den här informationen med hello jobb-id med en annan metod, till exempel [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) eller hello [Azure Automation API](https://msdn.microsoft.com/library/azure/mt163826.aspx).</span><span class="sxs-lookup"><span data-stu-id="8f92c-221">It can determine this information using hello job id with another method such as [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) or hello [Azure Automation API](https://msdn.microsoft.com/library/azure/mt163826.aspx).</span></span>

### <a name="example"></a><span data-ttu-id="8f92c-222">Exempel</span><span class="sxs-lookup"><span data-stu-id="8f92c-222">Example</span></span>
<span data-ttu-id="8f92c-223">hello följande exempel använder Windows PowerShell toostart en runbook med en webhook.</span><span class="sxs-lookup"><span data-stu-id="8f92c-223">hello following example uses Windows PowerShell toostart a runbook with a webhook.</span></span>  <span data-ttu-id="8f92c-224">Observera att alla språk som kan göra en HTTP-begäran kan använda en webhook; Windows PowerShell används bara exempel.</span><span class="sxs-lookup"><span data-stu-id="8f92c-224">Note that any language that can make an HTTP request can use a webhook; Windows PowerShell is just used here as an example.</span></span>

<span data-ttu-id="8f92c-225">Hej runbook förväntar sig en lista över virtuella datorer som har formaterats i JSON i hello brödtext hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="8f92c-225">hello runbook is expecting a list of virtual machines formatted in JSON in hello body of hello request.</span></span> <span data-ttu-id="8f92c-226">Vi också inklusive information om som startar hello runbook och hello datum och tid startas i hello-rubriken för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="8f92c-226">We also are including information about who is starting hello runbook and hello date and time it is being started in hello header of hello request.</span></span>      

    $uri = "https://s1events.azure-automation.net/webhooks?token=8ud0dSrSo%2fvHWpYbklW%3c8s0GrOKJZ9Nr7zqcS%2bIQr4c%3d"
    $headers = @{"From"="user@contoso.com";"Date"="05/28/2015 15:47:00"}

    $vms  = @(
                @{ Name="vm01";ServiceName="vm01"},
                @{ Name="vm02";ServiceName="vm02"}
            )
    $body = ConvertTo-Json -InputObject $vms

    $response = Invoke-RestMethod -Method Post -Uri $uri -Headers $headers -Body $body
    $jobid = ConvertFrom-Json $response


<span data-ttu-id="8f92c-227">hello följande bild visar hello rubrikinformation (med hjälp av en [Fiddler](http://www.telerik.com/fiddler) trace) från den här begäran.</span><span class="sxs-lookup"><span data-stu-id="8f92c-227">hello following image shows hello header information (using a [Fiddler](http://www.telerik.com/fiddler) trace) from this request.</span></span> <span data-ttu-id="8f92c-228">Detta inkluderar standard sidhuvuden för HTTP-begäran i tillägg toohello anpassade datum och från-huvuden som vi har lagt till.</span><span class="sxs-lookup"><span data-stu-id="8f92c-228">This includes standard headers of an HTTP request in addition toohello custom Date and From headers that we added.</span></span>  <span data-ttu-id="8f92c-229">Var och en av dessa värden är tillgänglig toohello runbook i hello **RequestHeaders** -egenskapen för **WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="8f92c-229">Each of these values is available toohello runbook in hello **RequestHeaders** property of **WebhookData**.</span></span>

![Knappen Webhooks](media/automation-webhooks/webhook-request-headers.png)

<span data-ttu-id="8f92c-231">hello följande bild visar hello brödtexten i begäran hello (med hjälp av en [Fiddler](http://www.telerik.com/fiddler) trace) som är tillgängliga toohello runbook i hello **RequestBody** -egenskapen för **WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="8f92c-231">hello following image shows hello body of hello request (using a [Fiddler](http://www.telerik.com/fiddler) trace)  that is available toohello runbook in hello **RequestBody** property of **WebhookData**.</span></span> <span data-ttu-id="8f92c-232">Detta formateras som JSON eftersom som var hello-format som ingick i hello brödtext hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="8f92c-232">This is formatted as JSON because that was hello format that was included in hello body of hello request.</span></span>     

![Knappen Webhooks](media/automation-webhooks/webhook-request-body.png)

<span data-ttu-id="8f92c-234">hello visar följande bild hello-begäran som skickas från Windows PowerShell och hello resulterande svar.</span><span class="sxs-lookup"><span data-stu-id="8f92c-234">hello following image shows hello request being sent from Windows PowerShell and hello resulting response.</span></span>  <span data-ttu-id="8f92c-235">hello jobb-id som extraheras från hello svar och konverterade tooa sträng.</span><span class="sxs-lookup"><span data-stu-id="8f92c-235">hello job id is extracted from hello response and converted tooa string.</span></span>

![Knappen Webhooks](media/automation-webhooks/webhook-request-response.png)

<span data-ttu-id="8f92c-237">hello följande exempel-runbook accepterar hello föregående exempelbegäran och startar hello virtuella datorer som anges i begärandetexten för hello.</span><span class="sxs-lookup"><span data-stu-id="8f92c-237">hello following sample runbook accepts hello previous example request and starts hello virtual machines specified in hello request body.</span></span>

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

            # Authenticate tooAzure resources
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
            Write-Error "Runbook mean toobe started only from webhook."
        }
    }


## <a name="starting-runbooks-in-response-tooazure-alerts"></a><span data-ttu-id="8f92c-238">Starta runbooks i svaret tooAzure aviseringar</span><span class="sxs-lookup"><span data-stu-id="8f92c-238">Starting runbooks in response tooAzure alerts</span></span>
<span data-ttu-id="8f92c-239">Webhook-aktiverade runbooks kan vara används tooreact för[Azure aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="8f92c-239">Webhook-enabled runbooks can be used tooreact too[Azure alerts](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span> <span data-ttu-id="8f92c-240">Resurser i Azure kan övervakas genom att samla in hello statistik som prestanda, tillgänglighet och användningsinformation med hello hjälp av Azure-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="8f92c-240">Resources in Azure can be monitored by collecting hello statistics like performance, availability and usage with hello help of Azure alerts.</span></span> <span data-ttu-id="8f92c-241">Du kan ta emot en avisering baserat på övervakning mått eller händelser för din Azure-resurser, för närvarande Automation-konton stöder endast mått.</span><span class="sxs-lookup"><span data-stu-id="8f92c-241">You can receive an alert based on monitoring metrics or events for your Azure resources, currently Automation Accounts support only metrics.</span></span> <span data-ttu-id="8f92c-242">När hello-värdet för ett visst mått överskrider hello tröskelvärdet tilldelade eller om hello konfigurerats händelsen utlöses och sedan skickas ett meddelande toohello administratören eller medadministratörer tooresolve hello varning service, mer information om mått och händelser finns för[ Azure-aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="8f92c-242">When hello value of a specified metric exceeds hello threshold assigned or if hello configured event is triggered then a notification is sent toohello service admin or co-admins tooresolve hello alert, for more information on metrics and events please refer too[Azure alerts](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="8f92c-243">Förutom att använda Azure-aviseringar som ett system, kan du också startar runbooks i svaret tooalerts.</span><span class="sxs-lookup"><span data-stu-id="8f92c-243">Besides using Azure alerts as a notification system, you can also kick off runbooks in response tooalerts.</span></span> <span data-ttu-id="8f92c-244">Azure Automation ger hello kapaciteten toorun webhook-aktiverade runbooks med Azure-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="8f92c-244">Azure Automation provides hello capability toorun webhook-enabled runbooks with Azure alerts.</span></span> <span data-ttu-id="8f92c-245">När ett mått överstiger konfigurerad hello tröskelvärdet hello varningsregeln blir aktiv och utlösare hello automation-webhook som i sin tur utför hello runbook.</span><span class="sxs-lookup"><span data-stu-id="8f92c-245">When a metric exceeds hello configured threshold value then hello alert rule becomes active and triggers hello automation webhook which in turn executes hello runbook.</span></span>

![Webhooks](media/automation-webhooks/webhook-alert.jpg)

### <a name="alert-context"></a><span data-ttu-id="8f92c-247">Varningskontext</span><span class="sxs-lookup"><span data-stu-id="8f92c-247">Alert context</span></span>
<span data-ttu-id="8f92c-248">Överväg att en Azure-resurs, till exempel en virtuell dator, CPU-användning för den här datorn är en hello KPI mått.</span><span class="sxs-lookup"><span data-stu-id="8f92c-248">Consider an Azure resource such as a virtual machine, CPU utilization of this machine is one of hello key performance metric.</span></span> <span data-ttu-id="8f92c-249">Om hello CPU-användning 100% eller mer än en viss mängd för lång tidsperiod, kanske du vill toorestart hello virtuella toofix hello problem.</span><span class="sxs-lookup"><span data-stu-id="8f92c-249">If hello CPU utilization is 100% or more than a certain amount for long period of time, you might want toorestart hello virtual machine toofix hello problem.</span></span> <span data-ttu-id="8f92c-250">Detta kan lösas genom att konfigurera en regel för varning toohello virtuell dator och den här regeln tar CPU-procent som dess mått.</span><span class="sxs-lookup"><span data-stu-id="8f92c-250">This can be solved by configuring an alert rule toohello virtual machine and this rule takes CPU percentage as its metric.</span></span> <span data-ttu-id="8f92c-251">CPU-procent här tas bara ett exempel men det finns många andra mått som du kan konfigurera tooyour Azure resurser och startar om hello virtuella datorn är en åtgärd som har vidtagits toofix det här problemet kan du konfigurera hello runbook tootake andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="8f92c-251">CPU percentage here is just taken as an example but there are many other metrics that you can configure tooyour Azure resources and restarting hello virtual machine is an action that is taken toofix this issue, you can configure hello runbook tootake other actions.</span></span>

<span data-ttu-id="8f92c-252">När regeln hello avisering aktiveras och utlösare hello webhook-aktiverade runbook, skickar hello aviseringskontext toohello runbook.</span><span class="sxs-lookup"><span data-stu-id="8f92c-252">When this hello alert rule becomes active and triggers hello webhook-enabled runbook, it sends hello alert context toohello runbook.</span></span> <span data-ttu-id="8f92c-253">[Aviseringskontext](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) innehåller information, inklusive **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** och **tidsstämpel** som krävs för hello runbook tooidentify hello resursen som den ska vidta åtgärder.</span><span class="sxs-lookup"><span data-stu-id="8f92c-253">[Alert context](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) contains details including **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** and **Timestamp** which are required for hello runbook tooidentify hello resource on which it will be taking action.</span></span> <span data-ttu-id="8f92c-254">Varna kontext är inbäddad i hello huvuddelen av hello **WebhookData** objektet skickade toohello runbook och den kan användas med **Webhook.RequestBody** egenskapen</span><span class="sxs-lookup"><span data-stu-id="8f92c-254">Alert context is embedded in hello body part of hello **WebhookData** object sent toohello runbook and it can be accessed with **Webhook.RequestBody** property</span></span>

### <a name="example"></a><span data-ttu-id="8f92c-255">Exempel</span><span class="sxs-lookup"><span data-stu-id="8f92c-255">Example</span></span>
<span data-ttu-id="8f92c-256">Skapa en virtuell Azure-dator i din prenumeration och koppla en [Varna toomonitor CPU-procent mått](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="8f92c-256">Create an Azure virtual machine in your subscription and associate an [alert toomonitor CPU percentage metric](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span> <span data-ttu-id="8f92c-257">Kontrollera att du fylla i fältet för hello webhook med hello URL hello webhook som skapades när du skapar hello webhook när du skapar hello avisering.</span><span class="sxs-lookup"><span data-stu-id="8f92c-257">While creating hello alert make sure you populate hello webhook field with hello URL of hello webhook which was generated while creating hello webhook.</span></span>

<span data-ttu-id="8f92c-258">hello utlöses följande exempel-runbook när hello varningsregeln blir aktiv och samlar det in hello aviseringskontext parametrar som krävs för hello runbook tooidentify hello resursen som den ska vidta åtgärder.</span><span class="sxs-lookup"><span data-stu-id="8f92c-258">hello following sample runbook is triggered when hello alert rule becomes active and it collects hello Alert context parameters which are required for hello runbook tooidentify hello resource on which it will be taking action.</span></span>

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

            # Outputs information on hello webhook name that called This
            Write-Output "This runbook was started from webhook $WebhookName."


            # Obtain hello WebhookBody containing hello AlertContext
            $WebhookBody = (ConvertFrom-Json -InputObject $WebhookBody)
            Write-Output "`nWEBHOOK BODY"
            Write-Output "============="
            Write-Output $WebhookBody

            # Obtain hello AlertContext     
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

            # Act on hello AlertContext data, in our case restarting hello VM.
            # Authenticate tooyour Azure subscription using Organization ID toobe able toorestart that Virtual Machine.
            $cred = Get-AutomationPSCredential -Name "MyAzureCredential"
            Add-AzureAccount -Credential $cred
            Select-AzureSubscription -subscriptionName "Visual Studio Ultimate with MSDN"

            #Check hello status property of hello VM
            Write-Output "Status of VM before taking action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
            Write-Output "Restarting VM"

            # Restart hello VM by passing VM name and Service name which are same in this case
            Restart-AzureVM -ServiceName $AlertContext.resourceName -Name $AlertContext.resourceName
            Write-Output "Status of VM after alert is active and takes action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
        }
        else  
        {
            Write-Error "This runbook is meant tooonly be started from a webhook."  
        }  
    }



## <a name="next-steps"></a><span data-ttu-id="8f92c-259">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f92c-259">Next steps</span></span>
* <span data-ttu-id="8f92c-260">Mer information om olika sätt toostart en runbook finns [starta en Runbook](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="8f92c-260">For details on different ways toostart a runbook, see [Starting a Runbook](automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="8f92c-261">Information om visning hello Status för en Runbook-jobb finns för[Runbook som körs i Azure Automation](automation-runbook-execution.md).</span><span class="sxs-lookup"><span data-stu-id="8f92c-261">For information on viewing hello Status of a Runbook Job, refer too[Runbook execution in Azure Automation](automation-runbook-execution.md).</span></span>
* <span data-ttu-id="8f92c-262">hur toouse Azure Automation tootake åtgärd på Azure aviseringar, se toolearn [åtgärda Azure VM aviseringar med Automation-Runbooks](automation-azure-vm-alert-integration.md).</span><span class="sxs-lookup"><span data-stu-id="8f92c-262">toolearn how toouse Azure Automation tootake action on Azure Alerts, see [Remediate Azure VM Alerts with Automation Runbooks](automation-azure-vm-alert-integration.md).</span></span>
