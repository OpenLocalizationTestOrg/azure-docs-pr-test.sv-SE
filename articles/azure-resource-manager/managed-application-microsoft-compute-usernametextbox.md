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
    "validationMessage": "Only alphanumeric characters are allowed, and hello value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="remarks"></a>Kommentarer
- Om `constraints.required` har angetts för**SANT**, hello textrutan måste innehålla ett värde toovalidate har. hello standardvärdet är **SANT**.
- `osPlatform`måste anges, och kan vara antingen **Windows** eller **Linux**.
- `constraints.regex`är en JavaScript-mönstret för reguljära uttryck. Om du sedan hello textrutans värde måste matcha hello mönster toovalidate har. Standardvärdet är **null**.
- `constraints.validationMessage`är en sträng toodisplay när hello textrutans värde inte hello verifiering som anges av `constraints.regex`. Om inget anges sedan hello textrutans inbyggda validering meddelanden används. hello standardvärdet är **null**.
- Det här elementet har inbyggda verifiering som baseras på hello-värde som angetts för `osPlatform`. hello inbyggda verifiering kan användas tillsammans med ett anpassat reguljärt uttryck.
Om ett värde för `constraints.regex` har angetts både hello inbyggda och anpassade kontroller har utlösts.

## <a name="sample-output"></a>Exempel på utdata
```json
"tabrezm"
```

## <a name="next-steps"></a>Nästa steg
* En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
* En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).
