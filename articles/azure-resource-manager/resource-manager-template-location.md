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
# <a name="set-resource-location-in-azure-resource-manager-templates"></a><span data-ttu-id="71331-103">Ange resursplats i Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="71331-103">Set resource location in Azure Resource Manager templates</span></span>
<span data-ttu-id="71331-104">När du distribuerar en mall måste du ange en plats för varje resurs.</span><span class="sxs-lookup"><span data-stu-id="71331-104">When deploying a template, you must provide a location for each resource.</span></span> <span data-ttu-id="71331-105">Det här avsnittet visar hur toodetermine hello platser som är tillgängliga tooyour prenumerationen för varje resurs skriver.</span><span class="sxs-lookup"><span data-stu-id="71331-105">This topic shows how toodetermine hello locations that are available tooyour subscription for each resource type.</span></span>

## <a name="determine-supported-locations"></a><span data-ttu-id="71331-106">Fastställa platser som stöds</span><span class="sxs-lookup"><span data-stu-id="71331-106">Determine supported locations</span></span>

<span data-ttu-id="71331-107">En fullständig lista över platser som stöds för varje resurstyp av finns [produkter som är tillgängliga efter region](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="71331-107">For a complete list of supported locations for each resource type, see [Products available by region](https://azure.microsoft.com/regions/services/).</span></span> <span data-ttu-id="71331-108">Prenumerationen kanske inte har åtkomst tooall hello platser i listan.</span><span class="sxs-lookup"><span data-stu-id="71331-108">However, your subscription might not have access tooall hello locations in that list.</span></span> <span data-ttu-id="71331-109">toosee en egen lista över platser som är tillgängliga tooyour prenumeration, använder du Azure PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="71331-109">toosee a customized list of locations that are available tooyour subscription, use Azure PowerShell or Azure CLI.</span></span> 

<span data-ttu-id="71331-110">hello följande exempel används PowerShell tooget hello platser för hello `Microsoft.Web\sites` resurstyp:</span><span class="sxs-lookup"><span data-stu-id="71331-110">hello following example uses PowerShell tooget hello locations for hello `Microsoft.Web\sites` resource type:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

<span data-ttu-id="71331-111">hello följande exempel använder Azure CLI 2.0 tooget hello platser för hello `Microsoft.Web\sites` resurstyp:</span><span class="sxs-lookup"><span data-stu-id="71331-111">hello following example uses Azure CLI 2.0 tooget hello locations for hello `Microsoft.Web\sites` resource type:</span></span>

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="set-location-in-template"></a><span data-ttu-id="71331-112">Ange plats i mallen</span><span class="sxs-lookup"><span data-stu-id="71331-112">Set location in template</span></span>

<span data-ttu-id="71331-113">När du fastställt hello stöds platser för dina resurser, måste tooset där i mallen.</span><span class="sxs-lookup"><span data-stu-id="71331-113">After determining hello supported locations for your resources, you need tooset that location in your template.</span></span> <span data-ttu-id="71331-114">Hej enklaste sättet tooset det här värdet är en resursgrupp i en plats som stöder hello resurstyper toocreate och ange varje plats för`[resourceGroup().location]`.</span><span class="sxs-lookup"><span data-stu-id="71331-114">hello easiest way tooset this value is toocreate a resource group in a location that supports hello resource types, and set each location too`[resourceGroup().location]`.</span></span> <span data-ttu-id="71331-115">Du kan distribuera hello mallen tooresource grupper på olika platser och inte ändra alla värden i hello mall eller parametrar.</span><span class="sxs-lookup"><span data-stu-id="71331-115">You can redeploy hello template tooresource groups in different locations, and not change any values in hello template or parameters.</span></span> 

<span data-ttu-id="71331-116">hello följande exempel visas ett lagringskonto som är distribuerade toohello samma plats som hello resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="71331-116">hello following example shows a storage account that is deployed toohello same location as hello resource group:</span></span>

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

<span data-ttu-id="71331-117">Om du behöver toohardcode hello plats i mallen, ange hello namnet på en av hello stöds regioner.</span><span class="sxs-lookup"><span data-stu-id="71331-117">If you need toohardcode hello location in your template, provide hello name of one of hello supported regions.</span></span> <span data-ttu-id="71331-118">hello som följande exempel visar ett lagringskonto som alltid är distribuerat tooNorth centrala USA:</span><span class="sxs-lookup"><span data-stu-id="71331-118">hello following example shows a storage account that is always deployed tooNorth Central US:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="71331-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="71331-119">Next steps</span></span>
* <span data-ttu-id="71331-120">Rekommendationer om hur toocreate mallar, se [bästa praxis för att skapa mallar för Azure Resource Manager](resource-manager-template-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="71331-120">For recommendations about how toocreate templates, see [Best practices for creating Azure Resource Manager templates](resource-manager-template-best-practices.md).</span></span>

