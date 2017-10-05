---
title: "Skapa nätverkssäkerhetsgrupper - Azure Resource Manager-mall | Microsoft Docs"
description: "Lär dig hur du skapar och distribuerar nätverkssäkerhetsgrupper med en Azure Resource Manager-mall."
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
ms.openlocfilehash: 88f7e5b2144daee7bf1c8e7312ba98e6fa967899
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-network-security-groups-using-an-azure-resource-manager-template"></a><span data-ttu-id="98bbd-103">Skapa nätverk säkerhetsgrupper med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="98bbd-103">Create network security groups using an Azure Resource Manager template</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="98bbd-104">Den här artikeln beskriver Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="98bbd-104">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="98bbd-105">Du kan också [skapa NSG: er i den klassiska distributionsmodellen](virtual-networks-create-nsg-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="98bbd-105">You can also [create NSGs in the classic deployment model](virtual-networks-create-nsg-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a><span data-ttu-id="98bbd-106">NSG-resurser i en mallfil</span><span class="sxs-lookup"><span data-stu-id="98bbd-106">NSG resources in a template file</span></span>
<span data-ttu-id="98bbd-107">Du kan visa och ladda ned den [exempelmall](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).</span><span class="sxs-lookup"><span data-stu-id="98bbd-107">You can view and download the [sample template](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).</span></span>

<span data-ttu-id="98bbd-108">Följande avsnitt innehåller definitionen av frontend NSG: N, baserat på scenariot.</span><span class="sxs-lookup"><span data-stu-id="98bbd-108">The following section shows the definition of the front-end NSG, based on the scenario.</span></span>

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
<span data-ttu-id="98bbd-109">Om du vill associera NSG frontend-undernät som du behöver ändra definitionen för undernätet i mallen och använda referens-id för NSG: N.</span><span class="sxs-lookup"><span data-stu-id="98bbd-109">To associate the NSG to the front-end subnet, you have to change the subnet definition in the template, and use the reference id for the NSG.</span></span>

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

<span data-ttu-id="98bbd-110">Lägg märke till samma som utfördes för NSG för backend- och backend-undernät i mallen.</span><span class="sxs-lookup"><span data-stu-id="98bbd-110">Notice the same being done for the back-end NSG and the back-end subnet in the template.</span></span>

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a><span data-ttu-id="98bbd-111">Distribuera ARM-mallen med klicka för att distribuera</span><span class="sxs-lookup"><span data-stu-id="98bbd-111">Deploy the ARM template by using click to deploy</span></span>
<span data-ttu-id="98bbd-112">Exempelmallen som är tillgänglig i den offentliga databasen använder en parameterfil som innehåller standardvärdena som används för att generera scenariot som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="98bbd-112">The sample template available in the public repository uses a parameter file containing the default values used to generate the scenario described above.</span></span> <span data-ttu-id="98bbd-113">Om du vill distribuera den här mallen genom att klicka för att distribuera följer du [den här länken](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), klickar på **Deploy to Azure** (Distribuera till Azure), ersätter standardparametervärdena om det behövs och följer anvisningarna på portalen.</span><span class="sxs-lookup"><span data-stu-id="98bbd-113">To deploy this template using click to deploy, follow [this link](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>

## <a name="deploy-the-arm-template-by-using-powershell"></a><span data-ttu-id="98bbd-114">Distribuera ARM-mallen med PowerShell</span><span class="sxs-lookup"><span data-stu-id="98bbd-114">Deploy the ARM template by using PowerShell</span></span>
<span data-ttu-id="98bbd-115">Följ stegen nedan för att distribuera ARM-mallen som du hämtade med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="98bbd-115">To deploy the ARM template you downloaded by using PowerShell, follow the steps below.</span></span>

1. <span data-ttu-id="98bbd-116">Om du aldrig har använt Azure PowerShell, följ instruktionerna i den [hur installera och konfigurera Azure PowerShell](/powershell/azure/overview) att installera och konfigurera den.</span><span class="sxs-lookup"><span data-stu-id="98bbd-116">If you have never used Azure PowerShell, follow the instructions in the [How to Install and Configure Azure PowerShell](/powershell/azure/overview) to install and configure it.</span></span>
2. <span data-ttu-id="98bbd-117">Kör den  **`New-AzureRmResourceGroup`**  för att skapa en resursgrupp med hjälp av mallen.</span><span class="sxs-lookup"><span data-stu-id="98bbd-117">Run the **`New-AzureRmResourceGroup`** cmdlet to create a resource group using the template.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location uswest `
    -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
    -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    <span data-ttu-id="98bbd-118">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="98bbd-118">Expected output:</span></span>

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

## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a><span data-ttu-id="98bbd-119">Distribuera ARM-mallen med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="98bbd-119">Deploy the ARM template by using the Azure CLI</span></span>
<span data-ttu-id="98bbd-120">Följ stegen nedan om du vill distribuera ARM-mallen med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="98bbd-120">To deploy the ARM template by using the Azure CLI, follow the steps below.</span></span>

1. <span data-ttu-id="98bbd-121">Om du aldrig har använt Azure CLI, se [installera och konfigurera Azure CLI](../cli-install-nodejs.md) och följ instruktionerna upp till den punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="98bbd-121">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="98bbd-122">Kör kommandot **`azure config mode`** för att byta till Resource Manager-läge enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="98bbd-122">Run the **`azure config mode`** command to switch to Resource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="98bbd-123">Följande är utdata som förväntas för kommandot:</span><span class="sxs-lookup"><span data-stu-id="98bbd-123">The following is the expected output for the command:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="98bbd-124">Kör cmdleten **`azure group deployment create`** för att distribuera det nya VNet med hjälp av mall- och parameterfilerna som du hämtade och ändrade ovan.</span><span class="sxs-lookup"><span data-stu-id="98bbd-124">Run the **`azure group deployment create`** cmdlet to deploy the new VNet by using the template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="98bbd-125">Listan som visas efter alla utdata förklarar parametrarna som använts.</span><span class="sxs-lookup"><span data-stu-id="98bbd-125">The list shown after the output explains the parameters used.</span></span>

    ```azurecli
    azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    <span data-ttu-id="98bbd-126">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="98bbd-126">Expected output:</span></span>
   
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
   
   * <span data-ttu-id="98bbd-127">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="98bbd-127">**-n (or --name)**.</span></span> <span data-ttu-id="98bbd-128">Namnet på resursgruppen som ska skapas.</span><span class="sxs-lookup"><span data-stu-id="98bbd-128">Name of the resource group to be created.</span></span>
   * <span data-ttu-id="98bbd-129">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="98bbd-129">**-l (or --location)**.</span></span> <span data-ttu-id="98bbd-130">Azure-region där resursgruppen kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="98bbd-130">Azure region where the resource group will be created.</span></span>
   * <span data-ttu-id="98bbd-131">**-f (eller --template-file)**.</span><span class="sxs-lookup"><span data-stu-id="98bbd-131">**-f (or --template-file)**.</span></span> <span data-ttu-id="98bbd-132">Sökväg till din ARM-mallfil.</span><span class="sxs-lookup"><span data-stu-id="98bbd-132">Path to your ARM template file.</span></span>
   * <span data-ttu-id="98bbd-133">**-e (eller --parameters-file)**.</span><span class="sxs-lookup"><span data-stu-id="98bbd-133">**-e (or --parameters-file)**.</span></span> <span data-ttu-id="98bbd-134">Sökväg till din ARM-parameterfil.</span><span class="sxs-lookup"><span data-stu-id="98bbd-134">Path to your ARM parameters file.</span></span>

