---
title: "Azure Site Recovery-kapacitetsplaneraren för Hyper-V till Azure| Microsoft Docs"
description: "I den här artikeln beskrivs analysen av genererade rapporter för Azure Site Recovery-kapacitetsplaneraren för Hyper-V till Azure-scenariot."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/02/2017
ms.author: nisoneji
ms.openlocfilehash: 9340fe48c1da874d6c0cf02c026e5dec6ddabbe7
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/15/2017
---
# <a name="analyze-the-azure-site-recovery-deployment-planner-report"></a>Analysera rapporten för Azure Site Recovery-kapacitetsplaneraren
Den genererade rapporten i Microsoft Excel innehåller följande ark:

## <a name="on-premises-summary"></a>Lokal sammanfattning
På sidan Lokal sammanfattning visas en översikt över den profilerade VMware-miljön ![Lokal sammanfattning](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-summary-h2a.png)

**Startdatum** och **Slutdatum**: Start- och slutdatum för de profileringsdata som ingår i rapportgenereringen. Som standard är startdatumet det datum då profileringen startades och slutdatumet är det datum när profileringen avslutades. Om du angav parametrarna -StartDate och -EndDate när du genererade rapporten visas dessa värden.

**Total number of profiling days** (Totalt antal dagar för profilering): Det totala antalet profileringsdagar mellan start- och slutdatumen som rapporten genererats för.

**Number of compatible virtual machines** (Antal kompatibla virtuella datorer): Det totala antalet kompatibla virtuella datorer som nödvändig nätverksbandbredd, nödvändigt antal lagringskonton och antal Microsoft Azure-kärnor beräknas för.

**Total number of disks across all compatible virtual machines (Totalt antal diskar för samtliga kompatibla virtuella datorer)**: Det totala antalet diskar för samtliga kompatibla virtuella datorer.

**Average number of disks per compatible virtual machine** (Genomsnittligt antal diskar per kompatibel virtuell dator): Det genomsnittliga antalet diskar beräknat för samtliga kompatibla virtuella datorer.

**Average disk size (GB)** (Genomsnittlig diskstorlek (GB)): Den genomsnittliga diskstorleken beräknad för samtliga kompatibla virtuella datorer.

**Desired RPO (minutes)** (Önskat RPO-mål (minuter)): Antingen standardvärdet för RPO-mål eller det värde som angavs för parametern ”DesiredRPO” när rapporten genererades för att uppskatta nödvändig bandbredd.

**Desired bandwidth (Mbps)** (Önskad bandbredd (Mbit/s)): Det värde du angav för parametern ”Bandwidth” när du genererade rapporten för att uppskatta vilket RPO-mål som kan uppnås.

**Observed typical data churn per day (GB)** (Observerad normal dataomsättning per dag (GB)) är den genomsnittliga dataomsättning som observerats under alla profileringsdagar.

## <a name="recommendations"></a>Rekommendationer  
Rekommendationsbladet för Hyper-V till Azure-rapporten innehåller följande information enligt vald önskat RPO:

![Rekommendationer för Hyper-V till Azure-rapport](media/site-recovery-hyper-v-deployment-planner-analyze-report/Recommendations-h2a.png)

### <a name="profile-data"></a>Profildata
![Profildata](media/site-recovery-hyper-v-deployment-planner-analyze-report/profile-data-h2a.png)

**Profiled data period** (Profileringsdataperiod): Den period då profileringen kördes. Som standard innehåller verktyget alla profilerade data i beräkningen. Om du har använt alternativet StartDate och EndDate i rapportgenerering genereras rapporten för den specifika perioden. 

**Number of Hyper-V servers profiled** (Antal profilerade Hyper-V-servrar): Antalet Hyper-V-servrar vars VM-rapport genereras. Klicka på det tal som ska visa namnet på Hyper-V-servrarna. Detta öppnar arket Krav för lokal lagring där alla servrar finns angivna tillsammans med deras lagringskrav.    

**Desired RPO** (Önskat återställningspunktmål): Återställningspunktmålet för din distribution. Som standard beräknas vilken nätverksbandbredd som krävs för RPO-värden på 15, 30 respektive 60 minuter. De aktuella värdena på bladet uppdateras baserat på vad du väljer. Om du använde parametern DesiredRPOinMin när du genererade rapporten så visas det här värdet i resultatet Desired RPO (Önskat RPO-mål).

### <a name="profiling-overview"></a>Profileringsöversikt
![Profileringsöversikt](media/site-recovery-hyper-v-deployment-planner-analyze-report/profiling-overview-h2a.png)

**Total Profiled Virtual Machines** (Totalt antal profilerade virtuella datorer): Det totala antalet virtuella datorer som det finns profileringsdata för. Om VMListFile innehåller namn på virtuella datorer som inte har profilerats så beaktas inte dessa virtuella datorer i rapporten och de ingår inte i värdet för antalet virtuella datorer som har profilerats.

**Compatible Virtual Machines** (Kompatibla virtuella datorer): Det antal virtuella datorer som kan skyddas i Azure med Azure Site Recovery. Det här är det totala antalet kompatibla virtuella datorer som nödvändig nätverksbandbredd, antal lagringskonton och antal Azure-kärnor beräknas för. Information om varje kompatibel virtuell dator finns i avsnittet ”Kompatibla virtuella datorer”.

**Incompatible Virtual Machines** (Inkompatibla virtuella datorer): Antalet profilerade virtuella datorer som inte kan skyddas med Azure Site Recovery. Orsaken till inkompatibiliteten beskrivs i avsnittet Inkompatibla virtuella datorer. Om VMListFile innehåller namnen på virtuella datorer som inte har profilerats undantas dessa virtuella datorer från antalet inkompatibla virtuella datorer. Dessa virtuella datorer visas under Data not found (Inga data hittades) i slutet av avsnittet Incompatible VMs (Inkompatibla virtuella datorer).

**Desired RPO** (Önskat återställningspunktmål): Önskat mål för återställningspunkten (RPO) i minuter. Rapporten genereras för tre värden för återställningspunktmål: 15 (standard), 30 respektive 60 minuter. Rekommendationen angående bandbredd i rapporten förändras baserat på vilket alternativ du väljer i listrutan Desired RPO (Önskat RPO-mål) uppe till höger på bladet. Om du har genererat rapporten med ett anpassat värde för parametern -DesiredRPO visas det här anpassade värdet som standardvärde i listrutan Desired RPO (Önskat återställningspunktmål).

### <a name="required-network-bandwidth-mbps"></a>Required Network Bandwidth (Mbps) (Nödvändig nätverksbandbredd (Mbit/s))
![Nödvändig nätverksbandbredd](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-network-bandwidth-h2a.png)

**To meet RPO 100 percent of the time** (För att nå RPO-målet 100 procent av gångerna): Det här är den rekommenderade bandbredd i mbit/s som du bör allokera om RPO-målet ska nås 100 procent av tiden. Den här mängden bandbredd måste vara reserverad för stabil deltareplikering av samtliga kompatibla virtuella datorer om du helt ska undvika överträdelser av RPO-målet.

**To meet RPO 90 percent of the time** (För att nå RPO-målet 90 procent av gångerna): Om bandbreddspriserna är för höga eller om du av någon annan anledning inte kan tilldela den bandbredd som krävs för att uppnå RPO-målet 100 procent av tiden kan du välja en lägre bandbreddsinställning som gör att du uppnår önskat RPO 90 procent av tiden. I rapporten ges även en ”tänk om”-analys av hur många RPO-överträdelser du kan förvänta dig och deras varaktighet, så att du bättre ska förstå vad som kan hända om du tilldelar den här lägre bandbredden.

**Achieved Throughput** (Uppnått dataflöde): Dataflödet från servern där du körde kommandot GetThroughput till den Azure-region där lagringskontot finns. Dataflödesvärdet är en uppskattning av den nivå du kan uppnå när du skyddar de kompatibla virtuella datorerna med Azure Site Recovery, förutsatt att lagrings- och nätverksegenskaperna för Hyper-V-serverlagringen förblir desamma som för den server där du körde verktyget.

För alla företagsdistributioner av Azure Site Recovery bör du använda [ExpressRoute](https://aka.ms/expressroute).

### <a name="required-storage-accounts"></a>Nödvändiga lagringskonton
I följande diagram visas hur många lagringskonton (standard och premium) som behövs för att skydda alla kompatibla virtuella datorer. Om du vill veta vilket lagringskonto som ska användas för varje virtuell dator kan du läsa avsnittet ”Placering av VM-lagring”.

![Nödvändiga lagringskonton](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-storage-accounts-h2a.png)

### <a name="required-number-of-azure-cores"></a>Nödvändigt antal Azure-kärnor
Resultatet är det totala antalet kärnor som ska konfigureras före redundansväxling eller redundanstest av alla kompatibla virtuella datorer. Om det inte finns tillräckligt många kärnor tillgängliga i prenumerationen kan inte Azure Site Recovery skapa virtuella datorer vid redundanstestet eller redundansväxlingen.
![Nödvändigt antal Azure-kärnor](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-number-of-azure-cores-h2a.png)


### <a name="additional-on-premises-storage-requirement"></a>Ytterligare krav för lokal lagring

Det totala lediga lagringsutrymmet som krävs för Hyper-V-servrar för en lyckad initial replikering och deltareplikering för att garantera att VM-replikeringen inte orsakar några oönskade driftstopp för dina produktionsprogram. Information om varje volymkrav finns i [krav för lokal lagring](#on-premises-storage-requirement). 

För att få veta varför ledigt utrymme krävs för replikeringen kan du läsa avsnittet [Krav för lokal lagring](#why-do-i-need-free-space-on-the-hyper-v-server-for-the-replication).

![krav för lokal lagring och kopieringsfrekvens](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-storage-and-copy-frequency-h2a.png)

### <a name="maximum-copy-frequency"></a>Maximal kopieringsfrekvens
Den rekommenderade maximala kopieringsfrekvensen måste anges för replikeringen för att önskad RPO ska uppnås. Standardvärdet är 5 minuter. Du kan ställa in en kopieringsfrekvens på 30 sekunder för att få bättre RPO (mål för återställningspunkt).

### <a name="what-if-analysis"></a>Konsekvensanalys
![Konsekvensanalys](media/site-recovery-hyper-v-deployment-planner-analyze-report/what-if-analysis-h2a.png) I den här analysen beskrivs hur många överträdelser som kan inträffa under profileringsperioden när du tilldelar en lägre bandbredd för att önskat RPO-mål ska uppfyllas 90 procent av gångerna. En eller flera överträdelser av återställningspunktmålen kan inträffa på en viss dag. Grafen visar toppåterställningspunktmålet för dagen. Utifrån den här analysen kan du avgöra om du kan godta antalet RPO-överträdelser och dagens största RPO-överträdelse sett till den lägre bandbredden. Om värdena är godtagbara kan du allokera den lägre bandbredden för replikering. Annars allokerar du den högre bandbredd som rekommenderas för att du ska uppnå önskat RPO-mål 100 % av gångerna. 

### <a name="recommendation-for-successful-initial-replication"></a>Rekommendation för lyckad initial replikering
I det här avsnittet rekommenderar vi antalet batchar i vilka de virtuella datorerna ska skyddas och den minsta bandbredd som krävs för att slutföra den initiala replikeringen (IR). 

![IR-batchbearbetning](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-h2a.png)

De virtuella datorerna måste skyddas i angiven batchordning. Varje grupp har en specifik lista över virtuella datorer. Virtuella Batch 1-datorer måste skyddas före Batch 2. Virtuella Batch 2-datorer måste skyddas före Batch 3 och så vidare. När den inledande replikeringen av de virtuella datorerna i batch 1 är klar kan du aktivera replikering för virtuella datorer i batch 2. När den inledande replikeringen av de virtuella datorerna i batch 2 är klar kan du på samma sätt aktivera replikering för virtuella datorer i batch 3 och så vidare. Om batchordningen inte följs kanske det inte finns tillräckligt med bandbredd för inledande replikering för virtuella datorer som ska skyddas senare. Resultatet blir antingen att de virtuella datorerna aldrig kan slutföra den inledande replikeringen eller att några skyddade virtuella datorer kan gå in i läget för omsynkronisering. IR-batchbearbetning för det valda RPO-arket innehåller detaljerad information om vilka virtuella datorer som ska ingå i varje batch.

Diagrammet här visar bandbreddsfördelningen för inledande replikering och deltareplikering i batchar i den angivna batchordningen. När du skyddar virtuella datorer i den första batchen är fullständig bandbredd tillgänglig för inledande replikering. När den inledande replikeringen är sluförd för den första batchen krävs en del av bandbredden för deltareplikering. Återstående bandbredd kommer att vara tillgänglig för inledande replikering för virtuella datorer i den andra batchen. Fältet för batch 2 visar den bandbredd som krävs för deltareplikering för virtuella datorer i batch 1 och den tillgängliga bandbredden för inledande replikering av virtuella datorer i batch 2. På samma sätt visar fältet för Batch 3 den bandbredd som krävs för deltareplikering av föregående batchar (virtuella datorer i Batch 1 och Batch 2) och tillgänglig bandbredd för inledande replikering för Batch 3 och så vidare.  När inledande replikering av alla batchar är klar visar det senaste fältet bandbredden som krävs för deltareplikering för alla skyddade virtuella datorer. 

**Varför behöver jag inledande batchbearbetning av replikering?**
Tidsåtgången för den inledande replikeringen med avseende på den virtuella datorns diskstorlek, förbrukat diskutrymme och tillgängligt dataflöde i nätverket. Informationen finns i IR-batchbearbetning för ett valt RPO-ark.

### <a name="cost-estimation"></a>Kostnadsuppskattning
Grafen visar sammanfattningsvyn av den uppskattade kostnaden haveriberedskapen (DR) för Azure för din valda målregion och valutan som du har angett för rapportgenerering.
![Sammanfattning av kostnadsuppskattning](media/site-recovery-hyper-v-deployment-planner-analyze-report/cost-estimation-summary-h2a.png) Sammanfattningen hjälper dig att förstå den kostnad som du behöver betala för lagring, beräkning, nätverk och licens när du skyddar alla dina kompatibla virtuella datorer till Azure med Azure Site Recovery. Kostnaden beräknas för kompatibla virtuella datorer och inte för alla profilerade virtuella datorer.  
 
Du kan visa kostnaden per månad eller per år. Läs mer om [målregioner som stöds](./site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-target-regions) och [valutor som stöds](./site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-currencies).

**Cost by components** (Kostnad per komponenter) Den totala DR-kostnaden delas upp i fyra komponenter: beräkning, lagring, nätverk och Azure Site Recovery-licenskostnad. Kostnaden beräknas baserat på förbrukningen som tillkommer under replikering och DR-testtiden för beräkning, lagring (premium och standard), ExpressRoute/VPN som har konfigurerats mellan den lokala platsen och Azure och Azure Site Recovery-licens.

**Cost by states** (Kostnad per tillstånd) Den totala kostnaden för haveriberedskap (DR) är kategorier baserat på två olika tillstånd – replikering och DR-test. 

**Replication cost** (Replikeringskostnad): Kostnaden som tillkommer under replikering. Det här täcker kostnaden för lagring, nätverk och Azure Site Recovery-licensen. 

**DR-Drill cost** (DR-testkostnad): Kostnaden som tillkommer under redundanstext. Azure Site Recovery startar virtuella datorer under redundanstest. DR-testkostnaden täcker beräkning och lagring för de virtuella datorer som körs. 

**Azure storage cost per Month/Year** (Azure Storage-kostnad per månad/år) Det visar den totala lagringskostnad som tillkommer för premium- och standardlagring för replikering och DR-test.
Du kan visa en detaljerad kostnadsanalys per VM på arket [Cost Estimation](site-recovery-hyper-v-deployment-planner-cost-estimation.md) (Kostnadsuppskattning).

### <a name="growth-factor-and-percentile-values-used"></a>Tillväxtfaktor och percentilvärde som används
I det här avsnittet längst ned på bladet visas vilket percentilvärde som använts för prestandaräknarna för de profilerade virtuella datorerna (standardvärdet är den 95:e percentilen) och vilken tillväxtfaktor som använts i alla beräkningar (standardvärdet är 30 procent).

![Tillväxtfaktor och percentilvärde som används](media/site-recovery-hyper-v-deployment-planner-run/growth-factor-max-percentile-value.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Rekommendationer med tillgänglig bandbredd som indata
![Profileringsöversikt med bandbreddsindata](media/site-recovery-hyper-v-deployment-planner-analyze-report/profiling-overview-bandwidth-input-h2a.png)

Du kan ha en situation där du vet att du inte kan ange en bandbredd på mer än x Mbit/s för Azure Site Recovery-replikering. Du kan välja att ange tillgänglig bandbredd i verktyget (med parametern -Bandwidth när du genererar rapporten) och då få se vilket RPO-mål i minuter du kan uppnå. Utifrån det här möjliga RPO-värdet kan du avgöra om du måste distribuera ytterligare bandbredd eller om du nöjer dig med en lösning för haveriberedskap med det aktuella RPO-värdet.

![Möjligt återställningspunktmål (RPO)](media/site-recovery-hyper-v-deployment-planner-analyze-report/achivable-rpo-h2a.PNG)

## <a name="vm-storage-placement-recommendation"></a>Rekommendation för placering av VM-lagring 

![Placering av VM-lagring](media/site-recovery-hyper-v-deployment-planner-analyze-report/vm-storage-placement-h2a.png)

**Disk Storage Type** (Typ av disklagring): Är antingen Standard eller Premium och avser det lagringskonto som ska användas för replikering av motsvarande virtuella datorer i kolumnen **VMs to Place** (Virtuella datorer att placera ut).

**Suggested Prefix** (Föreslaget prefix): Det föreslagna prefixet på tre tecken som du kan använda för att namnge lagringskontot. Du kan använda ditt eget prefix, men verktygets förslag följer [namngivningskonventionen för partitioner av lagringskonton](https://aka.ms/storage-performance-checklist).

**Suggested Account Name** (Föreslaget kontonamn): Namnet på lagringskontot när du inkluderar det föreslagna prefixet. Ersätt namnet inom hakparenteser (< och >) med egna indata.

**Log Storage Account** (Lagringskonto för loggar): alla replikeringsloggar lagras på ett lagringskonto av standardtyp. För virtuella datorer som replikerar till ett Premium Storage-konto konfigurerar du ytterligare ett Standard Storage-konto för logglagringsutrymme. Flera lagringskonton för premiumreplikering kan använda samma standardkonto för logglagring. Virtuella datorer som replikeras till lagringskonton av standardtyp använder samma lagringskonto för loggarna.

**Suggested Log Account Name** (Föreslaget loggkontonamn): Namnet på lagringsloggkontot när du inkluderar det föreslagna prefixet. Ersätt namnet inom hakparenteser (< och >) med egna indata.

**Placement Summary** (Placeringsöversikt): En översikt över den totala virtuella datorbelastningen på lagringskontot vid replikeringen samt vid redundanstest/redundansväxling. I översikten ingår det totala antalet virtuella datorer som har mappats till lagringskontot, totalt antal läs- och skrivåtgärder (IOPS) för de virtuella datorer som placerats på lagringskontot, totalt antal skrivoperationer (replikering), total etablerad storlek sett till alla diskar och det totala antalet diskar.

**Virtual Machines to Place** (Virtuella datorer att placera ut): En lista över de virtuella datorer som ska placeras på det angivna lagringskontot för att prestanda och användningsgrad ska vara optimala.

## <a name="compatible-vms"></a>Compatible VMs (Kompatibla virtuella datorer)
Microsoft Excel-rapporten som genereras av distributionshanteraren för Azure Site Recovery innehåller information om alla kompatibla virtuella datorer i arket ”Compatible VMs” (Kompatibla virtuella datorer).

![Compatible VMs (Kompatibla virtuella datorer)](media/site-recovery-hyper-v-deployment-planner-analyze-report/compatible-vms-h2a.png)

**VM Name** (Namn på virtuell dator): Den virtuella datorns namn som används i VMListFile när en rapport skapas. I den här kolumnen visas även de diskar (VHD:er) som är kopplade till de virtuella datorerna. Namnen inkluderar de Hyper-V-värdnamn där de virtuella datorerna placerades när verktyget upptäckte de under profileringsperioden.

**VM-kompatibilitet**: Värdena är **Ja** och **Ja**\*. **Ja**\* för instanser där den virtuella datorn är en anpassning för [Azure Premium Storage](https://aka.ms/premium-storage-workload). Här ryms den profilerade höga omsättningen eller IOPS-disken i en högre premiumdiskstorlek än storleken som är mappad till disken. Lagringskontot avgör vilken Premium Storage-disktyp som en disk ska mappas till, baserat på dess storlek. 
* < 128 GB är en P10.
* 128 GB till 256 GB är en P15
* 256 till 512 GB är en P20.
* 512 till 1 024 GB är en P30.
* 1 025 till 2 048 GB är en P40.
* 2 049 till 4 095 GB är en P50.

Om exempelvis arbetsbelastningsegenskaperna för en disk placerar den i kategorin P20 eller P30, men storleken mappar den till en lägre Premium Storage-disktyp, markerar verktyget den här virtuella datorn som **Ja**\*. Verktyget rekommenderar också att du antingen ändrar källdiskens storlek så att den passar den rekommenderade Premium Storage-disktypen eller ändrar måldisktypen efter redundansväxling.

**Lagringstyp**: Standard eller premium.

**Suggested Prefix** (Föreslaget prefix): Ett prefix på tre tecken för lagringskontot.

**Lagringskontot**: Namnet med prefixet till det föreslagna lagringskontot.

**Peak R/W IOPS (with Growth Factor)** (Högsta R/W IOPS (med tillväxtfaktor)): Den högsta IOPS-arbetsbelastningen för läsning/skrivning på disken (standardvärdet är den 95:e percentilen), inklusive faktorn för framtida tillväxt (standardvärdet är 30 procent). Observera att det totala antalet läs/skriv-IOPS för en virtuell dator inte alltid är summan av de enskilda diskarnas läs/skriv-IOPS, eftersom den virtuella datorns högsta läs/skriv-IOPS är den högsta summan av de enskilda diskarnas läs/skriv-IOPS under varje minut av profileringsperioden.

**Högsta dataomsättning i MB/s (med tillväxtfaktor)**: Den högsta dataomsättningsfrekvensen på disken (standardvärdet är den 95:e percentilen) inklusive faktorn för framtida tillväxt (standardvärdet är 30 procent). Observera att den totala dataomsättningen för den virtuella datorn inte alltid är summan av de enskilda diskarnas dataomsättning, eftersom den virtuella datorns högsta dataomsättning är den högsta summan av de enskilda diskarnas dataomsättning under varje minut av profileringsperioden.

**Azure VM Size** (Storlek för virtuell Azure-dator): Lämplig mappad storlek på den virtuella Azure Cloud Services-datorn för den här lokala virtuella datorn. Mappningen baseras på det lokala virtuella datorminnet, antalet diskar/kärnor/nätverkskort och läs- och skrivåtgärder, IOPS. Rekommendationen är alltid den lägsta virtuella Azure-datorstorlek som matchar alla lokala virtuella datoregenskaper.

**Number of Disks** (Antal diskar): Det totala antalet virtuella datordiskar (VHD:er) på den virtuella datorn.

**Disk size (GB)** (Diskstorlek (GB)): Total storlek för alla diskar på den virtuella datorn. Storleken för de enskilda diskarna i den virtuella datorn visas också i verktyget.

**Kärnor**: Antalet processorkärnor i den virtuella datorn.

**Minne (MB)**: Den virtuella datorns RAM-minne.

**Nätverkskort**: Antalet nätverkskort på den virtuella datorn.

**Starttyp**: Den virtuella datorns starttyp. Den kan vara BIOS eller EFI.

## <a name="incompatible-vms"></a>Incompatible VMs (Inkompatibla virtuella datorer)
Microsoft Excel-rapporten som genereras av distributionshanteraren för Azure Site Recovery innehåller information om alla inkompatibla virtuella datorer i arket ”Incompatible VMs” (Inkompatibla virtuella datorer).

![Incompatible VMs (Inkompatibla virtuella datorer)](media/site-recovery-hyper-v-deployment-planner-analyze-report/incompatible-vms-h2a.png)

**VM Name** (Namn på virtuell dator): Den virtuella datorns namn som används i VMListFile när en rapport skapas. I den här kolumnen visas även de diskar (VHD:er) som är kopplade till de virtuella datorerna. Namnen inkluderar de Hyper-V-värdnamn där de virtuella datorerna placerades när verktyget upptäckte de under profileringsperioden.

**VM Compatibility** (VM-kompatibilitet): Anger varför den här virtuella datorn inte kan skyddas med Azure Site Recovery. Anledningarna beskrivs för varje inkompatibel disk av den virtuella datorn och kan, baserat på publicerade [lagringsgränser](https://aka.ms/azure-storage-scalbility-performance), vara något av följande:

* Diskstorleken är > 4095 GB. Azure Storage har för närvarande inte stöd för diskar som är större än 4 095 GB.

* OS-disken är > 2047 GB för VM i Generation 1 (BIOS-starttyp).  Azure Site Recovery stöder inte OS-diskstorlekar på över 2047 GB för virtuella datorer i generation 1.

* OS-disken är > 300 GB för VM i Generation 2 (EFI-starttyp). Azure Site Recovery stöder inte OS-diskstorlekar på över 300 GB för virtuella datorer i generation 2.

* VM-namn som innehåller något av följande tecken “” [] ` stöds inte.  Verktyget kan inte hämta profilerade data för virtuella datorer som har några sådana tecken i sina namn. 

* VHD delas av minst två virtuella datorer. Azure stöder inte virtuella datorer med delad VHD.

* Virtuella datorer med Virtual Fiber Channel stöds inte. Azure Site Recovery stöder inte virtuella datorer med Virtual Fiber Channel.

* Hyper-V-kluster innehåller inte koordinatortjänst för replikering. Azure Site Recovery stöder inte en virtuell dator i ett Hyper-V-kluster om koordinatortjänsten för Hyper-V inte är konfigurerad för klustret.

* Den virtuella datorn har inte hög tillgänglighet. Azure Site Recovery stöder inte en virtuell dator om Hyper-V-klusternodens VHD är lagrade på den lokala disken istället för på klusterdisken. 

* Total storlek för den virtuella datorn (replikering + TFO) överskrider den gräns för premiumlagringskontostorlek som stöds (35 TB). Den här inkompatibiliteten uppstår vanligen när en enskild disk i den virtuella datorn har en prestandaegenskap som överskrider den maxgräns som stöds av Azure- eller Azure Site Recovery-gränserna för standardlagring. Denna instans skickar den virtuella datorn till Premium Storage-zonen. Maxgränsen för ett lagringskonto av premiumtyp är däremot 35 TB, och det går inte att skydda en enda virtuell dator över flera lagringskonton. Tänk också på att när ett redundanstest körs på en skyddad virtuell dator, om den ohanterade disken är konfigurerad för att köra redundanstest, körs det på samma lagringskonto där replikeringen körs. I den här instansen krävs ytterligare samma mängd lagringsutrymme som för replikeringen. Den garanterar att replikeringen ska gå vidare och att redundanstest ska lyckas parallellt. När hanterade diskar konfigureras för att testredundans behöver inget ytterligare utrymme redovisas för VM-testredundans.

* Käll-IOPS överskrider IOPS-gränsen för lagring på 7500 per disk.

* Käll-IOPS överskrider IOPS-gränsen för lagring på 80 000 per virtuell dator.

* Den genomsnittliga dataomsättningen för virtuella källdatorer överskrider den dataomsättningsgräns som stöds av Azure Site Recovery på 10 MB/s för den genomsnittliga I/O-storleken.

* Genomsnittligt antal effektiva skrivåtgärder (IOPS) för den virtuella källdatorn överskrider gränsen i Azure Site Recovery på 840.

* Beräknat lagringsutrymme för ögonblicksbilder överskrider gränsen på 10 TB.

**Peak R/W IOPS (with Growth Factor)** (Högsta R/W IOPS (med tillväxtfaktor)): Den högsta IOPS-arbetsbelastningen för läsning/skrivning på disken (standardvärdet är den 95:e percentilen), inklusive faktorn för framtida tillväxt (standardvärdet är 30 procent). Observera att det totala antalet läs/skriv-IOPS för den virtuella datorn inte alltid är summan av de enskilda diskarnas läs/skriv-IOPS, eftersom den virtuella datorns högsta läs/skriv-IOPS är den högsta summan av de enskilda diskarnas läs/skriv-IOPS under varje minut av profileringsperioden.

**Högsta dataomsättning i MB/s (med tillväxtfaktor)**: Den högsta dataomsättningsfrekvensen på disken (standardvärdet är den 95:e percentilen) inklusive faktorn för framtida tillväxt (standardvärdet är 30 procent). Observera att den totala dataomsättningen för den virtuella datorn inte alltid är summan av de enskilda diskarnas dataomsättning, eftersom den virtuella datorns högsta dataomsättning är den högsta summan av de enskilda diskarnas dataomsättning under varje minut av profileringsperioden.

**Number of Disks** (Antal diskar): Det totala antalet VHD:er på den virtuella datorn.

**Disk size (GB)** (Diskstorlek (GB)): Total installationsstorlek för alla diskar på den virtuella datorn. Storleken för de enskilda diskarna i den virtuella datorn visas också i verktyget.

**Kärnor**: Antalet processorkärnor i den virtuella datorn.

**Minne (MB)**: Mängden RAM-minne på den virtuella datorn.

**Nätverkskort**: Antalet nätverkskort på den virtuella datorn.

**Starttyp**: Den virtuella datorns starttyp. Den kan vara BIOS eller EFI.

## <a name="azure-site-recovery-limits"></a>Gränser för Azure Site Recovery
Följande tabell innehåller gränserna för Azure Site Recovery. Dessa gränser är baserade på våra tester, men de täcker inte alla möjliga kombinationer av program-I/O. De faktiska resultaten kan variera beroende på blandningen av I/O i ditt program. För bästa resultat även efter distributionsplaneringen rekommenderar vi alltid att du kör omfattande programtester med redundanstest för att få en bild av verklig prestanda för programmet.
 
**Replication Storage Target** (Lagringsmål för replikering) | **Genomsnittlig I/O-storlek för virtuell källdator** |**Genomsnittlig omsättning för virtuell källdator** | **Total dataomsättning per dag för virtuell källdisk**
---|---|---|---
Standard Storage | 8 kB | 2 MB/s per virtuell dator | 168 GB per virtuell dator
Premium Storage | 8 kB  | 5 MB/s per virtuell dator | 421 GB per virtuell dator
Premium Storage | 16 kB eller mer| 10 MB/s per virtuell dator | 842 GB per virtuell dator

Gränserna är genomsnittliga värden baserade på en I/O-överlappning på 30 procent. Azure Site Recovery kan hantera högre dataflöden med annan överlappning, större skrivningsstorlek och verkligt I/O-beteende under arbetsbelastningen. Föregående antal antar en typisk eftersläpning på cirka fem minuter. Det vill säga, när data har överförts bearbetas de och en återställningspunkt skapas inom fem minuter.

## <a name="on-premises-storage-requirement"></a>Krav för lokal lagring

Kalkylbladet tillhandahåller kravet på totalt ledigt diskutrymme för varje volym på Hyper-V-servrarna (där VHD:er finns) för lyckad inledande replikering och deltareplikering. Innan du aktiverar replikering lägger du till det lagringsutrymme som krävs för volymer för att se till att replikeringen inte orsakar några oönskade driftstopp för dina produktionsprogram. 

Distributionshanteraren för Azure Site Recovery identifierar kravet på optimal lagringsutrymme baserat på VHD-storleken och nätverksbandbredden som används för replikeringen.

![krav för lokal lagring](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-storage-requirement-h2a.png)

### <a name="why-do-i-need-free-space-on-the-hyper-v-server-for-the-replication"></a>Varför behöver jag ledigt utrymme på Hyper-V-servern för replikeringen?
* När du aktiverar replikering av en virtuell dator tar Azure Site Recovery en ögonblicksbild av varje VHD för den virtuella datorn för inledande replikering (IR). När den inledande replikeringen pågår skrivs nya ändringar till diskar i programmet. Azure Site Recovery spårar ändringarna delta i loggfiler, som kräver ytterligare lagringsutrymme.  Innan den inledande replikeringen är klar lagras loggfilerna lokalt. Om det inte finns tillräckligt med utrymme för loggfilerna och ögonblicksbilden (AVHDX) hamnar replikeringen i läget för omsynkronisering och replikeringen slutförs aldrig. I värsta fall behöver du 100 % ytterligare ledigt utrymme av samma storlek som VHD:n för inledande replikering.
* När den inledande replikeringen är klar startar deltareplikeringen. Azure Site Recovery spårar dessa deltaändringar i loggfilerna som lagras på volymen där den virtuella datorns virtuella hårddiskar (VHD) finns. Dessa loggfiler replikeras till Azure vid en konfigurerad koperingsfrekvens. Det kan ta lite tid att replikera loggfilerna till Azure, baserat på den tillgängliga nätverksbandbredden. Om det saknas tillräckligt ledigt utrymme för att lagra filerna pausas replikeringen och den virtuella datorns replikeringsstatus försätts i den omsynkronisering som krävs.
* Om nätverksbandbredden inte är tillräckligt för att skicka loggfilerna till Azure staplas loggfilerna på volymen. När en loggfils storlek ökar till 50 % av VHD-storleken försätts VM-replikeringen i värsta fall i den omsynkronisering som krävs. I värsta fall behöver du 50 % ytterligare ledigt utrymme av samma storlek som VHD:n för deltareplikering.

**Hyper-V-värd**: Listan över profilerade Hyper-V-servrar. Om en server är en del av ett Hyper-V-kluster grupperas alla klusternoder ihop.

**Volym (VHD-sökväg)**: Varje volym av en Hyper-V-värd där VHD:er/VHDX:er finns. 

**Tillräckligt ledigt utrymme (GB)**: Tillgängligt ledigt utrymme på volymen.

**Totalt lagringsutrymme som krävs på volymen (GB**: Det totala lediga utrymmet som krävs för volymen för lyckad inledande replikering och deltareplikering. 

**Total mängd ytterligare lagring som ska etableras på volymen för lyckad replikering**: Du blir rekommenderad det totala ytterligare utrymmet som måste etableras på volymen för lyckad inledande replikering och deltareplikering.

## <a name="initial-replication-batching"></a>Inledande batchbearbetning av replikering 

### <a name="why-do-i-need-initial-replication-ir-batching"></a>Varför behöver jag inledande batchbearbetning (IR) av replikering?
Om alla virtuella datorer skyddas samtidigt blir kravet på ledigt utrymme mycket högre, och om det inte finns tillräckligt med utrymme hamnar replikeringen i omsynkroniseringsläge. Kravet på nätverksbandbredd skulle också bli mycket högre för att slutföra inledande replikering av alla virtuella datorer samtidigt. 

### <a name="initial-replication-batching-for-a-selected-rpo"></a>Inledande batchbearbetning av replikering för en vald RPO
Det här kalkylbladet innehåller en detaljerad vy för varje batch för inledande replikering (IR). För varje RPO skapas ett separat IR-batchbearbetningsark. 

När du har följt rekommendationerna för lokal lagring för varje volym finns den viktigaste informationen du behöver för att replikera i listan över virtuella datorer som kan skyddas parallellt. Dessa virtuella datorer är grupperade tillsammans i en batch. Det kan finnas flera batchar. Du måste skydda virtuella datorer i den angivna batchordningen. Först ska du skydda virtuella datorer i Batch 1, och när den inledande replikeringen är klar ska du skydda virtuella datorer i Batch 2 och så vidare. Du kan hämta listan över batchar och motsvarande virtuella datorer från det här bladet. 

![Uppgifter för IR-batchbearbetning1](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-for-rpo-h2a.png)
![Uppgifter för IR-batchbearbetning2](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-for-rpo2-h2a.png)

### <a name="each-batch-provides-the-following-information"></a>Varje batch innehåller följande information 
**Hyper-V-värd**: Hyper-V-värden för den virtuella datorn som ska skyddas.
Virtuell dator: Den virtuella dator som ska skyddas. 

**Kommentarer**: Om det krävs någon åtgärd för en specifik volym på en virtuell dator anges kommentaren här.  Om det exempelvis inte finns tillräckligt med ledigt utrymme på en volym visas kommentaren Add additional storage to protect this VM (Lägg till ytterligare lagringsutrymme för att skydda den här virtuella datorn).

**Volym (VHD-sökväg)**: Volymens namn där den virtuella datorns VHD:er finns. Ledigt utrymme på volymen (GB): ledigt diskutrymme på volymen för den virtuella datorn. Vid beräkningen av ledigt utrymme på volymerna övervägs det använda diskutrymmet för deltareplikering av de virtuella datorerna för föregående batchar vars VHD:er finns på samma volym.  

Exempelvis kan VM1, VM2 och VM3 finnas på volymen E:\VHDpath. Ledigt utrymme på volymen är 500 GB före replikering. VM1 är en del av Batch 1, VM2 är en del av Batch 2 och VM3 är en del av Batch3.  För VM1 är det lediga tillgängliga utrymmet 500 GB. För VM2 blir det lediga tillgängliga utrymmet 500 – diskutrymme krävs för deltareplikering för VM1.  Om vi säger att VM1 kräver 300 GB utrymme för deltareplikering ska det lediga tillgängliga utrymmet för VM2 vara 500 GB – 300 GB = 200 GB.  På samma sätt kräver VM2 300 GB för deltareplikering. Det lediga tillgängliga utrymmet för VM3 skulle bli 200 GB - 300 GB = -100 GB.

**Lagring som krävs på volymen för inledande replikering (GB)**: Det lediga utrymmet som krävs för VM-volymen för lyckad inledande replikering.

**Lagring som krävs på volymen för deltareplikering (GB)**: Det lediga utrymmet som krävs för VM-volymen för lyckad deltareplikering.

**Ytterligare lagring som krävs baserat på brist för att undvika replikeringsfel (GB)**: Ytterligare lagringsutrymme som krävs på volymen för den virtuella datorn.  Det är maximum för den inledande replikeringens och deltareplikeringens utrymmeskrav – frigör utrymme på volymen.

**Minsta bandbredd som krävs för inledande replikering (Mbit/s)**: Den minsta bandbredd som krävs för inledande replikering för den virtuella datorn.

**Minsta bandbredd som krävs för deltareplikering (Mbit/s)**: Den minsta bandbredd som krävs för deltareplikering för den virtuella datorn

### <a name="network-utilization-details-for-each-batch"></a>Information om nätverksanvändning för varje batch 
Varje batchtabell innehåller en sammanfattning av nätverksanvändningen för varje batch.

**Tillgänglig bandbredd för batch**: Den tillgängliga bandbredden för batchen efter att hänsyn tagits till den föregående batchens deltareplikeringsbandbredd.

**Uppskattad tillgänglig bandbredd för inledande replikering av batch**: Den tillgängliga bandbredden för inledande replikering av batchens virtuella datorer. 

**Ungefärlig förbrukad bandbredd för deltareplikering av batch**: Den bandbredd som krävs för deltareplikering av batchens virtuella datorer. 

**Uppskattad inledande replikeringstid för batch (HH:MM)**: Uppskattad inledande replikeringstid i timmar:minuter.

## <a name="cost-estimation"></a>Kostnadsuppskattning
Läs mer om [kostnadsuppskattning](site-recovery-hyper-v-deployment-planner-cost-estimation.md). 

## <a name="next-steps"></a>Nästa steg
Läs mer om [kostnadsuppskattning](site-recovery-hyper-v-deployment-planner-cost-estimation.md).