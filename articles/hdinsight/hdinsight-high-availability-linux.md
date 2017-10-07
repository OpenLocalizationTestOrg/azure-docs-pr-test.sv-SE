---
title: "aaaHigh tillgänglighet för Hadoop - Azure HDInsight | Microsoft Docs"
description: "Lär dig mer om hur HDInsight-kluster bättre tillförlitlighet och tillgänglighet med hjälp av en ytterligare huvudnod. Lär dig hur detta påverkar Hadoop-tjänster, till exempel Ambari och Hive, samt hur tooindividually ansluta tooeach huvudnod via SSH."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: Blackmist
documentationcenter: 
tags: azure-portal
keywords: "hög tillgänglighet för hadoop"
ms.assetid: 99c9f59c-cf6b-4529-99d1-bf060435e8d4
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 07/28/2017
ms.author: larryfr
ms.openlocfilehash: 9ff62afe6b63b241cb984225233157219f8d7411
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-reliability-of-hadoop-clusters-in-hdinsight"></a>Tillgänglighet och pålitlighet för Hadoop-kluster i HDInsight

HDInsight-kluster tillhandahåller två huvudnoderna tooincrease hello tillgänglighet och tillförlitlighet för Hadoop-tjänster och jobb som körs.

Hadoop ger hög tillgänglighet och tillförlitlighet genom att replikera data och tjänster över flera noder i ett kluster. Men standard Hadoop-distributioner har vanligtvis bara en huvudnod. Eventuella avbrott i hello huvudnod kan orsaka hello klustret toostop fungerar. HDInsight tillhandahåller två headnodes tooimprove Hadoop tillgänglighet och tillförlitlighet.

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="availability-and-reliability-of-nodes"></a>Tillgänglighet och tillförlitlighet för noder

Noder i ett HDInsight-kluster implementeras med hjälp av Azure virtuella datorer. hello beskrivs följande avsnitt enskilda hello nodtyper som används med HDInsight. 

> [!NOTE]
> Inte alla nodtyper används för en typ av kluster. Till exempel har en typ av Hadoop-kluster inte några Nimbus-noder. Mer information om noder som används av HDInsight-klustertyper i avsnittet hello klustret typer av hello [skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) dokumentet.

### <a name="head-nodes"></a>HEAD-noder

tooensure hög tillgänglighet för Hadoop-tjänster, HDInsight tillhandahåller två huvudnoderna. Båda huvudnoderna är aktiv och körs i hello HDInsight-kluster samtidigt. Vissa tjänster, till exempel HDFS eller YARN, är bara aktiva i en huvudnod vid en given tidpunkt. Andra tjänster som HiveServer2 eller Hive MetaStore är aktiva på båda huvudnoderna på hello samtidigt.

HEAD-noder (och andra noder i HDInsight) har ett numeriskt värde som en del av hello värdnamnet för hello-nod. Till exempel `hn0-CLUSTERNAME` eller `hn4-CLUSTERNAME`.

> [!IMPORTANT]
> Koppla inte hello numeriskt värde till om en nod är primär eller sekundär. hello numeriskt värde är endast tillgänglig tooprovide ett unikt namn för varje nod.

### <a name="nimbus-nodes"></a>Nimbus-noder

Nimbus-noder är tillgängliga med Storm-kluster. Hej Nimbus-noder ger liknande funktionalitet toohello Hadoop JobTracker genom att distribuera och övervaka bearbetning över arbetsnoder. HDInsight tillhandahåller två Nimbus-noder för Storm-kluster

### <a name="zookeeper-nodes"></a>Zookeeper-noder

[ZooKeeper](http://zookeeper.apache.org/) noder används för ledande val av master-tjänster på huvudnoderna. De är också använda tooinsure tjänster, (worker) datanoder och gateways vet vilken huvudnod en master tjänst är aktiv på. Standard tillhandahåller HDInsight tre ZooKeeper-noder.

### <a name="worker-nodes"></a>Arbetsnoder

Arbetsnoderna analysera hello faktiska data när ett jobb är skickade toohello klustret. Om en arbetsnod misslyckas är hello-aktiviteten som det fungerar skickade tooanother arbetsnoden. Som standard skapar HDInsight fyra arbetsnoderna. Du kan ändra det här antalet toosuit dina behov både under och efter klustret skapas.

### <a name="edge-node"></a>Kantnod

En kantnod deltar inte aktivt i dataanalys i hello kluster. Den används av utvecklare eller datavetare när du arbetar med Hadoop. hello edge nod liv i hello samma virtuella Azure-nätverk som hello andra noder i klustret hello och direkt åtkomst till alla andra noder. hello kantnod kan användas utan att göra resurser från kritiska Hadoop-tjänster eller analys jobb.

R Server på HDInsight är för närvarande hello enda typ av kluster som innehåller en kantnod som standard. R Server på HDInsight hello kantnod används testkod R lokalt på hello nod innan den skickas toohello kluster för distribuerad databehandling.

Information om hur du använder en kantnod med klustertyper än R Server finns hello [använder edge-noder i HDInsight](hdinsight-apps-use-edge-node.md) dokumentet.

## <a name="accessing-hello-nodes"></a>Åtkomst till hello noder

Åtkomst toohello klustret via hello internet tillhandahålls via en gemensam gateway. Hej kantnod åtkomst är begränsad tooconnecting toohello huvudnoderna och (om sådan finns). Åtkomst tooservices körs på hello huvudnoderna genomförs inte genom att låta flera huvudnoderna. hello offentliga gateway vägar begäranden toohello huvudnod som är värd för hello den begärda tjänsten. Om Ambari finns på hello andra huvudnod, dirigerar hello-gateway inkommande förfrågningar för Ambari toothat nod.

Åtkomst via hello offentliga gateway är begränsad tooport 443 (HTTPS), 22 och 23.

* Port __443__ är används tooaccess Ambari och andra web Användargränssnittet eller REST API: er finns i hello head noderna.

* Port __22__ används tooaccess hello primära huvudnod eller kantnod med SSH.

* Port __23__ är används tooaccess hello andra huvudnod med SSH. Till exempel `ssh username@mycluster-ssh.azurehdinsight.net` ansluter toohello primära huvudnod i hello kluster med namnet **mycluster**.

Mer information om hur du använder SSH finns hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.

### <a name="internal-fully-qualified-domain-names-fqdn"></a>Internt fullständigt kvalificerade domännamn (FQDN)

Noder i ett HDInsight-kluster har interna IP-adress och FQDN som endast kan nås från hello klustret. Vid åtkomst till tjänster på hello-kluster med hjälp av hello interna FQDN eller IP-adress, bör du använda Ambari tooverify hello IP eller FQDN toouse vid åtkomst till hello-tjänsten.

Till exempel hello Oozie kan endast köras på en huvudnod och använder hello `oozie` hello URL toohello tjänsten krävs för kommando från en SSH-session. URL: en kan hämtas från Ambari med hjälp av hello följande kommando:

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

Det här kommandot returnerar ett värde liknande toohello följande kommando, som innehåller hello Intern URL toouse med hello `oozie` kommando:

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

Mer information om hur du arbetar med hello Ambari REST API finns [övervaka och hantera HDInsight med hjälp av hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).

### <a name="accessing-other-node-types"></a>Åtkomst till andra nodtyper

Du kan ansluta toonodes som är inte direkt tillgänglig över hello internet med hjälp av hello följande metoder:

* **SSH**: när du är ansluten tooa huvudnod via SSH, du kan sedan använda SSH från hello huvudnod tooconnect tooother noderna i klustret hello. Mer information finns i hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.

* **SSH-Tunnel**: Om du behöver tooaccess som en webbtjänst som finns på en av hello-noder som inte är exponerad toohello internet, måste du använda en SSH-tunnel. Mer information finns i hello [använder en SSH-tunnel med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentet.

* **Virtuella Azure-nätverket**: om din HDInsight klustret är en del av ett virtuellt Azure-nätverk, en resurs på hello samma virtuella nätverk direkt åtkomst till alla noder i klustret hello. Mer information finns i hello [utöka HDInsight med hjälp av Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentet.

## <a name="how-toocheck-on-a-service-status"></a>Hur toocheck på en Tjänststatus

toocheck hello status för tjänster som körs på hello huvudnoderna kan använda hello Ambari-Webbgränssnittet eller hello Ambari REST API.

### <a name="ambari-web-ui"></a>Ambari-webbgränssnittet

Hej Ambari-Webbgränssnittet kan visas på https://CLUSTERNAME.azurehdinsight.net. Ersätt **KLUSTERNAMN** med hello namnet på klustret. Om du uppmanas ange hello HTTP-autentiseringsuppgifter för klustret. hello Standardanvändarnamnet för HTTP är **admin** och hello lösenordet är hello lösenordet du angav när du skapar hello kluster.

När du kommer på hello Ambari sidan tjänsterna hello installerade hello till vänster i hello-sidan.

![Installerade tjänster](./media/hdinsight-high-availability-linux/services.png)

Det finns ett antal ikoner som kan visas nästa tooa tooindicate Tjänststatus. Alla aviseringar relaterade tooa service kan granskas med hello **aviseringar** länk hello överst på hello sidan. Du kan markera varje tjänst tooview mer information om den.

Hello sida innehåller information om hello status och konfiguration för varje tjänst, innehåller den inte information som huvudnod hello-tjänsten körs på. tooview denna information, Använd hello **värdar** länk hello överst på hello sidan. Den här sidan visar värdar inom hello klustret, inklusive hello huvudnoderna.

![lista över värdar](./media/hdinsight-high-availability-linux/hosts.png)

Du väljer hello länk för en hello huvudnoderna visar hello tjänster och komponenter som körs på noden.

![Komponentstatus](./media/hdinsight-high-availability-linux/nodeservices.png)

Läs mer om hur du använder Ambari [övervaka och hantera HDInsight med Ambari-Webbgränssnittet för hello](hdinsight-hadoop-manage-ambari.md).

### <a name="ambari-rest-api"></a>Ambari REST API

Hej Ambari REST API är tillgänglig över hello internet. Hej HDInsight offentliga gateway hanterar routning begäranden toohello huvudnod som för närvarande är värd hello REST API.

Du kan använda följande kommando toocheck hello tillstånd för en tjänst via hello Ambari REST API hello:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* Ersätt **lösenord** med hello HTTP (admin) lösenord.
* Ersätt **KLUSTERNAMN** med hello hello klustrets namn.
* Ersätt **SERVICENAME** hello namnet på Hej tjänst du vill toocheck hello status.

Till exempel toocheck hello status för hello **HDFS** på ett kluster med namnet **mycluster**, med ett lösenord för **lösenord**, använder du följande kommando hello:

    curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

hello svaret är liknande toohello följande JSON:

    {
      "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
      "ServiceInfo" : {
        "cluster_name" : "mycluster",
        "service_name" : "HDFS",
        "state" : "STARTED"
      }
    }

hello URL talar om för oss att hello-tjänsten körs på en huvudnod med namnet **hn0 CLUSTERNAME**.

hello tillstånd talar om för oss att hello-tjänsten körs för närvarande, eller **igång**.

Om du inte vet vilka tjänster som är installerade på hello klustret använder du hello efter kommandot tooretrieve en lista:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

Mer information om hur du arbetar med hello Ambari REST API finns [övervaka och hantera HDInsight med hjälp av hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).

#### <a name="service-components"></a>Tjänstens komponenter

Tjänsterna kan innehålla komponenter som du vill toocheck hello status för individuellt. HDFS innehåller till exempel hello NameNode komponent. tooview information om en komponent, hello skulle kommandot se ut:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

Om du inte vet vilka komponenter som tillhandahålls av en tjänst kan använda du hello efter kommandot tooretrieve en lista:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-tooaccess-log-files-on-hello-head-nodes"></a>Hur tooaccess loggfiler på hello huvudnoderna

### <a name="ssh"></a>SSH

När anslutna tooa huvudnod via SSH loggfiler finns under **/var/log**. Till exempel **/var/log/hadoop-yarn/yarn** innehåller loggarna för YARN.

Varje huvudnod kan ha unika loggposter, så du bör kontrollera hello loggar på båda.

### <a name="sftp"></a>SFTP

Du kan också ansluta toohello huvudnod använder hello SSH File Transfer Protocol eller SFTP Secure File Transfer Protocol () och hämta hello loggfiler direkt.

Liknande toousing en SSH-klienten, när du ansluter toohello klustret måste du ange hello SSH användarens konto namn och hello SSH-adressen för hello klustret. Till exempel `sftp username@mycluster-ssh.azurehdinsight.net`. Ange hello lösenord för hello kontot när du uppmanas, eller ange en offentlig nyckel med hjälp av hello `-i` parameter.

När du är ansluten, visas en `sftp>` prompt. I det här meddelandet kan du ändra kataloger, ladda upp och hämta filer. Till exempel följande kommandon hello ändra kataloger toohello **/var/log/hadoop/hdfs** directory och sedan ladda ned alla filer i hello directory.

    cd /var/log/hadoop/hdfs
    get *

En lista över tillgängliga kommandon, ange `help` på hello `sftp>` prompt.

> [!NOTE]
> Det finns också grafiska gränssnitt som gör att du toovisualize hello-filsystem när ansluten via SFTP. Till exempel [MobaXTerm](http://mobaxterm.mobatek.net/) kan du toobrowse hello file system med en liknande gränssnittet tooWindows Explorer.

### <a name="ambari"></a>Ambari

> [!NOTE]
> tooaccess loggfiler med Ambari måste du använda en SSH-tunnel. hello Webbgränssnitt för enskilda hello-tjänster visas inte offentligt på hello Internet. Information om hur du använder en SSH-tunnel finns hello [använda SSH-tunnlar](hdinsight-linux-ambari-ssh-tunnel.md) dokumentet.

Hello Ambari-Webbgränssnittet, Välj hello-tjänsten som du vill tooview loggar för (till exempel YARN). Använd sedan **snabblänkar** tooselect vilka huvudnod tooview hello-loggarna för.

![Med hjälp av snabb länkar tooview loggar](./media/hdinsight-high-availability-linux/viewlogs.png)

## <a name="how-tooconfigure-hello-node-size"></a>Hur tooconfigure hello nodstorlek

hello storleken på en nod kan endast väljas när klustret skapas. Du hittar en lista över hello annan VM-storlekar som finns tillgängliga för HDInsight på hello [HDInsight sida med priser](https://azure.microsoft.com/pricing/details/hdinsight/).

När du skapar ett kluster måste ange du hello storleken på hello noder. hello följande information innehåller information om hur toospecify hello storlek med hello [Azure-portalen][preview-portal], [Azure PowerShell][azure-powershell], och hello [Azure CLI][azure-cli]:

* **Azure-portalen**: när du skapar ett kluster måste du ange hello storleken på hello-noder som används av hello klustret:

    ![Bild av guiden för kluster skapas med val av storlek](./media/hdinsight-high-availability-linux/headnodesize.png)

* **Azure CLI**: när du använder hello `azure hdinsight cluster create` kommandot kan du ange hello storleken på hello head, arbetare och ZooKeeper-noder med hello `--headNodeSize`, `--workerNodeSize`, och `--zookeeperNodeSize` parametrar.

* **Azure PowerShell**: när du använder hello `New-AzureRmHDInsightCluster` cmdlet, kan du ange hello storleken på hello head, arbetare och ZooKeeper-noder med hello `-HeadNodeVMSize`, `-WorkerNodeSize`, och `-ZookeeperNodeSize` parametrar.

## <a name="next-steps"></a>Nästa steg

Använd hello följande länkar toolearn mer om vad som nämns i det här dokumentet.

* [Ambari REST-referens](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [Installera och konfigurera hello Azure CLI](../cli-install-nodejs.md)
* Installera och konfigurera [Azure PowerShell](/powershell/azure/overview)
* [Hantera HDInsight med Ambari](hdinsight-hadoop-manage-ambari.md)
* [Etablera Linux-baserade HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
