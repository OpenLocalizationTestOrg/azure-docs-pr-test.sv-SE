### <a name="toomodify-hello-local-network-gateway-gatewayipaddress"></a>toomodify hello lokal nätverksgateway 'gatewayIpAddress'

Om hello VPN-enhet som du vill tooconnect toohas ändras dess offentliga IP-adress, måste toomodify hello lokala nätverket gateway tooreflect som ändras. hello gateway IP-adress kan ändras utan att ta bort en befintlig VPN-gateway-anslutning (om du har en). toomodify hello gateway IP-adressen kan ersätta hello värden 'Webbplats2' och 'TestRG1' med din egen med hello [az nätverket lokala gateway-uppdatering](https://docs.microsoft.com/cli/azure/network/local-gateway#update) kommando.

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

Kontrollera att hello IP-adressen är korrekt i hello utdata:

```
"gatewayIpAddress": "23.99.222.170",
```