---
title: "aaaHigh tillgänglighet och katastrofåterställning återställning av SAP HANA i Azure (stora instanser) | Microsoft Docs"
description: "Upprätta hög tillgänglighet och planera för katastrofåterställning för SAP HANA i Azure (stora instanser)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0c0967f54cf29bbb275eb7cda9d36608488add9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>SAP HANA (stora instanser) hög tillgänglighet och katastrofåterställning recovery på Azure 

Hög tillgänglighet och katastrofåterställning är viktiga aspekter av din verksamhetskritiska SAP HANA i Azure (stora instanser) servrar. Det är viktigt toowork med SAP, din systemintegreraren, eller Microsoft tooproperly systemarkitekter och implementera hello strategi för åt höger hög-tillgänglighet/katastrofåterställning. Det är också viktigt tooconsider hello återställningspunktmål och mål, som är specifika tooyour miljö.

## <a name="high-availability"></a>Hög tillgänglighet

Microsoft stöder SAP HANA hög tillgänglighet metoder ”out-of-hello rutan” bland annat:

- **Storage-replikering:** hello storage-system kan tooreplicate alla tooanother plats (inom, eller separat från hello samma datacenter). SAP HANA fungerar oberoende av den här metoden.
- **HANA system replikering:** hello replikering av alla data i SAP HANA tooa separat SAP HANA-system. mål för hello minimeras via datareplikering med jämna mellanrum. SAP HANA stöder asynkrona, synkron i minnet och synkront läge (rekommenderas endast för SAP HANA system som är inom hello samma datacenter eller mindre än 100 KM från varandra). I hello aktuella designen av HANA stora instans stämplar kan HANA system replikering användas för hög tillgänglighet.
- **Värd för automatisk redundans:** en lokal fel återställningspunkt lösning toouse som en alternativ toosystem replikering. När hello huvudnoden blir otillgänglig, en eller flera vänteläge SAP HANA-noder som är konfigurerade i skalbar läge och SAP HANA växlar automatiskt tooanother nod.

Mer information om hög tillgänglighet för SAP HANA finns hello följande SAP-information:

- [Vitboken för SAP HANA hög tillgänglighet](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [SAP HANA Administrationsguide](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP Academy videon på SAP HANA System-replikering](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP stöd Obs #1999880 – vanliga frågor och svar på SAP HANA System-replikering](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- [SAP stöd Obs #2165547 – SAP HANA säkerhetskopiering och återställning i SAP HANA-systemmiljön replikering](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [SAP stöd Obs #1984882 – med SAP HANA System replikering för maskinvara Exchange med minsta/noll avbrottstid](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="disaster-recovery"></a>Haveriberedskap

SAP HANA i Azure (stora instanser) erbjuds i två Azure-regioner i en geopolitiska region. Är en direkt nätverksanslutning för att replikera data vid katastrofåterställning mellan hello två stora instans stämplar för två olika regioner. hello replikeras hello data lagringsinfrastruktur baserat. hello replikering görs inte som standard. Den är klar för hello kundkonfigurationer som sorterade hello katastrofåterställning. HANA system replikering kan inte användas för katastrofåterställning i hello aktuella designen.

Dock tootake nytta av hello katastrofåterställning, behöver du toostart toodesign hello network connectivity toohello två olika Azure-regioner. toodo så du behöver ett Azure ExpressRoute-kretsen anslutningen från lokala i dina viktigaste Azure-regionen och en annan krets anslutning från lokala tooyour katastrofåterställning region. Det här måttet bör ge en situation där en fullständig Azure-region, inklusive en Microsoft enterprise edge router (MSEE) plats, har ett problem.

Du kan ansluta alla virtuella Azure-nätverk som ansluter tooSAP HANA i Azure (stora instanser) i ett hello regioner tooboth av dessa ExpressRoute-kretsar som en andra åtgärd. Det här måttet adresser fall där endast en av platserna som hello MSEE som ansluter din lokala plats med Azure kan lämna avgift.

hello visar följande bild hello optimal konfiguration för katastrofåterställning:

![Bästa konfigurationen för katastrofåterställning](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)

hello är optimala fallet för en katastrofåterställning konfigurationen av hello nätverket toohave två ExpressRoute-kretsar från lokala toohello två olika Azure-regioner. En krets går tooregion #1, kör en instans för produktion. hello andra ExpressRoute-krets går tooregion #2, kör vissa icke-produktion HANA instanser. (Detta är viktigt om en hel Azure-region, inklusive hello MSEE och stora instans stämpel stängs av hello rutnät.)

Som en andra åtgärd hello olika virtuella nätverk är anslutna toohello olika ExpressRoute-kretsar som är anslutna tooSAP HANA i Azure (stora instanser). Du kan kringgå hello plats där en MSEE misslyckas eller så kan du sänka hello mål för återställningspunkt för katastrofåterställning, diskussion senare.

hello nästa kraven för en katastrofåterställning installation är:

- På Azure (stora instanser) SKU: er av hello samma storlek som din produktion SKU: er och distribuera dem i hello katastrofåterställning region måste du sortera SAP HANA. Dessa instanser kan vara används toorun test, sandbox eller QA HANA instanser.
- Du måste sortera en profil för katastrofåterställning för varje din SAP HANA på Azure (stora instanser) SKU: er som du vill toorecover i hello återställningsplats, om det behövs. Den här åtgärden leder toohello fördelning av lagringsvolymer som hello målet för hello lagringsreplikering från din produktionsregion till hello katastrofåterställning region.

När du uppfyller hello föregående krav är ditt ansvar toostart hello storage-replikering. I hello lagringsinfrastruktur som används för SAP HANA i Azure (stora instanser), är hello basen för storage-replikering lagring ögonblicksbilder. toostart hello katastrofåterställning replikering, behöver du tooperform:

- En ögonblicksbild av startenheter LUN, enligt beskrivningen ovan.
- En ögonblicksbild av dina HANA-relaterade volymer, som tidigare beskrivits.

När du kör dessa ögonblicksbilder, dirigeras en första replik av hello volymer på hello volymer som är associerade med din profil för katastrofåterställning i hello katastrofåterställning region.

Därefter hello senaste lagring ögonblicksbild används varje timme tooreplicate hello går som utvecklar på hello lagringsvolymer.

hello mål för återställningspunkt som uppnås med den här konfigurationen är 60 minuter för too90. tooachieve återställning av en bättre återställningspunktmål i hello katastrofåterställning fallet kopiera hello HANA säkerhetskopieringar av transaktionsloggen från SAP HANA på Azure (stora instanser) toohello andra Azure-region. tooachieve denna återställningspunktmål hello följande:

1. Logga så ofta som möjligt för eller hana/log/säkerhetskopiering säkerhetskopierar hello HANA transaktion.
2. Kopiera hello säkerhetskopieringar av transaktionsloggen när de är klar tooan Azure virtuell dator (VM), som är i ett virtuellt nätverk som har anslutit toohello SAP HANA på Azure (stora instanser)-server.
3. Kopiera hello säkerhetskopiering tooa virtuell dator som är i ett virtuellt nätverk i hello katastrofåterställning region från den virtuella datorn.
4. Behåll hello transaktion säkerhetskopior i regionen i hello VM.

Vid katastrofåterställning, när hello katastrofåterställning profil har distribuerats på en faktiska server, kopierar hello säkerhetskopieringar av transaktionsloggen från hello VM toohello SAP HANA i Azure (stora instanser) som är nu hello primära servern i hello katastrofåterställning region, och återställa dessa säkerhetskopior. Denna återställning är möjlig eftersom hello HANA på hello katastrofåterställning diskar är som en HANA ögonblicksbild. Detta är hello förskjutningen för ytterligare återställningar av säkerhetskopieringar av transaktionsloggen.

## <a name="backup-and-restore"></a>Säkerhetskopiering och återställning

Kontrollera hello de viktigaste aspekterna toooperating databaserna hello databas kan skyddas från olika katastrofer. Dessa händelser kan orsakas av något från naturkatastrof toosimple användarfel.

Säkerhetskopiera en databas med hello möjlighet toorestore den tooany punkt i tiden (som innan någon bort kritiska data), kan återställningen tooa tillstånd som är så nära som möjligt toohello sätt innan hello avbrott inträffade.

Två typer av säkerhetskopior måste utföras för bästa resultat:

- Säkerhetskopior av databasen
- Säkerhetskopieringar av transaktionsloggen

Dessutom toofull databassäkerhetskopieringar utförs på en på programnivå, du kan vara ännu mer omfattande genom att utföra säkerhetskopieringar med lagring ögonblicksbilder. Utför säkerhetskopior är också viktigt för att återställa databasen hello (och tooempty hello loggar från redan genomförda transaktioner).

SAP HANA i Azure (stora instanser) erbjuder två alternativ för säkerhetskopiering och återställning:

- Göra det själv (DIY). När du beräknar tooensure tillräckligt mycket ledigt diskutrymme, utföra fullständiga säkerhetskopieringar för databasen och loggfiler med disk säkerhetskopieringsmetoder (toothose diskar). Över tiden, hello säkerhetskopior är kopierade tooan Azure storage-konto (när du har skapat ett Azure-baserad filserver med praktiskt taget obegränsade storage) eller Använd ett Azure Backup-valv eller Azure kall lagring. Ett annat alternativ är toouse ett tredje parts data protection verktyg, till exempel Commvault toostore hello säkerhetskopieringar när de har kopierats tooa storage-konto. hello själv alternativet kan också krävas för data som behöver toobe lagras under längre perioder för kompatibilitet och granskning.
- Använd hello säkerhetskopiering och återställning av funktioner som hello underliggande infrastruktur för SAP HANA i Azure (stora instanser) tillhandahåller. Det här alternativet uppfyller hello behovet av säkerhetskopieringar och det gör manuella säkerhetskopieringar nästan föråldrade (utom där säkerhetskopiering av data krävs för efterlevnad). hello resten av det här avsnittet adresser hello säkerhetskopiering och återställning av funktioner som erbjuds med HANA (stora instanser).

> [!NOTE]
> hello ögonblicksbild teknik som används av hello underliggande infrastruktur HANA (stora instanser) har ett beroende på SAP HANA ögonblicksbilder. SAP HANA ögonblicksbilder fungerar inte tillsammans med SAP HANA Multitenant databasen behållare. Den här metoden för säkerhetskopiering måste därför använda toodeploy SAP HANA Multitenant databasen behållare.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Med hjälp av ögonblicksbilder för lagring av SAP HANA i Azure (stora instanser)

hello lagringsinfrastruktur underliggande SAP HANA i Azure (stora instanser) stöder hello begreppet en ögonblicksbild för lagring av volymer. Både säkerhetskopiering och återställning av en viss volym stöds, med hello följande överväganden:

- I stället för säkerhetskopiering av databaser vidtas ögonblicksbilder av lagring regelbundet.
- hello lagring ögonblicksbild initierar en SAP HANA-ögonblicksbild innan hello lagring ögonblicksbild körs. SAP HANA ögonblicksbilden är hello installationsprogrammet punkt för eventuell loggen återställningar efter återställning av hello lagring ögonblicksbild.
- Vid hello punkt där hello lagring ögonblicksbild har körts, tas hello SAP HANA ögonblicksbild bort.
- Säkerhetskopior av tas ofta och lagras i hello loggvolym för säkerhetskopiering eller i Azure.
- Om hello databas måste återställas tooa vissa punkt i tiden, en förfrågan görs tooMicrosoft Azure-supporten (produktion avbrott) eller SAP HANA på Azure-tjänsthantering toorestore tooa vissa lagring ögonblicksbild (till exempel en planerad återställning av ett begränsat system tooits ursprungliga tillstånd).
- hello SAP HANA ögonblicksbild som ingår i hello lagring ögonblicksbild är en offset för att tillämpa säkerhetskopior som har körts och lagrade när hello lagring ögonblicksbilden togs.
- Dessa säkerhetskopior kommer toorestore hello databasen tillbaka tooa vissa peka i tid.

Säkerhetskopiering för att ange hello\_namn skapas en ögonblicksbild av hello följande volymer:

- Hana-data
- Hana/logg
- Hana eller in\_säkerhetskopiering (monterade som säkerhetskopia hana/loggfiler)
- Hana/delat

### <a name="storage-snapshot-considerations"></a>Överväganden för ögonblicksbild av lagring

>[!NOTE]
>Lagring ögonblicksbilderna _inte_ tillhandahålls utan kostnad, eftersom ytterligare lagringsutrymme måste allokeras.

hello specifika säkerhetsnivån lagring ögonblicksbilder för SAP HANA i Azure (stora instanser) inkluderar:

- En särskilda lagrings-ögonblicksbild (vid hello tidpunkt när den tas) använder mycket lite lagringsutrymme.
- Hello ögonblicksbild måste toostore hello ursprungliga blockera innehåll som data innehållsändringar och hello innehållet i filer som ändras på hello lagringsvolym SAP HANA-data.
- hello lagring ögonblicksbild ökar i storlek. hello längre hello ögonblicksbild finns, blir hello större hello lagring ögonblicksbild.
- Hej flera ändringar toohello SAP HANA-databasvolymen över hello livslängden för en ögonblicksbild av lagring, blir hello större hello förbrukningen av diskutrymme i hello lagring ögonblicksbilden.

SAP HANA i Azure (stora instanser) levereras med fast Volymstorlekar för hello SAP HANA data och loggfilen volym. Utför ögonblicksbilder av volymerna eats i utrymmet på volymen så att det är ditt ansvar tooschedule lagring ögonblicksbilder (inom hello SAP HANA på Azure [stora instanser] process).

hello innehåller följande avsnitt information för att utföra dessa ögonblicksbilder, inklusive allmänna rekommendationer:

- Även om hello maskinvara kan klara 255 ögonblicksbilder per volym, rekommenderar vi att du hålla betydligt lägre än detta antal.
- Innan du utför lagring av ögonblicksbilder, övervaka och hålla reda på ledigt utrymme.
- Lägre hello antal lagring ögonblicksbilderna baserat på ledigt utrymme. Du kan behöva toolower hello antal ögonblicksbilder som du behåller eller så kanske måste tooextend hello volymer. (Du kan ordna ytterligare lagringsutrymme i enheter som 1 TB.)
- Vi rekommenderar att du inte utför alla ögonblicksbilder för lagring under aktiviteter, till exempel data flyttas till SAP HANA med migrering Systemverktyg (R3load eller genom att återställa databaser för SAP HANA från säkerhetskopior). (Om en system-migrering görs på ett nytt SAP HANA-system, lagring ögonblicksbilder inte behöver toobe utförs.)
- Under större omorganisering av SAP HANA tabeller bör lagring ögonblicksbilder undvikas om möjligt.
- Lagring ögonblicksbilder är en nödvändig tooengaging hello katastrofåterställning funktionerna för SAP HANA i Azure (stora instanser).

### <a name="setting-up-storage-snapshots"></a>Konfigurera lagring ögonblicksbilder

1. Kontrollera att Perl är installerat i hello Linux-operativsystem på hello HANA (stora instanser) server.
2. Ändra/etc/ssh/ssh\_config tooadd hello rad _Macintosh hmac-sha1_.
3. Skapa en SAP HANA säkerhetskopiering användarkonto på hello huvudnoden för varje SAP HANA-instans som du kör (om tillämpligt).
4. hello SAP HANA HDB klienten måste installeras på alla servrar för SAP HANA (stora instanser).
5. En offentlig nyckel måste skapas tooaccess hello underliggande lagringsinfrastruktur som styr skapa ögonblicksbilden för på hello första SAP HANA (stora instanser) server för varje region.
6. Kopiera hello skript azure\_hana\_backup.pl från/skript toohello finns i **hdbsql** av hello SAP HANA-installationen.
7. Kopiera hello HANABackupDetails.txt filen från/skript toohello samma plats som hello Perl-skript.
8. Ändra hello HANABackupDetails.txt filen efter behov för hello lämpliga kundens specifikationer.

### <a name="step-1-install-sap-hana-hdbclient"></a>Steg 1: Installera SAP HANA HDBClient

hello Linux är installerade på SAP HANA i Azure (stora instanser) innehåller hello mappar och skript nödvändiga tooexecute SAP HANA lagring ögonblicksbilder för säkerhetskopiering och katastrofåterställning. Men är det ditt ansvar tooinstall SAP HANA HDBclient medan du installerar SAP HANA. (Microsoft installerar varken hello HDBclient eller SAP HANA.)

### <a name="step-2-change-etcsshsshconfig"></a>Steg 2: Ändra/etc/ssh/ssh\_config

Ändra/etc/ssh/ssh\_config genom att lägga till _Macintosh hmac-sha1_ rad som visas här:
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

### <a name="step-3-create-a-public-key"></a>Steg 3: Skapa en offentlig nyckel

Den första SAP HANA på Azure (stora instanser)-server i varje Azure-region, skapa en infrastruktur för offentliga nycklar toobe används tooaccess hello lagring så att du kan skapa ögonblicksbilder på hello. hello offentliga nyckeln garanterar att ett lösenord inte är obligatoriska toosign i toohello lagring och inte bevaras lösenordsinformation. I Linux på hello SAP HANA (stora instanser) server, kör du hello efter kommandot toogenerate hello offentlig nyckel:
```
  ssh-keygen –t dsa –b 1024
```
hello ny plats är _/root/.ssh/id\_dsa.pub. Ange inte en verklig lösenfras, eller så kommer du att nödvändiga tooenter hello lösenfras varje gång du loggar in. Utan på **RETUR** två gånger tooremove hello ange lösenfras krav för att logga in.

Kontrollera att den offentliga nyckeln hello korrigerades som förväntat genom att ändra mappar too/root/.ssh/ och sedan köra hello toomake **ls** kommando. Om hello nyckeln finns kan du kopiera den genom att köra följande kommando hello:

![Offentliga nyckeln kopieras genom att köra det här kommandot](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Nu kontakta SAP HANA på Azure-tjänsthantering och ange hello nyckel. hello representant använder hello offentliga nyckel tooregister i hello underliggande lagringsinfrastruktur.

### <a name="step-4-create-an-sap-hana-user-account"></a>Steg 4: Skapa ett användarkonto för SAP HANA

Skapa ett användarkonto för SAP HANA SAP HANA Studio för säkerhetskopiering. Det här kontot måste ha följande behörigheter hello: _säkerhetskopiering Admin_ och _katalog viktigt_. I det här exemplet hello användarnamn SCADMIN skapas.

![Skapa en användare i HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-hello-sap-hana-user-account"></a>Steg 5: Ge hello SAP HANA-användarkonto

Auktorisera hello SAP HANA-användarkonto (toobe används av hello skript utan tillstånd varje gång hello skript körs). hello SAP HANA-kommandot `hdbuserstore` kan hello skapas en SAP HANA användarnyckel, vilken lagras på en eller flera SAP HANA-noder. hello användarnyckel kan också hello användaren tooaccess SAP HANA utan toomanage lösenord från inom hello scripting process som beskrivs senare.

>[!IMPORTANT]
>Kör hello följande kommando som `_root_`. Annars fungerar hello skriptet inte korrekt.

Ange hello `hdbuserstore` kommandot på följande sätt:

![Ange hello hdbuserstore kommando](./media/hana-overview-high-availability-disaster-recovery/image4-hdbuserstore-command.png)

I följande exempel, där hello användare SCADMIN01 och hello värdnamnet är lhanad01 kan hello är hello-kommandot:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Hantera alla skript från en enskild server för skalbar HANA instanser. I det här exemplet måste hello SAP HANA nyckeln SCADMIN01 ändras för varje värd på ett sätt som visar hello-värd som är relaterade toohello nyckel. Det vill säga hello SAP HANA säkerhetskopiering konto ändras med hello instans antalet hello HANA DB **lhanad**. hello nyckel måste ha administrativ behörighet på hello-värd som den är tilldelad till och hello skalbar säkerhetskopiering användaren måste ha åtkomst rättigheter tooall SAP HANA instanser.
```
hdbuserstore set SCADMIN01 lhanad:30015 SCADMIN <password>
hdbuserstore set SCADMIN02 lhanad:30115 SCADMIN <password>
hdbuserstore set SCADMIN03 lhanad:30215 SCADMIN <password>
```

### <a name="step-6-copy-items-from-hello-scripts-folder"></a>Steg 6: Kopiera objekt från hello/skript-mappen

Kopiera hello följande objekt från hello/scripts mapp (ingår på hello guld avbildning av hello-installation) toohello arbetskatalog för **hdbsql**. Katalogen för den aktuella HANA installationer är /hana/shared/D01/exe/linuxx86\_64/hdb.
```
azure\_hana\_backup.pl
testHANAConnection.pl
testStorageSnapshotConnection.pl
removeTestStorageSnapshot.pl
HANABackupCustomerDetails.txt
```
Kopiera följande objekt om de kör skalbar hello eller OLAP:
```
azure\_hana\_backup\_bw.pl
testHANAConnectionBW.pl
testStorageSnapshotConnectionBW.pl
removeTestStorageSnapshotBW.pl
HANABackupCustomerDetailsBW.txt
```
Hej HANABackupCustomerDetails.txt filen kan ändras på följande sätt för distribution av skala upp. Det är hello kontroll- och fil för hello-skript som körs hello storage snapshots. Du bör ha fått hello _namn på säkerhetskopia_ och _lagring IP-adress_ från SAP HANA på Azure-tjänsthantering när dina instanser har distribuerats. Du kan inte ändra hello sekvens ordning eller avstånd på någon av hello variabler eller hello skript körs inte korrekt.

Hello konfigurationsfilen skulle se ut för en distribution av skala upp:
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Created by customer using hdbuserstore
HANA Backup Name: SCADMIND01
```
För en skalbar konfiguration ser hello HANABackupCustomerDetailsBW.txt filen ut som:
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Node IP addresses, instance numbers, and HANA backup name
#provided by customer.  HANA backup name created using
#hdbuserstore utility.
Node 1 IP Address: 10.254.15.21
Node 1 HANA instance number: 01
Node 1 HANA Backup Name: SCADMIN01
Node 2 IP Address: 10.254.15.22
Node 2 HANA instance number: 02
Node 2 HANA Backup Name: SCADMIN02
Node 3 IP Address: 10.254.15.23
Node 3 HANA instance number: 03
Node 3 HANA Backup Name: SCADMIN03
Node 4 IP Address: 10.254.15.24
Node 4 HANA instance number: 04
Node 4 HANA Backup Name: SCADMIN04
Node 5 IP Address: 10.254.15.25
Node 5 HANA instance number: 05
Node 5 HANA Backup Name: SCADMIN05
Node 6 IP Address: 10.254.15.26
Node 6 HANA instance number: 06
Node 6 HANA Backup Name: SCADMIN06
Node 7 IP Address: 10.254.15.27
Node 7 HANA instance number: 07
Node 7 HANA Backup Name: SCADMIN07
Node 8 IP Address: 10.254.15.28
Node 8 HANA instance number: 08
Node 8 HANA Backup Name: SCADMIN08
```
>[!NOTE]
>Endast nod 1 information används för närvarande i hello faktiska HANA lagring ögonblicksbild skript. Vi rekommenderar att du testar åtkomst tooor från alla HANA noder så att om hello säkerhetskopiering huvudnoden någonsin ändras du redan har kontrollerat att en annan nod kan ske dess genom att ändra hello information i noden 1.

toocheck för hello rätt konfigurationer i konfigurationsfilen för hello eller korrekt anslutning toohello HANA instanser köra något av hello följande skript:
- För en skala upp-konfiguration (oberoende på SAP arbetsbelastningen):

 ```
testHANAConnection.pl
```
- För en skalbar konfiguration:

 ```
testHANAConnectionBW.pl
```

Kontrollera hello master HANA instansen har tooall krävs HANA servrar. Det finns inga parametrar toohello skript, men du måste slutföra hello lämpliga HANABackupCustomerDetails / HANABackupCustomerDetailsBW file för hello skriptet toorun korrekt. Eftersom endast hello shell-kommando felkoder returneras, går det inte för hello skript tooerror kontrollera varje instans. Trots detta ger hello skriptet vissa kommentarer för du toodouble-kontrollen.

toorun hello skript:
```
 ./testHANAConnection.pl
```
 Om hello har hämtas hello status för hello HANA instans, visas ett meddelande om att hello HANA anslutning har upprättats.

Det finns också andra typen av skript kan du använda toocheck hello master HANA instans serverns möjlighet toosign i toohello lagring. Innan du kan köra hello azure\_hana\_säkerhetskopiering (\_bw) .pl skript, måste du köra hello nästa skript. Om en volym som innehåller inga ögonblicksbilder, är det omöjligt toodetermine om hello volym är helt enkelt tom eller om det är ett ssh fel tooobtain hello ögonblicksbild information. Därför körs hello skriptet två steg:

- Verifierar hello lagring konsolen är tillgänglig.
- En test- eller dummy, ögonblicksbild för varje volym skapas av HANA instans.

Därför ingår hello HANA instans som ett argument. Igen, det är inte möjligt tooprovide fel söker efter hello lagringsanslutning, men hello skript innehåller användbara tips om hello körningen misslyckas.

hello skript körs som:
```
 ./testStorageSnapshotConnection.pl <hana instance>
```
Eller den körs som:
```
./testStorageSnapshotConnectionBW.pl <hana instance>
```
hello-skriptet visar också ett meddelande om att du är kan toosign i korrekt tooyour distribuerat storage-klient som är organiserat efter hello logiska enhetsnummer (LUN) som används av hello server-instanser som du äger.

Innan du kör hello första lagring ögonblicksbild säkerhetskopiering, kör du hello nästa skript toomake till att hello-konfigurationen är korrekt.

När du har kört dessa skript kan du ta bort hello ögonblicksbilder genom att köra:
```
./removeTestStorageSnapshot.pl <hana instance>
```
Eller
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="step-7-perform-on-demand-snapshots"></a>Steg 7: Utföra ögonblicksbilder på begäran

Utföra ögonblicksbilder på begäran (samt schemalägga regelbundna ögonblicksbilder med hjälp av cron) som beskrivs här.

Kör hello följande skript för skala upp konfigurationer:
```
./azure_hana_backup.pl lhanad01 customer 20
```
Kör hello följande skript för skalbara konfigurationer:
```
./azure_hana_backup_bw.pl lhanad01 customer 20
```
hello skalbar skriptet gör vissa ytterligare kontrollerar toomake till att alla HANA servrar kan nås och alla HANA instanser returnerar rätt status för hello-instansen innan du fortsätter med att skapa ögonblicksbilder SAP HANA eller lagring.

hello följande argument är obligatoriska:

- hello HANA instans kräver säkerhetskopiering.
- hello ögonblicksbild prefix för hello lagring ögonblicksbild.
- hello antalet ögonblicksbilder toobe för hello specifikt prefix.

```
./azure_hana_backup.pl lhanad01 customer 20
```

hello köra hello skriptet skapar hello lagring ögonblicksbild i dessa tre skilda faser:

- Köra en HANA ögonblicksbild.
- Köra en ögonblicksbild för lagring.
- Ta bort hello HANA ögonblicksbild.

Kör hello skriptet genom att anropa från hello HDB körbara mapp som den kopierades till. Det säkerhetskopierar hello minst följande volymer, men den också säkerhetskopierar alla volymer som har hello explicit SAP HANA-instansnamn i hello volymnamn.
```
hana_data_<hana instance>_prod_t020_vol
hana_log_<hana instance>_prod_t020_vol
hana_log_backup_<hana instance>_prod_t020_vol
hana_shared_<hana instance>_prod_t020_vol
```
hello kvarhållningsperiod är strikt administreras med hello antal ögonblicksbilder som skickas som en parameter när du kör hello-skript (till exempel 20 såg tidigare). Så är hello tidsperiod en funktion av hello tidsperiod körning och hello antalet ögonblicksbilder i hello anrop av hello skript. Om hello antal ögonblicksbilder som hålls överskrider hello-nummer som namnges som en parameter i hello anrop av hello skript, hello äldsta lagring ögonblicksbild av den här etiketten (i vårt fall tidigare _anpassade_) tas bort innan en ny ögonblicksbild köra. Detta innebär hello nummer som hello sista parametern för anropet hello är hello tal som du kan använda toocontrol hello antalet ögonblicksbilder.

>[!NOTE]
>När du ändrar hello etikett startas hello räknat igen.

Du måste tooinclude hello HANA med instansnamn som tillhandahålls av SAP HANA på Azure-tjänsthantering som ett argument om de skapar ögonblicksbilder i miljöer med flera noder. Hello hello SAP HANA på enheten i Azure (stora instanser) är tillräckligt i nod-miljöer, men rekommenderas fortfarande hello HANA instansnamn.

Dessutom kan du kan säkerhetskopiera Start volumes\LUNs med hjälp av hello samma skript. Du måste säkerhetskopiera startvolymen minst en gång när du först kör HANA, även om vi rekommenderar ett veckoschema eller nattetid schema för säkerhetskopiering för att starta i cron. Snarare än att lägga till en SAP HANA-instansnamn, infoga _Start_ som hello argumentet i hello skript på följande sätt:
```
./azure_hana_backup boot customer 20
```
hello är samma bevarandeprincip tillhandahållet toohello startvolymen samt. Använda ögonblicksbilder på begäran, enligt beskrivningen tidigare för specialfall, som under en uppgradering av SAP förbättring av paketet (EHP) eller när du behöver toocreate en ögonblicksbild av olika.

Vi rekommenderar att du tooperform schemalagda lagring ögonblicksbilder med hjälp av cron och vi rekommenderar att du använder hello samma skript för alla säkerhetskopiering och katastrofåterställning behov (ändra hello skript indata toomatch hello olika begärt säkerhetskopieringstiden). Dessa är alla schemalagda annorlunda i cron beroende på deras körningstid: varje timme, 12 timmar, varje dag eller varje vecka. hello cron schema är utformad toocreate lagring ögonblicksbilder som matchar hello diskuterats tidigare kvarhållning etiketter för långsiktig extern säkerhetskopiering. hello skriptet innehåller kommandon tooback upp alla produktionsvolymer, beroende på deras begärda frekvens (data och loggfiler säkerhetskopieras varje timme, medan hello startvolymen säkerhetskopieras varje dag).

hello poster i hello följande cron skript körs varje timme vid hello tionde minut, var 12: e timme på hello tionde minut, och varje dag på hello tionde minut. hello cron jobb skapas så att endast en SAP HANA lagring ögonblicksbild äger rum under en viss timme så att hello per timme och dag inte säkerhetskopieras vid hello samma tid (12:10:00). toohelp optimera din ögonblicksbilder skapas och replikering, SAP HANA på Azure-tjänsthantering visar hello rekommenderas tills du toorun säkerhetskopiorna.

hello standard cron schemaläggning i /etc/crontab är följande:
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 14
```
I hello föregående cron anvisningarna få hello HANA volymer (utan startvolymen) en varje timme ögonblicksbild med hello etiketten. Dessa ögonblicksbilder bevaras 66. Dessutom bevaras 14 ögonblicksbilder med hello 12-timmarsperiod etiketten. Du hämta eventuellt timvis ögonblicksbilder för tre dagar, plus 12-timmarsperiod ögonblicksbilder för en annan fyra dagar, vilket ger dig en hela veckan ögonblicksbilder.

Schemalägga inom cron kan vara svårt, eftersom bara ett skript ska köras när som helst viss om hello skript ut av flera minuter. Om du vill daglig säkerhetskopiering för långsiktig kvarhållning kan en ögonblicksbild sparas tillsammans med en 12-timmarsperiod ögonblicksbild (med en kvarhållning antal sju varje) eller hello timvis ögonblicksbild är successiva tootake plats 10 minuter senare. Endast en ögonblicksbild sparas i hello produktionsvolym.
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 7
10 0 * * * ./azure_hana_backup.pl lhanad01 daily 7
```
hello frekvenser som listas här är bara exempel. tooderive din optimala antalet ögonblicksbilder, Använd hello följande kriterier:

- Krav i mål för återställning i tidpunkt.
- Användning av diskutrymme.
- Krav på att återställas peka mål och mål för eventuell återställning.
- Eventuell körningen av HANA fullständiga databassäkerhetskopieringar mot diskar. När en fullständig säkerhetskopiering av databasen mot diskar, eller _backint_ gränssnitt är utföra hello lagring ögonblicksbilder går inte att köra. Om du planerar tooexecute fullständiga databassäkerhetskopieringar ovanpå lagring ögonblicksbilder, kontrollera att hello körningen av lagring ögonblicksbilder är inaktiverad under den här tiden.

>[!IMPORTANT]
> hello användningen av lagringsutrymme ögonblicksbilder för SAP HANA säkerhetskopior är endast giltig när hello ögonblicksbilder utförs tillsammans med SAP HANA säkerhetskopior. Dessa loggar säkerhetskopieringar behöver toobe kan toocover hello tidsperioder mellan hello storage snapshots. Om du har angett ett åtagande toousers för en punkt-in-time-återställning på 30 dagar, behöver du hello följande:

- Möjlighet tooaccess en ögonblicksbild av lagring som är 30 dagar gamla.
- Sammanhängande loggsäkerhetskopior över hello senaste 30 dagarna.

Skapa en ögonblicksbild av hello säkerhetskopiering loggvolym samt i hello antal säkerhetskopior. Dock vara säker på att tooperform regelbundna säkerhetskopieringar så att du kan:

- Behöva hello sammanhängande loggsäkerhetskopior tooperform i tidpunkt återställningar.
- Förhindra att hello SAP HANA loggvolym slut på utrymme.

En av hello sista stegen är tooschedule SAP HANA säkerhetskopieringsloggar i SAP HANA Studio. hello SAP HANA säkerhetskopiering mål loggmålet är hello som skapats speciellt hana eller in\_säkerhetskopieringar volym med hello monteringspunkt för /hana/log/backups.

![Schemalägga SAP HANA säkerhetskopieringsloggar i SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Du kan välja säkerhetskopior som är oftare än var 15: e minut. Vissa användare även utföra säkerhetskopior av varje minut, men vi inte rekommenderar kommer _över_ 15 minuter.

hello sista steget är ett filbaserat tooperform säkerhetskopiering (efter hello installationen av SAP HANA) toocreate en enda säkerhetskopiering posten som måste finnas inom hello säkerhetskopieringskatalogen. Annars kan inte SAP HANA initiera de angivna säkerhetskopiorna.

![Skapa en filbaserad säkerhetskopiering toocreate en enda säkerhetskopiering transaktion](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)

### <a name="monitoring-hello-number-and-size-of-snapshots-on-hello-disk-volume"></a>Övervaka hello antal och storlek för ögonblicksbilder på diskvolymen hello

Du kan övervaka hello antalet ögonblicksbilder och förbrukning av hello lagring av ögonblicksbilder på en viss lagringsvolym. Hej `ls` kommandot visas inte hello ögonblicksbild katalogen eller filer. Dock hello Linux OS-kommandot `du` gör med hello följande kommandon:

- `du –sh .snapshot`innehåller alla ögonblicksbilder i hello ögonblicksbild katalog totalt.
- `du –sh --max-depth=1`Listar alla ögonblicksbilder som sparats hello .snapshot mapp och hello storleken på varje ögonblicksbild.
- `du –hc`ger hello totala storlek som används av alla ögonblicksbilder.

Använd dessa kommandon toomake till att hello ögonblicksbilder som tas och lagras inte som förbrukar all hello lagring på hello volymer.

### <a name="reducing-hello-number-of-snapshots-on-a-server"></a>Minska hello antalet ögonblicksbilder på en server

Du kan minska hello antal vissa etiketter ögonblicksbilder som du lagrar som beskrivits tidigare. hello sista två parametrar av hello kommandot tooinitiate en ögonblicksbild är en etikett och hello antal ögonblicksbilder som du vill tooretain.
```
./azure_hana_backup.pl lhanad01 customer 20
```
I föregående exempel hello hello ögonblicksbild etiketten är _kunden_ och hello antalet ögonblicksbilder med den här etiketten toobe behålls är _20_. Du kanske vill tooreduce hello antalet lagrade ögonblicksbilder som du svarar toodisk förbrukningen av diskutrymme. hello enkelt tooreduce hello antalet ögonblicksbilder är toorun hello skriptet med hello sista parametern set too5:
```
./azure_hana_backup.pl lhanad01 customer 5
```
På grund av pågående hello-skript med den här inställningen, hello antalet ögonblicksbilder, inklusive hello ny lagring som en ögonblicksbild, är _5_.

 >[!NOTE]
 > Det här skriptet minskar hello antalet ögonblicksbilder endast om hello senaste tidigare ögonblicksbild är mer än en timme gamla. hello skriptet tar inte bort ögonblicksbilderna som är mindre än en timme gamla.

Dessa begränsningar är relaterade toohello valfria katastrofåterställning funktionerna som erbjuds.

Om du inte längre vill toomaintain en uppsättning ögonblicksbilder med detta prefix kan du köra hello skriptet med _0_ som hello kvarhållning number tooremove alla ögonblicksbilder som matchar det prefixet. Ta bort alla ögonblicksbilder kan dock påverka hello funktionerna för katastrofåterställning.

### <a name="recovering-toohello-most-recent-hana-snapshot"></a>Återställer toohello senaste HANA ögonblicksbild

I hello händelse kan du uppleva en hello process för att återställa från en ögonblicksbild för lagring och produktion nedåt scenariot initieras som en kundincident med SAP HANA på Azure-tjänsthantering. Ett oväntat scenario kan vara en hög angelägenhetsgrad fråga om data har tagits bort i produktion system och hello endast sätt tooretrieve hello data är toorestore hello produktionsdatabas.

På hello däremot punkten i tidsåterställningen kan vara låg angelägenhetsgrad och planerade för dagar i förväg. Du kan planera återställningen med SAP HANA på Azure-tjänsthantering i stället för att ett problem med hög prioritet. Exempelvis kan du planera tootry en uppgradering av hello SAP-program genom att använda ett nytt paket för förbättring, och du sedan behöver toorevert tillbaka tooa ögonblicksbild som representerar hello tillstånd innan hello EHP uppgraderingen.

Innan du skickar hello begäran måste toodo vissa förberedelser. SAP HANA på Azure Service Management-teamet kan sedan hantera hello begäran och ange hello återställa volymer. Därefter, är det upp tooyou toorestore hello HANA-databas utifrån hello ögonblicksbilder. Här är hur tooprepare för hello begäran:

>[!NOTE]
>Användargränssnittet kan skilja sig från hello följande skärmdumpar, beroende på hello SAP HANA-versionen som du använder.

1. Bestäm vilken ögonblicksbild toorestore. Endast hello hana/datavolym skulle återställas om du inte uppmanas annars.

2. Stäng hello HANA instans.

 ![Stäng hello HANA instans](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Demontera hello datavolymer på varje nod för HANA-databas. hello återställningen av hello ögonblicksbild misslyckas om hello datavolymer inte är frånkopplade.

 ![Demontera hello datavolymer på varje nod för HANA-databas](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Öppna ett Azure-supporten begäran tooinstruct hello återställning av en specifik ögonblicksbild.

 - Vid återställning av hello: SAP HANA på Azure-tjänsthantering be tooattend en konferenssamtal tooensure som inga data är att gå vilse.

 - Efter återställningen hello: SAP HANA på Azure-tjänsthantering meddelar dig när hello lagring ögonblicksbild har återställts.

5. Montera alla datavolymer när hello återställningsprocessen har slutförts.

 ![Montera alla datavolymer](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. Välj återställningsalternativ SAP HANA Studio, om de inte automatiskt kommer när du återansluter tooHANA DB via SAP HANA Studio. hello visar följande exempel en återställning toohello senaste HANA ögonblicksbild. En ögonblicksbild av lagring bäddar in en HANA ögonblicksbild och om du återställer toohello senaste lagring ögonblicksbild ska hello senaste HANA ögonblicksbild. (Om du återställer tooolder lagring ögonblicksbilder, måste toolocate hello HANA ögonblicksbilden baserat på hello hello lagring ögonblicksbilder togs.)

 ![Välj alternativ för Återställ SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Välj **Återställ hello tooa specifika data säkerhetskopierings- eller databasögonblicksbilden**.

 ![Hej ”ange typ av återställning” fönster](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Välj **ange säkerhetskopiering utan katalog**.

 ![Hej ”ange Säkerhetskopians plats”-fönstret](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. I hello **måltypen** väljer **ögonblicksbild**.

 ![Hej ”ange hello säkerhetskopiering tooRecover” fönster](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Klicka på **Slutför** toostart hello återställningsprocessen.

 ![Klicka på ”Slutför” toostart hello-återställningen](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. hello HANA-databas har återställts och återställas toohello HANA ögonblicksbild som ingår i hello lagring ögonblicksbild.

 ![HANA-databas har återställts och återställda toohello HANA ögonblicksbild](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-toohello-most-recent-state"></a>Återställer toohello senaste status

hello återställer följande process hello HANA ögonblicksbild som ingår i hello lagring ögonblicksbild. Den sedan återställer hello loggen säkerhetskopieringar toohello senaste transaktionstillstånd av hello-databasen innan du återställer hello lagring ögonblicksbild.

>[!IMPORTANT]
>Innan du fortsätter, kontrollera att du har en fullständig och sammanhängande kedja av säkerhetskopieringar av transaktionsloggen. Du kan inte återställa hello aktuell status för hello databas utan dessa säkerhetskopior.

1. Slutför steg 1 – 6 för hello föregående procedur i ”återställa toohello senaste HANA ögonblicksbild”.

2. Välj **återställa hello tooits senaste databastillståndet**.

 ![Välj ”återställa hello tooits senaste databastillståndet”](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. Ange hello platsen för hello säkerhetskopior senaste HANA. hello placering måste toocontain alla HANA säkerhetskopieringar av transaktionsloggen från hello HANA ögonblicksbild toohello senaste tillstånd.

 ![Ange hello platsen för hello senaste HANA loggsäkerhetskopior](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. Välj en säkerhetskopia som utgångspunkt från vilken toorecover hello-databas. I vårt exempel är hello HANA ögonblicksbild som ingick i hello lagring ögonblicksbild. (Endast en ögonblicksbild finns i följande skärmbild hello).

 ![Välj en säkerhetskopia som utgångspunkt från vilken toorecover hello-databas](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. Rensa hello **Använd Delta säkerhetskopieringar** kryssrutan om går inte finns mellan hello hello HANA ögonblicksbilder och hello senaste tillstånd.

 ![Rensa hello ”Använd Delta säkerhetskopieringar” kryssrutan om det finns inga går](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. Klicka på Sammanfattning hello-skärmen **Slutför** toostart hello återställningen.

 ![Klicka på ”Slutför” på hello översiktsskärm](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-tooanother-point-in-time"></a>Återställer tooanother tidpunkt
toorecover tooa tidpunkt mellan hello HANA ögonblicksbild (ingår i hello lagring ögonblicksbild) och som är senare än hello HANA ögonblicksbild point-in-time-återställning, hello följande:

1. Kontrollera att du har alla säkerhetskopieringar av transaktionsloggen från hello HANA ögonblicksbild toohello tid toorecover.
2. Börja hello under ”toohello senaste återställningsläge”.
3. I steg 2 i hello proceduren i hello **ange återställningstyp** väljer **Återställ hello databasen toohello följande punkten i**anger hello punkt i tiden och slutföra steg 3-6.

## <a name="monitoring-hello-execution-of-snapshots"></a>Övervaka hello körningen av ögonblicksbilder

Du måste toomonitor hello körningen av lagring ögonblicksbilder. hello-skript som kör en ögonblicksbild av lagring skriver tooa utdatafilen och sparar sedan toohello samma plats som hello Perl-skript. En separat fil skrivs för varje ögonblicksbild. hello utdata från varje fil visar hello olika faser som hello ögonblicksbild skriptet körs:

- Hitta hello volymer som behöver toocreate en ögonblicksbild
- Hitta hello-ögonblicksbilder tas från dessa volymer
- Ta bort eventuell befintlig ögonblicksbilder toomatch hello antal ögonblicksbilder som du har angett
- Skapa en ögonblicksbild av HANA
- Att skapa en ögonblicksbild för lagring av hello över hello volymer
- Du tar bort hello HANA ögonblicksbild
- Byta namn på hello senaste ögonblicksbilden för**.0**

hello viktigaste delen av hello skript är:
```
**********************Creating HANA snapshot**********************
Creating hello HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting hello HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
Från det här exemplet kan du se hur hello skript poster hello skapandet av hello HANA ögonblicksbild. Hello skalbar om initieras den här processen på hello huvudnod. hello huvudnoden initierar hello synkron hello att ögonblicksbilder skapas på varje hello arbetsnoderna. Sedan tas hello lagring ögonblicksbilden. Efter hello har körts hello lagring ögonblicksbilder, tas hello HANA ögonblicksbild bort.
