---
title: "aaaCreate en virtuell Windows-dator från en mall i Azure | Microsoft Docs"
description: "Använd en mall för Resource Manager och PowerShell tooeasily skapa en ny Windows virtuell dator."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 19129d61-8c04-4aa9-a01f-361a09466805
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: davidmu
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 630111482c7dc046091632e2ed458ac143325d59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a>Skapa en virtuell Windows-dator från en Resource Manager-mall

Den här artikeln beskrivs hur du toodeploy en Azure Resource Manager mallen med hjälp av PowerShell. hello-mall som du skapar distribuerar en enskild virtuell dator som kör Windows Server i ett nytt virtuellt nätverk med ett enda undernät.

En detaljerad beskrivning av hello virtuell datorresurs finns [virtuella datorer i en Azure Resource Manager-mall](template-description.md). Mer information om alla hello resurser i en mall finns [genomgång av Azure Resource Manager-mall](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Det bör ta ungefär fem minuter toodo hello stegen i den här artikeln.

## <a name="install-azure-powershell"></a>Installera Azure PowerShell

Se [hur tooinstall och konfigurera Azure PowerShell](../../powershell-install-configure.md) information om installation hello senaste versionen av Azure PowerShell, välja din prenumeration och loggar in tooyour konto.

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Alla resurser måste distribueras i en [resursgruppen](../../azure-resource-manager/resource-group-overview.md).

1. Hämta en lista över tillgängliga platser där resurser kan skapas.
   
    ```powershell   
    Get-AzureRmLocation | sort DisplayName | Select DisplayName
    ```

2. Skapa hello resursgrupp hello-plats som du väljer. Det här exemplet visar hello skapandet av en resursgrupp med namnet **myResourceGroup** i hello **västra USA** plats:

    ```powershell   
    New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
    ```

## <a name="create-hello-files"></a>Skapa hello-filer

I det här steget skapar du en mallfil som distribuerar hello resurser och en fil med parametrar som tillhandahåller parametern värden toohello mall. Du kan också skapa en auktoriseringsfil som har använt tooperform Azure Resource Manager-åtgärder.

1. Skapa en fil med namnet *CreateVMTemplate.json* och Lägg till den här tooit för JSON-kod:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" }
      },
      "variables": {
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks','myVNet')]", 
        "subnetRef": "[concat(variables('vnetID'),'/subnets/mySubnet')]", 
      },
      "resources": [
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "myPublicIPAddress",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "myresourcegroupdns1"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "myVNet",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "mySubnet",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "myNic",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/publicIPAddresses/', 'myPublicIPAddress')]",
            "[resourceId('Microsoft.Network/virtualNetworks/', 'myVNet')]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": { "id": "[resourceId('Microsoft.Network/publicIPAddresses','myPublicIPAddress')]" },
                  "subnet": { "id": "[variables('subnetRef')]" }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-04-30-preview",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "myVM",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkInterfaces/', 'myNic')]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_DS1" },
            "osProfile": {
              "computerName": "myVM",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "myManagedOSDisk",
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces','myNic')]"
                }
              ]
            }
          }
        }
      ]
    }
    ```

2. Skapa en fil med namnet *Parameters.json* och Lägg till den här tooit för JSON-kod:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      "adminUserName": { "value": "azureuser" },
        "adminPassword": { "value": "Azure12345678" }
      }
    }
    ```

3. Skapa ett nytt lagringskonto och en behållare:

    ```powershell
    $storageName = "st" + (Get-Random)
    New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" -AccountName $storageName -Location "West US" -SkuName "Standard_LRS" -Kind Storage
    $accountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name $storageName).Value[0]
    $context = New-AzureStorageContext -StorageAccountName $storageName -StorageAccountKey $accountKey 
    New-AzureStorageContainer -Name "templates" -Context $context -Permission Container
    ```

4. Överför hello filer toohello storage-konto:

    ```powershell
    Set-AzureStorageBlobContent -File "C:\templates\CreateVMTemplate.json" -Context $context -Container "templates"
    Set-AzureStorageBlobContent -File "C:\templates\Parameters.json" -Context $context -Container templates
    ```

    Ändra hello - sökvägar toohello plats där du sparade hello-filer.

## <a name="create-hello-resources"></a>Skapa hello resurser

Distribuera hello mallen med hjälp av hello parametrar:

```powershell
$templatePath = "https://" + $storageName + ".blob.core.windows.net/templates/CreateVMTemplate.json"
$parametersPath = "https://" + $storageName + ".blob.core.windows.net/templates/Parameters.json"
New-AzureRmResourceGroupDeployment -ResourceGroupName "myResourceGroup" -Name "myDeployment" -TemplateUri $templatePath -TemplateParameterUri $parametersPath 
```

> [!NOTE]
> Du kan också distribuera parametrarna från lokala filer och mallar. Det finns fler toolearn [med hjälp av Azure PowerShell med Azure Storage](../../storage/common/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Nästa steg

- Om det fanns problem med hello distribution, kan du ta en titt på [felsöka vanliga Azure-distribution med Azure Resource Manager](../../resource-manager-common-deployment-errors.md).
- Lär dig hur toocreate och hantera en virtuell dator i [skapa och hantera virtuella Windows-datorer med hello Azure PowerShell-modulen](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

