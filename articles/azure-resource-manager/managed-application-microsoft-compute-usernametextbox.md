---
title: "aaaAzure hanterade program UserNameTextBox gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Compute.UserNameTextBox UI-element för hanterade program i Azure"
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
ms.openlocfilehash: 33092014e804c4aabd56ba49144d9cd4d6a5fd1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a><span data-ttu-id="512a8-103">Microsoft.Compute.UserNameTextBox UI-element</span><span class="sxs-lookup"><span data-stu-id="512a8-103">Microsoft.Compute.UserNameTextBox UI element</span></span>
<span data-ttu-id="512a8-104">En textruta med inbyggda verifiering för Windows och Linux-användarnamn.</span><span class="sxs-lookup"><span data-stu-id="512a8-104">A text box control with built-in validation for Windows and Linux user names.</span></span> <span data-ttu-id="512a8-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="512a8-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="512a8-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="512a8-106">UI sample</span></span>
![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a><span data-ttu-id="512a8-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="512a8-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.UserNameTextBox",
  "label": "User name",
  "defaultValue": "",
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and hello value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="512a8-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="512a8-109">Remarks</span></span>
- <span data-ttu-id="512a8-110">Om `constraints.required` har angetts för**SANT**, hello textrutan måste innehålla ett värde toovalidate har.</span><span class="sxs-lookup"><span data-stu-id="512a8-110">If `constraints.required` is set too**true**, then hello text box must contain a value toovalidate successfully.</span></span> <span data-ttu-id="512a8-111">hello standardvärdet är **SANT**.</span><span class="sxs-lookup"><span data-stu-id="512a8-111">hello default value is **true**.</span></span>
- <span data-ttu-id="512a8-112">`osPlatform`måste anges, och kan vara antingen **Windows** eller **Linux**.</span><span class="sxs-lookup"><span data-stu-id="512a8-112">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span>
- <span data-ttu-id="512a8-113">`constraints.regex`är en JavaScript-mönstret för reguljära uttryck.</span><span class="sxs-lookup"><span data-stu-id="512a8-113">`constraints.regex` is a JavaScript regular expression pattern.</span></span> <span data-ttu-id="512a8-114">Om du sedan hello textrutans värde måste matcha hello mönster toovalidate har.</span><span class="sxs-lookup"><span data-stu-id="512a8-114">If specified, then hello text box's value must match hello pattern toovalidate successfully.</span></span> <span data-ttu-id="512a8-115">Standardvärdet är **null**.</span><span class="sxs-lookup"><span data-stu-id="512a8-115">The default value is **null**.</span></span>
- <span data-ttu-id="512a8-116">`constraints.validationMessage`är en sträng toodisplay när hello textrutans värde inte hello verifiering som anges av `constraints.regex`.</span><span class="sxs-lookup"><span data-stu-id="512a8-116">`constraints.validationMessage` is a string toodisplay when hello text box's value fails hello validation specified by `constraints.regex`.</span></span> <span data-ttu-id="512a8-117">Om inget anges sedan hello textrutans inbyggda validering meddelanden används.</span><span class="sxs-lookup"><span data-stu-id="512a8-117">If not specified, then hello text box's built-in validation messages are used.</span></span> <span data-ttu-id="512a8-118">hello standardvärdet är **null**.</span><span class="sxs-lookup"><span data-stu-id="512a8-118">hello default value is **null**.</span></span>
- <span data-ttu-id="512a8-119">Det här elementet har inbyggda verifiering som baseras på hello-värde som angetts för `osPlatform`.</span><span class="sxs-lookup"><span data-stu-id="512a8-119">This element has built-in validation that is based on hello value specified for `osPlatform`.</span></span> <span data-ttu-id="512a8-120">hello inbyggda verifiering kan användas tillsammans med ett anpassat reguljärt uttryck.</span><span class="sxs-lookup"><span data-stu-id="512a8-120">hello built-in validation can be used along with a custom regular expression.</span></span>
<span data-ttu-id="512a8-121">Om ett värde för `constraints.regex` har angetts både hello inbyggda och anpassade kontroller har utlösts.</span><span class="sxs-lookup"><span data-stu-id="512a8-121">If a value for `constraints.regex` is specified, then both hello built-in and custom validations are triggered.</span></span>

## <a name="sample-output"></a><span data-ttu-id="512a8-122">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="512a8-122">Sample output</span></span>
```json
"tabrezm"
```

## <a name="next-steps"></a><span data-ttu-id="512a8-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="512a8-123">Next steps</span></span>
* <span data-ttu-id="512a8-124">En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="512a8-124">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="512a8-125">En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="512a8-125">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="512a8-126">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="512a8-126">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
