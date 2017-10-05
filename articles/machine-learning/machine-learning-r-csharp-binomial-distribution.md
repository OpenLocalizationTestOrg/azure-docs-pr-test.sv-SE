---
title: "(föråldrad) Binomialfördelningen Suite - Azure | Microsoft Docs"
description: "(föråldrad) Binomialfördelningen Suite"
services: machine-learning
documentationcenter: 
author: ireiter
manager: jhubbard
editor: cgronlun
ms.assetid: 6d102d57-8f20-4ab3-be31-02fcfe4d15ed
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
ms.openlocfilehash: 6f0a6d06e7401c8360a92a707a0552f41ff3657c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-binomial-distribution-suite"></a><span data-ttu-id="8b5fd-103">(föråldrad) Binomialfördelningen Suite</span><span class="sxs-lookup"><span data-stu-id="8b5fd-103">(deprecated) Binomial Distribution Suite</span></span>

> [!NOTE]
> <span data-ttu-id="8b5fd-104">Microsoft DataMarket dras och detta API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="8b5fd-105">Du hittar många användbara exempel experiment och API: er i den [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="8b5fd-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="8b5fd-106">Mer information om galleriet finns [resursen och identifiera resurser i Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="8b5fd-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="8b5fd-107">Binomialfördelningen Suite är en uppsättning exempel webbtjänster ([Binomial Generator](https://datamarket.azure.com/dataset/aml_labs/bdg5), [sannolikhet Kalkylatorn](https://datamarket.azure.com/dataset/aml_labs/bdp4), [Quantile Kalkylatorn](https://datamarket.azure.com/dataset/aml_labs/bdq5)) som hjälp i genererar och Hantera binomial distributioner.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-107">The Binomial Distribution Suite is a set of sample web services ([Binomial Generator](https://datamarket.azure.com/dataset/aml_labs/bdg5), [Probability Calculator](https://datamarket.azure.com/dataset/aml_labs/bdp4), [Quantile Calculator](https://datamarket.azure.com/dataset/aml_labs/bdq5)) that help in generating and dealing with binomial distributions.</span></span> <span data-ttu-id="8b5fd-108">Tjänster kan generera en binomialfördelningen sekvens med en längd som beräknar quantiles av angiven sannolikhet och beräkning av sannolikheten från en viss quantile.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-108">The services allow generating a binomial distribution sequence of any length, calculating quantiles out of given probability and calculating probability from a given quantile.</span></span> <span data-ttu-id="8b5fd-109">Varje tjänst skickar annan utdata baserat på den valda tjänsten (se nedan).</span><span class="sxs-lookup"><span data-stu-id="8b5fd-109">Each of the services emits different outputs based on the selected service (see description below).</span></span> <span data-ttu-id="8b5fd-110">Programsviten binomialfördelningen baseras på R funktioner qbinom, rbinom och pbinom, som ingår i paketet för R-statistik.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-110">The Binomial Distribution Suite is based on the R functions qbinom, rbinom, and pbinom, which are included in the R stats package.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> <span data-ttu-id="8b5fd-111">Dessa webbtjänster kan användas av användare – potentiellt direkt på marketplace via en mobil app via en webbplats eller på en lokal dator, till exempel.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-111">These web services could be consumed by users – potentially directly on the marketplace, through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="8b5fd-112">Men syftet med webbtjänsten är också som fungerar som ett exempel på hur Azure Machine Learning kan användas för att skapa webbtjänster ovanpå R-koden.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-112">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="8b5fd-113">Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-113">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="8b5fd-114">Webbtjänsten kan sedan publiceras på Azure Marketplace och används av användare och enheter över hela världen – krävs inga infrastrukturinställningar av författaren till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-114">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world – no infrastructure setup by the author of the web service is required.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="8b5fd-115">Användning av web service</span><span class="sxs-lookup"><span data-stu-id="8b5fd-115">Consumption of web service</span></span>
<span data-ttu-id="8b5fd-116">Programsviten binomialfördelningen innehåller följande 3.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-116">The Binomial Distribution Suite includes the following 3 services.</span></span>

### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="8b5fd-117">Binomialfördelningen Quantile Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="8b5fd-117">Binomial Distribution Quantile Calculator</span></span>
<span data-ttu-id="8b5fd-118">Den här tjänsten accepterar 4 argument för en normal distribution och beräknar associerade quantile.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-118">This service accepts 4 arguments of a normal distribution and calculates the associated quantile.</span></span>
<span data-ttu-id="8b5fd-119">De angivna argumenten är:</span><span class="sxs-lookup"><span data-stu-id="8b5fd-119">The input arguments are:</span></span>

* <span data-ttu-id="8b5fd-120">p - en enda samman sannolikheten för flera försök.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-120">p - A single aggregated probability of multiple trials.</span></span>  
* <span data-ttu-id="8b5fd-121">storlek - antalet försök.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-121">size - The number of trials.</span></span>
* <span data-ttu-id="8b5fd-122">SANNOLIKHET - sannolikheten att lyckas i en utvärderingsversion.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-122">prob - The probability of success in a trial.</span></span>
* <span data-ttu-id="8b5fd-123">Sida - L för lägre sida av distribution, U för övre delen av distributionen.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-123">Side - L for the lower side of the distribution, U for the upper side of the distribution.</span></span> 

<span data-ttu-id="8b5fd-124">Utdata från tjänsten är beräknade quantile som är associerad med den angivna sannolikheten.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-124">The output of the service is the calculated quantile that is associated with the given probability.</span></span>

### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="8b5fd-125">Binomialfördelningen sannolikhet Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="8b5fd-125">Binomial Distribution Probability Calculator</span></span>
<span data-ttu-id="8b5fd-126">Den här tjänsten accepterar 4 argument för en binomialfördelningen och beräknar associerade sannolikheten.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-126">This service accepts 4 arguments of a binomial distribution and calculates the associated probability.</span></span>
<span data-ttu-id="8b5fd-127">De angivna argumenten är:</span><span class="sxs-lookup"><span data-stu-id="8b5fd-127">The input arguments are:</span></span>

* <span data-ttu-id="8b5fd-128">q - en enda quantile av en händelse med binomialfördelningen.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-128">q - A single quantile of an event with binomial distribution.</span></span> 
* <span data-ttu-id="8b5fd-129">storlek - antalet försök.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-129">size - The number of trials.</span></span>
* <span data-ttu-id="8b5fd-130">SANNOLIKHET - sannolikheten att lyckas i en utvärderingsversion.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-130">prob - The probability of success in a trial.</span></span>
* <span data-ttu-id="8b5fd-131">sida - L för distribution, U för övre delen av distribution eller E som är lika med ett enda antal lyckade nedre del.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-131">side - L for the lower side of the distribution, U for the upper side of the distribution, or E that is equal to a single number of successes.</span></span>

<span data-ttu-id="8b5fd-132">Utdata från tjänsten är beräknade sannolikheten som är associerad med den angivna quantile.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-132">The output of the service is the calculated probability that is associated with the given quantile.</span></span>

### <a name="binomial-distribution-generator"></a><span data-ttu-id="8b5fd-133">Binomialfördelningen Generator</span><span class="sxs-lookup"><span data-stu-id="8b5fd-133">Binomial Distribution Generator</span></span>
<span data-ttu-id="8b5fd-134">Den här tjänsten accepterar 3 argument för en binomialfördelningen och genererar en slumpmässig sekvens av siffror som binomially distribueras.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-134">This service accepts 3 arguments of a binomial distribution and generates a random sequence of numbers that are binomially distributed.</span></span> <span data-ttu-id="8b5fd-135">Följande argument måste tillhandahållas till den i begäran:</span><span class="sxs-lookup"><span data-stu-id="8b5fd-135">The following arguments should be provided to it within the request:</span></span>

* <span data-ttu-id="8b5fd-136">n - antal observationer.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-136">n - Number of observations.</span></span> 
* <span data-ttu-id="8b5fd-137">storlek - antalet försök.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-137">size - Number of trials.</span></span>
* <span data-ttu-id="8b5fd-138">SANNOLIKHET - sannolikheten att lyckas.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-138">prob - Probability of success.</span></span>

<span data-ttu-id="8b5fd-139">Utdata från tjänsten är en sekvens med längden n med en binomialfördelningen baserat på storleken och sannolikhet argument.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-139">The output of the service is a sequence of length n with a binomial distribution based on the size and prob arguments.</span></span>

> <span data-ttu-id="8b5fd-140">Den här tjänsten, är som finns på Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-140">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="8b5fd-141">Det finns flera olika sätt att använda tjänsten automatiskt (exempel appar finns här: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [sannolikhet Kalkylatorn](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Quantile Kalkylatorn](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)).</span><span class="sxs-lookup"><span data-stu-id="8b5fd-141">There are multiple ways of consuming the service in an automated fashion (example apps are here: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [Probability Calculator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Quantile Calculator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)).</span></span> 

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="8b5fd-142">Startar C#-kod för web service användning:</span><span class="sxs-lookup"><span data-stu-id="8b5fd-142">Starting C# code for web service consumption:</span></span>
### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="8b5fd-143">Binomialfördelningen Quantile Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="8b5fd-143">Binomial Distribution Quantile Calculator</span></span>
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="8b5fd-144">Binomialfördelningen sannolikhet Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="8b5fd-144">Binomial Distribution Probability Calculator</span></span>
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


### <a name="binomial-distribution-generator"></a><span data-ttu-id="8b5fd-145">Binomialfördelningen Generator</span><span class="sxs-lookup"><span data-stu-id="8b5fd-145">Binomial Distribution Generator</span></span>
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }





## <a name="creation-of-web-service"></a><span data-ttu-id="8b5fd-146">Skapandet av web service</span><span class="sxs-lookup"><span data-stu-id="8b5fd-146">Creation of web service</span></span>
> <span data-ttu-id="8b5fd-147">Den här webbtjänsten har skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-147">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="8b5fd-148">För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="8b5fd-148">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="8b5fd-149">Nedan visas en skärmbild av experiment som skapade web service och exempel koden för alla moduler i experimentet.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-149">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

### <a name="binomial-distribution-quantile-calculator"></a><span data-ttu-id="8b5fd-150">Binomialfördelningen Quantile Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="8b5fd-150">Binomial Distribution Quantile Calculator</span></span>
![Skapa arbetsyta][4]

#### <a name="module-1"></a><span data-ttu-id="8b5fd-152">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="8b5fd-152">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
#### <a name="module-2"></a><span data-ttu-id="8b5fd-153">Modulen 2:</span><span class="sxs-lookup"><span data-stu-id="8b5fd-153">Module 2:</span></span>
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


### <a name="binomial-distribution-probability-calculator"></a><span data-ttu-id="8b5fd-154">Binomialfördelningen sannolikhet Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="8b5fd-154">Binomial Distribution Probability Calculator</span></span>
![Skapa arbetsyta][5]

#### <a name="module-1"></a><span data-ttu-id="8b5fd-156">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="8b5fd-156">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


#### <a name="module-2"></a><span data-ttu-id="8b5fd-157">Modulen 2:</span><span class="sxs-lookup"><span data-stu-id="8b5fd-157">Module 2:</span></span>
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

### <a name="binomial-distribution-generator"></a><span data-ttu-id="8b5fd-158">Binomialfördelningen Generator</span><span class="sxs-lookup"><span data-stu-id="8b5fd-158">Binomial Distribution Generator</span></span>
![Skapa arbetsyta][6]

#### <a name="module-1"></a><span data-ttu-id="8b5fd-160">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="8b5fd-160">Module 1:</span></span>
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

#### <a name="module-2"></a><span data-ttu-id="8b5fd-161">Modulen 2:</span><span class="sxs-lookup"><span data-stu-id="8b5fd-161">Module 2:</span></span>
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a><span data-ttu-id="8b5fd-162">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="8b5fd-162">Limitations</span></span>
<span data-ttu-id="8b5fd-163">Dessa är väldigt enkelt exempel runt binomialfördelningen.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-163">These are very simple examples surrounding the binomial distribution.</span></span> <span data-ttu-id="8b5fd-164">Lite fel fånga har implementerats som kan ses från exempelkoden ovan.</span><span class="sxs-lookup"><span data-stu-id="8b5fd-164">As can be seen from the example code above, little error catching is implemented.</span></span>

## <a name="faq"></a><span data-ttu-id="8b5fd-165">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="8b5fd-165">FAQ</span></span>
<span data-ttu-id="8b5fd-166">Vanliga frågor om förbrukningen av webbtjänst eller publicering på Azure Marketplace finns [här](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="8b5fd-166">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png

