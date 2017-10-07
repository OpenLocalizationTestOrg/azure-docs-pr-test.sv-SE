---
title: "aaaCopy data till/från ett filsystem med Azure Data Factory | Microsoft Docs"
description: "Lär dig hur toocopy data tooand från ett lokalt filsystem med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ce19f1ae-358e-4ffc-8a80-d802505c9c84
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jingwang
ms.openlocfilehash: 201b8bc3ffa639df781443aa0c3f95c975d280be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-an-on-premises-file-system-by-using-azure-data-factory"></a>Kopiera data tooand från ett lokalt filsystem med hjälp av Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toocopy data till och från ett lokalt filsystem. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.

## <a name="supported-scenarios"></a>Scenarier som stöds
Du kan kopiera data **från ett lokalt filsystem** toohello följande datakällor:

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

Du kan kopiera data från hello följande datalager **tooan lokalt filsystem**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Kopieringsaktiviteten tar inte bort hello källfilen när den inte har kopierade toohello mål. Om du behöver toodelete hello källfilen efter en lyckad kopiering, skapa en anpassad aktivitet toodelete hello-fil och använda hello aktivitet i hello pipeline. 

## <a name="enabling-connectivity"></a>Aktivera anslutning
Data Factory stöder anslutande tooand från ett lokalt filsystem via **Data Management Gateway**. Du måste installera hello Data Management Gateway i din lokala miljö för hello Data Factory-tjänsten tooconnect tooany stöds lokalt datalager inklusive filsystemet. toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway finns [flytta data mellan lokala källor och hello moln med Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md). Förutom Data Management Gateway måste inte några binära filer toobe installerat toocommunicate tooand från ett lokalt filsystem. Du måste installera och använda hello Data Management Gateway, även om hello filsystemet är i Azure IaaS-VM. Detaljerad information om hello gateway finns [Data Management Gateway](data-factory-data-management-gateway.md).

toouse en Linux-filresurs, installera [Samba](https://www.samba.org/) på Linux-servern och installera Data Management Gateway på en Windows server. Det går inte att installera Data Management Gateway på en Linux-server.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till/från ett filsystem med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:

1. Skapa en **datafabriken**. En datafabrik kan innehålla en eller flera pipelines. 
2. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory. Till exempel om du kopierar data från ett Azure blob storage tooan lokalt filsystem skapa du två länkade tjänster toolink lokala filsystemet och Azure storage-konto tooyour data factory. Länkad tjänstegenskaper som är specifika tooan lokalt filsystem, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt.
3. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello blob-behållaren och mappen som innehåller hello indata. Och du skapar en annan dataset toospecify hello mapp och fil namn (valfritt) i filsystemet. Egenskaper för datamängd som är specifika tooon lokala filsystem, finns [egenskaper för datamängd](#dataset-properties) avsnitt.
4. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. I hello-exemplet ovan, använder du BlobSource som en källa och FileSystemSink som en mottagare för hello kopieringsaktiviteten. På samma sätt om du vill kopiera från lokalt Arkiv system tooAzure Blob Storage, använder du FileSystemSource och BlobSink i hello kopieringsaktiviteten. Kopiera Aktivitetsegenskaper som är specifika tooon lokala filsystem, finns [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt. Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager.

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från ett filsystem finns [JSON-exempel](#json-examples-for-copying-data-to-and-from-file-system) i den här artikeln.

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika toofile system:

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
Du kan länka en lokal fil system tooan Azure data factory med hello **lokala filserver** länkade tjänsten. hello i den följande tabellen finns beskrivningar JSON-element som är specifika toohello lokala filserver länkade tjänsten.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |Kontrollera att hello type-egenskap är inställd för**OnPremisesFileServer**. |Ja |
| värden |Anger hello rotsökvägen för hello mappen som du vill toocopy. Använd hello escape-tecknet ' \ ' för specialtecken i hello-sträng. Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel. |Ja |
| användar-ID |Ange hello-ID för hello-användare som har åtkomst till toohello servern. |Nej (om du väljer encryptedCredential) |
| lösenord |Ange hello användarlösenord hello (användar-ID). |Nej (om du väljer encryptedCredential |
| encryptedCredential |Ange hello krypterade autentiseringsuppgifter som du kan få genom att köra hello ny AzureRmDataFactoryEncryptValue cmdlet. |Nej (om du väljer toospecify användar-ID och lösenord i klartext) |
| gatewayName |Anger hello namnet på hello gateway som Data Factory ska använda tooconnect toohello lokal server. |Ja |


### <a name="sample-linked-service-and-dataset-definitions"></a>Exempel på länkad tjänst och dataset definitioner
| Scenario | Värden i länkade tjänstdefinitionen | folderPath i datauppsättningsdefinitionen |
| --- | --- | --- |
| Lokal mapp på Data Management Gateway-datorn: <br/><br/>Exempel: D:\\ \* eller D:\folder\subfolder\\* |D:\\ \\ (för Data Management Gateway 2.0 och senare) <br/><br/> localhost (för tidigare versioner än Data Management Gateway 2.0) |. \\ \\ eller mappen\\\\undermapp (för Data Management Gateway 2.0 och senare) <br/><br/>D:\\ \\ eller D:\\\\mappen\\\\undermapp (för gateway-versionen nedan 2.0) |
| Delad fjärrmapp: <br/><br/>Exempel: \\ \\minserver\\dela\\ \* eller \\ \\minserver\\dela\\mappen\\undermapp\\* |\\\\\\\\minserver\\\\dela |. \\ \\ eller mappen\\\\undermapp |


### <a name="example-using-username-and-password-in-plain-text"></a>Exempel: Med användarnamn och lösenord i klartext

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

### <a name="example-using-encryptedcredential"></a>Exempel: Använda encryptedcredential

```JSON
{
  "Name": " OnPremisesFileServerLinkedService ",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "D:\\",
      "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
      "gatewayName": "mygateway"
    }
  }
}
```

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns [skapa datauppsättningar](data-factory-create-datasets.md). Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen.

Hej typeProperties avsnittet är olika för varje typ av datauppsättningen. Den ger information, till exempel hello plats och dataformat hello i hello-datalagret. Hej typeProperties avsnittet för hello dataset av typen **filresursen** har hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| folderPath |Anger hello undersökvägen toohello mapp. Använd hello escape-tecknet ' \' för specialtecken i hello-sträng. Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.<br/><br/>Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger. |Ja |
| fileName |Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello. Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.<br/><br/>När **fileName** har inte angetts för en datamängd för utdata och **preserveHierarchy** har inte angetts i aktiviteten sink hello hello genereras filen heter i hello följande format: <br/><br/>`Data.<Guid>.txt`(Exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Nej |
| fileFilter |Ange ett filter toobe används tooselect en delmängd av filer i hello folderPath i stället för alla filer. <br/><br/>Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).<br/><br/>Exempel 1: ”fileFilter” ”: * .log”<br/>Exempel 2: ”fileFilter”: 2014 - 1-?. txt ”<br/><br/>Observera att fileFilter gäller för en inkommande filresursen datauppsättning. |Nej |
| partitionedBy |Du kan använda partitionedBy toospecify dynamiska folderPath/filnamn för time series-data. Ett exempel är folderPath parametriserade varje timme av data. |Nej |
| Format | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Ange hello **typen** egenskap under format tooone av dessa värden. Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. <br><br> Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner. |Nej |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**. Nivåer som stöds är: **Optimal** och **snabbast**. Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |

> [!NOTE]
> Du kan inte använda filnamn och fileFilter samtidigt.

### <a name="using-partitionedby-property"></a>Med hjälp av partitionedBy egenskap
Som nämnts tidigare under hello, du kan ange en dynamisk folderPath och ett filnamn för tid series-data med hello **partitionedBy** egenskapen [Data Factory-funktioner och hello systemvariabler](data-factory-functions-variables.md).

toounderstand mer information om tidsserier datauppsättningar, schemaläggning och segment, se [skapa datauppsättningar](data-factory-create-datasets.md), [schemaläggning och körning](data-factory-scheduling-and-execution.md), och [skapar pipelines](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Exempel 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

I det här exemplet {segment} ersätts med värdet hello hello Data Factory systemvariabeln SliceStart hello-format (YYYYMMDDHH). SliceStart refererar toostart tiden för hello sektorn. hello folderPath är olika för varje segment. Till exempel: 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout.

#### <a name="sample-2"></a>Exempel 2:

```JSON
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

I det här exemplet extraheras år, månad, dag och tid för SliceStart i separata variabler med hello folderPath och filnamnet egenskaper.

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, indata och utdata-datauppsättningar och -principer är tillgängliga för alla typer av aktiviteter. Medan egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp.

För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor. Om du flyttar data från ett lokalt filsystem du ställa in hello källtypen i hello kopieringsaktiviteten också**FileSystemSource**. På samma sätt om du flyttar data tooan lokalt filsystem, du anger hello Mottagartypen i hello kopieringsaktiviteten för**FileSystemSink**. Det här avsnittet innehåller en lista över egenskaper som stöds av FileSystemSource och FileSystemSink.

**FileSystemSource** stöder hello följande egenskaper:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| Rekursiva |Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen. |SANT, FALSKT (standard) |Nej |

**FileSystemSink** stöder hello följande egenskaper:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| copyBehavior |Definierar hello kopiera beteende när hello källa är BlobSource eller filsystem. |**PreserveHierarchy:** bevarar hello filen hierarki i hello målmappen. Hello relativ sökväg hello filen toohello källa källmappen är det vill säga hello samma som hello relativa sökväg hello filen toohello mål målmappen.<br/><br/>**FlattenHierarchy:** alla filer från källmappen hello skapas i hello första nivån i målmappen. hello mål filer skapas med ett namn som genererats automatiskt.<br/><br/>**MergeFiles:** sammanfogar alla filer från hello källfil mappen tooone. Om hello namn/blob filnamn har angetts är hello kopplade filnamnet hello angivet namn. Annars är ett automatiskt genererat namn. |Nej |

### <a name="recursive-and-copybehavior-examples"></a>rekursiva och copyBehavior exempel
Det här avsnittet beskrivs hello resultatet av hello kopia för olika kombinationer av värden för egenskaperna för hello rekursiv och copyBehavior.

| rekursiva värde | copyBehavior värde | Resultatet |
| --- | --- | --- |
| SANT |preserveHierarchy |För en källmapp Mapp1 med hello följande struktur,<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello målmappen Mapp1 skapas med samma struktur som källa för hello hello:<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| SANT |flattenHierarchy |För en källmapp Mapp1 med hello följande struktur,<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello mål Mapp1 skapas med följande struktur hello: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File5 |
| SANT |mergeFiles |För en källmapp Mapp1 med hello följande struktur,<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello mål Mapp1 skapas med följande struktur hello: <br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 + fil3 + File4 + filen 5 innehållet slås samman till en fil med ett automatiskt genererat namn. |
| FALSKT |preserveHierarchy |För en källmapp Mapp1 med hello följande struktur,<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello målmappen Mapp1 skapas med följande struktur hello:<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/><br/>Subfolder1 med fil3, File4 och File5 hämtas inte. |
| FALSKT |flattenHierarchy |För en källmapp Mapp1 med hello följande struktur,<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello målmappen Mapp1 skapas med följande struktur hello:<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2<br/><br/>Subfolder1 med fil3, File4 och File5 hämtas inte. |
| FALSKT |mergeFiles |För en källmapp Mapp1 med hello följande struktur,<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello målmappen Mapp1 skapas med följande struktur hello:<br/><br/>Mapp1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 innehållet slås samman till en fil med ett automatiskt genererat namn.<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatiskt genererade namnet på File1<br/><br/>Subfolder1 med fil3, File4 och File5 hämtas inte. |

## <a name="supported-file-and-compression-formats"></a>Fil- och komprimering format som stöds
Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikeln för information.

## <a name="json-examples-for-copying-data-tooand-from-file-system"></a>JSON-exempel för att kopiera data tooand från filsystemet
hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av hello [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De visar hur toocopy data tooand från ett lokalt filsystem och Azure Blob storage. Du kan dock kopiera data *direkt* från någon av hello källor tooany av hello sänkor som anges i [källor och sänkor stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av Kopieringsaktiviteten i Azure Data Factory.

### <a name="example-copy-data-from-an-on-premises-file-system-tooazure-blob-storage"></a>Exempel: Kopiera data från en lokal fil system tooAzure Blob storage
Det här exemplet visas hur toocopy data från en lokal fil system tooAzure Blob storage. hello exemplet har hello följande Data Factory-enheter:

* En länkad tjänst av typen [OnPremisesFileServer](#linked-service-properties).
* En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Indata [dataset](data-factory-create-datasets.md) av typen [filresursen](#dataset-properties).
* Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello följande exempel kopierar time series-data från en lokal fil system tooAzure Blob storage varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello avsnitt efter hello prover.

Som ett första steg bör ställa in Data Management Gateway enligt hello instruktionerna i [flytta data mellan lokala källor och hello moln med Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).

**Lokala filserver länkade tjänsten:**

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

Vi rekommenderar att du använder hello **encryptedCredential** egenskapen i stället hello **userid** och **lösenord** egenskaper. Se [filserver länkade tjänsten](#linked-service-properties) mer information om den här länkade tjänsten.

**Azure Storage länkade tjänsten:**

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

**Lokal fil system inkommande datauppsättningen:**

Data hämtas från en ny fil varje timme. Hej folderPath och fileName egenskaper bestäms baserat på hello starttiden för hello sektorn.  

Ange `"external": "true"` informerar Data Factory som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
{
  "name": "OnpremisesFileSystemInput",
  "properties": {
    "type": " FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
      ]
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

**Azure Blob storage utdatauppsättningen:**

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder hello år, månad, dag och timme delar av hello starttid.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kopieringsaktiviteten i en pipeline med filsystemet källa och mottagare för Blob:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**FileSystemSource**, och **sink** typ har angetts för**BlobSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T19:00:00",
    "description":"Pipeline for copy activity",
    "activities":[  
      {
        "name": "OnpremisesFileSystemtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "OnpremisesFileSystemInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "FileSystemSource"
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

### <a name="example-copy-data-from-azure-sql-database-tooan-on-premises-file-system"></a>Exempel: Kopiera data från Azure SQL Database tooan lokalt filsystem
följande exempel visar hello:

* En länkad tjänst av typen [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)
* En länkad tjänst av typen [OnPremisesFileServer](#linked-service-properties).
* En datauppsättning som indata av typen [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
* En datamängd för utdata av typen [filresursen](#dataset-properties).
* En pipeline med en kopia-aktivitet som använder [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) och [FileSystemSink](#copy-activity-properties).

hello exemplet kopierar time series-data från ett Azure SQL tabell tooan lokalt filsystem varje timme. hello JSON egenskaper som används i exemplen beskrivs i avsnitt efter hello prover.

**Azure SQL Database länkade tjänsten:**

```JSON
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

**Lokala filserver länkade tjänsten:**

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

Vi rekommenderar att du använder hello **encryptedCredential** egenskapen istället för att använda hello **userid** och **lösenord** egenskaper. Se [filsystemet länkade tjänsten](#linked-service-properties) mer information om den här länkade tjänsten.

**Azure SQL inkommande datamängd:**

hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Azure SQL och den innehåller en kolumn med namnet ”timestampcolumn” för time series-data.

Ange ``“external”: ”true”`` informerar Data Factory som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
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

**Lokal fil system utdatauppsättningen:**

Data är kopierade tooa ny fil varje timme. hello folderPath och filnamn för hello blob bestäms baserat på hello starttiden för hello sektorn.

```JSON
{
  "name": "OnpremisesFileSystemOutput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
      ]
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

**Kopieringsaktiviteten i en pipeline med SQL-källa och mottagare för filsystemet:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**SqlSource**, och hello **sink** typ har angetts för**FileSystemSink**. hello SQL-fråga som har angetts för hello **SqlReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T20:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoOnPremisesFile",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "OnpremisesFileSystemOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "FileSystemSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 3,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```


Du kan också mappa kolumner från källan dataset toocolumns från sink datauppsättning i aktivitetsdefinitionen för hello kopia. Mer information finns i [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestanda- och justering
 toolearn om nyckeln faktorer som påverkan hello prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize, se hello [prestandajustering guide och Kopieringsaktivitet prestanda](data-factory-copy-activity-performance.md).
