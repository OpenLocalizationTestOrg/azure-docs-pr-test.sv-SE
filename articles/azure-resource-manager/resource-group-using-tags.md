---
title: "Tagga Azure-resurser för logisk struktur | Microsoft Docs"
description: "Visar hur du lägger till taggar för att organisera Azure-resurser för fakturering och hantering."
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
ms.openlocfilehash: 4f52c30614ad39da8a34ff6ecfb707b75400517f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-tags-to-organize-your-azure-resources"></a><span data-ttu-id="09cf5-103">Använd taggar för att organisera Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="09cf5-103">Use tags to organize your Azure resources</span></span>
[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

> [!NOTE]
> <span data-ttu-id="09cf5-104">Du kan endast använda taggar för resurser som stöder Azure Resource Manager-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="09cf5-104">You can apply tags only to resources that support Azure Resource Manager operations.</span></span> <span data-ttu-id="09cf5-105">Om du har skapat en virtuell dator, virtuella nätverk eller lagringskonto via den klassiska distributionsmodellen (exempelvis via den klassiska Azure-portalen), du kan inte använda en tagg till resursen.</span><span class="sxs-lookup"><span data-stu-id="09cf5-105">If you created a virtual machine, virtual network, or storage account through the classic deployment model (such as through the Azure classic portal), you cannot apply a tag to that resource.</span></span> <span data-ttu-id="09cf5-106">Distribuera om dessa resurser via Resource Manager för att stödja Taggning.</span><span class="sxs-lookup"><span data-stu-id="09cf5-106">To support tagging, redeploy these resources through Resource Manager.</span></span> <span data-ttu-id="09cf5-107">Alla andra resurser stöd för märkning.</span><span class="sxs-lookup"><span data-stu-id="09cf5-107">All other resources support tagging.</span></span>
> 
> 

## <a name="policies-for-tag-consistency"></a><span data-ttu-id="09cf5-108">Principer för taggen konsekvenskontroll</span><span class="sxs-lookup"><span data-stu-id="09cf5-108">Policies for tag consistency</span></span>

<span data-ttu-id="09cf5-109">Du kan använda principer för företagsresurser för att skapa standardregler för din organisation.</span><span class="sxs-lookup"><span data-stu-id="09cf5-109">You can use resource policies to create standard rules for your organization.</span></span> <span data-ttu-id="09cf5-110">Du kan skapa principer som kontrollera resurser som är märkta med lämpliga värden.</span><span class="sxs-lookup"><span data-stu-id="09cf5-110">You can create policies that ensure resources are tagged with the appropriate values.</span></span> <span data-ttu-id="09cf5-111">Mer information finns i [gäller resursprinciper för taggar](resource-manager-policy-tags.md).</span><span class="sxs-lookup"><span data-stu-id="09cf5-111">For more information, see [Apply resource policies for tags](resource-manager-policy-tags.md).</span></span>

## <a name="powershell"></a><span data-ttu-id="09cf5-112">PowerShell</span><span class="sxs-lookup"><span data-stu-id="09cf5-112">PowerShell</span></span>
[!INCLUDE [resource-manager-tag-resources-powershell](../../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a><span data-ttu-id="09cf5-113">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="09cf5-113">Azure CLI</span></span>

<span data-ttu-id="09cf5-114">Om du vill visa de befintliga taggarna för en *resursgrupp* använder du:</span><span class="sxs-lookup"><span data-stu-id="09cf5-114">To see the existing tags for a *resource group*, use:</span></span>

```azurecli
az group show -n examplegroup --query tags
```

<span data-ttu-id="09cf5-115">Skriptet returnerar följande format:</span><span class="sxs-lookup"><span data-stu-id="09cf5-115">That script returns the following format:</span></span>

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

<span data-ttu-id="09cf5-116">Om du vill visa de befintliga taggarna för en *resurs som har ett angivet resurs-ID* använder du:</span><span class="sxs-lookup"><span data-stu-id="09cf5-116">To see the existing tags for a *resource that has a specified resource ID*, use:</span></span>

```azurecli
az resource show --id {resource-id} --query tags
```

<span data-ttu-id="09cf5-117">Eller så finns de befintliga taggarna för en *resurs som har en viss grupp namn, typ och resursen*, Använd:</span><span class="sxs-lookup"><span data-stu-id="09cf5-117">Or, to see the existing tags for a *resource that has a specified name, type, and resource group*, use:</span></span>

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

<span data-ttu-id="09cf5-118">För att få resursgrupper som har en specifik tagg, Använd `az group list`:</span><span class="sxs-lookup"><span data-stu-id="09cf5-118">To get resource groups that have a specific tag, use `az group list`:</span></span>

```azurecli
az group list --tag Dept=IT
```

<span data-ttu-id="09cf5-119">Alla resurser som har en viss tagg och värdet får använda `az resource list`:</span><span class="sxs-lookup"><span data-stu-id="09cf5-119">To get all the resources that have a particular tag and value, use `az resource list`:</span></span>

```azurecli
az resource list --tag Dept=Finance
```

<span data-ttu-id="09cf5-120">Varje gång du tillämpar taggar på en resurs eller resursgrupp skriver du över resursens eller resursgruppens befintliga taggar.</span><span class="sxs-lookup"><span data-stu-id="09cf5-120">Every time you apply tags to a resource or a resource group, you overwrite the existing tags on that resource or resource group.</span></span> <span data-ttu-id="09cf5-121">Därför måste du använda ett annat tillvägagångssätt beroende på om resursen eller resursgruppen har befintliga taggar.</span><span class="sxs-lookup"><span data-stu-id="09cf5-121">Therefore, you must use a different approach based on whether the resource or resource group has existing tags.</span></span> 

<span data-ttu-id="09cf5-122">Om du vill lägga till taggar till en *resursgrupp utan befintliga taggar* använder du:</span><span class="sxs-lookup"><span data-stu-id="09cf5-122">To add tags to a *resource group without existing tags*, use:</span></span>

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

<span data-ttu-id="09cf5-123">Om du vill lägga till taggar till en *resurs utan befintliga taggar* använder du:</span><span class="sxs-lookup"><span data-stu-id="09cf5-123">To add tags to a *resource without existing tags*, use:</span></span>

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
``` 

<span data-ttu-id="09cf5-124">Hämta befintliga taggar om du vill lägga till taggar till en resurs som redan har taggar, formatera om värdet och återanvända befintliga och nya taggar:</span><span class="sxs-lookup"><span data-stu-id="09cf5-124">To add tags to a resource that already has tags, retrieve the existing tags, reformat that value, and reapply the existing and new tags:</span></span> 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

<span data-ttu-id="09cf5-125">Om du vill tillämpa alla taggar från en resursgrupp på dess resurser *utan att behålla någon av de befintliga taggarna för resurserna* kan du använda följande skript:</span><span class="sxs-lookup"><span data-stu-id="09cf5-125">To apply all tags from a resource group to its resources, and *not retain existing tags on the resources*, use the following script:</span></span>

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

<span data-ttu-id="09cf5-126">Tillämpa alla taggar från en resursgrupp till dess resurser och *behåller befintliga taggar på resurser*, använder du följande skript:</span><span class="sxs-lookup"><span data-stu-id="09cf5-126">To apply all tags from a resource group to its resources, and *retain existing tags on resources*, use the following script:</span></span>

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


## <a name="templates"></a><span data-ttu-id="09cf5-127">Mallar</span><span class="sxs-lookup"><span data-stu-id="09cf5-127">Templates</span></span>

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a><span data-ttu-id="09cf5-128">Portalen</span><span class="sxs-lookup"><span data-stu-id="09cf5-128">Portal</span></span>
[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]


## <a name="rest-api"></a><span data-ttu-id="09cf5-129">REST API</span><span class="sxs-lookup"><span data-stu-id="09cf5-129">REST API</span></span>
<span data-ttu-id="09cf5-130">Azure portal och PowerShell använder båda de [Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="09cf5-130">The Azure portal and PowerShell both use the [Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) behind the scenes.</span></span> <span data-ttu-id="09cf5-131">Om du behöver integrera taggning till en annan miljö, kan du få taggar med hjälp av **hämta** på resurs-ID och uppdatera en uppsättning taggar med hjälp av en **korrigering** anropa.</span><span class="sxs-lookup"><span data-stu-id="09cf5-131">If you need to integrate tagging into another environment, you can get tags by using **GET** on the resource ID and update the set of tags by using a **PATCH** call.</span></span>

## <a name="tags-and-billing"></a><span data-ttu-id="09cf5-132">Taggar och fakturering</span><span class="sxs-lookup"><span data-stu-id="09cf5-132">Tags and billing</span></span>
<span data-ttu-id="09cf5-133">Du kan använda taggar för att gruppera dina faktureringsinformation.</span><span class="sxs-lookup"><span data-stu-id="09cf5-133">You can use tags to group your billing data.</span></span> <span data-ttu-id="09cf5-134">Till exempel om du kör flera virtuella datorer i olika organisationer använda taggar om du vill gruppera användning av kostnadsställe.</span><span class="sxs-lookup"><span data-stu-id="09cf5-134">For example, if you are running multiple VMs for different organizations, use the tags to group usage by cost center.</span></span> <span data-ttu-id="09cf5-135">Du kan också använda taggar för att kategorisera kostnader av körningsmiljön, till exempel fakturering användning för virtuella datorer som körs i produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="09cf5-135">You can also use tags to categorize costs by runtime environment, such as the billing usage for VMs running in the production environment.</span></span>


<span data-ttu-id="09cf5-136">Du kan hämta information om taggar via den [Azure Resursanvändning och RateCard APIs](../billing/billing-usage-rate-card-overview.md) eller filen användning fil med kommaavgränsade värden (CSV).</span><span class="sxs-lookup"><span data-stu-id="09cf5-136">You can retrieve information about tags through the [Azure Resource Usage and RateCard APIs](../billing/billing-usage-rate-card-overview.md) or the usage comma-separated values (CSV) file.</span></span> <span data-ttu-id="09cf5-137">Du hämta filen från användning av [Azure kontoportalen](https://account.windowsazure.com/) eller [EA portal](https://ea.azure.com).</span><span class="sxs-lookup"><span data-stu-id="09cf5-137">You download the usage file from the [Azure account portal](https://account.windowsazure.com/) or [EA portal](https://ea.azure.com).</span></span> <span data-ttu-id="09cf5-138">Mer information om programmatisk åtkomst till faktureringsinformationen finns [få insikter om dina Microsoft Azure-resursförbrukning](../billing/billing-usage-rate-card-overview.md).</span><span class="sxs-lookup"><span data-stu-id="09cf5-138">For more information about programmatic access to billing information, see [Gain insights into your Microsoft Azure resource consumption](../billing/billing-usage-rate-card-overview.md).</span></span> <span data-ttu-id="09cf5-139">REST-API: et finns [Azure Billing REST API-referens](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).</span><span class="sxs-lookup"><span data-stu-id="09cf5-139">For REST API operations, see [Azure Billing REST API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).</span></span>


<span data-ttu-id="09cf5-140">När du hämtar användning CSV för tjänster som stöder taggar med fakturering taggarna visas i den **taggar** kolumn.</span><span class="sxs-lookup"><span data-stu-id="09cf5-140">When you download the usage CSV for services that support tags with billing, the tags appear in the **Tags** column.</span></span> <span data-ttu-id="09cf5-141">Mer information finns i [förstå fakturan för Microsoft Azure](../billing/billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="09cf5-141">For more information, see [Understand your bill for Microsoft Azure](../billing/billing-understand-your-bill.md).</span></span>

![Se taggar i fakturering](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a><span data-ttu-id="09cf5-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="09cf5-143">Next steps</span></span>
* <span data-ttu-id="09cf5-144">Du kan tillämpa begränsningar och konventioner över din prenumeration med hjälp av anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="09cf5-144">You can apply restrictions and conventions across your subscription by using customized policies.</span></span> <span data-ttu-id="09cf5-145">En princip som du definierar kan kräva att alla resurser som har ett värde för en viss tagg.</span><span class="sxs-lookup"><span data-stu-id="09cf5-145">A policy that you define might require that all resources have a value for a particular tag.</span></span> <span data-ttu-id="09cf5-146">Mer information finns i [använda principer för att hantera resurser och åtkomstkontroll](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="09cf5-146">For more information, see [Use policies to manage resources and control access](resource-manager-policy.md).</span></span>
* <span data-ttu-id="09cf5-147">En introduktion till med hjälp av Azure PowerShell när du distribuerar resurser, se [med hjälp av Azure PowerShell med Azure Resource Manager](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="09cf5-147">For an introduction to using Azure PowerShell when you're deploying resources, see [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="09cf5-148">En introduktion till med hjälp av Azure CLI när du distribuerar resurser, se [med hjälp av Azure CLI för Mac, Linux och Windows med Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="09cf5-148">For an introduction to using the Azure CLI when you're deploying resources, see [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>
* <span data-ttu-id="09cf5-149">En introduktion till med hjälp av portalen finns [hantera Azure-resurser med hjälp av Azure portal](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="09cf5-149">For an introduction to using the portal, see [Using the Azure portal to manage your Azure resources](resource-group-portal.md).</span></span>  
* <span data-ttu-id="09cf5-150">Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="09cf5-150">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

