---
title: "aaa(deprecated) Multivariate linjär Regression - Azure | Microsoft Docs"
description: "(föråldrad) Multivariate linjär Regression"
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 2fb78220-ced9-4564-a439-9e5df6772994
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: jaymathe
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 0ff7221cd06c0ef059b0c5bf327016588174dcfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-multivariate-linear-regression"></a><span data-ttu-id="d60bc-103">(föråldrad) Multivariate linjär Regression</span><span class="sxs-lookup"><span data-stu-id="d60bc-103">(deprecated) Multivariate Linear Regression</span></span>

> [!NOTE]
> <span data-ttu-id="d60bc-104">hello Microsoft DataMarket dras och detta API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="d60bc-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="d60bc-105">Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="d60bc-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="d60bc-106">Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="d60bc-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="d60bc-107">Anta att du har en datauppsättning och skulle som tooquickly förutsäga beroende-variabeln (y) för varje enskild person (i) baserat på oberoende variabler.</span><span class="sxs-lookup"><span data-stu-id="d60bc-107">Suppose you have a dataset and would like tooquickly predict a dependent variable (y) for each individual (i) based on independent variables.</span></span> <span data-ttu-id="d60bc-108">Linjär regression är en populär statistiska teknik som används för sådana förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="d60bc-108">Linear regression is a popular statistical technique used for such predictions.</span></span> <span data-ttu-id="d60bc-109">Här antas hello beroende variabel y toobe ett kontinuerligt värde.</span><span class="sxs-lookup"><span data-stu-id="d60bc-109">Here hello dependent variable y is assumed toobe a continuous value.</span></span>  

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="d60bc-110">Ett enkelt scenario kan vara där hello forskare försöker toopredict hello vikten för en enskild (y) baserat på deras höjd (x).</span><span class="sxs-lookup"><span data-stu-id="d60bc-110">A simple scenario could be where hello researcher is trying toopredict hello weight of an individual (y) based on their height (x).</span></span> <span data-ttu-id="d60bc-111">Ett mer avancerat scenario kan vara där hello forskare innehåller ytterligare information för hello enskilda (till exempel vikt, kön, RAS) och försöker toopredict hello vikten av hello person.</span><span class="sxs-lookup"><span data-stu-id="d60bc-111">A more advanced scenario could be where hello researcher has additional information for hello individual (such as weight, gender, race) and attempts toopredict hello weight of hello individual.</span></span> <span data-ttu-id="d60bc-112">Detta [webbtjänsten](https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) passar hello linjär regression modelldata toohello och utdata hello förutsägelsevärdet (y) för varje hello observationer i hello data.</span><span class="sxs-lookup"><span data-stu-id="d60bc-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) fits hello linear regression model toohello data and outputs hello predicted value (y) for each of hello observations in hello data.</span></span>

> <span data-ttu-id="d60bc-113">Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="d60bc-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="d60bc-114">Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden.</span><span class="sxs-lookup"><span data-stu-id="d60bc-114">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="d60bc-115">Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="d60bc-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="d60bc-116">hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.</span><span class="sxs-lookup"><span data-stu-id="d60bc-116">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="d60bc-117">Användning av web service</span><span class="sxs-lookup"><span data-stu-id="d60bc-117">Consumption of web service</span></span>
<span data-ttu-id="d60bc-118">Den här web service ger hello förutsade värdena för hello beroende variabel baserat på hello oberoende variabler för alla hello observationer.</span><span class="sxs-lookup"><span data-stu-id="d60bc-118">This web service gives hello predicted values of hello dependent variable based on hello independent variables for all of hello observations.</span></span> <span data-ttu-id="d60bc-119">hello webbtjänst hello slutanvändaren tooinput data som en sträng där rader avgränsas med kommatecken (,) och kolumner avgränsade med semikolon (;).</span><span class="sxs-lookup"><span data-stu-id="d60bc-119">hello web service expects hello end user tooinput data as a string where rows are separated by commas (,) and columns are separated by semicolons (;).</span></span> <span data-ttu-id="d60bc-120">hello webbtjänsten förväntar sig 1 rad i taget och förväntar sig hello första kolumnen toobe hello beroende variabel.</span><span class="sxs-lookup"><span data-stu-id="d60bc-120">hello web service expects 1 row at a time and expects hello first column toobe hello dependent variable.</span></span> <span data-ttu-id="d60bc-121">En exempel-datauppsättning kan se ut så här:</span><span class="sxs-lookup"><span data-stu-id="d60bc-121">An example dataset could look like this:</span></span>

![Exempeldata][1]

<span data-ttu-id="d60bc-123">Måste ange observationer utan en beroende variabel som ”saknas” för den.</span><span class="sxs-lookup"><span data-stu-id="d60bc-123">Observations without a dependent variable should be input as “NA” for y.</span></span> <span data-ttu-id="d60bc-124">hello data indata för hello ovan dataset skulle vara hello följande sträng ”: 10, 5, 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2, 1, NA, 3, 4”.</span><span class="sxs-lookup"><span data-stu-id="d60bc-124">hello data input for hello above dataset would be hello following string: “10;5;2,18;1;6,6;5.3;2.1,7;5;5,22;3;4,12;2;1,NA;3;4”.</span></span> <span data-ttu-id="d60bc-125">hello utdata hello förutsagt värde för varje hello rader baseras på hello oberoende variabler.</span><span class="sxs-lookup"><span data-stu-id="d60bc-125">hello output is hello predicted value for each of hello rows based on hello independent variables.</span></span> 

> <span data-ttu-id="d60bc-126">Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder.</span><span class="sxs-lookup"><span data-stu-id="d60bc-126">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="d60bc-127">Det finns flera olika sätt att konsumera hello service automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx)).</span><span class="sxs-lookup"><span data-stu-id="d60bc-127">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="d60bc-128">Startar C#-kod för web service användning:</span><span class="sxs-lookup"><span data-stu-id="d60bc-128">Starting C# code for web service consumption:</span></span>
    public class Input
    {
            public string value;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




## <a name="creation-of-web-service"></a><span data-ttu-id="d60bc-129">Skapandet av web service</span><span class="sxs-lookup"><span data-stu-id="d60bc-129">Creation of web service</span></span>
> <span data-ttu-id="d60bc-130">Den här webbtjänsten har skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d60bc-130">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="d60bc-131">För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="d60bc-131">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="d60bc-132">Nedan visas en skärmbild av hello experiment som skapade hello web service och exempel-kod för varje hello moduler i hello experiment.</span><span class="sxs-lookup"><span data-stu-id="d60bc-132">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="d60bc-133">I Azure Machine Learning skapas ett nytt tomt experiment skapades från två [köra R-skriptet] [ execute-r-script] moduler som hämtades till hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="d60bc-133">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules were pulled onto hello workspace.</span></span> <span data-ttu-id="d60bc-134">Den här webbtjänsten körs ett Azure Machine Learning-experiment med en underliggande R-skriptet.</span><span class="sxs-lookup"><span data-stu-id="d60bc-134">This web service runs an Azure Machine Learning experiment with an underlying R script.</span></span> <span data-ttu-id="d60bc-135">Det finns 2 delar toothis experimentera: schemadefinition, och utbildning modellen + bedömningen.</span><span class="sxs-lookup"><span data-stu-id="d60bc-135">There are 2 parts toothis experiment: schema definition, and training model + scoring.</span></span> <span data-ttu-id="d60bc-136">hello första modulen definierar hello förväntade strukturen för hello inkommande dataset, där hello första variabeln är hello beroende variabel och hello återstående variabler är oberoende.</span><span class="sxs-lookup"><span data-stu-id="d60bc-136">hello first module defines hello expected structure of hello input dataset, where hello first variable is hello dependent variable and hello remaining variables are independent.</span></span> <span data-ttu-id="d60bc-137">andra hello-modul passar en allmän linjär regressionsmodell för hello indata.</span><span class="sxs-lookup"><span data-stu-id="d60bc-137">hello second module fits a generic linear regression model for hello input data.</span></span>  

![Experiment flöde][3]

#### <a name="module-1"></a><span data-ttu-id="d60bc-139">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="d60bc-139">Module 1:</span></span>
#### <a name="schema-definition"></a><span data-ttu-id="d60bc-140">Schemadefinitionen</span><span class="sxs-lookup"><span data-stu-id="d60bc-140">Schema definition</span></span>
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

#### <a name="module-2"></a><span data-ttu-id="d60bc-141">Modulen 2:</span><span class="sxs-lookup"><span data-stu-id="d60bc-141">Module 2:</span></span>
#### <a name="lm-modeling"></a><span data-ttu-id="d60bc-142">LM-modellering</span><span class="sxs-lookup"><span data-stu-id="d60bc-142">LM modeling</span></span>
    data <- maml.mapInputPort(1) # class: data.frame  

    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  

## <a name="limitations"></a><span data-ttu-id="d60bc-143">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="d60bc-143">Limitations</span></span>
<span data-ttu-id="d60bc-144">Detta är ett väldigt enkelt exempel på en flera linjär regression webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="d60bc-144">This is a very simple example of a multiple linear regression web service.</span></span> <span data-ttu-id="d60bc-145">Som kan ses från hello exempelkod ovan, inga fel som fångas implementeras och hello antas allt är en kontinuerlig variabel (inga kategoriska funktioner tillåts), som hello tjänsten endast indata numeriska värden för närvarande hello hello skapas för den här webbplatsen tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d60bc-145">As can be seen from hello example code above, no error catching is implemented and hello service assumes everything is a continuous variable (no categorical features allowed), as hello service only inputs numeric values at hello time of hello creation of this web service.</span></span> <span data-ttu-id="d60bc-146">Dessutom hello-tjänsten hanterar för närvarande begränsad datastorleken, på grund av toohello frågor och svar uppbyggnad hello webbtjänsten anropet och hello faktum att hello modell som passar varje gång hello webbtjänst anropas.</span><span class="sxs-lookup"><span data-stu-id="d60bc-146">Also, hello service currently handles limited data size, due toohello request/response nature of hello web service call and hello fact that hello model is being fit every time hello web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="d60bc-147">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="d60bc-147">FAQ</span></span>
<span data-ttu-id="d60bc-148">Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="d60bc-148">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

