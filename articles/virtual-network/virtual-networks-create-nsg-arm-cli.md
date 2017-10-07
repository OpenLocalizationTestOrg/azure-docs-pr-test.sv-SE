---
title: "aaaCreate nätverkssäkerhetsgrupper - Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur toocreate och distribuera nätverkssäkerhetsgrupper med hello Azure CLI 2.0."
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
ms.openlocfilehash: 30b1d60676331bf5e2bbbb046c747477be9d3338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-hello-azure-cli-20"></a><span data-ttu-id="46c41-103">Skapa nätverk med hjälp av hello Azure CLI 2.0-säkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="46c41-103">Create network security groups using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="46c41-104">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="46c41-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="46c41-105">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="46c41-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="46c41-106">[Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md) – våra CLI för hello klassisk och resurs management distributionsmodeller</span><span class="sxs-lookup"><span data-stu-id="46c41-106">[Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="46c41-107">[Azure CLI 2.0](#Create-the-nsg-for-the-front-end-subnet) -vår nästa generations CLI för hello management resursdistributionsmodell (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="46c41-107">[Azure CLI 2.0](#Create-the-nsg-for-the-front-end-subnet) - our next generation CLI for hello resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="46c41-108">hello exempel Azure CLI 2.0 kommandon följande förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="46c41-108">hello sample Azure CLI 2.0 commands following expect a simple environment already created based on hello scenario preceding.</span></span> 

## <a name="create-hello-nsg-for-hello-frontend-subnet"></a><span data-ttu-id="46c41-109">Skapa hello NSG för hello `FrontEnd` undernät</span><span class="sxs-lookup"><span data-stu-id="46c41-109">Create hello NSG for hello `FrontEnd` subnet</span></span>

<span data-ttu-id="46c41-110">toocreate en NSG som heter *NSG-klientdel* baserat på hello scenariot ovan, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="46c41-110">toocreate an NSG named *NSG-FrontEnd* based on hello scenario preceding, follow hello steps following.</span></span>

1. <span data-ttu-id="46c41-111">Om du inte har gjort det ännu, installerar och konfigurerar hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="46c41-111">If you haven't yet, install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span> 

2. <span data-ttu-id="46c41-112">Skapa en NSG med hello [az nätverket nsg skapa](/cli/azure/network/nsg#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="46c41-112">Create an NSG using hello [az network nsg create](/cli/azure/network/nsg#create) command.</span></span> 

    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-FrontEnd \
    --location centralus 
    ```

    <span data-ttu-id="46c41-113">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="46c41-113">Parameters:</span></span>
   
   * <span data-ttu-id="46c41-114">`--resource-group`: Namnet på hello resursgruppen där hello NSG skapas.</span><span class="sxs-lookup"><span data-stu-id="46c41-114">`--resource-group`: Name of hello resource group where hello NSG is created.</span></span> <span data-ttu-id="46c41-115">I vårt exempel, *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="46c41-115">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="46c41-116">`--location`: Azure-region där hello ny NSG skapas.</span><span class="sxs-lookup"><span data-stu-id="46c41-116">`--location`: Azure region where hello new NSG is created.</span></span> <span data-ttu-id="46c41-117">I vårt scenario, *westus*.</span><span class="sxs-lookup"><span data-stu-id="46c41-117">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="46c41-118">`--name`: Namn hello ny NSG.</span><span class="sxs-lookup"><span data-stu-id="46c41-118">`--name`: Name for hello new NSG.</span></span> <span data-ttu-id="46c41-119">I vårt scenario, *NSG-klientdel*.</span><span class="sxs-lookup"><span data-stu-id="46c41-119">For our scenario, *NSG-FrontEnd*.</span></span>

    <span data-ttu-id="46c41-120">hello förväntade utdata är ganska lite information, inklusive en lista över alla hello standardregler.</span><span class="sxs-lookup"><span data-stu-id="46c41-120">hello expected output is quite a bit of information including a list of all hello default rules.</span></span> <span data-ttu-id="46c41-121">hello följande exempel visar hello standardregler med hello en JMESPATH frågefilter `table` utdataformat:</span><span class="sxs-lookup"><span data-stu-id="46c41-121">hello following example shows hello default rules using a JMESPATH query filter with hello `table` output format:</span></span>

    ```azurecli
    az network nsg show \
    -g testrg \
    -n nsg-frontend \
    --query 'defaultSecurityRules[].{Access:access,Desc:description,DestPortRange:destinationPortRange,Direction:direction,Priority:priority}' \
    -o table
    ```
   
   <span data-ttu-id="46c41-122">Resultat:</span><span class="sxs-lookup"><span data-stu-id="46c41-122">Output:</span></span>

        Access    Desc                                                    DestPortRange    Direction      Priority
        
        Allow     Allow inbound traffic from all VMs in VNET              *                Inbound           65000
        Allow     Allow inbound traffic from azure load balancer          *                Inbound           65001
        Deny      Deny all inbound traffic                                *                Inbound           65500
        Allow     Allow outbound traffic from all VMs tooall VMs in VNET  *                Outbound          65000
        Allow     Allow outbound traffic from all VMs tooInternet         *                Outbound          65001
        Deny      Deny all outbound traffic                               *                Outbound          65500



3. <span data-ttu-id="46c41-123">Skapa en regel som tillåter åtkomst tooport 3389 (RDP) från hello Internet med hello [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="46c41-123">Create a rule that allows access tooport 3389 (RDP) from hello Internet with hello [az network nsg rule create](/cli/azure/network/nsg/rule#create) command.</span></span>

    > [!NOTE]
    > <span data-ttu-id="46c41-124">Beroende på hello skal som du använder, kanske du måste toomodify hello `*` tecken i hello argument efter det inte tooexpand hello argument innan körningen.</span><span class="sxs-lookup"><span data-stu-id="46c41-124">Depending on hello shell you are using, you might need toomodify hello `*` character in hello arguments following so as not tooexpand hello argument before execution.</span></span>
   
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
   
    <span data-ttu-id="46c41-125">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="46c41-125">Expected output:</span></span>
   
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

    <span data-ttu-id="46c41-126">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="46c41-126">Parameters:</span></span>

    * <span data-ttu-id="46c41-127">`--resource-group testrg`: hello resurs grupp toouse.</span><span class="sxs-lookup"><span data-stu-id="46c41-127">`--resource-group testrg`: hello resource group toouse.</span></span> <span data-ttu-id="46c41-128">Observera att den är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="46c41-128">Note that it is case-insensitive.</span></span>
    * <span data-ttu-id="46c41-129">`--nsg-name NSG-FrontEnd`: Namnet på hello NSG i vilka hello regeln har skapats.</span><span class="sxs-lookup"><span data-stu-id="46c41-129">`--nsg-name NSG-FrontEnd`: Name of hello NSG in which hello rule is created.</span></span>
    * <span data-ttu-id="46c41-130">`--name rdp-rule`: Namnet på hello nya regeln.</span><span class="sxs-lookup"><span data-stu-id="46c41-130">`--name rdp-rule`: Name for hello new rule.</span></span>
    * <span data-ttu-id="46c41-131">`--access Allow`: Åtkomstnivå för hello regel (Tillåt eller neka).</span><span class="sxs-lookup"><span data-stu-id="46c41-131">`--access Allow`: Access level for hello rule (Deny or Allow).</span></span>
    * <span data-ttu-id="46c41-132">`--protocol Tcp`: Protokoll (Tcp, Udp eller *).</span><span class="sxs-lookup"><span data-stu-id="46c41-132">`--protocol Tcp`: Protocol (Tcp, Udp, or *).</span></span>
    * <span data-ttu-id="46c41-133">`--direction Inbound`: Riktning hello-anslutning (inkommande eller utgående).</span><span class="sxs-lookup"><span data-stu-id="46c41-133">`--direction Inbound`: Direction of hello connection (Inbound or Outbound).</span></span>
    * <span data-ttu-id="46c41-134">`--priority 100`: Prioritet för hello regeln.</span><span class="sxs-lookup"><span data-stu-id="46c41-134">`--priority 100`: Priority for hello rule.</span></span>
    * <span data-ttu-id="46c41-135">`--source-address-prefix Internet`: Källadress-prefix i CIDR- eller använda standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="46c41-135">`--source-address-prefix Internet`: Source address prefix in CIDR or using default tags.</span></span>
    * <span data-ttu-id="46c41-136">`--source-port-range "*"`: Datakällan port eller ett intervall.</span><span class="sxs-lookup"><span data-stu-id="46c41-136">`--source-port-range "*"`: Source port or port range.</span></span> <span data-ttu-id="46c41-137">Porten som öppnats hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="46c41-137">Port that opened hello connection.</span></span>
    * <span data-ttu-id="46c41-138">`--destination-address-prefix "*"`: Måladress-prefix i CIDR- eller använda standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="46c41-138">`--destination-address-prefix "*"`: Destination address prefix in CIDR or using default tags.</span></span>
    * <span data-ttu-id="46c41-139">`--destination-port-range 3389`: Mål port eller ett intervall.</span><span class="sxs-lookup"><span data-stu-id="46c41-139">`--destination-port-range 3389`: Destination port or port range.</span></span> <span data-ttu-id="46c41-140">Porten som tar emot hello anslutningsbegäran.</span><span class="sxs-lookup"><span data-stu-id="46c41-140">Port that receives hello connection request.</span></span>



4. <span data-ttu-id="46c41-141">Skapa en regel som tillåter åtkomst tooport 80 (HTTP) från hello Internet **az nätverket nsg regeln skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="46c41-141">Create a rule that allows access tooport 80 (HTTP) from hello Internet **az network nsg rule create** command.</span></span>
   
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
   
    <span data-ttu-id="46c41-142">Förväntade putput:</span><span class="sxs-lookup"><span data-stu-id="46c41-142">Expected putput:</span></span>
   
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

5. <span data-ttu-id="46c41-143">Binda hello NSG toohello **klientdel** undernät med hello [az network vnet undernät uppdatering](/cli/azure/network/vnet/subnet#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="46c41-143">Bind hello NSG toohello **FrontEnd** subnet with hello [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) command.</span></span>
        
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name FrontEnd \
    --resource-group testrg \
    --network-security-group NSG-FrontEnd
    ```
   
    <span data-ttu-id="46c41-144">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="46c41-144">Expected output:</span></span>
   
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

## <a name="create-hello-nsg-for-hello-backend-subnet"></a><span data-ttu-id="46c41-145">Skapa hello NSG för hello `BackEnd` undernät</span><span class="sxs-lookup"><span data-stu-id="46c41-145">Create hello NSG for hello `BackEnd` subnet</span></span>
<span data-ttu-id="46c41-146">toocreate en NSG som heter *NSG BackEnd* baserat på hello scenariot ovan, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="46c41-146">toocreate an NSG named *NSG-BackEnd* based on hello scenario preceding, follow hello steps following.</span></span>

1. <span data-ttu-id="46c41-147">Skapa hello `NSG-BackEnd` NSG med **az nätverket nsg skapa**.</span><span class="sxs-lookup"><span data-stu-id="46c41-147">Create hello `NSG-BackEnd` NSG with **az network nsg create**.</span></span>
   
    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-BackEnd \
    --location centralus
    ```
   
    <span data-ttu-id="46c41-148">Som i steg 2, föregående, förväntade hello utdata är väldigt stora, inklusive standardregler.</span><span class="sxs-lookup"><span data-stu-id="46c41-148">As in step 2, preceding, hello expected output is quite large, including default rules.</span></span>
   
2. <span data-ttu-id="46c41-149">Skapa en regel som tillåter åtkomst tooport 1433 (SQL) från hello `FrontEnd` undernät med hello **az nätverket nsg regeln skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="46c41-149">Create a rule that allows access tooport 1433 (SQL) from hello `FrontEnd` subnet with hello **az network nsg rule create** command.</span></span>
   
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
   
    <span data-ttu-id="46c41-150">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="46c41-150">Expected output:</span></span>

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

3. <span data-ttu-id="46c41-151">Skapa en regel som nekar åtkomst toohello Internet med hello **az nätverket nsg regeln skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="46c41-151">Create a rule that denies access toohello Internet using hello **az network nsg rule create** command.</span></span>
   
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
   
    <span data-ttu-id="46c41-152">Förväntade putput:</span><span class="sxs-lookup"><span data-stu-id="46c41-152">Expected putput:</span></span>
   
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

4. <span data-ttu-id="46c41-153">Binda hello NSG toohello `BackEnd` undernätet med hello **az network vnet subnet set** kommando.</span><span class="sxs-lookup"><span data-stu-id="46c41-153">Bind hello NSG toohello `BackEnd` subnet using hello **az network vnet subnet set** command.</span></span>
   
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name BackEnd \
    --resource-group testrg \
    --network-security-group NSG-BackEnd
    ```
   
    <span data-ttu-id="46c41-154">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="46c41-154">Expected output:</span></span>
   
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
