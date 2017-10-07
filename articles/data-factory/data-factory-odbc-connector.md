---
title: "aaaMove data från ODBC datalager | Microsoft Docs"
description: "Läs mer om hur toomove data från ODBC data lagras med Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ad70a598-c031-4339-a883-c6125403cb76
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jingwang
ms.openlocfilehash: bf96e71da449313b6144bb194205c572d2ca2030
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-odbc-data-stores-using-azure-data-factory"></a>Flytta data från ODBC datalager med Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från ett lokalt ODBC lagrar. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.

Du kan kopiera data från ett ODBC data store tooany stöds sink dataarkiv. En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell. Data factory stöder för närvarande endast flytta data från ett ODBC-lagra tooother datalager, men inte för att flytta data från andra lagrar tooan ODBC data dataarkiv. 

## <a name="enabling-connectivity"></a>Aktivera anslutning
Data Factory-tjänsten stöder anslutande tooon lokala ODBC källor med hello Data Management Gateway. Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway. Använda hello gateway tooconnect tooan ODBC datalagret även om den finns i en Azure IaaS-VM.

Du kan installera hello gateway på hello samma lokala datorn eller hello Azure VM som hello ODBC-datalager. Vi rekommenderar dock att du installerar hello gateway på en separat dator/Azure IaaS-VM tooavoid resurskonflikter och bättre prestanda. När du installerar hello gateway på en separat dator ska hello datorn kunna tooaccess hello dator med hello ODBC-datalagret.

Förutom hello Data Management Gateway måste du också tooinstall hello ODBC-drivrutinen för hello datalager på hello gateway-datorn.

> [!NOTE]
> Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en ODBC-datalagret med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. 

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret: 

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden. 
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata. 

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från en ODBC-datalagret finns [JSON-exempel: kopieringsdata från ODBC data lagra tooAzure Blob](#json-example-copy-data-from-odbc-data-store-to-azure-blob) i den här artikeln. 

hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooODBC datalager:

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello följande tabell innehåller en beskrivning för JSON-element specifika tooODBC länkad tjänst.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello Typegenskapen måste anges till: **OnPremisesOdbc** |Ja |
| connectionString |hello-access credential del av hello anslutningssträngen och en valfri krypteras autentiseringsuppgifter. Se exemplen i följande avsnitt hello. |Ja |
| autentiseringsuppgifter |hello access credential delen av hello anslutningssträngen som angetts i drivrutinsspecifika egenskapsvärdet format. Exempel ”: Uid =<user ID>; Pwd =<password>; RefreshToken =<secret refresh token>”;. |Nej |
| AuthenticationType |Typ av autentisering används tooconnect toohello ODBC-datalagret. Möjliga värden är: anonyma och grundläggande. |Ja |
| användarnamn |Ange användarnamnet om du använder grundläggande autentisering. |Nej |
| lösenord |Ange lösenord för hello-användarkonto som du angav för hello användarnamn. |Nej |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello ODBC-datalagret. |Ja |

### <a name="using-basic-authentication"></a>Med grundläggande autentisering

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```
### <a name="using-basic-authentication-with-encrypted-credentials"></a>Med grundläggande autentisering och krypterade autentiseringsuppgifter
Du kan kryptera hello autentiseringsuppgifterna med hjälp av hello [ny AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (version 1.0 av Azure PowerShell) cmdlet eller [ny AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0,9 eller tidigare version av hello Azure PowerShell).  

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a>Använder anonym autentisering

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "mygateway"
        }
    }
}
```


## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).

Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej typeProperties avsnittet för dataset av typen **RelationalTable** (som innefattar ODBC dataset) har hello följande egenskaper

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| tableName |Namnet på hello tabell i hello ODBC-datalagret. |Ja |

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.

Egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet på hello andra sidan varierar med varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

I en Kopieringsaktivitet när datakällan är av typen **RelationalSource** (vilket innefattar ODBC), hello följande egenskaper finns i avsnittet typeProperties:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| DocumentDB |Använda hello anpassad fråga tooread data. |SQL-sträng. Till exempel: Välj * från mytable prefix. |Ja |


## <a name="json-example-copy-data-from-odbc-data-store-tooazure-blob"></a>JSON-exempel: kopieringsdata från ODBC data lagra tooAzure Blob
Det här exemplet innehåller definitioner av JSON som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Den visar hur toocopy från en ODBC-datakällan tooan Azure Blob Storage. Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.

hello exemplet har hello följande data factory-enheter:

1. En länkad tjänst av typen [OnPremisesOdbc](#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar data från ett frågeresultat i ett ODBC data store tooa blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

Som ett första steg bör du ställa in hello data management gateway. hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**ODBC länkade tjänsten** det här exemplet använder hello grundläggande autentisering. Se [ODBC länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.

```json
{
    "name": "OnPremOdbcLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

**Länkad Azure Storage-tjänst**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
    }
}
```

**ODBC-inkommande dataset**

hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i en ODBC-databas och innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.

Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremOdbcLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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
    "name": "AzureBlobOdbcDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odbc/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
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
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```


**Kopiera aktivitet i en pipeline med ODBC-datakällan (RelationalSource) och Blob sink (BlobSink)**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse dessa indata och utdata-datauppsättningar och schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** och **sink** typ har angetts för**BlobSink**. hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "OdbcDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOdbcDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "OdbcToBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-odbc"></a>Mappning för ODBC
Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande två sätt:

1. Konvertera från inbyggda typer too.NET källtypen
2. Konvertera från .NET typen toonative Mottagartypen

När du flyttar data från ODBC datalager ODBC-datatyper är mappade too.NET typer som anges i hello [ODBC mappningar av datatyper](https://msdn.microsoft.com/library/cc668763.aspx) avsnittet.

## <a name="map-source-toosink-columns"></a>Mappa källkolumner toosink
toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Upprepbar läsning från relationella källor
När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat. I Azure Data Factory, kan du köra en sektor manuellt. Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår. När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs. Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="ge-historian-store"></a>GE Historian store
Du skapar en ODBC-länkad tjänst toolink en [GE Proficy Historian (nu GE Historian)](http://www.geautomation.com/products/proficy-historian) tooan Azure data factory datalager som visas i följande exempel hello:

```json
{
    "name": "HistorianLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "connectionString": "DSN=<name of hello GE Historian store>",
            "gatewayName": "<gateway name>",
            "authenticationType": "Basic",
            "userName": "<user name>",
            "password": "<password>"
        }
    }
}
```

Installera Data Management Gateway på en lokal dator och registrera hello gateway med hello-portalen. hello gateway som har installerats på datorn lokalt använder hello ODBC-drivrutinen för att GE Historian tooconnect toohello GE Historian datalagret. Installera drivrutinen för hello därför om det redan inte är installerad på hello gateway-datorn. Se [aktivera anslutningen](#enabling-connectivity) information.

Innan du använder hello GE Historian lagra i en Data Factory-lösning kan du kontrollera om hello gateway kan ansluta toohello datalagret enligt anvisningarna i nästa avsnitt av hello.

Läs hello artikel från början hello en detaljerad översikt av med hjälp av ODBC data lagras som källa för datalager i en kopieringsåtgärd.  

## <a name="troubleshoot-connectivity-issues"></a>Felsökning av problem med nätverksanslutningen
tootroubleshoot anslutningsproblem använda hello **diagnostik** fliken **Data Management Gateway Configuration Manager**.

1. Starta **Data Management Gateway Configuration Manager**. Du kan antingen köra ”C:\Program Files\Microsoft Data Management Gateway\1.0\Shared\ConfigManager.exe” direkt (eller) sökning för **Gateway** toofind en länk för**Microsoft Data Management Gateway** program som visas i följande bild hello.

    ![Sök-gateway](./media/data-factory-odbc-connector/search-gateway.png)
2. Växla toohello **diagnostik** fliken.

    ![Gatewaydiagnostik](./media/data-factory-odbc-connector/data-factory-gateway-diagnostics.png)
3. Välj hello **typen** av data lagras (länkade tjänst).
4. Ange **autentisering** och ange **autentiseringsuppgifter** (eller) ange **anslutningssträngen** som har använt tooconnect toohello datalagret.
5. Klicka på **Testanslutningen** tootest hello anslutning toohello datalagret.

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.
