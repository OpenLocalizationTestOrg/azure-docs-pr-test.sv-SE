---
title: "aaaCreate första Azure Resource Manager-mall | Microsoft Docs"
description: "En stegvis guide toocreating ditt första Azure Resource Manager-mallen. Den visar hur toouse hello Mallreferens för en storage-konto toocreate hello mall."
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
ms.openlocfilehash: 92e6d6bb7094fe0e4537ee080704967862804bdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-your-first-azure-resource-manager-template"></a><span data-ttu-id="7f528-104">Skapa och distribuera din första Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="7f528-104">Create and deploy your first Azure Resource Manager template</span></span>
<span data-ttu-id="7f528-105">Det här avsnittet vägleder dig genom hello stegen för att skapa din första Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="7f528-105">This topic walks you through hello steps of creating your first Azure Resource Manager template.</span></span> <span data-ttu-id="7f528-106">Resource Manager-mallarna är JSON-filer som definierar hello resurser du behöver toodeploy för din lösning.</span><span class="sxs-lookup"><span data-stu-id="7f528-106">Resource Manager templates are JSON files that define hello resources you need toodeploy for your solution.</span></span> <span data-ttu-id="7f528-107">toounderstand hello begrepp som är associerade med att distribuera och hantera dina Azure lösningar finns [översikt över Azure Resource Manager](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7f528-107">toounderstand hello concepts associated with deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span> <span data-ttu-id="7f528-108">Om du har befintliga resurser och vill tooget en mall för dessa resurser, se [exportera en Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="7f528-108">If you have existing resources and want tooget a template for those resources, see [Export an Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>

<span data-ttu-id="7f528-109">toocreate och ändra mallar, behöver du en JSON-redigerare.</span><span class="sxs-lookup"><span data-stu-id="7f528-109">toocreate and revise templates, you need a JSON editor.</span></span> <span data-ttu-id="7f528-110">[Visual Studio Code](https://code.visualstudio.com/) är en enkel, plattformsoberoende kodredigerare med öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="7f528-110">[Visual Studio Code](https://code.visualstudio.com/) is a lightweight, open-source, cross-platform code editor.</span></span> <span data-ttu-id="7f528-111">Vi rekommenderar starkt att du använder Visual Studio Code för att skapa Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="7f528-111">We strongly recommend using Visual Studio Code for creating Resource Manager templates.</span></span> <span data-ttu-id="7f528-112">Den här artikeln förutsätter att du använder Virtual Studio Code, men om du har en annan JSON-redigerare kan du använda den.</span><span class="sxs-lookup"><span data-stu-id="7f528-112">This topic assumes you are using VS Code; however, if you have another JSON editor (like Visual Studio), you can use that editor.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f528-113">Krav</span><span class="sxs-lookup"><span data-stu-id="7f528-113">Prerequisites</span></span>

* <span data-ttu-id="7f528-114">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7f528-114">Visual Studio Code.</span></span> <span data-ttu-id="7f528-115">Om det behövs installerar du det från [https://code.visualstudio.com/](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="7f528-115">If needed, install it from [https://code.visualstudio.com/](https://code.visualstudio.com/).</span></span>
* <span data-ttu-id="7f528-116">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7f528-116">An Azure subscription.</span></span> <span data-ttu-id="7f528-117">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="7f528-117">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="create-template"></a><span data-ttu-id="7f528-118">Skapa mallen</span><span class="sxs-lookup"><span data-stu-id="7f528-118">Create template</span></span>

<span data-ttu-id="7f528-119">Låt oss börja med en enkel mall som distribuerar en tooyour lagringskontoprenumerationen.</span><span class="sxs-lookup"><span data-stu-id="7f528-119">Let's start with a simple template that deploys a storage account tooyour subscription.</span></span>

1. <span data-ttu-id="7f528-120">Välj **Arkiv** > **Ny fil**.</span><span class="sxs-lookup"><span data-stu-id="7f528-120">Select **File** > **New File**.</span></span> 

   ![Ny fil](./media/resource-manager-create-first-template/new-file.png)

2. <span data-ttu-id="7f528-122">Kopiera och klistra in hello följande JSON-syntax i filen:</span><span class="sxs-lookup"><span data-stu-id="7f528-122">Copy and paste hello following JSON syntax into your file:</span></span>

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

   <span data-ttu-id="7f528-123">Lagringskontonamn har flera begränsningar som gör dem svåra tooset.</span><span class="sxs-lookup"><span data-stu-id="7f528-123">Storage account names have several restrictions that make them difficult tooset.</span></span> <span data-ttu-id="7f528-124">hello namn måste vara mellan 3 och 24 tecken, Använd endast siffror och gemener och vara unika.</span><span class="sxs-lookup"><span data-stu-id="7f528-124">hello name must be between 3 and 24 characters in length, use only numbers and lower-case letters, and be unique.</span></span> <span data-ttu-id="7f528-125">hello föregående mallen använder hello [uniqueString](resource-group-template-functions-string.md#uniquestring) fungerar toogenerate ett hash-värde.</span><span class="sxs-lookup"><span data-stu-id="7f528-125">hello preceding template uses hello [uniqueString](resource-group-template-functions-string.md#uniquestring) function toogenerate a hash value.</span></span> <span data-ttu-id="7f528-126">toogive hashvärdet värde mer innebörd, läggs hello prefixet *lagring*.</span><span class="sxs-lookup"><span data-stu-id="7f528-126">toogive this hash value more meaning, it adds hello prefix *storage*.</span></span> 

3. <span data-ttu-id="7f528-127">Spara filen som **azuredeploy.json** tooa lokal mapp.</span><span class="sxs-lookup"><span data-stu-id="7f528-127">Save this file as **azuredeploy.json** tooa local folder.</span></span>

   ![Spara mallen](./media/resource-manager-create-first-template/save-template.png)

## <a name="deploy-template"></a><span data-ttu-id="7f528-129">Distribuera mallen</span><span class="sxs-lookup"><span data-stu-id="7f528-129">Deploy template</span></span>

<span data-ttu-id="7f528-130">Du är klar toodeploy den här mallen.</span><span class="sxs-lookup"><span data-stu-id="7f528-130">You are ready toodeploy this template.</span></span> <span data-ttu-id="7f528-131">Du kan använda PowerShell eller Azure CLI toocreate en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7f528-131">You use either PowerShell or Azure CLI toocreate a resource group.</span></span> <span data-ttu-id="7f528-132">Sedan distribuerar du en resursgrupp för storage-konto toothat.</span><span class="sxs-lookup"><span data-stu-id="7f528-132">Then, you deploy a storage account toothat resource group.</span></span>

* <span data-ttu-id="7f528-133">Använd följande kommandon från hello mapp som innehåller hello mall hello för PowerShell:</span><span class="sxs-lookup"><span data-stu-id="7f528-133">For PowerShell, use hello following commands from hello folder containing hello template:</span></span>

   ```powershell
   Login-AzureRmAccount
   
   New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
   New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json
   ```

* <span data-ttu-id="7f528-134">Använd hello följande kommandon från hello mapp som innehåller hello mall för en lokal installation av Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="7f528-134">For a local installation of Azure CLI, use hello following commands from hello folder containing hello template:</span></span>

   ```azurecli
   az login

   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file azuredeploy.json
   ```

<span data-ttu-id="7f528-135">När distributionen är klar finns ditt lagringskonto i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7f528-135">When deployment finishes, your storage account exists in hello resource group.</span></span>

## <a name="deploy-template-from-cloud-shell"></a><span data-ttu-id="7f528-136">Distribuera mallen från Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="7f528-136">Deploy template from Cloud Shell</span></span>

<span data-ttu-id="7f528-137">Du kan använda [moln Shell](../cloud-shell/overview.md) toorun hello Azure CLI-kommandon för att distribuera mallen.</span><span class="sxs-lookup"><span data-stu-id="7f528-137">You can use [Cloud Shell](../cloud-shell/overview.md) toorun hello Azure CLI commands for deploying your template.</span></span> <span data-ttu-id="7f528-138">Men du måste först läsa in mallen i hello filresurs för moln-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="7f528-138">However, you must first load your template into hello file share for your Cloud Shell.</span></span> <span data-ttu-id="7f528-139">Om du inte har använt Cloud Shell tidigare läser du [Overview of Azure Cloud Shell](../cloud-shell/overview.md) (Översikt över Azure Cloud Shell), som innehåller information om hur du konfigurerar Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="7f528-139">If you have not used Cloud Shell, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md) for information about setting it up.</span></span>

1. <span data-ttu-id="7f528-140">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7f528-140">Log in toohello [Azure portal](https://portal.azure.com).</span></span>   

2. <span data-ttu-id="7f528-141">Välj din Cloud Shell-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7f528-141">Select your Cloud Shell resource group.</span></span> <span data-ttu-id="7f528-142">hello namnmönstret är `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="7f528-142">hello name pattern is `cloud-shell-storage-<region>`.</span></span>

   ![Välj resursgrupp](./media/resource-manager-create-first-template/select-cs-resource-group.png)

3. <span data-ttu-id="7f528-144">Välj hello storage-konto för moln-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="7f528-144">Select hello storage account for your Cloud Shell.</span></span>

   ![Välj lagringskonto](./media/resource-manager-create-first-template/select-storage.png)

4. <span data-ttu-id="7f528-146">Välj **Filer**.</span><span class="sxs-lookup"><span data-stu-id="7f528-146">Select **Files**.</span></span>

   ![Välj filer](./media/resource-manager-create-first-template/select-files.png)

5. <span data-ttu-id="7f528-148">Välj hello filresurs för molnet Shell.</span><span class="sxs-lookup"><span data-stu-id="7f528-148">Select hello file share for Cloud Shell.</span></span> <span data-ttu-id="7f528-149">hello namnmönstret är `cs-<user>-<domain>-com-<uniqueGuid>`.</span><span class="sxs-lookup"><span data-stu-id="7f528-149">hello name pattern is `cs-<user>-<domain>-com-<uniqueGuid>`.</span></span>

   ![Välj filresurs](./media/resource-manager-create-first-template/select-file-share.png)

6. <span data-ttu-id="7f528-151">Välj **Lägg till katalog**.</span><span class="sxs-lookup"><span data-stu-id="7f528-151">Select **Add directory**.</span></span>

   ![Lägg till katalog](./media/resource-manager-create-first-template/select-add-directory.png)

7. <span data-ttu-id="7f528-153">Ge den namnet **templates** och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="7f528-153">Name it **templates**, and select **Okay**.</span></span>

   ![Namnge katalogen](./media/resource-manager-create-first-template/name-templates.png)

8. <span data-ttu-id="7f528-155">Välj den nya katalogen.</span><span class="sxs-lookup"><span data-stu-id="7f528-155">Select your new directory.</span></span>

   ![Välj katalog](./media/resource-manager-create-first-template/select-templates.png)

9. <span data-ttu-id="7f528-157">Välj **Överför**.</span><span class="sxs-lookup"><span data-stu-id="7f528-157">Select **Upload**.</span></span>

   ![Välj Överför](./media/resource-manager-create-first-template/select-upload.png)

10. <span data-ttu-id="7f528-159">Leta upp och överför mallen.</span><span class="sxs-lookup"><span data-stu-id="7f528-159">Find and upload your template.</span></span>

   ![Ladda upp filen](./media/resource-manager-create-first-template/upload-files.png)

11. <span data-ttu-id="7f528-161">Öppna hello-prompt.</span><span class="sxs-lookup"><span data-stu-id="7f528-161">Open hello prompt.</span></span>

   ![Öppna Cloud Shell](./media/resource-manager-create-first-template/start-cloud-shell.png)

12. <span data-ttu-id="7f528-163">Ange följande kommandon i hello molnet Shell hello:</span><span class="sxs-lookup"><span data-stu-id="7f528-163">Enter hello following commands in hello Cloud Shell:</span></span>

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json
   ```

<span data-ttu-id="7f528-164">När distributionen är klar finns ditt lagringskonto i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7f528-164">When deployment finishes, your storage account exists in hello resource group.</span></span>

## <a name="customize-hello-template"></a><span data-ttu-id="7f528-165">Anpassa hello-mall</span><span class="sxs-lookup"><span data-stu-id="7f528-165">Customize hello template</span></span>

<span data-ttu-id="7f528-166">hello mallen fungerar bra, men det är inte flexibel.</span><span class="sxs-lookup"><span data-stu-id="7f528-166">hello template works fine, but it is not flexible.</span></span> <span data-ttu-id="7f528-167">Den distribuerar alltid en lokalt redundant lagring tooSouth centrala USA.</span><span class="sxs-lookup"><span data-stu-id="7f528-167">It always deploys a locally redundant storage tooSouth Central US.</span></span> <span data-ttu-id="7f528-168">hello namn är alltid *lagring* följt av ett hash-värde.</span><span class="sxs-lookup"><span data-stu-id="7f528-168">hello name is always *storage* followed by a hash value.</span></span> <span data-ttu-id="7f528-169">tooenable använder hello mall för olika scenarier, lägga till parametrar toohello mall.</span><span class="sxs-lookup"><span data-stu-id="7f528-169">tooenable using hello template for different scenarios, add parameters toohello template.</span></span>

<span data-ttu-id="7f528-170">hello visar följande exempel hello parametrar avsnitt med två parametrar.</span><span class="sxs-lookup"><span data-stu-id="7f528-170">hello following example shows hello parameters section with two parameters.</span></span> <span data-ttu-id="7f528-171">Hej första parametern `storageSKU` kan du toospecify hello typen av redundans.</span><span class="sxs-lookup"><span data-stu-id="7f528-171">hello first parameter `storageSKU` enables you toospecify hello type of redundancy.</span></span> <span data-ttu-id="7f528-172">Det begränsar hello-värden som du kan skicka in toovalues som är giltiga för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7f528-172">It limits hello values you can pass in toovalues that are valid for a storage account.</span></span> <span data-ttu-id="7f528-173">Den definierar också ett standardvärde.</span><span class="sxs-lookup"><span data-stu-id="7f528-173">It also specifies a default value.</span></span> <span data-ttu-id="7f528-174">Hej andra parametern `storageNamePrefix` är uppsättningen tooallow Maximalt 11 tecken.</span><span class="sxs-lookup"><span data-stu-id="7f528-174">hello second parameter `storageNamePrefix` is set tooallow a maximum of 11 characters.</span></span> <span data-ttu-id="7f528-175">Den definierar ett standardvärde.</span><span class="sxs-lookup"><span data-stu-id="7f528-175">It specifies a default value.</span></span>

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
      "description": "hello type of replication toouse for hello storage account."
    }
  },
  "storageNamePrefix": {
    "type": "string",
    "maxLength": 11,
    "defaultValue": "storage",
    "metadata": {
      "description": "hello value toouse for starting hello storage account name. Use only lowercase letters and numbers."
    }
  }
},
```

<span data-ttu-id="7f528-176">Lägg till en variabel med namnet under hello variabler `storageName`.</span><span class="sxs-lookup"><span data-stu-id="7f528-176">In hello variables section, add a variable named `storageName`.</span></span> <span data-ttu-id="7f528-177">Det kombinerar hello prefixvärde från hello parametrar och ett hash-värde från hello [uniqueString](resource-group-template-functions-string.md#uniquestring) funktion.</span><span class="sxs-lookup"><span data-stu-id="7f528-177">It combines hello prefix value from hello parameters and a hash value from hello [uniqueString](resource-group-template-functions-string.md#uniquestring) function.</span></span> <span data-ttu-id="7f528-178">Den använder hello [toLower](resource-group-template-functions-string.md#tolower) fungerar tooconvert toolowercase för alla tecken.</span><span class="sxs-lookup"><span data-stu-id="7f528-178">It uses hello [toLower](resource-group-template-functions-string.md#tolower) function tooconvert all characters toolowercase.</span></span>

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

<span data-ttu-id="7f528-179">toouse dessa nya värden för ditt lagringskonto, ändra hello resursdefinitionen:</span><span class="sxs-lookup"><span data-stu-id="7f528-179">toouse these new values for your storage account, change hello resource definition:</span></span>

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

<span data-ttu-id="7f528-180">Observera att hello hello lagring kontonamn anges nu toohello variabel som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="7f528-180">Notice that hello name of hello storage account is now set toohello variable that you added.</span></span> <span data-ttu-id="7f528-181">hello SKU namn anges toohello hello parameterns värde.</span><span class="sxs-lookup"><span data-stu-id="7f528-181">hello SKU name is set toohello value of hello parameter.</span></span> <span data-ttu-id="7f528-182">hello platsen angetts hello samma plats som hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7f528-182">hello location is set hello same location as hello resource group.</span></span>

<span data-ttu-id="7f528-183">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="7f528-183">Save your file.</span></span> 

<span data-ttu-id="7f528-184">När du har slutfört hello stegen i den här artikeln mallen nu ser ut som:</span><span class="sxs-lookup"><span data-stu-id="7f528-184">After completing hello steps in this article, your template now looks like:</span></span>

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
        "description": "hello type of replication toouse for hello storage account."
      }
    },   
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "hello value toouse for starting hello storage account name. Use only lowercase letters and numbers."
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

## <a name="redeploy-template"></a><span data-ttu-id="7f528-185">Distribuera om mallen</span><span class="sxs-lookup"><span data-stu-id="7f528-185">Redeploy template</span></span>

<span data-ttu-id="7f528-186">Omdistribuera hello mallen med olika värden.</span><span class="sxs-lookup"><span data-stu-id="7f528-186">Redeploy hello template with different values.</span></span>

<span data-ttu-id="7f528-187">Om du använder PowerShell använder du:</span><span class="sxs-lookup"><span data-stu-id="7f528-187">For PowerShell, use:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json -storageNamePrefix newstore -storageSKU Standard_RAGRS
```

<span data-ttu-id="7f528-188">Om du använder Azure CLI använder du:</span><span class="sxs-lookup"><span data-stu-id="7f528-188">For Azure CLI, use:</span></span>

```azurecli
az group deployment create --resource-group examplegroup --template-file azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

<span data-ttu-id="7f528-189">Överför ändrade mallen toohello filresursen för hello molnet Shell.</span><span class="sxs-lookup"><span data-stu-id="7f528-189">For hello Cloud Shell, upload your changed template toohello file share.</span></span> <span data-ttu-id="7f528-190">Skriv över befintlig hello-fil.</span><span class="sxs-lookup"><span data-stu-id="7f528-190">Overwrite hello existing file.</span></span> <span data-ttu-id="7f528-191">Använd sedan följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7f528-191">Then, use hello following command:</span></span>

```azurecli
az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

## <a name="clean-up-resources"></a><span data-ttu-id="7f528-192">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="7f528-192">Clean up resources</span></span>

<span data-ttu-id="7f528-193">Rensa hello-resurser som du har distribuerat genom att ta bort resursgruppen hello när de inte längre behövs.</span><span class="sxs-lookup"><span data-stu-id="7f528-193">When no longer needed, clean up hello resources you deployed by deleting hello resource group.</span></span>

<span data-ttu-id="7f528-194">Om du använder PowerShell använder du:</span><span class="sxs-lookup"><span data-stu-id="7f528-194">For PowerShell, use:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name examplegroup
```

<span data-ttu-id="7f528-195">Om du använder Azure CLI använder du:</span><span class="sxs-lookup"><span data-stu-id="7f528-195">For Azure CLI, use:</span></span>

```azurecli
az group delete --name examplegroup
```

## <a name="next-steps"></a><span data-ttu-id="7f528-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f528-196">Next steps</span></span>
* <span data-ttu-id="7f528-197">toolearn mer om hello strukturen i en mall finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="7f528-197">toolearn more about hello structure of a template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="7f528-198">toolearn om hello egenskaper för ett lagringskonto finns [lagringskonton mallreferensen](/azure/templates/microsoft.storage/storageaccounts).</span><span class="sxs-lookup"><span data-stu-id="7f528-198">toolearn about hello properties for a storage account, see [storage accounts template reference](/azure/templates/microsoft.storage/storageaccounts).</span></span>
* <span data-ttu-id="7f528-199">tooview fullständig mallar för många olika typer av lösningar, se hello [Azure Quickstart mallar](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="7f528-199">tooview complete templates for many different types of solutions, see hello [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates/).</span></span>
