---
title: "Azure SKU inte tillgänglig fel | Microsoft Docs"
description: "Beskriver hur du felsöker SKU: N inte tillgängliga fel under distributionen."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: 25cea4ae23471d182105ca3f720aaf74f81bf8c4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-sku-not-available"></a>Åtgärda fel för SKU är inte tillgänglig

Den här artikeln beskriver hur du löser den **SkuNotAvailable** fel.

## <a name="symptom"></a>Symtom

När du distribuerar en resurs (vanligtvis en virtuell dator), visas följande felkod och felmeddelande:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

## <a name="cause"></a>Orsak

Du får detta felmeddelande när resursen SKU som du har valt (till exempel VM-storlek) inte är tillgänglig för den plats som du har valt.

## <a name="solution"></a>Lösning

För att lösa problemet måste du bestämma vilka SKU: er är tillgängliga i en region. Du kan använda PowerShell, portalen eller en REST-åtgärden för att hitta tillgängliga SKU: er.

### <a name="solution-1"></a>Lösning 1

Använd den [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) i PowerShell. Filtrera resultatet av platsen. Du måste ha den senaste versionen av PowerShell för det här kommandot.

```powershell
Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("southcentralus")}
```

Resultatet innehåller en lista över SKU: er för platsen och eventuella begränsningar för den SKU.

```powershell
ResourceType                Name      Locations Restriction                      Capability Value
------------                ----      --------- -----------                      ---------- -----
availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
virtualMachines      Standard_A0 southcentralus
virtualMachines      Standard_A1 southcentralus
virtualMachines      Standard_A2 southcentralus
```

### <a name="solution-2"></a>Lösning 2

I Azure CLI, använder den `az vm list-skus` kommando. Du kan sedan använda `grep` eller ett liknande verktyg för att filtrera utdata.

```bash
$ az vm list-skus --output table
ResourceType      Locations           Name                    Capabilities                       Tier      Size           Restrictions
----------------  ------------------  ----------------------  ---------------------------------  --------  -------------  ---------------------------
availabilitySets  eastus              Classic                 MaximumPlatformFaultDomainCount=3
avilabilitySets   eastus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Aligned                 MaximumPlatformFaultDomainCount=3
```

### <a name="solution-3"></a>Lösning 3

Använd den [portal](https://portal.azure.com). Logga in på portalen och lägga till en resurs via gränssnittet. När du anger värden kan du se tillgängliga SKU: er för den här resursen. Du behöver inte distribueras.

![tillgängliga SKU: er](./media/resource-manager-sku-not-available-errors/view-sku.png)

### <a name="solution-4"></a>Lösning 4

Använda REST API för virtuella datorer. Skicka följande begäran:

```HTTP 
GET
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
```

Tillgängliga SKU: er och regioner returneras i följande format:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    ...
  ]
}
```

Om det inte går att hitta en lämplig SKU i den regionen eller en annan region som uppfyller företagets behov, skicka ett [SKU begäran](https://aka.ms/skurestriction) till Azure-supporten.