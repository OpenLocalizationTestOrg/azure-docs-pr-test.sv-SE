---
title: "(föråldrad) Prognosticering - ETS + STL - Azure | Microsoft Docs"
description: "(föråldrad) Prognosticering - ETS + STL"
services: machine-learning
documentationcenter: 
author: xueshanz
manager: jhubbard
editor: cgronlun
ms.assetid: 153eab4d-6293-45e1-9871-ec339e810dd9
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
ms.openlocfilehash: a575af931a41b7a55eb2102f3553640a16099146
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-forecasting---ets--stl"></a><span data-ttu-id="b2b3f-103">(föråldrad) Prognosticering - ETS + STL</span><span class="sxs-lookup"><span data-stu-id="b2b3f-103">(deprecated) Forecasting - ETS + STL</span></span>

> [!NOTE]
> <span data-ttu-id="b2b3f-104">Microsoft DataMarket dras och detta API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="b2b3f-105">Du hittar många användbara exempel experiment och API: er i den [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="b2b3f-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="b2b3f-106">Mer information om galleriet finns [resursen och identifiera resurser i Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="b2b3f-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="b2b3f-107">Detta [webbtjänsten](https://datamarket.azure.com/dataset/aml_labs/demand_forecast) implementerar när Trend uppdelning (STL) och exponentiell utjämning ETS modeller för att skapa förutsägelser baserat på historisk data som anges av användaren.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-107">This [web service](https://datamarket.azure.com/dataset/aml_labs/demand_forecast) implements Seasonal Trend Decomposition (STL) and Exponential Smoothing (ETS) models to produce predictions based on the historical data provided by the user.</span></span> <span data-ttu-id="b2b3f-108">Ökar behovet av en viss produkt år?</span><span class="sxs-lookup"><span data-stu-id="b2b3f-108">Will the demand for a specific product increase this year?</span></span> <span data-ttu-id="b2b3f-109">Kan jag förutsäga min produktförsäljning för säsongen jul så att jag kan effektivt kunna planera min inventering?</span><span class="sxs-lookup"><span data-stu-id="b2b3f-109">Can I predict my product sales for the Christmas season, so that I can effectively plan my inventory?</span></span> <span data-ttu-id="b2b3f-110">Prognosmodellen modeller är lgh att åtgärda dessa frågor.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-110">Forecasting models are apt to address such questions.</span></span> <span data-ttu-id="b2b3f-111">Senaste data får granska dessa modeller dolda trender och säsongsvärdet att förutsäga framtida trender.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-111">Given the past data, these models examine hidden trends and seasonality to predict future trends.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="b2b3f-112">Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-112">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="b2b3f-113">Men syftet med webbtjänsten är också som fungerar som ett exempel på hur Azure Machine Learning kan användas för att skapa webbtjänster ovanpå R-koden.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-113">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="b2b3f-114">Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-114">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="b2b3f-115">Webbtjänsten kan sedan publiceras på Azure Marketplace och används av användare och enheter över hela världen med inga infrastrukturinställningar av författaren till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-115">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="b2b3f-116">Användning av web service</span><span class="sxs-lookup"><span data-stu-id="b2b3f-116">Consumption of web service</span></span>
<span data-ttu-id="b2b3f-117">Den här tjänsten accepterar 4 argument och beräknar prognoser.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-117">This service accepts 4 arguments and calculates the forecasts.</span></span>
<span data-ttu-id="b2b3f-118">De angivna argumenten är:</span><span class="sxs-lookup"><span data-stu-id="b2b3f-118">The input arguments are:</span></span>

* <span data-ttu-id="b2b3f-119">Frekvens - anger frekvensen av rådata (varje dag/vecka/månad/varje kvartal/år).</span><span class="sxs-lookup"><span data-stu-id="b2b3f-119">Frequency - Indicates the frequency of the raw data (daily/weekly/monthly/quarterly/yearly).</span></span>
* <span data-ttu-id="b2b3f-120">Horizon - framtiden prognos tidsram.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-120">Horizon - Future forecast time-frame.</span></span>
* <span data-ttu-id="b2b3f-121">Date - lägga till i den nya tid series-data för tid.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-121">Date - Add in the new time series data for time.</span></span>
* <span data-ttu-id="b2b3f-122">Värde: lägga till i de nya tidsvärdena serie data.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-122">Value - Add in the new time series data values.</span></span>

<span data-ttu-id="b2b3f-123">Utdata från tjänsten är de beräknade värdena.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-123">The output of the service is the calculated forecast values.</span></span>

<span data-ttu-id="b2b3f-124">Exempelindata kan vara:</span><span class="sxs-lookup"><span data-stu-id="b2b3f-124">Sample input could be:</span></span> 

* <span data-ttu-id="b2b3f-125">Frekvens - 12</span><span class="sxs-lookup"><span data-stu-id="b2b3f-125">Frequency - 12</span></span>
* <span data-ttu-id="b2b3f-126">Horizon - 12</span><span class="sxs-lookup"><span data-stu-id="b2b3f-126">Horizon - 12</span></span>
* <span data-ttu-id="b2b3f-127">Datum - 15/1/2012, 2012-2-15, 2012-3-15; 4-15-2012; 2012-5-15; 6/15/2012; 15/7/2012, 8 / 15/2012, 9-15-2012, 10/15/2012; 11-15-2012, 2012-12/15; 2013-1/15; 2013-2/15; 2013-3/15; 2013-4/15; 2013-5/15; 2013-6/15; 2013-7/15; 8 / 15/2013; 2013-9/15; 2013/10/15; 2013-11/15; 2013-12/15; 2014-1-15, 2014-2-15, 2014-3-15; 2014-4-15; 2014-5-15; 2014-6-15; 2014-7-15, 8 / 2014-15; 2014-9-15</span><span class="sxs-lookup"><span data-stu-id="b2b3f-127">Date - 1/15/2012;2/15/2012;3/15/2012;4/15/2012;5/15/2012;6/15/2012;7/15/2012;8/15/2012;9/15/2012;10/15/2012;11/15/2012;12/15/2012; 1/15/2013;2/15/2013;3/15/2013;4/15/2013;5/15/2013;6/15/2013;7/15/2013;8/15/2013;9/15/2013;10/15/2013;11/15/2013;12/15/2013; 1/15/2014;2/15/2014;3/15/2014;4/15/2014;5/15/2014;6/15/2014;7/15/2014;8/15/2014;9/15/2014</span></span>
* <span data-ttu-id="b2b3f-128">Värde: 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509</span><span class="sxs-lookup"><span data-stu-id="b2b3f-128">Value - 3.479;3.68;3.832;3.941;3.797;3.586;3.508;3.731;3.915;3.844;3.634;3.549;3.557;3.785;3.782;3.601;3.544;3.556;3.65;3.709;3.682;3.511; 3.429;3.51;3.523;3.525;3.626;3.695;3.711;3.711;3.693;3.571;3.509</span></span>

> <span data-ttu-id="b2b3f-129">Den här tjänsten, är som finns på Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-129">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="b2b3f-130">Det finns flera olika sätt att använda tjänsten automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/StlEtsForecasting.aspx)).</span><span class="sxs-lookup"><span data-stu-id="b2b3f-130">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/StlEtsForecasting.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="b2b3f-131">Startar C#-kod för web service användning:</span><span class="sxs-lookup"><span data-stu-id="b2b3f-131">Starting C# code for web service consumption:</span></span>
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
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };         var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


## <a name="creation-of-web-service"></a><span data-ttu-id="b2b3f-132">Skapandet av web service</span><span class="sxs-lookup"><span data-stu-id="b2b3f-132">Creation of web service</span></span>
> <span data-ttu-id="b2b3f-133">Den här webbtjänsten har skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-133">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="b2b3f-134">För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="b2b3f-134">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="b2b3f-135">Nedan visas en skärmbild av experiment som skapade web service och exempel koden för alla moduler i experimentet.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-135">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="b2b3f-136">I Azure Machine Learning skapas skapades ett nytt tomt experiment från.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-136">From within Azure Machine Learning, a new blank experiment was created.</span></span> <span data-ttu-id="b2b3f-137">Inkommande exempeldata överfördes med en fördefinierad dataschemat.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-137">Sample input data was uploaded with a predefined data schema.</span></span> <span data-ttu-id="b2b3f-138">Länkad till data schemat är en [köra R-skriptet] [ execute-r-script] modul, vilket genererar STL och ETS prognosmodeller med hjälp av stl, ets, och 'prognosen' funktioner från R.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-138">Linked to the data schema is an [Execute R Script][execute-r-script] module, which generates STL and ETS forecasting models by using ‘stl’, ‘ets’, and ‘forecast’ functions from R.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="b2b3f-139">Experiment flöde:</span><span class="sxs-lookup"><span data-stu-id="b2b3f-139">Experiment flow:</span></span>
![Experiment flöde][2]

#### <a name="module-1"></a><span data-ttu-id="b2b3f-141">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="b2b3f-141">Module 1:</span></span>
    # Add in the CSV file with the data in the format shown below 
![Exempeldata][3]    

#### <a name="module-2"></a><span data-ttu-id="b2b3f-143">Modulen 2:</span><span class="sxs-lookup"><span data-stu-id="b2b3f-143">Module 2:</span></span>
    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)

    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]

    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)

    # Fit a time series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- stl(train_ts,  s.window="periodic")
    train_model <- forecast(fit1, h = data$horizon, method = 'ets')
    plot(train_model)

    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")

    # Data output
    maml.mapOutputPort("data.forecast");

## <a name="limitations"></a><span data-ttu-id="b2b3f-144">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="b2b3f-144">Limitations</span></span>
<span data-ttu-id="b2b3f-145">Detta är ett väldigt enkelt exempel för ETS + STL prognoser.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-145">This is a very simple example for ETS + STL forecasting.</span></span> <span data-ttu-id="b2b3f-146">Som kan ses från exempelkoden ovan, inga fel som fångas implementeras och tjänsten förutsätter att alla variabler är kontinuerlig positiva värden och hur ofta ska vara ett heltal som är större än 1.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-146">As can be seen from the example code above, no error catching is implemented, and the service assumes that all the variables are continuous/positive values and the frequency should be an integer greater than 1.</span></span> <span data-ttu-id="b2b3f-147">Längden på angreppsmetoderna datum och värdet ska vara samma och längden för tidsserier ska vara större än 2 * frekvens.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-147">The length of the date and value vectors should be the same, and the length of the time series should be greater than 2*frequency.</span></span> <span data-ttu-id="b2b3f-148">Variabeln datum bör följa formatet ”mm/dd/åååå'.</span><span class="sxs-lookup"><span data-stu-id="b2b3f-148">The date variable should adhere to the format ‘mm/dd/yyyy’.</span></span>

## <a name="faq"></a><span data-ttu-id="b2b3f-149">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="b2b3f-149">FAQ</span></span>
<span data-ttu-id="b2b3f-150">Vanliga frågor om förbrukningen av webbtjänst eller publicering på Azure Marketplace finns [här](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="b2b3f-150">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img1.png
[2]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img2.png
[3]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

