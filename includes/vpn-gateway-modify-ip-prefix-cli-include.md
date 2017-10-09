### <a name="noconnection"></a>toomodify lokala nätverket gateway IP-adressprefix - ingen gateway-anslutningen

Om du inte har en gateway-anslutningen och du vill tooadd eller ta bort IP-adressprefix, använder du hello samma kommando som du använder toocreate hello lokal nätverksgateway, [az lokala-nätverksgateway skapa](https://docs.microsoft.com/cli/azure/network/local-gateway#create). Du kan också använda det här kommandot tooupdate hello gateway IP-adress för hello VPN-enhet. toooverwrite hello aktuella inställningar, Använd hello befintliga namnet på din lokala nätverksgateway. Om du använder ett annat namn, skapa en ny lokal nätverksgateway, i stället för att skriva över hello befintlig.

Varje gång du gör en ändring, hello hela listan över prefix måste anges, inte bara hello-prefix som du vill toochange. Ange endast hello prefix som du vill tookeep. I det här fallet 10.0.0.0/24 och 20.0.0.0/24

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --connection-name TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

### <a name="withconnection"></a>toomodify lokala nätverket gateway IP-adressprefix - befintlig gateway-anslutningen

Om du har en gateway-anslutning och vill tooadd eller ta bort IP-adressprefix, kan du uppdatera hello prefix med [az nätverket lokala gateway-uppdatering](https://docs.microsoft.com/cli/azure/network/local-gateway#update). Det medför en del avbrott för din VPN-anslutning. När du ändrar IP-adress för hello-prefix, behöver du inte toodelete hello VPN-gateway.

Varje gång du gör en ändring, hello hela listan över prefix måste anges, inte bara hello-prefix som du vill toochange. I det här exemplet är 10.0.0.0/24 och 20.0.0.0/24 redan med. Vi lägga till hello prefix 30.0.0.0/24 och 40.0.0.0/24 och anger alla 4 hello prefix vid uppdatering.

```azurecli
az network local-gateway update --local-address-prefixes 10.0.0.0/24 20.0.0.0/24 30.0.0.0/24 40.0.0.0/24 --name VNet1toSite2 --connection-name TestRG1
```