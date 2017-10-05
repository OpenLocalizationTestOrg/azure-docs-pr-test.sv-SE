---
title: Analysera data med Azure Machine Learning | Microsoft Docs
description: "Använd Azure Machine Learning för att skapa en förutsägbar Machine Learning-modell som baseras på data lagrade i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 95635460-150f-4a50-be9c-5ddc5797f8a9
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 03/02/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 3197948e32fe5c95b111fe5495a0e5f85966a24b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-data-with-azure-machine-learning"></a><span data-ttu-id="36b93-103">Analysera data med Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="36b93-103">Analyze data with Azure Machine Learning</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="36b93-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="36b93-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="36b93-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="36b93-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="36b93-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="36b93-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="36b93-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="36b93-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="36b93-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="36b93-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="36b93-109">Denna självstudie använder Azure Machine Learning för att skapa en förutsägbar Machine Learning-modell som baseras på data lagrade i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="36b93-109">This tutorial uses Azure Machine Learning to build a predictive machine learning model based on data stored in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="36b93-110">Mer specifikt skapar detta en riktad marknadsföringskampanj för Adventure Works, en cykelbutik, genom att förutsäga hur sannolikt det är att en kund kommer att köpa en cykel.</span><span class="sxs-lookup"><span data-stu-id="36b93-110">Specifically, this builds a targeted marketing campaign for Adventure Works, the bike shop, by predicting if a customer is likely to buy a bike or not.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Integrating-Azure-Machine-Learning-with-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="36b93-111">Krav</span><span class="sxs-lookup"><span data-stu-id="36b93-111">Prerequisites</span></span>
<span data-ttu-id="36b93-112">För att gå igenom de här självstudierna, behöver du:</span><span class="sxs-lookup"><span data-stu-id="36b93-112">To step through this tutorial, you need:</span></span>

* <span data-ttu-id="36b93-113">Ett SQL Data Warehouse förinstallerat med AdventureWorksDW som exempeldatabas.</span><span class="sxs-lookup"><span data-stu-id="36b93-113">A SQL Data Warehouse pre-loaded with AdventureWorksDW sample data.</span></span> <span data-ttu-id="36b93-114">Om du vill distribuera detta läser du [Skapa ett SQL Data Warehouse][Create a SQL Data Warehouse] och väljer att läsa in exempeldata.</span><span class="sxs-lookup"><span data-stu-id="36b93-114">To provision this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse] and choose to load the sample data.</span></span> <span data-ttu-id="36b93-115">Om du redan har ett Data Warehouse men inte har exempeldata kan du [läsa in exempeldata manuellt][load sample data manually].</span><span class="sxs-lookup"><span data-stu-id="36b93-115">If you already have a data warehouse but do not have sample data, you can [load sample data manually][load sample data manually].</span></span>

## <a name="1-get-the-data"></a><span data-ttu-id="36b93-116">1. Hämta data</span><span class="sxs-lookup"><span data-stu-id="36b93-116">1. Get the data</span></span>
<span data-ttu-id="36b93-117">Aktuella data finns i dbo.vTargetMail-vyn i AdventureWorksDW-databasen.</span><span class="sxs-lookup"><span data-stu-id="36b93-117">The data is in the dbo.vTargetMail view in the AdventureWorksDW database.</span></span> <span data-ttu-id="36b93-118">Så här läser du in dessa data:</span><span class="sxs-lookup"><span data-stu-id="36b93-118">To read this data:</span></span>

1. <span data-ttu-id="36b93-119">Logga in i [Azure Machine Learning Studio][Azure Machine Learning studio] och klicka på My experiments (Mina experiment).</span><span class="sxs-lookup"><span data-stu-id="36b93-119">Sign into [Azure Machine Learning studio][Azure Machine Learning studio] and click on my experiments.</span></span>
2. <span data-ttu-id="36b93-120">Klicka på **+NY** och välj **Tomt experiment**.</span><span class="sxs-lookup"><span data-stu-id="36b93-120">Click **+NEW** and select **Blank Experiment**.</span></span>
3. <span data-ttu-id="36b93-121">Ange ett namn för experimentet: Riktad marknadsföring.</span><span class="sxs-lookup"><span data-stu-id="36b93-121">Enter a name for your experiment: Targeted Marketing.</span></span>
4. <span data-ttu-id="36b93-122">Dra modulen **Läsare** från modulfönstret till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="36b93-122">Drag the **Reader** module from the modules pane into the canvas.</span></span>
5. <span data-ttu-id="36b93-123">Ange information om din SQL Data Warehouse-databas i fönstret Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="36b93-123">Specify the details of your SQL Data Warehouse database in the Properties pane.</span></span>
6. <span data-ttu-id="36b93-124">Ange **fråga** för databasen för att läsa data av intresse.</span><span class="sxs-lookup"><span data-stu-id="36b93-124">Specify the database **query** to read the data of interest.</span></span>

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

<span data-ttu-id="36b93-125">Kör försöket genom att klicka på **Kör** under experimentets arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="36b93-125">Run the experiment by clicking **Run** under the experiment canvas.</span></span>
<span data-ttu-id="36b93-126">![Kör experimentet][1]</span><span class="sxs-lookup"><span data-stu-id="36b93-126">![Run the experiment][1]</span></span>

<span data-ttu-id="36b93-127">När experimentet har körts, klicka på utdataporten längst ned i läsarmodulen och välj **Visualisera** för att se data som importerats.</span><span class="sxs-lookup"><span data-stu-id="36b93-127">After the experiment finishes running successfully, click the output port at the bottom of the Reader module and select **Visualize** to see the imported data.</span></span>
<span data-ttu-id="36b93-128">![Visa data som importerats][3]</span><span class="sxs-lookup"><span data-stu-id="36b93-128">![View imported data][3]</span></span>

## <a name="2-clean-the-data"></a><span data-ttu-id="36b93-129">2. Rensa data</span><span class="sxs-lookup"><span data-stu-id="36b93-129">2. Clean the data</span></span>
<span data-ttu-id="36b93-130">För att rensa data kommer vi att släppa vissa kolumner som inte är relevanta för modellen.</span><span class="sxs-lookup"><span data-stu-id="36b93-130">To clean the data, drop some columns that are not relevant for the model.</span></span> <span data-ttu-id="36b93-131">Gör så här:</span><span class="sxs-lookup"><span data-stu-id="36b93-131">To do this:</span></span>

1. <span data-ttu-id="36b93-132">Dra modulen **Projektkolumner** till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="36b93-132">Drag the **Project Columns** module into the canvas.</span></span>
2. <span data-ttu-id="36b93-133">Klicka på **Starta kolumnväljaren** i fönstret Egenskaper för att ange vilka kolumner du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="36b93-133">Click **Launch column selector** in the Properties pane to specify which columns you wish to drop.</span></span>
   <span data-ttu-id="36b93-134">![Projektkolumner][4]</span><span class="sxs-lookup"><span data-stu-id="36b93-134">![Project Columns][4]</span></span>
3. <span data-ttu-id="36b93-135">Exkludera två kolumner: CustomerAlternateKey och GeographyKey.</span><span class="sxs-lookup"><span data-stu-id="36b93-135">Exclude two columns: CustomerAlternateKey and GeographyKey.</span></span>
   <span data-ttu-id="36b93-136">![Ta bort onödiga kolumner][5]</span><span class="sxs-lookup"><span data-stu-id="36b93-136">![Remove unnecessary columns][5]</span></span>

## <a name="3-build-the-model"></a><span data-ttu-id="36b93-137">3. Skapa modellen</span><span class="sxs-lookup"><span data-stu-id="36b93-137">3. Build the model</span></span>
<span data-ttu-id="36b93-138">Vi delar data 80–20: 80 % för att träna en maskininlärningsmodell och 20 % för att testa modellen.</span><span class="sxs-lookup"><span data-stu-id="36b93-138">We will split the data 80-20: 80% to train a machine learning model and 20% to test the model.</span></span> <span data-ttu-id="36b93-139">Vi använder ”Tvåklassalgoritmer” för detta binära klassificeringsproblem.</span><span class="sxs-lookup"><span data-stu-id="36b93-139">We will make use of the “Two-Class” algorithms for this binary classification problem.</span></span>

1. <span data-ttu-id="36b93-140">Dra modulen **Dela** till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="36b93-140">Drag the **Split** module into the canvas.</span></span>
2. <span data-ttu-id="36b93-141">Ange 0,8 för andel av rader i den första utdatamängden i fönstret Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="36b93-141">Enter 0.8 for Fraction of rows in the first output dataset in the Properties pane.</span></span>
   <span data-ttu-id="36b93-142">![Dela data till uppsättningar för träning och testning][6]</span><span class="sxs-lookup"><span data-stu-id="36b93-142">![Split data into training and test set][6]</span></span>
3. <span data-ttu-id="36b93-143">Dra modulen **tvåklassförhöjt beslutsträd** till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="36b93-143">Drag the **Two-Class Boosted Decision Tree** module into the canvas.</span></span>
4. <span data-ttu-id="36b93-144">Dra modulen **Träningsmodell** till arbetsytan och ange indata.</span><span class="sxs-lookup"><span data-stu-id="36b93-144">Drag the **Train Model** module into the canvas and specify the inputs.</span></span> <span data-ttu-id="36b93-145">Klicka sedan på **Starta kolumnväljaren** i fönstret Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="36b93-145">Then, click **Launch column selector** in the Properties pane.</span></span>
   * <span data-ttu-id="36b93-146">Första indata: ML-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="36b93-146">First input: ML algorithm.</span></span>
   * <span data-ttu-id="36b93-147">Andra indata: Data att träna algoritmen på.</span><span class="sxs-lookup"><span data-stu-id="36b93-147">Second input: Data to train the algorithm on.</span></span>
     <span data-ttu-id="36b93-148">![Anslut träningsmodellmodulen][7]</span><span class="sxs-lookup"><span data-stu-id="36b93-148">![Connect the Train Model module][7]</span></span>
5. <span data-ttu-id="36b93-149">Välj kolumnen **BikeBuyer** som kolumn att förutsäga.</span><span class="sxs-lookup"><span data-stu-id="36b93-149">Select the **BikeBuyer** column as the column to predict.</span></span>
   <span data-ttu-id="36b93-150">![Välj kolumn att förutsäga][8]</span><span class="sxs-lookup"><span data-stu-id="36b93-150">![Select Column to predict][8]</span></span>

## <a name="4-score-the-model"></a><span data-ttu-id="36b93-151">4. Poängsätt modellen</span><span class="sxs-lookup"><span data-stu-id="36b93-151">4. Score the model</span></span>
<span data-ttu-id="36b93-152">Vi kommer nu att testa hur modellen presterar på testdata.</span><span class="sxs-lookup"><span data-stu-id="36b93-152">Now, we will test how the model performs on test data.</span></span> <span data-ttu-id="36b93-153">Vi kommer att jämföra algoritmen vi valt med en annan algoritm för att se vilken som presterar bäst.</span><span class="sxs-lookup"><span data-stu-id="36b93-153">We will compare the algorithm of our choice with a different algorithm to see which performs better.</span></span>

1. <span data-ttu-id="36b93-154">Dra modulen **Poängmodell** till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="36b93-154">Drag **Score Model** module into the canvas.</span></span>
    <span data-ttu-id="36b93-155">Första indata: tränad modell, andra indata: testdata ![poängsätt modellen][9]</span><span class="sxs-lookup"><span data-stu-id="36b93-155">First input: Trained model Second input: Test data ![Score the model][9]</span></span>
2. <span data-ttu-id="36b93-156">Dra **Tvåklass, Bayes Point-dator** till arbetsytan för experimentet.</span><span class="sxs-lookup"><span data-stu-id="36b93-156">Drag the **Two-Class Bayes Point Machine** into the experiment canvas.</span></span> <span data-ttu-id="36b93-157">Vi kommer att jämföra hur den här algoritmen presterar i jämförelse med det tvåklassförhöjda beslutsträdet.</span><span class="sxs-lookup"><span data-stu-id="36b93-157">We will compare how this algorithm performs in comparison to the Two-Class Boosted Decision Tree.</span></span>
3. <span data-ttu-id="36b93-158">Kopiera och klistra in modulerna Träningsmodell och Poängmodell i arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="36b93-158">Copy and Paste the modules Train Model and Score Model in the canvas.</span></span>
4. <span data-ttu-id="36b93-159">Dra modulen **Utvärdera modell** till arbetsytan för att jämföra de två algoritmerna.</span><span class="sxs-lookup"><span data-stu-id="36b93-159">Drag the **Evaluate Model** module into the canvas to compare the two algorithms.</span></span>
5. <span data-ttu-id="36b93-160">**Kör** experimentet.</span><span class="sxs-lookup"><span data-stu-id="36b93-160">**Run** the experiment.</span></span>
   <span data-ttu-id="36b93-161">![Kör experimentet][10]</span><span class="sxs-lookup"><span data-stu-id="36b93-161">![Run the experiment][10]</span></span>
6. <span data-ttu-id="36b93-162">Klicka på utdataporten längst ned i modulen Utvärdera modell och klicka på Visualisera.</span><span class="sxs-lookup"><span data-stu-id="36b93-162">Click the output port at the bottom of the Evaluate Model module and click Visualize.</span></span>
   <span data-ttu-id="36b93-163">![Visualisera utvärderingsresultaten][11]</span><span class="sxs-lookup"><span data-stu-id="36b93-163">![Visualize evaluation results][11]</span></span>

<span data-ttu-id="36b93-164">Mätvärdena är ROC-kurvan, precisionsåterkallningsdiagram och lyftkurvan.</span><span class="sxs-lookup"><span data-stu-id="36b93-164">The metrics provided are the ROC curve, precision-recall diagram and lift curve.</span></span> <span data-ttu-id="36b93-165">Genom att titta på de här måtten kan vi se att den första modellen presterade bättre än den andra.</span><span class="sxs-lookup"><span data-stu-id="36b93-165">Looking at these metrics, we can see that the first model performed better than the second one.</span></span> <span data-ttu-id="36b93-166">För att titta på hur den första modellen förutsade, klicka på utdataporten i Poängmodell och klicka på Visualisera.</span><span class="sxs-lookup"><span data-stu-id="36b93-166">To look at the what the first model predicted, click on output port of the Score Model and click Visualize.</span></span>
<span data-ttu-id="36b93-167">![Visualisera poängresultat][12]</span><span class="sxs-lookup"><span data-stu-id="36b93-167">![Visualize score results][12]</span></span>

<span data-ttu-id="36b93-168">Du ser två kolumner som läggs till i testdata.</span><span class="sxs-lookup"><span data-stu-id="36b93-168">You will see two more columns added to your test dataset.</span></span>

* <span data-ttu-id="36b93-169">Poängsatt sannolikhet: sannolikheten att en kund är en cykelköpare.</span><span class="sxs-lookup"><span data-stu-id="36b93-169">Scored Probabilities: the likelihood that a customer is a bike buyer.</span></span>
* <span data-ttu-id="36b93-170">Poängsatta etiketter: klassificering utförd av modellen – cykelköpare (1) eller inte (0).</span><span class="sxs-lookup"><span data-stu-id="36b93-170">Scored Labels: the classification done by the model – bike buyer (1) or not (0).</span></span> <span data-ttu-id="36b93-171">Det här sannolikhetströskelvärdet för etikettering anges till 50 % och kan justeras.</span><span class="sxs-lookup"><span data-stu-id="36b93-171">This probability threshold for labeling is set to 50% and can be adjusted.</span></span>

<span data-ttu-id="36b93-172">Genom att jämföra kolumnen BikeBuyer (faktiska) med Poängsatta etiketter (förutsagda), kan du se hur väl modellen har utförts.</span><span class="sxs-lookup"><span data-stu-id="36b93-172">Comparing the column BikeBuyer (actual) with the Scored Labels (prediction), you can see how well the model has performed.</span></span> <span data-ttu-id="36b93-173">Du kan använda den här modellen som nästa steg för att göra förutsägelser för nya kunder och publicera den här modellen som en webbtjänst eller skriva resultaten tillbaka till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="36b93-173">As next steps, you can use this model to make predictions for new customers and publish this model as a web service or write results back to SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="36b93-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="36b93-174">Next steps</span></span>
<span data-ttu-id="36b93-175">Mer information om hur du skapar förutsägbara maskininlärningsmodeller finns i [Introduktion till Machine Learning i Azure][Introduction to Machine Learning on Azure].</span><span class="sxs-lookup"><span data-stu-id="36b93-175">To learn more about building predictive machine learning models, refer to [Introduction to Machine Learning on Azure][Introduction to Machine Learning on Azure].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning studio]:https://studio.azureml.net/
[Introduction to Machine Learning on Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
