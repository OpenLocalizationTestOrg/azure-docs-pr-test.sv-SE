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
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a><span data-ttu-id="1b3ff-103">Ange namn och typ för underordnade resursen i Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="1b3ff-103">Set name and type for child resource in Resource Manager template</span></span>
<span data-ttu-id="1b3ff-104">När du skapar en mall måste du ofta tooinclude en underordnad resurs som är relaterade tooa överordnade resurs.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-104">When creating a template, you frequently need tooinclude a child resource that is related tooa parent resource.</span></span> <span data-ttu-id="1b3ff-105">Mallen kan exempelvis omfatta en SQLServer och en databas.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-105">For example, your template may include a SQL server and a database.</span></span> <span data-ttu-id="1b3ff-106">hello SQLServer är hello överordnade resursen och hello databasen är hello underordnade resursen.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-106">hello SQL server is hello parent resource, and hello database is hello child resource.</span></span> 

<span data-ttu-id="1b3ff-107">Resurstyp för hello underordnade hello format är:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`</span><span class="sxs-lookup"><span data-stu-id="1b3ff-107">hello format of hello child resource type is: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`</span></span>

<span data-ttu-id="1b3ff-108">hello format hello underordnade resursnamnet är:`{parent-resource-name}/{child-resource-name}`</span><span class="sxs-lookup"><span data-stu-id="1b3ff-108">hello format of hello child resource name is: `{parent-resource-name}/{child-resource-name}`</span></span>

<span data-ttu-id="1b3ff-109">Du dock ange hello typ och namn i en mall på olika sätt beroende på om den finns under hello överordnade resursen, eller på sin egen på hello översta nivån.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-109">However, you specify hello type and name in a template differently based on whether it is nested within hello parent resource, or on its own at hello top level.</span></span> <span data-ttu-id="1b3ff-110">Det här avsnittet visar hur toohandle båda närmar sig.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-110">This topic shows how toohandle both approaches.</span></span>

<span data-ttu-id="1b3ff-111">När man skapar en fullständiga referens tooa resurs hello ordning toocombine segment från hello typ och namn är inte bara en sammansättning av hello två.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-111">When constructing a fully qualified reference tooa resource, hello order toocombine segments from hello type and name  is not simply a concatenation of hello two.</span></span>  <span data-ttu-id="1b3ff-112">Använd i stället en sekvens med efter hello namnområde *typnamn/* par från minst specifika toomost specifika:</span><span class="sxs-lookup"><span data-stu-id="1b3ff-112">Instead, after hello namespace, use a sequence of *type/name* pairs from least specific toomost specific:</span></span>

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

<span data-ttu-id="1b3ff-113">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1b3ff-113">For example:</span></span>

<span data-ttu-id="1b3ff-114">`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`stämmer `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` är inte korrekt</span><span class="sxs-lookup"><span data-stu-id="1b3ff-114">`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` is correct `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` is not correct</span></span>

## <a name="nested-child-resource"></a><span data-ttu-id="1b3ff-115">Kapslad underordnad resurs</span><span class="sxs-lookup"><span data-stu-id="1b3ff-115">Nested child resource</span></span>
<span data-ttu-id="1b3ff-116">hello enklaste sättet toodefine en underordnad resurs är toonest det i hello överordnade resursen.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-116">hello easiest way toodefine a child resource is toonest it within hello parent resource.</span></span> <span data-ttu-id="1b3ff-117">hello visas följande exempel en SQL-databas som har kapslats i i en SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-117">hello following example shows a SQL database nested within in a SQL Server.</span></span>

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

<span data-ttu-id="1b3ff-118">För hello underordnade resursen hello-typ har angetts för`databases` men dess fullständiga resurstypen är `Microsoft.Sql/servers/databases`.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-118">For hello child resource, hello type is set too`databases` but its full resource type is `Microsoft.Sql/servers/databases`.</span></span> <span data-ttu-id="1b3ff-119">Du inte anger `Microsoft.Sql/servers/` eftersom det antas från hello överordnade resurstypen.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-119">You do not provide `Microsoft.Sql/servers/` because it is assumed from hello parent resource type.</span></span> <span data-ttu-id="1b3ff-120">hello underordnade resursnamnet har angetts för`exampledatabase` men hello fullständiga namn innehåller hello överordnade namn.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-120">hello child resource name is set too`exampledatabase` but hello full name includes hello parent name.</span></span> <span data-ttu-id="1b3ff-121">Du inte anger `exampleserver` eftersom det antas från hello överordnade resursen.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-121">You do not provide `exampleserver` because it is assumed from hello parent resource.</span></span>

## <a name="top-level-child-resource"></a><span data-ttu-id="1b3ff-122">Översta underordnade resursen</span><span class="sxs-lookup"><span data-stu-id="1b3ff-122">Top-level child resource</span></span>
<span data-ttu-id="1b3ff-123">Du kan definiera hello underordnade resursen på hello översta nivån.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-123">You can define hello child resource at hello top level.</span></span> <span data-ttu-id="1b3ff-124">Du kan använda den här metoden om hello överordnade resursen inte är distribuerat i hello samma mall eller om vill toouse `copy` toocreate flera underordnade resurser.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-124">You might use this approach if hello parent resource is not deployed in hello same template, or if want toouse `copy` toocreate multiple child resources.</span></span> <span data-ttu-id="1b3ff-125">Med den här metoden måste du ange hello fullständig resurstyp och inkludera hello överordnade resursnamn i hello underordnade resursnamnet.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-125">With this approach, you must provide hello full resource type, and include hello parent resource name in hello child resource name.</span></span>

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

<span data-ttu-id="1b3ff-126">hello-databasen är underordnade toohello resursservern även om de är definierade på samma nivå i hello mallen hello.</span><span class="sxs-lookup"><span data-stu-id="1b3ff-126">hello database is a child resource toohello server even though they are defined on hello same level in hello template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b3ff-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1b3ff-127">Next steps</span></span>
* <span data-ttu-id="1b3ff-128">Rekommendationer om hur toocreate mallar, se [bästa praxis för att skapa mallar för Azure Resource Manager](resource-manager-template-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="1b3ff-128">For recommendations about how toocreate templates, see [Best practices for creating Azure Resource Manager templates](resource-manager-template-best-practices.md).</span></span>
* <span data-ttu-id="1b3ff-129">Ett exempel på hur du skapar flera underordnade resurser, se [distribuera flera instanser av resurser i Azure Resource Manager-mallar](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="1b3ff-129">For an example of creating multiple child resources, see [Deploy multiple instances of resources in Azure Resource Manager templates](resource-group-create-multiple.md).</span></span>
