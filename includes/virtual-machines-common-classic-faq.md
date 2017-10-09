


Den här artikeln tar några vanliga frågor som användare ställa om Azure virtuella datorer som skapats med hello klassiska distributionsmodellen.

## <a name="can-i-migrate-my-vm-created-in-hello-classic-deployment-model-toohello-new-resource-manager-model"></a>Kan jag migrera den virtuella datorn skapas i hello klassiska modellen toohello nya Resource Manager-distributionsmodellen?
Ja. Mer information om hur toomigrate, se:

* [Migrera från klassiska tooAzure Resource Manager med Azure PowerShell](../articles/virtual-machines/windows/migration-classic-resource-manager-ps.md).
* [Migrera från klassiska tooAzure Resource Manager med Azure CLI](../articles/virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md).

## <a name="what-can-i-run-on-an-azure-vm"></a>Vad kan jag köra på en virtuell Azure-dator?
Alla prenumeranter kan köra serverprogramvara på en virtuell Azure-dator. Du kan köra de senaste versionerna av Windows Server, samt en mängd olika Linux-distributioner. Information om support finns i:

• För virtuella Windows-datorer – [Microsofts serverprogramsupport för virtuella Azure-datorer](http://go.microsoft.com/fwlink/p/?LinkId=393550)

• För virtuella Linux-datorer – [Linux på Azure-godkända distributioner](http://go.microsoft.com/fwlink/p/?LinkId=393551)

För Windows-klienten är vissa versioner av Windows 7 och Windows 8.1 tillgängliga tooMSDN Azure förmånen prenumeranter och MSDN Dev / Test-betala per användning prenumeranter för utveckling och testning uppgifter. Mer information, bland annat instruktioner och begränsningar, finns i [Windows-klientavbildningar för MSDN-prenumeranter](https://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/).

## <a name="why-are-affinity-groups-being-deprecated"></a>Varför blir tillhörighetsgrupper inaktuella?
Tillhörighetsgrupper är ett äldre koncept för en geografisk gruppering av en kunds molntjänstdistributioner och lagringskonton i Azure. De angavs ursprungligen tooimprove VM-till-VM nätverksprestanda i hello tidig utformning av Azure-nätverk. De stöds också hello första versionen av virtuella nätverk (Vnet) som har begränsad tooa liten uppsättning maskinvara i en region.

hello aktuella Azure-nätverk i en region har utformats så att tillhörighetsgrupper inte längre behövs. Virtuella nätverk har också ett regionalt omfång, så en tillhörighetsgrupp behövs inte längre när du använder ett virtuellt nätverk. På grund av toothese förbättringar rekommenderar längre vi att kunder använder tillhörighetsgrupper eftersom de kan vara att begränsa i vissa situationer. Med hjälp av tillhörighetsgrupper kopplar VMs toospecific maskinvara som begränsar hello valet av VM-storlekar som är tillgängliga tooyou i onödan. Det kan också leda toocapacity-relaterade problem när du försöker tooadd nya virtuella datorer om hello maskinvara som är associerade med hello tillhörighetsgrupp är nästan full.

Tillhörighetsgruppen funktioner är redan föråldrade i hello Azure Resource Manager-distributionsmodellen och hello Azure-portalen. Vi sluta stöd för att skapa tillhörighetsgrupper och skapa storage-resurser som är fästa tooan tillhörighetsgrupp för hello klassiska Azure-portalen. Du behöver inte toomodify befintliga molntjänster som använder en tillhörighetsgrupp. Men du bör inte använda tillhörighetsgrupper för nya molntjänster om inte en Azure-supportpersonal rekommenderar dem.

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Hur mycket lagringsutrymme kan jag använda med en virtuell dator?
Varje datadisk kan vara upp too1 TB. hello antalet datadiskar som du kan använda beror på hello storleken på hello virtuella datorn. Mer information finns i [Storlekar för virtuella datorer](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Ett Azure storage-konto innehåller lagring för hello operativsystemdisken och eventuella hårddiskar. Varje disk är en VHD-fil som lagras som en sidblob. Information om priser finns i [Information om lagringspriser](http://go.microsoft.com/fwlink/p/?LinkId=396819).

## <a name="which-virtual-hard-disk-types-can-i-use"></a>Vilka typer av virtuella hårddiskar kan jag använda?
Azure har bara stöd för fasta virtuella hårddiskar i VHD-format. Om du har en vhdx-disk som du vill toouse i Azure måste toofirst konvertera med hjälp av Hyper-V Manager eller hello [convert-VHD](http://go.microsoft.com/fwlink/p/?LinkId=393656) cmdlet. När du gör det kan du använda [Add-AzureVHD](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet (i Service Management-läge) tooupload hello VHD tooa storage-konto i Azure så att du kan använda den med virtuella datorer.

* Linux instruktioner finns i [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).
* Windows instruktioner finns i [skapa och ladda upp en Windows Server VHD-tooAzure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="are-these-virtual-machines-hello-same-as-hyper-v-virtual-machines"></a>Är de virtuella datorerna hello samma som Hyper-V-datorer?
På många sätt som de är liknande för ”Generation 1” Hyper-V virtuella datorer, men de är inte exakt hello sig samma. Båda typerna ange virtualiserade maskinvaran och virtuella hårddiskar för hello VHD-format är kompatibla. Det innebär att du kan flytta dem mellan Hyper-V och Azure. Det finns tre huvudsakliga skillnader som ibland överraskar Hyper-V-användare:

* Azure Ange inte konsolen åtkomst tooa virtuell dator. Det finns inget sätt tooaccess en virtuell dator tills det är klart starta.
* Virtuella Azure-datorer i de flesta [storlekar](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) har endast 1 virtuellt nätverkskort, vilket innebär att de även kan ha endast 1 extern IP-adress. (hello A8 och A9-storlekar använder du ett nätverkskort för programmet kommunikation mellan instanser i begränsade fall.)
* Virtuella Azure-datorer stöder inte funktioner för virtuella Hyper-V-datorer i Generation 2. Mer information om dessa funktioner finns i [Specifikationer för virtuell dator för Hyper-V](http://technet.microsoft.com/library/dn592184.aspx) och [Översikt över virtuella Generation 2-datorer](https://technet.microsoft.com/library/dn282285.aspx).

## <a name="can-these-virtual-machines-use-my-existing-on-premises-networking-infrastructure"></a>Kan dessa virtuella datorer använda min befintliga, lokala nätverksinfrastruktur?
Du kan använda din befintliga infrastruktur för Azure Virtual Network tooextend för virtuella datorer skapas i hello klassiska distributionsmodellen. hello-metoden är t.ex. ställer in ett avdelningskontor. Du kan etablera och hantera virtuella privata nätverk (VPN) i Azure samt ansluter dem på ett säkert sätt tooon lokal IT-infrastruktur. Mer information finns i [Översikt över Virtual Network](../articles/virtual-network/virtual-networks-overview.md).

Du behöver toospecify hello nätverk som du vill hello virtuella toobelong toowhen du skapar hello virtuell dator. Du kan inte ansluta till en befintlig tooa virtuella nätverk för virtuella datorer. Du kan dock lösa frånkoppling hello virtuell hårddisk (VHD) från hello befintlig virtuell dator och sedan använda den toocreate en ny virtuell dator med hello nätverkskonfigurationen som du vill.

## <a name="how-can-i-access--my-virtual-machine"></a>Hur kommer jag åt min virtuella dator?
Du måste tooestablish en anslutning till toolog på toohello virtuell dator med hjälp av anslutning till fjärrskrivbord för en virtuell Windows-dator eller en SSH (Secure Shell) för en Linux-VM. Anvisningar finns i:

* [Hur tooLog på tooa virtuell dator kör Windows Server](../articles/virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Högst 2 samtidiga anslutningar stöds, om inte hello-servern är konfigurerad som en värd för Remote Desktop Services-sessionen.  
* [Hur tooLog på tooa virtuella datorn kör Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Som standard tillåter SSH högst 10 samtidiga anslutningar. Du kan öka detta antal genom att redigera hello konfigurationsfilen.

Om du har problem med fjärrskrivbord eller SSH, installerar och använder hello [VMAccess](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tillägget toohelp korrigering hello problem.

Ytterligare alternativ för virtuella Windows-datorer:

* I hello klassiska Azure-portalen, hitta hello VM, och klicka sedan på **Återställ fjärråtkomst** från hello kommandofältet.
* Granska [Felsöka fjärrskrivbord anslutningar tooa Windows-baserade virtuella Azure-datorn](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Använd Windows PowerShell-fjärrkommunikation tooconnect toohello VM eller skapa ytterligare slutpunkter för andra resurser tooconnect toohello VM. Mer information finns i [hur tooSet in slutpunkter tooa virtuella](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Om du är bekant med Hyper-V, kan du söker efter en liknande tooVMConnect för verktyget. Azure erbjuder inte en liknande verktyg eftersom konsolen åtkomst tooa virtuella datorn inte stöds.

## <a name="can-i-use-hello-temporary-disk-hello-d-drive-for-windows-or-devsdb1-for-linux-toostore-data"></a>Kan jag använda hello diskutrymme (hello D: enhet för Windows eller /dev/sdb1 för Linux) toostore data?
Du bör inte använda hello diskutrymme (hello D: enheten som standard för Windows eller /dev/sdb1 för Linux) toostore data. De är bara tillfälligt lagringsutrymme, så du riskerar att förlora data som inte kan återställas. Detta kan inträffa när hello virtuella datorn flyttas tooa annan värd. Ändra storlek på en virtuell dator, är uppdatera hello värd eller ett maskinvarufel på hello värden några av hello orsaker kan flytta en virtuell dator.

## <a name="how-can-i-change-hello-drive-letter-of-hello-temporary-disk"></a>Hur kan jag ändra hello enhetsbeteckningen för hello diskutrymme?
Du kan ändra hello enhetsbeteckning genom att flytta filen till sidan hello och omtilldela enhetsbeteckningar på en Windows-dator, men du behöver toomake att du hello stegen i en viss ordning. Instruktioner finns i [ändra hello enhetsbeteckningen för hello Windows diskutrymme](../articles/virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="how-can-i-upgrade-hello-guest-operating-system"></a>Hur kan jag uppgradera hello gästoperativsystemet?
hello termen uppgraderingen innebär vanligtvis glidande tooa mer senaste versionen av operativsystemet, men hello samma maskinvara. Virtuella Azure-datorer hello i processen för att flytta tooa nyare version som skiljer sig för Linux och Windows:

* För Linux virtuella datorer kan använda hello paketet hanteringsverktyg och procedurer som är lämpliga för hello-distribution.
* För en virtuell Windows-dator behöver du toomigrate hello servern med hjälp av något som liknar hello Migreringsverktyg för Windows Server. Försök inte tooupgrade hello gästoperativsystemet medan den finns på Azure. Den stöds inte på grund av hello risken för att förlora åtkomst toohello virtuella datorn. Om problem uppstår under hello uppgraderingen kan gå förlorade hello möjlighet toostart Remote Desktop session och kan inte kan tootroubleshoot hello problem.

Allmän information om hello verktyg och processer för att migrera en Windows Server finns [migrera roller och funktioner tooWindows Server](http://go.microsoft.com/fwlink/p/?LinkId=396940).

## <a name="whats-hello-default-user-name-and-password-on-hello-virtual-machine"></a>Vad är hello standardanvändarnamn och lösenord på hello virtuella datorn?
hello-avbildningar som tillhandahålls av Azure har inte en förkonfigurerad användarnamn och lösenord. När du skapar en virtuell dator med en av dessa bilder, måste du tooprovide ett användarnamn och lösenord som du ska använda toolog på toohello virtuella datorn.

Om du har glömt hello användarnamn eller lösenord och du har installerat hello VM-Agent, kan du installera och använda hello [VMAccess](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tillägget toofix hello problem.

Ytterligare information:

* Om du använder hello klassiska Azure-portalen för hello Linux bilder 'azureuser' anges som en Standardanvändarnamnet men du kan ändra detta genom att använda 'Från galleriet' i stället för ”snabbt skapa” som hello sätt toocreate hello virtuell dator. Med hjälp av 'Från galleriet' också kan du bestämma om toouse lösenord, en SSH-nyckel eller båda toolog i. hello användarkonto är en icke-privilegierade användare som har åtkomst till 'sudo-toorun privilegierad kommandon. hello 'rot' konto har inaktiverats.
* För Windows-avbildningar behöver du tooprovide ett användarnamn och lösenord när du skapar hello VM. hello kontot läggs toohello administratörsgruppen.

## <a name="can-azure-run-anti-virus-on-my-virtual-machines"></a>Kan Azure köra antivirus på mina virtuella datorer?
Azure erbjuder flera alternativ för ett virusskyddsprogram lösningar, men det är in tooyou toomanage. Till exempel behöva en separat prenumeration för program mot skadlig kod och du behöver toodecide när toorun genomsökningar och installera uppdateringar. Du kan lägga till stöd för antivirus med ett VM-tillägg för Microsoft Antimalware, Symantec Endpoint Protection eller TrendMicro Deep Security Agent när du skapar en virtuell Windows-dator eller vid ett senare tillfälle. hello Symantec och TrendMicro tillägg kan du använda en kostnadsfri utvärderingsprenumeration begränsad tid eller en befintlig företagsprenumeration. Microsoft Antimalware är kostnadsfritt. Mer information finns i:

* [Hur tooinstall och konfigurera Symantec Endpoint Protection på en Azure VM](http://go.microsoft.com/fwlink/p/?LinkId=404207)
* [Hur tooinstall och konfigurera Trend Micro djup säkerhet som en tjänst på en Azure VM](http://go.microsoft.com/fwlink/p/?LinkId=404206)
* [Distribuera lösningar för skydd mot skadlig kod i Azure Virtual Machines](https://azure.microsoft.com/blog/2014/05/13/deploying-antimalware-solutions-on-azure-virtual-machines/)

## <a name="what-are-my-options-for-backup-and-recovery"></a>Vad har jag för alternativ för säkerhetskopiering och återställning?
Azure Backup finns som förhandsversion i vissa regioner. Mer information finns i [Säkerhetskopiera virtuella Azure-datorer](../articles/backup/backup-azure-vms.md). Det finns andra lösningar från certifierade partner. toofind reda på vad som är tillgänglig, Sök hello Azure Marketplace.

Ytterligare alternativ är funktioner för toouse hello ögonblicksbilder för blob-lagring. toodo detta, behöver du tooshut ned hello VM innan som förlitar sig på en blob-ögonblicksbild. Detta sparar väntande dataskrivning och placerar hello filsystemet i ett konsekvent tillstånd.

## <a name="how-does-azure-charge-for-my-vm"></a>Hur debiterar Azure för min virtuella dator?
Azure debiterar ett pris baserat på hello VM storleken och operativsystemet. För delar av timmar tar Azure debiterar endast för hello minuters användning. Om du skapar hello VM med en VM-avbildning som innehåller vissa förinstallerade program kan ytterligare timvis programvara tillkomma. Azure avgifter separat för lagring för hello VM operativsystemet och datadiskarna. Tillfällig disklagring är kostnadsfri.

Du debiteras när hello VM-statusen är igång eller Stoppad, men du inte debiteras när hello VM-statusen har stoppats (Frigör allokeras). tooput en virtuell dator i hello stopptillstånd (Frigör allokerade), gör du något av följande hello:

* Stänga av eller ta bort hello VM hello klassiska Azure-portalen.
* Använd hello stoppa AzureVM cmdlet, som finns i hello Azure PowerShell-modulen.
* Använd hello avstängning rollen åtgärden i hello Service Management REST-API och ange StoppedDeallocated för hello PostShutdownAction element.

Mer information finns i [Prissättning för Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="will-azure-reboot-my-vm-for-maintenance"></a>Startar Azure om den virtuella datorn för underhåll?
Azure startar ibland om den virtuella datorn som en del av reguljärt, planerat underhåll uppdateringar i hello Azure-datacenter.

Oplanerade underhållshändelser kan inträffa när Azure upptäcker ett allvarligt maskinvaruproblem som påverkar den virtuella datorn. Oplanerad händelser, Azure automatiskt migrerar hello VM tooa felfri värden och startar om hello VM.

För en fristående virtuell dator (d.v.s. hello VM inte ingår i en tillgänglighetsuppsättning) Azure meddelar hello prenumeration tjänstadministratör via e-post på minst en vecka innan planerat underhåll eftersom hello virtuella datorer kunde startas om under hello uppdatering. Program som körs på virtuella datorer hello kan eventuellt driftstopp.

Du kan också använda hello klassiska Azure-portalen eller Azure PowerShell tooview hello omstart loggar när hello omstart inträffade på grund av tooplanned underhåll. Mer information finns i [Visa VM-omstartsloggar](https://azure.microsoft.com/blog/2015/04/01/viewing-vm-reboot-logs/).

tooprovide redundans, placera två eller fler på samma sätt konfigurerade virtuella datorer i hello samma tillgänglighetsuppsättning. På så sätt ser du till att minst en virtuell dator är tillgänglig under planerat eller oplanerat underhåll. Azure garanterar vissa nivåer av VM-tillgänglighet för den här konfigurationen. Mer information finns i [hantera hello tillgängligheten för virtuella datorer](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="additional-resources"></a>Ytterligare resurser
[Om Azure Virtual Machines](../articles/virtual-machines/virtual-machines-linux-about.md)

[Skapa och hantera virtuella Linux-datorer med hello Azure CLI](../articles/virtual-machines/linux/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Skapa och hantera virtuella Windows-datorer med Azure PowerShell](../articles/virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

