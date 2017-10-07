---
title: "aaa(deprecated) Multivariate linjär Regression - Azure | Microsoft Docs"
description: "(föråldrad) Multivariate linjär Regression"
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 2fb78220-ced9-4564-a439-9e5df6772994
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: jaymathe
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 0ff7221cd06c0ef059b0c5bf327016588174dcfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-multivariate-linear-regression"></a>(föråldrad) Multivariate linjär Regression

> [!NOTE]
> hello Microsoft DataMarket dras och detta API är inaktuell. 
> 
> Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Anta att du har en datauppsättning och skulle som tooquickly förutsäga beroende-variabeln (y) för varje enskild person (i) baserat på oberoende variabler. Linjär regression är en populär statistiska teknik som används för sådana förutsägelser. Här antas hello beroende variabel y toobe ett kontinuerligt värde.  

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Ett enkelt scenario kan vara där hello forskare försöker toopredict hello vikten för en enskild (y) baserat på deras höjd (x). Ett mer avancerat scenario kan vara där hello forskare innehåller ytterligare information för hello enskilda (till exempel vikt, kön, RAS) och försöker toopredict hello vikten av hello person. Detta [webbtjänsten](https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) passar hello linjär regression modelldata toohello och utdata hello förutsägelsevärdet (y) för varje hello observationer i hello data.

> Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator. Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden. Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst. hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.  
> 
> 

## <a name="consumption-of-web-service"></a>Användning av web service
Den här web service ger hello förutsade värdena för hello beroende variabel baserat på hello oberoende variabler för alla hello observationer. hello webbtjänst hello slutanvändaren tooinput data som en sträng där rader avgränsas med kommatecken (,) och kolumner avgränsade med semikolon (;). hello webbtjänsten förväntar sig 1 rad i taget och förväntar sig hello första kolumnen toobe hello beroende variabel. En exempel-datauppsättning kan se ut så här:

![Exempeldata][1]

Måste ange observationer utan en beroende variabel som ”saknas” för den. hello data indata för hello ovan dataset skulle vara hello följande sträng ”: 10, 5, 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2, 1, NA, 3, 4”. hello utdata hello förutsagt värde för varje hello rader baseras på hello oberoende variabler. 

> Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder. 
> 
> 

Det finns flera olika sätt att konsumera hello service automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>Startar C#-kod för web service användning:
    public class Input
    {
            public string value;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
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

I Azure Machine Learning skapas ett nytt tomt experiment skapades från två [köra R-skriptet] [ execute-r-script] moduler som hämtades till hello arbetsytan. Den här webbtjänsten körs ett Azure Machine Learning-experiment med en underliggande R-skriptet. Det finns 2 delar toothis experimentera: schemadefinition, och utbildning modellen + bedömningen. hello första modulen definierar hello förväntade strukturen för hello inkommande dataset, där hello första variabeln är hello beroende variabel och hello återstående variabler är oberoende. andra hello-modul passar en allmän linjär regressionsmodell för hello indata.  

![Experiment flöde][3]

#### <a name="module-1"></a>Modul 1:
#### <a name="schema-definition"></a>Schemadefinitionen
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

#### <a name="module-2"></a>Modulen 2:
#### <a name="lm-modeling"></a>LM-modellering
    data <- maml.mapInputPort(1) # class: data.frame  

    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  

## <a name="limitations"></a>Begränsningar
Detta är ett väldigt enkelt exempel på en flera linjär regression webbtjänst. Som kan ses från hello exempelkod ovan, inga fel som fångas implementeras och hello antas allt är en kontinuerlig variabel (inga kategoriska funktioner tillåts), som hello tjänsten endast indata numeriska värden för närvarande hello hello skapas för den här webbplatsen tjänsten. Dessutom hello-tjänsten hanterar för närvarande begränsad datastorleken, på grund av toohello frågor och svar uppbyggnad hello webbtjänsten anropet och hello faktum att hello modell som passar varje gång hello webbtjänst anropas. 

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

