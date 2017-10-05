---
title: "Förutsägelseanalys för kreditrisk med Machine Learning | Microsoft Docs"
description: "En detaljerad genomgång av hur du skapar en förutsägelseanalys för kreditriskbedömning i Azure Machine Learning Studio."
keywords: "kreditrisk, lösning för förutsägelseanalys, riskbedömning"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: e1af999b2fde8ffa2a0ffd1b88230f0aaab32b37
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a><span data-ttu-id="d90ce-104">Genomgång: Utveckla en förutsägelseanalys för kreditriskbedömning i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="d90ce-104">Walkthrough: Develop a predictive analytics solution for credit risk assessment in Azure Machine Learning</span></span>

<span data-ttu-id="d90ce-105">I den här genomgången ska vi titta närmare på hur du utvecklar en lösning för förutsägelseanalys i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="d90ce-105">In this walkthrough, we take an extended look at the process of developing a predictive analytics solution in Machine Learning Studio.</span></span> <span data-ttu-id="d90ce-106">Vi ska skapa en enkel modell i Machine Learning Studio som vi sedan distribuerar som en Azure Machine Learning-webbtjänst, där modellen kan göra förutsägelser med nya data.</span><span class="sxs-lookup"><span data-stu-id="d90ce-106">We develop a simple model in Machine Learning Studio, and then deploy it as an Azure Machine Learning web service where the model can make predictions using new data.</span></span> 

<span data-ttu-id="d90ce-107">I den här genomgången förutsätter vi att du har använt Machine Learning Studio åtminstone någon gång och att du är någorlunda insatt i vad maskininlärning är.</span><span class="sxs-lookup"><span data-stu-id="d90ce-107">This walkthrough assumes that you've used Machine Learning Studio at least once before, and that you have some understanding of machine learning concepts.</span></span> <span data-ttu-id="d90ce-108">Men vi förväntar oss inte att du är en expert.</span><span class="sxs-lookup"><span data-stu-id="d90ce-108">But it doesn't assume you're an expert in either.</span></span>

<span data-ttu-id="d90ce-109">Om du aldrig har använt **Azure Machine Learning Studio** innan kanske du vill börja med självstudierna, [Skapa ditt första dataexperiment i Azure Machine Learning Studio](machine-learning-create-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="d90ce-109">If you've never used **Azure Machine Learning Studio** before, you might want to start with the tutorial, [Create your first data science experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md).</span></span> <span data-ttu-id="d90ce-110">Den självstudiekursen vänder sig till nybörjare och vägleder dig genom Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="d90ce-110">That tutorial takes you through Machine Learning Studio for the first time.</span></span> <span data-ttu-id="d90ce-111">Vi går vi igenom grunderna och förklarar hur du drar och släpper moduler i ett experiment, hur du kopplar ihop dem och hur du kör experimentet. Vi avslutar med att titta på resultatet.</span><span class="sxs-lookup"><span data-stu-id="d90ce-111">It shows you the basics of how to drag-and-drop modules onto your experiment, connect them together, run the experiment, and look at the results.</span></span> <span data-ttu-id="d90ce-112">Ett annat verktyg som hjälper dig att komma igång är ett diagram med en översikt över funktionerna i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="d90ce-112">Another tool that may be helpful for getting started is a diagram that gives an overview of the capabilities of Machine Learning Studio.</span></span> <span data-ttu-id="d90ce-113">Du kan hämta och skriva ut det här: [Översiktsdiagram över funktionerna i Azure Machine Learning Studio ](machine-learning-studio-overview-diagram.md).</span><span class="sxs-lookup"><span data-stu-id="d90ce-113">You can download and print it from here: [Overview diagram of Azure Machine Learning Studio capabilities](machine-learning-studio-overview-diagram.md).</span></span>
 
<span data-ttu-id="d90ce-114">Om du inte har erfarenhet av maskininlärning finns det en bra videoserie som vi rekommenderar.</span><span class="sxs-lookup"><span data-stu-id="d90ce-114">If you're new to the field of machine learning in general, there's a video series that might be helpful to you.</span></span> <span data-ttu-id="d90ce-115">Den heter [Datavetenskap för nybörjare](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) och är en bra introduktion som med enkelt språk förklarar vad maskininlärning är.</span><span class="sxs-lookup"><span data-stu-id="d90ce-115">It's called [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) and it can give you a great introduction to machine learning using everyday language and concepts.</span></span>


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 

## <a name="the-problem"></a><span data-ttu-id="d90ce-116">Problemet</span><span class="sxs-lookup"><span data-stu-id="d90ce-116">The problem</span></span>

<span data-ttu-id="d90ce-117">Anta att du behöver förutsäga kreditrisken för en person baserat på den information som han eller hon fyller i på en kreditansökan.</span><span class="sxs-lookup"><span data-stu-id="d90ce-117">Suppose you need to predict an individual's credit risk based on the information they gave on a credit application.</span></span>  

<span data-ttu-id="d90ce-118">Kreditriskbedömning är ett komplext problem, men vi ska förenkla det lite i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="d90ce-118">Credit risk assessment is a complex problem, but we can simplify it a bit for this walkthrough.</span></span> <span data-ttu-id="d90ce-119">Vi använder det som ett exempel på hur du skapar en lösning för förutsägelseanalys i Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d90ce-119">We'll use it as an example of how you can create a predictive analytics solution using Microsoft Azure Machine Learning.</span></span> <span data-ttu-id="d90ce-120">För att göra det använder vi Azure Machine Learning Studio och en Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="d90ce-120">To do this, we use Azure Machine Learning Studio and a Machine Learning web service.</span></span>  

## <a name="the-solution"></a><span data-ttu-id="d90ce-121">Lösningen</span><span class="sxs-lookup"><span data-stu-id="d90ce-121">The solution</span></span>

<span data-ttu-id="d90ce-122">I den här detaljerade genomgången börjar vi med offentligt tillgängliga kreditriskdata och utvecklar och tränar en förutsägbar modell baserat på dessa data.</span><span class="sxs-lookup"><span data-stu-id="d90ce-122">In this detailed walkthrough, we start with publicly available credit risk data and develop and train a predictive model based on that data.</span></span> <span data-ttu-id="d90ce-123">Därefter distribuerar vi modellen som en webbtjänst så att den kan användas av andra för kreditriskbedömning.</span><span class="sxs-lookup"><span data-stu-id="d90ce-123">Then we deploy the model as a web service so it can be used by others for credit risk assessment.</span></span>

<span data-ttu-id="d90ce-124">Vi skapar lösningen för kreditriskbedömning genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="d90ce-124">To create this credit risk assessment solution, we follow these steps:</span></span>  

1. [<span data-ttu-id="d90ce-125">Skapa en Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="d90ce-125">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="d90ce-126">Överför befintliga data</span><span class="sxs-lookup"><span data-stu-id="d90ce-126">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="d90ce-127">Skapa ett experiment</span><span class="sxs-lookup"><span data-stu-id="d90ce-127">Create an experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="d90ce-128">Träna och utvärdera modellerna</span><span class="sxs-lookup"><span data-stu-id="d90ce-128">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="d90ce-129">Distribuera webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="d90ce-129">Deploy the web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="d90ce-130">Få åtkomst till webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="d90ce-130">Access the web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

> [!TIP] 
> <span data-ttu-id="d90ce-131">Du hittar en fungerande kopia av experimentet som vi skapar i den här genomgången i [Cortana Intelligence-galleriet](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="d90ce-131">You can find a working copy of the experiment that we develop in this walkthrough in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="d90ce-132">Gå till **[Walkthrough - Credit risk prediction](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)** (Genomgång –kreditriskbedömning) och klicka på **Öppna i Studio** för att ladda ned en kopia av experimentet till din Machine Learning Studio-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="d90ce-132">Go to **[Walkthrough - Credit risk prediction](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)** and click **Open in Studio** to download a copy of the experiment into your Machine Learning Studio workspace.</span></span>
> 
> <span data-ttu-id="d90ce-133">Den här genomgången baseras på en förenklad version av exempelexperimentet [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270) (Binär klassificering: kreditriskbedömning), som också finns i [galleriet](http://gallery.cortanaintelligence.com/).</span><span class="sxs-lookup"><span data-stu-id="d90ce-133">This walkthrough is based on a simplified version of the sample experiment, [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270), also available in the [Gallery](http://gallery.cortanaintelligence.com/).</span></span>
