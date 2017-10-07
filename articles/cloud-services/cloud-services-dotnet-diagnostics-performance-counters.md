---
title: "aaaUse prestandaräknare i Azure-diagnostik | Microsoft Docs"
description: "Använd prestandaräknare i Azure-molntjänster eller virtuella toofind flaskhalsar och justera prestanda."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: fc4c61e9-d49d-4ed9-a32c-b91cdf851909
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/29/2016
ms.author: robb
ms.openlocfilehash: f3250816c01fc6e164a6aae48da5035845e6d2b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Skapa och använda prestandaräknare i ett Azure-program
Den här artikeln beskriver hello fördelarna och hur tooput prestandaräknare i Azure-program. Du kan använda dem toocollect data, hitta flaskhalsar och finjustera system- och programprestanda.

Prestandaräknare som är tillgängliga för Windows Server, IIS och ASP.NET kan också samlas in och användas toodetermine hello hälsan hos ditt Azure webbroller, arbetsroller och virtuella datorer. Du kan också skapa och använda anpassade prestandaräknare.  

Du kan undersöka prestandaräknardata

1. Direkt på hello programvärd med hello Performance Monitor-verktyget som nås med hjälp av fjärrskrivbord
2. Med System Center Operations Manager med hjälp av hello Azure Management Pack
3. Med andra övervakningsverktyg som har åtkomst till hello överförs diagnostikdata tooAzure lagring. Se [Store och visa diagnostiska Data i Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) för mer information.  

Mer information om hur du övervakar hello prestanda i hello [Azure-portalen](http://portal.azure.com/), se [hur tooMonitor Cloud Services](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Mer detaljerad information om att skapa en loggning och spårning strategi och använder diagnostik- och andra tekniker tootroubleshoot problem och optimera Azure-program, se [felsökning bästa praxis för att utveckla Azure Program](https://msdn.microsoft.com/library/azure/hh771389.aspx).

## <a name="enable-performance-counter-monitoring"></a>Aktivera övervakning av räknare
Prestandaräknare är inte aktiverade som standard. Ditt program eller en startåtgärd måste ändra hello standard diagnostik agent configuration tooinclude hello specifika prestandaräknare som du vill toomonitor för varje rollinstans.

### <a name="performance-counters-available-for-microsoft-azure"></a>Prestandaräknare för Microsoft Azure
Azure tillhandahåller en delmängd av hello prestandaräknare som är tillgängliga för Windows Server, IIS och hello ASP.NET-stacken. hello följande tabell visas några av hello prestandaräknarna särskilt intressanta för Azure-program.

| : Räknaren Kategoriobjektet (instans) | Räknarens namn | Referens |
| --- | --- | --- |
| .NET CLR-undantag (*globala*) |# Undantag som utlöses / sek |Prestandaräknare för undantag |
| .NET CLR-minne (*globala*) |Tid i GC i procent |Prestandaräknare för minnet |
| ASP.NET |Programmet startas om |Prestandaräknare för ASP.NET |
| ASP.NET |Körningstid för begäran |Prestandaräknare för ASP.NET |
| ASP.NET |Begäranden som kopplats från |Prestandaräknare för ASP.NET |
| ASP.NET |Omstart av arbetsprocess |Prestandaräknare för ASP.NET |
| ASP.NET-program (**totala**) |Totalt antal begäranden |Prestandaräknare för ASP.NET |
| ASP.NET-program (**totala**) |Begäranden per sekund |Prestandaräknare för ASP.NET |
| ASP.NET v4.0.30319 |Körningstid för begäran |Prestandaräknare för ASP.NET |
| ASP.NET v4.0.30319 |Väntetid för begäran |Prestandaräknare för ASP.NET |
| ASP.NET v4.0.30319 |Aktuella begäranden |Prestandaräknare för ASP.NET |
| ASP.NET v4.0.30319 |Begäranden i kö |Prestandaräknare för ASP.NET |
| ASP.NET v4.0.30319 |Begäranden som nekats |Prestandaräknare för ASP.NET |
| Minne |Tillgängliga megabyte |Prestandaräknare för minnet |
| Minne |Dedikerade byte |Prestandaräknare för minnet |
| Processor(_Total) |% Processortid |Prestandaräknare för ASP.NET |
| TCPv4 |Anslutningsfel |TCP-objekt |
| TCPv4 |Anslutningar |TCP-objekt |
| TCPv4 |Återställda anslutningar |TCP-objekt |
| TCPv4 |Segment/s |TCP-objekt |
| Nätverk Interface(*) |Mottagna byte/sek |Objektet för nätverksgränssnittet |
| Nätverk Interface(*) |Skickade byte/sek |Objektet för nätverksgränssnittet |
| Nätverksgränssnittet (Microsoft Virtual Machine Bus nätverk kort _2) |Mottagna byte/sek |Objektet för nätverksgränssnittet |
| Nätverksgränssnittet (Microsoft Virtual Machine Bus nätverk kort _2) |Skickade byte/sek |Objektet för nätverksgränssnittet |
| Nätverksgränssnittet (Microsoft Virtual Machine Bus nätverk kort _2) |Byte totalt/sek |Objektet för nätverksgränssnittet |

## <a name="create-and-add-custom-performance-counters-tooyour-application"></a>Skapa och lägga till anpassade prestandaräknare tooyour program
Azure har stöd för toocreate och ändra anpassade prestandaräknare för webbprogram och arbetsroller. hello räknare kan vara används tootrack och övervaka programspecifika beteende. Du kan skapa och ta bort anpassade prestandaräknarkategorier och specificerare från en startaktivitet, webbroll eller arbetsroll med förhöjd behörighet.

> [!NOTE]
> Kod som ändrar toocustom prestandaräknare har utökade behörigheter toorun. Om hello kod i en webbroll eller arbetsrollen hello rollen måste innehålla hello taggen <Runtime executionContext="elevated" /> i hello ServiceDefinition.csdef fil för hello rollen tooinitialize korrekt.
>
>

Du kan skicka anpassade räknare tooAzure lagringsprestanda med hello diagnostik-agenten.

hello standard prestandaräknardata genereras av hello Azure processer. Anpassade prestandaräknardata måste ha skapats av rollen eller worker-rollen webbprogrammet. Se [prestandaräknaren typer](https://msdn.microsoft.com/library/z573042h.aspx) information om hello datatyper som kan lagras i anpassade prestandaräknare. Se [PerformanceCounters exempel](http://code.msdn.microsoft.com/azure/) ett exempel som skapar och anger anpassade prestandaräknardata i en webbroll.

## <a name="store-and-view-performance-counter-data"></a>Lagra och visa information om prestandaräknare
Azure cachelagrar prestandaräknardata med annan diagnostisk information. Informationen är tillgänglig för fjärranslutna övervakning medan hälsningspaket rollinstansen körs med hjälp av fjärråtkomst till skrivbordet tooview verktyg, till exempel Prestandaövervakaren. toopersist hello utanför hälsningspaket rollinstansen, hello diagnostik agent måste dataöverföring hello tooAzure datalagring. hello storleksgränsen på hello cachelagras prestandaräknardata kan konfigureras i hello diagnostik agent eller också är det konfigurerade toobe en del av en delad gräns för alla hello diagnostikdata. Mer information om hur du anger hello buffertstorlek finns [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) och [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Se [Store och visa diagnostiska Data i Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) en översikt över hur du konfigurerar hello agent tootransfer data tooa diagnostiklagringskonto.

Varje konfigurerade prestandaräknarinstans registreras på en angiven samplingsfrekvensen och hello provtagning data överförs toohello storage-konto genom att en schemalagd begäran eller ett för uppringning på begäran. Automatiska överföringar kan schemaläggas så ofta som en gång per minut. Prestandaräknardata som överförs av hello diagnostik agent lagras i en tabell, WADPerformanceCountersTable, i hello storage-konto. Den här tabellen kan nås och efterfrågas med Azure standardlagring API-metoder. Se [Microsoft Azure prestandaräknarna Sample](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) ett exempel på frågor och visa prestandaräknardata från hello WADPerformanceCountersTable tabell.

> [!NOTE]
> Beroende på hello diagnostik agent överföring frekvens och kön latens kan hello senaste prestandaräknardata i hello storage-konto vara flera minuter för gammal.
>
>

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Aktivera prestandaräknare med diagnostik konfigurationsfilen
Använd följande procedur tooenable prestandaräknare i Azure-programmet hello.

## <a name="prerequisites"></a>Krav
Det här avsnittet förutsätter att du har importerat hello diagnostik övervakaren till ditt program och lagt till hello diagnostik configuration file tooyour Visual Studio-lösning (diagnostics.wadcfg i SDK 2.4 och nedan eller diagnostics.wadcfgx i SDK 2.5 och senare). Se steg 1 och 2 i [aktiverar diagnostik i Azure-molntjänster och virtuella datorer](cloud-services-dotnet-diagnostics.md)) mer information.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Steg 1: Samla in och lagra data från prestandaräknare
När du har lagt till hello diagnostik filen tooyour Visual Studio-lösning kan konfigurera du hello insamling och lagring av prestandaräknardata i ett Azure-program. Detta görs genom att lägga till prestandaräknare toohello diagnostik filen. Diagnostikdata, inklusive prestandaräknare, samlas först på hello-instansen. hello data är beständiga toohello WADPerformanceCountersTable tabell i hello Azure tabelltjänsten, så kommer du att också behöver toospecify hello storage-konto i ditt program. Om du testar programmet lokalt i hello Compute Emulator, kan du också lagra diagnostikdata lokalt i hello Storage-emulatorn. Innan du sparar diagnostikdata, måste du först gå toohello [Azure-portalen](http://portal.azure.com/) och skapa ett klassiska storage-konto. Ett bra tips är toolocate storage-konto i hello samma geografiska plats som din Azure-program. Genom att hålla hello Azure program- och storage-konto finns i hello samma geografiska plats kan du undvika betalar externa bandbreddskostnader och minska svarstiden.

### <a name="add-performance-counters-toohello-diagnostics-file"></a>Lägg till prestandaräknare toohello diagnostik-fil
Det finns många räknare som du kan använda. hello visar följande exempel flera prestandaräknare som rekommenderas för webb- och arbetsprocesser för övervakning av rollen.

Öppna hello diagnostik filen (diagnostics.wadcfg SDK 2.4 och lägre eller diagnostics.wadcfgx i SDK 2.5 och senare) och Lägg till följande toohello DiagnosticMonitorConfiguration element hello:

```xml
<PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
    <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use hello Process(w3wp) category counters in a web role -->

    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use hello Process(WaWorkerHost) category counters in a worker role.
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
</PerformanceCounters>
```

Hej bufferQuotaInMB attribut, som anger hello maximal mängd fillagring för system som är tillgängligt för hello samling datatyp (Azure loggar, IIS-loggar osv.). hello standardvärdet är 0. När hello kvoten har uppnåtts tas hello äldsta data bort när nya data läggs. hello summan av alla hello bufferQuotaInMB egenskaper måste vara större än hello värdet för hello OverallQuotaInMB attribut. En mer detaljerad beskrivning för att avgöra hur mycket lagringsutrymme måste finnas för hello samling diagnostikdata finns hello installationsprogrammet BOMULLSTUSS avsnitt i [felsökning bästa praxis för att utveckla program i Azure](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).

Hej scheduledTransferPeriod attribut, som anger hello intervallet mellan schemalagda dataöverföringar avrundat toohello närmast minuter. I följande exempel hello, är den inställd tooPT30M (30 minuter). Hello överföring period tooa små Inställningsvärdet, till exempel 1 minut, påverkar negativt programmets prestanda i produktion, men kan vara användbart för att visa diagnostiken arbetar snabbt när du testar. hello schemalagda överföring period måste vara tillräckligt litet tooensure diagnostikdata inte är över på hello-instans, men tillräckligt stor för att det inte påverkar hello prestanda för ditt program.

Hej counterSpecifier attributet anger hello prestandaräknaren toocollect. hello sampleRate attribut anger hello hastighet med vilken hello prestandaräknaren ska samlas in, i det här fallet 30 sekunder.

När du har lagt till hello prestandaräknare som du vill toocollect, sparar du ändringarna toohello diagnostik-filen. Sedan måste toospecify hello storage-konto som ska vara beständig hello diagnostikdata.

### <a name="specify-hello-storage-account"></a>Ange hello storage-konto
toopersist din diagnostik information tooyour Azure Storage-konto, måste du ange en anslutningssträng i din tjänstekonfigurationsfil (ServiceConfiguration.cscfg).

För Azure SDK 2.5 hello Storage-konto anges i hello diagnostics.wadcfgx-filen.

> [!NOTE]
> Dessa anvisningar gäller endast tooAzure SDK 2.4 och nedan. För Azure SDK 2.5 hello Storage-konto anges i hello diagnostics.wadcfgx-filen.
>
>

anslutningssträngar för tooset hello:

1. Öppna hello ServiceConfiguration.Cloud.cscfg filen med ditt favoritprogram för textredigering och ange hello anslutningssträngen för lagringen. Hej *AccountName* och *AccountKey* värden finns i hello Azure-portalen i hello storage-konto instrumentpanelen under åtkomstnycklar.

    ```xml
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Spara hello ServiceConfiguration.Cloud.cscfg filen.
3. Öppna hello ServiceConfiguration.Local.cscfg filen och kontrollera att UseDevelopmentStorage är tootrue.

    ```xml
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
   Nu när hello anslutningssträngar ställs finns programmet kvar diagnostiklagringskonto för data tooyour när programmet distribueras.
4. Spara och bygga projektet och sedan distribuera ditt program.

## <a name="step-2-optional-create-custom-performance-counters"></a>Steg 2: (Valfritt) skapa anpassade prestandaräknare
Dessutom toohello fördefinierad prestandaräknare, du kan lägga till egna anpassade prestandaräknare toomonitor webb eller arbetare roller. Anpassade prestandaräknare kan vara används tootrack och övervaka programspecifika beteende och kan skapas eller tas bort i en startaktivitet, webbroll eller arbetsroll med förhöjd behörighet.

hello Azure diagnostics agenten uppdaterar hello prestandaräknaren konfiguration från hello .wadcfg fil minuts när du har startat.  Om du skapar anpassade prestandaräknare i hello OnStart-metoden och Start aktiviteterna ta längre tid än en minut tooexecute, dina anpassade prestandaräknare kommer inte har skapats när hello Azure Diagnostics agent försöker tooload dem.  I det här scenariot ser du att Azure-diagnostik korrekt fångar upp alla diagnostikdata utom dina anpassade prestandaräknare.  tooresolve problemet, skapa hello prestandaräknare i en startåtgärd eller flytta vissa av dina startaktivitet fungerar toohello OnStart-metoden när du har skapat hello prestandaräknare.

Utför följande steg toocreate en enkel anpassade prestandaräknaren ”\MyCustomCounterCategory\MyButton1Counter” hello:

1. Öppna hello tjänstdefinitionsfilen (CSDEF) för ditt program.
2. Lägg till hello Runtime elementet toohello WebRole eller WorkerRole element tooallow körning med förhöjd behörighet:

    ```xml
    <runtime executioncontext="elevated"/>
    ```
3. Spara hello-filen.
4. Öppna hello diagnostik filen (diagnostics.wadcfg SDK 2.4 och lägre eller diagnostics.wadcfgx i SDK 2.5 och senare) och Lägg till följande toohello DiagnosticMonitorConfiguration hello

    ```xml
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
      <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Spara hello-filen.
6. Skapa hello anpassade prestandaräknarkategorin i hello OnStart-metoden i din roll innan du anropar base. OnStart. hello skapar följande C#-exempel en anpassad kategori, om den inte redan finns:

    ```csharp
    public override bool OnStart()
    {
      if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
      {
         CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

         // add a counter tracking user button1 clicks
         CounterCreationData operationTotal1 = new CounterCreationData();
         operationTotal1.CounterName = "MyButton1Counter";
         operationTotal1.CounterHelp = "My Custom Counter for Button1";
         operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
         counterCollection.Add(operationTotal1);

         PerformanceCounterCategory.Create(
           "MyCustomCounterCategory",
           "My Custom Counter Category",
           PerformanceCounterCategoryType.SingleInstance, counterCollection);

         Trace.WriteLine("Custom counter category created.");
      }
      else {
        Trace.WriteLine("Custom counter category already exists.");
      }

    return base.OnStart();
    }
    ```
7. Uppdatera hello räknare i ditt program. följande exempel hello uppdateringar för en anpassad prestandaräknare på Button1_Click händelser:

    ```csharp
    protected void Button1_Click(object sender, EventArgs e)
    {
      PerformanceCounter button1Counter = new PerformanceCounter(
        "MyCustomCounterCategory",
        "MyButton1Counter",
        string.Empty,
        false);
      button1Counter.Increment();
      this.Button1.Text = "Button 1 count: " +
        button1Counter.RawValue.ToString();
    }
    ```
8. Spara hello-filen.  

Anpassade prestandaräknardata samlas nu av hello Azure diagnostics Övervakare.

## <a name="step-3-query-performance-counter-data"></a>Steg 3: Fråga prestandaräknardata
När programmet har distribuerats och kör, hello diagnostik börjar samla in prestandaräknare och spara den tooAzure datalagringen. Du kan använda verktyg som till exempel Server Explorer i Visual Studio [Azure Lagringsutforskaren](http://azurestorageexplorer.codeplex.com/), eller [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) av Cerebrata tooview hello prestandaräknare för data i hello WADPerformanceCountersTable tabell. Du kan också programmässigt fråga hello tabell tjänsten använder [C#](../cosmos-db/table-storage-how-to-use-dotnet.md), [Java](../cosmos-db/table-storage-how-to-use-java.md), [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), eller [PHP](../cosmos-db/table-storage-how-to-use-php.md).

hello C# exemplet nedan visar en enkel fråga mot hello WADPerformanceCountersTable tabell och sparar hello diagnostik data tooa CSV-fil. När hello prestandaräknare sparas tooa CSV-fil, kan du använda hello rita in funktioner i Microsoft Excel eller andra verktyg toovisualize hello data. Vara säker på att tooadd en referens tooMicrosoft.WindowsAzure.Storage.dll som ingår i hello Azure SDK för .NET oktober 2012 och senare. hello-sammansättningen är installerade toohello % Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ katalog.

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Table;
...

// Get hello connection string. When using Microsoft Azure Cloud Services, it is recommended
// you store your connection string using hello Microsoft Azure service configuration
// system (*.csdef and *.cscfg files). You can you use hello CloudConfigurationManager type
// tooretrieve your storage connection string.  If you're not using Cloud Services, it's
// recommended that you store hello connection string in your web.config or app.config file.
// Use hello ConfigurationManager type tooretrieve your storage connection string.

string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
//string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

// Get a reference toohello storage account using hello connection string.  You can also use hello development
// storage account (Storage Emulator) for local debugging.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
//CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "WADPerformanceCountersTable" table.
CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

// Create hello table query, filter on a specific CounterName, DeploymentId and RoleInstance.
TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
  .Where(
    TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
      TableOperators.And,
      TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
      TableOperators.And,
      TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
    )
  )
);

// Execute hello table query.
IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

// Process hello query results and build a CSV file.
StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

foreach (PerformanceCountersEntity entity in result)
{
  sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
    + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
}

StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
sw.Write(sb.ToString());
sw.Close();
```

Entiteter mappar tooC # objekt med hjälp av en anpassad klass som härleds från **TableEntity**. hello följande kod definierar en entitetsklass som representerar en prestandaräknare i hello **WADPerformanceCountersTable** tabell.

```csharp
public class PerformanceCountersEntity : TableEntity
{
  public long EventTickCount { get; set; }
  public string DeploymentId { get; set; }
  public string Role { get; set; }
  public string RoleInstance { get; set; }
  public string CounterName { get; set; }
  public double CounterValue { get; set; }
}
```

## <a name="next-steps"></a>Nästa steg
[Visa ytterligare artiklar på Azure-diagnostik](../azure-diagnostics.md)
