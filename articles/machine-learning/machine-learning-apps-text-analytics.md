---
title: 'Machine Learning API: er: Textanalys | Microsoft Docs'
description: "Microsofts Machine Learning Text Analytics API kan använda tooanalyze Ostrukturerade text för sentiment analys, viktiga frasen extrahering, identifiera språk och avsnittet identifiering."
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
redirect_document_id: True
ms.openlocfilehash: 49380c83849c5d5fdd8dce4f3899ebcb3d6870f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a><span data-ttu-id="627a5-103">API:er för Machine Learning: Textanalys för attityder, Extrahering av diskussionsämne, Språkidentifiering och Ämnesidentifiering</span><span class="sxs-lookup"><span data-stu-id="627a5-103">Machine Learning APIs: Text Analytics for Sentiment, Key Phrase Extraction, Language Detection and Topic Detection</span></span>
> [!NOTE]
> <span data-ttu-id="627a5-104">Den här guiden är för version 1 av hello API.</span><span class="sxs-lookup"><span data-stu-id="627a5-104">This guide is for version 1 of hello API.</span></span> <span data-ttu-id="627a5-105">För version 2, [ **finns toothis dokument**](../cognitive-services/cognitive-services-text-analytics-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="627a5-105">For version 2, [**refer toothis document**](../cognitive-services/cognitive-services-text-analytics-quick-start.md).</span></span> <span data-ttu-id="627a5-106">Hello önskade versionen för detta API är nu version 2.</span><span class="sxs-lookup"><span data-stu-id="627a5-106">Version 2 is now hello preferred version of this API.</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="627a5-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="627a5-107">Overview</span></span>
<span data-ttu-id="627a5-108">hello Text Analytics API är en uppsättning textanalys [webbtjänster](https://datamarket.azure.com/dataset/amla/text-analytics) skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="627a5-108">hello Text Analytics API is a suite of text analytics [web services](https://datamarket.azure.com/dataset/amla/text-analytics) built with Azure Machine Learning.</span></span> <span data-ttu-id="627a5-109">hello API kan använda tooanalyze Ostrukturerade text för åtgärder som sentiment analys, viktiga frasen extrahering, identifiera språk och avsnittet identifiering.</span><span class="sxs-lookup"><span data-stu-id="627a5-109">hello API can be used tooanalyze unstructured text for tasks such as sentiment analysis, key phrase extraction, language detection and topic detection.</span></span> <span data-ttu-id="627a5-110">Inga data är utbildning behövs toouse detta API: bara ge dina textdata.</span><span class="sxs-lookup"><span data-stu-id="627a5-110">No training data is needed toouse this API: just bring your text data.</span></span> <span data-ttu-id="627a5-111">Detta API använder avancerade naturligt språk bearbetas tekniker toodeliver bäst i klassen förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="627a5-111">This API uses advanced natural language processing techniques toodeliver best in class predictions.</span></span>

<span data-ttu-id="627a5-112">Du kan se textanalys i praktiken på vår [demonstrationswebbplats](https://text-analytics-demo.azurewebsites.net/), där du hittar även [exempel](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) om hur tooimplement textanalys i C# och Python.</span><span class="sxs-lookup"><span data-stu-id="627a5-112">You can see text analytics in action on our [demo site](https://text-analytics-demo.azurewebsites.net/), where you will also find [samples](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) on how tooimplement text analytics in C# and Python.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

- - -
## <a name="sentiment-analysis"></a><span data-ttu-id="627a5-113">Känsloanalys</span><span class="sxs-lookup"><span data-stu-id="627a5-113">Sentiment analysis</span></span>
<span data-ttu-id="627a5-114">hello API returnerar ett numeriskt värde mellan 0 och 1.</span><span class="sxs-lookup"><span data-stu-id="627a5-114">hello API returns a numeric score between 0 & 1.</span></span> <span data-ttu-id="627a5-115">Poäng Stäng too1 ange positivt sentiment medan poäng Stäng too0 Ange negativt sentiment.</span><span class="sxs-lookup"><span data-stu-id="627a5-115">Scores close too1 indicate positive sentiment, while scores close too0 indicate negative sentiment.</span></span> <span data-ttu-id="627a5-116">Känslopoängen genereras med klassificeringstekniker.</span><span class="sxs-lookup"><span data-stu-id="627a5-116">Sentiment score is generated using classification techniques.</span></span> <span data-ttu-id="627a5-117">hello inkommande funktioner toohello klassificerare innehåller funktioner som skapas från en del av tal taggar och word inbäddningar n-gram.</span><span class="sxs-lookup"><span data-stu-id="627a5-117">hello input features toohello classifier include n-grams, features generated from part-of-speech tags, and word embeddings.</span></span> <span data-ttu-id="627a5-118">Engelska är för närvarande hello stöds bara språk.</span><span class="sxs-lookup"><span data-stu-id="627a5-118">Currently, English is hello only supported language.</span></span>

## <a name="key-phrase-extraction"></a><span data-ttu-id="627a5-119">Extraktion av nyckelfraser</span><span class="sxs-lookup"><span data-stu-id="627a5-119">Key phrase extraction</span></span>
<span data-ttu-id="627a5-120">hello API returnerar en lista med strängar som anger hello prata huvudpunkter i hello-indata.</span><span class="sxs-lookup"><span data-stu-id="627a5-120">hello API returns a list of strings denoting hello key talking points in hello input text.</span></span> <span data-ttu-id="627a5-121">Vi använder tekniker från de avancerade verktygen för språkbearbetning i Microsoft Office.</span><span class="sxs-lookup"><span data-stu-id="627a5-121">We employ techniques from Microsoft Office's sophisticated Natural Language Processing toolkit.</span></span> <span data-ttu-id="627a5-122">Engelska är för närvarande hello stöds bara språk.</span><span class="sxs-lookup"><span data-stu-id="627a5-122">Currently, English is hello only supported language.</span></span>

## <a name="language-detection"></a><span data-ttu-id="627a5-123">Språkspårning</span><span class="sxs-lookup"><span data-stu-id="627a5-123">Language detection</span></span>
<span data-ttu-id="627a5-124">hello API returnerar hello upptäckte språk och ett numeriskt värde mellan 0 och 1.</span><span class="sxs-lookup"><span data-stu-id="627a5-124">hello API returns hello detected language and a numeric score between 0 & 1.</span></span> <span data-ttu-id="627a5-125">Poäng Stäng too1 ange 100% säkerhet att hello identifieras språket är true.</span><span class="sxs-lookup"><span data-stu-id="627a5-125">Scores close too1 indicate 100% certainty that hello identified language is true.</span></span> <span data-ttu-id="627a5-126">120 språk stöds totalt.</span><span class="sxs-lookup"><span data-stu-id="627a5-126">A total of 120 languages are supported.</span></span>

## <a name="topic-detection"></a><span data-ttu-id="627a5-127">Ämnesspårning</span><span class="sxs-lookup"><span data-stu-id="627a5-127">Topic detection</span></span>
<span data-ttu-id="627a5-128">Detta är en nyligen utgivna API som returnerar hello översta identifierade avsnitt om en lista över textposter som skickats.</span><span class="sxs-lookup"><span data-stu-id="627a5-128">This is a newly released API which returns hello top detected topics for a list of submitted text records.</span></span> <span data-ttu-id="627a5-129">Ett ämne identifieras med ett diskussionsämne, som kan bestå av ett eller flera närliggande ord.</span><span class="sxs-lookup"><span data-stu-id="627a5-129">A topic is identified with a key phrase, which can be one or more related words.</span></span> <span data-ttu-id="627a5-130">Detta API kräver minst 100 text registrerar toobe har skickats, men är utformad toodetect avsnitt i hundratals toothousands av poster.</span><span class="sxs-lookup"><span data-stu-id="627a5-130">This API requires a minimum of 100 text records toobe submitted, but is designed toodetect topics across hundreds toothousands of records.</span></span> <span data-ttu-id="627a5-131">Observera att det här API:t debiteras 1 transaktion per textpost som skickas.</span><span class="sxs-lookup"><span data-stu-id="627a5-131">Note that this API charges 1 transaction per text record submitted.</span></span> <span data-ttu-id="627a5-132">hello API är utformad toowork bra för kort, mänskliga skrivs text, till exempel omdömen och feedback från användare.</span><span class="sxs-lookup"><span data-stu-id="627a5-132">hello API is designed toowork well for short, human written text such as reviews and user feedback.</span></span>

- - -
## <a name="api-definition"></a><span data-ttu-id="627a5-133">API-Definition</span><span class="sxs-lookup"><span data-stu-id="627a5-133">API Definition</span></span>
### <a name="headers"></a><span data-ttu-id="627a5-134">Rubriker</span><span class="sxs-lookup"><span data-stu-id="627a5-134">Headers</span></span>
<span data-ttu-id="627a5-135">Se till att du inkluderar rätt hello-huvuden i din begäran som ska vara följande:</span><span class="sxs-lookup"><span data-stu-id="627a5-135">Ensure that you include hello correct headers in your request, which should be as follows:</span></span>

    Authorization: Basic <creds>
    Accept: application/json

    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

<span data-ttu-id="627a5-136">Du hittar din kontonyckel från ditt konto i hello [Azure Data marknaden](https://datamarket.azure.com/account/keys).</span><span class="sxs-lookup"><span data-stu-id="627a5-136">You can find your account key from your account in hello [Azure Data Market](https://datamarket.azure.com/account/keys).</span></span> <span data-ttu-id="627a5-137">Observera att för närvarande endast JSON accepteras för inkommande och utgående format.</span><span class="sxs-lookup"><span data-stu-id="627a5-137">Note that currently only JSON is accepted for input and output formats.</span></span> <span data-ttu-id="627a5-138">XML stöds inte.</span><span class="sxs-lookup"><span data-stu-id="627a5-138">XML is not supported.</span></span>

- - -
## <a name="single-response-apis"></a><span data-ttu-id="627a5-139">Enda svar API: er</span><span class="sxs-lookup"><span data-stu-id="627a5-139">Single Response APIs</span></span>
### <a name="getsentiment"></a><span data-ttu-id="627a5-140">GetSentiment</span><span class="sxs-lookup"><span data-stu-id="627a5-140">GetSentiment</span></span>
<span data-ttu-id="627a5-141">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="627a5-141">**URL**</span></span>    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

<span data-ttu-id="627a5-142">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="627a5-142">**Example request**</span></span>

<span data-ttu-id="627a5-143">Hello anrop nedan begär vi sentiment analys för hello frasen ”Hello World”:</span><span class="sxs-lookup"><span data-stu-id="627a5-143">In hello call below, we are requesting sentiment analysis for hello phrase "Hello World":</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

<span data-ttu-id="627a5-144">Detta returnerar ett svar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="627a5-144">This will return a response as follows:</span></span>

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

- - -
### <a name="getkeyphrases"></a><span data-ttu-id="627a5-145">GetKeyPhrases</span><span class="sxs-lookup"><span data-stu-id="627a5-145">GetKeyPhrases</span></span>
<span data-ttu-id="627a5-146">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="627a5-146">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

<span data-ttu-id="627a5-147">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="627a5-147">**Example request**</span></span>

<span data-ttu-id="627a5-148">Hello anrop nedan begär vi hello viktiga fraser som påträffats i hello texten ”det var en fantastiska hotell toostay, med unika hemdekoration och eget personal”:</span><span class="sxs-lookup"><span data-stu-id="627a5-148">In hello call below, we are requesting hello key phrases found in hello text "It was a wonderful hotel toostay at, with unique decor and friendly staff":</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

<span data-ttu-id="627a5-149">Detta returnerar ett svar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="627a5-149">This will return a response as follows:</span></span>

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }

- - -
### <a name="getlanguage"></a><span data-ttu-id="627a5-150">GetLanguage</span><span class="sxs-lookup"><span data-stu-id="627a5-150">GetLanguage</span></span>
<span data-ttu-id="627a5-151">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="627a5-151">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

<span data-ttu-id="627a5-152">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="627a5-152">**Example request**</span></span>

<span data-ttu-id="627a5-153">I hello GET-anrop nedan vi begär hello sentiment för hello viktiga fraser i hello text *Hello World*</span><span class="sxs-lookup"><span data-stu-id="627a5-153">In hello GET call below, we are requesting for hello sentiment for hello key phrases in hello text *Hello World*</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

<span data-ttu-id="627a5-154">Detta returnerar ett svar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="627a5-154">This will return a response as follows:</span></span>

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

<span data-ttu-id="627a5-155">**Valfria parametrar finns:**</span><span class="sxs-lookup"><span data-stu-id="627a5-155">**Optional parameters**</span></span>

<span data-ttu-id="627a5-156">`NumberOfLanguagesToDetect`är en valfri parameter.</span><span class="sxs-lookup"><span data-stu-id="627a5-156">`NumberOfLanguagesToDetect` is an optional parameter.</span></span> <span data-ttu-id="627a5-157">hello standardvärdet är 1.</span><span class="sxs-lookup"><span data-stu-id="627a5-157">hello default is 1.</span></span>

- - -
## <a name="batch-apis"></a><span data-ttu-id="627a5-158">Batch-API: er</span><span class="sxs-lookup"><span data-stu-id="627a5-158">Batch APIs</span></span>
<span data-ttu-id="627a5-159">hello Text Analytics-tjänsten tillåter toodo sentiment och nyckeln frasen extractions i batch-läge.</span><span class="sxs-lookup"><span data-stu-id="627a5-159">hello Text Analytics service allows you toodo sentiment and key-phrase extractions in batch mode.</span></span> <span data-ttu-id="627a5-160">Observera att varje hello-poster ger räknas som en transaktion.</span><span class="sxs-lookup"><span data-stu-id="627a5-160">Note that each of hello records scored counts as one transaction.</span></span> <span data-ttu-id="627a5-161">Som exempel, om du begär sentiment för 1000 poster i ett enda anrop 1000 transaktioner kommer att dras av.</span><span class="sxs-lookup"><span data-stu-id="627a5-161">As an example, if you request sentiment for 1000 records in a single call, 1000 transactions will be deducted.</span></span>

<span data-ttu-id="627a5-162">Observera att hello-ID som angetts i hello system är hello-ID: N som returneras av hello system.</span><span class="sxs-lookup"><span data-stu-id="627a5-162">Note that hello IDs entered into hello system are hello IDs returned by hello system.</span></span> <span data-ttu-id="627a5-163">hello webbtjänsten kontrollerar inte att dessa ID: N är unika.</span><span class="sxs-lookup"><span data-stu-id="627a5-163">hello web service does not check that these IDs are unique.</span></span> <span data-ttu-id="627a5-164">Det är hello ansvar hello anroparen tooverify unikhet.</span><span class="sxs-lookup"><span data-stu-id="627a5-164">It is hello responsibility of hello caller tooverify uniqueness.</span></span> 

### <a name="getsentimentbatch"></a><span data-ttu-id="627a5-165">GetSentimentBatch</span><span class="sxs-lookup"><span data-stu-id="627a5-165">GetSentimentBatch</span></span>
<span data-ttu-id="627a5-166">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="627a5-166">**URL**</span></span>    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

<span data-ttu-id="627a5-167">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="627a5-167">**Example request**</span></span>

<span data-ttu-id="627a5-168">Anropa nedan, vi begär för hello våra hello fraser ”Hello World”, ”Foo hälsningsmeddelande” och ”Hello Mina World” i hello brödtext hello begäran i hello POST:</span><span class="sxs-lookup"><span data-stu-id="627a5-168">In hello POST call below, we are requesting for hello sentiments of hello phrases "Hello World", "Hello Foo World" and "Hello My World" in hello body of hello request:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

<span data-ttu-id="627a5-169">Begärandetexten:</span><span class="sxs-lookup"><span data-stu-id="627a5-169">Request body:</span></span>

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

<span data-ttu-id="627a5-170">Hello svar nedan får du hello lista med resultat som är associerade med din text-ID: N:</span><span class="sxs-lookup"><span data-stu-id="627a5-170">In hello response below, you get hello list of scores associated with your text Ids:</span></span>

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
### <a name="getkeyphrasesbatch"></a><span data-ttu-id="627a5-171">GetKeyPhrasesBatch</span><span class="sxs-lookup"><span data-stu-id="627a5-171">GetKeyPhrasesBatch</span></span>
<span data-ttu-id="627a5-172">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="627a5-172">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

<span data-ttu-id="627a5-173">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="627a5-173">**Example request**</span></span>

<span data-ttu-id="627a5-174">I det här exemplet begär vi hello lista över våra för hello viktiga fraser i hello följande text:</span><span class="sxs-lookup"><span data-stu-id="627a5-174">In this example, we are requesting for hello list of sentiments for hello key phrases in hello following texts:</span></span> 

* <span data-ttu-id="627a5-175">”Det var en fantastiska hotell toostay, med unika hemdekoration och eget personal”</span><span class="sxs-lookup"><span data-stu-id="627a5-175">"It was a wonderful hotel toostay at, with unique decor and friendly staff"</span></span>
* <span data-ttu-id="627a5-176">”Det var en fantastisk build-konferens med mycket intressanta pratar”</span><span class="sxs-lookup"><span data-stu-id="627a5-176">"It was an amazing build conference, with very interesting talks"</span></span>
* <span data-ttu-id="627a5-177">”Hej trafik har förskräckliga ut, du har använt tre timmar ska toohello flygplats”</span><span class="sxs-lookup"><span data-stu-id="627a5-177">"hello traffic was terrible, I spent three hours going toohello airport"</span></span>

<span data-ttu-id="627a5-178">Den här begäran har gjorts som en POST anropet toohello slutpunkt:</span><span class="sxs-lookup"><span data-stu-id="627a5-178">This request is made as a POST call toohello endpoint:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

<span data-ttu-id="627a5-179">Begärandetexten:</span><span class="sxs-lookup"><span data-stu-id="627a5-179">Request body:</span></span>

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel toostay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"hello traffic was terrible, I spent three hours going toohello airport"}
    ]}

<span data-ttu-id="627a5-180">Hello svar nedan får du hello lista över viktiga fraser som är associerade med din text-ID: N:</span><span class="sxs-lookup"><span data-stu-id="627a5-180">In hello response below, you get hello list of key phrases associated with your text Ids:</span></span>

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
### <a name="getlanguagebatch"></a><span data-ttu-id="627a5-181">GetLanguageBatch</span><span class="sxs-lookup"><span data-stu-id="627a5-181">GetLanguageBatch</span></span>

<span data-ttu-id="627a5-182">Hello efter anrop nedan begär vi identifiera språk för två text indata:</span><span class="sxs-lookup"><span data-stu-id="627a5-182">In hello POST call below, we are requesting language detection for two text inputs:</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

<span data-ttu-id="627a5-183">Begärandetexten:</span><span class="sxs-lookup"><span data-stu-id="627a5-183">Request body:</span></span>

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

<span data-ttu-id="627a5-184">Detta returnerar hello efter svar, där engelska har identifierats på hello första indata och franska i hello andra indata:</span><span class="sxs-lookup"><span data-stu-id="627a5-184">This returns hello following response, where English is detected in hello first input and French in hello second input:</span></span>

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
## <a name="topic-detection-apis"></a><span data-ttu-id="627a5-185">Avsnittet-API: er</span><span class="sxs-lookup"><span data-stu-id="627a5-185">Topic Detection APIs</span></span>
<span data-ttu-id="627a5-186">Detta är en nyligen utgivna API som returnerar hello översta identifierade avsnitt om en lista över textposter som skickats.</span><span class="sxs-lookup"><span data-stu-id="627a5-186">This is a newly released API which returns hello top detected topics for a list of submitted text records.</span></span> <span data-ttu-id="627a5-187">Ett ämne identifieras med ett diskussionsämne, som kan bestå av ett eller flera närliggande ord.</span><span class="sxs-lookup"><span data-stu-id="627a5-187">A topic is identified with a key phrase, which can be one or more related words.</span></span> <span data-ttu-id="627a5-188">Observera att det här API:t debiteras 1 transaktion per textpost som skickas.</span><span class="sxs-lookup"><span data-stu-id="627a5-188">Note that this API charges 1 transaction per text record submitted.</span></span>

<span data-ttu-id="627a5-189">Detta API kräver minst 100 text registrerar toobe har skickats, men är utformad toodetect avsnitt i hundratals toothousands av poster.</span><span class="sxs-lookup"><span data-stu-id="627a5-189">This API requires a minimum of 100 text records toobe submitted, but is designed toodetect topics across hundreds toothousands of records.</span></span>

### <a name="topics--submit-job"></a><span data-ttu-id="627a5-190">Avsnitt – skicka jobbet</span><span class="sxs-lookup"><span data-stu-id="627a5-190">Topics – Submit job</span></span>
<span data-ttu-id="627a5-191">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="627a5-191">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

<span data-ttu-id="627a5-192">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="627a5-192">**Example request**</span></span>

<span data-ttu-id="627a5-193">Hello efter anrop nedan begär vi information för en uppsättning 100 artiklar, där hello- och indata artiklar visas, och två StopPhrases ingår.</span><span class="sxs-lookup"><span data-stu-id="627a5-193">In hello POST call below, we are requesting topics for a set of 100 articles, where hello first and last input articles are shown, and two StopPhrases are included.</span></span>

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

<span data-ttu-id="627a5-194">Begärandetexten:</span><span class="sxs-lookup"><span data-stu-id="627a5-194">Request body:</span></span>

    {"Inputs":[
        {"Id":"1","Text":"I loved hello food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated hello decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

<span data-ttu-id="627a5-195">Hello svar nedan hämta hello JobId för hello skickat jobbet:</span><span class="sxs-lookup"><span data-stu-id="627a5-195">In hello response below, you get hello JobId for hello submitted job:</span></span>

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

<span data-ttu-id="627a5-196">En lista över ord eller flera word fraser som inte ska returneras som avsnitt.</span><span class="sxs-lookup"><span data-stu-id="627a5-196">A list of single word or multiple word phrases which should not be returned as topics.</span></span> <span data-ttu-id="627a5-197">Kan vara används toofilter ut mycket allmänt avsnitt.</span><span class="sxs-lookup"><span data-stu-id="627a5-197">Can be used toofilter out very generic topics.</span></span> <span data-ttu-id="627a5-198">Till exempel i en datamängd om hotell granskningar vara ”hotell” och ”hostel” sensible stoppa fraser.</span><span class="sxs-lookup"><span data-stu-id="627a5-198">For example, in a dataset about hotel reviews, "hotel" and "hostel" may be sensible stop phrases.</span></span>  

### <a name="topics--poll-for-job-results"></a><span data-ttu-id="627a5-199">Avsnitt – avsökning för jobbet resultat</span><span class="sxs-lookup"><span data-stu-id="627a5-199">Topics – Poll for job results</span></span>
<span data-ttu-id="627a5-200">**URL: EN**</span><span class="sxs-lookup"><span data-stu-id="627a5-200">**URL**</span></span>

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

<span data-ttu-id="627a5-201">**Exempelbegäran**</span><span class="sxs-lookup"><span data-stu-id="627a5-201">**Example request**</span></span>

<span data-ttu-id="627a5-202">Skicka hello JobId returnerades från hello överförings-jobbet steg toofetch hello resultat.</span><span class="sxs-lookup"><span data-stu-id="627a5-202">Pass hello JobId returned from hello ‘Submit job’ step toofetch hello results.</span></span> <span data-ttu-id="627a5-203">Vi rekommenderar att du anropar den här slutpunkten varje minut tills Status = 'Slutför' hello svar.</span><span class="sxs-lookup"><span data-stu-id="627a5-203">We recommend that you call this endpoint every minute until Status=’Complete’ in hello response.</span></span> <span data-ttu-id="627a5-204">Det tar cirka 10 minuter för ett jobb toocomplete eller längre för jobb med tusentals poster.</span><span class="sxs-lookup"><span data-stu-id="627a5-204">It will take around 10 mins for a job toocomplete, or longer for jobs with many thousands of records.</span></span>

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


<span data-ttu-id="627a5-205">Medan den bearbetar blir hello svar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="627a5-205">While it is processing, hello response will be as follows:</span></span>

    {
        "odata.metadata":"<url>",
        "Status":"Running",
         "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


<span data-ttu-id="627a5-206">hello API returnerar utdata i JSON-format i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="627a5-206">hello API returns output in JSON format in hello following format:</span></span>

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


<span data-ttu-id="627a5-207">hello egenskaper för varje del av hello svar är följande:</span><span class="sxs-lookup"><span data-stu-id="627a5-207">hello properties for each part of hello response are as follows:</span></span>

<span data-ttu-id="627a5-208">**TopicInfo egenskaper**</span><span class="sxs-lookup"><span data-stu-id="627a5-208">**TopicInfo properties**</span></span>

| <span data-ttu-id="627a5-209">Nyckel</span><span class="sxs-lookup"><span data-stu-id="627a5-209">Key</span></span> | <span data-ttu-id="627a5-210">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="627a5-210">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="627a5-211">Avsnittsklass</span><span class="sxs-lookup"><span data-stu-id="627a5-211">TopicId</span></span> |<span data-ttu-id="627a5-212">En unik identifierare för varje avsnitt.</span><span class="sxs-lookup"><span data-stu-id="627a5-212">A unique identifier for each topic.</span></span> |
| <span data-ttu-id="627a5-213">Poäng</span><span class="sxs-lookup"><span data-stu-id="627a5-213">Score</span></span> |<span data-ttu-id="627a5-214">Antal poster tilldelade tootopic.</span><span class="sxs-lookup"><span data-stu-id="627a5-214">Count of records assigned tootopic.</span></span> |
| <span data-ttu-id="627a5-215">KeyPhrase</span><span class="sxs-lookup"><span data-stu-id="627a5-215">KeyPhrase</span></span> |<span data-ttu-id="627a5-216">Sammanfattas ord eller fraser för hello ämnet.</span><span class="sxs-lookup"><span data-stu-id="627a5-216">A summarizing word or phrase for hello topic.</span></span> <span data-ttu-id="627a5-217">Kan vara 1 eller flera ord.</span><span class="sxs-lookup"><span data-stu-id="627a5-217">Can be 1 or multiple words.</span></span> |

<span data-ttu-id="627a5-218">**TopicAssignment egenskaper**</span><span class="sxs-lookup"><span data-stu-id="627a5-218">**TopicAssignment properties**</span></span>

| <span data-ttu-id="627a5-219">Nyckel</span><span class="sxs-lookup"><span data-stu-id="627a5-219">Key</span></span> | <span data-ttu-id="627a5-220">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="627a5-220">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="627a5-221">Id</span><span class="sxs-lookup"><span data-stu-id="627a5-221">Id</span></span> |<span data-ttu-id="627a5-222">Identifierare för hello-post.</span><span class="sxs-lookup"><span data-stu-id="627a5-222">Identifier for hello record.</span></span> <span data-ttu-id="627a5-223">Är lika toohello ID ingår i hello indata.</span><span class="sxs-lookup"><span data-stu-id="627a5-223">Equates toohello ID included in hello input.</span></span> |
| <span data-ttu-id="627a5-224">Avsnittsklass</span><span class="sxs-lookup"><span data-stu-id="627a5-224">TopicId</span></span> |<span data-ttu-id="627a5-225">hello ämnes-ID som hello posten har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="627a5-225">hello topic ID which hello record has been assigned to.</span></span> |
| <span data-ttu-id="627a5-226">Avstånd</span><span class="sxs-lookup"><span data-stu-id="627a5-226">Distance</span></span> |<span data-ttu-id="627a5-227">Förtroende som hello posten tillhör toohello avsnittet.</span><span class="sxs-lookup"><span data-stu-id="627a5-227">Confidence that hello record belongs toohello topic.</span></span> <span data-ttu-id="627a5-228">Avståndet närmare toozero anger högre tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="627a5-228">Distance closer toozero indicates higher confidence.</span></span> |

