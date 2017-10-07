---
title: "Ansluta ett virtuellt Azure-nätverk tooanother VNet: Portal | Microsoft Docs"
description: "Skapa en VPN-gateway-anslutningen mellan Vnet med hjälp av hanteraren för filserverresurser och hello Azure-portalen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a7015cfc-764b-46a1-bfac-043d30a275df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: a529f90d976bee0f50403947d06e9da8a6c05349
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-hello-azure-portal"></a>Konfigurera ett VNet-till-VNet VPN gateway-anslutningen med hjälp av hello Azure-portalen

Den här artikeln beskrivs hur du toocreate en VPN-gateway-anslutningen mellan virtuella nätverk. hello virtuella nätverk kan vara i samma eller olika regioner hello och hello från samma eller olika prenumerationer. När du ansluter Vnet från olika prenumerationer, hello prenumerationer inte behöver toobe som är associerade med hello samma Active Directory-klient. 

hello stegen i den här artikeln gäller toohello Resource Manager-distributionsmodellen och använda hello Azure-portalen. Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure Portal (klassisk)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Ansluta olika distributionsmodeller – Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Ansluta olika distributionsmodeller – PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![v2v-diagram](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

Ansluta ett virtuellt nätverk tooanother virtuellt nätverk (VNet-till-VNet) är liknande tooconnecting ett VNet tooan lokal plats. Båda typerna av anslutningen använder en VPN-gateway tooprovide en säker tunnel med IPsec/IKE. Om ditt Vnet i hello samma region som du kanske vill tooconsider ansluter dem med hjälp av VNet-Peering. Ingen VPN-gateway används för VNet-peering. Mer information finns i [VNet peering (Vnet-peering)](../virtual-network/virtual-network-peering-overview.md).

VNet-till-VNet-kommunikation kan kombineras med konfigurationer för flera platser. På så sätt kan du etablera nätverkstopologier som kombinerar korsanslutningar med mellan virtuell nätverksanslutning som visas i följande diagram hello:

![Om anslutningar](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Om anslutningar")

### <a name="why-connect-virtual-networks"></a>Varför ska man ansluta virtuella nätverk?

Du kanske vill tooconnect virtuella nätverk för hello följande orsaker:

* **Geografisk redundans i flera regioner och geografisk närvaro**
  
  * Du kan ange din egna geografiska replikering eller synkronisering med en säker anslutning, utan att passera några Internet-slutpunkter.
  * Med Azure Traffic Manager och Load Balancer kan du konfigurera arbetsbelastning med hög tillgänglighet och geografisk redundans över flera Azure-regioner. Ett viktigt exempel är tooset in SQL Always On med Tillgänglighetsgrupper sprida över flera Azure-regioner.
* **Regionala flernivåprogram med isolering eller administrativa gränser**
  
  * Hej i samma region, du kan konfigurera flera nivåer program med flera virtuella nätverk som kopplar samman förfallodatum tooisolation eller administrativa krav.

Mer information om VNet-till-VNet-anslutningar finns hello [VNet-till-VNet vanliga frågor och svar](#faq) hello slutet av den här artikeln. Observera att du inte kan skapa hello anslutning i hello-portalen om ditt Vnet i olika prenumerationer. Du kan använda [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) eller [CLI](vpn-gateway-howto-vnet-vnet-cli.md).

### <a name="values"></a>Exempelinställningar

När du använder de här stegen som Övning, kan du använda hello exempelvärden för inställningar. I exempelsyfte använder vi flera adressutrymmen för varje enskilt virtuellt nätverk. VNet-till-VNet-konfigurationer kräver dock inte flera adressutrymmen.

**Värden för TestVNet1:**

* VNet-namn: TestVNet1
* Adressutrymme: 10.11.0.0/16
  * Undernätsnamn: FrontEnd
  * Adressintervall för undernätet: 10.11.0.0/24
* Resursgrupp: TestRG1
* Plats: Östra USA
* Adressutrymme: 10.12.0.0/16
  * Undernätsnamn: BackEnd
  * Adressintervall för undernätet: 10.12.0.0/24
* Namnet för gateway-undernätet: GatewaySubnet (kommer Autofyll i hello portal)
  * Adressintervall för gateway-undernätet: 10.11.255.0/27
* DNS-Server: Använd hello IP-adress för DNS-Server
* Namn för det virtuella nätverkets gateway: TestVNet1GW
* Gateway-typ: VPN
* VPN-typ: Routningsbaserad
* SKU: Välj hello Gateway-SKU som du vill toouse
* Offentligt IP-adressnamn: TestVNet1GWIP
* Anslutningsvärden:
  * Namn: TestVNet1toTestVNet4
  * Delad nyckel: du kan skapa hello delad nyckel själv. I det här exemplet använder vi abc123. hello viktiga är att hello-värdet måste matcha när du skapar hello anslutning mellan hello Vnet.

**Värden för TestVNet4:**

* VNet-namn: TestVNet4
* Adressutrymme: 10.41.0.0/16
  * Undernätsnamn: FrontEnd
  * Adressintervall för undernätet: 10.41.0.0/24
* Resursgrupp: TestRG1
* Plats: Västra USA
* Adressutrymme: 10.42.0.0/16
  * Undernätsnamn: BackEnd
  * Adressintervall för undernätet: 10.42.0.0/24
* GatewaySubnet namn: GatewaySubnet (kommer Autofyll i hello portal)
  * Adressintervall för gateway-undernätet: 10.41.255.0/27
* DNS-Server: Använd hello IP-adress för DNS-Server
* Namn för det virtuella nätverkets gateway: TestVNet4GW
* Gateway-typ: VPN
* VPN-typ: Routningsbaserad
* SKU: Välj hello Gateway-SKU som du vill toouse
* Offentligt IP-adressnamn: TestVNet4GWIP
* Anslutningsvärden:
  * Namn: TestVNet4toTestVNet1
  * Delad nyckel: du kan skapa hello delad nyckel själv. I det här exemplet använder vi abc123. hello viktiga är att hello-värdet måste matcha när du skapar hello anslutning mellan hello Vnet.

## <a name="CreatVNet"></a>1. Skapa och konfigurera TestVNet1
Om du redan har ett virtuellt nätverk kontrollerar du att hello inställningarna är kompatibel med din design av VPN-gateway. Särskilt noga tooany undernät som kan överlappa andra nätverk. Om du har överlappande undernät fungerar inte anslutningen ordentligt. Om ditt VNet är konfigurerad med rätt hello-inställningar, kan du börja hello stegen i hello [ange en DNS-server](#dns) avsnitt.

### <a name="toocreate-a-virtual-network"></a>toocreate ett virtuellt nätverk
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="subnets"></a>2. Lägg till ytterligare adressutrymmen och skapa undernät
Du kan lägga till ytterligare adressutrymme och skapa undernät när ditt virtuella nätverk har skapats.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. Skapa ett gateway-undernät
Innan du ansluter din virtuella nätverksgateway tooa, måste du först toocreate hello gateway-undernätet hello virtuellt nätverk toowhich du vill ha tooconnect. Om möjligt, är det bästa toocreate ett gateway-undernät med CIDR-block av /28 eller minst/27 i ordning tooprovide tillräckligt med IP-adresser tooaccommodate ytterligare framtida konfigurationskrav.

Om du skapar den här konfigurationen som Övning finns toothese [exempel inställningarna](#values) när du skapar din gateway-undernätet.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="toocreate-a-gateway-subnet"></a>toocreate ett gateway-undernät
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>4. Ange en DNS-server (valfritt)
DNS krävs inte för VNet-till-VNet-anslutningar. Om du vill toohave namnmatchning för resurser som är distribuerade tooyour virtuella nätverk, ska du ange en DNS-server. Den här inställningen kan du ange hello DNS-server som du vill toouse för namnmatchning för det här virtuella nätverket. Den skapar inte någon DNS-server.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. Skapa en virtuell nätverksgateway
I det här steget skapar du hello virtuell nätverksgateway för din VNet. Skapa en gateway kan ofta ta 45 minuter eller mer beroende på hello markerad gateway SKU. Om du skapar den här konfigurationen som Övning, kan du läsa toohello [exempel inställningarna](#values).

### <a name="toocreate-a-virtual-network-gateway"></a>toocreate en virtuell nätverksgateway
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="CreateTestVNet4"></a>6. Skapa och konfigurera TestVNet4
När du har konfigurerat TestVNet1 kan du skapa TestVNet4 genom att upprepa föregående steg hello, ersätter hello värden med de TestVNet4. Du behöver inte toowait tills hello virtuell nätverksgateway för TestVNet1 har skapat innan du konfigurerar TestVNet4. Om du använder egna värden kan du kontrollera att hello adressutrymmen inte överlappar med någon av hello Vnet som du vill tooconnect till.

## <a name="TestVNet1Connection"></a>7. Konfigurera hello TestVNet1 anslutning
När hello virtuella nätverksgatewayerna för både TestVNet1 och TestVNet4 är klar kan skapa du det virtuella nätverket gateway-anslutningar. I det här avsnittet ska du skapa en anslutning från VNet1 tooVNet4. De här stegen fungerar endast för VNets i hello samma prenumeration. Om din Vnet har olika prenumerationer, måste du använda PowerShell toomake hello anslutning. Se hello [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) artikel.

1. I **alla resurser**, navigera toohello virtuell nätverksgateway för din VNet. Till exempel **TestVNet1GW**. Klicka på **TestVNet1GW** tooopen hello virtuella nätverksgateway-bladet.
   
    ![Bladet Anslutningar](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Bladet Anslutningar")
2. Klicka på **+ Lägg till** tooopen hello **Lägg till anslutning** bladet.
3. På hello **Lägg till anslutning** bladet i hello fältnamn, Skriv ett namn för anslutningen. Till exempel **TestVNet1toTestVNet4**.
   
    ![Anslutningsnamn](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Anslutningsnamn")
4. För **Anslutningstyp** Välj **VNet-till-VNet** hello listrutan.
5. Hej **första virtuella nätverksgateway** värdet i fältet fylls i automatiskt eftersom du skapar den här anslutningen från hello angiven virtuell nätverksgateway.
6. Hej **andra virtuella nätverksgateway** fältet är hello virtuell nätverksgateway av hello virtuella nätverk som du vill toocreate en anslutning till. Klicka på **Välj en annan virtuell nätverksgateway** tooopen hello **Välj virtuell nätverksgateway** bladet.
   
    ![Lägg till anslutning](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Lägg till en anslutning")
7. Visa hello virtuella nätverksgatewayer som listas på det här bladet. Observera att endast virtuella nätverksgatewayer som ingår i din prenumeration visas. Om du vill tooconnect tooa virtuell nätverksgateway som inte är i din prenumeration, Använd hello [PowerShell artikel](vpn-gateway-vnet-vnet-rm-ps.md). 
8. Klicka på hello virtuell nätverksgateway som du vill tooconnect till.
9. I hello **delad nyckel** skriver du en delad nyckel för anslutningen. Du kan generera eller skapa den här nyckeln själv. I en plats-till-plats-anslutning skulle hello nyckel vara exakt hello samma för din lokala enhet och virtuella gateway nätverksanslutningen. hello konceptet är liknande här, förutom att i stället för anslutande tooa VPN-enhet, ansluter du tooanother virtuell nätverksgateway.
   
    ![Delad nyckel](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Delad nyckel")
10. Klicka på **OK** på hello längst ned på hello bladet toosave ändringarna.

## <a name="TestVNet4Connection"></a>8. Konfigurera hello TestVNet4 anslutning
Skapa sedan en anslutning från TestVNet4 tooTestVNet1. Använd hello samma metod som du använde toocreate hello anslutning från TestVNet1 tooTestVNet4. Kontrollera att du använder hello samma delad nyckel.

## <a name="VerifyConnection"></a>9. Verifiera din anslutning
Verifiera hello-anslutning. För varje virtuell nätverksgateway hello följande:

1. Leta upp hello bladet för hello virtuell nätverksgateway. Till exempel **TestVNet4GW**. 
2. På hello virtuella nätverksgateway-bladet, klickar du på **anslutningar** tooview hello anslutningar bladet för hello virtuell nätverksgateway.

Visa hello anslutningar och kontrollera hello status. När hello anslutningen har skapats visas **lyckades** och **ansluten** som hello statusvärden.

![Lyckades](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Lyckades")

Du kan dubbelklicka på varje anslutning separat tooview mer information om hello-anslutning.

![Information](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Information")

## <a name="faq"></a>Vanliga frågor och svar om VNet-till-VNet
Visa information om hello vanliga frågor och svar för ytterligare information om VNet-till-VNet-anslutningar.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Nästa steg
När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Se hello [virtuella datorer dokumentationen](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) för mer information.
