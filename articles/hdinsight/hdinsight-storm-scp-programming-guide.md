---
title: "Programmeringsguide för aaaSCP.NET | Microsoft Docs"
description: "Lär dig hur toouse SCP.NET toocreate. NET-baserade Storm-topologier för att använda med Storm på HDInsight."
services: hdinsight
documentationcenter: 
author: raviperi
manager: jhubbard
editor: cgronlun
ms.assetid: 34192ed0-b1d1-4cf7-a3d4-5466301cf307
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/16/2016
ms.author: raviperi
ms.openlocfilehash: a57f4217b07e0e82a3f36650308695fbb45d9128
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scp-programming-guide"></a>Programmeringsguide för SCP
SCP är en plattform toobuild realtid, tillförlitliga och konsekventa hög prestanda databearbetning program. Det är byggt ovanpå [Apache Storm](http://storm.incubator.apache.org/) – en dataström som bearbetar system utformas av hello OSS communities. Storm är utformad med Nathan Marz och öppen källkod med Twitter. Den använder [Apache ZooKeeper](http://zookeeper.apache.org/), en annan Apache projektet tooenable mycket pålitlig distribuerade samordning och tillståndshantering. 

Inte bara hello SCP projektet portar Storm på Windows utan också hello-projektet till tillägg och anpassning av hello Windows-ekosystemet. hello tillägg inkluderar .NET developer upplevelse och bibliotek, hello anpassning innehåller Windows-baserad distribution. 

hello-tillägget och anpassning görs så att vi behöver inte toofork hello OSS projekt och vi kan utnyttja härledda ekosystem byggda på Storm.

## <a name="processing-model"></a>Processmodell
hello data i SCP modelleras som kontinuerliga strömmar av tupplar. Vanligtvis hello tupplar flöda till vissa kö först och sedan tas upp och transformeras av affärslogik som finns inuti en Storm-topologi, slutligen hello utdata kan skickas som tupplar tooanother SCP system eller vara allokerat toostores som distribuerat filsystem eller databaser som SQL Server.

![Ett diagram av en kö mata data tooprocessing som ett datalager-flöden](media/hdinsight-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

I Storm definierar en Programtopologi ett diagram över beräkning. Varje nod i en topologi innehåller logik för bearbetning och länkar mellan noder ange dataflöde. hello noder tooinject indata till hello topologi kallas kanaler, vilket kan vara används toosequence hello data. hello indata kan finnas i filen loggar, transaktionella databas, system-prestandaräknaren etc. hello noder med både inkommande och utgående dataflöden kallas bultar, vilket hello faktiska data filtrering och val och aggregering.

Tjänstanslutningspunkten stöder bästa för på-minst en gång och exakt-behandling av data en gång. I ett distribuerat strömmande bearbetning-program kan olika fel inträffa under bearbetning av data, till exempel nätverksavbrott, maskinvarufel eller användarfel kod osv. På-minst en gång process säkerställer att alla data bearbetas minst en gång genom att spela upp automatiskt hello samma data när fel inträffar. På-minst en gång bearbetning är enkel och tillförlitlig och passar bra i många program. Men när programmet hello kräver exakt inventering, räcker till exempel på-minst en gång bearbetning inte eftersom hello samma data kan potentiellt spelas upp i hello programmet topologi. I så fall, exakt-när bearbetningen är utformat toomake att hello resultatet är korrekt när hello data kan spelas och behandlas flera gånger.

SCP ger .NET-utvecklare toodevelop realtid processen program utnyttjar hello Java Virtual Machine (JVM) baserat Storm under hello skydd. hello .NET och JVM kommunicerar via TCP lokal socket. I praktiken är varje kanal/bult paret .net/Java processen där hello användaren logik körs i .net-process som ett plugin-program.

toobuild en databearbetningen program ovanpå SCP, flera steg behövs:

* Utforma och implementera hello kanaler toopull i data från kön.
* Utforma och implementera bultar tooprocess hello indata och spara tooexternal datalager, till exempel databasen.
* Utforma hello topologi, skicka och kör hello-topologi. hello topologi definierar brytpunkter och hello data flödar mellan hello noder. SCP tar hello topologi-specifikationen och distribuera den på ett Storm-kluster där varje vertex körs på en logisk nod. hello redundans och skalning kommer typen av hello Storm-Schemaläggaren.

Det här dokumentet kommer att använda några enkla exempel toowalk igenom hur toobuild databearbetning program med SCP.

## <a name="scp-plugin-interface"></a>SCP-Plugin-gränssnittet
SCP-plugin-program (eller program) är fristående EXEs som kan både körs i Visual Studio under hello utvecklingsfasen och anslutas till hello Storm pipeline efter distributionen i produktion. Skriva hello SCP-plugin-programmet är bara hello samma som att skriva andra standard Windows konsolprogram. SCP.NET plattform deklarerar vissa gränssnitt för kanal-/ bult och hello användarkod plugin-programmet ska implementera dessa gränssnitt. hello Huvudsyftet med den här designen är hello användaren kan fokusera på sina egna business logics och låta andra saker toobe som hanteras av SCP.NET plattform.

hello användarkod plugin-programmet ska implementera ett av hello följande gränssnitt, beror på om hello topologin är transaktionell eller icke-transaktionell och om hello komponenten är kanal eller en bult.

* ISCPSpout
* ISCPBolt
* ISCPTxSpout
* ISCPBatchBolt

### <a name="iscpplugin"></a>ISCPPlugin
ISCPPlugin är hello gemensamt gränssnitt för alla typer av plugin-program. Det är för närvarande ett dummy gränssnitt.

    public interface ISCPPlugin 
    {
    }

### <a name="iscpspout"></a>ISCPSpout
ISCPSpout är hello gränssnitt för icke-transaktionell kanal.

     public interface ISCPSpout : ISCPPlugin                    
     {
         void NextTuple(Dictionary<string, Object> parms);         
         void Ack(long seqId, Dictionary<string, Object> parms);   
         void Fail(long seqId, Dictionary<string, Object> parms);  
     }

När `NextTuple()` anropas, hello C\# användarkod kan generera en eller flera tupplar. Om det inte finns något tooemit, som den här metoden ska returnera utan avger något. Det bör noteras som `NextTuple()`, `Ack()`, och `Fail()` kallas i en tät loop i en enskild tråd i C\# process. När det finns inga tupplar tooemit, är Välkommen företagspolicy toohave NextTuple strömsparläge för kort tid (till exempel 10 millisekunder) som inte toowaste för mycket CPU.

`Ack()`och `Fail()` ska bara anropas om ack mekanism är aktiverat i spec filen. Hej `seqId` är används tooidentify hello tuppel som ADE eller misslyckades. Så om ack är aktiverad i icke-transaktionell topologi ska hello följande begär funktionen användas i kanal:

    public abstract void Emit(string streamId, List<object> values, long seqId); 

Om ack inte stöds i icke-transaktionell topologi, hello `Ack()` och `Fail()` kan lämnas tomt funktion.

Hej `parms` indataparametrar i dessa funktioner är bara tom ordlista, de är reserverad för framtida användning.

### <a name="iscpbolt"></a>ISCPBolt
ISCPBolt är hello gränssnitt för icke-transaktionell bulten.

    public interface ISCPBolt : ISCPPlugin 
    {
    void Execute(SCPTuple tuple);           
    }

När det finns nya tuppel hello `Execute()` funktionen anropas tooprocess den.

### <a name="iscptxspout"></a>ISCPTxSpout
ISCPTxSpout är hello gränssnitt för transaktionell kanal.

    public interface ISCPTxSpout : ISCPPlugin
    {
        void NextTx(out long seqId, Dictionary<string, Object> parms);  
        void Ack(long seqId, Dictionary<string, Object> parms);         
        void Fail(long seqId, Dictionary<string, Object> parms);        
    }

Precis som i sin icke-transaktionell motparterna del `NextTx()`, `Ack()`, och `Fail()` kallas i en tät loop i en enskild tråd i C\# process. När det finns inga data tooemit, är det Välkommen företagspolicy toohave `NextTx` viloläge under en kort tidsperiod (10 millisekunder) som inte toowaste för mycket CPU.

`NextTx()`kallas toostart en ny transaktion hello out-parameter `seqId` är används tooidentify hello transaktion, som också används i `Ack()` och `Fail()`. I `NextTx()`, användaren kan generera data tooJava sida. hello data lagras i ZooKeeper toosupport repetitionsattacker. Eftersom hello kapacitet ZooKeeper är mycket begränsad bör endast användare genererar metadata, inte stora mängder data i transaktionella kanal.

Storm ska spelas upp en transaktion automatiskt om den misslyckas så `Fail()` ska inte anropas i vanliga fall. Men om SCP kan kontrollera hello metadata som sänds av transaktionella kanal, kan det anropa `Fail()` när hello metadata är ogiltiga.

Hej `parms` indataparametrar i dessa funktioner är bara tom ordlista, de är reserverad för framtida användning.

### <a name="iscpbatchbolt"></a>ISCPBatchBolt
ISCPBatchBolt är hello gränssnitt för transaktionell bulten.

    public interface ISCPBatchBolt : ISCPPlugin           
    {
        void Execute(SCPTuple tuple);
        void FinishBatch(Dictionary<string, Object> parms);  
    }

`Execute()`anropas när det finns nya tuppel anländer till hello bulten. `FinishBatch()`anropas när transaktionen avslutas. Hej `parms` indataparameter är reserverad för framtida användning.

För transaktionell topologi, är ett viktigt begrepp – `StormTxAttempt`. Den har två fält `TxId` och `AttemptId`. `TxId`är används tooidentify en särskild transaktion och för en given transaktion, det kan finnas flera försök om hello transaktionen misslyckas och spelas. SCP.NET kommer nya en annan ISCPBatchBolt objektet tooprocess varje `StormTxAttempt`, precis som vilken vill Storm Java-sida. hello syftet med den här designen är toosupport parallella transaktioner bearbetning. Användaren bör Tänk det som om transaktionen försök är klar, kommer att raderas hello motsvarande ISCPBatchBolt objekt och skräpinsamlats.

## <a name="object-model"></a>Objektmodell
SCP.NET innehåller också en enkel uppsättning objekt nycklar för utvecklare tooprogram med. De är **kontexten**, **StateStore**, och **SCPRuntime**. De kommer att diskuteras i hello rest-delen av det här avsnittet.

### <a name="context"></a>Kontext
Kontexten innehåller ett program som körs miljö toohello. Varje ISCPPlugin-instans (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) har en motsvarande kontext-instans. hello-funktionalitet som tillhandahålls av sammanhanget kan delas in i två delar: (1) hello statiska del som är tillgängliga i hello hela C\# bearbeta, (2) hello dynamiska del som endast är tillgänglig för hello specifika kontexten instans.

### <a name="static-part"></a>Statiska del
    public static ILogger Logger = null;
    public static SCPPluginType pluginType;                      
    public static Config Config { get; set; }                    
    public static TopologyContext TopologyContext { get; set; }  

`Logger`finns i loggen syfte.

`pluginType`är används tooindicate hello plugin-programmet för typ av hello C\# process. Om hello C\# processen körs i lokala testläge (utan Java), hello plugin-programmet är `SCP_NET_LOCAL`.

    public enum SCPPluginType 
    {
        SCP_NET_LOCAL = 0,       
        SCP_NET_SPOUT = 1,       
        SCP_NET_BOLT = 2,        
        SCP_NET_TX_SPOUT = 3,   
        SCP_NET_BATCH_BOLT = 4  
    }

`Config`tillhandahålls tooget konfigurationsparametrar från Java-sida. hello parametrarna som skickas från Java sida när C\# plugin-programmet har initierats. Hej `Config` parametrar är uppdelat i två delar: `stormConf` och `pluginConf`.

    public Dictionary<string, Object> stormConf { get; set; }  
    public Dictionary<string, Object> pluginConf { get; set; }  

`stormConf`är parametrar som definierats av Storm och `pluginConf` är hello parametrar som definierats av SCP. Exempel:

    public class Constants
    {
        … …

        // constant string for pluginConf
        public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

        // constant string for stormConf
        public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";           
        public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";                 
    }

`TopologyContext`tillhandahålls tooget hello topologi kontext är den mest användbart för komponenter med flera parallellitet. Här är ett exempel:

    //demo how tooget TopologyContext info
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)                      
    {
        Context.Logger.Info("TopologyContext info:");
        TopologyContext topologyContext = Context.TopologyContext;                    
        Context.Logger.Info("taskId: {0}", topologyContext.GetThisTaskId());          
        taskIndex = topologyContext.GetThisTaskIndex();
        Context.Logger.Info("taskIndex: {0}", taskIndex);
        string componentId = topologyContext.GetThisComponentId();                    
        Context.Logger.Info("componentId: {0}", componentId);
        List<int> componentTasks = topologyContext.GetComponentTasks(componentId);  
        Context.Logger.Info("taskNum: {0}", componentTasks.Count);                    
    }

### <a name="dynamic-part"></a>Dynamiska del
Hej följande gränssnitt är relevanta tooa vissa kontextinstansen. Hej kontextinstansen skapas av SCP.NET plattform och skickades toohello användarkod:

    // Declare hello Output and Input Stream Schemas

    public void DeclareComponentSchema(ComponentStreamSchema schema);   

    // Emit tuple toodefault stream.
    public abstract void Emit(List<object> values);                   

    // Emit tuple toohello specific stream.
    public abstract void Emit(string streamId, List<object> values);  

För icke-transaktionell kanal som stöder ack tillhandahålls hello följande metod:

    // for non-transactional Spout which supports ack
    public abstract void Emit(string streamId, List<object> values, long seqId);  

För icke-transaktionell bult stöder ack, bör det explicit `Ack()` eller `Fail()` hello tuppel togs emot. Och när sändning nya tuppel, det måste även ange hello ankare för hello nya tuppel. hello följande metoder som tillhandahålls.

    public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values); 
    public abstract void Ack(SCPTuple tuple);
    public abstract void Fail(SCPTuple tuple);

### <a name="statestore"></a>StateStore
`StateStore`innehåller metadatatjänster, monotonisk sekvensgenerering och vänta utan samordning. Abstraktioner på högre nivåer distribuerade samtidighet kan baseras på `StateStore`, inklusive distribuerade lås, distribuerade köer, barriärer och Transaktionstjänster.

SCP-program kan använda hello `State` objekt toopersist information i ZooKeeper, särskilt för transaktionell topologi. Om du gör det transaktionella kanal kraschar och startar om, kan den hämta hello nödvändig information från ZooKeeper och starta om hello pipeline.

Hej `StateStore` -objektet har huvudsakligen dessa metoder:

    /// <summary>
    /// Static method tooretrieve a state store of hello given path and connStr 
    /// </summary>
    /// <param name="storePath">StateStore Path</param>
    /// <param name="connStr">StateStore Address</param>
    /// <returns>Instance of StateStore</returns>
    public static StateStore Get(string storePath, string connStr);

    /// <summary>
    /// Create a new state object in this state store instance
    /// </summary>
    /// <returns>State from StateStore</returns>
    public State Create();

    /// <summary>
    /// Retrieve all states that were previously uncommitted, excluding all aborted states 
    /// </summary>
    /// <returns>Uncommited States</returns>
    public IEnumerable<State> GetUnCommitted();

    /// <summary>
    /// Get all hello States in hello StateStore
    /// </summary>
    /// <returns>All hello States</returns>
    public IEnumerable<State> States();

    /// <summary>
    /// Get state or registry object
    /// </summary>
    /// <param name="info">Registry Name(Registry only)</param>
    /// <typeparam name="T">Type, Registry or State</typeparam>
    /// <returns>Return Registry or State</returns>
    public T Get<T>(string info = null);

    /// <summary>
    /// List all hello committed states
    /// </summary>
    /// <returns>Registries contain hello Committed State </returns> 
    public IEnumerable<Registry> Commited();

    /// <summary>
    /// List all hello Aborted State in hello StateStore
    /// </summary>
    /// <returns>Registries contain hello Aborted State</returns>
    public IEnumerable<Registry> Aborted();

    /// <summary>
    /// Retrieve an existing state object from this state store instance 
    /// </summary>
    /// <returns>State from StateStore</returns>
    /// <typeparam name="T">stateId, id of hello State</typeparam>
    public State GetState(long stateId)

Hej `State` -objektet har huvudsakligen dessa metoder:

    /// <summary>
    /// Set hello status of hello state object toocommit 
    /// </summary>
    public void Commit(bool simpleMode = true); 

    /// <summary>
    /// Set hello status of hello state object tooabort 
    /// </summary>
    public void Abort();

    /// <summary>
    /// Put an attribute value under hello give key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <param name="attribute">State Attribute</param> 
    public void PutAttribute<T>(string key, T attribute); 

    /// <summary>
    /// Get hello attribute value associated with hello given key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <returns>State Attribute</returns>               
    public T GetAttribute<T>(string key);                    

För hello `Commit()` metod när simpleMode anges tootrue, helt enkelt bort hello motsvarande ZNode i ZooKeeper. I annat fall raderas hello aktuella ZNode och lägga till en ny nod i hello GENOMFÖRD\_sökväg.

### <a name="scpruntime"></a>SCPRuntime
SCPRuntime ger hello följande två metoder.

    public static void Initialize();

    public static void LaunchPlugin(newSCPPlugin createDelegate);  

`Initialize()`är används tooinitialize hello SCP-körningsmiljön. I den här metoden hello C\# processen ansluter toohello Java-sida, och hämtar konfigurationsparametrar och topologi-kontexten.

`LaunchPlugin()`använda tookick av hello-meddelande bearbetar loop. I den här loop hello C\# plugin-program får meddelanden formuläret Java-sida (inklusive tupplar och kontroll signaler) och sedan processen hälsningsmeddelande kanske anropar hello gränssnittsmetod ge genom en användarkod hello. hello Indataparametern för metoden `LaunchPlugin()` är en delegat som kan returnera ett objekt som implementerar ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt-gränssnittet.

    public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary\<string, Object\> parms); 

För ISCPBatchBolt, kan vi hämta `StormTxAttempt` från `parms`, och använda den toojudge om det är ett uppspelat försök. Detta görs vanligtvis på hello commit bult och det visas i hello `HelloWorldTx` exempel.

Generellt sett kan hello SCP-plugin-program köras i två lägen här:

1. Lokala testläge: I det här läget hello SCP-plugin-program (hello C\# användarkod) körs i Visual Studio under hello utvecklingsfasen. `LocalContext`kan användas i det här läget, vilket ger metoden tooserialize hello orsakat tupplar toolocal filer och läsa dem tillbaka toomemory.
   
        public interface ILocalContext
        {
            List\<SCPTuple\> RecvFromMsgQueue();
            void WriteMsgQueueToFile(string filepath, bool append = false);  
            void ReadFromFileToMsgQueue(string filepath);                    
        }
2. Standardläget: I det här läget hello SCP-plugin-program startas av storm java-process.
   
    Här är ett exempel på Starta SCP-plugin-programmet:
   
        namespace Scp.App.HelloWorld
        {
        public class Generator : ISCPSpout
        {
            … …
            public static Generator Get(Context ctx, Dictionary<string, Object> parms)
            {
            return new Generator(ctx);
            }
        }
   
        class HelloWorld
        {
            static void Main(string[] args)
            {
            /* Setting hello environment variable here can change hello log file name */
            System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "HelloWorld");
   
            SCPRuntime.Initialize();
            SCPRuntime.LaunchPlugin(new newSCPPlugin(Generator.Get));
            }
        }
        }

## <a name="topology-specification-language"></a>Topologi specifikationsspråk
SCP är-topologi ett specifikt språk domän för att beskriva och konfigurera SCP topologier. Den är baserad på Storm's Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) och utökas med SCP.

Topologi specifikationer som kan skickas direkt toostorm kluster för körning via hello ***runspec*** kommando.

SCP.NET har Lägg till följande funktioner toodefine hello transaktionella topologi:

| **Nya funktioner** | **Parametrar** | **Beskrivning** |
| --- | --- | --- |
| **TX topolopy** |topologi namn<br />kanal-karta<br />bult-karta |Definiera en transaktionell topologi med hello topologi namnet &nbsp;spouts definitionen mappning och hello bultar definitionen mappning |
| **SCP-tx-kanal** |Exec-namn<br />argument<br />Fält |Definiera en transaktionell kanal. Det körs programmet hello med ***exec-name*** med ***argument***.<br /><br />Hej ***fält*** är hello utdatafält för kanal |
| **SCP-tx-batch-bult** |Exec-namn<br />argument<br />Fält |Definiera en transaktionell Batch bulten. Det körs programmet hello med ***exec-name*** med ***argument.***<br /><br />hello är fält hello utdatafält för bulten. |
| **SCP-tx-commit-bult** |Exec-namn<br />argument<br />Fält |Definiera en transaktionell Committer bulten. Det körs programmet hello med ***exec-name*** med ***argument***.<br /><br />Hej ***fält*** är hello utdatafält för bult |
| **nontx topolopy** |topologi namn<br />kanal-karta<br />bult-karta |Definiera en icke-transaktionell topologi med hello topologi namnet&nbsp; spouts definitionen mappning och hello bultar definitionen mappning |
| **SCP-kanal** |Exec-namn<br />argument<br />Fält<br />parameters |Definiera en icke-transaktionell kanal. Det körs programmet hello med ***exec-name*** med ***argument***.<br /><br />Hej ***fält*** är hello utdatafält för kanal<br /><br />Hej ***parametrar*** är valfritt, använder den toospecify vissa parametrar, till exempel ”nontransactional.ack.enabled”. |
| **SCP-bult** |Exec-namn<br />argument<br />Fält<br />parameters |Definiera en icke-transaktionell bulten. Det körs programmet hello med ***exec-name*** med ***argument***.<br /><br />Hej ***fält*** är hello utdatafält för bult<br /><br />Hej ***parametrar*** är valfritt, använder den toospecify vissa parametrar, till exempel ”nontransactional.ack.enabled”. |

SCP.NET har Följ nycklar ord som definierats:

| **Nyckelord** | **Beskrivning** |
| --- | --- |
| **: namn** |Definiera hello topologi namn |
| **: topologi** |Definiera hello topologi med hello ovan funktioner och skapa i viktiga. |
| **: p** |Definiera hello parallellitet tips för varje kanal eller en bult. |
| **: config** |Definiera konfigurera parametern eller uppdatera hello befintliga |
| **: schemat** |Definiera hello schemat för dataströmmen. |

Och vanliga parametrar:

| **Parametern** | **Beskrivning** |
| --- | --- |
| **”plugin.name”** |namnet på hello C#-plugin-programmet exe-filen |
| **”plugin.args”** |plugin-argument |
| **”output.schema”** |Utdataschemat |
| **”nontransactional.ack.enabled”** |Om ack har aktiverats för icke-transaktionell topologi |

hello runspec kommandot kommer att distribueras tillsammans med hello bits, hello användning är t.ex.:

    .\bin\runSpec.cmd
    usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
    ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target

Hej ***resurs dir*** parametern är valfri, måste du toospecify den när du vill tooplug en C\# programmet och den här katalogen innehåller hello programmet hello beroenden och konfigurationer.

Hej ***klassökvägen*** parametern också är valfri. Det är används toospecify hello Java klassökvägen om spec hello-filen innehåller Java-kanal eller en bult.

## <a name="miscellaneous-features"></a>Diverse funktioner
### <a name="input-and-output-schema-declaration"></a>Indata och utdata Schema-deklaration
hello användaren kan generera tuppel i C\# bearbeta, hello plattform måste tooserialize hello tuppel i byte [], överföring tooJava sida och Storm överför det här tuppeln toohello mål. Under tiden hello C i underordnade komponent\# processen kommer ta emot tuppel tillbaka från java sida och konvertera toohello ursprungliga typerna av plattform, dessa åtgärder är dolt enligt hello plattform.

toosupport hello serialisering och deserialisering måste användarkod toodeclare hello schemat för hello indata och utdata.

hello i/o-dataströmmen schemat är definierad som en ordlista hello nyckeln är hello StreamId och hello är hello typer av hello kolumner. hello komponent kan ha flera strömmar som deklarerats.

    public class ComponentStreamSchema
    {
        public Dictionary<string, List<Type>> InputStreamSchema { get; set; }
        public Dictionary<string, List<Type>> OutputStreamSchema { get; set; }
        public ComponentStreamSchema(Dictionary<string, List<Type>> input, Dictionary<string, List<Type>> output)
        {
            InputStreamSchema = input;
            OutputStreamSchema = output;
        }
    }


Vi har hello följande API lagts till i Context-objektet:

    public void DeclareComponentSchema(ComponentStreamSchema schema)

Användarkod måste kontrollera hello tupplar som orsakat lyder under hello-schema som definierats för dataströmmen eller hello system genereras ett undantagsfel för körning.

### <a name="multi-stream-support"></a>Stöd för flera dataström
SCP stöder användaren code tooemit eller ta emot från flera olika dataströmmar på hello samma tid. stöd för hello återspeglar i hello Context-objektet som hello begär metoden tar en valfri dataströmmen ID-parametern.

Två metoder i hello SCP.NET Context-objektet har lagts till. De är används tooemit tuppeln eller Tupplar toospecify StreamId. Hej StreamId är en sträng och den måste toobe konsekvent i båda C\# och hello topologi Definition-specifikationen.

        /* Emit tuple toohello specific stream. */
        public abstract void Emit(string streamId, List<object> values);

        /* for non-transactional Spout only */
        public abstract void Emit(string streamId, List<object> values, long seqId);

hello ljusavgivande tooa obefintligt dataströmmen kommer runtime-undantag.

### <a name="fields-grouping"></a>Fält gruppering
hello fungerar inbyggda fält gruppering i Strom inte korrekt i SCP.NET. Hello fält gruppering används hello byte [] objekt hash-kod tooperform hello gruppering i hello Java Proxy sida, alla datatyper i hello-fält är faktiskt byte []. hello byte [] hash-kod är hello-adressen för det här objektet i minnet. Så hello gruppering är fel för två byte objekt [] hello att dela samma innehåll men inte hello samma adress.

SCP.NET lägger till en metod för anpassad gruppering och använder hello innehållet i hello byte [] toodo hello gruppering. I **SPEC** filen hello syntax är t.ex.:

    (bolt-spec
        {
            "spout_test" (scp-field-group :non-tx [0,1])
        }
        …
    )


Här

1. ”scp-fältet-grupp”: ”anpassad fältet gruppering implementeras av SCP”.
2. ”: tx” eller ”: icke-tx” innebär att om det är transaktionell topologi. Vi behöver informationen eftersom hello startar index skiljer sig åt i tx kontra-tx topologier.
3. [0,1] innebär en hashset av fältet ID, början på 0.

### <a name="hybrid-topology"></a>Hybrid-topologi
hello intern Storm är skriven i Java. Och SCP.Net har förbättrad den tooenable våra tull toowrite C\# code toohandle sina affärslogik. Men vi stöder också hybridtopologier, som innehåller inte bara C\# kanaler/bultar men Java kanal/bultar.

### <a name="specify-java-spoutbolt-in-spec-file"></a>Ange kanal/bult för Java i spec fil
”Scp-kanal” och ”scp-bult” kan också vara används toospecify Java Spouts och bultar i spec fil, här är ett exempel:

    (spout-spec 
      (microsoft.scp.example.HybridTopology.Generator.)           
      :p 1)

Här `microsoft.scp.example.HybridTopology.Generator` är hello namnet på hello prata Java-klass.

### <a name="specify-java-classpath-in-runspec-command"></a>Ange Java-klassökvägen i runSpec kommando
Om du vill toosubmit topologi som innehåller Java Spouts eller bultar behöver toofirst kompilera hello Java Spouts eller bultar och hämta hello Jar-filer. Du bör ange hello java-klassökvägen som innehåller hello Jar-filer när du skickar in topologin. Här är ett exempel:

    bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*

Här **exempel\\HybridTopology\\java\\mål\\**  är hello mappen som innehåller hello Java kanal/bult Jar-fil.

### <a name="serialization-and-deserialization-between-java-and-c"></a>Serialisering och deserialisering mellan Java och C\
Vår SCP-komponenten innehåller Java sida och C\# sida. I ordning toointeract med inbyggda Java kanaler/bultar serialisering/deserialisering utföras mellan Java sida och C\# sida, enligt beskrivningen i följande diagram hello.

![diagram över java-komponent som skickar tooSCP komponenten skickar tooJava komponent](media/hdinsight-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

1. **Serialisering i Java-sida och deserialisering i C\# sida**
   
   Första vi tillhandahåller standardimplementering för serialisering i Java-sida och deserialisering i C\# sida. Hej serialiseringsmetod i Java-sida kan anges i SPEC filen:
   
       (scp-bolt
           {
               "plugin.name" "HybridTopology.exe"
               "plugin.args" ["displayer"]
               "output.schema" {}
               "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
           })
   
   Hej deserialisering metod i C\# sida måste anges i C\# användarkod:
   
       Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
       inputSchema.Add("default", new List<Type>() { typeof(Person) });
       this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
       this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());            
   
   Den här standardimplementering ska hantera de flesta fall om hello datatypen inte är för komplex. I vissa fall, antingen eftersom hello användardatatypen är för komplex eller för att hello prestanda för våra standardimplementering inte uppfyller hello användarens kravet användaren kan plugin-programmet sina egna implementering.
   
   hello serialisera gränssnittet i java sida definieras som:
   
       public interface ICustomizedInteropJavaSerializer {
           public void prepare(String[] args);
           public List<ByteBuffer> serialize(List<Object> objectList);
       }
   
   hello deserialisera gränssnitt i C\# sida definieras som:
   
   offentliga gränssnittet ICustomizedInteropCSharpDeserializer
   
       public interface ICustomizedInteropCSharpDeserializer
       {
           List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
       }
2. **Serialisering i C\# sida och deserialisering i Java sida sida**
   
   Hej serialiseringsmetod i C\# sida måste anges i C\# användarkod:
   
       this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
   
   hello deserialisering metod i Java-sida måste anges i SPEC fil:
   
     (scp-kanal
   
       {
         "plugin.name" "HybridTopology.exe"
         "plugin.args" ["generator"]
         "output.schema" {"default" ["person"]}
         "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
       })
   
   Är här ”microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer” hello namnet på funktionen för avserialisering och ”microsoft.scp.example.HybridTopology.Person” är hello måldata klassen hello avserialiseras till.
   
   Användaren kan också plugin-programmet sina egna implementering av C\# serialiseraren och Java funktionen för avserialisering. Detta är hello gränssnitt för C\# serialiseraren:
   
       public interface ICustomizedInteropCSharpSerializer
       {
           List<byte[]> Serialize(List<object> dataList);
       }
   
   Detta är hello gränssnitt för Java funktionen för avserialisering:
   
       public interface ICustomizedInteropJavaDeserializer {
           public void prepare(String[] targetClassNames);
           public List<Object> Deserialize(List<ByteBuffer> dataList);
       }

## <a name="scp-host-mode"></a>Värden för SCP-läge
I det här läget kan användaren kompilera sina koder tooDLL och använda SCPHost.exe som tillhandahålls av SCP toosubmit topologi. hello spec filen ser ut så här:

    (scp-spout
      {
        "plugin.name" "SCPHost.exe"
        "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
        "output.schema" {"default" ["sentence"]}
      })

Här, `plugin.name` har angetts som `SCPHost.exe` som tillhandahålls av SCP SDK. SCPHost.exe som accepterar exakt tre parametrar:

1. hello först en är hello DLL-namn som är `"HelloWorld.dll"` i det här exemplet.
2. hello andra är hello klassnamnet, som är `"Scp.App.HelloWorld.Generator"` i det här exemplet.
3. hello tredje är en hello namnet på en offentlig statisk metod som kan vara anropade tooget en instans av ISCPPlugin.

Användarkod i värden läge är kompilerad som DLL-filen och anropas av SCP-plattformen. SCP-plattformen kan så få fullständig kontroll över hello hela standardbearbetningslogiken. Så vi rekommenderar våra kunder toosubmit topologi i SCP värden läge eftersom den kan förenkla hello utveckling och sätta oss större flexibilitet och bättre bakåtkompatibilitet för samt senare version.

## <a name="scp-programming-examples"></a>Exempel för SCP-programmering
### <a name="helloworld"></a>HelloWorld
**HelloWorld** är ett väldigt enkelt exempel tooshow uppleva SCP.Net. Den använder en icke-transaktionell topologi med en kanal som kallas **generator**, och två bultar kallas **delningslisten** och **räknaren**. hello kanal **generator** att slumpmässigt generera vissa meningar och generera dessa meningar för**delningslisten**. hello bult **delningslisten** ska dela hello meningar toowords och generera dessa ord för**räknaren** bulten. räknaren ”hello bult” används en ordlista toorecord hello förekomstantal varje ord.

Det finns två spec filer **HelloWorld.spec** och **HelloWorld\_EnableAck.spec** för det här exemplet. I hello C\# kod, det kan ta reda på om ack aktiveras genom att hämta hello pluginConf från Java-sida.

    /* demo how tooget pluginConf info */
    if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
    {
        enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
    }
    Context.Logger.Info("enableAck: {0}", enableAck);

I hello kanal är en ordlista används toocache hello tupplar som inte är ADE om ack är aktiverad. Om Fail() anropas ska hello misslyckade tuppel återupprepas:

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        Context.Logger.Info("Fail, seqId: {0}", seqId);
        if (cachedTuples.ContainsKey(seqId))
        {
            /* get hello cached tuple */
            string sentence = cachedTuples[seqId];

            /* replay hello failed tuple */
            Context.Logger.Info("Re-Emit: {0}, seqId: {1}", sentence, seqId);
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), seqId);
        }
        else
        {
            Context.Logger.Warn("Fail(), can't find cached tuple for seqId {0}!", seqId);
        }
    }

### <a name="helloworldtx"></a>HelloWorldTx
Hej **HelloWorldTx** exemplet visar hur tooimplement transaktionella topologi. Den har en kanal som kallas **generator**en batch bultar kallas **partiell antal**, och en bult commit anropas **summan antal**. Det finns tre förskapade txt-filer: **DataSource0.txt**, **DataSource1.txt** och **DataSource2.txt**.

I varje transaktion, hello kanal **generator** slumpmässigt väljer två filer från hello förskapade tre filer, och genererar hello två filen namn toohello **partiell antal** bulten. hello bult **partiell antal** först hämta hello filnamnet från hello emot tuppel och sedan öppna hello fil- och antal hello antalet ord i den här filen och slutligen genererar hello word nummer toohello **count-summan**bulten. Hej **antal summan** bult sammanfattas hello totala antalet.

tooachieve **exakt en gång** semantik, hello commit bult **summan antal** behöver toojudge oavsett om det är en upprepat transaktion. I det här exemplet har den en statisk medlemsvariabel:

    public static long lastCommittedTxId = -1; 

När en instans av ISCPBatchBolt skapas får hello `txAttempt` från indataparametrar:

    public static CountSum Get(Context ctx, Dictionary<string, Object> parms)
    {
        /* for transactional topology, we can get txAttempt from hello input parms */
        if (parms.ContainsKey(Constants.STORM_TX_ATTEMPT))
        {
            StormTxAttempt txAttempt = (StormTxAttempt)parms[Constants.STORM_TX_ATTEMPT];
            return new CountSum(ctx, txAttempt);
        }
        else
        {
            throw new Exception("null txAttempt");
        }
    }

När `FinishBatch()` anropas, hello `lastCommittedTxId` kommer att uppdateringen om den inte är en upprepat transaktion.

    public void FinishBatch(Dictionary<string, Object> parms)
    {
        /* judge whether it is a replayed transaction? */
        bool replay = (this.txAttempt.TxId <= lastCommittedTxId);

        if (!replay)
        {
            /* If it is not replayed, update hello toalCount and lastCommittedTxId vaule */
            totalCount = totalCount + this.count;
            lastCommittedTxId = this.txAttempt.TxId;
        }
        … …
    }


### <a name="hybridtopology"></a>HybridTopology
Den här topologin innehåller en Java-kanalen och en C\# bulten. Den använder hello serialisering och deserialisering standardimplementering tillhandahålls av SCP-plattformen. Ange ref hello **HybridTopology.spec** i **exempel\\HybridTopology** mapp för hello spec filinformation, och **SubmitTopology.bat** för toospecify Java-klassökvägen.

### <a name="scphostdemo"></a>SCPHostDemo
Det här exemplet är i princip hello samma som HelloWorld. hello endast skillnaden är att hello användarkod har kompilerats som DLL-filen och hello topologi skickas med hjälp av SCPHost.exe. Ta ref hello avsnittet ”SCP värden läget” mer detaljerad förklaring.

## <a name="next-steps"></a>Nästa steg
Exempel på Storm-topologier som skapats med SCP finns hello följande:

* [Utveckla C#-topologier för Apache Storm på HDInsight med Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [Bearbeta händelser från Azure Event Hubs med Storm på HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md)
* [Skapa flera dataströmmar i en C# Storm-topologi](hdinsight-storm-twitter-trending.md)
* [Använd Power Bi toovisualize data från en Storm-topologi](hdinsight-storm-power-bi-topology.md)
* [Bearbeta vehicle sensordata från Händelsehubbar med Storm på HDInsight](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [Extrahering, transformering och inläsning (ETL) från Azure Event Hubs tooHBase](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
* [Samordna händelser med Storm och HBase på HDInsight](hdinsight-storm-correlation-topology.md)

