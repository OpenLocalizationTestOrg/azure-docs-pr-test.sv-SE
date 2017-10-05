---
title: Definiera underordnade resursen i Azure-mall | Microsoft Docs
description: "Visar hur du ställer resurstyp och namn för underordnade resursen i en Azure Resource Manager-mall"
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
ms.openlocfilehash: 5b6ce5526f354008eb4a697deec737876f22391f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a><span data-ttu-id="62b4c-103">Ange namn och typ för underordnade resursen i Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="62b4c-103">Set name and type for child resource in Resource Manager template</span></span>
<span data-ttu-id="62b4c-104">När du skapar en mall kan behöver du ofta innehåller en underordnad resurs som är relaterad till en överordnad resurs.</span><span class="sxs-lookup"><span data-stu-id="62b4c-104">When creating a template, you frequently need to include a child resource that is related to a parent resource.</span></span> <span data-ttu-id="62b4c-105">Mallen kan exempelvis omfatta en SQLServer och en databas.</span><span class="sxs-lookup"><span data-stu-id="62b4c-105">For example, your template may include a SQL server and a database.</span></span> <span data-ttu-id="62b4c-106">SQLServer är den överordnade resursen och databasen är den underordnade resursen.</span><span class="sxs-lookup"><span data-stu-id="62b4c-106">The SQL server is the parent resource, and the database is the child resource.</span></span> 

<span data-ttu-id="62b4c-107">Formatet för resurstypen underordnade är:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`</span><span class="sxs-lookup"><span data-stu-id="62b4c-107">The format of the child resource type is: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`</span></span>

<span data-ttu-id="62b4c-108">Formatet på underordnade resursnamnet är:`{parent-resource-name}/{child-resource-name}`</span><span class="sxs-lookup"><span data-stu-id="62b4c-108">The format of the child resource name is: `{parent-resource-name}/{child-resource-name}`</span></span>

<span data-ttu-id="62b4c-109">Du dock ange typ och namn i en mall på olika sätt beroende på om den finns under den överordnade resursen, eller på sin egen på den översta nivån.</span><span class="sxs-lookup"><span data-stu-id="62b4c-109">However, you specify the type and name in a template differently based on whether it is nested within the parent resource, or on its own at the top level.</span></span> <span data-ttu-id="62b4c-110">Det här avsnittet visar hur du hanterar båda metoderna.</span><span class="sxs-lookup"><span data-stu-id="62b4c-110">This topic shows how to handle both approaches.</span></span>

<span data-ttu-id="62b4c-111">När man skapar en fullständiga referens till en resurs, är ordningen att kombinera segment från typ och namn inte bara en sammanfogning av två.</span><span class="sxs-lookup"><span data-stu-id="62b4c-111">When constructing a fully qualified reference to a resource, the order to combine segments from the type and name  is not simply a concatenation of the two.</span></span>  <span data-ttu-id="62b4c-112">Använd i stället efter namnområde, en sekvens med *typnamn/* par från minst specifika mest specifika:</span><span class="sxs-lookup"><span data-stu-id="62b4c-112">Instead, after the namespace, use a sequence of *type/name* pairs from least specific to most specific:</span></span>

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

<span data-ttu-id="62b4c-113">Exempel:</span><span class="sxs-lookup"><span data-stu-id="62b4c-113">For example:</span></span>

<span data-ttu-id="62b4c-114">`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`stämmer `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` är inte korrekt</span><span class="sxs-lookup"><span data-stu-id="62b4c-114">`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` is correct `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` is not correct</span></span>

## <a name="nested-child-resource"></a><span data-ttu-id="62b4c-115">Kapslad underordnad resurs</span><span class="sxs-lookup"><span data-stu-id="62b4c-115">Nested child resource</span></span>
<span data-ttu-id="62b4c-116">Det enklaste sättet att definiera en underordnad resurs är att kapsla i den överordnade resursen.</span><span class="sxs-lookup"><span data-stu-id="62b4c-116">The easiest way to define a child resource is to nest it within the parent resource.</span></span> <span data-ttu-id="62b4c-117">I följande exempel visas en SQL-databas som har kapslats i i en SQL Server.</span><span class="sxs-lookup"><span data-stu-id="62b4c-117">The following example shows a SQL database nested within in a SQL Server.</span></span>

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

<span data-ttu-id="62b4c-118">För den underordnade resursen typ har angetts till `databases` men dess fullständiga resurstypen är `Microsoft.Sql/servers/databases`.</span><span class="sxs-lookup"><span data-stu-id="62b4c-118">For the child resource, the type is set to `databases` but its full resource type is `Microsoft.Sql/servers/databases`.</span></span> <span data-ttu-id="62b4c-119">Du inte anger `Microsoft.Sql/servers/` eftersom det antas från den överordnade resurstypen.</span><span class="sxs-lookup"><span data-stu-id="62b4c-119">You do not provide `Microsoft.Sql/servers/` because it is assumed from the parent resource type.</span></span> <span data-ttu-id="62b4c-120">Namnet på underordnade resursen har angetts till `exampledatabase` men det fullständiga namnet innehåller namnet på överordnade.</span><span class="sxs-lookup"><span data-stu-id="62b4c-120">The child resource name is set to `exampledatabase` but the full name includes the parent name.</span></span> <span data-ttu-id="62b4c-121">Du inte anger `exampleserver` eftersom det antas från den överordnade resursen.</span><span class="sxs-lookup"><span data-stu-id="62b4c-121">You do not provide `exampleserver` because it is assumed from the parent resource.</span></span>

## <a name="top-level-child-resource"></a><span data-ttu-id="62b4c-122">Översta underordnade resursen</span><span class="sxs-lookup"><span data-stu-id="62b4c-122">Top-level child resource</span></span>
<span data-ttu-id="62b4c-123">Du kan definiera den underordnade resursen på den översta nivån.</span><span class="sxs-lookup"><span data-stu-id="62b4c-123">You can define the child resource at the top level.</span></span> <span data-ttu-id="62b4c-124">Du kan använda den här metoden om den överordnade resursen inte är distribuerat i samma mall, eller om vill använda `copy` att skapa flera underordnade resurser.</span><span class="sxs-lookup"><span data-stu-id="62b4c-124">You might use this approach if the parent resource is not deployed in the same template, or if want to use `copy` to create multiple child resources.</span></span> <span data-ttu-id="62b4c-125">Med den här metoden måste du ange fullständig resurstypen och inkludera namnet på överordnade resursen i namnet på underordnade resursen.</span><span class="sxs-lookup"><span data-stu-id="62b4c-125">With this approach, you must provide the full resource type, and include the parent resource name in the child resource name.</span></span>

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

<span data-ttu-id="62b4c-126">Databasen är en underordnad resurs till servern även om de är definierade på samma nivå i mallen.</span><span class="sxs-lookup"><span data-stu-id="62b4c-126">The database is a child resource to the server even though they are defined on the same level in the template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="62b4c-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="62b4c-127">Next steps</span></span>
* <span data-ttu-id="62b4c-128">Rekommendationer om hur du skapar mallar finns i [bästa praxis för att skapa mallar för Azure Resource Manager](resource-manager-template-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="62b4c-128">For recommendations about how to create templates, see [Best practices for creating Azure Resource Manager templates](resource-manager-template-best-practices.md).</span></span>
* <span data-ttu-id="62b4c-129">Ett exempel på hur du skapar flera underordnade resurser, se [distribuera flera instanser av resurser i Azure Resource Manager-mallar](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="62b4c-129">For an example of creating multiple child resources, see [Deploy multiple instances of resources in Azure Resource Manager templates](resource-group-create-multiple.md).</span></span>
