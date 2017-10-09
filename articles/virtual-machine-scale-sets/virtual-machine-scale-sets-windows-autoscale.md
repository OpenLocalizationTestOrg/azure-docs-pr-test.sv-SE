---
title: "aaaAutoscale Windows Virtual Machine-Skalningsuppsättningar | Microsoft Docs"
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
ms.openlocfilehash: 67cf1c5063ceba4fc076dc270090defdbddbcfe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="315a3-103">Skala automatiskt datorer i en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="315a3-103">Automatically scale machines in a virtual machine scale set</span></span>
<span data-ttu-id="315a3-104">Skaluppsättningar för den virtuella datorn gör det lättare för dig toodeploy och hantera identiska virtuella datorer som en uppsättning.</span><span class="sxs-lookup"><span data-stu-id="315a3-104">Virtual machine scale sets make it easy for you toodeploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="315a3-105">Skaluppsättningar ger en skalbar och anpassningsbara beräkningslager för storskaliga program och stöd för avbildningar för Windows-plattformen, Linux-plattformen bilder, anpassade avbildningar och tillägg.</span><span class="sxs-lookup"><span data-stu-id="315a3-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="315a3-106">Läs mer om skaluppsättningar [Skalningsuppsättningarna för virtuella](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="315a3-106">For more information about scale sets, see [Virtual Machine Scale Sets](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="315a3-107">Den här kursen visar hur toocreate en skaluppsättning för virtuella Windows-datorer och automatiskt skala hello datorer i hello anges.</span><span class="sxs-lookup"><span data-stu-id="315a3-107">This tutorial shows you how toocreate a scale set of Windows virtual machines and automatically scale hello machines in hello set.</span></span> <span data-ttu-id="315a3-108">Du kan skapa hello skala och skalning genom att skapa en Azure Resource Manager-mall och distribuera den med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="315a3-108">You create hello scale set and set up scaling by creating an Azure Resource Manager template and deploying it using Azure PowerShell.</span></span> <span data-ttu-id="315a3-109">Mer information om mallar finns i [Redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="315a3-109">For more information about templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="315a3-110">toolearn mer om automatisk skalning av skalningsuppsättningar, se [automatisk skalning och Skalningsuppsättningarna för virtuella](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="315a3-110">toolearn more about automatic scaling of scale sets, see [Automatic scaling and Virtual Machine Scale Sets](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

<span data-ttu-id="315a3-111">I den här artikeln får du distribuera hello följande resurser och tillägg:</span><span class="sxs-lookup"><span data-stu-id="315a3-111">In this article, you deploy hello following resources and extensions:</span></span>

* <span data-ttu-id="315a3-112">Microsoft.Storage/storageAccounts</span><span class="sxs-lookup"><span data-stu-id="315a3-112">Microsoft.Storage/storageAccounts</span></span>
* <span data-ttu-id="315a3-113">Microsoft.Network/virtualNetworks</span><span class="sxs-lookup"><span data-stu-id="315a3-113">Microsoft.Network/virtualNetworks</span></span>
* <span data-ttu-id="315a3-114">Microsoft.Network/publicIPAddresses</span><span class="sxs-lookup"><span data-stu-id="315a3-114">Microsoft.Network/publicIPAddresses</span></span>
* <span data-ttu-id="315a3-115">Microsoft.Network/loadBalancers</span><span class="sxs-lookup"><span data-stu-id="315a3-115">Microsoft.Network/loadBalancers</span></span>
* <span data-ttu-id="315a3-116">Microsoft.Network/networkInterfaces</span><span class="sxs-lookup"><span data-stu-id="315a3-116">Microsoft.Network/networkInterfaces</span></span>
* <span data-ttu-id="315a3-117">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="315a3-117">Microsoft.Compute/virtualMachines</span></span>
* <span data-ttu-id="315a3-118">Microsoft.Compute/virtualMachineScaleSets</span><span class="sxs-lookup"><span data-stu-id="315a3-118">Microsoft.Compute/virtualMachineScaleSets</span></span>
* <span data-ttu-id="315a3-119">Microsoft.Insights.VMDiagnosticsSettings</span><span class="sxs-lookup"><span data-stu-id="315a3-119">Microsoft.Insights.VMDiagnosticsSettings</span></span>
* <span data-ttu-id="315a3-120">Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="315a3-120">Microsoft.Insights/autoscaleSettings</span></span>

<span data-ttu-id="315a3-121">Mer information om Resource Manager-resurser finns [Azure Resource Manager och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="315a3-121">For more information about Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

## <a name="step-1-install-azure-powershell"></a><span data-ttu-id="315a3-122">Steg 1: Installera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="315a3-122">Step 1: Install Azure PowerShell</span></span>
<span data-ttu-id="315a3-123">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) information om installation hello senaste versionen av Azure PowerShell, väljer din prenumeration och loggar in tooAzure.</span><span class="sxs-lookup"><span data-stu-id="315a3-123">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for information about installing hello latest version of Azure PowerShell, selecting your subscription, and signing in tooAzure.</span></span>

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a><span data-ttu-id="315a3-124">Steg 2: Skapa en resursgrupp och ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="315a3-124">Step 2: Create a resource group and a storage account</span></span>
1. <span data-ttu-id="315a3-125">**Skapa en resursgrupp** – alla resurser måste vara distribuerad tooa resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="315a3-125">**Create a resource group** – All resources must be deployed tooa resource group.</span></span> <span data-ttu-id="315a3-126">Använd [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) toocreate en resursgrupp med namnet **vmsstestrg1**.</span><span class="sxs-lookup"><span data-stu-id="315a3-126">Use [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) toocreate a resource group named **vmsstestrg1**.</span></span>
2. <span data-ttu-id="315a3-127">**Skapa ett lagringskonto** – det här lagringskontot sparas om hello mallen.</span><span class="sxs-lookup"><span data-stu-id="315a3-127">**Create a storage account** – This storage account is where hello template is stored.</span></span> <span data-ttu-id="315a3-128">Använd [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) toocreate ett lagringskonto med namnet **vmsstestsa**.</span><span class="sxs-lookup"><span data-stu-id="315a3-128">Use [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) toocreate a storage account named **vmsstestsa**.</span></span>

## <a name="step-3-create-hello-template"></a><span data-ttu-id="315a3-129">Steg 3: Skapa hello mall</span><span class="sxs-lookup"><span data-stu-id="315a3-129">Step 3: Create hello template</span></span>
<span data-ttu-id="315a3-130">En mall för Azure Resource Manager gör det möjligt för du toodeploy och hantera Azure-resurser tillsammans med hjälp av en JSON-beskrivning av hello resurser och associerade distributionen parametrar.</span><span class="sxs-lookup"><span data-stu-id="315a3-130">An Azure Resource Manager template makes it possible for you toodeploy and manage Azure resources together by using a JSON description of hello resources and associated deployment parameters.</span></span>

1. <span data-ttu-id="315a3-131">Skapa hello fil C:\VMSSTemplate.json i din favorit-redigeraren och lägga till hello inledande JSON strukturen toosupport hello mallen.</span><span class="sxs-lookup"><span data-stu-id="315a3-131">In your favorite editor, create hello file C:\VMSSTemplate.json and add hello initial JSON structure toosupport hello template.</span></span>

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

2. <span data-ttu-id="315a3-132">Parametrar krävs inte alltid, men de ger ett sätt tooinput som värden när hello mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="315a3-132">Parameters are not always required, but they provide a way tooinput values when hello template is deployed.</span></span> <span data-ttu-id="315a3-133">Lägg till de här parametrarna under hello parametrar överordnade element som du lagt till toohello mall.</span><span class="sxs-lookup"><span data-stu-id="315a3-133">Add these parameters under hello parameters parent element that you added toohello template.</span></span>

    ```json   
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
    * <span data-ttu-id="315a3-134">Ange ett namn för hello separat virtuell dator som används tooaccess hello datorer i hello skala.</span><span class="sxs-lookup"><span data-stu-id="315a3-134">A name for hello separate virtual machine that is used tooaccess hello machines in hello scale set.</span></span>
    * <span data-ttu-id="315a3-135">hello namnet på hello lagringskonto där hello mall ska lagras.</span><span class="sxs-lookup"><span data-stu-id="315a3-135">hello name of hello storage account where hello template is stored.</span></span>
    * <span data-ttu-id="315a3-136">hello antalet virtuella datorer tooinitially skapa i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="315a3-136">hello number of virtual machines tooinitially create in hello scale set.</span></span>
    * <span data-ttu-id="315a3-137">hello namnet och lösenordet för administratörskontot för hello på hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="315a3-137">hello name and password of hello administrator account on hello virtual machines.</span></span>
    * <span data-ttu-id="315a3-138">Ange ett namnprefix för hello-resurser som har skapats toosupport hello skala.</span><span class="sxs-lookup"><span data-stu-id="315a3-138">A name prefix for hello resources that are created toosupport hello scale set.</span></span>

3. <span data-ttu-id="315a3-139">Variabler kan användas i en mall toospecify värden som ändras ofta eller värden som behöver toobe som skapats från en kombination av parametervärden.</span><span class="sxs-lookup"><span data-stu-id="315a3-139">Variables can be used in a template toospecify values that may change frequently or values that need toobe created from a combination of parameter values.</span></span> <span data-ttu-id="315a3-140">Lägg till dessa variabler under hello variabler överordnade element som du lagt till toohello mall.</span><span class="sxs-lookup"><span data-stu-id="315a3-140">Add these variables under hello variables parent element that you added toohello template.</span></span>

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
   
    * <span data-ttu-id="315a3-141">DNS-namn som används av hello nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="315a3-141">DNS names that are used by hello network interfaces.</span></span>

        * <span data-ttu-id="315a3-142">hello IP-adressnamn och prefix för hello virtuella nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="315a3-142">hello IP address names and prefixes for hello virtual network and subnets.</span></span>
        * <span data-ttu-id="315a3-143">hello namn och identifierare för hello virtuellt nätverk, läsa in belastningsutjämning och nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="315a3-143">hello names and identifiers of hello virtual network, load balancer, and network interfaces.</span></span>
        * <span data-ttu-id="315a3-144">Lagringskontonamn för hello konton som är associerade med hello datorer i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="315a3-144">Storage account names for hello accounts associated with hello machines in hello scale set.</span></span>
        * <span data-ttu-id="315a3-145">Inställningar för hello diagnostik-tillägg som är installerad på hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="315a3-145">Settings for hello Diagnostics extension that is installed on hello virtual machines.</span></span> <span data-ttu-id="315a3-146">Läs mer om hello diagnostik tillägget [skapa en Windows virtuell dator med övervakning och diagnostik med Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="315a3-146">For more information about hello Diagnostics extension, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

4. <span data-ttu-id="315a3-147">Lägg till hello konto lagringsresurs under hello resurser överordnade element som du lagt till toohello mall.</span><span class="sxs-lookup"><span data-stu-id="315a3-147">Add hello storage account resource under hello resources parent element that you added toohello template.</span></span> <span data-ttu-id="315a3-148">Den här mallen använder en loop toocreate hello rekommenderas fem storage-konton där hello operativsystemet diskar och diagnostiska data lagras.</span><span class="sxs-lookup"><span data-stu-id="315a3-148">This template uses a loop toocreate hello recommended five storage accounts where hello operating system disks and diagnostic data are stored.</span></span> <span data-ttu-id="315a3-149">Denna uppsättning konton kan stödja av too100 virtuella datorer i en skaluppsättning hello aktuella högsta.</span><span class="sxs-lookup"><span data-stu-id="315a3-149">This set of accounts can support up too100 virtual machines in a scale set, which is hello current maximum.</span></span> <span data-ttu-id="315a3-150">Varje lagringskonto heter med en bokstav enhetsbeteckning som definierades i hello variabler i kombination med hello prefix du anger i hello parametrar för hello mall.</span><span class="sxs-lookup"><span data-stu-id="315a3-150">Each storage account is named with a letter designator that was defined in hello variables combined with hello prefix that you provide in hello parameters for hello template.</span></span>

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

5. <span data-ttu-id="315a3-151">Lägg till resurs för hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="315a3-151">Add hello virtual network resource.</span></span> <span data-ttu-id="315a3-152">Mer information finns i [Nätverksresursprovidern](../virtual-network/resource-groups-networking.md).</span><span class="sxs-lookup"><span data-stu-id="315a3-152">For more information, see [Network Resource Provider](../virtual-network/resource-groups-networking.md).</span></span>

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

6. <span data-ttu-id="315a3-153">Lägg till hello offentliga IP-adress-resurser som används av hello läsa in belastningsutjämning och nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="315a3-153">Add hello public IP address resources that are used by hello load balancer and network interface.</span></span>

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

7. <span data-ttu-id="315a3-154">Lägg till hello belastningsutjämningsresurs som används av hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="315a3-154">Add hello load balancer resource that is used by hello scale set.</span></span> <span data-ttu-id="315a3-155">Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnaren](../load-balancer/load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="315a3-155">For more information, see [Azure Resource Manager Support for Load Balancer](../load-balancer/load-balancer-arm.md).</span></span>

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

8. <span data-ttu-id="315a3-156">Lägg till hello gränssnittet nätverksresurs som används av hello separat virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="315a3-156">Add hello network interface resource that is used by hello separate virtual machine.</span></span> <span data-ttu-id="315a3-157">Eftersom datorer i en skaluppsättning inte är tillgängligt via en offentlig IP-adress, en separat virtuell dator skapas i hello samma virtuella nätverk tooremotely åtkomst hello datorer.</span><span class="sxs-lookup"><span data-stu-id="315a3-157">Because machines in a scale set aren't accessible through a public IP address, a separate virtual machine is created in hello same virtual network tooremotely access hello machines.</span></span>

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

9. <span data-ttu-id="315a3-158">Lägg till hello separat virtuell dator i samma nätverk som hello skaluppsättning hello.</span><span class="sxs-lookup"><span data-stu-id="315a3-158">Add hello separate virtual machine in hello same network as hello scale set.</span></span>

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

10. <span data-ttu-id="315a3-159">Lägga till hello virtuella skala resursen och ange hello diagnostik tillägg som är installerad på alla virtuella datorer i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="315a3-159">Add hello virtual machine scale set resource and specify hello diagnostics extension that is installed on all virtual machines in hello scale set.</span></span> <span data-ttu-id="315a3-160">Många av hello inställningar för den här resursen är liknande med hello virtuella datorresurser.</span><span class="sxs-lookup"><span data-stu-id="315a3-160">Many of hello settings for this resource are similar with hello virtual machine resource.</span></span> <span data-ttu-id="315a3-161">hello viktigaste skillnaderna är hello kapacitet element som anger hello antalet virtuella datorer i hello skaluppsättning och upgradePolicy som anger hur uppdateringar görs toovirtual datorer.</span><span class="sxs-lookup"><span data-stu-id="315a3-161">hello main differences are hello capacity element that specifies hello number of virtual machines in hello scale set, and upgradePolicy that specifies how updates are made toovirtual machines.</span></span> <span data-ttu-id="315a3-162">hello skaluppsättning inte skapas förrän alla hello storage-konton skapas som anges med hello dependsOn-element.</span><span class="sxs-lookup"><span data-stu-id="315a3-162">hello scale set is not created until all hello storage accounts are created as specified with hello dependsOn element.</span></span>

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

11. <span data-ttu-id="315a3-163">Lägga till hello autoscaleSettings resurs som definierar hur hello skaluppsättning justerar baserat på hello processoranvändning på hello datorer i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="315a3-163">Add hello autoscaleSettings resource that defines how hello scale set adjusts based on hello processor usage on hello machines in hello scale set.</span></span>

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

    <span data-ttu-id="315a3-164">Dessa värden är viktiga för den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="315a3-164">For this tutorial, these values are important:</span></span>
    
    * <span data-ttu-id="315a3-165">**metricName**</span><span class="sxs-lookup"><span data-stu-id="315a3-165">**metricName**</span></span>  
    <span data-ttu-id="315a3-166">Det här värdet är hello samma som hello prestandaräknare som vi har definierat i hello wadperfcounter variabeln.</span><span class="sxs-lookup"><span data-stu-id="315a3-166">This value is hello same as hello performance counter that we defined in hello wadperfcounter variable.</span></span> <span data-ttu-id="315a3-167">Använda den variabeln hello diagnostik tillägget samlar in hello **Processor(_Total)\% processortid** räknaren.</span><span class="sxs-lookup"><span data-stu-id="315a3-167">Using that variable, hello Diagnostics extension collects hello  **Processor(_Total)\% Processor Time** counter.</span></span>
    
    * <span data-ttu-id="315a3-168">**metricResourceUri**</span><span class="sxs-lookup"><span data-stu-id="315a3-168">**metricResourceUri**</span></span>  
    <span data-ttu-id="315a3-169">Det här värdet är hello resursidentifieraren för hello skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="315a3-169">This value is hello resource identifier of hello virtual machine scale set.</span></span>
    
    * <span data-ttu-id="315a3-170">**Tidskorn**</span><span class="sxs-lookup"><span data-stu-id="315a3-170">**timeGrain**</span></span>  
    <span data-ttu-id="315a3-171">Det här värdet är hello Granulariteten för hello mått som har samlats in.</span><span class="sxs-lookup"><span data-stu-id="315a3-171">This value is hello granularity of hello metrics that are collected.</span></span> <span data-ttu-id="315a3-172">I den här mallen anges tooone minut.</span><span class="sxs-lookup"><span data-stu-id="315a3-172">In this template, it is set tooone minute.</span></span>
    
    * <span data-ttu-id="315a3-173">**statistik**</span><span class="sxs-lookup"><span data-stu-id="315a3-173">**statistic**</span></span>  
    <span data-ttu-id="315a3-174">Det här värdet avgör hur hello är kombinerade tooaccommodate hello autoskalning åtgärd.</span><span class="sxs-lookup"><span data-stu-id="315a3-174">This value determines how hello metrics are combined tooaccommodate hello automatic scaling action.</span></span> <span data-ttu-id="315a3-175">hello möjliga värden är: medel, Min, Max.</span><span class="sxs-lookup"><span data-stu-id="315a3-175">hello possible values are: Average, Min, Max.</span></span> <span data-ttu-id="315a3-176">Hello genomsnittlig total CPU-användning av hello virtuella datorer samlas in i den här mallen.</span><span class="sxs-lookup"><span data-stu-id="315a3-176">In this template, hello average total CPU usage of hello virtual machines is collected.</span></span>
    
    * <span data-ttu-id="315a3-177">**värdet timeWindow**</span><span class="sxs-lookup"><span data-stu-id="315a3-177">**timeWindow**</span></span>  
    <span data-ttu-id="315a3-178">Det här värdet är hello tidsintervall som samlas in instansdata.</span><span class="sxs-lookup"><span data-stu-id="315a3-178">This value is hello range of time in which instance data is collected.</span></span> <span data-ttu-id="315a3-179">Det måste vara mellan 5 minuter och 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="315a3-179">It must be between 5 minutes and 12 hours.</span></span>
    
    * <span data-ttu-id="315a3-180">**timeAggregation**</span><span class="sxs-lookup"><span data-stu-id="315a3-180">**timeAggregation**</span></span>  
    <span data-ttu-id="315a3-181">hans värdet avgör hur hello data som samlas in ska kombineras med tiden.</span><span class="sxs-lookup"><span data-stu-id="315a3-181">his value determines how hello data that is collected should be combined over time.</span></span> <span data-ttu-id="315a3-182">hello standardvärdet är medelvärdet.</span><span class="sxs-lookup"><span data-stu-id="315a3-182">hello default value is Average.</span></span> <span data-ttu-id="315a3-183">hello möjliga värden är:, lägsta, högsta, senaste, totalt antal, räknas.</span><span class="sxs-lookup"><span data-stu-id="315a3-183">hello possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span>
    
    * <span data-ttu-id="315a3-184">**operatorn**</span><span class="sxs-lookup"><span data-stu-id="315a3-184">**operator**</span></span>  
    <span data-ttu-id="315a3-185">Det här värdet är hello operator som används toocompare hello mått data och hello tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="315a3-185">This value is hello operator that is used toocompare hello metric data and hello threshold.</span></span> <span data-ttu-id="315a3-186">hello möjliga värden är: är lika med, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="315a3-186">hello possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span>
    
    * <span data-ttu-id="315a3-187">**Tröskelvärde**</span><span class="sxs-lookup"><span data-stu-id="315a3-187">**threshold**</span></span>  
    <span data-ttu-id="315a3-188">Det här värdet är hello-värde som utlöser hello skalningsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="315a3-188">This value is hello value that triggers hello scale action.</span></span> <span data-ttu-id="315a3-189">I den här mallen läggs datorer toohello skaluppsättningen när hello genomsnittliga processoranvändningen mellan datorer i hello uppsättningen är över 50%.</span><span class="sxs-lookup"><span data-stu-id="315a3-189">In this template, machines are added toohello scale set when hello average CPU usage among machines in hello set is over 50%.</span></span>
    
    * <span data-ttu-id="315a3-190">**riktning**</span><span class="sxs-lookup"><span data-stu-id="315a3-190">**direction**</span></span>  
    <span data-ttu-id="315a3-191">Det här värdet fastställer hello vad som händer när hello tröskelvärde uppnås.</span><span class="sxs-lookup"><span data-stu-id="315a3-191">This value determines hello action that is taken when hello threshold value is achieved.</span></span> <span data-ttu-id="315a3-192">hello möjliga värden är ökning eller minskning.</span><span class="sxs-lookup"><span data-stu-id="315a3-192">hello possible values are Increase or Decrease.</span></span> <span data-ttu-id="315a3-193">I den här mallen ökas hello antalet virtuella datorer i hello skaluppsättning om hello tröskelvärdet över 50% under hello definierade tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="315a3-193">In this template, hello number of virtual machines in hello scale set is increased if hello threshold is over 50% in hello defined time window.</span></span>
    
    * <span data-ttu-id="315a3-194">**typ**</span><span class="sxs-lookup"><span data-stu-id="315a3-194">**type**</span></span>  
    <span data-ttu-id="315a3-195">Det här värdet är hello typ av åtgärd som ska ske och tooChangeCount måste anges.</span><span class="sxs-lookup"><span data-stu-id="315a3-195">This value is hello type of action that should occur and must be set tooChangeCount.</span></span>
    
    * <span data-ttu-id="315a3-196">**värdet**</span><span class="sxs-lookup"><span data-stu-id="315a3-196">**value**</span></span>  
    <span data-ttu-id="315a3-197">Det här värdet är hello antalet virtuella datorer som läggs till eller tas bort från hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="315a3-197">This value is hello number of virtual machines that are added or removed from hello scale set.</span></span> <span data-ttu-id="315a3-198">Det här värdet måste vara 1 eller högre.</span><span class="sxs-lookup"><span data-stu-id="315a3-198">This value must be 1 or greater.</span></span> <span data-ttu-id="315a3-199">hello standardvärdet är 1.</span><span class="sxs-lookup"><span data-stu-id="315a3-199">hello default value is 1.</span></span> <span data-ttu-id="315a3-200">I den här mallen hello antalet datorer i hello skala in ökar med 1 när hello tröskelvärdet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="315a3-200">In this template, hello number of machines in hello scale set increases by 1 when hello threshold is met.</span></span>
    
    * <span data-ttu-id="315a3-201">**cooldown**</span><span class="sxs-lookup"><span data-stu-id="315a3-201">**cooldown**</span></span>  
    <span data-ttu-id="315a3-202">Det här värdet är hello mängden tid toowait sedan hello senaste skalning åtgärd innan hello nästa åtgärd sker.</span><span class="sxs-lookup"><span data-stu-id="315a3-202">This value is hello amount of time toowait since hello last scaling action before hello next action occurs.</span></span> <span data-ttu-id="315a3-203">Det här värdet måste vara mellan en minut och en vecka.</span><span class="sxs-lookup"><span data-stu-id="315a3-203">This value must be between one minute and one week.</span></span>

12. <span data-ttu-id="315a3-204">Spara filen med hello mallen.</span><span class="sxs-lookup"><span data-stu-id="315a3-204">Save hello template file.</span></span>    

## <a name="step-4-upload-hello-template-toostorage"></a><span data-ttu-id="315a3-205">Steg 4: Överför hello mall toostorage</span><span class="sxs-lookup"><span data-stu-id="315a3-205">Step 4: Upload hello template toostorage</span></span>
<span data-ttu-id="315a3-206">hello mall kan överföras så länge som du känner hello namn och primärnyckel för hello storage-konto som du skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="315a3-206">hello template can be uploaded as long as you know hello name and primary key of hello storage account that you created in step 1.</span></span>

1. <span data-ttu-id="315a3-207">Ange en variabel som anger hello namn hello storage-konto som du skapade i steg 1 i hello Microsoft Azure PowerShell-fönster.</span><span class="sxs-lookup"><span data-stu-id="315a3-207">In hello Microsoft Azure PowerShell window, set a variable that specifies hello name of hello storage account that you created in step 1.</span></span>
   
    ```powershell
    $storageAccountName = "vmstestsa"
    ```

2. <span data-ttu-id="315a3-208">Ange en variabel som anger hello primärnyckel hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="315a3-208">Set a variable that specifies hello primary key of hello storage account.</span></span>
   
    ```powershell
    $storageAccountKey = "<primary-account-key>"
    ```
   
   <span data-ttu-id="315a3-209">Du kan hämta den här nyckeln genom att klicka på nyckelikonen hello när du visar hello konto lagringsresurs i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="315a3-209">You can get this key by clicking hello key icon when viewing hello storage account resource in hello Azure portal.</span></span>
3. <span data-ttu-id="315a3-210">Skapa hello storage-konto context-objektet som används toovalidate åtgärder med hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="315a3-210">Create hello storage account context object that is used toovalidate operations with hello storage account.</span></span>
   
    ```powershell
    $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
    ```

4. <span data-ttu-id="315a3-211">Skapa hello behållare för att lagra hello mall.</span><span class="sxs-lookup"><span data-stu-id="315a3-211">Create hello container for storing hello template.</span></span>

    ```powershell
    $containerName = "templates"
    New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob
    ```

5. <span data-ttu-id="315a3-212">Överför hello mallen filen toohello ny behållare.</span><span class="sxs-lookup"><span data-stu-id="315a3-212">Upload hello template file toohello new container.</span></span>

    ```powershell   
    $blobName = "VMSSTemplate.json"
    $fileName = "C:\" + $BlobName
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx
    ```

## <a name="step-5-deploy-hello-template"></a><span data-ttu-id="315a3-213">Steg 5: Distribuera hello mall</span><span class="sxs-lookup"><span data-stu-id="315a3-213">Step 5: Deploy hello template</span></span>
<span data-ttu-id="315a3-214">Nu när du har skapat hello mallen börja du distribuera hello resurser.</span><span class="sxs-lookup"><span data-stu-id="315a3-214">Now that you created hello template, you can start deploying hello resources.</span></span> <span data-ttu-id="315a3-215">Använd den här processen med kommandot toostart hello:</span><span class="sxs-lookup"><span data-stu-id="315a3-215">Use this command toostart hello process:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"
```

<span data-ttu-id="315a3-216">När du trycker på Ange är du tillfrågas tooprovide värden för hello variabler som du tilldelat.</span><span class="sxs-lookup"><span data-stu-id="315a3-216">When you press enter, you are prompted tooprovide values for hello variables you assigned.</span></span> <span data-ttu-id="315a3-217">Ange dessa värden:</span><span class="sxs-lookup"><span data-stu-id="315a3-217">Provide these values:</span></span>

```powershell
vmName: vmsstestvm1
  vmSSName: vmsstest1
  instanceCount: 5
  adminUserName: vmadmin1
  adminPassword: VMpass1
  resourcePrefix: vmsstest
```

<span data-ttu-id="315a3-218">Det bör ta ungefär 15 minuter för alla hello resurser toosuccessfully distribueras.</span><span class="sxs-lookup"><span data-stu-id="315a3-218">It should take about 15 minutes for all hello resources toosuccessfully be deployed.</span></span>

> [!NOTE]
> <span data-ttu-id="315a3-219">Du kan också använda hello portal möjlighet toodeploy hello resurser.</span><span class="sxs-lookup"><span data-stu-id="315a3-219">You can also use hello portal’s ability toodeploy hello resources.</span></span> <span data-ttu-id="315a3-220">Använd den här länken ”: https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template>”</span><span class="sxs-lookup"><span data-stu-id="315a3-220">Use this link: "https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template>"</span></span>
> 
> 

## <a name="step-6-monitor-resources"></a><span data-ttu-id="315a3-221">Steg 6: Övervaka resurser</span><span class="sxs-lookup"><span data-stu-id="315a3-221">Step 6: Monitor resources</span></span>
<span data-ttu-id="315a3-222">Du kan få information om skalningsuppsättningar i virtuella datorer på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="315a3-222">You can get some information about virtual machine scale sets using these methods:</span></span>

* <span data-ttu-id="315a3-223">hello Azure-portalen – du får för närvarande en begränsad mängd information med hjälp av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="315a3-223">hello Azure portal - You can currently get a limited amount of information using hello portal.</span></span>
* <span data-ttu-id="315a3-224">Hej [resursutforskaren Azure](https://resources.azure.com/) -verktyget är hello bästa för att utforska hello aktuell status för din skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="315a3-224">hello [Azure Resource Explorer](https://resources.azure.com/) - This tool is hello best for exploring hello current state of your scale set.</span></span> <span data-ttu-id="315a3-225">Följ den här sökvägen och du bör se hello instansvyn för hello skala ange som du skapade:</span><span class="sxs-lookup"><span data-stu-id="315a3-225">Follow this path and you should see hello instance view of hello scale set that you created:</span></span>
  
    <span data-ttu-id="315a3-226">prenumerationer > {din prenumeration} > resursgrupper > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines</span><span class="sxs-lookup"><span data-stu-id="315a3-226">subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines</span></span>

* <span data-ttu-id="315a3-227">Azure PowerShell - och använda det här kommandot tooget viss information:</span><span class="sxs-lookup"><span data-stu-id="315a3-227">Azure PowerShell - Use this command tooget some information:</span></span>

  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  ```
  
  <span data-ttu-id="315a3-228">Eller</span><span class="sxs-lookup"><span data-stu-id="315a3-228">Or</span></span>
  
  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView
  ```

* <span data-ttu-id="315a3-229">Ansluta toohello separat virtuella datorn precis som du skulle någon annan dator och sedan kan du fjärransluta till hello virtuella datorer i hello scale set toomonitor enskilda processer.</span><span class="sxs-lookup"><span data-stu-id="315a3-229">Connect toohello separate virtual machine just like you would any other machine and then you can remotely access hello virtual machines in hello scale set toomonitor individual processes.</span></span>

> [!NOTE]
> <span data-ttu-id="315a3-230">En fullständig REST-API för att få information om skaluppsättningar finns i [Skalningsuppsättningarna för virtuella datorer](https://msdn.microsoft.com/library/mt589023.aspx)</span><span class="sxs-lookup"><span data-stu-id="315a3-230">A complete REST API for obtaining information about scale sets can be found in [Virtual Machine Scale Sets](https://msdn.microsoft.com/library/mt589023.aspx)</span></span>

## <a name="step-7-remove-hello-resources"></a><span data-ttu-id="315a3-231">Steg 7: Ta bort hello resurser</span><span class="sxs-lookup"><span data-stu-id="315a3-231">Step 7: Remove hello resources</span></span>
<span data-ttu-id="315a3-232">Eftersom du debiteras för de resurser som används i Azure, men det är alltid en bra idé toodelete resurser som inte längre behövs.</span><span class="sxs-lookup"><span data-stu-id="315a3-232">Because you are charged for resources used in Azure, it is always a good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="315a3-233">Du behöver inte toodelete varje resurs separat från en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="315a3-233">You don’t need toodelete each resource separately from a resource group.</span></span> <span data-ttu-id="315a3-234">Du kan ta bort hello resursgruppen och alla dess resurser tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="315a3-234">You can delete hello resource group and all its resources are automatically deleted.</span></span>

  ```powershell
  Remove-AzureRmResourceGroup -Name vmsstestrg1
  ```

<span data-ttu-id="315a3-235">Om du vill tookeep resursgruppen kan du ta bort hello skala endast.</span><span class="sxs-lookup"><span data-stu-id="315a3-235">If you want tookeep your resource group, you can delete hello scale set only.</span></span>

  ```powershell
  Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"
  ```

## <a name="next-steps"></a><span data-ttu-id="315a3-236">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="315a3-236">Next steps</span></span>
* <span data-ttu-id="315a3-237">Hantera hello skaluppsättning som du precis skapat med hjälp av hello informationen i [hantera virtuella datorer i en virtuell dator Skaluppsättning](virtual-machine-scale-sets-windows-manage.md).</span><span class="sxs-lookup"><span data-stu-id="315a3-237">Manage hello scale set that you just created using hello information in [Manage virtual machines in a Virtual Machine Scale Set](virtual-machine-scale-sets-windows-manage.md).</span></span>
* <span data-ttu-id="315a3-238">Läs mer om vertikal skalning i [Vertikal automatisk skalning med skaluppsättningar för virtuella datorer](virtual-machine-scale-sets-vertical-scale-reprovision.md)</span><span class="sxs-lookup"><span data-stu-id="315a3-238">Learn more about vertical scaling by reviewing [Vertical autoscale with Virtual Machine Scale sets](virtual-machine-scale-sets-vertical-scale-reprovision.md)</span></span>
* <span data-ttu-id="315a3-239">Hitta exempel på Azure-Monitor övervakningsfunktionerna i [Azure-Monitor PowerShell snabb start prover](../monitoring-and-diagnostics/insights-powershell-samples.md)</span><span class="sxs-lookup"><span data-stu-id="315a3-239">Find examples of Azure Monitor monitoring features in [Azure Monitor PowerShell quick start samples](../monitoring-and-diagnostics/insights-powershell-samples.md)</span></span>
* <span data-ttu-id="315a3-240">Lär dig mer om meddelandefunktioner i [använda automatiska åtgärder toosend e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="315a3-240">Learn about notification features in [Use autoscale actions toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span></span>
* <span data-ttu-id="315a3-241">Lär dig hur för[Använd granskningsloggar toosend e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="315a3-241">Learn how too[Use audit logs toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>

