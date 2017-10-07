---
title: aaaRun MariaDB (MySQL) kluster i Azure | Microsoft Docs
description: "Skapa en MariaDB + Galera MySQL-kluster på Azure virtual machines"
services: virtual-machines-linux
documentationcenter: 
author: sabbour
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: d0d21937-7aac-4222-8255-2fdc4f2ea65b
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/15/2015
ms.author: asabbour
ms.openlocfilehash: f9a4d6c45d76478a8a3526b407c7bbe6aeb40423
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mariadb-mysql-cluster-azure-tutorial"></a>MariaDB (MySQL) kluster: Azure självstudiekursen
> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager](../../../resource-manager-deployment-model.md) och klassisk. Den här artikeln beskriver hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Azure Resource Manager-modellen.

> [!NOTE]
> MariaDB Enterprise kluster är nu tillgänglig i hello Azure Marketplace. hello nytt erbjudande distribuera automatiskt ett MariaDB Galera kluster på Azure Resource Manager. Du bör använda hello nytt erbjudande från [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).
>
>

Den här artikeln beskrivs hur du toocreate flera Master [Galera](http://galeracluster.com/products/) kluster med [MariaDBs](https://mariadb.org/en/about/) (en robust, skalbara och tillförlitliga gör att installera ersättning för MySQL) toowork i en miljö med hög tillgänglighet på Azure virtuella datorer.

## <a name="architecture-overview"></a>Översikt över arkitekturen
Den här artikeln beskriver hur toocomplete hello följande steg:

- Skapa ett kluster med tre noder.
- Separata hello datadiskar från hello OS-disken.
- Skapa hello datadiskar i RAID-0/stripe inställningen tooincrease IOPS.
- Använd Azure belastningsutjämnare toobalance hello belastningen för hello tre noder.
- toominimize återkommande fungerar, skapa en VM-avbildning som innehåller MariaDB + Galera och använda den toocreate hello andra kluster virtuella datorer.

![Systemarkitektur](./media/mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> Det här avsnittet använder hello [Azure CLI](../../../cli-install-nodejs.md) verktyg, så se till att toodownload dem och ansluter dem tooyour Azure-prenumeration bl.a toohello instruktioner. Om du behöver en referens toohello kommandon i hello Azure CLI finns hello [Azure CLI kommandoreferens](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Du måste också för[skapa en SSH-nyckel för autentisering] och anteckna hello .pem filens plats.
>
>

## <a name="create-hello-template"></a>Skapa hello mall
### <a name="infrastructure"></a>Infrastruktur
1. Skapa en tillhörighetsgrupp toohold hello resurser tillsammans.

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
2. Skapa ett virtuellt nätverk.

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
3. Skapa en storage-konto toohost våra diskar. Du bör placera mer än 40 hårt belastat diskar på hello samma storage-konto tooavoid träffa hello 20 000 IOPS lagringsgräns konto. I det här fallet är väl under denna gräns så att du lagrar allt på hello samma konto för enkelhetens skull.

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
4. Hitta hello namnet på hello CentOS 7 avbildning av virtuell dator.

        azure vm image list | findstr CentOS
   hello utdata blir något som liknar `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.

   Använd det här namnet i hello följande steg.
5. Skapa mall för virtuell dator hello och Ersätt /path/to/key.pem med hello sökvägen där du sparade hello genereras .pem SSH-nyckel.

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
6. Koppla fyra 500 GB data diskar toohello VM för användning i hello RAID-konfiguration.

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
7. Använda SSH toosign i toohello mall för virtuell dator som du skapade i mariadbhatemplate.cloudapp.net:22 och ansluta med hjälp av din privata nyckel.

### <a name="software"></a>Programvara
1. Hämta hello rot.

        sudo su

2. Installera stöd för RAID:

    a. Installera mdadm.

              yum install mdadm

    b. Skapa hello RAID0/stripe-konfigurationen med ett EXT4 filsystem.

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    c. Skapa hello avbildningskatalog punkt.

              mkdir /mnt/data
    d. Hämta hello UUID för hello nyskapad RAID-enhet.

              blkid | grep /dev/md0
    e. Redigera /etc/fstab.

              vi /etc/fstab
    f. Lägg till hello enheten tooenable automatisk montering på omstart, Ersätt hello UUID med hello-värde hämtades från hello tidigare **blkid** kommando.

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    g. Montera hello ny partition.

              mount /mnt/data

3. Installera MariaDB.

    a. Skapa hello MariaDB.repo fil.

                vi /etc/yum.repos.d/MariaDB.repo

    b. Fyll hello lagringsplatsen filen med hello följande innehåll:

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    c. tooavoid konflikter, ta bort befintliga username@Domain och mariadb-bibliotek.

           yum remove postfix mariadb-libs-*
    d. Installera MariaDB med Galera.

           yum install MariaDB-Galera-server MariaDB-client galera

4. Flytta hello MySQL data directory toohello RAID block-enhet.

    a. Kopiera hello MySQL-katalogen till den nya platsen och ta bort gamla hello-katalogen.

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    b. Ange behörigheter för hello ny katalog i enlighet med detta.

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    c. Skapa en symlink som pekar hello gamla toohello nya katalogplatsen på hello RAID-partitionen.

           ln -s /mnt/data/mysql /var/lib/mysql

5. Eftersom [SELinux kommer att störa klusteråtgärder hello](http://galeracluster.com/documentation-webpages/configuration.html#selinux), är det nödvändigt toodisable för hello aktuell session. Redigera `/etc/selinux/config` toodisable den för efterföljande omstart.

            setenforce 0

            then editing `/etc/selinux/config` tooset `SELINUX=permissive`
6. Validera MySQL körs.

   a. Starta MySQL.

           service mysql start
   b. Skydda hello MySQL installationen, ange hello rotlösenordet, ta bort anonyma användare toodisable remote rot inloggning och ta bort hello Testa databas.

           mysql_secure_installation
   c. Skapa en användare på hello databas för klusteråtgärder och eventuellt för dina program.

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* too'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   d. Stoppa MySQL.

            service mysql stop
7. Skapa en platshållare för konfigurationen.

   a. Redigera hello MySQL configuration toocreate platshållare för hello klusterinställningar. Ersätt inte hello ** `<Variables>` ** eller ta bort kommentarerna nu. Som sker när du har skapat en virtuell dator från den här mallen.

            vi /etc/my.cnf.d/server.cnf
   b. Redigera hello ** [galera] ** avsnittet och ta bort.

   c. Redigera hello **[mariadb]** avsnitt.

           wsrep_provider=/usr/lib64/galera/libgalera_smm.so
           binlog_format=ROW
           wsrep_sst_method=rsync
           bind-address=0.0.0.0 # When set too0.0.0.0, hello server listens tooremote connections
           default_storage_engine=InnoDB
           innodb_autoinc_lock_mode=2

           wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for hello SST cluster MySQL user
           #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
           #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
           #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
           #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set hello node name of this server
8. Öppna portar som krävs på hello-brandväggen med hjälp av FirewallD på CentOS 7.

   * MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
   * GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
   * GALERA IST:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
   * RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
   * Läs in hello brandväggen:`firewall-cmd --reload`

9. Optimera hello system för prestanda. Mer information finns i [strategi för prestandajustering](optimize-mysql.md).

   a. Redigera konfigurationsfilen för hello MySQL igen.

            vi /etc/my.cnf.d/server.cnf
   b. Redigera hello **[mariadb]** avsnittet och Lägg till hello följande innehåll:

   > [!NOTE]
   > Vi rekommenderar att innodb\_buffert\_pool_size är 70 procent av den Virtuella datorns minne. I det här exemplet, har den angetts på 2,45 GB för medelstora hello Azure VM med 3.5 GB RAM.
   >
   >

           innodb_buffer_pool_size = 2508M # hello buffer pool contains buffered data and hello index. This is usually set too70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give hello server more time toorecycle idled connections
           innodb_file_per_table = 1 # Speed up hello table space transmission and optimize hello debris management performance
           innodb_log_buffer_size = 128M # hello log buffer allows transactions toorun without having tooflush hello log toodisk before hello transactions commit
           innodb_flush_log_at_trx_commit = 2 # hello setting of 2 enables hello most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
10. Stoppa MySQL, inaktivera MySQL-tjänsten körs på Start tooavoid störa hello klustret när du lägger till en nod och ta bort etableringen hello-datorn.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
11. Avbilda hello VM hello-portalen. (För närvarande [utfärda #1268 i hello Azure CLI-verktygen](https://github.com/Azure/azure-xplat-cli/issues/1268) beskriver hello faktum att bilder som avbildas av hello Azure CLI-verktygen inte kan fångas hello kopplade datadiskar.)

    a. Stäng av datorn hello hello-portalen.

    b. Klicka på **avbilda** och ange hello Avbildningsnamnet som **mariadb galera avbildning**. Ange en beskrivning och markera ”jag har kört waagent”.
      
      ![Avbilda hello virtuella datorn](./media/mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-hello-cluster"></a>Skapa hello-kluster
Skapa tre virtuella datorer med hello mallen du skapat, och sedan konfigurera och starta hello klustret.

1. Skapa hello första CentOS 7-VM från hello mariadb-galera-avbildning bild du skapade, tillhandahåller hello följande information:

 - Virtuella nätverksnamnet: mariadbvnet
 - Undernät: mariadb
 - Datorn storlek: medelstor
 - Molntjänstnamnet: mariadbha (eller namnet du vill toobe som nås via mariadbha.cloudapp.net)
 - Datornamn: mariadb1
 - Användarnamn: azureuser
 - SSH-åtkomst: aktiverat
 - Skicka hello SSH certifikat PEM-filen och ersätter /path/to/key.pem med hello sökvägen där du sparade hello genereras .pem SSH-nyckel.

   > [!NOTE]
   > hello följande kommandon delas över flera rader för tydlighetens skull, men du bör ange varje som en rad.
   >
   >
        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser
2. Skapa två flera virtuella datorer genom att ansluta dem toohello mariadbha Molntjänsten. Ändra hello VM namn och hello SSH-port tooa unika-port som inte står i konflikt med andra virtuella datorer i hello samma tjänst i molnet.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
  För MariaDB3:

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser
3. Du behöver tooget hello interna IP-adressen för varje hello tre virtuella datorer för hello nästa steg:

    ![Hämtar IP-adress](./media/mariadb-mysql-cluster/IP.png)
4. Använd SSH toosign i toohello tre virtuella datorer och redigera hello konfigurationsfilen på var och en av dem.

        sudo vi /etc/my.cnf.d/server.cnf

    Ta bort kommentarerna ** `wsrep_cluster_name` ** och ** `wsrep_cluster_address` ** genom att ta bort hello ** # ** hello början av hello rad.
    Dessutom kan ersätta ** `<ServerIP>` ** i ** `wsrep_node_address` ** och ** `<NodeName>` ** i ** `wsrep_node_name` ** med hello Virtuella datorns IP-adress, namn, och Avkommentera dessa rader samt.
5. Starta hello kluster på MariaDB1 och låt den körs vid start.

        sudo service mysql bootstrap
        chkconfig mysql on
6. Starta MySQL på MariaDB2 och MariaDB3 och låt den körs vid start.

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-hello-cluster"></a>Läs in saldo hello kluster
När du har skapat hello klustrade virtuella datorer kan du lagt till dem i en tillgänglighetsuppsättning kallas clusteravset tooensure de placerades i olika fel- och update-domäner och att Azure aldrig har Underhåll på alla datorer på samma gång. Den här konfigurationen uppfyller hello toobe stöds av hello Azure servicenivåavtal (SLA).

Nu ska du använda Azure belastningsutjämnare toobalance begäranden mellan hello tre noder.

Kör följande kommandon på datorn med hjälp av hello Azure CLI hello.

Hej kommandostruktur parametrar är:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

hello CLI anger hello belastningen belastningsutjämnaren avsökningen intervall too15 i sekunder, som kan vara lite för långt. Ändra den i portalen hello under **slutpunkter** för någon av hello virtuella datorer.

![Redigera slutpunkt](./media/mariadb-mysql-cluster/Endpoint.PNG)

Välj **Reconfigure hello Load-Balanced ange**.

![Konfigurera om hello belastningsutjämnad uppsättning](./media/mariadb-mysql-cluster/Endpoint2.PNG)

Ändra **avsökning intervall** too5 sekunder och spara ändringarna.

![Ändra avsökningsintervall](./media/mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-hello-cluster"></a>Verifiera hello-kluster
hello tunga arbetet görs. hello klustret ska finnas nu tillgänglig för `mariadbha.cloudapp.net:3306`som träffar hello belastningsutjämnare och väg begäranden mellan hello tre virtuella datorer smidigt och effektivt sätt.

Använd din favorit MySQL klienten tooconnect eller ansluter från en hello VMs tooverify som fungerar som det här klustret.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Sedan skapar en databas och fylla det med vissa data.

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

hello-databasen som du skapade returnerar hello följande tabell:

    +----+--------+
    | id | value  |
    +----+--------+
    |  1 | Value1 |
    |  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg
Du har skapat en tre noder MariaDB + Galera Högtillgängliga kluster på Azure virtuella datorer körs CentOS 7 i den här artikeln. hello VMs belastningsutjämnas med Azure belastningsutjämnare.

Du kanske vill toolook på [ett annat sätt toocluster MySQL på Linux](mysql-cluster.md) och sätt för[optimera och testa MySQL prestanda på virtuella Azure Linux-datorer](optimize-mysql.md).

<!--Anchors-->
[Architecture overview]:#architecture-overview
[Creating hello template]:#creating-the-template
[Creating hello cluster]:#creating-the-cluster
[Load balancing hello cluster]:#load-balancing-the-cluster
[Validating hello cluster]:#validating-the-cluster
[Next steps]:#next-steps

<!--Image references-->

<!--Link references-->
[Galera]:http://galeracluster.com/products/
[MariaDBs]:https://mariadb.org/en/about/
[Skapa en SSH-nyckel för autentisering]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[issue #1268 in hello Azure CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268
