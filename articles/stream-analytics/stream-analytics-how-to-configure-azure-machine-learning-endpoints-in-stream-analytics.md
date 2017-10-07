---
title: aaaUse Azure Machine Learning-slutpunkterna i Stream Analytics | Microsoft Docs
description: "Datorn språk användardefinierade funktioner i Stream Analytics"
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 406b258f-b8c2-4e55-953c-b7f84e8e5354
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 013b841ee85b1e0b6d8139a9ba0dde88fc3f8ad0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-integration-in-stream-analytics"></a>Datorn Learning integrering i Stream Analytics
Stream Analytics stöder användardefinierade funktioner som anropar ut tooAzure Machine Learning-slutpunkter. REST API-stöd för den här funktionen beskrivs i hello [Stream Analytics REST API-bibliotek](https://msdn.microsoft.com/library/azure/dn835031.aspx). Den här artikeln innehåller ytterligare information som behövs för lyckad implementering av den här funktionen i Stream Analytics. En självstudiekurs har även publicerats och är tillgänglig [här](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Översikt: Terminologi för Azure Machine Learning
Microsoft Azure Machine Learning ger ett samarbete, dra och släpp verktyg som du kan använda toobuild, testa och distribuera prediktiva Analyslösningar utifrån dina data. Det här verktyget kallas hello *Azure Machine Learning Studio*. hello studio är används toointeract med hello Machine Learning-resurser och enkelt kan skapa, testa och iterera på din design. Dessa resurser och deras definitioner som är lägre än.

* **Arbetsytan**: hello *arbetsytan* är en behållare som innehåller alla andra Machine Learning resurser tillsammans i en behållare för hantering och kontroll.
* **Experiment**: *experiment* skapas av data forskare tooutilize datauppsättningar och tränar en modell för maskininlärning.
* **Slutpunkten**: *slutpunkter* hello tootake-funktioner för Azure Machine Learning-objektet som används som indata, tillämpa en angivna machine learning-modell och returnera poängsatta utdata.
* **Bedömningen Webservice**: A *bedömningen webservice* är en uppsättning slutpunkter som nämns ovan.

Varje slutpunkt har API: er för batchkörning och synkron körning. Stream Analytics använder synkron körning. hello specifika heter en [frågor och svar Service](../machine-learning/machine-learning-consume-web-services.md) i AzureML studio.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Machine Learning resurser som krävs för Stream Analytics-jobb
Jobbet bearbeta en begäran och svar-slutpunkt för hello Stream Analytics en [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), och en swagger-definition är alla nödvändiga för att kunna köras. Stream Analytics har en ytterligare slutpunkt som konstruktioner hello url för swagger-slutpunkt, letar upp hello-gränssnittet och returnerar en UDF definition toohello standardanvändare.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Konfigurera ett Stream Analytics och Maskininlärning UDF via REST-API
Du kan konfigurera din jobbfunktioner toocall språk för Azure-datorn med hjälp av REST API: er. hello stegen är följande:

1. Skapa ett Stream Analytics-jobb
2. Definiera indata
3. Definiera ett utgående
4. Skapa en användardefinierad funktion (UDF)
5. Skriv en Stream Analytics-omvandling att anrop hello UDF
6. Starta hello jobbet

## <a name="creating-a-udf-with-basic-properties"></a>Skapa en UDF med grundläggande egenskaper
Exempelvis hello följande exempelkod skapar en skalär användardefinierad funktion med namnet *newudf* som binder tooan Azure Machine Learning-slutpunkt. Observera att hello *endpoint* (service URI) finns på hello API hjälpsidan för hello valt tjänsten och hello *apiKey* kan hittas på huvudsidan för hello-tjänster.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Exempel frågans brödtext:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Anropet RetrieveDefaultDefinition slutpunkt för standard UDF
En gång hello stommen UDF skapas hello klar definition av hello UDF krävs. Hej RetreiveDefaultDefinition endpoint hjälper dig att få hello standarddefinitionen för en skalär funktion som är bundna tooan Azure Machine Learning-slutpunkt. hello nyttolast nedan kräver tooget hello UDF standarddefinition för en skalär funktion som är bundna tooan Azure Machine Learning-slutpunkt. Den ange inte hello faktisk slutpunkt som redan har angetts under PUT-förfrågan. Stream Analytics anropar hello-slutpunkt som anges i hello begäran om den anges explicit. Annars används hello en refereras ursprungligen. Här hello UDF tar en enda string parametern (en mening) och returnerar en enda utdata av typen sträng som anger hello ”sentiment” etikett för att meningen.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Exempel frågans brödtext:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Ett exempel på utdata för detta skulle titta liknande nedan.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-hello-response"></a>Korrigering UDF med hello svar
Nu måste hello UDF korrigeras med hello föregående svar, enligt nedan.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

Begärandetexten (utdata från RetrieveDefaultDefinition):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-toocall-hello-udf"></a>Implementera Stream Analytics omvandling toocall hello UDF
Nu frågar hello UDF (kallas här scoreTweet) för varje händelse och skriva ett svar för att händelsen tooan utdata.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
