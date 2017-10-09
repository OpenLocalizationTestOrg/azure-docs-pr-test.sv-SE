### <a name="noconnection"></a>toomodify lokala nätverket gateway IP-adressprefix - ingen gateway-anslutningen

tooadd ytterligare adressprefix:

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
```

tooremove adressprefix:<br>
Lämna ut hello-prefix som du inte längre behöver. I det här exemplet vi inte längre prefixet 20.0.0.0/24 (från hello föregående exempel), så vi uppdatera hello lokal nätverksgateway utan att prefix.

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','30.0.0.0/24')
```

### <a name="withconnection"></a>toomodify lokala nätverket gateway IP-adressprefix - befintlig gateway-anslutningen

Om du har en gateway-anslutning och vill tooadd eller ta bort hello IP-adressprefixet som ingår i din lokala nätverksgateway, behöver toodo hello följa stegen i ordning. Det medför en del avbrott för din VPN-anslutning. När du ändrar IP-adressprefix, behöver du inte toodelete hello VPN-gateway. Du behöver bara tooremove hello anslutning.


1. Ta bort hello anslutningen.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
  ```
2. Ändra hello adressprefix för din lokala nätverksgateway.
   
  Ange hello variabel för hello LocalNetworkGateway.

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
  ```
   
  Ändra hello prefix.
   
  ```powershell
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```
3. Skapa hello-anslutning. I det här exemplet konfigurerar vi en IPsec-anslutningstyp. När du återskapa din anslutning Använd hello anslutningstyp som har angetts för din konfiguration. Ytterligare anslutningstyper finns hello [PowerShell-cmdleten](https://msdn.microsoft.com/library/mt603611.aspx) sidan.
   
  Ange hello variabel för hello VirtualNetworkGateway.

  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
  ```
   
  Skapa hello-anslutning. Det här exemplet använder hello variabeln $local som du angav i steg 2.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName -Location 'West US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```