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
# <a name="manage-network-security-groups-using-hello-azure-cli-20"></a>Hantera nätverkssäkerhetsgrupper med hello Azure CLI 2.0

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet 

Du kan göra hello med hjälp av något av följande versioner av CLI hello: 

- [Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md) – våra CLI för hello klassisk och resurs management distributionsmodeller 
- [Azure CLI 2.0](#View-existing-NSGs) -vår nästa generations CLI för hello management resursdistributionsmodell (den här artikeln)


[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md). Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello klassiska distributionsmodellen.
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="prerequisite"></a>Krav
Om du inte har gjort det ännu, installerar och konfigurerar hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login). 


## <a name="view-existing-nsgs"></a>Visa befintliga NSG: er
tooview hello listan över NSG: er i en viss resursgrupp, kör hello [az nsg nätverkslistan](/cli/azure/network/nsg#list) med en `-o table` utdataformat:

```azurecli
az network nsg list -g RG-NSG -o table
```

Förväntad utdata:

    Location    Name          ProvisioningState    ResourceGroup    ResourceGuid
    ----------  ------------  -------------------  ---------------  ------------------------------------
    centralus   NSG-BackEnd   Succeeded            RG-NSG           <guid>
    centralus   NSG-FrontEnd  Succeeded            RG-NSG           <guid>

## <a name="list-all-rules-for-an-nsg"></a>Visa en lista med alla regler för en NSG
tooview hello regler för en NSG som heter **NSG-klientdel**kör hello [az nätverket nsg visa](/cli/azure/network/nsg#show) med en [JMESPATH frågefilter](/cli/azure/query-az-cli2) och hello `-o table` utdataformat:

```azurecli
    az network nsg show \
    --resource-group RG-NSG \
    --name NSG-FrontEnd \
    --query '[defaultSecurityRules[],securityRules[]][].{Name:name,Desc:description,Access:access,Direction:direction,DestPortRange:destinationPortRange,DestAddrPrefix:destinationAddressPrefix,SrcPortRange:sourcePortRange,SrcAddrPrefix:sourceAddressPrefix}' \
    -o table
```

Förväntad utdata:

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
> Du kan också använda [az nätverket nsg regellistan](/cli/azure/network/nsg/rule#list) toolist endast hello anpassade regler från en NSG.
>

## <a name="view-nsg-associations"></a>Visa NSG associationer

tooview vilka resurser hello **NSG-klientdel** NSG är associerat med, kör hello `az network nsg show` som visas nedan. 

```azurecli
az network nsg show -g RG-NSG -n nsg-frontend --query '[subnets,networkInterfaces]'
```

Leta efter hello **networkInterfaces** och **undernät** egenskaper som visas nedan:

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

I hello-exemplet ovan, hello NSG inte är associerad tooany nätverksgränssnitt (NIC), och det är associerade tooa undernät med namnet **klientdel**.

## <a name="add-a-rule"></a>Lägg till en regel
tooadd en regel som tillåter **inkommande** trafik tooport **443** från alla datorn toohello **NSG-klientdel** NSG, ange hello följande kommando:

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

Förväntad utdata:

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

## <a name="change-a-rule"></a>Ändra en regel
toochange hello regeln som skapade ovan tooallow inkommande trafik från hello **Internet** endast köra hello [az uppdatera regel för nätverket nsg](/cli/azure/network/nsg/rule#update) kommando:

```azurecli
az network nsg rule update \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https \
--source-address-prefix Internet
```

Förväntad utdata:

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

## <a name="delete-a-rule"></a>Ta bort en regel
toodelete hello regeln som skapade ovan, kör hello följande kommando:

```azurecli
az network nsg rule delete \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https
```


## <a name="associate-an-nsg-tooa-nic"></a>Koppla en NSG tooa NIC
tooassociate hello **NSG-klientdel** NSG toohello **TestNICWeb1** NIC, Använd hello [az nätverket nic uppdatering](/cli/azure/network/nic#update) kommando:

```azurecli
az network nic update \
--resource-group RG-NSG \
--name TestNICWeb1 \
--network-security-group NSG-FrontEnd    
```

Förväntad utdata:

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

## <a name="dissociate-an-nsg-from-a-nic"></a>Koppla bort en NSG från ett nätverkskort

toodissociate hello **NSG-klientdel** NSG från hello **TestNICWeb1** NIC, kör hello [az uppdatera regel för nätverket nsg](/cli/azure/network/nsg/rule#update) kommandot igen, men ersätt hello `--network-security-group` argument med en tom sträng (`""`).

```azurecli
az network nic update --resource-group RG-NSG --name TestNICWeb3 --network-security-group ""
```

Hello utdata och hello `networkSecurityGroup` nyckeluppsättning toonull.

## <a name="dissociate-an-nsg-from-a-subnet"></a>Koppla bort en NSG från ett undernät
toodissociate hello **NSG-klientdel** NSG från hello **klientdel** undernät, igen kör hello [az uppdatera regel för nätverket nsg](/cli/azure/network/nsg/rule#update) kommandot igen, men ersätt hello `--network-security-group` argument med en tom sträng (`""`).

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group ""
```

Hello utdata och hello `networkSecurityGroup` nyckeluppsättning toonull.

## <a name="associate-an-nsg-tooa-subnet"></a>Koppla en NSG tooa undernät
tooassociate hello **NSG-klientdel** NSG toohello **klientdel** undernät igen och kör hello följande kommando:

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group NSG-FrontEnd
```

Hello utdata och hello `networkSecurityGroup` nyckel har liknande för hello värdet:

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

## <a name="delete-an-nsg"></a>Ta bort en NSG
Du kan bara ta bort en NSG om den inte är kopplad till tooany resurs. toodelete en NSG åtgärderna hello nedan.

1. associerad toocheck hello resurser tooan NSG, kör hello `azure network nsg show` enligt [visa NSG: er kopplingarna](#View-NSGs-associations).
2. Om hello NSG är associerad tooany nätverkskort, kör hello `azure network nic set` enligt [koppla bort en NSG från ett nätverkskort](#Dissociate-an-NSG-from-a-NIC) för varje nätverkskort. 
3. Om hello NSG är associerad tooany undernät, kör hello `azure network vnet subnet set` enligt [koppla bort en NSG från ett undernät](#Dissociate-an-NSG-from-a-subnet) för varje undernät.
4. toodelete hello NSG, kör hello följande kommando:

    ```azurecli
    az network nsg delete --resource-group RG-NSG --name NSG-FrontEnd
    ```
## <a name="next-steps"></a>Nästa steg
* [Aktivera loggning](virtual-network-nsg-manage-log.md) för NSG: er.

