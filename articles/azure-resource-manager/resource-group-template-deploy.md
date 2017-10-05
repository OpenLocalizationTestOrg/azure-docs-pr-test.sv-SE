---
title: Distribuera resurser med PowerShell och mall | Microsoft Docs
description: "Använd Azure Resource Manager och Azure PowerShell för att distribuera en resurser till Azure. Resurserna definieras i en Resource Manager-mall."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 5f395abf8ebdfbac18fd17d8183b392673e280ec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a><span data-ttu-id="1c8c7-104">Distribuera resurser med Resource Manager-mallar och Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1c8c7-104">Deploy resources with Resource Manager templates and Azure PowerShell</span></span>

<span data-ttu-id="1c8c7-105">Det här avsnittet beskriver hur du använder Azure PowerShell med Resource Manager-mallar för att distribuera resurserna till Azure.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-105">This topic explains how to use Azure PowerShell with Resource Manager templates to deploy your resources to Azure.</span></span> <span data-ttu-id="1c8c7-106">Om du inte är bekant med principerna för att distribuera och hantera dina Azure lösningar finns [översikt över Azure Resource Manager](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-106">If you are not familiar with the concepts of deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

<span data-ttu-id="1c8c7-107">Resource Manager-mallen som du distribuerar kan antingen vara en lokal fil på din dator eller en extern fil som finns i en databas som GitHub.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-107">The Resource Manager template you deploy can either be a local file on your machine, or an external file that is located in a repository like GitHub.</span></span> <span data-ttu-id="1c8c7-108">Den mall som du distribuerar i den här artikeln är tillgänglig i den [exempelmall](#sample-template) avsnitt, eller som [lagring mall i GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-108">The template you deploy in this article is available in the [Sample template](#sample-template) section, or as [storage account template in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span></span>

[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a><span data-ttu-id="1c8c7-109">Distribuera en mall från den lokala datorn</span><span class="sxs-lookup"><span data-stu-id="1c8c7-109">Deploy a template from your local machine</span></span>

<span data-ttu-id="1c8c7-110">När du distribuerar resurser till Azure måste du:</span><span class="sxs-lookup"><span data-stu-id="1c8c7-110">When deploying resources to Azure, you:</span></span>

1. <span data-ttu-id="1c8c7-111">Logga in på ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="1c8c7-111">Log in to your Azure account</span></span>
2. <span data-ttu-id="1c8c7-112">Skapa en resursgrupp som fungerar som behållare för distribuerade resurser.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-112">Create a resource group that serves as the container for the deployed resources.</span></span> <span data-ttu-id="1c8c7-113">Namnet på resursgruppen får bara innehålla alfanumeriska tecken, punkter, understreck, bindestreck och parenteser.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-113">The name of the resource group can only include alphanumeric characters, periods, underscores, hyphens, and parenthesis.</span></span> <span data-ttu-id="1c8c7-114">Det kan vara upp till 90 tecken.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-114">It can be up to 90 characters.</span></span> <span data-ttu-id="1c8c7-115">Den kan inte sluta med en punkt.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-115">It cannot end in a period.</span></span>
3. <span data-ttu-id="1c8c7-116">Distribuera till resursgrupp mallen som definierar resurser för att skapa</span><span class="sxs-lookup"><span data-stu-id="1c8c7-116">Deploy to the resource group the template that defines the resources to create</span></span>

<span data-ttu-id="1c8c7-117">En mall kan innehålla parametrar som gör att du kan anpassa distributionen.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-117">A template can include parameters that enable you to customize the deployment.</span></span> <span data-ttu-id="1c8c7-118">Exempelvis kan du ange värden som är anpassade för en viss miljö (t.ex dev, test- och).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-118">For example, you can provide values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="1c8c7-119">Mallen exempel definierar en parameter för lagringskontot SKU.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-119">The sample template defines a parameter for the storage account SKU.</span></span>

<span data-ttu-id="1c8c7-120">I följande exempel skapar en resursgrupp och distribuerar en mall från den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="1c8c7-120">The following example creates a resource group, and deploys a template from your local machine:</span></span>

```powershell
Login-AzureRmAccount
 
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

<span data-ttu-id="1c8c7-121">Det kan ta några minuter att slutföra distributionen.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-121">The deployment can take a few minutes to complete.</span></span> <span data-ttu-id="1c8c7-122">När den är klar visas ett meddelande som innehåller resultatet:</span><span class="sxs-lookup"><span data-stu-id="1c8c7-122">When it finishes, you see a message that includes the result:</span></span>

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a><span data-ttu-id="1c8c7-123">Distribuera en mall från en extern källa</span><span class="sxs-lookup"><span data-stu-id="1c8c7-123">Deploy a template from an external source</span></span>

<span data-ttu-id="1c8c7-124">Du kanske föredrar att lagra dem på en extern plats istället för att lagra Resource Manager-mallar på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-124">Instead of storing Resource Manager templates on your local machine, you may prefer to store them in an external location.</span></span> <span data-ttu-id="1c8c7-125">Du kan lagra mallar i en källkontroll (till exempel GitHub).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-125">You can store templates in a source control repository (such as GitHub).</span></span> <span data-ttu-id="1c8c7-126">Eller, du kan lagra dem i ett Azure storage-konto för delad åtkomst i din organisation.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-126">Or, you can store them in an Azure storage account for shared access in your organization.</span></span>

<span data-ttu-id="1c8c7-127">Distribuera en extern mall att använda den **TemplateUri** parameter.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-127">To deploy an external template, use the **TemplateUri** parameter.</span></span> <span data-ttu-id="1c8c7-128">Använd URI: N i exemplet för att distribuera exempelmall från GitHub.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-128">Use the URI in the example to deploy the sample template from GitHub.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

<span data-ttu-id="1c8c7-129">I exemplet ovan kräver en offentligt tillgänglig URI för mallen, som fungerar i de flesta scenarier eftersom mallen inte innehålla känsliga data.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-129">The preceding example requires a publicly accessible URI for the template, which works for most scenarios because your template should not include sensitive data.</span></span> <span data-ttu-id="1c8c7-130">Om du behöver ange känsliga data (till exempel en adminlösenord) skicka det värdet som en säker parameter.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-130">If you need to specify sensitive data (like an admin password), pass that value as a secure parameter.</span></span> <span data-ttu-id="1c8c7-131">Om du inte vill att mallen ska vara offentligt tillgänglig, kan du dock skydda den genom att lagra det i en behållare för privat lagring.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-131">However, if you do not want your template to be publicly accessible, you can protect it by storing it in a private storage container.</span></span> <span data-ttu-id="1c8c7-132">Information om hur du distribuerar en mall som kräver en signatur (SAS) token för delad åtkomst finns i [distribuera privata mallar med SAS-token](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-132">For information about deploying a template that requires a shared access signature (SAS) token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>

## <a name="parameter-files"></a><span data-ttu-id="1c8c7-133">Parametern-filer</span><span class="sxs-lookup"><span data-stu-id="1c8c7-133">Parameter files</span></span>

<span data-ttu-id="1c8c7-134">I stället för att skicka parametrar som infogade värden i skriptet kan det vara lättare att använda en JSON-fil som innehåller parametrarnas värden.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-134">Rather than passing parameters as inline values in your script, you may find it easier to use a JSON file that contains the parameter values.</span></span> <span data-ttu-id="1c8c7-135">Parameterfilen måste vara i följande format:</span><span class="sxs-lookup"><span data-stu-id="1c8c7-135">The parameter file must be in the following format:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

<span data-ttu-id="1c8c7-136">Observera att avsnittet Parametrar innehåller ett parameternamn som matchar den parameter som definierats i mallen (storageAccountType).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-136">Notice that the parameters section includes a parameter name that matches the parameter defined in your template (storageAccountType).</span></span> <span data-ttu-id="1c8c7-137">Parameterfilen innehåller ett värde för parametern.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-137">The parameter file contains a value for the parameter.</span></span> <span data-ttu-id="1c8c7-138">Det här värdet skickas automatiskt till mallen under distributionen.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-138">This value is automatically passed to the template during deployment.</span></span> <span data-ttu-id="1c8c7-139">Du kan skapa flera parametern filer för olika distributionsscenarier och sedan ange lämpliga parameterfil.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-139">You can create multiple parameter files for different deployment scenarios, and then pass in the appropriate parameter file.</span></span> 

<span data-ttu-id="1c8c7-140">Kopiera föregående exempel och spara den som en fil med namnet `storage.parameters.json`.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-140">Copy the preceding example and save it as a file named `storage.parameters.json`.</span></span>

<span data-ttu-id="1c8c7-141">Om du vill lägga till en lokal parameterfil, Använd den **TemplateParameterFile** parameter:</span><span class="sxs-lookup"><span data-stu-id="1c8c7-141">To pass a local parameter file, use the **TemplateParameterFile** parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

<span data-ttu-id="1c8c7-142">Om du vill lägga till en extern parameterfil, Använd den **TemplateParameterUri** parameter:</span><span class="sxs-lookup"><span data-stu-id="1c8c7-142">To pass an external parameter file, use the **TemplateParameterUri** parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

<span data-ttu-id="1c8c7-143">Du kan använda infogade parametrar och en lokal parameterfil i samma åtgärd för distribution.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-143">You can use inline parameters and a local parameter file in the same deployment operation.</span></span> <span data-ttu-id="1c8c7-144">Du kan till exempel ange vissa värden i den lokala parameterfilen och lägga till andra värden infogade under distributionen.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-144">For example, you can specify some values in the local parameter file and add other values inline during deployment.</span></span> <span data-ttu-id="1c8c7-145">Om du anger värden för en parameter i både lokala parameterfilen och infogade företräde infogade värdet.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-145">If you provide values for a parameter in both the local parameter file and inline, the inline value takes precedence.</span></span>

<span data-ttu-id="1c8c7-146">Men när du använder en extern parameterfil, du kan inte skicka andra värden antingen infogade eller från en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-146">However, when you use an external parameter file, you cannot pass other values either inline or from a local file.</span></span> <span data-ttu-id="1c8c7-147">När du anger en parameterfil i den **TemplateParameterUri** parametern, alla infogade parametrar kommer att ignoreras.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-147">When you specify a parameter file in the **TemplateParameterUri** parameter, all inline parameters are ignored.</span></span> <span data-ttu-id="1c8c7-148">Ange alla värdena i den externa filen.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-148">Provide all parameter values in the external file.</span></span> <span data-ttu-id="1c8c7-149">Om din mall innehåller något känsligt värde som du inte kan ingå i parameterfilen, lägga till värdet i ett nyckelvalv eller dynamiskt tillhandahåller alla parametern värden infogad.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-149">If your template includes a sensitive value that you cannot include in the parameter file, either add that value to a key vault, or dynamically provide all parameter values inline.</span></span>

<span data-ttu-id="1c8c7-150">Om mallen innehåller en parameter med samma namn som en av parametrarna i PowerShell-kommandot, PowerShell visar parametern från din mall med username@Domain **från mall**.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-150">If your template includes a parameter with the same name as one of the parameters in the PowerShell command, PowerShell presents the parameter from your template with the postfix **FromTemplate**.</span></span> <span data-ttu-id="1c8c7-151">Till exempel en parameter med namnet **ResourceGroupName** i din mall står i konflikt med den **ResourceGroupName** parametern i den [ny AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-151">For example, a parameter named **ResourceGroupName** in your template conflicts with the **ResourceGroupName** parameter in the [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet.</span></span> <span data-ttu-id="1c8c7-152">Du uppmanas att ange ett värde för **ResourceGroupNameFromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-152">You are prompted to provide a value for **ResourceGroupNameFromTemplate**.</span></span> <span data-ttu-id="1c8c7-153">I allmänhet bör du undvika den här förvirring genom att namnge inte parametrar med samma namn som parametrar som används för distributionsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-153">In general, you should avoid this confusion by not naming parameters with the same name as parameters used for deployment operations.</span></span>

## <a name="test-a-template-deployment"></a><span data-ttu-id="1c8c7-154">Testa en för malldistribution</span><span class="sxs-lookup"><span data-stu-id="1c8c7-154">Test a template deployment</span></span>

<span data-ttu-id="1c8c7-155">Testa din mall och parametern-värden utan att faktiskt distribuera resurser med [Test AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-155">To test your template and parameter values without actually deploying any resources, use [Test-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment).</span></span> 

```powershell
Test-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

<span data-ttu-id="1c8c7-156">Om inga fel identifieras kommandot har slutförts utan ett svar.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-156">If no errors are detected, the command finishes without a response.</span></span> <span data-ttu-id="1c8c7-157">Om ett fel upptäcks returnerar kommandot ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-157">If an error is detected, the command returns an error message.</span></span> <span data-ttu-id="1c8c7-158">Försök att skicka ett felaktigt värde för lagringskontot SKU, returnerar till exempel följande fel:</span><span class="sxs-lookup"><span data-stu-id="1c8c7-158">For example, attempting to pass an incorrect value for the storage account SKU, returns the following error:</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'The provided value 'badSku' for the template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. The parameter value is not part of the allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

<span data-ttu-id="1c8c7-159">Om mallen har ett syntaxfel, returnerar kommandot ett felmeddelande om att det inte kunde tolka mallen.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-159">If your template has a syntax error, the command returns an error indicating it could not parse the template.</span></span> <span data-ttu-id="1c8c7-160">Meddelandet Anger radnumret och positionen för parsningsfel.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-160">The message indicates the line number and position of the parsing error.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

<span data-ttu-id="1c8c7-161">Om du vill använda fullständig läge i `Mode` parameter:</span><span class="sxs-lookup"><span data-stu-id="1c8c7-161">To use complete mode, use the `Mode` parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Mode Complete -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\storage.json 
```

## <a name="sample-template"></a><span data-ttu-id="1c8c7-162">Exempelmall</span><span class="sxs-lookup"><span data-stu-id="1c8c7-162">Sample template</span></span>

<span data-ttu-id="1c8c7-163">Följande mall används i exemplen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-163">The following template is used for the examples in this topic.</span></span> <span data-ttu-id="1c8c7-164">Kopiera och spara den som en fil med namnet storage.json.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-164">Copy and save it as a file named storage.json.</span></span> <span data-ttu-id="1c8c7-165">Information om hur du skapar den här mallen finns [skapa din första Azure Resource Manager-mallen](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-165">To understand how to create this template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="1c8c7-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1c8c7-166">Next steps</span></span>
* <span data-ttu-id="1c8c7-167">Exemplen i den här artikeln distribuera resurser i en resursgrupp i din standard-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1c8c7-167">The examples in this article deploy resources to a resource group in your default subscription.</span></span> <span data-ttu-id="1c8c7-168">Om du vill använda en annan prenumeration finns [hantera Azure-prenumerationer](/powershell/azure/manage-subscriptions-azureps).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-168">To use a different subscription, see [Manage multiple Azure subscriptions](/powershell/azure/manage-subscriptions-azureps).</span></span>
* <span data-ttu-id="1c8c7-169">En fullständig exempelskript som distribuerar en mall finns i [distributionsskriptet för Resource Manager-mallen](resource-manager-samples-powershell-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-169">For a complete sample script that deploys a template, see [Resource Manager template deployment script](resource-manager-samples-powershell-deploy.md).</span></span>
* <span data-ttu-id="1c8c7-170">Information om hur du definierar parametrar i mallen finns [förstå struktur och syntaxen för Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-170">To understand how to define parameters in your template, see [Understand the structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="1c8c7-171">Tips om hur du löser vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-171">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="1c8c7-172">Information om hur du distribuerar en mall som kräver en SAS-token finns [distribuera privata mallar med SAS-token](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-172">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
* <span data-ttu-id="1c8c7-173">Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="1c8c7-173">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

