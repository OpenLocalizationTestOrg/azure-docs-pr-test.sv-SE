---
title: "Steg 5: Distribuera hello webbtjänsten för Machine Learning | Microsoft Docs"
description: "Steg 5 i hello utveckla en förutsägelselösning genomgång: distribuera en prediktivt experiment i Machine Learning Studio som en webbtjänst."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 3fca74a3-c44b-4583-a218-c14c46ee5338
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 76391010972ed1450bbda8bfb2352c7b22b51ccc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-5-deploy-hello-azure-machine-learning-web-service"></a><span data-ttu-id="dbe69-103">Genomgång steg 5: Distribuera hello Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="dbe69-103">Walkthrough Step 5: Deploy hello Azure Machine Learning web service</span></span>
<span data-ttu-id="dbe69-104">Det här är hello femte steg i hello genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="dbe69-104">This is hello fifth step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="dbe69-105">Skapa en Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="dbe69-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="dbe69-106">Överför befintliga data</span><span class="sxs-lookup"><span data-stu-id="dbe69-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="dbe69-107">Skapa ett nytt experiment</span><span class="sxs-lookup"><span data-stu-id="dbe69-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="dbe69-108">Träna och utvärdera hello modeller</span><span class="sxs-lookup"><span data-stu-id="dbe69-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. <span data-ttu-id="dbe69-109">**Distribuera hello-webbtjänst**</span><span class="sxs-lookup"><span data-stu-id="dbe69-109">**Deploy hello web service**</span></span>
6. [<span data-ttu-id="dbe69-110">Åtkomst hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="dbe69-110">Access hello web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="dbe69-111">toogive andra en chansen toouse hello förutsägelsemodell som vi har utvecklat i den här genomgången, vi kan distribuera den som en webbtjänst på Azure.</span><span class="sxs-lookup"><span data-stu-id="dbe69-111">toogive others a chance toouse hello predictive model we've developed in this walkthrough, we can deploy it as a web service on Azure.</span></span>

<span data-ttu-id="dbe69-112">Vi har experimentera med vår modell för in toothis punkt.</span><span class="sxs-lookup"><span data-stu-id="dbe69-112">Up toothis point we've been experimenting with training our model.</span></span> <span data-ttu-id="dbe69-113">Men hello distribuerade tjänsten kommer inte längre toodo utbildning - vad vi toogenerate nya förutsägelser av bedömningen hello användarens indata baserat på vår modell.</span><span class="sxs-lookup"><span data-stu-id="dbe69-113">But hello deployed service is no longer going toodo training - it's going toogenerate new predictions by scoring hello user's input based on our model.</span></span> <span data-ttu-id="dbe69-114">Så vi toodo vissa förberedelser tooconvert detta experiment från en ***utbildning*** experimentera tooa ***förutsägande*** experiment.</span><span class="sxs-lookup"><span data-stu-id="dbe69-114">So we're going toodo some preparation tooconvert this experiment from a ***training*** experiment tooa ***predictive*** experiment.</span></span> 

<span data-ttu-id="dbe69-115">Detta är en process i tre steg:</span><span class="sxs-lookup"><span data-stu-id="dbe69-115">This is a three-step process:</span></span>  

1. <span data-ttu-id="dbe69-116">Ta bort en hello modeller</span><span class="sxs-lookup"><span data-stu-id="dbe69-116">Remove one of hello models</span></span>
2. <span data-ttu-id="dbe69-117">Konvertera hello *träningsexperiment* har vi skapat till en *prediktivt experiment*</span><span class="sxs-lookup"><span data-stu-id="dbe69-117">Convert hello *training experiment* we've created into a *predictive experiment*</span></span>
3. <span data-ttu-id="dbe69-118">Distribuera hello prediktivt experiment som en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="dbe69-118">Deploy hello predictive experiment as a web service</span></span>

## <a name="remove-one-of-hello-models"></a><span data-ttu-id="dbe69-119">Ta bort en hello modeller</span><span class="sxs-lookup"><span data-stu-id="dbe69-119">Remove one of hello models</span></span>

<span data-ttu-id="dbe69-120">Först måste vi tootrim experimentet ned lite.</span><span class="sxs-lookup"><span data-stu-id="dbe69-120">First, we need tootrim this experiment down a little.</span></span> <span data-ttu-id="dbe69-121">Vi har två olika modeller i hello experiment, men vi vill bara toouse en modell när vi distribuera den som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="dbe69-121">We currently have two different models in hello experiment, but we only want toouse one model when we deploy this as a web service.</span></span>  

<span data-ttu-id="dbe69-122">Anta att vi har valt att hello ökat trädet modellen presterade bättre än hello SVM modellen.</span><span class="sxs-lookup"><span data-stu-id="dbe69-122">Let's say we've decided that hello boosted tree model performed better than hello SVM model.</span></span> <span data-ttu-id="dbe69-123">Så ta bort hello hello första sak toodo [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulen och hello-moduler som användes för att träna den.</span><span class="sxs-lookup"><span data-stu-id="dbe69-123">So hello first thing toodo is remove hello [Two-Class Support Vector Machine][two-class-support-vector-machine] module and hello modules that were used for training it.</span></span> <span data-ttu-id="dbe69-124">Du kanske vill toomake en kopia av hello experiment först genom att klicka på **Spara som** längst hello hello experimentera arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="dbe69-124">You may want toomake a copy of hello experiment first by clicking **Save As** at hello bottom of hello experiment canvas.</span></span>

<span data-ttu-id="dbe69-125">Vi behöver toodelete hello följande moduler:</span><span class="sxs-lookup"><span data-stu-id="dbe69-125">We need toodelete hello following modules:</span></span>  

* <span data-ttu-id="dbe69-126">[Two-Class Support Vector Machine][two-class-support-vector-machine]</span><span class="sxs-lookup"><span data-stu-id="dbe69-126">[Two-Class Support Vector Machine][two-class-support-vector-machine]</span></span>
* <span data-ttu-id="dbe69-127">[Träna modellen] [ train-model] och [Poängmodell] [ score-model] moduler som var anslutna tooit</span><span class="sxs-lookup"><span data-stu-id="dbe69-127">[Train Model][train-model] and [Score Model][score-model] modules that were connected tooit</span></span>
* <span data-ttu-id="dbe69-128">[Normalisera Data] [ normalize-data] (båda)</span><span class="sxs-lookup"><span data-stu-id="dbe69-128">[Normalize Data][normalize-data] (both of them)</span></span>
* <span data-ttu-id="dbe69-129">[Utvärdera modellen] [ evaluate-model] (eftersom vi är klar utvärderar hello modeller)</span><span class="sxs-lookup"><span data-stu-id="dbe69-129">[Evaluate Model][evaluate-model] (because we're finished evaluating hello models)</span></span>

<span data-ttu-id="dbe69-130">Markera varje modul och tryck på hello del. eller högerklicka på hello modulen och välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="dbe69-130">Select each module and press hello Delete key, or right-click hello module and select **Delete**.</span></span> 

![Ta bort hello SVM modellen][3a]

<span data-ttu-id="dbe69-132">Vår modell bör nu se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="dbe69-132">Our model should now look something like this:</span></span>

![Ta bort hello SVM modellen][3]

<span data-ttu-id="dbe69-134">Nu är klar toodeploy detta modellen med hello [två-Tvåklassförhöjt beslutsträd][two-class-boosted-decision-tree].</span><span class="sxs-lookup"><span data-stu-id="dbe69-134">Now we're ready toodeploy this model using hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree].</span></span>

## <a name="convert-hello-training-experiment-tooa-predictive-experiment"></a><span data-ttu-id="dbe69-135">Konvertera hello utbildning experiment tooa prediktivt experiment</span><span class="sxs-lookup"><span data-stu-id="dbe69-135">Convert hello training experiment tooa predictive experiment</span></span>

<span data-ttu-id="dbe69-136">tooget detta modellen är redo för distribution måste vi tooconvert detta utbildning experiment tooa prediktivt experiment.</span><span class="sxs-lookup"><span data-stu-id="dbe69-136">tooget this model ready for deployment, we need tooconvert this training experiment tooa predictive experiment.</span></span> <span data-ttu-id="dbe69-137">Detta omfattar tre steg:</span><span class="sxs-lookup"><span data-stu-id="dbe69-137">This involves three steps:</span></span>

1. <span data-ttu-id="dbe69-138">Spara hello modell vi har tränat och Ersätt vår utbildningsmoduler</span><span class="sxs-lookup"><span data-stu-id="dbe69-138">Save hello model we've trained and then replace our training modules</span></span>
2. <span data-ttu-id="dbe69-139">Trim hello experiment tooremove moduler som behövdes endast för utbildning</span><span class="sxs-lookup"><span data-stu-id="dbe69-139">Trim hello experiment tooremove modules that were only needed for training</span></span>
3. <span data-ttu-id="dbe69-140">Definiera där hello-webbtjänsten ska ta emot indata och där det genererar hello-utdata</span><span class="sxs-lookup"><span data-stu-id="dbe69-140">Define where hello web service will accept input and where it generates hello output</span></span>

<span data-ttu-id="dbe69-141">Vi kan göra detta manuellt, men som tur är alla tre stegen kan utföras genom att klicka på **konfigurera Web Service** längst hello hello experimentet (och välja hello **förutsägande webbtjänsten** alternativet).</span><span class="sxs-lookup"><span data-stu-id="dbe69-141">We could do this manually, but fortunately all three steps can be accomplished by clicking **Set Up Web Service** at hello bottom of hello experiment canvas (and selecting hello **Predictive Web Service** option).</span></span>

> [!TIP]
> <span data-ttu-id="dbe69-142">Om du vill ha mer information om vad som händer när du konverterar en utbildning experiment tooa förutsägande experiment, se [hur tooprepare modellen för distribution i Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="dbe69-142">If you want more details on what happens when you convert a training experiment tooa predictive experiment, see [How tooprepare your model for deployment in Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).</span></span>

<span data-ttu-id="dbe69-143">När du klickar på **konfigurera Web Service**, flera saker:</span><span class="sxs-lookup"><span data-stu-id="dbe69-143">When you click **Set Up Web Service**, several things happen:</span></span>

* <span data-ttu-id="dbe69-144">hello tränad modell är konverterade tooa enda **Trained Model** modulen och lagras i hello modulen paletten toohello till vänster i hello experimentera arbetsytan (du hittar den under **tränade modeller**)</span><span class="sxs-lookup"><span data-stu-id="dbe69-144">hello trained model is converted tooa single **Trained Model** module and stored in hello module palette toohello left of hello experiment canvas (you can find it under **Trained Models**)</span></span>
* <span data-ttu-id="dbe69-145">Moduler som användes för träning tas bort; specifikt:</span><span class="sxs-lookup"><span data-stu-id="dbe69-145">Modules that were used for training are removed; specifically:</span></span>
  * <span data-ttu-id="dbe69-146">[Two-Class Tvåklassförhöjda beslutsträdet][two-class-boosted-decision-tree]</span><span class="sxs-lookup"><span data-stu-id="dbe69-146">[Two-Class Boosted Decision Tree][two-class-boosted-decision-tree]</span></span>
  * <span data-ttu-id="dbe69-147">[Träningsmodell][train-model]</span><span class="sxs-lookup"><span data-stu-id="dbe69-147">[Train Model][train-model]</span></span>
  * <span data-ttu-id="dbe69-148">[Dela Data][split]</span><span class="sxs-lookup"><span data-stu-id="dbe69-148">[Split Data][split]</span></span>
  * <span data-ttu-id="dbe69-149">Hej andra [köra R-skriptet] [ execute-r-script] modul som användes för testdata</span><span class="sxs-lookup"><span data-stu-id="dbe69-149">hello second [Execute R Script][execute-r-script] module that was used for test data</span></span>
* <span data-ttu-id="dbe69-150">hello sparade tränade modellen har lagts till hello experiment</span><span class="sxs-lookup"><span data-stu-id="dbe69-150">hello saved trained model is added back into hello experiment</span></span>
* <span data-ttu-id="dbe69-151">**Web service indata** och **Web service utdata** moduler läggs (dessa identifiera där hello användardata kommer att ange hello modellen och vilka data som returneras när hello webbtjänsten används)</span><span class="sxs-lookup"><span data-stu-id="dbe69-151">**Web service input** and **Web service output** modules are added (these identify where hello user's data will enter hello model, and what data is returned, when hello web service is accessed)</span></span>

> [!NOTE]
> <span data-ttu-id="dbe69-152">Du kan se att hello experiment sparas i två delar under flikar som lagts till hello överst i hello experimentet.</span><span class="sxs-lookup"><span data-stu-id="dbe69-152">You can see that hello experiment is saved in two parts under tabs that have been added at hello top of hello experiment canvas.</span></span> <span data-ttu-id="dbe69-153">hello ursprungliga träningsexperiment är under hello fliken **träningsexperiment**, och hello nyskapad prediktivt experiment är **Prediktivt experiment**.</span><span class="sxs-lookup"><span data-stu-id="dbe69-153">hello original training experiment is under hello tab **Training experiment**, and hello newly created predictive experiment is under **Predictive experiment**.</span></span> <span data-ttu-id="dbe69-154">Hej prediktivt experiment är hello något kommer vi att distribuera som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="dbe69-154">hello predictive experiment is hello one we'll deploy as a web service.</span></span>

<span data-ttu-id="dbe69-155">Vi behöver tootake ytterligare ett steg med viss experimentet.</span><span class="sxs-lookup"><span data-stu-id="dbe69-155">We need tootake one additional step with this particular experiment.</span></span>
<span data-ttu-id="dbe69-156">Vi har lagt till två [köra R-skriptet] [ execute-r-script] moduler tooprovide en viktas fungera toohello data.</span><span class="sxs-lookup"><span data-stu-id="dbe69-156">We added two [Execute R Script][execute-r-script] modules tooprovide a weighting function toohello data.</span></span> <span data-ttu-id="dbe69-157">Som var bara en lura vi behövs för träning och testning, så att vi kan ta reda på dessa moduler i hello sista modellen.</span><span class="sxs-lookup"><span data-stu-id="dbe69-157">That was just a trick we needed for training and testing, so we can take out those modules in hello final model.</span></span>
<span data-ttu-id="dbe69-158">Machine Learning Studio bort en [köra R-skriptet] [ execute-r-script] modulen när den tas bort hello [dela] [ split] modul.</span><span class="sxs-lookup"><span data-stu-id="dbe69-158">Machine Learning Studio removed one [Execute R Script][execute-r-script] module when it removed hello [Split][split] module.</span></span> <span data-ttu-id="dbe69-159">Nu kan vi bort hello andra och ansluta [Metadata Editor] [ metadata-editor] direkt för[Poängmodell][score-model].</span><span class="sxs-lookup"><span data-stu-id="dbe69-159">Now we can remove hello other and connect [Metadata Editor][metadata-editor] directly too[Score Model][score-model].</span></span>    

<span data-ttu-id="dbe69-160">Vår experimentet bör nu se ut så här:</span><span class="sxs-lookup"><span data-stu-id="dbe69-160">Our experiment should now look like this:</span></span>  

![Bedömningsprofil hello tränade modellen][4]  

> [!NOTE]
> <span data-ttu-id="dbe69-162">Du kanske undrar varför vi kvar hello UCI tyska kreditkortdata dataset i hello prediktivt experiment.</span><span class="sxs-lookup"><span data-stu-id="dbe69-162">You may be wondering why we left hello UCI German Credit Card Data dataset in hello predictive experiment.</span></span> <span data-ttu-id="dbe69-163">hello tjänsten kommer tooscore hello användarens data, inte hello ursprungliga datauppsättningen, så varför lämnar hello ursprungliga datauppsättningen i hello modellen?</span><span class="sxs-lookup"><span data-stu-id="dbe69-163">hello service is going tooscore hello user's data, not hello original dataset, so why leave hello original dataset in hello model?</span></span>
> 
> <span data-ttu-id="dbe69-164">Det gäller att hello-tjänsten inte behöver hello ursprungliga kreditkortdata.</span><span class="sxs-lookup"><span data-stu-id="dbe69-164">It's true that hello service doesn't need hello original credit card data.</span></span> <span data-ttu-id="dbe69-165">Men den behöver hello schemat för dessa data, som innehåller information, till exempel hur många kolumner finns och vilka kolumner som är numeriska.</span><span class="sxs-lookup"><span data-stu-id="dbe69-165">But it does need hello schema for that data, which includes information such as how many columns there are and which columns are numeric.</span></span> <span data-ttu-id="dbe69-166">Den här schemainformationen är nödvändiga toointerpret hello användarens data.</span><span class="sxs-lookup"><span data-stu-id="dbe69-166">This schema information is necessary toointerpret hello user's data.</span></span> <span data-ttu-id="dbe69-167">Vi lämnar komponenterna ansluten så att hello poängsättningsmodul har hello dataset schema när hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="dbe69-167">We leave these components connected so that hello scoring module has hello dataset schema when hello service is running.</span></span> <span data-ttu-id="dbe69-168">hello data inte används bara hello schemat.</span><span class="sxs-lookup"><span data-stu-id="dbe69-168">hello data isn't used, just hello schema.</span></span>  
> 
> 

<span data-ttu-id="dbe69-169">Kör experimentet hello en sista gång (klicka på **kör**.) Om du vill tooverify som hello modellen fortfarande fungerar, klickar du på hello utdata från hello [Poängmodell] [ score-model] modulen och välj **visa resultat**.</span><span class="sxs-lookup"><span data-stu-id="dbe69-169">Run hello experiment one last time (click **Run**.) If you want tooverify that hello model is still working, click hello output of hello [Score Model][score-model] module and select **View Results**.</span></span> <span data-ttu-id="dbe69-170">Du kan se att hello ursprungliga data visas, tillsammans med hello kredit risk värdet (”poängsatta etiketter”) och hello bedömningen sannolikhetsvärdet (”bedömas sannolikhet”.)</span><span class="sxs-lookup"><span data-stu-id="dbe69-170">You can see that hello original data is displayed, along with hello credit risk value ("Scored Labels") and hello scoring probability value ("Scored Probabilities".)</span></span> 

## <a name="deploy-hello-web-service"></a><span data-ttu-id="dbe69-171">Distribuera hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="dbe69-171">Deploy hello web service</span></span>
<span data-ttu-id="dbe69-172">Du kan distribuera hello experiment som antingen en klassisk webbtjänst eller som en ny webbtjänst som baseras på Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="dbe69-172">You can deploy hello experiment as either a Classic web service, or as a New web service that's based on Azure Resource Manager.</span></span>

### <a name="deploy-as-a-classic-web-service"></a><span data-ttu-id="dbe69-173">Distribuera som ett klassiskt-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="dbe69-173">Deploy as a Classic web service</span></span>
<span data-ttu-id="dbe69-174">toodeploy som ett klassiskt webbtjänsten som härletts från våra experiment, klickar du på **distribuera webbtjänsten** nedan hello arbetsytan och välj **distribuera webbtjänsten [klassisk]**.</span><span class="sxs-lookup"><span data-stu-id="dbe69-174">toodeploy a Classic web service derived from our experiment, click **Deploy Web Service** below hello canvas and select **Deploy Web Service [Classic]**.</span></span> <span data-ttu-id="dbe69-175">Machine Learning Studio distribuerar hello experiment som en webbtjänst och tar toohello instrumentpanel för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="dbe69-175">Machine Learning Studio deploys hello experiment as a web service and takes you toohello dashboard for that web service.</span></span> <span data-ttu-id="dbe69-176">Från den här sidan kan du returnera toohello experiment (**visa ögonblicksbild** eller **Visa senaste**) och köra en enkel hello-webbtjänsten (se **testa hello webbtjänsten** nedan).</span><span class="sxs-lookup"><span data-stu-id="dbe69-176">From this page you can return toohello experiment (**View snapshot** or **View latest**) and run a simple test of hello web service (see **Test hello web service** below).</span></span> <span data-ttu-id="dbe69-177">Det finns även information här för att skapa program som kan komma åt hello-webbtjänst (Mer information om det finns i hello nästa steg i den här genomgången).</span><span class="sxs-lookup"><span data-stu-id="dbe69-177">There is also information here for creating applications that can access hello web service (more on that in hello next step of this walkthrough).</span></span>

![Web service-instrumentpanelen][6]

<span data-ttu-id="dbe69-179">Du kan konfigurera hello tjänsten genom att klicka på hello **CONFIGURATION** fliken. Här kan du ändra hello tjänstnamn (det är angivet hello experiment namn som standard) och en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="dbe69-179">You can configure hello service by clicking hello **CONFIGURATION** tab. Here you can modify hello service name (it's given hello experiment name by default) and give it a description.</span></span> <span data-ttu-id="dbe69-180">Du kan också ge mer egna etiketter för hello indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="dbe69-180">You can also give more friendly labels for hello input and output data.</span></span>  

![Konfigurera hello-webbtjänsten][5]  

### <a name="deploy-as-a-new-web-service"></a><span data-ttu-id="dbe69-182">Distribuera som en ny webbtjänst</span><span class="sxs-lookup"><span data-stu-id="dbe69-182">Deploy as a New web service</span></span>

> [!NOTE] 
> <span data-ttu-id="dbe69-183">toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="dbe69-183">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you are deploying hello web service.</span></span> <span data-ttu-id="dbe69-184">Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="dbe69-184">For more information, see [Manage a web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="dbe69-185">toodeploy en ny webbtjänst som härrör från våra experiment:</span><span class="sxs-lookup"><span data-stu-id="dbe69-185">toodeploy a New web service derived from our experiment:</span></span>

1. <span data-ttu-id="dbe69-186">Klicka på **distribuera webbtjänsten** nedan hello arbetsytan och välj **distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="dbe69-186">Click **Deploy Web Service** below hello canvas and select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="dbe69-187">Machine Learning Studio överför toohello Azure Machine Learning-webbtjänster **distribuera Experiment** sidan.</span><span class="sxs-lookup"><span data-stu-id="dbe69-187">Machine Learning Studio transfers you toohello Azure Machine Learning web services **Deploy Experiment** page.</span></span>

2. <span data-ttu-id="dbe69-188">Ange ett namn för hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="dbe69-188">Enter a name for hello web service.</span></span> 

3. <span data-ttu-id="dbe69-189">För **prisplan**, kan du välja en befintlig prisavtal eller välj ”Skapa nya” och ge hello nya planera ett namn och välj hello månatliga plan alternativet.</span><span class="sxs-lookup"><span data-stu-id="dbe69-189">For **Price Plan**, you can select an existing pricing plan, or select "Create new" and give hello new plan a name and select hello monthly plan option.</span></span> <span data-ttu-id="dbe69-190">hello plan nivåerna standard toohello planer för standardregion och webbtjänsten är distribuerade toothat region.</span><span class="sxs-lookup"><span data-stu-id="dbe69-190">hello plan tiers default toohello plans for your default region and your web service is deployed toothat region.</span></span>

4. <span data-ttu-id="dbe69-191">Klicka på **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="dbe69-191">Click **Deploy**.</span></span>

<span data-ttu-id="dbe69-192">Efter några minuter hello **Quickstart** öppnas sidan för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="dbe69-192">After a few minutes, hello **Quickstart** page for your web service opens.</span></span>

<span data-ttu-id="dbe69-193">Du kan konfigurera hello tjänsten genom att klicka på hello **konfigurera** fliken. Här kan du ändra hello service title och en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="dbe69-193">You can configure hello service by clicking hello **Configure** tab. Here you can modify hello service title and give it a description.</span></span> 

<span data-ttu-id="dbe69-194">tootest hello webbtjänsten klickar du på hello **Test** fliken (se **testa hello webbtjänsten** nedan).</span><span class="sxs-lookup"><span data-stu-id="dbe69-194">tootest hello web service, click hello **Test** tab (see **Test hello web service** below).</span></span> <span data-ttu-id="dbe69-195">Information om hur du skapar program som kan komma åt hello webbtjänsten klickar du på hello **förbruka** fliken (hello nästa steg i den här genomgången hamnar i detalj).</span><span class="sxs-lookup"><span data-stu-id="dbe69-195">For information on creating applications that can access hello web service, click hello **Consume** tab (hello next step in this walkthrough will go into more detail).</span></span>

> [!TIP]
> <span data-ttu-id="dbe69-196">När du har distribuerat den, kan du uppdatera hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="dbe69-196">You can update hello web service after you've deployed it.</span></span> <span data-ttu-id="dbe69-197">Till exempel om du vill toochange din modell kan sedan du kan redigera hello träningsexperiment, justera hello modellparametrarna och på **distribuera webbtjänsten**, välja **distribuera webbtjänsten [klassisk]** eller  **Distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="dbe69-197">For example, if you want toochange your model, then you can edit hello training experiment, tweak hello model parameters, and click **Deploy Web Service**, selecting **Deploy Web Service [Classic]** or **Deploy Web Service [New]**.</span></span> <span data-ttu-id="dbe69-198">När du distribuerar hello experiment, ersätter hello webbtjänsten nu och Använd uppdaterade modellen.</span><span class="sxs-lookup"><span data-stu-id="dbe69-198">When you deploy hello experiment again, it replaces hello web service, now using your updated model.</span></span>  
> 
> 

## <a name="test-hello-web-service"></a><span data-ttu-id="dbe69-199">Testa hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="dbe69-199">Test hello web service</span></span>

<span data-ttu-id="dbe69-200">När hello webbtjänsten används hello användardata anger via hello **Web service indata** modulen där det har gått toohello [Poängmodell] [ score-model] modulen och poängsätts.</span><span class="sxs-lookup"><span data-stu-id="dbe69-200">When hello web service is accessed, hello user's data enters through hello **Web service input** module where it's passed toohello [Score Model][score-model] module and scored.</span></span> <span data-ttu-id="dbe69-201">hello sätt som vi har konfigurerat hello prediktivt experiment, hello modellen förväntar data i hello samma format som hello ursprungliga kredit risk dataset.</span><span class="sxs-lookup"><span data-stu-id="dbe69-201">hello way we've set up hello predictive experiment, hello model expects data in hello same format as hello original credit risk dataset.</span></span>
<span data-ttu-id="dbe69-202">hello resultat returneras toohello användaren från hello webbtjänst via hello **Web service utdata** modul.</span><span class="sxs-lookup"><span data-stu-id="dbe69-202">hello results are returned toohello user from hello web service through hello **Web service output** module.</span></span>

> [!TIP]
> <span data-ttu-id="dbe69-203">hello sätt som vi har hello prediktivt experiment konfigurerad, hello hela fås hello [Poängmodell] [ score-model] modulen returneras.</span><span class="sxs-lookup"><span data-stu-id="dbe69-203">hello way we have hello predictive experiment configured, hello entire results from hello [Score Model][score-model] module are returned.</span></span> <span data-ttu-id="dbe69-204">Detta omfattar alla hello indata plus hello kredit risk värdet och hello bedömningen sannolikheten.</span><span class="sxs-lookup"><span data-stu-id="dbe69-204">This includes all hello input data plus hello credit risk value and hello scoring probability.</span></span> <span data-ttu-id="dbe69-205">Men returnerar något annat om du vill, t.ex, du kan returnera bara hello kredit risk värde.</span><span class="sxs-lookup"><span data-stu-id="dbe69-205">But you can return something different if you want - for example, you could return just hello credit risk value.</span></span> <span data-ttu-id="dbe69-206">toodo detta, infoga en [Projektkolumner] [ project-columns] modulen mellan [Poängmodell] [ score-model] och hello **Web service utdata** tooeliminate kolumner som du inte vill hello web service tooreturn.</span><span class="sxs-lookup"><span data-stu-id="dbe69-206">toodo this, insert a [Project Columns][project-columns] module between [Score Model][score-model] and hello **Web service output** tooeliminate columns you don't want hello web service tooreturn.</span></span> 
> 
> 

<span data-ttu-id="dbe69-207">Du kan testa en klassisk web service antingen i **Machine Learning Studio** eller i hello **Azure Machine Learning-webbtjänster** portal.</span><span class="sxs-lookup"><span data-stu-id="dbe69-207">You can test a Classic web service either in **Machine Learning Studio** or in hello **Azure Machine Learning Web Services** portal.</span></span>
<span data-ttu-id="dbe69-208">Du kan testa en ny webbtjänst endast i hello **Machine Learning-webbtjänster** portal.</span><span class="sxs-lookup"><span data-stu-id="dbe69-208">You can test a New web service only in hello **Machine Learning Web Services** portal.</span></span>

> [!TIP]
> <span data-ttu-id="dbe69-209">Testa i hello Azure Machine Learning-webbtjänster portal, kan du ha hello portal Skapa exempeldata som du kan använda tootest hello svar på begäranden tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dbe69-209">When testing in hello Azure Machine Learning Web Services portal, you can have hello portal create sample data that you can use tootest hello Request-Response service.</span></span> <span data-ttu-id="dbe69-210">På hello **konfigurera** väljer ”Ja” för **exempel Data aktiverat?**.</span><span class="sxs-lookup"><span data-stu-id="dbe69-210">On hello **Configure** page, select "Yes" for **Sample Data Enabled?**.</span></span> <span data-ttu-id="dbe69-211">När du öppnar fliken hello begäran och svar på hello **Test** sidan hello portal fylls exempeldata från hello ursprungliga kredit risk dataset.</span><span class="sxs-lookup"><span data-stu-id="dbe69-211">When you open hello Request-Response tab on hello **Test** page, hello portal fills in sample data taken from hello original credit risk dataset.</span></span>

### <a name="test-a-classic-web-service"></a><span data-ttu-id="dbe69-212">Testa en klassisk-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="dbe69-212">Test a Classic web service</span></span>

<span data-ttu-id="dbe69-213">Du kan testa en klassisk webbtjänst i Machine Learning Studio eller hello Machine Learning-webbtjänster portal.</span><span class="sxs-lookup"><span data-stu-id="dbe69-213">You can test a Classic web service in Machine Learning Studio or in hello Machine Learning Web Services portal.</span></span> 

#### <a name="test-in-machine-learning-studio"></a><span data-ttu-id="dbe69-214">Testa i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="dbe69-214">Test in Machine Learning Studio</span></span>

1. <span data-ttu-id="dbe69-215">På hello **INSTRUMENTPANELEN** för hello-webbtjänsten, klickar du på hello **Test** knappen **standard Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="dbe69-215">On hello **DASHBOARD** page for hello web service, click hello **Test** button under **Default Endpoint**.</span></span> <span data-ttu-id="dbe69-216">En dialogruta visas och du uppmanas att ange hello indata för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dbe69-216">A dialog pops up and asks you for hello input data for hello service.</span></span> <span data-ttu-id="dbe69-217">Dessa är hello samma kolumner som fanns i hello ursprungliga kredit risk dataset.</span><span class="sxs-lookup"><span data-stu-id="dbe69-217">These are hello same columns that appeared in hello original credit risk dataset.</span></span>  

2. <span data-ttu-id="dbe69-218">Ange en uppsättning data och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="dbe69-218">Enter a set of data and then click **OK**.</span></span> 

#### <a name="test-in-hello-machine-learning-web-services-portal"></a><span data-ttu-id="dbe69-219">Testa i hello Machine Learning-webbtjänster portal</span><span class="sxs-lookup"><span data-stu-id="dbe69-219">Test in hello Machine Learning Web Services portal</span></span>

1. <span data-ttu-id="dbe69-220">På hello **INSTRUMENTPANELEN** för hello-webbtjänsten, klickar du på hello **Test preview** länken under **standard Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="dbe69-220">On hello **DASHBOARD** page for hello web service, click hello **Test preview** link under **Default Endpoint**.</span></span> <span data-ttu-id="dbe69-221">hello testsida hello Azure Machine Learning-webbtjänster för i portalen hello webbtjänstslutpunkt öppnas och du uppmanas att ange hello indata för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dbe69-221">hello test page in hello Azure Machine Learning Web Services portal for hello web service endpoint opens and asks you for hello input data for hello service.</span></span> <span data-ttu-id="dbe69-222">Dessa är hello samma kolumner som fanns i hello ursprungliga kredit risk dataset.</span><span class="sxs-lookup"><span data-stu-id="dbe69-222">These are hello same columns that appeared in hello original credit risk dataset.</span></span>

2. <span data-ttu-id="dbe69-223">Klicka på **testa begäran och svar**.</span><span class="sxs-lookup"><span data-stu-id="dbe69-223">Click **Test Request-Response**.</span></span> 

### <a name="test-a-new-web-service"></a><span data-ttu-id="dbe69-224">Testa en ny webbtjänst</span><span class="sxs-lookup"><span data-stu-id="dbe69-224">Test a New web service</span></span>

<span data-ttu-id="dbe69-225">Du kan testa en ny webbtjänst endast i hello Machine Learning-webbtjänster portal.</span><span class="sxs-lookup"><span data-stu-id="dbe69-225">You can test a New web service only in hello Machine Learning Web Services portal.</span></span>

1. <span data-ttu-id="dbe69-226">I hello [Azure Machine Learning-webbtjänster](https://services.azureml.net/quickstart) klickar du på **Test** hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="dbe69-226">In hello [Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal, click **Test** at hello top of hello page.</span></span> <span data-ttu-id="dbe69-227">Hej **Test** sidan öppnas och du kan ange data för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dbe69-227">hello **Test** page opens and you can input data for hello service.</span></span> <span data-ttu-id="dbe69-228">hello inmatningsfält visas överensstämmer toohello kolumner som fanns i hello ursprungliga kredit risk dataset.</span><span class="sxs-lookup"><span data-stu-id="dbe69-228">hello input fields displayed correspond toohello columns that appeared in hello original credit risk dataset.</span></span> 

2. <span data-ttu-id="dbe69-229">Ange en uppsättning data och klicka sedan på **Test-svar på begäranden**.</span><span class="sxs-lookup"><span data-stu-id="dbe69-229">Enter a set of data and then click **Test Request-Response**.</span></span>

<span data-ttu-id="dbe69-230">hello visas av hello test resultaten på hello höger hello-sidan i hello utdatakolumnen.</span><span class="sxs-lookup"><span data-stu-id="dbe69-230">hello results of hello test are displayed on hello right-hand side of hello page in hello output column.</span></span> 


## <a name="manage-hello-web-service"></a><span data-ttu-id="dbe69-231">Hantera hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="dbe69-231">Manage hello web service</span></span>

### <a name="manage-a-classic-web-service-in-hello-azure-classic-portal"></a><span data-ttu-id="dbe69-232">Hantera en klassisk webbtjänst i hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dbe69-232">Manage a Classic web service in hello Azure classic portal</span></span>

<span data-ttu-id="dbe69-233">När du har distribuerat klassisk webbtjänsten kan du hantera den från hello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="dbe69-233">Once you've deployed your Classic web service, you can manage it from hello [Azure classic portal](https://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="dbe69-234">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="dbe69-234">Sign in toohello [Azure classic portal](https://manage.windowsazure.com)</span></span>
2. <span data-ttu-id="dbe69-235">Klicka på hello Microsoft Azure-tjänster Kontrollpanelen **MACHINE LEARNING**</span><span class="sxs-lookup"><span data-stu-id="dbe69-235">In hello Microsoft Azure services panel, click **MACHINE LEARNING**</span></span>
3. <span data-ttu-id="dbe69-236">Klicka på arbetsytan</span><span class="sxs-lookup"><span data-stu-id="dbe69-236">Click your workspace</span></span>
4. <span data-ttu-id="dbe69-237">Klicka på hello **webbtjänster** fliken</span><span class="sxs-lookup"><span data-stu-id="dbe69-237">Click hello **Web services** tab</span></span>
5. <span data-ttu-id="dbe69-238">Klicka på hello-webbtjänst som vi skapade</span><span class="sxs-lookup"><span data-stu-id="dbe69-238">Click hello web service we created</span></span>
6. <span data-ttu-id="dbe69-239">Klicka på hello ”default” slutpunkt</span><span class="sxs-lookup"><span data-stu-id="dbe69-239">Click hello "default" endpoint</span></span>

<span data-ttu-id="dbe69-240">Här kan du till exempel övervaka hur hello webbtjänst fungerar och göra justeringar prestanda genom att ändra hur många samtidiga anrop hello-tjänsten kan hantera.</span><span class="sxs-lookup"><span data-stu-id="dbe69-240">From here, you can do things like monitor how hello web service is doing and make performance tweaks by changing how many concurrent calls hello service can handle.</span></span>

<span data-ttu-id="dbe69-241">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="dbe69-241">For more details, see:</span></span>

* [<span data-ttu-id="dbe69-242">Skapa slutpunkter</span><span class="sxs-lookup"><span data-stu-id="dbe69-242">Creating Endpoints</span></span>](machine-learning-create-endpoint.md)
* [<span data-ttu-id="dbe69-243">Skalning webbtjänst</span><span class="sxs-lookup"><span data-stu-id="dbe69-243">Scaling web service</span></span>](machine-learning-scaling-webservice.md)

### <a name="manage-a-classic-or-new-web-service-in-hello-azure-machine-learning-web-services-portal"></a><span data-ttu-id="dbe69-244">Hantera en klassisk eller ny webbtjänst i hello Azure Machine Learning-webbtjänster portal</span><span class="sxs-lookup"><span data-stu-id="dbe69-244">Manage a Classic or New web service in hello Azure Machine Learning Web Services portal</span></span>

<span data-ttu-id="dbe69-245">När du har distribuerat din webbtjänst om klassiska eller nya, du kan hantera den från hello [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/quickstart) portal.</span><span class="sxs-lookup"><span data-stu-id="dbe69-245">Once you've deployed your web service, whether Classic or New, you can manage it from hello [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal.</span></span>

<span data-ttu-id="dbe69-246">toomonitor hello prestanda för webbtjänsten:</span><span class="sxs-lookup"><span data-stu-id="dbe69-246">toomonitor hello performance of your web service:</span></span>

1. <span data-ttu-id="dbe69-247">Logga in toohello [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/quickstart) portal</span><span class="sxs-lookup"><span data-stu-id="dbe69-247">Sign in toohello [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal</span></span>
2. <span data-ttu-id="dbe69-248">Klicka på **webbtjänster**</span><span class="sxs-lookup"><span data-stu-id="dbe69-248">Click **Web services**</span></span>
3. <span data-ttu-id="dbe69-249">Klicka på din web service</span><span class="sxs-lookup"><span data-stu-id="dbe69-249">Click your web service</span></span>
4. <span data-ttu-id="dbe69-250">Klicka på hello **instrumentpanelen**</span><span class="sxs-lookup"><span data-stu-id="dbe69-250">Click hello **Dashboard**</span></span>

- - -
<span data-ttu-id="dbe69-251">**Nästa: [komma åt hello-webbtjänst](machine-learning-walkthrough-6-access-web-service.md)**</span><span class="sxs-lookup"><span data-stu-id="dbe69-251">**Next: [Access hello web service](machine-learning-walkthrough-6-access-web-service.md)**</span></span>

[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[3a]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3a.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
