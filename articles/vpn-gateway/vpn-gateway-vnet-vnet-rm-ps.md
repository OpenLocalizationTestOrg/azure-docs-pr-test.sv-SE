---
title: "Ansluta ett virtuellt Azure-nätverk tooanother VNet: PowerShell | Microsoft Docs"
description: "Den här artikeln visar hur du ansluter virtuella nätverk tillsammans med hjälp av Azure Resource Manager och PowerShell."
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
ms.openlocfilehash: 2da30c76867cc3f71d040e63e0dd15d153e15c10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-powershell"></a>Konfigurera en VPN-gatewayanslutning mellan virtuella nätverk med hjälp av PowerShell

Den här artikeln beskrivs hur du toocreate en VPN-gateway-anslutningen mellan virtuella nätverk. hello virtuella nätverk kan vara i samma eller olika regioner hello och hello från samma eller olika prenumerationer. När du ansluter Vnet från olika prenumerationer, hello prenumerationer inte behöver toobe som är associerade med hello samma Active Directory-klient. 

hello stegen i den här artikeln gäller toohello Resource Manager-modellen och använda PowerShell. Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:

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

![Om anslutningar](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)

### <a name="why-connect-virtual-networks"></a>Varför ska man ansluta virtuella nätverk?

Du kanske vill tooconnect virtuella nätverk för hello följande orsaker:

* **Geografisk redundans i flera regioner och geografisk närvaro**

  * Du kan ange din egna geografiska replikering eller synkronisering med en säker anslutning, utan att passera några Internet-slutpunkter.
  * Med Azure Traffic Manager och Load Balancer kan du konfigurera arbetsbelastning med hög tillgänglighet och geografisk redundans över flera Azure-regioner. Ett viktigt exempel är tooset in SQL Always On med Tillgänglighetsgrupper sprida över flera Azure-regioner.
* **Regionala flernivåprogram med isolering eller administrativa gränser**

  * Hej i samma region, du kan konfigurera flera nivåer program med flera virtuella nätverk som kopplar samman förfallodatum tooisolation eller administrativa krav.

Mer information om VNet-till-VNet-anslutningar finns hello [VNet-till-VNet vanliga frågor och svar](#faq) hello slutet av den här artikeln.

## <a name="which-set-of-steps-should-i-use"></a>Vilka steg ska jag använda?

I den här artikeln beskrivs två uppsättningar med steg. En uppsättning steg för [Vnet som finns i hello samma prenumeration](#samesub), och en annan för [Vnet som finns i olika prenumerationer](#difsub). hello viktigaste skillnaden mellan hello mängder är om du kan skapa och konfigurera alla virtuella nätverk och gateway resurser inom hello samma PowerShell-session.

hello använder stegen i den här artikeln variabler som har deklarerats hello början av varje avsnitt. Om du redan arbetar med befintliga Vnet, kan du ändra hello variabler tooreflect hello inställningarna i din egen miljö. Information om hur du använder namnmatchning för dina virtuella nätverk finns i [Namnmatchning](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="samesub"></a>Hur tooconnect Vnet som används i hello samma prenumeration

![v2v-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Innan du börjar

Innan du börjar måste du tooinstall hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets, minst 4.0 eller senare. Mer information om hur du installerar hello PowerShell-cmdlets finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

### <a name="Step1"></a>Steg 1 – Planera dina IP-adressintervall

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


### <a name="Step2"></a>Steg 2 – Skapa och konfigurera TestVNet1

1. Deklarera dina variabler. Det här exemplet deklarerar hello variabler med hello värden för den här övningen. I de flesta fall bör du ersätta hello värdena med dina egna. Du kan använda dessa variabler om du kör via hello steg toobecome bekant med den här typen av konfiguration. Ändra hello variabler vid behov, och sedan kopiera och klistra in dem i PowerShell-konsolen.

  ```powershell
  $Sub1 = "Replace_With_Your_Subcription_Name"
  $RG1 = "TestRG1"
  $Location1 = "East US"
  $VNetName1 = "TestVNet1"
  $FESubName1 = "FrontEnd"
  $BESubName1 = "Backend"
  $GWSubName1 = "GatewaySubnet"
  $VNetPrefix11 = "10.11.0.0/16"
  $VNetPrefix12 = "10.12.0.0/16"
  $FESubPrefix1 = "10.11.0.0/24"
  $BESubPrefix1 = "10.12.0.0/24"
  $GWSubPrefix1 = "10.12.255.0/27"
  $GWName1 = "VNet1GW"
  $GWIPName1 = "VNet1GWIP"
  $GWIPconfName1 = "gwipconf1"
  $Connection14 = "VNet1toVNet4"
  $Connection15 = "VNet1toVNet5"
  ```

2. Ansluta tooyour-konto. Använd hello följande exempel toohelp du ansluta:

  ```powershell
  Login-AzureRmAccount
  ```

  Kontrollera hello prenumerationer för hello-kontot.

  ```powershell
  Get-AzureRmSubscription
  ```

  Ange hello prenumeration som du vill toouse.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub1
  ```
3. Skapa en ny resursgrupp.

  ```powershell
  New-AzureRmResourceGroup -Name $RG1 -Location $Location1
  ```
4. Skapa hello undernät för TestVNet1. I det här exemplet skapas ett virtuellt nätverk med namnet TestVNet1 och tre undernät – GatewaySubnet, FrontEnd och BackEnd. När du ersätter värden är det viktigt att du alltid namnger gateway-undernätet specifikt till GatewaySubnet. Om du ger det något annat namn går det inte att skapa gatewayen.

  hello används följande exempel hello variabler som du angett tidigare. I det här exemplet hello gateway-undernätet med hjälp av en minst/27. Det är möjligt toocreate ett gatewayundernät så liten som /29, rekommenderar vi att du skapar ett större undernät som innehåller flera adresser genom att välja minst /28 eller minst/27. Detta gör att tillräckligt många adresser tooaccommodate möjliga ytterligare konfigurationer som du vill i hello framtida.

  ```powershell
  $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
  $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
  $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1
  ```
5. Skapa TestVNet1.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
  ```
6. Begär en offentlig IP-adress toobe allokerade toohello gateway skapas för ditt VNet. Observera att hello AllocationMethod är dynamiska. Du kan inte ange hello IP-adress som du vill toouse. Det är dynamiskt allokerade tooyour gateway. 

  ```powershell
  $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
  ```
7. Skapa hello gateway-konfigurationen. gateway-konfiguration för hello definierar hello undernät och hello offentliga IP-adressen toouse. Använd hello exempel toocreate gateway-konfiguration.

  ```powershell
  $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
  $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```
8. Skapa hello gateway för TestVNet1. I det här steget skapar du hello virtuell nätverksgateway för din TestVNet1. VNet-till-VNet-konfigurationer kräver VpnType RouteBased. Skapa en gateway kan ofta ta 45 minuter eller mer beroende på hello markerad gateway SKU.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-3---create-and-configure-testvnet4"></a>Steg 3 – Skapa och konfigurera TestVNet4

När du har konfigurerat TestVNet1 skapar du TestVNet4. Följ hello steg nedan, ersätter hello värden med dina egna vid behov. Det här steget kan utföras med hello samma PowerShell-session eftersom den är i hello samma prenumeration.

1. Deklarera dina variabler. Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.

  ```powershell
  $RG4 = "TestRG4"
  $Location4 = "West US"
  $VnetName4 = "TestVNet4"
  $FESubName4 = "FrontEnd"
  $BESubName4 = "Backend"
  $GWSubName4 = "GatewaySubnet"
  $VnetPrefix41 = "10.41.0.0/16"
  $VnetPrefix42 = "10.42.0.0/16"
  $FESubPrefix4 = "10.41.0.0/24"
  $BESubPrefix4 = "10.42.0.0/24"
  $GWSubPrefix4 = "10.42.255.0/27"
  $GWName4 = "VNet4GW"
  $GWIPName4 = "VNet4GWIP"
  $GWIPconfName4 = "gwipconf4"
  $Connection41 = "VNet4toVNet1"
  ```
2. Skapa en ny resursgrupp.

  ```powershell
  New-AzureRmResourceGroup -Name $RG4 -Location $Location4
  ```
3. Skapa hello undernät för TestVNet4.

  ```powershell
  $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
  $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
  $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4
  ```
4. Skapa TestVNet4.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4
  ```
5. Begär en offentlig IP-adress.

  ```powershell
  $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AllocationMethod Dynamic
  ```
6. Skapa hello gateway-konfigurationen.

  ```powershell
  $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
  $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
  $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4
  ```
7. Skapa hello TestVNet4 gateway. Skapa en gateway kan ofta ta 45 minuter eller mer beroende på hello markerad gateway SKU.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
  -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-4---create-hello-connections"></a>Steg 4 – skapa hello-anslutningar

1. Hämta båda virtuella nätverksgatewayerna. Om båda hello gatewayer finns i hello samma prenumeration, som i exemplet hello, kan du slutföra det här steget i hello samma PowerShell-session.

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4
  ```
2. Skapa hello TestVNet1 tooTestVNet4 anslutning. I det här steget skapar du hello anslutning från TestVNet1 tooTestVNet4. Du ser en delad nyckel som refereras i hello exempel. Du kan använda egna värden för hello delad nyckel. hello viktig sak är att hello delad nyckel måste matcha för både anslutningar. Skapa en anslutning kan ta en kort när toocomplete.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
  -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
3. Skapa hello TestVNet4 tooTestVNet1 anslutning. Det här steget är liknande toohello en ovan, förutom att du skapar hello anslutning från TestVNet4 tooTestVNet1. Se till att matcha hello delade nycklar. hello-anslutning upprättas efter några minuter.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
  -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. Verifiera anslutningen. Avsnittet hello [hur tooverify anslutningen](#verify).

## <a name="difsub"></a>Hur tooconnect Vnet som finns i olika prenumerationer

![v2v-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

I det här scenariot ansluter vi TestVNet1 och TestVNet5. TestVNet1 och TestVNet5 finns i olika prenumerationer. hello prenumerationer behöver inte toobe som är associerade med hello samma Active Directory-klient. hello skillnaden mellan dessa steg och hello tidigare uppsättningen är att vissa av hello konfigurationssteg måste toobe utförs i en separat PowerShell-session i hello kontexten av andra hello-prenumeration. Särskilt när hello två prenumerationer hör toodifferent organisationer.

### <a name="step-5---create-and-configure-testvnet1"></a>Steg 5 – Skapa och konfigurera TestVNet1

Du måste slutföra [steg 1](#Step1) och [steg 2](#Step2) från hello föregående avsnittet toocreate och konfigurera TestVNet1 hello VPN-Gateway för TestVNet1. För den här konfigurationen är du inte nödvändiga toocreate TestVNet4 från hello föregående avsnitt, men om du skapar det, kommer det står i konflikt med de här stegen. När du har slutfört steg 1 och 2 fortsätter du med steg 6 toocreate TestVNet5. 

### <a name="step-6---verify-hello-ip-address-ranges"></a>Steg 6 – verifiera hello IP-adressintervall

Det är viktigt att att hello IP-adressutrymme hello nytt virtuellt nätverk, TestVNet5, inte överlappar med någon av dina VNet-adressintervall eller en lokal gateway nätverksintervall toomake. I det här exemplet kan hello virtuella nätverk höra toodifferent organisationer. Du kan använda följande värden för hello TestVNet5 hello i den här övningen:

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

### <a name="step-7---create-and-configure-testvnet5"></a>Steg 7 – Skapa och konfigurera TestVNet5

Det här steget måste utföras i hello kontext hello ny prenumeration. Den här delen kan utföras av Hej administratör i en annan organisation som äger hello prenumeration.

1. Deklarera dina variabler. Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.

  ```powershell
  $Sub5 = "Replace_With_the_New_Subcription_Name"
  $RG5 = "TestRG5"
  $Location5 = "Japan East"
  $VnetName5 = "TestVNet5"
  $FESubName5 = "FrontEnd"
  $BESubName5 = "Backend"
  $GWSubName5 = "GatewaySubnet"
  $VnetPrefix51 = "10.51.0.0/16"
  $VnetPrefix52 = "10.52.0.0/16"
  $FESubPrefix5 = "10.51.0.0/24"
  $BESubPrefix5 = "10.52.0.0/24"
  $GWSubPrefix5 = "10.52.255.0/27"
  $GWName5 = "VNet5GW"
  $GWIPName5 = "VNet5GWIP"
  $GWIPconfName5 = "gwipconf5"
  $Connection51 = "VNet5toVNet1"
  ```
2. Ansluta toosubscription 5. Öppna PowerShell-konsolen och Anslut tooyour konto. Använd hello följande exempel toohelp du ansluta:

  ```powershell
  Login-AzureRmAccount
  ```

  Kontrollera hello prenumerationer för hello-kontot.

  ```powershell
  Get-AzureRmSubscription
  ```

  Ange hello prenumeration som du vill toouse.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub5
  ```
3. Skapa en ny resursgrupp.

  ```powershell
  New-AzureRmResourceGroup -Name $RG5 -Location $Location5
  ```
4. Skapa hello undernät för TestVNet5.

  ```powershell
  $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
  $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
  $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5
  ```
5. Skapa TestVNet5.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
  -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5
  ```
6. Begär en offentlig IP-adress.

  ```powershell
  $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
  -Location $Location5 -AllocationMethod Dynamic
  ```
7. Skapa hello gateway-konfigurationen.

  ```powershell
  $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
  $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
  $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5
  ```
8. Skapa hello TestVNet5 gateway.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
  -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-8---create-hello-connections"></a>Steg 8 – skapa hello-anslutningar

I det här exemplet eftersom hello gateways hello olika prenumerationer, har vi dela det här steget i två PowerShell-sessioner som markerats som [prenumeration 1] och [prenumeration 5].

1. **[Prenumeration 1]**  Get hello virtuell nätverksgateway för prenumerationen 1. Logga in och ansluta tooSubscription 1 innan du kör hello följande exempel:

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  ```

  Kopiera hello utdata från hello följande element och skicka dessa toohello administratör för prenumeration 5 via e-post eller någon annan metod.

  ```powershell
  $vnet1gw.Name
  $vnet1gw.Id
  ```

  Dessa två element har värden liknande toohello följande exempel på utdata:

  ```
  PS D:\> $vnet1gw.Name
  VNet1GW
  PS D:\> $vnet1gw.Id
  /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```
2. **[Prenumerationen 5]**  Get hello virtuell nätverksgateway för prenumerationen 5. Logga in och ansluta tooSubscription 5 innan du kör hello följande exempel:

  ```powershell
  $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5
  ```

  Kopiera hello utdata från hello följande element och skicka dessa toohello administratör för prenumeration 1 via e-post eller någon annan metod.

  ```powershell
  $vnet5gw.Name
  $vnet5gw.Id
  ```

  Dessa två element har värden liknande toohello följande exempel på utdata:

  ```
  PS C:\> $vnet5gw.Name
  VNet5GW
  PS C:\> $vnet5gw.Id
  /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```
3. **[Prenumeration 1]**  Skapa hello TestVNet1 tooTestVNet5 anslutning. I det här steget skapar du hello anslutning från TestVNet1 tooTestVNet5. hello skillnaden här är den $vnet5gw inte går att hämta direkt eftersom den tillhör en annan prenumeration. Du behöver toocreate ett nytt PowerShell-objekt med hello värden kommunicerat från prenumerationen 1 i hello stegen ovan. Använd hello exemplet nedan. Ersätt hello namn-Id och delad nyckel med egna värden. hello viktig sak är att hello delad nyckel måste matcha för både anslutningar. Skapa en anslutning kan ta en kort när toocomplete.

  Anslut tooSubscription 1 innan du kör hello följande exempel:

  ```powershell
  $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet5gw.Name = "VNet5GW"
  $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
  $Connection15 = "VNet1toVNet5"
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. **[Prenumerationen 5]**  Skapa hello TestVNet5 tooTestVNet1 anslutning. Det här steget är liknande toohello en ovan, förutom att du skapar hello anslutning från TestVNet5 tooTestVNet1. hello samma process för att skapa ett PowerShell-objekt baserat på hello värden som hämtas från prenumerationen 1 gäller också. I det här steget Glöm inte att matcha hello delade nycklar.

  Anslut tooSubscription 5 innan du kör hello följande exempel:

  ```powershell
  $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet1gw.Name = "VNet1GW"
  $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```

## <a name="verify"></a>Hur tooverify en anslutning

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="faq"></a>Vanliga frågor och svar om VNet-till-VNet

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Nästa steg

* När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Se hello [virtuella datorer dokumentationen](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) för mer information.
* Information om BGP finns hello [BGP översikt](vpn-gateway-bgp-overview.md) och [hur tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
