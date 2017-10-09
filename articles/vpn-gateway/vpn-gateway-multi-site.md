---
title: "Ansluta ett virtuellt nätverk toomultiple platser med hjälp av VPN-Gateway och PowerShell: klassiska | Microsoft Docs"
description: "Den här artikeln vägleder dig genom att ansluta flera lokala lokala platser tooa virtuellt nätverk med en VPN-Gateway för hello klassiska distributionsmodellen."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-service-management
ms.assetid: b043df6e-f1e8-4a4d-8467-c06079e2c093
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: yushwang
ms.openlocfilehash: 5404b1c55ed3453b4dbc94dfd93e47c0812025f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-site-to-site-connection-tooa-vnet-with-an-existing-vpn-gateway-connection-classic"></a>Lägg till en plats-till-plats-anslutning tooa VNet med en befintlig VPN-gateway-anslutning (klassiskt)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klassisk)](vpn-gateway-multi-site.md)
>
>

Den här artikeln vägleder dig genom att använda PowerShell tooadd plats-till-plats (S2S) anslutningar tooa VPN-gateway som har en befintlig anslutning. Den här typen av anslutning är ofta hänvisas tooas ”flera” platskonfiguration. hello gäller stegen i den här artikeln toovirtual nätverk som skapats med hello klassiska distributionsmodellen (även kallat servicehantering). De här stegen gäller inte konfigurationer för samtidiga tooExpressRoute/plats-till-plats-anslutning.

### <a name="deployment-models-and-methods"></a>Distributionsmodeller och distributionsmetoder

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Vi uppdatera den här tabellen som nya artiklar och ytterligare verktyg blir tillgängliga för den här konfigurationen. När det finns en artikel länkar vi direkt tooit från den här tabellen.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a>Om hur du ansluter

Du kan ansluta flera lokala platser tooa enda virtuella nätverk. Detta är särskilt tilltalande för att skapa hybrid molnlösningar. Skapa en gateway med flera plats-anslutning tooyour virtuella Azure-nätverket är liknande toocreating andra plats-till-plats-anslutningar. Faktum är du kan använda en befintlig Azure VPN-gateway, så länge hello gateway är dynamisk (route-baserade).

Om du redan har ett virtuellt nätverk för statiska gateway-anslutna tooyour ändrar du hello gateway typen toodynamic utan att behöva toorebuild hello virtuellt nätverk i ordning tooaccommodate flera platser. Innan du ändrar hello routning, kontrollerar du att din lokala VPN-gateway stöder ruttbaserad VPN-konfigurationer.

![diagram över flera platser](./media/vpn-gateway-multi-site/multisite.png "flera platser")

## <a name="points-tooconsider"></a>Punkter tooconsider

**Du kommer inte att kunna toouse hello portal toomake ändringar toothis virtuellt nätverk.** Du måste konfigurationsfilen för toomake ändringar toohello nätverk istället för att använda hello-portalen. Om du gör ändringar i hello portal kommer de över flera platser referens inställningarna för det här virtuella nätverket.

Du bör känna sig fria använder hello nätverket konfigurationsfil med hello tid du har slutfört proceduren för hello flera platser. Om du har flera personer arbetar på din nätverkskonfiguration behöver toomake till alla känner om den här begränsningen. Det betyder att du inte kan använda hello portal alls. Du kan använda den för alla andra utom gör configuration ändringar toothis viss virtuellt nätverk.

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar konfigurera måste du kontrollera att du har hello följande:

* Kompatibel VPN maskinvara för var och en lokal plats. Kontrollera [om VPN-enheter för virtuell nätverksanslutning](vpn-gateway-about-vpn-devices.md) tooverify om hello-enhet som du vill toouse är något som kallas toobe som är kompatibla.
* Ett externt Internetriktade offentliga IPv4-adress för varje VPN-enhet. hello IP-adress kan inte finnas bakom en NAT Detta är ett krav.
* Du behöver tooinstall hello senaste versionen av hello Azure PowerShell-cmdlets. Se till att installera hello Service Management (SM) version i tillägg toohello Resource Manager-version. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information.
* Någon som skicklig på Konfigurera VPN-maskinvara. Du har toohave en stark förståelse av hur tooconfigure din VPN-enhet eller arbeta med någon som.
* hello IP-adressintervall som du vill toouse för ditt virtuella nätverk (om du inte redan har skapat en).
* hello IP-adressintervall för varje hello lokala nätverksplatser som du ska kunna ansluta till. Du behöver toomake till att hello IP-adressintervall för varje hello lokala nätverksplatser som du vill tooconnect toodo inte överlappar varandra. Annars avvisar hello-portalen eller hello REST API hello-konfiguration som överförs.<br>Till exempel om du har två lokala nätverksplatser som innehåller båda hello IP-adressintervallet 10.2.3.0/24 och du har ett paket med en måladress 10.2.3.3 vet Azure inte vilken plats som du vill toosend hello paketet toobecause hello-adressintervall är överlappande. tooprevent routning problem, Azure tillåter inte att du tooupload en konfigurationsfil som innehåller överlappande intervall.

## <a name="1-create-a-site-to-site-vpn"></a>1. Skapa en VPN för plats-till-plats
Om du redan har en plats-till-plats VPN-anslutning med en dynamisk routning gateway utmärkt! Du kan fortsätta för[exportera konfigurationsinställningar virtuella nätverk för hello](#export). Om inte, hello följande:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Om du redan har ett virtuellt nätverk för plats-till-plats, men den har en statisk routning (principbaserad)-gateway.
1. Ändra din gateway typen toodynamic routning. En VPN-anslutning för flera platser kräver en dynamisk routning (även kallat ruttbaserad)-gateway. Ange om toochange din gateway, du får måste toofirst delete hello befintlig gateway och sedan skapa en ny. Instruktioner finns i [hur toochange hello routning VPN-typ för din gateway](vpn-gateway-configure-vpn-gateway-mp.md).  
2. Konfigurera din nya gateway och skapa VPN-tunnel. Instruktioner finns i [konfigurera en VPN-Gateway i hello Azure klassiska Portal](vpn-gateway-configure-vpn-gateway-mp.md). Ändra din gateway typen toodynamic routning.

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Om du inte har ett virtuellt nätverk för plats-till-plats:
1. Skapa det virtuella nätverket på plats-till-plats använder dessa instruktioner: [skapa ett virtuellt nätverk med en plats-till-plats VPN-anslutning i hello Azure klassiska Portal](vpn-gateway-site-to-site-create.md).  
2. Konfigurera en dynamisk routning gateway som använder dessa instruktioner: [konfigurera en VPN-Gateway](vpn-gateway-configure-vpn-gateway-mp.md). Vara säker på att tooselect **dynamisk routning** för gateway-typen.

## <a name="export"></a>2. Exportera konfigurationsfilen för hello nätverk
Exportera konfigurationsfilen Azure-nätverk genom att köra följande kommando hello. Du kan ändra hello platsen för hello tooexport tooa annan plats om det behövs.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

## <a name="3-open-hello-network-configuration-file"></a>3. Öppna hello konfigurationsfilen för nätverk
Öppna konfigurationsfilen för hello nätverk som du hämtade i hello sista steget. Använda en xml-redigerare som du vill använda. hello filen bör se ut ungefär toohello följande:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. Lägga till flera hänvisningar
När du lägger till eller ta bort referensen platsinformation du konfigurationsändringar toohello ConnectionsToLocalNetwork/LocalNetworkSiteRef. Lägga till en ny lokal plats referens utlöser Azure toocreate en ny tunnel. I hello exemplet nedan är hello nätverkskonfigurationen för en enskild plats-anslutning. Spara hello-filen när du är klar med ändringarna.

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

tooadd ytterligare referenser (skapa en konfiguration för flera platser), bara lägga till ytterligare ”LocalNetworkSiteRef” rader som visas i hello exemplet nedan:

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
      <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

## <a name="5-import-hello-network-configuration-file"></a>5. Importera konfigurationsfilen för hello nätverk
Importera konfigurationsfilen för hello nätverk. När du importerar filen med hello ändringar läggs hello nya tunnlar. hello tunnlar använder dynamiska hello-gateway som du skapade tidigare. Du kan antingen använda hello klassiska portalen eller PowerShell tooimport hello-fil.

## <a name="6-download-keys"></a>6. Hämta nycklar
När din nya tunnlar har lagts till, kan du använda hello PowerShell cmdlet 'Get-AzureVNetGatewayKey' tooget hello IPsec/IKE i förväg delade nycklar för varje tunnel.

Exempel:

```powershell
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"
```

Om du vill kan du också använda hello *hämta virtuella nätverk Gateway delad nyckel* REST API tooget hello i förväg delade nycklar.

## <a name="7-verify-your-connections"></a>7. Verifiera dina anslutningar
Kontrollera statusen för hello flera plats-tunneln. När du har hämtat hello nycklarna för varje tunnel, ska du tooverify anslutningar. Använd 'Get-AzureVnetConnection' tooget en lista över virtuella nätverk tunnlar som visas i hello exemplet nedan. VNet1 är hello namn hello virtuella nätverk.

```powershell
Get-AzureVnetConnection -VNetName VNET1
```

Exempel returnerar:

```
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : hello connectivity state for hello local network site 'Site1' changed from Not Connected tooConnected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : hello connectivity state for hello local network site 'Site2' changed from Not Connected tooConnected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
```

## <a name="next-steps"></a>Nästa steg

toolearn mer information om VPN-gatewayer finns [om VPN-gatewayer](vpn-gateway-about-vpngateways.md).
