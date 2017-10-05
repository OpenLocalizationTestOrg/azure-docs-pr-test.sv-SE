---
title: "Virtuella Azure-nätverk och virtuella Linux-datorer | Microsoft Docs"
description: "Självstudiekurs – hantera virtuella Azure-nätverk och virtuella Linux-datorer med Azure CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2366905b8160675f77cbc41ba97540af70be8c01
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-the-azure-cli"></a>Hantera virtuella Azure-nätverk och virtuella Linux-datorer med Azure CLI

Azure virtual machines använder Azure-nätverk för interna och externa nätverkskommunikation. Den här kursen går igenom hur du distribuerar två virtuella datorer och konfigurera Azure nätverk för dessa virtuella datorer. Exemplen i den här kursen förutsätter att de virtuella datorerna är värd för ett webbprogram med en-databas, men ett program inte har distribuerats i självstudiekursen. I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Distribuera ett virtuellt nätverk
> * Skapa ett undernät i ett virtuellt nätverk
> * Anslut virtuella datorer till ett undernät
> * Hantera virtuella offentliga IP-adresser
> * Skydda inkommande trafik för internet
> * Säker VM VM-trafik


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt kursen krävs att du använder Azure CLI version 2.0.4 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="vm-networking-overview"></a>VM-nätverk – översikt

Virtuella Azure-nätverk kan säkra nätverksanslutningar mellan virtuella datorer, internet och andra Azure-tjänster, till exempel Azure SQL-databas. Virtuella nätverk är uppdelad i logiska segment som kallas subnät. Undernät används för flödeskontroll för nätverk och som en säkerhetsgräns. När du distribuerar en virtuell dator innehåller vanligtvis ett virtuellt nätverksgränssnitt som är ansluten till ett undernät.

## <a name="deploy-virtual-network"></a>Distribuera virtuella nätverk

Ett enda virtuellt nätverk skapas med två undernät för den här kursen. Ett frontend undernät som värd för ett webbprogram och ett backend-undernät som värd för en databasserver.

Innan du kan skapa ett virtuellt nätverk, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). I följande exempel skapas en resursgrupp med namnet *myRGNetwork* eastus plats.

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a>Skapa det virtuella nätverket

Oss den [az network vnet skapa](/cli/azure/network/vnet#create) kommando för att skapa ett virtuellt nätverk. I det här exemplet är nätverket heter *mvVnet* och ges en adressprefixet *10.0.0.0/16*. Dessutom skapas ett undernät med namnet *mySubnetFrontEnd* och prefixet *10.0.1.0/24*. Senare i den här självstudiekursen är frontend VM anslutet till det här undernätet. 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnetFrontEnd \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a>Skapa ett undernät

Ett nytt undernät har lagts till i det virtuella nätverket med hjälp av den [az undernät för virtuellt nätverk skapa](/cli/azure/network/vnet/subnet#create) kommando. I det här exemplet undernätet med namnet *mySubnetBackEnd* och ges en adressprefixet *10.0.2.0/24*. Det här undernätet används med alla backend-tjänster.

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --address-prefix 10.0.2.0/24
```

Ett nätverk har nu skapats och upp i två undernät, en för frontend-tjänster och en för backend-tjänster. I nästa avsnitt, virtuella datorer skapas och anslutna till dessa undernät.

## <a name="understand-public-ip-address"></a>Förstå offentlig IP-adress

En offentlig IP-adress kan Azure-resurser ska vara tillgänglig på internet. I det här avsnittet av kursen skapas en virtuell dator för att demonstrera hur du arbetar med offentliga IP-adresser.

### <a name="allocation-method"></a>Allokeringsmetod

En offentlig IP-adress kan fördelas som dynamiska eller statiska. Offentliga IP-adressen tilldelas dynamiskt som standard. Dynamiska IP-adresser släpps när en virtuell dator har frigjorts. Detta medför IP-adressen ändras under åtgärder som innehåller en VM-flyttningen.

Allokeringsmetoden kan anges till statisk som säkerställer att IP adress fortsätter vara tilldelade till en virtuell dator, även under en frigjord tillstånd. Den IP-adressen kan inte anges när du använder en statiskt tilldelade IP-adress. Istället tilldelas den från en pool med tillgängliga adresser.

### <a name="dynamic-allocation"></a>Dynamisk allokering

När du skapar en virtuell dator med den [az vm skapa](/cli/azure/vm#create) kommandot offentliga IP-adress allokering standardmetoden är dynamisk. I följande exempel skapas en virtuell dator med en dynamisk IP-adress. 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myFrontEndVM \
  --vnet-name myVnet \
  --subnet mySubnetFrontEnd \
  --nsg myNSGFrontEnd \
  --public-ip-address myFrontEndIP \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="static-allocation"></a>Statisk tilldelning

När du skapar en virtuell dator med hjälp av den [az vm skapa](/cli/azure/vm#create) kommandot, innehåller den `--public-ip-address-allocation static` argumentet att tilldela en statisk offentlig IP-adress. Den här åtgärden visas inte i den här självstudiekursen, men i nästa avsnitt ändras dynamiskt allokerade IP-adress till ett statiskt allokerade adressen. 

### <a name="change-allocation-method"></a>Ändra allokeringsmetod

Allokeringsmetod för IP-adress kan ändras med den [az nätverket offentliga ip-uppdatering](/cli/azure/network/public-ip#update) kommando. I det här exemplet ändras allokeringsmetoden av frontend VM IP-adress till statisk.

Ta bort den virtuella datorn först.

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontEndVM
```

Använd den [az nätverket offentliga ip-uppdatering](/cli/azure/network/public-ip#update) kommando för att uppdatera allokeringsmetoden. I det här fallet den `--allocation-method` anges till *statiska*.

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myFrontEndIP --allocation-method static
```

Starta den virtuella datorn.

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontEndVM --no-wait
```

### <a name="no-public-ip-address"></a>Ingen offentlig IP-adress

Ofta behöver inte en virtuell dator som är tillgängliga via internet. Om du vill skapa en virtuell dator utan en offentlig IP-adress, använder den `--public-ip-address ""` argument med en tom uppsättning med dubbla citattecken. Den här konfigurationen visas senare i den här självstudiekursen

## <a name="secure-network-traffic"></a>Skydda nätverkstrafik

En nätverkssäkerhetsgrupp (NSG) innehåller en lista över säkerhetsregler som tillåter eller nekar nätverkstrafik till resurser som är anslutna till virtuella Azure-nätverk (VNet). NSG: er kan vara kopplad till undernät eller individuella nätverksgränssnitt. När en NSG är associerad med ett nätverksgränssnitt gäller bara den associera virtuella datorn. När en nätverkssäkerhetsgrupp är kopplad till ett undernät gäller reglerna för alla resurser som är anslutna till undernätet. 

### <a name="network-security-group-rules"></a>Regler för nätverkssäkerhetsgrupper

NSG-regler definiera nätverk portar under vilken trafik tillåts eller nekas. Regler kan innehålla käll- och IP-adressintervall så att trafik styrs mellan specifika system eller undernät. NSG-regler kan även innehålla en prioritet (mellan 1 – och 4096). Reglerna utvärderas prioritsordning. En regel med en prioritet på 100 utvärderas innan en regel med prioritet 200.

Alla NSG:er har en uppsättning standardregler. Standardreglerna kan inte tas bort, men eftersom de tilldelas lägst prioritet så kan de överskridas av de reglerna du själv skapar.

- **Virtuellt nätverk** - trafik med ursprung och slutar med ett virtuellt nätverk tillåts både i inkommande och utgående riktningar.
- **Internet** - utgående trafik tillåts, men inkommande trafik blockeras.
- **Belastningsutjämnaren** -Tillåt Azure belastningsutjämnare avsökning hälsotillståndet för dina virtuella datorer och rollinstanser. Om du inte använder en belastningsutjämnad uppsättning, kan du åsidosätta den här regeln.

### <a name="create-network-security-groups"></a>Skapa säkerhetsgrupper för nätverk

En nätverkssäkerhetsgrupp kan skapas samtidigt som en virtuell dator med hjälp av den [az vm skapa](/cli/azure/vm#create) kommando. När du gör det NSG: N är kopplad till nätverksgränssnitt för virtuella datorer och en NSG-regel är skapade för att tillåta trafik på port automatiskt *22* från andra källor. Tidigare i den här självstudien har frontend NSG: N skapats automatiskt med frontend VM. En regel för NSG har också automatiskt skapat för port 22. 

I vissa fall kan vara det bra att skapa en NSG till exempel när standardreglerna för SSH inte skapas eller när NSG: N ska kopplas till ett undernät. 

Använd den [az nätverket nsg skapa](/cli/azure/network/nsg#create) kommando för att skapa en nätverkssäkerhetsgrupp.

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myNSGBackEnd
```

I stället för att koppla NSG till ett nätverksgränssnitt, är den associerad med ett undernät. I den här konfigurationen ärver någon virtuell dator som är kopplad till undernätet NSG-regler.

Uppdatera befintliga undernätet med namnet *mySubnetBackEnd* med nya NSG: N.

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --network-security-group myNSGBackEnd
```

Nu skapa en virtuell dator som är ansluten till den *mySubnetBackEnd*. Observera att den `--nsg` argumentet har värdet tomt dubbla citattecken. En NSG behöver inte skapas med den virtuella datorn. Den virtuella datorn är ansluten till backend-undernät, som är skyddat med backend-förskapade NSG: N. Den här NSG gäller för den virtuella datorn. Observera också här som den `--public-ip-address` argumentet har värdet tomt dubbla citattecken. Den här konfigurationen skapar en virtuell dator utan en offentlig IP-adress. 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myBackEndVM \
  --vnet-name myVnet \
  --subnet mySubnetBackEnd \
  --public-ip-address "" \
  --nsg "" \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="secure-incoming-traffic"></a>Skydda inkommande trafik

När frontend VM skapades, skapades en NSG-regel för att tillåta inkommande trafik på port 22. Den här regeln kan SSH-anslutningar till den virtuella datorn. I det här exemplet trafik även ska tillåtas på port *80*. Den här konfigurationen kan ett program som kan nås på den virtuella datorn.

Använd den [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) kommando för att skapa en regel för port *80*.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGFrontEnd \
  --name http \
  --access allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80
```

Frontend VM är nu endast tillgänglig på port *22* och port *80*. All annan inkommande trafik blockeras på nätverkssäkerhetsgruppen. Det kan vara bra att visualisera regelkonfigurationer NSG. Returnera NSG regelkonfigurationen med den [az nätverket regellistan](/cli/azure/network/nsg/rule#list) kommando. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGFrontEnd --output table
```

Resultat:

```azurecli-interactive 
Access    DestinationAddressPrefix      DestinationPortRange  Direction    Name                 Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -----------------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                                               22  Inbound      default-allow-ssh        1000  Tcp         Succeeded            myRGNetwork      *                      *
Allow     *                                               80  Inbound      http                      200  Tcp         Succeeded            myRGNetwork      *                      *
```

### <a name="secure-vm-to-vm-traffic"></a>Säker VM VM-trafik

Regler för nätverkssäkerhetsgrupper kan också använda mellan virtuella datorer. I det här exemplet frontend VM behöver kommunicera med backend-VM på port *22* och *3306*. Den här konfigurationen tillåter SSH-anslutningar från frontend VM och även att ett program på den frontend virtuella datorn kan kommunicera med en backend-MySQL-databas. All annan trafik ska blockeras mellan frontend- och virtuella datorer.

Använd den [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) kommando för att skapa en regel för port 22. Observera att den `--source-address-prefix` argumentet anger ett värde för *10.0.1.0/24*. Den här konfigurationen säkerställer att endast trafik från undernätet som frontend tillåts via NSG: N.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name SSH \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "22"
```

Lägg till regel för MySQL-trafik på port 3306.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name MySQL \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "3306"
```

Slutligen: eftersom NSG: er har en standardregel som tillåter all trafik mellan virtuella datorer i samma virtuella nätverk, en regel skapas för backend-NSG: er att blockera all trafik. Observera som den `--priority` ges värdet *300*, vilket är lägre som både MySQL och NSG-regler. Den här konfigurationen garanterar att SSH och MySQL trafiken fortfarande tillåts NSG: N.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name denyAll \
  --access Deny \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "*"
```

Backend-VM är nu endast tillgänglig på port *22* och port *3306* från frontend undernät. All annan inkommande trafik blockeras på nätverkssäkerhetsgruppen. Det kan vara bra att visualisera regelkonfigurationer NSG. Returnera NSG regelkonfigurationen med den [az nätverket regellistan](/cli/azure/network/nsg/rule#list) kommando. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGBackEnd --output table
```

Resultat:

```azurecli-interactive 
Access    DestinationAddressPrefix    DestinationPortRange    Direction    Name       Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                           22                      Inbound      SSH             100  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Allow     *                           3306                    Inbound      MySQL           200  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Deny      *                           *                       Inbound      denyAll         300  Tcp         Succeeded            myRGNetwork      *                      *
```

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen skapas och skyddas av Azure-nätverk som är relaterade till virtuella datorer. Du har lärt dig hur till:

> [!div class="checklist"]
> * Distribuera ett virtuellt nätverk
> * Skapa ett undernät i ett virtuellt nätverk
> * Anslut virtuella datorer till ett undernät
> * Hantera virtuella offentliga IP-adresser
> * Skydda inkommande trafik för internet
> * Säker VM VM-trafik

Gå vidare till nästa kurs att lära dig att skydda data på virtuella datorer med Azure backup. 

> [!div class="nextstepaction"]
> [Säkerhetskopiera virtuella Linux-datorer i Azure](./tutorial-backup-vms.md)
