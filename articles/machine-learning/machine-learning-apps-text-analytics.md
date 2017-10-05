---
title: 'Machine Learning API: er: Textanalys | Microsoft Docs'
description: "Microsofts Machine Learning Text Analytics API kan användas för att analysera Ostrukturerade text för sentiment analys, viktiga frasen extrahering, identifiera språk och avsnittet identifiering."
services: machine-learning
documentationcenter: 
author: onewth
manager: jhubbard
editor: cgronlun
ms.assetid: 5b60694e-5521-4e4d-bf6a-1a92fdf94b65
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: onewth
ROBOTS: NOINDEX
redirect_url: ../cognitive-services/cognitive-services-text-analytics-quick-start
redirect_document_id: TRUE
ms.openlocfilehash: 10eae2ff5624dcb57de1cf72b326147f35bc2a0b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a><span data-ttu-id="a15d8-103">API:er för Machine Learning: Textanalys för attityder, Extrahering av diskussionsämne, Språkidentifiering och Ämnesidentifiering</span><span class="sxs-lookup"><span data-stu-id="a15d8-103">Machine Learning APIs: Text Analytics for Sentiment, Key Phrase Extraction, Language Detection and Topic Detection</span></span>
> [!NOTE]
> <span data-ttu-id="a15d8-104">Den här guiden är för version 1 av API: et.</span><span class="sxs-lookup"><span data-stu-id="a15d8-104">This guide is for version 1 of the API.</span></span> <span data-ttu-id="a15d8-105">För version 2, [ **finns i det här dokumentet**](../cognitive-services/cognitive-services-text-analytics-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="a15d8-105">For version 2, [**refer to this document**](../cognitive-services/cognitive-services-text-analytics-quick-start.md).</span></span> <span data-ttu-id="a15d8-106">Version 2 är nu den önskade versionen av detta API.</span><span class="sxs-lookup"><span data-stu-id="a15d8-106">Version 2 is now the preferred version of this API.</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="a15d8-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="a15d8-107">Overview</span></span>
<span data-ttu-id="a15d8-108">API: et för Text Analytics är en uppsättning textanalys [webbtjänster](https://datamarket.azure.com/dataset/amla/text-analytics) skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="a15d8-108">The Text Analytics API is a suite of text analytics [web services](https://datamarket.azure.com/dataset/amla/text-analytics) built with Azure Machine Learning.</span></span> <span data-ttu-id="a15d8-109">API: et kan användas för att analysera Ostrukturerade text för åtgärder som sentiment analys, viktiga frasen extrahering, identifiera språk och avsnittet identifiering.</span><span class="sxs-lookup"><span data-stu-id="a15d8-109">The API can be used to analyze unstructured text for tasks such as sentiment analysis, key phrase extraction, language detection and topic detection.</span></span> <span data-ttu-id="a15d8-110">Några övningsdata behövs för att använda den här API: bara ge dina textdata.</span><span class="sxs-lookup"><span data-stu-id="a15d8-110">No training data is needed to use this API: just bring your text data.</span></span> <span data-ttu-id="a15d8-111">Detta API använder avancerade naturligt språk bearbetning av tekniker för att leverera bäst i klassen förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="a15d8-111">This API uses advanced natural language processing techniques to deliver best in class predictions.</span></span>

<span data-ttu-id="a15d8-112">Du kan se textanalys i praktiken på vår [demonstrationswebbplats](https://text-analytics-demo.azurewebsites.net/), där du hittar även [exempel](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) för hur du implementerar textanalys i C# och Python.</span><span class="sxs-lookup"><span data-stu-id="a15d8-112">You can see text analytics in action on our [demo site](https://text-analytics-demo.azurewebsites.net/), where you will also find [samples](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) on how to implement text analytics in C# and Python.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

- - -
## <a name="sentiment-analysis"></a><span data-ttu-id="a15d8-113">Känsloanalys</span><span class="sxs-lookup"><span data-stu-id="a15d8-113">Sentiment analysis</span></span>
<span data-ttu-id="a15d8-114">API: et returnerar ett numeriskt värde mellan 0 och 1.</span><span class="sxs-lookup"><span data-stu-id="a15d8-114">The API returns a numeric score between 0 & 1.</span></span> <span data-ttu-id="a15d8-115">Poäng nära 1 anger positivt sentiment medan poäng nära 0 indikerar negativa sentiment.</span><span class="sxs-lookup"><span data-stu-id="a15d8-115">Scores close to 1 indicate positive sentiment, while scores close to 0 indicate negative sentiment.</span></span> <span data-ttu-id="a15d8-116">Känslopoängen genereras med klassificeringstekniker.</span><span class="sxs-lookup"><span data-stu-id="a15d8-116">Sentiment score is generated using classification techniques.</span></span> <span data-ttu-id="a15d8-117">Indata till klassificeraren bland annat n-g, funktioner som skapas från en del av tal taggar och word inbäddningar.</span><span class="sxs-lookup"><span data-stu-id="a15d8-117">The input features to the classifier include n-grams, features generated from part-of-speech tags, and word embeddings.</span></span> <span data-ttu-id="a15d8-118">Engelska är för närvarande det enda språket som stöds.</span><span class="sxs-lookup"><span data-stu-id="a15d8-118">Currently, English is the only supported language.</span></span>

## <a name="key-phrase-extraction"></a><span data-ttu-id="a15d8-119">Extraktion av nyckelfraser</span><span class="sxs-lookup"><span data-stu-id="a15d8-119">Key phrase extraction</span></span>
<span data-ttu-id="a15d8-120">API:t returnerar en lista med strängar som anger samtalsämnen i den inmatade texten.</span><span class="sxs-lookup"><span data-stu-id="a15d8-120">The API returns a list of strings denoting the key talking points in the input text.</span></span> <span data-ttu-id="a15d8-121">Vi använder tekniker från de avancerade verktygen för språkbearbetning i Microsoft Office.</span><span class="sxs-lookup"><span data-stu-id="a15d8-121">We employ techniques from Microsoft Office's sophisticated Natural Language Processing toolkit.</span></span> <span data-ttu-id="a15d8-122">Engelska är för närvarande det enda språket som stöds.</span><span class="sxs-lookup"><span data-stu-id="a15d8-122">Currently, English is the only supported language.</span></span>

## <a name="language-detection"></a><span data-ttu-id="a15d8-123">Språkspårning</span><span class="sxs-lookup"><span data-stu-id="a15d8-123">Language detection</span></span>
<span data-ttu-id="a15d8-124">API: N returnerar upptäckta språk och ett numeriskt värde mellan 0 och 1.</span><span class="sxs-lookup"><span data-stu-id="a15d8-124">The API returns the detected language and a numeric score between 0 & 1.</span></span> <span data-ttu-id="a15d8-125">1 poäng visar att korrekt språk har identifierats med 100 procents säkerhet.</span><span class="sxs-lookup"><span data-stu-id="a15d8-125">Scores close to 1 indicate 100% certainty that the identified language is true.</span></span> <span data-ttu-id="a15d8-126">120 språk stöds totalt.</span><span class="sxs-lookup"><span data-stu-id="a15d8-126">A total of 120 languages are supported.</span></span>

## <a name="topic-detection"></a><span data-ttu-id="a15d8-127">Ämnesspårning</span><span class="sxs-lookup"><span data-stu-id="a15d8-127">Topic detection</span></span>
<span data-ttu-id="a15d8-128">Detta är en nyligen utgivna API som returnerar upp upptäckte en lista över avsnitt skickats textposter.</span><span class="sxs-lookup"><span data-stu-id="a15d8-128">This is a newly released API which returns the top detected topics for a list of submitted text records.</span></span> <span data-ttu-id="a15d8-129">Ett ämne identifieras med ett diskussionsämne, som kan bestå av ett eller flera närliggande ord.</span><span class="sxs-lookup"><span data-stu-id="a15d8-129">A topic is identified with a key phrase, which can be one or more related words.</span></span> <span data-ttu-id="a15d8-130">API:t kräver att minst 100 textposter skickas, men är utformat för att identifiera ämnen i hundratusentals poster.</span><span class="sxs-lookup"><span data-stu-id="a15d8-130">This API requires a minimum of 100 text records to be submitted, but is designed to detect topics across hundreds to thousands of records.</span></span> <span data-ttu-id="a15d8-131">Observera att det här API:t debiteras 1 transaktion per textpost som skickas.</span><span class="sxs-lookup"><span data-stu-id="a15d8-131">Note that this API charges 1 transaction per text record submitted.</span></span> <span data-ttu-id="a15d8-132">API: et är avsedd att fungera bra för korta, mänsklig texten som omdömen och feedback från användare.</span><span class="sxs-lookup"><span data-stu-id="a15d8-132">The API is designed to work well for short, human written text such as reviews and user feedback.</span></span>

- - -
## <a name="api-definition"></a><span data-ttu-id="a15d8-133">API-Definition</span><span class="sxs-lookup"><span data-stu-id="a15d8-133">API Definition</span></span>
### <a name="headers"></a><span data-ttu-id="a15d8-134">Rubriker</span><span class="sxs-lookup"><span data-stu-id="a15d8-134">Headers</span></span>
<span data-ttu-id="a15d8-135">Se till att du inkluderar rätt huvuden i din begäran som ska vara följande:</span><span class="sxs-lookup"><span data-stu-id="a15d8-135">Ensure that you include the correct headers in your request, which should be as follows:</span></span>

    Authorization: Basic <creds>
    Accept: application/json

    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

<span data-ttu-id="a15d8-136">Du kan hitta din kontonyckel från ditt konto i den [Azure Data marknaden](https://datamarket.azure.com/account/keys).</span><span class="sxs-lookup"><span data-stu-id="a15d8-136">You can find your account key from your account in the [Azure Data Market](https://datamarket.azure.com/account/keys).</span></span> <span data-ttu-id="a15d8-137">Observera att för närvarande endast JSON accepteras för inkommande och utgående format.</span><span class="sxs-lookup"><span data-stu-id="a15d8-137">Note that currently only JSON is accepted for input and output formats.</span></span> <span data-ttu-id="a15d8-138">XML stöds inte.</span><span class="sxs-lookup"><span data-stu-id="a15d8-138">XML is not supported.</span></span>

- - -
## <a name="single-response-apis"></a><span data-ttu-id="a15d8-139">Enda svar API: er</span><span class="sxs-lookup"><span data-stu-id="a15d8-139">Single Response APIs</span></span>
### <a name="getsentiment"></a><span data-ttu-id="a15d8-140">GetSentiment</span><span class="sxs-lookup"><span data-stu-id="a15d8-140">GetSentiment</span></span>
<span data-ttu-id="a15d8-141">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="a15d8-141">**URL**</span></span>    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

<span data-ttu-id="a15d8-142">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="a15d8-142">**Example request**</span></span>

<span data-ttu-id="a15d8-143">I anropet nedan begär vi sentiment analys för frasen ”Hello World”:</span><span class="sxs-lookup"><span data-stu-id="a15d8-143">In the call below, we are requesting sentiment analysis for the phrase "Hello World":</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

<span data-ttu-id="a15d8-144">Detta returnerar ett svar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a15d8-144">This will return a response as follows:</span></span>

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

- - -
### <a name="getkeyphrases"></a><span data-ttu-id="a15d8-145">GetKeyPhrases</span><span class="sxs-lookup"><span data-stu-id="a15d8-145">GetKeyPhrases</span></span>
<span data-ttu-id="a15d8-146">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="a15d8-146">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

<span data-ttu-id="a15d8-147">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="a15d8-147">**Example request**</span></span>

<span data-ttu-id="a15d8-148">I anropet nedan begär vi viktiga fraser som påträffats i texten ”den har ett fantastiska hotell kan hålla sig, med unika hemdekoration och eget personal”:</span><span class="sxs-lookup"><span data-stu-id="a15d8-148">In the call below, we are requesting the key phrases found in the text "It was a wonderful hotel to stay at, with unique decor and friendly staff":</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

<span data-ttu-id="a15d8-149">Detta returnerar ett svar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a15d8-149">This will return a response as follows:</span></span>

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }

- - -
### <a name="getlanguage"></a><span data-ttu-id="a15d8-150">GetLanguage</span><span class="sxs-lookup"><span data-stu-id="a15d8-150">GetLanguage</span></span>
<span data-ttu-id="a15d8-151">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="a15d8-151">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

<span data-ttu-id="a15d8-152">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="a15d8-152">**Example request**</span></span>

<span data-ttu-id="a15d8-153">I GET anropet nedan, begär vi för sentiment för viktiga fraser i texten *Hello World*</span><span class="sxs-lookup"><span data-stu-id="a15d8-153">In the GET call below, we are requesting for the sentiment for the key phrases in the text *Hello World*</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

<span data-ttu-id="a15d8-154">Detta returnerar ett svar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a15d8-154">This will return a response as follows:</span></span>

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

<span data-ttu-id="a15d8-155">**Valfria parametrar finns:**</span><span class="sxs-lookup"><span data-stu-id="a15d8-155">**Optional parameters**</span></span>

<span data-ttu-id="a15d8-156">`NumberOfLanguagesToDetect`är en valfri parameter.</span><span class="sxs-lookup"><span data-stu-id="a15d8-156">`NumberOfLanguagesToDetect` is an optional parameter.</span></span> <span data-ttu-id="a15d8-157">Standardvärdet är 1.</span><span class="sxs-lookup"><span data-stu-id="a15d8-157">The default is 1.</span></span>

- - -
## <a name="batch-apis"></a><span data-ttu-id="a15d8-158">Batch-API: er</span><span class="sxs-lookup"><span data-stu-id="a15d8-158">Batch APIs</span></span>
<span data-ttu-id="a15d8-159">Text Analytics-tjänsten kan du göra sentiment och nyckeln frasen extractions i batch-läge.</span><span class="sxs-lookup"><span data-stu-id="a15d8-159">The Text Analytics service allows you to do sentiment and key-phrase extractions in batch mode.</span></span> <span data-ttu-id="a15d8-160">Observera att posterna bedömas räknas som en transaktion.</span><span class="sxs-lookup"><span data-stu-id="a15d8-160">Note that each of the records scored counts as one transaction.</span></span> <span data-ttu-id="a15d8-161">Som exempel, om du begär sentiment för 1000 poster i ett enda anrop 1000 transaktioner kommer att dras av.</span><span class="sxs-lookup"><span data-stu-id="a15d8-161">As an example, if you request sentiment for 1000 records in a single call, 1000 transactions will be deducted.</span></span>

<span data-ttu-id="a15d8-162">Observera att de ID som registreras i systemet ID: N som returneras av systemet.</span><span class="sxs-lookup"><span data-stu-id="a15d8-162">Note that the IDs entered into the system are the IDs returned by the system.</span></span> <span data-ttu-id="a15d8-163">Webbtjänsten kontrollerar inte att dessa ID: N är unika.</span><span class="sxs-lookup"><span data-stu-id="a15d8-163">The web service does not check that these IDs are unique.</span></span> <span data-ttu-id="a15d8-164">Ansvarar för att verifiera unikhet anroparen.</span><span class="sxs-lookup"><span data-stu-id="a15d8-164">It is the responsibility of the caller to verify uniqueness.</span></span> 

### <a name="getsentimentbatch"></a><span data-ttu-id="a15d8-165">GetSentimentBatch</span><span class="sxs-lookup"><span data-stu-id="a15d8-165">GetSentimentBatch</span></span>
<span data-ttu-id="a15d8-166">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="a15d8-166">**URL**</span></span>    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

<span data-ttu-id="a15d8-167">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="a15d8-167">**Example request**</span></span>

<span data-ttu-id="a15d8-168">I POST-anropet nedan begär vi för våra av fraser ”Hello World”, ”Hej Foo World” och ”Hello Mina World” i brödtexten för begäran:</span><span class="sxs-lookup"><span data-stu-id="a15d8-168">In the POST call below, we are requesting for the sentiments of the phrases "Hello World", "Hello Foo World" and "Hello My World" in the body of the request:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

<span data-ttu-id="a15d8-169">Begärandetexten:</span><span class="sxs-lookup"><span data-stu-id="a15d8-169">Request body:</span></span>

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

<span data-ttu-id="a15d8-170">I svaret nedan hämta listan med resultat som är associerade med din text-ID: N:</span><span class="sxs-lookup"><span data-stu-id="a15d8-170">In the response below, you get the list of scores associated with your text Ids:</span></span>

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


- - -
### <a name="getkeyphrasesbatch"></a><span data-ttu-id="a15d8-171">GetKeyPhrasesBatch</span><span class="sxs-lookup"><span data-stu-id="a15d8-171">GetKeyPhrasesBatch</span></span>
<span data-ttu-id="a15d8-172">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="a15d8-172">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

<span data-ttu-id="a15d8-173">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="a15d8-173">**Example request**</span></span>

<span data-ttu-id="a15d8-174">I det här exemplet begär vi en lista över våra för viktiga fraser i följande texter:</span><span class="sxs-lookup"><span data-stu-id="a15d8-174">In this example, we are requesting for the list of sentiments for the key phrases in the following texts:</span></span> 

* <span data-ttu-id="a15d8-175">”Den har ett fantastiska hotell kan hålla sig, med unika hemdekoration och eget personal”</span><span class="sxs-lookup"><span data-stu-id="a15d8-175">"It was a wonderful hotel to stay at, with unique decor and friendly staff"</span></span>
* <span data-ttu-id="a15d8-176">”Det var en fantastisk build-konferens med mycket intressanta pratar”</span><span class="sxs-lookup"><span data-stu-id="a15d8-176">"It was an amazing build conference, with very interesting talks"</span></span>
* <span data-ttu-id="a15d8-177">”Trafiken har förskräckliga ut, du har använt tre timmar ska flygplatsen”</span><span class="sxs-lookup"><span data-stu-id="a15d8-177">"The traffic was terrible, I spent three hours going to the airport"</span></span>

<span data-ttu-id="a15d8-178">Den här begäran har gjorts som en POST-anrop till slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="a15d8-178">This request is made as a POST call to the endpoint:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

<span data-ttu-id="a15d8-179">Begärandetexten:</span><span class="sxs-lookup"><span data-stu-id="a15d8-179">Request body:</span></span>

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

<span data-ttu-id="a15d8-180">I svaret nedan hämta listan med viktiga fraser som är associerade med din text-ID: N:</span><span class="sxs-lookup"><span data-stu-id="a15d8-180">In the response below, you get the list of key phrases associated with your text Ids:</span></span>

    { "odata.metadata":"<url>",
         "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

- - -
### <a name="getlanguagebatch"></a><span data-ttu-id="a15d8-181">GetLanguageBatch</span><span class="sxs-lookup"><span data-stu-id="a15d8-181">GetLanguageBatch</span></span>

<span data-ttu-id="a15d8-182">I POST-anropet nedan begär vi språkidentifiering för två text indata:</span><span class="sxs-lookup"><span data-stu-id="a15d8-182">In the POST call below, we are requesting language detection for two text inputs:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

<span data-ttu-id="a15d8-183">Begärandetexten:</span><span class="sxs-lookup"><span data-stu-id="a15d8-183">Request body:</span></span>

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

<span data-ttu-id="a15d8-184">Detta returnerar följande svar där engelska har identifierats på den första indata och franska i andra indata:</span><span class="sxs-lookup"><span data-stu-id="a15d8-184">This returns the following response, where English is detected in the first input and French in the second input:</span></span>

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "English",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "French",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

- - -
## <a name="topic-detection-apis"></a><span data-ttu-id="a15d8-185">Avsnittet-API: er</span><span class="sxs-lookup"><span data-stu-id="a15d8-185">Topic Detection APIs</span></span>
<span data-ttu-id="a15d8-186">Detta är en nyligen utgivna API som returnerar upp upptäckte en lista över avsnitt skickats textposter.</span><span class="sxs-lookup"><span data-stu-id="a15d8-186">This is a newly released API which returns the top detected topics for a list of submitted text records.</span></span> <span data-ttu-id="a15d8-187">Ett ämne identifieras med ett diskussionsämne, som kan bestå av ett eller flera närliggande ord.</span><span class="sxs-lookup"><span data-stu-id="a15d8-187">A topic is identified with a key phrase, which can be one or more related words.</span></span> <span data-ttu-id="a15d8-188">Observera att det här API:t debiteras 1 transaktion per textpost som skickas.</span><span class="sxs-lookup"><span data-stu-id="a15d8-188">Note that this API charges 1 transaction per text record submitted.</span></span>

<span data-ttu-id="a15d8-189">API:t kräver att minst 100 textposter skickas, men är utformat för att identifiera ämnen i hundratusentals poster.</span><span class="sxs-lookup"><span data-stu-id="a15d8-189">This API requires a minimum of 100 text records to be submitted, but is designed to detect topics across hundreds to thousands of records.</span></span>

### <a name="topics--submit-job"></a><span data-ttu-id="a15d8-190">Avsnitt – skicka jobbet</span><span class="sxs-lookup"><span data-stu-id="a15d8-190">Topics – Submit job</span></span>
<span data-ttu-id="a15d8-191">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="a15d8-191">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

<span data-ttu-id="a15d8-192">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="a15d8-192">**Example request**</span></span>

<span data-ttu-id="a15d8-193">I POST anropet nedan begär vi information för en uppsättning 100 artiklar, där de första och sista inkommande artiklarna visas, och två StopPhrases ingår.</span><span class="sxs-lookup"><span data-stu-id="a15d8-193">In the POST call below, we are requesting topics for a set of 100 articles, where the first and last input articles are shown, and two StopPhrases are included.</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

<span data-ttu-id="a15d8-194">Begärandetexten:</span><span class="sxs-lookup"><span data-stu-id="a15d8-194">Request body:</span></span>

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

<span data-ttu-id="a15d8-195">I svaret nedan hämta jobb-ID för det inlämnade projektet:</span><span class="sxs-lookup"><span data-stu-id="a15d8-195">In the response below, you get the JobId for the submitted job:</span></span>

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

<span data-ttu-id="a15d8-196">En lista över ord eller flera word fraser som inte ska returneras som avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a15d8-196">A list of single word or multiple word phrases which should not be returned as topics.</span></span> <span data-ttu-id="a15d8-197">Kan användas för att filtrera ut mycket allmänt avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a15d8-197">Can be used to filter out very generic topics.</span></span> <span data-ttu-id="a15d8-198">Till exempel i en datamängd om hotell granskningar vara ”hotell” och ”hostel” sensible stoppa fraser.</span><span class="sxs-lookup"><span data-stu-id="a15d8-198">For example, in a dataset about hotel reviews, "hotel" and "hostel" may be sensible stop phrases.</span></span>  

### <a name="topics--poll-for-job-results"></a><span data-ttu-id="a15d8-199">Avsnitt – avsökning för jobbet resultat</span><span class="sxs-lookup"><span data-stu-id="a15d8-199">Topics – Poll for job results</span></span>
<span data-ttu-id="a15d8-200">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="a15d8-200">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

<span data-ttu-id="a15d8-201">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="a15d8-201">**Example request**</span></span>

<span data-ttu-id="a15d8-202">Överför jobb-ID som returnerades från överförings-jobbsteg för att hämta resultaten.</span><span class="sxs-lookup"><span data-stu-id="a15d8-202">Pass the JobId returned from the ‘Submit job’ step to fetch the results.</span></span> <span data-ttu-id="a15d8-203">Vi rekommenderar att du anropar den här slutpunkten varje minut tills Status = ”Slutför” i svaret.</span><span class="sxs-lookup"><span data-stu-id="a15d8-203">We recommend that you call this endpoint every minute until Status=’Complete’ in the response.</span></span> <span data-ttu-id="a15d8-204">Det tar cirka 10 minuter för ett jobb att fullständig eller längre för jobb med tusentals poster.</span><span class="sxs-lookup"><span data-stu-id="a15d8-204">It will take around 10 mins for a job to complete, or longer for jobs with many thousands of records.</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


<span data-ttu-id="a15d8-205">Medan den bearbetar blir svaret på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a15d8-205">While it is processing, the response will be as follows:</span></span>

    {
        "odata.metadata":"<url>",
        "Status":"Running",
         "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


<span data-ttu-id="a15d8-206">API: N returnerar utdata i JSON-format i följande format:</span><span class="sxs-lookup"><span data-stu-id="a15d8-206">The API returns output in JSON format in the following format:</span></span>

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
          ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


<span data-ttu-id="a15d8-207">Egenskaperna för varje del av svaret är följande:</span><span class="sxs-lookup"><span data-stu-id="a15d8-207">The properties for each part of the response are as follows:</span></span>

<span data-ttu-id="a15d8-208">**TopicInfo egenskaper**</span><span class="sxs-lookup"><span data-stu-id="a15d8-208">**TopicInfo properties**</span></span>

| <span data-ttu-id="a15d8-209">Nyckel</span><span class="sxs-lookup"><span data-stu-id="a15d8-209">Key</span></span> | <span data-ttu-id="a15d8-210">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a15d8-210">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a15d8-211">Avsnittsklass</span><span class="sxs-lookup"><span data-stu-id="a15d8-211">TopicId</span></span> |<span data-ttu-id="a15d8-212">En unik identifierare för varje avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a15d8-212">A unique identifier for each topic.</span></span> |
| <span data-ttu-id="a15d8-213">Poäng</span><span class="sxs-lookup"><span data-stu-id="a15d8-213">Score</span></span> |<span data-ttu-id="a15d8-214">Antal poster som är tilldelade till avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a15d8-214">Count of records assigned to topic.</span></span> |
| <span data-ttu-id="a15d8-215">KeyPhrase</span><span class="sxs-lookup"><span data-stu-id="a15d8-215">KeyPhrase</span></span> |<span data-ttu-id="a15d8-216">Sammanfattas ord eller fraser i avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a15d8-216">A summarizing word or phrase for the topic.</span></span> <span data-ttu-id="a15d8-217">Kan vara 1 eller flera ord.</span><span class="sxs-lookup"><span data-stu-id="a15d8-217">Can be 1 or multiple words.</span></span> |

<span data-ttu-id="a15d8-218">**TopicAssignment egenskaper**</span><span class="sxs-lookup"><span data-stu-id="a15d8-218">**TopicAssignment properties**</span></span>

| <span data-ttu-id="a15d8-219">Nyckel</span><span class="sxs-lookup"><span data-stu-id="a15d8-219">Key</span></span> | <span data-ttu-id="a15d8-220">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a15d8-220">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a15d8-221">Id</span><span class="sxs-lookup"><span data-stu-id="a15d8-221">Id</span></span> |<span data-ttu-id="a15d8-222">Identifierare för posten.</span><span class="sxs-lookup"><span data-stu-id="a15d8-222">Identifier for the record.</span></span> <span data-ttu-id="a15d8-223">Är lika med det ID som ingår i indata.</span><span class="sxs-lookup"><span data-stu-id="a15d8-223">Equates to the ID included in the input.</span></span> |
| <span data-ttu-id="a15d8-224">Avsnittsklass</span><span class="sxs-lookup"><span data-stu-id="a15d8-224">TopicId</span></span> |<span data-ttu-id="a15d8-225">Ämnes-ID som har tilldelats posten.</span><span class="sxs-lookup"><span data-stu-id="a15d8-225">The topic ID which the record has been assigned to.</span></span> |
| <span data-ttu-id="a15d8-226">Avstånd</span><span class="sxs-lookup"><span data-stu-id="a15d8-226">Distance</span></span> |<span data-ttu-id="a15d8-227">Förtroende att posten tillhör avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a15d8-227">Confidence that the record belongs to the topic.</span></span> <span data-ttu-id="a15d8-228">Avståndet närmare till noll indikerar högre tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="a15d8-228">Distance closer to zero indicates higher confidence.</span></span> |

