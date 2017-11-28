---
title: "aaa(deprecated) normalfördelning Web Service Suite - Azure | Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 8bdb5afd9fee88587f548d7c5299480f64289bbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-normal-distribution-suite"></a><span data-ttu-id="4b6a7-103">(föråldrad) Normalfördelning Suite</span><span class="sxs-lookup"><span data-stu-id="4b6a7-103">(deprecated) Normal Distribution Suite</span></span>

> [!NOTE]
> <span data-ttu-id="4b6a7-104">hello Microsoft DataMarket dras och detta API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="4b6a7-105">Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="4b6a7-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="4b6a7-106">Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="4b6a7-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="4b6a7-107">hello normalfördelning Suite är en uppsättning exempel webbtjänster ([Generator](https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile Kalkylatorn](https://datamarket.azure.com/dataset/aml_labs/ndq5), [sannolikhet Kalkylatorn](https://datamarket.azure.com/dataset/aml_labs/ndp5)) som hjälp vid generering och hantering Normal distributioner.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-107">hello Normal Distribution Suite is a set of sample web services ([Generator](https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile Calculator](https://datamarket.azure.com/dataset/aml_labs/ndq5), [Probability Calculator](https://datamarket.azure.com/dataset/aml_labs/ndp5)) that help in generating and handling normal distributions.</span></span> <span data-ttu-id="4b6a7-108">hello tjänster kan generera en normalfördelning sekvens som helst, beräkna quantiles från en viss sannolikhet och beräkna sannolikheten från en viss quantile.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-108">hello services allow generating a normal distribution sequence of any length, calculating quantiles from a given probability, and calculating probability from a given quantile.</span></span> <span data-ttu-id="4b6a7-109">Var och en av hello-tjänster skickar olika utdata baserat på valda hello-tjänsten (se nedan).</span><span class="sxs-lookup"><span data-stu-id="4b6a7-109">Each of hello services emits different outputs based on hello selected service (see description below).</span></span> <span data-ttu-id="4b6a7-110">hello normalfördelning Suite baseras på hello R funktioner qnorm rnorm och pnorm, som ingår i statistik R-paket.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-110">hello Normal Distribution Suite is based on hello R functions qnorm, rnorm, and pnorm, which are included in R stats package.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="4b6a7-111">Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-111">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="4b6a7-112">Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-112">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="4b6a7-113">Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-113">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="4b6a7-114">hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-114">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="4b6a7-115">Användning av web service</span><span class="sxs-lookup"><span data-stu-id="4b6a7-115">Consumption of web service</span></span>
<span data-ttu-id="4b6a7-116">hello normalfördelning Suite innehåller hello följande 3 tjänster.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-116">hello Normal Distribution Suite includes hello following 3 services.</span></span>

### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="4b6a7-117">Normalfördelning Quantile Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="4b6a7-117">Normal Distribution Quantile Calculator</span></span>
<span data-ttu-id="4b6a7-118">Den här tjänsten accepterar 4 argument för en normal distribution och beräknar hello associerade quantile.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-118">This service accepts 4 arguments of a normal distribution and calculates hello associated quantile.</span></span>

<span data-ttu-id="4b6a7-119">hello-indataargument är:</span><span class="sxs-lookup"><span data-stu-id="4b6a7-119">hello input arguments are:</span></span>

* <span data-ttu-id="4b6a7-120">p - en enda sannolikheten för en händelse med normalfördelning.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-120">p - A single probability of an event with normal distribution.</span></span> 
* <span data-ttu-id="4b6a7-121">Medelvärde - hello normalfördelning medelvärdet.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-121">Mean - hello normal distribution mean.</span></span>
* <span data-ttu-id="4b6a7-122">SD - standardavvikelsen för hello normalfördelning.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-122">SD - hello normal distribution standard deviation.</span></span> 
* <span data-ttu-id="4b6a7-123">Sida - L för hello lägre sida av hello distribution och U för hello övre delen av hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-123">Side - L for hello lower side of hello distribution and U for hello upper side of hello distribution.</span></span>

<span data-ttu-id="4b6a7-124">hello utdata från hello-tjänsten är hello beräknade quantile som är associerad med hello angivna sannolikhet.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-124">hello output of hello service is hello calculated quantile that is associated with hello given probability.</span></span>

### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="4b6a7-125">Normalfördelning sannolikhet Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="4b6a7-125">Normal Distribution Probability Calculator</span></span>
<span data-ttu-id="4b6a7-126">Den här tjänsten accepterar 4 argument för en normal distribution och beräknar hello associerade sannolikhet.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-126">This service accepts 4 arguments of a normal distribution and calculates hello associated probability.</span></span>

<span data-ttu-id="4b6a7-127">hello-indataargument är:</span><span class="sxs-lookup"><span data-stu-id="4b6a7-127">hello input arguments are:</span></span>

* <span data-ttu-id="4b6a7-128">q - en enda quantile av en händelse med normalfördelning.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-128">q - A single quantile of an event with normal distribution.</span></span> 
* <span data-ttu-id="4b6a7-129">Medelvärde - hello normalfördelning medelvärdet.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-129">Mean - hello normal distribution mean.</span></span>
* <span data-ttu-id="4b6a7-130">SD - standardavvikelsen för hello normalfördelning.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-130">SD - hello normal distribution standard deviation.</span></span> 
* <span data-ttu-id="4b6a7-131">Sida - L för hello lägre sida av hello distribution och U för hello övre delen av hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-131">Side - L for hello lower side of hello distribution and U for hello upper side of hello distribution.</span></span>

<span data-ttu-id="4b6a7-132">hello utdata från hello-tjänsten är hello beräknade sannolikhet som associeras med hello angivna quantile.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-132">hello output of hello service is hello calculated probability that is associated with hello given quantile.</span></span>

### <a name="normal-distribution-generator"></a><span data-ttu-id="4b6a7-133">Normalfördelning Generator</span><span class="sxs-lookup"><span data-stu-id="4b6a7-133">Normal Distribution Generator</span></span>
<span data-ttu-id="4b6a7-134">Den här tjänsten accepterar 3 argument för en normal distribution och genererar en slumpmässig sekvens av siffror som normalt distribueras.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-134">This service accepts 3 arguments of a normal distribution and generates a random sequence of numbers that are normally distributed.</span></span> <span data-ttu-id="4b6a7-135">hello måste följande argument tillhandahållas tooit inom hello begäran:</span><span class="sxs-lookup"><span data-stu-id="4b6a7-135">hello following arguments should be provided tooit within hello request:</span></span>

* <span data-ttu-id="4b6a7-136">n - hello antal observationer.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-136">n - hello number of observations.</span></span> 
* <span data-ttu-id="4b6a7-137">medelvärde - hello normalfördelning medelvärdet.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-137">mean - hello normal distribution mean.</span></span>
* <span data-ttu-id="4b6a7-138">SD - standardavvikelsen för hello normalfördelning.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-138">sd - hello normal distribution standard deviation.</span></span> 

<span data-ttu-id="4b6a7-139">hello utdata från hello service är en sekvens med längden n med normalfördelning baserat på hello medelvärde och sd-argument.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-139">hello output of hello service is a sequence of length n with a normal distribution based on hello mean and sd arguments.</span></span>

> <span data-ttu-id="4b6a7-140">Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-140">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="4b6a7-141">Det finns flera olika sätt att konsumera hello service automatiskt (exempel appar finns här: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [sannolikhet Kalkylatorn](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Quantile Kalkylatorn](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).</span><span class="sxs-lookup"><span data-stu-id="4b6a7-141">There are multiple ways of consuming hello service in an automated fashion (example apps are here: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [Probability Calculator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Quantile Calculator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="4b6a7-142">Startar C#-kod för web service användning:</span><span class="sxs-lookup"><span data-stu-id="4b6a7-142">Starting C# code for web service consumption:</span></span>
### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="4b6a7-143">Normalfördelning Quantile Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="4b6a7-143">Normal Distribution Quantile Calculator</span></span>
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


### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="4b6a7-144">Normalfördelning sannolikhet Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="4b6a7-144">Normal Distribution Probability Calculator</span></span>
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

### <a name="normal-distribution-generator"></a><span data-ttu-id="4b6a7-145">Normalfördelning Generator</span><span class="sxs-lookup"><span data-stu-id="4b6a7-145">Normal Distribution Generator</span></span>
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


## <a name="creation-of-web-service"></a><span data-ttu-id="4b6a7-146">Skapandet av web service</span><span class="sxs-lookup"><span data-stu-id="4b6a7-146">Creation of web service</span></span>
> <span data-ttu-id="4b6a7-147">Den här webbtjänsten har skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-147">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="4b6a7-148">För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="4b6a7-148">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> 
> 
> 

<span data-ttu-id="4b6a7-149">Nedan visas en skärmbild av hello experiment som skapade hello web service och exempel-kod för varje hello moduler i hello experiment.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-149">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>

### <a name="normal-distribution-quantile-calculator"></a><span data-ttu-id="4b6a7-150">Normalfördelning Quantile Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="4b6a7-150">Normal Distribution Quantile Calculator</span></span>
<span data-ttu-id="4b6a7-151">Experiment flöde:</span><span class="sxs-lookup"><span data-stu-id="4b6a7-151">Experiment flow:</span></span>

![Experiment flöde][2]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data toooutput port

    # Map 1-based optional input ports toovariables
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

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-probability-calculator"></a><span data-ttu-id="4b6a7-153">Normalfördelning sannolikhet Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="4b6a7-153">Normal Distribution Probability Calculator</span></span>
<span data-ttu-id="4b6a7-154">Experiment flöde:</span><span class="sxs-lookup"><span data-stu-id="4b6a7-154">Experiment flow:</span></span>

![Experiment flöde][3]

     #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data toooutput port

    # Map 1-based optional input ports toovariables
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

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-generator"></a><span data-ttu-id="4b6a7-156">Normalfördelning Generator</span><span class="sxs-lookup"><span data-stu-id="4b6a7-156">Normal Distribution Generator</span></span>
<span data-ttu-id="4b6a7-157">Experiment flöde:</span><span class="sxs-lookup"><span data-stu-id="4b6a7-157">Experiment flow:</span></span>

![Experiment flöde][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data toooutput port

    # Map 1-based optional input ports toovariables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a><span data-ttu-id="4b6a7-159">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="4b6a7-159">Limitations</span></span>
<span data-ttu-id="4b6a7-160">Dessa är väldigt enkelt exempel omgivande hello normalfördelning.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-160">These are very simple examples surrounding hello normal distribution.</span></span> <span data-ttu-id="4b6a7-161">Lite fel fånga har implementerats som kan ses från hello exempelkod ovan.</span><span class="sxs-lookup"><span data-stu-id="4b6a7-161">As can be seen from hello example code above, little error catching is implemented.</span></span>

## <a name="faq"></a><span data-ttu-id="4b6a7-162">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="4b6a7-162">FAQ</span></span>
<span data-ttu-id="4b6a7-163">Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="4b6a7-163">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png

