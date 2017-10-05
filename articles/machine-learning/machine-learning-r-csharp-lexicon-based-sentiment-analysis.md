---
title: "(föråldrad) Lexikonet baserat Sentiment analys - Azure | Microsoft Docs"
description: "(föråldrad) Lexikonet baserat Sentiment analys"
services: machine-learning
documentationcenter: 
author: pengxia
manager: jhubbard
editor: cgronlun
ms.assetid: 912f41af-966c-4d79-a413-6f9fc02823df
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: pengxia
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: 7bc80a1e1067296528eca1a843ea30b0c27af616
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-lexicon-based-sentiment-analysis"></a><span data-ttu-id="486cf-103">(föråldrad) Lexikonet baserat Sentiment analys</span><span class="sxs-lookup"><span data-stu-id="486cf-103">(deprecated) Lexicon Based Sentiment Analysis</span></span>

> [!NOTE]
> <span data-ttu-id="486cf-104">Microsoft DataMarket dras och detta API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="486cf-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="486cf-105">Du hittar många användbara exempel experiment och API: er i den [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="486cf-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="486cf-106">Mer information om galleriet finns [resursen och identifiera resurser i Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="486cf-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="486cf-107">Hur kan du mäta användarnas åsikter och attityder mot varumärken eller avsnitt i online sociala nätverk såsom Facebook skickar tweets, granskningar osv.?</span><span class="sxs-lookup"><span data-stu-id="486cf-107">How can you measure users’ opinions and attitudes toward brands or topics in online social networks, such as Facebook posts, tweets, reviews, etc.?</span></span> <span data-ttu-id="486cf-108">Sentiment analys ger en metod för att analysera frågor.</span><span class="sxs-lookup"><span data-stu-id="486cf-108">Sentiment analysis provides a method for analyzing such questions.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="486cf-109">Det är vanligtvis två metoder för sentiment analys.</span><span class="sxs-lookup"><span data-stu-id="486cf-109">There are generally two methods for sentiment analysis.</span></span> <span data-ttu-id="486cf-110">Med en algoritm för övervakad inlärning och den andra kan behandlas som oövervakad inlärning.</span><span class="sxs-lookup"><span data-stu-id="486cf-110">One is using a supervised learning algorithm, and the other can be treated as unsupervised learning.</span></span> <span data-ttu-id="486cf-111">En modell för klassificering bygger vanligtvis på en stor kommenterade Kristi en övervakad inlärning-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="486cf-111">A supervised learning algorithm generally builds a classification model on a large annotated corpus.</span></span> <span data-ttu-id="486cf-112">Dess noggrannhet baserat huvudsakligen på kvaliteten på anteckningen och vanligtvis utbildning processen tar lång tid.</span><span class="sxs-lookup"><span data-stu-id="486cf-112">Its accuracy is mainly based on the quality of the annotation, and usually the training process will take a long time.</span></span> <span data-ttu-id="486cf-113">Förutom att när vi använder algoritmen till en annan domän är resultatet vanligtvis inte bra.</span><span class="sxs-lookup"><span data-stu-id="486cf-113">Besides that, when we apply the algorithm to another domain, the result is usually not good.</span></span> <span data-ttu-id="486cf-114">Jämfört med övervakad lära, lexikonet-baserad oövervakad inlärning använder en sentiment ordlista som inte kräver att lagra en stora mängder data Kristi och utbildning, vilket gör att hela processen snabbare.</span><span class="sxs-lookup"><span data-stu-id="486cf-114">Compared to supervised learning, lexicon-based unsupervised learning uses a sentiment dictionary, which doesn’t require storing a large data corpus and training - which makes the whole process much faster.</span></span> 

<span data-ttu-id="486cf-115">Vår [service](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) bygger på MPQA viss subjektivitet lexikonet (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), som är en av de vanligaste viss subjektivitet lexikon.</span><span class="sxs-lookup"><span data-stu-id="486cf-115">Our [service](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) is built on the MPQA Subjectivity Lexicon (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), which is one of the most commonly used subjectivity lexicons.</span></span> <span data-ttu-id="486cf-116">Det finns 5097 negativa och 2533 positivt ord i MPQA.</span><span class="sxs-lookup"><span data-stu-id="486cf-116">There are 5097 negative and 2533 positive words in MPQA.</span></span> <span data-ttu-id="486cf-117">Och ord kommenteras som starkt eller svaga polaritet.</span><span class="sxs-lookup"><span data-stu-id="486cf-117">And all of these words are annotated as strong or weak polarity.</span></span> <span data-ttu-id="486cf-118">Hela Kristi är under GNU General Public License.</span><span class="sxs-lookup"><span data-stu-id="486cf-118">The whole corpus is under GNU General Public License.</span></span> <span data-ttu-id="486cf-119">Webbtjänsten kan tillämpas på alla korta meningar, till exempel tweets och Facebook meddelandena.</span><span class="sxs-lookup"><span data-stu-id="486cf-119">The web service can be applied to any short sentences, such as tweets and Facebook posts.</span></span> 

> <span data-ttu-id="486cf-120">Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="486cf-120">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer for example.</span></span> <span data-ttu-id="486cf-121">Men syftet med webbtjänsten är också som fungerar som ett exempel på hur Azure Machine Learning kan användas för att skapa webbtjänster ovanpå R-koden.</span><span class="sxs-lookup"><span data-stu-id="486cf-121">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="486cf-122">Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="486cf-122">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="486cf-123">Webbtjänsten kan sedan publiceras på Azure Marketplace och används av användare och enheter över hela världen med inga infrastrukturinställningar av författaren till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="486cf-123">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="486cf-124">Användning av web service</span><span class="sxs-lookup"><span data-stu-id="486cf-124">Consumption of web service</span></span>
<span data-ttu-id="486cf-125">Indata kan vara text, men webbtjänsten fungerar bättre med korta meningar.</span><span class="sxs-lookup"><span data-stu-id="486cf-125">The input data can be any text, but the web service works better with short sentences.</span></span> <span data-ttu-id="486cf-126">Utdata är ett numeriskt värde mellan 1 och 1.</span><span class="sxs-lookup"><span data-stu-id="486cf-126">The output is a numeric value between -1 and 1.</span></span> <span data-ttu-id="486cf-127">Ett värde under 0 anger att sentiment text är negativt; positivt om högre än 0.</span><span class="sxs-lookup"><span data-stu-id="486cf-127">Any value below 0 denotes that the sentiment of the text is negative; positive if above 0.</span></span> <span data-ttu-id="486cf-128">Det absoluta värdet för resultatet anger styrkan hos den associerade sentiment.</span><span class="sxs-lookup"><span data-stu-id="486cf-128">The absolute value of the result denotes the strength of the associated sentiment.</span></span> 

> <span data-ttu-id="486cf-129">Den här tjänsten, är som finns på Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder.</span><span class="sxs-lookup"><span data-stu-id="486cf-129">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="486cf-130">Det finns flera olika sätt att använda tjänsten automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/)).</span><span class="sxs-lookup"><span data-stu-id="486cf-130">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="486cf-131">Startar C#-kod för web service användning:</span><span class="sxs-lookup"><span data-stu-id="486cf-131">Starting C# code for web service consumption:</span></span>
    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();

                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



<span data-ttu-id="486cf-132">Indata är ”idag är en bra dag”.</span><span class="sxs-lookup"><span data-stu-id="486cf-132">The input is “Today is a good day.”</span></span> <span data-ttu-id="486cf-133">Utdata är ”1”, som anger en positiv sentiment som är kopplade till den inkommande meningen.</span><span class="sxs-lookup"><span data-stu-id="486cf-133">The output is “1”, which indicates a positive sentiment associated with the input sentence.</span></span> 

## <a name="creation-of-web-service"></a><span data-ttu-id="486cf-134">Skapandet av web service</span><span class="sxs-lookup"><span data-stu-id="486cf-134">Creation of web service</span></span>
> <span data-ttu-id="486cf-135">Den här webbtjänsten har skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="486cf-135">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="486cf-136">För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="486cf-136">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="486cf-137">Nedan visas en skärmbild av experiment som skapade web service och exempel koden för alla moduler i experimentet.</span><span class="sxs-lookup"><span data-stu-id="486cf-137">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="486cf-138">I Azure Machine Learning skapas skapades ett nytt tomt experiment från.</span><span class="sxs-lookup"><span data-stu-id="486cf-138">From within Azure Machine Learning, a new blank experiment was created.</span></span> <span data-ttu-id="486cf-139">Bilden nedan visar experiment flödet av lexikonet-baserade sentiment analys.</span><span class="sxs-lookup"><span data-stu-id="486cf-139">The figure below shows the experiment flow of lexicon-based sentiment analysis.</span></span> <span data-ttu-id="486cf-140">Filen ”sent_dict.csv” är MPQA viss subjektivitet lexikonet och anges som en indata av [köra R-skriptet][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="486cf-140">The “sent_dict.csv” file is the MPQA subjectivity lexicon, and is set as one of the inputs of [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="486cf-141">En annan indata är en provade granskning från Amazon granska datamängden för test, där vi utföra markeringen, column name ändringar, och dela åtgärder.</span><span class="sxs-lookup"><span data-stu-id="486cf-141">Another input is a sampled review from the Amazon review dataset for test, where we performed selection, column name modification, and split operations.</span></span> <span data-ttu-id="486cf-142">Vi använder ett hash-paket för att lagra viss subjektivitet lexikonet i minnet och skynda på processen poäng beräkning.</span><span class="sxs-lookup"><span data-stu-id="486cf-142">We use a hash package to store the subjectivity lexicon in the memory and accelerate the score computation process.</span></span> <span data-ttu-id="486cf-143">Hela texten kommer tokeniserad av paketet ”tm” och jämförs med ordet i ordlistan sentiment.</span><span class="sxs-lookup"><span data-stu-id="486cf-143">The whole text will be tokenized by “tm” package and compared with the word in the sentiment dictionary.</span></span> <span data-ttu-id="486cf-144">Slutligen beräknas en poäng genom att lägga till vikten för varje subjektiva ord i texten.</span><span class="sxs-lookup"><span data-stu-id="486cf-144">Finally, a score will be calculated by adding the weight of each subjective word in the text.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="486cf-145">Experiment flöde:</span><span class="sxs-lookup"><span data-stu-id="486cf-145">Experiment flow:</span></span>
![Experiment flöde][2]

#### <a name="module-1"></a><span data-ttu-id="486cf-147">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="486cf-147">Module 1:</span></span>
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame

# <a name="install-hash-package"></a><span data-ttu-id="486cf-148">Installera Hash-paket</span><span class="sxs-lookup"><span data-stu-id="486cf-148">Install hash package</span></span>
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }

        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")



## <a name="limitations"></a><span data-ttu-id="486cf-149">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="486cf-149">Limitations</span></span>
<span data-ttu-id="486cf-150">En ur algoritmen är lexikonet-baserade sentiment analys ett allmänt sentiment analysverktyg som inte kanske presterar bättre än klassificeringsmetod för specifika fält.</span><span class="sxs-lookup"><span data-stu-id="486cf-150">From an algorithm perspective, lexicon-based sentiment analysis is a general sentiment analysis tool, which may not perform better than the classification method for specific fields.</span></span> <span data-ttu-id="486cf-151">Problemet negation är inte bra behandlas.</span><span class="sxs-lookup"><span data-stu-id="486cf-151">The negation problem is not well dealt with.</span></span> <span data-ttu-id="486cf-152">Vi hårdkoda flera negation ord i vårt program, men ett bättre sätt med hjälp av en negation ordlista och skapa vissa regler.</span><span class="sxs-lookup"><span data-stu-id="486cf-152">We hardcode several negation words in our program, but a better way is using a negation dictionary and build some rules.</span></span> <span data-ttu-id="486cf-153">Webbtjänsten utför bättre på kort och enkelt meningar, till exempel tweets och Facebook inlägg än långa och komplexa meningar, till exempel Amazon granskningar.</span><span class="sxs-lookup"><span data-stu-id="486cf-153">The web service performs better on short and simple sentences, such as tweets and Facebook posts, than on long and complex sentences such as Amazon reviews.</span></span> 

## <a name="faq"></a><span data-ttu-id="486cf-154">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="486cf-154">FAQ</span></span>
<span data-ttu-id="486cf-155">Vanliga frågor om förbrukningen av webbtjänst eller publicering på Azure Marketplace finns [här](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="486cf-155">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/


