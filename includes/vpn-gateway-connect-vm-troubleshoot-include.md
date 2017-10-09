Om du har problem med anslutning tooa virtuella datorn via VPN-anslutning, kontrollerar du hello följande:

- Kontrollera att VPN-anslutningen har genomförts.
- Kontrollera att du ansluter toohello privata IP-adress för hello VM.
- Om du kan ansluta toohello VM använder hello privat IP-adress, men inte hello datornamn, kontrollera att du har konfigurerat DNS korrekt. Mer information om hur namnmatchningen fungerar för virtuella datorer finns i [Namnmatchning för virtuella datorer](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

När du ansluter via punkt-till-plats kan du kontrollera hello följande ytterligare objekt:

- Använd 'ipconfig' toocheck hello IPv4-adress som tilldelats toohello Ethernet-nätverkskort på hello-dator som du ansluter. Om hello IP-adressen ligger inom hello adressintervall hello virtuella nätverk som du ansluter till eller hello-adressintervallet för din VPNClientAddressPool detta kallas tooas som en överlappande adressutrymmen. När adressutrymmet överlappar på detta sätt, hello nätverkstrafik nå inte Azure, förblir den i hello lokala nätverket.
- Kontrollera konfigurationspaket för VPN-klienten att hello genererades när hello DNS-serveradresser har angetts för hello virtuella nätverk. Om du har uppdaterat hello DNS-serveradresser generera och installera ett nytt konfigurationspaket för VPN-klienten.

Mer information om hur du felsöker en RDP-anslutning finns [Felsöka fjärrskrivbord anslutningar tooa VM](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).
