---
title: "aaaMove data från SFTP-servern med hjälp av Azure Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från en lokal eller en molnet SFTP-server med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jingwang
ms.openlocfilehash: b976289e2c1b1899634bb5565b375499077fa554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a>Flytta data från en SFTP-server med hjälp av Azure Data Factory
Den här artikeln beskrivs hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en på lokalt/i molnet SFTP server tooa stöds sink-datalagret. Den här artikeln bygger på hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med kopiera aktivitet och hello lista över datakällor som stöds som källor/sänkor.

Data factory stöder för närvarande endast flytta data från ett SFTP server tooother lagrar, men inte för att flytta data från andra data lagras tooan SFTP-server. Den stöder både lokalt och molntjänster SFTP-servrar.

> [!NOTE]
> Kopieringsaktiviteten tar inte bort hello källfilen när den inte har kopierade toohello mål. Om du behöver toodelete hello källfilen efter en lyckad kopiering, skapa en anpassad aktivitet toodelete hello-fil och använda hello aktivitet i hello pipeline. 

## <a name="supported-scenarios-and-authentication-types"></a>Scenarier som stöds och typer av autentisering
Du kan använda den här SFTP connector toocopy data från **både molnet SFTP-servrar och lokala SFTP servrar**. **Grundläggande** och **SshPublicKey** autentiseringstyper som stöds när du ansluter toohello SFTP-server.

När du kopierar data från en lokal SFTP-server, måste du installera en Data Management Gateway i hello lokala miljö/Azure VM. Se [Data Management Gateway](data-factory-data-management-gateway.md) information om hello gateway. Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner för att ställa in hello gateway och använder den.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en SFTP-datakälla med hjälp av olika verktyg/API: er.

- hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

- Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. JSON-exempel toocopy data från SFTP server tooAzure Blob Storage, finns [JSON-exempel: kopiera data från SFTP server tooAzure blob](#json-example-copy-data-from-sftp-server-to-azure-blob) i den här artikeln.

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello följande tabell innehåller en beskrivning för JSON-element specifika tooFTP länkad tjänst.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- | --- |
| typ | hello Typegenskapen måste anges för`Sftp`. |Ja |
| värden | Namn eller IP-adressen för hello SFTP-server. |Ja |
| port |Port på vilken hello SFTP servern lyssnar. hello standardvärdet är: 21 |Nej |
| AuthenticationType |Ange autentiseringstypen. Tillåtna värden: **grundläggande**, **SshPublicKey**. <br><br> Se för[använder grundläggande autentisering](#using-basic-authentication) och [med hjälp av SSH autentisering med offentlig nyckel](#using-ssh-public-key-authentication) respektive avsnitt på fler egenskaper och JSON-exempel. |Ja |
| skipHostKeyValidation | Ange om tooskip värd viktiga validering. | Nej. Hej standardvärdet: false |
| hostKeyFingerprint | Ange hello fingeravtryck hello värden nyckel. | Ja om hello `skipHostKeyValidation` toofalse anges.  |
| gatewayName |Namnet på hello Data Management Gateway tooconnect tooan lokalt SFTP-server. | Ja om du kopierar data från en lokal SFTP-server. |
| encryptedCredential | Krypterade autentiseringsuppgifter tooaccess hello SFTP-server. Genereras automatiskt när du anger grundläggande autentisering (användarnamn och lösenord) eller SshPublicKey autentisering (användarnamn + privat sökväg eller innehåll) i Kopiera guiden eller hello ClickOnce popup-dialogruta. | Nej. Gäller bara när du kopierar data från en lokal SFTP-server. |

### <a name="using-basic-authentication"></a>Med grundläggande autentisering

toouse grundläggande autentisering, ange `authenticationType` som `Basic`, och ange följande egenskaper utöver hello SFTP connector generiska som introducerades i hello sista avsnittet hello:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- | --- |
| användarnamn | Användare som har åtkomst till toohello SFTP-servern. |Ja |
| lösenord | Lösenordet för hello (användarnamn). | Ja |

#### <a name="example-basic-authentication"></a>Exempel: Grundläggande autentisering
```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Exempel: Grundläggande autentisering med krypterade autentiseringsuppgifter

```JSON
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
      }
}
```

### <a name="using-ssh-public-key-authentication"></a>Med hjälp av autentisering med SSH offentlig nyckel

toouse SSH autentisering med offentlig nyckel, ange `authenticationType` som `SshPublicKey`, och ange följande egenskaper utöver hello SFTP connector generiska som introducerades i hello sista avsnittet hello:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- | --- |
| användarnamn |Användare som har åtkomst till toohello SFTP-servern |Ja |
| privateKeyPath | Ange absolut sökväg toohello fil för privat nyckel som gateway kan komma åt. | Ange antingen hello `privateKeyPath` eller `privateKeyContent`. <br><br> Gäller bara när du kopierar data från en lokal SFTP-server. |
| privateKeyContent | En serialiserad textsträng hello privata nyckel innehåll. hello guiden Kopiera kan läsa hello-fil för privat nyckel och extrahera hello privata nyckel innehållet automatiskt. Om du använder någon annan verktyget/SDK, Använd hello privateKeyPath egenskapen i stället. | Ange antingen hello `privateKeyPath` eller `privateKeyContent`. |
| Lösenfrasen | Ange hello pass frasen/lösenord toodecrypt hello privata nyckel om hello nyckelfilen skyddas av ett lösenord. | Ja om hello privata nyckeln skyddas av ett lösenord. |

> [!NOTE]
> SFTP-anslutningen har endast stöd för OpenSSH-nyckel. Kontrollera att din nyckelfilen är hello rätt format. Du kan använda Putty verktyget tooconvert från .ppk tooOpenSSH format.

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a>Exempel: SshPublicKey autentisering med hjälp av privat nyckel filsökväg

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Exempel: SshPublicKey autentisering med hjälp av privat nyckel innehåll

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of hello private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen.

Hej **typeProperties** avsnittet är olika för varje typ av datauppsättningen. Den innehåller information som är specifik toohello dataset-typen. Hej typeProperties avsnittet för en dataset av typen **filresursen** datamängden har hello följande egenskaper:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| folderPath |Sökvägen toohello undermapp. Använda escape-tecknet ' \ ' för specialtecken i hello-sträng. Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.<br/><br/>Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger. |Ja |
| fileName |Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello. Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.<br/><br/>Om filnamnet inte anges för en utdatauppsättningen skulle hello namnet på hello genereras vara i hello efter det här formatet: <br/><br/>Data. <Guid>.txt (exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nej |
| fileFilter |Ange ett filter toobe används tooselect en delmängd av filer i hello folderPath i stället för alla filer.<br/><br/>Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).<br/><br/>Exempel 1:`"fileFilter": "*.log"`<br/>Exempel 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter gäller för en inkommande filresursen datauppsättning. Den här egenskapen stöds inte med HDFS. |Nej |
| partitionedBy |partitionedBy kan vara används toospecify en dynamisk folderPath filnamn för tid series-data. Till exempel folderPath som innehåller parametrar för varje timme av data. |Nej |
| Format | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Ange hello **typen** egenskap under format tooone av dessa värden. Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. <br><br> Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner. |Nej |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**. Nivåer som stöds är: **Optimal** och **snabbast**. Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |
| useBinaryTransfer |Ange om använder binära överföringsläge. True för en binär och FALSKT ASCII. Standardvärde: True. Den här egenskapen kan endast användas när associerade linked service-typen är av typen: FtpServer. |Nej |

> [!NOTE]
> filnamnet och fileFilter kan inte användas samtidigt.

### <a name="using-partionedby-property"></a>Med hjälp av egenskapen partionedBy
Du kan ange en dynamisk folderPath filnamn för tidsdata serien med partitionedBy som nämns i föregående avsnitt i hello. Du kan göra det med hello Data Factory makron och hello systemvariabeln SliceStart, SliceEnd som visar hello logiska tidsperiod för en viss datasektorn.

toolearn om tid serien datauppsättningar schemaläggning och segment, se [skapa datauppsättningar](data-factory-create-datasets.md), [schemaläggning och utförande](data-factory-scheduling-and-execution.md), och [skapar Pipelines](data-factory-create-pipelines.md) artiklar.

#### <a name="sample-1"></a>Exempel 1:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
I det här exemplet {segment} ersätts med hello värde för Data Factory systemvariabel SliceStart hello-format (YYYYMMDDHH). Hej SliceStart refererar toostart tiden för hello sektorn. hello folderPath är olika för varje segment. Exempel: 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout.

#### <a name="sample-2"></a>Exempel 2:

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
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.

Medan hello egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp. För kopieringsaktiviteten varierar hello Typegenskaper beroende på hello typer av datakällor och sänkor.

[!INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a>Fil- och komprimering format som stöds
Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikeln för information.

## <a name="json-example-copy-data-from-sftp-server-tooazure-blob"></a>JSON-exempel: Kopiera data från SFTP server tooAzure blob
hello följande exempel innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De visar hur toocopy från SFTP datakällan tooAzure Blob Storage. Dock datan kan kopieras **direkt** från alla källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.

> [!IMPORTANT]
> Det här exemplet innehåller JSON kodavsnitt. Stegvisa instruktioner för att skapa datafabriken hello inkluderas inte. Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.

hello exemplet har hello följande data factory-enheter:

* En länkad tjänst av typen [sftp](#linked-service-properties).
* En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Indata [dataset](data-factory-create-datasets.md) av typen [filresursen](#dataset-properties).
* Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar data från en SFTP server tooan Azure blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

**SFTP länkad tjänst**

Det här exemplet använder hello grundläggande autentisering med användarnamn och lösenord i klartext. Du kan också använda något av följande sätt hello:

* Grundläggande autentisering med krypterade autentiseringsuppgifter
* Autentisering med SSH offentlig nyckel

Se [FTP länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.

```JSON

{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "myuser",
            "password": "mypassword",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```
**Länkad Azure Storage-tjänst**

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
**SFTP inkommande dataset**

Den här datamängden refererar toohello SFTP mappen `mysharedfolder` och `test.csv`. hello pipeline kopierar hello filen toohello mål.

Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
{
  "name": "SFTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "SftpLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Azure Blob utdatauppsättningen**

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Pipeline med kopieringsaktiviteten**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**FileSystemSource** och **sink** typ har angetts för**BlobSink**.

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
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
        "start": "2017-02-20T18:00:00Z",
        "end": "2017-02-20T19:00:00Z"
    }
}
```

## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.

## <a name="next-steps"></a>Nästa steg
Se hello följande artiklar:

* [Kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) stegvisa instruktioner för att skapa en pipeline med en kopia-aktivitet.
