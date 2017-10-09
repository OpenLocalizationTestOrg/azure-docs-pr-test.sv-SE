---
title: "aaaConfigure HBase-kluster-replikering i virtuella nätverk - Azure | Microsoft Docs"
description: "Lär dig hur tooconfigure HBase-replikering för belastningsutjämning, hög tillgänglighet, noll avbrottstid migrering/uppdatera från en HDInsight version tooanother och katastrofåterställning."
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: ba1c44f26b7cbf4a7a88159b12b3e064ea9f9a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hbase-cluster-replication-within-virtual-networks"></a>Konfigurera HBase-kluster-replikering i virtuella nätverk

Lär dig hur tooconfigure HBase-replikering i ett virtuellt nätverk (VNet) eller mellan två virtuella nätverk.

Kluster-replikering använder en käll-push-metod. Ett HBase-kluster kan vara en käll- eller ett mål eller det kan uppfylla båda roller på samma gång. Replikering är asynkron och hello målet för replikering är slutliga konsekvensen. När hello källa tar emot en redigera tooa kolumnfamilj med replikering aktiverad, är att redigera spridda tooall målkluster. När data har replikerats från ett kluster tooanother är hello källa kluster och alla kluster som redan förbrukats hello data spårade tooprevent replikering slingor.

I den här självstudiekursen konfigurerar du en källa mål-replikeringen. Andra klustertopologier finns [referenshandbok för Apache HBase](http://hbase.apache.org/book.html#_cluster_replication).

HBase replikering användning ärenden för ett enda virtuellt nätverk:

* Belastningsutjämning – till exempel kör skanningar eller MapReduce-jobb på hello målklustret och vill föra in data på hello källklustret
* Hög tillgänglighet
* Migrera data från en HBase-kluster tooanother
* Uppgradera en Azure HDInsight-kluster från en version tooanother

HBase replikering användning fall för två virtuella nätverk:

* Haveriberedskap
* Belastningsutjämning och partitionering hello program
* Hög tillgänglighet

Du replikerar kluster med [skript åtgärd](hdinsight-hadoop-customize-cluster-linux.md) skript som finns i [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).

## <a name="prerequisites"></a>Krav
Innan du börjar följa de här självstudierna måste du ha en Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="configure-hello-environments"></a>Konfigurera hello-miljöer

Det finns tre möjliga konfigurationer:

- Två HBase-kluster i ett virtuellt Azure-nätverk
- Två HBase-kluster i två olika virtuella nätverk i hello samma region
- Två HBase-kluster i två olika virtuella nätverk i två olika regioner (geo-replikering)

toomake den enklare tooconfigure hello-miljöer, har vi skapat några [Azure Resource Manager-mallar](../azure-resource-manager/resource-group-overview.md). Om du föredrar tooconfigure hello miljöer genom att använda andra metoder, se:

- [Skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
- [Skapa HBase-kluster i Azure-nätverk](hdinsight-hbase-provision-vnet.md)

### <a name="configure-one-virtual-network"></a>Konfigurera ett virtuellt nätverk

Klicka på följande bild toocreate två HBase-kluster i hello hello samma virtuella nätverk. hello mallen lagras i [Azure QuickStart mallar](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-one-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>

### <a name="configure-two-virtual-networks-in-hello-same-region"></a>Konfigurera två virtuella nätverk i hello samma region

Klicka på följande bild toocreate två virtuella nätverk med VNet-peering och två HBase-kluster i hello hello samma region. hello mallen lagras i [Azure QuickStart mallar](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-two-vnets-same-region%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>



Det här scenariot kräver [VNet-peering](../virtual-network/virtual-network-peering-overview.md). hello mallen kan VNet-peering.   

HBase-replikering använder IP-adresser för hello ZooKeeper virtuella datorer. Du måste konfigurera statiska IP-adresser för hello mål HBase ZooKeeper-noder.

**tooconfigure statiska IP-adresser**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hello vänstra menyn klickar du på **resursgrupper**.
3. Klicka på resursgruppen som innehåller hello mål HBase-kluster. Detta är hello resursgrupp som du angav när du använde hello Resource Manager-mall toocreate hello miljö. Du kan använda hello filter toonarrow hello listrutan. Du kan se en lista över resurser som innehåller hello två virtuella nätverk.
4. Klicka på hello virtuella nätverk som innehåller hello mål HBase-kluster. Klicka till exempel **xxxx vnet2**. Du kan se tre enheter med namn som börjar med **nic-zookeepermode -**. Dessa enheter är hello tre ZooKeeper virtuella datorer.
5. Klicka på en hello ZooKeeper virtuella datorer.
6. Klicka på **IP-konfigurationer**.
7. Klicka på **ipConfig1** hello-listan.
8. Klicka på **Statiska**, och Skriv ned hello IP-adress. Du behöver hello IP-adress när du kör hello åtgärd tooenable replikering.

  ![HDInsight HBase replikering ZooKeeper statisk IP-adress](./media/hdinsight-hbase-replication/hdinsight-hbase-replication-zookeeper-static-ip.png)

9. Upprepa steg 6 tooset hello statisk IP-adress för hello andra två ZooKeeper-noder.

För hello mellan VNet-scenario, måste du använda hello **- IP-** växeln när du anropar hello **hdi_enable_replication.sh** skript åtgärd.

### <a name="configure-two-virtual-networks-in-two-different-regions"></a>Konfigurera två virtuella nätverk i två olika regioner

Klicka på hello följande bild toocreate två virtuella nätverk i två olika regioner. hello mallen lagras i en offentlig Azure Blob-behållare.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fdeploy-hbase-geo-replication.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>

Skapa en VPN-gateway mellan hello två virtuella nätverk. Instruktioner finns i [skapa ett VNet med en plats-till-plats-anslutning](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

HBase-replikering använder IP-adresser för hello ZooKeeper virtuella datorer. Du måste konfigurera statiska IP-adresser för hello mål HBase ZooKeeper-noder. tooconfigure statisk IP-adress, finns hello ”konfigurera två virtuella nätverk i hello samma region” i den här artikeln.

För hello mellan VNet-scenario, måste du använda hello **- IP-** växeln när du anropar hello **hdi_enable_replication.sh** skript åtgärd.

## <a name="load-test-data"></a>Läs in testdata

När du replikerar ett kluster måste du ange hello tabeller tooreplicate. I det här avsnittet ska du läsa in vissa data i hello källklustret. I nästa avsnitt om hello aktiverar du replikering mellan två kluster med hello.

Följ anvisningarna hello i [självstudier för HBase: komma igång med Apache HBase med Linux-baserade Hadoop i HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) toocreate en **kontakter** table och infoga data i hello-tabellen.

## <a name="enable-replication"></a>Aktivera replikering

hello följande steg visar hur toocall hello skriptet åtgärdsskriptet från hello Azure-portalen. Kör en skriptåtgärd med hjälp av Azure PowerShell och hello Azure-kommandoradsgränssnittet (CLI), finns i [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).

**tooenable HBase-replikering från hello Azure-portalen**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Öppna hello källa HBase-kluster.
3. Hello kluster-menyn, klickar du på **skriptåtgärder**.
4. Klicka på **skicka nya** från hello överkant hello-bladet.
5. Välj eller ange hello följande information:

  - **Namnet**: Ange **Aktivera replikering**.
  - **Bash Webbadress för skriptet**: Ange **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.
  - **HEAD**: markerad. Rensa hello andra nodtyper.
  - **Parametrarna**: hello följande exempel parametrar Aktivera replikering för alla befintliga hello-tabeller och kopiera alla hello data från hello källa klustret toohello målklustret:

            -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata

6. Klicka på **Skapa**. hello skript kan ta lite tid, särskilt när hello - copydata argument används.

Argument som krävs:

|Namn|Beskrivning|
|----|-----------|
|-s - src-kluster | Ange hello DNS-namnet på hello källa HBase-kluster. Till exempel: -s hbsrccluster,--src-kluster = hbsrccluster |
|-d,--dst-kluster | Ange hello DNS-namnet på hello mål (replik) HBase-kluster. Till exempel: -s dsthbcluster,--src-kluster = dsthbcluster |
|-sp,--src-ambari-lösenordet | Ange hello administratörslösenord för Ambari på hello källa HBase-kluster. |
|-dp,--dst-ambari-lösenordet | Ange hello administratörslösenord för Ambari på hello mål HBase-kluster.|

Valfria argument:

|Namn|Beskrivning|
|----|-----------|
|-su,--src-ambari-användare | Ange hello administratörsanvändarnamnet för Ambari på hello källa HBase-kluster. hello standardvärdet är **admin**. |
|-du--dst-ambari-användare | Ange hello administratörsanvändarnamnet för Ambari på hello mål HBase-kluster. hello standardvärdet är **admin**. |
|-t,--tabell-lista | Ange hello tabeller toobe replikeras. Till exempel:--tabell lista = ”tabell1; tabell2; tabell 3”. Om du inte anger tabeller, replikeras alla befintliga HBase-tabeller.|
|-m,--dator | Ange hello huvudnod där hello skriptåtgärd körs. hello-värdet är antingen hn1 eller hn0. Eftersom hn0 är vanligtvis busier, bör du använda hn1. Du kan använda det här alternativet när du kör hello $0 skriptet som en skriptåtgärd från hello HDInsight-portalen eller Azure PowerShell.|
|-ip | Det här argumentet är obligatorisk när du aktivera replikering mellan två virtuella nätverk. Det här argumentet fungerar som en växel toouse hello statiska IP-adresser för ZooKeeper-noder från repliken kluster i stället för FQDN-namn. hello statiska IP-adresser måste toobe förkonfigurerade innan du aktiverar replikering. |
|-cp, - copydata | Aktivera hello migrering av befintliga data på hello tabeller där replikering har aktiverats. |
|-rpm, -replikera-phoenix-metadata | Aktivera replikering på Phoenix systemtabeller. <br><br>*Använd det här alternativet med försiktighet.* Vi rekommenderar att du återskapar Phoenix tabeller på replik kluster innan du använder det här skriptet. |
|-h,--hjälp | Visa information om användning. |

Hej print_usage() avsnitt i hello [skriptet](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) ger en detaljerad förklaring av parametrarna.

När hello skriptåtgärder är har distribuerats, du använder SSH tooconnect toohello mål HBase-kluster och verifiera hello data har replikerats.

### <a name="replication-scenarios"></a>Replikeringsscenarier

hello visar följande lista du ibland allmän användning och deras parameterinställningarna:

- **Aktivera replikering för alla tabeller mellan hello två kluster**. Det här scenariot kräver inte hello kopia/migrering av befintliga data på hello tabeller och den använder inte Phoenix tabeller. Använd hello följande parametrar:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- **Aktivera replikering för specifika tabeller**. Använd följande parametrar tooenable replikering på tabell1 och tabell2 tabell 3 hello:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- **Aktivera replikering för specifika tabeller och kopiera hello befintliga data**. Använd följande parametrar tooenable replikering på tabell1 och tabell2 tabell 3 hello:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- **Aktivera replikering för alla tabeller med replikering phoenix metadata från källan toodestination**. Phoenix metadata replikering är inte exakt och ska aktiveras med försiktighet.

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a>Kopiera och migrera data

Det finns två separata skript åtgärd skript för att kopiera/migrera data när replikeringen har aktiverats:

- [Skriptet för små tabeller](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (några få gigabyte i storlek och övergripande kopian är förväntade toofinish på mindre än en timme)

- [Skriptet för stora tabeller](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (förväntade tootake som är längre än en timme toocopy)

Du kan följa samma procedur i hello [Aktivera replikering](#enable-replication) toocall hello skriptåtgärd med hello följande parametrar:

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

Hej print_usage() avsnitt i hello [skriptet](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) ger en detaljerad beskrivning av parametrar.

### <a name="scenarios"></a>Scenarier

- **Kopiera specifika tabeller (test1 test2 och test3) för alla rader som redigeras fram tills nu (aktuell tidsstämpel)**:

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  eller

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- **Kopiera specifika tabeller med angivet tidsintervall**:

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a>Inaktivera replikering

toodisable replikering, som du använder ett annat skript åtgärd skript som finns i [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh). Du kan följa samma procedur i hello [Aktivera replikering](#enable-replication) toocall hello skriptåtgärd med hello följande parametrar:

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari Password> <-all|-t "table1;table2;...">  

Hej print_usage() avsnitt i hello [skriptet](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) ger en detaljerad förklaring av parametrarna.

### <a name="scenarios"></a>Scenarier

- **Inaktivera replikering för alla tabeller**:

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  eller

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari username> --src-ambari-password=<source cluster Ambari password>

- **Inaktivera replikering för angivna tabeller (tabell1 tabell2 och tabell 3)**:

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a>Nästa steg

I kursen får du lära sig hur tooconfigure HBase-replikering mellan två datacenter. toolearn mer information om HDInsight och HBase, se:

* [Kom igång med Apache HBase i HDInsight][hdinsight-hbase-get-started]
* [Översikt över HDInsight HBase][hdinsight-hbase-overview]
* [Skapa HBase-kluster i Azure-nätverk][hdinsight-hbase-provision-vnet]
* [Analysera realtid Twitter-åsikter med HBase][hdinsight-hbase-twitter-sentiment]
* [Analysera sensordata med Storm och HBase i HDInsight (Hadoop)][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started-linux.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
