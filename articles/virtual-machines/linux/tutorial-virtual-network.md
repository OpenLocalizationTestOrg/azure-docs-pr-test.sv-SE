---
title: "aaaAzure virtuella nätverk och virtuella Linux-datorer | Microsoft Docs"
description: "Självstudiekurs – hantera virtuella Azure-nätverk och virtuella Linux-datorer med hello Azure CLI"
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
ms.openlocfilehash: 57e6bd4de16f0e31d53dc67bf50dc5730d43712b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-hello-azure-cli"></a>Hantera virtuella Azure-nätverk och virtuella Linux-datorer med hello Azure CLI

Azure virtual machines använder Azure-nätverk för interna och externa nätverkskommunikation. Den här kursen går igenom hur du distribuerar två virtuella datorer och konfigurera Azure nätverk för dessa virtuella datorer. hello exemplen i den här kursen förutsätter att hello virtuella datorer är värdar för ett webbprogram med en-databas, men ett program inte har distribuerats i hello självstudiekursen. I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Distribuera ett virtuellt nätverk
> * Skapa ett undernät i ett virtuellt nätverk
> * Anslut virtuella datorer tooa undernät
> * Hantera virtuella offentliga IP-adresser
> * Skydda inkommande trafik för internet
> * Skydda Virtuella tooVM trafik


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="vm-networking-overview"></a>VM-nätverk – översikt

Virtuella Azure-nätverk kan säkra nätverksanslutningar mellan virtuella datorer, hello internet och andra Azure-tjänster, till exempel Azure SQL-databas. Virtuella nätverk är uppdelad i logiska segment som kallas subnät. Undernät används toocontrol nätverk flöde, och som en säkerhetsgräns. När du distribuerar en virtuell dator innehåller vanligtvis ett virtuellt nätverksgränssnitt som är bifogade tooa undernät.

## <a name="deploy-virtual-network"></a>Distribuera virtuella nätverk

Ett enda virtuellt nätverk skapas med två undernät för den här kursen. Ett frontend undernät som värd för ett webbprogram och ett backend-undernät som värd för en databasserver.

Innan du kan skapa ett virtuellt nätverk, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myRGNetwork* hello eastus plats.

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a>Skapa det virtuella nätverket

Oss hello [az network vnet skapa](/cli/azure/network/vnet#create) kommandot toocreate ett virtuellt nätverk. I det här exemplet hello nätverk med namnet *mvVnet* och ges en adressprefixet *10.0.0.0/16*. Dessutom skapas ett undernät med namnet *mySubnetFrontEnd* och prefixet *10.0.1.0/24*. Senare i den här kursen är en frontend VM anslutna toothis undernät. 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnetFrontEnd \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a>Skapa ett undernät

Ett nytt undernät läggs toohello virtuellt nätverk med hello [az undernät för virtuellt nätverk skapa](/cli/azure/network/vnet/subnet#create) kommando. I det här exemplet hello undernät med namnet *mySubnetBackEnd* och ges en adressprefixet *10.0.2.0/24*. Det här undernätet används med alla backend-tjänster.

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --address-prefix 10.0.2.0/24
```

Ett nätverk har nu skapats och upp i två undernät, en för frontend-tjänster och en för backend-tjänster. I nästa avsnitt om hello, virtuella datorer skapas och anslutna toothese undernät.

## <a name="understand-public-ip-address"></a>Förstå offentlig IP-adress

En offentlig IP-adress kan toobe Azure-resurser tillgängliga på hello internet. I det här avsnittet av kursen hello skapas en virtuell dator toodemonstrate hur toowork med en offentlig IP-adresser.

### <a name="allocation-method"></a>Allokeringsmetod

En offentlig IP-adress kan fördelas som dynamiska eller statiska. Offentliga IP-adressen tilldelas dynamiskt som standard. Dynamiska IP-adresser släpps när en virtuell dator har frigjorts. Detta medför hello IP-adress toochange under åtgärder som innehåller en VM-flyttningen.

hello allokeringsmetod kan ställas in toostatic, vilket säkerställer att hello IP-adress vara tilldelad tooa VM, även under en frigjord tillstånd. När du använder en statiskt tilldelade IP-adress, kan hello själva IP-adressen inte anges. Istället tilldelas den från en pool med tillgängliga adresser.

### <a name="dynamic-allocation"></a>Dynamisk allokering

När du skapar en virtuell dator med hello [az vm skapa](/cli/azure/vm#create) kommandot hello standard offentliga IP-adress allokeringsmetoden är dynamisk. I följande exempel hello, skapas en virtuell dator med en dynamisk IP-adress. 

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

När du skapar en virtuell dator med hello [az vm skapa](/cli/azure/vm#create) kommandot, inkludera hello `--public-ip-address-allocation static` argumentet tooassign en statisk offentlig IP-adress. Den här åtgärden visas inte i den här kursen, men i nästa avsnitt om hello en dynamiskt tilldelad IP-adress är ändrade tooa statiskt tilldelade adressen. 

### <a name="change-allocation-method"></a>Ändra allokeringsmetod

hello allokeringsmetod för IP-adress kan ändras med hello [az nätverket offentliga ip-uppdatering](/cli/azure/network/public-ip#update) kommando. I det här exemplet hello allokeringsmetod för IP-adress för hello frontend VM ändras toostatic.

Först frigöra hello VM.

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontEndVM
```

Använd hello [az nätverket offentliga ip-uppdatering](/cli/azure/network/public-ip#update) kommandot tooupdate hello allokeringsmetod. I det här fallet hello `--allocation-method` anges för*statiska*.

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myFrontEndIP --allocation-method static
```

Starta hello VM.

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontEndVM --no-wait
```

### <a name="no-public-ip-address"></a>Ingen offentlig IP-adress

Ofta är en virtuell dator måste inte toobe nås över hello internet. toocreate en virtuell dator utan en offentlig IP-adress, Använd hello `--public-ip-address ""` argument med en tom uppsättning med dubbla citattecken. Den här konfigurationen visas senare i den här självstudiekursen

## <a name="secure-network-traffic"></a>Skydda nätverkstrafik

En nätverkssäkerhetsgrupp (NSG) innehåller en lista över säkerhetsregler som tillåter eller nekar nätverket trafik tooresources anslutna tooAzure virtuella nätverk (VNet). NSG: er kan vara associerade toosubnets eller enskilda nätverksgränssnitt. När en NSG är associerad med ett nätverksgränssnitt gäller endast hello associerade VM. När en NSG är associerad tooa undernät, tillämpas hello reglerna tooall resurser anslutna toohello undernät. 

### <a name="network-security-group-rules"></a>Regler för nätverkssäkerhetsgrupper

NSG-regler definiera nätverk portar under vilken trafik tillåts eller nekas. hello-regler kan innehålla käll- och IP-adressintervall så att trafik styrs mellan specifika system eller undernät. NSG-regler kan även innehålla en prioritet (mellan 1 – och 4096). Reglerna utvärderas hello efter prioritet. En regel med en prioritet på 100 utvärderas innan en regel med prioritet 200.

Alla NSG:er har en uppsättning standardregler. hello standardreglerna kan inte tas bort, men eftersom de har tilldelats lägst prioritet för hello, de kan åsidosättas med hello regler som du skapar.

- **Virtuellt nätverk** - trafik med ursprung och slutar med ett virtuellt nätverk tillåts både i inkommande och utgående riktningar.
- **Internet** - utgående trafik tillåts, men inkommande trafik blockeras.
- **Belastningsutjämnaren** -Tillåt Azure belastningen belastningsutjämnaren tooprobe hello hälsotillståndet för dina virtuella datorer och rollinstanser. Om du inte använder en belastningsutjämnad uppsättning, kan du åsidosätta den här regeln.

### <a name="create-network-security-groups"></a>Skapa säkerhetsgrupper för nätverk

En nätverkssäkerhetsgrupp kan skapas på hello samma tid som en virtuell dator med hjälp av hello [az vm skapa](/cli/azure/vm#create) kommando. När du gör det hello NSG är associerad med hello nätverksgränssnitt för virtuella datorer och en NSG-regel är skapas automatiskt tooallow trafik på port *22* från andra källor. Tidigare i den här självstudien hello hello frontend NSG har skapats automatiskt med frontend VM. En regel för NSG har också automatiskt skapat för port 22. 

I vissa fall kan det vara bra toopre-skapa en NSG till exempel när standardreglerna för SSH inte skapas eller när hello NSG ska vara anslutna tooa undernät. 

Använd hello [az nätverket nsg skapa](/cli/azure/network/nsg#create) kommandot toocreate en nätverkssäkerhetsgrupp.

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myNSGBackEnd
```

I stället för att associera hello NSG tooa nätverksgränssnitt, är den associerad med ett undernät. I den här konfigurationen ärver någon virtuell dator som är bifogade toohello undernät hello NSG-regler.

Uppdatera hello befintligt undernät med namnet *mySubnetBackEnd* med hello ny NSG.

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --network-security-group myNSGBackEnd
```

Nu skapa en virtuell dator som är bifogade toohello *mySubnetBackEnd*. Observera att hello `--nsg` argumentet har värdet tomt dubbla citattecken. En NSG behöver inte toobe som skapats med hello VM. hello VM är anslutna toohello backend-undernät, som är skyddat med hello förskapade backend-NSG. Den här NSG gäller toohello VM. Observera också här den hello `--public-ip-address` argumentet har värdet tomt dubbla citattecken. Den här konfigurationen skapar en virtuell dator utan en offentlig IP-adress. 

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

När hello frontend VM skapades skapades en NSG regeln tooallow inkommande trafik på port 22. Den här regeln kan SSH-anslutningar toohello VM. I det här exemplet trafik även ska tillåtas på port *80*. Den här konfigurationen kan en web application toobe åt på hello VM.

Använd hello [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) kommandot toocreate en regel för port *80*.

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

hello frontend virtuella datorn är nu endast tillgänglig på port *22* och port *80*. All annan inkommande trafik blockeras på hello nätverkssäkerhetsgruppen. Det kan vara användbara toovisualize hello NSG konfigurationer. Returnerar hello NSG regelkonfigurationen med hello [az nätverket regellistan](/cli/azure/network/nsg/rule#list) kommando. 

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

### <a name="secure-vm-toovm-traffic"></a>Skydda Virtuella tooVM trafik

Regler för nätverkssäkerhetsgrupper kan också använda mellan virtuella datorer. I det här exemplet hello hello frontend VM måste toocommunicate med backend-VM på port *22* och *3306*. Den här konfigurationen tillåter SSH-anslutningar från hello frontend VM och även att ett program på hello frontend VM toocommunicate med en backend-MySQL-databas. All annan trafik ska blockeras mellan hello frontend- och virtuella datorer.

Använd hello [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) kommandot toocreate en regel för port 22. Observera att hello `--source-address-prefix` argumentet anger ett värde för *10.0.1.0/24*. Den här konfigurationen säkerställer att endast trafik från hello frontend undernät tillåts via hello NSG.

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

Slutligen eftersom NSG: er har en standard regel all trafik mellan virtuella datorer i hello samma virtuella nätverk, en regel kan skapas för hello backend-NSG: er tooblock all trafik. Observera att hello `--priority` ges värdet *300*, vilket är lägre både hello NSG och MySQL regler. Den här konfigurationen garanterar att SSH och MySQL trafiken fortfarande tillåts hello NSG.

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

hello backend-VM är nu endast tillgänglig på port *22* och port *3306* från hello frontend undernät. All annan inkommande trafik blockeras på hello nätverkssäkerhetsgruppen. Det kan vara användbara toovisualize hello NSG konfigurationer. Returnerar hello NSG regelkonfigurationen med hello [az nätverket regellistan](/cli/azure/network/nsg/rule#list) kommando. 

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

I den här självstudiekursen skapas och skyddas av Azure-nätverk som relaterade toovirtual datorer. Du har lärt dig att:

> [!div class="checklist"]
> * Distribuera ett virtuellt nätverk
> * Skapa ett undernät i ett virtuellt nätverk
> * Anslut virtuella datorer tooa undernät
> * Hantera virtuella offentliga IP-adresser
> * Skydda inkommande trafik för internet
> * Skydda Virtuella tooVM trafik

Avancera toohello nästa självstudiekurs toolearn om hur du skyddar data på virtuella datorer med Azure backup. 

> [!div class="nextstepaction"]
> [Säkerhetskopiera virtuella Linux-datorer i Azure](./tutorial-backup-vms.md)
