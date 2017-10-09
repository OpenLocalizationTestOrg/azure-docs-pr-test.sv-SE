---
title: "aaaBatch och HPC-lösningar i hello cloud - Azure | Microsoft Docs"
description: "Läs mer om scenarier och lösningsalternativ för databehandling med höga prestanda (HCP och Big Compute) i Azure"
services: batch, virtual-machines, cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
ms.assetid: aab5401d-2baf-4cf2-bf20-ad224de33888
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c5a3c8859d1f95040bcdad15942a815d71eb4486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="batch-and-hpc-solutions-for-large-scale-computing-workloads"></a>Batch- och HPC-lösningar för storskaliga beräkningsarbetsbelastningar

Azure erbjuder effektiva, skalbara molnlösningar för Batch och HCP (High Performance Computing – databehandling med höga prestanda), även kallat *Big Compute*. Lär dig här stort Compute arbetsbelastningar och Azures tjänster toosupport dem eller hoppa direkt för[lösningsscenarier](#scenarios) senare i den här artikeln. Den här artikeln är främst för tekniska beslutsfattare, IT-chefer och oberoende programvaruleverantörer, men andra IT-proffs och utvecklare kan använda it toofamiliarize sig själva med dessa lösningar.

Många organisationer har problem med storskalig databearbetning, till exempel inom områden som teknisk design och analys, bildrendering, komplex modellering, Monte Carlo-simuleringar, beräkning av finansiella risker med mera. Azure hjälper organisationer att lösa dessa problem med hello resurser, skala och schema som de behöver. Med Azure kan organisationer:

* Skapa en hybridlösningar kan utöka ett lokalt HPC-kluster toooffload belastning arbetsbelastningar toohello moln
* Köra verktyg för HPC-kluster och arbetsbelastningar helt i Azure.
* Använda hanterade och skalbar Azure-tjänster som [Batch](https://azure.microsoft.com/documentation/services/batch/) toorun beräkningsintensiva arbetsbelastningar utan toodeploy och hantera beräknings-infrastruktur

Även om utanför hello i den här artikeln, ger Azure också utvecklare och partners en fullständig uppsättning funktioner, arkitektur val och utveckling verktyg toobuild stora, anpassade stort Compute arbetsflöden. Ett växande partner ekosystem är klar toohelp som du gör dina Compute-stora arbetsbelastningar produktiv i hello Azure-molnet.

## <a name="batch-and-hpc-applications"></a>Batch- och HPC-program
Till skillnad från webbprogram och många affärsapplikationer har Batch- och HCP-program en definierad starttid och sluttid, och de kan köras enligt ett schema eller på begäran, ibland i flera timmar eller längre. De flesta är indelade i två huvudkategorier: *är parallella* (kallas ibland ”embarrassingly parallell”, eftersom de lösa problem med hello lämpar sig toorunning parallellt på flera datorer eller processorer) och *tätt kopplade*. Se följande tabell för mer information om dessa typer av programmet hello. Vissa Azure lösning närmar sig bättre för en typ eller hello andra arbete.

> [!NOTE]
> I Batch- och HPC-lösningar kallas ofta en instans av ett program som körs för ett *jobb*, och varje jobb kan delas in i *aktiviteter*. Och hello klustrade beräkningsresurser för programmet hello kallas ofta *datornoder*.
> 
> 

| Typ | Egenskaper | Exempel |
| --- | --- | --- |
| **Parallella**<br/><br/>![Parallella][parallel] |• Enskilda datorer kör programlogiken oberoende av varandra.<br/><br/> • Att lägga till datorer ger hello programmet tooscale och minska beräkning tid<br/><br/>• Programmet består av separata körbara filer eller är uppdelat i en grupp med tjänster som anropas av en klient (ett SOA-program). |• Finansiell riskmodellering.<br/><br/>• Bildåtergivning och bildrendering.<br/><br/>• Mediekodning och transkodning.<br/><br/>• Monte Carlo-simuleringar.<br/><br/>• Programvarutestning. |
| **Nära kopplade**<br/><br/>![Nära kopplade][coupled] |• Program kräver compute-noder toointeract eller exchange mellanresultat<br/><br/>• Compute-noder kan kommunicera med hjälp av hello Message Passing Interface (MPI), ett gemensamt kommunikationsprotokoll för för parallell datorbearbetning<br/><br/>• hello program är känslig toonetwork svarstid och bandbredd<br/><br/>• Programmets prestanda kan förbättras genom användning av tekniker för höghastighetsnätverk, t.ex. InfiniBand och direktåtkomst till fjärrminne (RDMA) |• Modellering av olje- och gasreservoarer.<br/><br/>• Teknisk design och analys, till exempel beräkningsströmningsdynamik.<br/><br/>• Fysiska simuleringar, till exempel bilkraschar och kärnreaktioner.<br/><br/>• Väderprognoser. |

### <a name="considerations-for-running-batch-and-hpc-applications-in-hello-cloud"></a>Överväganden för att köra batch och HPC-program i hello moln
Du kan enkelt migrera många program som är utformad toorun i lokala HPC-kluster tooAzure eller tooa hybridmiljö (mellan platser). Det kan dock finnas vissa begränsningar eller saker som du måste ta hänsyn till, t.ex.:

* **Tillgängligheten för molnresurser** -beroende på hello typ av molnet beräkningsresurser som du använder, kanske du inte kan toorely på kontinuerlig tillgänglighet medan ett jobb körs. Hantering av status och förlopp Kontrollera pekar vanliga tekniker toohandle möjliga tillfälligt fel och mer behov när du använder molnresurser.
* **Dataåtkomst** -Data access tekniker ofta i enterprise-kluster, till exempel NFS, kräva att du konfigurerar i hello molnet. Eller så behöver tooadopt olika data access-metoder och mönster för hello molnet.
* **Dataflyttning** - för program att processen stora mängder data, strategier är nödvändiga toomove hello data till lagring och toocompute molnresurser. Du kan behöva höghastighetsnätverk för lokala system, till exempel [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/). Du måste också ta hänsyn till eventuella juridiska krav, föreskrifter och policybegränsningar för lagringen av och åtkomsten till dessa data.
* **Licensiering** – hello från tillverkaren av kommersiella program för licensiering av eller andra begränsningar för att köra i hello molnet. Alla leverantörer erbjuder inte licensiering enligt modellen Betala per användning. Du kanske behöver tooplan för licensieringsservern i hello molnet för din lösning eller ansluta tooan lokalt licensservern.

### <a name="big-compute-or-big-data"></a>Big Compute eller stordata?
hello uppdelning mellan stort beräknings- och Stordata program inte alltid rensa och vissa program kan ha båda. Båda involverar beräkningar i hög skala, oftast i datorkluster. Men hello lösning strategier och stödjande verktyg kan variera.

• **Stor Compute** tenderar tooinvolve program som förlitar sig på CPU och minne, till exempel engineering simulering finansiella risk modellering, och digitala återgivning. hello infrastruktur för en stor Compute-lösning kan innehålla med särskilda flera kärnor processorer tooperform raw beräkning och speciella, snabba nätverk maskinvara tooconnect hello-datorer.

• **Stordata** löser dataanalysproblem som omfattar stora mängder data som inte kan hanteras av en enskild dator eller ett enskilt databashanteringssystem. Bland exemplen finns stora mängder webbloggar eller andra BI-data (Business Intelligence). Stordata tenderar toorely mer diskkapacitet och i/o-prestanda än på CPU-ström. Det finns särskilda verktyg för Stordata, till exempel Apache Hadoop toomanage hello klustret och partition hello-data. (Information om Azure HDInsight och andra Azure Hadoop-lösningar finns i [Hadoop](https://azure.microsoft.com/solutions/hadoop/).)

## <a name="compute-management-and-job-scheduling"></a>Beräkningshantering och schemaläggning av jobb
Batch- och HPC-program som körs ofta innehåller en *Klusterhanterare* och en *jobbschemat* toohelp hantera klustrade beräkningsresurser och tilldela dem toohello program som körs hello jobb. Dessa funktioner kan utföras av olika verktyg, eller ett integrerat verktyg eller en integrerad tjänst.

* **Klusterhanterare** – Etablerar, frisläpper och administrerar beräkningsresurser (eller beräkningsnoder). En Klusterhanterare kan automatisera installation av avbildningar av operativsystem och program på datornoderna, skalar beräkningsresurser enligt toodemands och övervaka hello prestanda för hello noder.
* **Jobbschemat** -anger hello resurser (t.ex processorer eller minne) ett program behöver och hello villkor när den körs. En jobbschemat upprätthåller en kö med jobb och allokerar resurser toothem baserat på en tilldelad prioritet eller andra egenskaper.

Kluster och jobb schemaläggs verktyg för Windows- och Linux-baserade kluster kan migrera korrekt tooAzure. Exempelvis har [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029), Microsofts kostnadsfria beräkningsklusterlösning för Windows- och Linux HPC-arbetsbelastningar, flera alternativ för körning i Azure. Du kan också skapa Linux-kluster toorun öppen källkod verktyg som moment och SLURM. Du kan också hämta kommersiella rutnätet lösningar tooAzure som [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/), [IBM spektrumet Symphony och Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/), och [Univa rutnätet motorn](http://www.univa.com/products/grid-engine).

Som visas i följande avsnitt hello du dra nytta av Azure-tjänster toomanage beräkningsresurser och schemalägga jobb utan (eller förutom) traditionella hanteringsverktyg.

## <a name="scenarios"></a>Scenarier
Här följer tre vanliga scenarier toorun stor Compute arbetsbelastningar i Azure med hjälp av befintliga lösningar för HPC-kluster, Azure-tjänster eller en kombination av hello två. Viktiga saker att tänka på när du väljer respektive scenario framhävs, men listan är inte fullständig. Mer är om hello Azure tjänster du kan använda i din lösning senare i hello artikeln.

| Scenario | Varför ska jag välja det? |
| --- | --- | --- |
| **Burst tooAzure ett HPC-kluster**<br/><br/>[![Cluster burst][burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> Läs mer:<br/>• [Burst tooAzure worker-instanser med HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)<br/><br/>• [Konfigurera ett hybridberäkningskluster med HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)<br/><br/>• [Burst tooAzure Batch med HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)<br/><br/> |• Kombinera [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) eller andra lokala kluster med ytterligare Azure-resurser i en hybridlösning.<br/><br/>• Utvidga din stort Compute arbetsbelastningar toorun på plattformen som en tjänst (PaaS) virtuella instanser (för närvarande endast Windows Server).<br/><br/>• Kom åt en lokal licensserver eller databas genom att använda ett valfritt virtuellt Azure-nätverk. |
| **Skapa ett HPC-kluster helt och hållet i Azure**<br/><br/>[![Cluster in IaaS][iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>Läs mer:<br/>• [HPC-klusterlösningar i Azure](big-compute-resources.md)<br/><br/> |• Distribuera snabbt och konsekvent program och klusterverktyg på standardiserade eller anpassade Windows- eller Linux-baserade virtuella IaaS-datorer (Infrastructure as a Service).<br/><br/>• Kör olika stort Compute arbetsbelastningar hello jobb schemaläggs lösning av ditt val.<br/><br/>• Använda ytterligare Azure-tjänster som inklusive nätverk och lagring toocreate fullständiga molnbaserade lösningar. |
| **Skala ut en parallell programmet tooAzure**<br/><br/>[![Azure Batch][batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>Läs mer:<br/>• [Grunderna i Azure Batch](batch-technical-overview.md)<br/><br/>• [Komma igång med hello Azure Batch-biblioteket för .NET](batch-dotnet-get-started.md) |• Utveckla med [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) tooscale ut olika stort Compute arbetsbelastningar toorun på pooler för Windows eller Linux virtuella datorer.<br/><br/>• Använd en Azure-plattformen tjänstdistributionen toomanage och autoskalning för virtuella datorer, schemaläggning av jobb, katastrofåterställning, dataflyttning, beroende management och programdistribution. |

## <a name="azure-services-for-big-compute"></a>Azure-tjänster för Big Compute
Här är mer om hello beräkning, data, nätverk och tillhörande tjänster kan du kombinera för stora Compute lösningar och arbetsflöden. Mer detaljerad information om Azure-tjänster finns hello Azure-tjänster [dokumentationen](https://azure.microsoft.com/documentation/). Hej [scenarier](#scenarios) tidigare i den här artikeln visar några olika sätt att använda dessa tjänster.

> [!NOTE]
> Azure introducerar regelbundet nya tjänster som kan vara användbara för ditt scenario. Om du har frågor kan du kontakta en [Azure-partner](https://pinpoint.microsoft.com/en-US/search?keyword=azure) eller skicka ett e-postmeddelande till *bigcompute@microsoft.com*.
> 
> 

### <a name="compute-services"></a>Beräkningstjänster
Azure compute-tjänster är hello kärnan i en stor Compute lösning och hello olika beräkning services erbjudande fördelar för olika scenarier. På en grundläggande nivå erbjuder dessa tjänster olika lägen för program toorun på virtuellt datorbaserat compute-instanser som Azure tillhandahåller med hjälp av Windows Server Hyper-V-teknik. Dessa instanser kan köra standardiserade eller anpassade Linux- och Windows-baserade operativsystem och verktyg. Azure ger dig möjlighet att välja [instansstorlekar](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) med olika konfigurationer av CPU-kärnor, minne, diskutrymme och andra egenskaper. Beroende på dina behov kan du skala hello instanser toothousands kärnor och skala när du behöver färre resurser.

> [!NOTE]
> Dra nytta av hello Azure [beräkningsintensiva instanser som hello H-serien](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooimprove hello prestanda och skalbarhet för HPC-arbetsbelastning. Dessa instanser stöder även parallella MPI-program som kräver ett nätverk med låg latens och hög genomströmning. Även tillgängliga är [N-serien](../virtual-machines/windows/sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) virtuella datorer med NVIDIA GPU tooexpand hello olika scenarier för databehandling och visualisering i Azure.  
> 
> 

| Tjänst | Beskrivning |
| --- | --- |
| **[Virtuella datorer](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• Tillhandahåller beräkning i en IaaS-modell (Infrastructure as a Service) med hjälp av Microsoft Hyper-V-teknik.<br/><br/>• Aktivera tooflexibly etablera och hantera beständiga molnet datorer från standard Windows Server- eller Linux-avbildningar från hello [Azure Marketplace](https://azure.microsoft.com/marketplace/), eller bilder och datadiskar som du anger<br/><br/>• Kan distribueras och hanteras som [Skalningsuppsättningar](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/) toobuild storskaliga tjänster från identiska virtuella datorer med autoskalning tooincrease eller minska kapacitet automatiskt<br/><br/>• Kör lokalt compute cluster verktyg och program helt i molnet hello<br/><br/> |
| **[Molntjänster](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• Kan köra Big Compute-program i arbetarrollinstanser, som är virtuella datorer som kör en version av Windows Server och som hanteras helt av Azure.<br/><br/>• Ger möjlighet att använda skalbara och tillförlitliga program med mycket lite administrativt arbete som körs i en PaaS-modell (Platform as a Service).<br/><br/>• Kan kräva ytterligare verktyg eller utveckling toointegrate med lokala HPC-kluster-lösningar |
| **[Batch](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• Kör storskaliga parallella arbetsbelastningar och batch-arbetsbelastningar i en helt hanterad tjänst.<br/><br/>• Tillhandahåller schemaläggning av jobb och automatisk skalning av en hanterad pool med virtuella datorer.<br/><br/>• Kan utvecklare toobuild och köra program som en tjänst eller moln – aktivera befintliga program<br/> |

### <a name="storage-services"></a>Lagringstjänster
En Big Compute-lösning fungerar vanligtvis med en uppsättning indata och genererar data som resultat. Hello Azure storage-tjänster som används i stor Compute lösningar bland annat:

* [Blob Storage, Table Storage och Queue Storage](https://azure.microsoft.com/documentation/services/storage/) – Hantera stora mängder ostrukturerade data, NoSQL-data och meddelanden för arbetsflöden respektive kommunikation. Du kan använda blob storage för stora datamängder för tekniska eller hello inkommande avbildningar t.ex mediefiler din programprocesser. Du kan använda köer för asynkron kommunikation i en lösning. Se [introduktion tooMicrosoft Azure Storage](../storage/common/storage-introduction.md).
* [Azure File storage](https://azure.microsoft.com/services/storage/files/) -resurser delade filer och data i Azure med hjälp av hello SMB-standardprotokollet, vilket krävs för vissa lösningar för HPC-kluster.
* [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) -innehåller storskaliga Apache Hadoop Distributed File System för hello moln, användbart för batch, realtid, och interaktiv analys.

### <a name="data-and-analysis-services"></a>Data- och analystjänster
Vissa Big Compute-scenarier omfattar storskaliga dataflöden eller genererar data som behöver ytterligare bearbetning eller analys. Azure erbjuder flera olika data- och analystjänster, inklusive:

* [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) – Bygger datadrivna arbetsflöden (pipelines) som ansluter, aggregerar och transformerar data från lokala, molnbaserade och Internetbaserade datalager.
* [SQL-databas](https://azure.microsoft.com/documentation/services/sql-database/) -ger hello viktiga funktioner i en Microsoft SQL Server system för relationsdatabashantering i en hanterad tjänst.
* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/) – distribuerar och etablerar Windows Server- eller Linux-baserade Apache Hadoop-kluster i hello molnet toomanage, analysera och rapportera om stordata.
* [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) – Hjälper dig att skapa, testa, använda och hantera lösningar för förutsägbar analys i en helt hanterad tjänst.

### <a name="additional-services"></a>Ytterligare tjänster
Stor Compute-lösningen behöver andra Azure-tjänster tooconnect tooresources lokalt eller i andra miljöer. Exempel:

* [Virtuellt nätverk](https://azure.microsoft.com/documentation/services/virtual-network/) -skapar ett logiskt isolerade avsnitt i Azure tooconnect Azure-resurser tooeach andra eller tooyour lokalt datacenter. Med ett virtuellt nätverk som sträcker sig över flera platser kan Big Compute-program komma åt lokala data, Active Directory-tjänster och licensservrar
* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) – Skapar privata anslutningar mellan Microsofts datacenter och infrastruktur som finns lokalt eller i en samplaceringsmiljö. ExpressRoute ger högre säkerhet, mer tillförlitlighet, högre hastighet och lägre svarstider än vanliga anslutningar över hello Internet.
* [Service Bus](https://azure.microsoft.com/documentation/services/service-bus/) -innehåller flera metoder för program toocommunicate eller exchange-data, om de finns på Azure, på en annan molnplattform eller i ett datacenter.

## <a name="next-steps"></a>Nästa steg
* Se [tekniska resurser för Batch- och HPC](big-compute-resources.md) toofind teknisk vägledning toobuild din lösning.
* Diskutera dina Azure-alternativ med partner, inklusive Cycle Computing, Rescale och UberCloud.
* Läs mer om Big Compute-lösningar i Azure från [Towers Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222), [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/), [ANSYS](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/) och [d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088).
* Senaste hello-meddelanden finns hello [Microsoft HPC och Batch-teamets blogg](http://blogs.technet.com/b/windowshpc/) och hello [Azure blogg](https://azure.microsoft.com/blog/tag/hpc/).

<!--Image references-->
[parallel]: ./media/batch-hpc-solutions/parallel.png
[coupled]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png
