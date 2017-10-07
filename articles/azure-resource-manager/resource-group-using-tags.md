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
# <a name="use-tags-tooorganize-your-azure-resources"></a><span data-ttu-id="75b3b-103">Använd taggar tooorganize Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="75b3b-103">Use tags tooorganize your Azure resources</span></span>
[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

> [!NOTE]
> <span data-ttu-id="75b3b-104">Du kan använda taggar endast tooresources som stöd för Azure Resource Manager-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="75b3b-104">You can apply tags only tooresources that support Azure Resource Manager operations.</span></span> <span data-ttu-id="75b3b-105">Om du har skapat en virtuell dator, virtuella nätverk eller lagringskonto via hello klassiska distributionsmodellen (exempelvis som via hello klassiska Azure-portalen) kan du inte använda en tagg toothat resurs.</span><span class="sxs-lookup"><span data-stu-id="75b3b-105">If you created a virtual machine, virtual network, or storage account through hello classic deployment model (such as through hello Azure classic portal), you cannot apply a tag toothat resource.</span></span> <span data-ttu-id="75b3b-106">distribuera toosupport taggning, om dessa resurser via Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="75b3b-106">toosupport tagging, redeploy these resources through Resource Manager.</span></span> <span data-ttu-id="75b3b-107">Alla andra resurser stöd för märkning.</span><span class="sxs-lookup"><span data-stu-id="75b3b-107">All other resources support tagging.</span></span>
> 
> 

## <a name="policies-for-tag-consistency"></a><span data-ttu-id="75b3b-108">Principer för taggen konsekvenskontroll</span><span class="sxs-lookup"><span data-stu-id="75b3b-108">Policies for tag consistency</span></span>

<span data-ttu-id="75b3b-109">Du kan använda principer för resursen toocreate standardregler för din organisation.</span><span class="sxs-lookup"><span data-stu-id="75b3b-109">You can use resource policies toocreate standard rules for your organization.</span></span> <span data-ttu-id="75b3b-110">Du kan skapa principer som kontrollera resurser som är märkta med hello lämpliga värden.</span><span class="sxs-lookup"><span data-stu-id="75b3b-110">You can create policies that ensure resources are tagged with hello appropriate values.</span></span> <span data-ttu-id="75b3b-111">Mer information finns i [gäller resursprinciper för taggar](resource-manager-policy-tags.md).</span><span class="sxs-lookup"><span data-stu-id="75b3b-111">For more information, see [Apply resource policies for tags](resource-manager-policy-tags.md).</span></span>

## <a name="powershell"></a><span data-ttu-id="75b3b-112">PowerShell</span><span class="sxs-lookup"><span data-stu-id="75b3b-112">PowerShell</span></span>
[!INCLUDE [resource-manager-tag-resources-powershell](../../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a><span data-ttu-id="75b3b-113">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="75b3b-113">Azure CLI</span></span>

<span data-ttu-id="75b3b-114">toosee hello befintliga taggar för en *resursgruppen*, Använd:</span><span class="sxs-lookup"><span data-stu-id="75b3b-114">toosee hello existing tags for a *resource group*, use:</span></span>

```azurecli
az group show -n examplegroup --query tags
```

<span data-ttu-id="75b3b-115">Skriptet returnerar hello följande format:</span><span class="sxs-lookup"><span data-stu-id="75b3b-115">That script returns hello following format:</span></span>

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

<span data-ttu-id="75b3b-116">toosee hello befintliga taggar för en *resurs som har en viss resurs-ID*, Använd:</span><span class="sxs-lookup"><span data-stu-id="75b3b-116">toosee hello existing tags for a *resource that has a specified resource ID*, use:</span></span>

```azurecli
az resource show --id {resource-id} --query tags
```

<span data-ttu-id="75b3b-117">Eller toosee hello befintliga taggar för en *resurs som har en viss grupp namn, typ och resursen*, Använd:</span><span class="sxs-lookup"><span data-stu-id="75b3b-117">Or, toosee hello existing tags for a *resource that has a specified name, type, and resource group*, use:</span></span>

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

<span data-ttu-id="75b3b-118">Använd tooget resursgrupper som har en specifik tagg `az group list`:</span><span class="sxs-lookup"><span data-stu-id="75b3b-118">tooget resource groups that have a specific tag, use `az group list`:</span></span>

```azurecli
az group list --tag Dept=IT
```

<span data-ttu-id="75b3b-119">tooget alla hello-resurser som har en viss tagg och värde, Använd `az resource list`:</span><span class="sxs-lookup"><span data-stu-id="75b3b-119">tooget all hello resources that have a particular tag and value, use `az resource list`:</span></span>

```azurecli
az resource list --tag Dept=Finance
```

<span data-ttu-id="75b3b-120">Varje gång du tillämpar taggar tooa resurs eller en resursgrupp kan du skriva över hello befintliga taggar på denna resurs eller resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="75b3b-120">Every time you apply tags tooa resource or a resource group, you overwrite hello existing tags on that resource or resource group.</span></span> <span data-ttu-id="75b3b-121">Därför måste du använda en annan metod baserat på om hello resurs eller resursgrupp har befintliga taggar.</span><span class="sxs-lookup"><span data-stu-id="75b3b-121">Therefore, you must use a different approach based on whether hello resource or resource group has existing tags.</span></span> 

<span data-ttu-id="75b3b-122">tooadd taggar tooa *resursgruppen utan befintliga taggar*, Använd:</span><span class="sxs-lookup"><span data-stu-id="75b3b-122">tooadd tags tooa *resource group without existing tags*, use:</span></span>

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

<span data-ttu-id="75b3b-123">tooadd taggar tooa *resursen utan befintliga taggar*, Använd:</span><span class="sxs-lookup"><span data-stu-id="75b3b-123">tooadd tags tooa *resource without existing tags*, use:</span></span>

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
``` 

<span data-ttu-id="75b3b-124">tooadd taggar tooa resurs som redan har taggar, hämta hello befintliga taggar, formatera om värdet och tillämpa hello befintliga och nya taggar:</span><span class="sxs-lookup"><span data-stu-id="75b3b-124">tooadd tags tooa resource that already has tags, retrieve hello existing tags, reformat that value, and reapply hello existing and new tags:</span></span> 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

<span data-ttu-id="75b3b-125">tooapply alla taggar från resursen grupp tooits resurser och *inte behålla befintliga taggar på hello resurser*, använda hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="75b3b-125">tooapply all tags from a resource group tooits resources, and *not retain existing tags on hello resources*, use hello following script:</span></span>

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

<span data-ttu-id="75b3b-126">tooapply alla taggar från resursen grupp tooits resurser och *behåller befintliga taggar på resurser*, använda hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="75b3b-126">tooapply all tags from a resource group tooits resources, and *retain existing tags on resources*, use hello following script:</span></span>

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


## <a name="templates"></a><span data-ttu-id="75b3b-127">Mallar</span><span class="sxs-lookup"><span data-stu-id="75b3b-127">Templates</span></span>

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a><span data-ttu-id="75b3b-128">Portalen</span><span class="sxs-lookup"><span data-stu-id="75b3b-128">Portal</span></span>
[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]


## <a name="rest-api"></a><span data-ttu-id="75b3b-129">REST API</span><span class="sxs-lookup"><span data-stu-id="75b3b-129">REST API</span></span>
<span data-ttu-id="75b3b-130">hello Azure portal och PowerShell använder hello [Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) hello bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="75b3b-130">hello Azure portal and PowerShell both use hello [Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) behind hello scenes.</span></span> <span data-ttu-id="75b3b-131">Om du behöver toointegrate märkning i en annan miljö, kan du få taggar med hjälp av **hämta** på hello resurs-ID och uppdatera hello uppsättning taggar med hjälp av en **korrigering** anropa.</span><span class="sxs-lookup"><span data-stu-id="75b3b-131">If you need toointegrate tagging into another environment, you can get tags by using **GET** on hello resource ID and update hello set of tags by using a **PATCH** call.</span></span>

## <a name="tags-and-billing"></a><span data-ttu-id="75b3b-132">Taggar och fakturering</span><span class="sxs-lookup"><span data-stu-id="75b3b-132">Tags and billing</span></span>
<span data-ttu-id="75b3b-133">Du kan använda taggar toogroup din faktureringsinformation.</span><span class="sxs-lookup"><span data-stu-id="75b3b-133">You can use tags toogroup your billing data.</span></span> <span data-ttu-id="75b3b-134">Till exempel om du kör flera virtuella datorer i olika organisationer kan använda hello taggar toogroup användning per kostnadsställe.</span><span class="sxs-lookup"><span data-stu-id="75b3b-134">For example, if you are running multiple VMs for different organizations, use hello tags toogroup usage by cost center.</span></span> <span data-ttu-id="75b3b-135">Du kan också använda taggar toocategorize kostnader av körningsmiljön, till exempel hello fakturering användning för virtuella datorer som körs i hello produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="75b3b-135">You can also use tags toocategorize costs by runtime environment, such as hello billing usage for VMs running in hello production environment.</span></span>


<span data-ttu-id="75b3b-136">Du kan hämta information om taggar via hello [Azure Resursanvändning och RateCard APIs](../billing/billing-usage-rate-card-overview.md) eller hello användning fil med kommaavgränsade värden (CSV) fil.</span><span class="sxs-lookup"><span data-stu-id="75b3b-136">You can retrieve information about tags through hello [Azure Resource Usage and RateCard APIs](../billing/billing-usage-rate-card-overview.md) or hello usage comma-separated values (CSV) file.</span></span> <span data-ttu-id="75b3b-137">Du ladda ned filen för användning av hello från hello [Azure kontoportalen](https://account.windowsazure.com/) eller [EA portal](https://ea.azure.com).</span><span class="sxs-lookup"><span data-stu-id="75b3b-137">You download hello usage file from hello [Azure account portal](https://account.windowsazure.com/) or [EA portal](https://ea.azure.com).</span></span> <span data-ttu-id="75b3b-138">Mer information om Programmeringsåtkomst toobilling information finns i [få insikter om dina Microsoft Azure-resursförbrukning](../billing/billing-usage-rate-card-overview.md).</span><span class="sxs-lookup"><span data-stu-id="75b3b-138">For more information about programmatic access toobilling information, see [Gain insights into your Microsoft Azure resource consumption](../billing/billing-usage-rate-card-overview.md).</span></span> <span data-ttu-id="75b3b-139">REST-API: et finns [Azure Billing REST API-referens](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).</span><span class="sxs-lookup"><span data-stu-id="75b3b-139">For REST API operations, see [Azure Billing REST API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).</span></span>


<span data-ttu-id="75b3b-140">När du hämtar hello användning CSV för tjänster som stöder taggar med fakturering hello taggar visas i hello **taggar** kolumn.</span><span class="sxs-lookup"><span data-stu-id="75b3b-140">When you download hello usage CSV for services that support tags with billing, hello tags appear in hello **Tags** column.</span></span> <span data-ttu-id="75b3b-141">Mer information finns i [förstå fakturan för Microsoft Azure](../billing/billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="75b3b-141">For more information, see [Understand your bill for Microsoft Azure](../billing/billing-understand-your-bill.md).</span></span>

![Se taggar i fakturering](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a><span data-ttu-id="75b3b-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75b3b-143">Next steps</span></span>
* <span data-ttu-id="75b3b-144">Du kan tillämpa begränsningar och konventioner över din prenumeration med hjälp av anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="75b3b-144">You can apply restrictions and conventions across your subscription by using customized policies.</span></span> <span data-ttu-id="75b3b-145">En princip som du definierar kan kräva att alla resurser som har ett värde för en viss tagg.</span><span class="sxs-lookup"><span data-stu-id="75b3b-145">A policy that you define might require that all resources have a value for a particular tag.</span></span> <span data-ttu-id="75b3b-146">Mer information finns i [använda principer toomanage resurser och styr åtkomsten](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="75b3b-146">For more information, see [Use policies toomanage resources and control access](resource-manager-policy.md).</span></span>
* <span data-ttu-id="75b3b-147">En introduktion toousing Azure PowerShell när du distribuerar resurser, se [med hjälp av Azure PowerShell med Azure Resource Manager](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="75b3b-147">For an introduction toousing Azure PowerShell when you're deploying resources, see [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="75b3b-148">En introduktion toousing hello Azure CLI när du distribuerar resurser, se [Using hello Azure CLI för Mac, Linux och Windows med Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="75b3b-148">For an introduction toousing hello Azure CLI when you're deploying resources, see [Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>
* <span data-ttu-id="75b3b-149">En introduktion toousing hello portal finns [Using hello Azure portal toomanage resurserna i Azure](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="75b3b-149">For an introduction toousing hello portal, see [Using hello Azure portal toomanage your Azure resources](resource-group-portal.md).</span></span>  
* <span data-ttu-id="75b3b-150">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="75b3b-150">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

