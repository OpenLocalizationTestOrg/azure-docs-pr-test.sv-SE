---
title: "Azure Premium Storage: Utforma för prestanda | Microsoft Docs"
description: "Utforma program med höga prestanda med Azure Premium-lagring. Premium-lagring ger stöd för I/O-intensiva arbetsbelastningar som körs på Azure Virtual Machines diskar med hög prestanda, låg latens."
services: storage
documentationcenter: na
author: aungoo-msft
manager: tadb
editor: tysonn
ms.assetid: e6a409c3-d31a-4704-a93c-0a04fdc95960
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: aungoo
ms.openlocfilehash: 75845eb4707e8e5606153a5e1a7c10b4113ee121
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-premium-storage-design-for-high-performance"></a>Azure Premium Storage: Design för hög prestanda
## <a name="overview"></a>Översikt
Den här artikeln innehåller riktlinjer för att bygga program med Azure Premium-lagring med hög prestanda. Du kan använda hello anvisningarna i det här dokumentet som kombineras med prestanda bästa praxis tillämpliga tootechnologies används av ditt program. Vi har använt SQL Server körs på Premium-lagring som exempel i hela dokumentet tooillustrate hello riktlinjer.

Medan vi adressen prestanda scenarier för hello lagringsskikt i den här artikeln behöver toooptimize hello programnivå. Om du är värd för en SharePoint-grupp på Azure Premium-lagring, kan du till exempel använda hello SQL Server-exempel från den här artikeln toooptimize hello-databasservern. Dessutom kan optimera hello SharePoint-servergruppens webbservern och program server tooget hello de flesta prestanda.

Den här artikeln hjälper dig att besvara följande frågor om hur du optimerar prestanda på Azure Premium-lagring

* Hur toomeasure programmets prestanda?  
* Varför du inte ser förväntade högpresterande?  
* Vilka faktorer som påverkar ditt programprestanda på Premium-lagring?  
* Hur påverkar prestanda för programmet på Premium-lagring av dessa faktorer?  
* Hur kan optimeras för IOPS, bandbredd och fördröjning?  

Vi har angett dessa riktlinjer för Premium-lagring eftersom arbetsbelastningar som körs på Premium-lagring är mycket känslig prestanda. Vi har samlat exempel där det behövs. Du kan också använda några av dessa riktlinjer tooapplications som körs på virtuella IaaS-datorer med standardlagring diskar.

Innan du börjar, om du är ny tooPremium lagring, först läsa hello [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](storage-premium-storage.md) och [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md)artiklar.

## <a name="application-performance-indicators"></a>Nyckeltal för programmet
Vi bedöma om ett program fungerar bra eller inte använda prestanda symboler som, hur snabbt ett bearbetning av en användarbegäran, hur mycket data som bearbetar ett program per begäran, hur många begäranden som bearbetas ett program i en specifik tidsperiod, hur lång tid en användare har toowait tooget ett svar efter att ha skickat begäran. hello tekniska villkor för dessa nyckeltal är IOPS, genomflöde eller bandbredd och svarstid.

I det här avsnittet diskuteras hello vanliga nyckeltal hello kontexten för Premium-lagring. I hello efter avsnittet, samla in programkrav, får du lära dig hur toomeasure dessa nyckeltal för ditt program. Senare i Optimera programprestanda du lära dig om hello faktorer som påverkar dessa prestanda indikatorer och rekommendationer toooptimize dem.

## <a name="iops"></a>IOPS
IOPS är antalet begäranden som programmet skickar toohello diskar med lagringsutrymme i en sekund. Ett i/o-åtgärden gick att läsa eller skriva sekventiellt eller slumpmässigt. OLTP-program som en online retail-webbplats måste tooprocess många samtidiga användare begär omedelbart. hello användarförfrågningar är insert och uppdatera beräkningsintensiva databastransaktioner, vilket hello program måste bearbeta snabbt. Därför kräver OLTP program mycket hög IOPS. Dessa program hanterar miljontals små och slumpmässiga i/o-begäranden. Om du har ett sådant program, måste du skapa hello programmet infrastruktur toooptimize för IOPS. I hello senare avsnittet *optimera programprestanda*, diskuterar vi i detalj alla hello faktorer att överväga tooget höga IOPS.

När du ansluter en premium storage disk tooyour hög skala VM, Azure tillhandahåller du en garanterad antalet IOPS enligt hello disk-specifikationen. Till exempel etablerar en disk p 50 7500 IOPS. Varje hög skala VM-storlek har också en specifik IOPS-gräns som den kan klara. Till exempel har en Standard GS5 VM 80 000 IOPS begränsa.

## <a name="throughput"></a>Dataflöde
Genomströmning eller bandbredd är hello mängden data som programmet skickar toohello diskar med lagringsutrymme i ett visst intervall. Om ditt program fungerar i/o-åtgärder med större stora i/o, kräver hög genomströmning. Data warehouse program brukar tooissue beräkningsintensiva genomsökningen som åtkomst till stora delar av data i taget och utföra vanliga massåtgärder. Med andra ord kräver sådana program högre genomströmning. Om du har ett sådant program, måste du utforma sin infrastruktur toooptimize för genomströmning. I nästa avsnitt hello diskuterar vi i detalj hello faktorer du måste justera tooachieve detta.

När du ansluter en premium storage disk tooa hög skala VM, Azure tillhandahåller genomströmning enligt specifikationen för disken. Till exempel etablerar en disk p 50 250 MB per andra disken genomflöde. Varje hög skala VM-storlek har också som särskild genomströmningen gräns som den kan klara. Standard GS5 VM har till exempel en maximalt dataflöde 2 000 MB per sekund. 

Det finns en relation mellan genomflöde och IOPS enligt hello formeln nedan.

![](media/storage-premium-storage-performance/image1.png)

Därför är det viktigt toodetermine hello optimal genomströmning och IOPS värden som krävs för ditt program. När du provar toooptimize en hämtar hello andra också påverkas. I ett senare avsnitt *optimera programprestanda*, diskuteras i mer information om hur du optimerar IOPS och genomflöde.

## <a name="latency"></a>Svarstid
Svarstiden är hello tid det tar ett program tooreceive en enskild begäran, skickar den toohello lagringsdiskar och skicka hello svar toohello. Detta är en kritisk mått på ett programs prestanda i tillägget tooIOPS och dataflöde. hello svarstiden för en disk för premium-lagring är hello tid det tar tooretrieve hello information för en begäran och kommunicera tillbaka tooyour program. Premium-lagring ger genomgående korta svarstiderna. Om du aktiverar cachelagring på premium lagringsdiskar ReadOnly-värden, kan du hämta mycket lägre skrivskyddade latens. Diskuteras diskcachelagring i detalj i senare avsnitt på *optimera programprestanda*.

När du optimerar dina program tooget högre IOPS och dataflöde, påverkar hello svarstiden för programmet. Efter att justera hello programprestanda alltid utvärdera hello svarstiden för hello programmet tooavoid fördröjningar som oväntat beteende.

## <a name="gather-application-performance-requirements"></a>Samla in kraven på programmets prestanda
hello första steget i utforma högpresterande program som körs på Azure Premium-lagring är toounderstand hello prestandakraven för ditt program. Du kan optimera din tooachieve hello mest optimala programprestanda när du samlar in prestandakrav.

I föregående avsnitt hello förklarades hello vanliga nyckeltal, IOPS, genomflöde och svarstid. Du måste identifiera vilka av dessa nyckeltal är kritiska tooyour programmet toodeliver hello önskad användarupplevelsen. Till exempel är höga IOPS viktig de flesta tooOLTP program bearbetning av miljontals transaktioner på en sekund. Hög genomströmning är viktig för Data Warehouse-program som bearbetar stora mängder data på en sekund. Mycket låg latens är avgörande för realtidsprogram som direktuppspelad video streaming webbplatser.

Därefter Mät hello maximala prestandakraven för ditt program under hela sin livslängd. Använd hello exempel checklista nedan som en start. Registrera hello maximala prestandakrav under normal, belastning och låg belastning på nätverket arbetsbelastning punkter. Identifiera krav för alla arbetsbelastningar nivåer du ska kunna toodetermine hello övergripande prestandakrav på för programmet. Till exempel blir hello normal arbetsbelastning av webbplatser för e-handel hello transaktioner som den erbjuder under större delen av tiden till ett år. hello belastning arbetsbelastningen för hello webbplatsen kommer att hello transaktioner som den erbjuder under köpstarka jul eller särskilda försäljning händelser. hello belastning arbetsbelastningen är vanligtvis erfarna under en begränsad tid, men kan kräva att ditt program tooscale två eller flera gånger dess normal drift. Ta reda på hello 50: e percentilen 90: e percentilen krav, och 99 percentil. Detta hjälper filtrerar ut eventuella avvikare i hello prestandakrav och du kan fokusera arbetet på Optimera för hello rätt värden.

**Checklista för programmets prestanda krav**

| **Krav på prestanda** | **50: e percentilen** | **90: e percentilen** | **99 percentil** |
| --- | --- | --- | --- |
| Max. Transaktioner per sekund | | | |
| % Läsåtgärder | | | |
| % Skrivåtgärder | | | |
| % Slumpmässiga åtgärder | | | |
| % Sekventiella operationer | | | |
| Storlek för i/o-begäran | | | |
| Genomsnittlig genomströmning | | | |
| Max. Dataflöde | | | |
| Min. Svarstid | | | |
| Genomsnittlig svarstid | | | |
| Max. Processor | | | |
| Genomsnittlig CPU | | | |
| Max. Minne | | | |
| Genomsnittlig minne | | | |
| Ködjup | | | |

> [!NOTE]
> Du bör skalning talen utifrån förväntade tillväxt i ditt program. Det är en bra idé tooplan tillväxt i förväg, eftersom det kan vara svårare toochange hello infrastruktur för att förbättra prestanda senare.
>
>

Om du har ett befintligt program och vill toomove tooPremium lagring, först skapa hello checklista ovan för hello befintligt program. Skapa sedan en prototyp av programmet på Premium-lagring och design hello program baserat på riktlinjer som beskrivs i *optimera programprestanda* i ett senare avsnitt av det här dokumentet. hello nästa avsnitt beskrivs hello-verktyg som du kan använda toogather hello prestandamått.

Skapa en checklista liknande tooyour befintliga program för hello prototyp. Med hjälp av Benchmarking verktyg du simulera hello arbetsbelastningar och mäta prestanda på hello prototyp program. Avsnittet hello på [Benchmarking](#benchmarking) toolearn mer. Genom att göra så att du kan fastställa om Premium-lagring kan matcha eller överskrider dina prestandakrav för programmet. Sedan kan du implementera hello samma riktlinjer för ditt produktionsprogram.

### <a name="counters-toomeasure-application-performance-requirements"></a>Räknare för toomeasure programkrav för prestanda
Hej bästa sätt toomeasure prestandakraven för ditt program, är toouse verktyg för prestandaövervakning hello operativsystemet hello-Server. Du kan använda PerfMon för Windows och iostat för Linux. Dessa verktyg avbilda räknare motsvarande tooeach mått förklaras i hello senare avsnitt. Du måste hämta hello värdena för dessa räknare när programmet körs dess normal, belastning och låg belastning på nätverket arbetsbelastningar.

hello-prestandaräknare är tillgängliga för processor, minne och varje logisk disk och fysisk disk för servern. När du använder premiumdiskar för lagring med en virtuell dator hello fysisk diskräknare anges för varje disk för premium-lagring och räknare för logisk disk är för varje volym som har skapats på hello premium storage diskar. Du måste hämta hello värden för hello diskar som värd för din arbetsbelastning i programmet. Om det är en tooone mappning mellan logiska och fysiska diskar, kan du läsa toophysical diskräknare; Annars finns toohello räknare för logiska diskar. På Linux genererar hello iostat kommandot en rapport över processor- och diskresurser. Hej diskanvändningsrapporten innehåller statistik per fysisk enhet eller partition. Om du har en databasserver med dess data och loggfiler på separata diskar kan du samla in informationen för båda diskarna. Tabellen nedan beskrivs räknare för diskar, processor och minne:

| Räknaren | Beskrivning | PerfMon | Iostat |
| --- | --- | --- | --- |
| **IOPS eller transaktioner per sekund** |Antalet i/o-begäranden utfärdade toohello Lagringsdisken per sekund. |Diskläsningar/sek <br> Diskskrivningar/sek |transaktionsprogram <br> r/s <br> w/s |
| **Diskläsningar och skrivningar** |% av läsåtgärder och skrivåtgärder utföras på hello disk. |Läs Disktid i procent <br> Skriv Disktid i procent |r/s <br> w/s |
| **Dataflöde** |Mängd data läses från eller skrivs toohello disk per sekund. |Disk-lästa byte/sek <br> Disk-skrivna byte/s |kB_read/s <br> kB_wrtn/s |
| **Svarstid** |Total tid toocomplete en disk-i/o-begäran. |Medel s/diskläsning <br> Medel s/diskskrivning |await <br> svctm |
| **I/o-storlek** |hello storleken på i/o-begäranden utfärdar toohello diskar med lagringsutrymme. |Genomsnittligt antal byte/diskläsning <br> Genomsnittlig Disk byte/skrivning |avgrq sz |
| **Ködjup** |Antal utestående i/o-begäranden väntar toobe läsas formuläret eller skrivas toohello Lagringsdisken. |Aktuell diskkölängd |avgqu sz |
| **Max. Minne** |Mängden minne som krävs för toorun program smidigt |% Använda dedikerade byte |Använd vmstat |
| **Max. CPU** |Mängden CPU krävs toorun program smidigt |% Processortid |% util |

Lär dig mer om [iostat](http://linuxcommand.org/man_pages/iostat1.html) och [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).

## <a name="optimizing-application-performance"></a>Optimera programprestanda för
hello faktorer som påverkar prestanda för ett program som körs på Premium-lagring är natur av i/o-begäranden storlek på Virtuellt minne, diskutrymme, antalet diskar diskcachelagring, Multithreading och ködjup. Du kan kontrollera vissa av dessa faktorer med rattar som tillhandahålls av hello system. De flesta program kanske inte ger ett alternativ tooalter hello i/o-storlek och ködjup direkt. Om du använder SQL Server kan välja du inte hello-i/o-storlek och kön djup. SQL Server väljer hello optimala IO storlek och kön djup värden tooget hello de flesta prestanda. Det är viktigt toounderstand hello effekterna av båda typerna av faktorer på ditt programprestanda så att du kan etablera lämpliga resurser för toomeet prestandabehov.

I det här avsnittet finns toohello program krav checklista som du skapade tooidentify annuitet toooptimize programmets prestanda. Baserat på att du kommer att vara kan toodetermine behöver som faktorer från det här avsnittet tootune. toowitness hello effekterna av varje faktor på ditt programprestanda, köra prestandamätningar verktyg på programinstallationen. Se toohello [Benchmarking](#Benchmarking) avsnittet hello slutet av den här artikeln för steg toorun vanliga prestandamätningar verktyg i Windows och Linux virtuella datorer.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>Optimera IOPS, genomflöde och svarstid i korthet
hello tabellen nedan sammanfattar alla hello prestandafaktorer och hello steg toooptimize IOPS, genomflöde och svarstid. hello den här sammanfattningen i följande avsnitt beskriver varje faktorn är mycket mer djup.

| &nbsp; | **IOPS** | **Dataflöde** | **Svarstid** |
| --- | --- | --- | --- |
| **Exempelscenario** |Enterprise OLTP-program som kräver mycket hög transaktioner per andra hastighet. |Enterprise Data warehousing programmet bearbetning stora mängder data. |Nära realtidsprogram som kräver omedelbar svar toouser begäranden som onlinespel. |
| Prestandafaktorer | &nbsp; | &nbsp; | &nbsp; |
| **I/o-storlek** |Mindre i/o-storlek ger högre IOPS. |Större tooyields för i/o-storlek högre genomströmning. | &nbsp;|
| **VM-storlek** |Använda en VM-storlek som erbjuder IOPS som är större än din programkrav. Se VM-storlekar och deras här IOPS-gränser. |Använda en VM-storlek med genomflödet gräns som är större än din programkrav. Se VM-storlekar och deras genomströmning gränser här. |Använda en VM-storlek att erbjudanden skala gränser som är större än din programkrav. Se VM-storlekar och deras gränser här. |
| **Diskens storlek** |Använd storleken för en disk som erbjuder IOPS som är större än din programkrav. Visa storlekar för diskar och deras här IOPS-gränser. |Använd storleken för en disk med genomflödet gräns som är större än din programkrav. Se storlekar för diskar och deras genomströmning gränser här. |Använd en diskstorlek så att erbjudanden skala gränser som är större än din programkrav. Visa storlekar för diskar och deras gränser här. |
| **VM- och Skalningsgränser för Disk** |IOPS gränsen för vald för hello VM-storlek måste vara större än IOPS totalt styrs av premium lagringsdiskar kopplade tooit. |Genomströmning gränsen för vald för hello VM-storlek måste vara större än det totala genomflödet som drivs av premium lagringsdiskar kopplade tooit. |Gränser för vald för hello VM-storlek måste vara större än totala gränser för bifogade premium storage diskar. |
| **Cachelagring på disk** |Aktivera ReadOnly-cachen på premium storage diskar med Läs tunga operations tooget högre Läs IOPS. | &nbsp; |Aktivera ReadOnly Cache på premium storage diskar med redo tunga operations tooget mycket låg Läs latens. |
| **Disk-Striping** |Använda flera diskar och stripe dem tillsammans tooget en kombinerad högre IOPS och genomströmning gräns. Observera att hello kombinerade gränsen per VM bör vara högre än hello kombinerade gränser för bifogade premiumdiskar. | &nbsp; | &nbsp; |
| **Stripe-storlek** |Mindre stripe-storlek för slumpmässiga små i/o-mönster visas i OLTP-program. T.ex. använda stripe storlek på 64KB för SQL Server OLTP-program. |Större stripe-storlek för sekventiella stora i/o-mönster i datalagret program. T.ex. använda 256KB stripe-storlek för SQL Server-Data warehouse program. | &nbsp; |
| **Flertrådsteknik** |Använda flertrådsteknik toopush högre antal begäranden tooPremium lagring som dirigerar toohigher IOPS och genomflöde. Exempelvis på SQL Server ställer in en hög MAXDOP värdet tooallocate flera processorer tooSQL Server. | &nbsp; | &nbsp; |
| **Ködjup** |Större ködjup ger högre IOPS. |Större ködjup ger högre genomströmning. |Mindre ködjup ger lägre latens. |

## <a name="nature-of-io-requests"></a>Typ av i/o-begäranden
Ett i/o-begäran är en enhet av i/o-åtgärd som programmet kommer att utföra. Identifiera hello arten av i/o-begäranden, slumpmässiga eller sekventiella, läsa eller skriva små eller stora, hjälper dig att avgöra hello prestandakraven för ditt program. Det är mycket viktigt toounderstand hello arten av i/o-begäranden, toomake hello rätt beslut när du designar infrastrukturen för programmet.

I/o-storlek är en av hello viktigare faktorer. hello i/o-storlek är hello indata-/ utdata-begäran som skapats av programmet hello storlek. hello i/o-storlek har en betydande inverkan på prestanda särskilt på hello IOPS och bandbredd som hello program är kan tooachieve. hello följande formel visar hello förhållandet mellan IOPS, i/o-storlek och bandbredd/genomflöde.  
    ![](media/storage-premium-storage-performance/image1.png)

Vissa program tillåter tooalter sina IO storlek, medan vissa program inte. Till exempel SQL Server anger hello optimala IO storleken på själva och ger inte användare med alla rattar toochange den. Hej på andra sidan, Oracle innehåller en parameter med namnet [DB\_BLOCKERA\_storlek](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) med vilket du kan konfigurera hello i/o-begäran storleken på hello-databasen.

Om du använder ett program som inte tillåter du toochange hello i/o-storlek, Använd hello riktlinjerna i den här artikeln toooptimize hello prestanda KPI: er som är mest relevant tooyour program. Exempel:

* En OLTP-program genererar miljontals små och slumpmässiga i/o-begäranden. toohandle dessa typ av i/o-begäranden, måste du utformar din infrastruktur tooget för programmet högre IOPS.  
* Ett informationslager program genererar stora och sekventiella i/o-begäranden. toohandle dessa typ av i/o-begäranden, du måste skapa ditt program infrastruktur tooget högre bandbredd eller dataflöde.

Om du använder ett program som gör att du toochange hello-i/o-storlek, Använd den här tumregel för hello IO storlek dessutom tooother prestanda riktlinjer

* Mindre i/o storlek tooget högre IOPS. Till exempel 8 KB för ett OLTP-program.  
* Större tooget för i/o-storlek högre bandbredd/genomflöde. Till exempel 1024 KB för ett data warehouse-program.

Här är ett exempel på hur du kan beräkna hello IOPS och genomströmning/bandbredd för ditt program. Överväg att ett program som använder en P30 disk. hello högsta IOPS och genomströmning/bandbredd en P30 disk kan uppnå är 5 000 IOPS och 200 MB per sekund respektive. Om ditt program kräver hello högsta IOPS från hello P30 disk och du använder en mindre i/o-storlek som 8 KB hello resulterande bandbredd som du kommer att är kunna tooget nu 40 MB per sekund. Om ditt program kräver hello maximal dataflödet/bandbredd från P30 disk, och du använder en större i/o-storlek som 1024 KB, hello resulterande IOPS är dock mindre, 200 IOPS. Finjustera därför hello-i/o-storlek så att den uppfyller båda programmets IOPS och genomströmning/bandbredd. Tabellen nedan sammanfattar hello olika-i/o-storlekar och deras motsvarande IOPS och genomströmning för en P30 disk.

| Programkrav | I/o-storlek | IOPS | Genomströmning/bandbredd |
| --- | --- | --- | --- |
| Maximalt antal IOPS |8 kB |5,000 |40 MB per sekund |
| Max genomflöde |1024 KB |200 |200 MB per sekund |
| Max Throughput + höga IOPS |64 kB |3,200 |200 MB per sekund |
| Max IOPS + högt genomflöde |32 KB |5,000 |160 MB per sekund |

använda flera premiumdiskar stripe tillsammans tooget IOPS och bandbredd som är högre än hello maximivärdet för en enskild premium storage-disk. Till exempel stripe-två P30 diskar tooget en kombinerad IOPS i 10 000 IOPS eller en kombinerad genomströmning på 400 MB per sekund. Som beskrivs i nästa avsnitt om hello, måste du använda en VM-storlek som stöder hello kombineras disk IOPS och genomflöde.

> [!NOTE]
> Som du ökar antingen IOPS eller också ökar dataflödet hello andra, kontrollera att du inte träffar genomströmning eller IOPS-gränser för hello disk eller VM om du vill öka någon.
>
>

toowitness hello effekterna av i/o-storlek på programmets prestanda kan du köra benchmarking verktyg på den Virtuella diskar. Skapa flera testkörningar och annan i/o-storlek för varje kör toosee hello påverkan. Se toohello [Benchmarking](#Benchmarking) avsnittet hello slutet av den här artikeln för mer information.

## <a name="high-scale-vm-sizes"></a>Hög skalning VM-storlekar
När du startar ett program, är en hello första saker toodo, Välj en VM-toohost ditt program. Premium-lagring har hög skala VM-storlekar som kan köra program som kräver högre datorkraft och en hög lokal disk i/o-prestanda. Dessa virtuella datorer ger snabbare processorer, minne till core förhållandet och en Solid-State Drive (SSD) för hello lokal disk. Exempel på hög skala virtuella datorer stöder Premium-lagring är hello DS, DSv2 och GS-serien virtuella datorer.

Hög skalning för virtuella datorer finns i olika storlekar med ett annat antal CPU-kärnor, minne, OS och tillfällig diskstorleken. Varje VM-storlek har också maximala antalet datadiskar som att du kan bifoga toohello VM. Därför påverkar hello valt VM-storlek hur mycket bearbetning, minne och lagringsutrymme kapacitet är tillgänglig för ditt program. Det påverkar också hello beräkning och lagringskostnaden. Nedan visas till exempel hello specifikationer av hello största VM-storlek i DS-serien, DSv2-serien och GS-serien:

| Storlek på virtuell dator | Processorkärnor | Minne | Storlekar för Virtuella diskar | Max. Datadiskar | Cachestorlek | IOPS | Gränser för Cache-i/o-bandbredd |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS14 |16 |112 GB |OS = 1 023 GB <br> Lokal SSD = 224 GB |32 |576 GB |50 000 IOPS <br> 512 MB per sekund |4 000 IOPS och 33 MB per sekund |
| Standard_GS5 |32 |448 GB |OS = 1 023 GB <br> Lokal SSD = 896 GB |64 |4224 GB |80 000 IOPS <br> 2 000 MB per sekund |5 000 IOPS och 50 MB per sekund |

tooview en fullständig lista över alla tillgängliga Azure VM-storlekar finns för[Windows VM-storlekar](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eller [Linux VM-storlekar](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Välj en VM-storlek som kan uppfylla och skala tooyour önskad programkrav för prestanda. Dessutom beakta toothis, följande viktiga överväganden när du väljer VM-storlekar.

*Gränser*  
hello högsta IOPS-gränser per virtuell dator och per disk är olika och oberoende av varandra. Kontrollera att programmet hello aktiverar IOPS inom hello gränser hello VM samt hello premium diskar anslutna tooit. Annars får programprestanda begränsning.

Anta exempelvis ett program som krävs är högst 4 000 IOPS. tooachieve detta får du etablera en P30 disk på en virtuell dator DS1. Hej P30 disk kan ge dig too5 000 IOPS. Hello DS1 VM är dock begränsad too3, 200 IOPS. Följaktligen hello programprestanda kommer att begränsas av hello VM-gränsen på 3,200 IOPS och det blir försämrade prestanda. tooprevent den här situationen kan välja en virtuell dator och diskstorlek som kommer båda uppfyller programkrav.

*Kostnaden för åtgärden*  
I många fall är det möjligt att den totala kostnaden för åtgärden med Premium-lagring är lägre än med standardlagring.

Tänk dig ett program som kräver 16 000 IOPS. tooachieve prestanda, behöver du en Standard\_D14 Azure IaaS VM, som kan ge högsta IOPS för 16000 med 32 standardlagring 1 TB diskar. Varje 1TB standardlagring disk kan uppnå högst 500 IOPS. hello uppskattade kostnaden för den här virtuella datorn per månad är $1,570. hello månadskostnaden för 32 standardlagring diskar blir $1,638. hello uppskattade totala månatliga kostnaden blir $3,208.

Men om du värd hello samma program på Premium-lagring, behöver du en mindre VM-storlek och färre premium storage diskar, vilket minskar den totala kostnaden hello. En Standard\_DS13 VM kan uppfylla hello 16000 IOPS krav med hjälp av fyra P30 diskar. Hej DS13 VM har högsta IOPS för 25,600 och varje P30 disk har högsta IOPS för 5 000. Generellt sett den här konfigurationen kan uppnå 5 000 x 4 = 20 000 IOPS. hello uppskattade kostnaden för den här virtuella datorn per månad är $1,003. hello månadskostnaden för fyra P30 premium lagringsdiskar blir $544.34. hello uppskattade totala månatliga kostnaden blir $1,544.

Tabellen nedan sammanfattar hello kostnadsuppdelning av det här scenariot för Standard- och Premium-lagring.

| &nbsp; | **Standard** | **Premium** |
| --- | --- | --- |
| **Kostnaden för VM per månad** |$1,570.58 (standard\_D14) |$1,003.66 (standard\_DS13) |
| **Av diskarnas kostnad per månad** |$1,638.40 (32 x 1 TB diskar) |$544.34 (4 x P30 diskar) |
| **Övergripande kostnader per månad** |$3,208.98 |$1,544.34 |

*Linux-distributioner*  

Med Azure Premium Storage kan du hämta hello samma nivå av prestanda för virtuella datorer som kör Windows och Linux. Vi stöder många varianter av Linux-distributioner och du kan se hello lista [här](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Det är viktigt toonote att olika distributioner är det bättre passar för olika typer av arbetsbelastningar. Du ser olika nivåer av prestanda beroende på hello distro din arbetsbelastning körs på. Testa hello Linux-distributioner med ditt program och välja hello som fungerar bäst.

När du kör Linux med Premium-lagring, kontrollera hello senaste uppdateringarna om nödvändiga drivrutiner tooensure hög prestanda.

## <a name="premium-storage-disk-sizes"></a>Premium-lagring diskstorlekar
Azure Premium Storage erbjuder sju diskstorlekar för närvarande. Varje diskstorleken har en annan skala gräns för IOPS, bandbredd och lagring. Välj hello rätt Premium Storage diskstorleken beroende på hello programkrav och hello hög skala VM-storlek. hello tabellen nedan visar hello sju diskar storlekar och deras funktioner. P4 och P6 storlek är för närvarande stöds endast för hanterade diskar.

| Premium diskar typ  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Diskstorlek           | 32 GB | 64 GB | 128 GB| 512 GB            | 1 024 GB (1 TB)    | 2 048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPS per disk       | 120   | 240   | 500   | 2 300              | 5000              | 7500              | 7500              | 
| Dataflöde per disk | 25 MB per sekund  | 50 MB per sekund  | 100 MB per sekund | 150 MB per sekund | 200 MB per sekund | 250 MB per sekund | 250 MB per sekund | 


Hur många diskar som du väljer beror på hello disk storlek som väljs. Du kan använda en enskild p 50 disk eller flera P10 diskar toomeet din programkrav. Beakta överväganden för användarkonton nedan när du gör hello val.

*Gränser (IOPS och genomströmning)*  
hello IOPS och genomströmning gränserna för varje Premium-diskstorleken är olika och oberoende från hello VM gränser. Se till att hello IOPS totalt och genomströmning från hello diskar är inom skalningsgränser hello valt VM-storlek.

Om exempelvis en programkrav är högst 250 MB/s genomströmning och du använder en DS4 virtuell dator med en enda P30 disk. Hej DS4 VM kan ge dig too256 MB/s genomströmning. Men har en enda disk P30 genomströmning högst 200 MB per sekund. Hello programmet kommer därför begränsade på 200 MB per sekund på grund av toohello disk gränsen. tooovercome den här gränsen kan etablera mer än en data diskar toohello VM eller ändra storlek på din diskar tooP40 eller p 50.

> [!NOTE]
> Läsningar som hanteras av hello cache ingår inte i hello disk IOPS och dataflöde, därför som inte omfattas av toodisk gränser. Cachen har separata IOPS och genomströmning gränsen per virtuell dator.
>
> Till exempel är ursprungligen din läsningar och skrivningar 60MB per sekund och 40MB per sekund. Över tiden, hello cache värmer upp och fungerar i hello läser från hello cache. Sedan kan du skriva högre genomströmning från hello disken.
>
>

*Antal diskar*  
Fastställa hello antalet diskar som du behöver genom att bedöma programkrav. Varje VM-storlek har också en gräns på hello antalet diskar som du kan koppla toohello VM. Detta är vanligtvis två gånger hello antal kärnor. Se till att hello VM-storlek som du väljer kan stödja hello antalet diskar som behövs.

Kom ihåg att hello Premium-lagring diskar har högre prestanda jämfört med funktioner tooStandard diskar med lagringsutrymme. Om du migrerar programmet från Azure IaaS-VM med standardlagring tooPremium lagring, måste du därför kommer troligen färre premiumdiskar tooachieve hello samma eller högre prestanda för ditt program.

## <a name="disk-caching"></a>Cachelagring på disk
Hög skalning VMs som utnyttjar Azure Premium-lagring har en flera nivåer cachelagring teknik som kallas BlobCache. BlobCache använder en kombination av hello RAM för virtuella och lokala SSD för cachelagring. Det här cacheminnet är tillgänglig för hello Premium-lagring beständiga och hello VM lokala diskar. Den här inställningen är som standard tooRead/skrivning för OS-diskar och ReadOnly för datadiskar som finns på Premium-lagring. Med diskcachelagring aktiverad på hello Premium-lagring diskar, uppnå hello virtuella datorer med hög skala mycket hög prestanda som överskrider hello underliggande diskprestanda.

> [!WARNING]
> Ändra hello cache-inställningen för ett Azure-disken kopplas från och nytt bifogar hello måldisken. Om det är hello operativsystemdisk startas hello VM. Stoppa alla program/tjänster som kan påverkas av den här avbrott innan du ändrar hello diskcache-inställningen.
>
>

toolearn mer information om hur BlobCache fungerar, se toohello i [Azure Premium Storage](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) blogginlägg.

Det är viktigt tooenable cachen på hello rätt uppsättning diskar. Om du ska aktivera cachelagring på disk på en disk i premium eller inte beror på hello arbetsbelastning mönster disken hanterar. Tabellen nedan visar hello standard inställningar för cachelagring för Operativsystemet och datadiskarna.

| **Typ av disk** | **Standardinställning för Cache** |
| --- | --- |
| OS-disk |ReadWrite |
| Datadisk |Ingen |

Följande är hello inställningar för cachelagring av rekommenderas för datadiskar,

| **Disken cacheinställning** | **Rekommendation om när toouse inställningen** |
| --- | --- |
| Ingen |Konfigurera värd-cachen eftersom ingen för lässkyddad och skrivintensiv diskar. |
| Skrivskyddad |Konfigurera värd-cache som ReadOnly för skrivskyddade och läsa / skriva-diskar. |
| ReadWrite |Konfigurera värd-cache ReadWrite bara om programmet hanterar korrekt skrivning cachelagrade toopersistent datadiskar vid behov. |

*Skrivskyddad*  
Genom att konfigurera ReadOnly cachelagring på Premium-lagring data diskar kan du uppnå Läs fördröjning och hämta mycket hög Läs IOPS och genomströmning för ditt program. Detta är på grund av två skäl

1. Läser utföras från cache, som finns på hello datorminnet och lokala SSD, är mycket snabbare än läsningar av hello datadisk som är på hello Azure blob storage.  
2. Premium-lagring räknas inte hello läsningar hanteras från cache, ett hello disk IOPS och genomflöde. Programmet är därför kan tooachieve högre total IOPS och genomflöde.

*ReadWrite*  
Som standard har hello OS diskar ReadWrite cachelagring aktiverat. Vi har lagt till stöd för ReadWrite cachelagring på data samt diskar. Om du använder ReadWrite cachelagring, måste du ha ett korrekt sätt toowrite hello data från cache toopersistent diskar. Till exempel cachelagras SQLServer hanterar skrivning datadiskar toohello beständig lagring på sin egen. Med ett program som inte hanterar bestående hello ReadWrite cache krävs data kan leda toodata dataförlust om hello VM kraschar.

Exempelvis kan du använda dessa riktlinjer tooSQL Server körs på Premium-lagring genom att göra hello följande:

1. Konfigurera ”ReadOnly” cache på premium storage diskar som är värd för filer.  
   a.  hello snabb läser från cache lägre hello SQL Server Frågetid eftersom datasidor mycket snabbare hämtas från hello cache jämfört med toodirectly från hello datadiskar.  
   b.  Hanterar läser från cache, innebär ytterligare genomflöde är tillgänglig från premium datadiskar. SQL Server kan använda den här ytterligare genomflöde mot hämta mer datasidor och andra åtgärder som säkerhetskopiering/återställning batch belastningar och index återskapas.  
2. Konfigurera ”None”-cachen på premium-lagring diskar värd hello-loggfilerna.  
   a.  Loggfilerna har främst skrivintensiv åtgärder. Därför kan får de inte hello ReadOnly-cachen.

## <a name="disk-striping"></a>Disk-Striping
När en hög skala VM är kopplade till flera premium storage beständiga diskar, hello diskar kan vara stripe tillsammans tooaggregate sina IOPs och bandbredd lagringskapacitet.

I Windows, kan du använda lagringsutrymmen toostripe diskar tillsammans. Du måste konfigurera en kolumn för varje disk i en pool. Annars kan hello prestandan stripe-volym vara lägre än väntat, på grund av toouneven fördelning av trafik över hello diskar.

Viktigt: Använda Server Manager UI kan ange du hello Totalt antal kolumner in too8 för stripe-volymer. När du bifogar mer än 8 diskar, använder du PowerShell toocreate hello volym. Med hjälp av PowerShell, kan du ange hello antalet kolumner lika toohello antal diskar. Till exempel om det finns 16 diskar i en enda stripe-uppsättning; Ange 16 kolumner i hello *NumberOfColumns* parametern för hello *New-VirtualDisk* PowerShell-cmdlet.

Använd hello MDADM verktyget toostripe diskar tillsammans på Linux. Detaljerade anvisningar på striping diskar på Linux finns för[konfigurera programvara RAID på Linux](../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

*Stripe-storlek*  
En viktig konfiguration i disken striping är hello stripe-storlek. hello stripe-storlek eller blockstorleken är hello minsta datasegmentet som programmet kan åtgärda på stripe-volymer. hello stripe-storlek som du konfigurerar beror på hello typ av program och dess mönster i begäran. Om du väljer hello fel stripe-storlek kan leda det tooIO feljusteringarna, vilket leder toodegraded prestanda för ditt program.

Till exempel om en i/o-begäran som genererats av ditt program är större än hello stripe diskstorleken skriver hello lagringssystemet den över stripe enhet gränser på mer än en disk. När det är tid tooaccess data har tooseek över flera stripe enheter toocomplete hello begäran. hello kumulativa effekten av sådana beteende kan leda toosubstantial prestanda försämras. På hello disk däremot om hello IO begära storlek är mindre än stripe och om det är slumpmässigt hello-i/o-begäranden kan lägga på hello samma orsakar en flaskhals och slutligen försämring hello-i/o-prestanda.

Beroende på hello typ av arbetsbelastning som programmet körs, väljer du en lämplig stripe-storlek. Använda en mindre stripe-storlek för slumpmässiga små i/o-begäranden. Medan för stora sekventiella i/o-begäranden för att använda en större stripe. Ta reda på hello stripe storlek rekommendationer för hello programmet körs på Premium-lagring. Konfigurera stripe storlek på 64KB för OLTP-arbetsbelastningar och 256KB för data datalagring arbetsbelastningar för SQL Server. Se [prestandarelaterade Metodtips för SQL Server på Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md#disks-guidance) toolearn mer.

> [!NOTE]
> Du kan stripe-tillsammans högst 32 premium storage på DS-serien VM och 64 premium storage diskar på GS-serien VM.
>
>

## <a name="multi-threading"></a>Flertrådsteknik
Premium-lagring plattform toobe massivt parallell har utformats för Azure. Ett flertrådat program uppnår därför mycket högre prestanda än en enkeltrådig program. Ett flertrådat program delar upp sina uppgifter över flera trådar och ökar effektiviteten för körningen av genom att utnyttja hello VM och disk resurser toohello maximalt.

Om tillämpningsprogrammet körs på en enda kärna VM med två trådar, till exempel växla hello CPU mellan hello två trådar tooachieve effektivitet. När en tråd väntar på en disk-i/o-toocomplete växla hello CPU toohello annan tråd. På så sätt kan kan två trådar göra mer än en enkel tråd. Om hello VM har mer än en kärna, minskar ytterligare körtiden eftersom varje kärna kan köra uppgifter parallellt.

Du kanske inte kan toochange hello sätt ett färdigt program implementerar enda trådade eller flertrådsteknik. SQL Server är kan hantera flera processorer och flera kärnor. Dock beslutar SQL Server under vilka förhållanden den använder en eller flera trådar tooprocess en fråga. Den kan köra frågor och skapa index med flertrådsteknik. SQL Server för en fråga som inbegriper stora tabeller och sortera data innan det returneras toohello användaren att sannolikt använda flera trådar. En användare kan dock styra om SQL Server kör en fråga med hjälp av en enskild tråd eller flera trådar.

Det finns inställningar för att du kan ändra tooinfluence följande flertrådsteknik eller parallell bearbetning av ett program. Till exempel om SQL Server är det hello maximal grad av parallellitet konfiguration. Den här inställningen som kallas MAXDOP, kan du tooconfigure hello maximalt antal processorer som kan användas vid bearbetning av parallell i SQL Server. Du kan konfigurera MAXDOP för enskilda frågor eller Indexåtgärder. Det här är bra när du vill toobalance resurser i systemet för prestanda kritiska program.

Anta exempelvis att ditt program som använder SQL Server körs på en stor fråga och ett indexåtgärden vid hello samtidigt. Låt oss anta att du vill ha hello index åtgärden toobe mer performant jämfört med toohello stora frågan. I så fall kan ange du MAXDOP värde hello index åtgärden toobe högre än hello MAXDOP värdet för hello frågan. På så sätt kan SQL Server har fler processorer som den kan använda för hello index åtgärden jämfört med toohello antal processorer som den kan dedikera toohello stora frågan. Kom ihåg att du inte styra hello antalet trådar som ska användas för varje åtgärd i SQL Server. Du kan styra hello maximalt antal processorer som är dedikerad för flertrådsteknik.

Lär dig mer om [grader parallellitet](https://technet.microsoft.com/library/ms188611.aspx) i SQL Server. Ta reda på sådana inställningar som påverkar flertrådsteknik i ditt program och konfigurationer toooptimize prestanda.

## <a name="queue-depth"></a>Ködjup
Hej ködjup eller Kölängd köstorlek är hello antalet väntande i/o-begäranden i hello system. hello-värdet för ködjup bestämmer hur många i/o åtgärder tillämpningsprogrammet kan Rada upp, vilka hello lagringsdiskar ska bearbetas. Den påverkar alla hello tre program nyckeltal som beskrevs i d.v.s. på den här artikeln, IOPS, genomflöde och svarstid.

Kö djup och flertrådsteknik är nära relaterade. hello ködjup värdet anger hur mycket flertrådsteknik som kan uppnås av programmet hello. Om hello ködjup är stor, programmet kan köra flera åtgärder samtidigt, med andra ord mer flertrådsteknik. Om hello ködjup är litet, även om programmet är flertrådade, har inte tillräckligt med begäranden som väntar på samtidig körning.

Vanligtvis av hello kommersiellt tillgängliga program ger dig inte toochange hello ködjup, eftersom om angetts felaktigt den gör mer skada än bra. Program ska ange hello rätt värde kön djup tooget hello optimala prestanda. Men det är viktigt toounderstand detta begrepp så att du kan felsöka problem med prestanda med ditt program. Du kan också se hello effekterna av ködjup genom att köra benchmarking verktyg på datorn.

Vissa program innehåller inställningar för tooinfluence hello ködjup. Till exempel beskrivs hello MAXDOP (högsta grad av parallellitet) inställningen i SQL Server i föregående avsnitt. MAXDOP är ett sätt tooinfluence ködjup och flertrådsteknik, även om den inte direkt ändras hello ködjup värdet för SQL Server.

*Hög ködjup*  
En hög ködjup skrivs flera åtgärder på hello disk. hello disk vet hello nästa begäran i kön i förväg. Följaktligen hello disk schemalägga åtgärder i förväg och bearbeta dem i en optimal sekvens. Eftersom programmet hello skickar flera begäranden toohello disk, kan hello disk bearbeta flera parallella IOs. Slutligen hello programmet kommer att kunna tooachieve högre IOPS. Eftersom programmet bearbetar fler begäranden ökar hello totala genomflödet av programmet hello också.

Vanligtvis ett program kan uppnå maximalt dataflöde med 8-16 + utestående I/o per ansluten disk. Om ett ködjup är en, tillräckligt med IOs toohello system gör inte att program och mindre mängd bearbetas under en viss period. Med andra ord mindre genomflöde.

Till exempel i SQL Server, inställningen hello MAXDOP värdet för en fråga för ”4” meddelar SQL Server som kan användas på toofour kärnor tooexecute hello fråga. SQL Server avgör vad är bästa kön djup värdet och hello antalet kärnor för hello frågekörning.

*Optimalt ködjup*  
Mycket hög kön djup värdet har även dess nackdelar. Om kön djup värde är för högt hello program försöker toodrive mycket hög IOPS. Om programmet har beständiga diskar med tillräckligt etablerade IOPS, kan detta påverka programväntetider. Följande formel visar hello förhållandet mellan IOPS, svarstid och ködjup.  
    ![](media/storage-premium-storage-performance/image6.png)

Du bör inte konfigurera ködjup tooany högt värde, men tooan optimala-värde som kan ge tillräckligt med IOPS för hello programmet utan att påverka svarstider. Till exempel om hello programmet fördröjning måste toobe 1 millisekund, hello ködjup krävs tooachieve 5 000 IOPS är QD = 5000 x 0,001 = 5.

*Ködjup för stripe-volym*  
Upprätthålla ett tillräckligt högt ködjup för stripe-volymer så att alla diskar har en topp ködjup individuellt. Anta till exempel att ett program som skickar ett ködjup på 2 och det finns 4 diskar i hello stripe. hello två-i/o-begäranden skickas tootwo diskar och återstående två diskar blir inaktiv. Därför konfigurera hello ködjup så att alla hello diskar kan vara upptaget. Formeln nedan visar hur toodetermine hello ködjup på stripe-volymer.  
    ![](media/storage-premium-storage-performance/image7.png)

## <a name="throttling"></a>Begränsning
Azure bestämmelserna Premium-lagring antal IOPS och genomflöde för beroende på hello VM-storlekar och storlekar för diskar som du väljer. När programmet försöker toodrive IOPS eller dataflöde ovan dessa begränsningar för vilka hello VM eller disk kan hantera, det med begränsning av Premium-lagring. Detta visar i hello form av försämrade prestanda i ditt program. Detta kan innebära högre latens, sänka genomströmning eller lägre IOPS. Om Premium-lagring inte begränsning, misslyckas programmet helt av mer än vad dess resurser är möjligt att uppnå. Därför tooavoid prestandaproblem på grund av toothrottling alltid etablera tillräckligt med resurser för ditt program. Ta hänsyn till vi diskuterade i hello VM-storlekar och Disk storlekar ovan. Prestandamätningar är hello bästa sätt toofigure vilka resurser du behöver toohost ditt program.

## <a name="benchmarking"></a>Prestandamätningar
Prestandamätningar är hello process för att simulera olika arbetsbelastningar för ditt program och mäta hello programprestanda för varje arbetsbelastning. Med hello stegen som beskrivs i ett tidigare avsnitt kan du har samlat in hello programkrav för prestanda. Genom att köra prestandamätningar hello VMs som är värd för programmet hello-verktyg kan bestämma du hello prestandanivåer som programmet kan uppnå med Premium-lagring. I det här avsnittet får du exempel på en Standard DS14 VM etablerats med Azure Premium Storage diskar prestandamätningar.

Vi har använt gemensamma benchmarking verktyg Iometer och FIO, för Windows och Linux respektive. Dessa verktyg starta flera trådar simulera en produktion som arbetsbelastning och mått hello systemets prestanda. Med hjälp av hello verktyg kan du också konfigurera parametrar som block storlek och kön djup, vilket normalt inte kan ändra för ett program. Det ger mer flexibilitet toodrive hello maximala prestanda på hög nivå VM etablerats med premiumdiskar för olika typer av arbetsbelastningar för program. Mer om varje benchmarking verktyg finns toolearn [Iometer](http://www.iometer.org/) och [FIO](http://freecode.com/projects/fio).

toofollow hello exemplen nedan, skapa en Standard DS14 virtuell dator och koppla 11 Premium-lagring diskar toohello VM. Konfigurera 10 diskar med värdcachelagring som ”None” hello 11 diskar och stripe-dem till en volym med namnet NoCacheWrites. Konfigurera cachelagring som ”skrivskyddad” på hello återstående disken värden och skapa en volym med namnet CacheReads med den här disken. Med den här inställningen kommer du att kunna toosee hello maximala läsning och skrivning prestanda från en Standard DS14 virtuell dator. Detaljerade anvisningar om hur du skapar en virtuell dator DS14 med premiumdiskar gå för[skapa och använda en Premium-lagring konto för en virtuell dator datadisk](storage-premium-storage.md).

*Värma upp hello Cache*  
hello disk med cachelagring av ReadOnly-värden kommer att kunna toogive högre IOPS än hello disk gränsen. tooget läsa den här högsta prestanda från hello värden cache, du måste först värmt upp hello cache för den här disken. Detta säkerställer att hello Läs IOs vilket benchmarking verktyg kommer att driva på CacheReads volym faktiskt träffar hello cachen och inte hello disk direkt. hello cacheträffar resulterar i ytterligare IOPS från hello enda cache aktiverat disk.

> **Viktigt:**  
> Du måste värmt upp hello cachen innan du kör prestandatester, varje gång VM startas.
>
>

#### <a name="iometer"></a>Iometer
[Hämta hello Iometer](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) på hello VM.

*Testa fil*  
Iometer använder en testfil som lagras på hello volym som du vill köra hello prestandamätningar test. Den styr läsningar och skrivningar på den här testa toomeasure hello disken IOPS och genomflöde. Iometer skapar den här Testfilen om du inte har angett något. Skapa en fil för test av 200GB kallas iobw.tst på hello CacheReads och NoCacheWrites volymer.

*Specifikationer för Access*  
hello specifikationer, begär i/o-storlek, % läsning/skrivning, % slumpmässiga/sekventiella konfigureras med hjälp av hello ”specifikationer Access” fliken i Iometer. Skapa en åtkomst-specifikation för varje hello-scenarier som beskrivs nedan. Skapa hello åtkomst specifikationer och ”spara” med ett lämpligt namn som – RandomWrites\_8 kB RandomReads\_8 kB. Välj motsvarande hello-specifikationen när du kör hello Testscenario.

Ett exempel på åtkomst specifikationer för högsta IOPS skriva scenario visas nedan,  
    ![](media/storage-premium-storage-performance/image8.png)

*Specifikationer för högsta IOPS-Test*  
toodemonstrate högsta IOPs, använda mindre storlek på begäran. Använd 8 kB begära storlek och skapa specifikationer för slumpmässiga skrivningar och läsningar.

| Specifikation för åtkomst | Begära storlek | Slumpmässiga % | Läs % |
| --- | --- | --- | --- |
| RandomWrites\_8 kB |8 KB |100 |0 |
| RandomReads\_8 kB |8 KB |100 |100 |

*Maximalt dataflöde Test specifikationer*  
toodemonstrate maximalt dataflöde, Använd större storlek i begäran. Använd 64K begära storlek och skapa specifikationer för slumpmässiga skrivningar och läsningar.

| Specifikation för åtkomst | Begära storlek | Slumpmässiga % | Läs % |
| --- | --- | --- | --- |
| RandomWrites\_64 kB |64 KB |100 |0 |
| RandomReads\_64 kB |64 KB |100 |100 |

*Kör hello Iometer Test*  
Utför hello steg nedan toowarm upp cache

1. Skapa två åtkomst specifikationer med värden som visas nedan,

   | Namn | Begära storlek | Slumpmässiga % | Läs % |
   | --- | --- | --- | --- |
   | RandomWrites\_1 MB |1MB |100 |0 |
   | RandomReads\_1 MB |1MB |100 |100 |
2. Köra hello Iometer test för att initiera cache disk med följande parametrar. Använd tre trådar för hello målvolymen och ett ködjup på 128. Ange hello hello test too2hrs på hello ”Test” inställningsfliken ”körningstiden” varaktighet.

   | Scenario | Målvolymen | Namn | Varaktighet |
   | --- | --- | --- | --- |
   | Initiera Cache-Disk |CacheReads |RandomWrites\_1 MB |2hrs |
3. Köra hello Iometer test för uppvärmning cache disk med följande parametrar. Använd tre trådar för hello målvolymen och ett ködjup på 128. Ange hello hello test too2hrs på hello ”Test” inställningsfliken ”körningstiden” varaktighet.

   | Scenario | Målvolymen | Namn | Varaktighet |
   | --- | --- | --- | --- |
   | Varm upp Cache-Disk |CacheReads |RandomReads\_1 MB |2hrs |

När cachen disk är varmkörts fortsätter du med hello testscenarier som anges nedan. toorun hello Iometer test, Använd minst tre trådar för **varje** mål volym. För varje arbetstråd hello målvolymen, ange ködjup och markerar en hello sparade test specifikationer som visas i hello tabellen nedan toorun hello motsvarande test-scenario. hello visas också förväntat resultat för IOPS och dataflöde när du kör testerna. För alla scenarier är används en små i/o-storlek på 8KB och en hög ködjup på 128.

| Testscenario | Målvolymen | Namn | Resultat |
| --- | --- | --- | --- |
| Max. Läs IOPS |CacheReads |RandomWrites\_8 kB |50 000 IOPS |
| Max. Skriva IOPS |NoCacheWrites |RandomReads\_8 kB |64 000 IOPS |
| Max. Kombinerade IOPS |CacheReads |RandomWrites\_8 kB |100 000 IOPS |
| NoCacheWrites |RandomReads\_8 kB | &nbsp; | &nbsp; |
| Max. Läsa MB per sekund |CacheReads |RandomWrites\_64 kB |524 MB per sekund |
| Max. Skriva MB per sekund |NoCacheWrites |RandomReads\_64 kB |524 MB per sekund |
| Kombinerade MB per sekund |CacheReads |RandomWrites\_64 kB |1 000 MB per sekund |
| NoCacheWrites |RandomReads\_64 kB | &nbsp; | &nbsp; |

Nedan visas skärmdumpar av hello Iometer testresultaten för kombinerade IOPS och genomflöde.

*Kombinerade läsningar och skrivningar högsta IOPS*  
![](media/storage-premium-storage-performance/image9.png)

*Kombinerade läsningar och skrivningar maximalt dataflöde*  
![](media/storage-premium-storage-performance/image10.png)

### <a name="fio"></a>FIO
FIO är en populär verktyget toobenchmark lagring på hello virtuella Linux-datorer. Den har hello-flexibilitet tooselect olika-i/o-storlek, sekventiella eller slumpmässig läsning och skrivning. Det skapas trådar eller processer tooperform hello angetts i/o-åtgärder. Du kan ange hello typ av i/o-åtgärder som varje arbetstråd måste utföra med hjälp av jobbfiler. Vi har skapat en fil per scenariot som illustreras i hello exemplen nedan. Du kan ändra hello specifikationer i dessa jobb filer toobenchmark olika arbetsbelastningar som körs på Premium-lagring. Vi använder en Standard DS 14 VM som körs i hello exempel **Ubuntu**. Använd hello samma inställningar som beskrivs i hello början av hello [prestandamätningar avsnittet](#Benchmarking) och Varm in hello cachen innan du kör hello prestandamätningar tester.

Innan du börjar [hämta FIO](https://github.com/axboe/fio) och installera den på den virtuella datorn.

Kör följande kommando för Ubuntu, hello

```
apt-get install fio
```

Vi använder fyra trådar för att föra skrivåtgärder och fyra trådar för intresseväckande läsåtgärder på hello diskar. hello skrivåtgärder arbetare kommer att driva trafiken på hello ”nocache” volymen, som innehåller 10 diskar med cache som angetts för ”None”. hello Läs arbetare kommer att driva trafiken på hello ”readcache” volymen, som har 1 disk med cache för ”ReadOnly”.

*Maximal skrivåtgärder IOPS*  
Skapa hello jobbet fil med följande specifikationer tooget högsta skriva IOPS. Ge den namnet ”fiowrite.ini”.

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Obs hello Följ viktiga saker som följer hello designriktlinjer som beskrivs i föregående avsnitt. Dessa specifikationer är mycket viktigt toodrive högsta IOPS  

* En hög ködjup på 256.  
* En liten blockstorlek 8 kB.  
* Flera trådar utför slumpmässiga skrivningar.

Kör hello efter kommandot tookick av hello FIO testa i 30 sekunder  

```
sudo fio --runtime 30 fiowrite.ini
```

Medan hello testet körs, kommer du att kunna toosee hello antalet IOPS hello VM- och Premium diskar levererar. I hello exemplet nedan visas hello DS14 VM levererar sin maximala skrivåtgärder IOPS högst 50 000 IOPS.  
    ![](media/storage-premium-storage-performance/image11.png)

*Maximal Läs IOPS*  
Skapa hello jobbet fil med följande specifikationer tooget högsta IOPS för läsning. Ge den namnet ”fioread.ini”.

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Obs hello Följ viktiga saker som följer hello designriktlinjer som beskrivs i föregående avsnitt. Dessa specifikationer är mycket viktigt toodrive högsta IOPS

* En hög ködjup på 256.  
* En liten blockstorlek 8 kB.  
* Flera trådar utför slumpmässiga skrivningar.

Kör hello efter kommandot tookick av hello FIO testa i 30 sekunder

```
sudo fio --runtime 30 fioread.ini
```

Medan hello testet körs, kommer du att kunna toosee hello antal lästa IOPS hello VM och levererar premiumdiskar. I hello exemplet nedan visas hello DS14 VM leverera med fler än 64 000 Läs IOPS. Detta är en kombination av hello disk och hello cache prestanda.  
    ![](media/storage-premium-storage-performance/image12.png)

*Maximal läsning och skrivning IOPS*  
Skapa hello jobbet fil med följande specifikationer tooget maximala kombinerade läsa och skriva IOPS. Ge den namnet ”fioreadwrite.ini”.

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Obs hello Följ viktiga saker som följer hello designriktlinjer som beskrivs i föregående avsnitt. Dessa specifikationer är mycket viktigt toodrive högsta IOPS

* En hög ködjup på 128.  
* En liten blockstorlek 4KB.  
* Flera trådar som utför slumpmässig läsning och skrivning.

Kör hello efter kommandot tookick av hello FIO testa i 30 sekunder

```
sudo fio --runtime 30 fioreadwrite.ini
```

Medan hello testet körs, ska du vara kan toosee hello antal kombinerade läsa och skriva IOPS hello VM och levererar premiumdiskar. I hello exemplet nedan visas levererar hello DS14 VM med mer än 100 000 kombinerade läsa och skriva IOPS. Detta är en kombination av hello disk och hello cache prestanda.  
    ![](media/storage-premium-storage-performance/image13.png)

*Maximalt kombineras genomflöde*  
tooget hello maximala kombinerade läsa och skriva dataflöde, Använd en större blockstorlek och stora ködjup med flera trådar som utför läsningar och skrivningar. Du kan använda en blockstorlek på 64KB och ködjup på 128.

## <a name="next-steps"></a>Nästa steg
Lär dig mer om Azure Premium Storage:

* [Premium Storage: Lagring med höga prestanda för Azure Virtual Machines-arbetsbelastningar](storage-premium-storage.md)  

Läs artiklarna på bästa praxis för prestanda för SQL Server för SQL Server-användare:

* [Metodtips för prestanda för SQLServer på virtuella Azure-datorer](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md)
* [Azure Premium Storage ger högsta prestanda för SQL Server i Azure VM](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx)
