---
title: aaaKey valvet hemligheten med Resource Manager-mall | Microsoft Docs
description: "Visar hur valvet toopass en hemlighet från en nyckel som en parameter under distributionen."
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c582c144-4760-49d3-b793-a3e1e89128e2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 0bb7760c95b3b4ef34c9e5cc2e3421be56b5e5e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-key-vault-toopass-secure-parameter-value-during-deployment"></a><span data-ttu-id="cfe45-103">Använd Key Vault toopass säker parametervärdet under distributionen</span><span class="sxs-lookup"><span data-stu-id="cfe45-103">Use Key Vault toopass secure parameter value during deployment</span></span>

<span data-ttu-id="cfe45-104">När du behöver toopass säkert värde (till exempel ett lösenord) som en parameter under distributionen kan du hämta hello-värde från en [Azure Key Vault](../key-vault/key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cfe45-104">When you need toopass a secure value (like a password) as a parameter during deployment, you can retrieve hello value from an [Azure Key Vault](../key-vault/key-vault-whatis.md).</span></span> <span data-ttu-id="cfe45-105">Du kan hämta hello-värde genom att referera hello nyckelvalvet och hemlighet i parameterfilen.</span><span class="sxs-lookup"><span data-stu-id="cfe45-105">You retrieve hello value by referencing hello key vault and secret in your parameter file.</span></span> <span data-ttu-id="cfe45-106">hello värdet exponeras aldrig eftersom du endast referera till ett nyckelvalv-ID.</span><span class="sxs-lookup"><span data-stu-id="cfe45-106">hello value is never exposed because you only reference its key vault ID.</span></span> <span data-ttu-id="cfe45-107">Du behöver inte toomanually ange hello värde för hello hemlighet varje gång du distribuerar hello resurser.</span><span class="sxs-lookup"><span data-stu-id="cfe45-107">You do not need toomanually enter hello value for hello secret each time you deploy hello resources.</span></span> <span data-ttu-id="cfe45-108">Hej nyckelvalv kan finnas i en annan prenumeration än hello resursgrupp som du distribuerar till.</span><span class="sxs-lookup"><span data-stu-id="cfe45-108">hello key vault can exist in a different subscription than hello resource group you are deploying to.</span></span> <span data-ttu-id="cfe45-109">När du refererar till hello nyckelvalv inkludera du hello prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="cfe45-109">When referencing hello key vault, you include hello subscription ID.</span></span>

<span data-ttu-id="cfe45-110">Ange hello när du skapar hello nyckelvalv *enabledForTemplateDeployment* egenskapen för*SANT*.</span><span class="sxs-lookup"><span data-stu-id="cfe45-110">When creating hello key vault, set hello *enabledForTemplateDeployment* property too*true*.</span></span> <span data-ttu-id="cfe45-111">Genom att ange det här värdet tootrue kan bevilja du åtkomst från Resource Manager-mallar under distributionen.</span><span class="sxs-lookup"><span data-stu-id="cfe45-111">By setting this value tootrue, you permit access from Resource Manager templates during deployment.</span></span>  

## <a name="deploy-a-key-vault-and-secret"></a><span data-ttu-id="cfe45-112">Distribuera ett nyckelvalv och hemlighet</span><span class="sxs-lookup"><span data-stu-id="cfe45-112">Deploy a key vault and secret</span></span>

<span data-ttu-id="cfe45-113">toocreate ett nyckelvalv och hemlighet, använder du Azure CLI eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cfe45-113">toocreate a key vault and secret, use either Azure CLI or PowerShell.</span></span> <span data-ttu-id="cfe45-114">Observera att hello nyckelvalv har aktiverats för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="cfe45-114">Notice that hello key vault is enabled for template deployment.</span></span> 

<span data-ttu-id="cfe45-115">Om du använder Azure CLI använder du:</span><span class="sxs-lookup"><span data-stu-id="cfe45-115">For Azure CLI, use:</span></span>

```azurecli
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create --name $vaultname --resource-group examplegroup --location 'South Central US' --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

<span data-ttu-id="cfe45-116">Om du använder PowerShell använder du:</span><span class="sxs-lookup"><span data-stu-id="cfe45-116">For PowerShell, use:</span></span>

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault -VaultName $vaultname -ResourceGroupName examplegroup -Location "South Central US" -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-toohello-secret"></a><span data-ttu-id="cfe45-117">Aktivera åtkomst toohello hemlighet</span><span class="sxs-lookup"><span data-stu-id="cfe45-117">Enable access toohello secret</span></span>

<span data-ttu-id="cfe45-118">Om du använder ett nytt nyckelvalv eller en befintlig, kontrollera hello användaren distribuerar hello mall kan komma åt hello hemlighet.</span><span class="sxs-lookup"><span data-stu-id="cfe45-118">Whether you are using a new key vault or an existing one, ensure that hello user deploying hello template can access hello secret.</span></span> <span data-ttu-id="cfe45-119">hello-användaren distribuerar en mall som refererar till en hemlighet måste ha hello `Microsoft.KeyVault/vaults/deploy/action` för hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="cfe45-119">hello user deploying a template that references a secret must have hello `Microsoft.KeyVault/vaults/deploy/action` permission for hello key vault.</span></span> <span data-ttu-id="cfe45-120">Hej [ägare](../active-directory/role-based-access-built-in-roles.md#owner) och [deltagare](../active-directory/role-based-access-built-in-roles.md#contributor) roller båda bevilja åtkomst.</span><span class="sxs-lookup"><span data-stu-id="cfe45-120">hello [Owner](../active-directory/role-based-access-built-in-roles.md#owner) and [Contributor](../active-directory/role-based-access-built-in-roles.md#contributor) roles both grant this access.</span></span> <span data-ttu-id="cfe45-121">Du kan också skapa en [anpassad roll](../active-directory/role-based-access-control-custom-roles.md) som ger den här behörigheten och lägga till hello toothat användarrollen.</span><span class="sxs-lookup"><span data-stu-id="cfe45-121">You can also create a [custom role](../active-directory/role-based-access-control-custom-roles.md) that grants this permission and add hello user toothat role.</span></span> <span data-ttu-id="cfe45-122">Information om att lägga till en användarroll tooa finns [och tilldela en användare tooadministrator roller i Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cfe45-122">For information about adding a user tooa role, see [Assign a user tooadministrator roles in Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).</span></span>


## <a name="reference-a-secret-with-static-id"></a><span data-ttu-id="cfe45-123">Referera till en hemlighet med statiska ID</span><span class="sxs-lookup"><span data-stu-id="cfe45-123">Reference a secret with static ID</span></span>

<span data-ttu-id="cfe45-124">hello-mallen som tar emot en hemlighet i nyckelvalvet är precis som alla andra mallar.</span><span class="sxs-lookup"><span data-stu-id="cfe45-124">hello template that receives a key vault secret is like any other template.</span></span> <span data-ttu-id="cfe45-125">Det beror på **du refererar till hello nyckelvalvet i parameterfilen hello, inte hello mall.**</span><span class="sxs-lookup"><span data-stu-id="cfe45-125">That's because **you reference hello key vault in hello parameter file, not hello template.**</span></span> <span data-ttu-id="cfe45-126">Exempelvis distribuerar hello följande mall för en SQL-databas som innehåller ett administratörslösenord.</span><span class="sxs-lookup"><span data-stu-id="cfe45-126">For example, hello following template deploys a SQL database that includes an administrator password.</span></span> <span data-ttu-id="cfe45-127">Parametern för hello-password anges tooa säker sträng.</span><span class="sxs-lookup"><span data-stu-id="cfe45-127">hello password parameter is set tooa secure string.</span></span> <span data-ttu-id="cfe45-128">Men hello mallen anger inte där värdet kommer från.</span><span class="sxs-lookup"><span data-stu-id="cfe45-128">But, hello template does not specify where that value comes from.</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "type": "string"
        },
        "administratorLoginPassword": {
            "type": "securestring"
        },
        "collation": {
            "type": "string"
        },
        "databaseName": {
            "type": "string"
        },
        "edition": {
            "type": "string"
        },
        "requestedServiceObjectiveId": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "maxSizeBytes": {
            "type": "string"
        },
        "serverName": {
            "type": "string"
        },
        "sampleName": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "name": "[parameters('serverName')]",
            "properties": {
                "administratorLogin": "[parameters('administratorLogin')]",
                "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
                "version": "12.0"
            },
            "resources": [
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "[parameters('databaseName')]",
                    "properties": {
                        "collation": "[parameters('collation')]",
                        "edition": "[parameters('edition')]",
                        "maxSizeBytes": "[parameters('maxSizeBytes')]",
                        "requestedServiceObjectiveId": "[parameters('requestedServiceObjectiveId')]",
                        "sampleName": "[parameters('sampleName')]"
                    },
                    "type": "databases"
                },
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "AllowAllWindowsAzureIps",
                    "properties": {
                        "endIpAddress": "0.0.0.0",
                        "startIpAddress": "0.0.0.0"
                    },
                    "type": "firewallrules"
                }
            ],
            "type": "Microsoft.Sql/servers"
        }
    ]
}
```

<span data-ttu-id="cfe45-129">Nu skapa en parameterfil för hello föregående mall.</span><span class="sxs-lookup"><span data-stu-id="cfe45-129">Now, create a parameter file for hello preceding template.</span></span> <span data-ttu-id="cfe45-130">Ange en parameter som matchar hello namnet på hello parameter i hello mallen i hello parameterfil.</span><span class="sxs-lookup"><span data-stu-id="cfe45-130">In hello parameter file, specify a parameter that matches hello name of hello parameter in hello template.</span></span> <span data-ttu-id="cfe45-131">Hello parametervärde, referera till hello hemliga från hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="cfe45-131">For hello parameter value, reference hello secret from hello key vault.</span></span> <span data-ttu-id="cfe45-132">Du referera hello hemligheten genom att skicka hello resurs-ID för hello nyckelvalvet och hello namnet på hello hemlighet.</span><span class="sxs-lookup"><span data-stu-id="cfe45-132">You reference hello secret by passing hello resource identifier of hello key vault and hello name of hello secret.</span></span> <span data-ttu-id="cfe45-133">I följande exempel hello, hello nyckelvalv hemlighet måste redan finnas och du kan ange ett statiskt värde för resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="cfe45-133">In hello following example, hello key vault secret must already exist, and you provide a static value for its resource ID.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "value": "exampleadmin"
        },
        "administratorLoginPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/{guid}/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/{vault-name}"
              },
              "secretName": "examplesecret"
            }
        },
        "collation": {
            "value": "SQL_Latin1_General_CP1_CI_AS"
        },
        "databaseName": {
            "value": "exampledb"
        },
        "edition": {
            "value": "Standard"
        },
        "location": {
            "value": "southcentralus"
        },
        "maxSizeBytes": {
            "value": "268435456000"
        },
        "requestedServiceObjectiveId": {
            "value": "455330e1-00cd-488b-b5fa-177c226f28b7"
        },
        "sampleName": {
            "value": ""
        },
        "serverName": {
            "value": "exampleserver"
        }
    }
}
```

## <a name="reference-a-secret-with-dynamic-id"></a><span data-ttu-id="cfe45-134">Referera till en hemlighet med dynamiska ID</span><span class="sxs-lookup"><span data-stu-id="cfe45-134">Reference a secret with dynamic ID</span></span>

<span data-ttu-id="cfe45-135">hello föregående avsnitt visades hur toopass en statisk resurs-ID för hello nyckeln valvet hemlighet.</span><span class="sxs-lookup"><span data-stu-id="cfe45-135">hello previous section showed how toopass a static resource ID for hello key vault secret.</span></span> <span data-ttu-id="cfe45-136">I vissa scenarier behöver du dock tooreference nyckelvalv hemlighet som varierar baserat på aktuella hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="cfe45-136">However, in some scenarios, you need tooreference a key vault secret that varies based on hello current deployment.</span></span> <span data-ttu-id="cfe45-137">I så fall kan du inte hårdkoda hello resurs-ID i hello parameterfilen.</span><span class="sxs-lookup"><span data-stu-id="cfe45-137">In that case, you cannot hard-code hello resource ID in hello parameters file.</span></span> <span data-ttu-id="cfe45-138">Dynamiskt tyvärr, du kan inte generera hello resurs-ID i parameterfilen hello eftersom malluttryck inte tillåts i hello parameterfilen.</span><span class="sxs-lookup"><span data-stu-id="cfe45-138">Unfortunately, you cannot dynamically generate hello resource ID in hello parameters file because template expressions are not permitted in hello parameters file.</span></span>

<span data-ttu-id="cfe45-139">toodynamically generera hello-resurs-ID för en hemlighet i nyckelvalvet, måste du flytta hello-resurs som måste hello hemlighet i en kapslad mall.</span><span class="sxs-lookup"><span data-stu-id="cfe45-139">toodynamically generate hello resource ID for a key vault secret, you must move hello resource that needs hello secret into a nested template.</span></span> <span data-ttu-id="cfe45-140">I mallen master du lägger till hello kapslad mall och skicka in en parameter som innehåller en dynamiskt genererad hello resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="cfe45-140">In your master template, you add hello nested template and pass in a parameter that contains hello dynamically generated resource ID.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "secretName": {
        "type": "string"
      }
    },
    "resources": [
    {
      "apiVersion": "2015-01-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          }
        }
      }
    }],
    "outputs": {}
}
```

## <a name="next-steps"></a><span data-ttu-id="cfe45-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cfe45-141">Next steps</span></span>
* <span data-ttu-id="cfe45-142">Allmän information om nyckelvalv finns [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cfe45-142">For general information about key vaults, see [Get started with Azure Key Vault](../key-vault/key-vault-get-started.md).</span></span>
* <span data-ttu-id="cfe45-143">Komplett exempel på refererar till viktiga hemligheter finns [Key Vault exempel](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).</span><span class="sxs-lookup"><span data-stu-id="cfe45-143">For complete examples of referencing key secrets, see [Key Vault examples](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).</span></span>

