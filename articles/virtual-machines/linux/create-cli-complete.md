---
title: "Skapa en Linux-miljö med Azure CLI 2.0 | Microsoft Docs"
description: "Skapa lagring, en Linux VM, ett virtuellt nätverk och undernät, belastningsutjämning, ett nätverkskort, en offentlig IP-adress och en nätverkssäkerhetsgrupp alla från grunden med hjälp av Azure CLI 2.0."
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
ms.openlocfilehash: e5c4785428b2150e951923e98079e00808a82d87
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-complete-linux-virtual-machine-with-the-azure-cli"></a><span data-ttu-id="f092e-103">Skapa en fullständig Linux-dator med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f092e-103">Create a complete Linux virtual machine with the Azure CLI</span></span>
<span data-ttu-id="f092e-104">För att snabbt skapa en virtuell dator (VM) i Azure, kan du använda ett enda Azure CLI-kommando som använder standardvärden för att skapa alla nödvändiga stödfiler resurser.</span><span class="sxs-lookup"><span data-stu-id="f092e-104">To quickly create a virtual machine (VM) in Azure, you can use a single Azure CLI command that uses default values to create any required supporting resources.</span></span> <span data-ttu-id="f092e-105">Resurser, till exempel ett virtuellt nätverk, offentlig IP-adress och regler för nätverkssäkerhetsgrupper skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f092e-105">Resources such as a virtual network, public IP address, and network security group rules are automatically created.</span></span> <span data-ttu-id="f092e-106">Mer kontroll över din miljö i produktionen använder, du kan skapa dessa resurser i förväg och sedan lägga till dina virtuella datorer till dem.</span><span class="sxs-lookup"><span data-stu-id="f092e-106">For more control of your environment in production use, you may create these resources ahead of time and then add your VMs to them.</span></span> <span data-ttu-id="f092e-107">Den här artikeln hjälper dig att skapa en virtuell dator och varje stödjande resurs i taget.</span><span class="sxs-lookup"><span data-stu-id="f092e-107">This article guides you through how to create a VM and each of the supporting resources one by one.</span></span>

<span data-ttu-id="f092e-108">Kontrollera att du har installerat senast [Azure CLI 2.0](/cli/azure/install-az-cli2) och loggas på en Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="f092e-108">Make sure that you have installed the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and logged to an Azure account in with [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="f092e-109">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="f092e-109">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="f092e-110">Exempel parameternamn inkluderar *myResourceGroup*, *myVnet*, och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="f092e-110">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="f092e-111">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="f092e-111">Create resource group</span></span>
<span data-ttu-id="f092e-112">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="f092e-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="f092e-113">En resursgrupp måste skapas innan en virtuell dator och stödresurser för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f092e-113">A resource group must be created before a virtual machine and supporting virtual network resources.</span></span> <span data-ttu-id="f092e-114">Skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="f092e-114">Create the resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="f092e-115">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i den *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="f092e-115">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="f092e-116">Som standard är resultatet av Azure CLI-kommandona i JSON (JavaScript Object Notation).</span><span class="sxs-lookup"><span data-stu-id="f092e-116">By default, the output of Azure CLI commands is in JSON (JavaScript Object Notation).</span></span> <span data-ttu-id="f092e-117">Om du vill ändra standardvärdet för utdata till en lista eller tabell, till exempel använda [az konfigurera--utdata](/cli/azure/#configure).</span><span class="sxs-lookup"><span data-stu-id="f092e-117">To change the default output to a list or table, for example, use [az configure --output](/cli/azure/#configure).</span></span> <span data-ttu-id="f092e-118">Du kan också lägga till `--output` ändra i utdataformatet till ett kommando för en gång.</span><span class="sxs-lookup"><span data-stu-id="f092e-118">You can also add `--output` to any command for a one time change in output format.</span></span> <span data-ttu-id="f092e-119">I följande exempel visas JSON-utdata från den `az group create` kommando:</span><span class="sxs-lookup"><span data-stu-id="f092e-119">The following example shows the JSON output from the `az group create` command:</span></span>

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

## <a name="create-a-virtual-network-and-subnet"></a><span data-ttu-id="f092e-120">Skapa ett virtuellt nätverk och undernät</span><span class="sxs-lookup"><span data-stu-id="f092e-120">Create a virtual network and subnet</span></span>
<span data-ttu-id="f092e-121">Nästa du skapar ett virtuellt nätverk i Azure och ett undernät i som du kan skapa dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f092e-121">Next you create a virtual network in Azure and a subnet in to which you can create your VMs.</span></span> <span data-ttu-id="f092e-122">Använd [az network vnet skapa](/cli/azure/network/vnet#create) att skapa ett virtuellt nätverk med namnet *myVnet* med den *192.168.0.0/16* adressprefix.</span><span class="sxs-lookup"><span data-stu-id="f092e-122">Use [az network vnet create](/cli/azure/network/vnet#create) to create a virtual network named *myVnet* with the *192.168.0.0/16* address prefix.</span></span> <span data-ttu-id="f092e-123">Du också lägga till ett undernät med namnet *mySubnet* med adressprefixet för *192.168.1.0/24*:</span><span class="sxs-lookup"><span data-stu-id="f092e-123">You also add a subnet named *mySubnet* with the address prefix of *192.168.1.0/24*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

<span data-ttu-id="f092e-124">Utdata visar undernätet logiskt skapas i det virtuella nätverket:</span><span class="sxs-lookup"><span data-stu-id="f092e-124">The output shows the subnet as logically created inside the virtual network:</span></span>

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


## <a name="create-a-public-ip-address"></a><span data-ttu-id="f092e-125">Skapa en offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="f092e-125">Create a public IP address</span></span>
<span data-ttu-id="f092e-126">Nu skapar vi en offentlig IP-adress med [az nätverket offentliga IP-skapa](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="f092e-126">Now let's create a public IP address with [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="f092e-127">Den här offentliga IP-adressen kan du ansluta till dina virtuella datorer från Internet.</span><span class="sxs-lookup"><span data-stu-id="f092e-127">This public IP address enables you to connect to your VMs from the Internet.</span></span> <span data-ttu-id="f092e-128">Eftersom standardadressen är dynamisk kan vi också skapa en namngiven DNS-post med den `--domain-name-label` alternativet.</span><span class="sxs-lookup"><span data-stu-id="f092e-128">Because the default address is dynamic, we also create a named DNS entry with the `--domain-name-label` option.</span></span> <span data-ttu-id="f092e-129">I följande exempel skapas en offentlig IP-adress med namnet *myPublicIP* med DNS-namnet på *mypublicdns*.</span><span class="sxs-lookup"><span data-stu-id="f092e-129">The following example creates a public IP named *myPublicIP* with the DNS name of *mypublicdns*.</span></span> <span data-ttu-id="f092e-130">Eftersom DNS-namnet måste vara unikt, ange ditt eget unikt DNS-namn:</span><span class="sxs-lookup"><span data-stu-id="f092e-130">Because the DNS name must be unique, provide your own unique DNS name:</span></span>

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

<span data-ttu-id="f092e-131">Resultat:</span><span class="sxs-lookup"><span data-stu-id="f092e-131">Output:</span></span>

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


## <a name="create-a-network-security-group"></a><span data-ttu-id="f092e-132">Skapa en säkerhetsgrupp för nätverk</span><span class="sxs-lookup"><span data-stu-id="f092e-132">Create a network security group</span></span>
<span data-ttu-id="f092e-133">Skapa en säkerhetsgrupp för nätverk för att styra flödet av trafik till och från dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f092e-133">To control the flow of traffic in and out of your VMs, create a network security group.</span></span> <span data-ttu-id="f092e-134">En nätverkssäkerhetsgrupp kan tillämpas på ett nätverkskort eller ett undernät.</span><span class="sxs-lookup"><span data-stu-id="f092e-134">A network security group can be applied to a NIC or subnet.</span></span> <span data-ttu-id="f092e-135">I följande exempel används [az nätverket nsg skapa](/cli/azure/network/nsg#create) om du vill skapa en säkerhetsgrupp för nätverk med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="f092e-135">The following example uses [az network nsg create](/cli/azure/network/nsg#create) to create a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="f092e-136">Du kan definiera regler som tillåter eller nekar viss trafik.</span><span class="sxs-lookup"><span data-stu-id="f092e-136">You define rules that allow or deny the specific traffic.</span></span> <span data-ttu-id="f092e-137">För att tillåta inkommande anslutningar på port 22 (till stöd för SSH), skapa en inkommande regel för nätverkssäkerhetsgruppen med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="f092e-137">To allow inbound connections on port 22 (to support SSH), create an inbound rule for the network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="f092e-138">I följande exempel skapas en regel med namnet *myNetworkSecurityGroupRuleSSH*:</span><span class="sxs-lookup"><span data-stu-id="f092e-138">The following example creates a rule named *myNetworkSecurityGroupRuleSSH*:</span></span>

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

<span data-ttu-id="f092e-139">Lägg till grupp ett annat nätverkssäkerhetsregeln för att tillåta inkommande anslutningar på port 80 (för att stödja webbtrafik).</span><span class="sxs-lookup"><span data-stu-id="f092e-139">To allow inbound connections on port 80 (to support web traffic), add another network security group rule.</span></span> <span data-ttu-id="f092e-140">I följande exempel skapas en regel med namnet *myNetworkSecurityGroupRuleHTTP*:</span><span class="sxs-lookup"><span data-stu-id="f092e-140">The following example creates a rule named *myNetworkSecurityGroupRuleHTTP*:</span></span>

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

<span data-ttu-id="f092e-141">Granska nätverkssäkerhetsgruppen och regler med [az nätverket nsg visa](/cli/azure/network/nsg#show):</span><span class="sxs-lookup"><span data-stu-id="f092e-141">Examine the network security group and rules with [az network nsg show](/cli/azure/network/nsg#show):</span></span>

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

<span data-ttu-id="f092e-142">Resultat:</span><span class="sxs-lookup"><span data-stu-id="f092e-142">Output:</span></span>

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
      "description": "Allow outbound traffic from all VMs to all VMs in VNET",
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
      "description": "Allow outbound traffic from all VMs to Internet",
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

## <a name="create-a-virtual-nic"></a><span data-ttu-id="f092e-143">Skapa ett virtuellt nätverkskort</span><span class="sxs-lookup"><span data-stu-id="f092e-143">Create a virtual NIC</span></span>
<span data-ttu-id="f092e-144">Virtuella nätverkskort (NIC) är tillgängliga via programmering eftersom du kan använda regler för deras användning.</span><span class="sxs-lookup"><span data-stu-id="f092e-144">Virtual network interface cards (NICs) are programmatically available because you can apply rules to their use.</span></span> <span data-ttu-id="f092e-145">Du kan också ha fler än en.</span><span class="sxs-lookup"><span data-stu-id="f092e-145">You can also have more than one.</span></span> <span data-ttu-id="f092e-146">I följande [az nätverket nic skapa](/cli/azure/network/nic#create) kommando du skapar ett nätverkskort med namnet *myNic* och koppla den till nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="f092e-146">In the following [az network nic create](/cli/azure/network/nic#create) command, you create a NIC named *myNic* and associate it with the network security group.</span></span> <span data-ttu-id="f092e-147">Den offentliga IP-adressen *myPublicIP* är även associerat med det virtuella nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="f092e-147">The public IP address *myPublicIP* is also associated with the virtual NIC.</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="f092e-148">Resultat:</span><span class="sxs-lookup"><span data-stu-id="f092e-148">Output:</span></span>

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


## <a name="create-an-availability-set"></a><span data-ttu-id="f092e-149">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="f092e-149">Create an availability set</span></span>
<span data-ttu-id="f092e-150">Tillgänglighetsuppsättningar hjälp spridning din virtuella dator över feldomäner och update-domäner.</span><span class="sxs-lookup"><span data-stu-id="f092e-150">Availability sets help spread your VMs across fault domains and update domains.</span></span> <span data-ttu-id="f092e-151">Även om du bara skapa en virtuell dator just nu, är det bäst att använda tillgänglighetsuppsättningar för att göra det lättare att expandera i framtiden.</span><span class="sxs-lookup"><span data-stu-id="f092e-151">Even though you only create one VM right now, it's best practice to use availability sets to make it easier to expand in the future.</span></span> 

<span data-ttu-id="f092e-152">Feldomäner definiera en gruppering av virtuella datorer som delar en gemensam käll- och strömbrytare.</span><span class="sxs-lookup"><span data-stu-id="f092e-152">Fault domains define a grouping of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="f092e-153">Som standard är de virtuella datorer som har konfigurerats i din tillgänglighetsuppsättning åtskilda över upp till tre feldomäner.</span><span class="sxs-lookup"><span data-stu-id="f092e-153">By default, the virtual machines that are configured within your availability set are separated across up to three fault domains.</span></span> <span data-ttu-id="f092e-154">Maskinvaruproblem i något av dessa feldomäner påverkar inte varje virtuell dator som kör din app.</span><span class="sxs-lookup"><span data-stu-id="f092e-154">A hardware issue in one of these fault domains does not affect every VM that is running your app.</span></span>

<span data-ttu-id="f092e-155">Uppdatera domäner ange grupper av virtuella datorer och underliggande fysiska maskinvara som kan startas på samma gång.</span><span class="sxs-lookup"><span data-stu-id="f092e-155">Update domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at the same time.</span></span> <span data-ttu-id="f092e-156">Den ordning i vilken uppdatering domäner startas om är kanske inte sekventiell under planerat underhåll, men bara en uppdateringsdomän startas i taget.</span><span class="sxs-lookup"><span data-stu-id="f092e-156">During planned maintenance, the order in which update domains are rebooted might not be sequential, but only one update domain is rebooted at a time.</span></span>

<span data-ttu-id="f092e-157">Azure distribuerar automatiskt virtuella datorer i domäner fel- och update när de placeras i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="f092e-157">Azure automatically distributes VMs across the fault and update domains when placing them in an availability set.</span></span> <span data-ttu-id="f092e-158">Mer information finns i [hantera tillgängligheten för virtuella datorer](manage-availability.md).</span><span class="sxs-lookup"><span data-stu-id="f092e-158">For more information, see [managing the availability of VMs](manage-availability.md).</span></span>

<span data-ttu-id="f092e-159">Skapa en tillgänglighetsuppsättning för den virtuella datorn med [az vm tillgänglighetsuppsättning skapa](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="f092e-159">Create an availability set for your VM with [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="f092e-160">I följande exempel skapas en tillgänglighetsuppsättning namngivna *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="f092e-160">The following example creates an availability set named *myAvailabilitySet*:</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

<span data-ttu-id="f092e-161">Feldomäner för utdata anteckningar och uppdatera domäner:</span><span class="sxs-lookup"><span data-stu-id="f092e-161">The output notes fault domains and update domains:</span></span>

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


## <a name="create-the-linux-vms"></a><span data-ttu-id="f092e-162">Skapa de virtuella Linux-datorerna</span><span class="sxs-lookup"><span data-stu-id="f092e-162">Create the Linux VMs</span></span>
<span data-ttu-id="f092e-163">Du har skapat nätverksresurser för att stödja Internet-tillgängliga virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f092e-163">You've created the network resources to support Internet-accessible VMs.</span></span> <span data-ttu-id="f092e-164">Nu skapa en virtuell dator och skydda den med en SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="f092e-164">Now create a VM and secure it with an SSH key.</span></span> <span data-ttu-id="f092e-165">I det här fallet är det dags att skapa en Ubuntu VM baserat på senaste LTS.</span><span class="sxs-lookup"><span data-stu-id="f092e-165">In this case, we're going to create an Ubuntu VM based on the most recent LTS.</span></span> <span data-ttu-id="f092e-166">Du kan hitta ytterligare bilder med [az vm bildlista](/cli/azure/vm/image#list), enligt beskrivningen i [söker Azure VM-bilder](cli-ps-findimage.md).</span><span class="sxs-lookup"><span data-stu-id="f092e-166">You can find additional images with [az vm image list](/cli/azure/vm/image#list), as described in [finding Azure VM images](cli-ps-findimage.md).</span></span>

<span data-ttu-id="f092e-167">Vi kan även ange en SSH-nyckel ska användas för autentisering.</span><span class="sxs-lookup"><span data-stu-id="f092e-167">We also specify an SSH key to use for authentication.</span></span> <span data-ttu-id="f092e-168">Om du inte har en offentlig nyckel SSH kan du [skapa dem](mac-create-ssh-keys.md) eller använda den `--generate-ssh-keys` parameter till skapa dem åt dig.</span><span class="sxs-lookup"><span data-stu-id="f092e-168">If you do not have an SSH public key pair, you can [create them](mac-create-ssh-keys.md) or use the `--generate-ssh-keys` parameter to create them for you.</span></span> <span data-ttu-id="f092e-169">Om du redan ett nyckelpar, den här parametern används befintliga nycklar i `~/.ssh`.</span><span class="sxs-lookup"><span data-stu-id="f092e-169">If you already a key pair, this parameter uses existing keys in `~/.ssh`.</span></span>

<span data-ttu-id="f092e-170">Skapa den virtuella datorn genom att våra resurser och information tillsammans med den [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="f092e-170">Create the VM by bringing all our resources and information together with the [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="f092e-171">I följande exempel skapas en virtuell dator med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="f092e-171">The following example creates a VM named *myVM*:</span></span>

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

<span data-ttu-id="f092e-172">SSH till den virtuella datorn med DNS-posten som du angav när du skapade den offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="f092e-172">SSH to your VM with the DNS entry you provided when you created the public IP address.</span></span> <span data-ttu-id="f092e-173">Detta `fqdn` visas i utdata som du skapar den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="f092e-173">This `fqdn` is shown in the output as you create your VM:</span></span>

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

<span data-ttu-id="f092e-174">Resultat:</span><span class="sxs-lookup"><span data-stu-id="f092e-174">Output:</span></span>

```bash
The authenticity of host 'mypublicdns.eastus.cloudapp.azure.com (13.90.94.252)' can't be established.
ECDSA key fingerprint is SHA256:SylINP80Um6XRTvWiFaNz+H+1jcrKB1IiNgCDDJRj6A.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.eastus.cloudapp.azure.com,13.90.94.252' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.4.0-81-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@myVM:~$
```

<span data-ttu-id="f092e-175">Du kan installera NGINX och se att trafiken flödar till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f092e-175">You can install NGINX and see the traffic flow to the VM.</span></span> <span data-ttu-id="f092e-176">Installera NGINX på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f092e-176">Install NGINX as follows:</span></span>

```bash
sudo apt-get install -y nginx
```

<span data-ttu-id="f092e-177">Om du vill se NGINX standardwebbplatsen i åtgärden, öppna webbläsaren och ange fullständigt domännamn:</span><span class="sxs-lookup"><span data-stu-id="f092e-177">To see the default NGINX site in action, open your web browser and enter your FQDN:</span></span>

![Standard NGINX-platsen på den virtuella datorn](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a><span data-ttu-id="f092e-179">Exportera som en mall</span><span class="sxs-lookup"><span data-stu-id="f092e-179">Export as a template</span></span>
<span data-ttu-id="f092e-180">Vad händer om du vill nu skapa en ytterligare utvecklingsmiljö med samma parametrar eller en produktionsmiljö som matchar det?</span><span class="sxs-lookup"><span data-stu-id="f092e-180">What if you now want to create an additional development environment with the same parameters, or a production environment that matches it?</span></span> <span data-ttu-id="f092e-181">Hanteraren för filserverresurser använder JSON-mallar som definierar alla parametrar för din miljö.</span><span class="sxs-lookup"><span data-stu-id="f092e-181">Resource Manager uses JSON templates that define all the parameters for your environment.</span></span> <span data-ttu-id="f092e-182">Du bygga ut hela miljöer genom att referera till den här JSON-mallen.</span><span class="sxs-lookup"><span data-stu-id="f092e-182">You build out entire environments by referencing this JSON template.</span></span> <span data-ttu-id="f092e-183">Du kan [skapa JSON-mallarna manuellt](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller exportera en befintlig miljö för att skapa JSON-mallen för dig.</span><span class="sxs-lookup"><span data-stu-id="f092e-183">You can [build JSON templates manually](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or export an existing environment to create the JSON template for you.</span></span> <span data-ttu-id="f092e-184">Använd [az exportera](/cli/azure/group#export) att exportera resursgruppens namn på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f092e-184">Use [az group export](/cli/azure/group#export) to export your resource group as follows:</span></span>

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

<span data-ttu-id="f092e-185">Det här kommandot skapar den `myResourceGroup.json` filen i din aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="f092e-185">This command creates the `myResourceGroup.json` file in your current working directory.</span></span> <span data-ttu-id="f092e-186">När du skapar en miljö med den här mallen kan uppmanas du alla resursnamn.</span><span class="sxs-lookup"><span data-stu-id="f092e-186">When you create an environment from this template, you are prompted for all the resource names.</span></span> <span data-ttu-id="f092e-187">Du kan fylla i dessa namn i mallfilen genom att lägga till den `--include-parameter-default-value` parametern till den `az group export` kommando.</span><span class="sxs-lookup"><span data-stu-id="f092e-187">You can populate these names in your template file by adding the `--include-parameter-default-value` parameter to the `az group export` command.</span></span> <span data-ttu-id="f092e-188">Redigera JSON-mall om du vill ange resursnamn, eller [skapa en fil med parameters.json](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) som anger resursnamnen.</span><span class="sxs-lookup"><span data-stu-id="f092e-188">Edit your JSON template to specify the resource names, or [create a parameters.json file](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) that specifies the resource names.</span></span>

<span data-ttu-id="f092e-189">Så här skapar du en miljö med din mall [az distribution skapa](/cli/azure/group/deployment#create) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f092e-189">To create an environment from your template, use [az group deployment create](/cli/azure/group/deployment#create) as follows:</span></span>

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

<span data-ttu-id="f092e-190">Du kanske vill läsa [mer om hur du distribuerar från mallar](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f092e-190">You might want to read [more about how to deploy from templates](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="f092e-191">Läs mer om hur du uppdaterar miljöer, använda parameterfilen och komma åt mallar från en enda lagringsplats inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="f092e-191">Learn about how to incrementally update environments, use the parameters file, and access templates from a single storage location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f092e-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f092e-192">Next steps</span></span>
<span data-ttu-id="f092e-193">Nu är du redo att börja arbeta med flera nätverkskomponenter och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f092e-193">Now you're ready to begin working with multiple networking components and VMs.</span></span> <span data-ttu-id="f092e-194">Du kan använda det här exemplet miljö för att bygga ut ditt program med hjälp av de kärnkomponenter som introducerades här.</span><span class="sxs-lookup"><span data-stu-id="f092e-194">You can use this sample environment to build out your application by using the core components introduced here.</span></span>
