---
title: "aaaAzure hanterade program textruta gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Common.TextBox UI-element för hanterade program i Azure"
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
ms.openlocfilehash: 11771cd1d689b720384df98b8d1465703068af37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommontextbox-ui-element"></a><span data-ttu-id="a6dfc-103">Microsoft.Common.TextBox UI-element</span><span class="sxs-lookup"><span data-stu-id="a6dfc-103">Microsoft.Common.TextBox UI element</span></span>
<span data-ttu-id="a6dfc-104">En kontroll som kan använda tooedit oformaterad text.</span><span class="sxs-lookup"><span data-stu-id="a6dfc-104">A control that can be used tooedit unformatted text.</span></span> <span data-ttu-id="a6dfc-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="a6dfc-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="a6dfc-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="a6dfc-106">UI sample</span></span>
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a><span data-ttu-id="a6dfc-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="a6dfc-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Halp!",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and hello value must be 1-30 characters long."
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="a6dfc-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="a6dfc-109">Remarks</span></span>
- <span data-ttu-id="a6dfc-110">Om `constraints.required` har angetts för**SANT**, hello textrutan måste innehålla ett värde toovalidate har.</span><span class="sxs-lookup"><span data-stu-id="a6dfc-110">If `constraints.required` is set too**true**, then hello text box must contain a value toovalidate successfully.</span></span> <span data-ttu-id="a6dfc-111">hello standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="a6dfc-111">hello default value is **false**.</span></span>
- <span data-ttu-id="a6dfc-112">`constraints.regex`är en JavaScript-mönstret för reguljära uttryck.</span><span class="sxs-lookup"><span data-stu-id="a6dfc-112">`constraints.regex` is a JavaScript regular expression pattern.</span></span> <span data-ttu-id="a6dfc-113">Om du sedan hello textrutans värde måste matcha hello mönster toovalidate har.</span><span class="sxs-lookup"><span data-stu-id="a6dfc-113">If specified, then hello text box's value must match hello pattern toovalidate successfully.</span></span> <span data-ttu-id="a6dfc-114">Standardvärdet är **null**.</span><span class="sxs-lookup"><span data-stu-id="a6dfc-114">The default value is **null**.</span></span>
- <span data-ttu-id="a6dfc-115">`constraints.validationMessage`är en sträng toodisplay när hello textrutans värde inte klarar valideringen.</span><span class="sxs-lookup"><span data-stu-id="a6dfc-115">`constraints.validationMessage` is a string toodisplay when hello text box's value fails validation.</span></span> <span data-ttu-id="a6dfc-116">Om inget anges sedan hello textrutans inbyggda validering meddelanden används.</span><span class="sxs-lookup"><span data-stu-id="a6dfc-116">If not specified, then hello text box's built-in validation messages are used.</span></span> <span data-ttu-id="a6dfc-117">hello standardvärdet är **null**.</span><span class="sxs-lookup"><span data-stu-id="a6dfc-117">hello default value is **null**.</span></span>
- <span data-ttu-id="a6dfc-118">Det är möjligt toospecify ett värde för `constraints.regex` när `constraints.required` har angetts för**FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="a6dfc-118">It's possible toospecify a value for `constraints.regex` when `constraints.required` is set too**false**.</span></span> <span data-ttu-id="a6dfc-119">I det här scenariot krävs ett värde inte för hello text rutan toovalidate har.</span><span class="sxs-lookup"><span data-stu-id="a6dfc-119">In this scenario, a value is not required for hello text box toovalidate successfully.</span></span> <span data-ttu-id="a6dfc-120">Om en anges måste den matcha hello mönstret för reguljära uttryck.</span><span class="sxs-lookup"><span data-stu-id="a6dfc-120">If one is specified, it must match hello regular expression pattern.</span></span>

## <a name="sample-output"></a><span data-ttu-id="a6dfc-121">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="a6dfc-121">Sample output</span></span>

```json
"foobar"
```

## <a name="next-steps"></a><span data-ttu-id="a6dfc-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a6dfc-122">Next steps</span></span>
* <span data-ttu-id="a6dfc-123">En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a6dfc-123">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="a6dfc-124">En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a6dfc-124">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="a6dfc-125">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="a6dfc-125">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
