---
title: "aaaMove data med hjälp av Kopieringsaktiviteten | Microsoft Docs"
description: "Lär dig mer om flytt av data i Data Factory pipelines: migrering av data mellan moln butiker och mellan ett lokalt Arkiv och en molnarkivet. Använd Kopieringsaktiviteten."
keywords: "Kopiera data, dataflyttning, datamigrering, överföra data"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 67543a20-b7d5-4d19-8b5e-af4c1fd7bc75
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: 29b74154b9006795ead3b0ee9638a3dbf2c5d831
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-by-using-copy-activity"></a>Flytta data med hjälp av Kopieringsaktiviteten
## <a name="overview"></a>Översikt
Du kan använda Kopieringsaktiviteten toocopy data mellan lokala och moln i Azure Data Factory-datalager. När hello data kopieras, kan ytterligare omvandlas och analyseras. Du kan också använda Kopieringsaktiviteten toopublish omvandling och analysresultat för business intelligence (BI) och förbrukning av programmet.

![Rollen för Kopieringsaktivitet](media/data-factory-data-movement-activities/copy-activity.png)

Kopieringsaktiviteten drivs av ett säkert, tillförlitligt, skalbara och och [globalt tillgänglig service](#global). Den här artikeln innehåller information om flytt av data i Data Factory och Kopieringsaktivitet.

Först ska vi se hur migrering av data sker mellan två molnet datalager och mellan ett lokalt datalager och ett datalager för molnet.

> [!NOTE]
> i allmänhet finns i toolearn om aktiviteter [förstå pipelines och aktiviteter](data-factory-create-pipelines.md).
>
>

### <a name="copy-data-between-two-cloud-data-stores"></a>Kopiera data mellan två moln-datalager
När både käll- och mottagarnoderna datalager hello molnet, genomgår Kopieringsaktiviteten hello följande steg toocopy data från hello källa toohello mottagare. hello-tjänst som används för Kopieringsaktiviteten:

1. Läser data från datalagret för hello källa.
2. Utför serialisering/deserialisering, komprimering/dekomprimering, kolumn mappning och Skriv konvertering. Den gör dessa åtgärder baserat på hello konfigurationer av hello inkommande dataset, utdatauppsättningen och Kopieringsaktivitet.
3. Skriver data datalager toohello mål.

hello tjänsten väljer automatiskt hello optimala region tooperform hello dataflyttning. Den här regionen är vanligtvis hello en närmaste toohello sink datalagret.

![Moln-to-cloud-kopia](./media/data-factory-data-movement-activities/cloud-to-cloud.png)

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Kopiera data mellan ett lokalt datalager och ett datalager för molnet
toosecurely flytta data mellan ett lokalt datalager och ett moln datalager installera Data Management Gateway på din lokala dator. Data Management Gateway är en agent som möjliggör hybrid dataförflyttning och bearbetning. Du kan installera den på samma dator som hello datalager själva hello eller på en separat dator som har åtkomst till toohello data lagras.

Data Management Gateway i det här scenariot utför hello serialisering/deserialisering, komprimering/dekomprimering, kolumn mappning och Skriv konvertering. Data flödar inte via hello Azure Data Factory-tjänsten. I stället skriver Data Management Gateway direkt hello datalagret toohello mål.

![Kopiera på lokal-till-moln](./media/data-factory-data-movement-activities/onprem-to-cloud.png)

Se [flytta data mellan lokalt och i molnet datalager](data-factory-move-data-between-onprem-and-cloud.md) för en introduktion och genomgången. Se [Data Management Gateway](data-factory-data-management-gateway.md) detaljerad information om den här agenten.

Du kan också flytta data från / toosupported datalager som finns på Azure IaaS-virtuella maskiner (VMs) med hjälp av Data Management Gateway. I det här fallet kan du installera Data Management Gateway på hello samma virtuella dator som hello datalager själva, eller på en separat virtuell dator som har åtkomst till toohello data lagras.

## <a name="supported-data-stores-and-formats"></a>Lagrar data som stöds och format
Kopieringsaktiviteten i Data Factory kopierar data från en källa data store tooa sink datalagret. Data Factory stöder hello följande datalager. Data från en källa kan skrivas tooany mottagare. Klicka på en data store toolearn hur toocopy data tooand från detta Arkiv.

> [!NOTE] 
> Om du behöver toomove data till/från ett dataarkiv som Kopieringsaktiviteten inte stöder använda en **anpassad aktivitet** i Data Factory med din egen logik för att kopiera eller flytta data. Mer information om att skapa och använda en anpassad aktivitet finns i [Use custom activities in an Azure Data Factory pipeline (Använda anpassade aktiviteter i en Azure Data Factory-pipeline)](data-factory-use-custom-activities.md).

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Data som lagras med * kan vara lokalt eller på Azure IaaS och kräver tooinstall [Data Management Gateway](data-factory-data-management-gateway.md) på en på-lokal-/ Azure IaaS-dator.

### <a name="supported-file-formats"></a>Filformat som stöds
Du kan använda Kopieringsaktiviteten för**kopiera filer som-är** mellan två filbaserade datalager, kan du hoppa över hello [formatera avsnitt](data-factory-create-datasets.md) i både hello indata och utdata dataset definitioner. hello data kopieras effektivt utan någon serialisering/deserialisering.

Kopieringsaktiviteten också läser från och skriver toofiles i angivna format: **Text, JSON, Avro, ORC och parkettgolv**, och komprimerings-codec **GZip, Deflate, BZip2 och ZipDeflate** stöds. Se [stöds format och komprimering](data-factory-supported-file-and-compression-formats.md) med information.

Du kan till exempel göra hello följande kopiera aktiviteter:

* Kopiera data i lokala SQL Server- och skrivaktiviteter tooAzure Data Lake Store i ORC-format.
* Kopiera filerna i textformat (CSV) från lokala filsystemet och skriva tooAzure Blob i Avro-formatet.
* Kopiera komprimerade filer från lokala filsystemet och expandera sedan mark tooAzure Data Lake Store.
* Kopiera data i GZip komprimerade (CSV)-format från Azure Blob- och skrivaktiviteter tooAzure SQL-databas.

## <a name="global"></a>Globalt tillgänglig dataflyttning
Azure Data Factory är endast tillgänglig i hello västra USA, östra USA och Norra Europa regioner. Hello-tjänst som används för Kopieringsaktiviteten är dock tillgängligt globalt i hello följande regioner och geografiska områden. hello globalt tillgänglig topologi garanterar effektiv dataflyttning som vanligtvis undviker mellan region hopp. Se [tjänster efter region](https://azure.microsoft.com/regions/#services) tillgänglighet för Data Factory och flytt av Data i en region.

### <a name="copy-data-between-cloud-data-stores"></a>Kopiera data mellan moln datalager
När både käll- och mottagarnoderna datalager har hello molnet kan Data Factory använder en tjänstdistribution i hello region som är närmast toohello sink i hello samma geografiska toomove hello data. Se toohello i den följande tabellen för mappning:

| Geografisk plats av datalager för hello mål | Region hello datalager som mål | Region som används för dataflyttning |
|:--- |:--- |:--- |
| USA | Östra USA | Östra USA |
| &nbsp; | Östra USA 2 | Östra USA 2 |
| &nbsp; | Centrala USA | Centrala USA |
| &nbsp; | Norra centrala USA | Norra centrala USA |
| &nbsp; | Södra centrala USA | Södra centrala USA |
| &nbsp; | Västra centrala USA | Västra centrala USA |
| &nbsp; | Västra USA | Västra USA |
| &nbsp; | Västra USA 2 | Västra USA |
| Kanada | Östra Kanada | Centrala Kanada |
| &nbsp; | Centrala Kanada | Centrala Kanada |
| Brasilien | Södra Brasilien | Södra Brasilien |
| Europa | Norra Europa | Norra Europa |
| &nbsp; | Västra Europa | Västra Europa |
| Storbritannien | Storbritannien, västra | Storbritannien, södra |
| &nbsp; | Storbritannien, södra | Storbritannien, södra |
| Asien och stillahavsområdet | Sydostasien | Sydostasien |
| &nbsp; | Östasien | Sydostasien |
| Australien | Östra Australien | Östra Australien |
| &nbsp; | Sydöstra Australien | Sydöstra Australien |
| Japan | Östra Japan | Östra Japan |
| &nbsp; | Västra Japan | Östra Japan |
| Indien | Indien, centrala | Indien, centrala |
| &nbsp; | Indien, västra | Indien, centrala |
| &nbsp; | Södra Indien | Indien, centrala |

Alternativt kan du uttryckligen ange hello regionens Data Factory-tjänsten toobe används tooperform hello kopia genom att ange `executionLocation` egenskap under Kopieringsaktiviteten `typeProperties`. Värden som stöds för den här egenskapen är ovan i **Region används för dataflyttning** kolumn. Observera att dina data passerar den regionen under hello överföring vid kopiering. Till exempel toocopy mellan Azure lagrar i Sydkorea, kan du ange `"executionLocation": "Japan East"` tooroute via Japan region (se [exempel JSON](#by-using-json-scripts) som referens).

> [!NOTE]
> Om hello regionen för hello mål-datalagret är inte i föregående listan eller oidentifierbart, som standard Kopieringsaktiviteten misslyckas i stället för att gå via en annan region om `executionLocation` har angetts. hello stöds regionlistan ska expanderas över tid.
>

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Kopiera data mellan ett lokalt datalager och ett datalager för molnet
När data kopieras mellan lokalt (eller Azure virtuella datorer/IaaS) och molntjänster butiker [Data Management Gateway](data-factory-data-management-gateway.md) utför dataflytt på en lokal dator eller virtuell dator. hello data flödar inte via hello-tjänsten i hello molnet om du inte använder hello [mellanlagrad kopiera](data-factory-copy-activity-performance.md#staged-copy) kapaciteten. I det här fallet flödar data genom hello mellanlagring Azure Blob storage innan de skrivs till hello sink-datalagret.

## <a name="create-a-pipeline-with-copy-activity"></a>Skapa en pipeline med Kopieringsaktiviteten
Du kan skapa en pipeline med Kopieringsaktiviteten på ett par olika sätt:

### <a name="by-using-hello-copy-wizard"></a>Hej med hjälp av guiden Kopiera
hello Data Factory kopiera guiden hjälper dig att toocreate en pipeline med Kopieringsaktiviteten. Denna pipeline kan du toocopy data från källor som stöds toodestinations *utan att behöva skriva JSON* definitioner för länkade tjänster, datauppsättningar och rörledningar. Se [Data Factory kopiera guiden](data-factory-copy-wizard.md) för ytterligare information om hello guiden.  

### <a name="by-using-json-scripts"></a>Med hjälp av JSON-skript
Du kan använda Data Factory-redigeraren i hello Azure-portalen, Visual Studio eller Azure PowerShell toocreate JSON-definitionen för en pipeline (med hjälp av Kopieringsaktiviteten). Sedan kan du distribuera den toocreate hello pipeline i Data Factory. Se [Självstudier: Använd Kopieringsaktivitet i ett Azure Data Factory-pipelinen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) en självstudiekurs med stegvisa instruktioner.    

JSON-egenskaper (till exempel namn, beskrivning, ingående och utgående tabeller och principer) är tillgängliga för alla typer av aktiviteter. Egenskaper som är tillgängliga i hello `typeProperties` avsnittet hello aktivitet varierar med varje aktivitetstyp.

Kopieringsaktiviteten, hello `typeProperties` avsnittet varierar beroende på hello typer av datakällor och egenskaperna. Klicka på källor/mottagare i hello [källor och sänkor stöds](#supported-data-stores-and-formats) toolearn i avsnittet om egenskaper som Kopieringsaktiviteten stöder för att lagra data.

Här är ett exempel JSON-definitionen:

```json
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from Azure blob tooAzure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputBlobTable"
          }
        ],
        "outputs": [
          {
            "name": "OutputSQLTable"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
          },
          "executionLocation": "Japan East"          
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
}
```
hello-schema som har definierats i hello utdata dataset avgör när hello aktiviteten körs (till exempel: **dagliga**, frekvens som **dag**, och intervall som **1**). hello aktivitet kopierar data från en inkommande datauppsättning (**källa**) tooan utdatauppsättningen (**sink**).

Du kan ange fler än en inkommande dataset tooCopy aktivitet. De är används tooverify hello beroenden innan hello aktivitet körs. Dock kan endast hello data från hello första datauppsättningen kopierade toohello måldatamängden. Mer information finns i [schemaläggning och körning](data-factory-scheduling-and-execution.md).  

## <a name="performance-and-tuning"></a>Prestanda- och justering
Se hello [prestandajustering guide och Kopieringsaktivitet prestanda](data-factory-copy-activity-performance.md), som beskriver viktiga faktorer som påverkar hello prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory. Den visar hello observerade prestanda under interna tester och beskrivs olika sätt toooptimize hello prestanda för Kopieringsaktivitet.

## <a name="fault-tolerance"></a>Feltolerans
Som standard kopieringsaktiviteten stoppas kopiera data och returnera fel uppstår när inkompatibla data mellan käll- och mottagarnoderna; medan du explicit kan konfigurera tooskip och logg hello inkompatibla rader och endast kopiera dessa kompatibel toomake hello Datakopieringen är klar. Se hello [Kopieringsaktiviteten feltolerans](data-factory-copy-activity-fault-tolerance.md) för mer information.

## <a name="security-considerations"></a>Säkerhetsöverväganden
Se hello [säkerhetsaspekter](data-factory-data-movement-security-considerations.md), som beskriver säkerhetsinfrastruktur att data movement tjänster i Azure Data Factory använder toosecure dina data.

## <a name="scheduling-and-sequential-copy"></a>Schemaläggning och sekventiella kopia
Se [schemaläggning och körning](data-factory-scheduling-and-execution.md) detaljerad information om hur schemaläggning och körning fungerar i Data Factory. Det är möjligt toorun flera kopieringsåtgärder efter varandra på ett sätt som sekventiella/sorterade. Se hello [kopiera sekventiellt](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) avsnitt.

## <a name="type-conversions"></a>Typkonverteringar
Olika datalager har en inbyggd typ i olika system. Kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande två sätt:

1. Konvertera från inbyggda typer tooa .NET källtyp.
2. Konvertera från typen .NET typen tooa interna mottagare.

hello-mappning från en inbyggd typ system tooa .NET-typ för ett datalager är i hello respektive data store artikeln. (Klicka på hello specifika länk i hello [stöds datalager](#supported-data-stores) tabell). Du kan använda dessa mappningar toodetermine lämpliga typer när du skapar tabeller, så att Kopieringsaktiviteten utför hello rätt konverteringar.

## <a name="next-steps"></a>Nästa steg
* toolearn om hello Kopieringsaktiviteten mer, se [kopiera data från Azure Blob storage tooAzure SQL-databas](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* toolearn om att flytta data från ett lokalt data store tooa moln datalager, se [flytta data från datalager för lokal toocloud](data-factory-move-data-between-onprem-and-cloud.md).
