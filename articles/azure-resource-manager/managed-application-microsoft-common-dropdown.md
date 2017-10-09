---
title: "aaaAzure hanterade program DropDown gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Common.DropDown UI-element för hanterade program i Azure"
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
ms.openlocfilehash: 1c07a48ad66b8e8b7fd8f59561776ecb1fc6224f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommondropdown-ui-element"></a><span data-ttu-id="baf8b-103">Microsoft.Common.DropDown UI-element</span><span class="sxs-lookup"><span data-stu-id="baf8b-103">Microsoft.Common.DropDown UI element</span></span>
<span data-ttu-id="baf8b-104">En markering med en listruta.</span><span class="sxs-lookup"><span data-stu-id="baf8b-104">A selection control with a dropdown list.</span></span> <span data-ttu-id="baf8b-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="baf8b-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="baf8b-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="baf8b-106">UI sample</span></span>
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a><span data-ttu-id="baf8b-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="baf8b-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Some drop down",
  "defaultValue": "Foo",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Foo",
        "value": "Bar"
      },
      {
        "label": "Baz",
        "value": "Qux"
      }
    ]
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="baf8b-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="baf8b-109">Remarks</span></span>
- <span data-ttu-id="baf8b-110">hello etikett för `constraints.allowedValues` är hello visningstext för ett objekt och dess värde är hello utdatavärdet för hello elementet när du valt.</span><span class="sxs-lookup"><span data-stu-id="baf8b-110">hello label for `constraints.allowedValues` is hello display text for an item, and its value is hello output value of hello element when selected.</span></span>
- <span data-ttu-id="baf8b-111">Om anges hello standardvärdet måste vara en etikett som finns i `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="baf8b-111">If specified, hello default value must be a label present in `constraints.allowedValues`.</span></span> <span data-ttu-id="baf8b-112">Om inget anges hello första objektet i `constraints.allowedValues` är markerad.</span><span class="sxs-lookup"><span data-stu-id="baf8b-112">If not specified, hello first item in `constraints.allowedValues` is selected.</span></span> <span data-ttu-id="baf8b-113">hello standardvärdet är **null**.</span><span class="sxs-lookup"><span data-stu-id="baf8b-113">hello default value is **null**.</span></span>
- <span data-ttu-id="baf8b-114">`constraints.allowedValues`måste innehålla minst ett objekt.</span><span class="sxs-lookup"><span data-stu-id="baf8b-114">`constraints.allowedValues` must contain at least one item.</span></span>
- <span data-ttu-id="baf8b-115">Det här elementet har inte stöd för hello `constraints.required` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="baf8b-115">This element doesn't support hello `constraints.required` property.</span></span> <span data-ttu-id="baf8b-116">tooemulate problemet, lägga till ett objekt med en etikett och värdet för `""` (tom sträng) för`constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="baf8b-116">tooemulate this behavior, add an item with a label and value of `""` (empty string) too`constraints.allowedValues`.</span></span>

## <a name="sample-output"></a><span data-ttu-id="baf8b-117">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="baf8b-117">Sample output</span></span>
```json
"Bar"
```

## <a name="next-steps"></a><span data-ttu-id="baf8b-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="baf8b-118">Next steps</span></span>
* <span data-ttu-id="baf8b-119">En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="baf8b-119">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="baf8b-120">En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="baf8b-120">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="baf8b-121">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="baf8b-121">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
