---
title: "Azure händelse rutnätet säkerhet och autentisering"
description: "Beskriver Azure händelse rutnätet och dess begrepp."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/14/2017
ms.author: babanisa
ms.openlocfilehash: b6e1c7587c0b47d04862b4850741aaa3b7d191a8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="event-grid-security-and-authentication"></a><span data-ttu-id="01969-103">Händelsen rutnätet säkerhet och autentisering</span><span class="sxs-lookup"><span data-stu-id="01969-103">Event Grid security and authentication</span></span> 

<span data-ttu-id="01969-104">Azure händelse rutnätet har tre typer av autentisering:</span><span class="sxs-lookup"><span data-stu-id="01969-104">Azure Event Grid has three types of authentication:</span></span>

* <span data-ttu-id="01969-105">Prenumerationer på händelser</span><span class="sxs-lookup"><span data-stu-id="01969-105">Event subscriptions</span></span>
* <span data-ttu-id="01969-106">Händelse-publicering</span><span class="sxs-lookup"><span data-stu-id="01969-106">Event publishing</span></span>
* <span data-ttu-id="01969-107">WebHook händelse leverans</span><span class="sxs-lookup"><span data-stu-id="01969-107">WebHook event delivery</span></span>

## <a name="webhook-event-delivery"></a><span data-ttu-id="01969-108">WebHook-händelse leverans</span><span class="sxs-lookup"><span data-stu-id="01969-108">WebHook Event delivery</span></span>

<span data-ttu-id="01969-109">Webhooks är en av många olika sätt att ta emot händelser i realtid från Azure Event rutnät.</span><span class="sxs-lookup"><span data-stu-id="01969-109">Webhooks are one of many ways to receive events in real time from Azure Event Grid.</span></span>

<span data-ttu-id="01969-110">Varje gång det finns en ny händelse som är redo att levereras, skickar händelse rutnätet en HTTP-begäran med till din WebHook med händelsen i brödtexten.</span><span class="sxs-lookup"><span data-stu-id="01969-110">Every time there is a new event ready to be delivered, Event Grid sends an HTTP request with to your WebHook with the event in the body.</span></span>

<span data-ttu-id="01969-111">När du registrerar WebHook slutpunkten med händelsen rutnät skickar den en POST-begäran med en enkel verifiering kod för att bevisa att du äger för slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="01969-111">When you register your own WebHook endpoint with Event Grid, it sends you a POST request with a simple validation code in order to prove endpoint ownership.</span></span> <span data-ttu-id="01969-112">Din app behöver svara genom eko tillbaka verifieringskoden.</span><span class="sxs-lookup"><span data-stu-id="01969-112">Your app needs to respond by echoing back the validation code.</span></span> <span data-ttu-id="01969-113">Händelsen rutnätet Skicka inte händelser till WebHook-slutpunkter som inte har klarat valideringen.</span><span class="sxs-lookup"><span data-stu-id="01969-113">Event Grid will not deliver events to WebHook endpoints that have not passed the validation.</span></span>
 
### <a name="validation-details"></a><span data-ttu-id="01969-114">Information om verifiering:</span><span class="sxs-lookup"><span data-stu-id="01969-114">Validation details:</span></span>

* <span data-ttu-id="01969-115">Vid tidpunkten för händelsen prenumeration skapa/uppdatera bokför händelse rutnätet en ”SubscriptionValidationEvent”-händelse till mål-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="01969-115">At the time of event subscription creation/update, Event Grid posts a “SubscriptionValidationEvent” event to the target endpoint.</span></span>
* <span data-ttu-id="01969-116">Innehåller ett huvudvärde ”-Händelsetyp: verifiering”.</span><span class="sxs-lookup"><span data-stu-id="01969-116">The event contains a header value “Event-Type: Validation”.</span></span>
* <span data-ttu-id="01969-117">Händelsemeddelandet innehåller samma schema som andra händelser med händelse rutnätet.</span><span class="sxs-lookup"><span data-stu-id="01969-117">The event body has the same schema as other Event Grid events.</span></span>
* <span data-ttu-id="01969-118">Händelsedata som innehåller egenskapen ”ValidationCode” med en slumpmässigt genererad sträng t.ex. ”ValidationCode: acb13...”.</span><span class="sxs-lookup"><span data-stu-id="01969-118">The event data includes a “ValidationCode” property with a randomly generated string e.g. “ValidationCode: acb13…”.</span></span>

<span data-ttu-id="01969-119">För att bevisa endpoint ägarskap echo tillbaka validering koden t.ex ”. ValidationResponse: acb13...”.</span><span class="sxs-lookup"><span data-stu-id="01969-119">In order to prove endpoint ownership, echo back the validation code e.g “ValidationResponse: acb13…”.</span></span>

<span data-ttu-id="01969-120">Slutligen är det viktigt att Observera att Azure händelse rutnätet endast stöder HTTPS webhook-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="01969-120">Finally, it is important to note that Azure Event Grid only supports HTTPS webhook endpoints.</span></span>
## <a name="event-subscription"></a><span data-ttu-id="01969-121">Händelseprenumerationen</span><span class="sxs-lookup"><span data-stu-id="01969-121">Event subscription</span></span>

<span data-ttu-id="01969-122">Om du vill prenumerera på en händelse, måste du ha den **Microsoft.EventGrid/EventSubscriptions/Write** behörighet för den begärda resursen.</span><span class="sxs-lookup"><span data-stu-id="01969-122">To subscribe to an event, you must have the **Microsoft.EventGrid/EventSubscriptions/Write** permission on the required resource.</span></span> <span data-ttu-id="01969-123">Du behöver den här behörigheten eftersom du skriver en ny prenumeration på omfattningen av resursen.</span><span class="sxs-lookup"><span data-stu-id="01969-123">You need this permission because you are writing a new subscription at the scope of the resource.</span></span> <span data-ttu-id="01969-124">Den begärda resursen skiljer sig åt beroende på om du prenumerera på ett Systemavsnittet eller anpassade avsnittet.</span><span class="sxs-lookup"><span data-stu-id="01969-124">The required resource differs based on whether you are subscribing to a system topic or custom topic.</span></span> <span data-ttu-id="01969-125">Båda typerna beskrivs i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="01969-125">Both types are described in this section.</span></span>

### <a name="system-topics-azure-service-publishers"></a><span data-ttu-id="01969-126">System avsnitt (utgivare Azure-tjänst)</span><span class="sxs-lookup"><span data-stu-id="01969-126">System topics (Azure service publishers)</span></span>

<span data-ttu-id="01969-127">För system ämnen behöver du behörighet att skriva en ny händelseprenumeration på omfattningen av resurspublicerings händelsen.</span><span class="sxs-lookup"><span data-stu-id="01969-127">For system topics, you need permission to write a new event subscription at the scope of the resource publishing the event.</span></span> <span data-ttu-id="01969-128">Formatet för resursen är:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`</span><span class="sxs-lookup"><span data-stu-id="01969-128">The format of the resource is: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`</span></span>

<span data-ttu-id="01969-129">Till exempel att prenumerera på en händelse på ett lagringskonto med namnet **MITTKONTO**, du måste ha behörighet för Microsoft.EventGrid/EventSubscriptions/Write på:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`</span><span class="sxs-lookup"><span data-stu-id="01969-129">For example, to subscribe to an event on a storage account named **myacct**, you need the Microsoft.EventGrid/EventSubscriptions/Write permission on: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`</span></span>

### <a name="custom-topics"></a><span data-ttu-id="01969-130">Anpassade avsnitt</span><span class="sxs-lookup"><span data-stu-id="01969-130">Custom topics</span></span>

<span data-ttu-id="01969-131">För anpassade avsnitt behöver behörighet att skriva en ny händelseprenumeration på omfattningen av händelse rutnätet ämnet.</span><span class="sxs-lookup"><span data-stu-id="01969-131">For custom topics, you need permission to write a new event subscription at the scope of the Event Grid topic.</span></span> <span data-ttu-id="01969-132">Formatet för resursen är:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`</span><span class="sxs-lookup"><span data-stu-id="01969-132">The format of the resource is: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`</span></span>

<span data-ttu-id="01969-133">Om du till exempel att prenumerera på en anpassad avsnittet namnet **mytopic**, du måste ha behörighet för Microsoft.EventGrid/EventSubscriptions/Write på:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`</span><span class="sxs-lookup"><span data-stu-id="01969-133">For example, to subscribe to a custom topic named **mytopic**, you need the Microsoft.EventGrid/EventSubscriptions/Write permission on: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`</span></span>

## <a name="topic-publishing"></a><span data-ttu-id="01969-134">Avsnittet publicering</span><span class="sxs-lookup"><span data-stu-id="01969-134">Topic publishing</span></span>

<span data-ttu-id="01969-135">Avsnitt om använda delade signatur åtkomst (SAS) eller nyckelautentisering.</span><span class="sxs-lookup"><span data-stu-id="01969-135">Topics use either Shared Access Signature (SAS) or key authentication.</span></span> <span data-ttu-id="01969-136">Vi rekommenderar SAS, men nyckelautentisering ger enkel programmering och är kompatibel med många befintliga webhook-utgivare.</span><span class="sxs-lookup"><span data-stu-id="01969-136">We recommend SAS, but key authentication provides simple programming, and is compatible with many existing webhook publishers.</span></span> 

<span data-ttu-id="01969-137">Du kan inkludera autentiseringsvärdet för i HTTP-huvudet.</span><span class="sxs-lookup"><span data-stu-id="01969-137">You include the authentication value in the HTTP header.</span></span> <span data-ttu-id="01969-138">Använd för SAS, **aeg-sas-token** för huvudets värde.</span><span class="sxs-lookup"><span data-stu-id="01969-138">For SAS, use **aeg-sas-token** for the header value.</span></span> <span data-ttu-id="01969-139">Nyckelautentisering, Använd **aeg-sas-nyckeln** för huvudets värde.</span><span class="sxs-lookup"><span data-stu-id="01969-139">For key authentication, use **aeg-sas-key** for the header value.</span></span>

### <a name="key-authentication"></a><span data-ttu-id="01969-140">Autentisering med nyckel</span><span class="sxs-lookup"><span data-stu-id="01969-140">Key authentication</span></span>

<span data-ttu-id="01969-141">Nyckelautentisering är den enklaste formen av autentisering.</span><span class="sxs-lookup"><span data-stu-id="01969-141">Key authentication is the simplest form of authentication.</span></span> <span data-ttu-id="01969-142">Använd formatet:`aeg-sas-key: <your key>`</span><span class="sxs-lookup"><span data-stu-id="01969-142">Use the format: `aeg-sas-key: <your key>`</span></span>

<span data-ttu-id="01969-143">Exempelvis kan du ange en nyckel med:</span><span class="sxs-lookup"><span data-stu-id="01969-143">For example, you pass a key with:</span></span> 

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a><span data-ttu-id="01969-144">SAS-token</span><span class="sxs-lookup"><span data-stu-id="01969-144">SAS tokens</span></span>

<span data-ttu-id="01969-145">SAS-token för händelsen rutnätet är resursen, en förfallotid och en signatur.</span><span class="sxs-lookup"><span data-stu-id="01969-145">SAS tokens for Event Grid include the resource, an expiration time, and a signature.</span></span> <span data-ttu-id="01969-146">Formatet på SAS-token är: `r={resource}&e={expiration}&s={signature}`.</span><span class="sxs-lookup"><span data-stu-id="01969-146">The format of the SAS token is: `r={resource}&e={expiration}&s={signature}`.</span></span>

<span data-ttu-id="01969-147">Resursen är sökvägen till avsnittet som du skickar händelser.</span><span class="sxs-lookup"><span data-stu-id="01969-147">The resource is the path for the topic to which you are sending events.</span></span> <span data-ttu-id="01969-148">Till exempel är en giltig resurs-sökväg:`https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`</span><span class="sxs-lookup"><span data-stu-id="01969-148">For example, a valid resource path is: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`</span></span>

<span data-ttu-id="01969-149">Du kan skapa signaturen från en nyckel.</span><span class="sxs-lookup"><span data-stu-id="01969-149">You generate the signature from a key.</span></span>

<span data-ttu-id="01969-150">Till exempel en giltig **aeg-sas-token** värde är:</span><span class="sxs-lookup"><span data-stu-id="01969-150">For example, a valid **aeg-sas-token** value is:</span></span>

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
``` 

<span data-ttu-id="01969-151">I följande exempel skapas en SAS-token för användning med händelsen rutnät:</span><span class="sxs-lookup"><span data-stu-id="01969-151">The following example creates a SAS token for use with Event Grid:</span></span>

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

## <a name="management-access-control"></a><span data-ttu-id="01969-152">Åtkomstkontroll för hantering</span><span class="sxs-lookup"><span data-stu-id="01969-152">Management Access Control</span></span>

<span data-ttu-id="01969-153">Azure händelse rutnätet kan du styra nivån för olika användare att utföra olika hanteringsåtgärder, till exempel listan händelseprenumerationer, skapa nya och generera nycklar.</span><span class="sxs-lookup"><span data-stu-id="01969-153">Azure Event Grid allows you to control the level of access given to different users to do various management operations such as list event subscriptions, create new ones, and generate keys.</span></span> <span data-ttu-id="01969-154">Händelsen rutnätet använder Azures rollbaserad åtkomst Kontrollera (RBAC) för den här.</span><span class="sxs-lookup"><span data-stu-id="01969-154">Event Grid utilizes Azure's Role Based Access Check (RBAC) for this.</span></span>

### <a name="operation-types"></a><span data-ttu-id="01969-155">Åtgärdstyper</span><span class="sxs-lookup"><span data-stu-id="01969-155">Operation types</span></span>

<span data-ttu-id="01969-156">Azure händelse rutnätet har stöd för följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="01969-156">Azure event grid supports the following actions:</span></span>

* <span data-ttu-id="01969-157">Microsoft.EventGrid/*/read</span><span class="sxs-lookup"><span data-stu-id="01969-157">Microsoft.EventGrid/*/read</span></span> 
* <span data-ttu-id="01969-158">Microsoft.EventGrid/*/write</span><span class="sxs-lookup"><span data-stu-id="01969-158">Microsoft.EventGrid/*/write</span></span> 
* <span data-ttu-id="01969-159">Microsoft.EventGrid/*/delete</span><span class="sxs-lookup"><span data-stu-id="01969-159">Microsoft.EventGrid/*/delete</span></span> 
* <span data-ttu-id="01969-160">Microsoft.EventGrid/eventSubscriptions/getFullUrl/action</span><span class="sxs-lookup"><span data-stu-id="01969-160">Microsoft.EventGrid/eventSubscriptions/getFullUrl/action</span></span> 
* <span data-ttu-id="01969-161">Microsoft.EventGrid/topics/listKeys/action</span><span class="sxs-lookup"><span data-stu-id="01969-161">Microsoft.EventGrid/topics/listKeys/action</span></span> 
* <span data-ttu-id="01969-162">Microsoft.EventGrid/topics/regenerateKey/action</span><span class="sxs-lookup"><span data-stu-id="01969-162">Microsoft.EventGrid/topics/regenerateKey/action</span></span>

<span data-ttu-id="01969-163">De tre sista åtgärderna returnera potentiellt hemlig information som hämtar filtrerat utanför normal läsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="01969-163">The last three operations return potentially secret information which gets filtered out of normal read operations.</span></span> <span data-ttu-id="01969-164">Det är bäst för dig att begränsa åtkomsten till dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="01969-164">It is best practice for you to restrict access to these operations.</span></span> <span data-ttu-id="01969-165">Anpassade roller kan skapas med [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [Azure-kommandoradsgränssnittet (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md), och [REST API](../active-directory/role-based-access-control-manage-access-rest.md).</span><span class="sxs-lookup"><span data-stu-id="01969-165">Custom roles can be created using [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [Azure Command-Line Interface (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md), and the [REST API](../active-directory/role-based-access-control-manage-access-rest.md).</span></span>

### <a name="enforcing-role-based-access-check-rbac"></a><span data-ttu-id="01969-166">Framtvinga roll baserad åtkomstkontroll (RBAC)</span><span class="sxs-lookup"><span data-stu-id="01969-166">Enforcing Role Based Access Check (RBAC)</span></span>

<span data-ttu-id="01969-167">Använd följande steg för att genomdriva RBAC för olika användare:</span><span class="sxs-lookup"><span data-stu-id="01969-167">Use the following steps to enforce RBAC for different users:</span></span>

#### <a name="create-a-custom-role-definition-file-json"></a><span data-ttu-id="01969-168">Skapa en anpassad roll definitionsfil (JSON)</span><span class="sxs-lookup"><span data-stu-id="01969-168">Create a custom role definition file (.json)</span></span>

<span data-ttu-id="01969-169">Följande är exempel händelse rutnätet rolldefinitioner som tillåter användare att utföra olika uppsättningar av åtgärder.</span><span class="sxs-lookup"><span data-stu-id="01969-169">The following are sample Event Grid role definitions which allow users to perform different sets of actions.</span></span>

<span data-ttu-id="01969-170">**EventGridReadOnlyRole.json**: Tillåt endast endast läsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="01969-170">**EventGridReadOnlyRole.json**: Only allow read only operations.</span></span>
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

<span data-ttu-id="01969-171">**EventGridNoDeleteListKeysRole.json**: Tillåt åtgärder begränsade efter Tillåt men inte ta bort åtgärder.</span><span class="sxs-lookup"><span data-stu-id="01969-171">**EventGridNoDeleteListKeysRole.json**: Allow restricted post actions but disallow delete actions.</span></span>
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
 
<span data-ttu-id="01969-172">**EventGridContributorRole.json**: tillåter alla händelse rutnätet åtgärder.</span><span class="sxs-lookup"><span data-stu-id="01969-172">**EventGridContributorRole.json**: Allows all event grid actions.</span></span>  
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

#### <a name="install-and-login-to-azure-cli"></a><span data-ttu-id="01969-173">Installera och logga in på Azure CLI</span><span class="sxs-lookup"><span data-stu-id="01969-173">Install and login to Azure CLI</span></span>

* <span data-ttu-id="01969-174">Azure CLI version 0.8.8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="01969-174">Azure CLI version 0.8.8 or later.</span></span> <span data-ttu-id="01969-175">Om du vill installera den senaste versionen och koppla den till din Azure-prenumeration, se [installera och konfigurera Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="01969-175">To install the latest version and associate it with your Azure subscription, see [Install and Configure the Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="01969-176">Azure Resource Manager i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="01969-176">Azure Resource Manager in Azure CLI.</span></span> <span data-ttu-id="01969-177">Gå till [med hjälp av Azure CLI med Resource Manager](../xplat-cli-azure-resource-manager.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="01969-177">Go to [Using the Azure CLI with the Resource Manager](../xplat-cli-azure-resource-manager.md) for more details.</span></span>

#### <a name="create-a-custom-role"></a><span data-ttu-id="01969-178">Skapa en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="01969-178">Create a custom role</span></span>

<span data-ttu-id="01969-179">Använd för att skapa en anpassad roll:</span><span class="sxs-lookup"><span data-stu-id="01969-179">To create a custom role, use:</span></span>

    azure role create --inputfile <file path>

#### <a name="assign-the-role-to-a-user"></a><span data-ttu-id="01969-180">Tilldela rollen till en användare</span><span class="sxs-lookup"><span data-stu-id="01969-180">Assign the role to a user</span></span>


    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>


## <a name="next-steps"></a><span data-ttu-id="01969-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="01969-181">Next steps</span></span>

* <span data-ttu-id="01969-182">En introduktion till händelse rutnätet finns [om händelsen rutnätet](overview.md)</span><span class="sxs-lookup"><span data-stu-id="01969-182">For an introduction to Event Grid, see [About Event Grid](overview.md)</span></span>
