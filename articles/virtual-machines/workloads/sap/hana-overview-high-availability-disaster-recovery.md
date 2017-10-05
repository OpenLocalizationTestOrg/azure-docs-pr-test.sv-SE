---
title: "Hög tillgänglighet och katastrofåterställning återställning av SAP HANA i Azure (stora instanser) | Microsoft Docs"
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
ms.openlocfilehash: f95e944fc3ec3a831d97386443eb644420ae54dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>SAP HANA (stora instanser) hög tillgänglighet och katastrofåterställning recovery på Azure 

Hög tillgänglighet och katastrofåterställning är viktiga aspekter av din verksamhetskritiska SAP HANA i Azure (stora instanser) servrar. Det är viktigt att arbeta med SAP, din systemintegreraren eller Microsoft skapa och implementera rätt hög-tillgänglighet/katastrofåterställning strategin korrekt. Det är också viktigt att beakta återställningspunktmål och mål, som är specifika för din miljö.

## <a name="high-availability"></a>Hög tillgänglighet

Microsoft stöder SAP HANA hög tillgänglighet metoder ”out-of-rutan” bland annat:

- **Storage-replikering:** storage-system kan replikera alla data till en annan plats (inom, eller separat från samma datacenter). SAP HANA fungerar oberoende av den här metoden.
- **HANA system replikering:** replikering av alla data i SAP HANA till ett separat system för SAP HANA. Mål minimeras via datareplikering med jämna mellanrum. SAP HANA stöder asynkrona, synkron i minnet och synkront läge (rekommenderas endast för SAP HANA-system som är i samma datacenter eller mindre än 100 KM från varandra). I den aktuella designen av HANA stora instans stämplar kan HANA system replikering användas för hög tillgänglighet.
- **Värd för automatisk redundans:** en lokal fault-lösning ska användas som ett alternativ till att systemet replikering. När huvudnoden blir otillgänglig, en eller flera vänteläge SAP HANA-noder som är konfigurerade i skalbar läge och SAP HANA växlar automatiskt över till en annan nod.

Mer information om hög tillgänglighet för SAP HANA finns i följande SAP-information:

- [Vitboken för SAP HANA hög tillgänglighet](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [SAP HANA Administrationsguide](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP Academy videon på SAP HANA System-replikering](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP stöd Obs #1999880 – vanliga frågor och svar på SAP HANA System-replikering](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- [SAP stöd Obs #2165547 – SAP HANA säkerhetskopiering och återställning i SAP HANA-systemmiljön replikering](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [SAP stöd Obs #1984882 – med SAP HANA System replikering för maskinvara Exchange med minsta/noll avbrottstid](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="disaster-recovery"></a>Haveriberedskap

SAP HANA i Azure (stora instanser) erbjuds i två Azure-regioner i en geopolitiska region. Är en direkt nätverksanslutning för att replikera data vid katastrofåterställning mellan de två stora instans stämplarna för två olika regioner. Replikering av data är lagringsinfrastruktur baserat. Replikeringen görs inte som standard. Den är klar för kundkonfigurationer som sorterade disaster recovery. I den aktuella designen kan inte HANA system replikering användas för katastrofåterställning.

Men om du vill dra nytta av katastrofåterställning, måste du starta att utforma nätverksanslutningen till de två olika Azure-regionerna. Om du vill göra det, behöver du en Azure ExpressRoute-krets anslutning från lokala i dina viktigaste Azure-region och krets anslutning från lokal till din region för katastrofåterställning. Det här måttet bör ge en situation där en fullständig Azure-region, inklusive en Microsoft enterprise edge router (MSEE) plats, har ett problem.

Du kan ansluta alla virtuella Azure-nätverk som ansluter till SAP HANA i Azure (stora instanser) i någon av regionerna till båda dessa ExpressRoute-kretsar som en andra åtgärd. Det här måttet adresser fall där endast en av platserna som MSEE som ansluter din lokala plats med Azure kan lämna avgift.

Följande bild visar den bästa konfigurationen för katastrofåterställning:

![Bästa konfigurationen för katastrofåterställning](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)

Optimal fallet för en katastrofåterställning konfiguration av nätverket är att ha två ExpressRoute-kretsar från lokal till de två olika Azure-regionerna. En krets går till region #1, kör en instans för produktion. Andra ExpressRoute-kretsen går till region #2, kör vissa icke-produktion HANA instanser. (Detta är viktigt om en hel Azure-region, inklusive MSEE och stora instans stämpel stängs av rutnät.)

Som en andra åtgärd är olika virtuella nätverk anslutna till de olika ExpressRoute-kretsar som är anslutna till SAP HANA i Azure (stora instanser). Du kan kringgå den plats där en MSEE misslyckas eller så kan du sänka återställningspunktmål för katastrofåterställning, diskussion senare.

Nästa kraven för en katastrofåterställning installation är:

- Du måste beställa SAP HANA på Azure (stora instanser) SKU: er av samma storlek som din produktion SKU: er och distribuerar dem i området för katastrofåterställning. Dessa instanser kan användas för att köra testet, sandbox eller QA HANA instanser.
- En profil för katastrofåterställning måste du sortera för varje din SAP HANA på Azure (stora instanser) SKU: er som du vill återställa i återställningsplats, om det behövs. Den här åtgärden leder till tilldelning av lagringsvolymer som är mål för storage-replikering från din produktionsregion i området för katastrofåterställning.

Efter att du uppfyller ovanstående krav är det ditt ansvar att starta storage-replikering. Basen för storage-replikering är i lagringsinfrastruktur som används för SAP HANA i Azure (stora instanser), lagring ögonblicksbilder. Du måste utföra för att starta replikering för katastrofåterställning:

- En ögonblicksbild av startenheter LUN, enligt beskrivningen ovan.
- En ögonblicksbild av dina HANA-relaterade volymer, som tidigare beskrivits.

När du kör dessa ögonblicksbilder, dirigeras en första replik av volymer på de volymer som är associerade med din profil för katastrofåterställning i området för katastrofåterställning.

Därefter används ögonblicksbilden av senaste lagring för varje timme att replikera går utvecklar på lagringsvolymer.

Det återställningspunktmål som uppnås med den här konfigurationen är från 60 till 90 minuter. För att uppnå ett bättre återställningspunktmål i fallet med katastrofåterställning, kopierar du säkerhetskopieringarna av transaktionsloggen HANA från SAP HANA i Azure (stora instanser) till andra Azure-regionen. För att uppnå detta mål för återställningspunkt, gör du följande:

1. Logga in så ofta som möjligt /hana/log/backup säkerhetskopiera HANA transaktionen.
2. Kopiera säkerhetskopieringarna av transaktionsloggen när de är klara att en virtuell Azure virtuell dator (VM) som är i ett virtuellt nätverk som är ansluten till SAP HANA på Azure (stora instanser)-server.
3. Kopiera säkerhetskopian från den virtuella datorn till en virtuell dator som är i ett virtuellt nätverk i området för katastrofåterställning.
4. Behåll transaktionen säkerhetskopior i den regionen i den virtuella datorn.

Vid katastrofåterställning, när katastrofåterställning-profil har distribuerats på en faktiska server, kopiera säkerhetskopieringarna av transaktionsloggen från den virtuella datorn till SAP HANA i Azure (stora instanser) som nu är den primära servern i området för katastrofåterställning och återställa dessa säkerhetskopior. Återställningen är möjligt eftersom tillståndet för HANA på katastrofåterställning diskarna som en HANA ögonblicksbild. Detta är den offset för ytterligare återställningar av säkerhetskopieringar av transaktionsloggen.

## <a name="backup-and-restore"></a>Säkerhetskopiering och återställning

En av de viktigaste aspekterna att operativsystemet databaser Kontrollera att databasen skyddas från olika katastrofer. Dessa händelser kan orsakas av något från naturkatastrof enkel fel.

Säkerhetskopiera en databas med möjlighet att återställa den till en valfri tidpunkt (exempelvis innan någon bort kritiska data), kan återställningen till ett tillstånd som är så nära som möjligt så som den var innan avbrott uppstod.

Två typer av säkerhetskopior måste utföras för bästa resultat:

- Säkerhetskopior av databasen
- Säkerhetskopieringar av transaktionsloggen

Fullständig säkerhetskopiering utförs på en på programnivå, får du även mer omfattande genom att utföra säkerhetskopieringar med lagring ögonblicksbilder. Utför säkerhetskopior är också viktigt för att återställa databasen (och för att tömma loggar från redan bekräftats transaktioner).

SAP HANA i Azure (stora instanser) erbjuder två alternativ för säkerhetskopiering och återställning:

- Göra det själv (DIY). När du beräkna om du vill se till att tillräckligt mycket ledigt diskutrymme kan du utföra fullständiga säkerhetskopieringar för databasen och loggfilerna genom att använda metoder för disk-säkerhetskopiering (för att dessa diskar). Över tiden, säkerhetskopiorna kopieras till ett Azure storage-konto (när du har skapat ett Azure-baserad filserver med praktiskt taget obegränsade storage) eller Använd ett Azure Backup-valv eller Azure kall storage. Ett annat alternativ är att använda en tredje parts data protection verktyg, till exempel Commvault, där säkerhetskopiorna lagras när de har kopierats till ett lagringskonto. Alternativet själv säkerhetskopiering kan också krävas för data som behöver lagras under längre perioder för kompatibilitet och granskning.
- Säkerhetskopiera och återställa funktioner som innehåller den underliggande infrastrukturen för SAP HANA i Azure (stora instanser). Det här alternativet uppfyller behovet av säkerhetskopieringar och det gör manuella säkerhetskopieringar nästan föråldrade (utom där säkerhetskopiering av data krävs för efterlevnad). Resten av det här avsnittet adresser säkerhetskopiering och återställning av funktionerna som erbjuds med HANA (stora instanser).

> [!NOTE]
> Snapshot-teknik som används av den underliggande infrastrukturen HANA (stora instanser) har ett beroende på SAP HANA ögonblicksbilder. SAP HANA ögonblicksbilder fungerar inte tillsammans med SAP HANA Multitenant databasen behållare. Denna metod för säkerhetskopiering kan därför användas för att distribuera SAP HANA Multitenant databasen behållare.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Med hjälp av ögonblicksbilder för lagring av SAP HANA i Azure (stora instanser)

Lagringsinfrastruktur som underliggande SAP HANA i Azure (stora instanser) stöder begreppet en ögonblicksbild för lagring av volymer. Både säkerhetskopiering och återställning av en viss volym stöds, med följande:

- I stället för säkerhetskopiering av databaser vidtas ögonblicksbilder av lagring regelbundet.
- Lagring ögonblicksbilden initierar en SAP HANA-ögonblicksbild innan ögonblicksbilden lagring körs. SAP HANA ögonblicksbilden är installationen avseende eventuell loggen återställningar efter återställningen av ögonblicksbilden lagring.
- Vid den punkt där lagring ögonblicksbilden har körts, tas SAP HANA-ögonblicksbild bort.
- Säkerhetskopior av tas ofta och lagras i loggvolym för säkerhetskopiering eller i Azure.
- Om databasen måste återställas till en viss punkt i tiden, skickas en begäran till Microsoft Azure-supporten (produktion avbrott) eller SAP HANA på Azure-tjänsthantering att återställa till en viss lagring ögonblicksbild (till exempel en planerad återställning av ett system med begränsat till det ursprungliga tillståndet).
- SAP HANA-ögonblicksbilden som ingår i ögonblicksbilden lagring är en offset punkt för att tillämpa säkerhetskopior som har körts och lagrade när lagring ögonblicksbilden togs.
- Dessa säkerhetskopior kommer att återställa databasen till en viss punkt i tiden.

Anger säkerhetskopian\_namn skapas en ögonblicksbild av följande volymer:

- Hana-data
- Hana/logg
- Hana eller in\_säkerhetskopiering (monterade som säkerhetskopia hana/loggfiler)
- Hana/delat

### <a name="storage-snapshot-considerations"></a>Överväganden för ögonblicksbild av lagring

>[!NOTE]
>Lagring ögonblicksbilderna _inte_ tillhandahålls utan kostnad, eftersom ytterligare lagringsutrymme måste allokeras.

Specifika säkerhetsnivån lagring ögonblicksbilder för SAP HANA i Azure (stora instanser) inkluderar:

- En ögonblicksbild av en specifik lagring (vid tidpunkten när den tas) använder mycket lite lagringsutrymme.
- Ögonblicksbilden behöver lagra det ursprungliga innehållet i block som dataändringar innehåll och innehållet i filer som ändras på lagringsvolymen SAP HANA-data.
- Lagring ögonblicksbilden ökar i storlek. Ju längre ögonblicksbilden finns, desto större blir lagring ögonblicksbilden.
- Flera ändringar som gjorts till SAP HANA-databas volym under livslängden för lagring ögonblicksbild, desto större blir förbrukningen av diskutrymme för lagring ögonblicksbild.

SAP HANA i Azure (stora instanser) levereras med fast Volymstorlekar för SAP HANA data och loggfilen volymen. Utför ögonblicksbilder av volymerna eats i utrymmet på volymen så att det är ditt ansvar att schemalägga ögonblicksbilder av lagring (inom SAP HANA på Azure [stora instanser] process).

Följande avsnitt innehåller information för att utföra dessa ögonblicksbilder, inklusive allmänna rekommendationer:

- Även om maskinvaran kan klara 255 ögonblicksbilder per volym, rekommenderar vi att du hålla betydligt lägre än detta antal.
- Innan du utför lagring av ögonblicksbilder, övervaka och hålla reda på ledigt utrymme.
- Minska antalet lagring ögonblicksbilderna baserat på ledigt utrymme. Du kan behöva minska antalet ögonblicksbilder som du behåller eller du kan behöva utöka volymer. (Du kan ordna ytterligare lagringsutrymme i enheter som 1 TB.)
- Vi rekommenderar att du inte utför alla ögonblicksbilder för lagring under aktiviteter, till exempel data flyttas till SAP HANA med migrering Systemverktyg (R3load eller genom att återställa databaser för SAP HANA från säkerhetskopior). (Om en system-migrering görs på ett nytt SAP HANA-system, lagring ögonblicksbilder inte behöver utföras.)
- Under större omorganisering av SAP HANA tabeller bör lagring ögonblicksbilder undvikas om möjligt.
- Lagring ögonblicksbilder är en förutsättning för att engagera er katastrofåterställning funktionerna för SAP HANA i Azure (stora instanser).

### <a name="setting-up-storage-snapshots"></a>Konfigurera lagring ögonblicksbilder

1. Kontrollera att Perl är installerat i Linux-operativsystem på servern HANA (stora instanser).
2. Ändra/etc/ssh/ssh\_config att lägga till raden _Macintosh hmac-sha1_.
3. Skapa en SAP HANA säkerhetskopiering användarkonto på huvudnoden för varje SAP HANA-instans som du kör (om tillämpligt).
4. SAP HANA HDB klienten måste installeras på alla servrar för SAP HANA (stora instanser).
5. På den första servern SAP HANA (stora instanser) för varje region, måste du skapa en offentlig nyckel för att komma åt det underliggande lagringsinfrastruktur som styr ögonblicksbilder skapas.
6. Kopiera skriptet azure\_hana\_backup.pl från/skript till platsen för **hdbsql** för SAP HANA-installationen.
7. Kopiera filen HANABackupDetails.txt från/skript till samma plats som Perl-skript.
8. Ändra filen HANABackupDetails.txt efter behov för önskad kund-specifikationer.

### <a name="step-1-install-sap-hana-hdbclient"></a>Steg 1: Installera SAP HANA HDBClient

Linux installerad på SAP HANA i Azure (stora instanser) innehåller mappar och skript som krävs för att köra ögonblicksbilder för SAP HANA-lagring för säkerhetskopiering och katastrofåterställning. Dock är det ditt ansvar att installera SAP HANA HDBclient när du installerar SAP HANA. (Microsoft installerar varken HDBclient eller SAP HANA.)

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

På den första SAP HANA på Azure (stora instanser)-server i varje Azure-region, skapar du en offentlig nyckel som används för att komma åt lagringsinfrastrukturen så att du kan skapa ögonblicksbilder. Den offentliga nyckeln garanterar att ett lösenord inte krävs att logga in på lagring och inte bevaras lösenordsinformation. Kör följande kommando för att generera den offentliga nyckeln i Linux på servern för SAP HANA (stora instanser):
```
  ssh-keygen –t dsa –b 1024
```
Den nya platsen är _/root/.ssh/id\_dsa.pub. Ange inte en verklig lösenfras, eller så uppmanas du att ange lösenfrasen varje gång du loggar in. Utan på **RETUR** två gånger för att ta bort Ange lösenfras kravet för att logga in.

Kontrollera att den offentliga nyckeln korrigerades som förväntat genom att ändra mappar till /root/.ssh/ och sedan köra den **ls** kommando. Om nyckeln finns kan du kopiera den genom att köra följande kommando:

![Offentliga nyckeln kopieras genom att köra det här kommandot](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Nu kontakta SAP HANA på Azure-tjänsthantering och ange nyckeln. I representant använder den offentliga nyckeln för att registrera det i den underliggande lagringsinfrastrukturen.

### <a name="step-4-create-an-sap-hana-user-account"></a>Steg 4: Skapa ett användarkonto för SAP HANA

Skapa ett användarkonto för SAP HANA SAP HANA Studio för säkerhetskopiering. Det här kontot måste ha följande behörigheter: _säkerhetskopiering Admin_ och _katalog viktigt_. I det här exemplet användarnamnet SCADMIN skapas.

![Skapa en användare i HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-the-sap-hana-user-account"></a>Steg 5: Ge SAP HANA-användarkonto

Auktorisera SAP HANA-användarkonto (som ska användas av skripten utan tillstånd varje gång skriptet körs). Kommandot SAP HANA `hdbuserstore` kan skapas en SAP HANA användarnyckel, vilken lagras på en eller flera SAP HANA-noder. Nyckeln för användaren kan användare komma åt SAP HANA utan att behöva hantera lösenord från inom scripting processen som beskrivs senare.

>[!IMPORTANT]
>Kör följande kommando som `_root_`. Annars fungerar skriptet inte korrekt.

Ange den `hdbuserstore` kommandot på följande sätt:

![Ange kommandot hdbuserstore](./media/hana-overview-high-availability-disaster-recovery/image4-hdbuserstore-command.png)

I följande exempel, där användaren är SCADMIN01 och värdnamnet är lhanad01, är kommandot:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Hantera alla skript från en enskild server för skalbar HANA instanser. I det här exemplet måste nyckeln för SAP HANA SCADMIN01 ändras för varje värd på ett sätt som visar värden som är relaterade till nyckeln. Det vill säga SAP HANA säkerhetskopiering kontot ändras med instans antalet HANA-databas **lhanad**. Nyckeln måste ha administratörsbehörighet på den värd som har tilldelats och skalbar säkerhetskopiering användaren måste ha åtkomsträttigheter till alla instanser för SAP HANA.
```
hdbuserstore set SCADMIN01 lhanad:30015 SCADMIN <password>
hdbuserstore set SCADMIN02 lhanad:30115 SCADMIN <password>
hdbuserstore set SCADMIN03 lhanad:30215 SCADMIN <password>
```

### <a name="step-6-copy-items-from-the-scripts-folder"></a>Steg 6: Kopiera objekt från mappen/skript

Kopiera följande objekt från mappen/skript (som finns på guld avbildningen av installationen) till arbetskatalog för **hdbsql**. Katalogen för den aktuella HANA installationer är /hana/shared/D01/exe/linuxx86\_64/hdb.
```
azure\_hana\_backup.pl
testHANAConnection.pl
testStorageSnapshotConnection.pl
removeTestStorageSnapshot.pl
HANABackupCustomerDetails.txt
```
Kopiera följande objekt om de använder skalbar eller OLAP:
```
azure\_hana\_backup\_bw.pl
testHANAConnectionBW.pl
testStorageSnapshotConnectionBW.pl
removeTestStorageSnapshotBW.pl
HANABackupCustomerDetailsBW.txt
```
Filen HANABackupCustomerDetails.txt kan ändras på följande sätt för distribution av skala upp. Det är filen kontroll och konfiguration för det skript som körs storage snapshots. Du bör ha fått den _namn på säkerhetskopia_ och _lagring IP-adress_ från SAP HANA på Azure-tjänsthantering när dina instanser har distribuerats. Du kan inte ändra sekvensen beställning eller avstånd för någon av variablerna eller skriptet körs inte korrekt.

Konfigurationsfilen ser ut som en skala upp distribution:
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Created by customer using hdbuserstore
HANA Backup Name: SCADMIND01
```
Filen HANABackupCustomerDetailsBW.txt ser ut som för en skalbar-konfiguration:
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
>Endast nod 1 information används för närvarande i skriptet faktiska HANA för lagring ögonblicksbild. Vi rekommenderar att du testar åtkomst till eller från alla HANA noder så att om det ändras någonsin huvudnoden säkerhetskopiering kan du redan har kontrollerat att en annan nod kan ske dess genom att ändra informationen i nod 1.

Om du vill söka efter rätt konfigurationerna i konfigurationsfilen eller korrekt anslutning till HANA instanser genom att köra något av följande skript:
- För en skala upp-konfiguration (oberoende på SAP arbetsbelastningen):

 ```
testHANAConnection.pl
```
- För en skalbar konfiguration:

 ```
testHANAConnectionBW.pl
```

Kontrollera att den master HANA-instansen har åtkomst till alla nödvändiga HANA-servrar. Det finns inga parametrar för skriptet, men du måste slutföra lämpliga HANABackupCustomerDetails / HANABackupCustomerDetailsBW filen för att skriptet ska köras. Eftersom endast shell-kommando felkoder returneras, går det inte att kontrollera varje instans fel skript. Skriptet ger även så vissa kommentarer att kontrollera.

Att köra skriptet:
```
 ./testHANAConnection.pl
```
 Om skriptet erhåller har status för HANA instansen, visas ett meddelande som att HANA-anslutning har upprättats.

Dessutom är en andra typen av skript som du kan använda för att kontrollera master HANA instans serverns möjlighet att logga in på lagringsplatsen. Innan du kan köra azure\_hana\_säkerhetskopiering (\_bw) .pl skript, måste du köra skriptet nästa. Om en volym innehåller inga ögonblicksbilder, det går inte att avgöra om volymen är helt enkelt tom eller om det finns en ssh gick inte att hämta information för ögonblicksbilder. Skriptet körs därför två steg:

- Verifierar att konsolen lagring är tillgänglig.
- En test- eller dummy, ögonblicksbild för varje volym skapas av HANA instans.

Därför ingår HANA-instans som ett argument. Igen, det går inte att felsökning för anslutning till lagringsplatsen, men skriptet innehåller användbara tips om körningen misslyckas.

Skriptet körs som:
```
 ./testStorageSnapshotConnection.pl <hana instance>
```
Eller den körs som:
```
./testStorageSnapshotConnectionBW.pl <hana instance>
```
Skriptet visar även ett meddelande om att du ska kunna logga in på rätt sätt på din distribuerade lagring-klient är organiserat efter logiska enhetsnummer (LUN) som används av server-instanser som du äger.

Innan du kör de första lagring ögonblicksbild säkerhetskopieringarna, kör nästa skript och kontrollera att konfigurationen är korrekt.

När du har kört dessa skript kan du ta bort ögonblicksbilderna genom att köra:
```
./removeTestStorageSnapshot.pl <hana instance>
```
Eller
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="step-7-perform-on-demand-snapshots"></a>Steg 7: Utföra ögonblicksbilder på begäran

Utföra ögonblicksbilder på begäran (samt schemalägga regelbundna ögonblicksbilder med hjälp av cron) som beskrivs här.

Skala upp konfigurationer, kör du följande skript:
```
./azure_hana_backup.pl lhanad01 customer 20
```
Kör följande skript för skalbara konfigurationer:
```
./azure_hana_backup_bw.pl lhanad01 customer 20
```
Skalbar skriptet gör vissa ytterligare kontroller för att se till att alla HANA servrar kan nås och alla instanser av HANA returnera rätt status för instansen innan du fortsätter med att skapa ögonblicksbilder SAP HANA eller lagring.

Följande argument är obligatoriska:

- HANA instans kräver säkerhetskopieringen.
- Snapshot-prefix för lagring ögonblicksbilden.
- Antalet ögonblicksbilder som ska lagras för specifika prefix.

```
./azure_hana_backup.pl lhanad01 customer 20
```

Körningen av skriptet skapar ögonblicksbilden lagring i dessa tre skilda faser:

- Köra en HANA ögonblicksbild.
- Köra en ögonblicksbild för lagring.
- Ta bort ögonblicksbilden HANA.

Kör skript genom att anropa från HDB körbara mappen som den kopierades till. Säkerhetskopierar minst följande volymer, men den också säkerhetskopierar alla volymer som har explicit SAP HANA-instansnamnet i namn på volymer.
```
hana_data_<hana instance>_prod_t020_vol
hana_log_<hana instance>_prod_t020_vol
hana_log_backup_<hana instance>_prod_t020_vol
hana_shared_<hana instance>_prod_t020_vol
```
Kvarhållningsperioden administreras strikt, med antalet ögonblicksbilder som skickas som en parameter när du kör skriptet (till exempel 20 såg tidigare). Hur lång tid är därför en funktion av körningen och antalet ögonblicksbilder i anropet i skriptet. Om antalet ögonblicksbilder som hålls överskrider det antal som namnges som en parameter i anropet i skriptet, äldsta lagring ögonblicksbilden av den här etiketten (i vårt fall tidigare _anpassade_) tas bort innan en ny ögonblicksbild körs. Detta innebär att antalet som den sista parametern för anropet är det tal du kan använda för att styra antalet ögonblicksbilder.

>[!NOTE]
>När du ändrar etiketten startas inventeringen igen.

Du måste inkludera HANA instansnamnet som tillhandahålls av SAP HANA på Azure-tjänsthantering som ett argument om de skapar ögonblicksbilder i miljöer med flera noder. Namnet på SAP HANA på enheten i Azure (stora instanser) är tillräcklig i miljöer med en nod, men rekommenderas fortfarande instansnamnet HANA.

Du kan dessutom säkerhetskopiera Start volumes\LUNs genom att använda samma skript. Du måste säkerhetskopiera startvolymen minst en gång när du först kör HANA, även om vi rekommenderar ett veckoschema eller nattetid schema för säkerhetskopiering för att starta i cron. Snarare än att lägga till en SAP HANA-instansnamn, infoga _Start_ som argument till skriptet på följande sätt:
```
./azure_hana_backup boot customer 20
```
Samma bevarandeprincip är tillhandahållet startvolymen samt. Använda ögonblicksbilder på begäran, enligt beskrivningen tidigare för specialfall, som under en uppgradering av SAP förbättring av paketet (EHP) eller när du behöver skapa en ögonblicksbild av olika.

Vi rekommenderar att du kan utföra schemalagd lagring ögonblicksbilder med hjälp av cron och vi rekommenderar att du använder samma skript för alla säkerhetskopiering och katastrofåterställning behov (ändra skriptet indata som matchar de olika begärt säkerhetskopieringstiden). Dessa är alla schemalagda annorlunda i cron beroende på deras körningstid: varje timme, 12 timmar, varje dag eller varje vecka. Cron-schemat är utformade för att skapa ögonblicksbilder av lagring som matchar tidigare vi nämnt kvarhållning etiketter för långsiktig extern säkerhetskopiering. Skriptet innehåller kommandon för att säkerhetskopiera alla produktionsvolymer, beroende på deras begärda frekvens (data och loggfiler säkerhetskopieras varje timme, medan startvolymen säkerhetskopieras varje dag).

Posterna i följande skript för cron körs varje timme vid tionde minut, var 12: e timme på tionde minut, och varje dag på tionde minut. Cron jobb skapas så att endast en SAP HANA lagring ögonblicksbild äger rum under en viss timme så att varje timme och dagliga säkerhetskopieringar inte görs på samma gång (12:10:00). För att optimera din ögonblicksbilder skapas och replikering, ges SAP HANA på Azure-tjänsthantering rekommenderade tid du köra dina säkerhetskopieringar.

Standard-cron schemaläggning i /etc/crontab är följande:
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 14
```
I de föregående anvisningarna i cron HANA volymer (utan startvolymen) för att hämta en varje timme ögonblicksbild med etiketten. Dessa ögonblicksbilder bevaras 66. Dessutom bevaras 14 ögonblicksbilder med etiketten 12-timmarsperiod. Du hämta eventuellt timvis ögonblicksbilder för tre dagar, plus 12-timmarsperiod ögonblicksbilder för en annan fyra dagar, vilket ger dig en hela veckan ögonblicksbilder.

Schemalägga inom cron kan vara svårt, eftersom bara ett skript ska köras när som helst viss om skripten ut av flera minuter. Om du vill daglig säkerhetskopiering för långsiktig kvarhållning kan en ögonblicksbild sparas tillsammans med en 12-timmarsperiod ögonblicksbild (med en kvarhållning antal sju varje) eller timvis ögonblicksbilden sprids ska utföras senare 10 minuter. Endast en ögonblicksbild sparas i produktionsvolymen.
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 7
10 0 * * * ./azure_hana_backup.pl lhanad01 daily 7
```
Frekvenserna i den här listan är bara exempel. Använd följande kriterier för att visa din optimala antalet ögonblicksbilder:

- Krav i mål för återställning i tidpunkt.
- Användning av diskutrymme.
- Krav på att återställas peka mål och mål för eventuell återställning.
- Eventuell körningen av HANA fullständiga databassäkerhetskopieringar mot diskar. När en fullständig säkerhetskopiering av databasen mot diskar, eller _backint_ gränssnitt är utföra körningen av lagring ögonblicksbilder misslyckas. Om du planerar att köra fullständiga databassäkerhetskopieringar ovanpå lagring ögonblicksbilder, kontrollera att har körningen av lagring ögonblicksbilder inaktiverats under denna tid.

>[!IMPORTANT]
> Användningen av lagringsutrymme ögonblicksbilder för SAP HANA säkerhetskopior är giltigt endast när ögonblicksbilder utförs tillsammans med säkerhetskopior av SAP HANA. Dessa säkerhetskopior måste kunna omfatta tidsperioder mellan storage snapshots. Om du har angett ett åtagande för användare av en punkt-in-time-återställning av 30 dagar, behöver du följande:

- Möjlighet att komma åt en ögonblicksbild av lagring som är 30 dagar gamla.
- Sammanhängande loggsäkerhetskopior under de senaste 30 dagarna.

Skapa en ögonblicksbild av volymens loggavsnittet inom intervallet för säkerhetskopior. Se dock till att utföra regelbundna säkerhetskopieringar så att du kan:

- Ha sammanhängande loggsäkerhetskopior som behövs för att utföra i tidpunkt återställningar.
- Förhindra att loggvolymen SAP HANA utrymmet tar slut.

Ett av de sista stegen är att schemalägga SAP HANA säkerhetskopieringsloggar i SAP HANA Studio. Målområdet för SAP HANA loggavsnittet är särskilt skapade hana eller in\_säkerhetskopieringar volym med monteringspunkt för /hana/log/backups.

![Schemalägga SAP HANA säkerhetskopieringsloggar i SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Du kan välja säkerhetskopior som är oftare än var 15: e minut. Vissa användare även utföra säkerhetskopior av varje minut, men vi inte rekommenderar kommer _över_ 15 minuter.

Det sista steget är att säkerhetskopiera filbaserade (efter den första installationen av SAP HANA) om du vill skapa en enkel säkerhetskopiering posten som måste finnas i katalogen för säkerhetskopieringen. Annars kan inte SAP HANA initiera de angivna säkerhetskopiorna.

![Säkerhetskopiera filbaserade att skapa en enda post för säkerhetskopiering](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)

### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Övervakning av antal och storlek ögonblicksbilder på volymen

Du kan övervaka antalet ögonblicksbilder och lagringförbrukningen av ögonblicksbilder på en viss lagringsvolym. Den `ls` kommandot visas inte ögonblicksbild katalogen eller filer. Dock Linux OS-kommandot `du` matchar, med följande kommandon:

- `du –sh .snapshot`innehåller alla ögonblicksbilder i katalogen ögonblicksbild totalt.
- `du –sh --max-depth=1`Listar alla ögonblicksbilder som sparas i mappen .snapshot och storleken på varje ögonblicksbild.
- `du –hc`innehåller den totala storleken som används av alla ögonblicksbilder.

Använd dessa kommandon för att se till att ögonblicksbilder som tas och lagras inte som förbrukar all lagring på volymer.

### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Minska antalet ögonblicksbilder på en server

Du kan minska antalet vissa etiketter ögonblicksbilder som du lagrar som beskrivits tidigare. De två sista parametrarna för kommandot för att initiera en ögonblicksbild är en etikett och antalet ögonblicksbilder som du vill behålla.
```
./azure_hana_backup.pl lhanad01 customer 20
```
I det förra exemplet ögonblicksbild etiketten är _kunden_ och antalet ögonblicksbilder med den här etiketten ska behållas är _20_. När du har besvarat förbrukningen av diskutrymme kan du vill minska antalet lagrade ögonblicksbilder. Enkelt sätt att minska antalet ögonblicksbilder är att köra skriptet med den sista parametern inställd på 5:
```
./azure_hana_backup.pl lhanad01 customer 5
```
Skriptet som körs med den här inställningen antalet ögonblicksbilder, inklusive den nya lagring ögonblicksbilden därför _5_.

 >[!NOTE]
 > Det här skriptet minskar antalet ögonblicksbilder endast om den tidigare ögonblicksbilden av den senaste är mer än en timme gamla. Skriptet tar inte bort ögonblicksbilderna som är mindre än en timme gamla.

Dessa begränsningar är relaterade till de valfria katastrofåterställning funktionerna som erbjuds.

Om du inte längre vill hantera en uppsättning ögonblicksbilder med detta prefix kan du köra skriptet med _0_ kvarhållning antalet att ta bort alla ögonblicksbilder som matchar det prefixet som. Ta bort alla ögonblicksbilder kan dock påverka funktionerna för katastrofåterställning.

### <a name="recovering-to-the-most-recent-hana-snapshot"></a>Återställa till den senaste HANA ögonblicksbilden

I händelse av att det uppstår ett scenario för produktion och ned kan processen för att återställa från en ögonblicksbild av lagring initieras som en kundincident med SAP HANA på Azure-tjänsthantering. Ett oväntat scenario kan vara en hög angelägenhetsgrad fråga om data har tagits bort i en produktionssystem och det enda sättet att hämta data är att återställa produktionsdatabasen.

Å andra sidan punkten i tidsåterställningen kan vara låg angelägenhetsgrad och planerade för dagar i förväg. Du kan planera återställningen med SAP HANA på Azure-tjänsthantering i stället för att ett problem med hög prioritet. Du kan till exempel planera försök en uppgradering av SAP-program genom att använda ett nytt paket för förbättring, och du sedan behöver återgå till en ögonblicksbild som representerar tillstånd innan uppgraderingen EHP.

Innan du skickar en begäran måste du göra vissa förberedelser. SAP HANA på Azure Service Management-teamet kan sedan hantera begäran och ge de återställda volymerna. Därefter kan är det du återställa HANA-databas utifrån ögonblicksbilderna. Här är hur du förbereder för begäran:

>[!NOTE]
>Användargränssnittet kan skilja sig från följande skärmbilderna, beroende på SAP HANA-versionen som du använder.

1. Bestäm vilken ögonblicksbild för att återställa. Endast hana/datavolym skulle återställas om du inte uppmanas annars.

2. Stäng HANA-instans.

 ![Stäng HANA-instans](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Avmontera datavolymerna på varje nod för HANA-databas. Återställningen av ögonblicksbilden misslyckas om datavolymer inte är frånkopplade.

 ![Avmontera datavolymerna på varje nod för HANA-databas](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Öppna en Azure-supportbegäran att instruera återställning av en specifik ögonblicksbild.

 - Under återställningen: SAP HANA på Azure-tjänsthantering kanske du ombeds att delta i ett konferenssamtal så att inga data är att gå vilse.

 - Efter återställningen: SAP HANA på Azure-tjänsthantering meddelar dig när lagring ögonblicksbild har återställts.

5. Montera alla datavolymer när återställningsprocessen har slutförts.

 ![Montera alla datavolymer](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. Välj återställningsalternativ SAP HANA Studio, om de inte automatiskt kommer när du återansluter till HANA DB via SAP HANA Studio. I följande exempel visas en återställning till den senaste HANA ögonblicksbilden. En ögonblicksbild av lagring bäddar in en HANA ögonblicksbild och om du vill återställa till den senaste ögonblicksbilden av lagring, ska den senaste HANA ögonblicksbilden. (Om du återställer till äldre lagring ögonblicksbilder, du måste leta upp HANA ögonblicksbilden baserat på tiden lagring ögonblicksbilden togs.)

 ![Välj alternativ för Återställ SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Välj **återställa databasen till en specifik säkerhetskopierings- eller ögonblicksbild**.

 ![Fönstret ”Ange typ av återställning”](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Välj **ange säkerhetskopiering utan katalog**.

 ![”Ange Säkerhetskopians plats”-fönstret](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. I den **måltypen** väljer **ögonblicksbild**.

 ![Fönstret ”Ange det säkerhetskopia att återställa”](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Klicka på **Slutför** att starta återställningsprocessen.

 ![Klicka på ”Slutför” om du vill starta återställningen](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. HANA-databas är återställas och återställas till HANA ögonblicksbild som ingår i lagring ögonblicksbilden.

 ![HANA-databas är återställas och återställas till HANA ögonblicksbild](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-to-the-most-recent-state"></a>Återställa till det senaste tillståndet

Följande process återställer HANA ögonblicksbild som ingår i lagring ögonblicksbilden. Den sedan återställer säkerhetskopieringarna av transaktionsloggen senaste tillståndet för databasen innan du återställer lagring ögonblicksbilden.

>[!IMPORTANT]
>Innan du fortsätter, kontrollera att du har en fullständig och sammanhängande kedja av säkerhetskopieringar av transaktionsloggen. Utan dessa säkerhetskopior kan du återställa det aktuella tillståndet för databasen.

1. Slutför steg 1 – 6 i föregående procedur i ”återställning till senaste HANA ögonblicksbild”.

2. Välj **återställa databasen till det senaste tillståndet**.

 ![Välj ”återställa databasen till det senaste tillståndet”](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. Ange platsen för de senaste HANA säkerhetskopiorna. Platsen måste innehålla alla HANA säkerhetskopieringar av transaktionsloggen från HANA ögonblicksbild till det senaste tillståndet.

 ![Ange platsen för de senaste HANA säkerhetskopiorna](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. Välj en säkerhetskopia som en bas som du vill återställa databasen. I vårt exempel är HANA ögonblicksbild som ingick i lagring ögonblicksbilden. (Endast en ögonblicksbild finns i följande skärmbild).

 ![Välj en säkerhetskopia som en bas som du vill återställa databasen](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. Avmarkera den **Använd Delta säkerhetskopieringar** kryssrutan om går inte finns mellan HANA ögonblicksbilder och det senaste tillståndet.

 ![Avmarkera kryssrutan ”Använd Delta säkerhetskopieringar” om det finns inga går](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. Klicka på fönstret Sammanfattning **Slutför** att starta återställningen.

 ![Klicka på ”Slutför” på fönstret Sammanfattning](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-to-another-point-in-time"></a>Återställa till en annan punkt i tiden
Om du vill återställa till en punkt i tiden mellan HANA ögonblicksbilden (ingår i ögonblicksbilden av lagring) och ett som är senare än HANA ögonblicksbild point-in-time-återställning, gör du följande:

1. Kontrollera att du har alla säkerhetskopieringar av transaktionsloggen från HANA ögonblicksbild till den tid som du vill återställa.
2. Börja under ”återställning till det senaste tillståndet”.
3. I steg 2 i proceduren i det **ange återställningstyp** väljer **återställa databasen till följande punkt i tiden**anger punkten i tiden och utför steg 3-6.

## <a name="monitoring-the-execution-of-snapshots"></a>Övervakning av körningen av ögonblicksbilder

Du behöver övervaka körning av lagring ögonblicksbilder. Skriptet som kör en ögonblicksbild av lagring skriver utdata till en fil och sparar den på samma plats som Perl-skript. En separat fil skrivs för varje ögonblicksbild. Utdata från varje fil visar de olika faserna att ögonblicksbild skriptet körs:

- Hitta de volymer som behöver skapa en ögonblicksbild
- Hitta ögonblicksbilder tas från dessa volymer
- Ta bort eventuell befintlig ögonblicksbilder för att matcha antalet ögonblicksbilder som du har angett
- Skapa en ögonblicksbild av HANA
- Skapa ögonblicksbilden lagring över volymer
- När du raderar ögonblicksbilden HANA
- Byta namn på den senaste ögonblicksbilden på **.0**

Den viktigaste delen av skriptet är:
```
**********************Creating HANA snapshot**********************
Creating the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
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
Deleting the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
Från det här exemplet kan du se hur skriptet poster skapandet av HANA ögonblicksbilden. I fallet med skalbara måste initieras den här processen på huvudnoden. Huvudnoden initierar synkron genereringen av ögonblicksbilder på varje worker-nod. Sedan tas lagring ögonblicksbilden. När du har körts lagring ögonblicksbilder HANA ögonblicksbilden har tagits bort.
