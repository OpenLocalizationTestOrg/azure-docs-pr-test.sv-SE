---
title: "aaaProcess händelser från Event Hubs med Storm - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur tooprocess data från Azure Event Hubs med en C# Storm-topologi skapar i Visual Studio med hello HDInsight tools för Visual Studio."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 67f9d08c-eea0-401b-952b-db765655dad0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: 30cd910d80eba066f283197bcbbaf11145bc5524
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a>Bearbeta händelser från Azure Event Hubs med Storm på HDInsight (C#)

Lär dig hur toowork med Händelsehubbar från Apache Storm på HDInsight. Det här dokumentet använder en C# Storm-topologi tooread och skriva data från Evbent hubbar

> [!NOTE]
> En Java-version av det här projektet finns [bearbeta händelser från Azure Event Hubs med Storm på HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).

## <a name="scpnet"></a>SCP.NET

hello stegen i det här dokumentet använder SCP.NET NuGet-paketet som gör det enkelt toocreate C#-topologier och komponenter för användning med Storm på HDInsight.

> [!IMPORTANT]
> Medan hello stegen i det här dokumentet förlitar sig på en Windows-utvecklingsmiljö med Visual Studio, hello kompilerade projekt kan vara skickade tooa Storm på HDInsight-kluster som använder Linux. Linux-baserade kluster som skapas efter den 28 oktober 2016 stöder endast SCP.NET topologier.

HDInsight 3.4 och större användning monoljud toorun C#-topologier. hello-exempel som används i det här dokumentet fungerar med HDInsight 3,6. Om du planerar att skapa egna .NET-lösningar för HDInsight, kontrollera hello [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/) dokument för potentiella inkompatibiliteter.

### <a name="cluster-versioning"></a>Klustret versionshantering

Hej Microsoft.SCP.Net.SDK NuGet-paketet som du använder för ditt projekt måste matcha hello huvudversion av Storm installerad på HDInsight. HDInsight-version 3.5 och 3,6 använda Storm 1.x, så du måste använda SCP.NET version 1.0.x.x med dessa kluster.

> [!IMPORTANT]
> hello exemplet i det här dokumentet förväntar sig ett HDInsight-3.5 eller 3,6 klustret.
>
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

C#-topologier måste också ha .NET 4.5.

## <a name="how-toowork-with-event-hubs"></a>Hur toowork med Händelsehubbar

Microsoft tillhandahåller en uppsättning Java-komponenter som kan använda toocommunicate med Händelsehubbar från en Storm-topologi. Du kan hitta hello Java Arkiv (JAR)-fil som innehåller ett HDInsight-3,6 kompatibel version av dessa komponenter på [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

> [!IMPORTANT]
> Medan hello komponenter är skriven i Java, kan du enkelt använda dem från en C#-topologi.

i det här exemplet används hello följande komponenter:

* __EventHubSpout__: läser data från Händelsehubbar.
* __EventHubBolt__: skriver data tooEvent Hubs.
* __EventHubSpoutConfig__: tooconfigure EventHubSpout används.
* __EventHubBoltConfig__: tooconfigure EventHubBolt används.

### <a name="example-spout-usage"></a>Exempel på användning kanal

SCP.NET ger metoder för att lägga till en EventHubSpout tooyour topologi. Metoderna gör det enklare tooadd en kanal än att använda hello generiska metoder för att lägga till en Java-komponent. hello exemplet nedan visar hur toocreate en kanal med hjälp av hello __SetEventHubSpout__ och **EventHubSpoutConfig** metoder som tillhandahålls av SCP.NET:

```csharp
 topologyBuilder.SetEventHubSpout(
    "EventHubSpout",
    new EventHubSpoutConfig(
        ConfigurationManager.AppSettings["EventHubSharedAccessKeyName"],
        ConfigurationManager.AppSettings["EventHubSharedAccessKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        ConfigurationManager.AppSettings["EventHubEntityPath"],
        eventHubPartitions),
    eventHubPartitions);
```

hello det föregående exemplet skapas en ny kanal komponent med namnet __EventHubSpout__, och konfigurerar toocommunicate med en händelsehubb. hello parallellitet tips för hello komponent anges toohello antalet partitioner i hello händelsehubb. Den här inställningen kan Storm toocreate en instans av hello-komponenten för varje partition.

### <a name="example-bolt-usage"></a>Exempel på bult användning

Använd hello **JavaComponmentConstructor** metoden toocreate en instans av hello bulten. hello exemplet nedan visar hur toocreate och konfigurera en ny instans av hello **EventHubBolt**:

```csharp
// Java construcvtor for hello Event Hub Bolt
JavaComponentConstructor constructor = JavaComponentConstructor.CreateFromClojureExpr(
    String.Format(@"(org.apache.storm.eventhubs.bolt.EventHubBolt. (org.apache.storm.eventhubs.bolt.EventHubBoltConfig. " +
        @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
        ConfigurationManager.AppSettings["EventHubPolicyName"],
        ConfigurationManager.AppSettings["EventHubPolicyKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        "servicebus.windows.net",
        ConfigurationManager.AppSettings["EventHubName"],
        "true"));

// Set hello bolt toosubscribe toodata from hello spout
topologyBuilder.SetJavaBolt(
    "eventhubbolt",
    constructor,
    partitionCount)
        .shuffleGrouping("Spout");
```

> [!NOTE]
> Det här exemplet används en Clojure uttryck som skickas som en sträng, i stället för **JavaComponentConstructor** toocreate en **EventHubBoltConfig**, som gjorde hello kanal exempel. Antingen metoden fungerar. Använd hello-metod som känns bästa tooyou.

## <a name="download-hello-completed-project"></a>Ladda ned hello slutförts projekt

Du kan hämta en fullständig version av hello projektet har skapats i den här kursen från [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub). Behöver du dock fortfarande tooprovide konfigurationsinställningarna genom att följa hello stegen i den här självstudiekursen.

### <a name="prerequisites"></a>Krav

* En [Apache Storm på HDInsight-kluster av version 3.5 eller 3,6](hdinsight-apache-storm-tutorial-get-started.md).

    > [!WARNING]
    > hello-exempel som används i det här dokumentet kräver Storm på HDInsight version 3.5 eller 3,6. Detta fungerar inte med äldre versioner av HDInsight, på grund av toobreaking klassen namn ändras. En version av det här exemplet fungerar med äldre kluster, se [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).

* En [Azure händelsehubb](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

* Hej [Azure .NET SDK](http://azure.microsoft.com/downloads/).

* Hej [HDInsight tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

* Java JDK 1.8 eller senare på din utvecklingsmiljö. JDK hämtas från [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

  * Hej **JAVA_HOME** miljö variabeln måste punkt toohello katalog som innehåller Java.
  * Hej **%JAVA_HOME%/bin** katalogen måste vara i hello sökväg.

## <a name="download-hello-event-hubs-components"></a>Hämta hello Händelsehubbar komponenter

Hämta hello Händelsehubbar prata och bultar komponenten från [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

Skapa en katalog med namnet `eventhubspout`, och spara hello-filen till hello-katalogen.

## <a name="configure-event-hubs"></a>Konfigurera Händelsehubbar

Händelsehubbar är hello datakälla för det här exemplet. Använd hello information i avsnittet ”Skapa en händelsehubb” Hej för i [Kom igång med Händelsehubbar](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

1. När hello händelsehubb har skapats kan du visa hello **EventHub** bladet i hello Azure portal och välj **principer för delad åtkomst**. Välj **+ Lägg till** tooadd hello följande principer:

   | Namn | Behörigheter |
   | --- | --- |
   | Skrivare |Skicka |
   | läsare |Lyssna |

    ![Skärmbild av dela access principer fönster](./media/hdinsight-storm-develop-csharp-event-hub-topology/sas.png)

2. Välj hello **reader** och **writer** principer. Kopiera och spara hello primärnyckelvärde för båda principerna enligt dessa värden används senare.

## <a name="configure-hello-eventhubwriter"></a>Konfigurera hello EventHubWriter

1. Om du inte redan har installerat hello senaste versionen av hello HDInsight tools för Visual Studio finns [komma igång med HDInsight tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Hämta hello lösningen från [eventhub-storm-hybrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

3. I hello **EventHubWriter** projekt, öppna hello **App.config** fil. Använd hello information från hello händelsehubb att du konfigurerat tidigare toofill i hello-värdet för hello följande nycklar:

   | Nyckel | Värde |
   | --- | --- |
   | EventHubPolicyName |skrivare (om du har använt ett annat namn för hello-princip med *skicka* behörighet, Använd i stället.) |
   | EventHubPolicyKey |hello nyckel för hello writer princip. |
   | EventHubNamespace |hello-namnområde som innehåller din event hub. |
   | EventHubName |Din händelsehubbens namn. |
   | EventHubPartitionCount |hello antalet partitioner i din event hub. |

4. Spara och Stäng hello **App.config** fil.

## <a name="configure-hello-eventhubreader"></a>Konfigurera hello EventHubReader

1. Öppna hello **EventHubReader** projekt.

2. Öppna hello **App.config** -filen för hello **EventHubReader**. Använd hello information från hello händelsehubb att du konfigurerat tidigare toofill i hello-värdet för hello följande nycklar:

   | Nyckel | Värde |
   | --- | --- |
   | EventHubPolicyName |läsare (om du har använt ett annat namn för hello-princip med *lyssna* behörighet, Använd i stället.) |
   | EventHubPolicyKey |hello nyckel för hello reader princip. |
   | EventHubNamespace |hello-namnområde som innehåller din event hub. |
   | EventHubName |Din händelsehubbens namn. |
   | EventHubPartitionCount |hello antalet partitioner i din event hub. |

3. Spara och Stäng hello **App.config** fil.

## <a name="deploy-hello-topologies"></a>Distribuera hello topologier

1. Från **Solution Explorer**, högerklicka på hello **EventHubReader** projektet och välj **skicka tooStorm på HDInsight**.

    ![Skärmbild av Solution Explorer med skicka tooStorm på HDInsight markerat](./media/hdinsight-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. På hello **skicka topologi** väljer din **Storm-kluster**. Expandera **ytterligare konfigurationer**väljer **Java sökvägar**väljer **...** , och välj hello-katalog som innehåller hello JAR-filen som du hämtade tidigare. Klicka slutligen på **skicka**.

    ![Dialogrutan Skicka topologi skärmbild](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)

3. När hello-topologi har skickats, hello **Storm-topologier Viewer** visas. tooview information om hello topologi, Välj hello **EventHubReader** topologi i hello till vänster.

    ![Skärmbild av Storm-topologier Viewer](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. Från **Solution Explorer**, högerklicka på hello **EventHubWriter** projektet och välj **skicka tooStorm på HDInsight**.

5. På hello **skicka topologi** väljer din **Storm-kluster**. Expandera **ytterligare konfigurationer**väljer **Java sökvägar**väljer **...** , och välj hello-katalog som innehåller hello JAR-filen du hämtade tidigare. Klicka slutligen på **skicka**.

6. När hello-topologi har skickats, uppdatera hello topologi listan i hello **Storm-topologier Viewer** tooverify båda topologierna körs på hello klustret.

7. I **Storm-topologier Viewer**väljer hello **EventHubReader** topologi.

8. tooopen hello komponenten sammanfattning för hello bult dubbelklicka hello **LogBolt** komponenten i hello diagram.

9. I hello **Executors** väljer du någon av hello länkar i hello **Port** kolumn. Visar information som loggas av hello-komponenten. hello loggas information är liknande toohello följande text:

        2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
        2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
        2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}

## <a name="stop-hello-topologies"></a>Stoppa hello topologier

toostop hello topologier väljer varje topologi i hello **Storm-topologi Viewer**, klicka på **Kill**.

![Skärmbild av Storm-topologi Viewer, med Kill-knappen markerad](./media/hdinsight-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a>Ta bort klustret

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nästa steg

I det här dokumentet har du lärt dig hur toouse hello Java Händelsehubbar prata och bultar från en C#-topologi toowork med data i Händelsehubbar i Azure. toolearn mer om hur du skapar C#-topologier, finns följande hello:

* [Utveckla C#-topologier för Apache Storm på HDInsight med Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [Programmeringsguide för SCP](hdinsight-storm-scp-programming-guide.md)
* [Exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md)
