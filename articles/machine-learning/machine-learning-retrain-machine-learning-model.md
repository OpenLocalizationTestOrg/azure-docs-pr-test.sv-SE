---
title: "Träna om Machine Learning-modellen | Microsoft Docs"
description: "Lär dig mer om att träna om en modell och uppdatera webbtjänsten för att använda den nya tränade modellen i Azure Machine Learning."
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
ms.openlocfilehash: f86c2bc41dd7ff0bc31454a56b84d136dc7d026c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="retrain-a-machine-learning-model"></a><span data-ttu-id="9e631-103">Träna om Machine Learning-modellen</span><span class="sxs-lookup"><span data-stu-id="9e631-103">Retrain a Machine Learning Model</span></span>
<span data-ttu-id="9e631-104">Som en del av processen för operationalization för machine learning-modeller i Azure Machine Learning, din modell tränas och sparas.</span><span class="sxs-lookup"><span data-stu-id="9e631-104">As part of the process of operationalization of machine learning models in Azure Machine Learning, your model is trained and saved.</span></span> <span data-ttu-id="9e631-105">Du sedan använda den för att skapa en predicative webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="9e631-105">You then use it to create a predicative Web service.</span></span> <span data-ttu-id="9e631-106">Webbtjänsten kan sedan användas i webbplatser, instrumentpaneler och mobilappar.</span><span class="sxs-lookup"><span data-stu-id="9e631-106">The Web service can then be consumed in web sites, dashboards, and mobile apps.</span></span> 

<span data-ttu-id="9e631-107">Modeller som du skapar med Machine Learning finns vanligtvis inte statisk.</span><span class="sxs-lookup"><span data-stu-id="9e631-107">Models you create using Machine Learning are typically not static.</span></span> <span data-ttu-id="9e631-108">När nya data blir tillgängliga eller när konsumenten API har sina egna data måste vara retrained modellen.</span><span class="sxs-lookup"><span data-stu-id="9e631-108">As new data becomes available or when the consumer of the API has their own data the model needs to be retrained.</span></span> 

<span data-ttu-id="9e631-109">Omtränings kan inträffa ofta.</span><span class="sxs-lookup"><span data-stu-id="9e631-109">Retraining may occur frequently.</span></span> <span data-ttu-id="9e631-110">Med funktionen programmässiga Omtränings-API du programmässigt träna om modellen med Omtränings-API och uppdatera webbtjänsten med den nyligen tränade modellen.</span><span class="sxs-lookup"><span data-stu-id="9e631-110">With the Programmatic Retraining API feature, you can programmatically retrain the model using the Retraining APIs and update the Web service with the newly trained model.</span></span> 

<span data-ttu-id="9e631-111">Det här dokumentet beskrivs hur du omtränings och visar hur du använder Omtränings-API.</span><span class="sxs-lookup"><span data-stu-id="9e631-111">This document describes the retraining process, and shows you how to use the Retraining APIs.</span></span>

## <a name="why-retrain-defining-the-problem"></a><span data-ttu-id="9e631-112">Träna om varför: definiera problemet</span><span class="sxs-lookup"><span data-stu-id="9e631-112">Why retrain: defining the problem</span></span>
<span data-ttu-id="9e631-113">Som en del av maskininlärning utbildning process, är en modell tränas med hjälp av en uppsättning data.</span><span class="sxs-lookup"><span data-stu-id="9e631-113">As part of the machine learning training process, a model is trained using a set of data.</span></span> <span data-ttu-id="9e631-114">Modeller som du skapar med Machine Learning finns vanligtvis inte statisk.</span><span class="sxs-lookup"><span data-stu-id="9e631-114">Models you create using Machine Learning are typically not static.</span></span> <span data-ttu-id="9e631-115">När nya data blir tillgängliga eller när konsumenten API har sina egna data måste vara retrained modellen.</span><span class="sxs-lookup"><span data-stu-id="9e631-115">As new data becomes available or when the consumer of the API has their own data the model needs to be retrained.</span></span>

<span data-ttu-id="9e631-116">I dessa scenarier ger programmässiga API ett bekvämt sätt att låta dig eller konsumenter av din API: er att skapa en klient som kan, på en gång eller regelbunden basis träna om modellen med sina egna data.</span><span class="sxs-lookup"><span data-stu-id="9e631-116">In these scenarios, a programmatic API provides a convenient way to allow you or the consumer of your APIs to create a client that can, on a one-time or regular basis, retrain the model using their own data.</span></span> <span data-ttu-id="9e631-117">De kan sedan utvärdera resultaten av omtränings och uppdatera Web service API för att använda den nya tränade modellen.</span><span class="sxs-lookup"><span data-stu-id="9e631-117">They can then evaluate the results of retraining, and update the Web service API to use the newly trained model.</span></span>

> [!NOTE]
> <span data-ttu-id="9e631-118">Om du har en befintlig Träningsexperiment och nya Web service kanske du vill checka ut omtrimning en befintlig förutsägande webbtjänst i stället för att följa den här genomgången som nämns i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9e631-118">If you have an existing Training Experiment and New Web service, you may want to check out Retrain an existing Predictive Web service instead of following the walkthrough mentioned in the following section.</span></span>
> 
> 

## <a name="end-to-end-workflow"></a><span data-ttu-id="9e631-119">Slutpunkt till slutpunkt-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="9e631-119">End-to-end workflow</span></span>
<span data-ttu-id="9e631-120">Processen omfattar följande komponenter: A Träningsexperiment och Prediktivt Experiment publiceras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="9e631-120">The process involves the following components: A Training Experiment and a Predictive Experiment published as a Web service.</span></span> <span data-ttu-id="9e631-121">Om du vill aktivera omtränings av en tränad modell måste utbildning experimentet publiceras som en webbtjänst med utdata från en tränad modell.</span><span class="sxs-lookup"><span data-stu-id="9e631-121">To enable retraining of a trained model, the Training Experiment must be published as a Web service with the output of a trained model.</span></span> <span data-ttu-id="9e631-122">Detta gör det möjligt för API-åtkomst till modellen för omtränings.</span><span class="sxs-lookup"><span data-stu-id="9e631-122">This enables API access to the model for retraining.</span></span> 

<span data-ttu-id="9e631-123">Följande steg gäller för både nya och klassiska Web services:</span><span class="sxs-lookup"><span data-stu-id="9e631-123">The following steps apply to both New and Classic Web services:</span></span>

<span data-ttu-id="9e631-124">Skapa första förutsägande webbtjänsten:</span><span class="sxs-lookup"><span data-stu-id="9e631-124">Create the initial Predictive Web service:</span></span>

* <span data-ttu-id="9e631-125">Skapa ett experiment utbildning</span><span class="sxs-lookup"><span data-stu-id="9e631-125">Create a training experiment</span></span>
* <span data-ttu-id="9e631-126">Skapa en prediktiv web experiment</span><span class="sxs-lookup"><span data-stu-id="9e631-126">Create a predictive web experiment</span></span>
* <span data-ttu-id="9e631-127">Distribuera en prediktiv webbtjänst</span><span class="sxs-lookup"><span data-stu-id="9e631-127">Deploy a predictive web service</span></span>

<span data-ttu-id="9e631-128">Träna om webbtjänsten:</span><span class="sxs-lookup"><span data-stu-id="9e631-128">Retrain the Web service:</span></span>

* <span data-ttu-id="9e631-129">Uppdatera träningsexperiment för omtränings</span><span class="sxs-lookup"><span data-stu-id="9e631-129">Update training experiment to allow for retraining</span></span>
* <span data-ttu-id="9e631-130">Distribuera omtränings-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="9e631-130">Deploy the retraining web service</span></span>
* <span data-ttu-id="9e631-131">Använda Batch Execution Service-kod för att träna om modellen</span><span class="sxs-lookup"><span data-stu-id="9e631-131">Use the Batch Execution Service code to retrain the model</span></span>

<span data-ttu-id="9e631-132">En genomgång av den föregående steg finns [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="9e631-132">For a walkthrough of the preceding steps, see [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span>

> [!NOTE] 
> <span data-ttu-id="9e631-133">Om du vill distribuera en ny webbtjänst måste du ha tillräckliga behörigheter i prenumerationen som du distribuerar webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="9e631-133">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="9e631-134">Mer information finns i [hantera en webbtjänst med hjälp av Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="9e631-134">For more information see, [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="9e631-135">Om du har distribuerat en klassiska webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="9e631-135">If you deployed a Classic Web Service:</span></span>

* <span data-ttu-id="9e631-136">Skapa en ny slutpunkt på förutsägande webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="9e631-136">Create a new Endpoint on the Predictive Web service</span></span>
* <span data-ttu-id="9e631-137">Hämta korrigering URL och kod</span><span class="sxs-lookup"><span data-stu-id="9e631-137">Get the PATCH URL and code</span></span>
* <span data-ttu-id="9e631-138">Använda PATCH-URL så att den pekar på ny slutpunkt på retrained modellen</span><span class="sxs-lookup"><span data-stu-id="9e631-138">Use the PATCH URL to point the new Endpoint at the retrained model</span></span> 

<span data-ttu-id="9e631-139">En genomgång av den föregående steg finns [träna om en klassiska webbtjänst](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="9e631-139">For a walkthrough of the preceding steps, see [Retrain a Classic Web service](machine-learning-retrain-a-classic-web-service.md).</span></span>

<span data-ttu-id="9e631-140">Om du stöter på problem med omtränings klassiska webbtjänsten finns [felsökning omtränings av en webbtjänst för Azure Machine Learning klassiska](machine-learning-troubleshooting-retraining-models.md).</span><span class="sxs-lookup"><span data-stu-id="9e631-140">If you run into difficulties retraining a Classic Web service, see [Troubleshooting the retraining of an Azure Machine Learning Classic Web service](machine-learning-troubleshooting-retraining-models.md).</span></span>

<span data-ttu-id="9e631-141">Om du har distribuerat en ny webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="9e631-141">If you deployed a New Web service:</span></span>

* <span data-ttu-id="9e631-142">Logga in på Azure Resource Manager-konto</span><span class="sxs-lookup"><span data-stu-id="9e631-142">Sign in to your Azure Resource Manager account</span></span>
* <span data-ttu-id="9e631-143">Hämta Web service definition</span><span class="sxs-lookup"><span data-stu-id="9e631-143">Get the Web service definition</span></span>
* <span data-ttu-id="9e631-144">Exportera Web Service Definition som JSON</span><span class="sxs-lookup"><span data-stu-id="9e631-144">Export the Web Service Definition as JSON</span></span>
* <span data-ttu-id="9e631-145">Uppdatera referens till den `ilearner` blob i JSON</span><span class="sxs-lookup"><span data-stu-id="9e631-145">Update the reference to the `ilearner` blob in the JSON</span></span>
* <span data-ttu-id="9e631-146">Importera JSON till en Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="9e631-146">Import the JSON into a Web Service Definition</span></span>
* <span data-ttu-id="9e631-147">Uppdatera webbtjänsten med nya Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="9e631-147">Update the Web service with new Web Service Definition</span></span>

<span data-ttu-id="9e631-148">En genomgång av den föregående steg finns [träna om en ny webbtjänst med hjälp av Machine Learning Management PowerShell-cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="9e631-148">For a walkthrough of the preceding steps, see [Retrain a New Web service using the Machine Learning Management PowerShell cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<span data-ttu-id="9e631-149">Processen för att ställa in omtränings för det klassiska omfattar följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e631-149">The process for setting up retraining for a Classic Web service involves the following steps:</span></span>

![Översikt över omtränings][1]

<span data-ttu-id="9e631-151">Processen för att ställa in omtränings för en ny webbtjänst omfattar följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e631-151">The process for setting up retraining for a New Web service involves the following steps:</span></span>

![Översikt över omtränings][7]

## <a name="other-resources"></a><span data-ttu-id="9e631-153">Andra resurser</span><span class="sxs-lookup"><span data-stu-id="9e631-153">Other Resources</span></span>
* [<span data-ttu-id="9e631-154">Omtränings och uppdaterar Azure Machine Learning-modeller med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="9e631-154">Retraining and Updating Azure Machine Learning models with Azure Data Factory</span></span>](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [<span data-ttu-id="9e631-155">Skapa många Machine Learning-modeller och web service slutpunkter från ett experiment med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="9e631-155">Create many Machine Learning models and web service endpoints from one experiment using PowerShell</span></span>](machine-learning-create-models-and-endpoints-with-powershell.md)
* <span data-ttu-id="9e631-156">Den [AML Omtränings modeller med hjälp av API: er](https://www.youtube.com/watch?v=wwjglA8xllg) videon visar hur du träna om Machine Learning-modeller som skapats i Azure Machine Learning med Omtränings-API: er och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9e631-156">The [AML Retraining Models Using APIs](https://www.youtube.com/watch?v=wwjglA8xllg) video shows you how to retrain Machine Learning models created in Azure Machine Learning using the Retraining APIs and PowerShell.</span></span>

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

