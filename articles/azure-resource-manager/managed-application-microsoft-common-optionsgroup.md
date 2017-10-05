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
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Microsoft.Common.OptionsGroup UI-element
En markering med en rad med tillgängliga alternativ. Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).

## <a name="ui-sample"></a>UI-exempel
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a>Schemat
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

## <a name="remarks"></a>Kommentarer
- Etikett för `constraints.allowedValues` är texten som visas för ett objekt och dess värde är värdet av elementet när du valt.
- Om du standardvärdet måste vara en etikett som finns i `constraints.allowedValues`. Om inget anges det första objektet i `constraints.allowedValues` är markerad som standard. Standardvärdet är **null**.
- `constraints.allowedValues`måste innehålla minst ett objekt.
- Det här elementet har inte stöd för den `constraints.required` egendom; du måste välja ett objekt kan valideras.

## <a name="sample-output"></a>Exempel på utdata
```json
"Bar"
```

## <a name="next-steps"></a>Nästa steg
* En introduktion till hanterade program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion till att skapa UI-definitioner, se [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
* En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).
