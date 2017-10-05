---
title: "Skapa din första Azure Resource Manager-mall | Microsoft Docs"
description: "Stegvisa instruktioner för hur du skapar din första Azure Resource Manager-mall. I guiden beskrivs hur du skapar mallen med hjälp av mallreferensen för ett lagringskonto."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/27/2017
ms.topic: get-started-article
ms.author: tomfitz
ms.openlocfilehash: 49086b51e2db1aebed45746306ae14b6f1feb631
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-deploy-your-first-azure-resource-manager-template"></a><span data-ttu-id="b37e9-104">Skapa och distribuera din första Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="b37e9-104">Create and deploy your first Azure Resource Manager template</span></span>
<span data-ttu-id="b37e9-105">Den här artikeln beskriver steg för steg hur du skapar din första Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="b37e9-105">This topic walks you through the steps of creating your first Azure Resource Manager template.</span></span> <span data-ttu-id="b37e9-106">Resource Manager-mallar är JSON-filer som definierar de resurser du behöver för att distribuera lösningen.</span><span class="sxs-lookup"><span data-stu-id="b37e9-106">Resource Manager templates are JSON files that define the resources you need to deploy for your solution.</span></span> <span data-ttu-id="b37e9-107">En beskrivning av de begrepp som används i samband med distribution och hantering av Azure-lösningar finns i [Översikt över Azure Resource Manager](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b37e9-107">To understand the concepts associated with deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span> <span data-ttu-id="b37e9-108">Om du har befintliga resurser och behöver en mall för dessa resurser kan du läsa [Exportera en Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="b37e9-108">If you have existing resources and want to get a template for those resources, see [Export an Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>

<span data-ttu-id="b37e9-109">Du behöver en JSON-redigerare för att skapa och ändra mallar.</span><span class="sxs-lookup"><span data-stu-id="b37e9-109">To create and revise templates, you need a JSON editor.</span></span> <span data-ttu-id="b37e9-110">[Visual Studio Code](https://code.visualstudio.com/) är en enkel, plattformsoberoende kodredigerare med öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="b37e9-110">[Visual Studio Code](https://code.visualstudio.com/) is a lightweight, open-source, cross-platform code editor.</span></span> <span data-ttu-id="b37e9-111">Vi rekommenderar starkt att du använder Visual Studio Code för att skapa Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="b37e9-111">We strongly recommend using Visual Studio Code for creating Resource Manager templates.</span></span> <span data-ttu-id="b37e9-112">Den här artikeln förutsätter att du använder Virtual Studio Code, men om du har en annan JSON-redigerare kan du använda den.</span><span class="sxs-lookup"><span data-stu-id="b37e9-112">This topic assumes you are using VS Code; however, if you have another JSON editor (like Visual Studio), you can use that editor.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b37e9-113">Krav</span><span class="sxs-lookup"><span data-stu-id="b37e9-113">Prerequisites</span></span>

* <span data-ttu-id="b37e9-114">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b37e9-114">Visual Studio Code.</span></span> <span data-ttu-id="b37e9-115">Om det behövs installerar du det från [https://code.visualstudio.com/](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="b37e9-115">If needed, install it from [https://code.visualstudio.com/](https://code.visualstudio.com/).</span></span>
* <span data-ttu-id="b37e9-116">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b37e9-116">An Azure subscription.</span></span> <span data-ttu-id="b37e9-117">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="b37e9-117">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="create-template"></a><span data-ttu-id="b37e9-118">Skapa mallen</span><span class="sxs-lookup"><span data-stu-id="b37e9-118">Create template</span></span>

<span data-ttu-id="b37e9-119">Vi ska börja med en enkel mall som distribuerar ett lagringskonto till din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b37e9-119">Let's start with a simple template that deploys a storage account to your subscription.</span></span>

1. <span data-ttu-id="b37e9-120">Välj **Arkiv** > **Ny fil**.</span><span class="sxs-lookup"><span data-stu-id="b37e9-120">Select **File** > **New File**.</span></span> 

   ![Ny fil](./media/resource-manager-create-first-template/new-file.png)

2. <span data-ttu-id="b37e9-122">Kopiera och klistra in följande JSON-syntax i filen:</span><span class="sxs-lookup"><span data-stu-id="b37e9-122">Copy and paste the following JSON syntax into your file:</span></span>

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
     },
     "variables": {
     },
     "resources": [
       {
         "name": "[concat('storage', uniqueString(resourceGroup().id))]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "sku": {
           "name": "Standard_LRS"
         },
         "kind": "Storage",
         "location": "South Central US",
         "tags": {},
         "properties": {}
       }
     ],
     "outputs": {  }
   }
   ```

   <span data-ttu-id="b37e9-123">Lagringskontonamn har flera begränsningar som gör att de är svåra att ange.</span><span class="sxs-lookup"><span data-stu-id="b37e9-123">Storage account names have several restrictions that make them difficult to set.</span></span> <span data-ttu-id="b37e9-124">Namnet måste vara mellan 3 och 24 tecken långt, endast bestå av siffror och gemener samt vara unikt.</span><span class="sxs-lookup"><span data-stu-id="b37e9-124">The name must be between 3 and 24 characters in length, use only numbers and lower-case letters, and be unique.</span></span> <span data-ttu-id="b37e9-125">I den föregående mallen används funktionen [uniqueString](resource-group-template-functions-string.md#uniquestring) för att generera ett hash-värde.</span><span class="sxs-lookup"><span data-stu-id="b37e9-125">The preceding template uses the [uniqueString](resource-group-template-functions-string.md#uniquestring) function to generate a hash value.</span></span> <span data-ttu-id="b37e9-126">För att hash-värdet ska vara lättare att förstå har prefixet *storage* lagts till.</span><span class="sxs-lookup"><span data-stu-id="b37e9-126">To give this hash value more meaning, it adds the prefix *storage*.</span></span> 

3. <span data-ttu-id="b37e9-127">Spara den här filen som **azuredeploy.json** i en lokal mapp.</span><span class="sxs-lookup"><span data-stu-id="b37e9-127">Save this file as **azuredeploy.json** to a local folder.</span></span>

   ![Spara mallen](./media/resource-manager-create-first-template/save-template.png)

## <a name="deploy-template"></a><span data-ttu-id="b37e9-129">Distribuera mallen</span><span class="sxs-lookup"><span data-stu-id="b37e9-129">Deploy template</span></span>

<span data-ttu-id="b37e9-130">Nu är det dags att distribuera den här mallen.</span><span class="sxs-lookup"><span data-stu-id="b37e9-130">You are ready to deploy this template.</span></span> <span data-ttu-id="b37e9-131">Du använder PowerShell eller Azure CLI för att skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="b37e9-131">You use either PowerShell or Azure CLI to create a resource group.</span></span> <span data-ttu-id="b37e9-132">Sedan distribuerar du ett lagringskonto till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b37e9-132">Then, you deploy a storage account to that resource group.</span></span>

* <span data-ttu-id="b37e9-133">Om du använder PowerShell använder du följande kommandon från mappen som innehåller mallen:</span><span class="sxs-lookup"><span data-stu-id="b37e9-133">For PowerShell, use the following commands from the folder containing the template:</span></span>

   ```powershell
   Login-AzureRmAccount
   
   New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
   New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json
   ```

* <span data-ttu-id="b37e9-134">Om du använder en lokal installation av Azure CLI använder du följande kommandon från mappen som innehåller mallen:</span><span class="sxs-lookup"><span data-stu-id="b37e9-134">For a local installation of Azure CLI, use the following commands from the folder containing the template:</span></span>

   ```azurecli
   az login

   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file azuredeploy.json
   ```

<span data-ttu-id="b37e9-135">När distributionen är klar finns ditt lagringskonto i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b37e9-135">When deployment finishes, your storage account exists in the resource group.</span></span>

## <a name="deploy-template-from-cloud-shell"></a><span data-ttu-id="b37e9-136">Distribuera mallen från Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="b37e9-136">Deploy template from Cloud Shell</span></span>

<span data-ttu-id="b37e9-137">Du kan använda [Cloud Shell](../cloud-shell/overview.md) för att köra Azure CLI-kommandona och distribuera mallen.</span><span class="sxs-lookup"><span data-stu-id="b37e9-137">You can use [Cloud Shell](../cloud-shell/overview.md) to run the Azure CLI commands for deploying your template.</span></span> <span data-ttu-id="b37e9-138">Först måste du dock läsa in mallen till filresursen för Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="b37e9-138">However, you must first load your template into the file share for your Cloud Shell.</span></span> <span data-ttu-id="b37e9-139">Om du inte har använt Cloud Shell tidigare läser du [Overview of Azure Cloud Shell](../cloud-shell/overview.md) (Översikt över Azure Cloud Shell), som innehåller information om hur du konfigurerar Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="b37e9-139">If you have not used Cloud Shell, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md) for information about setting it up.</span></span>

1. <span data-ttu-id="b37e9-140">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b37e9-140">Log in to the [Azure portal](https://portal.azure.com).</span></span>   

2. <span data-ttu-id="b37e9-141">Välj din Cloud Shell-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="b37e9-141">Select your Cloud Shell resource group.</span></span> <span data-ttu-id="b37e9-142">Namnet har formatet `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="b37e9-142">The name pattern is `cloud-shell-storage-<region>`.</span></span>

   ![Välj resursgrupp](./media/resource-manager-create-first-template/select-cs-resource-group.png)

3. <span data-ttu-id="b37e9-144">Välj lagringskontot för Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="b37e9-144">Select the storage account for your Cloud Shell.</span></span>

   ![Välj lagringskonto](./media/resource-manager-create-first-template/select-storage.png)

4. <span data-ttu-id="b37e9-146">Välj **Filer**.</span><span class="sxs-lookup"><span data-stu-id="b37e9-146">Select **Files**.</span></span>

   ![Välj filer](./media/resource-manager-create-first-template/select-files.png)

5. <span data-ttu-id="b37e9-148">Välj filresursen för Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="b37e9-148">Select the file share for Cloud Shell.</span></span> <span data-ttu-id="b37e9-149">Namnet har formatet `cs-<user>-<domain>-com-<uniqueGuid>`.</span><span class="sxs-lookup"><span data-stu-id="b37e9-149">The name pattern is `cs-<user>-<domain>-com-<uniqueGuid>`.</span></span>

   ![Välj filresurs](./media/resource-manager-create-first-template/select-file-share.png)

6. <span data-ttu-id="b37e9-151">Välj **Lägg till katalog**.</span><span class="sxs-lookup"><span data-stu-id="b37e9-151">Select **Add directory**.</span></span>

   ![Lägg till katalog](./media/resource-manager-create-first-template/select-add-directory.png)

7. <span data-ttu-id="b37e9-153">Ge den namnet **templates** och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="b37e9-153">Name it **templates**, and select **Okay**.</span></span>

   ![Namnge katalogen](./media/resource-manager-create-first-template/name-templates.png)

8. <span data-ttu-id="b37e9-155">Välj den nya katalogen.</span><span class="sxs-lookup"><span data-stu-id="b37e9-155">Select your new directory.</span></span>

   ![Välj katalog](./media/resource-manager-create-first-template/select-templates.png)

9. <span data-ttu-id="b37e9-157">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="b37e9-157">Select **Upload**.</span></span>

   ![Välj Överför](./media/resource-manager-create-first-template/select-upload.png)

10. <span data-ttu-id="b37e9-159">Leta upp och överför mallen.</span><span class="sxs-lookup"><span data-stu-id="b37e9-159">Find and upload your template.</span></span>

   ![Ladda upp filen](./media/resource-manager-create-first-template/upload-files.png)

11. <span data-ttu-id="b37e9-161">Öppna kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="b37e9-161">Open the prompt.</span></span>

   ![Öppna Cloud Shell](./media/resource-manager-create-first-template/start-cloud-shell.png)

12. <span data-ttu-id="b37e9-163">Ange följande kommandon i Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="b37e9-163">Enter the following commands in the Cloud Shell:</span></span>

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json
   ```

<span data-ttu-id="b37e9-164">När distributionen är klar finns ditt lagringskonto i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b37e9-164">When deployment finishes, your storage account exists in the resource group.</span></span>

## <a name="customize-the-template"></a><span data-ttu-id="b37e9-165">Anpassa mallen</span><span class="sxs-lookup"><span data-stu-id="b37e9-165">Customize the template</span></span>

<span data-ttu-id="b37e9-166">Mallen fungerar bra, men den är inte flexibel.</span><span class="sxs-lookup"><span data-stu-id="b37e9-166">The template works fine, but it is not flexible.</span></span> <span data-ttu-id="b37e9-167">Den distribuerar alltid lokal redundant lagring till Södra centrala USA.</span><span class="sxs-lookup"><span data-stu-id="b37e9-167">It always deploys a locally redundant storage to South Central US.</span></span> <span data-ttu-id="b37e9-168">Namnet är alltid *storage* följt av ett hash-värde.</span><span class="sxs-lookup"><span data-stu-id="b37e9-168">The name is always *storage* followed by a hash value.</span></span> <span data-ttu-id="b37e9-169">Lägg till parametrar till mallen om du vill använda den i andra scenarier.</span><span class="sxs-lookup"><span data-stu-id="b37e9-169">To enable using the template for different scenarios, add parameters to the template.</span></span>

<span data-ttu-id="b37e9-170">Följande exempel illustrerar parameters-avsnittet med två parametrar.</span><span class="sxs-lookup"><span data-stu-id="b37e9-170">The following example shows the parameters section with two parameters.</span></span> <span data-ttu-id="b37e9-171">Med den första parametern, `storageSKU`, kan du ange typen av redundans.</span><span class="sxs-lookup"><span data-stu-id="b37e9-171">The first parameter `storageSKU` enables you to specify the type of redundancy.</span></span> <span data-ttu-id="b37e9-172">Den begränsar de värden som du kan använda till värden som är giltiga för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b37e9-172">It limits the values you can pass in to values that are valid for a storage account.</span></span> <span data-ttu-id="b37e9-173">Den definierar också ett standardvärde.</span><span class="sxs-lookup"><span data-stu-id="b37e9-173">It also specifies a default value.</span></span> <span data-ttu-id="b37e9-174">Den andra parametern, `storageNamePrefix`, har konfigurerats att tillåta högst 11 tecken.</span><span class="sxs-lookup"><span data-stu-id="b37e9-174">The second parameter `storageNamePrefix` is set to allow a maximum of 11 characters.</span></span> <span data-ttu-id="b37e9-175">Den definierar ett standardvärde.</span><span class="sxs-lookup"><span data-stu-id="b37e9-175">It specifies a default value.</span></span>

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  },
  "storageNamePrefix": {
    "type": "string",
    "maxLength": 11,
    "defaultValue": "storage",
    "metadata": {
      "description": "The value to use for starting the storage account name. Use only lowercase letters and numbers."
    }
  }
},
```

<span data-ttu-id="b37e9-176">Lägg till en variabel med namnet `storageName` i variables-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="b37e9-176">In the variables section, add a variable named `storageName`.</span></span> <span data-ttu-id="b37e9-177">Den kombinerar prefixvärdet från parametrarna och ett hash-värde från funktionen [uniqueString](resource-group-template-functions-string.md#uniquestring).</span><span class="sxs-lookup"><span data-stu-id="b37e9-177">It combines the prefix value from the parameters and a hash value from the [uniqueString](resource-group-template-functions-string.md#uniquestring) function.</span></span> <span data-ttu-id="b37e9-178">Den använder funktionen [toLower](resource-group-template-functions-string.md#tolower) för att konvertera alla tecken till gemener.</span><span class="sxs-lookup"><span data-stu-id="b37e9-178">It uses the [toLower](resource-group-template-functions-string.md#tolower) function to convert all characters to lowercase.</span></span>

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

<span data-ttu-id="b37e9-179">Ändra resursdefinitionen om du vill använda dessa nya värden för ditt lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="b37e9-179">To use these new values for your storage account, change the resource definition:</span></span>

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    "kind": "Storage",
    "location": "[resourceGroup().location]",
    "tags": {},
    "properties": {}
  }
],
```

<span data-ttu-id="b37e9-180">Observera att namnet på lagringskontot använder variabeln som du lade till.</span><span class="sxs-lookup"><span data-stu-id="b37e9-180">Notice that the name of the storage account is now set to the variable that you added.</span></span> <span data-ttu-id="b37e9-181">SKU-namnet motsvarar värdet för parametern.</span><span class="sxs-lookup"><span data-stu-id="b37e9-181">The SKU name is set to the value of the parameter.</span></span> <span data-ttu-id="b37e9-182">Platsen är samma som för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b37e9-182">The location is set the same location as the resource group.</span></span>

<span data-ttu-id="b37e9-183">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="b37e9-183">Save your file.</span></span> 

<span data-ttu-id="b37e9-184">När du har slutfört stegen i den här artikeln ser din mall ut så här:</span><span class="sxs-lookup"><span data-stu-id="b37e9-184">After completing the steps in this article, your template now looks like:</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "The type of replication to use for the storage account."
      }
    },   
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name. Use only lowercase letters and numbers."
      }
    }
  },
  "variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {}
    }
  ],
  "outputs": {  }
}
```

## <a name="redeploy-template"></a><span data-ttu-id="b37e9-185">Distribuera om mallen</span><span class="sxs-lookup"><span data-stu-id="b37e9-185">Redeploy template</span></span>

<span data-ttu-id="b37e9-186">Distribuera om mallen med andra värden.</span><span class="sxs-lookup"><span data-stu-id="b37e9-186">Redeploy the template with different values.</span></span>

<span data-ttu-id="b37e9-187">Om du använder PowerShell använder du:</span><span class="sxs-lookup"><span data-stu-id="b37e9-187">For PowerShell, use:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json -storageNamePrefix newstore -storageSKU Standard_RAGRS
```

<span data-ttu-id="b37e9-188">Om du använder Azure CLI använder du:</span><span class="sxs-lookup"><span data-stu-id="b37e9-188">For Azure CLI, use:</span></span>

```azurecli
az group deployment create --resource-group examplegroup --template-file azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

<span data-ttu-id="b37e9-189">Om du använder Cloud Shell laddar du upp den ändrade mallen till filresursen.</span><span class="sxs-lookup"><span data-stu-id="b37e9-189">For the Cloud Shell, upload your changed template to the file share.</span></span> <span data-ttu-id="b37e9-190">Skriv över den befintliga filen.</span><span class="sxs-lookup"><span data-stu-id="b37e9-190">Overwrite the existing file.</span></span> <span data-ttu-id="b37e9-191">Använd sedan följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b37e9-191">Then, use the following command:</span></span>

```azurecli
az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

## <a name="clean-up-resources"></a><span data-ttu-id="b37e9-192">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="b37e9-192">Clean up resources</span></span>

<span data-ttu-id="b37e9-193">När de inte längre behövs rensar du de resurser som du har distribuerat genom att ta bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b37e9-193">When no longer needed, clean up the resources you deployed by deleting the resource group.</span></span>

<span data-ttu-id="b37e9-194">Om du använder PowerShell använder du:</span><span class="sxs-lookup"><span data-stu-id="b37e9-194">For PowerShell, use:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name examplegroup
```

<span data-ttu-id="b37e9-195">Om du använder Azure CLI använder du:</span><span class="sxs-lookup"><span data-stu-id="b37e9-195">For Azure CLI, use:</span></span>

```azurecli
az group delete --name examplegroup
```

## <a name="next-steps"></a><span data-ttu-id="b37e9-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b37e9-196">Next steps</span></span>
* <span data-ttu-id="b37e9-197">Mer information om strukturen i en mall finns i [Redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b37e9-197">To learn more about the structure of a template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="b37e9-198">Mer information om egenskaperna för ett lagringskonto finns i [mallreferensen för lagringskonton](/azure/templates/microsoft.storage/storageaccounts).</span><span class="sxs-lookup"><span data-stu-id="b37e9-198">To learn about the properties for a storage account, see [storage accounts template reference](/azure/templates/microsoft.storage/storageaccounts).</span></span>
* <span data-ttu-id="b37e9-199">Om du vill visa kompletta mallar för många olika typer av lösningar kan du se [Azure-snabbstartsmallar](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="b37e9-199">To view complete templates for many different types of solutions, see the [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates/).</span></span>
