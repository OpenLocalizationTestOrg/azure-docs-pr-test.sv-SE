---
title: "aaaMove data från en FTP-server med hjälp av Azure Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från en FTP-server med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: eea3bab0-a6e4-4045-ad44-9ce06229c718
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jingwang
ms.openlocfilehash: c707e29532b2a8a870603948cb6150ab857bd6ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a>Flytta data från en FTP-server med hjälp av Azure Data Factory
Den här artikeln förklarar hur toouse hello kopieringsaktiviteten i Azure Data Factory toomove data från en FTP-server. Den bygger på hello [Data movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.

Du kan kopiera data från en FTP-server tooany stöds sink-datalagret. En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell. Data Factory stöder för närvarande endast flytta data från ett FTP-server tooother lagras, men inte flytta data från andra data lagras tooan FTP-servern. Den stöder både lokalt och molntjänster FTP-servrar.

> [!NOTE]
> Hej kopieringsaktiviteten tar inte bort hello källfilen när den inte har kopierade toohello mål. Om du behöver toodelete hello källfilen efter en lyckad kopiering, skapa en anpassad aktivitet toodelete hello-fil och Använd hello aktiviteten i hello pipeline. 

## <a name="enable-connectivity"></a>Aktivera anslutning
Om du flyttar data från en **lokalt** datalagret för FTP-server tooa molnet (exempelvis tooAzure Blob storage kvar), installera och använda Data Management Gateway. hello Data Management Gateway är en klientagent som är installerad på den lokala datorn och det tillåter cloud services tooconnect tooan lokala resursen. Mer information finns i [Data Management Gateway](data-factory-data-management-gateway.md). Stegvisa instruktioner för att ställa in hello gateway och använder den finns [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md). Du kan använda hello gateway tooconnect tooan FTP-server, även om hello-servern är på en Azure-infrastruktur som en tjänst (IaaS) virtuell dator (VM).

Det är möjligt tooinstall hello gateway på hello samma lokala dator eller IaaS-VM hello FTP-servern. Vi rekommenderar dock att du installerar hello gateway på en separat dator eller resurskonflikter för IaaS VM tooavoid och för bättre prestanda. När du installerar hello gateway på en separat dator ska hello datorn kunna tooaccess hello FTP-servern.

## <a name="get-started"></a>Kom igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en FTP-datakälla med hjälp av olika verktyg eller API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **Data Factory kopiera guiden**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.

Om du använder hello verktyg eller API: er, utför hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalager:

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder verktyg eller API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format. Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från en FTP-datalagret finns hello [JSON-exempel: kopiera data från FTP-servern tooAzure blob](#json-example-copy-data-from-ftp-server-to-azure-blob) i den här artikeln.

> [!NOTE]
> Mer information om stöds fil- och komprimering format toouse finns [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md).

hello följande avsnitt innehåller information om JSON-egenskaper som är specifika tooFTP för används toodefine Data Factory-enheter.

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello i den följande tabellen beskrivs JSON-element specifika tooan FTP-länkade tjänsten.

| Egenskap | Beskrivning | Krävs | Standard |
| --- | --- | --- | --- |
| typ |Ange den här tooFtpServer. |Ja |&nbsp; |
| värden |Ange hello namn eller IP-adressen för hello FTP-servern. |Ja |&nbsp; |
| AuthenticationType |Ange hello autentiseringstyp. |Ja |Grundläggande, anonyma |
| användarnamn |Ange hello-användare som har åtkomst till toohello FTP-servern. |Nej |&nbsp; |
| lösenord |Ange hello användarlösenord hello (användarnamn). |Nej |&nbsp; |
| encryptedCredential |Ange hello krypterade autentiseringsuppgifter tooaccess hello FTP-server. |Nej |&nbsp; |
| gatewayName |Ange hello namnet på hello gateway i Data Management Gateway tooconnect tooan lokalt FTP-servern. |Nej |&nbsp; |
| port |Ange hello port på vilken hello FTP-servern lyssnar. |Nej |21 |
| enableSsl |Ange om toouse FTP över SSL/TLS-kanal. |Nej |SANT |
| enableServerCertificateValidation |Ange om tooenable server SSL-certifikatet verifieringen när du använder FTP över SSL/TLS-kanalen. |Nej |SANT |

### <a name="use-anonymous-authentication"></a>Använd anonym autentisering

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {        
            "authenticationType": "Anonymous",
              "host": "myftpserver.com"
        }
    }
}
```

### <a name="use-username-and-password-in-plain-text-for-basic-authentication"></a>Använd användarnamn och lösenord i klartext för grundläggande autentisering

```JSON
{
    "name": "FTPLinkedService",
      "properties": {
    "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
}
```

### <a name="use-port-enablessl-enableservercertificatevalidation"></a>Använd port, enableSsl, enableServerCertificateValidation

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

### <a name="use-encryptedcredential-for-authentication-and-gateway"></a>Använda encryptedCredential för autentisering och gateway

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "mygateway"
        }
      }
}
```

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns [skapa datauppsättningar](data-factory-create-datasets.md). Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen.

Hej **typeProperties** avsnittet är olika för varje typ av datauppsättningen. Den innehåller information som är specifik toohello dataset-typen. Hej **typeProperties** avsnittet för en dataset av typen **filresursen** har hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| folderPath |Undersökvägen toohello mapp. Använda escape-tecknet ' \ ' för specialtecken i hello-sträng. Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.<br/><br/>Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn start- och sluttider datum. |Ja |
| fileName |Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello. Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.<br/><br/>När **fileName** har inte angetts för en datamängd för utdata hello hello genereras filen heter i hello följande format: <br/><br/>Data. <Guid>.txt (exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Nej |
| fileFilter |Ange ett filter toobe används tooselect en delmängd av filer i hello **folderPath**, i stället för alla filer.<br/><br/>Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).<br/><br/>Exempel 1:`"fileFilter": "*.log"`<br/>Exempel 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> **fileFilter** gäller för en inkommande filresursen datauppsättning. Den här egenskapen stöds inte med Hadoop Distributed File System (HDFS). |Nej |
| partitionedBy |Används toospecify en dynamisk **folderPath** och **fileName** för tid series-data. Du kan till exempel ange en **folderPath** som har fått parametrar för varje timme av data. |Nej |
| Format | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Ange hello **typen** egenskap under format tooone av dessa värden. Mer information finns i hello [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format ](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. <br><br> Om du vill toocopy filer som de är mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner. |Nej |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**, och stöds nivåerna **Optimal** och **snabbaste**. Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |
| useBinaryTransfer |Ange om toouse hello binary överföringsläge. hello-värden är true för en binär (detta är standardvärdet för hello) och false för ASCII. Den här egenskapen kan endast användas när hello associerade linked service-typen är av typen: FtpServer. |Nej |

> [!NOTE]
> **Filnamn** och **fileFilter** kan inte användas samtidigt.

### <a name="use-hello-partionedby-property"></a>Använd hello partionedBy egenskapen
Som nämnts tidigare under hello, kan du ange en dynamisk **folderPath** och **fileName** för tid series-data med hello **partitionedBy** egenskapen.

toolearn om tid serien datauppsättningar schemaläggning och segment, se [skapa datauppsättningar](data-factory-create-datasets.md), [schemaläggning och körning](data-factory-scheduling-and-execution.md), och [skapar pipelines](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Exempel 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
I det här exemplet {segment} ersätts med hello värdet för variabeln SliceStart Data Factory, i hello formatera angivna (YYYYMMDDHH). Hej SliceStart refererar toostart tiden för hello sektorn. hello mappsökvägen är olika för varje segment. (Till exempel 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout.)

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
I det här exemplet hello år, månad, dag och tidpunkt för SliceStart extraheras till olika variabler som används av hello **folderPath** och **fileName** egenskaper.

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter, se [skapar pipelines](data-factory-create-pipelines.md). Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.

Egenskaper som är tillgängliga i hello **typeProperties** avsnitt i hello aktivitet på hello andra sidan, varierar med varje aktivitetstyp. För hello kopieringsaktiviteten varierar hello Typegenskaper beroende på hello typer av datakällor och sänkor.

I en Kopieringsaktivitet när hello källa är av typen **FileSystemSource**, hello efter egenskapen finns i **typeProperties** avsnitt:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| Rekursiva |Anger om hello data läses rekursivt från hello undermappar eller endast från hello angivna mappen. |SANT, FALSKT (standard) |Nej |

## <a name="json-example-copy-data-from-ftp-server-tooazure-blob"></a>JSON-exempel: kopiera data från FTP-servern tooAzure Blob
Det här exemplet visas hur toocopy data från en FTP-server tooAzure Blob storage. Dock datan kan kopieras direkt tooany av hello egenskaperna anges i hello [datalager och format stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats), med hjälp av hello kopieringsaktiviteten i Data Factory.  

hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):

* En länkad tjänst av typen [FtpServer](#linked-service-properties)
* En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Indata [dataset](data-factory-create-datasets.md) av typen [filresursen](#dataset-properties)
* Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

hello exemplet kopierar data från en FTP-server tooan Azure blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

### <a name="ftp-linked-service"></a>Länkad FTP-tjänsten

Det här exemplet använder grundläggande autentisering med hello användarnamn och lösenord i klartext. Du kan också använda något av följande sätt hello:

* Anonym autentisering
* Grundläggande autentisering med krypterade autentiseringsuppgifter
* FTP över SSL/TLS (FTPS)

Se hello [FTP länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
    "type": "FtpServer",
    "typeProperties": {
        "host": "myftpserver.com",           
        "authenticationType": "Basic",
        "username": "Admin",
        "password": "123456"
    }
  }
}
```
### <a name="azure-storage-linked-service"></a>Länkad Azure-lagringstjänst

```JSON
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
### <a name="ftp-input-dataset"></a>FTP-inkommande dataset

Den här datamängden refererar toohello FTP mappen `mysharedfolder` och `test.csv`. hello pipeline kopierar hello filen toohello mål.

Ange **externa** för**SANT** informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
{
  "name": "FTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "FTPLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv",
      "useBinaryTransfer": true
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

### <a name="azure-blob-output-dataset"></a>Utdatauppsättning för Azure-blobb

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappsökväg för hello blob utvärderas dynamiskt, baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder hello år, månad, dag och timmar delar av hello starttid.

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a>En kopia aktivitet i en pipeline med filen system käll- och blob sink

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**FileSystemSource**, och hello **sink** typ har angetts för**BlobSink**.

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
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
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00Z",
        "end": "2016-08-24T19:00:00Z"
    }
}
```
> [!NOTE]
> toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).

## <a name="next-steps"></a>Nästa steg
Se hello följande artiklar:

* toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (kopieringsaktiviteten) i Data Factory och olika sätt toooptimize, se hello [kopiera aktivitet prestanda och prestandajustering guiden](data-factory-copy-activity-performance.md).

* Stegvisa instruktioner för att skapa en pipeline med en kopieringsaktiviteten finns hello [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
