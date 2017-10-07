---
title: aaaDefine underordnade resurs i Azure-mall | Microsoft Docs
description: "Visar hur tooset hello resurstyp och namn för underordnade resursen i en Azure Resource Manager-mall"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: tomfitz
ms.openlocfilehash: c502c589100d7ae864d7fb01b5ba10ddfaf92592
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a>Ange namn och typ för underordnade resursen i Resource Manager-mall
När du skapar en mall måste du ofta tooinclude en underordnad resurs som är relaterade tooa överordnade resurs. Mallen kan exempelvis omfatta en SQLServer och en databas. hello SQLServer är hello överordnade resursen och hello databasen är hello underordnade resursen. 

Resurstyp för hello underordnade hello format är:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

hello format hello underordnade resursnamnet är:`{parent-resource-name}/{child-resource-name}`

Du dock ange hello typ och namn i en mall på olika sätt beroende på om den finns under hello överordnade resursen, eller på sin egen på hello översta nivån. Det här avsnittet visar hur toohandle båda närmar sig.

När man skapar en fullständiga referens tooa resurs hello ordning toocombine segment från hello typ och namn är inte bara en sammansättning av hello två.  Använd i stället en sekvens med efter hello namnområde *typnamn/* par från minst specifika toomost specifika:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Exempel:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`stämmer `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` är inte korrekt

## <a name="nested-child-resource"></a>Kapslad underordnad resurs
hello enklaste sättet toodefine en underordnad resurs är toonest det i hello överordnade resursen. hello visas följande exempel en SQL-databas som har kapslats i i en SQL Server.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

För hello underordnade resursen hello-typ har angetts för`databases` men dess fullständiga resurstypen är `Microsoft.Sql/servers/databases`. Du inte anger `Microsoft.Sql/servers/` eftersom det antas från hello överordnade resurstypen. hello underordnade resursnamnet har angetts för`exampledatabase` men hello fullständiga namn innehåller hello överordnade namn. Du inte anger `exampleserver` eftersom det antas från hello överordnade resursen.

## <a name="top-level-child-resource"></a>Översta underordnade resursen
Du kan definiera hello underordnade resursen på hello översta nivån. Du kan använda den här metoden om hello överordnade resursen inte är distribuerat i hello samma mall eller om vill toouse `copy` toocreate flera underordnade resurser. Med den här metoden måste du ange hello fullständig resurstyp och inkludera hello överordnade resursnamn i hello underordnade resursnamnet.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

hello-databasen är underordnade toohello resursservern även om de är definierade på samma nivå i hello mallen hello.

## <a name="next-steps"></a>Nästa steg
* Rekommendationer om hur toocreate mallar, se [bästa praxis för att skapa mallar för Azure Resource Manager](resource-manager-template-best-practices.md).
* Ett exempel på hur du skapar flera underordnade resurser, se [distribuera flera instanser av resurser i Azure Resource Manager-mallar](resource-group-create-multiple.md).
