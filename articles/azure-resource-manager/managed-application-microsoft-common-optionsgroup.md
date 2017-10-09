---
title: "aaaAzure hanterade program OptionsGroup gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Common.OptionsGroup UI-element för hanterade program i Azure"
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
ms.openlocfilehash: e222468009c8db567bdde9f42836a48fb791da00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
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
- hello etikett för `constraints.allowedValues` är hello visningstext för ett objekt och dess värde är hello utdatavärdet för hello elementet när du valt.
- Om anges hello standardvärdet måste vara en etikett som finns i `constraints.allowedValues`. Om inget anges hello första objektet i `constraints.allowedValues` är markerad som standard. hello standardvärdet är **null**.
- `constraints.allowedValues`måste innehålla minst ett objekt.
- Det här elementet har inte stöd för hello `constraints.required` egendom; ett objekt måste vara valda toovalidate har.

## <a name="sample-output"></a>Exempel på utdata
```json
"Bar"
```

## <a name="next-steps"></a>Nästa steg
* En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
* En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).
