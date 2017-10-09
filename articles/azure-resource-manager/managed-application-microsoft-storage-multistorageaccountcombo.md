---
title: "aaaAzure hanterade program MultiStorageAccountCombo gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Storage.MultiStorageAccountCombo UI-element för hanterade program i Azure"
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
ms.openlocfilehash: 765be145b61c3dbf0a035a7a00aa18eee464a3eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a><span data-ttu-id="df2e0-103">Microsoft.Storage.MultiStorageAccountCombo UI-element</span><span class="sxs-lookup"><span data-stu-id="df2e0-103">Microsoft.Storage.MultiStorageAccountCombo UI element</span></span>
<span data-ttu-id="df2e0-104">En grupp av kontroller för att skapa flera lagringskonton med namn som börjar med en gemensam prefix.</span><span class="sxs-lookup"><span data-stu-id="df2e0-104">A group of controls for creating multiple storage accounts, with names that start with a common prefix.</span></span> <span data-ttu-id="df2e0-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="df2e0-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="df2e0-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="df2e0-106">UI sample</span></span>
![Microsoft.Storage.MultiStorageAccountCombo](./media/managed-application-elements/microsoft.storage.multistorageaccountcombo.png)

## <a name="schema"></a><span data-ttu-id="df2e0-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="df2e0-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="df2e0-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="df2e0-109">Remarks</span></span>
- <span data-ttu-id="df2e0-110">Hej värdet för `defaultValue.prefix` är sammanfogat med en eller flera heltal toogenerate hello sekvens av lagringskontonamn.</span><span class="sxs-lookup"><span data-stu-id="df2e0-110">hello value for `defaultValue.prefix` is concatenated with one or more integers toogenerate hello sequence of storage account names.</span></span> <span data-ttu-id="df2e0-111">Till exempel om `defaultValue.prefix` är **foobar** och `count` är **2**, sedan lagringskontonamn **foobar1** och **foobar2** genereras.</span><span class="sxs-lookup"><span data-stu-id="df2e0-111">For example, if `defaultValue.prefix` is **foobar** and `count` is **2**, then storage account names **foobar1** and **foobar2** are generated.</span></span> <span data-ttu-id="df2e0-112">Genererade lagringskontonamn verifiera för unika automatiskt.</span><span class="sxs-lookup"><span data-stu-id="df2e0-112">Generated storage account names are validated for uniqueness automatically.</span></span>
- <span data-ttu-id="df2e0-113">Hej lagringskontonamn genereras lexicographically baserat på `count`.</span><span class="sxs-lookup"><span data-stu-id="df2e0-113">hello storage account names are generated lexicographically based on `count`.</span></span> <span data-ttu-id="df2e0-114">Till exempel om `count` är 10 sedan hello lagringskontonamn avslutas med 2-siffrig heltal (01, 02, 03, etc.).</span><span class="sxs-lookup"><span data-stu-id="df2e0-114">For example, if `count` is 10, then hello storage account names end with 2-digit integers (01, 02, 03, etc.).</span></span>
- <span data-ttu-id="df2e0-115">Hej standardvärdet för `defaultValue.prefix` är **null**, och för `defaultValue.type` är **Premium_LRS**.</span><span class="sxs-lookup"><span data-stu-id="df2e0-115">hello default value for `defaultValue.prefix` is **null**, and for `defaultValue.type` is **Premium_LRS**.</span></span>
- <span data-ttu-id="df2e0-116">Någon typ som inte har angetts i `constraints.allowedTypes` är dolt och alla typer som inte har angetts i `constraints.excludedTypes` visas.</span><span class="sxs-lookup"><span data-stu-id="df2e0-116">Any type not specified in `constraints.allowedTypes` is hidden, and any type not specified in `constraints.excludedTypes` is shown.</span></span>
<span data-ttu-id="df2e0-117">`constraints.allowedTypes`och `constraints.excludedTypes` både valfria, men kan inte användas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="df2e0-117">`constraints.allowedTypes` and `constraints.excludedTypes` are both optional, but cannot be used simultaneously.</span></span>
- <span data-ttu-id="df2e0-118">I tillägg toogenerating lagringskontonamn, `count` är används tooset lämplig multiplikatorn för hello-elementet.</span><span class="sxs-lookup"><span data-stu-id="df2e0-118">In addition toogenerating storage account names, `count` is used tooset the appropriate multiplier for hello element.</span></span> <span data-ttu-id="df2e0-119">Den stöder ett statiskt värde som **2**, eller ett dynamiskt värde från ett annat element som `[steps('step1').storageAccountCount]`.</span><span class="sxs-lookup"><span data-stu-id="df2e0-119">It supports a static value, like **2**, or a dynamic value from another element, like `[steps('step1').storageAccountCount]`.</span></span> <span data-ttu-id="df2e0-120">hello standardvärdet är **1**.</span><span class="sxs-lookup"><span data-stu-id="df2e0-120">hello default value is **1**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="df2e0-121">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="df2e0-121">Sample output</span></span>
```json
{
  "prefix": "sa",
  "count": 2,
  "resourceGroup": "rg01",
  "type": "Premium_LRS"
}
```

## <a name="next-steps"></a><span data-ttu-id="df2e0-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="df2e0-122">Next steps</span></span>
* <span data-ttu-id="df2e0-123">En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="df2e0-123">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="df2e0-124">En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="df2e0-124">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="df2e0-125">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="df2e0-125">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
