---
title: "aaaAzure händelse rutnätet säkerhet och autentisering"
description: "Beskriver Azure händelse rutnätet och dess begrepp."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/14/2017
ms.author: babanisa
ms.openlocfilehash: 74f692c7e3a30856c3a80cbbfe82a26bf4c1c9b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-security-and-authentication"></a><span data-ttu-id="27b64-103">Händelsen rutnätet säkerhet och autentisering</span><span class="sxs-lookup"><span data-stu-id="27b64-103">Event Grid security and authentication</span></span> 

<span data-ttu-id="27b64-104">Azure händelse rutnätet har tre typer av autentisering:</span><span class="sxs-lookup"><span data-stu-id="27b64-104">Azure Event Grid has three types of authentication:</span></span>

* <span data-ttu-id="27b64-105">Prenumerationer på händelser</span><span class="sxs-lookup"><span data-stu-id="27b64-105">Event subscriptions</span></span>
* <span data-ttu-id="27b64-106">Händelse-publicering</span><span class="sxs-lookup"><span data-stu-id="27b64-106">Event publishing</span></span>
* <span data-ttu-id="27b64-107">WebHook händelse leverans</span><span class="sxs-lookup"><span data-stu-id="27b64-107">WebHook event delivery</span></span>

## <a name="webhook-event-delivery"></a><span data-ttu-id="27b64-108">WebHook-händelse leverans</span><span class="sxs-lookup"><span data-stu-id="27b64-108">WebHook Event delivery</span></span>

<span data-ttu-id="27b64-109">Webhooks är en av många olika sätt tooreceive händelser i realtid från Azure Event rutnät.</span><span class="sxs-lookup"><span data-stu-id="27b64-109">Webhooks are one of many ways tooreceive events in real time from Azure Event Grid.</span></span>

<span data-ttu-id="27b64-110">Varje gång det finns nya redo händelse-toobe levereras, skickar händelse rutnätet en HTTP-begäran med tooyour WebHook med hello händelse i hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="27b64-110">Every time there is a new event ready toobe delivered, Event Grid sends an HTTP request with tooyour WebHook with hello event in hello body.</span></span>

<span data-ttu-id="27b64-111">När du registrerar WebHook slutpunkten med händelsen rutnät skickar den en POST-begäran med en enkel verifiering kod i ordning tooprove endpoint ägarskap.</span><span class="sxs-lookup"><span data-stu-id="27b64-111">When you register your own WebHook endpoint with Event Grid, it sends you a POST request with a simple validation code in order tooprove endpoint ownership.</span></span> <span data-ttu-id="27b64-112">Appen måste toorespond av eko tillbaka hello verifieringskoden.</span><span class="sxs-lookup"><span data-stu-id="27b64-112">Your app needs toorespond by echoing back hello validation code.</span></span> <span data-ttu-id="27b64-113">Händelsen rutnätet Skicka inte händelser tooWebHook slutpunkter som inte har klarat hello validering.</span><span class="sxs-lookup"><span data-stu-id="27b64-113">Event Grid will not deliver events tooWebHook endpoints that have not passed hello validation.</span></span>
 
### <a name="validation-details"></a><span data-ttu-id="27b64-114">Information om verifiering:</span><span class="sxs-lookup"><span data-stu-id="27b64-114">Validation details:</span></span>

* <span data-ttu-id="27b64-115">När hello händelse prenumeration skapa/uppdatera bokför händelse rutnätet en ”SubscriptionValidationEvent” händelse toohello mål slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="27b64-115">At hello time of event subscription creation/update, Event Grid posts a “SubscriptionValidationEvent” event toohello target endpoint.</span></span>
* <span data-ttu-id="27b64-116">hello innehåller ett huvudvärde ”-Händelsetyp: verifiering”.</span><span class="sxs-lookup"><span data-stu-id="27b64-116">hello event contains a header value “Event-Type: Validation”.</span></span>
* <span data-ttu-id="27b64-117">hello händelsemeddelandet har hello samma schema som andra händelser med händelse rutnätet.</span><span class="sxs-lookup"><span data-stu-id="27b64-117">hello event body has hello same schema as other Event Grid events.</span></span>
* <span data-ttu-id="27b64-118">hello händelsedata innehåller egenskapen ”ValidationCode” med en slumpmässigt genererad sträng t.ex. ”ValidationCode: acb13...”.</span><span class="sxs-lookup"><span data-stu-id="27b64-118">hello event data includes a “ValidationCode” property with a randomly generated string e.g. “ValidationCode: acb13…”.</span></span>

<span data-ttu-id="27b64-119">I ordning tooprove endpoint ägarskap echo tillbaka hello validering koden t.ex ”. ValidationResponse: acb13...”.</span><span class="sxs-lookup"><span data-stu-id="27b64-119">In order tooprove endpoint ownership, echo back hello validation code e.g “ValidationResponse: acb13…”.</span></span>

<span data-ttu-id="27b64-120">Slutligen är det viktigt toonote att Azure händelse rutnätet endast stöder HTTPS webhook-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="27b64-120">Finally, it is important toonote that Azure Event Grid only supports HTTPS webhook endpoints.</span></span>
## <a name="event-subscription"></a><span data-ttu-id="27b64-121">Händelseprenumerationen</span><span class="sxs-lookup"><span data-stu-id="27b64-121">Event subscription</span></span>

<span data-ttu-id="27b64-122">toosubscribe tooan händelse, måste du ha hello **Microsoft.EventGrid/EventSubscriptions/Write** behörighet på hello krävs resurs.</span><span class="sxs-lookup"><span data-stu-id="27b64-122">toosubscribe tooan event, you must have hello **Microsoft.EventGrid/EventSubscriptions/Write** permission on hello required resource.</span></span> <span data-ttu-id="27b64-123">Du behöver den här behörigheten eftersom du skriver en ny prenumeration på hello omfattningen av hello resurs.</span><span class="sxs-lookup"><span data-stu-id="27b64-123">You need this permission because you are writing a new subscription at hello scope of hello resource.</span></span> <span data-ttu-id="27b64-124">hello krävs resurs skiljer sig åt beroende på om du prenumerera tooa Systemavsnittet eller anpassade avsnittet.</span><span class="sxs-lookup"><span data-stu-id="27b64-124">hello required resource differs based on whether you are subscribing tooa system topic or custom topic.</span></span> <span data-ttu-id="27b64-125">Båda typerna beskrivs i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="27b64-125">Both types are described in this section.</span></span>

### <a name="system-topics-azure-service-publishers"></a><span data-ttu-id="27b64-126">System avsnitt (utgivare Azure-tjänst)</span><span class="sxs-lookup"><span data-stu-id="27b64-126">System topics (Azure service publishers)</span></span>

<span data-ttu-id="27b64-127">Du måste behörighet toowrite en ny händelseprenumeration på hello omfattningen av hello resurs publishing hello händelse för system-avsnitt.</span><span class="sxs-lookup"><span data-stu-id="27b64-127">For system topics, you need permission toowrite a new event subscription at hello scope of hello resource publishing hello event.</span></span> <span data-ttu-id="27b64-128">hello resurs hello format är:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`</span><span class="sxs-lookup"><span data-stu-id="27b64-128">hello format of hello resource is: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`</span></span>

<span data-ttu-id="27b64-129">Till exempel toosubscribe tooan händelse på ett lagringskonto med namnet **MITTKONTO**, du behöver behörighet att hello Microsoft.EventGrid/EventSubscriptions/Write på:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`</span><span class="sxs-lookup"><span data-stu-id="27b64-129">For example, toosubscribe tooan event on a storage account named **myacct**, you need hello Microsoft.EventGrid/EventSubscriptions/Write permission on: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`</span></span>

### <a name="custom-topics"></a><span data-ttu-id="27b64-130">Anpassade avsnitt</span><span class="sxs-lookup"><span data-stu-id="27b64-130">Custom topics</span></span>

<span data-ttu-id="27b64-131">Du måste behörighet toowrite en ny händelseprenumeration i hello omfattningen av hello händelse rutnätet avsnittet för anpassade avsnitt.</span><span class="sxs-lookup"><span data-stu-id="27b64-131">For custom topics, you need permission toowrite a new event subscription at hello scope of hello Event Grid topic.</span></span> <span data-ttu-id="27b64-132">hello resurs hello format är:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`</span><span class="sxs-lookup"><span data-stu-id="27b64-132">hello format of hello resource is: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`</span></span>

<span data-ttu-id="27b64-133">Till exempel toosubscribe tooa anpassade ämne med namnet **mytopic**, du behöver behörighet att hello Microsoft.EventGrid/EventSubscriptions/Write på:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`</span><span class="sxs-lookup"><span data-stu-id="27b64-133">For example, toosubscribe tooa custom topic named **mytopic**, you need hello Microsoft.EventGrid/EventSubscriptions/Write permission on: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`</span></span>

## <a name="topic-publishing"></a><span data-ttu-id="27b64-134">Avsnittet publicering</span><span class="sxs-lookup"><span data-stu-id="27b64-134">Topic publishing</span></span>

<span data-ttu-id="27b64-135">Avsnitt om använda delade signatur åtkomst (SAS) eller nyckelautentisering.</span><span class="sxs-lookup"><span data-stu-id="27b64-135">Topics use either Shared Access Signature (SAS) or key authentication.</span></span> <span data-ttu-id="27b64-136">Vi rekommenderar SAS, men nyckelautentisering ger enkel programmering och är kompatibel med många befintliga webhook-utgivare.</span><span class="sxs-lookup"><span data-stu-id="27b64-136">We recommend SAS, but key authentication provides simple programming, and is compatible with many existing webhook publishers.</span></span> 

<span data-ttu-id="27b64-137">Du inkluderar hello autentisering värde i hello HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="27b64-137">You include hello authentication value in hello HTTP header.</span></span> <span data-ttu-id="27b64-138">Använd för SAS, **aeg-sas-token** för hello huvudvärde.</span><span class="sxs-lookup"><span data-stu-id="27b64-138">For SAS, use **aeg-sas-token** for hello header value.</span></span> <span data-ttu-id="27b64-139">Nyckelautentisering, Använd **aeg-sas-nyckeln** för hello huvudvärde.</span><span class="sxs-lookup"><span data-stu-id="27b64-139">For key authentication, use **aeg-sas-key** for hello header value.</span></span>

### <a name="key-authentication"></a><span data-ttu-id="27b64-140">Autentisering med nyckel</span><span class="sxs-lookup"><span data-stu-id="27b64-140">Key authentication</span></span>

<span data-ttu-id="27b64-141">Nyckelautentisering är hello enklaste formen av autentisering.</span><span class="sxs-lookup"><span data-stu-id="27b64-141">Key authentication is hello simplest form of authentication.</span></span> <span data-ttu-id="27b64-142">Använd hello format:`aeg-sas-key: <your key>`</span><span class="sxs-lookup"><span data-stu-id="27b64-142">Use hello format: `aeg-sas-key: <your key>`</span></span>

<span data-ttu-id="27b64-143">Exempelvis kan du ange en nyckel med:</span><span class="sxs-lookup"><span data-stu-id="27b64-143">For example, you pass a key with:</span></span> 

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a><span data-ttu-id="27b64-144">SAS-token</span><span class="sxs-lookup"><span data-stu-id="27b64-144">SAS tokens</span></span>

<span data-ttu-id="27b64-145">SAS-token för händelsen rutnätet innehåller hello resurs, en förfallotid och en signatur.</span><span class="sxs-lookup"><span data-stu-id="27b64-145">SAS tokens for Event Grid include hello resource, an expiration time, and a signature.</span></span> <span data-ttu-id="27b64-146">hello hello SAS-token har formatet: `r={resource}&e={expiration}&s={signature}`.</span><span class="sxs-lookup"><span data-stu-id="27b64-146">hello format of hello SAS token is: `r={resource}&e={expiration}&s={signature}`.</span></span>

<span data-ttu-id="27b64-147">hello resursen är hello sökväg för hello avsnittet toowhich du skickar händelser.</span><span class="sxs-lookup"><span data-stu-id="27b64-147">hello resource is hello path for hello topic toowhich you are sending events.</span></span> <span data-ttu-id="27b64-148">Till exempel är en giltig resurs-sökväg:`https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`</span><span class="sxs-lookup"><span data-stu-id="27b64-148">For example, a valid resource path is: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`</span></span>

<span data-ttu-id="27b64-149">Du kan generera hello signatur från en nyckel.</span><span class="sxs-lookup"><span data-stu-id="27b64-149">You generate hello signature from a key.</span></span>

<span data-ttu-id="27b64-150">Till exempel en giltig **aeg-sas-token** värde är:</span><span class="sxs-lookup"><span data-stu-id="27b64-150">For example, a valid **aeg-sas-token** value is:</span></span>

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
``` 

<span data-ttu-id="27b64-151">hello följande exempel skapas en SAS-token för användning med händelsen rutnätet:</span><span class="sxs-lookup"><span data-stu-id="27b64-151">hello following example creates a SAS token for use with Event Grid:</span></span>

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    string encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString());

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a><span data-ttu-id="27b64-152">Åtkomstkontroll för hantering</span><span class="sxs-lookup"><span data-stu-id="27b64-152">Management Access Control</span></span>

<span data-ttu-id="27b64-153">Azure händelse rutnätet kan du toocontrol hello andelen åtkomst ges toodifferent användare toodo olika hanteringsåtgärder, till exempel listan händelseprenumerationer, skapa nya och generera nycklar.</span><span class="sxs-lookup"><span data-stu-id="27b64-153">Azure Event Grid allows you toocontrol hello level of access given toodifferent users toodo various management operations such as list event subscriptions, create new ones, and generate keys.</span></span> <span data-ttu-id="27b64-154">Händelsen rutnätet använder Azures rollbaserad åtkomst Kontrollera (RBAC) för den här.</span><span class="sxs-lookup"><span data-stu-id="27b64-154">Event Grid utilizes Azure's Role Based Access Check (RBAC) for this.</span></span>

### <a name="operation-types"></a><span data-ttu-id="27b64-155">Åtgärdstyper</span><span class="sxs-lookup"><span data-stu-id="27b64-155">Operation types</span></span>

<span data-ttu-id="27b64-156">Azure händelse rutnätet stöder hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="27b64-156">Azure event grid supports hello following actions:</span></span>

* <span data-ttu-id="27b64-157">Microsoft.EventGrid/*/read</span><span class="sxs-lookup"><span data-stu-id="27b64-157">Microsoft.EventGrid/*/read</span></span> 
* <span data-ttu-id="27b64-158">Microsoft.EventGrid/*/write</span><span class="sxs-lookup"><span data-stu-id="27b64-158">Microsoft.EventGrid/*/write</span></span> 
* <span data-ttu-id="27b64-159">Microsoft.EventGrid/*/delete</span><span class="sxs-lookup"><span data-stu-id="27b64-159">Microsoft.EventGrid/*/delete</span></span> 
* <span data-ttu-id="27b64-160">Microsoft.EventGrid/eventSubscriptions/getFullUrl/action</span><span class="sxs-lookup"><span data-stu-id="27b64-160">Microsoft.EventGrid/eventSubscriptions/getFullUrl/action</span></span> 
* <span data-ttu-id="27b64-161">Microsoft.EventGrid/topics/listKeys/action</span><span class="sxs-lookup"><span data-stu-id="27b64-161">Microsoft.EventGrid/topics/listKeys/action</span></span> 
* <span data-ttu-id="27b64-162">Microsoft.EventGrid/topics/regenerateKey/action</span><span class="sxs-lookup"><span data-stu-id="27b64-162">Microsoft.EventGrid/topics/regenerateKey/action</span></span>

<span data-ttu-id="27b64-163">Hej sista tre åtgärder returnerar potentiellt hemlig information som hämtar filtrerat utanför normal läsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="27b64-163">hello last three operations return potentially secret information which gets filtered out of normal read operations.</span></span> <span data-ttu-id="27b64-164">Det är bäst för dig toorestrict åtkomståtgärder toothese.</span><span class="sxs-lookup"><span data-stu-id="27b64-164">It is best practice for you toorestrict access toothese operations.</span></span> <span data-ttu-id="27b64-165">Anpassade roller kan skapas med [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [Azure-kommandoradsgränssnittet (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md), och hello [REST API](../active-directory/role-based-access-control-manage-access-rest.md).</span><span class="sxs-lookup"><span data-stu-id="27b64-165">Custom roles can be created using [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [Azure Command-Line Interface (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md), and hello [REST API](../active-directory/role-based-access-control-manage-access-rest.md).</span></span>

### <a name="enforcing-role-based-access-check-rbac"></a><span data-ttu-id="27b64-166">Framtvinga roll baserad åtkomstkontroll (RBAC)</span><span class="sxs-lookup"><span data-stu-id="27b64-166">Enforcing Role Based Access Check (RBAC)</span></span>

<span data-ttu-id="27b64-167">Använd följande steg tooenforce RBAC för olika användare hello:</span><span class="sxs-lookup"><span data-stu-id="27b64-167">Use hello following steps tooenforce RBAC for different users:</span></span>

#### <a name="create-a-custom-role-definition-file-json"></a><span data-ttu-id="27b64-168">Skapa en anpassad roll definitionsfil (JSON)</span><span class="sxs-lookup"><span data-stu-id="27b64-168">Create a custom role definition file (.json)</span></span>

<span data-ttu-id="27b64-169">hello följande är exempel händelse rutnätet rolldefinitioner där användarna tooperform olika uppsättningar av åtgärder.</span><span class="sxs-lookup"><span data-stu-id="27b64-169">hello following are sample Event Grid role definitions which allow users tooperform different sets of actions.</span></span>

<span data-ttu-id="27b64-170">**EventGridReadOnlyRole.json**: Tillåt endast endast läsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="27b64-170">**EventGridReadOnlyRole.json**: Only allow read only operations.</span></span>
```json
{ 
  "Name": "Event grid read only role", 
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856", 
  "IsCustom": true, 
  "Description": "Event grid read only role", 
  "Actions": [ 
    "Microsoft.EventGrid/*/read" 
  ], 
  "NotActions": [ 
  ], 
  "AssignableScopes": [ 
    "/subscriptions/<Subscription Id>" 
  ] 
}
```

<span data-ttu-id="27b64-171">**EventGridNoDeleteListKeysRole.json**: Tillåt åtgärder begränsade efter Tillåt men inte ta bort åtgärder.</span><span class="sxs-lookup"><span data-stu-id="27b64-171">**EventGridNoDeleteListKeysRole.json**: Allow restricted post actions but disallow delete actions.</span></span>
```json
{ 
  "Name": "Event grid No Delete Listkeys role", 
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174", 
  "IsCustom": true, 
  "Description": "Event grid No Delete Listkeys role", 
  "Actions": [     
    "Microsoft.EventGrid/*/write", 
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action" 
    "Microsoft.EventGrid/topics/listkeys/action", 
    "Microsoft.EventGrid/topics/regenerateKey/action" 
  ], 
  "NotActions": [ 
    "Microsoft.EventGrid/*/delete" 
  ], 
  "AssignableScopes": [ 
    "/subscriptions/<Subscription id>" 
  ] 
}
```
 
<span data-ttu-id="27b64-172">**EventGridContributorRole.json**: tillåter alla händelse rutnätet åtgärder.</span><span class="sxs-lookup"><span data-stu-id="27b64-172">**EventGridContributorRole.json**: Allows all event grid actions.</span></span>  
```json
{ 
  "Name": "Event grid contributor role", 
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514", 
  "IsCustom": true, 
  "Description": "Event grid contributor role", 
  "Actions": [ 
    "Microsoft.EventGrid/*/write", 
    "Microsoft.EventGrid/*/delete", 
    "Microsoft.EventGrid/topics/listkeys/action", 
    "Microsoft.EventGrid/topics/regenerateKey/action", 
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action" 
  ], 
  "NotActions": [], 
  "AssignableScopes": [ 
    "/subscriptions/d48566a8-2428-4a6c-8347-9675d09fb851" 
  ] 
} 
```

#### <a name="install-and-login-tooazure-cli"></a><span data-ttu-id="27b64-173">Installera och logga in tooAzure CLI</span><span class="sxs-lookup"><span data-stu-id="27b64-173">Install and login tooAzure CLI</span></span>

* <span data-ttu-id="27b64-174">Azure CLI version 0.8.8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="27b64-174">Azure CLI version 0.8.8 or later.</span></span> <span data-ttu-id="27b64-175">tooinstall hello senaste versionen och koppla den med din Azure-prenumeration finns [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="27b64-175">tooinstall hello latest version and associate it with your Azure subscription, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="27b64-176">Azure Resource Manager i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="27b64-176">Azure Resource Manager in Azure CLI.</span></span> <span data-ttu-id="27b64-177">Gå för[Using hello Azure CLI med hello Resource Manager](../xplat-cli-azure-resource-manager.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="27b64-177">Go too[Using hello Azure CLI with hello Resource Manager](../xplat-cli-azure-resource-manager.md) for more details.</span></span>

#### <a name="create-a-custom-role"></a><span data-ttu-id="27b64-178">Skapa en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="27b64-178">Create a custom role</span></span>

<span data-ttu-id="27b64-179">Använd toocreate en anpassad roll:</span><span class="sxs-lookup"><span data-stu-id="27b64-179">toocreate a custom role, use:</span></span>

    azure role create --inputfile <file path>

#### <a name="assign-hello-role-tooa-user"></a><span data-ttu-id="27b64-180">Tilldela hello rollen tooa användare</span><span class="sxs-lookup"><span data-stu-id="27b64-180">Assign hello role tooa user</span></span>


    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>


## <a name="next-steps"></a><span data-ttu-id="27b64-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="27b64-181">Next steps</span></span>

* <span data-ttu-id="27b64-182">En introduktion tooEvent rutnätet finns [om händelsen rutnätet](overview.md)</span><span class="sxs-lookup"><span data-stu-id="27b64-182">For an introduction tooEvent Grid, see [About Event Grid](overview.md)</span></span>
