---
title: "aaaNetwork Prestandaövervakaren lösning i Azure Log Analytics | Microsoft Docs"
description: "Network Performance Monitor i Azure Log Analytics hjälper dig att övervaka hello prestanda för ditt nätverk – i nära real gång toodetect och leta upp flaskhalsar i nätverket."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.openlocfilehash: e074948221fdd003c640861d759c4ce69ced1cd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="network-performance-monitor-solution-in-log-analytics"></a>Network Performance Monitor-lösning i logganalys

![Nätverket Prestandaövervakaren symbol](./media/log-analytics-network-performance-monitor/npm-symbol.png)

Det här dokumentet beskriver hur tooset upp och Använd hello Network Performance Monitor-lösning i logganalys som hjälper dig att övervaka hello prestanda för ditt nätverk – i nära real gång toodetect och hitta flaskhalsar i nätverket. Du kan övervaka hello förluster eller fördröjningar mellan två nätverk, undernät eller servrar med hello Network Performance Monitor-lösning. Network Performance Monitor identifierar nätverksproblem som trafik blackholing routning fel och problem med att konventionella nätverket övervakning metoderna inte är kan toodetect. Network Performance Monitor genererar aviseringar och meddelar och när ett tröskelvärde komprometteras för en nätverkslänk. Dessa tröskelvärden kan hämtas automatiskt av hello system eller konfigurera toouse anpassade Varningsregler. Network Performance Monitor garanterar snabbt ska kunna identifiera problem med prestanda och localizes hello källa för hello problemet tooa särskilt nätverkssegment eller enheter.

Du kan identifiera problem med hello lösning instrumentpanelen som visar sammanfattningsinformation om nätverket inklusive senaste hälsa händelser på nätverket, nätverkslänkar och undernätverkslänkar som ställs inför hög paketförlust och latens. Du kan gå nedåt till ett nätverk länken tooview hello aktuella hälsotillstånd undernätverkslänkar samt nod till nod länkar. Du kan också visa hello historiska trender för förluster eller fördröjningar på hello nätverk, undernätverk och nod-till-nod-nivå. Du kan identifiera tillfälliga nätverksproblem genom att visa historiska trender diagram för paketförlust och fördröjning och hitta flaskhalsar i nätverk på en topologisk karta. hello interaktiva topologi diagrammet kan du toovisualize hello hopp som nexthop nätverksvägar och fastställa hello källan hello problemet. Precis som andra lösningar du kan använda loggen Sök efter olika analytics krav toocreate anpassade rapporter baserat på hello data som samlas in av Network Performance Monitor.

hello lösningen använder syntetiska transaktioner som en mekanism för primära toodetect nätverksfel. Därför kan du använda det utan hänsyn till en viss nätverksenhet tillverkare eller modell. Den fungerar även över lokala molnet (IaaS) och hybridmiljöer. hello lösningen upptäcker automatiskt hello nätverkstopologi och olika vägar i nätverket.

Vanliga nätverk övervakning produkter fokus på övervakning hello nätverk hälsotillståndet för enheten (routrar, växlar osv) men ger inte insikter om hello faktiska kvaliteten på nätverksanslutningen mellan två platser, vilket gör Network Performance Monitor.

### <a name="using-hello-solution-standalone"></a>Använder hello lösning fristående
Om du vill toomonitor hello kvalitet Nätverksanslutningar mellan sina kritiska arbetsbelastningar, kan nätverk, Datacenter eller office platser och du använda hello Network Performance Monitor lösning ensamt toomonitor anslutning hälsa mellan:

* flera Datacenter eller office platser som är anslutna via ett offentligt eller privat nätverk
* viktiga arbetsbelastningar som körs affärsprogram
* offentliga molntjänster som Microsoft Azure eller Amazon Web Services (AWS) och lokala nätverk, om du har IaaS (VM) tillgängliga och gateways har konfigurerats tooallow kommunikation mellan lokala nätverk och moln-nätverk
* Azure och lokala nätverk när du använder Express Route

### <a name="using-hello-solution-with-other-networking-tools"></a>Med andra nätverk verktyg hello lösning
Om du vill toomonitor en driftsapplikationer, kan du använda hello Network Performance Monitor-lösning som en tillhörande lösning tooother verktyg. Ett långsamt nätverk kan leda tooslow program och Network Performance Monitor kan hjälpa dig att undersöka problem med prestanda som orsakas av underliggande nätverksproblem. Eftersom hello lösning inte kräver någon åtkomst toonetwork enheter, behöver inte toorely på ett team tooprovide nätverksinformation om hur hello nätverket påverkar program programadministratör hello.

Även om du redan investerar i andra nätverksövervakningsverktyg kan sedan hello-lösning kompletterar dessa verktyg eftersom de flesta traditionella övervakning nätverkslösningar inte tillhandahåller insikter om slutpunkt till slutpunkt prestandamått som förluster eller fördröjningar.  hello Network Performance Monitor-lösningen hjälper dig att fylla den lucka.

## <a name="installing-and-configuring-agents-for-hello-solution"></a>Installera och konfigurera agenter för hello lösning
Använd hello grundläggande processer tooinstall agenter på [ansluta Windows-datorer tooLog Analytics](log-analytics-windows-agents.md) och [ansluta Operations Manager tooLog Analytics](log-analytics-om-agents.md).

> [!NOTE]
> Du behöver tooinstall minst 2 agenter i ordning toohave toodiscover tillräckligt med data och övervaka dina nätverksresurser. Annars förblir hello lösning i att konfigurera läge tills du installerar och konfigurerar ytterligare agenter.
>
>

### <a name="where-tooinstall-hello-agents"></a>Där tooinstall hello agenter
Innan du installerar agenter, Överväg hello topologin för ditt nätverk och vilka delar av hello nätverk du vill toomonitor. Vi rekommenderar att du installerar fler än en agent för varje undernät som du vill toomonitor. Med andra ord för varje undernät som du vill toomonitor, välja två eller flera servrar eller virtuella datorer och installera hello agent på dem.

Om du är osäker på om hello topologin för ditt nätverk kan du installera hello agenter på servrar med viktiga arbetsbelastningar där du vill att toomonitor hello nätverkets prestanda. Exempelvis kanske du vill tookeep reda på en nätverksanslutning mellan en webbserver och en server som kör SQL Server. I det här exemplet skulle du installerar en agent på båda servrarna.

Agenter övervakar nätverksanslutning (länkar) mellan värdar--inte hello värdar sig själva. I så fall toomonitor en nätverkslänk måste du installera agenter på båda slutpunkter för länken.

### <a name="configure-agents"></a>Konfigurera agenter

Om du planerar toouse hello ICMP-protokoll för syntetiska transaktioner, behöver du tooenable hello följande brandväggsregler för att på ett tillförlitligt sätt använder ICMP:

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow
```


Om du avser toouse hello TCP-protokoll måste tooopen portar i brandväggen för dessa datorer tooensure som agenter kan kommunicera. Du behöver toodownload och kör sedan hello [EnableRules.ps1 PowerShell-skript](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) utan några parametrar i ett PowerShell-fönster med administratörsbehörighet.

hello skriptet skapar registernycklar som krävs av hello Network Performance Monitor och skapar Windows-brandväggen regler tooallow agenter toocreate TCP-anslutningar med varandra. hello registernycklar som skapats av hello skript kan även ange om toolog hello debug-loggar och hello sökväg för hello loggar fil. Den definierar även hello agent TCP-port som används för kommunikation. hello värden för nycklarna anges automatiskt av hello skript, så du inte bör ändra dessa nycklar manuellt.

hello port öppnas som standard är 8084. Du kan använda en anpassad port genom att tillhandahålla hello parametern `portNumber` toohello skript. Dock ska hello samma port användas på alla hello datorer där hello skript körs.

> [!NOTE]
> hello EnableRules.ps1 skriptet konfigurerar regler för Windows-brandväggen på hello dator där hello skript körs. Om du har en brandvägg för nätverk, bör du kontrollera att den tillåter trafik för hello TCP-port som används av Network Performance Monitor.
>
>

## <a name="configuring-hello-solution"></a>Konfigurera hello lösning
Använd följande information tooinstall hello och konfigurera hello lösning.

1. hello Network Performance Monitor lösningen hämtar data från datorer som kör Windows Server 2008 SP 1 eller senare eller Windows 7 SP1 eller senare, vilket är hello samma krav som hello Microsoft Monitoring Agent (MMA). NPM agenter kan också köra på Windows desktop/client-operativsystem (Windows 10, Windows 8.1, Windows 8 och Windows 7).
    >[!NOTE]
    >hello agenter för Windows server-operativsystem stöder både TCP och ICMP som hello protokoll för syntetisk transaktion. Hello agenter för Windows-klientoperativsystem stöder dock endast ICMP som hello protokoll för syntetisk transaktion.

2. Lägg till hello Network Performance Monitor lösning tooyour arbetsyta från [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).  
   ![Nätverket Prestandaövervakaren symbol](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. I hello OMS-portalen, ser du en ny panel med titeln **Network Performance Monitor** med hälsningsmeddelande *lösningen kräver ytterligare konfiguration*. Du behöver tooconfigure hello lösning tooadd nätverk baserat på undernät och noder som identifieras av agenter. Klicka på **Network Performance Monitor** toostart konfigurera hello standardnätverk.  
   ![lösningen kräver ytterligare konfiguration](./media/log-analytics-network-performance-monitor/npm-config.png)

### <a name="configure-hello-solution-with-a-default-network"></a>Konfigurera hello lösning med en standardnätverk
På sidan för konfiguration av hello ser du ett enda nätverk med namnet **standard**. När du inte har definierats några nätverk placeras alla hello automatiskt identifieras undernät i hello standardnätverk.

När du skapar ett nätverk kan du lägga till ett undernät tooit och undernätet tas bort från hello standardnätverk. Om du tar bort ett nätverk, returneras alla undernät automatiskt toohello standardnätverk.

Med andra ord hello nätverk som är standard hello behållare för alla hello-undernät som inte ingår i en användardefinierad nätverk. Du kan inte redigera eller ta bort hello standardnätverk. Det finns alltid i hello system. Du kan dock skapa så många nätverk som du behöver.

I de flesta fall hello undernät i din organisation ska ordnas i flera nätverk och skapar du en eller flera nätverk toologically gruppera dina undernät.

### <a name="create-new-networks"></a>Skapa nya nätverk
Ett nätverk i Prestandaövervakaren för nätverk är en behållare för undernät. Du kan skapa ett nätverk med ett namn och Lägg till undernät toohello nätverk. Du kan till exempel skapa ett nätverk med namnet *Building1* och Lägg sedan till undernät eller skapa ett nätverk med namnet *DMZ* och Lägg sedan till alla undernät som hör toodemilitarized zon toothis nätverk.

#### <a name="toocreate-a-new-network"></a>toocreate ett nytt nätverk
1. Klicka på **Lägg till nätverket** och skriver sedan hello nätverkets namn och beskrivning.
2. Välj ett eller flera undernät och klicka sedan på **Lägg till**.
3. Klicka på **spara** toosave hello konfiguration.  
   ![lägga till nätverk](./media/log-analytics-network-performance-monitor/npm-add-network.png)

### <a name="wait-for-data-aggregation"></a>Vänta tills Datasammanställning
När du har sparat hello konfiguration för första gången, startar hello lösning samla in paket förluster eller fördröjningar nätverksinformation mellan hello noder där agenter är installerade. Den här processen kan ta en stund ibland över 30 minuter. Under det här tillståndet hello Network Performance Monitor hello översiktssidan visas ett meddelande om *Datasammanställning pågående*.

![Dataaggregering pågår](./media/log-analytics-network-performance-monitor/npm-aggregation.png)

När hello data har överförts visas hello Network Performance Monitor panelen uppdateras visar data.

![Prestandaövervakaren nätverksikon](./media/log-analytics-network-performance-monitor/npm-tile.png)

Klicka på hello panelen tooview hello Network Performance Monitor instrumentpanelen.

![Nätverket Prestandaövervakaren instrumentpanelen](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Redigera inställningarna för övervakning för undernät
Alla undernät där minst en agent har installerats visas på hello **undernätverk** fliken i hello konfigurationssidan.

#### <a name="tooenable-or-disable-monitoring-for-particular-subnetworks"></a>tooenable eller inaktivera övervakning för specifika undernät
1. Hello markerar eller avmarkerar kryssrutan nästa toohello **ID för undernätverk** och se till att **för övervakning** är markerad eller avmarkerad, vid behov. Du kan markera eller avmarkera flera undernät. När den är inaktiverad, övervakas inte undernätverk som hello agenter ska vara uppdaterade toostop pinga andra agenter.
2. Välj hello noder som du vill toomonitor för en viss undernätverk genom att välja hello undernätverk hello listan och flytta hello krävs noder mellan hello listor som innehåller oövervakade och övervakade noder.
   Du kan lägga till en anpassad **beskrivning** toohello undernätverk om du vill.
3. Klicka på **spara** toosave hello konfiguration.  
   ![Redigera undernätet](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-toomonitor"></a>Välj noder toomonitor
Alla hello-noder som har en agent installerad visas i hello **noder** fliken.

#### <a name="tooenable-or-disable-monitoring-for-nodes"></a>tooenable eller inaktivera övervakning för noder
1. Markera eller avmarkera hello noder som du vill toomonitor eller stoppa övervakningen.
2. Klicka på **för övervakning**, eller avmarkera den efter behov.
3. Klicka på **Spara**.  
   ![aktivera övervakning av nod](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)

### <a name="set-monitoring-rules"></a>Ange regler för övervakning
Network Performance Monitor genererar hälsa händelser om hello-anslutning mellan två noder eller undernätverk eller nätverket länkar när ett tröskelvärde komprometteras. Dessa tröskelvärden kan hämtas automatiskt av hello system eller konfigurera anpassade Varningsregler.

Hej *standardregel* skapas av hello system och skapar en hälsohändelse när förlust eller fördröjning mellan ett par nätverk eller undernätverk länkar överträdelser hello system lärt dig tröskelvärdet. Du kan välja toodisable hello standardregel och skapa anpassade regler för övervakning

#### <a name="toocreate-custom-monitoring-rules"></a>toocreate anpassade regler för övervakning
1. Klicka på **Lägg till regel** i hello **övervakaren** och ange hello namn och beskrivning.
2. Välj hello par nätverk eller undernätverk länkar toomonitor hello listor.
3. Först välja hello nätverk i vilka hello första undernätverk/s intressanta finns hello nätverket listrutan och välj sedan hello undernätverk/s hello motsvarande undernätverk listrutan.
   Välj **alla undernät** om du vill toomonitor hello alla undernät i en nätverkslänk. Välj hello på samma sätt andra undernätverk/s av intresse. Och du kan klicka på **Lägg till undantag** tooexclude övervakning för viss undernätverk länkar från hello val du har gjort.
4. Välj mellan ICMP- och TCP protokoll för att köra syntetiska transaktioner.
5. Om du inte vill toocreate hälsa händelser för hello-objekt som du har valt, avmarkera **aktivera övervakning av hälsotillstånd på hello länkar som omfattas av den här regeln**.
6. Välj övervakning villkor.
   Du kan ange anpassade tröskelvärden för hälsotillstånd händelse genereras genom att skriva tröskelvärden. När hello värdet av hello villkor går över dess valda tröskelvärdet för hello valda nätverket/undernätverk par, skapas en hälsohändelse.
7. Klicka på **spara** toosave hello konfiguration.  
   ![Skapa anpassad regel för övervakning](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)

När du har sparat en regel för övervakning, du kan integrera regeln med Alert Management genom att klicka på **skapa avisering**. En aviseringsregel skapas automatiskt med hello sökfråga och andra nödvändiga parametrar automatiskt ifyllda. Med hjälp av en aviseringsregel, du kan ta emot e-postbaserad aviseringar dessutom toohello befintliga aviseringar i NPM. Aviseringar kan också utlösa vidtar åtgärder med runbooks eller de kan integrera med befintliga lösningar tjänsten med hjälp av webhooks. Du kan klicka på **hantera avisering** tooedit hello aviseringsinställningar.

### <a name="choose-hello-right-protocol-icmp-or-tcp"></a>Välj hello rätt protokoll-ICMP- eller TCP

Network Performance Monitor (NPM) använder prestandamått för syntetiska transaktioner toocalculate nätverk som paketet dataförlust och länka svarstid. toounderstand detta bättre, Överväg att en NPM agent anslutna tooone slutet av en nätverkslänk. Den här NPM agenten skickar avsökningen paket tooa andra NPM agent anslutna tooanother slutet av hello nätverk. hello andra agenten svarar med svarspaket. Den här processen upprepas några gånger. Genom att mäta hello antalet svar och tidsåtgång tooreceive varje svar, hello första NPM agent utvärderar länkfördröjningen och tappade paket.

hello bestäms format, storlek och ordning i paketen av hello protokoll som du väljer när du skapar regler för övervakning. Baserat på protokollet för hälsningspaket hello mellanliggande nätverksenheter (routrar, växlar osv.) kan bearbeta paketen på olika sätt. Önskat protokoll påverkar därför hello riktighet hello resultat. Och önskat protokoll avgör också om du måste vidta någon manuella steg när du distribuerar hello NPM lösning.

NPM erbjuder du hello val mellan ICMP- och TCP-protokoll för att köra syntetiska transaktioner.
Om du väljer ICMP när du skapar en regel för syntetisk transaktion använder hello NPM agenter ICMP ECHO-meddelanden toocalculate hello nätverks-svarstid och paketförlust. ICMP ECHO använder hello samma meddelande som skickas av hello konventionella Ping-verktyget. När du använder TCP som protokoll hello skickar NPM agenter TCP SYN paket över hello nätverk. Detta följs av en TCP-handskakning slutförande och sedan ta bort hello anslutning med RST paket.

#### <a name="points-tooconsider-before-choosing-hello-protocol"></a>Punkter tooconsider innan du väljer hello-protokollet
Överväg följande information innan du väljer en protokollet toouse hello:

##### <a name="discovering-multiple-network-routes"></a>Identifiering av flera nätverksvägar
TCP är mer exakt när identifiering av flera vägar och den måste färre agenter i varje undernät. En eller två agenter som använder TCP kan exempelvis identifiera alla redundanta sökvägar mellan undernät. Du måste dock flera agenter med ICMP tooachieve liknande resultat. Med ICMP, om du har *N* antal vägar mellan två undernät som du behöver mer än 5*N* agenter i källan eller målet undernät.

##### <a name="accuracy-of-results"></a>Riktighet resultat
Routrar och växlar ofta tooassign lägre prioritet tooICMP ECHO paket jämfört med tooTCP-paket. I vissa situationer när nätverksenheter är högt belastad, visar hello data från TCP närmare hello förluster eller fördröjningar orsakade av program. Detta inträffar eftersom de flesta av hello programmet trafiken flödar över TCP. I sådana fall ger ICMP mindre exakt resultaten jämförs tooTCP.

##### <a name="firewall-configuration"></a>Brandväggskonfiguration
TCP-protokollet kräver att TCP-paket skickas tooa målport. hello standardport som används av NPM agenter är 8084, men du kan ändra detta när du konfigurerar agenter. Du måste därför tooensure att din nätverkets brandväggar eller NSG-regler (i Azure) tillåter trafik på port hello. Du måste också toomake till den lokala hello-brandväggen på hello datorer där agenter är installerade är konfigurerat tooallow trafik på den här porten.

Du kan använda PowerShell-skript tooconfigure brandväggsregler på dina datorer som kör Windows, men du behöver tooconfigure nätverkets brandvägg manuellt.

Däremot fungerar inte ICMP använder port. I de flesta företagsscenarier tillåts ICMP-trafik via hello brandväggar tooallow du toouse verktyg för Nätverksdiagnostik som hello Ping-verktyget. Så om du kan pinga en dator från en annan, kan du använda hello ICMP-protokollet utan tooconfigure brandväggar manuellt.

> [!NOTE]
> Om du inte är säker på vilket protokoll toouse välja ICMP toostart med. Om du inte är nöjd med resultaten hello växla du alltid tooTCP senare.


#### <a name="how-tooswitch-hello-protocol"></a>Hur tooswitch hello protokoll

Om du har valt toouse ICMP under distributionen kan du växla tooTCP när som helst genom att redigera hello standardinställningar för övervakning av regeln.

##### <a name="tooedit-hello-default-monitoring-rule"></a>tooedit hello övervakning Standardregeln för
1.  Navigera för**nätverksprestanda** > **övervakaren** > **konfigurera** > **övervakaren** och klicka sedan på **standardregel**.
2.  Rulla toohello **protokollet** avsnittet och väljer hello protokoll som du vill toouse.
3.  Klicka på **spara** tooapply hello inställningen.

Även om Standardregeln hello använder ett visst protokoll, kan du skapa nya regler med ett annat protokoll. Du kan även skapa en blandning av regler där vissa hello regler använder ICMP och en annan med TCP.




## <a name="data-collection-details"></a>Information om samlingen
Network Performance Monitor använder TCP SYN-SYNACK-ACK handskakning paket när TCP är valt och ICMP ECHO ICMP ECHO SVARSMEDDELANDEN när ICMP har valts som hello toocollect förluster eller fördröjningar protokollinformation. Traceroute är också används tooget topologiinformation.

hello följande tabell visar metoder för insamling av data och annan information om hur data samlas in för Network Performance Monitor.

| Plattform | Styr Agent | SCOM-agent | Azure Storage | SCOM krävs? | SCOM-agent data som skickas via management-grupp | Frekvens för samlingen |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |TCP-handskakningar på/ICMP ECHO-meddelanden var femte sekund data skickas var 3: e minut |

hello lösningen använder syntetiska transaktioner tooassess hello hälsotillstånd hello nätverk. OMS-agenter som installerats på olika punkt i hello nätverkspaket exchange TCP eller ICMP Echo (beroende på hello protokoll har valts för övervakning) med varandra. Hello pågående Läs agenter hello fram och åter tid och paket förlust, eventuella. Varje agent utförs med jämna mellanrum, även en trace väg tooother agenter toofind alla hello olika vägar i hello-nätverket som ska testas. Med dessa data kan härleda hello agenter hello nätverks-svarstid och paket förlust siffrorna. hello tester upprepas var femte sekund och data som sammanställs för tre minuter av hello-agenter innan du laddar upp det toohello logganalys-tjänsten.

> [!NOTE]
> Även om agenter är ofta kommunicerar med varandra, genererar de inte mycket nätverkstrafik när utföra hello tester. Agenter använda enbart TCP SYN-SYNACK-ACK handskakning paket toodetermine hello förluster eller fördröjningar – inga data som utbyts paket. Under den här processen agenter kommunicerar med varandra endast vid behov och hello agent kommunikation topologi optimeras tooreduce nätverkstrafik.
>
>

## <a name="using-hello-solution"></a>Med hello-lösning
Det här avsnittet beskriver alla hello instrumentpanelen funktioner och hur toouse dem.

### <a name="solution-overview-tile"></a>Lösning: översikt sida vid sida
När du har aktiverat hello Network Performance Monitor lösning ger hello lösning panelen på översiktssidan för hello OMS en snabb överblick över hello nätverkets tillstånd. Ett ringdiagram visar hello antal felfria och feltillstånd undernätverkslänkar visas. När du klickar på panelen hello öppnas hello lösning instrumentpanelen.

![Prestandaövervakaren nätverksikon](./media/log-analytics-network-performance-monitor/npm-tile.png)

### <a name="network-performance-monitor-solution-dashboard"></a>Nätverket Prestandaövervakaren lösning instrumentpanelen
Hej **nätverk sammanfattning** bladet visas en sammanfattning av hello nätverk tillsammans med relativ storlek. Detta följs av paneler visar totalt antal nätverkslänkar, undernät länkar och sökvägar i hello system (en sökväg som består av hello IP-adresser för två värdar med agenter och alla hello hopp mellan dem).

Hej **upp händelser på nätverket hälsa** bladet visar en lista över senaste hälsa händelser och aviseringar i hello system och hello tid eftersom hello händelse har varit aktiv. En hälsohändelse eller en varning genereras när hello paketförlust eller latens för en länk för nätverk eller undernätverk överskrider ett tröskelvärde.

Hej **upp ohälsosamt nätverkslänkar** bladet visar en lista över nätverkslänkar. Dessa är hello nätverkslänkar som har en eller flera negativa hälsohändelse för tillfället hello.

Hej **upp Undernätverkslänkar med mest förlust** och **Undernätverkslänkar med mest svarstid** blad visar hello översta undernätverkslänkar av paketförlust och främsta undernätverkslänkar efter svarstid respektive. Fördröjningar eller vissa mängden paketförlust kan förväntas på vissa nätverkslänkarna. Dessa länkar visas i topp tio hello-listor, men är inte dåligt hälsotillstånd.

Hej **vanliga frågor** bladet innehåller en uppsättning sökfrågor som hämta rådata för nätverksövervakning data direkt. Du kan använda de här frågorna som utgångspunkt för att skapa egna frågor för anpassad rapportering.

![Nätverket Prestandaövervakaren instrumentpanelen](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="drill-down-for-depth"></a>Gå nedåt för djup
Du kan klicka på olika länkar på hello lösning instrumentpanelen toodrill nedåt djupare i alla intresseområde. Till exempel när du ser en avisering eller ett feltillstånd nätverkslänken som visas på instrumentpanelen för hello, du kan klicka på den tooinvestigate ytterligare. Vidarebefordras du tooa sidan som visar alla hello undernätverkslänkar för hello nätverks-länk. Du kommer att kunna toosee hello förlust, svarstid och hälsa status för varje undernätverk länken och snabbt ta reda på vilka undernätverkslänkar orsakar hello problem. Du kan klicka **visa nodlänkar** toosee alla hello nodlänkar för hello ohälsosamt undernät länka. Du kan sedan se enskilda nod till nod länkar och hitta hello nodlänkar.

Du kan klicka på **visa topologi** tooview hello hopp av hopp topologi hello vägar mellan hello käll- och noder. Hej ohälsosamt vägar eller hopp visas i rött så att du snabbt kan identifiera hello problemet tooa viss del av hello nätverk.

![specificera-data](./media/log-analytics-network-performance-monitor/npm-drill.png)

### <a name="network-state-recorder"></a>Nätverket lådan

Varje visas en ögonblicksbild av nätverkets tillstånd vid en viss tidpunkt. Som standard visas hello senaste tillstånd. hello visas hello överst på hello sidan hello punkt i tiden som hello status visas. Du kan välja toogo tillbaka i tiden och visa hello ögonblicksbild av nätverkets tillstånd genom att klicka på hello-fältet på **åtgärder**. Du kan också välja tooenable eller inaktivera automatisk uppdatering för en sida när du visar hello senaste tillstånd.

![nätverket](./media/log-analytics-network-performance-monitor/network-state.png)

#### <a name="trend-charts"></a>Trend diagram
På varje nivå som du nedåt, du kan se hello trend förluster eller fördröjningar för en nätverkslänk. Trend diagram är också tillgängliga för undernätverk och nod-länkar. Du kan ändra hello tidsintervall för hello diagram tooplot genom att använda hello tid kontrollen hello överst i hello diagram.

Trend diagram visar en historisk perspektiv hello prestanda för en nätverkslänk för. Vissa nätverksproblem är tillfälligt till sin natur och skulle vara svårt toocatch endast genom att titta på hello hello nätverkets aktuella tillstånd. Detta beror på att problem kan ansluta snabbt och försvinner innan alla meddelanden endast tooreappear vid ett senare tillfälle. Sådana tillfälliga problem kan också vara svårt för administratörer eftersom de utfärdar ofta yta som oförklarade ökningen av svarstiden programmet, även när alla programkomponenter visas toorun smidigt.

Du kan enkelt identifiera dessa typer av problem genom att titta på trenddiagram där hello problemet visas som en plötslig topp i antal paket eller fördröjning nätverksförluster.

![trenddiagram](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>hopp av hopp topologisk karta
Network Performance Monitor visar du hello hopp av hopp topologin av routinginformation mellan två noder i en interaktiv topologisk karta. Du kan visa hello topologisk karta genom att markera en nod länk och sedan klicka på **visa topologi**. Du kan också visa hello topologisk karta genom att klicka på **sökvägar** rutan på hello instrumentpanel. När du klickar på **sökvägar** på hello instrumentpanelen har du tooselect hello käll- och noder hello vänstra panelen och klicka sedan på **Rita** tooplot hello vägar mellan hello två noder.

hello topologisk karta visar hur många vägar som finns mellan hello två noder och vilka sökvägar hello data paket ta. Flaskhalsar i nätverket är markerat i rött på hello topologisk karta. Du kan hitta en felaktig nätverksanslutning eller en felaktig nätverksenhet genom att titta på red färgade elementen på hello topologisk karta.

När du klickar på en nod eller hovra över den på hello topologisk karta visas hello egenskaper för hello nod som FQDN och IP-adressen. Klicka på ett hopp toosee IP-adressen är. Du kan välja toofilter viss vägar med hjälp av hello filter i hello döljas åtgärdsfönstret. Och du kan också förenkla hello nätverkstopologier genom att dölja hello mellanliggande hopp med hello skjutreglaget hello åtgärdsfönstret. Du kan zooma in eller out-of-hello topologisk karta med mushjulet.

Observera att hello-topologi som visas på kartan hello är nivå 3-topologi och innehåller inte nivå 2-enheter och anslutningar.

![hopp av hopp topologisk karta](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Lokalisering av fel
Network Performance Monitor är flaskhalsar i kan toofind hello nätverk utan att ansluta toohello nätverksenheter. Network Performance Monitor gör baserat på hello data som samlas in från hello nätverket och genom att använda avancerade algoritmer på hello nätverket diagram, en probabilistic uppskattning av hello delar av nätverket som är mest sannolika hello källan hello problemet.

Den här metoden är användbar toodetermine hello nätverksflaskhalsar när åtkomst toohops är inte tillgängligt eftersom det inte krävs någon toobe för data som samlats in från hello nätverksenheter som routrar och växlar. Detta är också användbart när hello hopp mellan två noder som inte ingår i din administrativ kontroll. Hello hopp kanske till exempel Internet-routrar.

### <a name="log-analytics-search"></a>Logganalys Sök
Alla data som exponeras grafiskt genom hello Network Performance Monitor instrumentpanelen och nedåt sidor finns inbyggt i logganalys-sökning. Du kan fråga hello data med hjälp av hello Sök frågespråket och skapa anpassade rapporter genom att exportera hello data tooExcel eller PowerBI. Hej **vanliga frågor** bladet hello instrumentpanelen har vissa användbara frågor som du kan använda som hello som startpunkt för att skapa egna frågor och rapporter.

![sökfrågor](./media/log-analytics-network-performance-monitor/npm-queries.png)

## <a name="investigate-hello-root-cause-of-a-health-alert"></a>Undersök hello grundorsaken till en avisering om hälsa
Nu när du har läst om Network Performance Monitor vi titta på en enkel undersökning hello-grundorsaken till en hälsohändelse.

1. På översiktssidan för hello får du en ögonblicksbild hello hälsotillståndet hos ditt nätverk genom att följa hello **Network Performance Monitor** panelen. Observera att utanför hello 6 undernätverk länkar som övervakas, 2 är felfria. Detta garanterar undersökning. Klicka på hello panelen tooview hello lösning instrumentpanelen.  
   ![Prestandaövervakaren nätverksikon](./media/log-analytics-network-performance-monitor/npm-investigation01.png)
2. I hello exempel bilden nedan märker du att det finns en hälsohändelse som en nätverkslänk som är i feltillstånd. Du bestämmer dig tooinvestigate hello problemet och klicka på hello **DMZ2 DMZ1** nätverk länken toofind ut hello roten för hello problem.  
   ![exempel ohälsosamt nätverk](./media/log-analytics-network-performance-monitor/npm-investigation02.png)
3. hello nedåt sidan visas alla hello undernätverkslänkar i **DMZ2 DMZ1** nätverkslänken. Lägg märke till att hello svarstid för både hello undernätverkslänkar har passerat hello tröskelvärdet gör nätverkslänken hello feltillstånd. Du kan också se hello latens trender för båda hello undernätverkslänkar. Du kan använda hello tidsåtgången markering i hello diagram toofocus på hello tidsintervall. Du kan se hello tidpunkten hello när latens har nått sin belastning. Du kan söka hello loggar för problemet tid period tooinvestigate hello senare. Klicka på **visa nodlänkar** toodrill nedåt ytterligare.  
   ![feltillstånd undernät länkar exempel](./media/log-analytics-network-performance-monitor/npm-investigation03.png)
4. Liknande toohello föregående sida hello nedåt sidan för hello viss undernätverk länken visas ned dess ingående nodlänkar. Du kan utföra samma åtgärder som du gjorde i hello föregående steg. Klicka på **visa topologi** tooview hello topologi mellan hello 2 noder.  
   ![felaktiga noden länkar exempel](./media/log-analytics-network-performance-monitor/npm-investigation04.png)
5. Alla hello sökvägar mellan hello 2 valda noder ritas i hello topologisk karta. Du kan visualisera hello hopp av hopp topologi vägar mellan två noder på hello topologisk karta. Den ger dig en tydlig bild av hur många vägar finns mellan hello två noder och vilka sökvägar hello datapaket tar. Flaskhalsar i nätverket markeras med röd färg. Du kan hitta en felaktig nätverksanslutning eller en felaktig nätverksenhet genom att titta på red färgade elementen på hello topologisk karta.  
   ![exemplet ohälsosamt topologi](./media/log-analytics-network-performance-monitor/npm-investigation05.png)
6. hello förlust, svarstid och hello antalet hopp i varje sökväg kan ses i hello **åtgärd** fönstret. Använd hello rullningslisten tooview hello information på sökvägarna feltillstånd.  Använd hello filter tooselect hello sökvägar med hello ohälsosamt hopp så att hello topologin för endast hello markerad sökvägar ritas. Du kan använda din mus hjul toozoom i eller utanför hello topologisk karta.

   I hello under bilden ser du tydligt hello grundorsaken till hello problemet områden toohello avsnittet hello nätverk genom att titta på hello sökvägar och hopp med röd färg. Klicka på en nod i hello topologisk karta visar hello egenskaper för hello-nod, inklusive hello FQDN och IP-adress. Klicka på ett hopp visar hello IP-adress hello hopp.  
   ![feltillstånd topologi - sökvägen information exempel](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="provide-feedback"></a>Ge feedback

- **UserVoice** -du kan publicera dina idéer för Network Performance Monitor funktioner som du vill oss toowork på. Besök vår [UserVoice sidan](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring).
- **Ansluta till vår kommittén** -vi alltid är intresserad av att nya kunder ansluta till vår kommittén. Som en del av det kan du få en förhandsåtkomst toonew funktioner och hjälp oss att förbättra Network Performance Monitor. Om du är intresserad av att koppla kan fylla i det här [snabb undersökning](https://aka.ms/npmcohort).

## <a name="next-steps"></a>Nästa steg
* [Söka i loggar](log-analytics-log-searches.md) tooview detaljerad dataposter prestanda för nätverket.
