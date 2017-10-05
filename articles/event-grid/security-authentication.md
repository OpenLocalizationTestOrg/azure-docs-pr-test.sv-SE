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
# <a name="event-grid-security-and-authentication"></a>Händelsen rutnätet säkerhet och autentisering 

Azure händelse rutnätet har tre typer av autentisering:

* Prenumerationer på händelser
* Händelse-publicering
* WebHook händelse leverans

## <a name="webhook-event-delivery"></a>WebHook-händelse leverans

Webhooks är en av många olika sätt att ta emot händelser i realtid från Azure Event rutnät.

Varje gång det finns en ny händelse som är redo att levereras, skickar händelse rutnätet en HTTP-begäran med till din WebHook med händelsen i brödtexten.

När du registrerar WebHook slutpunkten med händelsen rutnät skickar den en POST-begäran med en enkel verifiering kod för att bevisa att du äger för slutpunkten. Din app behöver svara genom eko tillbaka verifieringskoden. Händelsen rutnätet Skicka inte händelser till WebHook-slutpunkter som inte har klarat valideringen.
 
### <a name="validation-details"></a>Information om verifiering:

* Vid tidpunkten för händelsen prenumeration skapa/uppdatera bokför händelse rutnätet en ”SubscriptionValidationEvent”-händelse till mål-slutpunkten.
* Innehåller ett huvudvärde ”-Händelsetyp: verifiering”.
* Händelsemeddelandet innehåller samma schema som andra händelser med händelse rutnätet.
* Händelsedata som innehåller egenskapen ”ValidationCode” med en slumpmässigt genererad sträng t.ex. ”ValidationCode: acb13...”.

För att bevisa endpoint ägarskap echo tillbaka validering koden t.ex ”. ValidationResponse: acb13...”.

Slutligen är det viktigt att Observera att Azure händelse rutnätet endast stöder HTTPS webhook-slutpunkter.
## <a name="event-subscription"></a>Händelseprenumerationen

Om du vill prenumerera på en händelse, måste du ha den **Microsoft.EventGrid/EventSubscriptions/Write** behörighet för den begärda resursen. Du behöver den här behörigheten eftersom du skriver en ny prenumeration på omfattningen av resursen. Den begärda resursen skiljer sig åt beroende på om du prenumerera på ett Systemavsnittet eller anpassade avsnittet. Båda typerna beskrivs i det här avsnittet.

### <a name="system-topics-azure-service-publishers"></a>System avsnitt (utgivare Azure-tjänst)

För system ämnen behöver du behörighet att skriva en ny händelseprenumeration på omfattningen av resurspublicerings händelsen. Formatet för resursen är:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Till exempel att prenumerera på en händelse på ett lagringskonto med namnet **MITTKONTO**, du måste ha behörighet för Microsoft.EventGrid/EventSubscriptions/Write på:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Anpassade avsnitt

För anpassade avsnitt behöver behörighet att skriva en ny händelseprenumeration på omfattningen av händelse rutnätet ämnet. Formatet för resursen är:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Om du till exempel att prenumerera på en anpassad avsnittet namnet **mytopic**, du måste ha behörighet för Microsoft.EventGrid/EventSubscriptions/Write på:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="topic-publishing"></a>Avsnittet publicering

Avsnitt om använda delade signatur åtkomst (SAS) eller nyckelautentisering. Vi rekommenderar SAS, men nyckelautentisering ger enkel programmering och är kompatibel med många befintliga webhook-utgivare. 

Du kan inkludera autentiseringsvärdet för i HTTP-huvudet. Använd för SAS, **aeg-sas-token** för huvudets värde. Nyckelautentisering, Använd **aeg-sas-nyckeln** för huvudets värde.

### <a name="key-authentication"></a>Autentisering med nyckel

Nyckelautentisering är den enklaste formen av autentisering. Använd formatet:`aeg-sas-key: <your key>`

Exempelvis kan du ange en nyckel med: 

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS-token

SAS-token för händelsen rutnätet är resursen, en förfallotid och en signatur. Formatet på SAS-token är: `r={resource}&e={expiration}&s={signature}`.

Resursen är sökvägen till avsnittet som du skickar händelser. Till exempel är en giltig resurs-sökväg:`https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

Du kan skapa signaturen från en nyckel.

Till exempel en giltig **aeg-sas-token** värde är:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
``` 

I följande exempel skapas en SAS-token för användning med händelsen rutnät:

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

## <a name="management-access-control"></a>Åtkomstkontroll för hantering

Azure händelse rutnätet kan du styra nivån för olika användare att utföra olika hanteringsåtgärder, till exempel listan händelseprenumerationer, skapa nya och generera nycklar. Händelsen rutnätet använder Azures rollbaserad åtkomst Kontrollera (RBAC) för den här.

### <a name="operation-types"></a>Åtgärdstyper

Azure händelse rutnätet har stöd för följande åtgärder:

* Microsoft.EventGrid/*/read 
* Microsoft.EventGrid/*/write 
* Microsoft.EventGrid/*/delete 
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action 
* Microsoft.EventGrid/topics/listKeys/action 
* Microsoft.EventGrid/topics/regenerateKey/action

De tre sista åtgärderna returnera potentiellt hemlig information som hämtar filtrerat utanför normal läsåtgärder. Det är bäst för dig att begränsa åtkomsten till dessa åtgärder. Anpassade roller kan skapas med [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [Azure-kommandoradsgränssnittet (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md), och [REST API](../active-directory/role-based-access-control-manage-access-rest.md).

### <a name="enforcing-role-based-access-check-rbac"></a>Framtvinga roll baserad åtkomstkontroll (RBAC)

Använd följande steg för att genomdriva RBAC för olika användare:

#### <a name="create-a-custom-role-definition-file-json"></a>Skapa en anpassad roll definitionsfil (JSON)

Följande är exempel händelse rutnätet rolldefinitioner som tillåter användare att utföra olika uppsättningar av åtgärder.

**EventGridReadOnlyRole.json**: Tillåt endast endast läsåtgärder.
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

**EventGridNoDeleteListKeysRole.json**: Tillåt åtgärder begränsade efter Tillåt men inte ta bort åtgärder.
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
 
**EventGridContributorRole.json**: tillåter alla händelse rutnätet åtgärder.  
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

#### <a name="install-and-login-to-azure-cli"></a>Installera och logga in på Azure CLI

* Azure CLI version 0.8.8 eller senare. Om du vill installera den senaste versionen och koppla den till din Azure-prenumeration, se [installera och konfigurera Azure CLI](../cli-install-nodejs.md).
* Azure Resource Manager i Azure CLI. Gå till [med hjälp av Azure CLI med Resource Manager](../xplat-cli-azure-resource-manager.md) för mer information.

#### <a name="create-a-custom-role"></a>Skapa en anpassad roll

Använd för att skapa en anpassad roll:

    azure role create --inputfile <file path>

#### <a name="assign-the-role-to-a-user"></a>Tilldela rollen till en användare


    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>


## <a name="next-steps"></a>Nästa steg

* En introduktion till händelse rutnätet finns [om händelsen rutnätet](overview.md)
