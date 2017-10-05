---
title: "Azure hanterade program UserNameTextBox gränssnittselement | Microsoft Docs"
description: "Beskriver Microsoft.Compute.UserNameTextBox UI-element för hanterade program i Azure"
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
ms.openlocfilehash: c90be5a0ed3aadda81d7ec9b5388a96472f69af0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a><span data-ttu-id="d8f31-103">Microsoft.Compute.UserNameTextBox UI-element</span><span class="sxs-lookup"><span data-stu-id="d8f31-103">Microsoft.Compute.UserNameTextBox UI element</span></span>
<span data-ttu-id="d8f31-104">En textruta med inbyggda verifiering för Windows och Linux-användarnamn.</span><span class="sxs-lookup"><span data-stu-id="d8f31-104">A text box control with built-in validation for Windows and Linux user names.</span></span> <span data-ttu-id="d8f31-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="d8f31-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="d8f31-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="d8f31-106">UI sample</span></span>
![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a><span data-ttu-id="d8f31-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="d8f31-108">Schema</span></span>
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
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="d8f31-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="d8f31-109">Remarks</span></span>
- <span data-ttu-id="d8f31-110">Om `constraints.required` är inställd på **SANT**, textrutan måste innehålla ett värde som ska valideras.</span><span class="sxs-lookup"><span data-stu-id="d8f31-110">If `constraints.required` is set to **true**, then the text box must contain a value to validate successfully.</span></span> <span data-ttu-id="d8f31-111">Standardvärdet är **SANT**.</span><span class="sxs-lookup"><span data-stu-id="d8f31-111">The default value is **true**.</span></span>
- <span data-ttu-id="d8f31-112">`osPlatform`måste anges, och kan vara antingen **Windows** eller **Linux**.</span><span class="sxs-lookup"><span data-stu-id="d8f31-112">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span>
- <span data-ttu-id="d8f31-113">`constraints.regex`är en JavaScript-mönstret för reguljära uttryck.</span><span class="sxs-lookup"><span data-stu-id="d8f31-113">`constraints.regex` is a JavaScript regular expression pattern.</span></span> <span data-ttu-id="d8f31-114">Om anges måste textrutans värde matcha mönstret som ska valideras.</span><span class="sxs-lookup"><span data-stu-id="d8f31-114">If specified, then the text box's value must match the pattern to validate successfully.</span></span> <span data-ttu-id="d8f31-115">Standardvärdet är **null**.</span><span class="sxs-lookup"><span data-stu-id="d8f31-115">The default value is **null**.</span></span>
- <span data-ttu-id="d8f31-116">`constraints.validationMessage`är en sträng som ska visas när textrutans värde inte verifiering som anges av `constraints.regex`.</span><span class="sxs-lookup"><span data-stu-id="d8f31-116">`constraints.validationMessage` is a string to display when the text box's value fails the validation specified by `constraints.regex`.</span></span> <span data-ttu-id="d8f31-117">Om den inte anges används den textruta inbyggda verifieringsmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="d8f31-117">If not specified, then the text box's built-in validation messages are used.</span></span> <span data-ttu-id="d8f31-118">Standardvärdet är **null**.</span><span class="sxs-lookup"><span data-stu-id="d8f31-118">The default value is **null**.</span></span>
- <span data-ttu-id="d8f31-119">Det här elementet har inbyggda verifiering som baseras på det angivna värdet för `osPlatform`.</span><span class="sxs-lookup"><span data-stu-id="d8f31-119">This element has built-in validation that is based on the value specified for `osPlatform`.</span></span> <span data-ttu-id="d8f31-120">Valideringen av inbyggda kan användas tillsammans med ett anpassat reguljärt uttryck.</span><span class="sxs-lookup"><span data-stu-id="d8f31-120">The built-in validation can be used along with a custom regular expression.</span></span>
<span data-ttu-id="d8f31-121">Om ett värde för `constraints.regex` anges, och sedan båda de inbyggda och anpassade kontroller har utlösts.</span><span class="sxs-lookup"><span data-stu-id="d8f31-121">If a value for `constraints.regex` is specified, then both the built-in and custom validations are triggered.</span></span>

## <a name="sample-output"></a><span data-ttu-id="d8f31-122">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="d8f31-122">Sample output</span></span>
```json
"tabrezm"
```

## <a name="next-steps"></a><span data-ttu-id="d8f31-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d8f31-123">Next steps</span></span>
* <span data-ttu-id="d8f31-124">En introduktion till hanterade program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d8f31-124">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="d8f31-125">En introduktion till att skapa UI-definitioner, se [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d8f31-125">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="d8f31-126">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="d8f31-126">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
