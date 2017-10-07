---
title: "aaaCreate ett virtuellt nätverk | Azure Resource Manager-mall | Microsoft Docs"
description: "Lär dig hur toocreate ett virtuellt nätverk med en Azure Resource Manager-mall."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 69530861-2f97-4a6e-b336-a7baf2690044
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b9c289433ff2a84bec19eac25fa28ab40d131c7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-an-azure-resource-manager-template"></a><span data-ttu-id="6e71b-103">Skapa ett virtuellt nätverk med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="6e71b-103">Create a virtual network using an Azure Resource Manager template</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="6e71b-104">Azure har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="6e71b-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="6e71b-105">Microsoft rekommenderar att skapa resurser via hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="6e71b-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="6e71b-106">Mer om toolearn hello skillnaderna mellan hello två modeller, läsa hello [förstå Azure distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="6e71b-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>
 
<span data-ttu-id="6e71b-107">Den här artikeln förklarar hur toocreate ett VNet via hello Resource Manager distribution modellen med en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="6e71b-107">This article explains how toocreate a VNet through hello Resource Manager deployment model using an Azure Resource Manager template.</span></span> <span data-ttu-id="6e71b-108">Du kan också skapa ett VNet via Resource Manager med andra verktyg eller skapa ett VNet via hello klassiska distributionsmodellen genom att välja ett annat alternativ hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="6e71b-108">You can also create a VNet through Resource Manager using other tools or create a VNet through hello classic deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
- [<span data-ttu-id="6e71b-109">Portal</span><span class="sxs-lookup"><span data-stu-id="6e71b-109">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
- [<span data-ttu-id="6e71b-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6e71b-110">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
- [<span data-ttu-id="6e71b-111">CLI</span><span class="sxs-lookup"><span data-stu-id="6e71b-111">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
- [<span data-ttu-id="6e71b-112">Mall</span><span class="sxs-lookup"><span data-stu-id="6e71b-112">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
- [<span data-ttu-id="6e71b-113">Portal (klassisk)</span><span class="sxs-lookup"><span data-stu-id="6e71b-113">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
- [<span data-ttu-id="6e71b-114">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="6e71b-114">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [<span data-ttu-id="6e71b-115">CLI (klassisk)</span><span class="sxs-lookup"><span data-stu-id="6e71b-115">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

<span data-ttu-id="6e71b-116">Du får lära dig hur toodownload och ändra befintliga ARM-mall från GitHub och distribuerar hello-mall från GitHub, PowerShell och hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="6e71b-116">You will learn how toodownload and modify and existing ARM template from GitHub, and deploy hello template from GitHub, PowerShell, and hello Azure CLI.</span></span>

<span data-ttu-id="6e71b-117">Om du bara distribuerar hello ARM-mallen direkt från GitHub utan ändringar, hoppar du över för[distribuera en mall från github](#deploy-the-arm-template-by-using-click-to-deploy).</span><span class="sxs-lookup"><span data-stu-id="6e71b-117">If you are simply deploying hello ARM template directly from GitHub, without any changes, skip too[deploy a template from github](#deploy-the-arm-template-by-using-click-to-deploy).</span></span>

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="download-and-understand-hello-azure-resource-manager-template"></a><span data-ttu-id="6e71b-118">Hämta och förstå hello Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="6e71b-118">Download and understand hello Azure Resource Manager template</span></span>
<span data-ttu-id="6e71b-119">Du kan hämta hello befintlig mall för att skapa ett VNet och två undernät från GitHub, göra ändringar du vill och återanvända den.</span><span class="sxs-lookup"><span data-stu-id="6e71b-119">You can download hello existing template for creating a VNet and two subnets from GitHub, make any changes you might want, and reuse it.</span></span> <span data-ttu-id="6e71b-120">toodo slutföra så hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6e71b-120">toodo so, complete hello following steps:</span></span>

1. <span data-ttu-id="6e71b-121">Navigera för[hello exempelmallsidan](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span><span class="sxs-lookup"><span data-stu-id="6e71b-121">Navigate too[hello sample template page](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span></span>
2. <span data-ttu-id="6e71b-122">Klicka på **azuredeploy.json** och klicka sedan på **RAW**.</span><span class="sxs-lookup"><span data-stu-id="6e71b-122">Click **azuredeploy.json**, and then click **RAW**.</span></span>
3. <span data-ttu-id="6e71b-123">Spara hello filen tooa en lokal mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="6e71b-123">Save hello file tooa a local folder on your computer.</span></span>
4. <span data-ttu-id="6e71b-124">Om du är bekant med mallar kan du hoppa över toostep 7.</span><span class="sxs-lookup"><span data-stu-id="6e71b-124">If you are familiar with templates, skip toostep 7.</span></span>
5. <span data-ttu-id="6e71b-125">Öppna hello-filen som du just har sparat och titta på hello innehållet under **parametrar** på rad 5.</span><span class="sxs-lookup"><span data-stu-id="6e71b-125">Open hello file you just saved and look at hello contents under **parameters** in line 5.</span></span> <span data-ttu-id="6e71b-126">ARM-mallparametrarna agerar platshållare för värden som kan anges under distributionen.</span><span class="sxs-lookup"><span data-stu-id="6e71b-126">ARM template parameters provide a placeholder for values that can be filled out during deployment.</span></span>
   
   | <span data-ttu-id="6e71b-127">Parameter</span><span class="sxs-lookup"><span data-stu-id="6e71b-127">Parameter</span></span> | <span data-ttu-id="6e71b-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6e71b-128">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="6e71b-129">**Plats**</span><span class="sxs-lookup"><span data-stu-id="6e71b-129">**location**</span></span> |<span data-ttu-id="6e71b-130">Azure-region där hello VNet kommer att skapas</span><span class="sxs-lookup"><span data-stu-id="6e71b-130">Azure region where hello VNet will be created</span></span> |
   | <span data-ttu-id="6e71b-131">**vnetName**</span><span class="sxs-lookup"><span data-stu-id="6e71b-131">**vnetName**</span></span> |<span data-ttu-id="6e71b-132">Namn för hello nya VNet</span><span class="sxs-lookup"><span data-stu-id="6e71b-132">Name for hello new VNet</span></span> |
   | <span data-ttu-id="6e71b-133">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="6e71b-133">**addressPrefix**</span></span> |<span data-ttu-id="6e71b-134">Adressutrymmet för hello VNet, i CIDR-format</span><span class="sxs-lookup"><span data-stu-id="6e71b-134">Address space for hello VNet, in CIDR format</span></span> |
   | <span data-ttu-id="6e71b-135">**subnet1Name**</span><span class="sxs-lookup"><span data-stu-id="6e71b-135">**subnet1Name**</span></span> |<span data-ttu-id="6e71b-136">Namn för hello första VNet</span><span class="sxs-lookup"><span data-stu-id="6e71b-136">Name for hello first VNet</span></span> |
   | <span data-ttu-id="6e71b-137">**subnet1Prefix**</span><span class="sxs-lookup"><span data-stu-id="6e71b-137">**subnet1Prefix**</span></span> |<span data-ttu-id="6e71b-138">CIDR-block för hello första undernätet</span><span class="sxs-lookup"><span data-stu-id="6e71b-138">CIDR block for hello first subnet</span></span> |
   | <span data-ttu-id="6e71b-139">**subnet2Name**</span><span class="sxs-lookup"><span data-stu-id="6e71b-139">**subnet2Name**</span></span> |<span data-ttu-id="6e71b-140">Namn för hello andra VNet</span><span class="sxs-lookup"><span data-stu-id="6e71b-140">Name for hello second VNet</span></span> |
   | <span data-ttu-id="6e71b-141">**subnet2Prefix**</span><span class="sxs-lookup"><span data-stu-id="6e71b-141">**subnet2Prefix**</span></span> |<span data-ttu-id="6e71b-142">CIDR-block för hello andra undernätet</span><span class="sxs-lookup"><span data-stu-id="6e71b-142">CIDR block for hello second subnet</span></span> |
   
   > [!IMPORTANT]
   > <span data-ttu-id="6e71b-143">Azure Resource Manager-mallar som finns på GitHub kan ändras med tiden.</span><span class="sxs-lookup"><span data-stu-id="6e71b-143">Azure Resource Manager templates maintained in GitHub can change over time.</span></span> <span data-ttu-id="6e71b-144">Kontrollera inställningarna i hello mallen innan du använder den.</span><span class="sxs-lookup"><span data-stu-id="6e71b-144">Make sure you check hello template before using it.</span></span>
   > 
   > 
6. <span data-ttu-id="6e71b-145">Kontrollera hello innehåll under **resurser** och notera hello följande:</span><span class="sxs-lookup"><span data-stu-id="6e71b-145">Check hello content under **resources** and notice hello following:</span></span>
   
   * <span data-ttu-id="6e71b-146">**type**.</span><span class="sxs-lookup"><span data-stu-id="6e71b-146">**type**.</span></span> <span data-ttu-id="6e71b-147">Typ av resurs som skapas av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="6e71b-147">Type of resource being created by hello template.</span></span> <span data-ttu-id="6e71b-148">I det här fallet **Microsoft.Network/virtualNetworks**, vilket motsvarar ett VNet.</span><span class="sxs-lookup"><span data-stu-id="6e71b-148">In this case, **Microsoft.Network/virtualNetworks**, which represent a VNet.</span></span>
   * <span data-ttu-id="6e71b-149">**name**.</span><span class="sxs-lookup"><span data-stu-id="6e71b-149">**name**.</span></span> <span data-ttu-id="6e71b-150">Namnet för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="6e71b-150">Name for hello resource.</span></span> <span data-ttu-id="6e71b-151">Meddelande hello användning av **[parameters('vnetName')]**, vilket innebär hello namn kommer att anges som indata av hello användare eller en parameterfil under distributionen.</span><span class="sxs-lookup"><span data-stu-id="6e71b-151">Notice hello use of **[parameters('vnetName')]**, which means hello name will provided as input by hello user or a parameter file during deployment.</span></span>
   * <span data-ttu-id="6e71b-152">**properties**.</span><span class="sxs-lookup"><span data-stu-id="6e71b-152">**properties**.</span></span> <span data-ttu-id="6e71b-153">Lista över egenskaper för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="6e71b-153">List of properties for hello resource.</span></span> <span data-ttu-id="6e71b-154">Denna mall används hello egenskaper för adressutrymmet och undernätsegenskaperna vid skapandet av ett VNet.</span><span class="sxs-lookup"><span data-stu-id="6e71b-154">This template uses hello address space and subnet properties during VNet creation.</span></span>
7. <span data-ttu-id="6e71b-155">Gå tillbaka för[hello exempelmallsidan](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span><span class="sxs-lookup"><span data-stu-id="6e71b-155">Navigate back too[hello sample template page](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span></span>
8. <span data-ttu-id="6e71b-156">Klicka på **azuredeploy-parameters.json** och klicka sedan på **RAW**.</span><span class="sxs-lookup"><span data-stu-id="6e71b-156">Click **azuredeploy-paremeters.json**, and then click **RAW**.</span></span>
9. <span data-ttu-id="6e71b-157">Spara hello filen tooa en lokal mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="6e71b-157">Save hello file tooa a local folder on your computer.</span></span>
10. <span data-ttu-id="6e71b-158">Öppna hello-filen som du just har sparat och redigera hello värden för hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="6e71b-158">Open hello file you just saved and edit hello values for hello parameters.</span></span> <span data-ttu-id="6e71b-159">Använd följande värden under toodeploy hello VNet som beskrivs i scenariot hello hello:</span><span class="sxs-lookup"><span data-stu-id="6e71b-159">Use hello following values below toodeploy hello VNet described in hello scenario:</span></span>

    ```json
        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }
    ```

11. <span data-ttu-id="6e71b-160">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="6e71b-160">Save hello file.</span></span>


## <a name="deploy-hello-template-using-powershell"></a><span data-ttu-id="6e71b-161">Distribuera hello-mallen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="6e71b-161">Deploy hello template using PowerShell</span></span>

<span data-ttu-id="6e71b-162">Slutför hello följande steg toodeploy hello mallen som du hämtade med hjälp av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6e71b-162">Complete hello following steps toodeploy hello template you downloaded by using PowerShell:</span></span>

1. <span data-ttu-id="6e71b-163">Installera och konfigurera Azure PowerShell genom att slutföra hello stegen i hello [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.</span><span class="sxs-lookup"><span data-stu-id="6e71b-163">Install and configure Azure PowerShell by completing hello steps in hello [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="6e71b-164">Kör följande kommando toocreate hello en ny resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="6e71b-164">Run hello following command toocreate a new resource group:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    <span data-ttu-id="6e71b-165">hello-kommando skapar en resursgrupp med namnet *TestRG* i hello *centrala USA* azure-region.</span><span class="sxs-lookup"><span data-stu-id="6e71b-165">hello command creates a resource group named *TestRG* in hello *Central US* azure region.</span></span> <span data-ttu-id="6e71b-166">Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6e71b-166">For more information about resource groups, visit [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    <span data-ttu-id="6e71b-167">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="6e71b-167">Expected output:</span></span>

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/[Id]/resourceGroups/TestRG

3. <span data-ttu-id="6e71b-168">Kör följande kommando toodeploy hello nya VNet med hjälp av hello mallen och parametern filer du hämtade och ändrade ovan hello:</span><span class="sxs-lookup"><span data-stu-id="6e71b-168">Run hello following command toodeploy hello new VNet using hello template and parameter files you downloaded and modified above:</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestVNetDeployment -ResourceGroupName TestRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

    <span data-ttu-id="6e71b-169">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="6e71b-169">Expected output:</span></span>
   
        DeploymentName    : TestVNetDeployment
        ResourceGroupName : TestRG
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            location         String                     Central US
                            vnetName         String                     TestVNet
                            addressPrefix    String                     192.168.0.0/16
                            subnet1Prefix    String                     192.168.1.0/24
                            subnet1Name      String                     FrontEnd
                            subnet2Prefix    String                     192.168.2.0/24
                            subnet2Name      String                     BackEnd
   
        Outputs           :
4. <span data-ttu-id="6e71b-170">Hello kör följande kommando tooview hello egenskaper för hello nya VNet:</span><span class="sxs-lookup"><span data-stu-id="6e71b-170">Run hello following command tooview hello properties of hello new VNet:</span></span>

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

    <span data-ttu-id="6e71b-171">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="6e71b-171">Expected output:</span></span>

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              },
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="deploy-hello-template-using-click-to-deploy"></a><span data-ttu-id="6e71b-172">Distribuera hello mallen med Klicka för att distribuera</span><span class="sxs-lookup"><span data-stu-id="6e71b-172">Deploy hello template using click-to-deploy</span></span>

<span data-ttu-id="6e71b-173">Du kan återanvända fördefinierade Azure Resource Manager mallar överförda tooa GitHub databas som hanteras av Microsoft och öppna toohello community.</span><span class="sxs-lookup"><span data-stu-id="6e71b-173">You can reuse pre-defined Azure Resource Manager templates uploaded tooa GitHub repository maintained by Microsoft and open toohello community.</span></span> <span data-ttu-id="6e71b-174">De här mallarna kan distribueras direkt från GitHub, eller hämtade och ändrade toofit dina behov.</span><span class="sxs-lookup"><span data-stu-id="6e71b-174">These templates can be deployed straight out of GitHub, or downloaded and modified toofit your needs.</span></span> <span data-ttu-id="6e71b-175">toodeploy en mall som skapar ett VNet med två undernät fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6e71b-175">toodeploy a template that creates a VNet with two subnets, complete hello following steps:</span></span>

1. <span data-ttu-id="6e71b-176">Från en webbläsare navigerar för[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="6e71b-176">From a browser, navigate too[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span>
2. <span data-ttu-id="6e71b-177">Bläddra nedåt hello listan över mallar och klickar på **101-vnet-two-subnets**.</span><span class="sxs-lookup"><span data-stu-id="6e71b-177">Scroll down hello list of templates, and click **101-vnet-two-subnets**.</span></span> <span data-ttu-id="6e71b-178">Kontrollera hello **README.md** filen enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="6e71b-178">Check hello **README.md** file, as shown below.</span></span>

    ![README.md-filen i GitHub](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. <span data-ttu-id="6e71b-180">Klicka på **distribuera tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="6e71b-180">Click **Deploy tooAzure**.</span></span> <span data-ttu-id="6e71b-181">Ange dina inloggningsuppgifter för Azure vid behov.</span><span class="sxs-lookup"><span data-stu-id="6e71b-181">If necessary, enter your Azure login credentials.</span></span> 
4. <span data-ttu-id="6e71b-182">I hello **parametrar** bladet ange hello värden du toouse toocreate ditt nya VNet, och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e71b-182">In hello **Parameters** blade, enter hello values you want toouse toocreate your new VNet, and then click **OK**.</span></span> <span data-ttu-id="6e71b-183">hello visar följande bild hello värden för hello scenario:</span><span class="sxs-lookup"><span data-stu-id="6e71b-183">hello following figure shows hello values for hello scenario:</span></span>
   
    ![ARM-mallparametrar](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

5. <span data-ttu-id="6e71b-185">Klicka på **resursgruppen** och välj en resurs grupp tooadd hello VNet till, eller klicka på **Skapa nytt** tooadd hello VNet tooa ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6e71b-185">Click **Resource group** and select a resource group tooadd hello VNet to, or click **Create new** tooadd hello VNet tooa new resource group.</span></span> <span data-ttu-id="6e71b-186">hello följande bild visar hello resurs resursgruppsinställningarna för en ny resursgrupp som kallas **TestRG**:</span><span class="sxs-lookup"><span data-stu-id="6e71b-186">hello following figure shows hello resource group settings for a new resource group called **TestRG**:</span></span>

    ![Resursgrupp](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

6. <span data-ttu-id="6e71b-188">Om det behövs ändrar hello **prenumeration** och **plats** inställningar för din VNet.</span><span class="sxs-lookup"><span data-stu-id="6e71b-188">If necessary, change hello **Subscription** and **Location** settings for your VNet.</span></span>
7. <span data-ttu-id="6e71b-189">Om du inte vill toosee hello VNet som en panel i hello **startsidan**, inaktivera **PIN-kod tooStartboard**.</span><span class="sxs-lookup"><span data-stu-id="6e71b-189">If you do not want toosee hello VNet as a tile in hello **Startboard**, disable **Pin tooStartboard**.</span></span>
8. <span data-ttu-id="6e71b-190">Klicka på **juridiska villkor**, Läs hello villkoren och klicka på **köpa** tooagree.</span><span class="sxs-lookup"><span data-stu-id="6e71b-190">Click **Legal terms**, read hello terms, and click **Buy** tooagree.</span></span> 
9. <span data-ttu-id="6e71b-191">Klicka på **skapa** toocreate hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="6e71b-191">Click **Create** toocreate hello VNet.</span></span>
   
    ![Ikonen för Skicka in distribution i preview-portalen](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

10. <span data-ttu-id="6e71b-193">När hello distributionen är klar i hello Azure-portalen klickar du på **fler tjänster**, typen *virtuella nätverk* hello filter i rutan som visas och klicka sedan på virtuella nätverk toosee hello virtuella nätverk bladet.</span><span class="sxs-lookup"><span data-stu-id="6e71b-193">Once hello deployment is complete, in hello Azure portal click **More services**, type *virtual networks* in hello filter box that appears, then click Virtual networks toosee hello Virtual networks blade.</span></span> <span data-ttu-id="6e71b-194">Hello-bladet klickar du på *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="6e71b-194">In hello blade, click *TestVNet*.</span></span> <span data-ttu-id="6e71b-195">I hello *TestVNet* bladet, klickar du på **undernät** toosee hello skapade undernät, som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="6e71b-195">In hello *TestVNet* blade, click **Subnets** toosee hello created subnets, as shown in hello following picture:</span></span>
    
     ![Skapa VNet i preview-portalen](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.png)

## <a name="next-steps"></a><span data-ttu-id="6e71b-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6e71b-197">Next steps</span></span>

<span data-ttu-id="6e71b-198">Lär dig hur tooconnect:</span><span class="sxs-lookup"><span data-stu-id="6e71b-198">Learn how tooconnect:</span></span>

- <span data-ttu-id="6e71b-199">En virtuell dator (VM) tooa virtuellt nätverk genom att läsa hello [skapa en virtuell Windows-dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md) eller [skapa ett Linux VM](../virtual-machines/linux/quick-create-portal.md) artiklar.</span><span class="sxs-lookup"><span data-stu-id="6e71b-199">A virtual machine (VM) tooa virtual network by reading hello [Create a Windows VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) or [Create a Linux VM](../virtual-machines/linux/quick-create-portal.md) articles.</span></span> <span data-ttu-id="6e71b-200">I stället för att skapa en VNet och undernät i hello steg hello artiklar, kan du välja ett befintligt VNet och undernät tooconnect en virtuell dator till.</span><span class="sxs-lookup"><span data-stu-id="6e71b-200">Instead of creating a VNet and subnet in hello steps of hello articles, you can select an existing VNet and subnet tooconnect a VM to.</span></span>
- <span data-ttu-id="6e71b-201">Hej virtuellt nätverk tooother virtuella nätverk genom att läsa hello [ansluta Vnet](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="6e71b-201">hello virtual network tooother virtual networks by reading hello [Connect VNets](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) article.</span></span>
- <span data-ttu-id="6e71b-202">hello virtuellt nätverk tooan lokalt nätverk via ett plats-till-plats virtuellt privat nätverk (VPN) eller en ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="6e71b-202">hello virtual network tooan on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="6e71b-203">Lär dig hur genom att läsa hello [ansluta ett virtuellt nätverk tooan lokalt nätverk med hjälp av en plats-till-plats-VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) och [länka VNet-tooan ExpressRoute-krets](../expressroute/expressroute-howto-linkvnet-arm.md) artiklar.</span><span class="sxs-lookup"><span data-stu-id="6e71b-203">Learn how by reading hello [Connect a VNet tooan on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet tooan ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-arm.md) articles.</span></span>
