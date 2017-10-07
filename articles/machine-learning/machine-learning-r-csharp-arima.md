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
redirect_document_id: True
ms.openlocfilehash: 4f3af41fd8873fdf75c6426fd395351cb41db190
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-forecasting---autoregressive-integrated-moving-average-arima"></a><span data-ttu-id="ff7a2-103">(föråldrad) Prognosticering - Autoregressive integrerad glidande medelvärde (ARIMA)</span><span class="sxs-lookup"><span data-stu-id="ff7a2-103">(deprecated) Forecasting - Autoregressive Integrated Moving Average (ARIMA)</span></span>

> [!NOTE]
> <span data-ttu-id="ff7a2-104">hello Microsoft DataMarket dras och detta API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="ff7a2-105">Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="ff7a2-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="ff7a2-106">Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="ff7a2-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>


<span data-ttu-id="ff7a2-107">Detta [service](https://datamarket.azure.com/dataset/aml_labs/arima) implementerar Autoregressive integrerad glidande medelvärde (ARIMA) tooproduce förutsägelser baserat på hello historiska data som hello användaren.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-107">This [service](https://datamarket.azure.com/dataset/aml_labs/arima) implements Autoregressive Integrated Moving Average (ARIMA) tooproduce predictions based on hello historical data provided by hello user.</span></span> <span data-ttu-id="ff7a2-108">Kommer hello begäran för en specifik produkt ökad år?</span><span class="sxs-lookup"><span data-stu-id="ff7a2-108">Will hello demand for a specific product increase this year?</span></span> <span data-ttu-id="ff7a2-109">Kan jag förutsäga min produktförsäljning för hello jul säsongen så att jag kan effektivt kunna planera min inventering?</span><span class="sxs-lookup"><span data-stu-id="ff7a2-109">Can I predict my product sales for hello Christmas season, so that I can effectively plan my inventory?</span></span> <span data-ttu-id="ff7a2-110">Prognosmodeller är lgh tooaddress sådana frågor.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-110">Forecasting models are apt tooaddress such questions.</span></span> <span data-ttu-id="ff7a2-111">Angivna hello tidigare data, Undersök dessa modeller dolda trender och säsongsvärdet toopredict framtida trender.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-111">Given hello past data, these models examine hidden trends and seasonality toopredict future trends.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="ff7a2-112">Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-112">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="ff7a2-113">Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-113">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="ff7a2-114">Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-114">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="ff7a2-115">hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-115">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="ff7a2-116">Användning av web service</span><span class="sxs-lookup"><span data-stu-id="ff7a2-116">Consumption of web service</span></span>
<span data-ttu-id="ff7a2-117">Den här tjänsten accepterar 4 argument och beräknar hello ARIMA prognoser.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-117">This service accepts 4 arguments and calculates hello ARIMA forecasts.</span></span>
<span data-ttu-id="ff7a2-118">hello-indataargument är:</span><span class="sxs-lookup"><span data-stu-id="ff7a2-118">hello input arguments are:</span></span>

* <span data-ttu-id="ff7a2-119">Frekvens - anger hello frekvensen av hello rådata (varje dag/vecka/månad/varje kvartal/år).</span><span class="sxs-lookup"><span data-stu-id="ff7a2-119">Frequency - Indicates hello frequency of hello raw data (daily/weekly/monthly/quarterly/yearly).</span></span>
* <span data-ttu-id="ff7a2-120">Horizon - framtiden prognos tidsram.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-120">Horizon - Future forecast time-frame.</span></span>
* <span data-ttu-id="ff7a2-121">Datum – Lägg till i hello nya tidsserier data för tid.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-121">Date - Add in hello new time series data for time.</span></span>
* <span data-ttu-id="ff7a2-122">Värde: lägga till i hello nya serie data tidsvärden.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-122">Value - Add in hello new time series data values.</span></span>

<span data-ttu-id="ff7a2-123">hello utdata från hello-tjänsten är hello beräknade värden.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-123">hello output of hello service is hello calculated forecast values.</span></span> 

<span data-ttu-id="ff7a2-124">Exempelindata kan vara:</span><span class="sxs-lookup"><span data-stu-id="ff7a2-124">Sample input could be:</span></span> 

* <span data-ttu-id="ff7a2-125">Frekvens - 12</span><span class="sxs-lookup"><span data-stu-id="ff7a2-125">Frequency - 12</span></span>
* <span data-ttu-id="ff7a2-126">Horizon - 12</span><span class="sxs-lookup"><span data-stu-id="ff7a2-126">Horizon - 12</span></span>
* <span data-ttu-id="ff7a2-127">Datum - 15/1/2012, 2012-2-15, 2012-3-15; 4-15-2012; 2012-5-15; 6/15/2012; 15/7/2012, 8 / 15/2012, 9-15-2012, 10/15/2012; 11-15-2012, 2012-12/15; 2013-1/15; 2013-2/15; 2013-3/15; 2013-4/15; 2013-5/15; 2013-6/15; 2013-7/15; 8 / 15/2013; 2013-9/15; 2013/10/15; 2013-11/15; 2013-12/15; 2014-1-15, 2014-2-15, 2014-3-15; 2014-4-15; 2014-5-15; 2014-6-15; 2014-7-15, 8 / 2014-15; 2014-9-15</span><span class="sxs-lookup"><span data-stu-id="ff7a2-127">Date - 1/15/2012;2/15/2012;3/15/2012;4/15/2012;5/15/2012;6/15/2012;7/15/2012;8/15/2012;9/15/2012;10/15/2012;11/15/2012;12/15/2012; 1/15/2013;2/15/2013;3/15/2013;4/15/2013;5/15/2013;6/15/2013;7/15/2013;8/15/2013;9/15/2013;10/15/2013;11/15/2013;12/15/2013; 1/15/2014;2/15/2014;3/15/2014;4/15/2014;5/15/2014;6/15/2014;7/15/2014;8/15/2014;9/15/2014</span></span>
* <span data-ttu-id="ff7a2-128">Värde: 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509</span><span class="sxs-lookup"><span data-stu-id="ff7a2-128">Value - 3.479;3.68;3.832;3.941;3.797;3.586;3.508;3.731;3.915;3.844;3.634;3.549;3.557;3.785;3.782;3.601;3.544;3.556;3.65;3.709;3.682;3.511; 3.429;3.51;3.523;3.525;3.626;3.695;3.711;3.711;3.693;3.571;3.509</span></span>

> <span data-ttu-id="ff7a2-129">Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-129">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="ff7a2-130">Det finns flera olika sätt att konsumera hello service automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/ArimaForecasting.aspx)).</span><span class="sxs-lookup"><span data-stu-id="ff7a2-130">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/ArimaForecasting.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="ff7a2-131">Startar C#-kod för web service användning:</span><span class="sxs-lookup"><span data-stu-id="ff7a2-131">Starting C# code for web service consumption:</span></span>
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

## <a name="creation-of-web-service"></a><span data-ttu-id="ff7a2-132">Skapandet av web service</span><span class="sxs-lookup"><span data-stu-id="ff7a2-132">Creation of web service</span></span>
> <span data-ttu-id="ff7a2-133">Den här webbtjänsten har skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-133">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="ff7a2-134">För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="ff7a2-134">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="ff7a2-135">Nedan visas en skärmbild av hello experiment som skapade hello web service och exempel-kod för varje hello moduler i hello experiment.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-135">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="ff7a2-136">I Azure Machine Learning skapas skapades ett nytt tomt experiment från.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-136">From within Azure Machine Learning, a new blank experiment was created.</span></span> <span data-ttu-id="ff7a2-137">Inkommande exempeldata överfördes med en fördefinierad dataschemat.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-137">Sample input data was uploaded with a predefined data schema.</span></span> <span data-ttu-id="ff7a2-138">Länkade toohello dataschemat är en [köra R-skriptet] [ execute-r-script] modulen som genererar hello ARIMA prognosmodellen med funktionerna 'auto.arima' och 'prognos' från R.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-138">Linked toohello data schema is an [Execute R Script][execute-r-script] module, which generates hello ARIMA forecasting model by using ‘auto.arima’ and ‘forecast’ functions from R.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="ff7a2-139">Experiment flöde:</span><span class="sxs-lookup"><span data-stu-id="ff7a2-139">Experiment flow:</span></span>
![Skapa arbetsyta][2]

#### <a name="module-1"></a><span data-ttu-id="ff7a2-141">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="ff7a2-141">Module 1:</span></span>
    # Add in hello CSV file with hello data in hello format shown below 
![Skapa arbetsyta][3]    

#### <a name="module-2"></a><span data-ttu-id="ff7a2-143">Modulen 2:</span><span class="sxs-lookup"><span data-stu-id="ff7a2-143">Module 2:</span></span>
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


## <a name="limitations"></a><span data-ttu-id="ff7a2-144">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="ff7a2-144">Limitations</span></span>
<span data-ttu-id="ff7a2-145">Detta är ett väldigt enkelt exempel för Prognosticering ARIMA.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-145">This is a very simple example for ARIMA forecasting.</span></span> <span data-ttu-id="ff7a2-146">Som kan ses från hello exempelkod ovan, inga fel som fångas implementeras och hello service förutsätter att alla hello-variabler är kontinuerlig positiva värden och hello frekvensen måste vara ett heltal som är större än 1.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-146">As can be seen from hello example code above, no error catching is implemented, and hello service assumes that all hello variables are continuous/positive values and hello frequency should be an integer greater than 1.</span></span> <span data-ttu-id="ff7a2-147">hello tidslängd hello datum och värdet angreppsmetoderna bör hello samma.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-147">hello length of hello date and value vectors should be hello same.</span></span> <span data-ttu-id="ff7a2-148">hello datum variabeln bör följa toohello format 'mm/dd/åååå'.</span><span class="sxs-lookup"><span data-stu-id="ff7a2-148">hello date variable should adhere toohello format ‘mm/dd/yyyy’.</span></span>

## <a name="faq"></a><span data-ttu-id="ff7a2-149">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="ff7a2-149">FAQ</span></span>
<span data-ttu-id="ff7a2-150">Läs vanliga frågor om förbrukningen av hello webbtjänst eller publishing toomarketplace [här](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="ff7a2-150">For frequently asked questions on consumption of hello web service or publishing toomarketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-arima/arima-img1.png
[2]: ./media/machine-learning-r-csharp-arima/arima-img2.png
[3]: ./media/machine-learning-r-csharp-arima/arima-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

