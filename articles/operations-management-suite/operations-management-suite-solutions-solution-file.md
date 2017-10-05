---
title: "Skapa lösningar för hantering i Operations Management Suite (OMS) | Microsoft Docs"
description: "Hanteringslösningar utöka funktionerna i Operations Management Suite (OMS) genom att tillhandahålla paketerade hanteringsscenarier som kunder kan lägga till sina OMS-arbetsyta.  Den här artikeln innehåller information om hur du kan skapa lösningar för hantering som ska användas i din egen miljö eller göras tillgängligt för kunderna."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee3462c13101d18921dc488b08c79e1e4e02ff3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="creating-a-management-solution-file-in-operations-management-suite-oms-preview"></a><span data-ttu-id="b10e6-104">Skapa en management lösningsfilen i Operations Management Suite (OMS) (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="b10e6-104">Creating a management solution file in Operations Management Suite (OMS) (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="b10e6-105">Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="b10e6-106">Ett schema som beskrivs nedan kan ändras.</span><span class="sxs-lookup"><span data-stu-id="b10e6-106">Any schema described below is subject to change.</span></span>  

<span data-ttu-id="b10e6-107">Lösningar för hantering i Operations Management Suite (OMS) implementeras som [Resource Manager-mallar](../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="b10e6-107">Management solutions in Operations Management Suite (OMS) are implemented as [Resource Manager templates](../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>  <span data-ttu-id="b10e6-108">Aktiviteten huvudsakliga Lär dig hur du skapar hanteringslösningar learning så [skapar en mall](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b10e6-108">The main task in learning how to author management solutions is learning how to [author a template](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>  <span data-ttu-id="b10e6-109">Den här artikeln innehåller unik information om mallar som används för lösningar och hur du konfigurerar vanliga lösning resurser.</span><span class="sxs-lookup"><span data-stu-id="b10e6-109">This article provides unique details of templates used for solutions and how to configure typical solution resources.</span></span>


## <a name="tools"></a><span data-ttu-id="b10e6-110">Verktyg</span><span class="sxs-lookup"><span data-stu-id="b10e6-110">Tools</span></span>

<span data-ttu-id="b10e6-111">Du kan använda valfri textredigerare för att arbeta med lösningsfiler, men vi rekommenderar att utnyttja funktionerna i Visual Studio eller Visual Studio Code som beskrivs i följande artiklar.</span><span class="sxs-lookup"><span data-stu-id="b10e6-111">You can use any text editor to work with solution files, but we recommend leveraging the features provided in Visual Studio or Visual Studio Code as described in the following articles.</span></span>

- [<span data-ttu-id="b10e6-112">Skapa och distribuera Azure-resursgrupper via Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b10e6-112">Creating and deploying Azure resource groups through Visual Studio</span></span>](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [<span data-ttu-id="b10e6-113">Arbeta med Azure Resource Manager-mallar i Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b10e6-113">Working with Azure Resource Manager Templates in Visual Studio Code</span></span>](../azure-resource-manager/resource-manager-vs-code.md)




## <a name="structure"></a><span data-ttu-id="b10e6-114">struktur</span><span class="sxs-lookup"><span data-stu-id="b10e6-114">Structure</span></span>
<span data-ttu-id="b10e6-115">Den grundläggande strukturen i en fil för management-lösning är detsamma som en [Resource Manager-mall](../azure-resource-manager/resource-group-authoring-templates.md#template-format) som är som följer.</span><span class="sxs-lookup"><span data-stu-id="b10e6-115">The basic structure of a management solution file is the same as a [Resource Manager Template](../azure-resource-manager/resource-group-authoring-templates.md#template-format) which is as follows.</span></span>  <span data-ttu-id="b10e6-116">Var och en av nedanstående avsnitt beskrivs de översta elementen och och innehållet i en lösning.</span><span class="sxs-lookup"><span data-stu-id="b10e6-116">Each of the sections below describes the top level elements and and their contents in a solution.</span></span>  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a><span data-ttu-id="b10e6-117">Parametrar</span><span class="sxs-lookup"><span data-stu-id="b10e6-117">Parameters</span></span>
<span data-ttu-id="b10e6-118">[Parametrarna](../azure-resource-manager/resource-group-authoring-templates.md#parameters) är värden som du behöver från användaren när de installerar hanteringslösningen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-118">[Parameters](../azure-resource-manager/resource-group-authoring-templates.md#parameters) are values that you require from the user when they install the management solution.</span></span>  <span data-ttu-id="b10e6-119">Det finns standardparametrar som har alla lösningar och du kan lägga till ytterligare parametrar som krävs för din lösning.</span><span class="sxs-lookup"><span data-stu-id="b10e6-119">There are standard parameters that all solutions will have, and you can add additional parameters as required for your particular solution.</span></span>  <span data-ttu-id="b10e6-120">Hur användare ange parametervärden när de installerar lösningen beror på en viss parameter och hur lösningen installeras.</span><span class="sxs-lookup"><span data-stu-id="b10e6-120">How users will provide parameter values when they install your solution will depend on the particular parameter and how the solution is being installed.</span></span>

<span data-ttu-id="b10e6-121">När en användare installerar din lösning för hantering via den [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) eller [Azure-snabbstartsmallar](operations-management-suite-solutions.md#finding-and-installing-management-solutions) uppmanas de att välja en [OMS-arbetsytan och Automation-kontot](operations-management-suite-solutions.md#oms-workspace-and-automation-account).</span><span class="sxs-lookup"><span data-stu-id="b10e6-121">When a user installs your management solution through the [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) or [Azure QuickStart templates](operations-management-suite-solutions.md#finding-and-installing-management-solutions) they are prompted to select an [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account).</span></span>  <span data-ttu-id="b10e6-122">Dessa används för att fylla i värdena för var och en av parametrarna som standard.</span><span class="sxs-lookup"><span data-stu-id="b10e6-122">These are used to populate the values of each of the standard parameters.</span></span>  <span data-ttu-id="b10e6-123">Användaren uppmanas inte att direkt ange värden för parametrarna standard, men de uppmanas att ange värden för alla ytterligare parametrar.</span><span class="sxs-lookup"><span data-stu-id="b10e6-123">The user is not prompted to directly provide values for the standard parameters, but they are prompted to provide values for any additional parameters.</span></span>

<span data-ttu-id="b10e6-124">När användaren installerar lösningen [en annan metod](operations-management-suite-solutions.md#finding-and-installing-management-solutions), måste de ange ett värde för alla parametrar som standard och alla ytterligare parametrar.</span><span class="sxs-lookup"><span data-stu-id="b10e6-124">When the user installs your solution [another method](operations-management-suite-solutions.md#finding-and-installing-management-solutions), they must provide a value for all standard parameters and all additional parameters.</span></span>

<span data-ttu-id="b10e6-125">En exempel-parametern visas nedan.</span><span class="sxs-lookup"><span data-stu-id="b10e6-125">A sample parameter is shown below.</span></span>  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

<span data-ttu-id="b10e6-126">I följande tabell beskrivs attributen för en parameter.</span><span class="sxs-lookup"><span data-stu-id="b10e6-126">The following table describes the attributes of a parameter.</span></span>

| <span data-ttu-id="b10e6-127">Attribut</span><span class="sxs-lookup"><span data-stu-id="b10e6-127">Attribute</span></span> | <span data-ttu-id="b10e6-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b10e6-128">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b10e6-129">typ</span><span class="sxs-lookup"><span data-stu-id="b10e6-129">type</span></span> |<span data-ttu-id="b10e6-130">Datatypen för parametern.</span><span class="sxs-lookup"><span data-stu-id="b10e6-130">Data type for the parameter.</span></span> <span data-ttu-id="b10e6-131">Inkommande kontrollen visas för användaren beror på datatypen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-131">The input control displayed for the user depends on the data type.</span></span><br><br><span data-ttu-id="b10e6-132">bool - listrutan</span><span class="sxs-lookup"><span data-stu-id="b10e6-132">bool - Drop down box</span></span><br><span data-ttu-id="b10e6-133">String - textruta</span><span class="sxs-lookup"><span data-stu-id="b10e6-133">string - Text box</span></span><br><span data-ttu-id="b10e6-134">int - textruta</span><span class="sxs-lookup"><span data-stu-id="b10e6-134">int - Text box</span></span><br><span data-ttu-id="b10e6-135">SecureString - fält för lösenord</span><span class="sxs-lookup"><span data-stu-id="b10e6-135">securestring - Password field</span></span><br> |
| <span data-ttu-id="b10e6-136">category</span><span class="sxs-lookup"><span data-stu-id="b10e6-136">category</span></span> |<span data-ttu-id="b10e6-137">Valfri kategori för parametern.</span><span class="sxs-lookup"><span data-stu-id="b10e6-137">Optional category for the parameter.</span></span>  <span data-ttu-id="b10e6-138">Parametrarna i samma kategori grupperas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="b10e6-138">Parameters in the same category are grouped together.</span></span> |
| <span data-ttu-id="b10e6-139">Kontrollen</span><span class="sxs-lookup"><span data-stu-id="b10e6-139">control</span></span> |<span data-ttu-id="b10e6-140">Ytterligare funktioner för string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="b10e6-140">Additional functionality for string parameters.</span></span><br><br><span data-ttu-id="b10e6-141">datetime - Datetime kontrollen visas.</span><span class="sxs-lookup"><span data-stu-id="b10e6-141">datetime - Datetime control is displayed.</span></span><br><span data-ttu-id="b10e6-142">GUID - Guid-värde genereras automatiskt och parametern visas inte.</span><span class="sxs-lookup"><span data-stu-id="b10e6-142">guid - Guid value is automatically generated, and the parameter is not displayed.</span></span> |
| <span data-ttu-id="b10e6-143">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b10e6-143">description</span></span> |<span data-ttu-id="b10e6-144">Valfri beskrivning för parametern.</span><span class="sxs-lookup"><span data-stu-id="b10e6-144">Optional description for the parameter.</span></span>  <span data-ttu-id="b10e6-145">Visas i en information pratbubblor bredvid parametern.</span><span class="sxs-lookup"><span data-stu-id="b10e6-145">Displayed in an information balloon next to the parameter.</span></span> |

### <a name="standard-parameters"></a><span data-ttu-id="b10e6-146">Standard-parametrar</span><span class="sxs-lookup"><span data-stu-id="b10e6-146">Standard parameters</span></span>
<span data-ttu-id="b10e6-147">I följande tabell visas standardparametrar för alla hanteringslösningar för.</span><span class="sxs-lookup"><span data-stu-id="b10e6-147">The following table lists the standard parameters for all management solutions.</span></span>  <span data-ttu-id="b10e6-148">Dessa värden har angetts för användaren i stället för att fråga om dem när lösningen har installerats från Azure Marketplace eller snabbstartsmallar.</span><span class="sxs-lookup"><span data-stu-id="b10e6-148">These values are populated for the user instead of prompting for them when your solution is installed from the Azure Marketplace or Quickstart templates.</span></span>  <span data-ttu-id="b10e6-149">Användaren måste ange värden för dem om lösningen har installerats med en annan metod.</span><span class="sxs-lookup"><span data-stu-id="b10e6-149">The user must provide values for them if the solution is installed with another method.</span></span>

> [!NOTE]
> <span data-ttu-id="b10e6-150">Användargränssnittet i Azure Marketplace och snabbstartsmallar förväntas parameternamn i tabellen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-150">The user interface in the Azure Marketplace and Quickstart templates is expecting the parameter names in the table.</span></span>  <span data-ttu-id="b10e6-151">Om du använder olika parameternamn sedan användaren uppmanas att dem och fylls de inte automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b10e6-151">If you use different parameter names then the user will be prompted for them, and they will not be automatically populated.</span></span>
>
>

| <span data-ttu-id="b10e6-152">Parameter</span><span class="sxs-lookup"><span data-stu-id="b10e6-152">Parameter</span></span> | <span data-ttu-id="b10e6-153">Typ</span><span class="sxs-lookup"><span data-stu-id="b10e6-153">Type</span></span> | <span data-ttu-id="b10e6-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b10e6-154">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b10e6-155">Kontonamn</span><span class="sxs-lookup"><span data-stu-id="b10e6-155">accountName</span></span> |<span data-ttu-id="b10e6-156">Sträng</span><span class="sxs-lookup"><span data-stu-id="b10e6-156">string</span></span> |<span data-ttu-id="b10e6-157">Azure Automation-kontonamn.</span><span class="sxs-lookup"><span data-stu-id="b10e6-157">Azure Automation account name.</span></span> |
| <span data-ttu-id="b10e6-158">pricingTier</span><span class="sxs-lookup"><span data-stu-id="b10e6-158">pricingTier</span></span> |<span data-ttu-id="b10e6-159">Sträng</span><span class="sxs-lookup"><span data-stu-id="b10e6-159">string</span></span> |<span data-ttu-id="b10e6-160">Prisnivån för både logganalys-arbetsytan och Azure Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="b10e6-160">Pricing tier of both Log Analytics workspace and Azure Automation account.</span></span> |
| <span data-ttu-id="b10e6-161">regionId</span><span class="sxs-lookup"><span data-stu-id="b10e6-161">regionId</span></span> |<span data-ttu-id="b10e6-162">Sträng</span><span class="sxs-lookup"><span data-stu-id="b10e6-162">string</span></span> |<span data-ttu-id="b10e6-163">Region i Azure Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="b10e6-163">Region of the Azure Automation account.</span></span> |
| <span data-ttu-id="b10e6-164">SolutionName</span><span class="sxs-lookup"><span data-stu-id="b10e6-164">solutionName</span></span> |<span data-ttu-id="b10e6-165">Sträng</span><span class="sxs-lookup"><span data-stu-id="b10e6-165">string</span></span> |<span data-ttu-id="b10e6-166">Namnet för lösningen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-166">Name of the solution.</span></span>  <span data-ttu-id="b10e6-167">Om du distribuerar din lösning med snabbstartsmallar bör sedan du definiera solutionName som en parameter där du kan definiera en sträng i stället att användaren måste ange en.</span><span class="sxs-lookup"><span data-stu-id="b10e6-167">If you are deploying your solution through Quickstart templates, then you should define solutionName as a parameter so you can define a string instead requiring the user to specify one.</span></span> |
| <span data-ttu-id="b10e6-168">workspaceName</span><span class="sxs-lookup"><span data-stu-id="b10e6-168">workspaceName</span></span> |<span data-ttu-id="b10e6-169">Sträng</span><span class="sxs-lookup"><span data-stu-id="b10e6-169">string</span></span> |<span data-ttu-id="b10e6-170">Log Analytics arbetsytans namn.</span><span class="sxs-lookup"><span data-stu-id="b10e6-170">Log Analytics workspace name.</span></span> |
| <span data-ttu-id="b10e6-171">workspaceRegionId</span><span class="sxs-lookup"><span data-stu-id="b10e6-171">workspaceRegionId</span></span> |<span data-ttu-id="b10e6-172">Sträng</span><span class="sxs-lookup"><span data-stu-id="b10e6-172">string</span></span> |<span data-ttu-id="b10e6-173">Region i logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="b10e6-173">Region of the Log Analytics workspace.</span></span> |


<span data-ttu-id="b10e6-174">Följande är standard parametrar som du kan kopiera och klistra in i din lösningsfilen struktur.</span><span class="sxs-lookup"><span data-stu-id="b10e6-174">Following is the structure of the standard parameters that you can copy and paste into your solution file.</span></span>  

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
               "type": "string",
               "metadata": {
                   "description": "A valid Azure Automation account name"
               }
        },
        "workspaceRegionId": {
               "type": "string",
               "metadata": {
                   "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        }
    }


<span data-ttu-id="b10e6-175">Du refererar till parametervärden i andra element i lösningen med syntaxen **parametrar (parametern name)**.</span><span class="sxs-lookup"><span data-stu-id="b10e6-175">You refer to parameter values in other elements of the solution with the syntax **parameters('parameter name')**.</span></span>  <span data-ttu-id="b10e6-176">Till exempel vill komma åt arbetsytans namn måste använda du **parameters('workspaceName')**</span><span class="sxs-lookup"><span data-stu-id="b10e6-176">For example, to access the workspace name, you would use **parameters('workspaceName')**</span></span>

## <a name="variables"></a><span data-ttu-id="b10e6-177">Variabler</span><span class="sxs-lookup"><span data-stu-id="b10e6-177">Variables</span></span>
<span data-ttu-id="b10e6-178">[Variabler](../azure-resource-manager/resource-group-authoring-templates.md#variables) är värden som du ska använda i resten av hanteringslösningen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-178">[Variables](../azure-resource-manager/resource-group-authoring-templates.md#variables) are values that you will use in the rest of the management solution.</span></span>  <span data-ttu-id="b10e6-179">Dessa värden exponeras inte för den användare som installerar lösningen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-179">These values are not exposed to the user installing the solution.</span></span>  <span data-ttu-id="b10e6-180">De är avsedda att ge författaren med en enda plats där de kan hantera värden som kan användas flera gånger i hela lösningen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-180">They are intended to provide the author with a single location where they can manage values that may be used multiple times throughout the solution.</span></span> <span data-ttu-id="b10e6-181">Du bör placera alla värden specifika till lösningen i variabler i stället för hårda koda dem i den **resurser** element.</span><span class="sxs-lookup"><span data-stu-id="b10e6-181">You should put any values specific to your solution in variables as opposed to hard coding them in the **resources** element.</span></span>  <span data-ttu-id="b10e6-182">Detta gör det lättare att läsa koden och kan du enkelt ändra dessa värden i senare versioner.</span><span class="sxs-lookup"><span data-stu-id="b10e6-182">This makes the code more readable and allows you to easily change these values in later versions.</span></span>

<span data-ttu-id="b10e6-183">Följande är ett exempel på en **variabler** element med vanliga parametrar som används i lösningar.</span><span class="sxs-lookup"><span data-stu-id="b10e6-183">Following is an example of a **variables** element with typical parameters used in solutions.</span></span>

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

<span data-ttu-id="b10e6-184">Du refererar till variabelvärden genom lösningen med syntax **variabler (variabel namn)**.</span><span class="sxs-lookup"><span data-stu-id="b10e6-184">You refer to variable values through the solution with the syntax **variables('variable name')**.</span></span>  <span data-ttu-id="b10e6-185">Till exempel för att komma åt variabeln SolutionName, använder du **variables('SolutionName')**.</span><span class="sxs-lookup"><span data-stu-id="b10e6-185">For example, to access the SolutionName variable, you would use **variables('SolutionName')**.</span></span>

<span data-ttu-id="b10e6-186">Du kan också definiera komplex variabler som flera uppsättningar av värden.</span><span class="sxs-lookup"><span data-stu-id="b10e6-186">You can also define complex variables that multiple sets of values.</span></span>  <span data-ttu-id="b10e6-187">Detta är särskilt användbart i hanteringslösningar där du definierar flera egenskaper för olika typer av resurser.</span><span class="sxs-lookup"><span data-stu-id="b10e6-187">These are particularly useful in management solutions where you are defining multiple properties for different types of resources.</span></span>  <span data-ttu-id="b10e6-188">Du kan till exempel omstrukturera variablerna lösningen ovan följande.</span><span class="sxs-lookup"><span data-stu-id="b10e6-188">For example, you could restructure the solution variables shown above to the following.</span></span>

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

<span data-ttu-id="b10e6-189">I så fall måste du referera till variabelvärden genom lösningen med syntax **variables('variable name').property**.</span><span class="sxs-lookup"><span data-stu-id="b10e6-189">In this case, you refer to variable values through the solution with the syntax **variables('variable name').property**.</span></span>  <span data-ttu-id="b10e6-190">Till exempel vill komma åt variabeln lösningens namn måste använda du **variables('Solution'). Namnet**.</span><span class="sxs-lookup"><span data-stu-id="b10e6-190">For example, to access the Solution Name variable, you would use **variables('Solution').Name**.</span></span>

## <a name="resources"></a><span data-ttu-id="b10e6-191">Resurser</span><span class="sxs-lookup"><span data-stu-id="b10e6-191">Resources</span></span>
<span data-ttu-id="b10e6-192">[Resurser](../azure-resource-manager/resource-group-authoring-templates.md#resources) definierar de olika resurser som din lösning ska installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="b10e6-192">[Resources](../azure-resource-manager/resource-group-authoring-templates.md#resources) define the different resources that your management solution will install and configure.</span></span>  <span data-ttu-id="b10e6-193">Det här är den största och mest komplexa delen av mallen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-193">This will be the largest and most complex portion of the template.</span></span>  <span data-ttu-id="b10e6-194">Du kan hämta den struktur och en fullständig beskrivning av resursen element i [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md#resources).</span><span class="sxs-lookup"><span data-stu-id="b10e6-194">You can get the structure and complete description of resource elements in [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md#resources).</span></span>  <span data-ttu-id="b10e6-195">Olika resurser som du vanligtvis definierar beskrivs i andra artiklar i den här dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-195">Different resources that you will typically define are detailed in other articles in this documentation.</span></span> 


### <a name="dependencies"></a><span data-ttu-id="b10e6-196">Beroenden</span><span class="sxs-lookup"><span data-stu-id="b10e6-196">Dependencies</span></span>
<span data-ttu-id="b10e6-197">Den **dependsOn** element anger en [beroende](../azure-resource-manager/resource-group-define-dependencies.md) på en annan resurs.</span><span class="sxs-lookup"><span data-stu-id="b10e6-197">The **dependsOn** elements specifies a [dependency](../azure-resource-manager/resource-group-define-dependencies.md) on another resource.</span></span>  <span data-ttu-id="b10e6-198">När lösningen har installerats skapas en resurs tills alla dess beroenden har skapats.</span><span class="sxs-lookup"><span data-stu-id="b10e6-198">When the solution is installed, a resource is not created until all of its dependencies have been created.</span></span>  <span data-ttu-id="b10e6-199">Till exempel din lösning kan [startar en runbook](operations-management-suite-solutions-resources-automation.md#runbooks) när den är installerad med hjälp av en [jobbet resurs](operations-management-suite-solutions-resources-automation.md#automation-jobs).</span><span class="sxs-lookup"><span data-stu-id="b10e6-199">For example, your solution might [start a runbook](operations-management-suite-solutions-resources-automation.md#runbooks) when it's installed using a [job resource](operations-management-suite-solutions-resources-automation.md#automation-jobs).</span></span>  <span data-ttu-id="b10e6-200">Resursen jobbet är beroende av resursen runbook för att se till att runbooken har skapats innan jobbet har skapats.</span><span class="sxs-lookup"><span data-stu-id="b10e6-200">The job resource would be dependent on the runbook resource to make sure that the runbook is created before the job is created.</span></span>

### <a name="oms-workspace-and-automation-account"></a><span data-ttu-id="b10e6-201">OMS-arbetsytan och Automation-konto</span><span class="sxs-lookup"><span data-stu-id="b10e6-201">OMS workspace and Automation account</span></span>
<span data-ttu-id="b10e6-202">Av hanteringslösningar kräver en [OMS-arbetsytan](../log-analytics/log-analytics-manage-access.md) innehåller vyer och en [Automation-konto](../automation/automation-security-overview.md#automation-account-overview) ska innehålla runbooks och relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="b10e6-202">Management solutions require an [OMS workspace](../log-analytics/log-analytics-manage-access.md) to contain views and an [Automation account](../automation/automation-security-overview.md#automation-account-overview) to contain runbooks and related resources.</span></span>  <span data-ttu-id="b10e6-203">Dessa måste vara tillgängliga innan resurserna i lösningen skapas och ska inte definieras i själva lösningen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-203">These must be available before the resources in the solution are created and should not be defined in the solution itself.</span></span>  <span data-ttu-id="b10e6-204">Användaren kommer [ange arbetsytan och kontot](operations-management-suite-solutions.md#oms-workspace-and-automation-account) när de distribuerar din lösning, men som författare bör du överväga följande punkter.</span><span class="sxs-lookup"><span data-stu-id="b10e6-204">The user will [specify a workspace and account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) when they deploy your solution, but as the author you should consider the following points.</span></span>

## <a name="solution-resource"></a><span data-ttu-id="b10e6-205">Lösning för resurs</span><span class="sxs-lookup"><span data-stu-id="b10e6-205">Solution resource</span></span>
<span data-ttu-id="b10e6-206">Varje lösning kräver att en resurs i den **resurser** element som definierar själva lösningen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-206">Each solution requires a resource entry in the **resources** element that defines the solution itself.</span></span>  <span data-ttu-id="b10e6-207">Det har en typ av **Microsoft.OperationsManagement/solutions** och har följande struktur.</span><span class="sxs-lookup"><span data-stu-id="b10e6-207">This will have a type of **Microsoft.OperationsManagement/solutions** and have the following structure.</span></span> <span data-ttu-id="b10e6-208">Detta inkluderar [standardparametrar](#parameters) och [variabler](#variables) som vanligtvis används för att definiera egenskaperna för lösningen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-208">This includes [standard parameters](#parameters) and [variables](#variables) that are typically used to define properties of the solution.</span></span>


    {
      "name": "[concat(variables('Solution').Name, '[' ,parameters('workspacename'), ']')]",
      "location": "[parameters('workspaceRegionId')]",
      "tags": { },
      "type": "Microsoft.OperationsManagement/solutions",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
        <list-of-resources>
      ],
      "properties": {
        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
        "referencedResources": [
            <list-of-referenced-resources>
        ],
        "containedResources": [
            <list-of-contained-resources>
        ]
      },
      "plan": {
        "name": "[concat(variables('Solution').Name, '[' ,parameters('workspaceName'), ']')]",
        "Version": "[variables('Solution').Version]",
        "product": "[variables('ProductName')]",
        "publisher": "[variables('Solution').Publisher]",
        "promotionCode": ""
      }
    }




### <a name="dependencies"></a><span data-ttu-id="b10e6-209">Beroenden</span><span class="sxs-lookup"><span data-stu-id="b10e6-209">Dependencies</span></span>
<span data-ttu-id="b10e6-210">Resursen lösningen måste ha en [beroende](../azure-resource-manager/resource-group-define-dependencies.md) på alla andra resurser i lösningen eftersom de ska finnas innan du kan skapa lösningen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-210">The solution resource must have a [dependency](../azure-resource-manager/resource-group-define-dependencies.md) on every other resource in the solution since they need to exist before the solution can be created.</span></span>  <span data-ttu-id="b10e6-211">Det gör du genom att lägga till en post för varje resurs i den **dependsOn** element.</span><span class="sxs-lookup"><span data-stu-id="b10e6-211">You do this by adding an entry for each resource in the **dependsOn** element.</span></span>

### <a name="properties"></a><span data-ttu-id="b10e6-212">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="b10e6-212">Properties</span></span>
<span data-ttu-id="b10e6-213">Lösning resursen har egenskaperna i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="b10e6-213">The solution resource has the properties in the following table.</span></span>  <span data-ttu-id="b10e6-214">Detta inkluderar resurser refereras och finns i lösningen som definierar hur resursen hanteras när lösningen har installerats.</span><span class="sxs-lookup"><span data-stu-id="b10e6-214">This includes the resources referenced and contained by the solution which defines how the resource is managed after the solution is installed.</span></span>  <span data-ttu-id="b10e6-215">Varje resurs i lösningen ska visas i antingen den **referencedResources** eller **containedResources** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-215">Each resource in the solution should be listed in either the **referencedResources** or the **containedResources** property.</span></span>

| <span data-ttu-id="b10e6-216">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b10e6-216">Property</span></span> | <span data-ttu-id="b10e6-217">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b10e6-217">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b10e6-218">workspaceResourceId</span><span class="sxs-lookup"><span data-stu-id="b10e6-218">workspaceResourceId</span></span> |<span data-ttu-id="b10e6-219">ID för logganalys-arbetsytan i formuläret  *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Arbetsytenamn\>*.</span><span class="sxs-lookup"><span data-stu-id="b10e6-219">ID of the Log Analytics workspace in the form *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Workspace Name\>*.</span></span> |
| <span data-ttu-id="b10e6-220">referencedResources</span><span class="sxs-lookup"><span data-stu-id="b10e6-220">referencedResources</span></span> |<span data-ttu-id="b10e6-221">Lista över resurser i lösningen som inte tas bort när lösningen tas bort.</span><span class="sxs-lookup"><span data-stu-id="b10e6-221">List of resources in the solution that should not be removed when the solution is removed.</span></span> |
| <span data-ttu-id="b10e6-222">containedResources</span><span class="sxs-lookup"><span data-stu-id="b10e6-222">containedResources</span></span> |<span data-ttu-id="b10e6-223">Lista över resurser i lösningen som ska tas bort när lösningen tas bort.</span><span class="sxs-lookup"><span data-stu-id="b10e6-223">List of resources in the solution that should be removed when the solution is removed.</span></span> |

<span data-ttu-id="b10e6-224">Exemplet ovan är för en lösning med en runbook ett schema och visa.</span><span class="sxs-lookup"><span data-stu-id="b10e6-224">The example  above is for a solution with a runbook, a schedule, and view.</span></span>  <span data-ttu-id="b10e6-225">Schemat och runbook är *refererade* i den **egenskaper** elementet så att de inte tas bort när lösningen tas bort.</span><span class="sxs-lookup"><span data-stu-id="b10e6-225">The schedule and runbook are *referenced* in the  **properties**  element so they are not removed when the solution is removed.</span></span>  <span data-ttu-id="b10e6-226">Vy *innehöll* så att den tas bort när lösningen tas bort.</span><span class="sxs-lookup"><span data-stu-id="b10e6-226">The view is *contained* so it is removed when the solution is removed.</span></span>

### <a name="plan"></a><span data-ttu-id="b10e6-227">Planera</span><span class="sxs-lookup"><span data-stu-id="b10e6-227">Plan</span></span>
<span data-ttu-id="b10e6-228">Den **plan** entiteten av lösningen resursen har egenskaper i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="b10e6-228">The **plan** entity of the solution resource has the properties in the following table.</span></span>

| <span data-ttu-id="b10e6-229">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b10e6-229">Property</span></span> | <span data-ttu-id="b10e6-230">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b10e6-230">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b10e6-231">namn</span><span class="sxs-lookup"><span data-stu-id="b10e6-231">name</span></span> |<span data-ttu-id="b10e6-232">Namnet för lösningen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-232">Name of the solution.</span></span> |
| <span data-ttu-id="b10e6-233">Version</span><span class="sxs-lookup"><span data-stu-id="b10e6-233">version</span></span> |<span data-ttu-id="b10e6-234">Version av lösningen som bestäms av författaren.</span><span class="sxs-lookup"><span data-stu-id="b10e6-234">Version of the solution as determined by the author.</span></span> |
| <span data-ttu-id="b10e6-235">Produkten</span><span class="sxs-lookup"><span data-stu-id="b10e6-235">product</span></span> |<span data-ttu-id="b10e6-236">Unik sträng som identifierar lösningen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-236">Unique string to identify the solution.</span></span> |
| <span data-ttu-id="b10e6-237">Publisher</span><span class="sxs-lookup"><span data-stu-id="b10e6-237">publisher</span></span> |<span data-ttu-id="b10e6-238">Utgivaren av lösningen.</span><span class="sxs-lookup"><span data-stu-id="b10e6-238">Publisher of the solution.</span></span> |



## <a name="sample"></a><span data-ttu-id="b10e6-239">Exempel</span><span class="sxs-lookup"><span data-stu-id="b10e6-239">Sample</span></span>
<span data-ttu-id="b10e6-240">Du kan visa prover av lösningsfiler med en resurs på följande platser.</span><span class="sxs-lookup"><span data-stu-id="b10e6-240">You can view samples of solution files with a solution resource at the following locations.</span></span>

- [<span data-ttu-id="b10e6-241">Automation-resurser</span><span class="sxs-lookup"><span data-stu-id="b10e6-241">Automation resources</span></span>](operations-management-suite-solutions-resources-automation.md#sample)
- [<span data-ttu-id="b10e6-242">Sök och avisering resurser</span><span class="sxs-lookup"><span data-stu-id="b10e6-242">Search and alert resources</span></span>](operations-management-suite-solutions-resources-searches-alerts.md#sample)


## <a name="next-steps"></a><span data-ttu-id="b10e6-243">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b10e6-243">Next steps</span></span>
* <span data-ttu-id="b10e6-244">[Lägg till sparade sökningar och aviseringar](operations-management-suite-solutions-resources-searches-alerts.md) att din lösning för hantering.</span><span class="sxs-lookup"><span data-stu-id="b10e6-244">[Add saved searches and alerts](operations-management-suite-solutions-resources-searches-alerts.md) to your management solution.</span></span>
* <span data-ttu-id="b10e6-245">[Lägga till vyer](operations-management-suite-solutions-resources-views.md) att din lösning för hantering.</span><span class="sxs-lookup"><span data-stu-id="b10e6-245">[Add views](operations-management-suite-solutions-resources-views.md) to your management solution.</span></span>
* <span data-ttu-id="b10e6-246">[Lägg till runbooks och andra resurser för Automation](operations-management-suite-solutions-resources-automation.md) att din lösning för hantering.</span><span class="sxs-lookup"><span data-stu-id="b10e6-246">[Add runbooks and other Automation resources](operations-management-suite-solutions-resources-automation.md) to your management solution.</span></span>
* <span data-ttu-id="b10e6-247">Mer information om [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b10e6-247">Learn the details of [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="b10e6-248">Sök [Azure Quickstart mallar](https://azure.microsoft.com/documentation/templates) exempel på olika Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="b10e6-248">Search [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates) for samples of different Resource Manager templates.</span></span>
