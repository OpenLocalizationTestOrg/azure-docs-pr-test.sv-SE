---
title: "Autoskala Linux virtuella Skalningsuppsättningarna | Microsoft Docs"
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
ms.openlocfilehash: eff4add1cb16fe25022787668dc1d2277845dd95
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="automatically-scale-linux-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="f1158-103">Skala automatiskt Linux-datorer i en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f1158-103">Automatically scale Linux machines in a virtual machine scale set</span></span>
<span data-ttu-id="f1158-104">Skaluppsättningar för den virtuella datorn gör det enkelt att distribuera och hantera identiska virtuella datorer som en uppsättning.</span><span class="sxs-lookup"><span data-stu-id="f1158-104">Virtual machine scale sets make it easy for you to deploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="f1158-105">Skaluppsättningar ger en skalbar och anpassningsbara beräkningslager för storskaliga program och stöd för avbildningar för Windows-plattformen, Linux-plattformen bilder, anpassade avbildningar och tillägg.</span><span class="sxs-lookup"><span data-stu-id="f1158-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="f1158-106">Läs mer i [översikt över virtuella datorer skala anger](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f1158-106">To learn more, see [Virtual Machine Scale Sets Overview](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="f1158-107">Den här kursen visar hur du skapar en skaluppsättning för Linux virtuella datorer med den senaste versionen av Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="f1158-107">This tutorial shows you how to create a scale set of Linux virtual machines using the latest version of Ubuntu Linux.</span></span> <span data-ttu-id="f1158-108">Kursen visar även hur du skalar automatiskt på datorer i uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f1158-108">The tutorial also shows you how to automatically scale the machines in the set.</span></span> <span data-ttu-id="f1158-109">Du kan skapa skala och skalning genom att skapa en Azure Resource Manager-mall och distribuera den med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f1158-109">You create the scale set and set up scaling by creating an Azure Resource Manager template and deploying it using Azure CLI.</span></span> <span data-ttu-id="f1158-110">Mer information om mallar finns i [Redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f1158-110">For more information about templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="f1158-111">Läs mer om automatisk skalning av skalningsuppsättningar i [automatisk skalning och Skalningsuppsättningarna för virtuella](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f1158-111">To learn more about automatic scaling of scale sets, see [Automatic scaling and Virtual Machine Scale Sets](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

<span data-ttu-id="f1158-112">I kursen får distribuera du följande resurser och tillägg:</span><span class="sxs-lookup"><span data-stu-id="f1158-112">In this tutorial, you deploy the following resources and extensions:</span></span>

* <span data-ttu-id="f1158-113">Microsoft.Storage/storageAccounts</span><span class="sxs-lookup"><span data-stu-id="f1158-113">Microsoft.Storage/storageAccounts</span></span>
* <span data-ttu-id="f1158-114">Microsoft.Network/virtualNetworks</span><span class="sxs-lookup"><span data-stu-id="f1158-114">Microsoft.Network/virtualNetworks</span></span>
* <span data-ttu-id="f1158-115">Microsoft.Network/publicIPAddresses</span><span class="sxs-lookup"><span data-stu-id="f1158-115">Microsoft.Network/publicIPAddresses</span></span>
* <span data-ttu-id="f1158-116">Microsoft.Network/loadBalancers</span><span class="sxs-lookup"><span data-stu-id="f1158-116">Microsoft.Network/loadBalancers</span></span>
* <span data-ttu-id="f1158-117">Microsoft.Network/networkInterfaces</span><span class="sxs-lookup"><span data-stu-id="f1158-117">Microsoft.Network/networkInterfaces</span></span>
* <span data-ttu-id="f1158-118">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="f1158-118">Microsoft.Compute/virtualMachines</span></span>
* <span data-ttu-id="f1158-119">Microsoft.Compute/virtualMachineScaleSets</span><span class="sxs-lookup"><span data-stu-id="f1158-119">Microsoft.Compute/virtualMachineScaleSets</span></span>
* <span data-ttu-id="f1158-120">Microsoft.Insights.VMDiagnosticsSettings</span><span class="sxs-lookup"><span data-stu-id="f1158-120">Microsoft.Insights.VMDiagnosticsSettings</span></span>
* <span data-ttu-id="f1158-121">Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="f1158-121">Microsoft.Insights/autoscaleSettings</span></span>

<span data-ttu-id="f1158-122">Mer information om Resource Manager-resurser finns [Azure Resource Manager och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f1158-122">For more information about Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

<span data-ttu-id="f1158-123">Innan du börjar med stegen i den här självstudiekursen [installerar Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="f1158-123">Before you get started with the steps in this tutorial, [install the Azure CLI](../cli-install-nodejs.md).</span></span>

## <a name="step-1-create-a-resource-group-and-a-storage-account"></a><span data-ttu-id="f1158-124">Steg 1: Skapa en resursgrupp och ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="f1158-124">Step 1: Create a resource group and a storage account</span></span>

1. <span data-ttu-id="f1158-125">**Logga in på Microsoft Azure**</span><span class="sxs-lookup"><span data-stu-id="f1158-125">**Sign in to Microsoft Azure**</span></span>  
<span data-ttu-id="f1158-126">Växla till Resource Manager-läge i din kommandoradsgränssnittet (Bash, Terminal, Kommandotolken) och sedan [logga in med ditt arbets- eller skolkonto id](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Följ anvisningarna för en interaktiv inloggning upplevelse på Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="f1158-126">In your command-line interface (Bash, Terminal, Command prompt), switch to Resource Manager mode, and then [log in with your work or school id](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Follow the prompts for an interactive login experience to your Azure account.</span></span>

    ```cli   
    azure config mode arm

    azure login
    ```
   
    > [!NOTE]
    > <span data-ttu-id="f1158-127">Om du har ett arbets- eller skolkonto ID och du inte har tvåfaktorsautentisering aktiverad, använda `azure login -u` med att logga in utan en interaktiv session-ID.</span><span class="sxs-lookup"><span data-stu-id="f1158-127">If you have a work or school ID and you do not have two-factor authentication enabled, use `azure login -u` with the ID to log in without an interactive session.</span></span> <span data-ttu-id="f1158-128">Om du inte har ett arbets- eller skolkonto ID, kan du [skapar ett arbets- eller skolkonto id från ditt personliga Microsoft-konto](../active-directory/active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f1158-128">If you don't have a work or school ID, you can [create a work or school id from your personal Microsoft account](../active-directory/active-directory-users-create-azure-portal.md).</span></span>
    
2. <span data-ttu-id="f1158-129">**Skapa en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="f1158-129">**Create a resource group**</span></span>  
<span data-ttu-id="f1158-130">Alla resurser måste distribueras till en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f1158-130">All resources must be deployed to a resource group.</span></span> <span data-ttu-id="f1158-131">Den här självstudiekursen kommer du kalla resursgruppen **vmsstest1**.</span><span class="sxs-lookup"><span data-stu-id="f1158-131">For this tutorial, name the resource group **vmsstest1**.</span></span>
   
    ```cli
    azure group create vmsstestrg1 centralus
    ```

3. <span data-ttu-id="f1158-132">**Distribuera ett lagringskonto i den nya resursgruppen**</span><span class="sxs-lookup"><span data-stu-id="f1158-132">**Deploy a storage account into the new resource group**</span></span>  
<span data-ttu-id="f1158-133">Det här lagringskontot är där mallen lagras.</span><span class="sxs-lookup"><span data-stu-id="f1158-133">This storage account is where the template is stored.</span></span> <span data-ttu-id="f1158-134">Skapa ett lagringskonto med namnet **vmsstestsa**.</span><span class="sxs-lookup"><span data-stu-id="f1158-134">Create a storage account named **vmsstestsa**.</span></span>
   
    ```cli
    azure storage account create -g vmsstestrg1 -l centralus --kind Storage --sku-name LRS vmsstestsa
    ```

## <a name="step-2-create-the-template"></a><span data-ttu-id="f1158-135">Steg 2: Skapa mallen</span><span class="sxs-lookup"><span data-stu-id="f1158-135">Step 2: Create the template</span></span>
<span data-ttu-id="f1158-136">En mall för Azure Resource Manager gör det möjligt för dig att distribuera och hantera Azure-resurser tillsammans med hjälp av en JSON-beskrivning av resurser och associerade distributionen parametrar.</span><span class="sxs-lookup"><span data-stu-id="f1158-136">An Azure Resource Manager template makes it possible for you to deploy and manage Azure resources together by using a JSON description of the resources and associated deployment parameters.</span></span>

1. <span data-ttu-id="f1158-137">Skapa filen VMSSTemplate.json i din favorit-redigeraren och Lägg till den ursprungliga JSON-strukturen för att stödja mallen.</span><span class="sxs-lookup"><span data-stu-id="f1158-137">In your favorite editor, create the file VMSSTemplate.json and add the initial JSON structure to support the template.</span></span>

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

2. <span data-ttu-id="f1158-138">Parametrar krävs inte alltid, men de ger ett sätt att ange värden när mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="f1158-138">Parameters are not always required, but they provide a way to input values when the template is deployed.</span></span> <span data-ttu-id="f1158-139">Lägg till de här parametrarna under det överordnade elementet parametrar som du lagt till mallen.</span><span class="sxs-lookup"><span data-stu-id="f1158-139">Add these parameters under the parameters parent element that you added to the template.</span></span>

    ```json
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
   * <span data-ttu-id="f1158-140">Ange ett namn för den separata virtuella dator som används för att få åtkomst till datorer i skalan.</span><span class="sxs-lookup"><span data-stu-id="f1158-140">A name for the separate virtual machine that is used to access the machines in the scale set.</span></span>
   * <span data-ttu-id="f1158-141">Ett namn för det lagringskonto där mallen ska lagras.</span><span class="sxs-lookup"><span data-stu-id="f1158-141">A name for the storage account where the template is stored.</span></span>
   * <span data-ttu-id="f1158-142">Ange antalet instanser av virtuella datorer för att skapa först i skalan.</span><span class="sxs-lookup"><span data-stu-id="f1158-142">The number of instances of virtual machines to initially create in the scale set.</span></span>
   * <span data-ttu-id="f1158-143">Ett namn och lösenord för administratörskontot på de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="f1158-143">A name and password of the administrator account on the virtual machines.</span></span>
   * <span data-ttu-id="f1158-144">Ange ett namnprefix för de resurser som skapas för att stödja skalningen.</span><span class="sxs-lookup"><span data-stu-id="f1158-144">A name prefix for the resources that are created to support the scale set.</span></span>

3. <span data-ttu-id="f1158-145">Variabler kan användas i en mall för att ange värden som ändras ofta eller värden som måste skapas från en kombination av parametervärden.</span><span class="sxs-lookup"><span data-stu-id="f1158-145">Variables can be used in a template to specify values that may change frequently or values that need to be created from a combination of parameter values.</span></span> <span data-ttu-id="f1158-146">Lägg till dessa variabler under det överordnade elementet för variabler som du lagt till mallen.</span><span class="sxs-lookup"><span data-stu-id="f1158-146">Add these variables under the variables parent element that you added to the template.</span></span>

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

   * <span data-ttu-id="f1158-147">DNS-namn som används av nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="f1158-147">DNS names that are used by the network interfaces.</span></span>
   * <span data-ttu-id="f1158-148">IP-adressnamn och prefix för virtuellt nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="f1158-148">The IP address names and prefixes for the virtual network and subnets.</span></span>
   * <span data-ttu-id="f1158-149">Namn och identifierare för det virtuella nätverket att läsa in belastningsutjämning och nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="f1158-149">The names and identifiers of the virtual network, load balancer, and network interfaces.</span></span>
   * <span data-ttu-id="f1158-150">Ange lagringskontonamn för konton som är kopplade till datorer i skalan.</span><span class="sxs-lookup"><span data-stu-id="f1158-150">Storage account names for the accounts associated with the machines in the scale set.</span></span>
   * <span data-ttu-id="f1158-151">Inställningar för diagnostik-tillägg som är installerad på de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="f1158-151">Settings for the Diagnostics extension that is installed on the virtual machines.</span></span> <span data-ttu-id="f1158-152">Läs mer om diagnostik [skapa en Windows virtuell dator med övervakning och diagnostik med Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f1158-152">For more information about the Diagnostics extension, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

4. <span data-ttu-id="f1158-153">Lägg till konto lagringsresurs under det överordnade elementet för resurser som du lagt till mallen.</span><span class="sxs-lookup"><span data-stu-id="f1158-153">Add the storage account resource under the resources parent element that you added to the template.</span></span> <span data-ttu-id="f1158-154">Den här mallen använder en loop för att skapa de rekommenderade fem storage-kontona där operativsystemet diskar och diagnostiska data lagras.</span><span class="sxs-lookup"><span data-stu-id="f1158-154">This template uses a loop to create the recommended five storage accounts where the operating system disks and diagnostic data are stored.</span></span> <span data-ttu-id="f1158-155">Denna uppsättning konton stöder upp till 100 virtuella datorer i en skaluppsättning, vilket är största.</span><span class="sxs-lookup"><span data-stu-id="f1158-155">This set of accounts can support up to 100 virtual machines in a scale set, which is the current maximum.</span></span> <span data-ttu-id="f1158-156">Varje lagringskonto heter med en bokstav enhetsbeteckning som definierades i variablerna kombineras med suffixet som du anger i parametrarna för mallen.</span><span class="sxs-lookup"><span data-stu-id="f1158-156">Each storage account is named with a letter designator that was defined in the variables combined with the suffix that you provide in the parameters for the template.</span></span>
   
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

5. <span data-ttu-id="f1158-157">Lägg till resursen virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f1158-157">Add the virtual network resource.</span></span> <span data-ttu-id="f1158-158">Mer information finns i [Nätverksresursprovidern](../virtual-network/resource-groups-networking.md).</span><span class="sxs-lookup"><span data-stu-id="f1158-158">For more information, see [Network Resource Provider](../virtual-network/resource-groups-networking.md).</span></span>

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

6. <span data-ttu-id="f1158-159">Lägg till de offentliga IP-adress-resurser som används av belastningsutjämnare och nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="f1158-159">Add the public IP address resources that are used by the load balancer and network interface.</span></span>

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

7. <span data-ttu-id="f1158-160">Lägg till belastningsutjämningsresurs som används av skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="f1158-160">Add the load balancer resource that is used by the scale set.</span></span> <span data-ttu-id="f1158-161">Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnaren](../load-balancer/load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="f1158-161">For more information, see [Azure Resource Manager Support for Load Balancer](../load-balancer/load-balancer-arm.md).</span></span>

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

8. <span data-ttu-id="f1158-162">Lägg till gränssnittet nätverksresurs som används av en separat virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f1158-162">Add the network interface resource that is used by the separate virtual machine.</span></span> <span data-ttu-id="f1158-163">Eftersom datorer i en skaluppsättning inte är tillgängligt via en offentlig IP-adress, skapas en separat virtuell dator i samma virtuella nätverk via fjärranslutning åtkomst på datorer.</span><span class="sxs-lookup"><span data-stu-id="f1158-163">Because machines in a scale set aren't accessible through a public IP address, a separate virtual machine is created in the same virtual network to remotely access the machines.</span></span>

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

9. <span data-ttu-id="f1158-164">Lägg till en separat virtuell dator i samma nätverk som skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="f1158-164">Add the separate virtual machine in the same network as the scale set.</span></span>

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

10. <span data-ttu-id="f1158-165">Lägga till virtuella datorns skaluppsättning resurs och ange diagnostik-tillägget som är installerad på alla virtuella datorer i skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="f1158-165">Add the virtual machine scale set resource and specify the diagnostics extension that is installed on all virtual machines in the scale set.</span></span> <span data-ttu-id="f1158-166">Många av inställningarna för den här resursen är liknande med den virtuella datorresursen.</span><span class="sxs-lookup"><span data-stu-id="f1158-166">Many of the settings for this resource are similar with the virtual machine resource.</span></span> <span data-ttu-id="f1158-167">De viktigaste skillnaderna är kapacitet elementet som anger hur många virtuella datorer i skalningsuppsättningen och upgradePolicy som anger hur uppdateringar görs i virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f1158-167">The main differences are the capacity element that specifies the number of virtual machines in the scale set and upgradePolicy that specifies how updates are made to virtual machines.</span></span> <span data-ttu-id="f1158-168">Skaluppsättning skapas inte förrän alla lagringskonton skapas som anges med dependsOn-element.</span><span class="sxs-lookup"><span data-stu-id="f1158-168">The scale set is not created until all the storage accounts are created as specified with the dependsOn element.</span></span>

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

11. <span data-ttu-id="f1158-169">Lägg till resursen autoscaleSettings som definierar hur skaluppsättning justerar baserat på processoranvändning på datorer i uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f1158-169">Add the autoscaleSettings resource that defines how the scale set adjusts based on processor usage on the machines in the set.</span></span>

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
    
    <span data-ttu-id="f1158-170">Dessa värden är viktiga för den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="f1158-170">For this tutorial, these values are important:</span></span>
    
    * <span data-ttu-id="f1158-171">**metricName**</span><span class="sxs-lookup"><span data-stu-id="f1158-171">**metricName**</span></span>  
    <span data-ttu-id="f1158-172">Det här värdet är samma som de prestandaräknare som vi har definierat i variabeln wadperfcounter.</span><span class="sxs-lookup"><span data-stu-id="f1158-172">This value is the same as the performance counter that we defined in the wadperfcounter variable.</span></span> <span data-ttu-id="f1158-173">Med hjälp av variabeln, samlar in diagnostik-tillägget i **Processor\PercentProcessorTime** räknaren.</span><span class="sxs-lookup"><span data-stu-id="f1158-173">Using that variable, the Diagnostics extension collects the **Processor\PercentProcessorTime** counter.</span></span>
    
    * <span data-ttu-id="f1158-174">**metricResourceUri**</span><span class="sxs-lookup"><span data-stu-id="f1158-174">**metricResourceUri**</span></span>  
    <span data-ttu-id="f1158-175">Det här värdet är resursidentifieraren för virtuella datorns skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="f1158-175">This value is the resource identifier of the virtual machine scale set.</span></span>
    
    * <span data-ttu-id="f1158-176">**Tidskorn**</span><span class="sxs-lookup"><span data-stu-id="f1158-176">**timeGrain**</span></span>  
    <span data-ttu-id="f1158-177">Det här värdet är Granulariteten för de mätvärden som samlats in.</span><span class="sxs-lookup"><span data-stu-id="f1158-177">This value is the granularity of the metrics that are collected.</span></span> <span data-ttu-id="f1158-178">I den här mallen är den inställd på en minut.</span><span class="sxs-lookup"><span data-stu-id="f1158-178">In this template, it is set to one minute.</span></span>
    
    * <span data-ttu-id="f1158-179">**statistik**</span><span class="sxs-lookup"><span data-stu-id="f1158-179">**statistic**</span></span>  
    <span data-ttu-id="f1158-180">Det här värdet avgör hur mätvärdena kombineras för automatisk skalning åtgärd.</span><span class="sxs-lookup"><span data-stu-id="f1158-180">This value determines how the metrics are combined to accommodate the automatic scaling action.</span></span> <span data-ttu-id="f1158-181">Möjliga värden är: medel, Min, Max.</span><span class="sxs-lookup"><span data-stu-id="f1158-181">The possible values are: Average, Min, Max.</span></span> <span data-ttu-id="f1158-182">Genomsnittlig totala CPU-användningen av virtuella datorer samlas in i den här mallen.</span><span class="sxs-lookup"><span data-stu-id="f1158-182">In this template, the average total CPU usage of the virtual machines is collected.</span></span>

    * <span data-ttu-id="f1158-183">**värdet timeWindow**</span><span class="sxs-lookup"><span data-stu-id="f1158-183">**timeWindow**</span></span>  
    <span data-ttu-id="f1158-184">Det här värdet är tidsintervallet där instansens data samlas in.</span><span class="sxs-lookup"><span data-stu-id="f1158-184">This value is the range of time in which instance data is collected.</span></span> <span data-ttu-id="f1158-185">Det måste vara mellan 5 minuter och 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="f1158-185">It must be between 5 minutes and 12 hours.</span></span>
    
    * <span data-ttu-id="f1158-186">**timeAggregation**</span><span class="sxs-lookup"><span data-stu-id="f1158-186">**timeAggregation**</span></span>  
    <span data-ttu-id="f1158-187">hans värdet avgör hur data som samlas in ska kombineras med tiden.</span><span class="sxs-lookup"><span data-stu-id="f1158-187">his value determines how the data that is collected should be combined over time.</span></span> <span data-ttu-id="f1158-188">Standardvärdet är medelvärde.</span><span class="sxs-lookup"><span data-stu-id="f1158-188">The default value is Average.</span></span> <span data-ttu-id="f1158-189">Möjliga värden är:, lägsta, högsta, senaste, totalt antal, räknas.</span><span class="sxs-lookup"><span data-stu-id="f1158-189">The possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span>
    
    * <span data-ttu-id="f1158-190">**operatorn**</span><span class="sxs-lookup"><span data-stu-id="f1158-190">**operator**</span></span>  
    <span data-ttu-id="f1158-191">Det här värdet är den operator som används för att jämföra måttinformationen och tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="f1158-191">This value is the operator that is used to compare the metric data and the threshold.</span></span> <span data-ttu-id="f1158-192">Möjliga värden är: är lika med, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="f1158-192">The possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span>
    
    * <span data-ttu-id="f1158-193">**Tröskelvärde**</span><span class="sxs-lookup"><span data-stu-id="f1158-193">**threshold**</span></span>  
    <span data-ttu-id="f1158-194">Det här värdet utlöser skalningsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="f1158-194">This value triggers the scale action.</span></span> <span data-ttu-id="f1158-195">I den här mallen läggs datorer till skaluppsättningen när den genomsnittliga processoranvändningen mellan datorer i uppsättningen är över 50%.</span><span class="sxs-lookup"><span data-stu-id="f1158-195">In this template, machines are added to the scale set when the average CPU usage among machines in the set is over 50%.</span></span>
    
    * <span data-ttu-id="f1158-196">**riktning**</span><span class="sxs-lookup"><span data-stu-id="f1158-196">**direction**</span></span>  
    <span data-ttu-id="f1158-197">Det här värdet anger den åtgärd som utförs när ett tröskelvärde uppnås.</span><span class="sxs-lookup"><span data-stu-id="f1158-197">This value determines the action that is taken when the threshold value is achieved.</span></span> <span data-ttu-id="f1158-198">Möjliga värden är ökning eller minskning.</span><span class="sxs-lookup"><span data-stu-id="f1158-198">The possible values are Increase or Decrease.</span></span> <span data-ttu-id="f1158-199">I den här mallen ökar antalet virtuella datorer i skaluppsättning om tröskelvärdet för över 50% under den definierade tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="f1158-199">In this template, the number of virtual machines in the scale set is increased if the threshold is over 50% in the defined time window.</span></span>

    * <span data-ttu-id="f1158-200">**typ**</span><span class="sxs-lookup"><span data-stu-id="f1158-200">**type**</span></span>  
    <span data-ttu-id="f1158-201">Det här värdet är typ av åtgärd som ska ske och måste anges till ChangeCount.</span><span class="sxs-lookup"><span data-stu-id="f1158-201">This value is the type of action that should occur and must be set to ChangeCount.</span></span>
    
    * <span data-ttu-id="f1158-202">**värdet**</span><span class="sxs-lookup"><span data-stu-id="f1158-202">**value**</span></span>  
    <span data-ttu-id="f1158-203">Det här värdet är antalet virtuella datorer som läggs till eller tas bort från skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="f1158-203">This value is the number of virtual machines that are added or removed from the scale set.</span></span> <span data-ttu-id="f1158-204">Det här värdet måste vara 1 eller högre.</span><span class="sxs-lookup"><span data-stu-id="f1158-204">This value must be 1 or greater.</span></span> <span data-ttu-id="f1158-205">Standardvärdet är 1.</span><span class="sxs-lookup"><span data-stu-id="f1158-205">The default value is 1.</span></span> <span data-ttu-id="f1158-206">I den här mallen ange antalet datorer i skalan ökar med 1 när tröskelvärdet är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="f1158-206">In this template, the number of machines in the scale set increases by 1 when the threshold is met.</span></span>

    * <span data-ttu-id="f1158-207">**cooldown**</span><span class="sxs-lookup"><span data-stu-id="f1158-207">**cooldown**</span></span>  
    <span data-ttu-id="f1158-208">Det här värdet är hur lång tid att vänta sedan den senaste skalning åtgärden innan nästa åtgärd inträffar.</span><span class="sxs-lookup"><span data-stu-id="f1158-208">This value is the amount of time to wait since the last scaling action before the next action occurs.</span></span> <span data-ttu-id="f1158-209">Det här värdet måste vara mellan en minut och en vecka.</span><span class="sxs-lookup"><span data-stu-id="f1158-209">This value must be between one minute and one week.</span></span>

12. <span data-ttu-id="f1158-210">Spara filen med mallen.</span><span class="sxs-lookup"><span data-stu-id="f1158-210">Save the template file.</span></span>    

## <a name="step-3-upload-the-template-to-storage"></a><span data-ttu-id="f1158-211">Steg 3: Överför mallen till lagring</span><span class="sxs-lookup"><span data-stu-id="f1158-211">Step 3: Upload the template to storage</span></span>
<span data-ttu-id="f1158-212">Mallen kan överföras så länge som du känner till namn och en primär nyckel för lagringskontot som du skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="f1158-212">The template can be uploaded as long as you know the name and primary key of the storage account that you created in step 1.</span></span>

1. <span data-ttu-id="f1158-213">Kör dessa kommandon för att ställa in miljövariabler som behövs för att komma åt lagringskontot i din kommandoradsgränssnittet (Bash, Terminal, Kommandotolken):</span><span class="sxs-lookup"><span data-stu-id="f1158-213">In your command-line interface (Bash, Terminal, Command prompt), run these commands to set the environment variables needed to access the storage account:</span></span>

    ```cli   
    export AZURE_STORAGE_ACCOUNT={account_name}
    export AZURE_STORAGE_ACCESS_KEY={key}
    ```
    
    <span data-ttu-id="f1158-214">Du kan hämta nyckeln genom att klicka på nyckelikonen när du visar konto lagringsresurs i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f1158-214">You can get the key by clicking the key icon when viewing the storage account resource in the Azure portal.</span></span> <span data-ttu-id="f1158-215">När du använder en Windows kommandotolk, Skriv **ange** i stället för export.</span><span class="sxs-lookup"><span data-stu-id="f1158-215">When using a Windows command prompt, type **set** instead of export.</span></span>

2. <span data-ttu-id="f1158-216">Skapa behållare för lagring av mallen.</span><span class="sxs-lookup"><span data-stu-id="f1158-216">Create the container for storing the template.</span></span>
   
    ```cli
    azure storage container create -p Blob templates
    ```

3. <span data-ttu-id="f1158-217">Överför mallfilen till den nya behållaren.</span><span class="sxs-lookup"><span data-stu-id="f1158-217">Upload the template file to the new container.</span></span>
   
    ```cli
    azure storage blob upload VMSSTemplate.json templates VMSSTemplate.json
    ```

## <a name="step-4-deploy-the-template"></a><span data-ttu-id="f1158-218">Steg 4: Distribuera mallen</span><span class="sxs-lookup"><span data-stu-id="f1158-218">Step 4: Deploy the template</span></span>
<span data-ttu-id="f1158-219">Nu när du har skapat mallen kan du börja distribuera resurserna.</span><span class="sxs-lookup"><span data-stu-id="f1158-219">Now that you created the template, you can start deploying the resources.</span></span> <span data-ttu-id="f1158-220">Använd följande kommando för att starta processen:</span><span class="sxs-lookup"><span data-stu-id="f1158-220">Use this command to start the process:</span></span>

```cli
azure group deployment create --template-uri https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json vmsstestrg1 vmsstestdp1
```

<span data-ttu-id="f1158-221">När du trycker på Ange, du uppmanas att ange värden för variabler som du tilldelat.</span><span class="sxs-lookup"><span data-stu-id="f1158-221">When you press enter, you are prompted to provide values for the variables you assigned.</span></span> <span data-ttu-id="f1158-222">Ange dessa värden:</span><span class="sxs-lookup"><span data-stu-id="f1158-222">Provide these values:</span></span>

```cli
vmName: vmsstestvm1
vmSSName: vmsstest1
instanceCount: 5
adminUserName: vmadmin1
adminPassword: VMpass1
resourcePrefix: vmsstest
```

<span data-ttu-id="f1158-223">Det bör ta ungefär 15 minuter för alla resurser som har ska distribueras.</span><span class="sxs-lookup"><span data-stu-id="f1158-223">It should take about 15 minutes for all the resources to successfully be deployed.</span></span>

> [!NOTE]
> <span data-ttu-id="f1158-224">Du kan också använda portalens möjlighet för att distribuera resurserna.</span><span class="sxs-lookup"><span data-stu-id="f1158-224">You can also use the portal’s ability to deploy the resources.</span></span> <span data-ttu-id="f1158-225">Använd den här länken: https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template></span><span class="sxs-lookup"><span data-stu-id="f1158-225">Use this link: https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template></span></span>


## <a name="step-5-monitor-resources"></a><span data-ttu-id="f1158-226">Steg 5: Övervaka resurser</span><span class="sxs-lookup"><span data-stu-id="f1158-226">Step 5: Monitor resources</span></span>
<span data-ttu-id="f1158-227">Du kan få information om skalningsuppsättningar i virtuella datorer på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f1158-227">You can get some information about virtual machine scale sets using these methods:</span></span>

* <span data-ttu-id="f1158-228">Azure portal – du får för närvarande en begränsad mängd information med hjälp av portalen.</span><span class="sxs-lookup"><span data-stu-id="f1158-228">The Azure portal - You can currently get a limited amount of information using the portal.</span></span>

* <span data-ttu-id="f1158-229">Den [resursutforskaren Azure](https://resources.azure.com/) – det här verktyget är bäst för att utforska det aktuella tillståndet för din skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="f1158-229">The [Azure Resource Explorer](https://resources.azure.com/) - This tool is the best for exploring the current state of your scale set.</span></span> <span data-ttu-id="f1158-230">Följ den här sökvägen och du bör se Instansvy för skaluppsättning som du skapade:</span><span class="sxs-lookup"><span data-stu-id="f1158-230">Follow this path and you should see the instance view of the scale set that you created:</span></span>
  
    ```cli
    subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines
    ```

* <span data-ttu-id="f1158-231">Azure CLI - Använd detta kommando för att hämta viss information:</span><span class="sxs-lookup"><span data-stu-id="f1158-231">Azure CLI - Use this command to get some information:</span></span>

    ```cli  
    azure resource show -n vmsstest1 -r Microsoft.Compute/virtualMachineScaleSets -o 2015-06-15 -g vmsstestrg1
    ```

* <span data-ttu-id="f1158-232">Ansluta till den virtuella datorn jumpbox precis som du skulle någon annan dator och sedan kan du fjärransluta till de virtuella datorerna i skaluppsättningen att övervaka enskilda processer.</span><span class="sxs-lookup"><span data-stu-id="f1158-232">Connect to the jumpbox virtual machine just like you would any other machine and then you can remotely access the virtual machines in the scale set to monitor individual processes.</span></span>

> [!NOTE]
> <span data-ttu-id="f1158-233">En fullständig REST-API för att få information om skaluppsättningar finns i [Skalningsuppsättningarna för virtuella](https://msdn.microsoft.com/library/mt589023.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1158-233">A complete REST API for obtaining information about scale sets can be found in [Virtual Machine Scale Sets](https://msdn.microsoft.com/library/mt589023.aspx).</span></span>

## <a name="step-6-remove-the-resources"></a><span data-ttu-id="f1158-234">Steg 6: Ta bort resurserna</span><span class="sxs-lookup"><span data-stu-id="f1158-234">Step 6: Remove the resources</span></span>
<span data-ttu-id="f1158-235">Eftersom du debiteras för de resurser som används i Azure, men det är alltid en bra idé att ta bort resurser som inte längre behövs.</span><span class="sxs-lookup"><span data-stu-id="f1158-235">Because you are charged for resources used in Azure, it is always a good practice to delete resources that are no longer needed.</span></span> <span data-ttu-id="f1158-236">Du behöver inte ta bort varje resurs separat från en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f1158-236">You don’t need to delete each resource separately from a resource group.</span></span> <span data-ttu-id="f1158-237">Du kan ta bort resursgruppen och alla dess resurser tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f1158-237">You can delete the resource group and all its resources are automatically deleted.</span></span>

```cli
azure group delete vmsstestrg1
```

## <a name="next-steps"></a><span data-ttu-id="f1158-238">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1158-238">Next steps</span></span>
* <span data-ttu-id="f1158-239">Hitta exempel på Azure-Monitor övervakningsfunktionerna i [Azure-Monitor plattformsoberoende CLI snabb start prover](../monitoring-and-diagnostics/insights-cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="f1158-239">Find examples of Azure Monitor monitoring features in [Azure Monitor Cross-platform CLI quick start samples](../monitoring-and-diagnostics/insights-cli-samples.md)</span></span>
* <span data-ttu-id="f1158-240">Lär dig mer om meddelandefunktioner i [använda automatiska åtgärder för att skicka e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="f1158-240">Learn about notification features in [Use autoscale actions to send email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span></span>
* <span data-ttu-id="f1158-241">Lär dig hur du [Använd granskningsloggarna för att skicka e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="f1158-241">Learn how to [Use audit logs to send email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>
* <span data-ttu-id="f1158-242">Kolla in den [Autoskala demo-appen på Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) mall som ställer in en Python/bottle app att utöva automatisk skalning funktionerna i Skalningsuppsättningarna för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f1158-242">Check out the [Autoscale demo app on Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) template that sets up a Python/bottle app to exercise the automatic scaling functionality of Virtual Machine Scale Sets.</span></span>

