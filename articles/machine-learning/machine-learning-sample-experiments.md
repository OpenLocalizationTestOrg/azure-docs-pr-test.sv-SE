---
title: "aaaCopy maskininlärning exempel experiment - Azure | Microsoft Docs"
description: "Lär dig hur toouse exempel machine learning-experimenten toocreate nya experiment med Cortana Intelligence Gallery och Microsoft Azure Machine Learning."
keywords: machine learning examples, sample experiment, machine learning sample
services: machine-learning
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: 81e6c1d8-682c-4db3-bfd5-d7bfb1150ff3
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2017
ms.author: cgronlun
ms.openlocfilehash: 555f05cf9af2040d1703707f7583396d5da9f156
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-example-experiments-toocreate-new-machine-learning-experiments"></a><span data-ttu-id="a42fb-104">Kopiera exempel experiment toocreate nya maskininlärning experiment</span><span class="sxs-lookup"><span data-stu-id="a42fb-104">Copy example experiments toocreate new machine learning experiments</span></span>
<span data-ttu-id="a42fb-105">Lär dig hur toostart med exemplet experimenten från [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) istället för att skapa machine learning-experiment från grunden.</span><span class="sxs-lookup"><span data-stu-id="a42fb-105">Learn how toostart with example experiments from [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) instead of creating machine learning experiments from scratch.</span></span> <span data-ttu-id="a42fb-106">Du kan använda hello exempel toobuild din egen lösning för maskininlärning.</span><span class="sxs-lookup"><span data-stu-id="a42fb-106">You can use hello examples toobuild your own machine learning solution.</span></span>

<span data-ttu-id="a42fb-107">hello gallery har exempel experiment av hello Microsoft Azure Machine Learning-teamet samt exempel som delas av hello Machine Learning-communityn.</span><span class="sxs-lookup"><span data-stu-id="a42fb-107">hello gallery has example experiments by hello Microsoft Azure Machine Learning team as well as examples shared by hello Machine Learning community.</span></span> <span data-ttu-id="a42fb-108">Du kan även ställa frågor eller lämna kommentarer om experiment.</span><span class="sxs-lookup"><span data-stu-id="a42fb-108">You also can ask questions or post comments about experiments.</span></span>

<span data-ttu-id="a42fb-109">toosee hur toouse hello galleriet Se hello 3-minuters video [kopiera andras arbete toodo datavetenskap](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) från hello serien [datavetenskap för nybörjare](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span><span class="sxs-lookup"><span data-stu-id="a42fb-109">toosee how toouse hello gallery, watch hello 3-minute video [Copy other people's work toodo data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) from hello series [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-toocopy-in-cortana-intelligence-gallery"></a><span data-ttu-id="a42fb-110">Hitta ett experiment toocopy i Cortana Intelligence Gallery</span><span class="sxs-lookup"><span data-stu-id="a42fb-110">Find an experiment toocopy in Cortana Intelligence Gallery</span></span>
<span data-ttu-id="a42fb-111">toosee vilka experiment som är tillgängliga, gå toohello [galleriet](https://gallery.cortanaintelligence.com/) och på **experiment** hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="a42fb-111">toosee what experiments are available, go toohello [Gallery](https://gallery.cortanaintelligence.com/) and click **Experiments** at hello top of hello page.</span></span>

### <a name="find-hello-newest-or-most-popular-experiments"></a><span data-ttu-id="a42fb-112">Hitta hello senaste och mest populära experiment</span><span class="sxs-lookup"><span data-stu-id="a42fb-112">Find hello newest or most popular experiments</span></span>
<span data-ttu-id="a42fb-113">På den här sidan kan du se **nyligen lagt till** experiment eller bläddra nedåt toolook på **populära** eller hello senaste **populära Microsoft-experimenten**.</span><span class="sxs-lookup"><span data-stu-id="a42fb-113">On this page, you can see **Recently added** experiments, or scroll down toolook at **What's popular** or hello latest **Popular Microsoft experiments**.</span></span>

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a><span data-ttu-id="a42fb-114">Leta efter ett experiment som uppfyller specifika krav</span><span class="sxs-lookup"><span data-stu-id="a42fb-114">Look for an experiment that meets specific requirements</span></span>
<span data-ttu-id="a42fb-115">toobrowse alla försök:</span><span class="sxs-lookup"><span data-stu-id="a42fb-115">toobrowse all experiments:</span></span>

1. <span data-ttu-id="a42fb-116">Klicka på **Bläddra igenom alla** hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="a42fb-116">Click **Browse all** at hello top of hello page.</span></span>
2. <span data-ttu-id="a42fb-117">Hello vänster, under **förfina genom** i hello **kategorier** väljer **Experiment** toosee alla hello experiment i hello Gallery.</span><span class="sxs-lookup"><span data-stu-id="a42fb-117">On hello left-hand side, under **Refine by** in hello **Categories** section, select **Experiment** toosee all hello experiments in hello Gallery.</span></span>
3. <span data-ttu-id="a42fb-118">Du kan hitta experiment som uppfyller kraven på ett par olika sätt:</span><span class="sxs-lookup"><span data-stu-id="a42fb-118">You can find experiments that meet your requirements a couple different ways:</span></span>
   * <span data-ttu-id="a42fb-119">**Välj filter hello vänster.**</span><span class="sxs-lookup"><span data-stu-id="a42fb-119">**Select filters on hello left.**</span></span> <span data-ttu-id="a42fb-120">Till exempel toobrowse experiment som använder en PCA-baserad algoritm för avvikelseidentifiering: med **Experiment** valda under **kategorier**, klickar du på **visa alla**.</span><span class="sxs-lookup"><span data-stu-id="a42fb-120">For example, toobrowse experiments that use a PCA-based anomaly detection algorithm: With **Experiment** selected under **Categories**, click **Show all**.</span></span> <span data-ttu-id="a42fb-121">Sedan väljer du **PCA-Based Anomaly Detection** (PCA-baserad avvikelseidentifiering) under **Algorithms Used** (Algoritmer som används).</span><span class="sxs-lookup"><span data-stu-id="a42fb-121">Then, under **Algorithms Used**, choose **PCA-Based Anomaly Detection**.</span></span> <br></br><span data-ttu-id="a42fb-122">
     ![Välj filter](./media/machine-learning-sample-experiments/refine-the-view.png)</span><span class="sxs-lookup"><span data-stu-id="a42fb-122">
![Select filters](./media/machine-learning-sample-experiments/refine-the-view.png)</span></span>
   * <span data-ttu-id="a42fb-123">**Använd hello sökrutan.**</span><span class="sxs-lookup"><span data-stu-id="a42fb-123">**Use hello search box.**</span></span> <span data-ttu-id="a42fb-124">Till exempel toofind experiment har bidragit med Microsoft relaterade toodigit recognition som använder två-class support vector machine algoritmen anger ”digit recognition” i sökrutan för hello.</span><span class="sxs-lookup"><span data-stu-id="a42fb-124">For example, toofind experiments contributed by Microsoft related toodigit recognition that use a two-class support vector machine algorithm, enter "digit recognition" in hello search box.</span></span> <span data-ttu-id="a42fb-125">Sedan väljer hello filter **Experiment**, **Microsoft-innehåll**, och **Two-Class Support Vector Machine**:</span><span class="sxs-lookup"><span data-stu-id="a42fb-125">Then, select hello filters **Experiment**, **Microsoft content only**, and **Two-Class Support Vector Machine**:</span></span><br></br><span data-ttu-id="a42fb-126">
     ![Använda sökrutan hello](./media/machine-learning-sample-experiments/search-for-experiments.png)</span><span class="sxs-lookup"><span data-stu-id="a42fb-126">
![Use hello search box](./media/machine-learning-sample-experiments/search-for-experiments.png)</span></span>
4. <span data-ttu-id="a42fb-127">Klicka på ett experiment toolearn mer om den.</span><span class="sxs-lookup"><span data-stu-id="a42fb-127">Click an experiment toolearn more about it.</span></span>
5. <span data-ttu-id="a42fb-128">toorun och/eller ändra hello experiment, klickar på **öppna i Studio** på hello experimentsidan.</span><span class="sxs-lookup"><span data-stu-id="a42fb-128">toorun and/or modify hello experiment, click **Open in Studio** on hello experiment's page.</span></span> <br></br>

    ![Exempelexperiment](./media/machine-learning-sample-experiments/example-experiment.png)

    > [!NOTE]
    > <span data-ttu-id="a42fb-130">När du öppnar ett experiment i Machine Learning Studio för hello första gången, kan du testa det gratis eller köpa en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a42fb-130">When you open an experiment in Machine Learning Studio for hello first time, you can try it for free or buy an Azure subscription.</span></span> [<span data-ttu-id="a42fb-131">Lär dig mer om hello Machine Learning Studio kostnadsfri utvärderingsversion eller betald service</span><span class="sxs-lookup"><span data-stu-id="a42fb-131">Learn about hello Machine Learning Studio free trial vs. paid service</span></span>](https://azure.microsoft.com/pricing/details/machine-learning/)
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a><span data-ttu-id="a42fb-132">Skapa ett nytt experiment med ett exempel som mall</span><span class="sxs-lookup"><span data-stu-id="a42fb-132">Create a new experiment using an example as a template</span></span>
<span data-ttu-id="a42fb-133">Du kan också skapa ett nytt experiment i Machine Learning Studio genom att använda ett exempel från galleriet som mall.</span><span class="sxs-lookup"><span data-stu-id="a42fb-133">You also can create a new experiment in Machine Learning Studio using a Gallery example as a template.</span></span>

1. <span data-ttu-id="a42fb-134">Logga in med ditt Microsoft-konto autentiseringsuppgifter toohello [Studio](https://studio.azureml.net), och klicka sedan på **ny** toocreate ett experiment.</span><span class="sxs-lookup"><span data-stu-id="a42fb-134">Sign in with your Microsoft account credentials toohello [Studio](https://studio.azureml.net), and then click **New** toocreate an experiment.</span></span>
2. <span data-ttu-id="a42fb-135">Bläddra igenom hello exempel innehåll och klicka på ett.</span><span class="sxs-lookup"><span data-stu-id="a42fb-135">Browse through hello example content and click one.</span></span>

<span data-ttu-id="a42fb-136">Ett nytt experiment skapas i Machine Learning Studio arbetsytan med hello exempel experiment som en mall.</span><span class="sxs-lookup"><span data-stu-id="a42fb-136">A new experiment is created in your Machine Learning Studio workspace using hello example experiment as a template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a42fb-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a42fb-137">Next steps</span></span>
* [<span data-ttu-id="a42fb-138">Importera data från olika källor</span><span class="sxs-lookup"><span data-stu-id="a42fb-138">Import data from various sources</span></span>](machine-learning-data-science-import-data.md)
* [<span data-ttu-id="a42fb-139">Snabbstartsguide för hello R språket i Machine Learning</span><span class="sxs-lookup"><span data-stu-id="a42fb-139">Quickstart tutorial for hello R language in Machine Learning</span></span>](machine-learning-r-quickstart.md)
* [<span data-ttu-id="a42fb-140">Distribuera en Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="a42fb-140">Deploy a Machine Learning web service</span></span>](machine-learning-publish-a-machine-learning-web-service.md)
