---
title: "Kopiera exempelexperiment för maskininlärning – Azure | Microsoft Docs"
description: "Lär dig hur du använder exempelexperiment för maskininlärning för att skapa nya experiment med Cortana Intelligence-galleriet och Microsoft Azure Machine Learning."
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
ms.openlocfilehash: 55f9bd2ed0d555a14d31bf3d262707d65bd70244
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="copy-example-experiments-to-create-new-machine-learning-experiments"></a><span data-ttu-id="42c12-104">Kopiera exempelexperiment för att skapa nya maskininlärningsexperiment</span><span class="sxs-lookup"><span data-stu-id="42c12-104">Copy example experiments to create new machine learning experiments</span></span>
<span data-ttu-id="42c12-105">Lär dig hur du kommer igång med exempelexperiment från [Cortana Intelligence-galleriet](https://gallery.cortanaintelligence.com/) i stället för att skapa maskininlärningsexperiment från grunden.</span><span class="sxs-lookup"><span data-stu-id="42c12-105">Learn how to start with example experiments from [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) instead of creating machine learning experiments from scratch.</span></span> <span data-ttu-id="42c12-106">Du kan använda exemplen för att skapa din egen maskininlärningslösning.</span><span class="sxs-lookup"><span data-stu-id="42c12-106">You can use the examples to build your own machine learning solution.</span></span>

<span data-ttu-id="42c12-107">I galleriet finns exempelexperiment från Microsoft Azure Machine Learning-teamet samt exempel som delats av Machine Learning-communityn.</span><span class="sxs-lookup"><span data-stu-id="42c12-107">The gallery has example experiments by the Microsoft Azure Machine Learning team as well as examples shared by the Machine Learning community.</span></span> <span data-ttu-id="42c12-108">Du kan även ställa frågor eller lämna kommentarer om experiment.</span><span class="sxs-lookup"><span data-stu-id="42c12-108">You also can ask questions or post comments about experiments.</span></span>

<span data-ttu-id="42c12-109">Om du vill se hur du använder galleriet kan du titta på den 3 minuter långa videon [Copy other people's work to do data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) (Kopiera andras arbete för datavetenskap) från serien [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) (Datavetenskap för nybörjare).</span><span class="sxs-lookup"><span data-stu-id="42c12-109">To see how to use the gallery, watch the 3-minute video [Copy other people's work to do data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) from the series [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-to-copy-in-cortana-intelligence-gallery"></a><span data-ttu-id="42c12-110">Hitta ett experiment att kopiera i Cortana Intelligence-galleriet</span><span class="sxs-lookup"><span data-stu-id="42c12-110">Find an experiment to copy in Cortana Intelligence Gallery</span></span>
<span data-ttu-id="42c12-111">Du kan se vilka experiment som är tillgängliga genom att öppna [Galleriet](https://gallery.cortanaintelligence.com/) och klicka på **Experiment** högst uppe på sidan.</span><span class="sxs-lookup"><span data-stu-id="42c12-111">To see what experiments are available, go to the [Gallery](https://gallery.cortanaintelligence.com/) and click **Experiments** at the top of the page.</span></span>

### <a name="find-the-newest-or-most-popular-experiments"></a><span data-ttu-id="42c12-112">Hitta de senaste eller mest populära experimenten</span><span class="sxs-lookup"><span data-stu-id="42c12-112">Find the newest or most popular experiments</span></span>
<span data-ttu-id="42c12-113">På den här sidan kan du se **nyligen tillagda** experiment eller bläddra neråt för att se **populära experiment** eller de senaste **populära Microsoft-experimenten**.</span><span class="sxs-lookup"><span data-stu-id="42c12-113">On this page, you can see **Recently added** experiments, or scroll down to look at **What's popular** or the latest **Popular Microsoft experiments**.</span></span>

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a><span data-ttu-id="42c12-114">Leta efter ett experiment som uppfyller specifika krav</span><span class="sxs-lookup"><span data-stu-id="42c12-114">Look for an experiment that meets specific requirements</span></span>
<span data-ttu-id="42c12-115">Bläddra bland alla experiment:</span><span class="sxs-lookup"><span data-stu-id="42c12-115">To browse all experiments:</span></span>

1. <span data-ttu-id="42c12-116">Klicka på **Browse all** längst upp på sidan.</span><span class="sxs-lookup"><span data-stu-id="42c12-116">Click **Browse all** at the top of the page.</span></span>
2. <span data-ttu-id="42c12-117">Till vänster, under **Förfina efter** i avsnittet **Kategorier**, väljer du **Experiment** för att visa alla experiment i galleriet.</span><span class="sxs-lookup"><span data-stu-id="42c12-117">On the left-hand side, under **Refine by** in the **Categories** section, select **Experiment** to see all the experiments in the Gallery.</span></span>
3. <span data-ttu-id="42c12-118">Du kan hitta experiment som uppfyller kraven på ett par olika sätt:</span><span class="sxs-lookup"><span data-stu-id="42c12-118">You can find experiments that meet your requirements a couple different ways:</span></span>
   * <span data-ttu-id="42c12-119">**Välj filter till vänster.**</span><span class="sxs-lookup"><span data-stu-id="42c12-119">**Select filters on the left.**</span></span> <span data-ttu-id="42c12-120">Om du till exempel vill bläddra igenom experiment som använder en algoritm för PCA-baserad avvikelseidentifiering gör du så här: När du har valt **Experiment** under **Kategorier** klickar du på **Visa alla**.</span><span class="sxs-lookup"><span data-stu-id="42c12-120">For example, to browse experiments that use a PCA-based anomaly detection algorithm: With **Experiment** selected under **Categories**, click **Show all**.</span></span> <span data-ttu-id="42c12-121">Sedan väljer du **PCA-Based Anomaly Detection** (PCA-baserad avvikelseidentifiering) under **Algorithms Used** (Algoritmer som används).</span><span class="sxs-lookup"><span data-stu-id="42c12-121">Then, under **Algorithms Used**, choose **PCA-Based Anomaly Detection**.</span></span> <br></br><span data-ttu-id="42c12-122">
     ![Välj filter](./media/machine-learning-sample-experiments/refine-the-view.png)</span><span class="sxs-lookup"><span data-stu-id="42c12-122">
![Select filters](./media/machine-learning-sample-experiments/refine-the-view.png)</span></span>
   * <span data-ttu-id="42c12-123">**Använd sökrutan.**</span><span class="sxs-lookup"><span data-stu-id="42c12-123">**Use the search box.**</span></span> <span data-ttu-id="42c12-124">Om du till exempel vill hitta experiment från Microsoft som rör sifferigenkänning som använder en algoritm för stödvektormaskin med två klasser anger du ”digit recognition” i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="42c12-124">For example, to find experiments contributed by Microsoft related to digit recognition that use a two-class support vector machine algorithm, enter "digit recognition" in the search box.</span></span> <span data-ttu-id="42c12-125">Välj sedan filtren **Experiment**, **Endast Microsoft innehåll** och **Stödvektormaskin med två klasser**:</span><span class="sxs-lookup"><span data-stu-id="42c12-125">Then, select the filters **Experiment**, **Microsoft content only**, and **Two-Class Support Vector Machine**:</span></span><br></br><span data-ttu-id="42c12-126">
     ![Använd sökrutan](./media/machine-learning-sample-experiments/search-for-experiments.png)</span><span class="sxs-lookup"><span data-stu-id="42c12-126">
![Use the search box](./media/machine-learning-sample-experiments/search-for-experiments.png)</span></span>
4. <span data-ttu-id="42c12-127">Klicka på ett experiment om du vill veta mer om det.</span><span class="sxs-lookup"><span data-stu-id="42c12-127">Click an experiment to learn more about it.</span></span>
5. <span data-ttu-id="42c12-128">Om du vill köra och/eller ändra experimentet klickar du på **Open in Studio** på experimentsidan.</span><span class="sxs-lookup"><span data-stu-id="42c12-128">To run and/or modify the experiment, click **Open in Studio** on the experiment's page.</span></span> <br></br>

    ![Exempelexperiment](./media/machine-learning-sample-experiments/example-experiment.png)

    > [!NOTE]
    > <span data-ttu-id="42c12-130">När du öppnar ett experiment i Machine Learning Studio för första gången kan du prova det kostnadsfritt eller köpa en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="42c12-130">When you open an experiment in Machine Learning Studio for the first time, you can try it for free or buy an Azure subscription.</span></span> [<span data-ttu-id="42c12-131">Lär dig mer om den kostnadsfria utvärderingsversionen och den betalda tjänsten Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="42c12-131">Learn about the Machine Learning Studio free trial vs. paid service</span></span>](https://azure.microsoft.com/pricing/details/machine-learning/)
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a><span data-ttu-id="42c12-132">Skapa ett nytt experiment med ett exempel som mall</span><span class="sxs-lookup"><span data-stu-id="42c12-132">Create a new experiment using an example as a template</span></span>
<span data-ttu-id="42c12-133">Du kan också skapa ett nytt experiment i Machine Learning Studio genom att använda ett exempel från galleriet som mall.</span><span class="sxs-lookup"><span data-stu-id="42c12-133">You also can create a new experiment in Machine Learning Studio using a Gallery example as a template.</span></span>

1. <span data-ttu-id="42c12-134">Skapa ett experiment genom att logga in i [Studio](https://studio.azureml.net) med dina autentiseringsuppgifter för Microsoft-kontot och klicka sedan på **Nytt**.</span><span class="sxs-lookup"><span data-stu-id="42c12-134">Sign in with your Microsoft account credentials to the [Studio](https://studio.azureml.net), and then click **New** to create an experiment.</span></span>
2. <span data-ttu-id="42c12-135">Bläddra igenom exemplen och klicka på ett.</span><span class="sxs-lookup"><span data-stu-id="42c12-135">Browse through the example content and click one.</span></span>

<span data-ttu-id="42c12-136">Ett nytt experiment skapas på Machine Learning Studio-arbetsytan, som använder exempelexperimentet som mall.</span><span class="sxs-lookup"><span data-stu-id="42c12-136">A new experiment is created in your Machine Learning Studio workspace using the example experiment as a template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42c12-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="42c12-137">Next steps</span></span>
* [<span data-ttu-id="42c12-138">Importera data från olika källor</span><span class="sxs-lookup"><span data-stu-id="42c12-138">Import data from various sources</span></span>](machine-learning-data-science-import-data.md)
* [<span data-ttu-id="42c12-139">Snabbstartssjälvstudier till R-språket i Machine Learning</span><span class="sxs-lookup"><span data-stu-id="42c12-139">Quickstart tutorial for the R language in Machine Learning</span></span>](machine-learning-r-quickstart.md)
* [<span data-ttu-id="42c12-140">Distribuera en Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="42c12-140">Deploy a Machine Learning web service</span></span>](machine-learning-publish-a-machine-learning-web-service.md)
