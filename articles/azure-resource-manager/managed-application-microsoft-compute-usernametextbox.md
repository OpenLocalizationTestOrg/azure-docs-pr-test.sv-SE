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
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Microsoft.Compute.UserNameTextBox UI-element
En textruta med inbyggda verifiering för Windows och Linux-användarnamn. Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).

## <a name="ui-sample"></a>UI-exempel
![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a>Schemat
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

## <a name="remarks"></a>Kommentarer
- Om `constraints.required` är inställd på **SANT**, textrutan måste innehålla ett värde som ska valideras. Standardvärdet är **SANT**.
- `osPlatform`måste anges, och kan vara antingen **Windows** eller **Linux**.
- `constraints.regex`är en JavaScript-mönstret för reguljära uttryck. Om anges måste textrutans värde matcha mönstret som ska valideras. Standardvärdet är **null**.
- `constraints.validationMessage`är en sträng som ska visas när textrutans värde inte verifiering som anges av `constraints.regex`. Om den inte anges används den textruta inbyggda verifieringsmeddelanden. Standardvärdet är **null**.
- Det här elementet har inbyggda verifiering som baseras på det angivna värdet för `osPlatform`. Valideringen av inbyggda kan användas tillsammans med ett anpassat reguljärt uttryck.
Om ett värde för `constraints.regex` anges, och sedan båda de inbyggda och anpassade kontroller har utlösts.

## <a name="sample-output"></a>Exempel på utdata
```json
"tabrezm"
```

## <a name="next-steps"></a>Nästa steg
* En introduktion till hanterade program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion till att skapa UI-definitioner, se [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
* En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).
