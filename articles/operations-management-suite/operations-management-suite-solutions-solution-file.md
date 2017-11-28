---
title: "aaaCreating hanteringslösningar i Operations Management Suite (OMS) | Microsoft Docs"
description: "Hanteringslösningar utöka hello funktioner i Operations Management Suite (OMS) genom att tillhandahålla paketerade hanteringsscenarier som kunder kan lägga till tootheir OMS-arbetsyta.  Den här artikeln innehåller information om hur du kan skapa management lösningar toobe används i din egen miljö eller göras tillgänglig tooyour kunder."
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
ms.openlocfilehash: f408df1b21f519fd1eb2cbeb19cca18f6c4161f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-a-management-solution-file-in-operations-management-suite-oms-preview"></a><span data-ttu-id="03817-104">Skapa en management lösningsfilen i Operations Management Suite (OMS) (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="03817-104">Creating a management solution file in Operations Management Suite (OMS) (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="03817-105">Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="03817-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="03817-106">Ett schema som beskrivs nedan är ämne toochange.</span><span class="sxs-lookup"><span data-stu-id="03817-106">Any schema described below is subject toochange.</span></span>  

<span data-ttu-id="03817-107">Lösningar för hantering i Operations Management Suite (OMS) implementeras som [Resource Manager-mallar](../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="03817-107">Management solutions in Operations Management Suite (OMS) are implemented as [Resource Manager templates](../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>  <span data-ttu-id="03817-108">hello huvudsakliga uppgiften lär du dig hur tooauthor hanteringslösningar learning hur för[skapar en mall](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="03817-108">hello main task in learning how tooauthor management solutions is learning how too[author a template](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>  <span data-ttu-id="03817-109">Den här artikeln innehåller unik information om mallar som används för lösningar och hur tooconfigure vanliga lösning resurser.</span><span class="sxs-lookup"><span data-stu-id="03817-109">This article provides unique details of templates used for solutions and how tooconfigure typical solution resources.</span></span>


## <a name="tools"></a><span data-ttu-id="03817-110">Verktyg</span><span class="sxs-lookup"><span data-stu-id="03817-110">Tools</span></span>

<span data-ttu-id="03817-111">Du kan använda alla text editor toowork med lösningsfiler, men vi rekommenderar att utnyttja hello-funktioner som finns i Visual Studio eller Visual Studio Code som beskrivs i följande artiklar hello.</span><span class="sxs-lookup"><span data-stu-id="03817-111">You can use any text editor toowork with solution files, but we recommend leveraging hello features provided in Visual Studio or Visual Studio Code as described in hello following articles.</span></span>

- [<span data-ttu-id="03817-112">Skapa och distribuera Azure-resursgrupper via Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03817-112">Creating and deploying Azure resource groups through Visual Studio</span></span>](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [<span data-ttu-id="03817-113">Arbeta med Azure Resource Manager-mallar i Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03817-113">Working with Azure Resource Manager Templates in Visual Studio Code</span></span>](../azure-resource-manager/resource-manager-vs-code.md)




## <a name="structure"></a><span data-ttu-id="03817-114">struktur</span><span class="sxs-lookup"><span data-stu-id="03817-114">Structure</span></span>
<span data-ttu-id="03817-115">hello grundstrukturen för ett management lösningsfilen är hello samma som en [Resource Manager-mall](../azure-resource-manager/resource-group-authoring-templates.md#template-format) som är som följer.</span><span class="sxs-lookup"><span data-stu-id="03817-115">hello basic structure of a management solution file is hello same as a [Resource Manager Template](../azure-resource-manager/resource-group-authoring-templates.md#template-format) which is as follows.</span></span>  <span data-ttu-id="03817-116">Hello avsnitten nedan beskrivs hello toppnivå element och och innehållet i en lösning.</span><span class="sxs-lookup"><span data-stu-id="03817-116">Each of hello sections below describes hello top level elements and and their contents in a solution.</span></span>  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a><span data-ttu-id="03817-117">Parametrar</span><span class="sxs-lookup"><span data-stu-id="03817-117">Parameters</span></span>
<span data-ttu-id="03817-118">[Parametrarna](../azure-resource-manager/resource-group-authoring-templates.md#parameters) är värden som du behöver från hello användare när de installerar hello hanteringslösning.</span><span class="sxs-lookup"><span data-stu-id="03817-118">[Parameters](../azure-resource-manager/resource-group-authoring-templates.md#parameters) are values that you require from hello user when they install hello management solution.</span></span>  <span data-ttu-id="03817-119">Det finns standardparametrar som har alla lösningar och du kan lägga till ytterligare parametrar som krävs för din lösning.</span><span class="sxs-lookup"><span data-stu-id="03817-119">There are standard parameters that all solutions will have, and you can add additional parameters as required for your particular solution.</span></span>  <span data-ttu-id="03817-120">Hur användare ange parametervärden när de installerar lösningen beror på hello viss parameter och hur hello lösningen installeras.</span><span class="sxs-lookup"><span data-stu-id="03817-120">How users will provide parameter values when they install your solution will depend on hello particular parameter and how hello solution is being installed.</span></span>

<span data-ttu-id="03817-121">När en användare installerar din lösning för hantering via hello [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) eller [Azure-snabbstartsmallar](operations-management-suite-solutions.md#finding-and-installing-management-solutions) ange tooselect är en [OMS arbetsytan och Automation-konto ](operations-management-suite-solutions.md#oms-workspace-and-automation-account).</span><span class="sxs-lookup"><span data-stu-id="03817-121">When a user installs your management solution through hello [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) or [Azure QuickStart templates](operations-management-suite-solutions.md#finding-and-installing-management-solutions) they are prompted tooselect an [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account).</span></span>  <span data-ttu-id="03817-122">Detta är används toopopulate hello värden för varje hello-standardparametrar.</span><span class="sxs-lookup"><span data-stu-id="03817-122">These are used toopopulate hello values of each of hello standard parameters.</span></span>  <span data-ttu-id="03817-123">hello användaren behöver inte ange toodirectly ange värden för standard hello-parametrar, men de kan ange tooprovide värden för alla ytterligare parametrar.</span><span class="sxs-lookup"><span data-stu-id="03817-123">hello user is not prompted toodirectly provide values for hello standard parameters, but they are prompted tooprovide values for any additional parameters.</span></span>

<span data-ttu-id="03817-124">När hello användaren installerar lösningen [en annan metod](operations-management-suite-solutions.md#finding-and-installing-management-solutions), måste de ange ett värde för alla parametrar som standard och alla ytterligare parametrar.</span><span class="sxs-lookup"><span data-stu-id="03817-124">When hello user installs your solution [another method](operations-management-suite-solutions.md#finding-and-installing-management-solutions), they must provide a value for all standard parameters and all additional parameters.</span></span>

<span data-ttu-id="03817-125">En exempel-parametern visas nedan.</span><span class="sxs-lookup"><span data-stu-id="03817-125">A sample parameter is shown below.</span></span>  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

<span data-ttu-id="03817-126">hello i den följande tabellen beskrivs hello attributen för en parameter.</span><span class="sxs-lookup"><span data-stu-id="03817-126">hello following table describes hello attributes of a parameter.</span></span>

| <span data-ttu-id="03817-127">Attribut</span><span class="sxs-lookup"><span data-stu-id="03817-127">Attribute</span></span> | <span data-ttu-id="03817-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="03817-128">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="03817-129">typ</span><span class="sxs-lookup"><span data-stu-id="03817-129">type</span></span> |<span data-ttu-id="03817-130">Datatypen för hello-parametern.</span><span class="sxs-lookup"><span data-stu-id="03817-130">Data type for hello parameter.</span></span> <span data-ttu-id="03817-131">hello inkommande kontrollen visas för användaren hello beror på hello-datatypen.</span><span class="sxs-lookup"><span data-stu-id="03817-131">hello input control displayed for hello user depends on hello data type.</span></span><br><br><span data-ttu-id="03817-132">bool - listrutan</span><span class="sxs-lookup"><span data-stu-id="03817-132">bool - Drop down box</span></span><br><span data-ttu-id="03817-133">String - textruta</span><span class="sxs-lookup"><span data-stu-id="03817-133">string - Text box</span></span><br><span data-ttu-id="03817-134">int - textruta</span><span class="sxs-lookup"><span data-stu-id="03817-134">int - Text box</span></span><br><span data-ttu-id="03817-135">SecureString - fält för lösenord</span><span class="sxs-lookup"><span data-stu-id="03817-135">securestring - Password field</span></span><br> |
| <span data-ttu-id="03817-136">category</span><span class="sxs-lookup"><span data-stu-id="03817-136">category</span></span> |<span data-ttu-id="03817-137">Valfri kategori för hello-parametern.</span><span class="sxs-lookup"><span data-stu-id="03817-137">Optional category for hello parameter.</span></span>  <span data-ttu-id="03817-138">Parametrar i hello samma kategori grupperas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="03817-138">Parameters in hello same category are grouped together.</span></span> |
| <span data-ttu-id="03817-139">Kontrollen</span><span class="sxs-lookup"><span data-stu-id="03817-139">control</span></span> |<span data-ttu-id="03817-140">Ytterligare funktioner för string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="03817-140">Additional functionality for string parameters.</span></span><br><br><span data-ttu-id="03817-141">datetime - Datetime kontrollen visas.</span><span class="sxs-lookup"><span data-stu-id="03817-141">datetime - Datetime control is displayed.</span></span><br><span data-ttu-id="03817-142">GUID - Guid-värde genereras automatiskt och visas inte hello-parametern.</span><span class="sxs-lookup"><span data-stu-id="03817-142">guid - Guid value is automatically generated, and hello parameter is not displayed.</span></span> |
| <span data-ttu-id="03817-143">description</span><span class="sxs-lookup"><span data-stu-id="03817-143">description</span></span> |<span data-ttu-id="03817-144">Valfri beskrivning för hello-parametern.</span><span class="sxs-lookup"><span data-stu-id="03817-144">Optional description for hello parameter.</span></span>  <span data-ttu-id="03817-145">Visas i en information pratbubblor nästa toohello-parameter.</span><span class="sxs-lookup"><span data-stu-id="03817-145">Displayed in an information balloon next toohello parameter.</span></span> |

### <a name="standard-parameters"></a><span data-ttu-id="03817-146">Standard-parametrar</span><span class="sxs-lookup"><span data-stu-id="03817-146">Standard parameters</span></span>
<span data-ttu-id="03817-147">hello visas följande tabell hello-standardparametrar för alla hanteringslösningar för.</span><span class="sxs-lookup"><span data-stu-id="03817-147">hello following table lists hello standard parameters for all management solutions.</span></span>  <span data-ttu-id="03817-148">Dessa värden är fyllda för hello användaren i stället för att fråga om dem när lösningen har installerats från hello Azure Marketplace eller Quickstart mallar.</span><span class="sxs-lookup"><span data-stu-id="03817-148">These values are populated for hello user instead of prompting for them when your solution is installed from hello Azure Marketplace or Quickstart templates.</span></span>  <span data-ttu-id="03817-149">användaren hello måste ange värden för dem om hello lösningen har installerats med en annan metod.</span><span class="sxs-lookup"><span data-stu-id="03817-149">hello user must provide values for them if hello solution is installed with another method.</span></span>

> [!NOTE]
> <span data-ttu-id="03817-150">hello användargränssnittet i hello mallar för Azure Marketplace och Quickstart förväntas hello parameternamn i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="03817-150">hello user interface in hello Azure Marketplace and Quickstart templates is expecting hello parameter names in hello table.</span></span>  <span data-ttu-id="03817-151">Om du använder olika parameternamn sedan hello användaren uppmanas att dem och fylls de inte automatiskt.</span><span class="sxs-lookup"><span data-stu-id="03817-151">If you use different parameter names then hello user will be prompted for them, and they will not be automatically populated.</span></span>
>
>

| <span data-ttu-id="03817-152">Parameter</span><span class="sxs-lookup"><span data-stu-id="03817-152">Parameter</span></span> | <span data-ttu-id="03817-153">Typ</span><span class="sxs-lookup"><span data-stu-id="03817-153">Type</span></span> | <span data-ttu-id="03817-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="03817-154">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="03817-155">Kontonamn</span><span class="sxs-lookup"><span data-stu-id="03817-155">accountName</span></span> |<span data-ttu-id="03817-156">Sträng</span><span class="sxs-lookup"><span data-stu-id="03817-156">string</span></span> |<span data-ttu-id="03817-157">Azure Automation-kontonamn.</span><span class="sxs-lookup"><span data-stu-id="03817-157">Azure Automation account name.</span></span> |
| <span data-ttu-id="03817-158">pricingTier</span><span class="sxs-lookup"><span data-stu-id="03817-158">pricingTier</span></span> |<span data-ttu-id="03817-159">Sträng</span><span class="sxs-lookup"><span data-stu-id="03817-159">string</span></span> |<span data-ttu-id="03817-160">Prisnivån för både logganalys-arbetsytan och Azure Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="03817-160">Pricing tier of both Log Analytics workspace and Azure Automation account.</span></span> |
| <span data-ttu-id="03817-161">regionId</span><span class="sxs-lookup"><span data-stu-id="03817-161">regionId</span></span> |<span data-ttu-id="03817-162">Sträng</span><span class="sxs-lookup"><span data-stu-id="03817-162">string</span></span> |<span data-ttu-id="03817-163">Region i hello Azure Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="03817-163">Region of hello Azure Automation account.</span></span> |
| <span data-ttu-id="03817-164">SolutionName</span><span class="sxs-lookup"><span data-stu-id="03817-164">solutionName</span></span> |<span data-ttu-id="03817-165">Sträng</span><span class="sxs-lookup"><span data-stu-id="03817-165">string</span></span> |<span data-ttu-id="03817-166">Namnet på hello lösning.</span><span class="sxs-lookup"><span data-stu-id="03817-166">Name of hello solution.</span></span>  <span data-ttu-id="03817-167">Om du distribuerar din lösning med snabbstartsmallar bör sedan du definiera solutionName som en parameter där du kan definiera en sträng i stället kräver hello användaren toospecify en.</span><span class="sxs-lookup"><span data-stu-id="03817-167">If you are deploying your solution through Quickstart templates, then you should define solutionName as a parameter so you can define a string instead requiring hello user toospecify one.</span></span> |
| <span data-ttu-id="03817-168">workspaceName</span><span class="sxs-lookup"><span data-stu-id="03817-168">workspaceName</span></span> |<span data-ttu-id="03817-169">Sträng</span><span class="sxs-lookup"><span data-stu-id="03817-169">string</span></span> |<span data-ttu-id="03817-170">Log Analytics arbetsytans namn.</span><span class="sxs-lookup"><span data-stu-id="03817-170">Log Analytics workspace name.</span></span> |
| <span data-ttu-id="03817-171">workspaceRegionId</span><span class="sxs-lookup"><span data-stu-id="03817-171">workspaceRegionId</span></span> |<span data-ttu-id="03817-172">Sträng</span><span class="sxs-lookup"><span data-stu-id="03817-172">string</span></span> |<span data-ttu-id="03817-173">Region i hello logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="03817-173">Region of hello Log Analytics workspace.</span></span> |


<span data-ttu-id="03817-174">Följande är hello strukturen för hello-standardparametrar som du kan kopiera och klistra in i din lösningsfilen.</span><span class="sxs-lookup"><span data-stu-id="03817-174">Following is hello structure of hello standard parameters that you can copy and paste into your solution file.</span></span>  

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
                   "description": "Region of hello Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of hello Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        }
    }


<span data-ttu-id="03817-175">Du referera tooparameter värden i andra element för hello-lösning med hello syntax **parametrar (parametern name)**.</span><span class="sxs-lookup"><span data-stu-id="03817-175">You refer tooparameter values in other elements of hello solution with hello syntax **parameters('parameter name')**.</span></span>  <span data-ttu-id="03817-176">Till exempel tooaccess hello arbetsytans namn, använder du **parameters('workspaceName')**</span><span class="sxs-lookup"><span data-stu-id="03817-176">For example, tooaccess hello workspace name, you would use **parameters('workspaceName')**</span></span>

## <a name="variables"></a><span data-ttu-id="03817-177">Variabler</span><span class="sxs-lookup"><span data-stu-id="03817-177">Variables</span></span>
<span data-ttu-id="03817-178">[Variabler](../azure-resource-manager/resource-group-authoring-templates.md#variables) är värden som du ska använda i hello resten av hello hanteringslösning.</span><span class="sxs-lookup"><span data-stu-id="03817-178">[Variables](../azure-resource-manager/resource-group-authoring-templates.md#variables) are values that you will use in hello rest of hello management solution.</span></span>  <span data-ttu-id="03817-179">Dessa värden är inte synliga toohello användare som installerar hello lösning.</span><span class="sxs-lookup"><span data-stu-id="03817-179">These values are not exposed toohello user installing hello solution.</span></span>  <span data-ttu-id="03817-180">De är avsedda tooprovide hello författare med en enda plats där de kan hantera värden som kan användas flera gånger i hela hello lösning.</span><span class="sxs-lookup"><span data-stu-id="03817-180">They are intended tooprovide hello author with a single location where they can manage values that may be used multiple times throughout hello solution.</span></span> <span data-ttu-id="03817-181">Du bör placera alla värden specifika tooyour lösningar på variabler som skillnad från toohard kodning dem i hello **resurser** element.</span><span class="sxs-lookup"><span data-stu-id="03817-181">You should put any values specific tooyour solution in variables as opposed toohard coding them in hello **resources** element.</span></span>  <span data-ttu-id="03817-182">Det gör det lättare att läsa hello koden och att du tooeasily ändra dessa värden i senare versioner.</span><span class="sxs-lookup"><span data-stu-id="03817-182">This makes hello code more readable and allows you tooeasily change these values in later versions.</span></span>

<span data-ttu-id="03817-183">Följande är ett exempel på en **variabler** element med vanliga parametrar som används i lösningar.</span><span class="sxs-lookup"><span data-stu-id="03817-183">Following is an example of a **variables** element with typical parameters used in solutions.</span></span>

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

<span data-ttu-id="03817-184">Du referera toovariable värden via hello-lösning med hello syntax **variabler (variabel namn)**.</span><span class="sxs-lookup"><span data-stu-id="03817-184">You refer toovariable values through hello solution with hello syntax **variables('variable name')**.</span></span>  <span data-ttu-id="03817-185">Till exempel tooaccess hello SolutionName variabeln, använder du **variables('SolutionName')**.</span><span class="sxs-lookup"><span data-stu-id="03817-185">For example, tooaccess hello SolutionName variable, you would use **variables('SolutionName')**.</span></span>

<span data-ttu-id="03817-186">Du kan också definiera komplex variabler som flera uppsättningar av värden.</span><span class="sxs-lookup"><span data-stu-id="03817-186">You can also define complex variables that multiple sets of values.</span></span>  <span data-ttu-id="03817-187">Detta är särskilt användbart i hanteringslösningar där du definierar flera egenskaper för olika typer av resurser.</span><span class="sxs-lookup"><span data-stu-id="03817-187">These are particularly useful in management solutions where you are defining multiple properties for different types of resources.</span></span>  <span data-ttu-id="03817-188">Du kan till exempel omstrukturera hello lösning variabler ovan toohello följande.</span><span class="sxs-lookup"><span data-stu-id="03817-188">For example, you could restructure hello solution variables shown above toohello following.</span></span>

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

<span data-ttu-id="03817-189">I så fall måste du referera toovariable värden via hello-lösning med hello syntax **variables('variable name').property**.</span><span class="sxs-lookup"><span data-stu-id="03817-189">In this case, you refer toovariable values through hello solution with hello syntax **variables('variable name').property**.</span></span>  <span data-ttu-id="03817-190">Till exempel tooaccess hello lösningsnamn variabeln, använder du **variables('Solution'). Namnet**.</span><span class="sxs-lookup"><span data-stu-id="03817-190">For example, tooaccess hello Solution Name variable, you would use **variables('Solution').Name**.</span></span>

## <a name="resources"></a><span data-ttu-id="03817-191">Resurser</span><span class="sxs-lookup"><span data-stu-id="03817-191">Resources</span></span>
<span data-ttu-id="03817-192">[Resurser](../azure-resource-manager/resource-group-authoring-templates.md#resources) definiera hello olika resurser som din lösning ska installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="03817-192">[Resources](../azure-resource-manager/resource-group-authoring-templates.md#resources) define hello different resources that your management solution will install and configure.</span></span>  <span data-ttu-id="03817-193">Det här är hello största och mest komplexa del av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="03817-193">This will be hello largest and most complex portion of hello template.</span></span>  <span data-ttu-id="03817-194">Du kan hämta hello struktur och fullständig beskrivning av resursen element i [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md#resources).</span><span class="sxs-lookup"><span data-stu-id="03817-194">You can get hello structure and complete description of resource elements in [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md#resources).</span></span>  <span data-ttu-id="03817-195">Olika resurser som du vanligtvis definierar beskrivs i andra artiklar i den här dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="03817-195">Different resources that you will typically define are detailed in other articles in this documentation.</span></span> 


### <a name="dependencies"></a><span data-ttu-id="03817-196">Beroenden</span><span class="sxs-lookup"><span data-stu-id="03817-196">Dependencies</span></span>
<span data-ttu-id="03817-197">Hej **dependsOn** element anger en [beroende](../azure-resource-manager/resource-group-define-dependencies.md) på en annan resurs.</span><span class="sxs-lookup"><span data-stu-id="03817-197">hello **dependsOn** elements specifies a [dependency](../azure-resource-manager/resource-group-define-dependencies.md) on another resource.</span></span>  <span data-ttu-id="03817-198">När hello lösningen installeras, skapas inte en resurs tills alla dess beroenden har skapats.</span><span class="sxs-lookup"><span data-stu-id="03817-198">When hello solution is installed, a resource is not created until all of its dependencies have been created.</span></span>  <span data-ttu-id="03817-199">Till exempel din lösning kan [startar en runbook](operations-management-suite-solutions-resources-automation.md#runbooks) när den är installerad med hjälp av en [jobbet resurs](operations-management-suite-solutions-resources-automation.md#automation-jobs).</span><span class="sxs-lookup"><span data-stu-id="03817-199">For example, your solution might [start a runbook](operations-management-suite-solutions-resources-automation.md#runbooks) when it's installed using a [job resource](operations-management-suite-solutions-resources-automation.md#automation-jobs).</span></span>  <span data-ttu-id="03817-200">hello jobbet resurs skulle vara beroende av hello runbook resurs toomake till att hello runbook har skapats innan hello jobb skapas.</span><span class="sxs-lookup"><span data-stu-id="03817-200">hello job resource would be dependent on hello runbook resource toomake sure that hello runbook is created before hello job is created.</span></span>

### <a name="oms-workspace-and-automation-account"></a><span data-ttu-id="03817-201">OMS-arbetsytan och Automation-konto</span><span class="sxs-lookup"><span data-stu-id="03817-201">OMS workspace and Automation account</span></span>
<span data-ttu-id="03817-202">Av hanteringslösningar kräver en [OMS-arbetsytan](../log-analytics/log-analytics-manage-access.md) toocontain vyer och en [Automation-konto](../automation/automation-security-overview.md#automation-account-overview) toocontain runbooks och relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="03817-202">Management solutions require an [OMS workspace](../log-analytics/log-analytics-manage-access.md) toocontain views and an [Automation account](../automation/automation-security-overview.md#automation-account-overview) toocontain runbooks and related resources.</span></span>  <span data-ttu-id="03817-203">Dessa måste vara tillgänglig innan hello resurser i hello lösningen skapas och ska inte definieras i själva hello-lösningen.</span><span class="sxs-lookup"><span data-stu-id="03817-203">These must be available before hello resources in hello solution are created and should not be defined in hello solution itself.</span></span>  <span data-ttu-id="03817-204">hello användaren kommer [ange arbetsytan och kontot](operations-management-suite-solutions.md#oms-workspace-and-automation-account) när de distribuerar din lösning, men som hello författare bör du hello följande punkter.</span><span class="sxs-lookup"><span data-stu-id="03817-204">hello user will [specify a workspace and account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) when they deploy your solution, but as hello author you should consider hello following points.</span></span>

## <a name="solution-resource"></a><span data-ttu-id="03817-205">Lösning för resurs</span><span class="sxs-lookup"><span data-stu-id="03817-205">Solution resource</span></span>
<span data-ttu-id="03817-206">Varje lösning kräver att en resurs i hello **resurser** element som definierar själva hello-lösningen.</span><span class="sxs-lookup"><span data-stu-id="03817-206">Each solution requires a resource entry in hello **resources** element that defines hello solution itself.</span></span>  <span data-ttu-id="03817-207">Det har en typ av **Microsoft.OperationsManagement/solutions** och ha hello följande struktur.</span><span class="sxs-lookup"><span data-stu-id="03817-207">This will have a type of **Microsoft.OperationsManagement/solutions** and have hello following structure.</span></span> <span data-ttu-id="03817-208">Detta inkluderar [standardparametrar](#parameters) och [variabler](#variables) som är vanliga toodefine egenskaper för hello lösning.</span><span class="sxs-lookup"><span data-stu-id="03817-208">This includes [standard parameters](#parameters) and [variables](#variables) that are typically used toodefine properties of hello solution.</span></span>


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




### <a name="dependencies"></a><span data-ttu-id="03817-209">Beroenden</span><span class="sxs-lookup"><span data-stu-id="03817-209">Dependencies</span></span>
<span data-ttu-id="03817-210">hello lösning resursen måste ha en [beroende](../azure-resource-manager/resource-group-define-dependencies.md) på alla andra resurser i hello lösning eftersom de behöver tooexist innan hello-lösning kan skapas.</span><span class="sxs-lookup"><span data-stu-id="03817-210">hello solution resource must have a [dependency](../azure-resource-manager/resource-group-define-dependencies.md) on every other resource in hello solution since they need tooexist before hello solution can be created.</span></span>  <span data-ttu-id="03817-211">Det gör du genom att lägga till en post för varje resurs i hello **dependsOn** element.</span><span class="sxs-lookup"><span data-stu-id="03817-211">You do this by adding an entry for each resource in hello **dependsOn** element.</span></span>

### <a name="properties"></a><span data-ttu-id="03817-212">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="03817-212">Properties</span></span>
<span data-ttu-id="03817-213">hello resurs har hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="03817-213">hello solution resource has hello properties in hello following table.</span></span>  <span data-ttu-id="03817-214">Detta inkluderar hello resurser refereras och finns i hello-lösning som definierar hur hello resursen hanteras när hello lösningen har installerats.</span><span class="sxs-lookup"><span data-stu-id="03817-214">This includes hello resources referenced and contained by hello solution which defines how hello resource is managed after hello solution is installed.</span></span>  <span data-ttu-id="03817-215">Varje resurs i hello lösningen ska visas i antingen hello **referencedResources** eller hello **containedResources** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="03817-215">Each resource in hello solution should be listed in either hello **referencedResources** or hello **containedResources** property.</span></span>

| <span data-ttu-id="03817-216">Egenskap</span><span class="sxs-lookup"><span data-stu-id="03817-216">Property</span></span> | <span data-ttu-id="03817-217">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="03817-217">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="03817-218">workspaceResourceId</span><span class="sxs-lookup"><span data-stu-id="03817-218">workspaceResourceId</span></span> |<span data-ttu-id="03817-219">ID för hello logganalys-arbetsytan i form av hello  *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Arbetsytenamn\>*.</span><span class="sxs-lookup"><span data-stu-id="03817-219">ID of hello Log Analytics workspace in hello form *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Workspace Name\>*.</span></span> |
| <span data-ttu-id="03817-220">referencedResources</span><span class="sxs-lookup"><span data-stu-id="03817-220">referencedResources</span></span> |<span data-ttu-id="03817-221">Lista över resurser i hello-lösning som inte tas bort när hello lösningen tas bort.</span><span class="sxs-lookup"><span data-stu-id="03817-221">List of resources in hello solution that should not be removed when hello solution is removed.</span></span> |
| <span data-ttu-id="03817-222">containedResources</span><span class="sxs-lookup"><span data-stu-id="03817-222">containedResources</span></span> |<span data-ttu-id="03817-223">Listan över resurser i hello-lösning som ska tas bort när hello lösningen tas bort.</span><span class="sxs-lookup"><span data-stu-id="03817-223">List of resources in hello solution that should be removed when hello solution is removed.</span></span> |

<span data-ttu-id="03817-224">hello-exemplet ovan är för en lösning med en runbook ett schema och visa.</span><span class="sxs-lookup"><span data-stu-id="03817-224">hello example  above is for a solution with a runbook, a schedule, and view.</span></span>  <span data-ttu-id="03817-225">hello-schemat och runbook är *refererade* i hello **egenskaper** elementet så att de inte tas bort när hello lösningen tas bort.</span><span class="sxs-lookup"><span data-stu-id="03817-225">hello schedule and runbook are *referenced* in hello  **properties**  element so they are not removed when hello solution is removed.</span></span>  <span data-ttu-id="03817-226">hello är *innehöll* så att den tas bort när hello lösningen tas bort.</span><span class="sxs-lookup"><span data-stu-id="03817-226">hello view is *contained* so it is removed when hello solution is removed.</span></span>

### <a name="plan"></a><span data-ttu-id="03817-227">Planera</span><span class="sxs-lookup"><span data-stu-id="03817-227">Plan</span></span>
<span data-ttu-id="03817-228">Hej **plan** entiteten av hello resurs har hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="03817-228">hello **plan** entity of hello solution resource has hello properties in hello following table.</span></span>

| <span data-ttu-id="03817-229">Egenskap</span><span class="sxs-lookup"><span data-stu-id="03817-229">Property</span></span> | <span data-ttu-id="03817-230">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="03817-230">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="03817-231">namn</span><span class="sxs-lookup"><span data-stu-id="03817-231">name</span></span> |<span data-ttu-id="03817-232">Namnet på hello lösning.</span><span class="sxs-lookup"><span data-stu-id="03817-232">Name of hello solution.</span></span> |
| <span data-ttu-id="03817-233">Version</span><span class="sxs-lookup"><span data-stu-id="03817-233">version</span></span> |<span data-ttu-id="03817-234">Version av hello lösningen utifrån hello författare.</span><span class="sxs-lookup"><span data-stu-id="03817-234">Version of hello solution as determined by hello author.</span></span> |
| <span data-ttu-id="03817-235">Produkten</span><span class="sxs-lookup"><span data-stu-id="03817-235">product</span></span> |<span data-ttu-id="03817-236">Unik sträng tooidentify hello lösning.</span><span class="sxs-lookup"><span data-stu-id="03817-236">Unique string tooidentify hello solution.</span></span> |
| <span data-ttu-id="03817-237">Publisher</span><span class="sxs-lookup"><span data-stu-id="03817-237">publisher</span></span> |<span data-ttu-id="03817-238">Utgivaren av hello lösning.</span><span class="sxs-lookup"><span data-stu-id="03817-238">Publisher of hello solution.</span></span> |



## <a name="sample"></a><span data-ttu-id="03817-239">Exempel</span><span class="sxs-lookup"><span data-stu-id="03817-239">Sample</span></span>
<span data-ttu-id="03817-240">Du kan visa prover av lösningsfiler med en resurs på hello följande platser.</span><span class="sxs-lookup"><span data-stu-id="03817-240">You can view samples of solution files with a solution resource at hello following locations.</span></span>

- [<span data-ttu-id="03817-241">Automation-resurser</span><span class="sxs-lookup"><span data-stu-id="03817-241">Automation resources</span></span>](operations-management-suite-solutions-resources-automation.md#sample)
- [<span data-ttu-id="03817-242">Sök och avisering resurser</span><span class="sxs-lookup"><span data-stu-id="03817-242">Search and alert resources</span></span>](operations-management-suite-solutions-resources-searches-alerts.md#sample)


## <a name="next-steps"></a><span data-ttu-id="03817-243">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="03817-243">Next steps</span></span>
* <span data-ttu-id="03817-244">[Lägg till sparade sökningar och aviseringar](operations-management-suite-solutions-resources-searches-alerts.md) tooyour hanteringslösning.</span><span class="sxs-lookup"><span data-stu-id="03817-244">[Add saved searches and alerts](operations-management-suite-solutions-resources-searches-alerts.md) tooyour management solution.</span></span>
* <span data-ttu-id="03817-245">[Lägga till vyer](operations-management-suite-solutions-resources-views.md) tooyour hanteringslösning.</span><span class="sxs-lookup"><span data-stu-id="03817-245">[Add views](operations-management-suite-solutions-resources-views.md) tooyour management solution.</span></span>
* <span data-ttu-id="03817-246">[Lägg till runbooks och andra resurser för Automation](operations-management-suite-solutions-resources-automation.md) tooyour hanteringslösning.</span><span class="sxs-lookup"><span data-stu-id="03817-246">[Add runbooks and other Automation resources](operations-management-suite-solutions-resources-automation.md) tooyour management solution.</span></span>
* <span data-ttu-id="03817-247">Läs hello information om [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="03817-247">Learn hello details of [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="03817-248">Sök [Azure Quickstart mallar](https://azure.microsoft.com/documentation/templates) exempel på olika Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="03817-248">Search [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates) for samples of different Resource Manager templates.</span></span>
