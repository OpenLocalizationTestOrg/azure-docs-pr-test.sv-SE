---
title: "aaaTroubleshooting och övervakning av SAP HANA i Azure (stora instanser) | Microsoft Docs"
description: "Felsöka och övervaka SAP HANA på ett Azure (stora instanser)."
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
ms.openlocfilehash: 1f1cd35820e227fd99af495431cd4b826aa53600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-and-monitor-sap-hana-large-instances-on-azure"></a>Hur tootroubleshoot och övervaka SAP HANA (stora instanser) på Azure


## <a name="monitoring-in-sap-hana-on-azure-large-instances"></a>Övervakning av SAP HANA i Azure (stora instanser)

SAP HANA i Azure (stora instanser) skiljer sig inte från andra IaaS-distribution, behöver du toomonitor vad hello OS och hello programmet är gör och hur de använder hello följande resurser:

- Processor
- Minne
- Nätverkets bandbredd
- Diskutrymme

Som måste med Azure Virtual Machines toofigure ut om hello resursklasser med namnet ovan är tillräckligt eller om de få slut. Det finns mer information på varje hello olika klasser:

**CPU-resursanvändningen:** hello som SAP som definierats för vissa arbetsbelastningar mot HANA är tvingande toomake till att det ska vara tillräckligt med CPU resurser tillgängliga toowork via hello data som lagras i minnet. Det kan dock finnas fall där HANA förbrukar en stor mängd CPU köra frågor på grund av toomissing index eller liknande problem. Det innebär att du ska övervaka CPU-resursanvändningen hello HANA stora instans enhet samt CPU-resurser som används av hello specifika HANA tjänster.

**Minnesförbrukning:** är viktiga toomonitor från inom HANA samt utanför HANA på hello enheten. Övervaka hur hello data förbrukar allokerat minne i ordning toostay inom hello krävs storleksanpassa riktlinjer för SAP HANA inom HANA. Du bör också toomonitor minnesförbrukning på hello stora instans nivå toomake till att ytterligare installerade icke-HANA programvara inte förbrukar för mycket minne och därför konkurrerar med HANA för minne.

**Nätverkets bandbredd:** hello Azure VNet gateway har begränsad bandbredd på data flyttas till hello Azure VNet, så är det bra toomonitor hello data som tas emot av alla hello virtuella Azure-datorer inom ett VNet toofigure reda på hur nära du toohello gränserna för hello Azure gateway-SKU som du har valt. Det gör på hello HANA stora instans enheten meningsfullt toomonitor inkommande och utgående nätverkstrafik samt och tookeep spåra hello volymer som hanteras med tiden.

**Diskutrymme:** förbrukningen av diskutrymme vanligtvis ökar med tiden. Det finns många orsaker, men de flesta av alla är: datavolym ökar, körning av säkerhetskopiering av transaktionsloggar, lagra spårningsfiler och utföra storage snapshots. Därför är det viktigt toomonitor diskutrymmesanvändning och hantera hello diskutrymme som är associerade med hello HANA stora instans enhet.

## <a name="monitoring-and-troubleshooting-from-hana-side"></a>Övervakning och felsökning från HANA sida

I ordning tooeffectively analysera problem relaterade tooSAP HANA i Azure (stora instanser), är det användbart toonarrow ned hello orsaken till problemet. SAP har publicerat en stor mängd dokumentationen toohelp du.

Tillämpliga vanliga frågor och svar relaterade tooSAP HANA prestanda finns i följande SAP anteckningar hello:

- [SAP-kommentar #2222200 – vanliga frågor och svar: SAP HANA-nätverk](https://launchpad.support.sap.com/#/notes/2222200)
- [SAP-kommentar #2100040 – vanliga frågor och svar: SAP HANA-processor](https://launchpad.support.sap.com/#/notes/0002100040)
- [SAP-kommentar #199997 – vanliga frågor och svar: SAP HANA-minne](https://launchpad.support.sap.com/#/notes/2177064)
- [SAP-kommentar #200000 – vanliga frågor och svar: SAP HANA prestandaoptimering](https://launchpad.support.sap.com/#/notes/2000000)
- [SAP-kommentar #199930 – vanliga frågor och svar: SAP HANA-i/o analys](https://launchpad.support.sap.com/#/notes/1999930)
- [SAP-kommentar #2177064 – vanliga frågor och svar: SAP HANA-tjänsten startas om och kraschar](https://launchpad.support.sap.com/#/notes/2177064)

**SAP HANA aviseringar**

Som ett första steg loggarna hello aktuella SAP HANA avisering. SAP HANA Studio, gå för**administrationskonsolen: aviseringar: visa: alla aviseringar**. Den här fliken visas alla aviseringar för SAP HANA för specifika värden (fysiskt minne, CPU-användning osv.) som faller utanför hello lägsta och högsta tröskelvärden. Som standard är kontrollerar uppdateras automatiskt var 15: e minut.

![Gå tooAdministration konsolen i SAP HANA Studio: aviseringar: visa: alla aviseringar](./media/troubleshooting-monitoring/image1-show-alerts.png)

**CPU**

För en avisering som aktiveras på grund av tooimproper tröskelvärdet inställningen är en lösning tooreset toohello standardvärde eller ett rimliga tröskelvärde.

![Återställa toohello standardvärde eller ett rimliga tröskelvärde](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

hello följande aviseringar kan tyda på problem med CPU företagsresurser:

- Värd för CPU-användning (varning 5)
- Senaste lagringspunktsåtgärd (varning 28)
- Lagringspunkt varaktighet (varning 54)

Du kan upptäcka hög CPU-förbrukning på din SAP HANA-databas från en av följande hello:

- Aviseringen 5 (värd processoranvändning) aktiveras aktuella eller förfallna CPU-användning
- hello visas CPU-användning på hello översiktsskärm

![Visas CPU-användning på hello översiktsskärm](./media/troubleshooting-monitoring/image3-cpu-usage.png)

hello belastningen diagram kan visa hög CPU-förbrukning eller hög förbrukning i hello senaste:

![hello belastningen diagram kan visa hög CPU-förbrukning eller hög förbrukning i hello senaste](./media/troubleshooting-monitoring/image4-load-graph.png)

En avisering aktiveras på grund av toohigh CPU-användning kan orsakas av olika orsaker, inklusive men inte begränsat till: körning av vissa transaktioner, datainläsning, hängande för jobb, långvariga SQL-satser och felaktiga frågeprestanda (till exempel med BW på HANA kuber).

Se toohello [SAP HANA Felsökning: relaterade gör att CPU och lösningar](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) för detaljerad felsökning.

**Operativsystem**

En av de viktigaste hello kontrollerar för SAP HANA på Linux är toomake till att Transparent stora sidor är inaktiverat, se [SAP Obs #2131662 – Transparent stora sidor (THP) på SAP HANA-servrar](https://launchpad.support.sap.com/#/notes/2131662).

- Du kan kontrollera om Transparent stora sidor aktiveras via hello följande Linux-kommando: **cat /sys/kernel/mm/transparent\_hugepage aktiveras**
- Om _alltid_ omges inom hakparenteser enligt nedan, betyder det att hello Transparent stora sidor är aktiverad: [alltid] madvise aldrig; om _aldrig_ omges inom hakparenteser enligt nedan, innebär det att hello Transparent Stora sidor är inaktiverade: madvise alltid [aldrig]

följande Linux-kommando hello ska returnera ingenting: **rpm - qa | grep kommandot ulimit.** Om det verkar som _kommandot ulimit_ är installerad avinstallerar du den omedelbart.

**Minne**

Du kan se detta hello belopp minne som allokerats av hello SAP HANA-databasen är högre än förväntat. hello följande aviseringar tyda på problem med hög minnesanvändning:

- Värden fysisk minnesanvändning (varning 1)
- Minnesanvändning för namnserver (varning 12)
- Totalt antal minnesanvändningen för kolumnen Store tabeller (varning 40)
- Minnesanvändning av tjänster (avisering 43)
- Mängden minne på huvudsakliga lagring av kolumn Store tabeller (varning 45)
- Runtime dumpfiler (varning 46)

Se toohello [SAP HANA Felsökning: minnesproblem](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) för detaljerad felsökning.

**Nätverk**

Se för[SAP Obs #2081065 – felsökning av nätverk för SAP HANA](https://launchpad.support.sap.com/#/notes/2081065) och utföra felsökning i den här SAP-kommentar hello-nätverk.

1. Analysera tidsfördröjningen mellan servern och klienten.
  A. Kör SQL-skript för hello [ _HANA\_nätverk\_klienter_](https://launchpad.support.sap.com/#/notes/1969700)_._
  
2. Analysera dessa kommunikation.
  A. Kör SQL-skript [ _HANA\_nätverk\_Services_](https://launchpad.support.sap.com/#/notes/1969700)_._

3. Kör Linux-kommando **ifconfig** (hello utdata visar om paketförluster sker).
4. Kör Linux-kommando **tcpdump**.

Du kan också använda hello öppen källkod [IPERF](https://iperf.fr/) verktyget (eller liknande) toomeasure verkliga programmet nätverkets prestanda.

Se toohello [SAP HANA Felsökning: nätverk prestanda och anslutningsproblem](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) för detaljerad felsökning.

**Storage**

En ur slutanvändaren ett program (eller hello system som helhet) körs långsamt, svarar inte eller kan även verkar vara toohang om det finns problem med i/o-prestanda. I hello **volymer** fliken i SAP HANA Studio kan du se hello anslutna volymer och vilka volymer som används av varje tjänst.

![Hello volymer på fliken i SAP HANA Studio finns hello anslutna volymer och vilka volymer som används av varje tjänst](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

Anslutna volymer i hello nedre delen av du hittar information om hello-skärmen hello volymer, till exempel filer och i/o-statistik.

![Anslutna volymer i hello nedre delen av du hittar information om hello-skärmen hello volymer, till exempel filer och i/o-statistik](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

Se toohello [SAP HANA Felsökning: i/o relaterade rotorsaker och lösningar](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) och [SAP HANA Felsökning: Disk relaterade rotorsaker och lösningar](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) för detaljerad felsökning.

**Verktyg för Nätverksdiagnostik**

Utföra en SAP HANA hälsokontroll via HANA\_Configuration\_Minichecks. Det här verktyget returnerar allvarliga tekniska problem som bör har redan aktiverats som aviseringar i SAP HANA Studio.

Se för[SAP Obs #1969700 – SQL-instruktionen samlingen för SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) och hämta hello SQL Statements.zip filen bifogade toothat anteckning. Lagra den här ZIP-filen på hello lokala hårddisken.

I SAP HANA Studio på hello **Systeminformation** , högerklickar på i hello **namn** kolumn och markera **SQL importuttryck**.

![Högerklicka i hello namnkolumnen i SAP HANA Studio hello Systeminformation på fliken och markera Importera SQL-instruktioner](./media/troubleshooting-monitoring/image7-import-statements-a.png)

Välj hello SQL Statements.zip filen lagras lokalt och en mapp med hello motsvarande SQL-uttryck som ska importeras. Nu hello många olika diagnostiska kontroller kan köras med de här SQL-instruktioner.

Till exempel tootest bandbreddskraven för SAP HANA System replikering, högerklicka på hello **bandbredd** instruktionen under **replikering: bandbredd** och välj **öppna** i SQL-konsolen.

hello fullständig SQL-sats öppnas indataparametrar (ändring avsnittet) vilket gör att toobe ändrats och därmed köras.

![hello fullständig SQL-sats öppnar indataparametrar (ändring avsnittet) vilket gör att toobe ändras och sedan köras](./media/troubleshooting-monitoring/image8-import-statements-b.png)

Ett annat exempel är Högerklicka på hello instruktioner under **replikering: översikt över**. Välj **Execute** hello snabbmenyn:

![Ett annat exempel är Högerklicka på hello instruktioner under replikeringen: översikt. Välj Kör hello snabbmenyn](./media/troubleshooting-monitoring/image9-import-statements-c.png)

Detta resulterar i information som hjälper med felsökning:

![Detta leder till information som hjälper med felsökning](./media/troubleshooting-monitoring/image10-import-statements-d.png)

Hello samma för HANA\_Configuration\_Minichecks och Sök efter alla _X_ märken i hello _C_ (kritisk)-kolumn.

Exempel utdata:

**HANA\_Configuration\_MiniChecks\_Rev102.01 + 1** för allmän SAP HANA kontrollerar.

![HANA\_Configuration\_MiniChecks\_Rev102.01 + 1 för allmänna SAP HANA-kontroller](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

**HANA\_Services\_översikt** en översikt över vad SAP HANA tjänster som körs.

![HANA\_Services\_översikt för en översikt över vad SAP HANA tjänster som körs](./media/troubleshooting-monitoring/image12-services-overview.png)

**HANA\_Services\_statistik** för SAP HANA tjänstinformation (CPU, minne, osv.).

![HANA\_Services\_statistik för SAP HANA tjänstinformation ](./media/troubleshooting-monitoring/image13-services-statistics.png)

**HANA\_Configuration\_översikt\_Rev110 +** allmän information om hello SAP HANA-instans.

![HANA\_Configuration\_översikt\_Rev110 + allmän information om hello SAP HANA-instans](./media/troubleshooting-monitoring/image14-configuration-overview.png)

**HANA\_Configuration\_parametrar\_Rev70 +** toocheck SAP HANA-parametrar.

![HANA\_Configuration\_parametrar\_Rev70 + toocheck SAP HANA-parametrar](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

