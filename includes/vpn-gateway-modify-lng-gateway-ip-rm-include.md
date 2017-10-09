### <a name="gwipnoconnection"></a>toomodify hello lokal nätverksgateway GatewayIpAddress - ingen gateway-anslutningen

Om hello VPN-enhet som du vill tooconnect toohas ändras dess offentliga IP-adress, måste toomodify hello lokala nätverket gateway tooreflect som ändras. Använd hello exempel toomodify en lokal nätverksgateway som inte har en gateway-anslutningen.

När du ändrar det här värdet kan du också ändra hello adressprefix på hello samtidigt. Vara säker på att toouse hello befintliga namnet på din lokala nätverksgateway toooverwrite hello aktuella inställningar. Om du använder ett annat namn, skapa en ny lokal nätverksgateway, i stället för att skriva över hello befintlig.

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <a name="gwipwithconnection"></a>toomodify hello lokal nätverksgateway GatewayIpAddress - befintlig gateway-anslutningen

Om hello VPN-enhet som du vill tooconnect toohas ändras dess offentliga IP-adress, måste toomodify hello lokala nätverket gateway tooreflect som ändras. Om det finns redan en gatewayanslutning, måste du först tooremove hello anslutning. Du kan ändra hello gateway IP-adress och skapa en ny anslutning när hello anslutningen tas bort. Du kan också ändra hello adressprefix på hello samtidigt. Det medför en del avbrott för din VPN-anslutning. När du ändrar hello IP-adressen för gateway, behöver du inte toodelete hello VPN-gateway. Du behöver bara tooremove hello anslutning.
 

1. Ta bort hello anslutningen. Du hittar hello namnet på anslutningen genom att använda hello 'Get-AzureRmVirtualNetworkGatewayConnection-cmdlet.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. Ändra hello 'GatewayIpAddress'-värde. Du kan också ändra hello adressprefix på hello samtidigt. Vara säker på att toouse hello befintliga namnet på din lokala gateway toooverwrite hello aktuella nätverksinställningar. Om du inte skapa en ny lokal nätverksgateway, i stället för att skriva över hello befintlig.

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. Skapa hello-anslutning. I det här exemplet konfigurerar vi en IPsec-anslutningstyp. När du återskapa din anslutning Använd hello anslutningstyp som har angetts för din konfiguration. Ytterligare anslutningstyper finns hello [PowerShell-cmdleten](https://msdn.microsoft.com/library/mt603611.aspx) sidan.  Du kan köra hello 'Get-AzureRmVirtualNetworkGateway' cmdlet tooobtain hello VirtualNetworkGateway namn.
   
    Ange hello variabler.

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    Skapa hello-anslutning.

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```