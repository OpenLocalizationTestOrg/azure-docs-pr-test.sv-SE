---
title: "aaaInstall StorSimple-kortet för SharePoint | Microsoft Docs"
description: "Beskriver hur tooinstall och konfigurera eller ta bort hello StorSimple nätverkskort för SharePoint i en SharePoint-servergrupp."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 36c20b75-f2e5-4184-a6b5-9c5e618f79b2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/06/2017
ms.author: v-sharos
ms.openlocfilehash: 9a7347232fb80156d93212e6382cdd4fab98a2d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-hello-storsimple-adapter-for-sharepoint"></a>Installera och konfigurera hello StorSimple nätverkskort för SharePoint
## <a name="overview"></a>Översikt
Hej StorSimple-kortet för SharePoint är en komponent som låter dig ange Microsoft Azure StorSimple flexibel lagring och data protection tooSharePoint servergrupper. Du kan använda hello kortet toomove binärt stort objekt (BLOB) innehåll från hello SQL Server innehållsdatabaser toohello Microsoft Azure StorSimple hybrid cloud lagringsenhet.

Hej StorSimple-kortet för SharePoint fungerar som en Remote BLOB Storage (RBS)-provider och använder hello SQL Server Remote BLOB Storage-funktionen toostore Ostrukturerade SharePoint-innehåll (hello form av Blobbar) på en filserver som backas upp av en StorSimple-enhet.

> [!NOTE]
> Hej StorSimple-kortet för SharePoint har stöd för SharePoint Server 2010 Remote BLOB Storage (RBS). Det har inte stöd för SharePoint Server 2010 externa BLOB Storage (EBS).


* toodownload hello StorSimple-kortet för SharePoint, gå för[StorSimple-kortet för SharePoint] [ 1] i hello Microsoft Download Center.
* Information om planering för RBS och RBS begränsningar finns för[bestämmer toouse RBS i SharePoint 2013] [ 2] eller [planera för RBS (SharePoint Server 2010)] [3].

hello beskrivs resten av den här översikten kortfattat hello roll hello StorSimple-kortet för SharePoint och hello SharePoint-kapacitet och prestanda gränser som du bör känna till innan du installerar och konfigurerar hello nätverkskort. När du har läst informationen gå för[StorSimple-kortet för SharePoint-installationen](#storsimple-adapter-for-sharepoint-installation) toobegin konfigurera hello nätverkskort.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>StorSimple-kortet för SharePoint-förmåner
Innehåll lagras i en SharePoint-webbplats som Ostrukturerade BLOB-data i en eller flera innehållsdatabaser. Databaserna finns som standard på datorer som kör SQL Server och finns i hello SharePoint-servergrupp. Blobbar kan snabbt ökar i storlek, förbrukar stora mängder lokal lagring. Därför kanske du vill toofind en annan, mindre kostsam lagringslösning. SQL Server innehåller en teknik som kallas Remote Blob Storage (RBS) där du kan lagra BLOB-innehåll i hello filsystem, utanför hello SQL Server-databas. Blobbar kan finnas i hello filsystemet på hello-dator som kör SQL Server med RBS, eller de kan lagras i hello filsystemet på en annan server-datorn.

RBS kräver att du använder en RBS provider, till exempel hello StorSimple-kortet för SharePoint, tooenable RBS i SharePoint. Hej StorSimple-kortet för SharePoint fungerar med RBS, så att du kan flytta Blobbar tooa server säkerhetskopieras av hello Microsoft Azure StorSimple-system. Microsoft Azure StorSimple sedan lagrar hello BLOB-data lokalt eller i hello molnet baserat på användning. Blobbar som är mycket aktiv (vanligtvis enligt tooas nivå 1 eller varm data) finnas lokalt. Mindre aktiva data och arkiveringsdata finns i hello molnet. När du aktiverar RBS på en innehållsdatabas lagras alla nya BLOB-innehåll som skapats i SharePoint på hello StorSimple-enhet och inte i hello innehållsdatabas.

hello Microsoft Azure StorSimple-implementering av RBS innehåller hello följande fördelar:

* Du kan minska hello frågebelastningen på SQL Server, vilket kan förbättra svarstiden för SQL Server av glidande BLOB innehåll tooa separat server. 
* Azure StorSimple använder deduplicering och komprimering tooreduce datastorleken.
* Azure StorSimple ger dataskydd i hello form av lokala och molnögonblicksbilder. Även om du placerar själva hello-databasen på hello StorSimple-enhet, kan du säkerhetskopiera hello innehållsdatabasen och Blobbar tillsammans i en krasch konsekvent sätt. (Flytta hello innehållsdatabasen toohello enheten stöds endast för hello StorSimple 8000-serieenhet. Den här funktionen stöds inte för hello 5000 och 7000-serien.)
* Azure StorSimple innehåller disaster recovery funktioner inklusive redundans, fil- och återställning (inklusive test recovery) och snabb återställning av data.
* Du kan använda data recovery programvara, till exempel Kroll Ontrack PowerControls med StorSimple ögonblicksbilder av BLOB-data tooperform objektnivååterställning för SharePoint-innehåll. (Den här programvaran för återställning av data är en separat köp.)
* Hej StorSimple-kortet för SharePoint som ansluts till hello Central Administration av SharePoint-portalen så att du toomanage SharePoint hela lösningen från en central plats.

Flytta BLOB innehåll toohello filsystem kan ge andra kostnadsbesparingar och fördelar. Till exempel med RBS minska hello behovet av dyr nivå 1-lagring och eftersom den krymper hello innehållsdatabasen RBS kan minska hello antalet databaser som krävs i hello SharePoint-servergrupp. Andra faktorer, till exempel storleksbegränsningar för databasen och hello mängden ej RBS innehåll, kan också påverka lagringsbehov. Läs mer om hello kostnader och fördelarna med att använda RBS [planera för RBS (SharePoint Foundation 2010)] [ 4] och [bestämmer toouse RBS i SharePoint 2013] [ 5].

### <a name="capacity-and-performance-limits"></a>Kapacitets- och gränser
Innan du överväga att använda RBS i SharePoint-lösningen, bör du vara medveten om hello testas prestanda och kapacitetsgränserna för SharePoint Server 2010 och SharePoint Server 2013 och hur dessa begränsningar relaterade tooacceptable prestanda. Mer information finns i [programvara gränser och begränsningar för SharePoint 2013](https://technet.microsoft.com/library/cc262787.aspx).

Läs igenom hello följande innan du konfigurerar RBS:

* Kontrollera att den totala storleken hello på hello innehåll (en innehållsdatabas hello storlek) plus hello storleken på alla associerade externalized Blobbar inte överstiga hello RBS storleksgränsen som stöds av SharePoint. Den här gränsen är 200 GB. 
  
    **toomeasure innehållsdatabasen och BLOB storlek**
  
  1. Kör frågan på hello WFE för Central Administration. Starta hello hanteringsgränssnittet för SharePoint och ange sedan följande Windows PowerShell-kommandot tooget hello storleken på hello innehållsdatabaser hello:
     
     `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`
     
      Det här steget hämtar hello storleken på hello innehållsdatabas på hello disk.
  2. Kör något av följande SQL-frågor i SQL Management Studio på rutan hello SQL server på varje innehållsdatabas hello och Lägg till hello resultatet toohello nummer som erhölls i steg 1.
     
     Innehållsdatabaser för SharePoint 2013, ange:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`
     
     SharePoint 2010 innehållsdatabaser, ange:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`
     
     Det här steget hämtar hello storleken på hello Blobbar som har varit externalized.
* Vi rekommenderar att du lagrar alla BLOB och databasen innehållet lokalt på hello StorSimple-enhet. Hej StorSimple-enhet är ett kluster med två noder för hög tillgänglighet. Placera hello innehållsdatabaser och Blobbar på hello StorSimple-enhet ger hög tillgänglighet.
  
    Använd traditionella SQL Server-migrering bästa praxis toomove hello innehållsdatabasen toohello StorSimple-enhet. Flytta hello databasen först när alla BLOB-innehåll från hello-databasen har flyttats toohello filresursen via RBS. Om du väljer toomove hello innehållsdatabasen toohello StorSimple-enhet, rekommenderar vi att du konfigurerar hello innehållsdatabasen lagring på hello enhet som en primär volym.
* I Microsoft Azure StorSimple, om du använder nivåindelade volymer, går det inte tooguarantee innehåll lagras lokalt på hello StorSimple-enheten inte kommer att nivåindelade tooMicrosoft Azure molnlagring. Vi rekommenderar därför använda tillsammans med SharePoint RBS lokalt Fäst StorSimple-volymer. Detta säkerställer att alla BLOB-innehåll förblir lokalt på hello StorSimple-enhet, inte och har flyttats tooMicrosoft Azure.
* Om du inte sparar hello innehållsdatabaser på hello StorSimple-enhet, Använd traditionella SQL Server med hög tillgänglighet säkerhetsmetoder som har stöd för RBS. SQL Server-kluster stöder RBS, när SQL Server spegling inte. 

> [!WARNING]
> Om du inte har aktiverat RBS rekommenderar vi inte flytta hello innehållsdatabasen toohello StorSimple-enhet. Detta är en otestade konfiguration.

## <a name="storsimple-adapter-for-sharepoint-installation"></a>StorSimple-kortet för SharePoint-installationen
Innan du kan installera hello StorSimple nätverkskort för SharePoint, måste du konfigurera hello StorSimple-enheten och kontrollera att hello SharePoint-servergrupp och instansiering av SQL Server uppfyller alla krav. Den här självstudiekursen beskriver konfigurationskrav, samt procedurer för installation och uppgradering hello StorSimple nätverkskort för SharePoint.

## <a name="configure-prerequisites"></a>Konfigurera krav
Innan du kan installera hello StorSimple nätverkskort för SharePoint, kontrollerar du att hello StorSimple-enhet, SharePoint-servergrupp och instansiering av SQL Server uppfyller hello följande krav.

### <a name="system-requirements"></a>Systemkrav
Hej StorSimple-kortet för SharePoint fungerar med hello följande maskinvara och programvara:

* Operativsystem som stöds – Windows Server 2008 R2 SP1, Windows Server 2012 eller Windows Server 2012 R2
* SharePoint versioner som stöds – SharePoint Server 2010 eller SharePoint Server 2013
* SQL Server versioner som stöds – SQL Server 2008 Enterprise Edition, SQL Server 2008 R2 Enterprise Edition eller SQL Server 2012 Enterprise Edition
* Stöd för StorSimple-enheter – StorSimple 8000-serien, StorSimple 7000-serien eller StorSimple 5000-serien.

### <a name="storsimple-device-configuration-prerequisites"></a>Konfigurationskrav för StorSimple-enhet
Hej StorSimple-enhet är en blockenhet och kräver därför en filserver där hello data kan finnas. Vi rekommenderar att du använder en separat server i stället för en befintlig server från hello SharePoint-servergruppen. Den här filservern måste vara på hello samma lokala nätverk (LAN) som hello SQL Server-dator som värdar hello innehållsdatabaserna.

> [!TIP]
> * Om du konfigurerar SharePoint-servergruppen för hög tillgänglighet bör du också distribuera hello filserver för hög tillgänglighet.
> * Om du inte sparar hello innehållsdatabas på hello StorSimple-enhet, Använd traditionella hög tillgänglighet säkerhetsmetoder som har stöd för RBS. SQL Server-kluster stöder RBS, när SQL Server spegling inte. 


Kontrollera att din StorSimple-enheten är korrekt konfigurerad och att volymerna toosupport SharePoint-distributionen har konfigurerats och är tillgänglig från SQL Server-datorn. Gå för[distribuera din lokala StorSimple-enhet](storsimple-8000-deployment-walkthrough-u2.md) om du ännu inte har distribuerat och konfigurerat din StorSimple-enhet. Observera hello IP-adressen för hello StorSimple-enhet. Du behöver den under StorSimple nätverkskort för SharePoint-installationen.

Kontrollera dessutom att hello volym toobe som används för BLOB-externalization uppfyller hello följande krav:

* hello volymen måste vara formaterad med en storlek på allokeringsenhet 64 KB.
* Din frontwebb (WFE) och programservrar måste vara kan tooaccess hello volym via en Universal Naming Convention (UNC)-sökväg.
* hello SharePoint-servergruppen måste vara konfigurerade toowrite toohello volym.

> [!NOTE]
> När du installerar och konfigurerar hello kortet alla BLOB externalization måste gå igenom hello StorSimple-enhet (hello enheten kommer presentera hello volymer tooSQL Server och hantera hello lagringsnivåer). Du kan inte använda några andra mål för BLOB-externalization.


Om du planerar toouse StorSimple Snapshot Manager tootake ögonblicksbilder av hello BLOB och databasdata vara att tooinstall StorSimple Snapshot Manager på hello databasserver så att den kan använda hello SQL-skrivartjänsten tooimplement hello Windows Volume Shadow Copy Service (VSS).

> [!IMPORTANT]
> StorSimple Snapshot Manager stöder inte hello VSS-skrivaren för SharePoint och det går inte att ta programkonsekventa ögonblicksbilder av SharePoint-data. I en SharePoint-scenariot ger StorSimple Snapshot Manager endast kraschsäkra säkerhetskopior.


## <a name="sharepoint-farm-configuration-prerequisites"></a>Konfigurationskrav för SharePoint-grupp
Kontrollera att SharePoint-servergrupp är korrekt konfigurerad, enligt följande:

* Kontrollera att SharePoint-servergrupp är i felfritt tillstånd och kontrollera hello följande:
* Alla SharePoint WFE och programservrar som registrerats i hello servergruppen körs och kan pingas från hello server där du ska installera hello StorSimple nätverkskort för SharePoint.
* hello SharePoint-tidstjänsten (SPTimerV3 eller SPTimerV4) körs på varje WFE-servern och programservern.
* Både hello SharePoint-tidstjänsten och hello IIS-programpoolen hello Central Administration av SharePoint webbplatsen körs under som har administratörsbehörighet.
* Se till att Internet Explorer förbättrad säkerhetskontext (IE ESC) är inaktiverad. Följ dessa steg toodisable IE ESC:
  
  1. Stäng alla instanser av Internet Explorer.
  2. Starta hello Server Manager.
  3. Hello vänster klickar du på **lokal Server**.
  4. På hello Högerklicka rutan bredvid för**Förbättrad säkerhetskonfiguration**, klickar du på **på**.
  5. Under **administratörer**, klickar du på **av**.
  6. Klicka på **OK**.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Krav för Remote BLOB Storage (RBS)
Kontrollera att du använder en version av SQL Server stöds. Endast är hello följande versioner stöds och kan toouse RBS:

* SQL Server 2008 Enterprise Edition
* SQL Server 2008 R2 Enterprise Edition
* SQL Server 2012 Enterprise Edition

Blobbar kan vara externalized på endast de volymer som hello StorSimple-enhet visas tooSQL Server. Inga mål för BLOB-externalization stöds.

När du har slutfört alla nödvändiga konfigurationssteg gå för[installera hello StorSimple-kortet för SharePoint](#install-the-storsimple-adapter-for-sharepoint).

## <a name="install-hello-storsimple-adapter-for-sharepoint"></a>Installera hello StorSimple nätverkskort för SharePoint
Använd hello följande steg tooinstall hello StorSimple nätverkskort för SharePoint. Om du installerar om hello programvara finns [uppgradera eller installera om hello StorSimple nätverkskort för SharePoint](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). hello-tid som krävs för installation av hello beror på hello Totalt antal SharePoint-databaserna i SharePoint-servergrupp.

[!INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]

## <a name="configure-rbs"></a>Konfigurera RBS
När du har installerat hello StorSimple nätverkskort för SharePoint Konfigurera RBS enligt beskrivningen i hello nedan.

> [!TIP]
> Hej StorSimple-kortet för SharePoint ansluts till hello Central Administration av SharePoint-sidan, så att RBS toobe aktiverat eller inaktiverat på varje innehållsdatabas i hello SharePoint-servergrupp. Aktivera eller inaktivera RBS på hello innehållsdatabasen gör emellertid en IIS-återställning som, beroende på din Gruppkonfiguration tillfälligt kan störa hello tillgängligheten för hello SharePoint frontwebb (WFE). (I faktorer, till exempel hello användning av en frontend-belastningsutjämnare, hello aktuella serverbelastning, och så vidare, kan det begränsa eller eliminera den här avbrott.) tooprotect användare från ett avbrott, rekommenderar vi att du aktiverar eller inaktiverar RBS endast under ett planerat underhåll.


[!INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]

## <a name="configure-garbage-collection"></a>Konfigurera skräpinsamling
När objekt tas bort från en SharePoint-webbplats, är de inte bort automatiskt från hello RBS store volym. I stället en asynkron Underhåll bakgrundsprogrammet tar bort överblivna Blobbar från hello-lagring. Administratörer kan schemalägga den här processen toorun regelbundet eller de kan starta den vid behov.

Det här programmet för underhåll (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) installeras automatiskt på alla SharePoint WFE-servrar och programservrar när du aktiverar RBS. hello-programmet är installerat i hello följande plats: *starta enheten*: \Program\Microsoft SQL Remote Blob Storage 10.50\Maintainer\

Information om hur du konfigurerar och använder hello Underhåll programmet finns [Underhåll RBS i SharePoint Server 2013][8].

> [!IMPORTANT]
> hello RBS Underhållaren programmet är resurskrävande. Du schemalägga den toorun under perioder med lågt på hello SharePoint-servergruppen.


### <a name="delete-orphaned-blobs-immediately"></a>Ta bort överblivna Blobbar omedelbart
Du kan använda hello följande instruktioner om du behöver toodelete frånkopplade Blobbar omedelbart. Observera att dessa instruktioner är ett exempel på hur detta kan göras i en SharePoint 2013-miljö med hello följande komponenter:

* hello-innehållsdatabasens namn är WSS_Content.
* hello SQL Server-namnet är SHRPT13 SQL12\SHRPT13.
* hello webbprogrammets namn är SharePoint-80.

[!INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]

## <a name="upgrade-or-reinstall-hello-storsimple-adapter-for-sharepoint"></a>Uppgradera eller installera om hello StorSimple nätverkskort för SharePoint
Använda hello följa proceduren tooupgrade SharePoint server och installera om StorSimple-kortet för uppgradering av SharePoint eller toosimply eller installera om hello nätverkskort i en befintlig SharePoint-servergrupp.

> [!IMPORTANT]
> Granska följande information innan du försöker tooupgrade hello SharePoint-programvaran och/eller uppgradera eller installera om hello StorSimple nätverkskort för SharePoint:
> 
> * Alla filer som flyttats tidigare tooexternal lagring via RBS inte är tillgängliga förrän hello ominstallationen har slutförts och hello RBS funktionen aktiveras igen. toolimit användare påverkas, utföra en uppgradering eller ominstallation under ett planerat underhåll.
> * hello-tid som krävs för hello uppgradering eller ominstallation kan variera beroende på hello Totalt antal SharePoint-databaser i hello SharePoint-servergrupp.
> * När hello uppgradering eller ominstallation är klar, behöver du tooenable RBS för hello innehållsdatabaserna. Se [konfigurera RBS](#configure-rbs) för mer information.
> * Om du konfigurerar RBS för en SharePoint-grupp som har ett stort antal databaser (större än 200), hello **Central Administration av SharePoint** sida kan timeout. Om det inträffar kan du uppdatera hello-sidan. Detta påverkar inte hello konfigurationsprocessen.


[!INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]

## <a name="storsimple-adapter-for-sharepoint-removal"></a>StorSimple-kortet för borttagning av SharePoint
hello följande procedurer beskrivs hur toomove hello BLOB tillbaka innehåll toohello SQL Server-databaser och sedan avinstallera hello StorSimple nätverkskort för SharePoint. 

> [!IMPORTANT]
> Du har toomove hello BLOB tillbaka toohello innehållsdatabaser innan du avinstallerar hello kortets program.


### <a name="before-you-begin"></a>Innan du börjar
Samla in följande information innan du flyttar hello data tillbaka innehåll toohello SQL Server-databaser och börja hello kortet borttagningsprocessen hello:

* hello namnen på alla hello databaser som RBS är aktiverad
* hello UNC-sökvägen för hello konfigurerats blobstore

### <a name="move-hello-blobs-back-toohello-content-databases"></a>Flytta hello BLOB tillbaka toohello innehållsdatabaser
Innan du avinstallerar hello StorSimple nätverkskort för SharePoint-programvaran måste du migrera alla hello Blobbar som har innehåll externalized tillbaka toohello SQL Server-databaser. Om du försöker toouninstall hello StorSimple nätverkskort för SharePoint innan du flyttar alla hello BLOB tillbaka toohello-innehållsdatabaser, visas följande varningsmeddelande hello.

![Varningsmeddelande](./media/storsimple-adapter-for-sharepoint/sasp1.png)

#### <a name="toomove-hello-blobs-back-toohello-content-databases"></a>toomove hello BLOB tillbaka toohello innehållsdatabaser
1. Hämta alla hello externalized objekt.
2. Öppna hello **Central Administration av SharePoint** sidan och Bläddra för**systeminställningar**.
3. Under **Azure StorSimple**, klickar du på **konfigurera StorSimple nätverkskort**.
4. På hello **konfigurera StorSimple nätverkskort** klickar du på hello **inaktivera** knapp under varje hello innehållsdatabaser som du vill tooremove från externa BLOB storage. 
5. Ta bort hello objekt från SharePoint och överför dem igen.

Du kan också använda hello Microsoft` RBS Migrate()` PowerShell-cmdlet som ingår i SharePoint. Mer information finns i [migrerar innehåll till eller från RBS](https://technet.microsoft.com/library/ff628255.aspx).

När du flyttar hello BLOB tillbaka toohello innehållsdatabas gå toohello nästa steg: [avinstallera hello kortet](#uninstall-the-adapter).

### <a name="uninstall-hello-adapter"></a>Avinstallera hello adapter
När du flyttar innehållsdatabaser för hello BLOB tillbaka toohello SQL Server med någon av hello följande alternativ toouninstall hello StorSimple nätverkskort för SharePoint.

#### <a name="toouse-hello-installation-program-toouninstall-hello-adapter"></a>toouse hello installationen programmet toouninstall hello nätverkskort
1. Använd ett konto med administratören privilegier toolog på toohello web front-end (WFE)-server.
2. Dubbelklicka på hello StorSimple nätverkskort för SharePoint-installationsprogrammet. hello installationsguiden startar.
   
    ![Installationsguiden](./media/storsimple-adapter-for-sharepoint/sasp2.png)
3. Klicka på **Nästa**. hello följande sida visas.
   
    ![Guiden Ta bort sidan för installationsprogrammet](./media/storsimple-adapter-for-sharepoint/sasp3.png)
4. Klicka på **ta bort** tooselect hello borttagningen. hello följande sida visas.
   
    ![Konfigurera guiden bekräftelsesidan](./media/storsimple-adapter-for-sharepoint/sasp4.png)
5. Klicka på **ta bort** tooconfirm hello borttagning. hello följa förloppet sida visas.
   
    ![Installationsprogrammet guidesidan pågår](./media/storsimple-adapter-for-sharepoint/sasp5.png)
6. När hello borttagningen är klar visas sidan för hello Slutför. Klicka på **Slutför** tooclose hello installationsguiden.

#### <a name="toouse-hello-control-panel-toouninstall-hello-adapter"></a>toouse hello Kontrollpanelen toouninstall hello nätverkskort
1. Öppna hello på Kontrollpanelen och klicka sedan på **program och funktioner**.
2. Välj **StorSimple-kortet för SharePoint**, och klicka sedan på **avinstallera**.

## <a name="next-steps"></a>Nästa steg
[Lär dig mer om StorSimple](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/en-us/library/ff943565.aspx
