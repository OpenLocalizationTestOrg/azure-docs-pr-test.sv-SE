---
title: aaa(deprecated) skillnaden i proportioner Test - Azure | Microsoft Docs
description: "(föråldrad) Skillnad i proportioner Test"
services: machine-learning
documentationcenter: 
author: aniedea
manager: jhubbard
editor: cgronlun
ms.assetid: 9356b821-5345-44f6-8e26-719f2dea5e6d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: aniedea
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 820aad377f9dec12b0ef455974aaa95f6e8d723a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-difference-in-proportions-test"></a><span data-ttu-id="2bfb1-103">(föråldrad) Skillnad i proportioner Test</span><span class="sxs-lookup"><span data-stu-id="2bfb1-103">(deprecated) Difference in Proportions Test</span></span>

> [!NOTE]
> <span data-ttu-id="2bfb1-104">hello Microsoft DataMarket dras och detta API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="2bfb1-105">Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="2bfb1-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="2bfb1-106">Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="2bfb1-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="2bfb1-107">Är två proportioner statistiskt?</span><span class="sxs-lookup"><span data-stu-id="2bfb1-107">Are two proportions statistically different?</span></span> <span data-ttu-id="2bfb1-108">Anta att en användare vill toocompare två filmer toodetermine om en film som har en avsevärt högre andel av 'gillar' när jämfört med andra toohello.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-108">Suppose a user wants toocompare two movies toodetermine if one movie has a significantly higher proportion of ‘likes’ when compared toohello other.</span></span> <span data-ttu-id="2bfb1-109">Med ett stort urval, kan det finnas en statistiskt betydande skillnader mellan hello proportioner 0,50 och 0,51.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-109">With a large sample, there could be a statistically significant difference between hello proportions 0.50 and 0.51.</span></span> <span data-ttu-id="2bfb1-110">Med ett litet exempel, finns det inte tillräckligt med data toodetermine om dessa proportioner är faktiskt olika.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-110">With a small sample, there may not be enough data toodetermine if these proportions are actually different.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="2bfb1-111">Detta [webbtjänsten](https://datamarket.azure.com/dataset/aml_labs/prop_test) utför ett redundanstest hypotesen hello skillnaden i två proportioner baserat på indata från användaren hello antalet lyckade och hello Totalt antal försök för hello 2 jämförelse grupper.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-111">This [web service](https://datamarket.azure.com/dataset/aml_labs/prop_test) conducts a hypothesis test of hello difference in two proportions based on user input of hello number of successes and hello total number of trials for hello 2 comparison groups.</span></span> <span data-ttu-id="2bfb1-112">Den här webbtjänsten kan anropas från i ett möjligt scenario inom en jämförelse filmapp, om hello användaren om något av hello filmer är 'gillade' oftare än hello andra, baserat på film klassificeringar.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-112">In one possible scenario, this web service could be called from within a movie comparison app, telling hello user whether one of hello movies is really ‘liked’ more often than hello other, based on movie ratings.</span></span>

> <span data-ttu-id="2bfb1-113">Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="2bfb1-114">Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-114">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="2bfb1-115">Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="2bfb1-116">hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-116">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="2bfb1-117">Användning av web service</span><span class="sxs-lookup"><span data-stu-id="2bfb1-117">Consumption of web service</span></span>
<span data-ttu-id="2bfb1-118">Den här tjänsten accepteras 4 argument och har en hypotes testa av proportionerna.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-118">This service accepts 4 arguments and does a hypothesis test of proportions.</span></span>

<span data-ttu-id="2bfb1-119">hello-indataargument är:</span><span class="sxs-lookup"><span data-stu-id="2bfb1-119">hello input arguments are:</span></span>

* <span data-ttu-id="2bfb1-120">Successes1 - antalet lyckade händelser i exempel 1.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-120">Successes1 - Number of success events in sample 1.</span></span>
* <span data-ttu-id="2bfb1-121">Successes2 - antalet lyckade händelser i exempel 2.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-121">Successes2 - Number of success events in sample 2.</span></span>
* <span data-ttu-id="2bfb1-122">Total1 - storleken i exempel 1.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-122">Total1 -  Size of sample 1.</span></span>
* <span data-ttu-id="2bfb1-123">Total2 - storleken på exempel 2.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-123">Total2 - Size of sample 2.</span></span>

<span data-ttu-id="2bfb1-124">hello utdata från hello-tjänsten är hello resultatet av hello hypotesen testa tillsammans med hello chi-square statistik, df, p-värde och procentandel i exempel 1/2 och konfidensintervallet gränser.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-124">hello output of hello service is hello result of hello hypothesis test along with hello chi-square statistic, df, p-value, and proportion in sample 1/2 and confidence interval bounds.</span></span>

> <span data-ttu-id="2bfb1-125">Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-125">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="2bfb1-126">Det finns flera olika sätt att konsumera hello service automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx)).</span><span class="sxs-lookup"><span data-stu-id="2bfb1-126">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="2bfb1-127">Startar C#-kod för web service användning:</span><span class="sxs-lookup"><span data-stu-id="2bfb1-127">Starting C# code for web service consumption:</span></span>
    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


## <a name="creation-of-web-service"></a><span data-ttu-id="2bfb1-128">Skapandet av web service</span><span class="sxs-lookup"><span data-stu-id="2bfb1-128">Creation of web service</span></span>
> <span data-ttu-id="2bfb1-129">Den här webbtjänsten har skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-129">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="2bfb1-130">För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="2bfb1-130">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="2bfb1-131">Nedan visas en skärmbild av hello experiment som skapade hello web service och exempel-kod för varje hello moduler i hello experiment.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-131">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="2bfb1-132">Från inom Azure Machine Learning skapas ett nytt tomt experiment har skapats med två [köra R-skriptet] [ execute-r-script] moduler.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-132">From within Azure Machine Learning, a new blank experiment was created with two [Execute R Script][execute-r-script] modules.</span></span> <span data-ttu-id="2bfb1-133">I hello första modulen har hello dataschemat definierats, medan andra hello-modul använder hello prop.test kommandot R tooperform hello hypotestest för 2 proportionerna.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-133">In hello first module hello data schema is defined, while hello second module uses hello prop.test command within R tooperform hello hypothesis test for 2 proportions.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="2bfb1-134">Experiment flöde:</span><span class="sxs-lookup"><span data-stu-id="2bfb1-134">Experiment flow:</span></span>
![Experiment flöde][2]

#### <a name="module-1"></a><span data-ttu-id="2bfb1-136">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="2bfb1-136">Module 1:</span></span>
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data toooutput port
    dataset1 = maml.mapInputPort(1) #read data from input port


#### <a name="module-2"></a><span data-ttu-id="2bfb1-137">Modulen 2:</span><span class="sxs-lookup"><span data-stu-id="2bfb1-137">Module 2:</span></span>
    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"hello proportions are different!",
    "hello proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port


## <a name="limitations"></a><span data-ttu-id="2bfb1-138">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="2bfb1-138">Limitations</span></span>
<span data-ttu-id="2bfb1-139">Detta är ett väldigt enkelt exempel för ett test av skillnader i 2 proportionerna.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-139">This is a very simple example for a test of difference in 2 proportions.</span></span> <span data-ttu-id="2bfb1-140">Som kan ses från hello exempelkod ovan, inga fel som fångas implementeras och hello antas att alla hello-variabler är kontinuerlig.</span><span class="sxs-lookup"><span data-stu-id="2bfb1-140">As can be seen from hello example code above, no error catching is implemented and hello service assumes that all hello variables are continuous.</span></span>

## <a name="faq"></a><span data-ttu-id="2bfb1-141">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="2bfb1-141">FAQ</span></span>
<span data-ttu-id="2bfb1-142">Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="2bfb1-142">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

