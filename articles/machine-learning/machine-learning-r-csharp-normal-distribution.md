---
title: "(föråldrad) Normalfördelning Web Service Suite - Azure | Microsoft Docs"
description: "(föråldrad) Normalfördelning Web Service Suite"
services: machine-learning
documentationcenter: 
author: ireiter
manager: jhubbard
editor: cgronlun
ms.assetid: aab7b92e-953b-43d8-b0af-031394406bfe
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: ireiter
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: 79d1621330ad56b0c62ca46cfac424c2306e371f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-normal-distribution-suite"></a><span data-ttu-id="6703c-103">(föråldrad) Normalfördelning Suite</span><span class="sxs-lookup"><span data-stu-id="6703c-103">(deprecated) Normal Distribution Suite</span></span>

> [!NOTE]
> <span data-ttu-id="6703c-104">Microsoft DataMarket dras och detta API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="6703c-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="6703c-105">Du hittar många användbara exempel experiment och API: er i den [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="6703c-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="6703c-106">Mer information om galleriet finns [resursen och identifiera resurser i Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="6703c-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="6703c-107">Normalfördelning Suite är en uppsättning exempel webbtjänster ([Generator](https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile Kalkylatorn](https://datamarket.azure.com/dataset/aml_labs/ndq5), [sannolikhet Kalkylatorn](https://datamarket.azure.com/dataset/aml_labs/ndp5)) som hjälp vid generering och hantering Normal distributioner.</span><span class="sxs-lookup"><span data-stu-id="6703c-107">The Normal Distribution Suite is a set of sample web services ([Generator](https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile Calculator](https://datamarket.azure.com/dataset/aml_labs/ndq5), [Probability Calculator](https://datamarket.azure.com/dataset/aml_labs/ndp5)) that help in generating and handling normal distributions.</span></span> <span data-ttu-id="6703c-108">Tjänster kan generera en normalfördelning sekvens som helst, beräkna quantiles från en viss sannolikhet och beräkna sannolikheten från en viss quantile.</span><span class="sxs-lookup"><span data-stu-id="6703c-108">The services allow generating a normal distribution sequence of any length, calculating quantiles from a given probability, and calculating probability from a given quantile.</span></span> <span data-ttu-id="6703c-109">Varje tjänst skickar annan utdata baserat på den valda tjänsten (se nedan).</span><span class="sxs-lookup"><span data-stu-id="6703c-109">Each of the services emits different outputs based on the selected service (see description below).</span></span> <span data-ttu-id="6703c-110">Programsviten normalfördelning baseras på R funktioner qnorm, rnorm och pnorm, som ingår i statistik R-paket.</span><span class="sxs-lookup"><span data-stu-id="6703c-110">The Normal Distribution Suite is based on the R functions qnorm, rnorm, and pnorm, which are included in R stats package.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="6703c-111">Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="6703c-111">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="6703c-112">Men syftet med webbtjänsten är också som fungerar som ett exempel på hur Azure Machine Learning kan användas för att skapa webbtjänster ovanpå R-koden.</span><span class="sxs-lookup"><span data-stu-id="6703c-112">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="6703c-113">Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="6703c-113">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="6703c-114">Webbtjänsten kan sedan publiceras på Azure Marketplace och används av användare och enheter över hela världen med inga infrastrukturinställningar av författaren till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="6703c-114">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="6703c-115">Användning av web service</span><span class="sxs-lookup"><span data-stu-id="6703c-115">Consumption of web service</span></span>
<span data-ttu-id="6703c-116">Programsviten normalfördelning innehåller följande 3.</span><span class="sxs-lookup"><span data-stu-id="6703c-116">The Normal Distribution Suite includes the following 3 services.</span></span>

### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="6703c-117">Normalfördelning Quantile Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="6703c-117">Normal Distribution Quantile Calculator</span></span>
<span data-ttu-id="6703c-118">Den här tjänsten accepterar 4 argument för en normal distribution och beräknar associerade quantile.</span><span class="sxs-lookup"><span data-stu-id="6703c-118">This service accepts 4 arguments of a normal distribution and calculates the associated quantile.</span></span>

<span data-ttu-id="6703c-119">De angivna argumenten är:</span><span class="sxs-lookup"><span data-stu-id="6703c-119">The input arguments are:</span></span>

* <span data-ttu-id="6703c-120">p - en enda sannolikheten för en händelse med normalfördelning.</span><span class="sxs-lookup"><span data-stu-id="6703c-120">p - A single probability of an event with normal distribution.</span></span> 
* <span data-ttu-id="6703c-121">medelvärde - normalfördelning medelvärdet.</span><span class="sxs-lookup"><span data-stu-id="6703c-121">Mean - The normal distribution mean.</span></span>
* <span data-ttu-id="6703c-122">SD - standardavvikelsen normalfördelning.</span><span class="sxs-lookup"><span data-stu-id="6703c-122">SD - The normal distribution standard deviation.</span></span> 
* <span data-ttu-id="6703c-123">Sida - L för lägre sida av distributionen och U för övre delen av distributionen.</span><span class="sxs-lookup"><span data-stu-id="6703c-123">Side - L for the lower side of the distribution and U for the upper side of the distribution.</span></span>

<span data-ttu-id="6703c-124">Utdata från tjänsten är beräknade quantile som är associerad med den angivna sannolikheten.</span><span class="sxs-lookup"><span data-stu-id="6703c-124">The output of the service is the calculated quantile that is associated with the given probability.</span></span>

### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="6703c-125">Normalfördelning sannolikhet Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="6703c-125">Normal Distribution Probability Calculator</span></span>
<span data-ttu-id="6703c-126">Den här tjänsten accepterar 4 argument för en normal distribution och beräknar associerade sannolikheten.</span><span class="sxs-lookup"><span data-stu-id="6703c-126">This service accepts 4 arguments of a normal distribution and calculates the associated probability.</span></span>

<span data-ttu-id="6703c-127">De angivna argumenten är:</span><span class="sxs-lookup"><span data-stu-id="6703c-127">The input arguments are:</span></span>

* <span data-ttu-id="6703c-128">q - en enda quantile av en händelse med normalfördelning.</span><span class="sxs-lookup"><span data-stu-id="6703c-128">q - A single quantile of an event with normal distribution.</span></span> 
* <span data-ttu-id="6703c-129">medelvärde - normalfördelning medelvärdet.</span><span class="sxs-lookup"><span data-stu-id="6703c-129">Mean - The normal distribution mean.</span></span>
* <span data-ttu-id="6703c-130">SD - standardavvikelsen normalfördelning.</span><span class="sxs-lookup"><span data-stu-id="6703c-130">SD - The normal distribution standard deviation.</span></span> 
* <span data-ttu-id="6703c-131">Sida - L för lägre sida av distributionen och U för övre delen av distributionen.</span><span class="sxs-lookup"><span data-stu-id="6703c-131">Side - L for the lower side of the distribution and U for the upper side of the distribution.</span></span>

<span data-ttu-id="6703c-132">Utdata från tjänsten är beräknade sannolikheten som är associerad med den angivna quantile.</span><span class="sxs-lookup"><span data-stu-id="6703c-132">The output of the service is the calculated probability that is associated with the given quantile.</span></span>

### <a name="normal-distribution-generator"></a><span data-ttu-id="6703c-133">Normalfördelning Generator</span><span class="sxs-lookup"><span data-stu-id="6703c-133">Normal Distribution Generator</span></span>
<span data-ttu-id="6703c-134">Den här tjänsten accepterar 3 argument för en normal distribution och genererar en slumpmässig sekvens av siffror som normalt distribueras.</span><span class="sxs-lookup"><span data-stu-id="6703c-134">This service accepts 3 arguments of a normal distribution and generates a random sequence of numbers that are normally distributed.</span></span> <span data-ttu-id="6703c-135">Följande argument måste tillhandahållas till den i begäran:</span><span class="sxs-lookup"><span data-stu-id="6703c-135">The following arguments should be provided to it within the request:</span></span>

* <span data-ttu-id="6703c-136">n - antalet observationer.</span><span class="sxs-lookup"><span data-stu-id="6703c-136">n - The number of observations.</span></span> 
* <span data-ttu-id="6703c-137">medelvärde - normalfördelning medelvärdet.</span><span class="sxs-lookup"><span data-stu-id="6703c-137">mean - The normal distribution mean.</span></span>
* <span data-ttu-id="6703c-138">SD - standardavvikelsen normalfördelning.</span><span class="sxs-lookup"><span data-stu-id="6703c-138">sd - The normal distribution standard deviation.</span></span> 

<span data-ttu-id="6703c-139">Utdata från tjänsten är en sekvens med längden n med normalfördelning baserat på medelvärde och sd-argument.</span><span class="sxs-lookup"><span data-stu-id="6703c-139">The output of the service is a sequence of length n with a normal distribution based on the mean and sd arguments.</span></span>

> <span data-ttu-id="6703c-140">Den här tjänsten, är som finns på Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder.</span><span class="sxs-lookup"><span data-stu-id="6703c-140">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="6703c-141">Det finns flera olika sätt att använda tjänsten automatiskt (exempel appar finns här: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [sannolikhet Kalkylatorn](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Quantile Kalkylatorn](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).</span><span class="sxs-lookup"><span data-stu-id="6703c-141">There are multiple ways of consuming the service in an automated fashion (example apps are here: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [Probability Calculator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Quantile Calculator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="6703c-142">Startar C#-kod för web service användning:</span><span class="sxs-lookup"><span data-stu-id="6703c-142">Starting C# code for web service consumption:</span></span>
### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="6703c-143">Normalfördelning Quantile Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="6703c-143">Normal Distribution Quantile Calculator</span></span>
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="6703c-144">Normalfördelning sannolikhet Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="6703c-144">Normal Distribution Probability Calculator</span></span>
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

### <a name="normal-distribution-generator"></a><span data-ttu-id="6703c-145">Normalfördelning Generator</span><span class="sxs-lookup"><span data-stu-id="6703c-145">Normal Distribution Generator</span></span>
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


## <a name="creation-of-web-service"></a><span data-ttu-id="6703c-146">Skapandet av web service</span><span class="sxs-lookup"><span data-stu-id="6703c-146">Creation of web service</span></span>
> <span data-ttu-id="6703c-147">Den här webbtjänsten har skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="6703c-147">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="6703c-148">För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="6703c-148">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> 
> 
> 

<span data-ttu-id="6703c-149">Nedan visas en skärmbild av experiment som skapade web service och exempel koden för alla moduler i experimentet.</span><span class="sxs-lookup"><span data-stu-id="6703c-149">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>

### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="6703c-150">Normalfördelning Quantile Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="6703c-150">Normal Distribution Quantile Calculator</span></span>
<span data-ttu-id="6703c-151">Experiment flöde:</span><span class="sxs-lookup"><span data-stu-id="6703c-151">Experiment flow:</span></span>

![Experiment flöde][2]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port

    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="6703c-153">Normalfördelning sannolikhet Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="6703c-153">Normal Distribution Probability Calculator</span></span>
<span data-ttu-id="6703c-154">Experiment flöde:</span><span class="sxs-lookup"><span data-stu-id="6703c-154">Experiment flow:</span></span>

![Experiment flöde][3]

     #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port

    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-generator"></a><span data-ttu-id="6703c-156">Normalfördelning Generator</span><span class="sxs-lookup"><span data-stu-id="6703c-156">Normal Distribution Generator</span></span>
<span data-ttu-id="6703c-157">Experiment flöde:</span><span class="sxs-lookup"><span data-stu-id="6703c-157">Experiment flow:</span></span>

![Experiment flöde][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port

    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a><span data-ttu-id="6703c-159">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="6703c-159">Limitations</span></span>
<span data-ttu-id="6703c-160">Dessa är väldigt enkelt exempel runt normalfördelningen.</span><span class="sxs-lookup"><span data-stu-id="6703c-160">These are very simple examples surrounding the normal distribution.</span></span> <span data-ttu-id="6703c-161">Lite fel fånga har implementerats som kan ses från exempelkoden ovan.</span><span class="sxs-lookup"><span data-stu-id="6703c-161">As can be seen from the example code above, little error catching is implemented.</span></span>

## <a name="faq"></a><span data-ttu-id="6703c-162">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="6703c-162">FAQ</span></span>
<span data-ttu-id="6703c-163">Vanliga frågor om förbrukningen av webbtjänst eller publicering på Azure Marketplace finns [här](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="6703c-163">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png

