---
title: aaaCreate en virtuell dator med en statisk offentlig IP-adress - Azure Resource Manager-mall | Microsoft Docs
description: "Lär dig hur toocreate en virtuell dator med en statisk offentlig IP-adress med en Azure Resource Manager-mall."
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
ms.openlocfilehash: 6a8640ed4fad06b0e09820e6114fd6789db73847
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-an-azure-resource-manager-template"></a><span data-ttu-id="95399-103">Skapa en virtuell dator med en statisk offentlig IP-adress med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="95399-103">Create a VM with a static public IP address using an Azure Resource Manager template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="95399-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="95399-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="95399-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="95399-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="95399-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="95399-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="95399-107">Mall</span><span class="sxs-lookup"><span data-stu-id="95399-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="95399-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="95399-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="95399-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="95399-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="95399-110">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="95399-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-address-resources-in-a-template-file"></a><span data-ttu-id="95399-111">Offentliga IP-adress-resurser i en mallfil</span><span class="sxs-lookup"><span data-stu-id="95399-111">Public IP address resources in a template file</span></span>
<span data-ttu-id="95399-112">Du kan visa och hämta hello [exempelmall](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="95399-112">You can view and download hello [sample template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).</span></span>

<span data-ttu-id="95399-113">hello visar följande avsnitt hello definition av hello offentliga IP-resurs, baserat på hello scenariot ovan:</span><span class="sxs-lookup"><span data-stu-id="95399-113">hello following section shows hello definition of hello public IP resource, based on hello scenario above:</span></span>

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

<span data-ttu-id="95399-114">Meddelande hello **publicIPAllocationMethod** -egenskap som har angetts för*statiska*.</span><span class="sxs-lookup"><span data-stu-id="95399-114">Notice hello **publicIPAllocationMethod** property, which is set too*Static*.</span></span> <span data-ttu-id="95399-115">Den här egenskapen kan vara antingen *dynamiska* (standardvärdet) eller *statiska*.</span><span class="sxs-lookup"><span data-stu-id="95399-115">This property can be either *Dynamic* (default value) or *Static*.</span></span> <span data-ttu-id="95399-116">Ange värdet toostatic garanterar att hello offentliga IP-adress ska aldrig ändras.</span><span class="sxs-lookup"><span data-stu-id="95399-116">Setting it toostatic guarantees that hello public IP address assigned will never change.</span></span>

<span data-ttu-id="95399-117">hello visar följande avsnitt hello associering av hello offentlig IP-adress med ett nätverksgränssnitt:</span><span class="sxs-lookup"><span data-stu-id="95399-117">hello following section shows hello association of hello public IP address with a network interface:</span></span>

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

<span data-ttu-id="95399-118">Meddelande hello **publicIPAddress** egenskapen pekar toohello **Id** för en resurs med namnet **variables('webVMSetting').pipName**.</span><span class="sxs-lookup"><span data-stu-id="95399-118">Notice hello **publicIPAddress** property pointing toohello **Id** of a resource named **variables('webVMSetting').pipName**.</span></span> <span data-ttu-id="95399-119">Som är hello namn hello offentliga IP-resurs som visas ovan.</span><span class="sxs-lookup"><span data-stu-id="95399-119">That is hello name of hello public IP resource shown above.</span></span>

<span data-ttu-id="95399-120">Slutligen hello nätverksgränssnittet ovan visas i hello **networkProfile** -egenskapen för hello VM håller på att skapas.</span><span class="sxs-lookup"><span data-stu-id="95399-120">Finally, hello network interface above is listed in hello **networkProfile** property of hello VM being created.</span></span>

```json
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }
```

## <a name="deploy-hello-template-by-using-click-toodeploy"></a><span data-ttu-id="95399-121">Distribuera hello mallen med Klicka toodeploy</span><span class="sxs-lookup"><span data-stu-id="95399-121">Deploy hello template by using click toodeploy</span></span>

<span data-ttu-id="95399-122">hello exempelmall tillgängliga i hello offentliga databasen använder en parameterfil som innehåller hello standard värden som används för toogenerate hello scenario som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="95399-122">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="95399-123">toodeploy den här mallen med hjälp av klickar du på toodeploy, klickar du på **distribuera tooAzure** i hello Readme.md-filen för hello [virtuell dator med statiska PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) mall.</span><span class="sxs-lookup"><span data-stu-id="95399-123">toodeploy this template using click toodeploy, click **Deploy tooAzure** in hello Readme.md file for hello [VM with static PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) template.</span></span> <span data-ttu-id="95399-124">Ersätt hello Standardparametervärden om så önskas och ange värden för hello tomt parametrar.</span><span class="sxs-lookup"><span data-stu-id="95399-124">Replace hello default parameter values if desired and enter values for hello blank parameters.</span></span>  <span data-ttu-id="95399-125">Följ instruktionerna för hello i hello portal toocreate en virtuell dator med en statisk offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="95399-125">Follow hello instructions in hello portal toocreate a virtual machine with a static public IP address.</span></span>

## <a name="deploy-hello-template-by-using-powershell"></a><span data-ttu-id="95399-126">Distribuera hello-mallen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="95399-126">Deploy hello template by using PowerShell</span></span>

<span data-ttu-id="95399-127">toodeploy hello mallen som du hämtade med PowerShell, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="95399-127">toodeploy hello template you downloaded by using PowerShell, follow hello steps below.</span></span>

1. <span data-ttu-id="95399-128">Om du aldrig har använt Azure PowerShell fullständig hello stegen i hello [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.</span><span class="sxs-lookup"><span data-stu-id="95399-128">If you have never used Azure PowerShell, complete hello steps in hello [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="95399-129">I PowerShell-konsol kör hello `New-AzureRmResourceGroup` cmdlet toocreate en ny resursgrupp om det behövs.</span><span class="sxs-lookup"><span data-stu-id="95399-129">In a PowerShell console, run hello `New-AzureRmResourceGroup` cmdlet toocreate a new resource group, if necessary.</span></span> <span data-ttu-id="95399-130">Om du redan har en resursgrupp som skapats går toostep 3.</span><span class="sxs-lookup"><span data-stu-id="95399-130">If you already have a resource group created, go toostep 3.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name PIPTEST -Location westus
    ```

    <span data-ttu-id="95399-131">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="95399-131">Expected output:</span></span>
   
        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. <span data-ttu-id="95399-132">I PowerShell-konsol kör hello `New-AzureRmResourceGroupDeployment` cmdlet toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="95399-132">In a PowerShell console, run hello `New-AzureRmResourceGroupDeployment` cmdlet toodeploy hello template.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
        -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
        -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json
    ```

    <span data-ttu-id="95399-133">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="95399-133">Expected output:</span></span>
   
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

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a><span data-ttu-id="95399-134">Distribuera hello mallen med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="95399-134">Deploy hello template by using hello Azure CLI</span></span>
<span data-ttu-id="95399-135">toodeploy hello mall med hjälp av hello Azure CLI, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="95399-135">toodeploy hello template by using hello Azure CLI, complete hello following steps:</span></span>

1. <span data-ttu-id="95399-136">Om du aldrig har använt Azure CLI åtgärderna hello i hello [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) artikel tooinstall och konfigurera den.</span><span class="sxs-lookup"><span data-stu-id="95399-136">If you have never used Azure CLI, follow hello steps in hello [Install and Configure hello Azure CLI](../cli-install-nodejs.md) article tooinstall and configure it.</span></span>
2. <span data-ttu-id="95399-137">Kör hello `azure config mode` kommandot tooswitch tooResource Manager-läge enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="95399-137">Run hello `azure config mode` command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="95399-138">hello förväntas för hello kommandot ovan:</span><span class="sxs-lookup"><span data-stu-id="95399-138">hello expected output for hello command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="95399-139">Öppna hello [parameterfilen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), Välj dess innehåll och spara den tooa filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="95399-139">Open hello [parameter file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), select its content, and save it tooa file in your computer.</span></span> <span data-ttu-id="95399-140">I det här exemplet hello parametrar sparas tooa fil med namnet *parameters.json*.</span><span class="sxs-lookup"><span data-stu-id="95399-140">For this example, hello parameters are saved tooa file named *parameters.json*.</span></span> <span data-ttu-id="95399-141">Ändra hello parametervärden i hello-filen om du vill, men minst bör du ändra hello värde för hello adminPassword parametern tooa unika, komplexa lösenord.</span><span class="sxs-lookup"><span data-stu-id="95399-141">Change hello parameter values within hello file if desired, but at a minimum, it's recommended that you change hello value for hello adminPassword parameter tooa unique, complex password.</span></span>
4. <span data-ttu-id="95399-142">Kör hello `azure group deployment create` cmd toodeploy hello nya VNet med hjälp av hello mallen och parametern filer du hämtade och ändrade ovan.</span><span class="sxs-lookup"><span data-stu-id="95399-142">Run hello `azure group deployment create` cmd toodeploy hello new VNet by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="95399-143">Ersätt i hello kommandot nedan <path> med hello sökvägen som du sparade hello-filen till.</span><span class="sxs-lookup"><span data-stu-id="95399-143">In hello command below, replace <path> with hello path you saved hello file to.</span></span> 

    ```azurecli
    azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json
    ```

    <span data-ttu-id="95399-144">Förväntad utdata (visar parametervärden används):</span><span class="sxs-lookup"><span data-stu-id="95399-144">Expected output (lists parameter values used):</span></span>

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

