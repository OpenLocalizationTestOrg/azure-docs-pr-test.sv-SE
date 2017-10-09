---
title: "aaaMove data från lokala HDFS | Microsoft Docs"
description: "Läs mer om hur toomove data från lokala HDFS med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3215b82d-291a-46db-8478-eac1a3219614
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 96387e5dd089099fc2e983ab26d67c2044b973b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Flytta data från lokala HDFS med hjälp av Azure Data Factory
Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal HDFS. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.

Du kan kopiera data från datalagret för HDFS tooany stöds mottagare. En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell. Data factory stöder för närvarande endast flytta data från ett datalager för lokala HDFS tooother, men inte för att flytta data från andra data lagras tooan lokala HDFS.

> [!NOTE]
> Kopieringsaktiviteten tar inte bort hello källfilen när den inte har kopierade toohello mål. Om du behöver toodelete hello källfilen efter en lyckad kopiering, skapa en anpassad aktivitet toodelete hello-fil och använda hello aktivitet i hello pipeline. 

## <a name="enabling-connectivity"></a>Aktivera anslutning
Data Factory-tjänsten har stöd för den anslutande tooon lokala HDFS med hello Data Management Gateway. Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway. Använda hello gateway tooconnect tooHDFS även om den finns i en Azure IaaS-VM.

> [!NOTE]
> Se till att hello Data Management Gateway kan komma åt för**alla** hello [namn på noden server]: [name nod port] och [nod dataservrar]: [data nod port] hello Hadoop-klustret. Standard [namn på noden port] är 50070 och standardvärdet [data nod port] är 50075.

Medan du kan installera gatewayen på hello samma lokala dator eller hello Azure VM hello HDFS rekommenderar vi att du installerar hello gateway på en separat dator/Azure IaaS-VM. Med gatewayen på en separat dator minskar resurskonflikter och förbättrar prestandan. När du installerar hello gateway på en separat dator ska hello datorn kunna tooaccess hello dator med hello HDFS.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en källa för HDFS med hjälp av olika verktyg/API: er.

hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.

Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:

1. Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.
2. Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.
3. Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.

När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig. När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.  Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett HDFS-datalager finns [JSON-exempel: kopiera data från lokala HDFS tooAzure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) i den här artikeln.

hello följande avsnitt innehåller information om JSON-egenskaper som är specifika tooHDFS för används toodefine Data Factory-enheter:

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
En länkad tjänst länkar en data store tooa data factory. Du skapar en länkad tjänst av typen **Hdfs** toolink ett lokalt HDFS tooyour data factory. hello följande tabell innehåller en beskrivning för JSON-element specifika tooHDFS länkad tjänst.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ |hello Typegenskapen måste anges till: **Hdfs** |Ja |
| URL |URL: en toohello HDFS |Ja |
| AuthenticationType |Anonym, eller Windows. <br><br> toouse **Kerberos-autentisering** HDFS-anslutningen finns för[i det här avsnittet](#use-kerberos-authentication-for-hdfs-connector) tooset upp din lokala miljö därefter. |Ja |
| Användarnamn |Användarnamn för Windows-autentisering. |Ja (för Windows-autentisering) |
| lösenord |Lösenordet för Windows-autentisering. |Ja (för Windows-autentisering) |
| gatewayName |Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello HDFS. |Ja |
| encryptedCredential |[Nya AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) utdata från hello autentiseringsuppgifter. |Nej |

### <a name="using-anonymous-authentication"></a>Använder anonym autentisering

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-windows-authentication"></a>Med Windows-autentisering

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```
## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).

Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej typeProperties avsnittet för dataset av typen **filresursen** (som omfattar HDFS dataset) har hello följande egenskaper

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| folderPath |Sökvägen toohello mapp. Exempel:`myfolder`<br/><br/>Använda escape-tecknet ' \ ' för specialtecken i hello-sträng. Till exempel: Ange mapp för folder\subfolder,\\\\undermapp och ange d: för d:\samplefolder,\\\\Exempelmapp.<br/><br/>Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger. |Ja |
| fileName |Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello. Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.<br/><br/>Om filnamnet inte anges för en utdatauppsättningen skulle hello namnet på hello genereras vara i hello efter det här formatet: <br/><br/>Data. <Guid>.txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nej |
| partitionedBy |partitionedBy kan vara används toospecify en dynamisk folderPath filnamn för tid series-data. Exempel: folderPath parametriserade varje timme av data. |Nej |
| Format | hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Ange hello **typen** egenskap under format tooone av dessa värden. Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. <br><br> Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner. |Nej |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**. Nivåer som stöds är: **Optimal** och **snabbast**. Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |

> [!NOTE]
> filnamnet och fileFilter kan inte användas samtidigt.

### <a name="using-partionedby-property"></a>Med hjälp av egenskapen partionedBy
Som nämnts tidigare under hello, du kan ange en dynamisk folderPath och ett filnamn för tid series-data med hello **partitionedBy** egenskapen [Data Factory-funktioner och hello systemvariabler](data-factory-functions-variables.md).

toolearn mer om tid serien datauppsättningar, schemaläggning och segment, se [skapa datauppsättningar](data-factory-create-datasets.md), [schemaläggning och utförande](data-factory-scheduling-and-execution.md), och [skapar Pipelines](data-factory-create-pipelines.md) artiklar.

#### <a name="sample-1"></a>Exempel 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
I det här exemplet {segment} ersätts med hello värde för Data Factory systemvariabel SliceStart hello-format (YYYYMMDDHH). Hej SliceStart refererar toostart tiden för hello sektorn. hello folderPath är olika för varje segment. Till exempel: 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout.

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
I det här exemplet extraheras år, månad, dag och tidpunkt för SliceStart till olika variabler som används av egenskaperna folderPath och filnamn.

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.

De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

För Kopieringsaktiviteten när datakällan är av typen **FileSystemSource** hello följande egenskaper finns i avsnittet typeProperties:

**FileSystemSource** stöder hello följande egenskaper:

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| Rekursiva |Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen. |SANT, FALSKT (standard) |Nej |

## <a name="supported-file-and-compression-formats"></a>Fil- och komprimering format som stöds
Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikeln för information.

## <a name="json-example-copy-data-from-on-premises-hdfs-tooazure-blob"></a>JSON-exempel: kopiera data från lokala HDFS tooAzure Blob
Det här exemplet visas hur toocopy data från en lokal HDFS tooAzure Blob Storage. Dock datan kan kopieras **direkt** tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.  

hello exempel innehåller JSON definitioner för hello följande Data Factory entiteter. Du kan använda dessa definitioner toocreate en pipeline toocopy data från HDFS tooAzure Blob Storage med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).

1. En länkad tjänst av typen [OnPremisesHdfs](#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [filresursen](#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar data från en lokal HDFS tooan Azure blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

Som ett första steg bör du ställa in hello data management gateway. Hej instruktionerna i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**HDFS länkade tjänsten:** det här exemplet använder hello Windows-autentisering. Se [HDFS länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.

```JSON
{
    "name": "HDFSLinkedService",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

**Azure Storage länkade tjänsten:**

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

**HDFS indatauppsättning:** den här datamängden refererar toohello HDFS mappen DataTransfer/UnitTest /. hello pipeline kopierar alla hello-filer i den här mappen toohello målet.

Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

**Azure Blob utdatauppsättningen:**

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1). hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas. hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.

```JSON
{
    "name": "OutputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kopieringsaktiviteten i en pipeline med filsystemet källa och mottagare för Blob:**

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse dessa indata och utdata-datauppsättningar och schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**FileSystemSource** och **sink** typ har angetts för**BlobSink**. hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.

```JSON
{
    "name": "pipeline",
    "properties":
    {
        "activities":
        [
            {
                "name": "HdfsToBlobCopy",
                "inputs": [ {"name": "InputDataset"} ],
                "outputs": [ {"name": "OutputDataset"} ],
                "type": "Copy",
                "typeProperties":
                {
                    "source":
                    {
                        "type": "FileSystemSource"
                    },
                    "sink":
                    {
                        "type": "BlobSink"
                    }
                },
                "policy":
                {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>Använda Kerberos-autentisering för HDFS-anslutningen
Det finns två alternativ tooset in hello lokala miljö så toouse Kerberos-autentisering i HDFS-koppling. Du kan välja hello en bättre passar ditt ärende.
* Alternativ 1: [anslutning till gateway-datorn i Kerberos-sfär](#kerberos-join-realm)
* Alternativ 2: [aktivera ömsesidigt förtroende mellan Windows-domän och Kerberos-sfär](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>Alternativ 1: Ansluta till gateway-datorn i Kerberos-sfär

#### <a name="requirement"></a>Krav:

* hello gateway-datorn måste toojoin hello Kerberos-sfär och kan inte ansluta till en Windows-domän.

#### <a name="how-tooconfigure"></a>Hur tooconfigure:

**På gateway-datorn:**

1.  Kör hello **Ksetup** verktyget tooconfigure hello Kerberos KDC-server och sfären.

    hello datorn måste konfigureras som en medlem i en arbetsgrupp, eftersom en Kerberos-sfär skiljer sig från en Windows-domän. Detta kan uppnås genom att ange hello Kerberos-sfär och lägga till en KDC-server på följande sätt. Ersätt *REALM.COM* med din egen respektive sfär efter behov.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    **Starta om** hello datorn efter körning kommandona 2.

2.  Verifiera konfigurationen av hello med **Ksetup** kommando. hello utdata ska vara som:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**I Azure Data Factory:**

* Konfigurera hello HDFS en koppling med hjälp av **Windows-autentisering** tillsammans med Kerberos-huvudnamn namn och lösenord tooconnect toohello HDFS datakällan. Kontrollera [HDFS länkade tjänstegenskaper](#linked-service-properties) avsnittet konfigurationsinformation.

### <a name="kerberos-mutual-trust"></a>Alternativ 2: Aktivera ömsesidigt förtroende mellan Windows-domän och Kerberos-sfär

#### <a name="requirement"></a>Krav:
*   hello gateway-datorn måste ansluta till en Windows-domän.
*   Du behöver behörighet tooupdate hello domänkontrollantens inställningar.

#### <a name="how-tooconfigure"></a>Hur tooconfigure:

> [!NOTE]
> Ersätt REALM.COM och AD.COM i hello följande självstudier med din egen respektive domän och domänkontrollanten efter behov.

**På KDC-server:**

1.  Redigera hello KDC konfigurationen i **krb5.conf** filen toolet KDC förtroende hänvisar toohello följande konfigurationsmallen Windows-domän. Som standard hello konfiguration finns på **/etc/krb5.conf**.

            [logging]
             default = FILE:/var/log/krb5libs.log
             kdc = FILE:/var/log/krb5kdc.log
             admin_server = FILE:/var/log/kadmind.log

            [libdefaults]
             default_realm = REALM.COM
             dns_lookup_realm = false
             dns_lookup_kdc = false
             ticket_lifetime = 24h
             renew_lifetime = 7d
             forwardable = true

            [realms]
             REALM.COM = {
              kdc = node.REALM.COM
              admin_server = node.REALM.COM
             }
            AD.COM = {
             kdc = windc.ad.com
             admin_server = windc.ad.com
            }

            [domain_realm]
             .REALM.COM = REALM.COM
             REALM.COM = REALM.COM
             .ad.com = AD.COM
             ad.com = AD.COM

            [capaths]
             AD.COM = {
              REALM.COM = .
             }

  **Starta om** hello KDC-tjänsten efter konfigurationen.

2.  Förbereda en huvudansvarig med namnet  **krbtgt/REALM.COM@AD.COM**  i KDC-server med hello följande kommando:

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  I **hadoop.security.auth_to_local** HDFS tjänstkonfiguration lägger du till `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.

**På domänkontrollanten:**

1.  Kör följande hello **Ksetup** tooadd en sfär post-kommandon:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Upprätta förtroende från Windows-domän tooKerberos sfär. [lösenord] är hello lösenord för hello principal  **krbtgt/REALM.COM@AD.COM** .

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Välj krypteringsalgoritm som används i Kerberos.

    1. Gå tooServer Manager > hantering av Grupprincip > domän > grupprincipobjekt > standard eller aktiva domänprincip och redigera.

    2. I hello **Redigeraren för Grupprinciphantering** popup-fönster, gå tooComputer Configuration > Principer > Windows-inställningar > säkerhetsinställningar > lokala principer > säkerhetsalternativ, och konfigurera **nätverk säkerhet: Konfigurera krypteringstyper som tillåts för Kerberos**.

    3. Välj hello krypteringsalgoritm som du vill toouse när ansluter tooKDC. Ofta, kan du bara välja alla hello-alternativ.

        ![Config krypteringstyper för Kerberos](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. Använd **Ksetup** kommandot toospecify hello kryptering algoritmen toobe används på hello specifika SFÄR.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Skapa hello mappning mellan hello domänkonto och Kerberos-huvudnamn i ordning toouse Kerberos-huvudnamn i Windows-domän.

    1. Starta hello Administrationsverktyg > **Active Directory-användare och datorer**.

    2. Konfigurera avancerade funktioner genom att klicka på **visa** > **avancerade funktioner**.

    3. Leta upp hello konto toowhich du vill använda toocreate mappningar och högerklicka på tooview **namnmappningar** > klickar du på **Kerberos-namn** fliken.

    4. Lägga till en huvudansvarig från hello sfär.

        ![Mappa säkerhetsidentitet](media/data-factory-hdfs-connector/map-security-identity.png)

**På gateway-datorn:**

* Kör följande hello **Ksetup** tooadd en sfär post-kommandon.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**I Azure Data Factory:**

* Konfigurera hello HDFS en koppling med hjälp av **Windows-autentisering** tillsammans med din domänkonto eller Kerberos-huvudnamn tooconnect toohello HDFS-datakälla. Kontrollera [HDFS länkade tjänstegenskaper](#linked-service-properties) avsnittet konfigurationsinformation.

> [!NOTE]
> toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).


## <a name="performance-and-tuning"></a>Prestanda och finjustering
Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.
