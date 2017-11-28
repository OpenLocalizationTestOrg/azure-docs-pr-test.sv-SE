---
title: "aaa(deprecated) överlevnad analys med Azure Machine Learning | Microsoft Docs"
description: "(föråldrad) Överlevnadsguide analys händelse förekomsten sannolikhet"
services: machine-learning
documentationcenter: 
author: zhangya
manager: jhubbard
editor: cgronlun
ms.assetid: a142fc45-cdfb-4971-910e-05dab8bc699e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: zhangya
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: af946d8df5ba650a9d74fbabbe3b15d3a07dd508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-survival-analysis"></a><span data-ttu-id="d462c-103">(föråldrad) Överlevnadsguide analys</span><span class="sxs-lookup"><span data-stu-id="d462c-103">(deprecated) Survival Analysis</span></span>

> [!NOTE]
> <span data-ttu-id="d462c-104">hello Microsoft DataMarket dras och detta API är inaktuell.</span><span class="sxs-lookup"><span data-stu-id="d462c-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="d462c-105">Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="d462c-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="d462c-106">Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="d462c-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="d462c-107">I många fall är hello resultatet under assessment hello tid tooan händelse av intresse.</span><span class="sxs-lookup"><span data-stu-id="d462c-107">Under many scenarios, hello main outcome under assessment is hello time tooan event of interest.</span></span> <span data-ttu-id="d462c-108">Med andra ord hello frågan ”när den här händelsen inträffar”?</span><span class="sxs-lookup"><span data-stu-id="d462c-108">In other words, hello question “when this event will occur?”</span></span> <span data-ttu-id="d462c-109">begärs.</span><span class="sxs-lookup"><span data-stu-id="d462c-109">is asked.</span></span> <span data-ttu-id="d462c-110">Exempel, bör du situationer där hello data beskriver hello förfluten tid (dagar, år, hittills utfört, etc.) tills hello intressanta (sjukdom relapse, PhD grad togs emot, bromsar pad fel) inträffar.</span><span class="sxs-lookup"><span data-stu-id="d462c-110">As examples, consider situations where hello data describes hello elapsed time (days, years, mileage, etc.) until hello event of interest (disease relapse, PhD degree received, brake pad failure) occurs.</span></span> <span data-ttu-id="d462c-111">Varje instans i hello data representerar ett visst objekt (en öppen, student, en bil, etc.).</span><span class="sxs-lookup"><span data-stu-id="d462c-111">Each instance in hello data represents a specific object (a patient, a student, a car, etc.).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="d462c-112">Detta [webbtjänsten](https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) svar hello frågan ”vad är hello sannolikheten att hello händelse intressanta sker med tiden n för objektet x”?</span><span class="sxs-lookup"><span data-stu-id="d462c-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) answers hello question “what is hello probability that hello event of interest will occur by time n for object x?”</span></span> <span data-ttu-id="d462c-113">Den här webbtjänsten kan användare toosupply tootrain hello datamodellen genom att tillhandahålla en överlevnad analysmodell och testa den.</span><span class="sxs-lookup"><span data-stu-id="d462c-113">By providing a survival analysis model, this web service enables users toosupply data tootrain hello model and test it.</span></span> <span data-ttu-id="d462c-114">hello huvudsakliga tema hello experiment är toomodel hello hello förfluten tid tills hello intressanta inträffar.</span><span class="sxs-lookup"><span data-stu-id="d462c-114">hello main theme of hello experiment is toomodel hello length of hello elapsed time until hello event of interest occurs.</span></span> 

> <span data-ttu-id="d462c-115">Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="d462c-115">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="d462c-116">Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden.</span><span class="sxs-lookup"><span data-stu-id="d462c-116">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="d462c-117">Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="d462c-117">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="d462c-118">hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.</span><span class="sxs-lookup"><span data-stu-id="d462c-118">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="d462c-119">Användning av web service</span><span class="sxs-lookup"><span data-stu-id="d462c-119">Consumption of web service</span></span>
<span data-ttu-id="d462c-120">hello indata schemat för hello webbtjänsten visas i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="d462c-120">hello input data schema of hello web service is shown in hello following table.</span></span> <span data-ttu-id="d462c-121">Sex uppgifter krävs som hello indata: träningsdata, tester data, tid intressanta, hello index för ”time” dimension, hello index ”event” dimension och hello datatyper (kontinuerlig eller faktor).</span><span class="sxs-lookup"><span data-stu-id="d462c-121">Six pieces of information are needed as hello input: training data, testing data, time of interest, hello index of "time" dimension, hello index of "event" dimension, and hello variable types (continuous or factor).</span></span> <span data-ttu-id="d462c-122">Hej träningsdata representeras av en sträng, där hello rader avgränsade med kommatecken och hello kolumner avgränsade med semikolon.</span><span class="sxs-lookup"><span data-stu-id="d462c-122">hello training data is represented with a string, where hello rows are separated by comma, and hello columns are separated by semicolon.</span></span> <span data-ttu-id="d462c-123">hello antal funktioner hello data är flexibel.</span><span class="sxs-lookup"><span data-stu-id="d462c-123">hello number of features of hello data is flexible.</span></span> <span data-ttu-id="d462c-124">Alla hello-element i hello inmatade strängen måste vara numerisk.</span><span class="sxs-lookup"><span data-stu-id="d462c-124">All hello elements in hello input string must be numeric.</span></span> <span data-ttu-id="d462c-125">Hej träningsdata indikerar hello ”time” dimension hello antalet tidsenheter (dagar, år, hittills utfört, etc.) gått sedan hello startpunkten för hello studera (en tålamod tar emot läkemedel behandling program, en student första PhD undersökning, en bil startar toobe drivs, etc.) tills hello intressanta (hello tålamod returnerar toodrug användning, hello student erhålla hello PhD grad, hello bil med bromsar pad fel osv) inträffar.</span><span class="sxs-lookup"><span data-stu-id="d462c-125">In hello training data, hello “time” dimension indicates hello number of time units (days, years, mileage, etc.) elapsed since hello starting point of hello study (a patient receiving drug treatment programs, a student starting PhD study, a car starting toobe driven, etc.) until hello event of interest (hello patient returning toodrug usage, hello student obtaining hello PhD degree, hello car having brake pad failure, etc.) occurs.</span></span> <span data-ttu-id="d462c-126">Hej ”event” dimension anger om hello händelse intressanta hello slutet av hello undersökning.</span><span class="sxs-lookup"><span data-stu-id="d462c-126">hello “event” dimension indicates whether hello event of interest occurs at hello end of hello study.</span></span> <span data-ttu-id="d462c-127">Värdet ”händelse = 1” innebär att hello händelse intressanta inträffar vid hello tidpunkt som anges av hello ”time” dimension. ”händelse = 0” innebär att hello händelse av intresse har skett genom hello tidpunkt som anges av hello ”time” dimension.</span><span class="sxs-lookup"><span data-stu-id="d462c-127">A value of “event=1” means that hello event of interest occurs at hello time indicated by hello “time” dimension; “event=0” means that hello event of interest has not occurred by hello time indicated by hello “time” dimension.</span></span>

* <span data-ttu-id="d462c-128">trainingdata - en teckensträng.</span><span class="sxs-lookup"><span data-stu-id="d462c-128">trainingdata - A character string.</span></span> <span data-ttu-id="d462c-129">Rader avgränsas med kommatecken och kolumner avgränsade med semikolon.</span><span class="sxs-lookup"><span data-stu-id="d462c-129">Rows are separated by comma, and columns are separated by semicolon.</span></span> <span data-ttu-id="d462c-130">Varje rad innehåller dimensionen ”time”, ”event” dimension och ge prognoser variabler.</span><span class="sxs-lookup"><span data-stu-id="d462c-130">Each row includes “time” dimension, “event” dimension, and predictor variables.</span></span>
* <span data-ttu-id="d462c-131">testingdata - en rad med data som innehåller ge prognoser variabler för ett visst objekt.</span><span class="sxs-lookup"><span data-stu-id="d462c-131">testingdata - One row of data that contains predictor variables for a particular object.</span></span>
* <span data-ttu-id="d462c-132">time_of_interest - hello förfluten tid vid intresse n.</span><span class="sxs-lookup"><span data-stu-id="d462c-132">time_of_interest - hello elapsed time of interest n.</span></span>
* <span data-ttu-id="d462c-133">index_time - hello kolumnindex hello ”” tidsdimension (med början från 1).</span><span class="sxs-lookup"><span data-stu-id="d462c-133">index_time - hello column index of hello “time” dimension (starting from 1).</span></span>
* <span data-ttu-id="d462c-134">index_event - hello kolumnindex för hello ”event” dimension (med början från 1).</span><span class="sxs-lookup"><span data-stu-id="d462c-134">index_event - hello column index of hello “event” dimension (starting from 1).</span></span>
* <span data-ttu-id="d462c-135">variable_types - en teckensträng med semikolon som avgränsare i den.</span><span class="sxs-lookup"><span data-stu-id="d462c-135">variable_types - A character string with semicolons as separators in it.</span></span> <span data-ttu-id="d462c-136">0 representerar kontinuerlig variabler och 1 representerar faktor variabler.</span><span class="sxs-lookup"><span data-stu-id="d462c-136">0 represents continuous variables and 1 represents factor variables.</span></span>

<span data-ttu-id="d462c-137">hello-utdata är hello sannolikheten för en händelse inträffar inom en viss tid.</span><span class="sxs-lookup"><span data-stu-id="d462c-137">hello output is hello probability of an event occurring by a specific time.</span></span> 

> <span data-ttu-id="d462c-138">Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder.</span><span class="sxs-lookup"><span data-stu-id="d462c-138">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="d462c-139">Det finns flera olika sätt att konsumera hello service automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)).</span><span class="sxs-lookup"><span data-stu-id="d462c-139">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)).</span></span> 

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="d462c-140">Startar C#-kod för web service användning:</span><span class="sxs-lookup"><span data-stu-id="d462c-140">Starting C# code for web service consumption:</span></span>
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




<span data-ttu-id="d462c-141">hello tolkningen av det här testet är som följer.</span><span class="sxs-lookup"><span data-stu-id="d462c-141">hello interpretation of this test is as follows.</span></span> <span data-ttu-id="d462c-142">Under förutsättning att hello målet hello data är toomodel hello förfluten tid tills hello returnera toodrug användning för hello patienter som tog emot ett hello två behandling program.</span><span class="sxs-lookup"><span data-stu-id="d462c-142">Assuming hello goal of hello data is toomodel hello elapsed time until hello return toodrug usage for hello patients who received one of hello two treatment programs.</span></span> <span data-ttu-id="d462c-143">Hej utdata hello web service läsningar: för patienter som 35 år, med tidigare antal behandling 2 gånger tar hello länge lokal behandling program, och med användning av både heroin och cocaine hello sannolikheten för att returnera toohello läkemedel användning är 95.64% av dag 500.</span><span class="sxs-lookup"><span data-stu-id="d462c-143">hello output of hello web service reads: for patients being 35 years old, having previous drug treatment 2 times, taking hello long residential treatment program, and with both heroin and cocaine usage, hello probability of returning toohello drug usage is 95.64% by day 500.</span></span>

## <a name="creation-of-web-service"></a><span data-ttu-id="d462c-144">Skapandet av web service</span><span class="sxs-lookup"><span data-stu-id="d462c-144">Creation of web service</span></span>
> <span data-ttu-id="d462c-145">Den här webbtjänsten har skapats med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d462c-145">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="d462c-146">För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="d462c-146">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="d462c-147">Nedan visas en skärmbild av hello experiment som skapade hello web service och exempel-kod för varje hello moduler i hello experiment.</span><span class="sxs-lookup"><span data-stu-id="d462c-147">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="d462c-148">I Azure Machine Learning skapas ett nytt tomt experiment skapades från två [köra R-skriptet] [ execute-r-script] moduler som hämtades till hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="d462c-148">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules were pulled onto hello workspace.</span></span> <span data-ttu-id="d462c-149">hello dataschemat skapades med en enkel [köra R-skriptet][execute-r-script], som definierar hello indata schemat för hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="d462c-149">hello data schema was created with a simple [Execute R Script][execute-r-script], which defines hello input data schema for hello web service.</span></span> <span data-ttu-id="d462c-150">Denna modul är sedan länkade toohello andra [köra R-skriptet] [ execute-r-script] module, som högre arbete.</span><span class="sxs-lookup"><span data-stu-id="d462c-150">This module is then linked toohello second [Execute R Script][execute-r-script] module, which does major work.</span></span> <span data-ttu-id="d462c-151">Den här modulen innehåller dataförbearbetning modellskapandet och förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="d462c-151">This module does data preprocessing, model building, and predictions.</span></span> <span data-ttu-id="d462c-152">I hello data förbearbetning steget hello indata som representeras av en lång sträng omvandlas och konverteras till en data-ram.</span><span class="sxs-lookup"><span data-stu-id="d462c-152">In hello data preprocessing step, hello input data represented by a long string is transformed and converted into a data frame.</span></span> <span data-ttu-id="d462c-153">Hello modellen byggnad steg installeras ett externt R-paket ”survival_2.37 7.zip” först för att utföra överlevnad analys.</span><span class="sxs-lookup"><span data-stu-id="d462c-153">In hello model building step, an external R package “survival_2.37-7.zip” is first installed for conducting survival analysis.</span></span> <span data-ttu-id="d462c-154">Hej ”coxph” funktionen utförs sedan efter en serie databearbetningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="d462c-154">Then hello “coxph” function is executed after a series data processing tasks.</span></span> <span data-ttu-id="d462c-155">hello information om hello ”coxph”-funktionen för överlevnad analys kan läsas hello R-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="d462c-155">hello details of hello “coxph” function for survival analysis can be read from hello R documentation.</span></span> <span data-ttu-id="d462c-156">En testnings-instans har angetts till hello tränad modell med hello ”surfit”-funktionen i hello förutsägelse steg och hello överlevnad kurva för det här testet instans skapas som ”kurvan” variabel.</span><span class="sxs-lookup"><span data-stu-id="d462c-156">In hello prediction step, a testing instance is supplied into hello trained model with hello “surfit” function, and hello survival curve for this testing instance is produced as “curve” variable.</span></span> <span data-ttu-id="d462c-157">Slutligen hämtas hello sannolikhet hello tid av intresse.</span><span class="sxs-lookup"><span data-stu-id="d462c-157">Finally, hello probability of hello time of interest is obtained.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="d462c-158">Experiment flöde:</span><span class="sxs-lookup"><span data-stu-id="d462c-158">Experiment flow:</span></span>
![Experiment flöde][1]

#### <a name="module-1"></a><span data-ttu-id="d462c-160">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="d462c-160">Module 1:</span></span>
    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"

    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data toooutput port

#### <a name="module-2"></a><span data-ttu-id="d462c-161">Modulen 2:</span><span class="sxs-lookup"><span data-stu-id="d462c-161">Module 2:</span></span>
    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare toobuild model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct hello execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get hello predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find hello event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results toosend tooweb service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




## <a name="limitations"></a><span data-ttu-id="d462c-162">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="d462c-162">Limitations</span></span>
<span data-ttu-id="d462c-163">Den här webbtjänsten kan ta endast numeriska värden som funktionen variabler (kolumner).</span><span class="sxs-lookup"><span data-stu-id="d462c-163">This web service can take only numerical values as feature variables (columns).</span></span> <span data-ttu-id="d462c-164">Hej ”” händelsekolumnen kan ta endast värdet 0 eller 1.</span><span class="sxs-lookup"><span data-stu-id="d462c-164">hello “event” column can take only value 0 or 1.</span></span> <span data-ttu-id="d462c-165">Hej ”time” kolumn måste toobe ett positivt heltal.</span><span class="sxs-lookup"><span data-stu-id="d462c-165">hello “time” column needs toobe a positive integer.</span></span>

## <a name="faq"></a><span data-ttu-id="d462c-166">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="d462c-166">FAQ</span></span>
<span data-ttu-id="d462c-167">Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="d462c-167">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

