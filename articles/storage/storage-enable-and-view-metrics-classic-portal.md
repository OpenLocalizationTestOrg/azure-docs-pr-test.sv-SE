---
title: "aaaEnabling storage-mätvärden i hello Azure-portalen | Microsoft Docs"
description: "Hur tooenable storage-mätvärden för hello Blob, kön, tabell, och Filtjänster"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 2fb5b229-f099-4334-92be-4e0e7dd257d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: 4c990371e08a6586d935b0535149eabd4960cfaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-storage-metrics-and-viewing-metrics-data"></a>Aktivera Storage-mätvärden och visa mått data
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Översikt
Storage-mätvärden är aktiverad som standard när du skapar ett nytt lagringskonto. Du kan konfigurera övervakning med hjälp av antingen hello [klassiska Azure-portalen](https://manage.windowsazure.com), Windows PowerShell eller via programmering genom en API-lagring.

När du aktiverar Storage-mätvärden, måste du välja en kvarhållningsperiod för hello data: den här perioden avgör för hur länge hello lagring tjänsten håller hello mått och kostnader för hello utrymme krävs toostore dem. Du bör normalt använda en kortare period för minut mått än timvis mått på grund av hello betydande extra utrymme krävs för minut mått. Du bör välja en kvarhållningsperiod så att du har tillräcklig tid tooanalyze hello data och hämta några mått som du vill tookeep för offline-analys och rapportering. Kom ihåg att du också kommer att debiteras för att ladda ned mått data från ditt lagringskonto.

## <a name="how-tooenable-storage-metrics-using-hello-azure-classic-portal"></a>Hur tooenable lagring mått med hello klassiska Azure-portalen
I hello [klassiska Azure-portalen](https://manage.windowsazure.com), du använder hello konfigurationssidan för en storage-konto toocontrol lagring mått. För övervakning, kan du ange en nivå och en Bevarandeperiod i dagar för alla blobbar, tabeller och köer. I varje fall är hello nivån en av följande hello:

* Ut – Samlas inga mått.
* Minimalt-Storage-mätvärden samlar in en grundläggande uppsättning mått som ingång-/ utgång, tillgänglighet, svarstid och lyckade procenttal som aggregeras för hello Blob-, tabell- och kön.
* Utförlig – Storage-mätvärden samlar in en fullständig uppsättning mått som innehåller Hej samma mätvärden för varje storage API-åtgärd, dessutom toohello servicenivåer mått. Utförlig mått aktivera närmare analys av problem som uppstår under program-åtgärder.

Observera att hello klassiska Azure-portalen kan för närvarande du inte tooconfigure minut mått i ditt lagringskonto; Du måste aktivera minut mått med PowerShell eller programmässigt.

## <a name="how-tooenable-storage-metrics-using-powershell"></a>Hur tooenable lagring mått med PowerShell
Du kan använda PowerShell på din lokala dator tooconfigure Storage-mätvärden i ditt lagringskonto med hjälp av hello Azure PowerShell-cmdlet Get-AzureStorageServiceMetricsProperty tooretrieve hello aktuella inställningar och hello cmdlet Ange AzureStorageServiceMetricsProperty toochange hello aktuella inställningar.

hello-cmdletar som styr Storage-mätvärden använder hello följande parametrar:

* MetricsType möjliga värden är timmar och minuter.
* ServiceType möjliga värden är Blob, köer och tabellen.
* MetricsLevel möjliga värden är ingen (motsvarande tooOff i hello klassiska Azure-portalen)-tjänsten (motsvarande tooMinimal i hello klassiska Azure-portalen) och ServiceAndApi (motsvarande tooVerbose i hello klassiska Azure-portalen).

Till exempel växlar hello följande kommando på minut mått hello blob-tjänst i din standardkontot för lagring med hello Bevarandeperiod ställa in toofive dagar:

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5
```
hello hämtar följande kommando hello aktuella timvis mått nivå och kvarhållning dagar hello blob-tjänst i standardkontot för lagring:

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```
Information om hur tooconfigure hello Azure PowerShell-cmdlets toowork med din Azure-prenumeration och hur tooselect hello standardlagring kontot toouse finns: [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

## <a name="how-tooenable-storage-metrics-programmatically"></a>Hur tooenable Storage-mätvärden via programmering
hello följande C# fragment visar hur tooenable mått och loggning för hello Blob-tjänsten använder hello storage-klientbibliotek för .NET:

```csharp
//Parse hello connection string for hello storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

// Create service client for credentialed access toohello Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Enable Storage Analytics logging and set retention policy too10 days. 
ServiceProperties properties = new ServiceProperties();
properties.Logging.LoggingOperations = LoggingOperations.All;
properties.Logging.RetentionDays = 10;
properties.Logging.Version = "1.0";

// Configure service properties for metrics. Both metrics and logging must be set at hello same time.
properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.HourMetrics.RetentionDays = 10;
properties.HourMetrics.Version = "1.0";

properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.MinuteMetrics.RetentionDays = 10;
properties.MinuteMetrics.Version = "1.0";

// Set hello default service version toobe used for anonymous requests.
properties.DefaultServiceVersion = "2015-04-05";

// Set hello service properties.
blobClient.SetServiceProperties(properties);
```

## <a name="viewing-storage-metrics"></a>Visa Storage-mätvärden
När du har konfigurerat lagring mått toomonitor ditt lagringskonto, registrerar hello mått i en uppsättning välkända tabeller i ditt lagringskonto. Du kan använda hello övervakaren sidan för ditt lagringskonto i hello Azure klassiska Portal tooview hello timvis mått när de blir tillgängliga i ett diagram. På den här sidan i hello klassiska Azure-portalen kan du:

* Välj vilka mått tooplot i hello diagram (hello valet av tillgängliga mått beror på om du har valt utförlig eller minimalt övervakningen hello-tjänsten på konfigurationssidan hello).
* Välj hello tidsintervallet för hello mått som visas på hello diagram.
* Välj toouse en absolut eller relativ skala tooplot hello mått.
* Konfigurera e-aviseringar toonotify när ett specifikt mått når ett visst värde.

Om du vill toodownload hello mått för långsiktig lagring eller tooanalyze dem lokalt, du behöver toouse ett verktyg eller skriva kod tooread hello tabeller. Du måste hämta hello minut mått för analys. hello tabeller visas inte om du lista alla hello tabeller i ditt lagringskonto, men du kan komma åt dem direkt efter namn. Många tredjeparts-lagring – bläddring verktyg är medveten om dessa tabeller och aktivera tooview dem direkt (finns i blogginlägget hello [lagringsutforskare för Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) en lista över tillgängliga verktyg).

### <a name="hourly-metrics"></a>Varje timme mått
* $MetricsHourPrimaryTransactionsBlob
* $MetricsHourPrimaryTransactionsTable
* $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minut mått
* $MetricsMinutePrimaryTransactionsBlob
* $MetricsMinutePrimaryTransactionsTable
* $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Kapacitet
* $MetricsCapacityBlob

Du kan hitta fullständig information om hello scheman för dessa tabeller på [Storage Analytics mätvärden tabellschemat](https://msdn.microsoft.com/library/azure/hh343264.aspx). hello exempel raderna nedan visar endast en delmängd av hello tillgängliga kolumner, men illustrera vissa viktiga funktioner i hello sätt Storage-mätvärden sparar de här måtten:

| PartitionKey | RowKey | tidsstämpel | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Tillgänglighet | AverageE2ELatency | AverageServerLatency | PercentSuccess |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| 20140522T1100 |användaren. Alla |2014-05-22T11:01:16.7650250Z |7 |7 |4003 |46801 |100 |104.4286 |6.857143 |100 |
| 20140522T1100 |användaren. QueryEntities |2014-05-22T11:01:16.7640250Z |5 |5 |2694 |45951 |100 |143.8 |7.8 |100 |
| 20140522T1100 |användaren. QueryEntity |2014-05-22T11:01:16.7650250Z |1 |1 |538 |633 |100 |3 |3 |100 |
| 20140522T1100 |användaren. UpdateEntity |2014-05-22T11:01:16.7650250Z |1 |1 |771 |217 |100 |9 |6 |100 |

I det här exemplet minut mätvärdesdata kan använder hello partitionsnyckel hello tid i minuter upplösning. Hej radnyckel identifierar hello typ av information som lagras i hello raden och detta består av två delar av information, hello åtkomsttyp och hello Begärandetypen:

* hello åtkomsttypen är användaren eller systemet, där användaren refererar tooall användaren begäranden toohello storage-tjänsten och system refererar toorequests som gjorts av Storage Analytics.
* hello Begärandetypen är alla i så fall är det en sammanfattande rad eller identifierar hello specifika API: et, till exempel QueryEntity eller UpdateEntity.

hello exempeldata ovan visas alla hello registrerar för en minut (börjar på 11:00:00), så hello antalet QueryEntities förfrågningar plus hello antal QueryEntity begäranden plus hello antalet UpdateEntity begäranden summera tooseven som är hello totala visas på hello användare: All raden. Du kan dessutom härleda hello slutpunkt till slutpunkt Medelsvarstid 104.4286 på hello användare: All rad genom en beräkning ((143.8 * 5) + 3 + 9) / 7.

Bör du ställa in aviseringar i hello klassiska Azure-portalen på hello övervakaren sidan så att Storage-mätvärden automatiskt meddela dig om några viktiga ändringar i hello beteendet för storage-tjänster. Om du använder en lagring explorer verktyget toodownload informationen mått i en avgränsad format kan du använda Microsoft Excel tooanalyze hello-data. Finns i blogginlägget hello [lagringsutforskare för Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) för en lista över tillgängliga lagringsutrymmet explorer verktyg.

## <a name="accessing-metrics-data-programmatically"></a>Dataåtkomst mätvärden via programmering
hello visar nedan exempel C#-kod som har åtkomst till hello minut mått för ett antal minuter och visar hello resultaten i ett konsolfönster. Den använder hello Azure Storage-biblioteket version 4 som innehåller hello CloudAnalyticsClient klass som förenklar använder hello mått tabeller i lagringen.

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
{
    // Convert hello dates toohello format used in hello PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
        Console.WriteLine("Minute Metrics for Service {0} from {1} too{2} UTC", service, start, end);
        var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
        var t = analyticsClient.GetMinuteMetricsTable(service);
        var opContext = new OperationContext();
        var query =
          from entity in metricsQuery
          // Note, you can't filter using hello entity properties Time, AccessType, or TransactionType
          // because they are calculated fields in hello MetricsEntity class.
          // hello PartitionKey identifies hello DataTime of hello metrics.
          where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0 
        select entity;

        // Filter on "user" transactions after fetching hello metrics from Table Storage.
        // (StartsWith is not supported using LINQ with Azure table storage)
        var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
        var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
        Console.WriteLine(resultString);
    }
}

private static string MetricsString(MetricsEntity entity, OperationContext opContext)
{
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
}
```

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Vilka avgifter gör uppkommer när du aktiverar lagring mått?
Skriva begäranden toocreate tabellentiteter för mätvärden debiteras hello standardpriser gäller tooall Azure Storage-åtgärder.

Förfrågningar om läsning och ta bort en klient toometrics data är också fakturerbar på standardpriser. Om du har konfigurerat en databevarandeprincip debiteras du inte när Azure Storage tar bort den gamla mått data. Om du tar bort analysdata debiteras ditt konto för hello delete-åtgärder.

hello-kapaciteten som används av hello mått tabeller är också fakturerbar: du kan använda hello följande tooestimate hello storleken på den kapacitet som används för att lagra data för mått:

* Om varje timme en tjänst använder varje API i varje tjänst, är ungefär 148KB data lagrade varje timme i hello mått transaktion tabellerna om du har aktiverat både tjänsten och översikt över API-nivå.
* Om varje timme en tjänst använder varje API i varje tjänst, är cirka 12KB data lagrade varje timme i hello mått transaktion tabellerna om du har aktiverat bara servicenivå som är sammanfattning.
* hello kapacitet för BLOB har två rader läggs varje dag (förutsatt att användaren har valt i loggar): Detta innebär att varje dag hello ökar storleken på den här tabellen genom in tooapproximately 300 byte.

## <a name="next-steps"></a>Nästa steg:
[Aktivera Storage Analytics loggning och åtkomst till loggdata](https://msdn.microsoft.com/library/dn782840.aspx)
