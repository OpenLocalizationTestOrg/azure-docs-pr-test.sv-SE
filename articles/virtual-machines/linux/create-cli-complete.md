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
# <a name="create-a-complete-linux-virtual-machine-with-the-azure-cli"></a>Skapa en fullständig Linux-dator med Azure CLI
För att snabbt skapa en virtuell dator (VM) i Azure, kan du använda ett enda Azure CLI-kommando som använder standardvärden för att skapa alla nödvändiga stödfiler resurser. Resurser, till exempel ett virtuellt nätverk, offentlig IP-adress och regler för nätverkssäkerhetsgrupper skapas automatiskt. Mer kontroll över din miljö i produktionen använder, du kan skapa dessa resurser i förväg och sedan lägga till dina virtuella datorer till dem. Den här artikeln hjälper dig att skapa en virtuell dator och varje stödjande resurs i taget.

Kontrollera att du har installerat senast [Azure CLI 2.0](/cli/azure/install-az-cli2) och loggas på en Azure-konto med [az inloggningen](/cli/azure/#login).

Ersätt exempel parameternamn med egna värden i följande exempel. Exempel parameternamn inkluderar *myResourceGroup*, *myVnet*, och *myVM*.

## <a name="create-resource-group"></a>Skapa resursgrupp
En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. En resursgrupp måste skapas innan en virtuell dator och stödresurser för virtuellt nätverk. Skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). I följande exempel skapas en resursgrupp med namnet *myResourceGroup* i den *eastus* plats:

```azurecli
az group create --name myResourceGroup --location eastus
```

Som standard är resultatet av Azure CLI-kommandona i JSON (JavaScript Object Notation). Om du vill ändra standardvärdet för utdata till en lista eller tabell, till exempel använda [az konfigurera--utdata](/cli/azure/#configure). Du kan också lägga till `--output` ändra i utdataformatet till ett kommando för en gång. I följande exempel visas JSON-utdata från den `az group create` kommando:

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

## <a name="create-a-virtual-network-and-subnet"></a>Skapa ett virtuellt nätverk och undernät
Nästa du skapar ett virtuellt nätverk i Azure och ett undernät i som du kan skapa dina virtuella datorer. Använd [az network vnet skapa](/cli/azure/network/vnet#create) att skapa ett virtuellt nätverk med namnet *myVnet* med den *192.168.0.0/16* adressprefix. Du också lägga till ett undernät med namnet *mySubnet* med adressprefixet för *192.168.1.0/24*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

Utdata visar undernätet logiskt skapas i det virtuella nätverket:

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


## <a name="create-a-public-ip-address"></a>Skapa en offentlig IP-adress
Nu skapar vi en offentlig IP-adress med [az nätverket offentliga IP-skapa](/cli/azure/network/public-ip#create). Den här offentliga IP-adressen kan du ansluta till dina virtuella datorer från Internet. Eftersom standardadressen är dynamisk kan vi också skapa en namngiven DNS-post med den `--domain-name-label` alternativet. I följande exempel skapas en offentlig IP-adress med namnet *myPublicIP* med DNS-namnet på *mypublicdns*. Eftersom DNS-namnet måste vara unikt, ange ditt eget unikt DNS-namn:

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

Resultat:

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


## <a name="create-a-network-security-group"></a>Skapa en säkerhetsgrupp för nätverk
Skapa en säkerhetsgrupp för nätverk för att styra flödet av trafik till och från dina virtuella datorer. En nätverkssäkerhetsgrupp kan tillämpas på ett nätverkskort eller ett undernät. I följande exempel används [az nätverket nsg skapa](/cli/azure/network/nsg#create) om du vill skapa en säkerhetsgrupp för nätverk med namnet *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

Du kan definiera regler som tillåter eller nekar viss trafik. För att tillåta inkommande anslutningar på port 22 (till stöd för SSH), skapa en inkommande regel för nätverkssäkerhetsgruppen med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create). I följande exempel skapas en regel med namnet *myNetworkSecurityGroupRuleSSH*:

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

Lägg till grupp ett annat nätverkssäkerhetsregeln för att tillåta inkommande anslutningar på port 80 (för att stödja webbtrafik). I följande exempel skapas en regel med namnet *myNetworkSecurityGroupRuleHTTP*:

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

Granska nätverkssäkerhetsgruppen och regler med [az nätverket nsg visa](/cli/azure/network/nsg#show):

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

Resultat:

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

## <a name="create-a-virtual-nic"></a>Skapa ett virtuellt nätverkskort
Virtuella nätverkskort (NIC) är tillgängliga via programmering eftersom du kan använda regler för deras användning. Du kan också ha fler än en. I följande [az nätverket nic skapa](/cli/azure/network/nic#create) kommando du skapar ett nätverkskort med namnet *myNic* och koppla den till nätverkssäkerhetsgruppen. Den offentliga IP-adressen *myPublicIP* är även associerat med det virtuella nätverkskortet.

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

Resultat:

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


## <a name="create-an-availability-set"></a>Skapa en tillgänglighetsuppsättning
Tillgänglighetsuppsättningar hjälp spridning din virtuella dator över feldomäner och update-domäner. Även om du bara skapa en virtuell dator just nu, är det bäst att använda tillgänglighetsuppsättningar för att göra det lättare att expandera i framtiden. 

Feldomäner definiera en gruppering av virtuella datorer som delar en gemensam käll- och strömbrytare. Som standard är de virtuella datorer som har konfigurerats i din tillgänglighetsuppsättning åtskilda över upp till tre feldomäner. Maskinvaruproblem i något av dessa feldomäner påverkar inte varje virtuell dator som kör din app.

Uppdatera domäner ange grupper av virtuella datorer och underliggande fysiska maskinvara som kan startas på samma gång. Den ordning i vilken uppdatering domäner startas om är kanske inte sekventiell under planerat underhåll, men bara en uppdateringsdomän startas i taget.

Azure distribuerar automatiskt virtuella datorer i domäner fel- och update när de placeras i en tillgänglighetsuppsättning. Mer information finns i [hantera tillgängligheten för virtuella datorer](manage-availability.md).

Skapa en tillgänglighetsuppsättning för den virtuella datorn med [az vm tillgänglighetsuppsättning skapa](/cli/azure/vm/availability-set#create). I följande exempel skapas en tillgänglighetsuppsättning namngivna *myAvailabilitySet*:

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

Feldomäner för utdata anteckningar och uppdatera domäner:

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


## <a name="create-the-linux-vms"></a>Skapa de virtuella Linux-datorerna
Du har skapat nätverksresurser för att stödja Internet-tillgängliga virtuella datorer. Nu skapa en virtuell dator och skydda den med en SSH-nyckel. I det här fallet är det dags att skapa en Ubuntu VM baserat på senaste LTS. Du kan hitta ytterligare bilder med [az vm bildlista](/cli/azure/vm/image#list), enligt beskrivningen i [söker Azure VM-bilder](cli-ps-findimage.md).

Vi kan även ange en SSH-nyckel ska användas för autentisering. Om du inte har en offentlig nyckel SSH kan du [skapa dem](mac-create-ssh-keys.md) eller använda den `--generate-ssh-keys` parameter till skapa dem åt dig. Om du redan ett nyckelpar, den här parametern används befintliga nycklar i `~/.ssh`.

Skapa den virtuella datorn genom att våra resurser och information tillsammans med den [az vm skapa](/cli/azure/vm#create) kommando. I följande exempel skapas en virtuell dator med namnet *myVM*:

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

SSH till den virtuella datorn med DNS-posten som du angav när du skapade den offentliga IP-adressen. Detta `fqdn` visas i utdata som du skapar den virtuella datorn:

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

Resultat:

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

Du kan installera NGINX och se att trafiken flödar till den virtuella datorn. Installera NGINX på följande sätt:

```bash
sudo apt-get install -y nginx
```

Om du vill se NGINX standardwebbplatsen i åtgärden, öppna webbläsaren och ange fullständigt domännamn:

![Standard NGINX-platsen på den virtuella datorn](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a>Exportera som en mall
Vad händer om du vill nu skapa en ytterligare utvecklingsmiljö med samma parametrar eller en produktionsmiljö som matchar det? Hanteraren för filserverresurser använder JSON-mallar som definierar alla parametrar för din miljö. Du bygga ut hela miljöer genom att referera till den här JSON-mallen. Du kan [skapa JSON-mallarna manuellt](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller exportera en befintlig miljö för att skapa JSON-mallen för dig. Använd [az exportera](/cli/azure/group#export) att exportera resursgruppens namn på följande sätt:

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

Det här kommandot skapar den `myResourceGroup.json` filen i din aktuella arbetskatalogen. När du skapar en miljö med den här mallen kan uppmanas du alla resursnamn. Du kan fylla i dessa namn i mallfilen genom att lägga till den `--include-parameter-default-value` parametern till den `az group export` kommando. Redigera JSON-mall om du vill ange resursnamn, eller [skapa en fil med parameters.json](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) som anger resursnamnen.

Så här skapar du en miljö med din mall [az distribution skapa](/cli/azure/group/deployment#create) på följande sätt:

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

Du kanske vill läsa [mer om hur du distribuerar från mallar](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Läs mer om hur du uppdaterar miljöer, använda parameterfilen och komma åt mallar från en enda lagringsplats inkrementellt.

## <a name="next-steps"></a>Nästa steg
Nu är du redo att börja arbeta med flera nätverkskomponenter och virtuella datorer. Du kan använda det här exemplet miljö för att bygga ut ditt program med hjälp av de kärnkomponenter som introducerades här.
