---
title: aaaUse Azure Backup Server tooback in arbetsbelastningar tooAzure klassiska portal | Microsoft Docs
description: "Kontrollera att din miljö är korrekt förberedd tooback in arbetsbelastningar med Azure Backup Server"
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
keywords: "Azure backup-servern. säkerhetskopieringsvalvet"
ms.assetid: d86450e8-da63-4283-8fd7-2ee629fa14ab
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: masaran;trinadhk;pullabhk;markgal
ms.openlocfilehash: 7b574824c448096e0c0ba74a872ab8f2a434f6a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-tooback-up-workloads-using-azure-backup-server"></a>Förbereda tooback in arbetsbelastningar med Azure Backup Server
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Azure Backup-Server (klassisk)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klassisk)](backup-azure-dpm-introduction-classic.md)
>
>

Den här artikeln handlar om hur du förbereder din miljö tooback in arbetsbelastningar med Azure Backup Server. Med Azure Backup Server, kan du skydda arbetsbelastningar för program som Hyper-V virtuella datorer, Microsoft SQL Server, SharePoint Server, Microsoft Exchange och Windows-klienter från en enda konsol.

> [!WARNING]
> Azure Backup Server ärver hello funktioner för Data Protection Manager (DPM) för att säkerhetskopiera arbetsbelastningar. Du hittar pekare tooDPM dokumentation för några av dessa funktioner. Azure Backup Server dock inte ge skydd på band eller integrera med System Center.
>
>

## <a name="1-windows-server-machine"></a>1. Windows Server-dator
![steg 1](./media/backup-azure-microsoft-azure-backup/step1.png)

hello första steg för att hämta hello Azure Backup Server och kör är toohave en dator med Windows Server.

| Plats | Minimikrav | Ytterligare instruktioner |
| --- | --- | --- |
| Azure |Virtuell Azure IaaS-dator<br><br>A2 Standard: 2 kärnor, 3.5GB RAM-minne |Du kan börja med en enkel galleriet bild av Windows Server 2012 R2 Datacenter. [Skydda IaaS-arbetsbelastningar med Azure Backup Server (DPM)](https://technet.microsoft.com/library/jj852163.aspx) har många olika delarna. Se till att du läser hello artikel helt innan du distribuerar hello-datorn. |
| Lokal |Hyper-V-dator<br> VMWare-dator<br> eller en fysisk värd<br><br>2 kärnor och 4GB RAM-minne |Du kan deduplicera hello DPM-lagring med hjälp av Windows Server Deduplicering. Läs mer om hur [DPM och deduplicering](https://technet.microsoft.com/library/dn891438.aspx) tillsammans när de distribueras i Hyper-V virtuella datorer. |

> [!NOTE]
> Du rekommenderas att Azure Backup Server installeras på en dator med Windows Server 2012 R2 Datacenter. En mängd hello krav omfattas automatiskt med hello senaste versionen av operativsystemet för Windows hello.
>
>

Om du planerar toojoin Azure Backup Server tooa domän, bör du ansluta hello fysisk server eller virtuell dator toohello domän innan du installerar hello Azure Backup Server-programvara. Flytta ett Azure Backup Server tooa ny domän, efter distributionen är *stöds inte*.

## <a name="2-backup-vault"></a>2. Säkerhetskopieringsvalvet
![steg 2](./media/backup-azure-microsoft-azure-backup/step2.png)

Om du skickar säkerhetskopieringsdata tooAzure eller se till att den lokalt måste hello Azure Backup-Server vara registrerade tooa valvet. Om du har använt Azure Backup och vill toouse Azure Backup Server, se hello Azure portal versionen av den här artikeln - [förbereda tooback in arbetsbelastningar med Azure Backup Server](backup-azure-microsoft-azure-backup.md).

> [!IMPORTANT]
> Från mars 2017 kan använda du inte längre hello klassiska portal toocreate säkerhetskopieringsvalv.
> Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv. Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.<br/> Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017. **Från den 1 november 2017**:
>- Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.
>- Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen. Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.
>



## <a name="3-software-package"></a>3. Programpaket
![steg 3](./media/backup-azure-microsoft-azure-backup/step3.png)

### <a name="downloading-hello-software-package"></a>Hämta hello programpaket
Liknande toovault autentiseringsuppgifter, kan du hämta Microsoft Azure Backup för programbelastningar från hello **snabb Start Page** av hello säkerhetskopieringsvalvet.

1. Klicka på **för programbelastningar (Disk tooDisk tooCloud)**. Detta tar toohello Download Center sida från där du kan hämta hello programpaket.

    ![Microsoft Azure Backup välkomstskärmen](./media/backup-azure-microsoft-azure-backup/dpm-venus1.png)
2. Klicka på **Hämta**.

    ![Hämta center 1](./media/backup-azure-microsoft-azure-backup/downloadcenter1.png)
3. Välj alla hello-filer och klicka på **nästa**. Hämta alla hello filer som kommer från hello hämtningssidan för Microsoft Azure Backup och placera alla hello filer i hello samma mapp.
   ![Hämta center 1](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    Eftersom hello hämtningsstorleken för alla hello filer tillsammans > 3G, på en länk för hämtning av 10 Mbit/s som det kan ta upp too60 ned minuter för hello toocomplete.

### <a name="extracting-hello-software-package"></a>Extrahera hello programpaket
När du har laddat ned alla hello-filer, klickar du på **MicrosoftAzureBackupInstaller.exe**. Detta startar hello **installationsguiden för Microsoft Azure Backup** tooextract hello installationsfilerna tooa plats som anges av du. Fortsätta hello guiden och klicka på hello **extrahera** knappen toobegin hello extraheringsprocessen.

> [!WARNING]
> Minst 4GB ledigt utrymme är obligatoriska tooextract hello-installationsfilerna.
>
>

![Installationsguiden för Microsoft Azure Backup](./media/backup-azure-microsoft-azure-backup/extract/03.png)

En gång hello extraheringsprocessen är slutförd, kontrollera hello rutan toolaunch hello nyligen extraherade *setup.exe* toobegin installerar Microsoft Azure Backup Server och klicka på hello **Slutför** knappen.

### <a name="installing-hello-software-package"></a>Installera hello programpaket
1. Klicka på **Microsoft Azure Backup** toolaunch hello-installationsguiden.

    ![Installationsguiden för Microsoft Azure Backup](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. Klicka på välkomstskärmen hello hello **nästa** knappen. Detta tar toohello *nödvändiga kontrollerar* avsnitt. På den här skärmen klickar du på hello **Kontrollera** knappen toodetermine om hello maskin- och programvarukraven för Azure Backup Server har uppfyllts. Om alla krav är hello har har uppfyllts, visas ett meddelande som anger att hello-datorn uppfyller hello. Klicka på hello **nästa** knappen.

    ![Kontrollera om Azure Backup Server - Välkommen och förutsättningar](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Microsoft Azure Backup Server kräver SQL Server Standard och medföljer hello Azure Backup Server installationspaketet med hello lämplig SQL Server-binärfiler behövs. När du börjar med en ny installation av Azure Backup Server du bör välja alternativet hello **installera nya SQL Server-instans med de här inställningarna** och klicka på hello **kontrollera och installera** knappen. När hello nödvändiga komponenter har installerats klickar du på **nästa**.

    ![Azure Backup-Server - kontroll av SQL](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Om det uppstår ett fel med en rekommendation toorestart hello dator, göra det och klickar på **kontrollen igen**.

   > [!NOTE]
   > Azure Backup Server fungerar inte med en fjärrinstans av SQL Server. hello-instans som används av Azure Backup Server måste toobe lokala.
   >
   >

4. Ange en plats för hello installation av Microsoft Azure Backup serverfiler och klicka på **nästa**.

    ![Microsoft Azure Backup PreReq2](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    hello virtuellt plats är ett krav för säkerhetskopiering av tooAzure. Kontrollera hello tillfälliga platsen är minst 5% av hello data planerade toobe säkerhetskopierade toohello moln. För skydd på disk måste separata diskar toobe konfigurerats när hello installationen är klar. Mer information om lagringspooler finns [konfigurera lagringspooler och disklagring](https://technet.microsoft.com/library/hh758075.aspx).
5. Ange ett starkt lösenord för begränsade lokala användarkonton och klicka på **nästa**.

    ![Microsoft Azure Backup PreReq2](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Välj om du vill toouse *Microsoft Update* toocheck för uppdateringar och klickar på **nästa**.

   > [!NOTE]
   > Vi rekommenderar att du har Windows Update omdirigera tooMicrosoft uppdatering, vilket ger säkerhet och viktiga uppdateringar för Windows och andra produkter som Microsoft Azure Backup-Server.
   >
   >

    ![Microsoft Azure Backup PreReq2](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. Granska hello *inställningar för sammanfattning av* och på **installera**.

    ![Microsoft Azure Backup PreReq2](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. hello installationen sker i faser. I hello första fas hello installeras Microsoft Azure Recovery Services-agenten på hello-servern. hello guiden kontrollerar också Internet-anslutningen. Om Internet-anslutning är tillgänglig kan du fortsätta med installationen, annars måste tooprovide proxy information tooconnect toohello Internet.

    hello nästa steg är tooconfigure hello Microsoft Azure Recovery Services-agenten. Som en del av hello konfigurationen har du tooprovide din hello valvet autentiseringsuppgifter tooregister hello datorn toohello säkerhetskopieringsvalvet. Du kan också ange en lösenfras tooencrypt/dekryptera hello-data som skickas mellan Azure och ditt lokala. Du kan automatiskt generera en lösenfras eller ange egna minst 16 tecken lösenfras. Fortsätt med hello guiden tills hello agenten har konfigurerats.

    ![Azure Backup Serer PreReq2](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. När du har slutfört registreringen av hello Microsoft Azure Backup server fortsätter hello övergripande installationsguiden toohello installationen och konfigurationen av SQL Server och hello Azure Backup Server-komponenter. När hello komponentinstallation för SQL Server är klar installeras hello Azure Backup Server.

    ![Azure Backup Server](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

När hello installationssteget har slutförts kommer hello produktens skrivbordsikoner har skapats samt. Dubbelklickar du på hello ikonen toolaunch hello produkten.

### <a name="add-backup-storage"></a>Lägg till lagring för säkerhetskopiering
hello första säkerhetskopian sparas på lagringsplats ansluten toohello Azure Backup-Server-datorn. Läs mer om att lägga till diskar i [konfigurera lagringspooler och disklagring](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> Du måste tooadd säkerhetskopieringslagring även om du planerar toosend data tooAzure. Hello aktuella arkitekturen för Azure Backup-Server, hello Azure Backup-valvet har hello *andra* kopia av hello data medan hello lokal lagring innehåller hello första (och obligatoriska) säkerhetskopia.  
>
>

## <a name="4-network-connectivity"></a>4. Nätverksanslutning
![step4](./media/backup-azure-microsoft-azure-backup/step4.png)

Azure Backup-servern kräver anslutning toohello Azure Backup-tjänsten för hello produkten toowork har. toovalidate om hello datorn har hello anslutningen tooAzure använda hello ```Get-DPMCloudConnection``` cmdlet i hello Azure Backup Server PowerShell-konsolen. Om hello utdata från kommandot hello är TRUE och anslutning finns, eller det finns ingen nätverksanslutning.

Vid hello måste samtidigt, hello Azure-prenumeration toobe i felfritt tillstånd. toofind ut hello tillståndet för din prenumeration och toomanage den inloggningen toohello [prenumeration portal](https://account.windowsazure.com/Subscriptions).

När du vet hello tillstånd av hello Azure anslutning och hello Azure-prenumeration kan använda du hello tabellen nedan toofind ut hello inverkan på hello säkerhetskopiering/återställning funktionerna som erbjuds.

| Tillstånd för anslutning | Azure-prenumeration | Säkerhetskopiering tooAzure | Säkerhetskopiering toodisk | Återställa från Azure | Återställa från disken |
| --- | --- | --- | --- | --- | --- |
| Ansluten |Active |Tillåtna |Tillåtna |Tillåtna |Tillåtna |
| Ansluten |Upphört att gälla |Stoppad |Stoppad |Tillåtna |Tillåtna |
| Ansluten |Avetableras |Stoppad |Stoppad |Stoppad och Azure återställningspunkter tas bort |Stoppad |
| Förlorade anslutningen > 15 dagar |Active |Stoppad |Stoppad |Tillåtna |Tillåtna |
| Förlorade anslutningen > 15 dagar |Upphört att gälla |Stoppad |Stoppad |Tillåtna |Tillåtna |
| Förlorade anslutningen > 15 dagar |Avetableras |Stoppad |Stoppad |Stoppad och Azure återställningspunkter tas bort |Stoppad |

### <a name="recovering-from-loss-of-connectivity"></a>Återställa från anslutningsproblem
Om du har en brandvägg eller en proxy som hindrar åtkomst tooAzure måste toowhitelist hello efter domän adresser i hello brandvägg/proxy-profil:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

När anslutningen tooAzure har återställts toohello Azure Backup Server-datorn, bestäms hello-åtgärder som kan utföras av hello Azure-prenumeration tillstånd. hello tabellen ovan innehåller information om tillåtna när hello är ”ansluten” hello-åtgärder.

### <a name="handling-subscription-states"></a>Hantera prenumerationstillstånd
Det är möjligt tootake en Azure-prenumeration från en *utgångna* eller *Deprovisioned* tillstånd toohello *Active* tillstånd. Men det har vissa effekter på hello produkten beteende hello tillstånd är inte *Active*:

* En *Deprovisioned* prenumeration förlorar funktioner för hello period som den är avetableras. För att aktivera *Active*, hello funktionalitet för säkerhetskopiering/återställning återställas. hello säkerhetskopieringsdata på hello lokal disk kan också hämtas om det var hålls med en tillräckligt lång Bevarandeperiod. Hello säkerhetskopierade data i Azure är dock oåterkalleligen förlorade när hello prenumerationen anger hello *Deprovisioned* tillstånd.
* En *utgångna* prenumeration förlorar endast funktioner för tills det har gjorts *Active* igen. Alla schemalagda säkerhetskopieringar för hello period som hello prenumeration har *utgångna* körs inte.

## <a name="troubleshooting"></a>Felsökning
Om Microsoft Azure Backup-servern misslyckas med fel under installationsfasen hello (eller säkerhetskopiering eller återställning), se toothis [fel koder dokumentet](https://support.microsoft.com/kb/3041338) för mer information.
Du kan också se för[Azure Backup relaterade vanliga frågor och svar](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Nästa steg
Du kan få detaljerad information om [förbereda din miljö för DPM](https://technet.microsoft.com/library/hh758176.aspx) på hello Microsoft TechNet-webbplatsen. Det innehåller även information om konfigurationer som stöds med Azure Backup Server kan distribueras och används.

Du kan använda dessa artiklar toogain en djupare förståelse för skydd av arbetsbelastning med Microsoft Azure Backup-server.

* [SQL Server-säkerhetskopiering](backup-azure-backup-sql.md)
* [SharePoint server-säkerhetskopiering](backup-azure-backup-sharepoint.md)
* [Alternativ server-säkerhetskopiering](backup-azure-alternate-dpm-server.md)
