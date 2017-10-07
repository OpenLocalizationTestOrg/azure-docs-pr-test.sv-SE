---
title: "aaaClusterize MySQL med belastningsutjämnade uppsättningar | Microsoft Docs"
description: "Konfigurera en belastningsutjämnad hög tillgänglighet Linux MySQL-kluster skapas med hello klassiska distributionsmodellen på Azure"
services: virtual-machines-linux
documentationcenter: 
author: bureado
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 6c413a16-e9b5-4ffe-a8a3-ae67046bbdf3
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/14/2015
ms.author: jparrel
ms.openlocfilehash: 1829fd877c4b0ed177b23a8e3404dbb3db746561
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-load-balanced-sets-tooclusterize-mysql-on-linux"></a>Använda belastningsutjämnade uppsättningar tooclusterize MySQL på Linux
> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager](../../../resource-manager-deployment-model.md) och klassisk. Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. En [Resource Manager-mall](https://azure.microsoft.com/documentation/templates/mysql-replication/) är tillgänglig om du behöver toodeploy MySQL-kluster.

Den här artikeln utforskar och visar hello olika metoder tillgängliga toodeploy Linux-baserade tjänster med hög tillgänglighet i Microsoft Azure, utforska MySQL hög tillgänglighet som en introduktion. En video som visar den här metoden är tillgängligt på [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).

Vi kommer disposition delat ingenting, två noder, single master MySQL lösning för hög tillgänglighet baserat på DRBD, Corosync och Pacemaker. Endast en nod kör MySQL i taget. Läsning och skrivning från hello DRBD resursen är också begränsad tooonly en nod i taget.

Det finns inget behov av en VIP-lösning som LVS, eftersom du ska använda belastningsutjämnade uppsättningar i Microsoft Azure tooprovide resursallokering funktioner och slutpunkten identifiering, borttagning och korrekt återställning av hello VIP. hello VIP är en globalt dirigeras IPv4-adress som tilldelats av Microsoft Azure när du skapar hello-Molntjänsten.

Det finns andra möjliga arkitekturer för MySQL, inklusive nästa arbetsdag kluster, Percona, Galera och flera mellanprogram lösningar, inklusive minst en tillgänglig som en virtuell dator på [VM anläggningen](http://vmdepot.msopentech.com). Så länge dessa lösningar kan replikera på unicast kontra multicast eller broadcast inte och på delad lagring eller flera nätverksgränssnitt, bör hello scenarier vara enkelt toodeploy på Microsoft Azure.

Dessa kluster arkitekturer kan utökas tooother produkter som PostgreSQL och OpenLDAP på ett liknande sätt. Till exempel belastningsutjämning proceduren med delat ingenting har har testats med flera master OpenLDAP och du kan titta på den på vår Channel 9-bloggen.

## <a name="get-ready"></a>Gör dig redo
Du behöver följande hello resurser och funktioner:

  - En Microsoft Azure-konto med en giltig prenumeration kan toocreate minst två virtuella datorer (XS användes i det här exemplet)
  - Ett nätverk och ett undernät
  - En tillhörighetsgrupp
  - En tillgänglighetsuppsättning
  - Hej möjlighet toocreate virtuella hårddiskar i hello samma region som hello tjänst i molnet och koppla dem toohello virtuella Linux-datorer

### <a name="tested-environment"></a>Testad miljö
* Ubuntu 13.10
  * DRBD
  * MySQL-Server
  * Corosync och Pacemaker

### <a name="affinity-group"></a>Tillhörighetsgruppen
Skapa en tillhörighetsgrupp för hello lösningen genom att logga in toohello klassiska Azure-portalen att välja **inställningar**, och skapa en tillhörighetsgrupp. Resurserna som skapas senare kommer att tilldelas toothis tillhörighetsgrupp.

### <a name="networks"></a>Nätverk
Ett nytt nätverk skapas och ett undernät som har skapats i hello-nätverk. Det här exemplet använder ett 10.10.10.0/24 nätverk med endast en /24 undernät i.

### <a name="virtual-machines"></a>Virtuella datorer
hello första Ubuntu 13.10 VM skapas med hjälp av en bild Endorsed Ubuntu galleriet och kallas `hadb01`. En ny molntjänst skapas i hello process kallas hadb. Det här namnet visas delade hello belastningsutjämnade natur hello-tjänsten har när du lägger till fler resurser. Hej skapandet av `hadb01` är primärdomänkontrollant och slutförda med hello-portalen. En slutpunkt för SSH skapas automatiskt och hello nytt nätverk har valts. Nu kan du skapa en tillgänglighetsuppsättning för hello virtuella datorer.

När hello första virtuella datorn skapas (tekniskt sett när hello Molntjänsten skapas), skapa hello andra VM `hadb02`. Hello andra i VM, använda Ubuntu 13.10 VM från hello galleriet med hello-portalen, men en befintlig molntjänst `hadb.cloudapp.net`, i stället för att skapa en ny. hello nätverk och tillgänglighet som ska väljas automatiskt. En SSH-slutpunkt skapas för.

När båda VM: ar har skapats, anteckna hello SSH-port för `hadb01` (TCP 22) och `hadb02` (tilldelade automatiskt av Azure).

### <a name="attached-storage"></a>Direktansluten lagring
Koppla en ny disk tooboth virtuella datorer och skapa 5 GB-diskar i hello process. hello diskar finns i hello VHD-behållare för huvudsakliga operativsystem-diskar. När diskar har skapats och ansluten, finns inga behov toorestart Linux eftersom hello kernel visas hello ny enhet. Den här enheten är vanligtvis `/dev/sdc`. Kontrollera `dmesg` för hello utdata.

Skapa en partition på varje virtuell dator med hjälp av `cfdisk` (primära Linux partition) och skriva hello nya partitionstabell. Skapa ett filsystem på den här partitionen.

## <a name="set-up-hello-cluster"></a>Konfigurera hello kluster
Använd LGH tooinstall Corosync och Pacemaker DRBD på båda Ubuntu VM: ar. toodo så med `apt-get`kör hello följande kod:

    sudo apt-get install corosync pacemaker drbd8-utils.

Installera inte MySQL just nu. Debian och Ubuntu installationsskript initiera en MySQL-datakatalog på `/var/lib/mysql`, men eftersom hello directory ska ersättas av ett DRBD filsystem, måste tooinstall MySQL senare.

Kontrollera (med hjälp av `/sbin/ifconfig`) att både virtuella datorer använder adresser i hello 10.10.10.0/24 undernät och att de kan pinga varandra efter namn. Du kan också använda `ssh-keygen` och `ssh-copy-id` toomake att båda VM: ar kan kommunicera via SSH utan att kräva ett lösenord.

### <a name="set-up-drbd"></a>Ställ in DRBD
Skapa en resurs för DRBD som använder hello underliggande `/dev/sdc1` partitions tooproduce en `/dev/drbd1` resurs som formaterats med ext3 och används i både primära och sekundära noder.

1. Öppna `/etc/drbd.d/r0.res` och kopiera hello följa resursdefinitionen på båda VM: ar:

        resource r0 {
          on `hadb01` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.4:7789;
            meta-disk internal;
          }
          on `hadb02` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.5:7789;
            meta-disk internal;
          }
        }

2. Initiera hello resursen med hjälp av `drbdadm` på både virtuella datorer:

        sudo drbdadm -c /etc/drbd.conf role r0
        sudo drbdadm up r0

3. På hello primära virtuella datorn (`hadb01`), force ägarskapet (primär) för hello DRBD resurs:

        sudo drbdadm primary --force r0

Om du undersöker hello innehållet i proc/drbd (`sudo cat /proc/drbd`) på båda VM: ar, bör du se `Primary/Secondary` på `hadb01` och `Secondary/Primary` på `hadb02`och konsekvent med nu hello-lösning. hello 5 GB disk är synkroniserad över hello 10.10.10.0/24 nätverk på toocustomers utan kostnad.

När hello disk synkroniseras, kan du skapa hello filsystemet på `hadb01`. I testsyfte kan vi använde ext2 men hello följande kod skapar ett ext3 filsystem:

    mkfs.ext3 /dev/drbd1

### <a name="mount-hello-drbd-resource"></a>Montera hello DRBD resurs
Du är nu redo toomount hello DRBD resurser på `hadb01`. Debian och derivat Använd `/var/lib/mysql` som Mysqls datakatalog. Eftersom du inte har installerat MySQL skapa hello katalog och montera hello DRBD resurs. tooperform det här alternativet Kör hello följande kod `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="set-up-mysql"></a>Ställ in MySQL
Nu är du redo tooinstall MySQL på `hadb01`:

    sudo apt-get install mysql-server

För `hadb02`, har du två alternativ. Du kan installera mysql-servern, vilket skapar /var/lib/mysql, Fyll den med en ny datakatalog, och ta sedan bort hello innehållet. tooperform det här alternativet Kör hello följande kod `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

hello andra alternativet är toofailover för`hadb02` och installera sedan mysql-server det. Installationsskript ser hello befintlig installation och påverkas inte.

Kör hello följande kod i `hadb01`:

    sudo drbdadm secondary –force r0

Kör hello följande kod i `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Om du inte planerar toofailover DRBD nu, är hello första alternativet enklare även om tvekan mindre elegant. När du har du skapat kan börja du arbeta på MySQL-databas. Kör hello följande kod i `hadb02` (eller vilken hello servrar är aktiv, enligt tooDRBD):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* tooroot;

> [!WARNING]
> Det här sista instruktionen inaktiverar autentisering för hello rotanvändaren i den här tabellen. Detta bör ersättas med din produktion klass ge instruktioner och ingår endast som illustration.

Om du vill toomake frågor från utanför hello virtuella datorer (som är hello syftet med den här guiden), måste du också tooenable nätverksfunktioner för MySQL. Öppna på båda VM: ar, `/etc/mysql/my.cnf` och gå för`bind-address`. Ändra hello-adressen från 127.0.0.1 too0.0.0.0. När du har sparat filen hello utfärda en `sudo service mysql restart` på din aktuella primära.

### <a name="create-hello-mysql-load-balanced-set"></a>Skapa hello MySQL belastningsutjämnad uppsättning
Gå tillbaka toohello portal, gå för`hadb01`, och välj **slutpunkter**. toocreate en slutpunkt, välj MySQL (TCP 3306) hello listrutan och välj **skapa nya belastningsutjämnade uppsättningen**. Namnet hello belastningsutjämnade endpoint `lb-mysql`. Ange **tid** too5 sekunder minsta.

När du har skapat hello endpoint gå för`hadb02`, Välj **slutpunkter**, och skapa en slutpunkt. Välj `lb-mysql`, och välj sedan MySQL hello nedrullningsbara listan. Du kan också använda hello Azure CLI för det här steget.

Nu har du allt du behöver för manuell åtgärd hello-klustret.

### <a name="test-hello-load-balanced-set"></a>Testa hello belastningsutjämnad uppsättning
Testerna kan utföras från en extern dator med hjälp av en MySQL-klient eller med hjälp av vissa program som körs som en Azure-webbplats phpMyAdmin. I så fall måste använt du Mysqls kommandoradsverktyget på en annan Linux ruta:

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>Manuellt växling
Du kan simulera redundans genom att stänga av MySQL, växla DRBDS primära och starta MySQL igen.

tooperform den här uppgiften kör hello följande kod på hadb01:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Klicka på hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

När du manuellt växla över du kan upprepa remote frågan och bör fungerar perfekt.

## <a name="set-up-corosync"></a>Ställ in Corosync
Corosync är hello underliggande klustret infrastruktur som krävs för Pacemaker toowork. Pulsslag (och andra metoder som Ultramonkey) är Corosync en delning av hello CRM-funktioner, medan Pacemaker är mer liknande tooHeartbeat i funktionalitet.

hello huvudsakliga begränsningen för Corosync i Azure är att Corosync föredrar multicast över broadcast över unicast-kommunikation, men Microsoft Azure-nätverk stöder endast unicast.

Lyckligtvis har Corosync en fungerande unicast-läge. hello endast verkliga begränsning är eftersom noderna inte kan kommunicera med varandra, måste toodefine hello noder i konfigurationsfilerna, inklusive deras IP-adresser. Vi kan använda hello Corosync exempel filer för unicast- och ändrar binda adress, nod listor och loggning kataloger (Ubuntu använder `/var/log/corosync` medan hello exempel filer Använd `/var/log/cluster`), och aktivera kvorum verktyg.

> [!NOTE]
> Använder följande hello `transport: udpu` direktiv och hello manuellt definierade IP-adresser för båda noderna.

Kör hello följande kod i `/etc/corosync/corosync.conf` för båda noderna:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

Kopiera konfigurationsfilen på både virtuella datorer och starta Corosync i båda noderna:

    sudo service start corosync

Hello kluster bör upprättas i hello aktuella ringen strax efter hello-tjänsten startas, och kvorum bör bildas. Vi kan kontrollera den här funktionen genom att granska loggarna eller genom att köra hello följande kod:

    sudo corosync-quorumtool –l

Utdata liknande toohello följande bild visas:

![exempel på utdata från corosync quorumtool - l](./media/mysql-cluster/image001.png)

## <a name="set-up-pacemaker"></a>Ställ in Pacemaker
Pacemaker använder hello klustret toomonitor för resurser, definiera när primärfärgerna kraschar och växla toosecondaries dessa resurser. Du kan definiera resurser från en uppsättning tillgängliga skript eller LSB (init-liknande) skript, bland andra alternativ.

Vi vill Pacemaker för ”egna” Hej DRBD resurs hello monteringspunkt och hello MySQL-tjänst. Om Pacemaker kan aktivera och inaktivera DRBD, montera och demontera den, och sedan starta och stoppa MySQL i hello i rätt ordning när ett fel inträffar med hello primära installationen är klar.

När du installerar Pacemaker ska konfigurationen vara enkelt, exempelvis:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

1. Kontrollera hello-konfigurationen genom att köra `sudo crm configure show`.
2. Skapa en fil (t.ex. `/tmp/cluster.conf`) med hello följande resurser:

        primitive drbd_mysql ocf:linbit:drbd \
              params drbd_resource="r0" \
              op monitor interval="29s" role="Master" \
              op monitor interval="31s" role="Slave"

        ms ms_drbd_mysql drbd_mysql \
              meta master-max="1" master-node-max="1" \
                clone-max="2" clone-node-max="1" \
                notify="true"

        primitive fs_mysql ocf:heartbeat:Filesystem \
              params device="/dev/drbd/by-res/r0" \
              directory="/var/lib/mysql" fstype="ext3"

        primitive mysqld lsb:mysql

        group mysql fs_mysql mysqld

        colocation mysql_on_drbd \
               inf: mysql ms_drbd_mysql:Master

        order mysql_after_drbd \
               inf: ms_drbd_mysql:promote mysql:start

        property stonith-enabled=false

        property no-quorum-policy=ignore

3. Läsa in hello-filen i hello-konfiguration. Du behöver bara toodo detta i en nod.

        sudo crm configure
          load update /tmp/cluster.conf
          commit
          exit

4. Kontrollera att Pacemaker startar vid start i båda noderna:

        sudo update-rc.d pacemaker defaults

5. Med hjälp av `sudo crm_mon –L`, kontrollera att en av noderna har blivit hello-master för hello kluster och kör alla hello-resurser. Du kan använda montera och ps toocheck hello resurser med.

följande skärmbild visar hello `crm_mon` med en nod som stoppats (avsluta genom att välja Ctrl + C):

![crm_mon noden stoppades](./media/mysql-cluster/image002.png)

Den här skärmbilden visar noder, en överordnad och en slav:

![crm_mon operativa över-/ underordnade](./media/mysql-cluster/image003.png)

## <a name="testing"></a>Testning
Du är redo för att simulera en automatisk redundans. Det finns två sätt toodo detta: mjuk och hårddisk.

hello mjuka sätt funktionen hello klustrets avstängning: ``crm_standby -U `uname -n` -v on``. Om du använder det här på hello master tar hello slavserver över. Kom ihåg tooset denna tillbaka toooff. Om du inte visas crm_mon en nod i vänteläge.

hello hårda sätt stängs ned hello primära virtuella datorn (hadb01) via hello-portalen eller genom att ändra hello är på hello VM (det vill säga stopp, avstängning). Detta hjälper Corosync och Pacemaker av signalering som hello master gå nedåt. Du kan testa detta (användbart för underhållsfönster), men du kan också tvinga hello scenario genom att frysa hello VM.

## <a name="stonith"></a>STONITH
Det bör vara möjligt tooissue en VM avstängning via hello Azure CLI i stället för ett STONITH skript som styr en fysisk enhet. Du kan använda `/usr/lib/stonith/plugins/external/ssh` som en bas- och aktivera STONITH i hello klusterkonfiguration. Azure CLI globalt ska installeras och hello Publiceringsinställningar och profil ska läsas för hello klustrets användare.

Exempelkod för hello resursen är tillgänglig på [GitHub](https://github.com/bureado/aztonith). Ändra hello klusterkonfigurationen genom att lägga till hello följande för`sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

> [!NOTE]
> hello skript utföra inte upp/ned kontroller. hello ursprungliga SSH resursen hade 15 ping-kontroller, men tiden för återställning för en virtuell dator i Azure kan vara mer variabel.

## <a name="limitations"></a>Begränsningar
hello följande begränsningar gäller:

* Hej linbit DRBD resurs skriptet som hanterar DRBD som en resurs i Pacemaker använder `drbdadm down` när du stänger en nod, även om bara hello nod försätts i vänteläge. Detta är inte idealiskt eftersom hello slavserver inte kommer att synkronisera hello DRBD resurs medan hello master hämtar skrivningar. Om hello master inte misslyckas graciously, kan hello slavserver ta över en äldre tillståndet för filsystemet. Det finns två möjliga sätt att lösa det här:
  * Framtvinga en `drbdadm up r0` i alla noder i klustret via ett lokalt (inte clusterized) watchdog
  * Redigera hello linbit DRBD skript, se till att `down` inte anropas`/usr/lib/ocf/resource.d/linbit/drbd`
* hello belastningsutjämnaren måste vara minst 5 sekunder toorespond så att program ska vara kluster och vara mer tolerant för timeout. Andra arkitekturer som app-köer och fråga middlewares kan också.
* MySQL justera är nödvändiga tooensure som skrivning görs i en hanterbar takt och cacheminnen är en toodisk så ofta som möjligt toominimize minne dataförlust.
* Skriva prestanda beror på virtuell dator kopplas samman i hello virtuell växel eftersom det är hello mekanismen som används av DRBD tooreplicate hello enhet.
