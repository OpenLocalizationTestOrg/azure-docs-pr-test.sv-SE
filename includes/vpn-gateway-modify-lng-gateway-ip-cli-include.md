### <a name="toomodify-hello-local-network-gateway-gatewayipaddress"></a><span data-ttu-id="64c1b-101">toomodify hello lokal nätverksgateway 'gatewayIpAddress'</span><span class="sxs-lookup"><span data-stu-id="64c1b-101">toomodify hello local network gateway 'gatewayIpAddress'</span></span>

<span data-ttu-id="64c1b-102">Om hello VPN-enhet som du vill tooconnect toohas ändras dess offentliga IP-adress, måste toomodify hello lokala nätverket gateway tooreflect som ändras.</span><span class="sxs-lookup"><span data-stu-id="64c1b-102">If hello VPN device that you want tooconnect toohas changed its public IP address, you need toomodify hello local network gateway tooreflect that change.</span></span> <span data-ttu-id="64c1b-103">hello gateway IP-adress kan ändras utan att ta bort en befintlig VPN-gateway-anslutning (om du har en).</span><span class="sxs-lookup"><span data-stu-id="64c1b-103">hello gateway IP address can be changed without removing an existing VPN gateway connection (if you have one).</span></span> <span data-ttu-id="64c1b-104">toomodify hello gateway IP-adressen kan ersätta hello värden 'Webbplats2' och 'TestRG1' med din egen med hello [az nätverket lokala gateway-uppdatering](https://docs.microsoft.com/cli/azure/network/local-gateway#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="64c1b-104">toomodify hello gateway IP address, replace hello values 'Site2' and 'TestRG1' with your own using hello [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway#update) command.</span></span>

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

<span data-ttu-id="64c1b-105">Kontrollera att hello IP-adressen är korrekt i hello utdata:</span><span class="sxs-lookup"><span data-stu-id="64c1b-105">Verify that hello IP address is correct in hello output:</span></span>

```
"gatewayIpAddress": "23.99.222.170",
```