---
title: "aaaAzure hanterade program avsnittet gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Common.Section UI-element för hanterade program i Azure"
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
ms.openlocfilehash: d20b365b12fab66177e1a12db2ebbeefe507212e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommonsection-ui-element"></a><span data-ttu-id="5f004-103">Microsoft.Common.Section UI-element</span><span class="sxs-lookup"><span data-stu-id="5f004-103">Microsoft.Common.Section UI element</span></span>
<span data-ttu-id="5f004-104">En kontroll som grupperar ett eller flera element under en rubrik.</span><span class="sxs-lookup"><span data-stu-id="5f004-104">A control that groups one or more elements under a heading.</span></span> <span data-ttu-id="5f004-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="5f004-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="5f004-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="5f004-106">UI sample</span></span>
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a><span data-ttu-id="5f004-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="5f004-108">Schema</span></span>
```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Some section",
  "elements": [
    {
      "name": "element1",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 1"
    },
    {
      "name": "element2",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="5f004-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="5f004-109">Remarks</span></span>
- <span data-ttu-id="5f004-110">`elements`måste innehålla minst ett element och kan innehålla alla elementtyper utom `Microsoft.Common.Section`.</span><span class="sxs-lookup"><span data-stu-id="5f004-110">`elements` must contain at least one element, and can contain all element types except `Microsoft.Common.Section`.</span></span>
- <span data-ttu-id="5f004-111">Det här elementet har inte stöd för hello `toolTip` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="5f004-111">This element doesn't support hello `toolTip` property.</span></span>

## <a name="sample-output"></a><span data-ttu-id="5f004-112">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="5f004-112">Sample output</span></span>
<span data-ttu-id="5f004-113">tooaccess hello utdata värdena för element i `elements`, använda hello [basics()](managed-application-createuidefinition-functions.md#basics) eller [steps()](managed-application-createuidefinition-functions.md#steps) funktioner och punktnotation:</span><span class="sxs-lookup"><span data-stu-id="5f004-113">tooaccess hello output values of elements in `elements`, use hello [basics()](managed-application-createuidefinition-functions.md#basics) or [steps()](managed-application-createuidefinition-functions.md#steps) functions and dot notation:</span></span>

```json
basics('section1').element1
```

<span data-ttu-id="5f004-114">Element av typen `Microsoft.Common.Section` har inga utdata värdena.</span><span class="sxs-lookup"><span data-stu-id="5f004-114">Elements of type `Microsoft.Common.Section` have no output values themselves.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5f004-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5f004-115">Next steps</span></span>
* <span data-ttu-id="5f004-116">En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5f004-116">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="5f004-117">En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5f004-117">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="5f004-118">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="5f004-118">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
