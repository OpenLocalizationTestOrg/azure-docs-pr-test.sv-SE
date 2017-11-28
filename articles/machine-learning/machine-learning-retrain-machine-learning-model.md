---
title: "aaaRetrain en Maskininlärningsmodell | Microsoft Docs"
description: "Lär dig hur tooretrain en modell och uppdatera hello Web service toouse hello nyligen tränade modellen i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: d1cb6088-4f7c-4c32-94f2-f7523dad9059
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 342bb9954105339b4b634ff20968a64f4f9f750e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-machine-learning-model"></a><span data-ttu-id="051c2-103">Träna om Machine Learning-modellen</span><span class="sxs-lookup"><span data-stu-id="051c2-103">Retrain a Machine Learning Model</span></span>
<span data-ttu-id="051c2-104">Som en del av hello av operationalization för machine learning-modeller i Azure Machine Learning din modell tränas och sparas.</span><span class="sxs-lookup"><span data-stu-id="051c2-104">As part of hello process of operationalization of machine learning models in Azure Machine Learning, your model is trained and saved.</span></span> <span data-ttu-id="051c2-105">Du sedan använda den toocreate predicative webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="051c2-105">You then use it toocreate a predicative Web service.</span></span> <span data-ttu-id="051c2-106">hello webbtjänsten kan sedan användas i webbplatser, instrumentpaneler och mobilappar.</span><span class="sxs-lookup"><span data-stu-id="051c2-106">hello Web service can then be consumed in web sites, dashboards, and mobile apps.</span></span> 

<span data-ttu-id="051c2-107">Modeller som du skapar med Machine Learning finns vanligtvis inte statisk.</span><span class="sxs-lookup"><span data-stu-id="051c2-107">Models you create using Machine Learning are typically not static.</span></span> <span data-ttu-id="051c2-108">När nya data blir tillgängliga eller när hello konsumenter av hello API har måste sina egna data hello modellen toobe retrained.</span><span class="sxs-lookup"><span data-stu-id="051c2-108">As new data becomes available or when hello consumer of hello API has their own data hello model needs toobe retrained.</span></span> 

<span data-ttu-id="051c2-109">Omtränings kan inträffa ofta.</span><span class="sxs-lookup"><span data-stu-id="051c2-109">Retraining may occur frequently.</span></span> <span data-ttu-id="051c2-110">Med hello programmässiga Omtränings-API-funktionen, kan du programmässigt träna om hello modellen webbtjänsten hello Omtränings-API: er och uppdatera hello med hello nyligen tränad modell.</span><span class="sxs-lookup"><span data-stu-id="051c2-110">With hello Programmatic Retraining API feature, you can programmatically retrain hello model using hello Retraining APIs and update hello Web service with hello newly trained model.</span></span> 

<span data-ttu-id="051c2-111">Det här dokumentet beskriver hello omtränings processen och visar hur toouse hello Omtränings-API: er.</span><span class="sxs-lookup"><span data-stu-id="051c2-111">This document describes hello retraining process, and shows you how toouse hello Retraining APIs.</span></span>

## <a name="why-retrain-defining-hello-problem"></a><span data-ttu-id="051c2-112">Träna om varför: definiera hello problem</span><span class="sxs-lookup"><span data-stu-id="051c2-112">Why retrain: defining hello problem</span></span>
<span data-ttu-id="051c2-113">Som en del av hello maskininlärning utbildning process, är en modell tränas med hjälp av en uppsättning data.</span><span class="sxs-lookup"><span data-stu-id="051c2-113">As part of hello machine learning training process, a model is trained using a set of data.</span></span> <span data-ttu-id="051c2-114">Modeller som du skapar med Machine Learning finns vanligtvis inte statisk.</span><span class="sxs-lookup"><span data-stu-id="051c2-114">Models you create using Machine Learning are typically not static.</span></span> <span data-ttu-id="051c2-115">När nya data blir tillgängliga eller när hello konsumenter av hello API har måste sina egna data hello modellen toobe retrained.</span><span class="sxs-lookup"><span data-stu-id="051c2-115">As new data becomes available or when hello consumer of hello API has their own data hello model needs toobe retrained.</span></span>

<span data-ttu-id="051c2-116">I dessa scenarier programmässiga API ger ett bekvämt sätt tooallow du eller hello konsumenter av din API: er toocreate en klient som kan, på en gång eller regelbunden basis träna om hello modellen med sina egna data.</span><span class="sxs-lookup"><span data-stu-id="051c2-116">In these scenarios, a programmatic API provides a convenient way tooallow you or hello consumer of your APIs toocreate a client that can, on a one-time or regular basis, retrain hello model using their own data.</span></span> <span data-ttu-id="051c2-117">De kan sedan utvärdera hello resultaten av omtränings och uppdatera hello Web API toouse hello nyligen tränad modell.</span><span class="sxs-lookup"><span data-stu-id="051c2-117">They can then evaluate hello results of retraining, and update hello Web service API toouse hello newly trained model.</span></span>

> [!NOTE]
> <span data-ttu-id="051c2-118">Om du har en befintlig Träningsexperiment och nya webbtjänsten, kanske toocheck ut omtrimning en befintlig förutsägande webbtjänst i stället för följande hello genomgång som nämns i hello efter avsnittet.</span><span class="sxs-lookup"><span data-stu-id="051c2-118">If you have an existing Training Experiment and New Web service, you may want toocheck out Retrain an existing Predictive Web service instead of following hello walkthrough mentioned in hello following section.</span></span>
> 
> 

## <a name="end-to-end-workflow"></a><span data-ttu-id="051c2-119">Arbetsflödet slutpunkt till slutpunkt</span><span class="sxs-lookup"><span data-stu-id="051c2-119">End-to-end workflow</span></span>
<span data-ttu-id="051c2-120">hello innebär hello följande komponenter: A Träningsexperiment och Prediktivt Experiment publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="051c2-120">hello process involves hello following components: A Training Experiment and a Predictive Experiment published as a Web service.</span></span> <span data-ttu-id="051c2-121">tooenable omtränings av en tränad modell, hello Träningsexperiment måste publiceras som en webbtjänst med hello utdata från en tränad modell.</span><span class="sxs-lookup"><span data-stu-id="051c2-121">tooenable retraining of a trained model, hello Training Experiment must be published as a Web service with hello output of a trained model.</span></span> <span data-ttu-id="051c2-122">Detta gör att API toohello modell för omtränings.</span><span class="sxs-lookup"><span data-stu-id="051c2-122">This enables API access toohello model for retraining.</span></span> 

<span data-ttu-id="051c2-123">hello följande steg gäller tooboth nya och klassiska Web services:</span><span class="sxs-lookup"><span data-stu-id="051c2-123">hello following steps apply tooboth New and Classic Web services:</span></span>

<span data-ttu-id="051c2-124">Skapa hello inledande förutsägande webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="051c2-124">Create hello initial Predictive Web service:</span></span>

* <span data-ttu-id="051c2-125">Skapa ett experiment utbildning</span><span class="sxs-lookup"><span data-stu-id="051c2-125">Create a training experiment</span></span>
* <span data-ttu-id="051c2-126">Skapa en prediktiv web experiment</span><span class="sxs-lookup"><span data-stu-id="051c2-126">Create a predictive web experiment</span></span>
* <span data-ttu-id="051c2-127">Distribuera en prediktiv webbtjänst</span><span class="sxs-lookup"><span data-stu-id="051c2-127">Deploy a predictive web service</span></span>

<span data-ttu-id="051c2-128">Träna om hello-webbtjänsten:</span><span class="sxs-lookup"><span data-stu-id="051c2-128">Retrain hello Web service:</span></span>

* <span data-ttu-id="051c2-129">Uppdatera utbildning experiment tooallow för omtränings</span><span class="sxs-lookup"><span data-stu-id="051c2-129">Update training experiment tooallow for retraining</span></span>
* <span data-ttu-id="051c2-130">Distribuera hello omtränings-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="051c2-130">Deploy hello retraining web service</span></span>
* <span data-ttu-id="051c2-131">Använda hello Batch Execution Service code tooretrain hello modellen</span><span class="sxs-lookup"><span data-stu-id="051c2-131">Use hello Batch Execution Service code tooretrain hello model</span></span>

<span data-ttu-id="051c2-132">En genomgång av hello föregående steg finns [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="051c2-132">For a walkthrough of hello preceding steps, see [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span>

> [!NOTE] 
> <span data-ttu-id="051c2-133">toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="051c2-133">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="051c2-134">Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="051c2-134">For more information see, [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="051c2-135">Om du har distribuerat en klassiska webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="051c2-135">If you deployed a Classic Web Service:</span></span>

* <span data-ttu-id="051c2-136">Skapa en ny slutpunkt på hello förutsägande webbtjänst</span><span class="sxs-lookup"><span data-stu-id="051c2-136">Create a new Endpoint on hello Predictive Web service</span></span>
* <span data-ttu-id="051c2-137">Hämta hello korrigering URL och kod</span><span class="sxs-lookup"><span data-stu-id="051c2-137">Get hello PATCH URL and code</span></span>
* <span data-ttu-id="051c2-138">Använd hello korrigering URL toopoint hello ny slutpunkt på hello retrained modellen</span><span class="sxs-lookup"><span data-stu-id="051c2-138">Use hello PATCH URL toopoint hello new Endpoint at hello retrained model</span></span> 

<span data-ttu-id="051c2-139">En genomgång av hello föregående steg finns [träna om en klassiska webbtjänst](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="051c2-139">For a walkthrough of hello preceding steps, see [Retrain a Classic Web service](machine-learning-retrain-a-classic-web-service.md).</span></span>

<span data-ttu-id="051c2-140">Om du stöter på problem med omtränings klassiska webbtjänsten finns [felsökning hello omtränings av en webbtjänst för Azure Machine Learning klassiska](machine-learning-troubleshooting-retraining-models.md).</span><span class="sxs-lookup"><span data-stu-id="051c2-140">If you run into difficulties retraining a Classic Web service, see [Troubleshooting hello retraining of an Azure Machine Learning Classic Web service](machine-learning-troubleshooting-retraining-models.md).</span></span>

<span data-ttu-id="051c2-141">Om du har distribuerat en ny webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="051c2-141">If you deployed a New Web service:</span></span>

* <span data-ttu-id="051c2-142">Logga in tooyour Azure Resource Manager-konto</span><span class="sxs-lookup"><span data-stu-id="051c2-142">Sign in tooyour Azure Resource Manager account</span></span>
* <span data-ttu-id="051c2-143">Hämta hello Web service definition</span><span class="sxs-lookup"><span data-stu-id="051c2-143">Get hello Web service definition</span></span>
* <span data-ttu-id="051c2-144">Exportera hello Web Service Definition som JSON</span><span class="sxs-lookup"><span data-stu-id="051c2-144">Export hello Web Service Definition as JSON</span></span>
* <span data-ttu-id="051c2-145">Uppdatera hello referens toohello `ilearner` blob i hello JSON</span><span class="sxs-lookup"><span data-stu-id="051c2-145">Update hello reference toohello `ilearner` blob in hello JSON</span></span>
* <span data-ttu-id="051c2-146">Importera hello JSON till en Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="051c2-146">Import hello JSON into a Web Service Definition</span></span>
* <span data-ttu-id="051c2-147">Uppdatera hello-webbtjänsten med nya Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="051c2-147">Update hello Web service with new Web Service Definition</span></span>

<span data-ttu-id="051c2-148">En genomgång av hello föregående steg finns [träna om en ny webbtjänst med Machine Learning Management PowerShell-cmdlets för hello](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="051c2-148">For a walkthrough of hello preceding steps, see [Retrain a New Web service using hello Machine Learning Management PowerShell cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<span data-ttu-id="051c2-149">hello innebär för att ställa in omtränings för det klassiska hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="051c2-149">hello process for setting up retraining for a Classic Web service involves hello following steps:</span></span>

![Översikt över omtränings][1]

<span data-ttu-id="051c2-151">hello innebär för att ställa in omtränings för en ny webbtjänst hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="051c2-151">hello process for setting up retraining for a New Web service involves hello following steps:</span></span>

![Översikt över omtränings][7]

## <a name="other-resources"></a><span data-ttu-id="051c2-153">Andra resurser</span><span class="sxs-lookup"><span data-stu-id="051c2-153">Other Resources</span></span>
* [<span data-ttu-id="051c2-154">Omtränings och uppdaterar Azure Machine Learning-modeller med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="051c2-154">Retraining and Updating Azure Machine Learning models with Azure Data Factory</span></span>](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [<span data-ttu-id="051c2-155">Skapa många Machine Learning-modeller och web service slutpunkter från ett experiment med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="051c2-155">Create many Machine Learning models and web service endpoints from one experiment using PowerShell</span></span>](machine-learning-create-models-and-endpoints-with-powershell.md)
* <span data-ttu-id="051c2-156">Hej [AML Omtränings modeller med hjälp av API: er](https://www.youtube.com/watch?v=wwjglA8xllg) videon visar hur tooretrain Machine Learning-modeller skapas i Azure Machine Learning med hello Omtränings-API: er och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="051c2-156">hello [AML Retraining Models Using APIs](https://www.youtube.com/watch?v=wwjglA8xllg) video shows you how tooretrain Machine Learning models created in Azure Machine Learning using hello Retraining APIs and PowerShell.</span></span>

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

