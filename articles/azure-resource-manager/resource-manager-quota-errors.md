---
title: Azure kvoten fel | Microsoft Docs
description: "Beskriver hur du löser resurs qouta fel."
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
ms.date: 11/27/2017
ms.author: tomfitz
ms.openlocfilehash: 3ed3da2d9730d8c30d8170ddf40fe4895dfa5dec
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/28/2017
---
# <a name="resolve-errors-for-resource-quotas"></a>Åtgärda fel för resurskvoter

Den här artikeln beskriver kvoten fel som kan uppstå när du distribuerar resurser.

## <a name="symptom"></a>Symtom

Om du distribuerar en mall som skapar resurser som överskrider Azure-kvoter får du ett distribueringsfel som ser ut som:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

Eller så kan du se:

```
Code=ResourceQuotaExceeded
Message=Creating the resource of type <resource-type> would exceed the quota of <number>
resources of type <resource-type> per resource group. The current resource count is <number>,
please delete some resources of this type before creating a new one.
```

## <a name="cause"></a>Orsak

Kvoter tillämpas per resursgrupp, prenumerationer, konton och andra scope. Din prenumeration kan exempelvis konfigureras för att begränsa antalet kärnor för en region. Om du försöker att distribuera en virtuell dator med flera kärnor än den tillåtna visas ett felmeddelande om kvoten har överskridits.
För fullständig kvotinformation finns i [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).

## <a name="solution"></a>Lösning

### <a name="solution-1"></a>Lösning 1

Azure CLI, använder den `az vm list-usage` kommando för att hitta kvoter för virtuell dator.

```azurecli
az vm list-usage --location "South Central US"
```

Som returnerar:

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

### <a name="solution-2"></a>Lösning 2

PowerShell, Använd den **Get-AzureRmVMUsage** kommando för att hitta kvoter för virtuell dator.

```powershell
Get-AzureRmVMUsage -Location "South Central US"
```

Som returnerar:

```powershell
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional Cores                         0   100 Count
Virtual Machines                             0 10000 Count
```

### <a name="solution-3"></a>Lösning 3

Gå till portalen för att begära en ökad kvot och filen ett supportproblem. I support-problemet du begära en ökning av din kvot för den region som du vill distribuera.

> [!NOTE]
> Kom ihåg att för resursgrupper, kvoten för varje enskild region, inte för hela prenumerationen. Om du behöver distribuera 30 kärnor i USA, västra måste du be om 30 Resource Manager kärnor i USA, västra. Om du behöver distribuera 30 kärnor i någon av regionerna som du har åtkomst bör du be om 30 Resource Manager kärnor i alla regioner.
>
>

1. Välj **prenumerationer**.

   ![Prenumerationer](./media/resource-manager-quota-errors/subscriptions.png)

2. Välj den prenumeration som behöver en ökad kvot.

   ![Välj en prenumeration](./media/resource-manager-quota-errors/select-subscription.png)

3. Välj **användning + kvoter**

   ![Välj användnings- och kvoter](./media/resource-manager-quota-errors/select-usage-quotas.png)

4. I det övre högra hörnet väljer **begära**.

   ![Begära](./media/resource-manager-quota-errors/request-increase.png)

5. Fyll i formulär för typ av du behöva öka kvoten.

   ![Fyll i formuläret](./media/resource-manager-quota-errors/forms.png)