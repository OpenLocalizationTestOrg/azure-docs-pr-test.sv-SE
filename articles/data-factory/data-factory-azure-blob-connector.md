---
title: "aaaCopy data till/från Azure Blob Storage | Microsoft Docs"
description: "Lär dig hur toocopy blob-data i Azure Data Factory. Använda våra exempel: hur toocopy data tooand från Azure Blob Storage och Azure SQL Database."
keywords: BLOB-data, azure blob-kopia
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: bec8160f-5e07-47e4-8ee1-ebb14cfb805d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jingwang
ms.openlocfilehash: 8428c64e8e8b1084b3f2f680c4e1819559e4ffa3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooor-from-azure-blob-storage-using-azure-data-factory"></a>Kopiera data tooor från Azure Blob Storage med hjälp av Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toocopy data tooand från Azure Blob Storage. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.

## <a name="overview"></a>Översikt
Du kan kopiera data från en stöds källa data tooAzure Blob Storage eller Azure Blob Storage tooany stöds sink data. hello följande tabell innehåller en lista över datakällor som stöds som källor eller egenskaperna av hello kopieringsaktiviteten. Du kan till exempel flytta data **från** en SQL Server-databas eller en Azure SQL database **till** en Azure-blobblagring. Och du kan kopiera data **från** Azure-blobblagring **till** en Azure SQL Data Warehouse eller en Azure DB som Cosmos-samling. 

## <a name="supported-scenarios"></a>Scenarier som stöds
Du kan kopiera data **från Azure Blob Storage** toohello följande datakällor:

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

Du kan kopiera data från hello följande datalager **tooAzure Blob Storage**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]
 
> [!IMPORTANT]
> Kopieringsaktiviteten stöder kopiering av data från / tooboth allmänna Azure Storage-konton och Hot/kall Blob storage. hello aktiviteten stöder **läsning från block, Lägg till eller sidblobbar**, men stöder **skriva tooonly blockblobbar**. Azure Premium-lagring stöds inte som en mottagare eftersom den backas upp av sidblobar.
> 
> Kopieringsaktiviteten tar inte bort data från hello källa när hello data är har kopierats toohello mål. Om du behöver toodelete källdata efter en lyckad kopiering, skapa en [anpassad aktivitet](data-factory-use-custom-activities.md) toodelete hello data och använda hello aktivitet i hello pipeline. Ett exempel finns hello [ta bort blob- eller mappnamn prov på GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity). 

## <a name="get-started"></a>Kom igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till och från ett Azure Blob Storage med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Den här artikeln innehåller en [genomgången](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) för att skapa en pipeline toocopy data från ett Azure Blob Storage plats tooanother Azure Blob Storage-plats. En självstudiekurs om hur du skapar en pipeline toocopy data från ett Azure Blob Storage tooAzure SQL-databas finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md).

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:

1. Skapa en **datafabriken**. En datafabrik kan innehålla en eller flera pipelines. 
2. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory. Till exempel om du kopierar data från en Azure blob storage tooan Azure SQL database skapa du två länkade tjänster toolink dina Azure storage-konto och Azure SQL database tooyour data factory. Länkad tjänstegenskaper som är specifika tooAzure Blob Storage, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt. 
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello blob-behållaren och mappen som innehåller hello indata. Och du skapar en annan dataset toospecify hello SQL-tabellen i hello Azure SQL-databas som innehåller hello data som kopieras från hello blob storage. Egenskaper för datamängd som är specifika tooAzure Blob Storage, se [egenskaper för datamängd](#dataset-properties) avsnitt.
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. I hello-exemplet ovan, använder du BlobSource som en källa och SqlSink som en mottagare för hello kopieringsaktiviteten. På samma sätt om du vill kopiera från Azure SQL Database tooAzure Blob Storage, använder du SqlSource och BlobSink i hello kopieringsaktiviteten. Kopiera Aktivitetsegenskaper som är specifika tooAzure Blob Storage, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt. Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager.  

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från en Azure Blob Storage finns [JSON-exempel](#json-examples-for-copying-data-to-and-from-blob-storage  ) i den här artikeln.

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAzure Blob Storage.

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
Det finns två typer av länkade tjänster kan du använda toolink ett Azure Storage tooan Azure data factory. De är: **AzureStorage** länkade tjänsten och **AzureStorageSas** länkade tjänsten. hello länkad Azure Storage-tjänst ger hello data factory med global åtkomst toohello Azure Storage. Medan hello Azure Storage SAS (signatur för delad åtkomst) länkad ger tjänst hello data factory med begränsad/Tidsbundna åtkomst toohello Azure Storage. Det finns några skillnader mellan dessa två länkade tjänster. Välj hello länkade tjänst som passar dina behov. hello följande avsnitt innehåller mer information om dessa två länkade tjänster.

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Egenskaper för datamängd
toospecify en dataset toorepresent inkommande eller utgående data i ett Azure Blob Storage, anger du egenskapen hello typ av hello datamängden: **AzureBlob**. Ange hello **linkedServiceName** -egenskapen för hello dataset toohello namnet på hello Azure Storage eller Azure Storage SAS länkade tjänsten.  hello egenskaper i hello dataset ange hello **blobbehållaren** och hello **mappen** i hello blob storage.

En fullständig lista över JSON avsnitt & egenskaper som är tillgängliga för att definiera datauppsättningar finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).

Data factory stöder hello följande CLS-kompatibelt .NET baserat värden av typen för att ange information i ”struktur” för schemat på Läs datakällor som Azure blob: Int16, Int32, Int64, enskild, Double, Decimal, Byte [], Bool, String, Guid, Datetime, DateTimeOffset Timespan. Data Factory utför konverteringar automatiskt när du flyttar från en källdata datalager tooa sink-datalagret.

Hej **typeProperties** avsnitt är olika för varje typ av dataset och innehåller information om hello plats, format, etc. hello data i datalagret hello. Hej typeProperties avsnittet för dataset av typen **AzureBlob** datamängden har hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| folderPath |Sökvägen toohello behållaren och mappen i hello blob storage. Exempel: myblobcontainer\myblobfolder\ |Ja |
| fileName |Namnet på hello-blob. Filnamnet är valfria och skiftlägeskänsligt.<br/><br/>Om du anger ett filnamn, hello aktivitet (inklusive kopia) fungerar på hello specifika Blob.<br/><br/>Om filnamnet har angetts innehåller kopiera alla BLOB i hello folderPath för inkommande dataset.<br/><br/>När **fileName** har inte angetts för en datamängd för utdata och **preserveHierarchy** har inte angetts i aktiviteten sink hello namnet på hello genereras skulle vara i hello efter det här formatet: Data.<Guid>. txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nej |
| partitionedBy |partitionedBy är en valfri egenskap. Du kan använda det toospecify dynamiska folderPath och filnamnet för tid series-data. Exempelvis kan folderPath parameteriseras för varje timme av data. Se hello [med partitionedBy egenskap](#using-partitionedBy-property) information och exempel. |Nej |
| Format | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Ange hello **typen** egenskap under format tooone av dessa värden. Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. <br><br> Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner. |Nej |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**. Nivåer som stöds är: **Optimal** och **snabbast**. Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |

### <a name="using-partitionedby-property"></a>Med hjälp av partitionedBy egenskap
Som nämnts tidigare under hello, du kan ange en dynamisk folderPath och ett filnamn för tid series-data med hello **partitionedBy** egenskapen [Data Factory-funktioner och hello systemvariabler](data-factory-functions-variables.md).

Mer information om tid serien datauppsättningar, schemaläggning och segment finns [skapa datauppsättningar](data-factory-create-datasets.md) och [schemaläggning och utförande](data-factory-scheduling-and-execution.md) artiklar.

#### <a name="sample-1"></a>Exempel 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

I det här exemplet {segment} ersätts med hello värde för Data Factory systemvariabel SliceStart hello-format (YYYYMMDDHH). Hej SliceStart refererar toostart tiden för hello sektorn. hello folderPath är olika för varje segment. Till exempel: 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout

#### <a name="sample-2"></a>Exempel 2

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

I det här exemplet extraheras år, månad, dag och tidpunkt för SliceStart till olika variabler som används av egenskaperna folderPath och filnamn.

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, indata och utdata-datauppsättningar och -principer är tillgängliga för alla typer av aktiviteter. Medan egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor. Om du flyttar data från ett Azure Blob Storage måste du ställa in hello källtypen i hello kopieringsaktiviteten också**BlobSource**. På samma sätt om du flyttar data tooan Azure Blob Storage måste du ange hello Mottagartypen i hello kopieringsaktiviteten för**BlobSink**. Det här avsnittet innehåller en lista över egenskaper som stöds av BlobSource och BlobSink.

**BlobSource** stöder följande egenskaper i hello hello **typeProperties** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| Rekursiva |Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen. |SANT (standardvärdet), FALSKT |Nej |

**BlobSink** stöder följande egenskaper hello **typeProperties** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| copyBehavior |Definierar hello kopiera beteende när hello källa är BlobSource eller filsystem. |<b>PreserveHierarchy</b>: bevarar hello filstruktur i hello målmapp. hello relativa sökvägen till källmappen för filen toosource är identiska toohello relativa sökvägen till filen tootarget mapp.<br/><br/><b>FlattenHierarchy</b>: alla filer från källmappen hello finns i hello först för målmappen. hello målfilerna har genereras automatiskt namn. <br/><br/><b>MergeFiles</b>: sammanfogar alla filer från hello källfil mappen tooone. Om hello fil/Blobbnamnet anges skulle hello kopplade filnamnet vara hello angivna name; annars skulle vara automatiskt genererade filnamn. |Nej |

**BlobSource** stöder också de här två egenskaperna för bakåtkompatibilitet.

* **treatEmptyAsNull**: Anger om tootreat null eller en tom sträng som null-värde.
* **skipHeaderLineCount** – anger hur många rader måste hoppas över. Det gäller endast när inkommande dataset använder TextFormat.

På liknande sätt **BlobSink** stöder hello efter egenskapen för bakåtkompatibilitet.

* **blobWriterAddHeader**: Anger om tooadd ett huvud för kolumndefinitionerna vid skrivning till tooan utdatauppsättningen.

Följande egenskaper som implementerar datauppsättningar nu support hello hello samma funktion: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.

hello ger följande tabell vägledning om hur du använder hello nya egenskaper för datamängd i stället för dessa källor/mottagare blob-egenskaper.

| Kopiera aktivitetsegenskap | Egenskapen DataSet |
|:--- |:--- |
| skipHeaderLineCount på BlobSource |skipLineCount och firstRowAsHeader. Rader hoppas över först och sedan hello första rad läses som en rubrik. |
| treatEmptyAsNull på BlobSource |treatEmptyAsNull för inkommande datauppsättningen |
| blobWriterAddHeader på BlobSink |firstRowAsHeader på datamängd för utdata |

Se [anger TextFormat](data-factory-supported-file-and-compression-formats.md#text-format) för detaljerad information om dessa egenskaper.    

### <a name="recursive-and-copybehavior-examples"></a>rekursiva och copyBehavior exempel
Det här avsnittet beskrivs hello resultatet av hello kopia för olika kombinationer av värden rekursiv och copyBehavior.

| Rekursiva | copyBehavior | Resultatet |
| --- | --- | --- |
| SANT |preserveHierarchy |För en källmapp Mapp1 med hello följande struktur: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello målmappen Mapp1 skapas med hello samma struktur som hello källa<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| SANT |flattenHierarchy |För en källmapp Mapp1 med hello följande struktur: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello mål Mapp1 skapas med följande struktur hello: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File5 |
| SANT |mergeFiles |För en källmapp Mapp1 med hello följande struktur: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello mål Mapp1 skapas med följande struktur hello: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 + fil3 + File4 + filen 5 innehållet slås samman till en fil med automatiskt genererade namnet |
| FALSKT |preserveHierarchy |För en källmapp Mapp1 med hello följande struktur: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello målmappen Mapp1 skapas med följande struktur hello<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/><br/><br/>Subfolder1 med fil3, File4 och File5 har inte plockats. |
| FALSKT |flattenHierarchy |För en källmapp Mapp1 med hello följande struktur:<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello målmappen Mapp1 skapas med följande struktur hello<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2<br/><br/><br/>Subfolder1 med fil3, File4 och File5 har inte plockats. |
| FALSKT |mergeFiles |För en källmapp Mapp1 med hello följande struktur:<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello målmappen Mapp1 skapas med följande struktur hello<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 innehållet slås samman till en fil med automatiskt genererade namnet. automatiskt genererade namnet på File1<br/><br/>Subfolder1 med fil3, File4 och File5 har inte plockats. |

## <a name="walkthrough-use-copy-wizard-toocopy-data-tofrom-blob-storage"></a>Genomgång: Använd guiden för kopiera toocopy data till/från Blob Storage
Nu ska vi titta på hur tooquickly kopieringsdata till och från ett Azure blob storage. I den här genomgången lagrar data både källa och mål av typen: Azure Blob Storage. hello pipeline i den här genomgången kopierar data från en tooanother-mappen i hello samma blob-behållare. Den här genomgången är avsiktligt enkla tooshow du egenskaper när du använder Blob Storage som källa eller sink eller inställningar. 

### <a name="prerequisites"></a>Krav
1. Skapa en generell **Azure Storage-konto** om du inte redan har en. Du använder hello blob-lagring som båda **källa** och **mål** datalager i den här genomgången. Om du inte har ett Azure storage-konto finns hello [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) toocreate i steg en artikel.
2. Skapa en blobbbehållare med namnet **adfblobconnector** i hello storage-konto. 
4. Skapa en mapp med namnet **inkommande** i hello **adfblobconnector** behållare.
5. Skapa en fil med namnet **emp.txt** med hello efter innehåll och överföra den toohello **inkommande** mappen med hjälp av verktyg som [Azure Lagringsutforskaren](https://azurestorageexplorer.codeplex.com/)
    ```json
    John, Doe
    Jane, Doe
    ```
### <a name="create-hello-data-factory"></a>Skapa hello data factory
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **+ ny** hello övre vänstra hörnet och klicka på **Intelligence + analys**, och klicka på **Data Factory**.
3. I hello **nya data factory** bladet:   
    1. Ange **ADFBlobConnectorDF** för hello **namn**. hello namn i hello Azure data factory måste vara globalt unika. Om felmeddelandet hello: `*Data factory name “ADFBlobConnectorDF” is not available`, ändra hello namn i hello data factory (till exempel yournameADFBlobConnectorDF) och försök att skapa igen. Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.
    2. Välj din Azure-**prenumeration**.
    3. Resursgrupp, Välj **Använd befintliga** tooselect en befintlig grupp (eller) Välj **Skapa nytt** tooenter ett namn för en resursgrupp.
    4. Välj en **plats** för hello data factory.
    5. Välj **PIN-kod toodashboard** kryssrutan längst hello hello-bladet.
    6. Klicka på **Skapa**.
3. När hello har skapats visas hello **Datafabriken** bladet som visas i följande bild hello: ![Data factory-startsida](./media/data-factory-azure-blob-connector/data-factory-home-page.png)

### <a name="copy-wizard"></a>Guiden Kopiera
1. Klicka på startsidan för hello Data Factory hello **kopiera data [FÖRHANDSGRANSKNING]** panelen toolaunch **guiden Kopiera** i en separat flik.    
    
    > [!NOTE]
    >    Om du ser att hello webbläsare har ”auktorisera...”, inaktivera/avmarkera **blockerar cookies från tredje part och platsdata** inställning (eller) se till att den är aktiverad och skapa ett undantag för **login.microsoftonline.com**och försök sedan starta hello guiden igen.
2. I hello **egenskaper** sidan:
    1. Ange **CopyPipeline** för **aktivitet**. hello aktivitet är hello namn hello pipeline i din data factory.
    2. Ange en **beskrivning** för hello aktivitet (valfritt).
    3. För **aktivitet takt eller schemalägga**, hålla hello **körs regelbundet enligt schema** alternativet. Om du vill toorun aktiviteten en gång i stället för köras upprepade gånger enligt ett schema, Välj **kör nu en gång**. Om du väljer **kör nu en gång** alternativet en [enstaka pipeline](data-factory-create-pipelines.md#onetime-pipeline) skapas. 
    4. Behålla hello-inställningar för **mönster för återkommande**. Den här aktiviteten körs dagligen mellan hello börja och sluta tider du anger i hello nästa steg.
    5. Ändra hello **startdatum tid** för**2017-04/21**. 
    6. Ändra hello **sluttiden datum** för**04/25/2017**. Du kanske vill tootype hello datum i stället för att gå igenom hello kalender.     
    8. Klicka på **Nästa**.
      ![Kopiera verktyget - egenskapssidan](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png) 
3. På hello **källa datalagret** klickar du på **Azure Blob Storage** panelen. Du kan använda det här datalagret för sidan toospecify hello källa för hello kopiera aktivitet. Du kan använda en länkad tjänst för ett befintligt datalager (eller) ange ett nytt datalager. en befintlig toouse länkade tjänsten, väljer du **från befintliga LÄNKADE tjänster** och välj hello länkade tjänst. 
    ![Kopiera verktyget - källdata lagra sidan](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)
4. På hello **ange hello Azure Blob storage-konto** sidan:
   1. Behåll hello automatiskt genererade namnet för **anslutningsnamn**. hello anslutning är hello namn på hello länkade tjänsten av typen: Azure Storage. 
   2. Kontrollera att alternativet **Från Azure-prenumerationer** har valts för **Metod för kontoval**.
   3. Välj din Azure-prenumeration eller behålla **Markera alla** för **Azure-prenumeration**.   
   4. Välj en **Azure storage-konto** från hello lista över Azure storage-konton tillgängliga i hello valda prenumerationen. Du kan också välja tooenter lagringsutrymmet kontoinställningar manuellt genom att välja **ange manuellt** alternativ för hello **konto urvalsmetod**.
   5. Klicka på **Nästa**. 
      ![Kopiera verktyget - Ange hello Azure Blob storage-konto](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)
5. På **Välj hello inkommande fil eller mapp** sidan:
   1. Dubbelklicka på **adfblobcontainer**.
   2. Välj **inkommande**, och klicka på **Välj**. I den här genomgången kan du välja hello inkommande mapp. Du kan också markera hello emp.txt filen i mappen hello i stället. 
      ![Kopiera verktyget – Välj hello inkommande fil eller mapp](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)
6. På hello **Välj hello inkommande fil eller mapp** sidan:
    1. Bekräfta att hello **filen eller mappen** har angetts för**adfblobconnector/indata**. Om hello filer i undermappar, till exempel 2017-04-01, 02-04-2017 och så vidare, ange adfblobconnector/indata / {year} / {month} / {day} för filen eller mappen. När du trycker på FLIKEN utanför hello textruta finns tre listrutorna tooselect format för år (åååå), månad (MM) och dag (dd). 
    2. Ange inte **kopiera filen rekursivt**. Välj det här alternativet toorecursively Bläddra igenom mappar för filer toobe kopierade toohello mål. 
    3. Inte hello **binära kopiera** alternativet. Välj det här alternativet tooperform en binär kopia av källdatorn filen toohello mål. Markera inte den här genomgången så att du kan se fler alternativ i hello nästa sida. 
    4. Bekräfta att hello **Komprimeringstypen** har angetts för**ingen**. Välj ett värde för det här alternativet om källfilerna komprimeras i något av formaten hello stöds. 
    5. Klicka på **Nästa**.
    ![Kopiera verktyget – Välj hello inkommande fil eller mapp](./media/data-factory-azure-blob-connector/chose-input-file-folder.png) 
7. På hello **filen formatinställningar** kan du se hello avgränsare och hello schemat som är detekterade hello guiden med hello fil-parsning. 
    1. Bekräfta hello följande alternativ: en. Hej **filformatet** har angetts för**textformat**. Du kan se alla hello stöds formaten i hello nedrullningsbara listan. Till exempel: JSON, Avro, ORC, parkettgolv.
        b. Hej **kolumnen avgränsare** har angetts för`Comma (,)`. Du kan se hello andra kolumn avgränsare som stöds av Data Factory i hello nedrullningsbara listan. Du kan även ange anpassade avgränsare.
        c. Hej **raden avgränsare** har angetts för`Carriage Return + Line feed (\r\n)`. Du kan se hello andra raden avgränsare som stöds av Data Factory i hello nedrullningsbara listan. Du kan även ange anpassade avgränsare.
        d. Hej **hoppa över radnummer** har angetts för**0**. Om du vill att några rader toobe hoppas över hello överst i filen hello nummer hello här.
        e.  Hej **första dataraden innehåller kolumnnamn** har inte angetts. Om hello källfiler innehåller kolumnnamnen i hello första raden, väljer du det här alternativet.
        f. Hej **behandlar tomma kolumnvärde som null** alternativet anges.
    2. Expandera **avancerade inställningar** toosee avancerade alternativ som är tillgänglig.
    3. Se hello hello längst ned på sidan hello i **preview** av data från hello emp.txt fil.
    4. Klicka på **schemat** fliken på hello nedre toosee hello schemat hello kopiera guiden härledas genom att titta på hello data i hello källfil.
    5. Klicka på **nästa** när du granskar hello avgränsare och förhandsgranska data.
    ![Kopiera verktyget - inställningar för format](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)  
8. På hello **mål datalager sidan**väljer **Azure Blob Storage**, och klicka på **nästa**. Du använder hello Azure Blob Storage som båda hello käll- och datalager i den här genomgången.    
    ![Kopiera verktyget - Välj målserver datalager](media/data-factory-azure-blob-connector/select-destination-data-store.png)
9. På **ange hello Azure Blob storage-konto** sidan:
   1. Ange **AzureStorageLinkedService** för hello **anslutningsnamn** fältet.
   2. Kontrollera att alternativet **Från Azure-prenumerationer** har valts för **Metod för kontoval**.
   3. Välj din Azure-**prenumeration**.  
   4. Välj Azure storage-konto. 
   5. Klicka på **Nästa**.     
10. På hello **Välj hello utdata filen eller mappen** sidan: 
    6. Ange **mappsökväg** som **adfblobconnector-/ utdata / {year} / {month} / {day}**. Ange **FLIKEN**.
    7. För hello **år**väljer **åååå**.
    8. För hello **månad**, bekräfta att den har angetts för**MM**.
    9. För hello **dag**, bekräfta att den har angetts för**dd**.
    10. Bekräfta att hello **Komprimeringstypen** har angetts för**ingen**.
    11. Bekräfta att hello **kopiera beteende** har angetts för**Sammanfoga filer**. Om hello utdatafilen med samma namn finns redan hello, är hello nytt innehåll tillagda toohello samma fil hello slutet.
    12. Klicka på **Nästa**.
    ![Kopiera verktyget – Välj filen eller mappen](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)
11. På hello **filen formatinställningar** , granska hello inställningarna och klicka på **nästa**. En av hello här ytterligare alternativ är tooadd ett sidhuvud toohello utdatafilen. Om du väljer det alternativet läggs en rubrikrad med namnen på hello kolumner från hello schemat för hello källa. Du kan byta namn på hello standardkolumnvärdena när du visar hello schemat för hello källa. Du kan till exempel ändra hello första kolumnen tooFirst namn och den andra kolumnen tooLast namn. Hello utdatafilen genereras sedan med ett huvud med dessa namn som kolumnnamn. 
    ![Kopiera verktyget - format-inställningarna för mål](media/data-factory-azure-blob-connector/file-format-destination.png)
12. På hello **prestandainställningar** bekräftar som **molnet enheter** och **parallell kopior** har angetts för**automatisk**, och klicka på Nästa. Mer information om dessa inställningar finns [kopiera aktivitet prestanda och prestandajustering guiden](data-factory-copy-activity-performance.md#parallel-copy).
    ![Kopiera verktyget - prestandainställningar](media/data-factory-azure-blob-connector/copy-performance-settings.png) 
14. På hello **sammanfattning** , granska alla inställningar (Aktivitetsegenskaper, inställningar för källa och mål och inställningar) och klicka på **nästa**.
    ![Kopiera verktyget - sammanfattningssida](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)
15. Granska informationen i hello **sammanfattning** och klicka på **Slutför**. hello skapas två länkade tjänster, två datamängder (indata och utdata) och en pipeline i hello data factory (från där startas hello guiden Kopiera).
    ![Kopiera verktyget - distribution sida](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)

### <a name="monitor-hello-pipeline-copy-task"></a>Övervaka hello pipeline (kopiera aktivitet)

1. Klicka på länken hello `Click here toomonitor copy pipeline` på hello **distribution** sidan. 
2. Du bör se hello **övervaka och hantera program** i en separat flik.  ![Övervaka och hantera appen](media/data-factory-azure-blob-connector/monitor-manage-app.png)
3. Ändra hello **starta** tid hello överst för`04/19/2017` och **end** tid för`04/27/2017`, och klicka sedan på **tillämpa**. 
4. Du bör se fem aktivitet windows hello **aktivitet WINDOWS** lista. Hej **WindowStart** gånger bör omfatta alla dagar från pipeline starttiderna toopipeline slut. 
5. Klicka på **uppdatera** knapp för hello **aktivitet WINDOWS** några gånger tills du ser alla hello aktivitet windows hello status är inställd tooReady. 
6. Nu, kontrollera att hello utdatafilerna genereras i hello utdatamapp för adfblobconnector behållare. Du bör se följande mappstrukturen i hello utdatamapp hello: 
    ```
    2017/04/21
    2017/04/22
    2017/04/23
    2017/04/24
    2017/04/25    
    ```
Detaljerad information om övervakning och hantering av datafabriker finns [övervaka och hantera Data Factory-pipelinen](data-factory-monitor-manage-app.md) artikel. 
 
### <a name="data-factory-entities"></a>Data Factory entiteter
Växla tillbaka toohello flik med hello Data Factory-startsidan. Observera att det finns två länkade tjänster, två datamängder och en pipeline i din data factory nu. 

![Startsida för data Factory med entiteter](media/data-factory-azure-blob-connector/data-factory-home-page-with-numbers.png)

Klicka på **författare och distribuera** toolaunch Data Factory-redigeraren. 

![Data Factory-redigeraren](media/data-factory-azure-blob-connector/data-factory-editor.png)

Du bör se följande Data Factory-enheter i din data factory hello: 

 - Två länkade tjänster. En för hello käll- och hello andra för hello mål. Båda hello länkade tjänster finns toohello samma Azure Storage-konto i den här genomgången. 
 - Två datauppsättningar. En inkommande datauppsättning och en datamängd för utdata. I den här genomgången använder båda hello samma blobbehållaren men finns toodifferent mappar (indata och utdata).
 - En pipeline. hello pipelinen innehåller en kopia-aktivitet som använder en blob-källa och en mottagare toocopy blobbdata från ett Azure blob plats tooanother Azure blob-plats. 

hello innehåller följande avsnitt mer information om dessa enheter. 

#### <a name="linked-services"></a>Länkade tjänster
Du bör se två länkade tjänster. En för hello käll- och hello andra för hello mål. I den här genomgången hello båda definitioner utseende samma förutom hello namn. Hej **typen** av hello länkade tjänsten är inställd för**AzureStorage**. De viktigaste egenskapen hello länkade service definition är hello **connectionString**, som används av Data Factory tooconnect tooyour Azure Storage-konto vid körning. Ignorera hello hubName egenskap i hello definition. 

##### <a name="source-blob-storage-linked-service"></a>Källan blob länkade lagringstjänsten
```json
{
    "name": "Source-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

##### <a name="destination-blob-storage-linked-service"></a>Mål-blob länkade lagringstjänsten

```json
{
    "name": "Destination-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

Mer information om länkad Azure Storage-tjänst finns [länkade tjänstegenskaper](#linked-service-properties) avsnitt. 

#### <a name="datasets"></a>Datauppsättningar
Det finns två datamängder: en inkommande datauppsättning och en datamängd för utdata. hello typ av hello datamängden har angetts för**AzureBlob** för båda. 

hello inkommande dataset pekar toohello **inkommande** för hello **adfblobconnector** blob-behållare. Hej **externa** egenskapen för**SANT** för denna dataset som hello data inte tillverkas av hello pipeline med hello kopieringsaktiviteten som äger denna dataset som indata. 

Hej utdata dataset pekar toohello **utdata** för hello samma blob-behållare. Hej utdatauppsättningen använder också hello år, månad och dag av hello **SliceStart** system variabeln toodynamically utvärdera hello sökvägen för hello utdatafilen. En lista över funktioner och systemvariabler som stöds av Data Factory finns [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md). Hej **externa** egenskapen för**FALSKT** (standardvärdet) eftersom den här datauppsättningen produceras av hello pipeline. 

Mer information om egenskaper som stöds av Azure-blobbdatauppsättning finns [egenskaper för datamängd](#dataset-properties) avsnitt.

##### <a name="input-dataset"></a>Indatauppsättning

```json
{
    "name": "InputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Source-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/input/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

##### <a name="output-dataset"></a>Datamängd för utdata

```json
{
    "name": "OutputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Destination-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/output/{year}/{month}/{day}",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
            "partitionedBy": [
                { "name": "year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }
            ]
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

#### <a name="pipeline"></a>Pipeline
hello pipeline har bara en aktivitet. Hej **typen** av hello aktiviteten är inställd för**kopiera**.  Det finns två delar, en för käll- och hello andra för sink i hello Typegenskaper för hello aktiviteten. hello källtypen har angetts för**BlobSource** som hello aktivitet kopierar data från en blob storage. hello mottagartypen har angetts för**BlobSink** som hello aktivitet kopierar data tooa blob-lagring. Hej kopieringsaktiviteten använder InputDataset z4y som hello indata och OutputDataset z4y som hello utdata. 

Mer information om egenskaper som stöds av BlobSource och BlobSink finns [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt. 

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "MergeFiles",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-z4y"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-z4y"
                    }
                ],
                "policy": {
                    "timeout": "1.00:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 3,
                    "longRetry": 0,
                    "longRetryInterval": "00:00:00"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "Activity-0-Blob path_ adfblobconnector_input_->OutputDataset-z4y"
            }
        ],
        "start": "2017-04-21T22:34:00Z",
        "end": "2017-04-25T05:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
```

## <a name="json-examples-for-copying-data-tooand-from-blob-storage"></a>JSON-exempel för att kopiera data tooand från Blob Storage  
hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De visar hur toocopy data tooand från Azure Blob Storage och Azure SQL Database. Dock datan kan kopieras **direkt** från alla källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.

### <a name="json-example-copy-data-from-blob-storage-toosql-database"></a>JSON-exempel: Kopiera data från Blob Storage tooSQL databas
följande exempel visar hello:

1. En länkad tjänst av typen [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med en kopia-aktivitet som använder [BlobSource](#copy-activity-properties) och [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).

hello exemplet kopierar time series-data från en tabell i Azure blob tooan Azure SQL varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

**Azure SQL länkade tjänsten:**

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Storage länkade tjänsten:**

```json
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
Azure Data Factory stöder två typer av länkad Azure Storage services: **AzureStorage** och **AzureStorageSas**. För hello första, anger du hello anslutningssträng som innehåller hello kontonyckel och för hello senare, anger du hello delade signatur åtkomst (SAS)-Uri. Se [länkade tjänster](#linked-service-properties) information.  

**Azure Blob inkommande datamängd:**

Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1). hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad och som en dagdel av hello starttid och filnamn använder hello timme tillhör hello starttid. ”externa”: ”true” inställningen informerar Data Factory hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
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
**Azure SQL utdatauppsättningen:**

hello exemplet kopierar data tooa tabell med namnet ”mytable” som prefix i en Azure SQL database. Skapa hello tabell i Azure SQL database med hello samma antal kolumner som du förväntar dig hello Blob CSV-filen toocontain. Nya rader läggs toohello tabell varje timme.

```json
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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
**Kopieringsaktiviteten i en pipeline med Blob källa och mottagare för SQL:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och **sink** typ har angetts för**SqlSink**.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
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
### <a name="json-example-copy-data-from-azure-sql-tooazure-blob"></a>JSON-exempel: Kopiera data från Azure SQL tooAzure Blob
följande exempel visar hello:

1. En länkad tjänst av typen [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) och [BlobSink](#copy-activity-properties).

hello exemplet kopierar time series-data från en Azure SQL-tabell tooan Azure blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

**Azure SQL länkade tjänsten:**

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Storage länkade tjänsten:**

```json
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
Azure Data Factory stöder två typer av länkad Azure Storage services: **AzureStorage** och **AzureStorageSas**. För hello första, anger du hello anslutningssträng som innehåller hello kontonyckel och för hello senare, anger du hello delade signatur åtkomst (SAS)-Uri. Se [länkade tjänster](#linked-service-properties) information.  

**Azure SQL inkommande datamängd:**

hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Azure SQL och innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.

Inställningen ”externa”: ”true” informerar Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```json
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
      "partitionedBy": [
        {
          "name": "Year",
          "value": { "type": "DateTime",  "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
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

**Kopieringsaktiviteten i en pipeline med SQL-källa och mottagare för Blob:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**SqlSource** och **sink** typ har angetts för**BlobSink**. hello SQL-frågan som angetts för hello **SqlReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureSQLInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
> toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.
