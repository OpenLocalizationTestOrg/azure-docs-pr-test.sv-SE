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
# <a name="event-grid-security-and-authentication"></a>Händelsen rutnätet säkerhet och autentisering 

Azure händelse rutnätet har tre typer av autentisering:

* Prenumerationer på händelser
* Händelse-publicering
* WebHook händelse leverans

## <a name="webhook-event-delivery"></a>WebHook-händelse leverans

Webhooks är en av många olika sätt tooreceive händelser i realtid från Azure Event rutnät.

Varje gång det finns nya redo händelse-toobe levereras, skickar händelse rutnätet en HTTP-begäran med tooyour WebHook med hello händelse i hello brödtext.

När du registrerar WebHook slutpunkten med händelsen rutnät skickar den en POST-begäran med en enkel verifiering kod i ordning tooprove endpoint ägarskap. Appen måste toorespond av eko tillbaka hello verifieringskoden. Händelsen rutnätet Skicka inte händelser tooWebHook slutpunkter som inte har klarat hello validering.
 
### <a name="validation-details"></a>Information om verifiering:

* När hello händelse prenumeration skapa/uppdatera bokför händelse rutnätet en ”SubscriptionValidationEvent” händelse toohello mål slutpunkt.
* hello innehåller ett huvudvärde ”-Händelsetyp: verifiering”.
* hello händelsemeddelandet har hello samma schema som andra händelser med händelse rutnätet.
* hello händelsedata innehåller egenskapen ”ValidationCode” med en slumpmässigt genererad sträng t.ex. ”ValidationCode: acb13...”.

I ordning tooprove endpoint ägarskap echo tillbaka hello validering koden t.ex ”. ValidationResponse: acb13...”.

Slutligen är det viktigt toonote att Azure händelse rutnätet endast stöder HTTPS webhook-slutpunkter.
## <a name="event-subscription"></a>Händelseprenumerationen

toosubscribe tooan händelse, måste du ha hello **Microsoft.EventGrid/EventSubscriptions/Write** behörighet på hello krävs resurs. Du behöver den här behörigheten eftersom du skriver en ny prenumeration på hello omfattningen av hello resurs. hello krävs resurs skiljer sig åt beroende på om du prenumerera tooa Systemavsnittet eller anpassade avsnittet. Båda typerna beskrivs i det här avsnittet.

### <a name="system-topics-azure-service-publishers"></a>System avsnitt (utgivare Azure-tjänst)

Du måste behörighet toowrite en ny händelseprenumeration på hello omfattningen av hello resurs publishing hello händelse för system-avsnitt. hello resurs hello format är:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Till exempel toosubscribe tooan händelse på ett lagringskonto med namnet **MITTKONTO**, du behöver behörighet att hello Microsoft.EventGrid/EventSubscriptions/Write på:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Anpassade avsnitt

Du måste behörighet toowrite en ny händelseprenumeration i hello omfattningen av hello händelse rutnätet avsnittet för anpassade avsnitt. hello resurs hello format är:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Till exempel toosubscribe tooa anpassade ämne med namnet **mytopic**, du behöver behörighet att hello Microsoft.EventGrid/EventSubscriptions/Write på:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="topic-publishing"></a>Avsnittet publicering

Avsnitt om använda delade signatur åtkomst (SAS) eller nyckelautentisering. Vi rekommenderar SAS, men nyckelautentisering ger enkel programmering och är kompatibel med många befintliga webhook-utgivare. 

Du inkluderar hello autentisering värde i hello HTTP-huvud. Använd för SAS, **aeg-sas-token** för hello huvudvärde. Nyckelautentisering, Använd **aeg-sas-nyckeln** för hello huvudvärde.

### <a name="key-authentication"></a>Autentisering med nyckel

Nyckelautentisering är hello enklaste formen av autentisering. Använd hello format:`aeg-sas-key: <your key>`

Exempelvis kan du ange en nyckel med: 

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS-token

SAS-token för händelsen rutnätet innehåller hello resurs, en förfallotid och en signatur. hello hello SAS-token har formatet: `r={resource}&e={expiration}&s={signature}`.

hello resursen är hello sökväg för hello avsnittet toowhich du skickar händelser. Till exempel är en giltig resurs-sökväg:`https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

Du kan generera hello signatur från en nyckel.

Till exempel en giltig **aeg-sas-token** värde är:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
``` 

hello följande exempel skapas en SAS-token för användning med händelsen rutnätet:

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

Azure händelse rutnätet kan du toocontrol hello andelen åtkomst ges toodifferent användare toodo olika hanteringsåtgärder, till exempel listan händelseprenumerationer, skapa nya och generera nycklar. Händelsen rutnätet använder Azures rollbaserad åtkomst Kontrollera (RBAC) för den här.

### <a name="operation-types"></a>Åtgärdstyper

Azure händelse rutnätet stöder hello följande åtgärder:

* Microsoft.EventGrid/*/read 
* Microsoft.EventGrid/*/write 
* Microsoft.EventGrid/*/delete 
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action 
* Microsoft.EventGrid/topics/listKeys/action 
* Microsoft.EventGrid/topics/regenerateKey/action

Hej sista tre åtgärder returnerar potentiellt hemlig information som hämtar filtrerat utanför normal läsåtgärder. Det är bäst för dig toorestrict åtkomståtgärder toothese. Anpassade roller kan skapas med [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [Azure-kommandoradsgränssnittet (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md), och hello [REST API](../active-directory/role-based-access-control-manage-access-rest.md).

### <a name="enforcing-role-based-access-check-rbac"></a>Framtvinga roll baserad åtkomstkontroll (RBAC)

Använd följande steg tooenforce RBAC för olika användare hello:

#### <a name="create-a-custom-role-definition-file-json"></a>Skapa en anpassad roll definitionsfil (JSON)

hello följande är exempel händelse rutnätet rolldefinitioner där användarna tooperform olika uppsättningar av åtgärder.

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

#### <a name="install-and-login-tooazure-cli"></a>Installera och logga in tooAzure CLI

* Azure CLI version 0.8.8 eller senare. tooinstall hello senaste versionen och koppla den med din Azure-prenumeration finns [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md).
* Azure Resource Manager i Azure CLI. Gå för[Using hello Azure CLI med hello Resource Manager](../xplat-cli-azure-resource-manager.md) för mer information.

#### <a name="create-a-custom-role"></a>Skapa en anpassad roll

Använd toocreate en anpassad roll:

    azure role create --inputfile <file path>

#### <a name="assign-hello-role-tooa-user"></a>Tilldela hello rollen tooa användare


    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>


## <a name="next-steps"></a>Nästa steg

* En introduktion tooEvent rutnätet finns [om händelsen rutnätet](overview.md)
