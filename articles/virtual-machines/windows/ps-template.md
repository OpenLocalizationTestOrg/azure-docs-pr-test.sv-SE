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
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a><span data-ttu-id="21dca-103">Skapa en virtuell Windows-dator från en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="21dca-103">Create a Windows virtual machine from a Resource Manager template</span></span>

<span data-ttu-id="21dca-104">Den här artikeln beskrivs hur du toodeploy en Azure Resource Manager mallen med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="21dca-104">This article shows you how toodeploy an Azure Resource Manager template using PowerShell.</span></span> <span data-ttu-id="21dca-105">hello-mall som du skapar distribuerar en enskild virtuell dator som kör Windows Server i ett nytt virtuellt nätverk med ett enda undernät.</span><span class="sxs-lookup"><span data-stu-id="21dca-105">hello template that you create deploys a single virtual machine running Windows Server in a new virtual network with a single subnet.</span></span>

<span data-ttu-id="21dca-106">En detaljerad beskrivning av hello virtuell datorresurs finns [virtuella datorer i en Azure Resource Manager-mall](template-description.md).</span><span class="sxs-lookup"><span data-stu-id="21dca-106">For a detailed description of hello virtual machine resource, see [Virtual machines in an Azure Resource Manager template](template-description.md).</span></span> <span data-ttu-id="21dca-107">Mer information om alla hello resurser i en mall finns [genomgång av Azure Resource Manager-mall](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="21dca-107">For more information about all hello resources in a template, see [Azure Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="21dca-108">Det bör ta ungefär fem minuter toodo hello stegen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="21dca-108">It should take about five minutes toodo hello steps in this article.</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="21dca-109">Installera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="21dca-109">Install Azure PowerShell</span></span>

<span data-ttu-id="21dca-110">Se [hur tooinstall och konfigurera Azure PowerShell](../../powershell-install-configure.md) information om installation hello senaste versionen av Azure PowerShell, välja din prenumeration och loggar in tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="21dca-110">See [How tooinstall and configure Azure PowerShell](../../powershell-install-configure.md) for information about installing hello latest version of Azure PowerShell, selecting your subscription, and signing in tooyour account.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="21dca-111">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="21dca-111">Create a resource group</span></span>

<span data-ttu-id="21dca-112">Alla resurser måste distribueras i en [resursgruppen](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="21dca-112">All resources must be deployed in a [resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

1. <span data-ttu-id="21dca-113">Hämta en lista över tillgängliga platser där resurser kan skapas.</span><span class="sxs-lookup"><span data-stu-id="21dca-113">Get a list of available locations where resources can be created.</span></span>
   
    ```powershell   
    Get-AzureRmLocation | sort DisplayName | Select DisplayName
    ```

2. <span data-ttu-id="21dca-114">Skapa hello resursgrupp hello-plats som du väljer.</span><span class="sxs-lookup"><span data-stu-id="21dca-114">Create hello resource group in hello location that you select.</span></span> <span data-ttu-id="21dca-115">Det här exemplet visar hello skapandet av en resursgrupp med namnet **myResourceGroup** i hello **västra USA** plats:</span><span class="sxs-lookup"><span data-stu-id="21dca-115">This example shows hello creation of a resource group named **myResourceGroup** in hello **West US** location:</span></span>

    ```powershell   
    New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
    ```

## <a name="create-hello-files"></a><span data-ttu-id="21dca-116">Skapa hello-filer</span><span class="sxs-lookup"><span data-stu-id="21dca-116">Create hello files</span></span>

<span data-ttu-id="21dca-117">I det här steget skapar du en mallfil som distribuerar hello resurser och en fil med parametrar som tillhandahåller parametern värden toohello mall.</span><span class="sxs-lookup"><span data-stu-id="21dca-117">In this step, you create a template file that deploys hello resources and a parameters file that supplies parameter values toohello template.</span></span> <span data-ttu-id="21dca-118">Du kan också skapa en auktoriseringsfil som har använt tooperform Azure Resource Manager-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="21dca-118">You also create an authorization file that is used tooperform Azure Resource Manager operations.</span></span>

1. <span data-ttu-id="21dca-119">Skapa en fil med namnet *CreateVMTemplate.json* och Lägg till den här tooit för JSON-kod:</span><span class="sxs-lookup"><span data-stu-id="21dca-119">Create a file named *CreateVMTemplate.json* and add this JSON code tooit:</span></span>

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

2. <span data-ttu-id="21dca-120">Skapa en fil med namnet *Parameters.json* och Lägg till den här tooit för JSON-kod:</span><span class="sxs-lookup"><span data-stu-id="21dca-120">Create a file named *Parameters.json* and add this JSON code tooit:</span></span>

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

3. <span data-ttu-id="21dca-121">Skapa ett nytt lagringskonto och en behållare:</span><span class="sxs-lookup"><span data-stu-id="21dca-121">Create a new storage account and container:</span></span>

    ```powershell
    $storageName = "st" + (Get-Random)
    New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" -AccountName $storageName -Location "West US" -SkuName "Standard_LRS" -Kind Storage
    $accountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name $storageName).Value[0]
    $context = New-AzureStorageContext -StorageAccountName $storageName -StorageAccountKey $accountKey 
    New-AzureStorageContainer -Name "templates" -Context $context -Permission Container
    ```

4. <span data-ttu-id="21dca-122">Överför hello filer toohello storage-konto:</span><span class="sxs-lookup"><span data-stu-id="21dca-122">Upload hello files toohello storage account:</span></span>

    ```powershell
    Set-AzureStorageBlobContent -File "C:\templates\CreateVMTemplate.json" -Context $context -Container "templates"
    Set-AzureStorageBlobContent -File "C:\templates\Parameters.json" -Context $context -Container templates
    ```

    <span data-ttu-id="21dca-123">Ändra hello - sökvägar toohello plats där du sparade hello-filer.</span><span class="sxs-lookup"><span data-stu-id="21dca-123">Change hello -File paths toohello location where you stored hello files.</span></span>

## <a name="create-hello-resources"></a><span data-ttu-id="21dca-124">Skapa hello resurser</span><span class="sxs-lookup"><span data-stu-id="21dca-124">Create hello resources</span></span>

<span data-ttu-id="21dca-125">Distribuera hello mallen med hjälp av hello parametrar:</span><span class="sxs-lookup"><span data-stu-id="21dca-125">Deploy hello template using hello parameters:</span></span>

```powershell
$templatePath = "https://" + $storageName + ".blob.core.windows.net/templates/CreateVMTemplate.json"
$parametersPath = "https://" + $storageName + ".blob.core.windows.net/templates/Parameters.json"
New-AzureRmResourceGroupDeployment -ResourceGroupName "myResourceGroup" -Name "myDeployment" -TemplateUri $templatePath -TemplateParameterUri $parametersPath 
```

> [!NOTE]
> <span data-ttu-id="21dca-126">Du kan också distribuera parametrarna från lokala filer och mallar.</span><span class="sxs-lookup"><span data-stu-id="21dca-126">You can also deploy templates and parameters from local files.</span></span> <span data-ttu-id="21dca-127">Det finns fler toolearn [med hjälp av Azure PowerShell med Azure Storage](../../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="21dca-127">toolearn more, see [Using Azure PowerShell with Azure Storage](../../storage/common/storage-powershell-guide-full.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="21dca-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="21dca-128">Next Steps</span></span>

- <span data-ttu-id="21dca-129">Om det fanns problem med hello distribution, kan du ta en titt på [felsöka vanliga Azure-distribution med Azure Resource Manager](../../resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="21dca-129">If there were issues with hello deployment, you might take a look at [Troubleshoot common Azure deployment errors with Azure Resource Manager](../../resource-manager-common-deployment-errors.md).</span></span>
- <span data-ttu-id="21dca-130">Lär dig hur toocreate och hantera en virtuell dator i [skapa och hantera virtuella Windows-datorer med hello Azure PowerShell-modulen](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="21dca-130">Learn how toocreate and manage a virtual machine in [Create and manage Windows VMs with hello Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

