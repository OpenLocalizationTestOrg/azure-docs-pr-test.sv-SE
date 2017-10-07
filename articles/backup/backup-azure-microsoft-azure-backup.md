---
title: aaaUse Azure Backup Server tooback in arbetsbelastningar tooAzure | Microsoft Docs
description: "Använda Azure Backup Server tooprotect eller säkerhetskopiera arbetsbelastningar toohello Azure-portalen."
services: backup
documentationcenter: 
author: PVRK
manager: shivamg
editor: 
keywords: "Azure backup-servern. skydda arbetsbelastningar; Säkerhetskopiera arbetsbelastningar"
ms.assetid: e7fb1907-9dc1-4ca1-8c61-50423d86540c
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/20/2017
ms.author: masaran;trinadhk;pullabhk;markgal
ms.openlocfilehash: a99b2919ffd44c6133960e3a935038a2bb1281c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-tooback-up-workloads-using-azure-backup-server"></a>Förbereda tooback in arbetsbelastningar med Azure Backup Server
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

Den här artikeln förklarar hur tooprepare din miljö tooback in arbetsbelastningar med Azure Backup Server. Med Azure Backup Server, kan du skydda arbetsbelastningar för program som Hyper-V virtuella datorer, Microsoft SQL Server, SharePoint Server, Microsoft Exchange och Windows-klienter från en enda konsol.

> [!NOTE]
> Azure Backup-Server kan nu att skydda virtuella VMware-datorer och ger funktioner för ökad säkerhet. Installera hello produkten enligt beskrivningen i hello avsnitten nedan. Använd uppdatering 1 och hello senaste Azure Backup-agenten. toolearn mer information om hur du säkerhetskopierar VMware-servrar med Azure Backup Server, finns hello artikeln [Använd Azure Backup Server tooback in en VMware-server](backup-azure-backup-server-vmware.md). toolearn om säkerhetsfunktioner, referera för[Azure backup säkerhetsfunktioner dokumentationen](backup-azure-security-feature.md).
>
>

Du kan även skydda infrastruktur som en tjänst (IaaS) arbetsbelastningar som till exempel virtuella datorer i Azure.

> [!NOTE]
> Azure har två distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln innehåller hello information och procedurer för att återställa virtuella datorer som distribueras med hjälp av hello Resource Manager-modellen.
>
>

Azure Backup Server ärver många hello arbetsbelastning säkerhetskopiering funktioner från Data Protection Manager (DPM). Den här artikeln länkar tooDPM dokumentationen tooexplain vissa av hello Delade funktioner. Även om Azure Backup Server delar mycket hello samma funktioner som DPM. Azure Backup-servern inte säkerhetskopiera tootape eller integreras den med System Center.

## <a name="1-choose-an-installation-platform"></a>1. Välj en installationsplattform
hello första steg för att hämta hello Azure Backup Server och kör är tooset upp en Windows-Server. Servern kan vara i Azure eller lokalt.

### <a name="using-a-server-in-azure"></a>Använder en server i Azure
När du väljer en server för att köra Azure Backup Server, bör du börja med en bild i galleriet för Windows Server 2012 R2 Datacenter. hello artikel [skapa din första Windows virtuell dator i hello Azure-portalen](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), innehåller en vägledning för att komma igång med hello rekommenderas virtuell dator i Azure, även om du aldrig har använt Azure förut. hello rekommenderade minimikraven för en virtuell dator (VM) med hello server måste vara: A2 Standard med två kärnor och 3.5 GB RAM.

Skydda arbetsbelastningar med Azure Backup Server har många olika delarna. hello artikel [installera DPM som en virtuell Azure-dator](https://technet.microsoft.com/library/jj852163.aspx), hjälper beskrivs de olika delarna. Innan du distribuerar hello dator den här artikeln helt.

### <a name="using-an-on-premises-server"></a>Med hjälp av en lokal server
Om du inte vill att toorun hello base server i Azure kan du köra hello server på en Hyper-V virtuell dator, en VMware-VM eller en fysisk värd. hello bör minimikraven för maskinvara för hello-servern finns två kärnor och 4 GB RAM-minne. hello stöds operativsystem finns i hello följande tabell:

| Operativsystem | Plattform | SKU |
|:--- | --- |:--- |
| Windows Server 2012 R2 och senaste Service Pack |64-bitars |Standard, Datacenter, Foundation |
| Windows Server 2012 och senaste Service Pack |64-bitars |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 och senaste Service Pack |64-bitars |Standard, Workgroup |
| Windows Storage Server 2012 och senaste Service Pack |64-bitars |Standard, Workgroup |

Du kan deduplicera hello DPM-lagring med hjälp av Windows Server Deduplicering. Läs mer om hur [DPM och deduplicering](https://technet.microsoft.com/library/dn891438.aspx) tillsammans när de distribueras i Hyper-V virtuella datorer.

> [!NOTE]
> Azure Backup-Server är utformad toorun på en dedikerad server med enda syfte. Du kan inte installera Azure Backup Server på:
> - En dator som körs som en domänkontrollant
> - En dator på vilken hello Application Server-rollen är installerad
> - En dator som är en System Center Operations Manager-hanteringsserver
> - En dator som kör Exchange Server
> - En dator som är en nod i ett kluster

Alltid ansluta till Azure Backup Server tooa domän. Om du planerar toomove hello-tooa olika domän, bör du ansluta hello-toohello nya domän innan du installerar Azure Backup Server. Flytta en befintlig Azure Backup Server datorn tooa ny domän när distributionen är *stöds inte*.

## <a name="2-recovery-services-vault"></a>2. Recovery Services-valv
Om du skickar säkerhetskopieringsdata tooAzure eller se till att den lokalt, måste programmet hello toobe anslutna tooAzure. toobe som är mer specifik hello Azure Backup Server-datorn måste toobe som registrerats med en recovery services-valvet.

toocreate en recovery services-valv:

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hej hubbmenyn, klicka på **Bläddra** och Skriv i hello lista över resurser, **återställningstjänster**. När du börjar skriva in hello listan filtreras baserat på dina indata. Klicka på **Recovery Services-valv**.

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png) <br/>

    hello lista över Recovery Services-valv visas.
3. På hello **Recovery Services-valv** -menyn klickar du på **Lägg till**.

    ![Skapa Recovery Services-valv (steg 2)](./media/backup-azure-microsoft-azure-backup/rs-vault-menu.png)

    hello återställningstjänster valvet blad öppnas, där du uppmanas tooprovide en **namn**, **prenumeration**, **resursgruppen**, och **plats**.

    ![Skapa ett Recovery Services-valv (steg 5)](./media/backup-azure-microsoft-azure-backup/rs-vault-attributes.png)
4. För **namn**, ange ett eget namn tooidentify hello valv. hello namn måste toobe unika för hello Azure-prenumeration. Skriv ett namn som innehåller mellan 2 och 50 tecken. Det måste börja med en bokstav och får endast innehålla bokstäver, siffror och bindestreck.
5. Klicka på **prenumeration** toosee hello tillgängliga listan över prenumerationer. Om du inte är säker på vilken prenumeration toouse använder standard-hello (eller förslag) prenumerationen. Du kan bara välja mellan flera alternativ om ditt organisationskonto är associerat med flera Azure-prenumerationer.
6. Klicka på **resursgruppen** toosee hello tillgängliga listan över resursgrupper, eller klicka på **ny** toocreate en ny resursgrupp. För komplett information om resursgrupper, se [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
7. Klicka på **plats** tooselect hello geografiskt område för hello-valvet.
8. Klicka på **Skapa**. Det kan ta en stund innan hello Recovery Services-valvet toobe skapas. Övervaka hello statusmeddelanden i övre högra delen hello hello-portalen.
   När du har skapat ditt valv öppnas i hello-portalen.

### <a name="set-storage-replication"></a>Konfigurera lagringsreplikering
hello lagringsalternativ för replikering kan du toochoose mellan geo-redundant lagring och lokalt redundant lagring. Valvet använder geo-redundant lagring som standard. Om det här valvet är ditt primära valv lämna hello lagring alternativet set toogeo-redundant lagring. Välj lokalt redundant lagring om du vill använda ett billigare alternativ som inte är lika beständigt. Läs mer om [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) och [lokalt redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) lagringsalternativ i hello [Azure Storage-replikering: översikt](../storage/common/storage-redundancy.md).

replikeringsinställningen för tooedit hello lagring:

1. Välj valvet tooopen hello valvet instrumentpanelen och hello inställningsbladet. Om hello **inställningar** bladet inte öppnas, klickar du på **alla inställningar** hello valvet instrumentpanelen.
2. På hello **inställningar** bladet, klickar du på **säkerhetskopiering infrastruktur** > **konfigurering av säkerhetskopiering** tooopen hello **konfigurering av säkerhetskopiering** bladet. På hello **konfigurering av säkerhetskopiering** bladet välj hello replikering lagringsalternativ för ditt valv.

    ![Lista över säkerhetskopieringsvalv](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    När du väljer hello lagringsalternativ för ditt valv, är du redo tooassociate hello VM hello-valvet. toobegin hello association, ska du identifiera och registrera hello Azure virtuella datorer.

## <a name="3-software-package"></a>3. Programpaket
### <a name="downloading-hello-software-package"></a>Hämta hello programpaket
1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Om du redan har ett Recovery Services-valv öppna fortsätta toostep 3. Om du inte behöver öppna en Recovery Services-valv men i hello Azure-portalen på hello hubbmenyn, klicka på **Bläddra**.

   * Skriv i hello lista över resurser, **återställningstjänster**.
   * När du börjar skriva in hello listan filtreras baserat på dina indata. När du ser **Recovery Services-valv** klickar du på det.

     ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png)

     hello visas Recovery Services-valv.
   * Välj ett valv hello listan av Recovery Services-valv.

     hello valda valvet instrumentpanelen öppnas.

     ![Öppna bladet för valvet](./media/backup-azure-microsoft-azure-backup/vault-dashboard.png)
3. Hej **inställningar** bladet som öppnas som standard. Om den är stängd klickar du på **inställningar** tooopen hello inställningsbladet.

    ![Öppna bladet för valvet](./media/backup-azure-microsoft-azure-backup/vault-setting.png)
4. Klicka på **säkerhetskopiering** tooopen hello komma igång-guiden.

    ![Säkerhetskopiering komma igång](./media/backup-azure-microsoft-azure-backup/getting-started-backup.png)

    I hello **Kom igång med säkerhetskopiering** bladet som öppnas, **säkerhetskopiering mål** blir automatiskt markerad.

    ![Backup-mål-standard-öppnas](./media/backup-azure-microsoft-azure-backup/getting-started.png)

5. I hello **säkerhetskopiering målet** bladet från hello **var körs din arbetsbelastning** väljer du **lokalt**.

    ![lokalt och arbetsbelastningar som mål](./media/backup-azure-microsoft-azure-backup/backup-goals-azure-backup-server.png)

    Från hello **vad vill du vill toobackup?** nedrullningsbara menyn, Välj hello arbetsbelastningar du tooprotect med hjälp av Azure Backup Server, och klickar sedan på **OK**.

    Hej **Kom igång med säkerhetskopiering** guiden växlar hello **Förbered infrastruktur** alternativet tooback in arbetsbelastningar tooAzure.

   > [!NOTE]
   > Om du bara vill tooback av filer och mappar, rekommenderar vi använder hello Azure Backup-agenten och följa hello riktlinjerna i hello artikel [förhandstitt: säkerhetskopiera filer och mappar](backup-try-azure-backup-in-10-mins.md). Om du ska tooprotect behöver mer än filer och mappar eller du planerar tooexpand hello skydd i hello framtiden, Välj dessa arbetsbelastningar.
   >
   >

    ![Komma igång-guiden Ändra](./media/backup-azure-microsoft-azure-backup/getting-started-prep-infra.png)

6. I hello **Förbered infrastruktur** bladet som öppnas, klickar du på hello **hämta** länkar för att installera Azure Backup-Server och ladda ned valvautentiseringsuppgifter. Du kan använda hello valvautentiseringsuppgifter under registrering av Azure Backup Server toohello recovery services-valvet. hello länkar tar toohello Download Center där hello programpaket kan hämtas.

    ![Förbereda infrastrukturen för Azure Backup Server](./media/backup-azure-microsoft-azure-backup/azure-backup-server-prep-infra.png)

7. Välj alla hello-filer och klicka på **nästa**. Hämta alla hello filer som kommer från hello hämtningssidan för Microsoft Azure Backup och placera alla hello filer i hello samma mapp.

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
2. Klicka på välkomstskärmen hello hello **nästa** knappen. Detta tar toohello *nödvändiga kontrollerar* avsnitt. Klicka på den här skärmen **Kontrollera** toodetermine om hello maskin- och programvarukraven för Azure Backup Server har uppfyllts. Om alla krav är uppfyllda har, visas ett meddelande som anger att hello-datorn uppfyller hello. Klicka på hello **nästa** knappen.

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

    hello nästa steg är tooconfigure hello Microsoft Azure Recovery Services-agenten. Som en del av hello configuration behöver tooprovide din valvet autentiseringsuppgifter tooregister hello datorn toohello återställningstjänster valvet. Du kan också ange en lösenfras tooencrypt/dekryptera hello-data som skickas mellan Azure och ditt lokala. Du kan automatiskt generera en lösenfras eller ange egna minst 16 tecken lösenfras. Fortsätt med hello guiden tills hello agenten har konfigurerats.

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
Azure Backup-servern kräver anslutning toohello Azure Backup-tjänsten för hello produkten toowork har. toovalidate om hello datorn har hello anslutningen tooAzure använda hello ```Get-DPMCloudConnection``` cmdlet i hello Azure Backup Server PowerShell-konsolen. Om hello utdata från cmdleten hello är TRUE och anslutning finns, eller det finns ingen nätverksanslutning.

Vid hello måste samtidigt, hello Azure-prenumeration toobe i felfritt tillstånd. toofind ut hello tillståndet för din prenumeration och toomanage den inloggningen toohello [prenumeration portal](https://account.windowsazure.com/Subscriptions).

När du vet hello tillstånd av hello Azure anslutning och hello Azure-prenumeration kan använda du hello tabellen nedan toofind ut hello inverkan på hello säkerhetskopiering/återställning funktionerna som erbjuds.

| Tillstånd för anslutning | Azure-prenumeration | Säkerhetskopiera tooAzure | Säkerhetskopiera toodisk | Återställa från Azure | Återställa från disken |
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
