---
title: "aaaAzure hanterade program OptionsGroup gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Common.OptionsGroup UI-element för hanterade program i Azure"
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
ms.openlocfilehash: e222468009c8db567bdde9f42836a48fb791da00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a><span data-ttu-id="1a428-103">Microsoft.Common.OptionsGroup UI-element</span><span class="sxs-lookup"><span data-stu-id="1a428-103">Microsoft.Common.OptionsGroup UI element</span></span>
<span data-ttu-id="1a428-104">En markering med en rad med tillgängliga alternativ.</span><span class="sxs-lookup"><span data-stu-id="1a428-104">A selection control with a row of available options.</span></span> <span data-ttu-id="1a428-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="1a428-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="1a428-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="1a428-106">UI sample</span></span>
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a><span data-ttu-id="1a428-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="1a428-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="1a428-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="1a428-109">Remarks</span></span>
- <span data-ttu-id="1a428-110">hello etikett för `constraints.allowedValues` är hello visningstext för ett objekt och dess värde är hello utdatavärdet för hello elementet när du valt.</span><span class="sxs-lookup"><span data-stu-id="1a428-110">hello label for `constraints.allowedValues` is hello display text for an item, and its value is hello output value of hello element when selected.</span></span>
- <span data-ttu-id="1a428-111">Om anges hello standardvärdet måste vara en etikett som finns i `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="1a428-111">If specified, hello default value must be a label present in `constraints.allowedValues`.</span></span> <span data-ttu-id="1a428-112">Om inget anges hello första objektet i `constraints.allowedValues` är markerad som standard.</span><span class="sxs-lookup"><span data-stu-id="1a428-112">If not specified, hello first item in `constraints.allowedValues` is selected by default.</span></span> <span data-ttu-id="1a428-113">hello standardvärdet är **null**.</span><span class="sxs-lookup"><span data-stu-id="1a428-113">hello default value is **null**.</span></span>
- <span data-ttu-id="1a428-114">`constraints.allowedValues`måste innehålla minst ett objekt.</span><span class="sxs-lookup"><span data-stu-id="1a428-114">`constraints.allowedValues` must contain at least one item.</span></span>
- <span data-ttu-id="1a428-115">Det här elementet har inte stöd för hello `constraints.required` egendom; ett objekt måste vara valda toovalidate har.</span><span class="sxs-lookup"><span data-stu-id="1a428-115">This element doesn't support hello `constraints.required` property; an item must be selected toovalidate successfully.</span></span>

## <a name="sample-output"></a><span data-ttu-id="1a428-116">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="1a428-116">Sample output</span></span>
```json
"Bar"
```

## <a name="next-steps"></a><span data-ttu-id="1a428-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a428-117">Next steps</span></span>
* <span data-ttu-id="1a428-118">En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1a428-118">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="1a428-119">En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1a428-119">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="1a428-120">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="1a428-120">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
