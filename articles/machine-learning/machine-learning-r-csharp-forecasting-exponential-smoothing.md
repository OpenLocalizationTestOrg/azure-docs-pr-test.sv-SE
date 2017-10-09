---
title: "aaa(deprecated) Prognosticering - exponentiell utjämning - Azure | Microsoft Docs"
description: "(föråldrad) Webbtjänsten: prognoser exponentiell utjämning"
services: machine-learning
documentationcenter: 
author: yijichen
manager: jhubbard
editor: cgronlun
ms.assetid: a4150681-6eac-4145-9eca-0cdf60781dde
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: yijichen
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: ebc732d3a47943405b0cb26a373f529a50de9005
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-forecasting---exponential-smoothing"></a>(föråldrad) Prognosticering - exponentiell utjämning

> [!NOTE]
> hello Microsoft DataMarket dras och detta API är inaktuell. 
> 
> Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Detta [webbtjänsten](https://datamarket.azure.com/dataset/aml_labs/ets) implementerar hello exponentiell utjämning modellen (ETS) tooproduce förutsägelser baserat på hello historiska data som hello användaren. Kommer hello begäran för en specifik produkt ökad år? Kan jag förutsäga min produktförsäljning för hello jul säsongen så att jag kan effektivt kunna planera min inventering? Prognosmodeller är lgh tooaddress sådana frågor. Angivna hello tidigare data, Undersök dessa modeller dolda trender och säsongsvärdet toopredict framtida trender.  

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator. Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden. Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst. hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.
> 
> 

## <a name="consumption-of-web-service"></a>Användning av web service
Den här tjänsten accepterar 4 argument och beräknar hello ETS prognoser.
hello-indataargument är:

* Frekvens - anger hello frekvensen av hello rådata (varje dag/vecka/månad/varje kvartal/år).
* Horizon - framtiden prognos tidsram.
* Datum – Lägg till i hello nya tidsserier data för tid.
* Värde: lägga till i hello nya serie data tidsvärden.

hello utdata från hello-tjänsten är hello beräknade värden.

Exempelindata kan vara: 

* Frekvens - 12
* Horizon - 12
* Datum - 15/1/2012, 2012-2-15, 2012-3-15; 4-15-2012; 2012-5-15; 6/15/2012; 15/7/2012, 8 / 15/2012, 9-15-2012, 10/15/2012; 11-15-2012, 2012-12/15; 2013-1/15; 2013-2/15; 2013-3/15; 2013-4/15; 2013-5/15; 2013-6/15; 2013-7/15; 8 / 15/2013; 2013-9/15; 2013/10/15; 2013-11/15; 2013-12/15; 2014-1-15, 2014-2-15, 2014-3-15; 2014-4-15; 2014-5-15; 2014-6-15; 2014-7-15, 8 / 2014-15; 2014-9-15
* Värde: 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509

> Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder. 
> 
> 

Det finns flera olika sätt att konsumera hello service automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/etsForecasting.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>Startar C#-kod för web service användning:
    public class Input
    {
            public string frequency;
            public string horizon;
            public string date;
            public string value;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };
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

I Azure Machine Learning skapas skapades ett nytt tomt experiment från. Inkommande exempeldata överfördes med en fördefinierad dataschemat. Länkade toohello dataschemat är en [köra R-skriptet] [ execute-r-script] modul som genererar hello ETS prognoser modellen med hjälp av 'ets' och 'prognos' funktioner från R. 

![Experiment flöde][2]

#### <a name="module-1"></a>Modul 1:
    # Add in hello CSV file with hello data in hello format shown below 
![Data, exempel][3]    

#### <a name="module-2"></a>Modulen 2:
    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)

    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]

    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)

    # Fit a time-series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- ets(train_ts)
    train_model <- forecast(fit1, h = data$horizon)
    plot(train_model)

    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")

    # Data output
    maml.mapOutputPort("data.forecast");


## <a name="limitations"></a>Begränsningar
Detta är ett väldigt enkelt exempel för ETS Prognosticering. Som kan ses från hello exempelkod ovan, inga fel som fångas implementeras och hello service förutsätter att alla hello-variabler är kontinuerlig positiva värden och hello frekvensen måste vara ett heltal som är större än 1. hello tidslängd hello datum och värdet angreppsmetoderna bör hello samma. hello datum variabeln bör följa toohello format 'mm/dd/åååå'.

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img1.png
[2]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img2.png
[3]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

