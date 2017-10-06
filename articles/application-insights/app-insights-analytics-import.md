---
title: aaaImport dina data tooAnalytics i Azure Application Insights | Microsoft Docs
description: "Importera statiska data toojoin med app telemetri eller importera en separat data dataströmmen tooquery med Analytics."
services: application-insights
keywords: "Öppna schema, import av data"
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: bwren
ms.openlocfilehash: 7a3a3c9155adc1885dd366ddb13dda80bb894adb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-data-into-analytics"></a>Importera data till Analytics

Importera inga tabelldata i [Analytics](app-insights-analytics.md), antingen toojoin med [Programinsikter](app-insights-overview.md) telemetri från din app, eller så att du kan analysera dem som en separat dataström. Analytics är en kraftfull query language väl lämpade tooanalyzing omfattande tidsstämplad dataströmmar telemetri.

Du kan importera data till Analytics med hjälp av ditt eget schema. Den har inte toouse hello standard Application Insights scheman som begäran eller trace.

Du kan importera JSON eller DSV (avgränsare med kommaavgränsade värden - kommatecken eller semikolon fliken) filer.

Det finns tre situationer där importerar tooAnalytics är användbar:

* **Anslut med app telemetri.** Exempelvis kan du importera en tabell som mappar URL: er från din webbplats toomore läsbar rubriker. Du kan skapa en instrumentpanel diagramrapport som visar hello tio populäraste sidorna på webbplatsen i analyser. Det kan nu visa hello sidnamn i stället för hello URL: er.
* **Korrelera programmets telemetri** med andra källor, till exempel nätverkstrafik, server-data eller CDN loggfiler.
* **Tillämpa Analytics tooa separat dataström.** Application Insights Analytics är ett kraftfullt verktyg som fungerar bra med sparse, tidsstämplad dataströmmar - mycket bättre än SQL i många fall. Om du har en sådan dataström från någon annan källa kan analysera du dem med Analytics.

Skicka tooyour datakälla är enkelt. 

1. (En gång) Definiera hello schemat för dina data i en-datakälla.
2. (Med jämna mellanrum) Överför tooAzure datalagringen och anropa hello REST API toonotify oss att nya data väntar införandet. Hello data är tillgängliga för frågor i Analytics inom några minuter.

hello frekvensen av hello överför definieras av dig och hur snabbt vill du att dina data toobe som är tillgängligt för frågor. Det är effektivare tooupload data i större segment, men inte är större än 1GB.

> [!NOTE]
> *Har mycket data sources tooanalyze?* [*Överväg att använda* logstash *tooship data till Application Insights.*](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a>Innan du börjar

Du behöver:

1. En Application Insights-resurs i Microsoft Azure.

 * Om du vill tooanalyze dina data separat från andra telemetri [skapar en ny Application Insights-resurs](app-insights-create-new-resource.md).
 * Om du ansluter eller jämförelse av dina data med telemetri från en app som redan har konfigurerats med Application Insights, kan du använda hello resurs för appen.
 * Medarbetare eller ägare åtkomst toothat resurs.
 
2. Azure-lagring. Du överför tooAzure lagring och Analytics hämtar dina data därifrån. 

 * Vi rekommenderar att du skapar ett dedikerat storage-konto för din BLOB. Om dina blobbar som delas med andra processer, tar det längre tid för våra processer tooread dina blobbar.


## <a name="define-your-schema"></a>Definiera schemat

Innan du kan importera data, måste du definiera en *datakällan* som anger hello schemat för dina data.
Du kan ha too50 datakällor i en enda Application Insights-resurs

1. Starta guiden hello datakälla. Använd knappen ”Lägg till ny datakälla”. Du kan också - klicka på inställningsknappen i övre högra hörnet och välj ”datakällor” i listrutan.

    ![Lägg till ny datakälla](./media/app-insights-analytics-import/add-new-data-source.png)

    Ange ett namn för den nya datakällan.

2. Definiera format hello-filer som du överför.

    Du kan definiera hello format manuellt eller ladda upp en exempelfil.

    Om hello data i CSV-format, vara hello första raden i exempel hello kolumnrubrikerna. Du kan ändra hello fältnamnen i hello nästa steg.

    hello exempel ska innehålla minst 10 rader eller poster av data.

    Kolumnen eller fältnamn bör ha alfanumeriskt namn (utan blanksteg eller skiljetecken).

    ![Överför en exempelfil](./media/app-insights-analytics-import/sample-data-file.png)


3. Granska hello schema som hello guiden har stött på. Om den härleda hello typer från ett exempel behöva tooadjust hello härleda typer av hello kolumner.

    ![Granska hello härleda schema](./media/app-insights-analytics-import/data-source-review-schema.png)

 * (Valfritt.) Överför schemadefinition. Se hello-formatet.

 * Välj en tidsstämpel. Alla data i Analytics måste ha något tidsstämpelsfält. Det måste vara av typen `datetime`, men som inte har toobe med namnet 'tidsstämpel'. Om dina data har en kolumn som innehåller datum och tid med ISO-format, väljer du det som hello kolumn för tidsstämpel. Annars väljer du ”som data anlänt” och hello importen lägger till något tidsstämpelsfält.

5. Skapa hello datakälla.

### <a name="schema-definition-file-format"></a>Schemaformat för paketdefinitionsfiler

Du kan läsa in hello schemadefinition från en fil i stället för att redigera hello schemat i Användargränssnittet. hello schema definition format är följande: 

Avgränsad format 
```
[ 
    {"location": "0", "name": "RequestName", "type": "string"}, 
    {"location": "1", "name": "timestamp", "type": "datetime"}, 
    {"location": "2", "name": "IPAddress", "type": "string"} 
] 
```

JSON-format 
```
[ 
    {"location": "$.name", "name": "name", "type": "string"}, 
    {"location": "$.alias", "name": "alias", "type": "string"}, 
    {"location": "$.room", "name": "room", "type": "long"} 
]
```
 
Varje kolumn identifieras av hello plats, namn och typen. 

* Plats – är för avgränsad filformatet den hello positionen för hello mappade värdet. JSON-format är det hello jpath hello mappas nyckel.
* Namn – hello visas namnet på hello-kolumn.
* Typ – hello datatypen för kolumnen.
 
Om en exempeldata användes och filformatet avgränsas hello schemadefinition Mappa alla kolumner och lägga till nya kolumner hello slutet. 

JSON tillåter delvis mappning av hello data, därför hello schemadefinitionen för JSON-formatet saknar toomap alla nycklar som finns i en exempeldata. Det kan också mappa kolumner som inte är en del av hello exempeldata. 

## <a name="import-data"></a>Importera data

tooimport data, ladda upp den tooAzure lagring, skapa en snabbtangent för den och göra ett REST API-anrop.

![Lägg till ny datakälla](./media/app-insights-analytics-import/analytics-upload-process.png)

Du kan utföra hello följande process manuellt eller ställa in en automatisk toodo med jämna mellanrum. Du måste toofollow dessa steg för varje datablock som du vill använda tooimport.

1. Ladda upp hello data för[Azure-blobblagring](../storage/blobs/storage-dotnet-how-to-use-blobs.md). 

 * Blobbar kan vara valfri storlek in too1GB okomprimerade. Stora blobbar MB hundratals lämpar sig ur prestandaperspektiv.
 * Du kan komprimera den med Gzip tooimprove tid och svarstid för hello data toobe tillgängliga för frågor. Använd hello `.gz` filnamnstillägg.
 * Det är bästa toouse ett separat lagringskonto för detta ändamål tooavoid anrop från olika tjänster sänka prestanda.
 * När du skickar data i hög frekvens rekommenderas några sekunders toouse mer än ett lagringskonto av prestandaskäl.

 
2. [Skapa en nyckel för signatur för delad åtkomst för hello blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md). hello nyckel bör ha en utgångsperiod på en dag och ger läsbehörighet.
3. Skapa ett REST-anrop toonotify Application Insights som väntar på data.

 * Slutpunkt:`https://dc.services.visualstudio.com/v2/track`
 * HTTP-metod: POST
 * Nyttolasten:

```JSON

    {
       "data": {
            "baseType":"OpenSchemaData",
            "baseData":{
               "ver":"2",
               "blobSasUri":"<Blob URI with Shared Access Key>",
               "sourceName":"<Schema ID>",
               "sourceVersion":"1.0"
             }
       },
       "ver":1,
       "name":"Microsoft.ApplicationInsights.OpenSchema",
       "time":"<DateTime>",
       "iKey":"<instrumentation key>"
    }
```

hello platshållare är:

* `Blob URI with Shared Access Key`: Du behöver från hello procedur för att skapa en nyckel. Är det specifika toohello blob.
* `Schema ID`: hello schema-ID har genererats för din definierat schema. hello data i den här blob ska följa toohello schema.
* `DateTime`: hello UTC-tid på vilka hello begäran har skickats. Vi godkänna dessa format: ISO8601 (som ”2016-01-01 13:45:01”); Rfc822 (”ons, 14 Dec 16 14:57:01 + 0000”); RFC850 (”onsdag, 14-Dec-16 14:57:00 UTC”); RFC1123 (”ons, 14 Dec 2016 14:57:00 + 0000”).
* `Instrumentation key`i Application Insights-resurs.

hello data är tillgängliga i Analytics efter några minuter.

## <a name="error-responses"></a>Felsvar

* **400 Felaktig begäran**: Anger att hello-nyttolasten i begäran är ogiltig. Kontrollera:
 * Rätt instrumentation nyckel.
 * Giltig tid-värde. Det bör vara hello nu i UTC-tid.
 * JSON hello händelse överensstämmer toohello schema.
* **403 Nekad**: hello blob som du har skickat är inte tillgänglig. Kontrollera att nyckeln hello delad åtkomst är giltigt och inte har gått ut.
* **Det gick inte att hitta 404**:
 * hello-blob finns inte.
 * hello sourceId är felaktigt.

Mer detaljerad information finns i hello svar felmeddelande.


## <a name="sample-code"></a>Exempelkod

Den här koden använder hello [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) NuGet-paketet.

### <a name="classes"></a>Klasser

```C#
namespace IngestionClient 
{ 
    using System; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceIngestionRequest 
    { 
        #region Members 
        private const string BaseDataRequiredVersion = "2"; 
        private const string RequestName = "Microsoft.ApplicationInsights.OpenSchema"; 
        #endregion Members 

        public AnalyticsDataSourceIngestionRequest(string ikey, string schemaId, string blobSasUri, int version = 1) 
        { 
            Ver = version; 
            IKey = ikey; 
            Data = new Data 
            { 
                BaseData = new BaseData 
                { 
                    Ver = BaseDataRequiredVersion, 
                    BlobSasUri = blobSasUri, 
                    SourceName = schemaId, 
                    SourceVersion = version.ToString() 
                } 
            }; 
        } 


        [JsonProperty("data")] 
        public Data Data { get; set; } 

        [JsonProperty("ver")] 
        public int Ver { get; set; } 

        [JsonProperty("name")] 
        public string Name { get { return RequestName; } } 

        [JsonProperty("time")] 
        public DateTime Time { get { return DateTime.UtcNow; } } 

        [JsonProperty("iKey")] 
        public string IKey { get; set; } 
    } 

    #region Internal Classes 

    public class Data 
    { 
        private const string DataBaseType = "OpenSchemaData"; 

        [JsonProperty("baseType")] 
        public string BaseType 
        { 
            get { return DataBaseType; } 
        } 

        [JsonProperty("baseData")] 
        public BaseData BaseData { get; set; } 
    } 


    public class BaseData 
    { 
        [JsonProperty("ver")] 
        public string Ver { get; set; } 

        [JsonProperty("blobSasUri")] 
        public string BlobSasUri { get; set; } 

        [JsonProperty("sourceName")] 
        public string SourceName { get; set; } 

        [JsonProperty("sourceVersion")] 
        public string SourceVersion { get; set; } 
    } 

    #endregion Internal Classes 
} 


namespace IngestionClient 
{ 
    using System; 
    using System.IO; 
    using System.Net; 
    using System.Text; 
    using System.Threading.Tasks; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceClient 
    { 
        #region Members 
        private readonly Uri endpoint = new Uri("https://dc.services.visualstudio.com/v2/track"); 
        private const string RequestContentType = "application/json; charset=UTF-8"; 
        private const string RequestAccess = "application/json"; 
        #endregion Members 

        #region Public 

        public async Task<bool> RequestBlobIngestion(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            HttpWebRequest request = WebRequest.CreateHttp(endpoint); 
            request.Method = WebRequestMethods.Http.Post; 
            request.ContentType = RequestContentType; 
            request.Accept = RequestAccess; 

            string notificationJson = Serialize(ingestionRequest); 
            byte[] notificationBytes = GetContentBytes(notificationJson); 
            request.ContentLength = notificationBytes.Length; 

            Stream requestStream = request.GetRequestStream(); 
            requestStream.Write(notificationBytes, 0, notificationBytes.Length); 
            requestStream.Close(); 

            try 
            { 
                using (var response = (HttpWebResponse)await request.GetResponseAsync())
                {
                    return response.StatusCode == HttpStatusCode.OK;
                }
            } 
            catch (WebException e) 
            { 
                HttpWebResponse httpResponse = e.Response as HttpWebResponse; 
                if (httpResponse != null) 
                { 
                    Console.WriteLine( 
                        "Ingestion request failed with status code: {0}. Error: {1}", 
                        httpResponse.StatusCode, 
                        httpResponse.StatusDescription); 
                    return false; 
                }
                throw; 
            } 
        } 
        #endregion Public 

        #region Private 
        private byte[] GetContentBytes(string content) 
        { 
            return Encoding.UTF8.GetBytes(content);
        } 


        private string Serialize(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            return JsonConvert.SerializeObject(ingestionRequest); 
        } 
        #endregion Private 
    } 
} 
```

### <a name="ingest-data"></a>För in data

Använd den här koden för varje blob. 

```C#
   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);
```

## <a name="next-steps"></a>Nästa steg

* [Genomgång av hello Log Analytics-frågespråket](app-insights-analytics-tour.md)
* [Använd *Logstash* toosend data tooApplication insikter](https://github.com/Microsoft/logstash-output-application-insights)
