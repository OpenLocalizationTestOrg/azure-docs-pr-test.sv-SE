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
# <a name="create-a-complete-linux-virtual-machine-with-hello-azure-cli"></a>Skapa en fullständig Linux-dator med hello Azure CLI
tooquickly skapa en virtuell dator (VM) i Azure, kan du använda ett enda Azure CLI-kommando som använder standard värden toocreate alla nödvändiga stöder resurser. Resurser, till exempel ett virtuellt nätverk, offentlig IP-adress och regler för nätverkssäkerhetsgrupper skapas automatiskt. Mer kontroll över din miljö i produktionen använder, kan du skapa dessa resurser i förväg och Lägg sedan till din toothem för virtuella datorer. Den här artikeln guidar dig genom hur toocreate en virtuell dator och de olika hello stödresurser i taget.

Kontrollera att du har installerat hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och loggade tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar *myResourceGroup*, *myVnet*, och *myVM*.

## <a name="create-resource-group"></a>Skapa resursgrupp
En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. En resursgrupp måste skapas innan en virtuell dator och stödresurser för virtuellt nätverk. Skapa hello resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:

```azurecli
az group create --name myResourceGroup --location eastus
```

Som standard är hello utdata från Azure CLI-kommandona i JSON (JavaScript Object Notation). toochange hello standard utdata tooa tabellposter, till exempel använda [az konfigurera--utdata](/cli/azure/#configure). Du kan också lägga till `--output` tooany kommandot för en gång ändra i utdataformatet. hello följande exempel visar hello JSON-utdata från hello `az group create` kommando:

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
Sedan du skapar ett virtuellt nätverk i Azure och ett undernät i toowhich som du kan skapa dina virtuella datorer. Använd [az network vnet skapa](/cli/azure/network/vnet#create) toocreate ett virtuellt nätverk med namnet *myVnet* med hello *192.168.0.0/16* adressprefix. Du också lägga till ett undernät med namnet *mySubnet* med hello adressprefixet *192.168.1.0/24*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

hello utdata visar hello undernät logiskt skapas i hello virtuellt nätverk:

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
Nu skapar vi en offentlig IP-adress med [az nätverket offentliga IP-skapa](/cli/azure/network/public-ip#create). Den här offentliga IP-adressen kan du tooconnect tooyour virtuella datorer från hello Internet. Eftersom hello standardadressen är dynamiska, vi också skapa en namngiven DNS-post med hello `--domain-name-label` alternativet. hello följande exempel skapas en offentlig IP-adress med namnet *myPublicIP* med hello DNS-namnet för *mypublicdns*. Eftersom hello DNS-namnet måste vara unikt, ange ditt eget unikt DNS-namn:

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
toocontrol hello flödet av trafik till och från dina virtuella datorer, skapa en säkerhetsgrupp för nätverk. En nätverkssäkerhetsgrupp kan vara tillämpade tooa nätverkskort eller ett undernät. hello följande exempel används [az nätverket nsg skapa](/cli/azure/network/nsg#create) toocreate en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

Du kan definiera regler som tillåter eller nekar hello viss trafik. tooallow inkommande anslutningar på port 22 (toosupport SSH), skapa en inkommande regel för hello nätverkssäkerhetsgrupp med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create). hello följande exempel skapas en regel med namnet *myNetworkSecurityGroupRuleSSH*:

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

tooallow inkommande anslutningar på port 80 (toosupport webbtrafik), Lägg till grupp ett annat nätverkssäkerhetsregeln. hello följande exempel skapas en regel med namnet *myNetworkSecurityGroupRuleHTTP*:

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

Granska hello nätverkssäkerhetsgruppen och regler med [az nätverket nsg visa](/cli/azure/network/nsg#show):

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

## <a name="create-a-virtual-nic"></a>Skapa ett virtuellt nätverkskort
Virtuella nätverkskort (NIC) är tillgängliga via programmering eftersom du kan använda regler tootheir användning. Du kan också ha fler än en. I följande hello [az nätverket nic skapa](/cli/azure/network/nic#create) kommando du skapar ett nätverkskort med namnet *myNic* och koppla den till hello nätverkssäkerhetsgruppen. Hej offentliga IP-adressen *myPublicIP* är även associerat med hello virtuellt nätverkskort.

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
Tillgänglighetsuppsättningar hjälp spridning din virtuella dator över feldomäner och update-domäner. Även om du bara skapa en virtuell dator just nu, är det bästa praxis toouse tillgänglighet anger toomake den enklare tooexpand i hello framtida. 

Feldomäner definiera en gruppering av virtuella datorer som delar en gemensam käll- och strömbrytare. Som standard separeras hello virtuella datorer som har konfigurerats i din tillgänglighetsuppsättning över in toothree feldomäner. Maskinvaruproblem i något av dessa feldomäner påverkar inte varje virtuell dator som kör din app.

Uppdatera domäner ange grupper av virtuella datorer och underliggande fysiska maskinvara som kan startas på hello samtidigt. Hello ordning i vilken uppdatering domäner startas om är kanske inte sekventiell under planerat underhåll, men bara en uppdateringsdomän startas i taget.

Azure distribuerar automatiskt virtuella datorer över hello fel- och update domäner när de placeras i en tillgänglighetsuppsättning. Mer information finns i [hantera hello tillgängligheten för virtuella datorer](manage-availability.md).

Skapa en tillgänglighetsuppsättning för den virtuella datorn med [az vm tillgänglighetsuppsättning skapa](/cli/azure/vm/availability-set#create). hello följande exempel skapas en tillgänglighetsuppsättning namngivna *myAvailabilitySet*:

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

Hej feldomäner för utdata anteckningar och uppdatera domäner:

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


## <a name="create-hello-linux-vms"></a>Skapa hello virtuella Linux-datorer
Du har skapat hello nätverket resurser toosupport Internet-tillgängliga virtuella datorer. Nu skapa en virtuell dator och skydda den med en SSH-nyckel. I det här fallet ska vi toocreate en Ubuntu VM baserat på hello senaste LTS. Du kan hitta ytterligare bilder med [az vm bildlista](/cli/azure/vm/image#list), enligt beskrivningen i [söker Azure VM-bilder](cli-ps-findimage.md).

Vi kan även ange en SSH-nyckel toouse för autentisering. Om du inte har en offentlig nyckel SSH kan du [skapa dem](mac-create-ssh-keys.md) eller använda hello `--generate-ssh-keys` parametern toocreate dem åt dig. Om du redan ett nyckelpar, den här parametern används befintliga nycklar i `~/.ssh`.

Skapa hello VM genom att våra resurser och information tillsammans med hello [az vm skapa](/cli/azure/vm#create) kommando. hello följande exempel skapas en virtuell dator med namnet *myVM*:

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

SSH tooyour VM med hello DNS-posten som du angav när du skapade hello offentlig IP-adress. Detta `fqdn` visas i hello utdata som du skapar den virtuella datorn:

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

Du kan installera NGINX och se hello trafik flödet toohello VM. Installera NGINX på följande sätt:

```bash
sudo apt-get install -y nginx
```

toosee hello NGINX-standardwebbplats i åtgärden, öppna webbläsaren och ange fullständigt domännamn:

![Standard NGINX-platsen på den virtuella datorn](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a>Exportera som en mall
Vad händer om du nu vill toocreate en ytterligare utvecklingsmiljö med hello samma parametrar eller en produktionsmiljö som matchar det? Hanteraren för filserverresurser använder JSON-mallar som definierar alla hello parametrar för din miljö. Du bygga ut hela miljöer genom att referera till den här JSON-mallen. Du kan [skapa JSON-mallarna manuellt](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller exportera en befintlig miljö toocreate hello JSON-mall. Använd [az exportera](/cli/azure/group#export) tooexport din resursgrupp på följande sätt:

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

Det här kommandot skapar hello `myResourceGroup.json` fil i den aktuella arbetskatalogen. När du skapar en miljö med den här mallen kan uppmanas du alla hello resursnamn. Du kan fylla i dessa namn i mallfilen genom att lägga till hello `--include-parameter-default-value` parametern toohello `az group export` kommando. Redigera din JSON mallen toospecify hello resursnamn, eller [skapa en fil med parameters.json](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) som anger hello resursnamn.

toocreate en miljö med din mall, Använd [az distribution skapa](/cli/azure/group/deployment#create) på följande sätt:

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

Du kanske vill tooread [mer om hur toodeploy från mallar](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Läs mer om hur tooincrementally uppdatering miljöer, använda hello parameterfilen och komma åt mallar från en enda lagringsplats.

## <a name="next-steps"></a>Nästa steg
Nu är du redo toobegin arbeta med flera nätverkskomponenter och virtuella datorer. Du kan använda det här exemplet miljö toobuild ut ditt program med hjälp av hello kärnkomponenter introduceras här.
