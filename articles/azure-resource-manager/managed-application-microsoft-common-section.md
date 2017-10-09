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
# <a name="microsoftcommonsection-ui-element"></a>Microsoft.Common.Section UI-element
En kontroll som grupperar ett eller flera element under en rubrik. Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).

## <a name="ui-sample"></a>UI-exempel
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>Schemat
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

## <a name="remarks"></a>Kommentarer
- `elements`måste innehålla minst ett element och kan innehålla alla elementtyper utom `Microsoft.Common.Section`.
- Det här elementet har inte stöd för hello `toolTip` egenskapen.

## <a name="sample-output"></a>Exempel på utdata
tooaccess hello utdata värdena för element i `elements`, använda hello [basics()](managed-application-createuidefinition-functions.md#basics) eller [steps()](managed-application-createuidefinition-functions.md#steps) funktioner och punktnotation:

```json
basics('section1').element1
```

Element av typen `Microsoft.Common.Section` har inga utdata värdena.

## <a name="next-steps"></a>Nästa steg
* En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
* En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).
