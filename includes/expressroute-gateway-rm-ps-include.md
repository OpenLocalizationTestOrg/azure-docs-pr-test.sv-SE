hello steg för den här aktiviteten används ett VNet baserat på hello värdena i hello följande konfiguration är en lista. Ytterligare inställningar och namn också beskrivs i den här listan. Vi använda inte den här listan direkt i hello steg, även om vi lägger till variabler utifrån hello värdena i den här listan. Du kan kopiera hello listan toouse som en referens, ersätter hello värden med dina egna.

**Referens för konfigurationslistan**

* Virtuella nätverksnamnet = ”TestVNet”
* Virtuella nätverks-adressutrymme = 192.168.0.0/16
* Resursgruppens namn = ”TestRG”
* Undernät1 Name = ”FrontEnd” 
* Undernät1 adressutrymme = ”192.168.1.0/24”
* Namnet för gateway-undernätet: ”GatewaySubnet” måste du alltid namnge ett gateway-undernät *GatewaySubnet*.
* Gateway-undernätsadressutrymmet = ”192.168.200.0/26”
* Region = ”östra USA”
* Gatewaynamnet = ”GW”
* Gateway IP-Name = ”GWIP”
* Gateway-IP-konfiguration namn = ”gwipconf”
* Typ = ”ExpressRoute” den här typen krävs för en ExpressRoute-konfiguration.
* Gateway offentliga IP-namn = ”gwpip”

## <a name="add-a-gateway"></a>Lägga till en gateway
1. Ansluta tooyour Azure-prenumeration.

  ```powershell 
  Login-AzureRmAccount
  Get-AzureRmSubscription 
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. Deklarera variablerna för den här övningen. Vara säker på att tooedit hello exempel tooreflect hello inställningar som du vill toouse.

  ```powershell 
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "GW"
  $GWIPName = "GWIP"
  $GWIPconfName = "gwipconf"
  $VNetName = "TestVNet"
  ```
3. Lagra hello virtuella nätverksobjektet som en variabel.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  ```
4. Lägg till en gateway-undernätet tooyour virtuellt nätverk. hello gateway-undernätet måste ha namnet ”GatewaySubnet”. Du bör skapa en gateway-undernät som är minst/27 eller större (/ 26/25, etc.).

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
  ```
5. Ange hello konfiguration.

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. Lagra hello gateway-undernät som en variabel.

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
  ```
7. Begär en offentlig IP-adress. hello IP-adress krävs innan du skapar hello gateway. Du kan inte ange hello IP-adress som du vill toouse; Det allokeras dynamiskt. Du ska använda IP-adressen i nästa hello-konfigurationsavsnittet. Hej AllocationMethod måste vara dynamiska.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  ```
8. Skapa hello-konfigurationen för din gateway. gateway-konfiguration för hello definierar hello undernät och hello offentliga IP-adressen toouse. I det här steget anger du hello-konfiguration som ska användas när du skapar hello gateway. Det här steget skapar inte faktiskt hello gateway-objekt. Använda hello exemplet nedan toocreate gateway-konfiguration.

  ```powershell
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```
9. Skapa hello gateway. I det här steget hello **- GatewayType** är särskilt viktigt. Du måste använda hello värde **ExpressRoute**. När du har kört dessa cmdlets ta hello gateway 45 minuter eller mer toocreate.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
  ```

## <a name="verify-hello-gateway-was-created"></a>Kontrollera hello gateway har skapats
Använd följande kommandon tooverify som hello gateway har skapats hello:

```powershell
Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a>Ändra storlek på en gateway
Det finns ett antal [Gateway-SKU: er](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Du kan använda hello efter kommandot toochange hello Gateway-SKU när som helst.

> [!IMPORTANT]
> Det här kommandot fungerar inte för UltraPerformance gateway. toochange din gateway tooan UltraPerformance gateway tar du bort hello befintlig ExpressRoute-gateway och sedan skapa en ny UltraPerformance gateway. toodowngrade din gateway från en UltraPerformance gateway tar du först bort hello UltraPerformance gateway och sedan skapa en ny gateway.
> 
> 

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a>Ta bort en gateway
Använd hello efter kommandot tooremove en gateway:

```powershell
Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```