---
title: "aaa(deprecated) binära klassificerare - Azure | Microsoft Docs"
description: "(föråldrad) Binär klassificerare"
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 8045038a-9dcf-44b9-a6de-7f1f8e791575
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
ms.openlocfilehash: 0496fcec9952ca243270caf67f55fe191b2dc9f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-binary-classifier"></a><span data-ttu-id="48518-103">(föråldrad) Binär klassificerare</span><span class="sxs-lookup"><span data-stu-id="48518-103">(deprecated) Binary Classifier</span></span>

> [!NOTE]
> <span data-ttu-id="48518-104">hello Microsoft DataMarket dras och detta API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="48518-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="48518-105">Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="48518-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="48518-106">Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="48518-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="48518-107">Anta att du har en datauppsättning och vill toopredict en binär beroende variabel baserat på hello oberoende variabler.</span><span class="sxs-lookup"><span data-stu-id="48518-107">Suppose you have a dataset and would like toopredict a binary dependent variable based on hello independent variables.</span></span> <span data-ttu-id="48518-108">'Logistic Regression' är en populär statistiska teknik som används för sådana förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="48518-108">‘Logistic Regression’ is a popular statistical technique used for such predictions.</span></span> <span data-ttu-id="48518-109">Hello beroende variabel är här binär eller dichotomous och p är hello sannolikheten för förekomst av hello egenskap av intresse.</span><span class="sxs-lookup"><span data-stu-id="48518-109">Here hello dependent variable is binary or dichotomous, and p is hello probability of presence of hello characteristic of interest.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="48518-110">Ett enkelt scenario kan vara där en forskare försöker toopredict om potentiella student är sannolikt tooaccept en åtkomstkontroll erbjudande tooa university baserat på informationen (GPA i gymnasiet, family intäkter, fast status för kön).</span><span class="sxs-lookup"><span data-stu-id="48518-110">A simple scenario could be where a researcher is trying toopredict whether a prospective student is likely tooaccept an admission offer tooa university based on information (GPA in high school, family income, resident state, gender).</span></span> <span data-ttu-id="48518-111">hello förväntade resultatet är hello sannolikheten för hello potentiella student acceptera hello åtkomstkontroll erbjudande.</span><span class="sxs-lookup"><span data-stu-id="48518-111">hello predicted outcome is hello probability of hello prospective student accepting hello admission offer.</span></span> <span data-ttu-id="48518-112">Detta [webbtjänsten](https://datamarket.azure.com/dataset/aml_labs/log_regression) passar hello logistic regression modelldata toohello och utdata hello sannolikhetsvärdet (y) för varje hello observationer i hello data.</span><span class="sxs-lookup"><span data-stu-id="48518-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/log_regression) fits hello logistic regression model toohello data and outputs hello probability value (y) for each of hello observations in hello data.</span></span>  

> <span data-ttu-id="48518-113">Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="48518-113">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="48518-114">Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden.</span><span class="sxs-lookup"><span data-stu-id="48518-114">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="48518-115">Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="48518-115">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="48518-116">hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.</span><span class="sxs-lookup"><span data-stu-id="48518-116">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="48518-117">Användning av web service</span><span class="sxs-lookup"><span data-stu-id="48518-117">Consumption of web service</span></span>
<span data-ttu-id="48518-118">Den här web service ger hello förutsade värdena för hello beroende variabel baserat på hello oberoende variabler för alla hello observationer.</span><span class="sxs-lookup"><span data-stu-id="48518-118">This web service gives hello predicted values of hello dependent variable based on hello independent variables for all of hello observations.</span></span> <span data-ttu-id="48518-119">hello webbtjänst hello slutanvändaren tooinput data som en sträng där rader avgränsas med kommatecken (,) och kolumner avgränsade med semikolon (;).</span><span class="sxs-lookup"><span data-stu-id="48518-119">hello web service expects hello end user tooinput data as a string where rows are separated by comma (,) and columns are separated by semicolon (;).</span></span> <span data-ttu-id="48518-120">hello webbtjänsten förväntar sig 1 rad i taget och förväntar sig hello första kolumnen toobe hello beroende variabel.</span><span class="sxs-lookup"><span data-stu-id="48518-120">hello web service expects 1 row at a time and expects hello first column toobe hello dependent variable.</span></span> <span data-ttu-id="48518-121">En exempel-datauppsättning kan se ut så här:</span><span class="sxs-lookup"><span data-stu-id="48518-121">An example dataset could look like this:</span></span>

![Exempeldata][1]

<span data-ttu-id="48518-123">Måste ange observationer utan en beroende variabel som ”saknas” för den.</span><span class="sxs-lookup"><span data-stu-id="48518-123">Observations without a dependent variable should be input as “NA” for y.</span></span> <span data-ttu-id="48518-124">hello data indata för hello ovan dataset skulle vara hello följande sträng: ”1; 5; 2,1; 1; 6,0; 5.3; 2.1,0; 5; 5,0; 3; 4,1; 2, 1, NA; 3; 4”.</span><span class="sxs-lookup"><span data-stu-id="48518-124">hello data input for hello above dataset would be hello following string: “1;5;2,1;1;6,0;5.3;2.1,0;5;5,0;3;4,1;2;1,NA;3;4”.</span></span> <span data-ttu-id="48518-125">hello utdata hello förutsagt värde för varje hello rader baseras på hello oberoende variabler.</span><span class="sxs-lookup"><span data-stu-id="48518-125">hello output is hello predicted value for each of hello rows based on hello independent variables.</span></span> 

> <span data-ttu-id="48518-126">Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder.</span><span class="sxs-lookup"><span data-stu-id="48518-126">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="48518-127">Det finns flera olika sätt att konsumera hello service automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx)).</span><span class="sxs-lookup"><span data-stu-id="48518-127">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="48518-128">Startar C#-kod för web service användning:</span><span class="sxs-lookup"><span data-stu-id="48518-128">Starting C# code for web service consumption:</span></span>
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


## <a name="creation-of-web-service"></a><span data-ttu-id="48518-129">Skapandet av web service</span><span class="sxs-lookup"><span data-stu-id="48518-129">Creation of web service</span></span>
> <span data-ttu-id="48518-130">Den här webbtjänsten har skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="48518-130">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="48518-131">För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="48518-131">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="48518-132">Nedan visas en skärmbild av hello experiment som skapade hello web service och exempel-kod för varje hello moduler i hello experiment.</span><span class="sxs-lookup"><span data-stu-id="48518-132">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="48518-133">I Azure Machine Learning skapas ett nytt tomt experiment skapades från två [köra R-skriptet] [ execute-r-script] moduler hämtas till hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="48518-133">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules pulled onto hello workspace.</span></span> <span data-ttu-id="48518-134">Den här webbtjänsten körs ett Azure Machine Learning-experiment med en underliggande R-skriptet.</span><span class="sxs-lookup"><span data-stu-id="48518-134">This web service runs an Azure Machine Learning experiment with an underlying R script.</span></span> <span data-ttu-id="48518-135">Det finns 2 delar toothis experimentera: schemadefinition, och utbildning modellen + bedömningen.</span><span class="sxs-lookup"><span data-stu-id="48518-135">There are 2 parts toothis experiment: schema definition, and training model + scoring.</span></span> <span data-ttu-id="48518-136">hello första modulen definierar hello förväntade strukturen för hello inkommande dataset, där hello första variabeln är hello beroende variabel och hello återstående variabler är oberoende.</span><span class="sxs-lookup"><span data-stu-id="48518-136">hello first module defines hello expected structure of hello input dataset, where hello first variable is hello dependent variable and hello remaining variables are independent.</span></span> <span data-ttu-id="48518-137">andra hello-modul passar en allmän logistic regressionsmodell för hello indata.</span><span class="sxs-lookup"><span data-stu-id="48518-137">hello second module fits a generic logistic regression model for hello input data.</span></span>    

![Experiment flöde][2]

#### <a name="module-1"></a><span data-ttu-id="48518-139">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="48518-139">Module 1:</span></span>
    #Schema definition  
    data <- data.frame(value = "1;2;3,1;5;6,0;8;9", stringsAsFactors=FALSE) 
    maml.mapOutputPort("data");  

#### <a name="module-2"></a><span data-ttu-id="48518-140">Modulen 2:</span><span class="sxs-lookup"><span data-stu-id="48518-140">Module 2:</span></span>
    #GLM modeling   
    data <- maml.mapInputPort(1) # class: data.frame  

    data.split <- strsplit(data[1,1], ",")[[1]] 
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE) 
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE) 
    data.split <- as.data.frame(t(data.split)) data.split <- 
    data.matrix(data.split) 
    data.split <- data.frame(data.split) 

    model <- glm(data.split$V1 ~., family='binomial', data=data.split)  
    out <- data.frame(predict(model,data.split, type="response")) 
    pred1 <- as.data.frame(out) 
    group <- array(1:nrow(pred1)) 
    for (i in 1:nrow(pred1))  
        {
        if(as.numeric(pred1[i,])>0.5) {group[i]=1} 
        else {group[i]=0}
        } 
    pred2 <- as.data.frame(group) 
    maml.mapOutputPort("pred2");  


## <a name="limitations"></a><span data-ttu-id="48518-141">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="48518-141">Limitations</span></span>
<span data-ttu-id="48518-142">Detta är ett väldigt enkelt exempel på en binär klassificering webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="48518-142">This is a very simple example of a binary classification web service.</span></span> <span data-ttu-id="48518-143">Som kan ses från hello exempelkod ovan, inga fel som fångas implementeras och hello antas allt är en binär kontinuerlig variabel (inga kategoriska funktioner tillåts), som hello tjänsten endast indata numeriska värden för närvarande hello hello skapades detta webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="48518-143">As can be seen from hello example code above, no error catching is implemented and hello service assumes everything is a binary/continuous variable (no categorical features allowed), as hello service only inputs numeric values at hello time of hello creation of this web service.</span></span> <span data-ttu-id="48518-144">Dessutom hello-tjänsten hanterar för närvarande begränsad datastorleken, på grund av toohello frågor och svar uppbyggnad hello webbtjänsten anropet och hello faktum att hello modell som passar varje gång hello webbtjänst anropas.</span><span class="sxs-lookup"><span data-stu-id="48518-144">Also, hello service currently handles limited data size, due toohello request/response nature of hello web service call and hello fact that hello model is being fit every time hello web service is called.</span></span> 

## <a name="faq"></a><span data-ttu-id="48518-145">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="48518-145">FAQ</span></span>
<span data-ttu-id="48518-146">Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="48518-146">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-binary-classifier/binary1.png
[2]: ./media/machine-learning-r-csharp-binary-classifier/binary2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

