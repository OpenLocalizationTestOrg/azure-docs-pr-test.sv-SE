---
title: "aaaConnecting Azure SQL Database tooAzure Sök med hjälp av indexerare | Microsoft Docs"
description: "Lär dig hur toopull data från Azure SQL Database tooan Azure Search index med hjälp av indexerare."
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: e9bbf352-dfff-4872-9b17-b1351aae519f
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/13/2017
ms.author: eugenesh
ms.openlocfilehash: b28a11cf18ef994de99e09af90bbfeb171ef3cde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-azure-sql-database-tooazure-search-using-indexers"></a>Ansluta Azure SQL Database tooAzure sökningen med indexerare

Innan du kan fråga en [Azure Search index](search-what-is-an-index.md), måste du fylla den med dina data. Om hello data finns i en Azure SQL-databas, en **Azure Search indexerare för Azure SQL Database** (eller **Azure SQL indexeraren** för kort) kan automatisera indexering hello-processen, vilket innebär att mindre kod toowrite och mindre infrastruktur toocare om.

Den här artikeln beskriver hello säkerhetsnivån med [indexerare](search-indexer-overview.md), men även beskriver funktionerna kan endast användas med Azure SQL-databaser (till exempel integrerad ändringsspårning). 

I tillägg tooAzure SQL-databaser, ger Azure Search indexerare för [Azure Cosmos DB](search-howto-index-documentdb.md), [Azure Blob storage](search-howto-indexing-azure-blob-storage.md), och [Azure-tabellagring](search-howto-indexing-azure-tables.md). toorequest stöd för andra datakällor, ange din feedback på hello [Azure Search Feedbackforum](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Indexerare och datakällor

En **datakällan** anger vilka data tooindex, autentiseringsuppgifter för åtkomst till data och principer som effektivt identifiera ändringar i hello data (nya, ändrade eller borttagna rader). Den har definierats som en oberoende resurs så att den kan användas av flera indexerare.

En **indexeraren** är en resurs som ansluter en enskild datakälla med ett riktade search-index. En indexerare används i hello följande sätt:

* Utför en enstaka kopia av hello data toopopulate ett index.
* Uppdatera ett index med ändringar i hello datakällan enligt ett schema.
* Kör på begäran tooupdate ett index efter behov.

En enda indexeraren kan bara använda en tabell eller vy, men du kan skapa flera indexerare om du vill toopopulate flera search-index. Mer information om begrepp finns [Indexeringsåtgärder: vanliga arbetsflöde](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations#typical-workflow).

Du kan installera och konfigurera en Azure SQL indexeraren med:

* Guiden Importera Data i hello [Azure-portalen](https://portal.azure.com)
* Azure Search [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)
* Azure Search [REST-API](https://docs.microsoft.com/en-us/rest/api/searchservice/indexer-operations)

I den här artikeln använder vi hello REST API toocreate **indexerare** och **datakällor**.

## <a name="when-toouse-azure-sql-indexer"></a>När toouse Azure SQL indexeraren
Beroende på flera faktorer som rör tooyour data, hello användning av Azure SQL indexeraren kanske eller kanske inte är rätt. Om dina data uppfyller följande krav hello kan använda du Azure SQL indexeraren.

| Kriterie | Information |
|----------|---------|
| Data har sitt ursprung i en enda tabell eller vy | Om hello data är spridda över flera tabeller, kan du skapa en enda vy av hello data. Om du använder vyn inte kan toouse SQL Server integrerad ändra identifiering toorefresh ett index med stegvisa ändringar. Mer information finns i [fånga ändras och ta bort rader](#CaptureChangedRows) nedan. |
| Datatyperna är kompatibla | De flesta, men inte alla hello SQL-typer stöds i ett Azure Search-index. En lista, se [mappning datatyper](#TypeMapping). |
| Realtidsdata synkronisering krävs inte | En indexerare kan indexera tabellen högst var femte minut. Om data ändras ofta och hello ändras behöver toobe återspeglas i hello index inom några sekunder eller minuter som enda, bör du använda hello [REST API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) eller [.NET SDK](search-import-data-dotnet.md) toopush uppdatera rader direkt. |
| Det är möjligt att stegvis indexering | Om du har en stor mängd data och planera toorun hello indexeraren enligt ett schema, Azure Search måste kunna identifiera tooefficiently nya, ändrade eller borttagna rader. Icke-inkrementell indexering tillåts endast om du indexering på begäran (inte på schema), eller indexering färre än 100 000 rader. Mer information finns i [fånga ändras och ta bort rader](#CaptureChangedRows) nedan. |

## <a name="create-an-azure-sql-indexer"></a>Skapa en Azure SQL-indexerare

1. Skapa hello datakälla:

   ```
    POST https://myservice.search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of hello table or view that you want tooindex" }
    }
   ```

   Du kan hämta hello anslutningssträngen från hello [Azure-portalen](https://portal.azure.com); Använd hello `ADO.NET connection string` alternativet.

2. Skapa hello mål Azure Search index om du inte redan har en. Du kan skapa ett index med hello [portal](https://portal.azure.com) eller hello [skapa Index API](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Se till att hello schemat för mål-index är kompatibel med hello schemat för källtabellen hello - Se [mappning mellan SQL och Azure search-datatyper](#TypeMapping).

3. Skapa hello indexeraren genom att ge den ett namn och refererar till hello data källan och målet index:

    ```
    POST https://myservice.search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }
    ```

En indexerare som skapas på detta sätt har inte ett schema. Det körs automatiskt när när den har skapats. Du kan köra det igen på en gång med hjälp av en **köra indexeraren** begäran:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2016-09-01
    api-key: admin-key

Du kan anpassa flera aspekter av indexerare beteende, t.ex batchstorlek och hur många dokument kan hoppas över innan det går inte att köra en indexerare. Mer information finns i [skapa indexeraren API](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

Du kan behöva tooallow Azure services tooconnect tooyour-databasen. Se [ansluta från Azure](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) anvisningar för hur toodo som.

toomonitor hello indexeraren status och körningstiden (antal objekt som indexeras, fel, etc.), används en **indexeraren status** begäran:

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2016-09-01
    api-key: admin-key

hello svar bör se ut ungefär toohello följande:

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null
            },
            ... earlier history items
        ]
    }

Körningstiden som innehåller too50 av hello som senast har slutförts körningar som sorteras i hello omvänd kronologisk ordning (så att hello senaste körning kommer först hello svar).
Mer information om hello svar kan hittas i [Erhåll Status för indexerare](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Indexerare körs enligt ett schema
Du kan även ordna hello indexeraren toorun regelbundet enligt ett schema. toodo detta, Lägg till hello **schema** egenskapen när du skapar eller uppdaterar hello indexeraren. hello exemplet nedan visar en PUT-begäran tooupdate hello indexeraren:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

Hej **intervall** parametern är obligatorisk. hello intervall refererar toohello tiden mellan hello början av två på varandra följande indexeraren körningar. hello minsta tillåtna intervall är 5 minuter. hello längsta är en dag. Den måste formateras som ett XSD ”daytimeduration” XSD-värde (en begränsad delmängd av ett [ISO 8601 varaktighet](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) värdet). hello mönstret för detta är: `P(nD)(T(nH)(nM))`. Exempel: `PT15M` för varje kvart `PT2H` för varannan timme.

hello valfria **startTime** anger när hello schemalagda körningar ska börja. Om det utelämnas används hello aktuella UTC-tid. Nu kan vara i hello tidigare – då hello första körningen har schemalagts som om hello indexeraren har körts sedan hello startTime oavbrutet.  

Endast en körning av en indexerare kan köras samtidigt. Om en indexerare körs när körningen har schemalagts skjuts hello körningen tills hello nästa schemalagda tillfälle.

Vi anser att en exempel-toomake detta mer konkreta. Anta att vi hello följande timschema konfigurerade:

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Här är vad som händer:

1. börjar hello första indexeraren köras på eller runt den 1 mars 2015 klockan 12:00 UTC.
2. Anta att den här körningen tar 20 minuter (eller när mindre än 1 timme).
3. hello andra körningen startar på eller runt den 1 mars 2015 klockan 1:00
4. Anta nu att denna tar mer än en timme – till exempel 70: e minut – så att den är klar cirka 2 10 klockan:
5. Det är nu. 02:00, tid för hello tredje körning toostart. Men eftersom hello andra körning från 1 kl. är fortfarande körs hello tredje körning hoppas över. hello tredje körningen startar klockan 03

Du kan lägga till, ändra eller ta bort ett schema för en befintlig indexerare med hjälp av en **PUT indexeraren** begäran.

<a name="CaptureChangedRows"></a>

## <a name="capture-new-changed-and-deleted-rows"></a>Samla in nya, ändrade och borttagna rader

Azure Search använder **inkrementell indexering** tooavoid med toore-index hello hela tabellen eller vyn varje gång en indexeraren körs. Azure Search innehåller två ändra identifiering principer toosupport inkrementell indexering. 

### <a name="sql-integrated-change-tracking-policy"></a>SQL-integrerad ändringsspårning princip
Om SQL-databasen stöder [ändringsspårning](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server), bör du använda **SQL integrerad ändra Spårningsprincip**. Detta är mest effektiva hello-principen. Dessutom kan Azure Search tooidentify bort rader utan att behöva tooadd en explicit ”mjuk borttagning” kolumnen tooyour tabell.

#### <a name="requirements"></a>Krav 

+ Databaskrav version:
  * SQL Server 2012 SP3 och senare, om du använder SQL Server på Azure Virtual Machines.
  * Azure SQL Database V12, om du använder Azure SQL Database.
+ Tabeller endast (inga vyer). 
+ Hello databasen [Aktivera ändringsspårning](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-tracking-sql-server) för hello tabellen. 
+ Ingen sammansatt primärnyckel (en primär nyckel som innehåller mer än en kolumn) för hello tabellen.  

#### <a name="usage"></a>Användning

toouse principen, skapa eller uppdatera din datakälla Så här:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy"
      }
    }

Ta bort rader med hjälp av SQL-integrerad ändringsspårning princip, ange inte en separat data princip för identifiering av borttagning - principen har inbyggt stöd för att identifiera när. Men för automagically ”hello borttagningar toobe upptäckte” måste hello Dokumentnyckel i search-index vara hello samma som hello primärnyckel i hello SQL-tabell. 

<a name="HighWaterMarkPolicy"></a>

### <a name="high-water-mark-change-detection-policy"></a>Princip för identifiering av ändring av vattenmärke

Princip för den här ändringen är beroende av ett ”vattenmärke” kolumn fånga hello version eller tid när en rad uppdaterades senast. Om du använder en vy, måste du använda en princip för vattenmärke. hello vattenmärke kolumnen måste uppfylla följande krav hello.

#### <a name="requirements"></a>Krav 

* Alla infogningar ange ett värde för hello-kolumn.
* Alla uppdateringar tooan objektet kan du också ändra hello värdet för hello kolumnen.
* hello-värdet för den här kolumnen ökar med varje insert eller update.
* Frågor med hello följande WHERE och ORDER BY-satser kan köras effektivt:`WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`

> [!IMPORTANT] 
> Vi rekommenderar starkt med hello [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) datatypen för hello vattenmärke för kolumnen. Om-datatypen används garanteras inte ändringsspårning toocapture alla ändringar i hello förekomst av transaktioner som körs samtidigt med en indexerare fråga. När du använder **rowversion** i en konfiguration med skrivskyddade repliker, måste du peka hello indexeraren på hello primära repliken. Endast en primär replik kan användas för scenarier för synkronisering av data.

#### <a name="usage"></a>Användning

toouse vattenmärke princip, skapa eller uppdatera din datakälla Så här:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a rowversion or last_updated column name]"
      }
    }

> [!WARNING]
> Om hello källtabellen inte har ett index på hello vattenmärke kolumn får frågor som används av hello SQL indexeraren timeout. I synnerhet hello `ORDER BY [High Water Mark Column]` satsen kräver ett index toorun effektivt när hello tabellen innehåller många rader.
>
>

Om du får timeout-fel, kan du använda hello `queryTimeout` indexeraren configuration inställningen tooset hello tooa frågetimeoutvärdet senare än tidsgränsen för hello standard 5 minuter. Tooset hello timeout too10 minuter om du vill skapa eller uppdatera hello indexeraren med hello följande konfiguration:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

Du kan också inaktivera hello `ORDER BY [High Water Mark Column]` satsen. Men rekommenderas detta inte eftersom om hello indexeraren körningen har avbrutits av ett fel kan hello indexeraren har toore process alla rader om den körs senare – även om hello indexeraren har redan behandlats nästan alla hello rader av hello tid den avbröts. toodisable hello `ORDER BY` -satsen, Använd hello `disableOrderByHighWaterMarkColumn` i hello indexeraren definition:  

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Ej permanent ta bort kolumnen ta bort princip
När rader har tagits bort från hello källtabellen, vill du förmodligen toodelete dessa rader från hello sökindex samt. Om du använder hello SQL-integrerad ändringsspårning princip kan detta har åtgärdat för dig. Hello vattenmärke ändringsspårning principen hjälpa inte dig med borttagna rader. Vilka toodo?

Om fysiskt hello rader har tagits bort från hello tabellen kan har Azure Search inga sätt tooinfer hello förekomst av poster som inte längre finns.  Du kan dock använda hello ”soft-ta bort” tekniken toologically ta bort rader utan att ta bort dem från hello tabell. Lägg till en kolumn tooyour tabell eller vy och markera rader som tas bort med hjälp av kolumnen.

När du använder hello mjuk borttagning tekniken, kan du ange hello princip för mjuk borttagning på följande sätt när du skapar eller uppdaterar hello-datakälla:

    {
        …,
        "dataDeletionDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]",
           "softDeleteMarkerValue" : "[hello value that indicates that a row is deleted]"
        }
    }

Hej **softDeleteMarkerValue** måste vara en sträng – Använd hello strängrepresentation av din faktiskt värde. Till exempel om du har en heltalskolumn där borttagna rader har markerats med hello värdet 1, använda `"1"`. Om du har en BIT-kolumn där borttagna rader har markerats med hello booleska true-värde, Använd `"True"`.

<a name="TypeMapping"></a>

## <a name="mapping-between-sql-and-azure-search-data-types"></a>Mappning mellan datatyperna SQL och Azure Search
| SQL-datatypen | Tillåtna målindexet fälttyp | Anteckningar |
| --- | --- | --- |
| bitar |Edm.Boolean Edm.String | |
| int, smallint, tinyint |Edm.Int32 Edm.Int64, Edm.String | |
| bigint |Edm.Int64 Edm.String | |
| verkliga, float |Edm.Double Edm.String | |
| smallmoney pengar decimal numeriskt |Edm.String |Azure Search har inte stöd för konvertering av decimaltyper till Edm.Double eftersom detta skulle förlora precision |
| char, nchar, varchar, nvarchar |Edm.String<br/>Collection(Edm.String) |En SQL-sträng kan vara används toopopulate Collection(Edm.String) fält om hello sträng som representerar en JSON-matris med strängar:`["red", "white", "blue"]` |
| smalldatetime, datetime, datetime2, date, datetimeoffset |Edm.DateTimeOffset Edm.String | |
| uniqueidentifer |Edm.String | |
| geografisk plats |Edm.GeographyPoint |Endast geografi instanser av typen plats med SRID 4326 (som standard hello) stöds |
| ROWVERSION |Saknas |Radversioner kolumner kan inte lagras i hello sökindex, men de kan användas för ändringsspårning |
| tid, timespan, binary, varbinary, image, xml, geometry, CLR-typer |Saknas |Stöds inte |

## <a name="configuration-settings"></a>Konfigurationsinställningar
SQL-indexeraren visar flera konfigurationsinställningar:

| Inställning | Datatyp | Syfte | Standardvärde |
| --- | --- | --- | --- |
| queryTimeout |Sträng |Anger hello tidsgränsen för körning av SQL-fråga |5 minuter (”00: 05:00”) |
| disableOrderByHighWaterMarkColumn |bool |Gör hello SQL-fråga som används av hello vattenmärke princip tooomit hello ORDER BY-sats. Se [vattenmärke för principen](#HighWaterMarkPolicy) |FALSKT |

De här inställningarna används i hello `parameters.configuration` objekt i hello indexeraren definition. Tooset hello frågan timeout too10 minuter om du vill skapa eller uppdatera hello indexeraren med hello följande konfiguration:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

**F: kan jag använda Azure SQL indexeraren med SQL-databaser som körs på virtuella IaaS-datorer i Azure?**

Ja. Du måste dock tooallow search-tjänsten tooconnect tooyour databasen. Mer information finns i [konfigurera en anslutning från en Server med Azure Search indexeraren tooSQL på en Azure VM](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).

**F: kan jag använda Azure SQL indexeraren med SQL-databaser som körs lokalt?**

Inte direkt. Vi rekommenderar eller inte stöd för en direkt anslutning som gör så kräver du tooopen databaser tooInternet trafiken. Kunder har har slutförts med det här scenariot med bridge tekniker som Azure Data Factory. Mer information finns i [Push data tooan Azure Search index med Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-azure-search-connector).

**F: kan jag använda Azure SQL indexeraren med databaser än SQL Server körs i IaaS på Azure?**

Nej. Vi stöder inte det här scenariot eftersom vi inte har testat hello indexerare med alla databaser än SQL Server.  

**F: kan jag skapa flera indexerare som körs på ett schema?**

Ja. Dock kan bara en indexerare köras på en nod i taget. Överväg att skala upp din sökning service toomore än en sökning-enhet om du behöver flera indexerare som körs samtidigt.

**F: kan körning påverkar en indexerare min fråga?**

Ja. Indexeraren körs på en av noderna hello i din söktjänst och den noden resurser delas mellan indexering och hanterar frågetrafik och andra API-begäranden. Om du kör indexering och fråga arbetsbelastningar och påträffar en hög andel 503 fel eller ökande svarstider, bör du [skala upp din söktjänst](search-capacity-planning.md).

**F: kan jag använda en sekundär replik i en [redundanskluster](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) som en datakälla?**

Det beror på. Du kan använda en sekundär replik fullständig indexering av en tabell eller vy. 

Azure Search stöder två ändra principer för inkrementell indexering: SQL-integrerad ändra spårnings- och vattenmärke.

SQL-databasen stöder inte integrerad ändringsspårning på skrivskyddade repliker. Därför måste du använda vattenmärke för principen. 

Vår rekommendation som standard är toouse hello rowversion datatyp för hello vattenmärke för kolumnen. Men med rowversion bygger på SQL-databas `MIN_ACTIVE_ROWVERSION` funktion, vilket inte stöds på skrivskyddade repliker. Därför måste du peka hello indexeraren tooa primära repliken om du använder rowversion.

Om du försöker toouse rowversion på en skrivskyddad replik visas hello följande fel: 

    "Using a rowversion column for change tracking is not supported on secondary (read-only) availability replicas. Please update hello datasource and specify a connection toohello primary availability replica.Current database 'Updateability' property is 'READ_ONLY'".

**F: kan jag använda en alternativ, icke-rowversion kolumn för vattenmärke för ändringsspårning?**

Det rekommenderas inte. Endast **rowversion** gör det möjligt för tillförlitlig datasynkronisering. Beroende på din programlogik, det kan dock finnas säker om:

+ Du kan se till att när hello indexeraren körs, det finns inga väntande transaktioner för hello-tabell som indexeras (till exempel alla uppdateringar inträffa som en batch enligt ett schema och hello Azure Search indexerschemat anges tooavoid överlappar hello tabell uppdatera schemat).  

+ Du göra regelbundet en fullständig omindexera toopick in några saknade rader. 
