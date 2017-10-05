---
title: "Autoskala Windows virtuella Skalningsuppsättningarna | Microsoft Docs"
description: "Ställa in autoskalning för en Windows Skalningsuppsättning virtuell dator med hjälp av Azure PowerShell"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: 91f731bec46c005221f4e66e95822994070d4c26
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="a8bfd-103">Skala automatiskt datorer i en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a8bfd-103">Automatically scale machines in a virtual machine scale set</span></span>
<span data-ttu-id="a8bfd-104">Skaluppsättningar för den virtuella datorn gör det enkelt att distribuera och hantera identiska virtuella datorer som en uppsättning.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-104">Virtual machine scale sets make it easy for you to deploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="a8bfd-105">Skaluppsättningar ger en skalbar och anpassningsbara beräkningslager för storskaliga program och stöd för avbildningar för Windows-plattformen, Linux-plattformen bilder, anpassade avbildningar och tillägg.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="a8bfd-106">Läs mer om skaluppsättningar [Skalningsuppsättningarna för virtuella](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a8bfd-106">For more information about scale sets, see [Virtual Machine Scale Sets](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="a8bfd-107">Den här kursen visar hur du skapar en skaluppsättning för Windows-datorer och skala automatiskt på datorer i uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-107">This tutorial shows you how to create a scale set of Windows virtual machines and automatically scale the machines in the set.</span></span> <span data-ttu-id="a8bfd-108">Du kan skapa skala och skalning genom att skapa en Azure Resource Manager-mall och distribuera den med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-108">You create the scale set and set up scaling by creating an Azure Resource Manager template and deploying it using Azure PowerShell.</span></span> <span data-ttu-id="a8bfd-109">Mer information om mallar finns i [Redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a8bfd-109">For more information about templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="a8bfd-110">Läs mer om automatisk skalning av skalningsuppsättningar i [automatisk skalning och Skalningsuppsättningarna för virtuella](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a8bfd-110">To learn more about automatic scaling of scale sets, see [Automatic scaling and Virtual Machine Scale Sets](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

<span data-ttu-id="a8bfd-111">I den här artikeln får distribuera du följande resurser och tillägg:</span><span class="sxs-lookup"><span data-stu-id="a8bfd-111">In this article, you deploy the following resources and extensions:</span></span>

* <span data-ttu-id="a8bfd-112">Microsoft.Storage/storageAccounts</span><span class="sxs-lookup"><span data-stu-id="a8bfd-112">Microsoft.Storage/storageAccounts</span></span>
* <span data-ttu-id="a8bfd-113">Microsoft.Network/virtualNetworks</span><span class="sxs-lookup"><span data-stu-id="a8bfd-113">Microsoft.Network/virtualNetworks</span></span>
* <span data-ttu-id="a8bfd-114">Microsoft.Network/publicIPAddresses</span><span class="sxs-lookup"><span data-stu-id="a8bfd-114">Microsoft.Network/publicIPAddresses</span></span>
* <span data-ttu-id="a8bfd-115">Microsoft.Network/loadBalancers</span><span class="sxs-lookup"><span data-stu-id="a8bfd-115">Microsoft.Network/loadBalancers</span></span>
* <span data-ttu-id="a8bfd-116">Microsoft.Network/networkInterfaces</span><span class="sxs-lookup"><span data-stu-id="a8bfd-116">Microsoft.Network/networkInterfaces</span></span>
* <span data-ttu-id="a8bfd-117">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="a8bfd-117">Microsoft.Compute/virtualMachines</span></span>
* <span data-ttu-id="a8bfd-118">Microsoft.Compute/virtualMachineScaleSets</span><span class="sxs-lookup"><span data-stu-id="a8bfd-118">Microsoft.Compute/virtualMachineScaleSets</span></span>
* <span data-ttu-id="a8bfd-119">Microsoft.Insights.VMDiagnosticsSettings</span><span class="sxs-lookup"><span data-stu-id="a8bfd-119">Microsoft.Insights.VMDiagnosticsSettings</span></span>
* <span data-ttu-id="a8bfd-120">Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="a8bfd-120">Microsoft.Insights/autoscaleSettings</span></span>

<span data-ttu-id="a8bfd-121">Mer information om Resource Manager-resurser finns [Azure Resource Manager och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a8bfd-121">For more information about Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

## <a name="step-1-install-azure-powershell"></a><span data-ttu-id="a8bfd-122">Steg 1: Installera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a8bfd-122">Step 1: Install Azure PowerShell</span></span>
<span data-ttu-id="a8bfd-123">Se [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview) information om hur du installerar den senaste versionen av Azure PowerShell, markerar du din prenumeration och loggar in på Azure.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-123">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for information about installing the latest version of Azure PowerShell, selecting your subscription, and signing in to Azure.</span></span>

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a><span data-ttu-id="a8bfd-124">Steg 2: Skapa en resursgrupp och ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="a8bfd-124">Step 2: Create a resource group and a storage account</span></span>
1. <span data-ttu-id="a8bfd-125">**Skapa en resursgrupp** – alla resurser måste distribueras till en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-125">**Create a resource group** – All resources must be deployed to a resource group.</span></span> <span data-ttu-id="a8bfd-126">Använd [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) att skapa en resursgrupp med namnet **vmsstestrg1**.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-126">Use [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) to create a resource group named **vmsstestrg1**.</span></span>
2. <span data-ttu-id="a8bfd-127">**Skapa ett lagringskonto** – det här lagringskontot sparas om mallen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-127">**Create a storage account** – This storage account is where the template is stored.</span></span> <span data-ttu-id="a8bfd-128">Använd [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) att skapa ett lagringskonto med namnet **vmsstestsa**.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-128">Use [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) to create a storage account named **vmsstestsa**.</span></span>

## <a name="step-3-create-the-template"></a><span data-ttu-id="a8bfd-129">Steg 3: Skapa mallen</span><span class="sxs-lookup"><span data-stu-id="a8bfd-129">Step 3: Create the template</span></span>
<span data-ttu-id="a8bfd-130">En mall för Azure Resource Manager gör det möjligt för dig att distribuera och hantera Azure-resurser tillsammans med hjälp av en JSON-beskrivning av resurser och associerade distributionen parametrar.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-130">An Azure Resource Manager template makes it possible for you to deploy and manage Azure resources together by using a JSON description of the resources and associated deployment parameters.</span></span>

1. <span data-ttu-id="a8bfd-131">Skapa filen C:\VMSSTemplate.json i din favorit-redigeraren och Lägg till den ursprungliga JSON-strukturen för att stödja mallen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-131">In your favorite editor, create the file C:\VMSSTemplate.json and add the initial JSON structure to support the template.</span></span>

    ```json
    {
      "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      },
      "variables": {
      },
      "resources": [
      ]
    }
    ```

2. <span data-ttu-id="a8bfd-132">Parametrar krävs inte alltid, men de ger ett sätt att ange värden när mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-132">Parameters are not always required, but they provide a way to input values when the template is deployed.</span></span> <span data-ttu-id="a8bfd-133">Lägg till de här parametrarna under det överordnade elementet parametrar som du lagt till mallen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-133">Add these parameters under the parameters parent element that you added to the template.</span></span>

    ```json   
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
    * <span data-ttu-id="a8bfd-134">Ange ett namn för den separata virtuella dator som används för att få åtkomst till datorer i skalan.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-134">A name for the separate virtual machine that is used to access the machines in the scale set.</span></span>
    * <span data-ttu-id="a8bfd-135">Namnet på det lagringskonto där mallen ska lagras.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-135">The name of the storage account where the template is stored.</span></span>
    * <span data-ttu-id="a8bfd-136">Ange antalet virtuella datorer för att skapa först i skalan.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-136">The number of virtual machines to initially create in the scale set.</span></span>
    * <span data-ttu-id="a8bfd-137">Namn och lösenord för administratörskontot på de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-137">The name and password of the administrator account on the virtual machines.</span></span>
    * <span data-ttu-id="a8bfd-138">Ange ett namnprefix för de resurser som skapas för att stödja skalningen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-138">A name prefix for the resources that are created to support the scale set.</span></span>

3. <span data-ttu-id="a8bfd-139">Variabler kan användas i en mall för att ange värden som ändras ofta eller värden som måste skapas från en kombination av parametervärden.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-139">Variables can be used in a template to specify values that may change frequently or values that need to be created from a combination of parameter values.</span></span> <span data-ttu-id="a8bfd-140">Lägg till dessa variabler under det överordnade elementet för variabler som du lagt till mallen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-140">Add these variables under the variables parent element that you added to the template.</span></span>

    ```json
    "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
    "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
    "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
    "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
    "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
    "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
    "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
    "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
      "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```
   
    * <span data-ttu-id="a8bfd-141">DNS-namn som används av nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-141">DNS names that are used by the network interfaces.</span></span>

        * <span data-ttu-id="a8bfd-142">IP-adressnamn och prefix för virtuellt nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-142">The IP address names and prefixes for the virtual network and subnets.</span></span>
        * <span data-ttu-id="a8bfd-143">Namn och identifierare för det virtuella nätverket att läsa in belastningsutjämning och nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-143">The names and identifiers of the virtual network, load balancer, and network interfaces.</span></span>
        * <span data-ttu-id="a8bfd-144">Ange lagringskontonamn för konton som är kopplade till datorer i skalan.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-144">Storage account names for the accounts associated with the machines in the scale set.</span></span>
        * <span data-ttu-id="a8bfd-145">Inställningar för diagnostik-tillägg som är installerad på de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-145">Settings for the Diagnostics extension that is installed on the virtual machines.</span></span> <span data-ttu-id="a8bfd-146">Läs mer om diagnostik [skapa en Windows virtuell dator med övervakning och diagnostik med Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a8bfd-146">For more information about the Diagnostics extension, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

4. <span data-ttu-id="a8bfd-147">Lägg till konto lagringsresurs under det överordnade elementet för resurser som du lagt till mallen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-147">Add the storage account resource under the resources parent element that you added to the template.</span></span> <span data-ttu-id="a8bfd-148">Den här mallen använder en loop för att skapa de rekommenderade fem storage-kontona där operativsystemet diskar och diagnostiska data lagras.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-148">This template uses a loop to create the recommended five storage accounts where the operating system disks and diagnostic data are stored.</span></span> <span data-ttu-id="a8bfd-149">Denna uppsättning konton stöder upp till 100 virtuella datorer i en skaluppsättning, vilket är största.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-149">This set of accounts can support up to 100 virtual machines in a scale set, which is the current maximum.</span></span> <span data-ttu-id="a8bfd-150">Varje lagringskonto heter med en bokstav enhetsbeteckning som definierades i variablerna i kombination med det prefix du anger i parametrarna för mallen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-150">Each storage account is named with a letter designator that was defined in the variables combined with the prefix that you provide in the parameters for the template.</span></span>

    ```json
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
      "apiVersion": "2015-06-15",
      "copy": {
        "name": "storageLoop",
        "count": 5
      },
      "location": "[resourceGroup().location]",
      "properties": { "accountType": "Standard_LRS" }
    },
    ```

5. <span data-ttu-id="a8bfd-151">Lägg till resursen virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-151">Add the virtual network resource.</span></span> <span data-ttu-id="a8bfd-152">Mer information finns i [Nätverksresursprovidern](../virtual-network/resource-groups-networking.md).</span><span class="sxs-lookup"><span data-stu-id="a8bfd-152">For more information, see [Network Resource Provider](../virtual-network/resource-groups-networking.md).</span></span>

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
        "subnets": [
          {
            "name": "subnet1",
            "properties": { "addressPrefix": "10.0.0.0/24" }
          }
        ]
      }
    },
    ```

6. <span data-ttu-id="a8bfd-153">Lägg till de offentliga IP-adress-resurser som används av belastningsutjämnare och nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-153">Add the public IP address resources that are used by the load balancer and network interface.</span></span>

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP1')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName1')]"
        }
      }
    },
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP2')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName2')]"
        }
      }
    },
    ```

7. <span data-ttu-id="a8bfd-154">Lägg till belastningsutjämningsresurs som används av skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-154">Add the load balancer resource that is used by the scale set.</span></span> <span data-ttu-id="a8bfd-155">Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnaren](../load-balancer/load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="a8bfd-155">For more information, see [Azure Resource Manager Support for Load Balancer](../load-balancer/load-balancer-arm.md).</span></span>

    ```json   
    {
      "apiVersion": "2015-06-15",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
              }
            }
          }
        ],
        "backendAddressPools": [ { "name": "bepool1" } ],
        "inboundNatPools": [
          {
            "name": "natpool1",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPortRangeStart": 50000,
              "frontendPortRangeEnd": 50500,
              "backendPort": 3389
            }
          }
        ]
      }
    },
    ```

8. <span data-ttu-id="a8bfd-156">Lägg till gränssnittet nätverksresurs som används av en separat virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-156">Add the network interface resource that is used by the separate virtual machine.</span></span> <span data-ttu-id="a8bfd-157">Eftersom datorer i en skaluppsättning inte är tillgängligt via en offentlig IP-adress, skapas en separat virtuell dator i samma virtuella nätverk via fjärranslutning åtkomst på datorer.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-157">Because machines in a scale set aren't accessible through a public IP address, a separate virtual machine is created in the same virtual network to remotely access the machines.</span></span>

    ```json  
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
              },
              "subnet": {
                "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
              }
            }
          }
        ]
      }
    },
    ```

9. <span data-ttu-id="a8bfd-158">Lägg till en separat virtuell dator i samma nätverk som skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-158">Add the separate virtual machine in the same network as the scale set.</span></span>

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": { "vmSize": "Standard_A1" },
        "osProfile": {
          "computername": "[parameters('vmName')]",
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
            "name": "[concat(parameters('resourcePrefix'), 'os1')]",
            "vhd": {
              "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"        
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        }
      }
    },
    ```

10. <span data-ttu-id="a8bfd-159">Lägga till virtuella datorns skaluppsättning resurs och ange diagnostik-tillägget som är installerad på alla virtuella datorer i skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-159">Add the virtual machine scale set resource and specify the diagnostics extension that is installed on all virtual machines in the scale set.</span></span> <span data-ttu-id="a8bfd-160">Många av inställningarna för den här resursen är liknande med den virtuella datorresursen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-160">Many of the settings for this resource are similar with the virtual machine resource.</span></span> <span data-ttu-id="a8bfd-161">De viktigaste skillnaderna är kapacitet elementet som anger hur många virtuella datorer i skalningsuppsättningen och upgradePolicy som anger hur uppdateringar görs i virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-161">The main differences are the capacity element that specifies the number of virtual machines in the scale set, and upgradePolicy that specifies how updates are made to virtual machines.</span></span> <span data-ttu-id="a8bfd-162">Skaluppsättning skapas inte förrän alla lagringskonton skapas som anges med dependsOn-element.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-162">The scale set is not created until all the storage accounts are created as specified with the dependsOn element.</span></span>

    ```json
    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "apiVersion": "2016-03-30",
      "name": "[parameters('vmSSName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "sku": {
        "name": "Standard_A1",
        "tier": "Standard",
        "capacity": "[parameters('instanceCount')]"
      },
      "properties": {
        "upgradePolicy": {
          "mode": "Manual"
        },
        "virtualMachineProfile": {
          "storageProfile": {
            "osDisk": {
              "vhdContainers": [
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "MicrosoftWindowsServer",
              "offer": "WindowsServer",
              "sku": "2012-R2-Datacenter",
              "version": "latest"
            }
          },
          "osProfile": {
            "computerNamePrefix": "[parameters('vmSSName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
          },
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "networkconfig1",
                "properties": {
                  "primary": "true",
                  "ipConfigurations": [
                    {
                      "name": "ip1",
                      "properties": {
                        "subnet": {
                          "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          },
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Insights.VMDiagnosticsSettings",
                "properties": {
                  "publisher": "Microsoft.Azure.Diagnostics",
                  "type": "IaaSDiagnostics",
                  "typeHandlerVersion": "1.5",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint": "https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. <span data-ttu-id="a8bfd-163">Lägg till resursen autoscaleSettings som definierar hur skaluppsättning justerar baserat på processoranvändning på datorer på skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-163">Add the autoscaleSettings resource that defines how the scale set adjusts based on the processor usage on the machines in the scale set.</span></span>

    ```json
    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Processor(_Total)\\% Processor Time",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 50.0
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      }
    }
    ```

    <span data-ttu-id="a8bfd-164">Dessa värden är viktiga för den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="a8bfd-164">For this tutorial, these values are important:</span></span>
    
    * <span data-ttu-id="a8bfd-165">**metricName**</span><span class="sxs-lookup"><span data-stu-id="a8bfd-165">**metricName**</span></span>  
    <span data-ttu-id="a8bfd-166">Det här värdet är samma som de prestandaräknare som vi har definierat i variabeln wadperfcounter.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-166">This value is the same as the performance counter that we defined in the wadperfcounter variable.</span></span> <span data-ttu-id="a8bfd-167">Med hjälp av variabeln, samlar in diagnostik-tillägget i **Processor(_Total)\% processortid** räknaren.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-167">Using that variable, the Diagnostics extension collects the  **Processor(_Total)\% Processor Time** counter.</span></span>
    
    * <span data-ttu-id="a8bfd-168">**metricResourceUri**</span><span class="sxs-lookup"><span data-stu-id="a8bfd-168">**metricResourceUri**</span></span>  
    <span data-ttu-id="a8bfd-169">Det här värdet är resursidentifieraren för virtuella datorns skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-169">This value is the resource identifier of the virtual machine scale set.</span></span>
    
    * <span data-ttu-id="a8bfd-170">**Tidskorn**</span><span class="sxs-lookup"><span data-stu-id="a8bfd-170">**timeGrain**</span></span>  
    <span data-ttu-id="a8bfd-171">Det här värdet är Granulariteten för de mätvärden som samlats in.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-171">This value is the granularity of the metrics that are collected.</span></span> <span data-ttu-id="a8bfd-172">I den här mallen är den inställd på en minut.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-172">In this template, it is set to one minute.</span></span>
    
    * <span data-ttu-id="a8bfd-173">**statistik**</span><span class="sxs-lookup"><span data-stu-id="a8bfd-173">**statistic**</span></span>  
    <span data-ttu-id="a8bfd-174">Det här värdet avgör hur mätvärdena kombineras för automatisk skalning åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-174">This value determines how the metrics are combined to accommodate the automatic scaling action.</span></span> <span data-ttu-id="a8bfd-175">Möjliga värden är: medel, Min, Max.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-175">The possible values are: Average, Min, Max.</span></span> <span data-ttu-id="a8bfd-176">Genomsnittlig totala CPU-användningen av virtuella datorer samlas in i den här mallen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-176">In this template, the average total CPU usage of the virtual machines is collected.</span></span>
    
    * <span data-ttu-id="a8bfd-177">**värdet timeWindow**</span><span class="sxs-lookup"><span data-stu-id="a8bfd-177">**timeWindow**</span></span>  
    <span data-ttu-id="a8bfd-178">Det här värdet är tidsintervallet där instansens data samlas in.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-178">This value is the range of time in which instance data is collected.</span></span> <span data-ttu-id="a8bfd-179">Det måste vara mellan 5 minuter och 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-179">It must be between 5 minutes and 12 hours.</span></span>
    
    * <span data-ttu-id="a8bfd-180">**timeAggregation**</span><span class="sxs-lookup"><span data-stu-id="a8bfd-180">**timeAggregation**</span></span>  
    <span data-ttu-id="a8bfd-181">hans värdet avgör hur data som samlas in ska kombineras med tiden.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-181">his value determines how the data that is collected should be combined over time.</span></span> <span data-ttu-id="a8bfd-182">Standardvärdet är medelvärde.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-182">The default value is Average.</span></span> <span data-ttu-id="a8bfd-183">Möjliga värden är:, lägsta, högsta, senaste, totalt antal, räknas.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-183">The possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span>
    
    * <span data-ttu-id="a8bfd-184">**operatorn**</span><span class="sxs-lookup"><span data-stu-id="a8bfd-184">**operator**</span></span>  
    <span data-ttu-id="a8bfd-185">Det här värdet är den operator som används för att jämföra måttinformationen och tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-185">This value is the operator that is used to compare the metric data and the threshold.</span></span> <span data-ttu-id="a8bfd-186">Möjliga värden är: är lika med, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-186">The possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span>
    
    * <span data-ttu-id="a8bfd-187">**Tröskelvärde**</span><span class="sxs-lookup"><span data-stu-id="a8bfd-187">**threshold**</span></span>  
    <span data-ttu-id="a8bfd-188">Det här värdet är värdet som utlöser skalningsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-188">This value is the value that triggers the scale action.</span></span> <span data-ttu-id="a8bfd-189">I den här mallen läggs datorer till skaluppsättningen när den genomsnittliga processoranvändningen mellan datorer i uppsättningen är över 50%.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-189">In this template, machines are added to the scale set when the average CPU usage among machines in the set is over 50%.</span></span>
    
    * <span data-ttu-id="a8bfd-190">**riktning**</span><span class="sxs-lookup"><span data-stu-id="a8bfd-190">**direction**</span></span>  
    <span data-ttu-id="a8bfd-191">Det här värdet anger den åtgärd som utförs när ett tröskelvärde uppnås.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-191">This value determines the action that is taken when the threshold value is achieved.</span></span> <span data-ttu-id="a8bfd-192">Möjliga värden är ökning eller minskning.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-192">The possible values are Increase or Decrease.</span></span> <span data-ttu-id="a8bfd-193">I den här mallen ökar antalet virtuella datorer i skaluppsättning om tröskelvärdet för över 50% under den definierade tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-193">In this template, the number of virtual machines in the scale set is increased if the threshold is over 50% in the defined time window.</span></span>
    
    * <span data-ttu-id="a8bfd-194">**typ**</span><span class="sxs-lookup"><span data-stu-id="a8bfd-194">**type**</span></span>  
    <span data-ttu-id="a8bfd-195">Det här värdet är typ av åtgärd som ska ske och måste anges till ChangeCount.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-195">This value is the type of action that should occur and must be set to ChangeCount.</span></span>
    
    * <span data-ttu-id="a8bfd-196">**värdet**</span><span class="sxs-lookup"><span data-stu-id="a8bfd-196">**value**</span></span>  
    <span data-ttu-id="a8bfd-197">Det här värdet är antalet virtuella datorer som läggs till eller tas bort från skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-197">This value is the number of virtual machines that are added or removed from the scale set.</span></span> <span data-ttu-id="a8bfd-198">Det här värdet måste vara 1 eller högre.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-198">This value must be 1 or greater.</span></span> <span data-ttu-id="a8bfd-199">Standardvärdet är 1.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-199">The default value is 1.</span></span> <span data-ttu-id="a8bfd-200">I den här mallen ange antalet datorer i skalan ökar med 1 när tröskelvärdet är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-200">In this template, the number of machines in the scale set increases by 1 when the threshold is met.</span></span>
    
    * <span data-ttu-id="a8bfd-201">**cooldown**</span><span class="sxs-lookup"><span data-stu-id="a8bfd-201">**cooldown**</span></span>  
    <span data-ttu-id="a8bfd-202">Det här värdet är hur lång tid att vänta sedan den senaste skalning åtgärden innan nästa åtgärd inträffar.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-202">This value is the amount of time to wait since the last scaling action before the next action occurs.</span></span> <span data-ttu-id="a8bfd-203">Det här värdet måste vara mellan en minut och en vecka.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-203">This value must be between one minute and one week.</span></span>

12. <span data-ttu-id="a8bfd-204">Spara filen med mallen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-204">Save the template file.</span></span>    

## <a name="step-4-upload-the-template-to-storage"></a><span data-ttu-id="a8bfd-205">Steg 4: Överför mallen till lagring</span><span class="sxs-lookup"><span data-stu-id="a8bfd-205">Step 4: Upload the template to storage</span></span>
<span data-ttu-id="a8bfd-206">Mallen kan överföras så länge som du känner till namn och en primär nyckel för lagringskontot som du skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-206">The template can be uploaded as long as you know the name and primary key of the storage account that you created in step 1.</span></span>

1. <span data-ttu-id="a8bfd-207">Ange en variabel som anger namnet på lagringskontot som du skapade i steg 1 i Microsoft Azure PowerShell-fönster.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-207">In the Microsoft Azure PowerShell window, set a variable that specifies the name of the storage account that you created in step 1.</span></span>
   
    ```powershell
    $storageAccountName = "vmstestsa"
    ```

2. <span data-ttu-id="a8bfd-208">Ange en variabel som anger den primära nyckeln för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-208">Set a variable that specifies the primary key of the storage account.</span></span>
   
    ```powershell
    $storageAccountKey = "<primary-account-key>"
    ```
   
   <span data-ttu-id="a8bfd-209">Du kan hämta den här nyckeln genom att klicka på nyckelikonen när du visar konto lagringsresurs i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-209">You can get this key by clicking the key icon when viewing the storage account resource in the Azure portal.</span></span>
3. <span data-ttu-id="a8bfd-210">Skapa storage-konto context-objektet som används för att validera åtgärder med storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-210">Create the storage account context object that is used to validate operations with the storage account.</span></span>
   
    ```powershell
    $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
    ```

4. <span data-ttu-id="a8bfd-211">Skapa behållare för lagring av mallen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-211">Create the container for storing the template.</span></span>

    ```powershell
    $containerName = "templates"
    New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob
    ```

5. <span data-ttu-id="a8bfd-212">Överför mallfilen till den nya behållaren.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-212">Upload the template file to the new container.</span></span>

    ```powershell   
    $blobName = "VMSSTemplate.json"
    $fileName = "C:\" + $BlobName
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx
    ```

## <a name="step-5-deploy-the-template"></a><span data-ttu-id="a8bfd-213">Steg 5: Distribuera mallen</span><span class="sxs-lookup"><span data-stu-id="a8bfd-213">Step 5: Deploy the template</span></span>
<span data-ttu-id="a8bfd-214">Nu när du har skapat mallen kan du börja distribuera resurserna.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-214">Now that you created the template, you can start deploying the resources.</span></span> <span data-ttu-id="a8bfd-215">Använd följande kommando för att starta processen:</span><span class="sxs-lookup"><span data-stu-id="a8bfd-215">Use this command to start the process:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"
```

<span data-ttu-id="a8bfd-216">När du trycker på Ange, du uppmanas att ange värden för variabler som du tilldelat.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-216">When you press enter, you are prompted to provide values for the variables you assigned.</span></span> <span data-ttu-id="a8bfd-217">Ange dessa värden:</span><span class="sxs-lookup"><span data-stu-id="a8bfd-217">Provide these values:</span></span>

```powershell
vmName: vmsstestvm1
  vmSSName: vmsstest1
  instanceCount: 5
  adminUserName: vmadmin1
  adminPassword: VMpass1
  resourcePrefix: vmsstest
```

<span data-ttu-id="a8bfd-218">Det bör ta ungefär 15 minuter för alla resurser som har ska distribueras.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-218">It should take about 15 minutes for all the resources to successfully be deployed.</span></span>

> [!NOTE]
> <span data-ttu-id="a8bfd-219">Du kan också använda portalens möjlighet för att distribuera resurserna.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-219">You can also use the portal’s ability to deploy the resources.</span></span> <span data-ttu-id="a8bfd-220">Använd den här länken ”: https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>”</span><span class="sxs-lookup"><span data-stu-id="a8bfd-220">Use this link: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"</span></span>
> 
> 

## <a name="step-6-monitor-resources"></a><span data-ttu-id="a8bfd-221">Steg 6: Övervaka resurser</span><span class="sxs-lookup"><span data-stu-id="a8bfd-221">Step 6: Monitor resources</span></span>
<span data-ttu-id="a8bfd-222">Du kan få information om skalningsuppsättningar i virtuella datorer på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a8bfd-222">You can get some information about virtual machine scale sets using these methods:</span></span>

* <span data-ttu-id="a8bfd-223">Azure portal – du får för närvarande en begränsad mängd information med hjälp av portalen.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-223">The Azure portal - You can currently get a limited amount of information using the portal.</span></span>
* <span data-ttu-id="a8bfd-224">Den [resursutforskaren Azure](https://resources.azure.com/) – det här verktyget är bäst för att utforska det aktuella tillståndet för din skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-224">The [Azure Resource Explorer](https://resources.azure.com/) - This tool is the best for exploring the current state of your scale set.</span></span> <span data-ttu-id="a8bfd-225">Följ den här sökvägen och du bör se Instansvy för skaluppsättning som du skapade:</span><span class="sxs-lookup"><span data-stu-id="a8bfd-225">Follow this path and you should see the instance view of the scale set that you created:</span></span>
  
    <span data-ttu-id="a8bfd-226">prenumerationer > {din prenumeration} > resursgrupper > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines</span><span class="sxs-lookup"><span data-stu-id="a8bfd-226">subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines</span></span>

* <span data-ttu-id="a8bfd-227">Azure PowerShell - Använd detta kommando för att hämta viss information:</span><span class="sxs-lookup"><span data-stu-id="a8bfd-227">Azure PowerShell - Use this command to get some information:</span></span>

  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  ```
  
  <span data-ttu-id="a8bfd-228">Eller</span><span class="sxs-lookup"><span data-stu-id="a8bfd-228">Or</span></span>
  
  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView
  ```

* <span data-ttu-id="a8bfd-229">Ansluta till den separata virtuella datorn precis som du skulle någon annan dator och sedan kan du fjärransluta till de virtuella datorerna i skaluppsättningen att övervaka enskilda processer.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-229">Connect to the separate virtual machine just like you would any other machine and then you can remotely access the virtual machines in the scale set to monitor individual processes.</span></span>

> [!NOTE]
> <span data-ttu-id="a8bfd-230">En fullständig REST-API för att få information om skaluppsättningar finns i [Skalningsuppsättningarna för virtuella datorer](https://msdn.microsoft.com/library/mt589023.aspx)</span><span class="sxs-lookup"><span data-stu-id="a8bfd-230">A complete REST API for obtaining information about scale sets can be found in [Virtual Machine Scale Sets](https://msdn.microsoft.com/library/mt589023.aspx)</span></span>

## <a name="step-7-remove-the-resources"></a><span data-ttu-id="a8bfd-231">Steg 7: Ta bort resurserna</span><span class="sxs-lookup"><span data-stu-id="a8bfd-231">Step 7: Remove the resources</span></span>
<span data-ttu-id="a8bfd-232">Eftersom du debiteras för de resurser som används i Azure, men det är alltid en bra idé att ta bort resurser som inte längre behövs.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-232">Because you are charged for resources used in Azure, it is always a good practice to delete resources that are no longer needed.</span></span> <span data-ttu-id="a8bfd-233">Du behöver inte ta bort varje resurs separat från en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-233">You don’t need to delete each resource separately from a resource group.</span></span> <span data-ttu-id="a8bfd-234">Du kan ta bort resursgruppen och alla dess resurser tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-234">You can delete the resource group and all its resources are automatically deleted.</span></span>

  ```powershell
  Remove-AzureRmResourceGroup -Name vmsstestrg1
  ```

<span data-ttu-id="a8bfd-235">Om du vill behålla din resursgrupp kan du ta bort skaluppsättningen endast.</span><span class="sxs-lookup"><span data-stu-id="a8bfd-235">If you want to keep your resource group, you can delete the scale set only.</span></span>

  ```powershell
  Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"
  ```

## <a name="next-steps"></a><span data-ttu-id="a8bfd-236">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8bfd-236">Next steps</span></span>
* <span data-ttu-id="a8bfd-237">Hantera skaluppsättning som du just har skapat med hjälp av informationen i [hantera virtuella datorer i en virtuell dator Skaluppsättning](virtual-machine-scale-sets-windows-manage.md).</span><span class="sxs-lookup"><span data-stu-id="a8bfd-237">Manage the scale set that you just created using the information in [Manage virtual machines in a Virtual Machine Scale Set](virtual-machine-scale-sets-windows-manage.md).</span></span>
* <span data-ttu-id="a8bfd-238">Läs mer om vertikal skalning i [Vertikal automatisk skalning med skaluppsättningar för virtuella datorer](virtual-machine-scale-sets-vertical-scale-reprovision.md)</span><span class="sxs-lookup"><span data-stu-id="a8bfd-238">Learn more about vertical scaling by reviewing [Vertical autoscale with Virtual Machine Scale sets](virtual-machine-scale-sets-vertical-scale-reprovision.md)</span></span>
* <span data-ttu-id="a8bfd-239">Hitta exempel på Azure-Monitor övervakningsfunktionerna i [Azure-Monitor PowerShell snabb start prover](../monitoring-and-diagnostics/insights-powershell-samples.md)</span><span class="sxs-lookup"><span data-stu-id="a8bfd-239">Find examples of Azure Monitor monitoring features in [Azure Monitor PowerShell quick start samples](../monitoring-and-diagnostics/insights-powershell-samples.md)</span></span>
* <span data-ttu-id="a8bfd-240">Lär dig mer om meddelandefunktioner i [använda automatiska åtgärder för att skicka e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="a8bfd-240">Learn about notification features in [Use autoscale actions to send email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span></span>
* <span data-ttu-id="a8bfd-241">Lär dig hur du [Använd granskningsloggarna för att skicka e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="a8bfd-241">Learn how to [Use audit logs to send email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>

