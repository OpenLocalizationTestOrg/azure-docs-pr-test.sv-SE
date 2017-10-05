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
# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>API:er för Machine Learning: Textanalys för attityder, Extrahering av diskussionsämne, Språkidentifiering och Ämnesidentifiering
> [!NOTE]
> Den här guiden är för version 1 av API: et. För version 2, [ **finns i det här dokumentet**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). Version 2 är nu den önskade versionen av detta API.
> 
> 

## <a name="overview"></a>Översikt
API: et för Text Analytics är en uppsättning textanalys [webbtjänster](https://datamarket.azure.com/dataset/amla/text-analytics) skapats med Azure Machine Learning. API: et kan användas för att analysera Ostrukturerade text för åtgärder som sentiment analys, viktiga frasen extrahering, identifiera språk och avsnittet identifiering. Några övningsdata behövs för att använda den här API: bara ge dina textdata. Detta API använder avancerade naturligt språk bearbetning av tekniker för att leverera bäst i klassen förutsägelser.

Du kan se textanalys i praktiken på vår [demonstrationswebbplats](https://text-analytics-demo.azurewebsites.net/), där du hittar även [exempel](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) för hur du implementerar textanalys i C# och Python.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

- - -
## <a name="sentiment-analysis"></a>Känsloanalys
API: et returnerar ett numeriskt värde mellan 0 och 1. Poäng nära 1 anger positivt sentiment medan poäng nära 0 indikerar negativa sentiment. Känslopoängen genereras med klassificeringstekniker. Indata till klassificeraren bland annat n-g, funktioner som skapas från en del av tal taggar och word inbäddningar. Engelska är för närvarande det enda språket som stöds.

## <a name="key-phrase-extraction"></a>Extraktion av nyckelfraser
API:t returnerar en lista med strängar som anger samtalsämnen i den inmatade texten. Vi använder tekniker från de avancerade verktygen för språkbearbetning i Microsoft Office. Engelska är för närvarande det enda språket som stöds.

## <a name="language-detection"></a>Språkspårning
API: N returnerar upptäckta språk och ett numeriskt värde mellan 0 och 1. 1 poäng visar att korrekt språk har identifierats med 100 procents säkerhet. 120 språk stöds totalt.

## <a name="topic-detection"></a>Ämnesspårning
Detta är en nyligen utgivna API som returnerar upp upptäckte en lista över avsnitt skickats textposter. Ett ämne identifieras med ett diskussionsämne, som kan bestå av ett eller flera närliggande ord. API:t kräver att minst 100 textposter skickas, men är utformat för att identifiera ämnen i hundratusentals poster. Observera att det här API:t debiteras 1 transaktion per textpost som skickas. API: et är avsedd att fungera bra för korta, mänsklig texten som omdömen och feedback från användare.

- - -
## <a name="api-definition"></a>API-Definition
### <a name="headers"></a>Rubriker
Se till att du inkluderar rätt huvuden i din begäran som ska vara följande:

    Authorization: Basic <creds>
    Accept: application/json

    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Du kan hitta din kontonyckel från ditt konto i den [Azure Data marknaden](https://datamarket.azure.com/account/keys). Observera att för närvarande endast JSON accepteras för inkommande och utgående format. XML stöds inte.

- - -
## <a name="single-response-apis"></a>Enda svar API: er
### <a name="getsentiment"></a>GetSentiment
**URL: EN**    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Exempelbegäran**

I anropet nedan begär vi sentiment analys för frasen ”Hello World”:

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

Detta returnerar ett svar på följande sätt:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

- - -
### <a name="getkeyphrases"></a>GetKeyPhrases
**URL: EN**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Exempelbegäran**

I anropet nedan begär vi viktiga fraser som påträffats i texten ”den har ett fantastiska hotell kan hålla sig, med unika hemdekoration och eget personal”:

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

Detta returnerar ett svar på följande sätt:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }

- - -
### <a name="getlanguage"></a>GetLanguage
**URL: EN**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Exempelbegäran**

I GET anropet nedan, begär vi för sentiment för viktiga fraser i texten *Hello World*

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

Detta returnerar ett svar på följande sätt:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Valfria parametrar finns:**

`NumberOfLanguagesToDetect`är en valfri parameter. Standardvärdet är 1.

- - -
## <a name="batch-apis"></a>Batch-API: er
Text Analytics-tjänsten kan du göra sentiment och nyckeln frasen extractions i batch-läge. Observera att posterna bedömas räknas som en transaktion. Som exempel, om du begär sentiment för 1000 poster i ett enda anrop 1000 transaktioner kommer att dras av.

Observera att de ID som registreras i systemet ID: N som returneras av systemet. Webbtjänsten kontrollerar inte att dessa ID: N är unika. Ansvarar för att verifiera unikhet anroparen. 

### <a name="getsentimentbatch"></a>GetSentimentBatch
**URL: EN**    

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Exempelbegäran**

I POST-anropet nedan begär vi för våra av fraser ”Hello World”, ”Hej Foo World” och ”Hello Mina World” i brödtexten för begäran:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

Begärandetexten:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

I svaret nedan hämta listan med resultat som är associerade med din text-ID: N:

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
### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch
**URL: EN**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Exempelbegäran**

I det här exemplet begär vi en lista över våra för viktiga fraser i följande texter: 

* ”Den har ett fantastiska hotell kan hålla sig, med unika hemdekoration och eget personal”
* ”Det var en fantastisk build-konferens med mycket intressanta pratar”
* ”Trafiken har förskräckliga ut, du har använt tre timmar ska flygplatsen”

Den här begäran har gjorts som en POST-anrop till slutpunkten:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

Begärandetexten:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

I svaret nedan hämta listan med viktiga fraser som är associerade med din text-ID: N:

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
### <a name="getlanguagebatch"></a>GetLanguageBatch

I POST-anropet nedan begär vi språkidentifiering för två text indata:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Begärandetexten:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

Detta returnerar följande svar där engelska har identifierats på den första indata och franska i andra indata:

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
## <a name="topic-detection-apis"></a>Avsnittet-API: er
Detta är en nyligen utgivna API som returnerar upp upptäckte en lista över avsnitt skickats textposter. Ett ämne identifieras med ett diskussionsämne, som kan bestå av ett eller flera närliggande ord. Observera att det här API:t debiteras 1 transaktion per textpost som skickas.

API:t kräver att minst 100 textposter skickas, men är utformat för att identifiera ämnen i hundratusentals poster.

### <a name="topics--submit-job"></a>Avsnitt – skicka jobbet
**URL: EN**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Exempelbegäran**

I POST anropet nedan begär vi information för en uppsättning 100 artiklar, där de första och sista inkommande artiklarna visas, och två StopPhrases ingår.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

Begärandetexten:

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

I svaret nedan hämta jobb-ID för det inlämnade projektet:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

En lista över ord eller flera word fraser som inte ska returneras som avsnitt. Kan användas för att filtrera ut mycket allmänt avsnitt. Till exempel i en datamängd om hotell granskningar vara ”hotell” och ”hostel” sensible stoppa fraser.  

### <a name="topics--poll-for-job-results"></a>Avsnitt – avsökning för jobbet resultat
**URL: EN**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Exempelbegäran**

Överför jobb-ID som returnerades från överförings-jobbsteg för att hämta resultaten. Vi rekommenderar att du anropar den här slutpunkten varje minut tills Status = ”Slutför” i svaret. Det tar cirka 10 minuter för ett jobb att fullständig eller längre för jobb med tusentals poster.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Medan den bearbetar blir svaret på följande sätt:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
         "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


API: N returnerar utdata i JSON-format i följande format:

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


Egenskaperna för varje del av svaret är följande:

**TopicInfo egenskaper**

| Nyckel | Beskrivning |
|:--- |:--- |
| Avsnittsklass |En unik identifierare för varje avsnitt. |
| Poäng |Antal poster som är tilldelade till avsnittet. |
| KeyPhrase |Sammanfattas ord eller fraser i avsnittet. Kan vara 1 eller flera ord. |

**TopicAssignment egenskaper**

| Nyckel | Beskrivning |
|:--- |:--- |
| Id |Identifierare för posten. Är lika med det ID som ingår i indata. |
| Avsnittsklass |Ämnes-ID som har tilldelats posten. |
| Avstånd |Förtroende att posten tillhör avsnittet. Avståndet närmare till noll indikerar högre tillförlitlighet. |

