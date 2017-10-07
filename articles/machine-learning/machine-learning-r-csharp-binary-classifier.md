---
title: "aaa(deprecated) binära klassificerare - Azure | Microsoft Docs"
description: "(föråldrad) Binär klassificerare"
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 8045038a-9dcf-44b9-a6de-7f1f8e791575
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
ms.openlocfilehash: 0496fcec9952ca243270caf67f55fe191b2dc9f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-binary-classifier"></a>(föråldrad) Binär klassificerare

> [!NOTE]
> hello Microsoft DataMarket dras och detta API är inaktuell. 
> 
> Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Anta att du har en datauppsättning och vill toopredict en binär beroende variabel baserat på hello oberoende variabler. 'Logistic Regression' är en populär statistiska teknik som används för sådana förutsägelser. Hello beroende variabel är här binär eller dichotomous och p är hello sannolikheten för förekomst av hello egenskap av intresse. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Ett enkelt scenario kan vara där en forskare försöker toopredict om potentiella student är sannolikt tooaccept en åtkomstkontroll erbjudande tooa university baserat på informationen (GPA i gymnasiet, family intäkter, fast status för kön). hello förväntade resultatet är hello sannolikheten för hello potentiella student acceptera hello åtkomstkontroll erbjudande. Detta [webbtjänsten](https://datamarket.azure.com/dataset/aml_labs/log_regression) passar hello logistic regression modelldata toohello och utdata hello sannolikhetsvärdet (y) för varje hello observationer i hello data.  

> Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator. Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden. Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst. hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.  
> 
> 

## <a name="consumption-of-web-service"></a>Användning av web service
Den här web service ger hello förutsade värdena för hello beroende variabel baserat på hello oberoende variabler för alla hello observationer. hello webbtjänst hello slutanvändaren tooinput data som en sträng där rader avgränsas med kommatecken (,) och kolumner avgränsade med semikolon (;). hello webbtjänsten förväntar sig 1 rad i taget och förväntar sig hello första kolumnen toobe hello beroende variabel. En exempel-datauppsättning kan se ut så här:

![Exempeldata][1]

Måste ange observationer utan en beroende variabel som ”saknas” för den. hello data indata för hello ovan dataset skulle vara hello följande sträng: ”1; 5; 2,1; 1; 6,0; 5.3; 2.1,0; 5; 5,0; 3; 4,1; 2, 1, NA; 3; 4”. hello utdata hello förutsagt värde för varje hello rader baseras på hello oberoende variabler. 

> Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder. 
> 
> 

Det finns flera olika sätt att konsumera hello service automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx)).

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

I Azure Machine Learning skapas ett nytt tomt experiment skapades från två [köra R-skriptet] [ execute-r-script] moduler hämtas till hello arbetsytan. Den här webbtjänsten körs ett Azure Machine Learning-experiment med en underliggande R-skriptet. Det finns 2 delar toothis experimentera: schemadefinition, och utbildning modellen + bedömningen. hello första modulen definierar hello förväntade strukturen för hello inkommande dataset, där hello första variabeln är hello beroende variabel och hello återstående variabler är oberoende. andra hello-modul passar en allmän logistic regressionsmodell för hello indata.    

![Experiment flöde][2]

#### <a name="module-1"></a>Modul 1:
    #Schema definition  
    data <- data.frame(value = "1;2;3,1;5;6,0;8;9", stringsAsFactors=FALSE) 
    maml.mapOutputPort("data");  

#### <a name="module-2"></a>Modulen 2:
    #GLM modeling   
    data <- maml.mapInputPort(1) # class: data.frame  

    data.split <- strsplit(data[1,1], ",")[[1]] 
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE) 
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE) 
    data.split <- as.data.frame(t(data.split)) data.split <- 
    data.matrix(data.split) 
    data.split <- data.frame(data.split) 

    model <- glm(data.split$V1 ~., family='binomial', data=data.split)  
    out <- data.frame(predict(model,data.split, type="response")) 
    pred1 <- as.data.frame(out) 
    group <- array(1:nrow(pred1)) 
    for (i in 1:nrow(pred1))  
        {
        if(as.numeric(pred1[i,])>0.5) {group[i]=1} 
        else {group[i]=0}
        } 
    pred2 <- as.data.frame(group) 
    maml.mapOutputPort("pred2");  


## <a name="limitations"></a>Begränsningar
Detta är ett väldigt enkelt exempel på en binär klassificering webbtjänst. Som kan ses från hello exempelkod ovan, inga fel som fångas implementeras och hello antas allt är en binär kontinuerlig variabel (inga kategoriska funktioner tillåts), som hello tjänsten endast indata numeriska värden för närvarande hello hello skapades detta webbtjänsten. Dessutom hello-tjänsten hanterar för närvarande begränsad datastorleken, på grund av toohello frågor och svar uppbyggnad hello webbtjänsten anropet och hello faktum att hello modell som passar varje gång hello webbtjänst anropas. 

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-binary-classifier/binary1.png
[2]: ./media/machine-learning-r-csharp-binary-classifier/binary2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

