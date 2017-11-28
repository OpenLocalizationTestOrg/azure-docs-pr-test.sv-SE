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
# <a name="machine-learning-integration-in-stream-analytics"></a><span data-ttu-id="c8d39-103">Datorn Learning integrering i Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="c8d39-103">Machine Learning integration in Stream Analytics</span></span>
<span data-ttu-id="c8d39-104">Stream Analytics stöder användardefinierade funktioner som anropar ut tooAzure Machine Learning-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="c8d39-104">Stream Analytics supports user-defined functions that call out tooAzure Machine Learning endpoints.</span></span> <span data-ttu-id="c8d39-105">REST API-stöd för den här funktionen beskrivs i hello [Stream Analytics REST API-bibliotek](https://msdn.microsoft.com/library/azure/dn835031.aspx).</span><span class="sxs-lookup"><span data-stu-id="c8d39-105">REST API support for this feature is detailed in hello [Stream Analytics REST API library](https://msdn.microsoft.com/library/azure/dn835031.aspx).</span></span> <span data-ttu-id="c8d39-106">Den här artikeln innehåller ytterligare information som behövs för lyckad implementering av den här funktionen i Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="c8d39-106">This article provides supplemental information needed for successful implementation of this capability in Stream Analytics.</span></span> <span data-ttu-id="c8d39-107">En självstudiekurs har även publicerats och är tillgänglig [här](stream-analytics-machine-learning-integration-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="c8d39-107">A tutorial has also been posted and is available [here](stream-analytics-machine-learning-integration-tutorial.md).</span></span>

## <a name="overview-azure-machine-learning-terminology"></a><span data-ttu-id="c8d39-108">Översikt: Terminologi för Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c8d39-108">Overview: Azure Machine Learning terminology</span></span>
<span data-ttu-id="c8d39-109">Microsoft Azure Machine Learning ger ett samarbete, dra och släpp verktyg som du kan använda toobuild, testa och distribuera prediktiva Analyslösningar utifrån dina data.</span><span class="sxs-lookup"><span data-stu-id="c8d39-109">Microsoft Azure Machine Learning provides a collaborative, drag-and-drop tool you can use toobuild, test, and deploy predictive analytics solutions on your data.</span></span> <span data-ttu-id="c8d39-110">Det här verktyget kallas hello *Azure Machine Learning Studio*.</span><span class="sxs-lookup"><span data-stu-id="c8d39-110">This tool is called hello *Azure Machine Learning Studio*.</span></span> <span data-ttu-id="c8d39-111">hello studio är används toointeract med hello Machine Learning-resurser och enkelt kan skapa, testa och iterera på din design.</span><span class="sxs-lookup"><span data-stu-id="c8d39-111">hello studio is used toointeract with hello Machine Learning resources and easily build, test, and iterate on your design.</span></span> <span data-ttu-id="c8d39-112">Dessa resurser och deras definitioner som är lägre än.</span><span class="sxs-lookup"><span data-stu-id="c8d39-112">These resources and their definitions are below.</span></span>

* <span data-ttu-id="c8d39-113">**Arbetsytan**: hello *arbetsytan* är en behållare som innehåller alla andra Machine Learning resurser tillsammans i en behållare för hantering och kontroll.</span><span class="sxs-lookup"><span data-stu-id="c8d39-113">**Workspace**: hello *workspace* is a container that holds all other Machine Learning resources together in a container for management and control.</span></span>
* <span data-ttu-id="c8d39-114">**Experiment**: *experiment* skapas av data forskare tooutilize datauppsättningar och tränar en modell för maskininlärning.</span><span class="sxs-lookup"><span data-stu-id="c8d39-114">**Experiment**: *Experiments* are created by data scientists tooutilize datasets and train a machine learning model.</span></span>
* <span data-ttu-id="c8d39-115">**Slutpunkten**: *slutpunkter* hello tootake-funktioner för Azure Machine Learning-objektet som används som indata, tillämpa en angivna machine learning-modell och returnera poängsatta utdata.</span><span class="sxs-lookup"><span data-stu-id="c8d39-115">**Endpoint**: *Endpoints* are hello Azure Machine Learning object used tootake features as input, apply a specified machine learning model and return scored output.</span></span>
* <span data-ttu-id="c8d39-116">**Bedömningen Webservice**: A *bedömningen webservice* är en uppsättning slutpunkter som nämns ovan.</span><span class="sxs-lookup"><span data-stu-id="c8d39-116">**Scoring Webservice**: A *scoring webservice* is a collection of endpoints as mentioned above.</span></span>

<span data-ttu-id="c8d39-117">Varje slutpunkt har API: er för batchkörning och synkron körning.</span><span class="sxs-lookup"><span data-stu-id="c8d39-117">Each endpoint has apis for batch execution and synchronous execution.</span></span> <span data-ttu-id="c8d39-118">Stream Analytics använder synkron körning.</span><span class="sxs-lookup"><span data-stu-id="c8d39-118">Stream Analytics uses synchronous execution.</span></span> <span data-ttu-id="c8d39-119">hello specifika heter en [frågor och svar Service](../machine-learning/machine-learning-consume-web-services.md) i AzureML studio.</span><span class="sxs-lookup"><span data-stu-id="c8d39-119">hello specific service is named a [Request/Response Service](../machine-learning/machine-learning-consume-web-services.md) in AzureML studio.</span></span>

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a><span data-ttu-id="c8d39-120">Machine Learning resurser som krävs för Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="c8d39-120">Machine Learning resources needed for Stream Analytics jobs</span></span>
<span data-ttu-id="c8d39-121">Jobbet bearbeta en begäran och svar-slutpunkt för hello Stream Analytics en [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), och en swagger-definition är alla nödvändiga för att kunna köras.</span><span class="sxs-lookup"><span data-stu-id="c8d39-121">For hello purposes of Stream Analytics job processing, a Request/Response endpoint, an [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), and a swagger definition are all necessary for successful execution.</span></span> <span data-ttu-id="c8d39-122">Stream Analytics har en ytterligare slutpunkt som konstruktioner hello url för swagger-slutpunkt, letar upp hello-gränssnittet och returnerar en UDF definition toohello standardanvändare.</span><span class="sxs-lookup"><span data-stu-id="c8d39-122">Stream Analytics has an additional endpoint that constructs hello url for swagger endpoint, looks up hello interface and returns a default UDF definition toohello user.</span></span>

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a><span data-ttu-id="c8d39-123">Konfigurera ett Stream Analytics och Maskininlärning UDF via REST-API</span><span class="sxs-lookup"><span data-stu-id="c8d39-123">Configure a Stream Analytics and Machine Learning UDF via REST API</span></span>
<span data-ttu-id="c8d39-124">Du kan konfigurera din jobbfunktioner toocall språk för Azure-datorn med hjälp av REST API: er.</span><span class="sxs-lookup"><span data-stu-id="c8d39-124">By using REST APIs you may configure your job toocall Azure Machine Language functions.</span></span> <span data-ttu-id="c8d39-125">hello stegen är följande:</span><span class="sxs-lookup"><span data-stu-id="c8d39-125">hello steps are as follows:</span></span>

1. <span data-ttu-id="c8d39-126">Skapa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="c8d39-126">Create a Stream Analytics job</span></span>
2. <span data-ttu-id="c8d39-127">Definiera indata</span><span class="sxs-lookup"><span data-stu-id="c8d39-127">Define an input</span></span>
3. <span data-ttu-id="c8d39-128">Definiera ett utgående</span><span class="sxs-lookup"><span data-stu-id="c8d39-128">Define an output</span></span>
4. <span data-ttu-id="c8d39-129">Skapa en användardefinierad funktion (UDF)</span><span class="sxs-lookup"><span data-stu-id="c8d39-129">Create a user-defined function (UDF)</span></span>
5. <span data-ttu-id="c8d39-130">Skriv en Stream Analytics-omvandling att anrop hello UDF</span><span class="sxs-lookup"><span data-stu-id="c8d39-130">Write a Stream Analytics transformation that calls hello UDF</span></span>
6. <span data-ttu-id="c8d39-131">Starta hello jobbet</span><span class="sxs-lookup"><span data-stu-id="c8d39-131">Start hello job</span></span>

## <a name="creating-a-udf-with-basic-properties"></a><span data-ttu-id="c8d39-132">Skapa en UDF med grundläggande egenskaper</span><span class="sxs-lookup"><span data-stu-id="c8d39-132">Creating a UDF with basic properties</span></span>
<span data-ttu-id="c8d39-133">Exempelvis hello följande exempelkod skapar en skalär användardefinierad funktion med namnet *newudf* som binder tooan Azure Machine Learning-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="c8d39-133">As an example, hello following sample code creates a scalar UDF named *newudf* that binds tooan Azure Machine Learning endpoint.</span></span> <span data-ttu-id="c8d39-134">Observera att hello *endpoint* (service URI) finns på hello API hjälpsidan för hello valt tjänsten och hello *apiKey* kan hittas på huvudsidan för hello-tjänster.</span><span class="sxs-lookup"><span data-stu-id="c8d39-134">Note that hello *endpoint* (service URI) can be found on hello API help page for hello chosen service and hello *apiKey* can be found on hello Services main page.</span></span>

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

<span data-ttu-id="c8d39-135">Exempel frågans brödtext:</span><span class="sxs-lookup"><span data-stu-id="c8d39-135">Example request body:</span></span>  

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

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a><span data-ttu-id="c8d39-136">Anropet RetrieveDefaultDefinition slutpunkt för standard UDF</span><span class="sxs-lookup"><span data-stu-id="c8d39-136">Call RetrieveDefaultDefinition endpoint for default UDF</span></span>
<span data-ttu-id="c8d39-137">En gång hello stommen UDF skapas hello klar definition av hello UDF krävs.</span><span class="sxs-lookup"><span data-stu-id="c8d39-137">Once hello skeleton UDF is created hello complete definition of hello UDF is needed.</span></span> <span data-ttu-id="c8d39-138">Hej RetreiveDefaultDefinition endpoint hjälper dig att få hello standarddefinitionen för en skalär funktion som är bundna tooan Azure Machine Learning-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="c8d39-138">hello RetreiveDefaultDefinition endpoint helps you get hello default definition for a scalar function that is bound tooan Azure Machine Learning endpoint.</span></span> <span data-ttu-id="c8d39-139">hello nyttolast nedan kräver tooget hello UDF standarddefinition för en skalär funktion som är bundna tooan Azure Machine Learning-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="c8d39-139">hello payload below requires you tooget hello default UDF definition for a scalar function that is bound tooan Azure Machine Learning endpoint.</span></span> <span data-ttu-id="c8d39-140">Den ange inte hello faktisk slutpunkt som redan har angetts under PUT-förfrågan.</span><span class="sxs-lookup"><span data-stu-id="c8d39-140">It doesn’t specify hello actual endpoint as it has already been provided during PUT request.</span></span> <span data-ttu-id="c8d39-141">Stream Analytics anropar hello-slutpunkt som anges i hello begäran om den anges explicit.</span><span class="sxs-lookup"><span data-stu-id="c8d39-141">Stream Analytics calls hello endpoint provided in hello request if it is provided explicitly.</span></span> <span data-ttu-id="c8d39-142">Annars används hello en refereras ursprungligen.</span><span class="sxs-lookup"><span data-stu-id="c8d39-142">Otherwise it uses hello one originally referenced.</span></span> <span data-ttu-id="c8d39-143">Här hello UDF tar en enda string parametern (en mening) och returnerar en enda utdata av typen sträng som anger hello ”sentiment” etikett för att meningen.</span><span class="sxs-lookup"><span data-stu-id="c8d39-143">Here hello UDF takes a single string parameter (a sentence) and returns a single output of type string which indicates hello “sentiment” label for that sentence.</span></span>

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

<span data-ttu-id="c8d39-144">Exempel frågans brödtext:</span><span class="sxs-lookup"><span data-stu-id="c8d39-144">Example request body:</span></span>  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

<span data-ttu-id="c8d39-145">Ett exempel på utdata för detta skulle titta liknande nedan.</span><span class="sxs-lookup"><span data-stu-id="c8d39-145">A sample output of this would look something like below.</span></span>  

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

## <a name="patch-udf-with-hello-response"></a><span data-ttu-id="c8d39-146">Korrigering UDF med hello svar</span><span class="sxs-lookup"><span data-stu-id="c8d39-146">Patch UDF with hello response</span></span>
<span data-ttu-id="c8d39-147">Nu måste hello UDF korrigeras med hello föregående svar, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="c8d39-147">Now hello UDF must be patched with hello previous response, as shown below.</span></span>

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

<span data-ttu-id="c8d39-148">Begärandetexten (utdata från RetrieveDefaultDefinition):</span><span class="sxs-lookup"><span data-stu-id="c8d39-148">Request Body (Output from RetrieveDefaultDefinition):</span></span>

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

## <a name="implement-stream-analytics-transformation-toocall-hello-udf"></a><span data-ttu-id="c8d39-149">Implementera Stream Analytics omvandling toocall hello UDF</span><span class="sxs-lookup"><span data-stu-id="c8d39-149">Implement Stream Analytics transformation toocall hello UDF</span></span>
<span data-ttu-id="c8d39-150">Nu frågar hello UDF (kallas här scoreTweet) för varje händelse och skriva ett svar för att händelsen tooan utdata.</span><span class="sxs-lookup"><span data-stu-id="c8d39-150">Now query hello UDF (here named scoreTweet) for every input event and write a response for that event tooan output.</span></span>  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a><span data-ttu-id="c8d39-151">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="c8d39-151">Get help</span></span>
<span data-ttu-id="c8d39-152">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="c8d39-152">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8d39-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c8d39-153">Next steps</span></span>
* [<span data-ttu-id="c8d39-154">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="c8d39-154">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="c8d39-155">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="c8d39-155">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="c8d39-156">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="c8d39-156">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="c8d39-157">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="c8d39-157">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="c8d39-158">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="c8d39-158">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
