---
title: aaaHow tooload balansera Linux virtuella datorer i Azure | Microsoft Docs
description: "Lär dig hur toouse hello Azure läsa in belastningsutjämning toocreate ett program med hög tillgänglighet och säkert över tre virtuella Linux-datorer"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: f01752c3caec3489ee13e63000775769f3236e11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooload-balance-linux-virtual-machines-in-azure-toocreate-a-highly-available-application"></a>Hur balansera tooload Linux-datorer i Azure toocreate ett program med hög tillgänglighet
Belastningsutjämning ger högre tillgänglighet genom att sprida inkommande begäranden över flera virtuella datorer. I kursen får information du om hello olika komponenterna i hello Azure belastningsutjämnare som distribuerar trafik och ger hög tillgänglighet. Lär dig att:

> [!div class="checklist"]
> * Skapa en Azure belastningsutjämnare
> * Skapa en belastningsutjämnaren, hälsoavsökningen
> * Skapa regler för nätverkstrafik för belastningsutjämnare
> * Använda molntjänster init toocreate en grundläggande Node.js-app
> * Skapa virtuella datorer och bifoga tooa belastningsutjämnare
> * Visa en belastningsutjämnare i praktiken
> * Lägga till och ta bort virtuella datorer från en belastningsutjämnare


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="azure-load-balancer-overview"></a>Översikt över Azure belastningen belastningsutjämnare
En Azure belastningsutjämnare är en belastningsutjämnare för Layer-4 (TCP, UDP) som ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfri virtuella datorer. En belastningsutjämnaren, hälsoavsökningen övervakar en viss port på varje virtuell dator och distribuerar endast trafik tooan operativa VM.

Du kan definiera en frontend IP-konfiguration som innehåller en eller flera offentliga IP-adresser. Den här frontend IP-konfiguration kan din load balancer och program toobe tillgänglig via hello Internet. 

Virtuella datorer ansluta tooa belastningsutjämning med hjälp av deras virtuella nätverksgränssnittskortet (NIC). toodistribute trafik toohello virtuella datorer, en backend-adresspool innehåller hello IP-adresser för hello virtuella (NIC) anslutna toohello belastningsutjämnare.

toocontrol hello trafikflödet, definiera regler för inläsning av belastningsutjämning för specifika portar och protokoll som mappar tooyour virtuella datorer.

Om du har följt hello tidigare självstudier för[skapa en virtuella datorns skaluppsättning](tutorial-create-vmss.md), belastningsutjämning skapades för dig. Alla dessa komponenter konfigurerades för dig som en del av hello skaluppsättning.


## <a name="create-azure-load-balancer"></a>Skapa Azure belastningsutjämnare
Det här avsnittet beskrivs hur du kan skapa och konfigurera varje komponent av hello belastningsutjämnare. Innan du kan skapa din belastningsutjämnare, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroupLoadBalancer* i hello *eastus* plats:

```azurecli-interactive 
az group create --name myResourceGroupLoadBalancer --location eastus
```

### <a name="create-a-public-ip-address"></a>Skapa en offentlig IP-adress
tooaccess din app på hello Internet, måste en offentlig IP-adress för hello belastningsutjämnaren. Skapa en offentlig IP-adress med [az nätverket offentliga IP-skapa](/cli/azure/network/public-ip#create). hello följande exempel skapas en offentlig IP-adress med namnet *myPublicIP* i hello *myResourceGroupLoadBalancer* resursgrupp:

```azurecli-interactive 
az network public-ip create \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a>Skapa en belastningsutjämnare
Skapa en belastningsutjämnare med [az nätverket lb skapa](/cli/azure/network/lb#create). hello följande exempel skapas en belastningsutjämnare med namnet *myLoadBalancer* och tilldelar hello *myPublicIP* toohello frontend IP-adresskonfiguration:

```azurecli-interactive 
az network lb create \
    --resource-group myResourceGroupLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a>Skapa en hälsoavsökningen
tooallow hello belastningen belastningsutjämnaren toomonitor hello statusen för din app, använda en hälsoavsökningen. Hej hälsoavsökningen dynamiskt lägger till eller tar bort virtuella datorer från hello belastningen belastningsutjämnaren rotation baserat på deras svar toohealth kontroller. En virtuell dator tas bort från hello belastningsutjämnaren belastningsdistribution efter två på varandra följande fel intervaller 15 sekunder som standard. Du skapar en hälsoavsökningen baserat på ett protokoll eller ett visst hälsotillstånd sidan för kontroll för din app. 

hello följande exempel skapar en TCP-avsökning. Du kan också skapa anpassade HTTP-avsökningar mer detaljerade hälsokontroller. När du använder en anpassad HTTP-avsökningen, måste du skapa hello hälsa sidan kontroll som *healthcheck.js*. hello avsökning måste returnera ett **HTTP 200 OK** svar för hello belastningen belastningsutjämnaren tookeep hello värd i rotation.

toocreate en TCP-hälsoavsökningen du använder [az nätverket lb avsökningen skapa](/cli/azure/network/lb/probe#create). hello följande exempel skapas en hälsoavsökningen med namnet *myHealthProbe*:

```azurecli-interactive 
az network lb probe create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a>Skapa en regel för belastningsutjämnare
En regel för belastningsutjämnare är används toodefine hur trafiken är distribuerade toohello virtuella datorer. Du kan definiera hello frontend IP-konfiguration för hello inkommande trafik och hello backend-IP-adresspool tooreceive hello trafik, tillsammans med hello krävs käll- och målport. toomake att endast felfri virtuella datorer ta emot trafik, du också definiera hello hälsa avsökningen toouse.

Skapa en regel för belastningsutjämnare med [az nätverket lb regeln skapa](/cli/azure/network/lb/rule#create). hello följande exempel skapas en regel med namnet *myLoadBalancerRule*, använder hello *myHealthProbe* hälsoavsökningen och saldon trafik på port *80*:

```azurecli-interactive 
az network lb rule create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myLoadBalancerRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```


## <a name="configure-virtual-network"></a>Konfigurera ett virtuellt nätverk
Innan du distribuerar vissa virtuella datorer och testa din belastningsutjämnare, skapa hello stöder nätverksresurser på virtuella datorer. Mer information om virtuella nätverk finns hello [hantera virtuella Azure-nätverk](tutorial-virtual-network.md) kursen.

### <a name="create-network-resources"></a>Skapa nätverksresurser
Skapa ett virtuellt nätverk med [az network vnet skapa](/cli/azure/network/vnet#create). hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* med ett undernät med namnet *mySubnet*:

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

tooadd en nätverkssäkerhetsgrupp som du använder [az nätverket nsg skapa](/cli/azure/network/nsg#create). hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup*:

```azurecli-interactive 
az network nsg create \
    --resource-group myResourceGroupLoadBalancer \
    --name myNetworkSecurityGroup
```

Skapa en grupp nätverkssäkerhetsregeln med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create). hello följande exempel skapar en grupp nätverkssäkerhetsregeln med namnet *myNetworkSecurityGroupRule*:

```azurecli-interactive 
az network nsg rule create \
    --resource-group myResourceGroupLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

Virtuella nätverkskort skapas med [az nätverket nic skapa](/cli/azure/network/nic#create). hello skapas följande exempel tre virtuella nätverkskort. (Ett virtuellt nätverkskort för varje virtuell dator som du skapar för din app i hello följande steg). Du kan skapa ytterligare virtuella nätverkskort och virtuella datorer när som helst och lägga till dem toohello belastningsutjämnare:

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupLoadBalancer \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-virtual-machines"></a>Skapa virtuella datorer

### <a name="create-cloud-init-config"></a>Skapa moln init config
I en tidigare självstudiekurs om [hur toocustomize en Linux-dator vid den första starten](tutorial-automate-vm-deployment.md), du lärt dig hur tooautomate VM anpassning med molnet initiering. Du kan använda hello samma molnet init configuration file tooinstall NGINX och kör en enkel Hello World Node.js-app.

Skapa en fil med namnet i din aktuella shell *moln init.txt* och klistra in hello följande konfiguration. Till exempel skapa hello-filen i hello molnet Shell inte på den lokala datorn. Ange `sensible-editor cloud-init.txt` toocreate hello filen och visas i listan över tillgängliga redigerare. Kontrollera att filen hello hela molnet initiering kopierats korrekt, särskilt hello första raden:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-virtual-machines"></a>Skapa virtuella datorer
tooimprove hello hög tillgänglighet för din app, placera dina virtuella datorer i en tillgänglighetsuppsättning. Mer information om tillgänglighetsuppsättningar finns hello tidigare [hur toocreate högtillgängliga virtuella datorer](tutorial-availability-sets.md) kursen.

Skapa en tillgänglighetsuppsättning med [az vm tillgänglighetsuppsättning skapa](/cli/azure/vm/availability-set#create). hello följande exempel skapas en tillgänglighetsuppsättning namngivna *myAvailabilitySet*:

```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupLoadBalancer \
    --name myAvailabilitySet
```

Nu kan du skapa hello virtuella datorer med [az vm skapa](/cli/azure/vm#create). hello följande exempel skapas tre virtuella datorer och genererar SSH-nycklar, om de inte redan finns:

```bash
for i in `seq 1 3`; do
    az vm create \
        --resource-group myResourceGroupLoadBalancer \
        --name myVM$i \
        --availability-set myAvailabilitySet \
        --nics myNic$i \
        --image UbuntuLTS \
        --admin-username azureuser \
        --generate-ssh-keys \
        --custom-data cloud-init.txt \
        --no-wait
done
```

Det finns bakgrundsaktiviteter för att fortsätta toorun när hello Azure CLI returnerar toohello prompt. Hej `--no-wait` parametern inte väntar för alla hello uppgifter toocomplete. Det kan vara en annan några minuter innan du kan komma åt hello app. hello identifierar belastningsutjämnaren, hälsoavsökningen automatiskt när hello appen körs på varje virtuell dator. När hello appen körs startar hello-regel för belastningsutjämnare toodistribute trafik.


## <a name="test-load-balancer"></a>Testa belastningsutjämnare
Hämta hello offentliga IP-adressen för din belastningsutjämnare med [az nätverket offentliga ip-visa](/cli/azure/network/public-ip#show). hello följande exempel hämtar hello IP-adress för *myPublicIP* skapade tidigare:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

Du kan sedan ange hello offentliga IP-adressen i tooa webbläsare. Kom ihåg - det tar några minuter hello hello VMs toobe redo innan hello belastningsutjämnaren startar toodistribute trafik toothem. hello appen visas, inklusive hello värdnamnet för hello VM som hello belastningsutjämnaren distribuerade trafik tooas i följande exempel hello:

![Node.js-app som körs](./media/tutorial-load-balancer/running-nodejs-app.png)

toosee hello belastningsutjämnare distribuerar trafik över alla tre virtuella datorer som kör appen, du kan framtvinga uppdatera webbläsaren.


## <a name="add-and-remove-vms"></a>Lägga till och ta bort virtuella datorer
Du kan behöva tooperform Underhåll på hello virtuella datorer som kör appen, till exempel installera uppdateringar av OS. toodeal med ökat tooyour app som du kan behöva tooadd ytterligare virtuella datorer. Det här avsnittet beskrivs hur du tooremove eller lägga till en virtuell dator från hello belastningsutjämnaren.

### <a name="remove-a-vm-from-hello-load-balancer"></a>Ta bort en virtuell dator från hello belastningsutjämnare
Du kan ta bort en virtuell dator från hello backend-adresspool med [az nic ip-config-nätverksadresspool ta bort](/cli/azure/network/nic/ip-config/address-pool#remove). följande exempel tar bort hello hello virtuella nätverkskortet för **myVM2** från *myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool remove \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

toosee hello belastningsutjämnare distribuerar trafik över hello återstående två virtuella datorer som kör appen du kan framtvinga uppdatera webbläsaren. Du kan nu utföra underhåll på hello VM, till exempel installera uppdateringar av operativsystem eller utföra en VM-omstart.

### <a name="add-a-vm-toohello-load-balancer"></a>Lägga till en belastningsutjämnare för VM-toohello
När du utför underhåll på VM, eller om du behöver tooexpand kapacitet kan du lägga till en VM toohello backend-adresspool med [az nic ip-config-nätverksadresspool lägga till](/cli/azure/network/nic/ip-config/address-pool#add). hello följande exempel läggs hello virtuella nätverkskortet för **myVM2** för*myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool add \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```


## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen, skapas en belastningsutjämnare och kopplade tooit för virtuella datorer. Du har lärt dig att:

> [!div class="checklist"]
> * Skapa en Azure belastningsutjämnare
> * Skapa en belastningsutjämnaren, hälsoavsökningen
> * Skapa regler för nätverkstrafik för belastningsutjämnare
> * Använda molntjänster init toocreate en grundläggande Node.js-app
> * Skapa virtuella datorer och bifoga tooa belastningsutjämnare
> * Visa en belastningsutjämnare i praktiken
> * Lägga till och ta bort virtuella datorer från en belastningsutjämnare

Avancera toohello nästa självstudiekurs toolearn mer om virtuella Azure-nätverkskomponenter.

> [!div class="nextstepaction"]
> [Hantera virtuella datorer och virtuella nätverk](tutorial-virtual-network.md)
