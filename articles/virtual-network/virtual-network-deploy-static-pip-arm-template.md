---
title: Skapa en virtuell dator med en statisk offentlig IP-adress - Azure Resource Manager-mall | Microsoft Docs
description: "Lär dig hur du skapar en virtuell dator med en statisk offentlig IP-adress med en Azure Resource Manager-mall."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d551085a-c7ed-4ec6-b4c3-e9e1cebb774c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2f503aa60fdd9b7cf66ef482a1041e34c88e5c01
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-an-azure-resource-manager-template"></a><span data-ttu-id="bd062-103">Skapa en virtuell dator med en statisk offentlig IP-adress med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="bd062-103">Create a VM with a static public IP address using an Azure Resource Manager template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="bd062-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bd062-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="bd062-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bd062-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="bd062-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bd062-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="bd062-107">Mall</span><span class="sxs-lookup"><span data-stu-id="bd062-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="bd062-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="bd062-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="bd062-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="bd062-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="bd062-110">Den här artikeln täcker distributionsmodell hanteraren för filserverresurser, som Microsoft rekommenderar för de flesta nya distributioner i stället för den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="bd062-110">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-address-resources-in-a-template-file"></a><span data-ttu-id="bd062-111">Offentliga IP-adress-resurser i en mallfil</span><span class="sxs-lookup"><span data-stu-id="bd062-111">Public IP address resources in a template file</span></span>
<span data-ttu-id="bd062-112">Du kan visa och ladda ned den [exempelmall](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="bd062-112">You can view and download the [sample template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).</span></span>

<span data-ttu-id="bd062-113">I följande avsnitt beskrivs definitionen av den offentliga IP-resurs, baserat på scenariot ovan:</span><span class="sxs-lookup"><span data-stu-id="bd062-113">The following section shows the definition of the public IP resource, based on the scenario above:</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('webVMSetting').pipName]",
  "location": "[variables('location')]",
  "properties": {
    "publicIPAllocationMethod": "Static"
  },
  "tags": {
    "displayName": "PublicIPAddress - Web"
  }
},
```

<span data-ttu-id="bd062-114">Observera den **publicIPAllocationMethod** egenskapen som har angetts till *statiska*.</span><span class="sxs-lookup"><span data-stu-id="bd062-114">Notice the **publicIPAllocationMethod** property, which is set to *Static*.</span></span> <span data-ttu-id="bd062-115">Den här egenskapen kan vara antingen *dynamiska* (standardvärdet) eller *statiska*.</span><span class="sxs-lookup"><span data-stu-id="bd062-115">This property can be either *Dynamic* (default value) or *Static*.</span></span> <span data-ttu-id="bd062-116">Ange värdet till statisk garanterar att den offentliga IP-adress ska aldrig ändras.</span><span class="sxs-lookup"><span data-stu-id="bd062-116">Setting it to static guarantees that the public IP address assigned will never change.</span></span>

<span data-ttu-id="bd062-117">I följande avsnitt beskrivs associering av offentlig IP-adress med ett nätverksgränssnitt:</span><span class="sxs-lookup"><span data-stu-id="bd062-117">The following section shows the association of the public IP address with a network interface:</span></span>

```json
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('webVMSetting').nicName]",
    "location": "[variables('location')]",
    "tags": {
    "displayName": "NetworkInterface - Web"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
      "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
    ],
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
          "privateIPAllocationMethod": "Static",
          "privateIPAddress": "[variables('webVMSetting').ipAddress]",
          "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
          },
          "subnet": {
            "id": "[variables('frontEndSubnetRef')]"
          }
        }
      }
    ]
  }
},
```

<span data-ttu-id="bd062-118">Meddelande i **publicIPAddress** egenskap som pekar på den **Id** för en resurs med namnet **variables('webVMSetting').pipName**.</span><span class="sxs-lookup"><span data-stu-id="bd062-118">Notice the **publicIPAddress** property pointing to the **Id** of a resource named **variables('webVMSetting').pipName**.</span></span> <span data-ttu-id="bd062-119">Det är namnet på den offentliga IP-resurs som visas ovan.</span><span class="sxs-lookup"><span data-stu-id="bd062-119">That is the name of the public IP resource shown above.</span></span>

<span data-ttu-id="bd062-120">Slutligen nätverksgränssnittet ovan visas i den **networkProfile** -egenskapen för den virtuella datorn skapas.</span><span class="sxs-lookup"><span data-stu-id="bd062-120">Finally, the network interface above is listed in the **networkProfile** property of the VM being created.</span></span>

```json
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }
```

## <a name="deploy-the-template-by-using-click-to-deploy"></a><span data-ttu-id="bd062-121">Distribuera mallen genom att klicka för att distribuera</span><span class="sxs-lookup"><span data-stu-id="bd062-121">Deploy the template by using click to deploy</span></span>

<span data-ttu-id="bd062-122">Exempelmallen som är tillgänglig i den offentliga databasen använder en parameterfil som innehåller standardvärdena som används för att generera scenariot som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="bd062-122">The sample template available in the public repository uses a parameter file containing the default values used to generate the scenario described above.</span></span> <span data-ttu-id="bd062-123">Om du vill distribuera den här mallen med Klicka för att distribuera, klickar du på **till Azure** i Readme.md-filen för den [virtuell dator med statiska PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) mall.</span><span class="sxs-lookup"><span data-stu-id="bd062-123">To deploy this template using click to deploy, click **Deploy to Azure** in the Readme.md file for the [VM with static PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) template.</span></span> <span data-ttu-id="bd062-124">Ersätt parametern standardvärden om du vill och ange värden för parametrarna tomt.</span><span class="sxs-lookup"><span data-stu-id="bd062-124">Replace the default parameter values if desired and enter values for the blank parameters.</span></span>  <span data-ttu-id="bd062-125">Följ instruktionerna i portalen för att skapa en virtuell dator med en statisk offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="bd062-125">Follow the instructions in the portal to create a virtual machine with a static public IP address.</span></span>

## <a name="deploy-the-template-by-using-powershell"></a><span data-ttu-id="bd062-126">Distribuera mallen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="bd062-126">Deploy the template by using PowerShell</span></span>

<span data-ttu-id="bd062-127">Följ stegen nedan om du vill distribuera mallen som du hämtat med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bd062-127">To deploy the template you downloaded by using PowerShell, follow the steps below.</span></span>

1. <span data-ttu-id="bd062-128">Om du aldrig har använt Azure PowerShell kan du slutföra stegen i den [installera och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.</span><span class="sxs-lookup"><span data-stu-id="bd062-128">If you have never used Azure PowerShell, complete the steps in the [How to Install and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="bd062-129">Kör i PowerShell-konsolen på `New-AzureRmResourceGroup` för att skapa en ny resursgrupp om det behövs.</span><span class="sxs-lookup"><span data-stu-id="bd062-129">In a PowerShell console, run the `New-AzureRmResourceGroup` cmdlet to create a new resource group, if necessary.</span></span> <span data-ttu-id="bd062-130">Om du redan har en resursgrupp som skapats går du till steg 3.</span><span class="sxs-lookup"><span data-stu-id="bd062-130">If you already have a resource group created, go to step 3.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name PIPTEST -Location westus
    ```

    <span data-ttu-id="bd062-131">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="bd062-131">Expected output:</span></span>
   
        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. <span data-ttu-id="bd062-132">Kör i PowerShell-konsolen på `New-AzureRmResourceGroupDeployment` för att distribuera mallen.</span><span class="sxs-lookup"><span data-stu-id="bd062-132">In a PowerShell console, run the `New-AzureRmResourceGroupDeployment` cmdlet to deploy the template.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
        -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
        -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json
    ```

    <span data-ttu-id="bd062-133">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="bd062-133">Expected output:</span></span>
   
        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0
   
        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         
   
        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a><span data-ttu-id="bd062-134">Distribuera mallen med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bd062-134">Deploy the template by using the Azure CLI</span></span>
<span data-ttu-id="bd062-135">Om du vill distribuera mallen med hjälp av Azure CLI, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="bd062-135">To deploy the template by using the Azure CLI, complete the following steps:</span></span>

1. <span data-ttu-id="bd062-136">Om du aldrig har använt Azure CLI, följer du stegen i den [installera och konfigurera Azure CLI](../cli-install-nodejs.md) artikel för att installera och konfigurera den.</span><span class="sxs-lookup"><span data-stu-id="bd062-136">If you have never used Azure CLI, follow the steps in the [Install and Configure the Azure CLI](../cli-install-nodejs.md) article to install and configure it.</span></span>
2. <span data-ttu-id="bd062-137">Kör den `azure config mode` kommando för att växla till Resource Manager-läge enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="bd062-137">Run the `azure config mode` command to switch to Resource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="bd062-138">Förväntad utdata för det ovanstående kommandot:</span><span class="sxs-lookup"><span data-stu-id="bd062-138">The expected output for the command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="bd062-139">Öppna den [parameterfilen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), Välj dess innehåll och spara den till en fil i datorn.</span><span class="sxs-lookup"><span data-stu-id="bd062-139">Open the [parameter file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), select its content, and save it to a file in your computer.</span></span> <span data-ttu-id="bd062-140">I det här exemplet parametrarna sparas i en fil med namnet *parameters.json*.</span><span class="sxs-lookup"><span data-stu-id="bd062-140">For this example, the parameters are saved to a file named *parameters.json*.</span></span> <span data-ttu-id="bd062-141">Ändra parametervärden i filen om du vill, men minst bör du ändra värdet för parametern adminPassword till ett unikt, komplexa lösenord.</span><span class="sxs-lookup"><span data-stu-id="bd062-141">Change the parameter values within the file if desired, but at a minimum, it's recommended that you change the value for the adminPassword parameter to a unique, complex password.</span></span>
4. <span data-ttu-id="bd062-142">Kör den `azure group deployment create` cmd att distribuera det nya VNet med hjälp av mallen och parametern-filer du hämtade och ändrade ovan.</span><span class="sxs-lookup"><span data-stu-id="bd062-142">Run the `azure group deployment create` cmd to deploy the new VNet by using the template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="bd062-143">I kommandot nedan ersätter <path> med sökvägen som du sparade filen till.</span><span class="sxs-lookup"><span data-stu-id="bd062-143">In the command below, replace <path> with the path you saved the file to.</span></span> 

    ```azurecli
    azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json
    ```

    <span data-ttu-id="bd062-144">Förväntad utdata (visar parametervärden används):</span><span class="sxs-lookup"><span data-stu-id="bd062-144">Expected output (lists parameter values used):</span></span>

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

