---
title: "aaa(deprecated) Machine learning-webbtjänster exempel som skapats med R - Azure | Microsoft Docs"
description: "(föråldrad) Hitta en bra uppsättning web services exempel skapats med R-koden och Machine Learning och publicerats toohello Azure Marketplace."
keywords: csharp r-koden, web services-exempel
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 97d66cb7-6a84-4ae9-8095-0b5f5ba82d7f
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
ms.openlocfilehash: 20b074d38e65aed907d40549bb61f124cb5dfe1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-web-services-examples-using-r-code-on-azure-machine-learning-and-published-toomicrosoft-azure-marketplace"></a><span data-ttu-id="469b2-104">(föråldrad) Exempel med hjälp av R-koden på Azure Machine Learning och publicerade tooMicrosoft Azure Marketplace-webbtjänster</span><span class="sxs-lookup"><span data-stu-id="469b2-104">(deprecated) Web services examples using R code on Azure Machine Learning and published tooMicrosoft Azure Marketplace</span></span>

> [!NOTE]
> <span data-ttu-id="469b2-105">hello Microsoft DataMarket dras och dessa API: er är inaktuella.</span><span class="sxs-lookup"><span data-stu-id="469b2-105">hello Microsoft DataMarket is being retired and these APIs have been deprecated.</span></span> 
> 
> <span data-ttu-id="469b2-106">Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="469b2-106">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="469b2-107">Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="469b2-107">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="469b2-108">I den här artikeln är exempel web services har skapats med Azure Machine Learning och publiceras toohello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="469b2-108">In this article are example web services were created using Azure Machine Learning and published toohello Azure Marketplace.</span></span> <span data-ttu-id="469b2-109">Varje web service innehåller ett övergripande dokument anslutits bädda in provdatauppsättningar för att testa hello tjänster och förklarar hur hello användare kan skapa en liknande tjänst sig själva.</span><span class="sxs-lookup"><span data-stu-id="469b2-109">Each web service example has a comprehensive document attached, embedding sample data sets for testing hello services and explaining how hello user can create a similar service themselves.</span></span> 

<span data-ttu-id="469b2-110">I Azure Machine Learning Studio användarna skriva R-koden och med några få klick kan du publicera den som en webbtjänst för program och enheter tooconsume runt hello world.</span><span class="sxs-lookup"><span data-stu-id="469b2-110">In Azure Machine Learning Studio, users can write R code and with a few clicks, publish it as a web service for applications and devices tooconsume around hello world.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="469b2-111">Leder enkla räknare som tillhandahåller en anpassad text utvinningsmodellen sentiment analys ge prognoser för statistiska funktioner toocreating både nya och erfarna R-användare kan dra nytta av hello enkel som kan användare av Azure Machine Learning operationalisera R kod.</span><span class="sxs-lookup"><span data-stu-id="469b2-111">From producing simple calculators that provide statistical functionality toocreating a custom text-mining sentiment analysis predictor, both new and experienced R users can benefit from hello ease with which users of Azure Machine Learning can operationalize R code.</span></span> <span data-ttu-id="469b2-112">När dessa webbtjänster kan användas av användare potentiellt via en mobil app eller en webbplats hello syftet med dessa webbtjänster som exempel är tooshow hur Maskininlärning kan operationalisera R skript för analys och innehålla används toocreate webbtjänster överst i R-koden.</span><span class="sxs-lookup"><span data-stu-id="469b2-112">While these web services could be consumed by users, potentially through a mobile app or a website, hello purpose of these web services examples is tooshow how Machine Learning can operationalize R scripts for analytical purposes and be used toocreate web services on top of R code.</span></span>

<span data-ttu-id="469b2-113">Varje exempel innehåller ett C#-exempel för användning av web service.</span><span class="sxs-lookup"><span data-stu-id="469b2-113">Each example includes a C# example for web service consumption.</span></span>

![Diagram över R-koden i Azure Machine Learning: R lösningar för egna användning eller publicerade toohello Azure Marketplace.][1]

<span data-ttu-id="469b2-115">Överväg att hello följande scenarier.</span><span class="sxs-lookup"><span data-stu-id="469b2-115">Consider hello following scenarios.</span></span>

## <a name="scenario-1-generic-model"></a><span data-ttu-id="469b2-116">Scenario 1: Allmän modell</span><span class="sxs-lookup"><span data-stu-id="469b2-116">Scenario 1: Generic model</span></span>
<span data-ttu-id="469b2-117">En användare arbetar med en allmän modell som kan vara tillämpade tooa den nya användarens data, till exempel en grundläggande prognoser tid seriedata eller ett specialbyggt R-metod med avancerade analyser.</span><span class="sxs-lookup"><span data-stu-id="469b2-117">A user works with a generic model that can be applied tooa new user’s data, such as a basic forecasting of time series data or a custom-built R method with advanced analytics.</span></span> <span data-ttu-id="469b2-118">Den här användaren publicerar hello modellen som en webbtjänst för andra tooconsume med sina data.</span><span class="sxs-lookup"><span data-stu-id="469b2-118">This user publishes hello model as a web service for others tooconsume with their data.</span></span>

* [<span data-ttu-id="469b2-119">Binär klassificerare</span><span class="sxs-lookup"><span data-stu-id="469b2-119">Binary Classifier</span></span>](machine-learning-r-csharp-binary-classifier.md)
* [<span data-ttu-id="469b2-120">Klustermodell</span><span class="sxs-lookup"><span data-stu-id="469b2-120">Cluster Model</span></span>](machine-learning-r-csharp-cluster-model.md)
* [<span data-ttu-id="469b2-121">Multivarierad linjär regression</span><span class="sxs-lookup"><span data-stu-id="469b2-121">Multivariate Linear Regression</span></span>](machine-learning-r-csharp-multivariate-linear-regression.md)
* [<span data-ttu-id="469b2-122">Prognostisering – Exponentiell utjämning</span><span class="sxs-lookup"><span data-stu-id="469b2-122">Forecasting - Exponential Smoothing</span></span>](machine-learning-r-csharp-forecasting-exponential-smoothing.md)
* [<span data-ttu-id="469b2-123">ETS + STL prognoser</span><span class="sxs-lookup"><span data-stu-id="469b2-123">ETS + STL Forecasting</span></span>](machine-learning-r-csharp-retail-demand-forecasting.md)
* [<span data-ttu-id="469b2-124">Prognosticering - Autoregressive integrerad glidande medelvärde (ARIMA)</span><span class="sxs-lookup"><span data-stu-id="469b2-124">Forecasting - Autoregressive Integrated Moving Average (ARIMA)</span></span>](machine-learning-r-csharp-arima.md)
* [<span data-ttu-id="469b2-125">Analys av fortsatt giltighet</span><span class="sxs-lookup"><span data-stu-id="469b2-125">Survival Analysis</span></span>](machine-learning-r-csharp-survival-analysis.md)

## <a name="scenario-2-trained-model--specific-data"></a><span data-ttu-id="469b2-126">Scenario 2: Tränad modell – specifika data</span><span class="sxs-lookup"><span data-stu-id="469b2-126">Scenario 2: Trained model – specific data</span></span>
<span data-ttu-id="469b2-127">En användare har data som innehåller användbar förutsägelser via R-koden som ett stort urval av person enkäter klustrade via en k-means algoritmen toopredict hello användares personlighetstyp eller hälsa undersökning data som kan använda toopredict en person risk för lungcancer med ett överlevnad analys R-paket.</span><span class="sxs-lookup"><span data-stu-id="469b2-127">A user has data that provides useful predictions through R code, such as a large sample of personality questionnaires clustered through a k-means algorithm toopredict hello user’s personality type, or health survey data that can be used toopredict an individual’s risk for lung cancer with a survival analysis R package.</span></span> <span data-ttu-id="469b2-128">hello användaren publicerar hello data via en webbtjänst som beräknar resultatet av en ny användare.</span><span class="sxs-lookup"><span data-stu-id="469b2-128">hello user publishes hello data through a web service that predicts a new user’s outcome.</span></span>

## <a name="scenario-3-trained-model--generic-data"></a><span data-ttu-id="469b2-129">Scenario 3: Tränad modell – allmänna data</span><span class="sxs-lookup"><span data-stu-id="469b2-129">Scenario 3: Trained model – generic data</span></span>
<span data-ttu-id="469b2-130">En användare har allmänna data (till exempel en text Kristi) som gör att en web service toobe inbyggda och allmänna används över flera olika typer av användningsområden och scenarier.</span><span class="sxs-lookup"><span data-stu-id="469b2-130">A user has generic data (such as a text corpus) that allows a web service toobe built and applied generically across different types of use cases and scenarios.</span></span>

* [<span data-ttu-id="469b2-131">Lexikonbaserad attitydanalys</span><span class="sxs-lookup"><span data-stu-id="469b2-131">Lexicon Based Sentiment Analysis</span></span>](machine-learning-r-csharp-lexicon-based-sentiment-analysis.md)

## <a name="scenario-4-advanced-calculator"></a><span data-ttu-id="469b2-132">Scenario 4: Avancerade Kalkylatorn</span><span class="sxs-lookup"><span data-stu-id="469b2-132">Scenario 4: Advanced calculator</span></span>
<span data-ttu-id="469b2-133">En användare ger avancerade beräkningar eller simulering som inte kräver någon tränad modell eller montering av en modell toohello användarens data.</span><span class="sxs-lookup"><span data-stu-id="469b2-133">A user provides advanced calculations or simulations that don’t require any trained model or fitting of a model toohello user’s data.</span></span>

* [<span data-ttu-id="469b2-134">Skillnad i proportionstest</span><span class="sxs-lookup"><span data-stu-id="469b2-134">Difference in Proportions Test</span></span>](machine-learning-r-csharp-difference-in-two-proportions.md)
* [<span data-ttu-id="469b2-135">Normalt fördelningspaket</span><span class="sxs-lookup"><span data-stu-id="469b2-135">Normal Distribution Suite</span></span>](machine-learning-r-csharp-normal-distribution.md)
* [<span data-ttu-id="469b2-136">Binomialt fördelningspaket</span><span class="sxs-lookup"><span data-stu-id="469b2-136">Binomial Distribution Suite</span></span>](machine-learning-r-csharp-binomial-distribution.md)

## <a name="faq"></a><span data-ttu-id="469b2-137">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="469b2-137">FAQ</span></span>
<span data-ttu-id="469b2-138">Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Marketplace finns [här](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="469b2-138">For frequently asked questions on consumption of hello web service or publishing toohello Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-web-service-examples/machine-learning-r-code-options-for-using-and-sharing-cloud.png



