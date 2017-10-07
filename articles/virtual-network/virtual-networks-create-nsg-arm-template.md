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
# <a name="create-network-security-groups-using-an-azure-resource-manager-template"></a><span data-ttu-id="7eeab-103">Skapa nätverk säkerhetsgrupper med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="7eeab-103">Create network security groups using an Azure Resource Manager template</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="7eeab-104">Den här artikeln beskriver hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="7eeab-104">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="7eeab-105">Du kan också [skapa NSG: er i hello klassiska distributionsmodellen](virtual-networks-create-nsg-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="7eeab-105">You can also [create NSGs in hello classic deployment model](virtual-networks-create-nsg-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a><span data-ttu-id="7eeab-106">NSG-resurser i en mallfil</span><span class="sxs-lookup"><span data-stu-id="7eeab-106">NSG resources in a template file</span></span>
<span data-ttu-id="7eeab-107">Du kan visa och hämta hello [exempelmall](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).</span><span class="sxs-lookup"><span data-stu-id="7eeab-107">You can view and download hello [sample template](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).</span></span>

<span data-ttu-id="7eeab-108">hello följande avsnitt visar hello definition av hello frontend NSG, baserat på hello scenario.</span><span class="sxs-lookup"><span data-stu-id="7eeab-108">hello following section shows hello definition of hello front-end NSG, based on hello scenario.</span></span>

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
<span data-ttu-id="7eeab-109">tooassociate hello NSG toohello frontend-undernät du har toochange hello undernätsdefinition i hello mallen och Använd hello referens-id för hello NSG.</span><span class="sxs-lookup"><span data-stu-id="7eeab-109">tooassociate hello NSG toohello front-end subnet, you have toochange hello subnet definition in hello template, and use hello reference id for hello NSG.</span></span>

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

<span data-ttu-id="7eeab-110">Observera hello samma som utfördes för hello backend-NSG och hello backend-undernät i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="7eeab-110">Notice hello same being done for hello back-end NSG and hello back-end subnet in hello template.</span></span>

## <a name="deploy-hello-arm-template-by-using-click-toodeploy"></a><span data-ttu-id="7eeab-111">Distribuera hello ARM-mallen med hjälp av klickar du på toodeploy</span><span class="sxs-lookup"><span data-stu-id="7eeab-111">Deploy hello ARM template by using click toodeploy</span></span>
<span data-ttu-id="7eeab-112">hello exempelmall tillgängliga i hello offentliga databasen använder en parameterfil som innehåller hello standard värden som används för toogenerate hello scenario som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="7eeab-112">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="7eeab-113">toodeploy den här mallen med hjälp av klickar du på toodeploy, Följ [länken](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), klickar du på **distribuera tooAzure**, ersätta hello Standardparametervärden om det behövs och följ instruktionerna för hello hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="7eeab-113">toodeploy this template using click toodeploy, follow [this link](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

## <a name="deploy-hello-arm-template-by-using-powershell"></a><span data-ttu-id="7eeab-114">Distribuera hello ARM-mallen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="7eeab-114">Deploy hello ARM template by using PowerShell</span></span>
<span data-ttu-id="7eeab-115">toodeploy hello ARM-mallen som du hämtade med PowerShell, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="7eeab-115">toodeploy hello ARM template you downloaded by using PowerShell, follow hello steps below.</span></span>

1. <span data-ttu-id="7eeab-116">Om du aldrig har använt Azure PowerShell, följer du anvisningarna för hello i hello [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) tooinstall och konfigurera den.</span><span class="sxs-lookup"><span data-stu-id="7eeab-116">If you have never used Azure PowerShell, follow hello instructions in hello [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) tooinstall and configure it.</span></span>
2. <span data-ttu-id="7eeab-117">Kör hello  **`New-AzureRmResourceGroup`**  cmdlet toocreate en resurs med hello mallen.</span><span class="sxs-lookup"><span data-stu-id="7eeab-117">Run hello **`New-AzureRmResourceGroup`** cmdlet toocreate a resource group using hello template.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location uswest `
    -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
    -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    <span data-ttu-id="7eeab-118">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="7eeab-118">Expected output:</span></span>

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

## <a name="deploy-hello-arm-template-by-using-hello-azure-cli"></a><span data-ttu-id="7eeab-119">Distribuera hello ARM-mallen med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7eeab-119">Deploy hello ARM template by using hello Azure CLI</span></span>
<span data-ttu-id="7eeab-120">toodeploy hello ARM-mallen med hjälp av hello Azure CLI, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="7eeab-120">toodeploy hello ARM template by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="7eeab-121">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7eeab-121">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="7eeab-122">Kör hello  **`azure config mode`**  kommandot tooswitch tooResource Manager-läge enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="7eeab-122">Run hello **`azure config mode`** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="7eeab-123">hello följer hello förväntades utdata för hello-kommandot:</span><span class="sxs-lookup"><span data-stu-id="7eeab-123">hello following is hello expected output for hello command:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="7eeab-124">Kör hello  **`azure group deployment create`**  cmdlet toodeploy hello nya VNet med hjälp av hello mallen och parametern filer du hämtade och ändrade ovan.</span><span class="sxs-lookup"><span data-stu-id="7eeab-124">Run hello **`azure group deployment create`** cmdlet toodeploy hello new VNet by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="7eeab-125">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="7eeab-125">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    <span data-ttu-id="7eeab-126">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="7eeab-126">Expected output:</span></span>
   
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
   
   * <span data-ttu-id="7eeab-127">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="7eeab-127">**-n (or --name)**.</span></span> <span data-ttu-id="7eeab-128">Namnet på hello resurs grupp toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="7eeab-128">Name of hello resource group toobe created.</span></span>
   * <span data-ttu-id="7eeab-129">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="7eeab-129">**-l (or --location)**.</span></span> <span data-ttu-id="7eeab-130">Azure-region där resursgruppen hello kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="7eeab-130">Azure region where hello resource group will be created.</span></span>
   * <span data-ttu-id="7eeab-131">**-f (eller --template-file)**.</span><span class="sxs-lookup"><span data-stu-id="7eeab-131">**-f (or --template-file)**.</span></span> <span data-ttu-id="7eeab-132">Sökvägen tooyour ARM-mallfil.</span><span class="sxs-lookup"><span data-stu-id="7eeab-132">Path tooyour ARM template file.</span></span>
   * <span data-ttu-id="7eeab-133">**-e (eller --parameters-file)**.</span><span class="sxs-lookup"><span data-stu-id="7eeab-133">**-e (or --parameters-file)**.</span></span> <span data-ttu-id="7eeab-134">Sökvägen tooyour ARM-parameterfil.</span><span class="sxs-lookup"><span data-stu-id="7eeab-134">Path tooyour ARM parameters file.</span></span>

