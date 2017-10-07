---
title: "Premium-lagring för aaaHigh prestanda och Azure-hanterade diskar för virtuella datorer | Microsoft Docs"
description: "Läs mer om Premium-lagring med hög prestanda och hanterade diskar för virtuella Azure-datorer. Azure DS-serien, DSv2-serien GS-serien och Fs-serien virtuella datorer har stöd för Premium-lagring."
services: storage
documentationcenter: 
author: ramankumarlive
manager: aungoo-msft
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: ramankum
ms.openlocfilehash: 2474fa75116fe394672fde48520441fa80cf434f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="high-performance-premium-storage-and-managed-disks-for-vms"></a>Premium-lagring med hög prestanda och hanterade diskar för virtuella datorer
Azure Premium Storage ger stöd för hög prestanda, låg latens diskar för virtuella datorer (VM) med in-/ utdata (I/O)-intensiv arbetsbelastning. Virtuella diskar som använder Premium-lagring kan du lagra data på SSD-enheter (SSD). tootake nytta av hello hastighet och prestanda för lagring av premiumdiskar, du kan migrera befintliga Virtuella diskar tooPremium lagring.

Du kan bifoga flera premium storage diskar tooa VM i Azure. Använda flera diskar ger dina program in too256 TB lagringsutrymme per virtuell dator. Med Premium-lagring kan du uppnå 80 000 i/o-åtgärder per sekund (IOPS) per VM och en disk dataflöde in too2 000 MB per sekund (MB/s) per VM dina program. Läsåtgärder ger mycket låg latens.

Med Premium-lagring erbjuder Azure hello möjlighet tootruly lift och SKIFT krävande företagsprogram som Dynamics AX, Dynamics CRM, Exchange Server, SAP Business Suite och SharePoint-servergrupper toohello moln. Du kan köra prestanda-intensiva arbetsbelastningar i program som SQL Server, Oracle, MongoDB, MySQL och Redis, vilket kräver att konsekvent hög prestanda och låg latens.

> [!NOTE]
> För hello bästa prestanda för ditt program rekommenderar vi att du migrerar Virtuella diskar som kräver hög IOPS tooPremium lagring. Om disken inte kräver hög IOPS, kan du hjälpa gränsen kostnader genom att hålla den i Azure standardlagring. Standardlagring lagras VM diskdata på hårddiskar (HDD) i stället för på SSD-enheter.
> 

Azure erbjuder två sätt toocreate premiumdiskar lagring för virtuella datorer:

* **Ohanterade diskar**

    hello ursprungliga metoden är toouse ohanterad diskar. Ohanterad disken, kan du hantera hello storage-konton som du använder toostore hello virtuell hårddisk (VHD) filer som motsvarar tooyour Virtuella diskar. VHD-filer lagras som sidblobbar i Azure storage-konton. 

* **Hanterade diskar**

    När du väljer [Azure hanterade diskar](storage-managed-disks-overview.md), Azure hanterar hello storage-konton som du använder för din Virtuella diskar. Anger hello disktyp (Standard eller Premium) och hello storleken på hello-disk som du behöver. Azure skapar och hanterar hello disk du. Du har inte tooworry om placerar hello diskar i flera lagring tooensure för konton som du håller dig inom skalbarhetsbegränsningar för dina lagringskonton. Azure hanterar som du.

Vi rekommenderar att du väljer hanterade diskar, tootake nytta av de många funktionerna.

tooget igång med Premium-lagring [skapa din kostnadsfria Azure-konto](https://azure.microsoft.com/pricing/free-trial/). 

Mer information om att migrera dina befintliga virtuella datorer tooPremium lagring finns [konvertera en virtuell Windows-dator från ohanterade diskar toomanaged diskar](../virtual-machines/virtual-machines-windows-convert-unmanaged-to-managed-disks.md) eller [konvertera en Linux VM från ohanterade diskar toomanaged diskar](../virtual-machines/linux/convert-unmanaged-to-managed-disks.md).

> [!NOTE]
> Premium-lagring finns i de flesta regioner. Hello lista över tillgängliga regioner i [Azure-tjänster efter region](https://azure.microsoft.com/regions/#services), titta på hello regioner som stöds stöd för Premium storlek-serien virtuella datorer (DS-serien, DSV2-serien GS-serien och Fs-serien virtuella datorer) stöds.
> 

## <a name="features"></a>Funktioner

Här följer några av hello funktionerna i Premium-lagring:

* **Premium-lagringsdiskar**

    Premium Storage stöder Virtuella diskar som kan vara ansluten toospecific storlek-serien virtuella datorer. Premium-lagring stöder DS-serien, DSv2-serien GS-serien, Ls-serien och Fs-serien virtuella datorer. Du kan välja mellan sju diskstorlekar: P4 (32GB) P6 (64GB) P10 (128GB) P20 (512GB), P30 (1024GB), P40 (2 048 GB), p 50 (4095GB). P4 och P6 diskstorlekar stöds ännu bara för hanterade diskar. Varje diskstorleken har sin egen prestandakrav. Du kan koppla en eller flera diskar tooyour VM beroende på kraven för application. Vi beskriver hello specifikationer i detalj i [Premium-lagring skalbarhets- och prestandamål](#scalability-and-performance-targets).

* **Premium-sidblobbar**

    Premium-lagring stöder sidblobar. Använd sidblobbar toostore beständiga ohanterade diskar för virtuella datorer i Premium-lagring. Till skillnad från vanliga Azure Storage Premium-lagring inte stöder blockblobbar, Lägg till blobbar, filer, tabeller eller köer. Premium-sidblobbar stöder sex storlekar från P10 tooP50 och P60 (8191GiB). P60 Premium sidblob är inte stöds toobe anslutna som Virtuella diskar. 

    Alla objekt som placerats i ett premiumlagringskonto blir en sidblobb. Hej sidblob fäster tooone av hello etablerade storlekar som stöds. Det är därför ett premiumlagringskonto inte är avsedd toobe används toostore små blobbar.

* **Premium-lagringskonto**

    toostart med Premium-lagring, skapa ett premiumlagringskonto för ohanterade diskar. I hello [Azure-portalen](https://portal.azure.com), toocreate ett premiumlagringskonto väljer hello **Premium** prestandanivå. Välj hello **lokalt redundant lagring (LRS)** replikeringsalternativet. Du kan också skapa ett premiumlagringskonto genom att ange hello typ för**Premium_LRS** i något av följande platser hello:
    * [Storage REST API](https://docs.microsoft.com/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference) (version 2014-02-14 eller en senare version)
    * [Service Management REST API](http://msdn.microsoft.com/library/azure/ee460799.aspx) (version 2014-10-01 eller en senare version, för Azure klassiska distributioner)
    * [Azure Storage Resource Provider REST API](https://docs.microsoft.com/rest/api/storagerp) (för Azure Resource Manager distributioner)
    * [Azure PowerShell](../powershell-install-configure.md) (version 0.8.10 eller en senare version)

    toolearn om premium lagringskontogränser finns [Premium-lagring skalbarhets- och prestandamål](#premium-storage-scalability-and-performance-targets).

* **Lokalt redundant Premium-lagring**

    Ett premiumlagringskonto stöder endast lokalt redundant lagring som hello replikeringsalternativet. Lokalt redundant lagring behåller tre kopior av hello data inom en enskild region. För regional katastrofåterställning, du måste säkerhetskopiera Virtuella diskar i en annan region med hjälp av [Azure Backup](../backup/backup-introduction-to-azure-backup.md). Du måste också använda ett konto med geo-redundant lagring (GRS) som hello säkerhetskopieringsvalvet. 

    Azure använder ditt lagringskonto som en behållare för ohanterade diskarna. När du skapar Azure DS-serien, DSv2-serien GS-serien, eller Fs-serien virtuell dator med ohanterad diskar, och du väljer ett premiumlagringskonto, operativsystemet och datadiskar lagras i detta lagringskonto.

## <a name="supported-vms"></a>Virtuella datorer som stöds
Premium-lagring stöder DS-serien, DSv2-serien GS-serien, Ls-serien och Fs-serien virtuella datorer. Du kan använda standard och premium-lagringsdiskar med dessa VM-typer. Du kan inte använda premium lagringsdiskar med VM-serien som inte är Premium Storage-kompatibel.

Information om VM-typer och storlekar i Azure för Windows finns [Windows VM-storlekar](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Information om VM-typer och storlekar i Azure för Linux finns [Linux VM-storlekar](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Det här är några av hello funktionerna i hello DS-serien, DSv2-serien GS-serien, Ls-serien och Fs-serien virtuella datorer:

* **Molntjänsten**

    Du kan lägga till DS-serien VMs tooa tjänst i molnet som har endast DS-serien virtuella datorer. Lägg inte till DS-serien VMs tooan befintlig molntjänst som har en annan typ än DS-serien virtuella datorer. Du kan migrera dina befintliga virtuella hårddiskar tooa ny molntjänst som kan köras endast DS-serien virtuella datorer. Om du vill toouse hello samma virtuella IP-adressen för hello ny molntjänst som är värd för dina DS-serien virtuella datorer, Använd [reserverad IP-adress](../virtual-network/virtual-networks-instance-level-public-ip.md). GS-serien virtuella datorer kan läggas till tooan befintlig molntjänst som har endast GS-serien virtuella datorer.

* **Operativsystemdisken**

    Du kan ställa in Premium Storage VM-toouse en premium eller en standardoperativsystem disk. För bästa möjliga upplevelse med hello bör du använda en disk i Premium-lagring-baserat operativsystem.

* **Datadiskar**

    Du kan använda premium och standarddiskar i hello samma Premium Storage VM. Du kan använda Premium-lagring för att etablera en virtuell dator och bifoga flera beständiga data diskar toohello VM. Om det behövs, tooincrease hello kapacitet och prestanda för hello volym stripe du över diskarna.

    > [!NOTE]
    > Om du stripe-datadiskar för premium-lagring med hjälp av [lagringsutrymmen](http://technet.microsoft.com/library/hh831739.aspx), konfigurera lagringsutrymmen med 1 kolumn för varje disk som du använder. Annars övergripande vara prestanda för hello stripe-volym lägre än väntat på grund av en ojämn fördelning av trafik över hello diskar. Som standard i Serverhanteraren kan du skapa kolumner för in too8 diskar. Om du har mer än 8 diskar kan du använda PowerShell toocreate hello volym. Ange hello antalet kolumner manuellt. Annars fortsätter hello Serverhanteraren UI toouse 8 kolumner, även om du har flera diskar. Ange till exempel 32 kolumner om du har 32 diskar i en enda stripe-uppsättning. toospecify hello antalet kolumner hello virtuella disken använder i hello [New-VirtualDisk](http://technet.microsoft.com/library/hh848643.aspx) PowerShell cmdlet, Använd hello *NumberOfColumns* parameter. Mer information finns i [översikt över lagringsutrymmen](http://technet.microsoft.com/library/hh831739.aspx) och [lagring lagringsutrymmen vanliga frågor och svar](http://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).
    >
    > 

* **Cache**

    Virtuella datorer i hello storlek serie som stöd för Premium-lagring har en unik cachelagring funktion för hög genomflöde och svarstid. hello cachelagring kapaciteten överskrider underliggande diskprestanda för premium-lagring. Du kan ange hello disk princip på diskar med premium-lagring för cachelagring för**ReadOnly**, **ReadWrite**, eller **ingen**. hello standard disken princip för cachelagring är **ReadOnly** för alla diskar för premium-data, och **ReadWrite** för operativsystemet diskar. Använd hello korrigera cache-inställningen för optimala prestanda för ditt program. Till exempel för frekventa Läs-eller skrivskyddad diskar, till exempel SQL Server-datafiler, ställa in hello princip för cachelagring för**ReadOnly**. För skrivintensiv eller lässkyddad datadiskar som SQL Server-loggfiler, ställa in hello princip för cachelagring för**ingen**. toolearn mer information om hur du optimerar din design med Premium-lagring finns [Design för prestanda med Premium-lagring](storage-premium-storage-performance.md).

* **Analys**

    tooanalyze VM prestanda med hjälp av diskar i Premium-lagring, aktivera diagnostik för Virtuella datorer i hello [Azure-portalen](https://portal.azure.com). Mer information finns i [Azure VM övervakning med Azure Diagnostics tillägget](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/). 

    toosee diskprestanda, Använd operativsystem-baserat verktyg som [Windows Prestandaövervakaren](https://technet.microsoft.com/library/cc749249.aspx) för virtuella Windows-datorer och hello [iostat](http://linux.die.net/man/1/iostat) kommandot för virtuella Linux-datorer.

* **VM scale begränsningar och prestanda**

    Varje Premium-lagring stöds VM-storlek har gränser och prestandakrav för IOPS och bandbredd hello antalet diskar som kan bifogas per virtuell dator. När du använder premiumdiskar för lagring med virtuella datorer kan du kontrollera att det finns tillräcklig IOPS och bandbredd VM toodrive disk trafiken.

    En virtuell dator STANDARD_DS1 har till exempel en dedikerad bandbredd på 32 MB/s för premium lagringstrafik disk. En disk med P10 premium-lagring kan ge en bandbredd på 100 MB/s. Om en P10 premium storage disk är anslutna toothis VM, kan den bara gå in too32 MB/s. Den kan inte använda hello maximalt 100 MB/s hello P10 disken kan ge.

    Är för närvarande hello största VM i hello DS-serien hello Standard_DS15_v2. Hej Standard_DS15_v2 kan ge upp too960 MB/s över alla diskar. hello är största VM i hello GS-serien hello Standard_GS5. Hej Standard_GS5 kan ge upp too2 000 MB/s över alla diskar.

    Observera att dessa gränser för disk-trafik. Dessa begränsningar med inte cacheträffar och nätverkstrafik. En separat bandbredd är tillgänglig för VM-nätverkstrafik. Bandbredd för nätverkstrafik som skiljer sig från hello dedikerad bandbredd som används av premiumdiskar för lagring.

    Hello allra senaste informationen om högsta IOPS och genomströmning (bandbredd) för Premium-lagring stöds virtuella datorer, se [Windows VM-storlekar](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eller [Linux VM-storlekar](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

    Mer information om premium storage diskar och deras IOPS och genomströmning gränser finns hello tabellen i hello nästa avsnitt.

## <a name="scalability-and-performance-targets"></a>Mål för skalbarhet och prestanda
I det här avsnittet beskriver vi hello skalbarhet och prestanda mål tooconsider när du använder Premiumlagring.

Premium-lagringskonton har hello följande skalbarhetsmål:

| Total kapacitet | Totala bandbredden för ett lokalt redundant lagringskonto |
| --- | --- | 
| Disken kapacitet: 35 TB <br>Ögonblicksbild kapacitet: 10 TB | Konfigurera too50 Gigabit per sekund för inkommande<sup>1</sup> + utgående<sup>2</sup> |

<sup>1</sup> alla data (antal begäranden) som skickas tooa storage-konto

<sup>2</sup> alla data (svar) som tas emot från ett lagringskonto

Mer information finns i [skalbarhets- och prestandamål för Azure Storage](storage-scalability-targets.md).

Om du använder premium storage-konton för ohanterade diskar och programmet överskrider hello skalbarhetsmål för ett enda lagringskonto, kanske du vill toomigrate toomanaged diskar. Om du inte vill toomigrate toomanaged diskar, skapa ditt program toouse flera lagringskonton. Sedan partitionera data mellan dessa lagringskonton. Till exempel om du vill tooattach 51 TB diskar över flera virtuella datorer sprids dem två storage-konton. 35 TB är hello gränsen för ett enda premiumlagringskonto. Se till att ett enda premiumlagringskonto aldrig har mer än 35 TB etablerade diskar.

### <a name="premium-storage-disk-limits"></a>Premium Lagringsgränser disk
När du etablera en premium storage disk, hello hello diskens storlek anger hello högsta IOPS och genomströmning (bandbredd). Azure erbjuder sju typer av lagring premiumdiskar: P4 (hanterade diskar endast), P6 (hanterade diskar endast), P10, P20, P30, P40 och p 50. Varje premium storage disk har specifika gränser för IOPS och dataflöde. Gränser för hello disktyper beskrivs i följande tabell hello:

| Premium diskar typ  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Diskstorlek           | 32 GB| 64 GB| 128 GB| 512 GB            | 1 024 GB (1 TB)    | 2 048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPS per disk       | 120   | 240   | 500   | 2 300              | 5000              | 7500              | 7500              | 
| Dataflöde per disk | 25 MB per sekund  | 50 MB per sekund  | 100 MB per sekund | 150 MB per sekund | 200 MB per sekund | 250 MB per sekund | 250 MB per sekund | 

> [!NOTE]
> Kontrollera att tillräckligt med bandbredd är tillgänglig på din Virtuella toodrive disk trafik, enligt beskrivningen i [Premium-lagring stöds VMs](#premium-storage-supported-vms). Annars är dina diskgenomflödet och IOPS begränsad toolower värden. Maximalt dataflöde och IOPS baseras på hello VM gränser, inte på hello disk begränsningar som beskrivs i föregående tabell hello.  
> 
> 

Här följer några viktiga saker tooknow om skalbarhets- och prestandamål för Premium-lagring:

* **Etablerad kapacitet och prestanda**

    När du etablerar en premium storage-disk, till skillnad från vanlig lagring är du garanterat hello kapacitet, IOPS och dataflöde på disken. Om du skapar en p 50 disk, etablerar Azure exempelvis 4,095 GB lagringskapacitet, 7 500 IOPS och 250 MB/s genomströmning för disken. Programmet kan använda hela eller delar av hello kapacitet och prestanda.

* **Diskens storlek**

    Azure maps hello disk storlek (avrunda uppåt) toohello närmsta premium disk lagringsalternativ, som anges i hello-tabellen i föregående avsnitt hello. Till exempel klassificeras storleken för en disk på 100 GB som ett alternativ för P10. Den kan utföra in too500 IOPS, med upp too100 MB/s genomströmning. På samma sätt kan en disk som klassificeras som en P20 400 GB. Det går att utföra too2 300 IOPS, med 150 MB/s genomströmning.
    
    > [!NOTE]
    > Du kan enkelt öka hello storleken på befintliga diskar. Du kanske exempelvis vill tooincrease hello storleken på en disk på 30 GB too128 GB eller TB även too1. Eller vill du kanske tooconvert P20 disk tooa P30 disken eftersom du behöver mer kapacitet eller fler IOPS och genomflöde. 
    > 
 
* **I/o-storlek**

    hello storleken på ett i/o är 256 KB. Om hello data som överförs är mindre än 256 KB, anses 1 i/o-enhet. Större i/o-storlekar räknas som flera I/o storlek 256 KB. Till exempel räknas 1 100 KB i/o som 5 i/o-enheter.

* **Dataflöde**

    hello genomströmning begränsningen inkluderar skrivningar toohello disk och innehåller disk läsåtgärder som inte hanteras från hello cache. Till exempel har en disk P10 100 MB/s genomströmning per disk. Några exempel på giltiga genomströmning för en P10 disk visas i följande tabell hello:

    | Max genomströmning per P10 disk | Icke-cache läser från disk | Icke-cache skriver toodisk |
    | --- | --- | --- |
    | 100 MB/s | 100 MB/s | 0 |
    | 100 MB/s | 0 | 100 MB/s |
    | 100 MB/s | 60 MB/s | 40 MB/s |

* **Träffar i cache**

    Cacheträffar begränsas inte av hello allokerade IOPS eller genomströmning av hello disk. Till exempel när du använder en datadisk med en **ReadOnly** cache-inställningen på en virtuell dator som stöds av Premium-lagring, läsningar som hämtas från hello cache är inte ämne toohello IOPS och genomströmning caps av hello disk. Om hello arbetsbelastningen på en disk är huvudsakligen läser du kan hämta mycket hög genomströmning. hello-cachen är ämne tooseparate IOPS och genomströmning gränser på hello VM-nivå, baserat på hello VM-storlek. DS-serien virtuella datorer har ungefär 4 000 IOPS och 33 MB/s genomströmning per kärna för cache och lokala SSD I/o. GS-serien virtuella datorer har en gräns på 5 000 IOPS och 50 MB/s genomströmning per kärna för cache och lokala SSD I/o. 

## <a name="throttling"></a>Begränsning
Begränsning kan inträffa om programmet IOPS eller genomströmning överskrider hello allokerade gränserna för en disk för premium-lagring. Begränsning också inträffa om din totala disk-trafik över alla diskar på hello VM överskrider hello disk Bandbreddsgräns för hello VM. begränsning av tooavoid rekommenderar vi att du begränsar hello antalet väntande i/o-begäranden för hello disken. Använd en begränsning baserat på mål för skalbarhet och prestanda för hello disken du har etablerat och på hello disk bandbredd tillgänglig toohello VM.  

Programmet kan uppnå hello lägsta fördröjningen när den är utformad tooavoid begränsning. Men om hello antalet väntande i/o-begäranden för hello disken är för liten, programmet kan inte utnyttja hello IOPS och genomströmning gränsvärden som är tillgängliga toohello disk.

hello som följande exempel visar hur toocalculate begränsning nivåer. Alla beräkningar baseras på ett i/o-storlek på 256 KB.

### <a name="example-1"></a>Exempel 1
Programmet har bearbetat 495 i/o-enheter med 16 KB storlek i en sekund på en P10 disk. hello i/o-enheter räknas som 495 IOPS. Om du försöker en 2 MB-i/o i Hej samma sekund, hello summan av i/o-enheter är lika too495 + 8 IOPS. Det beror på 2 MB i/o = 2 048 KB / 256 KB = 8 i/o-enheter, när hello storlek på i/o är 256 KB. Eftersom hello summan av 495 + 8 överskrider hello 500 IOPS-gräns för hello disken, inträffar begränsning.

### <a name="example-2"></a>Exempel 2
Programmet har bearbetat 400 i/o-enheter med 256 KB storleken på en P10 disk. hello totala bandbredd är (400 &#215; 256) / 1 024 KB = 100 MB/s. En P10 disk är begränsad till 100 MB/s genomströmning. Om programmet försöker tooperform flera i/o-åtgärder i den andra, begränsas eftersom den överskrider gränsen hello allokerade.

### <a name="example-3"></a>Exempel 3
Du har en virtuell dator DS4 med två P30 diskar som är anslutna. Varje P30 disk kan 200 MB/s genomströmning. En virtuell dator DS4 har dock en totala diskkapaciteten bandbredd på 256 MB/s. Du kan inte enhet båda anslutna diskar toohello maximalt dataflöde på den här DS4 virtuell dator på hello samtidigt. tooresolve, du kan klara av trafik på 200 MB/s på en disk och 56 MB/s på hello andra disken. Om hello summan av disk-trafik färdas över 256 MB/s, begränsas disk trafik.

> [!NOTE]
> Om disken trafiken består främst av små i/o-storlekar, kommer ditt program som sannolikt nått gränsen för hello-IOPS innan hello genomströmning gränsen. Men om hello disk trafik främst består av stora i/o-storlek, kommer ditt program som sannolikt nått hello genomströmning gränsen först i stället för hello IOPS-gräns. Du kan utnyttja programmets IOPS och genomströmningskapaciteten med optimala i/o-storlek. Du kan också begränsa hello antalet väntande i/o-begäranden för en disk.
> 

toolearn mer information om hur man designar för hög prestanda genom att använda Premium-lagring finns [Design för prestanda med Premium-lagring](storage-premium-storage-performance.md).

## <a name="snapshots-and-copy-blob"></a>Ögonblicksbilder och kopiera Blob

toohello lagringstjänsten hello VHD-filen är en sidblobb. Du kan ta ögonblicksbilder av sidblobar och kopiera dem tooanother plats, till exempel tooa annat lagringskonto.

### <a name="unmanaged-disks"></a>Ohanterade diskar

Skapa [inkrementell ögonblicksbilder](storage-incremental-snapshots.md) för ohanterade premiumdiskar hello samma sätt som du använder ögonblicksbilder med standardlagring. Premium-lagring stöder endast lokalt redundant lagring som hello replikeringsalternativet. Vi rekommenderar att du skapar ögonblicksbilder och sedan kopiera hello ögonblicksbilder tooa geo-redundant standardlagringskonto. Mer information finns i [Azure redundans lagringsalternativ](storage-redundancy.md).

Om en disk ansluten tooa VM är vissa API-åtgärder på hello disken inte tillåtna. Du kan exempelvis utföra en [kopiera Blob](/rest/api/storageservices/Copy-Blob) åtgärden på den blobben om hello disken är ansluten tooa VM. I stället först skapa en ögonblicksbild av blobben med hjälp av hello [ögonblicksbild Blob](/rest/api/storageservices/Snapshot-Blob) REST API. Därefter utför hello [kopiera Blob](/rest/api/storageservices/Copy-Blob) av hello ögonblicksbild toocopy hello ansluten disk. Alternativt kan du koppla bort disk hello och utföra alla nödvändiga åtgärder.

hello följande begränsningar gäller toopremium storage blob ögonblicksbilder:

| Lagringsgränsen för Premium | Värde |
| --- | --- |
| Maximalt antal ögonblicksbilder per blob | 100 |
| Kapacitet för lagringskonton för ögonblicksbilder<br>(Innehåller data endast ögonblicksbilder. Inkluderar inte data i grundläggande blob.) | 10 TB |
| Minimitid mellan på varandra följande ögonblicksbilder | 10 minuter |

toomaintain geo-redundant kopior av dina ögonblicksbilder, du kan kopiera ögonblicksbilder från ett konto tooa geo-redundant standardlagring premiumlagringskonto med hjälp av AzCopy eller kopiera Blob. Mer information finns i [överföra data med kommandoradsverktyget azcopy hello](storage-use-azcopy.md) och [kopiera Blob](/rest/api/storageservices/Copy-Blob).

Detaljerad information om hur du gör REST-åtgärder mot sidblobbar i ett premiumlagringskonto finns [Blob tjänståtgärder med Azure Premium Storage](http://go.microsoft.com/fwlink/?LinkId=521969).

### <a name="managed-disks"></a>Hanterade diskar

En ögonblicksbild för hanterade diskar är en skrivskyddad kopia av hello hanterade diskar. hello ögonblicksbild lagras som standard hanterade disk. För närvarande [inkrementell ögonblicksbilder](storage-incremental-snapshots.md) stöds inte för hanterade diskar. toolearn hur tootake en ögonblicksbild för en hanterad disk, se [skapar en kopia av en virtuell Hårddisk lagras som en Azure-hanterad disk med hjälp av hanterade ögonblicksbilder i Windows](../virtual-machines/virtual-machines-windows-snapshot-copy-managed-disk.md) eller [skapar en kopia av en virtuell Hårddisk lagras som en Azure-hanterad disk med hjälp av hanterade ögonblicksbilder i Linux](../virtual-machines/linux/snapshot-copy-managed-disk.md).

Om hanterade diskar är anslutna tooa VM, är vissa API-åtgärder på hello disken inte tillåtna. Du kan till exempel generera en delad åtkomst (SAS)-signaturen tooperform en kopieringsåtgärd medan hello disken är anslutna tooa VM. I stället först skapa en ögonblicksbild av hello disken och sedan kopiera hello hello ögonblicksbilder. Du kan också koppla bort disk hello och generera en SAS tooperform hello kopieringen.


## <a name="premium-storage-for-linux-vms"></a>Premium-lagring för virtuella Linux-datorer
Du kan använda följande information toohelp du ställa in din virtuella Linux-datorer i Premium-lagring hello:

tooachieve skalbarhet mål i Premium-lagring för alla premium storage diskar med cachelagra set för**ReadOnly** eller **ingen**, måste du inaktivera ”barriärer” när du monterar hello-filsystemet. Du behöver inte barriärer i det här scenariot eftersom hello skriver toopremium lagringsdiskar blir hållbara för dessa inställningar för cachelagring. När hello write-förfrågan har slutförts korrekt har data skrivits toohello beständiga arkivet. toodisable ”barriärer” med någon av följande metoder hello. Välj hello en för filsystemet:
  
* För **reiserFS**, toodisable barriärer använda hello `barrier=none` montera alternativet. (Använd tooenable barriärer `barrier=flush`.)
* För **ext3/ext4**, toodisable barriärer använda hello `barrier=0` montera alternativet. (Använd tooenable barriärer `barrier=1`.)
* För **XFS**, toodisable barriärer använda hello `nobarrier` montera alternativet. (Använd tooenable barriärer `barrier`.)
* För premiumdiskar för lagring med cache som angetts för**ReadWrite**, aktivera hinder för hållbarhet för skrivning.
* För volymen etiketter toopersist när du startar om hello VM, måste du uppdatera /etc/fstab med hello universellt Unik identifierare (UUID) referenser toohello diskar. Mer information finns i [lägga till en disk hanterade tooa Linux VM](../virtual-machines/linux/add-disk.md).

hello följande Linux-distributioner har validerats för Azure Premium-lagring. För bättre prestanda och stabilitet med Premium-lagring, rekommenderar vi att du uppgraderar dina virtuella datorer tooone av dessa versioner minst (tooa eller senare). Vissa av hello versioner kräver hello senaste Linux Integration Services (LIS), v4.0, för Azure. toodownload och installera en distributionsplats länken hello som anges i följande tabell hello. Vi lägga till bilder toohello lista när vi slutför verifieringen. Observera att vår verifieringar visar att prestandan varierar för varje bild. Prestanda beror på arbetsbelastningen egenskaper och inställningar för avbildningen. Olika bilder justerade för olika typer av arbetsbelastningar.

| Distribution | Version | Stöds kernel | Information |
| --- | --- | --- | --- |
| Ubuntu | 12.04 | 3.2.0-75.110+ | Ubuntu-12_04_5-LTS-amd64-Server-20150119-en-us-30GB |
| Ubuntu | 14.04 | 3.13.0-44.73+ | Ubuntu-14_04_1-LTS-amd64-Server-20150123-en-us-30GB |
| Debian | 7.x, 8.x | 3.16.7-ckt4-1+ | &nbsp; |
| SUSE | SLES 12| 3.12.36-38.1+| SUSE sles-12-prioritet-v20150213 <br> SUSE-sles-12-v20150213 |
| SUSE | SLES 11 SP4 | 3.0.101-0.63.1+ | &nbsp; |
| CoreOS | 584.0.0+| 3.18.4+ | Virtuell CoreOS 584.0.0 |
| CentOS | 6.5, 6.6, 6.7, 7.0 | &nbsp; | [LIS4 krävs](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Se anmärkning i nästa avsnitt om hello* |
| CentOS | 7.1+ | 3.10.0-229.1.2.el7+ | [LIS4 rekommenderas](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Se anmärkning i nästa avsnitt om hello* |
| Red Hat Enterprise Linux (RHEL) | 6.8+, 7.2+ | &nbsp; | &nbsp; |
| Oracle | 6.0+, 7.2+ | &nbsp; | UEK4 eller RHCK |
| Oracle | 7.0-7.1 | &nbsp; | UEK4 eller RHCK med[LIS 4.1 +](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |
| Oracle | 6.4-6.7 | &nbsp; | UEK4 eller RHCK med[LIS 4.1 +](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |


### <a name="lis-drivers-for-openlogic-centos"></a>LIS drivrutiner för OpenLogic CentOS

Om du kör OpenLogic CentOS virtuella datorer, kör följande kommando tooinstall hello senaste drivrutinerna hello:

```
sudo rpm -e hypervkvpd  ## (Might return an error if not installed. That's OK.)
sudo yum install microsoft-hyper-v
```

tooactivate hello nya drivrutiner, starta om hello.

## <a name="pricing-and-billing"></a>Priser och fakturering

När du använder Premiumlagring gäller hello efter fakturering överväganden:

* **Premium-lagring disk och blob-storlek**

    Fakturering för en disk för premium-lagring eller blob beror på hello etablerats storleken på hello disk eller blob. Azure maps hello etablerade storlek (avrunda uppåt) toohello närmsta premium lagringsalternativ för disken. Mer information finns i hello tabellen i [Premium-lagring skalbarhets- och prestandamål](#premium-storage-scalability-and-performance-targets). Varje disk maps tooa stöds etablerad diskstorlek och därefter faktureras. Fakturering för etablerade diskar är linjärt varje timme med hjälp av hello månadsavgift för hello Premium-lagring erbjudande. Om du etableras en P10 disken och tog bort den efter 20 timmar, debiteras du till exempel för hello P10 erbjudande proportionellt fördelad too20 timmar. Detta är oavsett hello mängden faktiska data toohello disk eller hello IOPS och genomströmning används.

* **Premium ohanterade diskar ögonblicksbilder**

    Ögonblicksbilder på ohanterade premiumdiskar debiteras för hello ytterligare kapacitet som används av hello ögonblicksbilder. Mer information om ögonblicksbilder finns [skapa en ögonblicksbild av en blob](/rest/api/storageservices/Snapshot-Blob).

* **Premium hanterade diskar ögonblicksbilder**

    En ögonblicksbild av en hanterad disken är en skrivskyddad kopia av hello disk. hello disk lagras som standard hanterade disk. En ögonblicksbild kostnader hello samma som en standard hanterad disk. Om du skapar en ögonblicksbild av en hanterad premium 128 GB disk, är hello kostnaden för hello ögonblicksbild likvärdiga tooa 128 GB standard hanterade diskar.  

* **Utgående dataöverföring**

    [Utgående dataöverföringar](https://azure.microsoft.com/pricing/details/data-transfers/) (data skickas från Azure-datacenter) debiteras för bandbreddsanvändning.

Detaljerad information om priser för Premium-lagring, Premium-lagring stöds virtuella datorer och hanterade diskar finns i följande artiklar:

* [Prissättning för Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Virtuella datorer priser](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Hanterade diskar priser](https://azure.microsoft.com/pricing/details/managed-disks/)

## <a name="azure-backup-support"></a>Azure Backup-support 

För regional katastrofåterställning, du måste säkerhetskopiera Virtuella diskar i en annan region med hjälp av [Azure Backup](../backup/backup-introduction-to-azure-backup.md) och ett GRS-lagringskonto som ett säkerhetskopieringsvalv.

toocreate en säkerhetskopiering med tidsbaserade säkerhetskopieringar, enkelt VM-återställning och principer för lagring av säkerhetskopior Använd Azure Backup. Du kan använda säkerhetskopiering båda med ohanterade och hanterade diskar. Mer information finns i [Azure Backup för virtuella datorer med ohanterad diskar](../backup/backup-azure-vms-first-look-arm.md) och [Azure Backup för virtuella datorer med hanterade diskar](../backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). 

## <a name="next-steps"></a>Nästa steg
Mer information om Premium-lagring finns i följande artiklar hello.

### <a name="design-and-implement-with-premium-storage"></a>Utforma och implementera med Premium-lagring
* [Design för prestanda med Premium-lagring](storage-premium-storage-performance.md)
* [BLOB storage-åtgärder med Premium-lagring](http://go.microsoft.com/fwlink/?LinkId=521969)

### <a name="operational-guidance"></a>Driftvägledning
* [Migrera tooAzure Premium-lagring](storage-migration-to-premium-storage.md)

### <a name="blog-posts"></a>Blogginlägg
* [Azure Premium Storage allmänt tillgängligt](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/)
* [Om hello GS-serien: lägga till Premium-lagring stöd toohello största virtuella datorer i hello offentligt moln](https://azure.microsoft.com/blog/azure-has-the-most-powerful-vms-in-the-public-cloud/)
