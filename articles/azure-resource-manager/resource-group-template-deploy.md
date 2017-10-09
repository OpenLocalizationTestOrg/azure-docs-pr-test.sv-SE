---
title: aaaDeploy resurser med PowerShell och mall | Microsoft Docs
description: "Använd Azure Resource Manager och Azure PowerShell toodeploy en tooAzure resurser. hello resurser har definierats i en Resource Manager-mall."
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
ms.openlocfilehash: 41506811ba3c2ea5df6313db70978ade50f71161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a><span data-ttu-id="76e2d-104">Distribuera resurser med Resource Manager-mallar och Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="76e2d-104">Deploy resources with Resource Manager templates and Azure PowerShell</span></span>

<span data-ttu-id="76e2d-105">Det här avsnittet beskrivs hur toouse Azure PowerShell med Resource Manager mallar toodeploy tooAzure dina resurser.</span><span class="sxs-lookup"><span data-stu-id="76e2d-105">This topic explains how toouse Azure PowerShell with Resource Manager templates toodeploy your resources tooAzure.</span></span> <span data-ttu-id="76e2d-106">Om du inte är bekant med hello begrepp för att distribuera och hantera dina Azure lösningar finns [översikt över Azure Resource Manager](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="76e2d-106">If you are not familiar with hello concepts of deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

<span data-ttu-id="76e2d-107">hello Resource Manager-mall som du distribuerar kan antingen vara en lokal fil på din dator eller en extern fil som finns i en databas som GitHub.</span><span class="sxs-lookup"><span data-stu-id="76e2d-107">hello Resource Manager template you deploy can either be a local file on your machine, or an external file that is located in a repository like GitHub.</span></span> <span data-ttu-id="76e2d-108">hello-mallen som du distribuerar i den här artikeln är tillgänglig i hello [exempelmall](#sample-template) avsnitt, eller som [lagring mall i GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="76e2d-108">hello template you deploy in this article is available in hello [Sample template](#sample-template) section, or as [storage account template in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span></span>

[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a><span data-ttu-id="76e2d-109">Distribuera en mall från den lokala datorn</span><span class="sxs-lookup"><span data-stu-id="76e2d-109">Deploy a template from your local machine</span></span>

<span data-ttu-id="76e2d-110">När du distribuerar resurser tooAzure du:</span><span class="sxs-lookup"><span data-stu-id="76e2d-110">When deploying resources tooAzure, you:</span></span>

1. <span data-ttu-id="76e2d-111">Logga in tooyour Azure-konto</span><span class="sxs-lookup"><span data-stu-id="76e2d-111">Log in tooyour Azure account</span></span>
2. <span data-ttu-id="76e2d-112">Skapa en resursgrupp som fungerar som behållare för hello för hello distribuerade resurser.</span><span class="sxs-lookup"><span data-stu-id="76e2d-112">Create a resource group that serves as hello container for hello deployed resources.</span></span> <span data-ttu-id="76e2d-113">hello namnet på hello resursgruppen får bara innehålla alfanumeriska tecken, punkter, understreck, bindestreck och parenteser.</span><span class="sxs-lookup"><span data-stu-id="76e2d-113">hello name of hello resource group can only include alphanumeric characters, periods, underscores, hyphens, and parenthesis.</span></span> <span data-ttu-id="76e2d-114">Det kan vara upp too90 tecken.</span><span class="sxs-lookup"><span data-stu-id="76e2d-114">It can be up too90 characters.</span></span> <span data-ttu-id="76e2d-115">Den kan inte sluta med en punkt.</span><span class="sxs-lookup"><span data-stu-id="76e2d-115">It cannot end in a period.</span></span>
3. <span data-ttu-id="76e2d-116">Distribuera toohello resurs grupp hello-mallen som definierar hello resurser toocreate</span><span class="sxs-lookup"><span data-stu-id="76e2d-116">Deploy toohello resource group hello template that defines hello resources toocreate</span></span>

<span data-ttu-id="76e2d-117">En mall kan innehålla parametrar som möjliggör toocustomize hello distribution.</span><span class="sxs-lookup"><span data-stu-id="76e2d-117">A template can include parameters that enable you toocustomize hello deployment.</span></span> <span data-ttu-id="76e2d-118">Exempelvis kan du ange värden som är anpassade för en viss miljö (t.ex dev, test- och).</span><span class="sxs-lookup"><span data-stu-id="76e2d-118">For example, you can provide values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="76e2d-119">hello exempelmall definierar en parameter för hello lagringskontot SKU.</span><span class="sxs-lookup"><span data-stu-id="76e2d-119">hello sample template defines a parameter for hello storage account SKU.</span></span>

<span data-ttu-id="76e2d-120">hello följande exempel skapar en resursgrupp och distribuerar en mall från den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="76e2d-120">hello following example creates a resource group, and deploys a template from your local machine:</span></span>

```powershell
Login-AzureRmAccount
 
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

<span data-ttu-id="76e2d-121">hello distributionen kan ta några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="76e2d-121">hello deployment can take a few minutes toocomplete.</span></span> <span data-ttu-id="76e2d-122">När den är klar visas ett meddelande som innehåller hello resultat:</span><span class="sxs-lookup"><span data-stu-id="76e2d-122">When it finishes, you see a message that includes hello result:</span></span>

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a><span data-ttu-id="76e2d-123">Distribuera en mall från en extern källa</span><span class="sxs-lookup"><span data-stu-id="76e2d-123">Deploy a template from an external source</span></span>

<span data-ttu-id="76e2d-124">Istället för att lagra Resource Manager-mallar på din lokala dator kanske du hellre toostore dem i en extern plats.</span><span class="sxs-lookup"><span data-stu-id="76e2d-124">Instead of storing Resource Manager templates on your local machine, you may prefer toostore them in an external location.</span></span> <span data-ttu-id="76e2d-125">Du kan lagra mallar i en källkontroll (till exempel GitHub).</span><span class="sxs-lookup"><span data-stu-id="76e2d-125">You can store templates in a source control repository (such as GitHub).</span></span> <span data-ttu-id="76e2d-126">Eller, du kan lagra dem i ett Azure storage-konto för delad åtkomst i din organisation.</span><span class="sxs-lookup"><span data-stu-id="76e2d-126">Or, you can store them in an Azure storage account for shared access in your organization.</span></span>

<span data-ttu-id="76e2d-127">toodeploy en extern mallen använder hello **TemplateUri** parameter.</span><span class="sxs-lookup"><span data-stu-id="76e2d-127">toodeploy an external template, use hello **TemplateUri** parameter.</span></span> <span data-ttu-id="76e2d-128">Använd hello URI i hello exempel toodeploy hello exempel mallen från GitHub.</span><span class="sxs-lookup"><span data-stu-id="76e2d-128">Use hello URI in hello example toodeploy hello sample template from GitHub.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

<span data-ttu-id="76e2d-129">hello kräver exemplet ovan en offentligt tillgänglig URI för hello mallen, som fungerar i de flesta scenarier eftersom mallen inte innehålla känsliga data.</span><span class="sxs-lookup"><span data-stu-id="76e2d-129">hello preceding example requires a publicly accessible URI for hello template, which works for most scenarios because your template should not include sensitive data.</span></span> <span data-ttu-id="76e2d-130">Om du behöver toospecify känsliga data (till exempel en adminlösenord) kan skicka värdet som en säker parameter.</span><span class="sxs-lookup"><span data-stu-id="76e2d-130">If you need toospecify sensitive data (like an admin password), pass that value as a secure parameter.</span></span> <span data-ttu-id="76e2d-131">Om du inte vill att din mall toobe offentligt tillgänglig, kan du dock skydda det genom att lagra det i en behållare för privat lagring.</span><span class="sxs-lookup"><span data-stu-id="76e2d-131">However, if you do not want your template toobe publicly accessible, you can protect it by storing it in a private storage container.</span></span> <span data-ttu-id="76e2d-132">Information om hur du distribuerar en mall som kräver en signatur (SAS) token för delad åtkomst finns i [distribuera privata mallar med SAS-token](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="76e2d-132">For information about deploying a template that requires a shared access signature (SAS) token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>

## <a name="parameter-files"></a><span data-ttu-id="76e2d-133">Parametern-filer</span><span class="sxs-lookup"><span data-stu-id="76e2d-133">Parameter files</span></span>

<span data-ttu-id="76e2d-134">I stället för att skicka parametrar som infogade värden i skriptet kan du enklare toouse en JSON-fil som innehåller hello parametervärden.</span><span class="sxs-lookup"><span data-stu-id="76e2d-134">Rather than passing parameters as inline values in your script, you may find it easier toouse a JSON file that contains hello parameter values.</span></span> <span data-ttu-id="76e2d-135">hello parameterfil måste vara i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="76e2d-135">hello parameter file must be in hello following format:</span></span>

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

<span data-ttu-id="76e2d-136">Observera att hello parametrar avsnittet innehåller ett parameternamn som matchar hello-parameter som definierats i mallen (storageAccountType).</span><span class="sxs-lookup"><span data-stu-id="76e2d-136">Notice that hello parameters section includes a parameter name that matches hello parameter defined in your template (storageAccountType).</span></span> <span data-ttu-id="76e2d-137">hello parameterfilen innehåller ett värde för parametern hello.</span><span class="sxs-lookup"><span data-stu-id="76e2d-137">hello parameter file contains a value for hello parameter.</span></span> <span data-ttu-id="76e2d-138">Det här värdet skickas automatiskt toohello mallen under distributionen.</span><span class="sxs-lookup"><span data-stu-id="76e2d-138">This value is automatically passed toohello template during deployment.</span></span> <span data-ttu-id="76e2d-139">Du kan skapa flera parametern filer för olika distributionsscenarier och sedan ange hello lämpliga parameterfil.</span><span class="sxs-lookup"><span data-stu-id="76e2d-139">You can create multiple parameter files for different deployment scenarios, and then pass in hello appropriate parameter file.</span></span> 

<span data-ttu-id="76e2d-140">Kopiera hello föregående exempel och spara den som en fil med namnet `storage.parameters.json`.</span><span class="sxs-lookup"><span data-stu-id="76e2d-140">Copy hello preceding example and save it as a file named `storage.parameters.json`.</span></span>

<span data-ttu-id="76e2d-141">toopass en lokal parameterfil använda hello **TemplateParameterFile** parameter:</span><span class="sxs-lookup"><span data-stu-id="76e2d-141">toopass a local parameter file, use hello **TemplateParameterFile** parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

<span data-ttu-id="76e2d-142">toopass externa parameterfilen använda hello **TemplateParameterUri** parameter:</span><span class="sxs-lookup"><span data-stu-id="76e2d-142">toopass an external parameter file, use hello **TemplateParameterUri** parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

<span data-ttu-id="76e2d-143">Du kan använda infogade parametrar och en lokal parameter filen i hello samma distributionsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="76e2d-143">You can use inline parameters and a local parameter file in hello same deployment operation.</span></span> <span data-ttu-id="76e2d-144">Du kan till exempel ange vissa värden i hello lokala parameterfilen och lägga till andra värden infogade under distributionen.</span><span class="sxs-lookup"><span data-stu-id="76e2d-144">For example, you can specify some values in hello local parameter file and add other values inline during deployment.</span></span> <span data-ttu-id="76e2d-145">Om du anger värden för en parameter i både hello lokala parameterfilen och infogade företräde hello infogade värdet.</span><span class="sxs-lookup"><span data-stu-id="76e2d-145">If you provide values for a parameter in both hello local parameter file and inline, hello inline value takes precedence.</span></span>

<span data-ttu-id="76e2d-146">Men när du använder en extern parameterfil, du kan inte skicka andra värden antingen infogade eller från en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="76e2d-146">However, when you use an external parameter file, you cannot pass other values either inline or from a local file.</span></span> <span data-ttu-id="76e2d-147">När du anger en parameterfil i hello **TemplateParameterUri** parametern, alla infogade parametrar kommer att ignoreras.</span><span class="sxs-lookup"><span data-stu-id="76e2d-147">When you specify a parameter file in hello **TemplateParameterUri** parameter, all inline parameters are ignored.</span></span> <span data-ttu-id="76e2d-148">Ange alla värden i hello extern fil.</span><span class="sxs-lookup"><span data-stu-id="76e2d-148">Provide all parameter values in hello external file.</span></span> <span data-ttu-id="76e2d-149">Om mallen innehåller något känsligt värde som du inte kan ingå i hello parameterfil, lägger du till att värdet tooa nyckelvalv eller dynamiskt tillhandahåller alla parametern värden infogad.</span><span class="sxs-lookup"><span data-stu-id="76e2d-149">If your template includes a sensitive value that you cannot include in hello parameter file, either add that value tooa key vault, or dynamically provide all parameter values inline.</span></span>

<span data-ttu-id="76e2d-150">Om mallen innehåller en parameter med samma namn som en hello parametrar i hello PowerShell-kommando hello, PowerShell visar hello parametern från din mall med hello username@Domain **från mall**.</span><span class="sxs-lookup"><span data-stu-id="76e2d-150">If your template includes a parameter with hello same name as one of hello parameters in hello PowerShell command, PowerShell presents hello parameter from your template with hello postfix **FromTemplate**.</span></span> <span data-ttu-id="76e2d-151">Till exempel en parameter med namnet **ResourceGroupName** i din mall står i konflikt med hello **ResourceGroupName** parameter i hello [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment)cmdlet.</span><span class="sxs-lookup"><span data-stu-id="76e2d-151">For example, a parameter named **ResourceGroupName** in your template conflicts with hello **ResourceGroupName** parameter in hello [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet.</span></span> <span data-ttu-id="76e2d-152">Du är tooprovide ange ett värde för **ResourceGroupNameFromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="76e2d-152">You are prompted tooprovide a value for **ResourceGroupNameFromTemplate**.</span></span> <span data-ttu-id="76e2d-153">I allmänhet bör du undvika den här förvirring genom att inte namnges parametrar med samma namn som parametrar som används för distributionsåtgärder hello.</span><span class="sxs-lookup"><span data-stu-id="76e2d-153">In general, you should avoid this confusion by not naming parameters with hello same name as parameters used for deployment operations.</span></span>

## <a name="test-a-template-deployment"></a><span data-ttu-id="76e2d-154">Testa en för malldistribution</span><span class="sxs-lookup"><span data-stu-id="76e2d-154">Test a template deployment</span></span>

<span data-ttu-id="76e2d-155">tootest din mall och parametern värden utan att faktiskt distribuera några resurser använder [Test AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment).</span><span class="sxs-lookup"><span data-stu-id="76e2d-155">tootest your template and parameter values without actually deploying any resources, use [Test-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment).</span></span> 

```powershell
Test-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

<span data-ttu-id="76e2d-156">Om inga fel identifieras hello-kommandot har slutförts utan ett svar.</span><span class="sxs-lookup"><span data-stu-id="76e2d-156">If no errors are detected, hello command finishes without a response.</span></span> <span data-ttu-id="76e2d-157">Om ett fel upptäcks returnerar hello kommando ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="76e2d-157">If an error is detected, hello command returns an error message.</span></span> <span data-ttu-id="76e2d-158">Till exempel returnerar försöker toopass ett felaktigt värde för hello lagringskontot SKU, hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="76e2d-158">For example, attempting toopass an incorrect value for hello storage account SKU, returns hello following error:</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'hello provided value 'badSku' for hello template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. hello parameter value is not part of hello allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

<span data-ttu-id="76e2d-159">Om mallen har ett syntaxfel, returnerar hello kommando ett felmeddelande om att det inte kunde tolka hello mallen.</span><span class="sxs-lookup"><span data-stu-id="76e2d-159">If your template has a syntax error, hello command returns an error indicating it could not parse hello template.</span></span> <span data-ttu-id="76e2d-160">hello-meddelande visar hello radnummer och positionen för hello Tolkningsfel.</span><span class="sxs-lookup"><span data-stu-id="76e2d-160">hello message indicates hello line number and position of hello parsing error.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

<span data-ttu-id="76e2d-161">toouse fullständig läge, Använd hello `Mode` parameter:</span><span class="sxs-lookup"><span data-stu-id="76e2d-161">toouse complete mode, use hello `Mode` parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Mode Complete -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\storage.json 
```

## <a name="sample-template"></a><span data-ttu-id="76e2d-162">Exempelmall</span><span class="sxs-lookup"><span data-stu-id="76e2d-162">Sample template</span></span>

<span data-ttu-id="76e2d-163">hello används följande mall för hello exemplen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="76e2d-163">hello following template is used for hello examples in this topic.</span></span> <span data-ttu-id="76e2d-164">Kopiera och spara den som en fil med namnet storage.json.</span><span class="sxs-lookup"><span data-stu-id="76e2d-164">Copy and save it as a file named storage.json.</span></span> <span data-ttu-id="76e2d-165">toounderstand hur toocreate den här mallen finns [skapa din första Azure Resource Manager-mallen](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="76e2d-165">toounderstand how toocreate this template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>  

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

## <a name="next-steps"></a><span data-ttu-id="76e2d-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="76e2d-166">Next steps</span></span>
* <span data-ttu-id="76e2d-167">hello exemplen i den här artikeln distribuera resurser tooa resursgrupp i prenumerationen standard.</span><span class="sxs-lookup"><span data-stu-id="76e2d-167">hello examples in this article deploy resources tooa resource group in your default subscription.</span></span> <span data-ttu-id="76e2d-168">toouse en annan prenumeration finns [hantera Azure-prenumerationer](/powershell/azure/manage-subscriptions-azureps).</span><span class="sxs-lookup"><span data-stu-id="76e2d-168">toouse a different subscription, see [Manage multiple Azure subscriptions](/powershell/azure/manage-subscriptions-azureps).</span></span>
* <span data-ttu-id="76e2d-169">En fullständig exempelskript som distribuerar en mall finns i [distributionsskriptet för Resource Manager-mallen](resource-manager-samples-powershell-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="76e2d-169">For a complete sample script that deploys a template, see [Resource Manager template deployment script](resource-manager-samples-powershell-deploy.md).</span></span>
* <span data-ttu-id="76e2d-170">hur toodefine parametrarna i mallen, se toounderstand [förstå hello struktur och syntaxen för Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="76e2d-170">toounderstand how toodefine parameters in your template, see [Understand hello structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="76e2d-171">Tips om hur du löser vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="76e2d-171">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="76e2d-172">Information om hur du distribuerar en mall som kräver en SAS-token finns [distribuera privata mallar med SAS-token](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="76e2d-172">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
* <span data-ttu-id="76e2d-173">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="76e2d-173">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

