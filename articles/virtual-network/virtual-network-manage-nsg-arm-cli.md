---
title: "aaaManage nätverkssäkerhetsgrupper - Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur toomanage nätverkssäkerhetsgrupper med hello Azure-kommandoradsgränssnittet (CLI) 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ed17d314-07e6-4c7f-bcf1-a8a2535d7c14
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a3036b465e1e4049cba00e5e13ce1b479a2301d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-groups-using-hello-azure-cli-20"></a><span data-ttu-id="9635d-103">Hantera nätverkssäkerhetsgrupper med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9635d-103">Manage network security groups using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="9635d-104">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="9635d-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="9635d-105">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="9635d-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="9635d-106">[Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md) – våra CLI för hello klassisk och resurs management distributionsmodeller</span><span class="sxs-lookup"><span data-stu-id="9635d-106">[Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="9635d-107">[Azure CLI 2.0](#View-existing-NSGs) -vår nästa generations CLI för hello management resursdistributionsmodell (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="9635d-107">[Azure CLI 2.0](#View-existing-NSGs) - our next generation CLI for hello resource management deployment model (this article)</span></span>


[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="9635d-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9635d-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="9635d-109">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="9635d-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="prerequisite"></a><span data-ttu-id="9635d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9635d-110">Prerequisite</span></span>
<span data-ttu-id="9635d-111">Om du inte har gjort det ännu, installerar och konfigurerar hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="9635d-111">If you haven't yet, install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span> 


## <a name="view-existing-nsgs"></a><span data-ttu-id="9635d-112">Visa befintliga NSG: er</span><span class="sxs-lookup"><span data-stu-id="9635d-112">View existing NSGs</span></span>
<span data-ttu-id="9635d-113">tooview hello listan över NSG: er i en viss resursgrupp, kör hello [az nsg nätverkslistan](/cli/azure/network/nsg#list) med en `-o table` utdataformat:</span><span class="sxs-lookup"><span data-stu-id="9635d-113">tooview hello list of NSGs in a specific resource group, run hello [az network nsg list](/cli/azure/network/nsg#list) command with a `-o table` output format:</span></span>

```azurecli
az network nsg list -g RG-NSG -o table
```

<span data-ttu-id="9635d-114">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="9635d-114">Expected output:</span></span>

    Location    Name          ProvisioningState    ResourceGroup    ResourceGuid
    ----------  ------------  -------------------  ---------------  ------------------------------------
    centralus   NSG-BackEnd   Succeeded            RG-NSG           <guid>
    centralus   NSG-FrontEnd  Succeeded            RG-NSG           <guid>

## <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="9635d-115">Visa en lista med alla regler för en NSG</span><span class="sxs-lookup"><span data-stu-id="9635d-115">List all rules for an NSG</span></span>
<span data-ttu-id="9635d-116">tooview hello regler för en NSG som heter **NSG-klientdel**kör hello [az nätverket nsg visa](/cli/azure/network/nsg#show) med en [JMESPATH frågefilter](/cli/azure/query-az-cli2) och hello `-o table` utdataformat:</span><span class="sxs-lookup"><span data-stu-id="9635d-116">tooview hello rules of an NSG named **NSG-FrontEnd**, run hello [az network nsg show](/cli/azure/network/nsg#show) command using a [JMESPATH query filter](/cli/azure/query-az-cli2) and hello `-o table` output format:</span></span>

```azurecli
    az network nsg show \
    --resource-group RG-NSG \
    --name NSG-FrontEnd \
    --query '[defaultSecurityRules[],securityRules[]][].{Name:name,Desc:description,Access:access,Direction:direction,DestPortRange:destinationPortRange,DestAddrPrefix:destinationAddressPrefix,SrcPortRange:sourcePortRange,SrcAddrPrefix:sourceAddressPrefix}' \
    -o table
```

<span data-ttu-id="9635d-117">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="9635d-117">Expected output:</span></span>

    Name                           Desc                                                    Access    Direction    DestPortRange    DestAddrPrefix    SrcPortRange    SrcAddrPrefix
    -----------------------------  ------------------------------------------------------  --------  -----------  ---------------  ----------------  --------------  -----------------
    AllowVnetInBound               Allow inbound traffic from all VMs in VNET              Allow     Inbound      *                VirtualNetwork    *               VirtualNetwork
    AllowAzureLoadBalancerInBound  Allow inbound traffic from azure load balancer          Allow     Inbound      *                *                 *               AzureLoadBalancer
    DenyAllInBound                 Deny all inbound traffic                                Deny      Inbound      *                *                 *               *
    AllowVnetOutBound              Allow outbound traffic from all VMs tooall VMs in VNET  Allow     Outbound     *                VirtualNetwork    *               VirtualNetwork
    AllowInternetOutBound          Allow outbound traffic from all VMs tooInternet         Allow     Outbound     *                Internet          *               *
    DenyAllOutBound                Deny all outbound traffic                               Deny      Outbound     *                *                 *               *
    rdp-rule                                                                               Allow     Inbound      3389             *                 *               Internet
    web-rule                                                                               Allow     Inbound      80               *                 *               Internet
> [!NOTE]
> <span data-ttu-id="9635d-118">Du kan också använda [az nätverket nsg regellistan](/cli/azure/network/nsg/rule#list) toolist endast hello anpassade regler från en NSG.</span><span class="sxs-lookup"><span data-stu-id="9635d-118">You can also use [az network nsg rule list](/cli/azure/network/nsg/rule#list) toolist only hello custom rules from an NSG .</span></span>
>

## <a name="view-nsg-associations"></a><span data-ttu-id="9635d-119">Visa NSG associationer</span><span class="sxs-lookup"><span data-stu-id="9635d-119">View NSG associations</span></span>

<span data-ttu-id="9635d-120">tooview vilka resurser hello **NSG-klientdel** NSG är associerat med, kör hello `az network nsg show` som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="9635d-120">tooview what resources hello **NSG-FrontEnd** NSG is associate with, run hello `az network nsg show` command as shown below.</span></span> 

```azurecli
az network nsg show -g RG-NSG -n nsg-frontend --query '[subnets,networkInterfaces]'
```

<span data-ttu-id="9635d-121">Leta efter hello **networkInterfaces** och **undernät** egenskaper som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="9635d-121">Look for hello **networkInterfaces** and **subnets** properties as shown below:</span></span>

```json
[
  [
    {
      "addressPrefix": null,
      "etag": null,
      "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/FrontEnd",
      "ipConfigurations": null,
      "name": null,
      "networkSecurityGroup": null,
      "provisioningState": null,
      "resourceGroup": "RG-NSG",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  null
]
```

<span data-ttu-id="9635d-122">I hello-exemplet ovan, hello NSG inte är associerad tooany nätverksgränssnitt (NIC), och det är associerade tooa undernät med namnet **klientdel**.</span><span class="sxs-lookup"><span data-stu-id="9635d-122">In hello example above, hello NSG is not associated tooany network interfaces (NICs), and it is associated tooa subnet named **FrontEnd**.</span></span>

## <a name="add-a-rule"></a><span data-ttu-id="9635d-123">Lägg till en regel</span><span class="sxs-lookup"><span data-stu-id="9635d-123">Add a rule</span></span>
<span data-ttu-id="9635d-124">tooadd en regel som tillåter **inkommande** trafik tooport **443** från alla datorn toohello **NSG-klientdel** NSG, ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9635d-124">tooadd a rule allowing **inbound** traffic tooport **443** from any machine toohello **NSG-FrontEnd** NSG, enter hello following command:</span></span>

```azurecli
az network nsg rule create  \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd  \
--name allow-https \
--description "Allow access tooport 443 for HTTPS" \
--access Allow \
--protocol Tcp  \
--direction Inbound \
--priority 102 \
--source-address-prefix "*"  \
--source-port-range "*"  \
--destination-address-prefix "*" \
--destination-port-range "443"
```

<span data-ttu-id="9635d-125">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="9635d-125">Expected output:</span></span>

```json
{
  "access": "Allow",
  "description": "Allow access tooport 443 for HTTPS",
  "destinationAddressPrefix": "*",
  "destinationPortRange": "443",
  "direction": "Inbound",
  "etag": "W/\"<guid>\"",
  "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https",
  "name": "allow-https",
  "priority": 102,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "RG-NSG",
  "sourceAddressPrefix": "*",
  "sourcePortRange": "*"
}
```

## <a name="change-a-rule"></a><span data-ttu-id="9635d-126">Ändra en regel</span><span class="sxs-lookup"><span data-stu-id="9635d-126">Change a rule</span></span>
<span data-ttu-id="9635d-127">toochange hello regeln som skapade ovan tooallow inkommande trafik från hello **Internet** endast köra hello [az uppdatera regel för nätverket nsg](/cli/azure/network/nsg/rule#update) kommando:</span><span class="sxs-lookup"><span data-stu-id="9635d-127">toochange hello rule created above tooallow inbound traffic from hello **Internet** only, run hello [az network nsg rule update](/cli/azure/network/nsg/rule#update) command:</span></span>

```azurecli
az network nsg rule update \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https \
--source-address-prefix Internet
```

<span data-ttu-id="9635d-128">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="9635d-128">Expected output:</span></span>

```json
{
"access": "Allow",
"description": "Allow access tooport 443 for HTTPS",
"destinationAddressPrefix": "*",
"destinationPortRange": "443",
"direction": "Inbound",
"etag": "W/\"<guid>\"",
"id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https",
"name": "allow-https",
"priority": 102,
"protocol": "Tcp",
"provisioningState": "Succeeded",
"resourceGroup": "RG-NSG",
"sourceAddressPrefix": "Internet",
"sourcePortRange": "*"
}
```

## <a name="delete-a-rule"></a><span data-ttu-id="9635d-129">Ta bort en regel</span><span class="sxs-lookup"><span data-stu-id="9635d-129">Delete a rule</span></span>
<span data-ttu-id="9635d-130">toodelete hello regeln som skapade ovan, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9635d-130">toodelete hello rule created above, run hello following command:</span></span>

```azurecli
az network nsg rule delete \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https
```


## <a name="associate-an-nsg-tooa-nic"></a><span data-ttu-id="9635d-131">Koppla en NSG tooa NIC</span><span class="sxs-lookup"><span data-stu-id="9635d-131">Associate an NSG tooa NIC</span></span>
<span data-ttu-id="9635d-132">tooassociate hello **NSG-klientdel** NSG toohello **TestNICWeb1** NIC, Använd hello [az nätverket nic uppdatering](/cli/azure/network/nic#update) kommando:</span><span class="sxs-lookup"><span data-stu-id="9635d-132">tooassociate hello **NSG-FrontEnd** NSG toohello **TestNICWeb1** NIC, use hello [az network nic update](/cli/azure/network/nic#update) command:</span></span>

```azurecli
az network nic update \
--resource-group RG-NSG \
--name TestNICWeb1 \
--network-security-group NSG-FrontEnd    
```

<span data-ttu-id="9635d-133">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="9635d-133">Expected output:</span></span>

```json
{
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": [],
    "internalDnsNameLabel": null,
    "internalDomainNameSuffix": "k0wkaguidnqrh0ud.gx.internal.cloudapp.net",
    "internalFqdn": null
  },
  "enableAcceleratedNetworking": false,
  "enableIpForwarding": false,
  "etag": "W/\"<guid>\"",
  "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1",
  "ipConfigurations": [
    {
      "applicationGatewayBackendAddressPools": null,
      "etag": "W/\"<guid>\"",
      "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1",
      "loadBalancerBackendAddressPools": null,
      "loadBalancerInboundNatRules": null,
      "name": "ipconfig1",
      "primary": true,
      "privateIpAddress": "192.168.1.6",
      "privateIpAddressVersion": "IPv4",
      "privateIpAllocationMethod": "Static",
      "provisioningState": "Succeeded",
      "publicIpAddress": null,
      "resourceGroup": "RG-NSG",
      "subnet": {
        "addressPrefix": null,
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
        "ipConfigurations": null,
        "name": null,
        "networkSecurityGroup": null,
        "provisioningState": null,
        "resourceGroup": "RG-NSG",
        "resourceNavigationLinks": null,
        "routeTable": null
      }
    }
  ],
  "location": "centralus",
  "macAddress": "00-0D-3A-91-A9-60",
  "name": "TestNICWeb1",
  "networkSecurityGroup": {
    "defaultSecurityRules": null,
    "etag": null,
    "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
    "location": null,
    "name": null,
    "networkInterfaces": null,
    "provisioningState": null,
    "resourceGroup": "RG-NSG",
    "resourceGuid": null,
    "securityRules": null,
    "subnets": null,
    "tags": null,
    "type": null
  },
  "primary": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "RG-NSG",
  "resourceGuid": "<guid>",
  "tags": {},
  "type": "Microsoft.Network/networkInterfaces",
  "virtualMachine": null
}
```

## <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="9635d-134">Koppla bort en NSG från ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="9635d-134">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="9635d-135">toodissociate hello **NSG-klientdel** NSG från hello **TestNICWeb1** NIC, kör hello [az uppdatera regel för nätverket nsg](/cli/azure/network/nsg/rule#update) kommandot igen, men ersätt hello `--network-security-group` argument med en tom sträng (`""`).</span><span class="sxs-lookup"><span data-stu-id="9635d-135">toodissociate hello **NSG-FrontEnd** NSG from hello **TestNICWeb1** NIC, run hello [az network nsg rule update](/cli/azure/network/nsg/rule#update) command again but replace hello `--network-security-group` argument with an empty string (`""`).</span></span>

```azurecli
az network nic update --resource-group RG-NSG --name TestNICWeb3 --network-security-group ""
```

<span data-ttu-id="9635d-136">Hello utdata och hello `networkSecurityGroup` nyckeluppsättning toonull.</span><span class="sxs-lookup"><span data-stu-id="9635d-136">In hello output, hello `networkSecurityGroup` key is set toonull.</span></span>

## <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="9635d-137">Koppla bort en NSG från ett undernät</span><span class="sxs-lookup"><span data-stu-id="9635d-137">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="9635d-138">toodissociate hello **NSG-klientdel** NSG från hello **klientdel** undernät, igen kör hello [az uppdatera regel för nätverket nsg](/cli/azure/network/nsg/rule#update) kommandot igen, men ersätt hello `--network-security-group` argument med en tom sträng (`""`).</span><span class="sxs-lookup"><span data-stu-id="9635d-138">toodissociate hello **NSG-FrontEnd** NSG from hello **FrontEnd** subnet, again run hello [az network nsg rule update](/cli/azure/network/nsg/rule#update) command again but replace hello `--network-security-group` argument with an empty string (`""`).</span></span>

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group ""
```

<span data-ttu-id="9635d-139">Hello utdata och hello `networkSecurityGroup` nyckeluppsättning toonull.</span><span class="sxs-lookup"><span data-stu-id="9635d-139">In hello output, hello `networkSecurityGroup` key is set toonull.</span></span>

## <a name="associate-an-nsg-tooa-subnet"></a><span data-ttu-id="9635d-140">Koppla en NSG tooa undernät</span><span class="sxs-lookup"><span data-stu-id="9635d-140">Associate an NSG tooa subnet</span></span>
<span data-ttu-id="9635d-141">tooassociate hello **NSG-klientdel** NSG toohello **klientdel** undernät igen och kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9635d-141">tooassociate hello **NSG-FrontEnd** NSG toohello **FrontEnd** subnet again, run hello following command:</span></span>

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group NSG-FrontEnd
```

<span data-ttu-id="9635d-142">Hello utdata och hello `networkSecurityGroup` nyckel har liknande för hello värdet:</span><span class="sxs-lookup"><span data-stu-id="9635d-142">In hello output, hello `networkSecurityGroup` key has something similar for hello value:</span></span>

```json
"networkSecurityGroup": {
    "defaultSecurityRules": null,
    "etag": null,
    "id": "/subscriptions/0e220bf6-5caa-4e9f-8383-51f16b6c109f/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
    "location": null,
    "name": null,
    "networkInterfaces": null,
    "provisioningState": null,
    "resourceGroup": "RG-NSG",
    "resourceGuid": null,
    "securityRules": null,
    "subnets": null,
    "tags": null,
    "type": null
  }
  ```

## <a name="delete-an-nsg"></a><span data-ttu-id="9635d-143">Ta bort en NSG</span><span class="sxs-lookup"><span data-stu-id="9635d-143">Delete an NSG</span></span>
<span data-ttu-id="9635d-144">Du kan bara ta bort en NSG om den inte är kopplad till tooany resurs.</span><span class="sxs-lookup"><span data-stu-id="9635d-144">You can only delete an NSG if it's not associated tooany resource.</span></span> <span data-ttu-id="9635d-145">toodelete en NSG åtgärderna hello nedan.</span><span class="sxs-lookup"><span data-stu-id="9635d-145">toodelete an NSG, follow hello steps below.</span></span>

1. <span data-ttu-id="9635d-146">associerad toocheck hello resurser tooan NSG, kör hello `azure network nsg show` enligt [visa NSG: er kopplingarna](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="9635d-146">toocheck hello resources associated tooan NSG, run hello `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="9635d-147">Om hello NSG är associerad tooany nätverkskort, kör hello `azure network nic set` enligt [koppla bort en NSG från ett nätverkskort](#Dissociate-an-NSG-from-a-NIC) för varje nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9635d-147">If hello NSG is associated tooany NICs, run hello `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="9635d-148">Om hello NSG är associerad tooany undernät, kör hello `azure network vnet subnet set` enligt [koppla bort en NSG från ett undernät](#Dissociate-an-NSG-from-a-subnet) för varje undernät.</span><span class="sxs-lookup"><span data-stu-id="9635d-148">If hello NSG is associated tooany subnet, run hello `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="9635d-149">toodelete hello NSG, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9635d-149">toodelete hello NSG, run hello following command:</span></span>

    ```azurecli
    az network nsg delete --resource-group RG-NSG --name NSG-FrontEnd
    ```
## <a name="next-steps"></a><span data-ttu-id="9635d-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9635d-150">Next steps</span></span>
* <span data-ttu-id="9635d-151">[Aktivera loggning](virtual-network-nsg-manage-log.md) för NSG: er.</span><span class="sxs-lookup"><span data-stu-id="9635d-151">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

