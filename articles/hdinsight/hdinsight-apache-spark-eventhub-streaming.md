---
title: "aaaUse Apache Spark streaming med Händelsehubbar i Azure HDInsight | Microsoft Docs"
description: "Skapa ett Apache Spark streaming exempel på hur toosend data strömma tooAzure Event Hub och ta emot dessa händelser i HDInsight Spark-kluster med ett scala-program."
keywords: Apache spark streaming, spark streaming, spark-exemplet, apache spark streaming exempel event hub azure exempel, spark-exempel
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 68894e75-3ffa-47bd-8982-96cdad38b7d0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: 10cc5884047b3b8249fe8a8822a16a19780a4af3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-streaming-process-data-from-azure-event-hubs-with-spark-cluster-on-hdinsight"></a>Apache Spark streaming: bearbetning av data från Azure Event Hubs med Spark-kluster i HDInsight

I den här artikeln kan du skapa ett Apache Spark streaming prov som inbegriper hello följande steg:

1. Du kan använda ett fristående program tooingest meddelanden i en Azure-Händelsehubb.

2. Med två olika sätt hämta hälsningsmeddelande från Event Hub i realtid med hjälp av ett program som körs i Spark-kluster på Azure HDInsight.

3. Du skapar strömmande analytiska pipelines toopersist data toodifferent lagringssystem eller kunskap genom statistik från data på hello direkt.

## <a name="prerequisites"></a>Krav

* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="spark-streaming-concepts"></a>Spark Streaming begrepp

En detaljerad förklaring av Spark streaming finns [Apache Spark streaming översikt](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). HDInsight ger hello samma strömmande funktioner tooa Spark-kluster i Azure.  

## <a name="what-does-this-solution-do"></a>Vad är den här lösningen?

Utför hello följa stegen i den här artikeln toocreate en Spark streaming exempelvis:

1. Skapa en Azure-Händelsehubb som tar emot en dataström med händelser.

2. Kör ett lokala fristående program som genererar händelser och skickar den toohello Azure Event Hub. hello exempelprogram som gör detta publiceras på [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).

3. Kör en strömmande via fjärranslutning på ett Spark-kluster som läser strömningen av händelser från Azure Event Hub och utföra olika databearbetning/analys.

## <a name="create-an-azure-event-hub"></a>Skapa ett Azure Event Hub

1. Logga in toohello [Azure Portal](https://ms.portal.azure.com), och klicka på **ny** på hello upp till vänster i hello-skärmen.

2. Klicka på **Sakernas Internet** och sedan på **Event Hubs**.

    ![Skapa händelsehubb för Spark streaming exempel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "skapa händelsehubb för Spark streaming exempel")

3. I hello **skapa namnområdet** bladet, ange ett namn för namnområdet. Välj hello prisnivån (Basic eller Standard). Också välja en Azure-prenumeration, resursgrupp och plats i vilken toocreate hello-resurs. Klicka på **skapa** toocreate hello namnområde.

      ![Ange ett händelsenamn hubb Spark streaming exempel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "ger en händelsehubbens namn Spark streaming exempel")

    > [!NOTE]
    > Du ska markera hello samma **plats** som Apache Spark-kluster i HDInsight tooreduce svarstid och kostnader.
    >
    >

4. Klicka på hello nyskapade namnområde i hello Händelsehubbar namnområde.      


5. I hello namnområde bladet, klickar du på **Händelsehubbar**, och klicka sedan på **+ Event Hub** toocreate en ny Händelsehubb.
   
    ![Skapa händelsehubb för Spark streaming exempel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "skapa händelsehubb för Spark streaming exempel")

6. Ange ett namn för din Event Hub, ange hello partition antal too10 och meddelandet kvarhållning too1. Vi inte arkiverar hälsningsmeddelande i den här lösningen så att du kan lämna hello rest som standard och klicka sedan på **skapa**.
   
    ![Ange händelseinformationen hubb för Spark streaming exempel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "ange händelseinformationen hubb för Spark streaming exempel")

7. hello nyskapad Event Hub listas i hello Event Hub-blad.
    
     ![Visa Event Hub hello Spark streaming exempel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "View Event Hub för hello Väck strömmande exempel")

8. Klicka på tillbaka i hello namnområde bladet (inte hello specifik Händelsehubb bladet) **principer för delad åtkomst**, och klicka sedan på **RootManageSharedAccessKey**.
    
     ![Ange Event Hub-principer t.ex hello Spark streaming](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "ange Event Hub principer för hello Väck strömmande exempel")

9. Klicka på hello Kopiera knappen toocopy hello **RootManageSharedAccessKey** primära nyckel och anslutningen sträng toohello Urklipp. Spara dessa toouse senare i självstudiekursen hello.
    
     ![Visa Event Hub princip nycklar hello Spark streaming exempel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "View Event Hub princip nycklar för hello Väck strömmande exempel")

## <a name="send-messages-tooazure-event-hub-using-a-sample-scala-application"></a>Skicka meddelanden tooAzure Händelsehubb med hjälp av ett exempelprogram Scala

I det här avsnittet använder du en fristående-lokala Scala-program som genererar en dataström med händelser och skickar den tooAzure Händelsehubb som du skapade tidigare. Det här programmet är tillgängligt på GitHub på [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer). hello stegen här förutsätter att du redan har forked GitHub-lagringsplatsen.

1. Kontrollera att du har hello följande installerat på hello datorn där du kör det här programmet.

    * Oracle Java Development kit. Du kan installera den från [här](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
    * Apache Maven. Du kan ladda ned det från [här](https://maven.apache.org/download.cgi). Instruktioner tooinstall Maven finns [här](https://maven.apache.org/install.html).

2. Öppna en kommandotolk och navigera toohello plats som du har klonat hello GitHub-repo hello Scala exempelprogram och kör följande kommando toobuild hello programmet hello.

        mvn package

3. hello utdata jar för programmet hello **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, skapas under **/target** directory. Du kan använda den här JAR senare i den här artikeln tootest hello komplett lösning.

## <a name="create-application-tooreceive-messages-from-event-hub-into-a-spark-cluster"></a>Skapa program tooreceive meddelanden från Event Hub i ett Spark-kluster 

Vi har två metoder tooconnect Spark Streaming och Azure Event Hubs, mottagar-baserade anslutningen och Direct-DStream-baserad anslutning. Direct-DStream-baserade introduceras på januari 2017 i hello 2.0.3 versionen. Den bör tooreplace hello ursprungliga mottagar-baserade anslutningen eftersom det är mer performant och resurseffektiv. Mer information finns i [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs). Direkt DStream stöder endast Spark 2.0 +.

### <a name="build-applications-with-hello-dependency-toospark-eventhubs-connector"></a>Skapa program med hello beroende toospark eventhubs connector

Vi kommer även att publicera hello mellanlagring version av Spark-EventHubs i GitHub. toouse hello fristående versionen av Spark EventHubs, hello första steget är tooindicate GitHub som hello källa lagringsplatsen genom att lägga till följande post toopom.xml hello:

```xml
<repository>
      <id>spark-eventhubs</id>
      <url>https://raw.github.com/hdinsight/spark-eventhubs/maven-repo/</url>
      <snapshots>
        <enabled>true</enabled>
        <updatePolicy>always</updatePolicy>
      </snapshots>
</repository>
```

Du kan sedan lägga till hello följande beroende tooyour projekt tootake hello före utgivna versionen.

Maven beroende

```xml
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11 -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-streaming-eventhubs_2.11</artifactId>
    <version>2.0.4</version>
</dependency>
```

Segregerade Barlasttankar beroende

```
// https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11
libraryDependencies += "com.microsoft.azure" % "spark-streaming-eventhubs_2.11" % "2.0.4"
```

### <a name="direct-dstream-connection"></a>Direkt DStream anslutning

En förskapad jar-fil som innehåller exempel som använder direkt DStream kan hämtas i [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).

hello jar-filen innehåller tre exempel vars källkoden finns på [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).

Tar [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) som exempel:

```scala
private def createStreamingContext(
  sparkCheckpointDir: String,
  batchDuration: Int,
  namespace: String,
  eventHunName: String,
  eventhubParams: Map[String, String],
  progressDir: String) = {
val ssc = new StreamingContext(new SparkContext(), Seconds(batchDuration))
ssc.checkpoint(sparkCheckpointDir)
val inputDirectStream = EventHubsUtils.createDirectStreams(
  ssc,
  namespace,
  progressDir,
  Map(eventHunName -> eventhubParams))

inputDirectStream.map(receivedRecord => (new String(receivedRecord.getBody), 1)).
  reduceByKeyAndWindow((v1, v2) => v1 + v2, (v1, v2) => v1 - v2, Seconds(batchDuration * 3),
    Seconds(batchDuration)).print()

ssc
}

def main(args: Array[String]): Unit = {

if (args.length != 8) {
  println("Usage: program progressDir PolicyName PolicyKey EventHubNamespace EventHubName" +
    " BatchDuration(seconds) Spark_Checkpoint_Directory maxRate")
  sys.exit(1)
}

val progressDir = args(0)
val policyName = args(1)
val policykey = args(2)
val namespace = args(3)
val name = args(4)
val batchDuration = args(5).toInt
val sparkCheckpointDir = args(6)
val maxRate = args(7)

val eventhubParameters = Map[String, String] (
  "eventhubs.policyname" -> policyName,
  "eventhubs.policykey" -> policykey,
  "eventhubs.namespace" -> namespace,
  "eventhubs.name" -> name,
  "eventhubs.partition.count" -> "32",
  "eventhubs.consumergroup" -> "$Default",
  "eventhubs.maxRate" -> s"$maxRate"
)

val ssc = StreamingContext.getOrCreate(sparkCheckpointDir,
  () => createStreamingContext(sparkCheckpointDir, batchDuration, namespace, name,
    eventhubParameters, progressDir))

ssc.start()
ssc.awaitTermination()
}
```

I hello ovan exempelvis `eventhubParameters` hello parametrar specifika tooa EventHubs instans och du har toopass den toohello `createDirectStreams` API som skapar ett direkt DStream objektet mappning tooa Händelsehubbar namnområde. Du kan anropa DStream API: er som tillhandahålls av Spark Streaming API framework över hello direkt DStream objekt. I det här exemplet beräkna vi hello frekvensen för varje ord i hello sista 3 micro batch intervall.

### <a name="receiver-based-connection"></a>Mottagaren-baserade anslutningen

En Spark streaming exempelprogram som skrivits i Scala som tar emot händelser och väg hello toodifferent mål, finns på [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples). Gör hello under tooupdate hello programmet för din Event Hub-konfiguration och skapa hello utdata jar.

1. Starta IntelliJ IDEA och från hello Starta skärmen Välj **checka ut från versionskontroll** och klicka sedan på **Git**.
   
    ![Apache Spark streaming exempel – get källor från Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streaming exempel – get källor från Git")

2. I hello **klona databasen** dialogrutan Ange hello URL toohello Git-lagringsplatsen tooclone från ange hello directory tooclone till och klicka sedan på **klona**.
   
    ![Apache Spark streaming exempel – klona från Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streaming exempel – klona från Git")
3. Följ hello prompter till hello projekt helt klonas. Tryck på **Alt + 1** tooopen hello **projektvyn**. Det bör likna följande hello.
   
    ![Apache Spark streaming exempel – projektvyn](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streaming exempel – projektvyn")
4. Kontrollera att hello programkod kompileras med Java8. tooensure, genom att välja **filen**, klickar du på **projektstruktur**, och på hello **projekt** kontrollerar du att projektet språket är inställd för**8 - Lambdas, typ anteckningar, etc.**.
   
    ![Apache Spark streaming exempel - Set-kompilatorn](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streaming exempel - Set-kompilatorn")
5. Öppna hello **pom.xml** och kontrollera hello Spark-versionen är korrekt. Under `<properties>` , leta efter hello följande fragment och verifiera hello Spark-version.

        <scala.version>2.11.8</scala.version>
        <scala.compat.version>2.11.8</scala.compat.version>
        <scala.binary.version>2.11</scala.binary.version>
        <spark.version>2.0.0</spark.version>

6. hello-programmet kräver en beroende jar kallas **JDBC driver jar**. Detta är obligatorisk toowrite hälsningsmeddelande togs emot från Event Hub till en Azure SQL-databas. Du kan hämta den här jar (v4.1 eller senare) från [här](https://msdn.microsoft.com/sqlserver/aa937724.aspx). Lägg till referens toothis jar i hello projekt bibliotek. Utför följande steg hello:
     
     1. IntelliJ IDEA fönster där du har hello-programmet öppnas, klickar du på **filen**, klickar du på **projektstruktur**, och klicka sedan på **bibliotek**. 
     2. Klicka på hello lägga till ikonen (![lägga till ikonen](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)), klickar du på **Java**, och gå sedan toohello plats där du sparade hello JDBC driver jar. Följ hello prompter tooadd hello jar-filen toohello projekt bibliotek.

         ![lägga till beroenden som saknas](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "lägga till den saknade beroende burkar")
     3. Klicka på **Använd**.

7. Skapa hello utdata jar-filen. Utför följande steg hello.

   1. I hello **projektstruktur** dialogrutan klickar du på **artefakter** och klicka sedan på hello plus symbolen. Hello popup-menyn klickar du på **JAR**, och klicka sedan på **från moduler med beroenden**.      
       
       ![Apache Spark streaming exempel – skapa JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streaming exempel – skapa JAR")
   2. I hello **skapa JAR från moduler** dialogrutan klickar du på ellipsknappen hello (![tre punkter](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) mot hello **Main klassen**.
   3. I hello **Välj Main klassen** dialogrutan väljer någon av hello tillgängliga klasser och klicka sedan på **OK**.
      
       ![Apache Spark streaming exempel – Välj klass för jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streaming exempel – Välj klass för jar")
   4. I hello **skapa JAR från moduler** dialogrutan Kontrollera hello alternativet för**extrahera toohello mål JAR** är markerad och klicka sedan på **OK**. Detta skapar en enda JAR med alla beroenden.
      
       ![Apache Spark streaming exempel – skapa jar från moduler](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streaming exempel – skapa jar från moduler")
   5. Hej **utdata Layout** fliken listar alla hello burkar som ingår som en del av hello Maven-projekt. Du kan välja och ta bort hello sådana där hello Scala program har ingen direkt beroende. För hello program skapar vi här, kan du ta bort alla utom hello senast (**spark-streaming-data-beständiga-exempel kompilera utdata**). Välj hello burkar toodelete och klicka sedan på hello **ta bort** ikon (![ikonen Ta bort](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).
      
       ![Apache Spark streaming exempel – ta bort extraherade burkar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streaming exempel – ta bort extraherade burkar")
      
       Kontrollera att **bygger på Kontrollera** är markerad, vilket säkerställer att hello jar skapas varje gång hello projektet skapats eller uppdaterats. Klicka på **Använd**.
   6. I hello **utdata Layout** fliken höger längst hello hello **element** finns hello SQL JDBC jar som du lagt till tidigare toohello projekt bibliotek. Du måste lägga till den här toohello **utdata Layout** fliken. Högerklicka på hello jar-filen och klicka sedan på **extrahera roten till utdata**.
      
       ![Apache Spark streaming exempel – extrahera beroende jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streaming exempel – extrahera beroende jar")  
      
       Hej **utdata Layout** fliken bör nu se ut så här.
      
       ![Apache Spark streaming exempel – fliken slutversionen](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streaming exempel – fliken slutversionen")        
      
       I hello **projektstruktur** dialogrutan klickar du på **tillämpa** och klicka sedan på **OK**.    
   7. Hello menyraden klickar du på **skapa**, och klicka sedan på **Se projektet**. Du kan också klicka på **skapa artefakter** toocreate hello jar. hello utdata jar skapas under **\classes\artifacts**.
      
       ![Apache Spark streaming exempel - utdata JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "Apache Spark streaming exempel - utdata JAR")

## <a name="run-hello-application-remotely-on-a-spark-cluster-using-livy"></a>Kör hello via fjärranslutning på ett Spark-kluster med Livy

I den här artikeln använder du Livius toorun hello Apache Spark streaming program via fjärranslutning på ett Spark-kluster. Detaljerad information om hur toouse Livius med HDInsight Spark-kluster finns i [skicka jobb via fjärranslutning tooan Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-livy-rest-interface.md). Innan du kan börja hello Spark streaming programmet körs, finns det några saker du bör göra:

1. Starta hello lokala fristående toogenerate programhändelser och skickas tooEvent hubb. Använd följande kommando toodo så hello:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "mysbnamespace" --eventhubs-name "myeventhub" --policy-name "mysendpolicy" --policy-key "<policy key>" --message-length 32 --thread-count 32 --message-count -1

2. Kopiera hello strömning jar (**spark-streaming-data-beständiga-examples.jar**) toohello Azure Blob-lagring som associerats med hello kluster. Detta gör hello jar tillgänglig tooLivy. Du kan använda [ **AzCopy**](../storage/common/storage-use-azcopy.md), ett kommando rad toodo-verktyget så. Det finns många andra klienter kan du använda tooupload data. Du kan hitta mer om dem i [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md).
3. Installera CURL på hello datorn där du kör dessa program från. Vi använder CURL tooinvoke hello Livius slutpunkter toorun hello jobb via fjärranslutning.

### <a name="run-hello-spark-streaming-application-tooreceive-hello-events-into-an-azure-storage-blob-as-text"></a>Kör hello Spark streaming tooreceive hello programhändelser till en Azure Storage Blob som text

Öppna en kommandotolk, navigera toohello katalog där du installerade CURL och kör följande kommando (Ersätt användarnamn/lösenord och kluster namn) hello:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputBlob.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Hej parametrar i hello filen **inputBlob.txt** definieras enligt följande:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsEventCount", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Låt oss förstå hello parametrar i hello indatafilen är:

* **filen** är hello sökvägen toohello programmet jar-filen på hello Azure storage-konto som är associerade med hello-kluster.
* **className** är hello klassen i hello jar hello namn.
* **argument** är hello lista över de argument som krävs av hello-klass
* **numExecutors** är hello antal kärnor som används av Spark toorun hello direktuppspelning av program. Detta ska alltid vara minst två gånger hello antalet partitioner i Händelsehubben.
* **executorMemory**, **executorCores**, **driverMemory** är parametrar som används tooassign krävs resurser toohello strömmande program.

> [!NOTE]
> Du behöver inte toocreate hello utdatamappar (EventCheckpoint, EventCount/EventCount10) som används som parametrar. direktuppspelning av programmet hello skapar dem åt dig.
>
>

Du bör se utdata som liknar hello följande när du kör hello-kommandot:

    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /18
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Tue, 01 Dec 2015 05:39:10 GMT
    < Content-Length: 37
    <
    {"id":1,"state":"starting","log":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact

Anteckna hello batch-ID i hello sista raden i hello utdata (i det här exemplet är det '1'). tooverify som hello program har körts, titta på ditt Azure storage-konto som är associerade med hello kluster och du bör se hello **/EventCount/EventCount10** mapp som skapade det. Den här mappen bör innehålla blobbar som samlar in hello antalet händelser som bearbetas inom hello tidsperiod som angetts för parametern hello **batch intervall i sekunder**.

hello Spark streaming programmet fortsätter toorun tills du avsluta den. toodo så Använd hello följande kommando:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/1"

### <a name="run-hello-applications-tooreceive-hello-events-into-an-azure-storage-blob-as-json"></a>Kör hello program tooreceive hello händelser till en Azure Storage Blob som JSON
Öppna en kommandotolk, navigera toohello katalog där du installerade CURL och kör följande kommando (Ersätt användarnamn/lösenord och kluster namn) hello:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputJSON.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Hej parametrar i hello filen **inputJSON.txt** definieras enligt följande:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureBlobAsJSON", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-store-folder", "/EventStore10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

hello parametrar är liknande toowhat som du angav för hello text-utdata i hello föregående steg. Igen och behöver du inte toocreate hello utdatamappar (EventCheckpoint, EventCount/EventCount10) som används som parametrar. direktuppspelning av programmet hello skapar dem åt dig.

 När du kör kommandot hello kan du titta på ditt Azure storage-konto som är associerade med hello kluster och du bör se hello **/EventStore10** mapp som skapade det. Öppna en fil med prefixet **del -** och du bör se hello händelser som har bearbetats i en JSON-format.

### <a name="run-hello-applications-tooreceive-hello-events-into-a-hive-table"></a>Köra hello program tooreceive hello händelser till en Hive-tabell
toorun hello Spark streaming program som dataströmmar händelser till en annan registreringsdatafil tabell du måste vissa ytterligare komponenter. Dessa är:

* datanucleus-api-jdo-3.2.6.jar
* datanucleus-rdbms-3.2.9.jar
* datanucleus-core-3.2.10.jar
* hive-site.xml

Hej **.jar** filer är tillgängliga på HDInsight Spark-kluster på `/usr/hdp/current/spark-client/lib`. Hej **hive-site.xml** finns på `/usr/hdp/current/spark-client/conf`.

Du kan använda [WinScp](http://winscp.net/eng/download.php) toocopy över de här filerna från hello klustret tooyour lokal dator. Du kan sedan använda verktyg toocopy filerna över tooyour storage-konto som är associerade med hello klustret. Mer information om hur tooupload filer toohello storage-konto finns [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md).

När du har kopierat över hello filer tooyour Azure storage-konto kan öppna en kommandotolk, navigera toohello katalog där du installerade CURL och kör följande kommando (Ersätt användarnamn/lösenord och kluster namn) hello:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputHive.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Hej parametrar i hello filen **inputHive.txt** definieras enligt följande:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToHiveTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-hive-table", "EventHiveTable10" ], "jars":["wasb:///example/jars/datanucleus-api-jdo-3.2.6.jar", "wasb:///example/jars/datanucleus-rdbms-3.2.9.jar", "wasb:///example/jars/datanucleus-core-3.2.10.jar"], "files":["wasb:///example/jars/hive-site.xml"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

hello parametrar är liknande toowhat som du angav för hello text-utdata i hello föregående steg. Igen och du behöver inte toocreate hello utdata mappar (EventCheckpoint, EventCount/EventCount10) eller hello utdata Hive-tabell (EventHiveTable10) som används som parametrar. direktuppspelning av programmet hello skapar dem åt dig. Observera att hello **burkar** och **filer** alternativet innehåller sökvägar toohello .jar filer och hello hive-site.xml som du kopierade över toohello storage-konto.

tooverify som hello hive-tabellen har skapats kan du SSH till hello klustret och köra Hive-frågor. Instruktioner finns i [använda Hive med Hadoop i HDInsight med SSH](hdinsight-hadoop-use-hive-ssh.md). När du är ansluten med SSH, du kan köra följande kommando tooverify hello som hello Hive-tabell **EventHiveTable10**, skapas.

    show tables;

Du bör se en utdata liknande toohello följande:

    OK
    eventhivetable10
    hivesampletable

Du kan också köra en SELECT-frågan tooview hello tabell hello innehåll.

    SELECT * FROM eventhivetable10 LIMIT 10;

Du bör se utdata som liknar hello följande:

    ZN90apUSQODDTx7n6Toh6jDbuPngqT4c
    sor2M7xsFwmaRW8W8NDwMneFNMrOVkW1
    o2HcsU735ejSi2bGEcbUSB4btCFmI1lW
    TLuibq4rbj0T9st9eEzIWJwNGtMWYoYS
    HKCpPlWFWAJILwR69MAq863nCWYzDEw6
    Mvx0GQOPYvPR7ezBEpIHYKTKiEhYammQ
    85dRppSBSbZgThLr1s0GMgKqynDUqudr
    5LAWkNqorLj3ZN9a2mfWr9rZqeXKN4pF
    ulf9wSFNjD7BZXCyunozecov9QpEIYmJ
    vWzM3nvOja8DhYcwn0n5eTfOItZ966pa
    Time taken: 4.434 seconds, Fetched: 10 row(s)


### <a name="run-hello-applications-tooreceive-hello-events-into-an-azure-sql-database-table"></a>Köra hello program tooreceive hello händelser till en Azure SQL-databastabell
Kontrollera att du har en Azure SQL-databas som har skapats innan du kör det här steget. Instruktioner finns i [skapa en SQL-databas i minuter](../sql-database/sql-database-get-started.md). toocomplete det här avsnittet, behöver du värden för databasens namn, Databasservernamnet och Hej administratör Databasautentiseringsuppgifter som parametrar. Du behöver inte toocreate hello databastabell om. hello Spark streaming programmet skapas som.

Öppna en kommandotolk, navigera toohello katalog där du installerade CURL och kör följande kommando hello:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputSQL.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Hej parametrar i hello filen **inputSQL.txt** definieras enligt följande:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureSQLTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--sql-server-fqdn", "<database-server-name>.database.windows.net", "--sql-database-name", "mysparkdatabase", "--database-username", "sparkdbadmin", "--database-password", "<put-password-here>", "--event-sql-table", "EventContent" ], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

tooverify som hello programmet körs utan problem, kan du ansluta toohello Azure SQL database med SQL Server Management Studio. Anvisningar för hur toodo som finns i [ansluta tooSQL databasen med SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md). När du är ansluten toohello databasen kan du navigera toohello **EventContent** tabell som har skapats av hello direktuppspelning av program. Du kan köra en snabb fråga tooget hello data från hello tabell. Kör följande fråga hello:

    SELECT * FROM EventCount

Du bör se utdata liknande toohello följande:

    00046b0f-2552-4980-9c3f-8bba5647c8ee
    000b7530-12f9-4081-8e19-90acd26f9c0c
    000bc521-9c1b-4a42-ab08-dc1893b83f3b
    00123a2a-e00d-496a-9104-108920955718
    0017c68f-7a4e-452d-97ad-5cb1fe5ba81b
    001KsmqL2gfu5ZcuQuTqTxQvVyGCqPp9
    001vIZgOStka4DXtud0e3tX7XbfMnZrN
    00220586-3e1a-4d2d-a89b-05c5892e541a
    0029e309-9e54-4e1b-84be-cd04e6fce5ec
    003333cf-874f-4045-9da3-9f98c2b4ea49
    0043c07e-8d73-420a-9af7-1fcb94575356
    004a11a9-0c2c-4bc0-a7d5-2e0ebd947ab9


## <a name="seealso"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](hdinsight-apache-spark-overview.md)
* [Design av mottagare-baserade anslutningen och direkt DStream](https://www.slideshare.net/NanZhu/seattle-sparkmeetup032317)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Webbplatslogganalys med Spark i HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Verktyg och tillägg
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-appar](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]: ../storage-create-storage-account/
