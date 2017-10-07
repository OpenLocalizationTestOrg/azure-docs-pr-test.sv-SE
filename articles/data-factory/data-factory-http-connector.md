---
title: "aaaMove data från en källa för HTTP - Azure | Microsoft Docs"
description: "Läs mer om hur toomove från en lokal eller ett moln HTTP datakälla med hjälp av Azure Data Factory."
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
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: e39b9cbff870aef4be91938cacff39a2fd12d64a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-http-source-using-azure-data-factory"></a>Flytta data från en HTTP-datakälla med hjälp av Azure Data Factory
Den här artikeln beskrivs hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en på lokalt/i molnet http-slutpunkt tooa stöds sink-datalagret. Den här artikeln bygger på hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med kopiera aktivitet och hello lista över datakällor som stöds som källor/sänkor.

Data factory för närvarande stöder endast flyttning av data från en HTTP datakällan tooother datalager, men inte flytta data från andra data lagrar tooan HTTP-mål.

## <a name="supported-scenarios-and-authentication-types"></a>Scenarier som stöds och typer av autentisering
Du kan använda den här HTTP-anslutningen tooretrieve data från **både till molnet och lokala HTTP/s-slutpunkt** med hjälp av HTTP **hämta** eller **POST** metod. hello följande autentiseringstyper som stöds: **anonym**, **grundläggande**, **sammanfattad**, **Windows**, och  **ClientCertificate**. Observera hello skillnaden mellan den här kopplingen och hello [Web tabell connector](data-factory-web-table-connector.md) är: hello senare är används tooextract tabell innehåll från HTML-sidan.

När du kopierar data från en lokal HTTP-slutpunkt, måste du installera en Data Management Gateway i hello lokala miljö/Azure VM. Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway.

## <a name="getting-started"></a>Komma igång
Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en HTTP-datakälla med hjälp av olika verktyg/API: er.

- hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**. Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.

- Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**. Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet. JSON-exempel toocopy data från http-källa tooAzure Blob Storage, finns [JSON-exempel](#json-examples) avsnitt i den här artikeln.

## <a name="linked-service-properties"></a>Länkad tjänstegenskaper
hello följande tabell innehåller en beskrivning för JSON-element specifika tooHTTP länkad tjänst.

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| typ | hello Typegenskapen måste anges till: `Http`. | Ja |
| URL: en | Bas-URL: en toohello webbserver | Ja |
| AuthenticationType | Anger hello autentiseringstyp. Tillåtna värden är: **anonym**, **grundläggande**, **sammanfattad**, **Windows**, **ClientCertificate**. <br><br> Läs respektive toosections under den här tabellen på fler egenskaper och JSON-exempel för dessa typer av autentisering. | Ja |
| enableServerCertificateValidation | Ange om tooenable server SSL-certifikatverifieringen om datakällan är HTTPS-webbserver | Nej, standard är SANT |
| gatewayName | Namnet på hello Data Management Gateway tooconnect tooan lokalt http-källa. | Ja om du kopierar data från en lokal http-källa. |
| encryptedCredential | Krypterade autentiseringsuppgifter tooaccess hello HTTP-slutpunkten. Genereras automatiskt när du konfigurerar hello autentiseringsinformation i Kopiera guiden eller hello ClickOnce popup-dialogrutan. | Nej. Gäller bara när du kopierar data från en lokal HTTP-server. |

Se [flytta data mellan lokala källor och hello moln med Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) mer information om hur du anger autentiseringsuppgifter för datakälla för lokala HTTP-anslutningen.

### <a name="using-basic-digest-or-windows-authentication"></a>Med hjälp av grundläggande, sammanfattad eller Windows-autentisering

Ange `authenticationType` som `Basic`, `Digest`, eller `Windows`, och ange följande egenskaper utöver hello HTTP-anslutningen generiska de som introducerats ovan hello:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| användarnamn | Användarnamnet tooaccess hello HTTP-slutpunkten. | Ja |
| lösenord | Lösenordet för hello (användarnamn). | Ja |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Exempel: med grundläggande, sammanfattad eller Windows-autentisering

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "basic",
            "url" : "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a>Med hjälp av ClientCertificate autentisering

toouse grundläggande autentisering, ange `authenticationType` som `ClientCertificate`, och ange följande egenskaper utöver hello HTTP-anslutningen generiska de som introducerats ovan hello:

| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| embeddedCertData | hello Base64-kodade innehåll för binära data i hello Personal Information Exchange (PFX)-fil. | Ange antingen hello `embeddedCertData` eller `certThumbprint`. |
| certThumbprint | Hej tumavtrycket för certifikatet för hello som har installerats på gateway-datorns certifikatarkiv. Gäller bara när du kopierar data från en lokal http-källa. | Ange antingen hello `embeddedCertData` eller `certThumbprint`. |
| lösenord | Lösenordet som är associerat med hello certifikat. | Nej |

Om du använder `certThumbprint` för autentisering och hello certifikat installeras i hello personliga arkivet i hello lokala datorn, behöver du toogrant hello läsbehörighet toohello gateway-tjänsten:

1. Starta Microsoft Management Console (MMC). Lägg till hello **certifikat** snapin-modulen som mål hello **lokal dator**.
2. Expandera **certifikat**, **personliga**, och klicka på **certifikat**.
3. Högerklicka på hello certifikatet från datorarkivet hello och välj **alla aktiviteter**->**hantera privata nycklar...**
3. På hello **säkerhet** lägger du till hello-användarkonto som Data Management Gateway-värdtjänsten körs under med hello läsbehörighet toohello certifikat.  

#### <a name="example-using-client-certificate"></a>Exempel: använder klientcertifikat
Det här länkade tjänsten länkar din data factory tooan lokalt HTTP-server. Den använder ett klientcertifikat som är installerad på datorn hello med Data Management Gateway är installerad.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"

        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a>Exempel: använder klientcertifikat i en fil
Det här länkade tjänsten länkar din data factory tooan lokalt HTTP-server. Det använder en klient-certifikatfil på hello datorn med Data Management Gateway är installerad.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

## <a name="dataset-properties"></a>Egenskaper för datamängd
En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel. Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).

Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello. Hej typeProperties avsnittet för dataset av typen **Http** har hello följande egenskaper

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ | Ange hello typ av hello dataset. måste anges för`Http`. | Ja |
| relativeUrl | En relativ URL toohello resurs som innehåller hello data. Om sökvägen inte anges används endast hello-URL som anges i tjänstdefinitionen hello länkad. <br><br> dynamisk tooconstruct-URL som du kan använda [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md), t.ex. ”relativeUrl” ”: $$Text.Format ('/ min/rapporten? månad = {0:yyyy}-{0:MM} & fmt = csv', SliceStart)”. | Nej |
| requestMethod | HTTP-metod. Tillåtna värden är **hämta** eller **efter**. | Nej. Standardvärdet är `GET`. |
| additionalHeaders | Ytterligare HTTP-begärans sidhuvud. | Nej |
| requestBody | Brödtext för HTTP-begäran. | Nej |
| Format | Om du vill toosimply **hämta hello data från HTTP-slutpunkt som-är** hoppa över den här formatinställningar utan parsning den. <br><br> Om du vill tooparse hello HTTP-svar innehåll vid kopiering, hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt. |Nej |
| Komprimering | Ange hello typ och kompression för hello data. Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**. Nivåer som stöds är: **Optimal** och **snabbast**. Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nej |

### <a name="example-using-hello-get-default-method"></a>Exempel: genom att använda metoden för hello GET (standard)

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

### <a name="example-using-hello-post-method"></a>Exempel: använda hello POST-metoden

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
           "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet
En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel. Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.

Egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet på hello andra sidan varierar med varje aktivitetstyp. För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.

För närvarande när hello-källan i en Kopieringsaktivitet är av typen **HttpSource**, hello följande egenskaper stöds.

| Egenskap | Beskrivning | Krävs |
| -------- | ----------- | -------- |
| httpRequestTimeout | Hej timeout (TimeSpan) för hello HTTP-begäran tooget ett svar. Det är hello timeout tooget ett svar inte hello timeout tooread svarsdata. | Nej. Standardvärde: 00:01:40 |

## <a name="supported-file-and-compression-formats"></a>Fil- och komprimering format som stöds
Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikeln för information.

## <a name="json-examples"></a>JSON-exempel
följande exempel hello ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De visar hur toocopy från http-datakällan tooAzure Blob Storage. Dock datan kan kopieras **direkt** från alla källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.

### <a name="example-copy-data-from-http-source-tooazure-blob-storage"></a>Exempel: Kopiera data från http-källa tooAzure Blob Storage
hello Data Factory-lösning för det här exemplet innehåller hello följande Data Factory-enheter:

1. En länkad tjänst av typen [HTTP](#linked-service-properties).
2. En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Indata [dataset](data-factory-create-datasets.md) av typen [Http](#dataset-properties).
4. Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [HttpSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello exemplet kopierar data från en HTTP-källa tooan Azure blob varje timme. hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.

### <a name="http-linked-service"></a>HTTP-länkad tjänst
Det här exemplet använder hello HTTP länkade tjänsten med anonym autentisering. Se [HTTP länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
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

### <a name="http-input-dataset"></a>Inkommande HTTP-datamängd
Ange **externa** för**SANT** informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}

```

### <a name="azure-blob-output-dataset"></a>Utdatauppsättning för Azure-blobb

Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).

```JSON
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

### <a name="pipeline-with-copy-activity"></a>Pipeline med kopieringsaktiviteten

hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme. I hello pipeline JSON-definitionen hello **källa** typ har angetts för**HttpSource** och **sink** typ har angetts för**BlobSink**.

Se [HttpSource](#copy-activity-properties) hello lista över egenskaper som stöds av hello HttpSource.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "HttpSourceToAzureBlob",
        "description": "Copy from an HTTP source tooan Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "HttpSourceDataInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "HttpSource"
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
