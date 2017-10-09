---
title: "aaaAzure hanterade program PasswordBox gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Common.PasswordBox UI-element för hanterade program i Azure"
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
ms.openlocfilehash: bcb1f54c0bee464075ed732ead9aa3f88697f49e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommonpasswordbox-ui-element"></a><span data-ttu-id="eedd0-103">Microsoft.Common.PasswordBox UI-element</span><span class="sxs-lookup"><span data-stu-id="eedd0-103">Microsoft.Common.PasswordBox UI element</span></span>
<span data-ttu-id="eedd0-104">En kontroll som kan använda tooprovide och bekräfta ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="eedd0-104">A control that can be used tooprovide and confirm a password.</span></span> <span data-ttu-id="eedd0-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="eedd0-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="eedd0-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="eedd0-106">UI sample</span></span>
![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a><span data-ttu-id="eedd0-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="eedd0-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "",
    "validationMessage": ""
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="eedd0-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="eedd0-109">Remarks</span></span>
- <span data-ttu-id="eedd0-110">Det här elementet har inte stöd för hello `defaultValue` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="eedd0-110">This element doesn't support hello `defaultValue` property.</span></span>
- <span data-ttu-id="eedd0-111">För implementeringsinformation om `constraints`, se [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md).</span><span class="sxs-lookup"><span data-stu-id="eedd0-111">For implementation details of `constraints`, see [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md).</span></span>
- <span data-ttu-id="eedd0-112">Om `options.hideConfirmation` har angetts för**SANT**, hello andra textrutan för att bekräfta hello användarens lösenord är dolt.</span><span class="sxs-lookup"><span data-stu-id="eedd0-112">If `options.hideConfirmation` is set too**true**, hello second text box for confirming hello user's password is hidden.</span></span> <span data-ttu-id="eedd0-113">hello standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="eedd0-113">hello default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="eedd0-114">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="eedd0-114">Sample output</span></span>
```json
"p4ssw0rd"
```

## <a name="next-steps"></a><span data-ttu-id="eedd0-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eedd0-115">Next steps</span></span>
* <span data-ttu-id="eedd0-116">En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eedd0-116">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="eedd0-117">En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eedd0-117">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="eedd0-118">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="eedd0-118">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
