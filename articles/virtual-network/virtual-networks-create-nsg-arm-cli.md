---
title: "Skapa nätverkssäkerhetsgrupper - Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur du skapar och distribuerar nätverkssäkerhetsgrupper som använder Azure CLI 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9ea82c09-f4a6-4268-88bc-fc439db40c48
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8efb3ab66d07875b51f723fed5594bcb477ed025
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-network-security-groups-using-the-azure-cli-20"></a><span data-ttu-id="4c1d6-103">Skapa nätverk med Azure CLI 2.0-säkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="4c1d6-103">Create network security groups using the Azure CLI 2.0</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="4c1d6-104">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="4c1d6-104">CLI versions to complete the task</span></span> 

<span data-ttu-id="4c1d6-105">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="4c1d6-105">You can complete the task using one of the following CLI versions:</span></span> 

- <span data-ttu-id="4c1d6-106">[Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md) – vår CLI för distributionsmodellerna klassisk och resurshantering</span><span class="sxs-lookup"><span data-stu-id="4c1d6-106">[Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md) – our CLI for the classic and resource management deployment models</span></span> 
- <span data-ttu-id="4c1d6-107">[Azure CLI 2.0](#Create-the-nsg-for-the-front-end-subnet) -vår nästa generations CLI för hantering av resursdistributionsmodell (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="4c1d6-107">[Azure CLI 2.0](#Create-the-nsg-for-the-front-end-subnet) - our next generation CLI for the resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="4c1d6-108">Exemplet Azure CLI 2.0 kommandon förväntar sig en enkel miljö som redan har skapats baserat på scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-108">The sample Azure CLI 2.0 commands following expect a simple environment already created based on the scenario preceding.</span></span> 

## <a name="create-the-nsg-for-the-frontend-subnet"></a><span data-ttu-id="4c1d6-109">Skapa NSG för den `FrontEnd` undernät</span><span class="sxs-lookup"><span data-stu-id="4c1d6-109">Create the NSG for the `FrontEnd` subnet</span></span>

<span data-ttu-id="4c1d6-110">Så här skapar du en NSG som heter *NSG-klientdel* baserat på scenariot ovan, Följ stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-110">To create an NSG named *NSG-FrontEnd* based on the scenario preceding, follow the steps following.</span></span>

1. <span data-ttu-id="4c1d6-111">Om du inte har gjort det ännu, installerar och konfigurerar senast [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in till en Azure med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="4c1d6-111">If you haven't yet, install and configure the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span> 

2. <span data-ttu-id="4c1d6-112">Skapa en NSG med hjälp av den [az nätverket nsg skapa](/cli/azure/network/nsg#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-112">Create an NSG using the [az network nsg create](/cli/azure/network/nsg#create) command.</span></span> 

    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-FrontEnd \
    --location centralus 
    ```

    <span data-ttu-id="4c1d6-113">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="4c1d6-113">Parameters:</span></span>
   
   * <span data-ttu-id="4c1d6-114">`--resource-group`: Namnet på resursgruppen där NSG: N har skapats.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-114">`--resource-group`: Name of the resource group where the NSG is created.</span></span> <span data-ttu-id="4c1d6-115">I vårt exempel, *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-115">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="4c1d6-116">`--location`: Azure-region där den nya NSG skapas.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-116">`--location`: Azure region where the new NSG is created.</span></span> <span data-ttu-id="4c1d6-117">I vårt scenario, *westus*.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-117">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="4c1d6-118">`--name`: Namn på ny NSG: N.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-118">`--name`: Name for the new NSG.</span></span> <span data-ttu-id="4c1d6-119">I vårt scenario, *NSG-klientdel*.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-119">For our scenario, *NSG-FrontEnd*.</span></span>

    <span data-ttu-id="4c1d6-120">Förväntad utdata är ganska lite information, inklusive en lista över alla standardregler.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-120">The expected output is quite a bit of information including a list of all the default rules.</span></span> <span data-ttu-id="4c1d6-121">I följande exempel visas de standardregler som använder en JMESPATH frågefilter med den `table` utdataformat:</span><span class="sxs-lookup"><span data-stu-id="4c1d6-121">The following example shows the default rules using a JMESPATH query filter with the `table` output format:</span></span>

    ```azurecli
    az network nsg show \
    -g testrg \
    -n nsg-frontend \
    --query 'defaultSecurityRules[].{Access:access,Desc:description,DestPortRange:destinationPortRange,Direction:direction,Priority:priority}' \
    -o table
    ```
   
   <span data-ttu-id="4c1d6-122">Resultat:</span><span class="sxs-lookup"><span data-stu-id="4c1d6-122">Output:</span></span>

        Access    Desc                                                    DestPortRange    Direction      Priority
        
        Allow     Allow inbound traffic from all VMs in VNET              *                Inbound           65000
        Allow     Allow inbound traffic from azure load balancer          *                Inbound           65001
        Deny      Deny all inbound traffic                                *                Inbound           65500
        Allow     Allow outbound traffic from all VMs to all VMs in VNET  *                Outbound          65000
        Allow     Allow outbound traffic from all VMs to Internet         *                Outbound          65001
        Deny      Deny all outbound traffic                               *                Outbound          65500



3. <span data-ttu-id="4c1d6-123">Skapa en regel som tillåter åtkomst till port 3389 (RDP) från Internet med de [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-123">Create a rule that allows access to port 3389 (RDP) from the Internet with the [az network nsg rule create](/cli/azure/network/nsg/rule#create) command.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4c1d6-124">Beroende på gränssnittet som du använder kan du behöva ändra den `*` tecken i argumenten efter för att expandera argument före körning.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-124">Depending on the shell you are using, you might need to modify the `*` character in the arguments following so as not to expand the argument before execution.</span></span>
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name rdp-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 3389
    ```
   
    <span data-ttu-id="4c1d6-125">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="4c1d6-125">Expected output:</span></span>
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "3389",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule",
        "name": "rdp-rule",
        "priority": 100,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

    <span data-ttu-id="4c1d6-126">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="4c1d6-126">Parameters:</span></span>

    * <span data-ttu-id="4c1d6-127">`--resource-group testrg`: Resursgruppen som ska användas.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-127">`--resource-group testrg`: The resource group to use.</span></span> <span data-ttu-id="4c1d6-128">Observera att den är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-128">Note that it is case-insensitive.</span></span>
    * <span data-ttu-id="4c1d6-129">`--nsg-name NSG-FrontEnd`: Namnet på NSG: N som regeln skapades.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-129">`--nsg-name NSG-FrontEnd`: Name of the NSG in which the rule is created.</span></span>
    * <span data-ttu-id="4c1d6-130">`--name rdp-rule`: Namnet på den nya regeln.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-130">`--name rdp-rule`: Name for the new rule.</span></span>
    * <span data-ttu-id="4c1d6-131">`--access Allow`: Åtkomstnivå för regeln (Tillåt eller neka).</span><span class="sxs-lookup"><span data-stu-id="4c1d6-131">`--access Allow`: Access level for the rule (Deny or Allow).</span></span>
    * <span data-ttu-id="4c1d6-132">`--protocol Tcp`: Protokoll (Tcp, Udp eller *).</span><span class="sxs-lookup"><span data-stu-id="4c1d6-132">`--protocol Tcp`: Protocol (Tcp, Udp, or *).</span></span>
    * <span data-ttu-id="4c1d6-133">`--direction Inbound`: Riktning anslutningen (inkommande eller utgående).</span><span class="sxs-lookup"><span data-stu-id="4c1d6-133">`--direction Inbound`: Direction of the connection (Inbound or Outbound).</span></span>
    * <span data-ttu-id="4c1d6-134">`--priority 100`: Prioritet för regeln.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-134">`--priority 100`: Priority for the rule.</span></span>
    * <span data-ttu-id="4c1d6-135">`--source-address-prefix Internet`: Källadress-prefix i CIDR- eller använda standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-135">`--source-address-prefix Internet`: Source address prefix in CIDR or using default tags.</span></span>
    * <span data-ttu-id="4c1d6-136">`--source-port-range "*"`: Datakällan port eller ett intervall.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-136">`--source-port-range "*"`: Source port or port range.</span></span> <span data-ttu-id="4c1d6-137">Porten som öppnade anslutningen.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-137">Port that opened the connection.</span></span>
    * <span data-ttu-id="4c1d6-138">`--destination-address-prefix "*"`: Måladress-prefix i CIDR- eller använda standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-138">`--destination-address-prefix "*"`: Destination address prefix in CIDR or using default tags.</span></span>
    * <span data-ttu-id="4c1d6-139">`--destination-port-range 3389`: Mål port eller ett intervall.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-139">`--destination-port-range 3389`: Destination port or port range.</span></span> <span data-ttu-id="4c1d6-140">Porten som tar emot anslutningsbegäran.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-140">Port that receives the connection request.</span></span>



4. <span data-ttu-id="4c1d6-141">Skapa en regel som tillåter åtkomst till port 80 (HTTP) från Internet **az nätverket nsg regeln skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-141">Create a rule that allows access to port 80 (HTTP) from the Internet **az network nsg rule create** command.</span></span>
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name web-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 200 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 80
    ```
   
    <span data-ttu-id="4c1d6-142">Förväntade putput:</span><span class="sxs-lookup"><span data-stu-id="4c1d6-142">Expected putput:</span></span>
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "80",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule",
        "name": "web-rule",
        "priority": 200,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

5. <span data-ttu-id="4c1d6-143">Binda NSG till den **klientdel** undernät med den [az network vnet undernät uppdatering](/cli/azure/network/vnet/subnet#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-143">Bind the NSG to the **FrontEnd** subnet with the [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) command.</span></span>
        
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name FrontEnd \
    --resource-group testrg \
    --network-security-group NSG-FrontEnd
    ```
   
    <span data-ttu-id="4c1d6-144">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="4c1d6-144">Expected output:</span></span>
   
    ```json
    {
        "addressPrefix": "192.168.1.0/24",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/FrontEnd",
        "ipConfigurations": [
            {
            "etag": null,
            "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
            "name": null,
            "privateIpAddress": null,
            "privateIpAllocationMethod": null,
            "provisioningState": null,
            "publicIpAddress": null,
            "resourceGroup": "TestRG",
            "subnet": null
            }
        ],
        "name": "FrontEnd",
        "networkSecurityGroup": {
            "defaultSecurityRules": null,
            "etag": null,
            "id": "/subscriptions/<guid>f/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
            "location": null,
            "name": null,
            "networkInterfaces": null,
            "provisioningState": null,
            "resourceGroup": "testrg",
            "resourceGuid": null,
            "securityRules": null,
            "subnets": null,
            "tags": null,
            "type": null
        },
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "resourceNavigationLinks": null,
        "routeTable": null
    }
    ```

## <a name="create-the-nsg-for-the-backend-subnet"></a><span data-ttu-id="4c1d6-145">Skapa NSG för den `BackEnd` undernät</span><span class="sxs-lookup"><span data-stu-id="4c1d6-145">Create the NSG for the `BackEnd` subnet</span></span>
<span data-ttu-id="4c1d6-146">Så här skapar du en NSG som heter *NSG BackEnd* baserat på scenariot ovan, Följ stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-146">To create an NSG named *NSG-BackEnd* based on the scenario preceding, follow the steps following.</span></span>

1. <span data-ttu-id="4c1d6-147">Skapa den `NSG-BackEnd` NSG med **az nätverket nsg skapa**.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-147">Create the `NSG-BackEnd` NSG with **az network nsg create**.</span></span>
   
    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-BackEnd \
    --location centralus
    ```
   
    <span data-ttu-id="4c1d6-148">Som i steg 2, föregående, är förväntade utdata väldigt stora, inklusive standardregler.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-148">As in step 2, preceding, the expected output is quite large, including default rules.</span></span>
   
2. <span data-ttu-id="4c1d6-149">Skapa en regel som tillåter åtkomst till port 1433 (SQL) från den `FrontEnd` undernät med den **az nätverket nsg regeln skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-149">Create a rule that allows access to port 1433 (SQL) from the `FrontEnd` subnet with the **az network nsg rule create** command.</span></span>
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name sql-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix 192.168.1.0/24 \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 1433
    ```
   
    <span data-ttu-id="4c1d6-150">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="4c1d6-150">Expected output:</span></span>

    ```json  
    {
    "access": "Allow",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "1433",
    "direction": "Inbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/sql-rule",
    "name": "sql-rule",
    "priority": 100,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "192.168.1.0/24",
    "sourcePortRange": "*"
    }
    ```

3. <span data-ttu-id="4c1d6-151">Skapa en regel som nekar åtkomst till Internet med de **az nätverket nsg regeln skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-151">Create a rule that denies access to the Internet using the **az network nsg rule create** command.</span></span>
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name web-rule \
    --access Deny \
    --protocol Tcp  \
    --direction Outbound  \
    --priority 200 \
    --source-address-prefix "*" \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range "*"
    ```
   
    <span data-ttu-id="4c1d6-152">Förväntade putput:</span><span class="sxs-lookup"><span data-stu-id="4c1d6-152">Expected putput:</span></span>
   
    ```json
    {
    "access": "Deny",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "*",
    "direction": "Outbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/web-rule",
    "name": "web-rule",
    "priority": 200,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "*",
    "sourcePortRange": "*"
    }
    ```

4. <span data-ttu-id="4c1d6-153">Binda NSG till den `BackEnd` undernätet med hjälp av den **az network vnet subnet set** kommando.</span><span class="sxs-lookup"><span data-stu-id="4c1d6-153">Bind the NSG to the `BackEnd` subnet using the **az network vnet subnet set** command.</span></span>
   
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name BackEnd \
    --resource-group testrg \
    --network-security-group NSG-BackEnd
    ```
   
    <span data-ttu-id="4c1d6-154">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="4c1d6-154">Expected output:</span></span>
   
    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": {
        "defaultSecurityRules": null,
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "location": null,
        "name": null,
        "networkInterfaces": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "resourceGuid": null,
        "securityRules": null,
        "subnets": null,
        "tags": null,
        "type": null
    },
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```
