---
title: aaaDeploy resurser med Azure CLI och mall | Microsoft Docs
description: "Använd Azure Resource Manager och Azure CLI toodeploy en tooAzure resurser. hello resurser har definierats i en Resource Manager-mall."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: tomfitz
ms.openlocfilehash: 9f8bb9a8720399390a407030d2d32bcd97d32f13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a><span data-ttu-id="93dd6-104">Distribuera resurser med Resource Manager-mallar och Azure CLI</span><span class="sxs-lookup"><span data-stu-id="93dd6-104">Deploy resources with Resource Manager templates and Azure CLI</span></span>

<span data-ttu-id="93dd6-105">Det här avsnittet beskrivs hur toouse Azure CLI 2.0 med Resource Manager mallar toodeploy tooAzure dina resurser.</span><span class="sxs-lookup"><span data-stu-id="93dd6-105">This topic explains how toouse Azure CLI 2.0 with Resource Manager templates toodeploy your resources tooAzure.</span></span> <span data-ttu-id="93dd6-106">Om du inte är bekant med hello begrepp för att distribuera och hantera dina Azure lösningar finns [översikt över Azure Resource Manager](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="93dd6-106">If you are not familiar with hello concepts of deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>  

<span data-ttu-id="93dd6-107">hello Resource Manager-mall som du distribuerar kan antingen vara en lokal fil på din dator eller en extern fil som finns i en databas som GitHub.</span><span class="sxs-lookup"><span data-stu-id="93dd6-107">hello Resource Manager template you deploy can either be a local file on your machine, or an external file that is located in a repository like GitHub.</span></span> <span data-ttu-id="93dd6-108">hello-mallen som du distribuerar i den här artikeln är tillgänglig i hello [exempelmall](#sample-template) avsnitt, eller som en [lagring mall i GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="93dd6-108">hello template you deploy in this article is available in hello [Sample template](#sample-template) section, or as a [storage account template in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span></span>

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

<span data-ttu-id="93dd6-109">Om du inte har Azure CLI är installerad kan du använda hello [moln Shell](#deploy-template-from-cloud-shell).</span><span class="sxs-lookup"><span data-stu-id="93dd6-109">If you do not have Azure CLI installed, you can use hello [Cloud Shell](#deploy-template-from-cloud-shell).</span></span>

## <a name="deploy-local-template"></a><span data-ttu-id="93dd6-110">Distribuera lokala mall</span><span class="sxs-lookup"><span data-stu-id="93dd6-110">Deploy local template</span></span>

<span data-ttu-id="93dd6-111">När du distribuerar resurser tooAzure du:</span><span class="sxs-lookup"><span data-stu-id="93dd6-111">When deploying resources tooAzure, you:</span></span>

1. <span data-ttu-id="93dd6-112">Logga in tooyour Azure-konto</span><span class="sxs-lookup"><span data-stu-id="93dd6-112">Log in tooyour Azure account</span></span>
2. <span data-ttu-id="93dd6-113">Skapa en resursgrupp som fungerar som behållare för hello för hello distribuerade resurser.</span><span class="sxs-lookup"><span data-stu-id="93dd6-113">Create a resource group that serves as hello container for hello deployed resources.</span></span> <span data-ttu-id="93dd6-114">hello namnet på hello resursgruppen får bara innehålla alfanumeriska tecken, punkter, understreck, bindestreck och parenteser.</span><span class="sxs-lookup"><span data-stu-id="93dd6-114">hello name of hello resource group can only include alphanumeric characters, periods, underscores, hyphens, and parenthesis.</span></span> <span data-ttu-id="93dd6-115">Det kan vara upp too90 tecken.</span><span class="sxs-lookup"><span data-stu-id="93dd6-115">It can be up too90 characters.</span></span> <span data-ttu-id="93dd6-116">Den kan inte sluta med en punkt.</span><span class="sxs-lookup"><span data-stu-id="93dd6-116">It cannot end in a period.</span></span>
3. <span data-ttu-id="93dd6-117">Distribuera toohello resurs grupp hello-mallen som definierar hello resurser toocreate</span><span class="sxs-lookup"><span data-stu-id="93dd6-117">Deploy toohello resource group hello template that defines hello resources toocreate</span></span>

<span data-ttu-id="93dd6-118">En mall kan innehålla parametrar som möjliggör toocustomize hello distribution.</span><span class="sxs-lookup"><span data-stu-id="93dd6-118">A template can include parameters that enable you toocustomize hello deployment.</span></span> <span data-ttu-id="93dd6-119">Exempelvis kan du ange värden som är anpassade för en viss miljö (t.ex dev, test- och).</span><span class="sxs-lookup"><span data-stu-id="93dd6-119">For example, you can provide values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="93dd6-120">hello exempelmall definierar en parameter för hello lagringskontot SKU.</span><span class="sxs-lookup"><span data-stu-id="93dd6-120">hello sample template defines a parameter for hello storage account SKU.</span></span> 

<span data-ttu-id="93dd6-121">hello följande exempel skapar en resursgrupp och distribuerar en mall från den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="93dd6-121">hello following example creates a resource group, and deploys a template from your local machine:</span></span>

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

<span data-ttu-id="93dd6-122">hello distributionen kan ta några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="93dd6-122">hello deployment can take a few minutes toocomplete.</span></span> <span data-ttu-id="93dd6-123">När den är klar visas ett meddelande som innehåller hello resultat:</span><span class="sxs-lookup"><span data-stu-id="93dd6-123">When it finishes, you see a message that includes hello result:</span></span>

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a><span data-ttu-id="93dd6-124">Distribuera externa mall</span><span class="sxs-lookup"><span data-stu-id="93dd6-124">Deploy external template</span></span>

<span data-ttu-id="93dd6-125">Istället för att lagra Resource Manager-mallar på din lokala dator kanske du hellre toostore dem i en extern plats.</span><span class="sxs-lookup"><span data-stu-id="93dd6-125">Instead of storing Resource Manager templates on your local machine, you may prefer toostore them in an external location.</span></span> <span data-ttu-id="93dd6-126">Du kan lagra mallar i en källkontroll (till exempel GitHub).</span><span class="sxs-lookup"><span data-stu-id="93dd6-126">You can store templates in a source control repository (such as GitHub).</span></span> <span data-ttu-id="93dd6-127">Eller, du kan lagra dem i ett Azure storage-konto för delad åtkomst i din organisation.</span><span class="sxs-lookup"><span data-stu-id="93dd6-127">Or, you can store them in an Azure storage account for shared access in your organization.</span></span>

<span data-ttu-id="93dd6-128">toodeploy en extern mallen använder hello **mall-uri** parameter.</span><span class="sxs-lookup"><span data-stu-id="93dd6-128">toodeploy an external template, use hello **template-uri** parameter.</span></span> <span data-ttu-id="93dd6-129">Använd hello URI i hello exempel toodeploy hello exempel mallen från GitHub.</span><span class="sxs-lookup"><span data-stu-id="93dd6-129">Use hello URI in hello example toodeploy hello sample template from GitHub.</span></span>
   
```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
    --parameters storageAccountType=Standard_GRS
```

<span data-ttu-id="93dd6-130">hello kräver exemplet ovan en offentligt tillgänglig URI för hello mallen, som fungerar i de flesta scenarier eftersom mallen inte innehålla känsliga data.</span><span class="sxs-lookup"><span data-stu-id="93dd6-130">hello preceding example requires a publicly accessible URI for hello template, which works for most scenarios because your template should not include sensitive data.</span></span> <span data-ttu-id="93dd6-131">Om du behöver toospecify känsliga data (till exempel en adminlösenord) kan skicka värdet som en säker parameter.</span><span class="sxs-lookup"><span data-stu-id="93dd6-131">If you need toospecify sensitive data (like an admin password), pass that value as a secure parameter.</span></span> <span data-ttu-id="93dd6-132">Om du inte vill att din mall toobe offentligt tillgänglig, kan du dock skydda det genom att lagra det i en behållare för privat lagring.</span><span class="sxs-lookup"><span data-stu-id="93dd6-132">However, if you do not want your template toobe publicly accessible, you can protect it by storing it in a private storage container.</span></span> <span data-ttu-id="93dd6-133">Information om hur du distribuerar en mall som kräver en signatur (SAS) token för delad åtkomst finns i [distribuera privata mallar med SAS-token](resource-manager-cli-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="93dd6-133">For information about deploying a template that requires a shared access signature (SAS) token, see [Deploy private template with SAS token](resource-manager-cli-sas-token.md).</span></span>

## <a name="deploy-template-from-cloud-shell"></a><span data-ttu-id="93dd6-134">Distribuera mallen från Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="93dd6-134">Deploy template from Cloud Shell</span></span>

<span data-ttu-id="93dd6-135">Du kan använda [moln Shell](../cloud-shell/overview.md) toorun hello Azure CLI-kommandon för att distribuera mallen.</span><span class="sxs-lookup"><span data-stu-id="93dd6-135">You can use [Cloud Shell](../cloud-shell/overview.md) toorun hello Azure CLI commands for deploying your template.</span></span> <span data-ttu-id="93dd6-136">Men du måste först läsa in mallen i hello filresurs för moln-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="93dd6-136">However, you must first load your template into hello file share for your Cloud Shell.</span></span> <span data-ttu-id="93dd6-137">Om du inte har använt Cloud Shell tidigare läser du [Overview of Azure Cloud Shell](../cloud-shell/overview.md) (Översikt över Azure Cloud Shell), som innehåller information om hur du konfigurerar Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="93dd6-137">If you have not used Cloud Shell, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md) for information about setting it up.</span></span>

1. <span data-ttu-id="93dd6-138">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="93dd6-138">Log in toohello [Azure portal](https://portal.azure.com).</span></span>   

2. <span data-ttu-id="93dd6-139">Välj din Cloud Shell-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="93dd6-139">Select your Cloud Shell resource group.</span></span> <span data-ttu-id="93dd6-140">hello namnmönstret är `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="93dd6-140">hello name pattern is `cloud-shell-storage-<region>`.</span></span>

   ![Välj resursgrupp](./media/resource-group-template-deploy-cli/select-cs-resource-group.png)

3. <span data-ttu-id="93dd6-142">Välj hello storage-konto för moln-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="93dd6-142">Select hello storage account for your Cloud Shell.</span></span>

   ![Välj lagringskonto](./media/resource-group-template-deploy-cli/select-storage.png)

4. <span data-ttu-id="93dd6-144">Välj **Filer**.</span><span class="sxs-lookup"><span data-stu-id="93dd6-144">Select **Files**.</span></span>

   ![Välj filer](./media/resource-group-template-deploy-cli/select-files.png)

5. <span data-ttu-id="93dd6-146">Välj hello filresurs för molnet Shell.</span><span class="sxs-lookup"><span data-stu-id="93dd6-146">Select hello file share for Cloud Shell.</span></span> <span data-ttu-id="93dd6-147">hello namnmönstret är `cs-<user>-<domain>-com-<uniqueGuid>`.</span><span class="sxs-lookup"><span data-stu-id="93dd6-147">hello name pattern is `cs-<user>-<domain>-com-<uniqueGuid>`.</span></span>

   ![Välj filresurs](./media/resource-group-template-deploy-cli/select-file-share.png)

6. <span data-ttu-id="93dd6-149">Välj **Lägg till katalog**.</span><span class="sxs-lookup"><span data-stu-id="93dd6-149">Select **Add directory**.</span></span>

   ![Lägg till katalog](./media/resource-group-template-deploy-cli/select-add-directory.png)

7. <span data-ttu-id="93dd6-151">Ge den namnet **templates** och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="93dd6-151">Name it **templates**, and select **Okay**.</span></span>

   ![Namnge katalogen](./media/resource-group-template-deploy-cli/name-templates.png)

8. <span data-ttu-id="93dd6-153">Välj den nya katalogen.</span><span class="sxs-lookup"><span data-stu-id="93dd6-153">Select your new directory.</span></span>

   ![Välj katalog](./media/resource-group-template-deploy-cli/select-templates.png)

9. <span data-ttu-id="93dd6-155">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="93dd6-155">Select **Upload**.</span></span>

   ![Välj Överför](./media/resource-group-template-deploy-cli/select-upload.png)

10. <span data-ttu-id="93dd6-157">Leta upp och överför mallen.</span><span class="sxs-lookup"><span data-stu-id="93dd6-157">Find and upload your template.</span></span>

   ![Ladda upp filen](./media/resource-group-template-deploy-cli/upload-files.png)

11. <span data-ttu-id="93dd6-159">Öppna hello-prompt.</span><span class="sxs-lookup"><span data-stu-id="93dd6-159">Open hello prompt.</span></span>

   ![Öppna Cloud Shell](./media/resource-group-template-deploy-cli/start-cloud-shell.png)

12. <span data-ttu-id="93dd6-161">Ange följande kommandon i hello molnet Shell hello:</span><span class="sxs-lookup"><span data-stu-id="93dd6-161">Enter hello following commands in hello Cloud Shell:</span></span>

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageAccountType=Standard_GRS
   ```

## <a name="parameter-files"></a><span data-ttu-id="93dd6-162">Parametern-filer</span><span class="sxs-lookup"><span data-stu-id="93dd6-162">Parameter files</span></span>

<span data-ttu-id="93dd6-163">I stället för att skicka parametrar som infogade värden i skriptet kan du enklare toouse en JSON-fil som innehåller hello parametervärden.</span><span class="sxs-lookup"><span data-stu-id="93dd6-163">Rather than passing parameters as inline values in your script, you may find it easier toouse a JSON file that contains hello parameter values.</span></span> <span data-ttu-id="93dd6-164">hello parameterfil måste vara i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="93dd6-164">hello parameter file must be in hello following format:</span></span>

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

<span data-ttu-id="93dd6-165">Observera att hello parametrar avsnittet innehåller ett parameternamn som matchar hello-parameter som definierats i mallen (storageAccountType).</span><span class="sxs-lookup"><span data-stu-id="93dd6-165">Notice that hello parameters section includes a parameter name that matches hello parameter defined in your template (storageAccountType).</span></span> <span data-ttu-id="93dd6-166">hello parameterfilen innehåller ett värde för parametern hello.</span><span class="sxs-lookup"><span data-stu-id="93dd6-166">hello parameter file contains a value for hello parameter.</span></span> <span data-ttu-id="93dd6-167">Det här värdet skickas automatiskt toohello mallen under distributionen.</span><span class="sxs-lookup"><span data-stu-id="93dd6-167">This value is automatically passed toohello template during deployment.</span></span> <span data-ttu-id="93dd6-168">Du kan skapa flera parametern filer för olika distributionsscenarier och sedan ange hello lämpliga parameterfil.</span><span class="sxs-lookup"><span data-stu-id="93dd6-168">You can create multiple parameter files for different deployment scenarios, and then pass in hello appropriate parameter file.</span></span> 

<span data-ttu-id="93dd6-169">Kopiera hello föregående exempel och spara den som en fil med namnet `storage.parameters.json`.</span><span class="sxs-lookup"><span data-stu-id="93dd6-169">Copy hello preceding example and save it as a file named `storage.parameters.json`.</span></span>

<span data-ttu-id="93dd6-170">toopass en lokal parameterfil använda `@` toospecify en lokal fil med namnet storage.parameters.json.</span><span class="sxs-lookup"><span data-stu-id="93dd6-170">toopass a local parameter file, use `@` toospecify a local file named storage.parameters.json.</span></span>

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a><span data-ttu-id="93dd6-171">Testa en för malldistribution</span><span class="sxs-lookup"><span data-stu-id="93dd6-171">Test a template deployment</span></span>

<span data-ttu-id="93dd6-172">tootest din mall och parametern värden utan att faktiskt distribuera några resurser använder [az distribution Validera](/cli/azure/group/deployment#validate).</span><span class="sxs-lookup"><span data-stu-id="93dd6-172">tootest your template and parameter values without actually deploying any resources, use [az group deployment validate](/cli/azure/group/deployment#validate).</span></span> 

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

<span data-ttu-id="93dd6-173">Om inga fel identifieras returnerar hello kommando information om hello Testa distributionen.</span><span class="sxs-lookup"><span data-stu-id="93dd6-173">If no errors are detected, hello command returns information about hello test deployment.</span></span> <span data-ttu-id="93dd6-174">Observera att hello särskilt **fel** värdet är null.</span><span class="sxs-lookup"><span data-stu-id="93dd6-174">In particular, notice that hello **error** value is null.</span></span>

```azurecli
{
  "error": null,
  "properties": {
      ...
```

<span data-ttu-id="93dd6-175">Om ett fel upptäcks returnerar hello kommando ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="93dd6-175">If an error is detected, hello command returns an error message.</span></span> <span data-ttu-id="93dd6-176">Till exempel returnerar försöker toopass ett felaktigt värde för hello lagringskontot SKU, hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="93dd6-176">For example, attempting toopass an incorrect value for hello storage account SKU, returns hello following error:</span></span>

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'hello provided value 'badSKU' for hello template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. hello parameter value is not part of hello allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

<span data-ttu-id="93dd6-177">Om mallen har ett syntaxfel, returnerar hello kommando ett felmeddelande om att det inte kunde tolka hello mallen.</span><span class="sxs-lookup"><span data-stu-id="93dd6-177">If your template has a syntax error, hello command returns an error indicating it could not parse hello template.</span></span> <span data-ttu-id="93dd6-178">hello-meddelande visar hello radnummer och positionen för hello Tolkningsfel.</span><span class="sxs-lookup"><span data-stu-id="93dd6-178">hello message indicates hello line number and position of hello parsing error.</span></span>

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

<span data-ttu-id="93dd6-179">toouse fullständig läge, Använd hello `mode` parameter:</span><span class="sxs-lookup"><span data-stu-id="93dd6-179">toouse complete mode, use hello `mode` parameter:</span></span>

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

## <a name="sample-template"></a><span data-ttu-id="93dd6-180">Exempelmall</span><span class="sxs-lookup"><span data-stu-id="93dd6-180">Sample template</span></span>

<span data-ttu-id="93dd6-181">hello används följande mall för hello exemplen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="93dd6-181">hello following template is used for hello examples in this topic.</span></span> <span data-ttu-id="93dd6-182">Kopiera och spara den som en fil med namnet storage.json.</span><span class="sxs-lookup"><span data-stu-id="93dd6-182">Copy and save it as a file named storage.json.</span></span> <span data-ttu-id="93dd6-183">toounderstand hur toocreate den här mallen finns [skapa din första Azure Resource Manager-mallen](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="93dd6-183">toounderstand how toocreate this template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>  

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

## <a name="next-steps"></a><span data-ttu-id="93dd6-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93dd6-184">Next steps</span></span>
* <span data-ttu-id="93dd6-185">hello exemplen i den här artikeln distribuera resurser tooa resursgrupp i prenumerationen standard.</span><span class="sxs-lookup"><span data-stu-id="93dd6-185">hello examples in this article deploy resources tooa resource group in your default subscription.</span></span> <span data-ttu-id="93dd6-186">toouse en annan prenumeration finns [hantera Azure-prenumerationer](/cli/azure/manage-azure-subscriptions-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="93dd6-186">toouse a different subscription, see [Manage multiple Azure subscriptions](/cli/azure/manage-azure-subscriptions-azure-cli).</span></span>
* <span data-ttu-id="93dd6-187">En fullständig exempelskript som distribuerar en mall finns i [distributionsskriptet för Resource Manager-mallen](resource-manager-samples-cli-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="93dd6-187">For a complete sample script that deploys a template, see [Resource Manager template deployment script](resource-manager-samples-cli-deploy.md).</span></span>
* <span data-ttu-id="93dd6-188">hur toodefine parametrarna i mallen, se toounderstand [förstå hello struktur och syntaxen för Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="93dd6-188">toounderstand how toodefine parameters in your template, see [Understand hello structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="93dd6-189">Tips om hur du löser vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="93dd6-189">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="93dd6-190">Information om hur du distribuerar en mall som kräver en SAS-token finns [distribuera privata mallar med SAS-token](resource-manager-cli-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="93dd6-190">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-cli-sas-token.md).</span></span>
* <span data-ttu-id="93dd6-191">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="93dd6-191">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
