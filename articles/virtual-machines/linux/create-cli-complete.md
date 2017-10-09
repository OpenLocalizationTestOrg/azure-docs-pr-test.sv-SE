---
title: "aaaCreate en Linux-miljö med hello Azure CLI 2.0 | Microsoft Docs"
description: "Skapa lagring, en Linux VM, ett virtuellt nätverk och undernät, belastningsutjämning, ett nätverkskort, en offentlig IP-adress och en nätverkssäkerhetsgrupp allt från hello bakgrund med hjälp av hello Azure CLI 2.0."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/06/2017
ms.author: iainfou
ms.openlocfilehash: 7287ea178e76001b84dade628ead04a59dc27f40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-complete-linux-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="96440-103">Skapa en fullständig Linux-dator med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="96440-103">Create a complete Linux virtual machine with hello Azure CLI</span></span>
<span data-ttu-id="96440-104">tooquickly skapa en virtuell dator (VM) i Azure, kan du använda ett enda Azure CLI-kommando som använder standard värden toocreate alla nödvändiga stöder resurser.</span><span class="sxs-lookup"><span data-stu-id="96440-104">tooquickly create a virtual machine (VM) in Azure, you can use a single Azure CLI command that uses default values toocreate any required supporting resources.</span></span> <span data-ttu-id="96440-105">Resurser, till exempel ett virtuellt nätverk, offentlig IP-adress och regler för nätverkssäkerhetsgrupper skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="96440-105">Resources such as a virtual network, public IP address, and network security group rules are automatically created.</span></span> <span data-ttu-id="96440-106">Mer kontroll över din miljö i produktionen använder, kan du skapa dessa resurser i förväg och Lägg sedan till din toothem för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96440-106">For more control of your environment in production use, you may create these resources ahead of time and then add your VMs toothem.</span></span> <span data-ttu-id="96440-107">Den här artikeln guidar dig genom hur toocreate en virtuell dator och de olika hello stödresurser i taget.</span><span class="sxs-lookup"><span data-stu-id="96440-107">This article guides you through how toocreate a VM and each of hello supporting resources one by one.</span></span>

<span data-ttu-id="96440-108">Kontrollera att du har installerat hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och loggade tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="96440-108">Make sure that you have installed hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and logged tooan Azure account in with [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="96440-109">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="96440-109">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="96440-110">Exempel parameternamn inkluderar *myResourceGroup*, *myVnet*, och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="96440-110">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="96440-111">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="96440-111">Create resource group</span></span>
<span data-ttu-id="96440-112">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="96440-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="96440-113">En resursgrupp måste skapas innan en virtuell dator och stödresurser för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="96440-113">A resource group must be created before a virtual machine and supporting virtual network resources.</span></span> <span data-ttu-id="96440-114">Skapa hello resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="96440-114">Create hello resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="96440-115">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="96440-115">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="96440-116">Som standard är hello utdata från Azure CLI-kommandona i JSON (JavaScript Object Notation).</span><span class="sxs-lookup"><span data-stu-id="96440-116">By default, hello output of Azure CLI commands is in JSON (JavaScript Object Notation).</span></span> <span data-ttu-id="96440-117">toochange hello standard utdata tooa tabellposter, till exempel använda [az konfigurera--utdata](/cli/azure/#configure).</span><span class="sxs-lookup"><span data-stu-id="96440-117">toochange hello default output tooa list or table, for example, use [az configure --output](/cli/azure/#configure).</span></span> <span data-ttu-id="96440-118">Du kan också lägga till `--output` tooany kommandot för en gång ändra i utdataformatet.</span><span class="sxs-lookup"><span data-stu-id="96440-118">You can also add `--output` tooany command for a one time change in output format.</span></span> <span data-ttu-id="96440-119">hello följande exempel visar hello JSON-utdata från hello `az group create` kommando:</span><span class="sxs-lookup"><span data-stu-id="96440-119">hello following example shows hello JSON output from hello `az group create` command:</span></span>

```json                       
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "location": "eastus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-a-virtual-network-and-subnet"></a><span data-ttu-id="96440-120">Skapa ett virtuellt nätverk och undernät</span><span class="sxs-lookup"><span data-stu-id="96440-120">Create a virtual network and subnet</span></span>
<span data-ttu-id="96440-121">Sedan du skapar ett virtuellt nätverk i Azure och ett undernät i toowhich som du kan skapa dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96440-121">Next you create a virtual network in Azure and a subnet in toowhich you can create your VMs.</span></span> <span data-ttu-id="96440-122">Använd [az network vnet skapa](/cli/azure/network/vnet#create) toocreate ett virtuellt nätverk med namnet *myVnet* med hello *192.168.0.0/16* adressprefix.</span><span class="sxs-lookup"><span data-stu-id="96440-122">Use [az network vnet create](/cli/azure/network/vnet#create) toocreate a virtual network named *myVnet* with hello *192.168.0.0/16* address prefix.</span></span> <span data-ttu-id="96440-123">Du också lägga till ett undernät med namnet *mySubnet* med hello adressprefixet *192.168.1.0/24*:</span><span class="sxs-lookup"><span data-stu-id="96440-123">You also add a subnet named *mySubnet* with hello address prefix of *192.168.1.0/24*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

<span data-ttu-id="96440-124">hello utdata visar hello undernät logiskt skapas i hello virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="96440-124">hello output shows hello subnet as logically created inside hello virtual network:</span></span>

```json
{
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "location": "eastus",
  "name": "myVnet",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "ed62fd03-e9de-430b-84df-8a3b87cacdbb",
  "subnets": [
    {
      "addressPrefix": "192.168.1.0/24",
      "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
      "ipConfigurations": null,
      "name": "mySubnet",
      "networkSecurityGroup": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  "tags": {},
  "type": "Microsoft.Network/virtualNetworks",
  "virtualNetworkPeerings": null
}
```


## <a name="create-a-public-ip-address"></a><span data-ttu-id="96440-125">Skapa en offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="96440-125">Create a public IP address</span></span>
<span data-ttu-id="96440-126">Nu skapar vi en offentlig IP-adress med [az nätverket offentliga IP-skapa](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="96440-126">Now let's create a public IP address with [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="96440-127">Den här offentliga IP-adressen kan du tooconnect tooyour virtuella datorer från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="96440-127">This public IP address enables you tooconnect tooyour VMs from hello Internet.</span></span> <span data-ttu-id="96440-128">Eftersom hello standardadressen är dynamiska, vi också skapa en namngiven DNS-post med hello `--domain-name-label` alternativet.</span><span class="sxs-lookup"><span data-stu-id="96440-128">Because hello default address is dynamic, we also create a named DNS entry with hello `--domain-name-label` option.</span></span> <span data-ttu-id="96440-129">hello följande exempel skapas en offentlig IP-adress med namnet *myPublicIP* med hello DNS-namnet för *mypublicdns*.</span><span class="sxs-lookup"><span data-stu-id="96440-129">hello following example creates a public IP named *myPublicIP* with hello DNS name of *mypublicdns*.</span></span> <span data-ttu-id="96440-130">Eftersom hello DNS-namnet måste vara unikt, ange ditt eget unikt DNS-namn:</span><span class="sxs-lookup"><span data-stu-id="96440-130">Because hello DNS name must be unique, provide your own unique DNS name:</span></span>

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

<span data-ttu-id="96440-131">Resultat:</span><span class="sxs-lookup"><span data-stu-id="96440-131">Output:</span></span>

```json
{
  "publicIp": {
    "dnsSettings": {
      "domainNameLabel": "mypublicdns",
      "fqdn": "mypublicdns.eastus.cloudapp.azure.com",
      "reverseFqdn": null
    },
    "etag": "W/\"2632aa72-3d2d-4529-b38e-b622b4202925\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": null,
    "ipConfiguration": null,
    "location": "eastus",
    "name": "myPublicIP",
    "provisioningState": "Succeeded",
    "publicIpAddressVersion": "IPv4",
    "publicIpAllocationMethod": "Dynamic",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "4c65de38-71f5-4684-be10-75e605b3e41f",
    "tags": null,
    "type": "Microsoft.Network/publicIPAddresses"
  }
}
```


## <a name="create-a-network-security-group"></a><span data-ttu-id="96440-132">Skapa en säkerhetsgrupp för nätverk</span><span class="sxs-lookup"><span data-stu-id="96440-132">Create a network security group</span></span>
<span data-ttu-id="96440-133">toocontrol hello flödet av trafik till och från dina virtuella datorer, skapa en säkerhetsgrupp för nätverk.</span><span class="sxs-lookup"><span data-stu-id="96440-133">toocontrol hello flow of traffic in and out of your VMs, create a network security group.</span></span> <span data-ttu-id="96440-134">En nätverkssäkerhetsgrupp kan vara tillämpade tooa nätverkskort eller ett undernät.</span><span class="sxs-lookup"><span data-stu-id="96440-134">A network security group can be applied tooa NIC or subnet.</span></span> <span data-ttu-id="96440-135">hello följande exempel används [az nätverket nsg skapa](/cli/azure/network/nsg#create) toocreate en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="96440-135">hello following example uses [az network nsg create](/cli/azure/network/nsg#create) toocreate a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="96440-136">Du kan definiera regler som tillåter eller nekar hello viss trafik.</span><span class="sxs-lookup"><span data-stu-id="96440-136">You define rules that allow or deny hello specific traffic.</span></span> <span data-ttu-id="96440-137">tooallow inkommande anslutningar på port 22 (toosupport SSH), skapa en inkommande regel för hello nätverkssäkerhetsgrupp med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="96440-137">tooallow inbound connections on port 22 (toosupport SSH), create an inbound rule for hello network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="96440-138">hello följande exempel skapas en regel med namnet *myNetworkSecurityGroupRuleSSH*:</span><span class="sxs-lookup"><span data-stu-id="96440-138">hello following example creates a rule named *myNetworkSecurityGroupRuleSSH*:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 22 \
    --access allow
```

<span data-ttu-id="96440-139">tooallow inkommande anslutningar på port 80 (toosupport webbtrafik), Lägg till grupp ett annat nätverkssäkerhetsregeln.</span><span class="sxs-lookup"><span data-stu-id="96440-139">tooallow inbound connections on port 80 (toosupport web traffic), add another network security group rule.</span></span> <span data-ttu-id="96440-140">hello följande exempel skapas en regel med namnet *myNetworkSecurityGroupRuleHTTP*:</span><span class="sxs-lookup"><span data-stu-id="96440-140">hello following example creates a rule named *myNetworkSecurityGroupRuleHTTP*:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleWeb \
    --protocol tcp \
    --priority 1001 \
    --destination-port-range 80 \
    --access allow
```

<span data-ttu-id="96440-141">Granska hello nätverkssäkerhetsgruppen och regler med [az nätverket nsg visa](/cli/azure/network/nsg#show):</span><span class="sxs-lookup"><span data-stu-id="96440-141">Examine hello network security group and rules with [az network nsg show](/cli/azure/network/nsg#show):</span></span>

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

<span data-ttu-id="96440-142">Resultat:</span><span class="sxs-lookup"><span data-stu-id="96440-142">Output:</span></span>

```json
{
  "defaultSecurityRules": [
    {
      "access": "Allow",
      "description": "Allow inbound traffic from all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetInBound",
      "name": "AllowVnetInBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow inbound traffic from azure load balancer",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowAzureLoadBalancerInBou
      "name": "AllowAzureLoadBalancerInBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "AzureLoadBalancer",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all inbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllInBound",
      "name": "DenyAllInBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs tooall VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetOutBound",
      "name": "AllowVnetOutBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs tooInternet",
      "destinationAddressPrefix": "Internet",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowInternetOutBound",
      "name": "AllowInternetOutBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all outbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllOutBound",
      "name": "DenyAllOutBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
  "location": "eastus",
  "name": "myNetworkSecurityGroup",
  "networkInterfaces": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "47a9964e-23a3-438a-a726-8d60ebbb1c3c",
  "securityRules": [
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "22",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleSSH",
      "name": "myNetworkSecurityGroupRuleSSH",
      "priority": 1000,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "80",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleWeb",
      "name": "myNetworkSecurityGroupRuleWeb",
      "priority": 1001,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "subnets": null,
  "tags": null,
  "type": "Microsoft.Network/networkSecurityGroups"
}
```

## <a name="create-a-virtual-nic"></a><span data-ttu-id="96440-143">Skapa ett virtuellt nätverkskort</span><span class="sxs-lookup"><span data-stu-id="96440-143">Create a virtual NIC</span></span>
<span data-ttu-id="96440-144">Virtuella nätverkskort (NIC) är tillgängliga via programmering eftersom du kan använda regler tootheir användning.</span><span class="sxs-lookup"><span data-stu-id="96440-144">Virtual network interface cards (NICs) are programmatically available because you can apply rules tootheir use.</span></span> <span data-ttu-id="96440-145">Du kan också ha fler än en.</span><span class="sxs-lookup"><span data-stu-id="96440-145">You can also have more than one.</span></span> <span data-ttu-id="96440-146">I följande hello [az nätverket nic skapa](/cli/azure/network/nic#create) kommando du skapar ett nätverkskort med namnet *myNic* och koppla den till hello nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="96440-146">In hello following [az network nic create](/cli/azure/network/nic#create) command, you create a NIC named *myNic* and associate it with hello network security group.</span></span> <span data-ttu-id="96440-147">Hej offentliga IP-adressen *myPublicIP* är även associerat med hello virtuellt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="96440-147">hello public IP address *myPublicIP* is also associated with hello virtual NIC.</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="96440-148">Resultat:</span><span class="sxs-lookup"><span data-stu-id="96440-148">Output:</span></span>

```json
{
  "NewNIC": {
    "dnsSettings": {
      "appliedDnsServers": [],
      "dnsServers": [],
      "internalDnsNameLabel": null,
      "internalDomainNameSuffix": "brqlt10lvoxedgkeuomc4pm5tb.bx.internal.cloudapp.net",
      "internalFqdn": null
    },
    "enableAcceleratedNetworking": false,
    "enableIpForwarding": false,
    "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
    "ipConfigurations": [
      {
        "applicationGatewayBackendAddressPools": null,
        "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic/ipConfigurations/ipconfig1",
        "loadBalancerBackendAddressPools": null,
        "loadBalancerInboundNatRules": null,
        "name": "ipconfig1",
        "primary": true,
        "privateIpAddress": "192.168.1.4",
        "privateIpAddressVersion": "IPv4",
        "privateIpAllocationMethod": "Dynamic",
        "provisioningState": "Succeeded",
        "publicIpAddress": {
          "dnsSettings": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
          "idleTimeoutInMinutes": null,
          "ipAddress": null,
          "ipConfiguration": null,
          "location": null,
          "name": null,
          "provisioningState": null,
          "publicIpAddressVersion": null,
          "publicIpAllocationMethod": null,
          "resourceGroup": "myResourceGroup",
          "resourceGuid": null,
          "tags": null,
          "type": null
        },
        "resourceGroup": "myResourceGroup",
        "subnet": {
          "addressPrefix": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
          "ipConfigurations": null,
          "name": null,
          "networkSecurityGroup": null,
          "provisioningState": null,
          "resourceGroup": "myResourceGroup",
          "resourceNavigationLinks": null,
          "routeTable": null
        }
      }
    ],
    "location": "eastus",
    "macAddress": null,
    "name": "myNic",
    "networkSecurityGroup": {
      "defaultSecurityRules": null,
      "etag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
      "location": null,
      "name": null,
      "networkInterfaces": null,
      "provisioningState": null,
      "resourceGroup": "myResourceGroup",
      "resourceGuid": null,
      "securityRules": null,
      "subnets": null,
      "tags": null,
      "type": null
    },
    "primary": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "b3dbaa0e-2cf2-43be-a814-5cc49fea3304",
    "tags": null,
    "type": "Microsoft.Network/networkInterfaces",
    "virtualMachine": null
  }
}
```


## <a name="create-an-availability-set"></a><span data-ttu-id="96440-149">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="96440-149">Create an availability set</span></span>
<span data-ttu-id="96440-150">Tillgänglighetsuppsättningar hjälp spridning din virtuella dator över feldomäner och update-domäner.</span><span class="sxs-lookup"><span data-stu-id="96440-150">Availability sets help spread your VMs across fault domains and update domains.</span></span> <span data-ttu-id="96440-151">Även om du bara skapa en virtuell dator just nu, är det bästa praxis toouse tillgänglighet anger toomake den enklare tooexpand i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="96440-151">Even though you only create one VM right now, it's best practice toouse availability sets toomake it easier tooexpand in hello future.</span></span> 

<span data-ttu-id="96440-152">Feldomäner definiera en gruppering av virtuella datorer som delar en gemensam käll- och strömbrytare.</span><span class="sxs-lookup"><span data-stu-id="96440-152">Fault domains define a grouping of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="96440-153">Som standard separeras hello virtuella datorer som har konfigurerats i din tillgänglighetsuppsättning över in toothree feldomäner.</span><span class="sxs-lookup"><span data-stu-id="96440-153">By default, hello virtual machines that are configured within your availability set are separated across up toothree fault domains.</span></span> <span data-ttu-id="96440-154">Maskinvaruproblem i något av dessa feldomäner påverkar inte varje virtuell dator som kör din app.</span><span class="sxs-lookup"><span data-stu-id="96440-154">A hardware issue in one of these fault domains does not affect every VM that is running your app.</span></span>

<span data-ttu-id="96440-155">Uppdatera domäner ange grupper av virtuella datorer och underliggande fysiska maskinvara som kan startas på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="96440-155">Update domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at hello same time.</span></span> <span data-ttu-id="96440-156">Hello ordning i vilken uppdatering domäner startas om är kanske inte sekventiell under planerat underhåll, men bara en uppdateringsdomän startas i taget.</span><span class="sxs-lookup"><span data-stu-id="96440-156">During planned maintenance, hello order in which update domains are rebooted might not be sequential, but only one update domain is rebooted at a time.</span></span>

<span data-ttu-id="96440-157">Azure distribuerar automatiskt virtuella datorer över hello fel- och update domäner när de placeras i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="96440-157">Azure automatically distributes VMs across hello fault and update domains when placing them in an availability set.</span></span> <span data-ttu-id="96440-158">Mer information finns i [hantera hello tillgängligheten för virtuella datorer](manage-availability.md).</span><span class="sxs-lookup"><span data-stu-id="96440-158">For more information, see [managing hello availability of VMs](manage-availability.md).</span></span>

<span data-ttu-id="96440-159">Skapa en tillgänglighetsuppsättning för den virtuella datorn med [az vm tillgänglighetsuppsättning skapa](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="96440-159">Create an availability set for your VM with [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="96440-160">hello följande exempel skapas en tillgänglighetsuppsättning namngivna *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="96440-160">hello following example creates an availability set named *myAvailabilitySet*:</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

<span data-ttu-id="96440-161">Hej feldomäner för utdata anteckningar och uppdatera domäner:</span><span class="sxs-lookup"><span data-stu-id="96440-161">hello output notes fault domains and update domains:</span></span>

```json
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet",
  "location": "eastus",
  "managed": null,
  "name": "myAvailabilitySet",
  "platformFaultDomainCount": 2,
  "platformUpdateDomainCount": 5,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": null,
    "managed": true,
    "tier": null
  },
  "statuses": null,
  "tags": {},
  "type": "Microsoft.Compute/availabilitySets",
  "virtualMachines": []
}
```


## <a name="create-hello-linux-vms"></a><span data-ttu-id="96440-162">Skapa hello virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="96440-162">Create hello Linux VMs</span></span>
<span data-ttu-id="96440-163">Du har skapat hello nätverket resurser toosupport Internet-tillgängliga virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96440-163">You've created hello network resources toosupport Internet-accessible VMs.</span></span> <span data-ttu-id="96440-164">Nu skapa en virtuell dator och skydda den med en SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="96440-164">Now create a VM and secure it with an SSH key.</span></span> <span data-ttu-id="96440-165">I det här fallet ska vi toocreate en Ubuntu VM baserat på hello senaste LTS.</span><span class="sxs-lookup"><span data-stu-id="96440-165">In this case, we're going toocreate an Ubuntu VM based on hello most recent LTS.</span></span> <span data-ttu-id="96440-166">Du kan hitta ytterligare bilder med [az vm bildlista](/cli/azure/vm/image#list), enligt beskrivningen i [söker Azure VM-bilder](cli-ps-findimage.md).</span><span class="sxs-lookup"><span data-stu-id="96440-166">You can find additional images with [az vm image list](/cli/azure/vm/image#list), as described in [finding Azure VM images](cli-ps-findimage.md).</span></span>

<span data-ttu-id="96440-167">Vi kan även ange en SSH-nyckel toouse för autentisering.</span><span class="sxs-lookup"><span data-stu-id="96440-167">We also specify an SSH key toouse for authentication.</span></span> <span data-ttu-id="96440-168">Om du inte har en offentlig nyckel SSH kan du [skapa dem](mac-create-ssh-keys.md) eller använda hello `--generate-ssh-keys` parametern toocreate dem åt dig.</span><span class="sxs-lookup"><span data-stu-id="96440-168">If you do not have an SSH public key pair, you can [create them](mac-create-ssh-keys.md) or use hello `--generate-ssh-keys` parameter toocreate them for you.</span></span> <span data-ttu-id="96440-169">Om du redan ett nyckelpar, den här parametern används befintliga nycklar i `~/.ssh`.</span><span class="sxs-lookup"><span data-stu-id="96440-169">If you already a key pair, this parameter uses existing keys in `~/.ssh`.</span></span>

<span data-ttu-id="96440-170">Skapa hello VM genom att våra resurser och information tillsammans med hello [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="96440-170">Create hello VM by bringing all our resources and information together with hello [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="96440-171">hello följande exempel skapas en virtuell dator med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="96440-171">hello following example creates a VM named *myVM*:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --availability-set myAvailabilitySet \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="96440-172">SSH tooyour VM med hello DNS-posten som du angav när du skapade hello offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="96440-172">SSH tooyour VM with hello DNS entry you provided when you created hello public IP address.</span></span> <span data-ttu-id="96440-173">Detta `fqdn` visas i hello utdata som du skapar den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="96440-173">This `fqdn` is shown in hello output as you create your VM:</span></span>

```json
{
  "fqdns": "mypublicdns.eastus.cloudapp.azure.com",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-13-71-C8",
  "powerState": "VM running",
  "privateIpAddress": "192.168.1.5",
  "publicIpAddress": "13.90.94.252",
  "resourceGroup": "myResourceGroup"
}
```

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

<span data-ttu-id="96440-174">Resultat:</span><span class="sxs-lookup"><span data-stu-id="96440-174">Output:</span></span>

```bash
hello authenticity of host 'mypublicdns.eastus.cloudapp.azure.com (13.90.94.252)' can't be established.
ECDSA key fingerprint is SHA256:SylINP80Um6XRTvWiFaNz+H+1jcrKB1IiNgCDDJRj6A.
Are you sure you want toocontinue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.eastus.cloudapp.azure.com,13.90.94.252' (ECDSA) toohello list of known hosts.
Welcome tooUbuntu 16.04.2 LTS (GNU/Linux 4.4.0-81-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


hello programs included with hello Ubuntu system are free software;
hello exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, toohello extent permitted by
applicable law.

toorun a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@myVM:~$
```

<span data-ttu-id="96440-175">Du kan installera NGINX och se hello trafik flödet toohello VM.</span><span class="sxs-lookup"><span data-stu-id="96440-175">You can install NGINX and see hello traffic flow toohello VM.</span></span> <span data-ttu-id="96440-176">Installera NGINX på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="96440-176">Install NGINX as follows:</span></span>

```bash
sudo apt-get install -y nginx
```

<span data-ttu-id="96440-177">toosee hello NGINX-standardwebbplats i åtgärden, öppna webbläsaren och ange fullständigt domännamn:</span><span class="sxs-lookup"><span data-stu-id="96440-177">toosee hello default NGINX site in action, open your web browser and enter your FQDN:</span></span>

![Standard NGINX-platsen på den virtuella datorn](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a><span data-ttu-id="96440-179">Exportera som en mall</span><span class="sxs-lookup"><span data-stu-id="96440-179">Export as a template</span></span>
<span data-ttu-id="96440-180">Vad händer om du nu vill toocreate en ytterligare utvecklingsmiljö med hello samma parametrar eller en produktionsmiljö som matchar det?</span><span class="sxs-lookup"><span data-stu-id="96440-180">What if you now want toocreate an additional development environment with hello same parameters, or a production environment that matches it?</span></span> <span data-ttu-id="96440-181">Hanteraren för filserverresurser använder JSON-mallar som definierar alla hello parametrar för din miljö.</span><span class="sxs-lookup"><span data-stu-id="96440-181">Resource Manager uses JSON templates that define all hello parameters for your environment.</span></span> <span data-ttu-id="96440-182">Du bygga ut hela miljöer genom att referera till den här JSON-mallen.</span><span class="sxs-lookup"><span data-stu-id="96440-182">You build out entire environments by referencing this JSON template.</span></span> <span data-ttu-id="96440-183">Du kan [skapa JSON-mallarna manuellt](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller exportera en befintlig miljö toocreate hello JSON-mall.</span><span class="sxs-lookup"><span data-stu-id="96440-183">You can [build JSON templates manually](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or export an existing environment toocreate hello JSON template for you.</span></span> <span data-ttu-id="96440-184">Använd [az exportera](/cli/azure/group#export) tooexport din resursgrupp på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="96440-184">Use [az group export](/cli/azure/group#export) tooexport your resource group as follows:</span></span>

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

<span data-ttu-id="96440-185">Det här kommandot skapar hello `myResourceGroup.json` fil i den aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="96440-185">This command creates hello `myResourceGroup.json` file in your current working directory.</span></span> <span data-ttu-id="96440-186">När du skapar en miljö med den här mallen kan uppmanas du alla hello resursnamn.</span><span class="sxs-lookup"><span data-stu-id="96440-186">When you create an environment from this template, you are prompted for all hello resource names.</span></span> <span data-ttu-id="96440-187">Du kan fylla i dessa namn i mallfilen genom att lägga till hello `--include-parameter-default-value` parametern toohello `az group export` kommando.</span><span class="sxs-lookup"><span data-stu-id="96440-187">You can populate these names in your template file by adding hello `--include-parameter-default-value` parameter toohello `az group export` command.</span></span> <span data-ttu-id="96440-188">Redigera din JSON mallen toospecify hello resursnamn, eller [skapa en fil med parameters.json](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) som anger hello resursnamn.</span><span class="sxs-lookup"><span data-stu-id="96440-188">Edit your JSON template toospecify hello resource names, or [create a parameters.json file](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) that specifies hello resource names.</span></span>

<span data-ttu-id="96440-189">toocreate en miljö med din mall, Använd [az distribution skapa](/cli/azure/group/deployment#create) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="96440-189">toocreate an environment from your template, use [az group deployment create](/cli/azure/group/deployment#create) as follows:</span></span>

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

<span data-ttu-id="96440-190">Du kanske vill tooread [mer om hur toodeploy från mallar](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="96440-190">You might want tooread [more about how toodeploy from templates](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="96440-191">Läs mer om hur tooincrementally uppdatering miljöer, använda hello parameterfilen och komma åt mallar från en enda lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="96440-191">Learn about how tooincrementally update environments, use hello parameters file, and access templates from a single storage location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="96440-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="96440-192">Next steps</span></span>
<span data-ttu-id="96440-193">Nu är du redo toobegin arbeta med flera nätverkskomponenter och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96440-193">Now you're ready toobegin working with multiple networking components and VMs.</span></span> <span data-ttu-id="96440-194">Du kan använda det här exemplet miljö toobuild ut ditt program med hjälp av hello kärnkomponenter introduceras här.</span><span class="sxs-lookup"><span data-stu-id="96440-194">You can use this sample environment toobuild out your application by using hello core components introduced here.</span></span>
