---
title: aaa(deprecated) klustermodellen - Azure | Microsoft Docs
description: "(föråldrad) Ett kluster"
services: machine-learning
documentationcenter: 
author: FrancescaLazzeri
manager: jhubbard
editor: cgronlun
ms.assetid: 51b8d012-ed44-4312-920c-9c808dfd4ff6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: lazzeri
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 7b2dffb855a8d91114752b579115e97d07210e45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-cluster-model"></a><span data-ttu-id="2e2c3-103">(föråldrad) Ett kluster</span><span class="sxs-lookup"><span data-stu-id="2e2c3-103">(deprecated) Cluster Model</span></span>

> [!NOTE]
> <span data-ttu-id="2e2c3-104">hello Microsoft DataMarket dras och detta API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="2e2c3-105">Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="2e2c3-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="2e2c3-106">Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="2e2c3-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="2e2c3-107">Hur kan vi för att förutsäga grupper av kredit cardholders beteenden i ordning tooreduce hello kostnad av risken för kreditkort utfärdare?</span><span class="sxs-lookup"><span data-stu-id="2e2c3-107">How can we predict groups of credit cardholders’ behaviors in order tooreduce hello charge-off risk of credit card issuers?</span></span> <span data-ttu-id="2e2c3-108">Hur kan vi definiera grupper för person egenskaper för anställda i ordning tooimprove deras prestanda på jobbet?</span><span class="sxs-lookup"><span data-stu-id="2e2c3-108">How can we define groups of personality traits of employees in order tooimprove their performance at work?</span></span> <span data-ttu-id="2e2c3-109">Hur kan läkare klassificera patienter i grupper baserat på deras sjukdomar hello egenskaper?</span><span class="sxs-lookup"><span data-stu-id="2e2c3-109">How can doctors classify patients into groups based on hello characteristics of their diseases?</span></span> <span data-ttu-id="2e2c3-110">I princip besvaras alla frågor via klustret analys.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-110">In principle, all of these questions can be answered through cluster analysis.</span></span>   

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="2e2c3-111">Klustret analys klassificerar en uppsättning observationer i två eller flera ömsesidigt uteslutande okänd grupper baserat på kombinationer av variabler.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-111">Cluster analysis classifies a set of observations into two or more mutually exclusive unknown groups based on combinations of variables.</span></span> <span data-ttu-id="2e2c3-112">hello syftar klustret analys toodiscover ett system för att organisera observationer, vanligtvis personer eller deras egenskaper i grupper där medlemmar i grupper hello har egenskaper gemensamt.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-112">hello purpose of cluster analysis is toodiscover a system of organizing observations, usually people or their characteristics, into groups, where members of hello groups share properties in common.</span></span> <span data-ttu-id="2e2c3-113">Detta [service](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) använder hello K-Means metoder, en vanliga klustring teknik toocluster godtyckliga data i grupper.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-113">This [service](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) uses hello K-Means methodology, a commonly used clustering technique, toocluster arbitrary data into groups.</span></span> <span data-ttu-id="2e2c3-114">Den här webbtjänsten tar hello data och hello antalet k kluster som indata och producerar förutsägelser som hello k grupper toowhich varje observationer tillhör.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-114">This web service takes hello data and hello number of k clusters as input, and produces predictions of which of hello k groups toowhich each observations belongs.</span></span> 

> <span data-ttu-id="2e2c3-115">Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-115">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="2e2c3-116">Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-116">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="2e2c3-117">Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-117">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="2e2c3-118">hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-118">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="2e2c3-119">Användning av web service</span><span class="sxs-lookup"><span data-stu-id="2e2c3-119">Consumption of web service</span></span>
<span data-ttu-id="2e2c3-120">Den här webbtjänsten grupperas hello data i en uppsättning k grupper och utdata hello grupptilldelning för varje rad.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-120">This web service groups hello data into a set of k groups and outputs hello group assignment for each row.</span></span> <span data-ttu-id="2e2c3-121">hello webbtjänst hello slutanvändaren tooinput data som en sträng där rader avgränsas med kommatecken (,) och kolumner avgränsade med semikolon (;).</span><span class="sxs-lookup"><span data-stu-id="2e2c3-121">hello web service expects hello end user tooinput data as a string where rows are separated by commas (,) and columns are separated by semicolons (;).</span></span> <span data-ttu-id="2e2c3-122">hello webbtjänst 1 rad i taget.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-122">hello web service expects 1 row at a time.</span></span> <span data-ttu-id="2e2c3-123">En exempel-datauppsättning kan se ut så här:</span><span class="sxs-lookup"><span data-stu-id="2e2c3-123">An example dataset could look like this:</span></span>

![Exempeldata][1]

<span data-ttu-id="2e2c3-125">Anta att hello användaren vill tooseparate dessa data i 3 ömsesidigt uteslutande grupper.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-125">Suppose hello user wanted tooseparate this data into 3 mutually exclusive groups.</span></span> <span data-ttu-id="2e2c3-126">Hej indata hello ovan dataset skulle bli hello följande: value = ”10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4”; k = ”3”.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-126">hello data input for hello above dataset would be hello following: value = “10;5;2,18;1;6,7;5;5,22;3;4,12;2;1,10;3;4”; k=”3”.</span></span> <span data-ttu-id="2e2c3-127">hello-utdata är hello förväntade gruppmedlemskap för varje hello rader.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-127">hello output is hello predicted group membership for each of hello rows.</span></span>

> <span data-ttu-id="2e2c3-128">Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-128">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="2e2c3-129">Det finns flera olika sätt att konsumera hello service automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).</span><span class="sxs-lookup"><span data-stu-id="2e2c3-129">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="2e2c3-130">Startar C#-kod för web service användning:</span><span class="sxs-lookup"><span data-stu-id="2e2c3-130">Starting C# code for web service consumption:</span></span>
    public class Input
    {
            public string value;
            public string k;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




## <a name="creation-of-web-service"></a><span data-ttu-id="2e2c3-131">Skapandet av web service</span><span class="sxs-lookup"><span data-stu-id="2e2c3-131">Creation of web service</span></span>
> <span data-ttu-id="2e2c3-132">Den här webbtjänsten har skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-132">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="2e2c3-133">För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="2e2c3-133">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="2e2c3-134">Nedan visas en skärmbild av hello experiment som skapade hello web service och exempel-kod för varje hello moduler i hello experiment.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-134">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="2e2c3-135">I Azure Machine Learning skapas ett nytt tomt experiment skapades från två [köra R-skriptet] [ execute-r-script] moduler hämtas till hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-135">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules pulled onto hello workspace.</span></span> <span data-ttu-id="2e2c3-136">hello dataschemat skapades med en enkel [köra R-skriptet][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="2e2c3-136">hello data schema was created with a simple [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="2e2c3-137">Sedan hello dataschemat var länkade toohello kluster modellen avsnittet igen skapats med en [köra R-skriptet][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="2e2c3-137">Then, hello data schema was linked toohello cluster model section, again created with an [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="2e2c3-138">I hello [köra R-skriptet] [ execute-r-script] används för hello klustermodellen hello webbtjänsten sedan använder funktionen hello ”k-means”, som är färdiga i hello [köra R-skriptet] [ execute-r-script] av Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-138">In hello [Execute R Script][execute-r-script] used for hello cluster model, hello web service then utilizes hello “k-means” function, which is prebuilt into hello [Execute R Script][execute-r-script] of Azure Machine Learning.</span></span>    

![Experiment flöde][3]

#### <a name="module-1"></a><span data-ttu-id="2e2c3-140">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="2e2c3-140">Module 1:</span></span>
    #Enter hello input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)

    maml.mapOutputPort("mydata");     


#### <a name="module-2"></a><span data-ttu-id="2e2c3-141">Modulen 2:</span><span class="sxs-lookup"><span data-stu-id="2e2c3-141">Module 2:</span></span>
    # Map 1-based optional input ports toovariables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("mydatafinal");


## <a name="limitations"></a><span data-ttu-id="2e2c3-142">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="2e2c3-142">Limitations</span></span>
<span data-ttu-id="2e2c3-143">Detta är ett väldigt enkelt exempel på en kluster-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-143">This is a very simple example of a clustering web service.</span></span> <span data-ttu-id="2e2c3-144">Som kan ses från hello exempelkod ovan, inga fel som fångas implementeras och hello antas allt är en kontinuerlig variabel (inga kategoriska funktioner tillåts), som hello tjänsten endast indata numeriska värden för närvarande hello hello skapas för den här webbplatsen tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-144">As can be seen from hello example code above, no error catching is implemented and hello service assumes everything is a continuous variable (no categorical features allowed), as hello service only inputs numeric values at hello time of hello creation of this web service.</span></span> <span data-ttu-id="2e2c3-145">Dessutom hello-tjänsten hanterar för närvarande begränsad datastorleken, på grund av toohello frågor och svar uppbyggnad hello webbtjänsten anropet och hello faktum att hello modell som passar varje gång hello webbtjänst anropas.</span><span class="sxs-lookup"><span data-stu-id="2e2c3-145">Also, hello service currently handles limited data size, due toohello request/response nature of hello web service call and hello fact that hello model is being fit every time hello web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="2e2c3-146">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="2e2c3-146">FAQ</span></span>
<span data-ttu-id="2e2c3-147">Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="2e2c3-147">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

