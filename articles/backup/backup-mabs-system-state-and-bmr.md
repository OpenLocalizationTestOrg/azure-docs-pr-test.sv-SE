---
title: "aaaAzure Backup Server skyddar Systemtillstånd och återställer toobare operativsystem | Microsoft Docs"
description: "Använda Azure Backup Server tooback upp din Systemtillstånd och bare metal recovery (BMR) skydd."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
keywords: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.targetplatform: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: markgal,masaran
ms.openlocfilehash: d34c8bbdc7cc24c905f81ceaf199698c1ee923db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-system-state-and-restore-toobare-metal-with-azure-backup-server"></a>Säkerhetskopiera systemtillståndet och återställa toobare operativsystem med Azure Backup Server

Azure Backup Server säkerhetskopierar Systemtillstånd och bare metal recovery (BMR) skyddar.

*   **Säkerhetskopiering av systemtillstånd**: säkerhetskopierar operativsystemfiler, så kan du återställa när datorn startar, men systemfiler och registret hello går förlorade. En säkerhetskopia av systemtillståndet innehåller:
    * Domänmedlem: starta filer, registreringsdatabas för COM +-klassen, registret
    * Domänkontrollant: Windows Server Active Directory (NTDS), starta filer, registreringsdatabas för COM +-klassen, registret, systemvolymen (SYSVOL)
    * Dator som kör klustertjänster: klusterserverns metadata
    * Dator som kör Certifikattjänster: certifikatet data
* **Bare metal-säkerhetskopiering**: säkerhetskopierar operativsystemfiler och alla data på kritiska volymer (utom användardata). Per definition erbjuder omfattar en säkerhetskopiering av BMR en säkerhetskopia av systemtillståndet. Det ger skydd när du har toorecover allt och en dator inte startar.

hello följande tabell sammanfattar vad du kan säkerhetskopiera och återställa. Detaljerad information om appversioner som kan skyddas med Systemtillstånd och BMR finns [vad är Azure Backup Server säkerhetskopiera?](backup-mabs-protection-matrix.md).

|Säkerhetskopiering|Problem|Återställa från säkerhetskopia för Azure Backup-Server|Återställa från säkerhetskopia av systemtillståndet|BMR|
|----------|---------|---------------------------|------------------------------------|-------|
|**Fildata**<br /><br />Regelbunden säkerhetskopiering<br /><br />BMR/systemtillstånd|Förlorade fildata|Y|N|N|
|**Fildata**<br /><br />Azure Backup-Server-säkerhetskopiering av fildata<br /><br />BMR/systemtillstånd|Saknad eller skadad operativsystem|N|Y|Y|
|**Fildata**<br /><br />Azure Backup-Server-säkerhetskopiering av fildata<br /><br />BMR/systemtillstånd|Förlorad server (datavolymer orörda)|N|N|Y|
|**Fildata**<br /><br />Azure Backup-Server-säkerhetskopiering av fildata<br /><br />BMR/systemtillstånd|Förlorad server (datavolymer tappas bort)|Y|Nej|Ja (BMR, följt av reguljära återställning av säkerhetskopierade filen data)|
|**SharePoint-data**:<br /><br />Azure Backup-Server-säkerhetskopiering av servergruppdata<br /><br />BMR/systemtillstånd|Förlorade plats, listor, listobjekt och dokument|Y|N|N|
|**SharePoint-data**:<br /><br />Azure Backup-Server-säkerhetskopiering av servergruppdata<br /><br />BMR/systemtillstånd|Saknad eller skadad operativsystem|N|Y|Y|
|**SharePoint-data**:<br /><br />Azure Backup-Server-säkerhetskopiering av servergruppdata<br /><br />BMR/systemtillstånd|Haveriberedskap|N|N|N|
|Windows Server 2012 R2 Hyper-V<br /><br />Azure Backup-Server-säkerhetskopiering av Hyper-V-värd eller gäst<br /><br />BMR/säkerhetskopia av systemtillståndet på värden|Förlorade VM|Y|N|N|
|Hyper-V<br /><br />Azure Backup-Server-säkerhetskopiering av Hyper-V-värd eller gäst<br /><br />BMR/säkerhetskopia av systemtillståndet på värden|Saknad eller skadad operativsystem|N|Y|Y|
|Hyper-V<br /><br />Azure Backup-Server-säkerhetskopiering av Hyper-V-värd eller gäst<br /><br />BMR/säkerhetskopia av systemtillståndet på värden|Förlorad Hyper-V-värd (virtuella datorer orörda)|N|N|Y|
|Hyper-V<br /><br />Azure Backup-Server-säkerhetskopiering av Hyper-V-värd eller gäst<br /><br />BMR/säkerhetskopia av systemtillståndet på värden|Förlorad Hyper-V-värden (VMs tappas bort)|N|N|Y<br /><br />BMR, följt av reguljära återställning av Azure Backup Server|
|SQL Server Exchange<br /><br />Azure Backup-Server app-säkerhetskopiering<br /><br />BMR/systemtillstånd|Förlorade AppData|Y|N|N|
|SQL Server Exchange<br /><br />Azure Backup-Server app-säkerhetskopiering<br /><br />BMR/systemtillstånd|Saknad eller skadad operativsystem|N|Y|Y|
|SQL Server Exchange<br /><br />Azure Backup-Server app-säkerhetskopiering<br /><br />BMR/systemtillstånd|Förlorad server (databas/transaktionsloggar orörda)|N|N|Y|
|SQL Server Exchange<br /><br />Azure Backup-Server app-säkerhetskopiering<br /><br />BMR/systemtillstånd|Förlorad server (databas/transaktionsloggar tappas bort)|N|N|Y<br /><br />BMR-återställning, följt av reguljära återställning av Azure Backup Server|

## <a name="how-system-state-backup-works"></a>Så här fungerar systemtillstånd

När en säkerhetskopiering av systemtillstånd körs, säkerhetskopiera servern kommunicerar med Windows Server Backup toorequest en säkerhetskopia av systemtillståndet hello-servern. Som standard använder hello-enhet som har mest tillgängligt ledigt utrymme för hello av Backup Server och Windows Server Backup. Information om den här enheten har sparats i hello PSDataSourceConfig.xml-filen. Detta är hello-enhet som Windows Server Backup använder för säkerhetskopiering.

Du kan anpassa hello-enhet som Backup-servern använder för hello säkerhetskopia av systemtillståndet. Gå tooC:\Program Files\Microsoft Data Protection Manager\MABS\Datasources på hello skyddad server. Öppna filen för hello PSDataSourceConfig.xml för redigering. Ändra hello \<FilesToProtect\> värde för hello enhetsbeteckning. Spara och Stäng hello-filen. Om det finns en skyddsgrupp anger tooprotect hello systemtillstånd hello dator, kör du en konsekvenskontroll. Om en avisering genereras väljer **ändra skyddsgrupp** i hello aviseringen och sedan utför hello guiden. Kör sedan en annan konsekvenskontroll.

Observera att om hello skydd servern är i ett kluster, är det möjligt att en enhet för kluster markeras som hello enheten med mest ledigt utrymme för hello. Om ägarskap som enheten har växlat tooanother nod och ett systemtillstånd körs, hello-enheten är inte tillgänglig och hello säkerhetskopiering misslyckas. I det här scenariot, ändra PSDataSourceConfig.xml toopoint tooa lokal enhet.

Windows Server Backup skapar sedan en mapp som kallas WindowsImageBackup i hello återställningsmapp hello rot. Eftersom Windows Server Backup skapar hello säkerhetskopiering, placeras alla hello data i den här mappen. När hello säkerhetskopieringen är klar är hello filen överförda toohello Backup Server-datorn. Observera hello följande information:

* Den här mappen och dess innehåll rensas inte när hello säkerhetskopieringen eller överföringen är klar. Hej toothink på bästa sätt på detta är att hello utrymme som ska reserveras för hello nästa gång en säkerhetskopia är klar.
* hello mappen skapas varje gång en säkerhetskopia görs. hello tids- och datumstämpel avspeglar hello tiden för din senaste säkerhetskopia av systemtillståndet.

## <a name="bmr-backup"></a>BMR säkerhetskopiering

För BMR (inklusive en säkerhetskopia av systemtillståndet), sparas hello säkerhetskopieringsjobbet direkt tooa resursen på hello Backup Server-datorn. Det sparas inte tooa mapp på hello skyddad server.

Backup-servern anropar Windows Server Backup och delar ut hello replikvolymen för BMR säkerhetskopian. I så fall tala den inte Windows Server Backup toouse hello enheten med mest ledigt utrymme för hello här. Den används i stället hello-resurs som har skapats för hello jobbet.

När hello säkerhetskopieringen är klar är hello filen överförda toohello Backup Server-datorn. Loggfilerna lagras i C:\Windows\Logs\WindowsServerBackup.

## <a name="prerequisites-and-limitations"></a>Krav och begränsningar

-   BMR stöds inte för datorer som kör Windows Server 2003 eller för datorer som kör ett klientoperativsystem.

-   Du kan inte skydda BMR och systemet tillstånd för hello samma dator i olika skyddsgrupper.

-   En Backup Server-dator kan inte skydda sig själv för BMR.

-   Kortsiktigt skydd tootape (disk till band eller D2T) stöds inte för BMR. Långsiktig lagring tootape (disk-till-disk till band eller D2D2T) stöds.

-   Windows Server Backup måste installeras på hello skyddade datorn för BMR-skydd.

-   För BMR-skydd, till skillnad från för skydd av systemtillstånd Backup Server har inte några krav på diskutrymme på hello skyddad dator. Windows Server Backup Överför direkt säkerhetskopieringar toohello Backup Server-datorn. hello säkerhetskopiering överföringsjobb visas inte i hello reservserver **jobb** vyn.

-   Backup-Server reserverar 30 GB utrymme på hello replikvolymen för BMR. Du kan ändra detta på hello **diskallokering** sidan i guiden Ändra Skyddsgrupp för hello eller med hjälp av hello Get-DatasourceDiskAllocation och Set-DatasourceDiskAllocation PowerShell-cmdlets. På hello återställningspunktvolymen kräver BMR skydd om 6 GB för ett Kvarhållningsintervall på fem dagar.
    * Observera att du kan minska hello replik volym storlek tooless än 15 GB.
    * Backup-servern beräkna inte hello storleken på hello BMR-datakällan. Den förutsätter 30 GB för alla servrar. Ändra hello värde baserat på hello storleken på BMR säkerhetskopieringar som du förväntar dig i din miljö. hello storleken på en BMR säkerhetskopiering kan beräknas ungefär som hello summan av använt utrymme på alla kritiska volymer. Kritiska volymer = Start volymen + volym där systemtillstånd, till exempel Active Directory.

-   Om du ändrar från systemet tillstånd skydd tooBMR skydd kräver BMR skydd mindre utrymme på hello *återställningspunktvolymen*. Dock är hello extra utrymme på volymen hello inte frigöras. Du kan manuellt minska hello volymens storlek på hello **ändra diskallokering** sidan av guiden Ändra Skyddsgrupp för hello eller genom att använda hello Get-DatasourceDiskAllocation och Set-DatasourceDiskAllocation PowerShell-cmdlets.

    Om du ändrar från skydd tooBMR skydd av systemtillstånd BMR-skydd kräver mer utrymme på hello *replikvolymen*. hello volymen utökas automatiskt. Om du vill toochange hello standardutrymmesallokeringarna kan använda hello Modify-DiskAllocation PowerShell-cmdlet.

-   Om du ändrar från skydd av BMR skydd toosystem systemtillstånd behöver mer utrymme på återställningspunktvolymen för hello. Backup-Server kan försöka tooautomatically öka hello volym. Om det finns inte tillräckligt med utrymme i lagringspoolen hello, uppstår ett fel.

    Om du ändrar från skydd av BMR skydd toosystem systemtillstånd behöver du utrymme på hello skyddad dator. Detta beror på att skydd av systemtillstånd först skriver hello replik toohello lokala datorn och sedan överför toohello Backup Server-datorn.

## <a name="before-you-begin"></a>Innan du börjar

1.  **Distribuera Azure Backup-Server**. Kontrollera att Backup Server distribueras på rätt sätt. Mer information finns i:
    * [Systemkrav för Azure Backup Server](http://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites)
    * [Säkerhetskopiera servern skydd matris](backup-mabs-protection-matrix.md)

2.  **Konfigurera lagring**. Du kan lagra säkerhetskopierade data på disk, band och hello moln med Azure. Mer information finns i [förbereda datalagring](https://docs.microsoft.com/system-center/dpm/plan-long-and-short-term-data-storage).

3.  **Konfigurera hello skyddsagenten**. Installera hello skyddsagenten på hello-dator som du vill att tooback. Mer information finns i [distribuera hello DPM-skyddsagenten](http://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent).

## <a name="back-up-system-state-and-bare-metal"></a>Säkerhetskopiering av systemtillstånd och bare metal
Konfigurera en skyddsgrupp enligt beskrivningen i [distribuera skyddsgrupper](http://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups). Observera att du kan inte skydda BMR och systemet tillstånd för hello samma dator i olika grupper. När du väljer BMR aktiveras också automatiskt systemtillstånd.


1.  tooopen hello Skapa ny Skyddsgrupp guiden i hello Backup Server-administratörskonsolen väljer **skydd** > **åtgärder** > **skapa skydd Gruppen**.

2.  På hello **Välj typ av Skyddsgrupp** väljer **servrar**, och välj sedan **nästa**.

3.  På hello **Välj gruppmedlemmar** sidan, expandera hello datorn och välj sedan antingen **BMR** eller **systemtillstånd**.

    Kom ihåg att du inte kan skydda både BMR och systemtillstånd för hello samma dator i olika grupper. När du väljer BMR aktiveras också automatiskt systemtillstånd. Mer information finns i [distribuera skyddsgrupper](http://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups).

4.  På hello **Välj Dataskyddsmetod** väljer du hur toohandle kortsiktig och långsiktig säkerhetskopiering. Kortsiktig säkerhetskopiering alltid toodisk först, med hello alternativet att säkerhetskopiera från hello disk toohello Azure cloud med hjälp av Azure Backup (kortsiktiga eller långsiktiga). Ett alternativt toolong sikt säkerhetskopiering toohello moln är tooset upp långsiktig säkerhetskopiering tooa fristående enheten eller bandet bandbibliotek som har anslutit tooBackup Server.

5.  På hello **Välj kortvariga mål** väljer du hur tooback in tooshort-lagring på disk:
    1. För **Kvarhållningsintervall**, kan du välja hur länge tookeep hello data på disken. 
    2. För **Synkroniseringsfrekvens**, väljer du hur ofta du vill att toorun en inkrementell säkerhetskopiering toodisk. Om du inte vill tooset ett intervall för säkerhetskopiering, kan du kontrollera hello **precis innan en återställningspunkt** alternativet. Backup-servern körs en snabb fullständig säkerhetskopiering precis innan varje återställningspunkt schemaläggs.

6.  Om du vill toostore data på band för långsiktig lagring på hello **ange långsiktiga mål** väljer du hur länge du vill att tookeep Banddata (1 – 99 år). 
    1. För **säkerhetskopieringsfrekvens**, Välj hur ofta säkerhetskopiering tootape ska köras. hello frekvens baseras på hello kvarhållningsintervallet som du har valt:
        * När hello kvarhållningsintervallet är 1 – 99 år kan välja du säkerhetskopieringar toooccur dag, vecka, varannan vecka, månad, kvartalsvis, Halvårsvis, eller år.
        * När hello kvarhållningsintervallet är 1 – 11 månader kan välja du säkerhetskopieringar toooccur dag, varje vecka, varannan vecka eller månad.
        * När hello kvarhållningsintervallet är 1 – 4 veckor kan välja du säkerhetskopieringar toooccur dagliga och veckovisa.

    2. På hello **Välj band och biblioteksinformation** sidan, Välj hello band och bibliotek toouse, och om data ska komprimeras och krypteras.

7.  På hello **granska diskallokering** granskar hello lagringspoolens diskutrymme som allokerats för hello skyddsgrupp.

    1. **Total storlek på Data** är hello storleken på hello data som du vill att tooback.
    2. **Disk space toobe har etablerats på Azure Backup Server** hello utrymme som reservserver rekommenderar för hello skyddsgruppen. Backup-servern väljer hello perfekt säkerhetskopieringsvolymen baserat på hello inställningar. Men du kan redigera hello säkerhetskopieringsvolymen valen i **Disk detaljer om fördelningen**. 
    3. Välj hello önskad lagring för arbetsbelastningar i hello nedrullningsbara menyn. Redigeringarna ändra hello värden för **totala lagringsutrymmet** och **ledigt utrymme** i hello **tillgängliga disklagring** fönstret. Underprovisioned utrymme är hello mängden lagringsutrymme som Backup Server föreslår vi att du lägger till toohello volym, tooensure smooth säkerhetskopieringar.

8.  På hello **Välj Replikskapandemetod** väljer du hur toohandle hello inledande fullständig datareplikering. Om du väljer tooreplicate hello nätverket, rekommenderar vi att du väljer en tid. Överväg att replikera hello data offline med hjälp av flyttbart medium för stora mängder data eller för nätverksförhållanden som är mindre än optimala.

9. På hello **välja alternativ för konsekvenskontroll** väljer du hur tooautomate konsekvenskontroller. Du kan välja toorun en kontroll endast när replikdata blir inkonsekvent, eller enligt ett schema. Om du inte vill tooconfigure automatisk konsekvenskontroll kan köra du en manuell kontroll när som helst. toorun en manuell kontroll i hello **skydd** område i hello Backup Server administratörskonsolen högerklickar du på hello skydd gruppen och välj sedan **utför konsekvenskontroll**.

10. Om du har markerat tooback in toohello moln med Azure Backup på hello **ange Onlineskyddsdata** kontrollerar du att du väljer hello arbetsbelastningar som du vill tooback in tooAzure.

11. På hello **Ange schema för Online säkerhetskopiering** väljer du hur ofta inkrementella säkerhetskopieringar tooAzure sker. Du kan schemalägga säkerhetskopieringar toorun varje dag, vecka, månad och år och välja hello tid och datum då de ska köras. Säkerhetskopieringar kan inträffa in tootwice per dag. Varje gång en säkerhetskopiering körs en data-återställningspunkt skapas i Azure från hello kopia av hello säkerhetskopierade data lagras på hello reservserver disk.

12. På hello **ange bevarandeprincip** väljer du hur hello Återställningspunkter som skapas från hello dagliga, veckovisa, månatliga och årliga säkerhetskopior bevaras i Azure.

13. På hello **Välj Onlinereplikeringsmetoden** väljer du hur hello inledande fullständig replikering av data sker. Du kan replikera via nätverket hello eller göra ett offline säkerhetskopiering (offline seeding). Offlinesäkerhetskopiering använder hello Azure Import-funktionen. Mer information finns i [Offline säkerhetskopiering arbetsflödet i Azure Backup](backup-azure-backup-import-export.md).

14. På hello **sammanfattning** granskar du inställningarna. När du har valt **Skapa grupp**, inledande replikering av hello data. När replikering har slutförts på hello **Status** , hello skyddsgruppens status är **OK**. Säkerhetskopieringen sker sedan per hello skydd gruppinställningar.

## <a name="recover-system-state-or-bmr"></a>Återställa systemtillstånd eller BMR
Du kan återställa BMR eller tillstånd tooa plats i nätverket. Om du säkerhetskopierade BMR och använda Windows Recovery Environment (WinRE) toostart datorn och ansluta den toohello nätverk. Använd sedan Windows Server Backup toorecover från hello nätverksplats. Om du har säkerhetskopierat systemtillståndet kan du bara använda Windows Server Backup toorecover från hello nätverksplats.

### <a name="restore-bmr"></a>Återställa BMR
Köra återställning på hello Backup Server-datorn:

1.  I hello **Recovery** rutan, hitta hello dator du vill toorecover och välj sedan **Bare Metal Recovery**.

2.  Tillgängliga återställningspunkter visas i fetstil i kalendern hello. Välj hello datum och tid för hello återställningspunkt som du vill toouse.

3.  På hello **Välj typ av återställning** väljer **kopiera tooa nätverksmapp.**

4.  På hello **ange mål** anger där du vill att toocopy hello data. Kom ihåg att det valda målet hello måste toohave tillräckligt med utrymme. Vi rekommenderar att du skapar en ny mapp.

5.  På hello **Ange återställningsalternativ** sidan, Välj hello säkerhet inställningar tooapply. Markera om du vill toouse lagringsnätverk (SAN)-baserade ögonblicksbilder av maskinvara, för snabbare återställning. (Detta är ett alternativ om du har ett SAN-nätverk med den här funktionen är tillgänglig, och hello möjlighet toocreate och dela en klon toomake den skrivbar. In addition, hello skyddad dator och servern för säkerhetskopiering måste vara anslutna toohello samma nätverk.)

6.  Konfigurera alternativ för aviseringar. På hello **bekräftelse** väljer **återställa**.

Ställ in hello plats:

1.  Gå toohello mapp som hello säkerhetskopior i hello återställningsplatsen.

2.  Dela hello-mapp som är en nivå ovanför WindowsImageBackup så att hello roten för hello delad mapp hello WindowsImageBackup mapp. Om du inte gör det återställningen inte att hitta hello säkerhetskopiering. tooconnect genom att använda Windows Recovery Environment (WinRE) behöver du en resurs som du har åtkomst till i WinRE med hello rätt IP-adress och autentiseringsuppgifter.

Återställ hello system:

1.  Starta hello-dator som du vill använda toorestore hello avbildningen med hjälp av hello Windows-DVD för hello system du återställer.

2.  På första sidan i hello kontrollerar du språk och nationella inställningar. På hello **installera** väljer **reparera datorn**.

3.  På hello **Systemåterställningsalternativ** väljer **återställa datorn med en systemavbildning som du skapade tidigare**.

4.  På hello **Välj en säkerhetskopierad systemavbildning** väljer **Välj en systemavbildning** > **Avancerat** > **Sök efter ett system Bild på hello nätverket**. Om en varning visas väljer du **Ja**. Gå toohello resurssökväg, ange hello autentiseringsuppgifter och sedan väljer hello återställningspunkt. Detta söker efter specifika säkerhetskopior som är tillgängliga i denna återställningspunkt. Välj hello återställningspunkt som du vill toouse.

5.  På hello **väljer hur toorestore hello säkerhetskopiering** väljer **formatera och partitionera om diskar**. Kontrollera inställningarna på hello nästa sida. 

6.  toobegin hello återställning, Välj **Slutför**. En omstart krävs.

### <a name="restore-system-state"></a>Återställa systemtillståndet

Köra återställning Backup Server:

1.  I hello **Recovery** rutan Sök hello dator du vill toorecover och välj sedan **Bare Metal Recovery**.

2.  Tillgängliga återställningspunkter visas i fetstil i kalendern hello. Välj hello datum och tid för hello återställningspunkt som du vill toouse.

3.  På hello **Välj typ av återställning** väljer **kopiera tooa nätverksmapp**.

4.  På hello **ange mål** anger där du vill toocopy hello data. Kom ihåg att det valda målet hello behöver tillräckligt med utrymme. Vi rekommenderar att du skapar en ny mapp.

5.  På hello **Ange återställningsalternativ** sidan, Välj hello säkerhet inställningar tooapply. Markera om du vill toouse SAN-baserade ögonblicksbilder av maskinvara för snabbare återställning. (Detta är ett alternativ om du har ett SAN-nätverk med den här funktionen och hello möjlighet toocreate och dela en klon toomake den skrivbar. In addition, hello skyddad dator och Backup Server-servern måste vara anslutna toohello samma nätverk.)

6.  Konfigurera alternativ för aviseringar. På hello **bekräftelse** väljer **återställa**.

Kör Windows Serverbackup:

1.  Välj **åtgärder** > **återställa** > **servern** > **nästa**.

2.  Välj **en annan Server**väljer hello **Ange platstyp** och väljer sedan **Remote delade mappen**. Ange hello sökvägen toohello mapp som innehåller hello återställningspunkt.

3.  På hello **Välj typ av återställning** väljer **systemtillstånd**. 

4. På hello **Välj plats för återställning av systemtillstånd** väljer **ursprungsplatsen**.

5.  På hello **bekräftelse** väljer **återställa**. Starta om hello server efter hello återställning.

6.  Du kan också köra hello återställning av systemtillstånd i en kommandotolk. toodo detta, starta Windows Server Backup på hello dator du vill toorecover. Ange tooget hello version Identiferare, vid en kommandotolk:```wbadmin get versions -backuptarget \<servername\sharename\>```

    Använd hello version identifierare toostart hello återställning av systemtillstånd. Ange Kommandotolken hello:```wbadmin start systemstaterecovery -version:<versionidentified> -backuptarget:<servername\sharename>```

    Bekräfta att du vill toostart hello återställning. Du kan se hello processen i hello Kommandotolkens fönster. En återställningslogg skapas. Starta om hello server efter hello återställning.

