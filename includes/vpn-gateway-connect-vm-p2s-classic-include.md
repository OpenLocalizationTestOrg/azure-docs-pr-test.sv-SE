Du kan ansluta tooa virtuell dator som är distribuerade tooyour VNet genom att skapa en anslutning till fjärrskrivbord tooyour VM. hello bästa sätt tooinitially Kontrollera att du kan ansluta tooyour VM är tooconnect genom att använda sin privata IP-adress i stället för datornamn. På så sätt kan du testar toosee om du kan ansluta inte om namnmatchning är korrekt konfigurerat. 

1. Hitta hello privata IP-adressen för den virtuella datorn. Du hittar hello privata IP-adressen för en virtuell dator genom att antingen studera hello egenskaper för hello VM i hello Azure-portalen eller med hjälp av PowerShell.
2. Kontrollera att du är ansluten tooyour VNet med hello punkt-till-plats VPN anslutning. 
3. Öppna anslutning till fjärrskrivbord genom att skriva ”RDP” eller ”anslutning till fjärrskrivbord” i sökrutan för hello hello verktygsfältet och välj sedan anslutning till fjärrskrivbord. Du kan också öppna anslutning till fjärrskrivbord använda hello 'mstsc'-kommando i PowerShell. 
3. Ange hello privata IP-adressen för hello VM i anslutning till fjärrskrivbord. Du kan klicka på ”Visa alternativ” tooadjust ytterligare inställningar och sedan ansluta.

### <a name="tootroubleshoot-an-rdp-connection-tooa-vm"></a>tootroubleshoot en RDP-anslutning tooa VM

Om du har problem när du ansluter tooa virtuella datorn via VPN-anslutning, finns det några saker du kan kontrollera. Mer information om felsökning finns i [Felsöka fjärrskrivbord anslutningar tooa VM](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).

- Kontrollera att VPN-anslutningen har genomförts.
- Kontrollera att du ansluter toohello privata IP-adress för hello VM.
- Använd 'ipconfig' toocheck hello IPv4-adress som tilldelats toohello Ethernet-nätverkskort på hello-dator som du ansluter. Om hello IP-adressen ligger inom hello adressintervall hello virtuella nätverk som du ansluter till eller hello-adressintervallet för din VPNClientAddressPool detta kallas tooas som en överlappande adressutrymmen. När adressutrymmet överlappar på detta sätt, hello nätverkstrafik nå inte Azure, förblir den i hello lokala nätverket.
- Om du kan ansluta toohello VM använder hello privat IP-adress, men inte hello datornamn, kontrollera att du har konfigurerat DNS korrekt. Mer information om hur namnmatchningen fungerar för virtuella datorer finns i [Namnmatchning för virtuella datorer](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
- Kontrollera konfigurationspaket för VPN-klienten att hello genererades när hello DNS-serveradresser har angetts för hello virtuella nätverk. Om du har uppdaterat hello DNS-serveradresser generera och installera ett nytt konfigurationspaket för VPN-klienten.
