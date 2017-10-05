---
title: "(föråldrad) Modell - Azure-kluster | Microsoft Docs"
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
redirect_document_id: TRUE
ms.openlocfilehash: 7cbbbd6d4236dab638eb3051595a584557480841
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-cluster-model"></a>(föråldrad) Ett kluster

> [!NOTE]
> Microsoft DataMarket dras och detta API är inaktuell. 
> 
> Du hittar många användbara exempel experiment och API: er i den [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Mer information om galleriet finns [resursen och identifiera resurser i Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Hur kan vi för att förutsäga grupper av kredit cardholders beteenden för att minska risken av kostnad för kreditkort utfärdare? Hur kan vi definiera grupper av person egenskaper för anställda för att kunna förbättra deras prestanda på jobbet? Hur kan läkare klassificera patienter i grupper baserat på deras sjukdomar egenskaper? I princip besvaras alla frågor via klustret analys.   

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Klustret analys klassificerar en uppsättning observationer i två eller flera ömsesidigt uteslutande okänd grupper baserat på kombinationer av variabler. Syftet med klustret analys är att identifiera ett system för att organisera observationer, vanligtvis personer eller deras egenskaper i grupper där medlemmar i grupperna har egenskaper gemensamt. Detta [service](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) använder K-Means metoder, en vanliga klustring teknik till valfri klusterdata i grupper. Den här webbtjänsten hämtar data och antalet k kluster som indata och producerar förutsägelser som k-grupper som varje observationer tillhör. 

> Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator. Men syftet med webbtjänsten är också som fungerar som ett exempel på hur Azure Machine Learning kan användas för att skapa webbtjänster ovanpå R-koden. Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst. Webbtjänsten kan sedan publiceras på Azure Marketplace och används av användare och enheter över hela världen med inga infrastrukturinställningar av författaren till webbtjänsten.  
> 
> 

## <a name="consumption-of-web-service"></a>Användning av web service
Den här webbtjänsten grupperas data till en uppsättning k grupper och matar ut grupptilldelning för varje rad. Webbtjänst att användaren måste ange data som en sträng där rader avgränsas med kommatecken (,) och kolumner avgränsade med semikolon (;). Webbtjänst 1 rad i taget. En exempel-datauppsättning kan se ut så här:

![Exempeldata][1]

Anta att du vill dela informationen i 3 ömsesidigt uteslutande grupper. Indata för ovan datauppsättningen skulle vara följande: value = ”10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4”; k = ”3”. Utdata är förväntade gruppmedlemskap för var och en av raderna.

> Den här tjänsten, är som finns på Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder. 
> 
> 

Det finns flera olika sätt att använda tjänsten automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).

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
> Den här webbtjänsten har skapats med Azure Machine Learning. För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml). Nedan visas en skärmbild av experiment som skapade web service och exempel koden för alla moduler i experimentet.
> 
> 

I Azure Machine Learning skapas ett nytt tomt experiment skapades från två [köra R-skriptet] [ execute-r-script] moduler hämtas till arbetsytan. Dataschemat har skapats med en enkel [köra R-skriptet][execute-r-script]. Sedan dataschemat var kopplad till avsnittet cluster modellen skapas igen med en [köra R-skriptet][execute-r-script]. I den [köra R-skriptet] [ execute-r-script] används för klustret modellen webbtjänsten sedan använder funktionen ”k-means”, som är färdiga i den [köra R-skriptet] [ execute-r-script] av Azure Machine Learning.    

![Experiment flöde][3]

#### <a name="module-1"></a>Modul 1:
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)

    maml.mapOutputPort("mydata");     


#### <a name="module-2"></a>Modulen 2:
    # Map 1-based optional input ports to variables
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

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");


## <a name="limitations"></a>Begränsningar
Detta är ett väldigt enkelt exempel på en kluster-webbtjänst. Som kan ses från exempelkoden ovan, inga fel som fångas implementeras och tjänsten förutsätter att allt är en kontinuerlig variabel (inga kategoriska funktioner tillåts), som tjänsten endast indata numeriska värden vid tidpunkten för skapandet av den här webbtjänsten. Dessutom hanterar tjänsten för närvarande begränsad datastorleken förfallodatum för frågor och svar art webbtjänstanropet och det faktum att modellen som passar varje gång som webbtjänsten har anropats. 

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
Vanliga frågor om förbrukningen av webbtjänst eller publicering på Azure Marketplace finns [här](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

