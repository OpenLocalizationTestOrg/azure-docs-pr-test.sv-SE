---
title: "(föråldrad) Modell - Azure-kluster | Microsoft Docs"
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
redirect_document_id: TRUE
ms.openlocfilehash: 7cbbbd6d4236dab638eb3051595a584557480841
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-cluster-model"></a><span data-ttu-id="4aaf6-103">(föråldrad) Ett kluster</span><span class="sxs-lookup"><span data-stu-id="4aaf6-103">(deprecated) Cluster Model</span></span>

> [!NOTE]
> <span data-ttu-id="4aaf6-104">Microsoft DataMarket dras och detta API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="4aaf6-105">Du hittar många användbara exempel experiment och API: er i den [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="4aaf6-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="4aaf6-106">Mer information om galleriet finns [resursen och identifiera resurser i Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="4aaf6-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="4aaf6-107">Hur kan vi för att förutsäga grupper av kredit cardholders beteenden för att minska risken av kostnad för kreditkort utfärdare?</span><span class="sxs-lookup"><span data-stu-id="4aaf6-107">How can we predict groups of credit cardholders’ behaviors in order to reduce the charge-off risk of credit card issuers?</span></span> <span data-ttu-id="4aaf6-108">Hur kan vi definiera grupper av person egenskaper för anställda för att kunna förbättra deras prestanda på jobbet?</span><span class="sxs-lookup"><span data-stu-id="4aaf6-108">How can we define groups of personality traits of employees in order to improve their performance at work?</span></span> <span data-ttu-id="4aaf6-109">Hur kan läkare klassificera patienter i grupper baserat på deras sjukdomar egenskaper?</span><span class="sxs-lookup"><span data-stu-id="4aaf6-109">How can doctors classify patients into groups based on the characteristics of their diseases?</span></span> <span data-ttu-id="4aaf6-110">I princip besvaras alla frågor via klustret analys.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-110">In principle, all of these questions can be answered through cluster analysis.</span></span>   

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="4aaf6-111">Klustret analys klassificerar en uppsättning observationer i två eller flera ömsesidigt uteslutande okänd grupper baserat på kombinationer av variabler.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-111">Cluster analysis classifies a set of observations into two or more mutually exclusive unknown groups based on combinations of variables.</span></span> <span data-ttu-id="4aaf6-112">Syftet med klustret analys är att identifiera ett system för att organisera observationer, vanligtvis personer eller deras egenskaper i grupper där medlemmar i grupperna har egenskaper gemensamt.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-112">The purpose of cluster analysis is to discover a system of organizing observations, usually people or their characteristics, into groups, where members of the groups share properties in common.</span></span> <span data-ttu-id="4aaf6-113">Detta [service](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) använder K-Means metoder, en vanliga klustring teknik till valfri klusterdata i grupper.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-113">This [service](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) uses the K-Means methodology, a commonly used clustering technique, to cluster arbitrary data into groups.</span></span> <span data-ttu-id="4aaf6-114">Den här webbtjänsten hämtar data och antalet k kluster som indata och producerar förutsägelser som k-grupper som varje observationer tillhör.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-114">This web service takes the data and the number of k clusters as input, and produces predictions of which of the k groups to which each observations belongs.</span></span> 

> <span data-ttu-id="4aaf6-115">Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-115">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="4aaf6-116">Men syftet med webbtjänsten är också som fungerar som ett exempel på hur Azure Machine Learning kan användas för att skapa webbtjänster ovanpå R-koden.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-116">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="4aaf6-117">Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-117">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="4aaf6-118">Webbtjänsten kan sedan publiceras på Azure Marketplace och används av användare och enheter över hela världen med inga infrastrukturinställningar av författaren till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-118">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="4aaf6-119">Användning av web service</span><span class="sxs-lookup"><span data-stu-id="4aaf6-119">Consumption of web service</span></span>
<span data-ttu-id="4aaf6-120">Den här webbtjänsten grupperas data till en uppsättning k grupper och matar ut grupptilldelning för varje rad.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-120">This web service groups the data into a set of k groups and outputs the group assignment for each row.</span></span> <span data-ttu-id="4aaf6-121">Webbtjänst att användaren måste ange data som en sträng där rader avgränsas med kommatecken (,) och kolumner avgränsade med semikolon (;).</span><span class="sxs-lookup"><span data-stu-id="4aaf6-121">The web service expects the end user to input data as a string where rows are separated by commas (,) and columns are separated by semicolons (;).</span></span> <span data-ttu-id="4aaf6-122">Webbtjänst 1 rad i taget.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-122">The web service expects 1 row at a time.</span></span> <span data-ttu-id="4aaf6-123">En exempel-datauppsättning kan se ut så här:</span><span class="sxs-lookup"><span data-stu-id="4aaf6-123">An example dataset could look like this:</span></span>

![Exempeldata][1]

<span data-ttu-id="4aaf6-125">Anta att du vill dela informationen i 3 ömsesidigt uteslutande grupper.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-125">Suppose the user wanted to separate this data into 3 mutually exclusive groups.</span></span> <span data-ttu-id="4aaf6-126">Indata för ovan datauppsättningen skulle vara följande: value = ”10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4”; k = ”3”.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-126">The data input for the above dataset would be the following: value = “10;5;2,18;1;6,7;5;5,22;3;4,12;2;1,10;3;4”; k=”3”.</span></span> <span data-ttu-id="4aaf6-127">Utdata är förväntade gruppmedlemskap för var och en av raderna.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-127">The output is the predicted group membership for each of the rows.</span></span>

> <span data-ttu-id="4aaf6-128">Den här tjänsten, är som finns på Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-128">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="4aaf6-129">Det finns flera olika sätt att använda tjänsten automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).</span><span class="sxs-lookup"><span data-stu-id="4aaf6-129">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="4aaf6-130">Startar C#-kod för web service användning:</span><span class="sxs-lookup"><span data-stu-id="4aaf6-130">Starting C# code for web service consumption:</span></span>
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




## <a name="creation-of-web-service"></a><span data-ttu-id="4aaf6-131">Skapandet av web service</span><span class="sxs-lookup"><span data-stu-id="4aaf6-131">Creation of web service</span></span>
> <span data-ttu-id="4aaf6-132">Den här webbtjänsten har skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-132">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="4aaf6-133">För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="4aaf6-133">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="4aaf6-134">Nedan visas en skärmbild av experiment som skapade web service och exempel koden för alla moduler i experimentet.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-134">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="4aaf6-135">I Azure Machine Learning skapas ett nytt tomt experiment skapades från två [köra R-skriptet] [ execute-r-script] moduler hämtas till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-135">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules pulled onto the workspace.</span></span> <span data-ttu-id="4aaf6-136">Dataschemat har skapats med en enkel [köra R-skriptet][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="4aaf6-136">The data schema was created with a simple [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="4aaf6-137">Sedan dataschemat var kopplad till avsnittet cluster modellen skapas igen med en [köra R-skriptet][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="4aaf6-137">Then, the data schema was linked to the cluster model section, again created with an [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="4aaf6-138">I den [köra R-skriptet] [ execute-r-script] används för klustret modellen webbtjänsten sedan använder funktionen ”k-means”, som är färdiga i den [köra R-skriptet] [ execute-r-script] av Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-138">In the [Execute R Script][execute-r-script] used for the cluster model, the web service then utilizes the “k-means” function, which is prebuilt into the [Execute R Script][execute-r-script] of Azure Machine Learning.</span></span>    

![Experiment flöde][3]

#### <a name="module-1"></a><span data-ttu-id="4aaf6-140">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="4aaf6-140">Module 1:</span></span>
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)

    maml.mapOutputPort("mydata");     


#### <a name="module-2"></a><span data-ttu-id="4aaf6-141">Modulen 2:</span><span class="sxs-lookup"><span data-stu-id="4aaf6-141">Module 2:</span></span>
    # Map 1-based optional input ports to variables
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

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");


## <a name="limitations"></a><span data-ttu-id="4aaf6-142">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="4aaf6-142">Limitations</span></span>
<span data-ttu-id="4aaf6-143">Detta är ett väldigt enkelt exempel på en kluster-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-143">This is a very simple example of a clustering web service.</span></span> <span data-ttu-id="4aaf6-144">Som kan ses från exempelkoden ovan, inga fel som fångas implementeras och tjänsten förutsätter att allt är en kontinuerlig variabel (inga kategoriska funktioner tillåts), som tjänsten endast indata numeriska värden vid tidpunkten för skapandet av den här webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-144">As can be seen from the example code above, no error catching is implemented and the service assumes everything is a continuous variable (no categorical features allowed), as the service only inputs numeric values at the time of the creation of this web service.</span></span> <span data-ttu-id="4aaf6-145">Dessutom hanterar tjänsten för närvarande begränsad datastorleken förfallodatum för frågor och svar art webbtjänstanropet och det faktum att modellen som passar varje gång som webbtjänsten har anropats.</span><span class="sxs-lookup"><span data-stu-id="4aaf6-145">Also, the service currently handles limited data size, due to the request/response nature of the web service call and the fact that the model is being fit every time the web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="4aaf6-146">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="4aaf6-146">FAQ</span></span>
<span data-ttu-id="4aaf6-147">Vanliga frågor om förbrukningen av webbtjänst eller publicering på Azure Marketplace finns [här](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="4aaf6-147">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

