---
title: "aaaMove data tooand från SQL Server | Microsoft Docs"
description: "Lär dig mer om hur toomove data till/från SQL Server-databas som är lokalt eller i en virtuell Azure-dator med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 864ece28-93b5-4309-9873-b095bbe6fedd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jingwang
ms.openlocfilehash: f0cccf56a670e62ec893d75052a81eb26d562050
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a>Flytta data tooand från SQL Server lokalt eller på IaaS (Azure VM) med hjälp av Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data till/från en lokal SQL Server-databas. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten. 

## <a name="supported-scenarios"></a>Scenarier som stöds
Du kan kopiera data **från en SQL Server-databas** toohello följande datakällor:

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

Du kan kopiera data från hello följande datalager **tooa SQL Server-databas**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-sql-server-versions"></a>SQL Server-versioner som stöds
Den här connector-stöd för SQL Server som kopierar data från / toohello följande versioner av instansen finns på lokalt eller i Azure IaaS med både SQL-autentisering och Windows-autentisering: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQLServer 2005

## <a name="enabling-connectivity"></a>Aktivera anslutning
hello begrepp och steg som krävs för att ansluta med SQL Server finns lokalt eller i Azure IaaS (Infrastructure-as-a-Service) virtuella datorer är hello samma. I båda fallen måste toouse Data Management Gateway-anslutningen.

Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway. Ställa in en gateway-instans är ett krav för att ansluta med SQL Server.

Du kan installera gatewayen hello samma på lokala datorer eller moln VM-instans hello SQL Server för bättre prestanda, rekommenderar vi att du installerar dem på separata datorer. Med hello gateway och SQL Server på separata datorer minskar resurskonflikter.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till/från en lokal SQL Server-databas med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret: 

1. Skapa en **datafabriken**. En datafabrik kan innehålla en eller flera pipelines. 
2. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory. Exempelvis om du kopierar data från en SQL Server-databasen tooan Azure blob-lagring måste skapa du två länkade tjänster toolink din SQL Server-databas och Azure storage-konto tooyour data factory. Länkad tjänstegenskaper som är specifika tooSQL Server-databasen, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt. 
3. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello SQL-tabell i SQL Server-databasen som innehåller hello indata. Och, skapar du en annan dataset toospecify hello blob-behållaren och hello-mappen som innehåller hello data kopieras från hello SQL Server-databas. Egenskaper för datamängd som är specifika tooSQL Server-databasen, se [egenskaper för datamängd](#dataset-properties) avsnitt.
4. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. I hello-exemplet ovan, använder du SqlSource som en källa och BlobSink som en mottagare för hello kopieringsaktiviteten. På samma sätt om du vill kopiera från Azure Blob Storage tooSQL Server-databas, använder du BlobSource och SqlSink i hello kopieringsaktiviteten. Kopiera Aktivitetsegenskaper som är specifika tooSQL Server-databasen, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt. Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager. 

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från en lokal SQL Server-databas finns [JSON-exempel](#json-examples-for-copying-data-from-and-to-sql-server) i den här artikeln. 

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooSQL Server: 

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
Du skapar en länkad tjänst av typen **OnPremisesSqlServer** toolink fabriken för tooa en lokal SQL Server-databasen. hello följande tabell ger en beskrivning för JSON-element specifika tooon lokala SQL Server som är länkade tjänsten.

hello följande tabell ger en beskrivning för JSON-element specifika tooSQL länkad Server-tjänsten.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello typegenskapen ska anges till: **OnPremisesSqlServer**. |Ja |
| connectionString |Ange connectionString information som behövs för tooconnect toohello lokala SQL Server-databas med SQL-autentisering eller Windows-autentisering. |Ja |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala SQL Server-databas. |Ja |
| användarnamn |Ange användarnamnet om du använder Windows-autentisering. Exempel: **domainname\\användarnamn**. |Nej |
| lösenord |Ange lösenord för hello-användarkonto som du angav för hello användarnamn. |Nej |

Du kan kryptera autentiseringsuppgifterna med hjälp av hello **ny AzureRmDataFactoryEncryptValue** cmdlet och använda dem i hello anslutningssträngen som visas i följande exempel hello (**EncryptedCredential** egenskapen):  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

### <a name="samples"></a>Exempel
**JSON för att använda SQL-autentisering**

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties":
    {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
**JSON för att använda Windows-autentisering**

Data Management Gateway kommer personifiera hello angivna användare konto tooconnect toohello lokala SQL Server-databas. 

```json
{
     "Name": " MyOnPremisesSQLDB",
     "Properties":
     {
         "type": "OnPremisesSqlServer",
         "typeProperties": {
             "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
             "username": "<domain\\username>",
             "password": "<password>",
             "gatewayName": "<gateway name>"
        }
     }
}
```

## <a name="dataset-properties"></a>Egenskaper för datamängd
Hello samplingarna, du har använt ett DataSet-objekt av typen **SqlServerTable** toorepresent en tabell i SQL Server-databasen.  

En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen (SQL Server, Azure blob, Azure-tabellen, osv.).

hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej **typeProperties** avsnittet för hello dataset av typen **SqlServerTable** har hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabellen eller vyn i hello SQL Server-databasinstansen som den länkade tjänsten refererar till. |Ja |

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
Om du flyttar data från en SQL Server-databas måste du ange hello källtypen i hello kopieringsaktiviteten för**SqlSource**. På samma sätt om du flyttar data tooa SQL Server-databas måste du ange hello Mottagartypen i hello kopieringsaktiviteten för**SqlSink**. Det här avsnittet innehåller en lista över egenskaper som stöds av SqlSource och SqlSink.

En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.

> [!NOTE]
> Hej Kopieringsaktiviteten tar endast en inmatning och ger en enda utdata.

De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

### <a name="sqlsource"></a>SqlSource
När datakällan i en Kopieringsaktivitet är av typen **SqlSource**, hello följande egenskaper är tillgängliga i **typeProperties** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| sqlReaderQuery |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: Välj * från mytable prefix. Kan referera till flera tabeller från hello-databas som refereras av hello inkommande dataset. Om inget anges hello SQL-instruktionen som körs: Välj från mytable prefix. |Nej |
| sqlReaderStoredProcedureName |Namnet på hello lagrad procedur som läser data från hello källtabellen. |Namnet på hello lagrad procedur. hello senaste SQL-instruktionen måste vara en SELECT-instruktion i hello lagrad procedur. |Nej |
| storedProcedureParameters |Parametrar för hello lagrad procedur. |Namn/värde-par. Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar. |Nej |

Om hello **sqlReaderQuery** har angetts för hello SqlSource, hello Kopieringsaktiviteten kör den här frågan mot hello SQL Server-databas källa tooget hello data.

Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).

Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName är hello kolumner har definierats i hello struktur avsnitt används toobuild en select-frågan toorun mot hello SQL Server-databas. Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.

> [!NOTE]
> När du använder **sqlReaderStoredProcedureName**, du behöver fortfarande toospecify ett värde för hello **tableName** egenskap i hello dataset JSON. Det finns inga verifieringar utföras om mot den här tabellen.

### <a name="sqlsink"></a>SqlSink
**SqlSink** stöder hello följande egenskaper:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| writeBatchTimeout |Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås. |TimeSpan<br/><br/> Exempel ”: 00: 30:00” (30 minuter). |Nej |
| writeBatchSize |Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize. |Heltal (antalet rader) |Nej (standard: 10000) |
| sqlWriterCleanupScript |Ange frågan för Kopieringsaktiviteten tooexecute så att data för ett visst segment har rensats bort. Mer information finns i [repeterbara kopiera](#repeatable-copy) avsnitt. |En frågesats. |Nej |
| sliceIdentifierColumnName |Ange kolumnnamn för Kopieringsaktiviteten toofill med genereras automatiskt segment-ID som har använt tooclean in data för ett visst segment när köras på nytt. Mer information finns i [repeterbara kopiera](#repeatable-copy) avsnitt. |Kolumnnamnet för en kolumn med datatypen för binary(32). |Nej |
| sqlWriterStoredProcedureName |Namnet på hello lagrad procedur upserts (uppdateringar/infogar) data i hello måltabellen. |Namnet på hello lagrad procedur. |Nej |
| storedProcedureParameters |Parametrar för hello lagrad procedur. |Namn/värde-par. Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar. |Nej |
| sqlWriterTableType |Ange tabellen typen namnet toobe används i hello lagrade proceduren. Kopieringsaktiviteten tillgängliggör hello data flyttas i en temporär tabell med den här tabellen. Lagrade procedurer kan sedan koppla hello data kopieras med befintliga data. |Ett namn för tabellen. |Nej |


## <a name="json-examples-for-copying-data-from-and-toosql-server"></a>JSON-exempel för att kopiera data från och tooSQL Server
hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Hej följande exempel visar hur toocopy data tooand från SQL Server och Azure Blob Storage. Dock datan kan kopieras **direkt** från alla källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.     

## <a name="example-copy-data-from-sql-server-tooazure-blob"></a>Exempel: Kopiera data från SQL Server-tooAzure Blob
följande exempel visar hello:

1. En länkad tjänst av typen [OnPremisesSqlServer](#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [SqlServerTable](#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Hej [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [SqlSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar time series-data från en SQL Server tabellen tooan Azure blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

Som ett första steg bör du konfigurera hello data management gateway. hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**SQL Server som är länkad tjänst**
```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
**Azure Blob länkade lagringstjänsten**

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
**SQL Server inkommande dataset**

hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i SQL Server och den innehåller en kolumn med namnet ”timestampcolumn” för tid series-data. Du kan fråga över flera tabeller i samma databas som använder en enda dataset, men en enskild tabell måste användas för hello dataset tableName typeProperty hello.

Inställningen ”externa”: ”true” informerar Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```json
{
  "name": "SqlServerInput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
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
**Azure Blob utdatauppsättningen**

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.

```json
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
**Pipeline med kopieringsaktiviteten**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse dessa indata och utdata-datauppsättningar och schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**SqlSource** och **sink** typ har angetts för**BlobSink**. hello SQL-frågan som angetts för hello **SqlReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2016-06-01T18:00:00",
    "end":"2016-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
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
I det här exemplet **sqlReaderQuery** har angetts för hello SqlSource. Hej Kopieringsaktiviteten kör den här frågan mot hello SQL Server-databas källdata tooget hello. Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar). Hej sqlReaderQuery kan referera till flera tabeller i hello-databas som refereras av hello inkommande dataset. Det är inte begränsad tooonly hello tabellen som hello datauppsättnings tableName typeProperty.

Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName är hello kolumner har definierats i hello struktur avsnitt används toobuild en select-frågan toorun mot hello SQL Server-databas. Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.

Se hello [Sql-källans](#sqlsource) avsnitt och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) hello lista över egenskaper som stöds av SqlSource och BlobSink.

## <a name="example-copy-data-from-azure-blob-toosql-server"></a>Exempel: Kopiera data från Azure Blob-tooSQL Server
följande exempel visar hello:

1. hello länkade tjänsten av typen [OnPremisesSqlServer](#linked-service-properties).
2. hello länkade tjänsten av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
5. Hej [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [SqlSink](#sql-server-copy-activity-type-properties).

hello exemplet kopierar time series-data från en tabell i Azure blob tooa SQL Server varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

**SQL Server som är länkad tjänst**

```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
**Azure Blob länkade lagringstjänsten**

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
**Azure Blob-inkommande datamängd**

Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1). hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad och som en dagdel av hello starttid och filnamn använder hello timme tillhör hello starttid. ”externa”: ”true” inställningen informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```json
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
**SQL Server-utdatauppsättningen**

hello exemplet kopierar data tooa tabell med namnet ”mytable” som prefix i SQL Server. Skapa hello tabell i SQL Server med hello samma antal kolumner som du förväntar dig hello Blob CSV-filen toocontain. Nya rader läggs toohello tabell varje timme.

```json
{
  "name": "SqlServerOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
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
**Pipeline med kopieringsaktiviteten**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse dessa indata och utdata-datauppsättningar och schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och **sink** typ har angetts för**SqlSink**.

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
            "name": " SqlServerOutput "
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
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

## <a name="troubleshooting-connection-issues"></a>Felsökning av anslutningsproblem med
1. Konfigurera din SQL Server tooaccept fjärranslutningar. Starta **SQL Server Management Studio**, högerklicka på **server**, och klicka på **egenskaper**. Välj **anslutningar** hello listan och kontrollera **Tillåt fjärranslutningar toohello server**.

    ![Aktivera fjärranslutningar](./media/data-factory-sqlserver-connector/AllowRemoteConnections.png)

    Se [konfigurerar hello fjärråtkomst Server Configuration Option](https://msdn.microsoft.com/library/ms191464.aspx) detaljerade anvisningar.
2. Starta **SQL Server Configuration Manager**. Expandera **SQL Server-nätverkskonfigurationen** för hello-instans du vill och välj **protokoll för MSSQLSERVER**. Du bör se protokoll i hello högra rutan. Aktivera TCP/IP genom att högerklicka på **TCP/IP** och klicka på **aktivera**.

    ![Aktivera TCP/IP](./media/data-factory-sqlserver-connector/EnableTCPProptocol.png)

    Se [aktivera eller inaktivera nätverksprotokoll Server](https://msdn.microsoft.com/library/ms191294.aspx) information och alternativa metoder för att aktivera TCP/IP-protokollet.
3. I hello samma fönster genom att dubbelklicka på **TCP/IP** toolaunch **TCP/IP-egenskaper** fönster.
4. Växla toohello **IP-adresser** fliken. Bläddra nedåt toosee **IPAll** avsnitt. Skriv ner hello ** TCP-Port ** (standard är **1433**).
5. Skapa en **regel för hello Windows-brandväggen** på hello tooallow inkommande trafik via den här porten.  
6. **Verifiera anslutning**: tooconnect toohello SQL Server med fullständigt kvalificerade namnet använda SQL Server Management Studio från en annan dator. Till exempel ”:<machine>.<domain>. Corp.<company>.com, 1433 ”.

   > [!IMPORTANT]

   > Se [flytta data mellan lokala källor och hello moln med Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) detaljerad information.
   >
   > Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.
   >
   >


## <a name="identity-columns-in-hello-target-database"></a>Identitetskolumner i hello måldatabasen
Det här avsnittet innehåller ett exempel som kopierar data från en källtabell med ingen identitet kolumnen tooa måltabellen med en identitetskolumn.

**Källtabellen:**

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
**Måltabellen:**

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

Observera att hello måltabellen har en identitetskolumn.

**Källa JSON-definitionen för datauppsättning**

```json
{
    "name": "SampleSource",
    "properties": {
        "published": false,
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
**Mål JSON-definitionen för datauppsättning**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

Observera att som käll- och tabellen har olika schema (målet har en ytterligare kolumn med identity). I det här scenariot behöver du toospecify **struktur** egenskap i hello mål datauppsättningsdefinitionen, som inte innehåller hello identity-kolumn.

## <a name="invoke-stored-procedure-from-sql-sink"></a>Anropa lagrade procedur från SQL-mottagare
Se [anropa lagrad procedur för SQL-mottagare i en Kopieringsaktivitet](data-factory-invoke-stored-procedure-from-copy-activity.md) artikel ett exempel på att anropa en lagrad procedur från SQL-mottagare i en Kopieringsaktivitet för en pipeline.

## <a name="type-mapping-for-sql-server"></a>Mappning för SQLServer
Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln hello kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:

1. Konvertera från inbyggda typer too.NET källtypen
2. Konvertera från .NET typen toonative Mottagartypen

När du flyttar data & från SQLServer hello används följande mappningar från SQL too.NET typ och vice versa.

hello-mappning är samma som hello Datatypsmappningen i SQL Server för ADO.NET.

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

## <a name="mapping-source-toosink-columns"></a>Mappning källkolumner toosink
toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-copy"></a>Repeterbara kopia
När du kopierar data tooSQL Server-databasen läggs hello kopieringsaktiviteten datatabell toohello mottagare som standard. tooperform en UPSERT Läs [repeterbara skrivåtgärder tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) artikel. 

När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat. I Azure Data Factory, kan du köra en sektor manuellt. Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår. När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs. Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.
