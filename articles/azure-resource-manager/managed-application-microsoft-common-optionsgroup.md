---
title: "Azure hanterade program OptionsGroup gränssnittselement | Microsoft Docs"
description: "Beskriver Microsoft.Common.OptionsGroup UI-element för hanterade program i Azure"
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
ms.openlocfilehash: 6e147ed28c8248f7f17cb36fd7ae13468141dced
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a><span data-ttu-id="4c6f3-103">Microsoft.Common.OptionsGroup UI-element</span><span class="sxs-lookup"><span data-stu-id="4c6f3-103">Microsoft.Common.OptionsGroup UI element</span></span>
<span data-ttu-id="4c6f3-104">En markering med en rad med tillgängliga alternativ.</span><span class="sxs-lookup"><span data-stu-id="4c6f3-104">A selection control with a row of available options.</span></span> <span data-ttu-id="4c6f3-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="4c6f3-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="4c6f3-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="4c6f3-106">UI sample</span></span>
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a><span data-ttu-id="4c6f3-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="4c6f3-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Common.OptionsGroup",
  "label": "Some options group",
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

## <a name="remarks"></a><span data-ttu-id="4c6f3-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="4c6f3-109">Remarks</span></span>
- <span data-ttu-id="4c6f3-110">Etikett för `constraints.allowedValues` är texten som visas för ett objekt och dess värde är värdet av elementet när du valt.</span><span class="sxs-lookup"><span data-stu-id="4c6f3-110">The label for `constraints.allowedValues` is the display text for an item, and its value is the output value of the element when selected.</span></span>
- <span data-ttu-id="4c6f3-111">Om du standardvärdet måste vara en etikett som finns i `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="4c6f3-111">If specified, the default value must be a label present in `constraints.allowedValues`.</span></span> <span data-ttu-id="4c6f3-112">Om inget anges det första objektet i `constraints.allowedValues` är markerad som standard.</span><span class="sxs-lookup"><span data-stu-id="4c6f3-112">If not specified, the first item in `constraints.allowedValues` is selected by default.</span></span> <span data-ttu-id="4c6f3-113">Standardvärdet är **null**.</span><span class="sxs-lookup"><span data-stu-id="4c6f3-113">The default value is **null**.</span></span>
- <span data-ttu-id="4c6f3-114">`constraints.allowedValues`måste innehålla minst ett objekt.</span><span class="sxs-lookup"><span data-stu-id="4c6f3-114">`constraints.allowedValues` must contain at least one item.</span></span>
- <span data-ttu-id="4c6f3-115">Det här elementet har inte stöd för den `constraints.required` egendom; du måste välja ett objekt kan valideras.</span><span class="sxs-lookup"><span data-stu-id="4c6f3-115">This element doesn't support the `constraints.required` property; an item must be selected to validate successfully.</span></span>

## <a name="sample-output"></a><span data-ttu-id="4c6f3-116">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="4c6f3-116">Sample output</span></span>
```json
"Bar"
```

## <a name="next-steps"></a><span data-ttu-id="4c6f3-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4c6f3-117">Next steps</span></span>
* <span data-ttu-id="4c6f3-118">En introduktion till hanterade program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4c6f3-118">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="4c6f3-119">En introduktion till att skapa UI-definitioner, se [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4c6f3-119">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="4c6f3-120">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="4c6f3-120">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
