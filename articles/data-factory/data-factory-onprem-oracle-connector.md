---
title: "aaaCopy data till/från Oracle med hjälp av Data Factory | Microsoft Docs"
description: "Lär dig hur toocopy data till och från Oracle-databas som är lokalt med Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: adb6d5fbe38e18791616ac77e8179970bbea37fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Kopiera data till och från lokala Oracle med Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data till eller från en lokal Oracle-databas. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.

## <a name="supported-scenarios"></a>Scenarier som stöds
Du kan kopiera data **från en Oracle-databas** toohello följande datakällor:

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

Du kan kopiera data från hello följande datalager **tooan Oracle-databas**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a>Krav
Data Factory stöder anslutande tooon lokal Oracle källor med hello Data Management Gateway. Se [Data Management Gateway](data-factory-data-management-gateway.md) artikel toolearn om Data Management Gateway och [flytta data från lokala toocloud](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner om hur du konfigurerar hello gateway en data-pipeline toomove data.

Gateway krävs även om hello Oracle finns i en Azure IaaS-VM. Du kan installera hello gateway på hello samma IaaS VM som hello data lagras eller på en annan virtuell dator så länge som hello gateway kan ansluta toohello databas.

> [!NOTE]
> Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.

## <a name="supported-versions-and-installation"></a>Versioner som stöds och installation
Den här anslutningen Oracle stöder två versioner av drivrutiner:

- **Microsoft-drivrutin för Oracle (rekommenderas)**: från Data Management Gateway version 2.7, en Microsoft-drivrutin för Oracle installeras automatiskt tillsammans med hello gateway, så behöver du inte tooadditionally referensen hello drivrutin tooestablish anslutning tooOracle, och du kan också prestanda förbättras kopiera med hjälp av den här drivrutinen. Nedan versioner av Oracle stöds databaser:
    - Oracle 12c R1 (12.1)
    - Oracle 11g R1, R2 (11.1, 11.2)
    - Oracle 10g R1, R2 (10.1, 10,2)
    - Oracle 9i R1, R2 (9.0.1, 9.2)
    - Oracle 8i R3 (8.1.7)

> [!IMPORTANT]
> Microsoft-drivrutin för Oracle stöder för närvarande endast kopiering av data från Oracle men inte skriva tooOracle. Och Observera hello Testa anslutning funktionen fliken diagnostik för Data Management Gateway har inte stöd för den här drivrutinen. Du kan också använda hello kopiera guiden toovalidate hello anslutningen.
>

- **Oracle Data Provider för .NET:** du kan också välja toouse Oracle Data Provider toocopy data från / tooOracle. Den här komponenten ingår i [Oracle Data Access-komponenter för Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Installera hello rätt version (32/64-bitars) på hello datorn där hello gatewayen har installerats. [Oracle-dataprovidern .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) kan komma åt tooOracle Database 10 g version 2 eller senare.

    Om du väljer ”XCopy-Installation”, följer du anvisningarna i hello viktigt.htm. Vi rekommenderar att du väljer hello installer med användargränssnitt (icke-XCopy en).

    När du har installerat hello providern **starta om** hello Data Management Gateway-värdtjänsten på datorn med tjänster appleten (eller) Data Management Gateway Configuration Manager.  

Om du använder kopiera guiden tooauthor hello kopiera pipeline kommer hello drivrutinen typen att automatiskt identifiera. Microsoft-drivrutinen används som standard om inte din gatewayversionen är lägre än 2.7 eller Oracle som mottagare som du anger.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till eller från en lokal Oracle-databas med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:

1. Skapa en **datafabriken**. En datafabrik kan innehålla en eller flera pipelines. 
2. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory. Till exempel om du kopierar data från en Oralce databasen tooan Azure-blobblagring skapa du två länkade tjänster toolink till Oracle-databasen och Azure storage-konto tooyour data factory. Länkad tjänstegenskaper som är specifika tooOracle, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt.
3. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello tabell i Oracle-databasen som innehåller hello indata. Och, skapar du en annan dataset toospecify hello blob-behållaren och hello-mappen som innehåller hello data kopieras från hello Oracle-databas. Egenskaper för datamängd som är specifika tooOracle, se [egenskaper för datamängd](#dataset-properties) avsnitt.
4. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. I hello-exemplet ovan, använder du OracleSource som en källa och BlobSink som en mottagare för hello kopieringsaktiviteten. På samma sätt om du kopierar från Azure Blob Storage tooOracle databasen använder du BlobSource och OracleSink i hello kopieringsaktiviteten. Kopiera Aktivitetsegenskaper som är specifika tooOracle databasen, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt. Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager. 

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från en lokal Oracle-databas finns [JSON-exempel](#json-examples-for-copying-data-to-and-from-oracle-database) i den här artikeln.

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory-enheter:

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello följande tabell innehåller en beskrivning för JSON-element specifika tooOracle länkad tjänst.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello Typegenskapen måste anges till: **OnPremisesOracle** |Ja |
| driverType | Ange vilka drivrutinen toouse toocopy data från / tooOracle databasen. Tillåtna värden är **Microsoft** eller **ODP** (standard). Se [stöds version och vilka installationsalternativ](#supported-versions-and-installation) avsnitt för mer information. | Nej |
| connectionString | Ange nödvändig information tooconnect toohello Oracle-databasinstans för hello-egenskapen connectionString. | Ja |
| gatewayName | Namnet på hello gateway som som används tooconnect toohello lokal Oracle-server |Ja |

**Exempel: använda Microsoft-drivrutinen:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Exempel: med ODP drivrutin**

Se för[platsen](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) för hello tillåtet format.

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen (Oracle, Azure blob, Azure-tabellen, osv.).

hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej typeProperties avsnittet för hello dataset av typen OracleTable har hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i hello Oracle-databas som hello länkade tjänsten refererar till. |Nej (om **oracleReaderQuery** av **OracleSource** har angetts) |

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.

> [!NOTE]
> Hej Kopieringsaktiviteten tar endast en inmatning och ger en enda utdata.

De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

### <a name="oraclesource"></a>OracleSource
I en Kopieringsaktivitet när hello källa är av typen **OracleSource** hello följande egenskaper är tillgängliga i **typeProperties** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| oracleReaderQuery |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: Välj * från mytable prefix <br/><br/>Om inget anges hello SQL-instruktionen som körs: Välj * från mytable prefix |Nej (om **tableName** av **dataset** har angetts) |

### <a name="oraclesink"></a>OracleSink
**OracleSink** stöder hello följande egenskaper:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| writeBatchTimeout |Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås. |TimeSpan<br/><br/> Exempel: 00:30:00 (30 minuter). |Nej |
| writeBatchSize |Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize. |Heltal (antalet rader) |Nej (standard: 100) |
| sqlWriterCleanupScript |Ange en fråga för aktiviteten kopiera tooexecute så att data för ett visst segment har rensats bort. |En frågesats. |Nej |
| sliceIdentifierColumnName |Ange kolumnnamn för Kopieringsaktiviteten toofill med genereras automatiskt segment-ID som har använt tooclean in data för ett visst segment när köras på nytt. |Kolumnnamnet för en kolumn med datatypen för binary(32). |Nej |

## <a name="json-examples-for-copying-data-tooand-from-oracle-database"></a>JSON-exempel för att kopiera data tooand från Oracle-databas
hello följande exempel innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De visar hur toocopy data från / tooan Oracle-databas till/från Azure Blob Storage. Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.   

## <a name="example-copy-data-from-oracle-tooazure-blob"></a>Exempel: Kopiera data från Oracle tooAzure Blob

hello exemplet har hello följande data factory-enheter:

1. En länkad tjänst av typen [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) som källa och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) som mottagare.

hello exemplet kopierar data från en tabell i en lokal Oracle-databasen tooa blob varje timme. Mer information om olika egenskaper som används i hello-exempel finns i dokumentationen i hello-exempel i följande avsnitt.

**Oracle länkad tjänst:**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure Blob storage länkade tjänsten:**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Oracle inkommande datauppsättningen:**

hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Oracle och innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.

Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2014-02-27T12:00:00",
            "frequency": "Hour"
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

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.

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

**Pipeline med kopieringsaktiviteten:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**OracleSource** och **sink** typ har angetts för**BlobSink**.  hello SQL-frågan som anges med **oracleReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "OracletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": " OracleInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "OracleSource",
                        "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

## <a name="example-copy-data-from-azure-blob-toooracle"></a>Exempel: Kopiera data från Azure Blob-tooOracle
Det här exemplet visas hur toocopy data från ett Azure Blob Storage-tooan lokal Oracle-databas. Dock datan kan kopieras **direkt** från någon av hello källor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.  

hello exemplet har hello följande data factory-enheter:

1. En länkad tjänst av typen [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) som källa [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) som mottagare.

hello exemplet kopierar data från en tabell med blob tooa i en lokal Oracle-databas i timmen. Mer information om olika egenskaper som används i hello-exempel finns i dokumentationen i hello-exempel i följande avsnitt.

**Oracle länkad tjänst:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure Blob storage länkade tjänsten:**
```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Azure Blob-inkommande datamängd**

Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1). hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad och som en dagdel av hello starttid och filnamn använder hello timme tillhör hello starttid. ”externa”: ”true” inställningen informerar hello Data Factory-tjänsten att den här tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
            "frequency": "Day",
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

**Oracle datamängd för utdata:**

hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Oracle. Skapa hello tabell i Oracle med hello samma antal kolumner som du förväntar dig hello Blob CSV-filen toocontain. Nya rader läggs toohello tabell varje timme.

```json
{
    "name": "OracleOutput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Day",
            "interval": "1"
        }
    }
}
```

**Pipeline med kopieringsaktiviteten:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och hello **sink** typ har angetts för**OracleSink**.  

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
            {
                "name": "AzureBlobtoOracle",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "OracleOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "OracleSink"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
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


## <a name="troubleshooting-tips"></a>Felsökningstips
### <a name="problem-1-net-framework-data-provider"></a>Problem 1: .NET Framework dataprovider

Du ser följande hello **felmeddelande**:

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable toofind hello requested .Net Framework Data Provider. It may not be installed”.  

**Möjliga orsaker:**

1. hello .NET Framework Data Provider för Oracle har inte installerats.
2. hello .NET Framework Data Provider för Oracle var installerade too.NET Framework 2.0 och är inte finns i hello .NET Framework 4.0 mappar.

**Lösning/lösning:**

1. Om du inte har installerat hello .NET-Provider för Oracle, [installera den](http://www.oracle.com/technetwork/topics/dotnet/downloads/) och försök hello scenario.
2. Om du får felmeddelandet hello även när du har installerat providern hello hello gör du följande steg:
   1. Öppna datorn config av .NET 2.0 från hello mapp: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
   2. Sök efter **Oracle Data Provider för .NET**, och du bör vara kan toofind en post som visas i följande exempel under hello **system.data** -> **DbProviderFactories**”:<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle dataprovider för .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
3. Kopiera den här posten toohello machine.config-filen i hello följande v4.0 mapp: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config och ändra hello version too4.xxx.x.x.
4. Installera ”< ODP.NET installerad sökväg > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll” i hello globala sammansättningscachen (GAC) genom att köra `gacutil /i [provider path]`. ## felsökningstips

### <a name="problem-2-datetime-formatting"></a>Problem 2: datetime formatering

Du ser följande hello **felmeddelande**:

    Message=Operation failed in Oracle Database with hello following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**Lösning/lösning:**

Du kanske måste tooadjust hello frågesträng i din kopieringsaktiviteten baserat på hur datum är konfigurerade i Oracle-databas enligt hello följande exempel (med hjälp av funktionen för hello to_date):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a>Mappning för Oracle
Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:

1. Konvertera från inbyggda typer too.NET källtypen
2. Konvertera från .NET typen toonative Mottagartypen

När du flyttar data från Oracle, används följande mappningar hello från Oracle-typen too.NET datatyp och vice versa.

| Oracle-datatyp | .NET framework-datatyp |
| --- | --- |
| BFILE |byte] |
| BLOB |byte] |
| CHAR |Sträng |
| CLOB |Sträng |
| DATUM |Datum och tid |
| FLYTTAL |Decimal, sträng (om precision > 28) |
| HELTAL |Decimal, sträng (om precision > 28) |
| INTERVALL år tooMONTH |Int32 |
| INTERVALL dag tooSECOND |TimeSpan |
| LÅNG |Sträng |
| LÅNGT RÅDATA |byte] |
| NCHAR |Sträng |
| NCLOB |Sträng |
| ANTAL |Decimal, sträng (om precision > 28) |
| NVARCHAR2 |Sträng |
| RÅDATA |byte] |
| ROWID |Sträng |
| TIDSSTÄMPEL |Datum och tid |
| TIDSSTÄMPEL MED LOKALA TIDSZON |Datum och tid |
| TIDSSTÄMPEL MED TIDSZON |Datum och tid |
| OSIGNERAT HELTAL |Tal |
| VARCHAR2 |Sträng |
| XML |Sträng |

> [!NOTE]
> Datatypen **intervall år tooMONTH** och **intervall dag tooSECOND** stöds inte när du använder Microsoft-drivrutinen.

## <a name="map-source-toosink-columns"></a>Mappa källkolumner toosink
toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Upprepbar läsning från relationella källor
När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat. I Azure Data Factory, kan du köra en sektor manuellt. Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår. När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs. Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.
