---
title: aaaAnalyze data med Azure Machine Learning | Microsoft Docs
description: "Använd Azure Machine Learning toobuild förutsägande machine learning-modell baserad på data som lagras i Azure SQL Data Warehouse."
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
ms.openlocfilehash: 337a2cd77aaad4467683827c56e5015b262b2554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-data-with-azure-machine-learning"></a><span data-ttu-id="899ce-103">Analysera data med Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="899ce-103">Analyze data with Azure Machine Learning</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="899ce-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="899ce-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="899ce-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="899ce-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="899ce-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="899ce-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="899ce-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="899ce-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="899ce-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="899ce-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="899ce-109">Den här kursen används Azure Machine Learning toobuild förutsägande machine learning-modell baserad på data som lagras i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="899ce-109">This tutorial uses Azure Machine Learning toobuild a predictive machine learning model based on data stored in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="899ce-110">Mer specifikt kan detta skapar en riktad marknadsföringskampanj för Adventure Works, hello cykelbutik, genom att förutsäga om en kund är sannolikt toobuy en cykel eller inte.</span><span class="sxs-lookup"><span data-stu-id="899ce-110">Specifically, this builds a targeted marketing campaign for Adventure Works, hello bike shop, by predicting if a customer is likely toobuy a bike or not.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Integrating-Azure-Machine-Learning-with-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="899ce-111">Krav</span><span class="sxs-lookup"><span data-stu-id="899ce-111">Prerequisites</span></span>
<span data-ttu-id="899ce-112">toostep igenom den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="899ce-112">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="899ce-113">Ett SQL Data Warehouse förinstallerat med AdventureWorksDW som exempeldatabas.</span><span class="sxs-lookup"><span data-stu-id="899ce-113">A SQL Data Warehouse pre-loaded with AdventureWorksDW sample data.</span></span> <span data-ttu-id="899ce-114">tooprovision detta, se [skapa ett SQL Data Warehouse] [ Create a SQL Data Warehouse] och välj tooload hello exempeldata.</span><span class="sxs-lookup"><span data-stu-id="899ce-114">tooprovision this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse] and choose tooload hello sample data.</span></span> <span data-ttu-id="899ce-115">Om du redan har ett Data Warehouse men inte har exempeldata kan du [läsa in exempeldata manuellt][load sample data manually].</span><span class="sxs-lookup"><span data-stu-id="899ce-115">If you already have a data warehouse but do not have sample data, you can [load sample data manually][load sample data manually].</span></span>

## <a name="1-get-hello-data"></a><span data-ttu-id="899ce-116">1. Hämta hello data</span><span class="sxs-lookup"><span data-stu-id="899ce-116">1. Get hello data</span></span>
<span data-ttu-id="899ce-117">hello data har hello dbo.vTargetMail-vyn i hello AdventureWorksDW-databasen.</span><span class="sxs-lookup"><span data-stu-id="899ce-117">hello data is in hello dbo.vTargetMail view in hello AdventureWorksDW database.</span></span> <span data-ttu-id="899ce-118">tooread informationen:</span><span class="sxs-lookup"><span data-stu-id="899ce-118">tooread this data:</span></span>

1. <span data-ttu-id="899ce-119">Logga in i [Azure Machine Learning Studio][Azure Machine Learning studio] och klicka på My experiments (Mina experiment).</span><span class="sxs-lookup"><span data-stu-id="899ce-119">Sign into [Azure Machine Learning studio][Azure Machine Learning studio] and click on my experiments.</span></span>
2. <span data-ttu-id="899ce-120">Klicka på **+NY** och välj **Tomt experiment**.</span><span class="sxs-lookup"><span data-stu-id="899ce-120">Click **+NEW** and select **Blank Experiment**.</span></span>
3. <span data-ttu-id="899ce-121">Ange ett namn för experimentet: Riktad marknadsföring.</span><span class="sxs-lookup"><span data-stu-id="899ce-121">Enter a name for your experiment: Targeted Marketing.</span></span>
4. <span data-ttu-id="899ce-122">Dra hello **Reader** modul från hello modulfönstret till arbetsytan hello.</span><span class="sxs-lookup"><span data-stu-id="899ce-122">Drag hello **Reader** module from hello modules pane into hello canvas.</span></span>
5. <span data-ttu-id="899ce-123">Ange hello information för din SQL Data Warehouse-databas i hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="899ce-123">Specify hello details of your SQL Data Warehouse database in hello Properties pane.</span></span>
6. <span data-ttu-id="899ce-124">Ange hello databas **frågan** tooread hello data av intresse.</span><span class="sxs-lookup"><span data-stu-id="899ce-124">Specify hello database **query** tooread hello data of interest.</span></span>

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

<span data-ttu-id="899ce-125">Kör hello experiment genom att klicka på **kör** under hello experimentet.</span><span class="sxs-lookup"><span data-stu-id="899ce-125">Run hello experiment by clicking **Run** under hello experiment canvas.</span></span>
<span data-ttu-id="899ce-126">![Kör hello experiment][1]</span><span class="sxs-lookup"><span data-stu-id="899ce-126">![Run hello experiment][1]</span></span>

<span data-ttu-id="899ce-127">När hello experimentet har körts, klicka på hello utdataporten längst ned hello hello Reader modulen och välj **visualisera** toosee hello importerat data.</span><span class="sxs-lookup"><span data-stu-id="899ce-127">After hello experiment finishes running successfully, click hello output port at hello bottom of hello Reader module and select **Visualize** toosee hello imported data.</span></span>
<span data-ttu-id="899ce-128">![Visa data som importerats][3]</span><span class="sxs-lookup"><span data-stu-id="899ce-128">![View imported data][3]</span></span>

## <a name="2-clean-hello-data"></a><span data-ttu-id="899ce-129">2. Rensa hello data</span><span class="sxs-lookup"><span data-stu-id="899ce-129">2. Clean hello data</span></span>
<span data-ttu-id="899ce-130">tooclean hello data, släppa vissa kolumner som inte är relevanta för hello modellen.</span><span class="sxs-lookup"><span data-stu-id="899ce-130">tooclean hello data, drop some columns that are not relevant for hello model.</span></span> <span data-ttu-id="899ce-131">toodo detta:</span><span class="sxs-lookup"><span data-stu-id="899ce-131">toodo this:</span></span>

1. <span data-ttu-id="899ce-132">Dra hello **Projektkolumner** modulen hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="899ce-132">Drag hello **Project Columns** module into hello canvas.</span></span>
2. <span data-ttu-id="899ce-133">Klicka på **starta kolumnväljaren** i hello egenskaper fönstret toospecify vilka kolumner du vill toodrop.</span><span class="sxs-lookup"><span data-stu-id="899ce-133">Click **Launch column selector** in hello Properties pane toospecify which columns you wish toodrop.</span></span>
   <span data-ttu-id="899ce-134">![Projektkolumner][4]</span><span class="sxs-lookup"><span data-stu-id="899ce-134">![Project Columns][4]</span></span>
3. <span data-ttu-id="899ce-135">Exkludera två kolumner: CustomerAlternateKey och GeographyKey.</span><span class="sxs-lookup"><span data-stu-id="899ce-135">Exclude two columns: CustomerAlternateKey and GeographyKey.</span></span>
   <span data-ttu-id="899ce-136">![Ta bort onödiga kolumner][5]</span><span class="sxs-lookup"><span data-stu-id="899ce-136">![Remove unnecessary columns][5]</span></span>

## <a name="3-build-hello-model"></a><span data-ttu-id="899ce-137">3. Skapa hello modell</span><span class="sxs-lookup"><span data-stu-id="899ce-137">3. Build hello model</span></span>
<span data-ttu-id="899ce-138">Vi delar upp hello data 80-20: 80% tootrain en machine learning-modell och 20% tootest hello modellen.</span><span class="sxs-lookup"><span data-stu-id="899ce-138">We will split hello data 80-20: 80% tootrain a machine learning model and 20% tootest hello model.</span></span> <span data-ttu-id="899ce-139">Vi använder hello ”Two-Class” algoritmer för detta binära klassificeringsproblem.</span><span class="sxs-lookup"><span data-stu-id="899ce-139">We will make use of hello “Two-Class” algorithms for this binary classification problem.</span></span>

1. <span data-ttu-id="899ce-140">Dra hello **dela** modulen hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="899ce-140">Drag hello **Split** module into hello canvas.</span></span>
2. <span data-ttu-id="899ce-141">Ange 0,8 för andel av rader i hello första utdatamängden i hello egenskapsrutan.</span><span class="sxs-lookup"><span data-stu-id="899ce-141">Enter 0.8 for Fraction of rows in hello first output dataset in hello Properties pane.</span></span>
   <span data-ttu-id="899ce-142">![Dela data till uppsättningar för träning och testning][6]</span><span class="sxs-lookup"><span data-stu-id="899ce-142">![Split data into training and test set][6]</span></span>
3. <span data-ttu-id="899ce-143">Dra hello **två-Tvåklassförhöjt beslutsträd** modulen hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="899ce-143">Drag hello **Two-Class Boosted Decision Tree** module into hello canvas.</span></span>
4. <span data-ttu-id="899ce-144">Dra hello **Träningsmodell** modulen till hello arbetsytan och ange hello indata.</span><span class="sxs-lookup"><span data-stu-id="899ce-144">Drag hello **Train Model** module into hello canvas and specify hello inputs.</span></span> <span data-ttu-id="899ce-145">Klicka på **starta kolumnväljaren** hello egenskaper i fönstret.</span><span class="sxs-lookup"><span data-stu-id="899ce-145">Then, click **Launch column selector** in hello Properties pane.</span></span>
   * <span data-ttu-id="899ce-146">Första indata: ML-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="899ce-146">First input: ML algorithm.</span></span>
   * <span data-ttu-id="899ce-147">Andra indata: Data tootrain hello algoritmen på.</span><span class="sxs-lookup"><span data-stu-id="899ce-147">Second input: Data tootrain hello algorithm on.</span></span>
     <span data-ttu-id="899ce-148">![Ansluta hello träningsmodellmodulen][7]</span><span class="sxs-lookup"><span data-stu-id="899ce-148">![Connect hello Train Model module][7]</span></span>
5. <span data-ttu-id="899ce-149">Välj hello **BikeBuyer** enligt hello kolumnen toopredict.</span><span class="sxs-lookup"><span data-stu-id="899ce-149">Select hello **BikeBuyer** column as hello column toopredict.</span></span>
   <span data-ttu-id="899ce-150">![Välj kolumnen toopredict][8]</span><span class="sxs-lookup"><span data-stu-id="899ce-150">![Select Column toopredict][8]</span></span>

## <a name="4-score-hello-model"></a><span data-ttu-id="899ce-151">4. Hej poängmodell</span><span class="sxs-lookup"><span data-stu-id="899ce-151">4. Score hello model</span></span>
<span data-ttu-id="899ce-152">Vi kommer nu att testa hur hello modellen presterar på testdata.</span><span class="sxs-lookup"><span data-stu-id="899ce-152">Now, we will test how hello model performs on test data.</span></span> <span data-ttu-id="899ce-153">Vi kommer att jämföra hello algoritmen vi valt med en annan algoritm toosee som presterar bäst.</span><span class="sxs-lookup"><span data-stu-id="899ce-153">We will compare hello algorithm of our choice with a different algorithm toosee which performs better.</span></span>

1. <span data-ttu-id="899ce-154">Dra **Poängmodell** modulen hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="899ce-154">Drag **Score Model** module into hello canvas.</span></span>
    <span data-ttu-id="899ce-155">Första indata: tränats modell, andra indata: testdata ![hello poängmodell][9]</span><span class="sxs-lookup"><span data-stu-id="899ce-155">First input: Trained model Second input: Test data ![Score hello model][9]</span></span>
2. <span data-ttu-id="899ce-156">Dra hello **Two-Class Bayes Point-dator** till hello experimentet.</span><span class="sxs-lookup"><span data-stu-id="899ce-156">Drag hello **Two-Class Bayes Point Machine** into hello experiment canvas.</span></span> <span data-ttu-id="899ce-157">Vi kommer att jämföra hur den här algoritmen presterar i jämförelse toohello två-Tvåklassförhöjda beslutsträdet.</span><span class="sxs-lookup"><span data-stu-id="899ce-157">We will compare how this algorithm performs in comparison toohello Two-Class Boosted Decision Tree.</span></span>
3. <span data-ttu-id="899ce-158">Kopiera och klistra in hello modulerna Träningsmodell och Poängmodell i hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="899ce-158">Copy and Paste hello modules Train Model and Score Model in hello canvas.</span></span>
4. <span data-ttu-id="899ce-159">Dra hello **utvärdera modell** modulen till hello arbetsytan toocompare hello två algoritmer.</span><span class="sxs-lookup"><span data-stu-id="899ce-159">Drag hello **Evaluate Model** module into hello canvas toocompare hello two algorithms.</span></span>
5. <span data-ttu-id="899ce-160">**Kör** hello experiment.</span><span class="sxs-lookup"><span data-stu-id="899ce-160">**Run** hello experiment.</span></span>
   <span data-ttu-id="899ce-161">![Kör hello experiment][10]</span><span class="sxs-lookup"><span data-stu-id="899ce-161">![Run hello experiment][10]</span></span>
6. <span data-ttu-id="899ce-162">Hello utdataporten längst ned hello hello utvärdera modell och klicka på visualisera.</span><span class="sxs-lookup"><span data-stu-id="899ce-162">Click hello output port at hello bottom of hello Evaluate Model module and click Visualize.</span></span>
   <span data-ttu-id="899ce-163">![Visualisera utvärderingsresultaten][11]</span><span class="sxs-lookup"><span data-stu-id="899ce-163">![Visualize evaluation results][11]</span></span>

<span data-ttu-id="899ce-164">hello mätvärdena är hello ROC-kurvan, precision återkallning diagram och lyfta kurvan.</span><span class="sxs-lookup"><span data-stu-id="899ce-164">hello metrics provided are hello ROC curve, precision-recall diagram and lift curve.</span></span> <span data-ttu-id="899ce-165">Titta på de här måtten Se vi att hello första modellen presterade bättre än hello andra.</span><span class="sxs-lookup"><span data-stu-id="899ce-165">Looking at these metrics, we can see that hello first model performed better than hello second one.</span></span> <span data-ttu-id="899ce-166">toolook på hello vad hello första modellen förutsade, klicka på utdataporten för hello Poängmodell och klicka på visualisera.</span><span class="sxs-lookup"><span data-stu-id="899ce-166">toolook at hello what hello first model predicted, click on output port of hello Score Model and click Visualize.</span></span>
<span data-ttu-id="899ce-167">![Visualisera poängresultat][12]</span><span class="sxs-lookup"><span data-stu-id="899ce-167">![Visualize score results][12]</span></span>

<span data-ttu-id="899ce-168">Du ser två kolumner som lagts till tooyour testdata.</span><span class="sxs-lookup"><span data-stu-id="899ce-168">You will see two more columns added tooyour test dataset.</span></span>

* <span data-ttu-id="899ce-169">Poängsatt sannolikhet: hello sannolikheten att en kund är en cykelköpare.</span><span class="sxs-lookup"><span data-stu-id="899ce-169">Scored Probabilities: hello likelihood that a customer is a bike buyer.</span></span>
* <span data-ttu-id="899ce-170">Poängsatta etiketter: hello klassificering utförd av hello modellen – cykelköpare (1) eller inte (0).</span><span class="sxs-lookup"><span data-stu-id="899ce-170">Scored Labels: hello classification done by hello model – bike buyer (1) or not (0).</span></span> <span data-ttu-id="899ce-171">Det här sannolikhetströskelvärdet för etikettering anges too50% och kan justeras.</span><span class="sxs-lookup"><span data-stu-id="899ce-171">This probability threshold for labeling is set too50% and can be adjusted.</span></span>

<span data-ttu-id="899ce-172">Genom att jämföra hello kolumnen BikeBuyer (faktiska) med hello poängsatta etiketter (förutsagda), kan du se hur väl hello modellen har utförts.</span><span class="sxs-lookup"><span data-stu-id="899ce-172">Comparing hello column BikeBuyer (actual) with hello Scored Labels (prediction), you can see how well hello model has performed.</span></span> <span data-ttu-id="899ce-173">Som nästa steg kan du använda den här modellen toomake förutsägelser för nya kunder och publicera den här modellen som en webbtjänst eller skriva resultaten tillbaka tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="899ce-173">As next steps, you can use this model toomake predictions for new customers and publish this model as a web service or write results back tooSQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="899ce-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="899ce-174">Next steps</span></span>
<span data-ttu-id="899ce-175">toolearn mer information om hur du skapar förutsägbara maskininlärningsmodeller, referera för[introduktion tooMachine Learning på Azure][Introduction tooMachine Learning on Azure].</span><span class="sxs-lookup"><span data-stu-id="899ce-175">toolearn more about building predictive machine learning models, refer too[Introduction tooMachine Learning on Azure][Introduction tooMachine Learning on Azure].</span></span>

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
[Introduction tooMachine Learning on Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
