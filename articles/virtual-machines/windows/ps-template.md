---
title: "Skapa en virtuell Windows-dator från en mall i Azure | Microsoft Docs"
description: "Använd Resource Manager-mall och PowerShell för att skapa en ny Windows virtuell dator."
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
ms.openlocfilehash: ddab80262fe27c1f5995858ec7de75d7c46df081
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a><span data-ttu-id="f9fb2-103">Skapa en virtuell Windows-dator från en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="f9fb2-103">Create a Windows virtual machine from a Resource Manager template</span></span>

<span data-ttu-id="f9fb2-104">Den här artikeln visar hur du distribuerar en Azure Resource Manager-mallen med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f9fb2-104">This article shows you how to deploy an Azure Resource Manager template using PowerShell.</span></span> <span data-ttu-id="f9fb2-105">Den mall som du skapar distribuerar en enskild virtuell dator som kör Windows Server i ett nytt virtuellt nätverk med ett enda undernät.</span><span class="sxs-lookup"><span data-stu-id="f9fb2-105">The template that you create deploys a single virtual machine running Windows Server in a new virtual network with a single subnet.</span></span>

<span data-ttu-id="f9fb2-106">En detaljerad beskrivning av den virtuella datorresursen finns [virtuella datorer i en Azure Resource Manager-mall](template-description.md).</span><span class="sxs-lookup"><span data-stu-id="f9fb2-106">For a detailed description of the virtual machine resource, see [Virtual machines in an Azure Resource Manager template](template-description.md).</span></span> <span data-ttu-id="f9fb2-107">Mer information om alla resurser i en mall finns [genomgång av Azure Resource Manager-mall](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="f9fb2-107">For more information about all the resources in a template, see [Azure Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="f9fb2-108">Det bör ta ungefär fem minuter för att utföra stegen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f9fb2-108">It should take about five minutes to do the steps in this article.</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="f9fb2-109">Installera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f9fb2-109">Install Azure PowerShell</span></span>

<span data-ttu-id="f9fb2-110">Se [Installera och konfigurera Azure PowerShell](../../powershell-install-configure.md) för information om hur du installerar den senaste versionen av Azure PowerShell, väljer din prenumeration och loggar in på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="f9fb2-110">See [How to install and configure Azure PowerShell](../../powershell-install-configure.md) for information about installing the latest version of Azure PowerShell, selecting your subscription, and signing in to your account.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="f9fb2-111">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="f9fb2-111">Create a resource group</span></span>

<span data-ttu-id="f9fb2-112">Alla resurser måste distribueras i en [resursgruppen](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f9fb2-112">All resources must be deployed in a [resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

1. <span data-ttu-id="f9fb2-113">Hämta en lista över tillgängliga platser där resurser kan skapas.</span><span class="sxs-lookup"><span data-stu-id="f9fb2-113">Get a list of available locations where resources can be created.</span></span>
   
    ```powershell   
    Get-AzureRmLocation | sort DisplayName | Select DisplayName
    ```

2. <span data-ttu-id="f9fb2-114">Skapa resursgruppen på den plats som du väljer.</span><span class="sxs-lookup"><span data-stu-id="f9fb2-114">Create the resource group in the location that you select.</span></span> <span data-ttu-id="f9fb2-115">Det här exemplet visar skapandet av en resursgrupp med namnet **myResourceGroup** i den **västra USA** plats:</span><span class="sxs-lookup"><span data-stu-id="f9fb2-115">This example shows the creation of a resource group named **myResourceGroup** in the **West US** location:</span></span>

    ```powershell   
    New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
    ```

## <a name="create-the-files"></a><span data-ttu-id="f9fb2-116">Skapa filer</span><span class="sxs-lookup"><span data-stu-id="f9fb2-116">Create the files</span></span>

<span data-ttu-id="f9fb2-117">I det här steget skapar du en mallfil som distribuerar resurser och en fil med parametrar som tillhandahåller parametervärden för mallen.</span><span class="sxs-lookup"><span data-stu-id="f9fb2-117">In this step, you create a template file that deploys the resources and a parameters file that supplies parameter values to the template.</span></span> <span data-ttu-id="f9fb2-118">Du kan också skapa en auktoriseringsfil som används för att utföra åtgärder på Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f9fb2-118">You also create an authorization file that is used to perform Azure Resource Manager operations.</span></span>

1. <span data-ttu-id="f9fb2-119">Skapa en fil med namnet *CreateVMTemplate.json* och lägga till den här JSON-kod:</span><span class="sxs-lookup"><span data-stu-id="f9fb2-119">Create a file named *CreateVMTemplate.json* and add this JSON code to it:</span></span>

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

2. <span data-ttu-id="f9fb2-120">Skapa en fil med namnet *Parameters.json* och lägga till den här JSON-kod:</span><span class="sxs-lookup"><span data-stu-id="f9fb2-120">Create a file named *Parameters.json* and add this JSON code to it:</span></span>

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

3. <span data-ttu-id="f9fb2-121">Skapa ett nytt lagringskonto och en behållare:</span><span class="sxs-lookup"><span data-stu-id="f9fb2-121">Create a new storage account and container:</span></span>

    ```powershell
    $storageName = "st" + (Get-Random)
    New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" -AccountName $storageName -Location "West US" -SkuName "Standard_LRS" -Kind Storage
    $accountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name $storageName).Value[0]
    $context = New-AzureStorageContext -StorageAccountName $storageName -StorageAccountKey $accountKey 
    New-AzureStorageContainer -Name "templates" -Context $context -Permission Container
    ```

4. <span data-ttu-id="f9fb2-122">Ladda upp filer till lagringskontot:</span><span class="sxs-lookup"><span data-stu-id="f9fb2-122">Upload the files to the storage account:</span></span>

    ```powershell
    Set-AzureStorageBlobContent -File "C:\templates\CreateVMTemplate.json" -Context $context -Container "templates"
    Set-AzureStorageBlobContent -File "C:\templates\Parameters.json" -Context $context -Container templates
    ```

    <span data-ttu-id="f9fb2-123">Ändra - sökvägarna till den plats där du lagrade filerna.</span><span class="sxs-lookup"><span data-stu-id="f9fb2-123">Change the -File paths to the location where you stored the files.</span></span>

## <a name="create-the-resources"></a><span data-ttu-id="f9fb2-124">Skapa resurser</span><span class="sxs-lookup"><span data-stu-id="f9fb2-124">Create the resources</span></span>

<span data-ttu-id="f9fb2-125">Distribuera mallen med hjälp av parametrar:</span><span class="sxs-lookup"><span data-stu-id="f9fb2-125">Deploy the template using the parameters:</span></span>

```powershell
$templatePath = "https://" + $storageName + ".blob.core.windows.net/templates/CreateVMTemplate.json"
$parametersPath = "https://" + $storageName + ".blob.core.windows.net/templates/Parameters.json"
New-AzureRmResourceGroupDeployment -ResourceGroupName "myResourceGroup" -Name "myDeployment" -TemplateUri $templatePath -TemplateParameterUri $parametersPath 
```

> [!NOTE]
> <span data-ttu-id="f9fb2-126">Du kan också distribuera parametrarna från lokala filer och mallar.</span><span class="sxs-lookup"><span data-stu-id="f9fb2-126">You can also deploy templates and parameters from local files.</span></span> <span data-ttu-id="f9fb2-127">Läs mer i [med hjälp av Azure PowerShell med Azure Storage](../../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="f9fb2-127">To learn more, see [Using Azure PowerShell with Azure Storage](../../storage/common/storage-powershell-guide-full.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f9fb2-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f9fb2-128">Next Steps</span></span>

- <span data-ttu-id="f9fb2-129">Om det fanns problem med distributionen, kan du ta en titt på [felsöka vanliga Azure-distribution med Azure Resource Manager](../../resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="f9fb2-129">If there were issues with the deployment, you might take a look at [Troubleshoot common Azure deployment errors with Azure Resource Manager](../../resource-manager-common-deployment-errors.md).</span></span>
- <span data-ttu-id="f9fb2-130">Lär dig att skapa och hantera en virtuell dator i [skapa och hantera virtuella Windows-datorer med Azure PowerShell-modulen](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f9fb2-130">Learn how to create and manage a virtual machine in [Create and manage Windows VMs with the Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

