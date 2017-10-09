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
# <a name="getting-started-with-createuidefinition"></a><span data-ttu-id="e4522-103">Komma igång med CreateUiDefinition</span><span class="sxs-lookup"><span data-stu-id="e4522-103">Getting started with CreateUiDefinition</span></span>
<span data-ttu-id="e4522-104">Det här dokumentet introducerar hello grundläggande begrepp för en CreateUiDefinition som används av hello Azure portal toogenerate hello-användargränssnittet för att skapa ett hanterat program.</span><span class="sxs-lookup"><span data-stu-id="e4522-104">This document introduces hello core concepts of a CreateUiDefinition, which is used by hello Azure portal toogenerate hello user interface for creating a managed application.</span></span>

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

<span data-ttu-id="e4522-105">En CreateUiDefinition innehåller alltid tre egenskaper:</span><span class="sxs-lookup"><span data-stu-id="e4522-105">A CreateUiDefinition always contains three properties:</span></span> 

* <span data-ttu-id="e4522-106">hanterare</span><span class="sxs-lookup"><span data-stu-id="e4522-106">handler</span></span>
* <span data-ttu-id="e4522-107">Version</span><span class="sxs-lookup"><span data-stu-id="e4522-107">version</span></span>
* <span data-ttu-id="e4522-108">parameters</span><span class="sxs-lookup"><span data-stu-id="e4522-108">parameters</span></span>

<span data-ttu-id="e4522-109">För hanterade program hanterare ska alltid vara `Microsoft.Compute.MultiVm`, och hello stöds senaste versionen är `0.1.2-preview`.</span><span class="sxs-lookup"><span data-stu-id="e4522-109">For managed applications, handler should always be `Microsoft.Compute.MultiVm`, and hello latest supported version is `0.1.2-preview`.</span></span>

<span data-ttu-id="e4522-110">hello schemat för egenskapen parameters för hello beror på hello kombination av angivna hello-hanteraren och version.</span><span class="sxs-lookup"><span data-stu-id="e4522-110">hello schema of hello parameters property depends on hello combination of hello specified handler and version.</span></span> <span data-ttu-id="e4522-111">För hanterade program hello stöds egenskaper är `basics`, `steps`, och `outputs`.</span><span class="sxs-lookup"><span data-stu-id="e4522-111">For managed applications, hello supported properties are `basics`, `steps`, and `outputs`.</span></span> <span data-ttu-id="e4522-112">hello grunderna och steg egenskaper innehåller hello _element_ - som textrutor och nedrullningsbara listorna - visas toobe i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e4522-112">hello basics and steps properties contain hello _elements_ - like textboxes and dropdowns - toobe displayed in hello Azure portal.</span></span> <span data-ttu-id="e4522-113">hello utdata-egenskapen är används toomap hello utdatavärden för hello angivna elementen toohello parametrarna för hello Azure Resource Manager Distributionsmall.</span><span class="sxs-lookup"><span data-stu-id="e4522-113">hello outputs property is used toomap hello output values of hello specified elements toohello parameters of hello Azure Resource Manager deployment template.</span></span>

<span data-ttu-id="e4522-114">Inklusive `$schema` rekommenderade, men är valfritt.</span><span class="sxs-lookup"><span data-stu-id="e4522-114">Including `$schema` is recommended, but optional.</span></span> <span data-ttu-id="e4522-115">Om anges hello värde för `version` måste matcha hello version inom hello `$schema` URI.</span><span class="sxs-lookup"><span data-stu-id="e4522-115">If specified, hello value for `version` must match hello version within hello `$schema` URI.</span></span>

## <a name="basics"></a><span data-ttu-id="e4522-116">Grundläggande inställningar</span><span class="sxs-lookup"><span data-stu-id="e4522-116">Basics</span></span>
<span data-ttu-id="e4522-117">hello grunderna steg är alltid hello första steget hello guiden genereras när hello Azure-portalen Parsar en CreateUiDefinition.</span><span class="sxs-lookup"><span data-stu-id="e4522-117">hello Basics step is always hello first step of hello wizard generated when hello Azure portal parses a CreateUiDefinition.</span></span> <span data-ttu-id="e4522-118">Dessutom toodisplaying hello element anges i `basics`, hello portal lägger in element för användare toochoose hello prenumeration, resursgrupp och plats för hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="e4522-118">In addition toodisplaying hello elements specified in `basics`, hello portal injects elements for users toochoose hello subscription, resource group, and location for hello deployment.</span></span> <span data-ttu-id="e4522-119">I allmänhet ska element som frågar efter distributionen hela parametrar som hello namnet på ett kluster eller administratör autentiseringsuppgifter gå i det här steget.</span><span class="sxs-lookup"><span data-stu-id="e4522-119">Generally, elements that query for deployment-wide parameters, like hello name of a cluster or administrator credentials, should go in this step.</span></span>

<span data-ttu-id="e4522-120">Om ett element beteende beror på hello användarens prenumeration, resursgrupp eller plats, kan elementet inte användas i grunderna.</span><span class="sxs-lookup"><span data-stu-id="e4522-120">If an element's behavior depends on hello user's subscription, resource group, or location, then that element can't be used in basics.</span></span> <span data-ttu-id="e4522-121">Till exempel **Microsoft.Compute.SizeSelector** beror på hello användarens prenumerationen och platsen toodetermine hello lista över tillgängliga storlekar.</span><span class="sxs-lookup"><span data-stu-id="e4522-121">For example, **Microsoft.Compute.SizeSelector** depends on hello user's subscription and location toodetermine hello list of available sizes.</span></span> <span data-ttu-id="e4522-122">Därför **Microsoft.Compute.SizeSelector** kan bara användas i steg.</span><span class="sxs-lookup"><span data-stu-id="e4522-122">Therefore, **Microsoft.Compute.SizeSelector** can only be used in steps.</span></span> <span data-ttu-id="e4522-123">I allmänhet endast element i hello **Microsoft.Common** namnområde kan användas i grunderna.</span><span class="sxs-lookup"><span data-stu-id="e4522-123">Generally, only elements in hello **Microsoft.Common** namespace can be used in basics.</span></span> <span data-ttu-id="e4522-124">Även om vissa element i andra namnområden (t.ex. **Microsoft.Compute.Credentials**) som inte är beroende av hello användarkontext, fortfarande är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="e4522-124">Although some elements in other namespaces (like **Microsoft.Compute.Credentials**) that don't depend on hello user's context, are still allowed.</span></span>

## <a name="steps"></a><span data-ttu-id="e4522-125">Steg</span><span class="sxs-lookup"><span data-stu-id="e4522-125">Steps</span></span>
<span data-ttu-id="e4522-126">hello steg egenskapen kan innehålla noll eller flera ytterligare steg toodisplay efter grunderna, som innehåller ett eller flera element.</span><span class="sxs-lookup"><span data-stu-id="e4522-126">hello steps property can contain zero or more additional steps toodisplay after basics, each of which contains one or more elements.</span></span> <span data-ttu-id="e4522-127">Överväg att lägga till steg per roll eller tjänstnivå hello program som distribueras.</span><span class="sxs-lookup"><span data-stu-id="e4522-127">Consider adding steps per role or tier of hello application being deployed.</span></span> <span data-ttu-id="e4522-128">Lägg exempelvis till ett steg för indata för hello överordnade noder och ett steg för hello arbetarnoder i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="e4522-128">For example, add a step for inputs for hello master nodes, and a step for hello worker nodes in a cluster.</span></span>

## <a name="outputs"></a><span data-ttu-id="e4522-129">utdata</span><span class="sxs-lookup"><span data-stu-id="e4522-129">Outputs</span></span>
<span data-ttu-id="e4522-130">hello Azure-portalen använder hello `outputs` toomap Egenskapselement från `basics` och `steps` toohello parametrar i mallen för hello Azure Resource Manager distribution.</span><span class="sxs-lookup"><span data-stu-id="e4522-130">hello Azure portal uses hello `outputs` property toomap elements from `basics` and `steps` toohello parameters of hello Azure Resource Manager deployment template.</span></span> <span data-ttu-id="e4522-131">hello nycklar av den här ordlistan är hello namnen på hello mallparametrar och hello värden är egenskaper för hello utdata objekt från hello refererar till element.</span><span class="sxs-lookup"><span data-stu-id="e4522-131">hello keys of this dictionary are hello names of hello template parameters, and hello values are properties of hello output objects from hello referenced elements.</span></span>

## <a name="functions"></a><span data-ttu-id="e4522-132">Funktioner</span><span class="sxs-lookup"><span data-stu-id="e4522-132">Functions</span></span>
<span data-ttu-id="e4522-133">Liknande för[Mallfunktioner](resource-group-template-functions.md) i Azure Resource Manager (både i syntax och funktioner), CreateUiDefinition innehåller funktioner för att arbeta med elementens indata och utdata, samt funktioner som villkorlig sats.</span><span class="sxs-lookup"><span data-stu-id="e4522-133">Similar too[template functions](resource-group-template-functions.md) in Azure Resource Manager (both in syntax and functionality), CreateUiDefinition provides functions for working with elements' inputs and outputs, as well as features such as conditionals.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4522-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e4522-134">Next steps</span></span>
<span data-ttu-id="e4522-135">CreateUiDefinition själva har ett enkelt schema.</span><span class="sxs-lookup"><span data-stu-id="e4522-135">CreateUiDefinition itself has a simple schema.</span></span> <span data-ttu-id="e4522-136">hello verkliga djup av det kommer från alla hello stöds element och funktioner, vilka hello följande dokument som beskrivs i detalj wondrous:</span><span class="sxs-lookup"><span data-stu-id="e4522-136">hello real depth of it comes from all hello supported elements and functions, which hello following documents describe in wondrous detail:</span></span>

- [<span data-ttu-id="e4522-137">Element</span><span class="sxs-lookup"><span data-stu-id="e4522-137">Elements</span></span>](managed-application-createuidefinition-elements.md)
- [<span data-ttu-id="e4522-138">Funktioner</span><span class="sxs-lookup"><span data-stu-id="e4522-138">Functions</span></span>](managed-application-createuidefinition-functions.md)

<span data-ttu-id="e4522-139">En aktuell JSON-schema för CreateUiDefinition finns här: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.</span><span class="sxs-lookup"><span data-stu-id="e4522-139">A current JSON schema for CreateUiDefinition is available here: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.</span></span> 

<span data-ttu-id="e4522-140">Senare kommer att vara tillgänglig vid hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="e4522-140">Later versions will be available at hello same location.</span></span> <span data-ttu-id="e4522-141">Ersätt hello `0.1.2-preview` del av hello URL och hello `version` värdet med hello versions-ID som du avser att toouse.</span><span class="sxs-lookup"><span data-stu-id="e4522-141">Replace hello `0.1.2-preview` portion of hello URL and hello `version` value with hello version identifier you intend toouse.</span></span> <span data-ttu-id="e4522-142">hello stöds för närvarande version identifierare är `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, och `0.1.2-preview`.</span><span class="sxs-lookup"><span data-stu-id="e4522-142">hello currently supported version identifiers are `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, and `0.1.2-preview`.</span></span>
