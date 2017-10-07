---
title: "Konfigurera ExpressRoute-anslutningar och VPN-anslutningar för plats-till-plats som kan samexistera: Azure (klassisk) | Microsoft Docs"
description: "Den här artikeln beskriver hur du konfigurerar ExpressRoute- och en plats-till-plats VPN-anslutning som kan finnas för hello klassiska distributionsmodellen."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: dcf1a5af-a289-466a-b812-0bfedbd2bda0
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: abb30fff55e8ec243f2920c5b2f70c43717755fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections-classic"></a>Konfigurera ExpressRoute-anslutningar och VPN-anslutningar för plats-till-plats som kan samexistera (klassisk)
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-howto-coexist-resource-manager.md)
> * [PowerShell – Klassisk](expressroute-howto-coexist-classic.md)
> 
> 

Med hello möjlighet tooconfigure plats-till-plats VPN- och ExpressRoute har flera fördelar. Du kan konfigurera VPN för plats-till-plats som en säker redundans sökväg för ExressRoute eller använda plats-till-plats VPN tooconnect toosites som inte är anslutna via ExpressRoute. Vi tar upp hello steg tooconfigure båda scenarierna i den här artikeln. Den här artikeln gäller toohello klassiska distributionsmodellen. Den här konfigurationen är inte tillgänglig i hello-portalen.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Om distributionsmodeller för Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

> [!IMPORTANT]
> ExpressRoute-kretsar måste konfigureras före innan du följer hello anvisningarna nedan. Kontrollera att du har följt hello guider för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-classic.md) och [konfigurera routning](expressroute-howto-routing-classic.md) innan du följer hello stegen nedan.
> 
> 

## <a name="limits-and-limitations"></a>Gränser och begränsningar
* **Transit-routing stöds inte.** Du kan inte routa (via Azure) mellan ditt lokala nätverk som ansluter via plats-till-plats-VPN och ditt lokala nätverk som ansluter via ExpressRoute.
* **Punkt-till-plats stöds inte.** Du kan inte aktivera punkt-till-plats VPN-anslutningar toohello samma virtuella nätverk som är anslutna tooExpressRoute. Punkt-till-plats VPN- och ExpressRoute kan inte samexistera för hello samma virtuella nätverk.
* **Tvingad tunneling kan inte aktiveras på hello plats-till-plats VPN-gateway.** Du kan bara ”framtvinga” alla Internet-bunden trafik tillbaka tooyour lokalt nätverk via ExpressRoute.
* **Basic-SKU-gatewayen stöds inte.** Du måste använda en icke - grundläggande SKU-gateway för båda hello [ExpressRoute-gateway](expressroute-about-virtual-network-gateways.md) och hello [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* **Enbart routebaserad VPN-gateway stöds.** Du måste använda en routebaserad [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* **Statiska vägar ska konfigureras för din VPN-gateway.** Om nätverket är anslutet tooboth ExpressRoute och en plats-till-plats VPN, måste du ha en statisk väg som konfigurerats i ditt lokala nätverk tooroute hello plats-till-plats VPN-anslutningen toohello offentliga Internet.
* **ExpressRoute-gatewayen måste konfigureras först.** Du måste först skapa hello ExpressRoute-gateway innan du lägger till hello plats-till-plats VPN-gateway.

## <a name="configuration-designs"></a>Konfigurationsdesign
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Konfigurera en VPN för plats till plats som en redundanssökväg för ExpressRoute
Du kan konfigurera en VPN-anslutning för plats till plats som en säkerhetskopia av ExpressRoute. Detta gäller endast toovirtual nätverk länkade toohello Azure privat peering sökväg. Det finns ingen VPN-baserad redundanslösning för tjänster som är tillgängliga via Azures offentliga och Microsofts peerings. Hej ExpressRoute-kretsen är alltid hello primära länken. Data flödar genom hello plats-till-plats VPN-väg endast om hello ExpressRoute-krets misslyckades. 

> [!NOTE]
> ExpressRoute-kretsen är prioriterade via plats-till-plats VPN när båda vägar är hello samma, använder Azure hello längsta prefix matchar toochoose hello vägen mot hello paketets mål.
> 
> 

![Samexistera](media/expressroute-howto-coexist-classic/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-tooconnect-toosites-not-connected-through-expressroute"></a>Konfigurera en plats-till-plats VPN-tooconnect toosites som inte är anslutna via ExpressRoute
Du kan konfigurera vissa platser ansluter direkt tooAzure via plats-till-plats VPN och vissa platser ansluter via ExpressRoute. 

![Samexistera](media/expressroute-howto-coexist-classic/scenario2.jpg)

> [!NOTE]
> Det går inte att konfigurera ett virtuellt nätverk som en överföringsrouter.
> 
> 

## <a name="selecting-hello-steps-toouse"></a>Att välja hello steg toouse
Det finns två olika uppsättningar av procedurer toochoose från i ordning tooconfigure anslutningar som kan finnas. hello configuration procedur som du väljer beror på om du har ett befintligt virtuellt nätverk som du vill tooconnect till, eller önskade toocreate ett nytt virtuellt nätverk.

* Jag inte har ett VNet och behöver toocreate en.
  
    Om du inte redan har ett virtuellt nätverk, får den här proceduren du att skapa ett nytt virtuellt nätverk med hjälp av hello klassiska distributionsmodellen och skapa nya ExpressRoute- och plats-till-plats VPN-anslutningar. tooconfigure, följ hello instruktionerna i artikeln hello [toocreate ett nytt virtuellt nätverk och samtidiga anslutningar](#new).
* Jag har redan en klassisk distributionsmodell för VNet.
  
    Du kanske redan har ett virtuellt nätverk på plats med en befintlig VPN-anslutning för plats till plats eller en ExpressRoute-anslutning. Hej artikel avsnittet [tooconfigure coexsiting anslutningar för ett redan existerande VNet](#add) beskriver hur du tar bort hello gateway och sedan skapa nya ExpressRoute och plats-till-plats VPN-anslutningar. Observera att när du skapar hello nya anslutningar, hello steg måste slutföras i en särskild ordning. Använd inte hello instruktionerna i andra artiklar toocreate din gateway och anslutningar.
  
    I den här proceduren skapar anslutningar som kan finnas kräver du toodelete din gateway, och sedan konfigurera ny gateway. Detta innebär att du har driftstopp för anslutningar mellan platser när du tar bort och återskapa din gateway och anslutningar, men du behöver inte toomigrate någon av dina virtuella datorer eller tjänster tooa nytt virtuellt nätverk. Dina virtuella datorer och tjänster kommer fortfarande att kunna toocommunicate ut via hello belastningsutjämnare när du konfigurerar din gateway om de är konfigurerade toodo så.

## <a name="new"></a>toocreate ett nytt virtuellt nätverk och samtidiga anslutningar
Den här proceduren vägleder dig genom att skapa ett VNet samt plats-till-plats- och ExpressRoute-anslutningar som ska finnas samtidigt.

1. Du behöver tooinstall hello senaste versionen av hello Azure PowerShell-cmdlets. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information om hur du installerar hello PowerShell-cmdlets. Observera att hello-cmdlets som du vill använda för den här konfigurationen kan vara något annorlunda än vad du kan känna till. Anges till toouse hello-cmdlets i dessa instruktioner. 
2. Skapa ett schema för det virtuella nätverket. Läs mer om hello Konfigurationsschemat [Azure Virtual Network Konfigurationsschemat](https://msdn.microsoft.com/library/azure/jj157100.aspx).
   
    När du skapar schemat måste du kontrollera att du använder hello följande värden:
   
   * hello gateway-undernät för virtuellt nätverk för hello måste vara minst/27 eller ett kortare prefix (till exempel /26 eller /25).
   * anslutningstypen för hello gateway dedikerad ””.
     
             <VirtualNetworkSite name="MyAzureVNET" Location="Central US">
               <AddressSpace>
                 <AddressPrefix>10.17.159.192/26</AddressPrefix>
               </AddressSpace>
               <Subnets>
                 <Subnet name="Subnet-1">
                   <AddressPrefix>10.17.159.192/27</AddressPrefix>
                 </Subnet>
                 <Subnet name="GatewaySubnet">
                   <AddressPrefix>10.17.159.224/27</AddressPrefix>
                 </Subnet>
               </Subnets>
               <Gateway>
                 <ConnectionsToLocalNetwork>
                   <LocalNetworkSiteRef name="MyLocalNetwork">
                     <Connection type="Dedicated" />
                   </LocalNetworkSiteRef>
                 </ConnectionsToLocalNetwork>
               </Gateway>
             </VirtualNetworkSite>
3. När du skapar och konfigurerar xml-schemafil, överför du hello-fil. Detta skapar det virtuella nätverket.
   
    Använd följande cmdlet tooupload hello din fil ersätter hello värdet med dina egna.
   
        Set-AzureVNetConfig -ConfigurationPath 'C:\NetworkConfig.xml'
4. <a name="gw"></a>Skapa en ExpressRoute-gateway. Vara säker på att toospecify hello GatewaySKU som *Standard*, *HighPerformance*, eller *UltraPerformance* och hello GatewayType som *DynamicRouting*.
   
    Använd hello följande exempel, ersätter hello värden för din egen.
   
        New-AzureVNetGateway -VNetName MyAzureVNET -GatewayType DynamicRouting -GatewaySKU HighPerformance
5. Länka hello ExpressRoute-gateway toohello ExpressRoute-kretsen. Det här steget har slutförts, upprättas hello anslutning mellan ditt lokala nätverk och Azure via ExpressRoute.
   
        New-AzureDedicatedCircuitLink -ServiceKey <service-key> -VNetName MyAzureVNET
6. <a name="vpngw"></a>Därefter skapar du din plats-till-plats-VPN-gateway. Hej GatewaySKU måste vara *Standard*, *HighPerformance*, eller *UltraPerformance* och hello GatewayType måste vara *DynamicRouting*.
   
        New-AzureVirtualNetworkGateway -VNetName MyAzureVNET -GatewayName S2SVPN -GatewayType DynamicRouting -GatewaySKU  HighPerformance
   
    tooretrieve hello gatewayinställningarna för virtuella nätverk, inklusive hello gateway-ID och hello offentlig IP, använda hello `Get-AzureVirtualNetworkGateway` cmdlet.
   
        Get-AzureVirtualNetworkGateway
   
        GatewayId            : 348ae011-ffa9-4add-b530-7cb30010565e
        GatewayName          : S2SVPN
        LastEventData        :
        GatewayType          : DynamicRouting
        LastEventTimeStamp   : 5/29/2015 4:41:41 PM
        LastEventMessage     : Successfully created a gateway for hello following virtual network: GNSDesMoines
        LastEventID          : 23002
        State                : Provisioned
        VIPAddress           : 104.43.x.y
        DefaultSite          :
        GatewaySKU           : HighPerformance
        Location             :
        VnetId               : 979aabcf-e47f-4136-ab9b-b4780c1e1bd5
        SubnetId             :
        EnableBgp            : False
        OperationDescription : Get-AzureVirtualNetworkGateway
        OperationId          : 42773656-85e1-a6b6-8705-35473f1e6f6a
        OperationStatus      : Succeeded
7. Skapa en lokal plats för VPN-gatewayen. Det här kommandot konfigurerar inte din lokala VPN-gateway. I stället kan du tooprovide hello lokala gateway-inställningar, till exempel hello offentlig IP-adress och hello lokala adressutrymmet, så att hello Azure VPN-gateway kan ansluta tooit.
   
   > [!IMPORTANT]
   > hello lokal plats för hello plats-till-plats VPN har inte definierats i hello netcfg. I stället måste du använda den här cmdlet toospecify hello lokal plats parametrar. Du kan inte definiera den med hjälp av portalen eller hello netcfg filen.
   > 
   > 
   
    Använd hello följande exempel, ersätter hello värden med dina egna.
   
        New-AzureLocalNetworkGateway -GatewayName MyLocalNetwork -IpAddress <MyLocalGatewayIp> -AddressSpace <MyLocalNetworkAddress>
   
   > [!NOTE]
   > Om det lokala nätverket har flera routningar kan du ange dem i en matris.  $MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")  
   > 
   > 

    tooretrieve hello gatewayinställningarna för virtuella nätverk, inklusive hello gateway-ID och hello offentlig IP, använda hello `Get-AzureVirtualNetworkGateway` cmdlet. Se följande exempel hello.

        Get-AzureLocalNetworkGateway

        GatewayId            : 532cb428-8c8c-4596-9a4f-7ae3a9fcd01b
        GatewayName          : MyLocalNetwork
        IpAddress            : 23.39.x.y
        AddressSpace         : {10.1.2.0/24}
        OperationDescription : Get-AzureLocalNetworkGateway
        OperationId          : ddc4bfae-502c-adc7-bd7d-1efbc00b3fe5
        OperationStatus      : Succeeded


1. Konfigurera din lokala VPN-enhet tooconnect toohello ny gateway. Använd hello information som du hämtade i steg 6 när du konfigurerar VPN-enhet. Mer information om VPN-enhetskonfiguration finns i [VPN-enhetskonfiguration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).
2. Länken hello plats-till-plats VPN-gateway på Azure toohello lokala gateway.
   
    I det här exemplet är connectedEntityId hello lokala gateway-ID som du hittar genom att köra `Get-AzureLocalNetworkGateway`. Du kan hitta virtualNetworkGatewayId med hello `Get-AzureVirtualNetworkGateway` cmdlet. Efter det här steget upprättas hello anslutning mellan ditt lokala nätverk och Azure via hello plats-till-plats VPN-anslutning.

        New-AzureVirtualNetworkGatewayConnection -connectedEntityId <local-network-gateway-id> -gatewayConnectionName Azure2Local -gatewayConnectionType IPsec -sharedKey abc123 -virtualNetworkGatewayId <azure-s2s-vpn-gateway-id>

## <a name="add"></a>tooconfigure coexsiting anslutningar för ett befintligt virtuellt nätverk
Om du har ett befintligt virtuellt nätverk kontrollerar du hello gateway undernätets storlek. Om hello gateway-undernät är /28 eller /29, måste du först ta bort hello virtuell nätverksgateway och öka storleken på hello gateway-undernät. hello stegen i det här avsnittet visar hur toodo som.

Om hello gateway-undernät är minst/27 eller större och hello virtuellt nätverk är anslutna via ExpressRoute kan du hoppa över hello stegen nedan och fortsätta för[”steg 6 – skapa en plats-till-plats VPN-gateway”](#vpngw) hello föregående avsnitt.

> [!NOTE]
> När du tar bort hello befintlig gateway förlorar din lokala lokala hello anslutning tooyour virtuellt nätverk när du arbetar med den här konfigurationen.
> 
> 

1. Du behöver tooinstall hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information om hur du installerar hello PowerShell-cmdlets. Observera att hello-cmdlets som du vill använda för den här konfigurationen kan vara något annorlunda än vad du kan känna till. Anges till toouse hello-cmdlets i dessa instruktioner. 
2. Ta bort hello befintlig ExpressRoute eller plats-till-plats VPN-gateway. Använd hello följande cmdlet, ersätter hello värden med dina egna.
   
        Remove-AzureVNetGateway –VnetName MyAzureVNET
3. Exportera hello virtuellt nätverk schema. Använd hello följande PowerShell-cmdleten, ersätter hello värden med dina egna.
   
        Get-AzureVNetConfig –ExportToFile “C:\NetworkConfig.xml”
4. Redigera schemat för hello nätverket konfigurationsfilen så att hello gateway-undernät är minst/27 eller ett kortare prefix (till exempel /26 eller /25). Se följande exempel hello. 
   
   > [!NOTE]
   > Om du inte har tillräckligt med IP-adresser som är kvar i ditt virtuella nätverk tooincrease hello gateway undernätets storlek, måste tooadd flera IP-adressutrymme. Läs mer om hello Konfigurationsschemat [Azure Virtual Network Konfigurationsschemat](https://msdn.microsoft.com/library/azure/jj157100.aspx).
   > 
   > 
   
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.17.159.224/27</AddressPrefix>
          </Subnet>
5. Om din gateway som tidigare var en plats-till-plats-VPN, måste du också ändra hello anslutningstypen för**dedikerad**.
   
                 <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
6. Just nu har du ett VNet utan gateways. toocreate nya gatewayer och slutföra dina anslutningar, du kan fortsätta med [steg 4: skapa en ExpressRoute-gateway](#gw)hittades i hello föregående steg.

## <a name="next-steps"></a>Nästa steg
Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md)

