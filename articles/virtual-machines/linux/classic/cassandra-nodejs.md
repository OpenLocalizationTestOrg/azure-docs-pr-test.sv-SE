---
title: aaaRun Cassandra med Linux i Azure | Microsoft Docs
description: "Hur toorun en Cassandra kluster på Linux i Azure Virtual Machines från en Node.js-app"
services: virtual-machines-linux
documentationcenter: nodejs
author: tomarcher
manager: routlaw
editor: 
tags: azure-service-management
ms.assetid: 30de1f29-e97d-492f-ae34-41ec83488de0
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 381ca301bbe88d3740cf182f9c44fada5b9ba7cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="running-cassandra-with-linux-on-azure-and-accessing-it-from-nodejs"></a>Kör Cassandra med Linux på Azure och få åtkomst till det från Node.js
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Se Resource Manager-mallar för [Datastax Enterprise](https://azure.microsoft.com/documentation/templates/datastax) och [Väck klustret och Cassandra på CentOS](https://azure.microsoft.com/documentation/templates/spark-and-cassandra-on-centos/).

## <a name="overview"></a>Översikt
Microsoft Azure är en öppen molnplattform som körs både Microsoft som bra som icke-Microsoft-programvara som innehåller operativsystem, programservrar, meddelanden mellanprogram såväl som SQL- och NoSQL-databaser från båda modellerna kommersiella och Öppna källa. Bygga flexibel tjänster på offentliga moln, inklusive Azure kräver noggrann planering och arkitektur för avsiktlig för båda programservrarna som korrekt lagring lager. Cassandras distribuerade lagringsarkitektur naturligt hjälper till att skapa hög tillgänglighet system som är feltolerant för klustret. Cassandra är molnskala NoSQL-databas som underhålls av Apache Software Foundation på cassandra.apache.org; Cassandra är skriven i Java och därför körs på både på Windows och Linux plattformar.

hello fokuserar den här artikeln på tooshow Cassandra distributionen på Ubuntu som ett enstaka och flera Datacenter-kluster som utnyttjar Microsoft Azure virtuella datorer och virtuella nätverk. hello Klusterdistribution för produktionsarbetsbelastningar optimerade ligger utanför omfånget för den här artikeln eftersom den kräver flera disk nodkonfiguration, lämpliga ring topologi design- och datamodeller toosupport hello behövs replikering, datakonsekvens, genomflöde och krav för hög tillgänglighet.

Den här artikeln tar en grundläggande tillvägagångssätt tooshow vad ingår i byggnaden hello Cassandra klustret jämfört med Docker, Chef eller Puppet, vilket kan göra hello distribution av infrastruktur mycket enklare.  

## <a name="hello-deployment-models"></a>Hej distributionsmodeller
Microsoft Azure-nätverk kan hello distribution isolerat privat kluster, hello åtkomst som kan vara begränsad tooattain bra metataggkontroll nätverkssäkerhet.  Eftersom den här artikeln om hur du visar hello Cassandra distribution på en grundläggande nivå kan fokuserar vi inte på hello konsekvensnivå och hello optimala lagringsdesignen för genomströmning. Här är hello lista över nätverkskrav för vår hypotetiska klustret:

* Externa system kan inte komma åt Cassandra databasen från inom eller utanför Azure
* Cassandra klustret har toobe bakom en belastningsutjämnare för thrift-trafik
* Distribuera Cassandra noder i två grupper i varje Datacenter för en förbättrad klustret
* Låsa hello klustret så som endast programmet servergruppen har åtkomst toohello databasen direkt
* Inga offentliga nätverk slutpunkter än SSH
* Varje Cassandra nod måste en fast interna IP-adress

Cassandra kan vara distribuerade tooa enda Azure region eller toomultiple regioner baserat på hello distribuerade uppbyggnad hello arbetsbelastning. Distributionsmodell för flera regioner kan vara balanserad tooserve slutanvändare närmare tooa viss geografisk plats via hello samma Cassandra infrastruktur. Cassandras inbyggda nod replikering tar hand om hello synkronisering av flera master skriver härstammar från flera datacenter och anger en konsekvent överblick över hello data tooapplications. Distribution av flera regioner kan också med hello minska riskerna för hello bredare avbrott i Azure-tjänsten. Cassandras justerbara konsekvens och replikeringstopologin hjälper att uppfylla olika Återställningspunktmål behov av program.

### <a name="single-region-deployment"></a>Distribution av enskild Region
Vi börjar med en enskild region-distribution och skörd hello learnings att skapa en modell för flera regioner. Azure virtuella nätverk kommer att använda toocreate isolerade undernät så att hello nätverkssäkerhetskrav som nämns ovan kan uppfyllas.  hello processen som beskrivs i avsnittet Skapa hello enskild region distributionen använder Ubuntu 14.04 LTS och Cassandra 2.08; hello process kan dock lätt att antagna toohello andra varianter av Linux. hello nedan följer några av hello systemfel egenskaper hello enskild region distribution.  

**Hög tillgänglighet:** hello Cassandra noder som visas i bild 1 distribueras hello tootwo tillgänglighetsuppsättningar så att hello noder sprids mellan flera feldomäner för hög tillgänglighet. Virtuella datorer med varje tillgänglighetsuppsättning är mappade too2 feldomäner.  Microsoft Azure använder hello begreppet fel domän toomanage oplanerade stillestånd (t.ex. maskinvara eller programvara misslyckanden) under hello begreppet uppgraderingsdomänen (t.ex. värd eller OS-korrigering/uppgraderingar, programuppgraderingar) används för att hantera schemalagda stillestånd. Se [katastrofåterställning och hög tillgänglighet för Azure-program](http://msdn.microsoft.com/library/dn251004.aspx) för hello roll för fel- och domäner för att uppnå hög tillgänglighet.

![Distribution av enskild region](./media/cassandra-nodejs/cassandra-linux1.png)

Figur 1: Distribution av enskild region

Observera att när hello detta skrivs, Azure inte tillåter hello explicit mappningen för en grupp med virtuella datorer tooa specifika feldomän; även om hello distributionsmodell som illustreras i bild 1, är det därför statistiskt troligt att alla hello virtuella datorer kan vara mappade tootwo feldomäner i stället för fyra.

**Belastningsutjämna belastningsutjämning Thrift trafik:** Thrift klientbibliotek inuti hello webbservern ansluta toohello klustret via en intern belastningsutjämnare. Detta kräver hello processen att lägga till data ”hello interna belastningsutjämnare toohello-undernät (se bild 1) i hello kontexten för hello Molntjänsten värd hello Cassandra klustret. När hello intern belastningsutjämnare har definierats, kräver varje nod hello belastningsutjämnade endpoint toobe lagts till med hello anteckningar av en belastningsutjämnad uppsättning med tidigare definierade belastningsutjämnarens namn. Se [Azure intern belastningsutjämning ](../../../load-balancer/load-balancer-internal-overview.md)för mer information.

**Klustret frö:** är det viktigt tooselect hello de flesta noder för hög tillgänglighet för frö som hello nya noder kommunicerar med seed noder toodiscover hello topologi hello klustret. En nod från varje tillgänglighetsuppsättning utses seed noder tooavoid enskild felpunkt.

**Replikering faktor och konsekvensnivå:** Cassandras inbyggda hög tillgänglighet och data hållbarhet kännetecknas av hello replikering faktor (RF - antal kopior av varje rad som lagras på hello kluster) och konsekvensnivå (antal repliker toobe läsa/skriva innan det returneras hello resultatet toohello anroparen). Replikering faktor anges under hello KEYSPACE (liknande tooa relationsdatabas) skapa medan hello konsekvensnivå anges vid utfärdande hello CRUD-fråga. Se Cassandra dokumentation på [konfigurera konsekvent](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) konsekvenskontroll information och hello formel för beräkning av kvorum.

Cassandra stöder två typer av integritet datamodeller – konsekvens och slutliga konsekvensen; hello replikering faktor och konsekvensnivå avgör tillsammans om hello data blir konsekvent som en skrivåtgärd är klar eller att det blir överensstämmelse. Till exempel resulterar ange KVORUM som hello konsekvensnivå kommer alltid garanterar data konsekvenskontroll när alla konsekvensnivå nedan hello antal repliker toobe skrivs som behövs tooattain KVORUM (t.ex. en) i data som överensstämmelse.

hello 8 kluster visas ovan, med en faktor för replikering på 3 och KVORUM (2 noder läsas eller skrivas konsekvent) läsning och skrivning konsekvensnivå, kan överleva hello teoretisk förlust av på de flesta 1 nod per replikering gruppen innan programmet hello hello Granska resultatet hello-fel. Detta förutsätter att alla hello viktiga blanksteg väl har balanserad förfrågningar om läsning och skrivning.  hello följande är hello parametrar som vi ska använda för hello distribuerade klustret:

Enskild region Cassandra klusterkonfiguration:

| Kluster-Parameter | Värde | Kommentarer |
| --- | --- | --- |
| Antalet noder (N) |8 |Totalt antal noder i klustret hello |
| Replikering faktor (RF) |3 |Antal repliker på en viss rad |
| Konsekvensnivå (skriva) |QUORUM[(RF/2) +1) = 2] hello resultatet av hello formeln avrundas nedåt |Vid hello skriver de flesta 2 repliker innan hello svar skickas toohello anroparens 3 replik är skriven i ett överensstämmelse sätt. |
| Konsekvensnivå (läsa) |KVORUM [(RF/2) + 1 = 2] hello resultatet av hello formel avrundas nedåt |Läser 2 repliker innan du skickar svar toohello anroparen. |
| Replikeringsstrategi |NetworkTopologyStrategy Se [datareplikering](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) i Cassandra dokumentationen för mer information |Förstår hello topologi för distribution och placerar repliker på noder så att alla hello repliker inte slutar på hello samma rack |
| Snitch |GossipingPropertyFileSnitch Se [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) i Cassandra dokumentationen för mer information |NetworkTopologyStrategy använder ett koncept för snitch toounderstand hello-topologi. GossipingPropertyFileSnitch ger bättre kontroll i varje nod toodata center och rack. hello klustret använder sedan ett toopropagate denna information. Detta är mycket enklare i dynamisk IP-inställningen relativa tooPropertyFileSnitch |

**Azure överväganden för Cassandra kluster:** kapaciteten för Microsoft Azure-datorer använder Azure Blob storage för disk; Azure Storage sparar 3 repliker för varje disk för hög hållbarhet. Det innebär att varje rad med data som infogas i en tabell med Cassandra lagras redan i 3 repliker och därför datakonsekvens är redan tagit hand om även om hello replikering faktor (RF) är 1. hello huvudsakliga problem med replikering faktor 1 är att programmet hello får driftstopp, även om en enskild nod Cassandra misslyckas. Om en nod avser hello problem (t.ex. maskinvara, programvara systemfel) identifieras av Azure-Infrastrukturkontrollanten kan det dock etablerar en ny nod i dess ställe med hjälp av hello samma lagringsenheter. Etablera en ny nod tooreplace hello gamla en kan ta några minuter.  På samma sätt för planerat underhållsaktiviteter som gästoperativsystemet ändringar Cassandra uppgraderar och ändringar i programmet Azure-Infrastrukturkontrollanten utför rullande uppgraderingar av hello noder i klustret hello.  Även rullande uppgraderingar kan ta bort några noder samtidigt och därför hello klustret kan ha kort driftstopp för några partitioner. Dock går hello data inte förlorade på grund av toohello inbyggd redundans för Azure Storage.  

För system distribueras tooAzure som inte kräver hög tillgänglighet (t.ex. runt 99,9 som är likvärdiga too8.76 timmar per år, se [hög tillgänglighet](http://en.wikipedia.org/wiki/High_availability) information) du kan toorun med RF = 1 och konsekvensnivå = en.  För program med hög tillgänglighet, RF = 3 och konsekvensnivå = KVORUM tolererar hello stillestånd på en av hello noder en hello repliker. RF = 1 i traditionella distributioner (t.ex. på plats) kan inte användas på grund av förlust av data toohello uppstår problem angående diskfel.   

## <a name="multi-region-deployment"></a>Distribution av flera regioner
Cassandra's data-center-medveten replikering och konsekvent modell ovan hjälper dig med hello flera regioner distribution Out of box hello utan hello måste för några externa verktygsuppsättning. Detta är detsamma hello traditionella relationsdatabaser där hello konfigurerad för databasspegling för multimaster skrivningar kan vara ganska komplicerat. Cassandra i en flera region konfigurera kan hjälpa dig med hello Användningsscenarier inklusive hello följande:

**Närhet baserat distribution:** program med flera klienter, med Rensa koppling av användare för klient-till-region, kan vara fått av hello flera regioner klustret låg latens. Till exempel en learning management system för utbildningssyfte kan distribuera ett kluster som är distribuerade i östra USA och västra USA regioner tooserve hello respektive universitet för transaktionell samt analytics. hello data kan vara lokalt konsekvent vid hello tid läsningar och skrivningar och kan vara överensstämmelse över både hello regioner. Det finns andra exempel som Mediedistribution, e-handel och något och allt som fungerar geo koncentrerad användaren grundläggande är ett bra användningsfall för denna distributionsmodell.

**Hög tillgänglighet:** redundans är en nyckelfaktor för att uppnå hög tillgänglighet av programvara och maskinvara, se Skapa tillförlitliga Molnsystem på Microsoft Azure för ytterligare information. På Microsoft Azure är hello endast tillförlitligt sätt att uppnå true redundans genom att distribuera ett kluster med flera regioner. Program kan distribueras i en aktiv-aktiv eller aktivt-passivt läge och om någon av hello regioner är nere kan Azure Traffic Manager kan omdirigera trafik toohello aktiva regionen.  Med hello enskild region distribution om hello tillgänglighet är 99,9, en två-region-distribution kan uppnå tillgänglighet 99.9999 beräknas genom hello formel: (1-(1-0.999) * (1-0,999)) * 100); Se hello ovan papper för information.

**Katastrofåterställning:** flera regioner Cassandra klustret om korrekt utformad klarar oåterkalleligt data center avbrott. Om en region är nere kan starta hello programmet distribuerades tooother regioner betjänar hello slutanvändare. Precis som alla andra business continuity implementeringar har hello programmet toobe feltoleranta för dataförluster uppstår till följd av hello data i hello asynkron pipeline. Cassandra är dock hello recovery mycket snabbare än hello tidsåtgång av traditionella processer. Bild 2 visar hello vanliga flera regioner distributionsmodell med åtta noder i varje region. Båda regioner är speglade bilder från varandra för hello samma symmetriplan; Verkligt Designer är beroende av hello belastningstyp (t.ex. transaktionell eller analytiska), Återställningspunktsmål, RTO, datakonsekvens och tillgänglighet.

![Distribution av flera region](./media/cassandra-nodejs/cassandra-linux2.png)

Figur 2: Distribution av flera regioner Cassandra

### <a name="network-integration"></a>Integrering av nätverk
Uppsättningar av virtuella datorer distribuerade tooprivate nätverk finns på områdena kommunicerar med varandra med hjälp av en VPN-tunnel. hello VPN-tunnel som ansluter två programvara gateways etablerats under distributionsprocessen för hello nätverk. Båda områden har liknande nätverksarkitektur vad gäller ”web” och ”data” undernät; Azure-nätverk kan hello skapandet av så många undernät efter behov och tillämpa ACL: er som krävs av nätverkssäkerhet. När du utformar hello klustertopologi bland data center kommunikation latens och hello ekonomiska effekten av hello nätverket trafik måste toobe anses vara.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Datakonsekvens för distribution av flera Datacenter
Distribuerad distributioner behöver toobe medveten om hello klustret topologi inverkan på genomflöde och hög tillgänglighet. hello RF och konsekvensnivå måste toobe valt på sätt som hello kvorum beroende inte av hello tillgängligheten för alla hello-datacenter.
För ett system som kräver hög konsekvens, en LOCAL_QUORUM för konsekvensnivå (för läsning och skrivning) ser till att den hello lokala läser och skriver uppfylls från hello lokala replikeras asynkront noder när data är toohello remote datacenter.  Tabell 2 sammanfattar hello konfiguration information för hello flera regioner kluster beskrivs senare i hello Skriv upp.

**Två region Cassandra klusterkonfigurationen**

| Kluster-Parameter | Värde | Kommentarer |
| --- | --- | --- |
| Antalet noder (N) |8 + 8 |Totalt antal noder i klustret hello |
| Replikering faktor (RF) |3 |Antal repliker på en viss rad |
| Konsekvensnivå (skriva) |LOCAL_QUORUM [(sum(RF)/2) +1) = 4] hello resultatet av hello formel avrundas nedåt |2 noder skrivs toohello första Datacenter synkront; hello ytterligare 2 noder krävs för kvorum skrivs asynkront toohello 2 datacenter. |
| Konsekvensnivå (läsa) |LOCAL_QUORUM ((RF/2) + 1) = 2 hello resultatet av hello formel avrundas nedåt |Läs begäranden uppfylls från en enda region. 2 noder är skrivskyddade innan hello svar skickas tillbaka toohello klienten. |
| Replikeringsstrategi |NetworkTopologyStrategy Se [datareplikering](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) i Cassandra dokumentationen för mer information |Förstår hello topologi för distribution och placerar repliker på noder så att alla hello repliker inte slutar på hello samma rack |
| Snitch |GossipingPropertyFileSnitch Se [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) i Cassandra dokumentationen för mer information |NetworkTopologyStrategy använder ett koncept för snitch toounderstand hello-topologi. GossipingPropertyFileSnitch ger bättre kontroll i varje nod toodata center och rack. hello klustret använder sedan ett toopropagate denna information. Detta är mycket enklare i dynamisk IP-inställningen relativa tooPropertyFileSnitch |

## <a name="hello-software-configuration"></a>hello konfiguration av programvara
hello följande programvaruversioner som används under distributionen av hello:

<table>
<tr><th>Programvara</th><th>Källa</th><th>Version</th></tr>
<tr><td>JRE    </td><td>[JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA    </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu    </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 LTS</td></tr>
</table>

Hämtning av JRE kräver manuell godkännande av Oracle-licens, toosimplify hello distribution, ladda ned alla hello nödvändig programvara toohello desktop för senare överföring till hello Ubuntu mallavbildningen som vi ska skapa som ett ozonbildande toohello kluster distribution.

Hämta hello ovan programvara till en välkänd download katalog (t.ex. %TEMP%/downloads i Windows eller ~/Downloads på de flesta distributioner av Linux- eller Mac) på hello lokal dator.

### <a name="create-ubuntu-vm"></a>SKAPA VIRTUELL UBUNTU-DATOR
I det här steget i processen för hello skapar vi Ubuntu avbildningen med hello programvara som krävs så att hello avbildningen kan återanvändas för att etablera flera Cassandra noder.  

#### <a name="step-1-generate-ssh-key-pair"></a>STEG 1: Skapa SSH-nyckel
Azure behöver X509 offentlig nyckel som är PEM eller DER-kodade på hello etablering tid. Skapa en offentlig/privat nyckel använder hello instruktioner på hur tooUse SSH med Linux på Azure. Om du planerar toouse putty.exe som en SSH-klienten på Windows eller Linux måste du ha tooconvert hello PEM-kodade RSA privata nyckel tooPPK format med hjälp av puttygen.exe; hello instruktioner för detta finns i hello ovan webbsida.

#### <a name="step-2-create-ubuntu-template-vm"></a>STEG 2: Skapa Ubuntu VM-mallen
toocreate hello mallen VM, logga in på hello Azure klassiska portal och Använd hello följande sekvens: Klicka på ny, beräkning, VIRTUELLA, från GALLERIET, UBUNTU, Ubuntu Server 14.04 LTS och klicka på högerpilen för hello. En genomgång som beskriver hur toocreate en Linux VM, se Skapa en virtuell dator kör Linux.

Ange följande information på skärmen hello ”konfiguration av virtuell dator” #1 hello:

<table>
<tr><th>FÄLTNAMN              </td><td>       FÄLTVÄRDE               </td><td>         KOMMENTARER                </td><tr>
<tr><td>VERSION UTGIVNINGSDATUM    </td><td> Välj ett datum hello listrutan</td><td></td><tr>
<tr><td>NAMN PÅ VIRTUELL DATOR    </td><td> fråga mall                   </td><td> Detta är hello värdnamnet för hello VM </td><tr>
<tr><td>NIVÅ                     </td><td> STANDARD                           </td><td> Lämna hello standard              </td><tr>
<tr><td>STORLEK                     </td><td> A1                              </td><td>Välj hello VM baserat på hello i/o-behov. lämna hello standard för detta ändamål </td><tr>
<tr><td> NYTT ANVÄNDARNAMN             </td><td> localadmin                       </td><td> ”admin” är ett reserverat användarnamn i Ubuntu 12. xx och efter</td><tr>
<tr><td> AUTENTISERING         </td><td> Markera kryssrutan                 </td><td>Markera om du vill ha toosecure med en SSH-nyckel </td><tr>
<tr><td> CERTIFIKAT             </td><td> filnamnet för hello certifikat med offentlig nyckel </td><td> Använd hello offentliga nyckel som har skapats tidigare</td><tr>
<tr><td> Nytt lösenord    </td><td> starkt lösenord </td><td> </td><tr>
<tr><td> Bekräfta lösenord    </td><td> starkt lösenord </td><td></td><tr>
</table>

Ange följande information på skärmen hello ”konfiguration av virtuell dator” #2 hello:

<table>
<tr><th>FÄLTNAMN             </th><th> FÄLTVÄRDE                       </th><th> KOMMENTARER                                 </th></tr>
<tr><td> MOLNTJÄNSTEN    </td><td> Skapa en ny molntjänst    </td><td>Molntjänst är en behållare beräkningsresurser som virtuella datorer</td></tr>
<tr><td> DNS-MOLNTJÄNSTNAMNET    </td><td>Ubuntu template.cloudapp.net    </td><td>Ge en datorn storleksoberoende belastningsutjämnarens namn</td></tr>
<tr><td> REGION/TILLHÖRIGHETSGRUPP/VIRTUELLT NÄTVERK </td><td>    Västra USA    </td><td> Välj en region som ditt webbprogram åtkomst hello Cassandra klustret</td></tr>
<tr><td>STORAGE-KONTO </td><td>    Använd standard    </td><td>Använda hello standardkontot för lagring eller ett befintligt lagringskonto i en viss region</td></tr>
<tr><td>TILLGÄNGLIGHETSUPPSÄTTNING </td><td>    Ingen </td><td>    Lämna det tomt</td></tr>
<tr><td>SLUTPUNKTER    </td><td>Använd standard </td><td>    Använd hello standard SSH-konfigurationen </td></tr>
</table>

Klicka på högerpilen och lämna hello standardvärden på hello-skärmen #3 hello ”Kontrollera” knappen toocomplete hello VM etableringsprocessen. Hello VM med hello namnet ”ubuntu-mall” ska vara ”körs” status efter några minuter.

### <a name="install-hello-necessary-software"></a>INSTALLERA hello programvara som krävs
#### <a name="step-1-upload-tarballs"></a>STEG 1: Ladda upp tarballs
Använd scp eller pscp och kopiera hello tidigare hämtade programvara för ~ / hämtningar directory använder hello följande kommandoformatet:

##### <a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server-jre-8u5-linux-x64.tar.gzlocaladmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz
Upprepa hello ovanstående kommando för JRE samt som hello Cassandra bitar.

#### <a name="step-2-prepare-hello-directory-structure-and-extract-hello-archives"></a>STEG 2: Förbered hello katalogstrukturen och extrahera hello-Arkiv
Logga in på hello VM och skapa hello katalogstrukturen och extrahera programvara som en superanvändare använder hello bash-skriptet nedan:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change hello ownership toohello service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile tooadd JRE toohello PATH"
    echo "installation is complete"


Om du klistrar in det här skriptet i vim fönstret ser du att tooremove hello transport returnerade ('\r ”) med hjälp av hello följande kommando:

    tr -d '\r' <infile.sh >outfile.sh

#### <a name="step-3-edit-etcprofile"></a>Steg 3: Redigera etc-profil
Lägg till följande hello slutet hello:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

#### <a name="step-4-install-jna-for-production-systems"></a>Steg 4: Installera JNA för produktionssystem
Använd hello följande kommando sekvens: hello följande kommando installera jna-3.2.7.jar och jna-plattformen-3.2.7.jar too/usr/share.java directory sudo lgh-get installeras libjna java

Skapa symboliska länkar i $CASS_HOME/lib directory så att Cassandra startskript kan hitta dessa burkar:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

#### <a name="step-5-configure-cassandrayaml"></a>Steg 5: Konfigurera cassandra.yaml
Redigera cassandra.yaml på varje VM tooreflect konfiguration som krävs av alla hello virtuella datorer [vi ska justera detta under hello faktiska etableringen]:

<table>
<tr><th>Fältnamn   </th><th> Värde  </th><th>    Kommentarer </th></tr>
<tr><td>klusternamn </td><td>    ”CustomerService”    </td><td> Använd hello namn som motsvarar din distribution</td></tr>
<tr><td>listen_address    </td><td>[lämna det tomt]    </td><td> Ta bort ”localhost” </td></tr>
<tr><td>rpc_addres   </td><td>[lämna det tomt]    </td><td> Ta bort ”localhost” </td></tr>
<tr><td>frö    </td><td>"10.1.2.4, 10.1.2.6, 10.1.2.8"    </td><td>Lista över alla hello IP-adresser som är avsedda som frö.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Det här används av hello NetworkTopologyStrateg för procedurens hello datacenter och hello rack av hello VM</td></tr>
</table>

#### <a name="step-6-capture-hello-vm-image"></a>Steg 6: Hämta hello VM-avbildning
Logga in på hello virtuell dator med hello värdnamn (hk-cas-template.cloudapp.net) och hello privata SSH-nyckeln skapade tidigare. Se hur tooUse SSH med Linux i Azure för information om hur toolog med hello kommandot ssh eller putty.exe.

Kör följande sekvens med åtgärder toocapture hello avbildningen hello:

##### <a name="1-deprovision"></a>1. Avetablering
Kommandot hello ”sudo waagent – avetablering + användare” tooremove virtuella instansen specifik information. Se för [hur tooCapture en Linux-dator](capture-image.md) tooUse som en mall för mer information om hello image-hämtningen.

##### <a name="2-shutdown-hello-vm"></a>2: avstängning hello VM
Kontrollera att hello den virtuella datorn har markerats och klicka på hello avstängning länk från hello nedre kommandofältet.

##### <a name="3-capture-hello-image"></a>3: hello infångningsavbildning
Kontrollera att hello den virtuella datorn har markerats och klicka på hello AVBILDA länk från hello nedre kommandofältet. Namnge en bild (t.ex. hk-cas-2-08-ub-14-04-2014071) hello nästa skärm, lämpliga AVBILDNINGSBESKRIVNINGEN och på hello ”Kontrollera” Markera toofinish hello insamlingen.

Detta tar några sekunder och hello avbildningen ska vara tillgänglig under Mina AVBILDNINGAR i hello avbildningen gallery. Virtuella hello källdatorn tas automatiskt bort när hello avbildningen har avbildats. 

## <a name="single-region-deployment-process"></a>Processen för distribution av enskild Region
**Steg 1: Skapa hello virtuellt nätverk** logga in hello Azure-portalen och skapa ett virtuellt nätverk (klassiskt) med hello-attribut visas i följande tabell hello. Se [skapa ett virtuellt nätverk (klassiska) med hello Azure-portalen](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) detaljerade anvisningar för hello process.      

<table>
<tr><th>VM attributets namn</th><th>Värde</th><th>Kommentarer</th></tr>
<tr><td>Namn</td><td>vnet-fråga-Väst-oss</td><td></td></tr>
<tr><td>Region</td><td>Västra USA</td><td></td></tr>
<tr><td>DNS-servrar</td><td>Ingen</td><td>Ignorera detta eftersom vi inte använder en DNS-Server</td></tr>
<tr><td>Adressutrymmet</td><td>10.1.0.0/16</td><td></td></tr>    
<tr><td>Starta IP</td><td>10.1.0.0</td><td></td></tr>    
<tr><td>CIDR </td><td>/16 (65531)</td><td></td></tr>
</table>

Lägg till följande undernät hello:

<table>
<tr><th>Namn</th><th>Starta IP</th><th>CIDR</th><th>Kommentarer</th></tr>
<tr><td>webbprogram</td><td>10.1.1.0</td><td>/24 (251)</td><td>Undernät för hello webbgrupp</td></tr>
<tr><td>Data</td><td>10.1.2.0</td><td>/24 (251)</td><td>Undernät för hello databasnoder</td></tr>
</table>

Data och Web undernät kan skyddas via nätverkssäkerhetsgrupper hello täckning som ligger utanför omfånget för den här artikeln.  

**Steg 2: Etablera virtuella datorer** med hello avbildning som har skapats tidigare, vi skapar hello följande virtuella datorer i molnet hello server ”hk-c-svc-Väst” och binda dem toohello respektive undernät som visas nedan:

<table>
<tr><th>Namnet på datorn    </th><th>Undernät    </th><th>IP-adress    </th><th>Tillgänglighetsuppsättning</th><th>DC/Rack</th><th>Startvärde för?</th></tr>
<tr><td>HK-c1-Väst-oss    </td><td>Data    </td><td>10.1.2.4    </td><td>HK-c-aset-1    </td><td>DC = WESTUS rack = rack1 </td><td>Ja</td></tr>
<tr><td>HK-c2-Väst-oss    </td><td>Data    </td><td>10.1.2.5    </td><td>HK-c-aset-1    </td><td>DC = WESTUS rack = rack1    </td><td>Nej </td></tr>
<tr><td>HK-c3-Väst-oss    </td><td>Data    </td><td>10.1.2.6    </td><td>HK-c-aset-1    </td><td>DC = WESTUS rack = rack2    </td><td>Ja</td></tr>
<tr><td>HK-c4-Väst-oss    </td><td>Data    </td><td>10.1.2.7    </td><td>HK-c-aset-1    </td><td>DC = WESTUS rack = rack2    </td><td>Nej </td></tr>
<tr><td>HK-c5-Väst-oss    </td><td>Data    </td><td>10.1.2.8    </td><td>HK-c-aset-2    </td><td>DC = WESTUS rack = rack3    </td><td>Ja</td></tr>
<tr><td>HK-c6-Väst-oss    </td><td>Data    </td><td>10.1.2.9    </td><td>HK-c-aset-2    </td><td>DC = WESTUS rack = rack3    </td><td>Nej </td></tr>
<tr><td>HK-c7-Väst-oss    </td><td>Data    </td><td>10.1.2.10    </td><td>HK-c-aset-2    </td><td>DC = WESTUS rack = rack4    </td><td>Ja</td></tr>
<tr><td>HK-c8-Väst-oss    </td><td>Data    </td><td>10.1.2.11    </td><td>HK-c-aset-2    </td><td>DC = WESTUS rack = rack4    </td><td>Nej </td></tr>
<tr><td>HK-w1-Väst-oss    </td><td>webbprogram    </td><td>10.1.1.4    </td><td>HK-w-aset-1    </td><td>                       </td><td>Saknas</td></tr>
<tr><td>HK-w2-Väst-oss    </td><td>webbprogram    </td><td>10.1.1.5    </td><td>HK-w-aset-1    </td><td>                       </td><td>Saknas</td></tr>
</table>

Skapandet av hello ovan lista över virtuella datorer kräver hello följande process:

1. Skapa en tom molntjänst i ett visst område
2. Skapa en virtuell dator från hello tidigare avbildning och koppla den toohello virtuella nätverk som skapades tidigare; Upprepa detta för alla hello virtuella datorer
3. Lägg till en molntjänst för intern belastningsutjämnare toohello och koppla den toohello ”data-undernät
4. Lägg till en slutpunkt för Utjämning av nätverksbelastning för thrift-trafik via en belastningsutjämnad uppsättning anslutna toohello som tidigare har skapat intern belastningsutjämnare för varje VM som skapats tidigare

hello ovan processen kan utföras med hjälp av Azure klassiska portal; använda en Windows-dator (Använd en virtuell dator i Azure om du inte har åtkomst tooa Windows-dator), Använd följande PowerShell-skriptet tooprovision hello alla 8 virtuella datorer automatiskt.

**Lista över 1: PowerShell-skript för etablering av virtuella datorer**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into hello current Powershell session before proceeding
        #hello process: 1. create Azure Storage account, 2. create virtual network, 3.create hello VM template, 2. crate a list of VMs from hello template

        #fundamental variables - change these tooreflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores hello list of azure vm configuration objects
        #create hello list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add add hello thrift endpoint toohello internal load balancer for all hello VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**Steg 3: Konfigurera Cassandra på varje virtuell dator**

Logga in på hello VM och utför hello följande:

* Redigera $CASS_HOME/conf/cassandra-rackdc.properties toospecify hello data center och rack egenskaper:
  
       dc =EASTUS, rack =rack1
* Redigera cassandra.yaml tooconfigure seed noder enligt nedan:
  
       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Steg 4: Starta hello virtuella datorer och testa hello kluster**

Logga in på en av noderna i hello (t.ex. hk-c1-Väst-us) och kör hello efter kommandot toosee hello status för hello klustret:

       nodetool –h 10.1.2.4 –p 7199 status

Du bör se hello Visa liknande toohello en nedan för ett kluster på 8 noder:

<table>
<tr><th>Status</th><th>Adress    </th><th>Läsa in    </th><th>Token    </th><th>Äger </th><th>Värd-ID    </th><th>Rack</th></tr>
<tr><th>AVREGISTRERAR    </td><td>10.1.2.4     </td><td>87.81 KB    </td><td>256    </td><td>38.0%    </td><td>GUID (tas bort)</td><td>rack1</td></tr>
<tr><th>AVREGISTRERAR    </td><td>10.1.2.5     </td><td>41.08 KB    </td><td>256    </td><td>68.9%    </td><td>GUID (tas bort)</td><td>rack1</td></tr>
<tr><th>AVREGISTRERAR    </td><td>10.1.2.6     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (tas bort)</td><td>rack2</td></tr>
<tr><th>AVREGISTRERAR    </td><td>10.1.2.7     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (tas bort)</td><td>rack2</td></tr>
<tr><th>AVREGISTRERAR    </td><td>10.1.2.8     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (tas bort)</td><td>rack3</td></tr>
<tr><th>AVREGISTRERAR    </td><td>10.1.2.9     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (tas bort)</td><td>rack3</td></tr>
<tr><th>AVREGISTRERAR    </td><td>10.1.2.10     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (tas bort)</td><td>rack4</td></tr>
<tr><th>AVREGISTRERAR    </td><td>10.1.2.11     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (tas bort)</td><td>rack4</td></tr>
</table>

## <a name="test-hello-single-region-cluster"></a>Testa hello Region kluster
Använd hello följande steg tootest hello klustret:

1. Med hello Powershell-kommandot Get-AzureInternalLoadbalancer kommandot Hämta hello IP-adressen för hello intern belastningsutjämnare (t.ex.)  10.1.2.101). hello syntaxen för kommandot hello visas nedan: Get-AzureLoadbalancer – ServiceName ”hk-c-svc-Väst-oss” [visar hello information om hello intern belastningsutjämnare tillsammans med dess IP-adress]
2. Logga in på hello webbservergrupp VM (t.ex. hk-w1-Väst-us) med hjälp av Putty eller ssh
3. Köra $CASS_HOME/bin/cqlsh 10.1.2.101 9160
4. Använd hello efter CQL kommandon tooverify om hello klustret fungerar:
   
     Skapa KEYSPACE customers_ks med replikering = {'class': 'SimpleStrategy', 'replication_factor': 3};   ANVÄNDA customers_ks;   Skapa tabell Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   Infoga i Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   Infoga i Customers(customer_id, firstname, lastname) värden (2, ”Jane', 'Berg”).
   
     Välj * från kunder.

Du bör se en skärm som hello en nedan:

<table>
  <tr><th> customer_id </th><th> Förnamn </th><th> Efternamn </th></tr>
  <tr><td> 1 </td><td> John </td><td> Berg </td></tr>
  <tr><td> 2 </td><td> Jane </td><td> Berg </td></tr>
</table>

Observera att hello keyspace skapade i steg 4 använder SimpleStrategy med en replication_factor 3. SimpleStrategy rekommenderas för enda data center distributioner medan NetworkTopologyStrategy för flera data center distributioner. En replication_factor 3 ger tolerans för nodfel.

## <a id="tworegion"></a>Flera regioner distributionsprocessen
Utnyttja hello enskild region distributionen har slutförts och upprepa hello samma process för att installera andra hello-region. hello viktiga skillnaden mellan hello enda och distribution i flera region är hello VPN-tunnel konfiguration för kommunikation mellan region. Vi börjar med hello nätverksinstallation, etablera hello virtuella datorer och konfigurera Cassandra.

### <a name="step-1-create-hello-virtual-network-at-hello-2nd-region"></a>Steg 1: Skapa hello virtuella nätverk på hello 2 Region
Logga in på hello klassiska Azure-portalen och skapa ett virtuellt nätverk med hello attribut visas i hello tabell. Se [konfigurera ett virtuellt nätverk Cloud-Only i hello klassiska Azure-portalen](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) detaljerade anvisningar för hello process.      

<table>
<tr><th>Attributets namn    </th><th>Värde    </th><th>Kommentarer</th></tr>
<tr><td>Namn    </td><td>vnet-fråga-Öst-oss</td><td></td></tr>
<tr><td>Region    </td><td>Östra USA</td><td></td></tr>
<tr><td>DNS-servrar        </td><td></td><td>Ignorera detta eftersom vi inte använder en DNS-Server</td></tr>
<tr><td>Konfigurera en punkt-till-plats-VPN</td><td></td><td>        Ignorera detta</td></tr>
<tr><td>Konfigurera ett VPN mellan två platser</td><td></td><td>        Ignorera detta</td></tr>
<tr><td>Adressutrymmet    </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>Starta IP    </td><td>10.2.0.0    </td><td></td></tr>
<tr><td>CIDR    </td><td>/16 (65531)</td><td></td></tr>
</table>

Lägg till följande undernät hello:

<table>
<tr><th>Namn    </th><th>Starta IP    </th><th>CIDR    </th><th>Kommentarer</th></tr>
<tr><td>webbprogram    </td><td>10.2.1.0    </td><td>/24 (251)    </td><td>Undernät för hello webbgrupp</td></tr>
<tr><td>Data    </td><td>10.2.2.0    </td><td>/24 (251)    </td><td>Undernät för hello databasnoder</td></tr>
</table>


### <a name="step-2-create-local-networks"></a>Steg 2: Skapa lokala nätverk
Ett lokalt nätverk i Azure virtuella nätverk är en proxy-adressutrymme som mappar tooa fjärrplats, inklusive ett privat moln eller en annan Azure-region. Det här adressutrymmet för proxy är bundna tooa fjärr-gateway för routning nätverket toohello höger nätverk mål. Se [konfigurera VNet-tooVNet anslutning](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) hello instruktioner för VNET-till-VNET-anslutning upprättas.

Skapa två lokala nätverk per hello följande information:

| Nätverksnamn | VPN Gateway-adress | Adressutrymmet | Kommentarer |
| --- | --- | --- | --- |
| HK-lnet-Map-to-East-US |23.1.1.1 |10.2.0.0/16 |När du skapar hello lokalt nätverk ger en platshållare för gateway-adress. hello verkliga gatewayadress fylls när hello gateway har skapats. Se till att hello-adressutrymmet exakt matchar hello respektive Fjärrnätverket; i det här fallet skapas hello VNET i hello östra USA. |
| HK-lnet-Map-to-West-US |23.2.2.2 |10.1.0.0/16 |När du skapar hello lokalt nätverk ger en platshållare för gateway-adress. hello verkliga gatewayadress fylls när hello gateway har skapats. Se till att hello-adressutrymmet exakt matchar hello respektive Fjärrnätverket; i det här fallet skapas hello VNET i hello västra USA region. |

### <a name="step-3-map-local-network-toohello-respective-vnets"></a>Steg 3: Karta ”lokalt” nätverk toohello respektive Vnet
Markera varje virtuellt nätverk från hello klassiska Azure-portalen, klickar du på ”Konfigurera”, markera ”Anslut toohello lokala nätverk” och välj hello lokala nätverk per hello följande information:

| Virtual Network | Lokalt nätverk |
| --- | --- |
| HK-vnet-Väst-oss |HK-lnet-Map-to-East-US |
| HK-vnet-Öst-oss |HK-lnet-Map-to-West-US |

### <a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Steg 4: Skapa Gateways på VNET1 och VNET2
Klicka på Skapa GATEWAYEN som utlöser hello VPN-gateway etableringsprocessen hello instrumentpanelen för båda hello virtuella nätverk. Efter några minuter hello instrumentpanelen i varje virtuellt nätverk ska visa hello faktiska gateway-adress.

### <a name="step-5-update-local-networks-with-hello-respective-gateway-addresses"></a>Steg 5: Uppdatera ”lokala” nätverk med hello respektive ”” gatewayadresser
Redigera båda hello lokala nätverk tooreplace hello platshållare gateway IP-adressen med hello verkliga IP-adressen för hello bara etablerad gateway. Använd följande mappning hello:

<table>
<tr><th>Lokalt nätverk    </th><th>Virtuell nätverksgateway</th></tr>
<tr><td>HK-lnet-Map-to-East-US </td><td>Gateway för hk-vnet-Väst-oss</td></tr>
<tr><td>HK-lnet-Map-to-West-US </td><td>Gateway för hk-vnet-Öst-oss</td></tr>
</table>

### <a name="step-6-update-hello-shared-key"></a>Steg 6: Uppdatera hello delad nyckel
Använd hello följande Powershell-skriptet tooupdate hello IPSec-nyckel för varje VPN-gateway [Använd hello saké en nyckel för både hello gateways]: Set-AzureVNetGatewayKey - VNetName hk-vnet-Öst-oss - LocalNetworkSiteName hk-lnet-map-to-west-us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - VNetName hk-vnet-Väst-oss - LocalNetworkSiteName hk-lnet-map-to-east-us - SharedKey D9E76BKK

### <a name="step-7-establish-hello-vnet-to-vnet-connection"></a>Steg 7: Skapa hello VNET-till-VNET-anslutning
Använd hello ”INSTRUMENTPANELEN”-menyn i båda hello virtuella nätverk tooestablish gateway-till-gateway-anslutningen från hello klassiska Azure-portalen. Använd hello ”Anslut” menyalternativ i hello längst ned i verktygsfältet. Efter några minuter ska hello instrumentpanelen visa hello anslutningsinformation grafiskt.

### <a name="step-8-create-hello-virtual-machines-in-region-2"></a>Steg 8: Skapa hello virtuella datorer i området #2
Skapa hello Ubuntu avbildning enligt beskrivningen i region #1 distribution av följande hello samma steg eller kopiera hello avbildningen VHD-filen toohello Azure storage-konto finns i området #2 och skapa hello avbildning. Använd den här avbildningen och skapa hello efter listan över virtuella datorer till en ny molntjänst hk-c-svc-Öst-oss:

| Namnet på datorn | Undernät | IP-adress | Tillgänglighetsuppsättning | DC/Rack | Startvärde för? |
| --- | --- | --- | --- | --- | --- |
| HK-c1-Öst-oss |Data |10.2.2.4 |HK-c-aset-1 |DC = EASTUS rack = rack1 |Ja |
| HK-c2-Öst-oss |Data |10.2.2.5 |HK-c-aset-1 |DC = EASTUS rack = rack1 |Nej |
| HK-c3-Öst-oss |Data |10.2.2.6 |HK-c-aset-1 |DC = EASTUS rack = rack2 |Ja |
| HK-c5-Öst-oss |Data |10.2.2.8 |HK-c-aset-2 |DC = EASTUS rack = rack3 |Ja |
| HK-c6-Öst-oss |Data |10.2.2.9 |HK-c-aset-2 |DC = EASTUS rack = rack3 |Nej |
| HK-c7-Öst-oss |Data |10.2.2.10 |HK-c-aset-2 |DC = EASTUS rack = rack4 |Ja |
| HK-c8-Öst-oss |Data |10.2.2.11 |HK-c-aset-2 |DC = EASTUS rack = rack4 |Nej |
| HK-w1-Öst-oss |webbprogram |10.2.1.4 |HK-w-aset-1 |Saknas |Saknas |
| HK-w2-Öst-oss |webbprogram |10.2.1.5 |HK-w-aset-1 |Saknas |Saknas |

Följ hello samma instruktioner som region #1 men använda 10.2.xxx.xxx adressutrymme.

### <a name="step-9-configure-cassandra-on-each-vm"></a>Steg 9: Konfigurera Cassandra på varje virtuell dator
Logga in på hello VM och utför hello följande:

1. Redigera $CASS_HOME/conf/cassandra-rackdc.properties toospecify hello data center och rack egenskaper i hello-format: dc = EASTUS rack = rack1
2. Redigera de noder som cassandra.yaml tooconfigure startvärde: frö: ”10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10”

### <a name="step-10-start-cassandra"></a>Steg 10: Starta Cassandra
Logga in på varje virtuell dator och starta Cassandra i bakgrunden hello genom att köra följande kommando hello: $CASS_HOME/bin/cassandra

## <a name="test-hello-multi-region-cluster"></a>Testa hello flera regioner kluster
Nu har Cassandra distribuerade too16 noder med 8 noder i varje Azure-region. Dessa noder finns i samma kluster tack vare hello vanliga klusternamnet och hello seed nodkonfiguration hello. Använd följande process tootest hello klustret hello:

### <a name="step-1-get-hello-internal-load-balancer-ip-for-both-hello-regions-using-powershell"></a>Steg 1: Hämta hello interna belastningsutjämnare IP för båda hello regioner med hjälp av PowerShell
* Get-AzureInternalLoadbalancer - ServiceName ”hk-c-svc-Väst-oss”
* Get-AzureInternalLoadbalancer - ServiceName ”hk-c-svc-Öst-oss”  
  
    Observera hello IP-adresser (t.ex. west - 10.1.2.101, Öst - 10.2.2.101) visas.

### <a name="step-2-execute-hello-following-in-hello-west-region-after-logging-into-hk-w1-west-us"></a>Steg 2: Kör hello följande när du har loggat in hk-w1-Väst-oss i hello west region
1. Köra $CASS_HOME/bin/cqlsh 10.1.2.101 9160
2. Kör följande CQL kommandon hello:
   
     Skapa KEYSPACE customers_ks med replikering = {'class': 'NetworkToplogyStrategy', 'WESTUS': 3, EASTUS: 3};   ANVÄNDA customers_ks;   Skapa tabell Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   Infoga i Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   Infoga i Customers(customer_id, firstname, lastname) värden (2, ”Jane', 'Berg”).   Välj * från kunder.

Du bör se en skärm som hello en nedan:

| customer_id | Förnamn | Efternamn |
| --- | --- | --- |
| 1 |John |Berg |
| 2 |Jane |Berg |

### <a name="step-3-execute-hello-following-in-hello-east-region-after-logging-into-hk-w1-east-us"></a>Steg 3: Kör hello följande i hello Öst region när du har loggat in hk-w1-Öst-oss:
1. Köra $CASS_HOME/bin/cqlsh 10.2.2.101 9160
2. Kör följande CQL kommandon hello:
   
     ANVÄNDA customers_ks;   Skapa tabell Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   Infoga i Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   Infoga i Customers(customer_id, firstname, lastname) värden (2, ”Jane', 'Berg”).   Välj * från kunder.

Du bör se hello samma visas som visas för hello West region:

| customer_id | Förnamn | Efternamn |
| --- | --- | --- |
| 1 |John |Berg |
| 2 |Jane |Berg |

Kör några fler infogningar och finns att hämta de replikerade toowest-oss tillhör hello klustret.

## <a name="test-cassandra-cluster-from-nodejs"></a>Cassandra Testklustret från Node.js
Med hjälp av en av hello virtuella Linux-datorer reflektionsimportören i hello ”web” skiktet tidigare, vi ska utföra en enkel Node.js skriptet tooread hello tidigare infogas data

**Steg 1: Installera Node.js och Cassandra-klienten**

1. Installera Node.js och npm
2. Installera nod paketet ”cassandra-klient” med hjälp av npm
3. Kör hello följande skript i Kommandotolken hello shell som visar hello json-strängen hello hämtas data:
   
        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };
   
        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed toocreate Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }
   
        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed toocreate column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }
   
        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);
   
           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }
   
        //update will also insert hello record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }
   
        //read hello two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }
   
        //exectue hello code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)

## <a name="conclusion"></a>Slutsats
Microsoft Azure är en flexibel plattform som gör att hello körning av både Microsoft samt programvara med öppen källkod som visas av den här övningen. Hög tillgänglighet Cassandra kluster kan distribueras på ett enda datacenter via hello sprida hello klusternoder över flera feldomäner. Cassandra kluster kan också distribueras över flera Azure geografiskt avlägsna regioner för katastrofåterställning bevis system. Azure och Cassandra tillsammans aktiverar hello konstruktionen av skalbara, hög tillgänglighet och katastrofåterställning återställningsbara molntjänster behövs dagens internet skalas tjänster.  

## <a name="references"></a>Referenser
* [http://cassandra.apache.org](http://cassandra.apache.org)
* [http://www.datastax.com](http://www.datastax.com)
* [http://www.nodejs.org](http://www.nodejs.org)

