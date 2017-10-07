---
title: "aaaCreate en fullständig Linux-miljö med hello Azure CLI 1.0 | Microsoft Docs"
description: "Skapa lagring, en Linux VM, ett virtuellt nätverk och undernät, belastningsutjämning, ett nätverkskort, en offentlig IP-adress och en nätverkssäkerhetsgrupp allt från hello bakgrund med hjälp av hello Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 7fe00e138704fe9c9a1c9b87a7dd1afd6174e527
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-complete-linux-environment-with-hello-azure-cli-10"></a>Skapa en fullständig Linux-miljö med hello Azure CLI 1.0
I den här artikeln skapar vi ett enkelt nätverk med en belastningsutjämnare och ett par med virtuella datorer som är användbara för utveckling och enkel datoranvändning. Vi går igenom processen hello kommandot och kommandot, tills du har två fungerande säkra virtuella Linux-datorer toowhich du ansluter från var som helst på hello Internet. Du kan flytta på toomore komplexa nätverk och miljöer.

Hello vägen du lär dig mer om hello beroende hierarkin att hello Resource Manager-distributionsmodellen ger dig och om hur mycket energi ger. När du ser hur hello systemet byggs, kan du återskapa den mycket snabbare med hjälp av [Azure Resource Manager-mallar](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Även efter att du lär dig hur hello delar av din miljö fungerar ihop, hur du skapar mallar tooautomate dem blir enklare.

hello miljö innehåller:

* Två virtuella datorer i en tillgänglighetsuppsättning.
* En belastningsutjämnare med en regel för belastningsutjämning på port 80.
* Nätverkssäkerhetsgrupp (NSG) regler tooprotect den virtuella datorn från oönskad trafik.

toocreate den här anpassade miljön, behöver du hello senaste [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) i Resource Manager-läget (`azure config mode arm`). Du måste också en JSON-parsning verktyget. Det här exemplet används [jq](https://stedolan.github.io/jq/).


## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering


## <a name="quick-commands"></a>Snabbkommandon
Om du behöver tooquickly utföra hello uppgiften, hello följande avsnitt information hello bas kommandon tooupload tooAzure en VM. Mer detaljerad information och kontext för varje steg i hello resten av hello dokumentet med början [här](#detailed-walkthrough).

Se till att du har [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) loggas och med hjälp av Resource Manager-läge:

```azurecli
azure config mode arm
```

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar `myResourceGroup`, `mystorageaccount`, och `myVM`.

Skapa hello resursgrupp. hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westeurope` plats:

```azurecli
azure group create -n myResourceGroup -l westeurope
```

Kontrollera hello resursgrupp med hjälp av hello JSON-parsern:

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Skapa hello storage-konto. hello följande exempel skapas ett lagringskonto med namnet `mystorageaccount`. (hello lagringskontonamn måste vara unikt, så ange dina egna unika namn)

```azurecli
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Kontrollera hello storage-konto med hjälp av hello JSON-parsern:

```azurecli
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Skapa hello virtuellt nätverk. hello följande exempel skapas ett virtuellt nätverk med namnet `myVnet`:

```azurecli
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Skapa ett undernät. hello följande exempel skapas ett undernät med namnet `mySubnet`:

```azurecli
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Verifiera hello virtuellt nätverk och undernät med hello JSON-parsern:

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Skapa en offentlig IP-adress. hello följande exempel skapas en offentlig IP-adress med namnet `myPublicIP` med hello DNS-namnet på `mypublicdns`. (hello DNS-namn måste vara unikt, så ange dina egna unika namn)

```azurecli
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Skapa hello belastningsutjämnaren. hello följande exempel skapas en belastningsutjämnare med namnet `myLoadBalancer`:

```azurecli
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Skapa en frontend IP-adresspool för hello belastningsutjämnare och associera hello offentlig IP. hello följande exempel skapas en frontend IP-pool med namnet `mySubnetPool`:

```azurecli
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool
```

Skapa hello backend-IP-adresspool för hello belastningsutjämnaren. hello följande exempel skapas en backend-IP-pool med namnet `myBackEndPool`:

```azurecli
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Skapa SSH inkommande network adress translation (NAT) regler för hello belastningsutjämnaren. hello följande exempel skapar två regler för inläsning av belastningsutjämnare, `myLoadBalancerRuleSSH1` och `myLoadBalancerRuleSSH2`:

```azurecli
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Skapa hello webb ingående NAT-regler för hello belastningsutjämnare. hello följande exempel skapas en regel för belastningsutjämnare med namnet `myLoadBalancerRuleWeb`:

```azurecli
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Skapa hello belastningsutjämnaren, hälsoavsökningen. hello följande exempel skapas en TCP-avsökning med namnet `myHealthProbe`:

```azurecli
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Kontrollera hello belastningsutjämnare, IP-adresspooler och NAT-regler med hjälp av hello JSON-parsern:

```azurecli
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Skapa hello första nätverksgränssnittskortet (NIC). Ersätt hello `#####-###-###` avsnitt med din egen Azure prenumerations-ID. Din prenumeration ID anges i hello utdata från **jq** när du undersöker hello-resurser som du skapar. Du kan också visa ditt prenumerations-ID med `azure account list`.

hello följande exempel skapas ett nätverkskort med namnet `myNic1`:

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Skapa hello andra nätverkskort. hello följande exempel skapas ett nätverkskort med namnet `myNic2`:

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Verifiera hello två nätverkskort med hello JSON-parsern:

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Skapa hello nätverkssäkerhetsgruppen. hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet `myNetworkSecurityGroup`:

```azurecli
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Lägga till två regler för inkommande trafik för hello nätverkssäkerhetsgruppen. hello följande exempel skapar två regler `myNetworkSecurityGroupRuleSSH` och `myNetworkSecurityGroupRuleHTTP`:

```azurecli
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Kontrollera hello nätverkssäkerhetsgruppen och regler för inkommande trafik med hjälp av hello JSON-parsern:

```azurecli
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

Binda hello nätverkssäkerhet gruppera toohello två nätverkskort:

```azurecli
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Skapa hello tillgänglighetsuppsättning. hello följande exempel skapas en tillgänglighetsuppsättning namngivna `myAvailabilitySet`:

```azurecli
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

Skapa hello första Linux VM. hello följande exempel skapas en virtuell dator med namnet `myVM1`:

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

Skapa hello andra Linux VM. hello följande exempel skapas en virtuell dator med namnet `myVM2`:

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

Använd hello JSON-parsern tooverify att allt som har skapats:

```azurecli
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Exportera din nya miljö tooa mallen tooquickly återskapa nya instanser:

```azurecli
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Detaljerad genomgång
hello beskrivs detaljerade anvisningar som följer vad varje kommando gör när du skapar i din miljö. De här koncepten är användbara när du skapar dina egna anpassade miljöer för utveckling och produktion.

Se till att du har [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) loggas och med hjälp av Resource Manager-läge:

```azurecli
azure config mode arm
```

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar `myResourceGroup`, `mystorageaccount`, och `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Skapa resursgrupper och välj distributionen platser
Azure-resursgrupper är distribution av logiska enheter som innehåller information och metadata tooenable hello logiska konfigurationshantering för resurs-distributioner. hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westeurope` plats:

```azurecli
azure group create --name myResourceGroup --location westeurope
```

Resultat:

```azurecli                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>skapar ett lagringskonto
Du måste storage-konton för VM-diskar och eventuella ytterligare hårddiskar som du vill tooadd. Du kan skapa storage-konton nästan omedelbart när du skapar resursgrupper.

Här använder vi hello `azure storage account create` kommandot Skicka hello platsen för hello konto, hello resursgruppen som styr och hello typ av Lagringsstöd som du vill använda. hello följande exempel skapas ett lagringskonto med namnet `mystorageaccount`:

```azurecli
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Resultat:

```azurecli
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

tooexamine våra resursgrupp med hjälp av hello `azure group show` kommando använder vi hello [jq](https://stedolan.github.io/jq/) verktyget tillsammans med hello `--json` Azure CLI-alternativet. (Du kan använda **jsawk** eller något språk bibliotek som du föredrar tooparse hello JSON.)

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Resultat:

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

tooinvestigate hello storage-konto med hjälp av hello CLI, måste du först tooset hello kontonamn och nycklar. Ersätt hello namnet på hello storage-konto i följande exempel med ett namn som du väljer hello:

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Du kan sedan visa storage-informationen enkelt:

```azurecli
azure storage container list
```

Resultat:

```azurecli
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Skapa ett virtuellt nätverk och undernät
Härnäst ska tooneed toocreate ett virtuellt nätverk som körs i Azure och ett undernät som du kan skapa dina virtuella datorer. hello följande exempel skapas ett virtuellt nätverk med namnet `myVnet` med hello `192.168.0.0/16` adressprefix:

```azurecli
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16
```

Resultat:

```azurecli
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Igen, vi alternativet hello--json av `azure group show` och `jq` toosee hur vi bygger våra resurser. Nu har vi en `storageAccounts` resurs och ett `virtualNetworks` resurs.  

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Resultat:

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Nu ska vi skapa ett undernät i hello `myVnet` virtuellt nätverk i vilka hello virtuella datorer distribueras. Vi använder hello `azure network vnet subnet create` kommandot tillsammans med hello resurser som redan har vi skapat: hello `myResourceGroup` resursgrupp och hello `myVnet` virtuellt nätverk. I följande exempel hello, vi lägga till hello undernät med namnet `mySubnet` med hello undernät adressprefixet `192.168.1.0/24`:

```azurecli
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Resultat:

```azurecli
info:    Executing command network vnet subnet create
+ Looking up hello subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up hello subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Eftersom hello undernät är logiskt i hello virtuella nätverket, ser vi hello undernät information med ett något annorlunda kommando. hello-kommandot som vi använder `azure network vnet show`, men vi fortsätta tooexamine hello JSON-utdata med hjälp av `jq`.

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Resultat:

```json
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address"></a>Skapa en offentlig IP-adress
Nu skapar vi hello offentlig IP-adress (PIP) som vi tilldelar tooyour belastningsutjämnaren. Det gör att du tooconnect tooyour virtuella datorer från hello Internet med hjälp av hello `azure network public-ip create` kommando. Eftersom hello standardadressen är dynamiska, vi skapa en namngiven DNS-post i hello **cloudapp.azure.com** domänen med hjälp av hello `--domain-name-label` alternativet. hello följande exempel skapas en offentlig IP-adress med namnet `myPublicIP` med hello DNS-namnet på `mypublicdns`. Eftersom hello DNS-namnet måste vara unikt, kan du ange egna unikt DNS-namn:

```azurecli
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Resultat:

```azurecli
info:    Executing command network public-ip create
+ Looking up hello public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up hello public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

hello offentliga IP-adressen är också en översta resurs så att du kan se den med `azure group show`.

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Resultat:

```json
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

Du kan undersöka mer resursinformation,, inklusive hello fullständigt kvalificerade domännamnet (FQDN) för hello underdomänen med hjälp av hello fullständig `azure network public-ip show` kommando. hello offentliga IP-adressresurs har allokerats logiskt, men en specifik adress har ännu inte tilldelats. tooobtain en IP-adress ska tooneed en belastningsutjämnare som vi ännu inte har skapats.

```azurecli
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Resultat:

```json
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Skapa en belastningsutjämnare och IP-pooler
När du skapar en belastningsutjämnare kan du toodistribute trafik över flera virtuella datorer. Det ger också redundans tooyour program genom att köra flera virtuella datorer som svarar toouser begäranden i hello händelsen för underhåll eller tunga laster. hello följande exempel skapas en belastningsutjämnare med namnet `myLoadBalancer`:

```azurecli
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Resultat:

```azurecli
info:    Executing command network lb create
+ Looking up hello load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Vår belastningsutjämnare är ganska tomt, så vi skapa vissa IP-adresspooler. Vi vill toocreate två IP-adresspooler för våra belastningsutjämnare, en för hello klientdelen och en för hello serverdel. hello frontend IP-adresspool är synligt offentligt. Det är också hello plats toowhich vi tilldela hello PIP som vi skapade tidigare. Sedan använder vi hello backend-adresspool som en plats för våra VMs tooconnect till. På så sätt kan hello trafiken kan flöda via hello load balancer toohello virtuella datorer.

Först skapar vi vårt frontend IP-adresspool. hello följande exempel skapas en frontend-pool med namnet `myFrontEndPool`:

```azurecli
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool
```

Resultat:

```azurecli
info:    Executing command network lb frontend-ip create
+ Looking up hello load balancer "myLoadBalancer"
+ Looking up hello public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Observera hur vi använde hello `--public-ip-name` växla toopass i hello `myPublicIP` som vi skapade tidigare. Tilldela hello offentliga IP-adress toohello belastningsutjämnare kan du tooreach din virtuella dator över hello Internet.

Nu ska vi skapa våra andra IP-adresspool nu för våra backend-trafik. hello följande exempel skapas en backend-pool med namnet `myBackEndPool`:

```azurecli
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Resultat:

```azurecli
info:    Executing command network lb address-pool create
+ Looking up hello load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Vi kan se hur våra belastningsutjämnaren fungerar genom att titta med `azure network lb show` och undersöka hello JSON-utdata:

```azurecli
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Resultat:

```json
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Skapa belastningsutjämnaren NAT-regler
tooget trafik som passerar genom vår belastningsutjämnaren måste toocreate network address translation (NAT) regler som anger inkommande eller utgående åtgärder. Du kan ange hello protokollet toouse sedan mappa externa portar toointernal portar som du vill. Nu ska vi skapa vissa regler som tillåter SSH via vårt belastningen belastningsutjämnaren tooour virtuella datorer för våra miljö. Vi konfigurerar TCP-portarna 4222 och 4223 toodirect tooTCP-port 22 på vår virtuella datorer (som vi skapa senare). hello följande exempel skapas en regel med namnet `myLoadBalancerRuleSSH1` toomap TCP-port 4222 tooport 22:

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Resultat:

```azurecli
info:    Executing command network lb inbound-nat-rule create
+ Looking up hello load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Upprepa hello proceduren för andra NAT-regel för SSH. hello följande exempel skapas en regel med namnet `myLoadBalancerRuleSSH2` toomap TCP-port 4223 tooport 22:

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Nu ska vi också gå vidare och skapa en NAT-regel för TCP-port 80 för webbtrafik, koppla upp hello regeln upp tooour IP-adresspooler. Om vi koppla samman hello regeln tooan IP-poolen, i stället för att koppla upp dig hello regeln tooour VMs individuellt kan vi lägga till eller ta bort virtuella datorer från hello IP-pool. hello belastningsutjämnaren justeras automatiskt hello flödet av trafik. hello följande exempel skapas en regel med namnet `myLoadBalancerRuleWeb` toomap TCP-port 80 tooport 80:

```azurecli
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Resultat:

```azurecli
info:    Executing command network lb rule create
+ Looking up hello load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Skapa en belastningsutjämnaren, hälsoavsökningen
En avsökning med jämna mellanrum kontroller på hello virtuella datorer som finns bakom våra belastningen belastningsutjämnaren toomake att de fungerar och svarar toorequests som har definierats. Om inte, de tas bort från dirigerad åtgärden tooensure som användare inte toothem. Du kan definiera egna kontroller för hello hälsoavsökningen tillsammans med intervall och timeout-värden. Läs mer om hälsoavsökningar [avsökningar belastningsutjämnaren](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). hello följande exempel skapas en TCP hälsa avsöktes namngivna `myHealthProbe`:

```azurecli
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Resultat:

```azurecli
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up hello load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Vi anges här, ett intervall på 15 sekunder för våra hälsokontroller. Vi kan missa högst fyra avsökningar (en minut) innan hello belastningsutjämnaren anser hello värden fungerar inte längre.

## <a name="verify-hello-load-balancer"></a>Kontrollera hello belastningsutjämnare
Nu görs hello konfiguration av belastningsutjämning. Här är hello steg som du tog:

1. Du har skapat en belastningsutjämnare.
2. Du har skapat en frontend IP-adresspool och tilldelats en offentlig IP-tooit.
3. Du har skapat en backend-IP-adresspool som virtuella datorer kan ansluta till.
4. Du har skapat NAT-regler som tillåter SSH toohello virtuella datorer för hantering, tillsammans med en regel som tillåter TCP-port 80 för våra webbprogrammet.
5. Du har lagt till en hälsa avsökningen tooperiodically Kontrollera hello virtuella datorer. Den här hälsoavsökningen säkerställer att användare inte försöker tooaccess en virtuell dator som inte längre fungerar eller betjänar innehåll.

Nu ska vi se hur din belastningsutjämnare ser ut nu:

```azurecli
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Resultat:

```json
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-toouse-with-hello-linux-vm"></a>Skapa ett NIC-toouse med hello Linux VM
Nätverkskort är tillgängliga via programmering eftersom du kan använda regler tootheir användning. Du kan också ha fler än en. I följande hello `azure network nic create` kommandot du koppla samman hello NIC toohello belastningen backend-IP-pool och associera det med hello NAT-regeln toopermit SSH-trafik.

Ersätt hello `#####-###-###` avsnitt med din egen Azure prenumerations-ID. Din prenumeration ID anges i hello utdata från `jq` när du undersöker hello-resurser som du skapar. Du kan också visa ditt prenumerations-ID med `azure account list`.

hello följande exempel skapas ett nätverkskort med namnet `myNic1`:

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Resultat:

```azurecli
info:    Executing command network nic create
+ Looking up hello subnet "mySubnet"
+ Looking up hello network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Du kan se hello information genom att undersöka hello resursen direkt. Du undersöka hello resursen med hjälp av hello `azure network nic show` kommando:

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Resultat:

```json
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Nu skapar vi hello andra nätverkskortet, koppla upp i tooour backend-IP-poolen igen. Den här gången hello andra NAT-regeln tillåter SSH-trafik. hello följande exempel skapas ett nätverkskort med namnet `myNic2`:

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Skapa en säkerhetsgrupp för nätverk och regler
Nu när vi skapa en säkerhetsgrupp för nätverk och hello inkommande regler som styr åtkomst till toohello NIC. En nätverkssäkerhetsgrupp kan vara tillämpade tooa nätverkskort eller ett undernät. Du kan definiera regler toocontrol hello flödet av trafik till och från dina virtuella datorer. hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet `myNetworkSecurityGroup`:

```azurecli
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Lägg till hello regel för inkommande trafik hello NSG tooallow inkommande anslutningar på port 22 (toosupport SSH). hello följande exempel skapas en regel med namnet `myNetworkSecurityGroupRuleSSH` tooallow TCP på port 22:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Nu ska vi lägga till hello regel för inkommande trafik hello NSG tooallow inkommande anslutningar på port 80 (toosupport webbtrafik). hello följande exempel skapas en regel med namnet `myNetworkSecurityGroupRuleHTTP` tooallow TCP på port 80:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [!NOTE]
> regel för inkommande trafik hello är ett filter för inkommande nätverksanslutningar. I det här exemplet Binder vi hello NSG toohello virtuellt nätverkskort för virtuella datorer, vilket innebär att alla begäran tooport 22 överförs via toohello NIC på vår VM. Denna regel för inkommande trafik som handlar om en nätverksanslutning och inte om en slutpunkt som är det skulle ha varit om i klassiska distributioner. tooopen en port du lämnar hello `--source-port-range` angetts för '\*' (hello standardvärdet) tooaccept inkommande begäranden från **alla** begär port. Det finns vanligtvis dynamiska portar.
>
>

## <a name="bind-toohello-nic"></a>Binda toohello NIC
Binda hello NSG toohello nätverkskort. Vi behöver tooconnect våra nätverkskort med vår nätverkssäkerhetsgruppen. Kör både kommandon toohook in båda våra nätverkskort:

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Skapa en tillgänglighetsuppsättning
Tillgänglighetsuppsättningar hjälp spridning din virtuella dator över feldomäner och uppgradera domäner. Nu ska vi skapa en tillgänglighetsuppsättning för dina virtuella datorer. hello följande exempel skapas en tillgänglighetsuppsättning namngivna `myAvailabilitySet`:

```azurecli
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Feldomäner definiera en gruppering av virtuella datorer som delar en gemensam käll- och strömbrytare. Som standard separeras hello virtuella datorer som har konfigurerats i din tillgänglighetsuppsättning över in toothree feldomäner. hello idé är att maskinvaruproblem i något av dessa feldomäner inte påverkar varje virtuell dator som kör din app. Azure distribuerar automatiskt virtuella datorer över hello feldomäner när de placeras i en tillgänglighetsuppsättning.

Uppgraderingsdomäner ange grupper av virtuella datorer och underliggande fysiska maskinvara som kan startas på hello samtidigt. hello ordning i vilken uppgraderingsdomäner startas om kan inte vara sekventiella under planerat underhåll, men endast en uppgradering på att startas samtidigt. Igen, Azure automatiskt distribuerar din virtuella dator över uppgraderingsdomäner när de placeras i en tillgänglig plats.

Läs mer om [hantera hello tillgängligheten för virtuella datorer](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="create-hello-linux-vms"></a>Skapa hello virtuella Linux-datorer
Du har skapat hello lagring och nätverksresurser toosupport Internet-tillgängliga virtuella datorer. Nu ska vi skapa de virtuella datorer och skydda dem med en SSH-nyckel som inte har ett lösenord. I det här fallet ska vi toocreate en Ubuntu VM baserat på hello senaste LTS. Vi Leta upp avbildningsinformationen med hjälp av `azure vm image list`, enligt beskrivningen i [söker Azure VM-bilder](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Vi valde en bild med hello kommandot `azure vm image list westeurope canonical | grep LTS`. I detta fall kan vi använda `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Hello sista fältet, skickar vi `latest` så att i framtiden hello vi alltid får hello den senaste versionen. (vi använder hello-strängen är `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Nästa steg är bekant tooanyone som redan har skapat ett ssh rsa-offentliga och privata nyckel koppla på Linux- eller Mac med hjälp av **ssh-keygen - t rsa -b 2048**. Om du inte har några certifikat nyckelpar i din `~/.ssh` katalog så kan du skapa dem:

* Automatiskt, genom att använda hello `azure vm create --generate-ssh-keys` alternativet.
* Manuellt med hjälp av [hello instruktioner toocreate dem själv](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Du kan också använda hello `--admin-password` metoden tooauthenticate SSH-anslutningar när hello VM skapas. Den här metoden är vanligtvis mindre säker.

Vi skapar hello VM genom att våra resurser och information tillsammans med hello `azure vm create` kommando:

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

Resultat:

```azurecli
info:    Executing command vm create
+ Looking up hello VM "myVM1"
info:    Verifying hello public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using hello VM Size "Standard_DS1"
info:    hello [OS, Data] Disk or image configuration requires storage account
+ Looking up hello storage account mystorageaccount
+ Looking up hello availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up hello NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in hello NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    hello storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by hello parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Du kan ansluta tooyour VM omedelbart genom att använda din standard SSH-nycklar. Se till att du anger rätt port för hello eftersom vi passera hello belastningsutjämnaren. (För vårt första VM vi ställa in hello NAT-regel tooforward port 4222 tooour VM.)

```bash
ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Resultat:

```bash
hello authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want toocontinue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) toohello list of known hosts.
Welcome tooUbuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Gå vidare och skapa den virtuella datorn med andra i hello samma sätt:

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

Du kan nu använda hello `azure vm show myResourceGroup myVM1` kommandot tooexamine vad du har skapat. Nu är kör du dina Ubuntu virtuella datorer bakom en belastningsutjämnare i Azure som du kan logga in till endast med SSH-nyckel (eftersom lösenord är inaktiverade). Du kan installera nginx eller httpd distribuera en webbapp och se hello trafik flödet genom hello belastningen belastningsutjämnaren tooboth hello virtuella datorer.

```azurecli
azure vm show --resource-group myResourceGroup --name myVM1
```

Resultat:

```azurecli
info:    Executing command vm show
+ Looking up hello VM "TestVM1"
+ Looking up hello NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-hello-environment-as-a-template"></a>Exportera hello miljö som en mall
Nu när du har skapat i den här miljön om du vill att toocreate en ytterligare utvecklingsmiljö med hello samma parametrar eller en produktionsmiljö som matchar det? Hanteraren för filserverresurser använder JSON-mallar som definierar alla hello parametrar för din miljö. Du bygga ut hela miljöer genom att referera till den här JSON-mallen. Du kan [skapa JSON-mallarna manuellt](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller exportera en befintlig miljö toocreate hello JSON mall:

```azurecli
azure group export --name myResourceGroup
```

Det här kommandot skapar hello `myResourceGroup.json` fil i den aktuella arbetskatalogen. När du skapar en miljö med den här mallen kan uppmanas du alla hello resursnamn, inklusive hello namn för hello belastningsutjämnare, nätverkskort eller virtuella datorer. Du kan fylla i dessa namn i mallfilen genom att lägga till hello `-p` eller `--includeParameterDefaultValue` parametern toohello `azure group export` kommando som visades tidigare. Redigera din JSON mallen toospecify hello resursnamn, eller [skapa en fil med parameters.json](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) som anger hello resursnamn.

toocreate en miljö med din mall:

```azurecli
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Du kanske vill tooread [mer om hur toodeploy från mallar](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Läs mer om hur tooincrementally uppdatering miljöer, använda hello parameterfilen och komma åt mallar från en enda lagringsplats.

## <a name="next-steps"></a>Nästa steg
Nu är du redo toobegin arbeta med flera nätverkskomponenter och virtuella datorer. Du kan använda det här exemplet miljö toobuild ut ditt program med hjälp av hello kärnkomponenter introduceras här.
