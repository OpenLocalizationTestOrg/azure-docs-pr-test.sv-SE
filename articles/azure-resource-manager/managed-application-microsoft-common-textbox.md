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
# <a name="microsoftcommontextbox-ui-element"></a>Microsoft.Common.TextBox UI-element
En kontroll som kan använda tooedit oformaterad text. Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).

## <a name="ui-sample"></a>UI-exempel
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a>Schemat
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

## <a name="remarks"></a>Kommentarer
- Om `constraints.required` har angetts för**SANT**, hello textrutan måste innehålla ett värde toovalidate har. hello standardvärdet är **FALSKT**.
- `constraints.regex`är en JavaScript-mönstret för reguljära uttryck. Om du sedan hello textrutans värde måste matcha hello mönster toovalidate har. Standardvärdet är **null**.
- `constraints.validationMessage`är en sträng toodisplay när hello textrutans värde inte klarar valideringen. Om inget anges sedan hello textrutans inbyggda validering meddelanden används. hello standardvärdet är **null**.
- Det är möjligt toospecify ett värde för `constraints.regex` när `constraints.required` har angetts för**FALSKT**. I det här scenariot krävs ett värde inte för hello text rutan toovalidate har. Om en anges måste den matcha hello mönstret för reguljära uttryck.

## <a name="sample-output"></a>Exempel på utdata

```json
"foobar"
```

## <a name="next-steps"></a>Nästa steg
* En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
* En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).
