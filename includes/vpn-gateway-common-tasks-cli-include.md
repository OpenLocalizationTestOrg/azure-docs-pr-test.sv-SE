### <a name="tooview-local-network-gateways"></a><span data-ttu-id="07a8e-101">tooview lokala nätverksgatewayer</span><span class="sxs-lookup"><span data-stu-id="07a8e-101">tooview local network gateways</span></span>

<span data-ttu-id="07a8e-102">tooview en lista över hello lokala nätverksgatewayer, Använd hello [az nätverkslistan lokal gateway](https://docs.microsoft.com/cli/azure/network/local-gateway#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="07a8e-102">tooview a list of hello local network gateways, use hello [az network local-gateway list](https://docs.microsoft.com/cli/azure/network/local-gateway#list) command.</span></span>

```azurecli
az network local-gateway list --resource-group TestRG1
```

[!INCLUDE [modify-prefix](vpn-gateway-modify-ip-prefix-cli-include.md)]

[!INCLUDE [modify-gateway-IP](vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

### <a name="tooverify-hello-shared-key-values"></a><span data-ttu-id="07a8e-103">tooverify hello delade nyckelvärden</span><span class="sxs-lookup"><span data-stu-id="07a8e-103">tooverify hello shared key values</span></span>

<span data-ttu-id="07a8e-104">Kontrollera att hello delad nyckel/värde är hello samma värde som du använde för din konfiguration för VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="07a8e-104">Verify that hello shared key value is hello same value that you used for your VPN device configuration.</span></span> <span data-ttu-id="07a8e-105">Om det inte kör hello anslutningen igen med hello värdet från hello enheter eller uppdatera hello-enhet med hello-värde från hello returtyp.</span><span class="sxs-lookup"><span data-stu-id="07a8e-105">If it is not, either run hello connection again using hello value from hello device, or update hello device with hello value from hello return.</span></span> <span data-ttu-id="07a8e-106">hello-värden måste matcha.</span><span class="sxs-lookup"><span data-stu-id="07a8e-106">hello values must match.</span></span> <span data-ttu-id="07a8e-107">tooview hello delad nyckel, Använd hello [az nätverk vpn-anslutning-listan](https://docs.microsoft.com/cli/azure/network/vpn-connection#list).</span><span class="sxs-lookup"><span data-stu-id="07a8e-107">tooview hello shared key, use hello [az network vpn-connection-list](https://docs.microsoft.com/cli/azure/network/vpn-connection#list).</span></span>

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 --resource-group TestRG1
```
### <a name="tooview-hello-vpn-gateway-public-ip-address"></a><span data-ttu-id="07a8e-108">tooview hello VPN-gateway offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="07a8e-108">tooview hello VPN gateway Public IP address</span></span>

<span data-ttu-id="07a8e-109">toofind hello offentliga IP-adressen för din virtuella nätverksgateway, Använd hello [az offentliga ip-lista över](https://docs.microsoft.com/cli/azure/network/public-ip#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="07a8e-109">toofind hello public IP address of your virtual network gateway, use hello [az network public-ip list](https://docs.microsoft.com/cli/azure/network/public-ip#list) command.</span></span> <span data-ttu-id="07a8e-110">För att enkelt läsa är hello utdata för det här exemplet formaterad toodisplay hello lista över offentliga IP-adresser i tabellformat.</span><span class="sxs-lookup"><span data-stu-id="07a8e-110">For easy reading, hello output for this example is formatted toodisplay hello list of public IPs in table format.</span></span>

```azurecli
az network public-ip list --resource-group TestRG1 --output table
```
