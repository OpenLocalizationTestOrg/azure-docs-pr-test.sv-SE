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
# <a name="use-key-vault-toopass-secure-parameter-value-during-deployment"></a>Använd Key Vault toopass säker parametervärdet under distributionen

När du behöver toopass säkert värde (till exempel ett lösenord) som en parameter under distributionen kan du hämta hello-värde från en [Azure Key Vault](../key-vault/key-vault-whatis.md). Du kan hämta hello-värde genom att referera hello nyckelvalvet och hemlighet i parameterfilen. hello värdet exponeras aldrig eftersom du endast referera till ett nyckelvalv-ID. Du behöver inte toomanually ange hello värde för hello hemlighet varje gång du distribuerar hello resurser. Hej nyckelvalv kan finnas i en annan prenumeration än hello resursgrupp som du distribuerar till. När du refererar till hello nyckelvalv inkludera du hello prenumerations-ID.

Ange hello när du skapar hello nyckelvalv *enabledForTemplateDeployment* egenskapen för*SANT*. Genom att ange det här värdet tootrue kan bevilja du åtkomst från Resource Manager-mallar under distributionen.  

## <a name="deploy-a-key-vault-and-secret"></a>Distribuera ett nyckelvalv och hemlighet

toocreate ett nyckelvalv och hemlighet, använder du Azure CLI eller PowerShell. Observera att hello nyckelvalv har aktiverats för malldistribution. 

Om du använder Azure CLI använder du:

```azurecli
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create --name $vaultname --resource-group examplegroup --location 'South Central US' --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

Om du använder PowerShell använder du:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault -VaultName $vaultname -ResourceGroupName examplegroup -Location "South Central US" -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-toohello-secret"></a>Aktivera åtkomst toohello hemlighet

Om du använder ett nytt nyckelvalv eller en befintlig, kontrollera hello användaren distribuerar hello mall kan komma åt hello hemlighet. hello-användaren distribuerar en mall som refererar till en hemlighet måste ha hello `Microsoft.KeyVault/vaults/deploy/action` för hello nyckelvalvet. Hej [ägare](../active-directory/role-based-access-built-in-roles.md#owner) och [deltagare](../active-directory/role-based-access-built-in-roles.md#contributor) roller båda bevilja åtkomst. Du kan också skapa en [anpassad roll](../active-directory/role-based-access-control-custom-roles.md) som ger den här behörigheten och lägga till hello toothat användarrollen. Information om att lägga till en användarroll tooa finns [och tilldela en användare tooadministrator roller i Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).


## <a name="reference-a-secret-with-static-id"></a>Referera till en hemlighet med statiska ID

hello-mallen som tar emot en hemlighet i nyckelvalvet är precis som alla andra mallar. Det beror på **du refererar till hello nyckelvalvet i parameterfilen hello, inte hello mall.** Exempelvis distribuerar hello följande mall för en SQL-databas som innehåller ett administratörslösenord. Parametern för hello-password anges tooa säker sträng. Men hello mallen anger inte där värdet kommer från.

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

Nu skapa en parameterfil för hello föregående mall. Ange en parameter som matchar hello namnet på hello parameter i hello mallen i hello parameterfil. Hello parametervärde, referera till hello hemliga från hello nyckelvalvet. Du referera hello hemligheten genom att skicka hello resurs-ID för hello nyckelvalvet och hello namnet på hello hemlighet. I följande exempel hello, hello nyckelvalv hemlighet måste redan finnas och du kan ange ett statiskt värde för resurs-ID.

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

## <a name="reference-a-secret-with-dynamic-id"></a>Referera till en hemlighet med dynamiska ID

hello föregående avsnitt visades hur toopass en statisk resurs-ID för hello nyckeln valvet hemlighet. I vissa scenarier behöver du dock tooreference nyckelvalv hemlighet som varierar baserat på aktuella hello-distribution. I så fall kan du inte hårdkoda hello resurs-ID i hello parameterfilen. Dynamiskt tyvärr, du kan inte generera hello resurs-ID i parameterfilen hello eftersom malluttryck inte tillåts i hello parameterfilen.

toodynamically generera hello-resurs-ID för en hemlighet i nyckelvalvet, måste du flytta hello-resurs som måste hello hemlighet i en kapslad mall. I mallen master du lägger till hello kapslad mall och skicka in en parameter som innehåller en dynamiskt genererad hello resurs-ID.

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

## <a name="next-steps"></a>Nästa steg
* Allmän information om nyckelvalv finns [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md).
* Komplett exempel på refererar till viktiga hemligheter finns [Key Vault exempel](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

