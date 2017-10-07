---
title: aaaAzure resursplats i mallen | Microsoft Docs
description: "Visar hur tooset en plats för en resurs i en Azure Resource Manager-mall"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: tomfitz
ms.openlocfilehash: f2ad6ca6ac5f34484a2e5e57dd8d67c77dacc41a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-resource-location-in-azure-resource-manager-templates"></a>Ange resursplats i Azure Resource Manager-mallar
När du distribuerar en mall måste du ange en plats för varje resurs. Det här avsnittet visar hur toodetermine hello platser som är tillgängliga tooyour prenumerationen för varje resurs skriver.

## <a name="determine-supported-locations"></a>Fastställa platser som stöds

En fullständig lista över platser som stöds för varje resurstyp av finns [produkter som är tillgängliga efter region](https://azure.microsoft.com/regions/services/). Prenumerationen kanske inte har åtkomst tooall hello platser i listan. toosee en egen lista över platser som är tillgängliga tooyour prenumeration, använder du Azure PowerShell eller Azure CLI. 

hello följande exempel används PowerShell tooget hello platser för hello `Microsoft.Web\sites` resurstyp:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

hello följande exempel använder Azure CLI 2.0 tooget hello platser för hello `Microsoft.Web\sites` resurstyp:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="set-location-in-template"></a>Ange plats i mallen

När du fastställt hello stöds platser för dina resurser, måste tooset där i mallen. Hej enklaste sättet tooset det här värdet är en resursgrupp i en plats som stöder hello resurstyper toocreate och ange varje plats för`[resourceGroup().location]`. Du kan distribuera hello mallen tooresource grupper på olika platser och inte ändra alla värden i hello mall eller parametrar. 

hello följande exempel visas ett lagringskonto som är distribuerade toohello samma plats som hello resursgrupp:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

Om du behöver toohardcode hello plats i mallen, ange hello namnet på en av hello stöds regioner. hello som följande exempel visar ett lagringskonto som alltid är distribuerat tooNorth centrala USA:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="next-steps"></a>Nästa steg
* Rekommendationer om hur toocreate mallar, se [bästa praxis för att skapa mallar för Azure Resource Manager](resource-manager-template-best-practices.md).

