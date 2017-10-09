---
title: "aaaCopy data till/från Azure SQL Data Warehouse | Microsoft Docs"
description: "Lär dig hur toocopy data till/från Azure SQL Data Warehouse med Azure Data Factory"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: d90fa9bd-4b79-458a-8d40-e896835cfd4a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 75bfcf3c99844fc1297ca500107da23cf875e41f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-azure-sql-data-warehouse-using-azure-data-factory"></a>Kopiera data tooand från Azure SQL Data Warehouse med Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data till och från Azure SQL Data Warehouse. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.  

> [!TIP]
> tooachieve bästa prestanda, använda PolyBase tooload data till Azure SQL Data Warehouse. Hej [Använd PolyBase tooload data till Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) avsnitt innehåller information. En genomgång med ett användningsfall finns [läsa in 1 TB i Azure SQL Data Warehouse under 15 minuter med Azure Data Factory](data-factory-load-sql-data-warehouse.md).

## <a name="supported-scenarios"></a>Scenarier som stöds
Du kan kopiera data **från Azure SQL Data Warehouse** toohello följande datakällor:

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

Du kan kopiera data från hello följande datalager **tooAzure SQL Data Warehouse**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!TIP]
> När du kopierar data från SQL Server eller Azure SQL Database tooAzure kan SQL Data Warehouse, om hello tabellen inte finns i hello målarkivet, Data Factory automatiskt skapa hello tabell i SQL Data Warehouse med hjälp av hello schemat för hello tabell i hello källan datalagret. Se [automatiskt skapande av tabell](#auto-table-creation) mer information.

## <a name="supported-authentication-type"></a>Autentiseringstypen som stöds
Azure SQL Data Warehouse connector stöder grundläggande autentisering.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till och från ett Azure SQL Data Warehouse med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline som kopierar data till och från Azure SQL Data Warehouse är guiden för toouse hello kopiera data. Se [Självstudier: Läs in data till SQL Data Warehouse med Data Factory](../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:

1. Skapa en **datafabriken**. En datafabrik kan innehålla en eller flera pipelines. 
2. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory. Till exempel om du kopierar data från ett Azure blob storage tooan Azure SQL data warehouse skapa du två länkade tjänster toolink dina Azure storage-konto och Azure SQL data warehouse tooyour data factory. Länkad tjänstegenskaper som är specifika tooAzure SQL Data Warehouse, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt. 
3. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello blob-behållaren och mappen som innehåller hello indata. Och du skapar en annan dataset toospecify hello tabell i hello Azure SQL data warehouse som innehåller hello data som kopieras från hello blob storage. Egenskaper för datamängd som är specifika tooAzure SQL Data Warehouse, se [egenskaper för datamängd](#dataset-properties) avsnitt.
4. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. I hello-exemplet ovan, använder du BlobSource som en källa och SqlDWSink som en mottagare för hello kopieringsaktiviteten. På samma sätt om du vill kopiera från Azure SQL Data Warehouse tooAzure Blob Storage, använder du SqlDWSource och BlobSink i hello kopieringsaktiviteten. Kopiera Aktivitetsegenskaper som är specifika tooAzure SQL Data Warehouse, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt. Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager.

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från en Azure SQL Data Warehouse finns [JSON-exempel](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) i den här artikeln.

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAzure SQL Data Warehouse:

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello följande tabell ger en beskrivning för JSON-element specifika tooAzure SQL Data Warehouse länkade tjänsten.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello Typegenskapen måste anges till: **AzureSqlDW** |Ja |
| connectionString |Ange nödvändig information tooconnect toohello Azure SQL Data Warehouse-instans för hello-egenskapen connectionString. Endast grundläggande autentisering stöds. |Ja |

> [!IMPORTANT]
> Konfigurera [Azure SQL Database-brandvägg](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) och hello databasserver för[Tillåt Azure Services tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Om du kopierar data tooAzure SQL Data Warehouse från utanför Azure inklusive från lokala datakällor med data factory-gateway måste du dessutom konfigurera rätt IP-adressintervall för hello-dator som skickar data tooAzure SQL Data Warehouse.

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).

hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej **typeProperties** avsnittet för hello dataset av typen **AzureSqlDWTable** har hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabellen eller vyn i hello Azure SQL Data Warehouse-databas som hello länkade tjänsten refererar till. |Ja |

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.

> [!NOTE]
> Hej Kopieringsaktiviteten tar endast en inmatning och ger en enda utdata.

De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

### <a name="sqldwsource"></a>SqlDWSource
När datakällan är av typen **SqlDWSource**, hello följande egenskaper är tillgängliga i **typeProperties** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| sqlReaderQuery |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: Välj * från mytable prefix. |Nej |
| sqlReaderStoredProcedureName |Namnet på hello lagrad procedur som läser data från hello källtabellen. |Namnet på hello lagrad procedur. hello senaste SQL-instruktionen måste vara en SELECT-instruktion i hello lagrad procedur. |Nej |
| storedProcedureParameters |Parametrar för hello lagrad procedur. |Namn/värde-par. Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar. |Nej |

Om hello **sqlReaderQuery** har angetts för hello SqlDWSource, hello Kopieringsaktiviteten kör den här frågan mot hello Azure SQL Data Warehouse källdata tooget hello.

Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).

Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName är hello kolumner som anges i hello strukturen i hello dataset JSON används toobuild en fråga toorun mot hello Azure SQL Data Warehouse. Exempel: `select column1, column2 from mytable`. Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.

#### <a name="sqldwsource-example"></a>SqlDWSource-exempel

```JSON
"source": {
    "type": "SqlDWSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```
**definition av hello lagrade proceduren:**

```SQL
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sqldwsink"></a>SqlDWSink
**SqlDWSink** stöder hello följande egenskaper:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Ange en fråga för aktiviteten kopiera tooexecute så att data för ett visst segment har rensats bort. Mer information finns i [repeterbarhet avsnittet](#repeatability-during-copy). |En frågesats. |Nej |
| allowPolyBase |Anger om toouse PolyBase (i förekommande fall) i stället för BULKINSERT mekanism. <br/><br/> **Hello rekommenderat sätt tooload data till SQL Data Warehouse är med PolyBase.** Se [Använd PolyBase tooload data till Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) avsnittet begränsningar och information. |True <br/>FALSKT (standard) |Nej |
| polyBaseSettings |En grupp egenskaper som kan anges när hello **allowPolybase** egenskapen för**SANT**. |&nbsp; |Nej |
| rejectValue |Anger antalet hello eller procentandelen rader som kan avvisas innan hello frågan inte kunde köras. <br/><br/>Lär dig mer om hello PolyBase avvisa alternativ i hello **argument** avsnitt i [Skapa extern tabell (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) avsnittet. |0 (standard), 1, 2... |Nej |
| rejectType |Anger om hello rejectValue alternativet har angetts som ett litteralvärde eller ett procentvärde. |Värdet (standard), procent |Nej |
| rejectSampleValue |Anger hello antalet rader tooretrieve innan hello PolyBase beräknar hello procentandelen Avvisade rader. |1, 2, … |Ja, om **rejectType** är **procent** |
| useTypeDefault |Anger hur toohandle saknar värden i avgränsade textfiler när PolyBase hämtar data från hello textfil.<br/><br/>Mer information om den här egenskapen från hello argument-avsnittet i [skapa externt FILFORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). |SANT, FALSKT (standard) |Nej |
| writeBatchSize |Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize |Heltal (antalet rader) |Nej (standard: 10000) |
| writeBatchTimeout |Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås. |TimeSpan<br/><br/> Exempel ”: 00: 30:00” (30 minuter). |Nej |

#### <a name="sqldwsink-example"></a>SqlDWSink-exempel

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true
}
```

## <a name="use-polybase-tooload-data-into-azure-sql-data-warehouse"></a>Använd PolyBase tooload data till Azure SQL Data Warehouse
Med hjälp av  **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)**  är ett effektivt sätt för att läsa in stora mängder data till Azure SQL Data Warehouse med hög genomströmning. Du kan se en stor vinst i hello dataflöde med PolyBase i stället för hello BULKINSERT standardmekanism. Se [kopiera prestanda referensnummer](data-factory-copy-activity-performance.md#performance-reference) med detaljerad jämförelse. En genomgång med ett användningsfall finns [läsa in 1 TB i Azure SQL Data Warehouse under 15 minuter med Azure Data Factory](data-factory-load-sql-data-warehouse.md).

* Om datakällan finns i **Azure Blob- eller Azure Data Lake Store**, och hello format är kompatibelt med PolyBase, du kan kopiera tooAzure SQL Data Warehouse med PolyBase direkt. Se  **[direkt kopia med PolyBase](#direct-copy-using-polybase)**  med information.
* Om din käll-datalagret och format inte stöds ursprungligen av PolyBase, kan du använda hello  **[mellanlagrad kopia med PolyBase](#staged-copy-using-polybase)**  funktion i stället. Det ger dig bättre genomströmning genom att automatiskt konvertera hello data till PolyBase-kompatibelt format och lagra hello data i Azure Blob storage. Data hämtas sedan till SQL Data Warehouse.

Ange hello `allowPolyBase` egenskapen för**SANT** som visas i följande exempel för Azure Data Factory toouse PolyBase toocopy data till Azure SQL Data Warehouse hello. När du ställer in allowPolyBase tootrue, anger du PolyBase specifika egenskaper med hello `polyBaseSettings` egenskapsgrupp. Se hello [SqlDWSink](#SqlDWSink) finns mer information om egenskaper som du kan använda med polyBaseSettings.

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

### <a name="direct-copy-using-polybase"></a>Direkt kopia med PolyBase
SQL Data Warehouse PolyBase stöd direkt för Azure Blob och Azure Data Lake Store (med tjänstens huvudnamn) som källa och krav för fil-format. Om alla källdata uppfyller hello kriterier som beskrivs i det här avsnittet, kan du direkt kopiera från källan data store tooAzure SQL Data Warehouse med PolyBase. Annars kan du använda [mellanlagrad kopia med PolyBase](#staged-copy-using-polybase).

> [!TIP]
> toocopy data från Data Lake Store tooSQL informationslager effektivt, mer information från [Azure Data Factory gör det även enklare och smidig toouncover insikter från data när du använder Data Lake Store med SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Om hello krav inte uppfylls, Azure Data Factory kontrollerar hello inställningar och faller automatiskt tillbaka toohello BULKINSERT mekanism för hello dataflyttning.

1. **Källan länkade tjänsten** är av typen: **AzureStorage** eller **AzureDataLakeStore med huvudsakliga autentiseringen av tjänsten**.  
2. Hej **inkommande dataset** är av typen: **AzureBlob** eller **AzureDataLakeStore**, och hello formattyp under `type` egenskaper är **OrcFormat** , eller **TextFormat** med hello följande konfigurationer:

   1. `rowDelimiter`måste vara  **\n** .
   2. `nullValue`har angetts för**tom sträng** (””), eller `treatEmptyAsNull` har angetts för**SANT**.
   3. `encodingName`har angetts för**utf-8**, vilket är **standard** värde.
   4. `escapeChar`, `quoteChar`, `firstRowAsHeader`, och `skipLineCount` har inte angetts.
   5. `compression`kan vara **ingen komprimering**, **GZip**, eller **Deflate**.

    ```JSON
    "typeProperties": {
       "folderPath": "<blobpath>",
       "format": {
           "type": "TextFormat",     
           "columnDelimiter": "<any delimiter>",
           "rowDelimiter": "\n",       
           "nullValue": "",           
           "encodingName": "utf-8"    
       },
       "compression": {  
           "type": "GZip",  
           "level": "Optimal"  
       }  
    },
    ```

3. Det finns inga `skipHeaderLineCount` inställningen **BlobSource** eller **AzureDataLakeStore** för hello kopieringsaktiviteten i hello pipeline.
4. Det finns inga `sliceIdentifierColumnName` inställningen **SqlDWSink** för hello kopieringsaktiviteten i hello pipeline. (PolyBase garanterar att alla data har uppdaterats eller ingenting uppdateras i en enda körning. tooachieve **repeterbarhet**, du kan använda `sqlWriterCleanupScript`).
5. Det finns ingen `columnMapping` som används i hello associerade i en Kopieringsaktivitet.

### <a name="staged-copy-using-polybase"></a>Stegvis kopia med PolyBase
När källdata inte uppfyller hello villkor som introducerades i hello föregående avsnitt, kan du Aktivera kopiering av data via en mellanliggande fristående Azure Blob Storage (inte får vara Premium-lagring). I det här fallet Azure Data Factory automatiskt utför omformningar på hello data toomeet data format krav PolyBase och sedan använda PolyBase tooload data till SQL Data Warehouse och vid senaste Rensa temp data från hello Blob storage. Se [mellanlagrad kopiera](data-factory-copy-activity-performance.md#staged-copy) för information om hur kopiering av data via en fristående Azure-Blob fungerar i allmänhet.

> [!NOTE]
> Vid kopiering av data från en lokal lagring till Azure SQL Data Warehouse med PolyBase och mellanlagring, om Data Management Gateway-version som är lägre än 2.4 JRE (Java Runtime Environment) krävs för din gateway datorn som är används tootransform källdata i rätt format. Rekommenderar att du uppgraderar din gateway toohello senaste tooavoid sådana beroende.
>

toouse denna funktion, skapa en [Azure länkade lagringstjänsten](data-factory-azure-blob-connector.md#azure-storage-linked-service) som refererar toohello Azure Storage-konto som har hello tillfälliga blob-lagring, ange hello `enableStaging` och `stagingSettings` egenskaper för hello Kopieringsaktiviteten enligt hello följande kod:

```json
"activities":[  
{
    "name": "Sample copy activity from SQL Server tooSQL Data Warehouse via PolyBase",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDWOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlDwSink",
            "allowPolyBase": true
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob"
        }
    }
}
]
```

## <a name="best-practices-when-using-polybase"></a>Rekommenderade metoder när du använder PolyBase
hello följande avsnitt innehåller ytterligare bästa praxis toohello de som nämns i [Metodtips för Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Behörighet krävs
toouse PolyBase kräver hello användare att använda tooload data till SQL Data Warehouse har hello [”behörighet”](https://msdn.microsoft.com/library/ms191291.aspx) på hello måldatabasen. Enkelriktade tooachieve som är tooadd användaren som en medlem i ”db_owner”. Lär dig hur toodo som genom att följa [i det här avsnittet](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limitation"></a>Ange begränsning Radstorleken och data
Polybase belastningar begränsas tooloading rader både mindre än **1 MB** och det går inte att läsa in tooVARCHR(MAX), NVARCHAR(MAX) eller VARBINARY(MAX). Se för[här](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Om du har källdata med rader av större än 1 MB kan kanske du vill toosplit hello källtabellerna lodrätt i flera små om hello största Radstorleken på var och en av dem inte överstiger gränsen hello. hello mindre tabeller kan sedan läsas med PolyBase och sammanfogas tillsammans i Azure SQL Data Warehouse.

### <a name="sql-data-warehouse-resource-class"></a>SQL Data Warehouse resursklassen
tooachieve bästa möjliga genomströmningen, Överväg tooassign större resurs klassen toohello användaren som används tooload data till SQL Data Warehouse via PolyBase. Lär dig hur toodo som genom att följa [ändra ett exempel på användaren resurs klassen](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).

### <a name="tablename-in-azure-sql-data-warehouse"></a>Tabellnamn i Azure SQL Data Warehouse
hello följande tabell innehåller exempel på hur toospecify hello **tableName** egenskap i dataset JSON för olika kombinationer av schema och tabellnamn.

| DB-Schema | Tabellnamnet | tableName JSON-egenskapen |
| --- | --- | --- |
| dbo |Mytable prefix |Mytable prefix eller dbo. Mytable prefix eller [dbo]. [MyTable] |
| dbo1 |Mytable prefix |dbo1. Mytable prefix eller [dbo1]. [MyTable] |
| dbo |My.Table |[My.Table] eller [dbo]. [My.Table] |
| dbo1 |My.Table |[dbo1]. [My.Table] |

Det kan vara ett problem med hello-värde som du angav för hello tableName-egenskapen om hello följande fel visas. Se hello tabellen för hello rätt metod toospecify värden för hello tableName JSON-egenskapen.  

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Kolumner med standardvärden
För närvarande PolyBase-funktionen i Data Factory accepterar bara hello samma antal kolumner som hello måltabellen. Att du har en tabell med fyra kolumner och en av dem har definierats med ett standardvärde. hello-indata måste fortfarande innehålla fyra kolumner. Att tillhandahålla en inkommande dataset 3 kolumner ger ett felmeddelande liknande toohello följande meddelande:

```
All columns of hello table must be specified in hello INSERT BULK statement.
```
NULL-värde är en speciell typ av standardvärdet. Om hello kolumnen är nullbar hello indata (i blob) för kolumnen vara tom (kan inte vara saknas från hello inkommande datamängd). PolyBase infogas NULL för dem i hello Azure SQL Data Warehouse.  

## <a name="auto-table-creation"></a>Automatiskt skapande av tabell
Om du använder guiden Kopiera toocopy data från SQL Server eller Azure SQL Database tooAzure SQL Data Warehouse och hello tabell som motsvarar toohello källtabellen inte finns i hello målarkivet skapa Data Factory automatiskt hello tabell i hello datalager med hjälp av hello källa tabellens schema.

Data Factory skapar hello tabell i hello målarkivet med hello samma tabellnamn i datalagret för hello källa. hello-datatyper för kolumner väljs utifrån hello följande mappning. Om det behövs, utför typen konverteringar toofix alla inkompatibiliteter mellan käll- och Arkiv. Dessutom används resursallokering tabell distribution.

| Typ av kolumn SQL-databas | Måltypen SQL DW kolumn (storleksbegränsning) |
| --- | --- |
| int | int |
| BigInt | BigInt |
| SmallInt | SmallInt |
| TinyInt | TinyInt |
| bitar | bitar |
| Decimal | Decimal |
| numeriskt | Decimal |
| flyttal | flyttal |
| Money | Money |
| Real | Real |
| SmallMoney | SmallMoney |
| Binär | Binär |
| varbinary | Varbinary (upp too8000) |
| Date | Date |
| Datum och tid | Datum och tid |
| DateTime2 | DateTime2 |
| Tid | Tid |
| DateTimeOffset | DateTimeOffset |
| SmallDateTime | SmallDateTime |
| Text | Varchar (upp too8000) |
| NText | NVarChar (upp too4000) |
| Bild | VarBinary (upp too8000) |
| Unik identifierare | Unik identifierare |
| Char | Char |
| NChar | NChar |
| VarChar | VarChar (upp too8000) |
| NVarChar | NVarChar (upp too4000) |
| XML | Varchar (upp too8000) |

[!INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)]

## <a name="type-mapping-for-azure-sql-data-warehouse"></a>Mappning för Azure SQL Data Warehouse
Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:

1. Konvertera från inbyggda typer too.NET källtypen
2. Konvertera från .NET typen toonative Mottagartypen

När du flyttar data & från Azure SQL Data Warehouse, används hello följande mappningar från SQL too.NET typ och vice versa.

hello-mappning är samma som hello [Datatypsmappningen i SQL Server för ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).

| SQL Server Database Engine-typ | .NET framework-typ |
| --- | --- |
| bigint |Int64 |
| Binär |byte] |
| bitar |Booleskt värde |
| Char |Sträng, Char] |
| Datum |Datum och tid |
| Datum och tid |Datum och tid |
| datetime2 |Datum och tid |
| DateTimeOffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM-attributet (varbinary(max)) |byte] |
| flyttal |dubbla |
| Bild |byte] |
| int |Int32 |
| Money |Decimal |
| nchar |Sträng, Char] |
| ntext |Sträng, Char] |
| numeriskt |Decimal |
| nvarchar |Sträng, Char] |
| Verklig |Enskild |
| ROWVERSION |byte] |
| smalldatetime |Datum och tid |
| smallint |Int16 |
| smallmoney |Decimal |
| sql_variant |Objektet * |
| Text |Sträng, Char] |
| time |TimeSpan |
| tidsstämpel |byte] |
| tinyint |Mottagna byte |
| Unik identifierare |GUID |
| varbinary |byte] |
| varchar |Sträng, Char] |
| xml |XML |

Du kan också mappa kolumner från källan dataset toocolumns från sink datauppsättning i aktivitetsdefinitionen för hello kopia. Mer information finns i [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="json-examples-for-copying-data-tooand-from-sql-data-warehouse"></a>JSON-exempel för att kopiera data tooand från SQL Data Warehouse
hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De visar hur toocopy data tooand från Azure SQL Data Warehouse och Azure Blob Storage. Dock datan kan kopieras **direkt** från alla källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.

### <a name="example-copy-data-from-azure-sql-data-warehouse-tooazure-blob"></a>Exempel: Kopiera data från Azure SQL Data Warehouse tooAzure Blob
hello exemplet definierar hello följande Data Factory-enheter:

1. En länkad tjänst av typen [AzureSqlDW](#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [AzureSqlDWTable](#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [SqlDWSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar tidsserier (varje timme, dag, etc.) data från en tabell i Azure SQL Data Warehouse-databas tooa blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

**Azure SQL Data Warehouse länkade tjänsten:**

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Blob storage länkade tjänsten:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure SQL Data Warehouse-indatauppsättningen:**

hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Azure SQL Data Warehouse och innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.

Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
{
  "name": "AzureSqlDWInput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Azure Blob utdatauppsättningen:**

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Kopiera aktivitet i en pipeline med SqlDWSource och BlobSink:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**SqlDWSource** och **sink** typ har angetts för**BlobSink**. hello SQL-frågan som angetts för hello **SqlReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLDWtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSqlDWInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlDWSource",
            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```
> [!NOTE]
> I exemplet hello **sqlReaderQuery** har angetts för hello SqlDWSource. Hej Kopieringsaktiviteten kör den här frågan mot hello Azure SQL Data Warehouse källdata tooget hello.
>
> Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).
>
> Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName hello kolumner som anges i hello strukturen i hello dataset JSON är används toobuild en fråga (Välj kolumn1, kolumn2 från mytable prefix) toorun mot hello Azure SQL Data Warehouse. Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.
>
>

### <a name="example-copy-data-from-azure-blob-tooazure-sql-data-warehouse"></a>Exempel: Kopiera data från Azure Blob tooAzure SQL Data Warehouse
hello exemplet definierar hello följande Data Factory-enheter:

1. En länkad tjänst av typen [AzureSqlDW](#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureSqlDWTable](#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [SqlDWSink](#copy-activity-properties).

hello exemplet kopierar time series-data (varje timme, varje dag, etc.) från Azure blob tooa tabell i Azure SQL Data Warehouse-databas i timmen. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

**Azure SQL Data Warehouse länkade tjänsten:**

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Blob storage länkade tjänsten:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure Blob inkommande datamängd:**

Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1). hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad och som en dagdel av hello starttid och filnamn använder hello timme tillhör hello starttid. ”externa”: ”true” inställningen informerar hello Data Factory-tjänsten att den här tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Azure SQL Data Warehouse utdatauppsättningen:**

hello exemplet kopierar data tooa tabell med namnet ”mytable” som prefix i Azure SQL Data Warehouse. Skapa hello tabell i Azure SQL Data Warehouse med hello samma antal kolumner som du förväntar dig hello Blob CSV-filen toocontain. Nya rader läggs toohello tabell varje timme.

```JSON
{
  "name": "AzureSqlDWOutput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Kopiera aktivitet i en pipeline med BlobSource och SqlDWSink:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och **sink** typ har angetts för**SqlDWSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQLDW",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlDWOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlDWSink",
            "allowPolyBase": true
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```
En genomgång finns hello finns [läsa in 1 TB i Azure SQL Data Warehouse under 15 minuter med Azure Data Factory](data-factory-load-sql-data-warehouse.md) och [Läs in data med Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) artikeln i hello Azure SQL Data Warehouse dokumentation.

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.
