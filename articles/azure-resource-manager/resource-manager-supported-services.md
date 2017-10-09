---
title: aaaAzure resursproviders och resurstyper | Microsoft Docs
description: "Beskriver hello resursproviders som har stöd för hanteraren för filserverresurser, scheman och tillgängliga API-versioner och hello regioner som kan vara värd för hello resurser."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 23db1d3808a20166f3b44ec801e1bcc46fbb9bd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resource-providers-and-types"></a>Resursproviders och typer

När du distribuerar resurser kan behöver du ofta tooretrieve information om hello resursproviders och typer. I den här artikeln får du lära dig att:

* Visa alla providrar i Azure
* Kontrollera registreringsstatus av en resursprovider
* En registerresursleverantören
* Visa resurstyper för en resursprovider
* Visa giltiga platser för en resurstyp
* Visa giltiga API-versioner för en resurstyp

Du kan utföra dessa steg till hello-portalen, PowerShell eller Azure CLI.

## <a name="powershell"></a>PowerShell

toosee alla providrar i Azure och hello registreringsstatus för din prenumeration, Använd:

```powershell
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

Som returnerar resultat liknar:

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Registrera en resursleverantör konfigurerar din prenumeration toowork med hello resursprovidern. hello omfånget för registrering är alltid hello prenumeration. Många resursproviders registreras automatiskt som standard. Du kan dock behöva toomanually registrera vissa resursleverantörer. tooregister en resursleverantör, måste du ha behörigheten tooperform hello `/register/action` åtgärden för hello resursprovidern. Den här åtgärden finns i hello deltagare och ägare roller.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Som returnerar resultat liknar:

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

Du kan inte avregistrera en resursleverantör när du har fortfarande resurstyper från resursprovidern i din prenumeration.

toosee information för en viss resurs-provider, Använd:

```powershell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Som returnerar resultat liknar:

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

toosee hello resurstyper för en resurs-provider, Använd:

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Som returnerar:

```powershell
batchAccounts
operations
locations
locations/quotas
```

hello API-versionen motsvarar tooa version av REST API-åtgärder som ges ut av hello resursprovidern. Som en resursleverantör aktiverar nya funktioner, släpper en ny version av hello REST API. 

Använd tooget hello tillgängliga API-versioner för en resurstyp:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Som returnerar:

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Hanteraren för filserverresurser stöds i alla regioner, men hello-resurser som du distribuerar stöds inte i alla regioner. Dessutom kan finnas det begränsningar för din prenumeration som hindrar dig från att använda vissa områden som stöder hello resurs. 

Använd tooget hello stöds platser för en resurstyp.

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Som returnerar:

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>Azure CLI
toosee alla providrar i Azure och hello registreringsstatus för din prenumeration, Använd:

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Som returnerar resultat liknar:

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Registrera en resursleverantör konfigurerar din prenumeration toowork med hello resursprovidern. hello omfånget för registrering är alltid hello prenumeration. Många resursproviders registreras automatiskt som standard. Du kan dock behöva toomanually registrera vissa resursleverantörer. tooregister en resursleverantör, måste du ha behörigheten tooperform hello `/register/action` åtgärden för hello resursprovidern. Den här åtgärden finns i hello deltagare och ägare roller.

```azurecli
az provider register --namespace Microsoft.Batch
```

Som returnerar ett meddelande som registrering är pågående.

Du kan inte avregistrera en resursleverantör när du har fortfarande resurstyper från resursprovidern i din prenumeration.

toosee information för en viss resurs-provider, Använd:

```azurecli
az provider show --namespace Microsoft.Batch
```

Som returnerar resultat liknar:

```azurecli
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

toosee hello resurstyper för en resurs-provider, Använd:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Som returnerar:

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

hello API-versionen motsvarar tooa version av REST API-åtgärder som ges ut av hello resursprovidern. Som en resursleverantör aktiverar nya funktioner, släpper en ny version av hello REST API. 

Använd tooget hello tillgängliga API-versioner för en resurstyp:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Som returnerar:

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Hanteraren för filserverresurser stöds i alla regioner, men hello-resurser som du distribuerar stöds inte i alla regioner. Dessutom kan finnas det begränsningar för din prenumeration som hindrar dig från att använda vissa områden som stöder hello resurs. 

Använd tooget hello stöds platser för en resurstyp.

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Som returnerar:

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="portal"></a>Portalen

toosee alla providrar i Azure och hello registreringsstatus för din prenumeration, Välj **prenumerationer**.

![Välj prenumerationer](./media/resource-manager-supported-services/select-subscriptions.png)

Välj hello prenumeration tooview.

![Ange prenumeration](./media/resource-manager-supported-services/subscription.png)

Välj **resursproviders** och visa hello en lista över tillgängliga resursproviders.

![Visa resursprovidrar](./media/resource-manager-supported-services/show-resource-providers.png)

Registrera en resursleverantör konfigurerar din prenumeration toowork med hello resursprovidern. hello omfånget för registrering är alltid hello prenumeration. Många resursproviders registreras automatiskt som standard. Du kan dock behöva toomanually registrera vissa resursleverantörer. tooregister en resursleverantör, måste du ha behörigheten tooperform hello `/register/action` åtgärden för hello resursprovidern. Den här åtgärden finns i hello deltagare och ägare roller. Välj tooregister en resursleverantör **registrera**.

![registerresursleverantören](./media/resource-manager-supported-services/register-provider.png)

Du kan inte avregistrera en resursleverantör när du har fortfarande resurstyper från resursprovidern i din prenumeration.

toosee information för en viss resurs-provider, Välj **fler tjänster**.

![Välj fler tjänster](./media/resource-manager-supported-services/more-services.png)

Sök efter **Resursläsaren** och välj den hello tillgängliga alternativ.

![Välj resursläsaren](./media/resource-manager-supported-services/select-resource-explorer.png)

Välj **Providers**.

![Välj providers](./media/resource-manager-supported-services/select-providers.png)

Välj hello resursprovidern och resurs skriver du som du vill tooview.

![Välj resurstyp](./media/resource-manager-supported-services/select-resource-type.png)

Hanteraren för filserverresurser stöds i alla regioner, men hello-resurser som du distribuerar stöds inte i alla regioner. Dessutom kan finnas det begränsningar för din prenumeration som hindrar dig från att använda vissa områden som stöder hello resurs. Hej resursläsaren visar giltiga platser för hello resurstypen.

![Visa platser](./media/resource-manager-supported-services/show-locations.png)

hello API-versionen motsvarar tooa version av REST API-åtgärder som ges ut av hello resursprovidern. Som en resursleverantör aktiverar nya funktioner, släpper en ny version av hello REST API. Hej resursläsaren visar giltiga API-versioner för hello resurstypen.

![Visa API-versioner](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a>Nästa steg
* toolearn om hur du skapar Resource Manager-mallar finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* toolearn om hur du distribuerar resurser, se [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).
* tooview hello åtgärder för en resursleverantör finns [Azure REST API](/rest/api/).

