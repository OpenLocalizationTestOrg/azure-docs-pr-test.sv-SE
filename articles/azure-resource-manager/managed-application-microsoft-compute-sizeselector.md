---
title: "aaaAzure hanterade program SizeSelector gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Compute.SizeSelector UI-element för hanterade program i Azure"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: d93306135d9c6f9a83692766ce1ca7ea2b688086
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcomputesizeselector-ui-element"></a><span data-ttu-id="c0a88-103">Microsoft.Compute.SizeSelector UI-element</span><span class="sxs-lookup"><span data-stu-id="c0a88-103">Microsoft.Compute.SizeSelector UI element</span></span>
<span data-ttu-id="c0a88-104">En kontroll för att välja en storlek för en eller flera instanser av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c0a88-104">A control for selecting a size for one or more virtual machine instances.</span></span> <span data-ttu-id="c0a88-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="c0a88-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="c0a88-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="c0a88-106">UI sample</span></span>
![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

## <a name="schema"></a><span data-ttu-id="c0a88-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="c0a88-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": []
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="c0a88-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="c0a88-109">Remarks</span></span>
- <span data-ttu-id="c0a88-110">`recommendedSizes`ska innehålla minst en storlek.</span><span class="sxs-lookup"><span data-stu-id="c0a88-110">`recommendedSizes` should contain at least one size.</span></span> <span data-ttu-id="c0a88-111">hello bör först storlek används som standard för hello.</span><span class="sxs-lookup"><span data-stu-id="c0a88-111">hello first recommended size is used as hello default.</span></span>
- <span data-ttu-id="c0a88-112">Om en rekommenderade storleken inte är tillgänglig i hello valt plats, hello storlek automatiskt att hoppas över.</span><span class="sxs-lookup"><span data-stu-id="c0a88-112">If a recommended size is not available in hello selected location, hello size is automatically skipped.</span></span> <span data-ttu-id="c0a88-113">I stället används hello nästa rekommenderade storleken.</span><span class="sxs-lookup"><span data-stu-id="c0a88-113">Instead, hello next recommended size is used.</span></span>
- <span data-ttu-id="c0a88-114">Oavsett storlek inte har angetts i hello `constraints.allowedSizes` är dolt och valfri storlek inte har angetts i `constraints.excludedSizes` visas.</span><span class="sxs-lookup"><span data-stu-id="c0a88-114">Any size not specified in hello `constraints.allowedSizes` is hidden, and any size not specified in `constraints.excludedSizes` is shown.</span></span>
<span data-ttu-id="c0a88-115">`constraints.allowedSizes`och `constraints.excludedSizes` både valfria, men kan inte användas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c0a88-115">`constraints.allowedSizes` and `constraints.excludedSizes` are both optional, but cannot be used simultaneously.</span></span> <span data-ttu-id="c0a88-116">hello lista över tillgängliga storlekar kan fastställas genom att anropa [visa en lista med tillgängliga virtuella datorstorlekar för en prenumeration](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).</span><span class="sxs-lookup"><span data-stu-id="c0a88-116">hello list of available sizes can be determined by calling [List available virtual machine sizes for a subscription](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).</span></span>
- <span data-ttu-id="c0a88-117">`osPlatform`måste anges, och kan vara antingen **Windows** eller **Linux**.</span><span class="sxs-lookup"><span data-stu-id="c0a88-117">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span> <span data-ttu-id="c0a88-118">Den används toodetermine hello maskinvarukostnader hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c0a88-118">It's used toodetermine hello hardware costs of hello virtual machines.</span></span>
- <span data-ttu-id="c0a88-119">`imageReference`tas bort för från första part bilder, men det angivna för bilder från tredje part.</span><span class="sxs-lookup"><span data-stu-id="c0a88-119">`imageReference` is omitted for first-party images, but provided for third-party images.</span></span> <span data-ttu-id="c0a88-120">Den används toodetermine hello programvarukostnader hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c0a88-120">It's used toodetermine hello software costs of hello virtual machines.</span></span>
- <span data-ttu-id="c0a88-121">`count`är används tooset hello lämpliga multiplikatorn för hello-element.</span><span class="sxs-lookup"><span data-stu-id="c0a88-121">`count` is used tooset hello appropriate multiplier for hello element.</span></span> <span data-ttu-id="c0a88-122">Den stöder ett statiskt värde som **2**, eller ett dynamiskt värde från ett annat element som `[steps('step1').vmCount]`.</span><span class="sxs-lookup"><span data-stu-id="c0a88-122">It supports a static value, like **2**, or a dynamic value from another element, like `[steps('step1').vmCount]`.</span></span> <span data-ttu-id="c0a88-123">hello standardvärdet är **1**.</span><span class="sxs-lookup"><span data-stu-id="c0a88-123">hello default value is **1**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="c0a88-124">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="c0a88-124">Sample output</span></span>
```json
"Standard_D1"
```

## <a name="next-steps"></a><span data-ttu-id="c0a88-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c0a88-125">Next steps</span></span>
* <span data-ttu-id="c0a88-126">En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c0a88-126">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="c0a88-127">En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c0a88-127">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="c0a88-128">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="c0a88-128">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
