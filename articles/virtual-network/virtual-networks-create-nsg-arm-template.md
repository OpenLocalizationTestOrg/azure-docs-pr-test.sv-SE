---
title: "aaaCreate nätverkssäkerhetsgrupper - Azure Resource Manager-mall | Microsoft Docs"
description: "Lär dig hur toocreate och distribuera nätverkssäkerhetsgrupper med en Azure Resource Manager-mall."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: f3e7385d-717c-44ff-be20-f9aa450aa99b
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3750168284fea7b41c8c0f908b0d31a9da5e38ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-an-azure-resource-manager-template"></a>Skapa nätverk säkerhetsgrupper med en Azure Resource Manager-mall

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Den här artikeln beskriver hello Resource Manager-modellen. Du kan också [skapa NSG: er i hello klassiska distributionsmodellen](virtual-networks-create-nsg-classic-ps.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a>NSG-resurser i en mallfil
Du kan visa och hämta hello [exempelmall](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).

hello följande avsnitt visar hello definition av hello frontend NSG, baserat på hello scenario.

```json
"apiVersion": "2015-06-15",
"type": "Microsoft.Network/networkSecurityGroups",
"name": "[parameters('frontEndNSGName')]",
"location": "[resourceGroup().location]",
"tags": {
  "displayName": "NSG - Front End"
},
"properties": {
  "securityRules": [
    {
      "name": "rdp-rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 100,
        "direction": "Inbound"
      }
    },
    {
      "name": "web-rule",
      "properties": {
        "description": "Allow WEB",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 101,
        "direction": "Inbound"
      }
    }
  ]
}
```
tooassociate hello NSG toohello frontend-undernät du har toochange hello undernätsdefinition i hello mallen och Använd hello referens-id för hello NSG.

```json
"subnets": [
  {
    "name": "[parameters('frontEndSubnetName')]",
    "properties": {
      "addressPrefix": "[parameters('frontEndSubnetPrefix')]",
      "networkSecurityGroup": {
      "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('frontEndNSGName'))]"
      }
    }
  }, 
```

Observera hello samma som utfördes för hello backend-NSG och hello backend-undernät i hello mallen.

## <a name="deploy-hello-arm-template-by-using-click-toodeploy"></a>Distribuera hello ARM-mallen med hjälp av klickar du på toodeploy
hello exempelmall tillgängliga i hello offentliga databasen använder en parameterfil som innehåller hello standard värden som används för toogenerate hello scenario som beskrivs ovan. toodeploy den här mallen med hjälp av klickar du på toodeploy, Följ [länken](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), klickar du på **distribuera tooAzure**, ersätta hello Standardparametervärden om det behövs och följ instruktionerna för hello hello-portalen.

## <a name="deploy-hello-arm-template-by-using-powershell"></a>Distribuera hello ARM-mallen med hjälp av PowerShell
toodeploy hello ARM-mallen som du hämtade med PowerShell, följ hello stegen nedan.

1. Om du aldrig har använt Azure PowerShell, följer du anvisningarna för hello i hello [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) tooinstall och konfigurera den.
2. Kör hello  **`New-AzureRmResourceGroup`**  cmdlet toocreate en resurs med hello mallen.

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location uswest `
    -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
    -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    Förväntad utdata:

        ResourceGroupName : TestRG
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  
   
        Resources         :
                            Name                Type                                     Location
                            ==================  =======================================  ========
                            sqlAvSet            Microsoft.Compute/availabilitySets       westus  
                            webAvSet            Microsoft.Compute/availabilitySets       westus  
                            SQL1                Microsoft.Compute/virtualMachines        westus  
                            SQL2                Microsoft.Compute/virtualMachines        westus  
                            Web1                Microsoft.Compute/virtualMachines        westus  
                            Web2                Microsoft.Compute/virtualMachines        westus  
                            TestNICSQL1         Microsoft.Network/networkInterfaces      westus  
                            TestNICSQL2         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb1         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb2         Microsoft.Network/networkInterfaces      westus  
                            NSG-BackEnd         Microsoft.Network/networkSecurityGroups  westus  
                            NSG-FrontEnd        Microsoft.Network/networkSecurityGroups  westus  
                            TestPIPSQL1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPSQL2         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb2         Microsoft.Network/publicIPAddresses      westus  
                            TestVNet            Microsoft.Network/virtualNetworks        westus  
                            testvnetstorageprm  Microsoft.Storage/storageAccounts        westus  
                            testvnetstoragestd  Microsoft.Storage/storageAccounts        westus  
   
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG

## <a name="deploy-hello-arm-template-by-using-hello-azure-cli"></a>Distribuera hello ARM-mallen med hjälp av hello Azure CLI
toodeploy hello ARM-mallen med hjälp av hello Azure CLI, följ hello stegen nedan.

1. Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.
2. Kör hello  **`azure config mode`**  kommandot tooswitch tooResource Manager-läge enligt nedan.

    ```azurecli
    azure config mode arm
    ```

    hello följer hello förväntades utdata för hello-kommandot:

        info:    New mode is arm

3. Kör hello  **`azure group deployment create`**  cmdlet toodeploy hello nya VNet med hjälp av hello mallen och parametern filer du hämtade och ändrade ovan. hello-listan som visas efter hello utdata förklarar hello parametrar som används.

    ```azurecli
    azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    Förväntad utdata:
   
        info:    Executing command group create
        info:    Getting resource group TestRG
        info:    Creating resource group TestRG
        info:    Created resource group TestRG
        info:    Initializing template configurations and parameters
        info:    Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    
        info:    group create command OK
   
   * **-n (eller --name)**. Namnet på hello resurs grupp toobe skapas.
   * **-l (eller --location)**. Azure-region där resursgruppen hello kommer att skapas.
   * **-f (eller --template-file)**. Sökvägen tooyour ARM-mallfil.
   * **-e (eller --parameters-file)**. Sökvägen tooyour ARM-parameterfil.

