---
title: "aaaCreate och publicera ett program för katalogen som hanteras av Azure-tjänst | Microsoft Docs"
description: "Visar hur toocreate en Azure hanterade program som är avsedd för medlemmar i din organisation."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 31f2f9e3b50f57dae7f4dcf2edefa7366bfff25c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-managed-application-for-internal-consumption"></a><span data-ttu-id="a2baf-103">Publicera ett hanterat program för internt bruk</span><span class="sxs-lookup"><span data-stu-id="a2baf-103">Publish a managed application for internal consumption</span></span>

<span data-ttu-id="a2baf-104">Du kan skapa och publicera Azure [hanterade program](managed-application-overview.md) som är avsedda för medlemmar i din organisation.</span><span class="sxs-lookup"><span data-stu-id="a2baf-104">You can create and publish Azure [managed applications](managed-application-overview.md) that are intended for members of your organization.</span></span> <span data-ttu-id="a2baf-105">Exempelvis kan en IT-avdelning publicera hanterade program som säkerställa efterlevnad med organisationens normer.</span><span class="sxs-lookup"><span data-stu-id="a2baf-105">For example, an IT department can publish managed applications that ensure compliance with organizational standards.</span></span> <span data-ttu-id="a2baf-106">Dessa hanterade program är tillgängliga via hello tjänstkatalog, inte hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a2baf-106">These managed applications are available through hello service catalog, not hello Azure Marketplace.</span></span>

<span data-ttu-id="a2baf-107">toopublish ett hanterat program för hello tjänstkatalogen, måste du:</span><span class="sxs-lookup"><span data-stu-id="a2baf-107">toopublish a managed application for hello service catalog, you must:</span></span>

* <span data-ttu-id="a2baf-108">Skapa en ZIP-paketet som innehåller hello tre filer som krävs för mallen.</span><span class="sxs-lookup"><span data-stu-id="a2baf-108">Create a .zip package that contains hello three required template files.</span></span>
* <span data-ttu-id="a2baf-109">Bestäm vilka användare, grupp eller ett program behöver komma åt toohello resursgrupp i hello användarens prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a2baf-109">Decide which user, group, or application needs access toohello resource group in hello user's subscription.</span></span>
* <span data-ttu-id="a2baf-110">Skapa definition för hello hanteras som pekar toohello ZIP-paketet och begär åtkomst för hello identitet.</span><span class="sxs-lookup"><span data-stu-id="a2baf-110">Create hello managed application definition that points toohello .zip package and requests access for hello identity.</span></span>

## <a name="create-a-managed-application-package"></a><span data-ttu-id="a2baf-111">Skapa ett paket för hanterade program</span><span class="sxs-lookup"><span data-stu-id="a2baf-111">Create a managed application package</span></span>

<span data-ttu-id="a2baf-112">hello första steget är toocreate hello tre nödvändiga mallfiler.</span><span class="sxs-lookup"><span data-stu-id="a2baf-112">hello first step is toocreate hello three required template files.</span></span> <span data-ttu-id="a2baf-113">Paketera alla tre filerna till en ZIP-fil och överför den tooan tillgänglig plats, till exempel ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a2baf-113">Package all three files into a .zip file, and upload it tooan accessible location, such as a storage account.</span></span> <span data-ttu-id="a2baf-114">Du skickar en länk toothis ZIP-fil när du skapar hello hanterade definition för program.</span><span class="sxs-lookup"><span data-stu-id="a2baf-114">You pass a link toothis .zip file when creating hello managed application definition.</span></span>

* <span data-ttu-id="a2baf-115">**applianceMainTemplate.json**: den här filen definierar hello Azure resurser som tillhandahålls som en del av hello hanterade program.</span><span class="sxs-lookup"><span data-stu-id="a2baf-115">**applianceMainTemplate.json**: This file defines hello Azure resources that are provisioned as part of hello managed application.</span></span> <span data-ttu-id="a2baf-116">hello mallen är inte annorlunda än en vanlig Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="a2baf-116">hello template is no different than a regular Resource Manager template.</span></span> <span data-ttu-id="a2baf-117">Till exempel toocreate ett lagringskonto via ett hanterat program applianceMainTemplate.json innehåller:</span><span class="sxs-lookup"><span data-stu-id="a2baf-117">For example, toocreate a storage account through a managed application, applianceMainTemplate.json contains:</span></span>

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(parameters('storageAccountNamePrefix'), uniqueString(resourceGroup().id))]",
            "apiVersion": "2016-01-01",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ],
    "outputs": {}
  }
  ```

* <span data-ttu-id="a2baf-118">**mainTemplate.json**: användare kan distribuera den här mallen när du skapar hello hanterade program.</span><span class="sxs-lookup"><span data-stu-id="a2baf-118">**mainTemplate.json**: Users deploy this template when creating hello managed application.</span></span> <span data-ttu-id="a2baf-119">Den definierar hello hanterade programresurs, vilket är en Microsoft.Solutions/appliances resurstyp.</span><span class="sxs-lookup"><span data-stu-id="a2baf-119">It defines hello managed application resource, which is a Microsoft.Solutions/appliances resource type.</span></span> <span data-ttu-id="a2baf-120">Den här filen innehåller alla hello-parametrar som du behöver för hello resurser i applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="a2baf-120">This file contains all hello parameters you need for hello resources in applianceMainTemplate.json.</span></span>

  <span data-ttu-id="a2baf-121">Du kan ange två viktiga egenskaper i den här mallen.</span><span class="sxs-lookup"><span data-stu-id="a2baf-121">You set two important properties in this template.</span></span> <span data-ttu-id="a2baf-122">Först hello **applianceDefinitionId** egenskapen är hello-ID för definition av hello hanterade program.</span><span class="sxs-lookup"><span data-stu-id="a2baf-122">First, hello **applianceDefinitionId** property is hello ID of hello managed application definition.</span></span> <span data-ttu-id="a2baf-123">Du kan skapa definition hello senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a2baf-123">You create hello definition later in this topic.</span></span> <span data-ttu-id="a2baf-124">När du ställer in det här värdet måste du bestämma vilka prenumeration och resurs grupp toouse för att lagra hello programdefinitioner för hanterade.</span><span class="sxs-lookup"><span data-stu-id="a2baf-124">When setting this value, you must decide which subscription and resource group toouse for storing hello managed application definitions.</span></span> <span data-ttu-id="a2baf-125">Och du måste bestämma om ett namn för hello definition.</span><span class="sxs-lookup"><span data-stu-id="a2baf-125">And, you must decide on a name for hello definition.</span></span> <span data-ttu-id="a2baf-126">hello-ID har formatet hello:</span><span class="sxs-lookup"><span data-stu-id="a2baf-126">hello ID is in hello format:</span></span>

  `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Solutions/applianceDefinitions/<definition-name>`

  <span data-ttu-id="a2baf-127">Andra hello **managedResourceGroupId** egenskapen är hello-ID för hello resursgruppen där hello Azure-resurser skapas.</span><span class="sxs-lookup"><span data-stu-id="a2baf-127">Second, hello **managedResourceGroupId** property is hello ID of hello resource group where hello Azure resources are created.</span></span> <span data-ttu-id="a2baf-128">Du kan tilldela ett värde för den här resursgruppens namn eller låt hello användaren ange ett namn.</span><span class="sxs-lookup"><span data-stu-id="a2baf-128">You can assign a value for this resource group name or let hello user provide a name.</span></span> <span data-ttu-id="a2baf-129">hello-formatet för hello-ID är:</span><span class="sxs-lookup"><span data-stu-id="a2baf-129">hello format of hello ID is:</span></span>

  <span data-ttu-id="a2baf-130">`/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.</span><span class="sxs-lookup"><span data-stu-id="a2baf-130">`/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.</span></span>

  <span data-ttu-id="a2baf-131">hello som följande exempel visar en mainTemplate.json-fil.</span><span class="sxs-lookup"><span data-stu-id="a2baf-131">hello following example shows a mainTemplate.json file.</span></span> <span data-ttu-id="a2baf-132">Det anger en resursgrupp för hello distribuerade resurser.</span><span class="sxs-lookup"><span data-stu-id="a2baf-132">It specifies a resource group for hello deployed resources.</span></span> <span data-ttu-id="a2baf-133">hello-ID: N är uppsättningen toouse en definition med namnet **storageApp** i en resursgrupp med namnet **managedApplicationGroup**.</span><span class="sxs-lookup"><span data-stu-id="a2baf-133">hello definition ID is set toouse a definition named **storageApp** in a resource group named **managedApplicationGroup**.</span></span> <span data-ttu-id="a2baf-134">Du kan ändra dessa värden toouse olika namn.</span><span class="sxs-lookup"><span data-stu-id="a2baf-134">You can change these values toouse different names.</span></span> <span data-ttu-id="a2baf-135">Ange en egen prenumerations-ID i hello definition-ID.</span><span class="sxs-lookup"><span data-stu-id="a2baf-135">Provide your own subscription ID in hello definition ID.</span></span>

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        }
    },
    "variables": {
        "managedRGId": "[concat(resourceGroup().id,'-application-resources')]",
        "managedAppName": "[concat('managedStorage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Solutions/appliances",
            "name": "[variables('managedAppName')]",
            "apiVersion": "2016-09-01-preview",
            "location": "[resourceGroup().location]",
            "kind": "ServiceCatalog",
            "properties": {
                "managedResourceGroupId": "[variables('managedRGId')]",
                "applianceDefinitionId": "/subscriptions/<subscription-id>/resourceGroups/managedApplicationGroup/providers/Microsoft.Solutions/applianceDefinitions/storageApp",
                "parameters": {
                    "storageAccountNamePrefix": {
                        "value": "[parameters('storageAccountNamePrefix')]"
                    }
                }
            }
        }
    ]
  }
  ```

* <span data-ttu-id="a2baf-136">**applianceCreateUiDefinition.json**: hello Azure-portalen använder den här filen toogenerate hello användargränssnitt för användare som skapar hello hanterade program.</span><span class="sxs-lookup"><span data-stu-id="a2baf-136">**applianceCreateUiDefinition.json**: hello Azure portal uses this file toogenerate hello user interface for users who create hello managed application.</span></span> <span data-ttu-id="a2baf-137">Du definierar hur användare ange indata för varje parameter.</span><span class="sxs-lookup"><span data-stu-id="a2baf-137">You define how users provide input for each parameter.</span></span> <span data-ttu-id="a2baf-138">Du kan använda alternativ som en listrutan, textrutan, lösenord och andra indata verktyg.</span><span class="sxs-lookup"><span data-stu-id="a2baf-138">You can use options like a drop-down list, text box, password box, and other input tools.</span></span> <span data-ttu-id="a2baf-139">hur toocreate ett UI-definitionsfilen för hanterade program, se toolearn [Kom igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a2baf-139">toolearn how toocreate a UI definition file for a managed application, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>

  <span data-ttu-id="a2baf-140">hello visas följande exempel en applianceCreateUiDefinition.json-fil som gör att användare toospecify hello storage-konto namnprefix via en textruta.</span><span class="sxs-lookup"><span data-stu-id="a2baf-140">hello following example shows an applianceCreateUiDefinition.json file that enables users toospecify hello storage account name prefix through a textbox.</span></span>

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json",
    "handler": "Microsoft.Compute.MultiVm",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {
                "name": "storageAccounts",
                "type": "Microsoft.Common.TextBox",
                "label": "Storage account name prefix",
                "defaultValue": "storage",
                "toolTip": "Provide a value that is used for hello prefix of your storage account. Limit too11 characters.",
                "constraints": {
                    "required": true,
                    "regex": "^[a-z0-9A-Z]{1,11}$",
                    "validationMessage": "Only alphanumeric characters are allowed, and hello value must be 1-11 characters long."
                },
                "visible": true
            }
        ],
        "steps": [],
        "outputs": {
            "storageAccountNamePrefix": "[basics('storageAccounts')]"
        }
    }
  }
  ```

<span data-ttu-id="a2baf-141">När alla filer som behövs hello är klar kan du paketera dem som en .zip-fil.</span><span class="sxs-lookup"><span data-stu-id="a2baf-141">After all hello needed files are ready, package them as a .zip file.</span></span> <span data-ttu-id="a2baf-142">hello tre filer måste vara på hello rotnivå hello ZIP-filen.</span><span class="sxs-lookup"><span data-stu-id="a2baf-142">hello three files must be at hello root level of hello .zip file.</span></span> <span data-ttu-id="a2baf-143">Om du placerar dem i en mapp, felmeddelande ett när du skapar hello hanterade definition som anger hello krävs filer inte finns.</span><span class="sxs-lookup"><span data-stu-id="a2baf-143">If you put them in a folder, you receive an error when creating hello managed application definition that states hello required files are not present.</span></span> <span data-ttu-id="a2baf-144">Överför hello paketet tooan tillgänglig plats från där den kan användas.</span><span class="sxs-lookup"><span data-stu-id="a2baf-144">Upload hello package tooan accessible location from where it can be consumed.</span></span> <span data-ttu-id="a2baf-145">hello resten av den här artikeln förutsätter hello ZIP-filen finns i en offentligt tillgänglig lagring blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="a2baf-145">hello remainder of this article assumes hello .zip file exists in a publicly accessible storage blob container.</span></span>

## <a name="create-an-azure-active-directory-user-group-or-application"></a><span data-ttu-id="a2baf-146">Skapa ett Azure Active Directory-användargrupp eller ett program</span><span class="sxs-lookup"><span data-stu-id="a2baf-146">Create an Azure Active Directory user group or application</span></span>

<span data-ttu-id="a2baf-147">hello andra steget är tooselect en användargrupp eller ett program för att hantera hello resurser för hello kunds räkning.</span><span class="sxs-lookup"><span data-stu-id="a2baf-147">hello second step is tooselect a user group or application for managing hello resources on behalf of hello customer.</span></span> <span data-ttu-id="a2baf-148">Den här användargruppen eller program har behörigheter på hello hanterade resurser grupp bl.a toohello roll som är tilldelad.</span><span class="sxs-lookup"><span data-stu-id="a2baf-148">This user group or application has permissions on hello managed resource group according toohello role that is assigned.</span></span> <span data-ttu-id="a2baf-149">hello rollen kan vara en inbyggd roll rollbaserad åtkomstkontroll (RBAC) som ägare eller deltagare.</span><span class="sxs-lookup"><span data-stu-id="a2baf-149">hello role can be any built-in Role-Based Access Control (RBAC) role like Owner or Contributor.</span></span> <span data-ttu-id="a2baf-150">Du också kan ge en användare behörighet toomanage hello resurser, men vanligtvis du tilldela den här behörigheten tooa användargruppen.</span><span class="sxs-lookup"><span data-stu-id="a2baf-150">You also can give an individual user permission toomanage hello resources, but typically you assign this permission tooa user group.</span></span> <span data-ttu-id="a2baf-151">toocreate en ny Active Directory-användargrupp finns [skapar en grupp och lägga till medlemmar i Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a2baf-151">toocreate a new Active Directory user group, see [Create a group and add members in Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).</span></span>

<span data-ttu-id="a2baf-152">Du måste hello objekt-ID för hello användaren grupp toouse för att hantera hello resurser.</span><span class="sxs-lookup"><span data-stu-id="a2baf-152">You need hello object ID of hello user group toouse for managing hello resources.</span></span> <span data-ttu-id="a2baf-153">hello som följande exempel visar hur tooget hello objekt-ID från hello gruppens visningsnamn:</span><span class="sxs-lookup"><span data-stu-id="a2baf-153">hello following example shows how tooget hello object ID from hello group's display name:</span></span>

```azurecli-interactive
az ad group show --group exampleGroupName
```

<span data-ttu-id="a2baf-154">hello kommando returnerar hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="a2baf-154">hello example command returns hello following output:</span></span>

```azurecli
{
    "displayName": "exampleGroupName",
    "mail": null,
    "objectId": "9aabd3ad-3716-4242-9d8e-a85df479d5d9",
    "objectType": "Group",
    "securityEnabled": true
}
```

<span data-ttu-id="a2baf-155">tooretrieve bara hello objekt-ID, Använd:</span><span class="sxs-lookup"><span data-stu-id="a2baf-155">tooretrieve just hello object ID, use:</span></span>

```azurecli-interactive
groupid=$(az ad group show --group exampleGroupName --query objectId --output tsv)
```

## <a name="get-hello-role-definition-id"></a><span data-ttu-id="a2baf-156">Hämta hello roll-ID: N</span><span class="sxs-lookup"><span data-stu-id="a2baf-156">Get hello role definition ID</span></span>

<span data-ttu-id="a2baf-157">Sedan måste hello rolldefinitions-ID: av hello RBAC inbyggd roll som du vill toogrant åtkomst toohello användare, grupp eller programmet.</span><span class="sxs-lookup"><span data-stu-id="a2baf-157">Next, you need hello role definition ID of hello RBAC built-in role you want toogrant access toohello user, user group, or application.</span></span> <span data-ttu-id="a2baf-158">Normalt använder du hello ägare eller deltagare eller läsare roll.</span><span class="sxs-lookup"><span data-stu-id="a2baf-158">Typically, you use hello Owner or Contributor or Reader role.</span></span> <span data-ttu-id="a2baf-159">hello följande kommando visar hur tooget hello rolldefinitions-ID: för hello ägarrollen:</span><span class="sxs-lookup"><span data-stu-id="a2baf-159">hello following command shows how tooget hello role definition ID for hello Owner role:</span></span>


```azurecli-interactive
az role definition list --name owner
```

<span data-ttu-id="a2baf-160">Detta kommando returnerar hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="a2baf-160">That command returns hello following output:</span></span>

```azurecli
{
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "name": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "properties": {
      "assignableScopes": [
        "/"
      ],
      "description": "Lets you manage everything, including access tooresources.",
      "permissions": [
        {
          "actions": [
            "*"
         ],
         "notActions": []
        }
      ],
      "roleName": "Owner",
      "type": "BuiltInRole"
    },
    "type": "Microsoft.Authorization/roleDefinitions"
}
```

<span data-ttu-id="a2baf-161">Du måste hello name-egenskapen från föregående exempel hello hello värdet.</span><span class="sxs-lookup"><span data-stu-id="a2baf-161">You need hello value of hello "name" property from hello preceding example.</span></span> <span data-ttu-id="a2baf-162">Du kan hämta bara egenskapen med:</span><span class="sxs-lookup"><span data-stu-id="a2baf-162">You can retrieve just that property with:</span></span>

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

## <a name="create-hello-managed-application-definition"></a><span data-ttu-id="a2baf-163">Skapa definition för hello hanteras</span><span class="sxs-lookup"><span data-stu-id="a2baf-163">Create hello managed application definition</span></span>

<span data-ttu-id="a2baf-164">Om du inte redan har en resursgrupp för att lagra dina definition för hanterade program ska du skapa en nu:</span><span class="sxs-lookup"><span data-stu-id="a2baf-164">If you do not already have a resource group for storing your managed application definition, create one now:</span></span>

```azurecli-interactive
az group create --name managedApplicationGroup --location westcentralus
```

<span data-ttu-id="a2baf-165">Nu skapa hello hanteras programresursen definition.</span><span class="sxs-lookup"><span data-stu-id="a2baf-165">Now, create hello managed application definition resource.</span></span>

```azurecli-interactive
az managedapp definition create \
  --name storageApp \
  --location "westcentralus" \
  --resource-group managedApplicationGroup \
  --lock-level ReadOnly \
  --display-name myteststorageapp \
  --description storageapp \
  --authorizations "$groupid:$roleid" \
  --package-file-uri <uri-path-to-zip-file>
```

<span data-ttu-id="a2baf-166">hello-parametrar som används i föregående exempel hello är:</span><span class="sxs-lookup"><span data-stu-id="a2baf-166">hello parameters used in hello preceding example are:</span></span>

* <span data-ttu-id="a2baf-167">**resursgruppens namn**: hello namn hello resursgruppen där hello hanterade definition för program som har skapats.</span><span class="sxs-lookup"><span data-stu-id="a2baf-167">**resource-group**: hello name of hello resource group where hello managed application definition is created.</span></span>
* <span data-ttu-id="a2baf-168">**Lås på objektnivå**: hello typ av Lås placerad hello hanterade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="a2baf-168">**lock-level**: hello type of lock placed on hello managed resource group.</span></span> <span data-ttu-id="a2baf-169">Det förhindrar att hello kunden oönskade åtgärder pågår på den här resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="a2baf-169">It prevents hello customer from performing undesirable operations on this resource group.</span></span> <span data-ttu-id="a2baf-170">För närvarande är ReadOnly hello stöds endast Lås nivå.</span><span class="sxs-lookup"><span data-stu-id="a2baf-170">Currently, ReadOnly is hello only supported lock level.</span></span> <span data-ttu-id="a2baf-171">När ReadOnly anges kan hello kunden endast läsa hello resurser finns i hello hanterade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="a2baf-171">When ReadOnly is specified, hello customer can only read hello resources present in hello managed resource group.</span></span>
* <span data-ttu-id="a2baf-172">**tillstånd**: Beskriver hello ägar-ID och hello roll-ID: N som används toogrant behörighetsgruppen toohello hanterade resurser.</span><span class="sxs-lookup"><span data-stu-id="a2baf-172">**authorizations**: Describes hello principal ID and hello role definition ID that are used toogrant permission toohello managed resource group.</span></span> <span data-ttu-id="a2baf-173">Det har angetts i hello-format för `<principalId>:<roleDefinitionId>`.</span><span class="sxs-lookup"><span data-stu-id="a2baf-173">It's specified in hello format of `<principalId>:<roleDefinitionId>`.</span></span> <span data-ttu-id="a2baf-174">Flera värden kan också anges för den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="a2baf-174">Multiple values also can be specified for this property.</span></span> <span data-ttu-id="a2baf-175">Om flera värden krävs, de anges i form av hello `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`.</span><span class="sxs-lookup"><span data-stu-id="a2baf-175">If multiple values are needed, they should be specified in hello form `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`.</span></span> <span data-ttu-id="a2baf-176">Flera värden avgränsas med ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="a2baf-176">Multiple values are separated by a space.</span></span>
* <span data-ttu-id="a2baf-177">**paket-fil-uri**: hello platsen för hello hanterade programpaket som innehåller hello mallfilerna, vilket kan vara en Azure Storage blob.</span><span class="sxs-lookup"><span data-stu-id="a2baf-177">**package-file-uri**: hello location of hello managed application package that contains hello template files, which can be an Azure Storage blob.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a2baf-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a2baf-178">Next steps</span></span>

* <span data-ttu-id="a2baf-179">En introduktion toomanaged program, se [hanteras Programöversikt](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a2baf-179">For an introduction toomanaged applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="a2baf-180">Exempel på hello filer finns [hanterade program exempel](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).</span><span class="sxs-lookup"><span data-stu-id="a2baf-180">For examples of hello files, see [Managed application samples](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).</span></span>
* <span data-ttu-id="a2baf-181">Information om att använda ett Tjänstkatalogen hanterade program, se [använder ett Tjänstkatalogen hanterade program](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="a2baf-181">For information about consuming a Service Catalog managed application, see [Consume a Service Catalog managed application](managed-application-consumption.md).</span></span>
* <span data-ttu-id="a2baf-182">Information om publishing hanterade program toohello Azure Marketplace finns [Azure hanterade program i hello Marketplace](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="a2baf-182">For information about publishing managed applications toohello Azure Marketplace, see [Azure managed applications in hello Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="a2baf-183">Information om hur du använder ett hanterat program från hello Marketplace finns [använda Azure hanterade program i hello Marketplace](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="a2baf-183">For information about consuming a managed application from hello Marketplace, see [Consume Azure managed applications in hello Marketplace](managed-application-consume-marketplace.md).</span></span>
* <span data-ttu-id="a2baf-184">hur toocreate ett UI-definitionsfilen för hanterade program, se toolearn [Kom igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a2baf-184">toolearn how toocreate a UI definition file for a managed application, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
