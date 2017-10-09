---
title: "aaaExclude diskar från skydd med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur och varför tooexclude Virtuella diskar från replikeringen för scenarier av VMware tooAzure och Hyper-V tooAzure."
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
ms.date: 06/05/2017
ms.author: nisoneji
ms.openlocfilehash: f47146bc57aeab3fce90123d0894fa86dde93417
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exclude-disks-from-replication"></a>Undanta diskar från replikering
Den här artikeln beskriver hur tooexclude diskar från replikeringen. Detta undantag kan optimera hello används replikering bandbredd eller optimera hello målsidan resurser som använder dessa diskar. hello stöds för scenarier med VMware tooAzure och Hyper-V tooAzure.

## <a name="prerequisites"></a>Krav

Som standard replikeras alla diskar på en dator. tooexclude en disk från replikeringen, måste du manuellt installera hello mobilitetstjänsten på hello datorn innan du aktiverar replikering om du replikerar från VMware tooAzure.


## <a name="why-exclude-disks-from-replication"></a>Varför ska jag undanta en disk från replikering?
Vanliga orsaker till att diskar undantas från replikering:

- hello-data som churned på hello exkluderade disk är inte viktigt eller behöver inte toobe replikeras.

- Vill du toosave lagring och nätverksresurser genom att inte replikera den här omsättning.

## <a name="what-are-hello-typical-scenarios"></a>Vad är vanliga scenarier för hello?
Du kan identifiera specifika exempel på dataomsättning som är bra kandidater för uteslutning. Bland exemplen finns skriver tooa växlingsfilen (pagefile.sys) och skriver toohello tempdb-filen för Microsoft SQL Server. Beroende på hello arbetsbelastning och hello underlagringssystemet registrera hello växlingsfilen mycket omsättning. Replikering av data från hello primär plats tooAzure skulle dock vara resurskrävande. Därför kan du använda följande steg toooptimize replikering av en virtuell dator med en enda virtuell disk som har både hello operativsystem och hello växlingsfilen hello:

1. Dela hello enskild virtuell disk i två virtuella diskar. En virtuell disk har hello operativsystemet och andra hello har hello växlingsfilen.
2. Exkludera hello sidindelning disken från replikering.

På samma sätt använder du följande steg toooptimize en disk som har båda hello Microsoft SQL Server tempdb Fä hello och hello system databasfilen:

1. Håll hello-databas och tempdb på två olika diskar.
2. Exkludera hello tempdb disk från replikering.

## <a name="how-tooexclude-disks-from-replication"></a>Hur tooexclude diskar från replikeringen?

### <a name="vmware-tooazure"></a>VMware tooAzure
Följ hello [Aktivera replikering](site-recovery-vmware-to-azure.md) arbetsflöde tooprotect en virtuell dator från hello Azure Site Recovery-portalen. I hello fjärde steget i hello arbetsflöde, använder du hello **DISK tooREPLICATE** kolumnen tooexclude diskar från replikeringen. Som standard är alla diskar markerade för replikering. Avmarkera kryssrutan för hello diskar som du vill tooexclude från replikering och sedan utför hello steg tooenable replikering.

![Exkludera diskar från replikering och aktivera replikering för VMware tooAzure återställning efter fel](./media/site-recovery-exclude-disk/v2a-enable-replication-exclude-disk1.png)


>[!NOTE]
>
> * Du kan exkludera diskar som redan har hello mobilitetstjänsten installerad. Du behöver toomanually installera hello mobilitetstjänsten, eftersom hello mobilitetstjänsten installeras bara med hello push mekanism efter replikering har aktiverats.
> * Endast standarddiskar kan undantas från replikering. Du kan inte undanta operativsystemdiskar eller dynamiska diskar.
> * När du har aktiverat replikering kan du inte lägga till eller ta bort diskar för replikering. Om du vill tooadd eller utesluta en disk, behöver toodisable skydd för hello datorn och aktivera det sedan igen.
> * Om du undantar en disk som behövs för ett program toooperate efter redundans tooAzure måste toocreate hello disk manuellt i Azure så att hello replikeras program kan köras. Alternativt kan du integrera Azure automation i en plan toocreate hello återställningsdisk under växling vid fel för hello-datorn.
> * Virtuell Windows-dator: Diskar som du skapar manuellt i Azure växlas inte tillbaka igen. Om du till exempel växlar över tre diskar och skapar två diskar direkt i Azure Virtual Machines kommer endast tre diskar som växlades över att växlas tillbaka. Du kan inte innehålla diskar som du har skapat manuellt i återställning efter fel eller skydda igen från lokala tooAzure.
> * Virtuell Linux-dator: Diskar som du skapar manuellt i Azure växlas tillbaka igen. Om du till exempel växlar över tre diskar och skapar två diskar direkt i Azure Virtual Machines kommer alla fem diskar att växlas tillbaka. Du kan inte undanta diskar som har skapats manuellt från återställningen.
>

### <a name="hyper-v-tooazure"></a>Hyper-V tooAzure
Följ hello [Aktivera replikering](site-recovery-hyper-v-site-to-azure.md) arbetsflöde tooprotect en virtuell dator från hello Azure Site Recovery-portalen. I hello fjärde steget i hello arbetsflöde, använder du hello **DISK tooREPLICATE** kolumnen tooexclude diskar från replikeringen. Som standard är alla diskar markerade för replikering. Avmarkera kryssrutan för hello diskar som du vill tooexclude från replikering och sedan utför hello steg tooenable replikering.

![Exkludera diskar från replikering och aktivera replikering för Hyper-V tooAzure återställning efter fel](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)

>[!NOTE]
>
> * Du kan endast undanta standarddiskar från replikering. Du kan inte undanta operativsystemdiskar. Vi rekommenderar att du inte undantar dynamiska diskar. Azure Site Recovery kan inte identifiera vilken virtuell hårddisk (VHD) som är vanliga eller dynamiska i hello virtuella gästdatorn.  Om alla diskar för beroende dynamisk volym inte är undantagna hello skyddade dynamisk disk blir fel på en disk på en virtuell dator för växling vid fel och hello data på disken är inte tillgänglig.
> * När du har aktiverat replikering kan du inte lägga till eller ta bort diskar för replikering. Om du vill tooadd eller utesluta en disk, behöver toodisable skydd för hello virtuella datorn och aktivera det sedan igen.
> * Om du undantar en disk som behövs för ett program toooperate efter redundans tooAzure behöver du toocreate hello disk manuellt i Azure så att hello replikeras program kan köras. Alternativt kan du integrera Azure automation i en plan toocreate hello återställningsdisk under växling vid fel för hello-datorn.
> * Diskar som du skapar manuellt i Azure växlas inte tillbaka igen. Om du misslyckas under tre diskar och skapa två diskar direkt i Azure Virtual Machines misslyckades exempelvis endast tre diskar som har växlats över tillbaka från Azure tooHyper-V. Du kan inte innehålla diskar som har skapats manuellt i återställning efter fel eller omvända replikeringen från Hyper-V tooAzure.



## <a name="end-to-end-scenarios-of-exclude-disks"></a>Undanta diskar i olika scenarier från slutpunkt till slutpunkt
Nu ska vi titta två scenarier toounderstand hello utelämna disk funktionen:

- SQL Server tempdb-disk
- Växlingsfildisk (pagefile.sys)

### <a name="exclude-hello-sql-server-tempdb-disk"></a>Exkludera hello SQL Server tempdb-disk
Anta att du har en virtuell dator som kör SQL Server och som har en tmpdb som kan undantas.

hello heter hello virtuell disk SalesDB.

Diskar på hello virtuella källdatorn är följande:


**Disknamn** | **Antal gästoperativsystem** | **Enhetsbeteckning** | **Datatyp av på hello disk**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Operativsystemdisk
DB-Disk1| Disk1 | D:\ | SQL-systemdatabas och användardatabas1
DB-Disk2 (exkluderade hello disk från skydd) | Disk2 | E:\ | Tillfälliga filer
DB-Disk3 (exkluderade hello disk från skydd) | Disk3 | F:\ | SQL-tempdb-databasen (mappsökvägen (F:\MSSQL\Data\) < /br/>< /br/> Skriv ned hello mappsökväg före redundans.
DB-Disk4 | Disk4 |G:\ |Användardatabas2

Eftersom dataomsättningen på två diskar för hello virtuella datorn är tillfällig, samtidigt som du skyddar hello SalesDB virtuella datorn, undanta Disk2 och Disk3 från replikering. Azure Site Recovery replikerar inte diskarna. Vid redundans, dessa diskar inte finns på hello redundans virtuell dator på Azure.

Diskar på hello virtuella Azure-datorn efter växling vid fel är följande:

**Antal gästoperativsystem** | **Enhetsbeteckning** | **Datatyp av på hello disk**
--- | --- | ---
DISK0 | C:\ | Operativsystemdisk
Disk1 | E:\ | Tillfällig lagring < /br / >< /br / > Azure lägger till den här disken och tilldelar hello första tillgängliga enhetsbeteckning.
Disk2 | D:\ | SQL-systemdatabas och användardatabas1
Disk3 | G:\ | Användardatabas2

Eftersom Disk2 och Disk3 har undantagits från hello SalesDB virtuell dator, är E: hello första enhetsbeteckningen hello tillgängliga listan. Azure tilldelar E: toohello temporära lagringsvolymen. För alla hello replikeras diskar hello hello enhet bokstäver förblir samma.

Disk3 som var hello SQL tempdb disk (tempdb mappsökväg F:\MSSQL\Data\), har exkluderats från repliken. hello disken är inte tillgänglig på hello redundans virtuella datorn. Därför hello SQL-tjänsten i ett stoppat tillstånd och den måste hello F:\MSSQL\Data sökväg.

Det finns två sätt toocreate sökvägen:

- Lägga till en ny disk och tilldela tempdb-mappsökvägen.
- Använd en befintlig disk för tillfällig lagring för hello tempdb mappsökväg.

#### <a name="add-a-new-disk"></a>Lägga till en ny disk:

1. Skriv ned hello sökvägar för SQL tempdb.mdf och tempdb.ldf före redundans.
2. Hello Azure-portalen, lägga till en ny disk toohello redundans virtuell dator med hello samma eller mer storlek som hello SQL tempdb källdisken (Disk3).
3. Logga in toohello virtuella Azure-datorn. Från hello (diskmgmt.msc) Diskhanteringskonsolen tillagda initiera och formatera hello nyligen disk.
4. Tilldela hello samma enhetsbeteckning som användes av hello SQL tempdb disk (F:).
5. Skapa en tempdb-mapp på hello volym F: (F:\MSSQL\Data).
6. Starta hello SQL-tjänsten från hello service-konsolen.

#### <a name="use-an-existing-temporary-storage-disk-for-hello-sql-tempdb-folder-path"></a>Använd en befintlig disk för tillfällig lagring för hello SQL tempdb mappsökväg:

1. Öppna en kommandotolk.
2. Kör SQL Server i återställningsläge från hello kommandotolk.

        Net start MSSQLSERVER /f / T3608

3. Kör hello följande sqlcmd toochange hello tempdb sökvägen toohello nya sökvägen.

        sqlcmd -A -S SalesDB        **Use your SQL DBname**
        USE master;     
        GO      
        ALTER DATABASE tempdb       
        MODIFY FILE (NAME = tempdev, FILENAME = 'E:\MSSQL\tempdata\tempdb.mdf');
        GO      
        ALTER DATABASE tempdb       
        MODIFY FILE (NAME = templog, FILENAME = 'E:\MSSQL\tempdata\templog.ldf');       
        GO


4. Stoppa hello Microsoft SQL Server-tjänsten.

        Net stop MSSQLSERVER
5. Starta hello Microsoft SQL Server-tjänsten.

        Net start MSSQLSERVER

Se toohello följande Azure riktlinjer för tillfällig Lagringsdisken:

* [Med hjälp av SSD-enheter i Azure Virtual Machines toostore SQL Server TempDB-bufferten poolen tillägg](https://blogs.technet.microsoft.com/dataplatforminsider/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/)
* [Prestandametodtips för SQL Server i Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance)

### <a name="failback-from-azure-tooan-on-premises-host"></a>Återställning (från Azure tooan lokal värd)
Nu ska vi förstå hello diskar som är replikerade när du redundansväxlar från Azure tooyour lokal VMware eller Hyper-V-värden. De diskar som du skapar manuellt i Azure replikeras inte. Om du till exempel växlar över tre diskar och skapar två diskar direkt i Azure Virtual Machines kommer endast tre diskar som växlades över att växlas tillbaka. Du kan inte innehålla diskar som har skapats manuellt i återställning efter fel eller skydda igen från lokala tooAzure. Även replikerar inte hello tillfällig lagring disk tooon lokala värdar.

#### <a name="failback-toooriginal-location-recovery"></a>Återställning för återställning efter fel toooriginal plats

I föregående exempel hello är hello diskkonfigurationen för virtuella Azure-datorn följande:

**Antal gästoperativsystem** | **Enhetsbeteckning** | **Datatyp av på hello disk**
--- | --- | ---
DISK0 | C:\ | Operativsystemdisk
Disk1 | E:\ | Tillfällig lagring < /br / >< /br / > Azure lägger till den här disken och tilldelar hello första tillgängliga enhetsbeteckning.
Disk2 | D:\ | SQL-systemdatabas och användardatabas1
Disk3 | G:\ | Användardatabas2


#### <a name="vmware-tooazure"></a>VMware tooAzure
När återställningen är klar toohello ursprungsplatsen saknar hello diskkonfigurationen för återställning av virtuell dator exkluderade diskar. Diskar som har undantagits från VMware tooAzure blir inte tillgängliga på hello återställning av virtuell dator.

Efter planerad redundans från Azure tooon lokal VMware är diskar på hello virtuell VMWare-dator (ursprungsplatsen) följande:

**Antal gästoperativsystem** | **Enhetsbeteckning** | **Datatyp av på hello disk**
--- | --- | ---
DISK0 | C:\ | Operativsystemdisk
Disk1 | D:\ | SQL-systemdatabas och användardatabas1
Disk2 | G:\ | Användardatabas2

#### <a name="hyper-v-tooazure"></a>Hyper-V tooAzure
När återställning efter fel toohello ursprungsplatsen hello hello återställning virtuell disk konfigurationen förblir samma som den ursprungliga diskkonfiguration av virtuell dator för Hyper-V. Diskar som har undantagits från tooAzure för Hyper-V-platsen är tillgängliga på hello återställning av virtuell dator.

Efter en planerad redundansväxling från Azure tooon lokala Hyper-V är-diskar på hello Hyper-V virtuell dator (ursprungsplatsen) följande:

**Disknamn** | **Antal gästoperativsystem** | **Enhetsbeteckning** | **Datatyp av på hello disk**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 |   C:\ | Operativsystemdisk
DB-Disk1 | Disk1 | D:\ | SQL-systemdatabas och användardatabas1
DB-Disk2 (utesluten disk) | Disk2 | E:\ | Tillfälliga filer
DB-Disk3 (utesluten disk) | Disk3 | F:\ | SQL tempdb-databasen (mappsökväg (F:\MSSQL\Data\)
DB-Disk4 | Disk4 | G:\ | Användardatabas2


#### <a name="exclude-hello-paging-file-pagefilesys-disk"></a>Exkludera hello sidindelning fil (pagefile.sys) disk

Anta att du har en virtuell dator som har en växlingsdisk som kan undantas.
Det finns två fall.

#### <a name="case-1-hello-paging-file-is-configured-on-hello-d-drive"></a>Fall 1: hello växlingsfilen är konfigurerad på hello D: enhet
Här är hello diskkonfigurationen:


**Disknamn** | **Antal gästoperativsystem** | **Enhetsbeteckning** | **Datatyp av på hello disk**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Operativsystemdisk
DB-disk 1 (exkluderade hello disk från hello skydd) | Disk1 | D:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Användardata 1
DB-Disk3 | Disk3 | F:\ | Användardata 2

Här följer hello inställningar för växlingsfiler på hello virtuella källdatorn:

![Inställningar för växlingsfiler på den virtuella källdatorn](./media/site-recovery-exclude-disk/pagefile-on-d-drive-sourceVM.png)


Efter en redundansväxling av hello virtuell dator från VMware tooAzure eller Hyper-V tooAzure är diskar på hello Azure-dator följande:

**Disknamn** | **Antal gästoperativsystem** | **Enhetsbeteckning** | **Datatyp av på hello disk**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Operativsystemdisk
DB-Disk1 | Disk1 | D:\ | Temporär lagring</br /> </br />pagefile.sys
DB-Disk2 | Disk2 | E:\ | Användardata 1
DB-Disk3 | Disk3 | F:\ | Användardata 2

Eftersom disk 1 (D:) uteslöts är D: hello första enhetsbeteckningen hello tillgängliga listan. Azure tilldelar D: toohello temporära lagringsvolymen. Eftersom D: är tillgänglig på hello Azure virtuella hello samma hello inställningen växlingsfil hello virtuella finns kvar.

Här följer hello inställningar för växlingsfiler på hello virtuella Azure-datorn:

![Inställningar för växlingsfiler på den virtuella Azure-datorn](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover.png)

#### <a name="case-2-hello-paging-file-is-configured-on-another-drive-other-than-d-drive"></a>Fall 2: hello växlingsfilen har konfigurerats på en annan enhet (än D:)

Här är hello källdatorns virtuell diskkonfiguration:

**Disknamn** | **Antal gästoperativsystem** | **Enhetsbeteckning** | **Datatyp av på hello disk**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Operativsystemdisk
DB-disk 1 (exklusive hello disk från skydd) | Disk1 | G:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Användardata 1
DB-Disk3 | Disk3 | F:\ | Användardata 2

Här följer hello inställningar för växlingsfiler på hello lokala virtuella datorn:

![Växlingsfilens på hello lokala virtuella datorn](./media/site-recovery-exclude-disk/pagefile-on-g-drive-sourceVM.png)

Efter en redundansväxling av hello virtuell dator från VMware/Hyper-V tooAzure är diskar på hello Azure-dator följande:

**Disknamn**| **Antal gästoperativsystem**| **Enhetsbeteckning** | **Datatyp av på hello disk**
--- | --- | --- | ---
DB-Disk0-OS | DISK0  |C:\ |Operativsystemdisk
DB-Disk1 | Disk1 | D:\ | Temporär lagring</br /> </br />pagefile.sys
DB-Disk2 | Disk2 | E:\ | Användardata 1
DB-Disk3 | Disk3 | F:\ | Användardata 2

Eftersom D: hello första enhetsbeteckningen tillgängliga hello listan tilldelar Azure D: toohello temporära lagringsvolymen. För alla hello replikeras diskar hello hello enhet bokstav förblir densamma. Eftersom hello G: disken inte är tillgänglig används hello hello C:-enheten för hello växlingsfil.

Här följer hello inställningar för växlingsfiler på hello virtuella Azure-datorn:

![Inställningar för växlingsfiler på den virtuella Azure-datorn](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover-2.png)

## <a name="next-steps"></a>Nästa steg
När du har konfigurerat och fått igång distributionen kan du [läsa mer](site-recovery-failover.md) om olika typer av redundansväxlingar.
