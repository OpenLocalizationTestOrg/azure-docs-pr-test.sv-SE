---
title: "aaaAccess Hadoop YARN-program programmässigt - loggar Azure | Microsoft Docs"
description: "Programmet Access loggar programmässigt på ett Hadoop-kluster i HDInsight."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0198d6c9-7767-4682-bd34-42838cf48fc5
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 064efee1ea6a864c29ab897692ead0152c926c0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Åtkomst programloggar YARN på Windows-baserade HDInsight
Det här avsnittet beskrivs hur tooaccess hello loggar för YARN (ännu en annan resurs förhandlare)-program som har gått ut på en Windows-baserade Hadoop-kluster i Azure HDInsight

> [!IMPORTANT]
> hello informationen i det här dokumentet gäller endast tooWindows-baserade HDInsight-kluster. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). För information om hur du använder YARN loggar på Linux-baserade HDInsight-kluster, se [åtkomst YARN programloggar på Linux-baserade Hadoop i HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)
>


### <a name="prerequisites"></a>Krav
* Ett Windows-baserade HDInsight-kluster.  Se [skapa Windows-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="yarn-timeline-server"></a>YARN tidslinjen Server
Hej <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN tidslinjen Server</a> innehåller allmän information om slutförda program samt som framework-specifik information om programmet via två olika gränssnitt. Närmare bestämt:

* Lagring och hämtning av information för Allmänt program på HDInsight-kluster har aktiverat med version 3.1.1.374 eller högre.
* hello framework-specifik information programkomponent av hello tidslinjen Server är inte tillgänglig på HDInsight-kluster.

Allmän information om program innehåller hello följande typer av data:

* hello program-ID, en unik identifierare för ett program
* hello-användare som startade hello program
* Information om försök görs toocomplete hello program
* hello-behållare som används av ett visst program försök

Den här informationen kommer att lagras av Azure Resource Manager tooa historik store i hello standardbehållaren för ditt Azure Storage-standardkonto på HDInsight-kluster. Allmänna data på slutförda program kan hämtas via ett REST-API:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>YARN program och loggfiler
YARN stöder flera programmeringsmodeller (MapReduce är en av dem) genom Frikoppling resurshantering från schemaläggning/programövervakning. Detta görs via en global *ResourceManager* (RM) per arbetsnoden *NodeManagers* (NMs), och programspecifika *ApplicationMasters* (AMs). hello förhandlar programspecifika AM resurser (processor, minne, disk, nätverk) för att köra programmet med hello RM. hello RM fungerar med NMs toogrant dessa resurser, som beviljas som *behållare*. hello AM ansvarar för att spåra hello fortskrider hello behållare som tilldelats tooit av hello RM. Ett program kan kräva många behållare beroende på hello uppbyggnad hello program.

Dessutom kan varje program kan bestå av flera *program försöker* i ordning toofinish i hello förekomst av krascher eller på grund av toohello förlust av kommunikation mellan en AM och en RM. Därför måste beviljas behållare tooa specifika försök av ett program. En behållare ger hello kontext för grundläggande enheten för arbete som utförs av en YARN-program i en mening och allt arbete som utförs inom hello kontext för en behållare utförs på hello enskild worker nod på vilka hello behållaren har tilldelats. Se [YARN begrepp] [ YARN-concepts] ytterligare referens.

Programmet loggar (och associerade hello behållaren loggar) är kritiska felsöka problematiska Hadoop-program. YARN tillhandahåller ett bra ramverk för att samla in, sammanställa och lagra programloggarna med hello [loggen aggregering] [ log-aggregation] funktion. hello loggen aggregering funktionen gör att nå programloggarna mer deterministisk aggregerar loggar över alla behållare på en arbetsnod och lagrar dem som en sammansatt loggfil per arbetsnoden på hello standardfilsystem när ett program har slutförts. Programmet kan använda hundratals eller tusentals behållare, men loggar för alla behållare som körs på en enda arbetsnod kommer alltid att aggregerade tooa en fil, vilket resulterar i en loggfil per arbetsnoden som används av ditt program. Aggregering av loggen är aktiverad som standard i HDInsight-kluster (version 3.0 och senare), och aggregerade loggar finns i hello standardbehållaren på klustret på hello följande plats:

    wasb:///app-logs/<user>/logs/<applicationId>

På den platsen kan *användare* är hello användare som startade hello programmet hello namn och *applicationId* är hello Unik identifierare för ett program som tilldelats av hello YARN RM.

hello aggregerade loggar är inte direkt läsbar, som de är skrivna en [TFile][T-file], [binärformat] [ binary-format] indexeras av behållare. YARN tillhandahåller CLI verktyg toodump loggarna som oformaterad text för program eller behållare av intresse. Du kan visa dessa loggar som oformaterad text genom att köra ett hello följande YARN kommandon direkt på hello klusternoder (när du ansluter tooit via RDP):

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager-Användargränssnittet
Hej YARN ResourceManager-Användargränssnittet körs på hello klustret headnode och kan nås via hello Azure portalens instrumentpanel:

1. Logga in för[Azure-portalen](https://portal.azure.com/).
2. På hello vänstra menyn **Bläddra**, klickar du på **HDInsight-kluster**, klickar du på ett Windows-kluster som du vill tooaccess hello YARN programloggar.
3. På hello översta menyn **instrumentpanelen**. Du ser en sida öppnas på en ny webbläsare flik med namnet **HDInsight frågan konsolen**.
4. Från **HDInsight frågan konsolen**, klickar du på **Yarn-Användargränssnittet**.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
