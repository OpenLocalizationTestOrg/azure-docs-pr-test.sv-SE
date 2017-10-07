---
title: "aaaTroubleshoot säkerhetskopiering fel med Azure-dator | Microsoft Docs"
description: "Felsökning av säkerhetskopiering och återställning av virtuella Azure-datorer"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 73214212-57a4-4b57-a2e2-eaf9d7fde67f
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: trinadhk;markgal;jpallavi;
ms.openlocfilehash: 9224c47e02b52688adcba5876c674c88502557c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Felsöka säkerhetskopiering av virtuell Azure-dator
> [!div class="op_single_selector"]
> * [Recovery services-valvet](backup-azure-vms-troubleshoot.md)
> * [Säkerhetskopieringsvalvet](backup-azure-vms-troubleshoot-classic.md)
>
>

Du kan felsöka fel påträffades när med Azure Backup med information som anges i hello nedan.

## <a name="backup"></a>Säkerhetskopiering
| information om fel | Lösning |
| --- | --- |
| Hello-åtgärden kunde inte utföras eftersom VM finns inte längre. -Stoppa skydd av virtuell dator utan att ta bort säkerhetskopierade data. Mer information på http://go.microsoft.com/fwlink/?LinkId=808124 |Detta händer när hello primära virtuella datorn tas bort, men hello säkerhetskopieringsprincip fortsätter söker efter en VM-tooback. toofix felet: <ol><li> Återskapa hello virtuell dator med hello samma namn och samma resursgruppens namn [cloud service name]<br>(ELLER)</li><li> Sluta skydda virtuella datorer med eller utan att ta bort hello säkerhetskopierade data. [Mer information](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Snapshot-åtgärden misslyckades på grund av toono nätverksanslutningen på den virtuella datorn hello - Kontrollera att virtuella datorn har åtkomst till nätverket. För ögonblicksbild toosucceed antingen godkända Azure-datacenter IP-adressintervall eller konfigurera en proxyserver för nätverksåtkomst. Mer information finns för http://go.microsoft.com/fwlink/?LinkId=800034. Om du redan använder proxyserver, kontrollera att proxyserverinställningarna har konfigurerats korrekt | Det här felet returneras när du neka hello utgående Internetanslutning på hello virtuella datorn. Internetanslutning krävs för VM-ögonblicksbild tillägget tootake en ögonblicksbild av underliggande diskar på hello virtuella datorn. [Lär dig mer](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine) på hur toofix ögonblicksbild fel på grund av tooblocked nätverksåtkomst. |
| VM-agenten är toocommunicate med hello Azure Backup-tjänsten. -Kontrollera hello VM är ansluten till nätverket och hello VM-agenten är senaste och körs. Mer information, se för http://go.microsoft.com/fwlink/?LinkId=800034 |Det här felet returneras om det uppstår ett problem med hello VM-agenten eller network access toohello Azure-infrastrukturen är blockerad på något sätt. [Lär dig mer](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vm-agent-unable-to-communicate-with-azure-backup) om felsökning av VM ögonblicksbild problem.<br> Starta om hello VM om hello VM-agenten inte är orsakar problem. Ett felaktigt tillstånd för virtuell dator kan ibland orsaka problem och startar om hello VM återställer den här ”dåligt tillstånd”. |
| Den virtuella datorn är i felläge för etablering – starta om hello VM och kontrollera att den virtuella datorn är i körningsläge eller avställning läge för säkerhetskopiering hello | Detta inträffar när en hello tillägget fel leder VM tillstånd toobe i ett feltillstånd för etablering. Gå tooextensions lista och om det finns ett tillägg för misslyckade, ta bort den och försök att starta om hello virtuell dator. Om alla tillägg körs, kan du kontrollera om VM-agenttjänsten körs. Om inte, starta om tjänsten för hello VM-agent. | 
| VMSnapshot tillägget åtgärden misslyckades för hanterade diskar - Kontrollera försök hello säkerhetskopieringen. Om hello problemet upprepas Följ hello instruktionerna på 'http://go.microsoft.com/fwlink/?LinkId=800034'. Kontakta Microsoft support om det misslyckas ytterligare | Det här felet när Azure Backup-tjänsten misslyckas tootrigger en ögonblicksbild. [Lär dig mer](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vmsnapshot-extension-operation-failed) om felsökning VM ögonblicksbild problem. |
| Kunde inte kopiera hello ögonblicksbild av hello virtuell dator, på grund av tooinsufficient ledigt utrymme i hello storage-konto - Kontrollera att lagringskontot har ledigt diskutrymme motsvarande anslutna toohello data finns på hello premium storage diskar toohello virtuell dator | Vid premium virtuella datorer kopiera vi hello ögonblicksbild toostorage konto. Detta är toomake till att säkerhetskopiering hanteringstrafik som arbetar på ögonblicksbild, inte begränsar hello antalet IOPS tillgängliga toohello program med hjälp av premiumdiskar. Microsoft rekommenderar att du endast allokera 50% av hello totalt konto lagringsutrymme så hello Azure Backup-tjänsten kan kopiera hello ögonblicksbild toostorage konto och överför data från denna kopierade plats i storage-konto toohello valv. | 
| Tooperform hello åtgärden som hello VM agenten svarar inte |Det här felet returneras om det uppstår ett problem med hello VM-agenten eller network access toohello Azure-infrastrukturen är blockerad på något sätt. För virtuella Windows-datorer, kontrollera hello VM-agentstatus för tjänsten i services och om hello agenten visas i program på Kontrollpanelen. Försök att ta bort hello program från kontrollen panelen och installera hello agent som tidigare nämnts [under](#vm-agent). När du har installerat hello agent, Utlös tooverify en ad hoc-säkerhetskopiering. |
| Recovery services-tillägget åtgärden misslyckades. -Kontrollera att den senaste virtuella datorns agent finns på den virtuella datorn hello och agent-tjänsten körs. Försök utföra säkerhetskopieringen igen och kontakta Microsoft support om det misslyckas. |Det här felet returneras när VM-agenten är inaktuell. Läs ”uppdatera hello VM-agenten” avsnittet nedan tooupdate hello VM-agenten. |
| Virtuell dator finns inte. -Kontrollera att den virtuella datorn finns eller välj en annan virtuell dator. |Detta händer när hello primära virtuella datorn tas bort men hello säkerhetskopieringsprincip fortsätter toolook för en VM tooperform säkerhetskopiering. toofix felet: <ol><li> Återskapa hello virtuell dator med hello samma namn och samma resursgruppens namn [cloud service name]<br>(ELLER)<br></li><li>Sluta skydda hello virtuell dator utan att ta bort hello säkerhetskopierade data. [Mer information](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Det gick inte att köra. -En annan åtgärd pågår på det här objektet. Vänta tills hello tidigare åtgärden har slutförts och försök sedan igen |En befintlig säkerhetskopia på hello VM körs och går inte att starta ett nytt jobb medan hello befintligt jobb körs. |
| Kopiera virtuella hårddiskar från hello säkerhetskopieringsvalv timeout - försök hello igen om några minuter. Om hello problemet kvarstår kontaktar du Microsoft Support. | Detta händer om det är ett tillfälligt fel på sidan för lagring eller om säkerhetskopieringstjänsten inte får tillräckligt IOPS från storage-konto som är värd för hello VM i ordning tootransfer data inom timeout-period toovault. Kontrollera att du följt [metodtips](backup-azure-vms-introduction.md#best-practices) när du konfigurerar säkerhetskopiering. Försök att flytta VM tooa annat lagringskonto som inte har lästs in och försök att säkerhetskopiera.|
| Det gick inte att säkerhetskopiera med ett internt fel - försök hello igen om några minuter. Om hello problemet kvarstår kontaktar du Microsoft Support |Du kan få felet 2 skäl: <ol><li> Det finns ett övergående problem med att komma åt hello VM-lagring. Kontrollera [Azure Status](https://azure.microsoft.com/en-us/status/) toosee om det inte finns några pågående problem relaterade toocompute, lagring och nätverk i hello region. Försök hello säkerhetskopieringsjobbet när hello problemet är löst. <li>hello ursprungliga virtuella datorn har tagits bort och därför hello återställningspunkten kan inte utföras. tookeep hello säkerhetskopierade data för en borttagen virtuell dator, men ta bort hello säkerhetskopiering fel: ta bort skyddet från hello VM och välj hello alternativet tookeep hello data. Den här åtgärden stoppar hello schemalagda säkerhetskopieringsjobbet och hello återkommande felmeddelanden. |
| Misslyckades tooinstall hello Azure Recovery Services-tillägget hello markerat objekt - hello VM-agenten är en förutsättning för hello Azure Recovery Services-tillägget. Installera hello Azure VM-agenten och omstarten hello registrering |<ol> <li>Kontrollera om hello VM-agenten har installerats korrekt. <li>Kontrollera hello flaggan på hello konfigurationen är korrekt.</ol> [Läs mer](#validating-vm-agent-installation) om hur du installerar hello VM-agenten och hur toovalidate hello VM agentinstallation. |
| Installation av webbprogramtillägg misslyckades med felet hello ”COM + har tootalk toohello Microsoft Distributed Transaction Coordinator |Detta innebär vanligen att hello COM +-tjänst inte körs. Kontakta Microsoft support för hjälp med att åtgärda problemet. |
| Snapshot-åtgärden misslyckades med hello VSS-fel för åtgärden ”den här enheten är låst av BitLocker-diskkryptering. Du måste låsa upp enheten från hello på Kontrollpanelen. |Inaktivera BitLocker för alla enheter på hello VM och se om hello VSS problemet är löst |
| Virtuella datorn är inte i ett tillstånd som gör säkerhetskopior. |<ul><li>Kontrollera om VM är i ett övergående tillstånd mellan körs och stängs ned. Om det är vänta hello VM tillstånd toobe och en av dem och utlösa säkerhetskopiera igen. <li> Om hello VM är en Linux VM och använder [Security Enhanced Linux] kernel-modul, måste tooexclude hello Linux-agenten sökväg (_/var/lib/waagent_) från säkerhet att säkerhetskopiering principtillägg för toomake installeras.  |
| Virtuell Azure-dator inte att hitta. |Detta händer när hello primära virtuella datorn tas bort men hello säkerhetskopieringsprincip fortsätter toolook för en VM-tooperform säkerhetskopiera. toofix felet: <ol><li>Återskapa hello virtuell dator med hello samma namn och samma resursgruppens namn [cloud service name] <br>(ELLER) <li> Inaktivera skyddet för den här virtuella datorn så inte skapas hello säkerhetskopieringsjobb. </ol> |
| Virtuella datorns agent finns inte på den virtuella datorn hello - installera alla nödvändiga och hello VM-agenten och starta sedan om hello åtgärden. |[Läs mer](#vm-agent) om VM agentinstallation och hur toovalidate hello VM agentinstallation. |
| Snapshot-åtgärden misslyckades på grund av tooVSS skrivare i ett felaktigt tillstånd |Du måste toorestart VSS (Volume Shadow copy Service)-skrivare som är i ett felaktigt tillstånd. tooachieve, från en upphöjd kommandotolk genom att köra _vssadmin listan skrivare_. Utdata innehåller alla VSS-skrivare och deras tillstånd. För varje vars status är inte ”[1] stabil” VSS-skrivare måste du starta om VSS-skrivaren genom att köra följande kommandon från en upphöjd kommandotolk:<br> _net stop serviceName_ <br> _net start serviceName_|
| Snapshot-åtgärden misslyckades på grund av tooa tolkning fel i hello-konfiguration |Detta inträffar på grund av toochanged behörigheter på hello MachineKeys directory: _%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br>Kör nedan kommandot och kontrollera att behörigheterna för MachineKeys directory är standard-som:<br>_Icacls %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br><br> Standardbehörigheterna är:<br>Everyone:(R,W) <br>BUILTIN\Administrators:(F)<br><br>Om du ser behörigheter på MachineKeys directory annat än standard du följa nedanstående steg toocorrect behörigheter, ta bort hello certifikat och utlösa hello säkerhetskopiering.<ol><li>Åtgärda behörighet på MachineKeys directory.<br>Använder Explorer säkerhetsegenskaper och avancerade säkerhetsinställningar för hello katalog, Återställ behörigheter tillbaka toohello standardvärden, ta bort alla extra (än standardvärdet) användarobjekt från hello directory och se till att hello 'Alla' behörigheter har särskilda åtkomst för:<br>-Lista mappar / läsa data <br>-Läsa attribut <br>-Läs utökade attribut <br>-Skapa filer / skriva data <br>-Skapa mappar / lägga till data<br>-Skriva attribut<br>-Skriva utökade attribut<br>-Läsbehörighet<br><br><li>Ta bort alla certifikat med fältet utfärdat till = ”Windows Azure Service Management för tillägg” eller ”Windows Azure CRP-certifikatet Generator”.<ul><li>[Öppna konsolen certifikat (lokal dator)](https://msdn.microsoft.com/library/ms788967(v=vs.110).aspx)<li>Ta bort alla certifikat (-> certifikat under Personal) med fältet utfärdat till = ”Windows Azure Service Management för tillägg” eller ”Windows Azure CRP-certifikatet Generator”.</ul><li>Utlösaren säkerhetskopiering. </ol>|
| Verifieringen misslyckades eftersom den virtuella datorn är krypterad med BEK enbart. Säkerhetskopieringar kan bara aktiveras för virtuella datorer som har krypterats med både BEK och KEK. |Virtuell dator ska krypteras med både BitLocker-krypteringsnyckeln och nyckeln för kryptering. Efter att ska säkerhetskopieringen aktiveras. |
| Azure Backup-tjänsten har inte tillräckliga behörigheter tooKey valvet för säkerhetskopiering av krypterade virtuella datorer. |Säkerhetskopieringstjänsten ska tillhandahållas behörigheterna i PowerShell med hjälp av anvisningarna i **Aktivera säkerhetskopiering** avsnitt i [PowerShell dokumentationen](backup-azure-vms-automation.md). |
|Installation av ögonblicksbild misslyckades med felet tillägg – COM + har tootalk toohello Microsoft Distributed Transaction Coordinator | Kontrollera försök toostart windows-tjänst ”COM + System-program” (från en upphöjd kommandotolk - _net start COMSysApp_). <br>Följ nedanstående steg om det misslyckas under start:<ol><li> Kontrollera att hello inloggningskontot för tjänsten ”Distributed Transaction Coordinator” är ”Nätverkstjänst”. Om det inte ändra det för ”Nätverkstjänst” starta om tjänsten och försök sedan toostart service ”COM + System-program” ”.<li>Om det fortfarande inte toostart, avinstallera och installera tjänsten ”Distributed Transaction Coordinator” genom att följa nedanstående steg:<br> -Stoppa hello MSDTC-tjänsten<br> -Öppna en kommandotolk (cmd) <br> -Köra kommandot ”msdtc-avinstallera” <br> -Köra kommandot ”msdtc-installera” <br> -Börja hello MSDTC-tjänsten<li>Starta windows-tjänst ”COM + System-program” och när den startas startar Säkerhetskopiering från portalen.</ol> |
|  Snapshot-åtgärden misslyckades på grund av tooCOM +-fel | hello rekommenderad åtgärd är toorestart windows-tjänst ”COM + System-program” (från en upphöjd kommandotolk - _net start COMSysApp_). Om hello problemet kvarstår startar du om hello VM. Om omstart hello VM inte hjälper kan du försöka [tar bort hello VMSnapshot tillägget](https://docs.microsoft.com/en-us/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout#cause-3-the-backup-extension-fails-to-update-or-load) och utlösa hello säkerhetskopiering manuellt. |
| Det gick inte toofreeze en eller flera monteringspunkter på hello VM tootake en programkonsekvent ögonblicksbild för filsystemet | Använd hello följande steg: <ol><li>Kontrollera hello filsystemet tillståndet för alla monterade enheter med hjälp av _'tune2fs'_ kommando.<br> Exempel: tune2fs -l/dev/sdb1 \| GREP ”Filesystem tillstånd” <li>Demontera hello enheter för vilka filesystem tillstånd inte är ren med _'umount'_ kommando <li> Kör FileSystemConsistency finns på dessa enheter med hjälp av _'fsck'_ kommando <li> Montera hello enheter igen och försök säkerhetskopiering.</ol> |
| Åtgärden misslyckades på grund av en ögonblicksbild toofailure skapar säkert nätverk kommunikationskanalen | <ol><Li> Öppna Registereditorn genom att köra regedit.exe en förhöjd behörighet. <li> Identifiera alla versioner av. NetFramework finns i systemet. De finns under hello hierarkin för registernyckeln ”HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft” <li> För varje. NetFramework finns i registernyckeln, Lägg till följande nyckel: <br> ”SchUseStrongCrypto” = DWORD: 00000001 </ol>|
| Åtgärden misslyckades på grund av en ögonblicksbild toofailure i installationen av Visual C++ Redistributable för Visual Studio 2012 | Navigera tooC:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion och installera vcredist2012_x64. Kontrollera att värdet för registernyckeln för att tillåta installationen av den här tjänsten är värdet toocorrect dvs. värdet för registernyckeln _HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Msiserver_ anges too3 och inte 4. Om du fortfarande inför problem med installation, starta om installationen genom att köra _MSIEXEC /UNREGISTER_ följt av _MSIEXEC /REGISTER_ från en upphöjd kommandotolk.  |


## <a name="jobs"></a>Jobb
| information om fel | Lösning |
| --- | --- |
| Annullering stöds inte för den här jobbtypen - vänta tills hello jobbet har slutförts. |Ingen |
| hello jobbet är inte i ett cancelable tillstånd, vänta tills hello jobbet har slutförts. <br>ELLER<br> hello valda jobbet är inte i tillståndet cancelable - vänta hello jobbet toocomplete. |Med största sannolikhet slutförs nästan hello jobb. Vänta tills hello jobbet har slutförts.|
| Det går inte att avbryta hello jobbet eftersom det pågår inte – annullering stöds bara för jobb som pågår. Försök avbryter på ett pågående jobb. |Detta inträffar på grund av tooa övergående tillstånd. Vänta en stund och försök hello Avbryt åtgärden. |
| Misslyckade toocancel hello jobb - vänta tills jobbet har slutförts. |Ingen |

## <a name="restore"></a>Återställ
| information om fel | Lösning |
| --- | --- |
| Det gick inte att återställa med molnet internt fel |<ol><li>Cloud service toowhich som du försöker toorestore har konfigurerats med DNS-inställningarna. Du kan kontrollera <br>$deployment = get-AzureDeployment - ServiceName ”ServiceName”-fack ”produktion” Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Om adressen konfigurerad, innebär det att DNS-inställningarna har konfigurerats.<br> <li>Cloud service toowhich tooyou försöker toorestore har konfigurerats med ReservedIP och befintliga virtuella datorer i Molntjänsten har stoppats.<br>Du kan kontrollera en molnbaserad tjänst har reserverade IP-Adressen med hjälp av följande powershell-cmdlets:<br>$deployment = get-AzureDeployment - ServiceName ”servicename”-fack ”produktion” $dep. ReservedIPName <br><li>Du försöker toorestore en virtuell dator med följande särskilda nätverkskonfigurationer i toosame Molntjänsten. <br>-Virtuella datorer under konfigurationen av belastningsutjämnaren (interna och externa)<br>-Virtuella datorer med flera reserverade IP:<br>-Virtuella datorer med flera nätverkskort<br>Välj en ny molntjänst i hello Användargränssnittet eller se för[återställa överväganden](backup-azure-arm-restore-vms.md#restoring-vms-with-special-network-configurations) för virtuella datorer med särskilda nätverkskonfigurationer.</ol> |
| hello markerat DNS-namn är upptaget - ange ett annat DNS-namn och försök igen. |hello DNS-namnet här refererar toohello molntjänstnamnet (vanligtvis slutar med. cloudapp.net). Detta måste toobe unikt. Om felet uppstår måste toochoose ett annat VM-namn under återställningen. <br><br> Det här felet visas endast toousers av hello Azure-portalen. hello lyckas återställningsåtgärden via PowerShell eftersom det endast återställer hello diskar och skapar inte hello VM. att kommer inför hello fel när hello VM skapas av du uttryckligen efter hello disk återställningen igen. |
| konfiguration av hello angivna virtuellt nätverk är inte korrekt - ange ett annat virtuellt nätverk-konfigurationen och försök igen. |Ingen |
| hello anges i molntjänst en reserverad IP-adress, som inte överensstämmer med hello konfigurationen av hello virtuella dator som återställs – ange en annan molntjänst som inte använder reserverade IP: N, eller välj en annan återställningspunkt punkt toorestore från. |Ingen |
| Molntjänsten har nått gränsen för antalet inkommande slutpunkter - åtgärd för nytt hello genom att ange en annan molntjänst eller genom att använda en befintlig slutpunkt. |Ingen |
| Backup-valvet och mål för storage-konto finns i två olika regioner – Se till att hello storage-konto som angavs i restore-åtgärden hello samma Azure-region som hello säkerhetskopieringsvalvet. |Ingen |
| Storage-konto som angetts för hello återställningen inte är stöds - endast grundläggande/Standard storage-konton med lokalt redundant eller inställningarna för geo-redundant replikering stöds. Välj ett lagringskonto som stöds |Ingen |
| Typ av Lagringskonto som angetts för återställningen är inte online – se till att hello storage-konto som angavs i restore-åtgärden är online |Detta kan inträffa på grund av ett tillfälligt fel i Azure Storage eller på grund av tooan avbrott. Välj ett annat lagringskonto. |
| Resursgruppen kvoten har nåtts, bort ta vissa resursgrupper från Azure-portalen eller kontakta Azure-supporten tooincrease hello gränser. |Ingen |
| Valda undernätet finns inte - Välj ett undernät som finns |Ingen |
| Säkerhetskopieringstjänsten har inte tillstånd tooaccess resurser i din prenumeration. |tooresolve detta, första återställa diskar med hjälp av anvisningarna i avsnittet **återställning säkerhetskopierade diskar** i [välja VM Återställ konfiguration](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration). Sedan kan använda PowerShell-åtgärder som nämns i [skapa en virtuell dator från återställda diskar](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate fullständig VM från återställda diskar. |

## <a name="backup-or-restore-taking-time"></a>Säkerhetskopieringen eller återställningen tar tid
Om du ser ditt backup(>12 hours) eller återställa tar time(>6 hours):
* Förstå [faktorer bidrar toobackup tid](backup-azure-vms-introduction.md#total-vm-backup-time) och [faktorer bidrar toorestore tid](backup-azure-vms-introduction.md#total-restore-time).
* Se till att du följer [säkerhetskopiera metodtips](backup-azure-vms-introduction.md#best-practices). 

## <a name="vm-agent"></a>VM-Agent
### <a name="setting-up-hello-vm-agent"></a>Konfigurera hello VM-Agent
Normalt finns hello VM-agenten redan i virtuella datorer som skapas från hello Azure-galleriet. Virtuella datorer som har migrerats från lokala Datacenter skulle inte ha hello VM-agenten är installerad. För sådana virtuella datorer måste hello VM-agenten toobe installerat explicit.

För virtuella Windows-datorer:

* Hämta och installera hello [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Du måste administratören privilegier toocomplete hello installation.
* För klassiska virtuella datorer, [uppdatera hello VM egenskapen](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate som hello agenten är installerad. Det här steget krävs inte för hanteraren för virtuella datorer.

För Linux virtuella datorer:

* Installera senaste från databasen för distribution. Vi **rekommenderar** agent installeras bara igenom distribution-databasen. Mer information om paketnamn finns för[lagringsplatsen för Linux-agent](https://github.com/Azure/WALinuxAgent) 
* För klassiska virtuella datorer kan [uppdatera hello VM egenskapen](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate som hello agenten är installerad. Det här steget krävs inte för hanteraren för virtuella datorer.

### <a name="updating-hello-vm-agent"></a>Uppdatera hello VM-Agent
För virtuella Windows-datorer:

* Uppdaterar hello VM-agenten är så enkelt som att installera om hello [binärfilerna för VM-agenten](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Du måste dock tooensure inga Säkerhetskopieringsåtgärden körs vid hello VM-agenten uppdateras.

För Linux virtuella datorer:

* Följ instruktionerna för hello på [uppdatering Linux VM-agenten](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Vi **rekommenderar** uppdatering agenten endast via distribution databasen. Vi rekommenderar inte hämta hello agent koden från direkt github och uppdatera den. Om den senaste agenten inte är tillgängliga för din distribution, nå ut toodistribution stöd för instruktioner om hur tooinstall senaste agenten. Du kan kontrollera senaste [Windows Azure Linux-agenten](https://github.com/Azure/WALinuxAgent/releases) informationen i github-lagringsplatsen.

### <a name="validating-vm-agent-installation"></a>Verifiera installation av VM-Agent
Hur hello toocheck för VM-Agent-version på virtuella Windows-datorer:

1. Logga in toohello virtuella Azure-datorn och navigera toohello mappen *C:\WindowsAzure\Packages*. Du ska hitta hello WaAppAgent.exe filen finns.
2. Högerklicka på filen hello finns för**egenskaper**, och välj sedan hello **information** fliken hello produktversionen fältet måste innehålla 2.6.1198.718 eller högre

## <a name="troubleshoot-vm-snapshot-issues"></a>Felsökning av problem med VM ögonblicksbild
Säkerhetskopiering är beroende av utfärdande ögonblicksbild kommandon toounderlying lagring. Har inte åtkomst toostorage eller fördröjningar i en ögonblicksbild för körning av aktiviteten kan orsaka hello säkerhetskopieringsjobbet toofail. hello följande kan orsaka ögonblicksbild aktivitet, fel.

1. Network access tooStorage blockeras med hjälp av NSG<br>
    Mer information om hur för[aktivera nätverksåtkomst](backup-azure-vms-prepare.md#network-connectivity) tooStorage med hjälp av antingen Vitlistning av IP-adresser eller via en proxyserver.
2. Virtuella datorer med Sql Server säkerhetskopiering har konfigurerats kan orsaka ögonblicksbild uppgiften fördröjning <br>
   Standard VM säkerhetskopieringsproblem VSS fullständig säkerhetskopiering på virtuella Windows-datorer. Detta kan orsaka försening ögonblicksbild körning på virtuella datorer som kör Sql-servrar och om säkerhetskopiering av Sql Server är konfigurerad. Ange följande registernyckel om det uppstår fel vid säkerhetskopiering på grund av problem med ögonblicksbilder.

   ```
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```
3. VM-statusen Rapportera felaktigt eftersom VM stängs av i RDP.  <br>
   Om du har stängt hello virtuell dator i RDP Kontrollera tillbaka i hello portal att VM-statusen avspeglas korrekt. Om inte, Stäng hello VM-portalen med alternativet ”avslutning' i VM-instrumentpanelen.
4. Om fler än fyra VMs resursen hello samma molntjänst, och konfigurera flera principer för säkerhetskopiering toostage hello säkerhetskopieringstiden så inga fler än fyra VM-säkerhetskopieringar startas på hello samtidigt. Försök toospread hello säkerhetskopiering starttider timmen mellanrum mellan principer.
5. Virtuell dator körs på hög CPU/minne.<br>
   Om hello virtuella datorn körs på hög CPU usage(>90%) eller minne, ögonblicksbilden i kö, fördröjd och kommer så småningom hämtar timeout. Försök säkerhetskopiering på begäran i sådana situationer.

<br>

## <a name="networking"></a>Nätverk
Säkerhetskopiering tillägg måste komma åt toohello offentliga internet toowork som alla tillägg. Inte har åtkomst toohello kan offentliga internet inträffa på olika sätt:

* hello tillägget installationen kan misslyckas
* hello säkerhetskopieringsåtgärder (t.ex. disk snapshot) kan misslyckas
* Visa hello status för hello säkerhetskopieringen kan misslyckas

hello behöver för att lösa offentliga internet-adresser har tagits Ledad [här](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Du behöver toocheck hello DNS-konfigurationer för hello VNET och se till att hello Azure URI: er kan lösas.

När hello namnmatchning utförs korrekt, måste toobe tillhandahålls också åtkomst toohello Azure IP-adresser. toounblock åtkomst toohello Azure-infrastrukturen, gör du något av följande:

1. Godkända hello Azure-datacenter IP-adressintervall.
   * Hämta hello lista över [Azure-datacenter IP-adresser](https://www.microsoft.com/download/details.aspx?id=41653) toobe godkända.
   * Avblockera hello IP-adresser med hjälp av hello [ny NetRoute](https://technet.microsoft.com/library/hh826148.aspx) cmdlet. Kör denna cmdlet inom hello Azure VM, i ett upphöjt PowerShell-fönster (Kör som administratör).
   * Lägg till regler toohello NSG (om du har en plats) tooallow åtkomst toohello IP-adresser.
2. Skapa en sökväg för HTTP-trafik tooflow
   * Om du har några nätverksbegränsning på plats (en Nätverkssäkerhetsgrupp, till exempel) att distribuera en HTTP-proxy server tooroute hello trafik. Steg toodeploy en HTTP-proxyserver kan hitta [här](backup-azure-vms-prepare.md#network-connectivity).
   * Lägg till regler toohello NSG (om du har en plats) tooallow åtkomst toohello INTERNET från hello HTTP-Proxy.

> [!NOTE]
> DHCP måste vara aktiverat i hello gästen för IaaS säkerhetskopiering toowork.  Om du behöver en statisk privat IP-adress kan konfigurera du det via hello-plattformen. hello DHCP-alternativ i hello VM ska lämnas aktiverad.
> Visa mer information om [ange en statisk IP för interna privata](../virtual-network/virtual-networks-reserved-private-ip.md).
>
>
