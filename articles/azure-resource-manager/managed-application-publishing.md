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
# <a name="publish-a-managed-application-for-internal-consumption"></a>Publicera ett hanterat program för internt bruk

Du kan skapa och publicera Azure [hanterade program](managed-application-overview.md) som är avsedda för medlemmar i din organisation. Exempelvis kan en IT-avdelning publicera hanterade program som säkerställa efterlevnad med organisationens normer. Dessa hanterade program är tillgängliga via hello tjänstkatalog, inte hello Azure Marketplace.

toopublish ett hanterat program för hello tjänstkatalogen, måste du:

* Skapa en ZIP-paketet som innehåller hello tre filer som krävs för mallen.
* Bestäm vilka användare, grupp eller ett program behöver komma åt toohello resursgrupp i hello användarens prenumeration.
* Skapa definition för hello hanteras som pekar toohello ZIP-paketet och begär åtkomst för hello identitet.

## <a name="create-a-managed-application-package"></a>Skapa ett paket för hanterade program

hello första steget är toocreate hello tre nödvändiga mallfiler. Paketera alla tre filerna till en ZIP-fil och överför den tooan tillgänglig plats, till exempel ett lagringskonto. Du skickar en länk toothis ZIP-fil när du skapar hello hanterade definition för program.

* **applianceMainTemplate.json**: den här filen definierar hello Azure resurser som tillhandahålls som en del av hello hanterade program. hello mallen är inte annorlunda än en vanlig Resource Manager-mall. Till exempel toocreate ett lagringskonto via ett hanterat program applianceMainTemplate.json innehåller:

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

* **mainTemplate.json**: användare kan distribuera den här mallen när du skapar hello hanterade program. Den definierar hello hanterade programresurs, vilket är en Microsoft.Solutions/appliances resurstyp. Den här filen innehåller alla hello-parametrar som du behöver för hello resurser i applianceMainTemplate.json.

  Du kan ange två viktiga egenskaper i den här mallen. Först hello **applianceDefinitionId** egenskapen är hello-ID för definition av hello hanterade program. Du kan skapa definition hello senare i det här avsnittet. När du ställer in det här värdet måste du bestämma vilka prenumeration och resurs grupp toouse för att lagra hello programdefinitioner för hanterade. Och du måste bestämma om ett namn för hello definition. hello-ID har formatet hello:

  `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Solutions/applianceDefinitions/<definition-name>`

  Andra hello **managedResourceGroupId** egenskapen är hello-ID för hello resursgruppen där hello Azure-resurser skapas. Du kan tilldela ett värde för den här resursgruppens namn eller låt hello användaren ange ett namn. hello-formatet för hello-ID är:

  `/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.

  hello som följande exempel visar en mainTemplate.json-fil. Det anger en resursgrupp för hello distribuerade resurser. hello-ID: N är uppsättningen toouse en definition med namnet **storageApp** i en resursgrupp med namnet **managedApplicationGroup**. Du kan ändra dessa värden toouse olika namn. Ange en egen prenumerations-ID i hello definition-ID.

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

* **applianceCreateUiDefinition.json**: hello Azure-portalen använder den här filen toogenerate hello användargränssnitt för användare som skapar hello hanterade program. Du definierar hur användare ange indata för varje parameter. Du kan använda alternativ som en listrutan, textrutan, lösenord och andra indata verktyg. hur toocreate ett UI-definitionsfilen för hanterade program, se toolearn [Kom igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).

  hello visas följande exempel en applianceCreateUiDefinition.json-fil som gör att användare toospecify hello storage-konto namnprefix via en textruta.

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

När alla filer som behövs hello är klar kan du paketera dem som en .zip-fil. hello tre filer måste vara på hello rotnivå hello ZIP-filen. Om du placerar dem i en mapp, felmeddelande ett när du skapar hello hanterade definition som anger hello krävs filer inte finns. Överför hello paketet tooan tillgänglig plats från där den kan användas. hello resten av den här artikeln förutsätter hello ZIP-filen finns i en offentligt tillgänglig lagring blob-behållare.

## <a name="create-an-azure-active-directory-user-group-or-application"></a>Skapa ett Azure Active Directory-användargrupp eller ett program

hello andra steget är tooselect en användargrupp eller ett program för att hantera hello resurser för hello kunds räkning. Den här användargruppen eller program har behörigheter på hello hanterade resurser grupp bl.a toohello roll som är tilldelad. hello rollen kan vara en inbyggd roll rollbaserad åtkomstkontroll (RBAC) som ägare eller deltagare. Du också kan ge en användare behörighet toomanage hello resurser, men vanligtvis du tilldela den här behörigheten tooa användargruppen. toocreate en ny Active Directory-användargrupp finns [skapar en grupp och lägga till medlemmar i Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).

Du måste hello objekt-ID för hello användaren grupp toouse för att hantera hello resurser. hello som följande exempel visar hur tooget hello objekt-ID från hello gruppens visningsnamn:

```azurecli-interactive
az ad group show --group exampleGroupName
```

hello kommando returnerar hello följande utdata:

```azurecli
{
    "displayName": "exampleGroupName",
    "mail": null,
    "objectId": "9aabd3ad-3716-4242-9d8e-a85df479d5d9",
    "objectType": "Group",
    "securityEnabled": true
}
```

tooretrieve bara hello objekt-ID, Använd:

```azurecli-interactive
groupid=$(az ad group show --group exampleGroupName --query objectId --output tsv)
```

## <a name="get-hello-role-definition-id"></a>Hämta hello roll-ID: N

Sedan måste hello rolldefinitions-ID: av hello RBAC inbyggd roll som du vill toogrant åtkomst toohello användare, grupp eller programmet. Normalt använder du hello ägare eller deltagare eller läsare roll. hello följande kommando visar hur tooget hello rolldefinitions-ID: för hello ägarrollen:


```azurecli-interactive
az role definition list --name owner
```

Detta kommando returnerar hello följande utdata:

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

Du måste hello name-egenskapen från föregående exempel hello hello värdet. Du kan hämta bara egenskapen med:

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

## <a name="create-hello-managed-application-definition"></a>Skapa definition för hello hanteras

Om du inte redan har en resursgrupp för att lagra dina definition för hanterade program ska du skapa en nu:

```azurecli-interactive
az group create --name managedApplicationGroup --location westcentralus
```

Nu skapa hello hanteras programresursen definition.

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

hello-parametrar som används i föregående exempel hello är:

* **resursgruppens namn**: hello namn hello resursgruppen där hello hanterade definition för program som har skapats.
* **Lås på objektnivå**: hello typ av Lås placerad hello hanterade resursgruppen. Det förhindrar att hello kunden oönskade åtgärder pågår på den här resursgruppen. För närvarande är ReadOnly hello stöds endast Lås nivå. När ReadOnly anges kan hello kunden endast läsa hello resurser finns i hello hanterade resursgruppen.
* **tillstånd**: Beskriver hello ägar-ID och hello roll-ID: N som används toogrant behörighetsgruppen toohello hanterade resurser. Det har angetts i hello-format för `<principalId>:<roleDefinitionId>`. Flera värden kan också anges för den här egenskapen. Om flera värden krävs, de anges i form av hello `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`. Flera värden avgränsas med ett blanksteg.
* **paket-fil-uri**: hello platsen för hello hanterade programpaket som innehåller hello mallfilerna, vilket kan vara en Azure Storage blob.

## <a name="next-steps"></a>Nästa steg

* En introduktion toomanaged program, se [hanteras Programöversikt](managed-application-overview.md).
* Exempel på hello filer finns [hanterade program exempel](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).
* Information om att använda ett Tjänstkatalogen hanterade program, se [använder ett Tjänstkatalogen hanterade program](managed-application-consumption.md).
* Information om publishing hanterade program toohello Azure Marketplace finns [Azure hanterade program i hello Marketplace](managed-application-author-marketplace.md).
* Information om hur du använder ett hanterat program från hello Marketplace finns [använda Azure hanterade program i hello Marketplace](managed-application-consume-marketplace.md).
* hur toocreate ett UI-definitionsfilen för hanterade program, se toolearn [Kom igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
