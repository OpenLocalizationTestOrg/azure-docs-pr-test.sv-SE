---
title: "Skapa ett virtuellt nätverk | Azure Resource Manager-mall | Microsoft Docs"
description: "Lär dig hur du skapar ett virtuellt nätverk med en Azure Resource Manager-mall."
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
ms.openlocfilehash: 81602766848a91331c8d811ea1c8ec3ffae44b96
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-using-an-azure-resource-manager-template"></a><span data-ttu-id="ce7cb-103">Skapa ett virtuellt nätverk med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="ce7cb-103">Create a virtual network using an Azure Resource Manager template</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="ce7cb-104">Azure har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="ce7cb-105">Microsoft rekommenderar att skapa resurser med Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-105">Microsoft recommends creating resources through the Resource Manager deployment model.</span></span> <span data-ttu-id="ce7cb-106">Mer information om skillnaderna mellan de två modellerna finns i artikeln [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) (Förstå Azure-distributionsmodellerna).</span><span class="sxs-lookup"><span data-stu-id="ce7cb-106">To learn more about the differences between the two models, read the [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>
 
<span data-ttu-id="ce7cb-107">Den här artikeln förklaras hur du skapar ett VNet via Resource Manager-distributionsmodellen med hjälp av en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-107">This article explains how to create a VNet through the Resource Manager deployment model using an Azure Resource Manager template.</span></span> <span data-ttu-id="ce7cb-108">Du kan också skapa ett virtuellt nätverk med Resource Manager med hjälp av andra verktyg eller skapa ett virtuellt nätverk med hjälp av den klassiska distributionsmodellen genom att välja ett annat alternativ i följande lista:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-108">You can also create a VNet through Resource Manager using other tools or create a VNet through the classic deployment model by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
- [<span data-ttu-id="ce7cb-109">Portal</span><span class="sxs-lookup"><span data-stu-id="ce7cb-109">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
- [<span data-ttu-id="ce7cb-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ce7cb-110">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
- [<span data-ttu-id="ce7cb-111">CLI</span><span class="sxs-lookup"><span data-stu-id="ce7cb-111">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
- [<span data-ttu-id="ce7cb-112">Mall</span><span class="sxs-lookup"><span data-stu-id="ce7cb-112">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
- [<span data-ttu-id="ce7cb-113">Portal (klassisk)</span><span class="sxs-lookup"><span data-stu-id="ce7cb-113">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
- [<span data-ttu-id="ce7cb-114">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="ce7cb-114">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [<span data-ttu-id="ce7cb-115">CLI (klassisk)</span><span class="sxs-lookup"><span data-stu-id="ce7cb-115">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

<span data-ttu-id="ce7cb-116">Du kommer lära dig hur man hämtar och ändrar en befintlig ARM-mall från GitHub och distribuerar mallen från GitHub, PowerShell och Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-116">You will learn how to download and modify and existing ARM template from GitHub, and deploy the template from GitHub, PowerShell, and the Azure CLI.</span></span>

<span data-ttu-id="ce7cb-117">Om du bara distribuerar ARM-mallen direkt från GitHub utan ändringar, kan du hoppa till [distribuera en mall från github](#deploy-the-arm-template-by-using-click-to-deploy).</span><span class="sxs-lookup"><span data-stu-id="ce7cb-117">If you are simply deploying the ARM template directly from GitHub, without any changes, skip to [deploy a template from github](#deploy-the-arm-template-by-using-click-to-deploy).</span></span>

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="download-and-understand-the-azure-resource-manager-template"></a><span data-ttu-id="ce7cb-118">Hämta och förstå Azure Resource Manager-mallen</span><span class="sxs-lookup"><span data-stu-id="ce7cb-118">Download and understand the Azure Resource Manager template</span></span>
<span data-ttu-id="ce7cb-119">Du kan hämta den befintliga mallen för att skapa ett VNet och två undernät från GitHub, göra ändringar du vill och återanvända den.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-119">You can download the existing template for creating a VNet and two subnets from GitHub, make any changes you might want, and reuse it.</span></span> <span data-ttu-id="ce7cb-120">Om du vill göra det, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-120">To do so, complete the following steps:</span></span>

1. <span data-ttu-id="ce7cb-121">Gå till [exempelmallsidan](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span><span class="sxs-lookup"><span data-stu-id="ce7cb-121">Navigate to [the sample template page](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span></span>
2. <span data-ttu-id="ce7cb-122">Klicka på **azuredeploy.json** och klicka sedan på **RAW**.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-122">Click **azuredeploy.json**, and then click **RAW**.</span></span>
3. <span data-ttu-id="ce7cb-123">Spara filen på en lokal mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-123">Save the file to a a local folder on your computer.</span></span>
4. <span data-ttu-id="ce7cb-124">Om du är bekant med mallar kan du gå vidare till steg 7.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-124">If you are familiar with templates, skip to step 7.</span></span>
5. <span data-ttu-id="ce7cb-125">Öppna filen som du just har sparat och titta på innehållet i **parametrar** på rad 5.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-125">Open the file you just saved and look at the contents under **parameters** in line 5.</span></span> <span data-ttu-id="ce7cb-126">ARM-mallparametrarna agerar platshållare för värden som kan anges under distributionen.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-126">ARM template parameters provide a placeholder for values that can be filled out during deployment.</span></span>
   
   | <span data-ttu-id="ce7cb-127">Parameter</span><span class="sxs-lookup"><span data-stu-id="ce7cb-127">Parameter</span></span> | <span data-ttu-id="ce7cb-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ce7cb-128">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="ce7cb-129">**Plats**</span><span class="sxs-lookup"><span data-stu-id="ce7cb-129">**location**</span></span> |<span data-ttu-id="ce7cb-130">Azure-region där VNet kommer att skapas</span><span class="sxs-lookup"><span data-stu-id="ce7cb-130">Azure region where the VNet will be created</span></span> |
   | <span data-ttu-id="ce7cb-131">**vnetName**</span><span class="sxs-lookup"><span data-stu-id="ce7cb-131">**vnetName**</span></span> |<span data-ttu-id="ce7cb-132">Namn för det nya VNet</span><span class="sxs-lookup"><span data-stu-id="ce7cb-132">Name for the new VNet</span></span> |
   | <span data-ttu-id="ce7cb-133">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="ce7cb-133">**addressPrefix**</span></span> |<span data-ttu-id="ce7cb-134">Adressutrymmet för VNet, i CIDR-format</span><span class="sxs-lookup"><span data-stu-id="ce7cb-134">Address space for the VNet, in CIDR format</span></span> |
   | <span data-ttu-id="ce7cb-135">**subnet1Name**</span><span class="sxs-lookup"><span data-stu-id="ce7cb-135">**subnet1Name**</span></span> |<span data-ttu-id="ce7cb-136">Namn för det första VNet</span><span class="sxs-lookup"><span data-stu-id="ce7cb-136">Name for the first VNet</span></span> |
   | <span data-ttu-id="ce7cb-137">**subnet1Prefix**</span><span class="sxs-lookup"><span data-stu-id="ce7cb-137">**subnet1Prefix**</span></span> |<span data-ttu-id="ce7cb-138">CIDR-block för det första undernätet</span><span class="sxs-lookup"><span data-stu-id="ce7cb-138">CIDR block for the first subnet</span></span> |
   | <span data-ttu-id="ce7cb-139">**subnet2Name**</span><span class="sxs-lookup"><span data-stu-id="ce7cb-139">**subnet2Name**</span></span> |<span data-ttu-id="ce7cb-140">Namn för den andra VNet</span><span class="sxs-lookup"><span data-stu-id="ce7cb-140">Name for the second VNet</span></span> |
   | <span data-ttu-id="ce7cb-141">**subnet2Prefix**</span><span class="sxs-lookup"><span data-stu-id="ce7cb-141">**subnet2Prefix**</span></span> |<span data-ttu-id="ce7cb-142">CIDR-block för andra undernätet</span><span class="sxs-lookup"><span data-stu-id="ce7cb-142">CIDR block for the second subnet</span></span> |
   
   > [!IMPORTANT]
   > <span data-ttu-id="ce7cb-143">Azure Resource Manager-mallar som finns på GitHub kan ändras med tiden.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-143">Azure Resource Manager templates maintained in GitHub can change over time.</span></span> <span data-ttu-id="ce7cb-144">Var noga med att kontrollera mallen innan den används.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-144">Make sure you check the template before using it.</span></span>
   > 
   > 
6. <span data-ttu-id="ce7cb-145">Kontrollera innehållet i **resurser** och observera följande:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-145">Check the content under **resources** and notice the following:</span></span>
   
   * <span data-ttu-id="ce7cb-146">**type**.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-146">**type**.</span></span> <span data-ttu-id="ce7cb-147">Typ av resurs som skapas av mallen.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-147">Type of resource being created by the template.</span></span> <span data-ttu-id="ce7cb-148">I det här fallet **Microsoft.Network/virtualNetworks**, vilket motsvarar ett VNet.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-148">In this case, **Microsoft.Network/virtualNetworks**, which represent a VNet.</span></span>
   * <span data-ttu-id="ce7cb-149">**name**.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-149">**name**.</span></span> <span data-ttu-id="ce7cb-150">Namn på den virtuella resursen.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-150">Name for the resource.</span></span> <span data-ttu-id="ce7cb-151">Observera användningen av **[parameters('vnetName')]**, vilket betyder att namnet kommer att anges som indata från användaren eller en parameterfil vid distributionen.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-151">Notice the use of **[parameters('vnetName')]**, which means the name will provided as input by the user or a parameter file during deployment.</span></span>
   * <span data-ttu-id="ce7cb-152">**properties**.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-152">**properties**.</span></span> <span data-ttu-id="ce7cb-153">Lista över egenskaper för resursen.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-153">List of properties for the resource.</span></span> <span data-ttu-id="ce7cb-154">Den här mallen använder adressutrymmet och undernätsegenskaperna vid skapandet av ett VNet.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-154">This template uses the address space and subnet properties during VNet creation.</span></span>
7. <span data-ttu-id="ce7cb-155">Gå tillbaks till [exempelmallsidan](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span><span class="sxs-lookup"><span data-stu-id="ce7cb-155">Navigate back to [the sample template page](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).</span></span>
8. <span data-ttu-id="ce7cb-156">Klicka på **azuredeploy-parameters.json** och klicka sedan på **RAW**.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-156">Click **azuredeploy-paremeters.json**, and then click **RAW**.</span></span>
9. <span data-ttu-id="ce7cb-157">Spara filen i en lokal mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-157">Save the file to a a local folder on your computer.</span></span>
10. <span data-ttu-id="ce7cb-158">Öppna filen som du just har sparat och redigera värdena för parametrarna.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-158">Open the file you just saved and edit the values for the parameters.</span></span> <span data-ttu-id="ce7cb-159">Använd värdena nedan för att distribuera det VNet som beskrivs i scenariot:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-159">Use the following values below to deploy the VNet described in the scenario:</span></span>

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

11. <span data-ttu-id="ce7cb-160">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-160">Save the file.</span></span>


## <a name="deploy-the-template-using-powershell"></a><span data-ttu-id="ce7cb-161">Distribuera mallen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="ce7cb-161">Deploy the template using PowerShell</span></span>

<span data-ttu-id="ce7cb-162">Utför följande steg för att distribuera mallen som du hämtade med hjälp av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-162">Complete the following steps to deploy the template you downloaded by using PowerShell:</span></span>

1. <span data-ttu-id="ce7cb-163">Installera och konfigurera Azure PowerShell genom att slutföra stegen i den [installera och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-163">Install and configure Azure PowerShell by completing the steps in the [How to Install and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="ce7cb-164">Kör följande kommando för att skapa en ny resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-164">Run the following command to create a new resource group:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    <span data-ttu-id="ce7cb-165">Kommandot skapar en resursgrupp med namnet *TestRG* i den *centrala USA* azure-region.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-165">The command creates a resource group named *TestRG* in the *Central US* azure region.</span></span> <span data-ttu-id="ce7cb-166">Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ce7cb-166">For more information about resource groups, visit [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    <span data-ttu-id="ce7cb-167">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-167">Expected output:</span></span>

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/[Id]/resourceGroups/TestRG

3. <span data-ttu-id="ce7cb-168">Kör följande kommando för att distribuera det nya VNet med hjälp av mallen och parametern filer du hämtade och ändrade ovan:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-168">Run the following command to deploy the new VNet using the template and parameter files you downloaded and modified above:</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestVNetDeployment -ResourceGroupName TestRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

    <span data-ttu-id="ce7cb-169">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-169">Expected output:</span></span>
   
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
4. <span data-ttu-id="ce7cb-170">Kör följande kommando för att visa egenskaperna för det nya VNet:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-170">Run the following command to view the properties of the new VNet:</span></span>

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

    <span data-ttu-id="ce7cb-171">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-171">Expected output:</span></span>

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

## <a name="deploy-the-template-using-click-to-deploy"></a><span data-ttu-id="ce7cb-172">Distribuera mallen med Klicka för att distribuera</span><span class="sxs-lookup"><span data-stu-id="ce7cb-172">Deploy the template using click-to-deploy</span></span>

<span data-ttu-id="ce7cb-173">Du kan återanvända fördefinierade Azure Resource Manager-mallar som har överförts till en GitHub-databas som hanteras av Microsoft och öppen för allmänheten.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-173">You can reuse pre-defined Azure Resource Manager templates uploaded to a GitHub repository maintained by Microsoft and open to the community.</span></span> <span data-ttu-id="ce7cb-174">De här mallarna kan distribueras direkt från GitHub, eller hämtas och ändras så att de passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-174">These templates can be deployed straight out of GitHub, or downloaded and modified to fit your needs.</span></span> <span data-ttu-id="ce7cb-175">Om du vill distribuera en mall som skapar ett VNet med två undernät, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-175">To deploy a template that creates a VNet with two subnets, complete the following steps:</span></span>

1. <span data-ttu-id="ce7cb-176">Från en webbläsare, navigerar du till [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="ce7cb-176">From a browser, navigate to [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span>
2. <span data-ttu-id="ce7cb-177">Bläddra ned i mallistan och klicka på **101-vnet-two-subnets**.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-177">Scroll down the list of templates, and click **101-vnet-two-subnets**.</span></span> <span data-ttu-id="ce7cb-178">Kontrollera **README.md**-filen enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-178">Check the **README.md** file, as shown below.</span></span>

    ![README.md-filen i GitHub](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. <span data-ttu-id="ce7cb-180">Klicka på **Distribuera till Azure**.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-180">Click **Deploy to Azure**.</span></span> <span data-ttu-id="ce7cb-181">Ange dina inloggningsuppgifter för Azure vid behov.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-181">If necessary, enter your Azure login credentials.</span></span> 
4. <span data-ttu-id="ce7cb-182">I bladet **Parametrar** anger du de värden som du vill använda för att skapa ditt nya VNet och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-182">In the **Parameters** blade, enter the values you want to use to create your new VNet, and then click **OK**.</span></span> <span data-ttu-id="ce7cb-183">Följande bild visar värdena för scenariot:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-183">The following figure shows the values for the scenario:</span></span>
   
    ![ARM-mallparametrar](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

5. <span data-ttu-id="ce7cb-185">Klicka på **Resursgrupp** och välj en resursgrupp att lägga till VNet till, eller klicka på **Skapa ny** för att lägga till VNet till en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-185">Click **Resource group** and select a resource group to add the VNet to, or click **Create new** to add the VNet to a new resource group.</span></span> <span data-ttu-id="ce7cb-186">Följande bild visar resursen resursgruppsinställningarna för en ny resursgrupp som kallas **TestRG**:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-186">The following figure shows the resource group settings for a new resource group called **TestRG**:</span></span>

    ![Resursgrupp](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

6. <span data-ttu-id="ce7cb-188">Ändra vid behov inställningarna för **Prenumeration** och **Plats** för din VNet.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-188">If necessary, change the **Subscription** and **Location** settings for your VNet.</span></span>
7. <span data-ttu-id="ce7cb-189">Om du inte vill se VNet som en ikon på **Startsidan**, inaktiverar du **Fäst på startsidan**.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-189">If you do not want to see the VNet as a tile in the **Startboard**, disable **Pin to Startboard**.</span></span>
8. <span data-ttu-id="ce7cb-190">Klicka på **juridiska villkor**, Läs villkoren och klicka på **köpa** accepterar.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-190">Click **Legal terms**, read the terms, and click **Buy** to agree.</span></span> 
9. <span data-ttu-id="ce7cb-191">Skapa VNet genom att klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-191">Click **Create** to create the VNet.</span></span>
   
    ![Ikonen för Skicka in distribution i preview-portalen](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

10. <span data-ttu-id="ce7cb-193">När installationen är klar i Azure portal klickar du på **fler tjänster**, typen *virtuella nätverk* i filterrutan och klicka sedan på virtuella nätverk för att se virtuella nätverk-bladet.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-193">Once the deployment is complete, in the Azure portal click **More services**, type *virtual networks* in the filter box that appears, then click Virtual networks to see the Virtual networks blade.</span></span> <span data-ttu-id="ce7cb-194">I bladet, klickar du på *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-194">In the blade, click *TestVNet*.</span></span> <span data-ttu-id="ce7cb-195">I den *TestVNet* bladet, klickar du på **undernät** att se skapade undernät, som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-195">In the *TestVNet* blade, click **Subnets** to see the created subnets, as shown in the following picture:</span></span>
    
     ![Skapa VNet i preview-portalen](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.png)

## <a name="next-steps"></a><span data-ttu-id="ce7cb-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ce7cb-197">Next steps</span></span>

<span data-ttu-id="ce7cb-198">Lär dig hur du ansluter:</span><span class="sxs-lookup"><span data-stu-id="ce7cb-198">Learn how to connect:</span></span>

- <span data-ttu-id="ce7cb-199">En virtuell dator (VM) till ett virtuellt nätverk genom att läsa artiklarna om hur du [skapar en Windows-baserad virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md) eller [en Linux-baserad virtuell dator](../virtual-machines/linux/quick-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ce7cb-199">A virtual machine (VM) to a virtual network by reading the [Create a Windows VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) or [Create a Linux VM](../virtual-machines/linux/quick-create-portal.md) articles.</span></span> <span data-ttu-id="ce7cb-200">I stället för att skapa ett virtuellt nätverk och undernät i stegen i artikeln kan du ansluta en virtuell dator till ett befintligt virtuellt nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-200">Instead of creating a VNet and subnet in the steps of the articles, you can select an existing VNet and subnet to connect a VM to.</span></span>
- <span data-ttu-id="ce7cb-201">Det virtuella nätverket till andra virtuella nätverk genom att läsa artikeln [Connect VNets](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) (Ansluta virtuella nätverk).</span><span class="sxs-lookup"><span data-stu-id="ce7cb-201">The virtual network to other virtual networks by reading the [Connect VNets](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) article.</span></span>
- <span data-ttu-id="ce7cb-202">Det virtuella nätverket till ett lokalt nätverk med hjälp av ett virtuellt privat nätverk (VPN) för plats till plats eller en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="ce7cb-202">The virtual network to an on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="ce7cb-203">Mer information finns i artiklarna [Connect a VNet to an on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) (Ansluta ett virtuellt nätverk till ett lokalt nätverk med hjälp av VPN för plats till plats) och [Link a VNet to an ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-arm.md) (Länka ett virtuellt nätverk till en ExpressRoute-krets).</span><span class="sxs-lookup"><span data-stu-id="ce7cb-203">Learn how by reading the [Connect a VNet to an on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet to an ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-arm.md) articles.</span></span>
