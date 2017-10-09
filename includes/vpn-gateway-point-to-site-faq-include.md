### <a name="what-client-operating-systems-can-i-use-with-point-to-site"></a>Vilka klientoperativsystem kan jag använda med punkt-till-plats?

hello följande klientoperativsystem stöds:

* Windows 7 (32-bitars och 64-bitars)
* Windows Server 2008 R2 (endast 64-bitars)
* Windows 8 (32-bitars och 64-bitars)
* Windows 8.1 (32-bitars och 64-bitars)
* Windows Server 2012 (endast 64-bitars)
* Windows Server 2012 R2 (endast 64-bitars)
* Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Kan jag använda alla VPN-programklienter för punkt-till-plats som har stöd för SSTP?

Nej. Stöd för är begränsad endast toohello Windowsversioner av operativsystemet som anges ovan.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Hur många slutpunkter på VPN-klienter kan jag ha i min punkt-till-plats-konfiguration?

Vi stöder in too128 VPN-klienter toobe kan tooconnect tooa virtuella nätverk på hello samma tid.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kan jag använda min egen interna PKI-rotcertifikatutfärdare för punkt-till-plats-anslutningen?

Ja. Tidigare kunde bara självsignerade rotcertifikat användas. Du kan fortfarande överföra 20 rotcertifikat.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Kan jag korsa proxyservrar och brandväggar med punkt-till-plats-funktionen?

Ja. Vi använder SSTP (Secure Socket Tunneling Protocol) tootunnel genom brandväggar. Den här tunneln visas som en HTTPs-anslutning.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-hello-vpn-automatically-reconnect"></a>Om jag startar om en klientdator som konfigurerats för plats-till-plats kommer hello VPN återansluta automatiskt?

Som standard kommer hello klientdatorn inte återupprätta hello VPN-anslutningen automatiskt.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-hello-vpn-clients"></a>Stöd för punkt-till-plats att återansluta automatiskt och DDNS på hello VPN-klienter?

Automatisk återanslutning och DDNS stöds för närvarande inte i punkt-till-plats-VPN:er.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-hello-same-virtual-network"></a>Kan jag ha plats-till-plats och punkt-till-plats-konfigurationer samexistera för hello samma virtuella nätverk?

Ja. Båda dessa lösningar fungerar om du har en routningsbaserad VPN-typ för din gateway. Du måste en dynamisk gateway för hello klassiska distributionsmodellen. Vi har inte stöd för punkt-till-plats för statisk routning VPN-gatewayer eller gatewayar som använder hello `-VpnType PolicyBased` cmdlet.

### <a name="can-i-configure-a-point-to-site-client-tooconnect-toomultiple-virtual-networks-at-hello-same-time"></a>Konfigurerar en punkt-till-plats klienten tooconnect toomultiple virtuella nätverk på hello samtidigt?

Ja, det kan du. Men hello virtuella nätverk kan inte ha överlappande IP-adressprefix och hello punkt-till-plats-adressutrymmen överlappa inte mellan hello virtuella nätverk.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Hur högt dataflöde kan jag förvänta mig vid anslutningar för plats-till-plats eller punkt-till-plats?

Det är svårt toomaintain hello exakt genomflöde hello VPN-tunnlar. IPsec och SSTP är kryptografifrekventa VPN-protokoll. Genomströmning begränsas också av hello svarstid och bandbredd mellan din lokala och hello Internet.
