---
title: "aaaOverview och arkitektur för SAP HANA i Azure (stora instanser) | Microsoft Docs"
description: "Översikt över hur arkitekturen tooDeploy SAP HANA i Azure (stora instanser)."
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
ms.openlocfilehash: e3ee6864af37ac322635eaef43e3c20101e3a769
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>Översikt över SAP HANA (stora instanser) och arkitektur på Azure

## <a name="what-is-sap-hana-on-azure-large-instances"></a>Vad är SAP HANA i Azure (stora instanser)?

SAP HANA i Azure (stora instans) är en unik lösning tooAzure. Dessutom tooproviding Azure virtuella datorer i hello syftet att distribuera och köra SAP HANA Azure erbjuder du hello möjligheten toorun och distribuera SAP HANA på bare metal-servrar som är dedikerad tooyou som en kund. hello SAP HANA i Azure (stora instanser) lösningen bygger på icke-delade/server host bare metal-maskinvara som är tilldelad tooyou som en kund. hello servermaskinvaran är inbäddad i större stämplar som innehåller beräknings-server, nätverk och infrastruktur. Vilket, som en kombination är HANA TDI certifierade. hello tjänsterbjudande för SAP HANA i Azure (stora instanser) tillhandahåller olika SKU: er för annan server eller storlekar som börjar med enheter som har 72 processorer och 768 GB minne toounits som har 960 processorer och 20 TB minne.

hello kunden isolering inom hello infrastruktur stämpel utförs i klienter, som ser ut som i detalj:

- Nätverk: Isolering av kunder i infrastrukturen stapel via virtuella nätverk per kund tilldelats klient. En klient har tilldelats tooa kund. En kund kan ha flera innehavare. hello nätverksisolering av innehavare som förhindrar nätverkskommunikation mellan klienter i hello infrastruktur stämpel nivå. Även om klienter hör toohello samma kund.
- Lagringskomponenter: isolering via lagring virtuella datorer som har lagringsvolymer tilldelade tooit. Lagringsvolymer kan tilldelas tooone lagring virtuell dator. En virtuell dator för lagring har tilldelats exklusivt tooone enskild klient i hello SAP HANA TDI certifierade infrastruktur stacken. Därför kan lagringsvolymer som tilldelats tooa lagring virtuella datorn nås i en specifik och relaterade klienten endast. Och är inte synligt mellan hello olika distribuerade klienter.
- Server eller en värd: en server eller en värd enhet delas inte mellan kunder eller klienter. En server eller värd distribueras tooa kund, är en atomisk bare metal-beräknings-enhet som är tilldelad tooone enda innehavare. **Inte** partitionering av maskinvara eller soft-partitionering används som kan resultera i att du som kund kan dela en värd eller en server med en annan kund. Lagringsvolymer som tilldelas toohello lagring virtuell dator av hello specifika innehavaren är monterade toosuch en server. En klient kan ha en toomany server enheter av olika SKU: er som tilldelats exklusivt.
- Många olika klienter är inom en SAP HANA i Azure (stora instans) infrastruktur stämpel distribueras och isolerade mot varandra via hello klient begrepp på nätverk, lagring och beräkning nivå. 


Dessa bare metal-server-enheter är bara stöds toorun SAP HANA. hello SAP programnivå eller arbetsbelastning i mitten-ware layer körs i Microsoft Azure Virtual Machines. hello infrastruktur stämplarna körs hello SAP HANA i Azure (stora instans) är enheter anslutna toohello Azure Network backbones, så som låg latens anslutning mellan Azure virtuella datorer och SAP HANA på Azure (stora instans)-enheter har angetts.

Det här dokumentet är en av fem dokument, som omfattar hello-avsnittet för SAP HANA i Azure (stora instans). I det här dokumentet går vi genom hello grundläggande arkitektur, ansvar, tjänster och på en hög nivå via funktionerna i hello-lösningen. För de flesta hello områden som nätverk och anslutning, hello andra fyra dokument täcker information och öka detaljnivån listrutor. hello-dokumentationen för SAP HANA i Azure (stora instans) omfattar inte aspekter av SAP NetWeaver installation eller distribution av SAP NetWeaver i virtuella Azure-datorer. Det här avsnittet beskrivs i separata dokumentationen i hello samma dokumentationen behållare. 


hello fem delar av den här guiden omfattar hello följande avsnitt:

- [Översikt över SAP HANA (stora instans) och arkitektur på Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Infrastruktur för SAP HANA (stora instanser) och anslutningar på Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Hur tooinstall och konfigurera SAP HANA (stora instanser) på Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (stora instanser) med hög tillgänglighet och katastrofåterställning i Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Felsökning för SAP HANA (stora instanser) och övervakning på Azure](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="definitions"></a>Definitioner

Flera gemensamma definitioner används ofta i hello arkitektur och teknisk distribution. Obs hello följande villkor och deras innebörd:

- **IaaS:** infrastruktur som en tjänst
- **PaaS:** plattform som en tjänst
- **SaaS:** programvara som en tjänst
- **SAP komponent:** ett enskilt SAP-program, till exempel ECC BW, lösning Manager eller EP. SAP-komponenter kan baseras på traditionell ABAP eller Java-teknik eller icke-NetWeaver baserat program, till exempel Business-objekt.
- **SAP-miljö:** en eller flera komponenter för SAP logiskt grupperade tooperform en business-funktion, till exempel utveckling, QAS, träning, DR eller produktion.
- **SAP liggande:** refererar toohello hela SAP tillgångar i din IT-miljön. hello SAP liggande innehåller alla produktions- och icke-produktionsmiljöer.
- **SAP System:** hello kombination av DBMS lager- och programnivå en SAP ERP-utvecklingssystemet SAP BW testsystemet, SAP CRM produktionssystem, osv. Azure-distributioner stöder inte mellan dessa två lager mellan lokala och Azure. : Ett SAP-system är antingen distribuerade lokalt eller den har distribuerats i Azure. Du kan dock distribuera hello olika system för en SAP liggande i Azure eller lokalt. Du kan till exempel distribuera hello SAP CRM-utveckling och testning system i Azure, vid distribution av hello SAP CRM produktion system lokalt. Den är avsedd att du är värd hello SAP programnivå för SAP-system i virtuella Azure-datorer och hello relaterade SAP HANA-instans på en enhet i hello HANA stora instans stämpel för SAP HANA i Azure (stora instanser).
- **Stora instans stämpel:** maskinvara infrastruktur-stacken, som är SAP HANA TDI certifierad och dedikerade toorun SAP HANA-instanser i Azure.
- **SAP HANA i Azure (stora instanser):** officiellt namn för hello erbjudande på Azure toorun HANA instanser i på SAP HANA TDI certifierad maskinvara som distribueras i stora instans tidsstämplar i olika Azure-regioner. hello relaterade termen **HANA stora instans** kort för SAP HANA i Azure (stora instanser) och är ofta används den här tekniska distributionsguiden.
- **Anslutningar mellan lokala:** beskriver ett scenario där virtuella datorer som är distribuerade tooan Azure-prenumeration som har plats-till-plats, flera platser eller ExpressRoute-anslutning mellan hello lokalt datacenter(s) och Azure. Gemensamma Azure-dokumentationen dessa typer av distributioner beskrivs också som mellan lokala scenarier. hello orsaken till hello anslutning är tooextend lokala domänerna lokala Active Directory/OpenLDAP och lokala DNS i Azure. hello lokalt liggande är utökade toohello hello Azure-prenumeration(er) Azure tillgångar. Med det här tillägget, kan hello virtuella datorer vara en del av hello lokala domän. Domänanvändare av hello lokal domän kan komma åt hello servrar och kan köra services på de virtuella datorer (till exempel DBMS services). Det är möjligt kommunikation och namnmatchning mellan virtuella datorer som distribueras på lokalt och distribuerade virtuella Azure-datorer. Exempel är hello typiskt scenario i vilka de flesta SAP tillgångar distribueras. Se hello stödlinjer av [planering och design för VPN-Gateway](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) och [skapa ett VNet med en plats-till-plats-anslutning med hello Azure-portalen](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mer detaljerad information.
- **Klient:** en kund som distribuerats i HANA stora instanser stämpel hämtar isolerade till en ”klient”. En klient är separat i hello nätverk, lagring och beräkning lagret från andra klienter. Så att lagring och beräkning enheter tilldelade toohello olika klienter inte kan se varandra eller kommunicera med varandra på hello HANA stora instans tidsstämpel nivå. En kund kan välja toohave distributioner till olika klienter. Det finns även sedan ingen kommunikation mellan klienter i hello HANA stora stämpel instansnivå.

Det finns ett antal ytterligare resurser som har publicerats på hello-avsnittet för att distribuera SAP arbetsbelastningen för Microsoft Azures offentliga moln. Vi rekommenderar att alla planerings- och köra en SAP HANA-distribution i Azure är erfarna och medveten om huvudobjekt Hej av Azure IaaS och hello distribution av SAP arbetsbelastningar på Azure IaaS. hello följande resurser ger mer information och bör refereras innan du fortsätter:


- [Med hjälp av SAP-lösningar på Microsoft Azure-datorer](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="certification"></a>Certifiering

Utöver hello NetWeaver certifiering, kräver SAP en särskild certifikatutfärdare för SAP HANA toosupport SAP HANA på vissa infrastrukturer, till exempel Azure IaaS.

hello core SAP-kommentar på NetWeaver och tooa grad SAP HANA-certifiering, är [SAP Obs #1928533 – SAP-program i Azure: produkter och Virtuella Azure-typer](https://launchpad.support.sap.com/#/notes/1928533).

Detta [SAP Obs #2316233 - SAP HANA i Microsoft Azure (stora instanser)](https://launchpad.support.sap.com/#/notes/2316233/E) är också viktig. Den omfattar hello-lösningen som beskrivs i den här guiden. Du kan dessutom stöds toorun SAP HANA i hello GS5 VM typen av Azure. [Information om det här fallet har publicerats på hello SAP-webbplatsen](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).

hello SAP HANA i Azure (stora instanser)-lösningen som anges tooin SAP Obs #2316233 ger Microsoft och SAP-kunder hello möjlighet toodeploy stora SAP Business Suite, SAP Business Warehouse (BW), S/4 HANA, BW/4HANA eller andra SAP HANA-arbetsbelastningar i Azure. hello lösning baseras på hello SAP HANA-certifierade stämpel dedikerad maskinvara ([SAP HANA skräddarsydda Datacenter integrering – TDI](https://scn.sap.com/docs/DOC-63140)). Körs som en SAP HANA TDI konfigurerade lösningen ger du hello säkert sätt att veta att alla SAP HANA-baserade program (inklusive SAP Business Suite på SAP HANA, SAP Business Warehouse (BW) för SAP HANA, S4/HANA och BW4/HANA) ska toowork på hello infrastruktur för maskinvara.

Jämfört med toorunning SAP HANA i Azure Virtual Machines lösningen har en – den innehåller för mycket större volymer minne. Om du vill tooenable den här lösningen, finns det vissa viktiga aspekter toounderstand:

- hello SAP programnivå och icke-SAP-program körs i Azure Virtual Machines (virtuella datorer) som finns i hello vanliga Azure maskinvara stämplar.
- Kunden lokal infrastruktur, Datacenter och distribution av program är anslutet toohello Microsoft Azure cloud platform via Azure ExpressRoute (rekommenderas) eller virtuella privata nätverk (VPN). Active Directory (AD) och DNS också har utökats till Azure.
- hello SAP HANA-databasinstans för HANA arbetsbelastning körs på SAP HANA i Azure (stora instanser). hello stora instans stämpel är ansluten till Azure-nätverk så att program som körs i virtuella Azure-datorer kan interagera med hello HANA-instans som körs i HANA stora instanser.
- Maskinvara för SAP HANA i Azure (stora instanser) är dedikerad maskinvara som anges i en infrastruktur som en tjänst (IaaS) med SUSE Linux Enterprise Server eller Red Hat Enterprise Linux förinstallerat. Med Azure Virtual Machines, ytterligare uppdateringar och underhåll toohello operativsystem som är ditt ansvar.
- Installation av HANA eller eventuella ytterligare komponenter nödvändiga toorun SAP HANA på enheterna för stora HANA instanser är ditt ansvar, som är alla respektive pågående åtgärder och myndigheter SAP HANA på Azure.
- Dessutom toohello lösningar som beskrivs här, du kan installera andra komponenter i din Azure-prenumeration som ansluter tooSAP HANA i Azure (stora instanser).  Till exempel komponenter som aktiverar kommunikation med och/eller direkt toohello SAP HANA-databas (hopp servrar, RDP-servrar, SAP HANA Studio SAP datatjänster för SAP BI scenarier eller nätverk övervakar lösningar).
- Som Azure erbjuder HANA stora instanser stöder funktioner för hög tillgänglighet och katastrofåterställning.

## <a name="architecture"></a>Arkitektur

Vid en hög nivå har hello SAP HANA i Azure (stora instanser)-lösningen hello SAP programnivå som finns i Azure Virtual Machines och hello databasen layer på SAP TDI konfigurerad maskinvara finns i en stor instans stämpel i hello samma Azure-Region som är ansluten tooAzure IaaS.

> [!NOTE]
> Du behöver toodeploy hello SAP programnivå hello samma Azure-Region som hello SAP DBMS lager. Den här regeln är väl dokumenterat i publicerad information om SAP arbetsbelastningen på Azure. 

hello ger övergripande arkitekturen för SAP HANA i Azure (stora instanser) en SAP TDI certifierad maskinvarukonfiguration (icke-virtualiserade, bare metal, högpresterande server för hello SAP HANA-databas), och hello möjlighet och flexibilitet i Azure tooscale resurser för hello SAP application layer toomeet dina behov.

![Översikt över arkitekturen för SAP HANA i Azure (stora instanser)](./media/hana-overview-architecture/image1-architecture.png)

hello-arkitektur som visas är indelat i tre delar:

- **Höger:** en lokal infrastruktur som kör olika program i datacenter med slutanvändarna att öppna LOB-program (till exempel SAP). Vi rekommenderar detta lokala infrastruktur ansluts sedan tooAzure med Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Center:** visar Azure IaaS och i så fall använder virtuella Azure-datorer toohost SAP eller andra program som använder SAP HANA som ett DBMS-system. Mindre HANA instanser med funktionen med hello minne Azure virtuella datorer distribueras på virtuella Azure-datorer tillsammans med deras programnivå. Lär dig mer om [virtuella datorer](https://azure.microsoft.com/services/virtual-machines/).
<br />Azure-nätverk är används toogroup SAP-system tillsammans med andra program i Azure virtuella nätverk (Vnet). Dessa Vnet ansluta tooon lokala system samt tooSAP HANA i Azure (stora instanser).
<br />SAP NetWeaver program och databaser som stöds toorun i Microsoft Azure finns [SAP stöd Obs #1928533 – SAP-program i Azure: produkter och Virtuella Azure-typer](https://launchpad.support.sap.com/#/notes/1928533). Granska följande dokumentation om hur du distribuerar SAP-lösningar i Azure:

  -  [Med hjälp av SAP på virtuella Windows-datorer (VM)](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Med hjälp av SAP-lösningar på Microsoft Azure-datorer](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Vänster:** visar hello SAP HANA TDI certifierad maskinvara i hello Azure stora instans stämpel. hello HANA stora instans enheterna är anslutna toohello Azure Vnet för din prenumeration med hjälp av hello samma teknik som hello anslutningen från lokal till Azure.

hello Azure stora instans stämpel själva kombinerar hello följande komponenter:

- **Datorupplevelse:** servrar som är baserade på Intel Xeon E7-8890v3 eller Intel Xeon E7-8890v4 processorer som funktioner hello nödvändiga databehandling och SAP HANA-certifierad.
- **Nätverk:** en enhetlig snabb nätverksinfrastruktur som sammanbinder hello bearbetning, lagring och LAN-komponenter.
- **Lagring:** lagringsinfrastruktur som nås via en enhetlig nätverksinfrastruktur. Viss lagringskapacitet som är beroende på hello specifika SAP HANA på Azure (stora instanser) konfiguration som distribueras (mer lagringskapacitet är en extra månatliga kostnad).

I hello flera innehavare infrastruktur av hello stora instans stämpeln distribueras kunder som isolerade klienter. Vid distribution av hello-klient måste tooname en Azure-prenumeration i Azure-registrering. Den här pågående toobe hello Azure-prenumeration, hello HANA stora instanser kommer toobe debiteras mot. Dessa klienter har en 1:1-relation toohello Azure-prenumeration. Nätverket bör det är möjligt tooaccess en HANA stora instans enhet distribueras på en klient i en Azure-Region från olika Azure-Vnet som tillhör toodifferent Azure-prenumerationer. Även om de Azure-prenumerationerna måste toobelong toohello samma Azure-registrering. 

SAP HANA i Azure (stora instanser) erbjuds som virtuella Azure-datorer i Azure-regioner. Du kan välja tooopt i ordning toooffer katastrofåterställning funktionerna. Olika stora instans stämplarna inom en geo politiska region är anslutna tooeach andra. Till exempel är HANA stora instans tidsstämplar i West för oss och vi Öst anslutna via en dedikerad nätverkslänk för hello syftet DR-replikering. 

Precis som du kan välja mellan VM av olika typer med Azure Virtual Machines, kan du välja mellan olika SKU: er av HANA stora instanser som är skräddarsydda för arbetsbelastning för olika typer av SAP HANA. SAP gäller minne tooprocessor socket förhållandet för olika arbetsbelastningar baserat på hello Intel processor generationer – det finns fyra olika SKU typer som erbjuds:

Från och med juli 2017 finns SAP HANA i Azure (stora instanser) i flera konfigurationer i hello Azure-regioner för oss Väst och oss Öst, östra, Australien, sydost, västra Europa och Norra Europa:

| SAP-lösning | Processor | Minne | Lagring | Tillgänglighet |
| --- | --- | --- | --- | --- |
| Optimerad för OLAP-: SAP BW BW/4HANA<br /> eller SAP HANA för allmänna OLAP-arbetsbelastning | SAP HANA på Azure S72<br /> – 2 x Intel Xeon®-Processor E7 8890 v3<br /> 36 CPU-kärnor och 72 CPU-trådar |  768 GB |  3 TB | Tillgänglig |
| --- | SAP HANA på Azure S144<br /> – 4 x Intel Xeon®-Processor E7 8890 v3<br /> 72 CPU-kärnor och 144 CPU-trådar |  1,5 TB |  6 TB | Erbjuds inte längre |
| --- | SAP HANA på Azure S192<br /> – 4 x Intel Xeon®-Processor E7 8890 v4<br /> 96 CPU-kärnor och 192 CPU-trådar |  2.0 TB |  8 TB | Tillgänglig |
| --- | SAP HANA på Azure S384<br /> – 8 x Intel Xeon®-Processor E7 8890 v4<br /> 192 CPU-kärnor och 384 CPU-trådar |  4.0 TB |  16 TB | Redo tooOrder |
| Optimerad för OLTP: SAP Business Suite<br /> på SAP HANA eller S/4HANA (OLTP)<br /> allmän OLTP | SAP HANA på Azure S72m<br /> – 2 x Intel Xeon®-Processor E7 8890 v3<br /> 36 CPU-kärnor och 72 CPU-trådar |  1,5 TB |  6 TB | Tillgänglig |
|---| SAP HANA på Azure S144m<br /> – 4 x Intel Xeon®-Processor E7 8890 v3<br /> 72 CPU-kärnor och 144 CPU-trådar |  3.0 TB |  12 TB | Erbjuds inte längre |
|---| SAP HANA på Azure S192m<br /> – 4 x Intel Xeon®-Processor E7 8890 v4<br /> 96 CPU-kärnor och 192 CPU-trådar  |  4.0 TB |  16 TB | Tillgänglig |
|---| SAP HANA på Azure S384m<br /> – 8 x Intel Xeon®-Processor E7 8890 v4<br /> 192 CPU-kärnor och 384 CPU-trådar |  6.0 TB |  18 TB | Redo tooOrder |
|---| SAP HANA på Azure S384xm<br /> – 8 x Intel Xeon®-Processor E7 8890 v4<br /> 192 CPU-kärnor och 384 CPU-trådar |  8.0 TB |  22 TB |  Redo tooOrder |
|---| SAP HANA på Azure S576<br /> – 12 x Intel Xeon®-Processor E7 8890 v4<br /> 288 CPU-kärnor och 576 CPU-trådar |  12,0 TB |  28 TB | Redo tooOrder |
|---| SAP HANA på Azure S768<br /> – 16 x Intel Xeon®-Processor E7 8890 v4<br /> 384 CPU-kärnor och 768 CPU-trådar |  16,0 TB |  36 TB | Redo tooOrder |
|---| SAP HANA på Azure S960<br /> – 20 x Intel Xeon®-Processor E7 8890 v4<br /> 480 CPU-kärnor och 960 CPU-trådar |  20,0 TB |  46 TB | Redo tooOrder |

- CPU-kärnor = summan av icke-hypertrådade CPU-kärnor av hello hello processorer hello server enhet.
- CPU-trådar = summan av beräknings-trådar som tillhandahålls av hyper-threaded processorkärnor av hello hello processorer hello server enhet. Alla enheter konfigureras som standard toouse flertrådsteknik.


hello olika konfigurationer ovan som är tillgänglig eller 'erbjuds inte längre' refererar till [SAP stöd Obs #2316233 – SAP HANA i Microsoft Azure (stora instanser)](https://launchpad.support.sap.com/#/notes/2316233/E). hello-konfigurationer som har markerats som ”klara tooOrder' kommer att hitta träder i hello SAP-kommentar snart. Men kan dessa instans SKU: er beställas redan för hello sex olika Azure-regioner hello HANA stora instans-tjänsten är tillgänglig.

hello specifika konfigurationer som valts är beroende av arbetsbelastning, CPU-resurser och önskade minne. Det är möjligt för OLTP-arbetsbelastning tooleverage hello SKU: er som är optimerade för OLAP-arbetsbelastning. 

hello maskinvara för alla hello erbjudanden är SAP HANA TDI certifierade. Men skilja vi mellan två olika klasser av maskinvara, vilket delar upp hello SKU: er till:

- S72, S72m, S144, S144m, S192 och S192m som vi refererar tooas hello 'Type I klassen' av SKU: er.
- S384, S384m, S384xm, S576, S768 och S960 som vi refererar tooas hello typ II klass av SKU: er.

Det är viktigt toonote som en fullständig HANA stora instans stämpel uteslutande inte har allokerats för en kund &#39; s användning. Detta gäller toohello rack av beräkning och lagring resurser som är anslutna via en nätverksinfrastruktur samt distribuerade i Azure. HANA stora instanser infrastruktur som Azure, distribuerar annan kund &quot;hyresgäster&quot; som är isolerade från varandra i hello följande tre nivåer:

- Nätverk: Isolering av via virtuella nätverk i hello HANA stora instans stämpeln.
- Lagring: Isolering via lagring virtuella datorer som har lagringsvolymer tilldelas och isolera lagringsvolymer mellan klienter behålls.
- Beräkning: Dedikerade tilldelning av server enheter tooa enda innehavare. Nej hårddisken eller soft partitionering av server-enheter. Inga delar av en enskild server eller en värd enhet mellan klienter behålls. 

Därför är hello distributioner av HANA stora instanser enheter mellan olika klienter inte synliga tooeach andra. Inte heller kan HANA stora instans enheter som distribueras i olika klienter kommunicera direkt med varandra på hello HANA stora stämpel instansnivå. Endast HANA stora instans enheter inom en klient kan kommunicera tooeach andra på hello HANA stora stämpel instansnivå.
En distribuerad klient i hello stora instans stämpel tilldelas fakturering klokt tooone Azure-prenumeration. Dock hello nätverket wise den kan nås från Azure Vnet för andra Azure-prenumerationer inom samma Azure-registrering. Om du distribuerar med en annan Azure-prenumeration i hello samma Azure-region, du kan också välja tooask för en åtskilda HANA stora instans-klient.

Det finns betydande skillnader mellan kör SAP HANA HANA stora instanser och SAP HANA körs på Azure virtuella datorer som distribueras i Azure:

- Det finns ingen virtualiseringsskiktet för SAP HANA i Azure (stora instanser). Du får hello prestanda hello underliggande bare metal-maskinvara.
- Till skillnad från Azure är hello SAP HANA på Azure (stora instanser)-server dedikerade tooa viss kund. Det går inte att en server-enhet eller värden är svårt eller soft-partitionerad. Därför används en HANA stora instans-enhet som tilldelats som en klient för hela tooa och med den tooyou som en kund. En omstart eller avstängning av servern hello leder inte automatiskt toohello operativsystem och SAP HANA som distribueras på en annan server. (För typ I klassen SKU: er, hello enda undantaget är om problem kan uppstå i en server och Omdistributionen måste utföras på en annan server toobe.)
- Till skillnad från Azure, där värdtyper för processor är markerad för hello bästa pris och prestanda är hello processor typer som är valt för SAP HANA i Azure (stora instanser) hello högsta prestanda av hello Intel E7v3 och E7v4 processor raden.


### <a name="running-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Kör flera SAP HANA-instanser i en HANA stora instans-enhet
Det är möjligt toohost mer än en aktiv SAP HANA-instans på hello HANA stora instans enheter. I ordning toostill innehåller hello funktioner för ögonblicksbilder för lagring och katastrofåterställning, en sådan konfiguration kräver en volym som angetts per instans. Från och med nu hello HANA stora instans enheter kan delas upp på följande sätt:

- S72, S72m, S144, S192: Startar enheten i steg om 256 GB med 256 GB hello minsta. Inkrement som 256 GB, 512 GB och så vidare kan vara kombinerade toohello maximalt minne för hello av hello enhet.
- S144m och S192m: I steg om 256 GB med 512 GB hello minsta enheten. Inkrement som 512 GB, 768 GB och så vidare kan vara kombinerade toohello maximalt minne för hello av hello enhet.
- Skriv II klass: I steg om 512 GB med hello minsta från enheten på 2 TB. Inkrement som 512 GB, 1 TB, 1,5 TB och så vidare kan vara kombinerade toohello maximalt minne för hello av hello enhet.

Några exempel på flera SAP HANA-instanser körs kan se ut så:

| SKU | Minnesstorlek | Lagringsstorlek | Storlekar med flera databaser |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | 1 x 768 GB HANA instans<br /> eller 1 x 512 GB instans + 1 x 256 GB instans<br /> eller 3 x 256 GB instanser | 
| S72m | 768 GB | 3 TB | 3x512GB HANA instanser<br />eller 1 x 512 GB instans + 1 x 1 TB instans<br />eller 6 x 256 GB instanser<br />eller 1x1.5 TB instans | 
| S192m | 4 TB | 16 TB | 8 x 512 GB instanser<br />eller 4 x 1 TB instanser<br />eller 4 x 512 GB instanser + 2 x 1 TB instanser<br />eller 4 x 768 GB instanser + 2 x 512 GB instanser<br />eller 1 × 4 TB-instans |
| S384xm | 8 TB | 22 TB | 4 x 2 TB instanser<br />eller 2 × 4 TB instanser<br />eller instanser 2 x 3 TB + 1 x 2 TB instanser<br />eller instanser 2x2.5 TB + 1 x 3 TB instanser<br />eller 1 x 8 TB instans |


Du får hello idé. Det finns visserligen samt andra ändringar. 


## <a name="operations-model-and-responsibilities"></a>Modellen verksamhet och ansvarsområden

hello-tjänsten som tillhandahålls med SAP HANA i Azure (stora instanser) är justerad mot Azure IaaS-tjänster. Du får en stor instanser av HANA-instans med ett installerat operativsystem som är optimerad för SAP HANA. Med Azure IaaS-VM, de flesta av hello uppgifter med att förstärka hello OS, installera ytterligare programvara som du behöver installera HANA, fungerar hello OS och HANA och uppdaterar hello OS och HANA är ditt ansvar. Microsoft tvingar inte OS-uppdateringar eller HANA uppdateringar på du.

![Ansvaret för SAP HANA i Azure (stora instanser)](./media/hana-overview-architecture/image2-responsibilities.png)

Som du ser i hello diagrammet ovan för är SAP HANA i Azure (stora instanser) en infrastruktur med flera innehavare som ett tjänsterbjudande. Och därför hello divisionen av ansvar ingår i hello OS-infrastruktur gräns för hello de flesta. Microsoft ansvarar för alla aspekter av hello-tjänsten under hello raden i hello operativsystem och du är ansvarig ovan hello raden, inklusive hello operativsystem. Så kan senaste lokalt metoder du kan använda för kompatibilitet, säkerhet, programhantering, bas och hantering av OS fortsätta toobe används. hello system visas som om de finns i nätverket alla angående.

Men är den här tjänsten optimerad för SAP HANA så att det finns områden där dig och Microsoft måste toowork tillsammans toouse hello underliggande funktionerna i infrastrukturen för bästa resultat.

hello följande lista innehåller mer information om varje hello lager och dina ansvarsområden:

**Nätverk:** alla hello interna nätverk för hello stora instans stämpel kör SAP HANA lagringsplatsen åtkomst toohello anslutningen mellan hello instanser (för skalbar och andra funktioner), anslutning toohello liggande och anslutningar tooAzure där hello SAP programnivå finns i Azure Virtual Machines. Den omfattar också WAN-anslutning mellan Azure-Datacenter för katastrofåterställning syften replikering. Alla nätverk partitioneras av hello klient och QOS kopplat.

**Lagring:** hello virtualiserat partitionerade lagringsutrymme för alla volymer som behövs av hello SAP HANA-servrar och för ögonblicksbilder. 

**Servrar:** hello särskilda fysiska servrar toorun hello SAP HANA DBs tilldelade tootenants. Hej servrar hello typ I klassen SKU: er är maskinvara tas ut. Med dessa typer av servrar, hello serverkonfiguration samlas in och underhålls i profiler som kan flyttas från en fysisk maskinvara tooanother fysisk maskinvara. Sådana en (manuella) flyttning av en profil av åtgärder som kan jämföras lite tooAzure tjänsten återställning. hello servrar hello erbjuder typ II klassen SKU: er inte en sådan funktion.

**SDDC:** hello hanteringsprogramvara som används toomanage data datacenter som programvarudefinierade enheter. Det gör att Microsoft toopool resurser för skala, tillgänglighet och prestanda.

**Operativsystem:** hello OS som du väljer (SUSE Linux eller Red Hat Linux) som körs på hello-servrar. hello OS-avbildningar som du angav är hello bilder som tillhandahålls av hello enskilda Linux leverantör tooMicrosoft för hello syftet med SAP HANA. Du är obligatoriska toohave en prenumeration med hello Linux-leverantören för hello viss SAP HANA-optimerad bild. Dina ansvarsområden inkluderar registrera hello bilder med hello OS-leverantör. Från hello punkt övergång av Microsoft ansvarar du också för alla ytterligare korrigering av hello Linux-operativsystem. Korrigering också omfattar ytterligare paket som kan krävas för en lyckad installation för SAP HANA (se Toosap's HANA dokumentationen och SAP anteckningar) och som inte har inkluderats hello specifika Linux leverantör i sina SAP HANA optimerad operativsystemsavbildningar. hello ansvar hello kunden även korrigering av hello OS som är relaterade toomalfunction/optimering av hello OS och dess drivrutiner relaterade toohello specifika servermaskinvara. Eller säkerhet eller funktionella korrigering av hello OS. Kundens ansvar är samt övervaknings- och kapacitetsplanering för:

- CPU-resursanvändningen
- Minnesförbrukning
- Diskutrymme volymer relaterade toofree, IOPS och svarstid
- Volymen nätverkstrafik mellan HANA stora instans och SAP programnivå

hello underliggande infrastruktur av HANA stora instanser tillhandahåller funktioner för säkerhetskopiering och återställning av hello systemvolymen. Med den här funktionen är också ansvarig för.

**Mellanprogram:** hello SAP HANA-instans i första hand. Är du ansvarig för administration, åtgärder och övervakning. Det tillhandahålls funktioner som du kan använda toouse lagring ögonblicksbilder för säkerhetskopiering/återställning och katastrofåterställning. Dessa funktioner tillhandahålls av hello infrastruktur. Dina ansvarsområden är dock också utforma hög tillgänglighet och katastrofåterställning med dessa funktioner, använda dem och övervakning lagring ögonblicksbilder har körts utan problem.

**Data:** dina data hanteras av SAP HANA och andra data, till exempel säkerhetskopieringar delar filer i volymer eller fil. Dina ansvarsområden innehåller övervaka ledigt diskutrymme och hantera hello innehåll på hello volymer och övervaka hello har körts säkerhetskopior av volymer på diskar och lagring ögonblicksbilder. Data replikering tooDR platser har körts är dock Microsoft hello ansvar.

**Program:** hello SAP programinstanser eller vid ej SAP-program, hello programnivå av dessa program. Dina ansvarsområden omfattar distribution, administration, åtgärder och övervakning av programmen relaterade toocapacity planering av CPU-resursanvändningen, minnesförbrukning, Azure Storage-förbrukning och förbrukningen av nätverksbandbredd i Azure Vnet och från Azure Vnet tooSAP HANA i Azure (stora instanser).

**WAN:** hello anslutningar som du har skapat från lokala tooAzure distributioner för arbetsbelastningar. Våra kunder med HANA stora instanser använda Azure ExpressRoute-anslutningen. Den här anslutningen ingår inte i hello SAP HANA i Azure (stora instanser)-lösningen, så att du är ansvarig för hello installationen av den här anslutningen.

**Arkivera:** du kanske föredrar tooarchive kopior av data med egna metoder i storage-konton. Arkivering kräver hantering, kompatibilitet, kostnader och åtgärder. Du ansvarar för att generera Arkiv kopior och säkerhetskopieringar på Azure och lagrar dem på ett sätt som är kompatibla.

Se hello [SLA för SAP HANA i Azure (stora instanser)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Storlek

Storlek för stora HANA-instanser är inte annorlunda än storleken för HANA i allmänhet. För befintliga och distribuerade system, du vill använda toomove från andra RDBMS tooHANA, SAP tillhandahåller ett antal rapporter som körs på din befintliga SAP-system. Om hello databasen har flyttats tooHANA kan de här rapporterna Kontrollera hello data och beräkna minneskrav för hello HANA instans. Läs följande SAP anteckningar tooget hello mer information om hur toorun dessa rapporter, och hur tooobtain de senaste korrigeringar/versionerna:

- [SAP-kommentar #1793345 - storlek för SAP Suite på HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [SAP Obs #1872170 - Suite HANA och S/4 HANA storlek för rapporten](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP-kommentar #2121330 – vanliga frågor och svar: SAP BW på HANA storleksanpassa rapport](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP Obs #1736976 - storlek för rapporten för BW på HANA](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP Obs #2296290 - ny storlek för rapport för BW på HANA](https://launchpad.support.sap.com/#/notes/2296290)

Grön fältet implementeringar är SAP snabbt ange storlek tillgängliga toocalculate minneskrav för hello implementering av SAP-program ovanpå HANA.

Minneskrav för HANA ökar när datavolymen växer, så toobe medveten om hello minnesförbrukning nu och vara kan toopredict vad det är pågående toobe i hello framtida. Baserat på hello minneskrav, kan du sedan mappa din begäran till en hello HANA stora instans SKU: er.

## <a name="requirements"></a>Krav

Den här listan monterar krav för SAP HANA i Azure (större instanser).

**Microsoft Azure:**

- En Azure-prenumeration som kan vara länkad tooSAP HANA i Azure (stora instanser).
- Microsoft Premier Support-kontrakt. Finns [SAP stöd Obs #2015553 – SAP på Microsoft Azure: stöd för krav](https://launchpad.support.sap.com/#/notes/2015553) specifik information relaterade toorunning SAP i Azure. Med 384 och fler processorer HANA stora instans enheter, behöver du också tooextend hello Premier Support-kontrakt tooinclude Azure snabba svar (ARR).
- Medvetenhet om hello HANA stora instanser SKU: er du behöver när du utför en storlek utöva med SAP.

**Nätverksanslutning:**

- Azure ExpressRoute mellan lokala tooAzure: tooconnect ditt lokala datacenter tooAzure, se till att tooorder på minst 1 Gbit/s-anslutning från Leverantören. 

**Operativsystem:**

- Licenser för SUSE Linux Enterprise Server 12 för SAP-program.

> [!NOTE] 
> hello operativsystemet som levereras av Microsoft har inte registrerats med SUSE eller ansluten med en SMT-instans.

- SUSE Linux prenumeration Management Tool (SMT) distribuerade i Azure på Azure-VM. Detta ger hello möjligheten för SAP HANA på Azure (stora instanser) toobe registrerade och respektive uppdaterade SUSE (eftersom det inte finns någon internet-anslutning i HANA stora instanser datacenter). 
- Licenser för Red Hat Enterprise Linux 6.7 eller 7.2 för SAP HANA.

> [!NOTE]
> hello operativsystemet som levereras av Microsoft har inte registrerats med Red Hat, inte heller it anslutna tooa Red Hat prenumeration Manager-instansen.

- Red Hat prenumeration Manager distribueras i Azure på Azure-VM. hello Red Hat prenumeration Manager ger hello möjlighet för SAP HANA på Azure (stora instanser) toobe registrerade och respektive uppdaterade Red Hat (eftersom det finns ingen direkt Internetåtkomst från hello-klient som har distribuerats på hello Azure stora instans stämpel).
- SAP kräver toohave en supportbegäran kontrakt med leverantören Linux. Det här kravet tas inte bort av hello lösning av HANA stora instanser eller hello fakta som din kör Linux i Azure. Till skillnad från med några av hello Linux Azure-galleriet bilder ingår hello avgifter inte i hello lösningen-erbjudandet om HANA stora instanser. Det är på du som en kund toofulfill hello krav för SAP beträffande supportavtal med hello Linux distributören.   
   - Leta upp hello servicekontrakt kraven för stöd för SUSE Linux [SAP Obs #1984787 - SUSE LINUX Enterprise Server 12: installationsinformation](https://launchpad.support.sap.com/#/notes/1984787) och [SAP Obs #1056161 - SUSE prioritet stöd för SAP-program](https://launchpad.support.sap.com/#/notes/1056161).
   - För Red Hat Linux måste toohave hello korrekt prenumeration nivåer som innehåller och service (uppdaterar toohello operativsystem för HANA stora instanser. Red Hat rekommenderar att du hämtar en prenumeration ”RHEL för SAP Business-program”. Om support och tjänster, kontrollera [SAP Obs #2002167 - Red Hat Enterprise Linux 7.x: Installation och uppgradering](https://launchpad.support.sap.com/#/notes/2002167) och [SAP Obs #1496410 - Red Hat Enterprise Linux 6.x: Installation och uppgradering](https://launchpad.support.sap.com/#/notes/1496410) för information.

**Databas:**

- Licenser och installation av programkomponenter för SAP HANA (plattform eller enterprise edition).

**Program:**

- Licenser och installation av programkomponenter för SAP program ansluter tooSAP HANA och relaterade SAP support-kontrakt.
- Licenser och programvarukomponenter i installationen för alla icke-SAP-program används i relationen tooSAP HANA på Azure (stora instanser)-miljön och relaterade supportavtal.

**Kunskaper:**

- Erfarenhet och kunskap om Azure IaaS och dess komponenter.
- Erfarenhet och kunskap om hur du distribuerar SAP arbetsbelastning i Azure.
- Certifierad personliga SAP HANA-Installation.
- SAP systemarkitekt kunskaper toodesign hög tillgänglighet och katastrofåterställning runt SAP HANA.

**SAP:**

- Förutsättningen är att du är en SAP-kund och har stöd för ett kontrakt med SAP
- Särskilt för implementeringar på hello typ II klass av HANA stora instans SKU: er rekommenderas tooconsult med SAP på versioner av SAP HANA och eventuell konfigurationer på stora storlek skala upp maskinvara.


## <a name="storage"></a>Lagring

Hej lagringslayout för SAP HANA i Azure (stora instanser) konfigureras av SAP HANA på Azure Service Management via SAP rekommenderade riktlinjer som beskrivs i hello [lagringskraven för SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) vitboken.

hello HANA stora instanser av hello typ I klassen har fyra gånger hello minne volymen som lagringsvolym. För hello II enhetsklass HANA stort instans ska hello lagring inte toobe fyra gånger så. hello enheter har en volym som är avsedd för att lagra HANA säkerhetskopieringar av transaktionsloggen. Mer information i [hur tooinstall och konfigurera SAP HANA (stora instanser) på Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Se hello i den följande tabellen vad gäller lagringsallokering. hello tabell innehåller ungefär hello kapacitet för hello olika volymer med hello olika HANA stora instans enheter.

| HANA stora instans SKU | Hana-data | Hana/logg | Hana/delat | Hana/loggsäkerhetskopiering |
| --- | --- | --- | --- | --- |
| S72 | 1280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3328 GB | 768 GB |1280 GB | 768 GB |
| S192 | 4608 GB | 1024 GB | 1536 GB | 1024 GB |
| S192m | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384 | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384m | 12 000 GB | 2050 GB | 2050 GB | 2040 GB |
| S384xm | 16 000 GB | 2050 GB | 2050 GB | 2040 GB |
| S576 | 20 000 GB | 3100 GB | 2050 GB | 3100 GB |
| S768 | 28 000 GB | 3100 GB | 2050 GB | 3100 GB |
| S960 | 36,000 GB | 4100 GB | 2050 GB | 4100 GB |


Faktiska distribuerade volymer kan variera något beroende på distributionen och som du kan använda tooshow hello Volymstorlekar.

Om du dela upp en HANA stora instans SKU ser några exempel på uppdelning delar ut som:

| Minnespartition i GB | Hana-data | Hana/logg | Hana/delat | Hana/loggsäkerhetskopiering |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1280 GB | 512 GB | 768 GB | 512 GB |
| 1024 | 1792 GB | 640 GB | 1024 GB | 640 GB |
| 1536 | 3328 GB | 768 GB | 1280 GB | 768 GB |


Dessa storlekar är grov volym siffror som kan variera något baserat på distributionen och verktyg som används för toolook på hello volymer. Det är också andra partitionsstorlek thinkable som 2,5 TB. Dessa lagringsstorlekar skulle beräknas med en liknande formel som används för hello partitioner ovan. hello termen ”partitioner' anger inte att hello operativsystem, minne eller CPU-resurser på något sätt partitioneras. Det anger bara partitioner för lagring för hello olika HANA instanser som du kanske vill toodeploy på en enkel HANA stora instans enhet. 

Som en kund kan ha behöver du för mer lagringsutrymme har hello möjligheten tooadd lagring toopurchase ytterligare lagringsutrymme i enheter som 1 TB. Den här ytterligare lagringsutrymme kan kan läggas till som en ytterligare volym eller använda tooextend en eller flera av hello befintliga volymer. Det är inte möjligt toodecrease hello storlekar hello volymer som ursprungligen har distribuerats och främst dokumenterats av hello tabeller ovan. Det är också inte möjligt toochange hello namn hello volymer eller montera namn. hello lagringsvolymer är som beskrivs ovan anslutna toohello HANA stora instans enheter som NFS4 volymer.

Som en kund kan du toouse lagring ögonblicksbilder för säkerhetskopiering/återställning och katastrofåterställning återställning. Mer information om det här avsnittet beskrivs i [SAP HANA (stora instanser) med hög tillgänglighet och katastrofåterställning i Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="encryption-of-data-at-rest"></a>Kryptering av vilande data
hello lagringsutrymme som används för HANA stora instanser kan en transparent kryptering av hello data som lagras på hello diskar. Vid tidpunkten för distribution av en HANA stora instans enhet har du hello alternativet toohave den här typen av kryptering aktiverat. Du kan också välja toochange tooencrypted volymer efter redan hello-distributionen. hello flytt från icke-krypterade tooencrypted volymer är transparent och kräver inte ett driftstopp. 

Med hello typ I klassen SKU: er, hello volym hello Start LUN som är lagrad på, är krypterad. Vid hello typ II klass av SKU: er av HANA stora instanser behöver du tooencrypt hello Start LUN med OS-metoder. 


## <a name="networking"></a>Nätverk

hello arkitekturen i Azure-nätverk är en viktig del toosuccessful distribution av SAP-program på HANA stora instanser. SAP HANA på Azure (stora instanser)-distributioner har vanligtvis en större SAP-liggande med flera olika SAP-lösningar med olika storlekar för databaser, CPU-resursanvändningen och minnesanvändning. Inte alla dessa SAP-system är sannolikt baserat på SAP HANA så att din SAP-liggande skulle förmodligen en hybridlösning som använder:

- Distribuerade SAP system lokalt. På grund av tootheir storlekar och kan inte dessa system för närvarande finnas i Azure; ett exempel är ett produktionssystem SAP ERP som körs på Microsoft SQL Server (som hello databas) som kräver mer CPU eller minnesresurser virtuella Azure-datorer kan få.
- Distribuera SAP HANA-baserade SAP system lokalt.
- Distribuerade SAP-system i virtuella Azure-datorer. Dessa system kan vara utveckling, testning, sandbox eller produktion instanser för någon av hello SAP NetWeaver-baserade program som kan distribuera i Azure (på VM: ar), baserat på resursen användnings- och behov. Dessa system kan också baseras på databaser som SQL Server (se [SAP stöd Obs #1928533 – SAP-program på Azure: produkter och Virtuella Azure-typer](https://launchpad.support.sap.com/#/notes/1928533/E)) eller SAP HANA (finns [SAP HANA certifierade IaaS plattformar](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)).
- Distribuera SAP programservrar i Azure (på virtuella datorer) som utnyttjar SAP HANA i Azure (stora instans) i Azure stora instans stämplar.

Hybrid SAP liggande (med fyra eller flera olika distributionsscenarier) är typiska, finns men det många kunden fall av fullständig SAP liggande körs i Azure. Eftersom Microsoft Azure virtuella datorer blir allt mer kraftfulla, öka hello antalet flytta sina SAP-lösningar på Azure-kunder.

Azure-nätverk i hello kontexten för SAP-system som distribueras i Azure är inte komplicerad. Den är baserad på hello följande principer:

- Virtuella Azure-nätverk (Vnet) måste toobe anslutna toohello Azure ExpressRoute-krets som ansluter tooon lokala nätverk.
- En ExpressRoute-krets ansluta lokala tooAzure vanligtvis bör ha en bandbredd på 1 Gbit/s eller högre. Den här minimal bandbredd kan tillräcklig bandbredd för överföring av data mellan system som körs på virtuella Azure-datorer (samt anslutning tooAzure system från slutanvändare lokala) och lokala system.
- SAP-system i Azure måste toobe anges i Azure Vnet toocommunicate med varandra.
- Active Directory och DNS finns lokalt har utökats till Azure via ExpressRoute från lokalt.


> [!NOTE] 
> Endast en enda Azure-prenumeration kan länkas endast tooone enskild klient i en stor instans stämpel i en viss Azure-region fakturering synvinkel och omvänt en enda stor instans stämpel klient kan länkas endast tooone Azure-prenumeration. Detta är inte olika tooany andra fakturerbar objekt i Azure

Distribuera SAP HANA i Azure (stora instanser) i flera olika Azure-regioner distribueras resulterar i en separat klient toobe på hello stora instans stämpeln. Du kan dock köra både under hello samma Azure-prenumeration så länge dessa instanser är en del av hello samma SAP liggande. 

> [!IMPORTANT] 
> Azure Resource Manager-distribution stöds med SAP HANA i Azure (stora instanser).

 

### <a name="additional-azure-vnet-information"></a>Ytterligare information i Azure VNet

I ordning tooconnect ett Azure VNet tooExpressRoute måste du skapa en Azure gateway (se [om virtuella nätverksgatewayerna expressroute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). En Azure-gateway kan användas antingen med ExpressRoute tooan infrastruktur utanför Azure (eller tooan Azure stora instans stämpel) eller tooconnect mellan virtuella Azure-nätverk (se [och konfigurera en VNet-till-VNet-anslutning för Resource Manager med hjälp av PowerShell ](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Du kan ansluta hello Azure gateway tooa högst fyra olika ExpressRoute-anslutningar så länge dessa anslutningar kommer från olika MS Enterprise kanter (MSEE) routrar.  Mer information finns i [SAP HANA (stora instanser) infrastruktur och anslutningar på Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> hello genomströmning som ger en Azure-gateway är olika för både användningsfall (se [om VPN-Gateway](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). hello maximalt dataflöde vi kan åstadkomma med en gateway för virtuellt nätverk är 10 Gbit/s med hjälp av en ExpressRoute-anslutning. Kom ihåg att kopiera filer mellan en Azure VM som finns i ett Azure VNet och ett system lokalt (som en enda kopia-ström) inte uppnå hello totala genomflödet i hello annan gateway SKU: er. tooleverage hello fullständig bandbredd för hello VNet gateway, måste du använda flera strömmar eller kopiera olika filer i parallella strömmar av en enskild fil.


### <a name="networking-architecture-for-hana-large-instances"></a>Nätverksarkitekturen för HANA stora instanser
hello nätverksarkitekturen för HANA stora instanser kan som visas nedan, delas upp i fyra olika delar:

- Lokala nätverk och tooAzure för ExpressRoute-anslutning. Den här delen är hello kunder domän och anslutna tooAzure via ExpressRoute. Detta är hello del i hello nedre högra hörnet av hello grafik nedan.
- Azure-nätverk så kort som beskrivs ovan med virtuella Azure-nätverk, som har gateways igen. Detta är ett område där du behöver toofind hello lämpliga Designer för krav på program-, säkerhets- och efterlevnadskrav. Med stora HANA-instanser är ett annat övervägande vad gäller antalet Vnet och Azure gateway-SKU: er toochoose från. Detta är hello del i hello övre högra hörnet av hello grafik.
- Anslutningar av HANA stora instanser för ExpressRoute-teknik till Azure. Den här delen distribueras och hanteras av Microsoft. Allt du behöver toodo när en kund är tooprovide vissa IP-adressintervall och efter hello distributionen av dina tillgångar i HANA stora instanser ansluter hello ExpressRoute-krets toohello Azure VNet(s) (se [SAP HANA (stora instanser) infrastruktur och anslutningen på Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 
- Nätverk i HANA stora instanser som är mest transparent du som kund.

![Azure-VNet ansluten tooSAP HANA på Azure (stora instanser) och lokalt](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

hello faktum att du använder HANA stora instanser ändras inte hello krav tooget dina lokala tillgångar anslutna via ExpressRoute tooAzure. Också ändras inte hello krav för att ha en eller flera Vnet som kör virtuella hello Azure-datorer vilket lager av värden hello-program som ansluter toohello HANA instanser finns i HANA stora instans enheter. 

hello skillnaden tooSAP distributioner i ren Azure, medföljer hello fakta som:

- hello HANA stora instans enheter av din klientorganisation, kund är anslutna via en annan ExpressRoute-krets i Azure-VNet(s). I ordning tooseparate belastningen villkor hello hello lokala tooAzure VNets ExpressRoute och länkar mellan Azure Vnet och HANA stora instanser inte delar samma routrar.
- hello arbetsbelastning profil mellan hello SAP programnivå och hello HANA instans är en annan typ av många små begäranden och burst som data överförs (resultatmängder) från SAP HANA i hello programnivå.
- hello SAP programarkitektur är mer känslig toonetwork svarstid än i vanliga scenarier där data hämtar utbyts mellan lokala och Azure.
- Hej VNet gateway har minst två ExpressRoute-anslutningar och både anslutningar delar hello Maximal bandbredd för inkommande data för hello VNet gateway.

hello Nätverksfördröjningen erfarna mellan virtuella Azure-datorer och enheter för stora HANA instans kan vara högre än en typisk fördröjningen för VM-VM-nätverk. Beroende på hello Azure-region, hello mäts kan överstiga hello 0.7 ms fördröjningen klassificeras som under medel i [SAP Obs #1100926 – vanliga frågor och svar: nätverksprestanda](https://launchpad.support.sap.com/#/notes/1100926/E). Kunder distribuera ändå SAP HANA-baserade SAP produktionsprogram mycket på SAP HANA stora instanser. hello-kunder som distribuerats rapporterats bra förbättringar av deras SAP-program som körs på SAP HANA HANA stora instans enheter. Dock bör du testa dina affärsprocesser noggrant i Azure HANA stora instanser.
 
I ordning tooprovide deterministiska Nätverksfördröjningen mellan virtuella Azure-datorer och HANA stort instans är hello valet av hello Azure VNet Gateway-SKU viktigt. Till skillnad från hello trafikmönster mellan lokala och virtuella Azure-datorer utveckla mönster för hello trafik mellan virtuella Azure-datorer och HANA stora instanser små men hög belastning av förfrågningar och data volymer toobe överförs. I ordning toohave sådana belastning hanteras, rekommenderar vi hello användning av hello UltraPerformance Gateway-SKU. Hello användning av hello UltraPerformance gateway SKU som Azure VNet Gateway för hello typ II klass av HANA stora instans SKU: er är obligatoriskt.  

> [!IMPORTANT] 
> Angivna hello hello total nätverkstrafik mellan hello SAP-program och databasen lager endast HighPerformance eller UltraPerformance gateway SKU: er för VNets stöds för att ansluta tooSAP HANA i Azure (stora instanser). För stora HANA instans typ II SKU: er stöds endast hello UltraPerformance Gateway-SKU som Azure VNet Gateway.



### <a name="single-sap-system"></a>Enda SAP-system

hello lokal infrastruktur som visas ovan är anslutna via ExpressRoute till Azure och hello ExpressRoute-krets ansluter till en Microsoft Enterprise Edge Router (MSEE) (se [teknisk översikt för ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). När etablerade, den rutten ansluter till hello Microsoft Azure-stamnät och alla Azure-regioner är tillgängliga.

> [!NOTE] 
> Ansluta toohello MSEE närmaste toohello Azure-region i hello SAP liggande för att köra SAP landskap i Azure. Azure stora instans stämplar är anslutna via dedikerade MSEE enheter toominimize-Nätverksfördröjningen mellan Azure virtuella datorer i Azure IaaS och stora instans stämplar.

hello VNet-gateway för hello Azure virtuella datorer, värd för SAP programinstanser är anslutna toothat ExpressRoute-krets och hello samma virtuella nätverk är anslutna tooa separat MSEE Router dedikerade tooconnecting tooLarge instans stämplar.

Detta är ett enkelt exempel på ett enda SAP-system där hello SAP programnivå finns i Azure och hello SAP HANA-databas som körs på SAP HANA i Azure (stora instanser). hello antaganden är hello VNet gateway bandbredd på 2 Gbit/s eller 10 Gbit/s genomströmning representerar inte en flaskhals.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Flera SAP-system eller stora SAP-system

Om flera SAP-system eller stora SAP-system är distribuerade ansluter tooSAP HANA på Azure (stora instanser), den &#39; kan s rimliga tooassume hello genomflödet i hello VNet gateway bli en flaskhals. I sådana fall måste toosplit hello programmet lager till flera virtuella Azure-nätverk. Det kan också vara recommendable toocreate särskilda Vnet som ansluter tooHANA stora instanser för fall som:

- Att säkerhetskopiera direkt från hello HANA instanser i HANA stora instanser tooa VM i Azure som är värd för NFS-resurser
- Kopiera stora säkerhetskopieringar och andra filer från HANA stora enheter toodisk instansutrymme hanteras i Azure.

Med hjälp av separata Vnet som värd för virtuella datorer som hanterar hello lagring undviker effekten av stora filer eller dataöverföring från HANA stora instanser tooAzure på hello VNet-Gateway som fungerar hello virtuella datorer som kör hello SAP programnivå. 

För en mer skalbar nätverksarkitektur:

- Använda flera virtuella Azure-nätverk för ett enda, större SAP programmet lager.
- Distribuera en separat Azure VNet för varje SAP system har distribuerats, jämfört med toocombining systemen SAP i separata undernät under hello samma virtuella nätverk.

 En nätverk Mer skalbar arkitektur för SAP HANA i Azure (stora instanser):

![Distribuera SAP programnivå över flera virtuella Azure-nätverk](./media/hana-overview-architecture/image4-networking-architecture.png)

Distribuera hello SAP programnivå eller komponenter, över flera virtuella Azure-nätverk som visas ovan, introducerade oundvikligt latens kostnader som inträffade under kommunikation mellan hello program finns i de virtuella Azure-nätverk. Standard finns hello nätverkstrafik mellan virtuella Azure-datorer i olika Vnet vidarebefordra via hello MSEE routrar i den här konfigurationen. Men eftersom September 2016 kan denna routning optimeras. Hej toooptimize sätt och att minska hello svarstiden för kommunikationen mellan två Vnet peering Azure Vnet inom hello samma region. Även om dessa Vnet i olika prenumerationer. Med Azure VNet-peering, hello kommunikation mellan virtuella datorer i två olika virtuella Azure-nätverk kommunicera används hello Azure-nätverk stamnät toodirectly med varandra. Därmed visar liknande svarstid som om hello virtuella datorer är i hello samma virtuella nätverk. Medan trafik, IP-adressintervall som är anslutna via hello Azure VNet gateway-adresser dirigeras via hello enskilda VNet-gateway hello virtuella nätverk. Du kan få information om Azure VNet-peering i hello artikeln [VNet-peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Routning i Azure

Det finns två viktiga routning Nätverksöverväganden för SAP HANA i Azure (stora instanser):

1. SAP HANA i Azure (stora instanser) kan endast användas av virtuella Azure-datorer i hello dedikerade ExpressRoute-anslutning. inte direkt från lokalt. Vissa klienter för administration och alla program som kräver direkt åtkomst som SAP lösning Manager som körs lokalt, inte kan ansluta toohello SAP HANA-databas.

2. SAP HANA på Azure (stora instanser)-enheter har en tilldelad IP-adress från hello Server Pool för IP-adress vara du som hello-kund har skickats (se [SAP HANA (stora instanser) infrastruktur och anslutningar på Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) information).  Denna IP-adress är tillgänglig via hello Azure-prenumerationer och ExpressRoute som ansluter Azure Vnet tooHANA i Azure (stora instanser). hello IP-adress som tilldelats utanför Serverpool för IP-adressintervall direkt tilldelas toohello maskinvara enhet och är inte NAT'ed längre detta var hello fallet hello första distributioner av den här lösningen. 

> [!NOTE] 
> Om du behöver tooconnect tooSAP HANA i Azure (stora instanser) i en _datalagret_ scenario där program och/eller slutanvändare måste tooconnect toohello SAP HANA-databas (som körs direkt), måste vara en annan nätverkskomponent används: en omvänd proxy tooroute data, tooand från. Exempelvis F5 BIG-IP, NGINX med Traffic Manager distribueras i Azure som en virtuell brandväggen-trafik routning lösning.

### <a name="internet-connectivity-of-hana-large-instances"></a>Internet-anslutning av HANA stora instanser
HANA stora instanser har inte direkt Internetanslutning. Detta är att begränsa din förmåga att till exempel registrera hello OS-avbildningen direkt med hello OS-leverantör. Därför måste du kanske toowork med lokala SLES SMT-servern eller RHEL prenumeration Manager

### <a name="data-encryption-between-azure-vms-and-hana-large-instances"></a>Kryptering av data mellan virtuella Azure-datorer och HANA stora instanser
Data som överförs mellan HANA stora instanser och virtuella datorer i Azure krypteras inte. Enbart för hello exchange mellan hello HANA DBMS sida och JDBC/ODBC-baserat program kan du aktivera kryptering av trafik. Referens [denna dokumentation av SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false)

### <a name="using-hana-large-instance-units-in-multiple-regions"></a>Använda HANA stora instans enheter i flera regioner

Du kan ha andra orsaker toodeploy SAP HANA i Azure (stora instanser) i flera Azure-regioner, förutom katastrofåterställning. Kanske vill tooaccess HANA stora instanser från varje hello virtuella datorer som distribueras i hello olika Vnet i hello regioner. Eftersom hello IP-adresser tilldelas toohello olika HANA stora instanser enheter sprids inte utöver hello Azure Vnet (som är anslutna direkt via deras gateway toohello förekomster), finns det ett mindre ändra toohello VNet design introduceras ovan: en Azure VNet-gateway kan hantera fyra olika ExpressRoute-kretsar utanför olika MSEEs och varje virtuellt nätverk som är anslutna tooone för hello stora instans stämplar kan vara anslutna toohello stora instans stämpel i en annan Azure-region.

![Azure Vnet anslutna tooAzure stora instans tidsstämplar i olika Azure-regioner](./media/hana-overview-architecture/image8-multiple-regions.png)

hello ovanstående bild visar hur hello olika virtuella Azure-nätverk i både områden är anslutna tootwo olika ExpressRoute-kretsar som används tooconnect tooSAP HANA i Azure (stora instanser) i både Azure-regioner. hello introduceras nya anslutningar är hello rektangulär röda linjer. Med dessa anslutningar utanför hello Azure Vnet, hello virtuella datorer som körs i ett av dessa Vnet har åtkomst till var och en av hello olika HANA stora instanser enheter som distribueras i hello två regioner. Som du ser i hello graphics ovan, förutsätts att du har två ExpressRoute-anslutningar från lokala toohello två Azure regioner. Vi rekommenderar för katastrofåterställning orsaker.

> [!IMPORTANT] 
> Om flera ExpressRoute-kretsar används ska tillagt AS sökvägen och inställningar för lokala inställningar BGP använda tooensure rätt routning av trafik.


