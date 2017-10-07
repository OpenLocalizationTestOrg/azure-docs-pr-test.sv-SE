---
title: aaaAzure resursprinciper | Microsoft Docs
description: "Beskriver hur toouse Azure Resource Manager konsekvent resursegenskaper för principer tooensure anges under distributionen. Principer kan tillämpas på hello prenumerationen eller resursen grupper."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: abde0f73-c0fe-4e6d-a1ee-32a6fce52a2d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: tomfitz
ms.openlocfilehash: f1b0bbb5f838f6bb70721e1040ad3eac2d881cea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resource-policy-overview"></a>Översikt över princip för resurs
Resursprinciper aktivera tooestablish konventioner för resurser i din organisation. Du kan styra kostnader genom att definiera konventioner och mer hantera enkelt dina resurser. Du kan till exempel ange att endast vissa typer av virtuella datorer är tillåtna. Eller, du kan kräva att alla resurser som har en viss tagg. Principer ärvs av alla underordnade resurser. Så om en princip är tillämpade tooa resursgrupp, är det tillämpliga tooall hello resurser i resursgruppen.

Det finns två begrepp toounderstand om principer:

* principdefinitionen - beskrivs när hello tillämpas och vilken åtgärd tootake
* tilldelning av principer - installation av hello definition tooa principområdet (prenumeration eller resursgrupp)

Det här avsnittet fokuserar på principdefinitionen. Information om tilldelning av principer finns [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md) eller [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md).

Principer utvärderas när du skapar och uppdaterar resurser (PLACERA och korrigering operations).

> [!NOTE]
> Principen utvärderas för närvarande inte resurstyper som inte stöder taggar, typ och plats, till exempel hello Microsoft.Resources/deployments resurstypen. Det här stödet läggs vid ett senare tillfälle. tooavoid bakåtkompatibilitet problem bör du uttryckligen ange typen vid redigering av principer. Till exempel används taggen princip som inte anger typer för alla typer. I så fall kan en för malldistribution kan misslyckas om det finns en kapslad resurs som inte stöder taggar och hello distributionstypen resursen har lagts toopolicy utvärdering. 
> 
> 

## <a name="how-is-it-different-from-rbac"></a>Hur skiljer den från RBAC?
Det finns några viktiga skillnader mellan principen och rollbaserad åtkomstkontroll (RBAC). RBAC fokuserar på **användaren** åtgärder på olika omfång. Till exempel läggs du toohello deltagarrollen för en resursgrupp hello önskad definitionsområdet, så att du kan göra ändringar toothat resursgruppen. Principen fokuserar på **resurs** egenskaper under distributionen. Du kan till exempel styra hello typer av resurser som kan etableras via principer. Eller så kan du begränsa hello platser där hello resurser kan etableras. Till skillnad från RBAC, principen är en standard Tillåt och explicit neka system. 

toouse principer kan autentiseras via RBAC. Mer specifikt måste ditt konto den:

* `Microsoft.Authorization/policydefinitions/write`behörighet toodefine en princip
* `Microsoft.Authorization/policyassignments/write`behörighet tooassign en princip 

Dessa behörigheter ingår inte i hello **deltagare** roll.

## <a name="built-in-policies"></a>Inbyggda principer

Azure tillhandahåller vissa inbyggda principdefinitioner som kan minska antalet hello av principer som du har toodefine. Innan du fortsätter med principdefinitioner, bör du överväga om hello paketdefinition innehåller redan en inbyggd princip. hello inbyggda principdefinitioner är:

* Tillåtna platser
* Tillåtna resurstyper
* Tillåtna lagringskonto SKU: er
* Tillåtna virtuella SKU: er
* Värdet för taggen och standard
* Framtvinga taggen och värdet
* Inte tillåtet resurstyper
* Kräv SQL Server version 12.0
* Kräv kryptering för storage-konto

Du kan tilldela någon av dessa policys via hello [portal](resource-manager-policy-portal.md), [PowerShell](resource-manager-policy-create-assign.md#powershell), eller [Azure CLI](resource-manager-policy-create-assign.md#azure-cli).

## <a name="policy-definition-structure"></a>Definition av principstruktur
Du kan använda JSON toocreate en principdefinition. hello principdefinitionen innehåller element för:

* parameters
* Visningsnamn
* description
* Principregel
  * logiska utvärdering
  * effekt

hello som följande exempel visar en princip som begränsar där resurser har distribuerats:

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "hello list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you toorestrict hello locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="parameters"></a>Parametrar
Med parametrar som förenklar din för principhantering genom att minska hello antal principdefinitioner. Definiera en princip för en resursegenskap (till exempel begränsa hello platser där resurser kan distribueras) och inkludera parametrar i hello definition. Sedan kan du återanvända den principdefinitionen för olika scenarier genom att passera i olika värden (t.ex att ange en uppsättning platser för en prenumeration) när tilldelar hello principer.

Du kan ange parametrar när du skapar principdefinitioner.

```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "hello list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

hello-typ för en parameter kan vara antingen sträng eller matris. Hej metadataegenskapen används för verktyg som Azure portal toodisplay användarvänliga information. 

I hello principregeln referera parametrar med hello följande syntax: 

```json
{ 
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a>Namn och beskrivning

Du använder hello **displayName** och **beskrivning** tooidentify hello principdefinitionen och ge kontext för när den används.

## <a name="policy-rule"></a>Principregel

hello principregeln består av **om** och **sedan** block. I hello **om** block du definierar en eller flera villkor som anger när hello tillämpas. Du kan använda logiska operatorer toothese villkor tooprecisely definiera hello scenario för en princip. I hello **sedan** block, definierar du hello effekt som händer när hello **om** villkor är uppfyllda.

```json
{
  "if": {
    <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append"
  }
}
```

### <a name="logical-operators"></a>Logiska operatorer
hello stöds logiska operatorer är:

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

Hej **inte** syntax inverterar hello resultatet av hello villkor. Hej **allOf** syntax (liknande toohello logiska **och** åtgärden) kräver att alla villkor toobe är true. Hej **anyOf** syntax (liknande toohello logiska **eller** åtgärden) kräver en eller flera villkor toobe true.

Du kan kapsla logiska operatorer. följande exempel visar hello en **inte** åtgärden som är kapslad i en **allOf** igen. 

```json
"if": {
  "allOf": [
    {
      "not": {
        "field": "tags",
        "containsKey": "application"
      }
    },
    {
      "field": "type",
      "equals": "Microsoft.Storage/storageAccounts"
    }
  ]
},
```

### <a name="conditions"></a>Villkor
hello villkoret utvärderas om en **fältet** uppfyller vissa villkor. villkor för hello som stöds är:

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

När du använder hello **som** tillstånd, kan du ange ett jokertecken (*) i hello-värdet.

När du använder hello **matchar** villkor, ange `#` toorepresent en siffra `?` för en bokstav och andra tecken toorepresent faktiska tecknet. Exempel finns i [gäller resursprinciper för namn och text](resource-manager-policy-naming-convention.md).

### <a name="fields"></a>Fält
Villkor bildas genom att använda fält. Ett fält representerar egenskaper i nyttolasten för hello resursen begäran som används toodescribe hello tillstånd hello resurs.  

följande fält hello stöds:

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*` 
* Egenskapen alias - lista, se [alias](#aliases).

### <a name="effect"></a>Verkan
Stöder tre typer av effekt - `deny`, `audit`, och `append`. 

* **Neka** genererar en händelse i hello granskningsloggen och misslyckas hello begäran
* **Granska** genererar en varning-händelse i granskningsloggen men inte misslyckas hello begäran
* **Lägg till** lägger till hello definierad uppsättning fält toohello begäran 

För **bifoga**, måste du ange hello följande information:

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of hello field"
  }
]
```

hello-värdet kan vara en sträng eller ett JSON-format-objekt. 

## <a name="aliases"></a>Alias

Du kan använda egenskapen alias tooaccess specifika egenskaper för en resurstyp. Alias kan du toorestrict vilka värden eller villkor tillåts för en egenskap för en resurs. Varje alias mappar toopaths i olika API-versioner för en viss resurstyp. Under principutvärdering av hämtar hello principmodulen hello egenskapssökvägen för den API-versionen.

**Microsoft.Cache/Redis**

| Alias | Beskrivning |
| ----- | ----------- |
| Microsoft.Cache/Redis/enableNonSslPort | Ange är om hello ssl-Redis-servern port (6379) aktiverat. |
| Microsoft.Cache/Redis/shardCount | Ange hello antalet shards toobe skapas på en Premium klustret Cache.  |
| Microsoft.Cache/Redis/sku.capacity | Ange hello storleken på hello Redis-cache toodeploy.  |
| Microsoft.Cache/Redis/sku.family | Ange hello SKU-familjen toouse. |
| Microsoft.Cache/Redis/sku.name | Ange hello typ av toodeploy Redis-Cache. |

**Microsoft.Cdn/profiles**

| Alias | Beskrivning |
| ----- | ----------- |
| Microsoft.CDN/profiles/sku.name | Ange hello namn för hello prisnivån. |

**Microsoft.Compute/disks**

| Alias | Beskrivning |
| ----- | ----------- |
| Microsoft.Compute/imageOffer | Ange hello erbjudande hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/imagePublisher | Ange hello utgivaren av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/imageSku | Ange hello SKU hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/imageVersion | Ange hello version av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |


**Microsoft.Compute/virtualMachines**

| Alias | Beskrivning |
| ----- | ----------- |
| Microsoft.Compute/imageId | Ange hello identifierare för hello avbildningen toocreate hello virtuell dator. |
| Microsoft.Compute/imageOffer | Ange hello erbjudande hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/imagePublisher | Ange hello utgivaren av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/imageSku | Ange hello SKU hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/imageVersion | Ange hello version av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/licenseType | Ange hello avbildningen eller disken är licensierad lokalt. Det här värdet används bara för bilder som innehåller hello Windows Server-operativsystem.  |
| Microsoft.Compute/virtualMachines/imageOffer | Ange hello erbjudande hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/virtualMachines/imagePublisher | Ange hello utgivaren av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/virtualMachines/imageSku | Ange hello SKU hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/virtualMachines/imageVersion | Ange hello version av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/virtualMachines/osDisk.Uri | Ange hello vhd URI. |
| Microsoft.Compute/virtualMachines/sku.name | Ange hello storleken på hello virtuella datorn. |

**Microsoft.Compute/virtualMachines/extensions**

| Alias | Beskrivning |
| ----- | ----------- |
| Microsoft.Compute/virtualMachines/extensions/publisher | Ange hello namnet på hello tillägget utgivare. |
| Microsoft.Compute/virtualMachines/extensions/type | Ange hello typ av tillägget. |
| Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion | Ange hello version av hello extension. |

**Microsoft.Compute/virtualMachineScaleSets**

| Alias | Beskrivning |
| ----- | ----------- |
| Microsoft.Compute/imageId | Ange hello identifierare för hello avbildningen toocreate hello virtuell dator. |
| Microsoft.Compute/imageOffer | Ange hello erbjudande hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/imagePublisher | Ange hello utgivaren av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/imageSku | Ange hello SKU hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/imageVersion | Ange hello version av hello plattformsavbildning eller marketplace-avbildning används toocreate hello virtuell dator. |
| Microsoft.Compute/licenseType | Ange hello avbildningen eller disken är licensierad lokalt. Det här värdet används bara för bilder som innehåller hello Windows Server-operativsystem. |
| Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix | Ange hello prefixet för alla hello virtuella datorer i hello skaluppsättning. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl | Ange hello blob-URI för användaravbildning. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers | Ange hello behållaren URL-adresser som används toostore operativsystemet diskar för hello skaluppsättning. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.name | Ange hello storleken på virtuella datorer i en skaluppsättning. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.tier | Ange hello nivå med virtuella datorer i en skaluppsättning. |
  
**Microsoft.Network/applicationGateways**

| Alias | Beskrivning |
| ----- | ----------- |
| Microsoft.Network/applicationGateways/sku.name | Ange hello storleken på hello gateway. |

**Microsoft.Network/virtualNetworkGateways**

| Alias | Beskrivning |
| ----- | ----------- |
| Microsoft.Network/virtualNetworkGateways/gatewayType | Ange hello typ av den här virtuella nätverksgatewayen. |
| Microsoft.Network/virtualNetworkGateways/sku.name | Ange hello gateway SKU namn. |

**Microsoft.Sql/servers**

| Alias | Beskrivning |
| ----- | ----------- |
| Microsoft.Sql/servers/version | Ange hello hello server-versionen. |

**Microsoft.Sql/databases**

| Alias | Beskrivning |
| ----- | ----------- |
| Microsoft.Sql/servers/databases/edition | Ange hello version av hello-databasen. |
| Microsoft.Sql/servers/databases/elasticPoolName | Ange hello namnet på hello elastisk pool hello databasen är i. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveId | Ange hello konfigurerats service nivå mål-ID för hello-databasen. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveName | Ange namnet på hello av hello konfigurerad servicenivåmål av hello-databasen.  |

**Microsoft.Sql/elasticpools**

| Alias | Beskrivning |
| ----- | ----------- |
| servrar/elasticpools | Microsoft.Sql/servers/elasticPools/dtu | Ange hello totalt delade DTU för hello database-elastisk pool. |
| servrar/elasticpools | Microsoft.Sql/servers/elasticPools/edition | Ange hello utgåva av hello elastisk pool. |

**Microsoft.Storage/storageAccounts**

| Alias | Beskrivning |
| ----- | ----------- |
| Microsoft.Storage/storageAccounts/accessTier | Ange hello åtkomstnivå används för fakturering. |
| Microsoft.Storage/storageAccounts/accountType | Ange hello SKU namn. |
| Microsoft.Storage/storageAccounts/enableBlobEncryption | Ange om hello service krypterar hello data som lagras i hello blob storage-tjänst. |
| Microsoft.Storage/storageAccounts/enableFileEncryption | Ange om hello service krypterar hello data som lagras i hello file storage-tjänst. |
| Microsoft.Storage/storageAccounts/sku.name | Ange hello SKU namn. |
| Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly | Ange tooallow endast https-trafik toostorage service. |


## <a name="policy-examples"></a>Exempel på

hello följande avsnitt innehåller exempel på:

* Exempel på taggen principerna finns [gäller resursprinciper för taggar](resource-manager-policy-tags.md).
* Exempel på namngivning och text mönster finns [gäller resursprinciper för namn och text](resource-manager-policy-naming-convention.md).
* Exempel på principer för lagring finns [gäller resurskonton principer toostorage](resource-manager-policy-storage.md).
* Exempel på principer för virtuell dator finns [gäller resurs principer tooLinux VMs](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) och [gäller resurs principer tooWindows virtuella datorer](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)


## <a name="next-steps"></a>Nästa steg
* När du har definierat en regel, tilldela den tooa omfång. tooassign principer hello-portalen finns i [Använd Azure portal tooassign och hantera resursprinciper](resource-manager-policy-portal.md). tooassign principer via REST-API, PowerShell eller Azure CLI, se [tilldela och hantera principer via skript](resource-manager-policy-create-assign.md).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).
* hello princip schema har publicerats på [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

