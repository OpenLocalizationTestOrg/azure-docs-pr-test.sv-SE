---
title: "Ansluta virtuella nätverk tooanother VNet: Azure CLI | Microsoft Docs"
description: "Den här artikeln visar hur du ansluter virtuella nätverk tillsammans med hjälp av Azure Resource Manager och Azure CLI."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0683c664-9c03-40a4-b198-a6529bf1ce8b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: 70113914bcae03c80f9ad133ff081d1cf37fc309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-azure-cli"></a>Konfigurera en VPN-gatewayanslutning mellan virtuella nätverk med hjälp av Azure CLI

Den här artikeln beskrivs hur du toocreate en VPN-gateway-anslutningen mellan virtuella nätverk. hello virtuella nätverk kan vara i samma eller olika regioner hello och hello från samma eller olika prenumerationer. När du ansluter Vnet från olika prenumerationer, hello prenumerationer inte behöver toobe som är associerade med hello samma Active Directory-klient. 

hello stegen i den här artikeln gäller toohello Resource Manager-modellen och använder Azure CLI. Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure Portal (klassisk)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Ansluta olika distributionsmodeller – Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Ansluta olika distributionsmodeller – PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

Ansluta ett virtuellt nätverk tooanother virtuellt nätverk (VNet-till-VNet) är liknande tooconnecting ett VNet tooan lokal plats. Båda typerna av anslutningen använder en VPN-gateway tooprovide en säker tunnel med IPsec/IKE. Om ditt Vnet i hello samma region som du kanske vill tooconsider ansluter dem med hjälp av VNet-Peering. Ingen VPN-gateway används för VNet-peering. Mer information finns i [VNet peering (Vnet-peering)](../virtual-network/virtual-network-peering-overview.md).

VNet-till-VNet-kommunikation kan kombineras med konfigurationer för flera platser. På så sätt kan du etablera nätverkstopologier som kombinerar korsanslutningar med mellan virtuell nätverksanslutning som visas i följande diagram hello:

![Om anslutningar](./media/vpn-gateway-howto-vnet-vnet-cli/aboutconnections.png)

### <a name="why"></a>Varför ska man ansluta virtuella nätverk?

Du kanske vill tooconnect virtuella nätverk för hello följande orsaker:

* **Geografisk redundans i flera regioner och geografisk närvaro**

  * Du kan ange din egna geografiska replikering eller synkronisering med en säker anslutning, utan att passera några Internet-slutpunkter.
  * Med Azure Traffic Manager och Load Balancer kan du konfigurera arbetsbelastning med hög tillgänglighet och geografisk redundans över flera Azure-regioner. Ett viktigt exempel är tooset in SQL Always On med Tillgänglighetsgrupper sprida över flera Azure-regioner.
* **Regionala flernivåprogram med isolering eller administrativa gränser**

  * Hej i samma region, du kan konfigurera flera nivåer program med flera virtuella nätverk som kopplar samman förfallodatum tooisolation eller administrativa krav.

Mer information om VNet-till-VNet-anslutningar finns hello [VNet-till-VNet vanliga frågor och svar](#faq) hello slutet av den här artikeln.

### <a name="which-set-of-steps-should-i-use"></a>Vilka steg ska jag använda?

I den här artikeln beskrivs två uppsättningar med steg. En uppsättning steg för [Vnet som finns i hello samma prenumeration](#samesub), och en annan för [Vnet som finns i olika prenumerationer](#difsub).

## <a name="samesub"></a>Ansluta Vnet som finns i hello samma prenumeration

![v2v-diagram](./media/vpn-gateway-howto-vnet-vnet-cli/v2vrmps.png)

### <a name="before-you-begin"></a>Innan du börjar

Innan du börjar installera hello senaste versionen av hello CLI-kommandona (2.0 eller senare). Information om hur du installerar hello CLI-kommandon finns [installera Azure CLI 2.0](/cli/azure/install-azure-cli).

### <a name="Plan"></a>Planera dina IP-adressintervall

I följande steg hello, skapar vi två virtuella nätverk samt deras respektive gateway-undernät och konfigurationer. Vi skapar sedan en VPN-anslutning mellan hello två Vnet. Det är viktigt tooplan hello IP-adressintervall för nätverkskonfigurationen. Tänk på att inga av dina VNet-intervall eller lokala nätverksintervall får överlappa varandra på något sätt. I de här exemplen tar vi inte med någon DNS-server. Information om hur du använder namnmatchning för dina virtuella nätverk finns i [Namnmatchning](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Vi använder följande värden i hello exempel hello:

**Värden för TestVNet1:**

* VNet-namn: TestVNet1
* Resursgrupp: TestRG1
* Plats: Östra USA
* TestVNet1: 10.11.0.0/16 & 10.12.0.0/16
* FrontEnd: 10.11.0.0/24
* BackEnd: 10.12.0.0/24
* GatewaySubnet: 10.12.255.0/27
* GatewayName: VNet1GW
* Offentlig IP: VNet1GWIP
* VPNType: RouteBased
* Connection(1to4): VNet1toVNet4
* Connection(1to5): VNet1toVNet5
* ConnectionType: VNet2VNet

**Värden för TestVNet4:**

* VNet-namn: TestVNet4
* TestVNet2: 10.41.0.0/16 & 10.42.0.0/16
* FrontEnd: 10.41.0.0/24
* BackEnd: 10.42.0.0/24
* GatewaySubnet: 10.42.255.0/27
* Resursgrupp: TestRG4
* Plats: Västra USA
* GatewayName: VNet4GW
* Offentlig IP: VNet4GWIP
* VPNType: RouteBased
* Anslutning: VNet4toVNet1
* ConnectionType: VNet2VNet


### <a name="Connect"></a>Steg 1 – ansluta tooyour prenumeration

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-numbers-include.md)]

### <a name="TestVNet1"></a>Steg 2 – Skapa och konfigurera TestVNet1

1. Skapa en resursgrupp.

  ```azurecli
  az group create -n TestRG1  -l eastus
  ```
2. Skapa TestVNet1 och hello undernät för TestVNet1. I följande exempel skapas ett virtuellt nätverk med namnet ”TestVNet1” och ett undernät, ”FrontEnd”.

  ```azurecli
  az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.11.0.0/16 -l eastus --subnet-name FrontEnd --subnet-prefix 10.11.0.0/24
  ```
3. Skapa en ytterligare adressutrymme för hello backend-undernät. Observera att i det här steget ska vi anger både hello-adressutrymmet som vi skapade tidigare och hello ytterligare adressutrymme som vi vill tooadd. Detta beror på att hello [az network vnet uppdatering](https://docs.microsoft.com/cli/azure/network/vnet#update) kommando skriver över tidigare hello-inställningarna. Se till att toospecify alla hello adressprefix när du använder det här kommandot.

  ```azurecli
  az network vnet update -n TestVNet1 --address-prefixes 10.11.0.0/16 10.12.0.0/16 -g TestRG1
  ```
4. Skapa hello backend-undernät.
  
  ```azurecli
  az network vnet subnet create --vnet-name TestVNet1 -n BackEnd -g TestRG1 --address-prefix 10.12.0.0/24 
  ```
5. Skapa hello gateway-undernätet. Observera att hello gateway-undernätet har namnet 'GatewaySubnet'. Namnet är obligatoriskt. I det här exemplet hello gateway-undernätet med hjälp av en minst/27. Det är möjligt toocreate ett gatewayundernät så liten som /29, rekommenderar vi att du skapar ett större undernät som innehåller flera adresser genom att välja minst /28 eller minst/27. Detta gör att tillräckligt många adresser tooaccommodate möjliga ytterligare konfigurationer som du vill i hello framtida.

  ```azurecli 
  az network vnet subnet create --vnet-name TestVNet1 -n GatewaySubnet -g TestRG1 --address-prefix 10.12.255.0/27
  ```
6. Begär en offentlig IP-adress toobe allokerade toohello gateway skapas för ditt VNet. Observera att hello AllocationMethod är dynamiska. Du kan inte ange hello IP-adress som du vill toouse. Det är dynamiskt allokerade tooyour gateway.

  ```azurecli
  az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
  ```
7. Skapa hello virtuell nätverksgateway för TestVNet1. VNet-till-VNet-konfigurationer kräver VpnType RouteBased. Om du kör det här kommandot med hello – inget - wait-parameter, kan du inte ser några feedback eller utdata. hello tillåter – inget - wait-parameter som hello gateway toocreate i hello bakgrund. Det innebär inte att hello VPN-gateway är klar skapar omedelbart. Skapa en gateway kan ofta ta 45 minuter eller mer beroende på hello gateway SKU som du använder.

  ```azurecli
  az network vnet-gateway create -n VNet1GW -l eastus --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <a name="TestVNet4"></a>Steg 3 – Skapa och konfigurera TestVNet4

1. Skapa en resursgrupp.

  ```azurecli
  az group create -n TestRG4  -l westus
  ```
2. Skapa TestVNet4.

  ```azurecli
  az network vnet create -n TestVNet4 -g TestRG4 --address-prefix 10.41.0.0/16 -l westus --subnet-name FrontEnd --subnet-prefix 10.41.0.0/24
  ```

3. Skapa extra undernät för TestVNet4.

  ```azurecli
  az network vnet update -n TestVNet4 --address-prefixes 10.41.0.0/16 10.42.0.0/16 -g TestRG4 
  az network vnet subnet create --vnet-name TestVNet4 -n BackEnd -g TestRG4 --address-prefix 10.42.0.0/24 
  ```
4. Skapa hello gateway-undernätet.

  ```azurecli
   az network vnet subnet create --vnet-name TestVNet4 -n GatewaySubnet -g TestRG4 --address-prefix 10.42.255.0/27
  ```
5. Begär en offentlig IP-adress.

  ```azurecli
  az network public-ip create -n VNet4GWIP -g TestRG4 --allocation-method Dynamic
  ```
6. Skapa hello TestVNet4 virtuell nätverksgateway.

  ```azurecli
  az network vnet-gateway create -n VNet4GW -l westus --public-ip-address VNet4GWIP -g TestRG4 --vnet TestVNet4 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <a name="createconnect"></a>Steg 4 – skapa hello-anslutningar

Nu har du två VNets med VPN-gatewayer. hello nästa steg är toocreate VPN gateway-anslutningar mellan hello virtuella nätverksgatewayerna. Om du har använt hello exemplen ovan är VNet-gateways i olika resursgrupper. När gatewayer har olika resursgrupper kan du behöver tooidentify och ange hello resurs-ID för varje gateway när en anslutning upprättas. Om ditt Vnet i hello samma resursgrupp kan du använda hello [andra uppsättningen anvisningar](#samerg) eftersom du inte behöver toospecify hello resurs-ID.

### <a name="diffrg"></a>tooconnect Vnet som finns i olika resursgrupper

1. Hämta hello resurs-ID för VNet1GW från hello utdata från hello följande kommando:

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  Hello utdata och hitta hello ”id”: rad. hello värden inom citattecken hello är nödvändiga toocreate hello anslutning i hello nästa avsnitt. Kopiera dessa värden tooa textredigerare, till exempel Anteckningar, så att du enkelt kan klistra in dem när du skapar din anslutning.

  Exempel på utdata:

  ```
  "activeActive": false, 
  "bgpSettings": { 
    "asn": 65515, 
    "bgpPeeringAddress": "10.12.255.30", 
    "peerWeight": 0 
   }, 
  "enableBgp": false, 
  "etag": "W/\"ecb42bc5-c176-44e1-802f-b0ce2962ac04\"", 
  "gatewayDefaultSite": null, 
  "gatewayType": "Vpn", 
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW", 
  "ipConfigurations":
  ```

  Kopiera hello värden efter **”id”:** inom hello citattecken.

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
 ```

2. Hämta hello VNet4GW resurs-ID och kopiera hello värden tooa textredigerare.

  ```azurecli
  az network vnet-gateway show -n VNet4GW -g TestRG4
  ```

3. Skapa hello TestVNet1 tooTestVNet4 anslutning. I det här steget skapar du hello anslutning från TestVNet1 tooTestVNet4. Det finns en delad nyckel som refereras i hello exempel. Du kan använda egna värden för hello delad nyckel. hello viktig sak är att hello delad nyckel måste matcha för både anslutningar. Skapa en anslutning tar en kort när toocomplete.

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW 
  ```
4. Skapa hello TestVNet4 tooTestVNet1 anslutning. Det här steget är liknande toohello en ovan, förutom att du skapar hello anslutning från TestVNet4 tooTestVNet1. Se till att matcha hello delade nycklar. Det tar några minuter tooestablish hello anslutning.

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG4 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW -l westus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1G
  ```
5. Verifiera dina anslutningar. Se [Verifiera din anslutning](#verify).

### <a name="samerg"></a>tooconnect Vnet som finns i hello samma resursgrupp

1. Skapa hello TestVNet1 tooTestVNet4 anslutning. I det här steget skapar du hello anslutning från TestVNet1 tooTestVNet4. Meddelande hello resursgrupper är hello samma i hello exempel. Du kan också se en delad nyckel som refereras i hello exempel. Du kan använda egna värden för hello delad nyckel, men hello delad nyckel måste matcha för både anslutningar. Skapa en anslutning tar en kort när toocomplete.

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet4GW
  ```
2. Skapa hello TestVNet4 tooTestVNet1 anslutning. Det här steget är liknande toohello en ovan, förutom att du skapar hello anslutning från TestVNet4 tooTestVNet1. Se till att matcha hello delade nycklar. Det tar några minuter tooestablish hello anslutning.

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG1 --vnet-gateway1 VNet4GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet1GW
  ```
3. Verifiera dina anslutningar. Se [Verifiera din anslutning](#verify).

## <a name="difsub"></a>Ansluta VNets som finns i olika prenumerationer

![v2v-diagram](./media/vpn-gateway-howto-vnet-vnet-cli/v2vdiffsub.png)

I det här scenariot ansluter vi TestVNet1 och TestVNet5. Hej Vnet finnas olika prenumerationer. hello prenumerationer behöver inte toobe som är associerade med hello samma Active Directory-klient. hello steg för den här konfigurationen kan du lägga till ytterligare VNet-till-VNet en anslutning i ordning tooconnect TestVNet1 tooTestVNet5.

### <a name="TestVNet1diff"></a>Steg 5 – Skapa och konfigurera TestVNet1

Dessa instruktioner fortsätta från hello stegen i föregående avsnitt hello. Du måste slutföra [steg 1](#Connect) och [steg 2](#TestVNet1) toocreate TestVNet1 och konfigurera hello VPN-Gateway för TestVNet1. För den här konfigurationen är du inte nödvändiga toocreate TestVNet4 från hello föregående avsnitt, men om du skapar det, kommer det står i konflikt med de här stegen. När du har slutfört steg 1 och steg 2 fortsätter du med steg 6 (nedan).

### <a name="verifyranges"></a>Steg 6 – verifiera hello IP-adressintervall

När du skapar ytterligare anslutningar är det viktigt tooverify som inte överlappar hello IP-adressutrymme hello nytt virtuellt nätverk med någon av dina VNet intervall eller lokala nätverksintervall för gateway. Du kan använda följande värden för hello TestVNet5 hello i den här övningen:

**Värden för TestVNet5:**

* VNet-namn: TestVNet5
* Resursgrupp: TestRG5
* Plats: Östra Japan
* TestVNet5: 10.51.0.0/16 & 10.52.0.0/16
* FrontEnd: 10.51.0.0/24
* BackEnd: 10.52.0.0/24
* GatewaySubnet: 10.52.255.0.0/27
* GatewayName: VNet5GW
* Offentlig IP: VNet5GWIP
* VPNType: RouteBased
* Anslutning: VNet5toVNet1
* ConnectionType: VNet2VNet

### <a name="TestVNet5"></a>Steg 7 – Skapa och konfigurera TestVNet5

Det här steget måste utföras i hello kontext hello ny prenumeration, prenumeration 5. Den här delen kan utföras av Hej administratör i en annan organisation som äger hello prenumeration. tooswitch mellan prenumerationer Använd ' az listan över--alla ' toolist hello prenumerationer tillgängliga tooyour konto och sedan använda ' az inställt--prenumeration <subscriptionID>' tooswitch toohello prenumeration som du vill toouse.

1. Kontrollera att du är ansluten tooSubscription 5 och sedan skapa en resursgrupp.

  ```azurecli
  az group create -n TestRG5  -l japaneast
  ```
2. Skapa TestVNet5.

  ```azurecli
  az network vnet create -n TestVNet5 -g TestRG5 --address-prefix 10.51.0.0/16 -l japaneast --subnet-name FrontEnd --subnet-prefix 10.51.0.0/24
  ```

3. Lägg till undernät.

  ```azurecli
  az network vnet update -n TestVNet5 --address-prefixes 10.51.0.0/16 10.52.0.0/16 -g TestRG5
  az network vnet subnet create --vnet-name TestVNet5 -n BackEnd -g TestRG5 --address-prefix 10.52.0.0/24
  ```

4. Lägg till hello gateway-undernätet.

  ```azurecli
  az network vnet subnet create --vnet-name TestVNet5 -n GatewaySubnet -g TestRG5 --address-prefix 10.52.255.0/27
  ```

5. Begär en offentlig IP-adress.

  ```azurecli
  az network public-ip create -n VNet5GWIP -g TestRG5 --allocation-method Dynamic
  ```
6. Skapa hello TestVNet5 gateway

  ```azurecli
  az network vnet-gateway create -n VNet5GW -l japaneast --public-ip-address VNet5GWIP -g TestRG5 --vnet TestVNet5 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <a name="connections5"></a>Steg 8 – skapa hello-anslutningar

Vi dela det här steget i två CLI-sessioner som markerats som **[prenumeration 1]**, och **[prenumeration 5]** eftersom hello gateways hello olika prenumerationer. tooswitch mellan prenumerationer Använd ' az listan över--alla ' toolist hello prenumerationer tillgängliga tooyour konto och sedan använda ' az inställt--prenumeration <subscriptionID>' tooswitch toohello prenumeration som du vill toouse.

1. **[Prenumeration 1]**  Logga in och ansluta tooSubscription 1. Hello kör följande kommando tooget hello namn och ID för hello Gateway från hello utdata:

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  Kopiera hello utdata för ”id”:. Skicka hello-ID och hello namnet på hello virtuella nätverk (VNet1GW) toohello gatewayadministratören prenumeration 5 via e-post eller någon annan metod.

  Exempel på utdata:

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
  ```

2. **[Prenumerationen 5]**  Logga in och ansluta tooSubscription 5. Hello kör följande kommando tooget hello namn och ID för hello Gateway från hello utdata:

  ```azurecli
  az network vnet-gateway show -n VNet5GW -g TestRG5
  ```

  Kopiera hello utdata för ”id”:. Skicka hello-ID och hello namnet på hello virtuella nätverk (VNet5GW) toohello gatewayadministratören prenumeration 1 via e-post eller någon annan metod.

3. **[Prenumeration 1]**  i det här steget kan du skapa hello anslutning från TestVNet1 tooTestVNet5. Du kan använda egna värden för hello delad nyckel, men hello delad nyckel måste matcha för både anslutningar. Skapa en anslutning kan ta en kort när toocomplete. Kontrollera att du ansluter tooSubscription 1.

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet5 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```

4. **[Prenumerationen 5]**  Det här steget är liknande toohello en ovan, förutom att du skapar hello anslutning från TestVNet5 tooTestVNet1. Kontrollera att hello delade nycklar matchar och du ansluter tooSubscription 5.

  ```azurecli
  az network vpn-connection create -n VNet5ToVNet1 -g TestRG5 --vnet-gateway1 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW -l japaneast --shared-key "eeffgg" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```

## <a name="verify"></a>Kontrollera hello-anslutningar
[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections v2v cli](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <a name="faq"></a>Vanliga frågor och svar om VNet-till-VNet
[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Nästa steg

* När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Mer information finns i hello [virtuella datorer dokumentationen](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* Information om BGP finns hello [BGP översikt](vpn-gateway-bgp-overview.md) och [hur tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
