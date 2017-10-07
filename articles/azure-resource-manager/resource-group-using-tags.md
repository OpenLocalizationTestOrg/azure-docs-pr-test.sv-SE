---
title: "aaaTag Azure-resurser för logisk struktur | Microsoft Docs"
description: "Visar hur tooapply taggar tooorganize Azure-resurser för fakturering och hantering."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 003a78e5-2ff8-4685-93b4-e94d6fb8ed5b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: AzurePortal
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: tomfitz
ms.openlocfilehash: e07470463d160f8cefe5c80bc91e66a96af6ca45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-tags-tooorganize-your-azure-resources"></a>Använd taggar tooorganize Azure-resurser
[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

> [!NOTE]
> Du kan använda taggar endast tooresources som stöd för Azure Resource Manager-åtgärder. Om du har skapat en virtuell dator, virtuella nätverk eller lagringskonto via hello klassiska distributionsmodellen (exempelvis som via hello klassiska Azure-portalen) kan du inte använda en tagg toothat resurs. distribuera toosupport taggning, om dessa resurser via Resource Manager. Alla andra resurser stöd för märkning.
> 
> 

## <a name="policies-for-tag-consistency"></a>Principer för taggen konsekvenskontroll

Du kan använda principer för resursen toocreate standardregler för din organisation. Du kan skapa principer som kontrollera resurser som är märkta med hello lämpliga värden. Mer information finns i [gäller resursprinciper för taggar](resource-manager-policy-tags.md).

## <a name="powershell"></a>PowerShell
[!INCLUDE [resource-manager-tag-resources-powershell](../../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure CLI

toosee hello befintliga taggar för en *resursgruppen*, Använd:

```azurecli
az group show -n examplegroup --query tags
```

Skriptet returnerar hello följande format:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

toosee hello befintliga taggar för en *resurs som har en viss resurs-ID*, Använd:

```azurecli
az resource show --id {resource-id} --query tags
```

Eller toosee hello befintliga taggar för en *resurs som har en viss grupp namn, typ och resursen*, Använd:

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Använd tooget resursgrupper som har en specifik tagg `az group list`:

```azurecli
az group list --tag Dept=IT
```

tooget alla hello-resurser som har en viss tagg och värde, Använd `az resource list`:

```azurecli
az resource list --tag Dept=Finance
```

Varje gång du tillämpar taggar tooa resurs eller en resursgrupp kan du skriva över hello befintliga taggar på denna resurs eller resursgrupp. Därför måste du använda en annan metod baserat på om hello resurs eller resursgrupp har befintliga taggar. 

tooadd taggar tooa *resursgruppen utan befintliga taggar*, Använd:

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

tooadd taggar tooa *resursen utan befintliga taggar*, Använd:

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
``` 

tooadd taggar tooa resurs som redan har taggar, hämta hello befintliga taggar, formatera om värdet och tillämpa hello befintliga och nya taggar: 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

tooapply alla taggar från resursen grupp tooits resurser och *inte behålla befintliga taggar på hello resurser*, använda hello följande skript:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups 
do 
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv) 
  for resid in $r 
  do 
    az resource tag --tags $t --id $resid
  done 
done
```

tooapply alla taggar från resursen grupp tooits resurser och *behåller befintliga taggar på resurser*, använda hello följande skript:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups 
do 
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv) 
  for resid in $r 
  do 
    jsonrtag=$(az resource show --id $resid --query tags)
    rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
    az resource tag --tags $t$rt --id $resid
  done 
done
```


## <a name="templates"></a>Mallar

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a>Portalen
[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]


## <a name="rest-api"></a>REST API
hello Azure portal och PowerShell använder hello [Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) hello bakgrunden. Om du behöver toointegrate märkning i en annan miljö, kan du få taggar med hjälp av **hämta** på hello resurs-ID och uppdatera hello uppsättning taggar med hjälp av en **korrigering** anropa.

## <a name="tags-and-billing"></a>Taggar och fakturering
Du kan använda taggar toogroup din faktureringsinformation. Till exempel om du kör flera virtuella datorer i olika organisationer kan använda hello taggar toogroup användning per kostnadsställe. Du kan också använda taggar toocategorize kostnader av körningsmiljön, till exempel hello fakturering användning för virtuella datorer som körs i hello produktionsmiljö.


Du kan hämta information om taggar via hello [Azure Resursanvändning och RateCard APIs](../billing/billing-usage-rate-card-overview.md) eller hello användning fil med kommaavgränsade värden (CSV) fil. Du ladda ned filen för användning av hello från hello [Azure kontoportalen](https://account.windowsazure.com/) eller [EA portal](https://ea.azure.com). Mer information om Programmeringsåtkomst toobilling information finns i [få insikter om dina Microsoft Azure-resursförbrukning](../billing/billing-usage-rate-card-overview.md). REST-API: et finns [Azure Billing REST API-referens](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).


När du hämtar hello användning CSV för tjänster som stöder taggar med fakturering hello taggar visas i hello **taggar** kolumn. Mer information finns i [förstå fakturan för Microsoft Azure](../billing/billing-understand-your-bill.md).

![Se taggar i fakturering](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Nästa steg
* Du kan tillämpa begränsningar och konventioner över din prenumeration med hjälp av anpassade principer. En princip som du definierar kan kräva att alla resurser som har ett värde för en viss tagg. Mer information finns i [använda principer toomanage resurser och styr åtkomsten](resource-manager-policy.md).
* En introduktion toousing Azure PowerShell när du distribuerar resurser, se [med hjälp av Azure PowerShell med Azure Resource Manager](powershell-azure-resource-manager.md).
* En introduktion toousing hello Azure CLI när du distribuerar resurser, se [Using hello Azure CLI för Mac, Linux och Windows med Azure Resource Manager](xplat-cli-azure-resource-manager.md).
* En introduktion toousing hello portal finns [Using hello Azure portal toomanage resurserna i Azure](resource-group-portal.md).  
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

