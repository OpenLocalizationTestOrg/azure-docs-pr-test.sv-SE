---
title: aaa(deprecated) lexikonet baserat Sentiment analys - Azure | Microsoft Docs
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
redirect_document_id: True
ms.openlocfilehash: 1ed7e19441c6a8ad270a0c0f567b4aea588a583e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-lexicon-based-sentiment-analysis"></a>(föråldrad) Lexikonet baserat Sentiment analys

> [!NOTE]
> hello Microsoft DataMarket dras och detta API är inaktuell. 
> 
> Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Hur kan du mäta användarnas åsikter och attityder mot varumärken eller avsnitt i online sociala nätverk såsom Facebook skickar tweets, granskningar osv.? Sentiment analys ger en metod för att analysera frågor.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Det är vanligtvis två metoder för sentiment analys. Med en algoritm för övervakad inlärning och hello andra kan behandlas som oövervakad inlärning. En modell för klassificering bygger vanligtvis på en stor kommenterade Kristi en övervakad inlärning-algoritmen. Dess noggrannhet baserat huvudsakligen på hello kvaliteten på hello anteckningen och vanligtvis hello utbildning processen tar lång tid. Förutom att när det gäller hello algoritmen tooanother domän hello resultatet är vanligtvis inte bra. Jämfört med toosupervised inlärning lexikonet baserad oövervakad inlärning använder en sentiment ordlista som inte kräver att lagra en stora mängder data Kristi och utbildning - vilket gör hello hela processen mycket snabbare. 

Vår [service](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) bygger på hello MPQA viss subjektivitet lexikonet (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), som är en av de vanligaste hello viss subjektivitet lexikon. Det finns 5097 negativa och 2533 positivt ord i MPQA. Och ord kommenteras som starkt eller svaga polaritet. hello hela Kristi är under GNU General Public License. hello-webbtjänsten kan vara tillämpade tooany korta meningar, till exempel tweets och Facebook meddelandena. 

> Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator. Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden. Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst. hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.
> 
> 

## <a name="consumption-of-web-service"></a>Användning av web service
hello indata kan vara text, men hello webbtjänst fungerar bättre med korta meningar. hello-utdata är ett numeriskt värde mellan 1 och 1. Ett värde under 0 anger att hello sentiment hello text är negativt; positivt om högre än 0. hello absolutvärdet för hello resultatet anger hello styrkan hos hello associerade sentiment. 

> Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder. 
> 
> 

Det finns flera olika sätt att konsumera hello service automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/)).

### <a name="starting-c-code-for-web-service-consumption"></a>Startar C#-kod för web service användning:
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



hello-indata är ”idag är en bra dag”. hello-utdata är ”1”, som anger en positiv sentiment som är associerade med hello inkommande meningen. 

## <a name="creation-of-web-service"></a>Skapandet av web service
> Den här webbtjänsten har skapats med Azure Machine Learning. För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml). Nedan visas en skärmbild av hello experiment som skapade hello web service och exempel-kod för varje hello moduler i hello experiment.
> 
> 

I Azure Machine Learning skapas skapades ett nytt tomt experiment från. hello bilden nedan visar hello experiment flödet av lexikonet-baserade sentiment analys. Hej ”sent_dict.csv” filen är hello MPQA viss subjektivitet lexikonet och anges som en av hello tillförseln av [köra R-skriptet][execute-r-script]. En annan indata är en provade granskning från hello Amazon granska datamängden för test, där vi utföra markeringen, column name ändringar, och dela åtgärder. Vi använder en hash-paketet toostore hello viss subjektivitet lexikonet i hello minnet och påskynda hello poäng beräkning processen. hello hela texten kommer tokeniserad av paketet ”tm” och jämförs med hello ord i hello sentiment ordlistan. Slutligen beräknas en poäng genom att lägga till hello vikten av varje subjektiva ord i hello text. 

### <a name="experiment-flow"></a>Experiment flöde:
![Experiment flöde][2]

#### <a name="module-1"></a>Modul 1:
    # Map 1-based optional input ports toovariables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame

# <a name="install-hash-package"></a>Installera Hash-paket
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

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("data.set")



## <a name="limitations"></a>Begränsningar
En ur algoritmen är lexikonet-baserade sentiment analys ett allmänt sentiment analysverktyg som inte kanske presterar bättre än hello klassificeringsmetod för specifika fält. hello negation problem är inte bra behandlas. Vi hårdkoda flera negation ord i vårt program, men ett bättre sätt med hjälp av en negation ordlista och skapa vissa regler. hello webbtjänsten utför bättre på kort och enkelt meningar, till exempel tweets och Facebook inlägg än långa och komplexa meningar, till exempel Amazon granskningar. 

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/


