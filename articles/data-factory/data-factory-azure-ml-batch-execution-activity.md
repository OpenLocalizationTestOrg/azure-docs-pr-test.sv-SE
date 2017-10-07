---
title: "aaaCreate förutsägande data pipelines med Azure Data Factory | Microsoft Docs"
description: "Beskriver hur toocreate skapar förutsägbara pipelines med hjälp av Azure Data Factory och Azure Machine Learning"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4fad8445-4e96-4ce0-aa23-9b88e5ec1965
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 943210c28b1696e299ff9b7cc96369b95f182354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Skapa förutsägande pipelines med hjälp av Azure Machine Learning och Azure Data Factory

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-aktivitet](data-factory-hive-activity.md) 
> * [Pig-aktivitet](data-factory-pig-activity.md)
> * [MapReduce Activity](data-factory-map-reduce.md)
> * [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md)
> * [Spark-aktivitet](data-factory-spark.md)
> * [Machine Learning Batch-körningsaktivitet](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning-uppdateringsresursaktivitet](data-factory-azure-ml-update-resource-activity.md)
> * [Lagrad proceduraktivitet](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-aktivitet](data-factory-usql-activity.md)
> * [Anpassad aktivitet för .NET](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Introduktion

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) aktiverar toobuild, testa och distribuera prediktiva Analyslösningar. Från en övergripande synsätt görs i tre steg:

1. **Skapa en träningsexperiment**. Du kan göra det här steget med hello Azure ML Studio. hello ML studio är en gemensam visual utvecklingsmiljö om du använder tootrain och testa en förutsägelseanalysmodell med hjälp av utbildningsdata.
2. **Konvertera den tooa prediktivt experiment**. När din modell har tränats med befintliga data och du är redo toouse den tooscore nya data, du förbereda och förenkla experimentet för resultatfunktioner.
3. **Distribuera den som en webbtjänst**. Du kan publicera experimentet bedömningsprofil som Azure-webbtjänst. Du kan skicka tooyour datamodellen via den här slutet för webbtjänst och ta emot resultatet förutsägelser från hello modellen.  

### <a name="azure-data-factory"></a>Azure Data Factory
Data Factory är en molnbaserad integration datatjänst som samordnar och automatiserar hello **flytt** och **omvandling** av data. Du kan skapa data integration lösningar med hjälp av Azure Data Factory som kan mata in data från olika datakällor, transformera/bearbeta hello data och publicera hello resultatet data toohello datalager.

Data Factory-tjänsten kan du toocreate data pipelines som flyttas och transformera data och kör sedan hello pipelines enligt ett angivet schema (varje timme, varje dag, vecka, etc.). Dessutom ger omfattande visualiseringar toodisplay hello härkomst och beroenden mellan dina data pipelines och övervaka alla data pipelines från en enda enhetlig vy tooeasily hitta problem och konfigurera övervakningsaviseringar.

Se [introduktion tooAzure Data Factory](data-factory-introduction.md) och [skapa din första pipeline](data-factory-build-your-first-pipeline.md) artiklar tooquickly Kom igång med hello Azure Data Factory-tjänsten.

### <a name="data-factory-and-machine-learning-together"></a>Data Factory och Machine Learning tillsammans
Azure Data Factory aktiverar du tooeasily skapa pipelines som använder en publicerade [Azure Machine Learning] [ azure-machine-learning] webbtjänsten för förutsägelseanalys. Med hjälp av hello **Batchkörningsaktivitet** i Azure Data Factory-pipelinen, du kan anropa en Azure ML web service toomake förutsägelser på hello data i en batch. Se [anropar en Azure ML web service använder hello Batchkörningsaktivitet](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) information.

Över tiden måste hello förutsägelsemodeller i hello Azure ML bedömningsprofil experiment toobe retrained med nya indatauppsättningar. Du kan träna om Azure ML-modell från Data Factory-pipelinen genom att göra hello följande steg:

1. Publicera hello träningsexperiment (inte prediktivt experiment) som en webbtjänst. Du kan göra det här steget i hello Azure ML Studio som du gjorde tooexpose prediktivt experiment som en webbtjänst i hello föregående scenario.
2. Använd hello Azure ML-Batchkörningsaktivitet tooinvoke hello webbtjänsten hello träningsexperiment. I princip kan du använda hello Azure ML-batchkörning aktiviteten tooinvoke både utbildning webbtjänsten och poängsättning av webbtjänsten.

När du är klar med omtränings uppdatera hello bedömningen webbtjänst (prediktivt experiment visas som en webbtjänst) med hello nyligen tränade modellen med hjälp av hello **Azure ML uppdatera resurs aktiviteten**. Se [uppdatering modeller med Uppdateringsresursaktivitet](data-factory-azure-ml-update-resource-activity.md) artikeln för information.

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Anropa en webbtjänst med hjälp av Batchkörningsaktivitet
Du använder Azure Data Factory tooorchestrate dataförflyttning och bearbetning och sedan utföra batchkörning med hjälp av Azure Machine Learning. Här är hello översta steg:

1. Skapa en Azure Machine Learning länkad tjänst. Du behöver hello följande värden:

   1. **Begärande-URI** för hello Batch Execution API. Du kan hitta hello Begärd URI genom att klicka på hello **BATCH EXECUTION** länk hello web services-sidan.
   2. **API-nyckel** för hello publicerade Azure Machine Learning-webbtjänst. Du hittar hello API-nyckel genom att klicka på hello-webbtjänst som du har publicerat.
   3. Använd hello **AzureMLBatchExecution** aktivitet.

      ![Machine Learning-instrumentpanelen](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Batch URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-toodata-in-azure-blob-storage"></a>Scenario: Försök med hjälp av Web service indata/utdata som refererar toodata i Azure Blob Storage
I det här scenariot hello Azure Machine Learning-webbtjänsten gör förutsägelser med hjälp av data från en fil i ett Azure blob storage och lagrar hello förutsägelse resultat i hello blob storage. hello definierar följande JSON Data Factory-pipelinen med en AzureMLBatchExecution aktivitet. hello aktiviteten har hello dataset **DecisionTreeInputBlob** som indata och **DecisionTreeResultBlob** som hello utdata. Hej **DecisionTreeInputBlob** skickas som ett inkommande toohello webbtjänsten genom att använda hello **webServiceInput** JSON-egenskapen. Hej **DecisionTreeResultBlob** skickas som en webbtjänst för utdata toohello genom att använda hello **webServiceOutputs** JSON-egenskapen.  

> [!IMPORTANT]
> Om hello webbtjänst tar flera inmatningar kan använda hello **webServiceInputs** egenskapen istället för att använda **webServiceInput**. Se hello [webbtjänst kräver flera indata](#web-service-requires-multiple-inputs) för ett exempel på hur hello webServiceInputs egenskapen.
>
> Datauppsättningar som refereras av hello **webServiceInput**/**webServiceInputs** och **webServiceOutputs** egenskaper (i  **typeProperties**) måste också tas med i hello aktiviteten **indata** och **matar ut**.
>
> Ha standardnamnen (”input1”, ”input2”) som du kan anpassa i experimentet Azure ML webbtjänst och portar och globala parametrar. hello-namn som du använder för webServiceInputs, webServiceOutputs och globalParameters inställningar måste exakt matcha hello namnen i hello experiment. Du kan visa hello begärannyttolast för exempel på hello Batch Execution hjälpsidan för din Azure ML endpoint tooverify hello förväntades mappning.
>
>

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchExecution",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "DecisionTreeInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "DecisionTreeResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "typeProperties":
        {
            "webServiceInput": "DecisionTreeInputBlob",
            "webServiceOutputs": {
                "output1": "DecisionTreeResultBlob"
            }                
        },
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```
> [!NOTE]
> Endast indata och utdata för hello AzureMLBatchExecution aktivitet kan skickas som parametrar toohello webbtjänsten. Hello ovan JSON fragment är exempelvis DecisionTreeInputBlob en inkommande toohello AzureMLBatchExecution aktivitet som skickas som ett inkommande toohello webbtjänsten via webServiceInput-parametern.   
>
>

### <a name="example"></a>Exempel
Det här exemplet använder Azure Storage toohold hello både inkommande och utgående data.

Vi rekommenderar att du går igenom hello [skapa din första pipeline med Data Factory] [ adf-build-1st-pipeline] självstudiekursen innan du fortsätter med det här exemplet. Använd hello Data Factory-redigeraren toocreate Data Factory artefakter (länkade tjänster, datauppsättningar, rörledningar) i det här exemplet.   

1. Skapa en **länkade tjänsten** för din **Azure Storage**. Om hello inkommande och utgående filer finns i olika storage-konton, måste två länkade tjänster. Här är en JSON-exempel:

    ```JSON
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
        }
      }
    }
    ```
2. Skapa hello **inkommande** Azure Data Factory **dataset**. Till skillnad från vissa andra Data Factory datauppsättningar, måste dessa data inte innehålla både **folderPath** och **fileName** värden. Du kan använda partitionering toocause tooprocess varje batch-körningen (varje datasektorn) eller producera unika inkommande och utgående filer. Du kan behöva tooinclude vissa uppströmsaktivitet tootransform hello indata för hello CSV-filformat och placera den i hello storage-konto för varje segment. I så fall skulle du inte inkludera hello **externa** och **externalData** inställningarna i hello följande exempel och din DecisionTreeInputBlob skulle vara hello datamängd för utdata för en annan aktivitet.

    ```JSON
    {
      "name": "DecisionTreeInputBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/input",
          "fileName": "in.csv",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
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

    Inkommande csv-filen måste ha hello Kolumnrubrikrad. Om du använder hello **Kopieringsaktiviteten** toocreate/flytta hello csv hello blob-lagring, bör du ange hello sink egenskapen **blobWriterAddHeader** för**SANT**. Exempel:

    ```JSON
    sink:
    {
        "type": "BlobSink",     
        "blobWriterAddHeader": true
    }
    ```

    Om hello csv-filen saknar hello rubrikraden, du kan se hello följande fel: **fel i aktiviteten: fel vid läsning av strängen. Oväntad token: StartObject. Sökvägen '', rad 1, position 1**.
3. Skapa hello **utdata** Azure Data Factory **dataset**. Det här exemplet använder partitionering toocreate en unik Utdatasökväg för varje segment-körning. Utan hello partitionering, skulle hello aktiviteten över hello-fil.

    ```JSON
    {
      "name": "DecisionTreeResultBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/scored/{folderpart}/",
          "fileName": "{filepart}result.csv",
          "partitionedBy": [
            {
              "name": "folderpart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyyMMdd"
              }
            },
            {
              "name": "filepart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HHmmss"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 15
        }
      }
    }
    ```
4. Skapa en **länkade tjänsten** av typen: **AzureMLLinkedService**, tillhandahåller hello API-nyckel och modellerar batch execution URL.

    ```JSON
    {
      "name": "MyAzureMLLinkedService",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
          "mlEndpoint": "https://[batch execution endpoint]/jobs",
          "apiKey": "[apikey]"
        }
      }
    }
    ```
5. Slutligen skapar en pipeline som innehåller en **AzureMLBatchExecution** aktivitet. Vid körning utför pipeline hello följande steg:

   1. Hämtar hello platsen för filen med indata-hello från dina indata datauppsättningar.
   2. Anropar hello-batchkörning i Azure Machine Learning API
   3. Kopior hello batch execution utdata toohello blob i datamängd för utdata.

      > [!NOTE]
      > AzureMLBatchExecution aktivitet kan ha noll eller flera in- och utdata för en eller flera.
      >
      >

    ```JSON
    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [
            {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                {
                    "name": "DecisionTreeInputBlob"
                }
                ],
                "outputs": [
                {
                    "name": "DecisionTreeResultBlob"
                }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }
    ```

      Båda **starta** och **end** datum och tid måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601). Exempel: 2014-10-14T16:32:41Z. Hej **end** tid är valfritt. Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar.**” toorun hello pipeline på obestämd tid, ange **9999-09-09** som hello värde hello **end** egenskapen. Se [Referens för JSON-skript](https://msdn.microsoft.com/library/dn835050.aspx) för information om JSON-egenskaper.

      > [!NOTE]
      > Ange indata för hello AzureMLBatchExecution aktivitet är valfritt.
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-toorefer-toodata-in-various-storages"></a>Scenario: Försök använda Reader/Writer moduler toorefer toodata i olika lagringsobjekt
Ett annat vanligt scenario när du skapar Azure ML experiment är toouse läsare och skrivare moduler. modul för dataläsare för hello är används tooload data till ett experiment hello-skrivarmodul är toosave data från dina experiment. Mer information om läsare och skrivare finns [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) och [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) avsnitt i MSDN Library.     

När du använder hello läsare och skrivare moduler, är det bra toouse en Web service-parameter för varje egenskap för dessa moduler reader/writer. Dessa webb-parametrar kan du tooconfigure hello värden under körningen. Du kan till exempel skapa ett experiment med en modul för läsare som använder en Azure SQL Database: XXX.database.windows.net. När hello webbtjänst har distribuerats, vill du tooenable hello konsumenter av hello web service toospecify en annan Azure SQL-Server som kallas YYY.database.windows.net. Du kan använda en Web service parametern tooallow det här värdet toobe konfigurerats.

> [!NOTE]
> Webbtjänst och utdata skiljer sig från webbtjänstparametrar. I hello första scenariot har du sett hur indata och utdata kan anges för en Azure ML-webbtjänst. I det här scenariot kan du överföra parametrar för en webbtjänst som motsvarar tooproperties av reader/writer moduler.
>
>

Nu ska vi titta på ett scenario för att använda webbtjänstparametrar. Du har en distribuerad Azure Machine Learning-webbtjänst som använder en läsare modulen tooread data från en hello datakällor som stöds av Azure Machine Learning (till exempel: Azure SQL Database). När hello batchkörning utförs, skrivs hello resultat med skrivarmodul (Azure SQL Database).  Ingen service indata och utdata har definierats i hello experiment. I detta fall rekommenderar vi att du konfigurerar relevanta webbtjänstparametrar för hello läsare och skrivare moduler. Den här konfigurationen kan hello reader/writer moduler toobe konfigureras när du använder hello AzureMLBatchExecution aktiviteten. Du anger webbtjänstparametrar i hello **globalParameters** under hello aktivitets-JSON på följande sätt.

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

Du kan också använda [Data Factory funktioner](data-factory-functions-variables.md) i och ange värden för hello Web Serviceparametrar som visas i följande exempel hello:

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> hello webbtjänstparametrar är skiftlägeskänsligt, så se till att hello-namn som du anger i hello aktivitet JSON matchar hello som exponeras av hello webbtjänsten.
>
>

### <a name="using-a-reader-module-tooread-data-from-multiple-files-in-azure-blob"></a>Med hjälp av en läsare modulen tooread data från flera filer i Azure Blob
Stordata rörledningar med aktiviteter, till exempel Pig och Hive kan ge en eller flera utgående filer utan filnamnstillägg. Till exempel när du anger en extern Hive-tabell kan hello data för hello externa Hive-tabell lagras i Azure blob storage med följande namn 000000_0 hello. Du kan använda hello reader modul i ett experiment tooread flera filer och använda dem för förutsägelser.

När modulen hello läsare i en Azure Machine Learning-experiment, kan du ange Azure Blob som indata. hello-filer i hello Azure blob-lagring kan vara hello utdatafilerna (exempel: 000000_0) som produceras av ett Pig och Hive-skript som körs på HDInsight. hello modul för dataläsare för kan du tooread filer (med inga) genom att konfigurera hello **sökväg toocontainer, katalog-blob**. Hej **sökväg toocontainer** punkter toohello behållare och **directory/blob** pekar toofolder som innehåller hello filer enligt följande bild hello. hello asterisk som är \*) **anger att alla hello filer i hello behållare/mappen (det vill säga år-aggregateddata-data = månad/2014-6 /\*)** läses som en del av hello experiment.

![Azure Blob-egenskaper](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a>Exempel
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Pipeline med AzureMLBatchExecution aktivitet med Webbtjänstparametrar

```JSON
{
  "name": "MLWithSqlReaderSqlWriter",
  "properties": {
    "description": "Azure ML model with sql azure reader/writer",
    "activities": [
      {
        "name": "MLSqlReaderSqlWriterActivity",
        "type": "AzureMLBatchExecution",
        "description": "test",
        "inputs": [
          {
            "name": "MLSqlInput"
          }
        ],
        "outputs": [
          {
            "name": "MLSqlOutput"
          }
        ],
        "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
        "typeProperties":
        {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
                "output1": "MLSqlOutput"
            }
              "globalParameters": {
                "Database server name": "<myserver>.database.windows.net",
                "Database name": "<database>",
                "Server user account name": "<user name>",
                "Server user account password": "<password>"
              }              
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        },
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

I hello ovan JSON-exempel:

* hello distribuerade tjänsten använder en läsare och skrivare modulen tooread/skriva data från Azure Machine Learning Web / tooan Azure SQL Database. Den här webbtjänsten visar hello följande fyra parametrar: databasen servernamnet, databasnamnet, Server-användarkonto och serverlösenord.  
* Båda **starta** och **end** datum och tid måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601). Exempel: 2014-10-14T16:32:41Z. Hej **end** tid är valfritt. Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar.**” toorun hello pipeline på obestämd tid, ange **9999-09-09** som hello värde hello **end** egenskapen. Se [Referens för JSON-skript](https://msdn.microsoft.com/library/dn835050.aspx) för information om JSON-egenskaper.

### <a name="other-scenarios"></a>Andra scenarier
#### <a name="web-service-requires-multiple-inputs"></a>Webbtjänsten kräver flera indata
Om hello webbtjänst tar flera inmatningar kan använda hello **webServiceInputs** egenskapen istället för att använda **webServiceInput**. Datauppsättningar som refereras av hello **webServiceInputs** måste också tas med i hello aktiviteten **indata**.

Ha standardnamnen (”input1”, ”input2”) som du kan anpassa i experimentet Azure ML webbtjänst och portar och globala parametrar. hello-namn som du använder för webServiceInputs, webServiceOutputs och globalParameters inställningar måste exakt matcha hello namnen i hello experiment. Du kan visa hello begärannyttolast för exempel på hello Batch Execution hjälpsidan för din Azure ML endpoint tooverify hello förväntades mappning.

```JSON
{
    "name": "PredictivePipeline",
    "properties": {
        "description": "use AzureML model",
        "activities": [{
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [{
                "name": "inputDataset1"
            }, {
                "name": "inputDataset2"
            }],
            "outputs": [{
                "name": "outputDataset"
            }],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties": {
                "webServiceInputs": {
                    "input1": "inputDataset1",
                    "input2": "inputDataset2"
                },
                "webServiceOutputs": {
                    "output1": "outputDataset"
                }
            },
            "policy": {
                "concurrency": 3,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "02:00:00"
            }
        }],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
    }
}
```

#### <a name="web-service-does-not-require-an-input"></a>Webbtjänsten kräver inte indata
Azure ML batch execution webbtjänster kan vara används toorun alla arbetsflöden, till exempel R eller Python-skript som kan inte kräva att alla indata. Eller hello experiment kan konfigureras med en modul för läsare som inte exponerar någon GlobalParameters. I så fall ska hello AzureMLBatchExecution aktiviteten konfigureras på följande sätt:

```JSON
{
    "name": "scoring service",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "myBlob"
        }
    ],
    "typeProperties": {
        "webServiceOutputs": {
            "output1": "myBlob"
        }              
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-does-not-require-an-inputoutput"></a>Webbtjänsten kräver inte en in-/ utdata
hello Azure ML batch körningstjänsten kanske inte har några utdata för webbtjänst konfigurerats. Det finns ingen webbtjänsten indata eller utdata eller konfigureras alla GlobalParameters i det här exemplet. Det finns fortfarande utdata konfigurerats på hello aktivitet sig själv, men anges inte som en webServiceOutput.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "placeholderOutputDataset"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-uses-readers-and-writers-and-hello-activity-runs-only-when-other-activities-have-succeeded"></a>Web Service använder läsare och skrivare och hello aktiviteten körs endast när andra aktiviteter har lyckades
hello Azure ML web service läsare och skrivare moduler kan vara konfigurerade toorun med eller utan någon GlobalParameters. Du kanske vill tooembed tjänstanrop i en pipeline som använder dataset beroenden tooinvoke hello tjänst endast när vissa överordnade bearbetningen har slutförts. Du kan också utlösa någon annan åtgärd när hello batch-körningen har slutförts med den här metoden. I så fall kan du ange hello beroenden som använder aktivitetens indata och utdata, utan att namnge någon av dem som webbtjänsten indata eller utdata.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "inputs": [
        {
            "name": "upstreamData1"
        },
        {
            "name": "upstreamData2"
        }
    ],
    "outputs": [
        {
            "name": "downstreamData"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

Hej **takeaways** är:

* Om slutpunkten experimentet använder en webServiceInput: den representeras av en blobbdatauppsättning och ingår i hello aktivitetens indata- och hello webServiceInput egenskapen. Annars utelämnas hello webServiceInput egenskapen.
* Om slutpunkten experimentet använder webServiceOutput(s): de representeras av blob-datauppsättningar och ingår i hello aktivitetsutdata och hello webServiceOutputs egenskapen. hello aktivitet matar ut och webServiceOutputs mappas av hello namn för varje utdata i hello experiment. Annars utelämnas hello webServiceOutputs egenskapen.
* Om din experiment slutpunkt visar globalParameter(s), anges de i hello aktivitetsegenskap globalParameters som nyckel, värde-par. Annars utelämnas hello globalParameters egenskapen. hello nycklar är skiftlägeskänsliga. [Azure Data Factory-funktioner](data-factory-functions-variables.md) får användas i hello värden.
* Ytterligare datauppsättningar kan ingå i hello in- och utdataenheter Aktivitetsegenskaper, utan som refereras i hello aktiviteten typeProperties. De här datauppsättningarna styr körning med hjälp av sektorn beroenden men ignoreras annars av hello AzureMLBatchExecution aktivitet.


## <a name="updating-models-using-update-resource-activity"></a>Uppdatering av modeller som använder Uppdateringsresursaktivitet
När du är klar med omtränings uppdatera hello bedömningen webbtjänst (prediktivt experiment visas som en webbtjänst) med hello nyligen tränade modellen med hjälp av hello **Azure ML uppdatera resurs aktiviteten**. Se [uppdatering modeller med Uppdateringsresursaktivitet](data-factory-azure-ml-update-resource-activity.md) artikeln för information.

### <a name="reader-and-writer-modules"></a>Läsare och skrivare moduler
Ett vanligt scenario för att använda webbtjänstparametrar är hello använda Azure SQL-läsare och skrivare. modul för dataläsare för hello är används tooload data till ett experiment från data management-tjänster utanför Azure Machine Learning Studio. hello-skrivarmodul är toosave data från dina experiment till data management services utanför Azure Machine Learning Studio.  

Mer information om Azure Blob-/ Azure SQL reader/writer finns [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) och [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) avsnitt i MSDN Library. hello exemplet i föregående avsnitt i hello används hello Azure Blob-läsare och Azure Blob-skrivare. Det här avsnittet beskrivs med SQL Azure reader och Azure SQL writer.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
**F:** jag har flera filer som genereras av min stordata pipelines. Kan jag använda hello AzureMLBatchExecution aktiviteten toowork på alla hello-filer?

**S:** Ja. Se hello **med hjälp av en läsare modulen tooread data från flera filer i Azure Blob** information.

## <a name="azure-ml-batch-scoring-activity"></a>Azure ML bedömningen batchaktiviteten
Om du använder hello **AzureMLBatchScoring** aktiviteten toointegrate med Azure Machine Learning, rekommenderar vi att du använder hello senaste **AzureMLBatchExecution** aktivitet.

Hej AzureMLBatchExecution aktivitet introducerades i hello augusti 2015-versionen av Azure SDK och Azure PowerShell.

Om du vill toocontinue hello AzureMLBatchScoring aktivitet kan fortsätta läsa igenom det här avsnittet.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Azure ML-Batchbedömningen aktiviteten med Azure Storage för in-/ utdata

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchScoring",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "ScoringInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "ScoringResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

### <a name="web-service-parameters"></a>Webbtjänstparametrar
toospecify värden för webbtjänstparametrar, lägga till en **typeProperties** avsnittet toohello **AzureMLBatchScoringActivty** avsnitt i pipeline-hello JSON som visas i följande exempel hello:

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
Du kan också använda [Data Factory funktioner](data-factory-functions-variables.md) i och ange värden för hello Web Serviceparametrar som visas i följande exempel hello:

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> hello webbtjänstparametrar är skiftlägeskänsligt, så se till att hello-namn som du anger i hello aktivitet JSON matchar hello som exponeras av hello webbtjänsten.
>
>

## <a name="see-also"></a>Se även
* [Azure blogginlägg: komma igång med Azure Data Factory och Azure Machine Learning](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/
