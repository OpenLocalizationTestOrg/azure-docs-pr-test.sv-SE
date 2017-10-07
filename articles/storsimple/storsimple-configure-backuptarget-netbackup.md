---
title: "aaaStorSimple 8000-serien som mål för säkerhetskopian med NetBackup | Microsoft Docs"
description: "Beskriver hello StorSimple mål för säkerhetskopian konfiguration med Veritas NetBackup."
services: storsimple
documentationcenter: 
author: harshakirank
manager: matd
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: hkanna
ms.openlocfilehash: 7d032bbcf6e40e7609e51437e290fc92b232a48f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-as-a-backup-target-with-netbackup"></a>StorSimple som ett mål med NetBackup

## <a name="overview"></a>Översikt

Azure StorSimple är en cloud hybridlagringslösning från Microsoft. StorSimple adresser hello svårigheter av exponentiell datatillväxt med hjälp av Azure storage-konto som ett tillägg för hello lokal lösning och automatiskt lagringsnivåer data över lokala lagring och lagringsutrymmet i molnet.

I den här artikeln tar vi upp StorSimple integrering med Veritas NetBackup och bästa praxis för integrering av båda lösningarna. Vi kan också göra rekommendationer om hur tooset in Veritas NetBackup toobest integrera med StorSimple. Vi skjuta upp tooVeritas bästa praxis, säkerhetskopiering arkitekter och administratörer för hello tooset på bästa sätt in Veritas NetBackup toomeet enskilda säkerhetskopieringskrav och servicenivåavtal (SLA).

Även om vi illustrerar konfigurationssteg och viktiga begrepp är i den här artikeln inte menat att en stegvis guide för konfiguration och installation. Vi förutsätter att hello grundläggande komponenter och infrastruktur i fungerande skick och redo toosupport hello begrepp som beskrivs.

### <a name="who-should-read-this"></a>Vem ska läsa det här?

hello information i den här artikeln kommer att mest användbara toobackup administratörer och lagringsadministratörer lagring arkitekter med kunskaper om lagring, Windows Server 2012 R2, Ethernet, molntjänster och Veritas NetBackup.

### <a name="supported-versions"></a>Versioner som stöds

-   NetBackup 7.7.x och senare versioner
-   [StorSimple uppdatering 3 och senare versioner](storsimple-overview.md#storsimple-workload-summary)


## <a name="why-storsimple-as-a-backup-target"></a>Varför StorSimple som mål för säkerhetskopian?

StorSimple är ett bra alternativ för ett mål för säkerhetskopian eftersom:

-   Det ger standard, lokal lagring för säkerhetskopieringsprogram toouse som ett mål för snabb säkerhetskopiering, utan några ändringar. Du kan också använda StorSimple för en snabb återställning av nya säkerhetskopior.
-   Dess molnet skiktning har integrerats med en Azure-molnet storage-konto toouse kostnadseffektiv lagring i Azure.
-   Tillhandahåller det automatiskt lagring på annan plats för katastrofåterställning.

## <a name="key-concepts"></a>Viktiga begrepp

Precis som med alla lagringslösning, en noggrann utvärdering av hello lösning lagringsprestanda, SLA: er, är ändra och tillväxt kapacitetsbehov kritiska toosuccess. hello huvudsakliga idé är att genom att introducera en moln-nivå, åtkomsttider och genomflöden toohello molnet ha en grundläggande roll i hello möjligheten för StorSimple toodo sitt jobb.

StorSimple är utformad tooprovide lagring tooapplications som fungerar på en väldefinierad arbetsminnet för data (varm data). I den här modellen hello arbetsminnet för data lagras på hello lokala nivåerna och hello återstående ledig/kall/arkiverade data är nivåindelade toohello moln. Den här modellen representeras i hello följande bild. hello nästan platt grön rad motsvarar hello data som lagras på hello lokala nivåerna av hello StorSimple-enhet. hello representerar röd linje hello totala mängden data som lagras på hello StorSimple-lösningen över alla nivåer. hello blanksteg mellan flat grön hello rad och hello exponentiell red kurva representerar hello totala mängden data som lagras i hello molnet.

**StorSimple skiktning**
![StorSimple lagringsnivåer diagram](./media/storsimple-configure-backup-target-using-netbackup/image1.jpg)

Du kommer märka att StorSimple är idealisk toooperate som ett mål med den här arkitekturen i åtanke. Du kan använda StorSimple till:
-   Utföra dina mest frekventa återställningar från hello lokala arbetsminnet för data.
-   Använd hello molnet för externt katastrofåterställning och äldre data där återställningar är mer sällan.

## <a name="storsimple-benefits"></a>StorSimple fördelar

StorSimple är en lokal lösning som är sömlöst integrerad med Microsoft Azure, genom att utnyttja sömlös åtkomst till lokala tooon och molnlagring.

StorSimple använder automatisk skiktning mellan hello lokala enheten, vilket har SSD-enhet (SSD) och serieansluten SCSI (SAS) lagring och Azure Storage. Automatisk lagringsnivåer behåller data som ofta används lokalt på hello SSD och SAS-nivåer. Data som sällan används tooAzure lagring flyttas.

StorSimple ger följande fördelar:

-   Unik deduplicering och komprimering algoritmer som använder hello molnet tooachieve oöverträffad deduplicering nivåer
-   Hög tillgänglighet
-   GEO-replikering med hjälp av Azure geo-replikering
-   Azure-integrering
-   Datakryptering i hello moln
-   Förbättrad återställning och efterlevnad

Även om StorSimple uppvisar grunden två huvudsakliga distributionsscenarier (mål för säkerhetskopian av primära och sekundära mål för säkerhetskopian), är en vanlig, blocklagringsserver. StorSimple alla hello komprimering och deduplicering. Sömlöst skickar och hämtar data mellan hello molnet och hello program och filsystemet.

Läs mer om StorSimple [StorSimple 8000-serien: Hybrid cloud lagringslösning](storsimple-overview.md). Du kan också granska hello [tekniska specifikationer för StorSimple 8000-serien](storsimple-technical-specifications-and-compliance.md).

> [!IMPORTANT]
> Med hjälp av en StorSimple-enheten som ett mål stöds bara för StorSimple 8000 uppdatering 3 och senare versioner.

## <a name="architecture-overview"></a>Översikt över arkitekturen

hello visar följande tabeller hello enheten modell-arkitektur inledande vägledning.

**StorSimple kapacitet för lokala och lagringsutrymmet i molnet**

| Lagringskapacitet       | 8100          | 8600            |
|------------------------|---------------|-----------------|
| Lokal lagringskapacitet | &lt;10 TiB\*  | &lt;20 TiB\*  |
| Kapacitet för molnlagring | &gt;200 TiB\* | &gt;500 TiB\* |
\*Lagringsstorleken förutsätter inga deduplicering eller komprimering.

**StorSimple kapacitet för primära och sekundära säkerhetskopieringar**

| Säkerhetskopiering scenario  | Lokal lagringskapacitet  | Kapacitet för molnlagring  |
|---|---|---|
| Primär säkerhetskopiering  | Nya säkerhetskopior lokalt lagrade för snabb återställning toomeet återställningspunktmål (RPO) | Historik för säkerhetskopiering (RPO) passar i kapacitet |
| Sekundär säkerhetskopiering | Sekundär kopia av säkerhetskopierade data kan lagras i kapacitet  | Saknas  |

## <a name="storsimple-as-a-primary-backup-target"></a>StorSimple som en primär mål för säkerhetskopian

I det här scenariot visas StorSimple-volymer toohello säkerhetskopieringsprogram som hello enda lagringsplats för säkerhetskopiering. hello följande bild visar en lösningsarkitektur där alla säkerhetskopieringar används StorSimple nivåindelade volymer för säkerhetskopiering och återställning.

![StorSimple som ett logiskt diagram för primära mål för säkerhetskopian](./media/storsimple-configure-backup-target-using-netbackup/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Primära mål säkerhetskopiering logiska steg

1.  hello reservserver kontakter hello mål backup-agenten och hello backup-agenten skickar data toohello backup-servern.
2.  hello reservserver skriver data toohello StorSimple nivåindelade volymer.
3.  hello reservserver uppdaterar hello katalogdatabasen och är klar hello säkerhetskopieringsjobb.
4.  Ett skript för ögonblicksbild utlöser hello StorSimple snapshot manager (start eller ta bort).
5.  hello reservserver tar bort utgångna säkerhetskopieringar utifrån en bevarandeprincip.

### <a name="primary-target-restore-logical-steps"></a>Primära mål återställning logiska steg

1.  hello reservserver startar återställer hello lämplig data från hello lagringsplats.
2.  hello säkerhetskopieringsagent tar emot hello data från hello reservserver.
3.  hello-servern för säkerhetskopiering slutförs hello återställningsjobbet.

## <a name="storsimple-as-a-secondary-backup-target"></a>StorSimple som en sekundär mål för säkerhetskopian

I det här scenariot används främst StorSimple-volymer för långsiktig kvarhållning eller arkivering.

hello följande bild visar en arkitektur som inledande säkerhetskopior och återställer målvolymen hög prestanda. Dessa säkerhetskopior kopieras och arkiverade tooa StorSimple nivåer volym på ett schema.

Det är viktigt toosize högpresterande volymen så att den kan hantera din princip Kapacitets- och krav på datalagring.

![StorSimple som ett logiskt diagram sekundära mål för säkerhetskopian](./media/storsimple-configure-backup-target-using-netbackup/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>Sekundär mål säkerhetskopiering logiska steg

1.  hello reservserver kontakter hello mål backup-agenten och hello backup-agenten skickar data toohello backup-servern.
2.  hello reservserver skriver toohigh prestanda för datalagring.
3.  hello reservserver uppdaterar hello katalogdatabasen och är klar hello säkerhetskopieringsjobb.
4.  hello reservserver kopior säkerhetskopieringar tooStorSimple baserat på en bevarandeprincip.
5.  Ett skript för ögonblicksbild utlöser hello StorSimple snapshot manager (start eller ta bort).
6.  hello reservserver tar bort hello upphört att gälla säkerhetskopieringar utifrån en bevarandeprincip.

### <a name="secondary-target-restore-logical-steps"></a>Sekundär mål återställning logiska steg

1.  hello reservserver startar återställer hello lämplig data från hello lagringsplats.
2.  hello säkerhetskopieringsagent tar emot hello data från hello reservserver.
3.  hello-servern för säkerhetskopiering slutförs hello återställningsjobbet.

## <a name="deploy-hello-solution"></a>Distribuera hello lösning

Distribuera den här lösningen kräver tre steg:
1. Förbered hello nätverksinfrastruktur.
2. Distribuera din StorSimple-enhet som mål för säkerhetskopian.
3. Distribuera Veritas NetBackup.

Varje steg beskrivs i detalj i följande avsnitt hello.

### <a name="set-up-hello-network"></a>Konfigurera hello nätverk

Eftersom StorSimple är en lösning som är integrerad med hello Azure-molnet, kräver StorSimple en aktiv och fungerar anslutningen toohello Azure-molnet. Den här anslutningen används för åtgärder som molntjänster ögonblicksbilder, datahantering, och överföra metadata och tootier äldre, mindre används tooAzure moln datalagring.

För hello lösning tooperform optimalt, rekommenderar vi att du följer dessa nätverk metodtips:

-   hello-länk som ansluter hello StorSimple lagringsnivåer tooAzure måste uppfylla krav på bandbredd. tooachieve, tillämpa hello rätt tjänstkvalitet (QoS) nivå tooyour infrastruktur växlar toomatch din Återställningspunktmål och återställning tid mål för Återställningstid serviceavtal.

-   Maximal Azure Blob storage-åtkomstfördröjning ska vara runt 80 ms.

### <a name="deploy-storsimple"></a>Distribuera StorSimple

Stegvisa anvisningar för StorSimple-distribution, se [distribuera din lokala StorSimple-enhet](storsimple-deployment-walkthrough-u2.md).

### <a name="deploy-netbackup"></a>Distribuera NetBackup

Stegvisa NetBackup 7.7.x distribution vägledning finns hello [NetBackup 7.7.x dokumentationen](http://www.veritas.com/docs/000094423).

## <a name="set-up-hello-solution"></a>Ställa in hello lösning

I det här avsnittet visar vi några Konfigurationsexempel. hello följande exempel och rekommendationer visar hello mest grundläggande och grundläggande implementering. Den här implementeringen kan inte direkt tooyour särskilda säkerhetskopieringskrav.

### <a name="set-up-storsimple"></a>Ställ in StorSimple

| Distributionsuppgifter för StorSimple  | Ytterligare kommentarer |
|---|---|
| Distribuera din lokala StorSimple-enhet. | Versioner som stöds: uppdatera 3 och senare versioner. |
| Aktivera hello mål för säkerhetskopian. | Använd dessa kommandon tooturn på eller stänga av mål för säkerhetskopian läge och tooget status. Mer information finns i [fjärransluta tooa StorSimple-enhet](storsimple-remote-connect.md).</br> tooturn på säkerhetskopieringsläge: `Set-HCSBackupApplianceMode -enable`. </br> tooturn av säkerhetskopiering läge: `Set-HCSBackupApplianceMode -disable`. </br> tooget hello aktuell status för inställningar för säkerhetskopiering: `Get-HCSBackupApplianceMode`. |
| Skapa en gemensam volymbehållare för volymen som lagrar hello säkerhetskopierade data. Alla data i en volymbehållare är deduplicerad. | Behållare för StorSimple-volym definiera deduplicering domäner.  |
| Skapa StorSimple-volymer. | Skapa volymer med storlekar som Stäng toohello förväntat förbrukning som möjligt, eftersom volymens storlek påverkar molnet ögonblicksbild varaktighet. Information om hur toosize en volym, Läs mer om [bevarandeprinciper](#retention-policies).</br> </br> Använd StorSimple nivåer volymer och välj hello **Använd volymen för mindre ofta använda arkiveringsdata** kryssrutan. </br> Endast lokalt fästa volymer stöds inte. |
| Skapa en unik StorSimple-säkerhetskopieringsprincip för alla hello mål för säkerhetskopian volymer. | En princip för säkerhetskopiering av StorSimple definierar hello volym konsekvenskontroll grupp. |
| Inaktivera hello schema när hello ögonblicksbilder upphör. | Ögonblicksbilder utlöses som en efterbearbetning åtgärd. |

### <a name="set-up-hello-host-backup-server-storage"></a>Konfigurera hello värden reservserver lagring

Ställ in hello värden reservserver lagring enligt toothese riktlinjer:  

- Använd inte volymer (skapas av Windows Diskhantering;) volymer stöds inte.
- Formatera volymerna med NTFS med 64 KB allokeringsstorlek.
- Mappa hello StorSimple-volymer direkt toohello NetBackup server.
    - Använd iSCSI för fysiska servrar.
    - Använd genomströmning av diskar för virtuella servrar.


## <a name="best-practices-for-storsimple-and-netbackup"></a>Metodtips för StorSimple och NetBackup

Ställ in din lösning enligt toohello riktlinjerna i hello följande avsnitten.

### <a name="operating-system-best-practices"></a>Metodtips för operativsystem

-   Inaktivera Windows Server-kryptering och deduplicering för hello NTFS-filsystemet.
-   Inaktivera Windows Server defragmentering på hello StorSimple-volymer.
-   Inaktivera Windows Server indexering på hello StorSimple-volymer.
-   Köra antivirusprogram på hello källvärden (inte mot hello StorSimple-volymer).
-   Inaktivera hello standard [Windows serverunderhåll](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) i Aktivitetshanteraren. Gör på något av följande sätt hello:
    - Inaktivera hello Underhåll configurator i Schemaläggaren i Windows.
    - Hämta [PsExec](https://technet.microsoft.com/sysinternals/bb897553.aspx) från Windows Sysinternals. När du har hämtat PsExec, kör du Windows PowerShell som administratör och skriv:
      ```powershell
      psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
      ```

### <a name="storsimple-best-practices"></a>Metodtips för StorSimple

-   Se till att hello StorSimple-enheten har uppdaterats för[Update 3 eller senare](storsimple-install-update-3.md).
-   Isolera iSCSI och molntrafik. Använd dedikerade iSCSI-anslutningar för trafik mellan StorSimple och hello reservserver.
-   Se till att din StorSimple-enhet är en dedikerad mål för säkerhetskopian. Blandade arbetsbelastningar stöds inte eftersom de påverkar din RTO och Återställningspunktmål.

### <a name="netbackup-best-practices"></a>Metodtips för NetBackup

-   Hej NetBackup databasen bör vara lokala toohello server och inte finns på en StorSimple-volym.
-   För katastrofåterställning, säkerhetskopierar du hello NetBackup databasen på en StorSimple-volym.
-   Vi stöder NetBackup fullständiga och inkrementella säkerhetskopior (även hänvisade tooas differentiella säkerhetskopior i NetBackup) för den här lösningen. Vi rekommenderar att du inte använder syntetiska och kumulativa inkrementella säkerhetskopieringar.
-   Säkerhetskopierade datafiler bör innehålla endast hello data för ett specifikt jobb. Till exempel lägger till något medium över olika jobb är tillåtna.

Hello senaste NetBackup inställningar och bästa praxis för att implementera dessa krav finns hello NetBackup dokumentationen på [www.veritas.com](https://www.veritas.com).


## <a name="retention-policies"></a>Principer för kvarhållning

En av hello vanligaste lagring av säkerhetskopior principtyper är en princip för farfar, pappa och Son offentlig sektor (GFS). I en GFS princip en inkrementell säkerhetskopiering utförs dagligen och fullständiga säkerhetskopieringar är klar vecka och månad. Den här principen resulterar i sex StorSimple nivåindelade volymer: en volym som innehåller hello varje vecka, månad och år fullständig säkerhetskopiering. hello lagrar andra fem volymer dagliga inkrementella säkerhetskopieringar.

I följande exempel hello, använder vi en GFS rotation. hello exemplet förutsätter hello följande:

-   Icke-deduplicerade eller komprimerade data används.
-   Fullständiga säkerhetskopieringar är 1 TiB.
-   Dagliga säkerhetskopior är 500 GiB.
-   Fyra veckovisa säkerhetskopior bevaras under en månad.
-   Tolv månatliga säkerhetskopior hålls för ett år.
-   En årlig säkerhetskopiering sparas för 10 år.

Baserat på hello föregående antaganden kan du skapa en 26 TiB StorSimple nivåer volymen för hello månatliga och årliga fullständiga säkerhetskopieringar. Skapa en 5 TiB StorSimple nivåer volymen för varje hello dagliga säkerhetskopior.

| Typ av säkerhetskopiering kvarhållning | Storlek (TiB) | Multiplikatorn GFS\* | Total kapacitet (TiB)  |
|---|---|---|---|
| Varje vecka fullständig | 1 | 4  | 4 |
| Dagliga inkrementella | 0.5 | 20 (cykler lika antal veckor per månad) | 12 (2 för ytterligare kvoten) |
| Månatliga fullständig | 1 | 12 | 12 |
| Årlig fullständig | 1  | 10 | 10 |
| GFS krav |   | 38 |   |
| Ytterligare kvot  | 4  |   | 42 totala GFS krav  |
\*hello GFS multiplikator är hello antalet kopior som du behöver tooprotect och behålla toomeet säkerhetskopieringsprincip-krav.

## <a name="set-up-netbackup-storage"></a>Konfigurera NetBackup lagring

### <a name="tooset-up-netbackup-storage"></a>tooset NetBackup lagring av

1.  Markera i hello NetBackup-Administrationskonsolen, **Media och enhetshantering** > **enheter** > **Diskpooler**. Välj hello lagringstyp server i hello konfigurationsguiden för Disk-poolen, **AdvancedDisk**, och välj sedan **nästa**.

    ![Konfigurationsguiden för Disk-Pool i administrationskonsolen NetBackup](./media/storsimple-configure-backup-target-using-netbackup/nbimage1.png)

2.  Välj servern och välj sedan **nästa**.

    ![NetBackup-administrationskonsolen väljer hello-server](./media/storsimple-configure-backup-target-using-netbackup/nbimage2.png)

3.  Välj din StorSimple-volym.

    ![NetBackup-administrationskonsolen väljer hello StorSimple-volym disk](./media/storsimple-configure-backup-target-using-netbackup/nbimage3.png)

4.  Ange ett namn för hello mål för säkerhetskopian och välj sedan **nästa** > **nästa** toofinish hello guiden.

5.  Granska inställningarna för hello och välj sedan **Slutför**.

6.  Ändra hello lagring enhet inställningar toomatch de som rekommenderas i hello slutet av varje volym tilldelning, [bästa praxis för StorSimple och NetBackup](#best-practices-for-storsimple-and-netbackup).

7. Upprepa steg 1 – 6 tills du är klar med att tilldela din StorSimple-volymer.

    ![Diskkonfigurationen i administrationskonsolen NetBackup](./media/storsimple-configure-backup-target-using-netbackup/nbimage5.png)

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>Ställa in StorSimple som en primär mål för säkerhetskopian

> [!NOTE]
> Data återställs från en säkerhetskopia som har nivåindelade toohello moln uppstå hastigheter molnet.

hello visar följande bild hello mappning av en typisk volym tooa säkerhetskopiering. I så fall måste alla hello veckovisa säkerhetskopior mappa toohello lördag fullständig disk och hello inkrementella säkerhetskopieringar mappa tooMonday fredag inkrementell diskar. Alla hello säkerhetskopieringar och återställningar är från en StorSimple nivåer volym.

![Primära mål för säkerhetskopian configuration logiskt diagram ](./media/storsimple-configure-backup-target-using-netbackup/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>StorSimple som mål för primär säkerhetskopiering GFS schemalägga exempel

Här är ett exempel på ett GFS rotationsschema för fyra veckor, månatliga och årliga:

| Typ av frekvens/säkerhetskopiering | Fullständig | Stegvis (1-5 dagar)  |   
|---|---|---|
| Varje vecka (1 – 4 veckor) | Lördag | Måndag-fredag |
| Månadsvis  | Lördag  |   |
| Varje år | Lördag  |   |   |

## <a name="assigning-storsimple-volumes-tooa-netbackup-backup-job"></a>Tilldela StorSimple-volymer tooa NetBackup säkerhetskopieringsjobb

följande sekvens hello förutsätter att NetBackup och hello målvärden konfigureras i enlighet med hello NetBackup agent riktlinjer.

### <a name="tooassign-storsimple-volumes-tooa-netbackup-backup-job"></a>tooassign StorSimple-volymer tooa NetBackup säkerhetskopieringsjobb

1.  I hello NetBackup-administrationskonsolen väljer **NetBackup Management**, högerklicka på **principer**, och välj sedan **ny princip**.

    ![NetBackup-Administrationskonsolen, skapa en ny princip](./media/storsimple-configure-backup-target-using-netbackup/nbimage6.png)

2.  I hello **lägga till en ny princip** dialogrutan, ange ett namn för principen hello och välj sedan hello **Använd principen konfigurationsguiden** kryssrutan. Välj **OK**.

    ![NetBackup-Administrationskonsolen, lägga till en ny princip dialogruta](./media/storsimple-configure-backup-target-using-netbackup/nbimage7.png)

3.  I hello guiden Konfigurera säkerhetskopiering princip älj hello typ av säkerhetskopiering och väljer sedan **nästa**.

    ![Välj typ av säkerhetskopiering NetBackup-administrationskonsolen](./media/storsimple-configure-backup-target-using-netbackup/nbimage8.png)

4.  tooset hello principtypen, Välj **Standard**, och välj sedan **nästa**.

    ![Välj Principtyp NetBackup-administrationskonsolen](./media/storsimple-configure-backup-target-using-netbackup/nbimage9.png)

5.  Välj din värd väljer hello **identifierar klientens operativsystem** kryssrutan och välj sedan **Lägg till**. Välj **nästa**.

    ![Lista över klienter i en ny princip i administrationskonsolen NetBackup](./media/storsimple-configure-backup-target-using-netbackup/nbimage10.png)

6.  Välj hello-enheter som du vill att tooback.

    ![NetBackup-administrationskonsolen säkerhetskopiering val för en ny princip](./media/storsimple-configure-backup-target-using-netbackup/nbimage11.png)

7.  Välj hello frekvens och kvarhållning värdena som uppfyller dina krav för säkerhetskopiering rotation.

    ![NetBackup-Administrationskonsolen, säkerhetskopieringsfrekvens och rotation för en ny princip](./media/storsimple-configure-backup-target-using-netbackup/nbimage12.png)

8.  Välj **nästa** > **nästa** > **Slutför**.  Du kan ändra schemat för hello efter hello principen skapas.

9.  Välj tooexpand hello princip som du precis skapade och välj sedan **scheman**.

    ![Scheman för en ny princip i administrationskonsolen NetBackup](./media/storsimple-configure-backup-target-using-netbackup/nbimage13.png)

10.  Högerklicka på **differentiella Inc**väljer **kopiera toonew**, och välj sedan **OK**.

    ![Kopiera schema tooa ny princip i administrationskonsolen NetBackup](./media/storsimple-configure-backup-target-using-netbackup/nbimage14.png)

11.  Högerklicka på hello nyskapad schema och välj sedan **ändra**.

12.  På hello **attribut** fliken, väljer hello **Åsidosätt principen val av lagringsutrymme** kryssrutan och välj sedan hello volym måndag inkrementella säkerhetskopieringar var.

    ![Ändra schema-administrationskonsolen NetBackup](./media/storsimple-configure-backup-target-using-netbackup/nbimage15.png)

13.  På hello **starta fönstret** fliken, väljer hello tidsfönstret för dina säkerhetskopieringar.

    ![Ändra start fönster NetBackup-administrationskonsolen](./media/storsimple-configure-backup-target-using-netbackup/nbimage16.png)

14.  Välj **OK**.

15.  Upprepa steg 10-14 för varje inkrementell säkerhetskopiering. Välj lämplig hello-volym och schema för varje säkerhetskopiering som du skapar.

16.  Högerklicka på hello **differentiella Inc** schemalägga och tar bort den.

17.  Ändra din schemat toomeet säkerhetskopieringen måste.

    ![Ändra schema för fullständig NetBackup-administrationskonsolen](./media/storsimple-configure-backup-target-using-netbackup/nbimage17.png)

18.  Ändra hello start-fönstret.

    ![Ändra hello start fönster NetBackup-administrationskonsolen](./media/storsimple-configure-backup-target-using-netbackup/nbimage18.png)

19.  hello slutliga schema ser ut så här:

    ![NetBackup-administrationskonsolen slutliga schema](./media/storsimple-configure-backup-target-using-netbackup/nbimage19.png)

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>Ställa in StorSimple som en sekundär mål för säkerhetskopian

> [!NOTE]
>Data återställs från en säkerhetskopia som har nivåindelade toohello moln uppstå hastigheter molnet.

I den här modellen måste du ha en lagring media (andra än StorSimple) tooserve som en tillfällig cache. Du kan till exempel använda ett redundant matris av oberoende diskar (RAID) volymen tooaccommodate diskutrymme, indata/utdata (I/O) och bandbredd. Vi rekommenderar att du använder RAID 5, 50 och 10.

hello visas bilden nedan vanliga kortsiktig kvarhållning lokala (toohello server) volymer och långsiktig kvarhållning arkiveras volymer. Det här scenariot köras alla säkerhetskopior i på hello lokala (toohello server) RAID-volym. Dessa säkerhetskopior dupliceras regelbundet och arkiverade tooan arkiveras volym. Det är viktigt toosize din lokala (toohello server) RAID-volym så att den kan hantera ditt kortsiktiga krav på datalagring kapacitet och prestanda.

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>StorSimple som en sekundär mål för säkerhetskopian GFS-exempel

![StorSimple som ett logiskt diagram sekundära mål för säkerhetskopian](./media/storsimple-configure-backup-target-using-netbackup/secondarybackuptargetdiagram.png)

hello följande tabell visas hur tooset in säkerhetskopieringar toorun på hello lokala och StorSimple-diskar. Den innehåller enskilda och totala kapacitetsbehov.

### <a name="backup-configuration-and-capacity-requirements"></a>Konfigurationen för säkerhetskopiering och kapacitetskrav

| Typ av säkerhetskopiering och lagring | Konfigurerad lagring | Storlek (TiB) | Multiplikatorn GFS | Total kapacitet\* (TiB) |
|---|---|---|---|---|
| Vecka 1 (fullständiga och inkrementella) |Lokal disk (kortsiktig)| 1 | 1 | 1 |
| StorSimple veckor 2-4 |StorSimple disk (långsiktiga) | 1 | 4 | 4 |
| Månatliga fullständig |StorSimple disk (långsiktiga) | 1 | 12 | 12 |
| Årlig fullständig |StorSimple disk (långsiktiga) | 1 | 1 | 1 |
|Kravet GFS volymer |  |  |  | 18*|
\*Total kapacitet innehåller 17 TiB StorSimple-diskar och 1 TiB lokal RAID-volym.


### <a name="gfs-example-schedule-gfs-rotation-weekly-monthly-and-yearly-schedule"></a>GFS exempel schema: GFS rotation schema för varje vecka, månad och år

| Vecka | Fullständig | Inkrementell dag 1 | Inkrementell dag 2 | Inkrementell dag 3 | Inkrementell dag 4 | Inkrementell dag 5 |
|---|---|---|---|---|---|---|
| Vecka 1 | Lokal RAID-volym  | Lokal RAID-volym | Lokal RAID-volym | Lokal RAID-volym | Lokal RAID-volym | Lokal RAID-volym |
| Vecka 2 | StorSimple veckor 2-4 |   |   |   |   |   |
| Vecka 3 | StorSimple veckor 2-4 |   |   |   |   |   |
| Vecka 4 | StorSimple veckor 2-4 |   |   |   |   |   |
| Månadsvis | StorSimple varje månad |   |   |   |   |   |
| Varje år | StorSimple varje år  |   |   |   |   |   |   |


## <a name="assign-storsimple-volumes-tooa-netbackup-archive-and-duplication-job"></a>Tilldela StorSimple-volymer tooa NetBackup arkivet och duplicering jobb

Eftersom NetBackup erbjuder en mängd olika alternativ för hantering av lagring och media, rekommenderar vi att du kontaktar Veritas eller din NetBackup systemarkitekt tooproperly utvärdera dina lagringsbehov livscykel princip (SLP).

När du har definierat hello inledande diskpooler, behöver du toodefine tre ytterligare lagringsutrymme livscykel principer, för totalt fyra principer:
* LocalRAIDVolume
* StorSimpleWeek2 4
* StorSimpleMonthlyFulls
* StorSimpleYearlyFulls

### <a name="tooassign-storsimple-volumes-tooa-netbackup-archive-and-duplication-job"></a>tooassign StorSimple-volymer tooa NetBackup arkivet och duplicering jobb

1.  I hello NetBackup-administrationskonsolen väljer **lagring** > **lagring livscykel principer** > **ny Storage livscykel princip**.

    ![Ny princip för lagring livscykel i administrationskonsolen NetBackup](./media/storsimple-configure-backup-target-using-netbackup/nbimage20.png)

2.  Ange ett namn för hello ögonblicksbild och välj sedan **Lägg till**.

3.  I hello **nya åtgärden** på hello dialogrutan **egenskaper** fliken för **åtgärden**väljer **säkerhetskopiering**. Välj hello värden för **mållagringskontot**, **kvarhållning typen**, och **kvarhållningsperioden**. Välj **OK**.

    ![Dialogrutan ny funktion i administrationskonsolen NetBackup](./media/storsimple-configure-backup-target-using-netbackup/nbimage22.png)

    Detta definierar hello första säkerhetskopieringen och databasen.

4.  Välj toohighlight hello tidigare igen och välj sedan **Lägg till**. I hello **ändra Lagringsåtgärden** dialogrutan, Välj hello värden för **mållagringskontot**, **kvarhållning typen**, och **kvarhållningsperioden** .

    ![Åtgärd för lösenordsbyte lagring dialogruta NetBackup-administrationskonsolen](./media/storsimple-configure-backup-target-using-netbackup/nbimage23.png)

5.  Välj toohighlight hello tidigare igen och välj sedan **Lägg till**. I hello **ny Storage livscykel princip** dialogrutan Lägg till månatliga säkerhetskopior för ett år.

    ![Dialogrutan Ny lagring Lifecycle Policy-administrationskonsolen NetBackup](./media/storsimple-configure-backup-target-using-netbackup/nbimage24.png)

6.  Upprepa steg 4 och 5 tills du har skapat hello omfattande SLP bevarandeprincip som du behöver.

    ![Lägg till principer hello ny Storage livscykel princip i dialogrutan NetBackup-administrationskonsolen](./media/storsimple-configure-backup-target-using-netbackup/nbimage25.png)

7.  När du är klar med att definiera dina SLP bevarandeprincip under **princip**, definiera en princip för säkerhetskopiering genom att följa hello stegen som beskrivs i [tilldela StorSimple-volymer tooa NetBackup säkerhetskopieringsjobbet](#assigning-storsimple-volumes-to-a-netbackup-backup-job).

8.  Under **scheman**, i hello **Ändra schema** dialogrutan, högerklicka på **fullständig**, och välj sedan **ändra**.

    ![Ändra schema i dialogrutan NetBackup-administrationskonsolen](./media/storsimple-configure-backup-target-using-netbackup/nbimage26.png)

9.  Välj hello **Åsidosätt principen val av lagringsutrymme** kryssrutan och välj sedan hello SLP bevarandeprincip som du skapade i steg 1 – 6.

    ![Val av åsidosättning princip lagringsutrymme i administrationskonsolen NetBackup](./media/storsimple-configure-backup-target-using-netbackup/nbimage27.png)

10.  Välj **OK**, och upprepa sedan för hello-schemat för inkrementell säkerhetskopiering.

    ![Dialogrutan Ändra schema för inkrementell säkerhetskopiering i administrationskonsolen NetBackup](./media/storsimple-configure-backup-target-using-netbackup/nbimage28.png)


| Typ av säkerhetskopiering kvarhållning | Storlek (TiB) | Multiplikatorn GFS\* | Total kapacitet (TiB)  |
|---|---|---|---|
| Varje vecka fullständig |  1  |  4 | 4  |
| Dagliga inkrementella  | 0.5  | 20 (cykler är lika toohello antal veckor per månad) | 12 (2 för ytterligare kvoten) |
| Månatliga fullständig  | 1 | 12 | 12 |
| Årlig fullständig | 1  | 10 | 10 |
| GFS krav  |     |     | 38 |
| Ytterligare kvot  | 4  |    | 42 totala GFS krav |
\*hello GFS multiplikator är hello antalet kopior som du behöver tooprotect och behålla toomeet säkerhetskopieringsprincip-krav.

## <a name="storsimple-cloud-snapshots"></a>StorSimple molnögonblicksbilder

StorSimple molnögonblicksbilder skydda hello data som finns i din StorSimple-enhet. Skapa en ögonblicksbild i molnet är likvärdiga tooshipping lokala band tooan externt anläggning. Om du använder Azure geo-redundant lagring, är att skapa en ögonblicksbild i molnet likvärdiga tooshipping band toomultiple platser. Om du behöver toorestore en enhet efter en katastrof, kan du använda en annan StorSimple enhet online och gör en redundansväxling. Du kommer att kunna tooaccess hello data (hastigheter moln) från hello senaste ögonblicksbild i molnet efter hello redundans.

hello följande avsnitt beskriver hur toocreate en kort skriptet toostart och ta bort StorSimple molnbaserade ögonblicksbilder under säkerhetskopiering efter bearbetning.

> [!NOTE]
> Ögonblicksbilder som manuellt eller programmässigt skapas följer inte hello StorSimple snapshot princip. Dessa ögonblicksbilder raderas manuellt eller programmässigt.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Starta och ta bort molnögonblicksbilder med hjälp av ett skript

> [!NOTE]
> Utvärdera noggrant hello efterlevnad och konsekvenser för kvarhållning av data innan du tar bort en StorSimple-ögonblicksbild. Mer information om hur toorun efter säkerhetskopieringen skript finns hello [NetBackup dokumentationen](http://www.veritas.com/docs/000094423).

### <a name="backup-lifecycle"></a>Livscykeln för säkerhetskopiering

![Säkerhetskopiera livscykel diagram](./media/storsimple-configure-backup-target-using-netbackup/backuplifecycle.png)

### <a name="requirements"></a>Krav

-   hello-server som kör hello skriptet måste ha åtkomst tooAzure molnresurser.
-   hello-användarkontot måste ha behörighet för hello.
-   En princip för StorSimple-säkerhetskopiering med hello associerade StorSimple-volymer måste ställa in men inte aktiverad.
-   Du behöver hello StorSimple resursnamnet, registreringsnyckel, enhetsnamn och säkerhetskopieringsprincip-ID.

### <a name="toostart-or-delete-a-cloud-snapshot"></a>toostart eller ta bort en ögonblicksbild i molnet

1.  [Installera Azure PowerShell](/powershell/azure/overview).
2.  [Hämta och importera publicera inställningar och prenumerationsinformation om](https://msdn.microsoft.com/library/dn385850.aspx).
3.  I hello klassiska Azure-portalen, hämta hello resursnamnet och [Registreringsnyckeln för din StorSimple Manager-tjänsten](storsimple-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).
4.  Kör PowerShell som administratör på hello-server som kör hello skript. Ange följande kommando:

    `Get-AzureStorSimpleDeviceBackupPolicy –DeviceName <device name>`

    Obs hello säkerhetskopieringsprincip-ID.
5.  Skapa ett nytt PowerShell.skript med hjälp av hello följande kod i anteckningar.

    Kopiera och klistra in det här kodstycket:
    ```powershell
    Import-AzurePublishSettingsFile "c:\\CloudSnapshot Snapshot\\myAzureSettings.publishsettings"
    Disable-AzureDataCollection
    $ApplianceName = <myStorSimpleApplianceName>
    $RetentionInDays = 20
    $RetentionInDays = -$RetentionInDays
    $Today = Get-Date
    $ExpirationDate = $Today.AddDays($RetentionInDays)
    Select-AzureStorSimpleResource -ResourceName "myResource" –RegistrationKey
    Start-AzureStorSimpleDeviceBackupJob –DeviceName $ApplianceName -BackupType CloudSnapshot -BackupPolicyId <BackupId> -Verbose
    $CompletedSnapshots =@()
    $CompletedSnapshots = Get-AzureStorSimpleDeviceBackup -DeviceName $ApplianceName
    Write-Host "hello Expiration date is " $ExpirationDate
    Write-Host

    ForEach ($SnapShot in $CompletedSnapshots)
    {
        $SnapshotStartTimeStamp = $Snapshot.CreatedOn
        if ($SnapshotStartTimeStamp -lt $ExpirationDate)

        {
            $SnapShotInstanceID = $SnapShot.InstanceId
            Write-Host "This snpashotdate was created on " $SnapshotStartTimeStamp.Date.ToShortDateString()
            Write-Host "Instance ID " $SnapShotInstanceID
            Write-Host "This snpashotdate is older and needs toobe deleted"
            Write-host "\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#"
            Remove-AzureStorSimpleDeviceBackup -DeviceName $ApplianceName -BackupId $SnapShotInstanceID -Force -Verbose
        }
    }
    ```
      Spara hello PowerShell-skriptet toohello samma plats där du sparade din Azure Publiceringsinställningar. Exempelvis spara som C:\CloudSnapshot\StorSimpleCloudSnapshot.ps1.
6.  Lägg till skript hello tooyour säkerhetskopieringsjobbet i NetBackup. toodo denna, redigera din NetBackup jobbet alternativ förbearbetning och kommandon för efterbearbetning.

> [!NOTE]
> Vi rekommenderar att du kör din StorSimple moln ögonblicksbild säkerhetskopieringsprincip som ett efterbearbetning skript hello slutet av det dagliga säkerhetskopieringsjobbet. Mer information om hur tooback upp och återställning av ditt program för säkerhetskopiering miljö toohelp du uppfyller dina Återställningspunktmål och Återställningstidsmål, kontakta din backup systemarkitekter.

## <a name="storsimple-as-a-restore-source"></a>StorSimple som en källa för återställning

Återställer från en StorSimple-enhet fungerar som återställer från valfri enhet för lagring av block. Återställning av data som är nivåindelade toohello moln inträffar hastigheter molnet. Återställer finnas på hello lokala diskhastighet hello enhet för lokala data. Information om hur tooperform en återställning finns hello [NetBackup dokumentationen](http://www.veritas.com/docs/000094423). Vi rekommenderar att du uppfyller tooNetBackup återställning bästa praxis.

## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple redundans och disaster recovery

> [!NOTE]
> Mål för säkerhetskopian scenarier stöds inte StorSimple moln installation som ett mål för återställning.

En katastrof kan orsakas av olika faktorer. hello i den följande tabellen listas vanliga disaster recovery-scenarier.

| Scenario | Påverkan | Hur toorecover | Anteckningar |
|---|---|---|---|
| Fel för StorSimple-enhet | Åtgärder för säkerhetskopiering och återställning avbryts. | Ersätt hello misslyckad enhet och utföra [StorSimple redundans och disaster recovery](storsimple-device-failover-disaster-recovery.md). | Om du behöver tooperform en återställning efter återställning av enheten hämtas arbetsminnet fullständiga data från hello moln toohello ny enhet. Alla åtgärder är hastigheter molnet. alla säkerhetskopior toobe skannade och hämtas från hello nivå toohello lokala enhet molnnivån, som kan vara en tidskrävande process kan leda till hello index och katalogen som processen körs. |
| NetBackup-serverfel | Åtgärder för säkerhetskopiering och återställning avbryts. | Återskapa hello reservserver och utföra återställning av databasen. | Du måste återskapa eller återställa hello NetBackup server på hello disaster recovery plats. Återställningspunkt hello databasen toohello senaste. Om hello återställda NetBackup databasen inte är synkroniserad med din senaste säkerhetskopieringsjobb, krävs indexering och katalogiserar. Den här index och katalogen som processen körs kan orsaka alla säkerhetskopior toobe skannade och hämtas från hello nivå toohello lokala enhet molnnivån. Detta gör det mer tidskrävande. |
| Platsfel som resulterar i hello förlust av både hello reservserver och StorSimple | Åtgärder för säkerhetskopiering och återställning avbryts. | Återställ StorSimple först och sedan återställa NetBackup. | Återställ StorSimple först och sedan återställa NetBackup. Om du behöver tooperform en återställning efter återställning av enheten hämtas arbetsminnet för hello fullständiga data från hello moln toohello ny enhet. Alla åtgärder är hastigheter molnet. |

## <a name="references"></a>Referenser

hello efter dokument har referenser till den här artikeln:

- [StorSimple multipath i/o-installationen](storsimple-configure-mpio-windows-server.md)
- [Scenarier för lagring: tunn allokering](http://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [Använda GPT-enheter](http://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Konfigurera skuggkopior för delade mappar](http://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Nästa steg

- Läs mer om hur för[återställning från en säkerhetskopia](storsimple-restore-from-backup-set-u2.md).
- Läs mer om hur tooperform [enheten redundans och disaster recovery](storsimple-device-failover-disaster-recovery.md).
