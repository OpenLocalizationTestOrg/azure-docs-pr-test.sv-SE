---
title: "aaaAccess Hadoop YARN program loggar på Linux-baserade HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur tooaccess YARN program loggar på en Linux-baserat HDInsight (Hadoop)-kluster med hjälp av både hello kommandoradsverktyg och en webbläsare."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 3ec08d20-4f19-4a8e-ac86-639c04d2f12e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 0bab356e3b97114abbb05712c8e7b21a194f2508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-yarn-application-logs-on-linux-based-hdinsight"></a>Åtkomst programloggar YARN på Linux-baserat HDInsight

Lär dig hur tooaccess hello loggar för YARN (ännu en annan resurs förhandlare) program på ett Hadoop-kluster i Azure HDInsight.

> [!IMPORTANT]
> hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight component-versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="YARNTimelineServer"></a>YARN tidslinjen Server

Hej [YARN tidslinjen Server](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) ger allmän information om slutförda program och programinformation om framework-specifika via två olika gränssnitt. Närmare bestämt:

* Lagring och hämtning av information för Allmänt program på HDInsight-kluster har aktiverat med version 3.1.1.374 eller högre.
* hello framework-specifik information programkomponent av hello tidslinjen Server är inte tillgänglig på HDInsight-kluster.

Allmän information om program innehåller hello efter typ av data:

* hello program-ID, en unik identifierare för ett program
* hello-användare som startade hello program
* Information om försök görs toocomplete hello program
* hello-behållare som används av ett visst program försök

## <a name="YARNAppsAndLogs"></a>YARN program och loggfiler

YARN stöder flera programmeringsmodeller (MapReduce är en av dem) genom Frikoppling resurshantering från schemaläggning/programövervakning. YARN använder en global *ResourceManager* (RM) per arbetsnoden *NodeManagers* (NMs), och programspecifika *ApplicationMasters* (AMs). hello förhandlar programspecifika AM resurser (processor, minne, disk, nätverk) för att köra programmet med hello RM. hello RM fungerar med NMs toogrant dessa resurser, som beviljas som *behållare*. hello AM ansvarar för att spåra hello fortskrider hello behållare som tilldelats tooit av hello RM. Ett program kan kräva många behållare beroende på hello uppbyggnad hello program.

Varje program kan bestå av flera *program försöker*. Om ett program inte kan du göra det som ett nytt försök. Varje försök körs i en behållare. I en mening ger en behållare hello kontext för grundläggande enheten för arbete som utförs av en YARN-programmet. Allt arbete som utförs inom hello kontext för en behållare har utförts på hello enskild worker nod på vilka hello behållaren har tilldelats. Se [YARN begrepp] [ YARN-concepts] ytterligare referens.

Programmet loggar (och associerade hello behållaren loggar) är kritiska felsöka problematiska Hadoop-program. YARN tillhandahåller ett bra ramverk för att samla in, sammanställa och lagra programloggarna med hello [loggen aggregering] [ log-aggregation] funktion. hello loggen aggregering funktionen gör att nå programloggarna mer deterministisk. Den sammanställer loggar över alla behållare på en arbetsnod och lagrar dem som en sammansatt loggfil per arbetsnoden. hello loggen är lagrade på hello standardfilsystem när ett program har slutförts. Programmet kan använda hundratals eller tusentals behållare, men loggar för alla behållare som körs på en enda arbetsnod är en fil alltid sammansatt tooa. Det är därför bara 1 loggen per arbetsnoden som används av ditt program. Aggregering av loggen är aktiverad som standard på HDInsight-kluster av version 3.0 och senare. Sammanställda loggarna finns i standard lagringen för hello klustret. hello följande sökväg är hello HDFS sökväg toohello loggar:

    /app-logs/<user>/logs/<applicationId>

Hello sökvägen `user` är hello användare som startade hello programmet hello namn. Hej `applicationId` är hello Unik identifierare som tilldelas tooan program av hello YARN RM.

hello aggregerade loggar är inte direkt läsbar, som de är skrivna en [TFile][T-file], [binärformat] [ binary-format] indexeras av behållare. Använda hello YARN ResourceManager loggar eller CLI verktyg tooview loggarna som oformaterad text för program eller behållare av intresse.

## <a name="yarn-cli-tools"></a>YARN CLI-verktygen

toouse hello YARN CLI-verktygen, måste du först ansluta toohello HDInsight-kluster med SSH. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

Du kan visa dessa loggar som oformaterad text genom att köra ett hello följande kommandon:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

Ange hello &lt;applicationId >, &lt;användaren som-igång-program >, &lt;behållar-ID >, och &lt;worker-nod-adress > information när du kör kommandona.

## <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager-Användargränssnittet

Hej YARN ResourceManager-Användargränssnittet körs på hello klustret headnode. Den kan nås via hello Ambari-webbgränssnittet. Använd hello följande steg tooview hello YARN-loggar:

1. Navigera toohttps://CLUSTERNAME.azurehdinsight.net i din webbläsare. Ersätt KLUSTERNAMN med hello namnet på ditt HDInsight-kluster.
2. Välj hello listan över tjänster hello vänster **YARN**.

    ![Yarn-tjänsten som markerats](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. Från hello **snabblänkar** listrutan väljer du något av hello-klustrets huvudnoder och välj sedan **ResourceManager loggen**.

    ![Yarn snabblänkar](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)

    Visas en lista med länkar tooYARN loggar.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
