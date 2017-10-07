---
title: aaa(deprecated) klustermodellen - Azure | Microsoft Docs
description: "(föråldrad) Ett kluster"
services: machine-learning
documentationcenter: 
author: FrancescaLazzeri
manager: jhubbard
editor: cgronlun
ms.assetid: 51b8d012-ed44-4312-920c-9c808dfd4ff6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: lazzeri
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 7b2dffb855a8d91114752b579115e97d07210e45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-cluster-model"></a>(föråldrad) Ett kluster

> [!NOTE]
> hello Microsoft DataMarket dras och detta API är inaktuell. 
> 
> Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Hur kan vi för att förutsäga grupper av kredit cardholders beteenden i ordning tooreduce hello kostnad av risken för kreditkort utfärdare? Hur kan vi definiera grupper för person egenskaper för anställda i ordning tooimprove deras prestanda på jobbet? Hur kan läkare klassificera patienter i grupper baserat på deras sjukdomar hello egenskaper? I princip besvaras alla frågor via klustret analys.   

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Klustret analys klassificerar en uppsättning observationer i två eller flera ömsesidigt uteslutande okänd grupper baserat på kombinationer av variabler. hello syftar klustret analys toodiscover ett system för att organisera observationer, vanligtvis personer eller deras egenskaper i grupper där medlemmar i grupper hello har egenskaper gemensamt. Detta [service](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) använder hello K-Means metoder, en vanliga klustring teknik toocluster godtyckliga data i grupper. Den här webbtjänsten tar hello data och hello antalet k kluster som indata och producerar förutsägelser som hello k grupper toowhich varje observationer tillhör. 

> Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator. Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden. Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst. hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.  
> 
> 

## <a name="consumption-of-web-service"></a>Användning av web service
Den här webbtjänsten grupperas hello data i en uppsättning k grupper och utdata hello grupptilldelning för varje rad. hello webbtjänst hello slutanvändaren tooinput data som en sträng där rader avgränsas med kommatecken (,) och kolumner avgränsade med semikolon (;). hello webbtjänst 1 rad i taget. En exempel-datauppsättning kan se ut så här:

![Exempeldata][1]

Anta att hello användaren vill tooseparate dessa data i 3 ömsesidigt uteslutande grupper. Hej indata hello ovan dataset skulle bli hello följande: value = ”10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4”; k = ”3”. hello-utdata är hello förväntade gruppmedlemskap för varje hello rader.

> Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder. 
> 
> 

Det finns flera olika sätt att konsumera hello service automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>Startar C#-kod för web service användning:
    public class Input
    {
            public string value;
            public string k;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




## <a name="creation-of-web-service"></a>Skapandet av web service
> Den här webbtjänsten har skapats med Azure Machine Learning. För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml). Nedan visas en skärmbild av hello experiment som skapade hello web service och exempel-kod för varje hello moduler i hello experiment.
> 
> 

I Azure Machine Learning skapas ett nytt tomt experiment skapades från två [köra R-skriptet] [ execute-r-script] moduler hämtas till hello arbetsytan. hello dataschemat skapades med en enkel [köra R-skriptet][execute-r-script]. Sedan hello dataschemat var länkade toohello kluster modellen avsnittet igen skapats med en [köra R-skriptet][execute-r-script]. I hello [köra R-skriptet] [ execute-r-script] används för hello klustermodellen hello webbtjänsten sedan använder funktionen hello ”k-means”, som är färdiga i hello [köra R-skriptet] [ execute-r-script] av Azure Machine Learning.    

![Experiment flöde][3]

#### <a name="module-1"></a>Modul 1:
    #Enter hello input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)

    maml.mapOutputPort("mydata");     


#### <a name="module-2"></a>Modulen 2:
    # Map 1-based optional input ports toovariables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("mydatafinal");


## <a name="limitations"></a>Begränsningar
Detta är ett väldigt enkelt exempel på en kluster-webbtjänst. Som kan ses från hello exempelkod ovan, inga fel som fångas implementeras och hello antas allt är en kontinuerlig variabel (inga kategoriska funktioner tillåts), som hello tjänsten endast indata numeriska värden för närvarande hello hello skapas för den här webbplatsen tjänsten. Dessutom hello-tjänsten hanterar för närvarande begränsad datastorleken, på grund av toohello frågor och svar uppbyggnad hello webbtjänsten anropet och hello faktum att hello modell som passar varje gång hello webbtjänst anropas. 

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

