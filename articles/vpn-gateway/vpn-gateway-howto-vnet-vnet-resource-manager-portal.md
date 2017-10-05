---
title: "Ansluta ett virtuellt Azure-nätverk till ett annat VNet: portalen | Microsoft Docs"
description: "Skapa en VPN-gateway-anslutning mellan virtuella nätverk med hjälp av Resource Manager och Azure Portal."
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
ms.openlocfilehash: 0293495a9cbdab1fc797d9948e4cbb7759b1ba54
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-the-azure-portal"></a>Konfigurera en VPN-gatewayanslutning mellan virtuella nätverk med hjälp av Azure Portal

Den här artikeln visar hur du skapar en VPN-gatewayanslutning mellan virtuella nätverk. De virtuella nätverken kan finnas i samma eller olika regioner och i samma eller olika prenumerationer. När du ansluter virtuella nätverk från olika prenumerationer, behöver inte prenumerationerna vara associerade med samma Active Directory-klient. 

Anvisningarna i den här artikeln gäller för Resource Manager-distributionsmodellen och användning av Azure-portalen. Du kan också skapa den här konfigurationen med ett annat distributionsverktyg eller en annan distributionsmodell genom att välja ett annat alternativ i listan nedan:

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

Du ansluter ett virtuellt nätverk till ett annat virtuellt nätverk (VNet-till-VNet) på nästan samma sätt som du ansluter ett VNet till en lokal plats. Båda typerna av anslutning använder en VPN-gateway för att få en säker tunnel med IPsec/IKE. Om dina VNets finns i samma region kan det vara bättre att ansluta dem med hjälp av VNet-peering. Ingen VPN-gateway används för VNet-peering. Mer information finns i [VNet peering (Vnet-peering)](../virtual-network/virtual-network-peering-overview.md).

VNet-till-VNet-kommunikation kan kombineras med konfigurationer för flera platser. Därmed kan du etablera nätverkstopologier som kombinerar anslutningar mellan olika anläggningar med virtuell nätverksanslutning enligt följande diagram:

![Om anslutningar](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Om anslutningar")

### <a name="why-connect-virtual-networks"></a>Varför ska man ansluta virtuella nätverk?

Du kan vilja ansluta virtuella nätverk av följande skäl:

* **Geografisk redundans i flera regioner och geografisk närvaro**
  
  * Du kan ange din egna geografiska replikering eller synkronisering med en säker anslutning, utan att passera några Internet-slutpunkter.
  * Med Azure Traffic Manager och Load Balancer kan du konfigurera arbetsbelastning med hög tillgänglighet och geografisk redundans över flera Azure-regioner. Ett viktigt exempel är att konfigurera att SQL alltid är aktiverat med tillgänglighetsgrupper som är spridda över flera Azure-regioner.
* **Regionala flernivåprogram med isolering eller administrativa gränser**
  
  * Inom samma region kan du konfigurera flernivåprogram med flera virtuella nätverk som är anslutna till varandra på grund av isolering eller administrativa krav.

Mer information om anslutningar mellan virtuella nätverk finns i [Vanliga frågor om VNet-till-VNet](#faq) i slutet av den här artikeln. Observera att du inte skapa anslutningen i portalen om dina VNet finns i olika prenumerationer. Du kan använda [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) eller [CLI](vpn-gateway-howto-vnet-vnet-cli.md).

### <a name="values"></a>Exempelinställningar

När du följer dessa steg som en övning kan du använda följande exempelinställningsvärden. I exempelsyfte använder vi flera adressutrymmen för varje enskilt virtuellt nätverk. VNet-till-VNet-konfigurationer kräver dock inte flera adressutrymmen.

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
* Namn för gateway-undernät: GatewaySubnet (anges automatiskt i portalen)
  * Adressintervall för gateway-undernätet: 10.11.255.0/27
* DNS-Server: Använd IP-adressen för din DNS-server
* Namn för det virtuella nätverkets gateway: TestVNet1GW
* Gateway-typ: VPN
* VPN-typ: Routningsbaserad
* SKU: Välj den gateway-SKU som du vill använda
* Offentligt IP-adressnamn: TestVNet1GWIP
* Anslutningsvärden:
  * Namn: TestVNet1toTestVNet4
  * Delad nyckel: Du kan skapa den delade nyckeln själv. I det här exemplet använder vi abc123. Det viktiga är att värdet matchar när du skapar anslutningen mellan de virtuella nätverken.

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
* Namn för gateway-undernät: GatewaySubnet (anges automatiskt i portalen)
  * Adressintervall för gateway-undernätet: 10.41.255.0/27
* DNS-Server: Använd IP-adressen för din DNS-server
* Namn för det virtuella nätverkets gateway: TestVNet4GW
* Gateway-typ: VPN
* VPN-typ: Routningsbaserad
* SKU: Välj den gateway-SKU som du vill använda
* Offentligt IP-adressnamn: TestVNet4GWIP
* Anslutningsvärden:
  * Namn: TestVNet4toTestVNet1
  * Delad nyckel: Du kan skapa den delade nyckeln själv. I det här exemplet använder vi abc123. Det viktiga är att värdet matchar när du skapar anslutningen mellan de virtuella nätverken.

## <a name="CreatVNet"></a>1. Skapa och konfigurera TestVNet1
Om du redan har ett VNet, kontrollerar du att inställningarna är kompatibla med din VPN-gatewaydesign. Var särskilt noga med alla undernät som överlappar med andra nätverk. Om du har överlappande undernät fungerar inte anslutningen ordentligt. Om ditt VNet är konfigurerat med de korrekta inställningarna, kan du börja med stegen i avsnittet [Ange en DNS-server](#dns).

### <a name="to-create-a-virtual-network"></a>Så här skapar du ett virtuellt nätverk
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="subnets"></a>2. Lägg till ytterligare adressutrymmen och skapa undernät
Du kan lägga till ytterligare adressutrymme och skapa undernät när ditt virtuella nätverk har skapats.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. Skapa ett gateway-undernät
Innan du ansluter det virtuella nätverket till en gateway, måste du skapa gateway-undernätet för det virtuella nätverk som du vill ansluta till. Om möjligt är det bäst att skapa ett gateway-undernät med CIDR-block av /28 eller /27 för att tillhandahålla tillräckligt med IP-adresser för att hantera ytterligare framtida konfigurationskrav.

Om du skapar den här konfigurationen för att öva dig, kan du hänvisa till de här [exempelvärdena](#values) när du skapar gateway-undernätet.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Så här skapar du ett gateway-undernät
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>4. Ange en DNS-server (valfritt)
DNS krävs inte för VNet-till-VNet-anslutningar. Om du vill använda namnmatchning för resurser som distribueras till ditt virtuella nätverk bör du dock ange en DNS-server. Med den här inställningen kan du ange vilken DNS-server du vill använda för namnmatchning för det här virtuella nätverket. Den skapar inte någon DNS-server.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. Skapa en virtuell nätverksgateway
I det här steget ska du skapa den virtuella nätverksgatewayen för ditt virtuella nätverk. Att skapa en gateway kan ofta ta 45 minuter eller mer, beroende på vald gateway-SKU. Om du skapar den här konfigurationen för att öva dig kan du se [exempelinställningarna](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Så här skapar du en virtuell nätverksgateway
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="CreateTestVNet4"></a>6. Skapa och konfigurera TestVNet4
När du har konfigurerat TestVNet1 kan du skapa TestVNet4 genom att upprepa föregående steg och ersätta värdena med de för TestVNet4. Du behöver inte vänta tills den virtuella nätverksgatewayen för TestVNet1 har skapats innan du konfigurerar TestVNet4. Om du använder egna värden måste du kontrollera att adressutrymmena inte överlappar några av de virtuella nätverk som du vill ansluta till.

## <a name="TestVNet1Connection"></a>7. Konfigurera TestVNet1-anslutningen
När de virtuella nätverksgatewayerna för både TestVNet1 och TestVNet4 har slutförts kan du skapa gateway-anslutningar för de virtuella nätverken. I det här avsnittet skapar du en anslutning från VNet1 till VNet4. De här stegen fungerar endast för olika VNet i samma prenumeration. Om dina VNet finns i olika prenumerationer, måste du använda PowerShell för att ansluta. Mer information finns i [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)-artikeln.

1. I **Alla resurser** navigerar du till den virtuella nätverksgatewayen för ditt virtuella nätverk. Till exempel **TestVNet1GW**. Klicka på **TestVNet1GW** för att öppna bladet för den virtuella nätverksgatewayen.
   
    ![Bladet Anslutningar](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Bladet Anslutningar")
2. Klicka på **+ Lägg till** att öppna bladet **Lägg till anslutning**.
3. I namnfältet på bladet **Lägg till anslutning** anger du ett namn för anslutningen. Till exempel **TestVNet1toTestVNet4**.
   
    ![Anslutningsnamn](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Anslutningsnamn")
4. För **Anslutningstyp** väljer du **VNet till VNet** i den nedrullningsbara listan.
5. Fältet **Första virtuella nätverksgateway** fylls i automatiskt eftersom du skapar den här anslutningen från den angivna virtuella nätverksgatewayen.
6. Fältet **Andra virtuella nätverksgateway** är den virtuella nätverksgatewayen för det virtuella nätverk som du vill skapa en anslutning till. Klicka på **Välj en annan virtuell nätverksgateway** för att öppna bladet **Välj en virtuell nätverksgateway**.
   
    ![Lägg till anslutning](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Lägg till en anslutning")
7. Visa de virtuella nätverksgatewayer som anges på det här bladet. Observera att endast virtuella nätverksgatewayer som ingår i din prenumeration visas. Om du vill ansluta till en virtuell nätverksgateway som inte ingår i din prenumeration kan du läsa [PowerShell-artikeln](vpn-gateway-vnet-vnet-rm-ps.md). 
8. Klicka på den virtuella nätverksgatewayen som du vill ansluta till.
9. I fältet **Delad nyckel** anger du en delad nyckel för anslutningen. Du kan generera eller skapa den här nyckeln själv. I en plats-till-plats-anslutning är nyckeln du använder exakt densamma som för din lokala enhet och anslutningen via din virtuella nätverksgateway. Konceptet är i princip samma här, förutom att du istället för att ansluta till en VPN-enhet ansluter till en annan virtuell nätverksgateway.
   
    ![Delad nyckel](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Delad nyckel")
10. Klicka på **OK** längst ned på bladet för att spara ändringarna.

## <a name="TestVNet4Connection"></a>8. Konfigurera TestVNet4-anslutningen
Skapa sedan en anslutning från TestVNet4 till TestVNet1. Använd samma metod som du använde för att skapa anslutningen från TestVNet1 till TestVNet4. Kontrollera att du använder samma delad nyckel.

## <a name="VerifyConnection"></a>9. Verifiera din anslutning
Verifiera anslutningen. Gör följande för varje virtuell nätverksgateway:

1. Leta rätt på bladet för den virtuella nätverksgatewayen. Till exempel **TestVNet4GW**. 
2. På bladet för den virtuella nätverksgatewayen klickar du på **Anslutningar** för att visa anslutningsbladet för den virtuella nätverksgatewayen.

Visa anslutningarna och kontrollera statusen. När anslutningen har skapats kan du se **Lyckades** och **Ansluten** som statusvärden.

![Lyckades](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Lyckades")

Du kan dubbelklicka på varje anslutning om du vill visa mer information om anslutningen.

![Information](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Information")

## <a name="faq"></a>Vanliga frågor och svar om VNet-till-VNet
Visa vanliga frågor och svar om du vill ha mer information om anslutningar mellan virtuella nätverk.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Nästa steg
När anslutningen är klar kan du lägga till virtuella datorer till dina virtuella nätverk. Mer information finns i [dokumentationen för Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
