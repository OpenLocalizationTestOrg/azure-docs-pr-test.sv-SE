---
title: "aaaCreate en virtuell dator med flera nätverkskort – Azure Resource Manager-mall | Microsoft Docs"
description: "Skapa en virtuell dator med flera nätverkskort med en Azure Resource Manager-mall."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 486f7dd5-cf2f-434c-85d1-b3e85c427def
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f5d9ffcbd40c72dc18ae6de38e739eb5e45bf669
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-a-template"></a>Skapa en virtuell dator med flera nätverkskort med en mall
[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).  Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](virtual-network-deploy-multinic-classic-ps.md).
> 

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

hello så här använder du en resursgrupp med namnet *IaaSStory* för hello webbservrar och en resursgrupp med namnet *IaaSStory BackEnd* för hello DB-servrar.

## <a name="prerequisites"></a>Krav
Innan du kan skapa hello DB-servrar, behöver du toocreate hello *IaaSStory* resursgrupp med alla hello nödvändiga resurser för det här scenariot. toocreate dessa resurser, Slutför hello följande steg:

1. Navigera för[hello mallsidan](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Hello mallen sidan toohello höger i **överordnade resursgruppen**, klickar du på **distribuera tooAzure**.
3. Om det behövs, ändra hello parametervärden sedan gör hello i hello Azure preview portal toodeploy hello resursgruppen.

> [!IMPORTANT]
> Kontrollera att din lagringskontonamn är unika. Du kan inte ha dubbla lagringskontonamn i Azure.
> 

## <a name="understand-hello-deployment-template"></a>Förstå hello Distributionsmall
Innan du distribuerar den här dokumentationen hello-mall, kontrollera att du förstår hur det fungerar. hello följande ger en bra översikt över hello mallen:

1. Navigera för[hello mallsidan](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klicka på **azuredeploy.json** tooopen hello mallfilen.
3. Meddelande hello *osType* parametrar i listan nedan. Den här parametern är används tooselect toouse vilka VM-avbildning för hello databasserver, tillsammans med flera operativsystem relaterade inställningar.

    ```json
    "osType": {
      "type": "string",
      "defaultValue": "Windows",
      "allowedValues": [
        "Windows",
        "Ubuntu"
      ],
      "metadata": {
      "description": "Type of OS toouse for VMs: Windows or Ubuntu."
      }
    },
    ```

4. Bläddra nedåt i toohello lista över variabler och kontrollera hello definition för hello **dbVMSetting** variabler, som anges nedan. Den tar emot en hello matriselement i hello **dbVMSettings** variabeln. Om du är bekant med termer som software development, kan du visa hello **dbVMSettings** variabeln som en hashtabell eller en ordlista.

    ```json
    "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"
    ```

5. Anta att du bestämmer dig för toodeploy Windows virtuella datorer som kör SQL i hello serverdel. Hej värde för **osType** skulle vara *Windows*, och hello **dbVMSetting** variabeln skulle innehålla hello-element som anges nedan, och som motsvarar hello första värdet i hello **dbVMSettings** variabeln.

    ```json
    "Windows": {
      "vmSize": "Standard_DS3",
      "publisher": "MicrosoftSQLServer",
      "offer": "SQL2014SP1-WS2012R2",
      "sku": "Standard",
      "version": "latest",
      "vmName": "DB",
      "osdisk": "osdiskdb",
      "datadisk": "datadiskdb",
      "nicName": "NICDB",
      "ipAddress": "192.168.2.",
      "extensionDeployment": "",
      "avsetName": "ASDB",
      "remotePort": 3389,
      "dbPort": 1433
    },
    ```

6. Meddelande hello **vmSize** innehåller hello värdet *Standard_DS3*. Endast vissa VM-storlekar tillåta hello användning av flera nätverkskort. Du kan kontrollera vilka VM-storlekar stöd för flera nätverkskort genom att läsa hello [Windows VM-storlekar](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) och [Linux VM-storlekar](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artiklar.

7. Rulla nedåt för**resurser** och meddelande hello första elementet. Beskriver ett lagringskonto. Det här lagringskontot kommer att använda toomaintain hello datadiskar som används av varje VM-databas. I det här scenariot har varje databas VM en OS-disk som lagras i reguljära storage och två datadiskar för som lagras i SSD (premium) lagring.

    ```json
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('prmStorageName')]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "Storage Account - Premium"
      },
      "properties": {
      "accountType": "[parameters('prmStorageType')]"
      }
    },
    ```

8. Rulla ned toohello nästa resurs, enligt nedan. Denna resurs representerar hello nätverkskort används för åtkomst till databasen i varje VM-databasen. Meddelande hello användning av hello **kopiera** funktion i den här resursen. hello mall kan du toodeploy många virtuella datorer som du vill, som baseras på hello **dbCount** parameter. Du måste därför toocreate hello samma mängd nätverkskort för åtkomst till databasen, en för varje virtuell dator.

    ```json
    {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
    "location": "[variables('location')]",
    "tags": {
      "displayName": "NetworkInterfaces - DB DA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "privateIPAllocationMethod": "Static",
            "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
            "subnet": {
              "id": "[variables('backEndSubnetRef')]"
            }
          }
         }
       ] 
     }
    },
    ```

9. Rulla ned toohello nästa resurs, enligt nedan. Denna resurs representerar hello nätverkskort används för hantering i varje databas VM. Återigen behöver du något av dessa nätverkskort för varje databas VM. Meddelande hello **networkSecurityGroup** element, länka en NSG som tillåter åtkomst tooRDP/SSH toothis NIC endast.

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "NetworkInterfaces - DB RA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "networkSecurityGroup": {
             "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
             },
             "privateIPAllocationMethod": "Static",
             "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
             "subnet": {
              "id": "[variables('backEndSubnetRef')]"
             }
           }
          }
        ]
      }
    },
```

10. Rulla ned toohello nästa resurs, enligt nedan. Denna resurs representerar en tillgänglighet set toobe som delas av alla virtuella datorer som databas. På så sätt kan du garantera att det alltid är en virtuell dator i hello ange körs under underhåll.

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/availabilitySets",
      "name": "[variables('dbVMSetting').avsetName]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "AvailabilitySet - DB"
      }
    },
    ```

11. Rulla ned toohello nästa resurs. Denna resurs representerar hello databasen virtuella datorer, som visas först i hello raderna nedan. Meddelande hello användning av hello **kopiera** fungera igen, se till att flera virtuella datorer skapas baserat på hello **dbCount** parameter. Observera också hello **dependsOn** samling. Visas en lista med två nätverkskort som behövs toobe som skapats före hello VM distribueras tillsammans med hello tillgänglighetsuppsättning och hello storage-konto.

    ```json
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
    "location": "[variables('location')]",
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
      "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
      "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
    ],
    "tags": {
      "displayName": "VMs - DB"
    },
    "copy": {
      "name": "dbvmcount",
      "count": "[parameters('dbCount')]"
    },
    ```

12. Rulla nedåt i hello VM resurs toohello **networkProfile** element, enligt nedan. Observera att det finns två nätverkskort som referens för varje virtuell dator. När du skapar flera nätverkskort för en virtuell dator måste du ange hello **primära** -egenskapen för en av hello nätverkskort för*SANT*, och hello rest för*FALSKT*.

    ```json
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
          "properties": { "primary": true }
        },
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
          "properties": { "primary": false }
        }
      ]
    }
    ```

## <a name="deploy-hello-arm-template-by-using-click-toodeploy"></a>Distribuera hello ARM-mallen med hjälp av klickar du på toodeploy

> [!IMPORTANT]
> Se till att du följer hello [förutsättningar](#Pre-requisites) steg innan du följer hello anvisningarna nedan.
> 

hello exempelmall tillgängliga i hello offentliga databasen använder en parameterfil som innehåller hello standard värden som används för toogenerate hello scenario som beskrivs ovan. toodeploy den här mallen med hjälp av klickar du på toodeploy, Följ [länken](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), toohello höger i **Backend resursgrupp (se dokumentationen)** klickar du på **distribuera tooAzure**, Ersätt Hej Standardparametervärden om det behövs, och följ anvisningarna för hello hello-portalen.

hello bilden nedan visar hello innehållet i hello ny resursgrupp efter distributionen.

![Backend-resursgrupp](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-hello-template-by-using-powershell"></a>Distribuera hello-mallen med hjälp av PowerShell
toodeploy hello mallen som du hämtade med PowerShell, installerar och konfigurerar PowerShell genom att slutföra hello stegen i hello [installerar och konfigurerar PowerShell](/powershell/azure/overview) artikel och slutför sedan hello följande steg:

Kör hello  **`New-AzureRmResourceGroup`**  cmdlet toocreate en resurs med hello mallen.

```powershell
New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
-TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'
```

Förväntad utdata:

    ResourceGroupName : IaaSStory-Backend
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions  NotActions
                        =======  ==========
                        *
        Resources         :
                        Name                 Type                                 Location
                        ===================  ===================================  ========
                        ASDB                 Microsoft.Compute/availabilitySets   westus  
                        DB1                  Microsoft.Compute/virtualMachines    westus  
                        DB2                  Microsoft.Compute/virtualMachines    westus  
                        NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                        wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  
    ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a>Distribuera hello mallen med hello Azure CLI
toodeploy hello mall med hjälp av hello Azure CLI, följ hello stegen nedan.

1. Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.
2. Kör hello  **`azure config mode`**  kommandot tooswitch tooResource Manager-läge enligt nedan.

    ```azurecli
    azure config mode arm
    ```

    hello förväntas följande utdata:

        info:    New mode is arm

3. Öppna hello [parameterfilen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), Välj dess innehåll och spara den tooa filen på datorn. Det här exemplet, vi sparat hello parameterfilen för*parameters.json*.
4. Kör hello  **`azure group deployment create`**  cmdlet toodeploy hello nya VNet med hjälp av hello mallen och parametern filer du hämtade och ändrade ovan. hello-listan som visas efter hello utdata förklarar hello parametrar som används.

    ```azurecli
    azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json
    ```

    Förväntad utdata:
   
        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

