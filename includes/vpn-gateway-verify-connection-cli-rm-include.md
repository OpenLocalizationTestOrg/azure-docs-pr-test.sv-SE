Du kan verifiera att din anslutning lyckats med hjälp av hello [az nätverk vpn-anslutningen visar](/cli/azure/network/vpn-connection#show) kommando. I exemplet hello '--name' refererar toohello namnet på hello-anslutning som du vill tootest. När hello anslutning pågår hello av upprättas, anslutningsstatusen dess 'Ansluta'. När hello anslutning har upprättats hello status ändras too'Connected'.

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```

