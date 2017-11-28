---
title: "aaaA förutsägelselösning för kreditrisk med Machine Learning | Microsoft Docs"
description: "En detaljerad genomgång som visar hur toocreate en förutsägelseanalys för kredit riskbedömning i Azure Machine Learning Studio."
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
ms.openlocfilehash: 00ed39081e6952b8d03b37a8285d8dc3ddff2cb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a><span data-ttu-id="7c562-104">Genomgång: Utveckla en förutsägelseanalys för kreditriskbedömning i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="7c562-104">Walkthrough: Develop a predictive analytics solution for credit risk assessment in Azure Machine Learning</span></span>

<span data-ttu-id="7c562-105">I den här genomgången ska titta vi en utökad på hello arbetet med att utveckla en förutsägelseanalys i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="7c562-105">In this walkthrough, we take an extended look at hello process of developing a predictive analytics solution in Machine Learning Studio.</span></span> <span data-ttu-id="7c562-106">Vi utvecklar en enkel modell i Machine Learning Studio och sedan distribuera det som en Azure Machine Learning-webbtjänst där hello modell kan göra förutsägelser med hjälp av nya data.</span><span class="sxs-lookup"><span data-stu-id="7c562-106">We develop a simple model in Machine Learning Studio, and then deploy it as an Azure Machine Learning web service where hello model can make predictions using new data.</span></span> 

<span data-ttu-id="7c562-107">I den här genomgången förutsätter vi att du har använt Machine Learning Studio åtminstone någon gång och att du är någorlunda insatt i vad maskininlärning är.</span><span class="sxs-lookup"><span data-stu-id="7c562-107">This walkthrough assumes that you've used Machine Learning Studio at least once before, and that you have some understanding of machine learning concepts.</span></span> <span data-ttu-id="7c562-108">Men vi förväntar oss inte att du är en expert.</span><span class="sxs-lookup"><span data-stu-id="7c562-108">But it doesn't assume you're an expert in either.</span></span>

<span data-ttu-id="7c562-109">Om du aldrig har använt **Azure Machine Learning Studio** innan du kanske vill toostart med hello kursen [skapa din första datavetenskap experiment i Azure Machine Learning Studio](machine-learning-create-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="7c562-109">If you've never used **Azure Machine Learning Studio** before, you might want toostart with hello tutorial, [Create your first data science experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md).</span></span> <span data-ttu-id="7c562-110">Den här kursen tar dig igenom Machine Learning Studio för hello första gången.</span><span class="sxs-lookup"><span data-stu-id="7c562-110">That tutorial takes you through Machine Learning Studio for hello first time.</span></span> <span data-ttu-id="7c562-111">Den visar hello grunderna för hur toodrag och släpp-moduler på experimentet, ansluta dem till varandra, köra hello experiment och titta på hello resultat.</span><span class="sxs-lookup"><span data-stu-id="7c562-111">It shows you hello basics of how toodrag-and-drop modules onto your experiment, connect them together, run hello experiment, and look at hello results.</span></span> <span data-ttu-id="7c562-112">Ett annat verktyg som kan vara till hjälp för att komma igång är ett diagram som ger en översikt över hello funktionerna i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="7c562-112">Another tool that may be helpful for getting started is a diagram that gives an overview of hello capabilities of Machine Learning Studio.</span></span> <span data-ttu-id="7c562-113">Du kan hämta och skriva ut det här: [Översiktsdiagram över funktionerna i Azure Machine Learning Studio ](machine-learning-studio-overview-diagram.md).</span><span class="sxs-lookup"><span data-stu-id="7c562-113">You can download and print it from here: [Overview diagram of Azure Machine Learning Studio capabilities](machine-learning-studio-overview-diagram.md).</span></span>
 
<span data-ttu-id="7c562-114">Om du är ny toohello fältet för maskininlärning i allmänhet är det videoserie som kan vara användbara tooyou.</span><span class="sxs-lookup"><span data-stu-id="7c562-114">If you're new toohello field of machine learning in general, there's a video series that might be helpful tooyou.</span></span> <span data-ttu-id="7c562-115">Den kallas [datavetenskap för nybörjare](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) och den kan ge dig en bra överblick toomachine learning med vanliga ord och begrepp.</span><span class="sxs-lookup"><span data-stu-id="7c562-115">It's called [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) and it can give you a great introduction toomachine learning using everyday language and concepts.</span></span>


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 

## <a name="hello-problem"></a><span data-ttu-id="7c562-116">hello-problem</span><span class="sxs-lookup"><span data-stu-id="7c562-116">hello problem</span></span>

<span data-ttu-id="7c562-117">Anta att du behöver toopredict kreditrisken för en person baserat på hello information de gav på en kreditansökan.</span><span class="sxs-lookup"><span data-stu-id="7c562-117">Suppose you need toopredict an individual's credit risk based on hello information they gave on a credit application.</span></span>  

<span data-ttu-id="7c562-118">Kreditriskbedömning är ett komplext problem, men vi ska förenkla det lite i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="7c562-118">Credit risk assessment is a complex problem, but we can simplify it a bit for this walkthrough.</span></span> <span data-ttu-id="7c562-119">Vi använder det som ett exempel på hur du skapar en lösning för förutsägelseanalys i Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="7c562-119">We'll use it as an example of how you can create a predictive analytics solution using Microsoft Azure Machine Learning.</span></span> <span data-ttu-id="7c562-120">toodo kan vi använda Azure Machine Learning Studio och Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="7c562-120">toodo this, we use Azure Machine Learning Studio and a Machine Learning web service.</span></span>  

## <a name="hello-solution"></a><span data-ttu-id="7c562-121">hello-lösning</span><span class="sxs-lookup"><span data-stu-id="7c562-121">hello solution</span></span>

<span data-ttu-id="7c562-122">I den här detaljerade genomgången börjar vi med offentligt tillgängliga kreditriskdata och utvecklar och tränar en förutsägbar modell baserat på dessa data.</span><span class="sxs-lookup"><span data-stu-id="7c562-122">In this detailed walkthrough, we start with publicly available credit risk data and develop and train a predictive model based on that data.</span></span> <span data-ttu-id="7c562-123">Vi distribuera hello modellen som en webbtjänst så att den kan användas av andra för kreditriskbedömning.</span><span class="sxs-lookup"><span data-stu-id="7c562-123">Then we deploy hello model as a web service so it can be used by others for credit risk assessment.</span></span>

<span data-ttu-id="7c562-124">toocreate denna lösning, vi gör du följande:</span><span class="sxs-lookup"><span data-stu-id="7c562-124">toocreate this credit risk assessment solution, we follow these steps:</span></span>  

1. [<span data-ttu-id="7c562-125">Skapa en Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="7c562-125">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="7c562-126">Överför befintliga data</span><span class="sxs-lookup"><span data-stu-id="7c562-126">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="7c562-127">Skapa ett experiment</span><span class="sxs-lookup"><span data-stu-id="7c562-127">Create an experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="7c562-128">Träna och utvärdera hello modeller</span><span class="sxs-lookup"><span data-stu-id="7c562-128">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="7c562-129">Distribuera hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="7c562-129">Deploy hello web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="7c562-130">Åtkomst hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="7c562-130">Access hello web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

> [!TIP] 
> <span data-ttu-id="7c562-131">Du hittar en fungerande kopia av hello experiment som vi utvecklar i den här genomgången i hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="7c562-131">You can find a working copy of hello experiment that we develop in this walkthrough in hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="7c562-132">Gå för**[genomgången - kredit risk förutsägelse](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)**  och på **öppna i Studio** toodownload en kopia av hello experiment i Machine Learning Studio-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="7c562-132">Go too**[Walkthrough - Credit risk prediction](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)** and click **Open in Studio** toodownload a copy of hello experiment into your Machine Learning Studio workspace.</span></span>
> 
> <span data-ttu-id="7c562-133">Den här genomgången är baserad på en förenklad version av hello exempelexperimentet [binär klassificering: kredit risk förutsägelse](http://go.microsoft.com/fwlink/?LinkID=525270), även tillgänglig i hello [galleriet](http://gallery.cortanaintelligence.com/).</span><span class="sxs-lookup"><span data-stu-id="7c562-133">This walkthrough is based on a simplified version of hello sample experiment, [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270), also available in hello [Gallery](http://gallery.cortanaintelligence.com/).</span></span>
