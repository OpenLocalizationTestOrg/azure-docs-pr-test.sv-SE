---
title: "aaaPerformance bästa praxis för SQL Server i Azure | Microsoft Docs"
description: "Innehåller metodtips för att optimera prestanda för SQL Server i virtuella Microsoft Azure-datorer."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a0c85092-2113-4982-b73a-4e80160bac36
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/27/2017
ms.author: jroth
ms.openlocfilehash: 42ec9fbeb2dec3a654b93bbd08d666369835ee73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>Prestandametodtips för SQL Server på virtuella Azure-datorer

## <a name="overview"></a>Översikt

Det här avsnittet innehåller metodtips för att optimera prestanda för SQL Server i Microsoft Azure-dator. När du kör SQL Server i Azure Virtual Machines, rekommenderar vi att du fortsätter med hello samma databas prestandajustering alternativ som är tillämpliga tooSQL Server i lokal server-miljö. Hello prestanda i en relationsdatabas i ett offentligt moln beror dock på många faktorer, till exempel hello storleken på en virtuell dator och hello konfigurationen av hello datadiskar.

När du skapar SQL Server-avbildningar, [överväga etablering dina virtuella datorer i hello Azure-portalen](virtual-machines-windows-portal-sql-server-provision.md). SQL Server-datorer som etablerats i hello Portal med Resource Manager implementera alla bästa praxis, inklusive hello lagringskonfiguration.

Den här artikeln fokuserar på komma hello *bästa* prestanda för SQL Server på virtuella Azure-datorer. Om din arbetsbelastning är mindre systemresurser, kanske inte kräver varje optimering nedan. Överväg att dina prestandabehov och mönster för arbetsbelastningen som du utvärdera de här rekommendationerna.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="quick-check-list"></a>Snabbkontrollen lista

hello följande är en snabb kontroll lista för optimala prestanda av SQL Server på Azure Virtual Machines:

| Område | Optimeringar |
| --- | --- |
| [VM-storlek](#vm-size-guidance) |[DS3](../../virtual-machines-windows-sizes-memory.md) eller högre för SQL Enterprise edition.<br/><br/>[DS2](../../virtual-machines-windows-sizes-memory.md) eller högre för SQL Standard och webb-utgåvor. |
| [Storage](#storage-guidance) |Använd [Premiumlagring](../../../storage/common/storage-premium-storage.md). Standardlagring rekommenderas endast för utveckling och testning.<br/><br/>Behåll hello [lagringskonto](../../../storage/common/storage-create-storage-account.md) och SQL Server-VM i hello samma region.<br/><br/>Inaktivera Azure [geo-redundant lagring](../../../storage/common/storage-redundancy.md) (geo-replikering) för hello storage-konto. |
| [Diskar](#disks-guidance) |Använd minst 2 [P30 diskar](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets) (1 för loggfiler, 1 för datafiler och TempDB).<br/><br/>Undvik att använda operativsystem eller tillfälliga diskar för databaslagring eller loggning.<br/><br/>Aktivera cachelagring för läsning på hello diskarna som är värd för hello datafiler och TempDB.<br/><br/>Aktivera inte cachelagring på diskarna som är värd för hello loggfilen.<br/><br/>Viktigt: Stoppa hello SQL Server-tjänsten när du ändrar hello cacheinställningarna för en virtuell dator i Azure-disken.<br/><br/>Stripe-flera Azure diskar tooget ökat IO datagenomströmning.<br/><br/>Formatera med dokumenterade allokering storlekar. |
| [I/O](#io-guidance) |Aktivera komprimering för databas-sidan.<br/><br/>Aktivera omedelbara filen initiering av datafiler.<br/><br/>Begränsa eller inaktivera automatisk storleksökning hello-databasen.<br/><br/>Inaktivera autoshrink på hello-databasen.<br/><br/>Flytta alla databaser toodata diskar, inklusive systemdatabaser.<br/><br/>Flytta SQL Server fel logg- och spårningsfiler kataloger toodata diskar.<br/><br/>Konfigurera säkerhetskopiering och databasen standardsökvägar.<br/><br/>Aktivera låsta sidor.<br/><br/>Tillämpa korrigeringar för SQL Server-prestanda. |
| [Funktionen specifika](#feature-specific-guidance) |Säkerhetskopiera direkt tooblob lagring. |

Mer information om *hur* och *varför* toomake dessa optimeringar granska hello information och riktlinjer som anges i följande avsnitt.

## <a name="vm-size-guidance"></a>Vägledning för VM-storlek

För känsliga program för prestanda, rekommenderas att du använder följande hello [storlekar för virtuella datorer](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json):

* **SQL Server Enterprise Edition**: DS3 eller högre
* **SQL Server Standard och webb-utgåvor**: DS2 eller högre

## <a name="storage-guidance"></a>Vägledning för lagring

Stöd för virtuella datorer av DS-serien (tillsammans med DSv2-serien och GS-serien) [Premiumlagring](../../../storage/common/storage-premium-storage.md). Premium Storage rekommenderas för alla produktionsarbetsbelastningar.

> [!WARNING]
> Standardlagring har olika svarstider och bandbredd och rekommenderas endast för arbetsbelastningar för utveckling och testning. Produktionsarbetsbelastningar bör använda Premium-lagring.

Dessutom rekommenderar vi att du skapar ditt Azure storage-konto i hello samma datacenter som din SQL Server-datorer tooreduce överföring fördröjningar. När du skapar ett lagringskonto, inaktivera geo-replikering som konsekvent skrivåtgärder ordning över flera diskar inte är säkert. I stället konfigurerar en SQL Server disaster recovery-tekniken mellan två Azure-datacenter. Mer information finns i [hög tillgänglighet och Haveriberedskap för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Vägledning för diskar

Det finns tre huvudsakliga disktyper på en virtuell dator i Azure:

* **OS-disken**: när du skapar en virtuell dator i Azure hello plattform ska kopplas till minst en disk (märkta som hello **C** enhet) toohello VM för din operativsystemdisken. Den här disken är en VHD som lagras som en sidblobb i lagringen.
* **Diskutrymme**: Azure Virtual Machines innehåller en annan disk kallas hello diskutrymme (märkta som hello **D**: enheten). Det här är en disk på hello-nod som kan användas för arbetsyta.
* **Datadiskar**: du kan också koppla ytterligare diskar tooyour virtuell dator som datadiskar och de lagras i lagring som sidblobar.

hello beskrivs följande avsnitt rekommendationer för att använda dessa olika diskar.

### <a name="operating-system-disk"></a>Operativsystemdisk

En operativsystemdisk är en virtuell Hårddisk som du kan starta och montera som kör version av ett operativsystem och etiketteras som **C** enhet.

Standard cachelagring principen på hello operativsystemdisk är **Skrivskyddstyp**. För känsliga program för prestanda rekommenderar vi att du använder datadiskar i stället för hello operativsystemdisken. I avsnittet hello på Datadiskar nedan.

### <a name="temporary-disk"></a>Diskutrymme

hello tillfälliga lagringsenhet, märkta som hello **D**: enheten är inte beständiga tooAzure blob-lagring. Lagra inte din användardatabasfiler eller användaren transaktionsloggfiler på hello **D**: enheten.

D-serien, Dv2-serien och G-serien virtuella datorer är hello temporärkatalog på dessa virtuella datorer SSD-baserad. Om din arbetsbelastning gör tunga användning av TempDB (t.ex. för temporära objekt eller komplexa kopplingar) lagrar TempDB på hello **D** enhet kan leda till högre TempDB genomflöde och lägre TempDB latens.

För virtuella datorer som har stöd för Premium-lagring (DS-serien, DSv2-serien och GS-serien) kan rekommenderar vi att du lagrar TempDB på en disk som har stöd för Premium-lagring med skrivskyddade cachelagring aktiverad. Det finns ett undantag toothis rekommendation. Om TempDB-användning är processorintensiva skrivning, kan du uppnå högre prestanda genom att lagra TempDB på hello lokala **D** enhet som också SSD-baserad på dessa datorstorlekar.

### <a name="data-disks"></a>Datadiskar

* **Använd datadiskar för data och loggfiler**: minst 2 Premium-lagring som använder [P30 diskar](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets) där en disk som innehåller hello loggfilerna och hello andra innehåller hello data och filer på TempDB. Varje disk i Premium-lagring finns ett antal IOPs och bandbredd (MB/s) beroende på dess storlek, enligt beskrivningen i följande artikel hello: [med Premium-lagring för diskar](../../../storage/common/storage-premium-storage.md).

* **Disk-Striping**: för snabbare dataflöde kan du lägga till ytterligare datadiskar och använda Disk Striping. toodetermine hello antalet diskar, behöver du tooanalyze hello antalet IOPS och bandbredd som krävs för din loggfilerna och dina data och filer på TempDB. Observera att olika storlekar på VM har olika begränsningar på hello antalet IOPs och bandbredd som stöds, se hello tabeller på IOPS per [VM-storlek](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Använd följande riktlinjer hello:

  * Windows 8/Windows Server 2012 eller senare, Använd [lagringsutrymmen](https://technet.microsoft.com/library/hh831739.aspx) med hello följande riktlinjer:

      1. Ange hello interleave (stripe-storlek) too64 KB (65 536 byte) för OLTP-arbetsbelastningar och 256 KB (262 144 byte) för informationslager arbetsbelastningar tooavoid prestandapåverkan på grund av toopartition feljusteringarna. Detta måste anges med PowerShell.
      1. Ange kolumnantal = antalet fysiska diskar. Använd PowerShell när du konfigurerar mer än 8 diskar (inte Server Manager UI). 

    Till exempel hello följande PowerShell skapar en ny lagringspool med hello interleave storlek too64 KB och hello antal kolumner too2:

    ```powershell
    $PoolCount = Get-PhysicalDisk -CanPool $True
    $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

    New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false 
    ```

  * För Windows 2008 R2 eller tidigare, kan du använda dynamiska diskar (OS stripe-volymer) och hello stripe-storlek är alltid 64 KB. Observera att det här alternativet används inte i Windows 8 och Windows Server 2012. Mer information finns i hello support-instruktionen på [Virtual Disk Service är övergående tooWindows Storage Management API](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

  * Om din arbetsbelastning inte loggen minnesintensiva och behöver inte dedikerade IOPs, kan du konfigurera en lagringspoolen. Annars skapar du två lagringspooler, en för hello loggfilerna och en annan lagringspool för hello datafiler och TempDB. Fastställa hello antalet diskar som är associerade med varje lagringspool utifrån belastningen förväntningar. Tänk på att olika storlekar på VM tillåter olika antal anslutna datadiskar. Mer information finns i [storlekar för virtuella datorer](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

  * Om du inte använder Premium-lagring (scenarier för utveckling och testning), hello rekommenderar vi tooadd hello maximala antalet datadiskar som stöds av din [VM-storlek](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) och använda Disk Striping.

* **Princip för cachelagring av**: för Premium-lagring datadiskar aktivera cachelagring för läsning på hello datadiskar endast värd datafiler och TempDB. Om du inte använder Premium-lagring, aktivera inte någon cachelagring på eventuella hårddiskar. Instruktioner om hur du konfigurerar diskcachelagring finns hello följande ämnen: [Set AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) och [Set AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx).

  > [!WARNING]
  > Stoppa hello SQL Server-tjänsten när du ändrar hello cache-inställningen för Virtuella Azure-diskar tooavoid hello möjligheten att databasen är skadad.

* **Storlek på allokeringsenhet NTFS**: formaterar hello datadisk och det rekommenderas att du använder en storlek på allokeringsenhet 64 KB för data och loggfiler samt TempDB.

* **Disk management metodtips**: när bort en datadisk eller ändra sin cache skriver, stoppa hello SQL Server-tjänsten under hello ändringen. När inställningar för cachelagring av hello har ändrats på hello OS-disken Azure stoppar hello VM, ändras hello cachetyp, och startar om hello VM. När hello inställningar för cachelagring av en datadisk som ändras, hello VM har inte stoppats, men hello datadisk är frånkopplat från hello VM under hello ändra och sedan anbringas på nytt.

  > [!WARNING]
  > Fel toostop hello SQL Server-tjänsten under dessa åtgärder kan orsaka databasfel.

## <a name="io-guidance"></a>I/o-vägledning

* hello bäst resultat med Premium-lagring uppnås när du parallelize programmet och förfrågningar. Premium-lagring är avsedd för scenarier där hello-i/o-ködjup är större än 1, så ser lite eller ingen prestandavinster för Enkeltrådig seriella begäranden (även om de är beräkningsintensiva lagring). Exempelvis kan detta påverka hello Enkeltrådig testresultat för prestanda analysverktyg som SQLIO.

* Överväg att använda [databasen sidan komprimering](https://msdn.microsoft.com/library/cc280449.aspx) som kan förbättra prestanda för i/o-intensiv arbetsbelastning. Hello datakomprimering kan dock öka hello processorförbrukningen på hello databasserver.

* Överväg att aktivera instant file tooreduce hello Initieringstid som krävs för inledande filen allokering. tootake nytta av instant file initieringen du ge hello tjänstkontot för SQL Server (MSSQLSERVER) med SE_MANAGE_VOLUME_NAME och lägga till den toohello **utföra underhållsaktiviteter** säkerhetsprincip. Om du använder en SQL Server-plattformsavbildning för Azure hello standard tjänstkonto (NT Service\MSSQLSERVER) är inte lagts till toohello **utföra underhållsaktiviteter** säkerhetsprincip. Med andra ord är instant file initieringen inte aktiverat i en SQL Server Azure plattformsavbildning. När du lägger till hello SQL Server service-kontot toohello **utföra underhållsaktiviteter** säkerhetsprincip, starta om hello SQL Server-tjänsten. Det kan finnas säkerhetsöverväganden vid användning av den här funktionen. Mer information finns i [databasen File Initialization](https://msdn.microsoft.com/library/ms175935.aspx).

* **automatisk storleksökning** anses toobe bara beredskap för oväntat tillväxt. Hantera inte dina data och loggfilen tillväxt för varje dagliga autogrow. Om automatisk storleksökning används före växa hello-filen med hello storlek växel.

* Kontrollera att **autoshrink** är inaktiverad tooavoid onödig som kan påverka prestanda negativt.

* Flytta alla databaser toodata diskar, inklusive systemdatabaser. Mer information finns i [flytta systemdatabaser](https://msdn.microsoft.com/library/ms345408.aspx).

* Flytta SQL Server fel logg- och spårningsfiler kataloger toodata diskar. Detta kan göras i Konfigurationshanteraren för SQL Server genom att högerklicka på SQL Server-instansen och välja egenskaper. hello fel logg- och spårningsfiler inställningar kan ändras i hello **Startparametrar** fliken hello målkatalog har angetts i hello **Avancerat** fliken hello följande skärmbild visar var toolook för hello fel loggen Start-parameter.

    ![Skärmbild för SQL-felloggen](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

* Konfigurera säkerhetskopiering och databasen standardsökvägar. Använd hello rekommendationer i det här avsnittet och hello ändringar i egenskapsfönstret för hello-Server. Instruktioner finns i [visar eller ändrar hello standard platser för Data och loggfiler (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). hello följande skärmbild visar var toomake ändringarna.

    ![SQL Data logg-och säkerhetskopiering](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)
* Aktivera låsta sidor tooreduce i/o och sidindelning aktiviteter. Mer information finns i [aktivera hello låsa sidor i minnet (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

* Installera Service Pack 1 Cumulative Update 10 om du kör SQL Server 2012. Den här uppdateringen innehåller hello korrigering för låga prestanda på i/o när du kör väljer till tillfällig tabell-satsen i SQL Server 2012. Mer information finns i det här [knowledge base-artikel](http://support.microsoft.com/kb/2958012).

* Överväg att komprimera några filer när du överför in/ut Azure.

## <a name="feature-specific-guidance"></a>Funktionen specifika anvisningar

Vissa distributioner kan uppnå ytterligare prestandafördelarna med mer avancerad konfiguration metoder. hello visar följande lista några SQL Server-funktioner som kan hjälpa dig att bättre prestanda för tooachieve:

* **Säkerhetskopiera tooAzure lagring**: när du utför säkerhetskopieringar för SQL Server körs i virtuella Azure-datorer, kan du använda [SQL Server-säkerhetskopiering tooURL](https://msdn.microsoft.com/library/dn435916.aspx). Den här funktionen är tillgänglig från och med SQL Server 2012 SP1 CU2 och rekommenderas för att säkerhetskopiera toohello anslutna diskar. När du säkerhetskopiering/återställning till/från Azure storage följa hello rekommendationer som anges i [SQL Server-säkerhetskopiering tooURL bästa praxis och felsöka och återställa från säkerhetskopior lagras i Azure Storage](https://msdn.microsoft.com/library/jj919149.aspx). Du kan också automatisera dessa säkerhetskopior med hjälp av [automatisk säkerhetskopiering för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

    Tidigare tooSQL Server 2012 som du kan använda [SQL Server-säkerhetskopiering tooAzure verktyget](https://www.microsoft.com/download/details.aspx?id=40740). Det här verktyget kan hjälpa tooincrease säkerhetskopiering dataflöde med flera säkerhetskopiering stripe-mål.

* **SQL Server-datafiler i Azure**: den här nya funktionen [SQL Server-datafiler i Azure](https://msdn.microsoft.com/library/dn385720.aspx), är tillgängliga från och med SQL Server 2014. Kör SQL Server med datafiler i Azure visar jämförbara prestandaegenskaper som med Azure datadiskar.

## <a name="next-steps"></a>Nästa steg

Metodtips för säkerhet, se [säkerhetsaspekter för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-security.md).

Granska andra virtuell dator med SQL Server-ämnen på [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).
