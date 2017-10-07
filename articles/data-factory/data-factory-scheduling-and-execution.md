---
title: "aaaScheduling och körning med Data Factory | Microsoft Docs"
description: "Lär dig schemaläggning och körning av aspekter av Azure Data Factory programmodell."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 088a83df-4d1b-4ac1-afb3-0787a9bd1ca5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 6114dd4896f5537c789c3b632fb90e501b694285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-factory-scheduling-and-execution"></a>Data Factory schemaläggning och körning
Den här artikeln förklarar hello schemaläggning och körning av aspekter av hello Azure Data Factory programmodell. Den här artikeln förutsätter att du förstår grunderna för Data Factory programmet modellen begrepp, inklusive aktivitet, rörledningar, länkade tjänster och datauppsättningar. Grundläggande begrepp för Azure Data Factory finns hello följande artiklar:

* [Introduktion tooData fabriken](data-factory-introduction.md)
* [Pipelines](data-factory-create-pipelines.md)
* [Datauppsättningar](data-factory-create-datasets.md) 

## <a name="start-and-end-times-of-pipeline"></a>Start- och sluttider för pipeline
En pipeline är aktiv endast mellan dess **starta** tid och **end** tid. Det utförs inte före starttiden för hello eller efter hello sluttid. Om hello pipeline är pausad körs den inte oavsett dess start- och tid. För en pipeline-toorun bör det inte pausas. Du hittar dessa inställningar (start, slut, pausats) i hello pipeline definition: 

```json
"start": "2017-04-01T08:00:00Z",
"end": "2017-04-01T11:00:00Z"
"isPaused": false
```

Mer information finns i de här egenskaperna [skapa pipelines](data-factory-create-pipelines.md) artikel. 


## <a name="specify-schedule-for-an-activity"></a>Ange schemat för en aktivitet
Det är inte hello pipeline som körs. Det är hello aktiviteter i hello pipeline som utförs i hello övergripande kontext för hello pipeline. Du kan ange ett återkommande schema för en aktivitet med hello **scheduler** delen av aktivitets-JSON. T.ex, kan du schemalägga en aktivitet toorun varje timme på följande sätt:  

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},
```

Som visas i följande diagram hello, ange ett schema för en aktivitet skapas en serie rullande fönster med i hello pipeline start- och sluttider. Rullande fönster är en serie med fast storlek inte överlappar, sammanhängande intervall. Dessa logiska rullande fönster för en aktivitet kallas **aktivitet windows**.

![Aktiviteten scheduler-exempel](media/data-factory-scheduling-and-execution/scheduler-example.png)

Hej **scheduler** -egenskapen för en aktivitet är valfria. Om du anger den här egenskapen, måste den matcha hello takt som du anger i hello definitionen för utdatauppsättningen för hello-aktivitet. Datamängd för utdata är för närvarande vilka enheter hello schema. Därför måste du skapa en datamängd för utdata, även om hello aktiviteten inte producerar några utdata. 

## <a name="specify-schedule-for-a-dataset"></a>Ange schemat för en datamängd
En aktivitet i en Data Factory-pipelinen har noll eller fler indata **datauppsättningar** och skapa en eller flera utdata-datauppsättningar. Du kan ange hello takt i vilken hello indata är tillgänglig för en aktivitet eller hello-utdata skapas med hjälp av hello **tillgänglighet** avsnitt i hello dataset definitioner. 

**Frekvensen** i hello **tillgänglighet** avsnittet anger hello tidsenhet. hello tillåtna värden för frekvens är: minut, timma, dag, vecka och månad. Hej **intervall** egenskap hello tillgänglighet under anger en multiplikator för frekvens. Exempel: om hello frekvens anges tooDay och intervallet anges too1 för en datamängd för utdata, hello-utdata skapas varje dag. Om du anger hello frekvens som minut, rekommenderar vi att du ställer in hello intervall toono som är mindre än 15. 

I följande exempel hello, hello indata finns varje timme och hello-utdata skapas varje timme (`"frequency": "Hour", "interval": 1`). 

**Indatauppsättning:** 

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
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


**Datamängd för utdata**

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
            "format": {
                "type": "TextFormat"
            },
            "partitionedBy": [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" }}
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

För närvarande **utdata dataset enheter hello schema**. Med andra ord är hello schemat för utdatauppsättningen hello används toorun en aktivitet vid körning. Därför måste du skapa en datamängd för utdata, även om hello aktiviteten inte producerar några utdata. Om hello aktiviteten inte tar några indata, kan du hoppa över skapar hello inkommande dataset. 

I följande hello pipeline-definition, hello **scheduler** egenskapen är används toospecify schema för hello aktiviteten. Den här egenskapen är valfri. För närvarande måste hello schema för hello aktiviteten matcha hello schemat för hello datamängd för utdata.
 
```json
{
    "name": "SamplePipeline",
    "properties": {
        "description": "copy activity",
        "activities": [
            {
                "type": "Copy",
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 100000,
                        "writeBatchTimeout": "00:05:00"
                    }
                },
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
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        ],
        "start": "2017-04-01T08:00:00Z",
        "end": "2017-04-01T11:00:00Z"
    }
}
```

I det här exemplet hello aktiviteten körs varje timme mellan hello start och sluttider för hello pipeline. hello-utdata skapas varje timme för tre timmars windows (8: 00 – 9 AM, 9: 00 – 10: 00, och 10 AM - 11: 00). 

Varje dataenhet förbrukas eller produceras av en aktivitet som körs kallas en **datasektorn**. hello följande diagram visar ett exempel på en aktivitet med en inkommande datauppsättning och en datamängd för utdata: 

![Tillgänglighet Schemaläggaren](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

hello diagram visar hello varje timme datasektorer för hello indata och utdata dataset. hello diagram visar tre inkommande segment som är klara för bearbetning. hello 10 11 AM aktivitet pågår, producerar hello 10 11 AM utdata sektorn. 

Du kan komma åt hello tidsintervall som är kopplat till hello aktuellt segment i hello dataset JSON genom att använda variabler: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) och [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables). På liknande sätt kan du komma åt hello tidsintervall som är associerad med ett fönster med en aktivitet med hjälp av hello WindowStart och WindowEnd. hello schemat för en aktivitet måste matcha hello schemat för hello utdatauppsättningen för hello-aktivitet. Därför hello SliceStart och SliceEnd värden är hello samma som WindowStart och WindowEnd värden respektive. Mer information om dessa variabler finns [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md#data-factory-system-variables) artiklar.  

Du kan använda dessa variabler för olika ändamål i din aktivitets-JSON. T.ex, du kan använda dem tooselect data från inkommande och utgående datauppsättningar som representerar tid series-data (till exempel: 8 AM too9 är). Det här exemplet använder också **WindowStart** och **WindowEnd** tooselect relevanta data för en aktivitet körs och kopiera den tooa blob med lämpliga hello **folderPath**. Hej **folderPath** är parametriserade toohave en separat mapp för varje timme.  

I föregående exempel hello, hello schemat för inkommande och utgående datauppsättningar för hello samma (varje timme). Om hello inkommande datamängden för hello aktivitet är tillgänglig på en annan frekvens Säg var 15: e minut hello aktiviteten som producerar datauppsättningen utdata fortfarande körs en gång i timmen som hello utdatauppsättningen vilka enheter hello aktivitetsschema. Mer information finns i [modell datauppsättningar med olika frekvenser](#model-datasets-with-different-frequencies).

## <a name="dataset-availability-and-policies"></a>DataSet tillgänglighet och principer
Du har sett hello användning av frekvensen och intervall egenskaper under hello tillgänglighet i datauppsättningsdefinitionen. Det finns några andra egenskaper som påverkar hello schemaläggning och körning av en aktivitet. 

### <a name="dataset-availability"></a>DataSet-tillgänglighet 
hello följande tabell beskriver egenskaper som kan användas i hello **tillgänglighet** avsnitt:

| Egenskap | Beskrivning | Krävs | Standard |
| --- | --- | --- | --- |
| frequency |Anger hello tidsenhet för dataset sektorn produktion.<br/><br/><b>Stöd för frekvens</b>: minut, timma, dag, vecka, månad |Ja |Ej tillämpligt |
| interval |Anger en multiplikator för frekvens<br/><br/>”X frekvensintervall” avgör hur ofta hello segment skapas.<br/><br/>Om du behöver hello dataset toobe segmenterat timme, ange <b>frekvens</b> för<b>timme</b>, och <b>intervall</b> för<b>1</b>.<br/><br/><b>Obs</b>: Om du anger frekvens som minut, rekommenderar vi att du ställer in hello intervall toono som är mindre än 15 |Ja |Ej tillämpligt |
| format |Anger om hello segment ska produceras hello start/slutet av hello intervall.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Om frekvensen anges tooMonth och format anges tooEndOfInterval, tillverkas hello segment på hello sista dagen i månaden. Om hello format anges tooStartOfInterval, produceras hello segment på hello första dagen i månaden.<br/><br/>Om frekvensen anges tooDay och format anges tooEndOfInterval, tillverkas hello segment i hello senaste timmen på dagen hello.<br/><br/>Om frekvensen anges tooHour och format anges tooEndOfInterval, tillverkas hello sektorn hello slutet av hello timme. För ett segment för PM 1 – 2 PM period, till exempel produceras hello sektorn klockan 2. |Nej |EndOfInterval |
| anchorDateTime |Definierar hello absolut placering i tid som används av Schemaläggaren toocompute dataset sektorn gränser. <br/><br/><b>Obs</b>: om hello AnchorDateTime har datumdelar som är mer detaljerad än hello frekvens sedan hello mer detaljerade delar ignoreras. <br/><br/>Till exempel, om hello <b>intervall</b> är <b>varje timme</b> (frekvens: timme och intervall: 1) och hello <b>AnchorDateTime</b> innehåller <b>minuter och sekunder</b>, sedan hello <b>minuter och sekunder</b> delar av hello AnchorDateTime ignoreras. |Nej |01/01/0001 |
| förskjutning |TimeSpan genom vilka hello början och slutet av alla dataset segment flyttat. <br/><br/><b>Obs</b>: om både anchorDateTime och offset anges hello resultatet är hello kombineras SKIFT. |Nej |Ej tillämpligt |

### <a name="offset-example"></a>Offset exempel
Som standard varje dag (`"frequency": "Day", "interval": 1`) segment som börjar vid 12: 00 UTC-tid (midnatt). Om du vill hello start tid toobe 06: 00 UTC-tid i stället anger du hello förskjutning som visas i följande fragment hello: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>anchorDateTime-exempel
I följande exempel hello, produceras hello dataset var 23: e timme. hello första sektorn startar vid hello tid som anges av hello anchorDateTime som har angetts för`2017-04-19T08:00:00` (UTC-tid).

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a>förskjutning/stil exempel
hello följande dataset månatliga datauppsättning och skapas på 3: e varje månad kl 8:00 (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

### <a name="dataset-policy"></a>DataSet-princip
En dataset kan ha en verifieringsprincip har definierats som anger hur hello data som genereras av en sektor körning kan valideras innan den är klar för användning. I sådana fall när hello sektorn är klar körning och hello utdata sektorn status ändras för**väntar på** med en substatus av **validering**. Efter hello segment validerats, hello sektorn status ändras för**klar**. Om en datasektorn har producerats men validerades inte hello, bearbetas inte aktiviteten körs för underordnade segment som är beroende av det här segmentet. [Övervaka och hantera pipelines](data-factory-monitor-manage-pipelines.md) omfattar hello olika lägen för datasektorer i Data Factory.

Hej **princip** avsnitt i datauppsättningsdefinitionen definierar hello kriterier eller hello villkor som hello dataset segment måste vara uppfyllda. hello följande tabell beskriver egenskaper som kan användas i hello **princip** avsnitt:

| Principnamn | Beskrivning | Används för| Krävs | Standard |
| --- | --- | --- | --- | --- |
| minimumSizeMB | Verifierar att hello-data i en **Azure blob** uppfyller hello krav på minsta storlek (i megabyte). |Azure-blobb |Nej |Ej tillämpligt |
| minimumRows | Verifierar att hello-data i en **Azure SQL database** eller en **Azure-tabellen** innehåller hello minsta antal rader. |<ul><li>Azure SQL Database</li><li>Azure-tabellen</li></ul> |Nej |Ej tillämpligt |

#### <a name="examples"></a>Exempel
**minimumSizeMB:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

**minimumRows**

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

Mer information om dessa egenskaper och exempel finns [skapa datauppsättningar](data-factory-create-datasets.md) artikel. 

## <a name="activity-policies"></a>Principer för användaraktivitet
Principer påverkar hello körning beteendet för en aktivitet, särskilt när hello segment i en tabell som har bearbetats. hello följande tabell innehåller hello information.

| Egenskap | Tillåtna värden | Standardvärde | Beskrivning |
| --- | --- | --- | --- |
| Concurrency |Integer <br/><br/>Värdet för maximalt antal: 10 |1 |Antal samtidiga körningar av hello-aktivitet.<br/><br/>Den avgör hello antalet ParallellAktivitet körningar som kan inträffa på olika segment. Till exempel om en aktivitet måste toogo via snabbare en stor mängd tillgänglig data, med ett större värde för samtidighet hello databearbetning. |
| executionPriorityOrder |NewestFirst<br/><br/>OldestFirst |OldestFirst |Anger hello sorteringen av datasektorer som bearbetas.<br/><br/>Till exempel om du har 2 sektorer (en inträffar klockan 4, och en annan kl) och båda finns väntande körning. Om du ställer in hello executionPriorityOrder toobe NewestFirst bearbetas hello sektorn kl först. På liknande sätt om du ställer in hello executionPriorityORder toobe OldestFIrst bearbetas sedan hello sektorn klockan 4. |
| retry |Integer<br/><br/>Värdet för maximalt antal kan vara 10 |0 |Antalet försök innan hello databearbetning för hello segment har markerats som ett fel. Aktivitetskörningen för en datasektorn försöks in toohello angetts antal nya försök. hello försök görs så snart som möjligt efter hello-fel. |
| timeout |TimeSpan |00:00:00 |Tidsgräns för hello-aktivitet. Exempel: 00:10:00 (inbegriper timeout 10 minuter)<br/><br/>Om ett värde har angetts eller är 0, är hello timeout oändligt.<br/><br/>Om hello bearbetningstid på ett segment överskrider hello timeout-värde, det avbryts och hello system försöker tooretry hello bearbetning. hello antal återförsök beror på hello försök egenskapen. När timeout uppstår status hello tooTimedOut. |
| Fördröjning |TimeSpan |00:00:00 |Ange hello fördröjning innan data bearbetning av hello segment startas.<br/><br/>hello körningen av aktiviteten för en datasektorn startas efter att hello fördröjning har förfallit hello förväntades körningstid.<br/><br/>Exempel: 00:10:00 (inbegriper fördröjning på 10 minuter) |
| longRetry |Integer<br/><br/>Värdet för maximalt antal: 10 |1 |hello antal lång försök försök innan hello sektorn körningen misslyckades.<br/><br/>longRetry försök fördelade av longRetryInterval. Så om du behöver toospecify taget mellan nya försök använda longRetry. Om både försök och longRetry anges varje longRetry försök omförsök Hej max antal försök används och försök igen * longRetry.<br/><br/>Om till exempel har vi hello följande inställningar i hello aktivitetsprincip:<br/>Försök: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Anta att det finns bara ett segment tooexecute (status väntar) och hello aktivitetskörningen misslyckas varje gång. Det skulle inledningsvis 3 körning på varandra följande försök. Efter varje försök skulle hello sektorn status vara försök igen. Efter första 3 försök är över, är hello sektorn status LongRetry.<br/><br/>Efter en timme (det vill säga Longretryinteval's värde), skulle det finnas en annan uppsättning 3 körning på varandra följande försök. Efter detta hello sektorn status skulle misslyckades och inga fler försök kan inte genomföras. Därför har övergripande 6 försök gjorts.<br/><br/>Om alla körning lyckas hello sektorn status är klar och inga fler försök görs.<br/><br/>longRetry får användas i situationer där beroende data kommer till icke-deterministiska gånger eller hello hela miljön är flaky under vilken databearbetningen sker. I sådana fall kan inte göra återförsök efter varandra kan hjälpa och gör det med tiden resulterar i hello tidsintervall önskad utdata.<br/><br/>Liten varning: Ange inte höga värden för longRetry eller longRetryInterval. Normalt en högre värden andra systemfel problem. |
| longRetryInterval |TimeSpan |00:00:00 |hello fördröjning mellan försök har lång |

Mer information finns i [Pipelines](data-factory-create-pipelines.md) artikel. 

## <a name="parallel-processing-of-data-slices"></a>Parallell bearbetning av datasektorer
Du kan ange hello startdatum för hello pipeline i hello tidigare. När du gör det Data Factory beräknas (tillbaka fyllning) alla datasektorer i hello senaste och automatiskt börjar bearbeta dem. Exempel: Om du skapar en pipeline med startdatum 2017-04-01 och hello aktuellt datum är 2017-04-10. Om hello takt av hello utflöde dataset är varje dag och sedan Data Factory startar bearbetning av alla hello segment från 2017-04-01 är too2017-04-09 omedelbart eftersom hello startdatum hello tidigare. hello segment från 2017-04-10 bearbetas inte ännu eftersom hello värdet style-egenskapen i hello tillgänglighetssektion är EndOfInterval som standard. hello äldsta sektorn bearbetas först som hello standard är värdet för executionPriorityOrder OldestFirst. En beskrivning av hello style-egenskapen, se [dataset tillgänglighet](#dataset-availability) avsnitt. En beskrivning av hello executionPriorityOrder avsnittet finns hello [aktivitetsprinciper](#activity-policies) avsnitt. 

Du kan konfigurera fylls i med tillbaka data segment toobe bearbetas parallellt genom att ange hello **samtidighet** egenskap i hello **princip** avsnitt i hello aktivitets-JSON. Den här egenskapen anger hello antalet ParallellAktivitet körningar som kan inträffa på olika segment. hello standardvärdet för hello concurrency-egenskapen är 1. Därför bearbetas en sektor samtidigt som standard. hello maxvärdet är 10. När en pipeline måste toogo via en stor mängd tillgänglig data, med ett större värde för samtidighet snabbare hello databearbetning. 

## <a name="rerun-a-failed-data-slice"></a>Kör en misslyckad datasektorn
När ett fel uppstår under bearbetning av en datasektorn, hittar reda på varför hello bearbetning av ett segment misslyckats med hjälp av Azure portal blad eller övervaka och hantera appen. Se [övervakning och hantering av pipelines med hjälp av Azure portal blad](data-factory-monitor-manage-pipelines.md) eller [övervakning och hantering av](data-factory-monitor-manage-app.md) mer information.

Överväg att hello följande exempel, som innehåller två aktiviteter. Activity1 och aktivitet 2. Activity1 använder ett segment av Dataset1 och genererar ett segment av Dataset2 som används som indata av Activity2 tooproduce ett segment av hello slutliga Dataset.

![Misslyckade sektorn](./media/data-factory-scheduling-and-execution/failed-slice.png)

hello diagram visar att utanför tre senaste segment det uppstod ett fel berörda hello 9 10 AM segment för Dataset2. Data Factory spårar automatiskt beroende för hello tid serien dataset. Därför kan startar den inte hello aktiviteten kör för hello 9 10 AM underordnade sektorn.

Data Factory övervakning och hantering av verktygen kan du toodrill till hello diagnostikloggar för hello misslyckade sektorn tooeasily hitta hello rot orsaka för hello problemet och åtgärda problemet. När du har åtgärdat hello problemet, kan du enkelt starta hello aktiviteten kör tooproduce hello misslyckade sektorn. Mer information om hur toorerun och förstå tillståndsövergångar för datasektorer, se [övervakning och hantering av pipelines med hjälp av Azure portal blad](data-factory-monitor-manage-pipelines.md) eller [övervakning och hantering av](data-factory-monitor-manage-app.md).

När du kör hello 9 10 AM segmentera för **Dataset2**, Data Factory startar hello körs för hello 9 10 AM beroende segment på hello slutliga dataset.

![Kör misslyckade sektorn](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="multiple-activities-in-a-pipeline"></a>Flera aktiviteter i en pipeline
Du kan fler än en aktivitet i en pipeline. Om du har flera aktiviteter i en pipeline och hello utdata för en aktivitet inte är indata för en annan aktivitet, kan hello aktiviteter köras parallellt om indata segment för hello aktiviteter är klar.

Du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. hello aktiviteter kan vara i hello samma pipeline eller i olika pipelines. hello andra aktiviteten körs bara när hello först en slutförs.

Anta till exempel att hello följande om en pipeline har två aktiviteter:

1. Aktiviteten A1 som kräver indata externa datauppsättningen D1 och producerar utdatauppsättningen D2.
2. Aktiviteten A2 som kräver indata från dataset D2 och producerar utdatauppsättningen D3.

I det här scenariot, aktiviteter A1 och A2 är i hello samma pipeline. hello aktivitet A1 körs när hello externa data är tillgängliga och frekvens för hello schemalagd tillgänglighet har uppnåtts. hello aktivitet A2 körs när hello schemalagda segment från D2 bli tillgänglig och hello schemalagd tillgänglighet frekvens har uppnåtts. Om det finns ett fel i en hello segment i dataset D2, körs inte A2 för den sektorn förrän den blir tillgänglig.

Hej diagramvyn med både aktiviteter i hello samma pipeline som ser ut som följande diagram hello:

![Länkning aktiviteter i hello pipeline samma](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

Som tidigare nämnts kan hello aktiviteter vara i olika pipelines. I ett sådant scenario ser hello diagramvyn ut som följande diagram hello:

![Länkning aktiviteter i två rörledningar](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Se hello [kopiera sekventiellt](#copy-sequentially) avsnitt i hello bilagan ett exempel.

## <a name="model-datasets-with-different-frequencies"></a>Modellen datauppsättningar med olika frekvenser
I prover hello, har hello frekvenser för inkommande och utgående datauppsättningar och hello schema aktivitetsfönstret hello samma. Vissa scenarier kräver hello möjlighet tooproduce utdata med en frekvens som skiljer sig hello frekvenser på en eller flera inmatningar. Data Factory stöder modeling dessa scenarier.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Exempel 1: Skapa en daglig utdata-rapport för inkommande data som är tillgängliga i timmen
Föreställ dig ett scenario där du har definierat mätdata från sensorer som är tillgängliga i timmen i Azure Blob storage. Du vill tooproduce en daglig sammanställda rapport med statistik som medelvärde, högsta och lägsta för hello dag med [Data Factory hive aktiviteten](data-factory-hive-activity.md).

Här visas hur du kan utforma det här scenariot med Data Factory:

**Indatauppsättning**

hello ange varje timme filer tas bort i hello mapp för hello angivna dag. Tillgänglighet för indata är inställt på **timme** (frekvens: timme, intervall: 1).

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Datamängd för utdata**

En utdatafil skapas varje dag i hello dag mapp. Tillgängligheten för utdata är inställt på **dag** (frekvens: dag och intervall: 1).

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Aktiviteten: hive aktivitet i en pipeline**

hello hive-skript tar emot hello lämpliga *DateTime* information som parametrar som använder hello **WindowStart** variabeln som visas i följande fragment hello. hello hive-skript använder den här variabeln tooload hello data från hello rätt mapp för hello dag och kör hello aggregering toogenerate hello utdata.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
        {
            "name": "SampleHiveActivity",
            "inputs": [
                {
                    "name": "AzureBlobInput"
                }
            ],
            "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adftutorial\\hivequery.hql",
                "scriptLinkedService": "StorageLinkedService",
                "defines": {
                    "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                    "Month": "$$Text.Format('{0:MM}',WindowStart)",
                    "Day": "$$Text.Format('{0:dd}',WindowStart)"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },            
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 2,
                "timeout": "01:00:00"
            }
         }
     ]
   }
}
```

hello visar följande diagram hello scenariot ur en data-beroendet.

![Beroendet av data](./media/data-factory-scheduling-and-execution/data-dependency.png)

hello utdata sektorn för varje dag beror på 24 timvis segment från en datamängd som indata. Data Factory beräknar beroendena automatiskt genom att räkna ut hello indata-segment som faller inom hello samma tidsperioden som hello utdata sektorn toobe genereras. Om någon av hello 24 inkommande segment inte är tillgänglig väntar Data Factory hello inkommande segmentet toobe redo innan du startar hello daglig aktivitet körs.

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Exempel 2: Ange samband med uttryck och Data Factory-funktioner
Nu ska vi titta ett annat scenario. Anta att du har en hive-aktivitet som bearbetar två inkommande datauppsättningar. En av dem har nya data varje dag, men en av dem hämtar nya data varje vecka. Anta att du vill ha toodo en koppling mellan två hello-indata och skapar ett utgående varje dag.

hello enkel metod som Data Factory automatiskt räknat ut hello rätt indata sektorer tooprocess genom att justera toohello utdata data tidsintervallet tidsperiod inte fungerar.

Du måste ange att för varje aktivitet som kör hello Data Factory ska använda föregående vecka datasektorn för hello veckovisa inkommande dataset. Du kan använda Azure Data Factory-funktioner som visas i följande fragment tooimplement hello problemet.

**Input1: Azure blob**

hello första indata hello Azure blob uppdateras dagligen.

```json
{
  "name": "AzureBlobInputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Input2: Azure blob**

Input2 är hello Azure blob uppdateras varje vecka.

```json
{
  "name": "AzureBlobInputWeekly",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 7
    }
  }
}
```

**Utdata: Azure blob**

En utdatafil skapas varje dag i hello mapp för hello dag. Tillgängligheten för utdata har angetts för**dag** (frekvens: Day, intervall: 1).

```json
{
  "name": "AzureBlobOutputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Aktiviteten: hive aktivitet i en pipeline**

hello hive aktiviteten tar hello två indata och skapar ett utgående segment varje dag. Du kan ange varje dag utdata sektorn toodepend på hello föregående vecka inkommande segment för varje vecka indata på följande sätt.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
      {
        "name": "SampleHiveActivity",
        "inputs": [
          {
            "name": "AzureBlobInputDaily"
          },
          {
            "name": "AzureBlobInputWeekly",
            "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
            "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutputDaily"
          }
        ],
        "linkedServiceName": "HDInsightLinkedService",
        "type": "HDInsightHive",
        "typeProperties": {
          "scriptPath": "adftutorial\\hivequery.hql",
          "scriptLinkedService": "StorageLinkedService",
          "defines": {
            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
            "Month": "$$Text.Format('{0:MM}',WindowStart)",
            "Day": "$$Text.Format('{0:dd}',WindowStart)"
          }
        },
        "scheduler": {
          "frequency": "Day",
          "interval": 1
        },            
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 2,  
          "timeout": "01:00:00"
        }
       }
     ]
   }
}
```

Se [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md) en lista över funktioner och systemvariabler som har stöd för Data Factory.

## <a name="appendix"></a>Bilaga

### <a name="example-copy-sequentially"></a>Exempel: kopiera sekventiellt
Det är möjligt toorun flera kopieringsåtgärder efter varandra på ett sätt som sekventiella/sorterade. Du kan till exempel ha två utdatauppsättningar som kopiera aktiviteter i en pipeline (CopyActivity1 och CopyActivity2) med hello efter inkommande data:   

CopyActivity1

Indata: Dataset. Utdata: Dataset2.

CopyActivity2

Indata: Dataset2.  Utdata: Dataset3.

CopyActivity2 körs bara om hello CopyActivity1 har körts och Dataset2 är tillgänglig.

Här är hello exempel pipeline-JSON:

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob1ToBlob2",
                "description": "Copy data from a blob tooanother"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset3"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob2ToBlob3",
                "description": "Copy data from a blob tooanother"
            }
        ],
        "start": "2016-08-25T01:00:00Z",
        "end": "2016-08-25T01:00:00Z",
        "isPaused": false
    }
}
```

Observera att hello datamängd för utdata för hello första kopieringsaktiviteten (Dataset2) har angetts i hello exempel som indata för andra hello-aktiviteten. Därför körs hello andra aktiviteten bara hello datamängd för utdata från hello första aktiviteten är klar.  

I exemplet hello CopyActivity2 kan ha en annan inmatning, till exempel Dataset3, men du ange Dataset2 som ett inkommande tooCopyActivity2 så hello aktiviteten inte körs förrän CopyActivity1 har slutförts. Exempel:

CopyActivity1

Indata: Dataset1. Utdata: Dataset2.

CopyActivity2

Indata: Dataset3, Dataset2. Utdata: Dataset4.

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlobToBlob",
                "description": "Copy data from a blob tooanother"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset3"
                    },
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset4"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob3ToBlob4",
                "description": "Copy data from a blob tooanother"
            }
        ],
        "start": "2017-04-25T01:00:00Z",
        "end": "2017-04-25T01:00:00Z",
        "isPaused": false
    }
}
```

Observera att två indatauppsättningar har angetts i hello exempelvis för hello andra kopieringsaktiviteten. När flera inmatningar anges endast hello första inkommande dataset används för att kopiera data, men andra datauppsättningar som används som beroenden. CopyActivity2 skulle startas endast efter hello följande villkor är uppfyllda:

* CopyActivity1 har slutförts och Dataset2 är tillgänglig. Den här datauppsättningen används inte när du kopierar data tooDataset4. Det fungerar bara som ett schemaläggning beroende för CopyActivity2.   
* Dataset3 är tillgänglig. Den här datauppsättningen representerar hello data som är mål för kopierade toohello. 
