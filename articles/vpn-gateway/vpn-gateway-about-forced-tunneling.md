---
title: "Konfigurera Tvingad tunneltrafik för Azure plats-till-plats-anslutningar: klassiska | Microsoft Docs"
description: Hur tooredirect eller 'tvinga' alla Internet-bunden trafik tillbaka tooyour lokal plats.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 5c0177f1-540c-4474-9b80-f541fa44240b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 35b3a9ea370f9f962572629a69cc9aed16a87837
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-forced-tunneling-using-hello-classic-deployment-model"></a>Konfigurera Tvingad tunneling med hello klassiska distributionsmodellen

Tvingad tunneltrafik kan du omdirigera eller ”force” alla Internet-bunden trafik tillbaka tooyour lokal plats via en plats-till-plats VPN-tunnel för kontroll och granskning. Detta är en kritisk säkerhetskraven för de flesta företags IT principer. Utan Tvingad tunneling ska Internet-bunden trafik från din virtuella datorer i Azure alltid passerar från Azure nätverksinfrastruktur direkt ut toohello Internet, utan hello alternativet tooallow du tooinspect eller audit hello-trafik. Obehörig Internetåtkomst kan leda tooinformation avslöjas eller andra typer av säkerhetsintrång.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Den här artikeln beskriver hur du konfigurerar Tvingad tunneltrafik för virtuella nätverk som skapats med hello klassiska distributionsmodellen. Tvingad tunneltrafik kan konfigureras med hjälp av PowerShell inte via hello-portalen. Om du vill tooconfigure Tvingad tunneltrafik för hello Resource Manager-modellen, Välj klassiska artikel hello följande listrutan:

> [!div class="op_single_selector"]
> * [PowerShell – Klassisk](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell – Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a>Krav och överväganden
Tvingad tunneling i Azure konfigureras via virtuella nätverk användardefinierade vägar (UDR). Omdirigera trafik tooan lokal plats uttrycks som en standardväg toohello Azure VPN-gateway. hello innehåller följande avsnitt hello aktuella begränsning av hello routningstabellen och vägar för ett Azure Virtual Network:

* Varje undernät för virtuellt nätverk har en inbyggd, system-routningstabell. routningstabellen för hello system har hello följande tre grupper av vägar:

  * **Lokal VNet vägar:** direkt toohello mål virtuella datorer i hello samma virtuella nätverk.
  * **Lokala vägar:** toohello Azure VPN-gateway.
  * **Standardvägen:** direkt toohello Internet. Paket avsedda toohello privata IP-adresser som inte omfattas av hello föregående två vägar kommer att tas bort.
* Hello versionen av användardefinierade vägar kan du skapa en routning tabell tooadd en standardväg och sedan associerar hello routning tabell tooyour VNet adressundernät tooenable Tvingad tunneltrafik på dessa undernät.
* Du måste tooset en ”standardwebbplats” bland hello lokala mellan lokala platser anslutna toohello virtuellt nätverk.
* Tvingad tunneling måste vara kopplad till ett virtuellt nätverk som har en dynamisk routning VPN-gateway (inte en statisk gateway).
* ExpressRoute Tvingad tunneltrafik konfigureras inte via den här mekanismen men i stället har aktiverats genom att annonsera en standardväg via hello ExpressRoute BGP peeringsessioner. Se hello [ExpressRoute dokumentation](https://azure.microsoft.com/documentation/services/expressroute/) för mer information.

## <a name="configuration-overview"></a>Konfigurationsöversikt
I följande exempel hello, tunneldata hello klientdel undernät inte tvingas. hello arbetsbelastningar i hello klientdel undernät kan fortsätta tooaccept och svara toocustomer begäranden från hello Internet direkt. hello halva nivå och Backend-undernät tvingas tunnel. Alla utgående anslutningar från dessa två undernät toohello Internet blir framtvingad eller omdirigerade tillbaka tooan lokal plats via en hello S2S VPN-tunnlar.

Detta ger dig toorestrict och inspektera Internetåtkomst från dina virtuella datorer eller molntjänster i Azure, medan tooenable flernivåtjänst arkitekturen krävs. Du kan också använda Tvingad tunneltrafik toohello hela virtuella nätverk om det finns ingen Internet-riktade arbetsbelastningar i ditt virtuella nätverk.

![Forcerade tunnlar](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a>Innan du börjar
Kontrollera att du har följande objekt innan du börjar configuration hello.

* En Azure-prenumeration. Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).
* Konfigurerade virtuella nätverk. 
* hello senaste versionen av hello Azure PowerShell-cmdlets. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information om hur du installerar hello PowerShell-cmdlets.

## <a name="configure-forced-tunneling"></a>Konfigurera tvingad tunneltrafik
hello nedan hjälper dig att ange Tvingad tunneling för ett virtuellt nätverk. hello konfigurationssteg motsvarar toohello VNet nätverk konfigurationsfilen.

```
<VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>
```

I det här exemplet hello virtuella nätverket MultiTier-VNet har tre undernät: 'Klientdel', 'Midtier' och 'Backend-undernät med fyra mellan lokala anslutningar: 'DefaultSiteHQ' och tre filialer. 

hello steg ange hello 'DefaultSiteHQ' hello standard plats-anslutning för Tvingad tunneling och konfigurera hello Midtier och Backend-undernät toouse Tvingad tunneltrafik.

1. Skapa en routningstabell. Använd följande cmdlet toocreate hello din routningstabellen.

  ```powershell
  New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
  ```
2. Lägg till en standard väg toohello routningstabell. 

  hello följande exempel lägger till en standard väg toohello routningstabell skapade i steg 1. Observera att hello endast väg som stöds är hello målprefix för ”0.0.0.0/0” toohello ”VPNGateway” NextHop.

  ```powershell
  Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
  ```
3. Associera hello routning tabell toohello undernät. 

  När en routningstabell skapas och lägga till en väg, Använd följande exempel tooadd hello eller associera hello flödet tabell tooa VNet subnet. hello exempel läggs hello flödet tabell ”MyRouteTable” toohello Midtier och Backend-undernät i VNet MultiTier-VNet.

  ```powershell
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
  ```
4. Tilldela en standardplats för Tvingad tunneltrafik. 

  I föregående steg hello, hello cmdlet exempelskript skapas hello routningstabellen och associerade hello flödet tabell tootwo hello VNet-undernät. hello återstående steg är tooselect en lokal plats bland hello anslutningar på flera platser i hello virtuellt nätverk som hello standardwebbplatsen eller tunnel.

  ```powershell
  $DefaultSite = @("DefaultSiteHQ")
  Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
  ```

## <a name="additional-powershell-cmdlets"></a>Ytterligare PowerShell-cmdlets
### <a name="toodelete-a-route-table"></a>toodelete en routningstabell

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="toolist-a-route-table"></a>toolist en routningstabell

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="toodelete-a-route-from-a-route-table"></a>toodelete en väg från en routningstabell

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="tooremove-a-route-from-a-subnet"></a>tooremove en väg från ett undernät

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="toolist-hello-route-table-associated-with-a-subnet"></a>toolist hello vägtabell associerad med ett undernät

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="tooremove-a-default-site-from-a-vnet-vpn-gateway"></a>tooremove en standardwebbplats från ett VNet VPN-gateway

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```