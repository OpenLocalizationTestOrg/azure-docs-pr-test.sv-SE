---
title: "Förstå skapa UI-definition för hanterade program i Azure | Microsoft Docs"
description: "Beskriver hur du skapar UI definitioner för hanterade program i Azure"
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
ms.openlocfilehash: 176b891538f85c5638a2321561c3d8bd377d245b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-createuidefinition"></a><span data-ttu-id="0951f-103">Komma igång med CreateUiDefinition</span><span class="sxs-lookup"><span data-stu-id="0951f-103">Getting started with CreateUiDefinition</span></span>
<span data-ttu-id="0951f-104">Det här dokumentet introducerar grundläggande begrepp för en CreateUiDefinition som används av Azure-portalen för att generera användargränssnittet för att skapa en hanterad App.</span><span class="sxs-lookup"><span data-stu-id="0951f-104">This document introduces the core concepts of a CreateUiDefinition, which is used by the Azure portal to generate the user interface for creating a managed application.</span></span>

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

<span data-ttu-id="0951f-105">En CreateUiDefinition innehåller alltid tre egenskaper:</span><span class="sxs-lookup"><span data-stu-id="0951f-105">A CreateUiDefinition always contains three properties:</span></span> 

* <span data-ttu-id="0951f-106">hanterare</span><span class="sxs-lookup"><span data-stu-id="0951f-106">handler</span></span>
* <span data-ttu-id="0951f-107">Version</span><span class="sxs-lookup"><span data-stu-id="0951f-107">version</span></span>
* <span data-ttu-id="0951f-108">Parametrar</span><span class="sxs-lookup"><span data-stu-id="0951f-108">parameters</span></span>

<span data-ttu-id="0951f-109">För hanterade program hanterare ska alltid vara `Microsoft.Compute.MultiVm`, och den senaste versionen är `0.1.2-preview`.</span><span class="sxs-lookup"><span data-stu-id="0951f-109">For managed applications, handler should always be `Microsoft.Compute.MultiVm`, and the latest supported version is `0.1.2-preview`.</span></span>

<span data-ttu-id="0951f-110">Schemat för egenskapen parameters är beroende av en kombination av den angivna hanteraren och version.</span><span class="sxs-lookup"><span data-stu-id="0951f-110">The schema of the parameters property depends on the combination of the specified handler and version.</span></span> <span data-ttu-id="0951f-111">För hanterade program egenskaperna som stöds är `basics`, `steps`, och `outputs`.</span><span class="sxs-lookup"><span data-stu-id="0951f-111">For managed applications, the supported properties are `basics`, `steps`, and `outputs`.</span></span> <span data-ttu-id="0951f-112">Grunderna och steg egenskaper innehåller den _element_ - som textrutor och nedrullningsbara listorna - ska visas i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0951f-112">The basics and steps properties contain the _elements_ - like textboxes and dropdowns - to be displayed in the Azure portal.</span></span> <span data-ttu-id="0951f-113">Egenskapen utdata används för att mappa utdatavärden för de angivna elementen till parametrarna i mallen för distribution av Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0951f-113">The outputs property is used to map the output values of the specified elements to the parameters of the Azure Resource Manager deployment template.</span></span>

<span data-ttu-id="0951f-114">Inklusive `$schema` rekommenderade, men är valfritt.</span><span class="sxs-lookup"><span data-stu-id="0951f-114">Including `$schema` is recommended, but optional.</span></span> <span data-ttu-id="0951f-115">Om värdet för `version` måste matcha versionen i den `$schema` URI.</span><span class="sxs-lookup"><span data-stu-id="0951f-115">If specified, the value for `version` must match the version within the `$schema` URI.</span></span>

## <a name="basics"></a><span data-ttu-id="0951f-116">Grundläggande inställningar</span><span class="sxs-lookup"><span data-stu-id="0951f-116">Basics</span></span>
<span data-ttu-id="0951f-117">Steget grunderna är alltid det första steget i guiden skapas när Azure-portalen Parsar en CreateUiDefinition.</span><span class="sxs-lookup"><span data-stu-id="0951f-117">The Basics step is always the first step of the wizard generated when the Azure portal parses a CreateUiDefinition.</span></span> <span data-ttu-id="0951f-118">Förutom att visa de element som anges i `basics`, portalen lägger in element för användare att välja den prenumeration, resursgrupp och plats för distributionen.</span><span class="sxs-lookup"><span data-stu-id="0951f-118">In addition to displaying the elements specified in `basics`, the portal injects elements for users to choose the subscription, resource group, and location for the deployment.</span></span> <span data-ttu-id="0951f-119">I allmänhet ska element som frågar efter distributionen hela parametrar som namnet på ett kluster eller administratör autentiseringsuppgifter gå i det här steget.</span><span class="sxs-lookup"><span data-stu-id="0951f-119">Generally, elements that query for deployment-wide parameters, like the name of a cluster or administrator credentials, should go in this step.</span></span>

<span data-ttu-id="0951f-120">Om ett element beteende beror på användarens prenumeration, resursgrupp eller plats, kan elementet inte användas i grunderna.</span><span class="sxs-lookup"><span data-stu-id="0951f-120">If an element's behavior depends on the user's subscription, resource group, or location, then that element can't be used in basics.</span></span> <span data-ttu-id="0951f-121">Till exempel **Microsoft.Compute.SizeSelector** beror på användarens prenumeration och plats för att fastställa listan med tillgängliga storlekar.</span><span class="sxs-lookup"><span data-stu-id="0951f-121">For example, **Microsoft.Compute.SizeSelector** depends on the user's subscription and location to determine the list of available sizes.</span></span> <span data-ttu-id="0951f-122">Därför **Microsoft.Compute.SizeSelector** kan bara användas i steg.</span><span class="sxs-lookup"><span data-stu-id="0951f-122">Therefore, **Microsoft.Compute.SizeSelector** can only be used in steps.</span></span> <span data-ttu-id="0951f-123">I allmänhet endast element i den **Microsoft.Common** namnområde kan användas i grunderna.</span><span class="sxs-lookup"><span data-stu-id="0951f-123">Generally, only elements in the **Microsoft.Common** namespace can be used in basics.</span></span> <span data-ttu-id="0951f-124">Även om vissa element i andra namnområden (t.ex. **Microsoft.Compute.Credentials**) som inte är beroende av användarens kontext, fortfarande är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="0951f-124">Although some elements in other namespaces (like **Microsoft.Compute.Credentials**) that don't depend on the user's context, are still allowed.</span></span>

## <a name="steps"></a><span data-ttu-id="0951f-125">Steg</span><span class="sxs-lookup"><span data-stu-id="0951f-125">Steps</span></span>
<span data-ttu-id="0951f-126">Egenskapen steg kan innehålla noll eller flera ytterligare steg för att visa efter grunderna, som innehåller ett eller flera element.</span><span class="sxs-lookup"><span data-stu-id="0951f-126">The steps property can contain zero or more additional steps to display after basics, each of which contains one or more elements.</span></span> <span data-ttu-id="0951f-127">Överväg att lägga till steg per roll- eller nivå för det program som distribueras.</span><span class="sxs-lookup"><span data-stu-id="0951f-127">Consider adding steps per role or tier of the application being deployed.</span></span> <span data-ttu-id="0951f-128">Lägg exempelvis till ett steg för indata för de överordnade noderna och ett steg för arbetarnoder i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="0951f-128">For example, add a step for inputs for the master nodes, and a step for the worker nodes in a cluster.</span></span>

## <a name="outputs"></a><span data-ttu-id="0951f-129">utdata</span><span class="sxs-lookup"><span data-stu-id="0951f-129">Outputs</span></span>
<span data-ttu-id="0951f-130">Azure-portalen använder den `outputs` egenskap som ska mappas element från `basics` och `steps` till parametrar i mallen för distribution av Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0951f-130">The Azure portal uses the `outputs` property to map elements from `basics` and `steps` to the parameters of the Azure Resource Manager deployment template.</span></span> <span data-ttu-id="0951f-131">Nycklar för den här ordlistan är namnen i mallparametrarna och värdena är egenskaper för utdata-objekt från de refererade element.</span><span class="sxs-lookup"><span data-stu-id="0951f-131">The keys of this dictionary are the names of the template parameters, and the values are properties of the output objects from the referenced elements.</span></span>

## <a name="functions"></a><span data-ttu-id="0951f-132">Funktioner</span><span class="sxs-lookup"><span data-stu-id="0951f-132">Functions</span></span>
<span data-ttu-id="0951f-133">Liknar [Mallfunktioner](resource-group-template-functions.md) i Azure Resource Manager (både i syntax och funktioner), CreateUiDefinition innehåller funktioner för att arbeta med elementens indata och utdata, samt funktioner som villkorlig sats.</span><span class="sxs-lookup"><span data-stu-id="0951f-133">Similar to [template functions](resource-group-template-functions.md) in Azure Resource Manager (both in syntax and functionality), CreateUiDefinition provides functions for working with elements' inputs and outputs, as well as features such as conditionals.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0951f-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0951f-134">Next steps</span></span>
<span data-ttu-id="0951f-135">CreateUiDefinition själva har ett enkelt schema.</span><span class="sxs-lookup"><span data-stu-id="0951f-135">CreateUiDefinition itself has a simple schema.</span></span> <span data-ttu-id="0951f-136">Den verkliga djup kommer från alla element som stöds och funktioner, som i följande dokument som beskriver i detalj wondrous:</span><span class="sxs-lookup"><span data-stu-id="0951f-136">The real depth of it comes from all the supported elements and functions, which the following documents describe in wondrous detail:</span></span>

- [<span data-ttu-id="0951f-137">Element</span><span class="sxs-lookup"><span data-stu-id="0951f-137">Elements</span></span>](managed-application-createuidefinition-elements.md)
- [<span data-ttu-id="0951f-138">Funktioner</span><span class="sxs-lookup"><span data-stu-id="0951f-138">Functions</span></span>](managed-application-createuidefinition-functions.md)

<span data-ttu-id="0951f-139">En aktuell JSON-schema för CreateUiDefinition finns här: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.</span><span class="sxs-lookup"><span data-stu-id="0951f-139">A current JSON schema for CreateUiDefinition is available here: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.</span></span> 

<span data-ttu-id="0951f-140">Senare kommer att vara tillgänglig på samma plats.</span><span class="sxs-lookup"><span data-stu-id="0951f-140">Later versions will be available at the same location.</span></span> <span data-ttu-id="0951f-141">Ersätt den `0.1.2-preview` -delen i Webbadressen och `version` värdet med versions-ID som du tänker använda.</span><span class="sxs-lookup"><span data-stu-id="0951f-141">Replace the `0.1.2-preview` portion of the URL and the `version` value with the version identifier you intend to use.</span></span> <span data-ttu-id="0951f-142">Version som för närvarande stöds identifierare är `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, och `0.1.2-preview`.</span><span class="sxs-lookup"><span data-stu-id="0951f-142">The currently supported version identifiers are `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, and `0.1.2-preview`.</span></span>