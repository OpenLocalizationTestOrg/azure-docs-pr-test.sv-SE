---
title: "Steg 5: Distribuera webbtjänsten för Machine Learning | Microsoft Docs"
description: "Steg 5 i utveckla en förutsägelselösning genomgång: distribuera en prediktivt experiment i Machine Learning Studio som en webbtjänst."
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
ms.openlocfilehash: cec1bcceea158a20742c7019a266dcefaac4c9cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a><span data-ttu-id="ef285-103">Genomgång steg 5: Distribuera Azure Machine Learning-webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="ef285-103">Walkthrough Step 5: Deploy the Azure Machine Learning web service</span></span>
<span data-ttu-id="ef285-104">Det här är det femte steget i den här genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="ef285-104">This is the fifth step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="ef285-105">Skapa en Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="ef285-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="ef285-106">Överför befintliga data</span><span class="sxs-lookup"><span data-stu-id="ef285-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="ef285-107">Skapa ett nytt experiment</span><span class="sxs-lookup"><span data-stu-id="ef285-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="ef285-108">Träna och utvärdera modellerna</span><span class="sxs-lookup"><span data-stu-id="ef285-108">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. <span data-ttu-id="ef285-109">**Distribuera webbtjänsten**</span><span class="sxs-lookup"><span data-stu-id="ef285-109">**Deploy the web service**</span></span>
6. [<span data-ttu-id="ef285-110">Få åtkomst till webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="ef285-110">Access the web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="ef285-111">Om du vill ge andra användare möjlighet att använda förutsägelsemodell som vi har utvecklat i den här genomgången, kan vi distribuera den som en webbtjänst på Azure.</span><span class="sxs-lookup"><span data-stu-id="ef285-111">To give others a chance to use the predictive model we've developed in this walkthrough, we can deploy it as a web service on Azure.</span></span>

<span data-ttu-id="ef285-112">I det här läget har vi experimentera med vår modell.</span><span class="sxs-lookup"><span data-stu-id="ef285-112">Up to this point we've been experimenting with training our model.</span></span> <span data-ttu-id="ef285-113">Men den distribuerade tjänsten inte längre kommer att göra utbildning - kommer att generera nya förutsägelser av bedömningen användarindata baserat på vår modell.</span><span class="sxs-lookup"><span data-stu-id="ef285-113">But the deployed service is no longer going to do training - it's going to generate new predictions by scoring the user's input based on our model.</span></span> <span data-ttu-id="ef285-114">Så är det dags att göra vissa förberedelser för att konvertera experimentet från en ***utbildning*** experimentera till en ***förutsägande*** experiment.</span><span class="sxs-lookup"><span data-stu-id="ef285-114">So we're going to do some preparation to convert this experiment from a ***training*** experiment to a ***predictive*** experiment.</span></span> 

<span data-ttu-id="ef285-115">Detta är en process i tre steg:</span><span class="sxs-lookup"><span data-stu-id="ef285-115">This is a three-step process:</span></span>  

1. <span data-ttu-id="ef285-116">Ta bort en av modeller</span><span class="sxs-lookup"><span data-stu-id="ef285-116">Remove one of the models</span></span>
2. <span data-ttu-id="ef285-117">Omvandla den *träningsexperiment* har vi skapat till en *prediktivt experiment*</span><span class="sxs-lookup"><span data-stu-id="ef285-117">Convert the *training experiment* we've created into a *predictive experiment*</span></span>
3. <span data-ttu-id="ef285-118">Distribuera prediktivt experiment som en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="ef285-118">Deploy the predictive experiment as a web service</span></span>

## <a name="remove-one-of-the-models"></a><span data-ttu-id="ef285-119">Ta bort en av modeller</span><span class="sxs-lookup"><span data-stu-id="ef285-119">Remove one of the models</span></span>

<span data-ttu-id="ef285-120">Vi måste först trim experimentet ned lite.</span><span class="sxs-lookup"><span data-stu-id="ef285-120">First, we need to trim this experiment down a little.</span></span> <span data-ttu-id="ef285-121">Vi har två olika modeller i försöket, men vi vill använda en modell när vi distribuera den som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="ef285-121">We currently have two different models in the experiment, but we only want to use one model when we deploy this as a web service.</span></span>  

<span data-ttu-id="ef285-122">Anta att vi har valt att ökat trädet modellen presterade bättre än SVM modellen.</span><span class="sxs-lookup"><span data-stu-id="ef285-122">Let's say we've decided that the boosted tree model performed better than the SVM model.</span></span> <span data-ttu-id="ef285-123">Så att det första du gör är att ta bort den [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulen och moduler som användes för att träna den.</span><span class="sxs-lookup"><span data-stu-id="ef285-123">So the first thing to do is remove the [Two-Class Support Vector Machine][two-class-support-vector-machine] module and the modules that were used for training it.</span></span> <span data-ttu-id="ef285-124">Du kanske vill göra en kopia av experimentet först genom att klicka på **Spara som** längst ned i arbetsytan för experimentet.</span><span class="sxs-lookup"><span data-stu-id="ef285-124">You may want to make a copy of the experiment first by clicking **Save As** at the bottom of the experiment canvas.</span></span>

<span data-ttu-id="ef285-125">Vi måste du ta bort följande moduler:</span><span class="sxs-lookup"><span data-stu-id="ef285-125">We need to delete the following modules:</span></span>  

* <span data-ttu-id="ef285-126">[Two-Class Support Vector Machine][two-class-support-vector-machine]</span><span class="sxs-lookup"><span data-stu-id="ef285-126">[Two-Class Support Vector Machine][two-class-support-vector-machine]</span></span>
* <span data-ttu-id="ef285-127">[Träna modellen] [ train-model] och [Poängmodell] [ score-model] moduler som är anslutna till den</span><span class="sxs-lookup"><span data-stu-id="ef285-127">[Train Model][train-model] and [Score Model][score-model] modules that were connected to it</span></span>
* <span data-ttu-id="ef285-128">[Normalisera Data] [ normalize-data] (båda)</span><span class="sxs-lookup"><span data-stu-id="ef285-128">[Normalize Data][normalize-data] (both of them)</span></span>
* <span data-ttu-id="ef285-129">[Utvärdera modellen] [ evaluate-model] (eftersom vi är klar med att utvärdera modellerna)</span><span class="sxs-lookup"><span data-stu-id="ef285-129">[Evaluate Model][evaluate-model] (because we're finished evaluating the models)</span></span>

<span data-ttu-id="ef285-130">Markera varje modul och tryck på DELETE eller högerklicka på modulen och välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="ef285-130">Select each module and press the Delete key, or right-click the module and select **Delete**.</span></span> 

![Ta bort SVM modellen][3a]

<span data-ttu-id="ef285-132">Vår modell bör nu se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="ef285-132">Our model should now look something like this:</span></span>

![Ta bort SVM modellen][3]

<span data-ttu-id="ef285-134">Nu vi är redo att distribuera den här modellen med hjälp av den [två-Tvåklassförhöjt beslutsträd][two-class-boosted-decision-tree].</span><span class="sxs-lookup"><span data-stu-id="ef285-134">Now we're ready to deploy this model using the [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree].</span></span>

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a><span data-ttu-id="ef285-135">Omvandla utbildning experimentet till prediktivt experiment</span><span class="sxs-lookup"><span data-stu-id="ef285-135">Convert the training experiment to a predictive experiment</span></span>

<span data-ttu-id="ef285-136">För att förbereda den här modellen för distribution, måste vi du konvertera den här träningsexperiment till prediktivt experiment.</span><span class="sxs-lookup"><span data-stu-id="ef285-136">To get this model ready for deployment, we need to convert this training experiment to a predictive experiment.</span></span> <span data-ttu-id="ef285-137">Detta omfattar tre steg:</span><span class="sxs-lookup"><span data-stu-id="ef285-137">This involves three steps:</span></span>

1. <span data-ttu-id="ef285-138">Spara modellen vi har tränat och Ersätt vår utbildningsmoduler</span><span class="sxs-lookup"><span data-stu-id="ef285-138">Save the model we've trained and then replace our training modules</span></span>
2. <span data-ttu-id="ef285-139">Trim experiment om du vill ta bort moduler som behövdes endast för utbildning</span><span class="sxs-lookup"><span data-stu-id="ef285-139">Trim the experiment to remove modules that were only needed for training</span></span>
3. <span data-ttu-id="ef285-140">Definiera där webbtjänsten ska ta emot indata och där det genererar utdata</span><span class="sxs-lookup"><span data-stu-id="ef285-140">Define where the web service will accept input and where it generates the output</span></span>

<span data-ttu-id="ef285-141">Vi kan göra detta manuellt, men som tur är alla tre stegen kan utföras genom att klicka på **konfigurera Web Service** längst ned i arbetsytan för experimentet (och välja den **förutsägande webbtjänsten** alternativet).</span><span class="sxs-lookup"><span data-stu-id="ef285-141">We could do this manually, but fortunately all three steps can be accomplished by clicking **Set Up Web Service** at the bottom of the experiment canvas (and selecting the **Predictive Web Service** option).</span></span>

> [!TIP]
> <span data-ttu-id="ef285-142">Om du vill ha mer information om vad som händer när du konverterar en träningsexperiment till en prediktivt experiment, se [hur du förbereder din modell för distribution i Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="ef285-142">If you want more details on what happens when you convert a training experiment to a predictive experiment, see [How to prepare your model for deployment in Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).</span></span>

<span data-ttu-id="ef285-143">När du klickar på **konfigurera Web Service**, flera saker:</span><span class="sxs-lookup"><span data-stu-id="ef285-143">When you click **Set Up Web Service**, several things happen:</span></span>

* <span data-ttu-id="ef285-144">Den tränade modellen konverteras till en enda **Trained Model** modulen och lagras på modulpaletten till vänster om arbetsytan för experimentet (du hittar den under **tränade modeller**)</span><span class="sxs-lookup"><span data-stu-id="ef285-144">The trained model is converted to a single **Trained Model** module and stored in the module palette to the left of the experiment canvas (you can find it under **Trained Models**)</span></span>
* <span data-ttu-id="ef285-145">Moduler som användes för träning tas bort; specifikt:</span><span class="sxs-lookup"><span data-stu-id="ef285-145">Modules that were used for training are removed; specifically:</span></span>
  * <span data-ttu-id="ef285-146">[Two-Class Tvåklassförhöjda beslutsträdet][two-class-boosted-decision-tree]</span><span class="sxs-lookup"><span data-stu-id="ef285-146">[Two-Class Boosted Decision Tree][two-class-boosted-decision-tree]</span></span>
  * <span data-ttu-id="ef285-147">[Träningsmodell][train-model]</span><span class="sxs-lookup"><span data-stu-id="ef285-147">[Train Model][train-model]</span></span>
  * <span data-ttu-id="ef285-148">[Dela Data][split]</span><span class="sxs-lookup"><span data-stu-id="ef285-148">[Split Data][split]</span></span>
  * <span data-ttu-id="ef285-149">andra [köra R-skriptet] [ execute-r-script] modul som användes för testdata</span><span class="sxs-lookup"><span data-stu-id="ef285-149">the second [Execute R Script][execute-r-script] module that was used for test data</span></span>
* <span data-ttu-id="ef285-150">Den sparade tränade modellen har lagts till i experimentet igen</span><span class="sxs-lookup"><span data-stu-id="ef285-150">The saved trained model is added back into the experiment</span></span>
* <span data-ttu-id="ef285-151">**Web service indata** och **Web service utdata** moduler läggs (dessa identifiera där användarens data kommer att ange modellen och vilka data som returneras när webbtjänsten används)</span><span class="sxs-lookup"><span data-stu-id="ef285-151">**Web service input** and **Web service output** modules are added (these identify where the user's data will enter the model, and what data is returned, when the web service is accessed)</span></span>

> [!NOTE]
> <span data-ttu-id="ef285-152">Du kan se att experimentet sparas i två delar under flikar som lagts till överst på arbetsytan för experimentet.</span><span class="sxs-lookup"><span data-stu-id="ef285-152">You can see that the experiment is saved in two parts under tabs that have been added at the top of the experiment canvas.</span></span> <span data-ttu-id="ef285-153">Den ursprungliga träningsexperiment är under fliken **träningsexperiment**, och det nyligen skapade prediktivt experimentet är **Prediktivt experiment**.</span><span class="sxs-lookup"><span data-stu-id="ef285-153">The original training experiment is under the tab **Training experiment**, and the newly created predictive experiment is under **Predictive experiment**.</span></span> <span data-ttu-id="ef285-154">Prediktivt experiment är det som vi ska distribuera som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="ef285-154">The predictive experiment is the one we'll deploy as a web service.</span></span>

<span data-ttu-id="ef285-155">Vi behöver vidta ytterligare ett steg med viss experimentet.</span><span class="sxs-lookup"><span data-stu-id="ef285-155">We need to take one additional step with this particular experiment.</span></span>
<span data-ttu-id="ef285-156">Vi har lagt till två [köra R-skriptet] [ execute-r-script] moduler för att tillhandahålla en viktas funktion för data.</span><span class="sxs-lookup"><span data-stu-id="ef285-156">We added two [Execute R Script][execute-r-script] modules to provide a weighting function to the data.</span></span> <span data-ttu-id="ef285-157">Som var bara en lura vi behövs för träning och testning, så att vi kan ta reda på dessa moduler i den slutliga modellen.</span><span class="sxs-lookup"><span data-stu-id="ef285-157">That was just a trick we needed for training and testing, so we can take out those modules in the final model.</span></span>
<span data-ttu-id="ef285-158">Machine Learning Studio bort en [köra R-skriptet] [ execute-r-script] modulen när den tas bort i [dela] [ split] modul.</span><span class="sxs-lookup"><span data-stu-id="ef285-158">Machine Learning Studio removed one [Execute R Script][execute-r-script] module when it removed the [Split][split] module.</span></span> <span data-ttu-id="ef285-159">Nu när vi kan ta bort det andra och ansluta [Metadata Editor] [ metadata-editor] direkt till [Poängmodell][score-model].</span><span class="sxs-lookup"><span data-stu-id="ef285-159">Now we can remove the other and connect [Metadata Editor][metadata-editor] directly to [Score Model][score-model].</span></span>    

<span data-ttu-id="ef285-160">Vår experimentet bör nu se ut så här:</span><span class="sxs-lookup"><span data-stu-id="ef285-160">Our experiment should now look like this:</span></span>  

![Poängsättning av den tränade modellen][4]  

> [!NOTE]
> <span data-ttu-id="ef285-162">Du kanske undrar varför vi kvar UCI tyska kreditkortdata datauppsättningen i prediktivt experiment.</span><span class="sxs-lookup"><span data-stu-id="ef285-162">You may be wondering why we left the UCI German Credit Card Data dataset in the predictive experiment.</span></span> <span data-ttu-id="ef285-163">Tjänsten kommer att poängsätta användarens data, inte den ursprungliga datauppsättningen, så varför lämnar den ursprungliga datauppsättningen i modellen?</span><span class="sxs-lookup"><span data-stu-id="ef285-163">The service is going to score the user's data, not the original dataset, so why leave the original dataset in the model?</span></span>
> 
> <span data-ttu-id="ef285-164">Det gäller att tjänsten inte behöver den ursprungliga informationen för kreditkort.</span><span class="sxs-lookup"><span data-stu-id="ef285-164">It's true that the service doesn't need the original credit card data.</span></span> <span data-ttu-id="ef285-165">Men den behöver schemat för dessa data, som innehåller information, till exempel hur många kolumner finns och vilka kolumner som är numeriska.</span><span class="sxs-lookup"><span data-stu-id="ef285-165">But it does need the schema for that data, which includes information such as how many columns there are and which columns are numeric.</span></span> <span data-ttu-id="ef285-166">Den här schemainformation är nödvändigt att tolka användarens data.</span><span class="sxs-lookup"><span data-stu-id="ef285-166">This schema information is necessary to interpret the user's data.</span></span> <span data-ttu-id="ef285-167">Vi lämnar komponenterna ansluten så att poängsättningsmodul har dataset-schema när tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="ef285-167">We leave these components connected so that the scoring module has the dataset schema when the service is running.</span></span> <span data-ttu-id="ef285-168">Informationen används inte bara schemat.</span><span class="sxs-lookup"><span data-stu-id="ef285-168">The data isn't used, just the schema.</span></span>  
> 
> 

<span data-ttu-id="ef285-169">Kör experimentet en sista gång (klicka på **kör**.) Om du vill kontrollera att modellen fortfarande fungerar, klickar du på utdata från den [Poängmodell] [ score-model] modulen och välj **visa resultat**.</span><span class="sxs-lookup"><span data-stu-id="ef285-169">Run the experiment one last time (click **Run**.) If you want to verify that the model is still working, click the output of the [Score Model][score-model] module and select **View Results**.</span></span> <span data-ttu-id="ef285-170">Du kan se att ursprungliga data visas, tillsammans med kredit risk värdet (”poängsatta etiketter”) och bedömningsprofil sannolikhetsvärdet (”bedömas sannolikhet”.)</span><span class="sxs-lookup"><span data-stu-id="ef285-170">You can see that the original data is displayed, along with the credit risk value ("Scored Labels") and the scoring probability value ("Scored Probabilities".)</span></span> 

## <a name="deploy-the-web-service"></a><span data-ttu-id="ef285-171">Distribuera webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="ef285-171">Deploy the web service</span></span>
<span data-ttu-id="ef285-172">Du kan distribuera experimentet som antingen en klassisk webbtjänst eller som en ny webbtjänst som baseras på Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ef285-172">You can deploy the experiment as either a Classic web service, or as a New web service that's based on Azure Resource Manager.</span></span>

### <a name="deploy-as-a-classic-web-service"></a><span data-ttu-id="ef285-173">Distribuera som ett klassiskt-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="ef285-173">Deploy as a Classic web service</span></span>
<span data-ttu-id="ef285-174">Om du vill distribuera en klassisk webbtjänst som härletts från våra experiment, klickar du på **distribuera webbtjänsten** under arbetsytan och välj **distribuera webbtjänsten [klassisk]**.</span><span class="sxs-lookup"><span data-stu-id="ef285-174">To deploy a Classic web service derived from our experiment, click **Deploy Web Service** below the canvas and select **Deploy Web Service [Classic]**.</span></span> <span data-ttu-id="ef285-175">Machine Learning Studio distribuerar experiment som en webbtjänst och går till instrumentpanelen för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ef285-175">Machine Learning Studio deploys the experiment as a web service and takes you to the dashboard for that web service.</span></span> <span data-ttu-id="ef285-176">Från den här sidan kan du återgå till experimentet (**visa ögonblicksbild** eller **Visa senaste**) och kör ett enkelt test av webbtjänsten (se **testa webbtjänsten** nedan).</span><span class="sxs-lookup"><span data-stu-id="ef285-176">From this page you can return to the experiment (**View snapshot** or **View latest**) and run a simple test of the web service (see **Test the web service** below).</span></span> <span data-ttu-id="ef285-177">Det finns även information här för att skapa program som har åtkomst till webbtjänsten (Mer information om det finns i nästa steg i den här genomgången).</span><span class="sxs-lookup"><span data-stu-id="ef285-177">There is also information here for creating applications that can access the web service (more on that in the next step of this walkthrough).</span></span>

![Web service-instrumentpanelen][6]

<span data-ttu-id="ef285-179">Du kan konfigurera tjänsten genom att klicka på den **CONFIGURATION** fliken.</span><span class="sxs-lookup"><span data-stu-id="ef285-179">You can configure the service by clicking the **CONFIGURATION** tab.</span></span> <span data-ttu-id="ef285-180">Här kan du ändra tjänstens namn (den har fått namnet experiment som standard) och en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="ef285-180">Here you can modify the service name (it's given the experiment name by default) and give it a description.</span></span> <span data-ttu-id="ef285-181">Du kan också ge mer egna etiketter för inkommande och utgående data.</span><span class="sxs-lookup"><span data-stu-id="ef285-181">You can also give more friendly labels for the input and output data.</span></span>  

![Konfigurera webbtjänsten][5]  

### <a name="deploy-as-a-new-web-service"></a><span data-ttu-id="ef285-183">Distribuera som en ny webbtjänst</span><span class="sxs-lookup"><span data-stu-id="ef285-183">Deploy as a New web service</span></span>

> [!NOTE] 
> <span data-ttu-id="ef285-184">Du måste ha tillräckliga behörigheter i prenumerationen som du distribuerar webbtjänsten för att distribuera en ny webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="ef285-184">To deploy a New web service you must have sufficient permissions in the subscription to which you are deploying the web service.</span></span> <span data-ttu-id="ef285-185">Mer information finns i [hantera en webbtjänst med hjälp av Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="ef285-185">For more information, see [Manage a web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="ef285-186">Att distribuera en ny webbtjänst som härrör från våra experiment:</span><span class="sxs-lookup"><span data-stu-id="ef285-186">To deploy a New web service derived from our experiment:</span></span>

1. <span data-ttu-id="ef285-187">Klicka på **distribuera webbtjänsten** under arbetsytan och välj **distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="ef285-187">Click **Deploy Web Service** below the canvas and select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="ef285-188">Machine Learning Studio överför du till Azure Machine Learning-webbtjänster **distribuera Experiment** sidan.</span><span class="sxs-lookup"><span data-stu-id="ef285-188">Machine Learning Studio transfers you to the Azure Machine Learning web services **Deploy Experiment** page.</span></span>

2. <span data-ttu-id="ef285-189">Ange ett namn för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ef285-189">Enter a name for the web service.</span></span> 

3. <span data-ttu-id="ef285-190">För **prisplan**, du kan välja en befintlig prisavtal eller välj ”Skapa nya” och namnge den nya planen och väljer alternativet månatliga plan.</span><span class="sxs-lookup"><span data-stu-id="ef285-190">For **Price Plan**, you can select an existing pricing plan, or select "Create new" and give the new plan a name and select the monthly plan option.</span></span> <span data-ttu-id="ef285-191">Planera nivåerna standard att planer för standardregion och webbtjänsten har distribuerats till den regionen.</span><span class="sxs-lookup"><span data-stu-id="ef285-191">The plan tiers default to the plans for your default region and your web service is deployed to that region.</span></span>

4. <span data-ttu-id="ef285-192">Klicka på **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="ef285-192">Click **Deploy**.</span></span>

<span data-ttu-id="ef285-193">Efter några minuter på **Quickstart** öppnas sidan för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ef285-193">After a few minutes, the **Quickstart** page for your web service opens.</span></span>

<span data-ttu-id="ef285-194">Du kan konfigurera tjänsten genom att klicka på den **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="ef285-194">You can configure the service by clicking the **Configure** tab.</span></span> <span data-ttu-id="ef285-195">Här kan du ändra tjänsten title och en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="ef285-195">Here you can modify the service title and give it a description.</span></span> 

<span data-ttu-id="ef285-196">Om du vill testa webbtjänsten klickar du på den **testa** fliken (se **testa webbtjänsten** nedan).</span><span class="sxs-lookup"><span data-stu-id="ef285-196">To test the web service, click the **Test** tab (see **Test the web service** below).</span></span> <span data-ttu-id="ef285-197">Mer information om hur du skapar program som har åtkomst till webbtjänsten klickar du på **förbruka** fliken (nästa steg i den här genomgången hamnar i detalj).</span><span class="sxs-lookup"><span data-stu-id="ef285-197">For information on creating applications that can access the web service, click the **Consume** tab (the next step in this walkthrough will go into more detail).</span></span>

> [!TIP]
> <span data-ttu-id="ef285-198">När du har distribuerat den, kan du uppdatera webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ef285-198">You can update the web service after you've deployed it.</span></span> <span data-ttu-id="ef285-199">Till exempel om du vill ändra modellen, och sedan kan du redigera utbildning experiment, justera modellparametrarna och på **distribuera webbtjänsten**, välja **distribuera webbtjänsten [klassisk]** eller **distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="ef285-199">For example, if you want to change your model, then you can edit the training experiment, tweak the model parameters, and click **Deploy Web Service**, selecting **Deploy Web Service [Classic]** or **Deploy Web Service [New]**.</span></span> <span data-ttu-id="ef285-200">När du distribuerar experimentet igen ersätter webbtjänsten nu och Använd uppdaterade modellen.</span><span class="sxs-lookup"><span data-stu-id="ef285-200">When you deploy the experiment again, it replaces the web service, now using your updated model.</span></span>  
> 
> 

## <a name="test-the-web-service"></a><span data-ttu-id="ef285-201">Testa webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="ef285-201">Test the web service</span></span>

<span data-ttu-id="ef285-202">När webbtjänsten används anger användarens data via den **Web service indata** modulen där den skickas till den [Poängmodell] [ score-model] modulen och poängsätts.</span><span class="sxs-lookup"><span data-stu-id="ef285-202">When the web service is accessed, the user's data enters through the **Web service input** module where it's passed to the [Score Model][score-model] module and scored.</span></span> <span data-ttu-id="ef285-203">Hur vi har konfigurerat prediktivt experiment, modellen förväntar data i samma format som den ursprungliga kredit risk datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="ef285-203">The way we've set up the predictive experiment, the model expects data in the same format as the original credit risk dataset.</span></span>
<span data-ttu-id="ef285-204">Resultaten returneras till användaren från webbtjänsten via den **Web service utdata** modul.</span><span class="sxs-lookup"><span data-stu-id="ef285-204">The results are returned to the user from the web service through the **Web service output** module.</span></span>

> [!TIP]
> <span data-ttu-id="ef285-205">Hur vi har prediktivt experiment som konfigurerats för hela resultat av den [Poängmodell] [ score-model] modulen returneras.</span><span class="sxs-lookup"><span data-stu-id="ef285-205">The way we have the predictive experiment configured, the entire results from the [Score Model][score-model] module are returned.</span></span> <span data-ttu-id="ef285-206">Detta omfattar alla indata plus värdet kredit risk och sannolikheten för bedömningsprofil.</span><span class="sxs-lookup"><span data-stu-id="ef285-206">This includes all the input data plus the credit risk value and the scoring probability.</span></span> <span data-ttu-id="ef285-207">Men returnerar något annat om du vill, t.ex, du kan returnera bara kredit risk värde.</span><span class="sxs-lookup"><span data-stu-id="ef285-207">But you can return something different if you want - for example, you could return just the credit risk value.</span></span> <span data-ttu-id="ef285-208">Gör detta genom att infoga en [Projektkolumner] [ project-columns] modulen mellan [Poängmodell] [ score-model] och **Web service utdata** att ta bort kolumner som du inte vill webbtjänsten för att returnera.</span><span class="sxs-lookup"><span data-stu-id="ef285-208">To do this, insert a [Project Columns][project-columns] module between [Score Model][score-model] and the **Web service output** to eliminate columns you don't want the web service to return.</span></span> 
> 
> 

<span data-ttu-id="ef285-209">Du kan testa en klassisk web service antingen i **Machine Learning Studio** eller i den **Azure Machine Learning-webbtjänster** portal.</span><span class="sxs-lookup"><span data-stu-id="ef285-209">You can test a Classic web service either in **Machine Learning Studio** or in the **Azure Machine Learning Web Services** portal.</span></span>
<span data-ttu-id="ef285-210">Du kan testa en ny webbplats tjänsten i den **Machine Learning-webbtjänster** portal.</span><span class="sxs-lookup"><span data-stu-id="ef285-210">You can test a New web service only in the **Machine Learning Web Services** portal.</span></span>

> [!TIP]
> <span data-ttu-id="ef285-211">Testa i Azure Machine Learning-webbtjänster portal, kan du ha portal Skapa exempeldata som du kan använda för att testa tjänsten begäran och svar.</span><span class="sxs-lookup"><span data-stu-id="ef285-211">When testing in the Azure Machine Learning Web Services portal, you can have the portal create sample data that you can use to test the Request-Response service.</span></span> <span data-ttu-id="ef285-212">På den **konfigurera** väljer ”Ja” för **exempel Data aktiverat?**.</span><span class="sxs-lookup"><span data-stu-id="ef285-212">On the **Configure** page, select "Yes" for **Sample Data Enabled?**.</span></span> <span data-ttu-id="ef285-213">När du öppnar fliken begäran och svar på de **Test** sidan portalen fylls exempeldata från den ursprungliga kredit risk datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="ef285-213">When you open the Request-Response tab on the **Test** page, the portal fills in sample data taken from the original credit risk dataset.</span></span>

### <a name="test-a-classic-web-service"></a><span data-ttu-id="ef285-214">Testa en klassisk-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="ef285-214">Test a Classic web service</span></span>

<span data-ttu-id="ef285-215">Du kan testa en klassisk webbtjänst i Machine Learning Studio eller i Machine Learning-webbtjänster-portalen.</span><span class="sxs-lookup"><span data-stu-id="ef285-215">You can test a Classic web service in Machine Learning Studio or in the Machine Learning Web Services portal.</span></span> 

#### <a name="test-in-machine-learning-studio"></a><span data-ttu-id="ef285-216">Testa i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="ef285-216">Test in Machine Learning Studio</span></span>

1. <span data-ttu-id="ef285-217">På den **INSTRUMENTPANELEN** för webbtjänsten, klickar du på den **Test** knappen **standard Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="ef285-217">On the **DASHBOARD** page for the web service, click the **Test** button under **Default Endpoint**.</span></span> <span data-ttu-id="ef285-218">En dialogruta visas och du uppmanas att ange indata för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ef285-218">A dialog pops up and asks you for the input data for the service.</span></span> <span data-ttu-id="ef285-219">Det här är samma kolumner som fanns i den ursprungliga kredit risk datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="ef285-219">These are the same columns that appeared in the original credit risk dataset.</span></span>  

2. <span data-ttu-id="ef285-220">Ange en uppsättning data och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef285-220">Enter a set of data and then click **OK**.</span></span> 

#### <a name="test-in-the-machine-learning-web-services-portal"></a><span data-ttu-id="ef285-221">Testa i Machine Learning-webbtjänster portal</span><span class="sxs-lookup"><span data-stu-id="ef285-221">Test in the Machine Learning Web Services portal</span></span>

1. <span data-ttu-id="ef285-222">På den **INSTRUMENTPANELEN** för webbtjänsten, klickar du på den **Test preview** länken under **standard Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="ef285-222">On the **DASHBOARD** page for the web service, click the **Test preview** link under **Default Endpoint**.</span></span> <span data-ttu-id="ef285-223">Testsida i Azure Machine Learning-webbtjänster portal för webbtjänstslutpunkt öppnas och du uppmanas att ange indata för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ef285-223">The test page in the Azure Machine Learning Web Services portal for the web service endpoint opens and asks you for the input data for the service.</span></span> <span data-ttu-id="ef285-224">Det här är samma kolumner som fanns i den ursprungliga kredit risk datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="ef285-224">These are the same columns that appeared in the original credit risk dataset.</span></span>

2. <span data-ttu-id="ef285-225">Klicka på **testa begäran och svar**.</span><span class="sxs-lookup"><span data-stu-id="ef285-225">Click **Test Request-Response**.</span></span> 

### <a name="test-a-new-web-service"></a><span data-ttu-id="ef285-226">Testa en ny webbtjänst</span><span class="sxs-lookup"><span data-stu-id="ef285-226">Test a New web service</span></span>

<span data-ttu-id="ef285-227">Du kan testa en ny webbtjänst endast i Machine Learning-webbtjänster portal.</span><span class="sxs-lookup"><span data-stu-id="ef285-227">You can test a New web service only in the Machine Learning Web Services portal.</span></span>

1. <span data-ttu-id="ef285-228">I den [Azure Machine Learning-webbtjänster](https://services.azureml.net/quickstart) klickar du på **Test** överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="ef285-228">In the [Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal, click **Test** at the top of the page.</span></span> <span data-ttu-id="ef285-229">Den **Test** sidan öppnas och du kan ange data för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ef285-229">The **Test** page opens and you can input data for the service.</span></span> <span data-ttu-id="ef285-230">Inmatningsfält visas motsvarar de kolumner som fanns i den ursprungliga kredit risk datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="ef285-230">The input fields displayed correspond to the columns that appeared in the original credit risk dataset.</span></span> 

2. <span data-ttu-id="ef285-231">Ange en uppsättning data och klicka sedan på **Test-svar på begäranden**.</span><span class="sxs-lookup"><span data-stu-id="ef285-231">Enter a set of data and then click **Test Request-Response**.</span></span>

<span data-ttu-id="ef285-232">Resultatet av testet visas till höger på sidan i utdatakolumnen.</span><span class="sxs-lookup"><span data-stu-id="ef285-232">The results of the test are displayed on the right-hand side of the page in the output column.</span></span> 


## <a name="manage-the-web-service"></a><span data-ttu-id="ef285-233">Hantera webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="ef285-233">Manage the web service</span></span>

### <a name="manage-a-classic-web-service-in-the-azure-classic-portal"></a><span data-ttu-id="ef285-234">Hantera en klassisk webbtjänst i den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ef285-234">Manage a Classic web service in the Azure classic portal</span></span>

<span data-ttu-id="ef285-235">När du har distribuerat klassisk webbtjänsten kan du hantera den från den [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="ef285-235">Once you've deployed your Classic web service, you can manage it from the [Azure classic portal](https://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="ef285-236">Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="ef285-236">Sign in to the [Azure classic portal](https://manage.windowsazure.com)</span></span>
2. <span data-ttu-id="ef285-237">Klicka på panelen för Microsoft Azure-tjänster **MACHINE LEARNING**</span><span class="sxs-lookup"><span data-stu-id="ef285-237">In the Microsoft Azure services panel, click **MACHINE LEARNING**</span></span>
3. <span data-ttu-id="ef285-238">Klicka på arbetsytan</span><span class="sxs-lookup"><span data-stu-id="ef285-238">Click your workspace</span></span>
4. <span data-ttu-id="ef285-239">Klicka på den **webbtjänster** fliken</span><span class="sxs-lookup"><span data-stu-id="ef285-239">Click the **Web services** tab</span></span>
5. <span data-ttu-id="ef285-240">Klicka på den webbtjänst som vi skapade</span><span class="sxs-lookup"><span data-stu-id="ef285-240">Click the web service we created</span></span>
6. <span data-ttu-id="ef285-241">Klicka på ”default”-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="ef285-241">Click the "default" endpoint</span></span>

<span data-ttu-id="ef285-242">Härifrån kan du kan göra saker som övervaka hur webbtjänsten fungerar och kontrollera prestanda justeringar genom att ändra hur många samtidiga anropar tjänsten kan hantera.</span><span class="sxs-lookup"><span data-stu-id="ef285-242">From here, you can do things like monitor how the web service is doing and make performance tweaks by changing how many concurrent calls the service can handle.</span></span>

<span data-ttu-id="ef285-243">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="ef285-243">For more details, see:</span></span>

* [<span data-ttu-id="ef285-244">Skapa slutpunkter</span><span class="sxs-lookup"><span data-stu-id="ef285-244">Creating Endpoints</span></span>](machine-learning-create-endpoint.md)
* [<span data-ttu-id="ef285-245">Skalning webbtjänst</span><span class="sxs-lookup"><span data-stu-id="ef285-245">Scaling web service</span></span>](machine-learning-scaling-webservice.md)

### <a name="manage-a-classic-or-new-web-service-in-the-azure-machine-learning-web-services-portal"></a><span data-ttu-id="ef285-246">Hantera en klassisk eller ny webbtjänst i Azure Machine Learning-webbtjänster portal</span><span class="sxs-lookup"><span data-stu-id="ef285-246">Manage a Classic or New web service in the Azure Machine Learning Web Services portal</span></span>

<span data-ttu-id="ef285-247">När du har distribuerat din webbtjänst om klassiska eller nya kan du hantera den från den [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/quickstart) portal.</span><span class="sxs-lookup"><span data-stu-id="ef285-247">Once you've deployed your web service, whether Classic or New, you can manage it from the [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal.</span></span>

<span data-ttu-id="ef285-248">Att övervaka prestanda för webbtjänsten:</span><span class="sxs-lookup"><span data-stu-id="ef285-248">To monitor the performance of your web service:</span></span>

1. <span data-ttu-id="ef285-249">Logga in på den [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/quickstart) portal</span><span class="sxs-lookup"><span data-stu-id="ef285-249">Sign in to the [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal</span></span>
2. <span data-ttu-id="ef285-250">Klicka på **webbtjänster**</span><span class="sxs-lookup"><span data-stu-id="ef285-250">Click **Web services**</span></span>
3. <span data-ttu-id="ef285-251">Klicka på din web service</span><span class="sxs-lookup"><span data-stu-id="ef285-251">Click your web service</span></span>
4. <span data-ttu-id="ef285-252">Klicka på den **instrumentpanelen**</span><span class="sxs-lookup"><span data-stu-id="ef285-252">Click the **Dashboard**</span></span>

- - -
<span data-ttu-id="ef285-253">**Nästa: [få åtkomst till webbtjänsten](machine-learning-walkthrough-6-access-web-service.md)**</span><span class="sxs-lookup"><span data-stu-id="ef285-253">**Next: [Access the web service](machine-learning-walkthrough-6-access-web-service.md)**</span></span>

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
