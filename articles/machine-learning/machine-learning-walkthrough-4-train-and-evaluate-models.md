---
title: "Steg 4: Träna och utvärdera de analytiska förutsägelsemodeller | Microsoft Docs"
description: "Steg 4 i utveckla en förutsägelselösning genomgång: tåg och poängsätta och utvärdera flera modeller i Azure Machine Learning Studio."
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
ms.openlocfilehash: 58d46dd1464ec0a3fc9639f78d4429e0e778c2bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a><span data-ttu-id="01579-103">Genomgång steg 4: Utbilda i och utvärdera förutsägbara analytiska modeller</span><span class="sxs-lookup"><span data-stu-id="01579-103">Walkthrough Step 4: Train and evaluate the predictive analytic models</span></span>
<span data-ttu-id="01579-104">Det här avsnittet innehåller det fjärde steget i den här genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="01579-104">This topic contains the fourth step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="01579-105">Skapa en Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="01579-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="01579-106">Överför befintliga data</span><span class="sxs-lookup"><span data-stu-id="01579-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="01579-107">Skapa ett nytt experiment</span><span class="sxs-lookup"><span data-stu-id="01579-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. <span data-ttu-id="01579-108">**Träna och utvärdera modellerna**</span><span class="sxs-lookup"><span data-stu-id="01579-108">**Train and evaluate the models**</span></span>
5. [<span data-ttu-id="01579-109">Distribuera webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="01579-109">Deploy the Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="01579-110">Få åtkomst till webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="01579-110">Access the Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="01579-111">En av fördelarna med att använda Azure Machine Learning Studio för att skapa machine learning-modeller är möjligheten att testa fler än en typ av modellen samtidigt i en enda experiment och jämför resultaten.</span><span class="sxs-lookup"><span data-stu-id="01579-111">One of the benefits of using Azure Machine Learning Studio for creating machine learning models is the ability to try more than one type of model at a time in a single experiment and compare the results.</span></span> <span data-ttu-id="01579-112">Den här typen av experiment hjälper dig att hitta den bästa lösningen för ditt problem.</span><span class="sxs-lookup"><span data-stu-id="01579-112">This type of experimentation helps you find the best solution for your problem.</span></span>

<span data-ttu-id="01579-113">I experimentet Vi utvecklar i den här genomgången, vi skapa två olika typer av modeller och sedan jämföra bedömningsprofil resultaten för att avgöra vilken algoritm som vi vill använda i vår slutliga experimentet.</span><span class="sxs-lookup"><span data-stu-id="01579-113">In the experiment we're developing in this walkthrough, we'll create two different types of models and then compare their scoring results to decide which algorithm we want to use in our final experiment.</span></span>  

<span data-ttu-id="01579-114">Det finns olika modeller som vi kan välja från.</span><span class="sxs-lookup"><span data-stu-id="01579-114">There are various models we could choose from.</span></span> <span data-ttu-id="01579-115">För att se de tillgängliga modellerna, expandera den **Maskininlärning** nod på modulpaletten, och expandera sedan **initiera modell** och noderna under den.</span><span class="sxs-lookup"><span data-stu-id="01579-115">To see the models available, expand the **Machine Learning** node in the module palette, and then expand **Initialize Model** and the nodes beneath it.</span></span> <span data-ttu-id="01579-116">Vid tillämpningen av experimentet vi välja den [Two-Class Support Vector Machine] [ two-class-support-vector-machine] (SVM) och [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] moduler.</span><span class="sxs-lookup"><span data-stu-id="01579-116">For the purposes of this experiment, we'll select the [Two-Class Support Vector Machine][two-class-support-vector-machine] (SVM) and the [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] modules.</span></span>    

> [!TIP]
> <span data-ttu-id="01579-117">För att få hjälp för att bestämma vilken Machine Learning-algoritm som bäst passar viss problemet du försöker lösa, se [så väljer du algoritmer för Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).</span><span class="sxs-lookup"><span data-stu-id="01579-117">To get help deciding which Machine Learning algorithm best suits the particular problem you're trying to solve, see [How to choose algorithms for Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).</span></span>
> 
> 

## <a name="train-the-models"></a><span data-ttu-id="01579-118">Träna modeller</span><span class="sxs-lookup"><span data-stu-id="01579-118">Train the models</span></span>

<span data-ttu-id="01579-119">Vi lägger till både den [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] modulen och [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modul i experimentet.</span><span class="sxs-lookup"><span data-stu-id="01579-119">We'll add both the [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module and [Two-Class Support Vector Machine][two-class-support-vector-machine] module in this experiment.</span></span>

### <a name="two-class-boosted-decision-tree"></a><span data-ttu-id="01579-120">Two-Class Tvåklassförhöjda beslutsträdet</span><span class="sxs-lookup"><span data-stu-id="01579-120">Two-Class Boosted Decision Tree</span></span>

<span data-ttu-id="01579-121">Först ska vi ställa in förstärkta trädet modellen.</span><span class="sxs-lookup"><span data-stu-id="01579-121">First, let's set up the boosted decision tree model.</span></span>

1. <span data-ttu-id="01579-122">Hitta de [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] modulen på modulpaletten och drar den till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="01579-122">Find the [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module in the module palette and drag it onto the canvas.</span></span>

2. <span data-ttu-id="01579-123">Hitta de [Träningsmodell] [ train-model] modulen, drar den till arbetsytan och ansluter sedan utdata från den [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] modulen till vänster inkommande port för den [Träningsmodell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="01579-123">Find the [Train Model][train-model] module, drag it onto the canvas, and then connect the output of the [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module to the left input port of the [Train Model][train-model] module.</span></span>
   
   <span data-ttu-id="01579-124">Den [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] modulen initierar allmän modell och [träna modell] [ train-model] använder utbildningsdata för att träna på modell.</span><span class="sxs-lookup"><span data-stu-id="01579-124">The [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module initializes the generic model, and [Train Model][train-model] uses training data to train the model.</span></span> 

3. <span data-ttu-id="01579-125">Ansluta vänstra utdata från vänster [köra R-skriptet] [ execute-r-script] modulen till höger inkommande port för den [Träningsmodell] [ train-model] modul (vi valt i [Steg3](machine-learning-walkthrough-3-create-new-experiment.md) i den här genomgången för att använda data från vänster sida av modulen dela Data för utbildning).</span><span class="sxs-lookup"><span data-stu-id="01579-125">Connect the left output of the left [Execute R Script][execute-r-script] module to the right input port of the [Train Model][train-model] module (we decided in [Step 3](machine-learning-walkthrough-3-create-new-experiment.md) of this walkthrough to use the data coming from the left side of the Split Data module for training).</span></span>
   
   > [!TIP]
   > <span data-ttu-id="01579-126">Vi behöver inte två indata och en av utdata för den [köra R-skriptet] [ execute-r-script] modul för experimentet, så vi kan lämna dem.</span><span class="sxs-lookup"><span data-stu-id="01579-126">We don't need two of the inputs and one of the outputs of the [Execute R Script][execute-r-script] module for this experiment, so we can leave them unattached.</span></span> 
   > 
   > 

<span data-ttu-id="01579-127">Den här delen av experimentet nu ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="01579-127">This portion of the experiment now looks something like this:</span></span>  

![En modell][1]

<span data-ttu-id="01579-129">Nu behöver vi berätta det [Träningsmodell] [ train-model] modul som vi vill modellen för att förutsäga kreditrisken för värdet.</span><span class="sxs-lookup"><span data-stu-id="01579-129">Now we need to tell the [Train Model][train-model] module that we want the model to predict the Credit Risk value.</span></span>

1. <span data-ttu-id="01579-130">Välj den [Träningsmodell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="01579-130">Select the [Train Model][train-model] module.</span></span> <span data-ttu-id="01579-131">I den **egenskaper** rutan klickar du på **starta kolumnväljaren**.</span><span class="sxs-lookup"><span data-stu-id="01579-131">In the **Properties** pane, click **Launch column selector**.</span></span>

2. <span data-ttu-id="01579-132">I den **Markera en kolumn** dialogrutan Skriv ”kredit risk” i sökfältet under **tillgängliga kolumner**, markerar ”kredit risk” nedan och klicka på högerpilen ( **>** ) att flytta ”kreditrisk” till **valda kolumner**.</span><span class="sxs-lookup"><span data-stu-id="01579-132">In the **Select a single column** dialog, type "credit risk" in the search field under **Available Columns**, select "Credit risk" below, and click the right arrow button (**>**) to move "Credit risk" to **Selected Columns**.</span></span> 

    ![Välj kolumnen kreditrisken för träningsmodellmodulen][0]

3. <span data-ttu-id="01579-134">Klicka på den **OK** är markerat.</span><span class="sxs-lookup"><span data-stu-id="01579-134">Click the **OK** check mark.</span></span>

### <a name="two-class-support-vector-machine"></a><span data-ttu-id="01579-135">Tvåklassig dator för vektorstöd</span><span class="sxs-lookup"><span data-stu-id="01579-135">Two-Class Support Vector Machine</span></span>

<span data-ttu-id="01579-136">Nu ska ställa vi in SVM modellen.</span><span class="sxs-lookup"><span data-stu-id="01579-136">Next, we set up the SVM model.</span></span>  

<span data-ttu-id="01579-137">Första, en förklaring om SVM.</span><span class="sxs-lookup"><span data-stu-id="01579-137">First, a little explanation about SVM.</span></span> <span data-ttu-id="01579-138">Förstärkta beslutsträd fungerar bra med funktioner för alla typer.</span><span class="sxs-lookup"><span data-stu-id="01579-138">Boosted decision trees work well with features of any type.</span></span> <span data-ttu-id="01579-139">Men eftersom modulen SVM genererar en linjär klassificerare, har den modell som genereras bästa testfel när alla numeriska funktioner har samma skala.</span><span class="sxs-lookup"><span data-stu-id="01579-139">However, since the SVM module generates a linear classifier, the model that it generates has the best test error when all numeric features have the same scale.</span></span> <span data-ttu-id="01579-140">Om du vill konvertera alla numeriska funktioner samma skala vi använder en ”Tanh”-omvandling (med den [normalisera Data] [ normalize-data] module).</span><span class="sxs-lookup"><span data-stu-id="01579-140">To convert all numeric features to the same scale, we use a "Tanh" transformation (with the [Normalize Data][normalize-data] module).</span></span> <span data-ttu-id="01579-141">Detta omvandlar våra nummer i intervallet [0,1].</span><span class="sxs-lookup"><span data-stu-id="01579-141">This transforms our numbers into the [0,1] range.</span></span> <span data-ttu-id="01579-142">Modulen SVM konverterar sträng funktioner till kategoriska funktioner och sedan till binära 0-1-funktioner, så vi inte behöver manuellt Omforma strängen funktioner.</span><span class="sxs-lookup"><span data-stu-id="01579-142">The SVM module converts string features to categorical features and then to binary 0/1 features, so we don't need to manually transform string features.</span></span> <span data-ttu-id="01579-143">Dessutom vill vi inte transformera kreditrisk kolumnen (kolumn 21) - är det numeriska men det är det värde som vi tränar modellen för att förutsäga, så vi behöver inte använder den.</span><span class="sxs-lookup"><span data-stu-id="01579-143">Also, we don't want to transform the Credit Risk column (column 21) - it's numeric, but it's the value we're training the model to predict, so we need to leave it alone.</span></span>  

<span data-ttu-id="01579-144">Om du vill konfigurera SVM modellen gör du följande:</span><span class="sxs-lookup"><span data-stu-id="01579-144">To set up the SVM model, do the following:</span></span>

1. <span data-ttu-id="01579-145">Hitta de [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulen på modulpaletten och drar den till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="01579-145">Find the [Two-Class Support Vector Machine][two-class-support-vector-machine] module in the module palette and drag it onto the canvas.</span></span>

2. <span data-ttu-id="01579-146">Högerklicka på den [Träningsmodell] [ train-model] modulen, Välj **kopiera**, och högerklicka sedan på arbetsytan och välj **klistra in**.</span><span class="sxs-lookup"><span data-stu-id="01579-146">Right-click the [Train Model][train-model] module, select **Copy**, and then right-click the canvas and select **Paste**.</span></span> <span data-ttu-id="01579-147">Kopia av den [Träningsmodell] [ train-model] modulen har samma kolumn urval som originalet.</span><span class="sxs-lookup"><span data-stu-id="01579-147">The copy of the [Train Model][train-model] module has the same column selection as the original.</span></span>

3. <span data-ttu-id="01579-148">Ansluta utdata från den [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulen till vänster inkommande port för andra [Träningsmodell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="01579-148">Connect the output of the [Two-Class Support Vector Machine][two-class-support-vector-machine] module to the left input port of the second [Train Model][train-model] module.</span></span>

4. <span data-ttu-id="01579-149">Hitta de [normalisera Data] [ normalize-data] modulen och drar den till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="01579-149">Find the [Normalize Data][normalize-data] module and drag it onto the canvas.</span></span>

5. <span data-ttu-id="01579-150">Ansluta vänstra utdata från vänster [köra R-skriptet] [ execute-r-script] modulen till indata för den här modulen (Observera att utdataporten för en modul kan anslutas till mer än en modul).</span><span class="sxs-lookup"><span data-stu-id="01579-150">Connect the left output of the left [Execute R Script][execute-r-script] module to the input of this module (notice that the output port of a module may be connected to more than one other module).</span></span>

6. <span data-ttu-id="01579-151">Anslut den vänstra utdataporten för den [normalisera Data] [ normalize-data] modulen till höger inkommande port för andra [Träningsmodell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="01579-151">Connect the left output port of the [Normalize Data][normalize-data] module to the right input port of the second [Train Model][train-model] module.</span></span>

<span data-ttu-id="01579-152">Den här delen av vår experimentet bör nu se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="01579-152">This portion of our experiment should now look something like this:</span></span>  

![Andra modell][2]  

<span data-ttu-id="01579-154">Konfigurera nu den [normalisera Data] [ normalize-data] modulen:</span><span class="sxs-lookup"><span data-stu-id="01579-154">Now configure the [Normalize Data][normalize-data] module:</span></span>

1. <span data-ttu-id="01579-155">Markera den [normalisera Data] [ normalize-data] modul.</span><span class="sxs-lookup"><span data-stu-id="01579-155">Click to select the [Normalize Data][normalize-data] module.</span></span> <span data-ttu-id="01579-156">I den **egenskaper** väljer **Tanh** för den **omvandling metoden** parameter.</span><span class="sxs-lookup"><span data-stu-id="01579-156">In the **Properties** pane, select **Tanh** for the **Transformation method** parameter.</span></span>

2. <span data-ttu-id="01579-157">Klicka på **starta kolumnväljaren**, Välj ”inga kolumner” för **börjar med**väljer **inkludera** i den första listrutan väljer **kolumntypen** i den andra listrutan och välj **numeriska** i tredje listrutan.</span><span class="sxs-lookup"><span data-stu-id="01579-157">Click **Launch column selector**, select "No columns" for **Begin With**, select **Include** in the first dropdown, select **column type** in the second dropdown, and select **Numeric** in the third dropdown.</span></span> <span data-ttu-id="01579-158">Anger att alla numeriska kolumner (och endast numeriskt) omvandlas.</span><span class="sxs-lookup"><span data-stu-id="01579-158">This specifies that all the numeric columns (and only numeric) are transformed.</span></span>

3. <span data-ttu-id="01579-159">Klicka på plustecknet (+) till höger om den här raden - detta skapar en rad av nedrullningsbara listorna.</span><span class="sxs-lookup"><span data-stu-id="01579-159">Click the plus sign (+) to the right of this row - this creates a row of dropdowns.</span></span> <span data-ttu-id="01579-160">Välj **undanta** i den första listrutan väljer **kolumnnamn** i andra listrutan och ange ”kredit risk” i textfältet.</span><span class="sxs-lookup"><span data-stu-id="01579-160">Select **Exclude** in the first dropdown, select **column names** in the second dropdown, and enter "Credit risk" in the text field.</span></span> <span data-ttu-id="01579-161">Detta anger att kolumnen kreditrisk ska ignoreras (vi behöver göra detta eftersom den här kolumnen är numeriska och så skulle omvandlas om vi inte utesluter den).</span><span class="sxs-lookup"><span data-stu-id="01579-161">This specifies that the Credit Risk column should be ignored (we need to do this because this column is numeric and so would be transformed if we didn't exclude it).</span></span>

4. <span data-ttu-id="01579-162">Klicka på den **OK** är markerat.</span><span class="sxs-lookup"><span data-stu-id="01579-162">Click the **OK** check mark.</span></span>  

    ![Välj kolumner för modulen normalisera Data][5]

<span data-ttu-id="01579-164">Den [normalisera Data] [ normalize-data] modulen nu är inställd på att utföra en Tanh omvandling på alla numeriska kolumner utom den kreditrisk.</span><span class="sxs-lookup"><span data-stu-id="01579-164">The [Normalize Data][normalize-data] module is now set to perform a Tanh transformation on all numeric columns except for the Credit Risk column.</span></span>  

## <a name="score-and-evaluate-the-models"></a><span data-ttu-id="01579-165">Poängsätta och utvärdera modellerna</span><span class="sxs-lookup"><span data-stu-id="01579-165">Score and evaluate the models</span></span>

<span data-ttu-id="01579-166">Vi använder informationen tester som separeras av den [dela Data] [ split] modulen poängsätta våra tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="01579-166">We use the testing data that was separated out by the [Split Data][split] module to score our trained models.</span></span> <span data-ttu-id="01579-167">Vi kan sedan jämföra resultatet av de två modellerna Se som genererats bättre resultat.</span><span class="sxs-lookup"><span data-stu-id="01579-167">We can then compare the results of the two models to see which generated better results.</span></span>  

### <a name="add-the-score-model-modules"></a><span data-ttu-id="01579-168">Lägga till poängsätta modell-moduler</span><span class="sxs-lookup"><span data-stu-id="01579-168">Add the Score Model modules</span></span>

1. <span data-ttu-id="01579-169">Hitta de [Poängmodell] [ score-model] modulen och drar den till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="01579-169">Find the [Score Model][score-model] module and drag it onto the canvas.</span></span>

2. <span data-ttu-id="01579-170">Ansluta den [Träningsmodell] [ train-model] modul som är ansluten till den [två-Tvåklassförhöjt beslutsträd] [ two-class-boosted-decision-tree] modulen till vänster inkommande port för den [Poängmodell] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="01579-170">Connect the [Train Model][train-model] module that's connected to the [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module to the left input port of the [Score Model][score-model] module.</span></span>

3. <span data-ttu-id="01579-171">Ansluta till höger [köra R-skriptet] [ execute-r-script] modul (våra tester data) till höger inkommande port för den [Poängmodell] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="01579-171">Connect the right [Execute R Script][execute-r-script] module (our testing data) to the right input port of the [Score Model][score-model] module.</span></span>

    ![Ansluten att modulen poängsätta modell][6]
   
   <span data-ttu-id="01579-173">Den [Poängmodell] [ score-model] modul kan nu ta kredit information från tester data, kör via modellen och jämföra förutsägelser modellen genererar med den faktiska kredit risk kolumnen i den testa data.</span><span class="sxs-lookup"><span data-stu-id="01579-173">The [Score Model][score-model] module can now take the credit information from the testing data, run it through the model, and compare the predictions the model generates with the actual credit risk column in the testing data.</span></span>

4. <span data-ttu-id="01579-174">Kopiera och klistra in den [Poängmodell] [ score-model] modul för att skapa en andra kopia.</span><span class="sxs-lookup"><span data-stu-id="01579-174">Copy and paste the [Score Model][score-model] module to create a second copy.</span></span>

5. <span data-ttu-id="01579-175">Ansluta utdata från SVM modellen (det vill säga utdataporten för den [Träningsmodell] [ train-model] modul som är ansluten till den [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulen) till den inkommande porten för andra [Poängmodell] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="01579-175">Connect the output of the SVM model (that is, the output port of the [Train Model][train-model] module that's connected to the [Two-Class Support Vector Machine][two-class-support-vector-machine] module) to the input port of the second [Score Model][score-model] module.</span></span>

6. <span data-ttu-id="01579-176">Vi har utför samma transformeringen till testdata som vi gjorde för utbildning-data för SVM-modellen.</span><span class="sxs-lookup"><span data-stu-id="01579-176">For the SVM model, we have to do the same transformation to the test data as we did to the training data.</span></span> <span data-ttu-id="01579-177">Så kopiera och klistra in den [normalisera Data] [ normalize-data] modul för att skapa en andra kopia och ansluta till höger [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="01579-177">So copy and paste the [Normalize Data][normalize-data] module to create a second copy and connect it to the right [Execute R Script][execute-r-script] module.</span></span>

7. <span data-ttu-id="01579-178">Ansluta vänstra utdata från andra [normalisera Data] [ normalize-data] modulen till höger inkommande port för andra [Poängmodell] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="01579-178">Connect the left output of the second [Normalize Data][normalize-data] module to the right input port of the second [Score Model][score-model] module.</span></span>

    ![Båda poängsätta modell-moduler som är ansluten][7]

### <a name="add-the-evaluate-model-module"></a><span data-ttu-id="01579-180">Lägg till modulen utvärdera modell</span><span class="sxs-lookup"><span data-stu-id="01579-180">Add the Evaluate Model module</span></span>

<span data-ttu-id="01579-181">Om du vill utvärdera bedömningsprofil resultaten och jämför dem, använder vi en [utvärdera modell] [ evaluate-model] modul.</span><span class="sxs-lookup"><span data-stu-id="01579-181">To evaluate the two scoring results and compare them, we use an [Evaluate Model][evaluate-model] module.</span></span>  

1. <span data-ttu-id="01579-182">Hitta de [utvärdera modell] [ evaluate-model] modulen och drar den till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="01579-182">Find the [Evaluate Model][evaluate-model] module and drag it onto the canvas.</span></span>

2. <span data-ttu-id="01579-183">Ansluta utdataporten för den [Poängmodell] [ score-model] modul som är associerade med den förstärkta träd modellen till den vänstra indataporten av den [utvärdera modell] [ evaluate-model] modul.</span><span class="sxs-lookup"><span data-stu-id="01579-183">Connect the output port of the [Score Model][score-model] module associated with the boosted decision tree model to the left input port of the [Evaluate Model][evaluate-model] module.</span></span>

3. <span data-ttu-id="01579-184">Anslut den andra [Poängmodell] [ score-model] modulen till höger inkommande port.</span><span class="sxs-lookup"><span data-stu-id="01579-184">Connect the other [Score Model][score-model] module to the right input port.</span></span>  

    ![Utvärdera modellen modulen ansluten][8]

### <a name="run-the-experiment-and-check-the-results"></a><span data-ttu-id="01579-186">Kör experimentet och kontrollera resultaten</span><span class="sxs-lookup"><span data-stu-id="01579-186">Run the experiment and check the results</span></span>

<span data-ttu-id="01579-187">Kör experimentet genom att klicka på den **kör** knapp under arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="01579-187">To run the experiment, click the **RUN** button below the canvas.</span></span> <span data-ttu-id="01579-188">Det kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="01579-188">It may take a few minutes.</span></span> <span data-ttu-id="01579-189">En snurrande indikator för varje modul visar att den körs, och sedan en grön bock visas när modulen är klar.</span><span class="sxs-lookup"><span data-stu-id="01579-189">A spinning indicator on each module shows that it's running, and then a green check mark shows when the module is finished.</span></span> <span data-ttu-id="01579-190">När alla moduler är markerat har experimentet slutförts.</span><span class="sxs-lookup"><span data-stu-id="01579-190">When all the modules have a check mark, the experiment has finished running.</span></span>

<span data-ttu-id="01579-191">Experimentet bör nu se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="01579-191">The experiment should now look something like this:</span></span>  

![Utvärdering av båda modellerna][3]

<span data-ttu-id="01579-193">Kontrollera resultaten, klicka på utdataporten för den [utvärdera modell] [ evaluate-model] modulen och välj **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="01579-193">To check the results, click the output port of the [Evaluate Model][evaluate-model] module and select **Visualize**.</span></span>  

<span data-ttu-id="01579-194">Den [utvärdera modell] [ evaluate-model] modulen genererar ett par med kurvor och mått som gör det möjligt att jämföra resultatet av de två poängsatta modellerna.</span><span class="sxs-lookup"><span data-stu-id="01579-194">The [Evaluate Model][evaluate-model] module produces a pair of curves and metrics that allow you to compare the results of the two scored models.</span></span> <span data-ttu-id="01579-195">Du kan visa resultat som mottagaren operatorn egenskap (ROC) kurvor, Precision/återkalla kurvor eller Lift kurvor.</span><span class="sxs-lookup"><span data-stu-id="01579-195">You can view the results as Receiver Operator Characteristic (ROC) curves, Precision/Recall curves, or Lift curves.</span></span> <span data-ttu-id="01579-196">Ytterligare data som visas innehåller en förvirring matris kumulativa värden för området under kurvan (AUC) och andra mått.</span><span class="sxs-lookup"><span data-stu-id="01579-196">Additional data displayed includes a confusion matrix, cumulative values for the area under the curve (AUC), and other metrics.</span></span> <span data-ttu-id="01579-197">Du kan ändra tröskelvärdet genom att flytta skjutreglaget åt vänster eller höger och se hur den påverkar uppsättningen mått.</span><span class="sxs-lookup"><span data-stu-id="01579-197">You can change the threshold value by moving the slider left or right and see how it affects the set of metrics.</span></span>  

<span data-ttu-id="01579-198">Klicka till höger om diagrammet **bedömas dataset** eller **bedömas dataset för att jämföra** Markera associerade kurvan och visa den associerade måtten nedan.</span><span class="sxs-lookup"><span data-stu-id="01579-198">To the right of the graph, click **Scored dataset** or **Scored dataset to compare** to highlight the associated curve and to display the associated metrics below.</span></span> <span data-ttu-id="01579-199">I förklaringen för kurvorna ”bedömas dataset” motsvarar den vänstra indataporten av den [utvärdera modell] [ evaluate-model] modul - i vårt fall är detta förstärkta trädet modellen.</span><span class="sxs-lookup"><span data-stu-id="01579-199">In the legend for the curves, "Scored dataset" corresponds to the left input port of the [Evaluate Model][evaluate-model] module - in our case, this is the boosted decision tree model.</span></span> <span data-ttu-id="01579-200">”Bedömas dataset för att jämföra” motsvarar den högra indataporten - SVM modellen i vårt fall.</span><span class="sxs-lookup"><span data-stu-id="01579-200">"Scored dataset to compare" corresponds to the right input port - the SVM model in our case.</span></span> <span data-ttu-id="01579-201">När du klickar på en av etiketterna kurvan för den modellen som är markerad och motsvarande mått visas som visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="01579-201">When you click one of these labels, the curve for that model is highlighted and the corresponding metrics are displayed, as shown in the following graphic.</span></span>  

![ROC kurvor för modeller][4]

<span data-ttu-id="01579-203">Du kan välja vilken modell som ligger närmast ger dig det resultat som du letar efter genom att undersöka dessa värden.</span><span class="sxs-lookup"><span data-stu-id="01579-203">By examining these values, you can decide which model is closest to giving you the results you're looking for.</span></span> <span data-ttu-id="01579-204">Du kan gå tillbaka och iterera experimentet genom att ändra parametervärden i olika modeller.</span><span class="sxs-lookup"><span data-stu-id="01579-204">You can go back and iterate on your experiment by changing parameter values in the different models.</span></span> 

<span data-ttu-id="01579-205">Vetenskap och bilder av tolka resultaten och justera prestanda för modellen ligger utanför omfånget för den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="01579-205">The science and art of interpreting these results and tuning the model performance is outside the scope of this walkthrough.</span></span> <span data-ttu-id="01579-206">För ytterligare hjälp kan du läsa följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="01579-206">For additional help, you might read the following articles:</span></span>
- [<span data-ttu-id="01579-207">Hur du utvärdera modellen prestanda i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="01579-207">How to evaluate model performance in Azure Machine Learning</span></span>](machine-learning-evaluate-model-performance.md)
- [<span data-ttu-id="01579-208">Välj parametrar för att optimera algoritmerna i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="01579-208">Choose parameters to optimize your algorithms in Azure Machine Learning</span></span>](machine-learning-algorithm-parameters-optimize.md)
- [<span data-ttu-id="01579-209">Tolka modellen resultaten i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="01579-209">Interpret model results in Azure Machine Learning</span></span>](machine-learning-interpret-model-results.md)

> [!TIP]
> <span data-ttu-id="01579-210">Varje gång du kör experimentet en post på den iterationen sparas i historiken kör.</span><span class="sxs-lookup"><span data-stu-id="01579-210">Each time you run the experiment a record of that iteration is kept in the Run History.</span></span> <span data-ttu-id="01579-211">Du kan visa dessa iterationer och återgå till någon av dem, genom att klicka på **visa KÖRNINGSHISTORIK** under arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="01579-211">You can view these iterations, and return to any of them, by clicking **VIEW RUN HISTORY** below the canvas.</span></span> <span data-ttu-id="01579-212">Du kan också klicka på **tidigare kör** i den **egenskaper** rutan Gå tillbaka till iteration omedelbart före den som du har öppnat.</span><span class="sxs-lookup"><span data-stu-id="01579-212">You can also click **Prior Run** in the **Properties** pane to return to the iteration immediately preceding the one you have open.</span></span>
> 
> <span data-ttu-id="01579-213">Du kan göra en kopia av eventuella iterationer av experimentet genom att klicka på **Spara som** under arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="01579-213">You can make a copy of any iteration of your experiment by clicking **SAVE AS** below the canvas.</span></span> 
> <span data-ttu-id="01579-214">Använda arbetsytan för experimentet **sammanfattning** och **beskrivning** egenskaper för att hålla reda på vad du har gjort i iterationer av experiment.</span><span class="sxs-lookup"><span data-stu-id="01579-214">Use the experiment's **Summary** and **Description** properties to keep a record of what you've tried in your experiment iterations.</span></span>
> 
> <span data-ttu-id="01579-215">Mer information finns i [hantera iterationer av experiment i Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md).</span><span class="sxs-lookup"><span data-stu-id="01579-215">For more details, see [Manage experiment iterations in Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md).</span></span>  
> 
> 

- - -
<span data-ttu-id="01579-216">**Nästa: [distribuera webbtjänsten](machine-learning-walkthrough-5-publish-web-service.md)**</span><span class="sxs-lookup"><span data-stu-id="01579-216">**Next: [Deploy the web service](machine-learning-walkthrough-5-publish-web-service.md)**</span></span>

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
