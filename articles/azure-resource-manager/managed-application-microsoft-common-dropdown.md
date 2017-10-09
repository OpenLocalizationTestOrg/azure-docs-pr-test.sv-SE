---
title: "aaaAzure hanterade program DropDown gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Common.DropDown UI-element för hanterade program i Azure"
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
ms.openlocfilehash: 1c07a48ad66b8e8b7fd8f59561776ecb1fc6224f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommondropdown-ui-element"></a>Microsoft.Common.DropDown UI-element
En markering med en listruta. Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).

## <a name="ui-sample"></a>UI-exempel
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>Schemat
```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Some drop down",
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
- hello etikett för `constraints.allowedValues` är hello visningstext för ett objekt och dess värde är hello utdatavärdet för hello elementet när du valt.
- Om anges hello standardvärdet måste vara en etikett som finns i `constraints.allowedValues`. Om inget anges hello första objektet i `constraints.allowedValues` är markerad. hello standardvärdet är **null**.
- `constraints.allowedValues`måste innehålla minst ett objekt.
- Det här elementet har inte stöd för hello `constraints.required` egenskapen. tooemulate problemet, lägga till ett objekt med en etikett och värdet för `""` (tom sträng) för`constraints.allowedValues`.

## <a name="sample-output"></a>Exempel på utdata
```json
"Bar"
```

## <a name="next-steps"></a>Nästa steg
* En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
* En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).
