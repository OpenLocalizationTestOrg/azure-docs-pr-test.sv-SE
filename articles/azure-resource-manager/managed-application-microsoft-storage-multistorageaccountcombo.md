---
title: "aaaAzure hanterade program MultiStorageAccountCombo gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Storage.MultiStorageAccountCombo UI-element för hanterade program i Azure"
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
ms.openlocfilehash: 765be145b61c3dbf0a035a7a00aa18eee464a3eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a>Microsoft.Storage.MultiStorageAccountCombo UI-element
En grupp av kontroller för att skapa flera lagringskonton med namn som börjar med en gemensam prefix. Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).

## <a name="ui-sample"></a>UI-exempel
![Microsoft.Storage.MultiStorageAccountCombo](./media/managed-application-elements/microsoft.storage.multistorageaccountcombo.png)

## <a name="schema"></a>Schemat
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.MultiStorageAccountCombo",
  "label": {
    "prefix": "Storage account prefix",
    "type": "Storage account type"
  },
  "toolTip": {
    "prefix": "",
    "type": ""
  },
  "defaultValue": {
    "prefix": "sa",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Kommentarer
- Hej värdet för `defaultValue.prefix` är sammanfogat med en eller flera heltal toogenerate hello sekvens av lagringskontonamn. Till exempel om `defaultValue.prefix` är **foobar** och `count` är **2**, sedan lagringskontonamn **foobar1** och **foobar2** genereras. Genererade lagringskontonamn verifiera för unika automatiskt.
- Hej lagringskontonamn genereras lexicographically baserat på `count`. Till exempel om `count` är 10 sedan hello lagringskontonamn avslutas med 2-siffrig heltal (01, 02, 03, etc.).
- Hej standardvärdet för `defaultValue.prefix` är **null**, och för `defaultValue.type` är **Premium_LRS**.
- Någon typ som inte har angetts i `constraints.allowedTypes` är dolt och alla typer som inte har angetts i `constraints.excludedTypes` visas.
`constraints.allowedTypes`och `constraints.excludedTypes` både valfria, men kan inte användas samtidigt.
- I tillägg toogenerating lagringskontonamn, `count` är används tooset lämplig multiplikatorn för hello-elementet. Den stöder ett statiskt värde som **2**, eller ett dynamiskt värde från ett annat element som `[steps('step1').storageAccountCount]`. hello standardvärdet är **1**.

## <a name="sample-output"></a>Exempel på utdata
```json
{
  "prefix": "sa",
  "count": 2,
  "resourceGroup": "rg01",
  "type": "Premium_LRS"
}
```

## <a name="next-steps"></a>Nästa steg
* En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
* En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).
