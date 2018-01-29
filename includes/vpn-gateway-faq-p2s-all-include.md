### <a name="supportedclientos"></a>Vilka klientoperativsystem kan jag använda med punkt-till-plats?

Följande klientoperativsystem stöds:

* Windows 7 (32-bitars och 64-bitars)
* Windows Server 2008 R2 (endast 64-bitars)
* Windows 8 (32-bitars och 64-bitars)
* Windows 8.1 (32-bitars och 64-bitars)
* Windows Server 2012 (endast 64-bitars)
* Windows Server 2012 R2 (endast 64-bitars)
* Windows Server 2016 (endast 64-bitars)
* Windows 10
* OSX version 10.11 för Mac (El Capitan)
* macOS version 10.12 för Mac (Sierra)

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Hur många slutpunkter på VPN-klienter kan jag ha i min punkt-till-plats-konfiguration?

Vi stöder att upp till 128 VPN-klienter kan ansluta till ett virtuellt nätverk på samma gång.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Kan jag korsa proxyservrar och brandväggar med punkt-till-plats-funktionen?

Azure stöder två alternativ för VPN för punkt-till-plats:

* SSTP (Secure Socket Tunneling Protocol). SSTP är en SSL-baserad lösning från Microsoft som kan ta sig igenom brandväggar eftersom de flesta brandväggar öppnar TCP-port 443 som SSL använder.

* IKEv2 VPN. IKEv2 VPN är en standardbaserad IPsec VPN-lösning som använder UDP-port 500 och 4500 och IP-protokoll nummer 50. Eftersom inte alla brandväggar öppnar dessa portar kan det hända att IKEv2 VPN inte kan komma igenom proxyservrar och brandväggar.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Om jag startar om en klientdator som konfigurerats för punkt-till-plats, kommer då VPN att återansluta automatiskt?

Som standard kommer klientdatorn inte återupprätta VPN-anslutningen automatiskt.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Har punkt-till-plats stöd för automatisk återanslutning och DDNS på VPN-klienterna?

Automatisk återanslutning och DDNS stöds för närvarande inte i punkt-till-plats-VPN:er.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Kan jag ha plats-till-plats- och punkt-till-plats-konfigurationer samtidigt på samma virtuella nätverk?

Ja. Med Resource Manager-distributionsmodellen krävs en gateway med routningsbaserad VPN. I den klassiska distributionsmodellen måste du ha en dynamisk gateway. Punkt-till-plats stöds inte för VPN-gatewayer med statisk routning eller principbaserade VPN-gatewayer.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Kan jag konfigurera en punkt-till-plats-klient så att den ansluter till flera virtuella nätverk samtidigt?

Nej. En punkt-till-plats-klient kan bara ansluta till resurser i det virtuella nätverk som den virtuella nätverksgatewayen finns i.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Hur högt dataflöde kan jag förvänta mig vid anslutningar för plats-till-plats eller punkt-till-plats?

Det är svårt att bibehålla ett exakt dataflöde i VPN-tunnlarna. IPsec och SSTP är kryptografifrekventa VPN-protokoll. Dataflödet är också begränsat av svarstiden och bandbredden mellan din lokala plats och Internet. För en VPN-gateway med endast IKEv2-baserade VPN-anslutningar för punkt-till-plats beror det totala dataflödet som du kan förvänta dig på Gateway-SKU:n. Mer information om dataflödet finns i avsnittet om [Gateway-SKU:er](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku).

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp-andor-ikev2"></a>Kan jag använda alla VPN-programklienter för punkt-till-plats som har stöd för SSTP och/eller IKEv2?

Nej. Du kan bara använda den inbyggda VPN-klienten i Windows för SSTP, och den inbyggda VPN-klienten i Mac för IKEv2. Se listan över klientoperativsystem som stöds.

### <a name="does-azure-support-ikev2-vpn-with-windows"></a>Har Azure stöd för IKEv2 VPN-anslutningar i Windows?

IKEv2 stöds på Windows 10 och Server 2016. För att kunna använda IKEv2, måste du dock installera uppdateringar och ange ett registernyckelvärde lokalt. OS-versioner före Windows 10 stöds inte och kan bara använda SSTP.

Förbereda Windows 10 eller Server 2016 för IKEv2:

1. Installera uppdateringen.

  | OS-version | Date | Antal/länk |
  |---|---|---|---|
  | Windows Server 2016<br>Windows 10 Version 1607 | 17 januari 2018 | [KB4057142](https://support.microsoft.com/help/4057142/windows-10-update-kb4057142) |
  | Windows 10 Version 1703 | 17 januari 2018 | [KB4057144](https://support.microsoft.com/help/4057144/windows-10-update-kb4057144) |
  |  |  |  |  |

2. Ange registernyckelvärdet. Skapa eller ange ”HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\ IKEv2\DisableCertReqPayload” REG_DWORD nyckel i registret till 1.

### <a name="what-happens-when-i-configure-both-sstp-and-ikev2-for-p2s-vpn-connections"></a>Vad händer när jag konfigurera både SSTP och IKEv2 för P2S VPN-anslutningar?

När du konfigurerar både SSTP och IKEv2 i en blandad miljö (som består av Windows och Mac-enheter), Windows VPN-klienten försöker alltid IKEv2 tunnel först, men kommer att gå SSTP om IKEv2-anslutningen inte lyckas. MacOSX bara ansluter via IKEv2.

### <a name="other-than-windows-and-mac-which-other-platforms-does-azure-support-for-p2s-vpn"></a>För vilka andra plattformar förutom Windows och Mac har Azure stöd för P2S VPN?

Azure har endast stöd för Windows och Mac för P2S-VPN.

### <a name="i-already-have-an-azure-vpn-gateway-deployed-can-i-enable-radius-andor-ikev2-vpn-on-it"></a>Jag har redan en Azure VPN Gateway distribuerad. Kan jag aktivera RADIUS och/eller IKEv2 VPN på den?

Ja, kan du aktivera de här nya funktionerna på redan distribuerade gateways med Powershell eller Azure-portalen, förutsatt att gateway-SKU: N som du använder har stöd för RADIUS-och/eller IKEv2. Till exempel stöder inte VPN-gateway grundläggande SKU RADIUS- eller IKEv2.
