---
title: "Ansluta klassiska virtuella nätverk tooAzure Resource Manager VNets: PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en VPN-anslutning mellan klassiska Vnet och Resource Manager VNets med VPN-Gateway och PowerShell"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: f17c3bf0-5cc9-4629-9928-1b72d0c9340b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
ms.openlocfilehash: 8b1cf6ae4becf1829fa99961c5dd09a422fcc1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Anslut virtuella nätverk från olika distributionsmodeller med hjälp av PowerShell



Den här artikeln visar hur tooconnect klassiska Vnet tooResource Manager VNets tooallow hello resurser i hello separat distribution modeller toocommunicate med varandra. hello stegen i den här artikeln använder PowerShell, men du kan också skapa den här konfigurationen med hjälp av hello Azure-portalen genom att välja hello artikel från den här listan.

> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Ansluta ett klassiskt virtuellt nätverk tooa Resource Manager VNet är liknande tooconnecting ett VNet tooan lokal plats. Båda typerna av anslutningen använder en VPN-gateway tooprovide en säker tunnel med IPsec/IKE. Du kan skapa en anslutning mellan Vnet som finns i olika prenumerationer och i olika regioner. Du kan också ansluta Vnet som redan har anslutningar tooon lokala nätverk, så länge hello-gateway som de har konfigurerats med är dynamisk eller ruttbaserad. Mer information om VNet-till-VNet-anslutningar finns hello [VNet-till-VNet vanliga frågor och svar](#faq) hello slutet av den här artikeln. 

Om ditt Vnet i hello samma region som du kanske vill tooinstead bör överväga att ansluta dem med hjälp av VNet-Peering. Ingen VPN-gateway används för VNet-peering. Mer information finns i [VNet peering (Vnet-peering)](../virtual-network/virtual-network-peering-overview.md). 

## <a name="before-beginning"></a>Innan du börjar

hello följande steg vägleder dig genom hello inställningar nödvändiga tooconfigure en dynamisk eller ruttbaserad gateway för varje VNet och skapa en VPN-anslutning mellan hello-gateway. Den här konfigurationen stöder inte statisk eller principbaserad gateways.

### <a name="prerequisites"></a>Krav

* Båda Vnet har redan skapats.
* hello-adressintervall för hello Vnet inte överlappar varandra eller överlappar med någon av hello-intervall för andra anslutningar hello gateways kan anslutas till.
* Du har installerat hello senaste PowerShell-cmdlets. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information. Kontrollera att du installerar både hello Service Management (SM) och hello Resource Manager (RM) cmdlets. 

### <a name="exampleref"></a>Exempelinställningar

Du kan använda dessa värden toocreate en testmiljö eller referera toothem toobetter förstå hello exemplen i den här artikeln.

**Klassiska VNet-inställningarna**

VNet-Name = ClassicVNet <br>
Plats = västra USA <br>
Virtuellt nätverk adressutrymmen = 10.0.0.0/24 <br>
Undernät-1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Lokala nätverksnamn = RMVNetLocal <br>
GatewayType = DynamicRouting

**Hanteraren för filserverresurser VNet-inställningarna**

VNet-Name = RMVNet <br>
Resursgruppens namn = RG1 <br>
Virtuellt IP-adressutrymmen = 192.168.0.0/16 <br>
Undernät-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Plats = östra USA <br>
Gateway offentliga IP-namn = gwpip <br>
Lokal nätverksgateway = ClassicVNetLocal <br>
Virtuell nätverksgateway name = RMGateway <br>
Gateway-IP-adressering konfigurationen = gwipconfig

## <a name="createsmgw"></a>Avsnittet 1 – konfigurera hello klassiska virtuella nätverk
### <a name="part-1---download-your-network-configuration-file"></a>Del 1: hämta ditt nätverks-konfigurationsfil
1. Logga in tooyour Azure-konto i hello PowerShell-konsol med utökade behörigheter. hello efterfrågar följande cmdlet hello inloggningsuppgifterna för Azure-konto. När du loggar in hämtar den inställningarna för ditt konto, så att de är tillgängliga tooAzure PowerShell. Du använder hello SM PowerShell-cmdlets toocomplete den här delen av hello konfiguration.

  ```powershell
  Add-AzureAccount
  ```
2. Exportera konfigurationsfilen Azure-nätverk genom att köra följande kommando hello. Du kan ändra hello platsen för hello tooexport tooa annan plats om det behövs.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
3. Öppna hello XML-fil som du hämtade tooedit den. Ett exempel på hello nätverk-konfigurationsfilen finns hello [nätverk Konfigurationsschemat](https://msdn.microsoft.com/library/jj157100.aspx).

### <a name="part-2--verify-hello-gateway-subnet"></a>Del 2 – verifiera hello gateway-undernät
I hello **VirtualNetworkSites** element, lägga till en gateway-undernätet tooyour VNet om något inte redan har skapats. När du arbetar med hello nätverket konfigurationsfilen hello gateway-undernätet måste ha namnet ”GatewaySubnet” eller Azure kan inte identifiera och använda den som en gateway-undernätet.

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Exempel:**

    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>

### <a name="part-3---add-hello-local-network-site"></a>Del 3 – Lägg till hello lokal nätverksplats
hello lokal nätverksplats du lägger till representerar hello RM VNet toowhich som du vill tooconnect. Lägg till en **LocalNetworkSites** elementet toohello-filen om det inte redan finns. Nu i hello konfiguration kan hello VPNGatewayAddress vara någon giltig offentlig IP-adress eftersom vi inte har skapat hello-gateway för hello Resource Manager VNet. När vi har skapat hello gateway ersätta vi platshållare IP-adressen med hello korrekt offentlig IP-adress som har tilldelats toohello RM gateway.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-hello-vnet-with-hello-local-network-site"></a>En del 4 - Koppla hello VNet med hello lokal nätverksplats
I det här avsnittet anger vi hello lokal nätverksplats som du vill tooconnect hello VNet till. I det här fallet är det hello Resource Manager-VNet som refererar till tidigare. Se till att hello namn matchar. Det här steget kan inte skapa en gateway. Det anger hello lokala nätverket som hello gateway ansluter till.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-hello-file-and-upload"></a>En del 5 – spara hello-fil och överför
Spara hello-fil och sedan importera tooAzure genom att köra följande kommando hello. Kontrollera att du ändrar hello filsökväg som behövs för din miljö.

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

Du ser ett liknande resultat som visar att hello importen lyckades.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-hello-gateway"></a>Del 6 – skapa hello gateway

Innan du kör det här exemplet finns toohello nätverk konfigurationsfil som du hämtade för hello exakt namn som Azure förväntar sig toosee. konfigurationsfilen för hello nätverk innehåller hello värden för din klassiska virtuella nätverk. Ibland hello hello namn för klassiska Vnet har ändrats i konfigurationsfilen för hello nätverk när du skapar klassiska VNet-inställningarna i Azure-portalen på grund av toohello skillnader i hello distributionsmodeller. Till exempel, om du har använt hello Azure portal toocreate klassiska VNet med namnet 'klassiska virtuella nätverk, och skapas i en resursgrupp med namnet 'ClassicRG' hello som finns i konfigurationsfilen för hello nätverk är konverterade too'Group ClassicRG klassiska virtuella nätverk ”. När du anger hello namnet på ett virtuellt nätverk som innehåller blanksteg, citattecken hello värde.


Använd följande exempel toocreate en dynamisk routning gateway hello:

```powershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

Du kan kontrollera hello statusen för hello gateway med hjälp av hello **Get-AzureVNetGateway** cmdlet.

## <a name="creatermgw"></a>Avsnitt 2: Konfigurera hello RM VNet-gateway
toocreate en VPN-gateway för hello RM VNet, följ hello följa anvisningar. Starta inte hello steg tills när du har hämtat hello offentliga IP-adressen för gateway hello klassiska VNet. 

1. Logga in tooyour Azure-konto i hello PowerShell-konsolen. hello efterfrågar följande cmdlet hello inloggningsuppgifterna för Azure-konto. När du loggar in laddas inställningarna för ditt konto så att de är tillgängliga tooAzure PowerShell.

  ```powershell
  Login-AzureRmAccount
  ``` 
   
  Hämta en lista över dina Azure-prenumerationer om du har mer än en prenumeration.

  ```powershell
  Get-AzureRmSubscription
  ```
   
  Ange hello prenumeration som du vill toouse.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. Skapa en lokal nätverksgateway. I ett virtuellt nätverk refererar hello lokal nätverksgateway vanligtvis tooyour lokal plats. I det här fallet refererar hello lokal nätverksgateway tooyour klassiska virtuella nätverk. Ge det ett namn som Azure kan se tooit och även ange hello adressprefixet utrymme. Azure namnområdesprefixet hello IP-adress du anger tooidentify vilken trafik toosend tooyour lokal plats. Om du behöver senare tooadjust hello här information innan du skapar din gateway kan du ändra hello värden och kör hello exempel igen.
   
   **-Namnet** är hello-namn som du vill tooassign toorefer toohello lokal nätverksgateway.<br>
   **-AddressPrefix** är hello adressutrymmet för ditt klassiska VNet.<br>
   **-GatewayIpAddress** är hello offentliga IP-adressen för gateway hello klassiska VNet. Glöm toochange hello följande exempel tooreflect hello rätt IP-adress.<br>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
  -Location "West US" -AddressPrefix "10.0.0.0/24" `
  -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
  ```
3. Begär en offentlig IP-adress toobe allokerade toohello virtuell nätverksgateway för hello Resource Manager VNet. Du kan inte ange hello IP-adress som du vill toouse. hello IP-adressen tilldelas dynamiskt toohello virtuell nätverksgateway. Detta innebär dock inte hello IP-adress ändras. hello endast tid hello virtuellt gateway IP-adressändringarna är när hello gateway bort och återskapas. Ändringen inte över storleksändring, återställa eller andra internt Underhåll/uppgraderingar av hello gateway.

  I det här steget ska ange vi också en variabel som används i ett senare steg.

  ```powershell
  $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
  -ResourceGroupName RG1 -Location 'EastUS' `
  -AllocationMethod Dynamic
  ```

4. Kontrollera att det virtuella nätverket har en gateway-undernätet. Om det finns ingen gateway-undernät, kan du lägga till en. Kontrollera att hello gateway-undernätet har namnet *GatewaySubnet*.
5. Hämta hello undernät som används för hello gateway genom att köra följande kommando hello. I det här steget kan ange vi också en variabel toobe som används i hello nästa steg.
   
   **-Namnet** är hello namnet på ditt VNet Resource Manager.<br>
   **-ResourceGroupName** är hello-resursgrupp som hello virtuella nätverk som är associerad med. hello gateway-undernätet måste finnas för detta virtuella nätverk och måste ha namnet *GatewaySubnet* toowork korrekt.<br>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
  -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
  ``` 

6. Skapa hello gateway IP-adressering konfiguration. gateway-konfiguration för hello definierar hello undernät och hello offentliga IP-adressen toouse. Använd följande exempel toocreate hello gateway-konfiguration.

  I det här steget hello **- SubnetId** och **- PublicIpAddressId** parametrar måste överföras hello id-egenskapen från hello undernät och IP-adress-objekt, respektive. Du kan inte använda en sträng. Dessa variabler är inställda i hello steg toorequest en offentlig IP-adress och hello steg tooretrieve hello undernät.

  ```powershell
  $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
  -Name gwipconfig -SubnetId $subnet.id `
  -PublicIpAddressId $ipaddress.id
  ```
7. Skapa hello Resource Manager virtuella nätverks-gatewayen genom att köra följande kommando hello. Hej `-VpnType` måste vara *RouteBased*. Det kan ta 45 minuter eller mer för hello gateway toocreate.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
  -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
  -IpConfigurations $gwipconfig `
  -EnableBgp $false -VpnType RouteBased
  ```
8. Kopiera hello offentlig IP-adress när hello VPN-gateway har skapats. Du kan använda den för när du konfigurerar hello lokala nätverksinställningar för din klassiska VNet. Du kan använda hello följande cmdlet tooretrieve hello offentliga IP-adress. hello offentliga IP-adressen listas i hello returnerade som *IpAddress*.

  ```powershell
  Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1
  ```

## <a name="section-3-modify-hello-classic-vnet-local-site-settings"></a>Avsnitt 3: Ändra hello klassiska VNet lokala platsinställningar

I det här avsnittet kan du arbeta med hello klassiska virtuella nätverk. Du kan ersätta hello platshållare IP-adress som du använde när du anger hello lokala inställningar som kommer att använda tooconnect toohello Resource Manager VNet gateway. 

1. Exportera konfigurationsfilen för hello nätverk.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. Använd en textredigerare och ändra hello värdet för VPNGatewayAddress. Ersätt hello platshållare IP-adress med hello offentliga IP-adressen för hello Resource Manager-gateway och spara hello ändringar.

  ```
  <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
  ```
3. Importera hello ändrade network configuration file tooAzure.

  ```powershell
  Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
  ```

## <a name="connect"></a>Avsnitt 4: Skapa en anslutning mellan hello gateways
Skapa en anslutning mellan hello gateway kräver PowerShell. Du kanske måste tooadd din Azure-konto toouse hello klassiska versionen av hello PowerShell-cmdlets. så, Använd toodo **Add-AzureAccount**.

1. Ange den delade nyckeln i hello PowerShell-konsolen. Innan du kör hello cmdlets finns toohello nätverk konfigurationsfil som du hämtade för hello exakt namn som Azure förväntar sig toosee. När du anger hello namnet på ett virtuellt nätverk som innehåller blanksteg, Använd enkla citattecken runt hello värde.<br><br>I följande exempel **- VNetName** är hello namnet på hello klassiska virtuella nätverk och **- LocalNetworkSiteName** är hello namn du angett för hello lokal nätverksplats. Hej **- SharedKey** är ett värde som du skapar och ange. I exemplet hello vi använde 'abc123', men du kan skapa och använda något mer komplexa. hello viktig sak är att hello-värde som du anger här måste vara hello samma värde som du anger i hello nästa steg när du skapar anslutningen. hello returnerade ska visa **Status: lyckade**.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName ClassicVNet `
  -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
  ```
2. Skapa hello VPN-anslutning genom att köra följande kommandon hello:
   
  Ange hello variabler.

  ```powershell
  $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
  $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
  ```
   
  Skapa hello-anslutning. Observera att hello **- ConnectionType** är IPsec, inte Vnet2Vnet.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
  -Location "East US" -VirtualNetworkGateway1 `
  $vnet02gateway -LocalNetworkGateway2 `
  $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

## <a name="section-5-verify-your-connections"></a>Avsnitt 5: Kontrollera anslutningarna

### <a name="tooverify-hello-connection-from-your-classic-vnet-tooyour-resource-manager-vnet"></a>tooverify hello anslutning från din klassiska VNet tooyour Resource Manager VNet

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a>Azure Portal

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="tooverify-hello-connection-from-your-resource-manager-vnet-tooyour-classic-vnet"></a>tooverify hello anslutning från hanteraren för filserverresurser VNet-tooyour klassiska virtuella nätverk

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a>Azure Portal

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Vanliga frågor och svar om VNet-till-VNet

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

