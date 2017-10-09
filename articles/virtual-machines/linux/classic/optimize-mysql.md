---
title: "aaaOptimize MySQL prestanda på Linux | Microsoft Docs"
description: "Lär dig hur toooptimize MySQL körs på en Azure-dator (VM) kör Linux."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0c1c7fc5-a528-4d84-b65d-2df225f2233f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: ningk
ms.openlocfilehash: 9e6458723233721e06f30b9de33635d403eefcba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a>Optimera MySQL prestanda på virtuella Azure Linux-datorer
Det finns många faktorer som påverkar prestanda MySQL på Azure, både i val av virtuell maskinvara och konfiguration av programvara. Den här artikeln fokuserar på optimera prestanda via lagring, system och konfigurationer för databasen.

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager](../../../resource-manager-deployment-model.md) och klassisk. Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Information om Linux VM optimeringar med hello Resource Manager-modellen finns [optimera din Linux VM på Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="utilize-raid-on-an-azure-virtual-machine"></a>Använda RAID på en virtuell Azure-dator
Lagring är hello viktiga faktor som påverkar databasprestanda i miljöer i molnet. Jämfört med tooa enskild disk RAID ger snabbare åtkomst via samtidighet. Mer information finns i [Standard RAID-nivåer](http://en.wikipedia.org/wiki/Standard_RAID_levels).   

I/o-genomflöde och svarstid för i/o i Azure kan förbättras genom RAID. Våra tester visar att i/o-genomflöde kan vara dubblerad och i/o-svarstid kan reduceras med hälften i genomsnitt när fördubblas hello antalet RAID-diskar (från två toofour, fyra tooeight osv.). Se [bilaga A](#AppendixA) mer information.  

Dessutom toodisk i/o, MySQL prestanda förbättras om du ökar hello RAID-nivå.  Se [bilaga B](#AppendixB) mer information.  

Du kan också tooconsider hello segmentstorleken. I allmänhet när du har en större segmentstorlek kan hämta du lägre omkostnader, särskilt för stora skrivningar. När hello segmentstorleken är för stor, kan det dock lägga till ytterligare kostnader som hindrar dig från att dra nytta av RAID. hello aktuella standardstorleken är 512 KB som bevisas toobe som är optimala för mest allmänna produktionsmiljöer. Se [bilaga C](#AppendixC) mer information.   

Det finns begränsningar på hur många diskar som du kan lägga till för olika virtuella typer. Dessa värden beskrivs i [storlekar för virtuella datorer och moln för Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Du behöver fyra anslutna diskar toofollow hello RAID exempel på data i den här artikeln, men du kan välja tooset in RAID med färre diskar.  

Den här artikeln förutsätter att du redan har skapat en virtuell Linux-dator och ha MYSQL installeras och konfigureras. Mer information om att komma igång, se hur tooinstall MySQL på Azure.  

### <a name="set-up-raid-on-azure"></a>Ställ in RAID på Azure
hello följande steg visar hur toocreate RAID på Azure med hjälp av hello Azure-portalen. Du kan också ställa in RAID med hjälp av Windows PowerShell-skript.
I det här exemplet ska vi konfigurera RAID 0 med fyra diskar.  

#### <a name="add-a-data-disk-tooyour-virtual-machine"></a>Lägg till en data disk tooyour virtuell dator
Gå toohello instrumentpanelen och välj hello virtuella toowhich som du vill tooadd en datadisk i hello Azure-portalen. I det här exemplet är hello virtuella mysqlnode1.  

<!--![Virtual machines][1]-->

Klicka på **diskar** och klicka sedan på **bifoga nya**.

![Virtuella datorer lägger du till disk](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

Skapa en ny 500 GB disk. Se till att **värden Cache inställningar** har angetts för**ingen**.  När du är klar klickar du på **OK**.

![Anslut tom disk](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


Detta lägger till en tom disk i den virtuella datorn. Upprepa detta steg tre gånger så att du har fyra datadiskar för RAID.  

Du kan se hello läggas till enheter i hello virtuell dator genom att titta på hello kernel message-loggen. Till exempel toosee detta på Ubuntu, Använd hello följande kommando:  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-hello-additional-disks"></a>Skapa RAID med hello ytterligare diskar
hello följande steg beskriver hur för[konfigurera programvarubaserad RAID på Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> Om du använder hello XFS filsystem, köra hello följande steg när du har skapat RAID.
>
>

tooinstall XFS på Debian och Ubuntu Linux myntverket Använd hello följande kommando:  

    apt-get -y install xfsprogs  

tooinstall XFS på Fedora, CentOS eller RHEL, Använd hello följande kommando:  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a>Ställ in en ny lagringssökväg
Använd hello följande kommando tooset in sökvägen till en ny lagring:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-hello-original-data-toohello-new-storage-path"></a>Kopiera hello ursprungliga toohello ny lagring datasökväg
Använd följande kommando toocopy toohello ny lagring datasökväg hello:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-hello-data-disk"></a>Ändra behörigheter så att MySQL kan komma åt (läsning och skrivning) hello datadisk
Använd följande kommando toomodify behörigheter hello:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-hello-disk-io-scheduling-algorithm"></a>Justera hello disk i/o schemaläggning algoritm
Linux implementerar fyra typer av i/o schemaläggning algoritmer:  

* NOOP algoritmen (åtgärden kan Nej)
* Tidsgräns för algoritmen (tidsgräns)
* Helt verkliga MSMQ-algoritmen (CFQ)
* Budget period algoritmen (Anticipatory)  

Du kan välja olika i/o-planeringsprogram under olika scenarier toooptimize prestanda. I en miljö med helt direktåtkomst finns det inte en betydande skillnader mellan hello CFQ och tidsgräns algoritmer för prestanda. Vi rekommenderar att du ställer in hello MySQL-databas miljö tooDeadline för stabilitet. Om det är mycket sekventiella i/o, kan CFQ minska diskens i/o-prestanda.   

NOOP eller tidsgräns kan få bättre prestanda än hello standard scheduler för SSD och annan utrustning.   

Tidigare toohello kernel 2.5 hello standard i/o schemaläggning algoritmen är tidsgräns. Från och med hello kernel 2.6.18 blev CFQ hello standard-i/o: schemaläggning-algoritmen.  Du kan ange den här inställningen när datorn startas i kernel eller ändra den här inställningen dynamiskt när hello system körs.  

hello som följande exempel visar hur toocheck och ange hello standard scheduler toohello NOOP algoritmen i hello Debian distribution familj.  

### <a name="view-hello-current-io-scheduler"></a>Visa hello aktuella i/o-Schemaläggaren
tooview hello scheduler kör hello följande kommando:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Du kommer se följande utdata som visar hello aktuella Schemaläggaren:  

    noop [deadline] cfq


### <a name="change-hello-current-device-devsda-of-hello-io-scheduling-algorithm"></a>Ändra hello aktuell enhet (/ dev/sda) av schemaläggning hello i/o-algoritm
Kör följande kommandon toochange hello aktuell enhet hello:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> Ange detta för /dev/sda enbart är inte användbar. Det måste anges på alla datadiskar där hello databasen finns.  
>
>

Du bör se hello följande utdata, vilket visar att grub.cfg har återskapats och den hello standard Schemaläggaren har uppdaterats tooNOOP:  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

För hello Red Hat distribution familj, behöver du bara hello följande kommando:

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a>Konfigurera systeminställningar filen operations
Ett bra tips är toodisable hello *atime* loggningsfunktionen i hello-filsystemet. Atime är hello senaste åtkomsttid för fil. När en fil används hello hello poster tidsstämpel i hello-loggen. Men används den här informationen sällan. Du kan inaktivera den om du inte behöver den, vilket minskar övergripande disktid åtkomst.  

toodisable atime loggning av du behöver toomodify hello filen system configuration filen/etc / fstab och lägga till hello **noatime** alternativet.  

Till exempel redigera hello vim /etc/fstab filen till hello noatime som visas i följande exempel hello:  

    # CLOUD_IMG: This file was created/modified by hello Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add hello “noatime” option below toodisable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Sedan återansluta hello filsystem med hello följande kommando:  

    mount -o remount /RAID0

Testa hello ändrade resultat. När du ändrar hello testfil uppdateras hello åtkomsttid inte. Hej följande exempel visar vilka hello koden ser ut före och efter ändring.

Innan:        

![Code innan åtkomst ändringar][5]

Efter:

![Kod efter åtkomst ändring][6]

## <a name="increase-hello-maximum-number-of-system-handles-for-high-concurrency"></a>Öka hello maximala tillåtna antalet system handtag för hög samtidighet
MySQL är en hög concurrency-databas. hello standardantalet samtidiga referenser är 1024 för Linux som räcker inte alltid. Använd följande steg tooincrease hello maximala samtidiga i handtag hello system toosupport hög samtidighet på MySQL hello.

### <a name="modify-hello-limitsconf-file"></a>Ändra hello limits.conf filen
tooincrease hello högsta tillåtna samtidiga handtag, Lägg till hello följande fyra rader i hello /etc/security/limits.conf fil. Observera att 65536 hello maximalt antal hello-system kan stödja.   

    * mjuka nofile 65536
    * hårda nofile 65536
    * mjuka nproc 65536
    * hårda nproc 65536

### <a name="update-hello-system-for-hello-new-limits"></a>Uppdatera hello system för hello nya gränser
tooupdate hello system kör hello följande kommandon:  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-hello-limits-are-updated-at-boot-time"></a>Se till att hello gränser uppdateras vid start
Placera hello följande startkommandon i hello /etc/rc.local filen så att den börjar gälla när datorn startas.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a>MySQL-databas optimering
tooconfigure MySQL på Azure som du kan använda hello samma prestandajustering strategi som du använder på en lokal dator.  

hello huvudsakliga i/o optimering reglerna är:   

* Öka hello cachestorlek.
* Minska i/o-svarstid.  

toooptimize MySQL-serverinställningar, kan du uppdatera hello my.cnf fil som är hello standardkonfigurationsfilen för både servern och klientdatorerna.  

hello är följande konfigurationsobjekt hello viktigaste faktorerna som påverkar prestanda MySQL:  

* **innodb_buffer_pool_size**: hello buffertpool innehåller buffrade data och hello index. Det här värdet vanligtvis too70 procent av fysiskt minne.
* **innodb_log_file_size**: Detta är hello gör om loggfilens storlek. Du kan använda gör om loggar tooensure att skrivåtgärder är snabb, tillförlitlig och återställas efter en krasch. Detta är inställt too512 MB, vilket ger tillräckligt med utrymme för att logga skrivåtgärder.
* **max_connections**: ibland program Stäng inte anslutningar korrekt. Ett större värde ger hello server mer tid toorecycle inaktiv anslutningar. hello maximalt antal anslutningar är 10 000 men hello rekommenderade maximala antalet är 5 000.
* **Innodb_file_per_table**: den här inställningen aktiverar eller inaktiverar hello möjligheten för InnoDB toostore tabeller i separata filer. Aktivera hello alternativet tooensure att flera avancerade administration-åtgärder kan användas effektivt. Ur en prestanda, den påskynda hello tabell utrymme överföring och optimera hello skräp hantering prestanda. hello bör inställning för det här alternativet är Aktiverat.</br></br>
Från MySQL 5.6 är hello standardinställningen ON, så ingen åtgärd krävs. För tidigare versioner är hello standardinställningen AVSTÄNGD. hello inställningen ska ändras innan data har lästs in, eftersom endast nya tabeller påverkas.
* **innodb_flush_log_at_trx_commit**: hello standardvärdet är 1, med hello scope too0 ~ 2. hello standardvärdet är hello alternativ som passar för fristående MySQL-databas. hello inställningen av 2 aktiverar hello de flesta dataintegritet och lämpar sig för Master i MySQL-kluster. hello inställningen 0 dataförluster som kan påverka tillförlitlighet (i vissa fall med bättre prestanda) och lämpar sig för underordnade i MySQL-kluster.
* **Innodb_log_buffer_size**: hello loggen buffert kan transaktioner toorun utan tooflush hello loggen toodisk innan hello transaktioner genomförande. Om det finns stora binära objekt eller textfältet, hello cache används snabbt och ofta disk-i/o ska utlösas. Det är bättre öka hello buffertstorlek om Innodb_log_waits tillstånd variabeln inte är 0.
* **query_cache_size**: hello bästa alternativet är toodisable från hello början. Ange query_cache_size too0 (detta är standardinställningen för hello i MySQL 5.6) och använda andra metoder toospeed frågor.  

Se [bilaga D](#AppendixD) för en jämförelse av prestanda före och efter hello optimering.

## <a name="turn-on-hello-mysql-slow-query-log-for-analyzing-hello-performance-bottleneck"></a>Aktivera hello MySQL långsam frågeloggen för att analysera hello flaskhalsar
hello MySQL långsam fråga loggen kan hjälpa dig identifiera hello långsam frågor för MySQL. Du kan använda MySQL-verktyg som när du har aktiverat hello MySQL långsam frågeloggen **mysqldumpslow** tooidentify hello flaskhalsar.  

Som standard är detta inte aktiverat. Aktivera hello långsam frågeloggen kan använda vissa processorresurser. Vi rekommenderar att du aktiverar detta tillfälligt för felsökning av flaskhalsar. tooturn hello långsam fråga logga in:

1. Ändra hello my.cnf-filen genom att lägga till hello efter rader toohello:

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. Starta om hello MySQL-servern.

        service  mysql  restart

3. Kontrollera om hello inställningen börjar gälla med hjälp av hello **visa** kommando.

![Långsamma frågeloggen ON][7]   

![Långsamma frågeloggen resultat][8]

I det här exemplet kan du se hello långsam fråga funktionen har aktiverats. Du kan sedan använda hello **mysqldumpslow** verktyget toodetermine prestandaflaskhalsar och optimera prestanda, till exempel lägga till index.

## <a name="appendices"></a>Tilläggen
hello följande är exempel test prestandadata framställs i labbmiljö riktade. De ger grundläggande på hello prestanda data trend med olika metoder för prestandajustering. hello resultat kan variera under olika versioner av miljö eller produkt.

### <a name="AppendixA"></a>Bilaga A  
**Diskprestanda (IOPS) med olika RAID-nivåer**

![Disk-IOPS med olika RAID-nivåer][9]

**Test-kommandon**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> hello arbetsbelastningen för det här testet använder 64 trådar, försök tooreach hello övre gränsen för RAID.
>
>

### <a name="AppendixB"></a>Bilaga B  
**MySQL prestanda (dataflöde) jämförelse med olika RAID-nivåer**   
(XFS filsystem)

![MySQL prestanda jämförelse med olika RAID-nivåer][10]  
![MySQL prestanda jämförelse med olika RAID-nivåer][11]

**Test-kommandon**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**MySQL prestanda (OLTP) jämförelse med olika RAID-nivåer**  
![MySQL prestanda (OLTP) jämförelse med olika RAID-nivåer][12]

**Test-kommandon**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <a name="AppendixC"></a>Bilaga C   
**Jämförelse av disk prestanda (IOPS) för olika segment storlekar**  
(XFS filsystem)

![][13]

**Test-kommandon**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

hello filstorlekar som används för detta test är 30 och 1 GB respektive, med RAID 0 (4 diskar) XFS filsystem.

### <a name="AppendixD"></a>Bilaga D  
**MySQL prestanda (dataflöde) jämförelse före och efter optimering**  
(XFS File System)

![MySQL prestanda (dataflöde) jämförelse före och efter optimering][14]

**Test-kommandon**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**hello konfigurationsinställning för standard och optimering är följande:**

| Parametrar | Standard | Optimering |
| --- | --- | --- |
| **innodb_buffer_pool_size** |Ingen |7 GB |
| **innodb_log_file_size** |5 MB |512 MB |
| **max_connections** |100 |5000 |
| **innodb_file_per_table** |0 |1 |
| **innodb_flush_log_at_trx_commit** |1 |2 |
| **innodb_log_buffer_size** |8 MB |128 MB |
| **query_cache_size** |16 MB |0 |

Mer detaljerad [optimering konfigurationsparametrar](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), se toohello [MySQL officiella instruktioner](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).  

  **Testmiljö**  

| Maskinvara | Information |
| --- | --- |
| Processor |AMD Opteron (TM)-Processor 4171 HAN / 4 kärnor |
| Minne |14 GB |
| Disk |10 GB-disk |
| Operativsystem |Ubuntu 14.04.1 LTS |

[1]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png

