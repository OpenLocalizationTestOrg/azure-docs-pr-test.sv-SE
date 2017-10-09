---
title: "aaaAutoscale Skalningsuppsättningar i virtuella Linux | Microsoft Docs"
description: "Ställa in autoskalning för en Linux Skalningsuppsättning virtuell dator med hjälp av Azure CLI"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 83e93d9c-cac0-41d3-8316-6016f5ed0ce4
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: 4352b94ec2973c37bc5616e3be25bd0c9442632b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-scale-linux-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="9b628-103">Skala automatiskt Linux-datorer i en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9b628-103">Automatically scale Linux machines in a virtual machine scale set</span></span>
<span data-ttu-id="9b628-104">Skaluppsättningar för den virtuella datorn gör det lättare för dig toodeploy och hantera identiska virtuella datorer som en uppsättning.</span><span class="sxs-lookup"><span data-stu-id="9b628-104">Virtual machine scale sets make it easy for you toodeploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="9b628-105">Skaluppsättningar ger en skalbar och anpassningsbara beräkningslager för storskaliga program och stöd för avbildningar för Windows-plattformen, Linux-plattformen bilder, anpassade avbildningar och tillägg.</span><span class="sxs-lookup"><span data-stu-id="9b628-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="9b628-106">Det finns fler toolearn [översikt över virtuella datorer skala anger](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9b628-106">toolearn more, see [Virtual Machine Scale Sets Overview](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="9b628-107">Den här kursen visar hur toocreate en skala uppsättning hello senaste versionen av Ubuntu Linux virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="9b628-107">This tutorial shows you how toocreate a scale set of Linux virtual machines using hello latest version of Ubuntu Linux.</span></span> <span data-ttu-id="9b628-108">hello kursen visar också hur tooautomatically skala hello datorer i hello anges.</span><span class="sxs-lookup"><span data-stu-id="9b628-108">hello tutorial also shows you how tooautomatically scale hello machines in hello set.</span></span> <span data-ttu-id="9b628-109">Du kan skapa hello skala och skalning genom att skapa en Azure Resource Manager-mall och distribuera den med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9b628-109">You create hello scale set and set up scaling by creating an Azure Resource Manager template and deploying it using Azure CLI.</span></span> <span data-ttu-id="9b628-110">Mer information om mallar finns i [Redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9b628-110">For more information about templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="9b628-111">toolearn mer om automatisk skalning av skalningsuppsättningar, se [automatisk skalning och Skalningsuppsättningarna för virtuella](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9b628-111">toolearn more about automatic scaling of scale sets, see [Automatic scaling and Virtual Machine Scale Sets](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

<span data-ttu-id="9b628-112">I kursen får du distribuera hello följande resurser och tillägg:</span><span class="sxs-lookup"><span data-stu-id="9b628-112">In this tutorial, you deploy hello following resources and extensions:</span></span>

* <span data-ttu-id="9b628-113">Microsoft.Storage/storageAccounts</span><span class="sxs-lookup"><span data-stu-id="9b628-113">Microsoft.Storage/storageAccounts</span></span>
* <span data-ttu-id="9b628-114">Microsoft.Network/virtualNetworks</span><span class="sxs-lookup"><span data-stu-id="9b628-114">Microsoft.Network/virtualNetworks</span></span>
* <span data-ttu-id="9b628-115">Microsoft.Network/publicIPAddresses</span><span class="sxs-lookup"><span data-stu-id="9b628-115">Microsoft.Network/publicIPAddresses</span></span>
* <span data-ttu-id="9b628-116">Microsoft.Network/loadBalancers</span><span class="sxs-lookup"><span data-stu-id="9b628-116">Microsoft.Network/loadBalancers</span></span>
* <span data-ttu-id="9b628-117">Microsoft.Network/networkInterfaces</span><span class="sxs-lookup"><span data-stu-id="9b628-117">Microsoft.Network/networkInterfaces</span></span>
* <span data-ttu-id="9b628-118">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="9b628-118">Microsoft.Compute/virtualMachines</span></span>
* <span data-ttu-id="9b628-119">Microsoft.Compute/virtualMachineScaleSets</span><span class="sxs-lookup"><span data-stu-id="9b628-119">Microsoft.Compute/virtualMachineScaleSets</span></span>
* <span data-ttu-id="9b628-120">Microsoft.Insights.VMDiagnosticsSettings</span><span class="sxs-lookup"><span data-stu-id="9b628-120">Microsoft.Insights.VMDiagnosticsSettings</span></span>
* <span data-ttu-id="9b628-121">Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="9b628-121">Microsoft.Insights/autoscaleSettings</span></span>

<span data-ttu-id="9b628-122">Mer information om Resource Manager-resurser finns [Azure Resource Manager och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9b628-122">For more information about Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

<span data-ttu-id="9b628-123">Innan du börjar med hello steg i den här självstudiekursen [installera hello Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="9b628-123">Before you get started with hello steps in this tutorial, [install hello Azure CLI](../cli-install-nodejs.md).</span></span>

## <a name="step-1-create-a-resource-group-and-a-storage-account"></a><span data-ttu-id="9b628-124">Steg 1: Skapa en resursgrupp och ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="9b628-124">Step 1: Create a resource group and a storage account</span></span>

1. <span data-ttu-id="9b628-125">**Logga in tooMicrosoft Azure**</span><span class="sxs-lookup"><span data-stu-id="9b628-125">**Sign in tooMicrosoft Azure**</span></span>  
<span data-ttu-id="9b628-126">Växla tooResource Manager läge i din kommandoradsgränssnittet (Bash, Terminal, Kommandotolken) och sedan [logga in med ditt arbets- eller skolkonto id](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Följ hello efterfrågas en interaktiv inloggning upplevelse tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="9b628-126">In your command-line interface (Bash, Terminal, Command prompt), switch tooResource Manager mode, and then [log in with your work or school id](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Follow hello prompts for an interactive login experience tooyour Azure account.</span></span>

    ```cli   
    azure config mode arm

    azure login
    ```
   
    > [!NOTE]
    > <span data-ttu-id="9b628-127">Om du har ett arbets- eller skolkonto ID och du inte har tvåfaktorsautentisering aktiverad, använda `azure login -u` med hello ID toolog i utan en interaktiv session.</span><span class="sxs-lookup"><span data-stu-id="9b628-127">If you have a work or school ID and you do not have two-factor authentication enabled, use `azure login -u` with hello ID toolog in without an interactive session.</span></span> <span data-ttu-id="9b628-128">Om du inte har ett arbets- eller skolkonto ID, kan du [skapar ett arbets- eller skolkonto id från ditt personliga Microsoft-konto](../active-directory/active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9b628-128">If you don't have a work or school ID, you can [create a work or school id from your personal Microsoft account](../active-directory/active-directory-users-create-azure-portal.md).</span></span>
    
2. <span data-ttu-id="9b628-129">**Skapa en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="9b628-129">**Create a resource group**</span></span>  
<span data-ttu-id="9b628-130">Alla resurser måste vara distribuerad tooa resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9b628-130">All resources must be deployed tooa resource group.</span></span> <span data-ttu-id="9b628-131">Namn för den här självstudiekursen hello resursgruppen **vmsstest1**.</span><span class="sxs-lookup"><span data-stu-id="9b628-131">For this tutorial, name hello resource group **vmsstest1**.</span></span>
   
    ```cli
    azure group create vmsstestrg1 centralus
    ```

3. <span data-ttu-id="9b628-132">**Distribuera ett lagringskonto i hello ny resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="9b628-132">**Deploy a storage account into hello new resource group**</span></span>  
<span data-ttu-id="9b628-133">Det här lagringskontot är där hello mallen lagras.</span><span class="sxs-lookup"><span data-stu-id="9b628-133">This storage account is where hello template is stored.</span></span> <span data-ttu-id="9b628-134">Skapa ett lagringskonto med namnet **vmsstestsa**.</span><span class="sxs-lookup"><span data-stu-id="9b628-134">Create a storage account named **vmsstestsa**.</span></span>
   
    ```cli
    azure storage account create -g vmsstestrg1 -l centralus --kind Storage --sku-name LRS vmsstestsa
    ```

## <a name="step-2-create-hello-template"></a><span data-ttu-id="9b628-135">Steg 2: Skapa hello mall</span><span class="sxs-lookup"><span data-stu-id="9b628-135">Step 2: Create hello template</span></span>
<span data-ttu-id="9b628-136">En mall för Azure Resource Manager gör det möjligt för du toodeploy och hantera Azure-resurser tillsammans med hjälp av en JSON-beskrivning av hello resurser och associerade distributionen parametrar.</span><span class="sxs-lookup"><span data-stu-id="9b628-136">An Azure Resource Manager template makes it possible for you toodeploy and manage Azure resources together by using a JSON description of hello resources and associated deployment parameters.</span></span>

1. <span data-ttu-id="9b628-137">Skapa hello fil VMSSTemplate.json i din favorit-redigeraren och lägga till hello inledande JSON strukturen toosupport hello mallen.</span><span class="sxs-lookup"><span data-stu-id="9b628-137">In your favorite editor, create hello file VMSSTemplate.json and add hello initial JSON structure toosupport hello template.</span></span>

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

2. <span data-ttu-id="9b628-138">Parametrar krävs inte alltid, men de ger ett sätt tooinput som värden när hello mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="9b628-138">Parameters are not always required, but they provide a way tooinput values when hello template is deployed.</span></span> <span data-ttu-id="9b628-139">Lägg till de här parametrarna under hello parametrar överordnade element som du lagt till toohello mall.</span><span class="sxs-lookup"><span data-stu-id="9b628-139">Add these parameters under hello parameters parent element that you added toohello template.</span></span>

    ```json
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
   * <span data-ttu-id="9b628-140">Ange ett namn för hello separat virtuell dator som används tooaccess hello datorer i hello skala.</span><span class="sxs-lookup"><span data-stu-id="9b628-140">A name for hello separate virtual machine that is used tooaccess hello machines in hello scale set.</span></span>
   * <span data-ttu-id="9b628-141">Ett namn för hello lagringskonto där hello mall ska lagras.</span><span class="sxs-lookup"><span data-stu-id="9b628-141">A name for hello storage account where hello template is stored.</span></span>
   * <span data-ttu-id="9b628-142">hello antal instanser av virtuella datorer tooinitially skapa i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="9b628-142">hello number of instances of virtual machines tooinitially create in hello scale set.</span></span>
   * <span data-ttu-id="9b628-143">Ett namn och lösenord för administratörskontot för hello på hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9b628-143">A name and password of hello administrator account on hello virtual machines.</span></span>
   * <span data-ttu-id="9b628-144">Ange ett namnprefix för hello-resurser som har skapats toosupport hello skala.</span><span class="sxs-lookup"><span data-stu-id="9b628-144">A name prefix for hello resources that are created toosupport hello scale set.</span></span>

3. <span data-ttu-id="9b628-145">Variabler kan användas i en mall toospecify värden som ändras ofta eller värden som behöver toobe som skapats från en kombination av parametervärden.</span><span class="sxs-lookup"><span data-stu-id="9b628-145">Variables can be used in a template toospecify values that may change frequently or values that need toobe created from a combination of parameter values.</span></span> <span data-ttu-id="9b628-146">Lägg till dessa variabler under hello variabler överordnade element som du lagt till toohello mall.</span><span class="sxs-lookup"><span data-stu-id="9b628-146">Add these variables under hello variables parent element that you added toohello template.</span></span>

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
    "wadlogs": "<WadCfg><DiagnosticMonitorConfiguration>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor\\PercentProcessorTime\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU percentage guest OS\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```

   * <span data-ttu-id="9b628-147">DNS-namn som används av hello nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="9b628-147">DNS names that are used by hello network interfaces.</span></span>
   * <span data-ttu-id="9b628-148">hello IP-adressnamn och prefix för hello virtuella nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="9b628-148">hello IP address names and prefixes for hello virtual network and subnets.</span></span>
   * <span data-ttu-id="9b628-149">hello namn och identifierare för hello virtuellt nätverk, läsa in belastningsutjämning och nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="9b628-149">hello names and identifiers of hello virtual network, load balancer, and network interfaces.</span></span>
   * <span data-ttu-id="9b628-150">Lagringskontonamn för hello konton som är associerade med hello datorer i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="9b628-150">Storage account names for hello accounts associated with hello machines in hello scale set.</span></span>
   * <span data-ttu-id="9b628-151">Inställningar för hello diagnostik-tillägg som är installerad på hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9b628-151">Settings for hello Diagnostics extension that is installed on hello virtual machines.</span></span> <span data-ttu-id="9b628-152">Läs mer om hello diagnostik tillägget [skapa en Windows virtuell dator med övervakning och diagnostik med Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9b628-152">For more information about hello Diagnostics extension, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

4. <span data-ttu-id="9b628-153">Lägg till hello konto lagringsresurs under hello resurser överordnade element som du lagt till toohello mall.</span><span class="sxs-lookup"><span data-stu-id="9b628-153">Add hello storage account resource under hello resources parent element that you added toohello template.</span></span> <span data-ttu-id="9b628-154">Den här mallen använder en loop toocreate hello rekommenderas fem storage-konton där hello operativsystemet diskar och diagnostiska data lagras.</span><span class="sxs-lookup"><span data-stu-id="9b628-154">This template uses a loop toocreate hello recommended five storage accounts where hello operating system disks and diagnostic data are stored.</span></span> <span data-ttu-id="9b628-155">Denna uppsättning konton kan stödja av too100 virtuella datorer i en skaluppsättning hello aktuella högsta.</span><span class="sxs-lookup"><span data-stu-id="9b628-155">This set of accounts can support up too100 virtual machines in a scale set, which is hello current maximum.</span></span> <span data-ttu-id="9b628-156">Varje lagringskonto heter med en bokstav enhetsbeteckning som definierades i hello variabler i kombination med hello-suffix som du anger i hello parametrar för hello mall.</span><span class="sxs-lookup"><span data-stu-id="9b628-156">Each storage account is named with a letter designator that was defined in hello variables combined with hello suffix that you provide in hello parameters for hello template.</span></span>
   
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

5. <span data-ttu-id="9b628-157">Lägg till resurs för hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="9b628-157">Add hello virtual network resource.</span></span> <span data-ttu-id="9b628-158">Mer information finns i [Nätverksresursprovidern](../virtual-network/resource-groups-networking.md).</span><span class="sxs-lookup"><span data-stu-id="9b628-158">For more information, see [Network Resource Provider](../virtual-network/resource-groups-networking.md).</span></span>

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

6. <span data-ttu-id="9b628-159">Lägg till hello offentliga IP-adress-resurser som används av hello läsa in belastningsutjämning och nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="9b628-159">Add hello public IP address resources that are used by hello load balancer and network interface.</span></span>

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

7. <span data-ttu-id="9b628-160">Lägg till hello belastningsutjämningsresurs som används av hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="9b628-160">Add hello load balancer resource that is used by hello scale set.</span></span> <span data-ttu-id="9b628-161">Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnaren](../load-balancer/load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="9b628-161">For more information, see [Azure Resource Manager Support for Load Balancer](../load-balancer/load-balancer-arm.md).</span></span>

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
                "id": "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
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
              "backendPort": 22
            }
          }
        ]
      }
    },
    ```

8. <span data-ttu-id="9b628-162">Lägg till hello gränssnittet nätverksresurs som används av hello separat virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="9b628-162">Add hello network interface resource that is used by hello separate virtual machine.</span></span> <span data-ttu-id="9b628-163">Eftersom datorer i en skaluppsättning inte är tillgängligt via en offentlig IP-adress, en separat virtuell dator skapas i hello samma virtuella nätverk tooremotely åtkomst hello datorer.</span><span class="sxs-lookup"><span data-stu-id="9b628-163">Because machines in a scale set aren't accessible through a public IP address, a separate virtual machine is created in hello same virtual network tooremotely access hello machines.</span></span>

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

9. <span data-ttu-id="9b628-164">Lägg till hello separat virtuell dator i samma nätverk som hello skaluppsättning hello.</span><span class="sxs-lookup"><span data-stu-id="9b628-164">Add hello separate virtual machine in hello same network as hello scale set.</span></span>

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
            "publisher": "Canonical",
            "offer": "UbuntuServer",
            "sku": "14.04.4-LTS",
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

10. <span data-ttu-id="9b628-165">Lägga till hello virtuella skala resursen och ange hello diagnostik tillägg som är installerad på alla virtuella datorer i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="9b628-165">Add hello virtual machine scale set resource and specify hello diagnostics extension that is installed on all virtual machines in hello scale set.</span></span> <span data-ttu-id="9b628-166">Många av hello inställningar för den här resursen är liknande med hello virtuella datorresurser.</span><span class="sxs-lookup"><span data-stu-id="9b628-166">Many of hello settings for this resource are similar with hello virtual machine resource.</span></span> <span data-ttu-id="9b628-167">hello viktigaste skillnaderna är hello kapacitet element som anger hello antalet virtuella datorer i hello skaluppsättning och upgradePolicy som anger hur uppdateringar görs toovirtual datorer.</span><span class="sxs-lookup"><span data-stu-id="9b628-167">hello main differences are hello capacity element that specifies hello number of virtual machines in hello scale set and upgradePolicy that specifies how updates are made toovirtual machines.</span></span> <span data-ttu-id="9b628-168">hello skaluppsättning inte skapas förrän alla hello storage-konton skapas som anges med hello dependsOn-element.</span><span class="sxs-lookup"><span data-stu-id="9b628-168">hello scale set is not created until all hello storage accounts are created as specified with hello dependsOn element.</span></span>

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
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4],'.blob.core.windows.net/vmss')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "Canonical",
              "offer": "UbuntuServer",
              "sku": "14.04.4-LTS",
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
                "name":"LinuxDiagnostic",
                "properties": {
                  "publisher":"Microsoft.OSTCExtensions",
                  "type":"LinuxDiagnostic",
                  "typeHandlerVersion":"2.1",
                  "autoUpgradeMinorVersion":false,
                  "settings": {
                    "xmlCfg":"[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount":"[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName":"[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey":"[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint":"https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. <span data-ttu-id="9b628-169">Lägga till hello autoscaleSettings resurs som definierar hur hello skaluppsättning justerar baserat på processoranvändning på hello datorer i hello uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="9b628-169">Add hello autoscaleSettings resource that defines how hello scale set adjusts based on processor usage on hello machines in hello set.</span></span>

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
                  "metricName": "\\Processor\\PercentProcessorTime",
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
    
    <span data-ttu-id="9b628-170">Dessa värden är viktiga för den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="9b628-170">For this tutorial, these values are important:</span></span>
    
    * <span data-ttu-id="9b628-171">**metricName**</span><span class="sxs-lookup"><span data-stu-id="9b628-171">**metricName**</span></span>  
    <span data-ttu-id="9b628-172">Det här värdet är hello samma som hello prestandaräknare som vi har definierat i hello wadperfcounter variabeln.</span><span class="sxs-lookup"><span data-stu-id="9b628-172">This value is hello same as hello performance counter that we defined in hello wadperfcounter variable.</span></span> <span data-ttu-id="9b628-173">Använda den variabeln hello diagnostik tillägget samlar in hello **Processor\PercentProcessorTime** räknaren.</span><span class="sxs-lookup"><span data-stu-id="9b628-173">Using that variable, hello Diagnostics extension collects hello **Processor\PercentProcessorTime** counter.</span></span>
    
    * <span data-ttu-id="9b628-174">**metricResourceUri**</span><span class="sxs-lookup"><span data-stu-id="9b628-174">**metricResourceUri**</span></span>  
    <span data-ttu-id="9b628-175">Det här värdet är hello resursidentifieraren för hello skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="9b628-175">This value is hello resource identifier of hello virtual machine scale set.</span></span>
    
    * <span data-ttu-id="9b628-176">**Tidskorn**</span><span class="sxs-lookup"><span data-stu-id="9b628-176">**timeGrain**</span></span>  
    <span data-ttu-id="9b628-177">Det här värdet är hello Granulariteten för hello mått som har samlats in.</span><span class="sxs-lookup"><span data-stu-id="9b628-177">This value is hello granularity of hello metrics that are collected.</span></span> <span data-ttu-id="9b628-178">I den här mallen anges tooone minut.</span><span class="sxs-lookup"><span data-stu-id="9b628-178">In this template, it is set tooone minute.</span></span>
    
    * <span data-ttu-id="9b628-179">**statistik**</span><span class="sxs-lookup"><span data-stu-id="9b628-179">**statistic**</span></span>  
    <span data-ttu-id="9b628-180">Det här värdet avgör hur hello är kombinerade tooaccommodate hello autoskalning åtgärd.</span><span class="sxs-lookup"><span data-stu-id="9b628-180">This value determines how hello metrics are combined tooaccommodate hello automatic scaling action.</span></span> <span data-ttu-id="9b628-181">hello möjliga värden är: medel, Min, Max.</span><span class="sxs-lookup"><span data-stu-id="9b628-181">hello possible values are: Average, Min, Max.</span></span> <span data-ttu-id="9b628-182">Hello genomsnittlig total CPU-användning av hello virtuella datorer samlas in i den här mallen.</span><span class="sxs-lookup"><span data-stu-id="9b628-182">In this template, hello average total CPU usage of hello virtual machines is collected.</span></span>

    * <span data-ttu-id="9b628-183">**värdet timeWindow**</span><span class="sxs-lookup"><span data-stu-id="9b628-183">**timeWindow**</span></span>  
    <span data-ttu-id="9b628-184">Det här värdet är hello tidsintervall som samlas in instansdata.</span><span class="sxs-lookup"><span data-stu-id="9b628-184">This value is hello range of time in which instance data is collected.</span></span> <span data-ttu-id="9b628-185">Det måste vara mellan 5 minuter och 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="9b628-185">It must be between 5 minutes and 12 hours.</span></span>
    
    * <span data-ttu-id="9b628-186">**timeAggregation**</span><span class="sxs-lookup"><span data-stu-id="9b628-186">**timeAggregation**</span></span>  
    <span data-ttu-id="9b628-187">hans värdet avgör hur hello data som samlas in ska kombineras med tiden.</span><span class="sxs-lookup"><span data-stu-id="9b628-187">his value determines how hello data that is collected should be combined over time.</span></span> <span data-ttu-id="9b628-188">hello standardvärdet är medelvärdet.</span><span class="sxs-lookup"><span data-stu-id="9b628-188">hello default value is Average.</span></span> <span data-ttu-id="9b628-189">hello möjliga värden är:, lägsta, högsta, senaste, totalt antal, räknas.</span><span class="sxs-lookup"><span data-stu-id="9b628-189">hello possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span>
    
    * <span data-ttu-id="9b628-190">**operatorn**</span><span class="sxs-lookup"><span data-stu-id="9b628-190">**operator**</span></span>  
    <span data-ttu-id="9b628-191">Det här värdet är hello operator som används toocompare hello mått data och hello tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="9b628-191">This value is hello operator that is used toocompare hello metric data and hello threshold.</span></span> <span data-ttu-id="9b628-192">hello möjliga värden är: är lika med, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="9b628-192">hello possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span>
    
    * <span data-ttu-id="9b628-193">**Tröskelvärde**</span><span class="sxs-lookup"><span data-stu-id="9b628-193">**threshold**</span></span>  
    <span data-ttu-id="9b628-194">Det här värdet utlöser hello skalningsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="9b628-194">This value triggers hello scale action.</span></span> <span data-ttu-id="9b628-195">I den här mallen läggs datorer toohello skaluppsättningen när hello genomsnittliga processoranvändningen mellan datorer i hello uppsättningen är över 50%.</span><span class="sxs-lookup"><span data-stu-id="9b628-195">In this template, machines are added toohello scale set when hello average CPU usage among machines in hello set is over 50%.</span></span>
    
    * <span data-ttu-id="9b628-196">**riktning**</span><span class="sxs-lookup"><span data-stu-id="9b628-196">**direction**</span></span>  
    <span data-ttu-id="9b628-197">Det här värdet fastställer hello vad som händer när hello tröskelvärde uppnås.</span><span class="sxs-lookup"><span data-stu-id="9b628-197">This value determines hello action that is taken when hello threshold value is achieved.</span></span> <span data-ttu-id="9b628-198">hello möjliga värden är ökning eller minskning.</span><span class="sxs-lookup"><span data-stu-id="9b628-198">hello possible values are Increase or Decrease.</span></span> <span data-ttu-id="9b628-199">I den här mallen ökas hello antalet virtuella datorer i hello skaluppsättning om hello tröskelvärdet över 50% under hello definierade tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="9b628-199">In this template, hello number of virtual machines in hello scale set is increased if hello threshold is over 50% in hello defined time window.</span></span>

    * <span data-ttu-id="9b628-200">**typ**</span><span class="sxs-lookup"><span data-stu-id="9b628-200">**type**</span></span>  
    <span data-ttu-id="9b628-201">Det här värdet är hello typ av åtgärd som ska ske och tooChangeCount måste anges.</span><span class="sxs-lookup"><span data-stu-id="9b628-201">This value is hello type of action that should occur and must be set tooChangeCount.</span></span>
    
    * <span data-ttu-id="9b628-202">**värdet**</span><span class="sxs-lookup"><span data-stu-id="9b628-202">**value**</span></span>  
    <span data-ttu-id="9b628-203">Det här värdet är hello antalet virtuella datorer som läggs till eller tas bort från hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="9b628-203">This value is hello number of virtual machines that are added or removed from hello scale set.</span></span> <span data-ttu-id="9b628-204">Det här värdet måste vara 1 eller högre.</span><span class="sxs-lookup"><span data-stu-id="9b628-204">This value must be 1 or greater.</span></span> <span data-ttu-id="9b628-205">hello standardvärdet är 1.</span><span class="sxs-lookup"><span data-stu-id="9b628-205">hello default value is 1.</span></span> <span data-ttu-id="9b628-206">I den här mallen hello antalet datorer i hello skala in ökar med 1 när hello tröskelvärdet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="9b628-206">In this template, hello number of machines in hello scale set increases by 1 when hello threshold is met.</span></span>

    * <span data-ttu-id="9b628-207">**cooldown**</span><span class="sxs-lookup"><span data-stu-id="9b628-207">**cooldown**</span></span>  
    <span data-ttu-id="9b628-208">Det här värdet är hello mängden tid toowait sedan hello senaste skalning åtgärd innan hello nästa åtgärd sker.</span><span class="sxs-lookup"><span data-stu-id="9b628-208">This value is hello amount of time toowait since hello last scaling action before hello next action occurs.</span></span> <span data-ttu-id="9b628-209">Det här värdet måste vara mellan en minut och en vecka.</span><span class="sxs-lookup"><span data-stu-id="9b628-209">This value must be between one minute and one week.</span></span>

12. <span data-ttu-id="9b628-210">Spara filen med hello mallen.</span><span class="sxs-lookup"><span data-stu-id="9b628-210">Save hello template file.</span></span>    

## <a name="step-3-upload-hello-template-toostorage"></a><span data-ttu-id="9b628-211">Steg 3: Överför hello mall toostorage</span><span class="sxs-lookup"><span data-stu-id="9b628-211">Step 3: Upload hello template toostorage</span></span>
<span data-ttu-id="9b628-212">hello mall kan överföras så länge som du känner hello namn och primärnyckel för hello storage-konto som du skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="9b628-212">hello template can be uploaded as long as you know hello name and primary key of hello storage account that you created in step 1.</span></span>

1. <span data-ttu-id="9b628-213">I din kommandoradsgränssnittet (Bash, Terminal, Kommandotolken) köra dessa kommandon tooset hello miljövariabler behövs tooaccess hello storage-konto:</span><span class="sxs-lookup"><span data-stu-id="9b628-213">In your command-line interface (Bash, Terminal, Command prompt), run these commands tooset hello environment variables needed tooaccess hello storage account:</span></span>

    ```cli   
    export AZURE_STORAGE_ACCOUNT={account_name}
    export AZURE_STORAGE_ACCESS_KEY={key}
    ```
    
    <span data-ttu-id="9b628-214">Du kan hämta hello nyckel genom att klicka på nyckelikonen hello när du visar hello konto lagringsresurs i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9b628-214">You can get hello key by clicking hello key icon when viewing hello storage account resource in hello Azure portal.</span></span> <span data-ttu-id="9b628-215">När du använder en Windows kommandotolk, Skriv **ange** i stället för export.</span><span class="sxs-lookup"><span data-stu-id="9b628-215">When using a Windows command prompt, type **set** instead of export.</span></span>

2. <span data-ttu-id="9b628-216">Skapa hello behållare för att lagra hello mall.</span><span class="sxs-lookup"><span data-stu-id="9b628-216">Create hello container for storing hello template.</span></span>
   
    ```cli
    azure storage container create -p Blob templates
    ```

3. <span data-ttu-id="9b628-217">Överför hello mallen filen toohello ny behållare.</span><span class="sxs-lookup"><span data-stu-id="9b628-217">Upload hello template file toohello new container.</span></span>
   
    ```cli
    azure storage blob upload VMSSTemplate.json templates VMSSTemplate.json
    ```

## <a name="step-4-deploy-hello-template"></a><span data-ttu-id="9b628-218">Steg 4: Distribuera hello mall</span><span class="sxs-lookup"><span data-stu-id="9b628-218">Step 4: Deploy hello template</span></span>
<span data-ttu-id="9b628-219">Nu när du har skapat hello mallen börja du distribuera hello resurser.</span><span class="sxs-lookup"><span data-stu-id="9b628-219">Now that you created hello template, you can start deploying hello resources.</span></span> <span data-ttu-id="9b628-220">Använd den här processen med kommandot toostart hello:</span><span class="sxs-lookup"><span data-stu-id="9b628-220">Use this command toostart hello process:</span></span>

```cli
azure group deployment create --template-uri https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json vmsstestrg1 vmsstestdp1
```

<span data-ttu-id="9b628-221">När du trycker på Ange är du tillfrågas tooprovide värden för hello variabler som du tilldelat.</span><span class="sxs-lookup"><span data-stu-id="9b628-221">When you press enter, you are prompted tooprovide values for hello variables you assigned.</span></span> <span data-ttu-id="9b628-222">Ange dessa värden:</span><span class="sxs-lookup"><span data-stu-id="9b628-222">Provide these values:</span></span>

```cli
vmName: vmsstestvm1
vmSSName: vmsstest1
instanceCount: 5
adminUserName: vmadmin1
adminPassword: VMpass1
resourcePrefix: vmsstest
```

<span data-ttu-id="9b628-223">Det bör ta ungefär 15 minuter för alla hello resurser toosuccessfully distribueras.</span><span class="sxs-lookup"><span data-stu-id="9b628-223">It should take about 15 minutes for all hello resources toosuccessfully be deployed.</span></span>

> [!NOTE]
> <span data-ttu-id="9b628-224">Du kan också använda hello portal möjlighet toodeploy hello resurser.</span><span class="sxs-lookup"><span data-stu-id="9b628-224">You can also use hello portal’s ability toodeploy hello resources.</span></span> <span data-ttu-id="9b628-225">Använd den här länken: https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template></span><span class="sxs-lookup"><span data-stu-id="9b628-225">Use this link: https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template></span></span>


## <a name="step-5-monitor-resources"></a><span data-ttu-id="9b628-226">Steg 5: Övervaka resurser</span><span class="sxs-lookup"><span data-stu-id="9b628-226">Step 5: Monitor resources</span></span>
<span data-ttu-id="9b628-227">Du kan få information om skalningsuppsättningar i virtuella datorer på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9b628-227">You can get some information about virtual machine scale sets using these methods:</span></span>

* <span data-ttu-id="9b628-228">hello Azure-portalen – du får för närvarande en begränsad mängd information med hjälp av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="9b628-228">hello Azure portal - You can currently get a limited amount of information using hello portal.</span></span>

* <span data-ttu-id="9b628-229">Hej [resursutforskaren Azure](https://resources.azure.com/) -verktyget är hello bästa för att utforska hello aktuell status för din skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="9b628-229">hello [Azure Resource Explorer](https://resources.azure.com/) - This tool is hello best for exploring hello current state of your scale set.</span></span> <span data-ttu-id="9b628-230">Följ den här sökvägen och du bör se hello instansvyn för hello skala ange som du skapade:</span><span class="sxs-lookup"><span data-stu-id="9b628-230">Follow this path and you should see hello instance view of hello scale set that you created:</span></span>
  
    ```cli
    subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines
    ```

* <span data-ttu-id="9b628-231">Azure CLI - och använda det här kommandot tooget viss information:</span><span class="sxs-lookup"><span data-stu-id="9b628-231">Azure CLI - Use this command tooget some information:</span></span>

    ```cli  
    azure resource show -n vmsstest1 -r Microsoft.Compute/virtualMachineScaleSets -o 2015-06-15 -g vmsstestrg1
    ```

* <span data-ttu-id="9b628-232">Ansluta toohello jumpbox virtuella datorn precis som du skulle någon annan dator och sedan kan du fjärransluta till hello virtuella datorer i hello scale set toomonitor enskilda processer.</span><span class="sxs-lookup"><span data-stu-id="9b628-232">Connect toohello jumpbox virtual machine just like you would any other machine and then you can remotely access hello virtual machines in hello scale set toomonitor individual processes.</span></span>

> [!NOTE]
> <span data-ttu-id="9b628-233">En fullständig REST-API för att få information om skaluppsättningar finns i [Skalningsuppsättningarna för virtuella](https://msdn.microsoft.com/library/mt589023.aspx).</span><span class="sxs-lookup"><span data-stu-id="9b628-233">A complete REST API for obtaining information about scale sets can be found in [Virtual Machine Scale Sets](https://msdn.microsoft.com/library/mt589023.aspx).</span></span>

## <a name="step-6-remove-hello-resources"></a><span data-ttu-id="9b628-234">Steg 6: Ta bort hello resurser</span><span class="sxs-lookup"><span data-stu-id="9b628-234">Step 6: Remove hello resources</span></span>
<span data-ttu-id="9b628-235">Eftersom du debiteras för de resurser som används i Azure, men det är alltid en bra idé toodelete resurser som inte längre behövs.</span><span class="sxs-lookup"><span data-stu-id="9b628-235">Because you are charged for resources used in Azure, it is always a good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="9b628-236">Du behöver inte toodelete varje resurs separat från en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="9b628-236">You don’t need toodelete each resource separately from a resource group.</span></span> <span data-ttu-id="9b628-237">Du kan ta bort hello resursgruppen och alla dess resurser tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="9b628-237">You can delete hello resource group and all its resources are automatically deleted.</span></span>

```cli
azure group delete vmsstestrg1
```

## <a name="next-steps"></a><span data-ttu-id="9b628-238">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9b628-238">Next steps</span></span>
* <span data-ttu-id="9b628-239">Hitta exempel på Azure-Monitor övervakningsfunktionerna i [Azure-Monitor plattformsoberoende CLI snabb start prover](../monitoring-and-diagnostics/insights-cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="9b628-239">Find examples of Azure Monitor monitoring features in [Azure Monitor Cross-platform CLI quick start samples](../monitoring-and-diagnostics/insights-cli-samples.md)</span></span>
* <span data-ttu-id="9b628-240">Lär dig mer om meddelandefunktioner i [använda automatiska åtgärder toosend e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="9b628-240">Learn about notification features in [Use autoscale actions toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span></span>
* <span data-ttu-id="9b628-241">Lär dig hur för[Använd granskningsloggar toosend e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="9b628-241">Learn how too[Use audit logs toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>
* <span data-ttu-id="9b628-242">Kolla in hello [Autoskala demo-appen på Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) mall som ställer in en Python/bottle app tooexercise hello autoskalning funktionerna i Skalningsuppsättningarna för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9b628-242">Check out hello [Autoscale demo app on Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) template that sets up a Python/bottle app tooexercise hello automatic scaling functionality of Virtual Machine Scale Sets.</span></span>

