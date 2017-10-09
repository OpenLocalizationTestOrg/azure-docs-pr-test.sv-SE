Det finns flera orsaker när du inte får inledas eller ansluta tooan program som körs på en Azure-dator (VM). Orsaker är hello-program som inte körs eller lyssnar på hello förväntade portar hello lyssningsport blockeras eller nätverk regler som inte skickar trafik toohello program. Den här artikeln beskrivs problem med en metodisk metoden toofind och rätt hello.

Om du har problem med anslutning tooyour VM med RDP eller SSH, finns i en av hello följande artiklar först:

* [Felsöka fjärrskrivbord anslutningar tooa Windows-baserade virtuella Azure-dator](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)
* [Felsökning av SSH (Secure Shell) anslutningar tooa Linux-baserade virtuell Azure-dator](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../articles/resource-manager-deployment-model.md). Den här artikeln täcker bägge modellerna, men Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och hello Stack Overflow-forum](https://azure.microsoft.com/support/forums/). Alternativt kan du även filen en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.

## <a name="quick-start-troubleshooting-steps"></a>Snabbkurs felsökningssteg
Om du har problem med att ansluta tooan programmet försök hello följande allmänna instruktioner för felsökning. Försöka ansluta tooyour programmet igen efter varje steg:

* Starta om hello virtuella datorn
* Återskapa hello endpoint / regler i brandväggen / nätverk regler för nätverkssäkerhetsgrupper (NSG)
  * [Resource Manager-modellen - hantera Nätverkssäkerhetsgrupper](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
  * [Klassiska modellen - hantera molntjänster slutpunkter](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
* Ansluta från annan plats, till exempel ett annat Azure virtuellt nätverk
* Distribuera om hello virtuella datorn
  * [Distribuera Windows VM](../articles/virtual-machines/windows/redeploy-to-new-node.md)
  * [Distribuera virtuell Linux-dator](../articles/virtual-machines/linux/redeploy-to-new-node.md)
* Återskapa hello virtuell dator

Mer information finns i [felsöka slutpunktsanslutning (RDP/SSH/HTTP, etc. fel)](https://social.msdn.microsoft.com/Forums/azure/en-US/538a8f18-7c1f-4d6e-b81c-70c00e25c93d/troubleshooting-endpoint-connectivity-rdpsshhttp-etc-failures?forum=WAVirtualMachinesforWindows).

## <a name="detailed-troubleshooting-overview"></a>Detaljerad felsökning översikt
Det finns fyra huvudområden tootroubleshoot hello åtkomst till ett program som körs på en virtuell Azure-dator.

![Felsöka kan inte starta programmet](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access1.png)

1. hello-program som körs på hello virtuella Azure-datorn.
   * Körs hello programmet korrekt?
2. hello virtuella Azure-datorn.
   * Hello Virtuella datorn körs korrekt och svarar toorequests?
3. Azure-nätverksslutpunkter.
   * Moln slutpunkter för virtuella datorer i hello klassiska distributionsmodellen.
   * Nätverkssäkerhetsgrupper och inkommande NAT-regler för virtuella datorer i Resource Manager-distributionsmodellen.
   * Kan trafiken flödar från användare toohello VM/application på hello förväntade portar?
4. Din Internet-edge-enhet.
   * Brandväggsregler på plats förhindrar trafiken flödar korrekt?

För klientdatorer som ansluter till hello program via en plats-till-plats VPN eller ExpressRoute-anslutning, hello huvudområden som kan orsaka problem är hello program och hello virtuella Azure-datorn.

toodetermine hello källan hello problemet och dess korrigering, Följ dessa steg.

## <a name="step-1-access-application-from-target-vm"></a>Steg 1: Öppna programmet från virtuell dator
Försök tooaccess hello program med hello rätt klientprogram från hello VM som den körs. Använd hello lokala värdnamn, hello lokal IP-adress eller hello loopback-adressen (127.0.0.1).

![Starta programmet direkt från hello VM](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access2.png)

Om programmet hello en webbserver, öppna en webbläsare på hello VM och försök tooaccess en webbsida som finns på hello VM.

Om du har åtkomst till programmet hello gå för[steg 2](#step2).

Om du inte kan komma åt programmet hello Kontrollera hello följande inställningar:

* hello programmet körs på hello virtuella måldatorn.
* hello program lyssnar på förväntade hello TCP och UDP-portar.

Använd hello på både Windows- och Linux-baserade virtuella datorer, **netstat – en** kommandot tooshow hello aktiva lyssnarportar. Granska hello utdata för hello förväntade portar som ditt program bör lyssna. Starta om programmet hello eller konfigurera den toouse hello förväntade portar och försök tooaccess hello programmet lokalt igen.

## <a id="step2"></a>Steg 2: Komma åt program från en annan virtuell dator i hello samma virtuella nätverk
Tooaccess hello programmet från en annan virtuell dator, men det hello i samma virtuella nätverk som använder hello virtuella datorns värdnamn eller dess tilldelade Azure public, private eller providern IP-adress. För virtuella datorer som skapats med hjälp av hello klassiska distributionsmodellen, Använd inte hello offentliga IP-adressen för hello-Molntjänsten.

![Starta programmet från en annan virtuell dator](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access3.png)

Om programmet hello en webbserver, hello försök tooaccess en webbsida från en webbläsare på en annan virtuell dator i samma virtuella nätverk.

Om du har åtkomst till programmet hello gå för[steg3](#step3).

Om du inte kan komma åt programmet hello Kontrollera hello följande inställningar:

* hello värdbrandväggen på hello målet VM tillåter hello inkommande begäran och för utgående svarstrafik.
* Intrångsidentifiering eller program som körs på hello målet VM för nätverksövervakning tillåter hello trafik.
* Cloud Services-slutpunkter eller Nätverkssäkerhetsgrupper är att tillåta hello-trafik:
  * [Klassiska modellen - hantera molntjänster slutpunkter](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
  * [Resource Manager-modellen - hantera Nätverkssäkerhetsgrupper](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
* En separat komponent som körs i den virtuella datorn i hello sökvägen mellan hello test virtuella datorn och den virtuella datorn, till exempel en belastningsutjämnare eller brandväggen tillåter trafik hello.

Använda Windows-brandväggen med avancerad säkerhet toodetermine på en Windows-baserad virtuell dator, om hello brandväggsregler undanta inkommande och utgående trafik i ditt program.

## <a id="step3"></a>Steg 3: Komma åt program från utanför hello virtuellt nätverk
Försök tooaccess hello program från en dator utanför hello virtuella nätverk som hello VM som hello program körs. Använd ett annat nätverk som den ursprungliga klientdatorn.

![Starta programmet från en dator utanför hello virtuellt nätverk](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access4.png)

Till exempel om programmet hello en webbserver, försöka tooaccess hello webbsida från en webbläsare som körs på en dator som inte är i hello virtuellt nätverk.

Om du inte kan komma åt programmet hello Kontrollera hello följande inställningar:

* Skapa med hjälp av hello klassiska distributionsmodellen för virtuella datorer:
  
  * Kontrollera att hello slutpunktskonfigurationen för hello VM tillåter hello inkommande trafik, särskilt hello-protokoll (TCP och UDP) och hello offentliga och privata portnummer.
  * Kontrollera att åtkomstkontrollistor (ACL) på hello slutpunkten inte förhindrar inkommande trafik från hello Internet.
  * Mer information finns i [hur tooSet in slutpunkter tooa virtuella](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* För virtuella datorer skapas med Resource Manager-modellen hello:
  
  * Kontrollera som hello inkommande NAT-regelkonfigurationen för hello VM tillåter hello inkommande trafik, särskilt hello-protokoll (TCP och UDP) och hello offentliga och privata portnummer.
  * Kontrollera att Nätverkssäkerhetsgrupper tillåter hello inkommande begäran och för utgående svarstrafik.
  * Mer information finns i [Vad är en nätverkssäkerhetsgrupp?](../articles/virtual-network/virtual-networks-nsg.md)

Om du är medlem i en belastningsutjämnad uppsättning hello virtuella datorn eller slutpunkter:

* Kontrollera att hello avsökningen protocol (TCP eller UDP) och portnumret är korrekta.
* Om hello avsökning är protokoll och port ett annat än hello belastningsutjämnad uppsättning protokoll och port:
  * Kontrollera att hello program lyssnar på hello avsökningen protocol (TCP eller UDP) och portnumret (Använd **netstat – a** på hello mål VM).
  * Kontrollera hello värdbrandväggen på hello målet VM att tillåta att hello inkommande avsökningen begäran och utgående avsökningen svarstrafik.

Om du kan öppna programmet hello, kontrollerar du att din Internet-insticksenhet tillåter:

* hello utgående programmet begäran trafik från din klient datorn toohello virtuella Azure-datorn.
* Hej inkommande programmet svarstrafik från hello virtuella Azure-datorn.

## <a name="step-4-if-you-cannot-access-hello-application-use-ip-verify-toocheck-hello-settings"></a>Steg 4 om du inte kommer åt hello program, Använd IP-Kontrollera toocheck hello inställningar. 

Mer information finns i [Azure nätverksövervakning översikt](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="additional-resources"></a>Ytterligare resurser
[Felsöka fjärrskrivbord anslutningar tooa Windows-baserade virtuella Azure-dator](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)

[Felsökning av SSH (Secure Shell) anslutningar tooa Linux-baserade virtuell Azure-dator](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md)

