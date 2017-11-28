---
title: "Steg 4: Träna och utvärdera hello analytiska förutsägelsemodeller | Microsoft Docs"
description: "Steg 4 i hello utveckla en förutsägelselösning genomgång: tåg och poängsätta och utvärdera flera modeller i Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: d905f6b3-9201-4117-b769-5f9ed5ee1cac
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: d86d7c5ae7524f71fe44d985db67c4618b7965a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-4-train-and-evaluate-hello-predictive-analytic-models"></a><span data-ttu-id="60fe3-103">Genomgång steg 4: Träna och utvärdera hello analytiska förutsägelsemodeller</span><span class="sxs-lookup"><span data-stu-id="60fe3-103">Walkthrough Step 4: Train and evaluate hello predictive analytic models</span></span>
<span data-ttu-id="60fe3-104">Det här avsnittet innehåller hello fjärde steget i hello genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="60fe3-104">This topic contains hello fourth step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="60fe3-105">Skapa en Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="60fe3-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="60fe3-106">Överför befintliga data</span><span class="sxs-lookup"><span data-stu-id="60fe3-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="60fe3-107">Skapa ett nytt experiment</span><span class="sxs-lookup"><span data-stu-id="60fe3-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. <span data-ttu-id="60fe3-108">**Träna och utvärdera hello modeller**</span><span class="sxs-lookup"><span data-stu-id="60fe3-108">**Train and evaluate hello models**</span></span>
5. [<span data-ttu-id="60fe3-109">Distribuera hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="60fe3-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="60fe3-110">Få åtkomst till hello-webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="60fe3-110">Access hello Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="60fe3-111">En av hello fördelarna med att använda Azure Machine Learning Studio för att skapa machine learning-modeller är hello möjlighet tootry mer än en typ av modellen samtidigt i en enda experiment och jämföra hello resultat.</span><span class="sxs-lookup"><span data-stu-id="60fe3-111">One of hello benefits of using Azure Machine Learning Studio for creating machine learning models is hello ability tootry more than one type of model at a time in a single experiment and compare hello results.</span></span> <span data-ttu-id="60fe3-112">Den här typen av experiment hjälper dig att hitta hello bästa lösningen för ditt problem.</span><span class="sxs-lookup"><span data-stu-id="60fe3-112">This type of experimentation helps you find hello best solution for your problem.</span></span>

<span data-ttu-id="60fe3-113">I hello experiment Vi utvecklar i den här genomgången, vi skapa två olika typer av modeller och jämför deras bedömningsprofil resultat toodecide vilken algoritm vi vill toouse i vår slutliga experimentet.</span><span class="sxs-lookup"><span data-stu-id="60fe3-113">In hello experiment we're developing in this walkthrough, we'll create two different types of models and then compare their scoring results toodecide which algorithm we want toouse in our final experiment.</span></span>  

<span data-ttu-id="60fe3-114">Det finns olika modeller som vi kan välja från.</span><span class="sxs-lookup"><span data-stu-id="60fe3-114">There are various models we could choose from.</span></span> <span data-ttu-id="60fe3-115">toosee hello modeller är tillgängliga, expandera hello **Maskininlärning** nod i hello modulpaletten och expandera sedan **initiera modell** och hello noder under den.</span><span class="sxs-lookup"><span data-stu-id="60fe3-115">toosee hello models available, expand hello **Machine Learning** node in hello module palette, and then expand **Initialize Model** and hello nodes beneath it.</span></span> <span data-ttu-id="60fe3-116">Hello enligt experimentet vi välja hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] (SVM) och hello [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] moduler.</span><span class="sxs-lookup"><span data-stu-id="60fe3-116">For hello purposes of this experiment, we'll select hello [Two-Class Support Vector Machine][two-class-support-vector-machine] (SVM) and hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] modules.</span></span>    

> [!TIP]
> <span data-ttu-id="60fe3-117">tooget hjälp för att bestämma vilka Machine Learning-algoritmen bäst passar hello viss problemet du försöker toosolve, se [hur toochoose algoritmer för Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).</span><span class="sxs-lookup"><span data-stu-id="60fe3-117">tooget help deciding which Machine Learning algorithm best suits hello particular problem you're trying toosolve, see [How toochoose algorithms for Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).</span></span>
> 
> 

## <a name="train-hello-models"></a><span data-ttu-id="60fe3-118">Träna hello modeller</span><span class="sxs-lookup"><span data-stu-id="60fe3-118">Train hello models</span></span>

<span data-ttu-id="60fe3-119">Vi lägger till båda hello [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] modulen och [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modul i detta experimentet.</span><span class="sxs-lookup"><span data-stu-id="60fe3-119">We'll add both hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module and [Two-Class Support Vector Machine][two-class-support-vector-machine] module in this experiment.</span></span>

### <a name="two-class-boosted-decision-tree"></a><span data-ttu-id="60fe3-120">Two-Class Tvåklassförhöjda beslutsträdet</span><span class="sxs-lookup"><span data-stu-id="60fe3-120">Two-Class Boosted Decision Tree</span></span>

<span data-ttu-id="60fe3-121">Först ska vi ställa in hello ökat beslut trädet modellen.</span><span class="sxs-lookup"><span data-stu-id="60fe3-121">First, let's set up hello boosted decision tree model.</span></span>

1. <span data-ttu-id="60fe3-122">Hitta hello [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] modul i hello modulpaletten och drar den till hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="60fe3-122">Find hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module in hello module palette and drag it onto hello canvas.</span></span>

2. <span data-ttu-id="60fe3-123">Hitta hello [Träningsmodell] [ train-model] modulen, drar den till arbetsytan hello och ansluter sedan hello utdata från hello [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree]modulen toohello kvar indataport av hello [Träningsmodell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="60fe3-123">Find hello [Train Model][train-model] module, drag it onto hello canvas, and then connect hello output of hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module toohello left input port of hello [Train Model][train-model] module.</span></span>
   
   <span data-ttu-id="60fe3-124">Hej [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] modulen initierar hello allmän modell, och [Träningsmodell] [ train-model] använder träningsdata tootrain hello modell.</span><span class="sxs-lookup"><span data-stu-id="60fe3-124">hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module initializes hello generic model, and [Train Model][train-model] uses training data tootrain hello model.</span></span> 

3. <span data-ttu-id="60fe3-125">Ansluta hello vänstra utdata från hello vänster [köra R-skriptet] [ execute-r-script] modulen toohello höger inkommande port för hello [Träningsmodell] [ train-model] modul (vi valt i [steg3](machine-learning-walkthrough-3-create-new-experiment.md) för den här genomgången toouse hello data från vänster sida av hello dela Data-modulen för utbildning hello).</span><span class="sxs-lookup"><span data-stu-id="60fe3-125">Connect hello left output of hello left [Execute R Script][execute-r-script] module toohello right input port of hello [Train Model][train-model] module (we decided in [Step 3](machine-learning-walkthrough-3-create-new-experiment.md) of this walkthrough toouse hello data coming from hello left side of hello Split Data module for training).</span></span>
   
   > [!TIP]
   > <span data-ttu-id="60fe3-126">Vi behöver inte två hello indata och en av hello utdata för hello [köra R-skriptet] [ execute-r-script] modul för experimentet, så vi kan lämna dem.</span><span class="sxs-lookup"><span data-stu-id="60fe3-126">We don't need two of hello inputs and one of hello outputs of hello [Execute R Script][execute-r-script] module for this experiment, so we can leave them unattached.</span></span> 
   > 
   > 

<span data-ttu-id="60fe3-127">Den här delen av hello experiment nu ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="60fe3-127">This portion of hello experiment now looks something like this:</span></span>  

![En modell][1]

<span data-ttu-id="60fe3-129">Nu måste vi tootell hello [Träningsmodell] [ train-model] modul som vi vill hello modellen toopredict hello kreditrisk värde.</span><span class="sxs-lookup"><span data-stu-id="60fe3-129">Now we need tootell hello [Train Model][train-model] module that we want hello model toopredict hello Credit Risk value.</span></span>

1. <span data-ttu-id="60fe3-130">Välj hello [Träningsmodell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="60fe3-130">Select hello [Train Model][train-model] module.</span></span> <span data-ttu-id="60fe3-131">I hello **egenskaper** rutan klickar du på **starta kolumnväljaren**.</span><span class="sxs-lookup"><span data-stu-id="60fe3-131">In hello **Properties** pane, click **Launch column selector**.</span></span>

2. <span data-ttu-id="60fe3-132">I hello **Markera en kolumn** dialogrutan Skriv ”kredit risk” i hello sökfältet under **tillgängliga kolumner**markerar ”kredit risk” nedan och klicka på högerpilen för hello ( **>** ) toomove ”kredit risk” för**valda kolumner**.</span><span class="sxs-lookup"><span data-stu-id="60fe3-132">In hello **Select a single column** dialog, type "credit risk" in hello search field under **Available Columns**, select "Credit risk" below, and click hello right arrow button (**>**) toomove "Credit risk" too**Selected Columns**.</span></span> 

    ![Markera hello kreditrisk kolumnen för hello träningsmodellmodulen][0]

3. <span data-ttu-id="60fe3-134">Klicka på hello **OK** är markerat.</span><span class="sxs-lookup"><span data-stu-id="60fe3-134">Click hello **OK** check mark.</span></span>

### <a name="two-class-support-vector-machine"></a><span data-ttu-id="60fe3-135">Tvåklassig dator för vektorstöd</span><span class="sxs-lookup"><span data-stu-id="60fe3-135">Two-Class Support Vector Machine</span></span>

<span data-ttu-id="60fe3-136">Nu ska ställa vi in hello SVM modellen.</span><span class="sxs-lookup"><span data-stu-id="60fe3-136">Next, we set up hello SVM model.</span></span>  

<span data-ttu-id="60fe3-137">Första, en förklaring om SVM.</span><span class="sxs-lookup"><span data-stu-id="60fe3-137">First, a little explanation about SVM.</span></span> <span data-ttu-id="60fe3-138">Förstärkta beslutsträd fungerar bra med funktioner för alla typer.</span><span class="sxs-lookup"><span data-stu-id="60fe3-138">Boosted decision trees work well with features of any type.</span></span> <span data-ttu-id="60fe3-139">Men eftersom hello SVM modulen genererar en linjär klassificerare, hello som genereras har hello bästa testfel när alla numeriska funktioner har hello samma skala.</span><span class="sxs-lookup"><span data-stu-id="60fe3-139">However, since hello SVM module generates a linear classifier, hello model that it generates has hello best test error when all numeric features have hello same scale.</span></span> <span data-ttu-id="60fe3-140">tooconvert alla numeriska funktioner toohello samma skala måste vi använder en ”Tanh”-omvandling (med hello [normalisera Data] [ normalize-data] module).</span><span class="sxs-lookup"><span data-stu-id="60fe3-140">tooconvert all numeric features toohello same scale, we use a "Tanh" transformation (with hello [Normalize Data][normalize-data] module).</span></span> <span data-ttu-id="60fe3-141">Detta omvandlar våra siffror till intervallet för hello [0,1].</span><span class="sxs-lookup"><span data-stu-id="60fe3-141">This transforms our numbers into hello [0,1] range.</span></span> <span data-ttu-id="60fe3-142">hello SVM modulen konverterar sträng funktioner toocategorical funktioner och sedan toobinary 0-1-funktioner, så vi inte behöver toomanually Omforma strängen funktioner.</span><span class="sxs-lookup"><span data-stu-id="60fe3-142">hello SVM module converts string features toocategorical features and then toobinary 0/1 features, so we don't need toomanually transform string features.</span></span> <span data-ttu-id="60fe3-143">Dessutom bör inte tootransform hello kreditrisk kolumn (21) – är det numeriska, men det är hello värdet vi tränar hello modellen toopredict tooleave måste den enbart.</span><span class="sxs-lookup"><span data-stu-id="60fe3-143">Also, we don't want tootransform hello Credit Risk column (column 21) - it's numeric, but it's hello value we're training hello model toopredict, so we need tooleave it alone.</span></span>  

<span data-ttu-id="60fe3-144">tooset in hello SVM modell hello följande:</span><span class="sxs-lookup"><span data-stu-id="60fe3-144">tooset up hello SVM model, do hello following:</span></span>

1. <span data-ttu-id="60fe3-145">Hitta hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modul i hello modulpaletten och drar den till hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="60fe3-145">Find hello [Two-Class Support Vector Machine][two-class-support-vector-machine] module in hello module palette and drag it onto hello canvas.</span></span>

2. <span data-ttu-id="60fe3-146">Högerklicka på hello [Träningsmodell] [ train-model] modulen, Välj **kopiera**, och högerklicka hello arbetsytan och välj **klistra in**.</span><span class="sxs-lookup"><span data-stu-id="60fe3-146">Right-click hello [Train Model][train-model] module, select **Copy**, and then right-click hello canvas and select **Paste**.</span></span> <span data-ttu-id="60fe3-147">Hej kopia av hello [Träningsmodell] [ train-model] modulen har hello samma Kolumnurval av hello ursprungliga.</span><span class="sxs-lookup"><span data-stu-id="60fe3-147">hello copy of hello [Train Model][train-model] module has hello same column selection as hello original.</span></span>

3. <span data-ttu-id="60fe3-148">Ansluta hello utdata från hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulen toohello kvar indataport av hello andra [Träningsmodell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="60fe3-148">Connect hello output of hello [Two-Class Support Vector Machine][two-class-support-vector-machine] module toohello left input port of hello second [Train Model][train-model] module.</span></span>

4. <span data-ttu-id="60fe3-149">Hitta hello [normalisera Data] [ normalize-data] modulen och drar den till hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="60fe3-149">Find hello [Normalize Data][normalize-data] module and drag it onto hello canvas.</span></span>

5. <span data-ttu-id="60fe3-150">Ansluta hello vänstra utdata från hello vänster [köra R-skriptet] [ execute-r-script] toohello modulindata till den här modulen (Observera att hello utdataporten för en modul kan vara anslutna toomore än en modul).</span><span class="sxs-lookup"><span data-stu-id="60fe3-150">Connect hello left output of hello left [Execute R Script][execute-r-script] module toohello input of this module (notice that hello output port of a module may be connected toomore than one other module).</span></span>

6. <span data-ttu-id="60fe3-151">Ansluta hello kvar utdataporten för hello [normalisera Data] [ normalize-data] modulen toohello höger inkommande port för hello andra [Träningsmodell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="60fe3-151">Connect hello left output port of hello [Normalize Data][normalize-data] module toohello right input port of hello second [Train Model][train-model] module.</span></span>

<span data-ttu-id="60fe3-152">Den här delen av vår experimentet bör nu se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="60fe3-152">This portion of our experiment should now look something like this:</span></span>  

![Utbildning hello andra modell][2]  

<span data-ttu-id="60fe3-154">Nu konfigurera hello [normalisera Data] [ normalize-data] modulen:</span><span class="sxs-lookup"><span data-stu-id="60fe3-154">Now configure hello [Normalize Data][normalize-data] module:</span></span>

1. <span data-ttu-id="60fe3-155">Klicka på tooselect hello [normalisera Data] [ normalize-data] modul.</span><span class="sxs-lookup"><span data-stu-id="60fe3-155">Click tooselect hello [Normalize Data][normalize-data] module.</span></span> <span data-ttu-id="60fe3-156">I hello **egenskaper** väljer **Tanh** för hello **omvandling metoden** parameter.</span><span class="sxs-lookup"><span data-stu-id="60fe3-156">In hello **Properties** pane, select **Tanh** for hello **Transformation method** parameter.</span></span>

2. <span data-ttu-id="60fe3-157">Klicka på **starta kolumnväljaren**, Välj ”inga kolumner” för **börjar med**väljer **inkludera** i hello första listrutan väljer **kolumntypen**i hello andra listrutan och välj **numeriska** i hello tredje listrutan.</span><span class="sxs-lookup"><span data-stu-id="60fe3-157">Click **Launch column selector**, select "No columns" for **Begin With**, select **Include** in hello first dropdown, select **column type** in hello second dropdown, and select **Numeric** in hello third dropdown.</span></span> <span data-ttu-id="60fe3-158">Anger att alla hello numeriska kolumner (och endast numeriskt) omvandlas.</span><span class="sxs-lookup"><span data-stu-id="60fe3-158">This specifies that all hello numeric columns (and only numeric) are transformed.</span></span>

3. <span data-ttu-id="60fe3-159">Klicka på hello plustecken (+) toohello höger i den här raden - detta skapar en rad av nedrullningsbara listorna.</span><span class="sxs-lookup"><span data-stu-id="60fe3-159">Click hello plus sign (+) toohello right of this row - this creates a row of dropdowns.</span></span> <span data-ttu-id="60fe3-160">Välj **undanta** i hello första listrutan väljer **kolumnnamn** i hello andra listrutan och ange ”kredit risk” i hello textfältet.</span><span class="sxs-lookup"><span data-stu-id="60fe3-160">Select **Exclude** in hello first dropdown, select **column names** in hello second dropdown, and enter "Credit risk" in hello text field.</span></span> <span data-ttu-id="60fe3-161">Detta anger hello kreditrisk kolumnen ska ignoreras (vi måste toodo detta eftersom den här kolumnen är numeriska och så skulle omvandlas om vi inte utesluter den).</span><span class="sxs-lookup"><span data-stu-id="60fe3-161">This specifies that hello Credit Risk column should be ignored (we need toodo this because this column is numeric and so would be transformed if we didn't exclude it).</span></span>

4. <span data-ttu-id="60fe3-162">Klicka på hello **OK** är markerat.</span><span class="sxs-lookup"><span data-stu-id="60fe3-162">Click hello **OK** check mark.</span></span>  

    ![Välj kolumner för hello normalisera Data modulen][5]

<span data-ttu-id="60fe3-164">Hej [normalisera Data] [ normalize-data] modulen är nu set tooperform en Tanh omvandling på alla numeriska kolumner utom hello kreditrisk.</span><span class="sxs-lookup"><span data-stu-id="60fe3-164">hello [Normalize Data][normalize-data] module is now set tooperform a Tanh transformation on all numeric columns except for hello Credit Risk column.</span></span>  

## <a name="score-and-evaluate-hello-models"></a><span data-ttu-id="60fe3-165">Poängsätta och utvärdera hello modeller</span><span class="sxs-lookup"><span data-stu-id="60fe3-165">Score and evaluate hello models</span></span>

<span data-ttu-id="60fe3-166">Vi använder hello testa data som separeras av hello [dela Data] [ split] modulen tooscore våra tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="60fe3-166">We use hello testing data that was separated out by hello [Split Data][split] module tooscore our trained models.</span></span> <span data-ttu-id="60fe3-167">Vi kan sedan jämföra hello resultaten av hello två modeller toosee som genererade bättre resultat.</span><span class="sxs-lookup"><span data-stu-id="60fe3-167">We can then compare hello results of hello two models toosee which generated better results.</span></span>  

### <a name="add-hello-score-model-modules"></a><span data-ttu-id="60fe3-168">Lägga till hello poängsätta modell moduler</span><span class="sxs-lookup"><span data-stu-id="60fe3-168">Add hello Score Model modules</span></span>

1. <span data-ttu-id="60fe3-169">Hitta hello [Poängmodell] [ score-model] modulen och drar den till hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="60fe3-169">Find hello [Score Model][score-model] module and drag it onto hello canvas.</span></span>

2. <span data-ttu-id="60fe3-170">Ansluta hello [Träningsmodell] [ train-model] modul som har anslutit toohello [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] modulen toohello vänstra indata porten för hello [Poängmodell] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="60fe3-170">Connect hello [Train Model][train-model] module that's connected toohello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module toohello left input port of hello [Score Model][score-model] module.</span></span>

3. <span data-ttu-id="60fe3-171">Ansluta hello höger [köra R-skriptet] [ execute-r-script] modul (våra tester data) toohello höger inkommande port för hello [Poängmodell] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="60fe3-171">Connect hello right [Execute R Script][execute-r-script] module (our testing data) toohello right input port of hello [Score Model][score-model] module.</span></span>

    ![Ansluten att modulen poängsätta modell][6]
   
   <span data-ttu-id="60fe3-173">Hej [Poängmodell] [ score-model] modul kan nu ta hello kredit information från hello testning informationen genom att köra via hello modellen, och jämföra hello förutsägelser hello modellen genererar med hello faktiska kredit risk kolumnen i hello testa data.</span><span class="sxs-lookup"><span data-stu-id="60fe3-173">hello [Score Model][score-model] module can now take hello credit information from hello testing data, run it through hello model, and compare hello predictions hello model generates with hello actual credit risk column in hello testing data.</span></span>

4. <span data-ttu-id="60fe3-174">Kopiera och klistra in hello [Poängmodell] [ score-model] modulen toocreate en andra kopia.</span><span class="sxs-lookup"><span data-stu-id="60fe3-174">Copy and paste hello [Score Model][score-model] module toocreate a second copy.</span></span>

5. <span data-ttu-id="60fe3-175">Ansluta hello utdata från hello SVM modellen (det vill säga hello utgående port för hello [Träningsmodell] [ train-model] modul som har anslutit toohello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulen) toohello inkommande port för hello andra [Poängmodell] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="60fe3-175">Connect hello output of hello SVM model (that is, hello output port of hello [Train Model][train-model] module that's connected toohello [Two-Class Support Vector Machine][two-class-support-vector-machine] module) toohello input port of hello second [Score Model][score-model] module.</span></span>

6. <span data-ttu-id="60fe3-176">För hello SVM modellen har vi toodo hello samma omvandling toohello testdata som vi gjorde toohello utbildningsdata.</span><span class="sxs-lookup"><span data-stu-id="60fe3-176">For hello SVM model, we have toodo hello same transformation toohello test data as we did toohello training data.</span></span> <span data-ttu-id="60fe3-177">Kopiera och klistra in hello så [normalisera Data] [ normalize-data] modulen toocreate en andra kopia och koppla den högra toohello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="60fe3-177">So copy and paste hello [Normalize Data][normalize-data] module toocreate a second copy and connect it toohello right [Execute R Script][execute-r-script] module.</span></span>

7. <span data-ttu-id="60fe3-178">Ansluta hello vänstra utdata från hello andra [normalisera Data] [ normalize-data] modulen toohello höger inkommande port för hello andra [Poängmodell] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="60fe3-178">Connect hello left output of hello second [Normalize Data][normalize-data] module toohello right input port of hello second [Score Model][score-model] module.</span></span>

    ![Båda poängsätta modell-moduler som är ansluten][7]

### <a name="add-hello-evaluate-model-module"></a><span data-ttu-id="60fe3-180">Lägg till hello utvärdera modell</span><span class="sxs-lookup"><span data-stu-id="60fe3-180">Add hello Evaluate Model module</span></span>

<span data-ttu-id="60fe3-181">tooevaluate hello två bedömningsprofil resultat och jämför dem, vi använder en [utvärdera modell] [ evaluate-model] modul.</span><span class="sxs-lookup"><span data-stu-id="60fe3-181">tooevaluate hello two scoring results and compare them, we use an [Evaluate Model][evaluate-model] module.</span></span>  

1. <span data-ttu-id="60fe3-182">Hitta hello [utvärdera modell] [ evaluate-model] modulen och drar den till hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="60fe3-182">Find hello [Evaluate Model][evaluate-model] module and drag it onto hello canvas.</span></span>

2. <span data-ttu-id="60fe3-183">Ansluta hello utdataporten för hello [Poängmodell] [ score-model] modulen som hör till hello ökat beslut trädet modellen vänster toohello inkommande port för hello [utvärdera modell] [ evaluate-model] modul.</span><span class="sxs-lookup"><span data-stu-id="60fe3-183">Connect hello output port of hello [Score Model][score-model] module associated with hello boosted decision tree model toohello left input port of hello [Evaluate Model][evaluate-model] module.</span></span>

3. <span data-ttu-id="60fe3-184">Ansluta hello andra [Poängmodell] [ score-model] modulen toohello höger inkommande port.</span><span class="sxs-lookup"><span data-stu-id="60fe3-184">Connect hello other [Score Model][score-model] module toohello right input port.</span></span>  

    ![Utvärdera modellen modulen ansluten][8]

### <a name="run-hello-experiment-and-check-hello-results"></a><span data-ttu-id="60fe3-186">Kör experimentet hello och kontrollera hello resultat</span><span class="sxs-lookup"><span data-stu-id="60fe3-186">Run hello experiment and check hello results</span></span>

<span data-ttu-id="60fe3-187">toorun Hej experiment, klicka på hello **kör** nedan hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="60fe3-187">toorun hello experiment, click hello **RUN** button below hello canvas.</span></span> <span data-ttu-id="60fe3-188">Det kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="60fe3-188">It may take a few minutes.</span></span> <span data-ttu-id="60fe3-189">En snurrande indikator för varje modul visar att det körs och sedan en grön bock visas när hello modulen är klar.</span><span class="sxs-lookup"><span data-stu-id="60fe3-189">A spinning indicator on each module shows that it's running, and then a green check mark shows when hello module is finished.</span></span> <span data-ttu-id="60fe3-190">När alla hello-moduler har markerat, har hello experimentet slutförts.</span><span class="sxs-lookup"><span data-stu-id="60fe3-190">When all hello modules have a check mark, hello experiment has finished running.</span></span>

<span data-ttu-id="60fe3-191">hello experimentet bör nu se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="60fe3-191">hello experiment should now look something like this:</span></span>  

![Utvärdering av båda modellerna][3]

<span data-ttu-id="60fe3-193">toocheck hello resultaten klickar du på hello utdataporten för hello [utvärdera modell] [ evaluate-model] modulen och välj **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="60fe3-193">toocheck hello results, click hello output port of hello [Evaluate Model][evaluate-model] module and select **Visualize**.</span></span>  

<span data-ttu-id="60fe3-194">Hej [utvärdera modell] [ evaluate-model] modulen genererar ett par med kurvor och mått som gör att du toocompare hello resultaten av hello två poängsatta modeller.</span><span class="sxs-lookup"><span data-stu-id="60fe3-194">hello [Evaluate Model][evaluate-model] module produces a pair of curves and metrics that allow you toocompare hello results of hello two scored models.</span></span> <span data-ttu-id="60fe3-195">Du kan visa hello resultat som mottagaren operatorn egenskap (ROC) kurvor, Precision/återkalla kurvor eller Lift kurvor.</span><span class="sxs-lookup"><span data-stu-id="60fe3-195">You can view hello results as Receiver Operator Characteristic (ROC) curves, Precision/Recall curves, or Lift curves.</span></span> <span data-ttu-id="60fe3-196">Ytterligare data som visas innehåller en matris med förvirring kumulativa värden för hello område under hello kurvan (AUC) och andra mått.</span><span class="sxs-lookup"><span data-stu-id="60fe3-196">Additional data displayed includes a confusion matrix, cumulative values for hello area under hello curve (AUC), and other metrics.</span></span> <span data-ttu-id="60fe3-197">Du kan ändra hello tröskelvärdet genom att flytta skjutreglaget hello åt vänster eller höger och se hur den påverkar hello uppsättning mått.</span><span class="sxs-lookup"><span data-stu-id="60fe3-197">You can change hello threshold value by moving hello slider left or right and see how it affects hello set of metrics.</span></span>  

<span data-ttu-id="60fe3-198">toohello direkt av hello diagram, klickar du på **bedömas dataset** eller **bedömas dataset toocompare** toohighlight hello associerade kurva och toodisplay hello associerade mått nedan.</span><span class="sxs-lookup"><span data-stu-id="60fe3-198">toohello right of hello graph, click **Scored dataset** or **Scored dataset toocompare** toohighlight hello associated curve and toodisplay hello associated metrics below.</span></span> <span data-ttu-id="60fe3-199">Hello förklaring för hello kurvor, ”bedömas dataset” motsvarar toohello kvar indataport av hello [utvärdera modell] [ evaluate-model] modul - i vårt fall är hello ökat beslut trädet modellen.</span><span class="sxs-lookup"><span data-stu-id="60fe3-199">In hello legend for hello curves, "Scored dataset" corresponds toohello left input port of hello [Evaluate Model][evaluate-model] module - in our case, this is hello boosted decision tree model.</span></span> <span data-ttu-id="60fe3-200">”Bedömas dataset toocompare” motsvarar toohello högra indataporten - hello SVM modellen i vårt fall.</span><span class="sxs-lookup"><span data-stu-id="60fe3-200">"Scored dataset toocompare" corresponds toohello right input port - hello SVM model in our case.</span></span> <span data-ttu-id="60fe3-201">När du klickar på en av etiketterna hello kurva för den modellen som är markerad och hello motsvarande mått visas som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="60fe3-201">When you click one of these labels, hello curve for that model is highlighted and hello corresponding metrics are displayed, as shown in hello following graphic.</span></span>  

![ROC kurvor för modeller][4]

<span data-ttu-id="60fe3-203">Du kan välja vilken modell är närmaste toogiving du hello resultat som du letar efter genom att undersöka dessa värden.</span><span class="sxs-lookup"><span data-stu-id="60fe3-203">By examining these values, you can decide which model is closest toogiving you hello results you're looking for.</span></span> <span data-ttu-id="60fe3-204">Du kan gå tillbaka och iterera experimentet genom att ändra parametervärden i hello olika modeller.</span><span class="sxs-lookup"><span data-stu-id="60fe3-204">You can go back and iterate on your experiment by changing parameter values in hello different models.</span></span> 

<span data-ttu-id="60fe3-205">hello vetenskap och bilder av tolka resultaten och justera hello modellen prestanda är utanför hello omfånget för den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="60fe3-205">hello science and art of interpreting these results and tuning hello model performance is outside hello scope of this walkthrough.</span></span> <span data-ttu-id="60fe3-206">För ytterligare hjälp kan du läsa hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="60fe3-206">For additional help, you might read hello following articles:</span></span>
- [<span data-ttu-id="60fe3-207">Hur tooevaluate modell prestanda i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="60fe3-207">How tooevaluate model performance in Azure Machine Learning</span></span>](machine-learning-evaluate-model-performance.md)
- [<span data-ttu-id="60fe3-208">Välj parametrar toooptimize algoritmerna i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="60fe3-208">Choose parameters toooptimize your algorithms in Azure Machine Learning</span></span>](machine-learning-algorithm-parameters-optimize.md)
- [<span data-ttu-id="60fe3-209">Tolka modellen resultaten i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="60fe3-209">Interpret model results in Azure Machine Learning</span></span>](machine-learning-interpret-model-results.md)

> [!TIP]
> <span data-ttu-id="60fe3-210">Varje gång du kör hello experiment en post på den iterationen sparas i hello Körningshistorik.</span><span class="sxs-lookup"><span data-stu-id="60fe3-210">Each time you run hello experiment a record of that iteration is kept in hello Run History.</span></span> <span data-ttu-id="60fe3-211">Du kan visa dessa iterationer och returnera tooany av dem genom att klicka på **visa KÖRNINGSHISTORIK** nedan hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="60fe3-211">You can view these iterations, and return tooany of them, by clicking **VIEW RUN HISTORY** below hello canvas.</span></span> <span data-ttu-id="60fe3-212">Du kan också klicka på **tidigare kör** i hello **egenskaper** fönstret tooreturn toohello iteration omedelbart före hello en öppen.</span><span class="sxs-lookup"><span data-stu-id="60fe3-212">You can also click **Prior Run** in hello **Properties** pane tooreturn toohello iteration immediately preceding hello one you have open.</span></span>
> 
> <span data-ttu-id="60fe3-213">Du kan göra en kopia av eventuella iterationer av experimentet genom att klicka på **Spara som** nedan hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="60fe3-213">You can make a copy of any iteration of your experiment by clicking **SAVE AS** below hello canvas.</span></span> 
> <span data-ttu-id="60fe3-214">Använd hello experiment **sammanfattning** och **beskrivning** egenskaper tookeep en post på vad du har gjort i iterationer av experiment.</span><span class="sxs-lookup"><span data-stu-id="60fe3-214">Use hello experiment's **Summary** and **Description** properties tookeep a record of what you've tried in your experiment iterations.</span></span>
> 
> <span data-ttu-id="60fe3-215">Mer information finns i [hantera iterationer av experiment i Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md).</span><span class="sxs-lookup"><span data-stu-id="60fe3-215">For more details, see [Manage experiment iterations in Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md).</span></span>  
> 
> 

- - -
<span data-ttu-id="60fe3-216">**Nästa: [distribuera hello-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md)**</span><span class="sxs-lookup"><span data-stu-id="60fe3-216">**Next: [Deploy hello web service](machine-learning-walkthrough-5-publish-web-service.md)**</span></span>

[0]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train-model-select-column.png
[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/experiment-with-train-model.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/svm-model-added.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/final-experiment.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/roc-curves.png
[5]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/normalize-data-select-column.png
[6]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/score-model-connected.png
[7]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/both-score-models-added.png
[8]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/evaluate-model-added.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
