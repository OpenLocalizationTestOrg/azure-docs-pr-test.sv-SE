### <a name="tooview-local-network-gateways"></a>tooview lokala nätverksgatewayer

tooview en lista över hello lokala nätverksgatewayer, Använd hello [az nätverkslistan lokal gateway](https://docs.microsoft.com/cli/azure/network/local-gateway#list) kommando.

```azurecli
az network local-gateway list --resource-group TestRG1
```

[!INCLUDE [modify-prefix](vpn-gateway-modify-ip-prefix-cli-include.md)]

[!INCLUDE [modify-gateway-IP](vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

### <a name="tooverify-hello-shared-key-values"></a>tooverify hello delade nyckelvärden

Kontrollera att hello delad nyckel/värde är hello samma värde som du använde för din konfiguration för VPN-enhet. Om det inte kör hello anslutningen igen med hello värdet från hello enheter eller uppdatera hello-enhet med hello-värde från hello returtyp. hello-värden måste matcha. tooview hello delad nyckel, Använd hello [az nätverk vpn-anslutning-listan](https://docs.microsoft.com/cli/azure/network/vpn-connection#list).

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 --resource-group TestRG1
```
### <a name="tooview-hello-vpn-gateway-public-ip-address"></a>tooview hello VPN-gateway offentlig IP-adress

toofind hello offentliga IP-adressen för din virtuella nätverksgateway, Använd hello [az offentliga ip-lista över](https://docs.microsoft.com/cli/azure/network/public-ip#list) kommando. För att enkelt läsa är hello utdata för det här exemplet formaterad toodisplay hello lista över offentliga IP-adresser i tabellformat.

```azurecli
az network public-ip list --resource-group TestRG1 --output table
```
