---
title: "Azure hanterade program MultiStorageAccountCombo gränssnittselement | Microsoft Docs"
description: "Beskriver Microsoft.Storage.MultiStorageAccountCombo UI-element för hanterade program i Azure"
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
ms.openlocfilehash: 27843b116d949899e4eae65f342324f77ebca70b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a><span data-ttu-id="27651-103">Microsoft.Storage.MultiStorageAccountCombo UI-element</span><span class="sxs-lookup"><span data-stu-id="27651-103">Microsoft.Storage.MultiStorageAccountCombo UI element</span></span>
<span data-ttu-id="27651-104">En grupp av kontroller för att skapa flera lagringskonton med namn som börjar med en gemensam prefix.</span><span class="sxs-lookup"><span data-stu-id="27651-104">A group of controls for creating multiple storage accounts, with names that start with a common prefix.</span></span> <span data-ttu-id="27651-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="27651-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="27651-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="27651-106">UI sample</span></span>
![Microsoft.Storage.MultiStorageAccountCombo](./media/managed-application-elements/microsoft.storage.multistorageaccountcombo.png)

## <a name="schema"></a><span data-ttu-id="27651-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="27651-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.MultiStorageAccountCombo",
  "label": {
    "prefix": "Storage account prefix",
    "type": "Storage account type"
  },
  "toolTip": {
    "prefix": "",
    "type": ""
  },
  "defaultValue": {
    "prefix": "sa",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="27651-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="27651-109">Remarks</span></span>
- <span data-ttu-id="27651-110">Värdet för `defaultValue.prefix` är sammanfogat med en eller flera heltal att generera sekvens av lagringskontonamn.</span><span class="sxs-lookup"><span data-stu-id="27651-110">The value for `defaultValue.prefix` is concatenated with one or more integers to generate the sequence of storage account names.</span></span> <span data-ttu-id="27651-111">Till exempel om `defaultValue.prefix` är **foobar** och `count` är **2**, sedan lagringskontonamn **foobar1** och **foobar2** genereras.</span><span class="sxs-lookup"><span data-stu-id="27651-111">For example, if `defaultValue.prefix` is **foobar** and `count` is **2**, then storage account names **foobar1** and **foobar2** are generated.</span></span> <span data-ttu-id="27651-112">Genererade lagringskontonamn verifiera för unika automatiskt.</span><span class="sxs-lookup"><span data-stu-id="27651-112">Generated storage account names are validated for uniqueness automatically.</span></span>
- <span data-ttu-id="27651-113">Lagringskontonamn genereras lexicographically baserat på `count`.</span><span class="sxs-lookup"><span data-stu-id="27651-113">The storage account names are generated lexicographically based on `count`.</span></span> <span data-ttu-id="27651-114">Till exempel om `count` är 10 och lagringskontonamn avslutas med 2-siffrig heltal (01, 02, 03, etc.).</span><span class="sxs-lookup"><span data-stu-id="27651-114">For example, if `count` is 10, then the storage account names end with 2-digit integers (01, 02, 03, etc.).</span></span>
- <span data-ttu-id="27651-115">Standardvärdet för `defaultValue.prefix` är **null**, och för `defaultValue.type` är **Premium_LRS**.</span><span class="sxs-lookup"><span data-stu-id="27651-115">The default value for `defaultValue.prefix` is **null**, and for `defaultValue.type` is **Premium_LRS**.</span></span>
- <span data-ttu-id="27651-116">Någon typ som inte har angetts i `constraints.allowedTypes` är dolt och alla typer som inte har angetts i `constraints.excludedTypes` visas.</span><span class="sxs-lookup"><span data-stu-id="27651-116">Any type not specified in `constraints.allowedTypes` is hidden, and any type not specified in `constraints.excludedTypes` is shown.</span></span>
<span data-ttu-id="27651-117">`constraints.allowedTypes`och `constraints.excludedTypes` både valfria, men kan inte användas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="27651-117">`constraints.allowedTypes` and `constraints.excludedTypes` are both optional, but cannot be used simultaneously.</span></span>
- <span data-ttu-id="27651-118">Förutom att generera lagringskontonamn, `count` används för att ange lämplig multiplikatorn för elementet.</span><span class="sxs-lookup"><span data-stu-id="27651-118">In addition to generating storage account names, `count` is used to set the appropriate multiplier for the element.</span></span> <span data-ttu-id="27651-119">Den stöder ett statiskt värde som **2**, eller ett dynamiskt värde från ett annat element som `[steps('step1').storageAccountCount]`.</span><span class="sxs-lookup"><span data-stu-id="27651-119">It supports a static value, like **2**, or a dynamic value from another element, like `[steps('step1').storageAccountCount]`.</span></span> <span data-ttu-id="27651-120">Standardvärdet är **1**.</span><span class="sxs-lookup"><span data-stu-id="27651-120">The default value is **1**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="27651-121">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="27651-121">Sample output</span></span>
```json
{
  "prefix": "sa",
  "count": 2,
  "resourceGroup": "rg01",
  "type": "Premium_LRS"
}
```

## <a name="next-steps"></a><span data-ttu-id="27651-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="27651-122">Next steps</span></span>
* <span data-ttu-id="27651-123">En introduktion till hanterade program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="27651-123">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="27651-124">En introduktion till att skapa UI-definitioner, se [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="27651-124">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="27651-125">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="27651-125">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
