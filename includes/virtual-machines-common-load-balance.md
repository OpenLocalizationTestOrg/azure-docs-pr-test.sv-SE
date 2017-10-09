

Det finns två nivåer av belastningsutjämning för tjänster som Azure-infrastrukturen:

* **DNS-nivå**: belastningsutjämning för trafik toodifferent molntjänster finns i olika datacenter, toodifferent Azure webbplatser finns i olika Datacenter eller tooexternal slutpunkter. Detta görs med Azure Traffic Manager och hello resursallokering belastningsfördelningsmetod.
* **Nätverk nivå**: belastningsutjämning av inkommande Internet-trafik toodifferent virtuella datorer med en molnbaserad tjänst eller belastningsutjämning för trafik mellan virtuella datorer i en molnbaserad tjänst eller ett virtuellt nätverk. Detta görs med hello Azure belastningsutjämnare.

## <a name="traffic-manager-load-balancing-for-cloud-services-and-websites"></a>Traffic Manager belastningsutjämning för molntjänster och webbplatser
Traffic Manager kan du toocontrol hello distribution av användaren trafik tooendpoints, vilket kan innefatta molntjänster, webbplatser, externa webbplatser och andra Traffic Manager-profiler. Traffic Manager fungerar genom att tillämpa en princip för intelligent motorn tooDomain Name System (DNS) frågor för hello domännamn för Internet-resurser. Dina molntjänster eller webbplatser kan köras i olika Datacenter över hälsningsmeddelande.

Du måste använda REST- eller Windows PowerShell tooconfigure externa slutpunkter eller Traffic Manager-profiler som slutpunkter.

Traffic Manager använder tre belastningsutjämning metoder toodistribute trafik:

* **Redundans**: Använd den här metoden när du vill toouse en primär slutpunkt för all trafik, men ger säkerhetskopieringar om hello primära blir otillgänglig.
* **Prestanda**: Använd den här metoden när du har slutpunkter i olika geografiska platser och du vill begära klienter toouse hello ”närmaste” endpoint vad gäller hello lägsta svarstid.
* **Resursallokering:** Använd den här metoden när du vill toodistribute belastningen över en uppsättning cloud services i hello samma datacenter eller i molntjänster eller webbplatser i olika datacenter.

Mer information finns i [om trafik Load Balancing metoder Manager](../articles/traffic-manager/traffic-manager-routing-methods.md).

hello följande diagram visar ett exempel på hello resursallokering belastningen belastningsutjämning metod för att distribuera trafik mellan olika molntjänster.

![belastningsutjämning](./media/virtual-machines-common-load-balance/TMSummary.png)

hello grundläggande processen ser hello följande:

1. En domän namn motsvarande tooa webbtjänst frågar en Internetklient.
2. DNS vidarebefordrar hello namn frågan begäran tooTraffic Manager.
3. Traffic Manager väljer hello nästa Molntjänsten i hello resursallokering lista och skickar tillbaka hello DNS-namn. hello Internet klientens DNS-servern matchar hello tooan IP-adress och skickar det toohello Internet-klient.
4. hello Internet-klient ansluter med hello valt av Traffic Manager-Molntjänsten.

Mer information finns i [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md).

## <a name="azure-load-balancing-for-virtual-machines"></a>Azure belastningsutjämning för virtuella datorer
Virtuella datorer i hello samma molntjänst eller virtuella nätverket kan kommunicera med varandra direkt med sina privata IP-adresser. Datorer och tjänster utanför hello molntjänst eller virtuella nätverk kan endast kommunicera med virtuella datorer i en molnbaserad tjänst eller ett virtuellt nätverk med en konfigurerad slutpunkt. En slutpunkt är en mappning av en offentlig IP-adress och port toothat privat IP-adress och port för en virtuell dator eller en webbroll inom en Azure-molntjänst.

hello Azure belastningsutjämnare distribuerar slumpmässigt en viss typ av inkommande trafik över flera virtuella datorer eller tjänster i en konfiguration som kallas en belastningsutjämnad uppsättning. Sprida hello belastningen på begäran Internet-trafik över flera webbservrar eller webbroller.

hello visar följande diagram en belastningsutjämnad slutpunkt för standard (okrypterat) webbtrafik som delas mellan tre virtuella datorer för hello offentliga och privata TCP-port 80. Dessa tre virtuella datorer finns i en belastningsutjämnad uppsättning.

![belastningsutjämning](./media/virtual-machines-common-load-balance/LoadBalancing.png)

Mer information finns i [Azure belastningsutjämnare](../articles/load-balancer/load-balancer-overview.md). Hello stegen toocreate en belastningsutjämnad uppsättning, finns [konfigurera en belastningsutjämnad uppsättning](../articles/load-balancer/load-balancer-get-started-internet-arm-ps.md).

Azure kan också läsa belastningsutjämna inom en molnbaserad tjänst eller ett virtuellt nätverk. Detta kallas för intern belastningsutjämning och kan användas i hello följande sätt:

* tooload balans mellan servrar i olika nivåer av en flernivåapp (till exempel mellan webb- och databasnivåer).
* tooload saldo line-of-business (LOB) program finns i Azure utan ytterligare belastningen belastningsutjämnaren maskinvara eller programvara.
* tooinclude lokala servrar i hello uppsättning datorer vars trafik är Utjämning av nätverksbelastning.

Liknande tooAzure belastningsutjämning, intern belastningsutjämning underlättas genom att konfigurera en intern belastningsutjämnad uppsättning.

hello följande diagram visar ett exempel på en intern belastningsutjämning slutpunkt för en driftsapplikationer (LOB) som delas mellan tre virtuella datorer i ett virtuellt nätverk mellan platser.

![belastningsutjämning](./media/virtual-machines-common-load-balance/LOBServers.png)

## <a name="load-balancer-considerations"></a>Läsa in belastningsutjämning överväganden
En belastningsutjämnare är som standard konfigurerad tootimeout en inaktiv session i 4 minuter. Om ditt program bakom en belastningsutjämnare lämnar en anslutning inaktiv mer än 4 minuter och det inte har en Keep-Alive konfiguration, hello anslutningen att tas bort. Du kan ändra hello load balancer beteende tooallow en [längre timeout-inställningen för Azure belastningsutjämnare](../articles/load-balancer/load-balancer-tcp-idle-timeout.md).

Andra faktor är hello typ av distribution läge som stöds av Azure belastningsutjämnare. Du kan konfigurera IP-datakällan tillhörigheten (käll-IP, mål-IP) eller käll-IP-protokollet (käll-IP, mål-IP och protokoll). Checka ut [Azure belastningsutjämnare distribution läge (käll-IP-tillhörighet)](../articles/load-balancer/load-balancer-distribution-mode.md) för mer information.

## <a name="next-steps"></a>Nästa steg
Hello stegen toocreate en belastningsutjämnad uppsättning, finns [konfigurera en intern belastningsutjämnad uppsättning](../articles/load-balancer/load-balancer-get-started-ilb-arm-ps.md).

Mer information om belastningsutjämning finns [intern belastningsutjämning](../articles/load-balancer/load-balancer-internal-overview.md).

