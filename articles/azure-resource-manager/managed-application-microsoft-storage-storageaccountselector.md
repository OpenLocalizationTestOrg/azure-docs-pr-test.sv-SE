---
title: "aaaAzure hanterade program StorageAccountSelector gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Storage.StorageAccountSelector UI-element för hanterade program i Azure"
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
ms.openlocfilehash: a2c9545feed4c4afb3c64b30b42c94d5382a108d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a><span data-ttu-id="44e84-103">Microsoft.Storage.StorageAccountSelector UI-element</span><span class="sxs-lookup"><span data-stu-id="44e84-103">Microsoft.Storage.StorageAccountSelector UI element</span></span>
<span data-ttu-id="44e84-104">En kontroll för att välja ett nytt eller befintligt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="44e84-104">A control for selecting a new or existing storage account.</span></span> <span data-ttu-id="44e84-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="44e84-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="44e84-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="44e84-106">UI sample</span></span>
![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

## <a name="schema"></a><span data-ttu-id="44e84-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="44e84-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="44e84-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="44e84-109">Remarks</span></span>
- <span data-ttu-id="44e84-110">Om anges `defaultValue.name` verifieras automatiskt för unikhet.</span><span class="sxs-lookup"><span data-stu-id="44e84-110">If specified, `defaultValue.name` is automatically validated for uniqueness.</span></span> <span data-ttu-id="44e84-111">Om hello lagringskontonamn inte är unikt måste hello användaren ange ett annat namn eller välj ett befintligt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="44e84-111">If hello storage account name is not unique, hello user must specify a different name or choose an existing storage account.</span></span>
- <span data-ttu-id="44e84-112">Hej standardvärdet för `defaultValue.type` är **Premium_LRS**.</span><span class="sxs-lookup"><span data-stu-id="44e84-112">hello default value for `defaultValue.type` is **Premium_LRS**.</span></span>
- <span data-ttu-id="44e84-113">Någon typ som inte har angetts i `constraints.allowedTypes` är dolt och alla typer som inte har angetts i `constraints.excludedTypes` visas.</span><span class="sxs-lookup"><span data-stu-id="44e84-113">Any type not specified in `constraints.allowedTypes` is hidden, and any type not specified in `constraints.excludedTypes` is shown.</span></span>
<span data-ttu-id="44e84-114">`constraints.allowedTypes`och `constraints.excludedTypes` både valfria, men kan inte användas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="44e84-114">`constraints.allowedTypes` and `constraints.excludedTypes` are both optional, but cannot be used simultaneously.</span></span>
- <span data-ttu-id="44e84-115">Om `options.hideExisting` är **SANT**, hello användaren kan inte välja ett befintligt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="44e84-115">If `options.hideExisting` is **true**, hello user can't choose an existing storage account.</span></span> <span data-ttu-id="44e84-116">hello standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="44e84-116">hello default value is **false**.</span></span>


## <a name="sample-output"></a><span data-ttu-id="44e84-117">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="44e84-117">Sample output</span></span>
```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="next-steps"></a><span data-ttu-id="44e84-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="44e84-118">Next steps</span></span>
* <span data-ttu-id="44e84-119">En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="44e84-119">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="44e84-120">En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="44e84-120">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="44e84-121">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="44e84-121">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
