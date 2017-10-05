---
title: "(föråldrad) Prognoser: Autoregressive integrerad glidande medelvärde (ARIMA) - Azure | Microsoft Docs"
description: "(föråldrad) Prognosticering - Autoregressive integrerad glidande medelvärde (ARIMA)"
services: machine-learning
documentationcenter: 
author: yijichen
manager: jhubbard
editor: cgronlun
ms.assetid: 1e0d525f-8a9e-4b42-87e0-c9423f059f8c
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: yijichen
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: 6be76618c8ce5917f8fdfdea851c3ca65f9fddd4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-forecasting---autoregressive-integrated-moving-average-arima"></a>(föråldrad) Prognosticering - Autoregressive integrerad glidande medelvärde (ARIMA)

> [!NOTE]
> Microsoft DataMarket dras och detta API är inaktuell. 
> 
> Du hittar många användbara exempel experiment och API: er i den [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Mer information om galleriet finns [resursen och identifiera resurser i Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).


Detta [service](https://datamarket.azure.com/dataset/aml_labs/arima) implementerar Autoregressive integrerad glidande medelvärde (ARIMA) för att skapa förutsägelser baserat på historisk data som anges av användaren. Ökar behovet av en viss produkt år? Kan jag förutsäga min produktförsäljning för säsongen jul så att jag kan effektivt kunna planera min inventering? Prognosmodellen modeller är lgh att åtgärda dessa frågor. Senaste data får granska dessa modeller dolda trender och säsongsvärdet att förutsäga framtida trender. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator. Men syftet med webbtjänsten är också som fungerar som ett exempel på hur Azure Machine Learning kan användas för att skapa webbtjänster ovanpå R-koden. Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst. Webbtjänsten kan sedan publiceras på Azure Marketplace och används av användare och enheter över hela världen med inga infrastrukturinställningar av författaren till webbtjänsten.
> 
> 

## <a name="consumption-of-web-service"></a>Användning av web service
Den här tjänsten accepterar 4 argument och beräknar ARIMA prognoser.
De angivna argumenten är:

* Frekvens - anger frekvensen av rådata (varje dag/vecka/månad/varje kvartal/år).
* Horizon - framtiden prognos tidsram.
* Date - lägga till i den nya tid series-data för tid.
* Värde: lägga till i de nya tidsvärdena serie data.

Utdata från tjänsten är de beräknade värdena. 

Exempelindata kan vara: 

* Frekvens - 12
* Horizon - 12
* Datum - 15/1/2012, 2012-2-15, 2012-3-15; 4-15-2012; 2012-5-15; 6/15/2012; 15/7/2012, 8 / 15/2012, 9-15-2012, 10/15/2012; 11-15-2012, 2012-12/15; 2013-1/15; 2013-2/15; 2013-3/15; 2013-4/15; 2013-5/15; 2013-6/15; 2013-7/15; 8 / 15/2013; 2013-9/15; 2013/10/15; 2013-11/15; 2013-12/15; 2014-1-15, 2014-2-15, 2014-3-15; 2014-4-15; 2014-5-15; 2014-6-15; 2014-7-15, 8 / 2014-15; 2014-9-15
* Värde: 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509

> Den här tjänsten, är som finns på Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder. 
> 
> 

Det finns flera olika sätt att använda tjänsten automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/ArimaForecasting.aspx)).

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
        var acitionUri =  "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";

          var httpClient = new HttpClient();
           httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere","ChangeToAPIKey");
           httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
          var query = httpClient.PostAsync(acitionUri,new StringContent(json));
          var result = query.Result.Content;
          var scoreResult = result.ReadAsStringAsync().Result;
      }

## <a name="creation-of-web-service"></a>Skapandet av web service
> Den här webbtjänsten har skapats med Azure Machine Learning. För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml). Nedan visas en skärmbild av experiment som skapade web service och exempel koden för alla moduler i experimentet.
> 
> 

I Azure Machine Learning skapas skapades ett nytt tomt experiment från. Inkommande exempeldata överfördes med en fördefinierad dataschemat. Länkad till data schemat är en [köra R-skriptet] [ execute-r-script] modulen som genererar ARIMA prognoser modellen med funktionerna 'auto.arima' och 'prognosen' från R. 

### <a name="experiment-flow"></a>Experiment flöde:
![Skapa arbetsyta][2]

#### <a name="module-1"></a>Modul 1:
    # Add in the CSV file with the data in the format shown below 
![Skapa arbetsyta][3]    

#### <a name="module-2"></a>Modulen 2:
    # data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)

    # preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]

    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)

    # fit a time-series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- auto.arima(train_ts)
    train_model <- forecast(fit1, h = data$horizon)
    plot(train_model)

    # produce forecasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")

    # data output
    maml.mapOutputPort("data.forecast");


## <a name="limitations"></a>Begränsningar
Detta är ett väldigt enkelt exempel för Prognosticering ARIMA. Som kan ses från exempelkoden ovan, inga fel som fångas implementeras och tjänsten förutsätter att alla variabler är kontinuerlig positiva värden och hur ofta ska vara ett heltal som är större än 1. Längden på angreppsmetoderna datum och värdet ska vara samma. Variabeln datum bör följa formatet ”mm/dd/åååå'.

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
Vanliga frågor om förbrukningen av webbtjänst eller publicering till marketplace finns [här](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-arima/arima-img1.png
[2]: ./media/machine-learning-r-csharp-arima/arima-img2.png
[3]: ./media/machine-learning-r-csharp-arima/arima-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

