---
title: "aaaDeploy och hantera Apache Storm-topologier på Linux-baserade HDInsight | Microsoft Docs"
description: "Lär dig hur toodeploy, övervaka och hantera Apache Storm-topologier med hello Storm-instrumentpanelen på Linux-baserade HDInsight. Använda Hadoop-verktyg för Visual Studio."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 35086e62-d6d8-4ccf-8cae-00073464a1e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: 3a1edb773089cc596fea423710aa88cf83c7b841
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-hdinsight"></a>Distribuera och hantera Apache Storm-topologier på HDInsight

I detta dokument, lär du dig hello grunderna för att hantera och övervaka Storm-topologier som körs på Storm på HDInsight-kluster.

> [!IMPORTANT]
> hello krävs steg i den här artikeln ett Linux-baserade Storm på HDInsight-kluster. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). 
>
> Mer information om distribuera och övervaka topologier på Windows-baserade HDInsight finns [distribuera och hantera Apache Storm-topologier på Windows-baserade HDInsight](hdinsight-storm-deploy-monitor-topology.md)


## <a name="prerequisites"></a>Krav

* **En Linux-baserade Storm på HDInsight-kluster**: se [Kom igång med Apache Storm på HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) anvisningar om hur du skapar ett kluster

* (Valfritt) **Kunskap om SSH och SCP**: Mer information finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* (Valfritt) **Visual Studio**: Azure SDK 2.5.1 eller senare och hello Data Lake-verktyg för Visual Studio. Mer information finns i [komma igång med Data Lake-verktyg för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

    En av följande versioner av Visual Studio hello:

  * Visual Studio 2012 med [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)

  * Visual Studio 2013 med [uppdatering 4](http://www.microsoft.com/download/details.aspx?id=44921) eller [Visual Studio Community 2013](http://go.microsoft.com/fwlink/?LinkId=517284)
  * [Visual Studio 2015](https://www.visualstudio.com/downloads/)

  * Visual Studio 2015 (någon utgåva)

  * 2017 för Visual Studio (någon utgåva). Data Lake-verktyg för Visual Studio 2017 installeras som en del av hello Azure arbetsbelastning.

## <a name="submit-a-topology-visual-studio"></a>Skicka en topologi: Visual Studio

Hej HDInsight-verktyg kan vara används toosubmit eller hybrid C#-topologier tooyour Storm-kluster. hello följande använder ett exempelprogram. Information om hur du skapar egna topologier med hjälp av hello HDInsight Tools finns [utveckla C#-topologier med hello HDInsight Tools för Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

1. Om du inte redan har installerat hello senaste versionen av hello Data Lake-verktyg för Visual Studio finns [komma igång med Data Lake-verktyg för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

    > [!NOTE]
    > hello Data Lake-verktyg för Visual Studio kallades hello HDInsight Tools för Visual Studio.
    >
    > Data Lake-verktyg för Visual Studio ingår i hello __Azure arbetsbelastning__ för Visual Studio-2017.

2. Öppna Visual Studio, markera **filen** > **ny** > **projekt**.

3. I hello **nytt projekt** dialogrutan Expandera **installerad** > **mallar**, och välj sedan **HDInsight**. Välj hello listan över mallar **Storm exempel**. Ange ett namn för programmet hello hello längst ned i dialogrutan för hello i.

    ![Bild](./media/hdinsight-storm-deploy-monitor-topology-linux/sample.png)

4. I **Solution Explorer**, högerklicka på hello-projektet och välj **skicka tooStorm på HDInsight**.

   > [!NOTE]
   > Om du uppmanas ange hello inloggningsuppgifterna för din Azure-prenumeration. Om du har mer än en prenumeration kan du logga in toohello en som innehåller ditt Storm på HDInsight-kluster.

5. Välj ditt Storm på HDInsight-kluster från hello **Storm-kluster** listrutan och välj sedan **skicka**. Du kan övervaka om hello ansökan har genomförts med hello **utdata** fönster.

## <a name="submit-a-topology-ssh-and-hello-storm-command"></a>Skicka en topologi: SSH och hello Storm-kommando

1. Använda SSH tooconnect toohello HDInsight-kluster. Ersätt **användarnamn** hello namnet på SSH-inloggning. Ersätt **KLUSTERNAMN** med ditt HDInsight-klustrets namn:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Mer information om hur du använder SSH tooconnect tooyour HDInsight-kluster, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Använd följande kommando toostart en exempeltopologi hello:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount

    Detta kommando startar hello exempel WordCount-topologi på hello klustret. Den här topologin generera slumpmässigt meningar och antal hello förekomsten av varje ord i hello meningar.

   > [!NOTE]
   > När du skickar in topologin toohello klustret måste du först kopiera hello jar-filen som innehåller hello klustret innan du använder hello `storm` kommando. toocopy hello toohello filkluster, du kan använda hello `scp` kommando. Till exempel, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
   >
   > Hej WordCount-exemplet och andra i storm Starter ingår redan i klustret på `/usr/hdp/current/storm-client/contrib/storm-starter/`.

## <a name="submit-a-topology-programmatically"></a>Skicka en topologi: programmässigt

Du kan distribuera en topologi tooStorm på HDInsight via programmering genom att kommunicera med hello Nimbus-tjänst som finns i klustret. [https://github.com/Azure-Samples/hdinsight-Java-Deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) innehåller ett exempel på Java-program som visar hur toodeploy och starta en topologi via hello Nimbus-tjänsten.

## <a name="monitor-and-manage-visual-studio"></a>Övervaka och hantera: Visual Studio

När en topologi har skickats med Visual Studio, hello **Storm-topologier** visa för hello kluster visas. Välj hello topologi hello listan tooview information om hello kör topologi.

![Övervakare för Visual studio](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

> [!NOTE]
> Du kan också visa **Storm-topologier** från **Server Explorer** genom att expandera **Azure** > **HDInsight**, och sedan Högerklicka på ett Storm på HDInsight-kluster och välja **visa Storm-topologier**.

Markera hello form för hello spouts eller bolts tooview information om dessa komponenter. För varje vald öppnas ett nytt fönster.

### <a name="deactivate-and-reactivate"></a>Inaktivera och återaktivera

Inaktivera en topologi pausar den förrän den har avslutats eller återaktivera. tooperform dessa åtgärder använder hello __inaktivera__ och __återaktivera__ knappar hello överst i hello __Topology Summary__.

### <a name="rebalance"></a>Balansera

Ombalansering en topologi kan hello system toorevise hello parallellitet hello-topologi. Till exempel om du har ändrat storlek hello klustret tooadd anteckningar, kan ombalansering en topologi toosee hello nya noder.

toorebalance en topologi använder hello __ombalansera__ knappen hello överst i hello __Topology Summary__.

> [!WARNING]
> Ombalansering en topologi först inaktiverar hello topologi, och sedan distribuerar arbetare jämnt över hello kluster slutligen och returnerar sedan hello topologi toohello tillstånd innan ombalansering inträffade. Så om hello topologi var aktiv blir aktiva igen. Om det har inaktiverats, förblir den inaktiverade.

### <a name="kill-a-topology"></a>Avsluta en topologi

Storm-topologier fortsätta köras förrän de har stoppats eller hello klustret tas bort. toostop en topologi använder hello __Kill__ knappen hello överst i hello __Topology Summary__.

## <a name="monitor-and-manage-ssh-and-hello-storm-command"></a>Övervaka och hantera: SSH och hello Storm-kommando

Hej `storm` verktyget kan du toowork med topologier som körs från hello-kommandoraden. Använd `storm -h` för en fullständig lista över kommandon.

### <a name="list-topologies"></a>Lista över topologier

Använd följande kommando toolist hello alla topologier som körs:

    storm list

Det här kommandot returnerar informationen liknande toohello följande text:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Inaktivera och återaktivera

Inaktivera en topologi pausar den förrän den har avslutats eller återaktivera. Använd följande kommando toodeactivate hello och återaktivera:

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Avsluta en topologi som körs

Storm-topologier, startas en gång, fortsätta köras tills stoppades. toostop en topologi med hello följande kommando:

    storm kill TOPOLOGYNAME

### <a name="rebalance"></a>Balansera

Ombalansering en topologi kan hello system toorevise hello parallellitet hello-topologi. Till exempel om du har ändrat storlek hello klustret tooadd anteckningar, kan ombalansering en topologi toosee hello nya noder.

> [!WARNING]
> Ombalansering en topologi först inaktiverar hello topologi, och sedan distribuerar arbetare jämnt över hello kluster slutligen och returnerar sedan hello topologi toohello tillstånd innan ombalansering inträffade. Så om hello topologi var aktiv blir aktiva igen. Om det har inaktiverats, förblir den inaktiverade.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-storm-ui"></a>Övervaka och hantera: Storm UI

hello Storm-Användargränssnittet innehåller ett webbgränssnitt för att arbeta med topologier som körs och ingår i ditt HDInsight-kluster. tooview hello Storm-Användargränssnittet, använda en web webbläsare tooopen **https://CLUSTERNAME.azurehdinsight.net/stormui**, där **KLUSTERNAMN** är hello namnet på klustret.

> [!NOTE]
> Om du blir tillfrågad tooprovide ett användarnamn och lösenord, ange hello Klusteradministratören (admin) och lösenord som du använde när du skapar hello-klustret.

### <a name="main-page"></a>Huvudsida

hello huvudsidan för hello Storm-Användargränssnittet innehåller hello följande information:

* **Översikt över kluster**: grundläggande information om hello Storm-kluster.
* **Översikt över topologi**: en lista över topologier som körs. Använd hello länkar i det här avsnittet tooview mer information om specifika topologier.
* **Översikt över handledarens**: Information om hello Storm chef.
* **Nimbus configuration**: Nimbus-konfigurationen för hello kluster.

### <a name="topology-summary"></a>Topologi sammanfattning

Att välja en länk från hello **Topology summary** avsnitt visar följande information om hello topologi hello:

* **Översikt över topologi**: grundläggande information om hello-topologi.
* **Topologi åtgärder**: hanteringsåtgärder som du kan utföra för hello-topologi.

  * **Aktivera**: återupptar bearbetningen av en inaktiverad topologi.
  * **Inaktivera**: pausar en topologi som körs.
  * **Balansera**: justerar hello parallellitet hello-topologi. Du bör balansera om topologier som körs när du har ändrat hello antalet noder i klustret hello. Den här åtgärden tillåter hello topologi tooadjust parallellitet toocompensate för hello ökas eller minskas antalet noder i klustret hello.

    Mer information finns i <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">förstå hello parallellitet i en Storm-topologi</a>.
  * **Avsluta**: avslutar en Storm-topologi efter hello angivna timeout.
* **Topology stats**: statistik om hello-topologi. tooset hello tidsperioden för hello återstående poster på hello sidan kan du använda hello länkar i hello **fönstret** kolumn.
* **Spouts**: hello kanaler som används av hello-topologi. Använd hello länkar i det här avsnittet tooview mer information om specifika kanaler.
* **Bolts**: hello bultar som används av hello-topologi. Använd hello länkar i det här avsnittet tooview mer information om specifika bultar.
* **Topologi configuration**: hello konfigurationen av hello valt topologi.

### <a name="spout-and-bolt-summary"></a>Kanal och bult sammanfattning

Att välja en kanal hello **Spouts** eller **Bolts** avsnitt visar hello följande information om hello markerat objekt:

* **Sammanfattning**: grundläggande information om hello-kanal eller en bult.
* **Kanal/bult stats**: statistik om hello prata eller bultar. tooset hello tidsperioden för hello återstående poster på hello sidan kan du använda hello länkar i hello **fönstret** kolumn.
* **Input stats** (endast bultar): Information om hello indataström som används av hello bulten.
* **Utdata stats**: Information om hello-dataströmmar som orsakat av detta kanal eller bultar.
* **Executors**: Information om hello instanser av hello-kanal eller en bult. Välj hello **Port** post för en specifik utförare tooview genereras en logg över diagnostisk information för den här instansen.
* **Fel**: felinformation för den här kanalen eller bultar.

## <a name="monitor-and-manage-rest-api"></a>Övervaka och hantera: REST API

hello Storm-Användargränssnittet är byggt på hello REST-API, så att du kan göra liknande hantering och övervakning av funktioner med hjälp av hello REST API. Du kan använda hello REST API toocreate anpassade verktyg för att hantera och övervaka Storm-topologier.

Mer information finns i [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). hello är följande uppgifter specifika toousing hello REST-API med Apache Storm på HDInsight.

> [!IMPORTANT]
> hello Storm REST API är inte offentligt tillgänglig över hello internet och måste användas med en SSH-tunnel toohello HDInsight-klustrets huvudnod. Mer information om hur du skapar och använder en SSH-tunnel finns [använda SSH-tunnlar tooaccess Ambari web UI, resurshanteraren, jobbhistorik, NameNode, Oozie och andra webb-användargränssnitt](hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Bas-URI

hello bas-URI för hello REST API på Linux-baserade HDInsight-kluster finns i hello huvudnod vid **v1-https://HEADNODEFQDN:8744/api/**. hello domännamnet för hello huvudnod genereras när klustret skapas och är inte statisk.

Du kan hitta hello fullständigt kvalificerade domännamnet (FQDN) för hello klustrets huvudnod på flera olika sätt:

* **Från en SSH-session**: kommandot hello `headnode -f` från ett kluster med SSH-session toohello.
* **Ambari Web**: Välj **Services** från hello överst på hello-sidan, Välj **Storm**. Från hello **sammanfattning** väljer **Storm UI Server**. hello FQDN för hello-nod som kör hello Storm-Användargränssnittet och REST API är hello överst på hello sidan.
* **Från Ambari REST API**: kommandot hello `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` tooretrieve information om hello-nod som hello Storm-Användargränssnittet och REST-API som körs på. Ersätt **lösenord** med hello administratörslösenord för hello-kluster. Ersätt **KLUSTERNAMN** med hello klusternamnet. Hello svar innehåller hello ”värddatornamn” post hello FQDN för hello-nod.

### <a name="authentication"></a>Autentisering

Begäranden toohello REST API måste använda **grundläggande autentisering**, så du använda hello HDInsight-kluster administratörens namn och lösenord.

> [!NOTE]
> Eftersom grundläggande autentisering skickas i klartext, bör du **alltid** använder toosecure HTTPS-kommunikation med hello-kluster.

### <a name="return-values"></a>Returnera värden

Information som returneras från hello REST-API kan endast användas från inom hello kluster eller virtuella datorer på hello samma virtuella Azure-nätverk som hello kluster. Hello fullständigt kvalificerade domännamnet (FQDN) returneras för Zookeeper-servrar är till exempel inte att komma åt från hello Internet.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur toodeploy och övervaka topologier med hjälp av hello Storm-instrumentpanelen, lär du dig hur för[utveckla Java-baserade topologier med Maven](hdinsight-storm-develop-java-topology.md).

En lista över flera exempeltopologier finns [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md).
