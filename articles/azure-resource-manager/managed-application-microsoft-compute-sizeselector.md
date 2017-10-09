---
title: "aaaAzure hanterade program SizeSelector gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Compute.SizeSelector UI-element för hanterade program i Azure"
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
ms.openlocfilehash: d93306135d9c6f9a83692766ce1ca7ea2b688086
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Microsoft.Compute.SizeSelector UI-element
En kontroll för att välja en storlek för en eller flera instanser av virtuell dator. Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).

## <a name="ui-sample"></a>UI-exempel
![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

## <a name="schema"></a>Schemat
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": []
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Kommentarer
- `recommendedSizes`ska innehålla minst en storlek. hello bör först storlek används som standard för hello.
- Om en rekommenderade storleken inte är tillgänglig i hello valt plats, hello storlek automatiskt att hoppas över. I stället används hello nästa rekommenderade storleken.
- Oavsett storlek inte har angetts i hello `constraints.allowedSizes` är dolt och valfri storlek inte har angetts i `constraints.excludedSizes` visas.
`constraints.allowedSizes`och `constraints.excludedSizes` både valfria, men kan inte användas samtidigt. hello lista över tillgängliga storlekar kan fastställas genom att anropa [visa en lista med tillgängliga virtuella datorstorlekar för en prenumeration](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).
- `osPlatform`måste anges, och kan vara antingen **Windows** eller **Linux**. Den används toodetermine hello maskinvarukostnader hello virtuella datorer.
- `imageReference`tas bort för från första part bilder, men det angivna för bilder från tredje part. Den används toodetermine hello programvarukostnader hello virtuella datorer.
- `count`är används tooset hello lämpliga multiplikatorn för hello-element. Den stöder ett statiskt värde som **2**, eller ett dynamiskt värde från ett annat element som `[steps('step1').vmCount]`. hello standardvärdet är **1**.

## <a name="sample-output"></a>Exempel på utdata
```json
"Standard_D1"
```

## <a name="next-steps"></a>Nästa steg
* En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
* En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).
