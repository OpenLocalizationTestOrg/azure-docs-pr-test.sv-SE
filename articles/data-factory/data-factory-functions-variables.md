---
title: aaaData Factory funktioner och systemvariabler | Microsoft Docs
description: "Visar en lista över Azure Data Factory-funktioner och systemvariabler"
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
services: data-factory
ms.assetid: b6b3c2ae-b0e8-4e28-90d8-daf20421660d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: d2936c2821797947bb37d9775226a6c19c4b8ab9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---functions-and-system-variables"></a>Azure Data Factory - funktioner och systemvariabler
Den här artikeln innehåller information om funktioner och variabler som stöds av Azure Data Factory.

## <a name="data-factory-system-variables"></a>Data Factory systemvariabler
| Variabelnamn | Beskrivning | Objektets Scope | JSON-Scope och användningsområden |
| --- | --- | --- | --- |
| WindowStart |Början av tidsintervall för aktuell aktivitet som kör Windows |Aktivitet |<ol><li>Ange datafrågor val. Se connector artiklar som refereras i hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel.</li> |
| WindowEnd |Slutet av tidsintervall för aktuell aktivitet som kör Windows |Aktivitet |samma som WindowStart. |
| SliceStart |Början av tidsintervall för datasektor som skapas |Aktivitet<br/>DataSet |<ol><li>Ange dynamiska sökvägar och filnamn när du arbetar med [Azure Blob](data-factory-azure-blob-connector.md) och [filsystemet datauppsättningar](data-factory-onprem-file-system-connector.md).</li><li>Ange indata beroenden med data factory-funktioner i aktiviteten indata samling.</li></ol> |
| SliceEnd |Slut på tidsintervall för aktuella datasektorn. |Aktivitet<br/>DataSet |samma som SliceStart. |

> [!NOTE]
> För närvarande datafabriken kräver att hello schemalägga anges i hello aktivitet matchar exakt hello schema som angetts i tillgängligheten för utdatauppsättningen hello. Därför WindowStart, WindowEnd, och SliceStart och SliceEnd mappas alltid toohello samma tid period och ett enda utflöde segment.
> 

### <a name="example-for-using-a-system-variable"></a>Exempel för att använda en systemvariabel
I följande exempel, år, månad, dag och tid för hello **SliceStart** extraheras till olika variabler som används av **folderPath** och **fileName** egenskaper.

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

## <a name="data-factory-functions"></a>Data Factory-funktioner
Du kan använda funktionerna i data factory tillsammans med systemvariabler för hello följande syften:

1. Anger att markeringen datafrågor (se connector artiklar som refereras av hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel.
   
   Hej syntax tooinvoke är en funktion som data factory:  **$$ <function>**  för val av datafrågor och andra egenskaper hello och datauppsättningar.  
2. Ange indata beroenden med data factory-funktioner i aktivitetssamling indata.
   
    $$ behövs inte för att ange indata beroende uttryck.     

I följande exempel, hello **sqlReaderQuery** egenskap i en JSON-fil tilldelas tooa värdet som returneras av hello `Text.Format` funktion. Det här exemplet använder också en systemvariabeln med namnet **WindowStart**, som representerar hello starttiden för hello-aktivitet som kör Windows.

```json
{
    "Type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
}
```

Se [anpassade datum och tid formatsträngar](https://msdn.microsoft.com/library/8kb3ddd4.aspx) avsnitt som beskriver olika formateringsalternativ som du kan använda (till exempel: Visa kontra åååå). 

### <a name="functions"></a>Funktioner
hello följande tabeller visar alla hello funktioner i Azure Data Factory:

| Kategori | Funktionen | Parametrar | Beskrivning |
| --- | --- | --- | --- |
| Tid |AddHours(X,Y) |X: DateTime <br/><br/>Y: int |Lägger till Y timmar toohello angivna tid X. <br/><br/>Exempel:`9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM` |
| Tid |AddMinutes(X,Y) |X: DateTime <br/><br/>Y: int |Lägger till Y minuter tooX.<br/><br/>Exempel:`9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM` |
| Tid |StartOfHour(X) |X: Datetime |Hämtar hello starttid för hello timme som representeras av hello timkomponenten för X. <br/><br/>Exempel:`StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM` |
| Date |AddDays(X,Y) |X: DateTime<br/><br/>Y: int |Lägger till Y dagar tooX. <br/><br/>Exempel: 9/15/2013 12:00:00 PM + 2 dagar = 9/17/2013 12:00:00 PM.<br/><br/>Du kan ta bort dagar för genom att ange Y som ett negativt tal.<br/><br/>Exempel: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`. |
| Date |AddMonths(X,Y) |X: DateTime<br/><br/>Y: int |Lägger till Y månader tooX.<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.<br/><br/>Du kan ta bort månader för genom att ange Y som ett negativt tal.<br/><br/>Exempel: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.|
| Date |AddQuarters(X,Y) |X: DateTime <br/><br/>Y: int |Lägger till Y * tooX för tre månader.<br/><br/>Exempel:`9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM` |
| Date |AddWeeks(X,Y) |X: DateTime<br/><br/>Y: int |Lägger till Y * tooX 7 dagar<br/><br/>Exempel: 9/15/2013 12:00:00 PM + 1 vecka = 9/22/2013 12:00:00 PM<br/><br/>Du kan ta bort veckor för genom att ange Y som ett negativt tal.<br/><br/>Exempel: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`. |
| Date |AddYears(X,Y) |X: DateTime<br/><br/>Y: int |Lägger till Y år tooX.<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 year = 9/15/2014 12:00:00 PM`<br/><br/>Du kan ta bort år för genom att ange Y som ett negativt tal.<br/><br/>Exempel: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`. |
| Date |Day(X) |X: DateTime |Hämtar hello dagkomponenten för X.<br/><br/>Exempel: `Day of 9/15/2013 12:00:00 PM is 9`. |
| Date |DayOfWeek(X) |X: DateTime |Hämtar hello dagen i veckodagskomponenten för X.<br/><br/>Exempel: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`. |
| Date |DayOfYear(X) |X: DateTime |Hämtar hello dag hello år som representeras av hello årskomponenten för X.<br/><br/>Exempel:<br/>`12/1/2015: day 335 of 2015`<br/>`12/31/2015: day 365 of 2015`<br/>`12/31/2016: day 366 of 2016 (Leap Year)` |
| Date |DaysInMonth(X) |X: DateTime |Hämtar hello dagar i hello månad som representeras av hello månadskomponenten för parametern X.<br/><br/>Exempel: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in hello September month`. |
| Date |EndOfDay(X) |X: DateTime |Hämtar hello tid som representerar hello slutet av hello dag (dagkomponenten) för X.<br/><br/>Exempel: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`. |
| Date |EndOfMonth(X) |X: DateTime |Hämtar hello månadsslut hello representeras av månadskomponenten för parametern X. <br/><br/>Exempel: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (datum som representerar hello slutet av September månad) |
| Date |StartOfDay(X) |X: DateTime |Hämtar hello början av hello dag som representeras av hello dagkomponenten för parametern X.<br/><br/>Exempel: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`. |
| Datum och tid |FROM(X) |X: sträng |Parsa strängen X tooa datum och tid. |
| Datum och tid |Ticks(X) |X: DateTime |Hämtar hello tick-egenskapen för hello parametern X. En skalstreck är lika med 100 nanosekunder. hello värdet på egenskapen representerar hello antalet skalstreck som har förflutit sedan 12:00:00 midnatt den 1 januari 0001. |
| Text |Format(X) |X: string-variabel |Format hello text (Använd `\\'` kombination tooescape `'` tecken).|

> [!IMPORTANT]
> När du använder en funktion i en annan funktion behöver du inte toouse  **$$**  prefix för inre hello-funktionen. Till exempel: $$Text.Format ('PartitionKey eq \\' my_pkey_filter_value\\' och ge RowKey \\' {0: ÅÅÅÅ-MM-dd: mm: ss}\\'', Time.AddHours (SliceStart -6)). Observera att i det här exemplet  **$$**  prefix används inte för hello **Time.AddHours** funktion. 

#### <a name="example"></a>Exempel
I hello följande parametrar för hello Hive aktivitet som exempel, ingående och utgående bestäms genom att använda hello `Text.Format` funktion och SliceStart systemvariabeln. 

```json  
{
    "name": "HiveActivitySamplePipeline",
        "properties": {
    "activities": [
            {
            "name": "HiveActivitySample",
            "type": "HDInsightHive",
            "inputs": [
                    {
                    "name": "HiveSampleIn"
                    }
            ],
            "outputs": [
                    {
                    "name": "HiveSampleOut"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                }
            }
            }
    ]
    }
}
```

### <a name="example-2"></a>Exempel 2

I följande exempel hello, hello hello DateTime-parametern för hello lagrade Proceduraktiviteten bestäms med hjälp av Text. Formatera funktionen och hello SliceStart variabeln. 

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
            "start": "2016-08-02T00:00:00Z",
            "end": "2016-08-02T05:00:00Z",
        "isPaused": false
    }
}
```

### <a name="example-3"></a>Exempel 3
tooread data från föregående dag i stället för dag som representeras av hello SliceStart, funktionen hello AddDays som visas i följande exempel hello: 

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-01-01T08:00:00",
        "end": "2017-01-01T11:00:00",
        "description": "hive activity",
        "activities": [
            {
                "name": "SampleHiveActivity",
                "inputs": [
                    {
                        "name": "MyAzureBlobInput",
                        "startTime": "Date.AddDays(SliceStart, -1)",
                        "endTime": "Date.AddDays(SliceEnd, -1)"
                    }
                ],
                "outputs": [
                    {
                        "name": "MyAzureBlobOutput"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adftutorial\\hivequery.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Year": "$$Text.Format('{0:yyyy}',WindowsStart)",
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

Se [anpassade datum och tid formatsträngar](https://msdn.microsoft.com/library/8kb3ddd4.aspx) avsnitt som beskriver olika formateringsalternativ som du kan använda (till exempel: åå kontra åååå). 

