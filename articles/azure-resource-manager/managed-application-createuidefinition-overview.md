---
title: "aaaUnderstand att skapa UI-definition för hanterade program i Azure | Microsoft Docs"
description: "Beskriver hur toocreate UI definitioner för hanterade program i Azure"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: d53ddf438c24d5a6cb8dd53ca0b4694ab0462515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-createuidefinition"></a>Komma igång med CreateUiDefinition
Det här dokumentet introducerar hello grundläggande begrepp för en CreateUiDefinition som används av hello Azure portal toogenerate hello-användargränssnittet för att skapa ett hanterat program.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json",
   "handler": "Microsoft.Compute.MultiVm",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { }
   }
}
```

En CreateUiDefinition innehåller alltid tre egenskaper: 

* hanterare
* Version
* parameters

För hanterade program hanterare ska alltid vara `Microsoft.Compute.MultiVm`, och hello stöds senaste versionen är `0.1.2-preview`.

hello schemat för egenskapen parameters för hello beror på hello kombination av angivna hello-hanteraren och version. För hanterade program hello stöds egenskaper är `basics`, `steps`, och `outputs`. hello grunderna och steg egenskaper innehåller hello _element_ - som textrutor och nedrullningsbara listorna - visas toobe i hello Azure-portalen. hello utdata-egenskapen är används toomap hello utdatavärden för hello angivna elementen toohello parametrarna för hello Azure Resource Manager Distributionsmall.

Inklusive `$schema` rekommenderade, men är valfritt. Om anges hello värde för `version` måste matcha hello version inom hello `$schema` URI.

## <a name="basics"></a>Grundläggande inställningar
hello grunderna steg är alltid hello första steget hello guiden genereras när hello Azure-portalen Parsar en CreateUiDefinition. Dessutom toodisplaying hello element anges i `basics`, hello portal lägger in element för användare toochoose hello prenumeration, resursgrupp och plats för hello-distribution. I allmänhet ska element som frågar efter distributionen hela parametrar som hello namnet på ett kluster eller administratör autentiseringsuppgifter gå i det här steget.

Om ett element beteende beror på hello användarens prenumeration, resursgrupp eller plats, kan elementet inte användas i grunderna. Till exempel **Microsoft.Compute.SizeSelector** beror på hello användarens prenumerationen och platsen toodetermine hello lista över tillgängliga storlekar. Därför **Microsoft.Compute.SizeSelector** kan bara användas i steg. I allmänhet endast element i hello **Microsoft.Common** namnområde kan användas i grunderna. Även om vissa element i andra namnområden (t.ex. **Microsoft.Compute.Credentials**) som inte är beroende av hello användarkontext, fortfarande är tillåtna.

## <a name="steps"></a>Steg
hello steg egenskapen kan innehålla noll eller flera ytterligare steg toodisplay efter grunderna, som innehåller ett eller flera element. Överväg att lägga till steg per roll eller tjänstnivå hello program som distribueras. Lägg exempelvis till ett steg för indata för hello överordnade noder och ett steg för hello arbetarnoder i ett kluster.

## <a name="outputs"></a>utdata
hello Azure-portalen använder hello `outputs` toomap Egenskapselement från `basics` och `steps` toohello parametrar i mallen för hello Azure Resource Manager distribution. hello nycklar av den här ordlistan är hello namnen på hello mallparametrar och hello värden är egenskaper för hello utdata objekt från hello refererar till element.

## <a name="functions"></a>Funktioner
Liknande för[Mallfunktioner](resource-group-template-functions.md) i Azure Resource Manager (både i syntax och funktioner), CreateUiDefinition innehåller funktioner för att arbeta med elementens indata och utdata, samt funktioner som villkorlig sats.

## <a name="next-steps"></a>Nästa steg
CreateUiDefinition själva har ett enkelt schema. hello verkliga djup av det kommer från alla hello stöds element och funktioner, vilka hello följande dokument som beskrivs i detalj wondrous:

- [Element](managed-application-createuidefinition-elements.md)
- [Funktioner](managed-application-createuidefinition-functions.md)

En aktuell JSON-schema för CreateUiDefinition finns här: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json. 

Senare kommer att vara tillgänglig vid hello samma plats. Ersätt hello `0.1.2-preview` del av hello URL och hello `version` värdet med hello versions-ID som du avser att toouse. hello stöds för närvarande version identifierare är `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, och `0.1.2-preview`.
