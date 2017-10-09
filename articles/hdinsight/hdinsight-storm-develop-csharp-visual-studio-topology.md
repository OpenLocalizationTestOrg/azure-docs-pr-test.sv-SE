---
title: aaaApache Storm-topologier med Visual Studio och C# - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toocreate Storm-topologier i C#. Skapa en enkel word antal topologi i Visual Studio med hjälp av hello Hadoop-verktyg för Visual Studio."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 380d804f-a8c5-4b20-9762-593ec4da5a0d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: larryfr
ms.openlocfilehash: b3fb01a4dda144fd7fb4141e624e31e667f93753
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-c-topologies-for-apache-storm-by-using-hello-data-lake-tools-for-visual-studio"></a>Utveckla C#-topologier för Apache Storm genom att använda hello Data Lake-verktyg för Visual Studio

Lär dig hur toocreate en C# Storm-topologi med hjälp av hello Azure Data Lake (Hadoop) tools för Visual Studio. Det här dokumentet går igenom hello processen att skapa ett Storm-projekt i Visual Studio, lokal testning och distribuera den tooan Apache Storm på Azure HDInsight-kluster.

Du också lära dig hur toocreate hybridtopologier som använder C# och Java-komponenter.

> [!NOTE]
> Medan hello stegen i det här dokumentet är beroende av en Windows-utvecklingsmiljö med Visual Studio, kan hello kompilerade projekt vara skickade tooeither ett Linux- eller Windows-baserade HDInsight-kluster. Linux-baserade kluster som skapas efter den 28 oktober 2016 stöder endast SCP.NET topologier.

toouse C#-topologi med en Linux-baserade kluster, måste du uppdatera hello Microsoft.SCP.Net.SDK NuGet-paketet används av ditt projekt tooversion 0.10.0.6 eller senare. hello version av hello paketet måste även matcha hello huvudversion av Storm installerad på HDInsight.

| HDInsight-version | Storm-version | SCP.NET version | Monoljud standardversion |
|:-----------------:|:-------------:|:---------------:|:--------------------:|
| 3.3 |0.10.x |0.10.x.x</br>(endast på Windows-baserade HDInsight) | Ej tillämpligt |
| 3.4 | 0.10.0.x | 0.10.0.x | 3.2.8 |
| 3.5 | 1.0.2.x | 1.0.0.x | 4.2.1 |
| 3.6 | 1.1.0.x | 1.0.0.x | 4.2.8 |

> [!IMPORTANT]
> C#-topologier på Linux-baserade kluster måste använda .NET 4.5 och Använd monoljud toorun på hello HDInsight-kluster. Kontrollera [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/) för potentiella inkompatibiliteter.

## <a name="install-visual-studio"></a>Installera Visual Studio

Du kan utveckla C#-topologier med SCP.NET med någon av följande versioner av Visual Studio hello:

* Visual Studio 2012 med [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)

* Visual Studio 2013 med [uppdatering 4](http://www.microsoft.com/download/details.aspx?id=44921) eller [Visual Studio Community 2013](http://go.microsoft.com/fwlink/?LinkId=517284)

* Visual Studio 2015 eller [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)

* 2017 för Visual Studio (någon utgåva)

## <a name="install-data-lake-tools-for-visual-studio"></a>Installera Data Lake-verktyg för Visual Studio

tooinstall Data Lake-verktyg för Visual Studio gör hello i [komma igång med Data Lake-verktyg för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

## <a name="install-java"></a>Installera Java

När du skickar en Storm-topologi från Visual Studio genererar SCP.NET en zip-fil som innehåller hello topologi och beroenden. Java är används toocreate dessa zip-filer, eftersom den använder ett format som är mer kompatibel med Linux-baserade kluster.

1. Installera hello Java Developer Kit (JDK) 7 eller senare på din utvecklingsmiljö. Du kan hämta hello Oracle JDK från [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html). Du kan också använda [andra Java-distributioner](http://openjdk.java.net/).

2. Hej `JAVA_HOME` miljö variabeln måste punkt toohello katalog som innehåller Java.

3. Hej `PATH` miljövariabeln måste innehålla hello `%JAVA_HOME%\bin` directory.

Du kan använda följande C#-konsolen programmet tooverify att Java och hello JDK är korrekt installerade hello:

```csharp
using System;
using System.IO;
namespace ConsoleApplication2
{
   class Program
   {
       static void Main(string[] args)
       {
           string javaHome = Environment.GetEnvironmentVariable(“JAVA_HOME”);
           if (!string.IsNullOrEmpty(javaHome))
           {
               string jarExe = Path.Combine(javaHome + @”\bin”, “jar.exe”);
               if (File.Exists(jarExe))
               {
                   Console.WriteLine(“JAVA Is Installed properly”);
                    return;
               }
               else
               {
                   Console.WriteLine(“A valid JAVA JDK is not found. Looks like JRE is installed instead of JDK.”);
               }
           }
           else
           {
             Console.WriteLine(“A valid JAVA JDK is not found. JAVA_HOME environment variable is not set.”);
           }
       }  
   }
}
```

## <a name="storm-templates"></a>Storm-mallar

hello Data Lake-verktyg för Visual Studio innehåller hello följande mallar:

| Projekttypen | Visar |
| --- | --- |
| Storm-program |Ett tomt Storm-topologi projekt. |
| Storm Azure SQL Writer exempel |Hur toowrite tooAzure SQL-databas. |
| Storm Azure Cosmos DB Reader-exempel |Hur tooread från Azure Cosmos-databasen. |
| Storm Azure Cosmos DB Writer exempel |Hur toowrite tooAzure Cosmos DB. |
| Storm EventHub Reader-exempel |Hur tooread från Azure Event Hubs. |
| Storm EventHub Writer exempel |Hur toowrite tooAzure Händelsehubbar. |
| Storm HBase Reader-exempel |Hur tooread från HBase på HDInsight-kluster. |
| Storm HBase Writer exempel |Hur toowrite tooHBase på HDInsight-kluster. |
| Storm Hybrid-exempel |Hur toouse en Java-komponent. |
| Storm-exempel |En grundläggande word count-topologi. |

> [!WARNING]
> Inte alla mallar för att fungera med Linux-baserade HDInsight. Nuget-paket som används av hello mallar är kanske inte kompatibelt med Mono. Kontrollera hello [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentera och använda hello [.NET Portability Analyzer](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) tooidentify potentiella problem.

I hello stegen i det här dokumentet använder du hello grundläggande Storm-program projektet typen toocreate en topologi.

### <a name="hbase-templates-notes"></a>HBase mallar anteckningar

Hej HBase läsare och skrivare mallar kan du använda hello HBase REST API inte hello HBase Java API toocommunicate med en HBase på HDInsight-kluster.

### <a name="eventhub-templates-notes"></a>Anteckningar för EventHub-mallar

> [!IMPORTANT]
> hello Java-baserad EventHub-kanalen komponent som ingår i hello EventHub Reader mallen inte kanske fungerar med Storm på HDInsight version 3.5 eller senare. Det finns en uppdaterad version av den här komponenten på [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).

En exempeltopologi som använder den här komponenten och fungerar med Storm på HDInsight 3.5 finns [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

## <a name="create-a-c-topology"></a>Skapa en C#-topologi

1. Öppna Visual Studio, markera **filen** > **ny**, och välj sedan **projekt**.

2. Från hello **nytt projekt** fönstret expanderar **installerad** > **mallar**, och välj **Azure Data Lake**. Välj hello listan över mallar **Storm-program**. Längst ned hello hello-skärmen, ange **WordCount** som hello hello programmets namn.

    ![Skärmbild av nytt projekt fönster](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

3. När du har skapat hello projekt, ska du ha hello följande filer:

   * **Program.CS**: den här filen definierar hello topologi för projektet. En standard-topologi som består av en kanal och en bult skapas som standard.

   * **Spout.CS**: en exempel-kanal som genererar slumptal.

   * **Bolt.CS**: ett exempel bult som ett antal siffror som sänds av hello-kanal.

     När du skapar hello projekt NuGet hämtningar hello senaste [SCP.NET paketet](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).

     [!INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

### <a name="implement-hello-spout"></a>Implementera hello kanal

1. Öppna **Spout.cs**. Kanaler är används tooread data i en topologi från en extern källa. hello huvudkomponenterna för en kanal är:

   * **NextTuple**: anropas av Storm när hello kanal tillåts tooemit nya tupplar.

   * **Ack** (endast transaktionella topologi): hanterar bekräftelser som initieras av andra komponenter i hello-topologi för tupplar skickas från hello-kanal. Bekräfta en tuppel kan hello kanal vet att den har bearbetats av underordnade komponenter.

   * **Misslyckas** (endast transaktionella topologi): hanterar tupplar som misslyckas bearbetning av andra komponenter i hello-topologi. Implementera en metod som misslyckas kan du toore-genererar hello tuppel så att den kan bearbetas igen.

2. Ersätt hello innehållet i hello **prata** klassen med följande text hello. Den här kanal avger slumpmässigt en mening i hello-topologi.

    ```csharp
    private Context ctx;
    private Random r = new Random();
    string[] sentences = new string[] {
        "hello cow jumped over hello moon",
        "an apple a day keeps hello doctor away",
        "four score and seven years ago",
        "snow white and hello seven dwarfs",
        "i am at two with nature"
    };

    public Spout(Context ctx)
    {
        // Set hello instance context
        this.ctx = ctx;

        Context.Logger.Info("Generator constructor called");

        // Declare Output schema
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // hello schema for hello default output stream is
        // a tuple that contains a string field
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
    }

    // Get an instance of hello spout
    public static Spout Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Spout(ctx);
    }

    public void NextTuple(Dictionary<string, Object> parms)
    {
        Context.Logger.Info("NextTuple enter");
        // hello sentence toobe emitted
        string sentence;

        // Get a random sentence
        sentence = sentences[r.Next(0, sentences.Length - 1)];
        Context.Logger.Info("Emit: {0}", sentence);
        // Emit it
        this.ctx.Emit(new Values(sentence));

        Context.Logger.Info("NextTuple exit");
    }

    public void Ack(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }
    ```

### <a name="implement-hello-bolts"></a>Implementera hello bultar

1. Ta bort befintliga hello **Bolt.cs** filen från hello-projekt.

2. I **Solution Explorer**, högerklicka på hello-projektet och välj **Lägg till** > **nytt objekt**. Välj hello listan **Storm bult**, och ange **Splitter.cs** som hello namn. Upprepa den här processen toocreate andra bulten med namnet **Counter.cs**.

   * **Splitter.CS**: implementerar en bult som delar upp meningar i enskilda ord och genererar en ny dataström ord.

   * **Counter.CS**: implementerar en bult som räknar varje ord och genererar en ny dataström ord och hello antalet för varje ord.

     > [!NOTE]
     > Dessa bultar läsa och skriva toostreams, men du kan också använda en bult toocommunicate med källor, till exempel en databas eller tjänst.

3. Öppna **Splitter.cs**. Det har endast en metod som standard: **kör**. hello Execute-metoden anropas när hello bult tar emot en tuppel för bearbetning. Här kan du läsa och bearbeta inkommande tupplar och generera utgående tupplar.

4. Ersätt hello innehållet i hello **delningslisten** klassen med följande kod hello:

    ```csharp
    private Context ctx;

    // Constructor
    public Splitter(Context ctx)
    {
        Context.Logger.Info("Splitter constructor called");
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // Input contains a tuple with a string field (hello sentence)
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // Outbound contains a tuple with a string field (hello word)
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance of hello bolt
    public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Splitter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get hello sentence from hello tuple
        string sentence = tuple.GetString(0);
        // Split at space characters
        foreach (string word in sentence.Split(' '))
        {
            Context.Logger.Info("Emit: {0}", word);
            //Emit each word
            this.ctx.Emit(new Values(word));
        }

        Context.Logger.Info("Execute exit");
    }
    ```

5. Öppna **Counter.cs**, och Ersätt hello klassen innehållet med hello följande:

    ```csharp
    private Context ctx;

    // Dictionary for holding words and counts
    private Dictionary<string, int> counts = new Dictionary<string, int>();

    // Constructor
    public Counter(Context ctx)
    {
        Context.Logger.Info("Counter constructor called");
        // Set instance context
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string field - hello word
        inputSchema.Add("default", new List<Type>() { typeof(string) });

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string and integer field - hello word and hello word count
        outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance
    public static Counter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Counter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get hello word from hello tuple
        string word = tuple.GetString(0);
        // Do we already have an entry for hello word in hello dictionary?
        // If no, create one with a count of 0
        int count = counts.ContainsKey(word) ? counts[word] : 0;
        // Increment hello count
        count++;
        // Update hello count in hello dictionary
        counts[word] = count;

        Context.Logger.Info("Emit: {0}, count: {1}", word, count);
        // Emit hello word and count information
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
        Context.Logger.Info("Execute exit");
    }
    ```

### <a name="define-hello-topology"></a>Definiera hello-topologi

Kanaler och bultar ordnas i ett diagram som definierar hur hello data flödar mellan komponenter. För den här topologin är hello diagrammet:

![Diagram över hur komponenter ordnas](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Meningar orsakat från hello kanal och är distribuerade tooinstances av hello delningslisten bulten. hello delningslisten bult delar hello meningar i ord som är distribuerade toohello räknaren bulten.

Eftersom ordräkning lagras lokalt i hello räknarinstansen, vi vill toomake till att specifika ord flöda toohello samma räknarinstansen bulten. Varje instans håller reda på specifika ord. Eftersom hello delningslisten bult upprätthåller inget tillstånd, det verkligen spelar ingen roll vilken instans av hello delningslisten tar emot vilka meningen.

Öppna **Program.cs**. viktiga hello-metoden är **GetTopologyBuilder**, som har använt toodefine hello-topologi som har skickats tooStorm. Ersätt hello innehållet i **GetTopologyBuilder** med hello följande kod tooimplement hello-topologi som beskrivs ovan:

```csharp
// Create a new topology named 'WordCount'
TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

// Add hello spout toohello topology.
// Name hello component 'sentences'
// Name hello field that is emitted as 'sentence'
topologyBuilder.SetSpout(
    "sentences",
    Spout.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
    },
    1);
// Add hello splitter bolt toohello topology.
// Name hello component 'splitter'
// Name hello field that is emitted 'word'
// Use suffleGrouping toodistribute incoming tuples
//   from hello 'sentences' spout across instances
//   of hello splitter
topologyBuilder.SetBolt(
    "splitter",
    Splitter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
    },
    1).shuffleGrouping("sentences");

// Add hello counter bolt toohello topology.
// Name hello component 'counter'
// Name hello fields that are emitted 'word' and 'count'
// Use fieldsGrouping tooensure that tuples are routed
//   toocounter instances based on hello contents of field
//   position 0 (hello word). This could also have been
//   List<string>(){"word"}.
//   This ensures that hello word 'jumped', for example, will always
//   go toohello same instance
topologyBuilder.SetBolt(
    "counter",
    Counter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
    },
    1).fieldsGrouping("splitter", new List<int>() { 0 });

// Add topology config
topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
{
    {"topology.kryo.register","[\"[B\"]"}
});

return topologyBuilder;
```

## <a name="submit-hello-topology"></a>Skicka hello-topologi

1. I **Solution Explorer**, högerklicka på hello-projektet och välj **skicka tooStorm på HDInsight**.

   > [!NOTE]
   > Om du uppmanas du ange hello autentiseringsuppgifter för din Azure-prenumeration. Om du har mer än en prenumeration kan du logga in toohello en som innehåller ditt Storm på HDInsight-kluster.

2. Välj ditt Storm på HDInsight-kluster från hello **Storm-kluster** listrutan och välj sedan **skicka**. Du kan övervaka om hello ansökan har genomförts med hello **utdata** fönster.

3. När hello-topologi har skickats, hello **Storm-topologier** för hello klustret ska visas. Välj hello **WordCount** topologi från hello listan tooview information om hello kör topologi.

   > [!NOTE]
   > Du kan också visa **Storm-topologier** från **Server Explorer**. Expandera **Azure** > **HDInsight**, högerklicka på ett Storm på HDInsight-kluster och välj sedan **visa Storm-topologier**.

    tooview information om hello komponenter i hello topologi dubbelklicka hello-komponenten i hello diagram.

4. Från hello **Topology Summary** klickar du på **Kill** toostop hello-topologi.

   > [!NOTE]
   > Storm-topologier fortsätta toorun förrän de inaktiveras eller hello kluster har tagits bort.

## <a name="transactional-topology"></a>Transaktionell topologi

hello tidigare topologin är icke-transaktionell. hello komponenter i hello topologi implementeras inte funktionen tooreplaying meddelanden. Ett exempel på en transaktionell topologi, skapa ett projekt och välj **Storm exempel** som hello projekttypen.

Transaktionell topologier implementera hello följande toosupport omsändning av data:

* **Cachelagring av metadata**: hello kanal måste lagra metadata om hello data som sänds, så att hello data kan hämtas och orsakat igen om ett fel inträffar. Eftersom hello data som sänds av hello exempel är liten lagras hello rådata för varje tuppel i en ordlista för att svarsidentifiering.

* **Ack**: varje bult i hello-topologi kan anropa `this.ctx.Ack(tuple)` tooacknowledge att den har bearbetat en tuppel. När alla bultar har ADE hello tuppel, hello `Ack` hello kanal-metoden har anropats. Hej `Ack` metoden kan hello kanal tooremove data som har lagrats i cacheminnet för att svarsidentifiering.

* **Misslyckas**: varje bult kan anropa `this.ctx.Fail(tuple)` tooindicate bearbetningen misslyckades för en tuppel. hello fel sprider toohello `Fail` metod för hello kanal, där hello tuppel spelas upp med hjälp av cachelagrade metadata.

* **Aktivitetssekvensen ID**: när avger en tuppel, en unik sekvens-ID kan anges. Det här värdet identifierar hello tuppel för bearbetning av replay (Ack och misslyckas). Till exempel hello kanal i hello **Storm exempel** projektet använder hello följande vid sändning av data:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    Den här koden skickar en tuppel som innehåller en mening toohello standard dataström med hello sekvens-ID-värde som finns i **lastSeqId**. I det här exemplet **lastSeqId** ökas för varje tuppel orsakat.

Som visas i hello **Storm exempel** projekt, om en komponent är transaktionell kan anges vid körning, baserat på konfigurationen.

## <a name="hybrid-topology-with-c-and-java"></a>Hybrid-topologi med C# och Java

Du kan också använda Data Lake-verktyg för Visual Studio toocreate hybridtopologier, där vissa komponenter är C# och andra är Java.

Ett exempel på en hybrid-topologi, skapa ett projekt och välj **Storm Hybrid exempel**. Den här typen av exemplet visar hello följande begrepp:

* **Java-kanal** och **C# bult**: definierade i **HybridTopology_javaSpout_csharpBolt**.

    * En transaktionell version har definierats i **HybridTopologyTx_javaSpout_csharpBolt**.

* **C#-kanal** och **Java bult**: definierade i **HybridTopology_csharpSpout_javaBolt**.

    * En transaktionell version har definierats i **HybridTopologyTx_csharpSpout_javaBolt**.

  > [!NOTE]
  > Den här versionen visas också hur toouse Clojure kod från en textfil som en Java-komponent.


tooswitch hello topologin som används när hello projektet skickas bara flytta hello `[Active(true)]` instruktionen toohello topologi du vill toouse, innan den skickas toohello klustret.

> [!NOTE]
> Alla hello Java-filer som krävs har angetts som en del av det här projektet i hello **JavaDependency** mapp.

Tänk hello följande när du skapar och skickar en hybrid-topologi:

* Du måste använda **JavaComponentConstructor** toocreate en instans av hello Java-klass för en kanal eller en bult.

* Du bör använda **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** tooserialize till eller från Java-komponenter från Java-dataobjekt tooJSON.

* När hello topologi toohello server, måste du använda hello **ytterligare konfigurationer** alternativet toospecify hello **Java sökvägar**. hello-sökvägen ska vara hello-katalog som innehåller hello JAR-filer som innehåller Java-klasser.

### <a name="azure-event-hubs"></a>Azure Event Hubs

SCP.NET version 0.9.4.203 införs en ny klass och metod för att arbeta med hello Event Hub kanal (en Java kanal som läser från Händelsehubbar). När du skapar en topologi som använder en Event Hub-kanal, Använd hello följande metoder:

* **EventHubSpoutConfig** klass: skapar ett objekt som innehåller hello konfiguration för komponenten för hello-kanal.

* **TopologyBuilder.SetEventHubSpout** metod: lägger till hello Event Hub kanal komponenten toohello topologi.

> [!NOTE]
> Du måste fortfarande använda hello **CustomizedInteropJSONSerializer** tooserialize data som produceras av hello-kanal.

## <a id="configurationmanager"></a>Använd ConfigurationManager

Använd inte **ConfigurationManager** tooretrieve configuration värden från bultar och prata komponenter. Detta kan orsaka ett undantag för null-pekare. I stället skickas hello konfiguration för ditt projekt i hello Storm-topologi som en nyckel och värde-par i kontexten för hello-topologi. Varje komponent som förlitar sig på konfigurationsvärden måste hämta dem från hello kontext under initieringen.

hello följande kod visar hur tooretrieve dessa värden:

```csharp
public class MyComponent : ISCPBolt
{
    // toohold configuration information loaded from context
    Configuration configuration;
    ...
    public MyComponent(Context ctx, Dictionary<string, Object> parms)
    {
        // Save a copy of hello context for this component instance
        this.ctx = ctx;
        // If it exists, load hello configuration for hello component
        if(parms.ContainsKey(Constants.USER_CONFIG))
        {
            this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
        }
        // Retrieve hello value of "Foo" from configuration
        var foo = this.configuration.AppSettings.Settings["Foo"].Value;
    }
    ...
}
```

Om du använder en `Get` metoden tooreturn en instans av komponenten, måste du se till att den skickar båda hello `Context` och `Dictionary<string, Object>` parametrar toohello konstruktor. hello följande exempel är en grundläggande `Get` metod som korrekt skickar dessa värden:

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-tooupdate-scpnet"></a>Hur tooupdate SCP.NET

Nya versioner av SCP.NET stöd för uppgradering av paketet via NuGet. När en ny uppdatering är tillgänglig kan få du ett meddelande om uppgraderingen. toomanually Sök efter en uppgradering, Följ dessa steg:

1. I **Solution Explorer**, högerklicka på hello-projektet och välj **hantera NuGet-paket**.

2. Välj hello Pakethanteraren **uppdateringar**. Om en uppdatering är tillgänglig, visas. Klicka på **uppdatering** för hello paketet tooinstall den.

> [!IMPORTANT]
> Om projektet har skapats med en tidigare version av SCP.NET som inte har använt NuGet, måste du utföra hello följande steg tooupdate tooa nyare version:
>
> 1. I **Solution Explorer**, högerklicka på hello-projektet och välj **hantera NuGet-paket**.
> 2. Med hjälp av hello **Sök** fältet, söka efter och Lägg sedan till **Microsoft.SCP.Net.SDK** toohello projekt.

## <a name="troubleshoot-common-issues-with-topologies"></a>Felsöka vanliga problem med topologier

### <a name="null-pointer-exceptions"></a>Undantag för null-pekare

När du använder en C#-topologi med en Linux-baserade HDInsight-kluster, bultar och prata komponenter som använder **ConfigurationManager** tooread konfigurationsinställningar vid körning kan returnera null-pekare undantag.

hello konfiguration för ditt projekt skickas till hello Storm-topologi som en nyckel och värde-par i kontexten för hello-topologi. De kan hämtas från hello Ordlisteobjekt som skickas tooyour komponenter när de har initierats.

Mer information finns i hello [ConfigurationManager](#configurationmanager) i det här dokumentet.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Hello följande fel kan uppstå när du använder en C#-topologi med en Linux-baserade HDInsight-kluster:

    System.TypeLoadException: Failure has occurred while loading a type.

Det här felet uppstår när du använder en binärfil som inte är kompatibel med hello version av .NET som har stöd för Mono.

För Linux-baserade HDInsight-kluster, kontrollerar du att projektet använder binärfiler som kompilerats för .NET 4.5.

### <a name="test-a-topology-locally"></a>Testa en topologi lokalt

Även om det är enkelt toodeploy ett topologi tooa kluster, i vissa fall kan behöva du tootest en topologi lokalt. Använd följande steg toorun hello och testa hello exempeltopologi i den här självstudiekursen lokalt i din utvecklingsmiljö.

> [!WARNING]
> Lokal testning fungerar bara för basic och C#-topologier endast. Du kan inte använda lokala testar hybridtopologier eller topologier med flera strömmar.

1. I **Solution Explorer**, högerklicka på hello-projektet och välj **egenskaper**. Ändra hello i hello projektegenskaperna **utdata typen** för**konsolprogram**.

    ![Skärmbild av projektegenskaperna med utdatatypen markerat](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

   > [!NOTE]
   > Kom ihåg toochange hello **utdata typen** tillbaka för**klassbiblioteket** innan du distribuerar hello topologi tooa klustret.

2. I **Solution Explorer**, högerklicka på hello-projektet och välj sedan **Lägg till** > **nytt objekt**. Välj **klassen**, och ange **LocalTest.cs** som hello klassnamn. Klicka slutligen på **Lägg till**.

3. Öppna **LocalTest.cs**, och Lägg till följande hello **med** uttryck överst hello:

    ```csharp
    using Microsoft.SCP;
    ```

4. Använd hello följande kod som hello hello **LocalTest** klass:

    ```csharp
    // Drives hello topology components
    public void RunTestCase()
    {
        // An empty dictionary for use when creating components
        Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

        #region Test hello spout
        {
            Console.WriteLine("Starting spout");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext spoutCtx = LocalContext.Get();
            // Get a new instance of hello spout, using hello local context
            Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

            // Emit 10 tuples
            for (int i = 0; i < 10; i++)
            {
                sentences.NextTuple(emptyDictionary);
            }
            // Use LocalContext toopersist hello data stream toofile
            spoutCtx.WriteMsgQueueToFile("sentences.txt");
            Console.WriteLine("Spout finished");
        }
        #endregion

        #region Test hello splitter bolt
        {
            Console.WriteLine("Starting splitter bolt");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext splitterCtx = LocalContext.Get();
            // Get a new instance of hello bolt
            Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);

            // Set hello data stream toohello data created by hello spout
            splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
            // Get a batch of tuples from hello stream
            List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
            // Process each tuple in hello batch
            foreach (SCPTuple tuple in batch)
            {
                splitter.Execute(tuple);
            }
            // Use LocalContext toopersist hello data stream toofile
            splitterCtx.WriteMsgQueueToFile("splitter.txt");
            Console.WriteLine("Splitter bolt finished");
        }
        #endregion

        #region Test hello counter bolt
        {
            Console.WriteLine("Starting counter bolt");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext counterCtx = LocalContext.Get();
            // Get a new instance of hello bolt
            Counter counter = Counter.Get(counterCtx, emptyDictionary);

            // Set hello data stream toohello data created by splitter bolt
            counterCtx.ReadFromFileToMsgQueue("splitter.txt");
            // Get a batch of tuples from hello stream
            List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
            // Process each tuple in hello batch
            foreach (SCPTuple tuple in batch)
            {
                counter.Execute(tuple);
            }
            // Use LocalContext toopersist hello data stream toofile
            counterCtx.WriteMsgQueueToFile("counter.txt");
            Console.WriteLine("Counter bolt finished");
        }
        #endregion
    }
    ```

    Ta en stund tooread via hello kodkommentarer. Den här koden använder **LocalContext** toorun hello komponenter i hello development environment och den kvarstår hello dataströmmen mellan komponenter tootext filer på lokala hello-enheten.

1. Öppna **Program.cs**, och Lägg till följande toohello hello **Main** metoden:

    ```csharp
    Console.WriteLine("Starting tests");
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
    // Initialize hello runtime
    SCPRuntime.Initialize();

    //If we are not running under hello local context, throw an error
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
    {
        throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
    }
    // Create test instance
    LocalTest tests = new LocalTest();
    // Run tests
    tests.RunTestCase();
    Console.WriteLine("Tests finished");
    Console.ReadKey();
    ```

2. Spara hello ändringar och klicka sedan på **F5** eller välj **felsöka** > **Start Debugging** toostart hello projektet. Ett konsolfönster ska visas och logga status som hello testerna pågår. När **testerna slutförda** visas trycker du på alla viktiga tooclose hello fönster.

3. Använd **Windows Explorer** toolocate hello katalog som innehåller ditt projekt. Till exempel: **C:\Users\<användarnamn > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. Öppna i den här katalogen, **Bin**, och klicka sedan på **felsöka**. Du bör se hello textfiler som skapas när hello tester kördes: sentences.txt, counter.txt och splitter.txt. Öppna varje textfil och inspektera hello data.

   > [!NOTE]
   > Strängdata kvarstår som en matris med decimalvärden i dessa filer. Till exempel \[[97,103,111]] i hello **splitter.txt** filen är hello word *och*.

> [!NOTE]
> Vara säker på att tooset hello **projekttyp** tillbaka för**klassbiblioteket** innan du distribuerar tooa Storm på HDInsight-kluster.

### <a name="log-information"></a>Informationen i felloggen

Du kan enkelt logga information från din topologi-komponenter med hjälp av `Context.Logger`. Hello följande skapas ett informationsmeddelande loggpost:

```csharp
Context.Logger.Info("Component started");
```

Loggade informationen kan visas från hello **loggen för Hadoop-tjänsten**, som finns i **Server Explorer**. Expandera hello post för ditt Storm på HDInsight-klustret och expandera sedan **loggen för Hadoop-tjänsten**. Välj slutligen hello log file tooview.

> [!NOTE]
> hello loggfilerna lagras i hello Azure storage-konto som används av klustret. tooview hello loggar i Visual Studio, måste du logga in toohello Azure-prenumeration som äger hello storage-konto.

### <a name="view-error-information"></a>Visa information om fel

tooview fel som uppstått i en topologi som körs med hello följande steg:

1. Från **Server Explorer**, högerklicka på hello Storm på HDInsight-kluster och välj **visa Storm-topologier**.

2. För hello **prata** och **Bolts**, hello **senaste fel** kolumnen innehåller information om hello senaste fel.

3. Välj hello **prata Id** eller **bulten Id** för hello-komponent som har ett fel visas. På sidan hello som visas, ytterligare fel information visas i hello **fel** avsnitt på hello hello sidans nederkant.

4. tooobtain mer information, Välj en **Port** från hello **Executors** på hello-sidan, toosee hello Storm worker logg för hello senaste några minuter.

### <a name="errors-submitting-topologies"></a>Fel skickar topologier

Om det uppstår fel som skickar en topologi tooHDInsight hittar loggar för hello server-komponenter som hanterar topologi skickas på ditt HDInsight-kluster. tooretrieve dessa loggar, Använd hello följande kommando från en kommandorad:

    scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .

Ersätt __sshuser__ med hello SSH-användarkontot för hello-kluster. Ersätt __klusternamn__ med hello namnet hello HDInsight-kluster. Mer information om hur du använder `scp` och `ssh` med HDInsight, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

Bidrag kan misslyckas av flera skäl:

* JDK är inte installerat eller är inte i hello sökväg.
* Nödvändiga beroendena för Java ingår inte i hello bidrag.
* Inkompatibel beroenden.
* Dubblettnamn topologi.

Om hello `hdinsight-scpwebapi.out` loggen innehåller en `FileNotFoundException`, detta kan bero på hello följande villkor:

* Hej JDK finns inte i hello sökväg på hello-utvecklingsmiljö. Kontrollera att hello JDK installeras i hello development environment och som `%JAVA_HOME%/bin` i hello sökvägen.
* Du saknar ett Java-beroende. Kontrollera att du inkluderar alla nödvändiga .jar-filer som en del av hello ansökan.

## <a name="next-steps"></a>Nästa steg

Ett exempel på databearbetning från Händelsehubbar finns [bearbeta händelser från Azure Event Hubs med Storm på HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).

Ett exempel på en C#-topologi som delar upp dataströmmen data i flera strömmar finns [C# Storm exempel](https://github.com/Blackmist/csharp-storm-example).

Mer information om hur du skapar C#-topologier, finns i toodiscover [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Flera sätt toowork med HDInsight och mer Storm på HDInsight-exempel finns hello följande dokument:

**Microsoft SCP.NET**

* [Programmeringsguide för SCP](hdinsight-storm-scp-programming-guide.md)

**Apache Storm på HDInsight**

* [Distribuera och övervaka topologier med Apache Storm på HDInsight](hdinsight-storm-deploy-monitor-topology.md)
* [Exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md)

**Apache Hadoop i HDInsight**

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)
* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)

**Apache HBase på HDInsight**

* [Komma igång med HBase på HDInsight](hdinsight-hbase-tutorial-get-started.md)
