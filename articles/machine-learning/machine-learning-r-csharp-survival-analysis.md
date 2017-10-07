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
# <a name="deprecated-survival-analysis"></a>(föråldrad) Överlevnadsguide analys

> [!NOTE]
> hello Microsoft DataMarket dras och detta API är inaktuell. 
> 
> Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

I många fall är hello resultatet under assessment hello tid tooan händelse av intresse. Med andra ord hello frågan ”när den här händelsen inträffar”? begärs. Exempel, bör du situationer där hello data beskriver hello förfluten tid (dagar, år, hittills utfört, etc.) tills hello intressanta (sjukdom relapse, PhD grad togs emot, bromsar pad fel) inträffar. Varje instans i hello data representerar ett visst objekt (en öppen, student, en bil, etc.).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Detta [webbtjänsten](https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) svar hello frågan ”vad är hello sannolikheten att hello händelse intressanta sker med tiden n för objektet x”? Den här webbtjänsten kan användare toosupply tootrain hello datamodellen genom att tillhandahålla en överlevnad analysmodell och testa den. hello huvudsakliga tema hello experiment är toomodel hello hello förfluten tid tills hello intressanta inträffar. 

> Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator. Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden. Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst. hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.  
> 
> 

## <a name="consumption-of-web-service"></a>Användning av web service
hello indata schemat för hello webbtjänsten visas i hello i den följande tabellen. Sex uppgifter krävs som hello indata: träningsdata, tester data, tid intressanta, hello index för ”time” dimension, hello index ”event” dimension och hello datatyper (kontinuerlig eller faktor). Hej träningsdata representeras av en sträng, där hello rader avgränsade med kommatecken och hello kolumner avgränsade med semikolon. hello antal funktioner hello data är flexibel. Alla hello-element i hello inmatade strängen måste vara numerisk. Hej träningsdata indikerar hello ”time” dimension hello antalet tidsenheter (dagar, år, hittills utfört, etc.) gått sedan hello startpunkten för hello studera (en tålamod tar emot läkemedel behandling program, en student första PhD undersökning, en bil startar toobe drivs, etc.) tills hello intressanta (hello tålamod returnerar toodrug användning, hello student erhålla hello PhD grad, hello bil med bromsar pad fel osv) inträffar. Hej ”event” dimension anger om hello händelse intressanta hello slutet av hello undersökning. Värdet ”händelse = 1” innebär att hello händelse intressanta inträffar vid hello tidpunkt som anges av hello ”time” dimension. ”händelse = 0” innebär att hello händelse av intresse har skett genom hello tidpunkt som anges av hello ”time” dimension.

* trainingdata - en teckensträng. Rader avgränsas med kommatecken och kolumner avgränsade med semikolon. Varje rad innehåller dimensionen ”time”, ”event” dimension och ge prognoser variabler.
* testingdata - en rad med data som innehåller ge prognoser variabler för ett visst objekt.
* time_of_interest - hello förfluten tid vid intresse n.
* index_time - hello kolumnindex hello ”” tidsdimension (med början från 1).
* index_event - hello kolumnindex för hello ”event” dimension (med början från 1).
* variable_types - en teckensträng med semikolon som avgränsare i den. 0 representerar kontinuerlig variabler och 1 representerar faktor variabler.

hello-utdata är hello sannolikheten för en händelse inträffar inom en viss tid. 

> Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder. 
> 
> 

Det finns flera olika sätt att konsumera hello service automatiskt (en exempelapp är [här](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

### <a name="starting-c-code-for-web-service-consumption"></a>Startar C#-kod för web service användning:
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




hello tolkningen av det här testet är som följer. Under förutsättning att hello målet hello data är toomodel hello förfluten tid tills hello returnera toodrug användning för hello patienter som tog emot ett hello två behandling program. Hej utdata hello web service läsningar: för patienter som 35 år, med tidigare antal behandling 2 gånger tar hello länge lokal behandling program, och med användning av både heroin och cocaine hello sannolikheten för att returnera toohello läkemedel användning är 95.64% av dag 500.

## <a name="creation-of-web-service"></a>Skapandet av web service
> Den här webbtjänsten har skapats med Azure Machine Learning. För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml). Nedan visas en skärmbild av hello experiment som skapade hello web service och exempel-kod för varje hello moduler i hello experiment.
> 
> 

I Azure Machine Learning skapas ett nytt tomt experiment skapades från två [köra R-skriptet] [ execute-r-script] moduler som hämtades till hello arbetsytan. hello dataschemat skapades med en enkel [köra R-skriptet][execute-r-script], som definierar hello indata schemat för hello-webbtjänsten. Denna modul är sedan länkade toohello andra [köra R-skriptet] [ execute-r-script] module, som högre arbete. Den här modulen innehåller dataförbearbetning modellskapandet och förutsägelser. I hello data förbearbetning steget hello indata som representeras av en lång sträng omvandlas och konverteras till en data-ram. Hello modellen byggnad steg installeras ett externt R-paket ”survival_2.37 7.zip” först för att utföra överlevnad analys. Hej ”coxph” funktionen utförs sedan efter en serie databearbetningsuppgifter. hello information om hello ”coxph”-funktionen för överlevnad analys kan läsas hello R-dokumentationen. En testnings-instans har angetts till hello tränad modell med hello ”surfit”-funktionen i hello förutsägelse steg och hello överlevnad kurva för det här testet instans skapas som ”kurvan” variabel. Slutligen hämtas hello sannolikhet hello tid av intresse. 

### <a name="experiment-flow"></a>Experiment flöde:
![Experiment flöde][1]

#### <a name="module-1"></a>Modul 1:
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

#### <a name="module-2"></a>Modulen 2:
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




## <a name="limitations"></a>Begränsningar
Den här webbtjänsten kan ta endast numeriska värden som funktionen variabler (kolumner). Hej ”” händelsekolumnen kan ta endast värdet 0 eller 1. Hej ”time” kolumn måste toobe ett positivt heltal.

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

