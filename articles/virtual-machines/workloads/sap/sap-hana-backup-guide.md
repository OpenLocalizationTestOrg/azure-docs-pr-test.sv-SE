---
title: "aaaBackup guide för SAP HANA på Azure Virtual Machines | Microsoft Docs"
description: "Säkerhetskopiering guiden för SAP HANA innehåller två huvudsakliga säkerhetskopiering möjligheter för SAP HANA på Azure virtual machines"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
ms.openlocfilehash: e651091bb5da2698ec8bf80cad405bfce5b192cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="backup-guide-for-sap-hana-on-azure-virtual-machines"></a>Säkerhetskopieringsguide för SAP HANA på Azure Virtual Machines

## <a name="getting-started"></a>Komma igång

hello säkerhetskopiering guide för SAP HANA körs på Azure virtual Machines beskriver endast Azure-specifik information. Allmän SAP HANA säkerhetskopiering relaterade objekt, dokumentationen hello SAP HANA (se _SAP HANA säkerhetskopiering dokumentationen_ senare i den här artikeln).

hello fokuserar i den här artikeln på två huvudsakliga säkerhetskopiering möjligheter för SAP HANA på virtuella Azure-datorer:

- HANA säkerhetskopiering toohello filsystemet i en Azure Linux-dator (se [SAP HANA Azure Backup på filnivå](sap-hana-backup-file-level.md))
- HANA säkerhetskopiering baserat på lagring ögonblicksbilder funktionen hello Azure storage blob ögonblicksbild manuellt eller Azure Backup Service (se [SAP HANA säkerhetskopiering baserat på lagring ögonblicksbilder](sap-hana-backup-storage-snapshots.md))

SAP HANA erbjuder en säkerhetskopia API, vilket gör att verktyg för säkerhetskopiering från tredje part toointegrate direkt med SAP HANA. (Som ligger inte inom hello omfånget för den här handboken.) Det finns ingen direkt SAP HANA med Azure Backup-tjänsten tillgänglig höger nu baserat på den här API-integrering.

SAP HANA stöds officiellt på Azure VM typ GS5 som instans med en ytterligare begränsning tooOLAP arbetsbelastningar (se [hitta certifierade IaaS-plattformar](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html) på hello SAP-webbplats). Den här artikeln kommer att uppdateras som nya erbjudanden för SAP HANA på Azure som blir tillgängliga.

Det finns också en SAP HANA-hybridlösning på Azure, där SAP HANA körs icke-virtualiserade på fysiska servrar. Men den här säkerhetskopiering SAP HANA Azure-handboken innehåller en ren Azure-miljön där SAP HANA körs i en Azure VM inte SAP HANA körs på &quot;stora instanser.&quot; Se [SAP HANA (stora instanser) översikt och arkitektur för Azure](hana-overview-architecture.md) för mer information om denna lösning för säkerhetskopiering på &quot;stora instanser&quot; baserat på lagring ögonblicksbilder.

Allmän information om SAP-produkter som stöds i Azure finns i [SAP Obs 1928533](https://launchpad.support.sap.com/#/notes/1928533).

hello följande tre siffror ger en översikt över hello SAP HANA säkerhetskopieringsalternativ för närvarande använder inbyggda funktioner i Azure och visar också tre möjliga scenarier för framtida säkerhetskopiering. hello relaterade artiklar [SAP HANA Azure Backup på filnivå](sap-hana-backup-file-level.md) och [SAP HANA säkerhetskopiering baserat på lagring ögonblicksbilder](sap-hana-backup-storage-snapshots.md) beskrivs de olika alternativen i detalj, inklusive storlek och prestandaöverväganden för SAP HANA säkerhetskopior som har flera terabyte i storlek.

![Den här bilden visar två möjligheter för att spara hello aktuellt tillstånd för virtuell dator](media/sap-hana-backup-guide/image001.png)

Den här bilden visar hello möjlighet att spara hello aktuellt tillstånd för virtuell dator, antingen via Azure Backup-tjänsten eller manuell ögonblicksbilder av Virtuella diskar. Med den här metoden, ett &#39; har toomanage SAP HANA säkerhetskopieringar. hello utmaningen hello disk ögonblicksbild scenario är filsystemkonsekvens och tillståndet programkonsekventa disk. hello konsekvenskontroll avsnittet beskrivs i avsnittet hello _SAP HANA datakonsekvens när du tar lagring ögonblicksbilder_ senare i den här artikeln. Funktioner och begränsningar för Azure Backup-tjänsten-relaterade tooSAP HANA säkerhetskopieringar också beskrivs senare i den här artikeln.

![Den här bilden visar alternativ för att ta en SAP HANA filsäkerhetskopia inuti hello VM](media/sap-hana-backup-guide/image002.png)

Den här bilden visar alternativ för tar en SAP HANA säkerhetskopiering i hello VM och sedan lagra den HANA säkerhetskopior någon annan använder olika verktyg. Ta en säkerhetskopia HANA tar längre tid än en ögonblicksbild-baserad lösning för säkerhetskopiering, men den har fördelar om integritet och konsekvens. Mer information finns nedan.

![Den här bilden visar eventuella framtida SAP HANA säkerhetskopiering scenario](media/sap-hana-backup-guide/image003.png)

Den här bilden illustrerar ett potentiella framtida SAP HANA säkerhetskopiering scenario. Om du SAP HANA säkerhetskopiering från en sekundär replikering, det lägger till ytterligare alternativ för säkerhetskopieringsstrategier. För närvarande kan inte enligt tooa post i hello SAP HANA Wiki:

_&quot;Är det möjligt tootake säkerhetskopieringar på hello sekundära sida?_

_Nej, för närvarande kan endast ta data och loggsäkerhetskopior på hello primära sida. Om automatisk säkerhetskopiering är aktiverad när gäller toohello sekundära sida, skrivas automatiskt det hello säkerhetskopior.&quot;_

## <a name="sap-resources-for-hana-backup"></a>SAP-resurser för HANA säkerhetskopiering

### <a name="sap-hana-backup-documentation"></a>Säkerhetskopiering SAP HANA-dokumentation

- [Introduktion tooSAP HANA Administration](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US)
- [Planera säkerhetskopiering och återställning strategi](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)
- [Schemalägga HANA säkerhetskopia med ABAP DBACOCKPIT](http://www.hanatutorials.com/p/schedule-hana-backup-using-abap.html)
- [Schema för säkerhetskopiering av Data (SAP HANA Cockpit)](http://help.sap.com/saphelp_hanaplatform/helpdata/en/6d/385fa14ef64a6bab2c97a3d3e40292/frameset.htm)
- Vanliga frågor om SAP HANA-säkerhetskopiering på [SAP Obs 1642148](https://launchpad.support.sap.com/#/notes/1642148)
- Vanliga frågor och svar om ögonblicksbilder för SAP HANA-databasen och lagring i [SAP Obs 2039883](https://launchpad.support.sap.com/#/notes/2039883)
- Olämpliga nätverket filsystem för säkerhetskopiering och återställning i [SAP Obs 1820529](https://launchpad.support.sap.com/#/notes/1820529)

### <a name="why-sap-hana-backup"></a>Varför SAP HANA-säkerhetskopiering?

Azure storage erbjuder tillgänglighet och tillförlitlighet out of box hello (se [introduktion tooMicrosoft Azure Storage](../../../storage/common/storage-introduction.md) mer information om Azure storage).

hello minimum för &quot;säkerhetskopiering&quot; är toorely på hello Azure SLA: er att behålla hello SAP HANA data och loggfilen filer på Azure virtuella hårddiskar anslutna toohello SAP HANA server VM. Den här metoden omfattar VM fel, men inte eventuella skador toohello SAP HANA data och loggfiler eller logiska fel som tar bort data eller filer av misstag. Säkerhetskopieringar krävs för efterlevnad eller juridiska skäl. Kort sagt: det behövs alltid en SAP HANA-säkerhetskopieringar.

### <a name="how-tooverify-correctness-of-sap-hana-backup"></a>Hur tooverify är korrekt SAP HANA-säkerhetskopiering
När du använder lagring ögonblicksbilder, bör köra en test-återställning på en annan dator. Den här metoden ger ett sätt tooensure att en säkerhetskopia är rätt och interna processer för säkerhetskopiering och återställning fungerar som förväntat. Även om detta är en betydande överskrida lokalt, är det mycket enklare tooaccomplish i hello molnet genom att tillhandahålla nödvändiga resurser tillfälligt för detta ändamål.

Behåll i åtanke som gör en enkel återställning och kontrollera om HANA är igång och körs är inte tillräckligt. Vi rekommenderar bör en köra en tabell konsekvenskontroll Kontrollera toobe att hello återställa databasen är bra. SAP HANA erbjuder flera typer av konsekvenskontroller som beskrivs i [SAP Obs 1977584](https://launchpad.support.sap.com/#/notes/1977584).

Information om hello tabell konsekvenskontroll finns på hello SAP-webbplats på [tabell och katalogen konsekvenskontroller](http://help.sap.com/saphelp_hanaplatform/helpdata/en/25/84ec2e324d44529edc8221956359ea/content.htm#loio9357bf52c7324bee9567dca417ad9f8b).

En test-återställning är inte nödvändigt för vanliga filsäkerhetskopieringsverktyg. Det finns två SAP HANA-verktyg som hjälper toocheck där säkerhetskopian kan användas för återställning: hdbbackupdiag och hdbbackupcheck. Se [manuellt kontrollerar om en återställning är möjligt](https://help.sap.com/saphelp_hanaplatform/helpdata/en/77/522ef1e3cb4d799bab33e0aeb9c93b/content.htm) för mer information om dessa verktyg.

### <a name="pros-and-cons-of-hana-backup-versus-storage-snapshot"></a>För- och nackdelar HANA säkerhetskopiering jämfört med lagring ögonblicksbild

SAP &#39; t ge inställningar tooeither HANA säkerhetskopiering jämfört med lagring ögonblicksbild. Visas en lista med sina för- och nackdelar, så en kan avgöra vilken toouse beroende på hello situation och tillgängliga lagringsteknik (se [planera din säkerhetskopiering och återställning strategi](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)).

På Azure, vara medveten om hello faktum att hello Azure blob ögonblicksbild funktionen &#39; t garantera filsystemkonsekvens (se [Using blob-ögonblicksbilder med PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/)). Hej nästa avsnitt, _SAP HANA datakonsekvens när du tar lagring ögonblicksbilder_, beskriver vissa överväganden om den här funktionen.

Dessutom har toounderstand hello fakturering effekter när du arbetar ofta med blob ögonblicksbilder som beskrivs i den här artikeln: [förstå hur ögonblicksbilder påförs kostnader](/rest/api/storageservices/understanding-how-snapshots-accrue-charges)– den inte är &#39; t som uppenbara som använder Azure virtuella diskar.

### <a name="sap-hana-data-consistency-when-taking-storage-snapshots"></a>SAP HANA datakonsekvens när du tar ögonblicksbilder för lagring

System- och konsekvens i filen är ett komplicerat problem när du tar ögonblicksbilder för lagring. hello enklaste sättet tooavoid problem skulle vara tooshut ned SAP HANA eller kanske även hello hela virtuella datorn. En avstängning kanske doable med en demonstration eller prototyp eller även utvecklingssystemet, men det är inte ett alternativ för ett produktionssystem.

I Azure det, är ett tookeep ihåg att hello Azure blob ögonblicksbild funktionen &#39; t garantera filsystemkonsekvens. Den fungerar med hjälp av hello ögonblicksbild men SAP HANA-funktionen så länge det finns bara en virtuell disk som ingår. Men även med en enskild disk ytterligare objekt toobe markerad. [SAP Obs 2039883](https://launchpad.support.sap.com/#/notes/2039883) har viktig information om SAP HANA säkerhetskopieringar via storage snapshots. Till exempel nämns att med hello XFS filsystem, är det nödvändigt toorun **xfs\_låsa** innan du startar en ögonblicksbild tooguarantee konsekvenskontroll (se [xfs\_freeze(8) - Linux man sidan](https://linux.die.net/man/8/xfs_freeze) information om **xfs\_låsa**).

hello avsnittet enhetlighet blir ännu mer utmanande i fall där en enda filsystemet sträcker sig över flera diskar/volymer. Till exempel med hjälp av mdadm eller LVM och striping. hello SAP-kommentar som nämns ovan tillstånd:

_&quot;Men kom ihåg att hello lagringssystemet har tooguarantee i/o-konsekvenskontroll när du skapar en ögonblicksbild av lagring per volym för SAP HANA-data, t.ex. ögonblicksbilder för en SAP HANA tjänstspecifika datavolymen måste vara en atomisk åtgärd.&quot;_

Under förutsättning att det finns ett XFS filsystem utsträckning fyra Azure virtuella diskar, hello följande åtgärder för en programkonsekvent ögonblicksbild som representerar hello HANA dataområdet:

- Förbereda HANA ögonblicksbild
- Lås hello filsystem (till exempel använda **xfs\_låsa**)
- Skapa alla nödvändiga blob-ögonblicksbilder på Azure
- Lås upp hello filsystemet
- Bekräfta hello HANA ögonblicksbild

Rekommendation är toouse hello proceduren ovan i alla fall toobe på hello säkra sidan, oavsett vilket filsystem. Eller om det är en enskild disk eller striping via mdadm eller LVM över flera diskar.

Det är viktigt tooconfirm hello HANA ögonblicksbild. På grund av toohello &quot;kopiering vid skrivning,&quot; SAP HANA kanske inte kräver ytterligare diskutrymme i detta förbereda läge ögonblicksbild. Den &#39; s också inte möjligt toostart nya säkerhetskopior tills hello SAP HANA ögonblicksbild har bekräftats.

Azure Backup-tjänsten använder Azure VM-tillägg tootake vård hello filsystemkonsekvens. Dessa VM-tillägg är inte tillgängliga för fristående användning. En har fortfarande toomanage SAP HANA konsekvenskontroll. Se hello relaterad artikel [SAP HANA Azure Backup på filnivå](sap-hana-backup-file-level.md) för mer information.

### <a name="sap-hana-backup-scheduling-strategy"></a>SAP HANA schemaläggning säkerhetskopieringsstrategi

hello SAP HANA artikel [planera din säkerhetskopiering och återställning strategi](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm) tillstånd för en grundläggande planera toodo säkerhetskopieringar:

- Lagring ögonblicksbild (varje dag)
- Fullständig säkerhetskopiering med hjälp av filen eller bacint format (en gång i veckan)
- Automatisk loggsäkerhetskopior

Du kan också kan en gå helt utan ögonblicksbilder av lagring de kan ersättas av HANA delta säkerhetskopieringar som inkrementella eller differentiella säkerhetskopieringar (se [Delta säkerhetskopieringar](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm)).

hello HANA Administration guide innehåller en exempel-lista. Det tyder på att en återställa SAP HANA tooa viss punkt i tiden använder följande sekvens av säkerhetskopieringar hello:

1. Fullständig säkerhetskopiering
2. Differentiell säkerhetskopiering
3. Inkrementell säkerhetskopiering 1
4. Inkrementell säkerhetskopiering 2
5. Loggsäkerhetskopior

Om ett exakt schema som toowhen och hur ofta en viss typ av säkerhetskopiering ska hända, är det inte möjligt toogive Allmänt – det är för kundspecifika och beror på hur många data dataändringar hello system. En grundläggande rekommendation från SAP-sida, vilket kan ses som en allmän vägledning är toomake en fullständig HANA säkerhetskopiering en gång i veckan.
Angående loggsäkerhetskopior hello SAP HANA dokumentationen [Loggsäkerhetskopior](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm).

SAP rekommenderar även göra vissa underhåll av hello säkerhetskopieringskatalogen tookeep från växande oändligt (se [Housekeeping för säkerhetskopieringskatalogen och Backup Storage](http://help.sap.com/saphelp_hanaplatform/helpdata/en/ca/c903c28b0e4301b39814ef41dbf568/content.htm)).

### <a name="sap-hana-configuration-files"></a>Konfigurationsfiler för SAP HANA

Enligt informationen i hello vanliga frågor och svar i [SAP Obs 1642148](https://launchpad.support.sap.com/#/notes/1642148), hello SAP HANA konfigurationsfilerna inte är en del av en standard säkerhetskopiering HANA. De är inte viktigt toorestore ett system. hello HANA konfigurationen kan ändras manuellt efter hello återställning. Om en vill tooget hello samma anpassad konfiguration under hello återställningsprocessen är det nödvändigt tooback hello HANA configuration-filer separat.

Om standard HANA säkerhetskopior kommer tooa dedikerad HANA säkerhetskopian system, en kan också kopiera hello configuration filer toohello samma säkerhetskopiera filsystemet och kopiera allt tillsammans toohello slutliga lagringsplats som kall blob-lagring.

### <a name="sap-hana-cockpit"></a>SAP HANA Cockpit

SAP HANA Cockpit erbjuder hello möjligheten att övervaka och hantera SAP HANA via en webbläsare. Det kan också hantering av SAP HANA säkerhetskopieringar och därför kan användas som ett alternativt tooSAP HANA Studio och ABAP DBACOCKPIT (se [SAP HANA Cockpit](https://help.sap.com/saphelp_hanaplatform/helpdata/en/73/c37822444344f3973e0e976b77958e/content.htm) mer information).

![Den här bilden visar hello SAP HANA Cockpit databasen Administration skärmen](media/sap-hana-backup-guide/image004.png)

Den här bilden visar hello SAP HANA Cockpit databasen Administration skärmen och hello säkerhetskopiering panelen på hello kvar. Se hello säkerhetskopiering panelen kräver lämpliga användarbehörigheter för inloggningskontot.

![Säkerhetskopieringar kan övervakas i SAP HANA Cockpit medan de är pågående](media/sap-hana-backup-guide/image005.png)

Säkerhetskopieringar kan övervakas i SAP HANA Cockpit medan de är pågående, och när den är klar, alla hello säkerhetskopiering information är tillgänglig.

![Ett exempel på en Azure SLES 12 VM med gör väldigt lätt desktop Firefox](media/sap-hana-backup-guide/image006.png)

hello föregående skärmbilderna har gjorts från en Windows Azure-VM. Det här är ett exempel på en Azure SLES 12 VM med gör väldigt lätt desktop Firefox. Den visar hello alternativet toodefine SAP HANA scheman för säkerhetskopiering i Cockpit för SAP HANA. Som en kan också se föreslår datum/tid som ett prefix för hello säkerhetskopior. SAP HANA Studio är hello standard prefixet &quot;Slutför\_DATA\_säkerhetskopiering&quot; när du gör en fullständig säkerhetskopia. Du bör använda ett unikt prefix.

### <a name="sap-hana-backup-encryption"></a>SAP HANA säkerhetskopia av krypteringsnyckeln

SAP HANA erbjuder kryptering av data och loggfilen. Om SAP HANA-data och loggfilen inte krypteras kan sedan krypteras hello säkerhetskopieringar också inte. Är det upp toohello kunden toouse någon form av lösning från tredje part tooencrypt hello SAP HANA säkerhetskopieringar. Se [Data och loggfilen volymkryptering](https://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/01f36fbb5710148b668201a6e95cf2/content.htm) toofind mer information om SAP HANA-kryptering.

En kund kan använda hello IaaS VM kryptering funktionen tooencrypt i Microsoft Azure. En kan exempelvis använda dedicerade data diskar anslutna toohello VM, som är används toostore SAP HANA säkerhetskopior och sedan göra kopior av dessa diskar.

Azure Backup-tjänsten kan hantera krypterade VMs-diskar (se [hur tooback upp och återställning av krypterade virtuella datorer med Azure Backup](../../../backup/backup-azure-vms-encryption.md)).

Ett annat alternativ skulle toomaintain hello SAP HANA VM och dess diskar utan kryptering och lagra hello SAP HANA-säkerhetskopior i ett lagringskonto som kryptering aktiverades (se [Azure Storage Service-kryptering för Data i vila](../../../storage/common/storage-service-encryption.md)) .

## <a name="test-setup"></a>Testa installationen

### <a name="test-virtual-machine-on-azure"></a>Testa virtuell dator på Azure

En SAP HANA-installation i en Azure GS5 VM användes för hello efter säkerhetskopiering/återställning tester.

![Den här bilden visar en del av hello översikt av Azure portal för hello HANA VM](media/sap-hana-backup-guide/image007.png)

Den här bilden visar en del av hello översikt av Azure portal för hello HANA VM.

### <a name="test-backup-size"></a>Testa säkerhetskopieringsstorlek

![Denna bild har kopplats från hello säkerhetskopiering konsolen i HANA Studio och visar hello säkerhetskopian storlek på 229 GB för hello HANA indexserver](media/sap-hana-backup-guide/image008.png)

En dummy tabell fylldes med data tooget över 200 GB tooderive realistiska prestandadata totala data säkerhetskopiering storleken. hello bild hämtades från hello säkerhetskopiering konsolen i HANA Studio som visar hello säkerhetskopian storlek på 229 GB för hello HANA indexserver. För hello tester användes hello säkerhetskopiering standardprefix ”COMPLETE_DATA_BACKUP” i SAP HANA Studio. Ett mer användbar prefix ska vara definierat i verklig produktionssystem. SAP HANA Cockpit föreslår datum/tid.

### <a name="test-tool-toocopy-files-directly-tooazure-storage"></a>Testa verktyget toocopy filer direkt tooAzure lagring

tootransfer SAP HANA säkerhetskopierade filer direkt tooAzure blob storage eller Azure-filresurser hello blobxfer verktyget användes eftersom den stöder både mål och den kan enkelt integreras i automatiseringsskript på grund av tooits kommandoradsgränssnitt. Hej blobxfer verktyget är tillgängligt på [GitHub](https://github.com/Azure/blobxfer).

### <a name="test-backup-size-estimation"></a>Testa säkerhetskopieringsstorlek uppskattning

Det är viktigt tooestimate hello säkerhetskopiering storleken för SAP HANA. Denna uppskattning hjälper tooimprove prestanda genom att definiera Hej max säkerhetskopians storlek för ett antal säkerhetskopior, på grund av tooparallelism under en filkopia. (Dessa uppgifter beskrivs senare i den här artikeln.) En måste också bestämma om toodo en fullständig säkerhetskopiering eller en Deltasynkronisering säkerhetskopiering (inkrementell eller differentiell).

Lyckligtvis finns en enkel SQL-sats som beräknar hello storleken på hello säkerhetskopior: **Välj \* från M\_säkerhetskopiering\_storlek\_UPPSKATTNINGAR** (se [ Uppskattningen hello utrymme behövs i hello filsystem för säkerhetskopiering av Data](https://help.sap.com/saphelp_hanaplatform/helpdata/en/7d/46337b7a9c4c708d965b65bc0f343c/content.htm)).

![hello utdata från den här SQL-instruktionen matchar nästan exakt hello verkliga storleken på hello fullständig säkerhetskopiering på disk](media/sap-hana-backup-guide/image009.png)

För hello testsystemet matchar hello utdata från den här SQL-instruktionen nästan exakt hello verkliga storleken på hello fullständig säkerhetskopiering på disk.

### <a name="test-hana-backup-file-size"></a>Testa HANA säkerhetskopians storlek

![hello HANA Studio säkerhetskopiering konsolen kan en toorestrict hello maximal filstorlek HANA säkerhetskopiering av filer](media/sap-hana-backup-guide/image010.png)

hello HANA Studio säkerhetskopiering konsolen kan en toorestrict hello maximal filstorlek HANA säkerhetskopiering av filer. I hello exempel miljö gör funktionen det möjligt tooget flera mindre säkerhetskopieringsfilerna i stället för en 230 GB säkerhetskopian. Mindre filstorlek har en betydande inverkan på prestanda (se hello relaterad artikel [SAP HANA Azure Backup på filnivå](sap-hana-backup-file-level.md)).

## <a name="summary"></a>Sammanfattning

Baserat på hello testresultaten hello följande tabeller visar för- och nackdelar lösningar tooback in en SAP HANA-databas som körs på virtuella Azure-datorer.

**Säkerhetskopiera SAP HANA toohello filsystemet och kopiera säkerhetskopieringsfilerna efteråt toohello slutliga mål för säkerhetskopian**

|Lösning                                           |Tekniker                                 |Nackdelar                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|Behåll HANA säkerhetskopior på Virtuella diskar                      |Inget ytterligare arbete     |Eats lokala VM diskutrymme           |
|Blobxfer verktyget toocopy säkerhetskopior tooblob lagring |Parallellitet toocopy flera filer, val toouse kall blob-lagring | Ytterligare verktyget underhåll och anpassade skript | 
|BLOB-kopiering via Powershell eller CLI                    |Inget ytterligare verktyg som behövs, kan utföras via Azure Powershell eller CLI |manuell process kunden har tootake vård skript och hantering av kopierade BLOB för återställning|
|Kopiera tooNFS resursen                                  |Efter bearbetning av säkerhetskopior på andra Virtuella utan påverkan på hello HANA server|Långsam kopieringsprocessen|
|Blobxfer kopiera tooAzure Filtjänst                |Inte äta utrymme på den lokala Virtuella diskar|Ingen direkt skriva stöd av HANA säkerhetskopiering, storleksbegränsningen på filresursen för närvarande på 5 TB|
|Azure Backup-agenten                                 | Är en bättre lösning         | För närvarande inte tillgänglig på Linux    |



**Säkerhetskopiering SAP HANA baserat på lagring ögonblicksbilder**

|Lösning                                           |Tekniker                                 |Nackdelar                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|Azure Backup-tjänsten                               | Kan säkerhetskopiering baserat på blob ögonblicksbilder | När du använder inte filen nivån återställning, krävs hello skapandet av en ny virtuell dator för hello återställningsprocessen, vilket innebär hello behovet av en ny nyckel för SAP HANA-licens|
|Manuell blob ögonblicksbilder                              | Flexibilitet toocreate och återställning av specifika Virtuella diskar utan att ändra hello unikt ID för VM|Alla manuellt arbete som har toobe hello kunden har gjort|

## <a name="next-steps"></a>Nästa steg
* [SAP HANA Azure Backup på filnivå](sap-hana-backup-file-level.md) beskriver hello filbaserade alternativ för säkerhetskopiering.
* [SAP HANA säkerhetskopiering baserat på lagring ögonblicksbilder](sap-hana-backup-storage-snapshots.md) beskriver hello snapshot-baserad säkerhetskopiering lagringsalternativ.
* hur tooestablish hög tillgänglighet och planera för katastrofåterställning för SAP HANA i Azure (stora instanser), se toolearn [SAP HANA (stora instanser) hög tillgänglighet och katastrofåterställning recovery på Azure](hana-overview-high-availability-disaster-recovery.md).
