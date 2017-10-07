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
# <a name="deprecated-normal-distribution-suite"></a>(föråldrad) Normalfördelning Suite

> [!NOTE]
> hello Microsoft DataMarket dras och detta API är inaktuell. 
> 
> Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

hello normalfördelning Suite är en uppsättning exempel webbtjänster ([Generator](https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile Kalkylatorn](https://datamarket.azure.com/dataset/aml_labs/ndq5), [sannolikhet Kalkylatorn](https://datamarket.azure.com/dataset/aml_labs/ndp5)) som hjälp vid generering och hantering Normal distributioner. hello tjänster kan generera en normalfördelning sekvens som helst, beräkna quantiles från en viss sannolikhet och beräkna sannolikheten från en viss quantile. Var och en av hello-tjänster skickar olika utdata baserat på valda hello-tjänsten (se nedan). hello normalfördelning Suite baseras på hello R funktioner qnorm rnorm och pnorm, som ingår i statistik R-paket.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> Den här webbtjänsten kan till exempel användas av användare – potentiellt via en mobilapp via en webbplats eller på en lokal dator. Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden. Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst. hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world utan infrastruktur inställningar av hello författare av hello web service.  
> 
> 

## <a name="consumption-of-web-service"></a>Användning av web service
hello normalfördelning Suite innehåller hello följande 3 tjänster.

### <a name="normal-distribution-quantile-calculator"></a>Normalfördelning Quantile Kalkylatorn
Den här tjänsten accepterar 4 argument för en normal distribution och beräknar hello associerade quantile.

hello-indataargument är:

* p - en enda sannolikheten för en händelse med normalfördelning. 
* Medelvärde - hello normalfördelning medelvärdet.
* SD - standardavvikelsen för hello normalfördelning. 
* Sida - L för hello lägre sida av hello distribution och U för hello övre delen av hello-distribution.

hello utdata från hello-tjänsten är hello beräknade quantile som är associerad med hello angivna sannolikhet.

### <a name="normal-distribution-probability-calculator"></a>Normalfördelning sannolikhet Kalkylatorn
Den här tjänsten accepterar 4 argument för en normal distribution och beräknar hello associerade sannolikhet.

hello-indataargument är:

* q - en enda quantile av en händelse med normalfördelning. 
* Medelvärde - hello normalfördelning medelvärdet.
* SD - standardavvikelsen för hello normalfördelning. 
* Sida - L för hello lägre sida av hello distribution och U för hello övre delen av hello-distribution.

hello utdata från hello-tjänsten är hello beräknade sannolikhet som associeras med hello angivna quantile.

### <a name="normal-distribution-generator"></a>Normalfördelning Generator
Den här tjänsten accepterar 3 argument för en normal distribution och genererar en slumpmässig sekvens av siffror som normalt distribueras. hello måste följande argument tillhandahållas tooit inom hello begäran:

* n - hello antal observationer. 
* medelvärde - hello normalfördelning medelvärdet.
* SD - standardavvikelsen för hello normalfördelning. 

hello utdata från hello service är en sekvens med längden n med normalfördelning baserat på hello medelvärde och sd-argument.

> Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder. 
> 
> 

Det finns flera olika sätt att konsumera hello service automatiskt (exempel appar finns här: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [sannolikhet Kalkylatorn](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Quantile Kalkylatorn](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>Startar C#-kod för web service användning:
### <a name="normal-distribution-quantile-calculator"></a>Normalfördelning Quantile Kalkylatorn
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


### <a name="normal-distribution-probability-calculator"></a>Normalfördelning sannolikhet Kalkylatorn
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

### <a name="normal-distribution-generator"></a>Normalfördelning Generator
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


## <a name="creation-of-web-service"></a>Skapandet av web service
> Den här webbtjänsten har skapats med Azure Machine Learning. För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml). 
> 
> 

Nedan visas en skärmbild av hello experiment som skapade hello web service och exempel-kod för varje hello moduler i hello experiment.

### <a name="normal-distribution-quantile-calculator"></a>Normalfördelning Quantile Kalkylatorn
Experiment flöde:

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

### <a name="normal-distribution-probability-calculator"></a>Normalfördelning sannolikhet Kalkylatorn
Experiment flöde:

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

### <a name="normal-distribution-generator"></a>Normalfördelning Generator
Experiment flöde:

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

## <a name="limitations"></a>Begränsningar
Dessa är väldigt enkelt exempel omgivande hello normalfördelning. Lite fel fånga har implementerats som kan ses från hello exempelkod ovan.

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png

