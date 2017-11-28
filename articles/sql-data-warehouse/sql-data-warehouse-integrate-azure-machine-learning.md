---
title: aaaUse Azure Machine Learning med SQL Data Warehouse | Microsoft Docs
description: "Självstudier för att använda Azure Machine Learning med Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: barbkess
editor: 
ms.assetid: ac6bc731-6add-47a9-b3fe-68996e656f4d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: fdfe8c936d2bb7a02163a0bbf6435e1ebd518d4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a><span data-ttu-id="1b5ed-103">Använda Azure Machine Learning med SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="1b5ed-103">Use Azure Machine Learning with SQL Data Warehouse</span></span>
<span data-ttu-id="1b5ed-104">Azure Machine Learning är en helt hanterad förutsägelseanalyser tjänst att du kan använda toocreate förutsägelsemodeller mot dina data i SQL Data Warehouse och sedan publicera som redo att använda webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-104">Azure Machine Learning is a fully managed predictive analytics service that you can use toocreate predictive models against your data in SQL Data Warehouse, and then publish as ready-to-consume web services.</span></span> <span data-ttu-id="1b5ed-105">Du kan lära sig hello grunderna i förutsägelseanalyser och maskininlärning genom att läsa [introduktion tooMachine Learning på Azure][Introduction tooMachine Learning on Azure].</span><span class="sxs-lookup"><span data-stu-id="1b5ed-105">You can learn hello basics of predictive analytics and machine learning by reading [Introduction tooMachine Learning on Azure][Introduction tooMachine Learning on Azure].</span></span>  <span data-ttu-id="1b5ed-106">Du kan sedan lära dig hur toocreate, träna, poängsätta och testa en maskininlärningsmodell med hello [skapa experiment kursen][Create experiment tutorial].</span><span class="sxs-lookup"><span data-stu-id="1b5ed-106">You can then learn how toocreate, train, score and test a machine learning model using hello [Create experiment tutorial][Create experiment tutorial].</span></span>

<span data-ttu-id="1b5ed-107">I den här artikeln får du lära dig hur toodo hello följa med hello [Azure Machine Learning Studio][Azure Machine Learning Studio]:</span><span class="sxs-lookup"><span data-stu-id="1b5ed-107">In this article, you will learn how toodo hello following using hello [Azure Machine Learning Studio][Azure Machine Learning Studio]:</span></span>

* <span data-ttu-id="1b5ed-108">Läsa data från din databas toocreate, träna och betygsätta en förutsägelsemodell</span><span class="sxs-lookup"><span data-stu-id="1b5ed-108">Read data from your database toocreate, train and score a predictive model</span></span>
* <span data-ttu-id="1b5ed-109">Skriva data tooyour databas</span><span class="sxs-lookup"><span data-stu-id="1b5ed-109">Write data tooyour database</span></span>

## <a name="read-data-from-sql-data-warehouse"></a><span data-ttu-id="1b5ed-110">Läsa data från SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="1b5ed-110">Read data from SQL Data Warehouse</span></span>
<span data-ttu-id="1b5ed-111">Vi läser data från produkt-tabellen i hello AdventureWorksDW-databasen.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-111">We will read data from Product table in hello AdventureWorksDW database.</span></span>

### <a name="step-1"></a><span data-ttu-id="1b5ed-112">Steg 1</span><span class="sxs-lookup"><span data-stu-id="1b5ed-112">Step 1</span></span>
<span data-ttu-id="1b5ed-113">Starta ett nytt experiment genom att klicka på + ny längst hello hello Machine Learning Studio-fönstret Välj EXPERIMENT och välj sedan tomt Experiment.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-113">Start a new experiment by clicking +NEW at hello bottom of hello Machine Learning Studio window, select EXPERIMENT, and then select Blank Experiment.</span></span> <span data-ttu-id="1b5ed-114">Välj hello standard experimentera namn hello överst i hello arbetsytan och Byt toosomething beskrivande, till exempel cykel pris förutsägelse.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-114">Select hello default experiment name at hello top of hello canvas and rename it toosomething meaningful, for example, Bicycle price prediction.</span></span>

### <a name="step-2"></a><span data-ttu-id="1b5ed-115">Steg 2</span><span class="sxs-lookup"><span data-stu-id="1b5ed-115">Step 2</span></span>
<span data-ttu-id="1b5ed-116">Leta efter hello Reader modul i hello palett med datauppsättningar och moduler hello till vänster i hello experimentet.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-116">Look for hello Reader module in hello palette of datasets and modules on hello left of hello experiment canvas.</span></span> <span data-ttu-id="1b5ed-117">Dra hello modulen toohello för experimentet.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-117">Drag hello module toohello experiment canvas.</span></span>
![][drag_reader]

### <a name="step-3"></a><span data-ttu-id="1b5ed-118">Steg 3</span><span class="sxs-lookup"><span data-stu-id="1b5ed-118">Step 3</span></span>
<span data-ttu-id="1b5ed-119">Välj hello Reader modul och fylla i hello egenskapsrutan.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-119">Select hello Reader module and fill out hello properties pane.</span></span>

1. <span data-ttu-id="1b5ed-120">Välj Azure SQL Database som hello-datakälla.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-120">Select Azure SQL Database as hello Data Source.</span></span>
2. <span data-ttu-id="1b5ed-121">Databasservernamn: typen hello servernamn.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-121">Database server name: Type hello server name.</span></span> <span data-ttu-id="1b5ed-122">Du kan använda hello [Azure-portalen] [ Azure portal] toofind detta.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-122">You can use hello [Azure portal][Azure portal] toofind this.</span></span>

![][server_name]

1. <span data-ttu-id="1b5ed-123">Databasnamn: hello-typnamn för en databas på hello-server som du just har angett.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-123">Database name: Type hello name of a database on hello server you just specified.</span></span>
2. <span data-ttu-id="1b5ed-124">Servern användarkontonamnet: Skriv hello användarnamn för ett konto som har behörighet för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-124">Server user account name:  Type hello user name of an account that has access permissions for hello database.</span></span>
3. <span data-ttu-id="1b5ed-125">Serverlösenord: Ange hello lösenordet för hello angivna användarkontot.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-125">Server user account password: Provide hello password for hello specified user account.</span></span>
4. <span data-ttu-id="1b5ed-126">Acceptera alla servercertifikat: Använd det här alternativet (mindre säkert) om du vill tooskip granska hello platscertifikat innan du läser dina data.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-126">Accept any server certificate: Use this option (less secure) if you want tooskip reviewing hello site certificate before you read your data.</span></span>
5. <span data-ttu-id="1b5ed-127">Databasfrågan: Ange en SQL-instruktion som beskriver hello data som du vill tooread.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-127">Database query: Enter a SQL statement that describes hello data you want tooread.</span></span> <span data-ttu-id="1b5ed-128">I det här fallet kommer vi läsa data från produkt-tabellen med hjälp av hello följande fråga.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-128">In this case, we will read data from Product table using hello following query.</span></span>

```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a><span data-ttu-id="1b5ed-129">Steg 4</span><span class="sxs-lookup"><span data-stu-id="1b5ed-129">Step 4</span></span>
1. <span data-ttu-id="1b5ed-130">Kör hello experiment genom att klicka på kör under hello experimentet.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-130">Run hello experiment by clicking Run under hello experiment canvas.</span></span>
2. <span data-ttu-id="1b5ed-131">När hello experimentet har slutförts har modul för dataläsare för hello en grön bock tooindicate som har slutförts.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-131">When hello experiment finishes, hello Reader module will have a green check mark tooindicate that it has completed successfully.</span></span> <span data-ttu-id="1b5ed-132">Observera också hello avslutad Körningsstatus i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-132">Notice also hello Finished running status in hello upper-right corner.</span></span>

![][run]

1. <span data-ttu-id="1b5ed-133">toosee hello importerade data på hello utdataporten längst ned hello hello bildata och väljer visualisera.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-133">toosee hello imported data, click hello output port at hello bottom of hello automobile dataset and select Visualize.</span></span>

## <a name="create-train-and-score-a-model"></a><span data-ttu-id="1b5ed-134">Skapa, träna och betygsätta en modell</span><span class="sxs-lookup"><span data-stu-id="1b5ed-134">Create, train and score a model</span></span>
<span data-ttu-id="1b5ed-135">Nu kan du använda denna dataset för att:</span><span class="sxs-lookup"><span data-stu-id="1b5ed-135">Now you can use this dataset to:</span></span>

* <span data-ttu-id="1b5ed-136">Skapa en modell: bearbeta data och definiera funktioner</span><span class="sxs-lookup"><span data-stu-id="1b5ed-136">Create a Model: Process data and define features</span></span>
* <span data-ttu-id="1b5ed-137">Hej träningsmodell: välja och tillämpa en inlärningsalgoritm</span><span class="sxs-lookup"><span data-stu-id="1b5ed-137">Train hello model: Choose and apply a learning algorithm</span></span>
* <span data-ttu-id="1b5ed-138">Poängsätta och testa hello modell: förutsäga nya cykel pris</span><span class="sxs-lookup"><span data-stu-id="1b5ed-138">Score and test hello model: Predict new bicycle price</span></span>

![][model]

<span data-ttu-id="1b5ed-139">Mer om hur toocreate, träna, poängsätta och testa machine learning-modellen Använd hello toolearn [skapa experiment kursen][Create experiment tutorial].</span><span class="sxs-lookup"><span data-stu-id="1b5ed-139">toolearn more about how toocreate, train, score and test a machine learning model use hello [Create experiment tutorial][Create experiment tutorial].</span></span>

## <a name="write-data-tooazure-sql-data-warehouse"></a><span data-ttu-id="1b5ed-140">Skriva data tooAzure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="1b5ed-140">Write data tooAzure SQL Data Warehouse</span></span>
<span data-ttu-id="1b5ed-141">Vi skriver hello resultattabellen set tooProductPriceForecast i hello AdventureWorksDW-databasen.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-141">We will write hello result set tooProductPriceForecast table in hello AdventureWorksDW database.</span></span>

### <a name="step-1"></a><span data-ttu-id="1b5ed-142">Steg 1</span><span class="sxs-lookup"><span data-stu-id="1b5ed-142">Step 1</span></span>
<span data-ttu-id="1b5ed-143">Leta efter hello skrivarmodul i hello palett med datauppsättningar och moduler hello till vänster i hello experimentet.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-143">Look for hello Writer module in hello palette of datasets and modules on hello left of hello experiment canvas.</span></span> <span data-ttu-id="1b5ed-144">Dra hello modulen toohello för experimentet.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-144">Drag hello module toohello experiment canvas.</span></span>

![][drag_writer]

### <a name="step-2"></a><span data-ttu-id="1b5ed-145">Steg 2</span><span class="sxs-lookup"><span data-stu-id="1b5ed-145">Step 2</span></span>
<span data-ttu-id="1b5ed-146">Välj hello-skrivarmodul och fylla i hello egenskapsrutan.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-146">Select hello Writer module and fill out hello properties pane.</span></span>

1. <span data-ttu-id="1b5ed-147">Välj Azure SQL Database som hello datamål.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-147">Select Azure SQL Database as hello Data Destination.</span></span>
2. <span data-ttu-id="1b5ed-148">Databasservernamn: typen hello servernamn.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-148">Database server name: Type hello server name.</span></span> <span data-ttu-id="1b5ed-149">Du kan använda hello [Azure-portalen] [ Azure portal] toofind detta.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-149">You can use hello [Azure portal][Azure portal] toofind this.</span></span>
3. <span data-ttu-id="1b5ed-150">Databasnamn: hello-typnamn för en databas på hello-server som du just har angett.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-150">Database name: Type hello name of a database on hello server you just specified.</span></span>
4. <span data-ttu-id="1b5ed-151">Servern användarkontonamnet: typen hello användarnamnet för ett konto som har skrivbehörighet för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-151">Server user account name:  Type hello user name of an account that has write permissions for hello database.</span></span>
5. <span data-ttu-id="1b5ed-152">Serverlösenord: Ange hello lösenordet för hello angivna användarkontot.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-152">Server user account password: Provide hello password for hello specified user account.</span></span>
6. <span data-ttu-id="1b5ed-153">Acceptera alla servercertifikat (osäker): Välj det här alternativet om du inte vill tooview hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-153">Accept any server certificate (insecure): Select this option if you don’t want tooview hello certificate.</span></span>
7. <span data-ttu-id="1b5ed-154">Kommaavgränsad lista med kolumner toobe sparade: Ange en lista över hello datamängd eller resultatet kolumner som du vill toooutput.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-154">Comma-separated list of columns toobe saved: Provide a list of hello dataset or result columns that you want toooutput.</span></span>
8. <span data-ttu-id="1b5ed-155">Data tabellnamn: Ange hello datatabell hello namn.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-155">Data table name: Specify hello name of hello data table.</span></span>
9. <span data-ttu-id="1b5ed-156">Kommaavgränsad lista med kolumner som datatable: Ange hello kolumnen namn toouse i hello ny tabell.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-156">Comma-separated list of datatable columns:  Specify hello column names toouse in hello new table.</span></span> <span data-ttu-id="1b5ed-157">hello kolumnnamn kan skilja sig från hello dem i hello källa dataset, men du måste ange hello samma antal kolumner som du definierar för hello utdatatabellen.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-157">hello column names can be different from hello ones in hello source dataset, but you must list hello same number of columns here that you define for hello output table.</span></span>
10. <span data-ttu-id="1b5ed-158">Antalet rader som skrivs per SQL Azure-åtgärd: du kan konfigurera hello antalet rader som skrivs tooa SQL-databas i en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-158">Number of rows written per SQL Azure operation: You can configure hello number of rows that are written tooa SQL database in one operation.</span></span>

![][writer_properties]

### <a name="step-3"></a><span data-ttu-id="1b5ed-159">Steg 3</span><span class="sxs-lookup"><span data-stu-id="1b5ed-159">Step 3</span></span>
1. <span data-ttu-id="1b5ed-160">Kör hello experiment genom att klicka på kör under hello experimentet.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-160">Run hello experiment by clicking Run under hello experiment canvas.</span></span>
2. <span data-ttu-id="1b5ed-161">När hello experimentet har slutförts visas har alla moduler som en grön bock tooindicate som de har slutförts.</span><span class="sxs-lookup"><span data-stu-id="1b5ed-161">When hello experiment finishes, all modules will have a green check mark tooindicate that they completed successfully.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b5ed-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1b5ed-162">Next steps</span></span>
<span data-ttu-id="1b5ed-163">För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="1b5ed-163">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Create experiment tutorial]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Introduction toomachine learning on Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure Machine Learning Studio]: https://studio.azureml.net/Home
[Azure portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
