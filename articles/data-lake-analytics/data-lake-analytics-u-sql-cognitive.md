---
title: aaaUsing U-SQL kognitiva funktioner i Azure Data Lake Analytics | Microsoft Docs
description: "Lär dig hur toouse hello intelligence kognitiva funktioner i U-SQL"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: sukvg
editor: cgronlun
ms.assetid: 019c1d53-4e61-4cad-9b2c-7a60307cbe19
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: 2c9ac71f490e929070fa0e72b93c3ffdb1ab243b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-get-started-with-hello-cognitive-capabilities-of-u-sql"></a><span data-ttu-id="7d296-103">Självstudier: Kom igång med hello kognitiva funktionerna i U-SQL</span><span class="sxs-lookup"><span data-stu-id="7d296-103">Tutorial: Get started with hello Cognitive capabilities of U-SQL</span></span>

<span data-ttu-id="7d296-104">Kognitiva kan för U-SQL en utvecklare toouse placera intelligence i sina program för stordata.</span><span class="sxs-lookup"><span data-stu-id="7d296-104">Cognitive capabilities for U-SQL enable developers toouse put intelligence in their big data programs.</span></span> <span data-ttu-id="7d296-105">Hej övergripande processen i enkel:</span><span class="sxs-lookup"><span data-stu-id="7d296-105">hello overall process in simple:</span></span>

* <span data-ttu-id="7d296-106">Använd hello REFERENSSAMMANSÄTTNING instruktionen tooenable hello kognitiva funktionerna för hello U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="7d296-106">Use hello REFERENCE ASSEMBLY statement tooenable hello cognitive features for hello U-SQL Script</span></span>
* <span data-ttu-id="7d296-107">Anropa hello processen åtgärden toouse hello kognitiva funktioner</span><span class="sxs-lookup"><span data-stu-id="7d296-107">Call hello PROCESS operation toouse hello Cognitive capabilities</span></span> 

## <a name="imaging-scenarios"></a><span data-ttu-id="7d296-108">Scenarier för avbildning</span><span class="sxs-lookup"><span data-stu-id="7d296-108">Imaging scenarios</span></span>

### <a name="example-image-tagging"></a><span data-ttu-id="7d296-109">Exempel: Avbildningen taggning</span><span class="sxs-lookup"><span data-stu-id="7d296-109">Example: Image tagging</span></span>

<span data-ttu-id="7d296-110">hello som följande exempel visar en slutpunkt till slutpunkt använder hello imaging funktioner toodetect objekt i bilder.</span><span class="sxs-lookup"><span data-stu-id="7d296-110">hello following example shows an end-to-end use of hello imaging capabilities toodetect objects in images.</span></span>

    REFERENCE ASSEMBLY ImageCommon;
    REFERENCE ASSEMBLY FaceSdk;
    REFERENCE ASSEMBLY ImageEmotion;
    REFERENCE ASSEMBLY ImageTagging;
    REFERENCE ASSEMBLY ImageOcr;

    @imgs =
        EXTRACT FileName string, ImgData byte[]
        FROM @"/images/{FileName:*}.jpg"
        USING new Cognition.Vision.ImageExtractor();

    // Extract hello number of objects on each image and tag them 
    @objects =
        PROCESS @imgs 
        PRODUCE FileName,
                NumObjects int,
                Tags string
        READONLY FileName
        USING new Cognition.Vision.ImageTagger();


### <a name="extract-emotions-from-human-faces"></a><span data-ttu-id="7d296-111">Extrahera emotikoner från mänsklig ytor</span><span class="sxs-lookup"><span data-stu-id="7d296-111">Extract emotions from human faces</span></span> 

    @emotions =
        PROCESS @imgs
        PRODUCE FileName string,
                NumFaces int,
                Emotion string
        READONLY FileName
        USING new Cognition.Vision.EmotionAnalyzer();

### <a name="estimate-age-and-gender-for-human-faces"></a><span data-ttu-id="7d296-112">Beräkna ålder och kön för mänsklig ytor</span><span class="sxs-lookup"><span data-stu-id="7d296-112">Estimate age and gender for human faces</span></span>

    @faces = 
            PROCESS @imgs
            PRODUCE FileName,
                    NumFaces int,
                    FaceAge string,
                    FaceGender string
            READONLY FileName
            USING new Cognition.Vision.FaceDetector();

### <a name="detect-text-in-images-ocr"></a><span data-ttu-id="7d296-113">Identifiera text i bilder (OCR)</span><span class="sxs-lookup"><span data-stu-id="7d296-113">Detect text in Images (OCR)</span></span>

    @ocrs =
            PROCESS @imgs
            PRODUCE FileName,
                    Text string
            READONLY FileName
            USING new Cognition.Vision.OcrExtractor();

## <a name="text-scenarios"></a><span data-ttu-id="7d296-114">Text-scenarier</span><span class="sxs-lookup"><span data-stu-id="7d296-114">Text scenarios</span></span>

### <a name="input-data"></a><span data-ttu-id="7d296-115">Indata</span><span class="sxs-lookup"><span data-stu-id="7d296-115">Input data</span></span>

<span data-ttu-id="7d296-116">Anta att vi har indata som består av ”War fred” av Leo Tolstoy.</span><span class="sxs-lookup"><span data-stu-id="7d296-116">Assume that we have an input that consists of “War and Peace” by Leo Tolstoy.</span></span>

    REFERENCE ASSEMBLY [TextCommon];
    REFERENCE ASSEMBLY [TextSentiment];
    REFERENCE ASSEMBLY [TextKeyPhrase];

    @WarAndPeace =
        EXTRACT No int,
                Year string,
                Book string,
                Chapter string,
                Text string
        FROM @"/usqlext/samples/cognition/war_and_peace.csv"
        USING Extractors.Csv();

### <a name="extract-key-phrases-for-each-paragraph"></a><span data-ttu-id="7d296-117">Extrahera nyckeln fraser för varje stycke</span><span class="sxs-lookup"><span data-stu-id="7d296-117">Extract key phrases for each paragraph</span></span>

    @keyphrase =
        PROCESS @WarAndPeace
        PRODUCE No,
                Year,
                Book,
                Chapter,
                Text,
                KeyPhrase string
        READONLY No,
                Year,
                Book,
                Chapter,
                Text
        USING new Cognition.Text.KeyPhraseExtractor();

    // Tokenize hello key phrases.
    @kpsplits =
        SELECT No,
            Year,
            Book,
            Chapter,
            Text,
            T.KeyPhrase
        FROM @keyphrase
            CROSS APPLY
                new Cognition.Text.Splitter("KeyPhrase") AS T(KeyPhrase);
    
### <a name="perform-sentiment-analysis-on-each-paragraph"></a><span data-ttu-id="7d296-118">Analysera sentiment i varje stycke</span><span class="sxs-lookup"><span data-stu-id="7d296-118">Perform sentiment analysis on each paragraph</span></span>

    @sentiment =
        PROCESS @WarAndPeace
        PRODUCE No,
                Year,
                Book,
                Chapter,
                Text,
                Sentiment string,
                Conf double
        READONLY No,
                Year,
                Book,
                Chapter,
                Text
        USING new Cognition.Text.SentimentAnalyzer(true);

