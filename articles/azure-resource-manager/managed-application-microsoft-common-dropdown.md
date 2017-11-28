---
title: "Azure hanterade program DropDown gränssnittselement | Microsoft Docs"
description: "Beskriver Microsoft.Common.DropDown UI-element för hanterade program i Azure"
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
ms.openlocfilehash: a769e14efbae928b811fa1f1b1c2d4fba3c7692b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommondropdown-ui-element"></a><span data-ttu-id="6a0c2-103">Microsoft.Common.DropDown UI-element</span><span class="sxs-lookup"><span data-stu-id="6a0c2-103">Microsoft.Common.DropDown UI element</span></span>
<span data-ttu-id="6a0c2-104">En markering med en listruta.</span><span class="sxs-lookup"><span data-stu-id="6a0c2-104">A selection control with a dropdown list.</span></span> <span data-ttu-id="6a0c2-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="6a0c2-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="6a0c2-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="6a0c2-106">UI sample</span></span>
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a><span data-ttu-id="6a0c2-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="6a0c2-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="6a0c2-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="6a0c2-109">Remarks</span></span>
- <span data-ttu-id="6a0c2-110">Etikett för `constraints.allowedValues` är texten som visas för ett objekt och dess värde är värdet av elementet när du valt.</span><span class="sxs-lookup"><span data-stu-id="6a0c2-110">The label for `constraints.allowedValues` is the display text for an item, and its value is the output value of the element when selected.</span></span>
- <span data-ttu-id="6a0c2-111">Om du standardvärdet måste vara en etikett som finns i `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="6a0c2-111">If specified, the default value must be a label present in `constraints.allowedValues`.</span></span> <span data-ttu-id="6a0c2-112">Om inget anges det första objektet i `constraints.allowedValues` är markerad.</span><span class="sxs-lookup"><span data-stu-id="6a0c2-112">If not specified, the first item in `constraints.allowedValues` is selected.</span></span> <span data-ttu-id="6a0c2-113">Standardvärdet är **null**.</span><span class="sxs-lookup"><span data-stu-id="6a0c2-113">The default value is **null**.</span></span>
- <span data-ttu-id="6a0c2-114">`constraints.allowedValues`måste innehålla minst ett objekt.</span><span class="sxs-lookup"><span data-stu-id="6a0c2-114">`constraints.allowedValues` must contain at least one item.</span></span>
- <span data-ttu-id="6a0c2-115">Det här elementet har inte stöd för den `constraints.required` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="6a0c2-115">This element doesn't support the `constraints.required` property.</span></span> <span data-ttu-id="6a0c2-116">Emuleras problemet genom att lägga till ett objekt med en etikett och värdet för `""` (tom sträng) till `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="6a0c2-116">To emulate this behavior, add an item with a label and value of `""` (empty string) to `constraints.allowedValues`.</span></span>

## <a name="sample-output"></a><span data-ttu-id="6a0c2-117">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="6a0c2-117">Sample output</span></span>
```json
"Bar"
```

## <a name="next-steps"></a><span data-ttu-id="6a0c2-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a0c2-118">Next steps</span></span>
* <span data-ttu-id="6a0c2-119">En introduktion till hanterade program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6a0c2-119">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="6a0c2-120">En introduktion till att skapa UI-definitioner, se [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6a0c2-120">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="6a0c2-121">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="6a0c2-121">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
