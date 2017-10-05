---
title: "Med hjälp av Import/Export av Data i Azure Machine Learning-webbtjänster | Microsoft Docs"
description: "Lär dig hur du använder modulerna importera Data och exportera Data för att skicka och ta emot data från en webbtjänst."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: 3a7ac351-ebd3-43a1-8c5d-18223903d08e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 123c8c2b1c5bae268b2a61c185743f2c3920175e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a><span data-ttu-id="b53a3-103">Distribuera Azure ML-webbtjänster som använder moduler för dataimport och dataexport</span><span class="sxs-lookup"><span data-stu-id="b53a3-103">Deploying Azure ML web services that use Data Import and Data Export modules</span></span>

<span data-ttu-id="b53a3-104">När du skapar en prediktivt experiment kan du vanligtvis lägga till en webbtjänst och utgående.</span><span class="sxs-lookup"><span data-stu-id="b53a3-104">When you create a predictive experiment, you typically add a web service input and output.</span></span> <span data-ttu-id="b53a3-105">När du distribuerar experimentet kan konsumenterna skicka och ta emot data från webbtjänsten via in- och utdataenheter.</span><span class="sxs-lookup"><span data-stu-id="b53a3-105">When you deploy the experiment, consumers can send and receive data from the web service through the inputs and outputs.</span></span> <span data-ttu-id="b53a3-106">För vissa program, kan en konsument vara tillgänglig från en datafeed eller redan finns i en extern datakälla, till exempel Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="b53a3-106">For some applications, a consumer's data may be available from a data feed or already reside in an external data source such as Azure Blob storage.</span></span> <span data-ttu-id="b53a3-107">I dessa fall kan behöver de inte läsa och skriva data med hjälp av web service in- och utdataenheter.</span><span class="sxs-lookup"><span data-stu-id="b53a3-107">In these cases, they do not need read and write data using web service inputs and outputs.</span></span> <span data-ttu-id="b53a3-108">De kan i stället använda Batch Execution Service (BES) för att läsa data från datakällan modulen importera Data och skriva bedömningsprofil resultaten till en annan plats med hjälp av en modul exportera Data.</span><span class="sxs-lookup"><span data-stu-id="b53a3-108">They can, instead, use the Batch Execution Service (BES) to read data from the data source using an Import Data module and write the scoring results to a different data location using an Export Data module.</span></span>

<span data-ttu-id="b53a3-109">Importera Data och exportera data moduler kan läsa från och skriva till olika uppgifter platser, till exempel en URL via HTTP, en Hive-fråga, en Azure SQL-databas, Azure Table storage, Azure Blob storage Data Feed tillhandahåller eller en lokal SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b53a3-109">The Import Data and Export data modules, can read from and write to various data locations such as a Web URL via HTTP, a Hive Query, an Azure SQL database, Azure Table storage, Azure Blob storage, a Data Feed provide, or an on-premises SQL database.</span></span>

<span data-ttu-id="b53a3-110">Det här avsnittet använder den ”exempel 5: tåg och Test, utvärdera för binär klassificering: vuxna Dataset” exempel och förutsätter datamängden har redan lästs in i en Azure SQL-tabell med namnet censusdata.</span><span class="sxs-lookup"><span data-stu-id="b53a3-110">This topic uses the "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" sample and assumes the dataset has already been loaded into an Azure SQL table named censusdata.</span></span>

## <a name="create-the-training-experiment"></a><span data-ttu-id="b53a3-111">Skapa utbildning experiment</span><span class="sxs-lookup"><span data-stu-id="b53a3-111">Create the training experiment</span></span>
<span data-ttu-id="b53a3-112">När du öppnar den ”exempel 5: tåg och Test, utvärdera för binär klassificering: vuxna Dataset” exempel exempel vuxna inventering intäkter binär klassificering dataset används.</span><span class="sxs-lookup"><span data-stu-id="b53a3-112">When you open the "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" sample it uses the sample Adult Census Income Binary Classification dataset.</span></span> <span data-ttu-id="b53a3-113">Och experiment på arbetsytan kommer att likna följande bild:</span><span class="sxs-lookup"><span data-stu-id="b53a3-113">And the experiment in the canvas will look similar to the following image:</span></span>

![Inledande konfiguration av experimentet.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)

<span data-ttu-id="b53a3-115">Att läsa data från Azure SQL-tabellen:</span><span class="sxs-lookup"><span data-stu-id="b53a3-115">To read the data from the Azure SQL table:</span></span>

1. <span data-ttu-id="b53a3-116">Ta bort modulen dataset.</span><span class="sxs-lookup"><span data-stu-id="b53a3-116">Delete the dataset module.</span></span>
2. <span data-ttu-id="b53a3-117">Skriv i sökrutan komponenter import.</span><span class="sxs-lookup"><span data-stu-id="b53a3-117">In the components search box, type import.</span></span>
3. <span data-ttu-id="b53a3-118">Resultatlistan lägga till en *importera Data* modulen till experimentarbetsytan.</span><span class="sxs-lookup"><span data-stu-id="b53a3-118">From the results list, add an *Import Data* module to the experiment canvas.</span></span>
4. <span data-ttu-id="b53a3-119">Ansluta utdata från den *importera Data* modulen indata för den *Rensa Data som saknas* modul.</span><span class="sxs-lookup"><span data-stu-id="b53a3-119">Connect output of the *Import Data* module the input of the *Clean Missing Data* module.</span></span>
5. <span data-ttu-id="b53a3-120">Välj i egenskapsrutan, **Azure SQL Database** i den **datakällan** listrutan.</span><span class="sxs-lookup"><span data-stu-id="b53a3-120">In properties pane, select **Azure SQL Database** in the **Data Source** dropdown.</span></span>
6. <span data-ttu-id="b53a3-121">I den **Databasservernamnet**, **databasnamnet**, **användarnamn**, och **lösenord** ange lämplig information för din databas.</span><span class="sxs-lookup"><span data-stu-id="b53a3-121">In the **Database server name**, **Database name**, **User name**, and **Password** fields, enter the appropriate information for your database.</span></span>
7. <span data-ttu-id="b53a3-122">Ange följande fråga i fältet databasen frågan.</span><span class="sxs-lookup"><span data-stu-id="b53a3-122">In the Database query field, enter the following query.</span></span>
   
     <span data-ttu-id="b53a3-123">Välj [ålder]</span><span class="sxs-lookup"><span data-stu-id="b53a3-123">select [age],</span></span>
   
        [workclass],
        [fnlwgt],
        [education],
        [education-num],
        [marital-status],
        [occupation],
        [relationship],
        [race],
        [sex],
        [capital-gain],
        [capital-loss],
        [hours-per-week],
        [native-country],
        [income]
     <span data-ttu-id="b53a3-124">från dbo.censusdata;</span><span class="sxs-lookup"><span data-stu-id="b53a3-124">from dbo.censusdata;</span></span>
8. <span data-ttu-id="b53a3-125">Längst ned i arbetsytan för experimentet klickar du på **kör**.</span><span class="sxs-lookup"><span data-stu-id="b53a3-125">At the bottom of the experiment canvas, click **Run**.</span></span>

## <a name="create-the-predictive-experiment"></a><span data-ttu-id="b53a3-126">Skapa prediktivt experiment</span><span class="sxs-lookup"><span data-stu-id="b53a3-126">Create the predictive experiment</span></span>
<span data-ttu-id="b53a3-127">Nästa ställa in prediktivt experiment som du distribuerar webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="b53a3-127">Next you set up the predictive experiment from which you deploy your web service.</span></span>

1. <span data-ttu-id="b53a3-128">Längst ned i arbetsytan för experimentet klickar du på **konfigurera Web Service** och välj **förutsägande webbtjänst (rekommenderas)**.</span><span class="sxs-lookup"><span data-stu-id="b53a3-128">At the bottom of the experiment canvas, click **Set Up Web Service** and select **Predictive Web Service [Recommended]**.</span></span>
2. <span data-ttu-id="b53a3-129">Ta bort den *webbtjänst* och *Web Service utdata moduler* från prediktivt experiment.</span><span class="sxs-lookup"><span data-stu-id="b53a3-129">Remove the *Web Service Input* and *Web Service Output modules* from the predictive experiment.</span></span> 
3. <span data-ttu-id="b53a3-130">Skriv i sökrutan komponenter export.</span><span class="sxs-lookup"><span data-stu-id="b53a3-130">In the components search box, type export.</span></span>
4. <span data-ttu-id="b53a3-131">Resultatlistan lägga till en *exportera Data* modulen till experimentarbetsytan.</span><span class="sxs-lookup"><span data-stu-id="b53a3-131">From the results list, add an *Export Data* module to the experiment canvas.</span></span>
5. <span data-ttu-id="b53a3-132">Ansluta utdata från den *Poängmodell* modulen indata för den *exportera Data* modul.</span><span class="sxs-lookup"><span data-stu-id="b53a3-132">Connect output of the *Score Model* module the input of the *Export Data* module.</span></span> 
6. <span data-ttu-id="b53a3-133">Välj i egenskapsrutan, **Azure SQL Database** i den nedrullningsbara listrutan mål data.</span><span class="sxs-lookup"><span data-stu-id="b53a3-133">In properties pane, select **Azure SQL Database** in the data destination dropdown.</span></span>
7. <span data-ttu-id="b53a3-134">I den **Databasservernamnet**, **databasnamnet**, **Server användarkontonamnet**, och **serverlösenord** anger den information om din databas.</span><span class="sxs-lookup"><span data-stu-id="b53a3-134">In the **Database server name**, **Database name**, **Server user account name**, and **Server user account password** fields, enter the appropriate information for your database.</span></span>
8. <span data-ttu-id="b53a3-135">I den **kommaavgränsad lista med kolumner som ska sparas** anger poängsatta etiketter.</span><span class="sxs-lookup"><span data-stu-id="b53a3-135">In the **Comma separated list of columns to be saved** field, type Scored Labels.</span></span>
9. <span data-ttu-id="b53a3-136">I den **Data tabell namnfältet**, Skriv dbo. ScoredLabels.</span><span class="sxs-lookup"><span data-stu-id="b53a3-136">In the **Data table name field**, type dbo.ScoredLabels.</span></span> <span data-ttu-id="b53a3-137">Om tabellen inte finns, skapas den när experimentet körs eller webbtjänsten anropas.</span><span class="sxs-lookup"><span data-stu-id="b53a3-137">If the table does not exist, it is created when the experiment is run or the web service is called.</span></span>
10. <span data-ttu-id="b53a3-138">I den **kommaavgränsad lista med kolumner som datatable** anger ScoredLabels.</span><span class="sxs-lookup"><span data-stu-id="b53a3-138">In the **Comma separated list of datatable columns** field, type ScoredLabels.</span></span>

<span data-ttu-id="b53a3-139">När du skriver ett program som anropar slutliga webbtjänsten kanske du vill ange en annan indatafrågan eller måltabellen vid körning.</span><span class="sxs-lookup"><span data-stu-id="b53a3-139">When you write an application that calls the final web service, you may want to specify a different input query or destination table at run time.</span></span> <span data-ttu-id="b53a3-140">För att konfigurera dessa indata och utdata, använder du funktionen Webbtjänstparametrar för att ange den *importera Data* modulen *datakällan* egenskapen och *exportera Data* läge data mål-egenskap.</span><span class="sxs-lookup"><span data-stu-id="b53a3-140">To configure these inputs and outputs, use the Web Service Parameters feature to set the *Import Data* module *Data source* property and the *Export Data* mode data destination property.</span></span>  <span data-ttu-id="b53a3-141">Mer information om Webbtjänstparametrar finns i [AzureML Webbtjänstparametrar post](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) på Cortana Intelligence och Machine Learning-bloggen.</span><span class="sxs-lookup"><span data-stu-id="b53a3-141">For more information on Web Service Parameters, see the [AzureML Web Service Parameters entry](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) on the Cortana Intelligence and Machine Learning Blog.</span></span>

<span data-ttu-id="b53a3-142">Konfigurera Webbtjänstparametrar för import-fråga och måltabellen:</span><span class="sxs-lookup"><span data-stu-id="b53a3-142">To configure the Web Service Parameters for the import query and the destination table:</span></span>

1. <span data-ttu-id="b53a3-143">I egenskapsfönstret för den *importera Data* modulen, klickar du på ikonen överst i den **databasfrågan** fältet och välj **anges som parameter för web service**.</span><span class="sxs-lookup"><span data-stu-id="b53a3-143">In the properties pane for the *Import Data* module, click the icon at the top right of the **Database query** field and select **Set as web service parameter**.</span></span>
2. <span data-ttu-id="b53a3-144">I egenskapsfönstret för den *exportera Data* modulen, klickar du på ikonen överst i den **Data tabellnamn** fältet och välj **anges som parameter för web service**.</span><span class="sxs-lookup"><span data-stu-id="b53a3-144">In the properties pane for the *Export Data* module, click the icon at the top right of the **Data table name** field and select **Set as web service parameter**.</span></span>
3. <span data-ttu-id="b53a3-145">Längst ned i den *exportera Data* egenskapsrutan modulen i den **Webbtjänstparametrar** , på databasfrågan och byta namn på den frågan.</span><span class="sxs-lookup"><span data-stu-id="b53a3-145">At the bottom of the *Export Data* module properties pane, in the **Web Service Parameters** section, click Database query and rename it Query.</span></span>
4. <span data-ttu-id="b53a3-146">Klicka på **Data tabellnamn** och Byt namn på den **tabellen**.</span><span class="sxs-lookup"><span data-stu-id="b53a3-146">Click **Data table name** and rename it **Table**.</span></span>

<span data-ttu-id="b53a3-147">När du är klar bör experimentet likna följande bild:</span><span class="sxs-lookup"><span data-stu-id="b53a3-147">When you are done, your experiment should look similar to the following image:</span></span>

![Sista utseendet på experimentet.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

<span data-ttu-id="b53a3-149">Du kan nu distribuera experimentet som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="b53a3-149">Now you can deploy the experiment as a web service.</span></span>

## <a name="deploy-the-web-service"></a><span data-ttu-id="b53a3-150">Distribuera webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="b53a3-150">Deploy the web service</span></span>
<span data-ttu-id="b53a3-151">Du kan distribuera till en klassisk eller en ny webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="b53a3-151">You can deploy to either a Classic or New web service.</span></span>

### <a name="deploy-a-classic-web-service"></a><span data-ttu-id="b53a3-152">Distribuera en klassiska webbtjänst</span><span class="sxs-lookup"><span data-stu-id="b53a3-152">Deploy a Classic Web Service</span></span>
<span data-ttu-id="b53a3-153">Att distribuera som en klassiska webbtjänst och skapa ett program att använda den:</span><span class="sxs-lookup"><span data-stu-id="b53a3-153">To deploy as a Classic Web Service and create an application to consume it:</span></span>

1. <span data-ttu-id="b53a3-154">Klicka på Kör längst ned på arbetsytan för experimentet.</span><span class="sxs-lookup"><span data-stu-id="b53a3-154">At the bottom of the experiment canvas, click Run.</span></span>
2. <span data-ttu-id="b53a3-155">När körningen har slutförts klickar du på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [klassisk]**.</span><span class="sxs-lookup"><span data-stu-id="b53a3-155">When the run has completed, click **Deploy Web Service** and select **Deploy Web Service [Classic]**.</span></span>
3. <span data-ttu-id="b53a3-156">Leta upp din API-nyckel på infopanelen web service.</span><span class="sxs-lookup"><span data-stu-id="b53a3-156">On the web service dashboard, locate your API key.</span></span> <span data-ttu-id="b53a3-157">Kopiera och spara den för senare användning.</span><span class="sxs-lookup"><span data-stu-id="b53a3-157">Copy and save it to use later.</span></span>
4. <span data-ttu-id="b53a3-158">I den **standard Endpoint** tabell genom att klicka på den **Batch Execution** länk för att öppna sidan API.</span><span class="sxs-lookup"><span data-stu-id="b53a3-158">In the **Default Endpoint** table, click the **Batch Execution** link to open the API Help Page.</span></span>
5. <span data-ttu-id="b53a3-159">Skapa ett C#-konsolprogram i Visual Studio: **ny** > **projekt** > **Visual C#** > **Windows Klassiska Desktop** > **konsolen App (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="b53a3-159">In Visual Studio, create a C# console application: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
6. <span data-ttu-id="b53a3-160">På sidan API finns i **exempelkod** avsnittet längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="b53a3-160">On the API Help Page, find the **Sample Code** section at the bottom of the page.</span></span>
7. <span data-ttu-id="b53a3-161">Kopiera och klistra in exempelkoden C# i Program.cs-filen och ta bort alla referenser till blob storage.</span><span class="sxs-lookup"><span data-stu-id="b53a3-161">Copy and paste the C# sample code into your Program.cs file, and remove all references to the blob storage.</span></span>
8. <span data-ttu-id="b53a3-162">Uppdatera värdet för den *apiKey* variabeln med API-nyckeln som sparats tidigare.</span><span class="sxs-lookup"><span data-stu-id="b53a3-162">Update the value of the *apiKey* variable with the API key saved earlier.</span></span>
9. <span data-ttu-id="b53a3-163">Leta reda på begäran-deklarationen och uppdatera värdena för Webbtjänstparametrar som skickas till den *importera Data* och *exportera Data* moduler.</span><span class="sxs-lookup"><span data-stu-id="b53a3-163">Locate the request declaration and update the values of Web Service Parameters that are passed to the *Import Data* and *Export Data* modules.</span></span> <span data-ttu-id="b53a3-164">I så fall måste du använda den ursprungliga frågan, men definiera ett nytt tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="b53a3-164">In this case, you use the original query, but define a new table name.</span></span>
   
        var request = new BatchExecutionRequest() 
        {           
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. <span data-ttu-id="b53a3-165">Kör appen.</span><span class="sxs-lookup"><span data-stu-id="b53a3-165">Run the application.</span></span> 

<span data-ttu-id="b53a3-166">En ny tabell har lagts till den databas som innehåller bedömningsprofil resultaten på slutförande av körningen.</span><span class="sxs-lookup"><span data-stu-id="b53a3-166">On completion of the run, a new table is added to the database containing the scoring results.</span></span>

### <a name="deploy-a-new-web-service"></a><span data-ttu-id="b53a3-167">Distribuera en ny webbtjänst</span><span class="sxs-lookup"><span data-stu-id="b53a3-167">Deploy a New Web Service</span></span>

> [!NOTE] 
> <span data-ttu-id="b53a3-168">Om du vill distribuera en ny webbtjänst måste du ha tillräckliga behörigheter i prenumerationen som du distribuerar webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="b53a3-168">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="b53a3-169">Mer information finns i [hantera en webbtjänst med hjälp av Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="b53a3-169">For more information, see [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="b53a3-170">Distribuera som en ny webbtjänst och skapa ett program att använda den:</span><span class="sxs-lookup"><span data-stu-id="b53a3-170">To deploy as a New Web Service and create an application to consume it:</span></span>

1. <span data-ttu-id="b53a3-171">Längst ned i arbetsytan för experimentet klickar du på **kör**.</span><span class="sxs-lookup"><span data-stu-id="b53a3-171">At the bottom of the experiment canvas, click **Run**.</span></span>
2. <span data-ttu-id="b53a3-172">När körningen har slutförts klickar du på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="b53a3-172">When the run has completed, click **Deploy Web Service** and select **Deploy Web Service [New]**.</span></span>
3. <span data-ttu-id="b53a3-173">Ange ett namn för webbtjänsten, på sidan distribuera Experiment och välja en prisavtal, och klicka sedan **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="b53a3-173">On the Deploy Experiment page, enter a name for your web service, and select a pricing plan, then click **Deploy**.</span></span>
4. <span data-ttu-id="b53a3-174">På den **Quickstart** klickar du på **förbruka**.</span><span class="sxs-lookup"><span data-stu-id="b53a3-174">On the **Quickstart** page, click **Consume**.</span></span>
5. <span data-ttu-id="b53a3-175">I den **exempelkod** klickar du på **Batch**.</span><span class="sxs-lookup"><span data-stu-id="b53a3-175">In the **Sample Code** section, click **Batch**.</span></span>
6. <span data-ttu-id="b53a3-176">Skapa ett C#-konsolprogram i Visual Studio: **ny** > **projekt** > **Visual C#** > **Windows Klassiska Desktop** > **konsolen App (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="b53a3-176">In Visual Studio, create a C# console application: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
7. <span data-ttu-id="b53a3-177">Kopiera och klistra in exempelkoden C# i filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="b53a3-177">Copy and paste the C# sample code into your Program.cs file.</span></span>
8. <span data-ttu-id="b53a3-178">Uppdatera värdet för den *apiKey* variabeln med den **primärnyckel** finns i den **grundläggande förbrukning info** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b53a3-178">Update the value of the *apiKey* variable with the **Primary Key** located in the **Basic consumption info** section.</span></span>
9. <span data-ttu-id="b53a3-179">Leta upp den *scoreRequest* deklaration och uppdatera värdena för Webbtjänstparametrar som skickas till den *importera Data* och *exportera Data* moduler.</span><span class="sxs-lookup"><span data-stu-id="b53a3-179">Locate the *scoreRequest* declaration and update the values of Web Service Parameters that are passed to the *Import Data* and *Export Data* modules.</span></span> <span data-ttu-id="b53a3-180">I så fall måste du använda den ursprungliga frågan, men definiera ett nytt tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="b53a3-180">In this case, you use the original query, but define a new table name.</span></span>
   
        var scoreRequest = new
        {       
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };
10. <span data-ttu-id="b53a3-181">Kör appen.</span><span class="sxs-lookup"><span data-stu-id="b53a3-181">Run the application.</span></span> 

