---
title: aaaAzure Resource Manager mallstruktur och syntax | Microsoft Docs
description: "Beskriver hello struktur och egenskaperna för Azure Resource Manager-mallar med deklarativ JSON-syntax."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2017
ms.author: tomfitz
ms.openlocfilehash: b0709852f8777c91cc1704d6bca16257a017d515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-hello-structure-and-syntax-of-azure-resource-manager-templates"></a><span data-ttu-id="8efb7-103">Förstå hello struktur och syntaxen för Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="8efb7-103">Understand hello structure and syntax of Azure Resource Manager templates</span></span>
<span data-ttu-id="8efb7-104">Det här avsnittet beskriver hello strukturen i en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="8efb7-104">This topic describes hello structure of an Azure Resource Manager template.</span></span> <span data-ttu-id="8efb7-105">Den visar hello olika avsnitt i en mall och hello egenskaper som är tillgängliga i dessa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8efb7-105">It presents hello different sections of a template and hello properties that are available in those sections.</span></span> <span data-ttu-id="8efb7-106">hello mallen består av JSON och uttryck som du kan använda tooconstruct värden för din distribution.</span><span class="sxs-lookup"><span data-stu-id="8efb7-106">hello template consists of JSON and expressions that you can use tooconstruct values for your deployment.</span></span> <span data-ttu-id="8efb7-107">En stegvis självstudiekurs om hur du skapar en mall finns i [skapa din första Azure Resource Manager-mallen](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-107">For a step-by-step tutorial on creating a template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>

## <a name="template-format"></a><span data-ttu-id="8efb7-108">Mallformat</span><span class="sxs-lookup"><span data-stu-id="8efb7-108">Template format</span></span>
<span data-ttu-id="8efb7-109">I sin enklaste strukturen innehåller en mall hello följande element:</span><span class="sxs-lookup"><span data-stu-id="8efb7-109">In its simplest structure, a template contains hello following elements:</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "resources": [  ],
    "outputs": {  }
}
```

| <span data-ttu-id="8efb7-110">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="8efb7-110">Element name</span></span> | <span data-ttu-id="8efb7-111">Krävs</span><span class="sxs-lookup"><span data-stu-id="8efb7-111">Required</span></span> | <span data-ttu-id="8efb7-112">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8efb7-112">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="8efb7-113">$schema</span><span class="sxs-lookup"><span data-stu-id="8efb7-113">$schema</span></span> |<span data-ttu-id="8efb7-114">Ja</span><span class="sxs-lookup"><span data-stu-id="8efb7-114">Yes</span></span> |<span data-ttu-id="8efb7-115">Sökväg till hello JSON-schema som beskriver hello-versionen av hello mallen språket.</span><span class="sxs-lookup"><span data-stu-id="8efb7-115">Location of hello JSON schema file that describes hello version of hello template language.</span></span> <span data-ttu-id="8efb7-116">Använd hello-URL som visas i föregående exempel hello.</span><span class="sxs-lookup"><span data-stu-id="8efb7-116">Use hello URL shown in hello preceding example.</span></span> |
| <span data-ttu-id="8efb7-117">contentVersion</span><span class="sxs-lookup"><span data-stu-id="8efb7-117">contentVersion</span></span> |<span data-ttu-id="8efb7-118">Ja</span><span class="sxs-lookup"><span data-stu-id="8efb7-118">Yes</span></span> |<span data-ttu-id="8efb7-119">Version av hello mall (till exempel 1.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="8efb7-119">Version of hello template (such as 1.0.0.0).</span></span> <span data-ttu-id="8efb7-120">Du kan ange ett värde för det här elementet.</span><span class="sxs-lookup"><span data-stu-id="8efb7-120">You can provide any value for this element.</span></span> <span data-ttu-id="8efb7-121">När du distribuerar resurser med hjälp av mallen hello kan värdet vara används toomake till att rätt hello-mallen används.</span><span class="sxs-lookup"><span data-stu-id="8efb7-121">When deploying resources using hello template, this value can be used toomake sure that hello right template is being used.</span></span> |
| <span data-ttu-id="8efb7-122">parameters</span><span class="sxs-lookup"><span data-stu-id="8efb7-122">parameters</span></span> |<span data-ttu-id="8efb7-123">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-123">No</span></span> |<span data-ttu-id="8efb7-124">Värden som anges när distributionen är köras toocustomize resurs distribution.</span><span class="sxs-lookup"><span data-stu-id="8efb7-124">Values that are provided when deployment is executed toocustomize resource deployment.</span></span> |
| <span data-ttu-id="8efb7-125">variabler</span><span class="sxs-lookup"><span data-stu-id="8efb7-125">variables</span></span> |<span data-ttu-id="8efb7-126">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-126">No</span></span> |<span data-ttu-id="8efb7-127">Värden som används som JSON-fragment i hello mallen toosimplify mallspråksuttryck.</span><span class="sxs-lookup"><span data-stu-id="8efb7-127">Values that are used as JSON fragments in hello template toosimplify template language expressions.</span></span> |
| <span data-ttu-id="8efb7-128">Resurser</span><span class="sxs-lookup"><span data-stu-id="8efb7-128">resources</span></span> |<span data-ttu-id="8efb7-129">Ja</span><span class="sxs-lookup"><span data-stu-id="8efb7-129">Yes</span></span> |<span data-ttu-id="8efb7-130">Resurstyper som distribuerats eller uppdateras i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="8efb7-130">Resource types that are deployed or updated in a resource group.</span></span> |
| <span data-ttu-id="8efb7-131">utdata</span><span class="sxs-lookup"><span data-stu-id="8efb7-131">outputs</span></span> |<span data-ttu-id="8efb7-132">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-132">No</span></span> |<span data-ttu-id="8efb7-133">Värden som returneras efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="8efb7-133">Values that are returned after deployment.</span></span> |

<span data-ttu-id="8efb7-134">Varje element innehåller egenskaper som du kan ange.</span><span class="sxs-lookup"><span data-stu-id="8efb7-134">Each element contains properties you can set.</span></span> <span data-ttu-id="8efb7-135">hello följande exempel innehåller hello fullständig syntax för en mall:</span><span class="sxs-lookup"><span data-stu-id="8efb7-135">hello following example contains hello full syntax for a template:</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  
        "<parameter-name>" : {
            "type" : "<type-of-parameter-value>",
            "defaultValue": "<default-value-of-parameter>",
            "allowedValues": [ "<array-of-allowed-values>" ],
            "minValue": <minimum-value-for-int>,
            "maxValue": <maximum-value-for-int>,
            "minLength": <minimum-length-for-string-or-array>,
            "maxLength": <maximum-length-for-string-or-array-parameters>,
            "metadata": {
                "description": "<description-of-hello parameter>" 
            }
        }
    },
    "variables": {  
        "<variable-name>": "<variable-value>",
        "<variable-name>": { 
            <variable-complex-type-value> 
        }
    },
    "resources": [
      {
          "condition": "<boolean-value-whether-to-deploy>",
          "apiVersion": "<api-version-of-resource>",
          "type": "<resource-provider-namespace/resource-type-name>",
          "name": "<name-of-the-resource>",
          "location": "<location-of-resource>",
          "tags": {
              "<tag-name1>": "<tag-value1>",
              "<tag-name2>": "<tag-value2>"
          },
          "comments": "<your-reference-notes>",
          "copy": {
              "name": "<name-of-copy-loop>",
              "count": "<number-of-iterations>",
              "mode": "<serial-or-parallel>",
              "batchSize": "<number-to-deploy-serially>"
          },
          "dependsOn": [
              "<array-of-related-resource-names>"
          ],
          "properties": {
              "<settings-for-the-resource>",
              "copy": [
                  {
                      "name": ,
                      "count": ,
                      "input": {}
                  }
              ]
          },
          "resources": [
              "<array-of-child-resources>"
          ]
      }
    ],
    "outputs": {
        "<outputName>" : {
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

<span data-ttu-id="8efb7-136">Vi undersöka hello avsnitt i hello mallen i större detalj längre fram i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="8efb7-136">We examine hello sections of hello template in greater detail later in this topic.</span></span>

## <a name="expressions-and-functions"></a><span data-ttu-id="8efb7-137">Uttryck och funktioner</span><span class="sxs-lookup"><span data-stu-id="8efb7-137">Expressions and functions</span></span>
<span data-ttu-id="8efb7-138">grundläggande hello-syntaxen för hello mallen är JSON.</span><span class="sxs-lookup"><span data-stu-id="8efb7-138">hello basic syntax of hello template is JSON.</span></span> <span data-ttu-id="8efb7-139">Dock utöka uttryck och funktioner hello JSON-värden som är tillgängliga inom hello mall.</span><span class="sxs-lookup"><span data-stu-id="8efb7-139">However, expressions and functions extend hello JSON values available within hello template.</span></span>  <span data-ttu-id="8efb7-140">Uttryck skrivs i JSON-stränglitteraler vars första och sista tecknen är hello hakparenteser: `[` och `]`respektive.</span><span class="sxs-lookup"><span data-stu-id="8efb7-140">Expressions are written within JSON string literals whose first and last characters are hello brackets: `[` and `]`, respectively.</span></span> <span data-ttu-id="8efb7-141">hello-värdet för hello uttrycket utvärderas när hello mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="8efb7-141">hello value of hello expression is evaluated when hello template is deployed.</span></span> <span data-ttu-id="8efb7-142">Medan skrivs som en teckensträng kan hello resultat av utvärderingen av hello uttryck vara av en annan JSON-typ, till exempel en matris eller ett heltal, beroende på hello faktiska uttryck.</span><span class="sxs-lookup"><span data-stu-id="8efb7-142">While written as a string literal, hello result of evaluating hello expression can be of a different JSON type, such as an array or integer, depending on hello actual expression.</span></span>  <span data-ttu-id="8efb7-143">toohave som en teckensträng börja med en hakparentes `[`, men inte har det tolkas som ett uttryck, lägga till en extra hakparentes toostart hello sträng med `[[`.</span><span class="sxs-lookup"><span data-stu-id="8efb7-143">toohave a literal string start with a bracket `[`, but not have it interpreted as an expression, add an extra bracket toostart hello string with `[[`.</span></span>

<span data-ttu-id="8efb7-144">Normalt använder du uttryck med funktioner tooperform åtgärder för att konfigurera hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="8efb7-144">Typically, you use expressions with functions tooperform operations for configuring hello deployment.</span></span> <span data-ttu-id="8efb7-145">Precis som i JavaScript-funktionsanrop som är formaterade som `functionName(arg1,arg2,arg3)`.</span><span class="sxs-lookup"><span data-stu-id="8efb7-145">Just like in JavaScript, function calls are formatted as `functionName(arg1,arg2,arg3)`.</span></span> <span data-ttu-id="8efb7-146">Du kan referera egenskaper med hjälp av hello punkt och [index] operatörer.</span><span class="sxs-lookup"><span data-stu-id="8efb7-146">You reference properties by using hello dot and [index] operators.</span></span>

<span data-ttu-id="8efb7-147">Hej följande exempel visar hur toouse flera fungerar när man skapar värden:</span><span class="sxs-lookup"><span data-stu-id="8efb7-147">hello following example shows how toouse several functions when constructing values:</span></span>

```json
"variables": {
    "location": "[resourceGroup().location]",
    "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
    "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
}
```

<span data-ttu-id="8efb7-148">Hello fullständig lista över Mallfunktioner kan se [Azure Resource Manager Mallfunktioner](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-148">For hello full list of template functions, see [Azure Resource Manager template functions](resource-group-template-functions.md).</span></span> 

## <a name="parameters"></a><span data-ttu-id="8efb7-149">Parametrar</span><span class="sxs-lookup"><span data-stu-id="8efb7-149">Parameters</span></span>
<span data-ttu-id="8efb7-150">I hello parametrar i hello mall kan ange du vilka värden som du kan ange när du distribuerar hello resurser.</span><span class="sxs-lookup"><span data-stu-id="8efb7-150">In hello parameters section of hello template, you specify which values you can input when deploying hello resources.</span></span> <span data-ttu-id="8efb7-151">Dessa parametervärden aktivera toocustomize hello distributionen med värden som är anpassade för en viss miljö (t.ex dev, test- och).</span><span class="sxs-lookup"><span data-stu-id="8efb7-151">These parameter values enable you toocustomize hello deployment by providing values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="8efb7-152">Du har inte tooprovide parametrar i mallen, men utan parametrar mallen skulle du alltid distribuera hello hello i samma resurser med samma namn, platser och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8efb7-152">You do not have tooprovide parameters in your template, but without parameters your template would always deploy hello same resources with hello same names, locations, and properties.</span></span>

<span data-ttu-id="8efb7-153">Du kan definiera parametrar med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="8efb7-153">You define parameters with hello following structure:</span></span>

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-hello parameter>" 
        }
    }
}
```

| <span data-ttu-id="8efb7-154">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="8efb7-154">Element name</span></span> | <span data-ttu-id="8efb7-155">Krävs</span><span class="sxs-lookup"><span data-stu-id="8efb7-155">Required</span></span> | <span data-ttu-id="8efb7-156">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8efb7-156">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="8efb7-157">parameterName</span><span class="sxs-lookup"><span data-stu-id="8efb7-157">parameterName</span></span> |<span data-ttu-id="8efb7-158">Ja</span><span class="sxs-lookup"><span data-stu-id="8efb7-158">Yes</span></span> |<span data-ttu-id="8efb7-159">Namnet på hello-parameter.</span><span class="sxs-lookup"><span data-stu-id="8efb7-159">Name of hello parameter.</span></span> <span data-ttu-id="8efb7-160">Måste vara en giltig JavaScript-identifierare.</span><span class="sxs-lookup"><span data-stu-id="8efb7-160">Must be a valid JavaScript identifier.</span></span> |
| <span data-ttu-id="8efb7-161">typ</span><span class="sxs-lookup"><span data-stu-id="8efb7-161">type</span></span> |<span data-ttu-id="8efb7-162">Ja</span><span class="sxs-lookup"><span data-stu-id="8efb7-162">Yes</span></span> |<span data-ttu-id="8efb7-163">Typ av hello parametervärdet.</span><span class="sxs-lookup"><span data-stu-id="8efb7-163">Type of hello parameter value.</span></span> <span data-ttu-id="8efb7-164">Se hello lista över tillåtna typer efter den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="8efb7-164">See hello list of allowed types after this table.</span></span> |
| <span data-ttu-id="8efb7-165">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="8efb7-165">defaultValue</span></span> |<span data-ttu-id="8efb7-166">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-166">No</span></span> |<span data-ttu-id="8efb7-167">Standardvärdet för hello parameter om inget värde har angetts för parametern hello.</span><span class="sxs-lookup"><span data-stu-id="8efb7-167">Default value for hello parameter, if no value is provided for hello parameter.</span></span> |
| <span data-ttu-id="8efb7-168">allowedValues</span><span class="sxs-lookup"><span data-stu-id="8efb7-168">allowedValues</span></span> |<span data-ttu-id="8efb7-169">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-169">No</span></span> |<span data-ttu-id="8efb7-170">Matris med tillåtna värdena för hello parametern toomake du att rätt hello-värdet är angett.</span><span class="sxs-lookup"><span data-stu-id="8efb7-170">Array of allowed values for hello parameter toomake sure that hello right value is provided.</span></span> |
| <span data-ttu-id="8efb7-171">MinValue</span><span class="sxs-lookup"><span data-stu-id="8efb7-171">minValue</span></span> |<span data-ttu-id="8efb7-172">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-172">No</span></span> |<span data-ttu-id="8efb7-173">hello minimivärdet för int typparametrar, det här värdet är inklusiva.</span><span class="sxs-lookup"><span data-stu-id="8efb7-173">hello minimum value for int type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="8efb7-174">MaxValue</span><span class="sxs-lookup"><span data-stu-id="8efb7-174">maxValue</span></span> |<span data-ttu-id="8efb7-175">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-175">No</span></span> |<span data-ttu-id="8efb7-176">hello högsta värdet för int typparametrar, det här värdet är inklusiva.</span><span class="sxs-lookup"><span data-stu-id="8efb7-176">hello maximum value for int type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="8efb7-177">minLength</span><span class="sxs-lookup"><span data-stu-id="8efb7-177">minLength</span></span> |<span data-ttu-id="8efb7-178">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-178">No</span></span> |<span data-ttu-id="8efb7-179">hello minimilängden för string, secureString och array typparametrar, det här värdet är inklusiva.</span><span class="sxs-lookup"><span data-stu-id="8efb7-179">hello minimum length for string, secureString, and array type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="8efb7-180">maxLength</span><span class="sxs-lookup"><span data-stu-id="8efb7-180">maxLength</span></span> |<span data-ttu-id="8efb7-181">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-181">No</span></span> |<span data-ttu-id="8efb7-182">hello maxlängden för string, secureString och array typparametrar, det här värdet är inklusiva.</span><span class="sxs-lookup"><span data-stu-id="8efb7-182">hello maximum length for string, secureString, and array type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="8efb7-183">description</span><span class="sxs-lookup"><span data-stu-id="8efb7-183">description</span></span> |<span data-ttu-id="8efb7-184">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-184">No</span></span> |<span data-ttu-id="8efb7-185">Beskrivning av hello parameter visas toousers hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="8efb7-185">Description of hello parameter that is displayed toousers through hello portal.</span></span> |

<span data-ttu-id="8efb7-186">hello tillåtna typer och värden är:</span><span class="sxs-lookup"><span data-stu-id="8efb7-186">hello allowed types and values are:</span></span>

* <span data-ttu-id="8efb7-187">**sträng**</span><span class="sxs-lookup"><span data-stu-id="8efb7-187">**string**</span></span>
* <span data-ttu-id="8efb7-188">**secureString**</span><span class="sxs-lookup"><span data-stu-id="8efb7-188">**secureString**</span></span>
* <span data-ttu-id="8efb7-189">**int**</span><span class="sxs-lookup"><span data-stu-id="8efb7-189">**int**</span></span>
* <span data-ttu-id="8efb7-190">**bool**</span><span class="sxs-lookup"><span data-stu-id="8efb7-190">**bool**</span></span>
* <span data-ttu-id="8efb7-191">**objektet**</span><span class="sxs-lookup"><span data-stu-id="8efb7-191">**object**</span></span> 
* <span data-ttu-id="8efb7-192">**secureObject**</span><span class="sxs-lookup"><span data-stu-id="8efb7-192">**secureObject**</span></span>
* <span data-ttu-id="8efb7-193">**matris**</span><span class="sxs-lookup"><span data-stu-id="8efb7-193">**array**</span></span>

<span data-ttu-id="8efb7-194">toospecify en parameter som valfria, ange defaultValue (kan vara en tom sträng).</span><span class="sxs-lookup"><span data-stu-id="8efb7-194">toospecify a parameter as optional, provide a defaultValue (can be an empty string).</span></span> 

<span data-ttu-id="8efb7-195">Om du anger ett parameternamn i mallen som matchar en parameter i hello kommandot toodeploy hello mallen, finns det potentiella tvetydighet om hello-värden som du anger.</span><span class="sxs-lookup"><span data-stu-id="8efb7-195">If you specify a parameter name in your template that matches a parameter in hello command toodeploy hello template, there is potential ambiguity about hello values you provide.</span></span> <span data-ttu-id="8efb7-196">Hanteraren för filserverresurser löser den här förvirring genom att lägga till hello username@Domain **från mall** toohello mallparameter.</span><span class="sxs-lookup"><span data-stu-id="8efb7-196">Resource Manager resolves this confusion by adding hello postfix **FromTemplate** toohello template parameter.</span></span> <span data-ttu-id="8efb7-197">Till exempel om du lägger till en parameter med namnet **ResourceGroupName** i mallen, den orsakar en konflikt med hello **ResourceGroupName** parameter i hello [ Nya AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8efb7-197">For example, if you include a parameter named **ResourceGroupName** in your template, it conflicts with hello **ResourceGroupName** parameter in hello [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet.</span></span> <span data-ttu-id="8efb7-198">Under distributionen, är du tillfrågas tooprovide ett värde för **ResourceGroupNameFromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="8efb7-198">During deployment, you are prompted tooprovide a value for **ResourceGroupNameFromTemplate**.</span></span> <span data-ttu-id="8efb7-199">I allmänhet bör du undvika den här förvirring genom att inte namnges parametrar med samma namn som parametrar som används för distributionsåtgärder hello.</span><span class="sxs-lookup"><span data-stu-id="8efb7-199">In general, you should avoid this confusion by not naming parameters with hello same name as parameters used for deployment operations.</span></span>

> [!NOTE]
> <span data-ttu-id="8efb7-200">Alla lösenord, nycklar och andra hemligheter ska använda hello **secureString** typen.</span><span class="sxs-lookup"><span data-stu-id="8efb7-200">All passwords, keys, and other secrets should use hello **secureString** type.</span></span> <span data-ttu-id="8efb7-201">Om du skickar känsliga data i en JSON-objekt använder hello **secureObject** typen.</span><span class="sxs-lookup"><span data-stu-id="8efb7-201">If you pass sensitive data in a JSON object, use hello **secureObject** type.</span></span> <span data-ttu-id="8efb7-202">Mallparametrar med secureString eller secureObject typer går inte att läsa efter resurs distributionen.</span><span class="sxs-lookup"><span data-stu-id="8efb7-202">Template parameters with secureString or secureObject types cannot be read after resource deployment.</span></span> 
> 
> <span data-ttu-id="8efb7-203">Till exempel visar hello följande post i hello distributionshistoriken hello värdet för en sträng och objekt men inte för secureString och secureObject.</span><span class="sxs-lookup"><span data-stu-id="8efb7-203">For example, hello following entry in hello deployment history shows hello value for a string and object but not for secureString and secureObject.</span></span>
>
> ![Visa värden för distribution](./media/resource-group-authoring-templates/show-parameters.png)  
>

<span data-ttu-id="8efb7-205">följande exempel visar hur hello toodefine parametrar:</span><span class="sxs-lookup"><span data-stu-id="8efb7-205">hello following example shows how toodefine parameters:</span></span>

```json
"parameters": {
    "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
    },
    "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
    },
    "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
    },
    "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
    }
}
```

<span data-ttu-id="8efb7-206">Hur tooinput hello parametern värden under distributionen finns [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-206">For how tooinput hello parameter values during deployment, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span> 

## <a name="variables"></a><span data-ttu-id="8efb7-207">Variabler</span><span class="sxs-lookup"><span data-stu-id="8efb7-207">Variables</span></span>
<span data-ttu-id="8efb7-208">Under hello variabler skapa värden som kan användas i hela din mall.</span><span class="sxs-lookup"><span data-stu-id="8efb7-208">In hello variables section, you construct values that can be used throughout your template.</span></span> <span data-ttu-id="8efb7-209">Du behöver inte toodefine variabler, men de förenkla ofta din mall genom att minska komplexa uttryck.</span><span class="sxs-lookup"><span data-stu-id="8efb7-209">You do not need toodefine variables, but they often simplify your template by reducing complex expressions.</span></span>

<span data-ttu-id="8efb7-210">Du kan definiera variabler med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="8efb7-210">You define variables with hello following structure:</span></span>

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    }
}
```

<span data-ttu-id="8efb7-211">följande exempel visar hur hello toodefine en variabel som konstrueras utifrån två parametervärden:</span><span class="sxs-lookup"><span data-stu-id="8efb7-211">hello following example shows how toodefine a variable that is constructed from two parameter values:</span></span>

```json
"variables": {
    "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
}
```

<span data-ttu-id="8efb7-212">hello nästa exempel visas en variabel som är en komplex typ av JSON och variabler som skapas från andra variabler:</span><span class="sxs-lookup"><span data-stu-id="8efb7-212">hello next example shows a variable that is a complex JSON type, and variables that are constructed from other variables:</span></span>

```json
"parameters": {
    "environmentName": {
        "type": "string",
        "allowedValues": [
          "test",
          "prod"
        ]
    }
},
"variables": {
    "environmentSettings": {
        "test": {
            "instancesSize": "Small",
            "instancesCount": 1
        },
        "prod": {
            "instancesSize": "Large",
            "instancesCount": 4
        }
    },
    "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
    "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
    "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
}
```

## <a name="resources"></a><span data-ttu-id="8efb7-213">Resurser</span><span class="sxs-lookup"><span data-stu-id="8efb7-213">Resources</span></span>
<span data-ttu-id="8efb7-214">Under hello resurser kan du definiera hello resurser som distribueras eller uppdateras.</span><span class="sxs-lookup"><span data-stu-id="8efb7-214">In hello resources section, you define hello resources that are deployed or updated.</span></span> <span data-ttu-id="8efb7-215">Det här avsnittet får komplicerade eftersom du måste förstå hello skriver du distribuerar tooprovide hello rätt värden.</span><span class="sxs-lookup"><span data-stu-id="8efb7-215">This section can get complicated because you must understand hello types you are deploying tooprovide hello right values.</span></span> <span data-ttu-id="8efb7-216">Hej resursspecifika värden (apiVersion, typ och egenskaper) som du behöver tooset hittar [definiera resurser i Azure Resource Manager-mallar](/azure/templates/).</span><span class="sxs-lookup"><span data-stu-id="8efb7-216">For hello resource-specific values (apiVersion, type, and properties) that you need tooset, see [Define resources in Azure Resource Manager templates](/azure/templates/).</span></span> 

<span data-ttu-id="8efb7-217">Du kan definiera resurser med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="8efb7-217">You define resources with hello following structure:</span></span>

```json
"resources": [
  {
      "condition": "<boolean-value-whether-to-deploy>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": "<number-of-iterations>",
          "mode": "<serial-or-parallel>",
          "batchSize": "<number-to-deploy-serially>"
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| <span data-ttu-id="8efb7-218">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="8efb7-218">Element name</span></span> | <span data-ttu-id="8efb7-219">Krävs</span><span class="sxs-lookup"><span data-stu-id="8efb7-219">Required</span></span> | <span data-ttu-id="8efb7-220">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8efb7-220">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="8efb7-221">Villkor</span><span class="sxs-lookup"><span data-stu-id="8efb7-221">condition</span></span> | <span data-ttu-id="8efb7-222">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-222">No</span></span> | <span data-ttu-id="8efb7-223">Booleskt värde som anger om hello resurs har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="8efb7-223">Boolean value that indicates whether hello resource is deployed.</span></span> |
| <span data-ttu-id="8efb7-224">apiVersion</span><span class="sxs-lookup"><span data-stu-id="8efb7-224">apiVersion</span></span> |<span data-ttu-id="8efb7-225">Ja</span><span class="sxs-lookup"><span data-stu-id="8efb7-225">Yes</span></span> |<span data-ttu-id="8efb7-226">Version av hello REST API toouse för att skapa hello resurs.</span><span class="sxs-lookup"><span data-stu-id="8efb7-226">Version of hello REST API toouse for creating hello resource.</span></span> |
| <span data-ttu-id="8efb7-227">typ</span><span class="sxs-lookup"><span data-stu-id="8efb7-227">type</span></span> |<span data-ttu-id="8efb7-228">Ja</span><span class="sxs-lookup"><span data-stu-id="8efb7-228">Yes</span></span> |<span data-ttu-id="8efb7-229">Typ av hello resurs.</span><span class="sxs-lookup"><span data-stu-id="8efb7-229">Type of hello resource.</span></span> <span data-ttu-id="8efb7-230">Det här värdet är en kombination av hello namnområde med hello resursprovidern och resurstypen hello (exempelvis **Microsoft.Storage/storageAccounts**).</span><span class="sxs-lookup"><span data-stu-id="8efb7-230">This value is a combination of hello namespace of hello resource provider and hello resource type (such as **Microsoft.Storage/storageAccounts**).</span></span> |
| <span data-ttu-id="8efb7-231">namn</span><span class="sxs-lookup"><span data-stu-id="8efb7-231">name</span></span> |<span data-ttu-id="8efb7-232">Ja</span><span class="sxs-lookup"><span data-stu-id="8efb7-232">Yes</span></span> |<span data-ttu-id="8efb7-233">Namn på hello resurs.</span><span class="sxs-lookup"><span data-stu-id="8efb7-233">Name of hello resource.</span></span> <span data-ttu-id="8efb7-234">hello-namnet måste följa URI komponenten begränsningar som definierats i RFC3986.</span><span class="sxs-lookup"><span data-stu-id="8efb7-234">hello name must follow URI component restrictions defined in RFC3986.</span></span> <span data-ttu-id="8efb7-235">Dessutom Azure-tjänster som visar hello resurs namn toooutside parter Validera hello namnet toomake att den inte är en försök toospoof en annan identitet.</span><span class="sxs-lookup"><span data-stu-id="8efb7-235">In addition, Azure services that expose hello resource name toooutside parties validate hello name toomake sure it is not an attempt toospoof another identity.</span></span> |
| <span data-ttu-id="8efb7-236">location</span><span class="sxs-lookup"><span data-stu-id="8efb7-236">location</span></span> |<span data-ttu-id="8efb7-237">Det varierar</span><span class="sxs-lookup"><span data-stu-id="8efb7-237">Varies</span></span> |<span data-ttu-id="8efb7-238">Geo-platser som stöds av hello som resurs.</span><span class="sxs-lookup"><span data-stu-id="8efb7-238">Supported geo-locations of hello provided resource.</span></span> <span data-ttu-id="8efb7-239">Du kan välja någon av hello tillgängliga platser, men vanligtvis det gör meningsfullt toopick ett som är nära tooyour användare.</span><span class="sxs-lookup"><span data-stu-id="8efb7-239">You can select any of hello available locations, but typically it makes sense toopick one that is close tooyour users.</span></span> <span data-ttu-id="8efb7-240">Vanligtvis är det också klokt tooplace resurser som samverkar med varandra i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="8efb7-240">Usually, it also makes sense tooplace resources that interact with each other in hello same region.</span></span> <span data-ttu-id="8efb7-241">De flesta resurstyper kräver en plats, men vissa typer (till exempel en rolltilldelning) kräver inte en plats.</span><span class="sxs-lookup"><span data-stu-id="8efb7-241">Most resource types require a location, but some types (such as a role assignment) do not require a location.</span></span> <span data-ttu-id="8efb7-242">Se [som resursplats i Azure Resource Manager-mallar](resource-manager-template-location.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-242">See [Set resource location in Azure Resource Manager templates](resource-manager-template-location.md).</span></span> |
| <span data-ttu-id="8efb7-243">tags</span><span class="sxs-lookup"><span data-stu-id="8efb7-243">tags</span></span> |<span data-ttu-id="8efb7-244">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-244">No</span></span> |<span data-ttu-id="8efb7-245">Taggar som är associerade med hello resurs.</span><span class="sxs-lookup"><span data-stu-id="8efb7-245">Tags that are associated with hello resource.</span></span> <span data-ttu-id="8efb7-246">Se [tagga resurser i Azure Resource Manager-mallar](resource-manager-template-tags.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-246">See [Tag resources in Azure Resource Manager templates](resource-manager-template-tags.md).</span></span> |
| <span data-ttu-id="8efb7-247">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="8efb7-247">comments</span></span> |<span data-ttu-id="8efb7-248">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-248">No</span></span> |<span data-ttu-id="8efb7-249">Anteckningar för dokumentation hello resurser i mallen</span><span class="sxs-lookup"><span data-stu-id="8efb7-249">Your notes for documenting hello resources in your template</span></span> |
| <span data-ttu-id="8efb7-250">Kopiera</span><span class="sxs-lookup"><span data-stu-id="8efb7-250">copy</span></span> |<span data-ttu-id="8efb7-251">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-251">No</span></span> |<span data-ttu-id="8efb7-252">Hej antalet resurser toocreate om mer än en instans krävs.</span><span class="sxs-lookup"><span data-stu-id="8efb7-252">If more than one instance is needed, hello number of resources toocreate.</span></span> <span data-ttu-id="8efb7-253">hello standardläget är parallellt.</span><span class="sxs-lookup"><span data-stu-id="8efb7-253">hello default mode is parallel.</span></span> <span data-ttu-id="8efb7-254">Ange seriell läge när du inte vill att alla eller hello resurser toodeploy på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="8efb7-254">Specify serial mode when you do not want all or hello resources toodeploy at hello same time.</span></span> <span data-ttu-id="8efb7-255">Mer information finns i [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-255">For more information, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span> |
| <span data-ttu-id="8efb7-256">dependsOn</span><span class="sxs-lookup"><span data-stu-id="8efb7-256">dependsOn</span></span> |<span data-ttu-id="8efb7-257">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-257">No</span></span> |<span data-ttu-id="8efb7-258">Resurser som måste distribueras innan den här resursen har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="8efb7-258">Resources that must be deployed before this resource is deployed.</span></span> <span data-ttu-id="8efb7-259">Resource Manager utvärderar hello beroenden mellan resurser och distribuerar dem i hello rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="8efb7-259">Resource Manager evaluates hello dependencies between resources and deploys them in hello correct order.</span></span> <span data-ttu-id="8efb7-260">Om resurserna inte är beroende av varandra kan distribueras de parallellt.</span><span class="sxs-lookup"><span data-stu-id="8efb7-260">When resources are not dependent on each other, they are deployed in parallel.</span></span> <span data-ttu-id="8efb7-261">hello värdet kan vara en kommaavgränsad lista över en resurs namn eller resurs unika identifierare.</span><span class="sxs-lookup"><span data-stu-id="8efb7-261">hello value can be a comma-separated list of a resource names or resource unique identifiers.</span></span> <span data-ttu-id="8efb7-262">Endast lista över resurser som distribueras i den här mallen.</span><span class="sxs-lookup"><span data-stu-id="8efb7-262">Only list resources that are deployed in this template.</span></span> <span data-ttu-id="8efb7-263">Resurser som inte har definierats i denna mall måste redan finnas.</span><span class="sxs-lookup"><span data-stu-id="8efb7-263">Resources that are not defined in this template must already exist.</span></span> <span data-ttu-id="8efb7-264">Undvik att lägga till onödiga beroenden som de långsamma distributionen och skapa Cirkelberoenden.</span><span class="sxs-lookup"><span data-stu-id="8efb7-264">Avoid adding unnecessary dependencies as they can slow your deployment and create circular dependencies.</span></span> <span data-ttu-id="8efb7-265">Information om inställningen beroenden finns [definiera beroenden i Azure Resource Manager-mallar](resource-group-define-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-265">For guidance on setting dependencies, see [Defining dependencies in Azure Resource Manager templates](resource-group-define-dependencies.md).</span></span> |
| <span data-ttu-id="8efb7-266">properties</span><span class="sxs-lookup"><span data-stu-id="8efb7-266">properties</span></span> |<span data-ttu-id="8efb7-267">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-267">No</span></span> |<span data-ttu-id="8efb7-268">Resurs-specifika konfigurationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="8efb7-268">Resource-specific configuration settings.</span></span> <span data-ttu-id="8efb7-269">hello-värden för egenskaper för hello hello samma som hello-värden som du anger i hello begärandetexten för hello REST-API (PUT-metoden) toocreate hello resursen.</span><span class="sxs-lookup"><span data-stu-id="8efb7-269">hello values for hello properties are hello same as hello values you provide in hello request body for hello REST API operation (PUT method) toocreate hello resource.</span></span> <span data-ttu-id="8efb7-270">Du kan också ange en kopia matris toocreate flera instanser av en egenskap.</span><span class="sxs-lookup"><span data-stu-id="8efb7-270">You can also specify a copy array toocreate multiple instances of a property.</span></span> <span data-ttu-id="8efb7-271">Mer information finns i [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-271">For more information, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span> |
| <span data-ttu-id="8efb7-272">Resurser</span><span class="sxs-lookup"><span data-stu-id="8efb7-272">resources</span></span> |<span data-ttu-id="8efb7-273">Nej</span><span class="sxs-lookup"><span data-stu-id="8efb7-273">No</span></span> |<span data-ttu-id="8efb7-274">Underordnade resurser som är beroende av hello resurs som definieras.</span><span class="sxs-lookup"><span data-stu-id="8efb7-274">Child resources that depend on hello resource being defined.</span></span> <span data-ttu-id="8efb7-275">Ange endast resurstyper som tillåts av hello schemat för hello överordnade resursen.</span><span class="sxs-lookup"><span data-stu-id="8efb7-275">Only provide resource types that are permitted by hello schema of hello parent resource.</span></span> <span data-ttu-id="8efb7-276">hello fullständiga typ av hello underordnade resursen innehåller hello överordnade resurstyp, t.ex. **Microsoft.Web/sites/extensions**.</span><span class="sxs-lookup"><span data-stu-id="8efb7-276">hello fully qualified type of hello child resource includes hello parent resource type, such as **Microsoft.Web/sites/extensions**.</span></span> <span data-ttu-id="8efb7-277">Beroende på hello överordnade resursen är inte underförstådd.</span><span class="sxs-lookup"><span data-stu-id="8efb7-277">Dependency on hello parent resource is not implied.</span></span> <span data-ttu-id="8efb7-278">Du måste uttryckligen definiera sambandet.</span><span class="sxs-lookup"><span data-stu-id="8efb7-278">You must explicitly define that dependency.</span></span> |

<span data-ttu-id="8efb7-279">hello resurser avsnittet innehåller en matris med hello resurser toodeploy.</span><span class="sxs-lookup"><span data-stu-id="8efb7-279">hello resources section contains an array of hello resources toodeploy.</span></span> <span data-ttu-id="8efb7-280">Du kan också definiera en matris med underordnade resurser inom varje resurs.</span><span class="sxs-lookup"><span data-stu-id="8efb7-280">Within each resource, you can also define an array of child resources.</span></span> <span data-ttu-id="8efb7-281">Resurser-avsnitt kan därför ha en struktur som:</span><span class="sxs-lookup"><span data-stu-id="8efb7-281">Therefore, your resources section could have a structure like:</span></span>

```json
"resources": [
  {
      "name": "resourceA",
  },
  {
      "name": "resourceB",
      "resources": [
        {
            "name": "firstChildResourceB",
        },
        {   
            "name": "secondChildResourceB",
        }
      ]
  },
  {
      "name": "resourceC",
  }
]
```      

<span data-ttu-id="8efb7-282">Mer information om hur du definierar underordnade resurser finns [ange namn och typ för underordnade resursen i Resource Manager-mall](resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-282">For more information about defining child resources, see [Set name and type for child resource in Resource Manager template](resource-manager-template-child-resource.md).</span></span>

<span data-ttu-id="8efb7-283">Hej **villkoret** elementet anger huruvida hello resurs har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="8efb7-283">hello **condition** element specifies whether hello resource is deployed.</span></span> <span data-ttu-id="8efb7-284">hello-värdet för det här elementet löser tootrue eller false.</span><span class="sxs-lookup"><span data-stu-id="8efb7-284">hello value for this element resolves tootrue or false.</span></span> <span data-ttu-id="8efb7-285">Till exempel toospecify om ett nytt lagringskonto har distribuerats, Använd:</span><span class="sxs-lookup"><span data-stu-id="8efb7-285">For example, toospecify whether a new storage account is deployed, use:</span></span>

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

<span data-ttu-id="8efb7-286">Ett exempel på hur du använder en ny eller befintlig resurs finns [ny eller befintlig mall för villkoret](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span><span class="sxs-lookup"><span data-stu-id="8efb7-286">For an example of using a new or existing resource, see [New or existing condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span></span>

<span data-ttu-id="8efb7-287">toospecify om en virtuell dator distribueras med ett lösenord eller SSH-nyckeln du definierar två versioner av hello virtuell dator för i din mall och använda **villkoret** toodifferentiate användning.</span><span class="sxs-lookup"><span data-stu-id="8efb7-287">toospecify whether a virtual machine is deployed with a password or SSH key, define two versions of hello virtual machine in your template and use **condition** toodifferentiate usage.</span></span> <span data-ttu-id="8efb7-288">Skicka en parameter som anger vilket scenario toodeploy.</span><span class="sxs-lookup"><span data-stu-id="8efb7-288">Pass a parameter that specifies which scenario toodeploy.</span></span>

```json
{
    "condition": "[equals(parameters('passwordOrSshKey'),'password')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'password')]",
    "properties": {
        "osProfile": {
            "computerName": "[variables('vmName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
        },
        ...
    },
    ...
},
{
    "condition": "[equals(parameters('passwordOrSshKey'),'sshKey')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'ssh')]",
    "properties": {
        "osProfile": {
            "linuxConfiguration": {
                "disablePasswordAuthentication": "true",
                "ssh": {
                    "publicKeys": [
                        {
                            "path": "[variables('sshKeyPath')]",
                            "keyData": "[parameters('adminSshKey')]"
                        }
                    ]
                }
            }
        },
        ...
    },
    ...
}
``` 

<span data-ttu-id="8efb7-289">Ett exempel på med ett lösenord eller SSH-nyckel toodeploy virtuella finns [användarnamn eller SSH villkoret mallen](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span><span class="sxs-lookup"><span data-stu-id="8efb7-289">For an example of using a password or SSH key toodeploy virtual machine, see [Username or SSH condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span></span>

## <a name="outputs"></a><span data-ttu-id="8efb7-290">utdata</span><span class="sxs-lookup"><span data-stu-id="8efb7-290">Outputs</span></span>
<span data-ttu-id="8efb7-291">Ange värden som returneras från distribution under hello utdata.</span><span class="sxs-lookup"><span data-stu-id="8efb7-291">In hello Outputs section, you specify values that are returned from deployment.</span></span> <span data-ttu-id="8efb7-292">Du kan till exempel returnera hello URI tooaccess en distribuerad resurs.</span><span class="sxs-lookup"><span data-stu-id="8efb7-292">For example, you could return hello URI tooaccess a deployed resource.</span></span>

<span data-ttu-id="8efb7-293">hello visar följande exempel hello struktur för en definition av utdata:</span><span class="sxs-lookup"><span data-stu-id="8efb7-293">hello following example shows hello structure of an output definition:</span></span>

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| <span data-ttu-id="8efb7-294">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="8efb7-294">Element name</span></span> | <span data-ttu-id="8efb7-295">Krävs</span><span class="sxs-lookup"><span data-stu-id="8efb7-295">Required</span></span> | <span data-ttu-id="8efb7-296">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8efb7-296">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="8efb7-297">outputName</span><span class="sxs-lookup"><span data-stu-id="8efb7-297">outputName</span></span> |<span data-ttu-id="8efb7-298">Ja</span><span class="sxs-lookup"><span data-stu-id="8efb7-298">Yes</span></span> |<span data-ttu-id="8efb7-299">Namnet på hello utdatavärde.</span><span class="sxs-lookup"><span data-stu-id="8efb7-299">Name of hello output value.</span></span> <span data-ttu-id="8efb7-300">Måste vara en giltig JavaScript-identifierare.</span><span class="sxs-lookup"><span data-stu-id="8efb7-300">Must be a valid JavaScript identifier.</span></span> |
| <span data-ttu-id="8efb7-301">typ</span><span class="sxs-lookup"><span data-stu-id="8efb7-301">type</span></span> |<span data-ttu-id="8efb7-302">Ja</span><span class="sxs-lookup"><span data-stu-id="8efb7-302">Yes</span></span> |<span data-ttu-id="8efb7-303">Typ av hello utdatavärde.</span><span class="sxs-lookup"><span data-stu-id="8efb7-303">Type of hello output value.</span></span> <span data-ttu-id="8efb7-304">Utdatavärden stöder hello samma typer som mall indataparametrar.</span><span class="sxs-lookup"><span data-stu-id="8efb7-304">Output values support hello same types as template input parameters.</span></span> |
| <span data-ttu-id="8efb7-305">värde</span><span class="sxs-lookup"><span data-stu-id="8efb7-305">value</span></span> |<span data-ttu-id="8efb7-306">Ja</span><span class="sxs-lookup"><span data-stu-id="8efb7-306">Yes</span></span> |<span data-ttu-id="8efb7-307">Mallspråksuttrycket som utvärderas och returneras som utdata.</span><span class="sxs-lookup"><span data-stu-id="8efb7-307">Template language expression that is evaluated and returned as output value.</span></span> |

<span data-ttu-id="8efb7-308">hello visar följande exempel ett värde som returneras i hello utdata avsnittet.</span><span class="sxs-lookup"><span data-stu-id="8efb7-308">hello following example shows a value that is returned in hello Outputs section.</span></span>

```json
"outputs": {
    "siteUri" : {
        "type" : "string",
        "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
    }
}
```

<span data-ttu-id="8efb7-309">Mer information om hur du arbetar med utdata finns [dela tillstånd i Azure Resource Manager-mallar](best-practices-resource-manager-state.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-309">For more information about working with output, see [Sharing state in Azure Resource Manager templates](best-practices-resource-manager-state.md).</span></span>

## <a name="template-limits"></a><span data-ttu-id="8efb7-310">Mallen gränser</span><span class="sxs-lookup"><span data-stu-id="8efb7-310">Template limits</span></span>

<span data-ttu-id="8efb7-311">Begränsa hello av mall-too1 MB och varje parameter fil too64 KB.</span><span class="sxs-lookup"><span data-stu-id="8efb7-311">Limit hello size of your template too1 MB, and each parameter file too64 KB.</span></span> <span data-ttu-id="8efb7-312">hello 1 MB gränsen gäller toohello sluttillstånd för hello mall när den har utökats med iterativ resursdefinitionerna och värden för parametrar och variabler.</span><span class="sxs-lookup"><span data-stu-id="8efb7-312">hello 1-MB limit applies toohello final state of hello template after it has been expanded with iterative resource definitions, and values for variables and parameters.</span></span> 

<span data-ttu-id="8efb7-313">Du är begränsad till:</span><span class="sxs-lookup"><span data-stu-id="8efb7-313">You are also limited to:</span></span>

* <span data-ttu-id="8efb7-314">256 parametrar</span><span class="sxs-lookup"><span data-stu-id="8efb7-314">256 parameters</span></span>
* <span data-ttu-id="8efb7-315">256 variabler</span><span class="sxs-lookup"><span data-stu-id="8efb7-315">256 variables</span></span>
* <span data-ttu-id="8efb7-316">800 resurserna (inklusive antal kopior)</span><span class="sxs-lookup"><span data-stu-id="8efb7-316">800 resources (including copy count)</span></span>
* <span data-ttu-id="8efb7-317">64 utdatavärden</span><span class="sxs-lookup"><span data-stu-id="8efb7-317">64 output values</span></span>
* <span data-ttu-id="8efb7-318">24,576 tecken i ett malluttryck för</span><span class="sxs-lookup"><span data-stu-id="8efb7-318">24,576 characters in a template expression</span></span>

<span data-ttu-id="8efb7-319">Du kan överskrida vissa mallen med hjälp av en kapslad mall.</span><span class="sxs-lookup"><span data-stu-id="8efb7-319">You can exceed some template limits by using a nested template.</span></span> <span data-ttu-id="8efb7-320">Mer information finns i [använda länkade mallar när du distribuerar Azure-resurser](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-320">For more information, see [Using linked templates when deploying Azure resources](resource-group-linked-templates.md).</span></span> <span data-ttu-id="8efb7-321">tooreduce hello antalet parametrar, variabler eller utdata, du kan kombinera flera värden i ett objekt.</span><span class="sxs-lookup"><span data-stu-id="8efb7-321">tooreduce hello number of parameters, variables, or outputs, you can combine several values into an object.</span></span> <span data-ttu-id="8efb7-322">Mer information finns i [objekt som parametrar](resource-manager-objects-as-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-322">For more information, see [Objects as parameters](resource-manager-objects-as-parameters.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8efb7-323">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8efb7-323">Next steps</span></span>
* <span data-ttu-id="8efb7-324">tooview fullständig mallar för många olika typer av lösningar, se hello [Azure Quickstart mallar](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="8efb7-324">tooview complete templates for many different types of solutions, see hello [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates/).</span></span>
* <span data-ttu-id="8efb7-325">Mer information om hello-funktioner som du kan använda från i en mall finns [Azure Resource Manager mallen Functions](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-325">For details about hello functions you can use from within a template, see [Azure Resource Manager Template Functions](resource-group-template-functions.md).</span></span>
* <span data-ttu-id="8efb7-326">toocombine flera mallar under distributionen, se [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="8efb7-326">toocombine multiple templates during deployment, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="8efb7-327">Du kan behöva toouse resurser som finns i en annan resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="8efb7-327">You may need toouse resources that exist within a different resource group.</span></span> <span data-ttu-id="8efb7-328">Det här scenariot är vanligt när du arbetar med lagringskonton eller virtuella nätverk som delas mellan flera resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="8efb7-328">This scenario is common when working with storage accounts or virtual networks that are shared across multiple resource groups.</span></span> <span data-ttu-id="8efb7-329">Mer information finns i hello [resourceId funktionen](resource-group-template-functions-resource.md#resourceid).</span><span class="sxs-lookup"><span data-stu-id="8efb7-329">For more information, see hello [resourceId function](resource-group-template-functions-resource.md#resourceid).</span></span>
