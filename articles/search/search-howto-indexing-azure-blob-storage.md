---
title: aaaIndexing Azure Blob Storage med Azure Search
description: "Lär dig hur tooindex Azure Blob Storage och extrahera text från dokument med Azure Search"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 2a5968f4-6768-4e16-84d0-8b995592f36a
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/22/2017
ms.author: eugenesh
ms.openlocfilehash: 1bdd34e66a4a9192ed88cacbc7b8456d0dcdfeb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-documents-in-azure-blob-storage-with-azure-search"></a>Indexera dokument i Azure Blob Storage med Azure Search
Den här artikeln visar hur toouse Azure Search tooindex dokument (till exempel PDF-filer, Microsoft Office-dokument och flera andra vanliga format) lagras i Azure Blob storage. Det förklarar först hello grunderna för att installera och konfigurera en indexerare blob. Sedan erbjuder den en ingående undersökning av beteenden och scenarier som du är sannolikt tooencounter.

## <a name="supported-document-formats"></a>Dokumentets format som stöds
hello blob indexeraren kan extrahera text från hello följande dokument:

* PDF
* Microsoft Office-format: DOCX/DOC, XLSX/XLS, PPTX/PPT Ignorerad (Outlook e-postmeddelanden)  
* HTML
* XML
* ZIP-
* EML
* RTF
* Filer med oformaterad text (Se även [indexering oformaterad text](#IndexingPlainText))
* JSON (se [indexering JSON-blobbar](search-howto-index-json-blobs.md))
* CSV (se [indexering CSV-blobbar](search-howto-index-csv-blobs.md) förhandsgranskningsfunktion)

> [!IMPORTANT]
> Stöd för CSV och JSON-matriser stöds för närvarande under förhandsgranskning. Formaten är tillgänglig endast med version **2016-09-01-Preview** av hello version eller REST API 2.x-preview av hello .NET SDK. Kontrollera komma ihåg, förhandsgranska API: er är avsedd för testning och utvärdering och ska inte användas i produktionsmiljöer.
>
>

## <a name="setting-up-blob-indexing"></a>Konfigurera blob-indexering
Du kan konfigurera en Azure Blob Storage indexeraren med:

* [Azure Portal](https://ms.portal.azure.com)
* Azure Search [REST-API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure Search [.NET SDK](https://aka.ms/search-sdk)

> [!NOTE]
> Vissa funktioner (till exempel fältmappningar) är ännu inte tillgängliga i hello portal och har toobe används programmässigt.
>
>

Här kan visar vi hello flödet med hello REST API.

### <a name="step-1-create-a-data-source"></a>Steg 1: Skapa en datakälla
En datakälla anger vilka data tooindex, autentiseringsuppgifter som krävs för tooaccess hello data och principer tooefficiently identifiera ändringar i hello data (nya, ändrade eller borttagna rader). En datakälla som kan användas av flera indexerare i hello samma söktjänsten.

För blob-indexering måste hello datakällan ha följande nödvändiga egenskaper hello:

* **namnet** är hello unika namnet på hello datakälla inom din söktjänst.
* **typen** måste vara `azureblob`.
* **autentiseringsuppgifter** ger hello konto lagringsanslutningssträng som hello `credentials.connectionString` parameter. Se [hur toospecify autentiseringsuppgifter](#Credentials) nedan för information.
* **behållaren** anger en behållare i ditt lagringskonto. Som standard är alla blobbar i behållaren hello strängfält. Om du bara vill tooindex blobbar i en viss virtuell katalog kan du ange katalogen med hjälp av valfri hello **frågan** parameter.

toocreate en datakälla:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Mer information om hello skapa Datasource API finns [skapa Datasource](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="how-toospecify-credentials"></a>Hur toospecify autentiseringsuppgifter ####

Du kan ange hello autentiseringsuppgifter för hello blob-behållaren i något av följande sätt:

- **Fullständig åtkomst lagringsanslutningssträng för kontot**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>`. Du kan hämta hello anslutningssträngen från hello Azure-portalen genom att gå toohello lagring kontoblad > Inställningar > nycklar (för klassiska lagringskonton) eller inställningar > åtkomstnycklar (för Azure Resource Manager storage-konton).
- **Storage-konto signatur för delad åtkomst** (SAS)-anslutningssträng: `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<hello signature>&spr=https&se=<hello validity end time>&srt=co&ss=b&sp=rl` hello SAS ska ha hello listan och läsbehörighet på behållare och objekt (BLOB-objekt i det här fallet).
-  **Signatur för delad åtkomst i behållaren**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<hello signature>&se=<hello validity end time>&sp=rl` hello SAS ska ha hello listan och läsbehörighet på hello behållare.

Mer information om lagring delad åtkomst till signaturer finns i avsnittet [använder signaturer för delad åtkomst](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Om du använder SAS-autentiseringsuppgifter måste tooupdate hello autentiseringsuppgifterna för datakällan regelbundet med förnyat signaturer tooprevent de gått ut. Om SAS-autentiseringsuppgifter ut hello indexeraren misslyckas med ett felmeddelande för`Credentials provided in hello connection string are invalid or have expired.`.  

### <a name="step-2-create-an-index"></a>Steg 2: Skapa ett index
hello index anger hello fält i ett dokument, attribut, och andra konstruktioner som formen hello sökinställningar.

Här är hur toocreate ett index med en sökbar `content` fältet toostore hello text som hämtats från BLOB:   

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }

Mer information om hur du skapar index finns [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="step-3-create-an-indexer"></a>Steg 3: Skapa en indexerare
En indexerare ansluter en datakälla med en mål-sökindexet och ger en schemalagd tooautomate hello datauppdatering.

När hello index och datakälla har skapats kan är du klar toocreate hello indexeraren:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Indexeraren körs varannan timme (schemaintervallet anges för ”PT2H”). toorun en indexerare var 30: e minut, ange hello-intervall för ”PT30M”. hello är kortaste stöds 5 minuter. hello schemat är valfritt - om detta utelämnas, indexeraren körs bara en gång när den skapas. Du kan dock köra en indexerare på begäran när som helst.   

Mer information om Hej skapa indexeraren API, kolla [skapa indexeraren](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="how-azure-search-indexes-blobs"></a>Hur Azure Search index blobbar

Beroende på hello [indexeraren configuration](#PartsOfBlobToIndex), hello blob indexeraren kan indexera lagring metadata (användbart när du bara försiktighet om hello metadata och behöver inte tooindex hello innehållet i BLOB), lagring och innehåll metadata, eller båda metadata och textinnehåll. Som standard extraheras hello indexeraren både i metadata och innehåll.

> [!NOTE]
> Som standard indexeras blobbar med strukturerat innehåll, till exempel JSON eller CSV som en enda del av texten. Om du vill tooindex JSON och CSV-blobbar i ett strukturerat sätt se [indexering JSON-blobbar](search-howto-index-json-blobs.md) och [indexering CSV-blobbar](search-howto-index-csv-blobs.md) Förhandsgranska funktioner.
>
> Ett sammansatt eller inbäddade dokument (t.ex ett ZIP-arkiv eller ett Word-dokument med inbäddade Outlook e-postmeddelande som innehåller bifogade filer) indexeras också som ett enskilt dokument.

* hello textinnehåll hello dokumentet hämtas till en string-fält med namnet `content`.

> [!NOTE]
> Azure Search begränsar hur mycket text extraheras beroende på hello prisnivån: 32000 tecken kostnadsfritt nivån, 64,000 för grundläggande och 4 miljoner för Standard-, Standard S2- och Standard S3 nivåer. En varning ingår i hello indexeraren Statussvaret för trunkerat dokument.  

* Användaren har angett metadataegenskaper finns på hello blob extraheras eventuella ordagrant.
* Standard-blob metadataegenskaper extraheras till hello följande fält:

  * **metadata\_lagring\_namn** (Edm.String) - hello filnamn hello-blob. Till exempel om du har en blob-/my-container/my-folder/subfolder/resume.pdf hello värdet för det här fältet är `resume.pdf`.
  * **metadata\_lagring\_sökväg** (Edm.String) - hello fullständiga URI: N för hello blob, inklusive hello storage-konto. Till exempel, `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`
  * **metadata\_lagring\_innehåll\_typ** (Edm.String) - innehållstyp som angetts av hello-kod används tooupload hello blob. Till exempel `application/octet-stream`.
  * **metadata\_lagring\_senaste\_ändrade** (Edm.DateTimeOffset) - ändrades senast tidsstämpel för hello-blob. Azure Search använder den här tidsstämpel tooidentify ändras blobar tooavoid indexeringen allt efter hello inledande indexeringen.
  * **metadata\_lagring\_storlek** (Edm.Int64) - blob-storlek i byte.
  * **metadata\_lagring\_innehåll\_md5** (Edm.String) - MD5-hash hello blob-innehåll, om de är tillgängliga.
* Dokumentet metadataformat egenskaper specifika tooeach extraheras till hello fälten [här](#ContentSpecificMetadata).

Behöver du inte toodefine fält för alla hello ovan egenskaper i din sökindex - avbilda bara hello egenskaper som du behöver för ditt program.

> [!NOTE]
> Ofta är hello fältnamnen i ditt befintliga index olika från hello fältnamn genereras under Extraheringen av dokumentet. Du kan använda **fältet mappningar** toomap hello egenskapsnamn som tillhandahålls av Azure Search toohello fältnamnen i Sök-index. Ett exempel på fält mappningar använda nedan visas.
>
>

<a name="DocumentKeys"></a>
### <a name="defining-document-keys-and-field-mappings"></a>Definiera dokumentet nycklar och fältmappningar
I Azure Search identifierar hello Dokumentnyckel ett dokument. Varje sökindex måste ha exakt ett fält av typen Edm.String. hello nyckelfältet krävs för varje dokument som har lagts till toohello index (det är faktiskt hello endast obligatoriskt fält).  

Du bör noggrant vilka extraherade fältet ska mappa toohello nyckelfält för ditt index. hello kandidater är:

* **metadata\_lagring\_namn** – detta kan vara praktiskt, men Observera att 1) hello namn inte kan vara unika, som du kanske har blobbar med samma namn i olika mappar hello och 2) hello kanske innehåller tecken som är ogiltiga i dokumentet nycklar, till exempel bindestreck. Du kan hantera ogiltiga tecken med hjälp av hello `base64Encode` [fältet mappning funktionen](search-indexer-field-mappings.md#base64EncodeFunction) – om du gör detta, Kom ihåg tooencode dokumentet nycklar när skicka dem i API-anrop, till exempel sökning. (Till exempel i .NET du kan använda hello [UrlTokenEncode metoden](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) för detta ändamål).
* **metadata\_lagring\_sökväg** - hello fullständig sökväg garanterar unikhet, men hello sökvägen innehåller definitivt `/` tecken [ogiltig i en Dokumentnyckel](https://docs.microsoft.com/rest/api/searchservice/naming-rules).  Som ovan, har hello möjlighet att kodning hello nycklar med hjälp av hello `base64Encode` [funktionen](search-indexer-field-mappings.md#base64EncodeFunction).
* Ingen av hello alternativen ovan fungerar för dig, kan du lägga till en anpassad metadata egenskapen toohello blobbar. Det här alternativet, men kräver din blob överför processen tooadd som metadata egenskapen tooall blobbar. Eftersom hello nyckeln är en obligatorisk egenskap, misslyckas alla BLOB som inte har egenskapen toobe indexeras.

> [!IMPORTANT]
> Om det finns ingen explicit mappning för hello nyckelfältet i hello index, använder Azure Search automatiskt `metadata_storage_path` som hello nyckel och base-64 kodar nyckelvärdena (hello andra alternativ ovan).
>
>

I det här exemplet ska vi välja hello `metadata_storage_name` fält som hello Dokumentnyckel. Vi förutsätter också ditt index har ett fält med namnet `key` och ett fält `fileSize` för att lagra hello storlek. toowire saker upp efter behov, ange följande fältmappningar när du skapar eller uppdaterar indexeraren hello:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

toobring denna alla tillsammans, här är hur du kan lägga till fältmappningar och aktivera Base64-kodning av nycklar för en befintlig indexerare:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }

> [!NOTE]
> toolearn mer om fältmappningar finns [i den här artikeln](search-indexer-field-mappings.md).
>
>

<a name="WhichBlobsAreIndexed"></a>
## <a name="controlling-which-blobs-are-indexed"></a>Kontrollera vilka blobbar som indexeras
Du kan styra vilka blobbar indexeras och som hoppas över.

### <a name="index-only-hello-blobs-with-specific-file-extensions"></a>Indexera endast hello blobbar med specifika filnamnstillägg
Du kan indexera endast hello blobbar med hello filnamnstillägg som du anger genom att använda hello `indexedFileNameExtensions` indexeraren konfigurationsparameter. hello-värdet är en sträng som innehåller en kommaavgränsad lista med filnamnstillägg (med en inledande punkt). Till exempel tooindex endast hello. PDF och. DOCX blobbar, gör du följande:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions"></a>Uteslut BLOB med specifika filnamnstillägg
Du kan utesluta blobbar med specifika filnamnstillägg från att indexera med hjälp av hello `excludedFileNameExtensions` konfigurationsparameter. hello-värdet är en sträng som innehåller en kommaavgränsad lista med filnamnstillägg (med en inledande punkt). Till exempel blobbar tooindex alla utom de med hello. PNG och. JPEG-tillägg, gör du följande:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Om båda `indexedFileNameExtensions` och `excludedFileNameExtensions` parametrar finns, Azure har genomsöks vid `indexedFileNameExtensions`, sedan på `excludedFileNameExtensions`. Det innebär att om hello samma filnamnstillägg finns i båda listorna, den kommer att uteslutas från indexering.

### <a name="dealing-with-unsupported-content-types"></a>Hantera innehållstyper som inte stöds

Som standard avbryts hello blob-indexering så snart den stöter på en blob med en stöds inte innehållstyp (till exempel en bild). Naturligtvis kan du använda hello `excludedFileNameExtensions` parametern tooskip vissa typer av innehåll. Du kanske behöver dock tooindex blobbar utan att känna till alla hello möjliga typer av innehåll i förväg. toocontinue indexeringen när en innehållstyp som inte stöds har påträffats ange hello `failOnUnsupportedContentType` konfigurationsparameter för`false`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }

### <a name="ignoring-parsing-errors"></a>Ignorerar tolkningsfel

Azure Search dokumentet extrahering logik är inte perfekt och misslyckas ibland tooparse dokument för en stöds innehållstyp, t.ex. DOCX eller. PDF-FILEN. Om du inte vill att toointerrupt hello indexering i sådana fall kan du ange hello `maxFailedItems` och `maxFailedItemsPerBatch` parametrar toosome rimliga konfigurationsvärden. Exempel:

    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }

<a name="PartsOfBlobToIndex"></a>
## <a name="controlling-which-parts-of-hello-blob-are-indexed"></a>Kontrollera vilka delar av hello blob indexeras

Du kan styra vilka delar av hello blob som indexeras med hello `dataToExtract` konfigurationsparameter. Det kan ta hello följande värden:

* `storageMetadata`-Anger att endast hello [standard blob-egenskaper och användardefinierade metadata](../storage/blobs/storage-properties-metadata.md) indexeras.
* `allMetadata`-Anger att metadata för lagring och hello [innehållstypen specifika metadata](#ContentSpecificMetadata) extraheras från hello blob innehåll som indexeras.
* `contentAndMetadata`-Anger att alla metadata och textinnehåll extraheras från hello blob som indexeras. Detta är standardvärdet för hello.

Till exempel tooindex endast hello lagring metadata, Använd:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }

### <a name="using-blob-metadata-toocontrol-how-blobs-are-indexed"></a>Använda blob metadata toocontrol hur blobbar indexeras

hello konfigurationsparametrar som beskrivs ovan gäller tooall blobbar. Ibland kan du behöva toocontrol hur *enskilda blobbar* indexeras. Du kan göra detta genom att lägga till följande hello blob metadataegenskaper och värden:

| Egenskapsnamn | Egenskapsvärde | Förklaring |
| --- | --- | --- |
| AzureSearch_Skip |”true” |Instruerar hello blob indexeraren toocompletely hoppa över hello blob. Varken metadata eller innehåll extrahering görs. Detta är användbart när en viss blob misslyckas flera gånger och avbryter hello indexering processen. |
| AzureSearch_SkipContent |”true” |Detta är likvärdiga med `"dataToExtract" : "allMetadata"` inställningen beskrivs [ovan](#PartsOfBlobToIndex) begränsade tooa viss blob. |

## <a name="incremental-indexing-and-deletion-detection"></a>Identifiering av inkrementell indexering och borttagning
När du ställer in en blob indexeraren toorun enligt ett schema, den indexerar endast hello ändrade blobbar, enligt systemets hello blob `LastModified` tidsstämpel.

> [!NOTE]
> Du har inte toospecify en princip för identifiering av ändring – stegvis indexering aktiveras åt dig automatiskt.

toosupport tar bort dokument, Använd en ”mjuk borttagning”-metoden. Om du tar bort hello BLOB direkt tas motsvarande dokument inte bort från hello sökindex. Använd i stället hello följande steg:  

1. Lägg till en anpassad metadata egenskapen toohello blob tooindicate tooAzure sökning att logiskt bort
2. Konfigurera en princip för mjuk borttagning identifiering på hello-datakälla
3. När hello indexeraren har bearbetats hello blob (som visas efter hello indexeraren status API) kan du fysiskt tar bort hello blob

Till exempel hello följa principen betraktar en blob-toobe bort om det har en metadataegenskap `IsDeleted` med hello värdet `true`:

    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",     
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }   

## <a name="indexing-large-datasets"></a>Indexering stora datauppsättningar

Indexera blobbar kan vara en tidskrävande process. Du kan påskynda indexering av partitionering dina data och använder flera indexerare tooprocess hello data parallellt i fall där du har miljontals blobbar tooindex. Här är hur du kan konfigurera detta:

- Dela dina data i flera blob-behållare eller virtuella mappar
- Konfigurera flera Azure Search datakällor, en per behållare eller mappen. toopoint tooa blob-mappen, Använd hello `query` parameter:

    ```
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

- Skapa en motsvarande indexerare för varje datakälla. Alla hello indexerare kan punkt toohello sökindex för samma mål.  

- En sökning-enhet i din tjänst kan köra en indexerare vid en given tidpunkt. Flera indexerare är som beskrivs ovan endast användbar om de köras parallellt. toorun flera indexerare parallellt, skala upp din söktjänst genom att skapa ett lämpligt antal partitioner och repliker. Till exempel om din söktjänst har 6 search-enheter (till exempel 2 partitioner x 3 repliker), kan sedan 6 indexerare köras samtidigt, vilket resulterar i en six-fold ökning hello indexeringsflöde. toolearn mer information om skalning och kapacitetsplanering, se [skala resursen nivåer för fråga och indexering arbetsbelastningar i Azure Search](search-capacity-planning.md).

## <a name="indexing-documents-along-with-related-data"></a>Indexera dokument tillsammans med relaterade data

Du kanske vill för ”montera” dokument från flera källor i ditt index. Exempelvis kanske toomerge text från BLOB med andra metadata som lagras i Cosmos-databasen. Du kan även använda hello push indexering API tillsammans med olika indexerare för bygga upp Sök dokument från flera delar. 

Måste tooagree på hello Dokumentnyckel för den här toowork alla indexerare och andra komponenter. En detaljerad genomgång finns i den här externa artikeln: [kombinera dokument med andra data i Azure Search ](http://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

<a name="IndexingPlainText"></a>
## <a name="indexing-plain-text"></a>Indexering oformaterad text 

Om alla dina blobbar innehålla oformaterad text i hello samma kodning, kan du avsevärt förbättra indexering prestanda med hjälp av **text parsning läge**. toouse text parsning läge, ange hello `parsingMode` konfigurationsegenskapen för`text`:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }

Som standard hello `UTF-8` kodning antas. en annan kodning toospecify använda hello `encoding` konfigurationsegenskapen: 

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }


<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>Egenskaper för innehåll typspecifika metadata
hello följande tabell sammanfattas bearbetas för respektive format och beskriver hello metadataegenskaper extraheras med Azure Search.

| Dokumentformat / innehållstyp | Innehållstypen specifika metadataegenskaper | Mer information |
| --- | --- | --- |
| HTML (`text/html`) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |Remsans HTML-kod och extrahera text |
| PDF (`application/pdf`) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Extrahera text, inklusive embedded dokument (exklusive bilder) |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extrahera text, inklusive embedded dokument |
| DOC (application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extrahera text, inklusive embedded dokument |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extrahera text, inklusive embedded dokument |
| XLS (application/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extrahera text, inklusive embedded dokument |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extrahera text, inklusive embedded dokument |
| PPT (application/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extrahera text, inklusive embedded dokument |
| Meddelande (outlook-program/vnd.ms) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Extrahera text, inklusive bifogade filer |
| ZIP (program/zip) |`metadata_content_type` |Extrahera text från alla dokument i hello-Arkiv |
| XML (application/xml) |`metadata_content_type`</br>`metadata_content_encoding`</br> |Remsans XML-koden och extrahera text |
| JSON (application/json) |`metadata_content_type`</br>`metadata_content_encoding` |Extrahera text<br/>Obs: Om du behöver tooextract flera dokument fält från en JSON-blob finns [indexering JSON-blobbar](search-howto-index-json-blobs.md) information |
| EML (message/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Extrahera text, inklusive bifogade filer |
| RTF (program/rtf) |`metadata_content_type`</br>`metadata_author`</br>`metadata_character_count`</br>`metadata_creation_date`</br>`metadata_page_count`</br>`metadata_word_count`</br> | Extrahera text|
| Oformaterad text (text/plain) |`metadata_content_type`</br>`metadata_content_encoding`</br> | Extrahera text|


## <a name="help-us-make-azure-search-better"></a>Hjälp oss att förbättra Azure Search
Om du har funktionsförfrågningar om eller förslag på förbättringar kan berätta för oss på vår [UserVoice-webbplatsen](https://feedback.azure.com/forums/263029-azure-search/).
