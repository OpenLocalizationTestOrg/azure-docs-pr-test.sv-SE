---
title: "aaa(deprecated) binomialfördelningen Suite - Azure | Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 6f94436cd19abeb518d179f340c8d4f43fcf4520
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-binomial-distribution-suite"></a>(föråldrad) Binomialfördelningen Suite

> [!NOTE]
> hello Microsoft DataMarket dras och detta API är inaktuell. 
> 
> Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

hello binomialfördelningen Suite är en uppsättning exempel webbtjänster ([Binomial Generator](https://datamarket.azure.com/dataset/aml_labs/bdg5), [sannolikhet Kalkylatorn](https://datamarket.azure.com/dataset/aml_labs/bdp4), [Quantile Kalkylatorn](https://datamarket.azure.com/dataset/aml_labs/bdq5)) som hjälp i genererar och Hantera binomial distributioner. hello tjänster kan generera en binomialfördelningen sekvens med en längd som beräknar quantiles av angiven sannolikhet och beräkning av sannolikheten från en viss quantile. Var och en av hello-tjänster skickar olika utdata baserat på valda hello-tjänsten (se nedan). hello binomialfördelningen Suite baseras på hello R funktioner qbinom rbinom och pbinom, som ingår i statistik hello R-paket. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> Dessa webbtjänster kan användas av användare – potentiellt direkt på hello marketplace via en mobil app via en webbplats eller på en lokal dator, till exempel. Men hello syftet hello-webbtjänsten är också tooserve som ett exempel på hur Azure Machine Learning kan vara används toocreate webbtjänster ovanpå R-koden. Med bara några få rader med kod för R och klickar på en knapp i Azure Machine Learning Studio, ett experiment skapas med R-koden och publiceras som en webbtjänst. hello-webbtjänsten kan sedan publicerade toohello Azure Marketplace och används av användare och enheter över hello world – inga infrastrukturinställningar hello författare av hello web service krävs.
> 
> 

## <a name="consumption-of-web-service"></a>Användning av web service
hello binomialfördelningen Suite innehåller hello följande 3 tjänster.

### <a name="binomial-distribution-quantile-calculator"></a>Binomialfördelningen Quantile Kalkylatorn
Den här tjänsten accepterar 4 argument för en normal distribution och beräknar hello associerade quantile.
hello-indataargument är:

* p - en enda samman sannolikheten för flera försök.  
* storlek - hello antalet försök.
* sannolikhet - hello sannolikheten att lyckas i en utvärderingsversion.
* Sida - L för hello lägre sida av hello distribution, U för hello övre delen av hello-distribution. 

hello utdata från hello-tjänsten är hello beräknade quantile som är associerad med hello angivna sannolikhet.

### <a name="binomial-distribution-probability-calculator"></a>Binomialfördelningen sannolikhet Kalkylatorn
Den här tjänsten accepterar 4 argument för en binomialfördelningen och beräknar hello associerade sannolikhet.
hello-indataargument är:

* q - en enda quantile av en händelse med binomialfördelningen. 
* storlek - hello antalet försök.
* sannolikhet - hello sannolikheten att lyckas i en utvärderingsversion.
* sida - L för hello lägre sida av hello distribution, U för hello övre delen av hello distribution eller E som är lika tooa enda antalet lyckade försök.

hello utdata från hello-tjänsten är hello beräknade sannolikhet som associeras med hello angivna quantile.

### <a name="binomial-distribution-generator"></a>Binomialfördelningen Generator
Den här tjänsten accepterar 3 argument för en binomialfördelningen och genererar en slumpmässig sekvens av siffror som binomially distribueras. hello måste följande argument tillhandahållas tooit inom hello begäran:

* n - antal observationer. 
* storlek - antalet försök.
* SANNOLIKHET - sannolikheten att lyckas.

hello utdata från hello service är en sekvens med längden n med en binomialfördelningen baserat på hello storlek och sannolikhet argument.

> Den här tjänsten, är som finns på hello Azure Marketplace en OData-tjänst. Dessa kan anropas via POST eller GET-metoder. 
> 
> 

Det finns flera olika sätt att konsumera hello service automatiskt (exempel appar finns här: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [sannolikhet Kalkylatorn](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Quantile Kalkylatorn](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)). 

### <a name="starting-c-code-for-web-service-consumption"></a>Startar C#-kod för web service användning:
### <a name="binomial-distribution-quantile-calculator"></a>Binomialfördelningen Quantile Kalkylatorn
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

### <a name="binomial-distribution-probability-calculator"></a>Binomialfördelningen sannolikhet Kalkylatorn
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


### <a name="binomial-distribution-generator"></a>Binomialfördelningen Generator
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





## <a name="creation-of-web-service"></a>Skapandet av web service
> Den här webbtjänsten har skapats med Azure Machine Learning. För en kostnadsfri utvärderingsversion som inledande videoklipp om hur du skapar experiment och [publicering webbtjänster](machine-learning-publish-a-machine-learning-web-service.md), se [azure.com/ml](http://azure.com/ml). Nedan visas en skärmbild av hello experiment som skapade hello web service och exempel-kod för varje hello moduler i hello experiment.
> 
> 

### <a name="binomial-distribution-quantile-calculator"></a>Binomialfördelningen Quantile Kalkylatorn
![Skapa arbetsyta][4]

#### <a name="module-1"></a>Modul 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data toooutput port
#### <a name="module-2"></a>Modulen 2:
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

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");


### <a name="binomial-distribution-probability-calculator"></a>Binomialfördelningen sannolikhet Kalkylatorn
![Skapa arbetsyta][5]

#### <a name="module-1"></a>Modul 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data toooutput port


#### <a name="module-2"></a>Modulen 2:
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

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

### <a name="binomial-distribution-generator"></a>Binomialfördelningen Generator
![Skapa arbetsyta][6]

#### <a name="module-1"></a>Modul 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data toooutput port

#### <a name="module-2"></a>Modulen 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a>Begränsningar
Dessa är väldigt enkelt exempel omgivande hello binomialfördelningen. Lite fel fånga har implementerats som kan ses från hello exempelkod ovan.

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Azure Marketplace finns [här](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png

