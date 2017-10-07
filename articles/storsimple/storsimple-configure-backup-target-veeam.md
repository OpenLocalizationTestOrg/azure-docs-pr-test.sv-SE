---
title: "aaaStorSimple 8000-serien som mål för säkerhetskopian med Veeam | Microsoft Docs"
description: "Beskriver hello StorSimple mål för säkerhetskopian konfiguration med Veeam."
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
ms.date: 12/06/2016
ms.author: hkanna
ms.openlocfilehash: 74a4af307fab430942f94b3e28f514a9abce227b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-as-a-backup-target-with-veeam"></a>StorSimple som ett mål med Veeam

## <a name="overview"></a>Översikt

Azure StorSimple är en cloud hybridlagringslösning från Microsoft. StorSimple adresser hello svårigheter av exponentiell datatillväxt med hjälp av Azure Storage-konto som ett tillägg för hello lokal lösning och automatiskt lagringsnivåer data över lokala lagring och lagringsutrymmet i molnet.

I den här artikeln tar vi upp StorSimple integrering med Veeam och bästa praxis för integrering av båda lösningarna. Vi kan också göra rekommendationer om hur tooset in Veeam toobest integrera med StorSimple. Vi skjuta upp tooVeeam bästa praxis, säkerhetskopiering arkitekter och administratörer för hello tooset på bästa sätt in Veeam toomeet enskilda säkerhetskopieringskrav och servicenivåavtal (SLA).

Även om vi illustrerar konfigurationssteg och viktiga begrepp är i den här artikeln inte menat att en stegvis guide för konfiguration och installation. Vi förutsätter att hello grundläggande komponenter och infrastruktur i fungerande skick och redo toosupport hello begrepp som beskrivs.

### <a name="who-should-read-this"></a>Vem ska läsa det här?

hello information i den här artikeln kommer att mest användbara toobackup administratörer och lagringsadministratörer lagring arkitekter med kunskaper om lagring, Windows Server 2012 R2, Ethernet, molntjänster och Veeam.

### <a name="supported-versions"></a>Versioner som stöds

-   Veeam 9 och senare versioner
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
![StorSimple lagringsnivåer diagram](./media/storsimple-configure-backup-target-using-veeam/image1.jpg)

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

| Lagringskapacitet | 8100 | 8600 |
|---|---|---|
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

![StorSimple som ett logiskt diagram för primära mål för säkerhetskopian](./media/storsimple-configure-backup-target-using-veeam/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Primära mål säkerhetskopiering logiska steg

1.  hello reservserver kontakter hello mål backup-agenten och hello backup-agenten skickar data toohello backup-servern.
2.  hello reservserver skriver data toohello StorSimple nivåindelade volymer.
3.  hello reservserver uppdaterar hello katalogdatabasen och är klar hello säkerhetskopieringsjobb.
4.  Ett skript för ögonblicksbild utlöser hello StorSimple moln snapshot manager (start eller ta bort).
5.  hello reservserver tar bort utgångna säkerhetskopieringar utifrån en bevarandeprincip.

### <a name="primary-target-restore-logical-steps"></a>Primära mål återställning logiska steg

1.  hello reservserver startar återställer hello lämplig data från hello lagringsplats.
2.  hello säkerhetskopieringsagent tar emot hello data från hello reservserver.
3.  hello-servern för säkerhetskopiering slutförs hello återställningsjobbet.

## <a name="storsimple-as-a-secondary-backup-target"></a>StorSimple som en sekundär mål för säkerhetskopian

I det här scenariot används främst StorSimple-volymer för långsiktig kvarhållning eller arkivering.

hello följande bild visar en arkitektur som inledande säkerhetskopior och återställer målvolymen hög prestanda. Dessa säkerhetskopior kopieras och arkiverade tooa StorSimple nivåer volym på ett schema.

Det är viktigt toosize högpresterande volymen så att den kan hantera din princip Kapacitets- och krav på datalagring.

![StorSimple som ett logiskt diagram sekundära mål för säkerhetskopian](./media/storsimple-configure-backup-target-using-veeam/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>Sekundär mål säkerhetskopiering logiska steg

1.  hello reservserver kontakter hello mål backup-agenten och hello backup-agenten skickar data toohello backup-servern.
2.  hello reservserver skriver toohigh prestanda för datalagring.
3.  hello reservserver uppdaterar hello katalogdatabasen och är klar hello säkerhetskopieringsjobb.
4.  hello reservserver kopior säkerhetskopieringar tooStorSimple baserat på en bevarandeprincip.
5.  Ett skript för ögonblicksbild utlöser hello StorSimple moln snapshot manager (start eller ta bort).
6.  hello reservserver tar bort utgångna säkerhetskopieringar utifrån en bevarandeprincip.

### <a name="secondary-target-restore-logical-steps"></a>Sekundär mål återställning logiska steg

1.  hello reservserver startar återställer hello lämplig data från hello lagringsplats.
2.  hello säkerhetskopieringsagent tar emot hello data från hello reservserver.
3.  hello-servern för säkerhetskopiering slutförs hello återställningsjobbet.

## <a name="deploy-hello-solution"></a>Distribuera hello lösning

Distribuera hello lösning kräver tre steg:

1. Förbered hello nätverksinfrastruktur.
2. Distribuera din StorSimple-enhet som mål för säkerhetskopian.
3. Distribuera Veeam.

Varje steg beskrivs i detalj i följande avsnitt hello.

### <a name="set-up-hello-network"></a>Konfigurera hello nätverk

Eftersom StorSimple är en lösning som är integrerad med hello Azure-molnet, kräver StorSimple en aktiv och fungerar anslutningen toohello Azure-molnet. Den här anslutningen används för åtgärder som molntjänster ögonblicksbilder, datahantering, och överföra metadata och tootier äldre, mindre används tooAzure moln datalagring.

För hello lösning tooperform optimalt, rekommenderar vi att du följer dessa nätverk metodtips:

-   hello-länk som ansluter din StorSimple lagringsnivåer tooAzure måste uppfylla krav på bandbredd. Göra detta genom att använda hello nödvändiga tjänstkvalitet (QoS) nivå tooyour infrastruktur växlar toomatch din Återställningspunktmål och återställning tid mål för Återställningstid serviceavtal.
-   Maximal Azure Blob storage-åtkomstfördröjning ska vara runt 80 ms.

### <a name="deploy-storsimple"></a>Distribuera StorSimple

Stegvisa anvisningar för StorSimple-distribution, se [distribuera din lokala StorSimple-enhet](storsimple-deployment-walkthrough-u2.md).

### <a name="deploy-veeam"></a>Distribuera Veeam

Veeam Metodtips för installation, se [Veeam säkerhetskopiering och bästa praxis för replikering](https://bp.veeam.expert/), och Läs hello användarhandboken på [Veeam hjälp och support (teknisk dokumentation)](https://www.veeam.com/documentation-guides-datasheets.html).

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

- Använd inte volymer (skapas av Windows Diskhantering). Volymer stöds inte.
- Formatera volymerna med NTFS med 64 KB allokeringsenhetsstorlek.
- Mappa hello StorSimple-volymer direkt toohello Veeam server.
    - Använd iSCSI för fysiska servrar.


## <a name="best-practices-for-storsimple-and-veeam"></a>Metodtips för StorSimple och Veeam

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

### <a name="veeam-best-practices"></a>Metodtips för Veeam

-   Hej Veeam databasen bör vara lokala toohello server och inte finns på en StorSimple-volym.
-   För katastrofåterställning, säkerhetskopierar du hello Veeam databasen på en StorSimple-volym.
-   Vi stöder Veeam fullständiga och inkrementella säkerhetskopior för den här lösningen. Vi rekommenderar att du inte använder syntetiska och differentiell säkerhetskopiering.
-   Säkerhetskopierade datafiler bör innehålla endast hello data för ett specifikt jobb. Till exempel lägger till något medium över olika jobb är tillåtna.
-   Inaktivera verifiering av jobbet. Om det behövs ska verifiering schemaläggas efter hello senaste säkerhetskopieringsjobb. Det är viktigt toounderstand att jobbet påverkar säkerhetskopieringsfönstret.
-   Aktivera före allokering av media.
-   Se till parallell bearbetning är aktiverat.
-   Stäng av komprimering.
-   Stäng av deduplicering på hello säkerhetskopieringsjobb.
-   Ange optimering för**LAN mål**.
-   Aktivera **skapa active fullständig säkerhetskopiering** (varannan vecka).
-   Ställ in på hello säkerhetskopiera databasen, **använder per VM säkerhetskopior**.
-   Ange **använder flera överför strömmar per jobb** för**8** (högst 16 tillåts). Justera det här antalet uppåt eller nedåt baserat på processoranvändningen på hello StorSimple-enhet.

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

## <a name="set-up-veeam-storage"></a>Konfigurera Veeam lagring

### <a name="tooset-up-veeam-storage"></a>tooset Veeam lagring av

1.  I konsolen för replikering, och hello Veeam säkerhetskopiering i **databasen verktyg**, gå för**säkerhetskopiering infrastruktur**. Högerklicka på **säkerhetskopiering databaser**, och välj sedan **Lägg till säkerhetskopiering databas**.

    ![Säkerhetskopiera databasen sida i hanteringskonsolen Veeam](./media/storsimple-configure-backup-target-using-veeam/veeamimage1.png)

2.  I hello **nya säkerhetskopiering databasen** dialogrutan Ange ett namn och beskrivning för hello lagringsplats. Välj **nästa**.

    ![Veeam konsolen, namn och beskrivning av sidan för hantering](./media/storsimple-configure-backup-target-using-veeam/veeamimage2.png)

3.  Hello typ, Välj **Microsoft Windows server**. Välj hello Veeam server. Välj **nästa**.

    ![Välj typ av säkerhetskopiering databasen i hanteringskonsolen Veeam](./media/storsimple-configure-backup-target-using-veeam/veeamimage3.png)

4.  toospecify **plats**, bläddra och välja hello volym. Välj hello **maximala samtidiga uppgifter för att begränsa:** kryssrutan och ange hello värdet för**4**. Detta säkerställer att endast fyra virtuella diskar som bearbetas samtidigt medan varje virtuell dator (VM) bearbetas. Välj hello **Avancerat** knappen.

    ![Välj volymen i hanteringskonsolen Veeam](./media/storsimple-configure-backup-target-using-veeam/veeamimage4.png)


5.  I hello **lagring kompatibilitetsinställningar** dialogrutan, Välj hello **använder per VM säkerhetskopior** kryssrutan.

    ![Veeam konsolen för hantering av kompatibilitetsinställningar för lagring](./media/storsimple-configure-backup-target-using-veeam/veeamimage5.png)

6.  I hello **nya säkerhetskopiering databasen** dialogrutan, Välj hello **aktivera vPower NFS-tjänsten på hello montera server (rekommenderas)** kryssrutan. Välj **nästa**.

    ![Säkerhetskopiera databasen sida i hanteringskonsolen Veeam](./media/storsimple-configure-backup-target-using-veeam/veeamimage6.png)

7.  Granska inställningarna för hello och välj sedan **nästa**.

    ![Säkerhetskopiera databasen sida i hanteringskonsolen Veeam](./media/storsimple-configure-backup-target-using-veeam/veeamimage7.png)

    En databas läggs toohello Veeam server.

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>Ställa in StorSimple som en primär mål för säkerhetskopian

> [!IMPORTANT]
> Återställning av data från en säkerhetskopia som har nivåindelade toohello moln inträffar hastigheter molnet.

hello visar följande bild hello mappning av en typisk volym tooa säkerhetskopiering. I så fall måste alla hello veckovisa säkerhetskopior mappa toohello lördag fullständig disk och hello inkrementella säkerhetskopieringar mappa tooMonday fredag inkrementell diskar. Alla hello säkerhetskopieringar och återställningar är från en StorSimple nivåer volym.

![Primära mål för säkerhetskopian configuration logiskt diagram](./media/storsimple-configure-backup-target-using-veeam/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>StorSimple som mål för primär säkerhetskopiering GFS schemalägga exempel

Här är ett exempel på ett GFS rotationsschema för fyra veckor, månatliga och årliga:

| Typ av frekvens/säkerhetskopiering | Fullständig | Stegvis (1-5 dagar)  |   
|---|---|---|
| Varje vecka (1 – 4 veckor) | Lördag | Måndag-fredag |
| Månadsvis  | Lördag  |   |
| Varje år | Lördag  |   |   |


### <a name="assign-storsimple-volumes-tooa-veeam-backup-job"></a>Tilldela StorSimple-volymer tooa Veeam säkerhetskopieringsjobb

Skapa dagliga jobb för primära mål för säkerhetskopian scenariot med din primära Veeam StorSimple-volym. Skapa ett dagliga jobb med hjälp av direkt ansluten lagring (DAS), nätverksansluten lagring (NAS) eller bara en grupp av diskar JBOD-lagring för ett scenario med en sekundär mål för säkerhetskopian.

#### <a name="tooassign-storsimple-volumes-tooa-veeam-backup-job"></a>tooassign StorSimple-volymer tooa Veeam säkerhetskopieringsjobb

1.  Välj hello Veeam säkerhetskopiering och replikering konsolen **för säkerhetskopiering och replikering**. Högerklicka på **säkerhetskopiering**, och välj sedan **VMware** eller **Hyper-V**, beroende på din miljö.

    ![Nytt säkerhetskopieringsjobb i hanteringskonsolen Veeam](./media/storsimple-configure-backup-target-using-veeam/veeamimage8.png)

2.  I hello **nytt säkerhetskopieringsjobb i gång** dialogrutan Ange ett namn och beskrivning för hello daglig säkerhetskopiering.

    ![Veeam konsolen för hantering av ny säkerhetskopieringsjobbet sida](./media/storsimple-configure-backup-target-using-veeam/veeamimage9.png)

3.  Välj en virtuell dator tooback upp till.

    ![Veeam konsolen för hantering av ny säkerhetskopieringsjobbet sida](./media/storsimple-configure-backup-target-using-veeam/veeamimage10.png)

4.  Välj hello värden för **säkerhetskopiera proxy** och **säkerhetskopiering databasen**. Välj ett värde för **återställning pekar tookeep på disk** enligt toohello Återställningspunktmål och Återställningstidsmål definitioner för din miljö lokalt ansluten lagring. Välj **avancerade**.

    ![Veeam konsolen för hantering av ny säkerhetskopieringsjobbet sida](./media/storsimple-configure-backup-target-using-veeam/veeamimage11.png)

5. I hello **avancerade inställningar** på hello dialogrutan **säkerhetskopiering** väljer **stegvis**. Måste du se till att hello **skapa syntetiska fullständiga säkerhetskopieringar regelbundet** kryssrutan är avmarkerad. Välj hello **skapa active fullständiga säkerhetskopieringar regelbundet** kryssrutan. Under **Active fullständig säkerhetskopiering**väljer hello **varje vecka på valda dagar** kryssrutan för lördag.

    ![Veeam hanteringskonsolen nytt säkerhetskopieringsjobb i gång avancerade inställningar](./media/storsimple-configure-backup-target-using-veeam/veeamimage12.png)

6. På hello **lagring** kontrollerar du att den hello **aktivera infogad datadeduplicering** kryssrutan är avmarkerad. Välj hello **utelämna växlingen filblocken** kryssrutan och välj hello **utelämna bort filblocken** kryssrutan. Ange **komprimeringsnivå** för**ingen**. Ange för belastningsutjämnade prestanda och deduplicering **lagringsoptimering** för**LAN mål**. Välj **OK**.

    ![Veeam hanteringskonsolen nytt säkerhetskopieringsjobb i gång avancerade inställningar](./media/storsimple-configure-backup-target-using-veeam/veeamimage13.png)

    Information om inställningar för komprimering och deduplicering av Veeam finns [datakomprimering och Deduplicering](https://helpcenter.veeam.com/backup/vsphere/compression_deduplication.html).

7.  I hello **redigera säkerhetskopieringsjobb** dialogrutan kan du välja hello **aktivera Programmedveten bearbetning** kryssrutan (valfritt).

    ![Veeam konsolen för hantering av ny säkerhetskopieringsjobbet gäst bearbetning sida](./media/storsimple-configure-backup-target-using-veeam/veeamimage14.png)

8.  Definiera hello schema toorun när varje dag, kan du ange samtidigt.

    ![Veeam konsolen för hantering av ny säkerhetskopieringsjobbet schema sida](./media/storsimple-configure-backup-target-using-veeam/veeamimage15.png)

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>Ställa in StorSimple som en sekundär mål för säkerhetskopian

> [!NOTE]
> Data återställs från en säkerhetskopia som har nivåindelade toohello moln uppstå hastigheter molnet.

I den här modellen måste du ha en lagring media (andra än StorSimple) tooserve som en tillfällig cache. Du kan till exempel använda ett redundant matris av oberoende diskar (RAID) volymen tooaccommodate diskutrymme, indata/utdata (I/O) och bandbredd. Vi rekommenderar att du använder RAID 5, 50 och 10.

hello följande bild visar vanliga kortsiktig kvarhållning lokala (toohello server) volymer och långsiktig kvarhållning Arkiv volymer. Det här scenariot köras alla säkerhetskopior i på hello lokala (toohello server) RAID-volym. Dessa säkerhetskopior dupliceras regelbundet och arkiveras tooan Arkiv volym. Det är viktigt toosize din lokala (toohello server) RAID-volym så att den kan hantera ditt kortsiktiga krav på datalagring kapacitet och prestanda.

![StorSimple som sekundär mål för säkerhetskopian logiskt diagram](./media/storsimple-configure-backup-target-using-veeam/secondarybackuptargetdiagram.png)

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>StorSimple som en sekundär mål för säkerhetskopian GFS-exempel

hello följande tabell visas hur tooset in säkerhetskopieringar toorun på hello lokala och StorSimple-diskar. Den innehåller enskilda och totala kapacitetsbehov.

| Typ av säkerhetskopiering och lagring | Konfigurerad lagring | Storlek (TiB) | Multiplikatorn GFS | Total kapacitet\* (TiB) |
|---|---|---|---|---|
| Vecka 1 (fullständiga och inkrementella) |Lokal disk (kortsiktig)| 1 | 1 | 1 |
| StorSimple veckor 2-4 |StorSimple disk (långsiktiga) | 1 | 4 | 4 |
| Månatliga fullständig |StorSimple disk (långsiktiga) | 1 | 12 | 12 |
| Årlig fullständig |StorSimple disk (långsiktiga) | 1 | 1 | 1 |
|Kravet GFS volymer |  |  |  | 18*|
\*Total kapacitet innehåller 17 TiB StorSimple-diskar och 1 TiB lokal RAID-volym.


### <a name="gfs-example-schedule"></a>GFS exempel schema

GFS rotation schema för varje vecka, månad och år

| Vecka | Fullständig | Inkrementell dag 1 | Inkrementell dag 2 | Inkrementell dag 3 | Inkrementell dag 4 | Inkrementell dag 5 |
|---|---|---|---|---|---|---|
| Vecka 1 | Lokal RAID-volym  | Lokal RAID-volym | Lokal RAID-volym | Lokal RAID-volym | Lokal RAID-volym | Lokal RAID-volym |
| Vecka 2 | StorSimple veckor 2-4 |   |   |   |   |   |
| Vecka 3 | StorSimple veckor 2-4 |   |   |   |   |   |
| Vecka 4 | StorSimple veckor 2-4 |   |   |   |   |   |
| Månadsvis | StorSimple varje månad |   |   |   |   |   |
| Varje år | StorSimple varje år  |   |   |   |   |   |   |

### <a name="assign-storsimple-volumes-tooa-veeam-copy-job"></a>Tilldela StorSimple-volymer tooa Veeam kopieringsjobbet

#### <a name="tooassign-storsimple-volumes-tooa-veeam-copy-job"></a>tooassign StorSimple-volymer tooa Veeam kopieringsjobbet

1.  Välj hello Veeam säkerhetskopiering och replikering konsolen **för säkerhetskopiering och replikering**. Högerklicka på **säkerhetskopiering**, och välj sedan **VMware** eller **Hyper-V**, beroende på din miljö.

    ![Veeam konsolen för hantering av ny säkerhetskopia jobbet sida](./media/storsimple-configure-backup-target-using-veeam/veeamimage16.png)

2.  I hello **nytt säkerhetskopieringsjobb kopiera** dialogrutan Ange ett namn och beskrivning för hello jobbet.

    ![Veeam konsolen för hantering av ny säkerhetskopia jobbet sida](./media/storsimple-configure-backup-target-using-veeam/veeamimage17.png)

3.  Välj hello virtuella datorer du vill tooprocess. Välj från säkerhetskopior och välj sedan hello daglig säkerhetskopiering som du skapade tidigare.

    ![Veeam konsolen för hantering av ny säkerhetskopia jobbet sida](./media/storsimple-configure-backup-target-using-veeam/veeamimage18.png)

4.  Uteslut objekt från hello säkerhetskopia jobb, om det behövs.

5.  Välj lagringsplatsen för säkerhetskopiering och ange ett värde för **återställning pekar tookeep**. Vara säker på att tooselect hello **Behåll hello följande återställningspunkter för arkiveringsändamål** kryssrutan. Definiera hello frekvens för säkerhetskopiering och välj sedan **Avancerat**.

    ![Veeam konsolen för hantering av ny säkerhetskopia jobbet sida](./media/storsimple-configure-backup-target-using-veeam/veeamimage19.png)

6.  Ange hello följande avancerade inställningar:

    * På hello **Underhåll** fliken, Stäng av lagring nivån skadade guard.

    ![Veeam hanteringskonsolen nytt säkerhetskopia jobb avancerade inställningar](./media/storsimple-configure-backup-target-using-veeam/veeamimage20.png)

    * På hello **lagring** fliken, se till att deduplicering och komprimering inaktiveras.

    ![Veeam hanteringskonsolen nytt säkerhetskopia jobb avancerade inställningar](./media/storsimple-configure-backup-target-using-veeam/veeamimage21.png)

7.  Ange att hello dataöverföring är direkt.

8.  Definiera hello säkerhetskopia schema enligt tooyour behov och slutför sedan guiden hello.

Mer information finns i [Skapa säkerhetskopia jobb](https://helpcenter.veeam.com/backup/hyperv/backup_copy_create.html).

## <a name="storsimple-cloud-snapshots"></a>StorSimple molnögonblicksbilder

StorSimple molnögonblicksbilder skydda hello data som finns i din StorSimple-enhet. Skapa en ögonblicksbild i molnet är likvärdiga tooshipping lokala band tooan externt anläggning. Om du använder Azure geo-redundant lagring, är att skapa en ögonblicksbild i molnet likvärdiga tooshipping band toomultiple platser. Om du behöver toorestore en enhet efter en katastrof, kan du använda en annan StorSimple enhet online och gör en redundansväxling. Du kommer att kunna tooaccess hello data (hastigheter moln) från hello senaste ögonblicksbild i molnet efter hello redundans.

hello följande avsnitt beskriver hur toocreate en kort skriptet toostart och ta bort StorSimple molnbaserade ögonblicksbilder under säkerhetskopiering efter bearbetning.

> [!NOTE]
> Ögonblicksbilder som manuellt eller programmässigt skapas följer inte hello StorSimple snapshot princip. Dessa ögonblicksbilder raderas manuellt eller programmässigt.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Starta och ta bort molnögonblicksbilder med hjälp av ett skript

> [!NOTE]
> Utvärdera noggrant hello efterlevnad och konsekvenser för kvarhållning av data innan du tar bort en StorSimple-ögonblicksbild. Mer information om hur toorun skript efter säkerhetskopieringen hello Veeam dokumentationen.


### <a name="backup-lifecycle"></a>Livscykeln för säkerhetskopiering

![Diagram över livscykeln för säkerhetskopiering](./media/storsimple-configure-backup-target-using-veeam/backuplifecycle.png)

### <a name="requirements"></a>Krav

-   hello-server som kör hello skriptet måste ha åtkomst tooAzure molnresurser.
-   hello-användarkontot måste ha behörighet för hello.
-   En princip för StorSimple-säkerhetskopiering med hello associerade StorSimple-volymer måste ställa in men inte aktiverad.
-   Du behöver hello StorSimple resursnamnet, registreringsnyckel, enhetsnamn och säkerhetskopieringsprincip-ID.

### <a name="toostart-or-delete-a-cloud-snapshot"></a>toostart eller ta bort en ögonblicksbild i molnet

1. [Installera Azure PowerShell](/powershell/azure/overview).
2. [Hämta och importera publicera inställningar och prenumerationsinformation om](https://msdn.microsoft.com/library/dn385850.aspx).
3. I hello klassiska Azure-portalen, hämta hello resursnamnet och [Registreringsnyckeln för din StorSimple Manager-tjänsten](storsimple-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).
4. Kör PowerShell som administratör på hello-server som kör hello skript. Ange följande kommando:

    `Get-AzureStorSimpleDeviceBackupPolicy –DeviceName <device name>`

    Obs hello säkerhetskopieringsprincip-ID.
5. Skapa ett nytt PowerShell.skript med hjälp av hello följande kod i anteckningar.

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
6. tooadd hello skriptet tooyour säkerhetskopiering jobbet, redigera Veeam jobbet avancerade alternativ.

    ![Veeam avancerade inställningar för säkerhetskopiering skript fliken](./media/storsimple-configure-backup-target-using-veeam/veeamimage22.png)

Vi rekommenderar att du kör din StorSimple moln ögonblicksbild säkerhetskopieringsprincip som ett efterbearbetning skript hello slutet av det dagliga säkerhetskopieringsjobbet. Mer information om hur tooback upp och återställning av ditt program för säkerhetskopiering miljö toohelp du uppfyller dina Återställningspunktmål och Återställningstidsmål, kontakta din backup systemarkitekter.

## <a name="storsimple-as-a-restore-source"></a>StorSimple som en källa för återställning

Återställer från en StorSimple-enhet fungerar som återställer från valfri enhet för lagring av block. Återställning av data som är nivåindelade toohello moln inträffar hastigheter molnet. Återställer finnas på hello lokala diskhastighet hello enhet för lokala data.

Med Veeam får du snabb, detaljerade och filnivå recovery via StorSimple via hello inbyggda explorer vyer i hello Veeam-konsolen. Använd de Veeam olika utforskarfönster toorecover enskilda objekt, t.ex. e-postmeddelanden, Active Directory-objekt och SharePoint-objekt från säkerhetskopior. hello återställning kan göras utan lokala VM avbrott. Du kan också göra point-in-time-återställning för Azure SQL Database och Oracle-databaser. Veeam och StorSimple gör hello processen för återställning på objektnivå från Azure snabbt och enkelt. Information om hur tooperform en återställning hello Veeam dokumentationen:

- För [Exchange Server](https://www.veeam.com/microsoft-exchange-recovery.html)
- För [Active Directory](https://www.veeam.com/microsoft-active-directory-explorer.html)
- För [SQLServer](https://www.veeam.com/microsoft-sql-server-explorer.html)
- För [SharePoint](https://www.veeam.com/microsoft-sharepoint-recovery-explorer.html)
- För [Oracle](https://www.veeam.com/oracle-backup-recovery-explorer.html)


## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple redundans och disaster recovery

> [!NOTE]
> Mål för säkerhetskopian scenarier stöds inte StorSimple moln installation som ett mål för återställning.

En katastrof kan orsakas av olika faktorer. hello i den följande tabellen listas vanliga disaster recovery-scenarier.

| Scenario | Påverkan | Hur toorecover | Anteckningar |
|---|---|---|---|
| Fel för StorSimple-enhet | Åtgärder för säkerhetskopiering och återställning avbryts. | Ersätt hello misslyckad enhet och utföra [StorSimple redundans och disaster recovery](storsimple-device-failover-disaster-recovery.md). | Om du behöver tooperform en återställning efter återställning av enheten hämtas arbetsminnet fullständiga data från hello moln toohello ny enhet. Alla åtgärder är hastigheter molnet. alla säkerhetskopior toobe skannade och hämtas från hello nivå toohello lokala enhet molnnivån, som kan vara en tidskrävande process kan leda till hello index och katalogen som processen körs. |
| Veeam-serverfel | Åtgärder för säkerhetskopiering och återställning avbryts. | Återskapa hello reservserver och utföra återställning av databasen enligt anvisningarna i [Veeam hjälp och support (teknisk dokumentation)](https://www.veeam.com/documentation-guides-datasheets.html).  | Du måste återskapa eller återställa hello Veeam server på hello disaster recovery plats. Återställningspunkt hello databasen toohello senaste. Om hello återställda Veeam databasen inte är synkroniserad med din senaste säkerhetskopieringsjobb, krävs indexering och katalogiserar. Den här index och katalogen som processen körs kan orsaka alla säkerhetskopior toobe skannade och hämtas från hello nivå toohello lokala enhet molnnivån. Detta gör det mer tidskrävande. |
| Platsfel som resulterar i hello förlust av både hello reservserver och StorSimple | Åtgärder för säkerhetskopiering och återställning avbryts. | Återställ StorSimple först och sedan återställa Veeam. | Återställ StorSimple först och sedan återställa Veeam. Om du behöver tooperform en återställning efter återställning av enheten hämtas arbetsminnet för hello fullständiga data från hello moln toohello ny enhet. Alla åtgärder är hastigheter molnet. |


## <a name="references"></a>Referenser

hello efter dokument har referenser till den här artikeln:

- [StorSimple multipath i/o-installationen](storsimple-configure-mpio-windows-server.md)
- [Scenarier för lagring: tunn allokering](http://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [Använda GPT-enheter](http://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Konfigurera skuggkopior för delade mappar](http://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Nästa steg

- Läs mer om hur för[återställning från en säkerhetskopia](storsimple-restore-from-backup-set-u2.md).
- Läs mer om hur tooperform [enheten redundans och disaster recovery](storsimple-device-failover-disaster-recovery.md).
