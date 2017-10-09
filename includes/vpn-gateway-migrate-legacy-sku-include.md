> [!NOTE]
> * hello VPN-gateway offentliga IP-adressen ändras när du migrerar från en gamla SKU tooa nya SKU.
> * Du kan inte migrera klassiska VPN-gatewayer toohello nya SKU: er. Klassiska VPN-gatewayer kan endast använda hello äldre (gamla) SKU: er.
> 

Det går inte att ändra storlek på Azure VPN gateway mellan hello gamla SKU: er och hello nya SKU-familjer. Om du har VPN-gatewayer i hello Resource Manager-distributionsmodellen som använder hello äldre version av hello SKU: er kan du migrera toohello nya SKU: er. toomigrate, du ta bort hello befintliga VPN-gateway för det virtuella nätverket och sedan skapa en ny.

Arbetsflöde för migrering:

1. Ta bort alla anslutningar toohello virtuell nätverksgateway.
2. Ta bort hello gamla VPN-gateway.
3. Skapa hello nya VPN-gateway.
4. Uppdatera din lokala VPN-enheter med hello nya VPN-gateway IP-adressen (för anslutningar för plats-till-plats).
5. Uppdatera hello gateway IP-adressvärde för alla gateways för VNet-till-VNet lokala nätverk som ansluter toothis gateway.
6. Hämta nya VPN-konfiguration klientpaket för P2S-klienter som ansluter toohello virtuellt nätverk via VPN-gateway.
7. Återskapa hello anslutningar toohello virtuell nätverksgateway.
