---
title: aaaCreate en virtuell dator med en statisk offentlig IP-adress - Azure Resource Manager-mall | Microsoft Docs
description: "Lär dig hur toocreate en virtuell dator med en statisk offentlig IP-adress med en Azure Resource Manager-mall."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d551085a-c7ed-4ec6-b4c3-e9e1cebb774c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6a8640ed4fad06b0e09820e6114fd6789db73847
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-an-azure-resource-manager-template"></a>Skapa en virtuell dator med en statisk offentlig IP-adress med en Azure Resource Manager-mall

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Mall](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klassisk)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md). Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello klassiska distributionsmodellen.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-address-resources-in-a-template-file"></a>Offentliga IP-adress-resurser i en mallfil
Du kan visa och hämta hello [exempelmall](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

hello visar följande avsnitt hello definition av hello offentliga IP-resurs, baserat på hello scenariot ovan:

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('webVMSetting').pipName]",
  "location": "[variables('location')]",
  "properties": {
    "publicIPAllocationMethod": "Static"
  },
  "tags": {
    "displayName": "PublicIPAddress - Web"
  }
},
```

Meddelande hello **publicIPAllocationMethod** -egenskap som har angetts för*statiska*. Den här egenskapen kan vara antingen *dynamiska* (standardvärdet) eller *statiska*. Ange värdet toostatic garanterar att hello offentliga IP-adress ska aldrig ändras.

hello visar följande avsnitt hello associering av hello offentlig IP-adress med ett nätverksgränssnitt:

```json
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('webVMSetting').nicName]",
    "location": "[variables('location')]",
    "tags": {
    "displayName": "NetworkInterface - Web"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
      "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
    ],
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
          "privateIPAllocationMethod": "Static",
          "privateIPAddress": "[variables('webVMSetting').ipAddress]",
          "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
          },
          "subnet": {
            "id": "[variables('frontEndSubnetRef')]"
          }
        }
      }
    ]
  }
},
```

Meddelande hello **publicIPAddress** egenskapen pekar toohello **Id** för en resurs med namnet **variables('webVMSetting').pipName**. Som är hello namn hello offentliga IP-resurs som visas ovan.

Slutligen hello nätverksgränssnittet ovan visas i hello **networkProfile** -egenskapen för hello VM håller på att skapas.

```json
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }
```

## <a name="deploy-hello-template-by-using-click-toodeploy"></a>Distribuera hello mallen med Klicka toodeploy

hello exempelmall tillgängliga i hello offentliga databasen använder en parameterfil som innehåller hello standard värden som används för toogenerate hello scenario som beskrivs ovan. toodeploy den här mallen med hjälp av klickar du på toodeploy, klickar du på **distribuera tooAzure** i hello Readme.md-filen för hello [virtuell dator med statiska PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) mall. Ersätt hello Standardparametervärden om så önskas och ange värden för hello tomt parametrar.  Följ instruktionerna för hello i hello portal toocreate en virtuell dator med en statisk offentlig IP-adress.

## <a name="deploy-hello-template-by-using-powershell"></a>Distribuera hello-mallen med hjälp av PowerShell

toodeploy hello mallen som du hämtade med PowerShell, följ hello stegen nedan.

1. Om du aldrig har använt Azure PowerShell fullständig hello stegen i hello [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.
2. I PowerShell-konsol kör hello `New-AzureRmResourceGroup` cmdlet toocreate en ny resursgrupp om det behövs. Om du redan har en resursgrupp som skapats går toostep 3.

    ```powershell
    New-AzureRmResourceGroup -Name PIPTEST -Location westus
    ```

    Förväntad utdata:
   
        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. I PowerShell-konsol kör hello `New-AzureRmResourceGroupDeployment` cmdlet toodeploy hello mallen.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
        -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
        -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json
    ```

    Förväntad utdata:
   
        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0
   
        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         
   
        Outputs           :

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a>Distribuera hello mallen med hello Azure CLI
toodeploy hello mall med hjälp av hello Azure CLI, fullständig hello följande steg:

1. Om du aldrig har använt Azure CLI åtgärderna hello i hello [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) artikel tooinstall och konfigurera den.
2. Kör hello `azure config mode` kommandot tooswitch tooResource Manager-läge enligt nedan.

    ```azurecli
    azure config mode arm
    ```

    hello förväntas för hello kommandot ovan:

        info:    New mode is arm

3. Öppna hello [parameterfilen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), Välj dess innehåll och spara den tooa filen på datorn. I det här exemplet hello parametrar sparas tooa fil med namnet *parameters.json*. Ändra hello parametervärden i hello-filen om du vill, men minst bör du ändra hello värde för hello adminPassword parametern tooa unika, komplexa lösenord.
4. Kör hello `azure group deployment create` cmd toodeploy hello nya VNet med hjälp av hello mallen och parametern filer du hämtade och ändrade ovan. Ersätt i hello kommandot nedan <path> med hello sökvägen som du sparade hello-filen till. 

    ```azurecli
    azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json
    ```

    Förväntad utdata (visar parametervärden används):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

